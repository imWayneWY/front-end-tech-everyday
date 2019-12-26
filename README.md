I think my basic is not good enough, so I want to practise everyday to improve my skills, and also to practise my English. I want to insist it as long as I can. Ok, let's start.

# day 1
_21th, Dec, 2019_

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
_22th, Dec, 2019_

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
_23th, Dec, 2019_
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