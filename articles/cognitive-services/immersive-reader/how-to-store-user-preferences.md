---
title: Kullanıcı tercihlerini depolama
titleSuffix: Azure Cognitive Services
description: Bu makalede, kullanıcının tercihlerini nasıl depolayacaksınız.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: metang
ms.openlocfilehash: 31c1ef8d75b4c12e4dd6a360852feb27857ac412
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92636553"
---
# <a name="how-to-store-user-preferences"></a>Kullanıcı tercihlerini depolama

Bu makalede, Kullanıcı **tercihleri** olarak BILINEN Kullanıcı arabirimi ayarlarının, [-Preferences](./reference.md#options) ve [-onpreferenceschanged](./reference.md#options) derinlikli Okuyucu SDK seçenekleri aracılığıyla nasıl depolandığı gösterilmektedir.

Tanýmlama [ıepolicy](./reference.md#cookiepolicy-options) SDK seçeneği *etkin* olarak ayarlandığında, derinlikli okuyucu uygulaması **Kullanıcı tercihlerini** (metin boyutu, Tema rengi, yazı tipi vb.), belirli bir tarayıcı ve cihaza yerel olan tanımlama bilgilerinde depolar. Kullanıcı her ne kadar modern okuyucuyu aynı tarayıcı ve cihazda başlattığında, kullanıcının bu cihazdaki son oturumlarından gelen tercihleriyle birlikte açılır. Ancak Kullanıcı, modern okuyucuyu farklı bir tarayıcı veya cihazda açarsa, ayarlar başlangıçta tam ekran okuyucusu varsayılan ayarlarıyla yapılandırılır ve kullanıcının, kullandıkları her cihaz için tercihlerini yeniden ayarlaması gerekir. `-preferences`Ve `-onPreferencesChanged` modern Okuyucu SDK seçenekleri, uygulamaların, uygulamayı kullandıkları her yerde tutarlı bir deneyim sunacak şekilde, kullanıcıların çeşitli tarayıcılarda ve cihazlarda bir kullanıcının tercihlerini dolaşımını sağlayan bir yol sağlar.

İlk olarak, `-onPreferencesChanged` derinlikli okuyucu uygulamasını başlatırken geri arama SDK seçeneğini sağlayarak, tam ekran okuyucu `-preferences` oturumu sırasında Kullanıcı tercihlerini her değiştirilişinde, derinlikli okuyucu ana bilgisayar uygulamasına bir dize gönderir. Ana bilgisayar uygulaması, Kullanıcı tercihlerini kendi sistemlerinde depolamaktan sorumludur. Ardından, aynı kullanıcı tam ekran okuyucuyu yeniden başlattığında, ana bilgisayar uygulaması bu kullanıcının tercihlerini depolama alanından alabilir ve `-preferences` Kullanıcı tercihleri geri yüklendikten sonra bunları, derinlikli okuyucu uygulamasını başlatırken DIZE SDK seçeneği olarak sağlayabilir.

Bu işlevsellik, tanımlama bilgilerinin kullanılması istenmediği veya uygun olmadığı durumlarda **Kullanıcı tercihlerini** depolamanın alternatif bir yolu olarak kullanılabilir.

> [!CAUTION]
> **Önemli** Derinlikli okuyucu uygulamasına gönderilen dize değerlerini programlı olarak değiştirmeyi denemeyin, `-preferences` çünkü bu durum, müşterileriniz için daha fazla kullanıcı deneyimi oluşmasına neden olabilir. Konak uygulamalar asla bir özel değer atamamalıdır veya `-preferences` dizeyi işleyebilir. `-preferences`Dize seçeneğini kullanırken, yalnızca geri çağırma seçeneğinden döndürülen tam değeri kullanın `-onPreferencesChanged` .

## <a name="how-to-enable-storing-user-preferences"></a>Kullanıcı tercihlerini depolamayı etkinleştirme

Tam ekran okuyucu SDK [launchAsync](./reference.md#launchasync) `options` parametresi `-onPreferencesChanged` geri çağırma içerir. Bu işlev, Kullanıcı tercihlerini değiştirdiğinde her zaman çağrılacaktır. `value`Parametresi, kullanıcının geçerli tercihlerini temsil eden bir dize içerir. Daha sonra bu dize, bu kullanıcı için konak uygulama tarafından depolanır.

```typescript
const options = {
    onPreferencesChanged: (value: string) => {
        // Store user preferences here
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="how-to-load-user-preferences-into-the-immersive-reader"></a>Kullanıcı tercihlerini tam ekran okuyucusuna yükleme

Seçeneğini kullanarak kullanıcının tercihlerini derinlikli okuyucuya geçirin `-preferences` . Kullanıcı tercihlerini depolamak ve yüklemek için önemsiz bir örnek aşağıdaki gibidir:

```typescript
const storedUserPreferences = localStorage.getItem("USER_PREFERENCES");
let userPreferences = storedUserPreferences === null ? null : storedUserPreferences;
const options = {
    preferences: userPreferences,
    onPreferencesChanged: (value: string) => {
        userPreferences = value;
        localStorage.setItem("USER_PREFERENCES", userPreferences);
    }
};
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tam ekran okuyucu SDK başvurusunu](./reference.md) keşfet