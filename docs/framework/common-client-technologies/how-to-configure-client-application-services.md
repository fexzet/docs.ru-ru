---
title: Практическое руководство. Настройка служб клиентских приложений
ms.date: 03/30/2017
helpviewer_keywords:
- client application services, configuring
ms.assetid: 34a8688a-a32c-40d3-94be-c8e610c6a4e8
ms.openlocfilehash: 6f754a2a66187ac94d31d0d5a3a665c969652d26
ms.sourcegitcommit: 8c28ab17c26bf08abbd004cc37651985c68841b8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2018
ms.locfileid: "48846517"
---
# <a name="how-to-configure-client-application-services"></a>Практическое руководство. Настройка служб клиентских приложений
Этот раздел описывает использование **конструктора проектов** Visual Studio для активации и настройки служб клиентского приложения. Эти службы позволяют проверять подлинность пользователей, извлекать роли пользователей и параметры из существующей службы приложений [!INCLUDE[ajax_current_short](../../../includes/ajax-current-short-md.md)]. После настройки можно получить доступ к включенным службам в коде приложения, как описано в разделе [Общие сведения о службах клиентских приложений](../../../docs/framework/common-client-technologies/client-application-services-overview.md). Дополнительные сведения о службах приложений [!INCLUDE[ajax_current_short](../../../includes/ajax-current-short-md.md)] см. в разделе [Общие сведения о службах приложений ASP.NET](https://msdn.microsoft.com/library/1162e529-0d70-44b2-b3ab-83e60c695013).  
  
 Включать и настраивать службы клиентских приложений можно на странице **Службы** в **конструкторе проектов**. Изменения значений в файле проекта App.config вносятся на странице **Службы**. Открыть **конструктор проектов** можно с помощью команды **Свойства** в меню **Проект**. Дополнительные сведения о странице **Службы** см. в разделе [Страница "Службы" в конструкторе проектов](/visualstudio/ide/reference/services-page-project-designer).  
  
 В следующей процедуре описывается базовая настройка служб клиентских приложений. Расширенные параметры настройки описаны в дальнейших разделах.  
  
### <a name="to-configure-client-application-services"></a>Настройка служб клиентских приложений  
  
1.  В **обозревателе решений** выберите узел проекта, а затем в меню **Проект** щелкните пункт **Свойства**.  
  
     Открывается **конструктор проектов**.  
  
2.  Перейдите на вкладку **Службы**. Будет открыта страница **Службы**, как показано на следующем рисунке.  
  
     ![Вкладка "Службы" в конструкторе проектов](../../../docs/framework/common-client-technologies/media/casdesigner.png "casdesigner")  
  
3.  На странице **Службы** установите флажок **Включить службы клиентского приложения**.  
  
    > [!NOTE]
    >  Для служб клиентских приложений требуется полная версия .NET Framework, и они не поддерживаются в клиентском профиле .NET Framework. Если флажок **Включить службы клиентского приложения** снят, убедитесь, что **Целевая рабочая среда** имеет значение .NET Framework 3.5 или более поздней версии. Чтобы просмотреть значение параметра **Целевая рабочая среда** в C#, откройте конструктор проектов и щелкните страницу **Приложение**. Чтобы просмотреть значение параметра **Целевая рабочая среда** в Visual Basic, откройте конструктор проектов, щелкните страницу **Компиляция** и выберите **Дополнительные параметры компиляции**.  
  
4.  Выберите **Использовать проверку подлинности с помощью форм**, если планируется указать собственные элементы управления или окно входа, или выберите **Использовать проверку подлинности Windows**, чтобы использовать учетные данные, предоставляемые операционной системой. Дополнительные сведения см. в разделе [Общие сведения о службах клиентских приложений](../../../docs/framework/common-client-technologies/client-application-services-overview.md).  
  
    > [!NOTE]
    >  При выборе **Использовать проверку подлинности Windows** службы клиентских приложений будут автоматически настроены для использования базы данных SQL Server Compact. Это указано в окне **Дополнительные параметры служб**, как описано в следующем разделе. Если выбрать **Использовать проверку подлинности с помощью форм**, параметр **Использовать настраиваемую строку подключения** не будет автоматически отключен. Это может привести к ошибкам, если база данных [!INCLUDE[ssEW](../../../includes/ssew-md.md)] уже создана для проверки подлинности Windows. Чтобы избежать этих ошибок, снимите флажок **Использовать настраиваемую строку подключения** в окне **Дополнительные параметры служб**.  
  
5.  Если выбран параметр **Использовать проверку подлинности с помощью форм**, в поле **Местонахождение службы проверки подлинности** укажите URL-адрес узла службы, не указывая имени файла. Конструктор автоматически добавит стандартное имя файла (Authentication_JSON_AppService.axd) при записи значения в файл конфигурации.  
  
6.  При необходимости, если выбран параметр **Использовать проверку подлинности с помощью форм**, можно указать значение в поле **Поставщик учетных данных**. Поставщик учетных данных должен реализовать интерфейс <xref:System.Web.ClientServices.Providers.IClientFormsAuthenticationCredentialsProvider>. Используя поставщик учетных данных, можно отделить интерфейс входа от остального кода приложения. Это позволяет создать окно единого входа для использования в нескольких приложениях. Дополнительные сведения см. в разделе [Практическое руководство. Реализация входа пользователя с помощью служб клиентских приложений](../../../docs/framework/common-client-technologies/how-to-implement-user-login-with-client-application-services.md).  
  
     Если указывать поставщика учетных данных, требуется ввести имя типа сборки. Дополнительные сведения см. в разделах <xref:System.Type.AssemblyQualifiedName%2A?displayProperty=nameWithType> и [Имена сборок](../../../docs/framework/app-domains/assembly-names.md). В простейшем виде имя типа сборки выглядит примерно так:  
  
    ```  
    MyNamespace.MyLoginClass, MyAssembly  
    ```  
  
7.  В полях **Местоположение службы ролей** и **Местоположение службы веб-параметров** укажите расположение каждой службы, не указывая имени файла. Конструктор автоматически добавит стандартные имена файлов (Role_JSON_AppService.axd и Profile_JSON_AppService.axd) при записи значения в файл конфигурации.  
  
8.  При необходимости нажмите кнопку **Дополнительно** для изменения дополнительных параметров, таких как поведение локального кэширования. Дополнительные сведения см. в следующей процедуре.  
  
## <a name="advanced-configuration"></a>Расширенная конфигурация  
 В следующих процедурах описывается настройку служб клиентских приложений для менее распространенных сценариев. Например, эти параметры конфигурации можно использовать для приложений, развертываемых в общедоступных местах, или для использования зашифрованной базы данных SQL Server Compact в качестве локального кэша данных.  
  
#### <a name="to-configure-advanced-settings-for-client-application-services"></a>Настройка дополнительных параметров для служб клиентских приложений  
  
1.  На странице **Службы** в **конструкторе проектов** нажмите кнопку **Дополнительно**.  
  
     Откроется диалоговое окно **Дополнительные параметры служб**, как показано на следующем рисунке. Дополнительные сведения об этом диалоговом окне см. в разделе [Диалоговое окно "Дополнительные параметры служб"](/visualstudio/ide/reference/advanced-settings-for-services-dialog-box).  
  
     ![Диалоговое окно "Дополнительные параметры служб"](../../../docs/framework/common-client-technologies/media/casdialog.png "casdialog")  
  
2.  Установите или снимите флажок **Локально сохранять хэш пароля для обеспечения входа вне сети**. При выборе этого параметра зашифрованный пароль пользователя будут кэширован локально. Это удобно при реализации автономного режима работы приложения. При выборе этого параметра можно выполнять проверку подлинности пользователей, даже если свойству <xref:System.Web.ClientServices.ConnectivityStatus.IsOffline%2A> было задано значение `true`.  
  
3.  Установите или снимите флажок **Требовать, чтобы пользователи повторяли вход, если у файла cookie сервера истек срок действия**. Файл cookie для проверки подлинности настраивается в удаленной службе и указывает, в течение какого времени действует учетная запись пользователя. Дополнительные сведения о настройке файла cookie см. в атрибуте `timeout` в разделе [Элемент "forms" для проверки подлинности (схема параметров ASP.NET)](https://msdn.microsoft.com/library/8163b8b5-ea6c-46c8-b5a9-c4c3de31c0b3).  
  
     Если установить этот флажок, то при попытке доступа к удаленным ролям или службам веб-параметров после истечения срока действия файла cookie для проверки подлинности возникнет исключение <xref:System.Net.WebException>. Это исключение можно обработать и снова отобразить окно входа для повторной проверки пользователей. Пример этого поведения см. в разделе [Пошаговое руководство. Использование служб клиентских приложений](../../../docs/framework/common-client-technologies/walkthrough-using-client-application-services.md). Этот параметр полезен при развертывании приложений в общедоступных системах, чтобы пользователи, которые прекратили пользоваться приложением, не входили в систему автоматически.  
  
     Если снять этот флажок, при попытке доступа к удаленным службам после истечения срока действия файла cookie для проверки подлинности будет автоматически выполнена проверка пользователя.  
  
4.  Укажите значение для параметра **Тайм-аут кэша службы ролей**. Установите небольшой срок действия для часто обновляемых ролей и более длительный срок для редко обновляемых ролей. При реализации автономного режима установите длительный срок, чтобы срок действия сведений о ролях не истек, пока приложение находится в автономном режиме.  
  
     Поставщик ролей получает кэшированные значения ролей или службы ролей при вызове метода <xref:System.Web.Security.RolePrincipal.IsInRole%2A>. Для программного сброса кэша и принудительного доступа к удаленной службе требуется вызвать метод <xref:System.Web.ClientServices.Providers.ClientRoleProvider.ResetCache%2A>.  
  
5.  Установите или снимите флажок **Использовать настраиваемую строку подключения**. Дополнительные сведения см. в следующей процедуре.  
  
#### <a name="to-configure-client-application-services-to-use-a-database-for-the-local-cache"></a>Настройка служб клиентских приложений для использования базы данных локального кэша  
  
1.  На странице **Службы** в **конструкторе проектов** нажмите кнопку **Дополнительно**.  
  
     Откроется диалоговое окно **Дополнительные параметры служб**.  
  
2.  Выберите параметр **Использовать настраиваемую строку подключения**.  
  
     В текстовом поле отобразится значение по умолчанию `Data Source = |SQL/CE|`.  
  
3.  Для создания и использования базы данных SQL Server Compact оставьте значение по умолчанию строки подключения. Visual Studio создаст файл базы данных и поместит его в каталог, указанный в свойстве <xref:System.Windows.Forms.Application.UserAppDataPath%2A?displayProperty=nameWithType>.  
  
4.  Для создания и использования зашифрованной базы данных [!INCLUDE[ssEW](../../../includes/ssew-md.md)] добавьте значения `password` и `encrypt database` в строку подключения, как показано в следующем примере.  
  
    > [!NOTE]
    >  Обязательно используйте надежный пароль. После создания базы данных изменить пароль нельзя.  
  
    ```  
    Data Source = |SQL/CE|;password=<password>;encrypt database=true  
    ```  
  
5.  Чтобы использовать собственную базу данных SQL Server, укажите собственную строку подключения. Дополнительные сведения о форматах строки подключения см. в документации к SQL Server. Эта база данных не создается автоматически. Строка подключения должна ссылаться на существующую базу данных, которую можно создать с помощью следующих инструкций SQL.  
  
    ```  
    CREATE TABLE ApplicationProperties (PropertyName nvarchar(256),  
        PropertyValue nvarchar(256))  
    CREATE TABLE UserProperties (PropertyName nvarchar(256),  
        PropertyValue nvarchar(256))  
    CREATE TABLE Roles (UserName nvarchar(256),   
        RoleName nvarchar(256))  
    CREATE TABLE Settings (PropertyName nvarchar(256),   
        PropertyStoredAs nvarchar(1), PropertyValue nvarchar(2048))  
    ```  
  
## <a name="using-custom-providers"></a>Использование настраиваемых поставщиков  
 По умолчанию компонент служб клиентских приложений использует поставщиков в пространстве имен <xref:System.Web.ClientServices.Providers?displayProperty=nameWithType>. При настройке приложения с помощью страницы **Службы** в **конструкторе проектов** ссылки на этих поставщиков добавляются в файл App.config. Эти поставщики по умолчанию получают доступ к соответствующим поставщикам на сервере. Веб-службы часто настраивается для доступа к данным пользователя через поставщиков, таких как <xref:System.Web.Security.SqlMembershipProvider> и <xref:System.Web.Security.SqlRoleProvider>.  
  
 Если нужно использовать других поставщиков, обычно следует изменить поставщиков на стороне сервера, чтобы изменения действовали для всех клиентских приложений, получающих доступ к серверу. Впрочем, есть возможность использовать других поставщиков и на стороне клиента. В файле App.config можно указать настраиваемых поставщиков проверки подлинности и поставщиков ролей. Сведения о создании пользовательской проверки подлинности и поставщиков служб см. в разделах [Реализация поставщика членства](https://msdn.microsoft.com/library/d8658b8e-c962-4f64-95e1-4acce35e4582) и [Реализация поставщика ролей](https://msdn.microsoft.com/library/851671ce-bf9b-43f2-aba4-bc9d28b11c7d). Также можно использовать настраиваемый поставщик параметров, изменив в проекте класс `Settings` (доступен как `Properties.Settings.Default` в C# и `My.Settings` в Visual Basic). Дополнительные сведения см. в разделе [Архитектура параметров приложения](../../../docs/framework/winforms/advanced/application-settings-architecture.md).  
  
#### <a name="to-configure-client-application-services-to-use-non-default-providers"></a>Настройка служб клиентских приложений для использования поставщиков, отличных от поставщиков по умолчанию  
  
1.  Для использования настраиваемых поставщиков проверки подлинности или служб ролей сначала нужно задать значения прочим параметрам на странице **Службы**.  
  
2.  Закройте **конструктор проектов**. Это необходимо, так как страница **Службы** будет автоматически обновлять файл App.config, даже если вы не меняете никакие параметры. Если изменить файл App.config вручную, как описано в этой процедуре, и затем вернуться на страницу **Службы**, все изменения будут сброшены.  
  
3.  В **обозревателе решений** дважды щелкните файл App.config.  
  
     Файл конфигурации приложения откроется в текстовом редакторе.  
  
4.  Найдите элемент `<providers>` в элементе `<membership>` или `<roleManager>`. Это дочерние элементы элемента `<system.web>`. Элемент `<membership>` используется для указания поставщиков проверки подлинности, а элемент `<roleManager>` — для указания поставщиков ролей.  
  
5.  Добавьте элемент `<add>` в качестве дочернего для элемента `<providers>`. Необходимо указать атрибуты `name` и `type`, как показано в следующем примере. Значение атрибута `type` должно представлять собой имя типа сборки. Дополнительные сведения см. в разделах <xref:System.Type.AssemblyQualifiedName%2A?displayProperty=nameWithType> и [Имена сборок](../../../docs/framework/app-domains/assembly-names.md).  
  
    ```xml  
    <add name="MyCustomRoleProvider" type="MyNamespace.MyRoleProvider, MyAssembly" />  
    ```  
  
6.  Измените атрибут `defaultProvider` элемента `<membership>` или `<roleManager>`, чтобы указать значение имени элемента `<add>`, добавленного на предыдущем шаге.  
  
    ```xml  
    <roleManager enabled="true" defaultProvider="MyCustomRoleProvider">  
    ```  
  
## <a name="see-also"></a>См. также  
 [Службы клиентских приложений](../../../docs/framework/common-client-technologies/client-application-services.md)  
 [Общие сведения о службах клиентских приложений](../../../docs/framework/common-client-technologies/client-application-services-overview.md)  
 [Страница "Службы" в конструкторе проектов](/visualstudio/ide/reference/services-page-project-designer)  
 [Диалоговое окно "Дополнительные параметры служб"](/visualstudio/ide/reference/advanced-settings-for-services-dialog-box)  
 [Практическое руководство. Реализация входа пользователя с помощью служб клиентских приложений](../../../docs/framework/common-client-technologies/how-to-implement-user-login-with-client-application-services.md)  
 [Пошаговое руководство. Использование служб клиентских приложений](../../../docs/framework/common-client-technologies/walkthrough-using-client-application-services.md)  
 [Реализация поставщика членства](https://msdn.microsoft.com/library/d8658b8e-c962-4f64-95e1-4acce35e4582)  
 [Реализация поставщика ролей](https://msdn.microsoft.com/library/851671ce-bf9b-43f2-aba4-bc9d28b11c7d)  
 [Архитектура параметров приложения](../../../docs/framework/winforms/advanced/application-settings-architecture.md)  
 [Создание и настройка базы данных служб приложений для SQL Server](https://msdn.microsoft.com/library/ab894e83-7e2f-4af8-a116-b1bff8f815b2)
