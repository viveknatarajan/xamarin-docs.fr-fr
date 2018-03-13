---
title: "Extensions d’iOS"
description: "Introduite dans iOS 8, les Extensions sont des widgets qui sont présentés par iOS dans des contextes standards, comme dans le centre de Notification lorsque l’utilisateur demande un clavier personnalisé, ou lorsqu’ils sont photo modification. Toutes les Extensions sont installées conjointement avec une application conteneur et sont activées à partir d’un Point d’Extension particulier dans une application hôte."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 017f3c1b5fc14bf22a5ad85d1017bc8125b04038
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="ios-extensions"></a>Extensions d’iOS

_Introduite dans iOS 8, les Extensions sont des widgets qui sont présentés par iOS dans des contextes standards, comme dans le centre de Notification lorsque l’utilisateur demande un clavier personnalisé, ou lorsqu’ils sont photo modification. Toutes les Extensions sont installées conjointement avec une application conteneur et sont activées à partir d’un Point d’Extension particulier dans une application hôte._

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**Création d’Extensions dans iOS, par [Xamarin University](https://university.xamarin.com/)**

Extensions, introduite dans iOS 8, sont spécialisées `UIViewControllers` qui sont présentés par iOS dans des contextes standards tels que dans les **centre de notifications**, comme demandés par l’utilisateur pour effectuer des types de clavier personnalisés spécialisé entrée ou autres contextes comme modifier une photo où l’Extension peut fournir des filtres d’effet spécial.

Toutes les Extensions sont installées conjointement avec une application conteneur (avec les deux éléments écrits à l’aide de l’API unifiée 64 bits) et sont activées à partir d’un Point d’Extension particulier dans une application hôte. Et car ils seront utilisés comme compléments à des fonctions système existantes, ils doivent être des performances élevées, lean et robuste. 

Cet article couvre les rubriques suivantes :

- [Points d’extension](#Extension-Points) -répertorie le type des Points d’Extension disponibles et le type d’Extension qui peut être créé pour chaque point.
- [Limitations](#Limitations) -Extensions ont un nombre de limitations, certains d'entre eux sont universels à tous les types tandis que les autres types d’extensions peuvent avoir des limitations spécifiques de leur utilisation.
- [Distribution de l’installation et en cours d’exécution des Extensions](#Distributing-Installing-and-Running-Extensions) -Extensions sont distribuées via une application conteneur, qui, à son tour est envoyé et distribués via l’App Store. Les extensions distribuées avec l’application sont installées à ce stade, mais l’utilisateur doit activer chaque Extension explicitement. Les différents types d’Extensions sont activées dans les différentes façons.
- [Extension de cycle de vie](#Extension-Lifecycle) : une de Extension `UIViewController` sera instancié et commencer le cycle de vie de contrôleur d’affichage normale. Toutefois, contrairement à une application normale, les Extensions sont chargées, exécutées et puis interrompues à plusieurs reprises.
- [Création d’une Extension](#Creating-an-Extension) -lorsque vous développez une Extension, vos solutions contiendra au moins deux projets : l’application conteneur et un projet pour chaque Extension, le conteneur fournit.
- [Procédure pas à pas](#Walkthrough) - aborde la création d’un simple **aujourd'hui** Extension qui fournit son Interface utilisateur à un plan conceptuel à l’aide de widget ou dans le code, l’installation de l’Extension et le tester dans le simulateur iOS.
- [Communication avec l’application hôte](#Communicating-with-the-Host-App) -décrit brièvement la communication avec l’application hôte à partir d’une Extension.
- [Communication avec l’application parente](#Communicating-with-the-Parent-App) -décrit brièvement la communication avec l’application parente à partir d’une Extension.
- [Considérations et précautions](#Precautions-and-Considerations) -certaines connaître les précautions et les considérations qui doivent être prises en compte lors de la conception et implémentation d’une Extension de liste.
 

<a name="Extension-Points" />

## <a name="extension-points"></a>Points d’extension

Il existe plusieurs types d’extensions qui peuvent être créés dans iOS 8 (et supérieur) :

<table>
<colgroup>
<col />
<col />
<col />
</colgroup>

<thead>
<tr>
    <th >Type</th>
    <th >Description</th>
    <th >Point d’extension</th>
    <th >Application de l’hôte</th>
</tr>
</thead>

<tbody>
<tr>
    <td >Action</td>
    <td >Éditeur spécial ou la visionneuse pour un type de média spécifique</td>
    <td ><code>com.apple.ui-services</code></td>
    <td >Any</td>
</tr>
<tr>
    <td >Fournisseur de document</td>
    <td >Permet à l’application à utiliser un magasin de documents à distance</td>
    <td ><code>com.apple.fileprovider-ui</code></td>
    <td >Applications à l’aide un <a href="https://developer.xamarin.com/api/type/UIKit.UIDocumentPickerViewController/">UIDocumentPickerViewController</a></td>
</tr>
<tr>
    <td >Clavier</td>
    <td >Autres claviers</td>
    <td ><code>com.apple.keyboard-service</code></td>
    <td >Any</td>
</tr>
<tr>
    <td >Modification de la photo</td>
    <td >Manipulation de photo et de modification</td>
    <td ><code>com.apple.photo-editing</code></td>
    <td >Éditeur de photos.app</td>
</tr>
<tr>
    <td >Partager</td>
    <td >Partage des informations avec des réseaux sociaux, services, etc., de messagerie.</td>
    <td ><code>com.apple.share-services</code></td>
    <td >Any</td>
</tr>
<tr>
    <td >Aujourd'hui</td>
    <td >« Widgets » qui apparaissent sur l’écran de la date du jour ou d’un centre de Notification</td>
    <td ><code>com.apple.widget-extensions</code></td>
    <td >Centre de notifications et aujourd'hui</td>
</tr>
</tbody>
</table>

[Points d’extension supplémentaires](~/ios/platform/introduction-to-ios10/index.md#app-extensions) ont été ajoutées dans iOS 10.

<a name="Limitations" />

## <a name="limitations"></a>Limitations

Extensions ont un nombre de limitations, dont certains sont universels à tous les types (pour l’instance, aucun type d’Extension peut accéder les caméras ou qu’un micro) alors que les autres types d’extensions peuvent avoir des limitations spécifiques de leur utilisation (par exemple, personnalisés claviers ne peut pas être utilisé pour les champs d’entrée de sécuriser les données telles que les mots de passe). 

Les limitations universelles sont :

- Le [Kit de contrôle d’intégrité](~/ios/platform/healthkit.md) et [l’interface utilisateur du Kit d’événement](~/ios/platform/eventkit.md) infrastructures ne sont pas disponibles
- Les extensions ne peuvent pas utiliser [étendus des modes d’arrière-plan](http://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- Les extensions ne peut pas accéder à du périphérique caméras ou microphones (bien qu’ils peuvent accéder à des fichiers de support existants)
- Les extensions ne peut pas recevoir Air supprimer des données (même si elles peuvent transmettre les données via l’Air supprimer)
- [UIActionSheet](https://developer.xamarin.com/api/type/UIKit.UIActionSheet/) et [UIAlertView](https://developer.xamarin.com/api/type/UIKit.UIAlertView/) ne sont pas disponibles ; extensions doivent utiliser [UIAlertController](https://developer.xamarin.com/api/type/UIKit.UIAlertController/)
- Plusieurs membres de [UIApplication](https://developer.xamarin.com/api/type/UIKit.UIApplication/) ne sont pas disponibles : [UIApplication.SharedApplication](https://developer.xamarin.com/api/property/UIKit.UIApplication.SharedApplication/), `UIApplication.OpenURL`, `UIApplication.BeginIgnoringInteractionEvents` et `UIApplication.EndIgnoringInteractionEvents`
- iOS impose une limite d’utilisation de mémoire de 16 Mo sur les extensions d’aujourd'hui.
- Par défaut les extensions de clavier n’ont pas accès au réseau. Cela affecte le débogage sur l’appareil (la restriction n’est pas appliquée dans le simulateur), étant donné que Xamarin.iOS requiert l’accès réseau pour le fonctionnement du débogage. Il est possible de demander l’accès réseau en définissant le `Requests Open Access` valeur Info.plist du projet à `Yes`. Consultez d’Apple [Guide du clavier personnalisé](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) pour plus d’informations sur les limitations d’extension de clavier.

Pour connaître les limitations individuelles, consultez le site d’Apple [Guide de programmation des extensions d’application](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/).

<a name="Distributing-Installing-and-Running-Extensions" />

## <a name="distributing-installing-and-running-extensions"></a>Distribution, l’installation et l’exécution des Extensions

Les extensions sont distribuées à partir de dans une application conteneur, qui, à son tour est envoyé et distribués via l’App Store. Les extensions distribuées avec l’application sont installées à ce stade, mais l’utilisateur doit activer chaque Extension explicitement. Les différents types d’Extensions sont activées dans les différentes manières ; plusieurs nécessitent l’utilisateur accéder à la **paramètres** application et les activer à partir de là. Tandis que d’autres sont activés au moment de l’utilisation, telles que l’activation d’une Extension de partage lors de l’envoi d’une photo. 

L’application dans lequel l’Extension est utilisée (où l’utilisateur rencontre du Point d’Extension) est appelée le **hôte application**, puisqu’il s’agit de l’application qui héberge l’Extension lors de l’exécution. L’application qui installe l’Extension est la **application conteneur**car il s’agit de l’application contenant l’Extension lors de son installation.  

En règle générale, l’application conteneur décrit l’extension et guide l’utilisateur via le processus de l’activer.

<a name="Extension-Lifecycle" />

## <a name="extension-lifecycle"></a>Extension de cycle de vie

Une Extension peut être aussi simple qu’un seul [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) ou des Extensions plus complexes qui présentent plusieurs écrans d’interface utilisateur. Lorsque l’utilisateur rencontre un _des Points d’Extension_ (par exemple quand une image de partage), il aura la possibilité de choisir parmi les Extensions enregistrées pour ce Point d’Extension. 

S’ils choisissent un de votre application d’Extensions, son `UIViewController` sera instancié et commencer le cycle de vie de contrôleur d’affichage normale. Toutefois, contrairement à une application normale, est suspendue, mais généralement arrêté lorsque l’utilisateur a fini d’interagir avec eux, les Extensions sont chargées, exécutées et puis interrompues à plusieurs reprises.

Les extensions peuvent communiquer avec leurs applications de l’hôte via un [NSExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) objet. Certaines Extensions ont des opérations qui reçoivent des rappels asynchrones avec les résultats. Ces rappels seront exécutées sur des threads d’arrière-plan et l’Extension doit prendre cela en compte ; par exemple, à l’aide de [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/member/Foundation.NSObject.InvokeOnMainThread/) s’ils souhaitent mettre à jour l’interface utilisateur. Consultez le [communique avec l’application hôte](#Communicating-with-the-Host-App) section ci-dessous pour plus de détails.

Par défaut, Extensions et leurs applications conteneur ne peuvent pas communiquer, en dépit d’être installés ensemble. Dans certains cas, l’application de conteneur est essentiellement un conteneur « shipping » vide dont le but est pris en charge une fois que l’Extension est installée. Toutefois, si les circonstances imposent, l’application conteneur et l’Extension peuvent partager des ressources à partir d’une zone commune. En outre, un **aujourd'hui Extension** peut demander à son application conteneur pour ouvrir une URL. Ce comportement est indiqué dans le [faire évoluer le Widget du compte à rebours](http://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo).

<a name="Creating-an-Extension" />

## <a name="creating-an-extension"></a>Création d’une Extension

Extensions (et leurs applications conteneur) doivent être des fichiers binaires 64 bits et générées à l’aide de la Xamarin.iOS [unifiée des API](http://developer.xamarin.com/guides/cross-platform/macios/unified). Lorsque vous développez une Extension, vos solutions contiendra au moins deux projets : l’application conteneur et un projet pour chaque Extension, le conteneur fournit. 

<a name="Container-App-Project-Requirements" />

### <a name="container-app-project-requirements"></a>Configuration requise de projet d’application de conteneur

L’application conteneur utilisée pour installer l’Extension nécessite la configuration suivante :

- Il doit conserver une référence au projet d’Extension.   
- Il doit être une application complète (doit être en mesure de lancer et exécuter avec succès) même si elle n’a aucun effet plus de fournir un moyen pour installer une Extension. 
- Il doit avoir un identificateur de lot qui constitue la base de l’identificateur de lot de l’Extension de projet (voir la section ci-dessous pour plus de détails).

<a name="Extension-Project-Requirements" />

### <a name="extension-project-requirements"></a>Conditions de projet d’extension

En outre, les projets de l’Extension exige les éléments suivants :

- Il doit avoir un identificateur de lot qui commence par un identificateur de lot de l’application de son conteneur. Par exemple, si l’application conteneur a un identificateur de lot de `com.myCompany.ContainerApp`, identificateur de l’Extension peut être `com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- Il doit définir la clé `NSExtensionPointIdentifier`, avec une valeur appropriée (tel que `com.apple.widget-extension` pour un **aujourd'hui** widget de centre de Notification), dans son `Info.plist` fichier.
- Il doit également définir *soit* le `NSExtensionMainStoryboard` clé ou la `NSPrincipalClass` clé dans son `Info.plist` fichier avec une valeur appropriée :
    - Utilisez le `NSExtensionMainStoryboard` clé pour spécifier le nom de la table de montage séquentiel qui présente l’interface utilisateur principale de l’Extension (moins `.storyboard`). Par exemple, `Main` pour la `Main.storyboard` fichier.
    - Utilisez le `NSPrincipalClass` clé pour spécifier la classe qui sera initialisée lorsque l’Extension est démarré. La valeur doit correspondre à la **inscrire** valeur de votre `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

Les types spécifiques d’Extensions peuvent avoir des exigences supplémentaires. Par exemple, un **aujourd'hui** ou **centre de notifications** classe de principal de l’Extension doit implémenter [INCWidgetProviding](https://developer.xamarin.com/api/type/NotificationCenter.INCWidgetProviding/).

> [!IMPORTANT]
> **Remarque :** si vous démarrez votre projet à l’aide d’une modèles Extensions fournis par Visual Studio pour Mac, la plupart des (si pas toutes) de ces exigences seront fournies et remplies pour vous automatiquement par le modèle.

<a name="Walkthrough" />

## <a name="walkthrough"></a>Procédure pas à pas 

Dans la procédure suivante, vous allez créer un exemple **aujourd'hui** widget qui calcule le jour et le nombre de jours restants dans l’année :

[![](extensions-images/carpediemscreenshot-sm.png "Un widget aujourd'hui d’exemple qui calcule le jour et le nombre de jours restants dans l’année")](extensions-images/carpediemscreenshot.png#lightbox)

<a name="Creating-the-Solution" />

### <a name="creating-the-solution"></a>Création de la solution

Pour créer la solution requise, procédez comme suit :

1. Commencez par créer un nouveau iOS, **application vue unique** projet puis cliquez sur le **suivant** bouton : 

    [![](extensions-images/today01.png "Tout d’abord, créez un nouveau iOS, un projet d’application de vue unique et cliquez sur le bouton suivant")](extensions-images/today01.png#lightbox)
2. Appelez le projet `TodayContainer` et cliquez sur le **suivant** bouton : 

    [![](extensions-images/today02.png "Appelez le projet TodayContainer et cliquez sur le bouton suivant")](extensions-images/today02.png#lightbox)
3. Vérifiez le **nom du projet** et **SolutionName** et cliquez sur le **créer** bouton permettant de créer la solution : 

    [![](extensions-images/today03.png "Vérifiez le nom du projet et SolutionName et cliquez sur le bouton Créer pour créer la solution")](extensions-images/today03.png#lightbox)
4. Ensuite, dans le **l’Explorateur de solutions**, avec le bouton droit sur la Solution et ajoutez un nouveau **iOS Extension** de projet à partir de la **aujourd'hui Extension** modèle : 

    [![](extensions-images/today04.png "Ensuite, dans l’Explorateur de solutions, avec le bouton droit sur la Solution et ajouter un nouveau projet d’Extension iOS à partir du modèle d’Extension aujourd'hui")](extensions-images/today04.png#lightbox)
5. Appelez le projet `DaysRemaining` et cliquez sur le **suivant** bouton : 

    [![](extensions-images/today05.png "Appelez le projet DaysRemaining et cliquez sur le bouton suivant")](extensions-images/today05.png#lightbox)
6. Passez en revue le projet et cliquez sur le **créer** bouton pour la créer : 

    [![](extensions-images/today06.png "Passez en revue le projet et cliquez sur le bouton Créer pour la créer")](extensions-images/today06.png#lightbox)

La Solution résultante doit maintenant avoir deux projets, comme indiqué ici :

[![](extensions-images/today07.png "La Solution résultante devez maintenant avoir deux projets, comme illustré ici")](extensions-images/today07.png#lightbox)

<a name="Creating-the-Extension-User-Interface" />

### <a name="creating-the-extension-user-interface"></a>Création de l’Interface utilisateur d’Extension

Ensuite, vous devez concevoir l’interface pour votre **aujourd'hui** widget. Cela peut être fait soit à l’aide d’un Storyboard ou en créant l’interface utilisateur dans le code. Les deux méthodes seront abordées en détail ci-dessous.

<a name="Using-Storyboards" />

#### <a name="using-storyboards"></a>À l’aide de plans conceptuels

Pour générer l’interface utilisateur avec un plan conceptuel, procédez comme suit :

1. Dans le **l’Explorateur de solutions**, double-cliquez sur le projet d’Extension `Main.storyboard` fichier à ouvrir pour le modifier : 

    [![](extensions-images/today08.png "Double-cliquez sur le fichier de Main.storyboard projets Extension pour l’ouvrir pour modification")](extensions-images/today08.png#lightbox)
2. Sélectionnez l’étiquette qui a été ajouté automatiquement à l’interface utilisateur par le modèle et de lui donner le **nom** `TodayMessage` dans les **Widget** onglet de la **l’Explorateur de propriétés**: 

    [![](extensions-images/today09.png "Sélectionnez l’étiquette qui a été ajouté automatiquement à l’interface utilisateur par le modèle et de lui donner le nom TodayMessage dans l’onglet du Widget de l’Explorateur de propriétés")](extensions-images/today09.png#lightbox)
3. Enregistrez les modifications dans la table de montage séquentiel.

<a name="Using-Code" />

#### <a name="using-code"></a>Utilisation du code

Pour générer l’interface utilisateur dans le code, procédez comme suit : 

1. Dans le **l’Explorateur de solutions**, sélectionnez le **DaysRemaining** de projet, ajoutez une nouvelle classe et l’appeler `CodeBasedViewController`: 

    [![](extensions-images/code01.png "Aelect le projet DaysRemaining, ajoutez une nouvelle classe et l’appeler CodeBasedViewController")](extensions-images/code01.png#lightbox)
2. Là encore, dans le **l’Explorateur de solutions**, double-cliquez sur l’Extension `Info.plist` fichier à ouvrir pour le modifier : 

    [![](extensions-images/code02.png "Double-cliquez sur le fichier Info.plist des Extensions pour l’ouvrir pour modification")](extensions-images/code02.png#lightbox)
3. Sélectionnez le **vue de Source de** (du bas de l’écran) et ouvrez le `NSExtension` nœud : 

    [![](extensions-images/code03.png "Sélectionnez la vue de Source à partir du bas de l’écran et ouvrez le nœud NSExtension")](extensions-images/code03.png#lightbox)
4. Supprimer le `NSExtensionMainStoryboard` de clé et d’ajouter un `NSPrincipalClass` avec la valeur `CodeBasedViewController`: 

    [![](extensions-images/code04.png "Supprimer la clé NSExtensionMainStoryboard et ajouter un NSPrincipalClass avec la valeur CodeBasedViewController")](extensions-images/code04.png#lightbox)
5. Enregistrez les modifications apportées.

Modifiez ensuite le `CodeBasedViewController.cs` de fichier et le rendre l’aspect suivant :

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
    [Register("CodeBasedViewContoller")]
    public class CodeBasedViewController : UIViewController, INCWidgetProviding
    {
        public CodeBasedViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Add label to view
            var TodayMessage = new UILabel (new CGRect (0, 0, View.Frame.Width, View.Frame.Height)) {
                TextAlignment = UITextAlignment.Center
            };

            View.AddSubview (TodayMessage);
            
            // Insert code to power extension here...

        }
    }
}
```

Notez que la `[Register("CodeBasedViewContoller")]` correspond à la valeur que vous avez spécifié pour le `NSPrincipalClass` ci-dessus.

<a name="Coding-the-Extension" />

### <a name="coding-the-extension"></a>L’Extension de codage

Avec l’Interface utilisateur créé, ouvrez soit le `TodayViewController.cs` ou `CodeBasedViewController.cs` modification du fichier (en fonction de la méthode utilisée pour créer l’Interface utilisateur ci-dessus), le **ViewDidLoad** (méthode) et le rendre l’aspect suivant :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Calculate the values
    var dayOfYear = DateTime.Now.DayOfYear;
    var leapYearExtra = DateTime.IsLeapYear (DateTime.Now.Year) ? 1 : 0;
    var daysRemaining = 365 + leapYearExtra - dayOfYear;

    // Display the message
    if (daysRemaining == 1) {
        TodayMessage.Text = String.Format ("Today is day {0}. There is one day remaining in the year.", dayOfYear);
    } else {
        TodayMessage.Text = String.Format ("Today is day {0}. There are {1} days remaining in the year.", dayOfYear, daysRemaining);
    }
}
```

Si l’aide du code, méthode d’Interface utilisateur basée sur, remplacez le `// Insert code to power extension here...` commentaire avec le nouveau code ci-dessus. Après l’appel de l’implémentation de base (et insertion d’une étiquette pour la version de code en fonction), ce code effectue un calcul simple pour obtenir le jour de l’année et le nombre de jours restant. Ensuite, il affiche un message dans l’étiquette (`TodayMessage`) que vous avez créé dans la conception de l’interface utilisateur.

Notez le degré de similitude ce processus est le processus normal de l’écriture d’une application. D’une Extension `UIViewController` a le même cycle de vie en tant que vue contrôleur dans une application, sauf les Extensions n’ont pas de modes d’arrière-plan et ne sont pas interrompues lorsque l’utilisateur est terminé leur utilisation. Au lieu de cela, les Extensions sont à plusieurs reprises initialisées et désallouées en fonction des besoins.

<a name="Creating-the-Container-App-User-Interface" />

### <a name="creating-the-container-app-user-interface"></a>Création de l’Interface utilisateur de l’application de conteneur

Pour cette procédure pas à pas, l’application conteneur est simplement utilisée en tant que méthode pour livrer et installer l’Extension et n’offre aucune fonctionnalité lui est propre. Modification de la TodayContainer `Main.storyboard` de fichiers et d’ajouter du texte de définition de fonction de l’Extension et comment l’installer :

[![](extensions-images/today10.png "Modifiez le fichier TodayContainers Main.storyboard et ajouter du texte de définition de la fonction Extensions et comment l’installer")](extensions-images/today10.png#lightbox)

Enregistrez les modifications dans la table de montage séquentiel.

<a name="Testing-the-Extension" />

### <a name="testing-the-extension"></a>Test de l’Extension

Pour tester votre Extension dans le simulateur iOS, exécutez le **TodayContainer** application. Vue de principal du conteneur s’affichera :

[![](extensions-images/run01.png "La vue principale de conteneurs s’affichera.")](extensions-images/run01.png#lightbox)

Ensuite, appuyez sur la **accueil** bouton dans le simulateur, faites défiler vers le bas à partir du haut de l’écran pour ouvrir la **centre de notifications**, sélectionnez le **aujourd'hui** onglet et cliquez sur le **Modifier** bouton :

[![](extensions-images/run02.png "Cliquez sur le bouton Accueil dans le simulateur, faites défiler vers le bas à partir du haut de l’écran pour ouvrir le centre de Notification, sélectionnez l’onglet aujourd'hui et cliquez sur le bouton Modifier")](extensions-images/run02.png#lightbox)

Ajouter le **DaysRemaining** Extension pour le **aujourd'hui** afficher et cliquez sur le **fait** bouton :

[![](extensions-images/run03.png "Ajouter le DaysRemaining Extension la vue aujourd'hui et cliquez sur le bouton terminé")](extensions-images/run03.png#lightbox)

Le nouveau widget est ajouté à la **aujourd'hui** vue et les résultats seront affichent :

[![](extensions-images/run04.png "Le nouveau widget sera ajouté à la vue aujourd'hui et les résultats s’affichent")](extensions-images/run04.png#lightbox)

<a name="Communicating-with-the-Host-App" />

## <a name="communicating-with-the-host-app"></a>Communication avec l’application hôte

L’exemple aujourd'hui les extensions que vous avez créé précédemment ne communique pas avec son application hôte (le **aujourd'hui** écran). Si c’était le cas, il utilise le [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) propriété de la `TodayViewController` ou `CodeBasedViewController` classes. 

Pour les Extensions qui recevront les données à partir de leurs applications hôtes, les données sont sous la forme d’un tableau de [NSExtensionItem](https://developer.xamarin.com/api/type/Foundation.NSExtensionItem/) objets stockés dans le [InputItems](https://developer.xamarin.com/api/property/Foundation.NSExtensionContext.InputItems/) propriété de la [ExtensionContext ](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) de l’Extension `UIViewController`.

Autre Extension, telles que les extensions de l’édition Photo, peut faire la distinction entre l’utilisateur à la fin ou l’annulation de l’utilisation. Cela sera signalé vers l’application hôte via le [CompleteRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CompleteRequest/) et [CancelRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CancelRequest/) méthodes de [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) propriété.

Pour plus d’informations, consultez le site d’Apple [Guide de programmation des extensions d’application](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1).

<a name="Communicating-with-the-Parent-App" />

## <a name="communicating-with-the-parent-app"></a>Communication avec l’application parente

Un App Group est un groupe qui permet à différentes applications (ou à une application et ses extensions) d’accéder à un emplacement partagé du stockage de fichiers. Vous pouvez utiliser des App Groups pour les données suivantes, par exemple :

- [Apple Watch paramètres](~/ios/watchos/app-fundamentals/settings.md).
- [Partagé NSUserDefaults](~/ios/app-fundamentals/user-defaults.md).
- [Fichiers partagés](~/ios/watchos/app-fundamentals/parent-app.md#files).

Pour plus d’informations, consultez la [App Groups](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) section de notre **utilisation des fonctionnalités** documentation.

<a name="MobileCoreServices" />

## <a name="mobilecoreservices"></a>MobileCoreServices

Lorsque vous travaillez avec des extensions, utilisez un identificateur de Type uniforme (lecture) pour créer et manipuler des données qui sont échangées entre l’application, d’autres applications et/ou services.

Le `MobileCoreServices.UTType` classe statique définit les propriétés d’assistance suivantes qui se rapportent à Apple `kUTType...` définitions :

- `kUTTypeAlembic` - `Alembic`
- `kUTTypeAliasFile` - `AliasFile`
- `kUTTypeAliasRecord` - `AliasRecord`
- `kUTTypeAppleICNS` - `AppleICNS`
- `kUTTypeAppleProtectedMPEG4Audio` - `AppleProtectedMPEG4Audio`
- `kUTTypeAppleProtectedMPEG4Video` - `AppleProtectedMPEG4Video`
- `kUTTypeAppleScript` - `AppleScript`
- `kUTTypeApplication` - `Application`
- `kUTTypeApplicationBundle` - `ApplicationBundle`
- `kUTTypeApplicationFile` - `ApplicationFile`
- `kUTTypeArchive` - `Archive`
- `kUTTypeAssemblyLanguageSource` - `AssemblyLanguageSource`
- `kUTTypeAudio` - `Audio`
- `kUTTypeAudioInterchangeFileFormat` - `AudioInterchangeFileFormat`
- `kUTTypeAudiovisualContent` - `AudiovisualContent`
- `kUTTypeAVIMovie` - `AVIMovie`
- `kUTTypeBinaryPropertyList` - `BinaryPropertyList`
- `kUTTypeBMP` - `BMP`
- `kUTTypeBookmark` - `Bookmark`
- `kUTTypeBundle` - `Bundle`
- `kUTTypeBzip2Archive` - `Bzip2Archive`
- `kUTTypeCalendarEvent` - `CalendarEvent`
- `kUTTypeCHeader` - `CHeader`
- `kUTTypeCommaSeparatedText` - `CommaSeparatedText`
- `kUTTypeCompositeContent` - `CompositeContent`
- `kUTTypeConformsToKey` - `ConformsToKey`
- `kUTTypeContact` - `Contact`
- `kUTTypeContent` - `Content`
- `kUTTypeCPlusPlusHeader` - `CPlusPlusHeader`
- `kUTTypeCPlusPlusSource` - `CPlusPlusSource`
- `kUTTypeCSource` - `CSource`
- `kUTTypeData` - `Database`
- `kUTTypeDelimitedText` - `DelimitedText`
- `kUTTypeDescriptionKey` - `DescriptionKey`
- `kUTTypeDirectory` - `Directory`
- `kUTTypeDiskImage` - `DiskImage`
- `kUTTypeElectronicPublication` - `ElectronicPublication`
- `kUTTypeEmailMessage` - `EmailMessage`
- `kUTTypeExecutable` - `Executable`
- `kUTExportedTypeDeclarationsKey` - `ExportedTypeDeclarationsKey`
- `kUTTypeFileURL` - `FileURL`
- `kUTTypeFlatRTFD` - `FlatRTFD`
- `kUTTypeFolder` - `Folder`
- `kUTTypeFont` - `Font`
- `kUTTypeFramework` - `Framework`
- `kUTTypeGIF` - `GIF`
- `kUTTypeGNUZipArchive` - `GNUZipArchive` 
- `kUTTypeHTML` - `HTML`
- `kUTTypeICO` - `ICO`
- `kUTTypeIconFileKey` - `IconFileKey`
- `kUTTypeIdentifierKey` - `IdentifierKey`
- `kUTTypeImage` - `Image`
- `kUTImportedTypeDeclarationsKey` - `ImportedTypeDeclarationsKey`
- `kUTTypeInkText` - `InkText`
- `kUTTypeInternetLocation` - `InternetLocation`
- `kUTTypeItem` - `Item`
- `kUTTypeJavaArchive` - `JavaArchive`
- `kUTTypeJavaClass` - `JavaClass`
- `kUTTypeJavaScript` - `JavaScript`
- `kUTTypeJavaSource` - `JavaSource`
- `kUTTypeJPEG` - `JPEG`
- `kUTTypeJPEG2000` - `JPEG2000`
- `kUTTypeJSON` - `JSON`
- `kUTType3dObject` - `k3dObject`
- `kUTTypeLivePhoto` - `LivePhoto`
- `kUTTypeLog` - `Log` 
- `kUTTypeM3UPlaylist` - `M3UPlaylist`
- `kUTTypeMessage` - `Message`
- `kUTTypeMIDIAudio` - `MIDIAudio`
- `kUTTypeMountPoint` - `MountPoint`
- `kUTTypeMovie` - `Movie`
- `kUTTypeMP3` - `MP3`
- `kUTTypeMPEG` - `MPEG`
- `kUTTypeMPEG2TransportStream` - `MPEG2TransportStream`
- `kUTTypeMPEG2Video` - `MPEG2Video`
- `kUTTypeMPEG4` - `MPEG4`
- `kUTTypeMPEG4Audio` - `MPEG4Audio`
- `kUTTypeObjectiveCPlusPlusSource` - `ObjectiveCPlusPlusSource`
- `kUTTypeObjectiveCSource` - `ObjectiveCSource`
- `kUTTypeOSAScript` - `OSAScript`
- `kUTTypeOSAScriptBundle` - `OSAScriptBundle`
- `kUTTypePackage` - `Package`
- `kUTTypePDF` - `PDF`
- `kUTTypePerlScript` - `PerlScript`
- `kUTTypePHPScript` - `PHPScript`
- `kUTTypePICT` - `PICT`
- `kUTTypePKCS12` - `PKCS12`
- `kUTTypePlainText` - `PlainText`
- `kUTTypePlaylist` - `Playlist`
- `kUTTypePluginBundle` - `PluginBundle`
- `kUTTypePNG` - `PNG`
- `kUTTypePolygon` - `Polygon`
- `kUTTypePresentation` - `Presentation`
- `kUTTypePropertyList` - `PropertyList`
- `kUTTypePythonScript` - `PythonScript`
- `kUTTypeQuickLookGenerator` - `QuickLookGenerator`
- `kUTTypeQuickTimeImage` - `QuickTimeImage`
- `kUTTypeQuickTimeMovie` - `QuickTimeMovie` 
- `kUTTypeRawImage` - `RawImage`
- `kUTTypeReferenceURLKey` - `ReferenceURLKey`
- `kUTTypeResolvable` - `Resolvable`
- `kUTTypeRTF` - `RTF`
- `kUTTypeRTFD` - `RTFD`
- `kUTTypeRubyScript` - `RubyScript`
- `kUTTypeScalableVectorGraphics` - `ScalableVectorGraphics`
- `kUTTypeScript` - `Script`
- `kUTTypeShellScript` - `ShellScript`
- `kUTTypeSourceCode` - `SourceCode`
- `kUTTypeSpotlightImporter` - `SpotlightImporter`
- `kUTTypeSpreadsheet` - `Spreadsheet`
- `kUTTypeStereolithography` - `Stereolithography`
- `kUTTypeSwiftSource` - `SwiftSource`
- `kUTTypeSymLink` - `SymLink`
- `kUTTypeSystemPreferencesPane` - `SystemPreferencesPane`
- `kUTTypeTabSeparatedText` - `TabSeparatedText`
- `kUTTagClassFilenameExtension` - `TagClassFilenameExtension`
- `kUTTagClassMIMEType` - `TagClassMIMEType`
- `kUTTypeTagSpecificationKey` - `TagSpecificationKey`
- `kUTTypeText` - `Text`
- `kUTType3DContent` - `ThreeDContent`
- `kUTTypeTIFF` - `TIFF`
- `kUTTypeToDoItem` - `ToDoItem`
- `kUTTypeTXNTextAndMultimediaData` - `TXNTextAndMultimediaData`
- `kUTTypeUniversalSceneDescription` - `UniversalSceneDescription`
- `kUTTypeUnixExecutable` - `UnixExecutable`
- `kUTTypeURL` - `URL` 
- `kUTTypeURLBookmarkData` - `URLBookmarkData`
- `kUTTypeUTF16ExternalPlainText` - `UTF16ExternalPlainText`
- `kUTTypeUTF16PlainText` - `UTF16PlainText`
- `kUTTypeUTF8PlainText` - `UTF8PlainText`
- `kUTTypeUTF8TabSeparatedText` - `UTF8TabSeparatedText`
- `kUTTypeVCard` - `VCard`
- `kUTTypeVersionKey` - `VersionKey` 
- `kUTTypeVideo` - `Video` 
- `kUTTypeVolume` - `Volume` 
- `kUTTypeWaveformAudio` - `WaveformAudio`
- `kUTTypeWebArchive` - `WebArchive`
- `kUTTypeWindowsExecutable` - `WindowsExecutable`
- `kUTTypeX509Certificate` - `X509Certificate`
- `kUTTypeXML` - `XML`
- `kUTTypeXMLPropertyList` - `XMLPropertyList`
- `kUTTypeXPCService` - `XPCService`
- `kUTTypeZipArchive` - `ZipArchive`

Consultez l’exemple suivant :

```csharp
using MobileCoreServices;
...

NSItemProvider itemProvider = new NSItemProvider ();
itemProvider.LoadItem(UTType.PropertyList ,null, (item, err) => {
    if (err == null) {
        NSDictionary results = (NSDictionary )item;
        NSString baseURI =
results.ObjectForKey("NSExtensionJavaScriptPreprocessingResultsKey");
    }
});
```

Pour plus d’informations, consultez la [App Groups](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) section de notre **utilisation des fonctionnalités** documentation.


<a name="Precautions-and-Considerations" />

## <a name="precautions-and-considerations"></a>Précautions et considérations

Extensions ont beaucoup moins de mémoire disponible pour les applications de faire. Ils doivent effectuer rapidement et avec une intrusion minimale à l’utilisateur et de l’application, dans qu'ils sont hébergés. Toutefois, une Extension doit également fournir une fonction unique, utile pour l’application consommatrice avec une interface utilisateur personnalisée qui permet à l’utilisateur identifier les développeur de l’Extension ou une application conteneur qu'auquel ils appartiennent.

Étant donné ces étroite exigence, vous devez déployer uniquement les Extensions qui ont été soigneusement testées et optimisées pour les performances et la consommation de mémoire. 

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Ce document a couvert les Extensions, ce qu’ils sont, le type de Points d’Extension et les limitations imposées sur une Extension par iOS. Il décrit la création, la distribution, l’installation et les Extensions et le cycle de vie d’Extension en cours d’exécution. Il fourni une procédure pas à pas de création d’un simple **aujourd'hui** widget montrant deux façons de créer l’interface utilisateur du widget à l’aide des animations ou code. Il vous a montré comment une extension de test dans le simulateur iOS. Enfin, il décrit brièvement communique avec l’application hôte et quelques précautions et les considérations qui doivent être prises lors du développement d’une extension. 


## <a name="related-links"></a>Liens associés

- [ContainerApp (exemple)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Création d’extensions dans Xamarin.iOS (vidéo)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
