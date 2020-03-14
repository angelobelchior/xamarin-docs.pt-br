---
title: Submenu Shell do Xamarin.Forms
description: O submenu é o menu raiz de um aplicativo Shell e é acessível por meio de um ícone ou passando o dedo na lateral da tela. O submenu consiste em um cabeçalho opcional, itens de submenu e itens de menu opcionais.
ms.prod: xamarin
ms.assetid: FEDE51EB-577E-4B3E-9890-B7C1A5E52516
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/05/2019
ms.openlocfilehash: 4049b3bdfdd6077dcfa151df9553722e63def0ba
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305167"
---
# <a name="xamarinforms-shell-flyout"></a>Submenu Shell do Xamarin.Forms

[![Baixar Exemplo](~/media/shared/download.png) Baixar o exemplo](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

O submenu é o menu raiz de um aplicativo Shell e é acessível por meio de um ícone ou passando o dedo na lateral da tela. O submenu consiste em um cabeçalho opcional, itens de submenu e itens de menu opcionais:

![Captura de tela de um submenu anotado do Shell](flyout-images/flyout-annotated.png "Submenu anotado")

Se necessário, a cor da tela de fundo do submenu pode ser definida como uma [`Color`](xref:Xamarin.Forms.Color) por meio da propriedade associável `Shell.FlyoutBackgroundColor`. Essa propriedade também pode ser definida em uma folha de estilos em cascata (CSS). Saiba mais em [propriedades específicas do Shell do Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="flyout-icon"></a>Ícone de submenu

Por padrão, os aplicativos Shell têm um ícone de hambúrguer que, quando pressionado, abre o submenu suspenso. Esse ícone pode ser alterado definindo a propriedade associável `Shell.FlyoutIcon`, do tipo [`ImageSource`](xref:Xamarin.Forms.ImageSource), como um ícone apropriado:

```xaml
<Shell ...
       FlyoutIcon="flyouticon.png">
    ...       
</Shell>
```

## <a name="flyout-behavior"></a>Comportamento do submenu

O submenu por ser acessado pelo ícone de hambúrguer ou passando o dedo na lateral da tela. No entanto, esse comportamento pode ser alterado definindo a propriedade anexada `Shell.FlyoutBehavior` como um dos membros de enumeração `FlyoutBehavior`:

- `Disabled` – indica que o submenu não pode ser aberto pelo usuário.
- `Flyout` – indica que o submenu pode ser aberto e fechado pelo usuário. Este é o valor padrão da propriedade `FlyoutBehavior`.
- `Locked` – indica que o submenu não pode ser fechado pelo usuário, e o submenu não se sobrepõe ao conteúdo.

O exemplo a seguir mostra como desabilitar o submenu:

```xaml
<Shell ...
       FlyoutBehavior="Disabled">
    ...
</Shell>
```

> [!NOTE]
> A propriedade anexada `FlyoutBehavior` pode ser definida em `Shell`, `FlyoutItem`, `ShellContent` e em objetos da página para substituir o comportamento padrão do submenu.

Além disso, o submenu pode ser aberto de forma programática e fechado com a definição da propriedade associável `Shell.FlyoutIsPresented` como um valor `boolean` que indica se o submenu está visível ou não:

```csharp
Shell.Current.FlyoutIsPresented = false;
```

## <a name="flyout-header"></a>Cabeçalho do submenu

O cabeçalho do submenu é o conteúdo exibido opcionalmente na parte superior do submenu, e sua aparência é definida por um `object` que pode ser configurado por meio do valor da propriedade `Shell.FlyoutHeader`:

```xaml
<Shell.FlyoutHeader>
    <controls:FlyoutHeader />
</Shell.FlyoutHeader>
```

O tipo `FlyoutHeader` é mostrado no exemplo a seguir:

```xaml
<ContentView xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Xaminals.Controls.FlyoutHeader"
             HeightRequest="200">
    <Grid BackgroundColor="Black">
        <Image Aspect="AspectFill"
               Source="xamarinstore.jpg"
               Opacity="0.6" />
        <Label Text="Animals"
               TextColor="White"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentView>
```

Como resultado, teremos o cabeçalho do submenu a seguir:

![Captura de tela do cabeçalho do submenu](flyout-images/flyout-header.png "Cabeçalho do submenu")

Como alternativa, a aparência do cabeçalho do submenu pode ser definida com a configuração da propriedade `Shell.FlyoutHeaderTemplate` como um [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell.FlyoutHeaderTemplate>
    <DataTemplate>
        <Grid BackgroundColor="Black"
              HeightRequest="200">
            <Image Aspect="AspectFill"
                   Source="xamarinstore.jpg"
                   Opacity="0.6" />
            <Label Text="Animals"
                   TextColor="White"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center" />
        </Grid>            
    </DataTemplate>
</Shell.FlyoutHeaderTemplate>
```

Por padrão, o cabeçalho do submenu ficará fixo no submenu enquanto o conteúdo abaixo poderá ser rolado se houver itens suficientes. No entanto, esse comportamento pode ser alterado definindo-se a propriedade associável `Shell.FlyoutHeaderBehavior` como um dos membros da enumeração `FlyoutHeaderBehavior`:

- `Default` – indica que o comportamento padrão da plataforma será usado. Este é o valor padrão da propriedade `FlyoutHeaderBehavior`.
- `Fixed` – indica que o cabeçalho do submenu permanece sempre inalterado e visível.
- `Scroll` – indica que o cabeçalho do submenu rola para fora da exibição conforme o usuário rola os itens.
- `CollapseOnScroll` – indica que o cabeçalho do submenu é recolhido, tornando-se apenas um título à medida que o usuário rola os itens.

O exemplo a seguir mostra como recolher o cabeçalho do submenu à medida que o usuário rola a tela:

```xaml
<Shell ...
       FlyoutHeaderBehavior="CollapseOnScroll">
    ...
</Shell>
```

## <a name="flyout-background-image"></a>Imagem da tela de fundo do submenu

O submenu pode ter uma imagem de tela de fundo opcional, que aparece sob o cabeçalho do submenu e atrás de quaisquer itens do submenu e do menu. A imagem da tela de fundo pode ser especificada definindo a propriedade associável `FlyoutBackgroundImage`, do tipo [`ImageSource`](xref:Xamarin.Forms.ImageSource), para um arquivo, recurso inserido, URI ou fluxo.

A taxa de proporção da imagem da tela de fundo pode ser configurada definindo a propriedade associável `FlyoutBackgroundImageAspect`, do tipo [`Aspect`](xref:Xamarin.Forms.Aspect) para um dos membros da enumeração `Aspect`:

- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) – Recorta a imagem para que preencha a área de exibição e preserve a taxa de proporção.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) – Coloca a imagem em um formato letterbox, se necessário, para que ela caiba na área de exibição, e adiciona um espaço em branco à parte superior/inferior ou aos lados, de acordo com a largura ou a altura da imagem.
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) – Alonga a imagem para preencher a área de exibição de modo completo e exato. Isso pode resultar em distorção de imagem.

A propriedade de `FlyoutBackgroundImageAspect` é definida por padrão como `AspectFit`.

O exemplo a seguir mostra a definição dessas propriedades:

```xaml
<Shell ...
       FlyoutBackgroundImage="photo.jpg"
       FlyoutBackgroundImageAspect="AspectFill">
    ...
</Shell>
```

Isso resulta em uma imagem de tela de fundo que aparece no submenu:

![Captura de tela de uma imagem de fundo de submenu](flyout-images/flyout-backgroundimage.png "Imagem da tela de fundo do submenu")

## <a name="flyout-items"></a>Itens de submenu

Quando o padrão de navegação para um aplicativo inclui um submenu, o objeto `Shell` na subclasse deve conter um ou mais objetos `FlyoutItem`, sendo que cada objeto `FlyoutItem` representa um item no submenu. Cada objeto `FlyoutItem` deve ser um filho do objeto `Shell`.

O exemplo a seguir cria um submenu que contém um cabeçalho de submenu e dois itens de submenu:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <FlyoutItem Title="Cats"
                Icon="cat.png">
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
    <FlyoutItem Title="Dogs"
                Icon="dog.png">
        <Tab>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

Neste exemplo, cada [`ContentPage`](xref:Xamarin.Forms.ContentPage) só pode ser acessado por meio dos itens do submenu:

[![Captura de tela de um aplicativo de duas páginas do shell com itens de submenu, no iOS e no Android](flyout-images/two-page-app-flyout.png "Aplicativo de duas páginas do shell com itens de submenu")](flyout-images/two-page-app-flyout-large.png#lightbox "Aplicativo de duas páginas do shell com itens de submenu")

> [!NOTE]
> Quando o cabeçalho de submenu não estiver presente, os itens do submenu serão exibidos na parte superior dele. Caso contrário, eles aparecerão abaixo do cabeçalho do submenu.

O Shell tem operadores de conversão implícita que permitem que a hierarquia visual no Shell seja simplificada, sem a introdução de modos de exibição adicionais na árvore visual. Isso é possível porque um objeto `Shell` na subclasse só pode conter objetos `FlyoutItem` ou um objeto `TabBar`, que só podem conter objetos `Tab`, que só podem conter objetos `ShellContent`. Esses operadores de conversão implícita podem ser usados para remover os objetos de `FlyoutItem`, `Tab` e `ShellContent` do exemplo anterior:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <views:CatsPage IconImageSource="cat.png" />
    <views:DogsPage IconImageSource="dog.png" />
</Shell>
```

Essa conversão implícita encapsula automaticamente cada objeto [`ContentPage`](xref:Xamarin.Forms.ContentPage) nos objetos de `ShellContent`, que são encapsulados nos objetos de `Tab`, que são encapsulados nos objetos de `FlyoutItem`.

> [!IMPORTANT]
> Em um aplicativo do Shell, cada [`ContentPage`](xref:Xamarin.Forms.ContentPage) que é filho de um objeto `ShellContent` é criado durante a inicialização do aplicativo. Adicionar outros objetos `ShellContent` usando essa abordagem fará com que sejam criadas outras páginas durante a inicialização do aplicativo, o que pode levar a uma experiência ruim de inicialização. No entanto, o Shell também é capaz de criar páginas sob demanda, em resposta à navegação. Saiba mais em [Carregamento de páginas eficiente](tabs.md#efficient-page-loading) no guia sobre as [abas do Shell do Xamarin.Forms](tabs.md).

### <a name="flyoutitem-class"></a>Classe FlyoutItem

A classe `FlyoutItem` inclui as seguintes propriedades que controlam o comportamento e a aparência do item do submenu:

- `FlyoutDisplayOptions`, do tipo `FlyoutDisplayOptions`, define como o item e seus filhos são exibidos no submenu. O valor padrão é `AsSingleItem`.
- `CurrentItem`, do tipo `Tab`, o item selecionado.
- `Items`, do tipo `IList<Tab>`, define todas as guias dentro de um `FlyoutItem`.
- `FlyoutIcon`, do tipo `ImageSource`, o ícone a ser usado para o item. Se essa propriedade não for definida, ele fará o fallback para usar o valor da propriedade `Icon`.
- `Icon`, do tipo `ImageSource`, define o ícone a ser exibido em partes do cromado que não são o submenu.
- `IsChecked`, do tipo `boolean`, define se o item está realçado naquele momento no submenu.
- `IsEnabled`, do tipo `boolean`, define se o item é selecionável no cromado.
- `IsTabStop`, do tipo `bool`, indica se um `FlyoutItem` está incluído na navegação por tabulação. Seu valor padrão é `true`, e quando o valor é `false`, o `FlyoutItem` é ignorado pela infraestrutura da navegação por tabulação, independentemente de `TabIndex` estar definido.
- `TabIndex`, do tipo `int`, indica a ordem na qual os objetos de `FlyoutItem` recebem o foco quando o usuário navega pelos itens pressionando a tecla Tab. O valor padrão da propriedade é 0.
- `Title`, do tipo `string`, o título a ser exibido na interface do usuário.
- `Route`, do tipo `string`, a cadeia de caracteres usada para resolver o item.

Todas essas propriedades, com exceção da propriedade `Route`, são apoiadas pelos objetos da [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), o que significa que as propriedades podem ser o destino das vinculações de dados.

> [!NOTE]
> Todos os objetos de `FlyoutItem` em um objeto Shell na subclasse são adicionados à coleção `Shell.Items`, que define a lista de itens que será exibida no submenu.

Além disso, a classe `FlyoutItem` expõe os seguintes métodos substituíveis:

- `OnTabIndexPropertyChanged`, chamado sempre que a propriedade `TabIndex` muda.
- `OnTabStopPropertyChanged`, chamado sempre que a propriedade `IsTabStop` muda.
- `TabIndexDefaultValueCreator`, retorna um `int` e é chamado para definir o valor padrão da propriedade `TabIndex`.
- `TabStopDefaultValueCreator`, retorna um `bool` e é chamado para definir o valor padrão da propriedade `TabStop`.

## <a name="flyout-vertical-scroll"></a>Rolagem vertical do submenu

Por padrão, um submenu pode ser rolado verticalmente quando os itens do submenu não se ajustam ao submenu. Esse comportamento pode ser alterado definindo a propriedade `Shell.FlyoutVerticalScrollMode` vinculável como um dos membros da enumeração `ScrollMode`:

- `Disabled` – indica que a rolagem vertical será desabilitada.
- `Enabled` – indica que a rolagem vertical será habilitada.
- `Auto` – indica que a rolagem vertical será habilitada se os itens do submenu não couberem no submenu. Este é o valor padrão da propriedade `Shell.FlyoutVerticalScrollMode`.

O exemplo a seguir mostra como desabilitar a rolagem vertical:

```xaml
<Shell ...
       FlyoutVerticalScrollMode="Disabled"
    ...
</Shell>
```

## <a name="flyout-display-options"></a>Opções de exibição do submenu

A enumeração `FlyoutDisplayOptions` define os seguintes membros:

- `AsSingleItem`, indica que o item ficará visível como um item único.
- `AsMultipleItems`, indica que o item e seus filhos ficarão visíveis no submenu como um grupo de itens.

Ao definir a propriedade `FlyoutItem.FlyoutDisplayOptions` como `AsMultipleItems`, será criado um item do submenu para cada `Tab` dentro de um `FlyoutItem`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       FlyoutHeaderBehavior="CollapseOnScroll"
       x:Class="Xaminals.AppShell">

    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>

    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png"
                          ContentTemplate="{DataTemplate views:CatsPage}" />
            <ShellContent Title="Dogs"
                          Icon="dog.png"
                          ContentTemplate="{DataTemplate views:DogsPage}" />
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png"
                      ContentTemplate="{DataTemplate views:MonkeysPage}" />
        <ShellContent Title="Elephants"
                      Icon="elephant.png"
                      ContentTemplate="{DataTemplate views:ElephantsPage}" />  
        <ShellContent Title="Bears"
                      Icon="bear.png"
                      ContentTemplate="{DataTemplate views:BearsPage}" />
    </FlyoutItem>

    <ShellContent Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />    
</Shell>
```

Neste exemplo, os itens do submenu foram criados para o objeto `Tab` que é um filho do objeto `FlyoutItem` e os objetos de `ShellContent` que são filhos do objeto `FlyoutItem`. Isso ocorre porque cada objeto `ShellContent` que é um filho do objeto `FlyoutItem` é automaticamente encapsulado em um objeto `Tab`. Além disso, um item do submenu é criado para o último objeto `ShellContent`, que é encapsulado em um objeto `Tab` e, em seguida, em um objeto `FlyoutItem`.

Isso resulta nos seguintes itens de submenu:

[![Captura de tela do submenu contendo objetos FlyoutItem, no iOS e no Android](flyout-images/flyout-reduced.png "Submenu do Shell contendo objetos FlyoutItem")](flyout-images/flyout-reduced-large.png#lightbox "Submenu do Shell contendo objetos FlyoutItem")

## <a name="define-flyoutitem-appearance"></a>Definir a aparência de FlyoutItem

A aparência de cada `FlyoutItem` pode ser personalizada definindo a propriedade anexada `Shell.ItemTemplate` como um [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell ...>
    ...
    <Shell.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding FlyoutIcon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Title}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.ItemTemplate>
</Shell>
```

Este exemplo exibe o título de cada objeto `FlyoutItem` em itálico:

[![Captura de tela dos objetos modelados FlyoutItem, no iOS e no Android](flyout-images/flyoutitem-templated.png "Objetos FlyoutItem modelo de Shell")](flyout-images/flyoutitem-templated-large.png#lightbox "Objetos FlyoutItem modelo de Shell")


Como `Shell.ItemTemplate` é uma propriedade anexada, modelos diferentes podem ser anexados a objetos `FlyoutItem` específicos.

> [!NOTE]
> O Shell fornece as propriedades `Title` e `FlyoutIcon` para o [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) do `ItemTemplate`.


### <a name="default-template-for-flyoutitems-and-menuitems"></a>Modelo padrão para FlyoutItems e MenuItems
O Shell usa o modelo a seguir internamente para sua implementação padrão. Esse é um ótimo ponto de partida se tudo o que você deseja fazer é fazer pequenos ajustes nos layouts existentes. Isso também demonstra os recursos do Gerenciador de estado visual dos itens do submenu. Esse mesmo modelo também pode ser usado para MenuItems

```xaml
<DataTemplate x:Key="FlyoutTemplates">
    <Grid HeightRequest="{x:OnPlatform Android=50}">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroupList>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal">
                    </VisualState>
                    <VisualState x:Name="Selected">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="#F2F2F2" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateGroupList>
        </VisualStateManager.VisualStateGroups>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="{x:OnPlatform Android=54, iOS=50}"></ColumnDefinition>
            <ColumnDefinition Width="*"></ColumnDefinition>
        </Grid.ColumnDefinitions>
        <Image Source="{Binding FlyoutIcon}"
            VerticalOptions="Center"
            HorizontalOptions="Center"
            HeightRequest="{x:OnPlatform Android=24, iOS=22}"
            WidthRequest="{x:OnPlatform Android=24, iOS=22}">
        </Image>
        <Label VerticalOptions="Center"
                Text="{Binding Title}"
                FontSize="{x:OnPlatform Android=14, iOS=Small}"
                FontAttributes="Bold" Grid.Column="1">
            <Label.TextColor>
                <OnPlatform x:TypeArguments="Color">
                    <OnPlatform.Platforms>
                        <On Platform="Android" Value="#D2000000" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Label.TextColor>
            <Label.Margin>
                <OnPlatform x:TypeArguments="Thickness">
                    <OnPlatform.Platforms>
                        <On Platform="Android" Value="20, 0, 0, 0" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Label.Margin>
            <Label.FontFamily>
                <OnPlatform x:TypeArguments="x:String">
                    <OnPlatform.Platforms>
                        <On Platform="Android" Value="sans-serif-medium" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Label.FontFamily>
        </Label>
    </Grid>
</DataTemplate>
```

## <a name="flyoutitem-tab-order"></a>Ordem de tabulação do FlyoutItem

Por padrão, a ordem de tabulação dos objetos `FlyoutItem` é a mesma ordem em que eles são listados no XAML ou em que são adicionados de maneira programática a uma coleção filha. Essa é a ordem em que ocorrerá a navegação dos objetos `FlyoutItem` por meio do teclado e, geralmente, essa ordem padrão é a melhor ordem.

A ordem de tabulação padrão pode ser alterada definindo a propriedade `FlyoutItem.TabIndex`, que indica a ordem em que os objetos `FlyoutItem` recebem o foco quando o usuário navega pelos itens ao pressionar a tecla Tab. O valor padrão da propriedade é 0 e ela pode ser definida como qualquer valor de `int`.

As seguintes regras se aplicam ao usar a ordem de tabulação padrão ou ao definir a propriedade `TabIndex`:

- Os objetos `FlyoutItem` com um `TabIndex` igual a 0 são adicionados à ordem de tabulação com base em sua ordem de declaração no XAML ou nas coleções filhas.
- Os objetos `FlyoutItem` com um `TabIndex` maior que 0 são adicionados à ordem de tabulação com base em seu valor do `TabIndex`.
- Os objetos `FlyoutItem` com um `TabIndex` menor que 0 são adicionados à ordem de tabulação e aparecem antes de qualquer valor zero.
- Conflitos em um `TabIndex` são resolvidos pela ordem de declaração.

Após definir uma ordem de tabulação, ao pressionar a tecla Tab, o foco passará pelos objetos `FlyoutItem` na ordem ascendente do `TabIndex` e voltam ao início quando o último objeto é atingido.

Além de definir a ordem de tabulação dos objetos `FlyoutItem`, pode ser necessário excluir alguns objetos da ordem de tabulação. Isso pode ser feito usando a propriedade `FlyoutItem.IsTabStop`, que indica se um `FlyoutItem` está incluído na navegação por tabulação. Seu valor padrão é `true`, e quando o valor é `false`, o `FlyoutItem` é ignorado pela infraestrutura da navegação por tabulação, independentemente de `TabIndex` estar definido.

## <a name="set-the-current-flyoutitem"></a>Configurar o FlyoutItem atual

A classe `Shell` tem uma propriedade associável denominada `CurrentItem`, do tipo `FlyoutItem`, que representa o `FlyoutItem` selecionado naquele momento. Quando o aplicativo Shell é executado pela primeira vez, essa propriedade será definida como o primeiro `FlyoutItem` no objeto `Shell` na subclasse. No entanto, a propriedade pode ser definida para outro `FlyoutItem`, conforme mostrado no exemplo a seguir:

```xaml
<Shell ...
       CurrentItem="{x:Reference aboutItem}">
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        ...
    </FlyoutItem>
    <ShellContent x:Name="aboutItem"
                  Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />
</Shell>
```

Esse código define o objeto `ShellContent` denominado `aboutItem` como a propriedade `CurrentItem`, o que faz com que ele seja exibido. Nesse exemplo, uma conversão implícita é usada para encapsular o objeto `ShellContent` em um objeto `Tab`, que é encapsulado em um objeto `FlyoutItem`.

Este é o código C# equivalente:

```csharp
Shell.Current.CurrentItem = aboutItem;
```

## <a name="menu-items"></a>Itens de menu

Os itens do menu podem ser opcionalmente adicionados ao submenu, e cada item do menu é representado por um objeto [`MenuItem`](xref:Xamarin.Forms.MenuItem). A posição dos objetos `MenuItem` no submento depende da ordem de declaração deles na hierarquia visual do Shell. Portanto, qualquer objeto `MenuItem` declarado antes do objeto `FlyoutItem` aparecerá na parte superior do submenu, e qualquer objeto `MenuItem` declarado após `FlyoutItem` aparecerá na parte inferior do submenu.

> [!NOTE]
> A classe `MenuItem` tem um evento [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) e uma propriedade [`Command`](xref:Xamarin.Forms.MenuItem.Command). Portanto, os objetos `MenuItem` permitem cenários que executam uma ação em resposta ao `MenuItem` que está sendo tocado. Esses cenários incluem realizar a navegação e abrir um navegador em uma página da Web específica.

Os objetos [`MenuItem`](xref:Xamarin.Forms.MenuItem) podem ser adicionados ao submenu, conforme mostrado no exemplo a seguir:

```xaml
<Shell ...>
    ...            
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />    
</Shell>
```

Esse código adiciona dois objetos [`MenuItem`](xref:Xamarin.Forms.MenuItem) ao submenu, abaixo de todos os itens do submenu:

[![Captura de tela do submenu contendo objetos MenuItem, no iOS e no Android](flyout-images/flyout.png "Submenu do Shell contendo objetos MenuItem")](flyout-images/flyout-large.png#lightbox "Submenu do Shell contendo objetos MenuItem")

O primeiro objeto [`MenuItem`](xref:Xamarin.Forms.MenuItem) executa um `ICommand` denominado `RandomPageCommand`, que navega para uma página aleatória no aplicativo. O segundo objeto `MenuItem` executa um `ICommand` denominado `HelpCommand`, que abre a URL especificada pela propriedade `CommandParameter` em um navegador da Web.

> [!NOTE]
> O [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) de cada `MenuItem` é herdado do objeto `Shell` na subclasse.

## <a name="define-menuitem-appearance"></a>Definir a aparência do MenuItem

A aparência de cada `MenuItem` pode ser personalizada definindo a propriedade anexada `Shell.MenuItemTemplate` como um [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell ...>
    <Shell.MenuItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding Icon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Text}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.MenuItemTemplate>
    ...
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />  
</Shell>
```

Este exemplo anexa o `MenuItemTemplate` no nível do Shell a cada objeto `MenuItem`, exibindo o título de cada objeto `MenuItem` em itálico:

[![Captura de tela de objetos MenuItem com modelo, em iOS e Android](flyout-images/menuitem-templated.png "Objetos MenuItem modelo do Shell")](flyout-images/menuitem-templated-large.png#lightbox "Objetos MenuItem modelo do Shell")

> [!NOTE]
> O Shell fornece as propriedades [`Text`](xref:Xamarin.Forms.MenuItem.Text) e [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) à [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) do `MenuItemTemplate`. Você também pode usar `Title` no lugar de `Text` e `Icon` no lugar de `IconImageSource`, o que permitirá que você reutilize o mesmo modelo para itens de menu e itens de submenu

Como `Shell.MenuItemTemplate` é uma propriedade anexada, diferentes modelos podem ser anexados a objetos `MenuItem` específicos:

```xaml
<Shell ...>
    <Shell.MenuItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding Icon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Text}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.MenuItemTemplate>
    ...
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              Icon="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell">
        <Shell.MenuItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </Shell.MenuItemTemplate>
    </MenuItem>
</Shell>
```


> [!NOTE]
> O mesmo modelo usado para [itens de submenu](#default-template-for-flyoutitems-and-menuitems) também pode ser usado para itens de menu.

Este exemplo anexa o `MenuItemTemplate` no nível do Shell ao primeiro objeto `MenuItem` e anexa o `MenuItemTemplate` em linha ao segundo `MenuItem`.

## <a name="related-links"></a>Links relacionados

- [Xaminals (exemplo)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
