---
title: Trabalhar com direitos no Xamarin.iOS
description: Direitos são capacidades especiais do aplicativo e permissões de segurança concedidas a aplicativos que estão configurados corretamente para usá-las.
ms.prod: xamarin
ms.assetid: 8A3961A2-02AB-4228-A41D-06CB4108D9D0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/13/2018
ms.openlocfilehash: f6a38aea1e46e1165bb36d83e75e24769de0a1e2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2020
ms.locfileid: "75655312"
---
# <a name="working-with-entitlements-in-xamarinios"></a>Trabalhar com direitos no Xamarin.iOS

_Direitos são capacidades especiais do aplicativo e permissões de segurança concedidas a aplicativos que estão configurados corretamente para usá-las._

No iOS, aplicativos são executados em uma _área restrita_, que fornece um conjunto de regras que limitam o acesso entre o aplicativo e certos recursos do sistema ou dados de usuário. _Direitos_ são usados para solicitar que o sistema expanda a área restrita para fornecer capacidades adicionais ao aplicativo.

Para estender as capacidades do aplicativo, um direito deve ser fornecido no arquivo Entitlements.plist do aplicativo. Somente determinadas capacidades podem ser estendidas e elas estão listadas no guia [Trabalhando com capacidades](~/ios/deploy-test/provisioning/capabilities/index.md) e descritas [abaixo](#entitlement-key-reference). Os direitos são transmitidos para o sistema como um par chave-valor e, geralmente, é necessário apenas um por capacidade. As chaves e valores específicos são descritos na seção [de referência de teclas de direito](#entitlement-key-reference) mais tarde neste guia.
O Visual Studio para Mac e o Visual Studio fornecem uma interface clara para adicionar direitos em um aplicativo Xamarin.iOS por meio do editor de Entitlements.plist.
Este guia apresenta o editor de Entitlements.plist e como usá-lo. Ele também fornece uma referência de todos os direitos que podem ser adicionados a um projeto do iOS para cada capacidade.

## <a name="entitlements-and-provisioning"></a>Direitos e provisionamento

O arquivo Entitlements.plist é usado para especificar direitos e é usado para assinar o pacote de aplicativo.

No entanto, certo provisionamento adicional é necessário para garantir que o aplicativo seja assinado por código da forma correta. O perfil de provisionamento usado deve conter uma ID do aplicativo com a capacidade necessária habilitada. Para obter informações de como fazer isso, consulte o guia [Trabalhando com capacidades](~/ios/deploy-test/provisioning/capabilities/index.md).

> [!IMPORTANT]
> O arquivo Entitlements.plist ajuda a preencher as propriedades corretas de um aplicativo usando funcionalidades, mas não pode gerar um perfil de provisionamento porque não está vinculado a uma conta de desenvolvedor da Apple. Você ainda precisará gerar um perfil de provisionamento usando o portal do desenvolvedor para implantar e distribuir o aplicativo.

## <a name="set-entitlements-in-a-xamarinios-project"></a>Definir direitos em um projeto Xamarin.iOS

Além de selecionar e configurar os serviços de aplicativo necessários ao definir a ID do aplicativo, os direitos também devem ser configurados no projeto Xamarin.iOS editando os arquivos **Info.plist** e **Entitlements.plist**.

# <a name="visual-studio-for-mac"></a>[Visual Studio para Mac](#tab/macos)

Para configurar os direitos no Visual Studio para Mac, faça o seguinte:

1. No **bloco Solução,** clique duas vezes no arquivo **Info.plist** para abri-lo.
2. Na seção **Identidade,** preencha um nome para o aplicativo e digite o **Identificador de Pacote** criado quando o ID do aplicativo foi definido:

    ![Insira um Identificador do pacote](entitlements-images/servicexs01-sml.png)

3. Salve as alterações no arquivo **Info.plist**.
4. No **bloco Solução,** clique duas vezes no arquivo **Direitos.plist** para abri-lo para edição:

    ![Editando os direitos](entitlements-images/servicexs02-sml.png)

5. Selecione e configure os direitos necessários (se houver) para o aplicativo Xamarin.iOS de maneira que eles correspondam à configuração definida quando a ID do aplicativo foi criada.
6. Salve as alterações no arquivo **Entitlements.plist**.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Para configurar os direitos no Visual Studio, faça o seguinte:

1. No **Solution Explorer**, clique duas vezes no **Info.plist** para abri-lo para edição.
2. Na seção **Destino do aplicativo iOS**, preencha um nome para o aplicativo e insira o **Identificador do pacote** que foi criado quando a ID do aplicativo foi definida:

    ![Configuração de um Identificador do pacote](entitlements-images/servicevs01-sml.png)

3. Salve as alterações no arquivo **Info.plist**.
4. No **Solution Explorer**, clique duas vezes no arquivo **Entitlements.plist** para abri-lo:

    ![Editando os direitos](entitlements-images/servicevs02-sml.png)

    Você também pode clicar com o botão direito do mouse no arquivo **Direitos.plist** e escolher **Abrir com...** o editor de origem XML que permitirá definir a propriedade de Direito e o valor-chave conforme detalhado na seção [De referência de direitos](#entitlement-key-reference) abaixo.

5. Selecione e configure os direitos necessários (se houver) para o aplicativo Xamarin.iOS de maneira que eles correspondam à configuração definida quando a ID do aplicativo foi criada.
6. Salve as alterações no arquivo **Entitlements.plist**.

-----

## <a name="adding-a-new-entitlementsplist-file"></a>Adicionando um novo arquivo Entitlements.plist

Os direitos são adicionados a um aplicativo por meio do arquivo Entitlements.plist. Esse arquivo é incluído por padrão nos projetos Xamarin.iOS, mas pode estar ausente em projetos mais antigos.

Para adicionar um arquivo Entitlements.plist no Xamarin.iOS, faça o seguinte:

1. Clique com o botão direito do mouse no arquivo de projeto e navegue até **Adicionar > Novo Arquivo...**:

    ![Menu de contexto Adicionar Arquivos](entitlements-images/image1-sml.png)
2. Na caixa de diálogo Novo Arquivo, selecione **iOS > Lista de Propriedades** e nomeie-a como Direitos:

    ![Caixa de diálogo Novo Arquivo](entitlements-images/image2-sml.png)

## <a name="entitlement-key-reference"></a>Referência de chave de direito

Chaves de direitos podem ser adicionadas por meio do painel Código-fonte do editor de Entitlements.plist. As chaves necessárias normalmente serão adicionadas ao usar o editor de Entitlements.plist, mas estão listadas abaixo para referência.

### <a name="wallet"></a>Carteira

- **Descrição**: formalmente conhecido como Passbook, o wallet é um aplicativo que armazena e gerencia passes. Esses passes podem ser cartões de crédito, cartões de armazenamento, passagens aéreas ou tíquetes.

  - **Identificador de tipo de passe**
    - **Chaves**: com.apple.developer.pass-type-identifiers
    - **Corda:**`$(TeamIdentifierPrefix)*`

- **Observações:**
  - Isso habilitará o aplicativo a permitir todos os tipos de passes. Para restringir o aplicativo e permitir somente um subconjunto de tipos de passe de equipe, defina o valor de cadeia de caracteres como: `$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

  Em que pass.$(CFBundleIdentifier) é a ID do Passe criada [acima](~/ios/platform/passkit.md)

### <a name="icloud"></a>iCloud

- **Descrição**: o iCloud fornece aos usuários do iOS uma maneira fácil e simples de armazenar conteúdo e compartilhá-lo entre dispositivos. Os desenvolvedores podem usar o iCloud de quatro maneiras para fornecer um meio de armazenamento para seus usuários: armazenamento de chave-valor, armazenamento de UIDocument, CoreData e usando o CloudKit diretamente para fornecer armazenamento para arquivos e diretórios individuais. Para obter mais informações sobre essas maneiras, consulte o guia Introdução ao iCloud.

  - **Documentos e CloudKit do iCloud**
    - **Chaves**: com.apple.developer.ubiquity-container-identifiers
    - **Corda:**`$(TeamIdentifierPrefix)$(CFBundleIdentifier)`
  - **Armazenamento de KeyValue do iCloud**
    - **Chave**: com.apple.developer.ubiquity-kvstore-identifier
    - **Corda:**`$(TeamIdentifierPrefix)$(CFBundleIdentifier)`

- **Observações:**
  - A cadeia de caracteres `$(TeamIdentifierPrefix)` pode ser localizada fazendo logon no developer.apple.com e visitando a **Central de Membros > Sua Conta > Resumo da Conta de Desenvolvedor** para obter sua ID de Equipe (ou a ID individual para desenvolvedores únicos). Ela será uma cadeia de 10 caracteres (A93A5CM278, por exemplo).
  - A cadeia de caracteres `$(CFBundleIdentifier)` começa com `iCloud` e é definida quando o contêiner do iCloud é criado de acordo com as etapas no guia [Trabalhando com capacidades](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md).
  - Os espaços reservados $`(TeamIdentifierPrefix)` e `$(CFBundleIdentifier)` podem ser usados e serão substituídos pelos valores corretos no momento do build.

> [!IMPORTANT]
> A Apple [fornece ferramentas](https://developer.apple.com/support/allowing-users-to-manage-data/) para ajudar os desenvolvedores a lidar adequadamente com o GDPR (Regulamento Geral sobre a Proteção de Dados) da União Europeia.

### <a name="app-groups"></a>Grupos de Aplicativos

- **Descrição**: um Grupo de Aplicativos permite que diferentes aplicativos (ou um aplicativo e suas extensões) acessem um local de armazenamento de arquivo compartilhado.

  - **Chave**: com.apple.security.application-groups
  - **Cadeia de caracteres**: group.$(CFBundleIdentifier)

### <a name="apple-pay"></a>Apple Pay

- **Descrição**: o Apple Pay permite que os usuários paguem produtos físicos usando o dispositivo iOS.
  - **Chave**: com.apple.developer.in-app-payments
  - **Cadeia de caracteres**: merchant.your.mechantid

### <a name="push-notifications"></a>Notificações por Push

- **Chave**: aps-environment
- **Corda:** `development` ou`production`

### <a name="siri"></a>Siri

- **Descrição**: o SiriKit permite que um aplicativo iOS forneça serviços acessíveis aos aplicativos Siri e Mapas em um dispositivo iOS, usando Extensões de Aplicativo e as novas estruturas Intents e IU de Intents. Para obter mais informações, consulte o guia Introdução ao SiriKit.
  - **Chave**: com.apple.developer.siri

### <a name="personal-vpn"></a>VPN pessoal

- **Chave**: com.apple.developer.networking.vpn.api
- **Cadeia de caracteres**: allow-vpn

### <a name="keychain-sharing"></a>Compartilhamento de conjunto de chaves

- **Descrição**: o compartilhamento de conjunto de chaves permite que os desenvolvedores de aplicativos compartilhem as senhas armazenadas no conjunto de chaves do dispositivo com outros aplicativos desenvolvidos pela mesma equipe. O acesso pode ser restringido passando um identificador de grupo de acesso do conjunto de chaves na cadeia de caracteres.
  - **Chave**: keychain-access-groups
  - **Cadeia de caracteres**: $(AppIdentifierPrefix) $(CFBundleIdentifier)

### <a name="inter-app-audio"></a>Áudio entre aplicativos

- **Descrição**: o Áudio entre aplicativos permite que os desenvolvedores transmitam áudio entre aplicativos.
  - **Chave**: inter-app-audio
  - **Booliano**: sim

### <a name="associated-domains"></a>Domínios Associados

- **Descrição**: os domínios associados que precisam ser tratados como links universais devem ser transmitidos com esse direito. Os links universais podem ser implementados para permitir a vinculação entre o aplicativo e o site. Você deve fornecer uma entrada para cada domínio com suporte do aplicativo e cada entrada deve começar com `applinks:`
  - **Chave**: com.apple.developer.associated-domains
  - **Cadeia de caracteres**: webcredentials:example.com

### <a name="data-protection"></a>Proteção de dados

- **Descrição**: a habilitação da proteção de dados usa hardware de criptografia interno para armazenar os dados confidenciais usados no aplicativo em um formato criptografado. Por padrão, o nível de proteção é definido como proteção completa (os arquivos são acessíveis apenas quando o dispositivo está desbloqueado).
  - **Chave**: com.apple.developer.default-data-protection
  - **Cadeia de caracteres**: NSFileProtectionComplete

### <a name="homekit"></a>HomeKit

- **Descrição**: a estrutura HomeKit fornece uma plataforma para instalar, configurar e gerenciar dispositivos de automação residencial com suporte, tudo usando um dispositivo iOS. Para obter mais informações sobre como usar o HomeKit, consulte o guia Introdução ao HomeKit.
  - **Chave**: com.apple.developer.homekit
  - **Booliano**: sim

### <a name="healthkit"></a>HealthKit

- **Descrição**: o HealthKit é uma estrutura introduzida no iOS 8 que fornece um armazenamento de dados centralizado, coordenado e seguro para obter informações relacionadas à integridade. Para obter mais informações sobre como usar o HealthKit, consulte o guia Introdução ao HealthKit.
  - **Chave**: com.apple.developer.healthkit
  - **Booliano**: sim

### <a name="wireless-accessory-configuration"></a>Configuração de Acessório sem Fio

- **Descrição**: o uso da Configuração de Acessório Sem Fio permite que seu aplicativo configure acessórios MFi Wi-Fi
  - **Chave**: com.apple.external-accessory.wireless-configuration
  - **Booliano**: sim

### <a name="classkit"></a>ClassKit

- **Descrição**: o ClassKit permite que professores vejam o progresso do aluno em atividades atribuídas em seu aplicativo.
  - **Chave**: com.apple.developer.ClassKit-environment
  - **Corda:** `development` ou`production`

## <a name="summary"></a>Resumo

Este guia apresentou direitos e instruções sobre como usá-los no Visual Studio para Mac e no Visual Studio. Também forneceu uma referência de pares chave/valor para cada capacidade.
