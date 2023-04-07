---
date: 2022-09-30T01:10:27+03:00
authors: [Uygar]
description: "Javascript'te deklare ettiğimiz her değişken javascript değer evreninde bir değere sahip olur. Peki bu değerlerin tip kontrolü mekanizması nasıl çalışır?"
categories:
  - "TypeScript"
tags: ["typescript", "type operations", "type alias", "değer kümeleri"]
license: "All rights reserved"
image: "https://cdn-images-1.medium.com/max/2800/1*_rvkvAE5DhD35FZ_b9zPqw.jpeg"
featuredImage: "https://cdn-images-1.medium.com/max/2800/1*_rvkvAE5DhD35FZ_b9zPqw.jpeg"
pageStyle: "normal"
title: "TypeScript: Tipler ve Değer Kümeleri"
subtitle: "TypeScript: Tipleri, Değer Kümeleri Olarak Düşünmek | Literal, Union ve Intersection Tipler, Type Alias"
---

> Read this post in English [here](/posts/typescript-thinking-of-types-as-value-sets-part-i/).

Yazdığımız bir JS kodunu çalıştırdığımızda deklare ettiğimiz her değişken değer atanmış olsun veya olmasın javascript değer evreninde bir değere sahip olur. Bu değerler "uygar", 46, true, "saygin", undefined, null, { adi: "saygin", brans: "Beden Eğitimi" }, (a,b) => a-b… gibi olası tüm değerler olabilir. Tüm bu olası değerler ortak özelliklerine göre gruplandığında her ayrışan grup kendi içerisinde, olası değerlerini içeren bir küme halini alır. Biz bu kümelerini tipler olarak düşünebiliriz.

**Tüm olası değerleri içeren kümemiz yani evrenimiz:**
E = { …, "uygar", 46, true, "saygin", undefined, null, { adi:"saygin", brans: "Beden Eğitimi" }, (a,b) => a-b, … }

Typescript’te tüm olası değerleri içeren değer seti kümesinin yani evrenin tipi **“unknown”** dur. **“any”** tipi de tüm olası değerleri içerir fakat “unknown” dan bazı farkları vardır. unknown tipindeki bir değişkeni başka tip bir değişkene atayamayız, ayrıca “unknown” tipindeki bir nesnenin property’lerine de erişemeyiz. Fakat “any” tipinde bu şekilde kısıtlamalar yoktur.

**Evrenimizden sadece sayıları içeren bir alt küme elde edersek bu alt küme bizim number tipimiz olacaktır:**
number = {…, -2, -1, 0, 1, 2, …, 46, …}
```typescript
// Bu kümeyi bir tip olarak aşağıdaki şekilde ifade edebiliriz.
type number = … | -2 | -1 | 0 | 1 | 2 | … | 46 | …
```

**Aynı işlemi ikili durumları içeren bir alt küme için yaparsak:**

* boolean = {true, false} => type boolean = true | false

**Diğerleri için de mantığımızı aynı şekilde kurmalıyız.**

* object = {*olası tüm objeler*}

* function = {*olası tüm fonksiyonlar*}

* string = {*olası tüm string ifadeler*}

* undefined = {undefined}

Dikkat ederseniz oluşturduğumuz tipleri evrenimizdeki **tüm olası değerlerden sınırlandırma/daraltma (narrowing) yaparak** oluşturduk. Yani number için evren kümesinin tüm olası değerlerden yalnızca sayıları içeren bir sınırlandırmaya gittik. (… | -2 | -1 | 0 | 1 | 2 | …). Diğerleri için de aynı mantık geçerlidir.

Buradan yola çıktığımızda bir değişkenin tipi, değişkenin alabileceği tüm olası değerleri içeren bir kümeden ibarettir ve değişken bir T anında o kümeden yalnızca bir değere sahip olabilir.
```typescript
let dogumYili: number = 1990;
```
Peki tüm bu sınırlama işlemleri bittiğinde en az sayıda elemana sahip olan alt küme ne olabilir ? Tabii ki boş küme. Bu da Typescript’te ***“*never”*** t*ipine denk gelir.

