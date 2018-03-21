---
title: Compilation du code XAML
description: "XAML peut être éventuellement compilé directement en langage intermédiaire (IL) avec le compilateur XAML (XAMLC)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/21/2016
ms.openlocfilehash: 3ce0fd2b83599fca7f38c161bbd2137254b75bd5
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2018
---
# <a name="xaml-compilation"></a>Compilation du code XAML

_XAML peut être éventuellement compilé directement en langage intermédiaire (IL) avec le compilateur XAML (XAMLC)._

XAMLC offre un certain nombre d’avantages :

- Il effectue une vérification au moment de la compilation du code XAML et informe l’utilisateur des erreurs rencontrées.
- Il supprime une partie du temps de chargement et d’instanciation pour les éléments XAML.
- Il permet de réduire la taille de fichier de l’assembly final en n’incluant plus les fichiers .xaml.

XAMLC est désactivé par défaut pour assurer la compatibilité descendante. Elle peut être activée au niveau de l’assembly et la classe en ajoutant le `XamlCompilation` attribut.

L’exemple de code suivant illustre l’activation de XAMLC au niveau de l’assembly :

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

Dans cet exemple, la vérification de la compilation de tout le code XAML contenus dans le `PhotoApp` espace de noms sera effectuée avec des erreurs XAML sont signalés au moment de la compilation plutôt que d’exécution.
Le `assembly` de préfixe pour le `XamlCompilation` attribut spécifie que l’attribut s’applique à la totalité de l’assembly.

L’exemple de code suivant illustre l’activation de XAMLC au niveau de la classe :

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

Dans cet exemple, lors de la compilation du code XAML pour la vérification de la `HomePage` classe sera effectuée et les erreurs signalées en tant que partie du processus de compilation.

> [!NOTE]
> Le `XamlCompilation` attribut et la `XamlCompilationOptions` énumération se trouvent dans le `Xamarin.Forms.Xaml` espace de noms qui doit être importé pour pouvoir les utiliser.


## <a name="related-links"></a>Liens associés

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
