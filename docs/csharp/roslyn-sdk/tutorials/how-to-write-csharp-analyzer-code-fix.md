---
title: Руководство. Создание анализатора и исправления кода
description: В этом руководстве описано, как создать анализатор и исправление кода с помощью пакета SDK для .NET Compiler Platform (API Roslyn).
ms.date: 08/01/2018
ms.custom: mvc
ms.openlocfilehash: 2959fe3008bfca972d3a164ed27d05c2a8b0e69a
ms.sourcegitcommit: fb78d8abbdb87144a3872cf154930157090dd933
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2018
ms.locfileid: "47398002"
---
# <a name="tutorial-write-your-first-analyzer-and-code-fix"></a><span data-ttu-id="dc8b7-103">Руководство. Создание анализатора и исправления кода</span><span class="sxs-lookup"><span data-stu-id="dc8b7-103">Tutorial: Write your first analyzer and code fix</span></span>

<span data-ttu-id="dc8b7-104">Пакет SDK для .NET Compiler Platform предоставляет инструменты для создания пользовательских предупреждений для кода C# или Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-104">The .NET Compiler Platform SDK provides the tools you need to create custom warnings that target C# or Visual Basic code.</span></span> <span data-ttu-id="dc8b7-105">**Анализатор** содержит код, который распознает нарушения правила.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-105">Your **analyzer** contains code that recognizes violations of your rule.</span></span> <span data-ttu-id="dc8b7-106">**Исправление кода** содержит код, который исправляет эти нарушения.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-106">Your **code fix** contains the code that fixes the violation.</span></span> <span data-ttu-id="dc8b7-107">Правилами, которые вы реализуете, может быть что угодно — от структуры кода до его стиля или соглашений об именовании и многое другое.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-107">The rules you implement can be anything from code structure to coding style to naming conventions and more.</span></span> <span data-ttu-id="dc8b7-108">.NET Compiler Platform предоставляет платформу для выполнения анализа во время написания кода, а также все функции пользовательского интерфейса Visual Studio для отладки, включая отображение волнистых линий в редакторе, вывод списка ошибок в Visual Studio и отображение значка лампочки, указывающего на наличие предложений и предлагаемых исправлений.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-108">The .NET Compiler Platform provides the framework for running analysis as developers are writing code, and all the Visual Studio UI features for fixing code: showing squiggles in the editor, populating the Visual Studio Error List, creating the "light bulb" suggestions and showing the rich preview of the suggested fixes.</span></span>

<span data-ttu-id="dc8b7-109">В этом руководстве описано, как создать **анализатор** и соответствующее **исправление кода** с помощью API Roslyn.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-109">In this tutorial, you'll explore the creation of an **analyzer** and an accompanying **code fix** using the Roslyn APIs.</span></span> <span data-ttu-id="dc8b7-110">Анализатор выполняет анализ исходного кода и сообщает о проблеме пользователю.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-110">An analyzer is a way to perform source code analysis and report a problem to the user.</span></span> <span data-ttu-id="dc8b7-111">При необходимости анализатор может предложить соответствующее исправление для исходного кода пользователя.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-111">Optionally, an analyzer can also provide a code fix that represents a modification to the user's source code.</span></span> <span data-ttu-id="dc8b7-112">В этом руководстве описано, как создать анализатор, ищущий локальные переменные, которые можно объявить с помощью модификатора `const`, но которые не объявлены.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-112">This tutorial creates an analyzer that finds local variable declarations that could be declared using the `const` modifier but are not.</span></span> <span data-ttu-id="dc8b7-113">Сопутствующее исправление кода добавляет модификатор `const` в эти объявления.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-113">The accompanying code fix modifies those declarations to add the `const` modifier.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dc8b7-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="dc8b7-114">Prerequisites</span></span>

* [<span data-ttu-id="dc8b7-115">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="dc8b7-115">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads)

<span data-ttu-id="dc8b7-116">Вам нужно установить **пакет SDK для .NET Compiler Platform**:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-116">You'll need to install the **.NET Compiler Platform SDK**:</span></span>

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

<span data-ttu-id="dc8b7-117">Создать и проверить анализатор можно несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-117">There are several steps to creating and validating your analyzer:</span></span>

1. <span data-ttu-id="dc8b7-118">Создайте решение.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-118">Create the solution.</span></span>
1. <span data-ttu-id="dc8b7-119">Зарегистрируйте имя и описание анализатора.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-119">Register the analyzer name and description.</span></span>
1. <span data-ttu-id="dc8b7-120">Создайте отчет анализатора о предупреждениях и рекомендациях.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-120">Report analyzer warnings and recommendations.</span></span>
1. <span data-ttu-id="dc8b7-121">Внедрите исправление кода, чтобы принимать рекомендации.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-121">Implement the code fix to accept recommendations.</span></span>
1. <span data-ttu-id="dc8b7-122">Улучшите анализ с помощью модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-122">Improve the analysis through unit tests.</span></span>

## <a name="explore-the-analyzer-template"></a><span data-ttu-id="dc8b7-123">Изучение шаблона анализатора</span><span class="sxs-lookup"><span data-stu-id="dc8b7-123">Explore the analyzer template</span></span>

<span data-ttu-id="dc8b7-124">Анализатор сообщает пользователю о любых объявлениях локальной переменной, которые можно преобразовать в локальные константы.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-124">Your analyzer reports to the user any local variable declarations that can be converted to local constants.</span></span> <span data-ttu-id="dc8b7-125">Рассмотрим следующий пример кода:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-125">For example, consider the following code:</span></span>

```csharp
int x = 0;
Console.WriteLine(x);
```

<span data-ttu-id="dc8b7-126">В приведенном выше коде `x` присваивается значение константы, которое никогда не меняется.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-126">In the code above, `x` is assigned a constant value and is never modified.</span></span> <span data-ttu-id="dc8b7-127">Ее можно объявить с помощью модификатора `const`:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-127">It can be declared using the `const` modifier:</span></span>

```csharp
const int x = 0;
Console.WriteLine(x);
```

<span data-ttu-id="dc8b7-128">Чтобы определить, можно ли изменить переменную на константу, используется синтаксический анализ, анализ константы из выражения инициализатора, а также анализ потока данных, чтобы убедиться, что переменная не записана.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-128">The analysis to determine whether a variable can be made constant is involved, requiring syntactic analysis, constant analysis of the initializer expression and dataflow analysis to ensure that the variable is never written to.</span></span> <span data-ttu-id="dc8b7-129">.NET Compiler Platform предоставляет API для упрощения такого анализа.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-129">The .NET Compiler Platform provides APIs that make it easier to perform this analysis.</span></span> <span data-ttu-id="dc8b7-130">Сначала нужно создать новый проект C# **анализатора с исправлением кода**.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-130">The first step is to create a new C# **Analyzer with code fix** project.</span></span>

* <span data-ttu-id="dc8b7-131">В Visual Studio последовательно выберите **Файл > Создать > Проект**, чтобы открыть диалоговое окно "Новый проект".</span><span class="sxs-lookup"><span data-stu-id="dc8b7-131">In Visual Studio, choose **File > New > Project...** to display the New Project dialog.</span></span>
* <span data-ttu-id="dc8b7-132">В разделе **Visual C# > Расширяемость** выберите **Analyzer with code fix (.NET Standard)** (Анализатор с исправлением кода (.NET Standard)).</span><span class="sxs-lookup"><span data-stu-id="dc8b7-132">Under **Visual C# > Extensibility**, choose **Analyzer with code fix (.NET Standard)**.</span></span>
* <span data-ttu-id="dc8b7-133">Присвойте проекту имя **MakeConst** и нажмите кнопку "ОК".</span><span class="sxs-lookup"><span data-stu-id="dc8b7-133">Name your project "**MakeConst**" and click OK.</span></span>

<span data-ttu-id="dc8b7-134">Анализатор с шаблоном исправления кода создаст три проекта: один содержит анализатор и исправление кода, второй — проект модульного теста и третий — проект VSIX.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-134">The analyzer with code fix template creates three projects: one contains the analyzer and code fix, the second is a unit test project, and the third is the VSIX project.</span></span> <span data-ttu-id="dc8b7-135">Запускаемым проектом по умолчанию является проект VSIX.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-135">The default startup project is the VSIX project.</span></span> <span data-ttu-id="dc8b7-136">Нажмите клавишу **F5**, чтобы запустить проект VSIX.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-136">Press **F5** to start the VSIX project.</span></span> <span data-ttu-id="dc8b7-137">Это запустит второй экземпляр Visual Studio с загруженным в него анализатором.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-137">This starts a second instance of Visual Studio that has loaded your new analyzer.</span></span>

