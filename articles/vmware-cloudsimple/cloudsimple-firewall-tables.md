---
title: CloudSimple-Firewall tabloları tarafından Azure VMware çözümü
description: Her güvenlik duvarı tablosunda oluşturulan varsayılan kurallar dahil olmak üzere CloudSimple özel bulut güvenlik duvarı tabloları ve güvenlik duvarı kuralları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8c44c39f66a0a0161eea8a7e9656bbe0e3d1015c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88140879"
---
# <a name="firewall-tables-overview"></a>Güvenlik Duvarı tablolarına genel bakış

Bir güvenlik duvarı tablosu, özel bulut kaynaklarından gelen ve giden ağ trafiğini filtrelemek için kuralları listeler. Bir VLAN/subnet 'e güvenlik duvarı tabloları uygulayabilirsiniz. Kurallar, bir kaynak ağ veya IP adresi ile hedef ağ veya IP adresi arasındaki ağ trafiğini denetler.

## <a name="firewall-rules"></a>Güvenlik duvarı kuralları

Aşağıdaki tabloda bir güvenlik duvarı kuralındaki parametreler açıklanmaktadır.

| Özellik | Ayrıntılar |
| ---------| --------|
| **Ad** | Güvenlik duvarı kuralını ve amacını benzersiz bir şekilde tanımlayan bir ad. |
| **Priority** | 100 ile 4096 arasında bir sayı, 100 en yüksek önceliğe sahip. Kurallar öncelik sırasına göre işlenir. Trafik bir kural eşleşmesi ile karşılaştığında, kural işleme durduruluyor. Sonuç olarak, daha yüksek önceliklere sahip kurallarla aynı özniteliklere sahip düşük önceliklere sahip kurallar işlenmez.  Çakışan kurallardan kaçınmak için dikkatli olmanız. |
| **Durum Izleme** | İzleme durum bilgisiz (özel bulut, Internet veya VPN) ya da durum bilgisi olan (genel IP) olabilir.  |
| **Protokol** | Seçenekler arasında, TCP veya UDP bulunur. ICMP gerekliyse, herhangi birini kullanın. |
| **Yön** | Kuralın gelen veya giden trafiğe uygulanma seçeneği. |
| **Eylem** | Kuralda tanımlanan trafik türüne izin verin veya reddedin. |
| **Kaynak** | Bir IP adresi, sınıfsız etki alanları arası yönlendirme (CıDR) bloğu (10.0.0.0/24, örneğin) veya herhangi bir.  Bir Aralık, hizmet etiketi veya uygulama güvenlik grubu belirtme, daha az güvenlik kuralı oluşturmanıza olanak sağlar. |
| **Kaynak bağlantı noktası** | Ağ trafiğinin kaynaklandığı bağlantı noktası.  Tek bir bağlantı noktası veya bağlantı noktası aralığı belirtebilirsiniz (örneğin, 443 veya 8000-8080). Aralık belirterek oluşturmanız gereken güvenlik kuralı sayısını azaltabilirsiniz. |
| **Hedef** | Bir IP adresi, sınıfsız etki alanları arası yönlendirme (CıDR) bloğu (10.0.0.0/24, örneğin) veya herhangi bir.  Bir Aralık, hizmet etiketi veya uygulama güvenlik grubu belirtme, daha az güvenlik kuralı oluşturmanıza olanak sağlar.  |
| **Hedef bağlantı noktası** | Ağ trafiğinin akacağını belirten bağlantı noktası.  Tek bir bağlantı noktası veya bağlantı noktası aralığı belirtebilirsiniz (örneğin, 443 veya 8000-8080). Aralık belirterek oluşturmanız gereken güvenlik kuralı sayısını azaltabilirsiniz.|

### <a name="stateless"></a>Durum bilgisi olmayan

Durum bilgisi olmayan bir kural yalnızca tek tek paketlere bakar ve kurala göre filtre uygular.  
Trafik akışı için ters yönde ek kurallar gerekebilir.  Aşağıdaki noktaları arasındaki trafik için durum bilgisi olmayan kurallar kullanın:

* Özel bulutların alt ağları
* Şirket içi alt ağ ve özel bulut alt ağı
* Özel bulutlardan Internet trafiği

### <a name="stateful"></a>Durum Bilgisi Olan

 Durum bilgisi olan bir kural, üzerinden geçen bağlantılardan haberdar olur. Var olan bağlantılar için bir akış kaydı oluşturulur. Akış kaydının bağlantı durumuna göre iletişime izin verilir veya iletişim reddedilir.  Trafiği Internet 'ten filtrelemek için genel IP adresleri için bu kural türünü kullanın.

### <a name="default-rules"></a>Varsayılan kurallar

Aşağıdaki varsayılan kurallar her güvenlik duvarı tablosunda oluşturulur.

|Öncelik|Adı|Durum Izleme|Yön|Trafik türü|Protokol|Kaynak|Kaynak Bağlantı Noktası|Hedef|Hedef Bağlantı Noktası|Eylem|
|--------|----|--------------|---------|------------|--------|------|-----------|-----------|----------------|------|
|65000|Tüm-Internet 'e izin ver|Durum Bilgisi Olan|Outbound|Genel IP veya internet trafiği|Tümü|Herhangi biri|Herhangi biri|Herhangi biri|Herhangi biri|İzin Ver|
|65001|Reddet-tümü-internet 'ten|Durum Bilgisi Olan|Inbound|Genel IP veya internet trafiği|Tümü|Herhangi biri|Herhangi biri|Herhangi biri|Herhangi biri|Reddet|
|65002|tümünü-intranete izin ver|Durum bilgisi olmayan|Outbound|Özel bulut iç veya VPN trafiği|Tümü|Herhangi biri|Herhangi biri|Herhangi biri|Herhangi biri|İzin Ver|
|65003|-All-ıntranetden izin ver|Durum bilgisi olmayan|Inbound|Özel bulut iç veya VPN trafiği|Tümü|Herhangi biri|Herhangi biri|Herhangi biri|Herhangi biri|İzin Ver|

## <a name="next-steps"></a>Sonraki adımlar

* [Güvenlik duvarı tablolarını ve kurallarını ayarlama](firewall.md)
