Redux 介绍  
"Redux is all about State Management"

用一张图片来说明Redux工作的原理.

![](/assets/Redux-01.png)

## Actions

并不直接接触store,里面也不存储任何逻辑，也不知道如何操作store,实质上是一个messenger.

## Reducers

主要任务就是接收action然后根据action更新state.

必须是纯函数，同步函数，不能有异步操作。也就是在里面执行http请求之类的。



state更新后会将更新后的state传入相应的component中，这个是自动完成的。







