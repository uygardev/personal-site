---
date: 2023-01-02T01:53:42+03:00
authors: [Uygar]
description: "Javascript, çoğunlukla adı karıştırılan bir programlama dili. Java ile bağının olup olmadığı genelde akıllara gelen bir soru."
categories:
  - "JavaScript"
tags: ["javascript", "ecmascript", "tc39"]
license: "All rights reserved"
image: "https://cdn-images-1.medium.com/max/2800/1*hSArHJ0DzKDgCraPkGaYKA.png"
featuredImage: "https://cdn-images-1.medium.com/max/2800/1*hSArHJ0DzKDgCraPkGaYKA.png"
pageStyle: "normal"
title: "JavaScript mi, ECMAScript mi?"
subtitle: "Mocha, LiveScript, JavaScript, ECMAScript"
---

Javascript, çoğunlukla adı karıştırılan bir programlama dili. Java ile bağının olup olmadığı genelde akıllara gelen bir soru. Ayrıca bir de ECMAScript var. Şimdi gelin bu kavramlara bir açıklık getirelim.

**Brendan Eic** bu dili ilk tasarladığında kod adı Mocha’ydı. Sonrasında dil Netscape markasına dahil olarak LiveScript adını aldı. Resmi olarak açıklandığında ise JavaScript adı tercih edildi. Çünkü dilin hedef kitlesi Java programcılarıydı. Script kelimesi ise bu zamanlarda **“lightweight”** programları ifade etmesi açısından popülerdi. Özetle **JavaScript adının seçimi, bu yeni dili zamanının ağır ve çokça kullanılan Java diline karşı iyi bir alternatif olarak göstermek için yapılan bir pazarlama taktiğiydi.**

Günümüzde ise dilin resmi adı JS’yi yöneten TC39 adlı teknik yönetim komitesi tarafından belirtilen ve ECMA standartlar kuruluşu tarafından (JS’nin yazım kuralları ve davranışları ECMA tarafından tanımlanır) resmileştirilen ECMAScript’tir. 2015'den beri ise bu isimle beraber revizyon yılı veya spesifikayon/revizyon numaraları kullanılmaya başlanmıştır. Örneğin şu an resmi olarak ES2022 veya diğer adıyla ECMAScript 13th Edition, 2022 yılı Temmuz ayında yayınlanmış bulunmaktadır (https://www.wikiwand.com/en/ECMAScript).
> **TC39 Komitesi:** Bu komite, tarayıcı üreticileri (Mozilla, Google, Apple) ve cihaz üreticileri (Samsung, vb.) gibi web yatırımı yapan şirketlerden gelen 50–100 farklı kişiden oluşur. Komitenin tüm üyeleri gönüllüdür.

### **Her yıl yayınlanan bu standartlar nasıl belirlenir ve değişim kararları nasıl alınır?**

TC39 komitesi üyeleri, üzerinde anlaşmaya varılan değişiklikleri konuşmak için toplanırlar ve sürecin sonunda bu değişiklik tekliflerini ECMA’ya sunarlar.

TC39, son toplantıdan bu yana üyelerin yaptığı çalışmaları gözden geçirmek, sorunları tartışmak ve teklifleri oylamak için genellikle iki ayda bir, yaklaşık üç gün sürecek bir toplantı için bir araya gelirler. Teklif ancak bir TC39 üyesi tarafından sunabilir. Bu üye tekliften önce, teklif üzerinde gayrı-resmi olarak (blog yazıları, sosyal medya) çalışmış olmalıdır. Verilen teklifler kabul edilene kadar beş aşamalı bir süreçte ilerler. Stage 0 aşamasında teklif sunulur ve teklifin kabul edilip yeni ES spresifikasyonuna eklenmesi için Stage 4'ü geçmesi gerekir. Teklifin geçmesi birkaç ay ya da birkaç yıl sürebilir.

Tüm bu teklifler TC39'un public olan reposu üzerinden yürütülür ([https://github.com/tc39/proposals](https://github.com/tc39/proposals)). TC39 üyesi olsun ya da olmasın herkes bu repo üzerinden tartışmalara ve teklifler üzerinde çalışma süreçlerine katılabilir, fakat yanlızca TC39 üyeleri toplantılara katılma ve teklifler üzerinde oy kullanma hakkına sahiptir. Tüm aşamaları geçen teklifler ECMA’ya sunulur ve tüm tarayıcı ve cihaz üreticileri kendi JS implementasyonlarını (Örn: JS Motorları, Node.js vb.) bu tekliflerin kabulu sonucu ortaya çıkan yeni revizyona uyumlu tutmayı taahhüt etmiştir.


