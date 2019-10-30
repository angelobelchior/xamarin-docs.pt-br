---
title: Assemblies disponíveis
description: Este documento lista os assemblies disponíveis para uso no Xamarin. iOS, Xamarin. Android e Xamarin. Mac. Ele também fornece links para a documentação sobre bibliotecas de .NET Standard e bibliotecas de classes portáteis.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: 238011b4762f2d394629e75fbde476e618219df2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016360"
---
# <a name="available-assemblies"></a>Assemblies disponíveis

O xamarin. iOS, o Xamarin. Android e o Xamarin. Mac são fornecidos com mais de uma dúzia de assemblies. Assim como o Silverlight é um subconjunto estendido dos assemblies do .NET da área de trabalho, as plataformas Xamarin também são um subconjunto estendido de vários assemblies do Silverlight e do .NET da área de trabalho

As plataformas Xamarin não são compatíveis com ABI com assemblies existentes compilados para um perfil diferente. Você deve recompilar o código-fonte para gerar assemblies destinados ao perfil correto (assim como você precisa recompilar o código-fonte para o Silverlight e o .NET 3,5 de destino separadamente).

Os aplicativos Xamarin. Mac podem ser compilados em três modos: um que usa o perfil móvel organizado do Xamarin, a estrutura Xamarin. Mac .NET 4,5, que permite que você direcione assemblies de área de trabalho completos existentes e um que usa a API .NET encontrada em um sistema mono instalação. Para obter mais informações, consulte nossa documentação de [estruturas de destino](~/mac/platform/target-framework.md) .

## <a name="net-standard-libraries"></a>Bibliotecas de .NET Standard

Além das associações do iOS, do Android e do Mac, os projetos do Xamarin podem consumir [.net Standard bibliotecas](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Bibliotecas de Classes Portáteis

Os projetos do Xamarin também podem consumir [bibliotecas de classes portáteis do .net](~/cross-platform/app-fundamentals/pcl.md), embora essa tecnologia esteja sendo substituída em favor da .net Standard.

## <a name="supported-assemblies"></a>Assemblies com suporte

Esses são os assemblies disponíveis no **Gerenciador de referências > assemblies > Framework** (Visual Studio 2017) e **Editar referências > pacotes** (Visual Studio para Mac) e sua compatibilidade com as plataformas Xamarin.

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|Compatibilidade de API|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N. dll|Inclui CJK, Oriente Médio, outro, raro, oeste|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|Provedor de ADO.NET para SQLite; consulte limitações.|✓|✓|✓|
> |Mono.Data.Tds.dll|Suporte a protocolo TDS; usado para suporte a [System. Data. SqlClient](xref:System.Data.SqlClient) em [System. Data](xref:System.Data).|✓|✓|✓|
> |Mono. Dynamic. &#8203;Intérprete. dll| |✓| | |
> |Mono.Security.dll|APIs de criptografia.|✓|✓|✓|
> |monotouch.dll|Esse assembly contém a C# associação à API Cocoatouch. Isso só está disponível em projetos clássicos do iOS.|✓| | |
> |MonoTouch. &#8203;Dialog-1. dll| |✓| | |
> |MonoTouch. &#8203;Novo nunitlite. dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|As APIs do OpenGL/com orientação a objeto aberto, estendidas para fornecer suporte a dispositivos iPhone.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), além de tipos dos seguintes namespaces:<br />System.Collections.Specialized<br />Sistema. &#8203;ComponentModel<br />System. ComponentModel. Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System .net. cache<br />System.Net.Mail<br />System .net. MIME<br />System.Net. &#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System. Runtime. &#8203;InteropServices<br />System.Runtime.Versioning<br />System. Security. &#8203;AccessControl<br />System.Security.Authentication<br />System. Security. &#8203;Criptografia<br />System.Security.Permissions<br />System.Threading<br />System. Timers|✓|✓|✓|
> |Sistema. &#8203;ComponentModel. &#8203;Composição. dll| |✓|✓|✓|
> |Sistema. &#8203;ComponentModel. &#8203;Annotations. dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.Net 3,5](https://msdn.microsoft.com/library/ms229335.aspx) , com [algumas funcionalidades removidas](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |System. Data. &#8203;Serviços do. &#8203;Client. dll|Cliente oData completo.|✓|✓|✓|
> |System.IO. &#8203;Compactação| |✓|✓|✓|
> |System.IO. &#8203;Compactação. &#8203;Sistema de arquivos| |✓|✓|✓|
> |System.Json.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net. &#8203;Http. dll| |✓|✓|✓|
> |Sistema. &#8203;Numéricos. dll| |✓|✓|✓|
> |System. Runtime. &#8203;Serialization. dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |Sistema. &#8203;ServiceModel. dll|Pilha do WCF como presente no [Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |Sistema. &#8203;ServiceModel. &#8203;Interno. dll| |✓|✓|✓|
> |Sistema. &#8203;ServiceModel. &#8203;Web. dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), além de tipos dos seguintes namespaces: <br />Sistema<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |Sistema. &#8203;Transactions. dll|[.Net 3,5](https://msdn.microsoft.com/library/ms229335.aspx); parte do suporte a [System. Data](~/ios/data-cloud/system.data.md) .|✓|✓|✓|
> |System. Web. &#8203;Services. dll|Serviços Web básicos do perfil .NET 3,5, com os recursos do servidor removidos.|✓|✓|✓|
> |Sistema. &#8203;Windows. dll| |✓|✓|✓|
> |Sistema. &#8203;XML. dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System. xml. &#8203;LINQ. dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Esse assembly contém a C# associação à API Cocoatouch. Isso só é usado em projetos iOS unificados.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono. Android. &#8203;Exportar. dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |Sistema. &#8203;EnterpriseServices. dll| | |✓| |
> |Xamarin. Android. &#8203;Novo nunitlite. dll| | |✓| |
> |Mono. CompilerServices. &#8203;SymbolWriter. dll|Para escritores de compilador.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |Sistema. &#8203;Drawing. dll|Não há suporte para System. Drawing no API Unificada para o Xamarin. Mac, o .NET 4,5 ou para as estruturas móveis. O suporte a System. Drawing pode ser adicionado ao iOS e ao macOS usando a biblioteca [sysdrawing-CoreGraphics](https://github.com/mono/sysdrawing-coregraphics)|✓| |✓|
