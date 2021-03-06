---
title: HB Serisi VM 'ye genel bakış-Azure sanal makineleri | Microsoft Docs
description: Azure 'da HB Serisi VM boyutu için Önizleme desteği hakkında bilgi edinin.
author: vermagit
ms.service: virtual-machines
ms.subservice: workloads
ms.topic: article
ms.date: 08/19/2020
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: 1e37f13bc6e819079c63c583a1eeda12af5e213c
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94965468"
---
# <a name="hb-series-virtual-machines-overview"></a>HB Serisi sanal makinelere genel bakış

Yüksek performanslı bilgi işlem (HPC) uygulama performansının AMD EPıC üzerinde en üst düzeye çıkarmak, düşünceli bir yaklaşım bellek konumu ve işlem yerleşimi gerektirir. Aşağıda, AMD EPıC mimarisini ve bu uygulamayı HPC uygulamaları için Azure 'da uygulamamız ana hatlarıyla planlıyoruz. "PNUMA" terimini bir fiziksel NUMA etki alanına ve "vNUMA" adlı bir sanallaştırılmış NUMA etki alanına başvuracak şekilde kullanacağız.

Fiziksel olarak, bir [HB Serisi](../../hb-series.md) sunucu 2 * 32-çekirdekli epyıc 7551 CPU ve Toplam 64. fiziksel çekirdek için. Bu 64 çekirdekler, her biri dört çekirdekli ve "CPU karmaşık" (veya "CCX") olarak bilinen 16 pNUMA etki alanlarına (yuva başına 8) bölünmüştür. Her CCX 'in kendi L3 önbelleği vardır. Bu, bir işletim sisteminin bir pNUMA/vNUMA sınırını nasıl görebileceği anlamına gelir. Bitişik CCXs çifti, iki fiziksel DRAM kanalına erişimi paylaşır (32 GB/s-serisi sunucu).

Azure Hiper Yöneticisi 'nin VM ile kesintiye uğramadan çalışması için oda sağlamak üzere fiziksel pNUMA etki alanı 0 ' ı (ilk CCX) ayırdık. Daha sonra VM için pNUMA etki alanları 1-15 (kalan CCX birimleri) atarsınız. VM şu şekilde görüntülenir:

`(15 vNUMA domains) * (4 cores/vNUMA) = 60` VM başına çekirdek

Sanal makine, pNUMA 0 ' ın kendisine verilmediğini bilmez. VM, vNUMA 0-14 olarak pNUMA 1-15 'yi, vSocket 0 ve vSocket 1 üzerinde 8 vNUMA üzerinde 7 vNUMA ile anlamıştır. Bu, asimetrik olsa da, işletim sistemi normal şekilde önyükleme ve çalışır olmalıdır. Bu kılavuzda daha sonra, bu asimetrik NUMA düzeninde MPı uygulamalarının ne kadar iyi çalıştırılacağını öğreneceksiniz.

İşlem sabitleme, HB Serisi VM 'lerde çalışarak temel bir Silicon as 'yi konuk VM 'de kullanıma sunduk. En iyi performans ve tutarlılık için işlem sabitlenmesini önemle öneririz.

Aşağıdaki diyagramda, Azure Hiper Yöneticisi ve HB Serisi VM için ayrılan çekirdekler gösterilmektedir.

![Azure Hiper Yöneticisi ve HB Serisi VM için ayrılan çekirdekler](./media/hb-series-overview/segregation-cores.png)

## <a name="hardware-specifications"></a>Donanım belirtimleri

| Donanım belirtimleri                | HB Serisi VM                     |
|----------------------------------|----------------------------------|
| Çekirdekler                            | 60 (SMT devre dışı)                |
| CPU                              | AMD EPYıC 7551                    |
| CPU sıklığı (AVX olmayan)          | ~ 2,55 GHz (tek + tüm çekirdekler)   |
| Bellek                           | 4 GB/çekirdek (240 GB toplam)         |
| Yerel Disk                       | 700 GB SSD                       |
| InfiniBand                       | 100 GB EDR Mellanox ConnectX-5 |
| Ağ                          | 50 GB Ethernet (40 GB kullanılabilir) Azure ikinci gen Smartnıc |

## <a name="software-specifications"></a>Yazılım belirtimleri

| Yazılım belirtimleri           |HB Serisi VM           |
|-----------------------------|-----------------------|
| Maksimum MPı Işi boyutu            | 18000 çekirdek (tek bir sanal makine ölçek kümesindeki Tekplacementgroup = true ile, 300 VM)  |
| MPı desteği                 | HPC-X, Intel MPI, OpenMPI, MVAPICH2, MPICH, platform MPı  |
| Ek çerçeveler       | Birleşik Iletişim X, libfabric, PGAS |
| Azure depolama desteği       | Standart ve Premium diskler (en fazla 4 disk) |
| SRLOV RDMA için işletim sistemi desteği   | CentOS/RHEL 7.6 +, SLES 12 SP4 +, WinServer 2016 +  |
| Orchestrator desteği        | CycleCloud, Batch  |

## <a name="next-steps"></a>Sonraki adımlar

- [AMD epi mimarisi](https://bit.ly/2Epv3kC) ve [çok yongalı mimariler](https://bit.ly/2GpQIMb)hakkında daha fazla bilgi edinin. Daha ayrıntılı bilgi için bkz. [AMD EPYıC işlemcileri Için HPC ayarlama Kılavuzu](https://bit.ly/2T3AWZ9).
- En son duyurular ve bazı HPC örnekleri hakkında bilgi edinin ve [Azure Işlem teknik topluluk bloglarında](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute)bu sonuçları elde edin.
- Çalıştırılan HPC iş yüklerinin daha yüksek düzey mimari görünümü için bkz. [Azure 'Da yüksek performanslı bilgi işlem (HPC)](/azure/architecture/topics/high-performance-computing/).
