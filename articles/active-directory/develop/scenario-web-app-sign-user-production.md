---
title: Kullanıcılar için oturum açan Web uygulamasını üretime taşıma | Mavisi
titleSuffix: Microsoft identity platform
description: Kullanıcılara oturum açan bir Web uygulaması oluşturmayı öğrenin (üretime geçin)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: e4a47112d2f66edc8af9b7f100d48bc205f2e85e
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99584306"
---
# <a name="web-app-that-signs-in-users-move-to-production"></a>Kullanıcılara oturum açan Web uygulaması: üretime taşı

Artık, Web API 'Lerini çağırmaya yönelik bir belirteç almayı öğrenmiş olduğunuza göre, uygulamanızı üretime taşırken göz önünde bulundurmanız gereken bazı şeyler aşağıda verilmiştir.

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="troubleshooting"></a>Sorun giderme
Kullanıcılar Web uygulamasında ilk kez oturum açtığında, onaylaması gerekir. Ancak, bazı kuruluşlarda, kullanıcılar aşağıdaki gibi bir ileti görebilir: *AppName, kuruluşunuzda yalnızca bir yöneticinin verebileceği kaynaklara erişmek için izinlere ihtiyaç duyuyor olabilir. Bu uygulamayı kullanabilmeniz için lütfen bir yöneticiye bu uygulamaya izin vermesini isteyin.*
Bunun nedeni, kiracı yöneticinizin kullanıcıların onay iznini **devre dışı** bırakmış olması. Bu durumda, kiracı yöneticilerinizle iletişime geçerek uygulamanın gerektirdiği kapsamlar için yönetici onayı oluşturmaları gerekir.

## <a name="same-site"></a>Aynı site

Chrome tarayıcısının yeni sürümleriyle ilgili olası sorunları anladığınızdan emin olun: [Chrome tarayıcısında SameSite tanımlama bilgisi değişikliklerini işleme](howto-handle-samesite-cookie-changes-chrome-browser.md).

Microsoft. Identity. Web NuGet paketi en yaygın SameSite sorunlarını işler.

## <a name="deep-dive-aspnet-core-web-app-tutorial"></a>Derin bakış: ASP.NET Core Web uygulaması öğreticisi

Bu ASP.NET Core öğreticisiyle kullanıcıların oturum açma yolları hakkında bilgi edinin: 

[Web uygulamalarınızın kullanıcılara oturum açmasını ve geliştiriciler için Microsoft Identity platformu ile API 'Leri çağırmasını sağlama](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial)

Bu aşamalı öğreticide, bir Web uygulaması için, ' deki hesaplara oturum açma ekleme dahil olmak üzere üretime hazırlı kod vardır:

- Kuruluşunuz
- Birden çok kuruluş
- İş veya okul hesapları ya da kişisel Microsoft hesapları
- [Azure AD B2C](../../active-directory-b2c/overview.md)
- Ulusal bulutlar

## <a name="sample-code-java-web-app"></a>Örnek kod: Java Web uygulaması

GitHub 'da bu örnekten Java Web uygulaması hakkında daha fazla bilgi edinin: 

[Kullanıcılara Microsoft Identity platformu ve çağrıları ile oturum açan bir Java Web uygulaması Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp)

## <a name="next-steps"></a>Sonraki Adımlar

Web uygulamanız kullanıcılara kaydolduktan sonra, oturum açmış kullanıcılar adına Web API 'Leri çağırabilir. Web API 'Lerinden Web API 'Leri çağırmak şu senaryonun nesnesidir: Web [API 'lerini çağıran Web uygulaması](scenario-web-app-call-api-overview.md).