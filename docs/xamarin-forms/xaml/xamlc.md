---
title: Compilation XAML dans Xamarin.Forms
description: Cet article explique comment XAML peut être éventuellement compilé directement en langage intermédiaire (IL) avec le compilateur XAML de Xamarin.Forms (XAMLC).
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2018
ms.openlocfilehash: 9567f3ad8d748a94a03cd1c86254072d4ba3bbdc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075118"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Compilation XAML dans Xamarin.Forms

_XAML peut être éventuellement compilé directement en langage intermédiaire (IL) avec le compilateur XAML (XAMLC)._

Compilation de XAML offre un nombre d’avantages :

- Il effectue une vérification au moment de la compilation du code XAML et informe l’utilisateur des erreurs rencontrées.
- Il supprime une partie du temps de chargement et d’instanciation pour les éléments XAML.
- Il permet de réduire la taille de fichier de l’assembly final en n’incluant plus les fichiers .xaml.

Compilation de XAML est désactivée par défaut pour assurer la compatibilité descendante. Il peut être activé au niveau de l’assembly et la classe en ajoutant le [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) attribut.

L’exemple de code suivant illustre la compilation XAML activation au niveau de l’assembly :

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

Dans cet exemple, la vérification de tous les XAML contenue dans l’assembly lors de la compilation se fera, avec des erreurs XAML signalés au moment de la compilation plutôt que d’exécution. Par conséquent, le `assembly` de préfixe pour le [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) attribut spécifie que l’attribut s’applique à la totalité de l’assembly.

> [!NOTE]
> Le [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) attribut et la [ `XamlCompilationOptions` ](xref:Xamarin.Forms.Xaml.XamlCompilationOptions) énumération se trouvent dans le `Xamarin.Forms.Xaml` espace de noms, qui doit être importé à les utiliser.

L’exemple de code suivant illustre la compilation XAML activation au niveau de la classe :

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

Dans cet exemple, lors de la compilation la vérification de le XAML pour la `HomePage` classe sera effectuée et erreurs signalés comme faisant partie du processus de compilation.

> [!NOTE]
> Liaisons compilées peuvent être activées pour améliorer les performances de liaison de données dans les applications Xamarin.Forms. Pour plus d’informations, consultez [liaisons compilé](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

## <a name="related-links"></a>Liens associés

- [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
