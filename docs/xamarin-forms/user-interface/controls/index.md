---
title: Referência de Controles
description: Uma descrição de todos os elementos da interface do usuário usados para construir um aplicativo Xamarin. Forms. Este artigo lista os grupos de controle que compõem a interface do usuário de um aplicativo xamarin. Forms.
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2019
ms.openlocfilehash: b9436603c17eb8008f470e75a52a93e8e122034f
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488096"
---
# <a name="controls-reference"></a>Referência de Controles

[![Baixar Exemplo](~/media/shared/download.png) Baixar o exemplo](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

A interface do usuário de um aplicativo Xamarin. Forms é construída de objetos que são mapeados para os controles nativos de cada plataforma de destino. Isso permite que aplicativos específicos da plataforma para iOS, Android e o Plataforma Universal do Windows usem o código Xamarin. Forms contido em uma [biblioteca de .net Standard](~/cross-platform/app-fundamentals/net-standard.md).

Os quatro grupos principais de controle usados para criar a interface do usuário de um aplicativo Xamarin. Forms são os seguintes:

- [**Páginas**](pages.md)
- [**Layouts**](layouts.md)
- [**Modos de exibição**](views.md)
- [**Células**](cells.md)

Geralmente, uma página do xamarin. Forms ocupa a tela inteira. A página normalmente contém um layout, que contém exibições e possivelmente outros layouts. As células são componentes especializados usados na conexão com [ `TableView` ](views.md#tableview) e [ `ListView` ](views.md#listview). Um diagrama de classe que mostra a hierarquia de tipos que normalmente são usados para criar uma interface do usuário no Xamarin. Forms pode ser encontrado na [hierarquia de classes de controles xamarin. Forms](~/xamarin-forms/internals/class-hierarchy.md).

Nos quatro artigos sobre [ **páginas**](pages.md), [ **Layouts**](layouts.md), [ **exibições** ](views.md), e [ **células**](cells.md), cada tipo de controle é descrito com links para documentação da API, um artigo que descreve seu uso (se houver) e um ou mais programas de exemplo (se existirem). Cada tipo de controle também acompanha uma captura de tela mostrando uma página do exemplo [**FormsGallery**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) em execução em dispositivos IOS e Android. Cada captura de tela abaixo estão os links para o código-fonte para a página em C#, a página XAML equivalente e (quando apropriado) o arquivo de code-behind do c# para a página XAML.

> [!NOTE]
> Páginas, layouts e exibições derivam da classe `VisualElement`. A classe `VisualElement` fornece uma variedade de propriedades, métodos e eventos que são úteis na derivação de classes. Para obter mais informações, consulte [Propriedades, métodos e eventos do visualelement](common-properties.md).

Além dos controles fornecidos com o Xamarin. Forms, os controles de terceiros estão disponíveis. Para obter mais informações, consulte [controles](thirdparty.md)de terceiros.

## <a name="related-links"></a>Links Relacionados

- [Exemplo de xamarin. Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Hierarquia de classes de controles Xamarin. Forms](~/xamarin-forms/internals/class-hierarchy.md)
- [Documentação da API](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
