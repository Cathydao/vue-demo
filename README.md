# vue-demo

# 响应式原理
 - 设置属性值时，页面数据更新
 ```js
   Object.defineProperty(对象, '设置什么属性名',{
     writeable,
     configable,
     enumberable, // 控制属性是否可枚举，即是不是可以被 for-in 取出来
     set(){}, // 设置属性值
     get(){} // 获取属性值
   })
 ```
 - 对象可以使用递归实现响应式化，但是对数组也需要处理
 - 'push','pop','shift','unshift','reverse','splice','sort'

 -1、改变数组数据时发出通知
    - vue 2.0 中的缺陷，数组发生变化，设置length没法通知（vue 3.0 中使用Proxy 语法解决了这个问题）
- 2、加入的元素应该变成响应式的


- 技巧：如果一个函数已经定义了，我们需要扩展其功能，一般的处理流程是：
- 1、使用一个临时的函数名存储函数
- 2、重新定义原来的函数
- 3、定义扩展的功能
- 4、调用临时的那个函数
```js
// 扩展函数功能
function func(){
  console.log('原来的函数');
}
let tempFunc = func;
func = function (){
  console.log('更新的函数');
  tempFunc()
}
func()
```

- 扩展数组的push和pop方法
   - 直接修改prototype  不可行，因为会改变所有数组的方法
   - 修改要进行响应式化的数组的原型（__proto__）


- 若直接给对象赋值另一个对象



# 发布订阅模式

- 代理方法(app.name, app._data.name)
   - 将app._data 中的成员映射到 app 上 (因为需要在app.name 上也能访问到同一个数据)
   - app._data 已经是响应式的对象了， 只需要将 app 访问的成员去访问 app._data的对应成员就可以
   例如：
   ```js
     app.name 转换为 app._data.name
     app.xxx 转换为 app._data.xxx
   ```
    引入一个函数 proxy(target, src, prop) ,将 target 的操作 映射到 src.prop上(这是因为当时没有 'Proxy' 语法 ES6 语法)
    将  函数 修改，改为Observer方法
- 事件模型
-