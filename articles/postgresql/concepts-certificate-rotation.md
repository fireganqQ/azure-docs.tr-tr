---
title: PostgreSQL için Azure veritabanı tek sunucu için sertifika döndürme
description: PostgreSQL için Azure veritabanı tek sunucu için Azure veritabanı 'nı etkileyecek olan kök sertifika değişikliklerinin yakında değişiklikler hakkında bilgi edinin
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/02/2020
ms.openlocfilehash: 96720e156963a5fb542e72823a602aa2cc6a0ead
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93349010"
---
# <a name="understanding-the-changes-in-the-root-ca-change-for-azure-database-for-postgresql-single-server"></a>PostgreSQL için Azure veritabanı 'nın tek sunucu değişikliği için kök CA 'daki değişiklikleri anlama

PostgreSQL için Azure veritabanı, [veritabanı sunucusuna bağlanmak](concepts-connectivity-architecture.md)IÇIN kullanılan SSL ile etkinleştirilen istemci uygulaması/sürücüsü için kök sertifikayı değiştiriyor. Şu anda kullanılabilir kök sertifika, standart bakım ve güvenlik en iyi uygulamalarının bir parçası olarak 15 Şubat 2021 (02/15/2021) tarihinde dolacak şekilde ayarlanmıştır. Bu makale, yaklaşan değişiklikler hakkında daha fazla ayrıntıyı, etkilenecek kaynakları ve uygulamanızın veritabanı sunucunuza bağlantıyı korumasından emin olmak için gerekli adımları sağlar.

>[!NOTE]
> Müşterilerin geri bildirimlerine bağlı olarak, var olan Baltidaha fazla kök CA 'nın 11 Ekim 2021 ' den itibaren 2020 ' ye kadar kök sertifikayı kullanımdan kaldırabiliyoruz. Bu uzantının, kullanıcılarımız etkilendiklerinde istemci değişikliklerini uygulaması için yeterli sağlama süresi sağlamasını umuyoruz.

