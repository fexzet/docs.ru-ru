---
title: Пример безопасности сообщений
ms.date: 03/30/2017
ms.assetid: 82444166-6288-493a-85d4-85f43f134d19
ms.openlocfilehash: 8982e896f6ac383a1fd850bc5814bf99e5c3961d
ms.sourcegitcommit: c93fd5139f9efcf6db514e3474301738a6d1d649
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2018
ms.locfileid: "50193751"
---
# <a name="message-security-sample"></a>Пример безопасности сообщений
Данный образец демонстрирует реализацию приложения, в котором используется привязка `basicHttpBinding` и безопасность сообщений. Этот образец основан на [Приступая к работе](../../../../docs/framework/wcf/samples/getting-started-sample.md) , реализующем службу калькулятора.  
  
> [!NOTE]
>  Процедура настройки и инструкции по построению для данного образца приведены в конце этого раздела.  
  
 Режим безопасности `basicHttpBinding` может быть установлен в следующие значения: `Message`, `Transport`, `TransportWithMessageCredential`, `TransportCredentialOnly` и `None`. В следующем образце файла службы App.config, в определении конечной точки задаются привязка `basicHttpBinding` и ссылки на конфигурацию привязки с именем `Binding1`, как показано в следующем образце конфигурации.  
  
```xml  
<system.serviceModel>  
  <services>  
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
             behaviorConfiguration="CalculatorServiceBehavior">  
     <!-- This endpoint is exposed at the base address provided by -->  
     <!-- host: http://localhost:8000/ServiceModelSamples/service.-->  
     <endpoint address=""  
               binding="basicHttpBinding"  
               bindingConfiguration="Binding1"   
               contract="Microsoft.ServiceModel.Samples.ICalculator" />  
    </service>  
  </services>   …  
</system.serviceModel>  
```  
  
 Конфигурация привязки задает `mode` атрибут [ \<безопасности >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-basichttpbinding.md) для `Message` и задает `clientCredentialType` атрибут [ \<сообщения >](../../../../docs/framework/configure-apps/file-schema/wcf/message-of-basichttpbinding.md)для `Certificate` как показано в следующем примере конфигурации:  
  
```xml  
<bindings>  
  <basicHttpBinding>  
    <!--   
        This configuration defines the SecurityMode as Message and   
        the clientCredentialType as Certificate.  
        -->  
    <binding name="Binding1" >  
      <security mode = "Message">  
        <message clientCredentialType="Certificate"/>  
      </security>  
    </binding>  
  </basicHttpBinding>  
</bindings>  
```  
  
 Сертификат, используемый службой для проверки своей подлинности при подключении к клиенту, установлен в разделе поведений элемента `serviceCredentials` файла конфигурации. Режим проверки, применяемый к сертификату, который клиент использует для проверки своей подлинности при подключении к службе, также задается в разделе поведений элемента `clientCertificate`.  
  
