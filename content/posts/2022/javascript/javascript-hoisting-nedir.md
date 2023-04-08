---
authors: [ Uygar ]
date: 2022-04-26T01:10:27+03:00
description: "Hoisting Js kodlarımız Engine tarafından çalıştırıldığında deklerasyonların kodun en başına alınması işlemidir. Hosting davranışını etkileyen iki önemli unsur değer (value) türü ve tanımlanma şeklidir."
categories:
  - "JavaScript"
tags: [ "javascript", "hoisting", "hoisting nedir" ]
license: "All rights reserved"
image: "https://cdn-images-1.medium.com/max/2800/1*NVaJNY5KfKVbgAqyUo3OhA.png"
featuredImage: "https://cdn-images-1.medium.com/max/2800/1*NVaJNY5KfKVbgAqyUo3OhA.png"
pageStyle: "normal"
title: "JavaScript: Hoisting Nedir?"
subtitle: "Farklı Değer Türlerinde Hoisting Davranışı"
---

> Read this post in
> English [here](/posts/what-is-hoisting-in-js-part-i/)

Bu yazıda Js’de en çok kafa karıştıran konseptlerden biri olan Hoisting kavramından bahsedeceğim. Hoisting Js kodlarımız
Engine tarafından çalıştırıldığında deklerasyonların kodun en başına alınması işlemidir. Hosting davranışını etkileyen
iki önemli unsur değer (value) türü ve tanımlanma şeklidir.

### Değişkenlerde Hoisting

Öncelikle değişken tanımlama mekaniğinden bahsedelim. Değişken tanımlama iki aşamadan oluşur. Deklerasyon (Declaration)
ve Initialization. Bu işlemleri iki ayrı satırda yapabileceğimiz gibi aynı satırda da yapabiliriz. Örneğin:

```js
var a;  // Declaration
a = 1;  // Initialization
------------
var a = 1 // Declaration ve Initialization
```

Hoisting’in deklerasyonların, kodun en başına alınması işlemi olduğunu söylemiştik. Bu tanımdan yola çıktığımızda,
aşağıdaki gibi initialize’dan sonra gelen deklerasyonlarda bile hata almayız.

```js
a = 1;         // Initialization
var a;         // Declaration
console.log(a) // => 1
```

**var, let, const; hepsi hoist edilir. var, let ve const hoist’i arasındaki tek fark varsayılan olarak nasıl initialize
edildikleridir.**

Örneğin **var a;** deklerasyonu hoist edilir ve otomatik olarak undefined olarak initialize edilir. Bu yüzden
**console.log(a)** satırı çalıştırıldığında konsola **“Uncaught ReferenceError: a is not defined”** hatası yerine
**“undefined”** yazıldığını görürüz.

```js
console.log(a) // => undefined
var a;         // Declaration
a = 1;         // Initialization
console.log(a) // => 1
```

Kodun hoist edilmiş hali şu şekildedir:

```js
var a = undefined;
console.log(a);   // => undefined
a = 1;
console.log(a)    // => 1
```

**let’de deklerasyonlar hoist edilir fakat bir varsayılan değer initialize edilmez.**

Örneğin **_let a = 1;_** tanımlamasında, deklerasyon kısmı hoist edilir ve ***console.log(a)*** satırında
çalıştırıldığında ***a***’nın tanımlı olmadığına dair bir referans hatası yerine, değişkene initialize edilmeden
erişemeyeceğimiz hakkında bir referans hatası alırız. Yani **_console.log(a)_** satırına gelindiğinde **_a_** hoist
edilmiş fakat herhangi bir varsayılan değere initialize edilmemiştir. Sadece **_let a;_** şeklinde bir deklerasyon
yapsaydık da değişken tanımlama zaten deklerasyonu da içerdiği için aynı hatayı almış olacaktık.

```js
console.log(a); // => Uncaught ReferenceError: Cannot access ‘a’ before initialization”
let a = 1;      // Declaration ve Initialization
```

Kodun hoist edilmiş hali şu şekildedir:

```js
let a;         // Declaration
console.log(a) // => Uncaught ReferenceError: Cannot access ‘a’ before initialization”
a = 1;         // Initialization
```

**_const a;_** deklerasyonunda ise **_console.log(a)_** satırını yazmasak bile initializer’ın eksik olduğuna dair bir hata
alırız. Çünkü const, sadece deklere edilip initialize edilmeden kullanılmaz. Hem deklere edilip aynı satırda initialize
edilmesi gerekir.

```js
const a; // => Uncaught SyntaxError: Missing initializer in const declaration"
```

Şimdi de const’daki hoisting davranışına bakalım. Değişkeni console.log metoduna parametre olarak geçtikten sonra
tanımlayalım, bakalım ne sonuç alacağız.

```js
console.log(a);
const a = 1; // => Uncaught ReferenceError: Cannot access 'a' before initialization
```

