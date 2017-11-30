---
title: Serialization1
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-standard
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: bebb27ac-9712-4196-9931-de19fc04dbac
caps.latest.revision: "4"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.openlocfilehash: e63428400a7868b71a2d8e52637e4b22e4c44ee7
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2017
---
# <a name="serialization"></a>Сериализация
Сериализация — это процесс преобразования объекта в формате, который можно легко сохранять или передавать. Например можно сериализовать объект, передавать его через Интернет с помощью протокола HTTP и десериализовать его на целевой компьютер.  
  
 Платформа .NET Framework предоставляет три основных сериализации технологии, оптимизированными для различных сценариев сериализации. В следующей таблице описаны эти технологии, а также связанные с ними основные типы Framework.  
  
|**Имя технологии**|**Основные типы**|**Сценарии**|  
|-------------------------|--------------------|-------------------|  
|**Сериализации контракта данных**|<xref:System.Runtime.Serialization.DataContractAttribute> <br /> <xref:System.Runtime.Serialization.DataMemberAttribute> <br /> <xref:System.Runtime.Serialization.DataContractSerializer> <br /> <xref:System.Runtime.Serialization.NetDataContractSerializer> <br /> <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> <br /> <xref:System.Runtime.Serialization.ISerializable>|Общее сохранение<br />Веб-службы<br />JSON|  
|**Сериализация XML**|<xref:System.Xml.Serialization.XmlSerializer>|Формат XML с полным доступом формой создаваемой XML|  
|**Сериализация во время выполнения (Binary и SOAP)**|<xref:System.SerializableAttribute> <br /> <xref:System.Runtime.Serialization.ISerializable> <br /> <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> <br /> <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>|Удаленное взаимодействие .NET|  
  
 **✓ СДЕЛАТЬ** Вспомните сериализации при разработке новых типов.  
  
## <a name="choosing-the-right-serialization-technology-to-support"></a>Правильный выбор сериализации для поддержки  
 **✓ Попробуйте** поддержки сериализации контракта данных, если экземпляры этого типа, может потребоваться сохранить или использовать в веб-службы.  
  
 **✓ Попробуйте** поддержка XML-сериализации вместо или в дополнение к сериализации контракта данных, если требуется больший контроль над форматом XML, который создается при сериализации типа.  
  
 Это может потребоваться в некоторых взаимодействия создания сценариев, где необходимо использовать XML, не поддерживаемый сериализации контракта данных, например, для создания XML-атрибуты.  
  
 **✓ Попробуйте** поддерживающий сериализацию во время выполнения, если экземпляров типа проходить через границы удаленного взаимодействия .NET.  
  
 **X ИЗБЕГАЙТЕ** поддерживающий сериализацию во время выполнения или XML-сериализации только в целях общие сохраняемости. Вместо необходимости сериализации контракта данных.  
  
## <a name="supporting-data-contract-serialization"></a>Поддержка сериализации контракта данных  
 Типы поддерживают сериализации контракта данных, применяя <xref:System.Runtime.Serialization.DataContractAttribute> типу и <xref:System.Runtime.Serialization.DataMemberAttribute> членам типа (поля и свойства).  
  
 **✓ Попробуйте** пометить тип общедоступные элементы данных, если тип можно использовать в режиме частичного доверия.  
  
 В режиме полного доверия сериализаторы контрактов данных могут сериализовать и десериализовать закрытым типам и членам, но только открытые члены, которые можно сериализовать и десериализовать в режиме частичного доверия.  
  
 **СДЕЛАТЬ ✓** реализовать Get и set для всех свойств, имеющих <xref:System.Runtime.Serialization.DataMemberAttribute>. Сериализаторы контрактов данных требуется метод считывания и метод задания значения типа считается допустимым сериализуемым. (В .NET Framework 3.5 SP1, некоторые свойства коллекции может быть только для чтения.) Если тип не будет использоваться в среде с частичным доверием, то один или несколько методов доступа к свойству могут быть закрытыми.  
  
 **✓ Попробуйте** с помощью обратные вызовы сериализации для инициализации десериализованный экземпляров.  
  
 При десериализации объектов конструкторы не вызываются. (Отсутствуют исключения для правила. Конструкторы коллекций, отмеченные <xref:System.Runtime.Serialization.CollectionDataContractAttribute> вызывается во время десериализации.) Таким образом любой логике, которая выполняется во время обычного построения должен быть реализован как один из обратные вызовы сериализации.  
  
 `OnDeserializedAttribute`является атрибутом, наиболее часто используемые обратного вызова. Кроме того, в семейство входят атрибуты <xref:System.Runtime.Serialization.OnDeserializingAttribute>, <xref:System.Runtime.Serialization.OnSerializingAttribute> и <xref:System.Runtime.Serialization.OnSerializedAttribute>. Их можно использовать для пометки обратных вызовов, которые выполняются перед десериализацией, перед сериализацией и после сериализации соответственно.  
  
 **✓ Попробуйте** с помощью <xref:System.Runtime.Serialization.KnownTypeAttribute> для указания конкретных типов, которые должны использоваться при десериализации графа сложного объекта.  
  
 **✓ СДЕЛАТЬ** рассмотрите возможность прямой и обратной совместимости при создании или изменении сериализуемые типы.  
  
 Необходимо помнить, что сериализуемые потоки будущих версий типа могут быть десериализованы в текущую версию типа и наоборот.  
  
 Убедитесь, что вы понимаете, что элементы данных, даже частные и внутренние, нельзя изменить их имена, типы или даже их порядок в будущих версиях типа, не специальные меры, чтобы сохранить контракт с использованием явных параметров для атрибутов контракта данных .  
  
 Тестирование совместимости сериализации при внесении изменений в сериализуемые типы. Попробуйте десериализовать новую версию в предыдущую и наоборот.  
  
 **✓ Попробуйте** реализации <xref:System.Runtime.Serialization.IExtensibleDataObject> разрешающее полной совместимости между различными версиями типа.  
  
 Интерфейс дает сериализатору гарантию отсутствия потерь данных при выполнении циклов обработки значений. <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A?displayProperty=nameWithType> Свойство используется для хранения любых данных из будущей версии типа, неизвестный до текущей версии, и поэтому не могут быть сохранены в элементах данных. Если текущая версия впоследствии сериализуется и десериализуется в будущей версии, дополнительные данные будут доступны в сериализованном потоке.  
  
