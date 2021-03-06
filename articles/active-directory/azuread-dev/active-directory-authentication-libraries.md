---
title: Azure Active Directory kimlik doğrulama kitaplıkları | Microsoft Docs
description: Azure AD kimlik doğrulama kitaplığı (ADAL), istemci uygulama geliştiricilerinin kullanıcıların bulut veya şirket içi Active Directory (AD) için kolayca kimlik doğrulaması yapmasına ve ardından API çağrılarını güvenli hale getirmek için erişim belirteçleri almasına olanak tanır.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.topic: reference
ms.workload: identity
ms.date: 12/01/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 7b6ffd885e1be2dd59cea87cacd6e5fd5e0f8f49
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96861179"
---
# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory Kimlik Doğrulama Kitaplıkları

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Azure Active Directory kimlik doğrulama kitaplığı (ADAL) v 1.0, uygulama geliştiricilerinin kullanıcıların bulut veya şirket içi Active Directory (AD) kimliğini doğrulamasını ve API çağrılarını güvenli hale getirmek için belirteçleri almasını sağlar. ADAL, geliştiriciler için kimlik doğrulamasını daha kolay hale getirir; örneğin:

- Erişim belirteçlerini ve belirteçleri yenileme belirtecini depolayan yapılandırılabilir belirteç önbelleği
- Bir erişim belirtecinin süresi dolarsa ve yenileme belirteci kullanılabilir olduğunda otomatik belirteç Yenile
- Zaman uyumsuz yöntem çağrıları desteği

> [!NOTE]
> Azure AD v 2.0 kitaplıkları (MSAL) aranıyor mu? [Msal kitaplığı kılavuzunu](../develop/reference-v2-libraries.md)kullanıma alın.
>
>

## <a name="microsoft-supported-client-libraries"></a>Microsoft tarafından desteklenen Istemci kitaplıkları

