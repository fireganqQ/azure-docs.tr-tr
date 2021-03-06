---
title: Azure geçişi ile Azure 'a geçiş için çok sayıda fiziksel sunucuyu değerlendirin | Microsoft Docs
description: Azure geçişi hizmeti kullanılarak Azure 'a geçiş için çok sayıda fiziksel sunucuyu nasıl değerlendireceğinizi açıklar.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 01/19/2020
ms.openlocfilehash: 45954a42613fb1716e3b66305c0485a3966b8e37
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96753697"
---
# <a name="assess-large-numbers-of-physical-servers-for-migration-to-azure"></a>Azure 'a geçiş için çok sayıda fiziksel sunucuyu değerlendirin

Bu makalede, Azure geçişi sunucu değerlendirmesi Aracı kullanılarak Azure 'a geçiş için çok sayıda şirket içi fiziksel sunucuyu nasıl değerlendireceğiniz açıklanır.

[Azure geçişi](migrate-services-overview.md) , Microsoft Azure için uygulamaları, altyapıyı ve iş yüklerini keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan araçların merkezini sağlar. Hub, Azure geçiş araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir. 


Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
> * Ölçek ölçeğinde değerlendirme planlayın.
> * Azure izinlerini yapılandırın ve fiziksel sunucuları değerlendirme için hazırlayın.
> * Bir Azure geçişi projesi oluşturun ve bir değerlendirme oluşturun.
> * Taşımayı planlarken değerlendirmesi gözden geçirin.


> [!NOTE]
> Ölçeği değerlendirmek için birkaç sunucuyu değerlendirmek üzere bir kavram kanıtı denemek istiyorsanız, [öğretici serimizi](./tutorial-discover-physical.md)izleyin.

## <a name="plan-for-assessment"></a>Değerlendirme planı

Çok sayıda fiziksel sunucu değerlendirmesi için planlama yaparken, göz önünde bulundurmanız gereken birkaç nokta vardır:

- **Azure geçişi projelerini planlayın**: Azure geçişi projelerinin nasıl dağıtılacağını öğrenin. Örneğin, Veri merkezleriniz farklı coğrafi bölgelerde ise ya da bulma, değerlendirme veya geçişle ilgili meta verileri farklı bir Coğrafya 'da depolamanız gerekirse, birden çok proje gerekebilir.
- **Gereçler planı**: Azure geçişi, bir Windows makinesinde dağıtılan ve şirket Içi Azure geçiş gereci kullanarak, değerlendirme ve geçiş için sunucuları sürekli olarak bulur. Gereç, VM 'Leri, diskleri veya ağ bağdaştırıcılarını ekleme gibi ortam değişikliklerini izler. Ayrıca, Azure 'a bunlarla ilgili meta veriler ve performans verileri de gönderir. Dağıtım için kaç gereç belirlemeniz gerekir.


## <a name="planning-limits"></a>Planlama limitleri
 
Planlama için bu tabloda özetlenen limitleri kullanın.

**Planlama** | **Sınırlar**
--- | --- 
**Azure geçişi projeleri** | Bir projede en fazla 35.000 sunucuyu değerlendirin.
**Azure Geçişi gereci** | Bir gereç, en fazla 1000 sunucuyu bulabilir.<br/> Bir gereç, yalnızca tek bir Azure geçişi projesiyle ilişkilendirilebilir.<br/> Herhangi bir sayıda gereç, tek bir Azure geçişi projesiyle ilişkilendirilebilir. <br/><br/> 
**Grup** | Tek bir gruba en fazla 35.000 sunucu ekleyebilirsiniz.
**Azure geçişi değerlendirmesi** | Tek bir değerlendirmede en fazla 35.000 sunucu değerlendirebilirsiniz.


## <a name="other-planning-considerations"></a>Diğer planlama konuları

- Gereci bulmayı başlatmak için, her fiziksel sunucuyu seçmeniz gerekir. 

## <a name="prepare-for-assessment"></a>Değerlendirme için hazırlanma

Azure ve fiziksel sunucuları sunucu değerlendirmesi için hazırlayın. 

1. [Fiziksel sunucu desteği gereksinimlerini ve sınırlamalarını](migrate-support-matrix-physical.md)doğrulayın.
2. Azure hesabınız için Azure geçişi ile etkileşime geçmek üzere izinleri ayarlayın.
3. Fiziksel sunucuları hazırlayın.

Bu ayarları yapılandırmak için [Bu öğreticideki](./tutorial-discover-physical.md) yönergeleri izleyin.

## <a name="create-a-project"></a>Proje oluşturma

Planlama gereksinimlerinize uygun olarak şunları yapın:

1. Bir Azure Geçişi projesi oluşturun.
2. Azure geçişi sunucu değerlendirmesi aracını projelere ekleyin.

[Daha fazla bilgi edinin](./create-manage-projects.md)

## <a name="create-and-review-an-assessment"></a>Değerlendirme oluşturma ve gözden geçirme

1. Fiziksel sunucular için değerlendirmeler oluşturun.
1. Geçiş planlaması hazırlığı sırasında değerlendirmeleri gözden geçirin.

Değerlendirmeler oluşturma ve gözden geçirme hakkında [daha fazla bilgi edinin](tutorial-assess-physical.md) .
    

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede şunları yapacaksınız:
 
> [!div class="checklist"] 
> * Fiziksel sunucular için Azure geçişi değerlendirmelerinin ölçeklendirilmesi planlanmaktadır.
> * Değerlendirme için Azure ve fiziksel sunucular hazırlandı.
> * Bir Azure geçişi projesi oluşturdunuz ve değerlendirmeler çalıştırıldı.
> * Geçişe hazırlanmayla ilgili değerlendirmeler gözden geçirildi.

Şimdi, değerlendirmelerin nasıl hesaplanacağını ve [değerlendirmelerin nasıl değiştirileceğini](how-to-modify-assessment.md) [öğrenin](concepts-assessment-calculation.md) .