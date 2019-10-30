---
title: Noções básicas sobre o recurso do Android
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/01/2018
ms.openlocfilehash: ac228e6f0c251ae6f0fcabe1be855c6ed4a85d35
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025354"
---
# <a name="android-resource-basics"></a>Noções básicas sobre o recurso do Android

Quase todos os aplicativos Android terão algum tipo de recurso neles; no mínimo, eles geralmente têm os layouts da interface do usuário na forma de arquivos XML. Quando um aplicativo Xamarin. Android é criado pela primeira vez, os recursos padrão são configurados pelo modelo de projeto Xamarin. Android:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Arquivos de recurso](android-resource-basics-images/01-resource-files-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio para Mac](#tab/macos)

![Arquivos de recurso](android-resource-basics-images/01-resource-files-xs.png)

-----

Os cinco arquivos que compõem os recursos padrão foram criados na pasta de recursos:

- **Icon. png** &ndash; o ícone padrão para o aplicativo

- **Main. axml** &ndash; o arquivo de layout da interface do usuário padrão para um aplicativo. Observe que, embora o Android use a extensão de arquivo **. xml** , o Xamarin. Android usa a extensão de arquivo **. axml** .

- String **. xml** &ndash; uma tabela de cadeia de caracteres para ajudar na localização do aplicativo

- **AboutResources. txt** &ndash; isso não é necessário e pode ser excluído com segurança. Ele fornece apenas uma visão geral de alto nível da pasta de recursos e dos arquivos nela.

- **Resource.designer.cs** &ndash; esse arquivo é gerado automaticamente e mantido pelo Xamarin. Android e mantém a ID exclusiva atribuída a cada recurso. Isso é muito semelhante e idêntico à finalidade do arquivo R. Java que um aplicativo Android escrito em Java teria. Ele é criado automaticamente pelas ferramentas Xamarin. Android e será regenerado de tempos em tempos.

## <a name="creating-and-accessing-resources"></a>Criando e acessando recursos

A criação de recursos é tão simples quanto adicionar arquivos ao diretório para o tipo de recurso em questão. A captura de tela abaixo mostra recursos de cadeia de caracteres para localidades alemãs foram adicionadas a um projeto. Quando o **Strings. xml** foi adicionado ao arquivo, a **ação de compilação** foi definida automaticamente como **AndroidResource** pelas ferramentas Xamarin. Android:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Ação de Build para cadeias de caracteres. xml definida como AndroidResource](android-resource-basics-images/02-build-action-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio para Mac](#tab/macos)

![Ação de Build para cadeias de caracteres. xml definida como AndroidResource](android-resource-basics-images/02-build-action-xs.png)

-----

Isso permite que as ferramentas do Xamarin. Android compilem e insiram corretamente os recursos no arquivo APK. Se por algum motivo a **ação de compilação** não estiver definida como **recurso Android**, os arquivos serão excluídos do APK e qualquer tentativa de carregar ou acessar os recursos resultará em um erro de tempo de execução e o aplicativo falhará.

Além disso, é importante observar que, embora o Android dê suporte apenas a nomes de fileminúsculas para itens de recursos, o Xamarin. Android é um pouco mais tolerante; Ele dará suporte a nomes de filemaiúsculos e minúsculos. A Convenção de nomes de imagem é usar letras minúsculas com sublinhados como separadores (por exemplo, **minha imagem de\_\_Name. png**). Observe que os nomes de recursos não poderão ser processados se forem usados traços ou espaços como separadores.

Depois que os recursos tiverem sido adicionados a um projeto, há duas maneiras de usá-los em um aplicativo &ndash; de forma programática (dentro do código) ou de arquivos XML.

## <a name="referencing-resources-programmatically"></a>Referenciando recursos programaticamente

Para acessar esses arquivos programaticamente, eles recebem uma ID de recurso exclusiva. Essa ID de recurso é um inteiro definido em uma classe especial chamada `Resource`, que é encontrado no arquivo **Resource.designer.cs**e tem uma aparência semelhante a esta:

```csharp
public partial class Resource
{
    public partial class Attribute
    {
    }
    public partial class Drawable {
        public const int Icon=0x7f020000;
    }
    public partial class Id
    {
        public const int Textview=0x7f050000;
    }
    public partial class Layout
    {
        public const int Main=0x7f030000;
    }
    public partial class String
    {
        public const int App_Name=0x7f040001;
        public const int Hello=0x7f040000;
    }
}
```

Cada ID de recurso está contida dentro de uma classe aninhada que corresponde ao tipo de recurso. Por exemplo, quando o arquivo **Icon. png** foi adicionado ao projeto, o Xamarin. Android atualizou a classe `Resource`, criando uma classe aninhada chamada `Drawable` com uma constante dentro de `Icon`nomeada.
Isso permite que o arquivo **Icon. png** seja referenciado no código como `Resource.Drawable.Icon`. A classe `Resource` não deve ser editada manualmente, pois quaisquer alterações feitas nela serão substituídas pelo Xamarin. Android.

Ao referenciar recursos programaticamente (no código), eles podem ser acessados por meio da hierarquia de classes de recursos que usa a seguinte sintaxe:

```csharp
[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

- **Packagename** &ndash; pacote que está fornecendo o recurso e só é necessário quando recursos de outros pacotes estão sendo usados.

- **ResourceType** &ndash; esse é o tipo de recurso aninhado que está dentro da classe de recurso descrita acima.

- O **nome do recurso** &ndash; este é o nome do arquivo do recurso (sem a extensão) ou o valor do atributo Android: Name para recursos que estão em um elemento XML.

## <a name="referencing-resources-from-xml"></a>Referenciando recursos de XML

Os recursos em um arquivo XML são acessados por uma sintaxe especial a seguir:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>
```

- **Packagename** &ndash; pacote que está fornecendo o recurso e só é necessário quando recursos de outros pacotes estão sendo usados.

- **ResourceType** &ndash; esse é o tipo de recurso aninhado que está dentro da classe de recurso.

- O **nome do recurso** &ndash; este é o nome do arquivo do recurso (*sem* a extensão de tipo de arquivo) ou o valor do atributo `android:name` para recursos que estão em um elemento XML.

Por exemplo, o conteúdo de um arquivo de layout, **Main. axml**, é o seguinte:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent">
    <ImageView android:id="@+id/myImage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/flag" />
</LinearLayout>
```

Este exemplo tem um [`ImageView`](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/imageview) que requer um recurso com nome desenhado chamado **sinalizador**. O `ImageView` tem seu atributo `src` definido como `@drawable/flag`. Quando a atividade for iniciada, o Android procurará dentro do recurso de diretório **/empate** para um arquivo chamado **Flag. png** (a extensão de arquivo pode ser outro formato de imagem, como **Flag. jpg**) e carregar esse arquivo e exibi-lo no `ImageView`.
Quando esse aplicativo é executado, ele seria semelhante à imagem a seguir:

![ImageView localizada](android-resource-basics-images/03-localized-screenshot.png)
