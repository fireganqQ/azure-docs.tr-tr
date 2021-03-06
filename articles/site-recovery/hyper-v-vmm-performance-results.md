---
title: Azure Site Recovery kullanarak VMM ile ikincil siteye Hyper-V VM çoğaltmasını test etme
description: Bu makalede, Azure Site Recovery kullanarak VMM bulutlarındaki Hyper-V VM 'lerinin ikincil bir siteye çoğaltılmasına yönelik performans testi hakkında bilgi sağlanır.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: sutalasi
ms.openlocfilehash: 6c8219214e7053dcf6b119f6cd5dc97daaa355f7
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92327646"
---
# <a name="test-results-for-hyper-v-replication-to-a-secondary-site"></a>İkincil siteye Hyper-V çoğaltması için test sonuçları


Bu makalede, System Center Virtual Machine Manager (VMM) bulutlarındaki Hyper-V sanal makinelerini ikincil veri merkezine çoğaltılırken performans testinin sonuçları sağlanmaktadır.

## <a name="test-goals"></a>Test hedefleri

Testin amacı, Site Recovery kararlı durum çoğaltması sırasında nasıl gerçekleştiğini incelemektir.

- Sabit durum çoğaltma, VM 'Lerin ilk çoğaltmasını tamamladığı ve Delta değişikliklerini eşitlemekte meydana gelir.
- Beklenmeyen kesintiler gerçekleşmedikleri takdirde, çoğu VM 'nin kalacağı durum olduğundan, düzenli durum kullanılarak performansı ölçmek önemlidir.
- Test dağıtımı, her sitede bir VMM sunucusu ile iki şirket içi siteden oluşur. Bu test dağıtımı, Merkez Ofis/Şube ofisi dağıtımının, birincil site olarak hareket eden ve şube ofisi ikincil veya kurtarma sitesi olarak çalışan bir merkez ofisidir.

## <a name="what-we-did"></a>Yaptığımız

İşte test geçişimiz:

1. VMM şablonları kullanılarak VM 'Ler oluşturuldu.
2. Başlatılmış VM 'Ler ve 12 saat boyunca yakalanan temel performans ölçümleri.
3. Birincil ve kurtarma VMM sunucularında bulutlar oluşturuldu.
4. Kaynak ve kurtarma bulutları arasında eşleme de dahil olmak üzere Site Recovery olarak yapılandırılmış çoğaltma.
5. Sanal makineler için koruma etkinleştirildi ve ilk çoğaltmayı tamammaları için izin verildi.
6. Sistem sabitlemesi için birkaç saat bekledi.
7. 12 saat üzerinden yakalanan performans ölçümleri, tüm VM 'Lerin bu 12 saat için beklenen bir çoğaltma durumunda kaldığı.
8. Taban çizgisi performans ölçümleri ve çoğaltma performansı ölçümleri arasındaki Delta değerini ölçülür.


## <a name="primary-server-performance"></a>Birincil sunucu performansı

* Hyper-V çoğaltma (Site Recovery tarafından kullanılan), bir günlük dosyasındaki değişiklikleri zaman uyumsuz olarak birincil sunucuda en düşük depolama ek yüküne göre izler.
* Hyper-V çoğaltma, izleme için ıOPS yükünü en aza indirmek üzere kendi kendine korunan bellek önbelleğini kullanır. Yazma işlemlerini bellekte depoladığında depolar ve günlüğü kurtarma sitesine gönderilmeden önce günlük dosyasına boşaltır. Bir disk temizleme işlemi, yazmaları önceden belirlenmiş bir sınıra ulaşıyorsa da oluşur.
* Aşağıdaki grafikte, çoğaltma için sabit durum ıOPS ek yükü gösterilmektedir. Çoğaltma nedeniyle ıOPS ek yükünün %5 ' ün üzerinde olduğunu görebiliriz. Bu, oldukça düşüktür.

  ![Çoğaltma için sabit durum ıOPS ek yükünü gösteren grafik.](./media/hyper-v-vmm-performance-results/IC744913.png)

