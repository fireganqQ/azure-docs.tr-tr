---
title: Öğretici-Azure Active Directory Domain Services yönetilen bir etki alanı oluşturun | Microsoft Docs
description: Bu öğreticide, Azure portal kullanarak Azure Active Directory Domain Services yönetilen bir etki alanı oluşturmayı ve yapılandırmayı öğreneceksiniz.
author: justinha
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: tutorial
ms.date: 07/06/2020
ms.author: justinha
ms.openlocfilehash: 945c131394a0a3c6273f79044c8500a2feba70fe
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2020
ms.locfileid: "96618153"
---
# <a name="tutorial-create-and-configure-an-azure-active-directory-domain-services-managed-domain"></a>Öğretici: Azure Active Directory Domain Services yönetilen bir etki alanı oluşturma ve yapılandırma

Azure Active Directory Domain Services (Azure AD DS), Windows Server Active Directory ile tamamen uyumlu etki alanına katılması, Grup ilkesi, LDAP, Kerberos/NTLM kimlik doğrulaması gibi yönetilen etki alanı Hizmetleri sağlar. Etki alanı denetleyicilerini kendiniz dağıtmadan, yönetmeden ve düzeltme eki uygulamadan bu etki alanı hizmetlerini kullanırsınız. Azure AD DS, mevcut Azure AD kiracınızla tümleşir. Bu tümleştirme, kullanıcıların kurumsal kimlik bilgilerini kullanarak oturum açmasını sağlar ve kaynaklara erişimi güvenli hale getirmek için mevcut grupları ve Kullanıcı hesaplarını kullanabilirsiniz.

Ağ ve eşitleme için varsayılan yapılandırma seçeneklerini kullanarak yönetilen bir etki alanı oluşturabilir veya [Bu ayarları el ile tanımlayabilirsiniz][tutorial-create-instance-advanced]. Bu öğreticide, Azure portal kullanarak Azure AD DS yönetilen bir etki alanı oluşturmak ve yapılandırmak için varsayılan seçeneklerin nasıl kullanılacağı gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yönetilen bir etki alanı için DNS gereksinimlerini anlama
> * Yönetilen etki alanı oluşturma
> * Parola karması eşitlemeyi etkinleştirme

