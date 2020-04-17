---
ms.openlocfilehash: 2b524c657a95035773d18fb17bb86308a78ce37c
ms.sourcegitcommit: 89b3e383a37db5b940f0c63bbfe9cb806dc7d5d1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81389910"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. Em **MainPage.xaml**, modifique a declaração [`Label`](xref:Xamarin.Forms.Label) para alterar sua aparência visual:

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    Esse código define propriedades que alteram a aparência visual do [`Label`](xref:Xamarin.Forms.Label). A propriedade [`TextColor`](xref:Xamarin.Forms.Label.TextColor) define a cor do texto `Label`. A propriedade [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) define a fonte do rótulo como itálico e a propriedade [`FontSize`](xref:Xamarin.Forms.Label.FontSize) define o tamanho da fonte. Além disso, uma decoração de texto sublinhado é aplicada ao `Label` definindo sua propriedade [`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations) e ele é centralizado horizontalmente definindo a propriedade [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) como [`Center`](xref:Xamarin.Forms.LayoutOptions.Center).

1. Na barra de ferramentas do Visual Studio, pressione o botão **Iniciar** (o botão triangular que se parece com o botão Reproduzir) para iniciar o aplicativo dentro de seu simulador remoto de iOS ou do Android Emulator escolhido. Observe que a aparência de [`Label`](xref:Xamarin.Forms.Label) mudou:

    [![Captura de tela de um rótulo com uma aparência visual alterada, no iOS e no Android](../images/change-label-appearance.png "Rótulo com aparência alterada")](../images/change-label-appearance-large.png#lightbox "Rótulo com aparência alterada")

    Para obter mais informações sobre como definir a aparência de [`Label`](xref:Xamarin.Forms.Label), confira o guia [Rótulo do Xamarin.Forms](~/xamarin-forms/user-interface/text/label.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio para Mac](#tab/vsmac)

1. Em **MainPage.xaml**, modifique a declaração [`Label`](xref:Xamarin.Forms.Label) para alterar sua aparência visual:

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    Esse código define propriedades que alteram a aparência visual do [`Label`](xref:Xamarin.Forms.Label). A propriedade [`TextColor`](xref:Xamarin.Forms.Label.TextColor) define a cor do texto `Label`. A propriedade [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) define a fonte do rótulo como itálico e a propriedade [`FontSize`](xref:Xamarin.Forms.Label.FontSize) define o tamanho da fonte. Além disso, uma decoração de texto sublinhado é aplicada ao `Label` definindo sua propriedade [`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations) e ele é centralizado horizontalmente definindo a propriedade [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) como [`Center`](xref:Xamarin.Forms.LayoutOptions.Center).

1. Na barra de ferramentas do Visual Studio para Mac, pressione o botão **Iniciar** (o botão triangular que se parece com um botão Reproduzir) para iniciar o aplicativo dentro do simulador de iOS ou do Android Emulator escolhido. Observe que a aparência de [`Label`](xref:Xamarin.Forms.Label) mudou:

    [![Captura de tela de um rótulo com uma aparência visual alterada, no iOS e no Android](../images/change-label-appearance.png "Rótulo com aparência alterada")](../images/change-label-appearance-large.png#lightbox "Rótulo com aparência alterada")

    Para obter mais informações sobre como definir a aparência de [`Label`](xref:Xamarin.Forms.Label), confira o guia [Rótulo do Xamarin.Forms](~/xamarin-forms/user-interface/text/label.md).