> [!TIP]
> <span data-ttu-id="dc8b7-138">Когда вы запустите анализатор, откроется вторая копия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-138">When you run your analyzer, you start a second copy of Visual Studio.</span></span> <span data-ttu-id="dc8b7-139">Эта вторая копия использует другой куст реестра для хранения параметров,</span><span class="sxs-lookup"><span data-stu-id="dc8b7-139">This second copy uses a different registry hive to store settings.</span></span> <span data-ttu-id="dc8b7-140">что позволяет различить параметры визуальных элементов в обоих копиях Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-140">That enables you to differentiate the visual settings in the two copies of Visual Studio.</span></span> <span data-ttu-id="dc8b7-141">Вы можете выбрать другую тему для экспериментального запуска Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-141">You can pick a different theme for the experimental run of Visual Studio.</span></span> <span data-ttu-id="dc8b7-142">Кроме того, не следует перемещать параметры или выполнять вход в учетную запись Visual Studio в экспериментальном экземпляре Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-142">In addition, don't roam your settings or login to your Visual Studio account using the experimental run of Visual Studio.</span></span> <span data-ttu-id="dc8b7-143">Так параметры останутся разными.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-143">That keeps the settings different.</span></span>

<span data-ttu-id="dc8b7-144">Во втором экземпляре Visual Studio, который вы только что создали, создайте проект C# консольного приложения (это может быть как проект .NET Core, так и .NET Framework, так как анализаторы работают на уровне источника). Наведите указатель мыши на токен с волнистым подчеркиванием. Появится текст предупреждения от анализатора.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-144">In the second Visual Studio instance that you just started, create a new C# Console Application project (either .NET Core or .NET Framework project will work -- analyzers work at the source level.) Hover over the token with a wavy underline, and the warning text provided by an analyzer appears.</span></span>

<span data-ttu-id="dc8b7-145">Шаблон создает анализатор, который выдает предупреждение на каждое объявление типа, где имя типа состоит из букв нижнего регистра, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-145">The template creates an analyzer that reports a warning on each type declaration where the type name contains lowercase letters, as shown in the following figure:</span></span>

![Предупреждение от анализатора](media/how-to-write-csharp-analyzer-code-fix/report-warning.png)

<span data-ttu-id="dc8b7-147">Также шаблон содержит исправление кода, которое меняет любые буквы нижнего регистра в имени типа на буквы верхнего регистра.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-147">The template also provides a code fix that changes any type name containing lower case characters to all upper case.</span></span> <span data-ttu-id="dc8b7-148">Предлагаемые исправления можно просмотреть, щелкнув значок лампочки рядом с предупреждением.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-148">You can click on the light bulb displayed with the warning to see the suggested changes.</span></span> <span data-ttu-id="dc8b7-149">После принятия изменений имя типа и все ссылки на этот тип будут обновлены.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-149">Accepting the suggested changes updates the type name and all references to that type in the solution.</span></span> <span data-ttu-id="dc8b7-150">Теперь, когда вы увидели работу начального анализатора, закройте второй экземпляр Visual Studio и вернитесь к проекту анализатора.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-150">Now that you've seen the initial analyzer in action, close the second Visual Studio instance and return to your analyzer project.</span></span>

<span data-ttu-id="dc8b7-151">Для тестирования изменений в анализаторе не требуется каждый раз запускать вторую копию Visual Studio,</span><span class="sxs-lookup"><span data-stu-id="dc8b7-151">You don't have to start a second copy of Visual Studio and create new code to test every change in your analyzer.</span></span> <span data-ttu-id="dc8b7-152">так как шаблон создает проект модульного теста.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-152">The template also creates a unit test project for you.</span></span> <span data-ttu-id="dc8b7-153">В этом проекте содержатся два теста.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-153">That project contains two tests.</span></span> <span data-ttu-id="dc8b7-154">`TestMethod1` показывает обычный формат теста, при котором анализ кода происходит без активации диагностики.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-154">`TestMethod1` shows the typical format of a test that analyzes code without triggering a diagnostic.</span></span> <span data-ttu-id="dc8b7-155">`TestMethod2` — формат, при котором сначала активируется диагностика, а затем применяется исправление кода.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-155">`TestMethod2` shows the format of a test that triggers a diagnostic, and then applies a suggested code fix.</span></span> <span data-ttu-id="dc8b7-156">Во время сборки анализатора и исправления кода вы напишете тесты для проверки разных структур.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-156">As you build your analyzer and code fix, you'll write tests for different code structures to verify your work.</span></span> <span data-ttu-id="dc8b7-157">Модульные тесты проводятся гораздо быстрее, чем интерактивное тестирование анализаторов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-157">Unit tests for analyzers are much quicker than testing them interactively with Visual Studio.</span></span>

> [!TIP]
> <span data-ttu-id="dc8b7-158">Модульные тесты анализатора — отличный инструмент, если вы знаете, какие конструкции кода должны и не должны запускать анализатор.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-158">Analyzer unit tests are a great tool when you know what code constructs should and shouldn't trigger your analyzer.</span></span> <span data-ttu-id="dc8b7-159">В свою очередь запуск анализатора в другой копии Visual Studio позволяет определить и найти конструкции, о которых вы еще не задумывались.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-159">Loading your analyzer in another copy of Visual Studio is a great tool to explore and find constructs you may not have thought about yet.</span></span>

## <a name="create-analyzer-registrations"></a><span data-ttu-id="dc8b7-160">Регистрация анализатора</span><span class="sxs-lookup"><span data-stu-id="dc8b7-160">Create analyzer registrations</span></span>

<span data-ttu-id="dc8b7-161">В файле **MakeConstAnalyzer.cs** с помощью шаблона создается начальный класс `DiagnosticAnalyzer`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-161">The template creates the initial `DiagnosticAnalyzer` class, in the **MakeConstAnalyzer.cs** file.</span></span> <span data-ttu-id="dc8b7-162">В этом начальном анализаторе отображены два важных свойства каждого анализатора.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-162">This initial analyzer shows two important properties of every analyzer.</span></span>

* <span data-ttu-id="dc8b7-163">В каждом диагностическом анализаторе должен быть указан атрибут `[DiagnosticAnalyzer]`, который описывает язык, на котором он работает.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-163">Every diagnostic analyzer must provide a `[DiagnosticAnalyzer]` attribute that describes the language it operates on.</span></span>
* <span data-ttu-id="dc8b7-164">Каждый диагностический анализатор должен наследоваться от класса <xref:Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer>.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-164">Every diagnostic analyzer must derive from the <xref:Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer> class.</span></span>

<span data-ttu-id="dc8b7-165">Также в шаблоне отображены базовые функции любого анализатора:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-165">The template also shows the basic features that are part of any analyzer:</span></span>

1. <span data-ttu-id="dc8b7-166">Регистрация действий.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-166">Register actions.</span></span> <span data-ttu-id="dc8b7-167">Действия представляют изменения кода, которые запускают анализатор для проверки нарушений.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-167">The actions represent code changes that should trigger your analyzer to examine code for violations.</span></span> <span data-ttu-id="dc8b7-168">Когда Visual Studio обнаруживает изменения в коде, которые соответствуют зарегистрированному действию, происходит вызов зарегистрированного метода.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-168">When Visual Studio detects code edits that match a registered action, it calls your analyzer's registered method.</span></span>
1. <span data-ttu-id="dc8b7-169">Создание диагностики.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-169">Create diagnostics.</span></span> <span data-ttu-id="dc8b7-170">Обнаружив нарушения, анализатор создает объект диагностики, с помощью которого Visual Studio уведомляет пользователя об этом нарушении.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-170">When your analyzer detects a violation, it creates a diagnostic object that Visual Studio uses to notify the user of the violation.</span></span>

<span data-ttu-id="dc8b7-171">Действия регистрируются в переопределении метода <xref:Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer.Initialize(Microsoft.CodeAnalysis.Diagnostics.AnalysisContext)?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-171">You register actions in your override of <xref:Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer.Initialize(Microsoft.CodeAnalysis.Diagnostics.AnalysisContext)?displayProperty=nameWithType> method.</span></span> <span data-ttu-id="dc8b7-172">При работе с этим руководстве вы просмотрите **синтаксические узлы** локальных объявлений и узнаете, какие из них имеют значения констант.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-172">In this tutorial, you'll visit **syntax nodes** looking for local declarations, and see which of those have constant values.</span></span> <span data-ttu-id="dc8b7-173">Если объявление может быть константой, анализатор создаст диагностику и сформирует отчет.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-173">If a declaration could be constant, your analyzer will create and report a diagnostic.</span></span>

<span data-ttu-id="dc8b7-174">Сначала обновите константы регистрации и метод `Initialize`, так как константы определяют ваш анализатор MakeConst.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-174">The first step is to update the registration constants and `Initialize` method so these constants indicate your "Make Const" analyzer.</span></span> <span data-ttu-id="dc8b7-175">Большинство строковых констант определены в файле строковых ресурсов.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-175">Most of the string constants are defined in the string resource file.</span></span> <span data-ttu-id="dc8b7-176">Используйте их, чтобы упростить локализацию.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-176">You should follow that practice for easier localization.</span></span> <span data-ttu-id="dc8b7-177">Откройте файл **Resources.resx** для проекта анализатора **MakeConst**.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-177">Open the **Resources.resx** file for the **MakeConst** analyzer project.</span></span> <span data-ttu-id="dc8b7-178">Откроется редактор ресурсов.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-178">This displays the resource editor.</span></span> <span data-ttu-id="dc8b7-179">Измените строковые ресурсы, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-179">Update the string resources as follows:</span></span>

