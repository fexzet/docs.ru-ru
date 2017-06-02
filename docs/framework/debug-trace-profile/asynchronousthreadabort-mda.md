---
title: "asynchronousThreadAbort MDA | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "asynchronous thread aborts"
  - "AsynchronousThreadAbort MDA"
  - "managed debugging assistants (MDAs), asynchronous thread aborts"
  - "threading [.NET Framework], managed debugging assistants"
  - "MDAs (managed debugging assistants), asynchronous thread aborts"
ms.assetid: 9ebe40b2-d703-421e-8660-984acc42bfe0
caps.latest.revision: 10
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 10
---
# asynchronousThreadAbort MDA
Помощник по отладке управляемого кода \(MDA\) `asynchronousThreadAbort` активируется в том случае, если поток пытается выполнить асинхронное аварийное завершение работы в другом потоке.  В случае синхронного завершения работы потока, помощник по отладке управляемого кода `asynchronousThreadAbort` не активируется.  
  
## Признаки  
 При аварийном завершении работы потока основного приложения происходит сбой приложения и возникает необработанное исключение <xref:System.Threading.ThreadAbortException>.  Если приложение в этот момент продолжило работу, последствия могут быть более серьезными, чем сбой работы приложения. Вероятно, в результате будут повреждены данные.  
  
 Операции, которые предполагаются атомарными, скорее всего, были прерваны, завершившись частично, в результате чего данные приложения находятся в непредсказуемом состоянии.  Исключение <xref:System.Threading.ThreadAbortException> может возникать в произвольных точках выполнения кода, зачастую в тех местах, где исключение появляться не должно.  Возможно, код не сможет обработать такое исключение, что приводит к повреждению состояния.  
  
 Вследствие того, что данная проблема носит случайный характер, признаки могут быть разнообразными.  
  
## Причина  
 Код в одном потоке вызвал метод <xref:System.Threading.Thread.Abort%2A?displayProperty=fullName> в целевом потоке, чтобы выполнить асинхронное аварийное завершение работы потока.  Аварийное завершение работы потока является асинхронным, поскольку код, вызвавший метод <xref:System.Threading.Thread.Abort%2A>, выполняется в потоке, отличном от целевого потока.  Синхронное аварийное завершение работы не является проблемой, поскольку работа потока, в котором выполняется метод <xref:System.Threading.Thread.Abort%2A>, была завершена в безопасной контрольной точке с согласованным состоянием приложения.  
  
 Асинхронное аварийное завершение работы потока представляет собой проблему, поскольку оно обрабатывается в произвольных точках выполнения целевого потока.  Чтобы этого избежать, код, предназначенный для выполнения в потоке, работа которого может аварийно завершиться таким образом, должен будет обрабатывать исключение <xref:System.Threading.ThreadAbortException> практически в каждой строке кода и при этом возвращать данные приложения в согласованное состояние.  Вряд ли стоит ожидать, что код будет написан с учетом данной проблемы, или пытаться написать код, который будет способен справиться со всеми возможными последствиями.  
  
 Вызовы в неуправляемом коде и блоках `finally` не будут завершаться в аварийном режиме асинхронно, а непосредственно после выхода из одной из этих категорий.  
  
 Причину этого сложно определить вследствие случайного характера данной проблемы.  
  
## Решение  
 Следует избегать разработки кода, который требует использования асинхронного аварийного завершения работы потока.  Существует несколько приемов прерывания работы целевого потока, которые не требуют вызова метода <xref:System.Threading.Thread.Abort%2A>.  Безопаснее всего применить определенный механизм, например общее свойство, которое дает целевому потоку команду на запрос прерывания.  Целевой поток проверяет команду в определенных безопасных контрольных точках.  Если запрос на прерывание обнаружен, работа потока корректно завершается.  
  
## Влияние на среду выполнения  
 Данный помощник по отладке управляемого кода не оказывает влияния на среду CLR.  Он только сообщает сведения об асинхронных аварийных завершениях работы потока.  
  
## Output  
 Помощник по отладке управляемого кода выводит сведения об идентификаторе потока, выполнившего аварийное завершение работы, а также об идентификаторе целевого потока.  Эти потоки не могут совпадать, поскольку они ограничены асинхронными аварийными завершениями работы.  
  
## Configuration  
  
```  
<mdaConfig>  
  <assistants>  
    <asynchronousThreadAbort />  
  </assistants>  
</mdaConfig>  
```  
  
## Пример  
 Для активации помощника по отладке управляемого кода `asynchronousThreadAbort` требуется только вызов метода <xref:System.Threading.Thread.Abort%2A> в отдельном работающем потоке.  Следует учитывать последствия в случае, если содержимое функции запуска потока настроено для выполнения более сложных операций, которые могут аварийно прерываться в любой произвольной точке.  
  
```  
using System.Threading;  
void FireMda()  
{  
    Thread t = new Thread(delegate() { Thread.Sleep(1000); });  
    t.Start();  
    // The following line activates the MDA.  
    t.Abort();   
    t.Join();  
}  
```  
  
## См. также  
 <xref:System.Threading.Thread>   
 [Diagnosing Errors with Managed Debugging Assistants](../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)