---
title: Daemon uygulamasından bir Web API 'SI çağırma | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'sini çağıran bir Daemon uygulaması derlemeyi öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: bd0d53049c68843a6fd2cb6128c473d7c4f8d639
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99582800"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>Web API 'Lerini çağıran Daemon uygulaması-uygulamadan bir Web API 'SI çağırma

.NET Daemon uygulamaları, bir Web API 'SI çağırabilir. .NET Daemon uygulamaları, önceden onaylanmış birkaç Web API 'Sini de çağırabilir.

## <a name="calling-a-web-api-from-a-daemon-application"></a>Bir Daemon uygulamasından Web API 'SI çağırma

Bir API 'yi çağırmak için belirtecin nasıl kullanılacağı aşağıda verilmiştir:

# <a name="net"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# <a name="python"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

# <a name="java"></a>[Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

---

## <a name="calling-several-apis"></a>Çeşitli API 'Ler çağırma

Daemon uygulamaları için, çağırdığınız Web API 'Lerinin önceden onaylanmış olması gerekir. Daemon uygulamalarıyla artımlı izin yoktur. (Kullanıcı etkileşimi yoktur.) Kiracı yöneticisinin, uygulama ve tüm API izinleri için önceden onay sağlaması gerekir. Çeşitli API 'Ler çağırmak isterseniz her bir kaynak için her seferinde bir belirteç alın `AcquireTokenForClient` . MSAL, gereksiz hizmet çağrılarını önlemek için uygulama belirteci önbelleğini kullanır.

## <a name="next-steps"></a>Sonraki adımlar

# <a name="net"></a>[.NET](#tab/dotnet)

Bu senaryodaki sonraki makaleye geçin, [üretime geçin](./scenario-daemon-production.md?tabs=dotnet).

# <a name="python"></a>[Python](#tab/python)

Bu senaryodaki sonraki makaleye geçin, [üretime geçin](./scenario-daemon-production.md?tabs=python).

# <a name="java"></a>[Java](#tab/java)

Bu senaryodaki sonraki makaleye geçin, [üretime geçin](./scenario-daemon-production.md?tabs=java).

---
