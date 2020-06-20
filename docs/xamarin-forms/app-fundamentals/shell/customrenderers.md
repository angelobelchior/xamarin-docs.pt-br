---
title: Xamarin.FormsRenderizadores personalizados do Shell
description: Xamarin.FormsOs aplicativos shell são altamente personalizáveis por meio das propriedades e métodos que as várias classes Shell expõem. No entanto, também será possível criar um renderizador personalizado do Shell quando personalizações específicas de plataforma mais sofisticadas forem necessárias.
ms.prod: xamarin
ms.assetid: 3B1A6AE8-1D1E-4C34-B9AB-48F4444FEF32
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 321539f877a86804245d27a2d76d1edeb1abd1e9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137781"
---
# <a name="xamarinforms-shell-custom-renderers"></a>Xamarin.FormsRenderizadores personalizados do Shell

Uma das vantagens dos Xamarin.Forms aplicativos Shell é que sua aparência e comportamento é altamente personalizável por meio das propriedades e dos métodos que as várias classes Shell expõem. No entanto, também será possível criar um renderizador personalizado do Shell quando personalizações específicas de plataforma mais sofisticadas forem necessárias. Assim como acontece com outros renderizadores personalizados, um renderizador personalizado do Shell pode ser adicionado a apenas um projeto de plataforma para personalizar a aparência e o comportamento, permitindo o comportamento padrão na outra plataforma; ou um renderizador personalizado do Shell diferente pode ser adicionado a cada projeto de plataforma para personalizar a aparência e o comportamento no iOS e no Android.

Os aplicativos do Shell são renderizados usando a classe `ShellRenderer` no iOS e no Android. No iOS, a classe `ShellRenderer` pode ser encontrada no namespace `Xamarin.Forms.Platform.iOS`. No Android, a classe `ShellRenderer` pode ser encontrada no namespace `Xamarin.Forms.Platform.Android`.

O processo para criar um renderizador personalizado do Shell é o seguinte:

1. Crie uma subclasse da classe `Shell`. Isso já será realizado no seu aplicativo do Shell.
1. Consuma a classe `Shell` com a subclasse. Isso já será realizado no seu aplicativo do Shell.
1. Crie uma classe de renderizador personalizado que derive da classe `ShellRenderer` nas plataformas necessárias.

## <a name="create-a-custom-renderer-class"></a>Criar uma classe de renderizador personalizado

O processo para criar uma classe de renderizador personalizado é a seguinte:

1. Crie uma subclasse da classe `ShellRenderer`.
1. Substitua os métodos necessários para realizar a personalização necessária.
1. Adicione uma `ExportRendererAttribute` à subclasse `ShellRenderer` para especificar que ela será usada para renderizar o aplicativo do Shell. Esse atributo é usado para registrar o renderizador personalizado com Xamarin.Forms .

> [!NOTE]
> O fornecimento de um renderizador personalizado do Shell em cada projeto de plataforma é opcional. Se um renderizador personalizado não estiver registrado, então a classe `ShellRenderer` será usada.

A classe `ShellRenderer` expõe os seguintes métodos substituíveis:

| iOS | Android |
| --- | --- |
| `SetElementSize`<br />`CreateFlyoutRenderer`<br />`CreateNavBarAppearanceTracker`<br />`CreatePageRendererTracker`<br />`CreateShellFlyoutContentRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellItemTransition`<br />`CreateShellSearchResultsRenderer`<br />`CreateShellSectionRenderer`<br />`CreateTabBarAppearanceTracker`<br />`Dispose`<br />`OnCurrentItemChanged`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`UpdateBackgroundColor` | `CreateFragmentForPage`<br />`CreateShellFlyoutContentRenderer`<br />`CreateShellFlyoutRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellSectionRenderer`<br />`CreateTrackerForToolbar`<br />`CreateToolbarAppearanceTracker`<br />`CreateTabLayoutAppearanceTracker`<br />`CreateBottomNavViewAppearanceTracker`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`SwitchFragment`<br />`Dispose` |

