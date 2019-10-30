---
title: Exemplo do mundo real usando CocoaPods
description: Este documento demonstra como usar a nitidez objetiva para gerar automaticamente as C# definições de associação de um CocoaPod.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: davidortinau
ms.author: daortin
ms.date: 03/28/2018
ms.openlocfilehash: cf117880eb46b028d709a44aa453e111b007b441
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016271"
---
# <a name="real-world-example-using-cocoapods"></a>Exemplo do mundo real usando CocoaPods

> [!NOTE]
> Este exemplo usa [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).

Novidade na versão 3,0, o Objective Sharp dá suporte à vinculação de CocoaPods e até mesmo inclui um comando (`sharpie pod`) para tornar o download, a configuração e a criação de CocoaPods muito fáceis. Você deve se [familiarizar com o CocoaPods](https://cocoapods.org) em geral antes de usar esse recurso.

## <a name="creating-a-binding-for-a-cocoapod"></a>Criando uma associação para um CocoaPod

O comando `sharpie pod` tem uma opção global e dois subcomandos:

```bash
$ sharpie pod -help
usage: sharpie pod [OPTIONS] COMMAND [COMMAND_OPTIONS]

Pod Options:
  -d, -dir DIR     Use DIR as the CocoaPods binding directory,
                   defaulting to the current directory

Available Commands:
  init         Initialize a new Xamarin C# CocoaPods binding project
  bind         Bind an existing Xamarin C# CocoaPods project
```

O subcomando `init` também tem uma ajuda útil:

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

Vários nomes de CocoaPod e de subspecs podem ser fornecidos para `init`.

```bash
$ sharpie pod init ios AFNetworking
** Setting up CocoaPods master repo ...
   (this may take a while the first time)
** Searching for requested CocoaPods ...
** Working directory:
**   - Writing Podfile ...
**   - Installing CocoaPods ...
**     (running `pod install --no-integrate --no-repo-update`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
** 🍻 Success! You can now use other `sharpie podn`  commands.
```

Depois que o CocoaPod tiver sido configurado, agora você poderá criar a associação:

```bash
$ sharpie pod bind
```

Isso fará com que o projeto CocoaPod Xcode seja compilado e então avaliado e analisado por uma nitidez objetiva. Muitas saídas de console serão geradas, mas devem resultar na definição de associação no final:

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Próximas etapas

Depois de gerar os arquivos **ApiDefinitions.cs** e **StructsAndEnums.cs** , dê uma olhada na seguinte documentação para gerar um assembly a ser usado em seus aplicativos:

- [Visão geral do objetivo de associação-C](~/cross-platform/macios/binding/overview.md)
- [Associando bibliotecas de Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Walkthrough: associando uma biblioteca de Objective do iOS-C](~/ios/platform/binding-objective-c/walkthrough.md)
