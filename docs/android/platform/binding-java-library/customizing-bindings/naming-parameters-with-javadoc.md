---
title: Nomeando parâmetros com Javadoc
description: Este artigo explica como recuperar nomes de parâmetro em um projeto de associação Java usando o Javadoc gerado a partir do projeto Java.
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/20/2017
ms.openlocfilehash: 060c4759d39bc3b8c424ce46dc615644540fe9c2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027675"
---
# <a name="naming-parameters-with-javadoc"></a>Nomeando parâmetros com Javadoc

_Este artigo explica como recuperar nomes de parâmetro em um projeto de associação Java usando o Javadoc gerado a partir do projeto Java._

## <a name="overview"></a>Visão Geral

Ao associar uma biblioteca Java existente, alguns metadados sobre a API associada são perdidos. Em particular, os nomes dos parâmetros para os métodos. Os nomes de parâmetro serão exibidos como `p0`, `p1`, etc. Isso ocorre porque os arquivos Java `.class` não preservam os nomes de parâmetro que foram usados no código-fonte Java. 

Um projeto de associação Java do Xamarin. Android pode fornecer os nomes de parâmetro se ele tiver acesso ao HTML do Javadoc a partir da biblioteca original. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Integrando o Javadoc HTML em um projeto de associação Java

A integração do HTML do Javadoc a um projeto de associação Java é um processo manual que consiste nas seguintes etapas: 

1. Baixar o Javadoc para a biblioteca
2. Edite o arquivo de `.csproj` e adicione uma propriedade `<JavaDocPaths>`:
3. Limpar e recompilar o projeto

Depois que isso for feito, os nomes de parâmetro Java originais deverão estar presentes nas APIs associadas por um projeto de associação Java. 

> [!NOTE]
> Há uma grande quantidade de variação na saída do JavaDoc. Dos. A associação de JAR ferramentas não dá suporte a todas as permutações possíveis e, consequentemente, alguns parâmetros podem não ser nomeados corretamente.

## <a name="summary"></a>Resumo

Este artigo abordou como usar o Javadoc em um projeto de associação Java para fornecer nomes de parâmetro para APIs associadas. 
