---
title: Xamarin para desenvolvedores de Objective-C
description: Este documento fornece uma descrição do Xamarin.iOS para desenvolvedores do Objective-C. Ele tem links para guias que descrevem como fazer a transição para o C# partindo do Objective-C, como associar uma biblioteca do Objective-C para uso em C# e como criar um aplicativo móvel multiplataforma.
ms.prod: xamarin
ms.assetid: 9F3C86A3-403E-4025-99CA-99FCA86DC828
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
ms.openlocfilehash: 2c5451b379968230d75599601052e3a33f5b5951
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73022437"
---
# <a name="xamarin-for-objective-c-developers"></a>Xamarin para desenvolvedores de Objective-C

O Xamarin oferece um caminho para que desenvolvedores de iOS movam seus códigos que não sejam da interface do usuário para a plataforma agnóstica C#, para que eles possam ser usados em qualquer lugar que o C# esteja disponível, incluindo no Android por meio do Xamarin.Android e de várias versões do Windows. No entanto, só porque você usa o C# com o Xamarin, isso não significa que não é possível aproveitar as habilitadas e o código do Objective-C existentes. Na verdade, conhecer o Objective-C faz de você um desenvolvedor do Xamarin.iOS melhor, porque o Xamarin expõe todas as APIs de plataformas iOS e OS X que você conhece e adora, como o UIKit, Animação Principal, Infraestrutura Principal e Elementos Gráficos Principais, só para citar alguns. Ao mesmo tempo, você tem o potencial da linguagem C#, incluindo recursos como LINQ e Generics, assim como bibliotecas avançadas de classe base de .NET para usar nos seus aplicativos nativos.

Além disso, o Xamarin permite aproveitar ativos existentes do Objective-C por meio de uma tecnologia conhecida como associações. Basta criar uma biblioteca estática em Objective-C e expô-la ao C# por meio de uma associação, conforme ilustrado no diagrama a seguir:

 [![](images/01-bindings.png "A static library in Objective-C exposed to C# via a binding")](images/01-bindings.png#lightbox)

Isso não precisa ser limitado a um código que não seja da interface do usuário. A associações podem expor o código da interface do usuário desenvolvido também em Objective-C.

## <a name="transitioning-from-objective-c"></a>Como fazer a transição do Objective-C

Você encontrará uma grande quantidade de informações no site de documentação para ajudar a facilitar a transição para o Xamarin, mostrando como integrar o código C# com o que você já sabe. Alguns destaques de introdução incluem:

- [Prévia do C# para Desenvolvedores do Objective-C](primer.md) – Uma breve prévia para desenvolvedores do Objective-C que desejam migrar para o Xamarin e a linguagem C#. 
- [Passo a passo: Como associar uma Biblioteca do Objective-C](~/ios/platform/binding-objective-c/walkthrough.md) - um passo a passo para reutilizar o código Objective-C existente em um aplicativo Xamarin.iOS. 

## <a name="binding-objective-c"></a>Associação do Objective-C

Depois de ter uma melhor compreensão de como o C# se compara ao Objective-C e realizar as etapas de associação acima, você estará pronto para fazer a transição para a plataforma do Xamarin. Como acompanhamento, é possível encontrar mais informações detalhadas sobre as tecnologias de associação no Xamarin.iOS, incluindo uma ampla referência de associações, na seção [Associação do Objective-C](~/ios/platform/binding-objective-c/index.md).

## <a name="cross-platform-development"></a>Desenvolvimento entre plataformas

Por fim, depois de mudar para o Xamarin.iOS, convém conferir as orientações para diferentes plataformas que temos, incluindo estudos de caso de aplicativos de referência que desenvolvemos, juntamente com melhores práticas para criar códigos reutilizáveis para diferentes plataformas contidos na [seção Compilação de Aplicativos em Diferentes Plataformas](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
