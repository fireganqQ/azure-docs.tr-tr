---
title: Azure Stack Edge Pro 'Yu dağıtmaya Azure portal, Datacenter ortamını hazırlama öğreticisi | Microsoft Docs
description: Azure Stack Edge Pro 'Yu dağıtmaya yönelik ilk öğretici, Azure portal hazırlamayı içerir.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 01/22/2021
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Azure Stack Edge Pro so I can use it to transfer data to Azure.
ms.openlocfilehash: 07b526d443b5f1b41bc6f811b7cccc0fbc6165ee
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98761719"
---
# <a name="tutorial-prepare-to-deploy-azure-stack-edge-pro"></a>Öğretici: Azure Stack Edge Pro 'Yu dağıtmaya hazırlanma  

Bu, Azure Stack Edge Pro 'Yu tamamen dağıtmak için gereken dağıtım öğreticilerinde ilk öğreticidir. Bu öğreticide, Azure portal Azure Stack Edge kaynağını dağıtmaya yönelik nasıl hazırlanılacağı açıklanmaktadır.

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Portal hazırlığı 10 dakikadan kısa sürer.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="get-started"></a>başlarken

Azure Stack Edge Pro 'Yu dağıtmak için, önceden tanımlanmış sırada aşağıdaki öğreticilere bakın.

