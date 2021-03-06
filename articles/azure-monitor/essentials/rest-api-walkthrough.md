---
title: Azure İzleme REST API'si adım adım kılavuzu
description: Kullanılabilir ölçüm tanımlarını ve ölçüm değerlerini almak için isteklerin kimliğini doğrulama ve Azure Izleyici REST API kullanma.
ms.subservice: metrics
ms.topic: conceptual
ms.date: 03/19/2018
ms.custom: has-adal-ref
ms.openlocfilehash: 41c2432c3c80be7d28e8f33372dea44ba108c5d3
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100623120"
---
# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure İzleme REST API'si adım adım kılavuzu

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede, kodunuzun [Microsoft Azure Monitor REST API başvurusunu](/rest/api/monitor/)kullanabilmesi için kimlik doğrulamanın nasıl gerçekleştirileceği gösterilmektedir.

Azure Izleyici API 'SI, kullanılabilir varsayılan ölçüm tanımlarını, ayrıntı düzeyini ve ölçüm değerlerini programlı bir şekilde almayı mümkün kılar. Veriler, Azure SQL veritabanı, Azure Cosmos DB veya Azure Data Lake gibi ayrı bir veri deposuna kaydedilebilir. Gerektiğinde ek analizler gerçekleştirilebilir.

Farklı ölçüm veri noktalarıyla çalışmanın yanı sıra, Izleme API 'SI de uyarı kurallarını listeleyebilir, etkinlik günlüklerini görüntüleyebilir ve çok daha fazlasını yapar. Kullanılabilir işlemlerin tam listesi için [Microsoft Azure izleyicisi REST API başvurusuna](/rest/api/monitor/)bakın.

## <a name="authenticating-azure-monitor-requests"></a>Azure Izleyici isteklerinin kimliğini doğrulama

İlk adım isteğin kimliğini doğrulamadır.

Azure Izleyici API 'sine karşı yürütülen tüm görevler Azure Resource Manager kimlik doğrulama modelini kullanır. Bu nedenle, Azure Active Directory (Azure AD) ile tüm isteklerin doğrulanması gerekir. İstemci uygulamasının kimliğini doğrulamak için bir yaklaşım, bir Azure AD hizmet sorumlusu oluşturmak ve kimlik doğrulaması (JWT) belirtecini almak. Aşağıdaki örnek betik, PowerShell aracılığıyla bir Azure AD hizmet sorumlusu oluşturmayı göstermektedir. Daha ayrıntılı bir adım adım için, [kaynaklara erişmek üzere hizmet sorumlusu oluşturmak üzere Azure PowerShell kullanma](/powershell/azure/create-azure-service-principal-azureps)hakkındaki belgelere bakın. Ayrıca [, Azure Portal aracılığıyla bir hizmet sorumlusu oluşturmak](../../active-directory/develop/howto-create-service-principal-portal.md)da mümkündür.

```powershell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"

# Authenticate to a specific Azure subscription.
Connect-AzAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"
$secureStringPassword = ConvertTo-SecureString -String $pwd -AsPlainText -Force

# Create a new Azure AD application
$azureAdApplication = New-AzADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $secureStringPassword

# Create a new service principal associated with the designated application
New-AzADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Azure Izleyici API 'sini sorgulamak için, istemci uygulamanın kimlik doğrulaması için önceden oluşturulmuş hizmet sorumlusunu kullanması gerekir. Aşağıdaki örnek PowerShell betiği, JWT kimlik doğrulama belirtecini almak için [Active Directory Authentication Library](../../active-directory/azuread-dev/active-directory-authentication-libraries.md) (ADAL) kullanarak bir yaklaşımı gösterir. JWT belirteci, Azure Izleyici REST API isteklerde HTTP yetkilendirme parametresinin bir parçası olarak geçirilir.

```powershell
$azureAdApplication = Get-AzADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.microsoftonline.com/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)

$result = $AuthContext.AcquireTokenAsync("https://management.core.windows.net/", $cred).GetAwaiter().GetResult()

# Build an array of HTTP header values
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Kimlik doğrulamasından sonra sorgular Azure Izleyici REST API karşı yürütülür. İki yararlı sorgu vardır:

