---
title: Öğretici-Azure Red Hat OpenShift kümesini silme
description: Bu öğreticide, Azure CLı kullanarak bir Azure Red Hat OpenShift kümesini silmeyi öğrenin
author: sakthi-vetrivel
ms.custom: fasttrack-edit
ms.author: suvetriv
ms.topic: tutorial
ms.service: azure-redhat-openshift
ms.date: 04/24/2020
ms.openlocfilehash: 61347d052aeb5d563f49275d8f072d1079b94f26
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100634813"
---
# <a name="tutorial-delete-an-azure-red-hat-openshift-4-cluster"></a>Öğretici: Azure Red Hat OpenShift 4 kümesini silme

Bu öğreticide, üç üçüncü kısım, OpenShift 4 çalıştıran bir Azure Red Hat OpenShift kümesi silinir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure Red Hat OpenShift kümesini silme


## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, OpenShift web konsolunu kullanarak bir Azure Red Hat OpenShift kümesi oluşturulup bağlanmıştı. Bu adımları yapmadıysanız ve birlikte takip etmek istiyorsanız, [öğretici 1 ile başlayın-Azure Red Hat OpenShift 4 kümesi oluşturun.](tutorial-create-cluster.md)

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğreticide, Azure CLı sürüm 2.6.0 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure CLı 'yi yerel olarak çalıştırıyorsanız `az login` Azure 'da oturum açmak için öğesini çalıştırın.

```bash
az login
```

Birden çok aboneliğe erişiminiz varsa, `az account set -s {subscription ID}` kullanmak istediğiniz abonelikle değiştirmeyi çalıştırın `{subscription ID}` .

## <a name="delete-the-cluster"></a>Küme silme

Önceki öğreticilerde aşağıdaki değişkenler ayarlanmıştır.

```bash
CLUSTER=yourclustername
RESOURCEGROUP=yourresourcegroup
```

Bu değerleri kullanarak kümenizi silin:

```bash
az aro delete --resource-group $RESOURCEGROUP --name $CLUSTER
```

Daha sonra kümeyi silmek isteyip istemediğinizi onaylamanız istenir. İle doğruladıktan sonra `y` , kümenin silinmesi birkaç dakika sürer. Komut tamamlandığında, tüm kaynak grubu ve içindeki tüm kaynaklar (küme dahil) silinir.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Azure Red Hat OpenShift 4 kümesini silme

Resmi [Red Hat OpenShift belgeleriyle](https://docs.openshift.com/container-platform/4.6/welcome/index.html) OpenShift kullanma hakkında daha fazla bilgi edinin
