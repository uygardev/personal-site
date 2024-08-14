---
title: 'TypeScript: Thinking of Types as Value Sets — Part III'
description: "Type Operations on Object Types"
date: '2024-08-12T16:13:04.406Z'
keywords: ["typescript", "type", "object type", "union", "intersection", "type alias"]
slug: /@uygardev/typescript-thinking-of-types-as-value-sets-part-iii-e3a2174afdcf
authors: [Uygar]
categories:
   - "TypeScript"
tags: ["typescript", "type", "object type", "union", "intersection", "type alias"]
license: "All rights reserved"
image: "https://cdn-images-1.medium.com/max/2800/1*Aas9cSBztwzGAuhYuueDMQ.jpeg"
featuredImage: "https://cdn-images-1.medium.com/max/2800/1*Aas9cSBztwzGAuhYuueDMQ.jpeg"
pageStyle: "normal"
subtitle: "Type Operations on Object Types"
series: ["TypeScript: Thinking of Types as Value Sets"]
series_weight: 3
seriesNavigation: true
canonical: "https://blog.uygar.dev/typescript-thinking-of-types-as-value-sets-part-iii-e3a2174afdcf?source=friends_link&sk=05e1ee2f0f36f28ef3c7d17d77742f8a"
---

> Seriyi tek makale halinde ve Türkçe olarak [buradan](/posts/typescript-tipler-ve-deger-kumeleri) okuyabilirsiniz

