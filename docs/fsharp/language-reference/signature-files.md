---
title: 'Файлы сигнатур (F #)'
description: 'Узнайте, как использовать файлы сигнатур F # для хранения сведений об открытых сигнатурах набора F # элементы программы, например модули, типы и пространства имен.'
ms.date: 06/15/2018
ms.openlocfilehash: 21e1b1d1cb67ea64206070a947d667fd52441dd8
ms.sourcegitcommit: 6bc4efca63e526ce6f2d257fa870f01f8c459ae4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/19/2018
ms.locfileid: "36208674"
---
# <a name="signatures"></a><span data-ttu-id="ad086-103">Сигнатуры</span><span class="sxs-lookup"><span data-stu-id="ad086-103">Signatures</span></span>

<span data-ttu-id="ad086-104">В файле сигнатур содержатся сведения об открытых сигнатурах набора элементов программы на языке F#, таких как типы, пространства имен и модули.</span><span class="sxs-lookup"><span data-stu-id="ad086-104">A signature file contains information about the public signatures of a set of F# program elements, such as types, namespaces, and modules.</span></span> <span data-ttu-id="ad086-105">Файл сигнатур можно использовать для указания доступности этих элементов программы.</span><span class="sxs-lookup"><span data-stu-id="ad086-105">It can be used to specify the accessibility of these program elements.</span></span>


## <a name="remarks"></a><span data-ttu-id="ad086-106">Примечания</span><span class="sxs-lookup"><span data-stu-id="ad086-106">Remarks</span></span>
<span data-ttu-id="ad086-107">Для каждого файла кода F# может иметься *файл сигнатур*— файл с тем же именем, что и файл кода, но с расширением FSI вместо FS.</span><span class="sxs-lookup"><span data-stu-id="ad086-107">For each F# code file, you can have a *signature file*, which is a file that has the same name as the code file but with the extension .fsi instead of .fs.</span></span> <span data-ttu-id="ad086-108">Файлы сигнатур также можно добавлять в командную строку компиляции, если командная строка используется напрямую.</span><span class="sxs-lookup"><span data-stu-id="ad086-108">Signature files can also be added to the compilation command-line if you are using the command line directly.</span></span> <span data-ttu-id="ad086-109">Для различения файлов кода и файлов сигнатур файлы кода иногда называют *файлами реализации*.</span><span class="sxs-lookup"><span data-stu-id="ad086-109">To distinguish between code files and signature files, code files are sometimes referred to as *implementation files*.</span></span> <span data-ttu-id="ad086-110">В проекте файл сигнатур должен предшествовать связанному с ним файлу кода.</span><span class="sxs-lookup"><span data-stu-id="ad086-110">In a project, the signature file should precede the associated code file.</span></span>

<span data-ttu-id="ad086-111">Файл сигнатур описывает пространства имен, модули, типы и члены в соответствующем файле реализации.</span><span class="sxs-lookup"><span data-stu-id="ad086-111">A signature file describes the namespaces, modules, types, and members in the corresponding implementation file.</span></span> <span data-ttu-id="ad086-112">Информацию в файле сигнатур можно использовать для указания того, к каким частям кода в соответствующем файле реализации возможен доступ из кода, находящегося за пределами файла реализации, и какие части являются внутренними по отношению к файлу реализации.</span><span class="sxs-lookup"><span data-stu-id="ad086-112">You use the information in a signature file to specify what parts of the code in the corresponding implementation file can be accessed from code outside the implementation file, and what parts are internal to the implementation file.</span></span> <span data-ttu-id="ad086-113">Пространства имен, модули и типы, включаемые в файл сигнатур, должны представлять собой подмножество имен пространств, модулей и типов, включаемых в файл реализации.</span><span class="sxs-lookup"><span data-stu-id="ad086-113">The namespaces, modules, and types that are included in the signature file must be a subset of the namespaces, modules, and types that are included in the implementation file.</span></span> <span data-ttu-id="ad086-114">За некоторыми исключениями, рассмотренными ниже в этом разделе, языковые элементы, отсутствующие в файле сигнатур, считаются закрытыми по отношению к файлу реализации.</span><span class="sxs-lookup"><span data-stu-id="ad086-114">With some exceptions noted later in this topic, those language elements that are not listed in the signature file are considered private to the implementation file.</span></span> <span data-ttu-id="ad086-115">Если в проекте или в командной строке не найден файл сигнатур, используется доступность по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ad086-115">If no signature file is found in the project or command line, the default accessibility is used.</span></span>

