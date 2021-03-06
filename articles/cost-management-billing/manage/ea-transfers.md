---
title: Azure Kurumsal aktarımlar
description: Azure EA aktarımlarını açıklar
author: bandersmsft
ms.reviewer: baolcsva
ms.service: cost-management-billing
ms.subservice: enterprise
ms.topic: conceptual
ms.date: 01/27/2021
ms.author: banders
ms.custom: contperf-fy21q1
ms.openlocfilehash: 7aa57fa20c3a043cdb210ccd8a5ddbf61323716d
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98943687"
---
# <a name="azure-enterprise-transfers"></a>Azure Kurumsal aktarımlar

Bu makalede kurumsal aktarımlara ilişkin bir genel bakış sunulmaktadır.

## <a name="transfer-an-enterprise-account-to-a-new-enrollment"></a>Kurumsal bir hesabı yeni bir kayda aktarma

Hesap aktarımı işlemi, bir hesap sahibini bir kayıttan diğerine taşır. Hesap sahibine ilişkin tüm abonelikler hedef kayda taşınır. Birden fazla etkin kaydınız olduğunda ve yalnızca seçili hesap sahiplerini taşımak istediğinizde hesap aktarımını kullanın.

Eylem bir kurumsal yönetici tarafından gerçekleştirilemediği için bu bölüm yalnızca bilgilendirme amaçlıdır. Bir kurumsal hesabı yeni bir kayda aktarmak için destek isteği oluşturulması gerekir.

Kurumsal bir hesabı yeni bir kayda aktarırken aşağıdaki noktaları aklınızda bulundurun:

- Yalnızca istekte belirtilen hesaplar aktarılır. Tüm hesaplar seçilirse hepsi aktarılır.
- Kaynak kaydının durumu etkin veya genişletilmiş olarak korunur. Kaydı süresi dolana kadar kullanmaya devam edebilirsiniz.

### <a name="prerequisites"></a>Ön koşullar

Bir hesap aktarımı istediğinizde aşağıdaki bilgileri sağlayın:

- Hedef kaydın sayısı, hesap adı ve aktarımın yapılacağı hesabın sahibinin e-posta adresi
- Kaynak kaydı için, aktarılacak kayıt numarası ve hesap

Hesap aktarımından önce göz önünde bulundurmanız gereken diğer noktalar:

- Hedef ve kaynak kaydı için bir EA yöneticisinden onay gerekir
- Hesap aktarımı gereksinimlerinizi karşılamıyorsa, kayıt aktarımı yapmayı düşünün.
- Hesap aktarımı, belirli hesaplarla ilgili tüm hizmetleri ve abonelikleri aktarır.
- Aktarım tamamlandıktan sonra, aktarılan hesap kaynak kayıt altında etkin değil olarak, hedef kaydın altında ise etkin olarak görünür.
- Hesap, kaynak kayıtta etkin aktarım tarihine ve hedef kayıtta başlangıç tarihine karşılık gelen bitiş tarihini gösterir.
- Hesap için etkin aktarım tarihi öncesinde gerçekleştirilen tüm kullanımlar, kaynak kaydın altında kalır.

## <a name="transfer-enterprise-enrollment-to-a-new-one"></a>Kuruluş kaydını yeni bir kayda aktarma

Kayıt aktarımı şu durumlarda göz önünde bulundurulur:

- Geçerli bir kaydın Ön Ödeme döneminin sona ermesi.
- Bir kaydın süresi dolmuş/uzatılmış durumda olması ve yeni bir anlaşmanın yapılması.
- Birden fazla kaydınızın olması ve tüm hesaplarla faturaları tek kayıtta birleştirmek istemeniz.

