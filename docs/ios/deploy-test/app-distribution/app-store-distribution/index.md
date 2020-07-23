---
title: Distribuição da App Store
description: Este documento descreve como distribuir um aplicativo Xamarin.iOS na App Store. Ele discute como criar um certificado de distribuição, como criar uma perfil de provisionamento de distribuição e como configurar o iTunes Connect e enviar o aplicativo.
ms.prod: xamarin
ms.assetid: B07E2C1F-A6DF-43CB-BFB0-0252A5558467
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/23/2017
ms.openlocfilehash: c9882944938b2d5136d36304c4a400208a68663a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937326"
---
# <a name="app-store-distribution"></a>Distribuição da App Store

Depois que um aplicativo Xamarin.iOS foi desenvolvido, a próxima etapa do ciclo de vida de desenvolvimento de software é distribuir o aplicativo aos usuários usando a iTunes App Store. Essa é a maneira mais comum de distribuir aplicativos. Ao publicar um aplicativo na App Store da Apple, ele pode ser disponibilizado a consumidores em todo o mundo.

> [!IMPORTANT]
> A Apple [indicou](https://developer.apple.com/ios/submit/) que, a partir de março de 2019, todos os aplicativos e atualizações enviados à App Store deverão ter sido criados com o SDK do iOS 12.1 ou posterior, incluído no Xcode 10.1 ou posterior.
> Aplicativos também devem dar suporte aos tamanhos de tela do iPhone XS e de 12,9 pol. do iPad Pro.

> [!IMPORTANT]
> Se você estiver procurando recursos em relação ao aviso de `UIWebView` reprovação (ITMS-90809) ao usar o xamarin. Forms, consulte a documentação do [WebView Xamarin. Forms](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809) .

Distribuir um aplicativo, assim como ocorre com o desenvolvimento de um aplicativo, requer provisionar os aplicativos usando o *perfil de provisionamento* apropriado. Perfis de provisionamento são arquivos que contêm informações de assinatura de código, bem como a identidade do aplicativo e o mecanismo de distribuição desejado. Também apresentam informações sobre os dispositivos nos quais o aplicativo pode ser implantado para distribuição fora da App Store.

> [!IMPORTANT]
> Para usar o iTunes Connect e, portanto, publicar um aplicativo na App Store, você **deve** fazer parte de um Programa de Desenvolvedores da Apple individual ou organizacional. Você não poderá seguir as etapas nesta página se você for um membro de um Programa **Empresarial** de Desenvolvedor da Apple.

<a name="provisioning"></a>

## <a name="provisioning-an-app-for-app-store-distribution"></a>Provisionando um aplicativo para distribuição na App Store

Independentemente de como você pretende lançar um aplicativo Xamarin.iOS, você precisará criar um *Perfil de Provisionamento de Distribuição* específica para ele. Esse perfil permite assinar um aplicativo digitalmente para liberação de modo a poder ser instalado em um dispositivo iOS. Semelhante a um perfil de provisionamento de Desenvolvimento, um Perfil de Distribuição conterá o seguinte:

- Uma ID do Aplicativo
- Um Certificado de Distribuição

Você pode selecionar a mesma **ID do Aplicativo** e os mesmos **Dispositivos** usados para o perfil de provisionamento de desenvolvimento; porém, se ainda não tiver um, você precisará criar um Certificado de Distribuição para identificar sua organização ao enviar o aplicativo para a loja de aplicativos. As etapas sobre como criar um Certificado de Distribuição são descritas na seção a seguir.

> [!NOTE]
> Somente administradores e agentes de equipe podem criar certificados de distribuição e perfis de provisionamento.

<a name="creatingcertificate"></a>

## <a name="creating-a-distribution-certificate"></a>Criando um certificado de distribuição

1. Navegue até a seção *Certificates, Identifiers & Profiles (Certificados, Identificadores e Perfis)* do Apple Developer Member Center (Central de Associados do Desenvolvedor da Apple).
2. Em *Certificates (Certificados)*, selecione **Production (Produção)**.
3. Clique no **+** botão para criar um novo certificado.
4. No cabeçalho *produção* , selecione **loja de aplicativos e ad hoc**:

    [![Selecionar loja de aplicativos e ad hoc](images/createcertmanually01.png)](images/createcertmanually01.png#lightbox)
5. Clique em **Continue (Continuar)** e siga as instruções para criar uma Solicitação de Assinatura de Certificado por meio do Acesso do Sistema de Chaves: 

    [![Criar uma solicitação de assinatura de certificado por meio do acesso ao conjunto de chaves](images/createcertmanually02.png)](images/createcertmanually02.png#lightbox)
6. Depois de criar o CSR conforme as instruções, clique em **Continuar** e carregue o CSR na Central de Associados:    

    [![Carregar o CSR no centro de membros](images/createcertmanually03.png)](images/createcertmanually03.png#lightbox)

7. Clique em **Generate (Gerar)** para criar o certificado.
8. Por fim, **Baixe** o certificado concluído e clique duas vezes no arquivo para instalá-lo.
9. Neste ponto, o certificado deve ser instalado no computador, mas talvez seja necessário [atualizar seus perfis](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)para garantir que eles fiquem visíveis no Xcode.

Também é possível solicitar um Certificado usando a caixa de diálogo Preferências no Xcode. Para fazer isso, siga as etapas abaixo:

1. Selecione sua equipe e clique em **gerenciar certificados...**: [ ![ Selecione a equipe e exiba os detalhes](images/selectteam.png)](images/selectteam.png#lightbox)

2. Em seguida, clique no botão **criar** ao lado de **certificado de distribuição do IOS**: [ ![ criar um certificado de distribuição do IOS](images/selectcert.png)](images/selectcert.png#lightbox)

3. Dependendo de seus privilégios de equipe, a identidade de assinatura será gerada, conforme mostrado abaixo, ou talvez você precise esperar até que um administrador ou agente de equipe o aprove: [ ![ a identidade de assinatura será gerada e uma caixa de diálogo exibida](images/generated.png)](images/generated.png#lightbox)

<a name="creatingprofile"></a>

## <a name="creating-a-distribution-profile"></a>Como criar um perfil de distribuição

<a name="creatingappid"></a>

### <a name="creating-an-app-id"></a>Criando uma ID do aplicativo

Como com qualquer outro Perfil de Provisionamento que você criar, uma ID do Aplicativo será necessária para identificar o Aplicativo que você está distribuindo ao dispositivo do usuário. Se ainda não tiver criado uma, siga as etapas abaixo para criá-la:

1. No [Apple Developer Center (Centro de Desenvolvedores da Apple)](https://developer.apple.com/account/overview.action), navegue até a seção *Certificate, Identifiers and Profiles (Certificados, Identificadores e Perfis)*. Selecione **App IDs (IDs do Aplicativo)** em **Identifiers (Identificadores)**.
2. Clique no **+** botão e forneça um **nome** que o identificará no Portal.
3. O prefixo do aplicativo já deve estar definido como sua ID de Equipe e não pode ser alterado. Selecione uma ID do Aplicativo Explícita ou Curinga e insira uma ID de Pacote no formato DNS inverso, como:
    - **Explícito**: com.[DomainName].[AppName]
    - **Curinga**: com.[DomainName].*
4. Selecione quaisquer [Serviços de Aplicativos](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning-for-application-services) que seu aplicativo exija.
5. Clique no botão **Continue (Continuar)** e siga as instruções na tela para criar a nova ID do Aplicativo.

### <a name="creating-a-provisioning-profile"></a>Criando um perfil de provisionamento

Depois de obter os componentes obrigatórios necessários para criar um Perfil de Distribuição, siga as etapas abaixo para criá-lo:

1. Retorne ao portal de provisionamento da Apple e selecione **Provisioning**  >  **distribuição**de provisionamento:

    [![RSelect provisionamento > distribuição](images/distribute01.png)](images/distribute01.png#lightbox)

2. Clique no **+** botão e selecione o tipo de perfil de distribuição que você deseja criar como **loja de aplicativos**:

    [![Criar um perfil de distribuição da loja de aplicativos](images/distribute02.png)](images/distribute02.png#lightbox)

3. Clique no botão **Continue (Continuar)** e selecione a ID do Aplicativo na lista suspensa para qual você deseja criar um Perfil de Distribuição: 

    [![Selecione a ID do aplicativo na lista suspensa](images/distribute03.png)](images/distribute03.png#lightbox)

4. Clique no botão **Continuar** e selecione o certificado necessário para assinar o aplicativo:

    [![Selecione o certificado necessário para assinar o aplicativo](images/distribute04.png)](images/distribute04.png#lightbox)

5. Clique no botão **Continuar** e selecione os dispositivos iOS nos quais o aplicativo Xamarin.iOS poderá ser executado:

    [![Selecione os dispositivos iOS nos quais o aplicativo terá permissão para executar](images/distribute05.png)](images/distribute05.png#lightbox)

6. Clique no botão **Continuar** e digite um **Nome** para o novo Perfil de Distribuição:

    [![Insira um nome para o novo perfil de distribuição](images/distribute06.png)](images/distribute06.png#lightbox)

7. Clique no botão **Generate (Gerar)** para criar o novo perfil e finalizar o processo.

# <a name="visual-studio-for-mac"></a>[Visual Studio para Mac](#tab/macos)

 Talvez você precise fechar o Visual Studio para Mac e fazer com que o Xcode atualize sua lista de Identidades de Assinatura e Perfis de Provisionamento disponíveis (seguindo as instruções na seção [Solicitando Identidades de Assinatura](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)) antes de um novo perfil de distribuição ser disponibilizado no Visual Studio para Mac.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

 Talvez seja necessário fechar o Visual Studio e fazer com que o Xcode (no Mac do host de compilação) Atualize sua lista de identidades de assinatura e perfis de provisionamento disponíveis (seguindo as instruções na seção [solicitando identidades de assinatura](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) ) antes que um novo perfil de distribuição esteja disponível no Visual Studio.

-----

<a name="selectprofile"></a>

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Selecionando um perfil de distribuição em um projeto Xamarin.iOS

Quando você estiver pronto para fazer um build final de um aplicativo Xamarin.iOS para venda na iTunes App Store, selecione o Perfil de Distribuição criado acima.

# <a name="visual-studio-for-mac"></a>[Visual Studio para Mac](#tab/macos)

 No Visual Studio para Mac, faça o seguinte:

1. Clique duas vezes no nome do projeto no **Gerenciador de Soluções** para abri-lo para edição.
2. Selecione **Assinatura do Pacote do iOS** e **Versão | iPhone** na lista suspensa **Configuração**: 

    ![Selecione Versão | iPhone na lista suspensa Configuração](images/releasexs01.png)
3. Na maioria dos casos, a **Identidade de Assinatura** e o **Perfil de Provisionamento** podem ser deixados em seus valores padrão de **Automático** e o Visual Studio para Mac escolherá o perfil correto com base no Identificador de Pacote na Info.plist:

    ![A Identidade de assinatura e o Perfil de provisionamento definidos como os valores padrão de Automático](images/releasexs02.png)
4. Se necessário, selecione a Identidade de Assinatura e o Perfil de Distribuição (aquele criado acima) nas listas suspensas:

    ![Selecione a Identidade de assinatura e os Perfis de distribuição](images/releasexs03.png)
5. Clique no botão **OK** para salvar as alterações.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

 No Visual Studio, faça o seguinte:

1. Clique com o botão direito do mouse no nome do projeto no **Gerenciador de Soluções** e selecione **Propriedades** para abri-lo para edição.
2. Selecione **Assinatura do Pacote do iOS** e **Versão | iPhone** na lista suspensa **Configuração**: 

    ![Selecione Versão | iPhone na lista suspensa Configuração](images/releasevs01.png)
3. Na maioria dos casos, a **Identidade de Assinatura** e o **Perfil de Provisionamento** podem ser deixados em seus valores padrão de **Automático** e o Visual Studio escolherá o perfil correto com base no Identificador de Pacote na Info.plist

    ![A Identidade de assinatura e o Perfil de provisionamento definidos como os valores padrão de Automático](images/releasevs02.png)
4. Se necessário, selecione a Identidade de Assinatura e o Perfil de Distribuição (aquele criado acima) nas listas suspensas:

    ![Selecione a Identidade de assinatura e o Perfil de distribuição](images/releasevs03.png)
5. Salve suas alterações nas Propriedades do projeto.

-----

<a name="itunesconnect"></a>

## <a name="configuring-your-application-in-itunes-connect"></a>Configurando seu aplicativo no iTunes Connect

Depois que o aplicativo tiver sido provisionado com êxito, a próxima etapa será configurar os aplicativos no [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa), um pacote de ferramentas baseadas na Web para, entre outros fins, gerenciar aplicativos do iOS na App Store.

Seu aplicativo Xamarin.iOS precisará ser definido e configurado corretamente no iTunes Connect antes de ser enviado à Apple para análise e, por fim, ser liberado para venda ou como um aplicativo gratuito na App Store.

Para obter mais detalhes, consulte a documentação [Configurando um aplicativo no iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md).

<a name="submitting"></a>

## <a name="submitting-an-app-to-itunes-connect"></a>Como enviar um aplicativo para o iTunes Connect

Depois que o aplicativo for assinado usando o perfil de provisionamento de distribuição e criado no iTunes Connect, o binário do aplicativo será carregado para a Apple para análise. Após uma análise bem-sucedida pela Apple, ele será disponibilizado na App Store.

Para saber mais sobre a publicação de aplicativos na App Store, consulte [Como publicar na App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

<a name="windows"></a>

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Copiar automaticamente os pacotes do .app para o Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="summary"></a>Resumo

Este artigo abordou os principais componentes na preparação de um aplicativo Xamarin.iOS para distribuição na App Store.

## <a name="related-links"></a>Links Relacionados

- [Configurando um aplicativo no iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publicando na App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Distribuição interna](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Distribuição Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [O arquivo iTunesMetadata. plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Suporte do IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
