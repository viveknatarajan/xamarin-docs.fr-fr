---
title: "Comment resynchroniser manuellement les licences Xamarin ?"
ms.topic: article
ms.prod: xamarin
ms.assetid: D0BD93E9-3A1F-4E5B-8EE8-36ADC33DCFE4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 2413b6b7563a6ed1e17a8db61d2d61ddc85e71ae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-manually-resynchronize-xamarin-licenses"></a>Comment resynchroniser manuellement les licences Xamarin ?

> [!IMPORTANT]
> Ce guide ne s’applique pas à la plupart des utilisateurs MSDN car ils ne sont pas requis ou se connecter à des comptes de Xamarin, sauf si à l’aide de la [stocker les composants Xamarin](https://components.xamarin.com/) ou [Visual Studio pour Mac (Mac)](~/cross-platform/get-started/requirements.md).




## <a name="overview"></a>Vue d'ensemble

Généralement vos informations de licence seront il faut resynchroniser avec le serveur de licences Xamarin automatiquement lorsque vous démarrez l’IDE. Par exemple, rétrogradations, les renouvellements et les mises à niveau de la licence seront affiche normalement automatiquement après le redémarrage de l’IDE. Les informations de licence affichées dans l’IDE doivent correspondre les informations affichées sur votre [page compte](https://store.xamarin.com/account/my/subscription/computers). Si les informations sont toujours incompatibles, vous pouvez tout d’abord Assurez-vous que vos informations de compte du magasin d’est correctes, puis resynchroniser manuellement les licences par la déconnexion, la suppression des fichiers de licence sur le disque, puis connectez-vous.

### <a name="note-for-ios-developers-on-windows"></a>Remarque pour les développeurs iOS sous Windows

les développeurs iOS sous Windows devra peut-être également effectuer ces étapes pour Visual Studio pour Mac ; même si vous ne développez directement sur votre hôte Mac générer apparié.

## <a name="quick-manual-refresh-steps"></a>Étapes de l’actualisation manuelle rapide

Ce processus rapide ignore l’étape de la suppression des fichiers de licence sur le disque qui peut être suffisante dans certains cas. 

1.  Déconnecter votre compte Xamarin via l’IDE :
    -   Visual Studio pour Mac
        -   Angle supérieur droit de l’écran d’accueil
        -   **Visual Studio pour Mac > compte** (Mac)
        -   **Outils > compte** (Windows)
    -   Visual Studio
        -   **Outils > compte Xamarin**
2.  Reconnectez-vous à votre compte Xamarin dans l’IDE.

## <a name="extended-manual-license-refresh-steps"></a>Étendue des étapes de l’actualisation manuelle de licence

1.  Déconnectez-vous de votre compte de Xamarin via l’IDE. 
2.  Quittez l’IDE.
3.  Vérifiez que les informations de compte de stockage sont correctes. Vérifiez en particulier pour les noms d’ordinateur en double sur le [page ordinateurs](https://store.xamarin.com/account/my/subscription/computers).

4.  Si vous voyez une paire de noms d’ordinateur en double, utilisez le **Deactivate** élément de menu déroulant à supprimer _les deux_ membres de la paire :
    
    ![Licence -> Deactivate sur https://store.xamarin.com/account/my/subscription/computers](resync-licenses-images/deactivate.png "l’élément de menu déroulant Deactivate permet de supprimer les deux membres de la paire")

5.  Supprimer toutes les autres copies de fichiers de licence toujours présentes sur le disque.
    -   Windows

        Supprimez tous les dossiers suivants :
        -   `%PROGRAMDATA%\Mono for Android`
        -   `%PROGRAMDATA%\MonoTouch`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\Mono for Android`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\MonoTouch`

        Dans les installations par défaut de Windows :
        -   `%PROGRAMDATA%` se développe pour `C:\ProgramData`
        -   `%LOCALAPPDATA%` se développe pour `C:\Users\%USERNAME%\AppData\Local`

        Le `VirtualStore` copies des licences sont créées automatiquement par Windows dans certains scénarios. Si les copies VirtualStore existent, elles ne seront lues _à la place_ des licences dans `%PROGRAMDATA%`.

    -   Mac

        Supprimez tous les dossiers suivants :

        -   `~/Library/MonoAndroid`
        -   `~/Library/MonoTouch`
        -   `~/Library/Xamarin.Mac`

        Vous pouvez accéder à ces chemins d’accès dans **recherche** en sélectionnant le **accédez > accédez au dossier** menu et en les collant dans la boîte de dialogue. Finder remplace automatiquement le ~ caractère tilde avec votre dossier utilisateur.

6.  Ouvrez Visual Studio pour Mac ou Visual Studio et reconnectez-vous à votre compte Xamarin.

## <a name="supplementary-information-individual-license-file-locations"></a>Informations supplémentaires : les emplacements de fichiers de licence individuelle

### <a name="windows"></a>Windows

Sur Windows, les fichiers de licence individuels sont stockés dans les emplacements suivants :

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.licx`

Les licences de *d’évaluation* abonnements sont nommés différemment :

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.trial.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.trial.licx`

### <a name="mac"></a>Mac

Sur Mac, les fichiers de licence individuels sont stockés dans les emplacements suivants :

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.v2`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License`

Les licences de *d’évaluation* abonnements sont nommés différemment :

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License.trial`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.trial`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License.trial`

## <a name="additional-troubleshooting-steps"></a>Étapes de dépannage supplémentaires

-   Vérifiez si vous avez tous les caractères non-ASCII dans votre nom d’utilisateur, nom de votre ordinateur ou dans un des chemins de fichiers de licence complètement développés.

-   [Contactez le Support de développement Xamarin](http://xamarin.com/support)
