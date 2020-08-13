---
title: Xamarin.Forms Marcação C#
description: A marcação C# é um conjunto opcional de métodos e classes auxiliares fluentes para simplificar o processo de criação de interfaces de usuário declarativas Xamarin.Forms em C#.
ms.prod: xamarin
ms.assetid: D41B9DCD-5C34-4C2F-B177-FC082AB2E9E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3e8c95e84dd525c274403290712260ba86abd677
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918566"
---
# <a name="no-locxamarinforms-c-markup"></a>Xamarin.Forms Marcação C#

![API de pré-lançamento](~/media/shared/preview.png)

[![Baixar Exemplo](~/media/shared/download.png) Baixar o exemplo](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-csharpmarkupdemos/)

A marcação C# é um conjunto opcional de métodos e classes auxiliares fluentes para simplificar o processo de criação de interfaces de usuário declarativas Xamarin.Forms em C#. A API fluente fornecida pela marcação C# está disponível no `Xamarin.Forms.Markup` namespace.

Assim como acontece com o XAML, a marcação C# permite uma separação clara entre marcação de interface do usuário e lógica da interface do usuário. Isso pode ser obtido separando a marcação da interface do usuário e a lógica da interface do usuário em arquivos de classe parcial distintos. Por exemplo, para uma página de logon, a marcação de interface do usuário estaria em um arquivo chamado *LoginPage.cs*, enquanto a lógica da interface do usuário estaria em um arquivo chamado *LoginPage.Logic.cs*.

A marcação C# está disponível em Xamarin.Forms 4,6. No entanto, ele está experimental e só pode ser usado adicionando a seguinte linha de código ao arquivo *app.cs* :

```csharp
Device.SetFlags(new string[]{ "Markup_Experimental" });
```

> [!NOTE]
> A marcação C# está disponível em todas as plataformas com suporte no Xamarin.Forms .

## <a name="basic-example"></a>Exemplo básico

O exemplo a seguir mostra a configuração do conteúdo da página para um novo [`Grid`](xref:Xamarin.Forms.Grid) contendo um [`Label`](xref:Xamarin.Forms.Label) e um [`Entry`](xref:Xamarin.Forms.Entry) , em C#:

```csharp
Grid grid = new Grid();

Label label = new Label { Text = "Code: " };
grid.Children.Add(label, 0, 1);

Entry entry = new Entry
{
    Placeholder = "Enter number",
    Keyboard = Keyboard.Numeric,
    BackgroundColor = Color.AliceBlue,
    TextColor = Color.Black,
    FontSize = 15,
    HeightRequest = 44,
    Margin = fieldMargin
};
grid.Children.Add(entry, 0, 2);
Grid.SetColumnSpan(entry, 2);
entry.SetBinding(Entry.TextProperty, new Binding("RegistrationCode"));

Content = grid;
```

Este exemplo cria um [`Grid`](xref:Xamarin.Forms.Grid) objeto, com filho [`Label`](xref:Xamarin.Forms.Label) e [`Entry`](xref:Xamarin.Forms.Entry) objetos. O `Label` exibe texto e os `Entry` dados são associados à `RegistrationCode` Propriedade do ViewModel. Cada exibição filho é definida para aparecer em uma linha específica no `Grid` e o `Entry` abrange todas as colunas no `Grid` . Além disso, a altura do `Entry` é definida, junto com seu teclado, cores, o tamanho da fonte de seu texto e seu `Margin` . Por fim, a `Page.Content` propriedade é definida como o `Grid` objeto.

A marcação C# permite que esse código seja reescrito usando sua API fluente:

```csharp
using Xamarin.Forms.Markup;
using static Xamarin.Forms.Markup.GridRowsColumns;

Content = new Grid
{
  Children =
  {
    new Label { Text = "Code:" }
               .Row (BodyRow.CodeHeader) .Column (BodyCol.Header),

    new Entry { Placeholder = "Enter number", Keyboard = Keyboard.Numeric, BackgroundColor = Color.AliceBlue, TextColor = Color.Black } .Font (15)
               .Row (BodyRow.CodeEntry) .ColumnSpan (All<BodyCol>()) .Margin (fieldMargin) .Height (44)
               .Bind (nameof(vm.RegistrationCode))
  }
}};
```

