---
ms.openlocfilehash: 2afe3e29c5717eb8d4921f82f35083ecbb14828c
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99531607"
---
1. Depoyu bu konumdan kopyala: https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp .
1. Visual Studio Code, deponun indirildiği klasörü açın.
1. Visual Studio Code, *src/buluttan cihaza-Console-App* klasörüne gidin. Burada bir dosya oluşturun ve *appsettings.js*. Bu dosya, programı çalıştırmak için gereken ayarları içerir.
1. Bu hızlı başlangıçta oluşturduğunuz dosyadaki *~/CloudDrive/LVA-Sample/appsettings.js* içeriğini kopyalayın.

    Metin aşağıdaki çıktıya benzer şekilde görünmelidir.

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```
1. *Src/Edge* klasörüne gidin ve *. env* adlı bir dosya oluşturun.
1. */CloudDrive/LVA-Sample/Edge-Deployment/-env* dosyasının içeriğini kopyalayın. Metin aşağıdaki kod gibi görünmelidir.

    ```
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    VIDEO_INPUT_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"  
    VIDEO_OUTPUT_FOLDER_ON_DEVICE="/var/media"
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry password>"      
    ```
    