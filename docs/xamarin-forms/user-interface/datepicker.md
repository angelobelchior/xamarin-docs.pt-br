---
title: DatePicker do xamarin. Forms
description: O selecionador de data é um modo de exibição do xamarin. Forms que permite ao usuário selecionar uma data. Este artigo explica como consumir um selecionador de data em um aplicativo xamarin. Forms.
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 9bc6d47e4f935e84ae675eefeabac023920a344a
ms.sourcegitcommit: 191f1f3b13a14e2afadcb95126c5f653722f126f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/30/2019
ms.locfileid: "75545595"
---
# <a name="xamarinforms-datepicker"></a>DatePicker do xamarin. Forms

[![Baixar Exemplo](~/media/shared/download.png) Baixar o exemplo](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)

_Uma exibição do xamarin. Forms que permite que o usuário selecione uma data._

O xamarin. Forms [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) invoca o controle de seletor de data da plataforma e permite que o usuário selecione uma data. `DatePicker` define oito propriedades:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) do tipo [ `DateTime` ](xref:System.DateTime), cujo padrão é o primeiro dia do ano de 1900.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) do tipo `DateTime`, qual o padrão é o último dia do ano 2100.
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) do tipo `DateTime`, a data selecionada, cujo padrão é o valor [ `DateTime.Today` ](xref:System.DateTime.Today).
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) do tipo `string`, um [padrão](/dotnet/standard/base-types/standard-date-and-time-format-strings/) ou [personalizado](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET formatação de cadeia de caracteres, que assume como padrão "D", de longa data padrão.
- [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor) do tipo [ `Color` ](xref:Xamarin.Forms.Color), a cor usada para exibir a data selecionada, cujo padrão é [ `Color.Default` ](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) do tipo [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), cujo padrão é [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) do tipo `string`, cujo padrão é `null`.
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) do tipo `double`, cujo padrão é de -1,0.
- `CharacterSpacing`, do tipo `double`, é o espaçamento entre os caracteres do texto do `DatePicker`.

O `DatePicker` dispara uma [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) evento quando o usuário seleciona uma data.

> [!WARNING]
> Ao definir `MinimumDate` e `MaximumDate`, verifique se `MinimumDate` sempre é menor que ou igual a `MaximumDate`. Caso contrário, `DatePicker` gerarão uma exceção.

Internamente, o `DatePicker` garante que `Date` entre `MinimumDate` e `MaximumDate`, inclusive. Se `MinimumDate` ou `MaximumDate` é definido para que `Date` não está entre eles, `DatePicker` ajustará o valor de `Date`.

Todos os oito propriedades têm o respaldo [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) objetos, o que significa que eles podem ser estilizados e as propriedades podem ser alvos de vinculações de dados. O `Date` propriedade tem um modo de associação padrão de [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), que significa que ele pode ser um destino de associação de dados em um aplicativo que usa o [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) arquitetura.

## <a name="initializing-the-datetime-properties"></a>Inicializando as propriedades de data e hora

