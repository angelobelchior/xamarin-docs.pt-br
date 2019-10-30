---
title: Demonstração do ciclo de vida do aplicativo para Xamarin. iOS
description: Este documento examina vários eventos de ciclo de vida manipulados pelo representante do aplicativo em um aplicativo iOS, demonstrando quando e como esses eventos são tratados.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/17/2018
ms.openlocfilehash: 13f34f6287d68736ee509e6fb43e5fc47321b907
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73011194"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Demonstração do ciclo de vida do aplicativo para Xamarin. iOS

Este artigo e [código de exemplo](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo) demonstra os quatro Estados do aplicativo no Ios, e a função dos métodos de `AppDelegate` notificando a aplicação de quando os Estados são alterados. O aplicativo imprimirá atualizações no console sempre que o aplicativo alterar o estado:

[![](application-lifecycle-demo-images/image3-sml.png "The sample app")](application-lifecycle-demo-images/image3.png#lightbox)

[![](application-lifecycle-demo-images/image4.png "The app will print updates to the console whenever the app changes state")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>Passo a passo

1. Abra o projeto de **ciclo de vida** na solução **LifecycleDemo** .
1. Abra a classe `AppDelegate`. O registro em log foi adicionado aos métodos do ciclo de vida para indicar quando o estado do aplicativo foi alterado:

    ```csharp
    public override void OnActivated(UIApplication application)
    {
        Console.WriteLine("OnActivated called, App is active.");
    }
    public override void WillEnterForeground(UIApplication application)
    {
        Console.WriteLine("App will enter foreground");
    }
    public override void OnResignActivation(UIApplication application)
    {
        Console.WriteLine("OnResignActivation called, App moving to inactive state.");
    }
    public override void DidEnterBackground(UIApplication application)
    {
        Console.WriteLine("App entering background state.");
    }
    // not guaranteed that this will run
    public override void WillTerminate(UIApplication application)
    {
        Console.WriteLine("App is terminating.");
    }
    ```

1. Inicie o aplicativo no simulador ou no dispositivo. `OnActivated` será chamado quando o aplicativo for iniciado. O aplicativo agora está no estado _ativo_ .
1. Pressione o botão Início no simulador ou dispositivo para colocar o aplicativo em segundo plano. `OnResignActivation` e `DidEnterBackground` serão chamados como o aplicativo faz a transição de `Active` para `Inactive` e para o estado `Backgrounded`. Como não há nenhum código de aplicativo definido para ser executado em segundo plano, o aplicativo é considerado _suspenso_ na memória.
1. Navegue de volta para o aplicativo para trazê-lo de volta para o primeiro plano. `WillEnterForeground` e `OnActivated` serão chamados:

    ![](application-lifecycle-demo-images/image4.png "State changes printed to the console")

    A linha de código a seguir no controlador de exibição é executada quando o aplicativo insere o primeiro plano do plano de fundo e altera o texto exibido na tela:

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. Pressione o botão **página inicial** para colocar o aplicativo em segundo plano. Em seguida, toque duas vezes no botão **página inicial** para abrir o alternador de aplicativo. No iPhone X, passe o dedo para cima na parte inferior da tela:

    [![O alternador de aplicativo](application-lifecycle-demo-images/app-switcher-sml.png "O alternador de aplicativo")](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. Localize o aplicativo no alternador de aplicativo e deslize para cima para removê-lo (no iOS 11, com uma prensa longa até que os ícones vermelhos apareçam no canto):

    [![Passe o dedo para cima para remover um aplicativo em execução](application-lifecycle-demo-images/app-switcher-swipe-sml.png "Passe o dedo para cima para remover um aplicativo em execução")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

o iOS encerrará o aplicativo. Observe que `WillTerminate` não é chamado porque o aplicativo já está _suspenso_ em segundo plano.

## <a name="related-links"></a>Links relacionados

- [LifecycleDemo (exemplo)](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)
