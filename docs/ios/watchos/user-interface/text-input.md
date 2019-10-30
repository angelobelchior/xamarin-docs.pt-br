---
title: Trabalhando com entrada de texto watchOS no Xamarin
description: Este documento descreve a entrada de texto watchOS no Xamarin. Ele aborda o método PresentTextInputController, scribbling, texto sem formatação, emojis e ditado.
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 156a31e37d14ce3e3cbe7173ae97b608e9d4c32e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032656"
---
# <a name="working-with-watchos-text-input-in-xamarin"></a>Trabalhando com entrada de texto watchOS no Xamarin

O Apple Watch não fornece um teclado para que os usuários insiram texto, no entanto, ele dá suporte a algumas alternativas amigáveis para observação:

- Selecionando de uma lista predefinida de opções de texto,
- Ditado Siri,
- Escolhendo um Emoji,
- Reconhecimento de manuscrito de letra por letra do Rabisco (introduzido no watchOS 3).

No momento, o simulador não dá suporte a ditado, mas você ainda pode testar as outras opções do controlador de entrada de texto, como rabisco, como mostrado aqui:

![](text-input-images/textinput-sml.png "Testing the scribble option")

Para aceitar entrada de texto em um aplicativo Watch:

1. Crie uma matriz de cadeia de caracteres de opções predefinidas.
2. Chame `PresentTextInputController` com a matriz, se deseja permitir Emoji ou não, e um `Action` que é chamado quando o usuário é concluído.
3. Na ação de conclusão, teste o resultado da entrada e execute a ação apropriada no aplicativo (possivelmente definindo um valor de texto do rótulo).

O trecho de código a seguir apresenta três opções predefinidas para o usuário:

```csharp
var suggest = new string[] {"Get groceries", "Buy gas", "Post letter"};

PresentTextInputController (suggest, WatchKit.WKTextInputMode.AllowEmoji, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
    // this only works if result is a text response (Plain or AllowEmoji)
        enteredText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (enteredText);
        // do something, such as myLabel.SetText(enteredText);
    }
});
```

A enumeração de `WKTextInputMode` tem três valores:

- Queixa
- AllowEmoji
- AllowAnimatedEmoji

## <a name="plain"></a>Queixa

Quando o modo sem formatação é definido, o usuário pode escolher:

- Comandos
- Rabisco ou
- de uma lista predefinida que o aplicativo fornece.

[![](text-input-images/plain-scribble-sml.png "Dictation, Scribble, or from a pre-defined list that the app supplies")](text-input-images/plain-scribble.png#lightbox)

O resultado é sempre retornado como um `NSObject` que pode ser convertido em um `string`.

## <a name="emoji"></a>Emoji

Há dois tipos de Emoji:

- Emoji de Unicode regular
- Imagens animadas

Quando o usuário escolhe um Emoji Unicode, ele é retornado como uma cadeia de caracteres.

Se um Emoji de imagem animada for selecionado, o `result` no manipulador de conclusão conterá um objeto `NSData` que contém o emoji `UIImage`.

## <a name="accepting-dictation-only"></a>Aceitando apenas o ditado

Para levar o usuário diretamente para a tela de ditado sem mostrar nenhuma sugestão (ou a opção rabiscar):

- Passe uma matriz vazia para a lista de sugestões e
- definir `WatchKit.WKTextInputMode.Plain`.

```csharp
PresentTextInputController (new string[0], WatchKit.WKTextInputMode.Plain, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
        dictatedText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (dictatedText);
        // do something, such as myLabel.SetText(dictatedText);
    }
});
```

Quando o usuário está falando, a tela Watch exibe a tela a seguir, que inclui o texto como ele é compreendido (por exemplo, "Este é um teste"):

![](text-input-images/dictation.png "When the user is speaking, the watch screen displays the text as it is understood")

Depois que eles pressionarem o botão **concluído** , o texto será retornado.

## <a name="related-links"></a>Links relacionados

- [Documento de texto e rótulos da Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [Introdução ao watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
