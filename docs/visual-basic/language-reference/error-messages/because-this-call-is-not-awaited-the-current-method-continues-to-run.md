---
title: Поскольку этот вызов не ожидается, выполнение текущего метода продолжается до завершения вызова.
ms.date: 07/20/2015
f1_keywords:
- bc42358
- vbc42358
helpviewer_keywords:
- BC42358
ms.assetid: 43342515-c3c8-4155-9263-c302afabcbc2
ms.openlocfilehash: fe820b9d2157c09428903a36427d3ff5e4c0045b
ms.sourcegitcommit: 6eac9a01ff5d70c6d18460324c016a3612c5e268
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2018
ms.locfileid: "45619179"
---
# <a name="because-this-call-is-not-awaited-the-current-method-continues-to-run-before-the-call-is-completed"></a>Поскольку этот вызов не ожидается, выполнение текущего метода продолжается до завершения вызова.
Так как этот вызов не ожидается, выполнение существующего метода продолжается до тех пор, пока вызов не будет завершен. Рассмотрите возможность применения оператора Await к результату вызова.  
  
 Текущий метод вызывает асинхронный метод, который возвращает класс <xref:System.Threading.Tasks.Task> или <xref:System.Threading.Tasks.Task%601> и не применяет оператор [Await](../../../visual-basic/language-reference/operators/await-operator.md) к результату. Вызов асинхронного метода запускает асинхронную задачу. Однако поскольку оператор `Await` не применяется, программа продолжает выполнение, не ожидая завершения задачи. В большинстве случаев такое поведение не ожидается. Обычно другие аспекты вызывающего метода зависят от результатов вызова или, как минимум, вызывающий метод должен завершиться до возврата из метода, который содержит вызов.  
  
 Также важной проблемой является обработка исключений, возникающих в вызываемом асинхронном методе. Исключение, возникающее в методе, который возвращает <xref:System.Threading.Tasks.Task> или  <xref:System.Threading.Tasks.Task%601> , хранится в возвращенной задаче. Если не ожидать задачу или явно не проверять исключения, исключение будет потеряно. Если ожидать задачу, ее исключение будет создано повторно.  
  
 Рекомендуется всегда ожидать вызов.  
  
 По умолчанию данное сообщение является предупреждением. Дополнительные сведения о скрытии предупреждений или обработке предупреждений как ошибок см. в разделе [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **Идентификатор ошибки:** BC42358  
  
### <a name="to-address-this-warning"></a>Устранение предупреждения  
  
-   Рекомендуется отключать предупреждение, только если вы уверены, что не нужно ожидать завершения асинхронного вызова и что вызванный метод не создаст исключения. В этом случае можно отключить предупреждение, присвоив переменной результат вызова задачи.  
  
     В следующем примере показано, как вызвать предупреждение, как отключить его и как ожидать вызов.  
  
    ```vb  
    Async Function CallingMethodAsync() As Task  
  
        ResultsTextBox.Text &= vbCrLf & "  Entering calling method."  
  
        ' Variable delay is used to slow down the called method so that you  
        ' can distinguish between awaiting and not awaiting in the program's output.   
        ' You can adjust the value to produce the output that this topic shows   
        ' after the code.  
        Dim delay = 5000  
  
        ' Call #1.  
        ' Call an async method. Because you don't await it, its completion isn't   
        ' coordinated with the current method, CallingMethodAsync.  
        ' The following line causes the warning.  
        CalledMethodAsync(delay)  
  
        ' Call #2.  
        ' To suppress the warning without awaiting, you can assign the   
        ' returned task to a variable. The assignment doesn't change how  
        ' the program runs. However, the recommended practice is always to  
        ' await a call to an async method.  
        ' Replace Call #1 with the following line.  
        'Task delayTask = CalledMethodAsync(delay)  
  
        ' Call #3  
        ' To contrast with an awaited call, replace the unawaited call   
        ' (Call #1 or Call #2) with the following awaited call. The best   
        ' practice is to await the call.  
  
        'Await CalledMethodAsync(delay)  
  
        ' If the call to CalledMethodAsync isn't awaited, CallingMethodAsync  
        ' continues to run and, in this example, finishes its work and returns  
        ' to its caller.  
        ResultsTextBox.Text &= vbCrLf & "  Returning from calling method."  
    End Function  
  
    Async Function CalledMethodAsync(howLong As Integer) As Task  
  
        ResultsTextBox.Text &= vbCrLf & "    Entering called method, starting and awaiting Task.Delay."  
        ' Slow the process down a little so you can distinguish between awaiting  
        ' and not awaiting. Adjust the value for howLong if necessary.  
        Await Task.Delay(howLong)  
        ResultsTextBox.Text &= vbCrLf & "    Task.Delay is finished--returning from called method."  
    End Function  
    ```  
  
     В этом примере при выборе Call #1 или Call #2, неожидаемый асинхронный метод (`CalledMethodAsync`) завершается после того, как завершатся вызвавший его метод (`CallingMethodAsync`) и метод, вызвавший этот метод (`StartButton_Click`). Последняя строка в следующем выводе показывает, когда завершается вызванный метод. Вход и выход из обработчика событий, который вызывает `CallingMethodAsync` , в полном примере помечены в выводе.  
  
    ```  
    Entering the Click event handler.  
      Entering calling method.  
        Entering called method, starting and awaiting Task.Delay.  
      Returning from calling method.  
    Exiting the Click event handler.  
        Task.Delay is finished--returning from called method.  
    ```  
  
## <a name="example"></a>Пример  
 Следующее приложение Windows Presentation Foundation (WPF) содержит методы из предыдущего примера. Ниже описана настройка приложения.  
  
1.  Создайте приложение WPF с именем `AsyncWarning`.  
  
2.  В редакторе кода Visual Studio перейдите на вкладку **MainWindow.xaml** .  
  
     Если вкладка не отображается, откройте контекстное меню для MainWindow.xaml в **Обозревателе решений**и выберите пункт **Просмотреть код**.  
  
3.  Замените код в представлении **XAML** файла MainWindow.xaml на следующий.  
  
    ```vb  
    <Window x:Class="MainWindow"  
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"  
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  
            Title="MainWindow" Height="350" Width="525">  
        <Grid>  
            <Button x:Name="StartButton" Content="Start" HorizontalAlignment="Left" Margin="214,28,0,0" VerticalAlignment="Top" Width="75" HorizontalContentAlignment="Center" FontWeight="Bold" FontFamily="Aharoni" Click="StartButton_Click" />  
            <TextBox x:Name="ResultsTextBox" Margin="0,80,0,0" TextWrapping="Wrap" FontFamily="Lucida Console"/>  
        </Grid>  
    </Window>  
    ```  
  
     В представлении **Конструктор** файла MainWindow.xaml появится простое окно, содержащее кнопку и текстовое поле.  
  
     Дополнительные сведения о конструкторе XAML см. в разделе [Создание пользовательского интерфейса с помощью конструктора XAML](/visualstudio/designers/creating-a-ui-by-using-xaml-designer-in-visual-studio). Сведения о том, как построить собственный простой пользовательский интерфейс, см. в подразделах "Создание приложений WPF" и "Создание простого MainWindow WPF" в разделе [Пошаговое руководство. Получение доступа к Интернету с помощью модификатора Async и оператора Await](../../../visual-basic/programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md).  
  
4.  Замените код в MainWindow.xaml.vb на приведенный далее.  
  
    ```vb  
    Class MainWindow   
  
        Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)  
  
            ResultsTextBox.Text &= vbCrLf & "Entering the Click event handler."  
            Await CallingMethodAsync()  
            ResultsTextBox.Text &= vbCrLf & "Exiting the Click event handler."  
        End Sub  
  
        Async Function CallingMethodAsync() As Task  
  
            ResultsTextBox.Text &= vbCrLf & "  Entering calling method."  
  
            ' Variable delay is used to slow down the called method so that you  
            ' can distinguish between awaiting and not awaiting in the program's output.   
            ' You can adjust the value to produce the output that this topic shows   
            ' after the code.  
            Dim delay = 5000  
  
            ' Call #1.  
            ' Call an async method. Because you don't await it, its completion isn't   
            ' coordinated with the current method, CallingMethodAsync.  
            ' The following line causes the warning.  
            CalledMethodAsync(delay)  
  
            ' Call #2.  
            ' To suppress the warning without awaiting, you can assign the   
            ' returned task to a variable. The assignment doesn't change how  
            ' the program runs. However, the recommended practice is always to  
            ' await a call to an async method.  
  
            ' Replace Call #1 with the following line.  
            'Task delayTask = CalledMethodAsync(delay)  
  
            ' Call #3  
            ' To contrast with an awaited call, replace the unawaited call   
            ' (Call #1 or Call #2) with the following awaited call. The best   
            ' practice is to await the call.  
  
            'Await CalledMethodAsync(delay)  
  
            ' If the call to CalledMethodAsync isn't awaited, CallingMethodAsync  
            ' continues to run and, in this example, finishes its work and returns  
            ' to its caller.  
            ResultsTextBox.Text &= vbCrLf & "  Returning from calling method."  
        End Function  
  
        Async Function CalledMethodAsync(howLong As Integer) As Task  
  
            ResultsTextBox.Text &= vbCrLf & "    Entering called method, starting and awaiting Task.Delay."  
            ' Slow the process down a little so you can distinguish between awaiting  
            ' and not awaiting. Adjust the value for howLong if necessary.  
            Await Task.Delay(howLong)  
            ResultsTextBox.Text &= vbCrLf & "    Task.Delay is finished--returning from called method."  
        End Function  
  
    End Class  
  
    ' Output  
  
    ' Entering the Click event handler.  
    '   Entering calling method.  
    '     Entering called method, starting and awaiting Task.Delay.  
    '   Returning from calling method.  
    ' Exiting the Click event handler.  
    '     Task.Delay is finished--returning from called method.  
  
    ' Output  
  
    ' Entering the Click event handler.  
    '   Entering calling method.  
    '     Entering called method, starting and awaiting Task.Delay.  
    '     Task.Delay is finished--returning from called method.  
    '   Returning from calling method.  
    ' Exiting the Click event handler.  
    ```  
  
5.  Нажмите клавишу F5, чтобы запустить программу, а затем нажмите кнопку **Start** .  
  
     Ожидаемый результат отобразится в конце кода.  
  
## <a name="see-also"></a>См. также

- [Оператор Await](../../../visual-basic/language-reference/operators/await-operator.md)  
- [Асинхронное программирование с использованием ключевых слов Async и Await](../../../visual-basic/programming-guide/concepts/async/index.md)
