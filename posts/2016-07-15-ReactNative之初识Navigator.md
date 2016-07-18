---
title: ReactNative之初识Navigator
date: '2016-07-15'
description:
categories:

tags:react native

---

>

	注意：请将文中 {[ 替换为 {X2 , ]} 替换为 }X2

>

使用导航器可以让你在应用的不同场景（页面）间进行切换。

导航器通过路由对象来分辨不同的场景。利用renderScene方法，导航栏可以根据指定的路由来渲染场景。

可以通过configureScene属性获取指定路由对象的配置信息，从而改变场景的动画或者手势。

查看Navigator.SceneConfigs来获取默认的动画和更多的场景配置选项。

>

    getCurrentRoutes() - 获取当前栈里的路由，也就是push进来，没有pop掉的那些。
    jumpBack() - 跳回之前的路由，当然前提是保留现在的，还可以再跳回来，会给你保留原样。
    jumpForward() - 上一个方法不是调到之前的路由了么，用这个跳回来就好了。
    jumpTo(route) - 跳转到已有的场景并且不卸载。
    push(route) - 跳转到新的场景，并且将场景入栈，你可以稍后跳转过去
    pop() - 跳转回去并且卸载掉当前场景
    replace(route) - 用一个新的路由替换掉当前场景
    replaceAtIndex(route, index) - 替换掉指定序列的路由场景
    replacePrevious(route) - 替换掉之前的场景
    resetTo(route) - 跳转到新的场景，并且重置整个路由栈
    immediatelyResetRouteStack(routeStack) - 用新的路由数组来重置路由栈
    popToRoute(route) - pop到路由指定的场景，在整个路由栈中，处于指定场景之后的场景将会被卸载。
    popToTop() - pop到栈中的第一个场景，卸载掉所有的其他场景。

>

---

>

### 理解例子

>

	1, 在 index.android.js 中定义一个 Navigator 并配置初始化 Component
    2, 通过 this.props.navigator.push 进行 Component 间的跳转和 pop
    3, 传输参数 ... 待补充

>

	// index.android.js
    import React, { Component, PropTypes } from 'react';
    import {
      AppRegistry,
      Text,
      TouchableHighlight,
      View,
      StyleSheet,
      Navigator,
      TextInput,
      Alert,
    } from 'react-native';

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
              return <Component {...route.xxxx} navigator={navigator} />
            } } />
        );
      }
    }

    /*
      所谓props，就是属性传递，而且是单向的。
      React靠一个state来维护状态，当state发生变化则更新DOM。
    */

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
          //  view 默认宽度为 100%
          <View style={ styles.container }>
            <Text style={ styles.text }>欢迎使用</Text>
            <Text style={ styles.text }>欢迎 { this.state.user }</Text>
            <View style={ styles.container }>
              <TextInput
                placeholder='用户名'
                // 保存修改的用户名
                onChangeText={(text) => this.setState({ user: text }) }
                />
              <TextInput
                placeholder='密  码'
                // 保存修改的密码
                onChangeText={(text) => this.setState({ pwd: text }) }
                />
            </View>
            <TouchableHighlight onPress={ () => this._push() } 
            style={ styles.button }>
              <Text>登录</Text>
            </TouchableHighlight>
            <TouchableHighlight onPress={() =>
              Alert.alert(
                '提示信息',
                '欢迎使用',
              ) } style={ styles.button }>
              <Text>提示</Text>
            </TouchableHighlight>
          </View>
        )
      },

      _push: function () {
        if (!this.state.user || !this.state.pwd) {
          console.log("提示")
          Alert.alert(
            '提示信息',
            '用户名或密码不能为空',
          )
        } else {
          this.props.navigator.push({
            name: 'Login',
            component: Login,
            xxxx: {
              user: this.state.user,
              pwd: this.state.pwd
            }
          })
        }
      }
    })

    // StyleSheet
    var styles = StyleSheet.create({
      container: {
        backgroundColor: '#FFFFFF',
        flex: 1,
        marginTop: 20
      },
      button: {
        height: 50,
        backgroundColor: '#ededed',
        marginTop: 10,
        // 垂直居中用justifyContent
        justifyContent: 'center',
        // 水平居中用alignItems
        alignItems: 'center'
      },
      text: {
        marginTop: 10,
        height: 50,
        // 文本居中
        textAlign: 'center'
      }
    });

    // 注册
    AppRegistry.registerComponent('demo', () => demo);

