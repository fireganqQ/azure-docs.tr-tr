---
title: 'Hızlı başlangıç: bir konsol uygulamasında belirteci al & çağrısı Microsoft Graph | Mavisi'
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, bir .NET Core örnek uygulamasının bir belirteç almak ve Microsoft Graph çağırmak için istemci kimlik bilgileri akışını nasıl kullanabileceğinizi öğrenirsiniz.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/05/2020
ms.author: jmprieur
ms.reviewer: marsma
ms.custom: devx-track-csharp, aaddev, identityplatformtop40, scenarios:getting-started, languages:aspnet-core
ms.openlocfilehash: 99dcd81cd24f762a5c2b55f5f2977aaf61bc26e8
ms.sourcegitcommit: 126ee1e8e8f2cb5dc35465b23d23a4e3f747949c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100103545"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-using-console-apps-identity"></a>Hızlı başlangıç: konsol uygulamasının kimliğini kullanarak bir belirteç alın ve Microsoft Graph API 'sini çağırın

Bu hızlı başlangıçta, bir .NET Core konsol uygulamasının Microsoft Graph API 'yi çağırmak ve dizindeki [kullanıcıların listesini](/graph/api/user-list) göstermek için bir erişim belirteci nasıl alabilirim olduğunu gösteren bir kod örneği indirip çalıştırırsınız. Kod örneği Ayrıca bir işin veya bir Windows hizmetinin bir kullanıcı kimliği yerine bir uygulama kimliğiyle nasıl çalıştırılabileceğinizi gösterir. 

