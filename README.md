I think my basic is not good enough, so I want to practise everyday to improve my skills, and also to practise my English. I want to insist it as long as I can. Ok, let's start.

# day 1
_21st, Dec, 2019_

__What is closure and when to use it?__

A function who have external envirmental varible.

Example: 

```js
function fun() {
  let a = 10;     // Internal varible of fun
  return ()=>{
    return a;     // Now we can access a
  }
}

let gatA = fun();
let a = getA();
console.log(a);   // So we can access a internal varible from outside
```

Then when should we use it?
* scence 1: Set a private varible for a Object and use privileged method to visit it.

```js
function fun() {
  let name = 'weiyan';

  this.getName = function() {
    return name;
  }
}

var fun = new Fun();
console.log(fun.name);        // undefined, we can't access name from outside
console.log(fun.getName());   // success!
```

* scence 2: setTimeout call a function in the reference way.

  When we want to pass a refrence of a function type Object to the first param of _setTimeout_, we need to use closure.

```js
function fun(num) {
  let age = num;
  return function() {
    console.log(age);
  }
}

let getAge = fun(33);  // pass a param, and get reference(closure) of the function
let age = setTimeout(getAge, 1000);
```

* Encapsulation related functions set

```js
let getImgInPositionedDivHtml = (function () {
   let buffAr = [
     '<div id="',
     '',   //index 1, DIV ID attribute  
     '" style="position:absolute;top:',
     '',   //index 3, DIV top position  
     'px;left:',
     '',   //index 5, DIV left position  
     'px;width:',
     '',   //index 7, DIV width  
     'px;height:',
     '',   //index 9, DIV height  
     'px;overflow:hidden;\"><img src=\"',
     '',   //index 11, IMG URL  
     '\" width=\"',
     '',   //index 13, IMG width  
     '\" height=\"',
     '',   //index 15, IMG height  
     '\" alt=\"',
     '',   //index 17, IMG alt text  
     '\"><\/div>'
   ];
   return (function (url, id, width, height, top, left, altText) {
     buffAr[1] = id;
     buffAr[3] = top;
     buffAr[5] = left;
     buffAr[13] = (buffAr[7] = width);
     buffAr[15] = (buffAr[9] = height);
     buffAr[11] = url;
     buffAr[17] = altText;
     return buffAr.join('');
   });
 })();  
```

# day 2
_22nd, Dec, 2019_

Quicksort

```js
function quickSort (arr) {
  if (arr.length <= 1) return arr;
  // use the middle data as a reference value,
  // and delete this data
  let refIndex = Math.floor(arr.lenght/2)   // index of ref value
  let refNum = arr.splice(refIndex, 1)      // get and delete ref value

  let leftArr = [], rightArr = []
  arr.forEach(item => {
    if (item < refNum[0]) {
      leftArr.push(item)
    }
    if (item >= refNum[0]) {
      rightArr.push(item)
    }
  })
  // concat() connect two arrays
  return quickSort(leftArr).concat(refNum, quickSort(rightArr))
}
console.log(quickSort[10,5,15,2,4])
```

# day 3
_23rd, Dec, 2019_
Use CSS to draw a triangle

use border
```css
width: 100px;
height: 100px;
border-style: solid;
border-width: 100px;
border-color: red forestgreen blue cyan;
```

then remove height and height
```css
border-style: solid;
border-width: 100px;
border-color: red forestgreen blue cyan;
```

change 3 of the borders to transparent
```css
border-sytle: solid;
border-width: 100px;
border-color: transparent transparent blue transparent;
```

Now it has shown a triangle, but it still use a rect space, so we change the border-width
```css
border-style: solid;
border-width: 0 100px 100px 100px;
border-color: transparent transparent blue transparent;
```
Success!

# day 4
_24th, Dec, 2019_
Deep Clone

* Array
```js
let newArr = JSON.parse(JSON.stringify(oldArr))
```
or
```js
let newArr = [...oldArr]
```

