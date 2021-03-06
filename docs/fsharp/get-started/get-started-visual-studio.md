---
title: Начало работы с F# в Visual Studio
description: Сведения об использовании F# с помощью Visual Studio.
ms.date: 07/03/2018
ms.openlocfilehash: 3dac8466501338873aeb308ceac9274a7934a8a9
ms.sourcegitcommit: db8b83057d052c1f9f249d128b08d4423af0f7c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/02/2018
ms.locfileid: "43872855"
---
# <a name="get-started-with-f-in-visual-studio"></a>Начало работы с F# в Visual Studio

F# и инструментария Visual F# поддерживаются в Интегрированной среде разработки Visual Studio.

Чтобы начать, убедитесь, что у вас есть [Visual Studio, установленной с помощью F#](install-fsharp.md#install-f-with-visual-studio).

## <a name="creating-a-console-application"></a>Создание консольного приложения

Одним из основных проектов в Visual Studio — это консольное приложение.  Вот как это делается.  После открытия Visual Studio:

1. На **файл** последовательно выберите пункты **New**, а затем выберите **проекта**.

2.  В New Project диалоговое окно, в разделе **шаблоны**, вы должны увидеть **Visual F#**.  Выберите, чтобы отобразить шаблоны F#.

3. Выберите либо **.NET Core, консольное приложение** или **консольное приложение**.

3. Выберите **хорошо** кнопку, чтобы создать проект F#!  Теперь вы увидите проекта F# в обозревателе решений.

## <a name="writing-your-code"></a>Написание кода

Давайте начнем сначала пишет некоторый код.  Убедитесь, что `Program.fs` файл открыт и замените его содержимое следующим:

[!code-fsharp[HelloSquare](../../../samples/snippets/fsharp/getting-started/hello-square.fs)]

В предыдущем примере кода, функция `square` был определен который принимает ввод с именем `x` и умножает его самостоятельно.  Так как F# использует [вывод типа](../language-reference/type-inference.md), тип `x` не должны быть указаны.  Компилятор F# понимает типы, где умножение является допустимым и будет назначать тип для `x` зависит `square` вызывается.  Если навести `square`, вы должны увидеть следующее:

```
val square: x:int -> int
```

Это связано с так называемой сигнатура типа функции.  Может быть прочитан следующим образом: «квадрата является функция, которая принимает целое число с именем x и создает целое число».  Обратите внимание, что компилятор обеспечивал `square` `int` тип сейчас - это обусловлено умножения, не является универсальным через *все* типы, но довольно универсален через набор закрытых типов.  Компилятор F# выбраны `int` это точка, но он настроит сигнатуре типа при вызове метода `square` с другим ввода типа, например `float`.

Другую функцию `main`, определяется, который дополняется `EntryPoint` атрибут для указания компилятору F#, выполнение программы следует начать с этого.  Его следует тому же образцу что и другие [языков программирования в стиле C](https://en.wikipedia.org/wiki/Entry_point#C_and_C.2B.2B), где эту функцию можно передать аргументы командной строки и возвращается код целого числа (обычно `0`).

В этой функции, мы вызываем `square` функцию с аргументом `12`.  Компилятор F# затем назначает тип `square` быть `int -> int` (то есть функция, которая принимает `int` и создает `int`).  Вызов `printfn` является форматированный печати функция, которая использует строку формата, аналогичную языков программирования C-стиля, параметры, которые соответствуют заданным в строке формата, а затем распечатывает результат и новую строку.

## <a name="running-your-code"></a>Выполнение кода

Можно выполнить код и увидеть результаты, нажав клавишу **Ctrl**+**F5**.  Это запускает программу без отладки и дает возможность видеть результаты.  Кроме того, вы можете **Отладка** меню верхнего уровня элемент в Visual Studio, выбрав **Запуск без отладки**.

Теперь вы увидите напечатаны в окне консоли, Visual Studio появилось следующее:

```
12 squared is 144!
```

Поздравляем!  Вы создали первый проект F# в Visual Studio, написаны функция F#, распечатать результаты вызова этой функции и запустите проект, чтобы увидеть некоторые результаты.

## <a name="next-steps"></a>Следующие шаги

Если это еще не сделано, см. статью [обзор языка F#](../tour.md), где приведены некоторые основные возможности языка F#.  Он будет дам обзор некоторых возможностей языка F# и предоставляет примеры кода можно в полной мере, можно скопировать в Visual Studio и запустить.  Существуют также некоторые полезные внешние ресурсы, которые можно использовать, показывавшие в [руководство по F#](../index.md).

## <a name="see-also"></a>См. также

- [Обзор языка F#](../tour.md)
- [Справочник по языку F#](../language-reference/index.md)
- [Вывод типа](../language-reference/type-inference.md)
- [Справочник символов и оператор](../language-reference/symbol-and-operator-reference/index.md)
