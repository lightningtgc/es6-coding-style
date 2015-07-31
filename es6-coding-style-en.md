# ECMAScript6 Style Guide - Frontend Team of GF Securities

> This style guide is based on standard specifications of JavaScript，only agreed for the ES6 related content

> Such as variable naming convention, whether to add a semicolon or not, please refer to JavaScript specification

> Note that the current code compiling tools ( such as Babel, Traceur) is not perfect , some features should be used with caution

### [ES6 Coding Style Chinese Version](https://github.com/gf-web/es6-coding-style/blob/master/README.md)

## Contents

1. [Declarations](#declarations)
2. [Strings](#strings)
3. [Destructuring](#destructuring)
4. [Arrays](#arrays)
5. [Functions](#functions)
6. [Classes](#classes)
7. [Modules](#modules)

### Declarations

- 1.1 Variables

> For only valid under the current scope of the variables , you should use `let` instead of ` var`

> For global variable declaration , using `var`, but should avoid excessively declaring global variables which pollutes the global namespace

```js
// Bad
const variables;
const globalObj = null; // not a const
let globalObj = null;

for (var i=0; i<5; i++) {
  console.log(i);
}
console.log(i);


// Good
let variables;
var globalObj = null;

for (let i=0; i<5; i++) {
  console.log(i);
}
console.log(i);
```

- 1.2 Constant

> For constant, using `const` to declare whose naming should be all uppercase following the popular conventions

> For immutable data, using `const` to declare

> Note : `const` and ` let` are only valid within the block level which they are declared

```js
// Bad
const someNum = 123;
const AnotherStr = 'InvariantString';
let SOME_ARR = ['in','variant','array'];
var ANOTHER_OBJ = {
  'invariantObject': true
};


// Good
const SOME_NUM = 123;
const ANOTHER_STR = 'InvariantString';
const SOME_ARR = ['in','variant','array'];
const ANOTHER_OBJ = {
  'invariantObject': true
};

```

#### Strings

- 2.1 Handle multi-line strings , using the template string

> Use backquote ( `) to mark

> More readable code easier to write

```js
// Bad
const tmpl = '<h1>Multi-line strings</h1>\n' +
'<p>This is a new line.</p>';


// Good
const tmpl = `<h1>Multi-line strings</h1>
<p>This is a new line.</p>`;
```

- 2.2 When dealing with string and variable concatenation, use the template string

```js
  // Bad
  function sayHi(name) {
    return 'How are you, ' + name + '?';
  }


  // Good
  function sayHi(name) {
    return `How are you, ${name}?`;
  }
```

#### Destructuring

- 3.1 Layers of nested structures can not exceed 3 layers

```js
// Bad
let obj = {
  'one': [
    { 'newTwo': [
        { 'three': [
            'four': 'too many layers, omg!'
          ]
        }
      ]
  ]
};


// Good
let obj = {
  'one': [
    'two',
    {'twoObj': 'A clear structure' }
  ]
};

```

- 3.2 Parentheses never appear in deconstruction statement

```js
// Bad
[(a)] = [11]; // a undefined
let { a: (b) } = {}; // Parsing error


// Good
let [a, b] = [11, 22];
```

- 3.3 对象解构

> 对象解构 元素与顺序无关

> 对象指定默认值时仅对恒等于undefined ( !== null ) 的情况生效

- 3.3.1 若函数形参为对象时，使用对象解构赋值

```js
// Bad
function someFun(opt) {
  let opt1 = opt.opt1;
  let opt2 = opt.opt2;
  console.log(op1);
}


// Good
function someFun(opt) {
  let {opt1, opt2} = opt;
  console.log( `$(opt1) 加上 $(opt2)` );
}

function someFun( {opt1, opt2} ) {
  console.log(opt1);
}
```

- 3.3.2 若函数有多个返回值时，使用对象解构，不使用数组解构，避免添加顺序的问题

```js
// Bad
function anotherFun() {
  const one = 1, two = 2, three = 3;
  return [one, two, three];
}
const [one, three, two] = anotherFun(); // 顺序乱了


// Good
function anotherFun() {
  const one = 1, two = 2, three = 3;
  return { one, two, three };
}
const { one, three, two } = anotherFun(); // 不用管顺序
```

- 3.3.3 已声明的变量不能用于解构赋值（语法错误）

```js
// 语法错误
let a;
{a} = {a: 123};

```

- 3.4 数组解构

> 数组元素与顺序相关

- 3.4.1 交换变量的值

```js
let x = 1;
let y = 2;

// Bad
let temp;
temp = x;
x = y;
y = temp;


// Good
[x, y] = [y, x]; // 交换变量
```

- 3.4.2 将数组成员赋值给变量时，使用数组解构

```js
const arr = [1,2,3,4,5];

// Bad
const one = arr[0];
const two = arr[1];


// Good
const [one, two] = arr;
```

#### Arrays

- 4.1 Convert array-like object and traversable object( such as `Set`,` Map`) to real array

> Use `Array.from` to convert

```js
// Bad
function foo() {
  let args = Array.prototype.slice.call(arguments);
}


// Good
function foo() {
  let args = Array.from(arguments);
}

```
- 4.2 Remove duplicate values from an array

> Combine `Set` structure  with ` Array.from`

```js
// Bad
// Don't use indexOf, HashTable etc because of their complexity and confusion


// Good
function deduplication(arr) {
  return Array.from(new Set(arr));
}

```
- 4.3 Array Copy

> Use array spreads ... to copy arrays

```js
const items = [1,2,3];

// Bad
const len = items.length;
let copyTemp = [];
for (let i=0; i<len; i++) {
  copyTemp[i] = items[i];
}


// Good
let copyTemp = [...items];
```
- 4.4 To convert an array-like object to an array

> Use `Array.of` to convert

```js
// Bad
let arr1 = new Array(2); // [undefined x 2]
let arr2 = new Array(1,2,3); // [1, 2, 3]


// Good
let arr1 = Array.of(2);  // [2]
let arr2 = Array.of(1,2,3); // [1, 2, 3]
```

#### Functions

- 5.1 When you use function expressions or anonymous functions, use arrow function notation

> Arrow function is much more clear, and binds `this` automatically

```js
// Bad
const foo = function(x) {
  console.log('存在函数提升问题');
  console.log(foo.name); // Return '', function expression can not be named , but function declaration can
};

[1, 2, 3].forEach(function(x) {
  return x + 1;
});


// Good
const foo = x => {
  console.log('不存在函数提升问题');
  console.log(foo.name); // 返回'foo'
};

[1, 2, 3].forEach( x => {
  return x + 1;
});
```

- 5.1.1 箭头函数书写约定

> 函数体只有单行语句时，允许写在同一行并去除花括号

> 当函数只有一个参数时，允许去除参数外层的括号

```js
// Good
const foo = x => x + x; // 注意此处会默认return x + x，有花括号语句块时不会return

[1, 2, 3].map(x => x * x);

```
- 5.1.2 用箭头函数返回一个对象，应用括号包裹

```js
// Bad
let test = x => {x:x}; // 花括号会变成语句块，不表示对象


// Good
let test = x => ({x:x}); // 使用括号可正确return {x:x}
```

- 5.2 立即调用函数 IIFE

> 使用箭头函数

```js
// Bad
(function() {
  console.log('哈');
})();


// Good
(() => {
  console.log('哈');
})();

```

- 5.3 不使用 `arguments`, 采用rest语法`...`代替

> rest参数是真正的数组，不需要再转换

> 注意：箭头函数中不能使用`arguments`对象

```js
// Bad
function foo() {
  let args = Array.prototype.slice.call(arguments);
  return args.join('');
}


// Good
function foo(...args) {
  return args.join('');
}

```

- 5.4 函数参数指定默认值

> 采用函数默认参数赋值语法

```js
// Bad
function foo(opts) {
  opts = opts || {};// 此处有将0，''等假值转换掉为默认值的副作用
}


// Good
function foo( opts = {}) {
  console.log('更加简洁，安全');
}
```

- 5.5 对象中的函数方法使用缩写形式

> 更加简洁

```js
// Bad
const shopObj = {
  des: '对象模块写法',
  foo: function() {
    console.log('对象中的方法');
  }
};

// Good
const shopObj = {
  des: '对象模块写法',
  foo() {
    console.log('对象中的方法');
  }
};
```


#### Classes

- 6.1 类名应使用帕斯卡写法(PascalCased)

```js
class SomeClass{
}
```

- 6.2 定义类时，方法的顺序如下：

  - `constructor`

  - public `get/set` 公用访问器，`set`只能传一个参数

  - public methods 公用方法，以函数命名区分，不带下划线

  - private `get/set` 私有访问器，私有相关命名应加上下划线`_`为前缀

  - private methods 私有方法

```js
class SomeClass {
  constructor() {
    // constructor
  }

  get aval() {
    // public getter
  }

  set aval(val) {
    // public setter
  }

  doSth() {
    // 公用方法
  }

  get _aval() {
    // private getter
  }

  set _aval() {
    // private setter
  }

  _doSth() {
    // 私有方法
  }
}

```

- 6.3 如果不是class类，不使用`new`

```js
// Bad
function Foo() {
}
const foo = new Foo();


// Good
class Foo() {
}
const foo = new Foo();
```


- 6.4 使用真正意思上的类Class写法，不使用`prototype`进行模拟扩展

> Class更加简洁，易维护

```js
// Bad
function Dog(names = []) {
  this._names = [...names];
}
Dog.prototype.bark = function() {
  const currName = this._names[0];
  alert(`one one ${currName}`);
}

// Good
class Dog {
  constructor(names = []) {
    this._name = [...names];
  }
  bark() {
    const currName = this._names[0];
    alert(`one one ${currName}`);
  }
}
```

- 6.5 class应先定义后使用

> 虽然规范里class不存在hoist问题，但转换工具如babel，只是转换为函数表达式，此处仍有hoist

> 使用继承时，应先定义父类再定义子类

```js
// Bad
let foo = new Foo();
class Foo { }


// Good
class Foo { }
let foo = new Foo();

class SubFoo extends Foo {

}
```

- 6.6 `this`的注意事项

> 子类使用`super`关键字时，`this`应在调用`super`之后才能使用

> 可在方法中`return this`来实现链式调用写法

```js
class Foo {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}

// Bad
class SubFoo extends Foo {
  constructor(x, y, z) {
    this.z = z; // 引用错误
    super(x, y);
  }
}


// Good
class SubFoo extends Foo {
  constructor(x, y, z) {
    super(x, y);
    this.z = z; // this 放在 super 后调用
  }
  setHeight(height) {
    this.height = height;
    return this;
  }
}
```


#### 模块

- 7.1 使用`import / export`来做模块加载导出，不使用非标准模块写法

> 跟着标准走的人，运气总不会太差

```js
// Bad
const colors  = require('./colors');
module.exports = color.lightRed;


// Good
import { lightRed } from './colors';
export default lightRed;

```

- 7.2 应确保每个module有且只有一个默认导出模块

> 方便调用方使用

```js
// Bad
const lightRed = '#F07';

export lightRed;


// Good
const lightRed = '#F07';

export default lightRed;
```

- 7.3 `import` 不使用统配符 `* ` 进行整体导入

> 确保模块与模块之间的关系比较清晰

```js
// Bad
import * as colors from './colors';

// Good
import colors from './colors';

```

- 7.4 不要将`import`与`export`混合在一行

> 分开导入与导出，让结构更清晰，可读性更强

```js
// Bad
export { lightRed as default } from './colors';

// Good
import { lightRed } from './colors';
export default lightRed;
```

- 7.5 多变量要导出时应采用对象解构形式

> `export`置于底部，使欲导出变量更加清晰

```js
// Bad
export const lightRed = '#F07';
export const black  = '#000';
export const white  = '#FFF';

// Good
const lightRed = '#F07';
const black  = '#000';
const white  = '#FFF';

export default { lightRed, black, white };
```

#### Coming soon
