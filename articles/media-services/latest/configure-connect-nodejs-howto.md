---
title: Azure Media Services v3 API 'sine bağlanma-Node.js
description: Bu makalede, Node.js ile Media Services v3 API 'sine nasıl bağlanacağı gösterilmektedir.
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
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-js
ms.openlocfilehash: c6ea238edd68413646dda59b22d1c0dc2557d57e
ms.sourcegitcommit: f6236e0fa28343cf0e478ab630d43e3fd78b9596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94916840"
---
# <a name="connect-to-media-services-v3-api---nodejs"></a>Media Services v3 API 'sine bağlanma-Node.js

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu makalede hizmet sorumlusu oturum açma yöntemi kullanılarak Azure Media Services v3 node.js SDK 'sına nasıl bağlanabilmeniz gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

- [Node.js](https://nodejs.org/en/download/)'i yükler.
- [Media Services hesabı oluşturun](./create-account-howto.md). Kaynak grubu adını ve Media Services hesap adını unutduğunuzdan emin olun.

> [!IMPORTANT]
> [Adlandırma kurallarını](media-services-apis-overview.md#naming-conventions)gözden geçirin.

## <a name="create-packagejson"></a>package.jsoluşturma

1. En sevdiğiniz düzenleyiciyi kullanarak dosyada package.jsoluşturun.
1. Dosyayı açın ve aşağıdaki kodu yapıştırın:

   [JavaScript Için Azudüzeltici Aservices SDK 'sının](https://www.npmjs.com/package/@azure/arm-mediaservices)en son sürümünü aldığınızdan emin olun.

```json
{
  "name": "media-services-node-sample",
  "version": "0.1.0",
  "description": "",
  "main": "./index.js",
  "dependencies": {
    "azure-arm-mediaservices": "^8.0.0",
    "azure-storage": "^2.8.0",
    "ms-rest": "^2.3.3",
    "ms-rest-azure": "^2.5.5"
  }
}
```

Aşağıdaki paketler belirtilmelidir:

|Paket|Description|
|---|---|
|`azure-arm-mediaservices`|SDK Azure Media Services. <br/>En son Azure Media Services paketini kullandığınızdan emin olmak için [NPM 'yi Azure-ARM-mediaservices 'ı yüklemek](https://www.npmjs.com/package/azure-arm-mediaservices/)için denetleyin.|
|`azure-storage`|Depolama SDK 'Sı. Dosyalar varlıklara yüklenirken kullanılır.|
|`ms-rest-azure`| Oturum açmak için kullanılır.|

En son paketi kullandığınızdan emin olmak için aşağıdaki komutu çalıştırabilirsiniz:

```
npm install azure-arm-mediaservices
```

## <a name="connect-to-nodejs-client"></a>Node.js istemcisine Bağlan

1. En sevdiğiniz düzenleyiciyi kullanarak bir. js dosyası oluşturun.
1. Dosyayı açın ve aşağıdaki kodu yapıştırın.
1. "Endpoint config" bölümündeki değerleri, [erişim API 'lerinden](./access-api-howto.md)aldığınız değerlere ayarlayın.

```js
'use strict';

const MediaServices = require('azure-arm-mediaservices');
const msRestAzure = require('ms-rest-azure');
const msRest = require('ms-rest');
const azureStorage = require('azure-storage');

// endpoint config
// make sure your URL values end with '/'
const armAadAudience = "";
const aadEndpoint = "";
const armEndpoint = "";
const subscriptionId = "";
const accountName = "";
const region = "";
const aadClientId = "";
const aadSecret = "";
const aadTenantId = "";
const resourceGroup = "";

let azureMediaServicesClient;

///////////////////////////////////////////
//     Entrypoint for sample script      //
///////////////////////////////////////////

msRestAzure.loginWithServicePrincipalSecret(aadClientId, aadSecret, aadTenantId, {
  environment: {
    activeDirectoryResourceId: armAadAudience,
    resourceManagerEndpointUrl: armEndpoint,
    activeDirectoryEndpointUrl: aadEndpoint
  }
}, async function(err, credentials, subscriptions) {
    if (err) return console.log(err);
    azureMediaServicesClient = new MediaServices(credentials, subscriptionId, armEndpoint, { noRetryPolicy: true });
    
    console.log("connected");

});
```

## <a name="run-your-app"></a>Uygulamanızı çalıştırma

Bir komut istemi açın. Örneğin dizinine gidin ve aşağıdaki komutları yürütün:

```
npm install 
node index.js
```

## <a name="see-also"></a>Ayrıca bkz.

- [Media Services kavramlar](concepts-overview.md)
- [NPM azure-arm-mediaservices yüklemesi](https://www.npmjs.com/package/azure-arm-mediaservices/)

## <a name="next-steps"></a>Sonraki adımlar

Media Services [Node.js başvuru](/javascript/api/overview/azure/mediaservices/management) belgelerini inceleyin ve node.js ile Media Services API 'sinin nasıl kullanılacağını gösteren [örneklere](https://github.com/Azure-Samples/media-services-v3-node-tutorials) göz atın.
