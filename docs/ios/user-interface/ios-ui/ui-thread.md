---
title: Utilisation avec le Thread d’interface utilisateur
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 72f161001509519fb02a652f23eaa7805a55f7ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-the-ui-thread"></a>Utilisation avec le Thread d’interface utilisateur

Interfaces utilisateur des applications sont toujours monothread, même pour les périphériques multithreads : il n'existe qu’une seule représentation sous forme de l’écran et toute modification apportée à ce qui est affiché doivent être coordonnées via un seul « point d’accès ». Ainsi, plusieurs threads d’essayer de mettre à jour le pixel même en même temps (par exemple).

Votre code doit uniquement des threads de modifications apportées aux contrôles d’interface à partir de la main de l’utilisateur (ou l’interface utilisateur). Les mises à jour de l’interface utilisateur qui se produisent sur un autre thread (par exemple, un thread d’arrière-plan ou de rappel) ne peuvent pas s’affichent à l’écran, ou peuvent même provoquer un blocage.

## <a name="ui-thread-execution"></a>Exécution du Thread d’interface utilisateur

Lorsque vous créer des contrôles dans une vue, ou gestion d’un événement initiée par l’utilisateur, par exemple une touche, le code s’exécute déjà dans le contexte du thread d’interface utilisateur.

Si le code s’exécute sur un thread d’arrière-plan, dans une tâche ou un rappel il est probablement ne pas l’exécution sur le thread d’interface utilisateur. Dans ce cas, vous devez encapsuler le code dans un appel à `InvokeOnMainThread` ou `BeginInvokeOnMainThread` comme suit :

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

Le `InvokeOnMainThread` méthode est définie sur `NSObject` afin qu’il peut être appelée à partir de méthodes définies sur n’importe quel objet UIKit (par exemple, une vue ou une vue contrôleur).

Lors du débogage des applications de Xamarin.iOS, une erreur est levée si votre code essaie d’accéder à un contrôle d’interface utilisateur à partir d’un thread incorrect. Cela vous aide à détecter et résoudre ces problèmes avec la méthode InvokeOnMainThread. Cela seulement se produit pendant le débogage et ne lève pas d’une erreur dans les versions release. Le message d’erreur apparaîtra comme suit :

 ![](ui-thread-images/image10.png "Exécution du Thread d’interface utilisateur")

 <a name="Background_Thread_Example" />


## <a name="background-thread-example"></a>Exemple de Thread d’arrière-plan

Voici un exemple qui tente d’accéder à un contrôle d’interface utilisateur (un `UILabel`) à partir d’un thread d’arrière-plan à l’aide d’un thread simple :

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

Code lèvera le `UIKitThreadAccessException` pendant le débogage. Pour résoudre le problème (et vous assurer que le contrôle d’interface utilisateur est uniquement accessible à partir du thread d’interface utilisateur), encapsuler tout code qui fait référence à des contrôles d’interface utilisateur à l’intérieur d’un `InvokeOnMainThread` expression comme suit :

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

Vous ne devez utiliser cette option pour le reste des exemples de ce document, mais il est un concept important à retenir lors de votre application effectue des demandes de réseau, utilise le centre de notifications ou d’autres méthodes qui requièrent un gestionnaire d’achèvement qui s’exécute sur un autre thread.

 <a name="Async_Await_Example" />


## <a name="asyncawait-example"></a>Exemple d’asynchrones / d’attente

Lorsque vous utilisez les mots clés de c# 5 asynchrones / d’attente `InvokeOnMainThread` n’est pas nécessaire car lorsqu’une tâche attendue se termine la méthode continue sur le thread appelant.

Cet exemple de code (qui attend sur un appel de méthode délai purement à des fins de démonstration) présente une méthode asynchrone qui est appelée sur le thread d’interface utilisateur (il est un gestionnaire de TouchUpInside). Étant donné que le contenu dans la méthode est appelée sur le thread d’interface utilisateur, l’interface utilisateur opérations telles que la définition du texte sur un `UILabel` ou l’affichage d’un `UIAlertView` peut être appelée en toute sécurité une fois les opérations asynchrones terminées sur les threads d’arrière-plan.

```csharp
async partial void button2_TouchUpInside (UIButton sender)
{
    textfield1.ResignFirstResponder ();
    textfield2.ResignFirstResponder ();
    textview1.ResignFirstResponder ();
    label1.Text = "async method started";
    await Task.Delay(1000); // example purpose only
    label1.Text = "1 second passed";
    await Task.Delay(2000);
    label1.Text = "2 more seconds passed";
    await Task.Delay(1000);
    new UIAlertView("Async method complete", "This method", 
               null, "Cancel", null)
        .Show();
    label1.Text = "async method completed";
}
```

Si une méthode async est appelée à partir d’un thread d’arrière-plan (pas le thread d’interface utilisateur principal) puis `InvokeOnMainThread` seraient nécessaires.


## <a name="related-links"></a>Liens associés

- [Contrôles (exemple)](https://developer.xamarin.com/samples/Controls/)
- [Thread](~/ios/app-fundamentals/threading.md)
