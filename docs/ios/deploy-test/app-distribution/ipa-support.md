---
title: Suporte de IPA no Xamarin.iOS
description: Este artigo aborda como criar um arquivo de IPA que pode ser usado para implantar um aplicativo usando a distribuição Ad Hoc, seja para teste ou para distribuição Interna dos aplicativos internos.
ms.prod: xamarin
ms.assetid: D253C2DB-852E-6FC6-C9FD-574730B8DB19
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: b9254afdcb6286edcffc67a1a69af8b049f08b6b
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573216"
---
# <a name="ipa-support-in-xamarinios"></a>Suporte de IPA no Xamarin.iOS

_Este artigo aborda como criar um arquivo de IPA que pode ser usado para implantar um aplicativo usando a distribuição ad hoc, seja para teste ou para distribuição interna de aplicativos internos._

Além de liberar um aplicativo para venda por meio da iTunes App Store, ele pode ser implantado para os seguintes usos:

- **Teste Ad Hoc** – um aplicativo iOS pode ser implantado para até 100 usuários (identificados pelos UUIDs do dispositivo iOS específico) para fins de teste alfa e beta. Consulte nossa documentação de [Provisionamento de um dispositivo para o desenvolvimento de iOS](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning) para obter informações detalhadas sobre como adicionar dispositivos iOS de teste à sua conta de desenvolvedor da Apple e o guia [Ad-Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) para saber mais sobre como distribuir dessa forma.
- **Implantação Empresarial/Interna** – um aplicativo iOS pode ser implantado internamente, dentro de uma empresa, o que requer uma associação no programa de Desenvolvedores corporativos da Apple. Mais informações sobre a Distribuição interna estão descritas em detalhes no guia [Distribuição interna](~/ios/deploy-test/app-distribution/in-house-distribution.md).

Em ambos os casos, um pacote de IPA (um tipo especial de arquivo zip) deve ser criado e assinado digitalmente com o perfil de Provisionamento de Distribuição correto. Este artigo aborda as etapas necessárias para compilar o pacote IPA e instalá-lo em um dispositivo iOS usando o iTunes em um Mac ou PC com Windows.

<a name="iTunesMetadata"></a>

## <a name="the-itunesmetadataplist-file"></a>O arquivo iTunesMetadata.plist

Quando um aplicativo iOS é criado no iTunes Connect (para venda ou lançamento gratuito pela iTunes App Store), o desenvolvedor pode especificar informações, como gênero do aplicativo, subgênero, direitos autorais, dispositivos iOS com suporte e os recursos do dispositivo necessários.

Aplicativos iOS entregues por meio de distribuição Interna ou Ad Hoc precisam ter alguma maneira de dar suporte a essas informações para que possam ser visíveis no iTunes e no dispositivo do usuário. Por padrão, um pequeno arquivo iTunesMetadata.plist é criado sempre que você compila seu projeto e ele é armazenado no diretório do projeto.

Um **iTunesMetadata.plist** personalizado também pode ser criado para fornecer informações adicionais para uma distribuição. Para saber mais sobre o conteúdo desse arquivo e como criá-lo, consulte nossa documentação [Conteúdo do iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_contents) e [Como criar um arquivo iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_creating).

<a name="iTunesArtwork"></a>

## <a name="itunes-artwork"></a>Arte do iTunes

Ao entregar seu aplicativo por um meio que não seja a App Store, você também precisa incluir uma imagem 512x512 e outra 1024x1024 que serão usadas para representar seu aplicativo no iTunes.

Para especificar a Arte do iTunes, faça o seguinte:

1. Clique duas vezes no arquivo **Info.plist** no **Gerenciador de Soluções** para abri-lo para edição.
2. Role até a seção **Arte do iTunes** do editor.
3. Para qualquer imagem ausente, clique na miniatura no editor, selecione o arquivo de imagem para a arte do iTunes desejada na caixa de diálogo **Abrir Arquivo** e clique no botão **OK** ou **Abrir**.
4. Repita essa etapa até especificar todas as imagens necessárias para seu aplicativo.

Consulte a documentação [Arte do iTunes](~/ios/app-fundamentals/images-icons/app-icons.md) para obter mais detalhes.

