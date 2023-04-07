---
date: 2023-03-14T01:53:42+03:00
authors: [Uygar]
description: "Javascript'te deklare ettiğimiz her değişken javascript değer evreninde bir değere sahip olur. Peki bu değerlerin tip kontrolü mekanizması nasıl çalışır?"
categories:
  - "TypeScript"
tags: ["typescript", "nodejs", "path alias", "relative path"]
license: "All rights reserved"
image: "https://cdn-images-1.medium.com/max/2800/1*GsCozmG32VFSCK7O11w8qg.jpeg"
featuredImage: "https://cdn-images-1.medium.com/max/2800/1*GsCozmG32VFSCK7O11w8qg.jpeg"
pageStyle: "normal"
title: "TypeScript: Node.js ile Path Alias Kullanımı"
subtitle: "Path Alias, module-alias"
---

Codebase büyüdüğünde ve kod yapısı derinleştiğinde .ts dosyalarında yaptığımız import’ların relative path’leri uzamaya başlar.
```typescript
import { MicrosoftRightAdapter } from "../../../../../adapters/right/infrastructure/api/microsoft/right.adapter.microsoft";
```
Bu durum kod okunabilirliğini zorlaştırmanın yanında kodu dosya sistemi yapısına bağlı hale getirir. Ayrıca relative path’ler dosya sistemi üzerinden arama yapılmasına sebep olduğu için büyük code base’lerde performans sorunu yaratabilir.

Relative path yerine path alias kullandığımızda import’umuz aşağıdaki şekilde olacaktır.
```typescript
import { MicrosoftRightAdapter } from "@/adapters/right/infrastructure/api/microsoft/right.adapter.microsoft";
```
Hem typescript’te hem de /dist klasöründe bulunan compile edilmiş js dosyalarındaki @ ile başlayan path’lerin çözümlenebilmesi için ufak bir iki işlem yapmamız yeterli olacak.

Öncelikle typescipt’teki çözümleme için tsconfig’imize aşağıdaki satırları ekliyoruz. baseUrl’imizi proje ana klasörü olarak tanımlayıp @ ile başlayan pathlerin src içerisinde aranacak şekilde çözümlenmesini sağlıyoruz.
```json
// tsconfig.json
{
  "compilerOptions": {
    ...
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  },
    ...
}
```
Sonrasında /dist klasörü altındaki compile edilmiş js dosyarındaki @ ile başlayan pathlerin çözümlenebilmesini sağlayacak olan *module-alias* paketini kuruyoruz.
```bash
npm i --save module-alias
```
Bu paket için alias kuralı tanımlıyoruz. @ ile başlayan javascript modül pathlerinin dist klasörü üzerinden çözümlenmesi gerektiğini belirtiyoruz.
```json
// package.json
{
  ...
  "_moduleAliases": {
      "@": "dist"
  },
  ....
}
```
Son olarak src içerindeki ana index.ts’mizin içerisine bu paketi import ediyoruz.
```typescript
// src/index.ts
import "module-alias/register";
...
```
Ve birkaç ufak işlemle kodumuzu daha temiz ve okunabilir hale getirmiş olduk.
