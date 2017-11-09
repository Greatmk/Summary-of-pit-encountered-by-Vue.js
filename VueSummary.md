### 学Vue遇到的坑总结  Great开发过程曾经遇到的问题 2017.10.11
```
1.写组件的时候，组件的文件夹名称要用小写，大写不行，文件夹里面的组件名称可以用驼峰命名法（当时坑死我了，搞了我好久！）

2. 给给点击一个页面跳转的时候，用到的是router-link 给上面绑定点击事件 @click=“click” 是没有反应的，需要这样写才会有效果@click.native=“click” 

3. 组件中的template模版里 只能有一个根标签，切记！

4.在router-link里面 如果包含mt-button组件，记得一定要在router-link标签里面添加slot属性，如果只是添加slot属性是不行的，要记得添加slot=“right”或slot=“left”，不然该mt-button组件按钮将不见了，消失，切记！还有slot属性不能不是写在mt-button组件里，切记！！！

5.在项目中，两个不同的地方按钮点击跳到同一个页面，显示页面中其中有一块内容是不一样的，解决方法是，
（1）给点击按钮哪里 添加router-link，然后给route-link添加属性就是：:to="{path:'/SubmitOrders?from=productDetail'}"  ，
（2）然后在跳到那个页面标签上面写上v-show=“from==‘productDetail’ “并且并且并且 
（3）在data那里记得写上

data () {
      return {
        from: ''
      }
    },
    created(){
      this.from = this.$route.query.from
    }


6.vue页面中，现在点击商品进入商品详情页，发现点击进去，进入了页面的中间位置，解决办法是中入口文件main.js里面添加 
const router = new VueRouter({
  mode: 'history',
  routes: routes,
  linkActiveClass :'mui-active',
  scrollBehavior (to, from, savedPosition) {
    return { x: 0, y: 0 }
  }
});
注意：主要是添加scrollBehavior,里面有三个参数，to,from,savedPositionl

7.父组件传递数据给子组件      父 => 子
首先（1）在父组件绑定一个属性。v-bind:message=“ Message”
       （2）在子组件需要在data里面 通过props来接收父组件传过来的值，也就是：props：[ ‘ message’]
       (3)这样父组件的值Message就传到子组件了



8.子组件向父组件传值.   子 => 父
 首先 （1）一般都是给子组件绑定一个点击事件用来触发自定义事件 
                 例如：v-on:click=“sendMsgToParent”
           接下来找data里面 的 methods里面写自定义事件，通过$emit并把想传的值作为第二个参数传过去. 也就是：
Methods: {
	sendMsgToParent(){
		this.$emit(‘listonChild’,false);
	}
}
这里的listonChild就是自定义事件。
父组件<Child v-on:listonChild=“showChildMsg”><Child>
Methods: {
	showChildMsg(data){
		console.log(data);
	}
} 
这里的data就是子组件传过来的，false！

```