---
title: Error Handling
description: Different methods of error handing
img: https://res.cloudinary.com/djv69vvs7/image/upload/c_scale,w_695/v1629730831/paul-skorupskas-7KLa-xLbSXA-unsplash_ljdhta.jpg
alt: my first blog post
tags:
  - javascript
---

## Try, Catch, Finally
Error Handling is used most when handling data from other sources (asynchronous codes) or user inputs. So those could be unpredictable and unreliable. 

Using 3 key words, you can handle the errors and get the results you need:
- try
- catch
- finally

```js

try {
  //piece of code to test
  // if there's an error, the whole block stops running (might miss some piece of code)
  console.log('start of try runs')
  unicycle // no reference
  console.log('End of trying runs -- never reached')
} catch (error) {
  // runs only if there's an error
  console.log(error)
} finally {
  // run this piece of code regardless of whether there's an error or not
  console.log('Always run this code')
}
console.log('...Then the execution continues')
```

## The Error Object
JS has a built in error object that provides error information when an error ocurs.
The object contains two properties: name and message

Types of name errors:
- EvalError
- RangeError
- ReferenceError
- SyntaxError
- TypeError
- URIError

```js
catch(err) {
 console.log(err.name) // returns type of error
 console.log(err.message) // returns error msg
 console.log(err) // returns name: message
}
```

## Custom Error Message

Use throw to create a custom error
```js
try {
  let x = ''
  if (x == '') throw 'is empty';
} catch (err) {
  console.log(err) // will simply return 'is empty';
}

```

Creating custom error object using existing name errors (It has to be one of the 6 above)
```js
try {
  let x = ''
  if (x == '') throw new SyntaxError('is empty');
} catch (err) {
  console.log(err) // will return SyntaxError: is empty
}

```