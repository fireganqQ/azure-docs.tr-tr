---
title: Depolama Gezgini kullanmaya başlayın | Microsoft Docs
description: Depolama Gezgini ile Azure depolama kaynaklarını yönetmeye başlayın. Azure Depolama Gezgini indirin ve yükleyin, bir depolama hesabına veya hizmetine bağlanın ve daha fazlasını yapın.
services: storage
author: cawaMS
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.date: 11/08/2019
ms.author: cawa
ms.openlocfilehash: be9b2d9a31d4affc9615f5d2f4b2585b7533a0f6
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95545916"
---
# <a name="get-started-with-storage-explorer"></a>Depolama Gezgini ile çalışmaya başlama

## <a name="overview"></a>Genel Bakış

Microsoft Azure Depolama Gezgini; Windows, macOS ve Linux’ta Azure Depolama ile çalışmayı kolaylaştıran bir tek başına uygulamadır. Bu makalede, Azure depolama hesaplarınızı bağlamak ve yönetmek için kullanabileceğiniz çeşitli yollar öğreneceksiniz.

![Microsoft Azure Depolama Gezgini][0]

## <a name="prerequisites"></a>Önkoşullar

# <a name="windows"></a>[Windows](#tab/windows)

Aşağıdaki Windows destek sürümleri Depolama Gezgini:

* Windows 10 (önerilir)
* Windows 8
* Windows 7

Tüm Windows sürümleri için Depolama Gezgini en az .NET Framework 4.7.2 gerektirir.

# <a name="macos"></a>[macOS](#tab/macos)

MacOS desteğinin aşağıdaki sürümleri Depolama Gezgini:

* macOS 10,12 Sierra ve sonraki sürümleri

# <a name="linux"></a>[Linux](#tab/linux)

Depolama Gezgini, Linux 'un en yaygın dağıtımları için [ek mağazada](https://snapcraft.io/storage-explorer) kullanılabilir. Bu yükleme için ek depolama önerilir. Depolama Gezgini Snap, yeni sürümler Snap Store 'da yayımlandığında tüm bağımlılıklarını ve güncelleştirmelerini yüklüyor.