* <span data-ttu-id="dc8b7-180">Компонент `AnalyzerTitle` измените на Variable can be made constant (Переменная может быть константой).</span><span class="sxs-lookup"><span data-stu-id="dc8b7-180">Change `AnalyzerTitle` to "Variable can be made constant".</span></span>
* <span data-ttu-id="dc8b7-181">Компонент `AnalyzerMessageFormat` измените на Can be made constant (Может быть константой).</span><span class="sxs-lookup"><span data-stu-id="dc8b7-181">Change `AnalyzerMessageFormat` to "Can be made constant".</span></span>
* <span data-ttu-id="dc8b7-182">Компонент `AnalyzerDescription` измените на Make Constant (Сделать константой).</span><span class="sxs-lookup"><span data-stu-id="dc8b7-182">Change `AnalyzerDescription` to "Make Constant".</span></span>

<span data-ttu-id="dc8b7-183">Также измените раскрывающийся список **модификатора доступа** на `public`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-183">Also, change the **Access Modifier** drop-down to `public`.</span></span> <span data-ttu-id="dc8b7-184">Это упростит использование этих констант в модульных тестах.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-184">That makes it easier to use these constants in unit tests.</span></span> <span data-ttu-id="dc8b7-185">После настройки редактор ресурсов будет иметь следующий вид:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-185">When you have finished, the resource editor should appear as follow figure shows:</span></span>

![Обновление строковых ресурсов](media/how-to-write-csharp-analyzer-code-fix/update-string-resources.png)

<span data-ttu-id="dc8b7-187">Остальные изменения будут в файле анализатора.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-187">The remaining changes are in the analyzer file.</span></span> <span data-ttu-id="dc8b7-188">Откройте файл **MakeConstAnalyzer.cs** в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-188">Open **MakeConstAnalyzer.cs** in Visual Studio.</span></span> <span data-ttu-id="dc8b7-189">Измените зарегистрированное действие на символы на действие на синтаксис.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-189">Change the registered action from one that acts on symbols to one that acts on syntax.</span></span> <span data-ttu-id="dc8b7-190">В методе `MakeConstAnalyzerAnalyzer.Initialize` найдите строку с действием на символы:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-190">In the `MakeConstAnalyzerAnalyzer.Initialize` method, find the line that registers the action on symbols:</span></span>

```csharp
context.RegisterSymbolAction(AnalyzeSymbol, SymbolKind.NamedType);
```

<span data-ttu-id="dc8b7-191">Замените ее приведенным ниже кодом:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-191">Replace it with the following line:</span></span>

