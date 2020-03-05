---
title: Usando a cidade da equipe com o Xamarin
description: Este guia abordará as etapas envolvidas no uso do TeamCity para compilar aplicativos móveis e, em seguida, enviá-los para Xamarin Test Cloud.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 94bc775366d832e0994b8d3c74a45123ff56c13b
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2020
ms.locfileid: "78292527"
---
# <a name="using-team-city-with-xamarin"></a>Usando a cidade da equipe com o Xamarin

_Este guia abordará as etapas envolvidas no uso do TeamCity para compilar aplicativos móveis e, em seguida, enviá-los para Xamarin Test Cloud._

Conforme discutido no guia [introdução à integração contínua](~/tools/ci/intro-to-ci.md) , a CI (integração contínua) é uma prática útil ao desenvolver aplicativos móveis de qualidade. Há muitas opções viáveis para o software de servidor de integração contínua; Este guia se concentrará no [TeamCity](https://www.jetbrains.com/teamcity/) da JetBrains.

Há várias permutações diferentes de uma instalação do TeamCity. Veja a seguir uma lista de alguns destes:

- **Serviço do Windows** – neste cenário, o TeamCity é iniciado quando o Windows é inicializado como um serviço do Windows. Ele deve ser emparelhado com um host de Build do Mac para compilar qualquer aplicativo iOS.

- **Iniciar o daemon no os X** – conceitualmente, isso é muito semelhante a executar como um serviço do Windows descrito na etapa anterior. Por padrão, as compilações serão executadas na conta raiz.

- **Conta de usuário no os X** – é possível executar TeamCity em uma conta de usuário que inicia cada vez que o usuário faz logon.

Dos cenários anteriores, a execução de TeamCity em uma conta de usuário no OS X é a mais simples e fácil de configurar.

Há várias etapas envolvidas na configuração de TeamCity:

- **Instalando o TeamCity** – a instalação do TeamCity não é abordada neste guia. Este guia pressupõe que o TeamCity está instalado e em execução em uma conta de usuário. As instruções sobre como [instalar o TeamCity](https://confluence.jetbrains.com/display/TCD8/Installation) podem ser encontradas na [documentação do TeamCity 8](https://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) por JetBrains.

- **Preparando o servidor de Build** – esta etapa envolve a instalação do software, das ferramentas e dos certificados necessários para criar aplicativos móveis e prepará-los para distribuição.

- **Criar um script de compilação** – essa etapa não é estritamente necessária, mas um script de compilação é uma ajuda útil para a criação de aplicativos autônomos. O uso de um script de compilação ajudará na solução de problemas de compilação que podem surgir e fornece uma maneira consistente e reproduzível de criar os binários para distribuição, mesmo se a integração contínua não for praticado.

- **Criando um projeto TeamCity** – depois que as três etapas anteriores forem concluídas, devemos criar um projeto TeamCity que conterá todos os metadados necessários para recuperar o código-fonte, compilar os projetos e enviar os testes para Xamarin Test Cloud.

## <a name="requirements"></a>Requisitos

A experiência com o [teste de App Center](https://docs.microsoft.com/appcenter/test-cloud/) é necessária.

É necessário ter familiaridade com o TeamCity 8,1. A instalação do TeamCity está além do escopo deste documento. Supõe-se que o TeamCity está instalado no OS X Mavericks e está sendo executado em uma conta de usuário normal e não na conta raiz.

O servidor de compilação deve ser um computador autônomo, executando o OS X, que é dedicado à integração contínua. Idealmente, o servidor de compilação não será responsável por nenhuma outra função, como um servidor de banco de dados, servidor Web ou estação de trabalho de desenvolvedor.

> [!IMPORTANT]
> Este guia não abrange uma instalação "sem periféricos" do Xamarin.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>Preparando o servidor de compilação

Uma etapa crucial na configuração de um servidor de compilação é instalar todas as ferramentas, o software e os certificados necessários para criar os aplicativos móveis. É importante que o servidor de compilação seja capaz de compilar a solução móvel e executar qualquer teste. Para minimizar os problemas de configuração, o software e as ferramentas devem ser instalados na mesma conta de usuário que está hospedando o TeamCity. A seguir está uma lista do que é necessário:

1. **Visual Studio para Mac** – inclui Xamarin. Ios e Xamarin. Android.
2. **Faça logon na loja de componentes do xamarin** – essa é uma etapa opcional e só será necessária se seu aplicativo usar componentes da loja de componentes do Xamarin. Fazer logon proativamente na loja de componentes neste ponto impedirá quaisquer problemas quando uma compilação TeamCity tentar compilar o aplicativo.
3. **Xcode** – o Xcode é necessário para compilar e assinar aplicativos Ios.
4. **Ferramentas de linha de comando do Xcode** – isso é descrito na etapa 1 da seção de instalação do guia [atualizando Ruby com rbenv](https://github.com/calabash/calabash-ios/wiki) .
5. **Identidade de assinatura & perfis de provisionamento** – importe os certificados e o perfil de provisionamento por meio do Xcode. Consulte o guia da Apple sobre como [Exportar identidades de assinatura e perfis de provisionamento](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) para obter mais detalhes.
6. **Repositórios de chaves do Android** – Copie os repositórios de chaves Android necessários para um diretório ao qual o usuário TeamCity tenha acesso, ou seja, `~/Documents/keystores/MyAndroidApp1`.
7. **Calabash** – essa é uma etapa opcional se seu aplicativo tiver testes escritos usando o Calabash. Consulte o guia [instalando o Calabash no os X Mavericks](https://github.com/calabash/calabash-ios/wiki) e o guia [atualizando Ruby com o rbenv](https://github.com/calabash/calabash-ios/wiki) para obter mais informações.

O diagrama a seguir ilustra todos esses componentes:

![](teamcity-images/image1.png "This diagram illustrates all of these components")

Depois que todo o software tiver sido instalado, faça logon na conta de usuário e confirme se todo o software foi instalado corretamente e está funcionando. Isso deve envolver a compilação da solução e o envio do aplicativo para Test Cloud. Isso pode ser bastante simplificado com a execução do script de compilação, conforme descrito na próxima seção.

## <a name="create-a-build-script"></a>Criar um script de compilação

Embora seja totalmente possível para TeamCity lidar com todos os aspectos de compilação e envio dos aplicativos móveis para Test Cloud por si só, é altamente recomendável criar um script de compilação. Um script de compilação oferece as seguintes vantagens:

1. **Documentação** – um script de Build serve como uma forma de documentação sobre como o software é criado. Isso remove parte da "mágica" associada à implantação do aplicativo e permite que os desenvolvedores se concentrem na funcionalidade.
1. Capacidade de **repetição** – um script de Build garante que cada vez que o aplicativo é compilado e implantado, ele ocorre exatamente da mesma forma, independentemente de quem ou o que faz o trabalho. Essa consistência repetível remove quaisquer problemas ou erros que possam surgir devido a uma compilação ou erro humano executado incorretamente.
1. Controle de **versão** – um script de compilação pode ser incluído no sistema de controles do código-fonte. Isso significa que as alterações no script de compilação podem ser rastreadas, monitoradas e corrigidas se erros ou imprecisões forem encontrados.
1. **Preparar o ambiente** – um script de compilação pode incluir a lógica para instalar qualquer dependência de terceiros necessária. Isso garantirá que os aplicativos sejam criados com os componentes adequados.

O script de Build pode ser tão simples quanto um arquivo do PowerShell (no Windows) ou um script bash (no OS X). Ao criar o script de compilação, há várias opções para linguagens de script:

- [**Rake**](https://github.com/jimweirich/rake) – é uma DSL (linguagem específica de domínio) para compilar projetos, com base em Ruby. O rake tem a vantagem de popularidade e um rico ecossistema de bibliotecas.

- [**psake**](https://github.com/psake/psake) – esta é uma biblioteca do Windows PowerShell para a criação de software

- [**Falso**](https://fsharp.github.io/FAKE/) – é uma DSL baseada no, F# que possibilita utilizar bibliotecas existentes do .net, se necessário.

A linguagem de script usada depende de suas preferências e requisitos.

> [!NOTE]
> É possível usar um sistema de compilação baseado em XML, como o MSBuild ou o NAnt, mas eles não têm a expressividade e a capacidade de manutenção de uma DSL dedicada à criação de software.

### <a name="parameterizing-the-build-script"></a>Parametrizando o script de compilação

O processo de criação e teste de software requer informações que devem ser mantidas em segredo. Em particular, a criação de um APK pode exigir uma senha para o repositório de chaves e/ou o alias de chave no keystore. Da mesma forma, Test Cloud requer uma chave de API que seja exclusiva para um desenvolvedor. Esses tipos de valores não devem ser embutidos em código no script de compilação. Em vez disso, eles devem ser passados como variáveis para o script de compilação.

Menos confidencial são valores como a ID do dispositivo iOS ou a ID do dispositivo Android que identificam quais dispositivos Test Cloud devem ser usados para execuções de teste. Esses não são valores que precisam ser protegidos, mas podem mudar de Build para Build.

Armazenar esses tipos de variáveis fora do script de compilação também facilita o compartilhamento do script de Build em uma organização, com os desenvolvedores, por exemplo. Os desenvolvedores podem usar exatamente o mesmo script que o servidor de compilação, mas podem usar seus próprios repositórios de chaves e chave de API.

Há duas opções possíveis para armazenar esses valores confidenciais:

- **Um arquivo de configuração** – para proteger a chave de API de Test Cloud esse valor não deve ser verificado no controle de versão. O arquivo pode ser criado para cada computador. A forma como os valores são lidos neste arquivo depende da linguagem de script usada.

- **Variáveis de ambiente** – elas podem ser facilmente definidas em uma base por máquina e partilhadas independentes da linguagem de script subjacente.

Há vantagens e desvantagens em cada uma dessas opções. O TeamCity funciona bem com variáveis de ambiente, portanto, este guia recomendará essa técnica ao criar scripts de compilação.

### <a name="build-steps"></a>Etapas da compilação

O script de compilação deve ser capaz de executar as seguintes etapas:

- **Compilar o aplicativo** – isso inclui a assinatura do aplicativo com o perfil de provisionamento correto.

- **Enviar o aplicativo para Xamarin Test Cloud** – isso inclui a assinatura e o zip alinhando o apk com o keystore apropriado.

Essas duas etapas serão explicadas em mais detalhes abaixo.

#### <a name="compiling-a-xamarinios-application"></a>Compilando um aplicativo Xamarin. iOS

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Compilando um aplicativo Xamarin. Android

Para compilar um aplicativo Android, use **xbuild** (ou **MSBuild** no Windows):

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Observe que para compilar o aplicativo Xamarin Android **xbuild** usa o projeto, e isso para compilar o aplicativo IOS **xbuild** requer a solução.

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Enviando o Xamarin. UITests para Test Cloud

UITests são enviados usando o aplicativo `test-cloud.exe`, conforme mostrado no trecho a seguir:

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

Quando o teste é executado, os resultados de teste serão retornados na forma de um arquivo XML de estilo NUnit chamado **Report. xml**. TeamCity exibirá as informações no log de compilação.

Para obter mais informações sobre como enviar UITests para Test Cloud, consulte [preparando aplicativos xamarin. Android](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest) ou [preparando aplicativos xamarin. Ios](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest).

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Enviando testes de Calabash para Test Cloud

Os testes de Calabash são enviados usando o gem `test-cloud`, conforme mostrado no trecho a seguir:

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```

Para enviar um aplicativo Android para Test Cloud, é necessário primeiro recriar o servidor de teste APK usando o Calabash-Android:

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```

Para obter mais informações sobre como enviar testes de Calabash, consulte o guia do Xamarin sobre como [Enviar testes de Calabash para Test Cloud](https://github.com/calabash/calabash-ios/wiki).

## <a name="creating-a-teamcity-project"></a>Criando um projeto TeamCity

Quando o TeamCity é instalado e Visual Studio para Mac pode criar seu projeto, é hora de criar um projeto no TeamCity para criar o projeto e enviá-lo para Test Cloud.

1. Iniciado ao fazer logon no TeamCity por meio do navegador da Web. Navegue até o projeto raiz:

    ![Navegue até o projeto raiz](teamcity-images/image2.png "Navegue até o projeto raiz") Abaixo do projeto raiz, crie um novo subprojeto:

    ![Navegue até o projeto raiz abaixo do projeto raiz, crie um novo subprojeto](teamcity-images/image3.png "Navegue até o projeto raiz abaixo do projeto raiz, crie um novo subprojeto")
2. Depois que o subprojeto tiver sido criado, adicione uma nova configuração de compilação:

    ![Depois que o subprojeto tiver sido criado, adicione uma nova configuração de compilação](teamcity-images/image5.png "Depois que o subprojeto tiver sido criado, adicione uma nova configuração de compilação")
3. Anexe um projeto do VCS à configuração de compilação. Isso é feito por meio da tela de configuração de controle de versão:

    ![Isso é feito por meio da tela de configuração de controle de versão](teamcity-images/image6.png "Isso é feito por meio da tela de configuração de controle de versão")

    Se não houver nenhum projeto do VCS criado, você terá a opção de criar um da nova página raiz do VCS mostrada abaixo:

    ![Se não houver nenhum projeto do VCS criado, você terá a opção de criar um na página raiz do novo VCS](teamcity-images/image7.png "Se não houver nenhum projeto do VCS criado, você terá a opção de criar um na página raiz do novo VCS")

    Depois que a raiz do VCS tiver sido anexada, o TeamCity fará o check-out do projeto e tentará detectar automaticamente as etapas de compilação. Se você estiver familiarizado com o TeamCity, poderá selecionar uma das etapas de compilação detectadas. É seguro ignorar as etapas de compilação detectadas por enquanto.

4. Em seguida, configure um gatilho de compilação. Isso colocará uma compilação em fila quando determinadas condições forem atendidas, como quando um usuário confirmar o código para o repositório. A captura de tela a seguir mostra como adicionar um gatilho de compilação:

    ![Esta captura de tela mostra como adicionar um gatilho de compilação](teamcity-images/image8.png "Esta captura de tela mostra como adicionar um gatilho de compilação") Um exemplo de configuração de um gatilho de compilação pode ser visto na seguinte captura de tela:

    ![Um exemplo de configuração de um gatilho de compilação pode ser visto nesta captura de tela](teamcity-images/image9.png "Um exemplo de configuração de um gatilho de compilação pode ser visto nesta captura de tela")

5. A seção anterior, parametrizando o script de Build, sugerindo o armazenamento de alguns valores como variáveis de ambiente. Essas variáveis podem ser adicionadas à configuração de compilação por meio da tela parâmetros. Adicione as variáveis para a chave de API Test Cloud, a ID do dispositivo iOS e a ID do dispositivo Android, conforme mostrado na captura de tela abaixo:

    ![Adicione as variáveis para a chave de API Test Cloud, a ID do dispositivo iOS e a ID do dispositivo Android](teamcity-images/image11.png "Adicione as variáveis para a chave de API Test Cloud, a ID do dispositivo iOS e a ID do dispositivo Android")

6. A etapa final é adicionar uma etapa de compilação que invocará o script de compilação para compilar o aplicativo e enfileirar o aplicativo para Test Cloud. A captura de tela a seguir é um exemplo de uma etapa de compilação que usa um Rakefile para criar um aplicativo:

    ![Esta captura de tela é um exemplo de uma etapa de compilação que usa um Rakefile para criar um aplicativo](teamcity-images/image12.png "Esta captura de tela é um exemplo de uma etapa de compilação que usa um Rakefile para criar um aplicativo")

7. Neste ponto, a configuração da compilação foi concluída. É uma boa ideia disparar uma compilação para confirmar que o projeto está configurado corretamente. Uma boa maneira de fazer isso é confirmar uma alteração pequena e insignificante no repositório. TeamCity deve detectar a confirmação e iniciar uma compilação.

8. Depois que a compilação for concluída, inspecione o log de compilação e veja se há problemas ou avisos com a compilação que exigem atenção.

## <a name="summary"></a>Resumo

Este guia abordou como usar o TeamCity para criar aplicativos móveis Xamarin e, em seguida, enviá-los para Test Cloud. Discutimos a criação de um script de compilação para automatizar o processo de compilação. O script de Build cuida da compilação do aplicativo, do envio ao Test Cloud e da espera pelos resultados

Em seguida, abordamos como criar um projeto no TeamCity que colocará em fila uma compilação cada vez que um desenvolvedor confirmar o código e chamará o script de Build.

## <a name="related-links"></a>Links relacionados

- [Preparando aplicativos Xamarin. Android](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest)
- [Preparando aplicativos Xamarin. iOS](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [Instalando e Configurando o TeamCity](https://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
