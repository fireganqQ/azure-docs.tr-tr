---
title: Azure Media Services v3 API 'sine bağlanma-Python
description: Bu makalede, Python ile Media Services v3 API 'sine nasıl bağlanacağı gösterilmektedir.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/18/2020
ms.author: inhenkel
ms.custom: devx-track-python
ms.openlocfilehash: 76df8baaf170b05762b93478a496eb1e9ed802d5
ms.sourcegitcommit: f6236e0fa28343cf0e478ab630d43e3fd78b9596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94916755"
---
# <a name="connect-to-media-services-v3-api---python"></a>Media Services v3 API 'sine bağlanma-Python

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu makalede hizmet sorumlusu oturum açma yöntemi kullanılarak Azure Media Services v3 Python SDK 'sına nasıl bağlanabilmeniz gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

- [Python.org](https://www.python.org/downloads/) 'ten Python indirin
- Ortam değişkenini ayarladığınızdan emin olun `PATH`
- [Media Services hesabı oluşturun](./create-account-howto.md). Kaynak grubu adını ve Media Services hesap adını unutduğunuzdan emin olun.
- [Erişim API 'leri](./access-api-howto.md) konusundaki adımları izleyin. Abonelik KIMLIĞI, uygulama KIMLIĞI (istemci KIMLIĞI), kimlik doğrulama anahtarı (gizli) ve sonraki adımda ihtiyacınız olan kiracı KIMLIĞINI kaydedin.

> [!IMPORTANT]
> [Adlandırma kurallarını](media-services-apis-overview.md#naming-conventions)gözden geçirin.

## <a name="install-the-modules"></a>Modülleri yükleme

Python kullanarak Azure Media Services çalışmak için bu modülleri yüklemeniz gerekir.

* `azure-mgmt-resource`Active Directory Için Azure modüllerini içeren modül.
* `azure-mgmt-media`Media Services varlıklarını içeren modül.

    [Python için Media Services SDK 'sının en son sürümünü](https://pypi.org/project/azure-mgmt-media/)aldığınızdan emin olun.

Bir komut satırı aracı açın ve modülleri yüklemek için aşağıdaki komutları kullanın.

```
pip3 install azure-mgmt-resource
pip3 install azure-mgmt-media==3.0.0
```

## <a name="connect-to-the-python-client"></a>Python istemcisine bağlanma

1. Uzantılı bir dosya oluşturun `.py`
1. Dosyayı en sevdiğiniz düzenleyicide açın
1. Aşağıdaki kodu dosyaya ekleyin. Kod, gerekli modülleri içeri aktarır ve Media Services bağlanmak için gereken Active Directory kimlik bilgileri nesnesini oluşturur.

      Değişkenlerin değerlerini [erişim API 'lerinden](./access-api-howto.md) aldığınız değerlere ayarlayın

      ```
      import adal
      from msrestazure.azure_active_directory import AdalAuthentication
      from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD
      from azure.mgmt.media import AzureMediaServices
      from azure.mgmt.media.models import MediaService

      RESOURCE = 'https://management.core.windows.net/'
      # Tenant ID for your Azure Subscription
      TENANT_ID = '00000000-0000-0000-000000000000'
      # Your Service Principal App ID
      CLIENT = '00000000-0000-0000-000000000000'
      # Your Service Principal Password
      KEY = '00000000-0000-0000-000000000000'
      # Your Azure Subscription ID
      SUBSCRIPTION_ID = '00000000-0000-0000-000000000000'
      # Your Azure Media Service (AMS) Account Name
      ACCOUNT_NAME = 'amsv3account'
      # Your Resource Group Name
      RESOUCE_GROUP_NAME = 'AMSv3ResourceGroup'

      LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
      RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

      context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)
      credentials = AdalAuthentication(
          context.acquire_token_with_client_credentials,
          RESOURCE,
          CLIENT,
          KEY
      )

      # The AMS Client
      # You can now use this object to perform different operations to your AMS account.
      client = AzureMediaServices(credentials, SUBSCRIPTION_ID)

      print("signed in")

      # Now that you are authenticated, you can manipulate the entities.
      # For example, list assets in your AMS account
      print (client.assets.list(RESOUCE_GROUP_NAME, ACCOUNT_NAME).get(0))
      ```

1. Dosyayı çalıştırma

## <a name="next-steps"></a>Sonraki adımlar

- [Python SDK 'yı](https://aka.ms/ams-v3-python-sdk)kullanın.
- Media Services [Python başvuru](/python/api/overview/azure/mediaservices/management) belgelerini gözden geçirin.