Este exemplo é idêntico ao exemplo anterior, mas a API Fluent de marcação do C# simplifica o processo de criação da interface do usuário em C#.

> [!NOTE]
> A marcação C# inclui métodos de extensão que definem propriedades de exibição específicas. Esses métodos de extensão não se destinam a substituir todos os setters de propriedade. Em vez disso, eles foram projetados para melhorar a legibilidade do código e podem ser usados em combinação com setters de propriedade. É recomendável sempre usar um método de extensão quando houver um para uma propriedade, mas você pode escolher seu saldo preferido.

## <a name="data-binding"></a>Associação de dados

A marcação C# inclui um `Bind` método de extensão, juntamente com sobrecargas, que cria uma associação de dados entre uma propriedade de exibição vinculável e uma propriedade especificada. O `Bind` método conhece a propriedade vinculável padrão para a maioria dos controles incluídos no Xamarin.Forms . Portanto, normalmente não é necessário especificar a propriedade de destino ao usar esse método. No entanto, você também pode registrar a propriedade vinculável padrão para controles adicionais:

```csharp
using Xamarin.Forms.Markup;
//...

DefaultBindableProperties.Register(HoverButton.CommandProperty, RadialGauge.ValueProperty);
```

O `Bind` método pode ser usado para associar a qualquer propriedade vinculável:

```csharp
using Xamarin.Forms.Markup;
// ...

new Label { Text = "No data available" }
           .Bind (Label.IsVisibleProperty, nameof(vm.Empty))
```

Além disso, o `BindCommand` método de extensão pode se associar ao padrão de um controle `Command` e às `CommandParameter` Propriedades em uma única chamada de método:

```csharp
using Xamarin.Forms.Markup;
// ...

new TextCell { Text = "Tap me" }
              .BindCommand (nameof(vm.TapCommand))
```

Por padrão, o `CommandParameter` é associado ao contexto de associação. Você também pode especificar o caminho de associação e a origem para o `Command` e as `CommandParameter` associações:

```csharp
using Xamarin.Forms.Markup;
// ...

new TextCell { Text = "Tap Me" }
              .BindCommand (nameof(vm.TapCommand), vm, nameof(Item.Id))
```

Neste exemplo, o contexto de associação é uma `Item` instância, portanto, você não precisa especificar uma origem para a `Id` `CommandParameter` associação.

Se você só precisa associar ao `Command` , você pode passar `null` para o `parameterPath` argumento do `BindCommand` método. Como alternativa, use o `Bind` método.

Você também pode registrar o padrão `Command` e `CommandParameter` as propriedades para controles adicionais:

```csharp
using Xamarin.Forms.Markup;
//...

DefaultBindableProperties.RegisterCommand(
    (CustomViewA.CommandProperty, CustomViewA.CommandParameterProperty),
    (CustomViewB.CommandProperty, CustomViewB.CommandParameterProperty)
);
```

O código do conversor embutido pode ser passado para o `Bind` método com os `convert` `convertBack` parâmetros e:

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tree" }
           .Bind (Label.MarginProperty, nameof(TreeNode.TreeDepth),
                  convert: (int depth) => new Thickness(depth * 20, 0, 0, 0))
```

Também há suporte para parâmetros de conversor seguro de tipo:

```csharp
using Xamarin.Forms.Markup;
//...

new Label { }
           .Bind (nameof(viewModel.Text),
                  convert: (string text, int repeat) => string.Concat(Enumerable.Repeat(text, repeat)))
```

Além disso, o código do conversor e as instâncias podem ser reutilizados com a `FuncConverter` classe:

```csharp
using Xamarin.Forms.Markup;
//...

FuncConverter<int, Thickness> treeMarginConverter = new FuncConverter<int, Thickness>(depth => new Thickness(depth * 20, 0, 0, 0));
new Label { Text = "Tree" }
           .Bind (Label.MarginProperty, nameof(TreeNode.TreeDepth), converter: treeMarginConverter),
