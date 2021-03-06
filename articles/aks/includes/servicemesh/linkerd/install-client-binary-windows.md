---
author: paulbouwer
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 1a023475de1ce2891916807632d9ee15e382326c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "81736278"
---
## <a name="download-and-install-the-linkerd-linkerd-client-binary"></a>Linkerd linkerd istemci ikilisini indirin ve yükleyin

Windows üzerinde PowerShell tabanlı bir kabukta, `Invoke-WebRequest` Linkerd sürümünü şu şekilde indirmek için kullanın:

```powershell
# Specify the Linkerd version that will be leveraged throughout these instructions
$LINKERD_VERSION="stable-2.6.0"

# Enforce TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/linkerd/linkerd2/releases/download/$LINKERD_VERSION/linkerd2-cli-$LINKERD_VERSION-windows.exe" -OutFile "linkerd2-cli-$LINKERD_VERSION-windows.exe"
```

`linkerd`İstemci ikili dosyası, istemci makinenizde çalışır ve Linkerd hizmet ağıyla etkileşime girebilmeniz için izin verir. `linkerd`Windows üzerinde PowerShell tabanlı bir kabuğa Linkerd istemci ikilisini yüklemek için aşağıdaki komutları kullanın. Bu komutlar, `linkerd` istemci ikilisini bir Linkerd klasörüne kopyalar ve sonra (geçerli kabukta) ve kalıcı olarak (kabukta yeniden başlatmalar arasında) ile birlikte kullanılabilir hale getirir `PATH` . Bu komutları çalıştırmak için yükseltilmiş (yönetici) ayrıcalıklarına ihtiyacınız yoktur ve kabuğunu yeniden başlatmanız gerekmez.

```powershell
# Copy linkerd.exe to C:\Linkerd
New-Item -ItemType Directory -Force -Path "C:\Linkerd"
Copy-Item -Path ".\linkerd2-cli-$LINKERD_VERSION-windows.exe" -Destination "C:\Linkerd\linkerd.exe"

# Add C:\Linkerd to PATH. 
# Make the new PATH permanently available for the current User
$USER_PATH = [environment]::GetEnvironmentVariable("PATH", "User") + ";C:\Linkerd\"
[environment]::SetEnvironmentVariable("PATH", $USER_PATH, "User")
# Make the new PATH immediately available in the current shell
$env:PATH += ";C:\Linkerd\"
```