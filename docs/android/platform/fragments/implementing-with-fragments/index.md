---
title: Implementando fragmentos – passo a passos
description: Este artigo explica como usar fragmentos para desenvolver aplicativos Xamarin. Android.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/26/2018
ms.openlocfilehash: b601fc37cc75dcd43c3688de8d302f0a47a06b35
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027414"
---
# <a name="implementing-fragments---walkthrough"></a>Implementando fragmentos – passo a passos

_Os fragmentos são componentes autocontidos e modulares que podem ajudar a resolver a complexidade dos aplicativos Android que visam dispositivos com uma variedade de tamanhos de tela. Este artigo explica como criar e usar fragmentos ao desenvolver aplicativos Xamarin. Android._

## <a name="overview"></a>Visão Geral

Nesta seção, você examinará como criar e usar fragmentos em um aplicativo Xamarin. Android. Esse aplicativo exibirá os títulos de várias jogadas por William Shakespeare em uma lista. Quando o usuário toca no título de uma reprodução, o aplicativo exibirá uma cotação da reprodução em uma atividade separada:

[![aplicativo em execução em um telefone Android no modo retrato](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

Quando o telefone for girado para o modo paisagem, a aparência do aplicativo será alterada: a lista de execuções e citações aparecerão na mesma atividade. Quando uma reprodução for selecionada, a cotação será exibida na mesma atividade:

[![aplicativo em execução em um telefone Android no modo paisagem](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

Por fim, se o aplicativo estiver em execução em um Tablet:

[![aplicativo em execução em um tablet Android](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

Este aplicativo de exemplo pode se adaptar facilmente a diferentes fatores e orientações de forma com alterações mínimas de código usando fragmentos e [layouts alternativos](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources).

Os dados para o aplicativo existirão em duas matrizes de cadeia de caracteres codificadas no aplicativo C# como matrizes de cadeia de caracteres. Cada uma das matrizes servirá como a fonte de dados para um fragmento.  Uma matriz manterá o nome de algumas jogadas por Shakespeare, e a outra matriz conterá uma citação dessa reprodução. Quando o aplicativo for iniciado, ele exibirá os nomes de reprodução em um `ListFragment`. Quando o usuário clicar em uma reprodução na `ListFragment`, o aplicativo iniciará outra atividade que exibirá a cotação.

A interface do usuário para o aplicativo consistirá em dois layouts, um para retrato e outro para o modo paisagem. Em tempo de execução, o Android determinará qual layout será carregado com base na orientação do dispositivo e fornecerá esse layout à atividade a ser renderizada. Toda a lógica para responder aos cliques do usuário e exibir os dados será contida em fragmentos. As atividades no aplicativo existem somente como contêineres que irão hospedar os fragmentos.

Este tutorial será dividido em dois guias. A [primeira parte](./walkthrough.md) se concentrará nas partes principais do aplicativo. Um único conjunto de layouts (otimizado para o modo retrato) será criado, juntamente com dois fragmentos e duas atividades:

1. `MainActivity` &nbsp; essa é a atividade de inicialização para o aplicativo.
1. `TitlesFragment` &nbsp; este fragmento exibirá uma lista de títulos de reproduções que foram escritos por William Shakespeare. Ele será hospedado pelo `MainActivity`.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` iniciará o `PlayQuoteActivity` em resposta ao usuário, selecionando uma reprodução em `TitlesFragment`.
1. `PlayQuoteFragment` &nbsp; este fragmento exibirá uma cotação de uma reprodução por William Shakespeare. Ele será hospedado pelo `PlayQuoteActivity`.

A [segunda parte deste tutorial](./walkthrough-landscape.md) abordará a adição de um layout alternativo (otimizado para o modo paisagem), que exibirá ambos os fragmentos na tela. Além disso, algumas alterações de código secundárias serão feitas no código para que o aplicativo adapte seu comportamento ao número de fragmentos que são exibidos simultaneamente na tela.

## <a name="related-links"></a>Links relacionados

- [FragmentsWalkthrough (exemplo)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fragmentswalkthrough)
- [Visão geral do designer](~/android/user-interface/android-designer/index.md)
- [Implementando fragmentos](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Pacote de suporte](https://developer.android.com/sdk/compatibility-library.html)
