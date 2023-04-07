---
authors: [Uygar]
date: 2022-04-18T15:33:27+03:00
description: "Javascript’teki senkron ve asenkron kavramları başlangıçta kafa karıştırıcı olabiliyor ki ben de bu kafa karışıklığını yaşadım. Bu konudaki işleyişi ve bilinmesi gereken önemli noktaları kafasında halen daha soru işareti olanlar için paylaşmak istedim. En başından, başlıktaki sorunun cevabını vererek bunun nedenlerini inceleyelim."
categories:
  - "JavaScript"
tags: ["javascript", "asenkron", "senkron"]
license: "All rights reserved"
image: "https://cdn-images-1.medium.com/max/2800/1*r8BeClFn723lR3VZ3yUkvg.png"
featuredImage: "https://cdn-images-1.medium.com/max/2800/1*r8BeClFn723lR3VZ3yUkvg.png"
pageStyle: "normal"
title: "JavaScript: Senkron mu, Asenkron mu?"
subtitle: "JAVASCRIPT’TE ASENKRON YAPILAR"
---

Javascript’teki senkron ve asenkron kavramları başlangıçta kafa karıştırıcı olabiliyor ki ben de bu kafa karışıklığını yaşadım. Bu konudaki işleyişi ve bilinmesi gereken önemli noktaları kafasında halen daha soru işareti olanlar için paylaşmak istedim. En başından, başlıktaki sorunun cevabını vererek bunun nedenlerini inceleyelim.

**Javascript senkron ve single-thread bir dildir**. Yani satır satır çalışır ve bir satır execute edilip işlem tamamlanmadan diğerine geçmez. Örneğin;
```js
alert("Merhaba!!")
console.log("OK'e tıklanmadan bu satıra geçmez")
```

alert() fonksiyonu çalıştığında sayfadaki işleyişi bloklar ve OK’e tıklanmadığı sürece işleyiş devam etmez.

> **Single-thread:** Javascript Engine aynı anda sadece bir iş yapar. C#, Java gibi dillerde oluşturulan Thread yapılarıyla multi-therad çalışma sağlansa da (Parallel veya Multi-thread programming) javascript’te bu mümkün değildir, tüm işlemler tek thread üzerinden yürür ve bu thread de aynı anda sadece bir işlemci çekirdeği üzerinde çalışabilir.

Öte yandan XMLHttpRequest metodları, setTimeOut metodu gibi bazı metodlar bu senkronizasyonu bozar. İşlem sırası bu gibi metodlara geldiğinde işlem başlatılır ve cevap beklenmeden diğer satıra geçilir. Cevap daha sonra callback fonksiyonuyla döner. Bu şekilde senkronizasyonu bozabilen yapılara asenkron yapılar denir. Eee peki, hani Javascript senkron bir dildi ? Konuyu daha iyi anlamak için Javascript’in tarayıcı üzerinde çalışma mantığına değinmemiz gerekli.
> **Callback fonksiyonları**, bir metodun execute edilmesinin ardından yapılacak işlemleri içeren ve ana metoda parametre olarak geçilen fonksiyonlardır. Asenkron yapılarda sıklıkla kullanılır.

Js kodunu execute edebilen bir tarayıcıyı incelediğimizde karşımıza temelde JS Engine, WEB API’lar, Event Loop, Callback Queue yapıları çıkar.

**JS Engine içerisindeki Call Stack adı verilen yapı, js kodlarımızın execute edildiği yerdir.** Yazdığımız js kodu bu yapı içerisinde, çağrılma sırasına göre sıralanır ve bu sıraya göre execute edilir.

```js
const request = new XMLHttpRequest();
request.open ('GET', 'https://datausa.io/api/data?drilldowns=Nation&measures=Population')
request.send();
console.log(request.responseText)

request.addEventListener('load', function(){
 data = JSON.parse(this.responseText)
 console.log(data)
})

console.log("Execute edilecek satır kalmadı")
```

Fakat yukarıdaki örneği çalıştırdığımızda ilgili Ajax çağrısının bu sıraya uymadığını görürüz.
> **AJAX (Asynchronous JavaScript and XML)** 2005 yılında Jesse James Garrett tarafından ortaya atılan, web sayfasının postback yapmadan (yeniden yüklenmeden) güncellenmesini sağlayan ve HTML, XHTML, Javascript, XMLHttpRequest vb. teknolojilerini içeren bir model veya yaklaşımdır.

Kod bloğunun çıktısı aşağıdaki gibidir:
```bash
""                               // 4.satırın çıktısı
"Execute edilecek satır kalmadı" // 11.satırın çıktısı
{ data:
  [
    {
      ID Nation: "01000US",
      ID Year: 2019, Nation: "United States",
      Population: 328239523,
      Slug Nation: "united-states",
      Year: "2019" },
      ...                      // 8.satırın çıktısı
```

