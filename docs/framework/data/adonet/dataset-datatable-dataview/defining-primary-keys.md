---
title: Определение первичных ключей
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 2ea85959-e763-4669-8bd9-46a9dab894bd
ms.openlocfilehash: aecd16357857dca7393eac879159c916d8cf8ac7
ms.sourcegitcommit: 2eceb05f1a5bb261291a1f6a91c5153727ac1c19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2018
ms.locfileid: "43520222"
---
# <a name="defining-primary-keys"></a>Определение первичных ключей
База данных обычно содержит столбец или группу столбцов, уникально определяющих каждую строку в таблице. Такие столбцы или группы столбцов называются первичными ключами.  
  
 При определении одного <xref:System.Data.DataColumn> как <xref:System.Data.DataTable.PrimaryKey%2A> для <xref:System.Data.DataTable>, в таблице автоматически присваивается <xref:System.Data.DataColumn.AllowDBNull%2A> свойство столбца **false** и <xref:System.Data.DataColumn.Unique%2A> свойства  **значение true,**. Для нескольких столбцов первичных ключей, только **AllowDBNull** свойству автоматически присваивается **false**.  
  
 **PrimaryKey** свойство <xref:System.Data.DataTable> получает в качестве значения массив из одного или нескольких **DataColumn** объектов, как показано в следующих примерах. В первом примере в качестве первичного ключа определяется один столбец.  
  
```vb  
workTable.PrimaryKey = New DataColumn() {workTable.Columns("CustID")}  
  
' Or  
  
Dim columns(1) As DataColumn  
columns(0) = workTable.Columns("CustID")  
workTable.PrimaryKey = columns  
```  
  
```csharp  
workTable.PrimaryKey = new DataColumn[] {workTable.Columns["CustID"]};  
  
// Or  
  
DataColumn[] columns = new DataColumn[1];  
columns[0] = workTable.Columns["CustID"];  
workTable.PrimaryKey = columns;  
```  
  
 Следующий пример определяет два столбца в качестве первичного ключа.  
  
```vb  
workTable.PrimaryKey = New DataColumn() {workTable.Columns("CustLName"), _  
                                         workTable.Columns("CustFName")}  
  
' Or  
  
Dim keyColumn(2) As DataColumn  
keyColumn(0) = workTable.Columns("CustLName")  
keyColumn(1) = workTable.Columns("CustFName")  
workTable.PrimaryKey = keyColumn  
```  
  
```csharp  
workTable.PrimaryKey = new DataColumn[] {workTable.Columns["CustLName"],   
                                         workTable.Columns["CustFName"]};  
  
// Or  
  
DataColumn[] keyColumn = new DataColumn[2];  
keyColumn[0] = workTable.Columns["CustLName"];  
keyColumn[1] = workTable.Columns["CustFName"];  
workTable.PrimaryKey = keyColumn;  
```  
  
## <a name="see-also"></a>См. также  
 <xref:System.Data.DataTable>  
 [Определение схемы DataTable](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/datatable-schema-definition.md)  
 [DataTables](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/datatables.md)  
 [Центр разработчиков наборов данных и управляемых поставщиков ADO.NET](https://go.microsoft.com/fwlink/?LinkId=217917)
