---
title: Azure 'da geliştiriciler için Başlarken Kılavuzu | Microsoft Docs
description: Bu makalede, geliştirme ihtiyaçları için Microsoft Azure platformunu kullanmaya başlamak isteyen geliştiriciler için önemli bilgiler sağlanmaktadır.
author: ggailey777
ms.service: azure
ms.topic: article
ms.date: 11/18/2019
ms.author: glenga
ms.openlocfilehash: 1caab392af1dab4f2e706d7372b0a1d9b5e62db6
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101091215"
---
# <a name="get-started-guide-for-azure-developers"></a>Azure geliştiricileri için başlangıç kılavuzu

## <a name="what-is-azure"></a>Azure nedir?

Azure, mevcut uygulamalarınızı barındırabilmesi ve yeni uygulama geliştirmeyi kolaylaştırmaya yönelik bir bulut platformudur. Azure, şirket içi uygulamaları da geliştirebilir. Azure, bulut bilgi işlemin verimlilik avantajlarından yararlanarak uygulamalarınızı geliştirmek, test etmek, dağıtmak ve yönetmek için ihtiyacınız olan bulut hizmetlerini tümleştirir.

Uygulamalarınızı Azure 'da barındırarak küçük bir başlangıç yapabilir ve uygulamanızı, müşteri talebi büyüdükçe kolayca ölçeklendirebilirsiniz. Azure Ayrıca, farklı bölgeler arasında yük devretme da dahil olmak üzere yüksek kullanılabilirliğe sahip uygulamalar için gereken güvenilirliği sunar. [Azure Portal](https://portal.azure.com) tüm Azure hizmetlerinizi kolayca yönetmenize olanak sağlar. Ayrıca, hizmet özel API 'Leri ve şablonları kullanarak hizmetlerinizi programlı bir şekilde yönetebilirsiniz.

Bu kılavuz, uygulama geliştiricileri için Azure platformuna giriş niteliğindedir. Azure 'da yeni uygulamalar oluşturmaya veya mevcut uygulamaları Azure 'a geçirmeye başlamak için gereken yönergeler ve yön sağlar.

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

Azure 'un sunduğu tüm hizmetlerle, çözüm mimarinizi desteklemek için hangi hizmetleri gerektiğini öğrenmek için bir başlatma görevi olabilir. Bu bölümde geliştiricilerin yaygın olarak kullandığı Azure hizmetleri vurgulanmaktadır. Tüm Azure hizmetlerinin listesi için bkz. [Azure belgeleri](../../index.yml).

İlk olarak, Azure 'da uygulamanızı barındırmaya karar vermelisiniz. Tüm altyapınızı bir sanal makine (VM) olarak yönetmeniz gerekiyor mu? Azure 'un sağladığı platform yönetimi olanaklarını kullanabilir misiniz? Yalnızca kod yürütmeyi barındırmak için sunucusuz bir çerçeveye ihtiyacınız olabilir mi?

Uygulamanız için Azure 'un çeşitli seçeneklerini sağladığı bulut depolaması gerekir. Azure 'un kurumsal kimlik doğrulamasının avantajlarından yararlanabilirsiniz. Bulut tabanlı geliştirme ve izleme için de araçlar vardır ve birçok barındırma hizmeti DevOps tümleştirmesi sunar.

Şimdi, uygulamalarınız için araştırmanın önerdiğimiz belirli hizmetlerden bazılarını inceleyelim.

### <a name="application-hosting"></a>Uygulama barındırma

Azure, uygulamanızı çalıştırmak için çeşitli bulut tabanlı bilgi işlem teklifleri sağlar, böylece altyapı ayrıntıları konusunda endişelenmenize gerek kalmaz. Uygulamanızın kullanımı arttıkça kaynaklarınızın ölçeğini kolayca ölçeklendirebilir veya ölçekleyebilirsiniz.

Azure, uygulama geliştirme ve barındırma gereksinimlerinizi destekleyen hizmetler sunar. Azure, uygulamanızın barındırılması üzerinde tam denetim sağlamak için hizmet olarak altyapı (IaaS) sağlar. Azure 'un hizmet olarak platform (PaaS) teklifleri, uygulamalarınızı desteklemek için gereken tam olarak yönetilen Hizmetleri sağlar. Azure 'da, tüm yapmanız gereken, kodunuzun yazılması için de doğru sunucusuz barındırma vardır.

![Azure uygulama barındırma seçenekleri](./media/azure-developer-guide/azure-developer-hosting-options.png)

#### <a name="azure-app-service"></a>Azure App Service

En hızlı yolun Web tabanlı projelerinizi yayımlamasını istediğinizde Azure App Service göz önünde bulundurun. App Service, Web uygulamalarınızı mobil istemcilerinizi destekleyecek şekilde genişletmeyi ve kolayca tüketilen REST API 'Leri yayımlamayı kolaylaştırır. Bu platform Sosyal sağlayıcılar, trafik tabanlı otomatik ölçeklendirme, üretimde test etme ve sürekli ve kapsayıcı tabanlı dağıtımlar kullanarak kimlik doğrulaması sağlar.

Web uygulamaları, mobil uygulama arka uçları ve API uygulamaları oluşturabilirsiniz.

Her üç uygulama türü App Service çalışma zamanını paylaştığından, bir Web sitesi barındırabilir, mobil istemcileri destekleyebilir ve API 'lerinizi Azure 'da, her türlü aynı proje veya çözümden bir şekilde kullanıma sunabilirsiniz. App Service hakkında daha fazla bilgi edinmek için bkz. [Azure Web Apps nedir?](../../app-service/overview.md).