Desteklenen dağıtımlar için bkz. [anlık görüntü yükleme sayfası](https://snapcraft.io/docs/installing-snapd).

Depolama Gezgini, parola Yöneticisi 'nin kullanılmasını gerektirir. Parola yöneticisine el ile bağlanmanız gerekebilir. Aşağıdaki komutu çalıştırarak, Depolama Gezgini sisteminizin parola yöneticisine bağlayabilirsiniz:

```bash
snap connect storage-explorer:password-manager-service :password-manager-service
```

Depolama Gezgini *. tar. gz* Download olarak da kullanılabilir. Bağımlılıkları el ile yüklemelisiniz. Linux support *. tar. gz* yüklemesi için aşağıdaki dağıtımlar:

* Ubuntu 20,04 x64
* Ubuntu 18,04 x64
* Ubuntu 16,04 x64

*. Tar. gz* yüklemesi diğer dağıtımlar üzerinde çalışabilir, ancak yalnızca bu listelenmiş olanlar resmi olarak desteklenmektedir.

Linux üzerinde Depolama Gezgini yükleme hakkında daha fazla yardım için, Azure Depolama Gezgini sorun giderme kılavuzunda [Linux bağımlılıkları](./storage/common/storage-explorer-troubleshooting.md#linux-dependencies) bölümüne bakın.

---

## <a name="download-and-install"></a>İndirme ve yükleme

Depolama Gezgini indirmek ve yüklemek için bkz. [Azure Depolama Gezgini](https://www.storageexplorer.com).

## <a name="connect-to-a-storage-account-or-service"></a>Bir depolama hesabı veya hizmetine bağlanmak

Depolama Gezgini depolama hesaplarına bağlamak için birçok yol sağlar. Genel olarak şunlardan birini yapabilirsiniz:

* [Aboneliklerinize ve kaynaklarına erişmek için Azure 'da oturum açın](#sign-in-to-azure)
* [Belirli bir depolama alanı veya CosmosDB kaynağı iliştirme](#attach-a-specific-resource)

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

> [!NOTE]
> Oturum açtıktan sonra kaynakları tam olarak erişmek için Depolama Gezgini hem yönetim (Azure Resource Manager) hem de veri katmanı izinleri gerektirir. Bu, depolama hesabınıza, hesaptaki kapsayıcılara ve kapsayıcılardaki verilere erişmenizi sağlayan Azure Active Directory (Azure AD) izinlerinin olması gerektiği anlamına gelir. Yalnızca veri katmanında izinleriniz varsa, [Azure AD aracılığıyla bir kaynak eklemeyi](#add-a-resource-via-azure-ad)düşünün. Depolama Gezgini gerekli olan belirli izinler hakkında daha fazla bilgi için, bkz. [Azure Depolama Gezgini sorun giderme kılavuzu](./storage/common/storage-explorer-troubleshooting.md#azure-rbac-permissions-issues).

1. Depolama Gezgini, hesap yönetimini **görüntüle**' yi seçin  >  **Account Management** veya **hesapları Yönet** düğmesini seçin.

    ![Hesapları Yönet][1]

1. **Hesap yönetimi** artık oturum açtığınız tüm Azure hesaplarını görüntülüyor. Başka bir hesaba bağlanmak için **Hesap Ekle**' yi seçin.

1. **Azure depolama 'Ya Bağlan** bölümünde, ulusal bir bulutta veya bir Azure Stack oturum açmak için Azure **ortamından** bir Azure bulutu seçin. Ortamınızı seçtikten sonra **İleri**' yi seçin.

    ![Oturum açma seçeneği][2]

    Depolama Gezgini, oturum açmak için bir sayfa açar. Daha fazla bilgi için bkz. [depolama Gezginini bir Azure Stack aboneliğine veya depolama hesabına bağlama](/azure-stack/user/azure-stack-storage-connect-se).

1. Azure hesabıyla başarıyla oturum açtıktan sonra hesap **yönetimi** altında bu hesapla ilişkili hesap ve Azure abonelikleri görüntülenir. Seçiminize tüm Azure aboneliklerinden hiçbiri arasında geçiş yapmak için **tüm abonelikler** ' i seçin. Birlikte çalışmak istediğiniz Azure aboneliklerini seçin ve ardından **Uygula**’yı seçin.

    ![Azure aboneliklerini seçme][3]

    **Gezgin** seçili Azure abonelikleriyle ilişkili depolama hesaplarını görüntüler.

    ![Seçili Azure abonelikleri][4]

### <a name="attach-a-specific-resource"></a>Belirli bir kaynağı iliştirme

Depolama Gezgini bir kaynağa eklemenin birkaç yolu vardır:

* [Azure AD aracılığıyla bir kaynak ekleyin](#add-a-resource-via-azure-ad). Yalnızca veri katmanında izinleriniz varsa, bir blob kapsayıcısı veya Azure Data Lake Storage 2. blob depolama kapsayıcısı eklemek için bu seçeneği kullanın.
* [Bir bağlantı dizesi kullanın](#use-a-connection-string). Depolama hesabına yönelik bir bağlantı dizeniz varsa bu seçeneği kullanın. Depolama Gezgini hem anahtar hem de [paylaşılan erişim imzası](./storage/common/storage-sas-overview.md) bağlantı dizelerini destekler.
* [Paylaşılan erişim imzası URI 'Si kullanın](#use-a-shared-access-signature-uri). Blob kapsayıcısına, dosya paylaşımında, kuyrukta veya tabloda [paylaşılan erişim imzası URI 'si](./storage/common/storage-sas-overview.md) varsa, kaynağa eklemek için onu kullanın. Paylaşılan erişim imzası URI 'SI almak için [Depolama Gezgini](#generate-a-sas-in-storage-explorer) ya da [Azure Portal](https://portal.azure.com)kullanabilirsiniz.
* [Bir ad ve anahtar kullanın](#use-a-name-and-key). Depolama hesabınızda hesap anahtarlarından birini biliyorsanız, bu seçeneği kullanarak hızlı bir şekilde bağlantı oluşturabilirsiniz. Azure Portal **ayarları**  >  **erişim tuşları** ' nı seçerek depolama hesabı sayfasında anahtarlarınızı bulun. [Azure portal](https://portal.azure.com)
* [Yerel bir öykünücüye ekleyin](#attach-to-a-local-emulator). Kullanılabilir Azure Storage Öykünücülerinden birini kullanıyorsanız, öykünücüye kolayca bağlanmak için bu seçeneği kullanın.
* [Bir bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına bağlanın](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string). CosmosDB örneğine bir bağlantı dizeniz varsa bu seçeneği kullanın.
* [URI 'ye göre Azure Data Lake Store bağlanın](#connect-to-azure-data-lake-store-by-uri). Azure Data Lake Store URI 'SI varsa bu seçeneği kullanın.

#### <a name="add-a-resource-via-azure-ad"></a>Azure AD aracılığıyla kaynak ekleme

1. **Azure depolama 'Ya Bağlan**' ı açmak için **Bağlan** simgesini seçin.

    ![Azure Storage’a bağlanma seçeneği][9]

1. Daha önce yapmadıysanız, kaynağa erişimi olan Azure hesabı 'nda oturum açmak için **Azure hesabı ekle** seçeneğini kullanın. Oturum açtıktan sonra **Azure depolama 'Ya Bağlan**' a geri dönün.

1. **Azure Active Directory (Azure AD) aracılığıyla Kaynak Ekle**' yi seçin ve ardından **İleri**' yi seçin.

1. Bir Azure hesabı ve kiracı seçin. Bu değerler, iliştirmek istediğiniz depolama kaynağına erişebilmelidir. **İleri**’yi seçin.

1. Eklemek istediğiniz kaynak türünü seçin. Bağlanmak için gereken bilgileri girin. 

   Bu sayfaya girdiğiniz bilgiler, eklediğiniz kaynak türüne bağlıdır. Doğru kaynak türünü seçtiğinizden emin olun. Gerekli bilgileri girdikten sonra **İleri**' yi seçin.

1. Tüm bilgilerin doğru olduğundan emin olmak için **bağlantı özetini** gözden geçirin. Varsa, **Bağlan**' ı seçin. Aksi takdirde, yanlış bilgileri onarmak için önceki sayfalara geri dönmek üzere **geri** ' yi seçin.

Bağlantı başarıyla eklendikten sonra kaynak ağacı bağlantıyı temsil eden düğüme gider. Kaynak, **Yerel & bağlı**  >  **depolama hesapları**  >  **(ekli kapsayıcılar)**  >  **BLOB kapsayıcıları** altında görünür. Depolama Gezgini bağlantınızı ekleyemedik veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, bkz. [Azure Depolama Gezgini sorun giderme kılavuzu](./storage/common/storage-explorer-troubleshooting.md).

#### <a name="use-a-connection-string"></a>Bağlantı dizesi kullanma

1. **Azure depolama 'Ya Bağlan**' ı açmak için **Bağlan** simgesini seçin.

    ![Azure Storage’a bağlanma seçeneği][9]

1. **Bir bağlantı dizesi kullan**' ı seçin ve ardından **İleri**' yi seçin.

1. Bağlantınız için bir görünen ad seçin ve Bağlantı dizenizi girin. Ardından **İleri**' yi seçin.

1. Tüm bilgilerin doğru olduğundan emin olmak için **bağlantı özetini** gözden geçirin. Varsa, **Bağlan**' ı seçin. Aksi takdirde, yanlış bilgileri onarmak için önceki sayfalara geri dönmek üzere **geri** ' yi seçin.

Bağlantı başarıyla eklendikten sonra kaynak ağacı bağlantıyı temsil eden düğüme gider. Kaynak, **Yerel & bağlı**  >  **depolama hesapları** altında görünür. Depolama Gezgini bağlantınızı ekleyemedik veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, bkz. [Azure Depolama Gezgini sorun giderme kılavuzu](./storage/common/storage-explorer-troubleshooting.md).

#### <a name="use-a-shared-access-signature-uri"></a>Paylaşılan erişim imzası URI’si kullanma

1. **Azure depolama 'Ya Bağlan**' ı açmak için **Bağlan** simgesini seçin.

    ![Azure Storage’a bağlanma seçeneği][9]

1. **Paylaşılan erişim imzası (SAS) URI 'Si kullan**' ı seçin ve ardından **İleri**' yi seçin.

1. Bağlantınız için bir görünen ad seçin ve paylaşılan erişim imza URI 'nizi girin. İliştirmekte olduğunuz kaynak türü için hizmet uç noktası otomatik olarak doldurulur. Özel bir uç nokta kullanıyorsanız, bu mümkün olmayabilir. **İleri**’yi seçin.

1. Tüm bilgilerin doğru olduğundan emin olmak için **bağlantı özetini** gözden geçirin. Varsa, **Bağlan**' ı seçin. Aksi takdirde, yanlış bilgileri onarmak için önceki sayfalara geri dönmek üzere **geri** ' yi seçin.

Bağlantı başarıyla eklendikten sonra kaynak ağacı bağlantıyı temsil eden düğüme gider. Kaynak, eklenen kapsayıcı türü için **Yerel & bağlı**  >  **depolama hesapları**  >  **(ekli kapsayıcılar)**  >  *hizmet düğümü* altında görünür. Depolama Gezgini bağlantınızı ekleyemedik, bkz. [Azure Depolama Gezgini sorun giderme kılavuzu](./storage/common/storage-explorer-troubleshooting.md). Bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız sorun giderme kılavuzuna bakın.

#### <a name="use-a-name-and-key"></a>Ad ve anahtar kullanma

1. **Azure depolama 'Ya Bağlan**' ı açmak için **Bağlan** simgesini seçin.

    ![Azure Storage’a bağlanma seçeneği][9]

1. **Depolama hesabı adı ve anahtarı kullan**' ı seçin ve ardından **İleri**' yi seçin.

1. Bağlantınız için bir görünen ad seçin.

1. Depolama hesabı adınızı ve erişim anahtarlarından birini girin.

1. Kullanılacak **depolama alanını** seçin ve ardından **İleri**' yi seçin.

1. Tüm bilgilerin doğru olduğundan emin olmak için **bağlantı özetini** gözden geçirin. Varsa, **Bağlan**' ı seçin. Aksi takdirde, yanlış bilgileri onarmak için önceki sayfalara geri dönmek üzere **geri** ' yi seçin.

Bağlantı başarıyla eklendikten sonra kaynak ağacı bağlantıyı temsil eden düğüme gider. Kaynak, **Yerel & bağlı**  >  **depolama hesapları** altında görünür. Depolama Gezgini bağlantınızı ekleyemedik veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, bkz. [Azure Depolama Gezgini sorun giderme kılavuzu](./storage/common/storage-explorer-troubleshooting.md).

#### <a name="attach-to-a-local-emulator"></a>Yerel bir öykünücüye ekleme

Depolama Gezgini Şu anda iki adet resmi depolama öykünücüsünü desteklemektedir:

* [Azure depolama öykünücüsü](storage/common/storage-use-emulator.md) (yalnızca Windows)
* [Azurite](https://github.com/azure/azurite) (Windows, MacOS veya Linux)

Öykünücü, varsayılan bağlantı noktalarını dinliyorsa, öykünücüe erişmek için **öykünücü-varsayılan bağlantı noktaları** düğümünü kullanabilirsiniz. **Yerel & bağlı** depolama hesapları altında **öykünücü varsayılan bağlantı noktalarını** arayın  >  **Storage Accounts**.

Bağlantınız için farklı bir ad kullanmak istiyorsanız veya öykünücü varsayılan bağlantı noktalarında çalışmıyorsa, şu adımları izleyin:

1. Öykünücüyü başlatın. `AzureStorageEmulator.exe status`Her hizmet türü için bağlantı noktalarını göstermek üzere komutunu girin.

   > [!IMPORTANT]
   > Depolama Gezgini öykünücüyü otomatik olarak başlatmıyor. El ile başlatmanız gerekir.

1. **Azure depolama 'Ya Bağlan**' ı açmak için **Bağlan** simgesini seçin.

    ![Azure Storage’a bağlanma seçeneği][9]

1. **Yerel bir öykünücüye Ekle**' yi seçin ve ardından **İleri**' yi seçin.

1. Bağlantınız için bir görünen ad seçin ve her bir hizmet türü için öykünücünüzün dinlediği bağlantı noktalarını girin. **Yerel bir öykünücüye iliştirme** , çoğu öykünücüye yönelik varsayılan bağlantı noktası değerlerini önerir. **Dosya bağlantı noktası** , resmi öykünücülerinden hiçbiri şu anda dosyalar hizmetini desteklemediğinden boş. Kullanmakta olduğunuz öykünücü dosyaları destekliyorsa, kullanılacak bağlantı noktasını girebilirsiniz. Ardından **İleri**' yi seçin.

1. **Bağlantı özetini** gözden geçirin ve tüm bilgilerin doğru olduğundan emin olun. Varsa, **Bağlan**' ı seçin. Aksi takdirde, yanlış bilgileri onarmak için önceki sayfalara geri dönmek üzere **geri** ' yi seçin.

Bağlantı başarıyla eklendikten sonra kaynak ağacı bağlantıyı temsil eden düğüme gider. Düğüm, **Yerel & bağlı**  >  **depolama hesapları** altında görünmelidir. Depolama Gezgini bağlantınızı ekleyemedik veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, bkz. [Azure Depolama Gezgini sorun giderme kılavuzu](./storage/common/storage-explorer-troubleshooting.md).

#### <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına bağlanma

Bir Azure aboneliği aracılığıyla Azure Cosmos DB hesaplarını yönetmek yerine, bir bağlantı dizesi kullanarak Azure Cosmos DB bağlanabilirsiniz. Bağlanmak için şu adımları uygulayın:

1. **Gezgin** altında, **eklenen yerel &**' i genişletin, **Cosmos DB hesaplar**' a sağ tıklayın ve Azure Cosmos DB ' **ye Bağlan**' ı seçin

    ![Bağlantı dizesiyle Azure Cosmos DB’ye bağlanma][21]

1. Azure Cosmos DB API 'sini seçin, **bağlantı dizesi** verilerinizi girin ve sonra Azure Cosmos DB hesabını bağlamak için **Tamam** ' ı seçin. Bağlantı dizesini alma hakkında daha fazla bilgi için bkz. [Azure Cosmos hesabını yönetme](./cosmos-db/how-to-manage-database-account.md).

    ![Bağlantı dizesi][22]

#### <a name="connect-to-azure-data-lake-store-by-uri"></a>URI 'ye göre Azure Data Lake Store bağlanma

Aboneliğinizde olmayan bir kaynağa erişebilirsiniz. Size Kaynak URI 'sini sağlamak için bu kaynağa erişimi olan bir kişi olması gerekir. Oturum açtıktan sonra, URI 'yi kullanarak Data Lake Store bağlanın. Bağlanmak için şu adımları uygulayın:

1. **Gezgin** altında, **Yerel & ekli**' ı genişletin.

1. **Data Lake Storage 1.** öğesine sağ tıklayın ve **Data Lake Storage 1. Bağlan**' ı seçin.

    ![Data Lake Store bağlam menüsüne Bağlan](./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-explorer-connect-data-lake-storage.png)

1. URI 'yi girip **Tamam**' ı seçin. Data Lake Store **Data Lake Storage** altında görünür.

    ![Data Lake Store sonuca Bağlan](./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-explorer-attach-data-lake-finished.png)

Bu örnek Data Lake Storage 1. kullanır. Azure Data Lake Storage 2. artık kullanılabilir. Daha fazla bilgi için bkz. [Azure Data Lake Storage 1. nedir?](./data-lake-store/data-lake-store-overview.md).

## <a name="generate-a-shared-access-signature-in-storage-explorer"></a>Depolama Gezgini paylaşılan erişim imzası oluşturma<a name="generate-a-sas-in-storage-explorer"></a>

### <a name="account-level-shared-access-signature"></a>Hesap düzeyinde paylaşılan erişim imzası

1. Paylaşmak istediğiniz depolama hesabına sağ tıklayın ve ardından **paylaşılan erişim Imzasını al**' ı seçin.

    ![Paylaşılan erişim imzasını al bağlam menü seçeneği][14]

1. **Paylaşılan erişim imzası**' nda, hesap için istediğiniz zaman çerçevesini ve Izinleri belirtip **Oluştur**' u seçin.

    ![Paylaşılan erişim imzası al][15]

1. **Bağlantı dizesini** veya ham **sorgu dizesini** panonuza kopyalayın.

### <a name="service-level-shared-access-signature"></a>Hizmet düzeyi paylaşılan erişim imzası

Hizmet düzeyinde paylaşılan erişim imzası alabilirsiniz. Daha fazla bilgi için bkz. [bir blob kapsayıcısı IÇIN SAS edinme](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container).

## <a name="search-for-storage-accounts"></a>Depolama hesapları arama

Bir depolama kaynağını bulmak için **Gezgin** bölmesinde arama yapabilirsiniz.

Arama kutusuna metin girerken Depolama Gezgini, bu noktaya kadar girdiğiniz arama değeriyle eşleşen tüm kaynakları görüntüler. Bu örnekte, **uç noktaları** için arama gösterilmektedir:

![Depolama hesabı araması][23]

> [!NOTE]
> Aramanızı hızlandırmak için, aradığınız öğeyi içermeyen aboneliklerin seçimini kaldırmak için **Hesap yönetimi** 'ni kullanın. Ayrıca, bir düğüme sağ tıklayıp belirli bir düğümden aramayı başlatmak için **buradan ara** ' yı seçebilirsiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Gezgini ile Azure Blob depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-blobs.md)
* [Azure Depolama Gezgini'ni kullanarak verilerle çalışma](./cosmos-db/storage-explorer.md)
* [Depolama Gezgini ile Azure Data Lake Store kaynaklarını yönetme](./data-lake-store/data-lake-store-in-storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Overview.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ManageAccounts.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-azure-environment.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/account-panel-subscriptions-apply.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SubscriptionNode.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/PortalAccessKeys.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccessKeys.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-AddWithKeySelected.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-NameAndKeyPage.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount-Detach.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-shared-access-signature-for-storage-explorer.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/create-shared-access-signature-for-storage-explorer.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-WithConnStringOrSASSelected.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-1.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithSASAccount.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-2.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ServiceAttachedWithSAS.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-cosmos-db-by-connection-string.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string-for-cosmos-db.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-explorer-search-for-resource.png