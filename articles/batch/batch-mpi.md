---
title: MPı uygulamalarını çalıştırmak için çok örnekli görevleri kullanma
description: Ileti geçirme arabirimi (MPı) uygulamalarını Azure Batch ' deki çok örnekli görev türünü kullanarak yürütmeyi öğrenin.
ms.topic: how-to
ms.date: 10/08/2020
ms.custom: H1Hack27Feb2017, devx-track-csharp
ms.openlocfilehash: 6aa6a910dd57a255d9ec9292119bc692edf4946f
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96351529"
---
# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-batch"></a>Batch 'de Ileti geçirme arabirimi (MPı) uygulamalarını çalıştırmak için çok örnekli görevleri kullanma

Çok örnekli görevler birden çok işlem düğümünde aynı anda bir Azure Batch görevi çalıştırmanızı sağlar. Bu görevler, toplu Işteki Ileti geçirme arabirimi (MPı) uygulamaları gibi yüksek performanslı bilgi işlem senaryolarına olanak tanır. Bu makalede [Batch .net][api_net] kitaplığını kullanarak çok örnekli görevleri yürütmeyi öğreneceksiniz.

> [!NOTE]
> Bu makaledeki örneklerde Batch .NET, MS-MPI ve Windows işlem düğümlerine odaklanılmış olsa da, burada ele alınan çok örnekli görev kavramları diğer platformlar ve teknolojiler için geçerlidir (örneğin, Python ve Intel MPı Linux düğümlerinde).
>
>

## <a name="multi-instance-task-overview"></a>Çok örnekli göreve genel bakış
Batch 'de, her görev normalde tek bir işlem düğümünde yürütülür. bir işe birden çok görev gönderirseniz ve Batch hizmeti her görevi bir düğümde yürütmeye göre zamanlar. Ancak, bir görevin **Çoklu örnek ayarlarını** yapılandırarak, toplu olarak tek bir birincil görev ve daha sonra birden çok düğümde yürütülen birkaç alt görev oluşturmayı öğreneceksiniz.

![Çok örnekli göreve genel bakış][1]

Bir iş için çok örnekli ayarlarla bir görev gönderdiğinizde toplu Işlem, çok örnekli görevlere özgü birkaç adımı gerçekleştirir:

1. Batch hizmeti, çoklu örnek ayarlarına bağlı olarak bir **birincil** ve birkaç **alt görev** oluşturur. Toplam görev sayısı (birincil ve tüm alt görevler), çok örnekli ayarlarda belirttiğiniz **örnek** sayısıyla (işlem düğümleri) eşleşir.
2. Batch, işlem düğümlerinden birini **ana öğe** olarak belirler ve birincil görevi ana bilgisayarda yürütülecek şekilde zamanlar. Alt görevleri, çok örnekli göreve ayrılan işlem düğümlerinin geri kalanı üzerinde yürütülecek şekilde zamanlar, düğüm başına bir alt görev.
3. Birincil ve tüm alt görevler, çok örnekli ayarlarda belirttiğiniz **ortak kaynak dosyalarını** indirir.
4. Ortak kaynak dosyaları indirildikten sonra, birincil ve alt görevler, çoklu örnek ayarlarında belirttiğiniz **koordinasyon komutunu** yürütür. Düzenleme komutu genellikle, görevi yürütmek için düğüm hazırlamak üzere kullanılır. Bu, arka plan hizmetlerinin (örneğin, [Microsoft MPI][msmpi_msdn]) başlamasını `smpd.exe` ve düğümlerin düğümler arası iletileri işlemeye hazırlandığının doğrulanması olabilir.
5. Birincil görev, düzenleme komutu birincil ve tüm alt görevler tarafından başarıyla tamamlandıktan *sonra* ana düğümde **uygulama komutunu** yürütür. Uygulama komutu, çok örnekli görevin kendisi için komut satırı olur ve yalnızca birincil görev tarafından yürütülür. [MS-MPı][msmpi_msdn]tabanlı bir çözümde, bu, kullanarak MPI özellikli uygulamanızı yürütebileceğiniz yerdir `mpiexec.exe` .

