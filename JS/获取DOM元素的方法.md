在了解获取DOM元素的方法之前，我们先了解一下什么是DOM树

### DOM树（dom tree）

当浏览器加载HTML页面的时候，首先就是DOM结构的计算，计算出来的DOM结构就是DOM树（把页面中的HTML标签像树桩一样，分析出层级来）

![1564886945188.png](https://i.loli.net/2019/08/04/a1OvmCGSupqZLN9.png)

DOM树描述了标签与标签之间的关系（也就是节点间的关系），并且我们只要知道任何一个标签，都可以依据DOM中提供的属性和方法，获取到页面中任意一个标签或者节点

### 在JS中获取DOM元素的方法

`getElementById`

通过元素的ID获取指定的元素对象

```javascript
 document.getElementById('');
 //document：我们称之为"上下文context",同时也限制了获取元素的范围
```

`getElementBytagName`

在指定的上下文中，根据标签名获取到一组元素集合（HTMLcollection）
- 获取的元素集合是一个类数组（不能直接使用数组中的方法）
- 它会把当前上下文中，子子孙孙（后代）层级中所有的标签都获取到
- 基于这个方法获取到的结果永远都是一个集合，如果想操作集合中具体的每一项，需要基于索引获取到才可以

`getElementsBycalssName`

在指定的上下文中，基于元素的样式类名（calss = 'xxx'）获取到的一组元素集合
- 真实项目中，我们经常基于样式类类给元素设置样式，所有在js中我们也会经常基于样式类来获取元素，但是此方法在IE6-8不兼容

`getElementsByName`

在整个文档中，基于元素的name属性值获取一组节点集合（也是一个类数组）
- 在IE浏览器中，只对表单元素的name属性起作用（正常来说：我们项目中只会给表单元素设置name）

`querySelestor`

```javascript
[context].querySelector();
```
在指定的上下文中基于选择器（类似于CSS选择器）获取到指定的元素对象（获取的是一个元素，哪怕选择器匹配多个，我们只获取第一个）

`querySelestorAll`

在querySeletor的基础上，我们获取到选择器匹配到的所有元素，结果是一个节点集合（NodeList）

querySelector/querySelectorAll都是不兼容IE6-~8浏览器的（不考虑兼容情况下，我们能用byid或者其他方式获取，也尽量不要用这两种方法，因为这两种方法性能消耗较大）

![1561991844387.png](https://i.loli.net/2019/08/03/9OfS1CUEMT3pduP.png)

`document.head`

> 获取head元素对象

`document.body`

> 获取body元素对象

`document.documentElement`

> 获取HTML元素对象

```javascript
// 需求 =>获取浏览器一屏幕的宽度和高度（兼容所有的浏览器）
document.documentElement.clientWidth || document.body.clientWidth
document.documentelement.clientHeight || document.body.clientHeight
```
##### 面试题：获取当前页面中所有ID为active的元素，并且兼容所有的浏览器



```javascript
//首先获取当前文档中所有的HTML标签
//依次遍历这些元素标签对象，谁的ID等于active，咱们就把谁存储起来即可
function queryAllById(id){
    //通过通配符'*'，来获取所有的HTML标签
    var nodeList = document.getElementsByTagName('*')
    //遍历集合中的每一项，把元素ID和传递ID相同的这一项存储起来
    var ary = [];  //用于存储相同元素的数组
    for(var i = 0; i<nodeList.length; i++){
        var item = nodeList[i];
        item.id===id?ary.push(item):null
    }
    return ary;
}
```
