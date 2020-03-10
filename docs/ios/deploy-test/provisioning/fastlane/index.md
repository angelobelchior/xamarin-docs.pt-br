---
title: Introdução ao fastlane para iOS
description: Este guia apresenta várias ferramentas do Fastlane que podem ser usadas para assinatura de código de aplicativos do iOS Ele descreve como atualizar, instalar e usar as ferramentas do fastlane.
ms.prod: xamarin
ms.assetid: 8202C57D-22FF-4224-A5B1-AAEF12B7C106
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 0af85c1c27d2b329d81cc680a0fc4c075d4a86dd
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915403"
---
# <a name="introduction-to-fastlane-for-ios"></a>Introdução ao fastlane para iOS

O fastlane é um projeto de software livre, criado para simplificar o processo geralmente tedioso e confuso de lançar versões de aplicativos iOS e Android. Ele é composto de vários utilitários e cada um lida com um aspecto específico da versão do aplicativo, como:

- [deliver](https://github.com/fastlane/fastlane/tree/master/deliver#readme) – gerencia e carrega capturas de tela, metadados e pacotes de aplicativos para o iTunes Connect.
- [produce](https://github.com/fastlane/fastlane/tree/master/produce#readme) – cria o aplicativo no iTunes Connect e no Portal do Desenvolvedor (geralmente conhecido como AppID). Ele também inclui suporte para grupos de aplicativos e serviços de aplicativos.
- [pem](https://github.com/fastlane/fastlane/tree/master/pem#readme) – cria e gerencia perfis de provisionamento de notificação por push.
- [gym](https://github.com/fastlane/fastlane/tree/master/gym#readme) – pode ser usado para criar e assinar um aplicativo iOS. (Os aplicativos Xamarin já usam o MSBuild para compilar, assinar e arquivar aplicativos)
- [cert](https://github.com/fastlane/fastlane/tree/master/cert#readme) – cria e gerencia certificados de assinatura de código 
- [sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme) – cria e gerencia perfis de provisionamento
- [match](https://github.com/fastlane/fastlane/tree/master/match#readme) – cria e mantém os certificados e os perfis e armazená-os em um repositório git para que eles possam ser sincronizados com uma equipe de desenvolvimento.

O fastlane pode ser usado de diferentes maneiras: pelos comandos de terminal, com base em arquivo ou usando variáveis de ambiente para compilações de integração contínua. 

Este guia aborda especificamente a configuração de um dispositivo para desenvolvimento com aplicativos iOS e tem como foco os utilitários **cert**, **sigh** e **match**. 

O conteúdo fornecido pode ser usado como um springboard para ajudar com a distribuição do aplicativo, inclusive automatizando totalmente o processo em um servidor de integração contínua. No entanto, é importante observar que fastlane é um terceiro que faz ferramentas para dar suporte a projetos do Xcode e, portanto, algumas ferramentas ou comandos como `fastlane init` podem não funcionar conforme o esperado com arquivos csproj. Saiba mais sobre o uso do fastlane, as ferramentas adicionais ou o lançamento de versões para Android usando o fastlane em [https://fastlane.tools/](https://fastlane.tools/)

<a name="Installation" />

## <a name="installation"></a>Instalação

1. Certifique-se de que as ferramentas da linha de comando do Xcode estão instaladas no computador macOS. Para instalar as ferramentas, use o comando `xcode-select --install` no Terminal. Se já estiverem instaladas, o seguinte erro será exibido:

    ```bash
    error: command line tools are already installed, use "Software Update" to install updates
    ```

2. Baixe as ferramentas do Fastlane em [https://download.fastlane.tools](https://download.fastlane.tools). 

    > [!NOTE]
    > É possível instalar as ferramentas do Fastlane desde o Homebrew usando `brew cask install fastlane` ou via Rubygems (2.0 ou superior) usando `sudo gem install fastlane –NV`. No entanto, usar o instalador garantirá que as dependências corretas estão disponíveis. 

3. Instale o fastlane descompactando o arquivo e clicando duas vezes no executável `install`. Se você receber um erro informando que o arquivo "não pode ser aberto porque pertence a um desenvolvedor não identificado", pressione OK e faça o seguinte:
    - CTRL + clique no executável `install`. A caixa de diálogo abaixo será exibida:

     ![](images/fastlane-image12.png "The install dialog")

    - Pressione OK para iniciar a instalação das ferramentas do fastlane

4. O terminal solicitará o diálogo ilustrado abaixo. Pressione `y`:

   ![](images/fastlane-image13.png "The Terminal prompt")

5. Execute o `which fastlane` antes de usar o Fastlane pela primeira vez. O caminho deve se parecer com o seguinte: 

    ```bash
    /Users/[user]/.fastlane/bin
    ```

6. Se o caminho corresponder ao acima, você estará pronto para começar.

     Caso contrário, faça o seguinte: no macOS, abra `.bash_profile`, que é um arquivo de texto não criptografado oculto no diretório base, com o seguinte comando:

    ```bash
    open ~/.bash_profile
    ```

7. Adicione a seguinte variável de ambiente PATH e salve-a: 

    ```bash
    export PATH="$HOME/.fastlane/bin:$PATH"
    ```

8. Execute `which fastlane` novamente para confirmar que o caminho é igual a `/Users/[user]/.fastlane/bin`

## <a name="updating-fastlane"></a>Atualização do fastlane

O fastlane é um projeto de software livre muito ativo que regularmente envia por push novas versões. Quando uma nova versão do fastlane estiver disponível, você será informado ao executar qualquer comando do fastlane:

[![](images/fastlane-image0.png "The fast lane update prompt")](images/fastlane-image0.png#lightbox)

Para atualizar para uma nova versão do Fastlane, baixe o pacote mais recente [aqui](https://download.fastlane.tools) e clique duas vezes no pacote de instalação para executá-lo:

[![](images/fastlane-image0a.png "Running the install package")](images/fastlane-image0a.png#lightbox)

## <a name="contents"></a>Conteúdo

Esta série de guias apresenta algumas das ferramentas que o Fastlane usa para a assinatura de código de seu aplicativo iOS em preparação para desenvolvimento ou distribuição. As ferramentas abordadas no momento são:

- [cert](~/ios/deploy-test/provisioning/fastlane/cert.md)
- [sigh](~/ios/deploy-test/provisioning/fastlane/sigh.md)
- [match](~/ios/deploy-test/provisioning/fastlane/match.md)

O cert e o sigh lidam com a criação e o gerenciamento de certificados de autenticação e perfis de provisionamento em um computador local. O match leva esse processo para uma etapa além. Ele cria e gerencia certificados e perfis de provisionamento e armazena-os em um repositório git, permitindo que eles sejam acessíveis por todos os membros de uma equipe de desenvolvimento. Leia cada seção para saber como eles funcionam e como usá-los.

## <a name="using-fastlane-tools-with-xamarin"></a>Uso de ferramentas do fastlane com o Xamarin

Depois de criar uma identidade de assinatura e perfis de provisionamento com fastlane, configurar as opções de assinatura de pacote no Visual Studio para Mac deve ser simples, desde que os certificados e chaves privadas estejam no conjunto de chaves macOS e que os perfis de provisionamento estejam na pasta `~/Library/MobileDevice/Provisioning Profiles`.

Para definir as opções de assinatura de código para um aplicativo do Xamarin.iOS, clique com o botão direito do mouse no nome do projeto, selecione **Opções de Projeto > Build > Assinatura do Pacote iOS** e defina a Identidade de Assinatura e o Perfil de Provisionamento explicitamente, como demonstrado a seguir:

[![](images/fastlane-image11.png "Set the Signing Identity and Provisioning Profile explicitly")](images/fastlane-image11.png#lightbox)

## <a name="related-links"></a>Links relacionados

- [Documentos do fastlane](https://fastlane.tools/)
- [Documentos de assinatura de código do fastlane](https://docs.fastlane.tools/codesigning/getting-started/)
- [Guia de assinatura de código](https://codesigning.guide/)