1. Bir kaynağın ölçüm tanımlarını listeleyin
2. Ölçüm değerlerini al

> [!NOTE]
> Azure REST API kimlik doğrulaması hakkında daha fazla bilgi için lütfen [azure REST API başvurusuna](/rest/api/azure/)bakın.
>
>

## <a name="retrieve-metric-definitions-multi-dimensional-api"></a>Ölçüm tanımlarını alma (çok boyutlu API)

Bir hizmet için kullanılabilen ölçüm listesine erişmek için [Azure Izleyici ölçüm tanımlarını REST API](/rest/api/monitor/metricdefinitions) kullanın.

**Yöntem**: Get

**İstek URI 'si**: https: \/ \/ Management.Azure.com/subscriptions/*{SubscriptionID}*/ResourceGroups/*{resourcegroupname}*/Providers/*{resourceprovidernamespace}* / *{ResourceType}* / *{resourceName}*/Providers/Microsoft.insights/metricDefinitions? api-Version =*{apiversion}*

Örneğin, bir Azure depolama hesabının ölçüm tanımlarını almak için, istek şu şekilde görünür:

```powershell
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricDefinitions?api-version=2018-01-01"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contosostorage-metricdef-results.json" `
                  -Verbose

```

> [!NOTE]
> Çok boyutlu Azure Izleyici ölçümleri REST API kullanarak ölçüm tanımlarını almak için API sürümü olarak "2018-01-01" kullanın.
>
>

Sonuçta elde edilen JSON yanıt gövdesi şu örneğe benzer: (ikinci ölçümün boyut olduğunu unutmayın)

```json
{
    "value": [
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricdefinitions/UsedCapacity",
            "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage",
            "namespace": "Microsoft.Storage/storageAccounts",
            "category": "Capacity",
            "name": {
                "value": "UsedCapacity",
                "localizedValue": "Used capacity"
            },
            "isDimensionRequired": false,
            "unit": "Bytes",
            "primaryAggregationType": "Average",
            "supportedAggregationTypes": [
                "Total",
                "Average",
                "Minimum",
                "Maximum"
            ],
            "metricAvailabilities": [
                {
                    "timeGrain": "PT1H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT6H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT12H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "P1D",
                    "retention": "P93D"
                }
            ]
        },
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metricdefinitions/Transactions",
            "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage",
            "namespace": "Microsoft.Storage/storageAccounts",
            "category": "Transaction",
            "name": {
                "value": "Transactions",
                "localizedValue": "Transactions"
            },
            "isDimensionRequired": false,
            "unit": "Count",
            "primaryAggregationType": "Total",
            "supportedAggregationTypes": [
                "Total"
            ],
            "metricAvailabilities": [
                {
                    "timeGrain": "PT1M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT5M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT15M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT30M",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT1H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT6H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "PT12H",
                    "retention": "P93D"
                },
                {
                    "timeGrain": "P1D",
                    "retention": "P93D"
                }
            ],
            "dimensions": [
                {
                    "value": "ResponseType",
                    "localizedValue": "Response type"
                },
                {
                    "value": "GeoType",
                    "localizedValue": "Geo type"
                },
                {
                    "value": "ApiName",
                    "localizedValue": "API name"
                }
            ]
        },
        ...
    ]
}
```

## <a name="retrieve-dimension-values-multi-dimensional-api"></a>Boyut değerlerini alma (çok boyutlu API)

Kullanılabilir Ölçüm tanımları bilindikten sonra, boyutları olan bazı ölçümler olabilir. Ölçüyü sorgulamadan önce, bir boyutun değerlerinin hangi aralıkta olduğunu bulmayı isteyebilirsiniz. Bu boyut değerlerine göre, ölçümleri sorgularken boyut değerlerine göre ölçümleri filtreleyip segmentlere bölüyi tercih edebilirsiniz.  Bunu başarmak için [Azure Izleyici ölçümleri REST API](/rest/api/monitor/metrics) kullanın.

Herhangi bir filtreleme isteği için ölçüm adı ' Value ' değerini (' localizedValue ' değil) kullanın. Filtre belirtilmemişse, varsayılan ölçüm döndürülür. Bu API 'nin kullanımı yalnızca bir boyutun bir joker karakter filtresine sahip olmasına izin verir.

> [!NOTE]
> Azure Izleyici REST API kullanarak boyut değerlerini almak için API sürümü olarak "2018-01-01" kullanın.
>
>

**Yöntem**: Get

**İstek URI 'si**: \: https//Management.Azure.com/subscriptions/*{Subscription-ID}*/ResourceGroups/*{Resource-Group-Name}*/Providers/*{Resource-Provider-Namespace}* / *{Resource-Type}* / *{Resource-Name}*/Providers/Microsoft.insights/Metrics? metricnames =*{Metric}*&TimeSpan =*{StartTime/bitişsaati}*&$Filter =*{Filter}*&ResultType = Metadata&api-Version =*{apiversion}*

Örneğin, ' Işlemler ' ölçümü için "API adı boyutu", belirtilen zaman aralığı boyunca GeoType Dimension = ' PRIMARY ' olan ' API Name Dimension ' için yayınlanan boyut değerlerinin listesini almak için, istek aşağıdaki gibi olacaktır:

```powershell
$filter = "APIName eq '*' and GeoType eq 'Primary'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metrics?metricnames=Transactions&timespan=2018-03-01T00:00:00Z/2018-03-02T00:00:00Z&resultType=metadata&`$filter=${filter}&api-version=2018-01-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosostorage-dimension-values.json" `
    -Verbose
```

