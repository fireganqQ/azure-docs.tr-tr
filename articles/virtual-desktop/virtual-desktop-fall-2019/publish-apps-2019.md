---
title: Windows sanal masaüstü 'nde (klasik) yerleşik uygulamaları yayımlama-Azure
description: Windows sanal masaüstü 'nde (klasik) yerleşik uygulamaları yayımlama.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 80cd1a4c92441fb17ce0a66814ff0a39a92fb287
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88005576"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop-classic"></a>Windows sanal masaüstü 'nde yerleşik uygulamaları yayımlama (klasik)

>[!IMPORTANT]
>Bu içerik, Windows sanal masaüstü nesneleri Azure Resource Manager desteklemeyen Windows sanal masaüstü (klasik) için geçerlidir. Azure Resource Manager Windows sanal masaüstü nesnelerini yönetmeye çalışıyorsanız, [Bu makaleye](../publish-apps.md)bakın.

Bu makalede, Windows sanal masaüstü ortamınızda uygulamaların nasıl yayımlanacağı açıklanır.

## <a name="publish-built-in-apps"></a>Yerleşik uygulamaları yayımlama

Yerleşik bir uygulama yayımlamak için:

1. Konak havuzunuzdaki sanal makinelerden birine bağlanın.
2. [Bu makaledeki](/powershell/module/appx/get-appxpackage?view=win10-ps/)yönergeleri izleyerek yayımlamak Istediğiniz uygulamanın **PackageFamilyName** alın.
3. Son olarak, `<PackageFamilyName>` önceki adımda bulduğunuz **PackageFamilyName** ile değiştirilmiş aşağıdaki cmdlet 'i çalıştırın:

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:appsFolder\<PackageFamilyName>!App"
   ```

>[!NOTE]
> Windows sanal masaüstü, uygulamaları yalnızca ile başlayan bir konum yüklerken yayımlamayı destekler `C:\Program Files\Windows Apps` .

## <a name="update-app-icons"></a>Uygulama simgelerini Güncelleştir

Bir uygulamayı yayımladıktan sonra, normal simge resmi yerine varsayılan Windows uygulaması simgesine sahip olur. Simgeyi normal simgesine değiştirmek için, istediğiniz simgenin görüntüsünü bir ağ paylaşımında koyun. Desteklenen görüntü biçimleri PNG, BMP, GIF, JPG, JPEG ve ICO.

## <a name="publish-microsoft-edge"></a>Microsoft Edge 'i Yayımla

Microsoft Edge 'i yayımlamak için kullandığınız işlem, diğer uygulamalar için yayımlama işleminden biraz farklıdır. Microsoft Edge 'i varsayılan giriş sayfası ile yayımlamak için şu cmdlet 'i çalıştırın:

```powershell
New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge"
```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanıcıların [Windows sanal masaüstü kullanıcıları için özet akışını özelleştirmek](customize-feed-virtual-desktop-users-2019.md)üzere uygulamaların nasıl görüntülendiğini düzenlemek için akışların nasıl yapılandırılacağı hakkında bilgi edinin.
- Msix uygulama iliştirme özelliği hakkında daha fazla bilgi [edinin.](../app-attach.md)

