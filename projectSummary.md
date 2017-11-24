### 今天来总结一下在Vue项目中，尤其是后台管理项目，会经常用到弹出框，简单总结一下！ 20.17.11.24上午
```
第一种写法：

this.$message.warning('这里写你要弹出的内容！');

如果页面没有弹出，打开控制台，如果报warning未定义，这时候，你稍微修改下如下：

Message.warning('这里写你要弹出的内容！');


第二种方法：
   Message({
      showClose: true,
      message: '这里写你要弹出的内容！', //response.data.msg
      type: "error"
    });

注意以上两种方法中遇到的warning，是警告的意思！
如果弹出框为成功，就写成success！
如果是错误，就写成error！
如果是消息，就写成info！    

```
### 希望以上简单总结对你在实际开发中有所帮助！