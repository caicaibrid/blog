---
title: react总结
date: 2017-08-03 16:03:02
categories: Javascript
tags:
    - Javascript
---

# react 总结

[react-ppt下载](/img/react.ppt)

## react 中的坑

1. 获取store 
    
   在子组件中通过设置 contextTypes ,然后在生命周期中通过 this.context.store 获取对应的 state dispatch subscribe 等方法
   
2. 父子组件通信

   1). 可以通过props进行页面传值
   
   2). 可以通过context进行传值  (可以跨级传递数据)
        
        (1). 在父组件声明子组件的contextTypes接收类型的值 (childContextTypes)
         
         ```
            FundManageDetail.childContextTypes = {
                funding_doc:PropTypes.array,
                props:PropTypes.object,
            }
         ```
        (2). 在父组件中设置子组件的值 (getChildContext()) , getChildContext 该方法将funding_doc的值放在context中,同时必须声明了childrenContextTypes,这样子组件就可以获取了
       
        ```
            getChildContext(){
                return {
                    funding_doc:this.state.funding_doc,
                    props:this.props,
                }
            }
        ```
        
        (3). 子组件通过获取父组件 childContextTypes 对象中设置对应子页面的字段类型的contextTypes
        
        (4). 子组件中就可以通过 this.context 获取到父组件传过来的值

3. componentWillUpdate生命周期造成死循环(在组件接收到新的props或者state但还没有render时被调用)
   
   造成死循环的原因: 该生命周期在接收到新的props会被触发,但是在这里setState的话,也会触发该生命周期,因此造成死循环
   
   解决办法: 1). 可以直接在render里进行逻辑处理
            
            2). 通过 shouldComponentUpdate 生命周期进行判断是否需要触发render的更新
            
            3). 通过 componentWillReceiveProps 生命周期 (在组件接收到一个新的prop时被调用)
            
4. 在项目中同时使用 antd-mobeil 和 rcTable 的时候的一些坑

    1). 可以滚动的 rcTable 嵌套在 tab 里面时,会造成 rcTable 的滚动条失效 
        
        解决办法: 将tab选项卡的 prop swipeable 改为 {false}
         
    2). 在使用 Popup 弹出层的时候,因为第一个参数为html结构,因此在弹出来以后,如果里面有个文本框在修改内容的时候,通过setState不会把值立马显示出来,
        原因是因为需要再次Popup.show 这样才会显示
        
        解决办法: 将html结构写为一个组件,这样在输入的时候,其实改变的是html结构里的state,这样就会实时更新了
        
        官网事例:
        
        ```
            import { Popup, List, Button, InputItem } from 'antd-mobile';
            
            class PopupContent extends React.Component {
              state = {
                sel: '',
              };
              onSel = (sel) => {
                this.setState({ sel });
                this.props.onClose();
              };
              render() {
                return (
                  <List renderHeader={() => `账户总览，选择了：${this.state.sel}`}>
                    <List.Item
                      thumb="https://zos.alipayobjects.com/rmsportal/dNuvNrtqUztHCwM.png"
                      onClick={() => { this.onSel('东吴证券'); }}
                    >东吴证券</List.Item>
                    <List.Item
                      thumb="https://zos.alipayobjects.com/rmsportal/UmbJMbWOejVOpxe.png"
                      onClick={() => { this.onSel('西吴证券'); }}
                    >西吴证券</List.Item>
                    <InputItem value={this.state.val} onChange={val => this.setState({ val })}>输入内容</InputItem>
                  </List>
                );
              }
            }
            
            const Test = () => {
              const onMaskClose = () => {
                console.log('onMaskClose');
                // also support Promise
                // return new Promise((resolve) => {
                //   console.log('1000ms 后关闭');
                //   setTimeout(resolve, 1000);
                // });
              };
              const onClick = (e) => {
                e.preventDefault(); // 修复 Android 上点击穿透
                Popup.show(<PopupContent onClose={() => Popup.hide()} />, { onMaskClose });
              };
              // newInstance() {
              //  const ins = Popup.newInstance();
              //  ins.show(<Button onClick={() => ins.hide()}>关闭</Button>);
              // },
              return (
                <div style={{ padding: '0.3rem' }}>
                  <Button onClick={onClick}>显示</Button>
                </div>
              );
            };
            
            ReactDOM.render(<Test />, mountNode);
        ```

5. webpack中的一个坑,使用es6写完代码,打包的过程中,有些方法低级浏览器不可以识别,造成页面报错

   例如: "caicai".repeat(3)  //es6写法  include some 等方法在低级浏览器都会报错
         => //转化以后
         "caicai".repeat(3)
         
   原因是因为没有配置:babel-polyfill  
   
   解决办法: 在webpack的入口处配置
   
   ```javascript 1.8
        module.exports = {
            entry: [
                   require.resolve('webpack-dev-server/client') + '?/',
                   require.resolve('webpack/hot/dev-server'),
                   "babel-polyfill",// 垫片,加入是为了使用某个浏览器或者其他执行环境不支持的函数或者对象能够使用而添加的原型方法，或者第三方库
                   paths.appIndexJs //入口文件
               ]
        } 
   ```
   
6. refs 可以通过设置 ref 属性获取对应的dom结构 this.refs.属性值

7. 全局获取store 引入store.js (import store from '@/store/store.js') 通过 store.getState() 获取对应的store信息
