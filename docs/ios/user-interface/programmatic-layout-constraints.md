---
title: Restrições de layout programáticos no Xamarin. iOS
description: Este guia apresenta o trabalho com restrições de layout automático do iOS no código C# em vez de criá-las no designer do iOS.
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: d8dcf64f9a905557bea69402bcaf7fcfb51fb10b
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928569"
---
# <a name="programmatic-layout-constraints-in-xamarinios"></a>Restrições de layout programáticos no Xamarin. iOS

_Este guia apresenta o trabalho com restrições de layout automático do iOS no código C# em vez de criá-las no designer do iOS._

O layout automático (também chamado de "layout adaptável") é uma abordagem de design responsiva. Ao contrário do sistema de layout de transição, onde o local de cada elemento é embutido em código para um ponto na tela, o layout automático é sobre *relações* – as posições de elementos em relação a outros elementos na superfície de design. No coração do layout automático está a ideia de restrições ou regras que definem o posicionamento de um elemento ou conjunto de elementos no contexto de outros elementos na tela. Como os elementos não estão vinculados a uma posição específica na tela, as restrições ajudam a criar um layout adaptável que parece bom em diferentes tamanhos de tela e orientações de dispositivo.

Normalmente, ao trabalhar com o layout automático no iOS, você usará o designer do iOS para posicionar graficamente as restrições de layout nos itens da interface do usuário. No entanto, pode haver ocasiões em que você precisa criar e aplicar restrições no código C#. Por exemplo, ao usar elementos de interface do usuário criados dinamicamente adicionados a um `UIView` .

Este guia mostrará como criar e trabalhar com restrições usando código C# em vez de criá-las graficamente no designer do iOS.

<a name="Creating-Constraints-Programmatically"></a>

## <a name="creating-constraints-programmatically"></a>Criando restrições programaticamente

Como mencionado acima, normalmente você trabalhará com restrições de layout automático no designer do iOS. Para os horários em que você precisa criar suas restrições programaticamente, você tem três opções para escolher:

