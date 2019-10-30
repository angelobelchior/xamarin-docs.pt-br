---
title: Codificações de internacionalização no Xamarin. iOS
description: Este documento descreve as codificações de internacionalização no Xamarin. iOS, discutindo as codificações disponíveis e como adicioná-las a um aplicativo.
ms.prod: xamarin
ms.assetid: F5117294-28BB-4583-B6A0-A339B050FDE1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/28/2017
ms.openlocfilehash: 2d204aeac18fc08bf6eb2221b6f64ecbbded8121
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73009999"
---
# <a name="internationalization-encodings-in-xamarinios"></a>Codificações de internacionalização no Xamarin. iOS

Nem todas as codificações são incluídas na biblioteca de classes Xamarin. iOS por padrão.

Para reduzir o tamanho do aplicativo, o Xamarin. iOS não inclui nenhuma codificação específica e você precisa instruir o mTouch a incluir os assemblies que contêm o suporte para a codificação necessária.

Isso é feito selecionando as codificações extras no painel Build/avançado do iOS no Visual Studio para Mac ou no Visual Studio:

 [![](encodings-images/00.png "Selecting the extra encodings")](encodings-images/00.png#lightbox)

 [![](encodings-images/00a.png "Selecting the extra encodings")](encodings-images/00a.png#lightbox)

Você pode selecionar um destes:

- CJK: para Chineese, japonês e coreano
- Oriente Médio: árabe, Hebraico, turco e Latin5.
- outros: cirílico, Báltico, vietnamita, ucraniano e tailandês
- raras: codificações de EBCDIC e outras páginas de código raras
- Oeste: idiomas latinos, Páscoa e Europa Ocidental
- all

 <a name="cjk" />

## <a name="cjk"></a>CJK

- CP51932
- CP932
- CP936
- CP949
- CP950
- CP54936

 <a name="mideast" />

## <a name="mideast"></a>Oriente Médio

- CP1254
- CP1255
- CP1256
- CP28596
- CP28598
- CP28599
- CP38598

 <a name="other" />

## <a name="other"></a>outras

- CP1251
- CP1257
- CP1258
- CP20866
- CP21866
- CP28594
- CP28595
- CP57002
- CP874

 <a name="rare" />

## <a name="rare"></a>encontrar

- CP1026
- CP1047
- CP1140
- CP1141
- CP1142
- CP1143
- CP1144
- CP1145
- CP1146
- CP1147
- CP1148
- CP1149
- CP20273
- CP20277
- CP20278
- CP20280
- CP20284
- CP20285
- CP20290
- CP20297
- CP20420
- CP20424
- CP20871
- CP21025
- CP37
- CP500
- CP708
- CP852
- CP855
- CP857
- CP858
- CP862
- CP864
- CP866
- CP869
- CP870
- CP875

 <a name="west" />

## <a name="west"></a>noroeste

- CP10000
- CP10079
- CP1250
- CP1252
- CP1253
- CP28592
- CP28593
- CP28597
- CP28605
- CP437
- CP850
- CP860
- CP861
- CP863
- CP865