Eylem bir kurumsal yönetici tarafından gerçekleştirilemediği için bu bölüm yalnızca bilgilendirme amaçlıdır. Kayıt [otomatik kayıt aktarımına](#auto-enrollment-transfer)uygun olmadığı takdirde, bir kuruluş kaydını yeni bir kayda aktarmak için bir destek isteği gerekir.

Bir kuruluş kaydının tamamını bir kayda aktarmak istediğinizde aşağıdaki eylemler gerçekleşir:

- Tüm EA departman yöneticileri de dahil olmak üzere tüm Azure hizmetleri, abonelikleri, hesapları, departmanları ve kayıt yapısının tamamı yeni hedef kayda aktarılır.
- Kayıt durumu _Aktarıldı_ olarak ayarlanır. Aktarılan kayıt yalnızca geçmiş kullanım raporlama amacıyla kullanılabilir.
- Aktarılan bir kayda rol veya abonelik ekleyemezsiniz. Aktarılan durum, kayıtta daha fazla kullanım yapılmasını önler.
- Anlaşmadaki kalan Azure Ön Ödeme bakiyeleri, geleceğe dönük hükümler de dahil olmak üzere kaybedilir.
-    Aktardığınız kayıt, RI satın alımlarına sahipse, RI satın alma ücreti kaynak kaydında kalır, ancak tüm RI avantajları yeni kayıtta kullanılmak üzere üzerinden aktarılır.
-    Market tek seferlik satın alma ücreti ve eski kayıt üzerinde zaten tahakkuk eden aylık sabit ücretler yeni kayda aktarılmaz. Tüketime dayalı market ücretleri aktarılır.

### <a name="effective-transfer-date"></a>Geçerli aktarım tarihi

Geçerli aktarım tarihi, hedef kaydın başlangıç tarihi veya daha sonraki bir tarih olabilir.

Kaynak kayıt kullanımı, Azure Ön Ödemesi karşılığında veya fazla kullanım olarak ücretlendirilir. Etkin aktarım tarihi yeni kayda aktarıldıktan ve ücretlendirildikten sonra gerçekleşen kullanım.

### <a name="prerequisites"></a>Ön koşullar

Bir kayıt aktarımı istediğinizde aşağıdaki bilgileri sağlayın:

- Kaynak kaydı için, kayıt numarası.
- Hedef kaydı için, aktarımın yapılacağı kayıt numarası.
- Kayıt aktarımı geçerlilik tarihi, hedef kaydın başlangıç tarihi veya sonrasındaki bir tarih olabilir. Seçilen tarih, zaten verilen fazla kullanım faturasının kullanımını etkilemiyor.

Kayıt aktarımından önce göz önünde bulundurmanız gereken diğer noktalar:

- Hem hedef hem de kaynak kaydı EA Yöneticilerinden onay gerekir.
- Kayıt aktarımı gereksinimlerinizi karşılamıyorsa, hesap aktarımı yapmayı düşünün.
- Kaynak kaydı durumu aktarıldı olarak güncelleştirilir ve yalnızca geçmiş kullanım raporlama amaçları için kullanılabilir olur.

### <a name="auto-enrollment-transfer"></a>Otomatik kayıt aktarımı

Kayıt aktarımı istemek için bir destek bileti göndermemiş olsanız bile bir kaydın **aktarılan** duruma sahip olduğunu görebilirsiniz. **Aktarılan** durum, otomatik kayıt aktarma işleminden oluşur. Yenileme tümceciği sırasında otomatik kayıt aktarma işleminin gerçekleşmesi için, yeni sözleşmeye dahil edilmesi gereken birkaç öğe vardır:

- Önceki kayıt numarası (EA portalında bulunması gerekir)
- Önceki kayıt numarasının sona erme tarihi, yeni sözleşmenin geçerlilik başlangıç tarihinden bir gün önce
- Yeni sözleşme, geçerli bir tarihi olan veya geri eklenen bir faturalanmış Azure ön ödeme siparişi içeriyor
- Yeni kayıt EA portalında oluşturulur

EA portalında, önceki kayıt ve yeni kayıt arasında eksik kullanım verileri yoksa, bir aktarım destek bileti oluşturmanız gerekmez.

### <a name="azure-prepayment"></a>Azure Ön Ödemesi

Azure Ön Ödemesi, kayıtlar arasında aktarılamaz. Azure Ön Ödeme bakiyeleri, sipariş edildiği kayda sözleşmeyle bağlıdır. Azure Ön Ödemesi, hesap veya kayıt aktarma işleminin bir parçası olarak aktarılmaz.

### <a name="no-services-affected-for-account-and-enrollment-transfers"></a>Hesap ve kayıt aktarımları için etkilenen hizmet yok

Hesap veya kayıt aktarımı esnasında kesinti süresi yoktur. Gerekli tüm bilgiler sağlanırsa isteğiniz gün içinde tamamlanabilir.

## <a name="transfer-an-enterprise-subscription-to-a-pay-as-you-go-subscription"></a>Kurumsal aboneliği Kullandıkça Öde aboneliğine aktarma

Bir Kurumsal aboneliği Kullandıkça Öde fiyatlarına sahip bir aboneliğe aktarmak için Azure Enterprise Portal'da yeni bir destek isteği oluşturmanız gerekir. Destek isteği oluşturmak için **Yardım ve Destek** alanındaki **+ Yeni destek isteği**'ni seçin.

## <a name="change-azure-subscription-or-account-ownership"></a>Azure aboneliğini veya hesap sahipliğini değiştirme

Azure EA portalı, abonelikleri bir hesap sahibinden diğerine aktarabilir. Daha fazla bilgi için bkz. [Azure aboneliğini veya hesap sahipliğini değiştirme](ea-portal-administration.md#change-azure-subscription-or-account-ownership).

## <a name="subscription-transfer-effects"></a>Abonelik aktarımının etkileri

Bir Azure aboneliği aynı Azure Active Directory kiracısındaki bir hesaba aktarılırsa abonelikteki kaynakları yönetmek için [Azure rol tabanlı erişim denetimine (Azure RBAC)](../../role-based-access-control/overview.md) sahip olan tüm kullanıcılar, gruplar ve hizmet sorumluları aynı erişime sahip olmaya devam eder.

Aboneliğe RBAC erişimi olan kullanıcıları görüntülemek için:

1. Azure portalında **Abonelikler**’i açın.
2. Görüntülemek istediğiniz aboneliği seçin ve ardından **Erişim denetimi (IAM)** seçeneğini belirleyin.
3. **Rol atamaları**’nı seçin. Rol atamaları sayfasında, aboneliğe RBAC erişimi olan tüm kullanıcılar listelenir.

Abonelik farklı bir Azure AD kiracısındaki bir hesaba aktarılırsa, abonelikteki kaynakları yönetmek için [RBAC](../../role-based-access-control/overview.md) iznine sahip olan tüm kullanıcılar, gruplar ve hizmet sorumluları _erişimi_ kaybeder. RBAC erişimi mevcut olmasa da aboneliğe aşağıdaki güvenlik mekanizmaları aracılığıyla erişilebilir:

- Abonelik kaynaklarına kullanıcı yöneticisi hakları veren yönetim sertifikaları. Daha fazla bilgi için bkz. [Azure için Yönetim Sertifikası Oluşturma ve Karşıya Yükleme](../../cloud-services/cloud-services-certs-create.md).
- Depolama gibi hizmetler için erişim anahtarları. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../../storage/common/storage-account-overview.md).
- Azure Sanal Makineleri gibi hizmetler için Uzaktan Erişim kimlik bilgileri.

Alıcının Azure kaynaklarına erişimi kısıtlaması gerekiyorsa, hizmetle ilişkili tüm gizli dizileri güncelleştirmeyi düşünmelidir. Çoğu kaynak aşağıdaki adımlar kullanılarak güncelleştirilebilir:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Merkez menüsünde **Tüm kaynaklar**'ı seçin.
3. Kaynağı seçin.
4. Kaynak sayfasında, mevcut gizli dizileri görüntülemek ve güncelleştirmek için **Ayarlar**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure EA portalı sorunlarını gidermek için yardıma ihtiyacınız varsa [Azure EA portalı erişim sorunlarını giderme](ea-portal-troubleshoot.md) konusuna bakın.