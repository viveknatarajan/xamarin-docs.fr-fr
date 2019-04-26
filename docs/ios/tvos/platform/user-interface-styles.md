---
title: tvOS Styles d’interface utilisateur dans Xamarin
description: Cet article traite de la lumière et thèmes d’interface utilisateur foncé par Apple a ajouté à tvOS 10 et comment les implémenter dans une application Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 2536ca5d3bff3f5b7962bc4fcf58b31a130fd03c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61270886"
---
# <a name="tvos-user-interface-styles-in-xamarin"></a>tvOS Styles d’interface utilisateur dans Xamarin

_Cet article traite de la lumière et thèmes d’interface utilisateur foncé par Apple a ajouté à tvOS 10 et comment les implémenter dans une application Xamarin.tvOS._

tvOS 10 maintenant prend en charge un sombre et l’Interface utilisateur de lumière thème que tous le UIKit dans build contrôle va automatiquement s’adapter aux, en fonction des préférences de l’utilisateur. En outre, le développeur peut ajuster manuellement les éléments d’interface utilisateur en fonction du thème sélectionné par l’utilisateur a et peut substituer un thème donné.

<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>Sur les nouveaux Styles d’Interface utilisateur

Comme indiqué ci-dessus, tvOS 10 maintenant prend en charge un sombre et l’Interface utilisateur de lumière thème que tous le UIKit dans build contrôle va automatiquement s’adapter aux, en fonction des préférences de l’utilisateur.

L’utilisateur peut remplacer ce thème en accédant à **paramètres** > **général** > **apparence** et commutation entre **clair**  et **foncé**:

