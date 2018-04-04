---
title: Contrôles personnalisés dans le Concepteur de Xamarin pour iOS
description: Le Concepteur de Xamarin pour iOS prend en charge le rendu des contrôles personnalisés créés dans votre projet ou référencées à partir de sources externes telles que le magasin de composants Xamarin.
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 113fab2fd0d1a055d566606885cefbafe3185529
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>Contrôles personnalisés dans le Concepteur de Xamarin pour iOS

_Le Concepteur de Xamarin pour iOS prend en charge le rendu des contrôles personnalisés créés dans votre projet ou référencées à partir de sources externes telles que le magasin de composants Xamarin._

Le Concepteur de Xamarin pour iOS est un outil puissant pour visualiser l’interface utilisateur d’une application et fournit WYSIWYG édition prise en charge pour la plupart des vues d’iOS et les contrôleurs de la vue. Votre application peut également contenir des contrôles personnalisés qui étendent ceux intégrés dans iOS. Si ces contrôles personnalisés sont écrits avec quelques recommandations à l’esprit, elles peuvent également être rendues par le concepteur, en fournissant une expérience d’édition enrichie iOS. Ce document aborde ces instructions.

## <a name="requirements"></a>Spécifications

Un contrôle qui répond à toutes les exigences suivantes est rendu sur l’aire de conception :

1.  Il s’agit d’une sous-classe directe ou indirecte de [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) ou [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIView/Controller). Autres [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) sous-classes apparaît sous la forme d’une icône sur l’aire de conception.
2.  Il a un [RegisterAttribute](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) pour l’exposer aux objectif-C.
3.  Il a [le constructeur requis IntPtr](~/ios/internals/api-design/index.md).
4.  Il implémente soit le [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/) interface ou a un [DesignTimeVisibleAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DesignTimeVisibleAttribute/) la valeur True.

Les contrôles définis dans le code qui répondent aux spécifications requises ci-dessus seront affiche dans le concepteur lorsque leur projet conteneur est compilé pour le simulateur. Par défaut, tous les contrôles personnalisés seront affiche dans le **des composants personnalisés** section de la **boîte à outils**. Toutefois, le [CategoryAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.CategoryAttribute/) peut être appliqué à la classe du contrôle personnalisé pour spécifier une autre section.

Le concepteur ne prend pas en charge le chargement des Objective-C bibliothèques tierces.

## <a name="custom-properties"></a>Propriétés personnalisées

Une propriété déclarée par un contrôle personnalisé s’affiche dans le panneau de configuration de propriété si les conditions suivantes sont remplies :

