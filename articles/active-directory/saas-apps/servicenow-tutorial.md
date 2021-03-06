---
title: 'Öğretici: ServiceNow ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve ServiceNow arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/09/2020
ms.author: jeedes
ms.openlocfilehash: c90234249f3cf7eb6ed4793110d61e1f8190ed60
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99092641"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-servicenow"></a>Öğretici: ServiceNow ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide ServiceNow 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. ServiceNow 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de ServiceNow 'a erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla ServiceNow için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portal.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4Jao6]

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Bir ServiceNow çoklu oturum açma (SSO) aboneliği etkin.
* ServiceNow için ServiceNow örneği veya kiracısı Calgary, Kingston, Londra, Madrid, New York, Orlando ve Paris sürümlerini veya üstünü destekler.
* ServiceNow Express için ServiceNow Express, Helsinki sürümü veya üzeri bir örnek.
* ServiceNow kiracısında [birden çok sağlayıcı çoklu oturum açma eklentisi](https://old.wiki/index.php/Multiple_Provider_Single_Sign-On#gsc.tab=0) etkin olmalıdır.
* Otomatik yapılandırma için ServiceNow için Multi-Provider eklentisini etkinleştirin.
* ServiceNow Classic (mobil) uygulamasını yüklemek için uygun mağazaya gidin ve ServiceNow klasik uygulamasını arayın. Ardından indirin.

> [!NOTE]
> Bu tümleştirme Ayrıca Azure AD ABD kamu bulut ortamından kullanılabilir. Bu uygulamayı Azure AD ABD kamu bulutu uygulama galerisinde bulabilir ve bunu ortak buluttan yaptığınız şekilde yapılandırabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. 

* ServiceNow, **SP** tarafından başlatılan SSO 'yu destekler.

* ServiceNow [Otomatik Kullanıcı sağlamayı](servicenow-provisioning-tutorial.md)destekler.

* SSO 'yu etkinleştirmek için ServiceNow klasik (mobil) uygulamayı Azure AD ile yapılandırabilirsiniz. Hem Android hem de iOS kullanıcılarını destekler. Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

## <a name="add-servicenow-from-the-gallery"></a>Galeriden ServiceNow ekleme

ServiceNow 'ın tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden ServiceNow 'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabı kullanarak veya kişisel bir Microsoft hesabı kullanarak Azure portal oturum açın.
1. Sol bölmede **Azure Active Directory** hizmeti seçin.
1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **ServiceNow** yazın.
1. Sonuçlar panelinden **ServiceNow** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-servicenow"></a>ServiceNow için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak ServiceNow Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve ServiceNow içindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu ServiceNow ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso) .
    1. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için [bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user) .
    1. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere [Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user) .
    1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [ServiceNow Express Için Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso-for-servicenow-express) .
2. Uygulama tarafında SSO ayarlarını yapılandırmak için [ServiceNow 'ı yapılandırın](#configure-servicenow) .
    1. ServiceNow 'da, kullanıcının Azure AD gösterimine bağlı olarak bir [ServiceNow test kullanıcısı oluşturun](#create-servicenow-test-user) .
    1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için [ServiceNow Express SSO 'Yu yapılandırın](#configure-servicenow-express-sso) .  
3. Yapılandırmanın çalışıp çalışmadığını doğrulamak için [test SSO 'su](#test-sso) .
4. Yapılandırmanın çalışıp çalışmadığını doğrulamak için [ServiceNow Classic (mobil) Için test SSO 'su](#test-sso-for-servicenow-classic-mobile) .

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal **ServiceNow** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun. **Çoklu oturum açma** seçeneğini belirleyin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesini seçin.

   ![Kalem simgesi vurgulanmış şekilde, SAML sayfası ile tek Sign-On ayarlama ekran görüntüsü](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Oturum açma URL 'si**' nde, aşağıdaki kalıbı kullanan bir URL girin:`https://<instancename>.service-now.com/navpage.do`

    b. **Tanımlayıcı (VARLıK kimliği)** alanında, aşağıdaki kalıbı kullanan bir URL girin:`https://<instance-name>.service-now.com`

    c. **Yanıt URL 'si** IÇIN aşağıdaki URL desenlerinden birini girin:

    | Yanıt URL'si|
    |----------|
    | `https://<instancename>.service-now.com/navpage.do` |
    | `https://<instancename>.service-now.com/customer.do` | 

    d. **Oturum kapatma URL 'si**' nde, aşağıdaki kalıbı kullanan bir URL girin:`https://<instancename>.service-now.com/navpage.do`

    > [!NOTE]
    > Tanımlayıcı değere "/" eklenirse, lütfen el ile kaldırın.

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, Öğreticinin ilerleyen kısımlarında açıklanan gerçek oturum açma URL 'SI, yanıt URL 'si, oturum kapatma URL 'SI ve tanımlayıcı ile güncelleştirmeniz gerekir. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)**. 

   ![Yükleme vurgulanmış olarak SAML Imzalama sertifikası bölümünün ekran görüntüsü](common/certificatebase64.png)

   a. **Uygulama Federasyon meta veri URL 'sini** kopyalamak için Kopyala düğmesini seçin ve Not defteri 'ne yapıştırın. Bu URL daha sonra öğreticide kullanılacaktır.

    b. Sertifikayı indirmek için **İndir** ' i seçin **(base64)** ve ardından sertifika dosyasını bilgisayarınıza kaydedin.

1. **ServiceNow ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

   ![URL vurgulanmış şekilde ServiceNow bölümünün ayarlandığı ekran görüntüsü](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portal olarak B. Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure Portal sol bölmeden, kullanıcılar **Azure Active Directory**  >    >  **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** için girin `B.Simon` .  
   1. **Kullanıcı adı** için öğesini girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster**' i seçin ve ardından **parola** kutusunda gösterilen değeri yazın.
   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, ServiceNow 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure Portal **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **ServiceNow**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı ekle**'yi seçin. **Atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından **Seç**' i seçin.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

### <a name="configure-azure-ad-sso-for-servicenow-express"></a>ServiceNow Express için Azure AD SSO 'yu yapılandırma

1. [Azure Portal](https://portal.azure.com/), **ServiceNow** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Tek oturum açma vurgulanmış şekilde ServiceNow uygulama tümleştirme sayfasının ekran görüntüsü](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![SAML vurgulanmış bir çoklu oturum açma yöntemi seçme ekran görüntüsü](common/select-saml-option.png)

3. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** iletişim kutusunu açmak için kalem simgesini seçin.

    ![Kalem simgesi vurgulanmış şekilde SAML sayfası ile çoklu oturum açmayı ayarlama ekran görüntüsü](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Oturum açma URL 'si** için aşağıdaki kalıbı kullanan bir URL girin:`https://<instancename>.service-now.com/navpage.do`

    b. **Tanımlayıcı (VARLıK kimliği)** için aşağıdaki kalıbı kullanan bir URL girin:`https://<instance-name>.service-now.com`

    c. **Yanıt URL 'si** IÇIN aşağıdaki URL 'den birini girin:

    | Yanıt URL'si |
    |-----------|
    | `https://<instancename>.service-now.com/navpage.do` |
    | `https://<instancename>.service-now.com/customer.do` |

    d. **Oturum kapatma URL 'si**' nde, aşağıdaki kalıbı kullanan bir URL girin:`https://<instancename>.service-now.com/navpage.do`
    
    > [!NOTE]
    > Tanımlayıcı değere "/" eklenirse, lütfen el ile kaldırın.

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, Öğreticinin ilerleyen kısımlarında açıklanan gerçek oturum açma URL 'SI, yanıt URL 'si, oturum kapatma URL 'SI ve tanımlayıcı ile güncelleştirmeniz gerekir. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i, gereksiniminize göre belirtilen seçeneklerden indirmek için **İndir** ' i seçin. Bu dosyayı bilgisayarınıza kaydedin.

    ![Yükleme vurgulanmış olarak SAML Imzalama sertifikası bölümünün ekran görüntüsü](common/certificatebase64.png)

6. Azure AD 'nin SAML tabanlı kimlik doğrulaması için ServiceNow 'ı otomatik olarak yapılandırmasını sağlayabilirsiniz. Bu hizmeti etkinleştirmek için **ServiceNow ayarla** bölümüne gidin ve **adım adım yönergeleri görüntüle** ' yi seçerek **oturum açma yapılandırma** penceresini açın.

    ![Adım adım yönergeleri vurgulayarak ServiceNow bölümünün ayarlandığı ekran görüntüsü](./media/servicenow-tutorial/tutorial-servicenow-configure.png)

7. **Oturum açma yapılandırma** formunda ServiceNow örnek adınızı, Yönetici Kullanıcı adınızı ve yönetici parolanızı girin. **Şimdi Yapılandır**' ı seçin. Belirtilen Yönetici Kullanıcı adı, bunun çalışması için ServiceNow 'da atanmış **security_admin** rolüne sahip olmalıdır. Aksi takdirde, ServiceNow 'ı bir SAML kimlik sağlayıcısı olarak Azure AD 'yi kullanacak şekilde el ile yapılandırmak için **Çoklu oturum açmayı el ile yapılandır**' ı seçin. Oturum **kapatma URL 'sini, Azure ad tanımlayıcısını ve oturum açma URL** 'Sini hızlı başvuru bölümünden kopyalayın.

    ![Yapılandırma artık vurgulanmış şekilde, oturum açma formunun yapılandırma ekran görüntüsü](./media/servicenow-tutorial/configure.png "Uygulama URL 'sini Yapılandır")

## <a name="configure-servicenow"></a>ServiceNow 'ı yapılandırma

1. ServiceNow uygulamanızda yönetici olarak oturum açın.

1. **Tümleştirme-çoklu sağlayıcı çoklu oturum açma yükleyici** eklentisini aşağıdaki adımları izleyerek etkinleştirin:

    a. Sol bölmede, arama kutusundan **sistem tanımı** bölümünü arayın ve ardından **Eklentiler**' i seçin.

    ![Sistem tanımı bölümünün ekran görüntüsü, sistem tanımı ve eklentileri vurgulandı](./media/servicenow-tutorial/tutorial-servicenow-03.png "Eklentiyi etkinleştir")

    b. Tümleştirme araması **-birden çok sağlayıcı çoklu oturum açma yükleyicisi**.

     ![Tümleştirme ile sistem eklentileri sayfasının ekran görüntüsü-birden çok sağlayıcı tek Sign-On yükleyicisi vurgulanmış](./media/servicenow-tutorial/tutorial-servicenow-04.png "Eklentiyi etkinleştir")

    c. Eklentiyi seçin. Sağ tıklayın ve **Etkinleştir/Yükselt**' i seçin.

     ![Etkinleştirme/yükseltme vurgulanmış olarak, eklenti sağ tıklama menüsünün ekran görüntüsü](./media/servicenow-tutorial/tutorial-activate.png "Eklentiyi etkinleştir")

    d. **Etkinleştir**' i seçin.

     ![Etkin seçeneği vurgulanmış şekilde eklentiyi etkinleştirme iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/tutorial-activate-1.png "Eklentiyi etkinleştir")

1. Sol bölmede, arama çubuğundan **çok sağlayıcıya YÖNELIK SSO** bölümünü arayın ve ardından **Özellikler**' i seçin.

    ![Çok sağlayıcılı SSO ve Özellikler vurgulanmış şekilde çok sağlayıcılı SSO bölümünün ekran görüntüsü](./media/servicenow-tutorial/tutorial-servicenow-06.png "Uygulama URL 'sini Yapılandır")

1. **Birden çok sağlayıcı SSO özellikleri** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Birden çok sağlayıcı SSO özelliklerinin ekran görüntüsü iletişim kutusu](./media/servicenow-tutorial/ic7694981.png "Uygulama URL 'sini Yapılandır")

    * **Birden çok sağlayıcı SSO 'Yu etkinleştirmek** için **Evet**' i seçin.
  
    * **Kullanıcıların tüm kimlik sağlayıcılarından Kullanıcı tablosuna otomatik olarak Içeri aktarılmasını etkinleştirmek** için **Evet**' i seçin.

    * **Birden çok sağlayıcı SSO tümleştirmesi için hata ayıklama günlüğünü etkinleştirmek** için **Evet**' i seçin.

    * **Kullanıcı tablosundaki... Bu alan** için **e-posta** girin.
  
    * **Kaydet**’i seçin.

1. ServiceNow 'ı otomatik olarak veya el ile yapılandırabilirsiniz. ServiceNow 'ı otomatik olarak yapılandırmak için aşağıdaki adımları izleyin:

    1. Azure portal **ServiceNow** çoklu oturum açma sayfasına dönün.

    1. ServiceNow için tek tıklamayla yapılandırma hizmeti sunulmaktadır. Bu hizmeti etkinleştirmek için **ServiceNow yapılandırma** bölümüne gidip **ServiceNow** 'ı Yapılandır ' ı seçerek **oturum açma yapılandırma** penceresini açın.

        ![, Adım adım yönergeleri vurgulayarak ServiceNow ayarlama ekran görüntüsü](./media/servicenow-tutorial/tutorial-servicenow-configure.png)

    1. **Oturum açma yapılandırma** formunda ServiceNow örnek adınızı, Yönetici Kullanıcı adınızı ve yönetici parolanızı girin. **Şimdi Yapılandır**' ı seçin. Bunun çalışması için, belirtilen yönetici kullanıcı adının ServiceNow 'da atanmış **Güvenlik yönetici** rolü olmalıdır. Aksi takdirde, ServiceNow 'ı bir SAML kimlik sağlayıcısı olarak Azure AD 'yi kullanacak şekilde el ile yapılandırmak için **Çoklu oturum açmayı el ile yapılandır**' ı seçin. Hızlı başvuru bölümünde **oturum kapatma URL 'si, SAML VARLıK kimliği ve SAML çoklu oturum açma hizmeti URL 'sini** kopyalayın.

        ![Yapılandırma artık vurgulanmış şekilde, oturum açma formunun yapılandırma ekran görüntüsü](./media/servicenow-tutorial/configure.png "Uygulama URL 'sini Yapılandır")

    1. ServiceNow uygulamanızda yönetici olarak oturum açın.

       * Otomatik yapılandırmada, tüm gerekli ayarlar **ServiceNow** tarafında yapılandırılır, ancak **X. 509.440 sertifikası** varsayılan olarak etkinleştirilmez ve **tek Sign-On betik** değerini **MultiSSOv2_SAML2_custom** olarak verir. ServiceNow 'daki kimlik sağlayıcınızda el ile eşlemeniz gerekir. Şu adımları izleyin:

         1. Sol bölmede, arama kutusundan **çok sağlayıcıya YÖNELIK SSO** bölümünü arayın ve **kimlik sağlayıcıları**' nı seçin.

            ![Kimlik sağlayıcıları vurgulanmış şekilde çok sağlayıcılı SSO bölümünün ekran görüntüsü](./media/servicenow-tutorial/tutorial-servicenow-07.png "Çoklu oturum açmayı yapılandırma")

         1. Otomatik olarak oluşturulan kimlik sağlayıcısını seçin.

            ![Otomatik olarak oluşturulan kimlik sağlayıcısı vurgulanmış şekilde kimlik sağlayıcılarının ekran görüntüsü](./media/servicenow-tutorial/tutorial-servicenow-08.png "Çoklu oturum açmayı yapılandırma")

         1.  **Kimlik sağlayıcısı** bölümünde aşağıdaki adımları uygulayın:

             ![Kimlik sağlayıcısı bölümünün ekran görüntüsü](./media/servicenow-tutorial/automatic-config.png "Çoklu oturum açmayı yapılandırma")

               a. **Ad** için, yapılandırmanız için bir ad girin (örneğin, **Microsoft Azure federe çoklu oturum açma**).

               b. **ServiceNow giriş sayfası** değerini kopyalayıp Azure Portal **ServiceNow temel SAML yapılandırması** bölümünde **oturum açma URL 'sine** yapıştırın.

                > [!NOTE]
                > ServiceNow örneği giriş sayfası, **ServiceNow kiracı URL 'si** ve **/navpage.do** (örneğin: `https://fabrikam.service-now.com/navpage.do` ) birleşimi.

              c. **VARLıK kimliği/veren** değerini kopyalayın ve Azure Portal **ServiceNow temel SAML yapılandırması** bölümünde **tanımlayıcıya** yapıştırın.

              d. **NameID ilkesinin** değer olarak ayarlandığını onaylayın `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` . 

              e. **Gelişmiş** ' e tıklayın ve **tek Sign-On betik** değerini **MultiSSOv2_SAML2_custom** olarak verin.

         1. **X. 509.440 sertifikası** bölümüne gidin ve **Düzenle**' yi seçin.

             ![X. 509.440 sertifikası bölümünün ekran görüntüsü, vurgulanmış olarak Düzenle](./media/servicenow-tutorial/tutorial-servicenow-09.png "Çoklu oturum açmayı yapılandırma")

         1. Sertifikayı seçin ve sertifikayı eklemek için sağ ok simgesini seçin

            ![Sertifika ve sağ ok simgesi vurgulanmış şekilde koleksiyonun ekran görüntüsü](./media/servicenow-tutorial/tutorial-servicenow-11.png "Çoklu oturum açmayı yapılandırma")

          1. **Kaydet**’i seçin.

          1. Sayfanın sağ üst köşesinde **Bağlantıyı Sına**' yı seçin.

             ![Sınama bağlantısı vurgulanmış şekilde sayfanın ekran görüntüsü](./media/servicenow-tutorial/tutorial-activate-2.png "Eklentiyi etkinleştir")

             > [!NOTE]
             > Sınama bağlantısı başarısız olursa ve bu bağlantıyı etkinleştiremeyebilirsiniz, ServiceNow geçersiz kılma anahtarını sunar. Sys_properties girmeniz gerekir **.** **Arama GEZINTISINDE** listeleyin ve sistem özelliklerinin yeni sayfasını açar. Burada, ad **veri türü** ile **doğru/yanlış** olarak ayarlanmış ve sonra **değeri** **false** olarak ayarlanmış yeni bir özellik oluşturmanız **gerekir.**

             > ![Sınama bağlantısı sayfasının ekran görüntüsü](./media/servicenow-tutorial/test-connection-fail.png "Çoklu oturum açmayı yapılandırma")
        
          1. Kimlik bilgileriniz sorulduğunda, bunları girin. Aşağıdaki sayfayı görürsünüz. **SSO oturumu kapatma test sonuçları** hatası bekleniyor. Hatayı yoksayın ve  **Etkinleştir**' i seçin.

             ![Kimlik bilgileri sayfasının ekran görüntüsü](./media/servicenow-tutorial/servicenow-activate.png "Çoklu oturum açmayı yapılandırma")
  
1. **ServiceNow** 'ı el ile yapılandırmak için aşağıdaki adımları izleyin:

    1. ServiceNow uygulamanızda yönetici olarak oturum açın.

    1. Sol bölmede **kimlik sağlayıcıları**' nı seçin.

        ![Kimlik sağlayıcıları vurgulanmış şekilde çok sağlayıcılı SSO 'nun ekran görüntüsü](./media/servicenow-tutorial/tutorial-servicenow-07.png "Çoklu oturum açmayı yapılandırma")

    1. **Kimlik sağlayıcıları** Iletişim kutusunda **Yeni**' yi seçin.

        ![Yeni vurgulanmış olan kimlik sağlayıcıları iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694977.png "Çoklu oturum açmayı yapılandırma")

    1. **Kimlik sağlayıcıları** Iletişim kutusunda **SAML**' yi seçin.

        ![SAML vurgulanmış olarak kimlik sağlayıcıları iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694978.png "Çoklu oturum açmayı yapılandırma")

    1. **Kimlik sağlayıcısı meta verilerini Içeri aktar** bölümünde aşağıdaki adımları uygulayın:

        ![URL ve Içeri aktarma vurgulanmış şekilde, kimlik sağlayıcısı meta verilerinin Içeri aktarma ekran görüntüsü](./media/servicenow-tutorial/idp.png "Çoklu oturum açmayı yapılandırma")

        1. Azure portal kopyaladığınız **uygulama Federasyon meta veri URL 'sini** girin.

        1. **İçeri aktar**'ı seçin.

    1. IDP meta veri URL 'sini okur ve tüm alan bilgilerini doldurur.

        ![Kimlik sağlayıcısının ekran görüntüsü](./media/servicenow-tutorial/ic7694982.png "Çoklu oturum açmayı yapılandırma")

        a. **Ad** için, yapılandırmanız için bir ad girin (örneğin, **Microsoft Azure federe çoklu oturum açma**).

        b. **ServiceNow giriş sayfası** değerini kopyalayın. Azure portal **ServiceNow temel SAML yapılandırması** bölümünde **oturum açma URL 'sine** yapıştırın.

        > [!NOTE]
        > ServiceNow örneği giriş sayfası, **ServiceNow kiracı URL 'si** ve **/navpage.do** (örneğin: `https://fabrikam.service-now.com/navpage.do` ) birleşimi.

        c. **VARLıK kimliği/verenin** değerini kopyalayın. Azure portal **ServiceNow temel SAML yapılandırması** bölümünde **tanımlayıcıya** yapıştırın.

        d. **NameID ilkesinin** değer olarak ayarlandığını onaylayın `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified` .

        e. **Gelişmiş**'i seçin. **Kullanıcı alanına** **e-posta** girin.

        > [!NOTE]
        > Azure AD 'yi, SAML belirtecindeki benzersiz tanımlayıcı olarak Azure AD kullanıcı KIMLIĞINI (Kullanıcı asıl adı) veya e-posta adresini yaymaktır. Bunu, Azure Portal **ServiceNow**  >  **özniteliklerini**  >  **tek oturum açma** bölümüne giderek ve istenen alanı **NameIdentifier** özniteliğiyle eşleştirerek yapın. Azure AD 'de seçili öznitelik için depolanan değer (örneğin, Kullanıcı asıl adı), girilen alan için ServiceNow içinde depolanan değerle eşleşmelidir (örneğin, user_name).

        örneğin: Sayfanın sağ üst köşesindeki **test bağlantısı** ' nı seçin.

        > [!NOTE]
        > Sınama bağlantısı başarısız olursa ve bu bağlantıyı etkinleştiremeyebilirsiniz, ServiceNow geçersiz kılma anahtarını sunar. Sys_properties girmeniz gerekir **.** **Arama GEZINTISINDE** listeleyin ve sistem özelliklerinin yeni sayfasını açar. Burada, ad **veri türü** ile **doğru/yanlış** olarak ayarlanmış ve sonra **değeri** **false** olarak ayarlanmış yeni bir özellik oluşturmanız **gerekir.**

          > ![Test bağlantısının ekran görüntüsü](./media/servicenow-tutorial/test-connection-fail.png "Çoklu oturum açmayı yapılandırma")

        h. Kimlik bilgileriniz sorulduğunda, bunları girin. Aşağıdaki sayfayı görürsünüz. **SSO oturumu kapatma test sonuçları** hatası bekleniyor. Hatayı yoksayın ve  **Etkinleştir**' i seçin.

          ![Credentials](./media/servicenow-tutorial/servicenow-activate.png "Çoklu oturum açmayı yapılandırma")

### <a name="create-servicenow-test-user"></a>ServiceNow test kullanıcısı oluşturma

Bu bölümün amacı, ServiceNow 'da B. Simon adlı bir Kullanıcı oluşturmaktır. ServiceNow otomatik Kullanıcı sağlamayı destekler, varsayılan olarak etkindir.

> [!NOTE]
> El ile bir kullanıcı oluşturmanız gerekiyorsa [ServiceNow istemci destek ekibine](https://www.servicenow.com/support/contact-support.html)başvurun.

### <a name="configure-servicenow-express-sso"></a>ServiceNow Express SSO 'yu yapılandırma

1. ServiceNow Express uygulamanızda yönetici olarak oturum açın.

2. Sol bölmede **Çoklu oturum açma**' yı seçin.

    ![Tek Sign-On vurgulanmış ServiceNow Express uygulamasının ekran görüntüsü](./media/servicenow-tutorial/ic7694980ex.png "Uygulama URL 'sini Yapılandır")

3. **Çoklu oturum açma** iletişim kutusunda sağ üstteki yapılandırma simgesini seçin ve aşağıdaki özellikleri ayarlayın:

    ![Tek Sign-On iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694981ex.png "Uygulama URL 'sini Yapılandır")

    a. Sağ tarafta **birden çok sağlayıcı SSO 'Yu etkinleştir** .

    b. Sağ tarafta **birden çok sağlayıcı SSO tümleştirmesi için hata ayıklama günlüğünü etkinleştir** ' i açın.

    c. **Kullanıcı tablosundaki... alanında**, **user_name** girin.

4. **Çoklu oturum açma** Iletişim kutusunda **Yeni sertifika ekle**' yi seçin.

    ![Yeni sertifika ekle vurgulanarak tek Sign-On iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694973ex.png "Çoklu oturum açmayı yapılandırma")

5. **X. 509.440 sertifikaları** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![X. 509.440 sertifikaları iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694975.png "Çoklu oturum açmayı yapılandırma")

    a. **Ad** için, yapılandırmanız için bir ad girin (örneğin: **testsaml 2.0**).

    b. **Etkin**' i seçin.

    c. **Biçim** için **PEI**' yi seçin.

    d. **Tür** Için **güven deposu sertifikası**' nı seçin.

    e. Not defteri 'nde Azure portal indirilen base64 kodlu sertifikanızı açın. İçeriğini panonuza kopyalayın ve **Pee sertifikası** metin kutusuna yapıştırın.

    f. **Güncelleştir**’i seçin

6. **Çoklu oturum açma** Iletişim kutusunda **Yeni IDP Ekle**' yi seçin.

    ![Yeni IDP vurgulanmış şekilde tek Sign-On iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694976ex.png "Çoklu oturum açmayı yapılandırma")

7. **Yeni kimlik sağlayıcısı ekle** iletişim kutusunda, **kimlik sağlayıcısını Yapılandır** altında aşağıdaki adımları gerçekleştirin:

    ![Yeni kimlik sağlayıcısı ekle iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694982ex.png "Çoklu oturum açmayı yapılandırma")

    a. **Ad** için, yapılandırmanız için bir ad girin (örneğin: **SAML 2,0**).

    b. **Kimlik sağlayıcısı URL 'si** için, Azure Portal kopyaladığınız KIMLIK sağlayıcısı kimliği değerini yapıştırın.

    c. **Kimlik sağlayıcısının Authisteynliği** için, Azure Portal kopyaladığınız kimlik doğrulama isteği URL 'sinin değerini yapıştırın.

    d. **Kimlik sağlayıcısının SingleLogoutRequest** için, Azure Portal kopyaladığınız oturum kapatma URL 'sinin değerini yapıştırın.

    e. **Kimlik sağlayıcısı sertifikası** için, önceki adımda oluşturduğunuz sertifikayı seçin.

8. **Gelişmiş ayarlar**' ı seçin. **Ek kimlik sağlayıcısı özellikleri** altında aşağıdaki adımları gerçekleştirin:

    ![Gelişmiş ayarlar vurgulanmış şekilde yeni kimlik sağlayıcısı ekle iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694983ex.png "Çoklu oturum açmayı yapılandırma")

    a. **IDP 'Nin SingleLogoutRequest Için protokol bağlama** için **urn: oassıs: adlar: TC: SAML: 2.0: Bindings: http-Redirect** yazın.

    b. **NameID ilkesi** için **urn: oassıs: names: TC: SAML: 1.1: NameID-Format: Unspecified** girin.

    c. **Authncontextclassref yöntemi** için girin `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` .

    d. **AuthnContextClass oluşturmak** için, onu kapalı (seçilmemiş) olarak değiştirin.

9. **Ek hizmet sağlayıcısı özellikleri** altında aşağıdaki adımları gerçekleştirin:

    ![Çeşitli özellikler vurgulanmış şekilde yeni kimlik sağlayıcısı ekle iletişim kutusunun ekran görüntüsü](./media/servicenow-tutorial/ic7694984ex.png "Çoklu oturum açmayı yapılandırma")

    a. **ServiceNow giriş** sayfası Için ServiceNow örneği giriş sayfanız URL 'sini girin.

    > [!NOTE]
    > ServiceNow örneği giriş sayfası, **ServiceNow kiracı URL 'si** ve **/navpage.do** (örneğin: `https://fabrikam.service-now.com/navpage.do` ) birleşimi.

    b. **VARLıK kimliği/veren** Için, ServiceNow KIRACıNıZıN URL 'sini girin.

    c. **Hedef KITLE URI**'Si Için ServiceNow kiracınızın URL 'sini girin.

    d. **Saat eğme** için **60** girin.

    e. **Kullanıcı alanı** için **e-posta** girin.

    > [!NOTE]
    > Azure AD 'yi, SAML belirtecindeki benzersiz tanımlayıcı olarak Azure AD kullanıcı KIMLIĞINI (Kullanıcı asıl adı) veya e-posta adresini yaymaktır. Bunu, Azure Portal **ServiceNow**  >  **özniteliklerini**  >  **tek oturum açma** bölümüne giderek ve istenen alanı **NameIdentifier** özniteliğiyle eşleştirerek yapın. Azure AD 'de seçili öznitelik için depolanan değer (örneğin, Kullanıcı asıl adı), girilen alan için ServiceNow içinde depolanan değerle eşleşmelidir (örneğin, user_name).

    f. **Kaydet**’i seçin.

## <a name="test-sso"></a>Test SSO 'SU

Erişim panelinde ServiceNow kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız ServiceNow 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="test-sso-for-servicenow-classic-mobile"></a>ServiceNow Classic için test SSO 'SU (mobil)

1. **ServiceNow Classic (mobil)** uygulamanızı açın ve aşağıdaki adımları gerçekleştirin:

    a. Sağ alt köşedeki artı işaretini seçin.

    ![ServiceNow klasik uygulamasının, artı işareti vurgulanmış ekran görüntüsü](./media/servicenow-tutorial/test-03.png)

    b. ServiceNow örnek adınızı girip **devam**' ı seçin.

    ![Örnek Ekle sayfasının ekran görüntüsü, devam vurgulandı](./media/servicenow-tutorial/test-04.png)

    c. **Oturum aç** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Dış oturum açma vurgulanmış şekilde, oturum açma sayfasının ekran görüntüsü](./media/servicenow-tutorial/test-01.png)

    *  Gibi **Kullanıcı adı** girin B.simon@contoso.com .

    *  **Dış oturum açma kullan**' ı seçin. Oturum açmak için Azure AD sayfasına yönlendirilirsiniz.

    *  Kimlik bilgilerinizi girin. Herhangi bir üçüncü taraf kimlik doğrulama veya başka bir güvenlik özelliği etkinleştirilmişse, Kullanıcı buna uygun şekilde yanıt vermelidir. Uygulama **giriş sayfası** görüntülenir.

        ![Uygulama giriş sayfasının ekran görüntüsü](./media/servicenow-tutorial/test-02.png)

## <a name="next-steps"></a>Sonraki Adımlar

ServiceNow 'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimlerini zorunlu kılabilirsiniz. Oturum denetimleri koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-aad)