```

A `FuncConverter` classe também dá suporte a `CultureInfo` objetos:

```csharp
using Xamarin.Forms.Markup;
//...

cultureAwareConverter = new FuncConverter<DateTimeOffset, string, int>(
    (date, daysToAdd, culture) => date.AddDays(daysToAdd).ToString(culture)
);
```

Também é possível associar dados a `Span` objetos que são especificados com a `FormattedText` Propriedade:

```csharp
using Xamarin.Forms.Markup;
//...

new Label { } .FormattedText (
    new Span { Text = "Built with " },
    new Span { TextColor = Color.Blue, TextDecorations = TextDecorations.Underline }
              .BindTapGesture (nameof(vm.ContinueToCSharpForMarkupCommand))
              .Bind (nameof(vm.Title))
)
```

### <a name="gesture-recognizers"></a>Reconhecedores de gestos

`Command``CommandParameter`as propriedades e podem ser vinculadas a dados `GestureElement` e `View` tipos usando os `BindClickGesture` métodos de `BindSwipeGesture` extensão, e `BindTapGesture` :

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tap Me" }
           .BindTapGesture (nameof(vm.TapCommand))
```

Este exemplo cria um reconhecedor de gesto do tipo especificado e o adiciona ao [`Label`](xref:Xamarin.Forms.Label) . Os `Bind*Gesture` métodos de extensão oferecem os mesmos parâmetros que os `BindCommand` métodos de extensão. No entanto, por padrão, o não se `Bind*Gesture` associa `CommandParameter` , enquanto `BindCommand` o faz.

Para inicializar um reconhecedor de gesto com parâmetros, use os `ClickGesture` métodos de extensão,, `PanGesture` `PinchGesture` , `SwipeGesture` e `TapGesture` :

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tap Me" }
           .TapGesture (g => g.Bind(nameof(vm.DoubleTapCommand)).NumberOfTapsRequired = 2)
```

Como um reconhecedor de gesto é um `BindableObject` , você pode usar os `Bind` métodos de `BindCommand` extensão e ao inicializá-lo. Você também pode inicializar tipos de reconhecedor de gestos personalizados com o `Gesture<TGestureElement, TGestureRecognizer>` método de extensão.

## <a name="layout"></a>Layout

A marcação C# inclui uma série de métodos de extensão de layout que dão suporte ao posicionamento de exibições em layouts e conteúdo em exibições:

| Type | Métodos de extensão |
|---|---|
| `FlexLayout` | `AlignSelf`, `Basis`, `Grow`, `Menu`, `Order`, `Shrink` |
| `Grid` | `Row`, `Column`, `RowSpan`, `ColumnSpan` |
| `Label` | `TextLeft`, `TextCenterHorizontal`, `TextRight` <br/> `TextTop`, `TextCenterVertical`, `TextBottom` <br/> `TextCenter` |
| `Layout` | `Padding`, `Paddings` |
| `LayoutOptions` | `Left`, `CenterHorizontal`, `FillHorizontal`, `Right` <br/> `LeftExpand`, `CenterExpandHorizontal`, `FillExpandHorizontal`, `RightExpand` <br /> `Top`, `Bottom`, `CenterVertical`, `FillVertical` <br /> `TopExpand`, `BottomExpand`, `CenterExpandVertical`, `FillExpandVertical` <br /> `Center`, `Fill`, `CenterExpand`, `FillExpand` |
| `View` | `Margin`, `Margins` |
| `VisualElement` | `Height`, `Width`, `MinHeight`, `MinWidth`, `Size`, `MinSize` |

### <a name="left-to-right-and-right-to-left-support"></a>Suporte da esquerda para a direita e da direita para a esquerda

Para a marcação C# projetada para dar suporte à direção de fluxo da esquerda para a direita (EPD) ou da direita para a esquerda (RTL), os métodos de extensão listados acima oferecem o conjunto de nomes mais intuitivo: `Left` , `Right` `Top` e `Bottom` .

Para tornar o conjunto correto de métodos de extensão esquerdo e direito disponíveis e, no processo, tornar explícito para qual direção de fluxo a marcação é projetada, inclua uma das duas `using` diretivas a seguir: `using Xamarin.Forms.Markup.LeftToRight;` ou `using Xamarin.Forms.Markup.RightToLeft;` .

Para a marcação C# projetada para dar suporte à direção de fluxo da esquerda para a direita e da direita para a esquerda, é recomendável usar os métodos de extensão na tabela a seguir, em vez de um dos namespaces acima:

| Type | Métodos de extensão |
|---|---|
| `Label` | `TextStart`, `TextEnd` |
| `LayoutOptions` | `Start`, `End` <br/> `StartExpand`, `EndExpand` |

### <a name="layout-line-convention"></a>Convenção de linha de layout

A Convenção recomendada é colocar todos os métodos de extensão de layout para uma exibição em uma única linha na seguinte ordem:

1. A linha e a coluna que contêm a exibição.
1. Alinhamento dentro da linha e da coluna.
1. Margens em volta da exibição.
1. Tamanho da exibição.
1. Preenchimento dentro da exibição.
1. Alinhamento de conteúdo dentro do preenchimento.

O código a seguir mostra um exemplo dessa Convenção:

```csharp
new Label { }
           .Row (BodyRow.Prompt) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .CenterVertical () .Margin (fieldNameMargin) .TextCenterHorizontal () // Layout line
