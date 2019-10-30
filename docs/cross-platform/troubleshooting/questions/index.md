---
title: Perguntas frequentes gerais
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 0e49ef8fa0bf00d5ed41f3411393ffaf4891c1b8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013991"
---
# <a name="general-frequently-asked-questions"></a>Perguntas frequentes gerais

## <a name="portable-class-libraries"></a>Bibliotecas de Classes Portáteis

### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[Como posso exibir as bibliotecas com suporte em um PCL?](pcl-support-libraries.md)
Este guia lista recursos e métodos para determinar se a biblioteca existente tem suporte nas várias plataformas de destino PCL ou pode ser convertida em um perfil PCL.

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[API de reflexão de PCL](pcl-reflection.md)
A Microsoft desenvolveu uma nova API de reflexão para uso em bibliotecas de classes portáteis. Se você tiver algum código de reflexão existente que deseja mover para um PCL, ele poderá não funcionar.

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[Estudo de caso de PCL: como posso resolver problemas relacionados a System.Diagnostics.Tracing para o pacote NuGet de Fluxo de Dados de TPL da Microsoft?](pcl-case-study.md)
O xamarin. iOS e o Xamarin. Android não implementam 100% de cada perfil PCL que eles permitem como referências. Para conveniência prática no Visual Studio para Mac, no Visual Studio e no Gerenciador de pacotes NuGet, os projetos do Xamarin permitem o uso de vários perfis que têm apenas implementações incompletas. Por exemplo, nem Xamarin. iOS nem Xamarin. Android atualmente incluem uma implementação completa dos tipos no namespace de `System.Diagnostics.Tracing` PCL. Você pode contornar isso alternando o projeto de aplicativo para fazer referência à versão portátil-Net45 + win8 + WP8 + wpa81 da biblioteca de Dataflow TPL.

## <a name="nuget-packages--xamarin-components"></a>Pacotes NuGet & componentes do Xamarin
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[Como posso atualizar o NuGet?](nuget-update.md)
As atualizações, extensões e suplementos do NuGet podem ser encontrados na guia **atualizações** no **Gerenciador de pacotes NuGet**. Navegação detalhada para localizar as atualizações no Visual Studio para Mac & o Visual Studio está neste guia.

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[Como fazer downgrade de um pacote NuGet?](nuget-package-downgrade.md)
Visual Studio para Mac & o Visual Studio tem recursos para selecionar versões mais antigas de pacotes e instalá-los automaticamente; semelhante a como a atualização de pacotes funciona.

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Erro de pacotes ausentes após atualizar os pacotes do Nuget](nuget-packages-missing.md)
Esse problema foi relatado principalmente em soluções de aplicativo de exemplo do Xamarin. Forms, mas o potencial para esse problema pode ocorrer em qualquer projeto que usa pacotes NuGet.

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Unificar os componentes do Google Play Services e o NuGet](gps-components-nuget.md)
Há vários componentes Google Play Services e pacotes NuGet, mas para facilitar as coisas para os desenvolvedores, agora nós unificamos nossos componentes e pacotes NuGet em duas. Em quase todos os casos, Google Play Services deve ser usado. O único motivo para usar o pacote (Froyo) é se você estiver pretendendo ativamente a Froyo.

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[Onde meus componentes são armazenados no meu computador?](component-storage.md)
Sempre que você instala um componente do Xamarin em um projeto de aplicativo, ele é colocado nos dois locais listados neste guia.

## <a name="troubleshooting"></a>Solução de problemas
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[Onde posso encontrar informações e logs da minha versão?](version-logs.md)
Este guia fornece detalhes sobre onde encontrar a maioria das informações de diagnóstico que podem ser usadas para solucionar problemas com o Xamarin.

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[Quando e como eu devo criar um relatório de bugs?](howto-file-bug.md)
Este guia fornece dicas para o arquivamento de relatórios de bugs de alta qualidade, para que nossos engenheiros possam determinar a causa (e quaisquer possíveis correções) para um problema com mais eficiência.

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Por que não há suporte para Jenkins no Xamarin?](xamarin-jenkins.md)
Jenkins é um pacote de CI de código-fonte aberto; por causa dessa quantidade de problemas que são provocados diretamente pelo Jenkins em *si* , precisarão ser arquivados como problemas em relação ao local em que você obteve o código; como o [repositório Jenkins principal](https://github.com/jenkinsci/jenkins)ou o repositório para [Jenkins. app](https://github.com/stisti/jenkins-app).

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[Quais configurações do projeto são necessárias para o depurador?](debugger-settings.md)
Para que o depurador funcione conforme o esperado (pontos de interrupção de acesso, exibir logs de depuração etc.), a instrumentação do desenvolvedor e a exibição de informações de depuração devem ser habilitadas. Este guia fornece detalhes sobre como localizar e ativar essas configurações.
