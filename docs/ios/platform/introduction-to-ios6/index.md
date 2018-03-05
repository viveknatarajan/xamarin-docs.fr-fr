---
title: "Introduction à iOS 6"
description: "iOS 6 inclut une variété de nouvelles technologies de développement d’applications, ce qui Xamarin.iOS 6 offre aux développeurs c#."
ms.topic: article
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 0d08b2ff5131996984dbe41862ad1d6aac33b3d0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-6"></a>Introduction à iOS 6

_iOS 6 inclut une variété de nouvelles technologies de développement d’applications, ce qui Xamarin.iOS 6 offre aux développeurs c#._

[ ![](images/ios6-large.jpg "Le logo iOS 6")](images/ios6-large.jpg)

IOS 6 et 6 de Xamarin.iOS, les développeurs ont désormais une quantité de capacité à leur disposition pour créer des applications iOS, y compris celles qu’iPhone cible 5.
Ce document répertorie quelques-unes des plus intéressant de nouvelles fonctionnalités qui sont disponibles et des liens vers des articles pour chaque rubrique. En outre, il touche sur quelques modifications seront importantes que les développeurs déplacent iOS 6 et la nouvelle résolution d’iPhone 5.


## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[Introduction aux vues de Collection](~/ios/user-interface/controls/uicollectionview.md)

Vues de collection autoriser le contenu à afficher à l’aide de dispositions arbitraires. Elles permettent de créer aisément des mises en page de grille l’emploi, la prise en charge des dispositions personnalisées. Pour plus d’informations, le [Introduction aux vues de Collection](~/ios/user-interface/controls/uicollectionview.md) [ ](~/ios/user-interface/controls/uicollectionview.md)guide.


## <a name="introduction-to-pass-kitiosplatformpasskitmd"></a>[Introduction à passer Kit](~/ios/platform/passkit.md)

L’infrastructure de passer un Kit permet aux applications d’interagir avec des passes numériques qui sont gérés dans l’application de livret. Pour plus d’informations, le [Introduction au guide du Kit de passer](~/ios/platform/passkit.md).


##  <a name="introduction-to-event-kitiosplatformeventkitmd"></a>[Introduction au Kit d’événement](~/ios/platform/eventkit.md)

Le framework EventKit permet d’accéder aux calendriers, calendrier et les données des rappels qui stocke de la base de données de calendrier. Accès aux calendriers et calendrier événements est disponible depuis iOS 4, mais iOS 6 expose désormais accès aux données de rappels. Pour plus d’informations, consultez la [I](~/ios/platform/eventkit.md) [rit à EventKit](~/ios/platform/eventkit.md) guide.


##  <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[Introduction à l’infrastructure de réseaux sociaux](~/ios/platform/social-framework.md)

Le Framework Social fournit une API unifiée pour interagir avec les réseaux sociaux, y compris Twitter et Facebook, ainsi que SinaWeibo pour les utilisateurs en Chine. Pour plus d’informations, le [Introduction à l’infrastructure de réseaux sociaux](~/ios/platform/social-framework.md) guide.


##  <a name="changes-to-store-kitchanges-to-storekitmd"></a>[Modifications apportées à stocker Kit](changes-to-storekit.md)

Apple a introduit deux nouvelles fonctionnalités dans le magasin de Kit : achat et téléchargement iTunes ou contenu de magasin d’applications à partir de votre application et qui héberge vos fichiers de contenu pour les achats dans l’application !. Pour plus d’informations, le [change au Kit de magasin](changes-to-storekit.md) guide.


## <a name="other-changes"></a>Autres modifications


### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload et ViewDidUnload déconseillée

Le `ViewWillUnload` et `ViewDidUnload` méthodes de `UIViewController` sont appelés n’est plus dans iOS 6. Dans les versions antérieures d’iOS, ces méthodes peuvent avoir été utilisés par les applications de l’enregistrement de l’état avant décharge d’une vue et le code de nettoyage respectivement.

Par exemple, Visual Studio pour Mac créerait une méthode appelée `ReleaseDesignerOutlets`, comme illustré ci-dessous, puis appelé à partir de `ViewDidUnload`:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