```

Ao seguir a Convenção consistentemente, você poderá ler rapidamente a marcação C# e criar um mapa mental de onde o conteúdo da exibição está localizado na interface do usuário.

## <a name="grid-rows-and-columns"></a>Linhas e colunas de grade

As enumerações podem ser usadas para definir [`Grid`](xref:Xamarin.Forms.Grid) linhas e colunas, em vez de usar números. Isso oferece a vantagem de que a renumeração não é necessária ao adicionar ou remover linhas ou colunas.

> [!IMPORTANT]
> Definir [`Grid`](xref:Xamarin.Forms.Grid) linhas e colunas usando enumerações requer a seguinte `using` diretiva:`using static Xamarin.Forms.Markup.GridRowsColumns;`

O código a seguir mostra um exemplo de como definir e consumir [`Grid`](xref:Xamarin.Forms.Grid) linhas e colunas usando enumerações:

```csharp
using Xamarin.Forms.Markup;
using Xamarin.Forms.Markup.LeftToRight;
using static Xamarin.Forms.Markup.GridRowsColumns;
// ...

enum BodyRow
{
    Prompt,
    CodeHeader,
    CodeEntry,
    Button
}

enum BodyCol
{
    FieldLabel,
    FieldValidation
}

View Build() => new Grid
{
    RowDefinitions = Rows.Define(
        (BodyRow.Prompt    , 170 ),
        (BodyRow.CodeHeader, 75  ),
        (BodyRow.CodeEntry , Auto),
        (BodyRow.Button    , Auto)
    ),

    ColumnDefinitions = Columns.Define(
        (BodyCol.FieldLabel     , 160 ),
        (BodyCol.FieldValidation, Star)
    ),

    Children =
    {
        new Label { LineBreakMode = LineBreakMode.WordWrap } .Font (15) .Bold ()
                   .Row (BodyRow.Prompt) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .CenterVertical () .Margin (fieldNameMargin) .TextCenterHorizontal ()
                   .Bind (nameof(vm.RegistrationPrompt)),

        new Label { Text = "Registration code" } .Bold ()
                   .Row (BodyRow.CodeHeader) .Column(BodyCol.FieldLabel) .Bottom () .Margin (fieldNameMargin),

        new Label { } .Italic ()
                   .Row (BodyRow.CodeHeader) .Column (BodyCol.FieldValidation) .Right () .Bottom () .Margin (fieldNameMargin)
                   .Bind (nameof(vm.RegistrationCodeValidationMessage)),

        new Entry { Placeholder = "E.g. 123456", Keyboard = Keyboard.Numeric, BackgroundColor = Color.AliceBlue, TextColor = Color.Black } .Font (15)
                   .Row (BodyRow.CodeEntry) .ColumnSpan (All<BodyCol>()) .Margin (fieldMargin) .Height (44)
                   .Bind (nameof(vm.RegistrationCode), BindingMode.TwoWay),

        new Button { Text = "Verify" } .Style (FilledButton)
                    .Row (BodyRow.Button) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .Margin (PageMarginSize)
                    .Bind (Button.IsVisibleProperty, nameof(vm.CanVerifyRegistrationCode))
                    .Bind (nameof(vm.VerifyRegistrationCodeCommand)),
    }
};
```

Além disso, você pode definir de forma concisa linhas e colunas sem enumerações:

```csharp
new Grid
{
    RowDefinitions = Rows.Define (Auto, Star, 20),
    ColumnDefinitions = Columns.Define (Auto, Star, 20, 40)
    // ...
}
```

## <a name="fonts"></a>Fontes

Os controles na lista a seguir podem chamar os `FontSize` métodos de extensão,, `Bold` `Italic` e `Font` para definir a aparência do texto exibido pelo controle:

- `Button`
- `DatePicker`
- `Editor`
- `Entry`
- `Label`
- `Picker`
- `SearchBar`
- `Span`
- `TimePicker`

## <a name="effects"></a>Efeitos

Os efeitos podem ser anexados a controles com o `Effect` método de extensão:

```csharp
using Xamarin.Forms.Markup;
// ...

