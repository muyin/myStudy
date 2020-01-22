# instanceof方法

**istanceof运算符** 用于测试构造函数的prototype属性是否出现在对象的原型链中的任何位置

用来检测constructor.prototype是否存在于参数object的原型链上

**语法：** `object instanceof constructor`

**参数：**  
object: 要检测的对象  
constructor: 某个构造函数

```js
    function C(){ }
    function D(){ }

    var o = new C();

    o instanceof C; // true     因为Object.getPrototypeOf(o) === C.prototype
    o instanceof D; // false    因为D.prototype不在o的原型链上
```