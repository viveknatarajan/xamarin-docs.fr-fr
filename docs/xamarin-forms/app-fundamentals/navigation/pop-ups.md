---
title: Affichage des fenêtres contextuelles
description: Xamarin.Forms fournit deux éléments d’interface utilisateur pop-inscription de type – une alerte et une feuille de l’action. Cet article illustre l’utilisation de la feuille de l’alerte et l’action API pour poser des questions simples utilisateurs et pour guider les utilisateurs dans les tâches.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 156c2f9dca47a7755d4f810d7921a05662388ded
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996712"
---
# <a name="displaying-pop-ups"></a>Affichage des fenêtres contextuelles

_Xamarin.Forms fournit deux éléments d’interface utilisateur pop-inscription de type – une alerte et une feuille de l’action. Cet article illustre l’utilisation de la feuille de l’alerte et l’action API pour poser des questions simples utilisateurs et pour guider les utilisateurs dans les tâches._

Affichage d’une alerte ou demander à un utilisateur à faire un choix est une tâche courante de l’interface utilisateur. Xamarin.Forms a deux méthodes la [ `Page` ](xref:Xamarin.Forms.Page) classe permettant d’interagir avec l’utilisateur via une fenêtre contextuelle : [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) et [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*). Ils sont affichés avec les contrôles natifs appropriés sur chaque plateforme.

## <a name="displaying-an-alert"></a>Affichage d’une alerte

Toutes les plateformes prises en charge de Xamarin.Forms ont une fenêtre contextuelle modale pour alerter l’utilisateur ou de poser des questions simples d'entre eux. Pour afficher ces alertes dans Xamarin.Forms, utilisez le [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) méthode sur n’importe quel [ `Page` ](xref:Xamarin.Forms.Page). La ligne de code suivante montre un message simple à l’utilisateur :

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Boîte de dialogue alerte avec un bouton")

Cet exemple ne collecte aucune information à partir de l’utilisateur. L’alerte affiche de façon modale et une fois fermée de l’utilisateur continue d’interagir avec l’application.

Le [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) méthode peut également être utilisée pour capturer répond l’utilisateur en présentant les deux boutons et en retournant un `boolean`. Pour obtenir une réponse à partir d’une alerte, fournissez un texte pour les deux boutons et `await` la méthode. Une fois que l’utilisateur sélectionne une des options de que la réponse s’affichera à votre code. Remarque la `async` et `await` mots clés dans l’exemple de code ci-dessous :

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "boîte de dialogue avec deux boutons d’alerte")](pop-ups-images/alert2.png#lightbox "boîte de dialogue avec deux boutons d’alerte")

## <a name="guiding-users-through-tasks"></a>Guidage des utilisateurs via des tâches

Le [UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) est un élément d’interface utilisateur courants dans iOS. Xamarin.Forms [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*) méthode vous permet d’inclure ce contrôle dans les applications multiplateformes, rendu des alternatives natives dans Android et UWP.

Pour afficher une feuille de l’action, `await` [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*) dans les [ `Page` ](xref:Xamarin.Forms.Page), en passant le message et bouton étiquettes sous forme de chaînes. La méthode retourne l’étiquette de chaîne du bouton sur lequel l’utilisateur a cliqué. Voici un exemple simple :

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "Boîte de dialogue ActionSheet")

Le `destroy` bouton est restitué différemment des autres et peut être laissé `null` ou spécifiée en tant que troisième paramètre de chaîne. L’exemple suivant utilise le `destroy` bouton :

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "Action feuille boîte de dialogue avec le bouton de destruction")](pop-ups-images/action2.png#lightbox "Action feuille boîte de dialogue avec le bouton de destruction")

## <a name="summary"></a>Récapitulatif

Cet article a montré à l’aide de la feuille de l’alerte et l’action API pour poser des questions simples utilisateurs et pour guider les utilisateurs dans les tâches. Xamarin.Forms a deux méthodes la [ `Page` ](xref:Xamarin.Forms.Page) classe permettant d’interagir avec l’utilisateur via une fenêtre contextuelle : [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) et [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*), et ils sont les deux rendus avec les contrôles natifs appropriés sur chaque plateforme.



## <a name="related-links"></a>Liens associés

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
