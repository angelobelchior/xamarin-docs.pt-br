---
title: SceneKit no Xamarin. iOS
description: Este documento descreve o SceneKit, uma API de grafo de cena 3D que simplifica o trabalho com gráficos 3D, abstraindo as complexidades do OpenGL.
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: 5279effa83a8784f6d475188e67a535f7b5e1262
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032497"
---
# <a name="scenekit-in-xamarinios"></a>SceneKit no Xamarin. iOS

SceneKit é uma API de grafo de cena 3D que simplifica o trabalho com gráficos 3D. Ele foi introduzido pela primeira vez no OS X 10,8 e chegou ao iOS 8. Com o SceneKit, a criação de visualizações 3D de imersão e jogos 3D casuais não requer conhecimento em OpenGL. Aproveitando os conceitos comuns de grafo de cena, o SceneKit abstrai as complexidades do OpenGL e do OpenGL ES, facilitando muito a adição de conteúdo 3D a um aplicativo. No entanto, se você for um especialista em OpenGL, o SceneKit também terá excelente suporte para ligar diretamente com o OpenGL. Ele também inclui vários recursos que complementam gráficos 3D, como a física, e se integram muito bem com várias outras estruturas da Apple, como animação principal, imagem básica e kit de Sprite.

SceneKit é extremamente fácil de trabalhar com. É uma API declarativa que cuida da renderização. Basta configurar uma cena, adicionar propriedades a ela e o SceneKit lida com a renderização da cena.

Para trabalhar com o SceneKit, você cria um grafo de cena usando a classe `SCNScene`. Uma cena contém uma hierarquia de nós, representada por instâncias de `SCNNode`, definindo locais no espaço 3D. Cada nó tem propriedades como geometria, iluminação e materiais que afetam sua aparência, conforme ilustrado na figura a seguir:

![](scenekit-images/image7.png "The SceneKit hierarchy")

## <a name="create-a-scene"></a>Criar uma cena

Para fazer uma cena aparecer na tela, adicione-a a uma `SCNView` atribuindo-a à propriedade de cena da exibição. Além disso, se você fizer alterações na cena, `SCNView` será atualizada para exibir as alterações.

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

Os bastidores podem ser preenchidos de arquivos exportados por meio de uma ferramenta de modelagem 3D ou programaticamente de primitivos geométricos. Por exemplo, é como criar uma esfera e adicioná-la à cena:

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>Adicionando luz

Neste ponto, a esfera não exibirá nada porque não há luz na cena. Anexar instâncias de `SCNLight` a nós cria luzes em SceneKit. Há vários tipos de luzes que variam de várias formas de iluminação direcional até a iluminação de ambiente. Por exemplo, o código a seguir cria uma luz onidirecionais no lado da esfera:

```csharp
// omnidirectional light
var light = SCNLight.Create ();
var lightNode = SCNNode.Create ();
light.LightType = SCNLightType.Omni;
light.Color = UIColor.Blue;
lightNode.Light = light;
lightNode.Position = new SCNVector3 (-40, 40, 60);
scene.RootNode.AddChildNode (lightNode);
```

A iluminação onidirecionais produz uma reflexão difusa, resultando em uma iluminação uniforme, um tipo como iluminar uma lanterna. A criação de luz ambiente é semelhante, embora não tenha nenhuma direção, pois ela se destaca igualmente em todas as direções. Imagine-o como iluminação de humor:)

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

Com as luzes em vigor, a esfera agora está visível na cena.

![](scenekit-images/image8.png "The sphere is visible in the scene when lit")

## <a name="adding-a-camera"></a>Adicionando uma câmera

Adicionar uma câmera (SCNCamera) à cena altera o ponto de vista. O padrão para adicionar a câmera é semelhante. Crie a câmera, anexe-a a um nó e adicione o nó à cena.

```csharp
// camera
camera = new SCNCamera {
    XFov = 80,
    YFov = 80
};
cameraNode = new SCNNode {
    Camera = camera,
    Position = new SCNVector3 (0, 0, 40)
};
scene.RootNode.AddChildNode (cameraNode);
```

Como você pode ver no código acima, os objetos SceneKit podem ser criados usando construtores ou do método Create Factory. O primeiro permite usar C# a sintaxe do inicializador, mas qual delas usar é basicamente uma questão de preferência.

Com a câmera em vigor, toda a esfera é visível para o usuário:

![](scenekit-images/image9.png "The entire sphere is visible to the user")

Você também pode adicionar mais luzes à cena. Aqui está o que ele parece com algumas luzes mais onidirecionaiss:

![](scenekit-images/image10.png "The sphere with a few more omnidirectional lights")

Além disso, ao definir `sceneView.AllowsCameraControl = true`, o usuário pode alterar o ponto de vista com um gesto de toque.

### <a name="materials"></a>Materiais

Os materiais são criados com a classe SCNMaterial. Por exemplo, para adicionar uma imagem na superfície da esfera, defina a imagem como o conteúdo *difuso* do material.

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

Isso camadas a imagem no nó, como mostrado abaixo:

![](scenekit-images/image11.png "Layering the image onto the sphere")

Um material também pode ser definido para responder a outros tipos de iluminação. Por exemplo, o objeto pode ser tornado brilhante e ter seu conteúdo especulativo definido para exibir a reflexão especular, resultando em um ponto brilhante na superfície, como mostrado abaixo:

![](scenekit-images/image12.png "The object made shiny with specular reflection, resulting in a bright spot on the surface")

Os materiais são muito flexíveis, permitindo que você atinja muito pouco código. Por exemplo, em vez de definir a imagem para o conteúdo difuso, defina-a como o conteúdo reflexivo.

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

Agora, o macaco parece sentar visualmente dentro da esfera, independentemente do ponto de vista.

### <a name="animation"></a>Animação

O SceneKit foi projetado para funcionar bem com a animação. Você pode criar animações implícitas ou explícitas e até mesmo renderizar uma cena de uma árvore de camada de animação de núcleo. Ao criar uma animação implícita, o SceneKit fornece sua própria classe de transição, `SCNTransaction`.

Aqui está um exemplo que gira a esfera:

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

No entanto, é possível animar muito mais do que rotação. Muitas propriedades de SceneKit são animáveis. Por exemplo, o código a seguir anima o `Shininess` do material para aumentar a reflexão especular.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

O SceneKit é muito simples de usar. Ele oferece uma infinidade de recursos adicionais, incluindo restrições, física, ações declarativas, texto 3D, profundidade do suporte a campo, integração do kit Sprite e integração da imagem principal para citar apenas alguns.
