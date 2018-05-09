---
title: Implémentation de SiriKit
description: Cet article décrit les étapes requises pour implémenter la prise en charge SiriKit dans un Xamarin.iOS les applications.
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: a4f38e93cae3c9577a0b1e32067da2cfd2e4796d
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="implementing-sirikit"></a>Implémentation de SiriKit

_Cet article décrit les étapes requises pour implémenter la prise en charge SiriKit dans un Xamarin.iOS les applications._



Nouveau pour iOS 10, SiriKit permet à une application Xamarin.iOS fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri et l’application de cartes sur un appareil iOS. Cet article décrit les étapes requises pour implémenter la prise en charge SiriKit dans les applications Xamarin.iOS en ajoutant les Extensions intentions requises, les Extensions de l’interface utilisateur intentions et le vocabulaire.

Siri fonctionne avec le concept de **domaines**, groupes de connaître les actions pour les tâches associées. Chaque interaction de l’application avec Siri doit appartenir à l’un de ses domaines de service connu comme suit :

- Audio ou vidéo.
- Un cas de réservation.
- La gestion de sommeil.
- Gestion des messages.
- Recherche des photos.
- Envoyer ou recevoir des paiements.

Lorsque l’utilisateur effectue une demande de Siri impliquant un des services de l’Extension application, SiriKit envoie l’extension un **intention** objet qui décrit la demande de l’utilisateur, ainsi que toutes les données de prise en charge. l’Extension d’application, puis génère les **réponse** de l’objet pour la donnée **intention**, détaillant la façon dont l’extension peut gérer la demande.

Ce guide présente un exemple de prise en charge de SiriKit dans une application existante. Cet exemple, nous utiliserons l’application MonkeyChat fausse :