Elde edilen JSON yanıt gövdesi, aşağıdaki örneğe benzer olacaktır:

```json
{
  "timespan": "2018-03-01T00:00:00Z/2018-03-02T00:00:00Z",
  "value": [
    {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/Microsoft.Insights/metrics/Transactions",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Transactions",
        "localizedValue": "Transactions"
      },
      "unit": "Count",
      "timeseries": [
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "DeleteBlob"
            }
          ]
        },
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "SetBlobProperties"
            }
          ]
        },
        ...
      ]
    }
  ],
  "namespace": "Microsoft.Storage/storageAccounts",
  "resourceregion": "eastus"
}
```

## <a name="retrieve-metric-values-multi-dimensional-api"></a>Ölçüm değerlerini alma (çok boyutlu API)

Kullanılabilir Ölçüm tanımları ve olası boyut değerleri bilindikten sonra ilgili ölçüm değerlerini almak mümkündür.  Bunu başarmak için [Azure Izleyici ölçümleri REST API](/rest/api/monitor/metrics) kullanın.

Herhangi bir filtreleme isteği için ölçüm adı ' Value ' değerini (' localizedValue ' değil) kullanın. Hiçbir boyut filtresi belirtilmemişse, toplanan toplu ölçüm döndürülür. Ölçüm sorgusu birden çok timeseries döndürürse, zaman serisi sınırlı bir sıralı listesini döndürmek için ' top ' ve ' OrderBy ' sorgu parametrelerini kullanabilirsiniz.

> [!NOTE]
> Azure Izleyici REST API kullanarak çok boyutlu ölçüm değerlerini almak için API sürümü olarak "2018-01-01" kullanın.
>
>

**Yöntem**: Get

**İstek URI 'si**: https \/ :/*Management.Azure.com/subscriptions/{Subscription-ID}*/ResourceGroups/*{Resource-Group-Name}*/Providers/*{Resource-Provider-Namespace}* / *{Resource-Type}* / *{Resource-Name}*/Providers/Microsoft.insights/Metrics? metricnames =*{Metric}*&TimeSpan =*{StartTime/bitişsaati}*&$Filter =*{Filter}*&Interval =*{timegre}*&toplama =*{Aggreation}*&api-Version =*{apiversion}*

Örneğin, en iyi 3 API 'yi azalan değerde, bir 5 dakikalık Aralık sırasında ' Işlem ' sayısına göre, yani GeotType 'ın ' PRIMARY ' olması halinde, istek şu şekildedir:

```powershell
$filter = "APIName eq '*' and GeoType eq 'Primary'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/microsoft.insights/metrics?metricnames=Transactions&timespan=2018-03-01T02:00:00Z/2018-03-01T02:05:00Z&`$filter=${filter}&interval=PT1M&aggregation=Total&top=3&orderby=Total desc&api-version=2018-01-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosostorage-metric-values.json" `
    -Verbose
