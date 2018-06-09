---
title: Xamarin.Forms MessagingCenter
description: Cet article explique comment utiliser le Xamarin.Forms MessagingCenter pour envoyer et recevoir des messages, afin de réduire le couplage entre classes tels que les modèles d’affichage.
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 71f526f87a2536110a6d2292cd66a0f4d81c0bfc
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240335"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

_Xamarin.Forms inclut un simple service de messagerie pour envoyer et recevoir des messages._

<a name="Overview" />

## <a name="overview"></a>Vue d'ensemble

Xamarin.Forms `MessagingCenter` permet d’afficher des modèles et autres composants de communiquer avec et sans avoir à connaître l’autre en dehors d’un contrat de Message simple.

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>Fonctionne de la MessagingCenter

Il existe en deux parties `MessagingCenter`:

-  **S’abonner** - écouter les messages avec une certaine signature et effectuer une action lorsqu’ils sont reçus. Plusieurs abonnés peuvent être à l’écoute pour le même message.
-  **Envoyer** -publier un message pour les écouteurs sur lequel agir. Si aucun écouteur n’êtes pas abonné le message est ignoré.


Le `MessagingService` est une classe statique avec `Subscribe` et `Send` les méthodes qui sont utilisés dans la solution.

Les messages ont une chaîne `message` paramètre qui est utilisé comme moyen de *adresse* messages. Le `Subscribe` et `Send` méthodes utilisent les paramètres génériques pour contrôler la façon dont les messages sont remis, deux messages avec le même `message` texte mais des arguments de type générique différents ne seront pas remis à l’abonné même.

L’API pour `MessagingCenter` est simple :

-  S’abonner&lt;TSender > (abonné de l’objet, le message de type chaîne, Action&lt;TSender > rappel, une source de TSender = null)
-  S’abonner&lt;TSender, TArgs > (abonné de l’objet, le message de type chaîne, Action&lt;TSender, TArgs > rappel, une source de TSender = null)
-  Envoyer&lt;TSender > (expéditeur TSender, message de type chaîne)
-  Envoyer&lt;TSender, TArgs > (TSender expéditeur du message de type chaîne, TArgs args)
-  Annuler l’abonnement&lt;TSender, TArgs > (abonné de l’objet, un message de type chaîne)
-  Annuler l’abonnement&lt;TSender > (abonné de l’objet, un message de type chaîne)


Ces méthodes sont expliquées ci-dessous.

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>À l’aide de la MessagingCenter

Les messages peuvent être envoyés à la suite de l’interaction utilisateur (par exemple, un clic de bouton), un événement système (comme les contrôles de la modification d’état) ou certains autres incident (par exemple, un téléchargement asynchrone à la fin). Les abonnés peuvent être à l’écoute pour modifier l’apparence de l’interface utilisateur, d’enregistrer des données ou de déclencher une autre opération.

### <a name="simple-string-message"></a>Message de chaîne simple

Le message de la plus simple contient simplement une chaîne dans le `message` paramètre. A `Subscribe` méthode qui *écoute* pour un message de chaîne simple est indiqué ci-dessous - Notez le type générique, en spécifiant l’expéditeur est censé être de type `MainPage`. Toutes les classes dans la solution peuvent s’abonner au message à l’aide de cette syntaxe :

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

Dans le `MainPage` le code suivant de la classe *envoie* le message. Le `this` paramètre est une instance de `MainPage`.

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

La chaîne ne change pas - indique la *type de message* et est utilisé pour déterminer quels abonnés de notification. Ce type de message est utilisé pour indiquer qu’un événement s’est produite, tels que « téléchargement terminé », où aucune autre information n’est requise.

### <a name="passing-an-argument"></a>Passage d’un Argument

Pour passer un argument avec le message, spécifiez l’argument de Type dans le `Subscribe` arguments génériques et dans la signature de l’Action.

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

Pour envoyer le message avec l’argument, incluez le paramètre de Type générique et la valeur de l’argument dans le `Send` appel de méthode.

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

Cet exemple simple utilise un `string` argument, mais n’importe quel objet c# peut être passé.

### <a name="unsubscribe"></a>Annuler l’abonnement

Un objet peut annuler l’abonnement à une signature de message afin qu’aucun futurs messages ne sont remis. Le `Unsubscribe` syntaxe de méthode doit refléter la signature du message (peut donc inclure le paramètre de Type générique pour l’argument de message).

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Le MessagingCenter est un moyen simple pour réduire le couplage, en particulier entre les modèles d’affichage. Il peut être utilisé pour envoyer et recevoir des messages simples ou passez un argument entre les classes. Classes doivent annuler l’abonnement à elles ne souhaitent plus recevoir de messages.


## <a name="related-links"></a>Liens associés

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Exemples Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
