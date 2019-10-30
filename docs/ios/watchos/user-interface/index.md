---
title: Controles de interface do usuário watchOS no Xamarin
description: Este documento descreve os vários controles que estão disponíveis para uso em interfaces de usuário do watchOS. Ele fornece uma descrição de rótulos, botões, comutadores, controles deslizantes, imagens, separadores, mapas e muito mais.
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/19/2016
ms.openlocfilehash: 8836eafbbce30586116fd7a7b125da55fe6edf8e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032715"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>Controles de interface do usuário watchOS no Xamarin

O exemplo [**WatchKitCatalog**](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) demonstra vários controles watchOS. O storyboard do aplicativo é mostrado aqui (clique para aplicar zoom):

[![](images/storyboard-sml.png "Sample watchOS layout")](images/storyboard.png#lightbox)

Os nomes programáticos de todos os controles são prefixados com `WKInterface` (por exemplo, `WKInterfaceLabel`, `WKInterfaceButton`).

|Controle|Descrição|Captura|
|---|---|---|
|Rotular|Use `SetText` e outras propriedades para controlar a aparência do texto em um controle rótulo. também há suporte para `NSAttributedString`.<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Botão|Crie e defina as propriedades no storyboard. Ctrl + arrastar para adicionar um `Action` para implementar um manipulador para quando ele é clicado.<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Alternar|Use `SetOn` para controlar o estado do comutador.<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Controle deslizante|Muitos estilos diferentes são possíveis.<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Image|Use `myImage.SetImage("MyWatchImage")` para carregar imagens na inspeção ou `WKInterfaceDevice.CurrentDevice.AddCachedImage` para armazená-las em cache para uso repetido na inspeção.<br />[Documentação do controle de imagem](~/ios/watchos/user-interface/image.md)<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Separador|Use separadores para ajudar a criar UIs de inspeção atraentes.<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|Mapa|A imagem do mapa é exibida estaticamente na inspeção, mas você pode controlar vários aspectos de sua aparência, incluindo a adição de Pins.<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Filme & InlineMove|Os filmes podem ser abertos por conta própria ou em linha<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Grupo|Use grupos para ajudar a criar UIs de inspeção atraentes.<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|Tabela|Uma versão simplificada de tabelas no iOS. Implemente `DidSelectRow` para responder à seleção do usuário (ou use um transição).<br />[Documentação de controle de tabela](~/ios/watchos/user-interface/table.md)<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Dispositivo|`WKInterfaceDevice.CurrentDevice` inclui propriedades como `ScreenBounds`, `ScreenScale`e `PreferredContentSizeCategory`.<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menu](~/ios/watchos/user-interface/menu.md)|Defina o menu de forçar e pressione no storyboard e implemente as ações para cada botão no código.<br />[Documentação de controle de menu (Force Touch)](~/ios/watchos/user-interface/menu.md)<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Entrada de Texto|Use `PresentTextInputController` e a enumeração `WKTextInputMode`.<br />[Documentação de entrada de texto](~/ios/watchos/user-interface/text-input.md)<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Digital Crown|O Digital Crown pode ser usado para orientar um seletor ou a rotação pode ser controlada no código.<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Gestos|Há quatro tipos de reconhecimento de gesto que podem ser adicionados a uma cena: toque, passe o dedo, panorâmica e LongPress.<br />[Código do catálogo](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|

## <a name="related-links"></a>Links relacionados

- [WatchKitCatalog (exemplo)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Referência da API do kit de inspeção](xref:WatchKit)
