---
title: Communication entre faiblement couplés de composants
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 01669573f215c5a13bb918c9f9ba80aa5ca528c9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="communicating-between-loosely-coupled-components"></a>Communication entre faiblement couplés de composants

La publication-abonnement modèle est un modèle de messagerie dans lequel les serveurs de publication envoyer des messages sans avoir connaissance de toutes les récepteurs, appelés abonnés. De même, les abonnés écoutent les messages spécifiques, sans avoir connaissance d’aucun serveur de publication.

Événements dans .NET implémentent la publication-modèle abonnement, et sont le plus simple et directe approche pour une couche de communication entre les composants si un couplage faible n’est pas requis, tels qu’un contrôle et la page qui la contient. Toutefois, les durées de vie de serveur de publication et l’abonné sont associées par des références d’objet à l’autre, et le type d’abonné doit avoir une référence au type de serveur de publication. Cela peut créer des problèmes de gestion de mémoire en particulier lorsqu’il existe des objets à courte qui s’abonnent à un événement d’un objet statique ou à long terme. Si le Gestionnaire d’événements n’est pas supprimé, l’abonné est maintenu actif par sa référence dans le serveur de publication, et cela empêcher ou retarder le garbage collection de l’abonné.

## <a name="introduction-to-messagingcenter"></a>Introduction aux MessagingCenter

Le Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) classe implémente la publication-modèle, autorisant basée sur message la communication entre les composants qui n’est pas pratique lier par objet et références de type abonnement. Ce mécanisme permet les éditeurs et les abonnés communiquer sans avoir une référence à d’autres, afin de réduire les dépendances entre les composants, en tenant également compte des composants développés et testés indépendamment.

Le [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) classe fournit la multidiffusion de publication / abonnement fonctionnalité. Cela signifie qu’il peut y avoir plusieurs serveurs de publication qui publient un seul message et qu’il peut y avoir plusieurs abonnés à l’écoute pour le même message. La figure 4-1 illustre cette relation :

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "Multidiffusion de publication / abonnement fonctionnalités")

**La figure 4-1 :** multidiffusion de publication / abonnement fonctionnalités

Éditeurs envoient des messages à l’aide de la [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender%7D/p/TSender/System.String/) (méthode), tandis que les abonnés écoutent les messages à l’aide de la [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) (méthode). En outre, les abonnés peuvent également annuler votre abonnement à partir d’abonnements aux messages, si nécessaire, avec le [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender%7D/p/System.Object/System.String/) (méthode).

En interne, le [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) classe utilise des références faibles. Cela signifie qu’elle ne conservera pas les objets actifs et leur permettra d’être nettoyées. Par conséquent, il doit uniquement être nécessaire d’annuler l’abonnement à partir d’un message lorsqu’une classe ne souhaite plus recevoir le message.

Les utilisations d’application mobile eShopOnContainers le [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) composants de classe pour la communication entre faiblement couplés. L’application définit trois messages :

-   Le `AddProduct` message est publié par la `CatalogViewModel` classe lorsqu’un élément est ajouté au panier d’achat. En retour, le `BasketViewModel` classe s’abonne au message et incrémente le nombre d’éléments dans le panier d’achat dans la réponse. En outre, la `BasketViewModel` classe annule également l’abonnement à partir de ce message.
-   Le `Filter` message est publié par la `CatalogViewModel` classe lorsque l’utilisateur applique un filtre de type ou de marque pour les éléments affichés dans le catalogue. En retour, le `CatalogView` classe s’abonne au message et met à jour de l’interface utilisateur afin que seuls les éléments qui correspondent aux critères de filtre sont affichés.
-   Le `ChangeTab` message est publié par le `MainViewModel` classe lorsque les `CheckoutViewModel` accède à la `MainViewModel` après la création a réussi et la soumission d’une nouvelle commande. En retour, le `MainView` classe s’abonne au message et les mises à jour l’interface utilisateur ainsi que les **mon profil** onglet est actif, pour afficher les commandes de l’utilisateur.

> [!NOTE]
> Alors que le [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) classe permet la communication entre les classes de faiblement couplés, elle n’offre pas la solution d’architecture uniquement à ce problème. Par exemple, la communication entre un modèle d’affichage et une vue peut être obtenue par le moteur de liaison et via les notifications de modification de propriété. En outre, la communication entre les deux modèles de vue peut être obtenue en passant des données lors de la navigation.

