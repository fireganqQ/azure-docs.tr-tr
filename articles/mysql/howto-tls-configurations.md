---
title: TLS yapılandırması-Azure portal-MySQL için Azure veritabanı
description: MySQL için Azure veritabanınız için Azure portal kullanarak TLS yapılandırması ayarlamayı öğrenin
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 06/02/2020
ms.openlocfilehash: 290752c0e577e6c2cd58d83f77fea8a5406388e4
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93240639"
---
# <a name="configuring-tls-settings-in-azure-database-for-mysql-using-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı 'nda TLS ayarlarını yapılandırma

Bu makalede, bir MySQL için Azure veritabanı sunucusunu, bağlantıların en düşük TLS sürümüne sahip tüm bağlantıları ve bu sayede ağ güvenliğini artırmasını sağlamak için izin verilen minimum TLS sürümünü zorlamak üzere nasıl yapılandırabileceğiniz açıklanmaktadır.

MySQL için Azure veritabanı 'na bağlanmak üzere TLS sürümü uygulayabilirsiniz. Müşteriler artık veritabanı sunucuları için en düşük TLS sürümünü ayarlamayı tercih etmektedir. Örneğin, bu en düşük TLS sürümünü 1,0 olarak ayarlamak, TLS 1.0, 1.1 ve 1,2 kullanarak bağlanan istemcilere izin vermeyeceğiniz anlamına gelir. Alternatif olarak, bunu 1,2 olarak ayarlamak yalnızca TLS 1.2 + kullanarak bağlanan istemcilere izin vermek ve TLS 1,0 ve TLS 1,1 ile gelen tüm bağlantıları reddedilecek anlamına gelir.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

* [MySQL Için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için TLS yapılandırması ayarlama

MySQL Server en düşük TLS sürümünü ayarlamak için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com/), var olan MySQL Için Azure veritabanı sunucunuzu seçin.

1. MySQL sunucusu sayfasında, **Ayarlar** altında **bağlantı güvenliği** ' ne tıklayarak bağlantı güvenliği yapılandırması sayfasını açın.

1. **En düşük TLS** sürümü ' nde, MySQL sunucunuz için TLS 1,2 ' den düşük olan bağlantıları reddetmek için **1,2** ' ı seçin.

    :::image type="content" source="./media/howto-tls-configurations/setting-tls-value.png" alt-text="MySQL için Azure veritabanı TLS yapılandırması":::

1. Değişiklikleri kaydetmek için **Kaydet** ’e tıklayın.

1. Bildirim, bağlantı güvenliği ayarının başarıyla etkinleştirildiğini onaylanır.

    :::image type="content" source="./media/howto-tls-configurations/setting-tls-value-success.png" alt-text="MySQL için Azure veritabanı TLS yapılandırması başarılı":::

## <a name="next-steps"></a>Sonraki adımlar

- [Ölçümler üzerinde uyarı oluşturma](howto-alert-on-metric.md) hakkında bilgi edinin