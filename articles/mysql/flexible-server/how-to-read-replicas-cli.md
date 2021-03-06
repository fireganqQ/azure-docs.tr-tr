---
title: Azure CLı kullanarak MySQL için Azure veritabanı esnek sunucusu 'nda okuma çoğaltmalarını yönetin.
description: Azure CLı kullanarak MySQL için Azure veritabanı esnek sunucusu 'nda okuma çoğaltmalarını ayarlama ve yönetme hakkında bilgi edinin.
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 10/23/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: a7d63fd76a88430495c9f55200308f63b11c89d4
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96494316"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-flexible-server-using-the-azure-cli"></a>Azure CLı kullanarak MySQL için Azure veritabanı esnek sunucusu 'nda okuma çoğaltmaları oluşturma ve yönetme

> [!IMPORTANT]
> MySQL için Azure veritabanı 'nda çoğaltmaları okuma-esnek sunucu önizlemededir.

Bu makalede, Azure CLı kullanarak MySQL için Azure veritabanı esnek sunucusu 'nda okuma çoğaltmaları oluşturmayı ve yönetmeyi öğreneceksiniz. Okuma çoğaltmaları hakkında daha fazla bilgi edinmek için bkz. [genel bakış](concepts-read-replicas.md).

> [!Note]
> Çoğaltma, yüksek kullanılabilirlik etkinleştirilmiş sunucuda desteklenmiyor. 

## <a name="azure-cli"></a>Azure CLI
Azure CLı kullanarak okuma çoğaltmaları oluşturabilir ve yönetebilirsiniz.

### <a name="prerequisites"></a>Önkoşullar

- [Azure CLI 2.0’ı yükleme](/cli/azure/install-azure-cli)
- Kaynak sunucu olarak kullanılacak, [MySQL Için Azure veritabanı esnek sunucu](quickstart-create-server-cli.md) .

### <a name="create-a-read-replica"></a>Okuma amaçlı çoğaltma oluşturma

> [!IMPORTANT]
> Mevcut çoğaltmaları olmayan bir kaynak için çoğaltma oluşturduğunuzda, kaynak ilk olarak çoğaltma için hazırlamak üzere yeniden başlatılır. Bunu dikkate alın ve yoğun bir süre boyunca bu işlemleri gerçekleştirin.

Aşağıdaki komut kullanılarak bir okuma çoğaltması sunucusu oluşturulabilir:

```azurecli-interactive
az mysql flexible-server replica create --replica-name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup
``` 

> [!NOTE]
> Okuma çoğaltmaları kaynakla aynı sunucu yapılandırmasıyla oluşturulur. Çoğaltma sunucusu yapılandırması oluşturulduktan sonra değiştirilebilir. Çoğaltma sunucusu her zaman aynı kaynak grubunda, aynı konum ve kaynak sunucuyla aynı abonelikte oluşturulur. Farklı bir kaynak grubuna veya farklı bir aboneliğe çoğaltma sunucusu oluşturmak istiyorsanız, oluşturulduktan sonra [Çoğaltma sunucusunu taşıyabilirsiniz](../../azure-resource-manager/management/move-resource-group-and-subscription.md) . Çoğaltmanın kaynakla devam edebileceğinden emin olmak için çoğaltma sunucusunun yapılandırmasının kaynaktan eşit veya daha büyük bir değer tutulması önerilir.


### <a name="list-replicas-for-a-source-server"></a>Kaynak sunucu için çoğaltmaları listeleme

Belirli bir kaynak sunucu için tüm çoğaltmaları görüntülemek için aşağıdaki komutu çalıştırın: 

```azurecli-interactive
az mysql flexible-server replica list --server-name mydemoserver --resource-group myresourcegroup
```

### <a name="stop-replication-to-a-replica-server"></a>Çoğaltma sunucusuna çoğaltmayı durdur

> [!IMPORTANT]
> Bir sunucuya çoğaltma durdurulduğunda geri alınamaz. Bir kaynak ve çoğaltma arasında çoğaltma durdurulduktan sonra geri alınamaz. Çoğaltma sunucusu daha sonra tek başına bir sunucu olur ve artık hem okuma hem de yazma işlemlerini destekler. Bu sunucu tekrar bir çoğaltmaya yapılamaz.

Bir okuma çoğaltması sunucusuna çoğaltma, aşağıdaki komut kullanılarak durdurulabilir:

```azurecli-interactive
az mysql flexible-server replica stop-replication --replica-name mydemoreplicaserver --resource-group myresourcegroup
```

### <a name="delete-a-replica-server"></a>Çoğaltma sunucusunu silme

**[Az MySQL Server DELETE](/cli/azure/mysql/server)** komutu çalıştırılarak bir okuma çoğaltması sunucusunu silme işlemi gerçekleştirilebilir.

```azurecli-interactive
az mysql flexible-server delete --resource-group myresourcegroup --name mydemoreplicaserver
```

### <a name="delete-a-source-server"></a>Kaynak sunucuyu silme

> [!IMPORTANT]
> Bir kaynak sunucu durdurulduğunda, tüm çoğaltma sunucularına çoğaltma durdurulur ve kaynak sunucu silinir. Çoğaltma sunucuları artık hem okuma hem de yazma işlemlerini destekleyen tek başına sunucular haline gelir.

Bir kaynak sunucuyu silmek için **[az MySQL esnek-sunucu Delete](/cli/azure/mysql/flexible-server)** komutunu çalıştırabilirsiniz.

```azurecli-interactive
az mysql flexible-server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar

- [Okuma çoğaltmaları](concepts-read-replicas.md) hakkında daha fazla bilgi edinin