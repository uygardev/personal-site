---
date: 2023-03-14T10:53:42+03:00
authors: [Uygar]
description: "When we execute a JS code we have written, every variable we declare has a value in the javascript value universe, whether assigned a value or not."
categories:
  - "TypeScript"
tags: ["typescript", "type operations", "types", "set of values"]
license: "All rights reserved"
image: "https://cdn-images-1.medium.com/max/2800/1*Y5hYr_T_6ItuqfaXw0M_AQ.jpeg"
featuredImage: "https://cdn-images-1.medium.com/max/2800/1*Y5hYr_T_6ItuqfaXw0M_AQ.jpeg"
pageStyle: "normal"
title: "TypeScript: Thinking of Types as Value Sets — Part I"
subtitle: "Value Sets, Types"
series: ["TypeScript: Thinking of Types as Value Sets"]
series_weight: 1
seriesNavigation: true
---

> Seriyi tek makale halinde ve Türkçe olarak [buradan](/posts/typescript-tipler-ve-deger-kumeleri) okuyabilirsiniz.

When we execute a JS code we have written, every variable we declare has a value in the javascript value universe, whether assigned a value or not. These values are “uygar”, 46, true, “saygin”, undefined, null, { name: ‘saygin’, field: ‘Physical Education’ }, (a,b) => a-b… and all possible values. When all these possible values are grouped according to their common properties, each decomposed group becomes a set containing its possible values. We can think of these sets as types.

**Our set contains all possible values, i.e., our universe:**
E = { …, “uygar”, 46, true, “saygin”, undefined, null, { name:’saygin’, field: ‘Physical Education’ }, (a,b) => a-b, … }

In Typescript, the type of the universe, the set of values that contains all possible values, is “unknown”. The type “any” also includes all possible values but differs from “unknown”. We cannot assign a variable of type “unknown” to a variable of another type, and we cannot access the properties of an object of type “unknown”. But type “any” has no such restrictions.

**If we obtain a subset of our universe that contains only numbers, this subset will be our number type:**
number = { …, -2, -1, 0, 1, 2, …, 46, …}
```typescript
// We can express this set as a type as follows.
type number = ... | -2 | -1 | 0 | 1 | 2 | ... | 46 | ...
```
**If we do the same for a subset of binary states:**
* boolean = {true, false} => type boolean = true | false

**For the others, we have to set up our logic similarly.**
* object = {all possible objects}
* function = {all possible functions}
* string = {all possible string expressions}
* undefined = {undefined}

If you notice, we created the types we created **by narrowing from all possible values in our universe**. In other words, for number type, we went to a restriction that includes only numbers from all possible values of the universe set (… | -2 | -2 | -1 | -1 | 0 | 1 | 2 | …). The same logic applies to others.

Hence, the variable type is a set of all possible values the variable can take, and the variable can only have one value from that set at a time T.
```typescript
let birthdate: number = 1990;
```
So what subset has the least elements when all these delimitation operations are finished? The empty set, of course. This corresponds to the **“never”** type in Typescript.

[Typescript: Thinking of Types as Value Sets-Part II — Literal, Union, and Intersection Types, Type Alias (Coming Soon)](#)

### **Source**
[**Effective TypeScript: 62 Specific Ways to Improve Your TypeScript**
_Amazon.com: Effective TypeScript: 62 Specific Ways to Improve Your TypeScript eBook : Vanderkam, Dan: Kindle Store_ www.amazon.com](https://www.amazon.com/gp/product/B07Z8HRZZ3)

[**Understanding Excess Property Checking in Typescript**
_This post was first posted in my newsletter All Things Typescript focused on teaching developers how to build better…_ dev.to](https://dev.to/this-is-learning/understanding-excess-property-checking-in-typescript-ook)

[**Documentation - Object Types**
_How TypeScript describes the shapes of JavaScript objects._ www.typescriptlang.org](https://www.typescriptlang.org/docs/handbook/2/objects.html)

{{< youtube uN1zuV4DGRY >}}