> Part one of the series can be found [here](https://blog.uygar.dev/typescript-thinking-of-types-as-value-sets-introduction-c4360942ce4b?source=friends_link&sk=ca054598aa1eaabbc896654e3c4b0997)

Before we begin, I suggest reading this series’s first and second parts for better comprehension. Now, let’s get started.

## Object Types

Until now, we have discussed various types, including number, string, boolean, literal, and union types derived from these types. In addition, we can also define an object type and specify that an object created must provide the object structure defined as this type. We can do this by:

using an anonymous object;
```typescript
    function greet(person: { name: string; age: number }) {
       return "Hello "+ person.name;
    }
```
using an interface;
```typescript
    interface Person {
       name: string;
       age: number;
    }
    
    function greet(person: Person) {
       return "Hello "+ person.name;
    }
```
or using type alias.
```typescript
    type Person = {
       name: string;
       age: number;
    };
    
    function greet(person: Person) {
       return "Hello "+ person.name;
    }
```
What can we do if we want an object only to provide one of two different object types? Let’s look at an example:
```typescript
    type Person = {
       name: string;
       surname: string
    }
    // possible value set => {..., {name: "Alan"; surname: "Turing"}, ... }
    
    type Lifespan = {
       birth: Date;
       death: Date
    }
    // possible value set => { ..., {birth: "1954/06/23"; death: "1954/06/07"}, ...}
```
**Notice that the elements of the possible value set consist of objects, not type properties. **Additionally, in the possible value set of each type, there are infinitely many different objects consisting of combinations of infinitely different values that these properties can take.

### Union Operation on Object Types

Now, let’s create a union type from these two object types:
```typescript
    type PersonSpan = Person | Lifespan
    // possible value set => {..., {name: "Alan"; surname: "Turing"}, {birth: "1912/06/23"; death: "1954/06/07"}, ...}
```
**PersonSpan’s possible value set takes place from the union of these two sets (Person ∪ Lifespan).** So,** **a** **variable of type PersonSpan must satisfy one of the values in this infinite set to avoid a type-checking error.

Now let’s create three objects of type PersonSpan:
```typescript
    let o1 = {
       name: "Alan",
       surname: "Turing",
       birth: new Date('1912/06/23'),
       death: new Date('1954/06/07')
    }
    const ps1: PersonSpan = o1
    // NO ERROR
    
    
    let o2 = {
       name: "Claude Elwood",
       birth: new Date('1916/04/30')
    }
    const ps2: PersonSpan = o2
    // Type '{ name: string; birth: Date; }' is not assignable to type 'PersonSpan'. Property 'death' is missing in type '{ name: string; birth: Date; }' but required in type 'Lifespan'.
    
    
    let o3 = {
       name: "Claude Elwood",
       surname: "Shannon",
    }
    const ps3: PersonSpan = o3
    // NO ERROR
```

The ps2 object’s type annotation threw an error because the PersonSpan type’s value set cannot contain a value object with the name and birth properties. **Logically, the PersonSpan type’s value set cannot include any object with the “name, birth” property, even if it contains an infinite number of objects with the “name, surname” and “birth, death” properties.**

Since ps3 contains name and surname properties and the set resulting from the union type **can contain infinitely many objects with “name, surname” properties, type checking is error-free**. This is also logical.

But why didn’t we get an error in the type assignment in the ps1 variable? Even though there is no possibility of a value object containing “name, surname, birth, death” properties in the value set of the type? **This is because TypeScript is a duck-type language, or in other words, it has a structural type structure. **The basic logic in this structure is that for an object to provide a type, it must provide the properties of that type to a minimum extent.

For example, for an object to be of type Person, it is enough to provide at least one value from an infinite set of possible values.
```typescript
    type Person = {
      name: string;
      surname: string
    }
    // posibble value set => { ..., {name: "Mehmet"; surname: "Salih"}, {name: "Samet"; surname: "Çalışkan"}, {name: "Büşra"; surname: "Kent"}, ...}
```
Therefore, the following object assignment will not give an error. In the infinite set of possible values of the Person type, there is a value that **is an object with the name property “Claude Elwood” and the surname property “Shannon.”** Since these two minimum conditions are met, TypeScript ignores the third property, which is not in the type definition, and the person object passes the type check without any problems.
```typescript
    let scientist = {
       name: "Claude Elwood",
       surname: "Shannon",
       fields: "Mathematics and Electronic Engineering"
    }
    const person: Person = scientist
    // "fields" property is ignored in type checking
```
But there is one exception. If we had defined our object as an object literal, we would get an error that object literals can only specify the defined properties and that there cannot be a value with the “fields” property of the Person type.
```typescript
    // object literal
    const person: Person = {
       name: "Claude Elwood",
       surname: "Shannon",
       fields: "Mathematics and electronic engineering"
    }
    // Type '{ name: string; surname: string; fields: string; }' is not assignable to type 'Person'. Object literal may only specify known properties, and 'fields' does not exist in type 'Person'.
```
This is because a control mechanism called **excess property checking** is activated in type checks in object literals. This mechanism checks the extra properties according to the object type and returns an error, if any. Except for this exception, type-checking in TypeScript works structurally.

Given all this information, we can understand why the following example does not return an error.
```typescript
    let o1 = {
       name: "Alan",
       surname: "Turing",
       birth: new Date('1912/06/23'),
       death: new Date('1954/06/07')
    }
    const ps1: PersonSpan = o1
    // NO ERROR
```
Since the o1 object contains name and surname properties, it provides minimum values of objects containing name and surname from the value set of PersonSpan type. On the other hand, since it includes birth and death properties, it also provides minimum values of objects containing birth and death from the value set of the PersonSpan type. Therefore, it passes the type check without error.

### Intersection Operation on Object Types

Let’s start with the same type of definitions:
```typescript
    type Person = {
       name: string;
       surname: string
    }
    // possible value set => {..., {name: "Alan"; surname: "Turing"}, ... }
    
    type Lifespan = {
       birth: Date;
       death: Date
    }
    // possible value set => { ..., {birth: "1912/06/23"; death: "1954/06/07"}, ...}
```
The possible value set of the intersection of these two types consists of value objects generated by delimiting the universe and satisfying both types. **All value objects that satisfy this type are at the intersection of the possible value sets of Person and Lifespan types**. This operation is expressed with the & operator.
```typescript
    type PersonSpan = Person & Lifespan
    // posibble value set => {..., {name: "Alan"; surname: "Turing", birth: "1912/06/23"; death: "1954/06/07"}, ...}
    
    let o1 = {
       name: "Alan",
       surname: "Turing",
       birth: new Date('1912/06/23'),
       death: new Date('1954/06/07')
    }
    const ps1: PersonSpan = o1
    // NO ERROR
    
    let o2 = {
       name: "Claude Elwood",
       surname: "Shannon"
    }
    const ps2: PersonSpan = o2
    // Type '{ name: string; surname: string; }' is not assignable to type 'PersonSpan'. Type '{ name: string; surname: string; }' is missing the following properties from type 'Lifespan': birth, death
```
Since the ps2 object is not an object containing the “birth, death” properties, it cannot be a member of the set of possible values of the PersonSpan type, and therefore we get an error from the type check.
[**typeAsSet.ts**
*The Playground lets you write TypeScript or JavaScript online in a safe and sharable way.*www.typescriptlang.org](https://www.typescriptlang.org/play?#code/C4TwDgpgBAChBOBnA9gOygXigbwFBQNQEMBbCALikWHgEtUBzAbnwMQFd5izLq7HcAX1y5QkKABlaAMwiIwRdFjwEoAI1rxgAC0oARIsAgtVAEwiHdUA0aEix0OEjQBlBUtgIU6AD6SZcu4iuAA2EMBQyACMmDisUNwUUABEAIIhiskANPEcXKRJyQAqnPQM2fEaWlaoEADu1oYQABQA5FEAnFEATAD0AAwAbL3dAMytAJQ5ZhY6lLUNNi3tHQCsACwDw-0A7JN2AMZo1FBgiFGUTt5uirHRuL29UAASqUWpUACaAPIA0iJhCLIbqxFSEAqUZIAYQy7HMUAAoiE6shkKYKqoqnMEvVGkY2p0ottNqN+vthEdUCczt1Ll5XO47t0Hk8iuBoK1sAkIVQaGUmOpNNilgLBK0oLREAlkBEiIhELQGMQ1GEoMBkGr2VBWlcGYpWgA6WDwZCQLQgbXmSziyVQEiShWMCXoBzarmJXh8xgCrFWEVQMXqdgReAQACO7E0EFMzs14laUlk8n1BoB4Uio1B8Q9KRhRDh0CRKLRGLYnBzyRc2kUqDQFQpxwiZ1GdOcqBuHmQoxZLzeHx+-zTEUQB1oEFQwElEWU2Z50Nh8KLqPR0zL+R4KSrNbrq6g0jHIVMiEhAFlLBASIZaAcpYoY0iIAcaGhr4jGPQIAgyslDo3TvTUFbbxYhHMcJynHtkn3CBD0QZJThNM1QFaBU1VoMAoAAazQZ8QgAH9QeFTFoTDMKaKAiBCQBGQFQS8AC9gkpakANpTw2yzVQKzzAtEWRZdSyocs5y3VBa1QAToNg09z0vScbwowioBgx9n1QV9xwYD8v0YH9hEeKA2Xjd0eT4flBPXJJTO9PcDyPT1+GYAMbSlWtZXlRVlVVdU4w5XVUENKBvjUAArFSoBCWgjHgSi7SIC00BCC15EfGQLUw2s6nQMBEIQSc5CyBSY1aSSj3FUxkDkaUIggAAPKdY1dHUAMNIA)

### Conclusion

As a result, the basic logic that should be remembered in type checks is based on “**Does the assigned value exist or have the possibility of existing in the value set of the type of the object (in cases where the value set contains an infinite number of values)?**”. We have passed the type check when we can answer yes to one of these questions.

![Type Operations Diagram](https://cdn-images-1.medium.com/max/2000/1*yaYqfTwK-MlW0Tjt9NUrDQ.png)*Type Operations Diagram*

### **Source**
[**Effective TypeScript: 62 Specific Ways to Improve Your TypeScript**
_Amazon.com: Effective TypeScript: 62 Specific Ways to Improve Your TypeScript eBook : Vanderkam, Dan: Kindle Store_ www.amazon.com](https://www.amazon.com/gp/product/B07Z8HRZZ3)

[**Understanding Excess Property Checking in Typescript**
_This post was first posted in my newsletter All Things Typescript focused on teaching developers how to build better…_ dev.to](https://dev.to/this-is-learning/understanding-excess-property-checking-in-typescript-ook)

[**Documentation - Object Types**
_How TypeScript describes the shapes of JavaScript objects._ www.typescriptlang.org](https://www.typescriptlang.org/docs/handbook/2/objects.html)

{{< youtube uN1zuV4DGRY >}}