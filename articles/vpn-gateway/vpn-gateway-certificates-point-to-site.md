---
title: 'P2S için sertifikaları oluşturma ve dışarı aktarma: PowerShell'
titleSuffix: Azure VPN Gateway
description: Windows 10 veya Windows Server 2016 ' de PowerShell kullanarak otomatik olarak imzalanan bir kök sertifika oluşturun, ortak anahtarı dışarı aktarın ve P2S için istemci sertifikaları oluşturun.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: cherylmc
ms.openlocfilehash: c1c69b301199b054fe6b1ef42cfcf7878a7a161c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91306694"
---
# <a name="generate-and-export-certificates-for-point-to-site-using-powershell"></a>PowerShell kullanarak noktadan siteye bağlantılar için sertifika oluşturma ve dışarı aktarma

Noktadan siteye bağlantılar kimlik doğrulaması için sertifikaları kullanır. Bu makalede, otomatik olarak imzalanan bir kök sertifika oluşturma ve Windows 10 veya Windows Server 2016 ' de PowerShell kullanarak istemci sertifikaları oluşturma işlemlerinin nasıl yapılacağı gösterilir. Farklı sertifika yönergeleri arıyorsanız bkz. [Certificates-Linux](vpn-gateway-certificates-point-to-site-linux.md) veya [Certificates-MakeCert](vpn-gateway-certificates-point-to-site-makecert.md).

Bu makaledeki adımları Windows 10 veya Windows Server 2016 çalıştıran bir bilgisayarda gerçekleştirmeniz gerekir. Sertifika oluşturmak için kullandığınız PowerShell cmdlet 'leri işletim sisteminin bir parçasıdır ve Windows 'un diğer sürümlerinde çalışmaz. Windows 10 veya Windows Server 2016 bilgisayarı yalnızca sertifikaları oluşturmak için gereklidir. Sertifikalar oluşturulduktan sonra, bunları karşıya yükleyebilir veya desteklenen herhangi bir istemci işletim sistemine yükleyebilirsiniz.

Bir Windows 10 veya Windows Server 2016 bilgisayarına erişiminiz yoksa, sertifika oluşturmak için [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) kullanabilirsiniz. Her iki yöntemi kullanarak oluşturduğunuz Sertifikalar [desteklenen](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) herhangi bir istemci işletim sistemine yüklenebilir.

[!INCLUDE [generate and export certificates](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="install-an-exported-client-certificate"></a><a name="install"></a>Dışarı aktarılan bir istemci sertifikasını yükleme

VNet 'e bir P2S bağlantısı üzerinden bağlanan her istemci, istemci sertifikasının yerel olarak yüklenmesini gerektirir.

İstemci sertifikası yüklemek için bkz. [Noktadan siteye bağlantılar için istemci sertifikası yüklemesi](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye yapılandırma ile devam edin.

* **Kaynak Yöneticisi** dağıtım modeli adımları için bkz. [Yerel Azure sertifikası KIMLIK doğrulaması kullanarak P2S yapılandırma](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* **Klasik** dağıtım modeli adımları için bkz. [VNET 'e Noktadan siteye VPN bağlantısı yapılandırma (klasik)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).