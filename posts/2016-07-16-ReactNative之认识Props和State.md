---
title: ReactNative之认识Props和State
date: '2016-07-16'
description:
categories:

tags:other

---

>

	注意：请将文中 {[ 替换为 {X2 , ]} 替换为 }X2

>

#### 通过例子来理解Props和State

>

* 所谓props，就是单向的属性传递，将属性从一个Component传送到另一个Component ...
* 属性多的时候，可以传递一个对象，语法为{...xx}，这是es6的新特性。
* React靠一个state来维护当前Component状态，当state发生变化则更新DOM。

>

    'use strict';
    import React, {
      AppRegistry,
      Component,
      StyleSheet,
      Text,
      View
    } from 'react-native';

    class Hello extends Component {
      render() {
        return (
          <View>
              <Text>{ this.props.title}</Text>
              <Text>{ this.props.text}</Text>
          </View>
        );
      }
    }
    class HelloComponent extends React.Component{
        constructor (props) {
          super(props);
          // 初始化 state 参数
          this.state = {
             appendText: ''
          };
        }
        // 设置 state 参数
        _setText() {
           this.setState({appendText: ' Native!'});
        }
        render() {
           return (
             <View>
               <Text onPress={this._setText()}>
                   {this.props.text + this.state.appendText}
               </Text>
             </View>
           );
        }
    }
    class learn01 extends Component {
      render() {
      	// Props 参数
        const pros = {
           text: 'hi',
           title: 'title'
        }
        return (
          <View>
              <View style=[{height:30}]
              // 传递参数 ...
              <Hello {...pros} />
              <HelloComponent text="React"/>
          </View>
        );
      }
    }

>

---

>

#### 实际项目中的使用

>

	......

    var Login = require("./login")

    // 创建 Demo Class
    export default class demo extends Component {
      render() {
        let defaultName = 'Home';
        let defaultComponent = Home;
        return (
          <Navigator
            initialRoute=[{ name: defaultName, component: defaultComponent }]
            configureScene={(route) => {
              return Navigator.SceneConfigs.VerticalDownSwipeJump;
            } }
            renderScene={(route, navigator) => {
              let Component = route.component;
              // 注意：这里将props参数设定为route.params
              return <Component {...route.params} navigator={navigator} />
            } } />
        );
      }
    }

    // Home Class
    var Home = React.createClass({

      // 初始化 State
      getInitialState: function () {
        return ({
          user: '',
          pwd: '',
        });
      },

      render() {
        return (
          <View style={ styles.container }>
            <Text style={ styles.text }>欢迎使用</Text>
            <Text style={ styles.text }>欢迎 { this.state.user }</Text>
            <View style={ styles.container }>
              <TextInput
                placeholder='用户名'
                // 注意: 这里设置了state则上面{this.state.user}会随之更新
                onChangeText={(text) => this.setState({ user: text }) }
                />
              <TextInput
                placeholder='密  码'
                onChangeText={(text) => this.setState({ pwd: text }) }
                />
            </View>
            <TouchableHighlight onPress={ this._push() } style={ styles.button }>
              <Text>登录</Text>
            </TouchableHighlight>
          </View>
        )
      },

      _push: function () {
          this.props.navigator.push({
            name: 'Login',
            component: Login,
            // 注意：这里设置params既为props
            params: {
              user: this.state.user,
              pwd: this.state.pwd
            }
          })
      }
    })

    ......

>
