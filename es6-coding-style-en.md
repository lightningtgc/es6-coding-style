# ECMAScript6 Style Guide--Frontend Team of GF Securities

> This style guide is based on standard specifications drawn JavaScript，only agreed for the ES6 related content

> Such as variable naming convention, whether to add a semicolon or not, please refer to JavaScript specification

> Note that the current code compiling tools ( such as Babel, Traceur) is not perfect , some features to be used with caution

### [ES6 Coding Style Chinese Version](https://github.com/gf-web/es6-coding-style/edit/master/README.md)

## 规范内容

1. [Declarations](#Declarations)
2. [Strings](#Strings)
3. [Destructuring](#Destructuring)
4. [Arrays](#Arrays)
5. [Functions](#Functions)
6. [Classes](#Classes)
7. [Modules](#Modules)

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

#### Coming soon