<span data-ttu-id="ad086-116">Дополнительные сведения о доступности по умолчанию см. в разделе [управления доступом](access-control.md).</span><span class="sxs-lookup"><span data-stu-id="ad086-116">For more information about the default accessibility, see [Access Control](access-control.md).</span></span>

<span data-ttu-id="ad086-117">В файле сигнатур не нужно повторять определение типов и реализаций для каждого метода или функции.</span><span class="sxs-lookup"><span data-stu-id="ad086-117">In a signature file, you do not repeat the definition of the types and the implementations of each method or function.</span></span> <span data-ttu-id="ad086-118">Вместо этого для каждого метода и функции используется сигнатура, выступающая в качестве полной спецификации функциональности, реализуемой фрагментом модуля или пространства имен.</span><span class="sxs-lookup"><span data-stu-id="ad086-118">Instead, you use the signature for each method and function, which acts as a complete specification of the functionality that is implemented by a module or namespace fragment.</span></span> <span data-ttu-id="ad086-119">Синтаксис сигнатуры типа идентичен используемому в объявлениях абстрактных методов в интерфейсах и абстрактных классах; он также отображается IntelliSense и интерпретатором языка F# (fsi.exe) при выводе правильно скомпилированных входных данных.</span><span class="sxs-lookup"><span data-stu-id="ad086-119">The syntax for a type signature is the same as that used in abstract method declarations in interfaces and abstract classes, and is also shown by IntelliSense and by the F# interpreter fsi.exe when it displays correctly compiled input.</span></span>

<span data-ttu-id="ad086-120">Если в сигнатуре типа недостаточно информации для определения того, является ли тип запечатанным или типом интерфейса, нужно добавить атрибут, по которому компилятор сможет определить природу типа.</span><span class="sxs-lookup"><span data-stu-id="ad086-120">If there is not enough information in the type signature to indicate whether a type is sealed, or whether it is an interface type, you must add an attribute that indicates the nature of the type to the compiler.</span></span> <span data-ttu-id="ad086-121">Используемые с этой целью атрибуты приведены в таблице ниже.</span><span class="sxs-lookup"><span data-stu-id="ad086-121">Attributes that you use for this purpose are described in the following table.</span></span>



|<span data-ttu-id="ad086-122">Атрибут</span><span class="sxs-lookup"><span data-stu-id="ad086-122">Attribute</span></span>|<span data-ttu-id="ad086-123">Описание:</span><span class="sxs-lookup"><span data-stu-id="ad086-123">Description</span></span>|
|---------|-----------|
|`[<Sealed>]`|<span data-ttu-id="ad086-124">Для типа, который не имеет абстрактных членов или не должен расширяться.</span><span class="sxs-lookup"><span data-stu-id="ad086-124">For a type that has no abstract members, or that should not be extended.</span></span>|
|`[<Interface>]`|<span data-ttu-id="ad086-125">Для типа, являющегося интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="ad086-125">For a type that is an interface.</span></span>|
<span data-ttu-id="ad086-126">Компилятор выдает ошибку, если атрибуты в сигнатуре и в объявлении в файле реализации не соответствуют друг другу.</span><span class="sxs-lookup"><span data-stu-id="ad086-126">The compiler produces an error if the attributes are not consistent between the signature and the declaration in the implementation file.</span></span>