## **Literal Tipler (Unit Type)**

Öncelikle literal tip nedir onu tanımlayalım. Literal’in kelime anlamı kalıp, değişmez, tam olmakla beraber literal tip number, string ve boolean gibi toplu tiplerin sınırlandırılması ile elde edileden alt tiplerdir. Yani bir anlamda number, string ve boolean kümelerinden elde edilen alt kümelerdir. **Olası değer kümelerinde sadece bir değer bulunur ve bu sebeple değişmezdirler.** Farklı kaynaklarda unit types (birim tipleri) olarak da adlandırılır.

Örnek olarak const ile bir string literal oluşturalım. Aşağıdaki örneklerde kod editörü üzerinde fare imleci ile a ve b değişkenlerinin üzerine geldiğimizde Typescript’in çıkarsadığı tipleri görebiliriz.
```typescript
const a = "uygar" // const a: “uygar”
// olası değer kümesi => {“uygar”}

let b = "saygın" // let b: string
// olası değer kümesi => {…, “saygın”, …}
```
const ile tanımlanan bir değişken değişmez olduğu için olası değer kümesinde yani tipinde yalnızca bir değer vardır ki o da değişkene atanan değerdir. Bu sebeple typescript, tipi string olarak değil “uygar” olarak setler. Burada “uygar” hem değişkenin değeri hem de literal bir tip olmuş olur. **Dikkat edilmesi gereken nokta a halen bir değişken, eşit olduğu “uygar” halen bir değerdir. Typescript bu atamadan yola çıkarak “uygar” adında bir literal tip belirler ve bunu a’nın tipi olarak atar.**

let ile yapılan tanımlada ise b değişkeni değişebilir olduğundan olası değerler kümesi “saygın” string’ine ek olarak tüm olası stringlerdir ve typescript bu yüzden tipi string olarak belirler.

Typescript’in otomatik tanımlamasının yanında kodda kullanmak için biz bir literal tip tanımlamak istersek bu tanımlamayı aşağıdaki şekillerde yapabiliriz.
```typescript
type uygar = "uygar" // string literal tip
type bir = 1         // number literal tip
type dogru = true    // boolean literal tip
```
## Union Tipler

Peki literal tipleri birleştirerek kendi olası tip kümemizi oluşturabilir miyiz? Tabii ki evet.
```typescript
type uygar = "uygar"    // olası değer kümesi => {"uygar"}
type saygin = "saygın"  // olası değer kümesi => {"saygın"}
```

Ve biz olası değer kümesi {“uygar”, “saygin”} olan bir tip yaratmak istediğimizde yukarıdaki iki literal tipi birleştirerek bunu yapabiliriz. Yani yeni tipimizden olan bir değişken T anında “uygar” veya “saygın” değerlerinden yalnızca birini alabilmelidir.** Tersten düşünürsek yeniTip tipine sahip olan bir değişkenin değeri ya uygar tip kümesine ya da saygin tip kümesine ait olmalıdır.**
```typescript
type yeniTip = uygar | saygin
// olası değer kümesi => {“uygar”, “saygın”}
```

Yukarıdaki işlemi yaparak yeniTip tipini oluşturmak için iki olası tip kümesini birleştirmiş olduk. İstersek olası değer kümesini sonradan genişletebiliriz.
```typescript
type yepYeniTip = yeniTip | "ali"
// olası değer kümesi => {"uygar", "saygın", "ali"}
```
Yukarıdaki örneklerde görüldüğü gibi iki tipin birleşiminden oluşan tiplere union (birleşim) tipler denir. **Burada dikkat edilmesi gereken nokta her ne kadar “|” operatörünü kulladıysak da işlemleri olası değerler kümesi mantığı üzerinden düşündüğümüz için olay iki kümenin birleşimi olarak düşünülmelidir.**

İki tipin birleşimine verilen isme de **type alias** (tip takma adı) denir. Yukarıdaki örnekte yepYeniTip tipindeki type alias “yepYeniTip” ‘dir.

## Object Tiplerde Tip Operasyonları

