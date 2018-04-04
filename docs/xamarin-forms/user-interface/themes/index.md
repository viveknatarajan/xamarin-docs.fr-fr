---
title: Thèmes
ms.prod: xamarin
ms.assetid: 3DFB7C55-69F6-4980-A501-588719143482
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: c128e6130f0d8c2ba076a82ac85bda5aadea1ee9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="themes"></a>Thèmes

![](~/media/shared/preview.png "Cette API méthode est actuellement en version préliminaire")

Xamarin.Forms thèmes ont été annoncées lors Evolve 2016 et sont disponibles en version préliminaire pour les clients de tenter d’envoyer des commentaires.

Un thème est ajouté à une application de Xamarin.Forms par le **Xamarin.Forms.Theme.Base** du package Nuget, ainsi que d’un package supplémentaire qui définit un thème spécifique (par exemple). Xamarin.Forms.Theme.Light) ou sans quoi un thème local peut être défini pour l’application.

Reportez-vous à la [le thème clair](light.md) et [le thème sombre](dark.md) pages pour obtenir des instructions sur la façon de les ajouter à une application, ou extraire les [exemple de thème personnalisé](custom.md).

**IMPORTANT :** vous devez également suivre les étapes permettant de [charger des assemblys du thème (ci-dessous)](#loadtheme) en ajoutant du code réutilisable pour les e/s `AppDelegate` et Android `MainActivity`. Cela améliorera en version préliminaire de futures.


## <a name="control-appearance"></a>Contrôler l’apparence

Le [Light](light.md) et [foncé](dark.md) thèmes à la fois définissent une apparence visuelle spécifique pour les contrôles standards. Après avoir ajouté un thème au dictionnaire de ressources de l’application, l’apparence des contrôles standard change.

Le balisage XAML suivant montre les contrôles communs :

```xaml
<StackLayout Padding="40">
    <Label Text="Regular label" />
    <Entry Placeholder="type here" />
    <Button Text="OK" />
    <BoxView Color="Yellow" />
    <Switch />
</StackLayout>
```

Ces captures d’écran affichent ces contrôles avec :

* Aucun thème appliqué
* Thème clair (uniquement des différences subtiles à ne disposer d’aucun thème)
* Thème foncé

![](images/standard-none-sml.png "Les contrôles sans thème") ![ ] (images/standard-light-sml.png "contrôles avec le thème clair") ![ ] (images/standard-dark-sml.png "contrôles avec le thème sombre")

<a name="styleclass" />

## <a name="styleclass"></a>StyleClass

Le `StyleClass` propriété permet l’apparence d’une vue à modifier en fonction d’une définition fournie par un thème.

Le [Light](light.md) et [foncé](dark.md) thèmes à la fois définissent trois différentes apparences pour un `BoxView`: `HorizontalRule`, `Circle`, et `Rounded`. Ce balisage illustre trois différents `BoxView`s avec les classes de style différent appliqué :

```xaml
<StackLayout Padding="40">
    <BoxView StyleClass="HorizontalRule" />
    <BoxView StyleClass="Circle" />
    <BoxView StyleClass="Rounded" />
</StackLayout>
```

Cela restitue avec clair et foncé comme suit :

![](images/boxview-light-sml.png "BoxView avec un StyleClass le thème clair") ![ ] (images/boxview-dark-sml.png "BoxView avec un StyleClass le thème sombre")

<a name="builtin" />

## <a name="built-in-classes"></a>Classes intégrées

En plus des styles automatiquement les contrôles communs la lumière et thèmes foncés prend actuellement en charge les classes suivantes qui peuvent être appliquées en définissant le `StyleClass` sur ces contrôles :

**BoxView**

* HorizontalRule
* Cercle
* Arrondi

**Image**

* Cercle
* Arrondi
* Miniature

**Button**

* Par défaut
* Principale
* Opération réussie
* Info
* Warning
* Danger
* Lien
* Petit
* Grand

**Label**

* Header
* En-tête secondaire
* Corps
* Lien
* Inverse


## <a name="troubleshooting"></a>Résolution des problèmes

<a name="loadtheme" />

### <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Impossible de charger fichier ou l’assembly 'Xamarin.Forms.Theme.Light' ou une de ses dépendances

Dans la version préliminaire, thèmes n’est peut-être pas en mesure de charger lors de l’exécution. Ajoutez le code indiqué ci-dessous dans les projets pour corriger cette erreur.

**iOS**

Dans le **AppDelegate.cs** ajoutez les lignes suivantes après `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

Dans le **MainActivity.cs** ajoutez les lignes suivantes après `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```


## <a name="related-links"></a>Liens associés

- [Exemple de ThemesDemo](https://github.com/xamarin/xamarin-forms-samples/tree/master/Themes/ThemesDemo)