>

	// login.android.js
    import React, { Component, PropTypes } from 'react';
    import {
      AppRegistry,
      Text,
      TouchableHighlight,
      View,
      TextInput,
      StyleSheet,
    } from 'react-native';

    var About = require("./about")

    // Login Class
    var Login = React.createClass({

      _navigator() {
        this.props.navigator.push({
          name: 'About',
          component: About,
        })
      },

      render() {
        return (
          <View style={ styles.container }>
            <Text style={ styles.text }>登录成功</Text>
            <Text style={ styles.text }>用户名： { this.props.user }</Text>
            <Text style={ styles.text }>密 码: { this.props.pwd }</Text>
            <TouchableHighlight onPress={ () => this.props.navigator.pop() }
            style={ styles.button }>
              <Text>返回</Text>
            </TouchableHighlight>
            <TouchableHighlight onPress={ () =>
              this._navigator()
            } style={ styles.button }>
              <Text>关于</Text>
            </TouchableHighlight>
          </View >
        )
      }
    })

    // StyleSheet
    var styles = StyleSheet.create({
      container: {
        backgroundColor: '#FFFFFF',
        flex: 1,
        marginTop: 60
      },
      button: {
        height: 50,
        backgroundColor: '#ededed',
        marginTop: 10,
        // 垂直居中用justifyContent
        justifyContent: 'center',
        // 水平居中用alignItems
        alignItems: 'center'
      },
      text: {
        marginTop: 10,
        height: 50,
        // 文本居中
        textAlign: 'center'
      }
    });

    module.exports = Login

>

	// about.android.js
    import React, { Component, PropTypes } from 'react';
    import {
        AppRegistry,
        Text,
        TouchableHighlight,
        View,
        StyleSheet,
        Navigator,
    } from 'react-native';

    // About Class
    var About = React.createClass({
        render() {
            return (
                <View style={ styles.container }>
                    <TouchableHighlight onPress={ () => this.props.navigator.pop() }
                    style={ styles.button }>
                        <Text>返回</Text>
                    </TouchableHighlight>
                    <Text>关于软件</Text>
                </View>
            )
        }
    })


    // StyleSheet
    var styles = StyleSheet.create({
        container: {
            backgroundColor: '#FFFFFF',
            flex: 1,
            marginTop: 60
        },
        button: {
            height: 50,
            backgroundColor: '#ededed',
            marginTop: 10,
            // 垂直居中用justifyContent
            justifyContent: 'center',
            // 水平居中用alignItems
            alignItems: 'center'
        },
        text: {
            marginTop: 10,
            height: 50,
            // 文本居中
            textAlign: 'center'
        }
    });

    module.exports = About

>

---

>

---

>

#### 官方例子

>

    import React, { Component, PropTypes } from 'react';
    import {
    	Navigator,
        Text,
        TouchableHighlight,
        View,
        AppRegistry
    } from 'react-native';

    class demo extends Component {
      render() {
        return (
          // Navigator 导航(页面的控制器)
          <Navigator
            // 路由初始化配置信息，就是说页面加载时，第一次需要展现什么内容
            initialRoute=[{ title: '努力加贝' , id: 'main'}]
            // 场景转换动画配置
            configureScene={(route) => {
              return Navigator.SceneConfigs.FadeAndroid;
              // return Navigator.SceneConfigs.VerticalDownSwipeJump;
            } }
            // 渲染场景，读取initialRouter传来的数据，确定显示那些内容。
            renderScene={(route, navigator) =>
              // 渲染 Demo 场景　...
              <Demo
                title={route.title}

                // Function to call when a new scene should be displayed
                onForward={ () => {
                  navigator.push({
                    title: 'Hello World',
                  });
                } }
                // Function to call to go back to the previous scene
                onBack={() => {
                  navigator.pop();
                } }
                />
            }
            />
        )
      }
    }

    // <TouchableHighlight onPress={this.props.onBack}>
    //   <Text>Back</Text>
    // </TouchableHighlight>

    class Demo extends Component {
      render() {
        return (
          <View>
            <Text>{ this.props.title }</Text>
            <TouchableHighlight onPress={this.props.onForward}>
              <Text>进入HELLO页面</Text>
            </TouchableHighlight>
          </View>
        )
      }
    }


    AppRegistry.registerComponent('demo', () => demo);

>

---

>

#### 自定义例