Azure aboneliğiniz yoksa başlamadan önce [bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için aşağıdaki kaynaklar ve ayrıcalıklar gereklidir:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Abonelikle ilişkili bir Azure Active Directory kiracısı, şirket içi bir dizinle veya yalnızca bulut diziniyle eşitlenir.
    * Gerekirse, [bir Azure Active Directory kiracı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin][associate-azure-ad-tenant].
* Azure AD DS 'yi etkinleştirmek için Azure AD kiracınızda *genel yönetici* ayrıcalıklarına sahip olmanız gerekir.
* Gerekli Azure AD DS kaynaklarını oluşturmak için Azure aboneliğinizde *katılımcı* ayrıcalıklarına sahip olmanız gerekir.

Azure AD DS için gerekli olmasa da, Azure AD kiracısı için [self servis parola sıfırlama (SSPR) yapılandırmanız][configure-sspr] önerilir. Kullanıcılar, SSPR olmadan parolalarını değiştirebilir, ancak SSPR, parolalarını unutmaları ve sıfırlanması gerekir.

> [!IMPORTANT]
> Yönetilen bir etki alanı oluşturduktan sonra, yönetilen etki alanını farklı bir kaynak grubuna, sanal ağa, aboneliğe vb. taşıyamazsınız. Yönetilen etki alanını dağıtırken en uygun aboneliği, kaynak grubunu, bölgeyi ve sanal ağı seçin.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Bu öğreticide, Azure portal kullanarak yönetilen etki alanını oluşturup yapılandırırsınız. Başlamak için öncelikle [Azure Portal](https://portal.azure.com)oturum açın.

## <a name="create-a-managed-domain"></a>Yönetilen etki alanı oluşturma

**Etkinleştirme Azure AD Domain Services** Sihirbazı 'nı başlatmak için aşağıdaki adımları izleyin:

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin.
1. Arama çubuğuna *etki alanı Hizmetleri* girin, sonra arama önerilerindeki *Azure AD Domain Services* seçin.
1. Azure AD Domain Services sayfasında **Oluştur**' u seçin. **Etkinleştirme Azure AD Domain Services** Sihirbazı başlatılır.
1. Yönetilen etki alanını oluşturmak istediğiniz Azure **aboneliğini** seçin.
1. Yönetilen etki alanının ait olacağı **kaynak grubunu** seçin. **Yeni oluştur** veya var olan bir kaynak grubunu Seç seçeneğini belirleyin.

Yönetilen bir etki alanı oluşturduğunuzda bir DNS adı belirlersiniz. Bu DNS adını seçerken bazı noktalar vardır:

* **Yerleşik etki alanı adı:** Varsayılan olarak, dizinin yerleşik etki alanı adı kullanılır (bir *. onmicrosoft.com* soneki). Yönetilen etki alanına internet üzerinden güvenli LDAP erişimini etkinleştirmek istiyorsanız, bu varsayılan etki alanı ile bağlantıyı güvenli hale getirmek için dijital bir sertifika oluşturamazsınız. Microsoft *. onmicrosoft.com* etki alanına sahip olduğundan, bir sertifika YETKILISI (CA) bir sertifika vermez.
* **Özel etki alanı adları:** En yaygın yaklaşım, genellikle zaten sahip olduğunuz ve yönlendirilebilir olan özel bir etki alanı adı belirtmektir. Yönlendirilebilir, özel bir etki alanı kullandığınızda, uygulamalarınızı desteklemek için gereken şekilde trafik doğru şekilde akabilir.
* **Yönlendirilemeyen etki alanı sonekleri:** Genellikle, *contoso. Local* gibi yönlendirilebilir olmayan bir etki alanı adı sonekini önlemenize tavsiye ederiz. *. Local* son eki yönlendirilebilir DEĞILDIR ve DNS çözümlenme sorunlarına neden olabilir.

> [!TIP]
> Özel bir etki alanı adı oluşturursanız, mevcut DNS ad alanları ile ilgilenin. Mevcut bir Azure veya şirket içi DNS ad alanından ayrı bir etki alanı adı kullanılması önerilir.
>
> Örneğin, *contoso.com* ADLı bir DNS ad alanınız varsa, *aaddscontoso.com* özel etki alanı adına sahip bir yönetilen etki alanı oluşturun. Güvenli LDAP kullanmanız gerekiyorsa, gerekli sertifikaları oluşturmak için bu özel etki alanı adını kaydetmeniz ve sahip olmanız gerekir.
>
> Ortamınızdaki diğer hizmetler için bazı ek DNS kayıtları veya ortamınızda var olan DNS adı alanları arasında koşullu DNS ileticileri oluşturmanız gerekebilir. Örneğin, kök DNS adını kullanarak bir siteyi barındıran bir Web sunucusu çalıştırırsanız, ek DNS girişleri gerektiren adlandırma çakışmaları olabilir.
>
> Bu öğreticiler ve nasıl yapılır makalelerinde, *aaddscontoso.com* özel etki alanı kısa bir örnek olarak kullanılır. Tüm komutlarda kendi etki alanı adınızı belirtin.

Aşağıdaki DNS adı kısıtlamaları da geçerlidir:

* **Etki alanı ön eki kısıtlamaları:** Ön eki 15 karakterden daha uzun olan yönetilen bir etki alanı oluşturamazsınız. Belirtilen etki alanı adınızın ön eki ( *aaddscontoso.com* etki alanı adında *aaddscontoso* gibi) 15 veya daha az karakter içermelidir.
* **Ağ adı çakışmaları:** Yönetilen etki alanınız için DNS etki alanı adı, sanal ağda zaten mevcut olmamalıdır. Özellikle, bir ad çakışmasına yol açacak aşağıdaki senaryoları kontrol edin:
    * Azure sanal ağında aynı DNS etki alanı adına sahip bir Active Directory etki alanınız zaten varsa.
    * Yönetilen etki alanını etkinleştirmeyi planladığınız sanal ağın, şirket içi ağınızla bir VPN bağlantısı varsa. Bu senaryoda, şirket içi ağınızda aynı DNS etki alanı adına sahip bir etki alanı olmadığından emin olun.
    * Azure sanal ağında bu adı taşıyan mevcut bir Azure bulut hizmetiniz varsa.

Yönetilen bir etki alanı oluşturmak için Azure portal *temel bilgiler* penceresindeki alanları doldurun:

1. Yönetilen etki alanınız için bir **DNS etki alanı adı** girin, önceki noktaları dikkate alarak.
1. Yönetilen etki alanının oluşturulması gereken Azure **konumunu** seçin. Azure Kullanılabilirlik Alanları destekleyen bir bölge seçerseniz, Azure AD DS kaynakları daha fazla artıklık için bölgelere dağıtılır.

    > [!TIP]
    > Kullanılabilirlik Alanları, Azure bölgesi içinde fiziksel olarak benzersiz konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Dayanıklılığı güvence altına almak için etkinleştirilmiş tüm bölgelerde en az üç ayrı alan vardır.
    >
    > Azure AD DS bölgeler arasında dağıtılacak şekilde yapılandırmanız için bir şey yoktur. Azure platformu, kaynakların bölge dağılımını otomatik olarak işler. Daha fazla bilgi edinmek ve bölge kullanılabilirliğini görmek için bkz. [Azure 'da kullanılabilirlik alanları nedir?][availability-zones]

1. **SKU** , oluşturabileceğiniz bir performans, yedekleme sıklığı ve en fazla orman güveni sayısını belirler. İş talepleriniz veya gereksinimleriniz değiştikçe, yönetilen etki alanı oluşturulduktan sonra SKU 'YU değiştirebilirsiniz. Daha fazla bilgi için bkz. [Azure AD DS SKU kavramları][concepts-sku].

    Bu öğretici için *Standart* SKU 'yu seçin.
1. *Orman* bir veya daha fazla etki alanını gruplamak için Active Directory Domain Services tarafından kullanılan mantıksal bir yapıdır. Varsayılan olarak, yönetilen bir etki alanı bir *Kullanıcı* Ormanı olarak oluşturulur. Bu tür bir orman, şirket içi AD DS ortamında oluşturulan kullanıcı hesapları da dahil olmak üzere Azure AD 'deki tüm nesneleri eşitler.

    *Kaynak* ormanı yalnızca doğrudan Azure AD 'de oluşturulan kullanıcıları ve grupları eşitler. *Kaynak* ormanları hakkında daha fazla bilgi için, bir tane kullanabilirsiniz ve şirket içi AD DS etki alanlarıyla orman güvenleri oluşturma hakkında daha fazla bilgi için bkz. [Azure AD DS kaynak ormanları genel bakış][resource-forests].

    Bu öğreticide, bir *Kullanıcı* ormanı oluşturmayı seçin.

    ![Azure AD Domain Services yönetilen bir etki alanı için temel ayarları yapılandırma](./media/tutorial-create-instance/basics-window.png)

Hızlı bir şekilde yönetilen etki alanı oluşturmak için, ek varsayılan yapılandırma seçeneklerini kabul etmek için **gözden geçir + oluştur** seçeneğini belirleyebilirsiniz. Bu oluşturma seçeneğini belirlediğinizde aşağıdaki varsayılanlar yapılandırılır:

* *10.0.2.0/24* IP adresi aralığını kullanan *aeklemeleri-VNET* adlı bir sanal ağ oluşturur.
* *10.0.2.0/24* IP adresi aralığını kullanarak *aeklemesine-subnet* adlı bir alt ağ oluşturur.
* Azure AD 'deki *Tüm* kullanıcıları yönetilen etki alanına eşitler.

Bu varsayılan yapılandırma seçeneklerini kabul etmek için **gözden geçir + oluştur** ' u seçin.

## <a name="deploy-the-managed-domain"></a>Yönetilen etki alanını dağıtma

Sihirbazın **Özet** sayfasında, yönetilen etki alanınız için yapılandırma ayarlarını gözden geçirin. Değişiklik yapmak için sihirbazın herhangi bir adımına geri dönebilirsiniz. Yönetilen bir etki alanını bu yapılandırma seçeneklerini kullanarak tutarlı bir şekilde farklı bir Azure AD kiracısına yeniden dağıtmak için, **Otomasyon için bir şablon da indirebilirsiniz**.

1. Yönetilen etki alanını oluşturmak için **Oluştur**' u seçin. Azure AD DS yönetilen oluşturulduktan sonra DNS adı veya sanal ağ gibi belirli yapılandırma seçeneklerinin değiştirilemeytiğine ilişkin bir Note görüntülenir. Devam etmek için **Tamam**' ı seçin.
1. Yönetilen etki alanınızı sağlama işlemi bir saate kadar sürebilir. Portalda Azure AD DS dağıtımınızın ilerlemesini gösteren bir bildirim görüntülenir. Dağıtım için ayrıntılı ilerlemeyi görmek üzere bildirimi seçin.

    ![Dağıtımın Azure portal bildirim devam ediyor](./media/tutorial-create-instance/deployment-in-progress.png)

1. Bu sayfa, dizininizde yeni kaynakların oluşturulması dahil olmak üzere dağıtım işlemindeki güncelleştirmelerle yüklenir.
1. Kaynak grubunuzu ( *Myresourcegroup* gibi) seçin ve ardından *aaddscontoso.com* gibi Azure kaynakları listesinden yönetilen etki alanınızı seçin. **Genel bakış** sekmesi, yönetilen etki alanının şu anda *dağıtmakta* olduğunu gösterir. Yönetilen etki alanını, tam olarak sağlanana kadar yapılandıramazsınız.

    ![Sağlama durumu sırasında etki alanı Hizmetleri durumu](./media/tutorial-create-instance/provisioning-in-progress.png)

1. Yönetilen etki alanı tam olarak sağlandığında **genel bakış** sekmesi, etki alanı durumunu *çalışıyor* olarak gösterir.

    ![Başarılı bir şekilde sağlandıktan sonra etki alanı Hizmetleri durumu](./media/tutorial-create-instance/successfully-provisioned.png)

> [!IMPORTANT]
> Yönetilen etki alanı, Azure AD kiracınızla ilişkilendirilir. Azure AD DS, sağlama işlemi sırasında, Azure AD kiracısında *etki alanı denetleyicisi Hizmetleri* ve *AzureActiveDirectoryDomainControllerServices* adlı iki kurumsal uygulama oluşturur. Bu kurumsal uygulamaların, yönetilen etki alanınızı hizmetine yönelik olması gerekir. Bu uygulamaları silmeyin.

## <a name="update-dns-settings-for-the-azure-virtual-network"></a>Azure sanal ağı için DNS ayarlarını güncelleştirme

Azure AD DS başarıyla dağıtıldı, artık sanal ağı diğer bağlı VM 'Lerin ve uygulamaların yönetilen etki alanını kullanmasına izin verecek şekilde yapılandırın. Bu bağlantıyı sağlamak için, sanal ağınızın DNS sunucusu ayarlarını, yönetilen etki alanının dağıtıldığı iki IP adresini işaret etmek üzere güncelleştirin.

1. Yönetilen etki alanınız için **genel bakış** sekmesi bazı **gerekli yapılandırma adımlarını** gösterir. İlk yapılandırma adımı, sanal ağınızın DNS sunucusu ayarlarını güncelleştiryöneliktir. DNS ayarları doğru yapılandırıldıktan sonra bu adım artık gösterilmemektedir.

    Listelenen adresler, sanal ağda kullanılacak etki alanı denetleyicileridir. Bu örnekte, bu adresler *10.0.2.4* ve *10.0.2.5*. Daha sonra bu IP adreslerini **Özellikler** sekmesinde bulabilirsiniz.

    ![Sanal ağınız için DNS ayarlarını Azure AD Domain Services IP adresleriyle yapılandırın](./media/tutorial-create-instance/configure-dns.png)

1. Sanal ağın DNS sunucusu ayarlarını güncelleştirmek için **Yapılandır** düğmesini seçin. DNS ayarları, sanal ağınız için otomatik olarak yapılandırılır.

> [!TIP]
> Önceki adımlarda var olan bir sanal ağı seçtiyseniz, ağa bağlı tüm VM 'Ler, yeniden başlatmanın ardından yeni DNS ayarlarını alır. Azure portal, Azure PowerShell veya Azure CLı kullanarak VM 'Leri yeniden başlatabilirsiniz.

## <a name="enable-user-accounts-for-azure-ad-ds"></a>Azure AD DS için Kullanıcı hesaplarını etkinleştirme

Yönetilen etki alanındaki kullanıcıların kimliğini doğrulamak için Azure AD DS 'nin, NT LAN Manager (NTLM) ve Kerberos kimlik doğrulaması için uygun bir biçimde parola karmaları olması gerekir. Azure AD, kiracınız için Azure AD DS etkinleştirene kadar NTLM veya Kerberos kimlik doğrulaması için gereken biçimde parola karmaları oluşturmaz veya depolamaz. Azure AD, güvenlik nedenleriyle şifresiz metin biçiminde hiçbir parola kimlik bilgisi depolamaz. Bu nedenle, Azure AD kullanıcıların mevcut kimlik bilgilerini temel alarak bu NTLM veya Kerberos parola karmalarını otomatik olarak üretemiyor.

> [!NOTE]
> Uygun şekilde yapılandırıldıktan sonra, kullanılabilir parola karmaları yönetilen etki alanında depolanır. Yönetilen etki alanını silerseniz, o noktada depolanan herhangi bir parola karması de silinir.
>
> Azure AD 'deki eşitlenmiş kimlik bilgileri, daha sonra yönetilen bir etki alanı oluşturursanız yeniden kullanılamaz. parola karmalarını yeniden depolamak için Parola karması eşitlemesini yeniden yapılandırmanız gerekir. Daha önce etki alanına katılmış VM 'Ler veya kullanıcılar, parola karmalarını yeni yönetilen etki alanında oluşturmak ve depolamak için Azure AD ihtiyaçlarına hemen kimlik doğrulaması yapamaz.
>
> Daha fazla bilgi için bkz. [Azure AD DS Için Parola karması eşitleme işlemi ve Azure AD Connect][password-hash-sync-process].

Bu parola karmalarını oluşturma ve depolama adımları, Azure AD 'de oluşturulan yalnızca bulutta yer alan Kullanıcı hesapları için ve Azure AD Connect kullanarak şirket içi dizininizden eşitlenen Kullanıcı hesaplarında farklıdır.

Yalnızca bulutta yer alan bir kullanıcı hesabı, Azure portal veya Azure AD PowerShell cmdlet’leri kullanılarak Azure AD dizininizde oluşturulmuş bir hesaptır. Bu Kullanıcı hesapları şirket içi dizinden eşitlenmez.

> Bu öğreticide, yalnızca bir bulut kullanıcı hesabıyla çalışalım. Azure AD Connect kullanmak için gereken ek adımlar hakkında daha fazla bilgi için bkz. [Şirket ıçı ad 'nizden yönetilen etki alanınız ile eşitlenen Kullanıcı hesapları için parola karmalarını eşitleme][on-prem-sync].

> [!TIP]
> Azure AD kiracınızda, şirket içi AD 'nizden yalnızca bulutta bulunan kullanıcıların ve kullanıcıların bir birleşimi varsa, her iki adım kümesini de doldurmanız gerekir.

Yalnızca bulut Kullanıcı hesapları için, kullanıcıların Azure AD DS kullanabilmesi için parolalarını değiştirmesi gerekir. Bu parola değiştirme işlemi, Kerberos ve NTLM kimlik doğrulamasının parola karmalarının Azure AD 'de oluşturulmasına ve depolanmasına neden olur. Hesap, parola değiştirilene kadar Azure AD 'den Azure AD DS eşitlenmemiş. Azure AD DS kullanması gereken Kiracıdaki tüm bulut kullanıcıları için parolaların süresini bir sonraki oturum açma sırasında bir parola değişikliğine zorlar veya bulut kullanıcılarına parolaları el ile değiştirmelerini söyleyin. Bu öğreticide, Kullanıcı parolasını el ile değiştirelim.

Kullanıcının parolasını sıfırlayabilmesi için Azure AD kiracısı 'nin [self servis parola sıfırlama için yapılandırılmış][configure-sspr]olması gerekir.

Yalnızca bulutta bulunan bir kullanıcının parolasını değiştirmek için, kullanıcının aşağıdaki adımları tamamlaması gerekir:

1. Konumundaki Azure AD erişim paneli sayfasına gidin [https://myapps.microsoft.com](https://myapps.microsoft.com) .
1. Sağ üst köşede adınızı seçin, sonra açılır menüden **profil** ' i seçin.

    ![Profil seçme](./media/tutorial-create-instance/select-profile.png)

1. **Profil** sayfasında, **Parolayı Değiştir**' i seçin.
1. **Parolayı Değiştir** sayfasında, mevcut (eski) parolanızı girip yeni bir parola girin ve onaylayın.
1. **Gönder**’i seçin.

Yeni parolanın Azure AD DS kullanılabilir olması ve yönetilen etki alanına katılmış bilgisayarlarda başarıyla oturum açması için parolanızı değiştirdikten birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yönetilen bir etki alanı için DNS gereksinimlerini anlama
> * Yönetilen etki alanı oluşturma
> * Etki alanı yönetimine yönetici kullanıcılar ekleme
> * Azure AD DS kullanıcı hesaplarını etkinleştirme ve parola karmaları oluşturma

VM 'Leri etki alanına katmadan ve yönetilen etki alanını kullanan uygulamaları dağıtmadan önce, uygulama iş yükleri için bir Azure sanal ağı yapılandırın.

> [!div class="nextstepaction"]
> [Yönetilen etki alanınızı kullanmak için uygulama iş yükleri için Azure sanal ağını yapılandırın](tutorial-configure-networking.md)

<!-- INTERNAL LINKS -->
[tutorial-create-instance-advanced]: tutorial-create-instance-advanced.md
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[network-considerations]: network-considerations.md
[create-dedicated-subnet]: ../virtual-network/virtual-network-manage-subnet.md#add-a-subnet
[scoped-sync]: scoped-synchronization.md
[on-prem-sync]: tutorial-configure-password-hash-sync.md
[configure-sspr]: ../active-directory/authentication/tutorial-enable-sspr.md
[password-hash-sync-process]: ../active-directory/hybrid/how-to-connect-password-hash-synchronization.md#password-hash-sync-process-for-azure-ad-domain-services
[tutorial-create-instance-advanced]: tutorial-create-instance-advanced.md
[skus]: overview.md
[resource-forests]: concepts-resource-forest.md
[availability-zones]: ../availability-zones/az-overview.md
[concepts-sku]: administration-concepts.md#azure-ad-ds-skus

<!-- EXTERNAL LINKS -->
[naming-prefix]: /windows-server/identity/ad-ds/plan/selecting-the-forest-root-domain#selecting-a-prefix