Şimdiye kadar number, string ve boolean gibi tipler ve bu tipler üzeriden türetilen literal ve union tiplerden bahsettik. Diğer taraftan bir obje yapısını da bir tip olarak tanımlayıp, oluşturulan bir objenin bu tip olarak tanımlanan obje yapısını sağlaması gerektiğini belirtebiliriz. Bunu;

isimsiz bir obje kullanarak;
```typescript
function greet(person: { name: string; age: number }) {
   return "Hello " + person.name;
}
```
bir interface kullanarak;
```typescript
interface Person {
   name: string;
   age: number;
}

function greet(person: Person) {
   return "Hello " + person.name;
}
```
ve type alias kullanarak yapabiliriz.
```typescript
type Person = {
   name: string;
   age: number;
};

function greet(person: Person) {
   return "Hello " + person.name;
}
```
Peki bir nesnenin iki farklı obje yapılı tipten sadece birini sağlaması istediğimiz durumlarda ne yapabiliriz ? Gelin bir örnek üzerinden inceleyelim:
```typescript
type Person = {
   name: string;
   surname: string
}
// olası değer kümesi =>{... , {name: "Alan"; surname: "Turing"}, ... }

type Lifespan = {
   birth: Date;
   death: Date
}
// olası değer kümesi => { ... , {birth: "1954/06/23"; death: "1954/06/07"}, ... }
```
**Dikkat edersek olası değer kümesinin elemanları, tip property’leri değil objelerdir. Ve her tipin olası değer kümesinde bu property’lerin alabileceğin sonsuz farklı değerin kombinasyonundan oluşan sonsuz sayıda farklı obje vardır.**

Şimdi gelin bu iki object tipten bir union tip oluşturalım:
```typescript
type PersonSpan = Person | Lifespan
// olası değer kümesi => {... , {name: "Alan"; surname: "Turing"}, {birth: "1912/06/23"; death: "1954/06/07"}, ... }
```
**PersonSpan tipindeki bir değişkenin tip kontrolünde hata almaması için sonsuz elemanlı bu kümedeki değerlerinden birini sağlaması gerekir.**

Şimdi PersonSpan tipinde üç nesne oluşturalım:
```typescript
let o1 = {
   name: "Alan",
   surname: "Turing",
   birth: new Date('1912/06/23'),
   death: new Date('1954/06/07')
}
const ps1: PersonSpan = o1 // HATA YOK

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
const ps3: PersonSpan = o3  // HATA YOK
```
ps2 nesnesindeki tip anotasyonu, PersonSpan tipinin değer kümesinin name ve birth property’lerine sahip olan bir değer objesi içerme olasılığı olmadığı için hata verdi. Bu çok mantıklı.

**PersonSpan tipinin değer kümesi “name, surname” property’lerine sahip sonsuz sayıda objeyi ve “birth, death” property’lerine sahip sahip sonsuz sayıda objeyi içerse bile “name, birth” property’lerine sahip herhangi bir obje içeremez.**

ps3 name ve surname property’lerini içerdiği ve birleşim tipi sonucunda oluşan küme de **“name, surname” property’lerine sahip sonsuz sayıda objeyi içerebileceği için tip kontrolü hatasız sağlanır. Bu da mantıklı.**

Fakat ps1 değişkenindeki tip atamasında tipin değer kümesinde “name, surname, birth, death” property’leri içeren bir değer objesi bulunma ihtimali olmadığı halde neden hata almadık ?

**Burada devreye Typescript’in duck type bir dil olması ya da diğer bir deyişle structural type yapısına sahip olması girer. Bu yapıda temel mantık bir nesnenin bir tipi sağlaması için o tip özelliklerini asgari ölçüde sağlaması gerekliliğidir.**

