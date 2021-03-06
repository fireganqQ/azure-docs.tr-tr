---
title: Service Fabric ve VS ile Windows kapsayıcı hatalarını ayıklama
description: Visual Studio 2019 kullanarak Azure Service Fabric Windows kapsayıcılarındaki hataları nasıl ayıklayacağınızı öğrenin.
ms.topic: article
ms.date: 02/14/2019
ms.author: mikhegn
ms.openlocfilehash: 3e6e7785278b182cebb21115a70f35ade52303c3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86247260"
---
# <a name="how-to-debug-windows-containers-in-azure-service-fabric-using-visual-studio-2019"></a>Nasıl yapılır: Visual Studio 2019 kullanarak Azure Service Fabric Windows kapsayıcılarında hata ayıklama

Visual Studio 2019 ile kapsayıcılardaki .NET uygulamalarında Service Fabric Hizmetleri olarak hata ayıklaması yapabilirsiniz. Bu makalede, ortamınızı nasıl yapılandıracağınız ve ardından yerel bir Service Fabric kümesinde çalışan bir kapsayıcıda bir .NET uygulamasında hata ayıklamanın nasıl yapılacağı gösterilir.

## <a name="prerequisites"></a>Önkoşullar

* Windows 10 ' da Windows 10 ' u Windows [kapsayıcıları çalıştıracak şekilde yapılandırmak](/virtualization/windowscontainers/quick-start/quick-start-windows-10) için bu hızlı başlangıcı izleyin
* Windows Server 2016 ' de, Windows 2016 ' yi [Windows kapsayıcıları çalıştıracak şekilde yapılandırmak](/virtualization/windowscontainers/quick-start/quick-start-windows-server) için bu hızlı başlangıcı izleyin
* [Windows üzerinde geliştirme ortamınızı hazırlama '](./service-fabric-get-started.md) yı izleyerek Yerel Service Fabric ortamınızı ayarlama

## <a name="configure-your-developer-environment-to-debug-containers"></a>Geliştirici ortamınızı kapsayıcılara hata ayıklama için yapılandırma

1. Sonraki adımla devam etmeden önce Window hizmetinin Docker 'ın çalıştığından emin olun.

1. Kapsayıcılar arasındaki DNS çözümlemesini desteklemek için, makine adını kullanarak yerel geliştirme kümenizi ayarlamanız gerekir. Bu adımlar, Hizmetleri ters proxy üzerinden ele almak istiyorsanız da gereklidir.
   1. PowerShell 'i yönetici olarak aç
   2. Genellikle SDK küme kurulum klasörüne gidin `C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup` .
   3. Betiği Çalıştır `DevClusterSetup.ps1`

      ``` PowerShell
        C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1
      ```

      > [!NOTE]
      > `-CreateOneNodeCluster`Tek düğümlü bir küme kurmak için öğesini kullanabilirsiniz. Varsayılan olarak yerel bir beş düğümlü küme oluşturulur.
      >

      Service Fabric 'deki DNS hizmeti hakkında daha fazla bilgi edinmek için bkz. [Azure Service Fabric 'de DNS hizmeti](./service-fabric-dnsservice.md). Bir kapsayıcıda çalışan hizmetlerden Service Fabric ters proxy kullanma hakkında daha fazla bilgi edinmek için bkz. [kapsayıcılar üzerinde çalışan hizmetler Için ters proxy özel işleme](service-fabric-reverseproxy.md#special-handling-for-services-running-in-containers).

### <a name="known-limitations-when-debugging-containers-in-service-fabric"></a>Service Fabric kapsayıcılarında hata ayıklama sırasında bilinen sınırlamalar

Service Fabric ve olası çözünürlüklere yönelik hata ayıklama kapsayıcılarıyla ilgili bilinen kısıtlamaların listesi aşağıda verilmiştir:

* ClusterFQDNorIP için localhost kullanılması, kapsayıcılardaki DNS çözümlemesini desteklemez.
    * Çözüm: makine adını kullanarak yerel kümeyi ayarlayın (yukarıya bakın)
* Windows10 çalıştıran bir sanal makinede DNS yanıtı kapsayıcıya geri alınamaz.
    * Çözüm: sanal makineler NIC 'inde IPv4 için UDP sağlama boşaltmasını devre dışı bırak
    * Çalışan Windows10, makinede ağ performansını düşürür.
    * https://github.com/Azure/service-fabric-issues/issues/1061
* Uygulama Docker Compose kullanılarak dağıtılmışsa, DNS hizmeti adı kullanılarak aynı uygulamadaki hizmetlerin çözümlenmesi Windows10 üzerinde çalışmaz.
    * Çözüm: hizmet uç noktalarını çözümlemek için ServiceName. ApplicationName kullanın
    * https://github.com/Azure/service-fabric-issues/issues/1062
* ClusterFQDNorIP için IP adresi kullanılıyorsa, konaktaki birincil IP 'yi değiştirmek DNS işlevini keser.
    * Çözüm: konaktaki yeni birincil IP 'yi kullanarak kümeyi yeniden oluşturun veya makine adını kullanın. Bu ayırıcıya tasarım.
* Kümenin oluşturulduğu FQDN ağda çözümlenememişse, DNS başarısız olur.
    * Çözüm: yerel kümeyi konağın birincil IP 'sini kullanarak yeniden oluşturun. Bu hata tasarıma göre yapılır.
* Bir kapsayıcıda hata ayıklarken Docker günlükleri yalnızca Visual Studio çıktı penceresinde kullanılabilir, Service Fabric Explorer dahil Service Fabric API 'Leri üzerinden kullanılamaz

## <a name="debug-a-net-application-running-in-docker-containers-on-service-fabric"></a>Service Fabric Docker kapsayıcılarında çalışan bir .NET uygulamasında hata ayıklama

1. Visual Studio'yu yönetici olarak çalıştırın.

1. Mevcut bir .NET uygulamasını açın veya yeni bir tane oluşturun.

1. Projeye sağ tıklayın ve **Ekle-> kapsayıcı Orchestrator desteği->** ' ı seçin Service Fabric

1. Uygulamada hata ayıklamayı başlatmak için **F5** 'e basın.

    Visual Studio, .NET ve .NET Core için konsol ve ASP.NET proje türlerini destekler.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric ve kapsayıcıların özellikleri hakkında daha fazla bilgi edinmek için bkz. [Service Fabric kapsayıcılara genel bakış](service-fabric-containers-overview.md).