[![](implementing-sirikit-images/monkeychat01.png "L’icône MonkeyChat")](implementing-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat conserve son propre contact Carnet d’adresses de vos amis de l’utilisateur, chacune associée à un nom de l’écran (par exemple, Bobo par exemple) et permet à l’utilisateur d’envoyer des conversations de texte à chaque ami par leur nom de l’écran.

## <a name="extending-the-app-with-sirikit"></a>Extension de l’application avec SiriKit

Comme indiqué dans le [SiriKit de présentation des Concepts](~/ios/platform/sirikit/understanding-sirikit.md) guide, il existe trois parties principales relatives à l’extension d’une application avec SiriKit :

[![](implementing-sirikit-images/elements01.png "Extension de l’application avec un diagramme de SiriKit")](implementing-sirikit-images/elements01.png#lightbox)

Elles incluent notamment :

1. **Extension des intentions** -vérifie les réponses des utilisateurs, confirme que l’application puisse traiter la demande et effectue la tâche afin de répondre à la demande de l’utilisateur.
2. **Extension de l’interface utilisateur intentions** - *facultatif*, fournit une interface utilisateur personnalisée pour les réponses dans l’environnement Siri et pour faire passer les applications de l’interface utilisateur et de personnalisation dans Siri à enrichir l’expérience utilisateur.
3. **Application** -fournit l’application avec les vocabulaires spécifiques d’utilisateur pour faciliter son utilisation Siri. 

Tous ces éléments et les étapes pour les inclure dans l’application seront abordée en détail dans les sections ci-dessous.


## <a name="preparing-the-app"></a>Préparation de l’application

SiriKit repose sur des Extensions, toutefois, avant d’ajouter des Extensions à l’application, il existe quelques éléments que le développeur doit faire pour vous aider à l’adoption de SiriKit.

### <a name="moving-common-shared-code"></a>Déplacer le Code commun partagé 

Tout d’abord, le développeur peut déplacer une partie du code commun qui est partagé entre l’application et les extensions dans un _projets partagés_, _des bibliothèques de classes portables (PCLs)_ ou _natif Bibliothèques_.

Les Extensions devrez être en mesure d’effectuer toutes les opérations que l’application. Dans les termes du contrat de l’exemple d’application MonkeyChat, éléments tels que la recherche de contacts, ajout de nouveaux contacts, envoyer des messages et la récupération de l’historique du message.

En déplaçant ce code commun dans un projet partagé, la bibliothèque PCL ou la bibliothèque Native, il rend facile à maintenir que le code dans un emplacement commun et permet de s’assurer que l’Extension et l’application parente fournissent une expérience uniforme et fonctionnalités pour l’utilisateur.

Dans le cas de l’exemple d’application MonkeyChat, les modèles de données et le code de traitement telles que l’accès réseau et de la base de données seront déplacés vers une bibliothèque Native.

Effectuez ce qui suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Démarrez Visual Studio pour Mac et ouvrez l’application MonkeyChat.
2. Avec le bouton droit sur le nom de la Solution dans le **Solution remplissage** et sélectionnez **ajouter** > **nouveau projet...** : 

    [![](implementing-sirikit-images/prep01.png "Ajouter un nouveau projet")](implementing-sirikit-images/prep01.png#lightbox)
3. Sélectionnez **iOS** > **bibliothèque** > **bibliothèque de classes** et cliquez sur le **suivant** bouton : 

    [![](implementing-sirikit-images/prep02.png "Sélectionnez la bibliothèque de classes")](implementing-sirikit-images/prep02.png#lightbox)
4. Entrez `MonkeyChatCommon` pour le **nom** et cliquez sur le **créer** bouton : 

    [![](implementing-sirikit-images/prep03.png "Entrez MonkeyChatCommon pour le nom")](implementing-sirikit-images/prep03.png#lightbox)
5. Avec le bouton droit sur le **références** dossier de l’application principale dans le **l’Explorateur de solutions** et sélectionnez **modifier les références...** . Vérifiez le **MonkeyChatCommon** projet puis cliquez sur le **OK** bouton : 

    [![](implementing-sirikit-images/prep05.png "Archiver le projet MonkeyChatCommon")](implementing-sirikit-images/prep05.png#lightbox)
6. Dans le **l’Explorateur de solutions**, faites glisser le code commun partagé à partir de l’application principale pour la bibliothèque Native.
7. Dans le cas MonkeyChat, faites glisser le **DataModels** et **processeurs** dossiers à partir de l’application principale dans la bibliothèque Native : 

    [![](implementing-sirikit-images/prep06.png "Les dossiers DataModels et les processeurs dans l’Explorateur de solutions")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Démarrez Visual Studio et ouvrez l’application MonkeyChat.
2. Avec le bouton droit sur le nom de la Solution dans le **l’Explorateur de solutions** et sélectionnez **ajouter** > **nouveau projet...** .
3. Sélectionnez **Visual C#** > **projet partagé** et cliquez sur le **suivant** bouton : 

    [![](implementing-sirikit-images/prep02.w157-sml.png "Sélectionnez la bibliothèque de classes")](implementing-sirikit-images/prep02.w157.png#lightbox)
4. Entrez `MonkeyChatCommon` pour le **nom** et cliquez sur le **créer** bouton.
5. Avec le bouton droit sur le **références** dossier de l’application principale dans le **l’Explorateur de solutions** et sélectionnez **modifier les références...** . Vérifiez le **MonkeyChatCommon** projet puis cliquez sur le **OK** bouton : 

    [![](implementing-sirikit-images/prep05w.png "Archiver le projet MonkeyChatCommon")](implementing-sirikit-images/prep05w.png#lightbox)
6. Dans le **l’Explorateur de solutions**, faites glisser le code commun partagé à partir de l’application principale pour le projet partagé.
7. Dans le cas MonkeyChat, faites glisser le **DataModels** et **processeurs** dossiers à partir de l’application principale dans la bibliothèque Native.

-----

Modifiez les fichiers qui ont été déplacées vers la bibliothèque Native et l’espace de noms pour correspondre à celui de la bibliothèque. Par exemple, la modification `MonkeyChat` à `MonkeyChatCommon`:

```csharp
using System;
namespace MonkeyChatCommon
{
    /// <summary>
    /// A message sent from one user to another within a conversation.
    /// </summary>
    public class MonkeyMessage
    {
        public MonkeyMessage ()
        {
        }
        ...
    }
}
```

Ensuite, revenez à l’application principale et ajouter un `using` instruction pour l’espace de noms de la bibliothèque Native n’importe où l’application utilise l’une des classes qui ont été déplacés :

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using MonkeyChatCommon;

namespace MonkeyChat
{
    public partial class MasterViewController : UITableViewController
    {
        public DetailViewController DetailViewController { get; set; }

        DataSource dataSource;
        ...
    }
}
```

### <a name="architecting-the-app-for-extensions"></a>Architecture de l’application pour les Extensions

En règle générale, une application s’inscrire pour plusieurs modes et le développeur doit s’assurer que l’application est conçue pour le nombre approprié d’Extensions de l’intention.

Dans le cas où une application nécessite plus d’un seul objectif, le développeur a la possibilité de plaçant toutes sa gestion intention dans une Extension de l’intention d’ou de créer une Extension intention distincte pour chaque objectif.

Si vous choisissez Créer une Extension intention distincte pour chaque objectif, le développeur peut finir de duplication d’une grande quantité de code réutilisable dans chaque extension et créer une grande quantité de processeur et de la charge de mémoire.

Pour aider à choisir entre ces deux options, voir si un des modes de l’ensemble appartenir de naturellement. Par exemple, une application qui a effectué des appels audio et vidéo voudriez inclure deux de ces modes dans une même Extension intention, car elles sont la gestion des tâches similaires et peut fournir la réutilisation de code de la plupart des.

Pour toute intention ou un groupe de modes qui ne tiennent pas dans un groupe existant, créer une Extension intention dans la solution de l’application pour les contenir.


### <a name="setting-the-required-entitlements"></a>Définir les droits requis

N’importe quelle application Xamarin.iOS qui inclut l’intégration de SiriKit, doivent avoir les droits corrects défini. Si le développeur ne correctement ces droits requis, ils ne seront pas en mesure d’installer ou de tester l’application sur un matériel iOS réel de 10 (ou supérieur), qui est également depuis les 10 iOS Simulator ne prend pas en charge SiriKit.

Effectuez ce qui suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Double-cliquez sur le `Entitlements.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
2. Basculez vers le **Source** onglet.
3. Ajouter le `com.apple.developer.siri` **propriété**, définissez le **Type** à `Boolean` et **valeur** à `Yes`: 

    [![](implementing-sirikit-images/setup01.png "Ajoutez la propriété com.apple.developer.siri")](implementing-sirikit-images/setup01.png#lightbox)
4. Enregistrez les modifications dans le fichier.
5. Double-cliquez sur le **fichier projet** dans les **l’Explorateur de solutions** à ouvrir pour le modifier.
6. Sélectionnez **signature d’offre groupée iOS** et vérifiez que le `Entitlements.plist` fichier est sélectionné dans le **personnalisé droits** champ : 

    [![](implementing-sirikit-images/setup02.png "Sélectionnez le fichier Entitlements.plist dans le champ personnalisé droits")](implementing-sirikit-images/setup02.png#lightbox)
7. Cliquez sur le bouton **OK** pour enregistrer les changements.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Double-cliquez sur le `Entitlements.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
3. Ajouter le `com.apple.developer.siri` **propriété**, définissez le **Type** à `Boolean` et **valeur** à `Yes`: 

    [![](implementing-sirikit-images/setup01w.png "Ajoutez la propriété com.apple.developer.siri")](implementing-sirikit-images/setup01w.png#lightbox)
4. Enregistrez les modifications dans le fichier.
5. Double-cliquez sur le **fichier projet** dans les **l’Explorateur de solutions** à ouvrir pour le modifier.
6. Sélectionnez **signature d’offre groupée iOS** et vérifiez que le `Entitlements.plist` fichier est sélectionné dans le **personnalisé droits** champ.

-----

Lorsque vous avez terminé, l’application `Entitlements.plist` fichier doit se présenter comme suit (dans ouvert dans un éditeur externe) :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.siri</key>
    <true/>
</dict>
</plist>
```

### <a name="correctly-provisioning-the-app"></a>L’approvisionnement correctement l’application

En raison de la sécurité stricte Apple a placé autour du framework n’importe quelle application Xamarin.iOS qui implémente SiriKit SiriKit _doit_ ont l’ID d’application et les droits (voir la section ci-dessus) correcte et doivent être signées avec un approprié. Profil de configuration.

Procédez comme suit sur votre Mac :

1. Dans un navigateur web, accédez à [ http://developer.apple.com ](http://developer.apple.com) et connectez-vous à votre compte.
2. Cliquez sur **certificats**, **identificateurs** et **profils**.
3. Sélectionnez **profils de configuration** et sélectionnez **ID d’application**, puis cliquez sur le **+** bouton.
4. Entrez un **nom** pour le nouveau profil.
5. Entrez un **ID d’offre groupée** suivant Apple d’attribution de noms recommandation.
6. Faites défiler jusqu'à la **des Services d’application** section, sélectionnez **SiriKit** et cliquez sur le **continuer** bouton : 

    [![](implementing-sirikit-images/setup03.png "Sélectionnez SiriKit")](implementing-sirikit-images/setup03.png#lightbox)
7. Vérifiez que tous les paramètres, puis **Submit** l’application ID.
8. Sélectionnez **profils de configuration** > **développement**, cliquez sur le **+** bouton, sélectionnez le **ID Apple**, puis cliquez sur **continuer**.
9. Cliquez sur Sélectionner **tous les**, puis cliquez sur **continuer**.
10. Cliquez sur **sélectionner tout** , puis cliquez sur **continuer**.
11. Entrez un **nom du profil** à l’aide d’Apple d’attribution de noms des suggestions, puis cliquez sur **continuer**.
12. Démarrez Xcode.
13. Dans le menu Xcode sélectionnez **préférences...**
14. Sélectionnez **comptes**, puis cliquez sur le **afficher les détails...** bouton : 

    [![](implementing-sirikit-images/setup04.png "Sélectionnez les comptes")](implementing-sirikit-images/setup04.png#lightbox)
15. Cliquez sur le **télécharger tous les profils** bouton dans le coin inférieur gauche : 

    [![](implementing-sirikit-images/setup05.png "Télécharger tous les profils")](implementing-sirikit-images/setup05.png#lightbox)
16. Vérifiez que le **profil de préparation** créé ci-dessus a été installé dans Xcode.
17. Ouvrez le projet pour ajouter la prise en charge SiriKit dans Visual Studio pour Mac.
18. Double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions**.
18. Vérifiez que le **identificateur de lot** correspond à celui créé dans le portail des développeurs d’Apple ci-dessus : 

    [![](implementing-sirikit-images/setup06.png "L’identificateur de lot")](implementing-sirikit-images/setup06.png#lightbox)
18. Dans le **l’Explorateur de solutions**, sélectionnez le **projet**.
19. Cliquez sur le projet et sélectionnez **Options**.
21. Sélectionnez **signature d’offre groupée iOS**, sélectionnez le **identité de signature** et **profil de préparation** créé ci-dessus : 

    [![](implementing-sirikit-images/setup07.png "Sélectionnez l’identité de signature et d’un profil de configuration")](implementing-sirikit-images/setup07.png#lightbox)
22. Cliquez sur le bouton **OK** pour enregistrer les changements.

> [!IMPORTANT]
> Test SiriKit fonctionne uniquement sur un véritable iOS 10 périphérique matériel et non dans les 10 iOS Simulator. Si vous rencontrez des problèmes d’installation d’un SiriKit activé application Xamarin.iOS sur du vrai matériel, assurez-vous que les droits requis ID d’application, identificateur de signature et profil de préparation ont été correctement configurés dans Apple Developer Portal et Visual Studio pour Mac.

### <a name="requesting-siri-authorization"></a>Demande d’autorisation de Siri

Avant de l’application ajoute un vocabulaire spécifique d’utilisateur ou les Extensions intentions se connecte à Siri, il doit demande une autorisation de l’utilisateur pour accéder aux Siri.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Modifier l’application `Info.plist` file, basculez vers le **Source** afficher et ajouter la `NSSiriUsageDescription` clé avec une valeur de chaîne qui décrit l’utilisation de l’application Siri et ce que types de données seront envoyés. Par exemple, l’application MonkeyChat peut indiquer « MonkeyChat contacts seront envoyés à Siri » :

[![](implementing-sirikit-images/request01.png "La NSSiriUsageDescription dans l’éditeur de fichier Info.plist.")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Modifier l’application `Info.plist` et ajoutez le `NSSiriUsageDescription` clé avec une valeur de chaîne qui décrit l’utilisation de l’application Siri et ce que les types de données seront envoyés. Par exemple, l’application MonkeyChat peut indiquer « MonkeyChat contacts seront envoyés à Siri » :

[![](implementing-sirikit-images/request01w.png "La NSSiriUsageDescription dans l’éditeur de fichier Info.plist.")](implementing-sirikit-images/request01w.png#lightbox)

-----

Appelez le `RequestSiriAuthorization` méthode de la `INPreferences` classe au premier démarrage de l’application. Modifier la `AppDelegate.cs` classe et rendre le `FinishedLaunching` détaillée de la méthode comme suit :


```csharp
using Intents;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{

    // Request access to Siri
    INPreferences.RequestSiriAuthorization ((INSiriAuthorizationStatus status) => {
        // Respond to returned status
        switch (status) {
        case INSiriAuthorizationStatus.Authorized:
            break;
        case INSiriAuthorizationStatus.Denied:
            break;
        case INSiriAuthorizationStatus.NotDetermined:
            break;
        case INSiriAuthorizationStatus.Restricted:
            break;
        }
    });


    return true;
}
```

La première fois que cette méthode est appelée, une alerte s’affiche invitant l’utilisateur à autoriser l’application à accéder à Siri. Le message que le développeur est ajouté à la `NSSiriUsageDescription` ci-dessus s’affichera cette alerte. Si l’utilisateur refuse l’accès au départ, ils peuvent utiliser le **paramètres** application pour accorder l’accès à l’application.

À tout moment, l’application peut vérifier la configuration de l’application à accéder à Siri en appelant le `SiriAuthorizationStatus` méthode de la `INPreferences` classe.

### <a name="localization-and-siri"></a>Localisation et Siri

Sur un appareil iOS, l’utilisateur peut sélectionner une langue pour Siri est différent, puis la valeur par défaut du système. Lorsque vous travaillez avec des données localisées, l’application doit utiliser le `SiriLanguageCode` méthode de la `INPreferences` classe pour obtenir le code de langue à partir de Siri. Par exemple :

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>Ajout de vocabulaire spécifique d’utilisateur

Le vocabulaire spécifique utilisateur va fournir des mots ou expressions qui sont uniques à des utilisateurs individuels de l’application. Ces éléments seront fournis lors de l’exécution à partir de l’application principale (pas les Extensions d’application) comme un ensemble ordonné de termes du contrat, classés dans une priorité d’utilisation plus importante pour les utilisateurs, les termes du contrat plus important au début de la liste.

Vocabulaire spécifique d’utilisateur doit appartenir à une des catégories suivantes :

- Noms de contact (qui ne sont pas gérés par l’infrastructure de Contacts).
- Balises de la photo.
- Albums photo.
- Noms d’entraînement.

Lorsque vous sélectionnez la terminologie à inscrire en tant que le vocabulaire personnalisé, uniquement choisir des termes qui serait erroné par une personne ne connaissent pas l’application. Jamais register termes courants tels que « Mes entraînement » ou « Mon Album ». Par exemple, l’application MonkeyChat inscrira les surnoms associés à chaque contact dans le carnet d’adresses de l’utilisateur.

L’application fournit le vocabulaire spécifique utilisateur en appelant le `SetVocabularyStrings` méthode de la `INVocabulary` classe et en passant un `NSOrderedSet` collection à partir de l’application principale. L’application doit toujours appeler la `RemoveAllVocabularyStrings` méthode first, pour supprimer les termes du contrat existant avant d’ajouter de nouveaux. Par exemple :

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Foundation;
using Intents;

namespace MonkeyChatCommon
{
    public class MonkeyAddressBook : NSObject
    {
        #region Computed Properties
        public List<MonkeyContact> Contacts { get; set; } = new List<MonkeyContact> ();
        #endregion

        #region Constructors
        public MonkeyAddressBook ()
        {
        }
        #endregion

        #region Public Methods
        public NSOrderedSet<NSString> ContactNicknames ()
        {
            var nicknames = new NSMutableOrderedSet<NSString> ();

            // Sort contacts by the last time used
            var query = Contacts.OrderBy (contact => contact.LastCalledOn);

            // Assemble ordered list of nicknames by most used to least
            foreach (MonkeyContact contact in query) {
                nicknames.Add (new NSString (contact.ScreenName));
            }

            // Return names
            return new NSOrderedSet<NSString> (nicknames.AsSet ());
        }

        // This method MUST only be called on a background thread!
        public void UpdateUserSpecificVocabulary ()
        {
            // Clear any existing vocabulary
            INVocabulary.SharedVocabulary.RemoveAllVocabularyStrings ();

            // Register new vocabulary
            INVocabulary.SharedVocabulary.SetVocabularyStrings (ContactNicknames (), INVocabularyStringType.ContactName);
        }
        #endregion
    }
}
```

Avec ce code en place, il peut être appelé comme suit :

```csharp
using System;
using System.Threading;
using UIKit;
using MonkeyChatCommon;
using Intents;

namespace MonkeyChat
{
    public partial class ViewController : UIViewController
    {
        #region AppDelegate Access
        public AppDelegate ThisApp {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do we have access to Siri?
            if (INPreferences.SiriAuthorizationStatus == INSiriAuthorizationStatus.Authorized) {
                // Yes, update Siri's vocabulary
                new Thread (() => {
                    Thread.CurrentThread.IsBackground = true;
                    ThisApp.AddressBook.UpdateUserSpecificVocabulary ();
                }).Start ();
            }
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

> [!IMPORTANT]
> Siri traite le vocabulaire personnalisé en tant qu’indicateurs et incorporer à la plus grande partie de la terminologie que possible. Toutefois, l’espace vocabulaire personnalisé est limitée rendant important d’inscrire _uniquement_ la terminologie qui peut être déroutant, par conséquent en réduisant le nombre total de termes inscrits au minimum.

Pour plus d’informations, consultez notre [référence vocabulaire spécifique de l’utilisateur](~/ios/platform/sirikit/understanding-sirikit.md) et d’Apple [en spécifiant une référence de vocabulaire personnalisé](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

### <a name="adding-app-specific-vocabulary"></a>Ajout de vocabulaire d’application spécifique

Le vocabulaire spécifique d’application définit les mots et expressions spécifiques qui seront connues à tous les utilisateurs de l’application, telles que les types de véhicules ou noms d’entraînement. Voici la partie de l’application, elles sont définies dans un `AppIntentVocabulary.plist` fichier en tant que partie du regroupement principal de l’application. En outre, ces mots et expressions doivent être localisées.

Termes spécifiques de l’application doivent appartenir à une des catégories suivantes :

- Gérer les Options.
- Noms d’entraînement.

Le fichier de vocabulaire spécifique d’application contient deux clés de niveau racine :

- `ParameterVocabularies` **Requis** -définit des conditions personnalisées et des paramètres d’intention qu’ils s’appliquent à l’application.
- `IntentPhrases` **Facultatif** -contient des expressions utilisant les conditions personnalisées définies dans le `ParameterVocabularies`.

Chaque entrée dans le `ParameterVocabularies` doit spécifier une chaîne d’ID, terme et le terme s’applique à l’intention. En outre, un terme unique peut s’appliquer à plusieurs modes.

Pour obtenir la liste complète des valeurs acceptables et de la structure des fichiers requis, consultez le site d’Apple [référence du Format de fichier de vocabulaire application](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

Pour ajouter un `AppIntentVocabulary.plist` fichier au projet d’application, procédez comme suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Cliquez sur le nom du projet dans le **l’Explorateur de solutions** et sélectionnez **ajouter** > **nouveau fichier...**   >  **iOS**:

    [![](implementing-sirikit-images/plist01.png "Ajouter une liste de propriétés")](implementing-sirikit-images/plist01.png#lightbox)
2. Double-cliquez sur le `AppIntentVocabulary.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
3. Cliquez sur le **+** pour ajouter une clé, définissez la **nom** à `ParameterVocabularies` et **Type** à `Array`:

    [![](implementing-sirikit-images/plist02.png "Le nom de la valeur ParameterVocabularies et le Type de tableau")](implementing-sirikit-images/plist02.png#lightbox)
4. Développez `ParameterVocabularies` et cliquez sur le **+** bouton et définissez la **Type** à `Dictionary`:

    [![](implementing-sirikit-images/plist03.png "Définir le Type de dictionnaire")](implementing-sirikit-images/plist03.png#lightbox)
5. Cliquez sur le **+** pour ajouter une nouvelle clé, définissez la **nom** à `ParameterNames` et **Type** à `Array`:

    [![](implementing-sirikit-images/plist04.png "Le nom de la valeur ParameterNames et le Type de tableau")](implementing-sirikit-images/plist04.png#lightbox)
6. Cliquez sur le **+** pour ajouter une nouvelle clé avec la **Type** de `String` et la valeur qu’un des noms de paramètres disponibles. Par exemple, `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05.png "La clé INStartWorkoutIntent.workoutName")](implementing-sirikit-images/plist05.png#lightbox)
7. Ajouter le `ParameterVocabulary` clé pour la `ParameterVocabularies` clé avec la **Type** de `Array`:

    [![](implementing-sirikit-images/plist06.png "Ajouter la clé ParameterVocabulary à la clé ParameterVocabularies avec le Type de tableau")](implementing-sirikit-images/plist06.png#lightbox)
8. Ajoutez une nouvelle clé avec la **Type** de `Dictionary`:

    [![](implementing-sirikit-images/plist07.png "Ajoutez une nouvelle clé avec le Type de dictionnaire")](implementing-sirikit-images/plist07.png#lightbox)
9. Ajouter le `VocabularyItemIdentifier` clé avec la **Type** de `String` et spécifiez un ID unique pour le terme :

    [![](implementing-sirikit-images/plist08.png "Ajouter la clé VocabularyItemIdentifier avec le Type de chaîne et spécifiez un ID unique")](implementing-sirikit-images/plist08.png#lightbox)
10. Ajouter le `VocabularyItemSynonyms` clé avec la **Type** de `Array`:

    [![](implementing-sirikit-images/plist09.png "Ajouter la clé VocabularyItemSynonyms avec le Type de tableau")](implementing-sirikit-images/plist09.png#lightbox)
11. Ajoutez une nouvelle clé avec la **Type** de `Dictionary`:

    [![](implementing-sirikit-images/plist10.png "Ajoutez une nouvelle clé avec le Type de dictionnaire")](implementing-sirikit-images/plist10.png#lightbox)
12. Ajouter le `VocabularyItemPhrase` clé avec la **Type** de `String` et le terme de l’application définissez :

    [![](implementing-sirikit-images/plist11.png "Ajouter la clé VocabularyItemPhrase avec le Type de chaîne et le terme, définissez l’application")](implementing-sirikit-images/plist11.png#lightbox)
13. Ajouter le `VocabularyItemPronunciation` clé avec la **Type** de `String` et la prononciation phonétique du terme :

    [![](implementing-sirikit-images/plist12.png "Ajouter la clé VocabularyItemPronunciation avec le Type de chaîne et la prononciation phonétique du terme")](implementing-sirikit-images/plist12.png#lightbox)
14. Ajouter le `VocabularyItemExamples` clé avec la **Type** de `Array`:

    [![](implementing-sirikit-images/plist13.png "Ajouter la clé VocabularyItemExamples avec le Type de tableau")](implementing-sirikit-images/plist13.png#lightbox)
15. Ajouter quelques `String` clés avec des exemples d’utilisation du terme :

    [![](implementing-sirikit-images/plist14.png "Ajouter quelques clés de chaîne avec des exemples d’utilisation du terme")](implementing-sirikit-images/plist14.png#lightbox)
16. Répétez les étapes ci-dessus pour les autres conditions personnalisées de l’application doit définir.
17. Réduire le `ParameterVocabularies` clé.
18. Ajouter le `IntentPhrases` clé avec la **Type** de `Array`:

    [![](implementing-sirikit-images/plist15.png "Ajouter la clé IntentPhrases avec le Type de tableau")](implementing-sirikit-images/plist15.png#lightbox)
19. Ajoutez une nouvelle clé avec la **Type** de `Dictionary`:

    [![](implementing-sirikit-images/plist16.png "Ajoutez une nouvelle clé avec le Type de dictionnaire")](implementing-sirikit-images/plist16.png#lightbox)
20. Ajouter le `IntentName` clé avec la **Type** de `String` et l’intention de l’exemple :

    [![](implementing-sirikit-images/plist17.png "Ajouter la clé IntentName avec le Type de chaîne et l’intention de l’exemple")](implementing-sirikit-images/plist17.png#lightbox)
21. Ajouter le `IntentExamples` clé avec la **Type** de `Array`:

    [![](implementing-sirikit-images/plist18.png "Ajouter la clé IntentExamples avec le Type de tableau")](implementing-sirikit-images/plist18.png#lightbox)
22. Ajouter quelques `String` clés avec des exemples d’utilisation du terme :

    [![](implementing-sirikit-images/plist19.png "Ajouter quelques clés de chaîne avec des exemples d’utilisation du terme")](implementing-sirikit-images/plist19.png#lightbox)
23. Répétez les étapes ci-dessus pour les modes de l’application a besoin pour fournir l’exemple d’utilisation de.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Cliquez sur le nom du projet dans le **l’Explorateur de solutions** et sélectionnez **Ajouter > nouvel élément... > Apple > liste de propriétés > Info.plist**:

    [![](implementing-sirikit-images/plist01.w157-sml.png "Ajouter un nouveau fichier Info.plist.")](implementing-sirikit-images/plist01.w157.png#lightbox)

2. Double-cliquez sur le `AppIntentVocabulary.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
3. Cliquez sur le **+** pour ajouter une clé, définissez la **nom** à `ParameterVocabularies` et **Type** à `Array`:

    [![](implementing-sirikit-images/plist02w.png "Le nom de la valeur ParameterVocabularies et le Type de tableau")](implementing-sirikit-images/plist02w.png#lightbox)
4. Développez `ParameterVocabularies` et cliquez sur le **+** bouton et définissez la **Type** à `Dictionary`:

    [![](implementing-sirikit-images/plist03w.png "Définir le Type de dictionnaire")](implementing-sirikit-images/plist03w.png#lightbox)
5. Cliquez sur le **+** pour ajouter une nouvelle clé, définissez la **nom** à `ParameterNames` et **Type** à `Array`:

    [![](implementing-sirikit-images/plist04w.png "Le nom de la valeur ParameterNames et le Type de tableau")](implementing-sirikit-images/plist04w.png#lightbox)
6. Cliquez sur le **+** pour ajouter une nouvelle clé avec la **Type** de `String` et la valeur qu’un des noms de paramètres disponibles. Par exemple, `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05w.png "La clé INStartWorkoutIntent.workoutName")](implementing-sirikit-images/plist05w.png#lightbox)
7. Ajouter le `ParameterVocabulary` clé pour la `ParameterVocabularies` clé avec la **Type** de `Array`:

    [![](implementing-sirikit-images/plist06w.png "Ajouter la clé ParameterVocabulary à la clé ParameterVocabularies avec le Type de tableau")](implementing-sirikit-images/plist06w.png#lightbox)
8. Ajoutez une nouvelle clé avec la **Type** de `Dictionary`:

    [![](implementing-sirikit-images/plist07w.png "Ajoutez une nouvelle clé avec le Type de dictionnaire")](implementing-sirikit-images/plist07w.png#lightbox)
9. Ajouter le `VocabularyItemIdentifier` clé avec la **Type** de `String` et spécifiez un ID unique pour le terme :

    [![](implementing-sirikit-images/plist08w.png "Ajouter la clé VocabularyItemIdentifier avec le Type de chaîne et spécifiez un ID unique pour le terme")](implementing-sirikit-images/plist08w.png#lightbox)
10. Ajouter le `VocabularyItemSynonyms` clé avec la **Type** de `Array`:

    [![](implementing-sirikit-images/plist09w.png "Ajouter la clé VocabularyItemSynonyms avec le Type de tableau")](implementing-sirikit-images/plist09w.png#lightbox)
11. Ajoutez une nouvelle clé avec la **Type** de `Dictionary`:

    [![](implementing-sirikit-images/plist10w.png "Ajoutez une nouvelle clé avec le Type de dictionnaire")](implementing-sirikit-images/plist10w.png#lightbox)
12. Ajouter le `VocabularyItemPhrase` clé avec la **Type** de `String` et le terme de l’application définissez :

    [![](implementing-sirikit-images/plist11w.png "Ajouter la clé VocabularyItemPhrase avec le Type de chaîne et le terme, définissez l’application")](implementing-sirikit-images/plist11w.png#lightbox)
13. Ajouter le `VocabularyItemPronunciation` clé avec la **Type** de `String` et la prononciation phonétique du terme :

    [![](implementing-sirikit-images/plist12w.png "Ajouter la clé VocabularyItemPronunciation avec le Type de chaîne et la prononciation phonétique du terme")](implementing-sirikit-images/plist12w.png#lightbox)
14. Ajouter le `VocabularyItemExamples` clé avec la **Type** de `Array`:

    [![](implementing-sirikit-images/plist13w.png "Ajouter la clé VocabularyItemExamples avec le Type de tableau")](implementing-sirikit-images/plist13w.png#lightbox)
15. Ajouter quelques `String` clés avec des exemples d’utilisation du terme :

    [![](implementing-sirikit-images/plist14w.png "Ajouter quelques clés de chaîne avec des exemples d’utilisation du terme")](implementing-sirikit-images/plist14w.png#lightbox)
16. Répétez les étapes ci-dessus pour les autres conditions personnalisées de l’application doit définir.
17. Réduire le `ParameterVocabularies` clé.
18. Ajouter le `IntentPhrases` clé avec la **Type** de `Array`:

    [![](implementing-sirikit-images/plist15w.png "Ajouter la clé IntentPhrases avec le Type de tableau")](implementing-sirikit-images/plist15w.png#lightbox)
19. Ajoutez une nouvelle clé avec la **Type** de `Dictionary`:

    [![](implementing-sirikit-images/plist16w.png "Ajoutez une nouvelle clé avec le Type de dictionnaire")](implementing-sirikit-images/plist16w.png#lightbox)
20. Ajouter le `IntentName` clé avec la **Type** de `String` et l’intention de l’exemple :

    [![](implementing-sirikit-images/plist17w.png "Ajouter la clé IntentName avec le Type de chaîne et l’intention de l’exemple")](implementing-sirikit-images/plist17w.png#lightbox)
21. Ajouter le `IntentExamples` clé avec la **Type** de `Array`:

    [![](implementing-sirikit-images/plist18w.png "Ajouter la clé IntentExamples avec le Type de tableau")](implementing-sirikit-images/plist18w.png#lightbox)
22. Ajouter quelques `String` clés avec des exemples d’utilisation du terme :

    [![](implementing-sirikit-images/plist19w.png "Ajouter quelques clés de chaîne avec des exemples d’utilisation du terme")](implementing-sirikit-images/plist19w.png#lightbox)
23. Répétez les étapes ci-dessus pour les modes de l’application a besoin pour fournir l’exemple d’utilisation de.

-----

> [!IMPORTANT]
> Le `AppIntentVocabulary.plist` sera enregistrée avec Siri sur le test de périphériques pendant le développement et il peuvent prendre un certain temps pour Siri à intégrer le vocabulaire personnalisé. Par conséquent, le testeur devrez patienter quelques minutes avant de tester le vocabulaire spécifique d’application lorsqu’il a été mis à jour.

Pour plus d’informations, consultez notre [référence vocabulaire spécifique d’application](~/ios/platform/sirikit/understanding-sirikit.md) et d’Apple [en spécifiant une référence de vocabulaire personnalisé](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

## <a name="adding-an-intents-extension"></a>Ajout d’une Extension de modes

Maintenant que l’application a été préparée à adopter SiriKit, le développeur doit ajouter des Extensions de modes d’un (ou plus) à la solution pour gérer les intentions requises pour l’intégration de Siri.

Pour chaque Extension intentions requise, procédez comme suit :

- Ajouter un projet d’Extension de modes à la solution d’application Xamarin.iOS.
- Configurer l’Extension intentions `Info.plist` fichier.
- Modifiez la classe principale intentions Extension.

Pour plus d’informations, consultez notre [la référence d’Extension intentions](~/ios/platform/sirikit/understanding-sirikit.md) et d’Apple [création de la référence d’Extension d’intentions](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1).

### <a name="creating-the-extension"></a>Création de l’Extension

Pour ajouter une Extension de modes à la solution, procédez comme suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Avec le bouton droit sur le **nom de la Solution** dans les **Solution remplissage** et sélectionnez **ajouter** > **ajouter un nouveau projet...** .
2. Dans la boîte de dialogue Sélectionnez **iOS** > **Extensions** > **intention de l’Extension** et cliquez sur le **suivant** bouton : 

    [![](implementing-sirikit-images/intents05.png "Sélectionnez l’Extension Intent")](implementing-sirikit-images/intents05.png#lightbox)
3. Entrez ensuite une **nom** pour l’intention de l’Extension et cliquez sur le **suivant** bouton : 

    [![](implementing-sirikit-images/intents06.png "Entrez un nom pour l’Extension Intent")](implementing-sirikit-images/intents06.png#lightbox)
4. Enfin, cliquez sur le **créer** pour ajouter l’Extension de l’intention à la solution d’applications : 

    [![](implementing-sirikit-images/intents07.png "Ajouter l’Extension de l’intention à la solution d’applications")](implementing-sirikit-images/intents07.png#lightbox)
5. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le **références** dossier de l’Extension intention nouvellement créé. Vérifiez le nom du projet de bibliothèque code partagé commun (que l’application créée ci-dessus) et cliquez sur le **OK** bouton : 

    [![](implementing-sirikit-images/intents08.png "Sélectionnez le nom du projet de bibliothèque de code partagé commun")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Avec le bouton droit sur le **nom de la Solution** dans les **l’Explorateur de solutions** et sélectionnez **ajouter** > **ajouter un nouveau projet...** .
2. Dans la boîte de dialogue Sélectionnez **Visual c# > iOS Extensions > intention de l’Extension** et cliquez sur le **suivant** bouton :

    [![](implementing-sirikit-images/intents05.w157-sml.png "Sélectionnez l’Extension Intent")](implementing-sirikit-images/intents05.w157.png#lightbox)
3. Entrez ensuite une **nom** pour l’intention de l’Extension et cliquez sur le **OK** bouton.
1. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le **références** dossier de l’Extension intentions nouvellement créé et choisissez **Ajouter > référence**. Vérifiez le nom du projet de bibliothèque code partagé commun (que l’application créée ci-dessus) et cliquez sur le **OK** bouton :

    [![](implementing-sirikit-images/intents08w.png "Sélectionnez le nom du projet de bibliothèque de code partagé commun")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

Répétez ces étapes pour le nombre d’Extensions de l’intention (basé sur [architecture de l’application pour les Extensions](#Architecting-the-App-for-Extensions) ci-dessus) qui nécessite l’application.

### <a name="configuring-the-infoplist"></a>Configurer le fichier Info.plist.

Pour chacune des Extensions intentions que vous avez ajouté à la solution de l’application, doivent être configurés dans le `Info.plist` fichiers fonctionnent avec l’application.

Tout comme les extensions d’application par défaut, l’application aura les clés existantes de `NSExtension` et `NSExtensionAttributes`. Pour une Extension intentions, il existe deux nouveaux attributs qui doivent être configurés :

[![](implementing-sirikit-images/intents01.png "Les deux nouveaux attributs qui doivent être configurés.")](implementing-sirikit-images/intents01.png#lightbox)

- **IntentsSupported** - est obligatoire et se compose d’un tableau de noms de classe de l’intention afin de pouvoir prendre en charge de l’Extension de l’intention de l’application.
- **IntentsRestrictedWhileLocked** -est une clé facultative pour l’application spécifier le comportement d’écran de verrouillage de l’extension. Il se compose d’un tableau de noms de classe de l’intention veut exigent que l’utilisateur connecté à utiliser à partir de l’Extension de l’intention de l’application.

Pour configurer l’Extension d’intention `Info.plist` , double-cliquez dessus dans le **l’Explorateur de solutions** à ouvrir pour le modifier. Ensuite, basculez vers le **Source** afficher, puis développez le `NSExtension` et `NSExtensionAttributes` clés dans l’éditeur :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents02.png "Les clés NSExtension et NSExtensionAttributes dans l’éditeur")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents02w.png "Les clés NSExtension et NSExtensionAttributes dans l’éditeur")](implementing-sirikit-images/intents02w.png#lightbox)

-----

Développez le `IntentsSupported` de clé et ajoutez le nom de n’importe quelle classe d’intention de cette extension prend en charge. Pour l’exemple d’application MonkeyChat, il prend en charge la `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents09.png "La clé INSendMessageIntent")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents09w.png "La clé INSendMessageIntent")](implementing-sirikit-images/intents09w.png#lightbox)

-----

Si l’application nécessite éventuellement que l’utilisateur soit connecté à l’appareil pour utiliser une intention donnée, développez le `IntentRestrictedWhileLocked` de clé et ajoutez les noms de classe des intentions à accès limité. Pour l’exemple d’application MonkeyChat, l’utilisateur doit être connecté pour envoyer un message de conversation afin que nous avons ajouté `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents10.png "La clé INSendMessageIntent ajoutée")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents10w.png "La clé INSendMessageIntent ajoutée")](implementing-sirikit-images/intents10w.png#lightbox)

-----


Pour obtenir une liste complète des domaines intention disponibles, consultez le site d’Apple [intention de domaines de référence](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Configuration de la classe principale

Ensuite, le développeur doit configurer la classe principale qui agit en tant que point d’entrée principal pour l’Extension de l’intention dans Siri. Il doit être une sous-classe de `INExtension` qui est conforme à la `IINIntentHandler` déléguer. Par exemple :

```csharp
using System;
using System.Collections.Generic;

using Foundation;
using Intents;

namespace MonkeyChatIntents
{
    [Register ("IntentHandler")]
    public class IntentHandler : INExtension, IINSendMessageIntentHandling, IINSearchForMessagesIntentHandling, IINSetMessageAttributeIntentHandling
    {
        #region Constructors
        protected IntentHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override NSObject GetHandler (INIntent intent)
        {
            // This is the default implementation.  If you want different objects to handle different intents,
            // you can override this and return the handler you want for that particular intent.

            return this;
        }
        #endregion
        ...
    }
}
```

Il existe une méthode Solitaire que l’application doit implémenter sur la classe principale d’intention de l’Extension, le `GetHandler` (méthode). Cette méthode est passée à une intention par SiriKit et l’application doit retourner un **intention gestionnaire** qui correspond au type de l’intention de donnée.

Étant donné que l’exemple d’application MonkeyChat gère uniquement un seul objectif, elle retourne lui-même dans le `GetHandler` (méthode). Si l’extension géré plusieurs intention, le développeur aurait ajouter une classe pour chaque type d’intention et retourner une instance ici selon le `Intent` passé à la méthode.

### <a name="handling-the-resolve-stage"></a>Gestion de la phase de résolution

La phase de résolution est où l’intention de l’Extension sera clarifier et valider les paramètres transmis à partir de Siri et ont été définies par la conversation de l’utilisateur.

Pour chaque paramètre qui est envoyée à partir de Siri, il existe un `Resolve` (méthode). L’application doit implémenter cette méthode pour chaque paramètre que l’application peut-être aide de Siri pour obtenir la réponse correcte à partir de l’utilisateur.

Dans le cas de l’exemple d’application MonkeyChat, l’Extension de l’intention de nécessitera une un ou plusieurs destinataires envoyer le message. Pour chaque destinataire dans la liste, l’extension devez effectuer une recherche de contact qui peut avoir les résultats suivants :

- Un seul contact correspondant est trouvé.
- Deux ou plusieurs contacts correspondants sont trouvés.
- Aucun contact correspondant n’est trouvé.

En outre, MonkeyChat requiert le contenu pour le corps du message. Si l’utilisateur n’a pas fourni cela, Siri doit inviter l’utilisateur pour le contenu.

Vous devez l’Extension de l’intention de façon à gérer chacun de ces cas.


```csharp
[Export ("resolveRecipientsForSearchForMessages:withCompletion:")]
public void ResolveRecipients (INSendMessageIntent intent, Action<INPersonResolutionResult []> completion)
{
    var recipients = intent.Recipients;
    // If no recipients were provided we'll need to prompt for a value.
    if (recipients.Length == 0) {
        completion (new INPersonResolutionResult [] { (INPersonResolutionResult)INPersonResolutionResult.NeedsValue });
        return;
    }

    var resolutionResults = new List<INPersonResolutionResult> ();

    foreach (var recipient in recipients) {
        var matchingContacts = new INPerson [] { recipient }; // Implement your contact matching logic here to create an array of matching contacts
        if (matchingContacts.Length > 1) {
            // We need Siri's help to ask user to pick one from the matches.
            resolutionResults.Add (INPersonResolutionResult.GetDisambiguation (matchingContacts));
        } else if (matchingContacts.Length == 1) {
            // We have exactly one matching contact
            resolutionResults.Add (INPersonResolutionResult.GetSuccess (recipient));
        } else {
            // We have no contacts matching the description provided
            resolutionResults.Add ((INPersonResolutionResult)INPersonResolutionResult.Unsupported);
        }
    }

    completion (resolutionResults.ToArray ());
}

[Export ("resolveContentForSendMessage:withCompletion:")]
public void ResolveContent (INSendMessageIntent intent, Action<INStringResolutionResult> completion)
{
    var text = intent.Content;
    if (!string.IsNullOrEmpty (text))
        completion (INStringResolutionResult.GetSuccess (text));
    else
        completion ((INStringResolutionResult)INStringResolutionResult.NeedsValue);
}
```

Pour plus d’informations, consultez notre [résoudre la référence des étape](~/ios/platform/sirikit/understanding-sirikit.md) et d’Apple [résolution et référence de modes de gestion des](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1).

### <a name="handling-the-confirm-stage"></a>Gestion de l’étape de confirmation

L’étape de confirmation est où l’Extension intention vérifie qu’il possède toutes les informations pour répondre à la demande de l’utilisateur. L’application souhaite envoyer sera de confirmation le long de tous les détails de prise en charge de ce qui est sur de se produire à Siri afin qu’il peut être confirmée par l’utilisateur que c’est l’action prévue.

```csharp
[Export ("confirmSendMessage:completion:")]
public void ConfirmSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Verify user is authenticated and the app is ready to send a message.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Ready, userActivity);
    completion (response);
}
```

Pour plus d’informations, consultez notre [la référence d’étape confirmer](~/ios/platform/sirikit/understanding-sirikit.md).

### <a name="processing-the-intent"></a>L’objectif de traitement

Il s’agit du point où l’Extension intention effectue la tâche de répondre à la demande de l’utilisateur et passer les résultats à Siri afin de l’utilisateur soit informé.


```csharp
public void HandleSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Implement the application logic to send a message here.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, userActivity);
    completion (response);
}

public void HandleSearchForMessages (INSearchForMessagesIntent intent, Action<INSearchForMessagesIntentResponse> completion)
{
    // Implement the application logic to find a message that matches the information in the intent.
    ...

    var userActivity = new NSUserActivity (nameof (INSearchForMessagesIntent));
    var response = new INSearchForMessagesIntentResponse (INSearchForMessagesIntentResponseCode.Success, userActivity);

    // Initialize with found message's attributes
    var sender = new INPerson (new INPersonHandle ("sarah@example.com", INPersonHandleType.EmailAddress), null, "Sarah", null, null, null);
    var recipient = new INPerson (new INPersonHandle ("+1-415-555-5555", INPersonHandleType.PhoneNumber), null, "John", null, null, null);
    var message = new INMessage ("identifier", "I am so excited about SiriKit!", NSDate.Now, sender, new INPerson [] { recipient });
    response.Messages = new INMessage [] { message };
    completion (response);
}

public void HandleSetMessageAttribute (INSetMessageAttributeIntent intent, Action<INSetMessageAttributeIntentResponse> completion)
{
    // Implement the application logic to set the message attribute here.
    ...

    var userActivity = new NSUserActivity (nameof (INSetMessageAttributeIntent));
    var response = new INSetMessageAttributeIntentResponse (INSetMessageAttributeIntentResponseCode.Success, userActivity);
    completion (response);
}
```

Pour plus d’informations, consultez notre [la référence d’étape gérer](~/ios/platform/sirikit/understanding-sirikit.md).

## <a name="adding-an-intents-ui-extension"></a>Ajout d’une Extension de l’interface utilisateur de modes

L’Extension de l’interface utilisateur facultative intentions présente la possibilité d’afficher l’interface utilisateur de l’application et les personnalisations dans l’expérience de Siri et rendre les utilisateurs sentir connecté à l’application. Avec cette extension de l’application pour faire passer la marque, ainsi que visual et d’autres informations dans la transcription.

[![](implementing-sirikit-images/intentsui01.png "Un exemple de sortie d’Extension de l’interface utilisateur de modes")](implementing-sirikit-images/intentsui01.png#lightbox)

Tout comme l’Extension intentions, le développeur effectuera l’étape suivante pour l’Extension de l’interface utilisateur de modes :

- Ajouter un projet d’Extension de l’interface utilisateur de modes à la solution d’application Xamarin.iOS.
- Configurer l’Extension de l’interface utilisateur intentions `Info.plist` fichier.
- Modifiez la classe principale d’Extension de l’interface utilisateur d’intentions.

Pour plus d’informations, consultez notre [la référence d’Extension de l’interface utilisateur intentions](~/ios/platform/sirikit/understanding-sirikit.md) et d’Apple [en fournissant une référence de l’Interface personnalisée](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1).

### <a name="creating-the-extension"></a>Création de l’Extension

Pour ajouter une Extension de l’interface utilisateur de modes à la solution, procédez comme suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Avec le bouton droit sur le **nom de la Solution** dans les **Solution remplissage** et sélectionnez **ajouter** > **ajouter un nouveau projet...** .
2. Dans la boîte de dialogue Sélectionnez **iOS** > **Extensions** > **intention de l’Extension d’interface utilisateur** et cliquez sur le **suivant** bouton : 

    [![](implementing-sirikit-images/intents11.png "Sélectionnez l’Extension de l’intention de l’interface utilisateur")](implementing-sirikit-images/intents11.png#lightbox)
3. Entrez ensuite une **nom** pour l’intention de l’Extension et cliquez sur le **suivant** bouton : 

    [![](implementing-sirikit-images/intents12.png "Entrez un nom pour l’Extension Intent")](implementing-sirikit-images/intents12.png#lightbox)
4. Enfin, cliquez sur le **créer** pour ajouter l’Extension de l’intention à la solution d’applications : 

    [![](implementing-sirikit-images/intents13.png "Ajouter l’Extension de l’intention à la solution d’applications")](implementing-sirikit-images/intents13.png#lightbox)
5. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le **références** dossier de l’Extension intention nouvellement créé. Vérifiez le nom du projet de bibliothèque code partagé commun (que l’application créée ci-dessus) et cliquez sur le **OK** bouton : 

    [![](implementing-sirikit-images/intents14.png "Sélectionnez le nom du projet de bibliothèque de code partagé commun")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Avec le bouton droit sur le **nom de la Solution** dans les **l’Explorateur de solutions** et sélectionnez **ajouter** > **ajouter un nouveau projet...**
2. Dans la boîte de dialogue Sélectionnez **iOS** > **Extensions** > **intention de l’Extension d’interface utilisateur** et cliquez sur le **suivant** bouton.
3. Entrez ensuite une **nom** pour l’intention de l’Extension et cliquez sur le **OK** bouton.
5. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le **références** dossier de l’Extension intention nouvellement créé. Vérifiez le nom du projet de bibliothèque code partagé commun (que l’application créée ci-dessus) et cliquez sur le **OK** bouton.
    
-----

### <a name="configuring-the-infoplist"></a>Configurer le fichier Info.plist.

Configurer l’Extension de l’interface utilisateur d’intentions `Info.plist` fichier fonctionne avec l’application.

Tout comme les extensions d’application par défaut, l’application aura les clés existantes de `NSExtension` et `NSExtensionAttributes`. Pour une Extension intentions, il existe un nouvel attribut qui doit être configuré :

[![](implementing-sirikit-images/intents03.png "L’un nouvel attribut qui doit être configuré.")](implementing-sirikit-images/intents03.png#lightbox)

**IntentsSupported** est obligatoire et se compose d’un tableau de noms de classe de l’intention voulue pour prendre en charge à partir de l’Extension de l’intention de l’application.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour configurer l’Extension de l’interface utilisateur d’intention `Info.plist` , double-cliquez dessus dans le **l’Explorateur de solutions** à ouvrir pour le modifier. Ensuite, basculez vers le **Source** afficher, puis développez le `NSExtension` et `NSExtensionAttributes` clés dans l’éditeur :

[![](implementing-sirikit-images/intents04.png "Les clés NSExtension et NSExtensionAttributes dans l’éditeur")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour configurer l’Extension de l’interface utilisateur d’intention `Info.plist` , double-cliquez dessus dans le **l’Explorateur de solutions** à ouvrir pour le modifier. Développez le `NSExtension` et `NSExtensionAttributes` clés dans l’éditeur :

[![](implementing-sirikit-images/intents04w.png "Clés Tthe NSExtension et NSExtensionAttributes dans l’éditeur")](implementing-sirikit-images/intents04w.png#lightbox)

-----

Développez le `IntentsSupported` de clé et ajoutez le nom de n’importe quelle classe d’intention de cette extension prend en charge. Pour l’exemple d’application MonkeyChat, il prend en charge la `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents15.png "La clé INSendMessageIntent")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents15w.png "La clé INSendMessageIntent")](implementing-sirikit-images/intents15w.png#lightbox)

-----

Pour obtenir une liste complète des domaines intention disponibles, consultez le site d’Apple [intention de domaines de référence](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Configuration de la classe principale

Configurer la classe principale qui agit en tant que point d’entrée principal pour l’Extension de l’interface utilisateur intention dans Siri. Il doit être une sous-classe de `UIViewController` qui est conforme à la `IINUIHostedViewController` interface. Par exemple :

```csharp
using System;
using Foundation;
using CoreGraphics;
using Intents;
using IntentsUI;
using UIKit;

namespace MonkeyChatIntentsUI
{
    public partial class IntentViewController : UIViewController, IINUIHostedViewControlling
    {
        #region Constructors
        protected IntentViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }

        public override void DidReceiveMemoryWarning ()
        {
            // Releases the view if it doesn't have a superview.
            base.DidReceiveMemoryWarning ();

            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Public Methods
        [Export ("configureWithInteraction:context:completion:")]
        public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
        {
            // Do configuration here, including preparing views and calculating a desired size for presentation.

            if (completion != null)
                completion (DesiredSize ());
        }

        [Export ("desiredSize:")]
        public CGSize DesiredSize ()
        {
            return ExtensionContext.GetHostedViewMaximumAllowedSize ();
        }
        #endregion
    }
}
```

Siri passe un `INInteraction` instance de classe de la `Configure` méthode de la `UIViewController` instance à l’intérieur de l’Extension de l’interface utilisateur intention.

Le `INInteraction` objet fournit trois éléments d’information à l’extension de clés :

1. L’objet d’intention en cours de traitement.
2. L’objet de réponse de l’intention de le `Confirm` et `Handle` les méthodes de l’Extension de l’intention.
3. L’état d’Interaction qui définit l’état de l’interaction entre l’application et Siri.

Le `UIViewController` instance est la classe de principe pour l’interaction avec Siri et, car il hérite de `UIViewController`, il a accès à toutes les fonctionnalités de UIKit.

Lorsque Siri appelle la `Configure` méthode de le `UIViewController` elle passe dans un contexte d’affichage indiquant que le contrôleur de la vue soit soit hébergé dans un Siri Snippit ou une carte de cartes.

Siri passez également un gestionnaire d’achèvement qui l’application doit retourner la taille souhaitée de l’affichage une fois que l’application a terminé sa configuration.

### <a name="design-the-ui-in-ios-designer"></a>Concevoir l’interface utilisateur dans le Concepteur d’iOS

Mise en page dans le concepteur iOS de l’interface utilisateur de l’Extension l’interface utilisateur intentions. Double-cliquez sur l’extension `MainInterface.storyboard` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier. Faites glisser dans tous les éléments d’interface utilisateur requis pour générer l’interface utilisateur et enregistrer les modifications.

> [!IMPORTANT]
> Bien qu’il soit possible d’ajouter des éléments interactifs comme `UIButtons` ou `UITextFields` à l’Extension de l’interface utilisateur d’intention `UIViewController`, ceux-ci sont strictement interdites d’intention de l’interface utilisateur dans non interactive et l’utilisateur ne sera pas en mesure d’interagir avec eux.

### <a name="wire-up-the-user-interface"></a>L’Interface utilisateur d’accès réseau

Interface d’utilisateur de l’Extension de l’interface utilisateur intentions créé dans le Concepteur d’iOS, de modifier le `UIViewController` sous-classe et que vous remplacez le `Configure` méthode comme suit :

```csharp
[Export ("configureWithInteraction:context:completion:")]
public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
{
    // Do configuration here, including preparing views and calculating a desired size for presentation.
    ...

    // Return desired size
    if (completion != null)
        completion (DesiredSize ());
}

[Export ("desiredSize:")]
public CGSize DesiredSize ()
{
    return ExtensionContext.GetHostedViewMaximumAllowedSize ();
}
```

### <a name="overriding-the-default-siri-ui"></a>Substitution de l’interface utilisateur de Siri par défaut

L’Extension de l’interface utilisateur intentions seront toujours affichée, ainsi que tout autre contenu Siri telles que l’icône de l’application et le nom en haut de l’interface utilisateur ou, selon l’intention, les boutons (par exemple, envoyer ou annuler) peut être affiché en bas.

Il existe quelques cas où l’application peut remplacer les informations qui Siri affiche à l’utilisateur par défaut, telles que la messagerie ou les mappages où l’application peut remplacer l’expérience par défaut avec un adaptés à l’application.

Si l’Extension de l’interface utilisateur intentions doit remplacer les éléments de la valeur par défaut Siri UI, le `UIViewController` sous-classe devrez implémenter le `IINUIHostedViewSiriProviding` interface et participer à l’affichage d’un élément d’interface particulier.

Ajoutez le code suivant à la `UIViewController` sous-classe indiquer Siri que l’Extension de l’interface utilisateur intention déjà affiche le contenu du message :

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>Éléments à prendre en considération

Apple suggère que le développeur prendre en compte lors de la conception et implémentation les Extensions de l’interface utilisateur d’intention des points suivants :

- **Être conscients de l’utilisation de mémoire** - Extensions de l’interface utilisateur intention car sont temporaires et seule affichée pendant une courte période, le système impose des contraintes de mémoire plus strictes que sont utilisés avec une application complète.
- **Prendre en compte au Minimum et Maximum tailles d’affichage** -Vérifiez que les Extensions de l’interface utilisateur intention semble correcte sur chaque type d’appareil iOS, la taille et l’orientation. En outre, la taille souhaitée que l’application envoie à Siri peut-être pas à accorder.
- **Utiliser des modèles de mise en page adaptative et flexibles** - là encore, pour vous assurer que l’interface utilisateur semble sur chaque appareil.

## <a name="summary"></a>Récapitulatif

Cet article a couvert SiriKit et montre comment elle peut être ajoutée pour les applications Xamarin.iOS pour fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri et l’application de cartes sur un appareil iOS.




## <a name="related-links"></a>Liens associés

- [Exemple de ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Guide de programmation SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Référence de Framework intentions](https://developer.apple.com/reference/intents)
- [Référence de Framework intentions l’interface utilisateur](https://developer.apple.com/reference/intentsui)
- [Référence de l’intention de domaines](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
