---
title: Usar UrhoSharp em xamarin. Forms
description: Este artigo explica como UrhoSharp pode ser usado para adicionar elementos gráficos 3D a um aplicativo xamarin. Forms para visualização avançada.
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/11/2016
ms.openlocfilehash: 60c647a9a09ebffa5a9d50c799c09cf0dbf2e4ac
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75727973"
---
# <a name="using-urhosharp-in-xamarinforms"></a>Usar UrhoSharp em xamarin. Forms

[![Baixar Exemplo](~/media/shared/download.png) Baixar o exemplo](https://github.com/xamarin/urho-samples/tree/master/FormsSample)

## <a name="what-is-urhosharp"></a>O que é UrhoSharp?

[UrhoSharp](~/graphics-games/urhosharp/index.md) é um poderoso mecanismo 3D para desenvolvedores de Xamarin e .NET. O [introduction](~/graphics-games/urhosharp/introduction.md) explica mais sobre a biblioteca de UrhoSharp, e [estas notas](~/graphics-games/urhosharp/using.md) descrevem como programar cenas e ações.

UrhoSharp pode ser usado para renderizar elementos gráficos em aplicativos xamarin. Forms.
Isso [amostra](https://github.com/xamarin/urho-samples/tree/master/FormsSample) demonstra como UrhoSharp poderia ser usada para construir um gráfico 3D interativo:

![](urhosharp-images/ios-animation.gif "UrhoSharp 3D Interactive Chart on iOS")
![](urhosharp-images/android-animation.gif "UrhoSharp 3D Interactive Chart on Android")

## <a name="adding-the-urhosharp-nuget-packages"></a>Adicionando os pacotes NuGet do UrhoSharp

Antes de usar o UrhoSharp, os desenvolvedores precisam adicionar o pacote NuGet do UrhoSharp à sua solução. Este guia pressupõe que um projeto xamarin. Forms com um iOS, Android e o .NET Standard projeto de biblioteca. Todo o código será escrito no projeto de biblioteca de .NET Standard; Mas o NuGet UrhoSharp também deve ser adicionado aos projetos iOS e Android.

O pacote NuGet UrhoSharp. Forms contém todos os objetos necessários para criar objetos UrhoSharp. O pacote NuGet UrhoSharp. Forms inclui a classe `UrhoSurface`, que é usada para hospedar o UrhoSharp no Xamarin. Forms.
Para começar, clique com o botão direito do mouse na pasta **pacotes** no projeto de biblioteca .net Standard e selecione **adicionar pacotes...** . Insira o termo de pesquisa **UrhoSharp. Forms**, selecione **UrhoSharp para Xamarin. Forms**e clique em **Adicionar pacote**.

[![](urhosharp-images/add-package-sml.png "Add Packages Dialog")](urhosharp-images/add-package.png#lightbox "Add Packages Dialog")

O pacote do UrhoSharp.Forms NuGet será adicionado ao projeto:

![](urhosharp-images/packages.png "Packages Folder")

Repita as etapas acima para projetos específicos da plataforma (por exemplo, iOS e Android).

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>Passo a passo: Adicionando UrhoSharp em um aplicativo xamarin. Forms

Estas etapas descrevem o código no exemplo do xamarin. Forms UrhoSharp:

1. [Criar uma página de formulários do Xamarin](#1)
2. [Adicionar o UrhoSurface](#2)
3. [Criar um aplicativo Urho](#3)
4. [Adicione a classe de gráficos para o UrhoSurface](#4)
5. [Interagindo com UrhoSharp](#5)

Observe que o exemplo usa C# recursos de 6 e podem não ser compilados em versões anteriores do Visual Studio.

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1. criar uma página de formulários do Xamarin

O código a seguir mostra uma página do xamarin. Forms `UrhoPage` antes que qualquer código relacionado ao Urho foi adicionado:

```csharp
public class UrhoPage : ContentPage
{
  Slider selectedBarSlider, rotationSlider;

  public UrhoPage()
  {
    // we'll add Urho later

    rotationSlider = new Slider(0, 500, 250);

    selectedBarSlider = new Slider(0, 5, 2.5);

    Title = " UrhoSharp + Xamarin.Forms";
    Content = new StackLayout {
      Padding = new Thickness (12, 12, 12, 40),
      VerticalOptions = LayoutOptions.FillAndExpand,
      Children = {
        rotationSlider,
        new Label { Text = "SELECTED VALUE:" },
        selectedBarSlider,
      }
    };
  }
```

<a name="2"/>

### <a name="2-add-the-urhosurface"></a>2. Adicionar o UrhoSurface

UrhoSharp pode ser hospedado em um `ContentPage` como outros controles do xamarin. Forms.
O trecho de código abaixo mostra um `UrhoSurface` adicionado à página xamarin. Forms:

```csharp
using Urho;
using Urho.Forms;
...
public class UrhoPage : ContentPage
{
  UrhoSurface urhoSurface;

  public UrhoPage()
  {
    urhoSurface = new UrhoSurface();
    urhoSurface.VerticalOptions = LayoutOptions.FillAndExpand;
...
    Content = new StackLayout {
    Padding = new Thickness (12, 12, 12, 40),
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = {
      urhoSurface,  // added
      new Label { Text = "ROTATION:" },
      rotationSlider,
      new Label { Text = "SELECTED VALUE:" },
      selectedBarSlider,
    }
  };
```

<a name="3"/>

### <a name="3-build-a-urho-application"></a>3. criar um aplicativo Urho

Consulte o `Charts` classe para a implementação dos gráficos 3D Urho usados neste exemplo. A estrutura de tópicos de código básico é mostrada abaixo – Observe que a classe implementa `Urho.Application` que é diferente para o `Xamarin.Forms.Application` que é implementado na classe **App.cs**.

```csharp
using Urho;
using Urho.Actions;
using Urho.Gui;
using Urho.Shapes;

namespace FormsSample
{
    public class Charts : Urho.Application
    {
    public Charts (ApplicationOptions options = null) : base(options) { }
    protected override void Start ()
    {
      ...
    }
    protected override void OnUpdate(float timeStep)
    {
      ...
    }
```

O [UrhoSharp documentação](~/graphics-games/urhosharp/index.md) contém mais informações sobre como criar ações e cenas 3D.

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. adicionar a classe Charts ao UrhoSurface

Use o `UrhoSurface.Show<T>` método genérico para adicionar o aplicativo Urho para a página do xamarin. Forms. O trecho de código a seguir mostra o código adicional necessário para criar o `Charts` classe:

```csharp
public class UrhoPage : ContentPage
{
  Charts urhoApp;
  ...
  protected override async void OnAppearing ()
  {
    urhoApp = await urhoSurface.Show<Charts> (new ApplicationOptions(assetsFolder: null)
      { Orientation = ApplicationOptions.OrientationType.Portrait });
  }
```

Observação: os `Show<T>` método é assíncrono e deve ser chamado com o `await` palavra-chave.

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5. interagindo com UrhoSharp

O exemplo permite que as barras do gráfico selecionado e modificados. O `Charts` classe expõe o `Bars` e `SelectedBar` para permitir essa interação.

Cada "bar" tem um manipulador de eventos de seleção adicionado após o `Charts` tenha sido processado classe iterando sobre a exposta `Bars` coleção:

```csharp
protected override async void OnAppearing ()
{
  urhoApp = await urhoSurface.Show<Charts>(new ApplicationOptions(assetsFolder: null) { Orientation = ApplicationOptions.OrientationType.Portrait });
  foreach (var bar in urhoApp.Bars)
  {
    bar.Selected += OnBarSelection;
  }
}
```

O manipulador de eventos usa o valor do xamarin. Forms `Slider` controle para ajustar a altura da barra de ferramentas fornecida:

```csharp
private void OnBarSelection(Bar bar)
{
  //reset value
  selectedBarSlider.ValueChanged -= OnValuesSliderValueChanged;
  selectedBarSlider.Value = bar.Value;
  selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
}

void OnValuesSliderValueChanged(object sender, ValueChangedEventArgs e)
{  // C# 6
  if (urhoApp?.SelectedBar != null)
  {
    urhoApp.SelectedBar.Value = (float)e.NewValue;
  }
}
```

Por fim, conectar os dois `Slider` controles de modo que quando seu valor é alterado, a tela de UrhoSharp é afetada. O controle deslizante primeiro gira a imagem do gráfico 3D e o segundo controle deslizante ajusta a altura da barra de ferramentas selecionada.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

As animações na [superior da página](#what-is-urhosharp) mostram o exemplo em execução.

## <a name="summary"></a>Resumo

Esta página mostra como UrhoSharp pode ser usado para adicionar a visualização de dados 3D ao xamarin. Forms. Leia as [UrhoSharp documentação](~/graphics-games/urhosharp/index.md) para obter mais informações sobre como construir cenas Urho que podem ser incluídas em aplicativos xamarin. Forms usando o método mostrado acima.

## <a name="related-links"></a>Links Relacionados

- [Gráficos de exemplo (C# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
