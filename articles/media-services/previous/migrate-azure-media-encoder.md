---
title: Azure Media Encoder 'dan Media Encoder Standard 'e geçiş | Microsoft Docs
description: Bu konuda, Azure Medya Kodlayıcısı 'ndan Media Encoder Standard medya işlemcisine nasıl geçiş yapılacağı anlatılmaktadır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2019
ms.author: juliako
ms.custom: devx-track-csharp
ms.openlocfilehash: 32eab0f729280ad25f45853e05ea982b60a6d9f7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89259345"
---
# <a name="migrate-from-azure-media-encoder-to-media-encoder-standard"></a>Azure Medya Kodlayıcısı 'ndan Media Encoder Standard 'e geçiş

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Bu makalede, eski Azure Medya Kodlayıcısı (AME) medya işlemcisinden (kullanımdan kalkmakta olan) Media Encoder Standard medya işlemcisine geçiş adımları ele alınmaktadır. Kullanımdan kaldırma tarihleri için, bu [eski bileşenler](legacy-components.md) konusuna bakın.

Dosyaları AME ile kodlarken müşteriler genellikle gibi adlandırılmış bir ön ayar dizesi kullanıyordu `H264 Adaptive Bitrate MP4 Set 1080p` . Geçiş yapmak için, kodunuzun adı yerine **Media Encoder Standard** medya işlemcisini kullanacak şekilde güncelleştirilmesi gerekir ve gibi eşdeğer [sistem önayarlarından](media-services-mes-presets-overview.md) biridir `H264 Multiple Bitrate 1080p` . 

## <a name="migrating-to-media-encoder-standard"></a>Media Encoder Standard geçiriliyor

Eski medya işlemcisini kullanan tipik bir C# kod örneği aşağıda verilmiştir. 

```csharp
// Declare a new job. 
IJob job = _context.Jobs.Create("AME Job"); 
// Get a media processor reference, and pass to it the name of the  
// processor to use for the specific task. 
IMediaProcessor processor = GetLatestMediaProcessorByName("Azure Media Encoder"); 

// Create a task with the encoding details, using a string preset. 
// In this case " H264 Adaptive Bitrate MP4 Set 1080p" preset is used. 
ITask task = job.Tasks.AddNew("My encoding task", 
    processor, 
    " H264 Adaptive Bitrate MP4 Set 1080p", 
    TaskOptions.None); 
```

Media Encoder Standard kullanan güncelleştirilmiş sürüm aşağıda verilmiştir.

```csharp
// Declare a new job. 
IJob job = _context.Jobs.Create("Media Encoder Standard Job"); 
// Get a media processor reference, and pass to it the name of the  
// processor to use for the specific task. 
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard"); 

// Create a task with the encoding details, using a string preset. 
// In this case " H264 Multiple Bitrate 1080p" preset is used. 
ITask task = job.Tasks.AddNew("My encoding task", 
    processor, 
    "H264 Multiple Bitrate 1080p", 
    TaskOptions.None); 
```

### <a name="advanced-scenarios"></a>Gelişmiş senaryolar 

Onun şemasını kullanarak AME için kendi kodlama ön ayarını oluşturduysanız, [Media Encoder Standard için eşdeğer bir şema](media-services-mes-schema.md)vardır. Eski ayarların yeni kodlayıcıyla eşlenme hakkında sorularınız varsa, lütfen bizimle iletişime geçin mailto:amshelp@microsoft.com  
## <a name="known-differences"></a>Bilinen farklar 

Media Encoder Standard daha sağlam, güvenilir, daha iyi bir performansa sahiptir ve eski AME kodlayıcısından daha iyi kaliteli çıkış üretir. Bunlara ek olarak: 

* Media Encoder Standard, dosya adı farklı bir adlandırma kuralına sahip çıktı dosyaları üretir.
* Media Encoder Standard, [giriş dosyası meta verilerini](media-services-input-metadata-schema.md) ve [Çıkış dosyası meta verilerini](media-services-output-metadata-schema.md)içeren dosyalar gibi yapıtlar üretir.

## <a name="next-steps"></a>Sonraki adımlar

* [Eski bileşenler](legacy-components.md)
* [Fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/#encoding)
