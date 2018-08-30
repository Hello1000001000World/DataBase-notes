```js
var obj = {
  foo: function () { console.log(this.bar) },
  bar: 1
};

var bar = 2;

obj.foo() // 1

var foo = obj.foo;
foo() // 2

obj.foo()是通过obj找到foo，所以就是在obj环境执行。一旦var foo = obj.foo，变量foo就直接指向函数本身，所以foo()就变成在全局环境执行。
```

