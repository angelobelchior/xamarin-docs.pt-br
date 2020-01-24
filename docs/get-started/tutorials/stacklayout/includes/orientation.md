---
ms.openlocfilehash: 5a464196c08220158432219d96bf82578789d8e5
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/23/2020
ms.locfileid: "67559976"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Em **MainPage.xaml**, modifique a declaração [`StackLayout`](xref:Xamarin.Forms.StackLayout) para que seus elementos filho sejam alinhados horizontalmente, e não verticalmente:

    ```xaml
    <StackLayout Margin="20,35,20,25"
                 Orientation="Horizontal">
        <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
        <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
        <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
    </StackLayout>
    ```

    Este código define a propriedade [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) como [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal).

1. Na barra de ferramentas do Visual Studio, pressione o botão **Iniciar** (o botão triangular que se parece com um botão Reproduzir) para iniciar o aplicativo dentro do simulador remoto de iOS ou do Android Emulator escolhido:

    [![Captura de tela das exibições filho orientadas horizontalmente em um StackLayout, no iOS e no Android](../images/orientation.png "StackLayout que contém instâncias de rótulo orientadas horizontalmente")](../images/orientation-large.png#lightbox "StackLayout que contém instâncias de rótulo orientadas horizontalmente")

    Observe que as instâncias de [`Label`](xref:Xamarin.Forms.Label) dentro do [`StackLayout`](xref:Xamarin.Forms.StackLayout) agora estão alinhadas horizontalmente, e não verticalmente.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio para Mac](#tab/vsmac)

1. Em **MainPage.xaml**, modifique a declaração [`StackLayout`](xref:Xamarin.Forms.StackLayout) para que seus elementos filho sejam alinhados horizontalmente, e não verticalmente:

    ```xaml
    <StackLayout Margin="20,35,20,25"
                 Orientation="Horizontal">
        <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
        <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
        <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
    </StackLayout>
    ```

    Este código define a propriedade [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) como [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal).

1. Na barra de ferramentas do Visual Studio para Mac, pressione o botão **Iniciar** (o botão triangular que se parece com o botão Reproduzir) para iniciar o aplicativo dentro do simulador de iOS ou do Android Emulator escolhido:

    [![Captura de tela das exibições filho orientadas horizontalmente em um StackLayout, no iOS e no Android](../images/orientation.png "StackLayout que contém instâncias de rótulo orientadas horizontalmente")](../images/orientation-large.png#lightbox "StackLayout que contém instâncias de rótulo orientadas horizontalmente")

    Observe que as instâncias de [`Label`](xref:Xamarin.Forms.Label) dentro do [`StackLayout`](xref:Xamarin.Forms.StackLayout) agora estão alinhadas horizontalmente, e não verticalmente.

-----

> [!div class="nextstepaction"]
> [Encontrei um problema](https://github.com/MicrosoftDocs/xamarin-docs/issues/new?title=StackLayout+Tutorial+Step+2+Feedback&template=tutorial_template.md)