<a name="createipa"></a>

## <a name="creating-an-ipa"></a>Como criar um IPA

# <a name="visual-studio-for-mac"></a>[Visual Studio para Mac](#tab/macos)

A ação de criar um IPA agora é interna no novo fluxo de trabalho de publicação. Para fazer isso, siga as instruções abaixo para arquivar seu aplicativo, assiná-lo e salvar seu IPA.

Antes de começar a criar um IPA para uma solução de plataforma cruzada, selecione o projeto iOS como seu projeto de inicialização:

![](ipa-support-images/setasstartup.png "Selected the iOS project as the startup project")

### <a name="build-your-archive"></a>Compilar seu arquivo morto

Para compilar um IPA, é preciso criar um _arquivo morto_ de um build de versão de nosso aplicativo. Este Arquivo Morto contém nosso aplicativo e informações de identificação sobre ele.

1. Selecione a configuração **Versão | Dispositivo** no Visual Studio para Mac: !

    ![](ipa-support-images/buildxs01new.png "Select the Release | Device configuration")

1. No menu **Compilar** , selecione **arquivo morto para publicação**:

    ![](ipa-support-images/buildxs02new.png "Select Archive for Publishing")

1. Quando o arquivo tiver sido criado, a exibição **Arquivos Mortos** será exibida:

    ![](ipa-support-images/buildxs03new.png "The Archives view will be displayed")

### <a name="sign-and-distribute-your-app"></a>Assinar e distribuir seu aplicativo

Sempre que você compilar seu aplicativo para o Arquivo Morto, ele abrirá automaticamente a **Exibição de Arquivos Mortos**, exibindo todos os projetos arquivados, agrupados por solução. Por padrão essa exibição mostra apenas a solução atual aberta. Para ver todas as soluções que têm arquivos, clique na opção **Mostrar todos os arquivos mortos**.

É recomendável que os arquivos mortos implantados para clientes (implantações Ad-Hoc ou Internas) sejam mantidos, de modo que qualquer informação de depuração gerada possa ser simbolizada posteriormente.

Observe que, para os builds não da App Store, o arquivo **iTunesMetadata.plist** e o conjunto de arte do iTunes serão incluídos automaticamente no IPA se forem encontrados no arquivo morto.

Para assinar seu aplicativo e prepará-lo para distribuição:

1. Selecione o botão **Assinar e Distribuir...**, como ilustrado abaixo:

    ![](ipa-support-images/buildxs04new.png "Select Sign and Distribute...")

1. Isso abrirá o assistente de publicação. Selecione o canal de distribuição **Ad Hoc** ou **Empresarial** (Interno) para criar um pacote:

    ![](ipa-support-images/distribute01.png "Select the Ad-Hoc or Enterprise In-House distribution")

1. Na tela Perfil de Provisionamento, selecione sua identidade de assinatura e o perfil de provisionamento correspondente ou repita a assinatura com outra identidade:

    ![](ipa-support-images/distribute02.png "Select the signing identity and corresponding provisioning profile")

1. Verifique os detalhes de seu pacote e, em seguida, clique em **Publicar**:

    ![](ipa-support-images/distribute03.png "Verify the package details")

1. Por fim, salve o IPA em seu computador:

    ![](ipa-support-images/distribute04.png "Save the IPA to the computer")

### <a name="building-via-the-command-line-on-mac"></a>Criação via linha de comando (em Mac)

Em determinados casos, como em um ambiente de CI, pode ser necessário compilar o IPA via linha de comando. Siga as etapas abaixo para fazer isso:

1. Marque **Opções de Projeto > Opções de IPA do iOS > Incluir imagens do iTunesArtwork** e marque **Compilar pacote ad hoc/empresarial (IPA)**:

    ![](ipa-support-images/imagexs04.png "Include iTunesArtwork images and Build ad-hoc/enterprise package IPA is checked")

    Se preferir, em vez disso, você pode editar o arquivo **.csproj** em um editor de texto e adicionar manualmente as duas propriedades correspondentes ao `PropertyGroup` para a configuração que será usada para compilar o aplicativo:

    ```xml
    <BuildIpa>true</BuildIpa>
    <IpaIncludeArtwork>false</IpaIncludeArtwork>
    ```

