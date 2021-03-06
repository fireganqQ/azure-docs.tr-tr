---
title: Logic Apps ile tümleştirme
titleSuffix: Azure Digital Twins
description: Bkz. Azure dijital TWINS 'e Logic Apps bağlama, özel bağlayıcı kullanma
author: baanders
ms.author: baanders
ms.date: 9/11/2020
ms.topic: how-to
ms.service: digital-twins
ms.reviewer: baanders
ms.openlocfilehash: 3fbd9016bcbfa83574d894af7ca728b863f54344
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93129329"
---
# <a name="integrate-with-logic-apps-using-a-custom-connector"></a>Özel bağlayıcı kullanarak Logic Apps tümleştirme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) , uygulamalar ve hizmetler arasında iş akışlarını otomatikleştirmenize yardımcı olan bir bulut hizmetidir. Azure dijital TWINS API 'Lerine Logic Apps bağlayarak, Azure dijital TWINS ve bunların verileri etrafında bu tür otomatikleştirilmiş akışlar oluşturabilirsiniz.

Azure dijital TWINS 'in Şu anda Logic Apps için Sertifikalı (önceden oluşturulmuş) bir Bağlayıcısı yoktur. Bunun yerine, Azure Digital TWINS ile Logic Apps kullanmaya yönelik geçerli işlem, Logic Apps ile çalışmak üzere değiştirilmiş [özel bir Azure dijital TWINS Swagger](/samples/azure-samples/digital-twins-custom-swaggers/azure-digital-twins-custom-swaggers/) kullanarak [**özel bir Logic Apps Bağlayıcısı**](../logic-apps/custom-connector-overview.md)oluşturmaktır.

> [!NOTE]
> Yukarıdaki özel Swagger örneğinde bulunan Swagger 'un birden çok sürümü vardır. En son tarihe sahip alt klasörde en son sürüm bulunur, ancak örnekte yer alan önceki sürümler de desteklenmeye devam eder.

