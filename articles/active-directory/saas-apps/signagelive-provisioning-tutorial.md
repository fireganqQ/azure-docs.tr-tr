---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için Signagelive yapılandırma | Microsoft Docs'
description: Signagelive 'ye Kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamak üzere Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/23/2019
ms.author: Zhchia
ms.openlocfilehash: 10ad06041e8136b5661b1b1ff487cd4d3b0f5153
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94358414"
---
# <a name="tutorial-configure-signagelive--for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için Signagelive yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları Signagelive 'e otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için Signagelive ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Azure AD kiracısı.
* [Bir Signagelive kiracısı](https://signagelive.com/pricing/)
* Yönetici izinlerine sahip Signagelive içinde bir kullanıcı hesabı.

## <a name="assigning-users-to-signagelive"></a>Signagelive 'e Kullanıcı atama   

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Signagelive 'e erişmesi gerektiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları Signagelive 'e atayabilirsiniz:
* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-signagelive"></a>Signagelive 'e Kullanıcı atamaya yönelik önemli ipuçları   

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için Signagelive 'e tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı Signagelive 'e atarken atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="setup-signagelive--for-provisioning"></a>Sağlama için kurulum Signagelive

Signagelive 'i Azure AD ile otomatik Kullanıcı sağlaması için yapılandırmadan önce, Signagelive üzerinde SCıM sağlamasını etkinleştirmeniz gerekir.

  SCıM sağlamasını yapılandırmak için gereken gizli belirteci almak için  [Signagelive](mailto:development@signagelive.com) 'e ulaşın.

## <a name="add-signagelive-from-the-gallery"></a>Galeriden Signagelive ekleme

Signagelive 'i Azure AD ile otomatik Kullanıcı sağlaması için yapılandırmak üzere, Azure AD Uygulama Galerisi 'nden yönetilen SaaS uygulamaları listenize Signagelive eklemeniz gerekir.

**Azure AD Uygulama Galerisi 'nden Signagelive eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** ' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** ' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Signagelive** yazın, sonuçlar panelinde **Signagelive** ' yi seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuç listesinde Signagelive](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-signagelive"></a>Signagelive 'e otomatik Kullanıcı sağlamayı yapılandırma    

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak Signagelive içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

> [!TIP]
>  Ayrıca, [Signagelive çoklu oturum açma öğreticisinde](Signagelive-tutorial.md)sunulan yönergeleri Izleyerek, SIGNAGELIVE için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-automatic-user-provisioning-for-signagelive--in-azure-ad"></a>Azure AD 'de Signagelive için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar** 'ı ve ardından **Tüm uygulamalar** 'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Signagelive** ' yi seçin.

    ![Uygulamalar listesindeki Signagelive bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Sağlama seçeneğinin kullanıma aldığı yönetim seçeneklerinin ekran görüntüsü.](common/provisioning.png)

4. **Hazırlama Modu** 'nu **Otomatik** olarak ayarlayın.

    ![Otomatik seçeneği olarak adlandırılan sağlama modu açılan listesinin ekran görüntüsü.](common/provisioning-automatic.png)

5. Yönetici kimlik bilgileri bölümünün altında, ` https://samlapi.signagelive.com/scim/v2` **kiracı URL 'sini** girin. **Gizli belirteç** alanı ' nda, mühendislik geliştirme ekibi tarafından sunulan **taşıyıcı belirteç** değerini girin. Azure AD 'nin Signagelive 'e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Signagelive hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.
    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder** ' i işaretleyin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet** ’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Signagelive olarak eşitler** ' ı seçin.

    ![Eşlemeleri Azure Active Directory Kullanıcıları Signagelive seçeneğiyle Synchronize seçeneği ile eşlemeler bölümünün ekran görüntüsü.](media/signagelive-provisioning-tutorial/usermapping.png)

9. **Öznitelik eşleme** bölümünde Azure AD 'den Signagelive 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için Signagelive içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Yedi eşleşme görüntülenirken öznitelik eşlemeleri bölümünün ekran görüntüsü.](media/signagelive-provisioning-tutorial/userattribute.png)

10. **Eşlemeler** bölümünde **Azure Active Directory grubunu Signagelive olarak eşitler** ' ı seçin.

    ![Azure Active Directory grubu ile Signagelive seçeneğini eşitler seçeneği verilen eşlemeler bölümünün ekran görüntüsü.](media/signagelive-provisioning-tutorial/groupmapping.png)

11. **Öznitelik eşleme** bölümünde Azure AD 'den Signagelive 'e eşitlenen grup özniteliklerini gözden geçirin. **Eşlenen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için Signagelive içindeki grup hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Üç eşleme görüntülenirken öznitelik eşlemeleri bölümünün ekran görüntüsü.](media/signagelive-provisioning-tutorial/groupattribute.png)

12. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

13. Signagelive için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki istenen değerleri seçerek Signagelive için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

15. Hazırlama işlemini başlatmak için **Kaydet** 'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin sonraki eşitlemeler daha uzun sürer. Kullanıcıların ve/veya grupların sağlaması için ne kadar süreceğine ilişkin daha fazla bilgi için bkz. [kullanıcıları sağlamak için ne kadar sürer](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users). 

İlerlemeyi izlemek ve Signagelive üzerinde Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama etkinliği raporunuzun bağlantılarını izlemek için **geçerli durum** bölümünü kullanabilirsiniz. Daha fazla bilgi için bkz. [Kullanıcı hazırlama durumunu denetleme](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md). Azure AD sağlama günlüklerini okumak için bkz. [Otomatik Kullanıcı hesabı sağlama hakkında raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