1. Se estiver incluindo um arquivo **iTunesMetadata.plist** opcional, clique no botão **...**, selecione-o na lista e clique no botão **OK**:

     ![](ipa-support-images/imagexs03.png "Select iTunesMetadata.plist from the list")

1. Chame **msbuild** diretamente e passe esta propriedade na linha de comando:

    ```bash
    /Library/Frameworks/Mono.framework/Commands/msbuild YourSolution.sln /p:Configuration=Ad-Hoc /p:Platform=iPhone /p:BuildIpa=true
    ```

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

 Depois que o perfil de provisionamento for criado e selecionado, o arquivo **iTunesMetadata.plist** opcional for criado e a Arte do iTunes for definida no Visual Studio, você poderá compilar um IPA para distribuição. Em seguida, você precisará configurar seu projeto. Faça o seguinte:

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse no nome do projeto Xamarin.iOS e selecione **Propriedades** para abri-lo para edição:

    ![](ipa-support-images/imagevs01.png "Select Properties")

2. Selecione **Opções do IPA do iOS** e selecione **Ad Hoc** na lista suspensa **Configuração**:

    ![](ipa-support-images/imagevs02.png "Select Ad-Hoc from the Configuration dropdown list")

    > [!NOTE]
    > Uma configuração ad hoc pode não estar disponível para projetos do Xamarin.iOS mais recentes. Se não estiver disponível, selecione a configuração de **Versão**.

3. Se estiver incluindo um arquivo **iTunesMetadata.plist** opcional, clique no botão **...**, selecione-o na lista e clique no botão **Abrir**:

    ![](ipa-support-images/imagevs03.png "Select iTunesMetadata.plist from the list")

4. Opcionalmente, você pode especificar um **Nome do Pacote** para o IPA. Se não for especificado, ele terá o mesmo nome que o projeto Xamarin.iOS.
5. Salve suas alterações às Propriedades do Projeto.
6. Selecione **Ad Hoc** na lista suspensa **Configuração de Build**, se disponível. Caso contrário, selecione **Versão**:

    ![](ipa-support-images/imagevs05.png "Select Ad Hoc from the Build Configuration dropdown")

7. Compile o projeto para criar o pacote IPA.
8. O IPA será compilado na pasta **Bin > iOS Device > Ad Hoc (ou Versão)**:

    ![](ipa-support-images/imagevs06.png "The IPA in the file explorer")

-----

<a name="Customizing-the-IPA-Location"></a>

## <a name="customizing-the-ipa-location"></a>Personalizando o local de IPA

No **MSBuild**, uma nova propriedade `IpaPackageDir` foi adicionada para facilitar a personalização do local de saída do arquivo **.ipa**. Se `IpaPackageDir` estiver definido como um local personalizado, o arquivo **.ipa** será colocado nesse local, em vez de no subdiretório padrão com carimbo de data/hora. Isso pode ser útil ao criar builds automatizados que dependem de um caminho de diretório específico para funcionarem corretamente, como aqueles usados para builds de CI (Integração Contínua).

Há várias maneiras possíveis de se usar a nova propriedade:

Por exemplo, para fazer a saída do arquivo **.ipa** no antigo diretório padrão (como no Xamarin.iOS 9.6 e inferior), você pode definir a propriedade `IpaPackageDir` como `$(OutputPath)` usando uma das abordagens a seguir. As duas abordagens são compatíveis com todos os builds do Xamarin.iOS de API Unificada, incluindo builds do IDE, bem como builds de linha de comando que usam **msbuild**, **xbuild** ou **mdtool**:

