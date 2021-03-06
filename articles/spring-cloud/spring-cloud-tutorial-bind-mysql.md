---
title: MySQL için Azure veritabanı örneğini Azure Spring Cloud uygulamanıza bağlama
description: MySQL için Azure veritabanı örneğini Azure Spring Cloud uygulamanıza bağlamayı öğrenin
author: bmitchell287
ms.service: spring-cloud
ms.topic: how-to
ms.date: 11/04/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: 5ca8f2b003b7f2142da329a07f929ecf31fee627
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90908254"
---
# <a name="bind-an-azure-database-for-mysql-instance-to-your-azure-spring-cloud-application"></a>MySQL için Azure veritabanı örneğini Azure Spring Cloud uygulamanıza bağlama 

**Bu makale şu şekilde geçerlidir:** ✔️ Java

Azure Spring Cloud ile, Spring Boot uygulamanızı el ile yapılandırmak yerine, Azure hizmetlerini otomatik olarak uygulamalarınıza bağlayabilirsiniz. Bu makalede, uygulamanızı MySQL için Azure veritabanı örneğine nasıl bağlayacağınız gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Dağıtılan bir Azure yay bulutu örneği
* MySQL için Azure veritabanı hesabı
* Azure CLI

Dağıtılmış bir Azure yay bulut örneğiniz yoksa, ilk Spring Cloud uygulamanızı dağıtmak için [Azure Portal kullanarak Azure yay bulutu uygulamasını başlatma](spring-cloud-quickstart.md) bölümündeki yönergeleri izleyin.

## <a name="bind-your-app-to-your-azure-database-for-mysql-instance"></a>Uygulamanızı MySQL için Azure veritabanı örneğine bağlama

1. MySQL için Azure veritabanı hesabınızın yönetici kullanıcı adını ve parolasını aklınızda bir yere göz önüne alın. 

1. Sunucuya bağlanın, MySQL istemcisinden **TestDB** adlı bir veritabanı oluşturun ve ardından yeni yönetici olmayan bir hesap oluşturun.

1. Projenizin *pom.xml* dosyasında aşağıdaki bağımlılığı ekleyin:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    ```
1. *Application. Properties* dosyasında tüm `spring.datasource.*` özellikleri kaldırın.

1. Çalıştırarak geçerli dağıtımı güncelleştirin `az spring-cloud app update` veya bu değişiklik için çalıştırarak yeni bir dağıtım oluşturun `az spring-cloud app deployment create` .  Bu komutlar uygulamayı yeni bağımlılığı ile güncelleştirir veya oluşturur.

1. Azure portal **Azure Spring Cloud** Service sayfasında, **uygulama panosunu**bulun ve ardından MySQL için Azure veritabanı örneğine bağlamak üzere uygulamayı seçin.  Bu, önceki adımda güncelleştirdiğiniz veya dağıttığınız aynı uygulamadır. 

1. **Hizmet bağlaması**' nı seçin ve ardından **hizmet bağlaması oluştur** düğmesini seçin. 

1. Daha önce kullandığınız veritabanı adını kullanarak ve ilk adımda not ettiğiniz Kullanıcı adı ve parolayı kullanarak, formu doldurun, **bağlama türü**olarak **Azure MySQL** ' i seçin.

1. Uygulamayı yeniden başlatın ve bu bağlama artık çalışmalıdır.

1. Hizmet bağlamasının doğru olduğundan emin olmak için bağlama adını seçin ve sonra ayrıntılarını doğrulayın. `property`Alan şöyle görünmelidir:
    ```
    spring.datasource.url=jdbc:mysql://some-server.mysql.database.azure.com:3306/testdb?useSSL=true&requireSSL=false&useLegacyDatetimeCode=false&serverTimezone=UTC
    spring.datasource.username=admin@some-server
    spring.datasource.password=abc******
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir Azure Spring Cloud uygulamasını MySQL için Azure veritabanı örneğine bağlamayı öğrendiniz. Bir uygulamaya hizmet bağlama hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos db veritabanını Azure Spring Cloud uygulamasına bağlama](spring-cloud-tutorial-bind-cosmos.md).
