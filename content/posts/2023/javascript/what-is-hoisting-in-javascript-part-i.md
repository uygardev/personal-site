---
date: 2023-03-15T10:59:42+03:00
authors: [Uygar]
description: "In this article, I will talk about hoisting, one of the most confusing concepts in JavaScript."
categories:
  - "JavaScript"
tags: ["javascript", "hoisting", "variable", "declaration", "initialization"]
license: "All rights reserved"
image: "https://cdn-images-1.medium.com/max/2800/1*BkZPPGIOfDr2eOvH4xS27A.jpeg"
featuredImage: "https://cdn-images-1.medium.com/max/2800/1*BkZPPGIOfDr2eOvH4xS27A.jpeg"
pageStyle: "normal"
title: "JavaScript: What is Hoisting? — Part I"
subtitle: "Hoisting Behavior on Variables"
series: ["JavaScript: What is Hoisting?"]
series_weight: 1
seriesNavigation: true
canonical: "https://blog.uygar.dev/what-is-hoisting-in-javascript-how-does-it-work-part-i-on-variables-f7a610c888c?source=friends_link&sk=74a7ae33f347065323e98229d3875438"
---

> Seriyi tek makale halinde ve Türkçe olarak [buradan](/posts/javascript-hoisting-nedir/) okuyabilirsiniz.

In this article, I will talk about hoisting, one of the most confusing concepts in Js. Hoisting puts declarations at the beginning of the code when the Engine runs our Js code. Two essential elements that affect the hosting behavior are the type of value and how it is defined.

### Hoisting in Variables

First, let’s talk about the mechanics of variable definition. The variable definition consists of two stages. Declaration and Initialization. We can do these operations on two separate lines or the same line. For example;
```js
var a;      // Declaration
a = 1;      // Initialization
— — — — — — 
var a = 1   // Declaration and Initialization
```
I said that hoisting is the process of putting declarations at the beginning of the code. Based on this definition, we don’t get an error even with declarations after initialization, like below.
```js
a = 1;          // Initialization
var a;          // Declaration
console.log(a)  // => 1
```
**var, let, const; are all hoisted. The difference between var, let, and const hoist is how they are initialized by default.**

For example, the declaration ***var a***; is hoisted and automatically initialized to undefined. Therefore, when ***console.log(a)*** is executed, we see “***undefined***” instead of “***Uncaught ReferenceError: a is not defined***”.
```js
console.log(a)     // => undefined
var a;             // Declaration
a = 1;             // Initialization
console.log(a)     // => 1
```
The hoisted version of the code is as follows:
```js
var a = undefined;
console.log(a);     // => undefined
a = 1; 
console.log(a)      // => 1
```
**In let, declarations are hoisted, but a default value is not initialized.**

For example, in ***let a = 1;*** the declaration part is hoisted, and when console.log(a) is executed, instead of getting a reference error that a is undefined, we get a r**eference error that we cannot access the variable without initializing it**. At ***console.log(a)***, a is hoisted but not initialized to any default value. If we had just declared ***let a;*** we would have received the same error since the variable definition already includes the declaration.
```js
console.log(a); // => Uncaught ReferenceError: Cannot access ‘a’ before initialization”
let a = 1;      // Declaration and Initialization
```
The hoisted version of the code is as follows:
```js
let a;         // Declaration
console.log(a) // => Uncaught ReferenceError: Cannot access ‘a’ before initialization”
a = 1;         // Initialization
```
In ***const a;*** declaration, even if we don’t write ***console.log(a)***, we get an error that the **initializer is missing**. Because const cannot be used without declaring and initializing it, it needs to be declared and initialized on the same line.
```js
const a; // => Uncaught SyntaxError: Missing initializer in const declaration”
```
Now let’s look at the hoisting behavior in const. After passing it as a parameter to the console.log method, let’s define the variable and see what we get.
```js
console.log(a);
const a = 1; // => Uncaught ReferenceError: Cannot access ‘a’ before initialization
```
As you can see, it exhibited the same behavior as let. It was hoisted, but since no default value was initialized in ***console.log(a)***, instead of getting a reference error that a is not defined, we got a **reference error that we cannot access the variable without initializing it**.

As a result, const also hoists declarations but does not initialize a default value.

So what would we see if we typed ***console.log(a)*** directly without any declarations?
```js
console.log(a) // => Uncaught ReferenceError: a is not defined
```
Of course, a reference error that a is not defined: “***Uncaught ReferenceError: a is not defined***”.

[What is Hoisting in Javascript, How Does It Work?-Part II — Hoisting Behavior on Function and Class (Coming Soon)](#)