>

    import React, { Component, PropTypes } from 'react';
    import {
      AppRegistry,
      Text,
      TouchableHighlight,
      View,
      StyleSheet,
      Navigator,
    } from 'react-native';


    // 创建 Demo Class 
    var demo = React.createClass({

      // 渲染 ... 按照不同的ID渲染不同的页面
      _renderScene(route, navigator) {
        var routeId = route.id;

        // Home
        if (routeId === 'Home') {
          // 返回, Home对象, 并赋值参数navigator
          // 使用route.passProps属性传递其他参数
          return (<Home navigator={navigator}/>);
        }

        // Login
        if (routeId === 'Login') {
          // 返回, Login对象, 并赋值参数navigator
          // 使用route.passProps属性传递其他参数
          return (<Login {...route.passProps} navigator={navigator}/>);
        }

        // About
        if (routeId === 'About') {
          // 返回, About对象, 并赋值参数navigator
          // 使用route.passProps属性传递其他参数
          return (<About {...route.passProps} navigator={navigator}/>);
        }
      },

      // render 是 React 的最基本方法，用于将模板转为 HTML 语言，并插入指定的 DOM 节点。
      render: function () {
        return (
          // 导航 Navigator(页面的控制器)
          <Navigator
            // 路由初始化配置信息，就是说页面加载时，第一次需要展现什么内容
            initialRoute=[{ id: 'Home', name: 'Home', component: Home }]
            // 渲染场景，读取initialRouter传来的数据，确定显示那些内容。
            renderScene={ (route, navigator) => this._renderScene(route, navigator) }
            // 场景转换动画配置
            configureScene={(route) => {
              // return Navigator.SceneConfigs.VerticalDownSwipeJump;
              // return Navigator.SceneConfigs.FloatFromBottom; // 底部弹出
              return Navigator.SceneConfigs.PushFromRight; // 右侧弹出
            } }
            />
        );
      }
    });

    // About Class
    var About = React.createClass({
      render() {
        return (
          <View style={ styles.container }>
            <TouchableHighlight onPress={ () => this.props.goBack() } 
            style={ styles.button }>
              <Text>返回</Text>
            </TouchableHighlight>
            <Text>关于软件</Text>
            <Text>{ this.props.message }</Text>
          </View>
        )
      }
    })

    // Login Class
    var Login = React.createClass({
      render() {
        return (
          <View style={ styles.container }>
            <TouchableHighlight onPress={ () => this.props.goBack() } 
            style={ styles.button }>
              <Text>返回</Text>
            </TouchableHighlight>
            <Text>开始登录</Text>
            <Text>{ this.props.message }</Text>
          </View>
        )
      }
    })

    /*
      所谓props，就是属性传递，而且是单向的。
      React靠一个state来维护状态，当state发生变化则更新DOM。
    */

    // Home Class
    var Home = React.createClass({

      // 页面跳转 ...
      navigate(id, message) {
        // 推送新页面 ...
        this.props.navigator.push({
          id: id,
          // 使用passProps属性传递其他参数
          passProps: {
            // 传递 message
            message: message,
            // 传递 GoBack 函数
            goBack: this.goBack,
          }
        })
      },

      // 返回 ...
      goBack() {
        // 跳转回去并且卸载掉当前场景 
        this.props.navigator.pop()
      },

      render() {
        return (
          // view 默认宽度为 100%
          <View style={ styles.container }>
            <Text style={ styles.text }>欢迎使用</Text>
            <TouchableHighlight onPress={ () => this.navigate('About', '欢迎来到关于页!') } 
            style={ styles.button }>
              <Text>关于</Text>
            </TouchableHighlight>
            <TouchableHighlight onPress={ () => this.navigate('Login', '欢迎来到登录页!') } 
            style={ styles.button }>
              <Text>登录</Text>
            </TouchableHighlight>
          </View>
        )
      }
    })

    // StyleSheet
    var styles = StyleSheet.create({
      container: {
        flex: 1,
        marginTop: 60
      },
      button: {
        height: 50,
        backgroundColor: '#ededed',
        marginTop: 10,
        // 垂直居中用justifyContent
        justifyContent: 'center',
        // 水平居中用alignItems
        alignItems: 'center'
      },
      text: {
        height: 50,
        // 文本居中
        textAlign: 'center'
      }
    });

    // 注册
    AppRegistry.registerComponent('demo', () => demo);

>
