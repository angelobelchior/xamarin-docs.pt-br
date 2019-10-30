---
title: Introdução ao tvOS 12
description: Este documento fornece um alto nível de visão geral dos recursos novos e atualizados no tvOS 12 para os quais a versão de visualização do C# Xamarin atualmente fornece associações.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: 94fea1786497d04602ea6cf06d875206cf69eb3e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030551"
---
# <a name="introduction-to-tvos-12"></a>Introdução ao tvOS 12

Este documento fornece uma visão geral de alto nível do tvOS 12 novo e atualizado.

Para começar a criar aplicativos tvOS 12 com o Xamarin, dê uma olhada no [Guia de introdução](~/ios/platform/introduction-to-ios12/get-started.md).

## <a name="tvuikit"></a>TVUIKit

o tvOS 12 inclui o TVUIKit, um conjunto de APIs que possibilita aos desenvolvedores de tvOS usar controles de tvOS comuns, como exibições de pôster, botões de legenda, exibições de cartão e exibições de Monogram. o tvOS 12 também introduz uma propriedade que permite que os rótulos rolem o texto que é muito longo para ficar totalmente visível.

## <a name="password-autofill"></a>Preenchimento automático de senha

Com o tvOS 12, os usuários podem usar seus dispositivos iOS para entrar em um aplicativo tvOS com um único toque. Isso é habilitado por meio de uma combinação de uso de `UITextContentType` para especificar campos de nome de usuário e senha, domínios associados para estabelecer uma relação entre um aplicativo iOS e um aplicativo tvOS e ambientes de foco preferenciais para selecionar um item para receber foco depois de um usuário fornece um nome de usuário e senha.

## <a name="focus-engine-enhancements"></a>Aprimoramentos do mecanismo de foco

o tvOS 12 permite todos os aplicativos, independentemente de como eles são renderizados, para interagir com o mecanismo de foco. Através de interações de um usuário com o Siri remoto, o mecanismo de foco pode ser usado com qualquer aplicativo para selecionar um item, uma dica em possíveis alterações de foco e o foco de atualização naturalmente. Isso é habilitado em aplicativos personalizados por meio da interface `IUIFocusItemContainer` do UIKit, da classe `UIFocusMovementHint`, da interface do `IUIFocusItemScrollableContainer` e de outras classes e métodos relacionados.

## <a name="vision-framework"></a>Estrutura de visão

A estrutura de visão inclui um detector facial aprimorado que pode detectar rostos em várias orientações. Além disso, as revisões de solicitação agora podem ser usadas para selecionar uma revisão de algoritmo do Vision Framework específica.

## <a name="natural-language-framework"></a>Estrutura de linguagem natural

A estrutura de linguagem natural permite que os aplicativos executem vários tipos de análise de linguagem. Por exemplo, ele pode ser usado para identificar partes de fala e determinar o idioma representado por um bloco de texto.

## <a name="deprecations"></a>Substituições

Com o tvOS 12, a Apple preteriu OpenGL ES, [incentivando os desenvolvedores](https://developer.apple.com/tvos/whats-new/) a adotar o metal.

## <a name="related-links"></a>Links relacionados

- [Amostras do tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS – desenvolvedor da Apple (Apple)](https://developer.apple.com/tvos/)
- [O que há de novo no tvOS 12 (Apple) (vídeo)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