- [Âncoras de layout](#Layout-Anchors) -essa API fornece acesso às propriedades de âncora (como `TopAnchor` `BottomAnchor` ou `HeightAnchor` ) dos itens da interface do usuário que estão sendo restringidos.
- [Restrições de layout](#Layout-Constraints) -você pode criar restrições diretamente usando a `NSLayoutConstraint` classe.
- [Linguagem de formatação visual](#Visual-Format-Language) – fornece uma arte ASCII como método para definir suas restrições.

As seções a seguir abordarão cada opção em detalhes.

<a name="Layout-Anchors"></a>

### <a name="layout-anchors"></a>Âncoras de layout

Usando a `NSLayoutAnchor` classe, você tem uma interface fluente para criar restrições com base nas propriedades de âncora dos itens da interface do usuário que estão sendo restringidos. Por exemplo, os guias de layout superior e inferior de um controlador de exibição expõe as `TopAnchor` `BottomAnchor` Propriedades de âncora e, `HeightAnchor` enquanto uma exibição expõe as propriedades de borda, centro, tamanho e linha de base.

> [!IMPORTANT]
> Além do conjunto padrão de propriedades de âncora, as exibições do iOS também incluem as `LayoutMarginsGuides` `ReadableContentGuide` Propriedades e. Essas propriedades expõem `UILayoutGuide` objetos para trabalhar com as margens da exibição e os guias de conteúdo legível, respectivamente.

As âncoras de layout fornecem vários métodos para criar restrições em um formato de compactação fácil de ler:

- **ConstraintEqualTo** -define uma relação em que `first attribute = second attribute + [constant]` com um valor de deslocamento fornecido opcionalmente `constant` .
- **ConstraintGreaterThanOrEqualTo** -define uma relação em que `first attribute >= second attribute + [constant]` com um valor de deslocamento fornecido opcionalmente `constant` .
- **ConstraintLessThanOrEqualTo** -define uma relação em que `first attribute <= second attribute + [constant]` com um valor de deslocamento fornecido opcionalmente `constant` .

Por exemplo:

```csharp
// Get the parent view's layout
var margins = View.LayoutMarginsGuide;

// Pin the leading edge of the view to the margin
OrangeView.LeadingAnchor.ConstraintEqualTo (margins.LeadingAnchor).Active = true;

// Pin the trailing edge of the view to the margin
OrangeView.TrailingAnchor.ConstraintEqualTo (margins.TrailingAnchor).Active = true;

// Give the view a 1:2 aspect ratio
OrangeView.HeightAnchor.ConstraintEqualTo (OrangeView.WidthAnchor, 2.0f);
```

Uma restrição de layout típica pode ser expressa simplesmente como uma expressão linear. Veja o exemplo seguinte:

[![Uma restrição de layout expressa como uma expressão linear](programmatic-layout-constraints-images/graph01.png)](programmatic-layout-constraints-images/graph01.png#lightbox)

Que seria convertido na seguinte linha de código C# usando âncoras de layout:

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

Onde as partes do código C# correspondem às partes fornecidas da equação da seguinte maneira:

|Subscrito|Código|
|---|---|
|Item 1|PurpleView|
|Atributo 1|LeadingAnchor|
|Relação|ConstraintEqualTo|
|Multiplicador|O padrão é 1,0, portanto não especificado|
|Item 2|OrangeView|
|Atributo 2|TrailingAnchor|
|Constante|10.0|

Além de fornecer apenas os parâmetros necessários para resolver uma determinada equação de restrição de layout, cada um dos métodos de âncora de layout impõe a segurança de tipo dos parâmetros passados a eles. Portanto, as âncoras de restrição horizontal como `LeadingAnchor` ou `TrailingAnchor` só podem ser usadas com outros tipos de âncora horizontais e multiplicadores são fornecidos apenas para restrições de tamanho.

<a name="Layout-Constraints"></a>

### <a name="layout-constraints"></a>Restrições de layout

Você pode adicionar restrições de layout automático manualmente, construindo diretamente um `NSLayoutConstraint` no código C#. Ao contrário do uso de âncoras de layout, você deve especificar um valor para cada parâmetro, mesmo que ele não tenha nenhum efeito na restrição que está sendo definida. Como resultado, você acabará produzindo uma quantidade considerável de difícil de ler, código clichê. Por exemplo:

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

Em que `NSLayoutAttribute` enum define o valor das margens da exibição e corresponde às `LayoutMarginsGuide` Propriedades como `Left` , e `Right` `Top` `Bottom` e a `NSLayoutRelation` enum define a relação que será criada entre os atributos especificados como `Equal` , `LessThanOrEqual` ou `GreaterThanOrEqual` .

Ao contrário da API de âncora de layout, os `NSLayoutConstraint` métodos de criação não realçam os aspectos importantes de uma determinada restrição e não há nenhuma verificação de tempo de compilação executada na restrição. Como resultado, é fácil construir uma restrição inválida que gerará uma exceção em tempo de execução.

<a name="Visual-Format-Language"></a>

### <a name="visual-format-language"></a>Linguagem de formato Visual

A linguagem de formato visual permite que você defina restrições usando arte ASCII, como cadeias de caracteres que fornecem uma representação visual da restrição que está sendo criada. Isso tem as seguintes vantagens e desvantagens:

- A linguagem de formato Visual impõe a criação apenas de restrições válidas.
- O layout automático gera restrições para o console usando a linguagem de formato visual para que as mensagens de depuração sejam parecidas com o código usado para criar a restrição.
- A linguagem de formato visual permite que você crie várias restrições ao mesmo tempo com uma expressão muito compacta.
- Como não há nenhuma validação no lado da compilação das cadeias de caracteres de linguagem de formato visual, os problemas só podem ser descobertos no tempo de execução.
- Como a linguagem de formato Visual enfatiza a visualização sobre a integridade, alguns tipos de restrição não podem ser criados com ele (como as proporções).

Você segue as seguintes etapas ao usar a linguagem de formato visual para criar uma restrição:

1. Crie um `NSDictionary` que contenha os objetos View e guias de layout e uma chave de cadeia de caracteres que será usada ao definir os formatos.
2. Opcionalmente, crie um `NSDictionary` que defina um conjunto de chaves e valores ( `NSNumber` ) usados como o valor constante para a restrição.
3. Crie a cadeia de caracteres de formato para layout de uma única coluna ou linha de itens.
4. Chame o `FromVisualFormat` método da `NSLayoutConstraint` classe para gerar as restrições.
5. Chame o `ActivateConstraints` método da `NSLayoutConstraint` classe para ativar e aplicar as restrições.

Por exemplo, para criar uma restrição à esquerda e à direita na linguagem de formato visual, você pode usar o seguinte:

```csharp
// Get views being constrained
var views = new NSMutableDictionary (); 
views.Add (new NSString ("orangeView"), OrangeView);

// Define format and assemble constraints
var format = "|-[orangeView]-|";
var constraints = NSLayoutConstraint.FromVisualFormat (format, NSLayoutFormatOptions.AlignAllTop, null, views);

// Apply constraints
NSLayoutConstraint.ActivateConstraints (constraints);
```

Como a linguagem de formato Visual sempre cria restrições de ponto zero anexadas às margens da exibição pai ao usar o espaçamento padrão, esse código produz resultados idênticos aos exemplos apresentados acima.

Para designs de interface de usuário mais complexos, como várias exibições filho em uma única linha, a linguagem de formato Visual especifica o espaçamento horizontal e o alinhamento vertical. Como no exemplo acima, onde ele especifica o `AlignAllTop` `NSLayoutFormatOptions` alinha todas as exibições em uma linha ou coluna ao seu tops.

Consulte o [Apêndice de linguagem de formato Visual](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1) da Apple para ver alguns exemplos de como especificar restrições comuns e a gramática de cadeia de caracteres de formato visual.

<a name="Summary"></a>

## <a name="summary"></a>Resumo

Este guia apresentou a criação e o trabalho com restrições de layout automático em C# em vez de criá-las graficamente no designer do iOS. Primeiro, ele examinou o uso de âncoras de layout ( `NSLayoutAnchor` ) para lidar com o layout automático. Em seguida, ele mostrou como trabalhar com restrições de layout ( `NSLayoutConstraint` ). Por fim, ele apresentou o uso da linguagem de formato visual para layout automático.

## <a name="related-links"></a>Links Relacionados

- [Introdução ao Storyboards](~/ios/user-interface/storyboards/index.md)
- [Instruções de controles designáveis do iOS](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Layout automático com o Xamarin Designer para iOS](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple – criando restrições programaticamente](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple – apêndice de linguagem de formato Visual](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
