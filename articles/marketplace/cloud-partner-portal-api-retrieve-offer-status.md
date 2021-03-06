---
title: Teklif durumunu alma-Azure Marketi
description: Teklifin geçerli durumunu almak için API.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 336f23f83c33bcee1887d0e41710e686b794a663
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87272020"
---
# <a name="retrieve-offer-status"></a>Teklif durumunu alma

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri ile tümleşiktir ve Iş Ortağı Merkezi 'nde çalışmaya devam edecektir. Geçiş küçük değişiklikler sunar. Iş Ortağı Merkezi 'ne geçtikten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](./cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin. CPP API 'Leri yalnızca Iş Ortağı Merkezi 'ne geçişten önce tümleştirilmiş mevcut ürünler için kullanılmalıdır; Yeni ürünlerin Iş Ortağı Merkezi gönderme API 'Leri kullanması gerekir.

Teklifin geçerli durumunu alır.

  `GET  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/status?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI parametreleri

|  **Ad**       |   **Açıklama**                            |  **Veri türü** |
|  -------------  |  ------------------------------------------  |  ------------  |
|  PublisherId    | Yayımcı tanımlayıcı, örneğin `Contoso`  |     Dize     |
|  OfferId        | Teklifi benzersiz bir şekilde tanımlayan GUID      |     Dize     |
|  api-sürümü    | En son API sürümü                        |     Tarih       |
|  |  |

## <a name="header"></a>Üst bilgi


|  Name           |  Değer               |
|  -------------  | -------------------  |
|  İçerik Türü   |  `application/json`  |
|  Yetkilendirme  | `Bearer YOUR_TOKEN`  |
|  |  |

## <a name="body-example"></a>Gövde örneği

### <a name="response"></a>Yanıt

``` json
  {
      "status": "succeeded",
      "messages": [],
      "steps": [
      {
          "estimatedTimeFrame": "< 15 min",
          "id": "displaydummycertify",
          "stepName": "Validate Pre-Requisites",
          "description": "Offer settings provided are validated.",
          "status": "complete",
          "messages": [
              {
                  "messageHtml": "Step completed.",
                  "level": "information",
                  "timestamp": "2018-03-16T17:50:45.7215661Z"
              }
          ],       
          "progressPercentage": 100
      },
      {
          "estimatedTimeFrame": "~2-3 days",
          "id": "displaycertify",
          "stepName": "Certification",
          "description": "Your offer is analyzed by our certification systems for issues.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 day",
          "id": "displayprovision",
          "stepName": "Provisioning",
          "description": "Your virtual machine is being replicated in our production systems.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "displaypackage",
          "stepName": "Packaging and Lead Generation Registration",
          "description": "Your virtual machine is being packaged for customers. Additionally, lead systems are being configured and set up.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "publisher-signoff",
          "stepName": "Publisher signoff",
          "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "~2-5 days",
          "id": "live",
          "stepName": "Live",
          "description": "Offer is publicly visible and is available for purchase.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      }
      ],
      "previewLinks": [],
      liveLinks": [],
  }
```

### <a name="response-body-properties"></a>Yanıt gövdesi özellikleri

|  **Ad**             |    **Açıklama**                                                                             |
| --------------------  |   -------------------------------------------------------------------------------------------- |
|  durum               | Teklifin durumu. Olası değerler listesi için bkz. [teklif durumu](#offer-status) aşağıdaki. |
|  sayısı             | Teklifle ilişkili ileti dizisi                                                    |
|  adımlar                | Teklifin bir teklif yayımlaması sırasında gittiği adımların dizisi                      |
|  Estimatedzaman çerçevesi   | Bu adımın tamamlanması için gereken süre (kolay biçimde)                       |
|  kimlik                   | Adımın tanımlayıcısı                                                                         |
|  stepName             | Adımın adı                                                                               |
|  açıklama          | Adımın açıklaması                                                                        |
|  durum               | Adımın durumu. Olası değerler listesi için aşağıdaki [adım durumuna](#step-status) bakın.    |
|  sayısı             | Adımla ilgili ileti dizisi                                                          |
|  processPercentage    | Adımın tamamlanma yüzdesi                                                              |
|  Önizleme bağlantıları         | *Şu anda uygulanmadı*                                                                    |
|  Livelmürekkepler            | *Şu anda uygulanmadı*                                                                    |
|  Notificationepostalar   | Iş Ortağı Merkezi 'ne geçirilen tekliflere kullanım dışı bırakılmıştır. Geçirilen tekliflere yönelik bildirim e-postaları, hesap ayarları 'ndaki satıcı iletişim bilgileri altında belirtilen e-postaya gönderilir.<br><br>Geçirilmeyen tekliflere, işlemin ilerleme durumu hakkında bildirim almak için virgülle ayrılmış e-posta adresleri listesi        |
|  |  |

### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod** |   **Açıklama**                                                                                 |
| -------  |   ----------------------------------------------------------------------------------------------- |
|  200     |  `OK` -İstek başarıyla işlendi ve teklifin geçerli durumu döndürüldü. |
|  400     | `Bad/Malformed request` -Hata yanıtı gövdesinde daha fazla bilgi bulunabilir.                 |
|  404     | `Not found` -Belirtilen varlık yok.                                                |
|  |  |

### <a name="offer-status"></a>Teklif durumu

|  **Ad**                    |    **Açıklama**                                       |
|  --------------------------  |  ------------------------------------------------------  |
|  Neveryayınlandı              | Teklif hiç yayımlanmadı.                          |
|  NotStarted                  | Teklif yenidir ve başlatılmamaktadır.                            |
|  WaitingForPublisherReview   | Teklif, yayımcı onayını bekliyor.                 |
|  Çalışma                     | Teklif gönderimi işleniyor.                     |
|  Başarılı                   | Teklif gönderimi işlemeyi tamamladı.               |
|  İptal edildi                    | Teklif Gönderimi iptal edildi.                           |
|  Başarısız                      | Teklif gönderimi başarısız oldu.                                 |
|  |  |

### <a name="step-status"></a>Adım durumu

|  **Ad**                    |    **Açıklama**                           |
|  -------------------------   |  ------------------------------------------  |
|  NotStarted                  | Adım başlatılmadı.                        |
|  Ediyor                  | Adım çalışıyor.                             |
|  WaitingForPublisherReview   | Adım, yayımcı onayını bekliyor.      |
|  WaitingForApproval          | Adım, işlem onayı bekliyor.        |
|  Engellendi                     | Adım engellendi.                             |
|  Reddedildi                    | Adım reddedildi.                            |
|  Tamamla                    | Adım tamamlanmıştır.                            |
|  İptal edildi                    | Adım iptal edildi.                           |
|  |  |