## <a name="what-update-is-going-to-happen"></a>Hangi güncelleştirme gerçekleşecektir?

Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir sertifika yetkilisi (CA) sertifika dosyasından oluşturulan yerel bir sertifika dosyası kullanır. Şu anda müşteriler, [burada](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)yer alan bir PostgreSQL Için Azure veritabanı sunucusuna bağlanmak üzere önceden tanımlanmış sertifikayı kullanabilir. Bununla birlikte, [sertifika yetkilisi (CA) tarayıcı Forumu](https://cabforum.org/) , CA satıcıları tarafından verilen birden çok sertifikanın uyumsuz olması için en son yayımlanmış raporlar.

Sektörün uyumluluk gereksinimlerine göre, CA satıcıları uyumlu olmayan CA 'Lar için CA sertifikalarını iptal etmeyi, sunucuların uyumlu CA 'lar tarafından verilen sertifikaları kullanmalarını ve bu uyumlu CA 'lardan CA sertifikaları kullanmasını gerektirir. PostgreSQL için Azure veritabanı şu anda bu uyumlu olmayan sertifikalardan birini kullandığından, istemci uygulamalarının SSL bağlantılarını doğrulamak için kullandığı, PostgreSQL sunucularınız için olası etkiyi en aza indirmek için uygun eylemlerin (aşağıda açıklanmıştır) yapıldığından emin olmamız gerekir.

Yeni sertifika 15 Şubat 2021 tarihinden itibaren kullanılacaktır (02/15/2021). Bir PostgreSQL istemcisinden bağlanırken sunucu sertifikasının CA doğrulamasını veya tam doğrulamasını kullanırsanız (sslmode = Verify-CA veya sslmode = Verify-Full), 15 Şubat 2021 tarihinden önce uygulama yapılandırmanızı güncelleştirmeniz gerekir (02/15/2021).

## <a name="how-do-i-know-if-my-database-is-going-to-be-affected"></a>Nasıl yaparım? Veritabanımın etkilenip etkilenmediğini öğrensin mi?

SSL/TLS kullanan tüm uygulamalar ve kök sertifikanın kök sertifikayı güncelleştirmesi gerekir. Bağlantı dizenizi inceleyerek bağlantılarınızın kök sertifikayı doğrulayıp doğrulamayıp belirleyemeyeceğini belirleyebilirsiniz.
-    Bağlantı dizeniz veya içeriyorsa `sslmode=verify-ca` `sslmode=verify-full` , sertifikayı güncelleştirmeniz gerekir.
-    Bağlantı dizeniz,, `sslmode=disable` veya içerdiğinde, `sslmode=allow` `sslmode=prefer` `sslmode=require` sertifikaları güncelleştirmeniz gerekmez. 
-    Bağlantı dizeniz sslmode belirtmezse, sertifikaları güncelleştirmeniz gerekmez.

Bağlantı dizesini soyutlayan bir istemci kullanıyorsanız, sertifikaları doğrulayıp doğrulamadığını anlamak için istemci belgelerini gözden geçirin.

PostgreSQL sslmode ' i anlamak için PostgreSQL belgelerindeki [SSL modu açıklamalarını](https://www.postgresql.org/docs/11/libpq-ssl.html#ssl-mode-descriptions) gözden geçirin.

Sertifikaların beklenmedik şekilde iptal edildiği veya bir sertifikayı güncelleştirme, iptal edilmiş olması nedeniyle uygulamanızın kullanılabilirliğinin kesintiye uğramasını önlemek için, [**"bağlantıyı sürdürmek Için ne yapmam gerekir"**](concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity) bölümüne bakın.

## <a name="what-do-i-need-to-do-to-maintain-connectivity"></a>Bağlantıyı sürdürmek için ne yapmam gerekir?

Sertifikaların beklenmedik şekilde iptal edildiği veya bir sertifikayı güncelleştirme, iptal edilmiş olması nedeniyle uygulamanızın kullanılabilirliğinin kesintiye uğramasını önlemek için aşağıdaki adımları izleyin. Bu düşünce, geçerli sertifikayı ve yeni birini birleştiren yeni bir *. pem* dosyası oluşturmak ve izin verilen değerlerin BIR kez SSL sertifika doğrulaması sırasında kullanılacaktır. Aşağıdaki adımlara bakın:

*   Aşağıdaki bağlantılardan BaltimoreCyberTrustRoot & DigiCertGlobalRootG2 kök CA 'sını indirin:
    *   https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem
    *   https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem

*   Hem **Baltimorecyıbertrustroot** hem de **DigiCertGlobalRootG2** sertifikaları dahil olmak üzere bir birleştirilmiş CA sertifika deposu oluşturun.
    *   DefaultJavaSSLFactory kullanan Java (PostgreSQL JDBC) kullanıcıları için şunu çalıştırın:

          ```console
          keytool -importcert -alias PostgreSQLServerCACert  -file D:\BaltimoreCyberTrustRoot.crt.pem  -keystore truststore -storepass password -noprompt
          ```

          ```console
          keytool -importcert -alias PostgreSQLServerCACert2  -file D:\DigiCertGlobalRootG2.crt.pem -keystore truststore -storepass password  -noprompt
          ```

          Ardından orijinal anahtar deposu dosyasını yeni oluşturulan yeni bir anahtar ile değiştirin:
        *   System. setProperty ("javax. net. SSL. trustStore", "path_to_truststore_file"); 
        *   System. setProperty ("javax. net. SSL. trustStorePassword", "Password");
        
    *   Windows 'daki .NET (Npgsql) kullanıcıları için, Windows sertifika deposunda, güvenilen kök sertifika yetkilileri ' nde hem **Baltimore CyberTrust kökünün** hem de **DigiCert genel kökünün G2** olduğundan emin olun. Herhangi bir sertifika yoksa, eksik sertifikayı içeri aktarın.

        ![PostgreSQL için Azure veritabanı .net sertifikası](media/overview/netconnecter-cert.png)

    *   Linux üzerinde SSL_CERT_DIR kullanarak .NET (Npgsql) kullanıcıları için, SSL_CERT_DIR tarafından belirtilen dizinde **Baltimorecyıbertrustroot** ve **DigiCertGlobalRootG2** 'ın aynı olduğundan emin olun. Herhangi bir sertifika yoksa, eksik sertifika dosyasını oluşturun.

    *   Diğer PostgreSQL istemci kullanıcıları için aşağıdaki biçim gibi iki CA sertifika dosyasını birleştirebilirsiniz

        </br>-----BAŞLANGıÇ SERTIFIKASı-----
 </br>(Root CA1: BaltimoreCyberTrustRoot. CRT. pek)
 </br>-----SON SERTIFIKA-----
 </br>-----BAŞLANGıÇ SERTIFIKASı-----
 </br>(Root CA2: DigiCertGlobalRootG2. CRT. pek)
 </br>-----SON SERTIFIKA-----

*   Özgün kök CA ped dosyasını birleştirilmiş kök CA dosyası ile değiştirin ve uygulamanızı/istemcinizi yeniden başlatın.
*    Daha sonra, yeni sertifika sunucu tarafında dağıtıldıktan sonra, CA PEI dosyanızı DigiCertGlobalRootG2. CRT. ped olarak değiştirebilirsiniz.

## <a name="what-can-be-the-impact-of-not-updating-the-certificate"></a>Sertifikayı güncelleştirmeden ne etkisi olabilir?
Burada belgelenen PostgreSQL için Azure veritabanı 'na SSL bağlantısını doğrulamak üzere Baltimore CyberTrust kök sertifikası kullanıyorsanız, veritabanının ulaşılamaz olması nedeniyle uygulamanızın kullanılabilirliği kesintiye uğramış olabilir. Uygulamanıza bağlı olarak, aşağıdakiler dahil ancak bunlarla sınırlı olmamak üzere çeşitli hata iletileri alabilirsiniz:
*    Geçersiz sertifika/iptal edilmiş sertifika
*    Bağlantı zaman aşımına uğradı

> [!NOTE]
> Lütfen sertifika değişikliği yapılıncaya kadar **Baltidaha fazla sertifikayı** kaldırmayın veya değiştirmeyin. Değişiklik yapıldıktan sonra, Baltidaha fazla sertifikayı bırakması için güvenli olacak şekilde bir iletişim göndereceğiz. 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

###    <a name="1-if-i-am-not-using-ssltls-do-i-still-need-to-update-the-root-ca"></a>1. SSL/TLS kullanmıyorum, yine de kök CA 'yı güncelleştirmem gerekir mi?
SSL/TLS kullanmıyorsanız hiçbir eylem gerekmez. 

### <a name="2-if-i-am-using-ssltls-do-i-need-to-restart-my-database-server-to-update-the-root-ca"></a>2. SSL/TLS kullanıyorum, kök CA 'yı güncelleştirmek için veritabanı sunucusunu yeniden başlatmem gerekir mi?
Hayır, yeni sertifikayı kullanmaya başlamak için veritabanı sunucusunu yeniden başlatmanız gerekmez. Bu bir istemci tarafı değişikdir ve gelen istemci bağlantılarının veritabanı sunucusuna bağlanabildiklerinden emin olmak için yeni sertifikayı kullanması gerekir.

### <a name="3-what-will-happen-if-i-do-not-update-the-root-certificate-before-february-15-2021-02152021"></a>3. kök sertifikayı 15 Şubat 2021 (02/15/2021) tarihinden önce güncelleştirdiğimde ne olur?
Kök sertifikayı 15 Şubat 2021 (02/15/2021) tarihinden önce güncelleştirmediğinizi, SSL/TLS aracılığıyla bağlanan ve kök sertifika için doğrulama yapan uygulamalarınız PostgreSQL veritabanı sunucusuyla iletişim kuramaz ve uygulama PostgreSQL veritabanı sunucunuza bağlantı sorunlarıyla karşılaşacaktır.

### <a name="4-what-is-the-impact-if-using-app-service-with-azure-database-for-postgresql"></a>4. PostgreSQL için Azure veritabanı ile App Service kullanılıyorsa etki nedir?
Azure Uygulama Hizmetleri için PostgreSQL için Azure veritabanı 'na bağlanmak üzere iki olası senaryo olabilir ve bu, uygulamanızla birlikte SSL kullanma şeklinize bağlıdır.
*   Bu yeni sertifika platform düzeyinde App Service eklendi. Uygulamanızdaki App Service platforma dahil olan SSL sertifikalarını kullanıyorsanız hiçbir işlem gerekmez.
*   Kodunuzda SSL sertifika dosyasının yolunu açıkça dahil ediyorsanız, yeni sertifikayı indirmeniz ve kodu yeni sertifikayı kullanacak şekilde güncelleştirmeniz gerekir. Bu senaryonun iyi bir örneği, [App Service belgelerinde](../app-service/tutorial-multi-container-app.md#configure-database-variables-in-wordpress) paylaşıldığından App Service özel kapsayıcılar kullandığınızda oluşur

### <a name="5-what-is-the-impact-if-using-azure-kubernetes-services-aks-with-azure-database-for-postgresql"></a>5. PostgreSQL için Azure veritabanı ile Azure Kubernetes Hizmetleri (AKS) kullanılıyorsa etkisi nedir?
Azure Kubernetes Services (AKS) kullanarak PostgreSQL için Azure veritabanı 'na bağlanmaya çalışıyorsanız, adanmış bir müşterilerin ana bilgisayar ortamından erişime benzer. [Buradaki](../aks/ingress-own-tls.md)adımlara bakın.

### <a name="6-what-is-the-impact-if-using-azure-data-factory-to-connect-to-azure-database-for-postgresql"></a>6. PostgreSQL için Azure veritabanı 'na bağlanmak üzere Azure Data Factory kullanılıyorsa etkisi nedir?
Bağlayıcı, Azure Integration Runtime kullanan bağlayıcı için Azure 'da barındırılan ortamdaki Windows sertifika deposundaki sertifikalardan yararlanır. Bu sertifikalar zaten yeni uygulanan sertifikalarla uyumludur ve bu nedenle herhangi bir eylem gerekmez.

Şirket içinde barındırılan Integration Runtime kullanan bağlayıcı için, bağlantı dizinizdeki SSL sertifikası dosyasının yolunu açıkça eklediğiniz yerde, [yeni sertifikayı](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) indirip kullanılacak bağlantı dizesini güncelleştirmeniz gerekir.

### <a name="7-do-i-need-to-plan-a-database-server-maintenance-downtime-for-this-change"></a>7. bu değişiklik için bir veritabanı sunucusu bakım kapalı kalma süresi planlamanız gerekir mi?
Hayır. Buradaki değişiklik yalnızca istemci tarafında, veritabanı sunucusuna bağlanmak için olduğundan, bu değişiklik için veritabanı sunucusu için gerekli bakım kapalı kalma süresi yoktur.

### <a name="8--what-if-i-cannot-get-a-scheduled-downtime-for-this-change-before-february-15-2021-02152021"></a>8. bu değişiklik için 15 Şubat 2021 tarihinden önce zamanlanan kapalı kalma süresi (02/15/2021) yoksa ne olur?
Sunucuya bağlanmak için kullanılan istemcilerin [buradaki](./concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity)çözüm bölümünde açıklandığı gibi sertifika bilgilerini güncelleştirmesi gerektiğinden, bu durumda sunucu için kapalı kalma süresine gerek kalmaz.

### <a name="9-if-i-create-a-new-server-after-february-15-2021-02152021-will-i-be-impacted"></a>9.15 Şubat 2021 (02/15/2021) sonrasında yeni bir sunucu oluştururum, bundan etkilenecek mıyım?
15 Şubat 2021 (02/15/2021) ' den sonra oluşturulan sunucular için, SSL kullanarak bağlanmak üzere uygulamalarınız için yeni verilen sertifikayı kullanabilirsiniz.

###    <a name="10-how-often-does-microsoft-update-their-certificates-or-what-is-the-expiry-policy"></a>10. Microsoft sertifikalarını ne sıklıkla güncelleştiriyor veya süre sonu ilkesi nedir?
PostgreSQL için Azure veritabanı tarafından kullanılan bu sertifikalar, güvenilen sertifika yetkilileri (CA) tarafından sağlanır. Bu nedenle, PostgreSQL için Azure veritabanı 'nda bu sertifikaların desteklenmesi, CA tarafından bu sertifikaların desteğine bağlıdır. Bununla birlikte, bu örnekte olduğu gibi, önceden tanımlanmış bu sertifikalarda, en erken düzeltilmelidir.

###    <a name="11-if-i-am-using-read-replicas-do-i-need-to-perform-this-update-only-on-the-primary-server-or-the-read-replicas"></a>11. okuma çoğaltmaları kullanıyorum, bu güncelleştirmeyi yalnızca birincil sunucuda veya okuma çoğaltmalarıyla gerçekleştirmem gerekir mi?
Bu güncelleştirme bir istemci tarafı değişikliği olduğundan, istemci Çoğaltma sunucusundan veri okumak için kullanılıyorsa, bu istemciler için değişiklikleri de uygulamanız gerekecektir. 

### <a name="12-do-we-have-server-side-query-to-verify-if-ssl-is-being-used"></a>12. SSL 'nin kullanıldığını doğrulamak için sunucu tarafı sorgumuz var mı?
Sunucuya bağlanmak için SSL bağlantısı kullanıp kullandığınızı doğrulamak için [SSL doğrulaması](concepts-ssl-connection-security.md#applications-that-require-certificate-verification-for-tls-connectivity)' na başvurun.

### <a name="13-is-there-an-action-needed-if-i-already-have-the-digicertglobalrootg2-in-my-certificate-file"></a>13. sertifika dosyasında DigiCertGlobalRootG2 zaten varsa gerekli bir eylem var mı?
Hayır. Sertifika dosyanızda zaten **DigiCertGlobalRootG2** varsa herhangi bir eylem gerekmez.

### <a name="14-what-is-you-are-using-docker-image-of-pgbouncer-sidecar-provided-by-microsoft"></a>14. Microsoft tarafından sunulan pgbouncer sepet 'ın Docker görüntüsünü kullanıyorsunuz?
Hem [**Baltimore**](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) hem de [**DigiCert**](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) 'yi destekleyen yeni bir Docker görüntüsü [burada aşağıda verilmiştir](https://hub.docker.com/_/microsoft-azure-oss-db-tools-pgbouncer-sidecar) (en son etiket). 15 Şubat 2021 ' den itibaren bağlantının kesintiye uğramasını önlemek için bu yeni görüntüyü çekebilirsiniz. 

###    <a name="15-what-if-i-have-further-questions"></a>15. daha fazla sorunuz varsa ne yapmalıyım?
Sorularınız varsa, [Microsoft Q&A](mailto:AzureDatabaseforPostgreSQL@service.microsoft.com)'daki topluluk uzmanlarının yanıtlarını alın. Destek planınız varsa ve teknik yardıma ihtiyacınız varsa  [bizimle iletişime geçin](mailto:AzureDatabaseforPostgreSQL@service.microsoft.com)
