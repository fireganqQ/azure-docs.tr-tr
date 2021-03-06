---
title: Azure 'da Linux VM 'lerinde depolama kaynak silme hatalarını giderme | Microsoft Docs
description: Bağlı VHD 'Leri içeren depolama kaynaklarını silerken Linux VM 'lerinde sorunları nasıl giderebileceğinizi öğrenin.
keywords: ''
services: virtual-machines
author: genlin
manager: dcscontentpm
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 8d727bc8bdc8f015504baa57f9596b3bacac9712
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96022879"
---
# <a name="troubleshoot-storage-resource-deletion-errors"></a>Depolama kaynağı silme hatalarını giderme

Bazı senaryolarda, bir Azure Resource Manager dağıtımında bir Azure Depolama hesabını, kapsayıcısını veya blobu silmeye çalışırken aşağıdaki hatalardan biriyle karşılaşabilirsiniz:

> **' StorageAccountName ' depolama hesabı silinemedi. Hata: depolama hesabı, yapıtları kullanımda olduğundan silinemiyor.**
> 
> **# Tanesi #/kapsayıcı silinemedi: <br> VHD 'ler: Şu anda kapsayıcıda bir kira var ve istekte hiçbir kıra kimliği belirtilmemiş.**
> 
> **# Blob 'ları silme başarısız: <br> blobname. vhd: Şu anda blob üzerinde bir kira var ve istekte hiçbir kıra kimliği belirtilmemiş.**

Azure VM 'lerinde kullanılan VHD 'ler, Azure 'da standart veya Premium bir depolama hesabında sayfa Blobları olarak depolanan. vhd dosyalarıdır. Azure diskleri hakkında daha fazla bilgi için bkz. [yönetilen disklere giriş](../managed-disks-overview.md).

Azure, bir VM 'ye bağlı bir diskin bozulmasını engellemek için silinmesini engeller. Ayrıca, bir VM 'ye bağlı bir Sayfa Blobu olan kapsayıcıların ve depolama hesaplarının silinmesini engeller. 

