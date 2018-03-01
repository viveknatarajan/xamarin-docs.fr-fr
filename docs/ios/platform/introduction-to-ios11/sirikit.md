---
title: SiriKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/07/2017
ms.openlocfilehash: 0240dd5e381694a31ba9ebb12dd166ca0ef54750
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="sirikit"></a>SiriKit

SiriKit a été introduite dans iOS 10, avec un nombre de domaines de service (y compris sommeil, porté de réservation et effectuer des appels). Reportez-vous à la [SiriKit section](~/ios/platform/sirikit/index.md) pour SiriKit concepts et comment implémenter SiriKit dans votre application.

![Démonstration de liste de tâches Siri](sirikit-images/sirikit.png)

SiriKit dans iOS 11 ajoute ces domaines intentionnels nouveaux et mis à jour :

- [**Répertorie et Notes** ](#listsnotes) – nouveau ! Fournit une API pour les applications pour traiter les tâches et les notes.
- **Codes Visual** – nouveau ! Siri peut afficher des codes QR pour partager des informations de contact ou de participer aux transactions de paiement.
- **Paiements** – ajouté des modes de recherche et de transfert pour les interactions de paiement.
- **Par votre réservation** – Ajout Annuler intentions de gestion et de commentaires.

Autres nouvelles fonctionnalités sont les suivantes :

- [**Les noms d’application autre** ](#alternativenames) – fournit les alias qui aident les clients indiquent Siri à cibler votre application en offrant des noms/prononciation différente.
- **Démarrage de sommeil** – offre la possibilité de démarrer un entraînement en arrière-plan.

Certaines de ces fonctionnalités sont décrites ci-dessous. Pour plus d’informations sur les autres, consultez [documentation de SiriKit d’Apple](https://developer.apple.com/documentation/sirikit).

<a name="listsnotes" />

## <a name="lists-and-notes"></a>Listes et remarques

Le nouveau domaine de listes et les notes fournit une API pour les applications pour traiter les tâches et notes via des requêtes vocal Siri.

**Tâches**

- Avoir un titre et un état d’achèvement.
- Éventuellement inclure une échéance et un emplacement.

**Notes**

- Avoir un titre et un champ de contenu.

Les tâches et les notes peuvent être organisés en groupes. Le reste de cette section décrit comment implémenter ce nouveau domaine avec SiriKit, à l’aide de la [TasksNotes SiriKit exemple](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/).

### <a name="how-to-process-a-sirikit-request"></a>Comment faire pour traiter une demande de SiriKit

Traiter une demande de SiriKit en procédant comme suit :

1. **Résoudre** : valider les paramètres et demander des informations supplémentaires à partir de l’utilisateur (si nécessaire).
2. **Confirmer** – vérification que la demande puisse être traitée et la validation finale.
3. **Gérer** – permet d’effectuer l’opération (mise à jour des données ou effectuer des opérations de réseau).

Les deux premières étapes sont facultatives (bien qu’il est recommandé), et la dernière étape est requise.
Il existe des instructions plus détaillées dans le [SiriKit section](~/ios/platform/sirikit/index.md).

### <a name="resolve-and-confirm-methods"></a>Résoudre et confirmer les méthodes

Ces méthodes facultatives permettent à votre code d’effectuer la validation, sélectionnez valeurs par défaut ou demande des informations supplémentaires à partir de l’utilisateur.

Par exemple, pour le `IINCreateTaskListIntent` interface, la méthode requise est `HandleCreateTaskList`. Il existe quatre méthodes facultatives qui fournissent davantage de contrôle sur l’interaction de Siri :

- `ResolveTitle` – Valide le titre, définit un titre par défaut (le cas échéant) ou signale que les données ne sont pas requises.
- `ResolveTaskTitles` – Valide la liste des tâches parlée par l’utilisateur.
- `ResolveGroupName` – Valide le nom du groupe, choisit un groupe par défaut ou signale que les données ne sont pas requises.
- `ConfirmCreateTaskList` – Valide que votre code peut effectuer l’opération demandée, mais n’effectue pas d’il (uniquement la `Handle*` méthodes doivent modifier les données).

### <a name="handle-the-intent"></a>Gérer l’intention

Il existe six modes dans le domaine de listes et de notes, trois pour les tâches et trois pour les notes.
Les méthodes que vous devez implémenter pour gérer ces modes sont :

- Pour les tâches :
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- Pour les notes :
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

Chaque méthode a un type intention spécifique lui est passé, qui contient toutes les informations Siri a analysé à partir de la demande de l’utilisateur (et éventuellement la mise à jour dans le `Resolve*` et `Confirm*` méthodes).
Votre application doit analyser les données fournies, puis effectuer certaines actions pour stocker ou traitement de données et renvoient un résultat Siri parle et indique à l’utilisateur.

### <a name="response-codes"></a>Codes de réponse

Requis `Handle*` et facultatifs `Confirm*` méthodes indiquent un code de réponse en définissant une valeur sur l’objet qu’elles passent à leur gestionnaire d’achèvement. Réponses proviennent du `INCreateTaskListIntentResponseCode` énumération :

- `Ready` – Retourne pendant la phase de confirmation (ie. à partir d’un `Confirm*` (méthode), mais pas à partir d’un `Handle*` (méthode)).
- `InProgress` – Utilisé pour les tâches longues (par exemple, une opération de serveur/réseau).
- `Success` – Répond avec les détails de l’opération (uniquement à partir d’un `Handle*` méthode).
- `Failure` – Signifie qu’une erreur s’est produite et l’opération n’a pas pu être effectuée.
- `RequiringAppLaunch` : Ne peut pas être traité par l’intention, mais l’opération est possible dans l’application.
- `Unspecified` – Ne pas utiliser : message d’erreur s’affichera à l’utilisateur.

En savoir plus sur ces méthodes et les réponses d’Apple [SiriKit répertorie et notes de documentation](https://developer.apple.com/documentation/sirikit/lists_and_notes).

### <a name="implementing-lists-and-notes"></a>Implémentation de listes et remarques

Le [TasksNotes SiriKit exemple](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/) a été créé à l’aide de la procédure suivante pour ajouter la prise en charge SiriKit à une application iOS vide.

Tout d’abord, pour ajouter la prise en charge SiriKit, procédez comme suit pour votre application iOS :

1. Graduation **SiriKit** dans **Entitlements.plist**.
2. Ajouter le **confidentialité – Description de l’utilisation de Siri** clé à **Info.plist**, ainsi que d’un message pour vos clients.
3. Appelez le `INPreferences.RequestSiriAuthorization` méthode dans l’application, pour inviter l’utilisateur à autoriser les interactions Siri.
4. Ajoutez SiriKit à votre ID d’application sur le portail des développeurs et recréer vos profils de configuration pour inclure le nouveau droit.

Ensuite, ajoutez un nouveau projet d’extension à votre application pour gérer les demandes Siri :

1. Avec le bouton droit sur votre solution et choisissez **Ajouter > Ajouter un nouveau projet...** .
2. Choisissez le **iOS > Extension > intentions Extension** modèle.
3. Deux nouveaux projets seront ajoutés : intention et IntentUI. Personnalisation de l’interface utilisateur est facultatif, l’exemple inclut uniquement le code dans le **intention** projet.

Le projet d’extension est où toutes les demandes SiriKit seront traitées. Comme une extension distincte, il n’a pas automatiquement aucun moyen de communiquer avec votre application principale : il est généralement résolu en implémentant le stockage de fichiers partagés à l’aide de groupes d’applications.

#### <a name="configure-the-intenthandler"></a>Configurer le IntentHandler

Le `IntentHandler` classe est le point d’entrée pour les demandes Siri chaque intention est passée à la `GetHandler` méthode qui retourne un objet qui peut gérer la demande.

Le code suivant montre une implémentation simple :

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

La classe doit hériter de `INExtension`, et parce que l’exemple va à la gestion des listes et notes intentions, il implémente également `IINNotebookDomainHandling`.

> [!NOTE]
> **Remarque à propos d’affectation de noms :** il existe une convention dans .NET pour les interfaces être précédées d’un investissement `I`, qui respecte Xamarin lors de la liaison des protocoles dans le Kit de développement logiciel iOS.
>
> Xamarin conserve également les noms de types à partir d’iOS et Apple utilise les deux premiers caractères dans les noms de type afin de refléter le framework appartenant à un type.
>
> Pour le `Intents` framework, les types sont préfixés avec `IN*` (par exemple). `INExtension`), mais il s’agit _pas_ interfaces.
> Il suit également finissent protocoles (qui deviennent des interfaces en c#) avec deux `I`s, telles que `IINAddTasksIntentHandling`.

#### <a name="handling-intents"></a>Modes de gestion

Chaque objectif (ajouter une tâche, attribut de la tâche, etc.) est implémenté dans une seule méthode similaire à celui illustré ci-dessous. La méthode doit effectuer trois fonctions principales :

1. **Traiter l’intention** : les données analysées par Siri sont mises à disposition dans un `intent` objet spécifique au type de l’intention de. Votre application peut avoir validé ces données à l’aide de facultatif `Resolve*` méthodes.
2. **Valider et mettre à jour le magasin de données** : enregistrer des données sur le système de fichiers (à l’aide de groupes d’applications afin que l’application iOS principal peut également y accéder), ou via une demande réseau.
3. **Fournir une réponse** – utilisez le `completion` gestionnaire envoie une réponse au Siri à afficher en lecture / l’utilisateur :

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

Notez que `null` est passé en tant que second paramètre de la réponse – il s’agit du paramètre de l’activité utilisateur et lorsqu’il n’est pas fourni, une valeur par défaut sera utilisée.
Vous pouvez définir un type d’activité personnalisée, que votre application iOS prend en charge via la `NSUserActivityTypes` clé **Info.plist**. Vous pouvez ensuite gérer ce cas, l’ouverture de votre application et effectuer des opérations spécifiques (par exemple, l’ouverture d’un contrôleur de vue approprié et le chargement des données à partir de l’opération Siri).

L’exemple également code en dur la `Success` résultat, mais dans des scénarios réels, des rapports d’erreur approprié doit être ajouté.

### <a name="test-phrases"></a>Expressions de test

Les expressions de test suivant doivent fonctionner dans l’exemple d’application :

- « Dressez la liste courses avec pommes et poires banane dans TasksNotes »
- « Ajouter la tâche WWDC dans TasksNotes »
- « Ajouter la tâche WWDC à la liste de formation dans TasksNotes »
- « Interrogation assister WWDC comme étant terminées dans TasksNotes »
- « Dans TasksNotes me le rappeler à acheter un iphone ou s’accueil »
- « Interrogation acheter iPhone comme terminées dans TasksNotes »
- « Me le rappeler de laisser accueil à 8 h 00 dans TasksNotes »

![Créer un nouvel exemple de liste](sirikit-images/createtasklist-sml.png) ![Définies pour les tâches en tant qu’exemple complet](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> IOS 11 Simulateur prend en charge les tests avec Siri (contrairement aux versions antérieures).
>
> Si des tests sur les appareils réels, n’oubliez pas de configurer votre ID d’application et prend en charge les profils de configuration pour SiriKit.

<a name="alternativenames" />

## <a name="alternative-names"></a>Autres noms

Cette nouvelle fonctionnalité iOS 11 signifie que vous pouvez configurer d’autres noms pour votre application aider les utilisateurs à déclencher correctement avec Siri. Ajoutez les clés suivantes à la **Info.plist** fichier de projet d’application iOS :

![Info.plist affichant des valeurs et clés de nom d’application de remplacement](sirikit-images/alternative-names.png)

Avec le jeu de noms autre application, les expressions suivantes permettent également de l’exemple d’application (qui est réellement nommé **TasksNotes**) :

- « Créer une liste de courses avec pommes et poires dans banane _MonkeyNotes_»
- « Ajouter des tâches WWDC dans _MonkeyTodo_»


## <a name="troubleshooting"></a>Résolution des problèmes

Certaines erreurs que vous pouvez rencontrer lors de l’exécution de l’exemple ou l’ajout de SiriKit à vos propres applications :

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_Objective-C une exception.  Nom : NSInternalInconsistencyException raison : utilisation de la classe < INPreferences : 0x60400082ff00 > à partir d’une application requiert le com.apple.developer.siri droit. Avez-vous activé la fonctionnalité Siri dans votre projet Xcode ?_

- SiriKit est cochée dans **Entitlements.plist**.
- **Entitlements.plist** est configuré dans le **Options du projet > Générer > iOS signature d’offre groupée**.

  [![Options du projet affichant les que droits correctement définie](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png)

- (pour le déploiement de périphérique) ID de l’application a SiriKit activé et profil de configuration téléchargées.


## <a name="related-links"></a>Liens associés

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [Exemple de SiriKit TasksNotes](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
- [Nouveautés dans SiriKit (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/214/)
