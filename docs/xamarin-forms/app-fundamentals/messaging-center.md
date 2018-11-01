---
title: Xamarin.Forms MessagingCenter
description: Cet article explique comment utiliser le Xamarin.Forms MessagingCenter pour envoyer et recevoir des messages, à réduire le couplage entre les classes telles que des modèles de vue.
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 7fef4443cacba0fa8bdb8d5df070c4244730b4f5
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675172"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

_Xamarin.Forms inclut un service de messagerie simple pour envoyer et recevoir des messages._

<a name="Overview" />

## <a name="overview"></a>Vue d'ensemble

Xamarin.Forms `MessagingCenter` permet d’afficher des modèles et autres composants de communiquer avec sans avoir rien à savoir sur eux en dehors d’un contrat de Message simple.

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>Fonctionne de la MessagingCenter

Il existe deux parties `MessagingCenter`:

-  **S’abonner** : écouter les messages avec une certaine signature et effectuer une action lorsqu’ils sont reçus. Plusieurs abonnés peuvent être à l’écoute pour le même message.
-  **Envoyer** -publier un message pour les écouteurs sur lequel agir. Si aucun écouteur n’êtes abonné le message est ignoré.


Le `MessagingService` est une classe statique avec `Subscribe` et `Send` méthodes qui sont utilisées tout au long de la solution.

Messages ont une chaîne `message` paramètre qui est utilisé comme moyen de *adresse* messages. Le `Subscribe` et `Send` méthodes utilisent des paramètres génériques pour contrôler la façon dont les messages sont remis - deux messages possédant le même `message` texte mais des arguments de type générique différents ne seront pas remis à l’abonné même.

L’API pour `MessagingCenter` est simple :

- `Subscribe<TSender> (object subscriber, string message, Action<TSender> callback, TSender source = null)`
- `Subscribe<TSender, TArgs> (object subscriber, string message, Action<TSender, TArgs> callback, TSender source = null)`
- `Send<TSender> (TSender sender, string message)`
- `Send<TSender, TArgs> (TSender sender, string message, TArgs args)`
- `Unsubscribe<TSender, TArgs> (object subscriber, string message)`
- `Unsubscribe<TSender> (object subscriber, string message)`

Ces méthodes sont expliquées ci-dessous.

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>À l’aide de la MessagingCenter

Les messages peuvent être envoyés suite à l’interaction utilisateur (par exemple, un clic de bouton), un événement du système (tels que les contrôles de changement d’état) ou un autre incident (par exemple, un téléchargement asynchrone à la fin). Les abonnés peuvent être à l’écoute pour modifier l’apparence de l’interface utilisateur, d’enregistrer des données ou de déclencher une autre opération.

### <a name="simple-string-message"></a>Message de chaîne simple

Le message la plus simple contient simplement une chaîne dans le `message` paramètre. Un `Subscribe` méthode qui *écoute* pour un message de chaîne simple est illustré ci-dessous - Notez que le type générique en spécifiant l’expéditeur est censé être de type `MainPage`. Toutes les classes dans la solution peuvent s’abonner au message à l’aide de cette syntaxe :

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

Dans le `MainPage` le code suivant de la classe *envoie* le message. Le `this` paramètre est une instance de `MainPage`.

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

La chaîne ne change pas - indique la *type de message* et est utilisé pour déterminer quels abonnés à notifier. Ce type de message est utilisé pour indiquer qu’un événement s’est produite, par exemple « chargement terminé », où aucune information supplémentaire n’est requise.

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

Cet exemple simple utilise un `string` argument, mais les C# objet peut être transmis.

### <a name="unsubscribe"></a>Annuler l’abonnement

Un objet peut annuler l’abonnement à partir d’une signature de message afin qu’aucun futurs messages ne soient remis. Le `Unsubscribe` syntaxe de méthode doit refléter la signature du message (par conséquent, peut-être inclure le paramètre de Type générique pour l’argument de message).

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Le MessagingCenter est un moyen simple de réduire le couplage, en particulier entre les modèles de vue. Il peut être utilisé pour envoyer et recevoir des messages simples ou passer un argument entre les classes. Classes doivent annuler l’abonnement à partir d’elles ne souhaitent plus recevoir de messages.


## <a name="related-links"></a>Liens associés

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Exemples Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