Hyper-V çoğaltma, disk performansını iyileştirmek için birincil sunucuda bellek kullanır. Aşağıdaki grafikte gösterildiği gibi, birincil kümedeki tüm sunuculardaki bellek ek yükü marguinal olur. Gösterilen bellek yükü, Hyper-V sunucusundaki toplam yüklü bellekle karşılaştırıldığında çoğaltma tarafından kullanılan bellek yüzdesidir.

![Birincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744914.png)

Hyper-V çoğaltmasında en düşük CPU ek yükü vardır. Grafikte gösterildiği gibi, çoğaltma ek yükü% 2-3 aralığındadır.

![Çoğaltma ek yükünü gösteren grafik% 2-3 aralığındadır.](./media/hyper-v-vmm-performance-results/IC744915.png)

## <a name="secondary-server-performance"></a>İkincil sunucu performansı

Hyper-V çoğaltma, depolama işlemlerinin sayısını iyileştirmek için kurtarma sunucusunda az miktarda bellek kullanır. Grafik, kurtarma sunucusundaki bellek kullanımını özetler. Gösterilen bellek yükü, Hyper-V sunucusundaki toplam yüklü bellekle karşılaştırıldığında çoğaltma tarafından kullanılan bellek yüzdesidir.

![Kurtarma sunucusundaki bellek kullanımını özetleyen grafik.](./media/hyper-v-vmm-performance-results/IC744916.png)

Kurtarma sitesindeki g/ç işlemlerinin miktarı, birincil sitede yazma işlemi sayısı işlevidir. Birincil sitedeki toplam g/ç işlemleri ve yazma işlemleri ile karşılaştırıldığında, kurtarma sitesindeki toplam g/ç işlemlerine göz atalım. Grafiklerde, kurtarma sitesindeki toplam ıOPS 'nin olduğunu gösterir

* Birincil üzerinde yazma ıOPS 1,5 ' dir.
* Birincil sitedeki toplam ıOPS 'nin %37 ' i.

