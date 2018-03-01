---
title: Erreurs de Xamarin.iOS
ms.topic: article
ms.prod: xamarin
ms.assetid: 9F76162B-D622-45DA-996B-2FBF8017E208
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/26/2017
ms.openlocfilehash: 5a11ca19de7ce06088478f1a39dae5a246d701a8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinios-errors"></a>Erreurs de Xamarin.iOS


## <a name="mt0xxx-mtouch-error-messages"></a>MT0xxx : messages d’erreur mtouch

E.g. paramètres, environnement, manquant d’outils.

<!--
 MT0xxx mtouch itself, e.g. parameters, environment (e.g. missing tools)
 https://github.com/xamarin/xamarin-macios/blob/master/tools/mtouch/error.cs
    -->

### <a name="a-namemt0000mt0000-unexpected-error---please-fill-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT0000"/>MT0000 : Une erreur inattendue - Veuillez remplir un rapport de bogue à http://bugzilla.xamarin.com

Une condition d’erreur inattendue s’est produite. Veuillez [un rapport de bogue](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec autant d’informations que possible, notamment :

* Journaux, de la génération complète avec un maximum de commentaires (par exemple, `-v -v -v -v` dans les **mtouch supplémentaires arguments**) ;
* Un cas de test minimale que reproduire l’erreur ; et
* Toutes les informations de version

Pour obtenir des informations sur la version exacte le plus simple consiste à utiliser le **Visual Studio pour Mac** menu, **sur Visual Studio pour Mac** élément, **afficher les détails** bouton et copier/coller le les informations de version (vous pouvez utiliser la **copier les informations** bouton).

### <a name="a-namemt0001mt0001--devname-was-provided-without-any-device-specific-action"></a><a name="MT0001"/>MT0001 : '-nom_unité ' a été fourni sans qu’aucune action spécifique au périphérique

Il s’agit d’un avertissement est émis si - nom_unité est transmis à mtouch lorsqu’aucune action spécifique au périphérique (-logdev/installdev/killdev/launchdev /-listapps) a été demandée.

### <a name="a-namemt0002mt0002-could-not-parse-the-environment-variable-"></a><a name="MT0002"/>MT0002 : N’a pas pu analyser la variable d’environnement *.

Cette erreur se produit si vous essayez de définir une clé d’environnement non valide = paire de variables de valeur. Le format correct est : `mtouch --setenv=VARIABLE=VALUE`

### <a name="a-namemt0003mt0003-application-name-exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a><a name="MT0003"/>MT0003 : Nom de l’Application « * .exe ' est en conflit avec un nom d’assembly (.dll) SDK ou du produit.

Nom de l’assembly exécutable et le nom de l’application ne peut pas correspondre au nom de n’importe quelle dll dans l’application. Modifiez le nom de votre fichier exécutable.

### <a name="a-namemt0004mt0004-new-refcounting-logic-requires-sgen-to-be-enabled-too"></a><a name="MT0004"/>MT0004 : La nouvelle logique de refcounting nécessite SGen doit être activée en trop.

Si vous activez l’extension refcounting, vous devez également activer le SGen garbage collector dans e/s du projet options de Build (onglet Avancé).

À compter de Xamarin.iOS 7.2.1). cette exigence a été levée, la nouvelle logique de refcounting peut être activée avec Boehm et SGen Garbage Collectors.

### <a name="a-namemt0005mt0005-the-output-directory--does-not-exist"></a><a name="MT0005"/>MT0005 : Le répertoire de sortie * n’existe pas.

Créez le répertoire.

Cette erreur n’est pas générée plus, mtouch crée automatiquement le répertoire s’il n’existe.

### <a name="a-namemt0006mt0006-there-is-no-devel-platform-at--use---platformplat-to-specify-the-sdk"></a><a name="MT0006"/>MT0006 : Il n’existe aucune plateforme de développement au *, utilisez--platform = PLAT pour spécifier le Kit de développement.

Xamarin.iOS Impossible de trouver le répertoire SDK à l’emplacement mentionné dans le message d’erreur. Vérifiez que le chemin d’accès est correct.

### <a name="a-namemt0007mt0007-the-root-assembly--does-not-exist"></a><a name="MT0007"/>MT0007 : L’assembly racine * n’existe pas.

Xamarin.iOS ne peut pas trouver l’assembly à l’emplacement mentionné dans le message d’erreur. Vérifiez que le chemin d’accès est correct.

### <a name="a-namemt0008mt0008-you-should-provide-one-root-assembly-only-found--assemblies-"></a><a name="MT0008"/>MT0008 : Vous devez fournir une racine assembly # uniquement, il est trouvé assemblys : *.

Plusieurs assemblys racine a été passé à mtouch, peuvent être assembly qu’une seule racine.

### <a name="a-namemt0009mt0009-error-while-loading-assemblies-"></a><a name="MT0009"/>MT0009 : Erreur lors du chargement des assemblys : *.

Une erreur s’est produite lors du chargement des assemblys les références d’assembly racine. Plus d’informations peuvent être fournis dans la sortie de génération.

### <a name="a-namemt0010mt0010-could-not-parse-the-command-line-arguments-"></a><a name="MT0010"/>MT0010 : N’a pas pu analyser les arguments de ligne de commande : *.

Une erreur s’est produite lors de l’analyse des arguments de ligne de commande. Veuillez vérifier qu’ils sont tous corrects.

### <a name="a-namemt0011mt0011--was-built-against-a-more-recent-runtime--than-monotouch-supports"></a><a name="MT0011"/>MT0011 : * a été créé avec un runtime plus récent (*) que MonoTouch prend en charge.

Cet avertissement est généralement signalé, car le projet a une référence à une bibliothèque de classes qui n’a pas été générée à l’aide de Xamarin.iOS BCL.

La même façon qu’une application en utilisant le Kit de développement .NET 4.0 peut ne pas fonctionne sur un système uniquement prise en charge de .NET 2.0, une bibliothèque générée à l’aide de .NET 4.0 peut ne pas fonctionne sur Xamarin.iOS, il peut utiliser des API non présent sur Xamarin.iOS.

La solution générale consiste à générer la bibliothèque sous la forme d’une bibliothèque de classes Xamarin.iOS. Cela est possible en créant un nouveau projet de bibliothèque de classes Xamarin.iOS et ajoutez-y tous les fichiers sources. Si vous n’avez pas le code source pour la bibliothèque, vous devez contacter le fournisseur et demander qu’ils fournissent une version compatible avec Xamarin.iOS de leur bibliothèque.

### <a name="a-namemt0012mt0012-incomplete-data-is-provided-to-complete-"></a><a name="MT0012"/>MT0012 : Des données incomplètes sont fournies pour l’exécution *.

Cette erreur n’est pas signalée plus dans la version actuelle de Xamarin.iOS.

### <a name="a-namemt0013mt0013-profiling-support-requires-sgen-to-be-enabled-too"></a><a name="MT0013"/>MT0013 : Prise en charge du profilage requiert sgen doit être activée en trop.

SGen (--sgen) doit être activée si le profilage (--profilage) est activé.

### <a name="a-namemt0014mt0014-the-ios--sdk-does-not-support-building-applications-targeting-"></a><a name="MT0014"/>MT0014 : IOS * Kit de développement logiciel ne prend pas en charge la génération d’applications ciblant *.

Cela peut se produire dans les circonstances suivantes :

*  ARMv6 est activée et Xcode 4.5 ou version ultérieure est installé.
*  ARMv7s est activée et Xcode 4.4 ou version antérieure est installé.


Vérifiez que la version installée de Xcode prend en charge les architectures sélectionnées.

### <a name="a-namemt0015mt0015-invalid-abi--supported-abis-are-i386-x8664--armv7-armv7llvm-armv7llvmthumb2-armv7s-armv7sllvm-armv7sllvmthumb2-arm64-and-arm64llvm"></a><a name="MT0015"/>MT0015 : Les ABI non valide : *. ABIs pris en charge sont : i386, x86_64, armv7, armv7 llvm, armv7 + llvm + thumb2, armv7s, armv7s + llvm, armv7s + llvm + thumb2, arm64 et arm64 + llvm.

Un message ABI non valide a été passé à mtouch. Veuillez spécifier un ABI valide.

### <a name="a-namemt0016mt0016-the-option--has-been-deprecated"></a><a name="MT0016"/>MT0016 : L’option * a été déconseillée.

L’option mtouch indiqué a été déconseillée et sera ignorée.

### <a name="a-namemt0017mt0017-you-should-provide-a-root-assembly"></a><a name="MT0017"/>MT0017 : Vous devez fournir un assembly racine.

Il est nécessaire de spécifier un assembly racine (généralement le principal exécutable) lors de la création d’une application.

### <a name="a-namemt0018mt0018-unknown-command-line-argument-"></a><a name="MT0018"/>MT0018 : Argument de ligne de commande inconnu : *.

Mtouch ne reconnaît pas l’argument de ligne de commande indiqué dans le message d’erreur.

### <a name="a-namemt0019mt0019-only-one---loginstallkilllaunchdev-or---launchdebugsim-option-can-be-used"></a><a name="MT0019"/>MT0019 : Une seule--[journal | installer | kill | lancer] dev ou--[lancement | déboguer] sim option peut être utilisée.

Il existe plusieurs options pour mtouch ne peuvent pas être utilisés simultanément :

-  --logdev
-  --installdev
-  --killdev
-  --launchdev
-  --launchdebug
-  --launchsim




### <a name="a-namemt0020mt0020-the-valid-options-for--are-"></a><a name="MT0020"/>MT0020 Les options valides pour '\*'sont'\*'.



### <a name="a-namemt0021mt0021-cannot-compile-using-gccg---use-gcc-when-using-the-static-registrar-this-is-the-default-when-compiling-for-device-either-remove-the---use-gcc-flag-or-use-the-dynamic-registrar---registrardynamic"></a><a name="MT0021"/>MT0021 ne peut pas de compilation à l’aide de gcc / g ++ (--utilisez-gcc) lorsque vous utilisez le bureau d’enregistrement statique (il s’agit de la valeur par défaut lors de la compilation de l’appareil). Supprimez le--utilisez-gcc indicateur ou utiliser le bureau d’enregistrement dynamique (--bureau d’enregistrement : dynamique).



### <a name="a-namemt0022mt0022-the-options---unsupported--enable-generics-in-registrar-and---registrar-are-not-compatible"></a><a name="MT0022"/>MT0022 Options '--non pris en charge--enable-génériques-de-bureau d’enregistrement de' et '--bureau d’enregistrement ' ne sont pas compatibles.

Supprimez les deux options `--unsupported--enable-generics-in-registrar` et `--registrar`. À compter de Xamarin.iOS 7.2.1). le bureau d’enregistrement par défaut prend en charge les génériques.

Cette erreur est indiquée n’est plus (l’argument de ligne de commande `--unsupported--enable-generics-in-registrar` a été supprimé de mtouch).

### <a name="a-namemt0023mt0023-application-name-exe-conflicts-with-another-user-assembly"></a><a name="MT0023"/>Nom de l’Application de MT0023 ' * .exe ' est en conflit avec un autre assembly de l’utilisateur.

Nom de l’assembly exécutable et le nom de l’application ne peut pas correspondre au nom de n’importe quelle dll dans l’application. Modifiez le nom de votre fichier exécutable.

### <a name="a-namemt0024mt0024-could-not-find-required-file-"></a><a name="MT0024"/>MT0024 Impossible de trouver le fichier requis ' *'.



### <a name="a-namemt0025mt0025-no-sdk-version-was-provided-please-add---sdkxy-to-specify-which-ios-sdk-should-be-used-to-build-your-application"></a><a name="MT0025"/>Version du Kit de développement logiciel MT0025 non a été fournie. Veuillez ajouter `--sdk=X.Y` pour spécifier quels iOS SDK doit être utilisé pour générer votre application.



### <a name="a-namemt0026mt0026-could-not-parse-the-command-line-argument--"></a><a name="MT0026"/>Impossible de MT0026 pas analyser l’argument de ligne de commande ' *': *



### <a name="a-namemt0027mt0027-the-options--and--are-not-compatible"></a><a name="MT0027"/>MT0027 Les options'\*'et'\*' ne sont pas compatibles.



### <a name="a-namemt0028mt0028-cannot-enable-pie--pie-when-targeting-ios-41-or-earlier-please-disable-pie--piefalse-or-set-the-deployment-target-to-at-least-ios-42"></a><a name="MT0028"/>MT0028 ne peut pas activer le graphique à secteurs (-secteur) lors du ciblage iOS 4.1 ou une version antérieure. Désactivez le graphique à secteurs (-graphique à secteurs : false) ou définir la cible de déploiement au moins iOS 4.2



### <a name="a-namemt0029mt0029-repl---enable-repl-is-only-supported-in-the-simulator---sim"></a><a name="MT0029"/>MT0029 : REPL (--enable-repl) est uniquement prise en charge dans le simulateur (--sim).

REPL n’est possible que si vous générez pour le simulateur. Cela signifie que si vous passez `--enable-repl` à mtouch, vous devez également passer `--sim`.

### <a name="a-namemt0030mt0030-the-executable-name--and-the-app-name--are-different-this-may-prevent-crash-logs-from-getting-symbolicated-properly"></a><a name="MT0030"/>MT0030 : Le nom du fichier exécutable (\*) et le nom de l’application (\*) sont différents, cela peut empêcher la mise en route symbolicated correctement de journaux.

Lorsque Xcode symbolicates (traduit les adresses de mémoire pour les noms de fonction et le fichier/numéros de ligne) le processus peut échouer si l’exécutable et les applications ont des noms différents (sans l’extension).

Pour corriger cette modifiez Nom de l’Application dans les options de la Build/iOS Application du projet, ou la modification de nom de l’Assembly dans les options de sortie de la génération du projet.

### <a name="a-namemt0031mt0031-the-command-line-arguments---enable-background-fetch-and---launch-for-background-fetch-require---launchsim-too"></a><a name="MT0031"/>MT0031 : Les arguments de ligne de commande '--extraction activer-en arrière-plan ' et '--lancement pour extraction en arrière-plan ' nécessite '--launchsim' trop.

### <a name="a-namemt0032mt0032-the-option---debugtrack-is-ignored-unless---debug-is-also-specified"></a><a name="MT0032"/>MT0032 : L’option '--debugtrack' est ignorée sauf si '--debug' est également spécifié.

### <a name="a-namemt0033mt0033--a-xamarinios-project-must-reference-either-monotouchdll-or-xamariniosdll"></a><a name="MT0033"/>MT0033 A Xamarin.iOS projet doit faire référence à monotouch.dll ou Xamarin.iOS.dll

### <a name="a-namemt0034mt0034--cannot-include-both-monotouchdll-and-xamariniosdll-in-the-same-xamarinios-project----is-referenced-explicitly-while--is-referenced-by-"></a><a name="MT0034"/>MT0034 ne peut pas inclure 'monotouch.dll' et 'Xamarin.iOS.dll' dans le même projet Xamarin.iOS - '\*' est référencé explicitement, alors que '\*' est référencé par ' *'.

<!-- MT0035 unused -->

### <a name="a-namemt0036mt0036--cannot-launch-a--simulator-for-a--app-please-enable-the-correct-architectures-in-your-projects-ios-build-options-advanced-page"></a><a name="MT0036"/>MT0036 Impossible de lancer un * simulateur pour un * application. Activez l’architecture(s) correct dans iOS de votre projet Build options (page Avancé).

### <a name="a-namemt0037mt0037--monotouchdll-is-not-64-bit-compatible-either-reference-xamariniosdll-or-do-not-build-for-a-64-bit-architecture-arm64-andor-x8664"></a><a name="MT0037"/>MT0037 monotouch.dll n’est pas 64 bits. Référence Xamarin.iOS.dll, ou de ne pas générer d’une architecture 64 bits (ARM64 et/ou x86_64).

### <a name="a-namemt0038mt0038--the-old-registrars---registraroldstaticolddynamic-are-not-supported-when-referencing-xamariniosdll"></a><a name="MT0038"/>MT0038 L’ancien bureaux d’enregistrement (--bureau d’enregistrement : oldstatic | olddynamic) ne sont pas pris en charge lors du référencement Xamarin.iOS.dll.

### <a name="a-namemt0039mt0039--applications-targeting-armv6-cannot-reference-xamariniosdll"></a><a name="MT0039"/>Applications MT0039 ciblant ARMv6 ne peut pas faire référence à Xamarin.iOS.dll.

### <a name="a-namemt0040mt0040--could-not-find-the-assembly--referenced-by-"></a><a name="MT0040"/>MT0040 Impossible de trouver l’assembly '\*', référencé par'\*'.

### <a name="a-namemt0041mt0041--cannot-reference-both-monotouchdll-and-xamariniosdll"></a><a name="MT0041"/>MT0041 ne peut pas faire référence à la fois 'monotouch.dll' et 'Xamarin.iOS.dll'.

### <a name="a-namemt0042mt0042--no-reference-to-either-monotouchdll-or-xamariniosdll-was-found-a-reference-to-monotouchdll-will-be-added"></a><a name="MT0042"/>Référence MT0042 non monotouch.dll ou Xamarin.iOS.dll a été trouvé. Une référence à monotouch.dll sera ajoutée.

### <a name="a-namemt0043mt0043-the-boehm-garbage-collector-is-currently-not-supported-when-referencing-xamariniosdll-the-sgen-garbage-collector-has-been-selected-instead"></a><a name="MT0043"/>MT0043 : Le garbage collector de Boehm est actuellement pas pris en charge lors du référencement de 'Xamarin.iOS.dll'. Le garbage collector de SGen a été sélectionné à la place.

Uniquement le SGen garbage collector est pris en charge avec les projets unifié. N’Assurez-vous aucun indicateur mtouch supplémentaires en spécifiant Boehm en tant que le garbage collector.

### <a name="a-namemt0044mt0044---listsim-is-only-supported-with-xcode-60-or-later"></a><a name="MT0044"/>MT0044 :--listsim est uniquement pris en charge avec Xcode 6.0 ou version ultérieure.

Installez une version plus récente de Xcode.

### <a name="a-namemt0045mt0045---extension-is-only-supported-when-using-the-ios-80-or-later-sdk"></a><a name="MT0045"/>MT0045 :--extension est uniquement pris en charge à l’aide de l’iOS SDK 8.0 (ou version ultérieure).

<!-- MT0046 is not reported anymore -->

### <a name="a-namemt0047mt0047-the-minimum-deployment-target-for-unified-applications-is-511-the-current-deployment-target-is--please-select-a-newer-deployment-target-in-your-projects-ios-application-options"></a><a name="MT0047"/>MT0047 : La cible de déploiement minimum pour les applications unifié est 5.1.1, la cible de déploiement actuel est ' *'. Sélectionnez une cible de déploiement plus récente dans iOS de de votre projet options de l’Application.

<!-- MT0048 is not reported anymore -->

### <a name="a-namemt0049mt0049-framework-is-supported-only-if-deployment-target-is-80-or-later--features-might-not-work-correctly"></a><a name="MT0049"/>MT0049 : *.framework est pris en charge uniquement si la cible de déploiement est 8.0 ou version ultérieure. * fonctionnalités ne fonctionnent pas correctement.

Le framework spécifié n’est pas pris en charge dans la cible de déploiement fait référence à la version d’iOS. Mettre à jour la cible de déploiement vers une nouvelle version d’iOS, ou supprimez l’utilisation du framework spécifié à partir de l’application.

<!-- MT0050 is not reported anymore -->

### <a name="a-namemt0051mt0051-xamarinios--requires-xcode-50-or-later-the-current-xcode-version-found-in--is-"></a><a name="MT0051"/>MT0051 : Xamarin.iOS * nécessite Xcode 5.0 ou version ultérieure. La version actuelle de Xcode (trouvé dans *) est *.

Installez un Xcode plus récente.

### <a name="a-namemt0052mt0052-no-command-specified"></a><a name="MT0052"/>MT0052 : Aucune commande n’est spécifiée.

Aucune action n’a été spécifiée pour mtouch.

<!-- 0053 is used by mmp -->

### <a name="a-namemt0054mt0054-unable-to-canonicalize-the-path--"></a><a name="MT0054"/>MT0054 : Impossible de rendre canonique le chemin d’accès ' *': *