Örneğin bir çizim için [nasıl çalıştığını](#how-the-sample-works) görün.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç, [.NET Core 3,1](https://www.microsoft.com/net/download/dotnet-core)gerektirir.

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme

> [!div renderon="docs" class="sxs-lookup"]
>
> Hızlı başlangıç uygulamanızı başlatmak için iki seçeneğiniz vardır: Express (aşağıdaki 1. seçenek) ve manuel (seçenek 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. <a href="https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/DotNetCoreDaemonQuickstartPage/sourceType/docs" target="_blank">Azure portal uygulama kayıtları</a> hızlı başlangıç deneyimine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma

> [!div renderon="docs"]
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. <a href="https://portal.azure.com/" target="_blank">Azure Portal </span> </a>oturum açın.
> 1. Birden fazla kiracıya erişiminiz varsa, uygulamayı kaydetmek istediğiniz kiracıyı seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: .
> 1. **Azure Active Directory**'yi bulun ve seçin.
> 1. **Yönet** altında   >  **Yeni kayıt** uygulama kayıtları ' yi seçin.
> 1. Uygulamanız için bir **ad** girin (örneğin,) `Daemon-console` . Uygulamanızın kullanıcıları bu adı görebilir ve daha sonra değiştirebilirsiniz.
> 1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
> 1. **Yönet**’in altında **Sertifikalar ve gizli diziler**’i seçin.
> 1. **İstemci gizli** dizileri altında **yeni istemci parolası**' nı seçin, bir ad girin ve ardından **Ekle**' yi seçin. Daha sonraki bir adımda kullanmak üzere gizli bir konuma gizli değeri kaydedin.
> 1. **Yönet** altında **API izinleri**  >  **bir izin Ekle**' yi seçin. **Microsoft Graph** seçin.
> 1. **Uygulama izinleri**' ni seçin.
> 1. **Kullanıcı** düğümü altında **User. Read. All**' ı seçin ve ardından **izin Ekle**' yi seçin.

> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="download-and-configure-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı indirin ve yapılandırın
>
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1. Adım: Uygulamanızı Azure portalında yapılandırma
> Bu hızlı başlangıçtaki kod örneğinin çalışması için bir istemci parolası oluşturun ve Graph API **Kullanıcı. Read. All** uygulama iznini ekleyin.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-netcore-daemon/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-visual-studio-project"></a>2. Adım: Visual Studio projenizi indirme

> [!div renderon="docs"]
> [Visual Studio projesini indirin](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/archive/master.zip)
>
> Belirtilen projeyi Visual Studio 'da veya Mac için Visual Studio çalıştırabilirsiniz.


> [!div class="sxs-lookup" renderon="portal"]
> Visual Studio 2019 kullanarak projeyi çalıştırın.
> [!div class="sxs-lookup" renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-visual-studio-project"></a>3. Adım: Visual Studio projenizi yapılandırma
>
> 1. Zip dosyasını diskin köküne yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) ayıklayın.
> 1. Visual Studio- **1-Call-MSGraph\daemon-Console.sln** (isteğe bağlı) çözümünü açın.
> 1. **Üzerindeappsettings.js** düzenleyin ve alanların değerlerini `ClientId` `Tenant` ve aşağıdaki gibi değiştirin `ClientSecret` :
>
>    ```json
>    "Tenant": "Enter_the_Tenant_Id_Here",
>    "ClientId": "Enter_the_Application_Id_Here",
>    "ClientSecret": "Enter_the_Client_Secret_Here"
>    ```
>   Konum:
>   - `Enter_the_Application_Id_Here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.
>   - `Enter_the_Tenant_Id_Here` -Bu değeri **Kiracı kimliği** veya **kiracı adıyla** değiştirin (örneğin, contoso.Microsoft.com)
>   - `Enter_the_Client_Secret_Here` -Bu değeri, 1. adımda oluşturulan istemci gizli anahtarı ile değiştirin.

> [!div renderon="docs"]
> > [!TIP]
> > **Uygulama (istemci) kimliği**, **Dizin (kiracı) kimliği** değerlerini bulmak Için Azure Portal uygulamanın **genel bakış** sayfasına gidin. Yeni bir anahtar oluşturmak için **sertifikalar & gizlilikler** sayfasına gidin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-admin-consent"></a>3. Adım: yönetici onayı

> [!div renderon="docs"]
> #### <a name="step-4-admin-consent"></a>4. Adım: yönetici onayı

Bu noktada uygulamayı çalıştırmayı denerseniz, *HTTP 403-Yasak* hatası: ' ı alırsınız `Insufficient privileges to complete the operation` . Bunun nedeni, *yalnızca uygulama Izinlerinin* yönetici onayı gerektirdiğinden oluşur. Bu, dizininizin genel yöneticisinin uygulamanıza onay vermesi gerektiği anlamına gelir. Rolünüze bağlı olarak aşağıdaki seçeneklerden birini belirleyin:

##### <a name="global-tenant-administrator"></a>Genel Kiracı Yöneticisi

> [!div renderon="docs"]
> Küresel kiracı yöneticisiyseniz, Azure portalında **kurumsal uygulamalara** gidin > uygulama kaydınızı seçin > sol gezinti bölmesinin güvenlik bölümünde **"Izinler"** i seçin. **{Tenant Name} için yönetici onayı ver** etiketli büyük düğmeyi seçin (burada {Tenant Name} dizininizin adıdır).

> [!div renderon="portal" class="sxs-lookup"]
> Genel yöneticiyseniz, **API izinleri** sayfasına gidin **Enter_the_Tenant_Name_Here Için yönetici onayı ver** ' i seçin
> > [!div id="apipermissionspage"]
> > [API Izinleri sayfasına gidin]()

##### <a name="standard-user"></a>Standart Kullanıcı

Kiracınızın standart bir kullanıcısı kullanıyorsanız, genel bir yöneticiden uygulamanız için yönetici onayı vermesini isteyin. Bunu yapmak için, yöneticinize aşağıdaki URL 'YI verin:

```url
https://login.microsoftonline.com/Enter_the_Tenant_Id_Here/adminconsent?client_id=Enter_the_Application_Id_Here
```

> [!div renderon="docs"]
>> Konum:
>> * `Enter_the_Tenant_Id_Here` -Bu değeri **Kiracı kimliği** veya **kiracı adıyla** değiştirin (örneğin, contoso.Microsoft.com)
>> * `Enter_the_Application_Id_Here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.

> [!NOTE]
> Yukarıdaki URL 'YI kullanarak uygulamaya onay verdikten sonra *' AADSTS50011: uygulama Için hiçbir yanıt adresi kaydedilmedi '* hatasıyla karşılaşabilirsiniz. Bu durum, bu uygulama ve URL 'nin bir yeniden yönlendirme URI 'SI olmadığı için oluşur-lütfen hatayı yoksayın.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-4-run-the-application"></a>4. Adım: uygulamayı çalıştırma

> [!div renderon="docs"]
> #### <a name="step-5-run-the-application"></a>5. Adım: uygulamayı çalıştırma

Visual Studio veya Mac için Visual Studio kullanıyorsanız, uygulamayı çalıştırmak için **F5** tuşuna basın, aksi takdirde uygulamayı komut istemi, konsol veya Terminal aracılığıyla çalıştırın:

```dotnetcli
cd {ProjectFolder}\1-Call-MSGraph\daemon-console
dotnet run
```

> Konum:
> * *{ProjectFolder}* , ZIP dosyasını ayıkladığınız klasördür. Örnek **C:\Azure-Samples\active-Directory-dotnetcore-Daemon-v2**

Sonuç olarak Azure AD dizininizde bulunan kullanıcıların listesini görmeniz gerekir.

> [!IMPORTANT]
> Bu hızlı başlangıç uygulaması, kendisini gizli istemci olarak tanımlamak için bir istemci gizli anahtarı kullanır. İstemci parolası proje dosyalarınıza düz metin olarak eklendiğinden, güvenlik nedenleriyle, uygulamayı üretim uygulaması olarak düşünmeden önce istemci parolası yerine bir sertifika kullanmanız önerilir. Sertifika kullanma hakkında daha fazla bilgi için bu örnekteki GitHub deposunda yer alan [yönergelere](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/#variation-daemon-application-using-client-credentials-with-certificates) bakın.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı
![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-netcore-daemon/netcore-daemon-intro.svg)

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft. Identity. Client](https://www.nuget.org/packages/Microsoft.Identity.Client)), Microsoft Identity platform tarafından korunan bir API 'ye erişmek için kullanılan kullanıcılara ve istek belirteçlerine oturum açmak için kullanılan kitaplıktır. Bu hızlı başlangıç, açıklandığı şekilde, uygulamanın temsilci izinleri yerine kendi kimliğini kullanarak belirteçleri ister. Bu örnekte kullanılan kimlik doğrulama akışı, *[istemci kimlik bilgileri OAuth akışı](v2-oauth2-client-creds-grant-flow.md)* olarak bilinir. İstemci kimlik bilgileri akışı ile MSAL.NET kullanma hakkında daha fazla bilgi için [Bu makaleye](https://aka.ms/msal-net-client-credentials)bakın.

 Visual Studio 'nun **Paket Yöneticisi konsolunda** aşağıdaki komutu çalıştırarak msal.net yükleyebilirsiniz:

```dotnetcli
dotnet add package Microsoft.Identity.Client
```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```csharp
using Microsoft.Identity.Client;
```

Sonra şu kodu kullanarak MSAL'yi başlatın:

```csharp
IConfidentialClientApplication app;
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientSecret(config.ClientSecret)
                                          .WithAuthority(new Uri(config.Authority))
                                          .Build();
```

> | Konum: | Description |
> |---------|---------|
> | `config.ClientSecret` | Azure portalında uygulama için istemci gizli dizisi oluşturulmuştur. |
> | `config.ClientId` | **Uygulama (istemci) Kimliği**, Azure portalda kayıtlı uygulamadır. Bu değeri Azure portalda uygulamanın **Genel bakış** sayfasında bulabilirsiniz. |
> | `config.Authority`    | Seçim Kullanıcının kimlik doğrulaması için STS uç noktası. Genellikle `https://login.microsoftonline.com/{tenant}` {Tenant}, kiracınızın adı veya kiracı kimliğiniz olduğu genel bulut için.|

Daha fazla bilgi için lütfen [başvuru belgelerine `ConfidentialClientApplication` ](/dotnet/api/microsoft.identity.client.iconfidentialclientapplication) bakın

### <a name="requesting-tokens"></a>Belirteç isteme

Uygulamanın kimliğini kullanarak bir belirteç istemek için `AcquireTokenForClient` yöntemi kullanın:

```csharp
result = await app.AcquireTokenForClient(scopes)
                  .ExecuteAsync();
```

> |Konum:| Description |
> |---------|---------|
> | `scopes` | İstenen kapsamları içerir. Gizli istemciler için, `{Application ID URI}/.default` istenen kapsamların Azure portalında ayarlanmış uygulama nesnesi içinde statik olarak tanımlanmış olduğunu göstermek için şuna benzer biçimi kullanmalıdır (Microsoft Graph için, `{Application ID URI}` işaret eder `https://graph.microsoft.com` ). Özel Web API 'Leri için, `{Application ID URI}` Azure portalının uygulama kaydı 'nda (Önizleme) **bir API 'yi kullanıma** sunma bölümünde tanımlanmıştır. |

Daha fazla bilgi için lütfen [başvuru belgelerine `AcquireTokenForClient` ](/dotnet/api/microsoft.identity.client.confidentialclientapplication.acquiretokenforclient) bakın

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daemon uygulamaları hakkında daha fazla bilgi için bkz. senaryoya genel bakış:

> [!div class="nextstepaction"]
> [Web API 'Lerini çağıran Daemon uygulaması](scenario-daemon-overview.md)
