---
title: Nouveaux Styles de l’interface utilisateur
description: Cet article traite de la lumière et thèmes de l’interface utilisateur sombre que Apple a ajouté à tvOS 10 et comment les implémenter dans une application Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: da75a99e842b13d42251cdd1c5195ec66ff4a513
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="new-user-interface-styles"></a>Nouveaux Styles de l’interface utilisateur

_Cet article traite de la lumière et thèmes de l’interface utilisateur sombre que Apple a ajouté à tvOS 10 et comment les implémenter dans une application Xamarin.tvOS._

tvOS 10 maintenant prend en charge un thème sombre et l’Interface utilisateur de lumière thème que tous le UIKit build dans les contrôles seront automatiquement adapter, en fonction des préférences de l’utilisateur. En outre, le développeur peut ajuster manuellement les éléments d’interface utilisateur en fonction du thème de l’utilisateur a sélectionné et peut substituer un thème donné.


<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>Sur les nouveaux Styles d’Interface utilisateur

Comme indiqué ci-dessus, tvOS 10 maintenant prend en charge un thème sombre et l’Interface utilisateur de lumière thème que tous le UIKit build dans les contrôles seront automatiquement adapter, en fonction des préférences de l’utilisateur.

L’utilisateur peut basculer ce thème en accédant à **paramètres** > **général** > **apparence** et commutation entre **clair**  et **foncé**:

