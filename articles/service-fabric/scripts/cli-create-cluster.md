---
title: Azure CLI Betiği Dağıtma Örneği
description: Azure komut satırı arabirimi 'ni (CLı) kullanarak Azure 'da güvenli bir Service Fabric Linux kümesi oluşturma.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 675f6b9ac3ebb05539432b5febc279275f9de7f6
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98785777"
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Azure’da güvenli bir Service Fabric Linux kümesi oluşturma

Bu komut, otomatik olarak imzalanan sertifika oluşturur, bu sertifikayı bir anahtar kasasına ekler ve sertifikayı yerel ortamda indirir.  Yeni sertifika, dağıtıldığında kümenin güvenliğini sağlamak için kullanılır.  Yeni bir sertifika oluşturmak yerine mevcut sertifikayı da kullanabilirsiniz.  Her iki şekilde de, sertifikanın konu adı, Service Fabric kümesine erişmek için kullandığınız etki alanıyla eşleşmelidir. Kümenin HTTPS yönetim uç noktaları ve Service Fabric Explorer için TLS sağlamak için bu eşleşme gereklidir. Etki alanı için bir CA 'dan bir TLS/SSL sertifikası elde edilemez `.cloudapp.azure.com` . Kümeniz için özel bir etki alanı adı edinmeniz gerekir. CA’dan sertifika istediğinizde sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adıyla eşleşmelidir.

Gerekirse, [Azure CLI](/en-us/cli/azure/install-azure-cli)’yi yükleyin.

## <a name="sample-script"></a>Örnek betik

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu, kümeyi ve ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az sf cluster create](/cli/azure/sf/cluster) | Yeni bir Service Fabric kümesi oluşturur.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure Service Fabric’e yönelik ek Service Fabric CLI örnekleri [Service Fabric CLI örnekleri](../samples-cli.md)’nde bulunabilir.