App Service, DevOps göz önünde bulundurularak tasarlanmıştır. Yayımlama ve sürekli tümleştirme dağıtımları için çeşitli araçları destekler. Bu araçlar GitHub Web kancaları, Jenkins, Azure DevOps, TeamCity ve diğerleri içerir.

[Çevrimiçi geçiş aracını](https://appmigration.microsoft.com/)kullanarak, mevcut uygulamalarınızı App Service geçirebilirsiniz.

> **Ne zaman kullanılır**: mevcut Web uygulamalarını Azure 'a geçirirken ve Web uygulamalarınız için tam olarak yönetilen bir barındırma platformuna ihtiyacınız olduğunda App Service kullanın. Ayrıca, mobil istemcileri destekletmelisiniz veya REST API 'Leri uygulamanızla birlikte kullanıma sunabileceğiniz App Service de kullanabilirsiniz.
>
> **Kullanmaya başlayın**: App Service ilk [Web uygulamanızı](../../app-service/quickstart-dotnetcore.md), [Mobil uygulamanızı](/previous-versions/azure/app-service-mobile/app-service-mobile-ios-get-started)veya [API uygulamanızı](../../app-service/app-service-web-tutorial-rest-api.md)oluşturmayı ve dağıtmayı kolaylaştırır.
>
> **Şimdi deneyin**: App Service, bir Azure hesabına kaydolmadan platformu denemek için kısa süreli bir uygulama sağlamanıza olanak tanır. Platformu deneyin ve [Azure App Service uygulamanızı oluşturun](https://tryappservice.azure.com/).

#### <a name="azure-virtual-machines"></a>Azure Sanal Makineler

Hizmet olarak altyapı (IaaS) sağlayıcısı olarak Azure, uygulamanızı Windows veya Linux VM 'lerine dağıtmanıza veya geçirmenize olanak sağlar. Azure sanal ağ ile birlikte Azure sanal makineleri, Windows veya Linux VM 'lerinin Azure 'a dağıtımını destekler. VM 'Ler ile makinenin yapılandırması üzerinde toplam denetiminiz vardır. VM 'Leri kullanırken tüm sunucu yazılımı yükleme, yapılandırma, bakım ve işletim sistemi düzeltme ekleri sizin sorumluluğunuzdadır.

VM 'lerle sahip olduğunuz denetim düzeyi nedeniyle, Azure 'da PaaS modeline sığmayan çok çeşitli sunucu iş yüklerini çalıştırabilirsiniz. Bu iş yükleri, veritabanı sunucularını, Windows Server Active Directory ve Microsoft SharePoint 'i içerir. Daha fazla bilgi için bkz. [Linux](../../virtual-machines/linux/index.yml) veya [Windows](../../virtual-machines/windows/index.yml)için sanal makineler belgeleri.

> **Ne zaman kullanılır**: uygulama altyapınız üzerinde tam denetim yapmak veya şirket içi uygulama iş yüklerini değişiklik yapmadan Azure 'a geçirmek Istediğinizde sanal makineleri kullanın.
>
> **Kullanmaya başlayın**: Azure Portal BIR [Linux VM](../../virtual-machines/linux/quick-create-portal.md) veya [Windows VM](../../virtual-machines/windows/quick-create-portal.md) 'si oluşturun.

#### <a name="azure-functions-serverless"></a>Azure Işlevleri (sunucusuz)

Kodunuzu çalıştırmak için tüm uygulamayı veya altyapıyı oluşturma ve yönetme konusunda endişelenmek yerine, kodunuzu yazmak ve olaylara veya bir zamanlamaya yanıt olarak çalıştırmak için ne olur?  [Azure işlevleri](../../azure-functions/functions-overview.md) , yalnızca ihtiyacınız olan kodu yazmanızı sağlayan "sunucusuz" tarzı bir tekliftir. Işlevlerle, HTTP istekleri, Web kancaları, bulut hizmeti etkinlikleri veya bir zamanlamaya göre kod yürütmeyi tetikleyebilirsiniz. C \# , F \# , Node.js, Python veya PHP gibi tercih ettiğiniz geliştirme dilinizde kod oluşturabilirsiniz. Tüketim tabanlı faturalandırma sayesinde yalnızca kodunuzun yürütüldüğü süre için ödeme yaparsınız ve gerektiğinde Azure ölçeklendiriyor olursunuz.

> **Ne zaman kullanılır**: diğer Azure hizmetleri tarafından tetiklenen kodunuz varsa, Web tabanlı olaylara veya bir zamanlamaya göre Azure işlevleri 'ni kullanın. Ayrıca, tüm barındırılan bir projenin ek yüküne gerek duymadığınızda veya yalnızca kodunuzun çalıştığı zaman için ödeme yapmak istediğinizde Işlevleri de kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Işlevlerine genel bakış](../../azure-functions/functions-overview.md).
>
> **Kullanmaya başlayın**: portaldan [ilk işlevinizi oluşturmak](../../azure-functions/functions-get-started.md) için işlevler hızlı başlangıç öğreticisini izleyin.
>
> **Şimdi deneyin**: Azure Işlevleri, Azure hesabına kaydolmak zorunda kalmadan kodunuzu çalıştırmanızı sağlar. Şimdi deneyin ve [Ilk Azure işlevinizi oluşturun](https://tryappservice.azure.com/).

#### <a name="azure-service-fabric"></a>Azure Service Fabric

Azure Service Fabric, dağıtılmış bir sistem platformudur. Bu platform ölçeklenebilir ve güvenilir mikro hizmetler oluşturmayı, dağıtmayı, dağıtmayı ve yönetmeyi kolaylaştırır. Ayrıca, gibi kapsamlı uygulama yönetimi özellikleri de sağlar:

* Sağlama
* Dağıtılmasını
* İzleme
* Yükseltme/düzeltme eki uygulama
* Siliniyor

Paylaşılan bir makine havuzunda çalışan uygulamalar, gerektiğinde yüzlerce veya binlerce makineye kadar küçük ve ölçeklenebilir olabilir.

Service Fabric, .NET için açık Web arabirimi (OWıN) ve ASP.NET Core ile WebAPI 'yi destekler. Hem .NET Core hem de Java 'da Linux üzerinde hizmet oluşturmaya yönelik SDK 'lar sağlar. Service Fabric hakkında daha fazla bilgi edinmek için [Service Fabric belgelerine](../../service-fabric/index.yml)bakın.

> **Ne zaman kullanılır:** Service Fabric, bir uygulama oluştururken veya bir mikro hizmet mimarisini kullanmak için mevcut bir uygulamayı yeniden yazarken iyi bir seçenektir. Üzerinde daha fazla denetime ihtiyacınız olduğunda veya temel alınan altyapıya doğrudan erişim için Service Fabric kullanın.
>
> **Kullanmaya başlayın:** [İlk Azure Service Fabric uygulamanızı oluşturun](../../service-fabric/service-fabric-tutorial-create-dotnet-app.md).

#### <a name="azure-spring-cloud"></a>Azure Spring Cloud

Azure Spring Cloud, uygulamalarınızı bulutta oluşturmanıza, dağıtmanıza, ölçeklendirmenize ve izlemenize olanak tanıyan bir sunucusuz mikro hizmetler platformudur. Modern mikro hizmet desenlerini ön yükleme uygulamalarına getirmek için yay bulutu 'nı kullanın, sağlam Java uygulamalarını hızlıca oluşturmak için ortak kodu ortadan kaldırın.

* Bu kritik bileşenlerin en iyi koşullarda çalıştığından emin olmak için, Spring Cloud Service Discovery ve config Server 'ın yönetilen sürümlerinden yararlanın.
* İş mantığınızı oluşturmaya odaklanarak, güvenlik düzeltme ekleri, uyumluluk standartları ve yüksek kullanılabilirliğe sahip hizmet çalışma zamanından faydalanacağız.
* Azure Kubernetes hizmeti 'nin en üstünde uygulama yaşam döngüsünü (ör. dağıtma, başlatma, durdurma, ölçeklendirme) yönetin.
* Uygulamalarınız için Azure veritabanı ve Redsıs için Azure önbelleği gibi uygulamalarınız ve Azure hizmetleri arasında bağlantıları kolayca bağlayın.
* Uygulama bağımlılıkları ve operasyonel telemetri hakkında derin Öngörüler sunan kurumsal sınıf Birleşik izleme araçlarını kullanarak mikro hizmetleri ve uygulamaları izleyin ve sorun giderin.

> **Ne zaman kullanılır:** Tam olarak yönetilen bir hizmet olarak Azure yay bulutu, Azure 'da yay önyükleme/yay bulut tabanlı mikro Hizmetleri çalıştıran işlem maliyetini en aza indirmenizde iyi bir seçenektir. 
>
> **Kullanmaya başlayın:** [Ilk Azure Spring Cloud uygulamanızı dağıtın](../../spring-cloud/spring-cloud-quickstart.md).


### <a name="enhance-your-applications-with-azure-services"></a>Azure hizmetleriyle uygulamalarınızı geliştirin

Azure, uygulama barındırmakla birlikte, işlevselliği geliştirebilecek hizmet teklifleri sağlar. Azure, hem bulutta hem de şirket içinde uygulamalarınızın geliştirme ve bakımını de iyileştirebilir.

#### <a name="hosted-storage-and-data-access"></a>Barındırılan depolama ve veri erişimi

Çoğu uygulama verileri depolamalıdır, ancak uygulamanızı Azure 'da barındırmaya karar verirseniz, aşağıdaki depolama ve veri hizmetlerinden birini veya birkaçını göz önünde bulundurun.

* **Azure Cosmos DB**: Global olarak dağıtılmış, çok modelli bir veritabanı hizmeti. Bu veritabanı, kapsamlı bir SLA ile herhangi bir sayıda coğrafi bölgede işleme ve depolama alanı esnek ölçeklendirmenize olanak sağlar.

  > **Ne zaman kullanılır:** Uygulamanız, MongoDB veritabanları da dahil olmak üzere belge, tablo veya grafik veritabanlarına ihtiyaç duyduğunda, çok iyi tanımlanmış tutarlılık modelleriyle.
  >
  > **Kullanmaya** başlayın: [Azure Cosmos DB bir Web uygulaması oluşturun](../../cosmos-db/create-sql-api-dotnet.md). MongoDB geliştiriciyseniz, bkz. [Azure Cosmos DB MongoDB Web uygulaması oluşturma](../../cosmos-db/create-mongodb-dotnet.md).

* **Azure depolama**: Bloblar, kuyruklar, dosyalar ve diğer ilişkisel olmayan veri türleri için dayanıklı, yüksek oranda kullanılabilir depolama sağlar. Depolama, sanal makineler için depolama temeli sağlar.

  > **Ne zaman kullanılır**: uygulamanız, anahtar-değer çiftleri (tablolar), Bloblar, dosya paylaşımları veya iletiler (kuyruklar) gibi ilişkisel olmayan verileri depoladığında.
  >
  > **Kullanmaya başlayın**: şu türlerden birini depolama alanından seçin: [Bloblar](../../storage/blobs/storage-quickstart-blobs-dotnet.md), [Tablolar](../../cosmos-db/tutorial-develop-table-dotnet.md), [Kuyruklar](../../storage/queues/storage-dotnet-how-to-use-queues.md)veya [dosyalar](../../storage/files/storage-dotnet-how-to-use-files.md).

* **Azure SQL veritabanı**: bulutta ilişkisel tablo verilerini depolamak için Microsoft SQL Server altyapısının Azure tabanlı bir sürümüdür. SQL veritabanı, kesinti süresi olmadan öngörülebilir performans, ölçeklenebilirlik, iş sürekliliği ve veri koruması sağlar.

  > **Ne zaman kullanılır**: uygulamanız, bilgi tutarlılığı, işlem DESTEĞI ve TSQL sorguları desteği ile veri depolama alanı gerektirdiğinde.
  >
  > **Kullanmaya başlayın**: [Azure Portal kullanarak Azure SQL veritabanı 'nda dakikalar içinde veritabanı oluşturma](../../azure-sql/database/single-database-create-quickstart.md).


Mevcut şirket içi verileri Azure 'a taşımak için [Azure Data Factory](../../data-factory/introduction.md) kullanabilirsiniz. Verileri buluta taşımaya hazırsanız, Azure App Service [Karma Bağlantılar](../../app-service/app-service-hybrid-connections.md) App Service barındırılan uygulamanızı şirket içi kaynaklara bağlamanıza olanak tanır. Ayrıca, şirket içi uygulamalarınızdan Azure veri ve depolama hizmetleri 'ne de bağlanabilirsiniz.

#### <a name="docker-support"></a>Docker desteği

Bir işletim sistemi sanallaştırma biçimindeki Docker kapsayıcıları, uygulamaları daha verimli ve öngörülebilir bir şekilde dağıtmanıza olanak tanır. Kapsayıcılı bir uygulama, geliştirme ve test sistemlerinizle aynı şekilde üretimde çalışmaktadır. Standart Docker araçlarını kullanarak kapsayıcıları yönetebilirsiniz. Azure 'da kapsayıcı tabanlı uygulamalar dağıtmak ve yönetmek için mevcut becerilerinizi ve popüler açık kaynaklı araçları kullanabilirsiniz.

Azure, uygulamalarınızda kapsayıcıları kullanmanın birkaç yolunu sağlar.


* **Azure Kubernetes hizmeti**: Kapsayıcılı uygulamalar çalıştırmak için önceden yapılandırılmış bir sanal makine kümesi oluşturmanıza, yapılandırmanıza ve yönetmenize olanak sağlar. Azure Kubernetes hizmeti hakkında daha fazla bilgi edinmek için bkz. [Azure Kubernetes hizmet tanıtımı](../../aks/intro-kubernetes.md).

  > **Ne zaman kullanılır**: ek zamanlama ve yönetim araçları sağlayan ve bir Docker sısınma kümesi dağıttığınızda üretime hazırlamış, ölçeklenebilir ortamlar oluşturmanız gerektiğinde.
  >
  > **Kullanmaya** başlayın: [Kubernetes hizmet kümesi dağıtın](../../aks/tutorial-kubernetes-deploy-cluster.md).

* **Docker makinesi**: Docker-Machine komutlarını kullanarak sanal konaklara bir Docker altyapısı yüklemenize ve yönetmenize olanak sağlar.

  >**Ne zaman kullanılır**: tek bir Docker Konağı oluşturarak uygulamayı hızlıca prototipi oluşturmanız gerektiğinde.

* **App Service Için özel Docker görüntüsü**: Linux üzerinde bir Web uygulaması dağıtırken bir kapsayıcı kayıt defterinden veya bir müşteri kapsayıcısından Docker Kapsayıcıları kullanmanıza olanak sağlar.

  > **Ne zaman kullanılır**: Linux üzerinde bir Web uygulamasını Docker görüntüsüne dağıttığınızda.
  >
  > **Kullanmaya başlayın**: [Linux üzerinde App Service Için özel bir Docker görüntüsü kullanın](../../app-service/quickstart-custom-container.md?pivots=platform-linux%253fpivots%253dplatform-linux).

### <a name="authentication"></a>Kimlik Doğrulaması

Yalnızca uygulamalarınızı kimin kullandığını ve ayrıca Kaynaklarınıza yetkisiz erişimi engellemek için çok önemlidir. Azure, uygulama istemcilerinizin kimliğini doğrulamak için çeşitli yollar sağlar.

* **Azure Active Directory (Azure AD)**: Microsoft Multitenant, bulut tabanlı kimlik ve erişim yönetimi hizmeti. Azure AD ile tümleştirerek uygulamalarınıza çoklu oturum açma (SSO) ekleyebilirsiniz. Azure AD Graph API doğrudan veya Microsoft Graph API 'sini kullanarak dizin özelliklerine erişebilirsiniz. OAuth 2.0 yetkilendirme çerçevesi için Azure AD desteğiyle ve yerel HTTP/REST uç noktaları ve çok platformlu Azure AD kimlik doğrulama kitaplıklarını kullanarak Open ID Connect ile tümleştirilebilir.

  > **Ne zaman kullanılır**: bir SSO deneyimi sağlamak Istediğinizde, Graph tabanlı verilerle çalışın veya etki alanı tabanlı kullanıcıların kimliğini doğrulayın.
  >
  > **Kullanmaya** başlayın: daha fazla bilgi edinmek için [Geliştirici Kılavuzu Azure Active Directory](../../active-directory/develop/v2-overview.md)bakın.

* **App Service kimlik doğrulaması**: uygulamanızı barındırmak için App Service ' i seçtiğinizde Facebook, Google, Microsoft ve Twitter gibi sosyal kimlik sağlayıcılarının yanı sıra Azure AD için de yerleşik kimlik doğrulama desteği alırsınız.

  > **Ne zaman kullanılır**: Azure AD, sosyal kimlik sağlayıcıları veya her ikisini de kullanarak App Service bir uygulamada kimlik doğrulamasını etkinleştirmek istediğinizde.
  >
  > **Kullanmaya** başlayın: App Service kimlik doğrulaması hakkında daha fazla bilgi için, bkz. [Azure App Service kimlik doğrulama ve yetkilendirme](../../app-service/overview-authentication-authorization.md).

Azure 'da en iyi güvenlik uygulamaları hakkında daha fazla bilgi edinmek için bkz. [Azure Güvenlik en iyi uygulamaları ve desenleri](../../security/fundamentals/best-practices-and-patterns.md).

### <a name="monitoring"></a>İzleme

Uygulamanızı Azure 'da çalışır duruma getirmeniz, performansı izlemeniz, sorun izlemek ve müşterilerin uygulamanızı nasıl kullandığını görmeniz gerekir. Azure çeşitli izleme seçenekleri sağlar.

* **Application Insights**: Canlı Web uygulamalarınızı Izlemek üzere Visual Studio ile tümleştirilen, Azure 'da barındırılan Genişletilebilir analiz hizmeti. Uygulamalarınızın performansını ve kullanılabilirliğini sürekli olarak geliştirmek için ihtiyacınız olan verileri sağlar. Bu geliştirme, uygulamalarınızı Azure 'da barındırmanıza veya Not etmeksizin meydana gelir.

  > **Kullanmaya başlayın**: [Application Insights öğreticisini](../../azure-monitor/app/app-insights-overview.md)izleyin.

* **Azure izleyici**: Azure altyapınız ve kaynaklarla oluşturduğunuz ölçümleri ve günlükleri görselleştirmenize, sorgulamanızı, yönlendirmenize, arşivlemenize ve bunlarla işlem yapmanıza yardımcı olan bir hizmettir. İzleyici, Azure kaynaklarını izlemeye yönelik tek bir kaynaktır ve Azure portal gördüğünüz veri görünümlerini sağlar.

  > **Kullanmaya** başlayın: [Azure izleyici 'yi](../../azure-monitor/overview.md)kullanmaya başlayın.

### <a name="devops-integration"></a>DevOps tümleştirmesi

Azure 'un, sürekli tümleştirme ile VM 'Ler sağlayıp sağlamasının veya Web uygulamalarınızı yayımlamasından bağımsız olarak, Azure popüler DevOps araçlarının çoğuyla tümleştirilir. Zaten sahip olduğunuz araçlarla çalışabilir ve şu gibi araçlar için mevcut deneyiminizi en üst düzeye çıkarabilirsiniz:

* Jenkins
* GitHub
* Puppet
* Chef
* TeamCity
* Ansible
* Azure DevOps

> **Kullanmaya başlayın**: bir App Service uygulamasının DevOps seçeneklerini görmek için bkz. [Azure App Service için sürekli dağıtım](../../app-service/deploy-continuous-deployment.md).
>
> **Şimdi deneyin:** [çeşitli DevOps tümleştirmelerini deneyin](https://azure.microsoft.com/try/devops/).

## <a name="azure-regions"></a>Azure bölgeleri

Azure, dünyanın dört bir yanındaki birçok bölgede genel olarak kullanılabilen küresel bir bulut platformudur. Azure 'da bir hizmet, uygulama veya VM sağladığınızda bir bölge seçmeniz istenir. Bu bölge, uygulamanızın çalıştırıldığı veya verilerinizin depolandığı belirli bir veri merkezini temsil eder. Bu bölgeler, [Azure bölgeleri](https://azure.microsoft.com/regions/) sayfasında yayınlanan belirli konumlara karşılık gelir.

### <a name="choose-the-best-region-for-your-application-and-data"></a>Uygulamanız ve verileriniz için en iyi bölgeyi seçin

Azure kullanmanın avantajlarından biri, uygulamalarınızı dünyanın dört bir yanındaki çeşitli veri merkezlerinde dağıtmanıza olanak sağlar. Seçtiğiniz bölge, uygulamanızın performansını etkileyebilir. Örneğin, ağ isteklerindeki gecikmeyi azaltmak için müşterilerinizin çoğuna yakın bir bölge seçmek daha iyidir. Ayrıca, belirli ülkelerde/bölgelerde uygulamanızı dağıtmaya yönelik yasal gereksinimleri karşılamak için bölgenizi seçmeniz de gerekebilir. Uygulama verilerini, uygulamanızı barındıran veri merkezinde mümkün olduğunca yakın bir şekilde depolamak için her zaman en iyi uygulamadır.

### <a name="multi-region-apps"></a>Çok bölgeli uygulamalar

Büyük olasılıkla, doğal bir olağanüstü durum veya Internet arızası gibi bir olay nedeniyle tüm veri merkezinin çevrimdışı olması olanaksız olur. En yüksek kullanılabilirlik sağlamak için önemli iş uygulamalarını birden fazla veri merkezinde barındırmak en iyi uygulamadır. Aynı zamanda birden çok bölge kullanılması, genel kullanıcılar için gecikme süresini azaltabilir ve uygulamalar güncelleştirilirken esneklik için ek fırsatlar sağlayabilir.

Sanal makine ve uygulama hizmetleri gibi bazı hizmetler, yüksek kullanılabilirliğe sahip kurumsal uygulamaları desteklemek için bölgeler arasında yük devretmeyle çoklu bölge desteğini etkinleştirmek üzere [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md) kullanır. Bir örnek için bkz. [Azure başvuru mimarisi: bir Web uygulamasını birden çok bölgede çalıştırma](/azure/architecture/reference-architectures/app-service-web-app/multi-region).

>**Ne zaman kullanılır**: yük devretme ve çoğaltmadan faydalanabilir kurumsal ve yüksek kullanılabilirliğe sahip uygulamalarınız olduğunda.

## <a name="how-do-i-manage-my-applications-and-projects"></a>Uygulamalarımı ve projelerimi nasıl yönetebilirim?

Azure, Azure kaynaklarınızı, uygulamalarınızı ve projelerinizi hem programlı olarak hem de [Azure Portal](https://portal.azure.com/)oluşturmanız ve yönetmeniz için zengin bir deneyim kümesi sağlar.

### <a name="command-line-interfaces-and-powershell"></a>Komut satırı arabirimleri ve PowerShell

Azure, uygulamalarınızı ve hizmetlerinizi komut satırından yönetmenin iki yolunu sağlar. Bash, Terminal, komut istemi veya tercih ettiğiniz komut satırı aracınız gibi araçları kullanabilirsiniz. Genellikle, sanal makineleri, sanal ağları, Web uygulamalarını ve diğer hizmetleri oluşturma ve yapılandırma gibi Azure portal, komut satırından aynı görevleri gerçekleştirebilirsiniz.

* [Azure Command-Line arabirimi (CLI)](/cli/azure/install-azure-cli): Azure aboneliğine bağlanmanızı ve komut satırından Azure kaynaklarına yönelik çeşitli görevleri programlamanızı sağlar.

* [Azure PowerShell](/powershell/azure/): Windows PowerShell kullanarak Azure kaynaklarını yönetmenizi sağlayan cmdlet 'ler içeren bir modül kümesi sağlar.

### <a name="azure-portal"></a>Azure portalı

[Azure Portal](https://portal.azure.com) Web tabanlı bir uygulamadır. Azure kaynakları ve hizmetleri oluşturmak, yönetmek ve kaldırmak için Azure portal kullanabilirsiniz. Şunları içerir:

* Yapılandırılabilir bir pano
* Azure Kaynak yönetimi araçları
* Abonelik ayarlarına ve faturalandırma bilgilerine erişin. Daha fazla bilgi için [Azure Portal genel bakış](https://azure.microsoft.com/features/azure-portal/)bölümüne bakın.

### <a name="rest-apis"></a>REST API'leri

Azure, Azure portal Kullanıcı arabirimini destekleyen bir REST API kümesi üzerine kurulmuştur. Bu REST API 'lerinin çoğu, Azure kaynaklarınızı ve uygulamalarınızı herhangi bir Internet etkin cihazdan programlı bir şekilde sağlamanıza ve yönetmenize olanak sağlamak için de desteklenir. REST API belgelerinin tamamı için bkz. [Azure Rest SDK başvurusu](/rest/api/).

### <a name="apis"></a>API'ler

REST API 'lerle birlikte, birçok Azure hizmeti, aşağıdaki geliştirme platformları için SDK 'lar dahil olmak üzere platforma özgü Azure SDK 'larını kullanarak uygulamalarınızdan kaynakları programlı bir şekilde yönetmenizi sağlar:

* [.NET](/dotnet/api/)
* [Node.js](/azure/developer/javascript/)
* [Java](/java/azure)
* [PHP](https://github.com/Azure/azure-sdk-for-php/blob/master/README.md)
* [Python](/azure/python/)
* [Ruby](https://github.com/Azure/azure-sdk-for-ruby/blob/master/README.md)
* [Git](/azure/go)

[Mobile Apps](/previous-versions/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library) ve [Azure Media Services](../../media-services/previous/media-services-dotnet-how-to-use.md) gibi hizmetler, Web ve mobil istemci uygulamalarından hizmetlere erişmenizi sağlayan istemci tarafı SDK 'ları sağlar.

### <a name="azure-resource-manager"></a>Azure Resource Manager

Uygulamanızı Azure 'da çalıştırmak, büyük olasılıkla birden çok Azure hizmeti ile çalışmayı içerir. Bu hizmetler aynı yaşam döngüsünü izler ve mantıksal birim olarak düşünülebilir. Örneğin, bir Web uygulaması Web Apps, SQL veritabanı, depolama, Redsıs için Azure önbelleği ve Azure Content Delivery Network hizmetleri kullanabilir. [Azure Resource Manager](../../azure-resource-manager/management/overview.md) , uygulamanızdaki kaynaklarla bir grup olarak çalışmanıza olanak sağlar. Tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilirsiniz.

Azure Resource Manager, ilgili kaynakları mantıksal olarak gruplandırmanıza ve yönetmeye yönelik olarak, ilgili kaynakların dağıtımını ve yapılandırmasını özelleştirmenize olanak sağlayan dağıtım özelliklerini içerir. Örneğin, bir uygulamayı dağıtmak ve yapılandırmak Kaynak Yöneticisi kullanabilirsiniz. Bu uygulama birden çok sanal makineden, yük dengeleyiciden ve Azure SQL veritabanı 'nda tek bir birim olarak bir veritabanından oluşabilir.

Bu dağıtımları, JSON biçimli bir belge olan Azure Resource Manager şablonu kullanarak geliştirirsiniz. Şablonlar, bir dağıtım tanımlamanıza ve komut dosyaları yerine bildirim temelli şablonlar kullanarak uygulamalarınızı yönetmenize olanak sağlar. Şablonlarınız, test, hazırlama ve üretim gibi farklı ortamlarda çalışabilir. Örneğin, bir GitHub deposuna, bir Azure hizmetleri kümesine tek bir tıklama ile kod dağıtan bir düğme eklemek için şablonları kullanabilirsiniz.

> **Ne zaman kullanılır**: uygulamanız IÇIN, REST API 'Leri, Azure clı ve Azure PowerShell kullanarak programlı bir şekilde yönetebileceğiniz şablon tabanlı bir dağıtım istediğinizde Kaynak Yöneticisi şablonları kullanın.
>
> **Kullanmaya** başlayın: şablonları kullanmaya başlamak için bkz. [Azure Resource Manager şablonları yazma](../../azure-resource-manager/templates/template-syntax.md).

## <a name="understanding-accounts-subscriptions-and-billing"></a>Hesapları, abonelikleri ve faturalamayı anlama

Geliştiriciler olarak, koda doğru bir şekilde başlamak ve uygulamalarımızın çalışmasını sağlamak için mümkün olduğunca hızlı bir şekilde çalışmaya başlamak istiyoruz. Azure 'da mümkün olduğunca kolay çalışmaya başlamasını kesinlikle öneririz. Azure, çok daha kolay hale getirmenize yardımcı olmak için [ücretsiz bir deneme](https://azure.microsoft.com/free/)sunar. Ayrıca, bazı hizmetlerde bir "ücretsiz olarak dene" işlevselliğine sahip olan, hatta bir hesap oluşturmanızı gerektirmeyen [Azure App Service](https://tryappservice.azure.com/)gibi işlevler de vardır. Kodlanması ve uygulamanızı Azure 'a dağıtmaya kadar eğlenceye kadar, Azure 'un nasıl çalıştığını anlamak da önemlidir. Özellikle, bir kullanıcı hesabı, abonelik ve Faturalandırma açısından nasıl çalıştığını anlamanız gerekir.

### <a name="what-is-an-azure-account"></a>Azure hesabı nedir?

Bir Azure aboneliği oluşturmak veya bunlarla çalışmak için bir Azure hesabınızın olması gerekir. Azure hesabı, Azure AD 'nin güvendiği bir veya bir iş ya da okul organizasyonu gibi bir dizinde bulunan bir kimliktir. Böyle bir kuruluşa ait değilseniz, Azure AD tarafından güvenilen Microsoft hesabınızı kullanarak her zaman bir abonelik oluşturabilirsiniz. Şirket içi Windows Server Active Directory Azure AD ile tümleştirme hakkında daha fazla bilgi için bkz. Şirket [içi kimliklerinizi Azure Active Directory Ile tümleştirme](../../active-directory/hybrid/whatis-hybrid-identity.md).

Her Azure aboneliği bir Azure AD örneğiyle güven ilişkisine sahiptir. Bu; Azure aboneliğinin kullanıcılar, hizmetler ve cihazlar için kimlik doğrulaması yapmak üzere bu dizine güvendiği anlamına gelir. Birden çok abonelik aynı dizine güvenebilir ancak bir abonelik yalnızca bir dizine güvenir. Daha fazla bilgi edinmek için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkilendirilmesi](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

Ayrıca, *Kullanıcılar* olarak da bilinen tek Azure hesap kimliklerini tanımlamak IÇIN Azure AD 'de *grupları* tanımlayabilirsiniz. Kullanıcı gruplarının oluşturulması, rol tabanlı erişim denetimi (RBAC) kullanarak bir abonelikteki kaynaklara erişimi yönetmenin iyi bir yoludur. Grup oluşturma hakkında bilgi edinmek için bkz. [Azure Active Directory Preview 'da Grup oluşturma](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Ayrıca, [PowerShell kullanarak](../../active-directory/enterprise-users/groups-settings-v2-cmdlets.md)gruplar oluşturabilir ve yönetebilirsiniz.

### <a name="manage-your-subscriptions"></a>Aboneliklerinizi yönetme

Abonelik, Azure hesabına bağlı Azure hizmetlerinin mantıksal gruplandırmasıdır. Tek bir Azure hesabı, birden çok abonelik içerebilir. Azure hizmetleri için faturalandırma, abonelik başına esasına göre yapılır. Türe göre kullanılabilir abonelik tekliflerinin bir listesi için, [Microsoft Azure teklif ayrıntıları](https://azure.microsoft.com/support/legal/offer-details/)' na bakın. Azure aboneliklerinde, abonelik üzerinde tam denetim sahibi olan bir hesap yöneticisi vardır. Ayrıca, abonelikteki tüm hizmetler üzerinde denetim sahibi olan bir hizmet yöneticisi de vardır. Klasik abonelik yöneticileri hakkında daha fazla bilgi için bkz. [Azure abonelik yöneticileri ekleme veya değiştirme](../../cost-management-billing/manage/add-change-subscription-administrator.md). Azure [rol tabanlı erişim denetimi (Azure RBAC)](../../role-based-access-control/overview.md)kullanılarak tek tek hesaplara Azure kaynakları için ayrıntılı denetim verilebilir.

#### <a name="resource-groups"></a>Kaynak grupları

Yeni Azure hizmetleri sağladığınızda, bunu belirli bir abonelikte yapabilirsiniz. Kaynaklar olarak da bilinen ayrı Azure Hizmetleri, bir kaynak grubu bağlamında oluşturulur. Kaynak grupları uygulamanızın kaynaklarını dağıtmayı ve yönetmeyi kolaylaştırır. Kaynak grubu, bir birim olarak birlikte çalışmak istediğiniz uygulamanızın tüm kaynaklarını içermelidir. Kaynakları kaynak grupları arasında ve hatta farklı aboneliklere taşıyabilirsiniz. Kaynakları taşıma hakkında bilgi edinmek için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../../azure-resource-manager/management/move-resource-group-and-subscription.md).

Azure Kaynak Gezgini, aboneliğinizde önceden oluşturduğunuz kaynakları görselleştirmeye yönelik harika bir araçtır. Daha fazla bilgi için bkz. [kaynakları görüntülemek ve değiştirmek için Azure Kaynak Gezgini kullanma](/rest/api/).

#### <a name="grant-access-to-resources"></a>Kaynaklara erişim izni verme

Azure kaynaklarına erişime izin verildiğinde, kullanıcılara belirli bir görevi yapmak için gereken en az ayrıcalığı sağlamak her zaman en iyi uygulamadır.

* **Azure rol tabanlı erişim denetimi (Azure RBAC)**: Azure 'da, belirli bir kapsamdaki Kullanıcı hesaplarına (sorumlular) erişim izni verebilirsiniz: abonelik, kaynak grubu veya ayrı kaynaklar. Azure RBAC, kaynakları bir kaynak grubuna dağıtmanızı ve belirli bir kullanıcıya veya gruba izin vermenizi sağlar. Ayrıca, erişimi yalnızca hedef kaynak grubuna ait kaynaklarla sınırlandırmanıza da olanak tanır. Ayrıca, sanal makine veya sanal ağ gibi tek bir kaynağa erişim izni verebilirsiniz. Erişim vermek için Kullanıcı, Grup veya hizmet sorumlusuna bir rol atarsınız. Önceden tanımlanmış birçok rol vardır ve kendi özel rollerinizi de tanımlayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure rol tabanlı erişim denetimi (Azure RBAC) nedir?](../../role-based-access-control/overview.md).

  > **Ne zaman kullanılır**: kullanıcılar ve gruplar için ayrıntılı erişim yönetimine ihtiyacınız olduğunda veya bir kullanıcıya abonelik sahibi yapmanız gerektiğinde.
  >
  > **Kullanmaya** başlayın: daha fazla bilgi için bkz. [Azure Portal kullanarak Azure rolleri atama](../../role-based-access-control/role-assignments-portal.md).

- **Hizmet sorumlusu nesneleri**: Kullanıcı sorumluları ve gruplarına erişim sağlamaya birlikte, hizmet sorumlusu için de aynı erişimi verebilirsiniz.

  > **Ne zaman kullanılır**: Azure kaynaklarını program aracılığıyla yönetirken veya uygulamalar için erişim verirken. Daha fazla bilgi için bkz. [Active Directory uygulama ve hizmet sorumlusu oluşturma](../../active-directory/develop/howto-create-service-principal-portal.md).

#### <a name="tags"></a>Etiketler

Azure Resource Manager, tek tek kaynaklara özel Etiketler atamanıza olanak tanır. Anahtar-değer çiftleri olan Etiketler faturalandırma veya izleme için kaynakları düzenlemeniz gerektiğinde yararlı olabilir. Etiketler, kaynakları birden çok kaynak grubunda izlemek için bir yol sağlar. Aşağıdaki yollarla etiketler atayabilirsiniz:

* Portalda
* Azure Resource Manager şablonunda
* REST API’yi kullanma
* Azure CLI'yı kullanma
* PowerShell’i kullanma

Her kaynağa birden çok etiket atayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../../azure-resource-manager/management/tag-resources.md).

### <a name="billing"></a>Faturalandırma

Şirket içi bilgi işlemden bulutta barındırılan hizmetlere geçiş yaparken, hizmet kullanımını ve ilgili maliyetleri izlemek ve tahmin etmek önemli kaygılardır. Aylık olarak hangi yeni kaynakların çalışacağını tahmin etmek önemlidir. Ayrıca, geçerli harcama temelinde faturalandırmanın belirli bir aya nasıl göründüğünü de proje yapabilirsiniz.

#### <a name="get-resource-usage-data"></a>Kaynak kullanım verilerini al

Azure, Azure aboneliklerine yönelik kaynak tüketimine ve meta veri bilgilerine erişim veren bir Faturalandırma REST API 'si sağlar. Bu Faturalama API'leri, Azure maliyetlerini daha iyi tahmin etme ve yönetme olanağı sunar. Harcamayı saatlik artışlarla izleyip analiz edebilir ve harcama uyarıları oluşturabilirsiniz. Ayrıca, geçerli kullanım eğilimlerini temelinde gelecekteki faturalandırmayı da tahmin edebilirsiniz.

>**Kullanmaya** başlayın: faturalama API'leri hakkında daha fazla bilgi edinmek için bkz. [Azure tüketim API 'sine genel bakış](../../cost-management-billing/manage/consumption-api-overview.md)

#### <a name="predict-future-costs"></a>Gelecekteki maliyetleri tahmin etme

Maliyetleri önceden tahmin etmek zor olsa da, Azure 'un yardımcı olabilecek araçları vardır. Dağıtılan kaynakların maliyetini tahmin etmeye yardımcı olmak için [Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) vardır. Ayrıca, geçerli tüketimine göre gelecekteki maliyetleri tahmin etmek için portalda faturalandırma kaynaklarını ve Faturalandırma REST API 'Lerini de kullanabilirsiniz.

>**Kullanmaya başlayın**: bkz. [Azure tüketim API 'sine genel bakış](../../cost-management-billing/manage/consumption-api-overview.md).