![Birincil ve ikincil sitelerde ıOPS 'nin karşılaştırmasını gösteren grafik.](./media/hyper-v-vmm-performance-results/IC744917.png)

![İkincil sonuçlar](./media/hyper-v-vmm-performance-results/IC744918.png)

## <a name="effect-on-network-utilization"></a>Ağ kullanımı üzerindeki etki

Birincil ve kurtarma düğümleri arasında (sıkıştırma etkin), saniye başına 5 GB olan mevcut bir bant genişliğine karşı bir ortalama 275 MB 'Lık ağ bant genişliği kullanılır.

![Sonuç ağ kullanımı](./media/hyper-v-vmm-performance-results/IC744919.png)

## <a name="effect-on-vm-performance"></a>VM performansı üzerindeki etki

Önemli bir değerlendirme, sanal makinelerde çalışan üretim iş yüklerindeki çoğaltmanın etkisidir. Birincil site çoğaltma için yeterli şekilde sağlandıysa, iş yükleri üzerinde herhangi bir etkisi olmamalıdır. Hyper-V çoğaltmasının hafif izleme mekanizması, sanal makinelerde çalışan iş yüklerinin kararlı durum çoğaltma sırasında etkilenmemesini sağlar. Bu, aşağıdaki grafiklerde gösterilmiştir.

Bu grafik, çoğaltma etkinleştirilmeden önce ve sonra farklı iş yükleri çalıştıran sanal makineler tarafından gerçekleştirilen ıOPS 'yi gösterir. İkisi arasında fark olmadığını gözlemleyebilirsiniz.

![Çoğaltma efekti sonuçları](./media/hyper-v-vmm-performance-results/IC744920.png)

Aşağıdaki grafikte, çoğaltma etkinleştirilmeden önce ve sonra farklı iş yüklerini çalıştıran sanal makinelerin aktarım hızı gösterilmektedir. Çoğaltmanın önemli bir etkisi olmadığını gözlemleyebilirsiniz.

![Sonuçlar çoğaltma etkileri](./media/hyper-v-vmm-performance-results/IC744921.png)

## <a name="conclusion"></a>Sonuç

Sonuçlar, Hyper-V çoğaltmayla bağlanmış Site Recovery açıkça gösterir, büyük bir küme için en düşük ek yüke göre ölçeklenebilen. Site Recovery basit dağıtım, çoğaltma, yönetim ve izleme sağlar. Hyper-V çoğaltma, başarılı çoğaltma ölçeklendirmesi için gerekli altyapıyı sağlar. 

## <a name="test-environment-details"></a>Test ortamı ayrıntıları

### <a name="primary-site"></a>Birincil site

* Birincil sitenin, 470 sanal makine çalıştıran beş Hyper-V sunucusu içeren bir kümesi vardır.
* VM 'Ler farklı iş yüklerini çalıştırır ve tümünün Site Recovery koruması etkinleştirilmiştir.
* Küme düğümü için depolama, bir Iscsı SAN tarafından sağlanır. Model – Hitachi HUS130.
* Her küme sunucusunda dört GB/sn 'lik bir ağ kartı (NIC) vardır.
* Ağ kartlarının ikisi bir Iscsı özel ağına bağlıdır ve iki harici bir kurumsal ağa bağlanır. Dış ağlardan biri yalnızca küme iletişimleri için ayrılmıştır.

![Birincil donanım gereksinimleri](./media/hyper-v-vmm-performance-results/IC744922.png)

| Sunucu | RAM | Model | İşlemci | İşlemci sayısı | NIC | Yazılım |
| --- | --- | --- | --- | --- | --- | --- |
| Kümedeki Hyper-V sunucuları: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25 |128<br />ESTLAB-HOST25 256 |Dell™ PowerEdge™ R820 |Intel (R) Xeon (R) CPU E5-4620 0 \@ 2.20 GHz |4 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V rolü |
| VMM Sunucusu |2 | | |2 |1 Gb/sn |Windows Server veritabanı 2012 R2 (x64) + VMM 2012 R2 |

### <a name="secondary-site"></a>İkincil site

* İkincil sitede altı düğümlü bir yük devretme kümesi vardır.
* Küme düğümü için depolama, bir Iscsı SAN tarafından sağlanır. Model – Hitachi HUS130.

![Birincil donanım belirtimi](./media/hyper-v-vmm-performance-results/IC744923.png)

| Sunucu | RAM | Model | İşlemci | İşlemci sayısı | NIC | Yazılım |
| --- | --- | --- | --- | --- | --- | --- |
| Kümedeki Hyper-V sunucuları: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10 |96 |Dell™ PowerEdge™ R720 |Intel (R) Xeon (R) CPU E5-2630 0 \@ 2.30 GHz |2 |I Gbps x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V rolü |
| ESTLAB-HOST17 |128 |Dell™ PowerEdge™ R820 |Intel (R) Xeon (R) CPU E5-4620 0 \@ 2.20 GHz |4 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V rolü |
| ESTLAB-HOST24 |256 |Dell™ PowerEdge™ R820 |Intel (R) Xeon (R) CPU E5-4620 0 \@ 2.20 GHz |2 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V rolü |
| VMM Sunucusu |2 | | |2 |1 Gb/sn |Windows Server veritabanı 2012 R2 (x64) + VMM 2012 R2 |

### <a name="server-workloads"></a>Sunucu iş yükleri

* Test amacıyla kurumsal müşteri senaryolarında yaygın olarak kullanılan iş yüklerini seçtik.
* Simülasyonu için tabloda özetlenen iş yükü özelliği ile [Iometer](http://www.iometer.org) kullanıyoruz.
* Tüm Iometer profilleri, iş yükleri için en kötü durum yazma desenlerinin benzetimini yapmak için rastgele baytlar yazmak üzere ayarlanır.

| İş Yükü | G/ç boyutu (KB) | Erişim yüzdesi | % Okuma | Bekleyen g/ç | G/ç stili |
| --- | --- | --- | --- | --- | --- |
| Dosya Sunucusu |4<br />8<br />16<br />32<br />64 |%60<br />%20<br />%5<br />%5<br />%10 |%80<br />%80<br />%80<br />%80<br />%80 |8<br />8<br />8<br />8<br />8 |Tüm %100 rastgele |
| SQL Server (birim 1)<br />SQL Server (birim 2) |8<br />64 |%100<br />%100 |%70<br />%0 |8<br />8 |%100 rastgele<br />%100 sıralı |
| Exchange |32 |%100 |%67 |8 |%100 rastgele |
| İş istasyonu/VDı |4<br />64 |%66<br />%34 |%70<br />%95 |1<br />1 |Hem %100 rastgele |
| Web dosya sunucusu |4<br />8<br />64 |%33<br />%34<br />%33 |%95<br />%95<br />%95 |8<br />8<br />8 |Tüm %75 rastgele |

### <a name="vm-configuration"></a>VM yapılandırması

* birincil kümedeki 470 VM 'Ler.
* VHDX diski olan tüm VM 'Ler.
* Tabloda özetlenen iş yüklerini çalıştıran VM 'Ler. Tümü VMM şablonlarıyla oluşturulmuştur.

| İş Yükü | VM 'Ler | En düşük RAM (GB) | Maksimum RAM (GB) | VM başına mantıksal disk boyutu (GB) | Maksimum ıOPS |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1 |4 |167 |10 |
| Exchange Server |71 |1 |4 |552 |10 |
| Dosya Sunucusu |50 |1 |2 |552 |22 |
| VDI |149 |5 |1 |80 |6 |
| Web sunucusu |149 |5 |1 |80 |6 |
| TOPLAM |470 | | |96,83 TB |4108 |

### <a name="site-recovery-settings"></a>Site Recovery ayarları

* Site Recovery şirket içi ve şirket içi koruma için yapılandırıldı
* VMM sunucusunda, Hyper-V küme sunucularını ve bunların sanal makinelerini içeren dört bulut yapılandırılır.

| Birincil VMM bulutu | Korumalı VM'ler | Çoğaltma sıklığı | Ek kurtarma noktaları |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 dakika |Yok |
| PrimaryCloudRpo30s |47 |30 saniye |Yok |
| PrimaryCloudRpo30sArp1 |47 |30 saniye |1 |
| PrimaryCloudRpo5m |235 |5 dk. |Yok |

### <a name="performance-metrics"></a>Performans ölçümleri

Tablo, dağıtımda ölçülen performans ölçümlerini ve sayaçlarını özetler.

| Ölçüm | Sayaç |
| --- | --- |
| CPU |\Processor(_Total)\% Processor Time |
| Kullanılabilir bellek |\Bellek\kullanılabilir MBayt |
| IOPS |\Fiziksel disk (_Total) \Disk aktarımı/sn |
| VM okuma (ıOPS) işlemi/sn |\Hyper-V sanal depolama cihazı ( \<VHD> ) \ okuma işlemi/sn |
| VM yazma (ıOPS) işlemi/sn |\Hyper-V sanal depolama cihazı ( \<VHD> ) \Yazma işlemleri/sn |
| VM okuma performansı |\Hyper-V sanal depolama cihazı ( \<VHD> ) \Okunan bayt/sn |
| VM yazma performansı |\Hyper-V sanal depolama cihazı ( \<VHD> ) \Yazılan bayt/sn |

## <a name="next-steps"></a>Sonraki adımlar

[Çoğaltmayı ayarlama](hyper-v-vmm-disaster-recovery.md)
