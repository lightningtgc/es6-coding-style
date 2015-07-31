# ECMAScript6 Style Guide - Frontend Team of GF Securities

> This style guide is based on standard specifications of JavaScript，only agreed for the ES6 related content

> Such as variable naming convention, whether to add a semicolon or not, please refer to JavaScript specification

> Note that the current code compiling tools ( such as Babel, Traceur) is not perfect , some features should be used with caution

### [ES6编码规范中文版本](https://github.com/gf-web/es6-coding-style/blob/master/README.md)

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

- 3.3 Object destructuring

> Object elements are not related to the order of sequence

> It is effective only when The object specifies the default value that equals to undefined (! = = null)

- 3.3.1 When a function parameter is an object, the object is used to assign a value.

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

- 3.3.2 To avoid the order problem when adding, if the function have multiple return values, using object destructuring rather than array destructuring

```js
// Bad
function anotherFun() {
  const one = 1, two = 2, three = 3;
  return [one, two, three];
}
const [one, three, two] = anotherFun(); // out of order


// Good
function anotherFun() {
  const one = 1, two = 2, three = 3;
  return { one, two, three };
}
const { one, three, two } = anotherFun(); // let the order be
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

- 6.1 Class name is PascalCased

```js
class SomeClass{
}
```

- 6.2 Class members should be defined following subsequent order:

  - `constructor`

  - public `get/set` public getters and setters，`set` should only take one argument

  - public methods Use camel casing. Do not prefix _

  - private `get/set` private getters and setters. Prefix `_`

  - private methods

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
    // public method
  }

  get _aval() {
    // private getter
  }

  set _aval() {
    // private setter
  }

  _doSth() {
    // private method
  }
}

```

- 6.3 Do not use `new` on functions anymore.

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


- 6.4 Use class statement，deprecating `prototype` extension

> Class is both simpler and more readable than prototype.

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

- 6.5 class should be declared before use

> Even in ES6 standard, class declaration does not hoist. Trans-compiler such as babel convert them into function. So hoist exists.
> Super class should be declared before subclass in inheritance.

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

- 6.6 Caveats for `this`

> For subclass to use `super`, `super` should be the first call in the constructor.

> May use `return this` for chaining.

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
    this.z = z; // Reference error
    super(x, y);
  }
}


// Good
class SubFoo extends Foo {
  constructor(x, y, z) {
    super(x, y);
    this.z = z; // `this` is after `super` call
  }
  setHeight(height) {
    this.height = height;
    return this;
  }
}
```


#### Module

- 7.1 Use `import / export` for dependencies, deprecating commonjs require

> Follow standards and always have a lucky day.

```js
// Bad
const colors  = require('./colors');
module.exports = color.lightRed;


// Good
import { lightRed } from './colors';
export default lightRed;

```

- 7.2 Make sure every module has a default exported namespace

> This makes it easy to use for module users.

```js
// Bad
const lightRed = '#F07';

export lightRed;


// Good
const lightRed = '#F07';

export default lightRed;
```

- 7.3 Do not use `* ` to import all exported variables

> Make modules dependencies as clear as possible.

```js
// Bad
import * as colors from './colors';

// Good
import colors from './colors';

```

- 7.4 Do not mix`import` and `export` on the same line

> Put exports and inputs on separate lines makes code more readable.

```js
// Bad
export { lightRed as default } from './colors';

// Good
import { lightRed } from './colors';
export default lightRed;
```

- 7.5 Use destructuring to make exports clear

> `export` should be placed at the end of each file to make exports clear.

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
