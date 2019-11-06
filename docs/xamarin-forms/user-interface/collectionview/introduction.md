---
title: Introdução de CollectionView do Xamarin. Forms
description: CollectionView é uma exibição flexível e de alto desempenho para apresentar listas de dados usando diferentes especificações de layout.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
ms.openlocfilehash: 871d7cad6c57cd34757ae992ce14d5f686935584
ms.sourcegitcommit: 283810340de5310f63ef7c3e4b266fe9dc2ffcaf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73662308"
---
# <a name="xamarinforms-collectionview-introduction"></a>Introdução de CollectionView do Xamarin. Forms

[![Baixar Exemplo](~/media/shared/download.png) Baixar o exemplo](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) é uma exibição para apresentar listas de dados usando especificações de layout diferentes. Ele visa fornecer uma alternativa mais flexível e de alto desempenho para [`ListView`](xref:Xamarin.Forms.ListView). Por exemplo, as capturas de tela a seguir mostram um `CollectionView` que usa uma grade vertical de duas colunas e que permite seleção múltipla:

[![Captura de tela de um layout de grade vertical CollectionView, no iOS e no Android](introduction-images/verticalgrid-multipleselection.png "Layout de grade vertical CollectionView com seleção múltipla")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "Layout de grade vertical CollectionView com seleção múltipla")

o [`CollectionView`](xref:Xamarin.Forms.CollectionView) está disponível no Xamarin. Forms 4,3.

> [!IMPORTANT]
> o [`CollectionView`](xref:Xamarin.Forms.CollectionView) está disponível no Ios e no Android, mas está [disponível apenas parcialmente](https://gist.github.com/hartez/7d0edd4182dbc7de65cebc6c67f72e14) no plataforma universal do Windows.

## <a name="collectionview-and-listview-differences"></a>Diferenças de CollectionView e ListView

Embora as APIs de [`CollectionView`](xref:Xamarin.Forms.CollectionView) e [`ListView`](xref:Xamarin.Forms.ListView) sejam semelhantes, há algumas diferenças notáveis:

- [`CollectionView`](xref:Xamarin.Forms.CollectionView) tem um modelo de layout flexível, que permite que os dados sejam apresentados vertical ou horizontalmente, em uma lista ou em uma grade.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) dá suporte a seleção única e múltipla.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) não tem conceito de células. Em vez disso, um modelo de dados é usado para definir a aparência de cada item de dados na lista.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) utiliza automaticamente a virtualização fornecida pelos controles nativos subjacentes.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) reduz a superfície de API do [`ListView`](xref:Xamarin.Forms.ListView). Muitas propriedades e eventos de [`ListView`](xref:Xamarin.Forms.ListView) não estão presentes no `CollectionView`.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) não inclui separadores internos.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) gerará uma exceção se seu [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) for atualizado do thread da interface do usuário.

## <a name="move-from-listview-to-collectionview"></a>Mover de ListView para CollectionView

[`ListView`](xref:Xamarin.Forms.ListView) implementações em implementações existentes do Xamarin. Forms podem ser migradas para [`CollectionView`](xref:Xamarin.Forms.CollectionView) implementações com a ajuda da seguinte tabela:

| Conceito | API de ListView | CollectionView |
|---|---|---|
| Dados | `ItemsSource` | Uma [`CollectionView`](xref:Xamarin.Forms.CollectionView) é populada com dados definindo sua propriedade `ItemsSource`. Para obter mais informações, consulte [popular um CollectionView com dados](populate-data.md#populate-a-collectionview-with-data). |
| Aparência do item | `ItemTemplate` | A aparência de cada item em um [`CollectionView`](xref:Xamarin.Forms.CollectionView) pode ser definida definindo a propriedade `ItemTemplate` como uma [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). Para obter mais informações, consulte [definir a aparência do item](populate-data.md#define-item-appearance). |
| Células | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) não tem conceito de células. Em vez disso, um modelo de dados é usado para definir a aparência de cada item de dados na lista. |
| Separadores de linha | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) não inclui separadores internos. Eles podem ser fornecidos, se desejado, no modelo de item. |
| Seleção | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) dá suporte a seleção única e múltipla. Para obter mais informações, consulte [seleção de CollectionView do Xamarin. Forms](selection.md). |
| Altura da linha | `HasUnevenRows`, `RowHeight` | Em um `CollectionView`, a altura da linha de cada item é determinada pela propriedade `ItemSizingStrategy`. Para obter mais informações, consulte [dimensionamento de item](layout.md#item-sizing).|
| Cache | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) usa automaticamente a virtualização fornecida pelos controles nativos subjacentes. |
| Cabeçalhos e rodapés | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) pode apresentar um cabeçalho e um rodapé que rolam com os itens da lista, por meio das propriedades `Header`, `Footer`, `HeaderTemplate` e `FooterTemplate`. Para obter mais informações, consulte [cabeçalhos e rodapés](layout.md#headers-and-footers). |
| Agrupamento | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) exibe dados agrupados corretamente definindo sua propriedade `IsGrouped` como `true`. Cabeçalhos de grupo e rodapés de grupo podem ser personalizados definindo as propriedades `GroupHeaderTemplate` e `GroupFooterTemplate` como [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) objetos. Para obter mais informações, consulte [agrupamento CollectionView do Xamarin. Forms](grouping.md). |
| Efetuar pull para atualizar | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Há suporte para a funcionalidade de pull para atualizar ao definir um [`CollectionView`](xref:Xamarin.Forms.CollectionView) como o filho de um `RefreshView`. Para obter mais informações, consulte [pull to Refresh](populate-data.md#pull-to-refresh). |
| Ações de contexto | `ContextActions` | No momento, não há suporte para ações de contexto no `CollectionView`, mas elas serão adicionadas em uma versão futura. |
| Rolagem | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) define `ScrollTo` métodos, que rolam itens para o modo de exibição. Para obter mais informações, consulte [rolagem](scrolling.md). |

## <a name="related-links"></a>Links relacionados

- [CollectionView (exemplo)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