Bu hatalardan birini alırken bir depolama hesabını, kapsayıcıyı veya blobu silme işlemi şunlardır: 
1. Bir VM 'ye bağlı Blobları tanımla
2. [Bağlı **Işletim sistemi diski** Ile VM 'leri silme](#step-2-delete-vm-to-detach-os-disk)
3. [Tüm **veri diskleri (** ler) kalan VM 'lerden ayır](#step-3-detach-data-disk-from-the-vm)

Bu adımlar tamamlandıktan sonra depolama hesabını, kapsayıcıyı veya blobu silme işlemini yeniden deneyin.

## <a name="step-1-identify-blob-attached-to-a-vm"></a>1. Adım: bir VM 'ye bağlı blobu tanımla

### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Senaryo 1: blob silme – bağlı VM 'yi tanımla
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Merkez menüsünde **Tüm kaynaklar**'ı seçin. Depolama hesabına gidin, **BLOB hizmeti** kapsayıcılar ' ın altında **kapsayıcı**' yı seçin ve silinecek bloba gidin.
3. Blob **kira durumu** **kiralandıysanız**, sağ tıklayıp blob meta verileri bölmesini açmak için **meta verileri Düzenle** ' yi seçin. 

    ![Depolama hesabı Blobları ile portalın ekran görüntüsü ve sağ tıklama > "meta verileri Düzenle" vurgulanmış](./media/troubleshoot-vhds/utd-edit-metadata-sm.png)

4. Blob meta verileri bölmesinde **MicrosoftAzureCompute_VMName** için değeri denetleyin ve kaydedin. Bu değer, VHD 'nin eklendiği VM 'nin adıdır. (Bu alan yoksa **önemli** bölümüne bakın)
5. Blob meta verileri bölmesinde **MicrosoftAzureCompute_DiskType** değerini denetleyin ve kaydedin. Bu değer, eklenen diskin işletim sistemi mi yoksa veri diski mi olduğunu tanımlar (Bu alan yoksa **önemli** bölümüne bakın). 

     ![Depolama "blob meta verileri" bölmesi açık olan portalın ekran görüntüsü](./media/troubleshoot-vhds/utd-blob-metadata-sm.png)

6. Blob disk türü **OSDisk** ise [2. Adım: işletim sistemi DISKINI ayırmak için VM silme ' ye gidin](#step-2-delete-vm-to-detach-os-disk). Aksi takdirde, blob disk türü **Datadisk** ise [Adım 3: VERI diski 'Ni VM 'den ayırma](#step-3-detach-data-disk-from-the-vm)' daki adımları izleyin. 

> [!IMPORTANT]
> Blob meta verilerinde **MicrosoftAzureCompute_VMName** ve **MicrosoftAzureCompute_DiskType** görünmezse, Blobun açıkça KIRALANDıĞıNı ve bir VM 'ye iliştirilmediğini belirtir. Kiralanan blob 'lar önce kirayı bozmadan silinemez. Kirayı kesmek için bloba sağ tıklayıp **kirayı kes**' i seçin. Bir VM 'ye eklenmemiş olan kiralanan Bloblar Blobun silinmesini engeller, ancak kapsayıcı veya depolama hesabının silinmesini engellemez.

### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>Senaryo 2: kapsayıcıyı silme-VM 'lere bağlı kapsayıcı içindeki tüm Blobları tanımla
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Merkez menüsünde **Tüm kaynaklar**'ı seçin. Depolama hesabına gidin, **BLOB hizmeti** kapsayıcılar ' ın altında **kapsayıcı**' yı seçin ve silinecek kapsayıcıyı bulun.
3. Kapsayıcıyı açmak için tıklayın ve içindeki Blobların listesi görüntülenir. Blob türü = **Sayfa Blobu** ve kira durumu = ile **kiralanmış** tüm Blobları bu listeden tanımla. Bu Blobların her biriyle ilişkili VM 'yi tanımlamak için Senaryo 1 ' i izleyin.

    ![Depolama hesabı Blobları ve "kiralanan" vurgulanmış "kira durumu" ile portalın ekran görüntüsü](./media/troubleshoot-vhds/utd-disks-sm.png)

4. **OSDisk** ile VM 'Leri ve **veri diski** ayır 'ı silmek için [Adım 2](#step-2-delete-vm-to-detach-os-disk) ve [3. adım](#step-3-detach-data-disk-from-the-vm) 'ı izleyin. 

### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Senaryo 3: depolama hesabını silme-VM 'lere bağlı depolama hesabı içindeki tüm blob 'ları tanımla
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Merkez menüsünde **Tüm kaynaklar**'ı seçin. Depolama hesabına gidin, **BLOB hizmeti** altında **Bloblar**' ı seçin.
3. **Kapsayıcılar** bölmesinde, **kira durumunun** **kiraladığı** tüm kapsayıcıları tanımlayabilir ve **kiralanan** her kapsayıcı için [Senaryo 2](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) ' yi izleyin.
4. **OSDisk** ile VM 'Leri ve **veri diski** ayır 'ı silmek için [Adım 2](#step-2-delete-vm-to-detach-os-disk) ve [3. adım](#step-3-detach-data-disk-from-the-vm) 'ı izleyin. 

## <a name="step-2-delete-vm-to-detach-os-disk"></a>2. Adım: işletim sistemi diskini ayırmak için VM 'yi silme
VHD bir işletim sistemi diskiyorsa, eklenen VHD 'nin silinebilmesi için önce VM 'yi silmeniz gerekir. Bu adımlar tamamlandıktan sonra aynı VM 'ye bağlı veri diskleri için başka bir eylem gerekli olmaz:

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Hub menüsünde **sanal makineler**' i seçin.
3. VHD 'nin bağlı olduğu VM 'yi seçin.
4. Sanal makineyi etkin bir şekilde kullanan hiçbir şeyin olmadığından ve artık sanal makineye ihtiyacınız olmadığından emin olun.
5. **Sanal makine ayrıntıları** bölmesinin üst kısmında **Sil**' i seçin ve ardından onaylamak için **Evet** ' i tıklatın.
6. VM silinmeli, ancak VHD korunabilir. Ancak, VHD 'nin artık bir VM 'ye bağlı olmaması veya üzerinde bir kiralama olması gerekir. Kira yayımlanamadığında birkaç dakika sürebilir. Kiralamanın serbest bırakıldığını doğrulamak için blob konumuna gidin ve **BLOB özellikleri** bölmesinde **kira durumunun** **kullanılabilir** olması gerekir.

## <a name="step-3-detach-data-disk-from-the-vm"></a>3. Adım: veri diskini VM 'den ayırma
VHD bir veri diskise, kirayı kaldırmak için VHD 'yi VM 'den ayırın:

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Hub menüsünde **sanal makineler**' i seçin.
3. VHD 'nin bağlı olduğu VM 'yi seçin.
4. **Sanal makine ayrıntıları** bölmesinde **diskler** ' i seçin.
5. VHD 'nin bağlı olduğu silinecek veri diskini seçin. VHD 'nin URL 'sini denetleyerek diskte hangi Blobun bağlı olduğunu belirleyebilirsiniz.
6. **VHD URI 'si** alanındaki yolu denetlemek için diske tıklayarak blob konumunu doğrulayabilirsiniz.
7. **Diskler** bölümünde **Düzenle** ' yi seçin.
8. Silinecek veri diskinin **Ayır simgesine** tıklayın.

     ![Depolama "blob meta verileri" bölmesi açık olan portalın ekran görüntüsü ve silinecek veri diskinin ayır simgesini vurgular.](./media/troubleshoot-vhds/utd-vm-disks-edit.png)

9. **Kaydet**'i seçin. Disk artık VM 'den ayrılır ve VHD artık kiralanır. Kira yayımlanamadığında birkaç dakika sürebilir. Kiralamanın verildiğini doğrulamak için blob konumuna gidin ve **BLOB özellikleri** bölmesinde, **kira durumu** değeri **kilidinin açık** veya **kullanılabilir** olmalıdır.

[Storage deletion errors in Resource Manager deployment]: #storage-delete-errors-in-rm