[!code-csharp[Register the node action](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstAnalyzer.cs#RegisterNodeAction "Register a node action")]

<span data-ttu-id="dc8b7-192">После этого метод `AnalyzeSymbol` можно удалить.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-192">After that change, you can delete the `AnalyzeSymbol` method.</span></span> <span data-ttu-id="dc8b7-193">Этот анализатор проверяет <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.LocalDeclarationStatement?displayProperty=nameWithType>, а не операторы <xref:Microsoft.CodeAnalysis.SymbolKind.NamedType?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-193">This analyzer examines <xref:Microsoft.CodeAnalysis.CSharp.SyntaxKind.LocalDeclarationStatement?displayProperty=nameWithType>, not <xref:Microsoft.CodeAnalysis.SymbolKind.NamedType?displayProperty=nameWithType> statements.</span></span> <span data-ttu-id="dc8b7-194">Обратите внимание на то, что `AnalyzeNode` подчеркивается красной волнистой линией,</span><span class="sxs-lookup"><span data-stu-id="dc8b7-194">Notice that `AnalyzeNode` has red squiggles under it.</span></span> <span data-ttu-id="dc8b7-195">так как код, который вы только что вставили, ссылается на метод `AnalyzeNode`, который еще не объявлен.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-195">The code you just added references an `AnalyzeNode` method that hasn't been declared.</span></span> <span data-ttu-id="dc8b7-196">Объявите этот метод, используя приведенный ниже код:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-196">Declare that method using the following code:</span></span>

```csharp
private void AnalyzeNode(SyntaxNodeAnalysisContext context)
{
}
```

<span data-ttu-id="dc8b7-197">В файле **MakeConstAnalyzer.cs** измените `Category` на Usage (Использование), как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-197">Change the `Category` to "Usage" in **MakeConstAnalyzer.cs** as shown in the following code:</span></span>

```csharp
private const string Category = "Usage";
```

## <a name="find-local-declarations-that-could-be-const"></a><span data-ttu-id="dc8b7-198">Поиск локальных объявлений, которые могут быть константами</span><span class="sxs-lookup"><span data-stu-id="dc8b7-198">Find local declarations that could be const</span></span>

<span data-ttu-id="dc8b7-199">Теперь мы напишем первую версию метода `AnalyzeNode`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-199">It's time to write the first version of the `AnalyzeNode` method.</span></span> <span data-ttu-id="dc8b7-200">Он должен найти одно локальное объявление, которое может быть `const`, но таковым не является. Пример приведен ниже:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-200">It should look for a single local declaration that could be `const` but is not, like the following code:</span></span>

```csharp
int x = 0;
Console.WriteLine(x);
```

<span data-ttu-id="dc8b7-201">Сначала найдите локальные объявления.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-201">The first step is to find local declarations.</span></span> <span data-ttu-id="dc8b7-202">Добавьте приведенный ниже код в `AnalyzeNode` в файле **MakeConstAnalyzer.cs**:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-202">Add the following code to `AnalyzeNode` in **MakeConstAnalyzer.cs**:</span></span>

```csharp
var localDeclaration = (LocalDeclarationStatementSyntax)context.Node;
```

<span data-ttu-id="dc8b7-203">Это приведение всегда завершается успешно, так как ваш анализатор зарегистрирован для отслеживания изменений только локальных объявлений.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-203">This cast always succeeds because your analyzer registered for changes to local declarations, and only local declarations.</span></span> <span data-ttu-id="dc8b7-204">Другие типы узлов не вызывают метод `AnalyzeNode`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-204">No other node type triggers a call to your `AnalyzeNode` method.</span></span> <span data-ttu-id="dc8b7-205">Затем проверьте объявление на наличие модификаторов `const`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-205">Next, check the declaration for any `const` modifiers.</span></span> <span data-ttu-id="dc8b7-206">Если они есть, сразу же выполните возврат.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-206">If you find them, return immediately.</span></span> <span data-ttu-id="dc8b7-207">Приведенный ниже код ищет модификаторы `const` в локальном объявлении:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-207">The following code looks for any `const` modifiers on the local declaration:</span></span>

```csharp
// make sure the declaration isn't already const:
if (localDeclaration.Modifiers.Any(SyntaxKind.ConstKeyword))
{
    return;
}
```

<span data-ttu-id="dc8b7-208">В конце проверьте, может ли переменная быть `const`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-208">Finally, you need to check that the variable could be `const`.</span></span> <span data-ttu-id="dc8b7-209">Это означает, что она не может быть назначена после инициализации.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-209">That means making sure it is never assigned after it is initialized.</span></span>

<span data-ttu-id="dc8b7-210">Выполните семантический анализ с помощью <xref:Microsoft.CodeAnalysis.Diagnostics.SyntaxNodeAnalysisContext>.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-210">You'll perform some semantic analysis using the <xref:Microsoft.CodeAnalysis.Diagnostics.SyntaxNodeAnalysisContext>.</span></span> <span data-ttu-id="dc8b7-211">Используйте аргумент `context`, чтобы определить, можно ли объявление локальной переменной сделать `const`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-211">You use the `context` argument to determine whether the local variable declaration can be made `const`.</span></span> <span data-ttu-id="dc8b7-212"><xref:Microsoft.CodeAnalysis.SemanticModel?displayProperty=nameWithType> представляет все семантические сведения в одном исходном файле.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-212">A <xref:Microsoft.CodeAnalysis.SemanticModel?displayProperty=nameWithType> represents of all semantic information in a single source file.</span></span> <span data-ttu-id="dc8b7-213">См. дополнительные сведения о [семантических моделях](../work-with-semantics.md).</span><span class="sxs-lookup"><span data-stu-id="dc8b7-213">You can learn more in the article that covers [semantic models](../work-with-semantics.md).</span></span> <span data-ttu-id="dc8b7-214">С помощью <xref:Microsoft.CodeAnalysis.SemanticModel?displayProperty=nameWithType> выполните анализ потока данных на операторе локального объявления.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-214">You'll use the <xref:Microsoft.CodeAnalysis.SemanticModel?displayProperty=nameWithType> to perform data flow analysis on the local declaration statement.</span></span> <span data-ttu-id="dc8b7-215">Затем, используя результаты этого анализа, убедитесь в том, что локальная переменная не содержит новые значения где-либо еще.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-215">Then, you use the results of this data flow analysis to ensure that the local variable isn't written with a new value anywhere else.</span></span> <span data-ttu-id="dc8b7-216">Вызовите метод расширения <xref:Microsoft.CodeAnalysis.ModelExtensions.GetDeclaredSymbol%2A>, чтобы извлечь <xref:Microsoft.CodeAnalysis.ILocalSymbol> переменной и убедиться, что она не содержится в коллекции <xref:Microsoft.CodeAnalysis.DataFlowAnalysis.WrittenOutside%2A?displayProperty=nameWithType> из анализа потока данных.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-216">Call the <xref:Microsoft.CodeAnalysis.ModelExtensions.GetDeclaredSymbol%2A> extension method to retrieve the <xref:Microsoft.CodeAnalysis.ILocalSymbol> for the variable and check that it isn't contained with the <xref:Microsoft.CodeAnalysis.DataFlowAnalysis.WrittenOutside%2A?displayProperty=nameWithType> collection of the data flow analysis.</span></span> <span data-ttu-id="dc8b7-217">Добавьте в конце метода `AnalyzeNode` приведенный ниже код:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-217">Add the following code to the end of the `AnalyzeNode` method:</span></span>

```csharp
// Perform data flow analysis on the local declaration.
var dataFlowAnalysis = context.SemanticModel.AnalyzeDataFlow(localDeclaration);

// Retrieve the local symbol for each variable in the local declaration
// and ensure that it is not written outside of the data flow analysis region.
var variable = localDeclaration.Declaration.Variables.Single();
var variableSymbol = context.SemanticModel.GetDeclaredSymbol(variable);
if (dataFlowAnalysis.WrittenOutside.Contains(variableSymbol))
{
    return;
}
```

<span data-ttu-id="dc8b7-218">Этот код гарантирует, что переменная не изменена и может быть `const`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-218">The code just added ensures that the variable isn't modified, and can therefore be made `const`.</span></span> <span data-ttu-id="dc8b7-219">Теперь мы активируем диагностику.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-219">It's time to raise the diagnostic.</span></span> <span data-ttu-id="dc8b7-220">Добавьте приведенный ниже код в последнюю строку метода `AnalyzeNode`:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-220">Add the following code as the last line in `AnalyzeNode`:</span></span>

```csharp
context.ReportDiagnostic(Diagnostic.Create(Rule, context.Node.GetLocation()));
```

<span data-ttu-id="dc8b7-221">Чтобы проверить состояние, запустите анализатор, нажав клавишу **F5**.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-221">You can check your progress by pressing **F5** to run your analyzer.</span></span> <span data-ttu-id="dc8b7-222">Загрузите консольное приложение, созданное ранее, и добавьте приведенный ниже код теста:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-222">You can load the console application you created earlier and then add the following test code:</span></span>

```csharp
int x = 0;
Console.WriteLine(x);
```

<span data-ttu-id="dc8b7-223">Появится лампочка и анализатор выдаст отчет с диагностическими сведениями.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-223">The light bulb should appear, and your analyzer should report a diagnostic.</span></span> <span data-ttu-id="dc8b7-224">При этом поведение лампочки по-прежнему основано на исправлении кода, сгенерированном шаблоном, указывая на то, что можно использовать буквы верхнего регистра.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-224">However, the light bulb still uses the template generated code fix, and tells you it can be made upper case.</span></span> <span data-ttu-id="dc8b7-225">В следующем разделе показано, как написать исправление кода.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-225">The next section explains how to write the code fix.</span></span>

## <a name="write-the-code-fix"></a><span data-ttu-id="dc8b7-226">Написание исправления кода</span><span class="sxs-lookup"><span data-stu-id="dc8b7-226">Write the code fix</span></span>

<span data-ttu-id="dc8b7-227">Анализатор поддерживает одно или несколько исправлений кода.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-227">An analyzer can provide one or more code fixes.</span></span> <span data-ttu-id="dc8b7-228">Исправление кода определяет, какие правки нужно внести для решения обнаруженной проблемы.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-228">A code fix defines an edit that addresses the reported issue.</span></span> <span data-ttu-id="dc8b7-229">Исправление кода вашего анализатора предоставляет код с ключевым словом const:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-229">For the analyzer that you created, you can provide a code fix that inserts the const keyword:</span></span>

```csharp
const int x = 0;
Console.WriteLine(x);
```

<span data-ttu-id="dc8b7-230">Пользователь выбирает исправление в интерфейсе лампочки в редакторе, а Visual Studio изменяет код.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-230">The user chooses it from the light bulb UI in the editor and Visual Studio changes the code.</span></span>

<span data-ttu-id="dc8b7-231">Откройте файл **MakeConstCodeFixProvider.cs**, добавленный шаблоном.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-231">Open the **MakeConstCodeFixProvider.cs** file added by the template.</span></span>  <span data-ttu-id="dc8b7-232">Это исправление уже привязано к идентификатору диагностики вашего анализатора, но оно пока не реализует преобразование кода.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-232">This code fix is already wired up to the Diagnostic ID produced by your diagnostic analyzer, but it doesn't yet implement the right code transform.</span></span> <span data-ttu-id="dc8b7-233">Сначала удалите часть кода шаблона.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-233">First you should remove some of the template code.</span></span> <span data-ttu-id="dc8b7-234">Измените строку заголовка на Make constant (Сделать константой):</span><span class="sxs-lookup"><span data-stu-id="dc8b7-234">Change the title string to "Make constant":</span></span>

[!code-csharp[Update the CodeFix title](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#CodeFixTitle "Update the CodeFix title")]

<span data-ttu-id="dc8b7-235">Затем удалите метод `MakeUppercaseAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-235">Next, delete the `MakeUppercaseAsync` method.</span></span> <span data-ttu-id="dc8b7-236">Он больше не применяется.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-236">It no longer applies.</span></span>

<span data-ttu-id="dc8b7-237">Все исправления кода наследуются от <xref:Microsoft.CodeAnalysis.CodeFixes.CodeFixProvider></span><span class="sxs-lookup"><span data-stu-id="dc8b7-237">All code fixes derive from <xref:Microsoft.CodeAnalysis.CodeFixes.CodeFixProvider>.</span></span> <span data-ttu-id="dc8b7-238">и переопределяют <xref:Microsoft.CodeAnalysis.CodeFixes.CodeFixProvider.RegisterCodeFixesAsync(Microsoft.CodeAnalysis.CodeFixes.CodeFixContext)?displayProperty=nameWithType> на сообщение о доступных исправлениях.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-238">They all override <xref:Microsoft.CodeAnalysis.CodeFixes.CodeFixProvider.RegisterCodeFixesAsync(Microsoft.CodeAnalysis.CodeFixes.CodeFixContext)?displayProperty=nameWithType> to report available code fixes.</span></span> <span data-ttu-id="dc8b7-239">В <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax> измените тип узла-предка на `RegisterCodeFixesAsync` в соответствии с диагностикой:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-239">In `RegisterCodeFixesAsync`, change the ancestor node type you're searching for to a <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LocalDeclarationStatementSyntax> to match the diagnostic:</span></span>

[!code-csharp[Find local declaration node](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#FindDeclarationNode  "Find the local declaration node that raised the diagnostic")]

<span data-ttu-id="dc8b7-240">Затем, чтобы зарегистрировать исправление кода, измените последнюю строку.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-240">Next, change the last line to register a code fix.</span></span> <span data-ttu-id="dc8b7-241">Исправление создаст документ, полученный после добавления модификатора `const` к существующему объявлению:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-241">Your fix will create a new document that results from adding the `const` modifier to an existing declaration:</span></span>  

[!code-csharp[Register the new code fix](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#RegisterCodeFix  "Register the new code fix")]

<span data-ttu-id="dc8b7-242">Под символом `MakeConstAsync` появятся красные волнистые линии.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-242">You'll notice red squiggles in the code you just added on the symbol `MakeConstAsync`.</span></span> <span data-ttu-id="dc8b7-243">Добавьте объявление в `MakeConstAsync`, например как указано ниже:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-243">Add a declaration for `MakeConstAsync` like the following code:</span></span>

```csharp
private async Task<Document> MakeConstAsync(Document document,
   LocalDeclarationStatementSyntax localDeclaration,
   CancellationToken cancellationToken)
{
}
```

<span data-ttu-id="dc8b7-244">Новый метод `MakeConstAsync` преобразует <xref:Microsoft.CodeAnalysis.Document> исходного файла пользователя в новый экземпляр <xref:Microsoft.CodeAnalysis.Document>, содержащий объявление `const`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-244">Your new `MakeConstAsync` method will transform the <xref:Microsoft.CodeAnalysis.Document> representing the user's source file into a new <xref:Microsoft.CodeAnalysis.Document> that now contains a `const` declaration.</span></span>

<span data-ttu-id="dc8b7-245">Создайте новый токен ключевого слова `const` и вставьте его перед оператором объявления.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-245">You create a new `const` keyword token to insert at the front of the declaration statement.</span></span> <span data-ttu-id="dc8b7-246">Не забудьте сначала удалить все элементы trivia из первого оператора объявления и подключить к токену `const`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-246">Be careful to first remove any leading trivia from the first token of the declaration statement and attach it to the `const` token.</span></span> <span data-ttu-id="dc8b7-247">Добавьте следующий код в метод `MakeConstAsync`:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-247">Add the following code to the `MakeConstAsync` method:</span></span>

[!code-csharp[Create a new const keyword token](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#CreateConstToken  "Create the new const keyword token")]

<span data-ttu-id="dc8b7-248">Затем добавьте токен `const` к объявлению с помощью приведенного ниже кода:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-248">Next, add the `const` token to the declaration using the following code:</span></span>

```csharp
// Insert the const token into the modifiers list, creating a new modifiers list.
var newModifiers = trimmedLocal.Modifiers.Insert(0, constToken);
// Produce the new local declaration.
var newLocal = trimmedLocal
    .WithModifiers(newModifiers)
    .WithDeclaration(localDeclaration.Declaration);
```

<span data-ttu-id="dc8b7-249">Отформатируйте новое объявление в соответствии с правилами форматирования C#.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-249">Next, format the new declaration to match C# formatting rules.</span></span> <span data-ttu-id="dc8b7-250">Форматирование изменений в соответствии с существующим кодом упрощает работу.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-250">Formatting your changes to match existing code creates a better experience.</span></span> <span data-ttu-id="dc8b7-251">Добавьте приведенный ниже оператор сразу после существующего кода:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-251">Add the following statement immediately after the existing code:</span></span>

[!code-csharp[Format the new declaration](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#FormatLocal  "Format the new declaration")]

<span data-ttu-id="dc8b7-252">Для выполнения этого кода требуется новое пространство имен.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-252">A new namespace is required for this code.</span></span> <span data-ttu-id="dc8b7-253">Добавьте следующий оператор `using` в начало файла:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-253">Add the following `using` statement to the top of the file:</span></span>

```csharp
using Microsoft.CodeAnalysis.Formatting;
```

<span data-ttu-id="dc8b7-254">В конце нужно внести правки.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-254">The final step is to make your edit.</span></span> <span data-ttu-id="dc8b7-255">Для этого выполните эти три шага:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-255">There are three steps to this process:</span></span>

1. <span data-ttu-id="dc8b7-256">Получите дескриптор существующего документа.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-256">Get a handle to the existing document.</span></span>
1. <span data-ttu-id="dc8b7-257">Создайте документ, заменив существующее объявление на новое.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-257">Create a new document by replacing the existing declaration with the new declaration.</span></span>
1. <span data-ttu-id="dc8b7-258">Верните новый документ.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-258">Return the new document.</span></span>

<span data-ttu-id="dc8b7-259">Добавьте в конце метода `MakeConstAsync` приведенный ниже код:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-259">Add the following code to the end of the `MakeConstAsync` method:</span></span>

[!code-csharp[replace the declaration](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#ReplaceDocument  "Generate a new document by replacing the declaration")]

<span data-ttu-id="dc8b7-260">Ваше исправление кода готово.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-260">Your code fix is ready to try.</span></span>  <span data-ttu-id="dc8b7-261">Нажмите клавишу F5, чтобы запустить проект анализатора во втором экземпляре Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-261">Press F5 to run the analyzer project in a second instance of Visual Studio.</span></span> <span data-ttu-id="dc8b7-262">Во втором экземпляре Visual Studio создайте проект C# консольного приложения и добавьте несколько объявлений локальной переменной, инициализированных со значениями константы в методе main.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-262">In the second Visual Studio instance, create a new C# Console Application project and add a few local variable declarations initialized with constant values to the Main method.</span></span> <span data-ttu-id="dc8b7-263">Они будут отмечены как предупреждения. См. пример ниже.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-263">You'll see that they are reported as warnings as below.</span></span>

![Предупреждение о назначении константы](media/how-to-write-csharp-analyzer-code-fix/make-const-warning.png)

<span data-ttu-id="dc8b7-265">Мы уже много сделали.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-265">You've made a lot of progress.</span></span> <span data-ttu-id="dc8b7-266">Объявления, которые могут быть `const`, подчеркнуты волнистой линией.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-266">There are squiggles under the declarations that can be made `const`.</span></span> <span data-ttu-id="dc8b7-267">Но нам еще нужно с ними поработать.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-267">But there is still work to do.</span></span> <span data-ttu-id="dc8b7-268">Здесь следует добавить `const` в объявления `i`, затем `j` и `k`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-268">This works fine if you add `const` to the declarations starting with `i`, then `j` and finally `k`.</span></span> <span data-ttu-id="dc8b7-269">Но если вы добавите модификатор `const` в обратном порядке, начиная с `k`, анализатор выдаст ошибки: `k` не может быть объявлен как `const`, пока `i` и `j` не являются `const`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-269">But, if you add the `const` modifier i a different order, starting with `k`, your analyzer creates errors: `k` can't be declared `const`, unless `i` and `j` are both already `const`.</span></span> <span data-ttu-id="dc8b7-270">Нужно выполнить дополнительный анализ, чтобы убедиться, что переменные можно объявить и инициализировать различными путями.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-270">You've got to do more analysis to ensure you handle the different ways variables can be declared and initialized.</span></span>

## <a name="build-data-driven-tests"></a><span data-ttu-id="dc8b7-271">Выполнение теста, управляемого данными</span><span class="sxs-lookup"><span data-stu-id="dc8b7-271">Build data driven tests</span></span>

<span data-ttu-id="dc8b7-272">Анализатор и исправление кода работают на простом примере одного объявления, которое может быть константой.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-272">Your analyzer and code fix work on a simple case of a single declaration that can be made const.</span></span> <span data-ttu-id="dc8b7-273">Есть множество возможных операторов объявления, где они выдают ошибки.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-273">There are numerous possible declaration statements where this implementation makes mistakes.</span></span> <span data-ttu-id="dc8b7-274">В этих ситуациях можно обратиться к библиотеке модульных тестов, созданной шаблоном.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-274">You'll address these cases by working with the unit test library written by the template.</span></span> <span data-ttu-id="dc8b7-275">Это гораздо быстрее, чем каждый раз запускать вторую копию Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-275">It's much faster than repeatedly opening a second copy of Visual Studio.</span></span>

<span data-ttu-id="dc8b7-276">Откройте файл **MakeConstUnitTests.cs** в проекте модульного теста.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-276">Open the **MakeConstUnitTests.cs** file in the unit test project.</span></span> <span data-ttu-id="dc8b7-277">В нем созданы два теста по общим шаблонам для анализатора и для модульного теста исправления кода.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-277">The template created two tests that follow the two common patterns for an analyzer and code fix unit test.</span></span> <span data-ttu-id="dc8b7-278">Метод `TestMethod1` гарантирует, что анализатор не выдаст отчет о диагностике, когда это не нужно.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-278">`TestMethod1` shows the pattern for a test that ensures the analyzer doesn't report a diagnostic when it shouldn't.</span></span> <span data-ttu-id="dc8b7-279">Метод `TestMethod2` выдает отчет о диагностике и запускает исправление кода.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-279">`TestMethod2` shows the pattern for reporting a diagnostic and running the code fix.</span></span>

<span data-ttu-id="dc8b7-280">Код почти каждого теста вашего анализатора работает по одному из этих шаблонов.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-280">The code for almost every test for your analyzer follows one of these two patterns.</span></span> <span data-ttu-id="dc8b7-281">Сначала переделайте эти тесты в тесты, управляемые данными.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-281">For the first step, you can rework these tests as data driven tests.</span></span> <span data-ttu-id="dc8b7-282">Так будет проще создавать новые тесты, добавляя новые строковые константы для представления различных входных данных.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-282">Then, it will be easy to create new tests by adding new string constants to represent different test inputs.</span></span>

<span data-ttu-id="dc8b7-283">Для повышения эффективности сначала выполните рефакторинг двух тестов в тесты, управляемые данными.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-283">For efficiency, the first step is to refactor the two tests into data driven tests.</span></span> <span data-ttu-id="dc8b7-284">Затем можно определять несколько строковых констант для каждого нового теста.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-284">Then, you only need to define a couple string constants for each new test.</span></span> <span data-ttu-id="dc8b7-285">Во время рефакторинга переименуйте оба метода.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-285">While your refactoring, rename both methods to better names.</span></span> <span data-ttu-id="dc8b7-286">Замените `TestMethod1` тестом, который гарантирует отсутствие диагностики:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-286">Replace `TestMethod1` with this test that ensures no diagnostic is raised:</span></span>

```csharp
[DataTestMethod]
[DataRow("")]
public void WhenTestCodeIsValidNoDiagnosticIsTriggered(string testCode)
{
    VerifyCSharpDiagnostic(testCode);
}
```

<span data-ttu-id="dc8b7-287">Вы можете создать новую строку данных для теста. Для этого определите фрагмент кода, который не должен вызывать предупреждение диагностики.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-287">You can create a new data row for this test by defining any code fragment that should not cause your diagnostic to trigger a warning.</span></span> <span data-ttu-id="dc8b7-288">Эта перегрузка `VerifyCSharpDiagnostic` передается, если для фрагмента исходного кода диагностика не вызывалась.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-288">This overload of `VerifyCSharpDiagnostic` passes when there are no diagnostics triggered for the source code fragment.</span></span>  

<span data-ttu-id="dc8b7-289">Затем замените `TestMethod2` этим тестом, который гарантирует вызов диагностики, а также то, что исправление применяется к этому фрагменту исходного кода:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-289">Next, replace `TestMethod2` with this test that ensures a diagnostic is raised and a code fix applied for the source code fragment:</span></span>

```csharp
[DataTestMethod]
[DataRow(LocalIntCouldBeConstant, LocalIntCouldBeConstantFixed, 10, 13)]
public void WhenDiagosticIsRaisedFixUpdatesCode(
    string test,
    string fixTest,
    int line,
    int column)
{
    var expected = new DiagnosticResult
    {
        Id = MakeConstAnalyzer.DiagnosticId,
        Message = new LocalizableResourceString(nameof(MakeConst.Resources.AnalyzerMessageFormat), MakeConst.Resources.ResourceManager, typeof(MakeConst.Resources)).ToString(),
        Severity = DiagnosticSeverity.Warning,
        Locations =
            new[] {
                    new DiagnosticResultLocation("Test0.cs", line, column)
                }
    };

    VerifyCSharpDiagnostic(test, expected);

    VerifyCSharpFix(test, fixTest);
}
```

<span data-ttu-id="dc8b7-290">Приведенный выше код также вносит изменения в код, который компилирует ожидаемый результат диагностики.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-290">The preceding code also made a couple changes to the code that builds the expected diagnostic result.</span></span> <span data-ttu-id="dc8b7-291">Для этого используются открытые константы, зарегистрированные в анализаторе `MakeConst`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-291">It uses the public constants registered in the `MakeConst` analyzer.</span></span> <span data-ttu-id="dc8b7-292">Также используются две строковые константы для введенного и исправленного источника.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-292">In addition, it uses two string constants for the input and fixed source.</span></span> <span data-ttu-id="dc8b7-293">Добавьте приведенные ниже строковые константы в класс `UnitTest`:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-293">Add the following string constants to the `UnitTest` class:</span></span>

[!code-csharp[string constants for fix test](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#FirstFixTest "string constants for fix test")]

<span data-ttu-id="dc8b7-294">Чтобы убедиться в том, что они переданы, запустите оба теста.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-294">Run these two tests to make sure they pass.</span></span> <span data-ttu-id="dc8b7-295">Откройте **обозреватель тестов** в Visual Studio, последовательно выбрав **Тест** > **Windows** > **Обозреватель тестов**.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-295">In Visual Studio, open the **Test Explorer** by selecting **Test** > **Windows** > **Test Explorer**.</span></span>  <span data-ttu-id="dc8b7-296">Щелкните ссылку **Выполнить все**.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-296">The press the **Run All** link.</span></span>

## <a name="create-tests-for-valid-declarations"></a><span data-ttu-id="dc8b7-297">Создание тестов для допустимых объявлений</span><span class="sxs-lookup"><span data-stu-id="dc8b7-297">Create tests for valid declarations</span></span>

<span data-ttu-id="dc8b7-298">Как правило, анализаторы должны завершать работу как можно быстрее, выполняя минимум операций.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-298">As a general rule, analyzers should exit as quickly as possible, doing minimal work.</span></span> <span data-ttu-id="dc8b7-299">Когда пользователь правит код, Visual Studio вызывает зарегистрированные анализаторы.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-299">Visual Studio calls registered analyzers as the user edits code.</span></span> <span data-ttu-id="dc8b7-300">Основным требованием здесь является скорость реагирования.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-300">Responsiveness is a key requirement.</span></span> <span data-ttu-id="dc8b7-301">Существует несколько тестовых случаев для кода, который не должен вызывать диагностику.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-301">There are several test cases for code that should not raise your diagnostic.</span></span> <span data-ttu-id="dc8b7-302">Анализатор уже обрабатывает один из них — тот, при котором переменная присваивается после инициализации.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-302">Your analyzer already handles one of those tests, the case where a variable is assigned after being initialized.</span></span> <span data-ttu-id="dc8b7-303">Чтобы воспроизвести этот случай, добавьте приведенную ниже строковую константу в тест:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-303">Add the following string constant to your tests to represent that case:</span></span>

[!code-csharp[variable assigned](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#VariableAssigned "a variable that is assigned after being initialized won't raise the diagnostic")]

<span data-ttu-id="dc8b7-304">Затем добавьте строку данных, как показано во фрагменте ниже:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-304">Then, add a data row for this test as shown in the snippet below:</span></span>

```csharp
[DataTestMethod]
[DataRow(""),
 DataRow(VariableAssigned)]
public void WhenTestCodeIsValidNoDiagnosticIsTriggered(string testCode)
```

<span data-ttu-id="dc8b7-305">Этот тест также проходит.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-305">This test passes as well.</span></span> <span data-ttu-id="dc8b7-306">Далее добавьте константы для условий, которые еще не обработаны:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-306">Next, add constants for conditions you haven't handled yet:</span></span>

* <span data-ttu-id="dc8b7-307">Объявления, которые представляют `const`, так как они уже являются константами:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-307">Declarations that are already `const`, because they are already const:</span></span>

   [!code-csharp[already const declaration](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#AlreadyConst "a declaration that is already const should not raise the diagnostic")]

* <span data-ttu-id="dc8b7-308">Объявления, у которых нет инициализатора, так как нет соответствующего значения:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-308">Declarations that have no initializer, because there is no value to use:</span></span>

   [!code-csharp[declarations that have no initializer](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#NoInitializer "a declaration that has no initializer should not raise the diagnostic")]

* <span data-ttu-id="dc8b7-309">Объявления, у которых инициализатор не является константой, так как они не могут быть константами времени компиляции:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-309">Declarations where the initializer is not a constant, because they can't be compile-time constants:</span></span>

   [!code-csharp[declarations where the initializer isn't const](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#InitializerNotConstant "a declaration where the initializer is not a compile-time constant should not raise the diagnostic")]

<span data-ttu-id="dc8b7-310">Трудность заключается еще в том, что в C# допускается несколько объявлений, работающих как один оператор.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-310">It can be even more complicated because C# allows multiple declarations as one statement.</span></span> <span data-ttu-id="dc8b7-311">Рассмотрите приведенную ниже строковую константу тестового случая:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-311">Consider the following test case string constant:</span></span>

[!code-csharp[multiple initializers](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#MultipleInitializers "A declaration can be made constant only if all variables in that statement can be made constant")]

<span data-ttu-id="dc8b7-312">Переменная `i` может быть константой, а переменная `j` — нет.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-312">The variable `i` can be made constant, but the variable `j` cannot.</span></span> <span data-ttu-id="dc8b7-313">Поэтому этот оператор не может быть объявлением константы.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-313">Therefore, this statement cannot be made a const declaration.</span></span> <span data-ttu-id="dc8b7-314">Добавьте объявления `DataRow` для всех тестов:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-314">Add the `DataRow` declarations for all these tests:</span></span>

```csharp
[DataTestMethod]
[DataRow(""),
    DataRow(VariableAssigned),
    DataRow(AlreadyConst),
    DataRow(NoInitializer),
    DataRow(InitializerNotConstant),
    DataRow(MultipleInitializers)]
public void WhenTestCodeIsValidNoDiagnosticIsTriggered(string testCode)
```

<span data-ttu-id="dc8b7-315">Снова запустите тесты. Произойдет сбой в новых тестовых случаях.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-315">Run your tests again, and you'll see these new test cases fail.</span></span>

## <a name="update-your-analyzer-to-ignore-correct-declarations"></a><span data-ttu-id="dc8b7-316">Обновление анализатора для пропуска верных объявлений</span><span class="sxs-lookup"><span data-stu-id="dc8b7-316">Update your analyzer to ignore correct declarations</span></span>

<span data-ttu-id="dc8b7-317">Чтобы отфильтровать код, который соответствует условиям, необходимо преобразовать метод `AnalyzeNode` анализатора.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-317">You need some enhancements to your analyzer's `AnalyzeNode` method to filter out code that matches these conditions.</span></span> <span data-ttu-id="dc8b7-318">Эти условия связаны между собой, и изменения в одном из них будут применены ко всем.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-318">They are all related conditions, so similar changes will fix all these conditions.</span></span> <span data-ttu-id="dc8b7-319">Внесите следующие изменения в `AnalyzeNode`:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-319">Make the following changes to `AnalyzeNode`:</span></span>

* <span data-ttu-id="dc8b7-320">Для объявления одной переменной был выполнен семантический анализ.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-320">Your semantic analysis examined a single variable declaration.</span></span> <span data-ttu-id="dc8b7-321">Этот код должен находиться в цикле `foreach`, который проверяет все переменные, объявленные в одном и том же операторе.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-321">This code needs to be in a `foreach` loop that examines all the variables declared in the same statement.</span></span>
* <span data-ttu-id="dc8b7-322">Каждая объявленная переменная должна иметь инициализатор.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-322">Each declared variable needs to have an initializer.</span></span>
* <span data-ttu-id="dc8b7-323">Каждый инициализатор переменной должен быть константой времени компиляции.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-323">Each declared variable's initializer must be a compile-time constant.</span></span>

<span data-ttu-id="dc8b7-324">В методе `AnalyzeNode` замените исходный семантический анализ:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-324">In your `AnalyzeNode` method, replace the original semantic analysis:</span></span>

```csharp
// Perform data flow analysis on the local declaration.
var dataFlowAnalysis = context.SemanticModel.AnalyzeDataFlow(localDeclaration);

// Retrieve the local symbol for each variable in the local declaration
// and ensure that it is not written outside of the data flow analysis region.
var variable = localDeclaration.Declaration.Variables.Single();
var variableSymbol = context.SemanticModel.GetDeclaredSymbol(variable);
if (dataFlowAnalysis.WrittenOutside.Contains(variableSymbol))
{
    return;
}
```

<span data-ttu-id="dc8b7-325">приведенным ниже фрагментом кода:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-325">with the following code snippet:</span></span>

```csharp
// Ensure that all variables in the local declaration have initializers that
// are assigned with constant values.
foreach (var variable in localDeclaration.Declaration.Variables)
{
    var initializer = variable.Initializer;
    if (initializer == null)
    {
        return;
    }

    var constantValue = context.SemanticModel.GetConstantValue(initializer.Value);
    if (!constantValue.HasValue)
    {
        return;
    }
}

// Perform data flow analysis on the local declaration.
var dataFlowAnalysis = context.SemanticModel.AnalyzeDataFlow(localDeclaration);

foreach (var variable in localDeclaration.Declaration.Variables)
{
    // Retrieve the local symbol for each variable in the local declaration
    // and ensure that it is not written outside of the data flow analysis region.
    var variableSymbol = context.SemanticModel.GetDeclaredSymbol(variable);
    if (dataFlowAnalysis.WrittenOutside.Contains(variableSymbol))
    {
        return;
    }
}
```

<span data-ttu-id="dc8b7-326">Первый цикл `foreach` проверяет каждое объявление переменной с помощью синтаксического анализа.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-326">The first `foreach` loop examines each variable declaration using syntactic analysis.</span></span> <span data-ttu-id="dc8b7-327">В первой проверке подтверждается наличие инициализатора.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-327">The first check guarantees that the variable has an initializer.</span></span> <span data-ttu-id="dc8b7-328">Во второй — что этот инициализатор является константой.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-328">The second check guarantees that the initializer is a constant.</span></span> <span data-ttu-id="dc8b7-329">Во втором цикле запускается исходный семантический анализ.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-329">The second loop has the original semantic analysis.</span></span> <span data-ttu-id="dc8b7-330">Семантические проверки проходят в отдельных циклах, так как они серьезно влияют на производительность.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-330">The semantic checks are in a separate loop because it has a greater impact on performance.</span></span> <span data-ttu-id="dc8b7-331">Снова запустите тест. Все проверки должны быть пройдены.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-331">Run your tests again, and you should see them all pass.</span></span>

## <a name="add-the-final-polish"></a><span data-ttu-id="dc8b7-332">Финальные штрихи</span><span class="sxs-lookup"><span data-stu-id="dc8b7-332">Add the final polish</span></span>

<span data-ttu-id="dc8b7-333">Вы почти у цели.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-333">You're almost done.</span></span> <span data-ttu-id="dc8b7-334">Анализатор должен обработать еще несколько условий.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-334">There are a few more conditions for your analyzer to handle.</span></span> <span data-ttu-id="dc8b7-335">Пока пользователь пишет код, Visual Studio вызывает анализаторы.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-335">Visual Studio calls analyzers while the user is writing code.</span></span> <span data-ttu-id="dc8b7-336">Часто бывает так, что анализатор вызван для кода, который не компилируется.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-336">It's often the case that your analyzer will be called for code that doesn't compile.</span></span> <span data-ttu-id="dc8b7-337">Метод `AnalyzeNode` диагностического анализатора не проверяет, можно ли преобразовать значение константы в тип переменной.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-337">The diagnostic analyzer's `AnalyzeNode` method does not check to see if the constant value is convertible to the variable type.</span></span> <span data-ttu-id="dc8b7-338">Поэтому в текущей реализации все неверные объявления, такие как int i = "abc", преобразуются в локальные константы.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-338">So, the current implementation will happily convert an incorrect declaration such as int i = "abc"' to a local constant.</span></span> <span data-ttu-id="dc8b7-339">Добавьте исходную строковую константу в это условие:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-339">Add a source string constant for that condition:</span></span>

[!code-csharp[Mismatched types don't raise diagnostics](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#DeclarationIsInvalid "When the variable type and the constant type don't match, there's no diagnostic")]

<span data-ttu-id="dc8b7-340">Кроме того, ссылочные типы обрабатываются неправильно.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-340">In addition, reference types are not handled properly.</span></span> <span data-ttu-id="dc8b7-341">Единственное допустимое значение константы для ссылочного типа — `null`, кроме случаев с <xref:System.String?displayPropert=nameWIthType>, в которых работают строковые литералы.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-341">The only constant value allowed for a reference type is `null`, except in this case of <xref:System.String?displayPropert=nameWIthType>, which allows string literals.</span></span> <span data-ttu-id="dc8b7-342">Другими словами, `const string s = "abc"` является допустимым, а `const object s = "abc"` — нет.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-342">In other words, `const string s = "abc"` is legal, but `const object s = "abc"` is not.</span></span> <span data-ttu-id="dc8b7-343">Этот фрагмент кода проверяет это условие:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-343">This code snippet verifies that condition:</span></span>

[!code-csharp[Reference types don't raise diagnostics](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#DeclarationIsntString "When the variable type is a reference type other than string, there's no diagnostic")]

<span data-ttu-id="dc8b7-344">Чтобы убедиться в том, что можно создать объявление константы для строки, добавьте еще один тест.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-344">To be thorough, you need to add another test to make sure that you can create a constant declaration for a string.</span></span> <span data-ttu-id="dc8b7-345">В приведенным ниже фрагменте кода определен как код, вызывающий диагностику, так и код после исправления:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-345">The following snippet defines both the code that raises the diagnostic, and the code after the fix has been applied:</span></span>

[!code-csharp[string reference types raise diagnostics](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#ConstantIsString "When the variable type is string, it can be constant")]

<span data-ttu-id="dc8b7-346">Если переменная объявлена с ключевым словом `var`, исправление выдает объявление `const var`, которое не поддерживается в C#.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-346">Finally, if a variable is declared with the `var` keyword, the code fix does the wrong thing and generates a `const var` declaration, which is not supported by the C# language.</span></span> <span data-ttu-id="dc8b7-347">Чтобы исправить эту ошибку, исправление должно заменить ключевое слово `var` именем выведенного типа:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-347">To fix this bug, the code fix must replace the `var` keyword with the inferred type's name:</span></span>

[!code-csharp[var references need to use the inferred types](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#VarDeclarations "Declarations made using var must have the type replaced with the inferred type")]

<span data-ttu-id="dc8b7-348">Эти изменения обновят объявления строк данных для обоих тестов.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-348">These changes update the data row declarations for both tests.</span></span> <span data-ttu-id="dc8b7-349">В приведенном ниже коде показаны тесты со всеми атрибутами строк данных:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-349">The following code shows these tests with all data row attributes:</span></span>

[!code-csharp[The finished tests](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst.Test/MakeConstUnitTests.cs#FinishedTests "The finished tests for the make const analyzer")]

<span data-ttu-id="dc8b7-350">К счастью, все вышеперечисленные ошибки можно устранить с помощью методов, которые вы только что изучили.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-350">Fortunately, all of the above bugs can be addressed using the same techniques that you just learned.</span></span>

<span data-ttu-id="dc8b7-351">Чтобы исправить первую ошибку, сначала откройте файл **DiagnosticAnalyzer.cs** и найдите цикл foreach, в котором проверяются инициализаторы локальных объявлений. Им должны быть назначены значения констант.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-351">To fix the first bug, first open **DiagnosticAnalyzer.cs** and locate the foreach loop where each of the local declaration's initializers are checked to ensure that they're assigned with constant values.</span></span> <span data-ttu-id="dc8b7-352">Сразу же, _до_ выполнения цикла foreach, вызовите `context.SemanicModel.GetTypeInfo()`, чтобы извлечь подробные сведения об объявленном типе из локального объявления:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-352">Immediately _before_ the first foreach loop, call `context.SemanicModel.GetTypeInfo()` to retrieve detailed information about the declared type of the local declaration:</span></span>

```csharp
var variableTypeName = localDeclaration.Declaration.Type;
var variableType = context.SemanticModel.GetTypeInfo(variableTypeName).ConvertedType;
```

<span data-ttu-id="dc8b7-353">Затем проверьте каждый инициализатор внутри цикла `foreach`, чтобы убедиться в том, что его можно преобразовать в тип переменной.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-353">Then, inside your `foreach` loop, check each initializer to make sure it's convertible to the variable type.</span></span> <span data-ttu-id="dc8b7-354">Убедившись в том, что инициализатор является константой, добавьте приведенную ниже проверку:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-354">Add the following check after ensuring that the initializer is a constant:</span></span>

```csharp
// Ensure that the initializer value can be converted to the type of the
// local declaration without a user-defined conversion.
var conversion = context.SemanticModel.ClassifyConversion(initializer.Value, variableType);
if (!conversion.Exists || conversion.IsUserDefined)
{
    return;
}
```

<span data-ttu-id="dc8b7-355">Следующее изменение основано на последнем.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-355">The next change builds upon the last one.</span></span> <span data-ttu-id="dc8b7-356">Перед закрывающей фигурной скобкой первого цикла foreach добавьте приведенный ниже код. Он проверит тип локального объявления, когда константа является строкой или имеет значение null.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-356">Before the closing curly brace of the first foreach loop, add the following code to check the type of the local declaration when the constant is a string or null.</span></span>

```csharp
// Special cases:
//  * If the constant value is a string, the type of the local declaration
//    must be System.String.
//  * If the constant value is null, the type of the local declaration must
//    be a reference type.
if (constantValue.Value is string)
{
    if (variableType.SpecialType != SpecialType.System_String)
    {
        return;
    }
}
else if (variableType.IsReferenceType && constantValue.Value != null)
{
    return;
}
```

<span data-ttu-id="dc8b7-357">Необходимо заменить ключевое слово var правильным именем типа в вашем поставщике исправлений кода.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-357">You must write a bit more code in your code fix provider to replace the var' keyword with the correct type name.</span></span> <span data-ttu-id="dc8b7-358">Вернитесь к файлу **CodeFixProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-358">Return to **CodeFixProvider.cs**.</span></span> <span data-ttu-id="dc8b7-359">Код, который вы добавите, выполняет следующие шаги:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-359">The code you'll add does the following steps:</span></span>

* <span data-ttu-id="dc8b7-360">Проверяет, является ли объявление `var`, если да:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-360">Check if the declaration is a `var` declaration, and if it is:</span></span>
* <span data-ttu-id="dc8b7-361">Создает тип для выводимого типа.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-361">Create a new type for the inferred type.</span></span>
* <span data-ttu-id="dc8b7-362">Проверяет, что объявление типа не является псевдонимом.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-362">Make sure the type declaration is not an alias.</span></span> <span data-ttu-id="dc8b7-363">Если это так, можно объявить `const var`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-363">If so, it is legal to declare `const var`.</span></span>
* <span data-ttu-id="dc8b7-364">Проверяет, что `var` не является именем типа в программе</span><span class="sxs-lookup"><span data-stu-id="dc8b7-364">Make sure that `var` isn't a type name in this program.</span></span> <span data-ttu-id="dc8b7-365">(если это так, `const var` является допустимым).</span><span class="sxs-lookup"><span data-stu-id="dc8b7-365">(If so, `const var` is legal).</span></span>
* <span data-ttu-id="dc8b7-366">Упрощает полное имя типа.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-366">Simplify the full type name</span></span>

<span data-ttu-id="dc8b7-367">Кажется, что здесь довольно много кода.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-367">That sounds like a lot of code.</span></span> <span data-ttu-id="dc8b7-368">Но это не так.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-368">It's not.</span></span> <span data-ttu-id="dc8b7-369">Замените строку, которая объявляет и инициализирует `newLocal` приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-369">Replace the line that declares and initializes `newLocal` with the following code.</span></span> <span data-ttu-id="dc8b7-370">Он выполняется сразу после инициализации `newModifiers`:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-370">It goes immediately after the initialization of `newModifiers`:</span></span>

[!code-csharp[Replace Var designations](~/samples/csharp/roslyn-sdk/Tutorials/MakeConst/MakeConst/MakeConstCodeFixProvider.cs#ReplaceVar "Replace a var designation with the explicit type")]

<span data-ttu-id="dc8b7-371">Чтобы использовать тип <xref:Microsoft.CodeAnalysis.Simplification.Simplifier>, потребуется добавить один оператор `using`:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-371">You'll need to add one `using` statement to use the <xref:Microsoft.CodeAnalysis.Simplification.Simplifier> type:</span></span>

```csharp
using Microsoft.CodeAnalysis.Simplification;
```

<span data-ttu-id="dc8b7-372">Запустите тесты. Они все должны быть пройдены.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-372">Run your tests, and they should all pass.</span></span> <span data-ttu-id="dc8b7-373">Можете поздравить себя, запустив готовый анализатор.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-373">Congratulate yourself by running your finished analyzer.</span></span> <span data-ttu-id="dc8b7-374">Чтобы запустить проект анализатора во втором экземпляре Visual Studio с загруженным расширением предварительной версии Roslyn, нажмите клавиши Ctrl+F5.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-374">Press Ctrl+F5 to run the analyzer project in a second instance of Visual Studio with the Roslyn Preview extension loaded.</span></span>

* <span data-ttu-id="dc8b7-375">Во втором экземпляре Visual Studio создайте новый проект C# консольного приложения и добавьте `int x = "abc";` в метод main.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-375">In the second Visual Studio instance, create a new C# Console Application project and add `int x = "abc";` to the Main method.</span></span> <span data-ttu-id="dc8b7-376">Так как первая ошибка была исправлена, для объявления локальной переменной не должно выдаваться предупреждение (хотя есть ошибка компилятора, как и предполагалось).</span><span class="sxs-lookup"><span data-stu-id="dc8b7-376">Thanks to the first bug fix, no warning should be reported for this local variable declaration (though there's a compiler error as expected).</span></span>
* <span data-ttu-id="dc8b7-377">Затем добавьте `object s = "abc";` в метод main.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-377">Next, add `object s = "abc";` to the Main method.</span></span> <span data-ttu-id="dc8b7-378">Так как вторая ошибка была исправлена, не должно быть никаких предупреждений.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-378">Because of the second bug fix, no warning should be reported.</span></span>
* <span data-ttu-id="dc8b7-379">Наконец, добавьте другую локальную переменную, использующую ключевое слово `var`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-379">Finally, add another local variable that uses the `var` keyword.</span></span> <span data-ttu-id="dc8b7-380">Внизу слева появится предупреждение и предложение исправлений.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-380">You'll see that a warning is reported and a suggestion appears beneath to the left.</span></span>
* <span data-ttu-id="dc8b7-381">Наведите курсор редактора на волнистую линию и нажмите клавиши Ctrl+.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-381">Move the editor caret over the squiggly underline and press Ctrl+.</span></span> <span data-ttu-id="dc8b7-382">Отобразятся предложенные исправления.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-382">to display the suggested code fix.</span></span> <span data-ttu-id="dc8b7-383">Обратите внимание, что после выбора исправления ключевое слово var теперь обрабатывается правильно.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-383">Upon selecting your code fix, note that the var' keyword is now handled correctly.</span></span>

<span data-ttu-id="dc8b7-384">В конце добавьте приведенный ниже код:</span><span class="sxs-lookup"><span data-stu-id="dc8b7-384">Finally, add the following code:</span></span>

```csharp
int i = 2;
int j = 32;
int k = i + j;
```

<span data-ttu-id="dc8b7-385">После этого только две переменные будут подчеркнуты красными волнистыми линиями.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-385">After these changes, you get red squiggles only on the first two variables.</span></span> <span data-ttu-id="dc8b7-386">Добавьте `const` в `i` и `j`. Появится предупреждение, указывающее на то, что `k` может быть `const`.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-386">Add `const` to both `i` and `j`, and you get a new warning on `k` because it can now be `const`.</span></span>

<span data-ttu-id="dc8b7-387">Поздравляем!</span><span class="sxs-lookup"><span data-stu-id="dc8b7-387">Congratulations!</span></span> <span data-ttu-id="dc8b7-388">Вы создали свое первое расширение .NET Compiler Platform, которое выполняет анализ кода в режиме реального времени, находит проблемы и быстро их исправляет.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-388">You've created your first .NET Compiler Platform extension that performs on-the-fly code analysis to detect an issue and provides a quick fix to correct it.</span></span> <span data-ttu-id="dc8b7-389">Кроме того, вы изучили множество API, входящих в пакет SDK .NET Compiler Platform (API Roslyn).</span><span class="sxs-lookup"><span data-stu-id="dc8b7-389">Along the way, you've learned many of the code APIs that are part of the .NET Compiler Platform SDK (Roslyn APIs).</span></span> <span data-ttu-id="dc8b7-390">Вы можете сравнить результат своей работы с [готовым примером](https://github.com/dotnet/samples/tree/master/csharp/roslyn-sdk/Tutorials/MakeConst) в нашем репозитории в GitHub.</span><span class="sxs-lookup"><span data-stu-id="dc8b7-390">You can check your work against the [completed sample](https://github.com/dotnet/samples/tree/master/csharp/roslyn-sdk/Tutorials/MakeConst) in our samples GitHub repository.</span></span> <span data-ttu-id="dc8b7-391">Или загрузить [ZIP-файл готового проекта](https://github.com/dotnet/samples/blob/master/csharp/roslyn-sdk/Tutorials/MakeConst.zip).</span><span class="sxs-lookup"><span data-stu-id="dc8b7-391">Or you can download [zip file of the completed project](https://github.com/dotnet/samples/blob/master/csharp/roslyn-sdk/Tutorials/MakeConst.zip)</span></span>

## <a name="other-resources"></a><span data-ttu-id="dc8b7-392">Другие источники</span><span class="sxs-lookup"><span data-stu-id="dc8b7-392">Other resources</span></span>

- [<span data-ttu-id="dc8b7-393">Начало работы с функциями синтаксического анализа</span><span class="sxs-lookup"><span data-stu-id="dc8b7-393">Get started with syntax analysis</span></span>](../get-started/syntax-analysis.md)
- [<span data-ttu-id="dc8b7-394">Начало работы с семантическим анализом</span><span class="sxs-lookup"><span data-stu-id="dc8b7-394">Get started with semantic analysis</span></span>](../get-started/semantic-analysis.md)