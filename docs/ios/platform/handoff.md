---
title: Procédure de transfert dans Xamarin.iOS
description: Cet article couvre utilisation de remise dans une application Xamarin.iOS à transférer des activités des utilisateurs entre les applications en cours d’exécution sur l’utilisateur d’autres dispositifs.
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ec324e8fb8327b622424311b89567608311a6a19
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787518"
---
# <a name="handoff-in-xamarinios"></a>Procédure de transfert dans Xamarin.iOS

_Cet article couvre utilisation de remise dans une application Xamarin.iOS à transférer des activités des utilisateurs entre les applications en cours d’exécution sur l’utilisateur d’autres dispositifs._

Apple a introduit le transfert dans iOS 8 et OS X Yosemite (10.10) pour fournir un mécanisme commun pour l’utilisateur à transférer des activités a démarré sur l’un de leurs appareils, sur un autre appareil exécutant la même application ou une autre application qui prend en charge la même activité.

[![](handoff-images/handoff02.png "Un exemple d’une opération de transfert")](handoff-images/handoff02.png#lightbox)

Cet article prend un coup de œil rapide à l’activation d’activité partage dans une application Xamarin.iOS et couvrent l’infrastructure de remise en détail :

## <a name="about-handoff"></a>À propos de la procédure de transfert

Procédure de transfert (également appelée la continuité des activités) a été introduite par Apple dans iOS 8 et OS X Yosemite (10.10) comme un moyen de l’utilisateur démarrer une activité sur l’un de leurs appareils (iOS ou Mac) et continuer à cette même activité sur un autre de leurs appareils (telles qu’identifiées par iClou de l’utilisateur d compte).

Procédure de transfert a été développé dans iOS 9 prend également en charge, les fonctionnalités de recherche améliorées. Pour plus d’informations, consultez notre [recherche améliorations](~/ios/platform/search/index.md) documentation.

Par exemple, l’utilisateur peut démarrer un e-mail sur leur iPhone et continuer sans interruption de l’adresse de messagerie sur leur Mac, avec toutes les mêmes informations de message renseigné et le curseur dans le même emplacement qu’ils laissé dans iOS.

Une de vos applications qui partagent le même _ID d’équipe_ sont éligibles à l’aide de la procédure de transfert à poursuivre utilisateur entre les applications, que ces applications sont remis via l’iTunes App Store ou signé par un développeur inscrit (pour Mac, entreprise ou des applications Ad Hoc).

N’importe quel `NSDocument` ou `UIDocument` applications disposent automatiquement de remise prennent en charge intégrée et nécessitent des modifications minimales pour prendre en charge la remise.

### <a name="continuing-user-activities"></a>Activités des utilisateurs continue

Le `NSUserActivity` classe (ainsi que certaines modifications mineures à `UIKit` et `AppKit`) fournit la prise en charge pour la définition d’activité d’un utilisateur qui peut potentiellement être poursuivie sur un autre des périphériques de l’utilisateur.

Pour une activité être transféré vers un autre des périphériques de l’utilisateur, il doit être encapsulé dans une instance `NSUserActivity`, marquée en tant que le _activité en cours_, ont la charge utile est définie (les données utilisées pour effectuer la continuation) et le activité doit ensuite être transmise à ce périphérique.

Procédure de transfert transmet au minimum d’informations pour définir l’activité continue avec les paquets de données de grande taille en cours de synchronisation via iCloud.

Sur le périphérique de réception, l’utilisateur recevra une notification qu’une activité est disponible pour la continuation. Si l’utilisateur choisit de continuer l’activité sur le nouveau périphérique, l’application spécifiée est lancée (pas déjà en cours d’exécution) et la charge utile de la `NSUserActivity` est utilisée pour redémarrer l’activité.

[![](handoff-images/handoffinteractions.png "Une vue d’ensemble des activités de l’utilisateur continue")](handoff-images/handoffinteractions.png#lightbox)

Seules les applications qui partagent le même ID d’équipe de développeur et de répondant à une donnée _Type d’activité_ sont éligibles pour la continuation. Une application définit les Types d’activité pris en charge sous le `NSUserActivityTypes` clé de sa **Info.plist** fichier. Compte tenu de cela, un appareil continu choisit l’application pour effectuer la continuation en fonction de l’ID d’équipe, Type d’activité et éventuellement le _titre de l’activité_.

L’application de réception utilise les informations de la `NSUserActivity`de `UserInfo` dictionnaire qui permet de configurer son interface utilisateur et restaurer l’état de l’activité donnée afin que la transition apparaît transparente pour l’utilisateur final.

Si la continuation nécessite plus d’informations que celles qui peuvent être envoyées efficacement à un `NSUserActivity`, la reprise d’application peut envoyer un appel à l’application d’origine et établir un ou plusieurs flux pour transmettre les données requises. Par exemple, si l’activité a été modification d’un document de texte de grande taille avec plusieurs images, diffusion en continu doit transférer les informations nécessaires pour continuer l’activité sur le périphérique de réception. Pour plus d’informations, consultez la [prenant en charge les flux de Continuation](#Supporting-Continuation-Streams) section ci-dessous.

Comme indiqué précédemment, `NSDocument` ou `UIDocument` applications disposent automatiquement de remise intégrées de prendre en charge. Pour plus d’informations, consultez la [prise en charge de la remise dans les applications basées sur le Document](#Supporting-Handoff-in-Document-Based-Apps) section ci-dessous.

### <a name="the-nsuseractivity-class"></a>La classe NSUserActivity

La `NSUserActivity` classe est l’objet principal dans un échange de remise et est utilisé pour encapsuler l’état d’une activité utilisateur qui est disponible pour la continuation. Instancie une copie d’une application `NSUserActivity` toute activité, il prend en charge et souhaite poursuivre sur un autre périphérique. Par exemple, éditeur de document créerait une activité pour chaque document ouvert. Toutefois, que le document de premier plan (affiché dans la fenêtre de premier plan ou un onglet) est la _activité en cours_ et, par conséquent, disponibles pour la continuation.

Une instance de `NSUserActivity` est identifié par les deux sa `ActivityType` et `Title` propriétés. Le `UserInfo` propriété du dictionnaire est utilisée pour transporter les informations sur l’état de l’activité. Définir le `NeedsSave` propriété `true` si vous souhaitez différée à charger les informations d’état via le `NSUserActivity`du délégué. Utilisez le `AddUserInfoEntries` méthode pour fusionner les nouvelles données à partir d’autres clients dans le `UserInfo` dictionnaire comme requis pour conserver l’état de l’activité.

### <a name="the-nsuseractivitydelegate-class"></a>La classe NSUserActivityDelegate

Le `NSUserActivityDelegate` est utilisé pour conserver les informations dans un `NSUserActivity`de `UserInfo` dictionnaire à jour et synchronisé avec l’état actuel de l’activité. Lorsque le système a besoin des informations dans l’activité de mise à jour (tels qu’avant la continuation sur un autre périphérique), il appelle la `UserActivityWillSave` méthode du délégué.

Vous devez implémenter la `UserActivityWillSave` (méthode) et apportez les modifications à la `NSUserActivity` (tel que `UserInfo`, `Title`, etc.) pour vous assurer qu’il reflète toujours l’état de l’activité actuelle. Lorsque le système appelle le `UserActivityWillSave` (méthode), la `NeedsSave` indicateur sera effacé. Si vous modifiez les propriétés de données de l’activité, vous devez définir `NeedsSave` à `true` à nouveau.

Au lieu d’utiliser le `UserActivityWillSave` méthode présentée ci-dessus, vous pouvez éventuellement laisser `UIKit` ou `AppKit` gérer l’activité des utilisateurs automatiquement. Pour ce faire, définissez l’objet de répondeur `UserActivity` propriété et implémenter le `UpdateUserActivityState` (méthode). Consultez le [prenant en charge la remise de répondeurs](#Supporting-Handoff-in-Responders) section ci-dessous pour plus d’informations.

### <a name="app-framework-support"></a>Prise en charge de l’infrastructure d’application

Les deux `UIKit` (iOS) et `AppKit` (OS X) fournissent une prise en charge intégrée pour la remise dans le `NSDocument`, répondeur (`UIResponder`/`NSResponder`), et `AppDelegate` classes. Chaque système d’exploitation implémente remise légèrement différemment, le mécanisme de base et les API sont les mêmes.

#### <a name="user-activities-in-document-based-apps"></a>Activités des utilisateurs dans les applications basées sur le Document

IOS de basé sur le document et les applications du système d’exploitation X disposent automatiquement de prise en charge intégrée de remise. Pour activer cette prise en charge, vous devez ajouter une `NSUbiquitousDocumentUserActivityType` clé et une valeur pour chaque `CFBundleDocumentTypes` entrée dans l’application **Info.plist** fichier.

Si cette clé est présente, les deux `NSDocument` et `UIDocument` créer automatiquement `NSUserActivity` instances pour les documents iCloud du type spécifié. Vous devez fournir un type d’activité pour chaque type de document qui prend en charge de l’application et plusieurs types de documents peuvent utiliser le même type d’activité. Les deux `NSDocument` et `UIDocument` remplir automatiquement la `UserInfo` propriété de la `NSUserActivity` avec leurs `FileURL` valeur de la propriété.

Sur OS X, le `NSUserActivity` gérés par `AppKit` et associés à des répondeurs automatiquement devenir l’activité actuelle lorsque la fenêtre de document devient la fenêtre principale. Sur iOS, pour `NSUserActivity` objets gérés par `UIKit`, vous devez appeler `BecomeCurrent` méthode explicitement ou du document `UserActivity` propriété définie sur une `UIViewController` lorsque l’application s’affiche au premier plan.

`AppKit` restaure automatiquement les `UserActivity` propriété créée de cette façon sur OS X. Cela se produit si le `ContinueUserActivity` méthode retourne `false` ou si elle n’est pas implémentée. Dans ce cas, le document est ouvert avec le `OpenDocument` méthode de la `NSDocumentController` et il recevra ensuite un `RestoreUserActivityState` appel de méthode.

Consultez le [prise en charge de la remise dans les applications basées sur le Document](#Supporting-Handoff-in-Document-Based-Apps) section ci-dessous pour plus d’informations.

#### <a name="user-activities-and-responders"></a>Répondeurs et les activités des utilisateurs

Les deux `UIKit` et `AppKit` peut gérer automatiquement une activité de l’utilisateur si vous le définissez comme un objet de répondeur `UserActivity` propriété. Si l’état a été modifié, vous devez définir le `NeedsSave` propriété du répondeur `UserActivity` à `true`. Le système enregistre automatiquement le `UserActivity` à la demande, après avoir donné le temps de répondeur à mettre à jour l’état en appelant son `UpdateUserActivityState` (méthode).

Si plusieurs répondeurs partagent un seul `NSUserActivity` instance, ils reçoivent un `UpdateUserActivityState` rappel lorsque le système met à jour l’objet d’activité utilisateur. Le répondeur doit appeler le `AddUserInfoEntries` pour mettre à jour le `NSUserActivity`de `UserInfo` dictionnaire afin de refléter l’état actuel de l’activité à ce stade. Le `UserInfo` dictionnaire est effacé avant chaque `UpdateUserActivityState` appeler.

Pour dissocier lui-même à partir d’une activité, un répondeur peut définir ses `UserActivity` propriété `null`. Lorsqu’une infrastructure d’application gérée `NSUserActivity` instance n’a pas plus associés répondeurs ou les documents, il est automatiquement invalidé.

Consultez le [prenant en charge la remise de répondeurs](#Supporting-Handoff-in-Responders) section ci-dessous pour plus d’informations.

#### <a name="user-activities-and-the-appdelegate"></a>Les activités des utilisateurs et le AppDelegate

De votre application `AppDelegate` est son point d’entrée principal lors du traitement d’une continuation de remise. Lorsque l’utilisateur répond à une notification de remise, l’application appropriée est lancée (si ce n’est pas déjà en cours d’exécution) et la `WillContinueUserActivityWithType` méthode de la `AppDelegate` sera appelée. À ce stade, l’application doit informer l’utilisateur qui démarre la continuation.

Le `NSUserActivity` instance est remise lorsque le `AppDelegate`de `ContinueUserActivity` méthode est appelée. À ce stade, vous devez configurer l’interface utilisateur de l’application et continuer l’activité donnée.

Consultez le [mise en œuvre de remise](#Implementing-Handoff) section ci-dessous pour plus d’informations.

## <a name="enabling-handoff-in-a-xamarin-app"></a>L’activation de remise dans une application Xamarin

En raison des exigences de sécurité imposées par la procédure de transfert, une application Xamarin.iOS qui utilise l’infrastructure de remise doit être configurée correctement dans les deux le portail des développeurs Apple et dans le fichier de projet Xamarin.iOS.

Effectuez ce qui suit :

1. Connectez-vous à la [portail des développeurs Apple](http://developer.apple.com).
2. Cliquez sur **certificats, les identificateurs et les profils**.
3. Si vous n’avez pas déjà fait, cliquez sur **identificateurs** et créer un ID de votre application (par exemple, `com.company.appname`), sinon modifier votre code existant.
4. Vérifiez que le **iCloud** service a été activé pour l’ID donné : 

    [![](handoff-images/provision01.png "Activer le service iCloud pour l’ID donné")](handoff-images/provision01.png#lightbox)
5. Enregistrez les modifications apportées.
4. Cliquez sur **profils de configuration** > **développement** et créer un profil de configuration pour vous de développement nouvelle application : 

    [![](handoff-images/provision02.png "Créer un nouveau développement, profil de configuration pour l’application")](handoff-images/provision02.png#lightbox)
5. Télécharger et installer le profil de configuration ou utiliser Xcode pour télécharger et installer le profil.
6. Modifier les options de votre projet de Xamarin.iOS et vérifiez que vous utilisez le profil de configuration que vous venez de créer : 

    [![](handoff-images/provision03.png "Sélectionnez le profil de configuration venez de créer")](handoff-images/provision03.png#lightbox)
7. Ensuite, modifiez votre **Info.plist** de fichier et vérifiez que vous utilisez l’ID d’application qui a été utilisé pour créer le profil de configuration : 

    [![](handoff-images/provision04.png "Définir l’ID de l’application")](handoff-images/provision04.png#lightbox)
8. Faites défiler vers le **Modes d’arrière-plan** section et vérifiez les éléments suivants : 

    [![](handoff-images/provision05.png "Activer les modes d’arrière-plan requis")](handoff-images/provision05.png#lightbox)
9. Enregistrez les modifications à tous les fichiers.

Avec ces paramètres en place, l’application est maintenant prête à accéder aux API d’infrastructure de remise. Pour plus d’informations sur la configuration, consultez notre [vos appareils](~/ios/get-started/installation/device-provisioning/index.md) et [de configuration de votre application](~/ios/get-started/installation/device-provisioning/index.md) guides.

## <a name="implementing-handoff"></a>Implémentation de remise

Les activités des utilisateurs peuvent être poursuivies parmi les applications qui sont signées avec le même ID d’équipe de développeur et prend en charge le même Type d’activité. Implémentation de remise dans une application Xamarin.iOS vous oblige à créer un objet d’activité utilisateur (soit dans `UIKit` ou `AppKit`), mettre à jour l’état d’objet pour effectuer le suivi de l’activité et en continuant de l’activité sur un périphérique de réception.

### <a name="identifying-user-activities"></a>Identifier les activités des utilisateurs

La première étape de l’implémentation de remise consiste à identifier les types d’activités des utilisateurs qui prend en charge de votre application, et indique quelle de ces activités est de bons candidats à la continuation sur un autre périphérique. Par exemple : une application de tâches peut-être prendre en charge la modification d’éléments comme un _Type d’activité utilisateur_et prennent en charge la navigation de la liste des éléments disponibles en tant qu’un autre.

Une application peut créer autant utilisateur Types d’activité sont requis, un pour n’importe quelle fonction qui fournit de l’application. Pour chaque Type d’activité utilisateur, l’application doit suivre lorsqu’une activité de type commence et se termine et ont besoin de conserver les informations d’état à jour pour continuer cette tâche sur un autre périphérique.

Les activités des utilisateurs peuvent être poursuivies sur n’importe quelle application signée avec le même ID d’équipe sans un mappage entre les applications émettrices et réceptrices. Par exemple, une application donnée peut créer quatre types d’activités, qui sont utilisées par différents, et les applications sur un autre périphérique. Il s’agit d’une occurrence courante entre la version Mac de l’application (qui peut avoir plusieurs fonctions et fonctionnalités) et des applications iOS, où chaque application est plus petit et se concentrent sur une tâche spécifique.

### <a name="creating-activity-type-identifiers"></a>Création d’identificateurs de Type d’activité

Le _identificateur de Type d’activité_ est une chaîne courte ajoutée à la `NSUserActivityTypes` tableau de l’application **Info.plist** fichier utilisé pour identifier un Type d’activité utilisateur donné. Il y aura une entrée dans le tableau pour chaque activité qui prend en charge de l’application. Apple suggère à l’aide d’une notation DNS inverse-style pour l’identificateur de Type d’activité pour éviter les collisions. Par exemple : `com.company-name.appname.activity` activités basées sur pour une application spécifique ou `com.company-name.activity` pour les activités qui peuvent s’exécuter sur plusieurs applications.

L’identificateur de Type d’activité est utilisée lorsque vous créez un `NSUserActivity` instance pour identifier le type d’activité. Lors de la poursuite d’une activité sur un autre périphérique, le Type d’activité (avec l’ID d’équipe de l’application) détermine quelle application pour lancer pour continuer l’activité.

Par exemple, nous allons créer un exemple d’application appelé **MonkeyBrowser** ([télécharger ici](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)). Cette application présente quatre onglets, chacun avec un open URL différents dans une vue de navigateur web. L’utilisateur pourra continuer à n’importe quel onglet sur un appareil iOS autre exécution de l’application.

Pour créer les identificateurs de Type d’activité requis pour prendre en charge ce comportement, modifiez le **Info.plist** de fichiers et de basculer vers le **Source** vue. Ajouter un `NSUserActivityTypes` de clé et de créer les identificateurs suivants :

[![](handoff-images/type01.png "Les identificateurs requis dans l’éditeur plist NSUserActivityTypes clé")](handoff-images/type01.png#lightbox)

Nous avons créé quatre nouveaux identificateurs de Type d’activité, un pour chacun des onglets dans l’exemple **MonkeyBrowser** application. Lorsque vous créez vos propres applications, remplacez le contenu de la `NSUserActivityTypes` de tableau avec les identificateurs de Type d’activité spécifiques aux activités de votre application prend en charge.

### <a name="tracking-user-activity-changes"></a>Le suivi des modifications d’activité utilisateur

Lorsque nous créons une nouvelle instance de la `NSUserActivity` (classe), nous spécifions un `NSUserActivityDelegate` instance pour effectuer le suivi des modifications à l’état de l’activité. Par exemple, le code suivant peut être utilisé pour effectuer le suivi des modifications d’état :

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;

namespace MonkeyBrowse
{
    public class UserActivityDelegate : NSUserActivityDelegate
    {
        #region Constructors
        public UserActivityDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void UserActivityReceivedData (NSUserActivity userActivity, NSInputStream inputStream, NSOutputStream outputStream)
        {
            // Log
            Console.WriteLine ("User Activity Received Data: {0}", userActivity.Title);
        }

        public override void UserActivityWasContinued (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity Was Continued: {0}", userActivity.Title);
        }

        public override void UserActivityWillSave (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity will be Saved: {0}", userActivity.Title);
        }
        #endregion
    }
}
```

Le `UserActivityReceivedData` méthode est appelée quand un flux de Continuation a reçu des données à partir d’un périphérique d’envoi. Pour plus d’informations, consultez la [prenant en charge les flux de Continuation](#Supporting-Continuation-Streams) section ci-dessous.

Le `UserActivityWasContinued` méthode est appelée quand un autre périphérique a repris d’une activité à partir de l’appareil en cours. Selon le type d’activité, telles que l’ajout d’un nouvel élément dans une liste de tâches, l’application peut devez annuler l’activité sur le périphérique d’envoi.

Le `UserActivityWillSave` méthode est appelée avant toute modification apportée à l’activité est enregistrées et synchronisées entre les appareils disponibles localement. Vous pouvez utiliser cette méthode pour apporter des modifications de dernière minute pour le `UserInfo` propriété de le `NSUserActivity` avant qu’il est envoyé de l’instance.

### <a name="creating-a-nsuseractivity-instance"></a>Création d’une Instance NSUserActivity

Chaque activité de votre application souhaite fournir la possibilité de continuer sur un autre appareil doit être encapsulée dans un `NSUserActivity` instance. L’application peut créer autant d’activités en fonction des besoins et la nature de ces activités est dépendante de la fonctionnalité et les fonctionnalités de l’application en question. Par exemple, une application de messagerie peut créer une activité pour la création d’un nouveau message et l’autre pour la lecture d’un message.

Pour notre exemple d’application, un nouveau `NSUserActivity` est créé chaque fois que l’utilisateur entre une nouvelle URL dans un de l’affichage du navigateur web à onglets. Le code suivant stocke l’état d’un onglet donné :

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSUserActivity UserActivity { get; set; }
...

UserActivity = new NSUserActivity (UserActivityTab1);
UserActivity.Title = "Weather Tab";
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Il crée un nouveau `NSUserActivity` à l’aide de Type d’activité utilisateur créé ci-dessus et fournit un titre explicite pour l’activité. Il se connecte à une instance de la `NSUserActivityDelegate` créé ci-dessus à surveiller l’état change et informe iOS que cette activité de l’utilisateur est l’activité en cours.

### <a name="populating-the-userinfo-dictionary"></a>Remplir le dictionnaire UserInfo

Comme nous l’avons vu ci-dessus, le `UserInfo` propriété de la `NSUserActivity` classe est un `NSDictionary` de paires clé-valeur utilisées pour définir l’état d’une activité donnée. Les valeurs stockées dans `UserInfo` doit être un des types suivants : `NSArray`, `NSData`, `NSDate`, `NSDictionary`, `NSNull`, `NSNumber`, `NSSet`, `NSString`, ou `NSURL`. `NSURL` les valeurs de données qui pointent vers des documents iCloud seront automatiquement ajustées afin qu’ils pointent vers les mêmes documents sur un périphérique de réception.

Dans l’exemple ci-dessus, nous avons créé un `NSMutableDictionary` de l’objet et il rempli avec une clé unique fournissant l’URL de l’utilisateur a été affichée sous l’onglet donné. Le `AddUserInfoEntries` méthode de l’activité de l’utilisateur a été utilisée pour mettre à jour de l’activité avec les données qui permet de restaurer l’activité sur le périphérique de réception :

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple vous suggérons de conserver les informations envoyées au strict minimum pour vous assurer que l’activité est envoyée dans un délai raisonnable pour le périphérique de réception. Si les informations supérieure sont requises, comme modifier une image attachée à un document doit être envoyé, le vous devez utiliser des flux de Continuation. Consultez le [prenant en charge les flux de Continuation](#Supporting-Continuation-Streams) section ci-dessous pour plus de détails.

### <a name="continuing-an-activity"></a>Poursuite d’une activité

Procédure de transfert informe automatiquement local appareils iOS et OS X à proximité physique de l’appareil d’origine et signés dans le même compte iCloud, de la disponibilité des activités des utilisateurs non bloquantes. Si l’utilisateur choisit de continuer une activité sur un nouveau périphérique, le système lance l’application appropriée (selon l’ID de l’équipe et le Type d’activité) et les informations son `AppDelegate` que la continuation doit se produire.

Tout d’abord, la `WillContinueUserActivityWithType` méthode est appelée afin de l’application peut informer l’utilisateur que la continuation est sur le point de commencer. Nous utilisons le code suivant dans le **AppDelegate.cs** fichier de notre exemple d’application pour gérer une continuation à partir :

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSString UserActivityTab2 = new NSString ("com.xamarin.monkeybrowser.tab2");
public NSString UserActivityTab3 = new NSString ("com.xamarin.monkeybrowser.tab3");
public NSString UserActivityTab4 = new NSString ("com.xamarin.monkeybrowser.tab4");
...

public FirstViewController Tab1 { get; set; }
public SecondViewController Tab2 { get; set;}
public ThirdViewController Tab3 { get; set; }
public FourthViewController Tab4 { get; set; }
...

public override bool WillContinueUserActivity (UIApplication application, string userActivityType)
{
    // Report Activity
    Console.WriteLine ("Will Continue Activity: {0}", userActivityType);

    // Take action based on the user activity type
    switch (userActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Inform view that it's going to be modified
        Tab1.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Inform view that it's going to be modified
        Tab2.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Inform view that it's going to be modified
        Tab3.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Inform view that it's going to be modified
        Tab4.PreparingToHandoff ();
        break;
    }

    // Inform system we handled this
    return true;
}
```

Dans l’exemple ci-dessus, chaque contrôleur d’affichage est inscrit avec le `AppDelegate` et a un public `PreparingToHandoff` méthode qui affiche un indicateur d’activité et un message informe l’utilisateur que l’activité est sur le point d’être remis à l’appareil en cours. Exemple :

```csharp
private void ShowBusy(string reason) {

    // Display reason
    BusyText.Text = reason;

    //Define Animation
    UIView.BeginAnimations("Show");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0.5f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PreparingToHandoff() {
    // Inform caller
    ShowBusy ("Continuing Activity...");
}
```

Le `ContinueUserActivity` de la `AppDelegate` sera appelée pour continuer en fait de l’activité donnée. Là encore, à partir de notre exemple d’application :

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

Public `PerformHandoff` méthode de chaque contrôleur d’affichage valide la remise réellement et restaure l’activité sur l’appareil en cours. Dans le cas de l’exemple, il affiche la même URL dans un onglet donné a été de navigation de l’utilisateur sur un autre appareil. Exemple :

```csharp
private void HideBusy() {

    //Define Animation
    UIView.BeginAnimations("Hide");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PerformHandoff(NSUserActivity activity) {

    // Hide busy indicator
    HideBusy ();

    // Extract URL from dictionary
    var url = activity.UserInfo ["Url"].ToString ();

    // Display value
    URL.Text = url;

    // Display the give webpage
    WebView.LoadRequest(new NSUrlRequest(NSUrl.FromString(url)));

    // Save activity
    UserActivity = activity;
    UserActivity.BecomeCurrent ();

}
```

Le `ContinueUserActivity` méthode inclut un `UIApplicationRestorationHandler` que vous pouvez appeler pour le document ou le répondeur en fonction de la reprise d’activité. Vous devez passer un `NSArray` ou des objets qui peuvent être restaurés vers le Gestionnaire de restauration lorsqu’elle est appelée. Exemple :

```csharp
completionHandler (new NSObject[]{Tab4});
```

Pour chaque objet passé, son `RestoreUserActivityState` méthode sera appelée. Chaque objet peut ensuite utiliser les données dans le `UserInfo` dictionnaire qui permet de restaurer son propre état. Exemple :

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

Pour les applications basées sur le document, si vous n’implémentez pas la `ContinueUserActivity` (méthode), ou il retourne `false`, `UIKit` ou `AppKit` peut reprendre automatiquement l’activité. Consultez le [prise en charge de la remise dans les applications basées sur le Document](#Supporting-Handoff-in-Document-Based-Apps) section ci-dessous pour plus d’informations.

### <a name="failing-handoff-gracefully"></a>Échec de remise en douceur

Procédure de transfert s’appuie sur la transmission des informations entre un collection faiblement connecté appareils iOS et OS X, le processus de transfert peut échouer. Vous devez concevoir votre application de gérer ces défaillances naturellement et informer l’utilisateur de toute situation qui surviennent.

En cas de défaillance, le `DidFailToContinueUserActivitiy` méthode de la `AppDelegate` sera appelée. Exemple :

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

Vous devez utiliser fourni `NSError` pour fournir des informations à l’utilisateur sur l’échec.

## <a name="native-app-to-web-browser-handoff"></a>Application native pour la remise de navigateur Web

Un utilisateur peut souhaiter continuer une activité sans avoir une application native appropriée installée sur le périphérique souhaité. Dans certaines situations, une interface basée sur le web peut fournir la fonctionnalité requise et l’activité peut toujours être poursuivie. Par exemple, le compte de messagerie peut fournir une interface utilisateur de base de web pour composer et lire des messages.

Si l’application d’origine, native connaît l’URL de l’interface web (et la syntaxe requise pour identifier l’élément en cours de suite), il peut encoder ces informations dans le `WebpageURL` propriété de la `NSUserActivity` instance. Si le périphérique de réception n’a pas une application native appropriée installée pour gérer la continuation, l’interface web fourni peut être appelée.

## <a name="web-browser-to-native-app-handoff"></a>Navigateur Web pour la remise d’application Native

Si l’utilisateur utilise une interface basée sur le web sur le périphérique d’origine, et une application native sur le périphérique de réception de revendications la partie domaine de le `WebpageURL` propriété, puis le système utilisera cette application, le handle de la continuation. Reçoit le nouveau périphérique un `NSUserActivity` instance qui marque le Type d’activité en tant que `BrowsingWeb` et `WebpageURL` contient l’URL de la visite de l’utilisateur, le `UserInfo` dictionnaire sera vide.

Pour une application de participer à ce type de remise, elle doit demander le domaine dans un `com.apple.developer.associated-domains` droit avec le format `<service>:<fully qualified domain name>` (par exemple : `activity continuation:company.com`).

Si le domaine spécifié correspond à un `WebpageURL` la valeur de la propriété, procédure de transfert télécharge la liste des applications approuvées ID depuis le site Web de ce domaine. Le site Web doit fournir une liste d’ID approuvées dans un fichier JSON signé nommé **apple-app-site-association** (par exemple, `https://company.com/apple-app-site-association`).

Ce fichier JSON contient un dictionnaire qui spécifie une liste d’ID d’application sous la forme `<team identifier>.<bundle identifier>`. Exemple :

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

Pour signer le fichier JSON (afin qu’il puisse le bon `Content-Type` de `application/pkcs7-mime`), utilisez la **Terminal** application et un `openssl` avec un certificat et la clé émis par une autorité de certification approuvée par iOS (consultez [ http://support.apple.com/kb/ht5012 ](http://support.apple.com/kb/ht5012) pour obtenir la liste). Exemple :

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

Le `openssl` commande génère un fichier JSON signé que vous placez sur votre site Web à la **apple-app-site-association** URL. Exemple :

```csharp
https://example.com/apple-app-site-association.
```

L’application ne recevra pas les activités dont `WebpageURL` domaine est dans son `com.apple.developer.associated-domains` droit. Uniquement les `http` et `https` protocoles sont pris en charge, un autre protocole lève une exception.

## <a name="supporting-handoff-in-document-based-apps"></a>Prise en charge de la remise dans les applications basées sur le Document

Comme indiqué précédemment, sur iOS et OS X, les applications basées sur le document prendra automatiquement en charge le transfert de documents basés sur iCloud si l’application **Info.plist** fichier contient un `CFBundleDocumentTypes` clé de `NSUbiquitousDocumentUserActivityType`. Exemple :

```xml
<key>CFBundleDocumentTypes</key>
<array>
    <dict>
        <key>CFBundleTypeName</key>
        <string>NSRTFDPboardType</string>
        . . .
        <key>LSItemContentTypes</key>
        <array>
        <string>com.myCompany.rtfd</string>
        </array>
        . . .
        <key>NSUbiquitousDocumentUserActivityType</key>
        <string>com.myCompany.myEditor.editing</string>
    </dict>
</array>
```

Dans cet exemple, la chaîne est un indicateur d’application DNS inverse avec le nom de l’activité ajoutée. Si l’entrée de cette façon, les entrées de type d’activité n’avez pas besoin être répété dans le `NSUserActivityTypes` tableau de la **Info.plist** fichier.

L’objet de l’activité des utilisateurs créés automatiquement (disponibles par le biais du document `UserActivity` propriété) peuvent être référencés par d’autres objets dans l’application et utilisés pour restaurer l’état de continuation. Par exemple, pour suivre les éléments sélection et document de position. Vous devez définir ce activités `NeedsSave` propriété `true` chaque fois que l’état change et mettre à jour le `UserInfo` dictionnaire dans le `UpdateUserActivityState` (méthode).

Le `UserActivity` propriété peut être utilisée à partir de n’importe quel thread et est conforme au protocole clé-valeur observation (KVO), donc il peut être utilisé pour synchroniser un document lors de leur déplacement et l’extraction d’iCloud. Le `UserActivity` propriété sera invalidée lorsque le document est fermé.

Pour plus d’informations, consultez le site d’Apple [prise en charge des activités utilisateur dans les applications basées sur le Document](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) documentation.

## <a name="supporting-handoff-in-responders"></a>Prise en charge de la remise dans des répondeurs

Vous pouvez associer des répondeurs (hérité de le `UIResponder` sur iOS ou `NSResponder` sur OS X) à des activités en définissant leurs `UserActivity` propriétés. Le système enregistre automatiquement le `UserActivity` propriété à la fois, l’appel du répondeur `UpdateUserActivityState` pour ajouter des données actuelles à l’objet de l’activité des utilisateurs en utilisant le `AddUserInfoEntriesFromDictionary` (méthode).

## <a name="supporting-continuation-streams"></a>Prise en charge des flux de Continuation

Les situations où la quantité d’informations nécessaires pour continuer une activité ne peut pas être transférée efficacement par la charge utile de transfert initiale. Dans ces situations, l’application réceptrice peut établir un ou plusieurs flux de données entre lui-même et l’application d’origine pour transférer les données.

Définit l’application d’origine le `SupportsContinuationStreams` propriété de la `NSUserActivity` instance `true`. Exemple :

```csharp
// Create a new user Activity to support this tab
UserActivity = new NSUserActivity (ThisApp.UserActivityTab1){
    Title = "Weather Tab",
    SupportsContinuationStreams = true
};
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

L’application réceptrice peut ensuite appeler la `GetContinuationStreams` méthode de la `NSUserActivity` dans son `AppDelegate` pour établir le flux de données. Exemple :

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

Sur le périphérique d’origine, le délégué d’activité utilisateur reçoit les flux de données en appelant son `DidReceiveInputStream` méthode pour fournir les données demandées pour continuer l’activité des utilisateurs sur le périphérique de reprise.

Vous allez utiliser un `NSInputStream` pour fournir un accès en lecture seule pour transmettre des données et un `NSOutputStream` fournir un accès en écriture seule. Les flux de données doit être utilisé dans une demande et réponse de manière, où l’application de réception demande davantage de données et lui fournit l’application d’origine. Afin que les données écrites dans le flux de sortie sur le périphérique d’origine sont en lecture à partir du flux d’entrée sur l’appareil continue et vice versa.

Même dans les situations où la Continuation de flux de données est requise, il doit être un nombre minimal du et les deux sens la communication entre les deux applications.

Pour plus d’informations, consultez Apple [à l’aide des flux de Continuation](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) documentation.

## <a name="handoff-best-practices"></a>Meilleures pratiques de remise

La mise en œuvre réussie de la continuation transparente d’une activité de l’utilisateur via la procédure de transfert requiert une conception minutieuse en raison de l’ensemble des composants impliqués. Apple suggère l’adoption de l’appliquer les meilleures pratiques pour vos applications de remise activé :

- Concevez vos activités de l’utilisateur pour exiger la charge utile de plus petite possible d’associer l’état de l’activité de se poursuivre. Plus la charge utile, plus il faut la continuation à démarrer.
- Si vous devez transférer de grandes quantités de données pour une continuation réussie, prendre en compte les coûts impliqués dans la configuration et la charge réseau.
- Il est courant pour une application Mac volumineuse créer des activités des utilisateurs qui sont gérés par plusieurs, plus petites, les applications spécifiques à une tâche sur les appareils iOS. L’autre application et les versions de système d’exploitation doivent être conçues pour bien fonctionner ensemble ou échouer correctement.
- Lorsque vous spécifiez vos Types d’activité, utilisez la notation DNS inverse pour éviter les collisions. Si une activité est spécifique à une application donnée, son nom doit être inclus dans la définition de type (par exemple `com.myCompany.myEditor.editing`). Si l’activité peut fonctionner dans plusieurs applications, supprimez le nom de l’application à partir de la définition (par exemple `com.myCompany.editing`).
- Si votre application a besoin pour mettre à jour l’état d’une activité de l’utilisateur (`NSUserActivity`) définir la `NeedsSave` propriété `true`. Au moment opportun, remise appellera du délégué `UserActivityWillSave` méthode, vous pouvez mettre à jour le `UserInfo` dictionnaire en fonction des besoins.
- Étant donné que le processus de remise ne peut pas initialiser immédiatement sur le périphérique de réception, vous devez implémenter la `AppDelegate`de `WillContinueUserActivity` et informe les utilisateurs qu’une continuation est prêt à démarrer.

## <a name="example-handoff-app"></a>Exemple d’application remise

Ainsi, à l’aide de la procédure de transfert dans une application Xamarin.iOS, nous avons inclus le [ **MonkeyBrowser** ](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/) exemple d’application avec ce guide. L’application comporte quatre onglets que l’utilisateur peut utiliser pour parcourir le web, chacun avec un type d’activité donnée : météo, Favoris, pause café et travail.

Sur un onglet, lorsque l’utilisateur entre une URL et les drainages de nouveau la **accédez** bouton, un nouveau `NSUserActivity` est créé pour cet onglet qui contient l’URL de l’utilisateur parcourt actuellement :

[![](handoff-images/handoff01.png "Exemple d’application remise")](handoff-images/handoff01.png#lightbox)

Si un autre des périphériques de l’utilisateur a le **MonkeyBrowser** application installée, iCloud utilisant le même compte d’utilisateur est connecté, se trouve sur le même réseau et à proximité du périphérique ci-dessus, l’activité de remise s’affichera sur la page d’accueil écran (dans le coin inférieur gauche) :

[![](handoff-images/handoff02.png "L’activité de remise affiché sur l’écran d’accueil dans le coin inférieur gauche")](handoff-images/handoff02.png#lightbox)

Si l’utilisateur fait glisser vers le haut sur l’icône de transfert, l’application est lancée et l’activité de l’utilisateur spécifié dans le `NSUserActivity` sera poursuivie sur le nouveau périphérique :

[![](handoff-images/handoff03.png "Suite de l’activité des utilisateurs sur le nouveau périphérique")](handoff-images/handoff03.png#lightbox)

Lorsque l’activité des utilisateurs a été correctement envoyée à un autre appareil Apple, le périphérique d’envoi `NSUserActivity` recevra un appel à la `UserActivityWasContinued` méthode sur son `NSUserActivityDelegate` pour lui indiquer que l’activité des utilisateurs a été transférée vers un autre périphérique.

## <a name="summary"></a>Récapitulatif

Cet article a fourni une introduction à l’infrastructure de remise utilisé pour continuer à une activité des utilisateurs entre plusieurs appareils Apple de l’utilisateur. Ensuite, elle a montré comment activer et mettre en œuvre de remise dans une application Xamarin.iOS. Enfin, il décrit les différents types de continuations de remise disponibles et les meilleures pratiques de remise.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Exemple de MonkeyBrowser](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)
- [iOS 9 pour les développeurs](https://developer.apple.com/ios/pre-release/)
- [Nouveautés dans iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Guide de HomeKitDeveloper](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [Indications de l’Interface utilisateur de HomeKit](https://developer.apple.com/homekit/ui-guidelines/)
- [Référence HomeKit Framework](https://developer.apple.com/library/ios/home_kit_framework_ref)
