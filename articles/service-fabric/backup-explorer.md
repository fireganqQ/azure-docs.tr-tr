---
title: Güvenilir koleksiyonlar yedeklemesini yerel olarak okuma ve güncelleştirme
description: Yerel güvenilir koleksiyonlar yedeklemesini okumak ve güncelleştirmek için Azure Service Fabric yedekleme Gezgini 'ni kullanın.
author: athinanthny
ms.topic: conceptual
ms.date: 07/01/2020
ms.author: atsenthi
ms.openlocfilehash: 1da70c37c8a6ed93e7abe1b5d329e808c592e43a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86034990"
---
# <a name="read-and-update-a-reliable-collections-backup-by-using-backup-explorer"></a>Yedek gezgin kullanarak güvenilir koleksiyonlar yedeklemesini okuma ve güncelleştirme

Azure Service Fabric yedekleme Gezgini, Service Fabric güvenilir koleksiyonlardaki veriler bozuksa veri düzeltmesine yardımcı olur. Verilerin geçerli durumu, bir uygulamada tanıtılan herhangi bir hata veya canlı bir kümede yapılan yanlış girişler tarafından bozulabilir.

Yedekleme Gezgini yardımı ile aşağıdaki görevleri gerçekleştirebilirsiniz:
-   Koleksiyonun meta verilerini sorgulayın.
-   Geçerli durumu ve onun girdilerini yüklenen yedekleme koleksiyonunda görüntüleyin.
-   Son denetim noktasından bu yana gerçekleştirilen işlemleri listeleyin.
-   Koleksiyondaki girişleri ekleyerek, güncelleştirerek veya silerek koleksiyonu güncelleştirin.
-   Güncelleştirilmiş durumu kullanarak yeni bir yedekleme yapın.

> [!NOTE]
> Service Fabric yedekleme Gezgini Şu anda yalnızca Yedeklemedeki güvenilir koleksiyonları görüntülemeyi ve düzenlemesini desteklemektedir.
>

## <a name="access-the-backup"></a>Yedeklemeye erişin

Service Fabric yedekleme Gezgini, yedeklemedeki güvenilir koleksiyonları görüntülemek veya güncelleştirmek için aşağıdaki yollarla kullanılabilir:
-   **İkili**: güvenilir koleksiyonları görüntülemek ve değiştirmek Için bir NuGet paketi kullanın.
-   **Http/Rest**: güvenilir koleksiyonları görüntülemek ve DEğIşTIRMEk için HTTP tabanlı bir rest sunucusu kullanın.
-   **bkpctl**: güvenilir koleksiyonlar yedeklemesini görüntülemek ve değiştirmek Için Service Fabric yedekleme Gezgini komut satırı ARABIRIMINI (CLI) kullanın.

Yedekleme Gezgini, gelişmiş kullanıcılar için bir C# kitaplığı içerir. Müşterilerin durum Yöneticisi ile, mevcut durum bilgisi olan hizmetlerinde çalışmasına benzer şekilde, güvenilir Koleksiyonlar ile çalışmak için doğrudan uygulamanın kitaplığını kullanabilirsiniz. Temel kullanıcılar ve temel bir kullanım durumunda, Gezgin, yedeklemeleri denetlemek için API 'Leri kullanıma sunan tek başına REST sunucusuna da sahiptir. Bkpctl CLı aracı REST API 'lerinin üstünde çalışmaktadır ve Python 'u temel alır. Yedeklemeleri okumak ve güncelleştirmek ve komut satırı aracılığıyla yeni yedeklemeler almak için CLı aracını kullanabilirsiniz.

Daha fazla bilgi için bkz. [yedekleme Gezgini](https://github.com/microsoft/service-fabric-backup-explorer) GitHub deposu Service Fabric. Depo, kaynak ve sürüm bilgilerini ve kurulum yönergelerini içerir.

Ayrıca depoyu yerel olarak oluşturabilir ve yedeklemeler üzerinde çalışabilirsiniz.
 
Yedekleme Gezgini için NuGet (Microsoft. ServiceFabric. ReliableCollectionBackup. Parser), [NuGet.org](https://www.nuget.org/)' de kullanıma sunulacaktır. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Service Fabric durum bilgisi olan hizmetlerde güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)hakkında daha fazla bilgi edinin.
* [En iyi Service Fabric uygulamaları](service-fabric-best-practices-overview.md)inceleyin.