As classes `FlyoutItem` e `TabBar` são aliases da classe `ShellItem`, e a classe `Tab` é um alias da classe `ShellSection`. Portanto, o método `CreateShellItemRenderer` deve ser substituído ao criar um renderizador personalizado para objetos `FlyoutItem`, e o método `CreateShellSectionRenderer` deve ser substituído ao criar um renderizador personalizado para objetos `Tab`.

> [!IMPORTANT]
> Há classes de renderizador do Shell adicionais, como `ShellSectionRenderer` e `ShellItemRenderer`, no iOS e no Android. No entanto, elas são criadas por substituições na classe `ShellRenderer`. Portanto, a personalização do comportamento dessas classes de renderizador adicionais pode ser obtida por meio da criação de subclasses, e a criação de uma instância da subclasse na substituição apropriada na classe `ShellRenderer` com subclasse.

### <a name="ios-example"></a>Exemplo de iOS

O exemplo de código a seguir mostra uma `ShellRenderer` com subclasse para iOS, que define uma imagem de plano de fundo na barra de navegação do aplicativo de Shell:

```csharp
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(Xaminals.AppShell), typeof(Xaminals.iOS.MyShellRenderer))]
namespace Xaminals.iOS
{
    public class MyShellRenderer : ShellRenderer
    {
        protected override IShellSectionRenderer CreateShellSectionRenderer(ShellSection shellSection)
        {
            var renderer = base.CreateShellSectionRenderer(shellSection);
            if (renderer != null)
            {
                (renderer as ShellSectionRenderer).NavigationBar.SetBackgroundImage(UIImage.FromFile("monkey.png"), UIBarMetrics.Default);
            }
            return renderer;
        }
    }
}
```

A classe `MyShellRenderer` substitui o método `CreateShellSectionRenderer` e recupera o renderizador criado pela classe base. Ela, em seguida, modifica o renderizador ao definir uma imagem de plano de fundo na barra de navegação antes de retornar o renderizador.

### <a name="android-example"></a>Exemplo do Android

O exemplo de código a seguir mostra uma `ShellRenderer` com subclasse para Android, que define uma imagem de plano de fundo na barra de navegação do aplicativo do Shell:

```csharp
using Android.Content;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(Xaminals.AppShell), typeof(Xaminals.Droid.MyShellRenderer))]
namespace Xaminals.Droid
{
    public class MyShellRenderer : ShellRenderer
    {
        public MyShellRenderer(Context context) : base(context)
        {
        }

        protected override IShellToolbarAppearanceTracker CreateToolbarAppearanceTracker()
        {
            return new MyShellToolbarAppearanceTracker(this);
        }
    }
}
```

A classe `MyShellRenderer` substitui o método `CreateToolbarAppearanceTracker` e retorna uma instância da classe `MyShellToolbarAppearanceTracker`. A classe `MyShellToolbarAppearanceTracker`, que deriva da classe `ShellToolbarAppearanceTracker`, é mostrada no exemplo a seguir:

```csharp
using Android.Support.V7.Widget;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

namespace Xaminals.Droid
{
    public class MyShellToolbarAppearanceTracker : ShellToolbarAppearanceTracker
    {
        public MyShellToolbarAppearanceTracker(IShellContext context) : base(context)
        {
        }

        public override void SetAppearance(Toolbar toolbar, IShellToolbarTracker toolbarTracker, ShellAppearance appearance)
        {
            base.SetAppearance(toolbar, toolbarTracker, appearance);
            toolbar.SetBackgroundResource(Resource.Drawable.monkey);
        }
    }
}
```

A classe `MyShellToolbarAppearanceTracker` substitui o método `SetAppearance` e modifica a barra de ferramentas ao definir uma imagem de plano de fundo nela.

> [!IMPORTANT]
> Só é necessário adicionar `ExportRendererAttribute` a um renderizador personalizado que derive da classe `ShellRenderer`. As classes de renderizador do Shell com subclasse adicionais são criadas pela classe `ShellRenderer` com subclasse.

## <a name="related-links"></a>Links relacionados

- [Xamarin.FormsRenderizadores personalizados](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