“Execute edilecek satır kalmadı” çıktısını son sırada beklerken ilginç bir şekilde 8. satırdaki console.log(data) fonksiyonunun çıktısını aldık. Peki ama neden?

**Ajax çağrıları Js Engine üzerinden değil browser içerisindeki Web API’lar içerisinde yer alan XMLHttpRequest objesi üzeriden gerçekleştirilir.** Ajax request.send() çağrısı Call Stack üzerinden yapılır, bundan sonraki execution XMLHttpRequest yapısı üzerinde, arka palanda devam eder ve Call Stack için bu satır artık execute edilmiş bir satır olduğu için işlem sırası diğer satıra geçer. XMLHttpRequest üzerindeki load işlemi tamamlandığında load EventListener’ına ait olan callback metodu Callback Queue’ya gönderilir ve kuyruğa eklenir.
> **Event Loop** JS Runtime’da Callback’leri koordine eden yapıdır.
> **Callback Queue** farklı yapılardan gelen callback’lerin sıralandığı ve Call Stack’de çalıştırılmak üzere bekletildikleri yapıdır.

Event Loop, Call Stack’de execute edilmesi gereken bir iş kalmadığında Callback Queue’ya bakar ve sırada bir callback fonksiyon çağrısı var ise onu alıp Call Stack’e koyar. Sonuç olarak verinin Ajax ile getirilmesine olayına bağlı olan callback en son yazdırılır. Görsel üzerinden süreci adım adım özetlersek;

![](https://cdn-images-1.medium.com/max/3148/1*VYpQcm8j-xI8s_GzOYIH3Q.png)

1. request.send() metodu çağrılır.

2. XMLHttpRequest üzerinden veri getirme işlemi başlar. Bu sırada Call Stack’in çalışması devam eder ve sırada olan fonksiyon çağrılarını işler. request.ResponseText nesnesi o anda boş olduğu için konsola “” yazılır. En son olarak da “Execute edilecek satır kalmadı” metni konsola yazılır.

3. Veriler başarılı bir şekilde geldikten sonra veri getirme işlemini dinleyen load eventi tetiklenir. XMLHttpRequest, load EventListener’ın üzerinde olan callback’i Callback Queue’ya iletir.

4. Event Loop Call Stack’i kontrol eder. Eğer stack’de sırada bekleyen işlem yoksa Callback Queue’da sırada olan callback’i alır, işlenmesini sağlamak için Call Stack’e koyar ve callback’in içeriği Call Stack’de işlenir (veri konsolda daha iyi okunabilsin diye Json nesnesine çevrilir ve yazdırılır).

**İşleyişe genel olarak bakıldığında asenkronluğu sağlayan etkenin js’nin yapısı olmadığı aksine WEB API’lara yapılan metod çağrılarının olduğu görülmektedir.** Bunun yanında WEB API’ları DOM API, SVG API, Console API, History API, XHTTPRequest, Fetch API vb. bir çok farklı API veya obje oluşturmaktadır ([https://developer.mozilla.org/en-US/docs/Web/API](https://developer.mozilla.org/en-US/docs/Web/API)).

Bu işleyişte gözden kaçırılmaması gereken önemli noktaları sıralarsak:

* Js senkron, single-thread ve blocking bir dildir.

* Asenkron özellikler Web API’lar ile sağlanır ve bu yapılar Call Stack ile asenkron çalışır.

* Hangi metodların asenkron çalışıp çalışmadığı bilinmelidir. **Bir metodun callback dönmesi veya EventListener olması asenkron çalıştığını göstermez. Örneğin Array.map() metodu ve click EventListener’ı callback döndükleri halde senkron çalışırlar.**

Son olarak javascript’te neden asenkron yapılara ihtiyacımız olduğuna değinelim. Örneğin sayfamıza bir çok farklı kaynaktan veri çektiğimizi varsayalım. Bir API’dan data ve farklı bir kaynaktan bir görsel yükleme olsun. Bu işlemlerin asenkron olmadığı bir ortamda sayfamız her işlem için bloklanacak ve ancak bir işlem bittiğinde diğer işleme geçebilecektik. Özellikle günümüzde birçok veri kaynağından beslenen web uygulamalarını göz önüne aldığımızda, veri yüklemesinin gecikebileceği durumlarda önemli lag problemleri ile karşı karşıya kalabilirdik. Bu noktada asenkron yapılar imdadımıza yetişerek uygulamadaki diğer akışları bozmadan, asenkron bir şekilde işlem yapmamıza olanak sağlar.
