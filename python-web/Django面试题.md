# Django面试题

---

### 1.什么是wsgi？

WSGI是Python在处理HTTP请求时，规定的一种处理方式。如一个HTTP Request过来了，那么就有一个相应的处理函数来进行处理和返回结果。WSGI就是规定这个处理函数的参数长啥样的，它的返回结果是长啥样的？至于该处理函数的名子和处理逻辑是啥样的，那无所谓。简单而言，WSGI就是规定了处理函数的输入和输出格式。

### 2.django请求的生命周期？

- 当用户在浏览器中输入url时,浏览器会生成请求头和请求体发给服务端
  请求头和请求体中会包含浏览器的动作(action),这个动作通常为get或者post,体现在url之中.
- url经过Django中的wsgi,再经过Django的中间件,最后url到过路由映射表,在路由中一条一条进行匹配,
  一旦其中一条匹配成功就执行对应的视图函数,后面的路由就不再继续匹配了.
- 视图函数根据客户端的请求查询相应的数据.返回给Django,然后Django把客户端想要的数据做为一个字符串返回给客户端.
- 客户端浏览器接收到返回的数据,经过渲染后显示给用户.

### 3.列举django的内置组件？

- Admin是对model中对应的数据表进行增删改查提供的组件
- model组件：负责操作数据库
- form组件：1.生成HTML代码2.数据有效性校验3校验信息返回并展示
- ModelForm组件即用于数据库操作,也可用于用户请求的验证