Toutefois, dans iOS 6, il n’est plus nécessaire d’appeler `ReleaseDesignerOutlets`.   
   
   
   
Pour le code de nettoyage, doivent utiliser des applications iOS 6 `DidReceiveMemoryWarning`. Toutefois, le code qui appelle la méthode `Dispose` doit être utilisée avec modération et uniquement pour la mémoire intensif objets comme indiqué ci-dessous :

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

Là encore, l’appel `Dispose` comme ci-dessus doit rarement nécessaire. En général que le meilleur parti d’applications doivent faire est de supprimer des gestionnaires d’événements.

Dans le cas d’enregistrement de l’état, les applications peuvent effectuer cela dans `ViewWillDisappear` et `ViewDidDisappear` au lieu de `ViewWillUnload`.


### <a name="iphone-5-resolution"></a>iPhone 5 résolution

appareils iPhone 5 ont une résolution de 640 x 1136. Les applications ciblant les versions antérieures d’iOS apparaîtra letterboxed sur un iPhone 5, comme indiqué ci-dessous :

 [ ![](images/01-letterboxed.png "Les applications ciblant les versions antérieures d’iOS apparaîtra letterboxed lorsque vous exécutez sur un iPhone 5")](images/01-letterboxed.png)

Par ordre d’affichage de l’application plein écran sur iPhone 5, ajoutez simplement une image nommée `Default-568h@2x.png` ayant une résolution de 640 x 1136. La capture d’écran suivante montre l’application en cours d’exécution une fois cette image a été incluse :

 [ ![](images/02-fullscreen.png "Cette capture d’écran montre l’application en cours d’exécution une fois cette image a été incluse")](images/02-fullscreen.png)

### <a name="subclassing-uinavigationbar"></a>Sous-classement UINavigationBar

Dans iOS 6 `UINavigationBar` peuvent être sous-classés. Cela permet un contrôle supplémentaire de l’apparence de la `UINavigationBar`. Par exemple, les applications peuvent sous-classe pour ajouter sous-vues, animer ces vues et modifier les limites de la `UINavigationBar`.

Le code suivant montre un exemple d’une sous-classe `UINavigationBar` qui ajoute un `UIImageView`:

```csharp
public class CustomNavBar : UINavigationBar
{
    UIImageView iv;
    public CustomNavBar (IntPtr h) : base(h)
    {
        iv = new UIImageView (UIImage.FromFile ("monkey.png"));
        iv.Frame = new CGRect (75, 0, 30, 39);
    }
    public override void Draw (RectangleF rect)
    {
        base.Draw (rect);
        TintColor = UIColor.Purple;
        AddSubview (iv);
    }
}
```

Pour ajouter un sous-classé `UINavigationBar` à un `UINavigationController`, utilisez le `UINavigationController` constructeur qui prend le type de la `UINavigationBar` et `UIToolbar`, comme illustré ci-dessous :

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

L’utilisation de cette `UINavigationBar` sous-classe entraîne l’affichage de l’image affichée comme indiqué dans la capture d’écran suivante :

 [ ![](images/03-navbar.png "L’utilisation de ce UINavigationBar sous-classe la vue de l’image affichée comme indiqué dans cette capture d’écran")](images/03-navbar.png)

### <a name="interface-orientation"></a>Orientation de l’interface

Avant d’iOS 6 applications peuvent substituer `ShouldAutorotateToInterfaceOrientation`, retournant true pour les orientations contrôleur spécifique pris en charge. Par exemple, le code suivant serait être utilisé pour prendre en charge uniquement le mode portrait :

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

Dans iOS 6 `ShouldAutorotateToInterfaceOrientation` est déconseillée.
À la place les applications peuvent remplacer `GetSupportedInterfaceOrientations` sur le contrôleur de vue racine, comme indiqué ci-dessous :

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

Sur un iPad, l’emplacement par défaut pour les quatre orientations si `GetSupportedInterfaceOrientation` n’est pas implémentée. Sur l’iPhone et iPod Touch, la valeur par défaut est toutes les orientations sauf `PortraitUpsideDown`.