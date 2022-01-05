---
title: Recreate New in Javascript
date: "2022-01-04"
description: "Learn how to recreate new in javascript"
---

I came across an interesting programming challenge in Javascript attempting to recreate the operator *new*.

## New operator in javascript

According to [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new):

The **new operator** lets developers create an instance of an object type or an object that has a constructor function.

```js
  function Car() {}
  Car.prototype.color = 'blue'
  Car.prototype.log = function() {
  }
  const car = new Car()
  console.log(car.color) // 'blue'
```

The **new** keyword does the following:
  1) creates a blank, plan javascript object {}. You can call this the instance
  2) Adds a property to the new object (__proto__)  that links to the constructor function's prototype
  3) Binds the newly created object instance as the ```this``` context (in methods like Function.call or Function.apply)
  4) returns an object/function if the function returns one else it returns the instance

## [__proto__ vs prototype](https://stackoverflow.com/questions/9959727/proto-vs-prototype-in-javascript)

As I was learning about how to do this, I was confused between __proto__ and prototype
```__proto___``` is the actual object that is used in the lookup chain to resolve methods, etc.
```prototype``` is the object that is used to build ```__proto__``` when you create an object with ```new```
```js
(new Car).__proto__ === Car.prototype
(new Car).prototype === undefined
```

## Write our own version of **new**

If I were to create the atarashii ('new' in Japanese) function. It will take in a function constructor.
Let us start with the first creating the instance, this will act as our context for methods that accept a ```this``` context.
Let's copy the Constructor's prototype into the __proto__. This is needed because we need to maintain the context of our function in our new instance. Once our instance is created, we will need to apply the arguments to the Constructor with our Constructor's context. If the function returns a value, we will return that, else we will return the instance we created.

```js
function atarashii(Constructor, ...args) {
  const instance = {
    __proto__ = Constructor.prototype
  }
  const obj = Constructor.apply(instance, args)
  return obj ?? instance
}
```

Let's clean this up a little. __proto__ is a deprecated property that was supported by browsers, however it isn't part of the ECMAScript language spec and isn't safe to use in any production code (see the [docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto) for more info). It is now more appropriate and recommended to use ```Object.getPrototypeOf([[Prototype]])```. We can also use ```Object.create([[Prototype]])``` to build our object. It creates a new object using an existing object as the prototype of the newly created object.

Here is what our function would look like utilizing Object.create

```js
function atarashii(Constructor, ...args) {
  const instance = Object.create(Constructor.prototype)
  const obj = Constructor.apply(instance, args)
  return obj === Object(obj) ? obj : instance
}
```

Let's use it.

```js
function Car(make, year) {
  this.make = make
  this.year = year
  const log = () => console.log(`Car: ${this.make}, Year: ${this.year}`)
}

const car = atarashii(Car, "Hyundai", 2007)
car.log() //Car: Hyundai, Year: 2007
```


