---
title: Résolution des problèmes
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/0201
ms.openlocfilehash: 6a179c1d63e9b5a7b8a42705d5c112a7b71a4906
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>Résolution des problèmes

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Impossible de résoudre Xamarin.iOS System.ValueTuple

Cette erreur se produit en raison d’une incompatibilité avec Visual Studio.

- **Mise à jour 1 de Visual Studio 2017** (version 15.1 ou antérieure) est compatible uniquement avec **System.ValueTuple NuGet 4.3.0** (ou version antérieure).

- **Mise à jour 2 de Visual Studio 2017** (version 15,2 ou une version ultérieure) est compatible uniquement avec les **System.ValueTuple NuGet 4.3.1** ou une version ultérieure.

Veuillez choisir System.ValueTuple NuGet correct qui correspond à votre installation de Visual Studio 2017.


## <a name="receiving-error-retrieving-update-information-error-message"></a>Message d’erreur « Erreur de récupération des informations de mise à jour »

Lors de la tentative de mise à jour le logiciel et le message d’erreur suivant s’affiche, essayez de redémarrer votre IDE et la déconnexion, puis dans votre compte dans l’IDE.

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>Comment créer des prises de courant ou des actions avec le Générateur de Interface ?

Avec l’Introduction du concepteur Xamarin IOS dans Visual Studio pour Mac et Visual studio, les développeurs de Xamarin.iOS peuvent désormais tirer parti de la création d’une interface utilisateur via des plans conceptuels et .xibs. Reportez-vous à la [Hello, iOS](~/ios/get-started/hello-ios/index.md) guides pour plus d’informations sur l’utilisation du concepteur.

Vous pouvez également faire référence à d’Apple [prise](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) et [Actions](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html) guides pour plus d’informations sur l’utilisation de points de vente et les Actions dans IB.

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>System.Text.Encoding.GetEncoding lève NotSupportedException

Vous utilisez peut-être un encodage n’est pas ajoutée par défaut. Vérifiez le [internationalisation](~/ios/app-fundamentals/localization/index.md) page pour savoir comment ajouter la prise en charge pour l’encodage de plus.

## <a name="systemmissingmethodexception-anything-else"></a>System.MissingMethodException (rien)

Le membre a été probablement supprimé par l’éditeur de liens et par conséquent n’existe pas dans l’assembly au moment de l’exécution.  Il existe plusieurs solutions à cela :

-  Ajouter le [[conserver]](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute) au membre d’attribut.  Cela empêchera l’éditeur de liens de le supprimer.
-  Lors de l’appel [mtouch](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29) , utilisez le **- nolink** ou **- linksdkonly** options. -    **-nolink** désactive la liaison de tous les.
-    **-linksdkonly** liera uniquement les assemblys Xamarin.iOS fourni, telles que *monotouch.dll* ou xamarin.ios.dll.

Notez que les assemblys sont liés pour que le fichier exécutable obtenu est plus petit ; Par conséquent, la désactivation de la liaison peut entraîner un fichier exécutable plus volumineux qu’il n’est souhaitable.

## <a name="you-are-getting-a-modelnotimplementedexception"></a>Vous obtenez une ModelNotImplementedException

Si vous obtenez cette exception, cela signifie que vous appelez base. Méthode () sur une classe qui remplace un modèle. Vous n’avez pas besoin d’appeler la méthode de base dans une classe pour les modèles (il s’agit des classes qui sont marqués avec l’attribut [modèle]).

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>Cette classe n’est pas un codage compatible avec la clé XXXX de valeur de clé

Si vous obtenez cette erreur lors du chargement d’un fichier NIB il signifie que la valeur que XXXX est introuvable sur votre classe managée. Cela signifie qu’il manque une déclaration comme suit :

```csharp
[Connect]
TypeName XXXX {
   get {
       return (TypeName) GetNativeField ("XXXX");
   }
   set {
       SetNativeField ("XXXX", value);
   }
}
```

