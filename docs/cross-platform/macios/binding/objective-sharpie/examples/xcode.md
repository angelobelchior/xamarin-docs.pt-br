---
title: Exemplo do mundo real usando um projeto do Xcode
description: Este documento descreve como usar um projeto do Xcode como uma entrada direta para a nitidez objetiva, simplificando o processo de criação C# de associações para o código Objective-C.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: davidortinau
ms.author: daortin
ms.date: 01/15/2016
ms.openlocfilehash: 16191b7ee5bae353a438340d6099d39a296bffd0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016223"
---
# <a name="real-world-example-using-an-xcode-project"></a>Exemplo do mundo real usando um projeto do Xcode

**Este exemplo usa a [biblioteca pop do Facebook](https://github.com/facebook/pop).**

Novidade na versão 3,0, a nitidez do objetivo dá suporte a projetos do Xcode como entrada. Esses projetos especificam os arquivos de cabeçalho corretos e os sinalizadores de compilador necessários para compilar a biblioteca nativa e, portanto, são necessários também para associá-la. A nitidez do objetivo selecionará o primeiro _destino_ e sua configuração padrão de um projeto, caso não seja instruído a fazer o contrário.

Antes de o objetivo tentar analisar os arquivos de projeto e de cabeçalho, ele deve compilá-lo. Os projetos geralmente têm fases de compilação que estruturarão corretamente os arquivos de cabeçalho para o consumo e a integração externos, portanto, é melhor compilar sempre o projeto completo antes de tentar associá-lo.

```
$ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   (more git clone output)

$ cd pop
$ sharpie bind pop.xcodeproj -sdk iphoneos9.0
```
