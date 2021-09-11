# JS 深拷贝

JavaScript数据类型分为基础数据类型和引用数据类型，基础类型的复制直接复制给其它变量即可，但是JS本身不支持引用类型的深拷贝，所以简单记录如何实现JS的深拷贝。

<!--more-->

目前我比较常用`Object.assign`和`{...object}`来复制对象类型数据，但是这两个方法都只是浅拷贝，举个例子，对于嵌套的对象类型：
```js
let user = {
  name: 'Bob',
  body: {
    weight: 60,
    height: 180
  }
};

let clone = Object.assign({}, user);
clone.name = 'clone';
clone.body.height += 10;

console.log(user);    //{ name: 'Bob', body: { weight: 60, height: 190 } }
console.log(clone);   //{ name: 'clone', body: { weight: 60, height: 190 } }
```
可以看到，在这种情况下副本修改嵌套对象的内容，还是会影响到原来的数据。之前和同事讨论的时候，发现他们会用`JSON.parse(JSON.stringify(obj))`这样的方式来实现深度拷贝，但是搜了一下，这种方式对于特殊的function、正则类型无效，对于循环引用的对象数据会报错，所以局限性还是比较大。

实际工作中，一般用lodash的`cloneDeep`方法来处理此类场景，实现原生JS的深拷贝，就是实现一下这个方法。

Show me the code
```js
function cloneDeep(source) {
  if (source === null) return null;
  if (source instanceof RegExp) return new RegExp(source);
  if (source instanceof Date) return new Date(source);
  if (typeof source === 'Function') return new function(source) {};
  if (typeof source !== 'object') return source;
  
  let clone = Array.isArray(source) ? [] : {};
  for (let key in source) {
    clone[key] = cloneDeep(source[key])
  }
  return clone;
}
```