* Object
```js
function deepClone (obj) {
  for (let i in obj) {
    newObj[i] = typeof obj[i] =- 'object' ? deepClone(obj[i]) : obj[i]
  }
  rerturn newObj
}
```

# day 5
_25th, Dec, 2019_
reduce()

params:
> * callback (a function which will call on every item of the array. It will accept 4 params)
>   + previousValue (the return value from last call)
>   + currentValue (the current item value)
>   + currentIndex (the current item index)
>   + array
> * initialValue (optional)

* example 1  sum: 
_calc the sum for array: arr = [1,2,3,4]_

use forEach()
```js
let arr = [1,2,3,4],
sum = 0;
arr.forEach(function(e){sum += e});  // sum = 10
```

use map()
```js
let arr = [1,2,3,4],
sum = 0;
arr.map(function(obj){sum += obj});  // return undefined array. sum = 10
```

use reduce()
```js
let arr = [1,2,3,4];
arr.reduce(function(pre,cur){return pre + cur});  // return 10
```

* example 2  max:
```js
let max = arr.reduce(function(pre, cur, index, arr){return pre>cur?pre:cur;});
```

* example 3:
> I got a data format like this: let arr = [{name: 'brick1'}, {name: 'brick2', {name: 'brick3}}]
> But I want a format like this: 'brick1, brick2 & brick3'
> Of course [{name: 'brick1'}] will get 'brick1'
> and {} will get ''

```js
let arr = [{name: 'brick1'}, {name: 'brick2', {name: 'brick3}}];
let result = arr.reduce(function(prev, current, index, arr){
  if (index === 0) {
    return current.name;
  } else if (index === arr.length - 1) {
    return prev + '&' + current.name;
  } else {
    return prev + ',' + current.name;
  }
}, '');
```

# day 6
_26th, Dec, 2019_
__debounce and throttle__

_debounce_

Reduce overhead by preventing a function from being called several times in succession. 

```js
function debounce (fn, time) {
  let timeout = null;
  return function() {
    clearTimeout(timeout)
    timeout = setTimeout(() => {
      fn.call(this.arguments)
    }, time)
  }
}

function handler() {
  console.log('debounce success!')
}

debounce(handler, 1000)
```

_throttle_
Want one last invocation to happen after the throttle is over.
```js
function throttle (fn, time) {
  // use closure to set a varible to judge if the fn can run
  let canRun = true
  return function () {
    // if it is false, stop the function call
    if (!canRun) return;
    canRun = false
    setTimeout(()=> {
      fn.call(this.arguments)
      canRun = true
    }, time)
  }
}

function handler() {
  console.log('throttle success!')
}

throttle(handler, 1000)
```

# day 7
_27th, Dec, 2019_

CSS Layout

* Header and Footer's height fixed:

```html
<div class="body">
  <div class="header"></div>
  <div class="section">
    <div class="left"></div>
    <div class="right"></div>
  </div>
  <div class="footer"></div>
</div>
```
+ use __position__

```css
html, body{
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
}
.body{
  position: relative;
  height: 100%;
}
.header{
  height: 80px;
  background-color: #ccc;
}
.section{
  position: absolute;
  top: 80px;
  left: 0;
  right: 0;
  bottom: 80px;
  background-color: #f8f8f9;
}
.footer{
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 80px;
  background-color: #ccc;
}

```

+ use __flex__

```css
html, body{
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
}
.body{
  height: 100%;
  background-color: #808695;
  display: flex;
  flex-direction: column;
}
.header{
  height: 80px;
  background-color: #ccc;
}
.section{
  flex: 1;
  background-color: #f8f8f9;
}
.footer{
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 80px;
  background-color: #ccc;
}

```

# day 8
_28th, Dec, 2019_

* Header's height fixed. Navigation on the left and it's width is fixed

```html
<div class="body">
  <div class="header"></div>
  <div class="section">
    <div class="left"></div>
    <div class="right"></div>
  </div>
  <div class="footer"></div>
</div>
```

+ position

```css
body{
  height: 100%;
  position: relative;
  background-color: #f5f7f9;
}
.header{
  height: 80px;
  background-color: #515A6E;
}
.section{
  background-color: #F5F7F9;
  position: absolute;
  left: 0;
  top: 80px;
  bottom: 0;
  right: 0;
}
.left{
  background-color: #fff;
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 100px;
}
.right{
  background-color: #F5F7F9;
  position: absolute;
  top: 0;
  left: 100px;
  bottom: 0;
  right: 0;
}
```

+ float + margin
```css
.body{
  height: 100%;
  position: relative;
}
.header{
  height: 80px;
  background-color: #515A6E;
}
.section{
  background-color: #AFC7de;
  position: absolute;
  left: 0;
  top: 80px;
  bottom: 0;
  right: 0;
}
.left{
  background-color: #fff;
  float: left;
  width: 100px;
  height: 100%;
}
.right{
  background-color: #F5F7F9;
  height: 100%;
  margin-left: 100px;
}
```

+ flex
```css
.body{
  height: 100%;
  display: flex;
  flex-direction: column;
}
.header{
  height: 80px;
  background-color: #515A6E;
}
.section{
  background-color: #AFC7DE;
  flex: 1;
  display: flex;
}
.left{
  background-color: #fff;
  width: 100px;
}
.right{
  flex: 1;
  background-color: #F5F7F9;
}
```



# day 9
_29th, Dec, 2019_
* Holy Grail layout

```html
<div class="body">
    <div class="header"></div>
    <div class="section">
      <div class="left"></div>
      <div class="center">111</div>
      <div class="right"></div>
    </div>
</div>
```

+ flex

```css
.header{
  height: 80px;
  background-color: #515a6e;
}
.section{
  background-color: #afc7de;
  flex: 1;
  display: flex;
}
.left{
  background-color: #fff;
  width: 100px;
}
.center{
  flex: 1;
  background-color: $f5f7f9;
}
.right{
  width: 100px;
  background-color: #fff;
}
```

+ position

```css
.body{
  height: 100%;
  position: relative;
}
.header{
  height: 80px;
  background-color: #515a6e;
}
.section{
  position: absolute;
  width: 100%;
  left: 0;
  top: 80px;
  bottom: 0;
  right: 0;
  background-color: #afc7de;
}
.left{
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  background-color: #fff;
  width: 100px;
}
.center{
  height: 100%;
  margin-left: 100px;
  margin-right: 100px;
  background-color: #f5f7f9;
}
.right{
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
  width: 100px;
  background-color: #fff;
}
```

+ float + margin

```css
.body{
  height: 100%;
  position: relative;
}
.header{
  height: 80px;
  background-color: #515a6e;
}
.section{
  position: absolute;
  width: 100%;
  left: 0;
  top: 80px;
  bottom: 0;
  right: 0;
  background-color: #afc7de;
}
.left{
  float: left;
  background-color: #fff;
  width: 100px;
  height: 1000%
}
.center{
  height: 100%;
  margin-right: 100px;
  margin-left: 100px;
  background-color: #f5f7f9;
}
.right{
  float: right;
  height: 100%;
  width: 100px;
  background-color: #fff;
}
```

# day 10
_30th,Dec,2019_
Write a function to deal with number, add a ',' in every 3 numbers.

```js
function test (num) {
  let arr1 = [], arr2 = [], arr = []  // arr1 is used for the numbers before Decimal point, and arr2 is used for after that
  arr = num.toString().split('.')
  arr2 = arr[1] ? [...arr[1]] : []
  arr1 = [...arr[0]]
  let newArr1 = arr1.map((item, index) => arr1.length === (index+1) && (index+1)%3 === 0 ? item+',' : item )
  let newArr2 = arr2.map((item, index) => arr2.length === (index+1) && (index+1)%3 === 0 ? item+',' : item )
  newArr2.unshift('.')
  console.log(newArr1.concat(newArr2).join(''))
}
test(123456789.123)
```

# day 11
_31st,Dec,2019_
The last day of 2019, almost forgot this -_-b
* Difference between Webpack, Grunt, and Grunt

Webpack is _Bundler_ . Grunt  and Gulp is _Task Runner_

+ Grunt
  Grunt is earlier than Gulp. They all use JavaScript to  achieve functions of sell script. like use jshint to check the code.

```js
// Gruntfile.js
module.exports = function(grunt) {
  grunt.initConfig({
    // js格式检查任务
    jshint: {
      src: 'src/test.js'
    }
    //  代码压缩打包任务
    uglify: {}
  });
  // 导入任务插件
  grunt.loadnpmTasks('grunt-contrib-uglify');
  // 注册自定义任务, 如果有多个任务可以添加到数组中
  grunt.regusterTask('default', ['jshint'])
}
```

+ Gulp
  Gulp takes advantage of Grunt, the config is easier. Use Stream to simplify the config and output of the tasks.
```js
// gulpfile.js
var gulp = require('gulp');
var jshint = require('gulp-jshint');
var uglify = require('gulp-uglify');

// 代码检查任务 gulp 采取了pipe 方法，用流的方法直接往下传递
gulp.task('lint', function() {
  return gulp.src('src/test.js')
    .pipe(jshint())
    .pipe(jshint.reporter('default'));
});

// 压缩代码任务
gulp.task('compress'， function() {
  return gulp.src('src/test.js')
    .pipe(uglify())
    .pipe(gulp.dest('build'));
});

// 将代码检查和压缩组合，新建一个任务
gulp.task('default', ['lint', 'compress']);
```

+ Webpack
  use loader, can use ES6, and code splitting. It is a powerful tool.
```js
'use strict'
const path = require('path')
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const merge = require('webpack-merge')
const utils = require('./utils')

var config = {
  // 入口
  entry: {
    app: './src/main.js'
  },
  // 出口
  output: {
    path: config.build.assetsRoot,
    filename: '[name].js',
    publicPath: process.env.NODE_ENV === 'production'
      ? config.build.assetsPublicPath
      : config.dev.assetsPublicPath
  },
  // 加载器配置（需要加载器转化的模块类型）
  module: {
    rules: [
      {
        test: '/\.css$/',
        use: [ 'style-loader', 'css-loader' ]
      }
    ]
  }
  // 插件
  plugins: [
    new webpack.DefinePlugin({
      'process.env': require('../config/dev.env')
    }),
    new webpack.HotModuleReplacementPlugin(),
    new HtmlWebpackPlugin({
      filename: 'index.html',
      template: 'index.html',
      inject: true
    }),
  ]

}

module.exports = config
```

# day 12
_1st, Jan, 2020_

use react hooks to build a counter

```js
import React, {useEffect, useState} from 'react'

function Counter() {
  const [count, setCount] = useState(0)
  useEffect(()=> {
    const timer = setInterval(() => {
      setCount(count => count+1)
    }, 1000)

    return ()=>{
      clearInterval(timer)
    }
  }, [])

  return <h1>{count}</h1>
}
```

# day 13
_2nd, Dec, 2020_

Create and Inheritance Class

+ create
new a function, add method and property in prototype

+ inheritance
  * prototype
    ```js
    function Cat(){}
    Cat.prototype = new Animal();
    Cat.protptype.name = 'cat';

    var cat = new Cat();
    ```
  * constructor
    ```js
    function Cat(name) {
      Animal.call(this);
      this.name = name || 'Tom';
    }
    var cat = new Cat();
    ```

# day 14
_3rd, January, 2020_
studied GraphQL today
```js
const express = require('express')
const expressGraphQL = require('express-graphql')
const {
  GraphQLSchema,
  GraphQLObjectType,
  GraphQLString,
  GraphQLList,
  GraphQLNonNull,
  GraphQLInt,
} = require('graphql')

const authors = [
	{ id: 1, name: 'J. K. Rowling' },
	{ id: 2, name: 'J. R. R. Tolkien' },
	{ id: 3, name: 'Brent Weeks' }
]

const books = [
	{ id: 1, name: 'Harry Potter and the Chamber of Secrets', authorId: 1 },
	{ id: 2, name: 'Harry Potter and the Prisoner of Azkaban', authorId: 1 },
	{ id: 3, name: 'Harry Potter and the Goblet of Fire', authorId: 1 },
	{ id: 4, name: 'The Fellowship of the Ring', authorId: 2 },
	{ id: 5, name: 'The Two Towers', authorId: 2 },
	{ id: 6, name: 'The Return of the King', authorId: 2 },
	{ id: 7, name: 'The Way of Shadows', authorId: 3 },
	{ id: 8, name: 'Beyond the Shadows', authorId: 3 }
]

const app = express()
const BookType = new GraphQLObjectType({
  name: 'Book',
  description: 'This represents a book written by an author',
  fields: () => ({
    id: { type: GraphQLNonNull(GraphQLInt)},
    name: {type: GraphQLNonNull(GraphQLString)},
    authorId: {type: GraphQLNonNull(GraphQLInt)},
    author: {
      type: AuthorType,
      resolve: (book) => {
        return authors.find(author => author.id === book.authorId)
      }
    }
  })
})


const AuthorType = new GraphQLObjectType({
  name: 'Author',
  description: 'This represents a an author of a book',
  fields: () => ({
    id: { type: GraphQLNonNull(GraphQLInt)},
    name: {type: GraphQLNonNull(GraphQLString)},
    books: {
      type: new GraphQLList(BookType),
      resolve: (author) => {
        return books.filter(book => book.authorId === author.id)
      }
    }
  })
})


const RootQueryType = new GraphQLObjectType({
  name: 'Query',
  description: 'Root Query',
  fields: () => ({
    books: {
      type: new GraphQLList(BookType),
      description: 'List of All Books',
      resolve: () => books
    },
    authors: {
      type: new GraphQLList(AuthorType),
      description: 'List of All Author',
      resolve: () => authors
    },
    book: {
      type: BookType,
      description: 'A Single Book',
      args: {
        id: { type: GraphQLInt }
      },
      resolve: (parent, args) => books.find(book => book.id === args.id)
    },
    author: {
      type: AuthorType,
      description: 'A Single Author',
      args: {
        id: { type: GraphQLInt }
      },
      resolve: (parent, args) => authors.find(book => book.id === args.id)
    }
  })
})

const RootMutationType = new GraphQLObjectType({
  name: "Mutation",
  description: 'Root Mutation',
  fields: () => ({
    addBook: {
      type: BookType,
      description: 'Add a Book',
      args: {
        name: { type: GraphQLNonNull(GraphQLString)},
        authorId: { type: GraphQLNonNull(GraphQLInt)},
      },
      resolve: (parent, args) => {
        const book = { id: books.length + 1, name: args.name, authorId: args.authorId}
        books.push(book)
        return book
      }
    },
    addAuthor: {
      type: AuthorType,
      description: 'Add a Author',
      args: {
        name: { type: GraphQLNonNull(GraphQLString)},
      },
      resolve: (parent, args) => {
        const author = { id: authors.length + 1, name: args.name}
        authors.push(author)
        return author
      }
    }
  })
})


const schema = new GraphQLSchema({
  query: RootQueryType,
  mutation: RootMutationType
})

app.use('/graphql', expressGraphQL({
  schema,
  graphiql: true
}))
app.listen(5000, ()=> console.log('Server Running'))
```

# day 15
_4th, Jan, 2020_
Why we need keys in React?
Keys is a mark used for track which elements in a list are updated, added, or removed.
```js
render(){
  return(
    <ul>
      {this.state.todoItems.map(({item, key}) => {
        return <li key={key}>{item}</li>
      })}
    </ul>
  )
}
```
When develop, we have to make sure every key in the element is unique. React need key value's help to judge wether this element need rerender in the DIFF method.



# day 16
_5th, Jan, 2020_
What's the difference between __createElement__ and __cloneElement__
* React.createElement(): JSX use React.createElement to build React element. It has 3 params:
  + tag name: like div, span, or React Component.
  + props
  + child components
```js
React.createElement(
  type,
  [props],
  [...children]
)
```

* React.cloneElement(): It is pretty similar with createElement(), but only the first param is different.
  + React element, the new element will copy the old element.

```js
React.cloneElement(
  element,
  [props],
  [...children]
)
```

# day 17
_6th, Jan, 2020_
I am starting practice with leetcode
* Remove Duplicates from Sorted Array

My solution:
```js
var removeDuplicates = function(nums) {
    let result = [];
    nums.forEach((num) => {
        if (result.indexOf(num) < 0) {
            result.push(num)
        } 
    })
    return result;
};

```

A solution I searched on Google:

```js
// Given a sorted array, remove the duplicates in-place such that each element appear only once and return the new length.
// Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

// Example
// Given nums = [1,1,2],
// Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
// It doesn't matter what you leave beyond the new length.

var nums = [1, 1, 2];

var removeDuplicates = function(nums) {
    var sortedArray = nums.sort();
    var len = sortedArray.length - 1;
    var newArr = [];

    if (len >= 0) {
        for (var i = 0; i < len; i++) {
            if (sortedArray[i] !== sortedArray[i + 1]) {
                newArr.push(sortedArray[i]);
            }
        }
        newArr.push(sortedArray[len]);
    }
    return newArr

}

removeDuplicates(nums)
```

My code works in nodejs environment, I don't know why it doesn't work in Leetcode


# day 18
_7th, Jan, 2020_

what's the output of the code below?
```js
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2;
  },
  perimeter: () => 2 * Math.PI * this.radius
};

shape.diameter();
shape.perimeter();
```

answer: 20 and NaN

diameter is a regular function but perimeter is an arrow function.
Arrow function's this is pointing to the context when run it. It means when we run perimeter, it is in the windows context. And windows doesn't have radius.

# day 19

_8th, Jan, 2020_

How to change a html5 web to amp

1. add this line to the bottom of the <head> tag:
```html
<script async src="https://cdn.ampproject.org/v0.js"></script>
```
then we can use #development=1 to debug

for example: 
>>> http://localhost:8000/article.amp.html#development=1

2. Debug
  * The mandatory tag 'meta charset=utf-8' is missing or incorrect.
    Add the following code as the first line of the <head> tag:
    ```html
    <meta charset="utf-8" />
    ```
  
  * The mandatory tag 'link rel=canonical' is missing or incorrect.
    add the following code below the <meta charset="utf-8" /> tag:
    ```html
    <link rel="canonical" href="/article.html">
    ```

  * The mandatory attribute '⚡' is missing in tag 'html ⚡ for top-level html', or The mandatory tag 'html ⚡ for top-level html' is missing or incorrect.
    adding the ⚡attribute to the <html> tag like so:
    ```html
    <html ⚡ lang="en">
    ```
    or
    ```html
    <html amp lang="en">
    ```
  * The mandatory tag 'meta name=viewport' is missing or incorrect.
    add the following HTML snippet to the <head> tag:
    ```html
    <meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">
    ```

  * The attribute 'href' in tag 'link rel=stylesheet for fonts' is set to the invalid value 'base.css'.
    This is because the css file. In AMP, all stylesheet rules must be embedded in the AMP document using <style amp-custom></style> tags, or as inline styles.
    So we need to remove the <link> tag of the css in the head and replace it with an inline <style amp-custom></style> tag
    remove:
    ```html
    <link href="base.css" rel="stylesheet" />
    ```
    Copy all the styles from the base.css file into the <style amp-custom></style> tags
    ```html
    <style amp-custom>

      /* The content from base.css */

    </style>
    ```

  * The tag 'script' is disallowed except in specific forms.
    In general, scripts in AMP are only allowed if they follow two major requirements:

    + All JavaScript must be asynchronous (i.e., include the async attribute in the script tag).
    + The JavaScript is for the AMP library and for any AMP components on the page.

    So we need to remove the unnecessary js file or change it to AMP component
  
  * The mandatory tag 'noscript enclosure for boilerplate' is missing or incorrect.
    or The mandatory tag 'head > style : boilerplate' is missing or incorrect.
    or The mandatory tag 'noscript > style : boilerplate' is missing or incorrect.

    Every AMP document requires the following AMP boilerplate code:
    ```html
    <style amp-boilerplate>body{-webkit-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-moz-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-ms-animation:-amp-start 8s steps(1,end) 0s 1 normal both;animation:-amp-start 8s steps(1,end) 0s 1 normal both}@-webkit-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-moz-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-ms-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-o-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}</style><noscript><style amp-boilerplate>body{-webkit-animation:none;-moz-animation:none;-ms-animation:none;animation:none}</style></noscript>
    ```
    Add the boilerplate code to the bottom of the <head> tag of your document.


  * The tag 'img' may only appear as a descendant of tag 'noscript'. Did you mean 'amp-img'?
    Replace the <img> tag with the above <amp-img> tag 
    ```html
    <amp-img src="mountains.jpg"></amp-img>
    ```
  
  * Layout not supported: container The implied layout 'CONTAINER' is not supported by tag 'amp-img'.
    Because amp-img is not a direct substitute of the traditional HTML img tag. There are additional requirements when using amp-img.
    ```html
    <amp-img src="mountains.jpg" layout="responsive" width="266" height="150"></amp-img>
    ```

    layout="responsive" can make the image responsive


# day 20, 21
_Tur, Fri, 9th.10th, Jan, 2020_

I learned vue this days. Sorry for didn't update here.
https://juejin.im/entry/5a54b747518825734216c3df

# day 22
_Sat, 11th, Jan, 2020_
Given an array, rotate the array to the right by k steps, where k is non-negative.
Example 1:

>Input: [1,2,3,4,5,6,7] and k = 3
>Output: [5,6,7,1,2,3,4]
>Explanation:
>rotate 1 steps to the right: [7,1,2,3,4,5,6]
>rotate 2 steps to the right: [6,7,1,2,3,4,5]
>rotate 3 steps to the right: [5,6,7,1,2,3,4]
Example 2:

>Input: [-1,-100,3,99] and k = 2
>Output: [3,99,-1,-100]
>Explanation: 
>rotate 1 steps to the right: [99,-1,-100,3]
>rotate 2 steps to the right: [3,99,-1,-100]

My answer
```js
var rotate = function(nums, k) {
    var temp = nums;
    for (var i=0; i<k; i++) {
        let num = temp.splice(-1,1);
        temp.splice(0,0,...num);
    }
    console.log(temp)
    return temp;
};
```
# day 23
_13th, Jan, 2020_
Miss another day. Shoot.

Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

Example 1:

>Input: [1,2,3,1]
>Output: true
Example 2:

>Input: [1,2,3,4]
>Output: false
Example 3:

>Input: [1,1,1,3,3,4,3,2,4,2]
>Output: true

```js
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var containsDuplicate = function(nums) {
    let result = false;
    for(let i=0; i<nums.length; i++){
        let num = nums[i];
        let cnt = 0;
        for(let j=0; j<nums.length; i++){
            if(nums[j]===num){
                cnt++;
            }
            if(cnt>1){
                break;
            }
        }
        if(cnt>1){
            result = true;
            break;
        }
    }
    return result;
};
```