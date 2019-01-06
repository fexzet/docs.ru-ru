---
title: Включение автодополнения клавишей TAB
description: В этой статье объясняется, как включить автодополнение клавишей TAB для .NET Core CLI в средах PowerShell, Bash и zsh.
author: thraka
ms.author: adegeo
ms.date: 12/17/2018
ms.openlocfilehash: 10b2e13aad9821295efc5c36d1cad04f1a95477c
ms.sourcegitcommit: 0888d7b24f475c346a3f444de8d83ec1ca7cd234
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2018
ms.locfileid: "53784400"
---
# <a name="how-to-enable-tab-completion-for-the-net-core-cli"></a>Включение автодополнения клавишей TAB для .NET Core CLI

Начиная с пакета SDK для .NET Core 2.0 среда .NET Core CLI поддерживает автодополнение по нажатию клавиши TAB. В этой статье описывается, как включить автодополнение нажатием клавиши TAB для трех оболочек: PowerShell, Bash и zsh. Другие оболочки могут иметь поддержку автодополнения. Чтобы узнать, как настроить автодополнение, ознакомьтесь с документацией к этим оболочкам; использование автодополнения будет аналогичным, описанному в этой статье.

[!INCLUDE [topic-appliesto-net-core-2plus](~/includes/topic-appliesto-net-core-2plus.md)]

После установки, автодополнение клавишей TAB для .NET Core CLI можно активировать, введя в командной строке `dotnet ` и нажав клавишу TAB. Текущая командная строка будет передана команде `dotnet complete`, а оболочка обработает результаты. Вы можете проверить результаты без активации автодополнения клавишей TAB, передав что-либо непосредственно команде `dotnet complete`. Например:

```
> dotnet complete "dotnet a"
add
clean
--diagnostics
migrate
pack
```

Если команда не сработала, убедитесь, что установлен пакет SDK для .NET Core 2.0 или более поздней версии. Если команда не сработала, хотя этот пакет установлен, убедитесь, что команда `dotnet` содержит путь к .NET Core 2.0 или более поздней версии. Воспользуйтесь командой `dotnet --version`, чтобы узнать, к которой версии `dotnet` указывает текущий путь. Дополнительные сведения см. в статье [Выбор версии .NET Core для использования](../versions/selection.md).

### <a name="examples"></a>Примеры

Ниже приведено несколько примеров возможностей, которые предоставляет автодополнение клавишей TAB:

Входные данные                                | becomes                                                                     | because
:------------------------------------|:----------------------------------------------------------------------------|:--------------------------------
`dotnet a⇥`                          | `dotnet add`                                                                 | `add` является первой подкомандой в алфавитном порядке.
`dotnet add p⇥`                      | `dotnet add --help`                                                          | При автодополнении клавишей TAB подстроки сопоставляются и первой по алфавиту является `--help`.
`dotnet add p⇥⇥`                    | `dotnet add package`                                                          | После второго нажатия клавиши TAB подставляется следующее предложение.      
`dotnet add package Microsoft⇥`      | `dotnet add package Microsoft.ApplicationInsights.Web`                      | Результаты возвращаются в алфавитном порядке.
`dotnet remove reference ⇥`          | `dotnet remove reference ..\..\src\OmniSharp.DotNet\OmniSharp.DotNet.csproj` | Автодополнение клавишей TAB зависит от файла проекта.

## <a name="powershell"></a>PowerShell

Чтобы добавить автодополнение клавишей TAB для .NET Core CLI в **PowerShell**, создайте или измените профиль, хранящийся в переменной `$PROFILE`. Дополнительные сведения см. в разделах [How to create your profile](/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-6#how-to-create-a-profile) (Как создать свой профиль) и [Profiles and execution policy](/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-6#profiles-and-execution-policy) (Профили и политика выполнения). 

Добавьте в свой профиль представленный ниже код:

```powershell
# PowerShell parameter completion shim for the dotnet CLI 
Register-ArgumentCompleter -Native -CommandName dotnet -ScriptBlock {
     param($commandName, $wordToComplete, $cursorPosition)
         dotnet complete --position $cursorPosition "$wordToComplete" | ForEach-Object {
            [System.Management.Automation.CompletionResult]::new($_, $_, 'ParameterValue', $_)
         }
 }
```

## <a name="bash"></a>Bash

Чтобы добавить автодополнение клавишей TAB для .NET Core CLI в **bash**, добавьте в свой файл `.bashrc` представленный ниже код:

```bash
# bash parameter completion for the dotnet CLI

_dotnet_bash_complete()
{
  local word=${COMP_WORDS[COMP_CWORD]}

  local completions
  completions="$(dotnet complete --position "${COMP_POINT}" "${COMP_LINE}")"

  COMPREPLY=( $(compgen -W "$completions" -- "$word") )
}

complete -f -F _dotnet_bash_complete dotnet
```

## <a name="zsh"></a>Zsh

Чтобы добавить автодополнение клавишей TAB для .NET Core CLI в **zsh**, добавьте в свой файл `.zshrc` представленный ниже код:

```zsh
# zsh parameter completion for the dotnet CLI

_dotnet_zsh_complete() 
{
  local completions=("$(dotnet complete "$words")")

  reply=( "${(ps:\n:)completions}" )
}

compctl -K _dotnet_zsh_complete dotnet
```