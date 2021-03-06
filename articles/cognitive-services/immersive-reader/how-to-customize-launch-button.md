---
title: Tam Ekran Okuyucu’yu Özelleştir düğmesi
titleSuffix: Azure Cognitive Services
description: Bu makalede, tam ekran okuyucuyu Başlatan düğmeyi nasıl özelleştireceğiniz gösterilmektedir.
services: cognitive-services
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: metang
ms.openlocfilehash: b24a9dcb7a369708b1374f367186dc4d9a6ce475
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2020
ms.locfileid: "93146884"
---
# <a name="how-to-customize-the-immersive-reader-button"></a>Tam ekran okuyucu düğmesini özelleştirme

Bu makalede, tam ekran okuyucuyu uygulamanızın ihtiyaçlarına uyacak şekilde Başlatan düğmenin nasıl özelleştirileceği gösterilmektedir.

## <a name="add-the-immersive-reader-button"></a>Derinlikli okuyucu düğmesini ekleme

Modern Okuyucu SDK 'Sı, tam ekran okuyucuyu başlatan düğme için varsayılan stil sağlar. `immersive-reader-button`Bu stillendirme özelliğini etkinleştirmek için Class özniteliğini kullanın.

```html
<div class='immersive-reader-button'></div>
```

## <a name="customize-the-button-style"></a>Düğme stilini özelleştirme

`data-button-style`Düğmenin stilini ayarlamak için özniteliğini kullanın. İzin verilen değerler, `icon` , `text` ve `iconAndText` . `icon` varsayılan değerdir.

### <a name="icon-button"></a>Simge düğmesi

```html
<div class='immersive-reader-button' data-button-style='icon'></div>
```

Bu, aşağıdakileri işler:

![Bu, işlenmiş metin düğmesidir](./media/button-icon.png)

### <a name="text-button"></a>Metin düğmesi

```html
<div class='immersive-reader-button' data-button-style='text'></div>
```

Bu, aşağıdakileri işler:

![Bu, işlenmiş derinlikli okuyucu düğmesidir.](./media/button-text.png)

### <a name="icon-and-text-button"></a>Simge ve metin düğmesi

```html
<div class='immersive-reader-button' data-button-style='iconAndText'></div>
```

Bu, aşağıdakileri işler:

![Simge düğmesi](./media/button-icon-and-text.png)

## <a name="customize-the-button-text"></a>Düğme metnini özelleştirme

Özniteliğini ve düğmesini kullanarak düğme için alternatif metni yapılandırın `data-locale` . İngilizce varsayılan dildir.

```html
<div class='immersive-reader-button' data-locale='fr-FR'></div>
```

## <a name="customize-the-size-of-the-icon"></a>Simgenin boyutunu özelleştirme

Derinlikli okuyucu simgesinin boyutu özniteliği kullanılarak yapılandırılabilir `data-icon-px-size` . Bu, simgenin boyutunu piksel cinsinden ayarlar. Varsayılan boyut 20 px ' dir.

```html
<div class='immersive-reader-button' data-icon-px-size='50'></div>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tam ekran okuyucu SDK başvurusunu](./reference.md) keşfet