La définition ci-dessus est automatiquement générée par Visual Studio pour Mac pour tous les fichiers XIB que vous ajoutez à Visual Studio pour Mac dans le `NAME_OF_YOUR_XIB_FILE.designer.xib.cs` fichier.

En outre, les types contenant le code ci-dessus doivent être une sous-classe de [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).  Si le type conteneur est dans un espace de noms, il doit également avoir un [[Enregistrer]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) attribut qui fournit un nom de type sans un espace de noms (comme le Générateur de l’Interface ne prend pas en charge les espaces de noms de types) :

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>Classe inconnue XXXX dans le fichier de l’Interface Générateur

Cette erreur est générée si vous définissez une classe dans vos fichiers de générateur d’interface, mais vous ne fournissez pas l’implémentation réelle pour lui dans votre code c#.

Vous devez ajouter du code comme suit :

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```
## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>System.MissingMethodException : Aucun constructeur trouvé pour Foo.Bar::ctor(System.IntPtr)

Cette erreur se produit lors de l’exécution lorsque le code tente d’instancier une instance des classes que vous avez référencé à partir de votre fichier de l’Interface générateur. Cela signifie que vous avez oublié d’ajouter un constructeur qui accepte un IntPtr unique en tant que paramètre.

Le constructeur avec un handle d’IntPtr est utilisé pour lier des objets gérés avec leurs représentations non managées.

Pour résoudre ce problème, ajoutez la ligne de code suivante à la classe Foo.Bar :

```csharp
public Bar (IntPtr handle) : base (handle) { }
```
## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>Type {Foo} ne contient pas de définition pour `GetNativeField' and no extension method `GetNativeField' de type {Foo} a été trouvé.

Si vous obtenez cette erreur dans les fichiers générés concepteur (*. xib.designer.cs), cela signifie qu’une des deux choses :

 **(1) manquant de la classe partielle ou la classe de base**

Les classes partielles généré par le concepteur doivent avoir des classes partielles correspondants dans le code utilisateur qui héritent d’une sous-classe de `NSObject`, souvent `UIViewController`. Assurez-vous que vous disposez d’une telle classe pour le type qui est et affiche l’erreur.

 **(2) les espaces de noms par défaut modifiés**

Les fichiers de concepteur sont générés à l’aide des paramètres d’espace de noms de votre projet par défaut. Si vous avez modifié ces paramètres, ou de renommer le projet, les classes partielles générées peuvent ne plus être dans le même espace de noms que leurs équivalents de code utilisateur.

Namespace paramètres se trouvent dans la boîte de dialogue Options du projet. L’espace de noms par défaut se trouve dans le **général -> Paramètres principaux** section. Si elle est vide, le nom de votre projet est utilisé en tant que la valeur par défaut. Paramètres d’espace de noms plus avancées se trouvent dans le **Code Source -> stratégies d’appellation de .NET** section.

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>Avertissement pour les actions : la méthode privée « Foo » n’est jamais utilisé. (CS0169)

Actions pour les fichiers de générateur d’interface sont connectées pour les widgets par la réflexion au moment de l’exécution, cet avertissement est normal.

Vous pouvez utiliser « #pragma warning désactiver 0169 » « #pragma warning activer 0169" autour de vos actions si vous souhaitez supprimer cet avertissement pour ces méthodes, ou ajoutez 0169 au champ « Ignorer les avertissements » dans les options du compilateur si vous souhaitez pour votre projet dans son ensemble (à ne pas désactiver recommandé).

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>mtouch a échoué avec le message suivant : Impossible d’ouvrir l’assembly ' / path/to/yourproject.exe'

Si vous voyez ce message d’erreur, généralement le problème est que le chemin d’accès absolu à votre projet contient un espace. Ce problème sera résolu dans une future version de Xamarin.iOS, mais vous pouvez contourner le problème en déplaçant le projet dans un dossier sans espaces.

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Votre version sqlite3 est ancien - Veuillez mettre à niveau vers au moins v3.5.0 !

