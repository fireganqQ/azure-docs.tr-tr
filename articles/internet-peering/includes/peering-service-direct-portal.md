---
title: dosya dahil etme
titleSuffix: Azure
description: dosya dahil etme
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: include
ms.date: 3/18/2020
ms.author: derekol
ms.openlocfilehash: e5804aa1b005e670d8b430b1c0a3bd62efd0bb06
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "81687035"
---
1. Azure eşleme hizmeti için etkinleştirmek istediğiniz eşleme bağlantısını seçin. Ardından **.**  >  .. öğesini seçin **Bağlantıyı Düzenle**.
    > [!div class="mx-imgBorder"]
    > ![Eşleme bağlantısı bağlantıyı Düzenle](../media/setup-direct-modify-editconnection.png)
1. **Eşleme hizmeti Için kullanım**altında, **etkin** ' i seçin ve ardından **Kaydet**' i seçin.
    > [!div class="mx-imgBorder"]
    > ![Eşleme bağlantısı eşleme hizmetini etkinleştir](../media/setup-direct-modify-editconnectionsettings-peering-service.png)
1. **Genel bakış** ekranında dağıtım ayrıntılarını görürsünüz. Dağıtımınız tamamlandıktan sonra **Kaynağa Git**' i seçin.
    > [!div class="mx-imgBorder"]
    > ![Dağıtımınız tamamlanmıştır](../media/setup-direct-modify-overview-deployment-complete.png)

1. Kayıtlı ön **ekler** bölmesinde, **kayıtlı ön ek ekle**' yi seçin.
    > [!div class="mx-imgBorder"]
    > ![Kayıtlı ön ek ekle](../media/setup-direct-modify-add-registered-prefix.png)
1. Bir **ad** ve **ön ek** seçerek ve **Kaydet**' i seçerek ön eki kaydedin.
    > [!div class="mx-imgBorder"]
    >  ![Ön ek Kaydet](../media/setup-direct-modify-register-a-prefix.png) 

1. Ön ek oluşturulduktan sonra, **kayıtlı ön ekler**listesinde görürsünüz. Daha fazla ayrıntı görmek için ön ek **adını** seçin.
    > [!div class="mx-imgBorder"]
    > ![Kayıtlı ön ekler ve bağlantılar](../media/setup-direct-modify-registered-prefixes.png)
1. Kayıtlı önek sayfasında, her ön ek için **önek anahtarını** içeren tüm ayrıntıları görürsünüz. Bu anahtarın, sağlayıcı ISS 'sinden bu öneki ayırdığı müşteriye sağlanması gerekir. Daha sonra bu anahtarı kullanarak, müşteri ön eklerini kendi abonelikleri içinde kaydedebilir.
    > [!div class="mx-imgBorder"]
    > ![Ön ek anahtarına sahip önek](../media/setup-direct-modify-registered-prefix-detail.png)