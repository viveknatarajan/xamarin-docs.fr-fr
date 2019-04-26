---
title: Gestion des fragments
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: 3e0430b8ed9c42030441021e71c3b08b1ddccc57
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61022196"
---
# <a name="managing-fragments"></a>Gestion des fragments

Pour faciliter la gestion des Fragments, Android fournit la `FragmentManager` classe. Chaque activité possède une instance de `Android.App.FragmentManager` chargé de rechercher ou modifier dynamiquement ses Fragments. Chaque ensemble de ces modifications est appelé un *transaction*et est effectuée en utilisant l’une des API contenues dans la classe `Android.App.FragmentTransation`, qui est gérée par le `FragmentManager`. Une activité peut démarrer une transaction comme suit :

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

Ces modifications sur les Fragments sont effectuées dans le `FragmentTransaction` instance à l’aide de méthodes telles que `Add()`, `Remove(),` et `Replace().` les modifications sont ensuite appliquées à l’aide de `Commit()`. Les modifications dans une transaction ne sont pas effectuées immédiatement.
Au lieu de cela, elles sont planifiées pour s’exécuter dès que possible sur le thread d’interface utilisateur de l’activité.

L’exemple suivant montre comment ajouter un Fragment à un conteneur existant :

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

Si une transaction est validée après `Activity.OnSaveInstanceState()` est appelée, une exception sera levée. Cela se produit, car lorsque l’activité enregistre son état, Android enregistre également l’état de tous les Fragments hébergés. Si toutes les transactions Fragment sont validées après ce stade, l’état de ces transactions seront perdue lors de la restauration de l’activité.

Il est possible d’enregistrer les transactions de Fragment à l’activité [pile back](https://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) en effectuant un appel à `FragmentTransaction.AddToBackStack()`. Cela permet à l’utilisateur de naviguer vers l’arrière via Fragment change lorsque le **retour** bouton est enfoncé. Sans un appel à cette méthode, les Fragments sont supprimés seront détruits et ne sera pas disponibles si l’utilisateur accède par le biais de l’activité.

L’exemple suivant montre comment utiliser le `AddToBackStack` méthode d’un `FragmentTransaction` pour remplacer un Fragment, tout en conservant l’état du premier Fragment sur la pile de retour :

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// Replace the fragment that is in the View fragment_container (if applicable).
fragmentTx.Replace(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Add the transaction to the back stack.
fragmentTx.AddToBackStack(null);

// Commit the transaction.
fragmentTx.Commit();
```


## <a name="communicating-with-fragments"></a>Communication avec des Fragments

Le *FragmentManager* connaît tous les Fragments qui sont attachés à une activité et propose deux méthodes pour vous aider à trouver ces Fragments :

-   **FindFragmentById** &ndash; cette méthode trouve un Fragment à l’aide de l’ID a été spécifié dans le fichier de disposition ou l’ID de conteneur lorsque le Fragment a été ajouté en tant que partie d’une transaction.

-   **FindFragmentByTag** &ndash; cette méthode est utilisée pour rechercher un Fragment qui a une balise qui a été fourni dans le fichier de disposition ou qui a été ajoutée dans une transaction.

Référence à la fois des Fragments et des activités le `FragmentManager`, de sorte que les mêmes techniques sont utilisés pour communiquer dans les deux sens entre eux. Une application peut rechercher une Fragment de référence en utilisant l’une de ces deux méthodes, convertir cette référence vers le type approprié et appeler directement des méthodes sur le Fragment. L’extrait de code suivant fournit un exemple :

Il est également possible pour l’activité à utiliser le `FragmentManager` pour trouver les Fragments :

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```


### <a name="communicating-with-the-activity"></a>Communication avec l’activité

Il est possible pour un Fragment d’utiliser le `Fragment.Activity` propriété à référencer son hôte. En effectuant un cast de l’activité à un type plus spécifique, il est possible pour une activité pour appeler des méthodes et propriétés sur son ordinateur hôte, comme indiqué dans l’exemple suivant :

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
