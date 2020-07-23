---
title: Estruturas inseridas no Xamarin. iOS
description: Este documento descreve como compartilhar código com estruturas inseridas em um aplicativo Xamarin. iOS. Isso pode ser feito com a ferramenta mTouch ou as referências nativas.
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: 690aaf81ee2600bd792a36f14b81df3d15e2d21b
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930267"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Estruturas inseridas no Xamarin. iOS

_Este documento descreve como os desenvolvedores de aplicativos podem inserir estruturas de usuário em seus aplicativos._

Com o iOS 8,0, a Apple tornou possível criar uma estrutura inserida para compartilhar código entre extensões de aplicativo e o aplicativo principal no Xcode.

O Xamarin. iOS 9,0 adiciona suporte para o consumo dessas estruturas inseridas (criadas com o Xcode) em aplicativos Xamarin. iOS. ***Não** será possível criar estruturas inseridas de nenhum tipo de projeto Xamarin. Ios, consumir apenas estruturas nativas (objetiva-C) existentes.*

Há duas maneiras de consumir estruturas no Xamarin. iOS:

- Passe a estrutura para a ferramenta mTouch, adicionando o seguinte aos argumentos mTouch adicionais nas opções de **Build do IOS** do projeto:

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  Isso deve ser definido para cada configuração de projeto.

- Adicionar referências nativas no menu de contexto

# <a name="visual-studio-for-mac"></a>[Visual Studio para Mac](#tab/macos)

Clique com o botão direito do mouse no projeto e navegue até adicionar referências nativas

![Selecione Adicionar referências nativas no Visual Studio para Mac](embedded-frameworks-images/xam-native-refs.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Clique com o botão direito do mouse no projeto e navegue até adicionar referências nativas

![Selecione Adicionar referências nativas no Visual Studio](embedded-frameworks-images/vs-native-refs.png)

-----

  Isso funcionará para todas as configurações.

Em versões futuras do Visual Studio para Mac e as ferramentas do Xamarin para Visual Studio, será possível consumir estruturas de dentro do IDE (sem editar os arquivos de projeto manualmente).

Alguns projetos de exemplo podem ser encontrados no [GitHub](https://github.com/rolfbjarne/embedded-frameworks)

## <a name="limitations"></a>Limitações

- Estruturas inseridas só têm suporte em projetos [unificados](~/cross-platform/macios/unified/index.md) .
- Estruturas inseridas só têm suporte em projetos com um destino de implantação de pelo menos iOS 8,0.
- Se uma extensão exigir uma estrutura inserida, o aplicativo de contêiner também deverá ter uma referência à estrutura, caso contrário, a estrutura não será incluída no pacote de aplicativo.

## <a name="the-mono-runtime"></a>O tempo de execução mono

O Xamarin. iOS internamente aproveita esse recurso para vincular com o tempo de execução do mono como uma estrutura, em vez de vincular o tempo de execução do mono estaticamente em cada extensão e no aplicativo de contêiner.

Isso é feito automaticamente se o aplicativo de contêiner é um aplicativo unificado, ele contém extensões e a implantação de destino é iOS 8,0 ou superior.

Aplicativos sem extensões ainda serão vinculados com o tempo de execução mono estaticamente, pois há uma penalidade de tamanho para usar uma estrutura se houver apenas um aplicativo que faz referência a ela.

Esse comportamento pode ser substituído pelo desenvolvedor do aplicativo, adicionando o seguinte como um argumento mTouch adicional nas opções de Build do iOS do projeto:

- `--mono:static`: Links com o tempo de execução mono estaticamente.
- `--mono:framework`: Links com o tempo de execução mono como uma estrutura.

Um cenário para vincular com o tempo de execução do mono como uma estrutura mesmo para aplicativos sem extensões é diminuir o tamanho do executável para superar as restrições de tamanho que a Apple impõe no executável. Para referência, o tempo de execução do mono adiciona aproximadamente 1.7 MB por arquitetura (a partir do Xamarin. iOS 8,12, mas seu varia entre as versões e até mesmo entre aplicativos). A estrutura mono adiciona aproximadamente 2,3 MB por arquitetura, o que significa que, para um aplicativo de arquitetura única sem qualquer extensão, fazer com que o link do aplicativo com o tempo de execução mono como uma estrutura reduzirá o executável em aproximadamente 1.7 MB, mas adicionará uma estrutura de ~ 2,3 MB, resultando em um aplicativo de ~ 0,6 MB maior juntos.
