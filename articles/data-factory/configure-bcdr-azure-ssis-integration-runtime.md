---
title: SQL veritabanı yük devretmesi için Azure-SSIS tümleştirme çalışma zamanını yapılandırma
description: Bu makalede, SSıSDB veritabanı için Azure SQL veritabanı coğrafi çoğaltma ve yük devretme ile Azure-SSIS tümleştirme çalışma zamanının nasıl yapılandırılacağı açıklanır.
ms.service: data-factory
ms.devlang: powershell
author: swinarko
ms.author: sawinark
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/06/2020
ms.openlocfilehash: e12939d1003ce708889ca0b3dbc710096f9ee955
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100364452"
---
# <a name="configure-the-azure-ssis-integration-runtime-with-sql-database-geo-replication-and-failover"></a>Azure-SSIS tümleştirme çalışma zamanını SQL veritabanı coğrafi çoğaltma ve yük devretme ile yapılandırma

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, SSıSDB veritabanı için Azure SQL veritabanı coğrafi çoğaltma ile Azure-SSIS tümleştirme çalışma zamanının (IR) nasıl yapılandırılacağı açıklanır. Yük devretme gerçekleştiğinde, Azure-SSIS IR ikincil veritabanıyla çalışmayı sürdürdüğünden emin olabilirsiniz.