<span data-ttu-id="ad086-127">Для создания сигнатуры для значения или значения функции используется ключевое слово `val` .</span><span class="sxs-lookup"><span data-stu-id="ad086-127">Use the keyword `val` to create a signature for a value or function value.</span></span> <span data-ttu-id="ad086-128">Сигнатура типа предваряется ключевым словом `type` .</span><span class="sxs-lookup"><span data-stu-id="ad086-128">The keyword `type` introduces a type signature.</span></span>

<span data-ttu-id="ad086-129">Сформировать файл сигнатур можно с помощью параметра `--sig` компилятора.</span><span class="sxs-lookup"><span data-stu-id="ad086-129">You can generate a signature file by using the `--sig` compiler option.</span></span> <span data-ttu-id="ad086-130">Как правило, вручную FSI-файлы не пишутся.</span><span class="sxs-lookup"><span data-stu-id="ad086-130">Generally, you do not write .fsi files manually.</span></span> <span data-ttu-id="ad086-131">Вместо этого вы создаете FSI-файлы с помощью компилятора, добавляете их в проект (при работе с проектом) и редактируете их, удаляя методы и функции, которые не должны быть доступными.</span><span class="sxs-lookup"><span data-stu-id="ad086-131">Instead, you generate .fsi files by using the compiler, add them to your project, if you have one, and edit them by removing methods and functions that you do not want to be accessible.</span></span>

<span data-ttu-id="ad086-132">В отношении сигнатур типов действует ряд правил.</span><span class="sxs-lookup"><span data-stu-id="ad086-132">There are several rules for type signatures:</span></span>


- <span data-ttu-id="ad086-133">Аббревиатуры типов в файле реализации не должны совпадать с полными названиями типов в файле сигнатур.</span><span class="sxs-lookup"><span data-stu-id="ad086-133">Type abbreviations in an implementation file must not match a type without an abbreviation in a signature file.</span></span>


- <span data-ttu-id="ad086-134">Записи и размеченные объединения должны предоставлять либо все свои поля и конструкторы, либо никакие из них, и порядок в сигнатуре должен совпадать с порядком в файле реализации.</span><span class="sxs-lookup"><span data-stu-id="ad086-134">Records and discriminated unions must expose either all or none of their fields and constructors, and the order in the signature must match the order in the implementation file.</span></span> <span data-ttu-id="ad086-135">Классы могут открывать некоторые, все или никакие из своих полей и методов в сигнатуре.</span><span class="sxs-lookup"><span data-stu-id="ad086-135">Classes can reveal some, all, or none of their fields and methods in the signature.</span></span>


- <span data-ttu-id="ad086-136">Классы и структуры, имеющие конструкторы, должны предоставлять объявления своих базовых классов (объявление `inherits` ).</span><span class="sxs-lookup"><span data-stu-id="ad086-136">Classes and structures that have constructors must expose the declarations of their base classes (the `inherits` declaration).</span></span> <span data-ttu-id="ad086-137">Кроме того, классы и структуры, имеющие конструкторы, должны предоставлять все свои абстрактные методы и объявления интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="ad086-137">Also, classes and structures that have constructors must expose all of their abstract methods and interface declarations.</span></span>


- <span data-ttu-id="ad086-138">Типы интерфейсов должны открывать все свои методы и интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="ad086-138">Interface types must reveal all their methods and interfaces.</span></span>


<span data-ttu-id="ad086-139">К сигнатурам значений применяются приведенные ниже правила.</span><span class="sxs-lookup"><span data-stu-id="ad086-139">The rules for value signatures are as follows:</span></span>


- <span data-ttu-id="ad086-140">Модификаторы доступности (`public`, `internal`и т. д.) и модификаторы `inline` и `mutable` в сигнатуре должны совпадать с модификаторами в реализации.</span><span class="sxs-lookup"><span data-stu-id="ad086-140">Modifiers for accessibility (`public`, `internal`, and so on) and the `inline` and `mutable` modifiers in the signature must match those in the implementation.</span></span>