new Button { Text = "Tap Me" }
            .Effects (new ButtonMixedCaps())
```

## <a name="logic-integration"></a>Integração de lógica

O `Invoke` método de extensão pode ser usado para executar código embutido em sua marcação C#:

```csharp
using Xamarin.Forms.Markup;
// ...

new ListView { } .Invoke (l => l.ItemTapped += OnListViewItemTapped)
```

Além disso, você pode usar o `Assign` método de extensão para acessar um controle de fora da marcação da interface do usuário (no arquivo lógico da interface do usuário):

```csharp
using Xamarin.Forms.Markup;
// ...

new ListView { } .Assign (out MyListView)
```

## <a name="styles"></a>Estilos

O exemplo a seguir mostra como criar estilos implícitos e explícitos usando a marcação C#:

```csharp
using Xamarin.Forms.Markup;
using Xamarin.Forms;

namespace CSharpForMarkupDemos
{
    public static class Styles
    {
        static Style<Button> buttons, filledButton;
        static Style<Label> labels;
        static Style<Span> link;

        #region Implicit styles

        public static ResourceDictionary Implicit => new ResourceDictionary { Buttons, Labels };

        public static Style<Button> Buttons => buttons ?? (buttons = new Style<Button>(
            (Button.HeightRequestProperty, 44),
            (Button.FontSizeProperty, 13),
            (Button.HorizontalOptionsProperty, LayoutOptions.Center),
            (Button.VerticalOptionsProperty, LayoutOptions.Center)
        ));

        public static Style<Label> Labels => labels ?? (labels = new Style<Label>(
            (Label.FontSizeProperty, 13),
            (Label.TextColorProperty, Color.Black)
        ));

        #endregion Implicit styles

        #region Explicit styles

        public static Style<Button> FilledButton => filledButton ?? (filledButton = new Style<Button>(
            (Button.TextColorProperty, Color.White),
            (Button.BackgroundColorProperty, Color.FromHex("#1976D2")),
            (Button.CornerRadiusProperty, 5)
        )).BasedOn(Buttons);

        public static Style<Span> Link => link ?? (link = new Style<Span>(
            (Span.TextColorProperty, Color.Blue),
            (Span.TextDecorationsProperty, TextDecorations.Underline)
        ));

        #endregion Explicit styles
    }
}
```

Os estilos implícitos podem ser consumidos carregando-os no dicionário de recursos do aplicativo:

```csharp
public App()
{
    Resources = Styles.Implicit;
    // ...
}
```

Estilos explícitos podem ser consumidos com o `Style` método de extensão.

```csharp
using static CSharpForMarkupExample.Styles;
// ...