Bu makalede, bir Azure dijital TWINS örneğine Logic Apps bağlamak için kullanılabilecek **özel bir bağlayıcı oluşturmak** için [Azure Portal](https://portal.azure.com) kullanacaksınız. Daha sonra bu bağlantıyı bir örnek senaryo için kullanan **bir mantıksal uygulama oluşturacaksınız** . Bu, bir Zamanlayıcı tarafından tetiklenen olayların Azure dijital TWINS örneğindeki bir ikizi otomatik olarak güncelleştirilmesini sağlayacaktır. 

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce **[ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun** .
[Azure Portal](https://portal.azure.com) bu hesapla oturum açın. 

Önkoşul kurulumunun bir parçası olarak aşağıdaki öğeleri de doldurmanız gerekir. Bu bölümün geri kalanı aşağıdaki adımlarda size yol gösterir:
- Azure dijital TWINS örneği ayarlama
- Dijital ikizi ekleme

### <a name="set-up-azure-digital-twins-instance"></a>Azure dijital TWINS örneğini ayarlama

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

### <a name="add-a-digital-twin"></a>Dijital ikizi ekleme

Bu makalede, Azure dijital TWINS örneğiniz içindeki bir ikizi güncelleştirmek için Logic Apps kullanılır. Devam etmek için, örneğinize en az bir ikizi eklemeniz gerekir. 

[Digitaltwıns API 'lerini](/rest/api/digital-twins/dataplane/twins), [.net (C#) SDK 'Sını](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet-preview&preserve-view=true)veya [Azure dijital TWINS CLI](how-to-use-cli.md)'yi kullanarak TWINS ekleyebilirsiniz. Bu yöntemleri kullanarak TWINS oluşturma hakkında ayrıntılı adımlar için bkz. [*nasıl yapılır: dijital TWINS 'ı yönetme*](how-to-manage-twin.md).

Örneğiniz içinde oluşturduğunuz bir ikizi **_IKIZI ID_** 'ye ihtiyacınız olacaktır.

## <a name="set-up-app-registration"></a>Uygulama kaydını ayarlama

[!INCLUDE [digital-twins-prereq-registration.md](../../includes/digital-twins-prereq-registration.md)]

### <a name="get-app-registration-client-secret"></a>Uygulama kaydı istemci gizliliğini al

Ayrıca, Azure AD uygulama kaydınız için bir **_istemci gizli anahtarı_** oluşturmanız gerekir. Bunu yapmak için, Azure portal [uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) sayfasına gidin (Bu bağlantıyı kullanabilir veya Portal arama çubuğunda bulabilirsiniz). Ayrıntılarını açmak için listeden önceki bölümde oluşturduğunuz kaydınızı seçin. 

Kayıt menüsündeki *Sertifikalar ve gizli* dizileri vurun ve *+ yeni istemci parolası* ' nı seçin.

:::image type="content" source="media/how-to-integrate-logic-apps/client-secret.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

*Açıklama* ve *süre sonu* için Istediğiniz değerleri girin ve *Ekle* 'ye basın.

:::image type="content" source="media/how-to-integrate-logic-apps/add-client-secret.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

Şimdi, istemci parolasının _süre sonu_ ve _değer_ alanlarıyla birlikte _Sertifikalar & gizlilikler_ sayfasında görünür olduğunu doğrulayın. Daha sonra kullanmak için _değerini_ bir yere göz atın (kopyalama simgesiyle birlikte Pano 'ya de kopyalayabilirsiniz)

:::image type="content" source="media/how-to-integrate-logic-apps/client-secret-value.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

## <a name="create-custom-logic-apps-connector"></a>Özel Logic Apps Bağlayıcısı oluştur

Şimdi Azure dijital TWINS API 'Leri için [özel bir Logic Apps Bağlayıcısı](../logic-apps/custom-connector-overview.md) oluşturmaya hazırsınız. Bunu yaptıktan sonra, bir sonraki bölümde bir mantıksal uygulama oluştururken Azure dijital TWINS 'i yedeklenebilir.

Azure portal [Logic Apps özel bağlayıcı](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Web%2FcustomApis) sayfasına gidin (Bu bağlantıyı kullanabilir veya Portal arama çubuğunda arama yapabilirsiniz). İsabet *+ Ekle* .

:::image type="content" source="media/how-to-integrate-logic-apps/logic-apps-custom-connector.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

Aşağıdaki *Logic Apps özel bağlayıcı oluştur* sayfasında, aboneliğiniz ve kaynak grubunuz ' ı ve yeni Bağlayıcınız için bir ad ve dağıtım konumunu seçin. *İnceleme ve oluşturma* düğmesine basın. 

:::image type="content" source="media/how-to-integrate-logic-apps/create-logic-apps-custom-connector.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

Bu işlem sizi, kaynağı oluşturmak için en altta *Oluştur* ' u vuran *İnceleme + oluştur* sekmesine götürür.

:::image type="content" source="media/how-to-integrate-logic-apps/review-logic-apps-custom-connector.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

Bağlayıcının dağıtım sayfasına yönlendirilirsiniz. Dağıtım tamamlandığında, bağlayıcının portalda ayrıntılarını görüntülemek için *Kaynağa Git* düğmesine basın.

### <a name="configure-connector-for-azure-digital-twins"></a>Azure dijital TWINS için bağlayıcı yapılandırma

Daha sonra, oluşturduğunuz bağlayıcıyı Azure dijital TWINS 'e ulaşmak için yapılandıracaksınız.

İlk olarak, Logic Apps çalışmak üzere değiştirilmiş olan özel bir Azure dijital TWINS Swagger 'yi indirin. *Posta indirme* düğmesine basarak [**Bu bağlantıdan**](/samples/azure-samples/digital-twins-custom-swaggers/azure-digital-twins-custom-swaggers/) **Azure Digital twıns özel Swaggers (Logic Apps Bağlayıcısı)** örneğini indirin. İndirilen *Azure_Digital_Twins_custom_Swaggers__Logic_Apps_connector_.zip* klasörüne gidin ve sıkıştırmayı açın. 

Bu öğreticide özel Swagger, _* * Azure_Digital_Twins_custom_Swaggers__Logic_Apps_connector_ \Logicapps **_ klasöründe bulunur. Bu klasör, her ikisi de tarihe göre düzenlenmiş farklı Swagger sürümlerini tutan, *Stable* ve *Preview* adlı alt klasörleri içerir. En son tarihi olan klasör Swagger 'nin en son kopyasını içerir. Hangi sürümü seçerseniz, Swagger dosyası** * * _ üzerinde _digitaltwins.jsolarak adlandırılır.

> [!NOTE]
> Bir önizleme özelliğiyle çalışmıyorsanız, genellikle Swagger 'nin en son *kararlı* sürümünü kullanmanız önerilir. Ancak, Swagger 'nin önceki sürümleri ve önizleme sürümleri de desteklenmeye devam eder. 

Sonra, [Azure Portal](https://portal.azure.com) bağlayıcının genel bakış sayfasına gidin ve *Düzenle* ' ye basın.

:::image type="content" source="media/how-to-integrate-logic-apps/edit-connector.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

Aşağıdaki *Logic Apps özel bağlayıcıyı Düzenle* sayfasında, bu bilgileri yapılandırın:
* **Özel bağlayıcılar**
    - API uç noktası: REST (varsayılan olarak bırakın)
    - İçeri aktarma modu: Openapı dosyası (varsayılanı bırak)
    - Dosya: Bu, daha önce indirdiğiniz özel Swagger dosyası olacaktır. *Içeri aktar* ' a gidin, makinenizde dosyayı bulun ( *Azure_Digital_Twins_custom_Swaggers__Logic_Apps_connector_ \logicapps \...\digitaltwins.js* ) ve *Aç* ' a basın.
* **Genel bilgiler**
    - Simge: dilediğiniz simgeyi karşıya yükleyin
    - Simge arka plan rengi: renk için ' #xxxxxx ' biçiminde onaltılık kod girin.
    - Açıklama: istediğiniz değerleri girin.
    - Düzen: HTTPS (varsayılan olarak bırakın)
    - Ana bilgisayar: Azure dijital TWINS örneğinizin *konak adı* .
    - Temel URL:/(varsayılanı bırak)

Ardından, bir sonraki yapılandırma adımına devam etmek için pencerenin alt kısmındaki *güvenlik* düğmesine basın.

:::image type="content" source="media/how-to-integrate-logic-apps/configure-next.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

Güvenlik adımında, bu bilgileri *Düzenle* ve Yapılandır ' a basın.
* **Kimlik doğrulama türü** : OAuth 2,0
* **OAuth 2,0** :
    - Kimlik sağlayıcısı: Azure Active Directory
    - İstemci KIMLIĞI: Azure AD uygulama kaydınız için *uygulama (istemci) kimliği*
    - İstemci gizli dizisi: Azure AD uygulama kaydınız için [*Önkoşullar*](#prerequisites) bölümünde oluşturduğunuz *istemci gizli* dizisi
    - Oturum açma URL 'SI: https://login.windows.net (varsayılanı bırak)
    - Kiracı KIMLIĞI: Azure AD uygulama kaydınız için *Dizin (kiracı) kimliği*
    - Kaynak URL 'SI: 0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    - Kapsam: Directory. AccessAsUser. All
    - Yeniden yönlendirme URL 'SI: (şimdilik varsayılan olarak bırakın)

Yeniden yönlendirme URL 'SI alanının, *YÖNLENDIRME URL 'sini oluşturmak için özel bağlayıcıyı kaydetdüğüne* emin olduğunu unutmayın. Bunu şimdi, bağlayıcı ayarlarınızı onaylamak için bölmenin üst kısmında bulunan *güncelleştirme bağlayıcısını* vurarak yapın.

:::image type="content" source="media/how-to-integrate-logic-apps/update-connector.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

<!-- Success message? didn't see one -->

Yeniden yönlendirme URL 'SI alanına dönün ve oluşturulan değeri kopyalayın. Bunu bir sonraki adımda kullanacaksınız.

:::image type="content" source="media/how-to-integrate-logic-apps/copy-redirect-url.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

Bu, bağlayıcınızı oluşturmak için gereken tüm bilgiler (tanım adımına geçmiş güvenlik 'e devam etmeniz gerekmez). *Düzenle Logic Apps özel bağlayıcı* bölmesini kapatabilirsiniz.

>[!NOTE]
>İlk olarak *düzenlemeye* vurduysanız bağlayıcının genel bakış sayfasına geri dönmek için, yeniden *Düzenle* ' nin, yapılandırma seçimlerinizi girme işleminin tamamını yeniden başlatdığına göz atın. Bu, sizin tarafınızdan son yaptığınız zamandan itibaren doldurmayacak, bu nedenle güncelleştirilmiş bir yapılandırmayı değiştirilen değerlerle kaydetmek istiyorsanız, diğer tüm değerleri yeniden girmeniz ve bunların üzerine yazılmaları önlenir.

### <a name="grant-connector-permissions-in-the-azure-ad-app"></a>Azure AD uygulamasında bağlayıcı izinleri verme

Daha sonra, Azure AD uygulama kaydınıza bağlayıcı izinleri vermek için son adımda kopyaladığınız özel bağlayıcının *yeniden YÖNLENDIRME URL* değerini kullanın.

Azure portal [uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) sayfasına gidin ve listeden kaydınızı seçin. 

Kayıt menüsündeki *kimlik doğrulaması* altında bir URI ekleyin.

:::image type="content" source="media/how-to-integrate-logic-apps/add-uri.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var"::: 

Özel bağlayıcının *yeniden YÖNLENDIRME URL* 'sini yeni alana girin ve *Kaydet* simgesine basın.

:::image type="content" source="media/how-to-integrate-logic-apps/save-uri.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

Artık Azure dijital TWINS API 'Lerine erişebilen bir özel bağlayıcı ayarlamayı tamamladınız. 

## <a name="create-logic-app"></a>Mantıksal uygulama oluşturma

Ardından, Azure dijital TWINS güncelleştirmelerini otomatikleştirmek için yeni bağlayıcınızı kullanacak bir mantıksal uygulama oluşturacaksınız.

[Azure Portal](https://portal.azure.com), Portal arama çubuğunda *Logic Apps* ' i arayın. Bunu seçtiğinizde *Logic Apps* sayfasına uygulamanız gerekir. Yeni bir mantıksal uygulama oluşturmak için *mantıksal uygulama oluştur* düğmesine basın.

:::image type="content" source="media/how-to-integrate-logic-apps/create-logic-app.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

Aşağıdaki *mantıksal uygulama* sayfasında, aboneliğinizi ve kaynak grubunuzu girin. Ayrıca, mantıksal uygulamanız için bir ad seçin ve dağıtım konumunu seçin.

_Gözden geçir + oluştur_ düğmesine basın.

Böylece, ayrıntılarınızı gözden geçirebileceğiniz ve kaynağınızı oluşturmak için en altta *Oluştur* düğmesine basarak *Gözden geçirme + oluştur* sekmesine gidersiniz.

Mantıksal uygulama için dağıtım sayfasına yönlendirilirsiniz. Dağıtım tamamlandığında, *Logic Apps tasarımcısına* devam etmek Için *Kaynağa Git* düğmesine basın. Bu, iş akışının mantığını dolduracaktır.

### <a name="design-workflow"></a>Tasarım iş akışı

*Logic Apps tasarımcısında* , *ortak bir tetikleyiciden başla* ' nın altında _**yinelenme**_ ' yi seçin.

:::image type="content" source="media/how-to-integrate-logic-apps/logic-apps-designer-recurrence.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

Aşağıdaki *Logic Apps tasarımcı* sayfasında **yinelenme** sıklığını *ikinci* olarak değiştirin, böylece olay 3 saniyede bir tetiklenir. Bu, daha sonra çok uzun süre beklemek zorunda kalmadan sonuçları görmeyi kolaylaştırır.

*Yeni adım* ' a basın.

Bu *işlem bir eylem Seç* kutusunu açar. *Özel* sekmeye geçiş yapın. Özel bağlayıcınızı daha önce üstteki kutuda görmeniz gerekir.

:::image type="content" source="media/how-to-integrate-logic-apps/custom-action.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

Bu bağlayıcının içerdiği API 'lerin listesini göstermek için bunu seçin. **DigitalTwins_Add** seçmek için arama çubuğunu kullanın veya listede ilerleyin. (Bu makalede kullanılan API budur, ancak aynı zamanda başka bir API 'YI Logic Apps bağlantı için geçerli bir seçenek olarak seçebilirsiniz).

Bağlayıcıya bağlanmak için Azure kimlik bilgilerinizle oturum açmanız istenebilir. *Istenen izinlere* sahip bir iletişim kutusu alırsanız, uygulamanıza izin vermek ve kabul etmek için istemleri izleyin.

Yeni *DigitalTwinsAdd* kutusunda, alanları aşağıdaki gibi girin:
* _kimlik_ : örnekte mantıksal uygulamanın güncelleştirilmesini istediğiniz dijital Ikizi 'ıN *ikizi kimliğini* doldurursunuz.
* _ikizi_ : Bu alan, seçilen API isteğinin gerektirdiği gövdeye girebileceğiniz yerdir. *Digitaltwınsupdate* için bu gövde JSON Patch kodu biçimindedir. İkizi 'nizi güncelleştirmek için bir JSON Patch yapılandırma hakkında daha fazla bilgi için *nasıl yapılır: dijital TWINS yönetme* konusunun [dijital ikizi güncelleştirme](how-to-manage-twin.md#update-a-digital-twin) bölümüne bakın.
* _api sürümü_ : en son API sürümü. Şu anda bu değer *2020-10-31* ' dir.

Logic Apps tasarımcısında *Kaydet* ' i ziyaret edin.

Aynı pencerede _+ yeni adım_ ' ı seçerek diğer işlemleri seçebilirsiniz.

:::image type="content" source="media/how-to-integrate-logic-apps/save-logic-app.png" alt-text="Azure AD uygulama kaydının Portal görünümü. Kaynak menüsündeki ' sertifikalar ve gizlilikler ' etrafında bir vurgulama ve sayfada ' yeni istemci gizli dizisi ' etrafında vurgu var":::

## <a name="query-twin-to-see-the-update"></a>Güncelleştirmeyi görmek için ikizi sorgulama

Mantıksal uygulamanız oluşturulduktan sonra, Logic Apps tasarımcısında tanımladığınız ikizi Update olayı her üç saniyede bir tekrarda gerçekleşmelidir. Bu, üç saniyeden sonra, ikizi sorgunuzu sorgulayabilmeniz ve yeni düzeltme eki uygulanmış değerlerinizin yansıtıldığını görmeniz gerekir.

İkizi 'nizi tercih ettiğiniz Yöntem (örneğin, [özel bir istemci uygulaması](tutorial-command-line-app.md), [Azure Digital TWINS Explorer örnek uygulaması](/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/), [SDK 'Lar ve API 'ler](how-to-use-apis-sdks.md)veya [CLI](how-to-use-cli.md)) aracılığıyla sorgulayabilirsiniz. 

Azure dijital TWINS örneğinizi sorgulama hakkında daha fazla bilgi için bkz. [*nasıl yapılır: ikizi grafiğini sorgulama*](how-to-query-graph.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure dijital TWINS örneğiniz içindeki bir ikizi, verdiğiniz bir düzeltme ekiyle düzenli olarak güncelleştiren bir mantıksal uygulama oluşturdunuz. Örneğiniz üzerinde çeşitli eylemler için Logic Apps oluşturmak üzere özel bağlayıcıdaki diğer API 'Leri seçmeyi deneyebilirsiniz.

Kullanılabilir API işlemleri ve ihtiyaç duydukları ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [*nasıl yapılır: Azure Digital TWINS API 'leri ve SDK 'Larını kullanma*](how-to-use-apis-sdks.md).