Dans l’application mobile eShopOnContainers,[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) est utilisé pour mettre à jour dans l’interface utilisateur en réponse à une action qui se produisent dans une autre classe. Par conséquent, les messages sont publiés sur le thread d’interface utilisateur, avec les abonnés qui reçoivent le message sur le même thread.

> [!TIP]
> Marshaler le thread d’interface utilisateur lors de l’exécution de l’interface utilisateur met à jour. Si un message est envoyé à partir d’un thread d’arrière-plan est requis pour mettre à jour l’interface utilisateur, traiter le message sur le thread d’interface utilisateur de l’abonné en appelant le [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) (méthode).

Pour plus d’informations sur [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/), consultez [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).

## <a name="defining-a-message"></a>Définition d’un Message

[`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) les messages sont des chaînes qui sont utilisés pour identifier les messages. L’exemple de code suivant montre les messages définis dans l’application mobile eShopOnContainers :

```csharp
public class MessengerKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

Dans cet exemple, les messages sont définis à l’aide de constantes. L’avantage de cette approche est qu’il fournit la sécurité de type de la compilation et la prise en charge de la refactorisation.

## <a name="publishing-a-message"></a>Publication d’un Message

Éditeurs notifier les abonnés d’un message avec l’un de le [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) surcharges. L’exemple de code suivant illustre la publication la `AddProduct` message :

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

Dans cet exemple, le [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) méthode spécifie trois arguments :

-   Le premier argument spécifie la classe de l’expéditeur. La classe de l’expéditeur doit être spécifiée par les abonnés pour recevoir le message.
-   Le deuxième argument spécifie le message.
-   Le troisième argument spécifie les données de charge utile à envoyer à l’abonné. Dans ce cas les données de la charge utile sont un `CatalogItem` instance.

Le [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) méthode publie le message et ses données de charge utile à l’aide d’une approche d’incendie initiale. Par conséquent, le message est envoyé même si aucun abonné inscrit pour recevoir le message. Dans ce cas, le message envoyé est ignoré.

> [!NOTE]
> Le [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) méthode peut utiliser des paramètres génériques pour contrôler la façon dont les messages sont remis. Par conséquent, plusieurs messages qui partagent une identité de message, mais envoient des types de données de charge utile différents peuvent être reçus par différents abonnés.

## <a name="subscribing-to-a-message"></a>S’abonner à un Message

Les abonnés peuvent s’inscrire pour recevoir un message à l’aide d’un de le [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) surcharges. L’exemple de code suivant montre comment l’application mobile eShopOnContainers s’abonne à et traite, le `AddProduct` message :

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

Dans cet exemple, le [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) méthode s’abonne à la `AddProduct` , le message et exécute un délégué de rappel en réponse à la réception du message. Ce délégué de rappel spécifié comme une expression lambda, exécute le code qui met à jour l’interface utilisateur.

> [!TIP]
> Envisagez d’utiliser des données de charge utile immuable. N’essayez pas de modifier les données de charge utile à partir d’un délégué de rappel parce que plusieurs threads Impossible d’accéder simultanément aux données reçues. Dans ce scénario, les données de charge doivent être immuables pour éviter les erreurs d’accès concurrentiel.

Un abonné n’est peut-être pas nécessaire de gérer chaque instance d’un message publié, et cela peut être contrôlé par les arguments de type générique sont spécifiés sur le [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) (méthode). Dans cet exemple, l’abonné reçoit uniquement `AddProduct` les messages envoyés à partir de la `CatalogViewModel` (classe), dont les données charge utile sont un `CatalogItem` instance.

## <a name="unsubscribing-from-a-message"></a>Interruption de l’abonnement à partir d’un Message

Les abonnés peuvent vous désabonner qu’ils ne souhaitent plus recevoir de messages. Cela est possible avec l’un de le [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) surcharges, comme illustré dans l’exemple de code suivant :

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

Dans cet exemple, le [ `Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) syntaxe de méthode reflète les arguments de type spécifiés lors de l’abonnement pour recevoir le `AddProduct` message.

## <a name="summary"></a>Récapitulatif

Le Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) classe implémente la publication-modèle, autorisant basée sur message la communication entre les composants qui n’est pas pratique lier par objet et références de type abonnement. Ce mécanisme permet les éditeurs et les abonnés communiquer sans avoir une référence à d’autres, afin de réduire les dépendances entre les composants, en tenant également compte des composants développés et testés indépendamment.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