Görüldüğü üzere let ile aynı davranışı sergiledi. Hoist edildi fakat **_console.log(a)_** satırına gelindiğinde herhangi
bir varsayılan değer initialize edilmediği için **_a_**’nın tanımlı olmadığına dair bir referans hatası yerine,
değişkene initialize edilmeden erişemeyeceğimiz hakkında bir referans hatası aldık.

**Sonuç olarak const’da da deklerasyonlar hoist edilir fakat bir varsayılan değer initialize edilmez.**

Peki hiç bir deklerasyon yapmadan direkt **_console.log(a)_** yazdığımızda ne görecektik.

```js
console.log(a) // => Uncaught ReferenceError: a is not defined
```

Tabi ki **_a_**’nın tanımlı olmadığına dair bir referans hatası: **“Uncaught ReferenceError: a is not defined”**

### Fonksiyonlarda Hoisting

Fonksiyon tanımlaması sadece deklerasyondan oluştuğu için, fonksiyonlardaki hoisting davranışı deklere edilmiş
değişkenlerin hoisting davranışıyla aynıdır.
```js
hoistOlduMu();

function hoistOlduMu() {
   console.log("Oldu!") // => Oldu!
}
```

Örneği çalıştırdığımızda fonksiyon çağrısı deklerasyondan önce olduğu halde konsola **_“Oldu!”_** yazılacaktır.

### Sınıflarda (Class) Hoisting

**Class deklerasyonu kullanılarak tanımlanan sınıflar hoist edilir fakat varsayılan olarak initialize edilmez.**
```js
const p = new User(); // => Uncaught ReferenceError: Cannot access 'User' before initialization
class User {...}
```

Yukarıdaki kodu çalıştırdığımızda class hoist edildiği için ***“Uncaught ReferenceError: User is not defined” ***hatası
almayız. Onun yerine initialize edilmediği için initialize edilmeden User’a ulaşamayacağımıza dair bir hata alırız.

### Fonksiyon ve Sınıf Expression’larında Hoisting

Bir değişken üzerinden tanımlama yapılarak oluşturulan fonksiyonlara Function Expression, sınıflara ise Class Expression
adı verilir.
```js
console.log(hoistOlduMu);     // => undefined
hoistOlduMu()                 // => Uncaught TypeError: hoistOlduMu is not a function
var hoistOlduMu = function() {
   console.log("Olmadı")
}
```

**Kodu çalıştırdığımızda fonksiyonun hoist olmadığını fakat değişken tanımlamalarının deklerasyon kısımlarının hoist
olduğunu anlayabiliriz. **var, let ve const** deklarasyonlarındaki hoist davranışları aynen geçerlidir.** hoistOlduMu
değişkenini tanımlayıp function’a initialize ettiğimizde **var** keyword’ünden dolayı **hoistOlduMu**
tanımalamasının deklerasyon kısmı hoist edilir ve **undefined** olarak initialize edilir. hoistOlduMu değişkenini
console.log ile yazdırdığımızda konsola **_“undefined”_** yazar. Sonraki satırda fonksiyon olarak çağırdığımızda ise
henüz fonksiyona initialize edilmediği için hoistOlduMu değerinin bir fonksiyon olmadığına dair bir tip hatası alırız.
**Sonuç olarak function expressions’larda fonksiyonlar hoist edilmez fakat değişken deklerasyonları hoist edilir.**

Benzer şekilde class expression’larında class kısmı hoist edilmez fakat değişken tanımlamadaki deklerasyon kısmı hoist
edilir. Yine **var, let ve const deklarasyonlarındaki hoisting davranışları aynen geçerlidir.**
```js
console.log(a)         // => undefined
var a = class User {}
console.log(a)         // => class User {}
console.log(a.name)    // => "User"
```

### Sonuç

* **var, let **ve** const** keyword’leri ile tanımlanmış her değişken hoist edilir. Tek fark initialize edilme
  şekilleridir. **let ve const** varsayılan olarak initialize edilmez, **var** ise **undefined** olarak initialize
  edilir.

* **function** ve **class** deklerasyonları da hoist edilir. **class** deklerasyonları varsayılan olarak initialize
  edilmez.

* function ve class expression’larındaki **function ve class’lar** **hoist edilmez** fakat **atama yapıldıkları değişken
  deklerasyonları hoist edilir**. Bu hoisting davranışında **var, let **ve** const** davranışları geçerlidir.

Peki hoist davranışının bize faydası nedir? Bu işlevsellik aslında bir özellik değil davranıştır. Bu yüzden mutlaka bir
faydası olmalı diye düşünemeyiz. Fakat fonksiyonlardaki hoisting davranışı, kodun genel yapısını oluştururken yapıyı
fonksiyon çağrılarıyla kurup sonrasında bu fonksiyonların içini doldurmak açısından daha efektif çalışmamızı
sağlayabilir.


