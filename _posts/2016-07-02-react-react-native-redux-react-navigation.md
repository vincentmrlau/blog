---
layout: post
title:  "redux 以及react-navigation的应用 学习笔记"
date:   2017-07-02 00:46:18
tags:
- front-end
- note
- react
- react-native
- react-navigation
- redux
excerpt: redux 和 react-navigation在react-native上的应用
published: true
---
##  redux总结
> react-native, react, react-redux, react-navigaition, redux-thunk, redux-persist

> 用户发出action, reducer函数算出新的state, view重新渲染

##  三大原则

1.  单一数据源，利用props的形式向下传播（react数据流决定）
2.  state只读，通过action修改
3.  通过纯函数reducer来修改组件状态，reducer是描述action动作的纯函数


## react-redux 连接react和redux

```
import { connect, Provider } from 'react-redux'
```

### connet([mapStateToProps], [mapDispatchToProps], [mergeProps])(App)
*   mapStateToProps: 基于全局的state，选择我们要注入的props
*   不同的组件分开connec

```javaScript
function select(state){
    return{
        // 把state.todos注入，读取方法： this.props.visibleTodo
        visibleTodo: state.todos
    }
}

const todoApp connect(select)(App)
// export default todoApp
```

```javaScript
// store = "balabala"
<Provider store={store}>
    <todoApp />
</Provider>
```

##  redux三剑客

```javaScript
// 先确定一下初始状态（状态表示的格式）
export default {
    money: 0,
    lastJob:'no job'
}
```

###  ACTION
*   定义动作，比如抢劫银行、搬砖，但是怎么抢怎么搬还是reducer来定

```javaScript
/**
 * Created by liuyiman on 2017/7/3.
 */

// 定义一个menoy+的action
function addmoney(money){
    return {
        // 可以用一个文件管理type，之所以是type是因为我的reducer要根据这个来判断
        type: 'ADD_MONEY',
        money
    }
}
function setLastJob(job){
    return {
        type: 'SET_JOB',
        job
    }
}
// 定义一个赚钱的方式1 ，抢劫银行
export function robBank(){
    return (dispatch, getState) => {
        // 赚一百万
        let { earn } = getState()
        dispatch(addmoney(earn.money + 1000000))
        return dispatch(setLastJob('robber'))
    }
}
// 定义一个赚钱的方式2，建材转移者
export function moveBrick(){
    return (dispatch, getState) => {
        console.log('s',getState())
        let { earn } = getState()
        // 赚一块钱
        dispatch(addmoney( earn.money + 1 ))
        dispatch(setLastJob('brick mover'))
    }
}
// 破产,数据清零
export function goBroke() {
    return {
        type: 'BROKE',
        money: 0,
        job:'broke'
    }
}
```

### reducers
*   描述action如何改变store(in fact state)

```javaScript
import { combineReducers } from 'redux'
import initializeState from './initializeState'
// 定义一个reducer
function earn( state = initializeState, action ) {
    switch (action.type) {
        case 'ADD_MONEY':
            return{
                ...state,
                money:action.money
            }
        case 'SET_JOB':
            return{
                ...state,
                lastJob:action.job
            }
        case 'BROKE':
            return{
                ...state,
                money:action.money,
                lastJob:action.job
            }
        default:
            return state
    }
}

export default earn
```

### store
*   不同的组件可以 connet 到不同的 store

```javaScript
import { createStore, applyMiddleware, compose, combineReducers } from 'redux'
import thunkMidlleware from 'redux-thunk'
import earn from './reducers'
import { persistStore, autoRehydrate} from 'redux-persist'
import { AsyncStorage } from 'react-native'

// base reducer
let baseReducers = {
    earn: earn
}
/*
* 考虑到后面要将react-navigation的reducer加进来，使用redux-persist,所以写了这个helper
* const store = configStore(reducers)()
* */
const configStore = function (reducers = {}) {
    const rootReducer = combineReducers({
        ...baseReducers,
        ...reducers
    })
    return function (_options = {}) {
        const store = createStore(
            rootReducer,
            _options.initialState,
            compose(
                applyMiddleware(thunkMidlleware),
                autoRehydrate()
            )
        )
        const options = {
            storage: AsyncStorage,
            blacklist: _options.blacklist
        }
        persistStore(store, options)
        return store
    }

}

export default configStore
```

##  more
### 中间件 middleware
* 对store.dispatch的重新定义
* 在发出action和执行reducer之间，添加了其他功能

### redux-thunk
>   使dispatch可以接受函数说作为参数，使异步的action可以被触发
```
// 无论killSomeOne是Action create还是普通的返回{}的action
this.props.dispath(killSomeOne('vincent'))
```

