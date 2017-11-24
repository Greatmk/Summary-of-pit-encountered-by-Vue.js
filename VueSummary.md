###  学习Vue框架遇到的坑总结   Great开发过程曾经遇到的问题  2017.10.11
```
1.写组件的时候，组件的文件夹名称要用小写开头，大写不行，文件夹里面的组件名称可以用驼峰命名法（当时坑死我了，搞了我好久！）

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

9.在开发中下拉框的选项，有时候需要通过调接口来处理，这里有个小坑 如下：（2017.11.14添加）

<el-form-item label="订单金额："  prop="orderAmount">
  <el-select v-model="form.orderAmount" placeholder="请选择订单金额">
    <el-option :label="orderAmount.text" :value="orderAmount.value" v-for="orderAmount in orderAmounts" :key="orderAmount.id">
    </el-option>
  </el-select>
</el-form-item>

接口如下：
this.$axios.get(this.apiUrl + '/api/SelectListManage/GetHmPriceTypeList')
  .then( (response) => {
    this.orderAmounts = response.data.data;
  })

此时orderAmounts是一个数组，从后台拿到所有的下拉框数据，在页面上用v-for 循环，记得:label="orderAmount.text" 这个 需要在label前面加个冒号：，这样才可以绑定上。:value="orderAmount.value" 这个value前面也是，如果不加的话，当你每次选择的时候，选择的始终是最后一个，所以需要加上冒号，
:key="orderAmount.id" 这个大多人可能会忘记写，如果不写控制台也不会报错，但回报警告⚠️，因为遍历的时候需要每一个id，也就是key，所以得记得！！

10.在开发中如果遇到控制台打印出如下东西： console.log(error);  2017.11.24添加）
  Error: Request failed with status code 401
    at createError (createError.js?c712:16)
    at settle (settle.js?4fa6:18)
    at XMLHttpRequest.handleLoad (xhr.js?0730:77)

这个时候你需要转换一下，方法为 console.log(JSON.stringify(error)) ;这样就打印出JSON对象了！！    

```