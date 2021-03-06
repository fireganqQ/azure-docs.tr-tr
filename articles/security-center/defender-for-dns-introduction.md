---
title: DNS için Azure Defender-avantajlar ve Özellikler
description: DNS için Azure Defender 'ın avantajları ve özellikleri hakkında bilgi edinin
author: memildin
ms.author: memildin
ms.date: 12/07/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 957e39f7629337182c3e19a1a514c42883666301
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98796993"
---
# <a name="introduction-to-azure-defender-for-dns"></a>DNS için Azure Defender 'a giriş

[Azure DNS](../dns/dns-overview.md) , Microsoft Azure altyapısını kullanarak ad ÇÖZÜMLEMESI sağlayan DNS etki alanları için bir barındırma hizmetidir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.

DNS için Azure Defender, bulut kaynaklarınız için ek bir koruma katmanı sağlar:

- Azure kaynaklarınızdaki tüm DNS sorgularını sürekli olarak izleme
- şüpheli etkinlik konusunda sizi uyarmak için gelişmiş güvenlik analizlerini çalıştırma

## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Önizleme<br>[!INCLUDE [Legalese](../../includes/security-center-preview-legal-text.md)] |
|Fiyat|**DNS Için Azure Defender** , [fiyatlandırma sayfasında](security-center-pricing.md) gösterildiği gibi faturalandırılır|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Hayır](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||

## <a name="what-are-the-benefits-of-azure-defender-for-dns"></a>DNS için Azure Defender 'ın avantajları nelerdir?

DNS için Azure Defender, aşağıdakiler dahil olmak üzere sorunları önler:

- DNS tüneli kullanarak Azure kaynaklarınızdan veri kaybı
- C&C sunucusuyla iletişim kuran kötü amaçlı yazılım
- Kötü amaçlı etki alanlarıyla kimlik avı ve şifre araştırma olarak iletişim
- DNS saldırıları-kötü amaçlı DNS çözümleyicilerine sahip iletişim 

DNS için Azure Defender tarafından sunulan uyarıların tam listesi, [Uyarılar başvurusu sayfasında](alerts-reference.md#alerts-dns)bulunur.

## <a name="dependencies"></a>Bağımlılıklar

DNS için Azure Defender hiçbir aracı kullanmaz. 

DNS katmanınızı korumak için [Azure Defender 'ı etkinleştirme](security-center-pricing.md#enable-azure-defender)bölümünde anlatıldığı şekilde aboneliklerinizin her biri Için Azure Defender 'ı etkinleştirin.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, DNS için Azure Defender hakkında bilgi edindiniz. İlgili malzemeler için aşağıdaki makaleye bakın: 

- Güvenlik uyarıları Güvenlik Merkezi tarafından üretilebilir veya Güvenlik Merkezi tarafından farklı güvenlik ürünlerinden alınabilir. Bu uyarıların tümünü Azure Sentinel 'e, herhangi bir üçüncü taraf SıEM 'e veya herhangi bir harici araca aktarmak için [uyarıları BIR SıEM 'ye aktarma](continuous-export.md)konusundaki yönergeleri izleyin.

- > [!div class="nextstepaction"]
    > [Azure Defender’ı etkinleştirme](security-center-pricing.md#enable-azure-defender)