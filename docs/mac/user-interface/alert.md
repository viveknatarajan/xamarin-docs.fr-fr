---
title: Alertes dans Xamarin.Mac
description: Cet article aborde l’utilisation des alertes dans une application Xamarin.Mac. Il décrit la création et affichage des alertes à partir de C# de code et de répondre aux interactions de l’utilisateur.
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 8f84b688998251db52c8c2be71949e1a2e665dc0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103957"
---
# <a name="alerts-in-xamarinmac"></a>Alertes dans Xamarin.Mac

_Cet article aborde l’utilisation des alertes dans une application Xamarin.Mac. Il décrit la création et affichage des alertes à partir de C# de code et de répondre aux interactions de l’utilisateur._

Lorsque vous travaillez avec C# et .NET dans une application Xamarin.Mac, que vous avez accès à la même prévient qu’un développeur travaillant dans *Objective-C* et *Xcode* est. 

Une alerte est un type spécial de boîte de dialogue qui s’affiche lorsqu’un problème grave se produit (par exemple, une erreur) ou en tant qu’avertissement (par exemple, la préparation de la suppression d’un fichier). Une alerte étant une boîte de dialogue, elle requiert également une réponse de l’utilisateur avant d’être fermée.

[![](alert-images/alert06.png "Un exemple d’alerte")](alert-images/alert06.png#lightbox)

Dans cet article, nous traiterons les principes fondamentaux de l’utilisation des alertes dans une application Xamarin.Mac. 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>Introduction aux alertes

Une alerte est un type spécial de boîte de dialogue qui s’affiche lorsqu’un problème grave se produit (par exemple, une erreur) ou en tant qu’avertissement (par exemple, la préparation de la suppression d’un fichier). Étant donné que les alertes d’interrompre l’utilisateur dans la mesure où ils doivent être fermées avant que l’utilisateur peut poursuivre sur leurs tâches, éviter d’afficher une alerte, sauf si elle est absolument nécessaire.

Apple suggère les recommandations suivantes :

- N’utilisez pas une alerte simplement pour permettre aux utilisateurs d’informations.
- Ne pas afficher une alerte pour les actions courantes, annulables. Même si cette situation peut entraîner une perte de données.
- Si une situation mérite d’une alerte, évitez d’utiliser tout autre élément d’interface utilisateur ou une autre méthode pour l’afficher.
- L’icône d’avertissement doit être utilisée avec parcimonie.
- Décrire la situation alerte clairement et brièvement dans le message d’alerte.
- Le nom du bouton par défaut doit correspondre à l’action que vous décrivez dans votre message d’alerte.

Pour plus d’informations, consultez le [alertes](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1) section d’Apple [X Human Interface Guidelines pour OS](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>Anatomie d’une alerte

Comme indiqué ci-dessus, les alertes doivent être affichées à l’utilisateur de votre application lorsqu’un problème grave survient ou en tant qu’avertissement d’éventuelles pertes de données (comme la fermeture d’un fichier non enregistré). Dans Xamarin.Mac, une alerte est créée dans C# de code, par exemple :

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Critical,
    InformativeText = "We need to save the document here...",
    MessageText = "Save Document",
};
alert.RunModal ();
```

Le code ci-dessus affiche une alerte avec l’icône d’applications en superposition sur l’icône d’avertissement, un titre, un message d’avertissement et un seul **OK** bouton :

[![](alert-images/alert01.png "Une alerte avec un bouton OK")](alert-images/alert01.png#lightbox)

Apple fournit plusieurs propriétés qui peuvent être utilisées pour personnaliser une alerte :

- **AlertStyle** définit le type d’une alerte comme l’une des opérations suivantes :
    - **Avertissement** : utilisé pour avertir l’utilisateur un événement en cours ou à venir qui n’est pas critique. Il s’agit du style par défaut.
    - **D’information** : utilisé pour avertir l’utilisateur sur un événement en cours ou à venir. Actuellement, il n’existe aucune différence visible entre un **avertissement** et un **information**
    - **Critique** : utilisé pour avertir l’utilisateur à propos de graves conséquences d’un événement à venir (par exemple, la suppression d’un fichier). Ce type d’alerte doit être utilisé avec parcimonie.
- **MessageText** -il s’agit du message principal ou le titre de l’alerte et doit définir rapidement la situation à l’utilisateur.
- **InformativeText** -il s’agit du corps de l’alerte, où vous devez définir clairement de la situation et présenter des options envisageables à l’utilisateur.
- **Icône** -permet à une icône personnalisée à afficher à l’utilisateur.
- **HelpAnchor** & **ShowsHelp** -permet à l’alerte être lié à l’application HelpBook et afficher l’aide de l’alerte.
- **Boutons** -par défaut, une alerte a uniquement les **OK** bouton mais la **boutons** collection vous permet d’ajouter plus de choix en fonction des besoins.
- **ShowsSuppressionButton** : si `true` affiche une case à cocher l’utilisateur peut utiliser pour supprimer l’alerte d’autres occurrences de l’événement qui l’a déclenchée.
- **AccessoryView** -permet de joindre un autre sous-affichage à l’alerte pour fournir des informations supplémentaires, telles que l’ajout un **champ de texte** pour l’entrée de données. Si vous définissez un nouveau **AccessoryView** ou modifier un existant, vous devez appeler la `Layout()` méthode pour ajuster la disposition visible de l’alerte.

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>Affichage d’une alerte

Il existe deux façons différentes qu’une alerte peut être affichée, flottant ou sous forme de tableau. Le code suivant affiche une alerte sous la forme flottante :

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.RunModal ();
```
Si ce code est exécuté, ce qui suit s’affiche :

[![](alert-images/alert02.png "Une alerte simple")](alert-images/alert02.png#lightbox)

Le code suivant affiche la même alerte sous forme de tableau :

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

Si ce code est exécuté, ce qui suit s’affiche :

[![](alert-images/alert03.png "Une alerte s’affichée sous forme de tableau")](alert-images/alert03.png#lightbox)


<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>Travaillez avec des boutons d’alerte

Par défaut, une alerte s’affiche uniquement les **OK** bouton. Toutefois, vous n’êtes pas limité à, vous pouvez créer des boutons supplémentaires en les ajoutant à la **boutons** collection. Le code suivant crée une alerte flottante avec un **OK**, **Annuler** et **peut-être** bouton :

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
var result = alert.RunModal ();
```

Le tout premier bouton ajouté sera le _bouton par défaut_ qui sera activé si l’utilisateur appuie sur la touche ENTRÉE. La valeur retournée sera un nombre entier représentant quel bouton l’utilisateur a appuyé sur. Dans notre cas, les valeurs suivantes seront retournées :

- **OK** - 1000.
- **Annuler** : 1001.
- **Peut-être** - 1002.

Si nous exécutons le code, ce qui suit s’affiche :

[![](alert-images/alert04.png "Une alerte avec trois options de bouton")](alert-images/alert04.png#lightbox)

Voici le code pour la même alerte sous forme de tableau :

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}", result);
});
```
Si ce code est exécuté, ce qui suit s’affiche :

[![](alert-images/alert05.png "Une alerte de trois bouton affichée sous la forme d’une feuille")](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> Vous ne devez jamais ajouter plus de trois boutons à une alerte.

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>Affichant le bouton Supprimer

Si l’alerte `ShowSuppressButton` propriété est `true`, l’alerte affiche une case à cocher l’utilisateur peut utiliser pour supprimer l’alerte d’autres occurrences de l’événement qui l’a déclenchée. Le code suivant affiche une alerte flottant avec un bouton Supprimer :

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Si la valeur de la `alert.SuppressionButton.State` est `NSCellStateValue.On`, l’utilisateur a activé la case à cocher Supprimer, sinon ils n’ont pas.

Si le code est exécuté, ce qui suit s’affiche :

[![](alert-images/alert06.png "Une alerte avec un bouton Supprimer")](alert-images/alert06.png#lightbox)

Voici le code pour la même alerte sous forme de tableau :

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Si ce code est exécuté, ce qui suit s’affiche :

[![](alert-images/alert07.png "Une alerte avec un bouton Supprimer afficher sous forme de tableau")](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>Ajout d’un sous-affichage personnalisé

Les alertes présentent une `AccessoryView` propriété qui peut être utilisée pour personnaliser davantage l’alerte et ajouter des éléments tels qu’un **champ de texte** pour l’entrée utilisateur. Le code suivant crée une alerte flottante avec un champ d’entrée de texte ajouté :

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
AlertStyle = NSAlertStyle.Informational,
InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Les lignes de clés sont `var input = new NSTextField (new CGRect (0, 0, 300, 20));` qui crée un nouveau **champ de texte** que nous ajouterons l’alerte. `alert.AccessoryView = input;` les attache le **champ de texte** à l’alerte et l’appel à la `Layout()` (méthode), qui est requis pour redimensionner l’alerte pour tenir dans la nouvelle sous-affichage.

Si nous exécutons le code, ce qui suit s’affiche :

[![](alert-images/alert08.png "Si nous exécutons le code, ce qui suit s’affiche")](alert-images/alert08.png#lightbox)

Voici la même alerte sous forme de tableau :

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Si nous exécutons ce code, ce qui suit s’affiche :

[![](alert-images/alert09.png "Une alerte avec un affichage personnalisé")](alert-images/alert09.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a examiné en détail à l’utilisation des alertes dans une application Xamarin.Mac. Nous avons vu les différents types et les utilisations des alertes, comment créer et personnaliser les alertes et l’utilisation des alertes dans C# code.

## <a name="related-links"></a>Liens associés

- [MacWindows (exemple)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Utilisation de Windows](~/mac/user-interface/window.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Introduction à Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [NSAlert](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