Örneğin bir nesnenin Person tipinde olabilmesi için sonsuz elemanlı olası değer kümesinden bir değeri asgari olarak sağlaması yeterlidir.
```typescript
type Person = {
  name: string;
  surname: string
}
// olası değer kümesi => { ..., {name: "Mehmet"; surname: "Salih"}, {name: "Samet"; surname: "Çalışkan"}, {name: "Büşra"; surname: "Kent"}, ... }
```
Dolayısıyla aşağıdaki nesne ataması hata vermeyecektir. Çünkü Person tipininin sonsuz elemanlı olası değer kümesinde **obje olan**, **“Claude Elwood” değerinde bir name** ve “**Shannon” değerinde bir surname** property’sine sahip olan bir değer vardır. Bu üç asgari şart sağlandığı için Typescript, tip tanımda olmayan üçüncü property’yi dikkate almaz ve person nesnesi tip kontrolünden sorunsuz geçer.
```typescript
let scientist = {
   name: "Claude Elwood",
   surname: "Shannon",
   fields: "Mathematics and Electronic Engineering"
}
const person: Person = scientist
// "fields" property'si tip kontrolünde dikkate alınmaz
```
Fakat bir istisnai durum vardır. Eğer nesnemizi object literal olarak tanımlasaydık object literallerin sadece tanımlanmış olan property’leri belirtebileceğini ve Person tipinde “fields” property’sine sahip bir değerin olamayacağına dair bir hata alcaktık.
```typescript
const person: Person = {
   name: "Claude Elwood",
   surname: "Shannon",
   fields: "Mathematics and electronic engineering"
}
// Type '{ name: string; surname: string; fields: string; }' is not assignable to type 'Person'. Object literal may only specify known properties, and 'fields' does not exist in type 'Person'.
```
Bunun sebebi object literallerdeki tip kontrollerinde **“excess property checking”** adında bir kontrol mekanizmasının devreye girmesidir. Bu mekanizma nesnenin tipine göre fazla olan property’leri kontrol eder ve varsa hata döner. Bu istisna dışında typescript’te tip kontrolleri structural (yapısal) olarak çalışır.

Tüm bu bilgilerin ışığında artık aşağıdaki örneğin neden hata vermediği anlayabiliriz.
```typescript
let o1 = {
   name: "Alan",
   surname: "Turing",
   birth: new Date('1912/06/23'),
   death: new Date('1954/06/07')
}
const ps1: PersonSpan = o1
```
**o1 nesnesi name ve surname property’lerini içerdiği için PersonSpan tipinin değer kümesindenki name ve surname içeren obje değerlerini asgari düzeyde sağlıyor. Diğer yandan birth ve death property’lerini içerdiği için de PersonSpan tipinin değer kümesindenki birth ve death içeren obje değerlerini de asgari düzeyde sağlıyor.** Bu yüzden tip kontrolünden hata almadan geçiyor.

