---
title: Afficher les fenêtres contextuelles
description: 'Xamarin.Forms fournit deux éléments d’interface utilisateur de pop haut-like : une alerte et une feuille de l’action. Cet article décrit à l’aide de la feuille de l’alerte et l’action API pour demander aux utilisateurs des questions simples et guider les utilisateurs lors des tâches.'
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 97f0917e4e8670ab379aae1b2707ae08cb29bb70
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2018
ms.locfileid: "32018877"
---
# <a name="displaying-pop-ups"></a>Afficher les fenêtres contextuelles

_Xamarin.Forms fournit deux éléments d’interface utilisateur de pop haut-like : une alerte et une feuille de l’action. Cet article décrit à l’aide de la feuille de l’alerte et l’action API pour demander aux utilisateurs des questions simples et guider les utilisateurs lors des tâches._

Affiche une alerte ou demander à un utilisateur d’effectuer un choix est une tâche courante de l’interface utilisateur. Xamarin.Forms a deux méthodes la [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) classe permettant d’interagir avec l’utilisateur via une fenêtre contextuelle : [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) et [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/). Ils sont rendus avec des contrôles natifs appropriés sur chaque plateforme.

## <a name="displaying-an-alert"></a>Affiche une alerte

Toutes les plateformes prises en charge de Xamarin.Forms ont une fenêtre contextuelle modale pour alerter l’utilisateur ou poser des questions simples d'entre eux. Pour afficher ces alertes dans Xamarin.Forms, utilisez le [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) méthode sur n’importe quel [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). La ligne de code suivante montre un message simple à l’utilisateur :

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Boîte de dialogue alerte avec un bouton")

Cet exemple ne collecte pas les informations de l’utilisateur. L’alerte affiche de façon modale et une fois que l’utilisateur de rejet continue d’interagir avec l’application.

Le [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) méthode peut également être utilisée pour capturer répond l’utilisateur en présentant deux boutons et en retournant un `boolean`. Pour obtenir une réponse à partir d’une alerte, fournissez un texte pour les deux boutons et `await` la méthode. Une fois que l’utilisateur sélectionne une des options de que la réponse s’affichera à votre code. Remarque la `async` et `await` mots clés dans l’exemple de code ci-dessous :

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "boîte de dialogue avec deux boutons d’alerte")](pop-ups-images/alert2.png#lightbox "boîte de dialogue avec deux boutons d’alerte")

## <a name="guiding-users-through-tasks"></a>Guide pour les utilisateurs via des tâches

Le [UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) est un élément d’interface utilisateur commun dans iOS. Le Xamarin.Forms [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/) méthode vous permet d’inclure ce contrôle dans les applications cross-plateformes, rendu natifs alternatives dans Android et UWP.

Pour afficher une feuille de l’action, `await` [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/) dans les [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), en passant le message et le bouton étiquettes sous forme de chaînes. La méthode retourne l’étiquette de la chaîne du bouton sur lequel l’utilisateur a cliqué. Voici un exemple simple :

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "Boîte de dialogue ActionSheet")

Le `destroy` bouton est restitué différemment des autres et peuvent être laissé `null` ou spécifiée en tant que troisième paramètre de chaîne. L’exemple suivant utilise le `destroy` bouton :

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "boîte de dialogue Action feuille avec le bouton de destruction")](pop-ups-images/action2.png#lightbox "boîte de dialogue Action feuille avec le bouton de destruction")

## <a name="summary"></a>Récapitulatif

Cet article ne présente à l’aide de la feuille de l’alerte et l’action API pour demander aux utilisateurs des questions simples et guider les utilisateurs lors des tâches. Xamarin.Forms a deux méthodes la [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) classe permettant d’interagir avec l’utilisateur via une fenêtre contextuelle : [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) et [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/), et ils sont les deux est rendue avec des contrôles natifs appropriés sur chaque plateforme.



## <a name="related-links"></a>Liens associés

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
