---
title: SMB kullanarak Linux VM 'lerinde Azure dosya depolamayı bağlama
description: Azure CLı ile SMB kullanarak Linux VM 'lerinde Azure dosya depolama 'yı bağlama
author: cynthn
ms.service: virtual-machines-linux
ms.topic: how-to
ms.workload: infrastructure
ms.date: 06/28/2018
ms.author: cynthn
ms.openlocfilehash: 6a5d4f388d9e5f47a13812687489525590c520c3
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96016106"
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>SMB kullanarak Linux VM 'lerinde Azure dosya depolamayı bağlama

Bu makalede, Azure CLı ile bir SMB bağlama kullanarak bir Linux sanal makinesinde Azure dosya depolama hizmeti 'nin nasıl kullanılacağı gösterilir. Azure dosya depolama, standart SMB protokolünü kullanarak bulutta dosya paylaşımları sunar. 

Dosya depolama, bulutta standart SMB protokolünü kullanan dosya paylaşımları sunar. SMB 3,0 ' ü destekleyen herhangi bir IŞLETIM sisteminden bir dosya paylaşma bağlayabilirsiniz. Linux üzerinde bir SMB bağlama kullandığınızda, bir SLA tarafından desteklenen sağlam, kalıcı bir arşivleme depolama konumuna kolayca yedeklemeler elde edersiniz.

Dosyaları bir VM 'den dosya depolamada barındırılan SMB bağlamasına taşımak, günlükleri hata ayıklamanın harika bir yoludur. Aynı SMB paylaşımının Mac, Linux veya Windows iş istasyonunuza yerel olarak bağlanması gerekir. SMB protokolü, bu tür ağır günlüğe kaydetme görevlerini işleyecek şekilde oluşturulmadığından, SMB, Linux veya uygulama günlüklerinin gerçek zamanlı olarak akışını sağlamak için en iyi çözümdür. Floentd gibi adanmış, Birleşik bir günlüğe kaydetme katmanı aracı, Linux ve uygulama günlüğü çıkışının toplanması için SMB 'den daha iyi bir seçimdir.

Bu kılavuzda, Azure CLı sürüm 2.0.4 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için **az--Version** ' i çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

*Doğu ABD* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturun.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[Az Storage Account Create](/cli/azure/storage/account)kullanılarak oluşturduğunuz kaynak grubu içinde yeni bir depolama hesabı oluşturun. Bu örnek, *Mystorageacct \<random number>* adlı bir depolama hesabı oluşturur ve bu depolama hesabının adını **storageacct** değişkenine koyar. Depolama hesabı adları benzersiz olmalıdır, bu, ' ı kullanarak `$RANDOM` sonuna bir sayı ekler ve benzersiz hale getirir.

```azurecli
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

## <a name="get-the-storage-key"></a>Depolama anahtarını al

Bir depolama hesabı oluşturduğunuzda, hizmet kesintisi olmadan döndürülebilen şekilde hesap anahtarları çiftler halinde oluşturulur. Çiftin ikinci anahtarına geçtiğinizde yeni bir anahtar çifti oluşturursunuz. Yeni depolama hesabı anahtarları her zaman çiftler halinde oluşturulur, bu nedenle her zaman en az bir kullanılmamış depolama hesabı anahtarına geçiş yapmaya hazır olursunuz.

Depolama hesabı anahtarlarını [az Storage Account Keys List](/cli/azure/storage/account/keys)kullanarak görüntüleyin. Bu örnek, **StorageKey** değişkeninde anahtar 1 değerini depolar.

```azurecli
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma

[Az Storage Share Create](/cli/azure/storage/share)kullanarak dosya depolama payını oluşturun. 

Paylaşma adları, küçük harf, sayı ve tek kısa çizgilerden oluşmalıdır, ancak kısa çizgi ile başlayamaz. Dosya paylaşımlarını ve dosyaları adlandırma hakkında tüm ayrıntılar için bkz. [adlandırma ve başvuru paylaşımları, dizinler, dosyalar ve meta veriler](/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata).

Bu örnek, 10-GiB kotası ile *myshare* adlı bir paylaşma oluşturur. 

```azurecli
az storage share create --name myshare \
    --quota 10 \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY
```

## <a name="create-a-mount-point"></a>Bağlama noktası oluşturma

Azure dosya paylaşımının Linux bilgisayarınıza bağlanması için, **CIFS-utils** paketinin yüklü olduğundan emin olmanız gerekir. Yükleme yönergeleri için bkz. [Linux dağıtım için CIFS-utils paketini yükleme](../../storage/files/storage-how-to-use-files-linux.md#install-cifs-utils).

Azure dosyaları, TCP bağlantı noktası 445 üzerinden iletişim kuran SMB protokolünü kullanır.  Azure dosya paylaşımınızı bağlama konusunda sorun yaşıyorsanız, güvenlik duvarının TCP bağlantı noktası 445 ' i engellemediğinden emin olun.


```bash
mkdir -p /mnt/MyAzureFileShare
```

## <a name="mount-the-share"></a>Paylaşma bağlama

Azure dosya paylaşımından yerel dizine bağlayın. 

```bash
sudo mount -t cifs //$STORAGEACCT.file.core.windows.net/myshare /mnt/MyAzureFileShare -o vers=3.0,username=$STORAGEACCT,password=$STORAGEKEY,dir_mode=0777,file_mode=0777,serverino
```

Yukarıdaki komut, [CIFS](https://linux.die.net/man/8/mount.cifs)'ye özgü Azure dosya paylaşımından ve seçeneklerini bağlamak için [Mount](https://linux.die.net/man/8/mount) komutunu kullanır. Özellikle, file_mode ve dir_mode seçenekleri dosya ve dizinleri izin olarak ayarlar `0777` . `0777`İzin, tüm kullanıcılara okuma, yazma ve yürütme izinleri verir. Değerleri diğer [chmod izinleriyle](https://en.wikipedia.org/wiki/Chmod)değiştirerek bu izinleri değiştirebilirsiniz. Ayrıca GID veya Uid gibi diğer [CIFS](https://linux.die.net/man/8/mount.cifs) seçeneklerini de kullanabilirsiniz. 


## <a name="persist-the-mount"></a>Bağlama devam ettir

Linux VM 'yi yeniden başlattığınızda, bağlı SMB paylaşımının kapalı olması sırasında takılmasını sağlayabilirsiniz. Önyükleme sırasında SMB paylaşımının yeniden bağlantısını yapmak için Linux/etc/fstabto 'a bir satır ekleyin. Linux, önyükleme işlemi sırasında bağlanması gereken dosya sistemlerini listelemek için fstab dosyasını kullanır. SMB paylaşımının eklenmesi, dosya depolama paylaşımının Linux VM için kalıcı olarak bağlanmış bir dosya sistemi olmasını sağlar. Cloud-init kullandığınızda dosya depolama SMB paylaşımının yeni bir sanal makineye eklenmesi mümkündür.

```bash
//myaccountname.file.core.windows.net/mystorageshare /mnt/mymountpoint cifs vers=3.0,username=mystorageaccount,password=myStorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

Üretim ortamlarında daha fazla güvenlik için kimlik bilgilerinizi fstab dışında depolamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Oluşturma sırasında bir Linux sanal makinesini özelleştirmek için Cloud-init kullanma](using-cloud-init.md)
- [Linux VM'ye disk ekleme](add-disk.md)
- [Linux VM'leri için Azure Disk Şifrelemesi](disk-encryption-overview.md)
