# 一，vue3

## 1，setup

用于设置响应式数据和方法等。

函数基本写法：

`const add = () =>{}`

## 2，return

返回数据

## 3，ref响应式数据

用于存储单个基本类型的数据，如：数字、字符串等。

`const number = ref (10)`

修改值：`number.value = 10`

## 4，v-on: click绑定事件

其作用和@click作用一致。

## 5，显示和隐藏v-show

- 切换显示内容时，可以考虑一下这种方法。

```javascript
const web = reactive({
   show: true
            })
const toggle = () => {
   web.show = !web.show
            }
```

- v-show = ‘ ’ 获取数值，如果为true，则显示；如果为false，则不显示。

## 6，v-if条件渲染

true则渲染，false则不渲染。

还可以放置条件表达式。

## 7，动态属性绑定v-bind

简写形式，只在前边加上冒号。

只是单项数据绑定

## 8，v-model

双向数据绑定，

