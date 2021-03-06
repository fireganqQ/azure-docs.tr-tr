---
title: Dağıtım mimarisi-Azure ayrılmış HSM | Microsoft Docs
description: Uygulama mimarisinin bir parçası olarak Azure ayrılmış HSM kullanırken temel tasarım konuları
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 02/05/2020
ms.author: mbaldwin
ms.openlocfilehash: 6a0767b077886337331f24b15715247006f3fe2c
ms.sourcegitcommit: 230d5656b525a2c6a6717525b68a10135c568d67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94888904"
---
# <a name="azure-dedicated-hsm-deployment-architecture"></a>Azure Ayrılmış HSM dağıtım mimarisi

Azure adanmış HSM, Azure 'da şifreleme anahtar depolaması sağlar. Sıkı güvenlik gereksinimlerini karşılar. Müşteriler, şu durumlarda Azure ayrılmış HSM 'yi kullanmaya yarar olacaktır:

* FIPS 140-2 düzey 3 sertifikalarıyla Buluşmalıdır
* HSM 'ye özel erişime sahip olmaları gerekir
* cihazların tamamen denetimine sahip olması gerekir

HSM 'ler, Microsoft 'un veri merkezlerinde dağıtılır ve yüksek oranda kullanılabilir bir çözümün temeli olarak kolayca bir cihaz çifti olarak sağlanabilir. Ayrıca, olağanüstü durum dayanıklı bir çözüm için bölgeler arasında da dağıtılabilir. Ayrılmış HSM 'ye sahip bölgeler Şu anda, [bölgelere göre ürünler sayfası](https://azure.microsoft.com/global-infrastructure/services/?products=azure-dedicated-hsm)kullanılarak denetlenebilir. 

Bölgelerin her birinde iki bağımsız veri merkezinde veya en az iki bağımsız kullanılabilirlik alanında dağıtılan HSM rafları vardır. Örneğin, Güney Doğu Asya üç kullanılabilirlik bölgesine sahiptir ve Doğu ABD 2 iki tane vardır. Avrupa, Asya ve ABD genelinde toplam sekiz bölge mevcuttur. Bu, yeni bölgelerde yeni HSM raflarını eklediğimiz için bu değişiklikleri sunmaktadır. Azure bölgeleri hakkında daha fazla bilgi için bkz. resmi  [Azure bölgeleri bilgileri](https://azure.microsoft.com/global-infrastructure/regions/).
Tüm özel HSM tabanlı çözümler için bazı tasarım faktörleri konum/gecikme, yüksek kullanılabilirlik ve diğer dağıtılmış uygulamalar için destek.

## <a name="device-location"></a>Cihaz konumu

En iyi HSM cihaz konumu, şifreleme işlemleri gerçekleştiren uygulamalara en yakın yakındır. Bölge içi gecikme süresinin tek basamaklı milisaniye olması beklenir. Çapraz bölge gecikmesi bundan daha yüksek 5 ila 10 kat olabilir.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Yüksek kullanılabilirlik elde etmek için bir müşterinin yüksek kullanılabilirlik çifti olarak II yazılımı kullanılarak yapılandırılmış bir bölgede iki HSM cihazı kullanması gerekir. Bu dağıtım türü, tek bir cihaz, anahtar işlemlerini işlemesini önlemek için bir sorun yaşadığında anahtarların kullanılabilirliğini sağlar. Ayrıca güç kaynağı değişikliği gibi onarım/çözme bakım işlemi gerçekleştirirken riski önemli ölçüde azaltır. Her türlü bölgesel düzey hata için bir tasarımın hesaba göre olması önemlidir. Bölgesel düzey arızalar, acericanes, floods veya deprem gibi doğal felaketler olduğunda meydana gelebilir. Bu tür olaylar, başka bir bölgedeki HSM cihazları sağlanarak azaltılmalıdır. Başka bir bölgede dağıtılan cihazlar Thales yazılım yapılandırması aracılığıyla birlikte eşleştirilebilir. Bu, yüksek oranda kullanılabilir ve olağanüstü durum dayanıklı bir çözüm için en düşük dağıtımın iki bölgede dört HSM cihazı olduğu anlamına gelir. Bölgeler arasında yerel artıklığı ve artıklık, gecikme süresi, kapasiteyi desteklemek veya uygulamaya özel diğer gereksinimleri karşılamak için başka bir HSM cihaz dağıtımı eklemek için temel olarak kullanılabilir.

## <a name="distributed-application-support"></a>Dağıtılmış uygulama desteği

Adanmış HSM cihazları genellikle anahtar depolama ve anahtar alma işlemleri gerçekleştirmesi gereken uygulamalar desteğiyle dağıtılır. Adanmış HSM cihazlarının bağımsız uygulama desteği için 10 bölümü vardır. Cihaz konumu, hizmeti kullanması gereken tüm uygulamaların bütünsel görünümünü temel almalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım mimarisi belirlendikten sonra, bu mimariyi uygulamak için gereken yapılandırma etkinliklerinin çoğu Thales tarafından sağlanacaktır. Buna cihaz yapılandırması ve uygulama tümleştirme senaryoları da dahildir. Daha fazla bilgi için [Thales müşteri destek](https://supportportal.gemalto.com/csm/) portalını kullanın ve yönetim ve yapılandırma kılavuzlarını indirin. Microsoft iş ortağı sitesinin çeşitli Tümleştirme kılavuzu vardır.
Hizmetin yüksek kullanılabilirlik ve güvenlik gibi tüm temel kavramlarının cihaz sağlama veya uygulama tasarımı ve dağıtımdan önce iyi anlaşıldığından emin olmanız önerilir.
Daha fazla kavram düzeyi konuları:

* [Yüksek kullanılabilirlik](high-availability.md)
* [Fiziksel Güvenlik](physical-security.md)
* [Ağ](networking.md)
* [Desteklenebilirlik](supportability.md)
* [İzleme](monitoring.md)