Il s’agit d’une erreur interne. Si vous voyez cette erreur, Veuillez entrer un bogue [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt0055mt0055-the-xcode-path--does-not-exist"></a><a name="MT0055"/>MT0055 : Le chemin de Xcode ' *' n’existe pas.

Le chemin d’accès Xcode passé à l’aide de `--sdkroot` n’existe pas. Veuillez spécifier un chemin d’accès valide.

### <a name="a-namemt0056mt0056-cannot-find-xcode-in-the-default-location-applicationsxcodeapp-please-install-xcode-or-pass-a-custom-path-using---sdkroot-path"></a><a name="MT0056"/>MT0056 : Impossible de trouver l’emplacement par défaut Xcode (/ Applications/Xcode.app). Installez Xcode ou passer un chemin d’accès personnalisé à l’aide de--sdkroot <path>.
### <a name="a-namemt0057mt0057-cannot-determine-the-path-to-xcodeapp-from-the-sdk-root--please-specify-the-full-path-to-the-xcodeapp-bundle"></a><a name="MT0057"/>MT0057 : Impossible de déterminer le chemin d’accès à Xcode.app à partir de la racine du Kit de développement logiciel « * ». Spécifiez le chemin d’accès complet à l’offre groupée Xcode.app.

Le chemin d’accès passés à l’aide `--sdkroot` ne spécifie pas une application Xcode valide. Veuillez spécifier un chemin d’accès à une application de Xcode.

### <a name="a-namemt0058mt0058-the-xcodeapp--is-invalid-the-file--does-not-exist"></a><a name="MT0058"/>MT0058 : Le Xcode.app '\*' n’est pas valide (le fichier '\*' n’existe pas).

Le chemin d’accès passés à l’aide `--sdkroot` ne spécifie pas une application Xcode valide. Veuillez spécifier un chemin d’accès à une application de Xcode.

### <a name="a-namemt0059mt0059-could-not-find-the-currently-selected-xcode-on-the-system-"></a><a name="MT0059"/>MT0059 : N’a pas pu trouver le Xcode actuellement sélectionnée sur le système : *

### <a name="a-namemt0060mt0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned--but-that-directory-does-not-exist"></a><a name="MT0060"/>MT0060 : Le Xcode actuellement sélectionnée est introuvable sur le système. 'xcode sélection--print-path' retournée ' *', mais ce répertoire n’existe pas.

### <a name="a-namemt0061mt0061-no-xcodeapp-specified-using---sdkroot-using-the-system-xcode-as-reported-by-xcode-select---print-path-"></a><a name="MT0061"/>MT0061 : Aucun Xcode.app spécifié (à l’aide de--sdkroot), à l’aide du système Xcode comme indiqué par 'xcode select--print-path' : *

Il s’agit d’un avertissement d’information, expliquant qui Xcode sera utilisé, car aucun n’a été spécifié.

### <a name="a-namemt0062mt0062-no-xcodeapp-specified-using---sdkroot-or-xcode-select---print-path-using-the-default-xcode-instead-"></a><a name="MT0062"/>MT0062 : Aucun Xcode.app spécifié (à l’aide de--sdkroot ou 'xcode-sélectionnez--print-path'), à l’aide de la valeur par défaut Xcode à la place : *

Il s’agit d’un avertissement d’information, expliquant qui Xcode sera utilisé, car aucun n’a été spécifié.

### <a name="a-namemt0063mt0063-cannot-find-the-executable-in-the-extension--no-cfbundleexecutable-entry-in-its-infoplist"></a><a name="MT0063"/>MT0063 : Impossible de trouver le fichier exécutable dans l’extension * (aucune entrée CFBundleExecutable dans son fichier Info.plist.)

Chaque fichier Info.plist doit avoir un fichier exécutable (à l’aide de l’entrée CFBundleExecutable), toutefois, une entrée doit être générée automatiquement lors de la build.

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec un cas de test.

### <a name="a-namemt0064mt0064-xamarinios-only-supports-embedded-frameworks-with-unified-projects"></a><a name="MT0064"/>MT0064 : Xamarin.iOS prend uniquement en charge infrastructures incorporées avec les projets unifié.

Xamarin.iOS prend uniquement en charge des infrastructures incorporés lorsque vous utilisez l’API unifiée ; Mettez à jour votre projet pour utiliser l’API unifiée.

### <a name="a-namemt0065mt0065-xamarinios-only-supports-embedded-frameworks-when-deployment-target-is-at-least-80-current-deployment-target--embedded-frameworks-"></a><a name="MT0065"/>MT0065 : Xamarin.iOS prend uniquement en charge des infrastructures incorporés lors de la cible de déploiement est au moins 8.0 (cible de déploiement actuelle : * incorporé infrastructures : *)

Xamarin.iOS prend uniquement en charge les infrastructures incorporés lorsque la cible de déploiement est au moins 8.0 (car les versions antérieures d’iOS ne prend pas en charge les infrastructures incorporés).

Mettez à jour la cible de déploiement dans le fichier Info.plist du projet à 8.0 ou version ultérieure.

### <a name="a-namemt0066mt0066-invalid-build-registrar-assembly-"></a><a name="MT0066"/>MT0066 : Assembly du bureau d’enregistrement de build non valide : *

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec un cas de test.

### <a name="a-namemt0067mt0067-invalid-registrar-"></a><a name="MT0067"/>MT0067 : Bureau d’enregistrement non valide : *

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec un cas de test.

### <a name="a-namemt0068mt0068-invalid-value-for-target-framework-"></a><a name="MT0068"/>MT0068 : La valeur non valide pour le framework cible : *.

Une infrastructure cible non valide a été passée à l’aide de l’argument de l’infrastructure cible. Veuillez spécifier une infrastructure cible valide.

<!--### <a name="MT0069"/>MT0069: currently unused -->

### <a name="a-namemt0070mt0070-invalid-target-framework--valid-target-frameworks-are-"></a><a name="MT0070"/>MT0070 : Framework cible non valide : *. Infrastructure cible valides est : *.

Une infrastructure cible non valide a été passée à l’aide de l’argument de l’infrastructure cible. Veuillez spécifier une infrastructure cible valide.

### <a name="a-namemt0071mt0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinios-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a><a name="MT0071"/>MT0071 : Plateforme d’inconnu : *. Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez archiver un rapport de bogue à http://bugzilla.xamarin.com avec un cas de test.

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec un cas de test.

### <a name="a-namemt0072mt0072-extensions-are-not-supported-for-the-platform-"></a><a name="MT0072"/>MT0072 : Extensions ne sont pas pris en charge pour la plateforme ' *'.

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec un cas de test.

### <a name="a-namemt0073mt0073-xamarinios--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a><a name="MT0073"/>MT0073 : Xamarin.iOS * ne prend pas en charge une cible de déploiement de * (le minimum est *). Sélectionnez une cible de déploiement plus récente dans le fichier Info.plist de votre projet.

La cible de déploiement minimum est celle spécifiée dans le message d’erreur ; Sélectionnez une cible de déploiement plus récente dans le fichier Info.plist du projet.

Si la cible de déploiement de mise à jour n’est pas possible, utilisez une version antérieure de Xamarin.iOS.

### <a name="a-namemt0074mt0074-xamarinios--does-not-support-a-minimum-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinios"></a><a name="MT0074"/>MT0074 : Xamarin.iOS * ne prend pas en charge une cible de déploiement minimum de * (le maximum est *). Sélectionnez une cible de déploiement plus ancienne dans le fichier Info.plist de votre projet ou mettre à niveau vers une version plus récente de Xamarin.iOS.

Xamarin.iOS ne prend pas en charge la définition de la cible de déploiement minimum vers une version supérieure à la version de pour que cette version particulière de Xamarin.iOS a été générée.

Sélectionnez une cible de déploiement minimum plus ancienne dans le fichier Info.plist du projet, ou mettre à niveau vers une version plus récente de Xamarin.iOS.

### <a name="a-namemt0075mt0075-invalid-architecture--for--projects-valid-architectures-are-"></a><a name="MT0075"/>MT0075 : Architecture non valide ' *' pour * projets. Architectures valides sont : *

Une architecture non valide a été spécifiée. Vérifiez que l’architecture est valide.

### <a name="a-namemt0076mt0075-no-architecture-specified-using-the---abi-argument-an-architecture-is-required-for--projects"></a><a name="MT0076"/>MT0075 : Aucune architecture spécifiée (à l’aide de l’argument--abi). Une architecture est requise pour * projets.

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec un cas de test.

### <a name="a-namemt0077mt0076-watchos-projects-must-be-extensions"></a><a name="MT0077"/>MT0076 : WatchOS projets doivent être des extensions.

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec un cas de test.

### <a name="a-namemt0078mt0077-incremental-builds-are-enabled-with-a-deployment-target--80-currently--this-is-not-supported-the-resulting-application-will-not-launch-on-ios-9-so-the-deployment-target-will-be-set-to-80"></a><a name="MT0078"/>MT0077 : Les générations incrémentielles sont activées avec une cible de déploiement < 8.0 (actuellement *). Cela n’est pas pris en charge (l’application qui en résulte n’est pas lancée sur iOS 9), afin de la cible de déploiement a la valeur 8.0.

Il s’agit d’un avertissement informant que la cible de déploiement a été définie à 8.0 pour cette build afin que les générations incrémentielles fonctionnent correctement.

Générations incrémentielles sont uniquement pris en charge la cible de déploiement est au moins 8.0 (étant donné que l’application qui en résulte n’est pas lancée sur iOS 9 dans le cas contraire).

### <a name="a-namemt0079mt0078-the-recommended-xcode-version-for-xamarinios--is-xcode--or-later-the-current-xcode-version-found-in--is-"></a><a name="MT0079"/>MT0078 : La version de Xcode recommandée pour Xamarin.iOS * est Xcode * ou version ultérieure. La version actuelle de Xcode (trouvé dans *) est *.

Il s’agit d’un avertissement informant que la version actuelle de Xcode n’est pas la version de Xcode recommandée pour cette version de Xamarin.iOS.

Mettez à niveau Xcode pour garantir un comportement optimal.

### <a name="a-namemt0080mt0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a><a name="MT0080"/>MT0080 : La désactivation de NewRefCount,--new-refcount:false, est déconseillée.

Il s’agit d’un avertissement indiquant que la demande de désactivation de la nouvelle refcount (--new - refcount:false) a été ignoré.

La nouvelle fonctionnalité de compteur refcount est désormais obligatoire pour tous les projets, et il n’est donc pas possible de désactiver plus.

### <a name="a-namemt0081mt0081-the-command-line-argument---download-crash-report-also-requires---download-crash-report-to"></a><a name="MT0081"/>MT0081 : L’argument de ligne de commande--rapport d’incidents téléchargement nécessite également--téléchargement-crash-rapport-to.
### <a name="a-namemt0082mt0082-repl---enable-repl-is-only-supported-when-linking-is-not-used---nolink"></a><a name="MT0082"/>MT0082 : REPL (--enable-repl) est uniquement pris en charge lors de la liaison n’est pas utilisée (--nolink).
### <a name="a-namemt0083mt0083-asm-only-bitcode-is-not-supported-on-watchos-use-either---bitcodemarker-or---bitcodefull"></a><a name="MT0083"/>MT0083 : Bitcode Asm uniquement n’est pas pris en charge sur watchOS. Utilisez soit bitcode-- : marqueur ou--bitcode : complet.
### <a name="a-namemt0084mt0084-bitcode-is-not-supported-in-the-simulator-do-not-pass---bitcode-when-building-for-the-simulator"></a><a name="MT0084"/>MT0084 : Bitcode n’est pas pris en charge dans le simulateur. Ne passez pas--bitcode lors de la génération pour le simulateur.
### <a name="a-namemt0085mt0085-no-reference-to--was-found-it-will-be-added-automatically"></a><a name="MT0085"/>MT0085 : Aucune référence à ' *' a été trouvé. Il est ajouté automatiquement.
### <a name="a-namemt0086mt0086-a-target-framework---target-framework-must-be-specified-when-building-for-tvos-or-watchos"></a><a name="MT0086"/>MT0086 : Une infrastructure cible (--l’infrastructure cible) doit être spécifié lors de la génération de TVOS ou WatchOS.

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec un cas de test.

### <a name="a-namemt0087mt0087-incremental-builds---fastdev-is-not-supported-with-the-boehm-gc-incremental-builds-will-be-disabled"></a><a name="MT0087"/>MT0087 : Les générations incrémentielles (--fastdev) n’est pas pris en charge avec le garbage collector Boehm. Générations incrémentielles va être désactivées.

### <a name="a-namemt0088mt0088-the-gc-must-be-in-cooperative-mode-for-watchos-apps-please-remove-the---coopfalse-argument-to-mtouch"></a><a name="MT0088"/>MT0088 : Le garbage collector doit être en mode coopératif pour les applications watchOS. Supprimez l’argument mtouch coop : false.

### <a name="a-namemt0089mt0089-the-option--cannot-take-the-value--when-cooperative-mode-is-enabled-for-the-gc"></a><a name="MT0089"/>MT0089 : L’option '\*'ne peut pas de prendre la valeur'\*' lorsque le mode coopératif est activé pour le catalogue global.

### <a name="a-namemt0091mt0091-this-version-of-xamarinios-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-set-the-managed-linker-behaviour-to-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a><a name="MT0091"/>MT0091 : Cette version de Xamarin.iOS requiert la * Kit de développement logiciel (fourni avec Xcode *). Soit vous devez mettre à niveau Xcode pour obtenir les fichiers d’en-tête requis ou définissez le comportement de l’éditeur de liens gérés avec le lien Framework kits de développement logiciel uniquement (pour tenter d’éviter les nouvelles API).

Xamarin.iOS requiert les fichiers d’en-tête, à partir de la version du Kit de développement logiciel spécifié dans le message d’erreur pour générer votre application. La méthode recommandée pour corriger cette erreur consiste à mettre à niveau Xcode pour obtenir le Kit de développement logiciel requis, cela inclut tous les fichiers d’en-tête requis. Si vous disposez de plusieurs versions de Xcode installé, ou que vous souhaitez utiliser un Xcode dans un emplacement non définis par défaut, veillez à définir l’emplacement de Xcode correct dans les préférences de l’IDE de.

Un risque potentiel, autre solution consiste à activer l’éditeur de liens managé. Cela supprimera inutilisé API, dans la plupart des cas, la nouvelle API où les fichiers d’en-tête sont manquants (ou incomplets). Cela ne fonctionne pas si votre projet utilise des API qui a été introduit dans un kit de développement logiciel plus récent que celui votre Xcode fournit toutefois.

Une solution de dernier-paille serait d’utiliser une version antérieure de Xamarin.iOS, une qui prend en charge le Kit de développement logiciel votre projet requiert.

<!-- MT0092 used by mlaunch -->

### <a name="a-namemt0093mt0093-could-not-find-mlaunch"></a><a name="MT0093"/>MT0093 : N’a pas pu trouver 'mlaunch'.

<!-- MT0094 is not reported anymore -->

### <a name="a-namemt0095mt0095-aot-files-could-not-be-copied-to-the-destination-directory-dest-error"></a><a name="MT0095"/>MT0095 : Impossible de copier les fichiers AOA vers le répertoire de destination {et} : {error}

### <a name="a-namemt0096mt0096-no-reference-to-xamariniosdll-was-found"></a><a name="MT0096"/>MT0096 : Aucune référence à Xamarin.iOS.dll a été trouvé.

<!-- MT0097: used by mmp -->
<!-- MT0098: used by mmp -->

### <a name="a-namemt0099mt0099-internal-error--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a><a name="MT0099"/>MT0099 : Erreur interne *. Veuillez soumettre un rapport de bogue à un cas de test (http://bugzilla.xamarin.com).

Ce message d’erreur est signalé en cas d’échec d’une vérification de cohérence interne dans Xamarin.iOS.

Cela indique un bogue dans Xamarin.iOS ; Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec un cas de test.

### <a name="a-namemt0100mt0100-invalid-assembly-build-target--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a><a name="MT0100"/>MT0100 : Cible de génération assembly non valide : ' *'. Veuillez soumettre un rapport de bogue à un cas de test (http://bugzilla.xamarin.com).

Ce message d’erreur est signalé en cas d’échec d’une vérification de cohérence interne dans Xamarin.iOS.

Il s’agit toujours d’un bogue dans Xamarin.iOS ; Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec un cas de test.

### <a name="a-namemt0101mt0101-the-assembly--is-specified-multiple-times-in---assembly-build-target-arguments"></a><a name="MT0101"/>MT0101 : L’assembly ' *' est spécifié plusieurs fois dans--les arguments de la cible de génération d’assembly.

L’assembly mentionné dans le message d’erreur est spécifié plusieurs fois dans--les arguments de la cible de génération d’assembly. Assurez-vous que chaque assembly est mentionné uniquement une seule fois.

### <a name="a-namemt0102mt0102-the-assemblies--and--have-the-same-target-name--but-different-targets--and-"></a><a name="MT0102"/>MT0102 : Les assemblys\*'et'\*' portent le même nom cible ('\*»), mais différentes cibles ('\*' et ' *').

Les assemblys mentionnés dans le message d’erreur ont des cibles de génération en conflit.

Exemple :

    --assembly-build-target:Assembly1.dll=framework=MyBinary --assembly-build-target:Assembly2.dll=dynamiclibrary=MyBinary

Cet exemple tente de créer une bibliothèque dynamique et une infrastructure avec la même marque (`MyBinary`).

### <a name="a-namemt0103mt0103-the-static-object--contains-more-than-one-assembly--but-each-static-object-must-correspond-with-exactly-one-assembly"></a><a name="MT0103"/>MT0103 : L’objet statique '\*' contient plus d’un assembly ('\*'), mais chaque objet statique doit correspondre à un seul assembly.

Les assemblys mentionnés dans le message d’erreur sont compilées à un seul objet statique. Cela n’est pas autorisé, tous les assemblys doivent être compilées à un autre objet statique.

Exemple :

    --assembly-build-target:Assembly1.dll=staticobject=MyBinary --assembly-build-target:Assembly2.dll=staticobject=MyBinary

Cet exemple tente de générer un objet statique (`MyBinary`) composée de deux assemblys (`Assembly1.dll` et `Assembly2.dll`), ce qui n’est pas autorisé.

### <a name="a-namemt0105mt0105-no-assembly-build-target-was-specified-for-"></a><a name="MT0105"/>MT0105 : Aucune cible de génération d’assembly a été spécifié pour ' *'.

Lorsque la spécification de l’assembly générer à l’aide de la cible `--assembly-build-target`, tous les assemblys dans l’application doivent avoir une cible de génération affectée.

Cette erreur est signalée lorsque l’assembly mentionné dans le message d’erreur n’a pas un assembly cible attribué de génération.

Consultez la documentation `--assembly-build-target` pour plus d’informations.

### <a name="a-namemt0106mt0106-the-assembly-build-target-name--is-invalid-the-character--is-not-allowed"></a><a name="MT0106"/>MT0106 : Nom de la cible de build de l’assembly '\*' n’est pas valide : le caractère '\*' n’est pas autorisée.

Le nom de cible de build assembly doit être un nom de fichier valide.

Par exemple, ces valeurs seront déclenche cette erreur :

    --assembly-build-target:Assembly1.dll=staticobject=my/path.o

Étant donné que `my/path.o` n’est pas un nom de fichier valide en raison du caractère de séparation de répertoire.

### <a name="a-namemt0107mt0107-the-assemblies--have-different-custom-llvm-optimizations--which-is-not-allowed-when-they-are-all-compiled-to-a-single-binary"></a><a name="MT0107"/>MT0107 : Les assemblys\*' ont différentes optimisations LLVM personnalisées (\*), ce qui n’est pas autorisé quand elles sont compilées en un seul binaire.

### <a name="a-namemt0108mt0108-the-assembly-build-target--did-not-match-any-assemblies"></a><a name="MT0108"/>MT0108 : Cible de génération de l’assembly ' *' ne correspond pas à tous les assemblys.

### <a name="a-namemt0109mt0109-the-assembly-0-was-loaded-from-a-different-path-than-the-provided-path-provided-path-1-actual-path-2"></a><a name="MT0109"/>MT0109 : L’assembly '{0}' a été chargé à partir d’un chemin différent du chemin d’accès fourni (fourni le chemin d’accès : {1}, chemin d’accès réel : {{2}).

Il s’agit d’un avertissement indiquant qu’un assembly référencé par l’application a été chargé à partir d’un emplacement autre que celle demandée.

Cela peut signifier que l’application fait référence à plusieurs assemblys portant le même nom, mais à partir de différents emplacements, ce qui peuvent provoquer des résultats inattendus (uniquement le premier assembly sera utilisé).

### <a name="a-namemt0110mt0110-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-third-party-binding-libraries-and-that-compiles-to-bitcode"></a><a name="MT0110"/>MT0110 : Les générations incrémentielles ont été désactivées, car cette version de Xamarin.iOS ne prend pas en charge les générations incrémentielles dans les projets qui incluent des bibliothèques de liaison de tiers et qui est compilé en bitcode.

Générations incrémentielles ont été désactivées, car cette version de Xamarin.iOS ne prend pas en charge les générations incrémentielles dans les projets qui incluent des bibliothèques de liaison de tiers et qui est compilé en bitcode (projets tvOS et watchOS).

Aucune action n’est requise de votre part, ce message est purement informatif.

Pour plus d’informations, consultez bogue[51710](https://bugzilla.xamarin.com/show_bug.cgi?id=51710).

Cet avertissement n’est pas plus signalé.

### <a name="a-namemt0111mt0111-bitcode-has-been-enabled-because-this-version-of-xamarinios-does-not-support-building-watchos-projects-using-llvm-without-enabling-bitcode"></a><a name="MT0111"/>MT0111 : Bitcode a été activé, car cette version de Xamarin.iOS ne prend pas en charge watchOS de génération des projets à l’aide de LLVM sans activer bitcode.

Bitcode a été activée automatiquement, car cette version de Xamarin.iOS ne prend pas en charge watchOS de génération des projets à l’aide de LLVM sans activer bitcode.

Aucune action n’est requise de votre part, ce message est purement informatif.

Pour plus d’informations, consultez bogue[51634](https://bugzilla.xamarin.com/show_bug.cgi?id=51634).

### <a name="a-namemt0112mt0112-native-code-sharing-has-been-disabled-because-"></a><a name="MT0112"/>MT0112 : Partage de code natif a été désactivée car *

Il existe plusieurs raisons partage de code peut être désactivé :

* Étant donné que la cible de déploiement de l’application conteneur est antérieure à iOS 8.0 (elle a *)).

Partage de code natif nécessite iOS 8.0, car le partage de code natif est implémentée à l’aide d’infrastructures de l’utilisateur, qui a été introduite avec iOS 8.0.

* Étant donné que l’application de conteneur inclut les assemblys I18N (*).

Partage de code natif est actuellement pas pris en charge si l’application de conteneur inclut les assemblys I18N.

* Étant donné que l’application conteneur contient des définitions xml personnalisé pour l’éditeur de liens managé (*).

Partage de code natif requiert n’est pas prise en charge pour les projets qui utilisent des définitions xml personnalisé pour l’éditeur de liens managé.


### <a name="a-namemt0113mt0113-native-code-sharing-has-been-disabled-for-the-extension--because-"></a><a name="MT0113"/>MT0113 : Partage de code natif a été désactivé pour l’extension ' *', car *.

* Étant donné que les options bitcode diffèrent entre l’application conteneur (\*) et l’extension (\*).

  Partage de code natif requiert que les options bitcode correspondent entre les projets qui partagent du code.

* Étant donné que la--cible de génération d’assembly options diffèrent entre l’application conteneur (\*) et l’extension (\*).

  Partage de code natif exige que l’option--cible de génération d’assembly options sont identiques entre les projets qui partagent du code.

  Cette condition peut se produire si les générations incrémentielles sont pas soit activé ou désactivé dans tous les projets.

* Étant donné que les assemblys I18N diffèrent entre l’application conteneur (\*) et l’extension (\*).

  Partage de code natif n’est pas actuellement pris en charge pour les extensions qui incluent les assemblys I18N.

* Étant donné que les arguments du compilateur AOA diffèrent entre l’application conteneur (\*) et l’extension (\*).

  Partage de code natif requiert que les arguments du compilateur AOA ne diffèrent pas entre les projets qui partagent du code.

* Étant donné que les autres arguments au compilateur AOA diffèrent entre l’application conteneur (\*) et l’extension (\*).

  Partage de code natif requiert que les arguments du compilateur AOA ne diffèrent pas entre les projets qui partagent du code.

  Cette condition se produit si 'Effectuer toutes les opérations de virgule flottante de 32 bits en tant que valeur float 64 bits' diffère entre les projets.

* Étant donné que LLVM n’est pas activé ou désactivé dans les deux l’application de conteneur (\*) et l’extension (\*).

  Partage de code natif requiert que LLVM est activé ou désactivé pour tous les projets qui partagent du code.

* Étant donné que les paramètres de l’éditeur de liens gérés sont différents entre l’application conteneur (\*) et l’extension (\*).

  Partage de code natif requiert que les paramètres de l’éditeur de liens gérés sont identiques pour tous les projets qui partagent du code.

* Étant donné que les assemblys a été ignorées pour l’éditeur de liens géré sont différents entre l’application conteneur (\*) et l’extension (\*).

  Partage de code natif requiert que les paramètres de l’éditeur de liens gérés sont identiques pour tous les projets qui partagent du code.

* Étant donné que l’extension a des définitions xml personnalisé pour l’éditeur de liens managé (*).

  Partage de code natif requiert n’est pas prise en charge pour les projets qui utilisent des définitions xml personnalisé pour l’éditeur de liens managé.

* Étant donné que l’application de conteneur ne génère pas de l’ABI * (pendant l’extension de construction pour ce ABI).

  Partage de code natif requiert que l’application conteneur généré pour toutes les architectures toute extension d’application pour les builds.

  Par exemple : cette condition se produit quand une extension génère pour ARM64 + ARMv7, mais l’application conteneur génère uniquement pour ARM64.

* Étant donné que la création de l’application conteneur pour l’ABI \*, ce qui n’est pas compatible avec ABI de l’extension (\*).

  Partage de code natif requiert que tous les projets de build pour exactement la même API.

  Par exemple : cette condition se produit quand une extension génère pour ARMv7 + llvm + thumb2, mais l’application de conteneur ne génère qu’ARMv7 + llvm.

* Étant donné que l’application conteneur fait référence à l’assembly '\*'de'\*', alors que l’extension fait référence à une version différente de ' *'.

  Partage de code natif requiert que tous les projets qui partagent du code utilisent les mêmes versions de tous les assemblys.

<!-- MT0114: used by mmp -->

### <a name="a-namemt0115mt0115-it-is-recommended-to-reference-dynamic-symbols-using-code---dynamic-symbol-modecode-when-bitcode-is-enabled"></a><a name="MT0115"/>MT0115 : Il est recommandé de référencer des symboles dynamiques à l’aide de code (--en mode dynamique symbole = code) lorsque bitcode est activé.

Les projets Xamarin.iOS souvent référence symbole natif dynamiquement, ce qui signifie que l’éditeur de liens native peut supprimer des symboles natifs pendant le processus de liaison natif, car l’éditeur de liens natif ne voit pas que ces symboles sont utilisés.

Généralement Xamarin.iOS demandera à l’éditeur de liens natif pour conserver des symboles (à l’aide de la `-u symbol` indicateur de l’éditeur de liens), mais lorsque la compilation pour bitcode de l’éditeur de liens natif n’accepte pas les `-u` indicateur.

Xamarin.iOS a implémenté une solution alternative : nous générer du code natif supplémentaire qui fait référence à ces symboles et par conséquent, l’éditeur de liens natif verront que ces symboles sont utilisés. Cela est fait automatiquement lors de la compilation à bitcode.

Si `--dynamic-symbol-mode=linker` est passé à mtouch, ce autre solution sera désactivée et Xamarin.iOS tente de se passer `-u` pour l’éditeur de liens natif. Très probablement, cela entraîne des erreurs de l’éditeur de liens natif.

La solution consiste à supprimer la `--dynamic-symbol-mode=linker` argument à partir des arguments supplémentaires mtouch dans les options de génération du projet.

<!-- 0116 - 0124: free to use -->

### <a name="a-namemt0125mt0125-the---assembly-build-target-command-line-argument-is-ignored-in-the-simulator"></a><a name="MT0125"/>MT0125 : L’assembly-cible de génération argument de ligne de commande est ignorée dans le simulateur.

Aucune action n’est requise, ce message est purement informatif.

### <a name="a-namemt0126mt0126-incremental-builds-have-been-disabled-because-incremental-builds-are-not-supported-in-the-simulator"></a><a name="MT0126"/>MT0126 : Les générations incrémentielles ont été désactivées, car les générations incrémentielles ne sont pas pris en charge dans le simulateur.

Aucune action n’est requise, ce message est purement informatif.

### <a name="a-namemt0127mt0127-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-more-than-one-third-party-binding-libraries"></a><a name="MT0127"/>MT0127 : Les générations incrémentielles ont été désactivées, car cette version de Xamarin.iOS ne prend pas en charge les générations incrémentielles dans les projets qui incluent des bibliothèques de liaison de plus d’un tiers.

Générations incrémentielles ont été désactivées automatiquement, car cette version de Xamarin.iOS ne pas toujours générer des projets avec plusieurs bibliothèques tierces liaison correctement.

Aucune action n’est requise, ce message est purement informatif.

Pour plus d’informations, consultez bogue[52727](https://bugzilla.xamarin.com/show_bug.cgi?id=52727).

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx : Messages d’erreur d’au projet

### <a name="mt10xx-installer--mtouch"></a>MT10xx : Programme d’installation / mtouch

<!--
 MT1xxx file copy / symlinks (project related)
  MT10xx installer.cs / mtouch.cs
  -->

### <a name="a-namemt1001mt1001-could-not-find-an-application-at-the-specified-directory"></a><a name="MT1001"/>MT1001 Impossible de trouver l’application dans le répertoire spécifié



### <a name="a-namemt1002mt1002-could-not-create-symlinks-files-were-copied"></a><a name="MT1002"/>Impossible de la MT1002 de créer des liens symboliques, de fichiers ont été copiés



### <a name="a-namemt1003mt1003-could-not-kill-the-application--you-may-have-to-kill-the-application-manually"></a><a name="MT1003"/>Impossible de MT1003 d’interrompre l’application ' *'. Vous devrez peut-être supprimer l’application manuellement.



### <a name="a-namemt1004mt1004-could-not-get-the-list-of-installed-applications"></a><a name="MT1004"/>Impossible de MT1004 d’obtenir la liste des applications installées.



### <a name="a-namemt1005mt1005-could-not-kill-the-application--on-the-device----you-may-have-to-kill-the-application-manually"></a><a name="MT1005"/>Impossible de MT1005 d’interrompre l’application '\*'sur l’appareil'\*' : *-il se peut que vous deviez arrêter l’application manuellement.



### <a name="a-namemt1006mt1006-could-not-install-the-application--on-the-device--"></a><a name="MT1006"/>Impossible de MT1006 pas installer l’application '\*'sur l’appareil'\*' : *.



### <a name="a-namemt1007mt1007-failed-to-launch-the-application--on-the-device---you-can-still-launch-the-application-manually-by-tapping-on-it"></a><a name="MT1007"/>MT1007 n’a pas pu lancer l’application '\*'sur l’appareil'\*' : *. Vous pouvez toujours lancer l’application manuellement en appuyant sur dessus.



### <a name="a-namemt1008mt1008-failed-to-launch-the-simulator"></a><a name="MT1008"/>MT1008 : Impossible de lancer le simulateur



Cette erreur est signalée si mtouch n’a pas pu lancer le simulateur.   Cela peut arriver parfois car il existe déjà un processus de simulateur obsolètes ou morts en cours d’exécution.

La commande suivante est émise sur la ligne de commande Unix peut être utilisée pour supprimer un processus de simulateur bloqué :

```bash
$ launchctl list|grep UIKitApplication|awk '{print $3}'|xargs launchctl remove
```

### <a name="a-namemt1009mt1009-could-not-copy-the-assembly--to--"></a><a name="MT1009"/>MT1009 peut pas copier l’assembly '\*'à'\*' : *

Il s’agit d’un problème connu dans certaines versions de Xamarin.iOS.

Si cela se produit pour vous, essayez la solution de contournement suivante :

```csharp
sudo chmod 0644 /Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/*/*.mdb
```

Toutefois, étant donné que ce problème a été résolu dans la dernière version de Xamarin.iOS, veuillez soumettre un nouveau bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec vos informations de version complètes et la sortie de journal de génération.

### <a name="a-namemt1010mt1010-could-not-load-the-assembly--"></a><a name="MT1010"/>MT1010 Impossible de charger l’assembly ' *': *



### <a name="a-namemt1011mt1011-could-not-add-missing-resource-file-"></a><a name="MT1011"/>MT1011 Impossible d’ajouter le fichier de ressources manquant : ' *'



### <a name="a-namemt1012mt1012-failed-to-list-the-apps-on-the-device--"></a><a name="MT1012"/>Échec de MT1012 pour répertorier les applications sur l’appareil ' *': *



### <a name="a-namemt1013mt1013-dependency-tracking-error-no-files-to-compare-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a><a name="MT1013"/>Dépendance de MT1013 suivi d’erreur : aucun fichier à comparer. Veuillez archiver un rapport de bogue à http://bugzilla.xamarin.com avec un cas de test.

Cela indique un bogue dans Xamarin.iOS. Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec un caes de test.

### <a name="a-namemt1014mt1014-failed-to-re-use-cached-version-of--"></a><a name="MT1014"/>MT1014 n’a pas pu réutiliser une version mise en cache de ' *': *.



### <a name="a-namemt1015mt1015--failed-to-create-the-executable--"></a><a name="MT1015"/>MT1015 n’a pas pu créer l’exécutable ' *': *

### <a name="a-namemt1015mt1015--failed-to-create-the-executable--"></a><a name="MT1015"/>MT1015 n’a pas pu créer l’exécutable ' *': *

### <a name="a-namemt1016mt1016-failed-to-create-the-notice-file-because-a-directory-already-exists-with-the-same-name"></a><a name="MT1016"/>MT1016 : Impossible de créer le fichier de notification, car il existe déjà un répertoire portant le même nom.

Supprimez le répertoire `NOTICE` à partir du projet.

### <a name="a-namemt1017mt1017-failed-to-create-the-notice-file-"></a><a name="MT1017"/>MT1017 : Impossible de créer le fichier de notification : *.

### <a name="a-namemt1018mt1018-your-application-failed-code-signing-checks-and-could-not-be-installed-on-the-device--check-your-certificates-provisioning-profiles-and-bundle-ids-probably-your-device-is-not-part-of-the-selected-provisioning-profile-error-0xe8008015"></a><a name="MT1018"/>MT1018 : Échec de la vérification de signature de code de votre application et n’a pas pu être installée sur le périphérique ' *'. Vérifier vos certificats, des profils, de configuration et les ID de regroupement. Probablement votre appareil n’est pas dans le profil de configuration sélectionné (erreur : 0xe8008015).
### <a name="a-namemt1019mt1019-your-application-has-entitlements-not-supported-by-your-current-provisioning-profile-and-could-not-be-installed-on-the-device--please-check-the-ios-device-log-for-more-detailed-information-error-0xe8008016"></a><a name="MT1019"/>MT1019 : Votre application a des droits non pris en charge par votre profil de configuration en cours et n’a pas pu être installée sur le périphérique ' *'. Consultez le journal de l’appareil iOS pour plus d’informations (erreur : 0xe8008016).

Cela peut se produire si :

* Votre application dispose de droits que ceux que le profil de configuration actuel ne prend pas en charge.
  Solutions possibles :
  - Spécifiez un autre profil de configuration qui prend en charge les droits nécessaires à votre application.
  - Supprimer les droits non pris en charge dans le profil de configuration en cours.
* L’appareil que vous tentez de le déployer sur n’est ne pas inclus dans le profil de configuration que vous utilisez.
  Solutions possibles :
  - Créer une application à partir d’un modèle dans Xcode, sélectionnez le même profil de configuration et déployer sur le même appareil. Parfois Xcode peut actualiser automatiquement les profils de configuration avec les nouveaux appareils (dans les autres cas que Xcode vous demande comment procéder).
  -Aller dans le centre de développement iOS et mettre à jour le profil de configuration avec le nouveau périphérique, puis téléchargez le profil de configuration mis à jour sur votre ordinateur.


Dans la plupart des cas que plus d’informations sur l’échec est imprimée dans le journal de l’appareil iOS, ce qui peut aider à diagnostiquer le problème.

### <a name="a-namemt1020mt1020-failed-to-launch-the-application--on-the-device--"></a><a name="MT1020"/>MT1020 : Échec du lancement de l’application '\*'sur l’appareil'\*' : *

### <a name="a-namemt1021mt1021-could-not-copy-the-file--to--2"></a><a name="MT1021"/>MT1021 : N’a pas pu copier le fichier '\*'à'\*' : {2}

Impossible de copier un fichier. Le message d’erreur à partir de l’opération de copie a plus d’informations sur l’erreur.

### <a name="a-namemt1022mt1022-could-not-copy-the-directory--to--2"></a><a name="MT1022"/>MT1022 : Impossible de copier le répertoire '\*'à'\*' : {2}

Impossible de copier un répertoire. Le message d’erreur à partir de l’opération de copie a plus d’informations sur l’erreur.

### <a name="a-namemt1023mt1023-could-not-communicate-with-the-device-to-find-the-application---"></a><a name="MT1023"/>MT1023 : Ne peut pas communiquer avec le périphérique de l’application ' *' : *

Une erreur s’est produite lors de la tentative de recherche d’une application sur l’appareil.

Choses à essayer résoudre ce problème :

* Supprimer l’application de l’appareil, puis réessayez.
* Déconnectez l’appareil et le reconnecter.
* Redémarrez l’appareil.
* Redémarrez le Mac.

### <a name="a-namemt1024mt1024-the-application-signature-could-not-be-verified-on-device--please-make-sure-that-the-provisioning-profile-is-installed-and-not-expired-error-0xe8008017"></a><a name="MT1024"/>MT1024 : La signature de l’application pas pu être vérifiée que sur l’appareil ' *'. Assurez-vous que le profil de configuration est installé et n’a pas expiré (erreur : 0xe8008017).

Le périphérique a rejeté l’installation de l’application, car la signature n’a pas pu être vérifiée.

Assurez-vous que le profil de configuration est installé et n’a pas expiré.

### <a name="a-namemt1025mt1025-could-not-list-the-crash-reports-on-the-device-"></a><a name="MT1025"/>MT1025 : Impossible de répertorier les rapports d’incidents sur le périphérique *.

Une erreur s’est produite lors de la tentative répertorier les rapports d’incidents sur l’appareil.

Choses à essayer résoudre ce problème :

* Supprimer l’application de l’appareil, puis réessayez.
* Déconnectez l’appareil et le reconnecter.
* Redémarrez l’appareil.
* Redémarrez le Mac.
* Synchronisez l’appareil avec iTunes (Cela supprimera les rapports d’incidents à partir de l’appareil).

### <a name="a-namemt1026mt1026-could-not-download-the-crash-report--from-the-device-"></a><a name="MT1026"/>MT1026 : Impossible de télécharger le rapport d’incident * à partir du périphérique *.

Une erreur s’est produite lors de la tentative de télécharger les rapports d’incidents à partir de l’appareil.

Choses à essayer résoudre ce problème :

* Supprimer l’application de l’appareil, puis réessayez.
* Déconnectez l’appareil et le reconnecter.
* Redémarrez l’appareil.
* Redémarrez le Mac.
* Synchronisez l’appareil avec iTunes (Cela supprimera les rapports d’incidents à partir de l’appareil).

### <a name="a-namemt1027mt1027-cant-use-xcode-7-to-launch-applications-on-devices-with-ios--xcode-7-only-supports-ios-6"></a><a name="MT1027"/>MT1027 : Ne peut pas utiliser Xcode 7 + pour lancer des applications sur les appareils iOS * (Xcode 7 prend uniquement en charge iOS 6 +).

Il n’est pas possible d’utiliser Xcode 7 + pour lancer des applications sur les appareils iOS version est inférieure à 6.0.

Utilisez une version antérieure de Xcode, ou appuyer sur l’application manuellement pour le lancer.

### <a name="a-namemt1028mt1028-invalid-device-specification--expected-ios-watchos-or-all"></a><a name="MT1028"/>MT1028 : Spécification du périphérique non valide : ' *'. Attendu 'e', 'watchos' ou 'all'.

La spécification de périphérique passé à l’aide de--appareil n’est pas valide. Les valeurs valides sont : 'e', 'watchos' ou 'all'.

### <a name="a-namemt1029mt1029-could-not-find-an-application-at-the-specified-directory-"></a><a name="MT1029"/>MT1029 : Impossible de trouver une application dans le répertoire spécifié : *

Le chemin d’accès de l’application passé à--launchdev n’existe pas. Veuillez spécifier un lot d’applications valide.

### <a name="a-namemt1030mt1030-launching-applications-on-device-using-a-bundle-identifier-is-deprecated-please-pass-the-full-path-to-the-bundle-to-launch"></a><a name="MT1030"/>MT1030 : Lancement d’applications sur l’appareil à l’aide d’un identificateur de lot est déconseillé. Passez le chemin d’accès complet à l’application de lancement.

Il est recommandé de passer le chemin d’accès à l’application sur un appareil au lieu de l’id d’ensemble.

### <a name="a-namemt1031mt1031-could-not-launch-the-app--on-the-device--because-the-device-is-locked-please-unlock-the-device-and-try-again"></a><a name="MT1031"/>MT1031 : N’a pas pu lancer l’application '\*'sur l’appareil'\*', car l’appareil est verrouillé. Veuillez déverrouiller l’appareil, puis réessayez.

Veuillez déverrouiller l’appareil, puis réessayez.

### <a name="a-namemt1032mt1032-this-application-executable-might-be-too-large--mb-to-execute-on-device-if-bitcode-was-enabled-you-might-want-to-disable-it-for-development-it-is-only-required-to-submit-applications-to-apple"></a><a name="MT1032"/>MT1032 : Cet exécutable de l’application peut être trop volumineux (* Mo) à exécuter sur l’appareil. Si bitcode a été activé vous souhaiterez peut-être la désactiver pour le développement, il est nécessaire uniquement pour soumettre des applications à Apple.

### <a name="a-namemt1033mt1033-could-not-uninstall-the-application--from-the-device--"></a><a name="MT1033"/>MT1033 : Impossible de désinstaller l’application '\*'à partir de l’appareil'\*' : *

<!-- 1034 used by mmp -->

### <a name="a-namemt1035mt1035-cannot-include-different-versions-of-the-framework-name"></a><a name="MT1035"/>MT1035 : Ne peut pas inclure de différentes versions du framework « {name} »

Il n’est pas possible de lier avec différentes versions de la même infrastructure.

Cela se produit généralement lorsqu’une extension fait référence à une version différente d’une infrastructure à l’application conteneur (éventuellement par l’intermédiaire d’un assembly tiers liaison).

Ce message d’erreur Il y aura plusieurs [MT1036](#MT1036) erreurs qui répertorie les chemins d’accès pour chaque autre infrastructure.

### <a name="a-namemt1036mt1036-framework-name-included-from-path-related-to-previous-error"></a><a name="MT1036"/>MT1036 : Framework « {name} » inclus à partir de : {chemin d’accès} (associé à l’erreur précédente)

Cette erreur est signalée uniquement avec [MT1036](#MT1036). Consultez [MT1036](#MT1036) pour plus d’informations.

### <a name="mt11xx-debug-service"></a>MT11xx : Déboguer le Service

<!--
  MT11xx DebugService.cs
  -->

### <a name="a-namemt1101mt1101-could-not-start-app"></a><a name="MT1101"/>Impossible de MT1101 ne démarrer pas d’application



### <a name="a-namemt1102mt1102-could-not-attach-to-the-app-to-kill-it-"></a><a name="MT1102"/>MT1102 a pas pu attacher à l’application (pour les supprimer) : *



### <a name="a-namemt1103mt1103-could-not-detach"></a><a name="MT1103"/>MT1103 Impossible de détacher



### <a name="a-namemt1104mt1104-failed-to-send-packet-"></a><a name="MT1104"/>MT1104 n’a pas pu envoyer le paquet : *



### <a name="a-namemt1105mt1105-unexpected-response-type"></a><a name="MT1105"/>Type de réponse MT1105 inattendue



### <a name="a-namemt1106mt1106-could-not-get-list-of-applications-on-the-device-request-timed-out"></a><a name="MT1106"/>Impossible de MT1106 d’obtenir la liste des applications sur l’appareil : demande a expiré.



### <a name="a-namemt1107mt1107-application-failed-to-launch-"></a><a name="MT1107"/>MT1107 : Échec de l’Application lancer : *

Vérifiez si votre appareil est verrouillé.

Si vous déployez une application d’entreprise ou à l’aide d’un profil d’approvisionnement, vous devez faire confiance au développeur (cela est expliqué <a href="http://stackoverflow.com/a/30726375/183422">ici</a>).

### <a name="a-namemt1108mt1108-could-not-find-developer-tools-for-this-xx-yy-device"></a><a name="MT1108"/>MT1108 : Impossible de trouver les outils de développement pour cet appareil XX (aa).

Quelques opérations de mtouch requièrent le <tt>DeveloperDiskImage.dmg</tt> fichier.   Ce fichier fait partie de Xcode et se trouve généralement par rapport au Kit de développement logiciel que vous utilisez pour créer, dans le <tt>Xcode.app/Contents/Developer/iPhoneOS.platform/DeviceSupport/VERSION/DeveloperDiskImage.dmg</tt>.

Cette erreur peut se produire soit parce que vous n’avez pas un DeveloperDiskImage.dmg qui correspond à l’appareil que vous avez connectée.


### <a name="a-namemt1109mt1109-application-failed-to-launch-because-the-device-is-locked-please-unlock-the-device-and-try-again"></a><a name="MT1109"/>MT1109 : Échec de l’Application à lancer, car l’appareil est verrouillé. Veuillez déverrouiller l’appareil, puis réessayez.

Vérifiez si votre appareil est verrouillé.

### <a name="a-namemt1110mt1110-application-failed-to-launch-because-of-ios-security-restrictions-please-ensure-the-developer-is-trusted"></a><a name="MT1110"/>MT1110 : Échec de l’Application lancer en raison des restrictions de sécurité iOS. Vérifiez que le développeur est approuvé.

Si vous déployez une application d’entreprise ou à l’aide d’un profil d’approvisionnement, vous devez faire confiance au développeur (cela est expliqué <a href="http://stackoverflow.com/a/30726375/183422">ici</a>).

### <a name="mt12xx-simulator"></a>MT12xx : simulateur

<!--
  MT12xx simcontroller.cs
  -->

### <a name="a-namemt1201mt1201-could-not-load-the-simulator-"></a><a name="MT1201"/>MT1201 : N’a pas pu charger le simulateur : *
### <a name="a-namemt1202mt1202-invalid-simulator-configuration-"></a><a name="MT1202"/>MT1202 : Configuration de simulateur non valide : *
### <a name="a-namemt1203mt1203-invalid-simulator-specification-"></a><a name="MT1203"/>MT1203 : Spécification du simulateur non valide : *
### <a name="a-namemt1204mt1204-invalid-simulator-specification--runtime-not-specified"></a><a name="MT1204"/>MT1204 : Spécification du simulateur non valide ' *': runtime non spécifiée.
### <a name="a-namemt1205mt1205-invalid-simulator-specification--device-type-not-specified"></a><a name="MT1205"/>MT1205 : Spécification du simulateur non valide ' *': type de périphérique non spécifié.
### <a name="a-namemt1206mt1206-could-not-find-the-simulator-runtime-"></a><a name="MT1206"/>MT1206 : N’a pas trouvé l’exécution du simulateur ' *'.
### <a name="a-namemt1207mt1207-could-not-find-the-simulator-device-type-"></a><a name="MT1207"/>MT1207 : Impossible de trouver le type de périphérique de simulateur ' *'.
### <a name="a-namemt1208mt1208-could-not-find-the-simulator-runtime-"></a><a name="MT1208"/>MT1208 : N’a pas trouvé l’exécution du simulateur ' *'.
### <a name="a-namemt1209mt1209-could-not-find-the-simulator-device-type-"></a><a name="MT1209"/>MT1209 : Impossible de trouver le type de périphérique de simulateur ' *'.
### <a name="a-namemt1210mt1210-invalid-simulator-specification--unknown-key-"></a><a name="MT1210"/>MT1210 : Spécification du simulateur non valide : \*, clé inconnue '\*'
### <a name="a-namemt1211mt1211-the-simulator-version--does-not-support-the-simulator-type-"></a><a name="MT1211"/>MT1211 : La version du simulateur '\*'ne prend pas en charge le type de simulateur'\*'
### <a name="a-namemt1212mt1212-failed-to-create-a-simulator-version-where-type---and-runtime--"></a><a name="MT1212"/>MT1212 : Impossible de créer une version du simulateur où taper = * et runtime = *.
### <a name="a-namemt1213mt1213-invalid-simulator-specification-for-xcode-4-"></a><a name="MT1213"/>MT1213 : Spécification de simulateur non valide pour Xcode 4 : *
### <a name="a-namemt1214mt1214-invalid-simulator-specification-for-xcode-5-"></a><a name="MT1214"/>MT1214 : Spécification de simulateur non valide pour Xcode 5 : *
### <a name="a-namemt1215mt1215-invalid-sdk-specified-"></a><a name="MT1215"/>MT1215 : Les SDK non valide spécifié : *
### <a name="a-namemt1216mt1216-could-not-find-the-simulator-udid-"></a><a name="MT1216"/>MT1216 : N’a pas pu trouver le simulateur UDID ' *'.
### <a name="a-namemt1217mt1217-could-not-load-the-app-bundle-at-"></a><a name="MT1217"/>MT1217 : N’a pas pu charger le lot d’applications à ' *'.
### <a name="a-namemt1218mt1218-no-bundle-identifier-found-in-the-app-at-"></a><a name="MT1218"/>MT1218 : Aucun identificateur de lot ne trouvé dans l’application à ' *'.
### <a name="a-namemt1219mt1219-could-not-find-the-simulator-for-"></a><a name="MT1219"/>MT1219 : N’a pas pu trouver le simulateur pour ' *'.
### <a name="a-namemt1220mt1220-could-not-find-the-latest-simulator-runtime-for-device-"></a><a name="MT1220"/>MT1220 : Impossible de trouver la dernière exécution du simulateur pour le périphérique ' *'.

Cela indique généralement un problème avec Xcode.

Choses à essayer résoudre ce problème :

* Utilisez le simulateur une fois dans Xcode.
* Passer d’une version de kit de développement logiciel explicite à l’aide du Kit de développement logiciel-- <version>.
* Réinstallez Xcode.

### <a name="a-namemt1221mt1221-could-not-find-the-paired-iphone-simulator-for-the-watchos-simulator-"></a><a name="MT1221"/>MT1221 : Impossible de trouver le simulateur iPhone appariée pour le simulateur WatchOS ' *'.

Lors du lancement d’une application WatchOS dans un simulateur WatchOS, il doit y avoir un paire iOS Simulator ainsi.

Regardez simulateurs peuvent être associés à iOS simulateurs à l’aide de l’interface utilisateur des appareils de Xcode (menu Fenêtre -> appareils).

### <a name="mt13xx-linkwith"></a>MT13xx : [LinkWith]

<!--
  MT13xx [LinkWith]
  -->


### <a name="a-namemt1301mt1301-native-library---was-ignored-since-it-does-not-match-the-current-build-architectures-"></a><a name="MT1301"/>La bibliothèque Native MT1301 `*` (\*) a été ignoré, car il ne correspond pas à l’architecture(s) de build en cours (\*)



### <a name="a-namemt1302mt1302-could-not-extract-the-native-library--from--please-ensure-the-native-library-was-properly-embedded-in-the-managed-assembly-if-the-assembly-was-built-using-a-binding-project-the-native-library-must-be-included-in-the-project-and-its-build-action-must-be-objcbindingnativelibrary"></a><a name="MT1302"/>Impossible de MT1302 pas extraire la bibliothèque native ' *' à partir de '+'. Vérifiez que la bibliothèque native a été correctement incorporée dans l’assembly managé (si l’assembly a été généré à l’aide d’un projet de liaison, la bibliothèque native doit être incluse dans le projet et son Action de génération doit être 'ObjcBindingNativeLibrary').

### <a name="a-namemt1303mt1303-could-not-decompress-the-native-framework--from--please-review-the-build-log-for-more-information-from-the-native-zip-command"></a><a name="MT1303"/>MT1303 : N’a pas pu décompresser le framework natif '\*'de'\*'. Passez en revue le journal de génération pour plus d’informations à partir de la commande native « zip ».

N’a pas pu décompresser le framework natif spécifié à partir de la bibliothèque de liaison.

Passez en revue le journal bulid pour plus d’informations sur cet échec de la commande native « zip ».

### <a name="a-namemt1304mt1304-the-embedded-framework--in--is-invalid-it-does-not-contain-an-infoplist"></a><a name="MT1304"/>MT1304 : Le framework incorporé ' *' dans * n’est pas valide : il ne contient pas un fichier Info.plist.

Le framework incorporé spécifié ne contient-elle pas un fichier Info.plist et n’est donc pas une infrastructure valide.

Vérifiez que le framework n’est pas valid.

### <a name="a-namemt1305mt1305-the-binding-library--contains-a-user-framework--but-embedded-user-frameworks-require-ios-80-the-current-deployment-target-is--please-set-the-deployment-target-in-the-infoplist-file-to-at-least-80"></a><a name="MT1305"/>MT1305 : La bibliothèque de liaison '\*' contient une infrastructure de l’utilisateur (\*), mais les infrastructures utilisateur incorporées nécessitent iOS 8.0 (la cible de déploiement en cours est *). Définissez la cible de déploiement dans le fichier Info.plist au moins 8.0.

La bibliothèque de liaison spécifié contient une structure incorporée, mais Xamarin.iOS prend uniquement en charge des infrastructures incorporées sur iOS 8.0 ou version ultérieure.

Définir la cible de déploiement dans le fichier Info.plist au moins 8.0 pour résoudre cette erreur (ou n’utilisez pas les infrastructures incorporés).

### <a name="mt14xx-crash-reports"></a>MT14xx : Rapports d’incidents

<!--
  MT14xx    CrashReports.cs
  -->

### <a name="a-namemt1400mt1400-could-not-open-crash-report-service-afcconnectionopen-returned-"></a><a name="MT1400"/>MT1400 : Impossible d’ouvrir le service de rapport d’incident : AFCConnectionOpen retourné *

Une erreur s’est produite lors de la tentative d’accéder aux rapports d’incidents à partir de l’appareil.

Choses à essayer résoudre ce problème :

* Supprimer l’application de l’appareil, puis réessayez.
* Déconnectez l’appareil et le reconnecter.
* Redémarrez l’appareil.
* Redémarrez le Mac.
* Synchronisez l’appareil avec iTunes (Cela supprimera les rapports d’incidents à partir de l’appareil).

### <a name="a-namemt1401mt1401-could-not-close-crash-report-service-afcconnectionclose-returned-"></a><a name="MT1401"/>MT1401 : Impossible de fermer le service de rapport d’incident : AFCConnectionClose retourné *

Une erreur s’est produite lors de la tentative d’accéder aux rapports d’incidents à partir de l’appareil.

Choses à essayer résoudre ce problème :

* Supprimer l’application de l’appareil, puis réessayez.
* Déconnectez l’appareil et le reconnecter.
* Redémarrez l’appareil.
* Redémarrez le Mac.
* Synchronisez l’appareil avec iTunes (Cela supprimera les rapports d’incidents à partir de l’appareil).

### <a name="a-namemt1402mt1402-could-not-read-file-info-for--afcfileinfoopen-returned-"></a><a name="MT1402"/>MT1402 : N’a pas pu lire les informations de fichier pour * : AFCFileInfoOpen retourné *

Une erreur s’est produite lors de la tentative d’accéder aux rapports d’incidents à partir de l’appareil.

Choses à essayer résoudre ce problème :

* Supprimer l’application de l’appareil, puis réessayez.
* Déconnectez l’appareil et le reconnecter.
* Redémarrez l’appareil.
* Redémarrez le Mac.
* Synchronisez l’appareil avec iTunes (Cela supprimera les rapports d’incidents à partir de l’appareil).

### <a name="a-namemt1403mt1403-could-not-read-crash-report-afcdirectoryopen--returned-"></a><a name="MT1403"/>MT1403 : N’a pas pu lire le rapport d’incident : AFCDirectoryOpen (*) retourné : *

Une erreur s’est produite lors de la tentative d’accéder aux rapports d’incidents à partir de l’appareil.

Choses à essayer résoudre ce problème :

* Supprimer l’application de l’appareil, puis réessayez.
* Déconnectez l’appareil et le reconnecter.
* Redémarrez l’appareil.
* Redémarrez le Mac.
* Synchronisez l’appareil avec iTunes (Cela supprimera les rapports d’incidents à partir de l’appareil).

### <a name="a-namemt1404mt1404-could-not-read-crash-report-afcfilerefopen--returned-"></a><a name="MT1404"/>MT1404 : N’a pas pu lire le rapport d’incident : AFCFileRefOpen (*) retourné : *

Une erreur s’est produite lors de la tentative d’accéder aux rapports d’incidents à partir de l’appareil.

Choses à essayer résoudre ce problème :

* Supprimer l’application de l’appareil, puis réessayez.
* Déconnectez l’appareil et le reconnecter.
* Redémarrez l’appareil.
* Redémarrez le Mac.
* Synchronisez l’appareil avec iTunes (Cela supprimera les rapports d’incidents à partir de l’appareil).

### <a name="a-namemt1405mt1405-could-not-read-crash-report-afcfilerefread--returned-"></a><a name="MT1405"/>MT1405 : N’a pas pu lire le rapport d’incident : AFCFileRefRead (*) retourné : *

Une erreur s’est produite lors de la tentative d’accéder aux rapports d’incidents à partir de l’appareil.

Choses à essayer résoudre ce problème :

* Supprimer l’application de l’appareil, puis réessayez.
* Déconnectez l’appareil et le reconnecter.
* Redémarrez l’appareil.
* Redémarrez le Mac.
* Synchronisez l’appareil avec iTunes (Cela supprimera les rapports d’incidents à partir de l’appareil).

### <a name="a-namemt1406mt1406-could-not-list-crash-reports-afcdirectoryopen--returned-"></a><a name="MT1406"/>MT1406 : Impossible de répertorier les rapports d’incidents : AFCDirectoryOpen (*) retourné : *

Une erreur s’est produite lors de la tentative d’accéder aux rapports d’incidents à partir de l’appareil.

Choses à essayer résoudre ce problème :

* Supprimer l’application de l’appareil, puis réessayez.
* Déconnectez l’appareil et le reconnecter.
* Redémarrez l’appareil.
* Redémarrez le Mac.
* Synchronisez l’appareil avec iTunes (Cela supprimera les rapports d’incidents à partir de l’appareil).

<!--- 1407 used by mmp -->

### <a name="mt16xx-macho"></a>MT16xx: MachO

<!--
  MT16xx    MachO.cs
  -->

### <a name="a-namemt1600mt1600-not-a-mach-o-dynamic-library-unknown-header-0x-"></a><a name="MT1600"/>MT1600 : Pas une O de faire correspondre la bibliothèque dynamique (en-tête inconnu « 0 x * ») : *.

Une erreur s’est produite lors du traitement de la bibliothèque dynamique en question.

Vérifiez que la bibliothèque dynamique est une bibliothèque dynamique de faire correspondre la-o valide.

Le format d’une bibliothèque peut être vérifié à l’aide de la `file` commande à partir d’un terminal :

    file -arch all -l /path/to/library.dylib

### <a name="a-namemt1601mt1601-not-a-static-library-unknown-header--"></a><a name="MT1601"/>MT1601 : Pas une bibliothèque statique (en-tête inconnu ' *') : *.

Une erreur s’est produite lors du traitement de la bibliothèque statique en question.

Vérifiez que la bibliothèque statique est une bibliothèque statique O de faire correspondre la valide.

Le format d’une bibliothèque peut être vérifié à l’aide de la `file` commande à partir d’un terminal :

    file -arch all -l /path/to/library.a

### <a name="a-namemt1602mt1602-not-a-mach-o-dynamic-library-unknown-header-0x-"></a><a name="MT1602"/>MT1602 : Pas une O de faire correspondre la bibliothèque dynamique (en-tête inconnu « 0 x * ») : *.

Une erreur s’est produite lors du traitement de la bibliothèque dynamique en question.

Vérifiez que la bibliothèque dynamique est une bibliothèque dynamique de faire correspondre la-o valide.

Le format d’une bibliothèque peut être vérifié à l’aide de la `file` commande à partir d’un terminal :

    file -arch all -l /path/to/library.dylib

### <a name="a-namemt1603mt1603-unknown-format-for-fat-entry-at-position--in-"></a><a name="MT1603"/>MT1603 : Le format inconnu pour l’entrée à la position de la fat * dans *.

Une erreur s’est produite lors du traitement de l’archive fat en question.

Assurez-vous que l’archive fat est valide.

Le format d’une archive fat peut être vérifié à l’aide de la `file` commande à partir d’un terminal :

    file -arch all -l /path/to/file

### <a name="a-namemt1604mt1604-file-of-type--is-not-a-macho-file-"></a><a name="MT1604"/>MT1604 : Type de fichier * n’est pas un fichier MachO (*).

Une erreur s’est produite lors du traitement du fichier MachO en question.

Vérifiez que le fichier est une bibliothèque dynamique de faire correspondre la-o valide.

Le format d’un fichier peut être vérifié à l’aide de la `file` commande à partir d’un terminal :

    file -arch all -l /path/to/file

## <a name="mt2xxx-linker-error-messages"></a>MT2xxx : Messages d’erreur l’éditeur de liens

<!--
 MT2xxx Linker
  MT20xx Linker (general) errors
  -->

### <a name="a-namemt2001mt2001-could-not-link-assemblies"></a><a name="MT2001"/>Impossible de MT2001 pas lier des assemblys

Cette erreur signifie que l’éditeur de liens managé a rencontré une erreur inattendue, par exemple, une exception et ne peut pas terminer ou enregistrez l’assembly en cours de traitement. Plus d’informations sur l’erreur exacte fera partie du journal de génération, par exemple

``` 
error MT2001: Could not link assemblies.
    Method: `System.Void Todo.TodoListPageCS/<<-ctor>b__1_0>d::SetStateMachine(System.Runtime.CompilerServices.IAsyncStateMachine)`
    Assembly: `QuickTodo, Version=1.0.6297.28241, Culture=neutral, PublicKeyToken=null`
Reason: Value cannot be null.
Parameter name: instruction
```

Il est important d’un rapport de bogue pour ces problèmes. Dans la plupart des cas, une solution de contournement peut être fournie avant la publication d’un correctif approprié. Les informations ci-dessus sont critiques (ainsi que d’un cas de test et/ou le binairy assembly) pour résoudre le problème.


### <a name="a-namemt2002mt2002-can-not-resolve-reference-"></a><a name="MT2002"/>MT2002 peut résoudre la référence : *



### <a name="a-namemt2003mt2003-option--will-be-ignored-since-linking-is-disabled"></a><a name="MT2003"/>Option de MT2003 ' *' sera ignoré, car la liaison est désactivée.



### <a name="a-namemt2004mt2004-extra-linker-definitions-file--could-not-be-located"></a><a name="MT2004"/>Fichier de définitions de l’éditeur de liens supplémentaires MT2004 ' *' n’a pas été trouvé.



### <a name="a-namemt2005mt2005-definitions-from--could-not-be-parsed"></a><a name="MT2005"/>MT2005 des définitions à partir de ' *' a pas pu être analysé.



### <a name="a-namemt2006mt2006-can-not-load-mscorlibdll-from--please-reinstall-xamarinios"></a><a name="MT2006"/>MT2006 : Ne peut pas charger de mscorlib.dll à partir de : *. Réinstallez Xamarin.iOS.

Cela indique généralement qu’il existe un problème avec votre installation de Xamarin.iOS. Essayez de réinstaller les Xamarin.iOS.

<!--- 2007 used by mmp -->
<!--- 2009 used by mmp -->

### <a name="a-namemt2010mt2010-unknown-httpmessagehandler--valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a><a name="MT2010"/>MT2010 : HttpMessageHandler inconnu `*`. Les valeurs valides sont HttpClientHandler (par défaut), CFNetworkHandler ou NSUrlSessionHandler

### <a name="a-namemt2011mt2011-unknown-tlsprovider---valid-values-are-default-or-appletls"></a><a name="MT2011"/>MT2011 : TlsProvider inconnu `*`.  Les valeurs valides sont appletls ou par défaut.

La valeur donnée `tls-provider=` n’est pas un fournisseur TLS (Transport Layer Security) valide.

Le `default` et `appletls` sont les seules valeurs valides et elles représentent la même option, qui consiste à fournir les SSL/TLS prend en charge à l’aide de l’API TLS Apple native.

<!--- 2012 used by mmp -->
<!--- 2013 used by mmp -->
<!--- 2014 used by mmp -->

### <a name="a-namemt2015mt2015-invalid-httpmessagehandler--for-watchos-the-only-valid-value-is-nsurlsessionhandler"></a><a name="MT2015"/>MT2015 : HttpMessageHandler non valide `*` pour watchOS. La seule valeur valide est NSUrlSessionHandler.

Il s’agit d’un avertissement se produit parce que le fichier projet spécifie un HttpMessageHandler non valide.

Les versions antérieures de nos outils Aperçu généré par défaut une valeur non valide dans le fichier projet.

Pour résoudre cet avertissement, ouvrez le fichier projet dans un éditeur de texte et supprimer tous les nœuds de HttpMessageHandler à partir du XML.

### <a name="a-namemt2016mt2016-invalid-tlsprovider-legacy-option-the-only-valid-value-appletls-will-be-used"></a><a name="MT2016"/>MT2016 : TlsProvider non valide `legacy` option. La seule valeur valide `appletls` sera utilisé.

Le `legacy` fournisseur, qui a été un SSLv3 entièrement géré / TLSv1 seul fournisseur, n’est pas livré avec Xamarin.iOS plus. Les projets qui ont été à l’aide de ce fournisseur ancien et maintenant générer avec la plus récente `appletls` une.

Pour résoudre cet avertissement, ouvrez le fichier projet dans un éditeur de texte et supprimer tous les ' MtouchTlsProvider'' nœuds à partir du XML.

### <a name="a-namemt2017mt2017-could-not-process-xml-description"></a><a name="MT2017"/>MT2017 : N’a pas pu traiter la description XML.

Cela signifie qu’une erreur sur le [fichier de configuration de l’éditeur de liens XML personnalisé](https://developer.xamarin.com/guides/cross-platform/advanced/custom_linking/) fournies, veuillez consulter votre fichier.

### <a name="a-namemt2018mt2018-the-assembly--is-referenced-from-two-different-locations--and-"></a><a name="MT2018"/>MT2018 : L’assembly '\*' est référencé à partir de deux emplacements différents : '\*' et ' *'.

L’assembly mentionné dans le message d’erreur est chargé à partir de plusieurs emplacements. Veillez à toujours utiliser la même version d’un assembly.

### <a name="a-namemt2019mt2019-can-not-load-the-root-assembly-"></a><a name="MT2019"/>MT2019 : Ne peut pas charger l’assembly racine ' *'

L’assembly racine n’a pas pu être chargé. Vérifiez que le chemin d’accès dans le message d’erreur fait référence à un fichier existant, et qu’il est d’un assembly .NET valide.

### <a name="a-namemt202xmt202x-binding-optimizer-failed-processing-"></a><a name="MT202x"/>MT202x : Échec de la liaison optimiseur traitement `...`.

Un événement inattendu s’est produite optimiser généré le code de liaison. L’élément à l’origine du problème est nommé dans le message d’erreur. Pour résoudre ce problème, l’assembly nommé (ou qui contient le type ou une méthode nommée) sera doivent être fournies dans un [des bogues, rapport](http://bugzilla.xamarin.com) ainsi que d’un journal de génération complète avec commentaires activée (c'est-à-dire `-v -v -v -v` dans les **mtouch supplémentaire arguments**).

Le dernier chiffre `x` sera :
* `0` pour un nom de l’assembly ;
* `1` nom du type ;
* `3` pour un nom de méthode ;

### <a name="a-namemt2030mt2030-remove-user-resources-failed-processing-"></a><a name="MT2030"/>MT2030 : Ressources d’utilisateur de supprimer le traitement a échoué `...`.

Un événement inattendu s’est produite lors de la tentative de suppression des ressources de l’utilisateur. L’assembly à l’origine du problème est nommé dans le message d’erreur. Pour résoudre ce problème à l’assembly sera doivent être fournies dans un [des bogues, rapport](http://bugzilla.xamarin.com) ainsi que d’un journal de génération complète avec commentaires activée (c'est-à-dire `-v -v -v -v` dans les **les arguments supplémentaires mtouch**).

Ressources de l’utilisateur sont des fichiers inclus dans les assemblys qui doivent être extraites au moment de la build, pour créer l’offre groupée d’application (en tant que ressources). Cela concerne :

* `__monotouch_content_*` et `__monotouch_pages_*` ressources ; et
* Bibliothèques natives incorporé à l’intérieur d’un assembly de liaison ;

### <a name="a-namemt2040mt2040-default-httpmessagehandler-setter-failed-processing-"></a><a name="MT2040"/>MT2040 : Par défaut HttpMessageHandler setter échoué du traitement `...`.

Un événement inattendu s’est produite lors de la tentative de définition de la valeur par défaut `HttpMessageHandler` pour l’application. Entrez un [des bogues, rapport](http://bugzilla.xamarin.com) ainsi que d’un journal de génération complète avec commentaires activée (c'est-à-dire `-v -v -v -v` dans le **les arguments supplémentaires mtouch**).

### <a name="a-namemt2050mt2050-code-remover-failed-processing-"></a><a name="MT2050"/>MT2050 : Échec du traitement du Code de programme de suppression `...`.

Un événement inattendu s’est produite lors de la tentative de suppression du code à partir de BCL avec l’application d’expédition. Entrez un [des bogues, rapport](http://bugzilla.xamarin.com) ainsi que d’un journal de génération complète avec commentaires activée (c'est-à-dire `-v -v -v -v` dans le **les arguments supplémentaires mtouch**).

### <a name="a-namemt2060mt2060-sealer-failed-processing-"></a><a name="MT2060"/>MT2060 : Échec de la fermeture du traitement `...`.

Un événement inattendu s’est produite lors de la tentative de sceller les types ou des méthodes (final), ou lorsque devirtualizing certaines méthodes. L’assembly à l’origine du problème est nommé dans le message d’erreur. Pour résoudre ce problème à l’assembly sera doivent être fournies dans un [des bogues, rapport](http://bugzilla.xamarin.com) ainsi que d’un journal de génération complète avec commentaires activée (c'est-à-dire `-v -v -v -v` dans les **les arguments supplémentaires mtouch**).

### <a name="a-namemt2070mt2070-metadata-reducer-failed-processing-"></a><a name="MT2070"/>MT2070 : Échec du traitement du réducteur de métadonnées `...`.

Un événement inattendu s’est produite lors de la tentative réduire les métadonnées à partir de l’application. L’assembly à l’origine du problème est nommé dans le message d’erreur. Pour résoudre ce problème à l’assembly sera doivent être fournies dans un [des bogues, rapport](http://bugzilla.xamarin.com) ainsi que d’un journal de génération complète avec commentaires activée (c'est-à-dire `-v -v -v -v` dans les **les arguments supplémentaires mtouch**).

### <a name="a-namemt2080mt2080-marknsobjects-failed-processing-"></a><a name="MT2080"/>MT2080 : Échec du traitement du MarkNSObjects `...`.

Un événement inattendu s’est produite lors de la tentative de marquer `NSObject` sous-classes à partir de l’application. L’assembly à l’origine du problème est nommé dans le message d’erreur. Pour résoudre ce problème à l’assembly sera doivent être fournies dans un [des bogues, rapport](http://bugzilla.xamarin.com) ainsi que d’un journal de génération complète avec commentaires activée (c'est-à-dire `-v -v -v -v` dans les **les arguments supplémentaires mtouch**).

<!-- MT21xx: more linker errors -->

<!--- 2100 used by mmp -->

### <a name="a-namemt2101mt2101-cant-resolve-the-reference--referenced-from-the-method--in-"></a><a name="MT2101"/>MT2101 : Impossible de résoudre la référence '\*', référencé à partir de la méthode'\*' dans ' *'.

Une référence d’assembly non valide a été rencontrée lors du traitement de la méthode indiquée dans le message d’erreur.

L’assembly à l’origine du problème est nommé dans le message d’erreur. Pour résoudre ce problème à l’assembly sera doivent être fournies dans un [des bogues, rapport](https://bugzilla.xamarin.com) ainsi que d’un journal de génération complète avec commentaires activée (c'est-à-dire `-v -v -v -v` dans les **les arguments supplémentaires mtouch**).

### <a name="a-namemt2102mt2102-error-processing-the-method--in-the-assembly--"></a><a name="MT2102"/>MT2102 : Erreur lors du traitement de la méthode '\*'dans l’assembly'\*' : *

Un événement inattendu s’est produite lors de la tentative de marquer la méthode indiquée dans le message d’erreur.

L’assembly à l’origine du problème est nommé dans le message d’erreur. Pour résoudre ce problème à l’assembly sera doivent être fournies dans un [des bogues, rapport](https://bugzilla.xamarin.com) ainsi que d’un journal de génération complète avec commentaires activée (c'est-à-dire `-v -v -v -v` dans les **les arguments supplémentaires mtouch**).

## <a name="mt3xxx-aot-error-messages"></a>MT3xxx : Messages d’erreur AOA

<!--
 MT3xxx AOT
  MT30xx AOT (general) errors
  -->

### <a name="a-namemt3001mt3001-could-not-aot-the-assembly-"></a><a name="MT3001"/>Impossible de MT3001 pas AOA l’assembly ' *'

Cela indique généralement un bogue dans le compilateur AOA. Veuillez entrer un bogue [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) avec un projet qui peut être utilisé pour reproduire l’erreur.

Il est parfois possible de contourner ce problème en désactivant les générations incrémentielles dans les e/s du projet option de génération (mais il est toujours un bogue, par conséquent, signalez néanmoins).

### <a name="a-namemt3002mt3002-aot-restriction-method--must-be-static-since-it-is-decorated-with-monopinvokecallback-see-httpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbackshttpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbacks"></a><a name="MT3002"/>Restriction de MT3002 AOA : méthode ' *' doit être statique, car elle est décorée avec [MonoPInvokeCallback]. See [https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks](https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks)

Ce message d’erreur provient le compilateur AOA.



### <a name="a-namemt3003mt3003-conflicting---debug-and---llvm-options-soft-debugging-is-disabled"></a><a name="MT3003"/>MT3003 En conflit--llvm--options et débogage. Soft-débogage est désactivé.

Le débogage n’est pas pris en charge lorsque LLVM est activé. Si vous avez besoin déboguer l’application, désactivez d’abord LLVM.

### <a name="a-namemt3004mt3004-could-not-aot-the-assembly--because-it-doesnt-exist"></a><a name="MT3004"/>Impossible de MT3004 pas AOA l’assembly ' *', car il n’existe pas.



### <a name="a-namemt3005mt3005-the-dependency--of-the-assembly--was-not-found-please-review-the-projects-references"></a><a name="MT3005"/>MT3005 La dépendance '\*'de l’assembly'\*' n’a été trouvé. Passez en revue les références du projet.

Cela se produit généralement lorsqu’un assembly fait référence à une autre version d’un assembly de plateforme (généralement la version .NET 4 de mscorlib.dll).

Cela n’est pas pris en charge et ne peut-être pas générer ou s’exécuter correctement (à partir de la version de .NET 4 de mscorlib.dll la version Xamarin.iOS n’a pas, l’assembly peut utiliser API).

### <a name="a-namemt3006mt3006-could-not-compute-a-complete-dependency-map-for-the-project-this-will-result-in-slower-build-times-because-xamarinios-cant-properly-detect-what-needs-to-be-rebuilt-and-what-does-not-need-to-be-rebuilt-please-review-previous-warnings-for-more-details"></a><a name="MT3006"/>Impossible de MT3006 de calculer une carte complète des dépendances pour le projet. Cela entraîne des durées de génération plus lentes car Xamarin.iOS ne peut pas détecter correctement ce qui doit être reconstruit (et ce qui ne doivent pas être reconstruits). Passez en revue les avertissements précédents pour plus de détails.

 générer ou s’exécuter correctement (à partir de la version de .NET 4 de mscorlib.dll la version Xamarin.iOS n’a pas, l’assembly peut utiliser API).

### <a name="a-namemt3007mt3007-debug-info-files-mdb-will-not-be-loaded-when-llvm-is-enabled"></a><a name="MT3007"/>MT3007 : Fichiers d’informations de débogage (*.mdb) ne seront pas chargés lorsque llvm est activé.

### <a name="a-namemt3008mt3008-bitcode-support-requires-the-use-of-the-llvm-aot-backend---llvm"></a><a name="MT3008"/>MT3008 : Prise en charge Bitcode requiert l’utilisation du serveur principal LLVM AOT (--llvm)

Prise en charge Bitcode requiert l’utilisation du serveur principal LLVM AOT (--llvm).

Désactivez la prise en charge Bitcode ou activer LLVM.

<!--- 3009 used by mmp -->
<!--- 3010 used by mmp -->

## <a name="mt4xxx-code-generation-error-messages"></a>MT4xxx : Messages d’erreur de génération de Code

### <a name="mt40xx-main"></a>MT40xx : principal

<!--
 MT4xxx code generation
  MT40xx main.m
  -->

### <a name="a-namemt4001mt4001-the-main-template-could-not-be-expanded-to-"></a><a name="MT4001"/>MT4001 Impossible de développer le modèle principal à `*`.

Une erreur s’est produite lors de la génération main.m. Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4002mt4002-failed-to-compile-the-generated-code-for-pinvoke-methods-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4002"/>Échec de MT4002 pour compiler le code généré pour les méthodes P/Invoke. Veuillez soumettre un rapport de bogue à http://bugzilla.xamarin.com

Impossible de compiler le code généré pour les méthodes P/Invoke. Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="mt41xx-registrar"></a>MT41xx : bureau d’enregistrement

<!--
  MT41xx registrar.m
  -->

### <a name="a-namemt4101mt4101-the-registrar-cannot-build-a-signature-for-type-"></a><a name="MT4101"/>MT4101 Le bureau d’enregistrement ne peut pas générer une signature de type `*`.

Un type a été trouvé dans les API exporté que le runtime ne sait pas comment marshaler à partir d’objectif-C.

Si vous pensez Xamarin.iOS doit prendre en charge le type en question, veuillez soumettre une demande d’amélioration à [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4102mt4102-the-registrar-found-an-invalid-type--in-signature-for-method--use--instead"></a><a name="MT4102"/>MT4102 Bureau d’enregistrement de trouver un type non valide `*` dans la signature de méthode `*`. Utilisez plutôt `*`.

Cela actuellement se produit uniquement avec un type : System.DateTime. Utilisez l’équivalent Objective-C (NSDate) à la place.

### <a name="a-namemt4103mt4103-the-registrar-found-an-invalid-type--in-signature-for-method--the-type-implements-inativeobject-but-does-not-have-a-constructor-that-takes-two-intptr-bool-arguments"></a><a name="MT4103"/>MT4103 Bureau d’enregistrement de trouver un type non valide `*` dans la signature de méthode `*`: le type implémente INativeObject, mais n’a pas de constructeur qui accepte deux (IntPtr, bool) arguments

Cela se produit lorsque le bureau d’enregistrement rencontrent un type dans une signature avec les caractéristiques mentionnées. Le bureau d’enregistrement devrez peut-être créer de nouvelles instances du type, et dans ce cas, il requiert un constructeur avec (IntPtr, bool) signature - le premier argument (IntPtr) spécifie le handle géré, tandis que la seconde si l’appelant transmet possession du natif gérer (si cette valeur est false, « conserver » sera appelée sur l’objet).

### <a name="a-namemt4104mt4104-the-registrar-cannot-marshal-the-return-value-for-type--in-signature-for-method-"></a><a name="MT4104"/>MT4104 Le bureau d’enregistrement ne peut pas marshaler de la valeur de retour de type `*` dans la signature de méthode `*`.

Un type a été trouvé dans les API exporté que le runtime ne sait pas comment marshaler à partir d’objectif-C.

Si vous pensez Xamarin.iOS doit prendre en charge le type en question, veuillez soumettre une demande d’amélioration à [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4105mt4105-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a><a name="MT4105"/>MT4105 Le bureau d’enregistrement ne peut pas marshaler le paramètre de type `*` dans la signature de méthode `*`.

Si vous pensez Xamarin.iOS doit prendre en charge le type en question, veuillez soumettre une demande d’amélioration à [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4106mt4106-the-registrar-cannot-marshal-the-return-value-for-structure--in-signature-for-method-"></a><a name="MT4106"/>MT4106 Le bureau d’enregistrement ne peut pas marshaler de la valeur de retour pour la structure `*` dans la signature de méthode `*`.

Un type a été trouvé dans les API exporté que le runtime ne sait pas comment marshaler à partir d’objectif-C.

Si vous pensez Xamarin.iOS doit prendre en charge le type en question, veuillez soumettre une demande d’amélioration à [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4107mt4107-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a><a name="MT4107"/>MT4107 Le bureau d’enregistrement ne peut pas marshaler le paramètre de type `*` dans la signature de méthode `+`.

Un type a été trouvé dans les API exporté que le runtime ne sait pas comment marshaler à partir d’objectif-C.

Si vous pensez Xamarin.iOS doit prendre en charge le type en question, veuillez soumettre une demande d’amélioration à [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4108mt4108-the-registrar-cannot-get-the-objectivec-type-for-managed-type-"></a><a name="MT4108"/>MT4108 Le bureau d’enregistrement ne peut pas obtenir le type ObjectiveC pour le type managé `*`.

Un type a été trouvé dans les API exporté que le runtime ne sait pas comment marshaler à partir d’objectif-C.

Si vous pensez Xamarin.iOS doit prendre en charge le type en question, veuillez soumettre une demande d’amélioration à [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4109mt4109-failed-to-compile-the-generated-registrar-code-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4109"/>Échec de MT4109 pour compiler le code généré de bureau d’enregistrement. Veuillez soumettre un rapport de bogue à http://bugzilla.xamarin.com

Impossible de compiler le code généré pour le bureau d’enregistrement. Le journal de génération contient la sortie de la compilation native, expliquant pourquoi le code n’est pas la compilation.

Il s’agit toujours d’un bogue dans Xamarin.iOS ; Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com) avec votre projet ou un cas de test.

### <a name="a-namemt4110mt4110-the-registrar-cannot-marshal-the-out-parameter-of-type--in-signature-for-method-"></a><a name="MT4110"/>MT4110 Le bureau d’enregistrement ne peut pas marshaler le paramètre out de type `*` dans la signature de méthode `*`.



### <a name="a-namemt4111mt4111-the-registrar-cannot-build-a-signature-for-type--in-method-"></a><a name="MT4111"/>MT4111 Le bureau d’enregistrement ne peut pas générer une signature pour le type `*` dans la méthode `*`.



### <a name="a-namemt4112mt4112-the-registrar-found-an-invalid-type--registering-generic-types-with-objective-c-is-not-supported-and-may-lead-to-random-behavior-andor-crashes-for-backwards-compatibility-with-older-versions-of-xamarinios-it-is-possible-to-ignore-this-error-by-passing---unsupported--enable-generics-in-registrar-as-an-additional-mtouch-argument-in-the-projects-ios-build-options-page-see-developerxamarincomguidesiosadvancedtopicsregistrarhttpsdeveloperxamarincomguidesiosadvancedtopicsregistrar-for-more-information"></a><a name="MT4112"/>MT4112 Bureau d’enregistrement de trouver un type non valide `*`. L’inscription des types génériques avec Objective-C n’est pas prise en charge et peut entraîner un comportement aléatoire et/ou des pannes (pour descendante compatibilité avec les versions antérieures de Xamarin.iOS il est possible d’ignorer cette erreur en passant `--unsupported--enable-generics-in-registrar` comme un mtouch supplémentaire argument dans la page d’options Build du projet iOS. Consultez [developer.xamarin.com/guides/ios/advanced_topics/registrar](https://developer.xamarin.com/guides/ios/advanced_topics/registrar) pour plus d’informations).



### <a name="a-namemt4113mt4113-the-registrar-found-a-generic-method--exporting-generic-methods-is-not-supported-and-will-lead-to-random-behavior-andor-crashes"></a><a name="MT4113"/>MT4113 Bureau d’enregistrement de trouver une méthode générique : '\*.\*'. Exportation des méthodes génériques n’est pas prise en charge et entraîne un comportement aléatoire et/ou de pannes.



### <a name="a-namemt4114mt4114-unexpected-error-in-the-registrar-for-the-method----please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4114"/>Une erreur inattendue de MT4114 dans le bureau d’enregistrement pour la méthode '\*.\*'-Veuillez archiver un rapport de bogue à http://bugzilla.xamarin.com



### <a name="a-namemt4116mt4116-could-not-register-the-assembly--"></a><a name="MT4116"/>Impossible de MT4116 pas inscrire l’assembly ' *': *



### <a name="a-namemt4117mt4117-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-method-takes--parameters-while-the-managed-method-has--parameters"></a><a name="MT4117"/>MT4117 Le bureau d’enregistrement a trouvé une non-correspondance de signature dans la méthode '*.*»-le sélecteur indique que la méthode prend * paramètres, alors que la méthode managée a * paramètres.



### <a name="a-namemt4118mt4118-cannot-register-two-managed-types--and--with-the-same-native-name-"></a><a name="MT4118"/>MT4118 ne peut pas inscrire deux types managés ('\*'et'\*') avec le même nom natif ('* ').



### <a name="a-namemt4119mt4119-could-not-register-the-selector--of-the-member--because-the-selector-is-already-registered-on-a-different-member"></a><a name="MT4119"/>MT4119 peut pas inscrire le sélecteur '\*'du membre'\*. *', car le sélecteur est déjà enregistré sur un autre membre.



### <a name="a-namemt4120mt4120-the-registrar-found-an-unknown-field-type--in-field--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4120"/>MT4120 Bureau d’enregistrement de trouver un type de champ inconnu '\*« dans le champ'\*. *'. Veuillez soumettre un rapport de bogue à http://bugzilla.xamarin.com

Cette erreur indique un bogue dans Xamarin.iOS. Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4121mt4121-cannot-use-gccg-to-compile-the-generated-code-from-the-static-registrar-when-using-the-accounts-framework-the-header-files-provided-by-apple-used-during-the-compilation-require-clang-either-use-clang---compilerclang-or-the-dynamic-registrar---registrardynamic"></a><a name="MT4121"/>MT4121 ne peut pas utiliser GCC / G ++ pour compiler le code généré à partir du bureau d’enregistrement statique lors de l’utilisation de l’infrastructure de comptes (les fichiers d’en-tête fournies par Apple utilisé lors de la compilation requièrent Clang). Utilisez l’option Clang (--du compilateur : clang) ou le bureau d’enregistrement dynamique (--bureau d’enregistrement : dynamique).



### <a name="a-namemt4122mt4122-cannot-use-the-clang-compiler-provided-in-the--sdk-to-compile-the-generated-code-from-the-static-registrar-when-non-ascii-type-names--are-present-in-the-application-either-use-gccg---compilergccg-the-dynamic-registrar---registrardynamic-or-a-newer-sdk"></a><a name="MT4122"/>MT4122 ne peut pas utiliser le compilateur Clang fourni dans le *.* Kit de développement logiciel pour compiler le code généré à partir du bureau d’enregistrement statique lors de la non-ASCII les noms de type ('* ') sont présents dans l’application. Utilisez l’option GCC / G ++ (--du compilateur : gcc | g ++), le bureau d’enregistrement dynamique (--bureau d’enregistrement : dynamiques) ou d’un kit de développement logiciel plus récent.



### <a name="a-namemt4123mt4123-the-type-of-the-variadic-parameter-in-the-variadic-function--must-be-systemintptr"></a><a name="MT4123"/>MT4123 Le type du paramètre variadique dans la fonction variadique ' *' doit être System.IntPtr.



### <a name="a-namemt4124mt4124-invalid--found-on--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4124"/>MT4124 non valide * trouvé sur ' *'. Veuillez soumettre un rapport de bogue à http://bugzilla.xamarin.com

Cette erreur indique un bogue dans Xamarin.iOS. Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4125mt4125-the-registrar-found-an-invalid-type--in-signature-for-method--the-interface-must-have-a-protocol-attribute-specifying-its-wrapper-type"></a><a name="MT4125"/>MT4125 Bureau d’enregistrement de trouver un type non valide '\*« dans la signature de méthode'\*' : l’interface doit posséder un attribut de protocole en spécifiant son type de wrapper.



### <a name="a-namemt4126mt4126-cannot-register-two-managed-protocols--and--with-the-same-native-name-"></a><a name="MT4126"/>MT4126 ne peut pas inscrire deux protocoles managés ('\*'et'\*') avec le même nom natif ('* ').



### <a name="a-namemt4127mt4127-cannot-register-more-than-one-interface-method-for-the-method--which-is-implementing-"></a><a name="MT4127"/>MT4127 ne peut pas inscrire plus d’une méthode d’interface pour la méthode '\*» (qui implémente '\*').



### <a name="a-namemt4128mt4128--the-registrar-found-an-invalid-generic-parameter-type--in-the-method--the-generic-parameter-must-have-an-nsobject-constraint"></a><a name="MT4128"/>MT4128 Bureau d’enregistrement de trouver un type de paramètre générique non valide '\*« dans la méthode'\*'. Le paramètre générique doit avoir une contrainte de 'NSObject'.

### <a name="a-namemt4129mt4129--the-registrar-found-an-invalid-generic-return-type--in-the-method--the-generic-return-type-must-have-an-nsobject-constraint"></a><a name="MT4129"/>MT4129 Bureau d’enregistrement de trouver un type de retour générique non valide '\*« dans la méthode'\*'. Le type de retour générique doit avoir une contrainte de 'NSObject'.

### <a name="a-namemt4130mt4130--the-registrar-cannot-export-static-methods-in-generic-classes-"></a><a name="MT4130"/>MT4130 Le bureau d’enregistrement ne peut pas exporter les méthodes statiques dans les classes génériques ('* ').

### <a name="a-namemt4131mt4131--the-registrar-cannot-export-static-properties-in-generic-classes-"></a><a name="MT4131"/>MT4131 Le bureau d’enregistrement ne peut pas exporter les propriétés statiques dans les classes génériques («\*.\*').

### <a name="a-namemt4132mt4132--the-registrar-found-an-invalid-generic-return-type--in-the-property--the-return-type-must-have-an-nsobject-constraint"></a><a name="MT4132"/>MT4132 Bureau d’enregistrement de trouver un type de retour générique non valide '\*« dans la propriété'\*'. Le type de retour doit avoir une contrainte de 'NSObject'.

### <a name="a-namemt4133mt4133--cannot-construct-an-instance-of-the-type--from-objective-c-because-the-type-is-generic-runtime-exception"></a><a name="MT4133"/>MT4133 ne peut pas construire une instance du type ' *' à partir de Objective-C, car le type est générique. [Exception Runtime]

### <a name="a-namemt4134mt4134--your-application-is-using-the--framework-which-isnt-included-in-the-ios-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-ios--while-youre-building-with-the-ios--sdk-please-select-a-newer-sdk-in-your-apps-ios-build-options"></a><a name="MT4134"/>MT4134 Votre application utilise le ' *' framework, qui n’est pas inclus dans le SDK que vous utilisez pour générer votre application iOS (cette infrastructure a été introduite dans iOS *, tandis que vous créez avec iOS * SDK.) Veuillez sélectionner un kit de développement logiciel plus récent dans iOS de de votre application options de génération.

### <a name="a-namemt4135mt4135--the-member--has-an-export-attribute-that-doesnt-specify-a-selector-a-selector-is-required"></a><a name="MT4135"/>MT4135 Le membre '\*.\*' possède un attribut d’exportation qui ne spécifie pas un sélecteur. Un sélecteur est requis.

### <a name="a-namemt4136mt4136--the-registrar-cannot-marshal-the-parameter-type--of-the-parameter--in-the-method-"></a><a name="MT4136"/>MT4136 Le bureau d’enregistrement ne peut pas marshaler le type de paramètre '\*'du paramètre'\*« dans la méthode'\*. *'

<!-- MT4137 is unused -->

### <a name="a-namemt4138mt4138--the-registrar-cannot-marshal-the-property-type--of-the-property-"></a><a name="MT4138"/>MT4138 Le bureau d’enregistrement ne peut pas marshaler le type de propriété '\*'de la propriété'\*. *'.

### <a name="a-namemt4139mt4139--the-registrar-cannot-marshal-the-property-type--of-the-property--properties-with-the-connect-attribute-must-have-a-property-type-of-nsobject-or-a-subclass-of-nsobject"></a><a name="MT4139"/>MT4139 Le bureau d’enregistrement ne peut pas marshaler le type de propriété '\*'de la propriété'\*. *'. Propriétés avec l’attribut [connexion] doivent avoir un type de propriété de NSObject (ou une sous-classe de NSObject).

### <a name="a-namemt4140mt4140--the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-variadic-method-takes--parameters-while-the-managed-method-has--parameters"></a><a name="MT4140"/>MT4140 Le bureau d’enregistrement a trouvé une non-correspondance de signature dans la méthode '*.*»-le sélecteur indique la méthode variadic prend * paramètres, alors que la méthode managée a * paramètres.

### <a name="a-namemt4141mt4141--cannot-register-the-selector--on-the-member--because-xamarinios-implicitly-registers-this-selector"></a><a name="MT4141"/>MT4141 ne peut pas inscrire le sélecteur '\*'sur le membre'\*', car Xamarin.iOS inscrit implicitement ce sélecteur.

Cela se produit lorsque le sous-classement d’un type .NET framework et essaie d’implémenter un « conserver », « release » ou « dealloc » méthode :

```csharp
class MyNSObject : NSObject
{
    [Export ("retain")]
    new void Retain () {}

    [Export ("release")]
    new void Release () {}

    [Export ("dealloc")]
    new void Dealloc () {}
}
```

Il est toutefois possible de substituer ces méthodes si la classe n’est pas la première sous-classe de l’infrastructure de type :

```csharp
class MyNSObject : NSObject
{
}

class MyCustomNSObject : MyNSObject
{
    [Export ("retain")]
    new void Retain () {}

    [Export ("release")]
    new void Release () {}

    [Export ("dealloc")]
    new void Dealloc () {}
}
```

Dans ce cas remplace Xamarin.iOS `retain`, `release` et `dealloc` sur la `MyNSObject` (classe), et il n’existe aucun conflit.

### <a name="a-namemt4142mt4142-failed-to-register-the-type-"></a><a name="MT4142"/>MT4142 : Impossible d’inscrire le type de ' *'.
### <a name="a-namemt4143mt4143-the-objectivec-class--could-not-be-registered-it-does-not-seem-to-derive-from-any-known-objectivec-class-including-nsobject"></a><a name="MT4143"/>MT4143 : La classe ObjectiveC ' *' ne peut pas être enregistré, il ne semble pas dériver à partir de n’importe quelle classe ObjectiveC connu (y compris NSObject).
### <a name="a-namemt4144mt4144-cannot-register-the-method--since-it-does-not-have-an-associated-trampoline-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4144"/>MT4144 : Impossible d’inscrire la méthode ' *', car il n’a pas un trampoline associé. Veuillez archiver un rapport de bogue à http://bugzilla.xamarin.com.

Cela indique un bogue dans Xamarin.iOS. Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4145mt4145-invalid-enum--enums-with-the-native-attribute-must-have-a-underlying-enum-type-of-either-long-or-ulong"></a><a name="MT4145"/>MT4145 : Enum non valide ' *': les enums avec l’attribut [natif] doit avoir un type enum sous-jacent de 'long' ou 'ulong'.
### <a name="a-namemt4146mt4146-the-name-parameter-of-the-registrar-attribute-on-the-class---contains-an-invalid-character--"></a><a name="MT4146"/>MT4146 : Le paramètre de nom de l’attribut de bureau d’enregistrement sur la classe\*' ('\*') contient un caractère non valide : '\*» (\*).

Le nom d’une classe Objectice-C ne peut pas contenir d’espace blanc, ce qui signifie que la `Register` attribut sur la classe managée correspondante ne peut pas avoir un `Name` paramètre ne peut pas contenir des espaces blancs soit.

Vérifiez que le `Register` attribut sur la classe managée mentionnée dans le message d’erreur ne contient pas d’espace blanc.

### <a name="a-namemt4147mt4147-detected-a-protocol-inheriting-from-the-jsexport-protocol-while-using-the-dynamic-registrar-it-is-not-possible-to-export-protocols-to-javascriptcore-dynamically-the-static-registrar-must-be-used-add---registrarstatic-to-the-additional-mtouch-arguments-in-the-projects-ios-build-options-to-select-the-static-registrar"></a><a name="MT4147"/>MT4147 : A détecté un protocole héritant du protocole JSExport lors de l’utilisation du bureau d’enregistrement dynamique. Il n’est pas possible d’exporter les protocoles à JavaScriptCore dynamiquement ; le bureau d’enregistrement statique doit être utilisé (ajoutez '--bureau d’enregistrement : statique pour les arguments supplémentaires mtouch dans les e/s du projet des options de génération pour sélectionner le bureau d’enregistrement statique).
### <a name="a-namemt4148mt4148-the-registrar-found-a-generic-protocol--exporting-generic-protocols-is-not-supported"></a><a name="MT4148"/>MT4148 : Le bureau d’enregistrement trouvé un protocole générique : ' *'. Exportation des protocoles génériques n’est pas prise en charge.
### <a name="a-namemt4149mt4149-cannot-register-the-method--because-the-type-of-the-first-parameter--does-not-match-the-category-type-"></a><a name="MT4149"/>MT4149 : Impossible d’inscrire la méthode '\*.\*', car le type du premier paramètre («\*') ne correspond pas au type de catégorie ('\*').
### <a name="a-namemt4150mt4150-cannot-register-the-type--because-the-type-property--in-its-category-attribute-does-not-inherit-from-nsobject"></a><a name="MT4150"/>MT4150 : Impossible d’inscrire le type '\*', car la propriété de Type ('\*') dans sa catégorie attribut n’hérite pas NSObject.
### <a name="a-namemt4151mt4151-cannot-register-the-type--because-the-type-property-in-its-category-attribute-isnt-set"></a><a name="MT4151"/>MT4151 : Impossible d’inscrire le type de ' *', car la propriété de Type dans son attribut de catégorie n’est pas définie.
### <a name="a-namemt4152mt4152-cannot-register-the-type--as-a-category-because-it-implements-inativeobject-or-subclasses-nsobject"></a><a name="MT4152"/>MT4152 : Impossible d’inscrire le type de ' *' comme une catégorie, car il implémente INativeObject ou une sous-classe NSObject.
### <a name="a-namemt4153mt4153-cannot-register-the-type--as-a-category-because-its-generic"></a><a name="MT4153"/>MT4153 : Impossible d’inscrire le type '\*' en tant que catégorie, car elle est générique.
### <a name="a-namemt4154mt4154-cannot-register-the-method--as-a-category-method-because-its-generic"></a><a name="MT4154"/>MT4154 : Impossible d’inscrire la méthode '\*' comme méthode de catégorie, car elle est générique.
### <a name="a-namemt4155mt4155-cannot-register-the-method--with-the-selector--as-a-category-method-on--because-the-objective-c-already-has-an-implementation-for-this-selector"></a><a name="MT4155"/>MT4155 : Impossible d’inscrire la méthode '\*« avec le sélecteur'\*' comme une méthode de catégorie sur ' *', car le Objective-C a déjà une implémentation de ce sélecteur.
### <a name="a-namemt4156mt4156-cannot-register-two-categories--and--with-the-same-native-name-"></a><a name="MT4156"/>MT4156 : Impossible d’inscrire deux catégories ('\*'et'\*') avec le même nom natif ('* ').
### <a name="a-namemt4157mt4157-cannot-register-the-category-method--because-at-least-one-parameter-is-required-and-its-type-must-match-the-category-type-"></a><a name="MT4157"/>MT4157 : Impossible d’inscrire la méthode de la catégorie '\*', car au moins un paramètre est obligatoire (et son type doit correspondre au type de catégorie '\*')
### <a name="a-namemt4158mt4158-cannot-register-the-constructor--in-the-category--because-constructors-in-categories-are-not-supported"></a><a name="MT4158"/>MT4158 : Impossible d’inscrire le constructeur * dans la catégorie *, car les constructeurs dans les catégories ne sont pas pris en charge.
### <a name="a-namemt4159mt4159-cannot-register-the-method--as-a-category-method-because-category-methods-must-be-static"></a><a name="MT4159"/>MT4159 : Impossible d’inscrire la méthode ' *' comme méthode de catégorie, car les méthodes de catégorie doivent être statiques.

### <a name="a-namemt4160mt4160-invalid-character---found-in-selector--for-"></a><a name="MT4160"/>MT4160 : Caractère non valide '\*» (\*) trouvé dans le sélecteur '\*'for'\*'.

### <a name="a-namemt4161mt4161-the-registrar-found-an-unsupported-structure--all-fields-in-a-structure-must-also-be-structures-field--with-type-2-is-not-a-structure"></a><a name="MT4161"/>MT4161 : Le bureau d’enregistrement trouvé une structure non pris en charge '\*' : tous les champs dans une structure doivent également être des structures (champ '\*' avec le type « {{2} » n’est pas une structure).

Le bureau d’enregistrement trouvé une structure avec des champs non pris en charge.

Tous les champs dans une structure qui est exposé à Objective-C doivent également être des structures (et non des classes).

### <a name="a-namemt4162mt4162-the-type--used-as--2-is-not-available-in---it-was-introduced-in---please-build-with-a-newer--sdk-usually-done-by-using-the-most-recent-version-of-xcode"></a><a name="MT4162"/>MT4162 : Le type '\*' (utilisé en tant que * {{2}) n’est pas disponible dans ** (elle a été introduite dans * *)\* générez avec une version * Kit de développement logiciel (généralement effectuée à l’aide de la version la plus récente de Xcode.

Le bureau d’enregistrement de trouver un type qui n’est pas inclus dans le Kit de développement actuel.

Mettez à niveau Xcode.

### <a name="a-namemt4163mt4163-internal-error-in-the-registrar--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4163"/>MT4163 : Une erreur interne dans le bureau d’enregistrement (*). Veuillez soumettre un rapport de bogue à http://bugzilla.xamarin.com

Cette erreur indique un bogue dans Xamarin.iOS. Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4164mt4164-cannot-export-the-property--because-its-selector--is-an-objective-c-keyword-please-use-a-different-name"></a><a name="MT4164"/>MT4164 : Impossible d’exporter la propriété '\*', car son sélecteur '\*' est un mot clé Objective-C. Utilisez un autre nom.

Le sélecteur pour la propriété en question n’est pas un identificateur valid de Objective-C.

Veuillez utiliser un identificateur valide de Objective-C comme sélecteurs.

### <a name="a-namemt4165mt4165-the-registrar-couldnt-find-the-type-systemvoid-in-any-of-the-referenced-assemblies"></a><a name="MT4165"/>MT4165 : Le bureau d’enregistrement n’a pas pu trouver le type 'System.Void' dans un des assemblys référencés.

Cette erreur probablement indique un bogue dans Xamarin.iOS. Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4166mt4166-cannot-register-the-method--because-the-signature-contains-a-type--that-isnt-a-reference-type"></a><a name="MT4166"/>MT4166 : Impossible d’inscrire la méthode '\*', car la signature contient un type (\*) qui n’est pas un type référence.

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4167mt4167-cannot-register-the-method--because-the-signature-contains-a-generic-type--with-a-generic-argument-type-that-isnt-an-nsobject-subclass-"></a><a name="MT4167"/>MT4167 : Impossible d’inscrire la méthode '\*', car la signature contient un type générique (\*) avec un type d’argument générique qui n’est pas une sous-classe NSObject (*).

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4168mt4168-cannot-register-the-type-managedname-because-its-objective-c-name-exportedname-is-an-objective-c-keyword-please-use-a-different-name"></a><a name="MT4168"/>MT4168 : Impossible d’inscrire le type ' {gérés\_nom}', car son nom Objective-C ' {exporté\_nom}' est un mot clé Objective-C. Utilisez un autre nom.

Le nom de Objective-C pour le type en question n’est pas un identificateur valide de Objective-C.

Veuillez utiliser un identificateur valide de Objective-C.

## <a name="mt5xxx-gcc-and-toolchain-error-messages"></a>MT5xxx : Messages d’erreur GCC et la chaîne d’outils

### <a name="mt51xx-compilation"></a>MT51xx : Compilation

<!--
 MT5xxx GCC and toolchain
  MT51xx compilation
  -->

### <a name="a-namemt5101mt5101-missing--compiler-please-install-xcode-command-line-tools-component"></a><a name="MT5101"/>MT5101 manquant ' *' compilateur. Installez Xcode composant « Outils de ligne de commande »



### <a name="a-namemt5102mt5102-failed-to-assemble-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5102"/>Échec de MT5102 pour assembler le fichier ' *'. Veuillez soumettre un rapport de bogue à http://bugzilla.xamarin.com



### <a name="a-namemt5103mt5103-failed-to-compile-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5103"/>MT5103 n’a pas pu compiler le fichier ' *'. Veuillez soumettre un rapport de bogue à http://bugzilla.xamarin.com



### <a name="a-namemt5104mt5104-could-not-find-neither-the--nor-the--compiler-please-install-xcode-command-line-tools-component"></a><a name="MT5104"/>MT5104 trouvé ni la «\*'et'\*' compilateur. Installez Xcode composant « Outils de ligne de commande »

<!-- 5105 is used by mmp -->

### <a name="a-namemt5106mt5106-could-not-compile-the-files--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5106"/>MT5106 : Impossible de compiler le fichier (s) ' *'. Veuillez soumettre un rapport de bogue à http://bugzilla.xamarin.com

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="mt52xx-linking"></a>MT52xx : liaison

<!--
  MT52xx linking
  -->

### <a name="a-namemt5201mt5201-native-linking-failed-please-review-the-build-log-and-the-user-flags-provided-to-gcc-"></a><a name="MT5201"/>Échec de la liaison MT5201 natif. Passez en revue le journal de génération et les indicateurs d’utilisateur fournis à gcc : *

### <a name="a-namemt5202mt5202-native-linking-failed-please-review-the-build-log"></a><a name="MT5202"/>Échec de la liaison MT5202 natif. Passez en revue le journal de génération.

### <a name="a-namemt5203mt5203-native-linking-warning-"></a><a name="MT5203"/>MT5203 natif liaison Avertissement : *

<!--- 5204-5208 are not used -->

### <a name="a-namemt5209mt5209-native-linking-error-"></a><a name="MT5209"/>MT5209 liaison d’erreur Native : *

### <a name="a-namemt5210mt5210-native-linking-failed-undefined-symbol--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-are-properly-linked-in"></a><a name="MT5210"/>MT5210 : Liaison natif a échoué, symbole non défini : *. Vérifiez que toutes les infrastructures nécessaires ont été référencés et bibliothèques natives sont correctement liées.

Cela se produit lorsque l’éditeur de liens natif ne peut pas rechercher un symbole qui est référencé ailleurs. Il existe plusieurs raisons, que cela peut se produire :

* Une liaison tiers nécessite une infrastructure, mais la liaison ne spécifie pas cela dans son `[LinkWith]` attribut. Solutions :
  - Si vous êtes l’auteur de la liaison des tiers, ou avez accès à sa source, modification de la liaison `[LinkWith]` attribut pour inclure le framework dont il a besoin :

            [LinkWith ("mylib.a", Frameworks = "SystemConfiguration")]

  - Si vous ne pouvez pas modifier la liaison des tiers, vous pouvez lier manuellement avec l’infrastructure requise en passant `-gcc_flags '-framework SystemFramework'` à `mtouch` (cette opération est effectuée en modifiant les arguments mtouch supplémentaires dans la page d’options Build du projet iOS. N’oubliez pas que cela doit être effectuée pour chaque configuration de projet).
* Dans certains cas une liaison managée est composée de plusieurs bibliothèques natives, et tous les doivent être inclus dans les liaisons. Il est possible d’avoir plusieurs bibliothèques natives dans chaque projet de liaison, par conséquent, la solution est d’ajouter simplement toutes les bibliothèques natives requis au projet de liaison.</li>
* Une liaison managée fait référence au symbole natif qui n’existe pas dans la bibliothèque native.
    Cela se produit généralement lorsqu’une liaison qui a existé pendant un certain temps, et le code natif a été modifié pendant ce temps, afin qu’une classe native particulier a été supprimée ou renommée, tandis que la liaison n’a pas été mis à jour.
* Un appel P/Invoke fait référence à un symbole natif qui n’existe pas. En commençant par Xamarin.iOS 7.4 un <a href="#MT5214">MT5214</a> erreur est signalée pour ce cas (pour plus d’informations, consultez MT5214).
* Une liaison tiers / bibliothèque a été générée à l’aide de C++, mais la liaison ne spécifie pas cela dans son `[LinkWith]` attribut. Cela est généralement relativement facile à reconnaître, étant donné que les symboles ont sont décomposés symboles C++ (un exemple courant est `__ZNKSt9exception4whatEv`).
  - Si vous êtes l’auteur de la liaison des tiers, ou avez accès à sa source, modification de la liaison `[LinkWith]` attribut à définir le `IsCxx` indicateur :

            [LinkWith ("mylib.a", IsCxx = true)]

  - Si vous ne pouvez pas modifier la liaison de l’application tierce ou si vous attachez manuellement avec une bibliothèque tierce, vous pouvez définir l’indicateur équivalent en passant <code>-cxx</code> à mtouch (cela en modifiant les arguments mtouch supplémentaires dans la page d’options Build du projet iOS . N’oubliez pas que cela doit être effectuée pour chaque configuration de projet).



### <a name="a-namemt5211mt5211-native-linking-failed-undefined-objective-c-class--the-symbol--could-not-be-found-in-any-of-the-libraries-or-frameworks-linked-with-your-application"></a><a name="MT5211"/>MT5211 : Liaison natif a échoué, classe Objective-C non définie : \*. Le symbole '\*' est introuvable dans les bibliothèques ou les infrastructures liés à votre application.

Cela se produit lorsque l’éditeur de liens natif ne peut pas trouver une classe Objective-C qui est référencée ailleurs. Il existe plusieurs raisons, cela peut se produire : les mêmes que pour [MT5210](#MT5210) et, en outre :

* Une liaison tiers lié un protocole Objective-C, mais ne pas annoter avec la <code>[Protocol]</code> attribut dans sa définition d’api. Solutions :
  - Ajouter manquants `[Protocol]` attribut :

              [BaseType (typeof (NSObject))]
              [Protocol] // Add this
              public interface MyProtocol
              {
              }



### <a name="a-namemt5212mt5212-native-linking-failed-duplicate-symbol-"></a><a name="MT5212"/>MT5212 : Liaison natif a échoué, symbole dupliqué : *.

Cela se produit lorsque l’éditeur de liens natif rencontre des symboles dupliqués entre toutes les bibliothèques natives. Ce message d’erreur Il peut y avoir un ou plusieurs [MT5213](#MT5213) erreurs avec l’emplacement pour chaque occurrence du symbole. Raisons possibles de cette erreur :


* La même bibliothèque native est incluse deux fois.
* Deux des bibliothèques natives distinctes se produisent pour définir les mêmes symboles.
* Une bibliothèque native n’est pas correctement construite et contient le même symbole plusieurs fois.
  Vous pouvez le vérifier à l’aide de l’ensemble suivant de commandes à partir d’un terminal (remplacez i386 x86_64/armv7/armv7s/arm64, en fonction de l’architecture sur laquelle vous créez pour) :

        # Native libraries are usually fat libraries, containing binary code for
        # several architectures in the same file. First we extract the binary
        # code for the architecture we're interested in.
        lipo libNative.a -thin i386 -output libNative.i386.a

        # Now query the native library for the duplicated symbol.
        nm libNative.i386.a | fgrep 'SYMBOL'

        # You can also list the object files inside the native library.
        # In most cases this will reveal duplicated object files.
        ar -t libNative.i386.a

  Il existe plusieurs façons possibles pour résoudre ce problème :

  - Demande que le fournisseur de la bibliothèque native Corrigez-le et fournit la version mise à jour.
  - Résoudre vous-même en supprimant les fichiers d’objet supplémentaire (fonctionne uniquement si le problème est en fait des fichiers objets en double)


            # Find out if the library is a fat library, and which
            # architectures it contains.
            lipo -info libNative.a

            # Extract each architecture (i386/x86_64/armv7/armv7s/arm64) to a separate file
            lipo libNative.a -thin ARCH -output libNative.ARCH.a

            # Extract the object files for the offending architecture
            # This will remove the duplicates by overwriting them
            # (since they have the same filename)
            mkdir -p ARCH
            cd ARCH
            ar -x ../libNative.ARCH.a

            # Reassemble the object files in an .a
            ar -r ../libNative.ARCH.a *.o
            cd ..

            # Reassemble the fat library
            lipo *.a -create -output libNative.a


  - Demandez à l’éditeur de liens à supprimer du code non utilisé. Xamarin.iOS fera automatiquement si toutes les conditions suivantes sont remplies :
    - Toutes les liaisons des tiers `[LinkWith]` attributs ont activé SmartLink :

            [assembly: LinkWith ("libNative.a", SmartLink = true)]

    - Ne `-gcc_flags` est passé à mtouch (dans le champ d’arguments supplémentaires mtouch d’e/s du projet options de Build).
    - Il est également possible de demander à l’éditeur de liens directement à supprimer du code non utilisé en ajoutant `-gcc_flags -dead_strip` options de génération pour les arguments supplémentaires mtouch dans les e/s du projet.


### <a name="a-namemt5213mt5213-duplicate-symbol-in--location-related-to-previous-error"></a><a name="MT5213"/>MT5213 : Dupliquer le symbole dans : * (emplacement liés à l’erreur précédente)

Cette erreur est signalée uniquement avec [MT5212](#MT5212). Consultez [MT5212](#MT5212) pour plus d’informations.

### <a name="a-namemt5214mt5214--native-linking-failed-undefined-symbol--this-symbol-was-referenced-the-managed-member--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-linked"></a><a name="MT5214"/>MT5214 natif de liaison a échoué, symbole non défini : *. Ce symbole a été référencé le membre managé *. Vérifiez que toutes les infrastructures nécessaires ont été bibliothèques natives et référencés liés.

Cette erreur est signalée lorsque le code managé contient un appel P/Invoke vers une méthode native qui n’existe pas. Exemple :

```csharp
using System.Runtime.InteropServices;
class MyImports {
   [DllImport ("__Internal")]
   public static extern void MyInexistentFunc ();
}
```

Il existe quelques solutions possibles :

  -  Supprimez le P/Invoke en question à partir du code source.
  -  Activer l’éditeur de liens managé pour tous les assemblys (cela dans les e/s du projet options de Build, en définissant « Comportement de l’éditeur de liens » à « Tous les assemblys »). Cela supprimerez tous le P/Invoke vous n’utilisez pas de l’application (automatiquement, au lieu de manuellement comme le point précédent). L’inconvénient est que cela permet de rendre vos builds simulateur un peu plus lentes et cette action peut interrompre votre application si elle utilise la réflexion - trouverez plus d’informations sur l’éditeur de liens [ici](~/ios/deploy-test/linker.md) )
  -  Créer une deuxième bibliothèque native qui contient les symboles natifs manquants. Notez qu’il s’agit simplement d’une solution de contournement (si vous vous trouvez à essayer d’appeler ces fonctions, votre application se bloque).

### <a name="a-namemt5215mt5215-references-to--might-require-additional--frameworkxxx-or--lxxx-instructions-to-the-native-linker"></a><a name="MT5215"/>MT5215 : Fait référence à ' *' peut nécessiter plus - framework = XXX ou lXXX - instructions pour l’éditeur de liens natif

Il s’agit d’un avertissement indiquant qu’un appel P/Invoke a été détecté pour référencer la bibliothèque en question, mais l’application n’est pas lié avec lui.

### <a name="a-namemt5216mt5216-native-linking-failed-for--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5216"/>MT5216 : Échec de la liaison natif pour *. Veuillez soumettre un rapport de bogue à http://bugzilla.xamarin.com

Cette erreur est signalée lors de la liaison de la sortie du compilateur AOA.

Cette erreur probablement indique un bogue dans Xamarin.iOS. Veuillez archiver un rapport de bogue à [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt5217mt5217-native-linking-possibly-failed-because-the-linker-command-line-was-too-long--characters"></a><a name="MT5217"/>MT5217 : Liaison natif éventuellement a échoué, car la ligne de commande de l’éditeur de liens a été trop longue (* caractères).

Échec de la liaison natif, et il est possible du fait que la commande de l’éditeur de liens a été trop longue.

Les projets Xamarin.iOS souvent référence symbole natif dynamiquement, ce qui signifie que l’éditeur de liens native peut supprimer des symboles natifs pendant le processus de liaison natif, car l’éditeur de liens natif ne voit pas que ces symboles sont utilisés.

Généralement Xamarin.iOS demandera à l’éditeur de liens natif pour conserver des symboles à l’aide de la `-u symbol` indicateur de l’éditeur de liens, mais s’il existe de nombreuses des symboles, l’ensemble de ligne de commande peut dépasser la longueur maximale de ligne de commande tel que spécifié par le système d’exploitation.

Il existe quelques sources possibles pour ces symboles dynamiques :

* P/Invoke aux méthodes des bibliothèques liées statiquement (où le nom de la dll est `__Internal` dans l’attribut DllImport `[DllImport ("__Internal")]`).
* Champ de références à des emplacements de mémoire dans les bibliothèques liées statiquement à partir de la liaison des projets (`[Field]` attributs).
* Classes objective-C référencé dans les bibliothèques liées statiquement à partir de la liaison des projets (lors de l’utilisation de générations incrémentielles ou lorsque vous n’utilisez ne pas le bureau d’enregistrement statique).

Solutions possibles :

* Activer l’éditeur de liens géré (si possible pour tous les assemblys au lieu des assemblys SDK uniquement). Cela peut supprimer suffisamment de sources pour les symboles dynamiques afin que l’éditeur de liens 's de ligne de commande n’a dépassé le nombre maximal.
* Réduisez le nombre de P/Invoke, des références de champs et/ou les classes Objective-C.
* Réécrivez les symboles dynamiques pour un nom plus court.
* Transmettez `-dlsym:false` comme argument mtouch supplémentaires dans les e/s du projet des options de génération. Avec cette option, Xamarin.iOS génère une référence native dans le code compilé d’AOA et n’avez pas besoin de demander à l’éditeur de liens pour conserver ce symbole. Toutefois, les builds cette fonctionne uniquement pour les appareils et il génère des erreurs de l’éditeur de liens s’il existe de P/Invoke vers des fonctions qui n’existent pas dans la bibliothèque statique.
* Passer `--dynamic-symbol-mode=code` options de génération en tant qu’arguments mtouch supplémentaires dans les e/s du projet. Avec cette option, Xamarin.iOS génèrent le code natif supplémentaire qui fait référence à ces symboles au lieu de demander de l’éditeur de liens natif pour conserver ces symboles à l’aide des arguments de ligne de commande. L’inconvénient de cette approche est qu’il augmente quelque peu la taille de l’exécutable.
* Activer le bureau d’enregistrement statique en passant `--registrar:static` comme argument mtouch supplémentaires dans les e/s du projet Build options (pour les versions de simulateur, étant donné que le bureau d’enregistrement statique est déjà la valeur par défaut pour les versions de l’appareil). Le bureau d’enregistrement statique génère un code qui fait référence à des classes de Objective-C de manière statique, il est donc inutile pour demander à l’éditeur de liens natif pour conserver ces classes.
* Désactiver les générations incrémentielles (pour les builds d’appareil). Lorsque les builds incrémentielles sont activées, le code généré par le bureau d’enregistrement statique ne sont pas pris en compte par l’éditeur de liens native, ce qui signifie que Xamarin.iOS doit demander toujours conserver l’éditeur de liens référencé Objective-C classes. Par conséquent, la désactivation de générations incrémentielles empêchera ce besoin.

### <a name="a-namemt5218mt5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a><a name="MT5218"/>MT5218 : Impossible d’ignorer le symbole dynamique {symbol} (--symbole dynamique ignorer = {symbol}), car il n’a pas été détecté comme un symbole dynamique.

L’argument de ligne de commande `--ignore-dynamic-symbol=symbol` a été passé, mais ce symbole n’est pas un symbole qui a été reconnu comme un symbole dynamique qui doit être conservé manuellement.

Il existe deux raisons à cela :

* Le nom du symbole est incorrect.
    * Ne pas ajouter un trait de soulignement pour le nom du symbole.
    * Le symbole pour les classes de Objective-C est `OBJC_CLASS_$_<classname>`.
* Le symbole est correct, mais il est un symbole qui est déjà conservé par des moyens normaux (certaines build causes d’options la liste exacte des symboles dynamiques pour faire varier).

### <a name="mt53xx-other-tools"></a>MT53xx : Autres outils

<!--
  MT53xx other tools
  -->

### <a name="a-namemt5301mt5301-missing-strip-tool-please-install-xcode-command-line-tools-component"></a><a name="MT5301"/>MT5301 : Manquant 'supprimer' un outil. Installez Xcode composant « Outils de ligne de commande »



### <a name="a-namemt5302mt5302-missing-dsymutil-tool-please-install-xcode-command-line-tools-component"></a><a name="MT5302"/>MT5302 : Outil de 'dsymutil' manquant. Installez Xcode composant « Outils de ligne de commande »



### <a name="a-namemt5303mt5303-failed-to-generate-the-debug-symbols-dsym-directory-please-review-the-build-log"></a><a name="MT5303"/>MT5303 : Impossible de générer les symboles de débogage (répertoire dSYM). Passez en revue le journal de génération.

Une erreur s’est produite lors de l’exécution dsymutil sur le répertoire final .app pour créer les symboles de débogage. Passez en revue le journal de génération pour la sortie s’affiche de dsymutil et voir comment il peut être résolu.

### <a name="a-namemt5304mt5304-failed-to-strip-the-final-binary-please-review-the-build-log"></a><a name="MT5304"/>MT5304 : Impossible de supprimer le fichier binaire final. Passez en revue le journal de génération.

Une erreur s’est produite lors de l’exécution de l’outil de « bande » pour supprimer les informations de débogage de l’application.

### <a name="a-namemt5305mt5305-missing-lipo-tool-please-install-xcode-command-line-tools-component"></a><a name="MT5305"/>MT5305 : Outil de 'lipo' manquant. Installez Xcode composant « Outils de ligne de commande »



### <a name="a-namemt5306mt5306-failed-to-create-the-a-fat-library-please-review-the-build-log"></a><a name="MT5306"/>MT5306 : Impossible de créer l’une bibliothèque fat. Passez en revue le journal de génération.

Une erreur s’est produite lors de l’exécution de l’outil 'lipo'. Passez en revue le journal de génération pour voir l’erreur signalée par « lipo ».

### <a name="a-namemt5307mt5307-failed-to-sign-the-executable-please-review-the-build-log"></a><a name="MT5307"/>MT5307 : Impossible de signer le fichier exécutable. Passez en revue le journal de génération.

Une erreur s’est produite lors de la signature de l’application. Passez en revue le journal de génération pour voir l’erreur signalée par « codesign ».

<!-- 5308 is used by mmp -->
<!-- 5309 is used by mmp -->
<!-- 5310 is used by mmp -->

## <a name="mt6xxx-mtouch-internal-tools-error-messages"></a>MT6xxx : messages d’erreur des outils mtouch interne

### <a name="mt600x-stripper"></a>MT600x : rectification

<!--
 MT6xxx mtouch internal tools
  MT600x Stripper
  -->

### <a name="a-namemt6001mt6001-running-version-of-cecil-doesnt-support-assembly-stripping"></a><a name="MT6001"/>MT6001 : Version en cours d’exécution de Cecil ne prend pas en charge de suppression d’assembly

### <a name="a-namemt6002mt6002-could-not-strip-assembly-"></a><a name="MT6002"/>MT6002 : N’a pas pu supprimer assembly `*`.

Une erreur s’est produite lors de la suppression du code managé (en supprimant le code MILS) à partir des assemblys dans l’application.

### <a name="a-namemt6003mt6003-unauthorizedaccessexception-message"></a><a name="MT6003"/>MT6003 : [Message UnauthorizedAccessException]

Une erreur de sécurité s’est produite lors de la suppression des symboles à partir de l’application de débogage.

## <a name="mt7xxx-msbuild-error-messages"></a>MT7xxx : Messages d’erreur MSBuild

<!--
 MT7xxx msbuild errors
  -->

### <a name="a-namemt7001mt7001-could-not-resolve-host-ips-for-wifi-debugger-settings"></a><a name="MT7001"/>MT7001 : N’a pas pu résoudre l’hôte des adresses IP pour les paramètres du débogueur Wi-Fi.

*Tâche MSBuild : DetectDebugNetworkConfigurationTaskBase*

Étapes de dépannage :

- Essayez d’exécuter `csharp -e 'System.Net.Dns.GetHostEntry (System.Net.Dns.GetHostName ()).AddressList'` (qui doivent vous fournir une adresse IP et non une erreur évidemment).
- Essayez d’exécuter « ping \`nom d’hôte\`» qui peut vous donner plus d’informations, tels que : `cannot resolve MyHost.local: Unknown host`

Dans certains cas, il s’agit d’un problème de « réseau local » et peut être traité par l’ajout de l’hôte inconnu `127.0.0.1   MyHost.local` dans `/etc/hosts`.

### <a name="a-namemt7002mt7002-this-machine-does-not-have-any-network-adapters-this-is-required-when-debugging-or-profiling-on-device-over-wifi"></a><a name="MT7002"/>MT7002 : Cet ordinateur n’a pas de cartes réseau. Cela est nécessaire lors du débogage ou de profilage sur l’appareil via le Wi-Fi.

*Tâche MSBuild : DetectDebugNetworkConfigurationTaskBase*

### <a name="a-namemt7003mt7003-the-app-extension--does-not-contain-an-infoplist"></a><a name="MT7003"/>MT7003 : L’Extension d’application ' *' ne contient pas un fichier Info.plist.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7004mt7004-the-app-extension--does-not-specify-a-cfbundleidentifier"></a><a name="MT7004"/>MT7004 : L’Extension d’application ' *' ne spécifie pas un CFBundleIdentifier.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7005mt7005-the-app-extension--does-not-specify-a-cfbundleexecutable"></a><a name="MT7005"/>MT7005 : L’Extension d’application ' *' ne spécifie pas un CFBundleExecutable.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7006mt7006-the-app-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7006"/>MT7006 : L’Extension d’application '\*' a un CFBundleIdentifier non valide (\*), il ne commence pas par CFBundleIdentifier du groupe principal de l’application (*).

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7007mt7007-the-app-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a><a name="MT7007"/>MT7007 : L’Extension d’application '\*' a un CFBundleIdentifier (\*) qui se termine par le suffixe non conforme de « .key ».

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7008mt7008-the-app-extension--does-not-specify-a-cfbundleshortversionstring"></a><a name="MT7008"/>MT7008 : L’Extension d’application ' *' ne spécifie pas un CFBundleShortVersionString.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7009mt7009-the-app-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a><a name="MT7009"/>MT7009 : L’Extension d’application ' *' a un Info.plist non valide : il ne contient pas un dictionnaire NSExtension.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7010mt7010-the-app-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionpointidentifier-value"></a><a name="MT7010"/>MT7010 : L’Extension d’application ' *' a un Info.plist non valide : le dictionnaire NSExtension ne contient pas une valeur NSExtensionPointIdentifier.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7011mt7011-the-watchkit-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionattributes-dictionary"></a><a name="MT7011"/>MT7011 : Le WatchKit Extension ' *' a un Info.plist non valide : le dictionnaire NSExtension ne contient-elle pas un dictionnaire NSExtensionAttributes.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7012mt7012-the-watchkit-extension--does-not-have-exactly-one-watch-app"></a><a name="MT7012"/>MT7012 : Le WatchKit Extension ' *' n’a pas exactement une seule application de surveillance.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7013mt7013-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-must-contain-the-watch-companion-capability"></a><a name="MT7013"/>MT7013 : Le WatchKit Extension ' *' a un Info.plist non valide : UIRequiredDeviceCapabilities doit contenir la fonction 'espion-accompagnement'.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7014mt7014-the-watch-app--does-not-contain-an-infoplist"></a><a name="MT7014"/>MT7014 : L’application Espion ' *' ne contient pas un fichier Info.plist.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7015mt7015-the-watch-app--does-not-specify-a-cfbundleidentifier"></a><a name="MT7015"/>MT7015 : L’application Espion ' *' ne spécifie pas un CFBundleIdentifier.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7016mt7016-the-watch-app--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7016"/>MT7016 : L’application Espion '\*' a un CFBundleIdentifier non valide (\*), il ne commence pas par CFBundleIdentifier du groupe principal de l’application (*).

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7017mt7017-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected-watch-4-but-found--"></a><a name="MT7017"/>MT7017 : L’application Espion '\*' n’a pas une valeur UIDeviceFamily valide. Attendu 'espion (4)', mais trouvé '\* (*) ».

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7018mt7018-the-watch-app--does-not-specify-a-cfbundleexecutable"></a><a name="MT7018"/>MT7018 : L’application Espion ' *' ne spécifie pas un CFBundleExecutable

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7019mt7019-the-watch-app--has-an-invalid-wkcompanionappbundleidentifier-value--it-does-not-match-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7019"/>MT7019 : L’application Espion '\*' a une valeur WKCompanionAppBundleIdentifier non valide ('\*'), il ne correspond pas à CFBundleIdentifier de l’application principal de l’application ('* ').

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7020mt7020-the-watch-app--has-an-invalid-infoplist-the-wkwatchkitapp-key-must-be-present-and-have-a-value-of-true"></a><a name="MT7020"/>MT7020 : L’application Espion ' *' a un Info.plist non valide : la clé WKWatchKitApp doit être présente et avoir la valeur 'true'.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7021mt7021-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a><a name="MT7021"/>MT7021 : L’application Espion ' *' a un Info.plist non valide : la clé LSRequiresIPhoneOS ne doit pas être présente.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7022mt7022-the-watch-app--does-not-contain-a-watch-extension"></a><a name="MT7022"/>MT7022 : L’application Espion ' *' ne contient pas une Extension de surveillance.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7023mt7023-the-watch-extension--does-not-contain-an-infoplist"></a><a name="MT7023"/>MT7023 : L’Extension de surveillance ' *' ne contient pas un fichier Info.plist.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7024mt7024-the-watch-extension--does-not-specify-a-cfbundleidentifier"></a><a name="MT7024"/>MT7024 : L’Extension de surveillance ' *' ne spécifie pas un CFBundleIdentifier.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7025mt7025-the-watch-extension--does-not-specify-a-cfbundleexecutable"></a><a name="MT7025"/>MT7025 : L’Extension de surveillance ' *' ne spécifie pas un CFBundleExecutable.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7026mt7026-the-watch-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7026"/>MT7026 : L’Extension de surveillance '\*' a un CFBundleIdentifier non valide (\*), il ne commence pas par CFBundleIdentifier du groupe principal de l’application (*).

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7027mt7027-the-watch-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a><a name="MT7027"/>MT7027 : L’Extension de surveillance '\*' a un CFBundleIdentifier (\*) qui se termine par le suffixe non conforme de « .key ».

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7028mt7028-the-watch-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a><a name="MT7028"/>MT7028 : L’Extension de surveillance ' *' a un Info.plist non valide : il ne contient pas un dictionnaire NSExtension.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7029mt7029-the-watch-extension--has-an-invalid-infoplist-the-nsextensionpointidentifier-must-be-comapplewatchkit"></a><a name="MT7029"/>MT7029 : L’Extension de surveillance ' *' a un Info.plist non valide : le NSExtensionPointIdentifier doit être « com.apple.watchkit ».

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7030mt7030-the-watch-extension--has-an-invalid-infoplist-the-nsextension-dictionary-must-contain-nsextensionattributes"></a><a name="MT7030"/>MT7030 : L’Extension de surveillance ' *' a un Info.plist non valide : le dictionnaire NSExtension doit contenir NSExtensionAttributes.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7031mt7031-the-watch-extension--has-an-invalid-infoplist-the-nsextensionattributes-dictionary-must-contain-a-wkappbundleidentifier"></a><a name="MT7031"/>MT7031 : L’Extension de surveillance ' *' a un Info.plist non valide : le dictionnaire NSExtensionAttributes doit contenir un WKAppBundleIdentifier.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7032mt7032-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-should-not-contain-the-watch-companion-capability"></a><a name="MT7032"/>MT7032 : Le WatchKit Extension ' *' a un Info.plist non valide : UIRequiredDeviceCapabilities ne doit pas contenir la fonctionnalité 'espion-accompagnement'.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7033mt7033-the-watch-app--does-not-contain-an-infoplist"></a><a name="MT7033"/>MT7033 : L’application Espion ' *' ne contient pas un fichier Info.plist.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7034mt7034-the-watch-app--does-not-specify-a-cfbundleidentifier"></a><a name="MT7034"/>MT7034 : L’application Espion ' *' ne spécifie pas un CFBundleIdentifier.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7035mt7035-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected--but-found--"></a><a name="MT7035"/>MT7035 : L’application Espion '\*' n’a pas une valeur UIDeviceFamily valide. Attendu '\*'mais rencontré'\* (\*)'.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7036mt7036-the-watch-app--does-not-specify-a-cfbundleexecutable"></a><a name="MT7036"/>MT7036 : L’application Espion ' *' ne spécifie pas un CFBundleExecutable.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7037mt7037-the-watchkit-extension-extensionname-has-an-invalid-wkappbundleidentifier-value--it-does-not-match-the-watch-apps-cfbundleidentifier-"></a><a name="MT7037"/>MT7037 : Le WatchKit Extension '{extensionName}' a une valeur WKAppBundleIdentifier non valide ('\*»), il ne correspond pas à CFBundleIdentifier l’application espion ('\*').

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7038mt7038-the-watch-app--has-an-invalid-infoplist-the-wkcompanionappbundleidentifier-must-exist-and-must-match-the-main-app-bundles-cfbundleidentifier"></a><a name="MT7038"/>MT7038 : L’application Espion ' *' a un Info.plist non valide : le WKCompanionAppBundleIdentifier doit exister et doit correspondre à CFBundleIdentifier de l’application principal de l’application.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7039mt7039-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a><a name="MT7039"/>MT7039 : L’application Espion ' *' a un Info.plist non valide : la clé LSRequiresIPhoneOS ne doit pas être présente.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7040mt7040-the-app-bundle-appbundlepath-does-not-contain-an-infoplist"></a><a name="MT7040"/>MT7040 : L’offre groupée {AppBundlePath} ne contient pas un fichier Info.plist.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7041mt7041-main-infoplist-path-does-not-specify-a-cfbundleidentifier"></a><a name="MT7041"/>MT7041 : Le chemin d’accès principal Info.plist ne spécifie pas un CFBundleIdentifier.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7042mt7042-main-infoplist-path-does-not-specify-a-cfbundleexecutable"></a><a name="MT7042"/>MT7042 : Le chemin d’accès principal Info.plist ne spécifie pas un CFBundleExecutable.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7043mt7043-main-infoplist-path-does-not-specify-a-cfbundlesupportedplatforms"></a><a name="MT7043"/>MT7043 : Le chemin d’accès principal Info.plist ne spécifie pas un CFBundleSupportedPlatforms.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7044mt7044-main-infoplist-path-does-not-specify-a-uidevicefamily"></a><a name="MT7044"/>MT7044 : Le chemin d’accès principal Info.plist ne spécifie pas un UIDeviceFamily.

*Tâche MSBuild : ValidateAppBundleTaskBase*

### <a name="a-namemt7045mt7045-unrecognized-format-"></a><a name="MT7045"/>MT7045 : Non reconnue de Format : *.

*Tâche MSBuild : PropertyListEditorTaskBase*

Où * peut être :

- chaîne
- array
- dict
- bool
- réels
- entiers
- date
- Données

### <a name="a-namemt7046mt7046-add-entry--incorrectly-specified"></a><a name="MT7046"/>MT7046 : Ajouter : entrée, *, incorrectement spécifié.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7047mt7047-add-entry--contains-invalid-array-index"></a><a name="MT7047"/>MT7047 : Ajouter : entrée, *, contient l’Index de tableau non valide.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7048mt7048-add--entry-already-exists"></a><a name="MT7048"/>MT7048 : Ajouter : * l’entrée existe déjà.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7049mt7049-add-cant-add-entry--to-parent"></a><a name="MT7049"/>MT7049 : Ajouter : Impossible d’ajouter l’entrée, *, vers le Parent.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7050mt7050-delete-cant-delete-entry--from-parent"></a><a name="MT7050"/>MT7050 : Supprimer : Impossible de supprimer l’entrée, *, à partir du Parent.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7051mt7051-delete-entry--contains-invalid-array-index"></a><a name="MT7051"/>MT7051 : Supprimer : entrée, *, contient l’Index de tableau non valide.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7052mt7052-delete-entry--does-not-exist"></a><a name="MT7052"/>MT7052 : Supprimer : entrée, *, n’existe pas.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7053mt7053-import-entry--incorrectly-specified"></a><a name="MT7053"/>MT7053 : Importer : entrée, *, incorrectement spécifié.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7054mt7054-import-entry--contains-invalid-array-index"></a><a name="MT7054"/>MT7054 : Importer : entrée, *, contient l’Index de tableau non valide.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7055mt7055-import-error-reading-file-"></a><a name="MT7055"/>MT7055 : Importer : erreur de lecture du fichier : *.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7056mt7056-import-cant-add-entry--to-parent"></a><a name="MT7056"/>MT7056 : Importer : Impossible d’ajouter l’entrée, *, vers le Parent.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7057mt7057-merge-cant-add-array-entries-to-dict"></a><a name="MT7057"/>MT7057 : Fusion : Impossible d’ajouter des entrées de tableau au dictionnaire

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7058mt7058-merge-specified-entry-must-be-a-container"></a><a name="MT7058"/>MT7058 : Fusion : spécifié d’entrée doit être un conteneur.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7059mt7059-merge-entry--contains-invalid-array-index"></a><a name="MT7059"/>MT7059 : Fusion : entrée, *, contient l’Index de tableau non valide.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7060mt7060-merge-entry--does-not-exist"></a><a name="MT7060"/>MT7060 : Fusion : entrée, *, n’existe pas.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7061mt7061-merge-error-reading-file-"></a><a name="MT7061"/>MT7061 : Fusion : erreur de lecture du fichier : *.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7062mt7062-set-entry--incorrectly-specified"></a><a name="MT7062"/>MT7062 : Valeur : entrée, *, incorrectement spécifié.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7063mt7063-set-entry--contains-invalid-array-index"></a><a name="MT7063"/>MT7063 : Valeur : entrée, *, contient l’Index de tableau non valide.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7064mt7064-set-entry--does-not-exist"></a><a name="MT7064"/>MT7064 : Valeur : entrée, *, n’existe pas.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7065mt7065-unknown-propertylist-editor-action-"></a><a name="MT7065"/>MT7065 : Action de l’éditeur PropertyList inconnu : *.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7066mt7066-error-loading--"></a><a name="MT7066"/>MT7066 : Erreur lors du chargement ' *': *.

*Tâche MSBuild : PropertyListEditorTaskBase*

### <a name="a-namemt7067mt7067-error-saving--"></a><a name="MT7067"/>MT7067 : Erreur de sauvegarde ' *': *.

*Tâche MSBuild : PropertyListEditorTaskBase*


## <a name="mt8xxx-runtime-error-messages"></a>MT8xxx : Messages d’erreur d’exécution

<!--
 MT8xxx runtime
  MT800x misc
  -->

### <a name="a-namemt8001mt8001--version-mismatch-between-the-native-xamarinios-runtime-and-monotouchdll-please-reinstall-xamarinios"></a><a name="MT8001"/>Incompatibilité de Version de MT8001 entre le runtime natif de Xamarin.iOS et monotouch.dll. Réinstallez Xamarin.iOS.

### <a name="a-namemt8002mt8002--could-not-find-the-method--in-the-type-"></a><a name="MT8002"/>MT8002 Impossible de trouver la méthode '\*'dans le type'\*'.

### <a name="a-namemt8003mt8003--failed-to-find-the-closed-generic-method--on-the-type-"></a><a name="MT8003"/>MT8003 Impossible de trouver la méthode générique fermée '\*'sur le type'\*'.

### <a name="a-namemt8004mt8004-cannot-create-an-instance-of--for-the-native-object-0x-of-type--because-another-instance-already-exists-for-this-native-object-of-type-"></a><a name="MT8004"/>MT8004 : Impossible de créer une instance de * pour l’objet natif 0 x * (de type ' *'), car une autre instance existe déjà pour cet objet natif (de type *).

### <a name="a-namemt8005mt8005-wrapper-type--is-missing-its-native-objectivec-class-"></a><a name="MT8005"/>MT8005 : Type de Wrapper '\*'manque sa classe ObjectiveC natif'\*'.

### <a name="a-namemt8006mt8006-failed-to-find-the-selector--on-the-type-"></a><a name="MT8006"/>MT8006 : Impossible de trouver le sélecteur '\*'sur le type'\*'

### <a name="a-namemt8007mt8007-cannot-get-the-method-descriptor-for-the-selector--on-the-type--because-the-selector-does-not-correspond-to-a-method"></a><a name="MT8007"/>MT8007 : Impossible d’obtenir le descripteur de méthode pour le sélecteur '\*'sur le type'\*', car le sélecteur ne correspond pas à une méthode

### <a name="a-namemt8008mt8008-the-loaded-version-of-xamariniosdll-was-compiled-for--bits-while-the-process-is--bits-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8008"/>MT8008 : La version chargée de Xamarin.iOS.dll a été compilée pour * le service bits, tandis que le processus est * bits. Veuillez entrer un bogue à http://bugzilla.xamarin.com.

Cela indique un problème se pose dans le processus de génération. Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8009mt8009-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-s-parameter--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8009"/>MT8009 : Impossible de trouver le bloc de méthode de conversion pour la méthode de délégué *.*' s paramètre #*. Veuillez entrer un bogue à http://bugzilla.xamarin.com.

Cela indique une API n’a pas été liée correctement. S’il s’agit d’une API exposée par Xamarin, Veuillez entrer un bogue dans notre bugzilla ([http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)), s’il s’agit d’une liaison de tiers, contactez le fournisseur.

### <a name="a-namemt8010mt8010-native-type-size-mismatch-between-xamariniosmacdll-and-the-executing-architecture-xamariniosmacdll-was-built-for--bit-while-the-current-process-is--bit"></a><a name="MT8010"/>MT8010 : Incompatibilité de taille type natif entre Xamarin. [iOS | Mac] .dll et l’architecture en cours d’exécution. Xamarin. [iOS | Mac] .dll a été généré pour *-bit, alors que le processus en cours est *-bits.

Cela indique un problème se pose dans le processus de génération. Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8011mt8011-unable-to-locate-the-delegate-to-block-conversion-attribute-delegateproxy-for-the-return-value-for-the-method--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8011"/>MT8011 : Impossible de localiser le délégué à l’attribut block de conversion ([DelegateProxy]) pour la valeur de retour de la méthode *.*. Veuillez entrer un bogue à http://bugzilla.xamarin.com.

Xamarin.iOS n’a pas pu trouver une méthode requise pendant l’exécution (pour convertir un délégué à un bloc).

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8012mt8012-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8012"/>MT8012 : Les DelegateProxyAttribute non valide pour la valeur de retour de la méthode *.*: Type_délégué a la valeur null. Veuillez entrer un bogue à http://bugzilla.xamarin.com.

L’attribut DelegateProxy pour la méthode en question n’est pas valide.

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8013mt8013-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-2-specifies-a-type-without-a-handler-field-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8013"/>MT8013 : Les DelegateProxyAttribute non valide pour la valeur de retour de la méthode *.*: Type_délégué ({{2}) spécifie un type de champ 'Handler'. Veuillez entrer un bogue à http://bugzilla.xamarin.com.

L’attribut DelegateProxy pour la méthode en question n’est pas valide.

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8014mt8014-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8014"/>MT8014 : Les DelegateProxyAttribute non valide pour la valeur de retour de la méthode *.*: Type_délégué le champ de 'Handler' ({{2}) est null. Veuillez entrer un bogue à http://bugzilla.xamarin.com.

L’attribut DelegateProxy pour la méthode en question n’est pas valide.

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8015mt8015-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-not-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8015"/>MT8015 : Les DelegateProxyAttribute non valide pour la valeur de retour de la méthode *.*: Type_délégué le champ de 'Handler' ({{2}) n’est pas un délégué, il s’agit d’un *. Veuillez entrer un bogue à http://bugzilla.xamarin.com.

L’attribut DelegateProxy pour la méthode en question n’est pas valide.

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8016mt8016-unable-to-convert-delegate-to-block-for-the-return-value-for-the-method--because-the-input-isnt-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8016"/>MT8016 : Impossible de convertir le délégué pour bloquer la valeur de retour pour la méthode *.*, car l’entrée n’est pas un délégué, il s’agit d’un *. Veuillez entrer un bogue à http://bugzilla.xamarin.com.

L’attribut DelegateProxy pour la méthode en question n’est pas valide.

Cela indique généralement un bogue dans Xamarin.iOS ; Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<!-- 8017 is used by mmp -->

### <a name="a-namemt8018mt8018-internal-consistency-error-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT8018"/>MT8018 : Erreur de cohérence interne. Veuillez archiver un rapport de bogue à http://bugzilla.xamarin.com.

Cela indique un bogue dans Xamarin.iOS. Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8019mt8019-could-not-find-the-assembly--in-the-loaded-assemblies"></a><a name="MT8019"/>MT8019 : Impossible de trouver l’assembly * dans les assemblys chargés.

Cela indique un bogue dans Xamarin.iOS. Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8020mt8020-could-not-find-the-module-with-metadatatoken--in-the-assembly-"></a><a name="MT8020"/>MT8020 : N’a pas pu trouver le module avec MetadataToken * dans l’assembly *.

Cela indique un bogue dans Xamarin.iOS. Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8021mt8021-unknown-implicit-token-type-"></a><a name="MT8021"/>MT8021 : Le type de jeton implicit inconnu : *.

Cela indique un bogue dans Xamarin.iOS. Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8022mt8022-expected-the-token-reference--to-be-a--but-its-a--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT8022"/>MT8022 : Attendu de la référence du jeton * soit un *, mais il s’agit d’un *. Veuillez archiver un rapport de bogue à http://bugzilla.xamarin.com.

Cela indique un bogue dans Xamarin.iOS. Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8023mt8023-an-instance-object-is-required-to-construct-a-closed-generic-method-for-the-open-generic-method--token-reference--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT8023"/>MT8023 : Un objet d’instance est requis pour construire une méthode générique fermée de la méthode générique ouverte : * (référence de jeton : *). Veuillez archiver un rapport de bogue à http://bugzilla.xamarin.com.

Cela indique un bogue dans Xamarin.iOS. Veuillez entrer un bogue sur [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).