- <span data-ttu-id="ad086-141">Количество параметров универсального типа (неявно выведенных или явно объявленных) должно совпадать. Также должны совпадать типы и ограничения типов в параметрах универсального типа.</span><span class="sxs-lookup"><span data-stu-id="ad086-141">The number of generic type parameters (either implicitly inferred or explicitly declared) must match, and the types and type constraints in generic type parameters must match.</span></span>


- <span data-ttu-id="ad086-142">Если используется атрибут `Literal` , он должен присутствовать и в сигнатуре, и в реализации, и в обоих случая должно использоваться одно и то же литеральное значение.</span><span class="sxs-lookup"><span data-stu-id="ad086-142">If the `Literal` attribute is used, it must appear in both the signature and the implementation, and the same literal value must be used for both.</span></span>


- <span data-ttu-id="ad086-143">Шаблон параметров (также называемый *арностью*) сигнатур должен соответствовать шаблону параметров реализаций.</span><span class="sxs-lookup"><span data-stu-id="ad086-143">The pattern of parameters (also known as the *arity*) of signatures and implementations must be consistent.</span></span>


- <span data-ttu-id="ad086-144">Если имена параметров в файле сигнатур отличаются от соответствующий файл реализации, имя в файле сигнатур будет использоваться вместо, что может вызвать проблемы при отладке или профилирования.</span><span class="sxs-lookup"><span data-stu-id="ad086-144">If parameter names in a signature file differ from the corresponding implementation file, the name in the signature file will be used instead, which may cause issues when debugging or profiling.</span></span> <span data-ttu-id="ad086-145">Если вы хотите получать уведомления о таких несоответствий включить предупреждение 3218 в файле проекта или при вызове компилятора (в разделе `--warnon` под [параметры компилятора](compiler-options.md)).</span><span class="sxs-lookup"><span data-stu-id="ad086-145">If you wish to be notified of such mismatches, enable warning 3218 in your project file or when invoking the compiler (see `--warnon` under [Compiler Options](compiler-options.md)).</span></span>


<span data-ttu-id="ad086-146">В приведенном ниже примере показан файл сигнатур, содержащий сигнатуры пространства имен, модуля, значения функции и типов вместе с соответствующими атрибутами.</span><span class="sxs-lookup"><span data-stu-id="ad086-146">The following code example shows an example of a signature file that has namespace, module, function value, and type signatures together with the appropriate attributes.</span></span> <span data-ttu-id="ad086-147">Показан также соответствующий файл реализации.</span><span class="sxs-lookup"><span data-stu-id="ad086-147">It also shows the corresponding implementation file.</span></span>

[!code-fsharp[Main](../../../samples/snippets/fsharp/fssignatures/snippet9002.fs)]

<span data-ttu-id="ad086-148">В приведенном ниже коде показан файл реализации.</span><span class="sxs-lookup"><span data-stu-id="ad086-148">The following code shows the implementation file.</span></span>

[!code-fsharp[Main](../../../samples/snippets/fsharp/fssignatures/snippet9001.fs)]
    
## <a name="see-also"></a><span data-ttu-id="ad086-149">См. также</span><span class="sxs-lookup"><span data-stu-id="ad086-149">See Also</span></span>
[<span data-ttu-id="ad086-150">Справочник по языку F#</span><span class="sxs-lookup"><span data-stu-id="ad086-150">F# Language Reference</span></span>](index.md)

[<span data-ttu-id="ad086-151">Управление доступом</span><span class="sxs-lookup"><span data-stu-id="ad086-151">Access Control</span></span>](access-control.md)

[<span data-ttu-id="ad086-152">Параметры компилятора</span><span class="sxs-lookup"><span data-stu-id="ad086-152">Compiler Options</span></span>](compiler-options.md)