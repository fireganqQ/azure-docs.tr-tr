---
title: Kimlik doğrulama belirteçlerini Azure Media Services geçir | Microsoft Docs
description: İstemciden Azure Media Services anahtar teslim hizmetine kimlik doğrulama belirteçleri gönderme hakkında bilgi edinin
services: media-services
keywords: içerik koruma, DRM, belirteç kimlik doğrulaması
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.custom: devx-track-csharp
ms.openlocfilehash: 498e6d9e38468b126dbe10e0d0f59f54f1816ff0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89261053"
---
# <a name="learn-how-clients-pass-tokens-to-the-azure-media-services-key-delivery-service"></a>İstemcilerin belirteçleri Azure Media Services anahtar teslim hizmetine nasıl ilettireceğinizi öğrenin

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Müşteriler çoğu zaman Player 'ın anahtarı edinebilmesi için Azure Media Services anahtar teslim hizmetine belirteçleri nasıl geçirebilmesini ister. Media Services basit Web belirteci (SWT) ve JSON Web Token (JWT) biçimlerini destekler. Belirteç kimlik doğrulaması, sistemde ortak şifreleme veya Gelişmiş Şifreleme Standardı (AES) zarf şifrelemesi kullanıp kullanmayacağınızı bağımsız olarak herhangi bir anahtar türüne uygulanır.

 Hedeflediğiniz yürütücüye ve platforma bağlı olarak, aşağıdaki yollarla belirteci oyununuza geçirebilirsiniz:

- HTTP yetkilendirme üst bilgisi aracılığıyla.
    > [!NOTE]
    > "Taşıyıcı" ön eki, OAuth 2,0 özelliklerine göre beklenmektedir. Belirteç yapılandırmasına sahip bir örnek oynatıcı Azure Media Player [demo sayfasında](https://ampdemo.azureedge.net/)barındırılır. Video kaynağını ayarlamak için **AES (JWT belirteci)** veya **AES (SWT belirteci)** öğesini seçin. Belirteç, yetkilendirme üst bilgisi aracılığıyla geçirilir.

- "Token = tokenvalue" ile bir URL sorgu parametresi ekleme.  
    > [!NOTE]
    > "Taşıyıcı" ön eki beklenmiyor. Belirteç bir URL aracılığıyla gönderildiğinden, belirteç dizesini koruma sağlamak gerekir. Burada nasıl yapılacağını gösteren bir C# örnek kodu verilmiştir:

    ```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
    ```

- CustomData alanı aracılığıyla.
Bu seçenek, yalnızca PlayReady lisans alma sınaması 'nın CustomData alanı aracılığıyla PlayReady lisans alımı için kullanılır. Bu durumda, belirtecin XML belgesinin içinde burada açıklandığı gibi olması gerekir:

    ```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
    ```
    Kimlik doğrulama belirtecinizi belirteç öğesine yerleştirin.

- Alternatif bir HTTP Canlı Akışı (HLS) çalma listesi aracılığıyla. İOS/Safari 'de AES + HLS oynatma için belirteç kimlik doğrulamasını yapılandırmanız gerekiyorsa, doğrudan belirtece gönderebilmeniz için bir yol yoktur. Bu senaryoyu etkinleştirmek için çalma listesini değiştirme hakkında daha fazla bilgi için bu [blog gönderisine](https://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]