SQL veritabanı için coğrafi çoğaltma ve yük devretme hakkında daha fazla bilgi için bkz. [genel bakış: etkin coğrafi çoğaltma ve otomatik yük devretme grupları](../azure-sql/database/auto-failover-group-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-ssis-ir-failover-with-a-sql-managed-instance"></a>SQL yönetilen örneği ile yük devretmeyi Azure-SSIS IR

### <a name="prerequisites"></a>Önkoşullar

Azure SQL yönetilen örneği, veri, kimlik bilgileri ve bir veritabanında depolanan bağlantı bilgilerinin güvenliğinin sağlanmasına yardımcı olmak için bir *veritabanı ana anahtarı (DMK)* kullanır. DMK otomatik şifre çözmeyi etkinleştirmek için, anahtarın bir kopyası *sunucu ana anahtarı (SMK)* ile şifrelenir. 

SMK bir yük devretme grubunda çoğaltılmaz. Yük devretmenin ardından DMK şifre çözme için hem birincil hem de ikincil örneklere bir parola eklemeniz gerekir.

1. Birincil örnekte SSıSDB için aşağıdaki komutu çalıştırın. Bu adım yeni bir şifreleme parolası ekler.

   ```sql
   ALTER MASTER KEY ADD ENCRYPTION BY PASSWORD = 'password'
   ```

2. SQL yönetilen örneği üzerinde bir yük devretme grubu oluşturun.

3. Yeni şifreleme parolasını kullanarak ikincil örnekte **sp_control_dbmasterkey_password** çalıştırın.

   ```sql
   EXEC sp_control_dbmasterkey_password @db_name = N'SSISDB', @password = N'<password>', @action = N'add';  
   GO
   ```

### <a name="scenario-1-azure-ssis-ir-is-pointing-to-a-readwrite-listener-endpoint"></a>Senaryo 1: Azure-SSIS IR okuma/yazma dinleyicisi uç noktasını işaret ediyor

Azure-SSIS IR bir okuma/yazma dinleyicisi uç noktasına işaret etmek istiyorsanız, öncelikle birincil sunucu uç noktasını işaret etmeniz gerekir. SSıSDB 'yi bir yük devretme grubuna geçirdikten sonra, Azure-SSIS IR Azure PowerShell kullanarak okuma/yazma dinleyicisi uç noktasına işaret etmek üzere değiştirebilir ve yeniden başlatabilirsiniz.

```powershell
Set-AzDataFactoryV2IntegrationRuntime -CatalogServerEndpoint "Azure SQL Managed Instance read/write listener endpoint"
```

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde, aşağıdaki adımları uygulayın:

1. Birincil bölgedeki Azure-SSIS IR durdurun.

2. İkincil örnekteki özel kurulum için yeni bölge, sanal ağ ve paylaşılan erişim imzası (SAS) URI bilgileriyle Azure-SSIS IR düzenleyin. Azure-SSIS IR bir okuma/yazma dinleyicisine işaret ettiğinden ve uç nokta Azure-SSIS IR saydam olduğundan, uç noktayı düzenlemeniz gerekmez.

   ```powershell
   Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
      -VNetId "new VNet" `
      -Subnet "new subnet" `
      -SetupScriptContainerSasUri "new custom setup SAS URI"
   ```

3. Azure-SSIS IR yeniden başlatın.

### <a name="scenario-2-azure-ssis-ir-is-pointing-to-a-primary-server-endpoint"></a>Senaryo 2: Azure-SSIS IR birincil sunucu uç noktasını işaret ediyor

Bu senaryo, Azure-SSIS IR birincil sunucu uç noktasını işaret ettiğinden uygundur.

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde, aşağıdaki adımları uygulayın:

1. Birincil bölgedeki Azure-SSIS IR durdurun.

2. İkincil örnek için yeni bölge, uç nokta ve sanal ağ bilgileriyle Azure-SSIS IR düzenleyin.

   ```powershell
   Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
      -CatalogServerEndpoint "Azure SQL Database endpoint" `
      -CatalogAdminCredential "Azure SQL Database admin credentials" `
      -VNetId "new VNet" `
      -Subnet "new subnet" `
      -SetupScriptContainerSasUri "new custom setup SAS URI"
   ```

3. Azure-SSIS IR yeniden başlatın.

### <a name="scenario-3-azure-ssis-ir-is-pointing-to-a-public-endpoint-of-a-sql-managed-instance"></a>Senaryo 3: Azure-SSIS IR SQL yönetilen örneğinin genel uç noktasını işaret ediyor

Bu senaryo, Azure-SSIS IR bir Azure SQL yönetilen örneğinin genel uç noktasını işaret ettiğinden ve sanal ağa katılamazsa uygundur. Senaryo 2 ' den tek fark, yük devretmeden sonra Azure-SSIS IR için sanal ağ bilgilerini düzenlemenize gerek kalmaz.

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde, aşağıdaki adımları uygulayın:

1. Birincil bölgedeki Azure-SSIS IR durdurun.

2. Azure-SSIS IR ikincil örnek için yeni bölge ve uç nokta bilgileriyle düzenleyin.

   ```powershell
   Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
      -CatalogServerEndpoint "Azure SQL Database server endpoint" `
      -CatalogAdminCredential "Azure SQL Database server admin credentials" `
      -SetupScriptContainerSasUri "new custom setup SAS URI"
   ```

3. Azure-SSIS IR yeniden başlatın.

### <a name="scenario-4-attach-an-existing-ssisdb-instance-ssis-catalog-to-a-new-azure-ssis-ir"></a>Senaryo 4: var olan bir SSıSDB örneğini (SSIS Kataloğu) yeni bir Azure-SSIS IR Iliştirme

Bu senaryo, geçerli bölgede Azure Data Factory veya Azure-SSIS IR olağanüstü bir durum oluştuğunda SSSıSDB 'nin yeni bir bölgede yeni bir Azure-SSIS IR çalışmasını istiyorsanız uygundur.

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde, aşağıdaki adımları uygulayın.

> [!NOTE]
> 4. adım (IR oluşturma) için PowerShell 'i kullanın. Bunu yapmazsanız, Azure portal SSıSDB zaten var olduğunu belirten bir hata rapor eder.

1. Birincil bölgedeki Azure-SSIS IR durdurun.

2. Ve ' den gelen bağlantıları kabul etmek için SSSıSDB içindeki meta verileri güncelleştirmek için bir saklı yordam çalıştırın **\<new_data_factory_name\>** **\<new_integration_runtime_name\>** .
   
   ```sql
   EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
   ```

3. Yeni bölgede adlı yeni bir veri fabrikası oluşturun **\<new_data_factory_name\>** .

   ```powershell
   Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
      -Location "new region"`
      -Name "<new_data_factory_name>"
   ```
   
   Bu PowerShell komutu hakkında daha fazla bilgi için bkz. [PowerShell kullanarak Azure Veri Fabrikası oluşturma](quickstart-create-data-factory-powershell.md).

4. Azure PowerShell kullanarak yeni bölgede adlı yeni bir Azure-SSIS IR oluşturun **\<new_integration_runtime_name\>** .

   ```powershell
   Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "new resource group name" `
      -DataFactoryName "new data factory name" `
      -Name "<new_integration_runtime_name>" `
      -Description $AzureSSISDescription `
      -Type Managed `
      -Location $AzureSSISLocation `
      -NodeSize $AzureSSISNodeSize `
      -NodeCount $AzureSSISNodeNumber `
      -Edition $AzureSSISEdition `
      -LicenseType $AzureSSISLicenseType `
      -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
      -VnetId "new vnet" `
      -Subnet "new subnet" `
      -CatalogServerEndpoint $SSISDBServerEndpoint `
      -CatalogPricingTier $SSISDBPricingTier
   ```
   
   Bu PowerShell komutu hakkında daha fazla bilgi için, bkz. [Azure Data Factory Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md).

## <a name="azure-ssis-ir-failover-with-sql-database"></a>SQL veritabanı ile yük devretmeyi Azure-SSIS IR

### <a name="scenario-1-azure-ssis-ir-is-pointing-to-a-readwrite-listener-endpoint"></a>Senaryo 1: Azure-SSIS IR okuma/yazma dinleyicisi uç noktasını işaret ediyor

Bu senaryo şu durumlarda uygundur:

- Azure-SSIS IR, yük devretme grubunun okuma/yazma dinleyicisi uç noktasını işaret eder.
- SQL veritabanı sunucusu, sanal ağ hizmeti uç noktası *kuralıyla yapılandırılmadı.*

Azure-SSIS IR bir okuma/yazma dinleyicisi uç noktasına işaret etmek istiyorsanız, öncelikle birincil sunucu uç noktasını işaret etmeniz gerekir. SSıSDB 'yi bir yük devretme grubuna geçirdikten sonra, Azure-SSIS IR Azure PowerShell kullanarak okuma/yazma dinleyicisi uç noktasına işaret etmek üzere değiştirebilir ve yeniden başlatabilirsiniz.

```powershell
Set-AzDataFactoryV2IntegrationRuntime -CatalogServerEndpoint "Azure SQL Database read/write listener endpoint"
```

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde, Azure-SSIS IR saydam olur. Azure-SSIS IR, yük devretme grubunun yeni birincili otomatik olarak bağlanır. 

Azure-SSIS IR bölgeyi veya diğer bilgileri güncelleştirmek istiyorsanız, bunu durdurabilir, düzenleyebilir ve yeniden başlatabilirsiniz.


### <a name="scenario-2-azure-ssis-ir-is-pointing-to-a-primary-server-endpoint"></a>Senaryo 2: Azure-SSIS IR birincil sunucu uç noktasını işaret ediyor

Bu senaryo, Azure-SSIS IR birincil sunucu uç noktasını işaret ettiğinden uygundur.

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde, aşağıdaki adımları uygulayın:

1. Birincil bölgedeki Azure-SSIS IR durdurun.

2. İkincil örnek için yeni bölge, uç nokta ve sanal ağ bilgileriyle Azure-SSIS IR düzenleyin.

   ```powershell
   Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
      -CatalogServerEndpoint "Azure SQL Database endpoint" `
      -CatalogAdminCredential "Azure SQL Database admin credentials" `
      -VNetId "new VNet" `
      -Subnet "new subnet" `
      -SetupScriptContainerSasUri "new custom setup SAS URI"
   ```

3. Azure-SSIS IR yeniden başlatın.

### <a name="scenario-3-attach-an-existing-ssisdb-ssis-catalog-to-a-new-azure-ssis-ir"></a>Senaryo 3: var olan bir SSıSDB (SSIS Kataloğu) yeni bir Azure-SSIS IR Iliştirme

Bu senaryo, ikincil bölgede yeni bir Azure-SSIS IR sağlamak istiyorsanız uygundur. Ayrıca, geçerli bölgede bir Azure Data Factory veya Azure-SSIS IR olağanüstü durum oluştuğunda SSıSDB 'nizin yeni bir bölgede yeni bir Azure-SSIS IR çalışmaya devam etmesini istiyorsanız de uygundur.

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde, aşağıdaki adımları uygulayın.

> [!NOTE]
> 4. adım (IR oluşturma) için PowerShell 'i kullanın. Bunu yapmazsanız, Azure portal SSıSDB zaten var olduğunu belirten bir hata rapor eder.

1. Birincil bölgedeki Azure-SSIS IR durdurun.

2. Ve ' den gelen bağlantıları kabul etmek için SSSıSDB içindeki meta verileri güncelleştirmek için bir saklı yordam çalıştırın **\<new_data_factory_name\>** **\<new_integration_runtime_name\>** .
   
   ```sql
   EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
   ```

3. Yeni bölgede adlı yeni bir veri fabrikası oluşturun **\<new_data_factory_name\>** .

   ```powershell
   Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
      -Location "new region"`
      -Name "<new_data_factory_name>"
   ```
   
   Bu PowerShell komutu hakkında daha fazla bilgi için bkz. [PowerShell kullanarak Azure Veri Fabrikası oluşturma](quickstart-create-data-factory-powershell.md).

4. Azure PowerShell kullanarak yeni bölgede adlı yeni bir Azure-SSIS IR oluşturun **\<new_integration_runtime_name\>** .

   ```powershell
   Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "new resource group name" `
      -DataFactoryName "new data factory name" `
      -Name "<new_integration_runtime_name>" `
      -Description $AzureSSISDescription `
      -Type Managed `
      -Location $AzureSSISLocation `
      -NodeSize $AzureSSISNodeSize `
      -NodeCount $AzureSSISNodeNumber `
      -Edition $AzureSSISEdition `
      -LicenseType $AzureSSISLicenseType `
      -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
      -VnetId "new vnet" `
      -Subnet "new subnet" `
      -CatalogServerEndpoint $SSISDBServerEndpoint `
      -CatalogPricingTier $SSISDBPricingTier
   ```

   Bu PowerShell komutu hakkında daha fazla bilgi için, bkz. [Azure Data Factory Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure-SSIS IR için diğer yapılandırma seçeneklerini göz önünde bulundurun:

- [Azure-SSIS tümleştirme çalışma zamanını yüksek performans için yapılandırın](configure-azure-ssis-integration-runtime-performance.md)

- [Azure-SSIS Integration Runtime için kurulumu özelleştirme](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Azure-SSIS tümleştirme çalışma zamanı için Enterprise Edition sağlama](how-to-configure-azure-ssis-ir-enterprise-edition.md)
