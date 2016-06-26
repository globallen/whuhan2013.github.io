---
layout: post
title: React Native实例
date: 2016-6-24
categories: blog
tags: [React Native]
description: React Native实例
---


**本文主要包括以下内容** 

1. View组件的实例            
2. Text组件实例         
3. Navigator组件实例    
4. TextInput组件实例

### View组件的实例

**效果如下** 

![这里写图片描述](http://img.blog.csdn.net/20160624155046443)


**代码如下** 

```
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
    PixelRatio,
  View
} from 'react-native';

class Abc extends Component {
  render() {
    return (
     <View style={styles.flex}>
      <View style={styles.container}>
        <View style={[styles.item,styles.center]}>

          <Text style={styles.font}>酒店</Text>

        </View>

        <View style={[styles.item,styles.lineLeftRight]}>

          <View style={[styles.center,styles.flex,styles.lineCenter]}>
            <Text style={styles.font}>海外酒店</Text>
          </View>

          <View style={[styles.center,styles.flex]}>
            <Text style={styles.font}>特惠酒店</Text>

          </View>



        </View>



        <View style={styles.item}>

          <View style={[styles.center,styles.flex,styles.lineCenter]}>
            <Text style={styles.font}>团购</Text>
          </View>

          <View style={[styles.center,styles.flex]}>
            <Text style={styles.font}>客栈，公寓</Text>

          </View>




        </View>
      </View>

        </View>
    );
  }
}

const styles = StyleSheet.create({
 container: {
    marginTop:200,
    marginLeft:5,
    marginRight:5,
    height:84,
    flexDirection:'row',
    borderRadius:5,
    padding:2,
    backgroundColor:'#FF0067',
  },
  item: {
    flex: 1,
    height:80,

  },
  center:{
    justifyContent:'center',
    alignItems:'center',
  },
  flex:{
    flex:1,
  },
  font:{
    color:'#fff',
    fontSize:16,
    fontWeight:'bold',
  },
  lineLeftRight:{
    borderLeftWidth:1/PixelRatio.get(),
    borderRightWidth:1/PixelRatio.get(),
    borderColor:'#fff',
  },
  lineCenter:{
    borderBottomWidth:1/PixelRatio.get(),
    borderColor:'#fff',
  },
});

AppRegistry.registerComponent('Abc', () => Abc);
```


### Text组件实例 

**head.js** 

```
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
    PixelRatio,
  View
} from 'react-native';

class Header extends Component {
  render() {
    return (
       <View style={styles.flex}>
                <Text style={styles.font}>

                    <Text style={styles.font_1}>网易</Text>
                    <Text style={styles.font_2}>新闻</Text>
                    <Text>有态度"</Text>
                </Text>

        </View>
    );
  }
}

const styles = StyleSheet.create({
  flex:{
        marginTop:25,
        height:50,
        borderBottomWidth:3/PixelRatio.get(),
        borderBottomColor:'#EF2D36',
        alignItems:'center',
    },
    font:{
        fontSize:25,
        fontWeight:'bold',
        textAlign:'center',
    },
    font_1:{
        color:'#CD1D1C',
    },
    font_2:{
        color:'#FFF',
        backgroundColor:'#CD1D1C',
    },
});

module.exports=Header;
```


将head.js导入到主JS中的代码为const Header=require('./head');

**主JS详细代码**

实现了列表，并且有点击效果 

```
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
    PixelRatio,
  View
} from 'react-native';

const Header=require('./head');

class Abc extends Component {
  render() {
    return (
     <View style={styles.flex}>
            <Header></Header>
            <List title='一线城市楼市退烧 有房源一夜跌价160万'></List>
            <List title='上海市民称墓地太贵买不起 买房存骨灰'></List>
            <List title='朝鲜再发视频:摧毁青瓦台 一切化作灰烬'></List>
            <List title='生活大爆炸人物原型都好牛逼'></List>

             <ImportantNews
                news={[
                '解放军报报社大楼正在拆除 标识已被卸下(图)',
                '韩国停签东三省52家旅行社 或为阻止朝旅游创汇',
                '南京大学生发起亲吻陌生人活动 有女生献初吻-南京大学生发起亲吻陌生人活动 有女生献初吻-南京大学生发起亲吻陌生人活动 有女生献初吻',
                '防总部署长江防汛:以防御98年量级大洪水为目标'
                ]}>
            </ImportantNews>
      </View>

    );
  }
}


class List extends Component{
    render(){
        return (
            <View style={styles.list_item}>
               <Text style={styles.list_item_font}>{this.props.title}</Text>
            </View>
        );
    }
}


class ImportantNews extends Component{

    show(title){

        alert(title);

    }


    render(){
        var news=[];
        for(var i in this.props.news){
            var text=(
                <Text
                    onPress={this.show.bind(this,this.props.news[i])}
                    numberOfLines={2}
                    style={styles.news_item}
                    key={i}

                    >{this.props.news[i]}</Text>
            );
            news.push(text);
        }
        return (
            <View style={styles.flex}>
                <Text style={styles.news_title}>今日要闻</Text>
                {news}

            </View>

        );
    }
}

const styles = StyleSheet.create({
  flex:{
    flex:1,
  },

    list_item:{
        height:40,
        marginLeft:10,
        marginRight:10,
        borderBottomWidth:1,
        borderBottomColor:'#ddd',
        justifyContent:'center',
    },

    list_item_font:{
        fontSize:16,
    },

    news_item:{
        marginLeft:10,
        marginRight:10,
        fontSize:15,
        lineHeight:40,
    },

    news_title:{
        fontSize:20,
        fontWeight:'bold',
        color:'#CD1D1C',
        marginLeft:10,
        marginTop:15,

    },
});

AppRegistry.registerComponent('Abc', () => Abc);
```


**效果如下** 


![这里写图片描述](http://img.blog.csdn.net/20160624182446906)

![这里写图片描述](http://img.blog.csdn.net/20160624182458596)


### Navigator组件实例

实现了页面跳转和通过Navigator传递数据并回传数据，在componentDidMount中获取传递过来的数据  


```
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
   Navigator,
   ScrollView,
  Text,
    PixelRatio,
  View
} from 'react-native';

const Header=require('./head');

class Abc extends Component {
  render() {
     let defaultName='List';
      let defaultComponent=List;
    return (
        <Navigator
            initialRoute={{ name: defaultName, component: defaultComponent }}
            //配置场景
            configureScene=
                {
            (route) => {

//这个是页面之间跳转时候的动画，具体有哪些？可以看这个目录下，有源代码的: node_modules/react-//native/Libraries/CustomComponents/Navigator/NavigatorSceneConfigs.js

            return Navigator.SceneConfigs.VerticalDownSwipeJump;
          }
          }
            renderScene={
            (route, navigator) =>
             {
            let Component = route.component;
            return <Component {...route.params} navigator={navigator} />
          }
          } />


    );  }
}


class List extends Component {

    constructor(props) {
        super(props);
        this.state = {
             id:1,
            user:null,
        };
    }

    _pressButton() {
        const { navigator } = this.props;
        //为什么这里可以取得 props.navigator?请看上文:
        //<Component {...route.params} navigator={navigator} />
        //这里传递了navigator作为props
        const self=this;
         if(navigator) {
            navigator.push({
                name: 'Detail',
                component: Detail,
                params:{
                    id:this.state.id,
                    //从详情页获取user
                    getUser: function(user) {
                        self.setState({
                            user: user
                        })
                    }
                }
            })
        }
    }


    render(){
         if(this.state.user){
            return(
                <View>
                    <Text style={styles.list_item}>用户信息: { JSON.stringify(this.state.user) }</Text>
                </View>
            );
        }else{
        return (
            <ScrollView style={styles.flex}>
               <Text style={styles.list_item} onPress={this._pressButton.bind(this)} >☆ 豪华邮轮济州岛3日游</Text>
                <Text style={styles.list_item} onPress={this._pressButton.bind(this)}>☆ 豪华邮轮台湾3日游</Text>
                <Text style={styles.list_item} onPress={this._pressButton.bind(this)}>☆ 豪华邮轮地中海8日游</Text>
            </ScrollView>
        );
    }
    }


}


const USER_MODELS = {
    1: { name: 'mot', age: 23 },
    2: { name: '晴明大大', age: 25 }
};

class Detail extends Component{

    constructor(props) {
        super(props);
        this.state = {
             id:null

        };
    }

      componentDidMount() {
        //这里获取从FirstPageComponent传递过来的参数: id
        this.setState({
            id: this.props.id
        });
    }

    _pressButton() {
        const { navigator } = this.props;
        if(this.props.getUser) {
            let user = USER_MODELS[this.props.id];
            this.props.getUser(user);
        }
        if(navigator) {
            //很熟悉吧，入栈出栈~ 把当前的页面pop掉，这里就返回到了上一个页面:List了
            navigator.pop();
        }
    }

    render(){
        return(

            <ScrollView>
             <Text style={styles.list_item} >传递过来的用户id是：{this.state.id}</Text>
              <Text style={styles.list_item} onPress={this._pressButton.bind(this)} >点击我可以跳回去</Text>

            </ScrollView>

        );
    }
}



const styles = StyleSheet.create({
  flex:{
    flex:1,

  },
    list_item:{
        height:40,
        marginLeft:10,
        marginRight:10,
        fontSize:20,
        borderBottomWidth:1,
        borderBottomColor:'#ddd',
        justifyContent:'center',
    },
});

AppRegistry.registerComponent('Abc', () => Abc);
```


**效果如下** 

![这里写图片描述](http://img.blog.csdn.net/20160624201649400)


### TextInput组件实例

```
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
   Navigator,
   ScrollView,
   TextInput,
  Text,
    PixelRatio,
  View
} from 'react-native';


class Abc extends Component {
  render() {
     
           return (

            <View style={[styles.flex,styles.topStatus]}>

                <Search></Search>

            </View>

        );
       }
}


class Search extends Component {
    render(){
        return(
            <View style={[styles.flex,styles.flexDirection]}>
                <View style={[styles.flex,styles.input]}>

                    <TextInput  returnKeyType="search" />

                </View>


                <View style={styles.btn}>
                    <Text style={styles.search}>搜索</Text>
                </View>
            </View>

        );
    }
}



const styles = StyleSheet.create({
 
    flex:{
        flex:1,

    },
    flexDirection:{
        flexDirection:'row',
    },
    topStatus:{
        marginTop:25,
    },
    input:{
        height:45,
        borderColor:'red',
        borderWidth:1,
        marginLeft:10,
        paddingLeft:10,
        borderRadius:5,

    },
    btn:{
        width:45,
        marginLeft:-5,
        marginRight:5,
        backgroundColor:'#23BEFF',
        height:45,
        justifyContent:'center',
        alignItems:'center',
    },
    search:{
        color:'#fff',
        fontSize:15,
        fontWeight:'bold',
    },
});

AppRegistry.registerComponent('Abc', () => Abc);
```

**效果如下** 

![这里写图片描述](http://img.blog.csdn.net/20160624205238566)