Siz de denemek isteseniz aşağıdaki bağlantı üzerinden playground’a ulaşabilirsiniz.
[**typeAsSet.ts**](https://www.typescriptlang.org/play?#code/C4TwDgpgBAChBOBnA9gOygXigbwFBQNQEMBbCALikWHgEtUBzAbnwMQFd5izLq7HcAX1y5QkKABlaAMwiIwRdFjwEoAI1rxgAC0oARIsAgtVAEwiHdUA0aEix0OEjQBlBUtgIU6AD6SZcu4iuAA2EMBQyACMmDisUNwUUABEAIIhiskANPEcXKRJyQAqnPQM2fEaWlaoEADu1oYQABQA5FEAnFEATAD0AAwAbL3dAMytAJQ5ZhY6lLUNNi3tHQCsACwDw-0A7JN2AMZo1FBgiFGUTt5uirHRuL29UAASqUWpUACaAPIA0iJhCLIbqxFSEAqUZIAYQy7HMUAAoiE6shkKYKqoqnMEvVGkY2p0ottNqN+vthEdUCczt1Ll5XO47t0Hk8iuBoK1sAkIVQaGUmOpNNilgLBK0oLREAlkBEiIhELQGMQ1GEoMBkGr2VBWlcGYpWgA6WDwZCQLQgbXmSziyVQEiShWMCXoBzarmJXh8xgCrFWEVQMXqdgReAQACO7E0EFMzs14laUlk8n1BoB4Uio1B8Q9KRhRDh0CRKLRGLYnBzyRc2kUqDQFQpxwiZ1GdOcqBuHmQoxZLzeHx+-zTEUQB1oEFQwElEWU2Z50Nh8KLqPR0zL+R4KSrNbrq6g0jHIVMiEhAFlLBASIZaAcpYoY0iIAcaGhr4jGPQIAgyslDo3TvTUFbbxYhHMcJynHtkn3CBD0QZJThNM1QFaBU1VoMAoAAazQZ8QgAH9QeFTFoTDMKaKAiBCQBGQFQS8AC9gkpakANpTw2yzVQKzzAtEWRZdSyocs5y3VBa1QAToNg09z0vScbwowioBgx9n1QV9xwYD8v0YH9hEeKA2Xjd0eT4flBPXJJTO9PcDyPT1+GYAMbSlWtZXlRVlVVdU4w5XVUENKBvjUAArFSoBCWgjHgSi7SIC00BCC15EfGQLUw2s6nQMBEIQSc5CyBSY1aSSj3FUxkDkaUIggAAPKdY1dHUAMNIA)

## Intersection (Kesişim) Tipler

Yine aynı tip tanımlarından yola çıkalım:
```typescript
type Person = {
   name: string;
   surname: string
}
// olası değer kümesi => {... , {name: "Alan"; surname: "Turing"}, ... }

type Lifespan = {
   birth: Date;
   death: Date
}
// olası değer kümesi => { ..., {birth: "1912/06/23"; death: "1954/06/07"}, ... }
```
Bu iki tipin kesişiminin olası değer kümesi evrenden sınırlandırılarak oluşturulan ve iki tipi de aynı anda sağlayan değer objelerinden oluşur. **Yani bu tipi sağlayan tüm değer objeleri Person ve Lifespan tiplerinin olası değer kümerlerinin kesişiminde yer alır. **Bu işlem & operatörü ile ifade edilir.
```typescript
type PersonSpan = Person & Lifespan
// olası değer kümesi => {..., {name: "Alan"; surname: "Turing", birth: "1912/06/23"; death: "1954/06/07"}, ... }

let o1 = {
   name: "Alan",
   surname: "Turing",
   birth: new Date('1912/06/23'),
   death: new Date('1954/06/07')
}
const ps1: PersonSpan = o1 // HATA YOK

let o2 = {
   name: "Claude Elwood",
   surname: "Shannon"
}
const ps2: PersonSpan = o2
// Type '{ name: string; surname: string; }' is not assignable to type 'PersonSpan'. Type '{ name: string; surname: string; }' is missing the following properties from type 'Lifespan': birth, death
```
ps2 nesnesi “birth, death” property’lerini içeren bir nesne olmadığı için PersonSpan tipinin olası değer kümesinin bir üyesi olamaz ve bu yüzden tip kontrolünden hata alırız.

![Tip Operasyonları Şeması](https://cdn-images-1.medium.com/max/2000/1*yaYqfTwK-MlW0Tjt9NUrDQ.png "Tip Operasyonları Şeması")

Sonuç olarak tip kontrollerinde unutulmaması gereken temel mantık **“atanılan değer nesnenin tipinin değer kümesinde var mı ya da olma ihtimali var mı (değer kümesinin sonsuz sayıda değer içerdiği durumlarda) ?”** üzerine kuruludur. Bu sorulardan birine evet cevabını verebildiğimize tip kontrolünden geçmiş oluruz.

### KAYNAKÇA
[**Effective TypeScript: 62 Specific Ways to Improve Your TypeScript**
_Amazon.com: Effective TypeScript: 62 Specific Ways to Improve Your TypeScript eBook : Vanderkam, Dan: Kindle Store_ www.amazon.com](https://www.amazon.com/gp/product/B07Z8HRZZ3)

[**Understanding Excess Property Checking in Typescript**
_This post was first posted in my newsletter All Things Typescript focused on teaching developers how to build better…_ dev.to](https://dev.to/this-is-learning/understanding-excess-property-checking-in-typescript-ook)

[**Documentation - Object Types**
_How TypeScript describes the shapes of JavaScript objects._ www.typescriptlang.org](https://www.typescriptlang.org/docs/handbook/2/objects.html)

{{< youtube uN1zuV4DGRY >}}