1.  La propriété a un accesseur Get public et un accesseur Set.
1.  La propriété a une [ExportAttribute](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) , ainsi qu’une [BrowsableAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.BrowsableAttribute/) définie sur True.
1.  Le type de propriété est un type numérique type énumération, string, bool, [SizeF](https://developer.xamarin.com/api/type/System.Drawing.SizeF/), [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/), ou [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/). Cette liste des types pris en charge peut se développer à l’avenir.


La propriété peut également être décorée avec un [DisplayNameAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DisplayNameAttribute/) pour spécifier l’étiquette qui s’affiche pour celui-ci dans le panneau de configuration de propriété.

## <a name="initialization"></a>Initialisation

Pour `UIViewController` sous-classes, vous devez utiliser le [ViewDidLoad](https://developer.xamarin.com/api/member/UIKit.UIViewController.ViewDidLoad/) méthode pour le code qui dépend des vues que vous avez créé dans le concepteur.

Pour `UIView` et d’autres `NSObject` sous-classes, le [AwakeFromNib](https://developer.xamarin.com/api/member/Foundation.NSObject.AwakeFromNib/) (méthode) est l’emplacement recommandé pour effectuer l’initialisation de votre contrôle personnalisé après son chargement à partir du fichier de mise en page. Il s’agit, car toutes les propriétés personnalisées définies dans le panneau de configuration de propriété ne seront pas définies lorsque le constructeur du contrôle est exécuté, mais ils seront définis avant `AwakeFromNib` est appelée :


```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        // Initialize the view here.
    }
}
```

Si le contrôle est également conçu pour être créées directement à partir de code, vous souhaiterez probablement créer une méthode qui a le code d’initialisation courantes, comme suit :

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
    }
}
```

## <a name="property-initialization-and-awakefromnib"></a>AwakeFromNib et l’initialisation des propriétés

Soyez vigilant sur quand et où initialiser les propriétés possible dans un composant personnalisé à ne remplacent pas les valeurs qui ont été définies dans le concepteur iOS. Par exemple, prenez le code suivant :

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {
    
    [Export ("Counter"), Browsable (true)]
    public int Counter {get; set;}

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
        Counter = 0;
    }
}
```

Le `CustomView` composant expose un `Counter` propriété pouvant être définies par le développeur à l’intérieur du concepteur iOS. Toutefois, quelle que soit la valeur est définie dans le concepteur, la valeur de la `Counter` propriété ne sera toujours zéro (0). En voici les raisons :

-  Une instance de la `CustomControl` est augmentée à partir du fichier de plan conceptuel.
-  Toutes les propriétés modifiées dans le concepteur iOS sont définies (telles que la définition de la valeur de la `Counter` à deux (2), par exemple).
-  Le `AwakeFromNib` méthode s’exécute et un appel est fait pour le composant `Initialize` (méthode).
-  À l’intérieur de `Initialize` la valeur de la `Counter` propriété est réinitialisée sur zéro (0).


Pour corriger cette situation, soit d’initialiser le `Counter` propriété ailleurs (par exemple, comme dans le constructeur du composant) ou vous ne substituez pas la `AwakeFromNib` méthode et appel `Initialize` si le composant ne requiert aucune initialisation supplémentaire en dehors de ce que est actuellement géré par ses constructeurs.

## <a name="design-mode"></a>En Mode Création

Sur l’aire de conception, un contrôle personnalisé doit respecter certaines restrictions :

-  Ressources de groupe d’application ne sont pas disponibles en mode Création. Les images sont disponibles lors du chargement via [UIImage méthodes](https://developer.xamarin.com/api/type/UIKit.UIImage/%2fM) .
-  Les opérations asynchrones, tels que les requêtes web, ne doivent pas être effectuées en mode Création. L’aire de conception ne prend pas en charge l’animation ou toutes les autres mises à jour asynchrones à l’interface utilisateur du contrôle.


Un contrôle personnalisé peut implémenter [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/) et utiliser le [DesignMode](https://developer.xamarin.com/api/property/System.ComponentModel.ISite.DesignMode/) propriété pour vérifier s’il est sur l’aire de conception. Dans cet exemple, l’étiquette affiche « Mode de conception » sur l’aire de conception et « Exécution » lors de l’exécution :

```csharp
[Register ("DesignerAwareLabel")]
public class DesignerAwareLabel : UILabel, IComponent {

    #region IComponent implementation

    public ISite Site { get; set; }
    public event EventHandler Disposed;

    #endregion

    public DesignerAwareLabel (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        if (Site != null &amp;&amp; Site.DesignMode)
            Text = "Design Mode";
        else
            Text = "Runtime";
    }
}
```

Vous devez toujours vérifier le `Site` propriété `null` avant d’essayer d’accéder à un de ses membres. Si `Site` est `null`, il est possible de supposer le contrôle n’est pas en cours d’exécution dans le concepteur.
En mode Création, `Site` sera défini après l’exécution par le constructeur du contrôle et avant `AwakeFromNib` est appelée.

## <a name="debugging"></a>Débogage

Un contrôle qui répond aux exigences ci-dessus sera affiché dans la boîte à outils et affichée sur l’aire.
Si un contrôle n’est pas affiché, recherchez les bogues dans le contrôle ou une de ses dépendances.

L’aire de conception peut souvent intercepter les exceptions levées par des contrôles individuels, tout en continuant à restituer les autres contrôles. Le contrôle défectueux est remplacé par un espace réservé rouge, et vous pouvez afficher la trace de l’exception en cliquant sur l’icône d’exclamation :

 ![](ios-designable-controls-overview-images/exception-box.png "Un contrôle défectueux en tant qu’espace réservé rouge et les détails de l’exception")

Si les symboles de débogage sont disponibles pour le contrôle, la trace auront des noms de fichier et les numéros de ligne. Double-cliquez sur une ligne dans la trace de pile accédera à la ligne dans le code source.

Si le concepteur ne peut pas isoler le contrôle défectueux, un message d’avertissement s’affiche en haut de l’aire de conception :

 ![](ios-designable-controls-overview-images/info-bar.png "Un message d’avertissement en haut de l’aire de conception")

Rendu complète reprend lorsque le contrôle défectueux est fixe ou supprimé de l’aire de conception.

## <a name="summary"></a>Récapitulatif

Cet article a introduit la création et l’application de contrôles personnalisés dans le concepteur iOS. Tout d’abord il décrit la configuration requise pour que les contrôles restituer sur l’aire de conception et d’exposer des propriétés personnalisées dans le panneau de configuration de propriété. Il recherche ensuite dans le code-behind - l’initialisation du contrôle et de la propriété DesignMode. Enfin, il décrit ce qui se passe lorsque les exceptions sont levées et comment résoudre ce problème.