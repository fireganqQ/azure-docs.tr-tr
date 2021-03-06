---
title: Azure geçişi ile Azure 'a geçiş için çok sayıda Hyper-V VM 'yi değerlendirin | Microsoft Docs
description: Azure geçişi hizmeti kullanılarak Azure 'a geçiş için çok sayıda Hyper-V sanal makinesi değerlendirme işlemini açıklar.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 07/10/2019
ms.openlocfilehash: 92c275ee3f8e00e71b80e448c9adb94f0b6d21dc
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96753731"
---
# <a name="assess-large-numbers-of-hyper-v-vms-for-migration-to-azure"></a>Azure 'a geçiş için çok sayıda Hyper-V VM 'yi değerlendirin

Bu makalede, Azure geçişi sunucu değerlendirmesi Aracı kullanılarak Azure 'a geçiş için çok sayıda şirket içi Hyper-V sanal makinelerinden nasıl değerlendirireceğiniz açıklanır.

[Azure geçişi](migrate-services-overview.md) , Microsoft Azure için uygulamaları, altyapıyı ve iş yüklerini keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan araçların merkezini sağlar. Hub, Azure geçiş araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir. 


Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
> * Ölçek ölçeğinde değerlendirme planlayın.
> * Azure izinlerini yapılandırın ve Hyper-V ' i değerlendirme için hazırlayın.
> * Bir Azure geçişi projesi oluşturun ve bir değerlendirme oluşturun.
> * Taşımayı planlarken değerlendirmesi gözden geçirin.


> [!NOTE]
> Ölçeği değerlendirmek için birkaç VM 'yi değerlendirmek üzere kavram kanıtı denemek istiyorsanız, [öğretici serimizi](./tutorial-discover-hyper-v.md) izleyin

## <a name="plan-for-assessment"></a>Değerlendirme planı

Çok sayıda Hyper-V VM değerlendirmesi için planlama yaparken göz önünde bulundurmanız gereken birkaç nokta vardır:

- **Azure geçişi projelerini planlayın**: Azure geçişi projelerinin nasıl dağıtılacağını öğrenin. Örneğin, Veri merkezleriniz farklı coğrafi bölgelerde ise ya da bulma, değerlendirme veya geçişle ilgili meta verileri farklı bir Coğrafya 'da depolamanız gerekirse, birden çok proje gerekebilir.
- **Gereçler planı**: Azure geçişi, bir Hyper-V sanal makinesi olarak dağıtılan, bir şirket Içi Azure geçiş gereci kullanarak değerlendirme ve geçiş Için sanal makineleri sürekli olarak bulur. Gereç, VM 'Leri, diskleri veya ağ bağdaştırıcılarını ekleme gibi ortam değişikliklerini izler. Ayrıca, Azure 'a bunlarla ilgili meta veriler ve performans verileri de gönderir. Dağıtım için kaç gereç belirlemeniz gerekir.


## <a name="planning-limits"></a>Planlama limitleri
 
Planlama için bu tabloda özetlenen limitleri kullanın.

**Planlama** | **Sınırlar**
--- | --- 
**Azure geçişi projeleri** | Bir projede en fazla 35.000 VM 'yi değerlendirin.
**Azure Geçişi gereci** | Bir gereç, en fazla 5000 VM bulabilir.<br/> Bir gereç, 300 adede kadar Hyper-V konaklarına bağlanabilir.<br/> Bir gereç, yalnızca tek bir Azure geçişi projesiyle ilişkilendirilebilir.<br/> Herhangi bir sayıda gereç, tek bir Azure geçişi projesiyle ilişkilendirilebilir. <br/><br/> 
**Grup** | Tek bir gruba en fazla 35.000 VM ekleyebilirsiniz.
**Azure geçişi değerlendirmesi** | Tek bir değerlendirmede 35.000 adede kadar VM 'yi değerlendirebilirsiniz.



## <a name="other-planning-considerations"></a>Diğer planlama konuları

- Gereci bulmayı başlatmak için, her bir Hyper-V konağını seçmeniz gerekir. 
- Çok kiracılı bir ortam çalıştırıyorsanız, şu anda yalnızca belirli bir kiracıya ait olan VM 'Leri keşfedesiniz. 

## <a name="prepare-for-assessment"></a>Değerlendirme için hazırlanma

Sunucu değerlendirmesi için Azure ve Hyper-V ' i hazırlayın. 

1. [Hyper-V destek gereksinimlerini ve sınırlamalarını](migrate-support-matrix-hyper-v.md)doğrulayın.
2. Azure hesabınız için Azure geçişi ile etkileşime geçmek üzere izinleri ayarlama
3. Hyper-V konakları ve VM 'Leri hazırlama

Bu ayarları yapılandırmak için [Bu öğreticideki](./tutorial-discover-hyper-v.md) yönergeleri izleyin.

## <a name="create-a-project"></a>Proje oluşturma

Planlama gereksinimlerinize uygun olarak şunları yapın:

1. Bir Azure geçişi projesi oluşturun.
2. Azure geçişi sunucu değerlendirmesi aracını projelere ekleyin.

[Daha fazla bilgi edinin](./create-manage-projects.md)

## <a name="create-and-review-an-assessment"></a>Değerlendirme oluşturma ve gözden geçirme

1. Hyper-V VM 'Leri için değerlendirmeler oluşturun.
1. Geçiş planlaması hazırlığı sırasında değerlendirmeleri gözden geçirin.

Değerlendirmeler oluşturma ve gözden geçirme hakkında [daha fazla bilgi edinin](tutorial-assess-hyper-v.md) .
    

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede şunları yapacaksınız:
 
> [!div class="checklist"] 
> * Hyper-V VM 'Leri için Azure geçişi değerlendirmelerinin ölçeklendirilmesi planlanmaktadır
> * Değerlendirme için Azure ve Hyper-V hazırlandı
> * Bir Azure geçişi projesi oluşturdunuz ve değerlendirmeler çalıştırıldı
> * Geçişe hazırlanmayla ilgili değerlendirmeler gözden geçirildi.

Şimdi, değerlendirmelerin nasıl hesaplanacağını ve [değerlendirmelerin nasıl değiştirileceğini](how-to-modify-assessment.md) [öğrenin](concepts-assessment-calculation.md)