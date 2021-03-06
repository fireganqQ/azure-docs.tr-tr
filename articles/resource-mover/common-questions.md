---
title: Azure Kaynak taşıyıcısı hakkında sık sorulan sorular
description: Azure Kaynak taşıyıcısı hakkında sık sorulan soruların yanıtlarını alın
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: conceptual
ms.date: 02/04/2021
ms.author: raynew
ms.openlocfilehash: a75cd3c5dbf205f49aa606bfe96623a61bce39db
ms.sourcegitcommit: 49ea056bbb5957b5443f035d28c1d8f84f5a407b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2021
ms.locfileid: "100007066"
---
# <a name="common-questions"></a>Sık sorulan sorular

Bu makalede, [Azure Kaynak taşıyıcısı](overview.md)hakkında sık sorulan sorular yanıtlanmaktadır.


## <a name="moving-across-regions"></a>Bölgeler arasında taşıma

### <a name="can-i-move-resources-across-any-regions"></a>Kaynakları herhangi bir bölgede taşıyabilir miyim?

Şu anda, [söz konusu bölgede bulunan kaynak türlerine](https://azure.microsoft.com/global-infrastructure/services/)bağlı olarak, kaynakları herhangi bir kaynak ortak bölgesinden herhangi bir hedef ortak bölgeye taşıyabilirsiniz. Azure Kamu bölgelerinde kaynak taşıma Şu anda desteklenmiyor.

### <a name="what-resources-can-i-move-across-regions-using-resource-mover"></a>Kaynak taşıyıcısı kullanarak bölgeler arasında hangi kaynakları taşıyabilirim?

Kaynak taşıyıcısı kullanarak şu anda bölgeler arasında şu kaynakları taşıyabilirsiniz:

- Azure VM 'Leri ve ilişkili diskler
- NIC’ler
- Kullanılabilirlik kümeleri 
- Azure sanal ağları 
- Genel IP adresleri
- Ağ güvenlik grupları (NSG)
- İç ve genel yük dengeleyiciler 
- Azure SQL veritabanları ve elastik havuzlar

### <a name="can-i-move-disks-across-regions"></a>Diskleri bölgeler arasında taşıyabilir miyim?

Bölgeler arasında taşınan kaynak olarak disk seçemezsiniz. Ancak, diskler VM taşımanın bir parçası olarak taşınır.

### <a name="what-does-it-mean-to-move-a-resource-group"></a>Bir kaynak grubunu taşımanın ne anlama geliyor?

Taşıma için bir kaynak seçildiğinde, karşılık gelen kaynak grubu taşıma için otomatik olarak eklenir. Hedef kaynağın hedefte olduğu gibi bir kaynak grubuna yerleştirilmesi gerektiğinden, bu gereklidir. Taşıma için eklendikten sonra, özelleştirmeyi ve bir kaynak grubu sağlamayı tercih edebilirsiniz. Kaynak grubunun taşınması, kaynak **kaynak grubundaki tüm kaynakların taşınacağı anlamına gelmez** .

### <a name="can-i-move-resources-across-subscriptions-when-i-move-them-across-regions"></a>Kaynakları bölgeler arasında taşırken kaynakları abonelikler arasında taşıyabilir miyim?

Kaynakları hedef bölgeye taşıdıktan sonra aboneliği değiştirebilirsiniz. Kaynakları farklı bir aboneliğe taşıma hakkında [daha fazla bilgi edinin](../azure-resource-manager/management/move-resource-group-and-subscription.md) . 

### <a name="does-azure-resource-move-service-store-customer-data"></a>Azure kaynağı hizmet mağazası müşteri verilerini taşır mi? 
Hayır. Kaynak taşıma hizmeti müşteri verilerini depolamaz, yalnızca müşteri tarafından taşıma için seçilen kaynakların izlenmesini ve ilerlemesini kolaylaştıran meta veri bilgilerini depolar.


### <a name="where-is-the-metadata-for-moving-across-regions-stored"></a>Bölgeler arasında taşınabilecek meta veriler nerede?

Bir Microsoft aboneliğinde [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md) veritabanında ve [Azure Blob depolamada](../storage/common/storage-service-encryption.md)depolanır. Şu anda meta veriler Doğu ABD 2 ve Kuzey Avrupa depolanıyor. Bu kapsamı diğer bölgelere genişleteceğiz. Bu, kaynakları herhangi bir genel bölgede taşımaya kısıtlama vermez.

### <a name="is-the-collected-metadata-encrypted"></a>Toplanan meta veriler şifrelendi mı?

Evet, hem aktarım hem de bekleyen.
- Aktarım sırasında, meta veriler HTTPS kullanılarak internet üzerinden kaynak taşıyıcısı hizmetine güvenli bir şekilde gönderilir.
- Depolama alanında meta veriler şifrelenir.

### <a name="how-is-managed-identity-used-in-resource-mover"></a>Kaynak taşıyıcısı içinde yönetilen kimlik nasıl kullanılır?

[Yönetilen kimlik](../active-directory/managed-identities-azure-resources/overview.md) (eski adıyla YÖNETILEN HIZMET KIMLIĞI (MSI)), Azure hizmetleri 'NI Azure AD 'de otomatik olarak yönetilen bir kimlikle sağlar.
- Kaynak taşıyıcısı, kaynakları bölgeler arasında taşımak için Azure aboneliklerine erişebilmek üzere yönetilen kimlik kullanır.
- Taşıma koleksiyonu, taşıdığınız kaynakları içeren aboneliğe erişimi olan sistem tarafından atanan bir kimliğe ihtiyaç duyuyor.

- Portaldaki bölgeler arasında kaynakları taşırsanız, bu işlem otomatik olarak gerçekleşir.
- PowerShell kullanarak kaynakları taşırsanız, koleksiyona sistem tarafından atanan bir kimlik atamak için cmdlet 'leri çalıştırır ve ardından kimlik sorumlusu için doğru abonelik izinlerine sahip bir rol atamalısınız. 

### <a name="what-managed-identity-permissions-does-resource-mover-need"></a>Kaynak taşıyıcısı hangi yönetilen kimlik izinlerine ihtiyaç duyuyor? 

Azure Kaynak Taşıyıcı yönetilen kimliğinin en azından şu izinlere sahip olması gerekir: 

- Kullanıcı aboneliğine kaynak yazma/oluşturma izni, *katkıda* bulunan rolüyle kullanılabilir. 
- Rol ataması oluşturma izni. Genellikle *sahip* veya *Kullanıcı erişimi yönetici* rolleriyle veya *Microsoft. Authorization/role atama/yazma izni* atanmış özel bir rolle kullanılabilir. Veri paylaşımı kaynağının yönetilen kimliğine zaten Azure veri deposuna erişim izni verildiyse, bu izin gerekli değildir. 
 
Portaldaki Kaynak Taşıyıcı hub'ında kaynak eklediğinizde kullanıcı yukarıda belirtilen izinlere sahip olduğu sürece izinler otomatik olarak işlenir. PowerShell 'e kaynak eklerseniz, izinleri el ile atarsınız.

> [!IMPORTANT]
> Kimlik rolü atamalarını değiştirmemenizi veya kaldırmanızı kesinlikle öneririz. 

### <a name="what-should-i-do-if-i-dont-have-permissions-to-assign-role-identity"></a>Rol kimliği atama iznim yoksa ne yapmam gerekir?

**Olası nedeni** | **Öneri**
--- | ---
İlk kez bir kaynak eklediğinizde *katkıda bulunan* ve *Kullanıcı erişimi Yöneticisi* (veya *sahibi*) değilsiniz. | Abonelik için *katkıda* bulunan ve *Kullanıcı erişimi Yöneticisi* (veya *sahibi*) izinleri olan bir hesap kullanın.
Kaynak taşıyıcısı tarafından yönetilen kimliğin gerekli rolü yok. | ' Katkıda bulunan ' ve ' Kullanıcı erişimi Yöneticisi ' rollerini ekleyin.
Kaynak taşıyıcısı yönetilen kimliği *none* olarak sıfırlandı. | Taşıma koleksiyonunda sistem tarafından atanan bir kimliği yeniden etkinleştirin > **kimliği**. Alternatif olarak, kaynağı Ekle ' ye aynı şeyi sağlayan **kaynakları** yeniden ekleyin.  
Abonelik farklı bir kiracıya taşındı. | Taşıma koleksiyonu için yönetilen kimliği devre dışı bırakıp etkinleştirin.

### <a name="how-can-i-do-multiple-moves-together"></a>Birden çok daha fazla hareketi nasıl yapabilirim?

Portalda Değiştir seçeneğini kullanarak kaynak/hedef birleşimleri gerektiği gibi değiştirin.

### <a name="what-happens-when-i-remove-a-resource-from-a-list-of-move-resources"></a>Kaynak taşıma listesinden bir kaynağı kaldırdığımda ne olur?

Taşıma listesine eklediğiniz kaynakları kaldırabilirsiniz. Listeden bir kaynağı kaldırdığınızda oluşan davranış, kaynak durumuna bağlıdır. [Daha fazla bilgi edinin](remove-move-resources.md#vm-resource-state-after-removing).



## <a name="next-steps"></a>Sonraki adımlar

Kaynak taşıyıcısı bileşenleri ve taşıma işlemi hakkında [daha fazla bilgi edinin](about-move-process.md) .