```

Elde edilen JSON yanıt gövdesi, aşağıdaki örneğe benzer olacaktır:

```json
{
  "cost": 0,
  "timespan": "2018-03-01T02:00:00Z/2018-03-01T02:05:00Z",
  "interval": "PT1M",
  "value": [
    {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/ContosoStorage/providers/Microsoft.Insights/metrics/Transactions",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Transactions",
        "localizedValue": "Transactions"
      },
      "unit": "Count",
      "timeseries": [
        {
          "metadatavalues": [
            {
              "name": {
                "value": "apiname",
                "localizedValue": "apiname"
              },
              "value": "GetBlobProperties"
            }
          ],
          "data": [
            {
              "timeStamp": "2017-09-19T02:00:00Z",
              "total": 2
            },
            {
              "timeStamp": "2017-09-19T02:01:00Z",
              "total": 1
            },
            {
              "timeStamp": "2017-09-19T02:02:00Z",
              "total": 3
            },
            {
              "timeStamp": "2017-09-19T02:03:00Z",
              "total": 7
            },
            {
              "timeStamp": "2017-09-19T02:04:00Z",
              "total": 2
            }
          ]
        },
        ...
      ]
    }
  ],
  "namespace": "Microsoft.Storage/storageAccounts",
  "resourceregion": "eastus"
}
```

## <a name="retrieve-metric-definitions"></a>Ölçüm tanımlarını al

Bir hizmet için kullanılabilen ölçüm listesine erişmek için [Azure Izleyici ölçüm tanımlarını REST API](/rest/api/monitor/metricdefinitions) kullanın.

**Yöntem**: Get

**İstek URI 'si**: https: \/ \/ Management.Azure.com/subscriptions/*{SubscriptionID}*/ResourceGroups/*{resourcegroupname}*/Providers/*{resourceprovidernamespace}* / *{ResourceType}* / *{resourceName}*/Providers/Microsoft.insights/metricDefinitions? api-Version =*{apiversion}*

Örneğin, bir Azure mantıksal uygulaması için ölçüm tanımlarını almak üzere, istek şu şekilde görünür:

```powershell
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricDefinitions?api-version=2016-03-01"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -OutFile ".\contosotweets-metricdef-results.json" `
                  -Verbose
```

> [!NOTE]
> Azure Izleyici REST API kullanarak ölçüm tanımlarını almak için API sürümü olarak "2016-03-01" kullanın.
>
>

Elde edilen JSON yanıt gövdesi, aşağıdaki örneğe benzer olacaktır:

```json
{
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricdefinitions",
  "value": [
    {
      "name": {
        "value": "RunsStarted",
        "localizedValue": "Runs Started"
      },
      "category": "AllMetrics",
      "startTime": "0001-01-01T00:00:00Z",
      "endTime": "0001-01-01T00:00:00Z",
      "unit": "Count",
      "primaryAggregationType": "Total",
      "resourceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets",
      "resourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets",
      "metricAvailabilities": [
        {
          "timeGrain": "PT1M",
          "retention": "P30D",
          "location": null,
          "blobLocation": null
        },
        {
          "timeGrain": "PT1H",
          "retention": "P30D",
          "location": null,
          "blobLocation": null
        }
      ],
      "properties": null,
      "dimensions": null,
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricdefinitions/RunsStarted",
      "supportedAggregationTypes": [ "None", "Average", "Minimum", "Maximum", "Total", "Count" ]
    }
  ]
}
```

Daha fazla bilgi için bkz. [Azure izleyici 'de bir kaynağın ölçüm tanımlarını listeleme REST API](/rest/api/monitor/metricdefinitions) belgeleri.

## <a name="retrieve-metric-values"></a>Ölçüm değerlerini al

Kullanılabilir Ölçüm tanımları bilindikten sonra ilgili ölçüm değerlerini almak mümkündür. Herhangi bir filtreleme isteği için ölçüm adı ' Value ' değerini (' localizedValue ' değil) kullanın (örneğin, ' CpuTime ' ve ' Requests ' ölçüm veri noktalarını alın). Filtre belirtilmemişse, varsayılan ölçüm döndürülür.

> [!NOTE]
> Azure Izleyici REST API kullanarak ölçüm değerlerini almak için API sürümü olarak "2016-09-01" kullanın.
>
>

**Yöntem**: Get

**İstek URI 'si**: `https:\//management.azure.com/subscriptions/\*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?$filter=*{filter}*&api-version=*{apiVersion}*`