> [!NOTE]
> İşlevsel farklı olsa da, "Çoklu örnek görevi" [startTask][net_starttask] veya [JobPreparationTask][net_jobprep]gibi benzersiz bir görev türü değildir. Çok örnekli görev, çok örnekli ayarları yapılandırılmış standart bir Batch görevi (Batch .NET 'te[cloudtask][net_task] ) olur. Bu makalede, buna **Çoklu örnek görevi** olarak değineceğiz.
>
>

## <a name="requirements-for-multi-instance-tasks"></a>Çok örnekli görevler için gereksinimler
Çok örnekli görevler, **düğüm içi iletişimin etkin** olduğu bir havuz gerektirir ve **eşzamanlı görev yürütme devre dışı bırakılır**. Eşzamanlı görev yürütmeyi devre dışı bırakmak için [Cloudpool. TaskSlotsPerNode](/dotnet/api/microsoft.azure.batch.cloudpool) özelliğini 1 olarak ayarlayın.

> [!NOTE]
> Batch, düğüm içi iletişim etkin olan bir havuzun boyutunu [sınırlar](batch-quota-limit.md#pool-size-limits) .


Bu kod parçacığı, Batch .NET kitaplığı kullanılarak çok örnekli görevler için bir havuzun nasıl oluşturulacağını gösterir.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.TaskSlotsPerNode = 1;
```

> [!NOTE]
> Internode iletişimi devre dışı bırakılmış bir havuzda çok örnekli bir görevi çalıştırmayı denerseniz veya 1 ' den büyük bir *Taskslotspernode* değeri varsa, görev hiçbir şekilde zamanlanmadı; "etkin" durumunda süresiz olarak kalır.


### <a name="use-a-starttask-to-install-mpi"></a>MPı 'yi yüklemek için bir StartTask kullanın
MPı uygulamalarını çok örnekli bir görevle çalıştırmak için, önce havuzdaki işlem düğümlerine bir MPı uygulaması (örneğin, MS-MPı veya Intel MPı) yüklemeniz gerekir. Bu, bir düğüm bir havuza katıldığında veya yeniden başlatıldığında yürütülen bir [startTask][net_starttask]kullanmak için iyi bir zamandır. Bu kod parçacığı, bir [kaynak dosyası][net_resourcefile]olarak MS-MPI kurulum paketini belirten bir startTask oluşturur. Başlangıç görevinin komut satırı, kaynak dosyası düğüme indirildikten sonra yürütülür. Bu durumda, komut satırı MS-MPı 'nin katılımsız bir yüklemesini gerçekleştirir.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Doğrudan uzak bellek erişimi (RDMA)
Batch havuzunuzdaki işlem düğümleri için A9 gibi bir [RDMA özellikli bir boyut](../virtual-machines/sizes-hpc.md?toc=/azure/virtual-machines/windows/toc.json) seçtiğinizde, MPI uygulamanız Azure 'un yüksek performanslı, düşük gecikmeli uzak doğrudan bellek ERIŞIMI (RDMA) ağından faydalanabilir.

Aşağıdaki makalelerde "RDMA özellikli" olarak belirtilen boyutları arayın:

* **Cloudserviceconfiguration** havuzları

  * [Cloud Services Için boyutlar](../cloud-services/cloud-services-sizes-specs.md) (yalnızca Windows)
* **Virtualmachineconfiguration** havuzları

  * [Azure 'daki sanal makinelerin boyutları](../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)
  * [Azure 'daki sanal makinelerin boyutları](../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)

> [!NOTE]
> [Linux işlem DÜĞÜMLERINDE](batch-linux-nodes.md)RDMA 'nin avantajlarından yararlanmak Için düğümlerde **Intel MPI** kullanmanız gerekir.
>

## <a name="create-a-multi-instance-task-with-batch-net"></a>Batch .NET ile çok örnekli bir görev oluşturma
Artık havuz gereksinimlerini ve MPı paketi yüklemesini kapsadığımıza göre, çok örnekli görevi oluşturalım. Bu kod parçacığında, bir standart [Cloudtask][net_task]oluşturup [multiınstancesettings][net_multiinstance_prop] özelliğini yapılandıracağız. Daha önce belirtildiği gibi, çok örnekli görev ayrı bir görev türü değildir, ancak çok örnekli ayarlarla yapılandırılmış standart bir Batch görevi.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Birincil görev ve alt görevler
Bir görev için çoklu örnek ayarlarını oluştururken, görevi yürütecek olan işlem düğümlerinin sayısını belirtirsiniz. Görevi bir işe gönderdiğinizde, Batch hizmeti bir **birincil** görev ve belirttiğiniz düğüm sayısıyla eşleşen çok sayıda **alt** iş oluşturur.

Bu görevlere 0- *Numberofınstances* -1 aralığında bir tamsayı kimliği atanır. Kimliği 0 olan görev birincil görevdir ve diğer tüm kimlikler alt görevlerdir. Örneğin, bir görev için aşağıdaki çok örnekli ayarları oluşturursanız, birincil görevin kimliği 0 olur ve alt görevler 1 ile 9 arasında kimlik içermelidir.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Ana düğüm
Çok örnekli bir görevi gönderdiğinizde, Batch hizmeti işlem düğümlerinden birini "ana" düğüm olarak belirler ve birincil görevi ana düğümde yürütülecek şekilde zamanlar. Alt görevler, çok örnekli göreve ayrılan düğümlerin geri kalanı üzerinde yürütülecek şekilde zamanlanır.

## <a name="coordination-command"></a>Koordinasyon komutu
**Koordinasyon komutu** hem birincil hem de alt görevler tarafından yürütülür.

Koordinasyon komutunun çağrılması,--düzenleme komutu tüm alt görevler için başarılı bir şekilde döndürülünceye kadar uygulama komutunu çalıştırmaz. Koordinasyon komutu bu nedenle gerekli arka plan hizmetlerini başlatıp, kullanıma hazırdıklarından emin olun ve ardından çıkış yapın. Örneğin, MS-MPI sürüm 7 kullanan bir çözüm için bu düzenleme komutu, düğümdeki SMPD hizmetini başlatır ve çıkar:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Bu koordinasyon komutunda kullanılması gerektiğini aklınızda edin `start` . `smpd.exe`Uygulama yürütmeden hemen sonra dönmediğinden bu gereklidir. [Start][cmd_start] komutunun kullanımı olmadan, bu koordinasyon komutu dönmez ve bu nedenle uygulama komutunun çalışmasını engeller.

## <a name="application-command"></a>Uygulama komutu
Birincil görev ve tüm alt görevler koordinasyon komutunu yürütmeyi tamamladığında, çok örnekli görevin komut satırı *yalnızca* birincil görev tarafından yürütülür. Bu **uygulama komutunu** , koordinasyon komutundan ayırt etmek için çağırıyoruz.

MS-MPı uygulamalarında, MPı özellikli uygulamanızı ile yürütmek için uygulama komutunu kullanın `mpiexec.exe` . Örneğin, MS-MPı sürüm 7 kullanan bir çözüm için uygulama komutu aşağıda verilmiştir:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> MS-MPı `mpiexec.exe` `CCP_NODES` Varsayılan olarak değişkenini kullandığından (bkz. [ortam değişkenleri](#environment-variables)) Yukarıdaki örnek uygulama komut satırı hariç tutar.
>
>

## <a name="environment-variables"></a>Ortam değişkenleri
Batch, çok örnekli bir göreve ayrılan işlem düğümlerinde çok örnekli görevlere özgü birkaç [ortam değişkeni][msdn_env_var] oluşturur. Koordinasyon ve uygulama komut hatlarınız, yürüttikleri betikler ve programlar gibi bu ortam değişkenlerine başvurabilir.

Aşağıdaki ortam değişkenleri, Batch hizmeti tarafından çok örnekli görevler tarafından kullanılmak üzere oluşturulur:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Bunlarla ilgili tüm ayrıntılar ve bunların içerikleri ve görünürlüğü dahil diğer toplu işlem düğümü ortam değişkenleri için bkz. [işlem düğümü ortam değişkenleri][msdn_env_var].

> [!TIP]
> Batch Linux MPı kodu örneği, bu ortam değişkenlerinin kaç tane kullanıldığına ilişkin bir örnek içerir.

## <a name="resource-files"></a>Kaynak dosyalar
Çok örnekli görevler için göz önünde bulundurmanız gereken iki kaynak dosyası kümesi vardır: *Tüm* görevlerin indirdiği **ortak kaynak dosyaları** (birincil ve alt görevler) ve *yalnızca birincil* görevin indirileceği çok örnekli görevin kendisi için belirtilen **kaynak dosyaları** .

Bir görev için çoklu örnek ayarlarında bir veya daha fazla **ortak kaynak dosyası** belirtebilirsiniz. Bu ortak kaynak dosyaları, [Azure depolama](../storage/common/storage-introduction.md) 'dan her bir düğümün **görev paylaşılan dizinine** birincil ve tüm alt görevler tarafından indirilir. Görev paylaşılan dizinine, ortam değişkenini kullanarak, uygulama ve düzenleme komut satırlarından erişebilirsiniz `AZ_BATCH_TASK_SHARED_DIR` . `AZ_BATCH_TASK_SHARED_DIR`Yol, çok örnekli göreve ayrılan her düğümde aynıdır, bu sayede birincil ve tüm alt görevler arasında tek bir düzenleme komutu paylaşabilirsiniz. Batch, dizini bir uzaktan erişim açısından "paylaşmaz", ancak ortam değişkenlerinde ipucu içinde daha önce bahsedildiği gibi bir bağlama veya paylaşma noktası olarak kullanabilirsiniz.

Çoklu örnek görevi için belirttiğiniz kaynak dosyaları, varsayılan olarak görevin çalışma dizinine indirilir `AZ_BATCH_TASK_WORKING_DIR` . Belirtildiği gibi, yaygın kaynak dosyalarının aksine yalnızca birincil görev, çok örnekli görevin kendisi için belirtilen kaynak dosyalarını indirir.

> [!IMPORTANT]
> Her zaman ortam değişkenlerini kullanın `AZ_BATCH_TASK_SHARED_DIR` ve `AZ_BATCH_TASK_WORKING_DIR` komut satırlarında bu dizinlere başvurun. Yolları el ile oluşturmaya çalışmayın.
>
>

## <a name="task-lifetime"></a>Görev ömrü
Birincil görevin ömrü, tüm çok örnekli görevin ömrünü denetler. Birincil çıkış yapıldığında tüm alt görevler sonlandırılır. Birincil çıkış kodu, görevin çıkış kodudur ve bu nedenle, yeniden deneme amacıyla görevin başarısını veya başarısızlığını belirlemede kullanılır.

Alt görevlerden herhangi biri başarısız olursa, sıfır olmayan bir dönüş koduyla çıkılıyor, örneğin, tüm çok örnekli görev başarısız olur. Daha sonra, yeniden deneme sınırına kadar çok örnekli görev sonlandırılır ve yeniden denenir.

Çok örnekli bir görevi sildiğinizde, birincil ve tüm alt görevler de Batch hizmeti tarafından silinir. Tüm alt görev dizinleri ve dosyaları, standart bir görevde olduğu gibi işlem düğümlerinden silinir.

[Maxtaskretrycount][net_taskconstraint_maxretry], [maxduvar Clocktime][net_taskconstraint_maxwallclock]ve [RetentionTime][net_taskconstraint_retention] özellikleri gibi çok örnekli bir görevin [taskkýsýtlamalarından][net_taskconstraints] bazıları standart bir görevde olduğu gibi kabul edilir ve birincil ve tüm alt görevlere uygulanır. Ancak, iş için çoklu örnek görevi ekledikten sonra [RetentionTime][net_taskconstraint_retention] özelliğini değiştirirseniz, bu değişiklik yalnızca birincil göreve uygulanır. Tüm alt görevler özgün [RetentionTime][net_taskconstraint_retention]kullanmaya devam eder.

İşlem düğümünün son görev listesi, son görev çok örnekli bir görevin parçasıysa bir alt görevin kimliğini yansıtır.

## <a name="obtain-information-about-subtasks"></a>Alt görevler hakkında bilgi edinin
Batch .NET kitaplığı 'nı kullanarak alt görevler hakkında bilgi edinmek için [Cloudtask. Listalt görevler][net_task_listsubtasks] yöntemini çağırın. Bu yöntem, tüm alt görevlerle ilgili bilgileri ve görevleri yürüten işlem düğümü hakkındaki bilgileri döndürür. Bu bilgilerden, her alt görevin kök dizinini, havuz kimliğini, geçerli durumunu, çıkış kodunu ve daha fazlasını belirleyebilirsiniz. Bu bilgileri, alt görevin dosyalarını almak için [Pooloperations. GetNodeFile][poolops_getnodefile] yöntemiyle birlikte kullanabilirsiniz. Bu yöntemin birincil görev (kimlik 0) için bilgi döndürmediğini unutmayın.

> [!NOTE]
> Aksi belirtilmediği takdirde, çok örnekli [Cloudtask][net_task] üzerinde çalışan Batch .net metotları *yalnızca* birincil görev için geçerlidir. Örneğin, [cloudtask. ListNodeFiles][net_task_listnodefiles] yöntemini çok örnekli bir görevde çağırdığınızda yalnızca birincil görevin dosyaları döndürülür.
>
>

Aşağıdaki kod parçacığı, alt görev bilgilerinin nasıl alınacağını ve yürütüldüğü düğümlerin dosya içeriğini nasıl isteyeceğini gösterir.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == SubtaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Kod örneği
GitHub 'daki [Multiınstancetasks][github_mpi] kod örneği, toplu işlem düğümlerinde bir [MS-MPI][msmpi_msdn] uygulamasını çalıştırmak için çok örnekli bir görevin nasıl kullanılacağını gösterir. Örneği çalıştırmak için [hazırlama](#preparation) ve [yürütme](#execution) bölümündeki adımları izleyin.

### <a name="preparation"></a>Hazırlık
1. [Basit BIR MS-MPı programını derlemek ve çalıştırmak için][msmpi_howto]ilk iki adımı izleyin. Bu, aşağıdaki adım için önkoşulları karşılar.
2. [Mpihelloworld][helloworld_proj] örnek MPI programının *yayın* sürümünü oluşturun. Bu, işlem düğümlerinde çoklu örnek görevi tarafından çalıştırılacak olan programdır.
3. `MPIHelloWorld.exe`(Adım 2 ' i yapılandırdığınız) ve `MSMpiSetup.exe` (1. adımı indirdiğiniz) içeren bir ZIP dosyası oluşturun. Sonraki adımda bu zip dosyasını bir uygulama paketi olarak karşıya yüklersiniz.
4. "MPIHelloWorld" adlı bir Batch [uygulaması](batch-application-packages.md) oluşturmak için [Azure Portal][portal] kullanın ve önceki adımda oluşturduğunuz zip dosyasını uygulama paketinin "1,0" sürümü olarak belirtin. Daha fazla bilgi için bkz. [uygulamaları karşıya yükleme ve yönetme](batch-application-packages.md#upload-and-manage-applications) .

> [!TIP]
> *Release* `MPIHelloWorld.exe` Uygulama paketinize ek bağımlılıklar (örneğin, veya) eklemeniz gerekmiyorsa, bir yayın sürümü oluşturun `msvcp140d.dll` `vcruntime140d.dll` .
>
>

### <a name="execution"></a>Yürütme
1. GitHub 'dan [Azure-Batch-Samples][github_samples_zip] ' i indirin.
2. Visual Studio 2019 ' de Multiınstancetasks **çözümünü** açın. `MultiInstanceTasks.sln`Çözüm dosyası şu konumda bulunur:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. `AccountSettings.settings` **Microsoft.Azure.Batch. Samples. Common** projesinde toplu işlem ve depolama hesabı kimlik bilgilerinizi girin.
4. Bir Batch havuzundaki işlem düğümlerinde MPı örnek uygulamasını yürütmek için Multiınstancetasks çözümünü **derleyin ve çalıştırın** .
5. *Isteğe bağlı*: kaynakları silmeden önce örnek havuzunu, işi ve görevi ("Multiınstancesamplepool", "Multiınstancesamplejob", "Multiınstancesampletask") incelemek için [Azure Portal][portal] veya [Batch Explorer][batch_labs] kullanın.

> [!TIP]
> Visual Studio [Community][visual_studio] 'yi ücretsiz olarak indirebilirsiniz.
>
>

Çıkış `MultiInstanceTasks.exe` , aşağıdakine benzer:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Sonraki adımlar
* Microsoft HPC & Azure Batch ekip blogu, [Azure Batch Linux Için MPI desteğini][blog_mpi_linux]ele alır ve Batch Ile [openfom][openfoam] kullanımı hakkında bilgiler içerir. [GitHub 'Da Openfohar örneği][github_mpi]için Python kod örneklerini bulabilirsiniz.
* Azure Batch MPı çözümlerinizde kullanılmak üzere [Linux işlem düğümü havuzları oluşturmayı](batch-linux-nodes.md) öğrenin.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: /dotnet/api/microsoft.azure.batch
[api_rest]: /rest/api/batchservice/
[batch_labs]: https://azure.github.io/BatchExplorer/
[blog_mpi_linux]: /archive/blogs/windowshpc/introducing-mpi-support-for-linux-on-azure-batch
[cmd_start]: /previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc770297(v=ws.11)
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: ./batch-compute-node-environment-variables.md
[msmpi_msdn]: /message-passing-interface/microsoft-mpi
[msmpi_sdk]: https://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: /archive/blogs/windowshpc/how-to-compile-and-run-a-simple-ms-mpi-program
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: /dotnet/api/microsoft.azure.batch.jobpreparationtask
[net_multiinstance_class]: /dotnet/api/microsoft.azure.batch.multiinstancesettings
[net_multiinstance_prop]: /dotnet/api/microsoft.azure.batch.cloudtask
[net_multiinsance_commonresfiles]: /dotnet/api/microsoft.azure.batch.multiinstancesettings
[net_multiinstance_coordcmdline]: /dotnet/api/microsoft.azure.batch.multiinstancesettings
[net_multiinstance_numinstances]: /dotnet/api/microsoft.azure.batch.multiinstancesettings
[net_pool]: /dotnet/api/microsoft.azure.batch.cloudpool
[net_pool_create]: /dotnet/api/microsoft.azure.batch.pooloperations
[net_pool_starttask]: /dotnet/api/microsoft.azure.batch.cloudpool
[net_resourcefile]: /dotnet/api/microsoft.azure.batch.resourcefile
[net_starttask]: /dotnet/api/microsoft.azure.batch.starttask
[net_starttask_cmdline]: /dotnet/api/microsoft.azure.batch.starttask
[net_task]: /dotnet/api/microsoft.azure.batch.cloudtask
[net_taskconstraints]: /dotnet/api/microsoft.azure.batch.taskconstraints
[net_taskconstraint_maxretry]: /dotnet/api/microsoft.azure.batch.taskconstraints
[net_taskconstraint_maxwallclock]: /dotnet/api/microsoft.azure.batch.taskconstraints
[net_taskconstraint_retention]: /dotnet/api/microsoft.azure.batch.taskconstraints
[net_task_listsubtasks]: /dotnet/api/microsoft.azure.batch.cloudtask
[net_task_listnodefiles]: /dotnet/api/microsoft.azure.batch.cloudtask
[poolops_getnodefile]: /dotnet/api/microsoft.azure.batch.pooloperations

[portal]: https://portal.azure.com
[rest_multiinstance]: /previous-versions/azure/mt637905(v=azure.100)

[1]: ./media/batch-mpi/batch_mpi_01.png "Çok örnekli genel bakış"