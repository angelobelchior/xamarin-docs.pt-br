---
title: Certificados e identificadores no Xamarin.Mac
description: Esse guia o orienta durante a criação dos Certificados e Identificadores que serão necessários para publicar um aplicativo Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 393d0066-7f6f-4ac3-a48d-4b5db65bc4cd
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 59c3372350caf3939a4e40ba2999ffb490d4a1f7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030089"
---
# <a name="certificates-and-identifiers-in-xamarinmac"></a>Certificados e identificadores no Xamarin.Mac

_Esse guia lhe orienta durante a criação dos Certificados e Identificadores que serão necessários para publicar um aplicativo Xamarin.Mac._

## <a name="certificates-and-identifiers"></a>Certificados e identificadores

Visite o [Apple Developer Member Center (Central de Associados do Desenvolvedor da Apple)](https://developer.apple.com) para configurar o Mac para desenvolvimento. O menu principal é mostrado abaixo:

[![O centro de membros do desenvolvedor da Apple](certificates-identifiers-images/devcenter01.png "O centro de membros do desenvolvedor da Apple")](certificates-identifiers-images/devcenter01-large.png#lightbox)

Clique no link **Certificates, Identifiers & Profiles (Certificados, Identificadores e Perfis)** :

[![Selecionando certificados, identificadores & perfis](certificates-identifiers-images/devcenter02.png "Selecionando certificados, identificadores & perfis")](certificates-identifiers-images/devcenter02-large.png#lightbox)

Em seguida, clique no **Certificates Link (Link de Certificados)** sob a seção **Mac Apps (Aplicativos do Mac)** :

[![Selecionando o link de certificados](certificates-identifiers-images/devcenter03.png "Selecionando o link de certificados")](certificates-identifiers-images/devcenter03-large.png#lightbox)

Clique no link **All (Todos)** e clique no botão **+** :

[![Selecionando tudo e adicionando um novo item](certificates-identifiers-images/certif01.png "Selecionando tudo e adicionando um novo item")](certificates-identifiers-images/certif01-large.png#lightbox)

Daqui, baixe os **Intermediate Certificates (Certificados intermediários)** (Autoridade de Certificação de Relações de Desenvolvedores em todo o mundo e Autoridade de Certificação de ID de Desenvolvedor), se necessário. No entanto, eles devem ser instalados automaticamente para o desenvolvedor pelo Xcode.

O restante dessa seção descreve cada uma das quatro seções para concluir a instalação de uma Conta de Desenvolvedor Mac.

- **Register Mac App ID (Registrar a ID do aplicativo Mac)** – O desenvolvedor precisa seguir essas etapas para cada aplicativo que grava.
- **Register macOS Systems (Registrar sistemas macOS)** – Isso só é necessário ao adicionar computadores para teste.
- **Create Certificates (Criar certificados)** – Necessário somente uma vez ao definir os certificados e, posteriormente, ao renová-los.
- **Create Provisioning Profile (Criar perfil de provisionamento)** – O desenvolvedor precisa seguir essas etapas para cada novo aplicativo gravado e ao adicionar novos sistemas.

Clique no link **Overview (Visão geral)** na parte superior esquerda da página para retornar ao menu a qualquer momento.

### <a name="register-mac-app-id"></a>Registrar a ID do Aplicativo Mac

O desenvolvedor precisa registrar uma ID do aplicativo para cada aplicativo gravado. Use as etapas abaixo para criar uma entrada para um aplicativo de exemplo básico chamado "MacWriter".

1. Insira uma **App ID Description (Descrição da ID do Aplicativo)** e selecione quaisquer **App Services (Serviços de Aplicativos)** que o aplicativo exigir:

    [![Inserindo a descrição e os serviços de aplicativos](certificates-identifiers-images/devcenter04.png "Inserindo a descrição e os serviços de aplicativos")](certificates-identifiers-images/devcenter04-large.png#lightbox)
2. Insira uma **Bundle ID (ID do pacote)** para o aplicativo e clique no botão **Continue (Continuar)** :

    [![Inserindo uma ID de pacote](certificates-identifiers-images/devcenter05.png "Inserindo uma ID de pacote")](certificates-identifiers-images/devcenter05-large.png#lightbox)
3. Verifique as informações e clique no botão **Submit (Enviar)** :

    [![Verificando as informações](certificates-identifiers-images/devcenter06.png "Verificando as informações")](certificates-identifiers-images/devcenter06-large.png#lightbox)

Alguns **App Services (Serviços de Aplicativos)** poderão exigir outras configurações (por exemplo, iCloud). Se esse for o caso, selecione a nova ID do aplicativo recém-criada e clique no botão **Edit (Editar)** :

[![Editando a nova ID do aplicativo](certificates-identifiers-images/devcenter07.png "Editando a nova ID do aplicativo")](certificates-identifiers-images/devcenter07-large.png#lightbox)

Para configurar os serviços do iCloud, clique no botão **Edit (Editar)** :

[![Configurando os serviços do iCloud](certificates-identifiers-images/devcenter08.png "Configurando os serviços do iCloud")](certificates-identifiers-images/devcenter08-large.png#lightbox)

Aqui, o desenvolvedor pode configurar os bancos de dados que usará:

[![Configurando os bancos de dados](certificates-identifiers-images/devcenter09.png "Configurando os bancos de dados")](certificates-identifiers-images/devcenter09-large.png#lightbox)

### <a name="register-macos-systems"></a>Registrar sistemas macOS

Para criar um perfil de provisionamento para teste, o desenvolvedor precisa ter seus computadores Mac registrados. Ele pode registrar no máximo 100 computadores para testar seus aplicativos Mac.

Na Central de Desenvolvedores Mac, selecione **All (Todos)** na seção **Devices (Dispositivos)** e clique no botão **+** :

[![Adicionando um novo computador](certificates-identifiers-images/devcenter10.png "Adicionando um novo computador")](certificates-identifiers-images/devcenter10-large.png#lightbox)

Insira um **Name (Nome)** e o **UUID** do computador para adicionar e clique no botão **Continue (Continuar)** . Examine as informações e clique no botão **Register (Registrar)** :

[![Inserindo as informações do novo computador](certificates-identifiers-images/devcenter11.png "Inserindo as informações do novo computador")](certificates-identifiers-images/devcenter11-large.png#lightbox)

### <a name="create-certificates"></a>Criar certificados

Use a seção Certificados para criar vários tipos diferentes de certificados que serão usados para assinar os Aplicativos Mac:

[![Criando um novo certificado](certificates-identifiers-images/certif01.png "Criando um novo certificado")](certificates-identifiers-images/certif01-large.png#lightbox)

Há três tipos principais de certificado:

- **Certificado de Desenvolvimento Mac** – Opcional para o desenvolvimento de aplicativos gerais, mas obrigatório se o desenvolvedor planeja usar recursos, como o iCloud ou as notificações por push. O desenvolvedor precisará de um Certificado de Desenvolvimento antes que ele possa criar perfis de provisionamento que lhe permita acessar esses recursos.
- **Mac App Store** – O desenvolvedor precisará de um certificado para seu aplicativo e outro certificado para o instalador.
- **ID de desenvolvedor** – Certificados para o aplicativo e para o instalador, se optar por distribuir fora da Mac App Store.

As seções a seguir fornecerão exemplos da criação de cada um dos tipos de certificado acima.

#### <a name="mac-development-certificate"></a>Certificado de desenvolvimento do Mac

Conforme mencionado anteriormente, o Certificado de Desenvolvimento de Aplicativo Mac não é necessário, a menos que recursos macOS, como o iCloud ou as notificações por push, estejam em uso.

Faça o seguinte para criar um novo Certificado de Desenvolvimento:

1. Selecione o botão de opção **Mac Development (Desenvolvimento do Mac)** e clique em **Continue (Continuar)** :

     [![Adicionando um certificado de desenvolvimento](certificates-identifiers-images/certif02.png "Adicionando um certificado de desenvolvimento")](certificates-identifiers-images/certif02-large.png#lightbox)
2. A próxima tela explica como usar o Acesso ao Conjunto de Chaves para criar um certificado de assinatura do arquivo de solicitação para upload:

    [![A tela de carregamento de acesso do conjunto de chaves](certificates-identifiers-images/certif03.png "A tela de carregamento de acesso do conjunto de chaves")](certificates-identifiers-images/certif03-large.png#lightbox)
3. Escolha um Nome Comum significativo para o certificado, de forma que ele seja facilmente reconhecível posteriormente quando o certificado final for criado. Lembre-se em qual lugar o arquivo foi salvo, assim ele poderá ser encontrado na próxima etapa:

    ![Exportando um certificado](certificates-identifiers-images/image12.png "Exportando um certificado")
4. Um arquivo de solicitação de certificado (extensão `.certSigningRequest`) será salvo localmente no Mac. Lembre-se em qual lugar ele foi salvo (o local padrão é a Área de Trabalho). Ele precisará ser selecionado na próxima etapa:

    [![Carregando o arquivo de certificado](certificates-identifiers-images/image13.png "Carregando o arquivo de certificado")](certificates-identifiers-images/image13-large.png#lightbox)
5. Clique em **Download (Baixar)** para obter o certificado e clique duas vezes para instalá-lo no **Keychain (Conjunto de Chaves)** :

    [![Baixando um certificado de desenvolvimento](certificates-identifiers-images/image15.png "Baixando um certificado de desenvolvimento")](certificates-identifiers-images/image15-large.png#lightbox)
6. Clique em **Download (Baixar)** para obter o certificado e clique duas vezes para instalá-lo no **Keychain (Conjunto de Chaves)** . O **Developer Certificate Utility (Utilitário de Certificado do Desenvolvedor)** mostrará os certificados como este:

    [![O utilitário de certificado do desenvolvedor](certificates-identifiers-images/image16.png "O utilitário de certificado do desenvolvedor")](certificates-identifiers-images/image16-large.png#lightbox)
7. Ele também aparecerá no **Keychain (Conjunto de Chaves)** dessa forma:

    ![O certificado no acesso ao conjunto de chaves](certificates-identifiers-images/image17.png "O certificado no acesso ao conjunto de chaves")

Conforme mencionado anteriormente, o certificado do Desenvolvedor nem sempre é necessário, a menos que o desenvolvedor esteja implementando recursos macOS, como o iCloud e as notificações por push. Ele também é necessário para criar um **Development Provisioning Profile (Perfil de Provisionamento de Desenvolvimento)** , que será necessário para testar os aplicativos da Mac App Store.

#### <a name="mac-app-store-certificates"></a>Certificados da Mac App Store

Para lançar um aplicativo na App Store, será necessário criar um certificado da **Mac App Store**, que será usado para assinar o aplicativo e o Pacote do Instalador Mac.

1. Selecione **Mac App Store** como o tipo de certificado e clique no botão **Continue (Continuar)** :

    [![Criando um certificado de loja de aplicativos](certificates-identifiers-images/certif04.png "Criando um certificado de loja de aplicativos")](certificates-identifiers-images/certif04-large.png#lightbox)
2. Selecione o tipo de certificado a ser criado (será necessário um de cada tipo para lançar na App Store):

    [![Selecionando o tipo de certificado](certificates-identifiers-images/certif05.png "Selecionando o tipo de certificado")](certificates-identifiers-images/certif05-large.png#lightbox)
3. A próxima página explica como usar o **Acesso ao Conjunto de Chaves** para gerar um arquivo de solicitação de certificado. Siga as instruções:

    [![Gerando a solicitação de conjunto de chaves](certificates-identifiers-images/certif06.png "Gerando a solicitação de conjunto de chaves")](certificates-identifiers-images/certif06-large.png#lightbox)
4. Escolha um **Common Name (Nome Comum)** descritivo – por exemplo, use o texto “App Store Application” no nome:

    ![Inserindo um nome descritivo](certificates-identifiers-images/image20.png "Inserindo um nome descritivo")
5. Um arquivo de solicitação de certificado (extensão `.certSigningRequest`) será salvo localmente no Mac. Lembre-se em qual lugar ele foi salvo (o local padrão é a Área de Trabalho):

    [![Salvando o certificado](certificates-identifiers-images/image21.png "Salvando o certificado")](certificates-identifiers-images/image21-large.png#lightbox)
6. Clique em **Download (Baixar)** para obter seu certificado e clique duas vezes para instalá-lo no **Keychain (Conjunto de Chaves)** :

    [![Baixando o certificado da App Store](certificates-identifiers-images/image23.png "Baixando o certificado da App Store")](certificates-identifiers-images/image23-large.png#lightbox)
7. Clique em **Continue (Continuar)** e siga as mesmas etapas exatas para baixar outro certificado, desta vez será para o *Instalador*:

    [![Selecionando o instalador](certificates-identifiers-images/image24.png "Selecionando o instalador")](certificates-identifiers-images/image24-large.png#lightbox)
8. Escolha um **Common Name (Nome Comum)** descritivo – por exemplo, use o texto “AppStore Installer” no nome:

    ![Definindo o nome do certificado](certificates-identifiers-images/image25.png "Definindo o nome do certificado")
9. Um arquivo de solicitação de certificado (extensão `.certSigningRequest`) será salvo localmente no Mac. Lembre-se em qual lugar ele foi salvo (o local padrão é a Área de Trabalho):

    [![Carregando o certificado](certificates-identifiers-images/image26.png "Carregando o certificado")](certificates-identifiers-images/image26-large.png#lightbox)

    [![Baixando o certificado de distribuição](certificates-identifiers-images/image28.png "Baixando o certificado de distribuição")](certificates-identifiers-images/image28-large.png#lightbox)
10. Clique em **Download (Baixar)** para obter o certificado e clique duas vezes para instalá-lo no **Keychain (Conjunto de Chaves)** . O Developer Certificate Utility (Utilitário de Certificado do Desenvolvedor) mostrará os certificados como este:

    [![O utilitário de certificado do desenvolvedor](certificates-identifiers-images/image29.png "O utilitário de certificado do desenvolvedor")](certificates-identifiers-images/image29-large.png#lightbox)
11. Os dois novos certificados agora estarão visíveis no **Conjunto de Chaves**:

    [![O certificado no acesso ao conjunto de chaves](certificates-identifiers-images/image30.png "O certificado no acesso ao conjunto de chaves")](certificates-identifiers-images/image30-large.png#lightbox)

#### <a name="developer-id-certificates"></a>Certificados de ID de desenvolvedor

Para lançar automaticamente um aplicativo Xamarin.Mac (em vez de lançar pela Apple App Store), o desenvolvedor precisará de um Certificado de ID de Desenvolvedor para assinar o aplicativo para o lançamento e a instalação.

Faça o seguinte:

1. Na seção **Certificates (Certificados)** , comece ao clicar no botão **+** e, em seguida, selecione o botão de opção **Developer ID (ID de Desenvolvedor)** :

    [![Adicionando uma ID de desenvolvedor](certificates-identifiers-images/certif07.png "Adicionando uma ID de desenvolvedor")](certificates-identifiers-images/certif07-large.png#lightbox)
2. Clique no botão **Continue (Continuar)** e selecione o tipo de ID de desenvolvedor para criar:

    [![Selecionando o tipo de ID do desenvolvedor](certificates-identifiers-images/certif08.png "Selecionando o tipo de ID do desenvolvedor")](certificates-identifiers-images/certif08-large.png#lightbox)
3. Dois serão necessários, um para assinar o aplicativo em si e outro para assinar o instalador do aplicativo. Tenha cuidado ao nomear as solicitações de certificado para essas chaves: use nomes descritivos que incluem o texto `Application` e o `Installer` para que possam ser diferenciados posteriormente.
4. A próxima tela fornecerá instruções detalhadas sobre como criar o certificado, clique no botão **Continue (Continuar)** :

    [![Como criar o certificado](certificates-identifiers-images/certif09.png "Como criar o certificado")](certificates-identifiers-images/certif09-large.png#lightbox)
5. Escolha um **nome comum** descritivo – por exemplo, use o texto "aplicativo de ID do desenvolvedor" no nome:

    ![Inserindo um nome para o certificado](certificates-identifiers-images/image33.png "Inserindo um nome para o certificado")
6. Um arquivo de solicitação de certificado (extensão `.certSigningRequest`) será salvo localmente em seu Mac. Lembre-se em qual lugar ele foi salvo (o local padrão é a Área de Trabalho):

    [![Carregando o certificado](certificates-identifiers-images/certif10.png "Carregando o certificado")](certificates-identifiers-images/certif10-large.png#lightbox)

    [![Baixando a ID do desenvolvedor](certificates-identifiers-images/certif11.png "Baixando a ID do desenvolvedor")](certificates-identifiers-images/certif11-large.png#lightbox)
7. Clique em **Download (Baixar)** para obter o certificado e clique duas vezes para instalá-lo no **Keychain (Conjunto de Chaves)** .
8. Clique em **Continue (Continuar)** e siga as mesmas etapas exatas para baixar outro certificado, dessa vez será para o *Instalador*.
9. Escolha um **nome comum** descritivo – por exemplo, use o texto "instalador da ID do desenvolvedor" no nome:

    ![Inserindo um nome comum](certificates-identifiers-images/image38.png "Inserindo um nome comum")
10. Um arquivo de solicitação de certificado (extensão `.certSigningRequest`) será salvo localmente no Mac. Lembre-se em qual lugar ele foi salvo (o local padrão é a Área de Trabalho):

    [![Carregando um certificado](certificates-identifiers-images/certif10.png "Carregando um certificado")](certificates-identifiers-images/certif10-large.png#lightbox)
11. O certificado estará disponível para download – clique no botão **Download (Baixar)** antes de clicar em **Done (Concluído)** :

    [![Baixando um certificado](certificates-identifiers-images/certif11.png "Baixando um certificado")](certificates-identifiers-images/certif11-large.png#lightbox)
12. Clique em **Download (Baixar)** para obter o certificado e clique duas vezes para instalá-lo no **Keychain (Conjunto de Chaves)** . O **Developer Certificate Utility (Utilitário de Certificado do Desenvolvedor)** mostrará os certificados como este:

    [![O utilitário de certificado do desenvolvedor](certificates-identifiers-images/certif12.png "O utilitário de certificado do desenvolvedor")](certificates-identifiers-images/certif12-large.png#lightbox)
13. Os seguintes itens ficarão visíveis no **Keychain (Conjunto de Chaves)** :

    [![O certificado no acesso ao conjunto de chaves](certificates-identifiers-images/image43.png "O certificado no acesso ao conjunto de chaves")](certificates-identifiers-images/image43-large.png#lightbox)

## <a name="related-links"></a>Links relacionados

- [Instalação](/visualstudio/mac/installation/)
- [Amostra do Hello, Mac](~/mac/get-started/hello-mac.md)
- [Distribua aplicativos na Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [ID de Desenvolvedor e GateKeeper](https://developer.apple.com/resources/developer-id/)
