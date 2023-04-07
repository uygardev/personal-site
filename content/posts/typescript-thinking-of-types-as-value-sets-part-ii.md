---
date: 2023-04-03T11:53:42+03:00
authors: [Uygar]
description: "Literal, Union, and Intersection Types, Type Alias"
categories:
  - "TypeScript"
tags: ["typescript", "type", "literal", "union", "intersection", "type alias"]
license: "All rights reserved"
image: "https://cdn-images-1.medium.com/max/2800/1*Aas9cSBztwzGAuhYuueDMQ.jpeg"
featuredImage: "https://cdn-images-1.medium.com/max/2800/1*Aas9cSBztwzGAuhYuueDMQ.jpeg"
pageStyle: "normal"
title: "Typescript: Thinking of Types as Value Sets — Part II"
subtitle: "Literal, Union, and Intersection Types, Type Alias"
---

> Seriyi tek makale halinde ve Türkçe olarak [buradan](https://blog.uygar.dev/typescriptte-tipleri-deger-kumeleri-olarak-dusunmek-3cfd5b6adbf4?source=friends_link&sk=47d43b6f579d7e77367cac90a09bd1f9) okuyabilirsiniz

> Part one of the series can be found [here](https://blog.uygar.dev/typescript-thinking-of-types-as-value-sets-introduction-c4360942ce4b?source=friends_link&sk=ca054598aa1eaabbc896654e3c4b0997)

Before starting, if you didn’t read the [first part of the series](https://blog.uygar.dev/typescript-thinking-of-types-as-value-sets-introduction-c4360942ce4b?source=friends_link&sk=ca054598aa1eaabbc896654e3c4b0997) for introduction, I advise you to read it for a better understanding. Then let’s get started.

### Literal Types (Unit Types)

First, let’s define what a literal type is. Literal means mold, immutable, and complete, but literal types are subtypes obtained by limiting collective types such as number, string, and boolean. In a sense, they are subsets of number, string, and boolean sets. **They have only one value in their set of possible values and are immutable.** They are also called unit types in different sources.

As an example, let’s create a string literal with const. In the examples below, we can see the types Typescript infer when we hover the variables “a” and “b” with the mouse cursor on the code editor.
```typescript
const a = "uygar" // const a: "uygar"
// possible value set => {"uygar"}
let b = "saygin" // let b: string
// possible value set => {…,"saygin",…}
```
Since a variable defined with const is immutable, there is only one value in the set of possible values, the type, which is the value assigned to the variable. For this reason, the Typescript sets the type to “uygar”, not string. Here “uygar” becomes both the variable's value and a literal type. **Note that “a” is still a variable, and “uygar” is still a value. Based on this assignment, Typescript sets a literal type called “uygar” and assigns it as the type of “a”.**

In the definition with let, since the variable b is mutable, the set of possible values is all possible strings in addition to the string “uygar”, so Typescript sets the type to string.

In addition to Typescript’s automatic definition, if we want to define a literal type to use in the code, we can define it as follows.
```typescript
type myName = "uygar"    // string literal type
type one = 1             // number literal type
type acceptible = true   // boolean literal type
```
### Union Types

So can we create our own set of possible types by combining literal types? Of course, yes.
```typescript
type myName = "uygar"   // possible value set => {"uygar"}
type broName = "saygin" // possible value set => {"saygin"}
```
And when we want to create a type with a set of possible values {“uygar”, “saygin”}, we can do so by combining the two literal types above. That is, a variable of our new type must be able to take only one of the values “uygar” or “saygin” at time T. **In other words, the value of a variable of type newType must belong to either the myName type set or the broName type set.**
```typescript
type newType = myName | broName
// possible value set => {"uygar", "saygin"}
```
By doing the above, we have combined two sets of possible types to create the newType type. We can expand the set of possible values later if we want.
```typescript
type newerType = newType | "ali"
// possible value set => {"uygar", "saygin", "ali"}
```
As seen in the examples above, the types that consist of the union of two types are called union types. **The point to note here is that even though we use the “|” operator, the event should be considered as a union of two sets since we think of operations in terms of a set of possible values.**

The name given to the combination of two types is called a **type alias**. In the example above, the type alias of type newerType is “newerType”.

### Intersection Types

The intersection of two types consists of value objects whose set of possible values is delimited from the universe and which satisfy both types at the same time. **That is, the new type created must be in the intersection set of the possible values of the types from which it was created.** This is expressed by the “&” operator.

Based on these definitions, what could be the intersection of myName and broName types? {“uygar”} ∩ {“saygin”} is, of course, the empty set since these two sets of values have no common element. From the first part of this article series, the equivalent of the empty set in Typescript is the “never” type. So the intersection of these two types will be “never”.
```typescript
type myName = "uygar"   // possible value set => {"uygar"}
type broName = "saygin" // possible value set => {"saygin"}

type newType = myName & broName // type newType = never
```
Intersection types are more functional for type operations on objects. In the next part, we will discuss union and intersection types again when we talk about type operations on objects.

[Typescript: Thinking of Types as Value Sets-Part III — ](#)Type Operations on Object Types[ (Coming Soon)](#)

### **Source**
[**Effective TypeScript: 62 Specific Ways to Improve Your TypeScript**
_Amazon.com: Effective TypeScript: 62 Specific Ways to Improve Your TypeScript eBook : Vanderkam, Dan: Kindle Store_ www.amazon.com](https://www.amazon.com/gp/product/B07Z8HRZZ3)

[**Understanding Excess Property Checking in Typescript**
_This post was first posted in my newsletter All Things Typescript focused on teaching developers how to build better…_ dev.to](https://dev.to/this-is-learning/understanding-excess-property-checking-in-typescript-ook)

[**Documentation - Object Types**
_How TypeScript describes the shapes of JavaScript objects._ www.typescriptlang.org](https://www.typescriptlang.org/docs/handbook/2/objects.html)

{{< youtube uN1zuV4DGRY >}}