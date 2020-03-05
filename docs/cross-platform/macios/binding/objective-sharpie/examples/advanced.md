---
title: Exemplo avançado (manual) do mundo real
description: Este documento descreve como usar a saída de xcodebuild como a entrada para a nitidez objetiva, que fornece informações sobre qual é a nitidez do objetivo nos bastidores.
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 5e36a66949c55a85d84cbbb17fa4d276e3af1eee
ms.sourcegitcommit: acbaedbcb78bb5629d4a32e3b00f11540c93c216
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/04/2020
ms.locfileid: "78291473"
---
# <a name="advanced-manual-real-world-example"></a>Exemplo avançado (manual) do mundo real

**Este exemplo usa a [biblioteca pop do Facebook](https://github.com/facebook/pop).**

Esta seção aborda uma abordagem mais avançada de associação, onde usaremos a ferramenta de `xcodebuild` da Apple para primeiro criar o projeto POP e, em seguida, deduzir manualmente a entrada para a nitidez objetiva. Basicamente, isso aborda o que a nitidez do objetivo está fazendo nos bastidores da seção anterior.

```bash
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Como a biblioteca POP tem um projeto do Xcode (`pop.xcodeproj`), podemos usar `xcodebuild` para criar POP. Esse processo pode, por sua vez, gerar arquivos de cabeçalho que podem ser analisados pelo objetivo de nitidez. É por isso que a criação antes da associação é importante. Ao criar por meio de `xcodebuild` certifique-se de que você passe o mesmo identificador e arquitetura do SDK que pretende passar para a nitidez objetiva (e lembre-se, o objetivo de nitidez 3,0 pode fazer isso para você!):

```bash
$ xcodebuild -sdk iphoneos9.0 -arch arm64

Build settings from command line:
    ARCHS = arm64
    SDKROOT = iphoneos8.1

=== BUILD TARGET pop OF PROJECT pop WITH THE DEFAULT CONFIGURATION (Release) ===

...

CpHeader pop/POPAnimationTracer.h build/Headers/POP/POPAnimationTracer.h
    cd /Users/aaron/src/sharpie/pop
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/Users/aaron/bin::/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/usr/local/git/bin:/Users/aaron/.rvm/bin"
    builtin-copy -exclude .DS_Store -exclude CVS -exclude .svn -exclude .git -exclude .hg -strip-debug-symbols -strip-tool /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/strip -resolve-src-symlinks /Users/aaron/src/sharpie/pop/pop/POPAnimationTracer.h /Users/aaron/src/sharpie/pop/build/Headers/POP

...

** BUILD SUCCEEDED **
```

Haverá muitas saídas de informações de compilação no console como parte do `xcodebuild`. Como exibido acima, podemos ver que um destino "CpHeader" foi executado onde os arquivos de cabeçalho foram copiados para um diretório de saída de compilação. Esse geralmente é o caso e facilita a associação: como parte da compilação da biblioteca nativa, os arquivos de cabeçalho são frequentemente copiados para um local de consumo "publicamente", o que pode facilitar a análise da associação. Nesse caso, sabemos que os arquivos de cabeçalho do POP estão no diretório `build/Headers`.

Agora estamos prontos para associar o POP. Sabemos que desejamos criar para o SDK `iphoneos8.1` com a arquitetura de `arm64` e que os arquivos de cabeçalho nos quais nos preocupamos estão em `build/Headers` na retirada do git POP. Se olharmos no diretório `build/Headers`, veremos vários arquivos de cabeçalho:

```bash
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

Se olharmos `POP.h`, podemos ver que ele é o principal arquivo de cabeçalho de nível superior da biblioteca que `#import`s outros arquivos. Por isso, só precisamos passar `POP.h` para a nitidez objetiva, e Clang fará o restante nos bastidores:

```bash
$ sharpie bind -output Binding -sdk iphoneos8.1 \
    -scope build/Headers build/Headers/POP/POP.h \
    -c -Ibuild/Headers -arch arm64

Parsing Native Code...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Binding Analysis:
  Automated binding is complete, but there are a few APIs which have
  been flagged with [Verify] attributes. While the entire binding
  should be audited for best API design practices, look more closely
  at APIs with the following Verify attribute hints:

  ConstantsInterfaceAssociation (1 instance):
    There's no fool-proof way to determine with which Objective-C
    interface an extern variable declaration may be associated.
    Instances of these are bound as [Field] properties in a partial
    interface into a near-by concrete interface to produce a more
    intuitive API, possibly eliminating the 'Constants' interface
    altogether.

  StronglyTypedNSArray (4 instances):
    A native NSArray* was bound as NSObject[]. It might be possible
    to more strongly type the array in the binding based on
    expectations set through API documentation (e.g. comments in the
    header file) or by examining the array contents through testing.
    For example, an NSArray* containing only NSNumber* instances can
    be bound as NSNumber[] instead of NSObject[].

  MethodToProperty (2 instances):
    An Objective-C method was bound as a C# property due to
    convention such as taking no parameters and returning a value (
    non-void return). Often methods like these should be bound as
    properties to surface a nicer API, but sometimes false-positives
    can occur and the binding should actually be a method.

  Once you have verified a Verify attribute, you should remove it
  from the binding source code. The presence of Verify attributes
  intentionally cause build failures.

  For more information about the Verify attribute hints above,
  consult the Objective Sharpie documentation by running 'sharpie
  docs' or visiting the following URL:

    http://xmn.io/sharpie-docs

Submitting usage data to Xamarin...
  Submitted - thank you for helping to improve Objective Sharpie!

Done.
```

Você observará que passamos um argumento `-scope build/Headers` para a nitidez objetiva. Como as bibliotecas C e Objective-C devem `#import` ou `#include` outros arquivos de cabeçalho que são detalhes de implementação da biblioteca e não a API que você deseja associar, o argumento `-scope` informa a nitidez do objetivo para ignorar qualquer API que não esteja definida em um arquivo em algum lugar dentro do diretório `-scope`.

Você encontrará o argumento de `-scope` geralmente é opcional para bibliotecas implementadas com clareza, no entanto, não há nenhum dano no fornecimento explícito.

Além disso, especificamos `-c -Ibuild/headers`. Em primeiro lugar, o argumento `-c` informa a nitidez do objetivo para interromper a interpretação de argumentos de linha de comando e passar quaisquer argumentos subsequentes _diretamente para o compilador Clang_. Portanto, `-Ibuild/Headers` é um argumento de compilador Clang que instrui o Clang a procurar por inclusões em `build/Headers`, que é onde os cabeçalhos POP residem. Sem esse argumento, Clang não saberá onde localizar os arquivos que `POP.h` está `#import`ing. _Quase todos os "problemas" com o uso de nitidez objetiva se resumem para descobrir o que passar para o Clang_.

### <a name="completing-the-binding"></a>Concluindo a associação

A Sharpde objetiva agora gerou arquivos `Binding/ApiDefinitions.cs` e `Binding/StructsAndEnums.cs`.

Essas são as primeiras passagens básicas do objetivo inicial da ligação e, em alguns casos, pode ser tudo o que você precisa. Como mencionado acima, no entanto, o desenvolvedor geralmente precisará modificar manualmente os arquivos gerados após a conclusão do objetivo de ajuste de nitidez para [corrigir quaisquer problemas](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) que não puderam ser manipulados automaticamente pela ferramenta.

Depois que as atualizações forem concluídas, esses dois arquivos agora poderão ser adicionados a um projeto de associação no Visual Studio para Mac ou serem passados diretamente para as ferramentas `btouch` ou `bmac` para produzir a associação final.

Para obter uma descrição completa do processo de associação, consulte nossas [instruções completas sobre explicação](~/ios/platform/binding-objective-c/walkthrough.md).