### redux-persist
>   本地保存store状态（react-native 本地缓存），可以设置白名单黑名单自动保存等等，特别好用

```javaScript
/*
* 考虑到后面要将react-navigation的reducer加进来，使用redux-persist,所以写了这个helper
* const store = configStore(reducers)()
* */
const configStore = function (reducers = {}) {
    const rootReducer = combineReducers({
        ...baseReducers,
        ...reducers
    })
    return function (_options = {}) {
        const store = createStore(
            rootReducer,
            _options.initialState,
            compose(
                applyMiddleware(thunkMidlleware),
                autoRehydrate()
            )
        )
        const options = {
            storage: AsyncStorage,
            blacklist: _options.blacklist
        }
        persistStore(store, options)
        return store
    }

}

export default configStore
```

### react-navigation + redux

```javaScript
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
  Button
} from 'react-native';
// navigation
import { TabNavigator, addNavigationHelpers, StackNavigator } from 'react-navigation'
// for connect redux and react
import { Provider,connect } from 'react-redux'
import configStore from './redux/store'
// import actions
import {moveBrick, robBank, goBroke} from './redux/actions'
// initialState
import initialState from './redux/store'

// 赚钱页面
class Earn extends Component {
  constructor(props) {
    super(props)
  }
  render(){
    const { earn, dispatch } = this.props
    console.log( 'saa',initialState,earn )
    return(
        <View style={styles.container}>
          <Text>先赚一个亿！</Text>
          <Text>my money$:{earn.money}</Text>
          <Text>my last job:{earn.lastJob}</Text>
          <Button title="rob a bank" onPress={() => dispatch(robBank())}/>
          <Button title="move some Brick" onPress={() => dispatch(moveBrick())}/>
          <Text>价格指导：搬砖 -> $1; 打劫 -> $1000000</Text>
            <Button title="broke" onPress={() => dispatch(goBroke())}/>
        </View>
        )
  }
}

// 传入 earn页面的redux
const earnSelect = function (state) {
    return{
        earn: state.earn
    }
}
// 连接，吧earn上面的select传入earn的props里面
const ConnnetedEarn = connect(earnSelect)(Earn)

// tab navigation的另一个页面
class Screen extends Component{
    constructor(props){
        super(props)
    }
    render() {
        const { navigate } = this.props.navigation
        return(
            <View style={styles.container}>
                <Text>SCREEN!</Text>
                <Button title="to stack2" onPress={() => {navigate('Stack2')}}/>
            </View>
        )
    }
}

// 注册一个tag navigator
const AppNav = TabNavigator({
    'earn':{
        screen: ConnnetedEarn
    },
    'screen': {
        screen: Screen
    }
})

// stack navigation 的页面
class Stack2 extends Component {
    constructor(props){
        super(props)
    }
    render(){
        const {navigate} = this.props.navigation
        return (
            <View style={styles.container}>
                <Text>Stack2</Text>
                <Button title="TO APP NAV" onPress={() => navigate('screen')}/>
            </View>
        )
    }
}

/*
* 注册stack
* 一个是 上面的 tag navigation的页面
* 另一个是 上面的 stack2
* */
const StackNav = StackNavigator({
    App:{
        screen: AppNav,
        title: 'app'
    },
    Stack2: {
        screen: Stack2,
        title: 'stack2'
    }
})

const navInitialState = StackNav.router.getStateForAction(AppNav.router.getActionForPathAndParams('screen'))
const navReducer = (state = navInitialState, action) => {
    console.log('state:',state)
    let nextState = StackNav.router.getStateForAction(action, state);
    console.log('action', action)
    return nextState || state
}

/*
* 加入navReducer,生成store
* */
const store = configStore({
    nav: navReducer
})({
    blacklist:['nav']
})

/*
* stack app
* 利用addNavigationHelper吧navigation传进去
* */
class App extends Component{
    render(){
        return(
            <StackNav navigation={addNavigationHelpers({
                dispatch: this.props.dispatch,
                state: this.props.nav
            })} />
        )
    }
}

/*
* 把nav传进去
* */
function select(state) {
    console.log('state',state)
    return {
        nav: state.nav
    }
}
/*
* connect connect!
* */
const ConnectedApp = connect(select)(App)


// 加上 provider和store
class reduxLearn extends Component {
  render() {
    return (
        <Provider store={store}>
          <ConnectedApp/>
        </Provider>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});

// 注册
AppRegistry.registerComponent('reduxLearn', () => reduxLearn);
```

