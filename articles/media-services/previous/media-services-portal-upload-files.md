---
title: Azure portalını kullanarak Media Services hesabına dosya yükleme | Microsoft Belgeleri
description: Bu öğretici, Azure portalını kullanarak bir Azure Media Services hesabına dosya yükleme adımlarında size kılavuzluk eder
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: fdb301cd719d98d806e2a9e539cd81e6778461bb
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98695315"
---
# <a name="upload-files-to-a-media-services-account-in-the-azure-portal"></a>Azure portalını kullanarak Media Services hesabına dosya yükleme

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!div class="op_single_selector"]
> * [Portal](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. Portalla güncel karşıya yükleme dosyaları için bkz. [içeriği karşıya yüklemek, kodlamak ve akışa almak için portalı kullanma](../latest/manage-assets-quickstart.md).<br/>Ayrıca, kullanıma alma: [Media Services v3](../latest/index.yml). Ayrıca bkz. [v2 'den v3 'e geçiş kılavuzu](../latest/migrate-v-2-v-3-migration-introduction.md)

Azure Media Services’de dijital dosyalar bir varlığa yüklenir. Varlık; video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları (ve bu dosyalar için meta veriler) içerebilir. Dosyalar yüklendiğinde, içeriğiniz sonraki işleme ve akışla aktarma faaliyetleri için güvenli bir şekilde bulutta depolanmış olur.

Media Services dosyalarını işlemek için bir maksimum dosya boyutu vardır. Dosya boyutu sınırları hakkında daha fazla ayrıntı için bkz. [Media Services kotaları ve kısıtlamaları](media-services-quotas-and-limitations.md).

Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılar için bkz. [Azure Ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="upload-files"></a>Dosyaları karşıya yükleme
1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. **Ayar**  >  **varlıkları**' nı seçin. Ardından **Karşıya Yükle** düğmesini seçin.
   
    ![Dosyaları karşıya yükleme](./media/media-services-portal-vod-get-started/media-services-upload.png)
   
    **Varlığı karşıya yükle** penceresi görüntülenir.
   
   > [!NOTE]
   > Media Services, karşıya yüklenecek videoların dosya boyutunu kısıtlamaz.
 
3. Bilgisayarınızda, karşıya yüklemek istediğiniz videoya gidin. Videoyu seçin ve ardından **Tamam**’ı seçin.  
   
    Karşıya yükleme başlar. Dosya adı altında ilerleme durumunu görebilirsiniz.  

Karşıya yükleme tamamlandığında, yeni varlık **Varlıklar** bölmesinde listelenir. 

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geribildirim gönderme
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Karşıya yüklenen varlıklarınızı kodlamayı](media-services-portal-encode.md) öğrenin.

* Yapılandırılmış kapsayıcıya gelen dosyaya göre bir kodlama işi tetiklemek için Azure İşlevleri’ni de kullanabilirsiniz. Daha fazla bilgi için [Media Services: Azure Media Services’ı Azure İşlevleri ve Logic Apps ile tümleştirme](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/) örneğine bakın.