[![](user-interface-styles-images/theme01.png "L’application paramètres")](user-interface-styles-images/theme01.png#lightbox)

Lorsque le **foncé** thème est sélectionné, tous les éléments d’Interface utilisateur passe en texte clair sur un arrière-plan foncé :

[![](user-interface-styles-images/theme02.png "Le thème sombre")](user-interface-styles-images/theme02.png#lightbox)

L’utilisateur a la possibilité de basculer le thème à tout moment et peut le faire par conséquent, en fonction de l’activité actuelle, où se trouve l’Apple TV ou l’heure du jour.

Le thème de l’interface utilisateur de lumière est le thème par défaut, et toutes les applications tvOS existantes continuera d’utiliser le thème clair, indépendamment des préférences de l’utilisateur, sauf si elles sont modifiées pour tvOS 10 pour tirer parti du thème foncé. Une application tvOS 10 a également la possibilité de remplacer le thème actuel et de toujours utiliser le thème sombre ou clair pour tout ou partie de son interface utilisateur.

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>Adopter les thèmes claires et foncées

Pour prendre en charge cette fonctionnalité, Apple a ajouté une nouvelle API pour le `UITraitCollection` classe et une application tvOS doivent participer pour prendre en charge de l’apparence foncé (via un paramètre dans son `Info.plist` fichier).

Pour participer à la prise en charge de thème clairs et foncés, procédez comme suit :

1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier `Info.plist` pour l’ouvrir et le modifier.
2. Sélectionnez le **Source** vue (à partir du bas de l’éditeur).
3. Ajoutez une nouvelle clé et appelez-le `UIUserInterfaceStyle`: 

    [![](user-interface-styles-images/theme03.png "La clé UIUserInterfaceStyle")](user-interface-styles-images/theme03.png#lightbox)
4. Laissez le type défini sur `String` et entrez la valeur `Automatic`: 

    [![](user-interface-styles-images/theme04.png "Entrez automatique")](user-interface-styles-images/theme04.png#lightbox)
5. Enregistrez les modifications dans le fichier.

Il existe trois valeurs possibles pour le `UIUserInterfaceStyle` clé :

- **Lumière** -force l’interface utilisateur de l’application tvOS à toujours utiliser le thème clair.
- **Foncé** -force l’interface utilisateur de l’application tvOS pour toujours utiliser le thème sombre.
- **Automatique** -bascule entre le thème clairs et foncés en fonction des préférences de l’utilisateur dans les paramètres. Il s’agit du paramètre recommandé.

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>Prise en charge du thème de UIKit

Si une application tvOS est à l’aide intégrée, standard, `UIView` en fonction des contrôles, ils répondront automatiquement pour le thème de l’interface utilisateur sans aucune intervention du développeur.

En outre, `UILabel` et `UITextView` change automatiquement leur couleur en fonction du thème de l’interface utilisateur sélectionnez :

- Le texte sera noir dans le thème clair.
- Le texte sera blanc dans le thème sombre.

Si le développeur change la couleur du texte manuellement (soit dans la table de montage séquentiel ou le code), ils seront responsables de la gestion des modifications de couleur en fonction du thème de l’interface utilisateur.

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>Nouveaux effets de flou

Pour prendre en charge les thèmes clairs et foncés dans une application tvOS 10, Apple a ajouté deux nouveaux effets flou. Ces nouveaux effets ajuste automatiquement le flou selon le thème de l’interface utilisateur que l’utilisateur a sélectionnée comme suit :

- `UIBlurEffectStyleRegular` -Utilise une lumière flou dans le thème clair et sombre flou dans le thème sombre.
- `UIBlurEffectStyleProminent` : Utilise un flou extra-Light dans le thème clair et un flou très sombre dans le thème sombre.

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>Utilisation des Collections de Trait

La nouvelle `UserInterfaceStyle` propriété de la `UITraitCollection` classe peut être utilisée pour obtenir le thème de l’interface utilisateur actuellement sélectionné et sera un `UIUserInterfaceStyle` enum de l’une des valeurs suivantes :

- **Lumière** -thème de l’interface utilisateur de Silverlight est sélectionné.
- **Foncé** -thème de l’interface utilisateur foncé est sélectionné.
- **Non spécifié** -la vue n’a pas été affichée à l’écran, par conséquent, le thème de l’interface utilisateur actuel est inconnu.

En outre, les Collections de caractéristique possèdent les fonctionnalités suivantes dans tvOS 10 :
 
- Le proxy de l’apparence peut être personnalisé selon le `UserInterfaceStyle` d’une donnée `UITraitCollection` pour modifier des choses telles que des images ou d’élément selon le thème de couleurs.
- Une application tvOS peut gérer les modifications de la Collection de Trait en substituant le `TraitCollectionDidChange` méthode d’un `UIView` ou `UIViewController` classe.

> [!IMPORTANT]
> La version préliminaire Xamarin.tvOS pour tvOS 10 ne prennent entièrement en charge `UIUserInterfaceStyle` pour `UITraitCollection` encore. Prise en charge complète sera ajoutée dans une version ultérieure.




<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>Personnaliser l’apparence en fonction de thème

Pour les éléments de l’Interface utilisateur qui prennent en charge le proxy de l’apparence, leur apparence peut être ajustée en fonction du thème de l’interface utilisateur de leur Collection de caractéristique. Par conséquent, pour un élément d’interface utilisateur donné, le développeur peut spécifier une couleur pour le thème clair et un autre pour le thème sombre.

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> Malheureusement, la version préliminaire Xamarin.tvOS pour tvOS 10 ne prennent entièrement en charge `UIUserInterfaceStyle` pour `UITraitCollection`, de sorte que ce type de personnalisation n’est pas encore disponible. Prise en charge complète sera ajoutée dans une version ultérieure.

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>Réponse directe aux modifications de thème

Dans le developer nécessite le plus grand contrôle sur l’apparence d’un élément d’interface utilisateur en fonction du thème de l’interface utilisateur sélectionné, ils peuvent remplacer la `TraitCollectionDidChange` méthode d’un `UIView` ou `UIViewController` classe.

Exemple :

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

En fonction de la conception d’une application tvOS, il peut arriver lorsque le développeur doit remplacer la Collection de caractéristique d’un élément d’Interface utilisateur donné et de sorte qu’il utilise toujours un thème de l’interface utilisateur spécifique.

Cela est possible à l’aide de la `SetOverrideTraitCollection` méthode sur le `UIViewController` classe. Exemple :

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

Pour plus d’informations, consultez le [Traits](~/ios/user-interface/storyboards/unified-storyboards.md) et [Traits substitution](~/ios/user-interface/storyboards/unified-storyboards.md) sections de notre [Introduction aux Storyboards unifiés](~/ios/user-interface/storyboards/unified-storyboards.md) documentation.

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>Storyboards et des Collections de trait

Dans tvOS 10, table de montage séquentiel d’une application peut être définie pour répondre à des Collections de Trait et de nombreux éléments d’interface utilisateur peuvent être informés clair et thème sombre. Le cours Xamarin.tvOS version préliminaire pour tvOS 10 ne prend en charge cette fonctionnalité dans le Concepteur d’Interface encore, sorte que la table de montage séquentiel devront être modifiées dans l’Interface Builder de Xcode pour résoudre ce problème.

Pour activer la prise en charge de la Collection de caractéristique, procédez comme suit :

1. Avec le bouton droit sur le fichier de la table de montage séquentiel dans le **l’Explorateur de solutions** et sélectionnez **ouvrir avec** > **Xcode Interface Builder**: 

    [![](user-interface-styles-images/theme05.png "Ouvrir avec Xcode Interface Builder")](user-interface-styles-images/theme05.png#lightbox) 
2. Pour activer la prise en charge de la Collection de caractéristique, basculez vers le **inspecteur de fichier** et vérifiez le **Variations des caractéristiques utilisation** propriété dans le **Document de générateur d’Interface** section : 

    [![](user-interface-styles-images/theme06.png "Activer la prise en charge de la Collection de Trait")](user-interface-styles-images/theme06.png#lightbox)
3. Confirmer la modification pour utiliser les Variations des caractéristiques : 

    [![](user-interface-styles-images/theme07.png "L’alerte de Variations des caractéristiques d’utilisation")](user-interface-styles-images/theme07.png#lightbox)
4. Enregistrer les modifications dans le fichier d’animation.

Apple a ajouté les aptitudes suivantes lors de la modification d’animations tvOS dans Interface Builder :

* Le développeur peut spécifier différentes variantes d’éléments d’Interface utilisateur basées sur le thème de l’interface utilisateur dans le **inspecteur d’attributs**:
    
    * Plusieurs propriétés ont maintenant un **+** à côté d’eux qui peut être activé pour ajouter une version spécifique de thème de l’interface utilisateur : 

        [![](user-interface-styles-images/theme08.png "Ajouter une version spécifique de thème de l’interface utilisateur")](user-interface-styles-images/theme08.png#lightbox) 
    
    * Le développeur peut spécifier une nouvelle propriété ou cliquez sur le **x** bouton pour le supprimer : 

        [![](user-interface-styles-images/theme09.png "Spécifiez une nouvelle propriété, ou cliquez sur le bouton x pour le supprimer")](user-interface-styles-images/theme09.png#lightbox)
* Le développeur peut afficher un aperçu une conception d’interface utilisateur dans le thème sombre ou clair à partir d’Interface Builder :
    
    * Le bas de l’aire de conception permet au développeur de basculer le thème de l’interface utilisateur actuels : 

        [![](user-interface-styles-images/theme10.png "Le bas de l’aire de conception")](user-interface-styles-images/theme10.png#lightbox)
        
    * Le nouveau thème s’affichera dans le Générateur d’Interface et des ajustements spécifiques de Collection de caractéristique seront affichera : 

        [![](user-interface-styles-images/theme11.png "Le thème affiché dans l’Interface Builder")](user-interface-styles-images/theme11.png#lightbox)

En outre, le simulateur tvOS a maintenant un raccourci clavier pour permettre au développeur de basculer rapidement entre les thèmes clairs et foncés lors du débogage d’une application tvOS. Utilisez le **commande-MAJ-D** séquence pour basculer entre clairs et foncés de clavier.

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté la lumière et thèmes d’interface utilisateur foncé par Apple a ajouté à tvOS 10 et comment les implémenter dans une application Xamarin.tvOS.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Quelles sont les nouveautés dans tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
