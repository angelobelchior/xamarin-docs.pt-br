---
title: Xamarin.Mac – documentação relacionada
description: 'Este documento fornece links para a documentação relevante para desenvolvedores Xamarin.Mac: documentação do Xamarin.iOS, Mac Dev Center da Apple e vários guias que descrevem como criar interfaces do usuário com Xamarin.Mac.'
ms.prod: xamarin
ms.assetid: 0a282c58-1c37-4f73-8440-85de2daf454a
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
ms.openlocfilehash: 10450bbb87ed974475001102920ec6fb90cf7976
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029985"
---
# <a name="xamarinmac-related-documentation"></a>Xamarin.Mac – documentação relacionada

Além da seção do Mac no [Microsoft Docs](~/mac/get-started/index.md), existem três excelentes fontes de documentação que podem ajudar em caso de dúvidas sobre o Xamarin.Mac:

- [**Documentação do Xamarin.IOS**](~/ios/get-started/index.md) – Para muitas APIs (principalmente fora do AppKit/UIKit), há apenas pequenas diferenças entre as versões de iOS e macOS. Em alguns casos em que determinada API de iOS tem um nome de `UIFoo`, uma API semelhante chamada `NSFoo` pode ser encontrada no macOS. Esses exemplos geralmente estarão em C#.

- **Centro de desenvolvimento de Mac [da Apple](https://developer.apple.com/devcenter/mac/)** – Muitas vezes um exemplo de quais APIs chamar em Objective-C pode ser convertido em C# de uma maneira direta. Consulte [Noções básicas de APIs do Mac](~/mac/app-fundamentals/mac-apis.md) para obter detalhes de como fazer isso.

- [**Estouro de pilha**](https://stackoverflow.com/) – Um excelente recurso para perguntas simples como ["Como expandir automaticamente todos os nós em uma NSOutlineView"](https://stackoverflow.com/questions/519751/nsoutlineview-auto-expand-all-nodes). Esses exemplos geralmente estarão em Objective-C e precisam ser convertidos em C#, mas há um subconjunto de respostas em C#.

## <a name="user-interface"></a>Interface do Usuário

Ao trabalhar com C# e .NET em um aplicativo Xamarin.Mac, o desenvolvedor tem acesso aos mesmos controles de interface do usuário que um desenvolvedor que trabalha em *Objective-C* e *Xcode*. Como o Xamarin.Mac se integra diretamente com Xcode, o desenvolvedor pode usar o _Interface Builder_ do Xcode para criar e manter interfaces de usuário de um aplicativo (ou, opcionalmente, criá-los diretamente em código C#).

Os guias listados abaixo oferecem informações detalhadas sobre como trabalhar com elementos do macOS em um aplicativo Xamarin.Mac:

- [Windows](~/mac/user-interface/window.md)
- [Caixas de diálogo](~/mac/user-interface/dialog.md)
- [Alertas](~/mac/user-interface/alert.md)
- [Menus](~/mac/user-interface/menu.md)
- [Barras de ferramentas](~/mac/user-interface/toolbar.md)
- [Modos de exibição de tabela](~/mac/user-interface/table-view.md)
- [Modos de exibição de estrutura de tópicos](~/mac/user-interface/outline-view.md)
- [Listas de origem](~/mac/user-interface/source-list.md)