Cela se produit lorsque vous effectuez toutes les opérations suivantes :

1.  Utilisez Mono.Data.Sqlite
1.  Utiliser Mac OS X Leopard (10.5)
1.  Exécutez votre application dans le simulateur.


Le problème est que Mono est reprenant le système d’exploitation X `libsqlite3.dylib`, de pas l’iPhoneSimulator `libsqlite3.dylib` fichier. Votre application *sera* Professionnel sur l’appareil, mais pas le simulateur.

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>Déployer à l’échec de l’appareil avec System.Exception : AMDeviceInstallApplication retourné 3892346901

Cette erreur signifie que la configuration de signature de code de votre id de certificat/regroupement ne correspond pas le profil de configuration installé sur votre appareil.  Vérifiez que le certificat approprié est sélectionné dans les Options de projet -> iPhone signature d’offre groupée et l’id d’ensemble correct spécifié dans les Options du projet -> iPhone Application

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>Exécution de code ne fonctionne pas dans Visual Studio pour Mac

Assurez-vous que vous utilisez la version la plus récente de Visual Studio pour Mac et Xamarin.iOS

Si le problème persiste, veuillez [entrer un bogue](http://monodevelop.com/Developers#Reporting_Bugs), attachement le **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{TIMESTAMP} .log**, et **composants-{TIMESTAMP} .log** des fichiers journaux.

Si tout le reste échoue, vous pouvez essayer de supprimer le cache de saisie semi-automatique du code afin qu’il est régénéré :

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

Veillez à ce que vous tapez cette commande correctement ou que vous pourriez supprimer accidentellement des fichiers importants.

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>Visual Studio pour Mac se bloque lorsque vous copiez le texte

Les utilitaires Mac populaires QuickSilver, barre d’outils Google et barre de lancement ont des fonctionnalités de Presse-papiers qui endommagée de Visual Studio pour la mémoire du Mac. Dans les options, vous pouvez répertorier Visual Studio pour Mac en tant que processus qu’ils ne doivent pas interférer avec.

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>Visual Studio pour Mac plaint 2.4 Mono requis

Si la mise à jour de Visual Studio pour Mac en raison d’une mise à jour récente, et lorsque vous essayez de le démarrer à nouveau il plaint 2.4 Mono n’est ne pas présent, il vous est [mettre à niveau votre installation Mono 2.4](http://www.go-mono.com/mono-downloads/download.html).  

2.4.2.3_6 mono résout certains problèmes importants qui empêchait Visual Studio pour Mac fonctionnent de manière fiable parfois le blocage de Visual Studio pour Mac au démarrage ou l’a empêché la base de données de saisie semi-automatique de code en cours de génération.

Une fois que vous installez le nouveau Mono, Visual Studio pour Mac démarre comme prévu.

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>Assertion au... /.. /.. /.. /mono/Metadata/Generic-Sharing.c:704, condition 'oti' non satisfaite

Si vous recevez la trace de pile suivante :

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

Cela signifie que vous liez une bibliothèque statique compilée avec code thumb dans votre projet. À compter de la version du Kit de développement logiciel iPhone 3.1 (ou version ultérieure au moment de la rédaction) Apple a introduit un bogue dans son éditeur de liens lors de la liaison du code non-Thumb (Xamarin.iOS) avec le code Thumb (votre bibliothèque statique). Vous devez créer un lien avec une version non-Thumb de votre bibliothèque statique pour atténuer ce problème.

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1getcount-"></a>System.ExecutionEngineException : Tentative de JIT compilation (méthode) (un wrapper managé au code managé) Foo[]:System.Collections.Generic.ICollection'1.get_Count ()

Le suffixe [] indique que vous ou la bibliothèque de classes appelant une méthode sur un tableau dans une collection générique, telles que IEnumerable <>, ICollection <> ou <> de IList. Pour résoudre ce problème, vous pouvez explicitement force le compilateur AOA pour inclure cette méthode en appelant la méthode vous-même et en faisant en sorte que ce code est exécuté avant l’appel qui a déclenché l’exception. Dans ce cas, vous pouvez écrire :

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

Ce qui force le compilateur AOA à inclure la méthode get_Count.

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>Visual Studio pour l’éditeur de code source Mac est extrêmement lent

Parfois, Visual Studio pour l’éditeur de code source Mac devient extrêmement lent, semble être bloquée pendant quelques secondes entre la saisie de caractères.

Ce problème est très rare et très difficiles à reproduire - il généralement ne peut pas être reproduit sur le même ordinateur après le redémarrage de Visual Studio pour Mac. C’est pourquoi nous vous prions si vous pouvez effectuer plusieurs étapes de débogage avant de redémarrer Visual Studio pour Mac et nous envoyer les résultats.

1.  Essayez de fermer l’onglet d’éditeur et ouvrant de nouveau. Prend un peu de modification ou de déplacer le signe insertion jusqu'à ce que le ralentissement se produit à nouveau ?
1.  Désactiver la synchronisation des faisceaux « » à l’aide de l’outil de développement « Débogage Quartz » (qui se trouve à l’aide de la galerie) et vérifiez si les performances de l’éditeur de source sont restaurée à la normale.
1.  Répétez l’étape (1) avec la synchronisation de faisceaux reste désactivé.
1.  Si l’éditeur se bloque pendant plus de quelques secondes, essayez d’exécuter « killall-quitter Visual Studio pour Mac] » dans un terminal pendant qu’il est bloqué. Il peut être difficile à l’heure de la commande kill alors que l’éditeur est bloqué, mais il est essentiel pour ce faire, car la commande force Mono pour écrire des suivis de pile de tous les threads dans le journal MD, que nous pouvons utiliser pour découvrir quel état les threads se trouvent dans pendant x est bloqué.



Joignez les journaux XS, **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{TIMESTAMP} .log**, et **composants-{TIMESTAMP} .log**(dans les versions antérieures de XS/MonoDevelop, envoyez **~/Library/Logs/MonoDevelop-(3.0|2.8|2.6)/MonoDevelop.log**).

 **Remarque : Le problème ci-dessus a été résolu dans XS 2.2 finale**

## <a name="compiled-application-is-very-large"></a>Application compilée est très volumineux

Pour prendre en charge le débogage, les versions debug contiennent du code supplémentaire. Les projets générés en mode release sont une fraction de la taille.

À compter de Xamarin.iOS 1.3 le débogage builds inclus débogage prise en charge pour chaque composant unique de Mono (chaque méthode dans chaque classe des infrastructures).  

Avec Xamarin.iOS 1.4, nous allons vous présenter une méthode plus précisément à grains fins de débogage, la valeur par défaut sera de fournir uniquement l’instrumentation de débogage pour votre code et vos bibliothèques et pas pour tous les [assemblys Mono](~/cross-platform/internals/available-assemblies.md) (Cela continue possible, mais vous devez choisir de déboguer ces assemblys).

## <a name="installation-hangs"></a>Blocage de l’installation

Programmes d’installation Mono et Xamarin.iOS se bloquer si vous disposez de l’exécution du simulateur iPhone. Ce problème n’est pas limité à Mono ou Xamarin.iOS, il s’agit d’un problème de cohérence sur un logiciel qui tente d’installer des logiciels sur MacOS Snow Leopard si iPhone Simulator est en cours d’exécution au moment de l’installation.

Assurez-vous que vous quittez le simulateur iPhone et recommencez l’installation.

<a name="trampolines" />

## <a name="ran-out-of-trampolines-of-type-0"></a>A manqué d’espace trampolines de type 0

Si vous obtenez ce message lors de l’appareil en cours d’exécution, vous pouvez créer plusieurs types de 0 trampolines (type spécifique) en modifiant votre projet-section options « iPhone Build ».  Vous souhaitez ajouter des cibles de génération des arguments supplémentaires pour le périphérique :

 `-aot "ntrampolines=2048"`

Le nombre par défaut de trampolines est 1024.  Essayez d’augmenter ce nombre jusqu'à ce que vous disposez de suffisamment de votre application.

## <a name="ran-out-of-trampolines-of-type-1"></a>A manqué d’espace trampolines de type 1

Si vous faites un usage intensif de récursive génériques, vous pouvez obtenir ce message sur l’appareil.  Vous pouvez créer plusieurs types de 1 trampolines (type RGCTX) en modifiant votre projet-section options « iPhone Build ».  Vous souhaitez ajouter des cibles de génération des arguments supplémentaires pour le périphérique :

 `-aot "nrgctx-trampolines=2048"`

Le nombre par défaut de trampolines est 1024.  Essayez d’augmenter ce nombre jusqu'à ce que vous disposez de suffisamment de votre utilisation de génériques.

## <a name="ran-out-of-trampolines-of-type-2"></a>A manqué d’espace trampolines de type 2

Si vous apportez à utilisation intensive des interfaces, vous pouvez obtenir ce message sur l’appareil.
Vous pouvez créer plusieurs types de 2 trampolines (type IMT Thunks) en modifiant votre projet-section options « iPhone Build ».  Vous souhaitez ajouter des cibles de génération des arguments supplémentaires pour le périphérique :

 `-aot "nimt-trampolines=512"`

Le nombre par défaut de trampolines de IMT Thunk est 128.  Essayez d’augmenter ce nombre jusqu'à ce que vous disposez de suffisamment de votre utilisation des interfaces.

## <a name="debugger-is-unable-to-connect-with-the-device"></a>Débogueur ne peut pas se connecter à l’appareil

Lorsque vous démarrez le débogage d’une configuration d’appareil, vous verrez le débogueur affiche une boîte de dialogue indiquant qu’il tente de se connecter à l’application. Il existe plusieurs raisons, que le débogueur ne peut pas être en mesure de se connecter à l’application, en fonction du mode vous utilisez pour vous connecter (USB ou Wi-Fi).

 **Si l’appareil et l’hôte du débogueur se trouvent sur différents réseaux**, un pare-feu ou un réseau privé empêchent l’application de se connecter à l’hôte du débogueur en mode de Wi-Fi.

 **Visual Studio pour Mac ne peut pas être en mesure de rechercher l’adresse IP correct de l’hôte**. Dans Wi-Fi mode Visual Studio pour Mac donne à l’application toutes les adresses IP qu’il peut trouver de l’hôte et l’application tente de toutes les pour voir si elle peut utiliser un d'entre eux pour se connecter à Visual Studio pour Mac.

 **Un autre périphérique est connecté à un port USB sur l’ordinateur hôte.** Dans certains cas autres périphériques connectés au port USB ports sur l’ordinateur hôte sont connus pour une certaine manière interfère avec le débogage en mode USB.

Si le mode Wi-Fi ou USB ne fonctionne pas, vous pouvez facilement essayer les autres : dans Visual Studio pour Mac, ouvrez les préférences, accédez à la page des préférences/débogueur/iPhone débogueur et activer/désactiver la case à cocher « Débogage des appareils iOS via le Wi-Fi au lieu d’USB ».   Si aucune des deux fonctionnement, vous pouvez voir plus d’informations sur l’échec dans la console de l’appareil en mode documenté (qui est activé en ajoutant «-v - v - v » pour les arguments supplémentaires mtouch dans les options du projet).

## <a name="error-134-mtouch-failed-with-the-following-message"></a>Erreur 134 : mtouch a échoué avec le message suivant :

Cette erreur peut être déclenchée si vous tentez de générer avec - nolink le style de Xamarin.iOS 1.4 de versions. Vous pouvez contourner cette erreur en spécifiant des Arguments supplémentaires dans votre configuration de projet monodevelop.

Ajoutez l’argument

 `-nosymbolstrip`

et le problème doit être résolu.

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>Identité de distribution ne figure pas dans Visual Studio pour le projet Mac options de signature

Visual Studio pour Mac 2.2 comporte un bogue qui oblige ne pas à détecter les certificats de distribution qui contiennent une virgule. Mettez à jour vers Visual Studio pour Mac 2.2.1.

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>Erreur « AFCFileRefWrite retourné : 1 » lors du téléchargement

Lors du téléchargement d’une application sur votre appareil, vous pouvez recevoir une erreur « AFCFileRefWrite retourné : 1 ». Cela peut se produire si vous avez un fichier de longueur nulle.

## <a name="error-mtouch-failed-with-no-output"></a>Erreur « échoué sans aucune sortie de mtouch »

La version actuelle de Xamarin.iOS et de Visual Studio pour Mac échoue lorsque le nom du projet ou le répertoire où sont stockés la solution ou le projet contient des espaces.
Pour résoudre ce problème :


-  Assurez-vous que votre projet, ni le répertoire où il est stocké contient un espace.
-  Dans votre projet « paramètres principal », assurez-vous que le nom du projet ne contient pas d’espaces.

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>Erreur « le fichier binaire que vous avez téléchargé n’est pas valide. Une version bêta de la version préliminaire du Kit de développement a été utilisée pour générer l’application »

Cette erreur est généralement dû à un projet qui a été démarré dans le développement d’iPad avant la parution de Xamarin.iOS 2.0.0, vous avez probablement certaines clés dans votre fichier Info.plist telles que :

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

Cette paire de clés doit être supprimé comme Visual Studio pour Mac prend en charge pour vous automatiquement.

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>Erreur « une version bêta de la version préliminaire du Kit de développement a été utilisée pour générer l’application. »

(Fournis par Ed Anuff)

Procédez comme suit :

-  Modification de la version SDK dans iPhone version 3.2 ou iTunes connecter refusera lors du téléchargement, car il voit une application compatible iPad générée à l’aide d’une version du Kit de développement logiciel inférieur à 3.2
-  Créer un Info.plist personnalisé pour le projet et définir explicitement MinimumOSVersion 3.0 qu’elle contient.   Cela remplacera la valeur de MinimumOSVersion 3.2 définie par Xamarin.iOS.   Si vous ne le faites pas, l’application ne sera pas en mesure d’exécuter sur un iPhone.
-  Reconstruction, zip et du téléchargement iTunes se connectent.

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>Exception non gérée : System.Exception : Impossible de trouver le sélecteur someSelector : {type}

Cette exception est provoquée par une des trois opérations :


1.  Vous avez fourni un sélecteur pour Objective-C runtime sans appliquer l’attribut [Exporter] correspondant à une méthode
1.  Vous avez activé la liaison complète et ne pas appliqué l’attribut [Preserve] à la méthode [Exporter] ed.
1.  Vous avez appliqué l’attribut [Exporter] à une méthode privée d’un type hérité.


## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>Fichier de MainWindow.xib.designer.cs n’est pas mis à jour.

Raison d’un bogue dans Xamarin Studio 2.4 entraîné ne pas le fichier MainWindow.xib avec le fichier MainWindow.xib.designer dans les nouveaux projets de groupe. Cela signifiait que pas à jour le code du concepteur pour ce fichier particulier.

Ce problème est résolu dans la version de Visual Studio pour Mac est disponible dans sa mise à jour intégrée, donc Vérifiez que vous utilisez une version plus récente.

Vous pouvez corriger des projets existants en supprimant (vous ne supprimez ne pas) le xib et son fichier de concepteur, puis il rajoutez. Il doit regrouper les fichiers correctement.

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>UIAlertView ou UIActionSheet disparaître après avoir été créés

Si vous avez du code comme suit :

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

l’objet « actionSheet » vie d’une variable temporaire dans la fonction et dès que la fonction s’arrête, l’objet est éligible pour le garbage collection, donc il finit par le garbage collecté.

Pour résoudre ce problème, vous devez conserver une référence à « actionSheet » en dehors de votre méthode quelque part qui se trouvera au-delà de votre méthode.

## <a name="project-always-runs-in-the-ipad-simulator"></a>Projet toujours s’exécute dans le simulateur iPad

Le programme d’installation de l’iPhone SDK 4.0 installe 2 kits de développement logiciel - le Kit de développement logiciel 3.2, pour générer des applications iPad uniquement et le Kit de développement logiciel 4.0, utilisé pour la construction iPhone et les applications universelles. Il installe également un simulateur 3.2, ce qui simule uniquement sur un iPad, et un simulateur 4.0 qui simule les iPhone ou iPhone 4. Tous les kits de développement logiciel et simulateurs plus anciens sont supprimés.

Visual Studio pour les options de génération de projet Mac iPhone incluent un paramètre pour la version du Kit de développement logiciel qui sera utilisé dans la génération de votre application. Ce paramètre peut se trouver dans **Options du projet -> Build -> iPhone Build**.

Nouveaux projets dans Visual Studio pour Mac utilisent le plus ancien SDK installé en tant que paramètre de kit de développement logiciel par défaut, et si le SDK spécifié n’existe pas, Visual Studio pour Mac utilisera la plus proche qu’il peut trouver pour générer votre application. Cela a été effectuée afin que les projets ne sont pas toujours requre le Kit de développement logiciel les plus récents. Toutefois, cela entraîne actuellement dans le Kit de développement logiciel 3.2 en cours utilisé - ce qui entraîne le simulateur iPad utilisé.

Pour résoudre ce problème à l’aide du Kit de développement logiciel 4.0, accédez à **Options du projet -> Build -> iPhone Build**> et remplacez la valeur du Kit de développement logiciel « 4.0 » à l’aide de la liste déroulante. Vous devez le faire pour chaque combinaison de plateforme, accessibles via les menus déroulants en haut du Panneau de configuration.

La version du Kit de développement logiciel ne doit pas être confondue avec le paramètre « Version minimale du système d’exploitation ».
Cette valeur n’a pas à correspondre à la valeur de version du Kit de développement logiciel - il affecte la version minimale du système d’exploitation votre application s’installera sur, qui peut être plus ancienne que le Kit de développement, tant que vous utilisez uniquement les API qui existent dans le système d’exploitation plus anciens, ou protéger l’utilisation des nouvelles fonctionnalités à l’aide de & de version de runtime du système d’exploitation KS. Vous devez le définir à la version la plus ancienne du système d’exploitation sur lequel vous testez votre application.

Notez également que la **projet -> iPhone simulateur cible**> menu peut être utilisé pour sélectionner le simulateur est utilisé par défaut lors de l’exécution et de déboguer un projet. En outre, le **exécuter -> exécuter avec**> menu peut être utilisé pour choisir un simulateur spécifique avec lequel exécuter

## <a name="ibtool-returns-error-133"></a>ibtool renvoie l’erreur 133

Cela signifie que vous avez XCode 4 est installé.   Dans XCode 4, l’outil ibtool a été supprimé, il n’est plus possible de modifier vos fichiers XIB avec un outil autonome.

Si vous souhaitez utiliser l’Interface générateur, installez [XCode série 3](http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792), disponible à partir du site web d’Apple.

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-wbrinterface-builder"></a>« Impossible de créer la liaison complet du type mime : application/vnd.apple -<wbr/>Générateur d’interface »

Cette erreur se produit si vous essayez de créer un iPhone, l’interface utilisateur à partir d’un projet non-iPhone. Assurez-vous que vous démarrez avec une solution iPhone/iPad, il n’est pas possible d’ajouter simplement les éléments d’interface utilisateur iPhone à un projet iPhone/iPad.

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>Panne de démarrage lors de l’exécution dans le simulateur iOS

Si vous obtenez une panne du runtime (SIGSEGV) dans le simulateur, ainsi que d’une trace de pile qui ressemble à ceci :

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```
... puis que vous avez probablement un (ou plusieurs) assembly obsolète dans le répertoire de votre application de simulateur. Ces assemblys peuvent existe depuis Apple iOS simulator ajoute et met à jour les fichiers, mais ne les supprime. Si cela produit, la solution la plus simple consiste à sélectionner « Réinitialiser et de contenu et les paramètres... » dans le menu du simulateur.   

**Avertissement :** cela supprimera tous les fichiers, applications et données à partir du simulateur.   Prochaine fois que vous exécuterez votre application, Visual Studio pour Mac déploierez dans le simulateur et il n’y aura aucun assembly ancien, périmé pour provoquer le blocage.

## <a name="simulator-hangs-during-application-installation"></a>Simulator se bloque lors de l’installation de l’application

Cela peut se produire lorsque les noms d’application incluent un '.' (point) dans leur nom.
Cela est interdit en tant que le nom d’exécutable dans CFBundleExecutable - même si elle peut fonctionne dans de nombreux autres cas (par exemple, les appareils).

 * » La valeur ne peut inclure n’importe quelle extension de nom. » - [http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf](http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf)

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>Erreur : « 0 x 43 de type d’attribut personnalisé n'est pas prise en charge » lorsque le double-clic sur les fichiers .xib

Cela est dû à une tentative d’ouverture de fichiers de .xib lorsque les variables d’environnement sont définies de façon incorrecte. Cela ne devrait pas se produire avec une utilisation normale de Visual Studio pour Mac/Xamarin.iOS et rouvrir Visual Studio pour Mac à partir de/applications doit corriger le problème.

Lorsque vous tentez de mettre à jour le logiciel et le message d’erreur suivant s’affiche, veuillez par courrier électronique *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>Application s’exécute sur le simulateur, mais échoue sur l’appareil

Ce problème peut se manifester dans plusieurs formulaires et ne produit pas toujours une erreur de cohérence. Si l’application contient un .xib, vérifiez que le **Action de génération** sur la .xib est définie sur **interface**. Il s’agit de l’action de génération par défaut pour .xibs.

Pour vérifier l’action de génération, cliquez avec le bouton droit sur le fichier .xib et choisissez **Action de génération**.


## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException : Aucune donnée n’est disponible pour l’encodage 437

Lorsque vous incluez des bibliothèques tierces 3e dans votre application Xamarin.iOS, vous pouvez obtenir une erreur sous la forme « System.NotSupportedException : aucune donnée n’est disponible pour l’encodage 437 » lorsque vous tentez de compiler et exécuter l’application. Par exemple, bibliothèques, telles que `Ionic.Zip.ZipFile`, peut lever cette exception au cours de l’opération.

Cela peut être résolu en ouvrant les options pour le projet Xamarin.iOS, en accédant à **Build iOS** > **internationalisation** et en vérifiant la **ouest** internationalisation.



<a name="Can't_upgrade_to_Indie/Business_from_Trial_Account" />


## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>Ne peut pas mettre à niveau vers contient/entreprise à partir de la version d’évaluation de compte

Si vous acheté récemment Xamarin.iOS et démarrée précédemment une version d’évaluation Xamarin.iOS, vous devrez peut-être effectuer les étapes suivantes pour obtenir cette modification de la licence sélectionnée par Visual Studio pour Mac ou Visual Studio.

-  Fermez Visual Studio pour Mac/Visual Studio
-  Supprimez tous les fichiers ~/Library/MonoTouch sur Mac ou %PROGRAMDATA%\MonoTouch\License\ pour Windows
-  Ouvrez à nouveau Visual Studio pour Mac/Visual Studio et générer un projet Xamarin.iOS


## <a name="receiving-activation-incomplete-error-message"></a>Message d’erreur « Activation incomplet »

Ce problème peut se produire lors de l’utilisation de Xamarin.iOS pour Visual Studio. Pour résoudre ce problème, envoyez les journaux à partir de l’emplacement suivant pour [ contact@xamarin.com ](mailto:contact@xamarin.com).

-  Emplacement du journal : %LocalAppData%/Xamarin/Logs
