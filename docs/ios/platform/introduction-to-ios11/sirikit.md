---
title: Mises à jour de SiriKit dans iOS 11
description: Ce document décrit comment utiliser SiriKit dans iOS 11. En particulier, il examine comment travailler avec les tâches et les notes de publication et comment fournir des noms de remplacement pour une application.
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/07/2017
ms.openlocfilehash: 7e895dc2865880ec2789a40f8cdf047a20f8693b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61400304"
---
# <a name="sirikit-updates-in-ios-11"></a>Mises à jour de SiriKit dans iOS 11

SiriKit a été introduit dans iOS 10, avec un nombre de domaines de service (y compris le sommeil, tour de réservation et effectuer des appels). Reportez-vous à la [SiriKit section](~/ios/platform/sirikit/index.md) pour les concepts de SiriKit et l’implémentation de SiriKit dans votre application.

![Démonstration de liste de tâches de Siri](sirikit-images/sirikit.png)

SiriKit dans iOS 11 ajoute ces domaines intentions nouveaux et mis à jour :

- [**Répertorie et note** ](#listsnotes) – nouveau ! Fournit une API pour les applications pour traiter les tâches et notes de publication.
- **Les Codes Visual** – nouveau ! Siri peut afficher des codes QR pour partager des informations de contact ou de participer aux transactions de paiement.
- **Paiements** – ajouter des intentions de recherche et de transfert pour les interactions de paiement.
- **Puisse surfer sur la réservation** – Ajout Annuler intentions tour et commentaires.

Quelques autres nouvelles fonctions :

- [**Les noms d’application autre** ](#alternativenames) – fournit les alias qui aident les clients dire Siri à cibler votre application en offrant des noms prononciations.
- **Démarrage de sommeil** – offre la possibilité de démarrer un entraînement en arrière-plan.

Certaines de ces fonctionnalités sont expliquées ci-dessous. Pour plus d’informations sur les autres, reportez-vous à [documentation de SiriKit d’Apple](https://developer.apple.com/documentation/sirikit).

<a name="listsnotes" />

## <a name="lists-and-notes"></a>Listes et les notes de publication

Le nouveau domaine de listes et les notes de publication fournit une API pour les applications pour traiter les tâches et notes de publication via les demandes vocales Siri.

**Tâches**

- Avoir un titre et un état d’achèvement.
- Si vous le souhaitez inclure une échéance et un emplacement.

**Notes**

- Avoir un titre et un champ de contenu.

Tâches et notes de publication peuvent être organisés en groupes. Le reste de cette section décrit comment implémenter ce nouveau domaine avec SiriKit, à l’aide de la [TasksNotes SiriKit exemple](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/).

### <a name="how-to-process-a-sirikit-request"></a>Comment traiter une demande de SiriKit

Traiter une demande de SiriKit en suivant ces étapes :

1. **Résoudre** : valider les paramètres et demander des informations supplémentaires à partir de l’utilisateur (si nécessaire).
2. **Confirmer** – validation finale et la vérification que la demande peut être traitée.
3. **Gérer** – permet d’effectuer l’opération (la mise à jour des données ou d’opérations de réseau).

Les deux premières étapes sont facultatives (bien qu’il est recommandé), et la dernière étape est nécessaire.
Il existe des instructions plus détaillées dans le [SiriKit section](~/ios/platform/sirikit/index.md).

### <a name="resolve-and-confirm-methods"></a>Résoudre et confirmer les méthodes

Ces méthodes facultatives permettent à votre code d’effectuer la validation, sélectionnez Paramètres par défaut ou demande des informations supplémentaires à partir de l’utilisateur.

Par exemple, pour le `IINCreateTaskListIntent` interface, la méthode requise est `HandleCreateTaskList`. Il existe quatre méthodes facultatives qui fournissent davantage de contrôle sur l’interaction de Siri :

- `ResolveTitle` – Valide le titre, définit le titre par défaut (le cas échéant) ou signale que les données ne sont pas requises.
- `ResolveTaskTitles` – Valide la liste des tâches parlée par l’utilisateur.
- `ResolveGroupName` – Valide le nom du groupe, choisit un groupe par défaut ou signale que les données ne sont pas requises.
- `ConfirmCreateTaskList` – Valide le fait que votre code peut effectuer l’opération demandée, mais n’est pas effectue (uniquement la `Handle*` méthodes doivent modifier des données).

### <a name="handle-the-intent"></a>Gérer l’intention

Il existe six intentions dans le domaine de listes et des notes, trois pour les tâches et trois pour les notes de publication.
Les méthodes que vous devez implémenter pour gérer ces intentions sont :

- Pour les tâches :
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- Pour accéder aux notes :
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

Chaque méthode a un type intention spécifique lui est passé, qui contient toutes les informations Siri a analysé à partir de la demande de l’utilisateur (et éventuellement la mise à jour dans le `Resolve*` et `Confirm*` méthodes).
Votre application doit analyser les données fournies, puis effectuer certaines actions pour stocker ou traitement de données et retournent un résultat qui parle de Siri et présente à l’utilisateur.

### <a name="response-codes"></a>Codes de réponse

Requis `Handle*` et facultatifs `Confirm*` méthodes indiquent un code de réponse en définissant une valeur sur l’objet qu’il transmet à son gestionnaire d’achèvement. Réponses proviennent du `INCreateTaskListIntentResponseCode` énumération :

- `Ready` – Retourne pendant la phase de confirmation (ie. à partir d’un `Confirm*` (méthode), mais pas à partir d’un `Handle*` (méthode)).
- `InProgress` – Utilisé pour les tâches longues (par exemple, une opération de serveur/réseau).
- `Success` – Répond avec les détails de l’opération réussie (uniquement à partir d’un `Handle*` méthode).
- `Failure` – Signifie qu’une erreur s’est produite et que l’opération n’a pas pu être effectuée.
- `RequiringAppLaunch` : Ne peut pas être traité par l’intention, mais l’opération est possible dans l’application.
- `Unspecified` – Ne pas utiliser : message d’erreur s’affichera à l’utilisateur.

En savoir plus sur ces méthodes et des réponses dans Apple [SiriKit répertorie et notes de documentation](https://developer.apple.com/documentation/sirikit/lists_and_notes).

### <a name="implementing-lists-and-notes"></a>Implémentation de listes et les notes de publication

Le [TasksNotes SiriKit exemple](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/) a été créé à l’aide de la procédure suivante pour ajouter la prise en charge de SiriKit à une application iOS vide.

Tout d’abord, pour ajouter la prise en charge de SiriKit, suivez ces étapes pour votre application iOS :

1. Graduation **SiriKit** dans **Entitlements.plist**.
2. Ajouter le **confidentialité – Description de l’utilisation de Siri** clé **Info.plist**, ainsi que d’un message pour vos clients.
3. Appelez le `INPreferences.RequestSiriAuthorization` méthode dans l’application, pour inviter l’utilisateur à autoriser des interactions de Siri.
4. Ajoutez SiriKit à votre ID d’application sur le portail des développeurs et recréez vos profils de provisionnement pour inclure le nouveau droit.

Ensuite, ajoutez un nouveau projet d’extension à votre application pour traiter les requêtes Siri :

1. Avec le bouton droit sur votre solution et choisissez **Ajouter > Ajouter un nouveau projet...** .
2. Choisissez le **iOS > Extension > Extension Intents** modèle.
3. Deux nouveaux projets vont être ajoutées : Intention et IntentUI. Personnalisation de l’interface est facultative, donc l’échantillon inclut uniquement le code dans le **intention** projet.

Le projet d’extension est où toutes les demandes de SiriKit seront traitées. Comme aucune extension distincte, il n’a pas automatiquement n’importe quel moyen de communiquer avec votre application principale, il est généralement résolu en implémentant le stockage de fichiers partagés à l’aide de groupes d’applications.

#### <a name="configure-the-intenthandler"></a>Configurer le IntentHandler

Le `IntentHandler` classe est le point d’entrée pour les requêtes Siri – chaque objectif est passé à la `GetHandler` (méthode), qui retourne un objet qui peut gérer la demande.

Le code ci-dessous montre une implémentation simple :

```csharp
[Register("IntentHandler")]
public partial class IntentHandler : INExtension, IINNotebookDomainHandling
{
  protected IntentHandler(IntPtr handle) : base(handle)
  {}
  public override NSObject GetHandler(INIntent intent)
  {
    // This is the default implementation.  If you want different objects to handle different intents,
    // you can override this and return the handler you want for that particular intent.
    return this;
  }
  // add intent handlers here!
}
```

La classe doit hériter `INExtension`, et étant donné que l’exemple est destinée à la gestion des listes et intentions de notes, elle implémente également `IINNotebookDomainHandling`.

> [!NOTE]
> - Il existe une convention dans .NET pour les interfaces à faire précéder avec une majuscule `I`, qui respecte le Xamarin lors de la liaison des protocoles dans le SDK iOS.
> - Xamarin conserve également les noms de types à partir d’iOS et Apple utilise les deux premiers caractères dans les noms de type afin de refléter le framework appartenant à un type.
> - Pour le `Intents` framework, les types sont précédés de `IN*` (par exemple). `INExtension`), mais il s’agit de _pas_ interfaces.
> - Elle suit également cette protocoles (qui deviennent des interfaces dans C#) vous retrouver avec deux `I`s, telles que `IINAddTasksIntentHandling`.

#### <a name="handling-intents"></a>Gestion des intentions

Chaque objectif (ajouter une tâche, définir l’attribut de tâche, etc.) est implémenté dans une seule méthode similaire à celui illustré ci-dessous. La méthode doit effectuer trois fonctions principales :

1. **Traiter l’intention** – les données analysées par Siri mises à disposition dans un `intent` spécifique au type d’intention d’objet. Votre application peut avoir validé ces données à l’aide de facultatif `Resolve*` méthodes.
2. **Valider et mettre à jour le magasin de données** , enregistrez les données vers le système de fichiers (à l’aide de groupes d’applications afin que l’application iOS principale peut également y accéder), ou via une demande réseau.
3. **Fournir une réponse** – utilisez le `completion` gestionnaire envoie une réponse au Siri à lecture/affichage à l’utilisateur :

```csharp
public void HandleCreateTaskList(INCreateTaskListIntent intent, Action<INCreateTaskListIntentResponse> completion)
{
  var list = TaskList.FromIntent(intent);
  // TODO: have to create the list and tasks... in your app data store
  var response = new INCreateTaskListIntentResponse(INCreateTaskListIntentResponseCode.Success, null)
  {
    CreatedTaskList = list
  };
  completion(response);
}
```

Notez que `null` est passée comme deuxième paramètre à la réponse – il s’agit du paramètre d’activité utilisateur, et lorsqu’il n’est pas fourni, une valeur par défaut sera utilisée.
Vous pouvez définir un type d’activité personnalisé tant que votre application iOS prend en charge par le biais de la `NSUserActivityTypes` clé dans **Info.plist**. Vous pouvez ensuite gérer ce cas, lorsque votre application est ouverte et effectuer des opérations spécifiques (par exemple, l’ouverture d’un contrôleur d’affichage approprié et le chargement des données à partir de l’opération de Siri).

L’exemple de code en dur d’également le `Success` résultat, mais dans des scénarios réels, des rapports d’erreur approprié doit être ajouté.

### <a name="test-phrases"></a>Expressions de test

Les expressions de test suivant doivent fonctionner dans l’exemple d’application :

- « Vérifiez une liste de courses avec pommes et bananes poires dans TasksNotes »
- « Ajouter la tâche WWDC dans TasksNotes »
- « Ajouter la tâche WWDC à la liste de formation dans TasksNotes »
- « Marque assister WWDC comme étant terminées dans TasksNotes »
- « Dans TasksNotes me le rappeler d’acheter un iphone ou s’accueil »
- « Interrogation acheter iPhone comme terminés dans TasksNotes »
- « Me le rappeler à laisser accueil à 8 h 00 dans TasksNotes »

![Créer un exemple de liste de](sirikit-images/createtasklist-sml.png) ![Tâche de jeu en tant qu’exemple complet](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> IOS 11 Simulateur prend en charge les tests avec Siri (contrairement aux versions antérieures).
>
> Si un test sur des appareils réels, n’oubliez pas de configurer votre ID d’application et profils de provisionnement pour SiriKit prennent en charge.

<a name="alternativenames" />

## <a name="alternative-names"></a>Autres noms

Cette nouvelle fonctionnalité d’iOS 11 signifie que vous pouvez configurer d’autres noms pour votre application aider les utilisateurs à déclencher correctement avec Siri. Ajoutez les clés suivantes à la **Info.plist** fichier de projet d’application iOS :

![Info.plist affichant des valeurs et clés de nom d’application autre](sirikit-images/alternative-names.png)

Avec l’ensemble de noms autre application, les expressions suivantes sont également valables pour l’exemple d’application (qui est réellement nommé **TasksNotes**) :

- « Rendre une liste de courses avec pommes et bananes poires dans _MonkeyNotes_»
- « Ajouter des tâches WWDC dans _MonkeyTodo_»


## <a name="troubleshooting"></a>Résolution des problèmes

Certaines erreurs que vous pouvez rencontrer lors de l’exemple en cours d’exécution ou l’ajout de SiriKit à vos propres applications :

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_Objective-C une exception.  Nom : NSInternalInconsistencyException raison : Utilisation de la classe < INPreferences : 0x60400082ff00 > à partir d’une application requiert le droit com.apple.developer.siri. Avez-vous activé la fonctionnalité Siri dans votre projet Xcode ?_

- SiriKit est coché dans **Entitlements.plist**.
- **Entitlements.plist** est configuré dans le **Options du projet > Build > signature du Bundle iOS**.

  [![Options du projet montrant les que droits correctement définie](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png#lightbox)

- (pour le déploiement de périphérique) ID d’application a SiriKit activé et profil de provisionnement téléchargé.


## <a name="related-links"></a>Liens associés

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [Exemple de SiriKit TasksNotes](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
- [Quelles sont les nouveautés dans SiriKit (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/214/)