[![](user-interface-styles-images/theme01.png "L’application de paramètres")](user-interface-styles-images/theme01.png#lightbox)

Lorsque le **foncé** thème est sélectionné, tous les éléments d’Interface utilisateur passe en texte clair sur un arrière-plan foncé :

[![](user-interface-styles-images/theme02.png "Le thème sombre")](user-interface-styles-images/theme02.png#lightbox)

L’utilisateur a la possibilité de basculer le thème à tout moment et peut le faire donc basé sur l’activité en cours, où se trouve l’Apple TV ou l’heure du jour.

Le thème de l’interface utilisateur de lumière est le thème par défaut, et toutes les applications tvOS existant continuera d’utiliser le thème clair, indépendamment des préférences de l’utilisateur, sauf si elles sont modifiées de tvOS 10 afin de tirer parti du thème foncé. Une application tvOS 10 a également la possibilité de remplacer le thème actuel et de toujours utiliser le thème sombre ou clair pour tout ou partie de son interface utilisateur.

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>Adopter les thèmes clairs et foncés

Pour prendre en charge cette fonctionnalité, Apple a ajouté une nouvelle API pour le `UITraitCollection` classe et une application tvOS doivent participer pour prendre en charge l’apparence foncé (via un paramètre dans son `Info.plist` fichier).

Pour participer à la prise en charge de thème clairs et foncés, procédez comme suit :

1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Info.plist` pour l’ouvrir et le modifier.
2. Sélectionnez le **Source** vue (du bas de l’éditeur).
3. Ajoutez une nouvelle clé et l’appeler `UIUserInterfaceStyle`: 

    [![](user-interface-styles-images/theme03.png "La clé UIUserInterfaceStyle")](user-interface-styles-images/theme03.png#lightbox)
4. Laissez le type défini sur `String` et entrez la valeur `Automatic`: 

    [![](user-interface-styles-images/theme04.png "Entrez automatique")](user-interface-styles-images/theme04.png#lightbox)
5. Enregistrez les modifications dans le fichier.

Il existe trois valeurs possibles pour le `UIUserInterfaceStyle` clé :

- **Lumière** -force l’interface utilisateur de l’application tvOS à toujours utiliser ce thème clair.
- **Sombre** -force l’interface utilisateur de l’application tvOS à toujours utiliser le thème sombre.
- **Automatique** -bascule entre le thème clairs et foncés en fonction des préférences de l’utilisateur dans les paramètres. Il s’agit du paramètre recommandé.

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>Prise en charge le thème UIKit

Si une application de tvOS est à l’aide intégrée, standard, `UIView` en fonction des contrôles, ils doit répondre automatiquement le thème de l’interface utilisateur sans aucune intervention du développeur.

En outre, `UILabel` et `UITextView` change automatiquement leur couleur en fonction du thème de l’interface utilisateur de sélectionner :

- Le texte sera noir dans le thème clair.
- Le texte sera blanc dans le thème sombre.

Si le développeur change la couleur du texte manuellement (soit dans le code ou la table de montage séquentiel), ils seront chargés de gérer les modifications de couleur en fonction du thème de l’interface utilisateur.

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>Nouveaux effets Flou

Pour prendre en charge les thèmes clairs et foncés dans une application tvOS 10, Apple a ajouté deux nouveaux effets flou. Ces nouveaux effets ajuste automatiquement l’effet de flou selon le thème de l’interface utilisateur que l’utilisateur a sélectionné comme suit :

- `UIBlurEffectStyleRegular` : Utilise une lumière flou dans le thème clair et un thème sombre flou dans le thème sombre.
- `UIBlurEffectStyleProminent` -Utilise un flou très clair dans le thème clair et un flou très sombre dans le thème sombre.

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>Utilisation des Collections de Trait

La nouvelle `UserInterfaceStyle` propriété de la `UITraitCollection` classe peut être utilisé pour obtenir le thème de l’interface utilisateur actuellement sélectionné et sera une `UIUserInterfaceStyle` enum de l’une des valeurs suivantes :

- **Lumière** -thème de l’interface utilisateur de la lumière est sélectionné.
- **Sombre** -thème de l’interface utilisateur foncé est sélectionné.
- **Non spécifié** -la vue n’a pas été affiché à l’écran, auquel cas le thème actuel de l’interface utilisateur est inconnu.

En outre, les regroupements de caractéristique présentent les fonctionnalités suivantes dans tvOS 10 :
 
- Le proxy de l’apparence peut être personnalisé en fonction de la `UserInterfaceStyle` d’une donnée `UITraitCollection` à modifier des éléments tels que des images ou des élément basés sur le thème de couleurs.
- Une application de tvOS peut gérer les modifications de la Collection de Trait en substituant le `TraitCollectionDidChange` méthode d’un `UIView` ou `UIViewController` classe.

> [!IMPORTANT]
> Ne prend entièrement en charge la version préliminaire Xamarin.tvOS de tvOS 10 `UIUserInterfaceStyle` pour `UITraitCollection` encore. Prise en charge complète sera ajoutée dans une version ultérieure.




<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>Personnaliser l’apparence en fonction de thème

Pour les éléments d’Interface utilisateur qui prennent en charge le proxy de l’apparence, leur apparence peut être ajustée en fonction du thème de l’interface utilisateur de leur Collection caractéristique. Par conséquent, pour un élément d’interface utilisateur donné, le développeur peut spécifier une couleur pour le thème clair et une autre couleur pour le thème sombre.

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> Malheureusement, la version d’évaluation Xamarin.tvOS de tvOS 10 ne prend entièrement en charge `UIUserInterfaceStyle` pour `UITraitCollection`, de sorte que ce type de personnalisation n’est pas encore disponible. Prise en charge complète sera ajoutée dans une version ultérieure.

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>Réponse aux modifications de thème directe

Dans le développeur requiert un contrôle plus approfondie sur l’apparence d’un élément d’interface utilisateur en fonction du thème de l’interface utilisateur sélectionné, ils peuvent remplacer la `TraitCollectionDidChange` méthode d’un `UIView` ou `UIViewController` classe.

Par exemple :

```csharp
public override void TraitCollectionDidChange (UITraitCollection previousTraitCollection)
{
    base.TraitCollectionDidChange (previousTraitCollection);
    
    // Take action based on the Light or Dark theme
    ...
}
```

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="overriding-a-trait-collection"></a>Substitution d’une Collection de Trait

En fonction de la conception d’une application de tvOS, il peut arriver lorsque le développeur doit substituer la Collection caractéristique d’un élément d’Interface utilisateur donné et de toujours utiliser un thème de l’interface utilisateur spécifique.

Cela est possible à l’aide de la `SetOverrideTraitCollection` méthode sur la `UIViewController` classe. Par exemple :

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

Pour plus d’informations, consultez la [caractéristiques](~/ios/user-interface/storyboards/unified-storyboards.md) et [caractéristiques de substitution](~/ios/user-interface/storyboards/unified-storyboards.md) sections de notre [présentation unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md) documentation.

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>Plans conceptuels et les Collections de trait

Dans tvOS 10, table de montage séquentiel d’une application peut être définie pour répondre à des Collections de Trait et de nombreux éléments d’interface utilisateur peuvent être informés clair et thème sombre. L’aperçu Xamarin.tvOS anticipée de tvOS 10 ne prend en charge cette fonctionnalité dans le Concepteur de l’Interface encore, l’animation sera doivent être modifiés dans le Générateur de Xcode Interface comme solution de contournement.

Pour activer la prise en charge de la Collection de caractéristiques, procédez comme suit :

1. Avec le bouton droit sur le fichier d’animation dans le **l’Explorateur de solutions** et sélectionnez **ouvrir avec** > **Xcode Interface Builder**: 

    [![](user-interface-styles-images/theme05.png "Ouvrir avec le générateur Xcode Interface")](user-interface-styles-images/theme05.png#lightbox) 
2. Pour activer la prise en charge de la Collection de caractéristiques, basculez vers le **inspecteur de fichier** et vérifiez la **utilisez caractéristique Variations** propriété dans le **Interface Générateur de Document** section : 

    [![](user-interface-styles-images/theme06.png "Activer la prise en charge de la Collection de Trait")](user-interface-styles-images/theme06.png#lightbox)
3. Confirmer la modification pour utiliser le Trait de variantes : 

    [![](user-interface-styles-images/theme07.png "L’alerte de caractéristique Variations d’utilisation")](user-interface-styles-images/theme07.png#lightbox)
4. Enregistrez les modifications dans le fichier d’animation.

Apple a ajouté les capacités suivantes lorsque vous modifiez des animations tvOS dans le Générateur de Interface :

* Le développeur peut spécifier les types d’éléments d’Interface utilisateur basées sur le thème de l’interface utilisateur dans le **inspecteur de l’attribut**:
    
    * Plusieurs propriétés ont désormais une **+** en regard de ce qui peut être sélectionné pour ajouter une version spécifique de thème de l’interface utilisateur : 

        [![](user-interface-styles-images/theme08.png "Ajouter une version spécifique de thème de l’interface utilisateur")](user-interface-styles-images/theme08.png#lightbox) 
    
    * Le développeur peut spécifier une nouvelle propriété, ou cliquez sur le **x** bouton Supprimer : 

        [![](user-interface-styles-images/theme09.png "Spécifiez une nouvelle propriété, ou cliquez sur le bouton x pour le supprimer")](user-interface-styles-images/theme09.png#lightbox)
* Le développeur peut afficher un aperçu une conception d’interface utilisateur dans le thème sombre ou clair d’Interface Builder :
    
    * Le bas de l’aire de conception permet au développeur basculer le thème actuel de l’interface utilisateur : 

        [![](user-interface-styles-images/theme10.png "Le bas de l’aire de conception")](user-interface-styles-images/theme10.png#lightbox)
        
    * Le nouveau thème s’affichera dans le constructeur d’Interface et les réglages spécifiques caractéristique Collection seront affichera : 

        [![](user-interface-styles-images/theme11.png "Le thème affiché dans le constructeur d’Interface")](user-interface-styles-images/theme11.png#lightbox)

En outre, le simulateur de tvOS a maintenant un raccourci clavier pour permettre au développeur basculer rapidement entre les thèmes clairs et foncés lors du débogage d’une application tvOS. Utilisez le **commande-MAJ-D** séquence pour basculer entre les clairs et foncés de clavier.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a traité la lumière et thèmes de l’interface utilisateur sombre que Apple a ajouté à tvOS 10 et comment les implémenter dans une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Nouveautés de tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