```xml  
<!--For debugging purposes, set the includeExceptionDetailInFaults attribute to true.-->  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="CalculatorServiceBehavior">  
      <serviceMetadata httpGetEnabled="True"/>  
      <serviceDebug includeExceptionDetailInFaults="False" />  
      <!--The serviceCredentials behavior allows one to define a -->  
      <!--service certificate. A service certificate is used by a -->  
      <!--client to authenticate the service and provide message -->  
      <!-- protection. This configuration references the "localhost"-->  
      <!--certificate installed during the setup instructions. -->  
      <serviceCredentials>  
        <serviceCertificate findValue="localhost"  
               storeLocation="LocalMachine"   
               storeName="My" x509FindType="FindBySubjectName" />  
        <clientCertificate>  
          <!-- Setting the certificateValidationMode to -->  
          <!-- PeerOrChainTrust means that if the certificate -->  
          <!--is in the user's Trusted People store, then it is -->  
          <!-- trusted without performing a validation of the -->  
          <!-- certificate's issuer chain. This setting is used -->  
          <!-- here for convenience so that the sample can be run -->  
          <!-- without having to have certificates issued by a -->  
          <!-- certification authority (CA). -->  
          <!-- This setting is less secure than the default, -->  
          <!-- ChainTrust. The security implications of this -->  
          <!-- setting should be carefully considered before using -->  
          <!-- PeerOrChainTrust in production code. -->  
          <authentication   
                       certificateValidationMode="PeerOrChainTrust" />  
        </clientCertificate>  
      </serviceCredentials>  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
 Та же привязка и данные безопасности задаются в файле конфигурации клиента.  
  
 Идентификация абонента отображается в окне консоли службы с помощью следующего кода.  

```csharp
Console.WriteLine("Called by {0}", ServiceSecurityContext.Current.PrimaryIdentity.Name);  
```

 При выполнении примера запросы и ответы операций отображаются в окне консоли клиента. Чтобы закрыть клиент, нажмите клавишу ВВОД в окне клиента.  
  
```  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
Press <ENTER> to terminate client.  
```  
  
### <a name="to-set-up-and-build-the-sample"></a>Настройка и сборка образца  
  
1.  Убедитесь, что вы выполнили [выполняемая однократно процедура настройки для образцов Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2.  Чтобы создать выпуск решения на языке C# или Visual Basic .NET, следуйте инструкциям в разделе [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
### <a name="to-run-the-sample-on-the-same-machine"></a>Запуск образца на том же компьютере  
  
1.  Запустите файл Setup.bat из папки установки примера. При этом устанавливаются все сертификаты, необходимые для выполнения образца.  
  
    > [!NOTE]
    >  Пакетный файл Setup.bat предназначен для запуска из командной строки Windows SDK. Требуется, чтобы переменная среды MSSDK указывала на каталог, в котором установлен пакет SDK. Эта переменная среды автоматически устанавливается в командной строке Windows SDK.  
  
2.  Запустите приложение службы из \service\bin.  
  
3.  Запустите клиентское приложение из \service\bin. Действия клиента отображаются в консольном приложении клиента.  
  
4.  Если клиенту и службе не удается взаимодействовать, см. раздел [Troubleshooting Tips](https://msdn.microsoft.com/library/8787c877-5e96-42da-8214-fa737a38f10b).  
  
5.  После завершения работы образца запустите файл Cleanup.bat, чтобы удалить сертификаты. В других образцах обеспечения безопасности используются те же сертификаты.  
  
### <a name="to-run-the-sample-across-machines"></a>Выполнение примера на нескольких компьютерах  
  
1.  Создайте на компьютере службы каталог для двоичных файлов службы.  
  
2.  Скопируйте файлы программ службы из каталога службы на сервер. Также скопируйте на сервер файлы Setup.bat, Cleanup.bat и ImportClientCert.bat.  
  
3.  Создайте на клиентском компьютере каталог для двоичных файлов клиента.  
  
4.  Скопируйте в клиентский каталог на клиентском компьютере файлы программы клиента. Кроме того, скопируйте на клиент файлы Setup.bat, Cleanup.bat и ImportServiceCert.bat.  
  
5.  На сервере выполните команду `setup.bat service`. Под управлением `setup.bat` с `service` аргумент создается сертификат службы с полным доменным именем компьютера и экспортируется в файл с именем Service.cer.  
  
6.  Изменить файл Service.exe.config изменения в соответствии с новым именем сертификата (в `findValue` атрибут в [ \<serviceCertificate >](../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) элемент) которого совпадает со значением полное доменное имя компьютера. Также измените значение базового адреса для указания полного доменного имени компьютера вместо localhost`.`  
  
7.  Скопируйте файл Service.cer из каталога службы в клиентский каталог на клиентском компьютере.  
  
8.  На клиенте выполните команду `setup.bat client`. При выполнении команды `setup.bat`с аргументом `client` создается сертификат клиента с именем client.com, который экспортируется в файл с именем Client.cer.  
  
9. В файле Client.exe.config на клиентском компьютере измените значение адреса конечной точки, чтобы оно соответствовало новому адресу службы. Для этого замените имя localhost полным именем домена сервера. Кроме того, изменить `findValue` атрибут [ \<defaultCertificate >](../../../../docs/framework/configure-apps/file-schema/wcf/defaultcertificate-element.md) к новым именем сертификата службы, которое — полное доменное имя сервера.  
  
10. Скопируйте файл Client.cer из клиентского каталога в каталог службы на сервере.  
  
11. Запустите на клиенте файл ImportServiceCert.bat. Он импортирует сертификат службы из файла Service.cer в хранилище CurrentUser - TrustedPeople.  
  
12. На сервере запустите файл ImportClientCert.bat. Он импортирует сертификат клиента из файла Client.cer в хранилище LocalMachine - TrustedPeople.  
  
13. Запустите программу Service.exe из командной строки на компьютере службы.  
  
14. На клиентском компьютере из окна командной строки запустите программу Client.exe.  
  
    1.  Если клиенту и службе не удается взаимодействовать, см. раздел [Troubleshooting Tips](https://msdn.microsoft.com/library/8787c877-5e96-42da-8214-fa737a38f10b).  
  
### <a name="to-clean-up-after-the-sample"></a>Очистка после образца  
  
-   После завершения работы примера запустите в папке примеров файл Cleanup.bat.  
  
    > [!NOTE]
    >  Этот скрипт не удаляет сертификаты службы на клиенте при выполнении примера на нескольких компьютерах. Если вы запустили примеры Windows Communication Foundation (WCF), которые используют сертификаты между компьютерами, обязательно удалите сертификаты службы, которые были установлены в хранилище CurrentUser - trustedpeople. Для этого воспользуйтесь следующей командой: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>`. Пример: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`  
  
> [!IMPORTANT]
>  Образцы уже могут быть установлены на компьютере. Перед продолжением проверьте следующий каталог (по умолчанию).  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  Если этот каталог не существует, перейдите к [Windows Communication Foundation (WCF) и образцы Windows Workflow Foundation (WF) для .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) для загрузки всех Windows Communication Foundation (WCF) и [!INCLUDE[wf1](../../../../includes/wf1-md.md)] примеры. Этот образец расположен в следующем каталоге.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Basic\MessageSecurity`  
  
## <a name="see-also"></a>См. также
