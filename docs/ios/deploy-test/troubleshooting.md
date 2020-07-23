---
title: Teste e implantação do Xamarin.iOS – solução de problemas
description: Este documento fornece dicas de solução de problemas relacionadas à assinatura de código e provisionamento, ao TestFlight e à cópia do lote de aplicativo do iOS do host de build do Mac para o Windows.
ms.prod: xamarin
ms.assetid: 65286D09-F74D-4F22-B6CD-D1BCD7FC7992
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/23/2017
ms.openlocfilehash: 12194eed21fc95aaadc194be8a57319cf5c3cd3d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937235"
---
# <a name="xamarinios-testing-and-deployment---troubleshooting"></a>Teste e implantação do Xamarin.iOS – solução de problemas

## <a name="code-signing--provisioning"></a>Assinatura de Código e Provisionamento

Assinatura de Código e Provisionamento com iOS podem ser bastante complicados e, portanto, é importante certificar-se de que o código de certificados de autenticação e provisionamento perfis estejam em ordem.

- Equipes grandes devem evitar o uso do botão “Corrigir problema” no Xcode, como ilustrado aqui:

    [![A caixa de diálogo corrigir problemas](troubleshooting-images/fixissue.png)](troubleshooting-images/fixissue.png#lightbox)

    Isso criará novos perfis de provisionamento e de certificados. Na melhor das hipóteses, isso criará um perfil de provisionamento sempre que um membro da equipe clicar nele, causando desordem nos perfis. Na pior das hipóteses, ele revogará certificados para qualquer outra pessoa da empresa, fazendo com que seus aplicativos parem de funcionar.

- Mantenha o Acesso de Conjunto de Chaves organizado e exclua certificados e perfis expirados. Certificados corporativos duram três anos, enquanto outros duram apenas um ano. Certificados não podem ser renovados, portanto, será necessário criar novos certificados antes dos antigos expirarem. Certifique-se de revogar e excluir certificados antigos e assinar novamente os aplicativos com novos certificados.

- Remova os perfis de provisionamento antigos conforme novos são instalados. Isso significa que o Visual Studio para Mac não está em uma posição em que ele deve decidir qual perfil a ser usado. Para conseguir isso, primeiro certifique-se de excluir o perfil no centro de desenvolvedores da Apple e, em seguida, navegue até *preferências > sua conta > exibir detalhes...*. Selecione o perfil de provisionamento e clique em **Mostrar no Finder**. Isso revelará o local do perfil no sistema de arquivos do Mac, no qual ele pode ser excluído usando o Finder.

- Certifique-se de que todos os certificados e as chaves privadas correspondentes estejam disponíveis. Para cada equipe será necessário obter um certificado de desenvolvedor (para instalar aplicativos em um dispositivo próprio) e um certificado de distribuição (para instalar em outros dispositivos)

- Reinicie o Xcode e o Visual Studio para Mac/Visual Studio quando um novo Perfil de Provisionamento ou Certificado for instalado.

## <a name="testflight"></a>TestFlight

Às vezes, o teste pode não ocorrer de forma tão tranquila como planejado.  As etapas a seguir podem ajudar a resolver problemas com TestFlight:

- O TestFlight só está disponível para aplicativos destinados ao iOS 8 e superior.

- Deve haver um *perfil de distribuição da App Store* com direitos de beta.

- A janela **Novo envio de Aplicativo iOS** deve conter exatamente as mesmas informações que **Info.plist** do aplicativo e todas as seções devem ser preenchidas. Ícones devem ser especificados para o aplicativo antes do carregamento para TestFlight.

- Ao carregar um novo build, levará entre 1 e 5 minutos até que o build seja exibido no iTunes Connect.

- O [comutador de teste Beta do TestFlight](~/ios/deploy-test/testflight.md#beta-testing) deve estar ativado para cada versão do seu aplicativo.

- Cada membro da equipe do desenvolvedor que também é um testador interno deve ter o comutador **Testador interno** ativado.

- Os usuários que pertencem a uma conta do iTunes Connect ou que são proprietários e uma não podem ser testadores internos. Eles só podem ser adicionados como testadores externos.

- Usuários internos e externos são adicionados, selecionados e convidados separadamente. Cada lista deve ser gerenciada separadamente.

- A Apple deverá aprovar cada build que será distribuído aos testadores externos. Se a versão de um build for alterada, uma nova análise beta da Apple será necessária. Se o número de build for alterado, a análise será opcional.

- Os metadados devem ser adicionados às compilações distribuídas a testadores externos. Ele pode ser acessado ao clicar no número de build em **Meus Aplicativos > Pré-lançamento**.

- Apenas duas compilações podem ser enviadas para análise por dia. Como a alteração da versão força uma análise, isso significa que os números da versão só podem ser alterados duas vezes por dia.

<a name="Automatically_copy_app_bundles_back_to_Windows"></a>

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Copiar automaticamente os pacotes do .app para o Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]
