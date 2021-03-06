---
title: Azure Izleyici kaynak günlüklerinde biçim değişikliğine hazırlanma
description: Azure Kaynak günlükleri 1 Kasım 2018 ' de Append blob 'ları kullanmak üzere taşınır.
author: johnkemnetz
services: monitoring
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: e6704eeab853c8d56c6e9cde5b8539b690d9b5c7
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100623150"
---
# <a name="prepare-for-format-change-to-azure-monitor-platform-logs-archived-to-a-storage-account"></a>Azure Izleyici platformu günlüklerine yapılan biçim değişikliğine hazırlanma bir depolama hesabına arşivlendi

> [!WARNING]
> [Günlük profillerini kullanarak bir depolama](./resource-logs.md#send-to-azure-storage)hesabına Tanılama ayarları veya etkinlik günlükleri [kullanarak bir depolama hesabına Azure Kaynak günlükleri veya ölçümleri](./resource-logs.md#send-to-azure-storage) gönderiyorsanız, depolama hesabındaki verilerin BIÇIMI, kas 'de JSON satırları olarak değiştirilmiştir. 1, 2018. Aşağıdaki yönergeler etkisini ve yeni biçimi işlemek için araçlarınızın nasıl güncelleşmesini açıklamaktadır.
>

## <a name="what-changed"></a>Değişen

Azure Izleyici, Azure Izleyici 'deki bir Azure depolama hesabına, Event Hubs ad alanına veya bir Log Analytics çalışma alanına kaynak günlükleri ve etkinlik günlükleri göndermenizi sağlayan bir özellik sunar. Bir sistem performansı sorununa yönelik olarak, **1 kasım 2018 ' de 12:00 gece yarısı UTC** , blob depolamaya gönderilen günlük verilerinin biçimi değişti. Blob depolamadan veri okuyan bir araç varsa, yeni veri biçimini anlamak için araç araçlarınızı güncelleştirmeniz gerekir.

* Salı günü, 1 Kasım 2018, 12:00 gece UTC 'de, blob biçimi [JSON satırları](http://jsonlines.org/)olarak değiştirildi. Bu, her kaydın bir yeni satır tarafından sınırlandırıldığından, dış kayıt dizisi olmadan ve JSON kayıtları arasında virgül olmaması anlamına gelir.
* Blob biçimi tüm aboneliklerdeki tüm Tanılama ayarları için aynı anda değişmiştir. 1 Kasım 'da oluşturulan ilk PT1H.js, bu yeni biçimi kullandı. Blob ve kapsayıcı adları aynı kalır.
* Verileri geçerli biçimde 1 Kasım 'a kadar göstermek için 1 Kasım 'dan önce 1 Kasım 'a kadar devam eden bir tanılama ayarı ayarlama.
* Bu değişiklik, tüm genel bulut bölgelerinde bir kez oluştu. Bu değişiklik, 21Vianet tarafından çalıştırılan Microsoft Azure, Azure Almanya veya Azure Kamu bulutlarında henüz gerçekleşmeyecektir.
* Bu değişiklik aşağıdaki veri türlerini etkiler:
  * [Azure Kaynak günlükleri](./resource-logs.md#send-to-azure-storage) ([kaynakların listesini buraya bakın](./resource-logs-schema.md))
  * [Tanılama ayarları tarafından aktarılan Azure Kaynak ölçümleri](../essentials/diagnostic-settings.md)
  * [Azure etkinlik günlüğü verileri, günlük profilleri tarafından veriliyor](./activity-log.md)
* Bu değişiklik etkilenmez:
  * Ağ akışı günlükleri
  * Azure hizmet günlükleri henüz Azure Izleyici aracılığıyla kullanılamaz (örneğin, kaynak günlükleri Azure App Service, depolama Analizi günlükleri)
  * Azure Kaynak günlüklerinin ve etkinlik günlüklerinin diğer hedeflere yönlendirilmesi (Event Hubs Log Analytics)

### <a name="how-to-see-if-you-are-impacted"></a>Etkilenip etkilenmediğinizi görme

Bu değişiklikten yalnızca şunları yaptıysanız etkilenmiş olursunuz:
1. Bir Azure depolama hesabına bir tanılama ayarı kullanarak günlük verileri gönderiyor ve
2. Depolama alanındaki bu günlüklerin JSON yapısına bağlı olan araçları izleyin.
 
Bir Azure depolama hesabına veri gönderen tanılama ayarlarınıza sahip olup olmadığını belirlemek için portalın **izleyici** bölümüne gidebilir, **Tanılama ayarları**' na tıklayabilir ve **Tanılama durumu** **etkin** olarak ayarlanmış olan tüm kaynakları belirleyebilirsiniz:

![Azure Izleyici Tanılama ayarları dikey penceresi](media/resource-logs-blob-format/portal-diag-settings.png)

Tanı durumu etkin olarak ayarlanırsa, bu kaynak üzerinde etkin bir tanılama ayarınız vardır. Bir depolama hesabına veri gönderip göndermeyen tanılama ayarlarının olup olmadığını görmek için kaynağa tıklayın:

![Depolama hesabı etkin](media/resource-logs-blob-format/portal-storage-enabled.png)

Bu kaynak tanılama ayarlarını kullanarak bir depolama hesabına veri gönderen kaynaklarınız varsa, bu depolama hesabındaki verilerin biçimi bu değişiklikten etkilenir. Bu depolama hesaplarının dışında çalışan özel bir araç yoksa, biçim değişikliği sizi etkilemez.

### <a name="details-of-the-format-change"></a>Biçim değişikliğinin ayrıntıları

Azure Blob depolama alanındaki PT1H.jsdosyanın geçerli biçimi, bir JSON dizi kayıt kullanır. İşte şu anda bir Keykasası günlük dosyası örneği:

```json
{
    "records": [
        {
            "time": "2016-01-05T01:32:01.2691226Z",
            "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
            "operationName": "VaultGet",
            "operationVersion": "2015-06-01",
            "category": "AuditEvent",
            "resultType": "Success",
            "resultSignature": "OK",
            "resultDescription": "",
            "durationMs": "78",
            "callerIpAddress": "104.40.82.76",
            "correlationId": "",
            "identity": {
                "claim": {
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com",
                    "appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"
                }
            },
            "properties": {
                "clientInfo": "azure-resource-manager/2.0",
                "requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01",
                "id": "https://contosokeyvault.vault.azure.net/",
                "httpStatusCode": 200
            }
        },
        {
            "time": "2016-01-05T01:33:56.5264523Z",
            "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
            "operationName": "VaultGet",
            "operationVersion": "2015-06-01",
            "category": "AuditEvent",
            "resultType": "Success",
            "resultSignature": "OK",
            "resultDescription": "",
            "durationMs": "83",
            "callerIpAddress": "104.40.82.76",
            "correlationId": "",
            "identity": {
                "claim": {
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com",
                    "appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"
                }
            },
            "properties": {
                "clientInfo": "azure-resource-manager/2.0",
                "requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01",
                "id": "https://contosokeyvault.vault.azure.net/",
                "httpStatusCode": 200
            }
        }
    ]
}
```

Yeni biçim, her olayın bir satır olduğu ve yeni bir olayı gösterdiği yeni bir olayı gösteren [JSON hatlarını](http://jsonlines.org/)kullanır. Yukarıdaki örnek, değişiklikten sonra dosyada PT1H.jsşöyle görünür:

```json
{"time": "2016-01-05T01:32:01.2691226Z","resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT","operationName": "VaultGet","operationVersion": "2015-06-01","category": "AuditEvent","resultType": "Success","resultSignature": "OK","resultDescription": "","durationMs": "78","callerIpAddress": "104.40.82.76","correlationId": "","identity": {"claim": {"http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com","appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},"properties": {"clientInfo": "azure-resource-manager/2.0","requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id": "https://contosokeyvault.vault.azure.net/","httpStatusCode": 200}}
{"time": "2016-01-05T01:33:56.5264523Z","resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT","operationName": "VaultGet","operationVersion": "2015-06-01","category": "AuditEvent","resultType": "Success","resultSignature": "OK","resultDescription": "","durationMs": "83","callerIpAddress": "104.40.82.76","correlationId": "","identity": {"claim": {"http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com","appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},"properties": {"clientInfo": "azure-resource-manager/2.0","requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id": "https://contosokeyvault.vault.azure.net/","httpStatusCode": 200}}
```

Bu yeni biçim, Azure Izleyici 'nin, yeni olay verilerini sürekli olarak eklemek için daha verimli olan [ekleme Blobları](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-append-blobs)kullanarak günlük dosyalarını gönderebilmesine olanak sağlar.

## <a name="how-to-update"></a>Güncelleştirme

Yalnızca bu günlük dosyalarını daha fazla işlemeye yönelik bir özel araç varsa, güncelleştirmeler yapmanız gerekir. Bir dış Log Analytics veya SıEM aracı kullanıyorsanız, [bunun yerine bu verileri almak için Olay Hub 'larını kullanmanızı](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)öneririz. Olay Hub 'ları tümleştirmesi, belirli bir günlükteki birçok hizmet ve yer işareti konumundan günlükleri işleme açısından daha kolay hale getirir.

Özel araçlar, yukarıda açıklanan hem geçerli biçimi hem de JSON satırları biçimini işleyecek şekilde güncellenecektir. Bu, verilerin yeni biçimde görüntülenmesinden sonra araçlarınızın kesintiye uğramadığından emin olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak kaynak günlüklerini bir depolama hesabına arşivleme](./resource-logs.md#send-to-azure-storage) hakkında bilgi edinin
* [Etkinlik günlüğü verilerini bir depolama hesabına arşivleme](./activity-log.md#legacy-collection-methods) hakkında bilgi edinin