| Platform | Kitaplık | İndir | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET Istemcisi, Windows Mağazası, UWP, Xamarin iOS ve Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Masaüstü uygulaması](../develop/quickstart-v2-windows-desktop.md) |[Başvuru](/dotnet/api/microsoft.identitymodel.clients.activedirectory) |
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Tek sayfalı uygulama](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS uygulaması](../develop/quickstart-v2-ios.md) | [Başvuru](http://cocoadocs.org/docsets/ADAL/2.5.1/)|
| Android |ADAL |[Maven](https://search.maven.org/search?q=g:com.microsoft.aad+AND+a:adal&core=gav) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android uygulaması](../develop/quickstart-v2-android.md) | [JavaDocs](https://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | [Node.js web uygulaması](https://github.com/Azure-Samples/active-directory-node-webapp-openidconnect)|[Başvuru](/javascript/api/overview/azure/activedirectory) |
| Java |ADAL4J |[Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3Aadal4j%20g%3Acom.microsoft.azure) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java web uygulaması](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) |[Başvuru](https://javadoc.io/doc/com.microsoft.azure/adal4j) |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[Python web uygulaması](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi) |[Başvuru](https://adal-python.readthedocs.io/) |

## <a name="microsoft-supported-server-libraries"></a>Microsoft tarafından desteklenen sunucu kitaplıkları

| Platform | Kitaplık | İndir | Kaynak kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| .NET |AzureAD için OWıN|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.ActiveDirectory) |[MVC uygulaması](../develop/quickstart-v2-aspnet-webapp.md) | |
| .NET |Openıdconnect için OWIN |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.OpenIdConnect) |[Web uygulaması](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| .NET |WS-Federation için OWıN |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[GitHub](https://github.com/aspnet/AspNetKatana/tree/dev/src/Microsoft.Owin.Security.WsFederation) |[MVC web uygulaması](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |.NET 4,5 için kimlik Protokolü Uzantıları |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |.NET 4,5 için JWT Işleyicisi |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web API](../develop/authentication-flows-app-scenarios.md)| |

## <a name="scenarios"></a>Senaryolar

Aşağıda, bir uzak kaynağa erişen istemcide ADAL kullanmaya yönelik üç yaygın senaryo verilmiştir:

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Bir cihazda çalışan yerel istemci uygulaması kullanıcılarının kimliğini doğrulama

Bu senaryoda, bir geliştirici, bir Web API 'SI gibi uzak bir kaynağa erişmesi gereken bir mobil istemci veya masaüstü uygulamasına sahiptir. Web API 'SI anonim çağrılara izin vermez ve kimliği doğrulanmış bir kullanıcı bağlamında çağrılmalıdır. Web API 'SI, belirli bir Azure AD kiracısı tarafından verilen erişim belirteçlerine güvenecek şekilde önceden yapılandırılmıştır. Azure AD, bu kaynak için erişim belirteçleri vermek üzere önceden yapılandırılmıştır. Web API 'sini istemciden çağırmak için, geliştirici Azure AD ile kimlik doğrulamasını kolaylaştırmak için ADAL kullanır. ADAL kullanmanın en güvenli yolu Kullanıcı kimlik bilgilerini toplamak için Kullanıcı arabirimini (tarayıcı penceresi olarak işlenir) işlemelidir.

ADAL, kullanıcının kimliğini doğrulamayı, Azure AD 'den bir erişim belirteci almanızı ve belirteci yenilemeyi kolaylaştırır ve sonra erişim belirtecini kullanarak Web API 'sini çağırır.

Azure AD kimlik doğrulamasını kullanarak bu senaryoyu gösteren bir kod örneği için, bkz. [Yerel ISTEMCI WPF uygulaması Web API 'si](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Web sunucusunda çalışan bir gizli istemci uygulamasının kimliğini doğrulama

Bu senaryoda, bir geliştirici, bir Web API 'SI gibi uzak bir kaynağa erişmesi gereken bir sunucuda çalışan bir uygulamaya sahiptir. Web API 'SI anonim çağrılara izin vermiyor, bu nedenle yetkilendirilmiş bir hizmetten çağrılmalıdır. Web API 'SI, belirli bir Azure AD kiracısı tarafından verilen erişim belirteçlerine güvenecek şekilde önceden yapılandırılmıştır. Azure AD, istemci kimlik bilgileri (istemci KIMLIĞI ve gizli) olan bir hizmete bu kaynak için erişim belirteçleri vermek üzere önceden yapılandırılmıştır. ADAL, Web API 'sini çağırmak için kullanılabilecek bir erişim belirteci döndüren Azure AD ile hizmetin kimlik doğrulamasını kolaylaştırır. ADAL Ayrıca, erişim belirtecinin ömrünü önbelleğe alarak ve gerektiği şekilde yenileerek yönetimini de işler. Bu senaryoyu gösteren bir kod örneği için bkz. [Daemon konsol uygulaması Web API 'si](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Bir kullanıcı adına sunucuda çalışan bir gizli istemci uygulamasının kimliğini doğrulama

Bu senaryoda, bir geliştirici, bir Web API 'SI gibi uzak bir kaynağa erişmesi gereken bir sunucuda çalışan bir Web uygulamasına sahiptir. Web API 'SI anonim çağrılara izin vermediğinden, kimliği doğrulanmış bir kullanıcı adına yetkilendirilmiş bir hizmetten çağrılmalıdır. Web API 'SI, belirli bir Azure AD kiracısı tarafından verilen erişim belirteçlerine güvenmek üzere önceden yapılandırılmıştır ve Azure AD, istemci kimlik bilgileri olan bir hizmete bu kaynağa erişim belirteçleri vermek üzere önceden yapılandırılmıştır. Web uygulamasında kullanıcının kimliği doğrulandıktan sonra, uygulama Azure AD 'den Kullanıcı için bir yetkilendirme kodu alabilir. Web uygulaması daha sonra, Azure AD 'den uygulamayla ilişkili yetkilendirme kodunu ve istemci kimlik bilgilerini kullanarak bir kullanıcı adına bir erişim belirteci almak ve belirteci yenilemek için ADAL kullanabilir. Web uygulaması erişim belirtecini elinde olduktan sonra, belirtecin süresi dolana kadar Web API 'sini çağırabilir. Belirtecin süresi dolarsa, Web uygulaması daha önce alınmış yenileme belirtecini kullanarak yeni bir erişim belirteci almak için ADAL kullanabilir. Bu senaryoyu gösteren bir kod örneği için bkz. [Native Client to Web API to Web API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>Ayrıca Bkz.

- [Azure Active Directory Geliştirici Kılavuzu](v1-overview.md)
- [Azure Active Directory için kimlik doğrulama senaryoları](v1-authentication-scenarios.md)
- [Azure Active Directory kodu örnekleri](sample-v1-code.md)
