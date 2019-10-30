---
title: Quais configurações do projeto são necessárias para o depurador?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 856c04d129058e8cbac30dcdf619e8b2b5a66cb6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014258"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Quais configurações do projeto são necessárias para o depurador?

Para que o depurador funcione conforme o esperado (pontos de interrupção de acesso, exibir logs de depuração etc.), a instrumentação do desenvolvedor e a exibição de informações de depuração devem ser habilitadas.

Siga estas etapas para verificar as configurações do seu ambiente:

## <a name="visual-studio"></a>Visual Studio

1. Abrir as opções de projeto
2. Vá para **compilação > avançado...** Definir informações de depuração como **completas**
3. Configurações para cada plataforma:
   - Vá para **Opções do Android > opções de depuração**. Marque a caixa **habilitar instrumentação do desenvolvedor** .
   - Vá para **depuração do iOS > depuração de & instrumentação**. Marque a caixa de seleção **Habilitar depuração** .

## <a name="visual-studio-for-mac"></a>Visual Studio para Mac

1. Abrir as opções de projeto
2. Vá para **compilar > compilador > opções gerais**. Definir informações de depuração como **completas**
3. Configurações para cada plataforma:
    - Acesse **compilar > Android build > opções de depuração**. Marque a caixa **habilitar instrumentação do desenvolvedor** .
    - Vá para **compilar > depuração do IOS**. Marque a caixa de seleção **Habilitar depuração** .
