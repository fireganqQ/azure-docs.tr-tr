---
title: Sunucuyu yeniden başlatma-Azure CLı-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, Azure CLı kullanarak bir PostgreSQL için Azure veritabanı 'nı (tek sunucu) nasıl yeniden başlatabileceğinizi açıklanmaktadır
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: e812b7872dd4d41d9a6cb87d75403524106c5981
ms.sourcegitcommit: 0830e02635d2f240aae2667b947487db01f5fdef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2020
ms.locfileid: "97706873"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-cli"></a>Azure CLı kullanarak PostgreSQL için Azure veritabanı 'nı yeniden başlatma-tek sunucu
Bu konuda, PostgreSQL için Azure veritabanı sunucusunu nasıl yeniden başlatabileceğinizi açıklar. Sunucu işlemi gerçekleştirdiğinden kısa süreli bir kesinti oluşmasına neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatması engellenir. Örneğin, hizmet, sanal çekirdekleri ölçeklendirirken daha önce istenen bir işlemi işliyor olabilir.
 
> [!NOTE] 
> Yeniden başlatmayı tamamlaması için gereken süre, PostgreSQL kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda gerçekleşen etkinlik miktarını en aza indirmenizi öneririz. Ayrıca, kontrol noktası sıklığını artırmak isteyebilirsiniz. Ayrıca dahil olmak üzere denetim noktası ilişkili parametre değerlerini de ayarlayabilirsiniz `max_wal_size` . Ayrıca `CHECKPOINT` , sunucuyu yeniden başlatmadan önce komutunu çalıştırmanız önerilir.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için:
- [PostgreSQL Için Azure veritabanı sunucusu](quickstart-create-server-up-azure-cli.md)oluşturun.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- Bu makale, Azure CLı 'nin 2,0 veya sonraki bir sürümünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür.

## <a name="restart-the-server"></a>Sunucuyu yeniden başlatın

Aşağıdaki komutla sunucuyu yeniden başlatın:

```azurecli-interactive
az postgres server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

[PostgreSQL Için Azure veritabanı 'nda parametrelerin nasıl ayarlanacağı](howto-configure-server-parameters-using-cli.md) hakkında bilgi edinin