## <a name="supporting-xml-serialization"></a>Поддержка XML-сериализации  
 Сериализации контракта данных является основным (по умолчанию) технологию сериализации в .NET Framework, однако существуют сценарии сериализации, сериализации контракта данных не поддерживает. Например, она не обеспечивает полный контроль над формой XML-кода, создаваемого или используемого сериализатором. При необходимости такие улучшения управления, имеет XML-сериализации для использования и вам нужно создать типы для поддержки этой технологии сериализации.  
  
 **X ИЗБЕГАЙТЕ** полученных проектирование к типам специально для сериализации XML, если нет очень сильный причины для контроля формы XML. Эта технология сериализации была заменена сериализацией контракта данных, описанной в предыдущем разделе.  
  
 **✓ Попробуйте** реализации <xref:System.Xml.Serialization.IXmlSerializable> интерфейс, если требуется, чтобы еще больше возможностей контроля формы сериализованного XML чем предлагаемый путем применения атрибутов XML-сериализации. Два метода интерфейса, <xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> и <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A>, позволяют полностью управлять сериализованного потока XML. Можно также управлять схемы XML, которая получает созданный для типа, применяя `XmlSchemaProviderAttribute`.  
  
## <a name="supporting-runtime-serialization"></a>Поддержка сериализации во время выполнения  
 Сериализация во время выполнения — это технология, используемой в удаленном взаимодействии .NET. Если вы считаете, что типы передаются с помощью удаленного взаимодействия .NET, необходимо убедитесь, что они поддерживают сериализацию во время выполнения.  
  
 Базовую поддержку сериализации среды выполнения могут предоставляться с применением <xref:System.SerializableAttribute>, и более сложных сценариев, включающих реализация простой шаблон сериализуемые среды выполнения (реализовать <xref:System.Runtime.Serialization.ISerializable> и предоставляют конструктора сериализации).  
  
 **✓ Попробуйте** поддерживающий сериализацию во время выполнения, если к типам, которые будут использоваться при удаленном взаимодействии .NET. Например <xref:System.AddIn?displayProperty=nameWithType> пространства имен использует удаленное взаимодействие .NET и обмениваются таким образом, все типы `System.AddIn` надстроек должны поддерживать сериализацию во время выполнения.  
  
 **✓ Попробуйте** реализации шаблона сериализуемые среды выполнения, если требуется полный контроль над процессом сериализации. Например, в том случае, когда необходимо преобразовать сериализуемые или десериализуемые данные.  
  
 Шаблон очень прост. Нужно всего лишь реализовать интерфейс <xref:System.Runtime.Serialization.ISerializable> и предусмотреть специальный конструктор, который будет использоваться при десериализации объекта.  
  
 **✓ СДЕЛАТЬ** защищенный конструктор сериализации и предоставляют два параметра типизированных и именованных точно так, как показано в примере ниже.  
  
```  
[Serializable]  
public class Person : ISerializable {  
    protected Person(SerializationInfo info, StreamingContext context) {  
        ...  
    }  
}  
```  
  
 **СДЕЛАТЬ ✓** реализовать `ISerializable` члены явным образом.  
  
 **СДЕЛАТЬ ✓** запрос компоновки, чтобы применить <xref:System.Runtime.Serialization.ISerializable.GetObjectData%2A?displayProperty=nameWithType> реализации. Это гарантирует, что только с полным доверием core и сериализатор среды выполнения имеют доступ к элементу.  
  
 *Фрагменты © 2005, 2009 корпорации Майкрософт. Все права защищены.*  
  
 *Перепечатываются разрешении Пирсона для образовательных учреждений, Inc. из [Framework рекомендации по проектированию: условные обозначения, стили и шаблоны для библиотеки .NET для повторного использования, 2-е издание](http://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) Krzysztof Cwalina и Брэд Абрамс, опубликованные 22 октября 2008 г., Addison-Wesley Professional в составе ряда разработки Microsoft Windows.*  
  
## <a name="see-also"></a>См. также  
 [Рекомендации по проектированию на основе Framework](../../../docs/standard/design-guidelines/index.md)  
 [Рекомендации по использованию](../../../docs/standard/design-guidelines/usage-guidelines.md)