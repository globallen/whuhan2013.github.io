---
layout: post
title: AsyncTask源码解析
date: 2016-8-15
categories: blog
tags: [源码解析]
description: 源码解析
---


### 简介

AsyncTask是android提供的一种异步消息处理的解决方案，能简化我们在子线程中更新UI控件，使用AsyncTask你将看不到任何关于操作线程的代码。


### 版本差别

1、线程池配置

android3.0以前线程池配置，代码如下所示：

```
private static final int CORE_POOL_SIZE = 5;//核心线程数量
private static final int MAXIMUM_POOL_SIZE = 128;//线程池中允许的最大线程数目
private static final it KEEP_ALIVE = 10;//当线程数目大于核心线程数目时，如果超过这个keepAliveTime时间，那么空闲的线程会被终止。
……    
private static final ThreadPoolExecutor sExecutor = new ThreadPoolExecutor(CORE_POOL_SIZE,    
        MAXIMUM_POOL_SIZE, KEEP_ALIVE, TimeUnit.SECONDS, sWorkQueue, sThreadFactory);  
```

android3.0以后更加灵活，根据cpu核数配置CORE_POOL_SIZE和MAXIMUM_POOL_SIZE

```
private static final int CPU_COUNT = Runtime.getRuntime().availableProcessors();//根据cpu的大小来配置核心的线程
private static final int CORE_POOL_SIZE = CPU_COUNT + 1;//核心线程数量
private static final int MAXIMUM_POOL_SIZE = CPU_COUNT * 2 + 1;//线程池中允许的最大线程数目
private static final int KEEP_ALIVE = 1;//空闲线程的超时时间
```

