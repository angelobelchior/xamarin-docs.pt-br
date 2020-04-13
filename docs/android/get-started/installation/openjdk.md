---
title: Distribuição do Mobile OpenJDK da Microsoft
description: Um guia passo a passo para configuração e solução de problemas da distribuição do OpenJDK para Desenvolvimento Móvel da Microsoft.
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: a24edbc10d529878092b474df7f186d14049d5e0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2020
ms.locfileid: "60955062"
---
# <a name="microsofts-mobile-openjdk-distribution"></a>Distribuição do Mobile OpenJDK da Microsoft

_Este guia descreve as etapas para mudar para uma distribuição interna do OpenJDK. Esta distribuição destina-se ao desenvolvimento móvel._

## <a name="overview"></a>Visão geral

Começando pelo Visual Studio 15.9 e pelo Visual Studio para Mac 7.7, as Ferramentas do Visual Studio para Xamarin mudaram do JDK da Oracle para uma **versão leve do OpenJDK que se destina exclusivamente ao desenvolvimento para Android**. Isso é uma migração necessária, pois a Oracle está encerrando o suporte para distribuição comercial do JDK 8 em 2019 e o JDK 8 é uma dependência necessária para todo o desenvolvimento do Android.

Os benefícios dessa mudança são:

- Você sempre terá uma versão do OpenJDK que funciona para desenvolvimento para Android.

- Baixar o JDK 9 ou superior da Oracle não afetará a experiência de desenvolvimento.

- Redução no volume de memória e tamanho do download.

- Não haverá mais nenhum problema com os instaladores e servidores de terceiros.

Caso queira mudar mais cedo para a experiência aprimorada, há builds de distribuição do Microsoft Mobile OpenJDK disponíveis para teste no Windows e no Mac. O processo de configuração é descrito abaixo, e você poderá reverter para o JDK da Oracle a qualquer momento.

## <a name="download"></a>Baixar

A distribuição do Mobile OpenJDK será instalada automaticamente para você, se você selecionar os pacotes do SDK do Android no instalador do Visual Studio no Windows.

No Mac, o Mobile OpenJDK será instalado como parte da carga de trabalho do Android para novas instalações. Para usuários existentes do Visual Studio para Mac, será solicitado que você o instale como parte da atualização. O IDE solicitará que você mude para o novo JDK e passará a usá-lo na próxima reinicialização.

## <a name="troubleshooting"></a>Solução de problemas

Se encontrar problemas com a instalação no Mac ou Windows, você poderá executar as seguintes etapas para configuração manual:

Verifique se o OpenJDK está instalado no computador no local correto:

- **Mac** &ndash; **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.x**
- **Windows** &ndash; **C:\\Arquivos de Programas\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.x**

Aponte o IDE para o novo JDK:

- **Mac** &ndash; Clique em **Ferramentas > Gerenciador de SDK > Locais** e altere o **Local do SDK do Java (JDK)** para o caminho completo da instalação do OpenJDK. No exemplo a seguir, esse caminho é definido como **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**, mas sua versão pode ser mais recente.

![Definir o caminho do JDK para a distribuição do Microsoft Mobile OpenJDK no Mac](openjdk-images/vsm.png)

- **Windows** &ndash; Clique em **Ferramentas > Opções > Xamarin > Configurações do Android** e altere o **Local do Java Development Kit** para o caminho completo da instalação do OpenJDK. No exemplo a seguir, esse caminho é definido como **C:\\Arquivos de Programas\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**, mas sua versão pode ser mais recente:

![Definir o caminho do JDK para a distribuição do Microsoft Mobile OpenJDK no Windows](openjdk-images/vs.png)

## <a name="known-issues"></a>Problemas conhecidos

### <a name="package-openjdkv1regkeyversion18025chipx64-failed-to-install"></a>Falha na instalação do pacote 'OpenJDKV1.RegKey,version=1.8.0.25,chip=x64'

Isso pode ser um problema em alguns ambientes corporativos. O OpenJDK já está no computador – siga as [etapas de solução de problemas acima](#troubleshooting) para apontar o seu IDE para o local correto. Você pode seguir os status dos problemas [aqui](https://developercommunity.visualstudio.com/content/problem/382549/packageidopenjdkv1regkeypackageactioninstallreturn.html).

## <a name="summary"></a>Resumo

Neste artigo, você aprendeu a configurar seu IDE para usar a distribuição do Mobile OpenJDK da Microsoft e a solucionar problemas caso venha a encontrá-los.