No código, você pode inicializar o `MinimumDate`, `MaximumDate`, e `Date` propriedades para valores do tipo `DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Quando um `DateTime` valor é especificado no XAML, o analisador XAML usa a `DateTime.Parse` método com um `CultureInfo.InvariantCulture` argumento para converter a cadeia de caracteres para um `DateTime` valor. As datas devem ser especificadas em um formato exato: dois dígitos meses, dias de dois dígitos e anos de quatro dígitos separados por barras "/":

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Se a propriedade `BindingContext` de `DatePicker` for definida como uma instância de um ViewModel que contém propriedades do tipo `DateTime` chamado `MinDate`, `MaxDate`e `SelectedDate` (por exemplo,), você poderá instanciar o `DatePicker` da seguinte maneira:

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

Neste exemplo, todas as três propriedades são inicializadas para as propriedades correspondentes no ViewModel. Como a propriedade `Date` tem um modo de associação de `TwoWay`, qualquer nova data que o usuário seleciona é refletida automaticamente no ViewModel.

Se o `DatePicker` não contém uma associação em seu `Date` propriedade, um aplicativo deve anexar um manipulador para o `DateSelected` evento a ser informado quando o usuário seleciona uma nova data.

Para obter informações sobre como definir propriedades de fonte, consulte [fontes](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="datepicker-and-layout"></a>Layout e DatePicker

É possível usar uma opção de layout horizontal irrestrito, como `Center`, `Start`, ou `End` com `DatePicker`:

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

No entanto, isso não é recomendado. Dependendo da configuração de `Format` propriedade, selecionada datas podem exigir que as larguras de exibição diferentes. Por exemplo, faz com que a cadeia de caracteres de formato "D" `DateTime` exibir datas em um formato longo e "Quarta-feira, 12 de setembro de 2018" requer uma largura de exibição maior que "Sexta-feira, 4 de maio de 2018". Dependendo da plataforma, essa diferença pode causar a `DateTime` exibição para alterar a largura de layout, ou para a exibição a ser truncado.

> [!TIP]
> É melhor usar o padrão `HorizontalOptions` configuração do `Fill` com `DatePicker`e não deve usar uma largura de `Auto` ao colocar `DatePicker` em um `Grid` célula.

## <a name="datepicker-in-an-application"></a>Selecionador de data em um aplicativo

O [ **DaysBetweenDates** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) exemplo inclui dois `DatePicker` exibições em sua página. Eles podem ser usados para selecionar as duas datas, e o programa calcula o número de dias entre essas datas. O programa não altera as configurações do `MinimumDate` e `MaximumDate` propriedades, portanto, as duas datas devem estar entre 1900 e 2100.

Aqui está o arquivo XAML:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DaysBetweenDates"
             x:Class="DaysBetweenDates.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <StackLayout Margin="10">
        <Label Text="Days Between Dates"
               Style="{DynamicResource TitleStyle}"
               Margin="0, 20"
               HorizontalTextAlignment="Center" />

        <Label Text="Start Date:" />

        <DatePicker x:Name="startDatePicker"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <Label Text="End Date:" />

        <DatePicker x:Name="endDatePicker"
                    MinimumDate="{Binding Source={x:Reference startDatePicker},
                                          Path=Date}"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <StackLayout Orientation="Horizontal"
                     Margin="0, 0, 0, 30">
            <Label Text="Include both days in total: "
                   VerticalOptions="Center" />
            <Switch x:Name="includeSwitch"
                    Toggled="OnSwitchToggled" />
        </StackLayout>

        <Label x:Name="resultLabel"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Cada `DatePicker` é atribuído um `Format` propriedade de "D" para um formato de data por extenso. Observe também que o `endDatePicker` objeto possui uma associação que tem como alvo seus `MinimumDate` propriedade. A origem da associação é selecionado `Date` propriedade do `startDatePicker` objeto. Isso garante que a data de término é sempre posterior ou igual à data de início. Além dos dois `DatePicker` objetos, um `Switch` é rotulado como "Incluir ambos os dias no total".

Os dois `DatePicker` modos de exibição têm manipuladores anexados para o `DateSelected` evento e o `Switch` tem um manipulador anexado seu `Toggled` eventos. Esses manipuladores de eventos estão no arquivo code-behind e disparam um novo cálculo dos dias entre as duas datas:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    void OnDateSelected(object sender, DateChangedEventArgs args)
    {
        Recalculate();
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        Recalculate();
    }

    void Recalculate()
    {
        TimeSpan timeSpan = endDatePicker.Date - startDatePicker.Date +
            (includeSwitch.IsToggled ? TimeSpan.FromDays(1) : TimeSpan.Zero);

        resultLabel.Text = String.Format("{0} day{1} between dates",
                                            timeSpan.Days, timeSpan.Days == 1 ? "" : "s");
    }
}
```

Quando o exemplo é primeiro executado, ambos `DatePicker` modos de exibição são inicializados a data de hoje. A captura de tela a seguir mostra o programa em execução no iOS e Android:

[![Dias entre as datas de início](datepicker-images/DaysBetweenDatesStart.png "Dias entre as datas de início")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "Dias entre as datas de início")

Tocando em qualquer uma da `DatePicker` exibe invoca o selecionador de data de plataforma. As plataformas de implementam esse seletor de data em duas maneiras diferentes, mas cada abordagem é familiar aos usuários da plataforma:

[![Dias entre as datas Select](datepicker-images/DaysBetweenDatesSelect.png "Dias entre as datas Select")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "Dias entre as datas Select")

> [!TIP]
> No Android, o `DatePicker` caixa de diálogo pode ser personalizada substituindo o `CreateDatePickerDialog` método em um renderizador personalizado. Isso permite, por exemplo, botões adicionais a serem adicionados à caixa de diálogo.

Depois de duas datas forem selecionadas, o aplicativo exibe o número de dias entre essas datas:

[![Dias entre os resultados de datas](datepicker-images/DaysBetweenDatesResult.png "Dias entre os resultados de datas")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "Dias entre os resultados de datas")

## <a name="related-links"></a>Links relacionados

- [Exemplo de DaysBetweenDates](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)
- [API de DatePicker](xref:Xamarin.Forms.DatePicker)