| **#** | **Bu adımda** | **Bu belgeleri kullanın** |
| --- | --- | --- | 
| 1. |**[Azure Stack Edge Pro için Azure portal hazırlama](azure-stack-edge-deploy-prep.md)** |Azure Stack Edge kaynağını bir Azure Stack Box Edge fiziksel cihazı yüklemeden önce oluşturun ve yapılandırın. |
| 2. |**[Azure Stack Edge Pro 'Yu yükler](azure-stack-edge-deploy-install.md)**|Azure Stack Edge Pro fiziksel cihazının paketini açın, raf ve kablo.  |
| 3. |**[Azure Stack Edge Pro 'Yu bağlama, ayarlama ve etkinleştirme](azure-stack-edge-deploy-connect-setup-activate.md)** |Yerel web kullanıcı arabirimine bağlayın, cihaz kurulumunu tamamlayın ve cihazı etkinleştirin. Cihaz SMB veya NFS paylaşımlarının kurulması için hazırdır.  |
| 4. |**[Azure Stack Edge Pro ile veri aktarma](azure-stack-edge-deploy-add-shares.md)** |Paylaşımları ekleyin ve SMB veya NFS üzerinden paylaşımlara bağlanın. |
| 5. |**[Azure Stack Edge Pro ile veri dönüştürme](azure-stack-edge-deploy-configure-compute.md)** |Verileri Azure 'a taşırken verileri dönüştürmek için cihazdaki işlem modüllerini yapılandırın. |

Artık Azure portalını ayarlamaya başlayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Azure Stack Edge kaynağınız, Azure Stack Edge Pro cihazınız ve veri merkezi ağı için yapılandırma önkoşulları aşağıda verilmiştir.

### <a name="for-the-azure-stack-edge-resource"></a>Azure Stack Edge kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

* Microsoft Azure aboneliğiniz Azure Stack Edge kaynağı için etkinleştirildi. [Microsoft kurumsal anlaşma (EA)](https://azure.microsoft.com/overview/sales-number/), [bulut çözümü sağlayıcısı (CSP)](/partner-center/azure-plan-lp)veya [Microsoft Azure sponsorluğu](https://azure.microsoft.com/offers/ms-azr-0036p/)gibi desteklenen bir abonelik kullandığınızdan emin olun. Kullandıkça öde abonelikleri desteklenmez.

* Azure Stack Edge/Data Box Gateway, IoT Hub ve Azure depolama kaynakları için kaynak grubu düzeyinde sahip veya katkıda bulunan erişiminiz var.

  * Katkıda bulunan erişimi sağlamak için abonelik düzeyinde bir **sahip** olmanız gerekir. Başka birine katkıda bulunan erişim sağlamak için, Azure Portal, **tüm hizmetler**  >  **abonelikleri**  >  **erişim denetimi (IAM)**  >  **+**  >  **Rol Ekle ataması** Ekle ' ye gidin. Daha fazla bilgi için bkz. [öğretici: Azure Portal kullanarak Azure kaynaklarına Kullanıcı erişimi verme](../role-based-access-control/quickstart-assign-role-user-portal.md).

  * Azure Stack Edge/Data Box Gateway kaynağı oluşturmak için, kaynak grubu düzeyinde katkıda bulunan (veya üzeri) izinlere sahip olmanız gerekir. Ayrıca, kaynak sağlayıcısının kayıtlı olduğundan emin olmanız gerekir `Microsoft.DataBoxEdge` . Kaynak sağlayıcısını kaydetme hakkında daha fazla bilgi için bkz. [kayıt kaynak sağlayıcısı](azure-stack-edge-manage-access-power-connectivity-mode.md#register-resource-providers).
  * Herhangi bir IoT Hub kaynağı oluşturmak için, Microsoft. Devices sağlayıcısının kayıtlı olduğundan emin olun. Nasıl kaydedileceği hakkında bilgi için [Kaynak sağlayıcısını kaydetme](azure-stack-edge-manage-access-power-connectivity-mode.md#register-resource-providers) bölümüne gidin.
  * Bir depolama hesabı kaynağı oluşturmak için, kaynak grubu düzeyinde katkıda bulunan veya daha yüksek erişim kapsamına ihtiyacınız vardır. Azure depolama, varsayılan olarak kayıtlı bir kaynak sağlayıcısıdır.
* Azure Active Directory Graph API için yönetici veya Kullanıcı erişiminiz var. Daha fazla bilgi için bkz. [Azure Active Directory Graph API](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).
* Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.
* Sistem yöneticiniz tarafından ayarlanan herhangi bir Azure ilkesi tarafından engellenmiyor. İlkeler hakkında daha fazla bilgi için bkz. [hızlı başlangıç: uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma](../governance/policy/assign-policy-portal.md).

### <a name="for-the-azure-stack-edge-pro-device"></a>Azure Stack Edge Pro cihazı için

Fiziksel cihazı dağıtmadan önce şunlardan emin olun:

* Sevkiyat paketine dahil edilen güvenlik bilgilerini gözden geçirdiniz.
* Veri merkezinizde bulunan ve cihazı bağlamak için standart 19 "rafındaki bir 1U yuvasına sahipsiniz.
* Cihazın güvenle geri kalanında düz, kararlı ve düzeyi bir iş yüzeyine erişebilirsiniz.
* Cihazı ayarlamayı planladığınız sitenin, bağımsız bir kaynaktan veya kesintisiz güç kaynağı (UPS) olan bir raf güç dağıtım biriminden (PDU) standart AC gücü vardır.
* Fiziksel cihaza erişebiliyor olmanız gerekir.

### <a name="for-the-datacenter-network"></a>Veri merkezi ağı için

Başlamadan önce aşağıdakilerden emin olun:

* Veri merkezinizdeki ağ, Azure Stack Edge Pro cihazınız için ağ gereksinimleri uyarınca yapılandırılır. Daha fazla bilgi için bkz. [Edge Pro sistem gereksinimleri Azure Stack](azure-stack-edge-system-requirements.md).

* Azure Stack Edge Pro 'nun normal işletim koşullarında şunları yapabilirsiniz:

  * Cihazın güncel kalmasını sağlamak için en az 10 Mbps indirme bant genişliği.
  * Dosyaları aktarmak için en az 20 Mbps adanmış karşıya yükleme ve indirme bant genişliği.

## <a name="create-a-new-resource"></a>Yeni kaynak oluşturma

Fiziksel cihazınızı yönetmek için mevcut bir Azure Stack Edge kaynağınız varsa, bu adımı atlayın ve [etkinleştirme anahtarını almak](#get-the-activation-key)için gidin.

Azure Stack Edge kaynağı oluşturmak için Azure portal aşağıdaki adımları uygulayın.

1. Oturum açmak için Microsoft Azure kimlik bilgilerinizi kullanın 

    - Bu URL 'de Azure portal: [https://portal.azure.com](https://portal.azure.com) .
    - Ya da bu URL 'de Azure Kamu Portalı: [https://portal.azure.us](https://portal.azure.us) . Daha fazla ayrıntı için [portalı kullanarak Azure Kamu 'Ya bağlanma](../azure-government/documentation-government-get-started-connect-with-portal.md)konusuna gidin.

2. Sol bölmede **+ kaynak oluştur**' u seçin. **Azure Stack Edge/Data Box Gateway** için arama yapın ve seçin. **Oluştur**’u seçin.
3. Azure Stack Edge Pro cihazı için kullanmak istediğiniz aboneliği seçin. Azure Stack Edge kaynağını dağıtmak istediğiniz bölgeyi seçin. Azure Stack Edge kaynağının kullanılabildiği tüm bölgelerin listesi için bkz. [bölgeye göre kullanılabilir Azure ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all).

    Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin. Bölge yalnızca cihaz yönetimi için meta verileri depolar. Gerçek veriler herhangi bir depolama hesabında depolanabilir.
    
    **Azure Stack Edge Pro** seçeneğinde **Oluştur**' u seçin.

    ![Azure Stack Edge hizmeti ara](media/azure-stack-edge-deploy-prep/data-box-edge-sku.png)

3. **Temel bilgiler** sekmesinde, aşağıdaki **proje ayrıntılarını** girin veya seçin.
    
    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik    |Bu, önceki seçime göre otomatik olarak doldurulur. Abonelik fatura hesabınıza bağlıdır. |
    |Kaynak grubu  |Mevcut grubu seçin veya yeni bir grup oluşturun.<br>[Azure Kaynak Grupları](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin.     |

4. Aşağıdaki **örnek ayrıntılarını** girin veya seçin.

    |Ayar  |Değer  |
    |---------|---------|
    |Ad   | Kaynağı tanımlamak için kolay bir ad.<br>Ad, harfler, rakamlar ve kısa çizgiler dahil olmak üzere 2 ve 50 karakterden oluşmalıdır.<br> Ad bir harf veya rakamla başlar ve biter.        |
    |Bölge     |Azure Stack Edge kaynağının kullanılabildiği tüm bölgelerin listesi için bkz. [bölgeye göre kullanılabilir Azure ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Azure Kamu kullanıyorsanız, tüm kamu bölgeleri [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/regions/)gösterildiği gibi kullanılabilir.<br> Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın bir konum seçin.|

    ![Proje ve örnek ayrıntıları](media/azure-stack-edge-deploy-prep/data-box-edge-resource.png)

5. **İleri ' yi seçin: sevkiyat adresi**.

    - Zaten bir cihazınız varsa, **Azure Stack Edge cihazım** için Birleşik giriş kutusunu seçin.
    - Bu, sipariş ettiğiniz yeni bir cihaz ise, kişi adını, şirketi, cihazı sevk etmek için adresi ve iletişim bilgilerini girin.

    ![Yeni cihaz için sevkiyat adresi](media/azure-stack-edge-deploy-prep/data-box-edge-resource1.png)

6. **Sonraki: Gözden geçirme ve oluşturma**’yı seçin.

7. **Gözden geçir + oluştur** sekmesinde, **fiyatlandırma ayrıntılarını**, **kullanım koşulları** ve kaynağınızın ayrıntılarını gözden geçirin. **Gizlilik koşullarını Incelediğim** Birleşik giriş kutusunu seçin.

    ![Azure Stack Edge kaynak ayrıntılarını ve gizlilik koşullarını gözden geçirin](media/azure-stack-edge-deploy-prep/data-box-edge-resource2.png)

8. **Oluştur**’u seçin.

   Kaynağın oluşturulması birkaç dakika sürer. Kaynak başarıyla oluşturulup dağıtıldıktan sonra bilgilendirirsiniz. **Kaynağa git**’i seçin.

   ![Azure Stack Edge kaynağına git](media/azure-stack-edge-deploy-prep/data-box-edge-resource3.png)

Sipariş yerleştirildikten sonra, Microsoft siparişi inceler ve gönderim ayrıntıları ile size (e-posta aracılığıyla) ulaşır.

![Azure Stack Edge Pro sırasının incelenmesi için bildirim](media/azure-stack-edge-deploy-prep/data-box-edge-resource4.png)


> [!NOTE]
> Aynı anda birden çok sipariş oluşturmak veya var olan bir siparişi kopyalamak istiyorsanız, [Azure örnekleri içindeki betikleri](https://github.com/Azure-Samples/azure-stack-edge-order)kullanabilirsiniz. Daha fazla bilgi için bkz. README dosyası.

## <a name="get-the-activation-key"></a>Etkinleştirme anahtarı alma

Azure Stack Edge kaynağı çalışır duruma geçtikten sonra etkinleştirme anahtarını almanız gerekir. Bu anahtar Azure Stack Edge Pro cihazınızı etkinleştirmek ve kaynakla bağlamak için kullanılır. Bu anahtarı şimdi, Azure portalındayken alabilirsiniz.

1. Oluşturduğunuz kaynağa gidin ve **genel bakış**' ı seçin. Siparişinizin işlenme efektiyle ilgili bir bildirim görürsünüz.

    ![Genel Bakış ' ı seçin](media/azure-stack-edge-deploy-prep/data-box-edge-select-devicesetup.png)

2. Sipariş işlendikten ve cihaz sizin bir biçimde olduğunda, **genel bakış** güncelleştirilir. Varsayılan **Azure Key Vault adını** kabul edin veya yeni bir tane girin. **Etkinleştirme anahtarı oluştur**' u seçin. Anahtarı kopyalamak için Kopyala simgesini seçin ve daha sonra kullanmak üzere kaydedin.

    ![Etkinleştirme anahtarını alma](media/azure-stack-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
>
> * Etkinleştirme anahtarı üretilmeden üç gün sonra dolar.
> * Anahtarın süresi dolmuşsa yeni bir anahtar oluşturun. Eski anahtar geçerli olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki gibi Azure Stack Edge Pro konuları hakkında bilgi edindiniz:

> [!div class="checklist"]
>
> * Yeni kaynak oluşturma
> * Etkinleştirme anahtarı alma

Azure Stack Edge Pro 'Yu nasıl yükleyeceğinizi öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Stack Edge Pro 'Yu yükler](./azure-stack-edge-deploy-install.md)