Örneğin, belirli bir zaman aralığı ve 1 saatlik zaman dilimi için RunsSucceeded ölçüm veri noktalarını almak için, istek aşağıdaki gibi olacaktır:

```powershell
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2017-08-18T19:00:00 and endTime eq 2017-08-18T23:00:00 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosotweets-metrics-results.json" `
    -Verbose
```

Elde edilen JSON yanıt gövdesi, aşağıdaki örneğe benzer olacaktır:

```json
{
  "value": [
    {
      "data": [
        {
          "timeStamp": "2017-08-18T19:00:00Z",
          "total": 0.0
        },
        {
          "timeStamp": "2017-08-18T20:00:00Z",
          "total": 159.0
        },
        {
          "timeStamp": "2017-08-18T21:00:00Z",
          "total": 174.0
        },
        {
          "timeStamp": "2017-08-18T22:00:00Z",
          "total": 97.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/RunsSucceeded",
      "name": {
        "value": "RunsSucceeded",
        "localizedValue": "Runs Succeeded"
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    }
  ]
}
```

Birden çok veri veya toplama noktası almak için, aşağıdaki örnekte görüldüğü gibi ölçüm tanımı adlarını ve toplama türlerini filtreye ekleyin:

```powershell
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2017-08-18T21:00:00 and endTime eq 2017-08-18T21:30:00 and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=2016-09-01"
Invoke-RestMethod -Uri $request `
    -Headers $authHeader `
    -Method Get `
    -OutFile ".\contosotweets-metrics-multiple-results.json" `
    -Verbose
```

Elde edilen JSON yanıt gövdesi, aşağıdaki örneğe benzer olacaktır:

```json
{
  "value": [
    {
      "data": [
        {
          "timeStamp": "2017-08-18T21:03:00Z",
          "total": 5.0,
          "average": 1.0
        },
        {
          "timeStamp": "2017-08-18T21:04:00Z",
          "total": 7.0,
          "average": 1.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/ActionsCompleted",
      "name": {
        "value": "ActionsCompleted",
        "localizedValue": "Actions Completed "
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    },
    {
      "data": [
        {
          "timeStamp": "2017-08-18T21:03:00Z",
          "total": 5.0,
          "average": 1.0
        },
        {
          "timeStamp": "2017-08-18T21:04:00Z",
          "total": 7.0,
          "average": 1.0
        }
      ],
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/Microsoft.Insights/metrics/RunsSucceeded",
      "name": {
        "value": "RunsSucceeded",
        "localizedValue": "Runs Succeeded"
      },
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    }
  ]
}
```

### <a name="use-armclient"></a>ARMClient kullanma

Ek bir yaklaşım, Windows makinenizde [Armclient](https://github.com/projectkudu/armclient) kullanmaktır. ARMClient, Azure AD kimlik doğrulamasını (ve sonuç JWT belirtecini) otomatik olarak işler. Aşağıdaki adımlarda, ölçüm verilerini almak için ARMClient kullanımı ana hatlarıyla verilmiştir:

1. [Chocolatey](https://chocolatey.org/) ve [armclient](https://github.com/projectkudu/armclient)'ı yükler.
2. Bir Terminal penceresinde *oturumarmclient.exe* yazın. Bunu yaptığınızda Azure 'da oturum açmanız istenir.
3. *Armclient Get [your_resource_id]/Providers/Microsoft.insights/MetricDefinitions? api-Version = 2016-03-01* yazın
4. *Armclient Get [your_resource_id]/Providers/Microsoft.insights/Metrics? api-Version = 2016-09-01* yazın

Örneğin, belirli bir mantıksal uygulamanın ölçüm tanımlarını almak için aşağıdaki komutu verin:

```console
armclient GET /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets/providers/microsoft.insights/metricDefinitions?api-version=2016-03-01
```

## <a name="retrieve-the-resource-id"></a>Kaynak KIMLIĞINI alma

REST API kullanımı, kullanılabilir ölçüm tanımlarını, ayrıntı düzeyini ve ilgili değerleri anlamak için gerçekten yardımcı olabilir. Bu bilgiler, [Azure Yönetim Kitaplığı](/previous-versions/azure/reference/mt417623(v=azure.100))kullanılırken yararlı olur.

Yukarıdaki kod için, kullanılacak kaynak KIMLIĞI, istenen Azure kaynağının tam yoludur. Örneğin, bir Azure Web uygulamasında sorgulama yapmak için kaynak KIMLIĞI şöyle olacaktır:

*/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{site-name}/*

Aşağıdaki listede çeşitli Azure kaynakları için kaynak KIMLIĞI biçimlerinin birkaç örneği yer almaktadır:

* **IoT Hub** -/Subscriptions/*{Subscription-ID}*/ResourceGroups/*{Resource-Group-Name}*/Providers/Microsoft.Devices/IotHubs/*{IoT-Hub-Name}*
* **Elastik SQL havuzu** -/Subscriptions/*{Subscription-ID}*/ResourceGroups/*{Resource-Group-Name}*/Providers/Microsoft.SQL/Servers/*{Pool-DB}*/elakıhavuzs/*{SQL-Pool-Name}*
* **SQL veritabanı (V12)** -/Subscriptions/*{abonelik-kimliği}*/ResourceGroups/*{Resource-Group-Name}*/Providers/Microsoft.SQL/Servers/*{Server-Name}*/Databases/*{Database-Name}*
* **Service Bus** -/Subscriptions/*{Subscription-ID}*/ResourceGroups/*{Resource-Group-Name}*/Providers/Microsoft.ServiceBus/*{Namespace}* / *{ServiceBus-Name}*
* **Sanal Makine Ölçek Kümeleri** -/Subscriptions/*{Subscription-ID}*/ResourceGroups/*{Resource-Group-Name}*/Providers/Microsoft.COMPUTE/virtualMachineScaleSets/*{VM-Name}*
* **VM 'ler** -/Subscriptions/*{Subscription-ID}*/ResourceGroups/*{Resource-Group-Name}*/Providers/Microsoft.COMPUTE/virtualMachines/*{VM-Name}*
* **Event Hubs** -/Subscriptions/*{Subscription-ID}*/ResourceGroups/*{Resource-Group-Name}*/Providers/Microsoft.EventHub/namespaces/*{EventHub-Namespace}*

Azure Kaynak Gezgini kullanma, Azure portal istenen kaynağı görüntüleme, PowerShell veya Azure CLı aracılığıyla kaynak KIMLIĞINI almaya yönelik alternatif yaklaşımlar vardır.

### <a name="azure-resource-explorer"></a>Azure Resource Manager

İstenen bir kaynağın kaynak KIMLIĞINI bulmak için, [Azure Kaynak Gezgini](https://resources.azure.com) aracını kullanmak yararlı bir yaklaşım olabilir. İstenen kaynağa gidin ve aşağıdaki ekran görüntüsünde gösterildiği gibi gösterilen KIMLIĞE bakın:

![Alt "Azure Kaynak Gezgini"](./media/rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure portalı

Kaynak KIMLIĞI Azure portal de elde edilebilir. Bunu yapmak için, istenen kaynağa gidin ve Özellikler ' i seçin. Kaynak KIMLIĞI, aşağıdaki ekran görüntüsünde görüldüğü gibi Özellikler bölümünde görüntülenir:

![Alt "kaynak KIMLIĞI, Azure portal Özellikler dikey penceresinde görüntüleniyor"](./media/rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell

Kaynak KIMLIĞI, Azure PowerShell cmdlet 'leri kullanılarak da alınabilir. Örneğin, bir Azure mantıksal uygulamasının kaynak KIMLIĞINI elde etmek için, aşağıdaki örnekte olduğu gibi Get-AzureLogicApp cmdlet 'ini yürütün:

```powershell
Get-AzLogicApp -ResourceGroupName azmon-rest-api-walkthrough -Name contosotweets
```

Sonuç aşağıdaki örneğe benzer olmalıdır:

```output
Id             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Logic/workflows/ContosoTweets
Name           : ContosoTweets
Type           : Microsoft.Logic/workflows
Location       : centralus
ChangedTime    : 8/21/2017 6:58:57 PM
CreatedTime    : 8/18/2017 7:54:21 PM
AccessEndpoint : https://prod-08.centralus.logic.azure.com:443/workflows/f3a91b352fcc47e6bff989b85446c5db
State          : Enabled
Definition     : {$schema, contentVersion, parameters, triggers...}
Parameters     : {[$connections, Microsoft.Azure.Management.Logic.Models.WorkflowParameter]}
SkuName        :
AppServicePlan :
PlanType       :
PlanId         :
Version        : 08586982649483762729
```

### <a name="azure-cli"></a>Azure CLI’si

Azure CLı kullanarak bir Azure depolama hesabının kaynak KIMLIĞINI almak için `az storage account show` Aşağıdaki örnekte gösterildiği gibi komutunu yürütün:

```azurecli
az storage account show -g azmon-rest-api-walkthrough -n contosotweets2017
```

Sonuç aşağıdaki örneğe benzer olmalıdır:

```json
{
  "accessTier": null,
  "creationTime": "2017-08-18T19:58:41.840552+00:00",
  "customDomain": null,
  "enableHttpsTrafficOnly": false,
  "encryption": null,
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/azmon-rest-api-walkthrough/providers/Microsoft.Storage/storageAccounts/contosotweets2017",
  "identity": null,
  "kind": "Storage",
  "lastGeoFailoverTime": null,
  "location": "centralus",
  "name": "contosotweets2017",
  "networkAcls": null,
  "primaryEndpoints": {
    "blob": "https://contosotweets2017.blob.core.windows.net/",
    "file": "https://contosotweets2017.file.core.windows.net/",
    "queue": "https://contosotweets2017.queue.core.windows.net/",
    "table": "https://contosotweets2017.table.core.windows.net/"
  },
  "primaryLocation": "centralus",
  "provisioningState": "Succeeded",
  "resourceGroup": "azmon-rest-api-walkthrough",
  "secondaryEndpoints": null,
  "secondaryLocation": "eastus2",
  "sku": {
    "name": "Standard_GRS",
    "tier": "Standard"
  },
  "statusOfPrimary": "available",
  "statusOfSecondary": "available",
  "tags": {},
  "type": "Microsoft.Storage/storageAccounts"
}
```

> [!NOTE]
> Azure CLı aracılığıyla Azure Logic Apps henüz kullanılamadığından, önceki örnekte bir Azure depolama hesabı gösterilir.
>
>

## <a name="retrieve-activity-log-data"></a>Etkinlik günlüğü verilerini alma

Ölçüm tanımlarına ve ilgili değerlere ek olarak Azure Izleyici REST API kullanarak Azure kaynaklarıyla ilgili ek ilginç Öngörüler elde edebilirsiniz. Örnek olarak, [etkinlik günlüğü](/rest/api/monitor/activitylogs) verilerini sorgulamak mümkündür. Aşağıdaki örnek istekler, etkinlik günlüğünü sorgulamak için Azure Izleyici REST API kullanır.

Filtre ile etkinlik günlüklerini al:

``` HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2018-01-21T20:00:00Z' and eventTimestamp le '2018-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'
```

Filtreli etkinlik günlüklerini alın ve şunları seçin:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2015-01-21T20:00:00Z' and eventTimestamp le '2015-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Select ile etkinlik günlüklerini al:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Filtre olmadan etkinlik günlüklerini Al veya Seç:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01
```

## <a name="next-steps"></a>Sonraki adımlar

* [Izlemeye genel bakış](../overview.md)konusunu gözden geçirin.
* [Azure izleyici Ile desteklenen ölçümleri](../platform/metrics-supported.md)görüntüleyin.
* [Microsoft Azure Monitor REST API başvurusunu](/rest/api/monitor/)gözden geçirin.
* [Azure Yönetim Kitaplığı](/previous-versions/azure/reference/mt417623(v=azure.100))' nı gözden geçirin.

