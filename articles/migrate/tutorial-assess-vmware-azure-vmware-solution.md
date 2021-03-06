---
title: Azure geçişi ile Azure VMware çözümüne (AVS) geçiş için VMware VM 'lerini değerlendirin
description: Azure geçişi sunucu değerlendirmesi ile AVS 'ye geçiş için VMware VM 'lerini değerlendirme hakkında bilgi edinin.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: tutorial
ms.date: 09/14/2020
ms.custom: MVC
ms.openlocfilehash: e57084dab00210802edbd46e3380313e034eb036
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2021
ms.locfileid: "98566773"
---
# <a name="tutorial-assess-vmware-vms-for-migration-to-avs"></a>Öğretici: AVS 'ye geçiş için VMware VM 'lerini değerlendirin

Azure 'a geçiş sürecinizin bir parçası olarak, bulut hazırlığını ölçmek, riskleri belirlemek ve maliyetleri ve karmaşıklığı tahmin etmek için şirket içi iş yüklerinizi değerlendirmenizi sağlar.

Bu makalede, Azure geçişi: Sunucu değerlendirmesi Aracı kullanılarak Azure VMware çözümüne (AVS) geçiş için keşfedilen VMware sanal makinelerini (VM 'Ler) nasıl değerlendireceğiniz gösterilmektedir. AVS, Azure 'da VMware platformunu çalıştırmanıza olanak tanıyan bir yönetilen hizmettir.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:
> [!div class="checklist"]
- Makine meta verileri ve yapılandırma bilgilerine göre bir değerlendirme çalıştırın.
- Performans verilerine göre bir değerlendirme çalıştırın.

> [!NOTE]
> Öğreticiler senaryo denemek için en hızlı yolu gösterir ve mümkün olan yerlerde varsayılan seçenekleri kullanır. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.



## <a name="prerequisites"></a>Önkoşullar

Makinelerinizi AVS 'ye geçiş için değerlendirmek üzere bu öğreticiyi izlemeden önce, değerlendirmek istediğiniz makineleri keşfetdiğinizden emin olun:

- Azure geçişi gereci kullanarak makineleri bulma [öğreticisini izleyin](tutorial-discover-vmware.md). 
- İçeri aktarılan bir CSV dosyası kullanan makineleri saptamak için [Bu öğreticiyi izleyin](tutorial-discover-import.md).


## <a name="decide-which-assessment-to-run"></a>Hangi değerlendirmenin çalıştırılacağını belirleyin


Şirket içinde olduğu gibi toplanan veya dinamik performans verilerinde makine yapılandırma verilerine/meta verilere dayalı olarak boyutlandırma ölçütlerini kullanarak bir değerlendirme çalıştırmak istediğinize karar verin.

**Değerlendirme** | **Ayrıntılar** | **Öneri**
--- | --- | ---
**Şirket içinde olduğu gibi** | Makine yapılandırma verilerine/meta verilere göre değerlendirin.  | AVS 'de önerilen düğüm boyutu, düğüm türü, depolama türü ve başarısızlık-Tolerans ayarı için değerlendirmede belirlediğiniz ayarlarla birlikte Şirket içi VM boyutunu temel alır.
**Performans tabanlı** | Toplanan dinamik performans verilerine göre değerlendirin. | AVS 'de önerilen düğüm boyutu, CPU ve bellek kullanım verilerine, düğüm türü, depolama türü ve başarısızlık-Tolerans ayarı için değerlendirmede belirlediğiniz ayarlarla birlikte dayanır.

## <a name="run-an-assessment"></a>Değerlendirme çalıştırma

Bir değerlendirmeyi aşağıdaki gibi çalıştırın:

1. **Windows ve Linux sunucuları**> **sunucular** sayfasında, **sunucuları değerlendir ve geçir**' e tıklayın.

   ![Sunucuları değerlendir ve geçir düğmesinin konumu](./media/tutorial-assess-vmware-azure-vmware-solution/assess.png)

1. **Azure geçişi: Sunucu değerlendirmesi**' nde **değerlendir**' e tıklayın.

1. **Sunucu**  >  **değerlendirmesi türünü** değerlendir bölümünde **Azure VMware çözümü (AVS) (Önizleme)** seçeneğini belirleyin.

1. **Bulma kaynağında**:

    - Gereci kullanarak makineler keşfetiyorsanız, **Azure geçişi gereci ' ndan bulunan makineler**' i seçin.
    - İçeri aktarılan bir CSV dosyası kullanan makineler tespit ederseniz, **Içeri aktarılan makineler**' i seçin. 
    
1. Değerlendirme özelliklerini gözden geçirmek için **Düzenle** ' ye tıklayın.

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/assess-servers.png" alt-text="Değerlendirme ayarlarını seçme sayfası":::
 

1. **Değerlendirme özellikleri**  >  **hedef özellikleri**:

    - **Hedef konum**' da, geçirmek istediğiniz Azure bölgesini belirtin.
       - Boyut ve maliyet önerileri belirttiğiniz konuma göre hesaplanır.
       - Şu anda dört bölge için değerlendirme yapabilirsiniz (Avustralya Doğu, Doğu ABD, Batı Avrupa, Batı ABD)
   - **Depolama türü** varsayılan olarak **vSAN** olarak ayarlanır. Bu, bir AVS özel bulutu için varsayılan depolama türüdür.
   - **Ayrılmış örnekler** Şu anda AVS düğümleri için desteklenmiyor.
1. **VM boyutu**:
    - **Düğüm türü** varsayılan olarak **AV36**' dir. Azure geçişi, VM 'Leri AVS 'ye geçirmek için gereken düğüm düğümünü önerir.
    - **FTT ayarında, RAID düzeyinde**, tolerans ve RAID birleşimine yönelik hata seçin.  Seçili FTT seçeneği, şirket içi VM disk gereksinimiyle birlikte kullanıldığında, AVS 'de gereken toplam vSAN depolama alanını belirler.
    - **CPU fazla aboneliği**' nde, AVS düğümündeki bir fiziksel çekirdekle ilişkili sanal çekirdekleri oranını belirtin. 4:1 'den büyük abonelik, performans düşüşüne neden olabilir, ancak Web sunucusu türü iş yükleri için de kullanılabilir.

1. **Düğüm boyutu**: 
    - **Boyutlandırma ölçütünde**, değerlendirmeyi statik meta verilerde veya performans tabanlı verilerde temel almak istiyorsanız seçin. Performans verileri kullanıyorsanız:
        - **Performans geçmişi**' nde, değerlendirmeye dayandırmak istediğiniz veri süresini belirtin
        - **Yüzdelik kullanım**' de, performans örneği için kullanmak istediğiniz yüzdebirlik değerini belirtin. 
    - **Rahatlık faktörüyle**, değerlendirme sırasında kullanmak istediğiniz arabelleği belirtin. Bu hesaplar, dönemsel kullanım, kısa performans geçmişi ve gelecekteki kullanımlarda olası artışlar gibi sorunlara yöneliktir. Örneğin, iki bir rahatlık faktörü kullanıyorsanız:
    
        **Bileşen** | **Etkin kullanım** | **Rakip çarpanı ekleme (2,0)**
        --- | --- | ---
        Çekirdekler | 2  | 4
        Bellek | 8 GB | 16 GB  

1. **Fiyatlandırma**:
    - **Teklifte**, kaydettiğiniz [Azure teklifi](https://azure.microsoft.com/support/legal/offer-details/) görüntülenir sunucu değerlendirmesi, bu teklifin maliyetini tahmin eder.
    - **Para birimi**' nde, hesabınız için faturalandırma para birimini seçin.
    - **İndirim (%)** bölümünde, Azure teklifinin üzerine aldığınız aboneliğe özgü indirimleri ekleyin. Varsayılan ayar, %0’dır.

1. Değişiklik yaparsanız **Kaydet** ' e tıklayın.

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/avs-view-all.png" alt-text="Değerlendirme özellikleri":::

1. **Sunucuları değerlendir** bölümünde **İleri**' ye tıklayın.

1. Değerlendirme **adını değerlendirmek için makineleri seçin**  >   > değerlendirme için bir ad belirtin. 
 
1. > **Grup Seç veya oluştur** bölümünde **Yeni oluştur** ' u seçin ve bir grup adı belirtin. 
    
    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/assess-group.png" alt-text="Bir gruba VM ekleme":::
 
1. Gereç ' ı seçin ve gruba eklemek istediğiniz VM 'Leri seçin. Ardından **İleri**'ye tıklayın.

1. **İnceleme ve değerlendirme oluştur**' da, değerlendirme ayrıntılarını gözden geçirin ve grubu oluşturmak ve değerlendirmeyi çalıştırmak Için değerlendirme **Oluştur** ' a tıklayın.

    > [!NOTE]
    > Performans tabanlı değerlendirmeler için, değerlendirme oluşturmadan önce bulmayı başlattıktan sonra en az bir gün beklemeniz önerilir. Bu, daha yüksek güvenilirliğe sahip performans verilerinin toplanması için zaman sağlar. İdeal olarak, bulmayı başlattıktan sonra, belirttiğiniz performans süresini (gün/hafta/ay) yüksek güvenilirlikli bir derecelendirme için bekleyin.

## <a name="review-an-assessment"></a>Değerlendirmeyi gözden geçirme

Bir AVS değerlendirmesi şunları açıklar:

- AVS hazırlığı: şirket içi VM 'Lerin Azure VMware çözümüne (AVS) geçiş için uygun olup olmadığı.
- AVS düğüm sayısı: VM 'Leri çalıştırmak için gereken, tahmini AVS düğüm sayısı.
- AVS düğümleri genelinde kullanım: tüm düğümlerde öngörülen CPU, bellek ve depolama kullanımı.
    - Kullanım vCenter Server, NSX Yöneticisi (büyük), NSX Edge gibi aşağıdaki küme yönetimi üst kısmında UPX 'in yanı sıra, HCX, sıkıştırma ve yinelenenleri kaldırma işleminden önce yaklaşık 44vCPU (11 CPU), 75GB RAM ve 722GB depolama alanı kullanan düzenleme. 
    - Bellek, yinelenenleri kaldırma ve Compression Şu anda bellek ve 1,5 yinelenenleri kaldırma için %100 kullanımı ve sıkıştırma için Kullanıcı tanımlı bir giriş olacak şekilde, kullanıcının gerekli boyutlandırmasına ince ayar yapılmasına izin verir.
- Aylık maliyet tahmini: şirket içi VM 'Leri çalıştıran tüm Azure VMware çözümü (AVS) düğümlerine yönelik tahmini aylık maliyetler.

## <a name="view-an-assessment"></a>Değerlendirmeyi görüntüleme

Bir değerlendirmeyi görüntülemek için:

1. **Sunucular**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde, **değerlendirmeler**' ın yanındaki sayıya tıklayın.

1. **Değerlendirmeler** sayfasında açmak istediğiniz değerlendirmeye tıklayın. Örnek olarak (yalnızca Örneğin, tahminler ve maliyetler): 

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/avs-assessment-summary.png" alt-text="AVS değerlendirmesi Özeti":::

1. Değerlendirme özetini gözden geçirin. Ayrıca değerlendirme özelliklerini düzenleyebilir veya değerlendirmeyi yeniden hesaplayabilirsiniz.
 

### <a name="review-readiness"></a>İnceleme hazırlığı

1. **Azure hazırlığı**' ne tıklayın.
2. **Azure hazırlığı**' nde VM durumunu gözden geçirin.

    - **AVS Için hazırlanma**: makine, Azure AVS 'ye, hiçbir değişiklik yapılmadan olduğu gibi geçirilebilir. Makine, tam AVS desteğiyle AVS 'de başlatılır.
    - **Koşullara göre**: makinenin geçerli vSphere sürümü ile uyumluluk sorunları olabilir. AVS 'de tam işlevselliğe sahip olmak için VMware araçlarının yüklü olması veya diğer ayarlar olması gerekebilir.
    - **AVS için hazırlanma**: VM, AVS 'de başlamaz. Örneğin, şirket içi bir VMware sanal makinesine bağlı bir dış cihaz (CD-ROM gibi) varsa ve VMware VMotion kullanıyorsanız, VMotion işlemi başarısız olur.
 - **Hazır olma bilinmiyor**: Azure geçişi, şirket içi ortamdan toplanan yetersiz meta veriler nedeniyle makine hazırlığını saptayamadık.

3. Önerilen aracı inceleyin.

    - VMware HCX veya Enterprise: VMware makineleri Için VMWare karma bulut uzantısı (HCX) çözümü, şirket içi iş yükünüzü Azure VMware çözümünüz (AVS) özel bulutuna geçirmek için önerilen geçiş aracıdır. Daha fazla bilgi edinin.
    - Bilinmiyor: CSV dosya yoluyla içeri aktarılan makinelerde, varsayılan geçiş aracı bilinmiyor. Ancak VMware makinelerinde, VMware karma bulut uzantısı (HCX) çözümünün kullanılması önerilir.
4. AVS hazırlığı durumuna tıklayın. VM hazırlığı ayrıntılarını görüntüleyebilir ve işlem, depolama ve ağ ayarları dahil olmak üzere VM ayrıntılarını görmek için ayrıntıya gidebilirsiniz.

### <a name="review-cost-estimates"></a>Tahmini maliyetleri gözden geçirme

Değerlendirme özeti, Azure 'da çalışan VM 'lerin tahmini işlem ve depolama maliyetini gösterir. 

1. Aylık toplam maliyetleri gözden geçirin. Ücretler, değerlendirilen gruptaki tüm VM 'Ler için toplanır.

    - Maliyet tahminleri, toplam tüm VM 'lerin kaynak gereksinimlerini dikkate alarak gereken AVS düğüm sayısına bağlıdır.
    - AVS 'nin fiyatı düğüm başına olduğunda, toplam maliyet işlem maliyeti ve depolama maliyeti dağıtımına sahip değildir.
    - Maliyet tahmini, AVS 'de şirket içi VM 'Leri çalıştırmak içindir. Azure geçişi sunucu değerlendirmesi PaaS veya SaaS maliyetlerini göz önünde bulundurmaz.

2. Aylık depolama tahminlerini gözden geçirin. Görünüm, değerlendirilen grup için toplanan depolama maliyetlerini gösterir ve farklı türlerdeki depolama disklerinin üzerine bölünür. 
3. Belirli VM 'Ler için maliyet ayrıntılarını görmek için ayrıntıya gidebilirsiniz.

### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

Sunucu değerlendirmesi, performans tabanlı değerlendirmelere güvenle bir derecelendirme atar. Derecelendirme bir yıldız (en düşük) ile beş yıldız (en yüksek) arasında.

![Güvenilirlik derecelendirmesi](./media/tutorial-assess-vmware-azure-vmware-solution/confidence-rating.png)

Güvenilirlik derecelendirmesi, değerlendirmede boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur. Derecelendirme, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliğine bağlıdır.

> [!NOTE]
> Bir CSV dosyasını temel alan bir değerlendirme oluşturursanız güven derecelendirmeleri atanmaz.

Güven derecelendirmeleri aşağıdaki gibidir.

**Veri noktası kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
--- | ---
%0-%20 | 1 yıldız
%21-%40 | 2 yıldız
%41-%60 | 3 yıldız
%61-%80 | 4 yıldız
%81-%100 | 5 yıldız

Güvenilirlikli derecelendirmeler hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md#confidence-ratings-performance-based) .

## <a name="next-steps"></a>Sonraki adımlar

- [Bağımlılık eşlemesini](concepts-dependency-visualization.md)kullanarak makine bağımlılıklarını bulun.
- [Aracısız](how-to-create-group-machine-dependencies-agentless.md) veya [aracı tabanlı](how-to-create-group-machine-dependencies.md) bağımlılık eşlemesini ayarlayın.