2、串行和并行,引用来自这篇[文章](http://www.jianshu.com/p/a8b1861f2efc)

- android 1.5以前的时候execute是串行执行的
- android 1.6直到android 2.3.2被修改为并行执行，执行任务的线程池就是THREAD_POOL_EXECUTOR
- android 3.0以后，默认任务是串行执行的，如果想要并行执行任务可调用executeOnExecutor(Executor exec, Params.. params)。具体用法可参照[Android AsyncTask的骗术](http://glanwang.com/post/android/android-asynctaskde-pian-zhu)


### 源码分析(基于android3.0以后分析)       


**AsyncTask构造函数**      

```
 /**AsyncTask的构造函数源码片段**/
   public AsyncTask() {
        mWorker = new WorkerRunnable<Params, Result>() {
            //异步任务执行的时候回调call方法
            public Result call() throws Exception {
                mTaskInvoked.set(true);
                //设置线程的优先级
                Process.setThreadPriority(Process.THREAD_PRIORITY_BACKGROUND);
                //noinspection unchecked
                Result result = doInBackground(mParams);
                Binder.flushPendingCommands();
                //将结果发送出去
                return postResult(result);
            }
        };

        mFuture = new FutureTask<Result>(mWorker) {
            //任务执行完毕后会调用done方法
            @Override
            protected void done() {
                try {
                    //get()表示获取mWorker的call的返回值，即Result.然后看postResultIfNotInvoked方法
                    postResultIfNotInvoked(get());
                } catch (InterruptedException e) {
                    android.util.Log.w(LOG_TAG, e);
                } catch (ExecutionException e) {
                    throw new RuntimeException("An error occurred while executing doInBackground()",
                            e.getCause());
                } catch (CancellationException e) {
                    postResultIfNotInvoked(null);
                }
            }
        };
    }
```

AsyncTask的实例化是在UI线程中。

```
  //@MainThread表示这个动作是在UI线程中完成
  @MainThread
    public final AsyncTask<Params, Progress, Result> execute(Params... params) {
        return executeOnExecutor(sDefaultExecutor, params);
    }
```

**WorkerRunnable是一个实现了Callable的抽象类,扩展了Callable多了一个Params参数**

```
private static abstract class WorkerRunnable<Params, Result> implements Callable<Result> 
{
        Params[] mParams;
}//java
```

构造函数初始化了两个成员变量mWorker和mFuture。mWorker为WorkerRunnable类型的匿名内部类实例对象（实现了Callable接口），mFuture为FutureTask类型的匿名内部类实例对象，将mWorker作为mFuture的形参（重写了FutureTask类的done方法）。

下面讲述下Callable和Runnable的区别。

- Callable的接口方法是call，Runnable是run
- Callable可以带返回值，Runnable不行,结果通过Future.get()获取
- Callable可以捕获异常，Runnable不行


```
public class CallableAndFuture {
    public static void main(String[] args) {
        Callable<Integer> callable = new Callable<Integer>() {
            public Integer call() throws Exception {
                return new Random().nextInt(100);
            }
        };
        //那WorkerRunnable的回调方法call肯定是在FutureTask中调用的
        FutureTask<Integer> future = new FutureTask<Integer>(callable)
      );
        new Thread(future).start();
        try {
            Thread.sleep(5000);// 可能做一些事情
            System.out.println(future.get());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}//java
```

FutureTask的构造函数如下，

```  
        public FutureTask(Callable<V> callable) {
        if (callable == null)
            throw new NullPointerException();
        //将AsyncTask里面初始化的callable赋值给FutureTask里面的callable，证实了WorkerRunnable的回调方法call肯定是在FutureTask中调用的
        this.callable = callable;
        this.state = NEW;       // ensure visibility of callable
    }
```

查看FutureTask类，它实现了接口Runnable

public class FutureTask<V> implements RunnableFuture<V>//java
实现了RunnableFuture接口


作为Runnable被线程执行，同时将Callable作为构造函数的参数传入，这样组合的好处是，假设有一个很耗时的返回值需要计算，并且这个返回值不是立刻需要的话，就可以使用这个组合，用另一个线程去计算返回值，而当前线程在使用这个返回值之前可以做其它的操作，等到需要这个返回值时，再通过Future得到。FutureTask的run方法要开始回调WorkerRunable的call方法了，call里面调用doInBackground(mParams),终于回到我们后台任务了，调用我们AsyncTask子类的doInBackground(),由此可以看出doInBackground()是在子线程中执行的，如下图所示 


![](https://github.com/white37/AndroidSdkSourceAnalysis/raw/master/images/FutureTask(run).png)

#### 核心方法

**execute()方法**

```
private static volatile Executor sDefaultExecutor = SERIAL_EXECUTOR;
public static final Executor SERIAL_EXECUTOR = new SerialExecutor();

 /** AsyncTask类的execute方法**/
 public final AsyncTask<Params, Progress, Result> execute(Params... params) {
    //调用executeOnExecutor方法
    return executeOnExecutor(sDefaultExecutor, params);
}
```

当执行execute方法时，实际上是调用了executeOnExecutor方法。这里传递了两个参数，一个是sDefaultExecutor，一个是params。从上面的源码可以看出，sDefaultExecutor其实是一个SerialExecutor对象，实现了串行线程队列。params其实最终会赋给doInBackground方法去处理。         


**executeOnExecutor()方法**

```
//exec执行AsyncTask.execute()方法时传递进来的参数sDefaultExecutor，这个sDefaultExecutor其实就是SerialExecutor对象。默认是串行执行的
//若想变成并发执行exec可以传入AsyncTask.THREAD_POOL_EXECUTOR。
public final AsyncTask<Params, Progress, Result> executeOnExecutor(Executor exec,
            Params... params) {
        //如果一个任务已经进入执行的状态，再执行就会抛异常。这就决定了一个AsyncTask只能执行一次
        if (mStatus != Status.PENDING) {
            switch (mStatus) {
                case RUNNING:
                    throw new IllegalStateException("Cannot execute task:"
                            + " the task is already running.");
                case FINISHED:
                    throw new IllegalStateException("Cannot execute task:"
                            + " the task has already been executed "
                            + "(a task can be executed only once)");
            }
        }
    //一旦executeOnExecutor调用了就标记为运行状态
        mStatus = Status.RUNNING;
    //实际是调用子类里面的onPreExecute
        onPreExecute();
    //将处理的参数类型赋值给mWorker
        mWorker.mParams = params;
        //execute是调用SERIAL_EXECUTOR的execute，mFuture就是之前AsyncTask构造初始化赋值的FutureTask。
        exec.execute(mFuture);
        return this;
    }
```

这里要说明一下，AsyncTask的异步任务有三种状态

- PENDING 待执行状态。当AsyncTask被创建时，就进入了PENDING状态。
- RUNNING 运行状态。当调用executeOnExecutor，就进入了RUNNING状态。
- FINISHED 结束状态。当AsyncTask完成(用户cancel()或任务执行完毕)时，就进入了FINISHED状态。


**SerialExecutor的execute方法**      

```
private static class SerialExecutor implements Executor {
    //循环数组实现的双向Queue。大小是2的倍数，默认是16。有队头队尾两个下标
        final ArrayDeque<Runnable> mTasks = new ArrayDeque<Runnable>();
        //当前正在运行的runnable
        Runnable mActive;

        public synchronized void execute(final Runnable r) {
            //添加到双端队列里面去
            mTasks.offer(new Runnable() {
                public void run() {
                    try {
                        //执行的是mFuture就是之前AsyncTask构造初始化赋值的FutureTask的run()方法
                        r.run();
                    } finally {
                        //无论执行结果如何都会取出下一个任务执行
                        scheduleNext();
                    }
                }
            });
            //如果没有活动的runnable，从双端队列里面取出一个runnable放到线程池中运行
            //第一个请求任务过来的时候mActive是空的
            if (mActive == null) {
                //取出下一个任务来执行
                scheduleNext();
            }
        }

        protected synchronized void scheduleNext() {
            //从双端队列中取出一个任务
            if ((mActive = mTasks.poll()) != null) {
                //线程池执行取出来的任务，真正执行任务的
                THREAD_POOL_EXECUTOR.execute(mActive);
            }
        }
    }//java
```


exec.execute(mFuture)执行时，SerialExecutor将FutureTask作为参数执行execute方法。在execute方法中，假设FutureTask插入进了两个以上的任务队列到mTasks中，第一次过来mActive==null，通过mTasks.poll()取出一个任务丢给线程池运行，线程池执行r.run，其实就是执行FutureTask的run方法，因为传递进来的r参数就是mFuture。等到上一个线程执完r.run()完之后，这里是通过一个try-finally代码块，并在finally中调用了scheduleNext()方法，保证无论发生什么情况，scheduleNext()都会取出下一个任务执行。接着因为mActive不为空了，不会再执行`scheduleNext()，由于存在一个循环队列，每个 Runnable 被执行的时候，都进入去队列，然后在执行完后出队，才会进入下一个 Runnable 的执行流程。由此可知道这是一个串行的执行过程，同一时刻只会有一个线程正在执行，其余的均处于等待状态。


```
mWorker = new WorkerRunnable<Params, Result>() {
            public Result call() throws Exception {
                mTaskInvoked.set(true);

                Process.setThreadPriority(Process.THREAD_PRIORITY_BACKGROUND);
                //noinspection unchecked
                Result result = doInBackground(mParams);//调用子类的doInBackground
                Binder.flushPendingCommands();
                return postResult(result);//执行完后通过postResult结果传递出去
            }
        };
```


上文中提到调用call()的流程：SerialExecutor.execute() -> FutureTask.run() -> WorkerRunnable.call() 如果回调了call()方法，就会调用了doInBackground(mParams)方法，这都是在子线程中执行的。执行完后，将结果通过postResult(result)发送出去。



**AsyncTask的postResult方法**           

```
 private Result postResult(Result result) {
        @SuppressWarnings("unchecked")
        //获取一个handler，等到一个消息，将结果封装在Message
        Message message = getHandler().obtainMessage(MESSAGE_POST_RESULT,
                new AsyncTaskResult<Result>(this, result));//new AsyncTaskResult<Result>(this, result)将得到的结果再做了一层封装
        //将消息发送到主线程，会回调handleMessage()方法
        message.sendToTarget();
        return result;
    }
```


因为postResult(Result result)还是在子线程中调用的，如果要发送给主线程，必须通过Handler。源码中使用sHandler并带着MESSAGE_POST_RESULT和封装了任务执行结果的对象AsyncTaskResult，然后message.sendToTarget()开始发消息。


```
private static Handler getHandler() {
        synchronized (AsyncTask.class) {
            if (sHandler == null) {
                //初始化一个InternalHandler，用与将结果发送给主线程
                sHandler = new InternalHandler();
            }
            return sHandler;
        }
    }
```

并在InternalHandler的handleMessage中开始处理消息，InternalHandler的源码如下所示：

```
 private static class InternalHandler extends Handler {
        public InternalHandler() {
            // 这个handler是关联到主线程的
            super(Looper.getMainLooper());
        }

        @SuppressWarnings({"unchecked", "RawUseOfParameterizedType"})
        @Override
        public void handleMessage(Message msg) {
            AsyncTaskResult<?> result = (AsyncTaskResult<?>) msg.obj;
            switch (msg.what) {
                case MESSAGE_POST_RESULT:
                    // There is only one result
                    result.mTask.finish(result.mData[0]);
                    break;
                case MESSAGE_POST_PROGRESS:
                    result.mTask.onProgressUpdate(result.mData);
                    break;
            }
        }
    }
```

这里根据消息的类型进行了判断，如果是MESSAGE_POST_RESULT消息，就会去执行finish()方法，finish()源码如下文所示：


```
private void finish(Result result) {  
    if (isCancelled()) {  
        onCancelled(result);  
    } else {  
        onPostExecute(result);  
    }  
    mStatus = Status.FINISHED;  
} 
```

如果任务已经取消了，调用onCancelled方法，如果没被取消，则调用onPostExecute()方法。

如果doInBackground(Void... params)调用publishProgress()方法，实际就是发送一条MESSAGE_POST_PROGRESS消息，就会去执行onProgressUpdate()方法。publishProgress()的源码如下文所示：

```
    @WorkerThread
    protected final void publishProgress(Progress... values) {
        if (!isCancelled()) {
            getHandler().obtainMessage(MESSAGE_POST_PROGRESS,
                    new AsyncTaskResult<Progress>(this, values)).sendToTarget();
        }
    }
```

如果你还不够清晰，请看下面的这个流程图。 


![](https://github.com/white37/AndroidSdkSourceAnalysis/raw/master/images/AsyncTask%E6%B5%81%E7%A8%8B%E5%9B%BE.png)


### AsyncTask需要注意的坑

- AsyncTask的对象必须在主线程中实例化，execute方法也要在主线程调用(查看3.1节-AsyncTask构造函数)
- 同一个AsyncTask任务只能被执行一次，即只能调用一次execute方法，多次调用时将会抛异常（查看3.2里面的第二小节）
- cancel()方法无法直接中断子线程，只是更改了中断的标志位。控制异步任务执行结束后不会回调onPostExecute()。正确的取消异步任务要cancel()方法+doInbacground()做判断跳出循环
- AsyncTask在Activit通常作为匿名的内部类来使用，如果 AsyncTask 中的异步任务在 Activity 退出时还没执行完或者阻塞了，那么这个保持的外部的 Activity 实例得不到释放（内部类保持隐式外部类的实例的引用），最后导致会引起OOM，解决办法是：在 AsyncTask 使用弱引用外部实例，或者保证在 Activity 退出时，所有的 AsyncTask 已执行完成或被取消
- 会产生阻塞问题，尤其是单任务顺序执行的情况下，一个任务执行时间过长会阻塞其他任务的执行
- 不建议使用AsyncTask进行网络操作                  
AsyncTasks should ideally be used for short operations (a few seconds at the most.) If you need to keep threads running for long periods of time, it is highly recommended you use the various APIs。
Android文档中有写到AsyncTask应该处理几秒钟的操作（通常为轻量的本地IO操作），由于网络操作存在不确定性，可能达到几秒以上，所以不建议使用。


### 总结

尽管AsyncTask现在已经很少使用了，但是它的一些设计思路可以借鉴到我们的框架中。比如我们的代码中尽量设计灵活一些，就像AysnTask里面存在串行、并行的操作一样，提供用户同的api，让用户在不同的场景下选择不同的业务逻辑处理。