- A primeira opção é definir a propriedade `IpaPackageDir` dentro de um elemento `<PropertyGroup>` em um arquivo **MSBuild**. Por exemplo, você poderia adicionar o seguinte `<PropertyGroup>` à parte inferior do arquivo **.csproj** do projeto de aplicativo do iOS (logo antes da marca de fechamento `</Project>`):

    ```xml
    <PropertyGroup>
        <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- Uma abordagem melhor seria adicionar um elemento `<IpaPackageDir>` à parte inferior do `<PropertyGroup>` existente que corresponde à configuração usada para compilar o arquivo **.ipa**. Ela é melhor porque preparará o projeto para compatibilidade futura com uma configuração planejada na página de propriedades do projeto Opções do IPA do iOS. Se você estiver usando a configuração `Release|iPhone` para compilar o arquivo **.ipa** no momento, o grupo de propriedades completo atualizado poderá ficar parecido com o seguinte:

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' ">
        <Optimize>true</Optimize>
        <OutputPath>bin\iPhone\Release</OutputPath>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <ConsolePause>false</ConsolePause>
        <CodesignKey>iPhone Developer</CodesignKey>
        <MtouchUseSGen>true</MtouchUseSGen>
        <MtouchUseRefCounting>true</MtouchUseRefCounting>
        <MtouchFloat32>true</MtouchFloat32>
        <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
        <MtouchLink>SdkOnly</MtouchLink>
        <MtouchArch>;ARMv7, ARM64</MtouchArch>
        <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
        <MtouchTlsProvider>Default</MtouchTlsProvider>
        <PlatformTarget>x86&</PlatformTarget>
        <BuildIpa>true</BuildIpa>
        <IpaPackageDir>$(OutputPath</IpaPackageDir>
    </PropertyGroup>
    ```

Uma técnica alternativa para builds de linha de comando **msbuild** ou **xbuild** é a adição de um argumento de linha de comando `/p:` para definir a propriedade `IpaPackageDir`. Neste caso, observe que **msbuild** não expande as expressões `$()` passadas na linha de comando, portanto, não é possível usar a sintaxe `$(OutputPath)`. Em vez disso, você deve fornecer um nome de caminho completo. O comando **xbuild** do Mono expande as expressões `$()`, mas ainda é preferível usar um nome de caminho completo, pois o **xbuild** foi preterido em favor da [versão multiplataforma do **msbuild**](https://www.mono-project.com/docs/about-mono/releases/5.0.0/#msbuild).

Um exemplo completo que usa essa abordagem pode ser semelhante ao seguinte no Windows:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Ou semelhante ao seguinte no Mac:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

<a name="installipa"></a>

## <a name="installing-an-ipa-using-itunes"></a>Como instalar um IPA usando iTunes

O pacote IPA resultante pode ser entregue a seus usuários de teste para instalação em seus dispositivos iOS ou enviado para implantação Empresarial. Não importa o método escolhido, o usuário final instalará o pacote nos aplicativos iTunes no Mac ou computador Windows clicando duas vezes no arquivo IPA (ou arrastando-o para a janela do iTunes aberta).

O novo aplicativo iOS será mostrado na seção **Meus Aplicativos**, na qual você pode clicar com o botão direito do mouse e obter informações sobre o aplicativo:

# <a name="visual-studio-for-mac"></a>[Visual Studio para Mac](#tab/macos)

 ![](ipa-support-images/installxs01.png "The new iOS application in the My Apps section")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

 ![](ipa-support-images/installvs01.png "The new iOS application in the My Apps section")

-----

O usuário agora pode sincronizar o iTunes com seu dispositivo para instalar o novo aplicativo iOS.

<a name="Summary"></a>

## <a name="summary"></a>Resumo

Este artigo abordou a configuração necessária para preparar um aplicativo Xamarin.iOS para um build fora da App Store. Ele mostrou como criar um Pacote IPA e como instalar o aplicativo iOS resultante no dispositivo iOS do usuário final para teste ou Distribuição Interna.

## <a name="related-links"></a>Links relacionados

- [Distribuição da loja de aplicativos](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Configurando um aplicativo no iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publicando na App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Distribuição interna](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Distribuição Ad Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [O arquivo iTunesMetadata. plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Solução de problemas](~/ios/deploy-test/troubleshooting.md)
- [Arte do iTunes](~/ios/app-fundamentals/images-icons/app-icons.md#itunes)
- [Develop and distribute enterprise apps (Apple)](https://help.apple.com/xcode/mac/current/#/devba5e7054d) [Desenvolver e distribuir aplicativos empresariais (Apple)]
- [Distributing Enterprise Apps (WWDC Video)](https://developer.apple.com/videos/play/wwdc2014/705/) [Distribuindo aplicativos empresariais (vídeo do WWDC)]