new Button { Text = "Tap Me" } .Style (FilledButton),
```

> [!NOTE]
> Além do método de `Style` extensão, também há os `ApplyToDerivedTypes` métodos de `BasedOn` extensão,, `Add` e `CanCascade` .

Como alternativa, você pode criar seus próprios métodos de extensão de estilo:

```csharp
public static TButton Filled<TButton>(this TButton button) where TButton : Button
{
    button.Buttons(); // Equivalent to Style .BasedOn (Buttons)
    button.TextColor = Color.White;
    button.BackgroundColor = Color.Red;
    return button;
}
```

O `Filled` método de extensão pode ser consumido da seguinte maneira:

```csharp
new Button { Text = "Tap Me" } .Filled ()
```

## <a name="platform-specifics"></a>Elementos específicos da plataforma

O `Invoke` método de extensão pode ser usado para aplicar as especificações de plataforma. No entanto, para evitar erros de ambiguidade, não inclua `using` diretivas para os `Xamarin.Forms.PlatformConfiguration.*Specific` namespaces diretamente. Em vez disso, crie um alias de namespace e consuma a plataforma específica por meio do alias:

```csharp
using Xamarin.Forms.Markup;
using PciOS = Xamarin.Forms.PlatformConfiguration.iOSSpecific;
// ...

new ListView { } .Invoke (l => PciOS.ListView.SetGroupHeaderStyle(l, PciOS.GroupHeaderStyle.Grouped))
```

Além disso, se você consumir determinadas especificações de plataforma com frequência, poderá criar métodos de extensão fluente para eles em sua própria classe de extensões:

```csharp
public static T iOSGroupHeaderStyle<T>(this T listView, PciOS.GroupHeaderStyle style) where T : Forms.ListView
{
  PciOS.ListView.SetGroupHeaderStyle(listView, style);
  return listView;
}
```

O método de extensão pode ser consumido da seguinte maneira:

```csharp
new ListView { } .iOSGroupHeaderStyle(PciOS.GroupHeaderStyle.Grouped)
```

Para obter mais informações sobre especificações de plataforma, consulte [recursos da plataforma Android](~/xamarin-forms/platform/android/index.md), recursos da plataforma [Ios](~/xamarin-forms/platform/ios/index.md)e [recursos da plataforma Windows](~/xamarin-forms/platform/windows/index.md).

## <a name="recommended-convention"></a>Convenção recomendada

Uma ordem recomendada e o agrupamento de propriedades e métodos auxiliares é:

- **Finalidade**: qualquer propriedade ou método auxiliar cujo valor identifica a finalidade do controle (por exemplo `Text` , `Placeholder` ,,. `Assign` ).
- **Outro**: todas as propriedades ou métodos auxiliares que não são layout ou associação, na mesma linha ou em várias linhas.
- **Layout**: o layout é ordenado de maneira invertida: linhas e colunas, opções de layout, margem, tamanho, preenchimento e alinhamento de conteúdo.
- **Ligação**: a associação de dados é executada no final da cadeia de métodos, com uma propriedade associada por linha. Se a propriedade ligável *padrão* estiver associada, ela deverá estar no final da cadeia de métodos.

O código a seguir mostra um exemplo de como seguir esta Convenção:

```csharp
new Button { Text = "Verify" /* purpose */ } .Style (FilledButton) // other
            .Row (BodyRow.Button) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .Margin (10) // layout
            .Bind (Button.IsVisibleProperty, nameof(vm.CanVerifyRegistrationCode)) // bind
            .Bind (nameof(vm.VerifyRegistrationCodeCommand)), // bind default

new Label { }
           .Assign (out animatedMessageLabel) // purpose
           .Invoke (label => label.SizeChanged += MessageLabel_SizeChanged) // other
           .Row (BodyRow.Message) .ColumnSpan (All<BodyCol>()) // layout
           .Bind (nameof(vm.Message)), // bind default
```

Aplicar consistentemente essa Convenção permite que você examine rapidamente sua marcação em C# e crie uma imagem mental do layout da interface do usuário.

## <a name="related-links"></a>Links relacionados

- [CSharpForMarkupDemos (exemplo)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-csharpmarkupdemos/)
- [Recursos da plataforma Android](~/xamarin-forms/platform/android/index.md)
- [recursos da plataforma iOS](~/xamarin-forms/platform/ios/index.md)
- [Recursos da plataforma Windows](~/xamarin-forms/platform/windows/index.md)
