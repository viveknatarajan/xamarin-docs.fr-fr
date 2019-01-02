---
title: Affichage des fenêtres contextuelles
description: 'Xamarin.Forms fournit deux éléments d’interface utilisateur de type fenêtre contextuelle : une alerte et une feuille d’action. Cet article explique comment utiliser les API de feuille d’action et d’alerte pour poser des questions simples aux utilisateurs et pour les guider lors de l’exécution de tâches.'
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 1334340f18e664d4c652803e7678f45ee942eea8
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057403"
---
# <a name="displaying-pop-ups"></a>Affichage des fenêtres contextuelles

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)

_Xamarin.Forms fournit deux éléments d’interface utilisateur de type fenêtre contextuelle : une alerte et une feuille d’action. Cet article explique comment utiliser les API de feuille d’action et d’alerte pour poser des questions simples aux utilisateurs et pour les guider lors de l’exécution de tâches._

Afficher une alerte ou demander à un utilisateur de faire un choix est une tâche d’interface utilisateur courante. Xamarin.Forms propose deux méthodes dans la classe [`Page`](xref:Xamarin.Forms.Page) pour interagir avec l’utilisateur par le biais d’une fenêtre contextuelle : [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) et [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*). Elles sont affichées avec les contrôles natifs appropriés sur chaque plateforme.

## <a name="displaying-an-alert"></a>Affichage d’une alerte

Toutes les plateformes prises en charge par Xamarin.Forms ont une fenêtre contextuelle modale pour alerter l’utilisateur ou lui poser des questions simples. Pour afficher ces alertes dans Xamarin.Forms, utilisez la méthode [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) sur n’importe quelle [`Page`](xref:Xamarin.Forms.Page). La ligne de code suivante présente un message simple à l’utilisateur :

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Boîte de dialogue d’alerte avec un bouton")

Cet exemple ne recueille aucune information de l’utilisateur. L’alerte s’affiche de façon modale, et une fois qu’elle est fermée l’utilisateur continue à interagir avec l’application.

Vous pouvez également utiliser la méthode [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) pour capturer la réponse d’un utilisateur en présentant deux boutons et en retournant une valeur `boolean`. Pour obtenir une réponse à partir d’une alerte, fournissez du texte pour les deux boutons et attendez (`await`) la méthode. Une fois que l’utilisateur a sélectionné l’une des options, la réponse est retournée à votre code. Notez les mots clés `async` et `await` dans l’exemple de code ci-dessous :

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "Boîte de dialogue d’alerte avec deux boutons")](pop-ups-images/alert2.png#lightbox "Boîte de dialogue d’alerte avec deux boutons")

## <a name="guiding-users-through-tasks"></a>Assistance aux utilisateurs lors de l’exécution de tâches

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) est un élément d’interface utilisateur courant dans iOS. La méthode [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) Xamarin.Forms vous permet d’inclure ce contrôle dans les applications multiplateformes et d’afficher des alternatives natives dans Android et UWP.

Pour afficher une feuille d’action, attendez (`await`) [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) dans n’importe quelle [`Page`](xref:Xamarin.Forms.Page), en passant les étiquettes de message et de bouton sous forme de chaînes. La méthode retourne l’étiquette de chaîne du bouton sur lequel l’utilisateur a cliqué. Voici un exemple simple :

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "Boîte de dialogue ActionSheet")

Le bouton `destroy` est restitué différemment des autres et peut être laissé `null` ou spécifié en tant que troisième paramètre de chaîne. L’exemple suivant utilise le bouton `destroy` :

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "Boîte de dialogue de feuille d’action avec bouton destroy")](pop-ups-images/action2.png#lightbox "Boîte de dialogue de feuille d’action avec bouton destroy")

## <a name="summary"></a>Récapitulatif

Dans cet article, vous avez vu comment utiliser les API de feuille d’action et d’alerte pour poser des questions simples aux utilisateurs et pour les guider lors de l’exécution de tâches. Xamarin.Forms propose deux méthodes dans la classe [`Page`](xref:Xamarin.Forms.Page) pour interagir avec l’utilisateur par le biais d’une fenêtre contextuelle : [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) et [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*). Toutes deux sont affichées avec les contrôles natifs appropriés sur chaque plateforme.



## <a name="related-links"></a>Liens associés

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
