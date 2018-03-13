---
title: "Conseils de dépannage"
ms.topic: article
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 145c8507ca5ebea6197fa8827b93f58fbc9bb078
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="troubleshooting-tips"></a>Conseils de dépannage


## <a name="getting-diagnostic-information"></a>Obtention des informations de Diagnostic

Xamarin.Android a quelques endroits à rechercher lors du suivi des bogues différents.
Elles incluent notamment :

1.  Sortie de diagnostic MSBuild.
2.  Journaux de déploiement de périphérique.
3.  Sortie du journal de débogage Android.


<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>Diagnostic de sortie MSBuild

MSBuild diagnostic contiennent des informations supplémentaires relatives à la création de package et peut contenir des informations de déploiement de package.

Pour activer la sortie MSBuild de diagnostic dans Visual Studio :

1.  Cliquez sur **Outils > Options...**
2.  Dans l’arborescence à gauche, sélectionnez **projets et Solutions > Générer et exécuter**
3.  Dans le volet droit, définissez la liste déroulante de MSBuild build sortie commentaires pour le Diagnostic
4.  Cliquez sur **OK**.
5.  Nettoyez et regénérez votre package.
6.  Sortie de diagnostic est visible dans le volet de sortie.


Pour activer un diagnostic de sortie de MSBuild dans Visual Studio pour Mac/OS x :

1.  Cliquez sur **Visual Studio pour Mac > Préférences...**
2.  Dans l’arborescence à gauche, sélectionnez **projets > Générer**
3.  Dans le volet droit, définissez le journal de détail liste déroulante pour le Diagnostic
4.  Cliquez sur **OK**.
5.  Redémarrer Visual Studio pour Mac
6.  Nettoyez et regénérez votre package.
7.  Sortie de diagnostic est visible dans la zone d’erreurs (**vue > remplit > erreurs** ), en cliquant sur le bouton de sortie de génération.




## <a name="device-deployment-logs"></a>Journaux de déploiement de périphérique

Pour activer la journalisation de déploiement de périphérique dans Visual Studio :

1.  **Outils > Options...**>
2.  Dans l’arborescence à gauche, sélectionnez **Xamarin > Paramètres Android**
3.  Dans le volet droit, activez la [X] **(écritures monodroid.log sur votre bureau) de la journalisation du débogage extension** case à cocher.
4.  Messages du journal sont écrites dans le fichier monodroid.log sur votre bureau.


Visual Studio pour Mac écrit toujours les journaux de déploiement de périphérique. Recherche les est légèrement plus difficile ; un *AndroidUtils* fichier journal est créé pour chaque jour + une fois un déploiement se produit, par exemple : **AndroidTools-2012-10-24_12-35-45.log**.

-  Sous Windows, les fichiers journaux sont écrits dans `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`.
-  Sur OS X, les fichiers journaux sont écrits dans `$HOME/Library/Logs/XamarinStudio-{VERSION}`.




## <a name="android-debug-log-output"></a>Sortie du journal débogage Android

Android écrira le nombre de messages à la [journal de débogage Android](~/android/deploy-test/debugging/android-debug-log.md).
Xamarin.Android utilise les propriétés système Android pour contrôler la génération de messages supplémentaires dans le journal de débogage Android. Propriétés système Android peuvent être définies via la *setprop* commande au sein de la [Android Debug Bridge (adb)](http://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

Propriétés système sont lues lors du démarrage du processus et doivent donc être défini avant que l’application est lancée ou l’application doit être redémarrée une fois les propriétés système sont modifiées.



### <a name="xamarinandroid-system-properties"></a>Propriétés système de Xamarin.Android

Xamarin.Android prend en charge les propriétés système suivantes :

-   *Debug.mono.Debug*: si une chaîne non vide, cela équivaut à `*mono-debug*`.

-   *Debug.mono.env*: séparées par un canal ('*|*') liste des variables d’environnement à exporter pendant le démarrage de l’application, *avant* mono a été initialisé. Cela permet de définir des variables d’environnement que la journalisation mono contrôle.

    - *Remarque*: étant donné que la valeur est '*|*'-séparés, la valeur doit avoir un niveau supplémentaire de guillemets, comme le \` *adb shell* \` commande va supprimer un jeu de guillemets doubles.

    - *Remarque*: les valeurs de propriété système Android peuvent comporter plus de 92 caractères.

    - Exemple :

            adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"

-   *Debug.mono.log*: séparées par une virgule («*,*') liste des composants qui doit imprimer les messages supplémentaires dans le journal de débogage Android. Par défaut, rien n’est définie. Composants :

    -   *tous les*: tous les messages d’impression
    -   *dans le catalogue global*: les messages relatifs à l’impression dans le catalogue global.
    -   *gref*: afficher les messages d’allocation et la désallocation de référence (faible, global).
    -   *lref*: afficher les messages d’allocation et la désallocation de référence locale.

    *Remarque*: il s’agit *extrêmement* détaillé. N’activez pas, sauf si cela est réellement nécessaire.

-   *Debug.mono.trace*: permet de définir la [mono--trace](http://docs.go-mono.com/?link=man%3amono(1)) `=PROPERTY_VALUE` paramètre.



## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Impossible de résoudre Xamarin.Android System.ValueTuple

Cette erreur se produit en raison d’une incompatibilité avec Visual Studio.

- **Mise à jour 1 de Visual Studio 2017** (version 15.1 ou antérieure) est compatible uniquement avec les **System.ValueTuple NuGet 4.3.0** (ou version antérieure).

- **Mise à jour 2 de Visual Studio 2017** (version 15,2 ou une version ultérieure) est compatible uniquement avec les **System.ValueTuple NuGet 4.3.1** (ou ultérieure).

Veuillez choisir System.ValueTuple NuGet correct qui correspond à votre installation de Visual Studio 2017.


## <a name="gc-messages"></a>Messages dans le catalogue global

Dans le catalogue global composant peuvent être affichés en définissant la propriété système debug.mono.log à une valeur qui contient le catalogue global.

Les messages dans le catalogue global sont générés à chaque fois que le garbage collector s’exécute et fournit des informations sur la quantité de travail le garbage collector a :

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

Des informations supplémentaires telles que les informations de minutage GC peuvent être générées en définissant le `MONO_LOG_LEVEL` variable d’environnement `debug`:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

Il en résulte dans les messages Mono supplémentaires (un grand nombre de), y compris ces trois conséquences :

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

Dans le `GC_BRIDGE` message, `num-objects` est le nombre d’objets de pont envisage de cette étape, et `num_hash_entries` est le nombre d’objets traités au cours de cet appel du code de pont.

Dans le `GC_MINOR` et `GC_MAJOR` messages, `total` est la quantité de temps pendant que le monde est suspendu (aucun threads n’exécutent), tandis que `bridge` est la quantité de temps dans le pont de code (qui porte sur la machine virtuelle Java) de traitement. Le monde est *pas* en pause pendant l’exécution de traitement du pont.

 *En général*, plus la valeur de `num_hash_entries`, le plus de temps que la `bridge` collections prendra et le plus grand le `total` sera de temps passé à collecter.



## <a name="global-reference-messages"></a>Messages de référence globale

Pour activer la référence globale loggig (GREF), journalisation, le *debug.mono.log* propriété système doit contenir *gref*, par exemple :

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android utilise des références globales Android pour fournir des mappages entre les instances de Java et les instances managées associées, comme lors de l’appel de méthode Java qu'une instance Java doit être fourni pour Java.

Malheureusement, les émulateurs Android autorisent uniquement les références de global 2000 existe à la fois. Matériel possède une quantité limite plus élevée de 52000 références globales. La limite inférieure peut être problématique lors de l’exécution des applications sur l’émulateur, par conséquent, en sachant *où* le provient de l’instance de peut être très utile.

 *Remarque*: le nombre de références global est interne à Xamarin.Android et n’est pas (et ne peut pas) incluent des références globales par les autres bibliothèques natives chargés dans le processus. Utiliser le nombre de références global comme une estimation.

```shell
I/monodroid-gref(12405): +g+ grefc 108 gwrefc 0 obj-handle 0x40517468/L -> new-handle 0x40517468/L from    at Java.Lang.Object.RegisterInstance(IJavaObject instance, IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object.SetHandle(IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object..ctor(IntPtr handle, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler, Boolean removable)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler)
I/monodroid-gref(12405):    at Android.App.Activity.RunOnUiThread(System.Action action)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.UseLotsOfMemory(Android.Widget.TextView textview)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.<OnCreate>m__3(System.Object o)
I/monodroid-gref(12405): handle 0x40517468; key_handle 0x40517468: Java Type: `mono/java/lang/RunnableImplementor`; MCW type: `Java.Lang.Thread+RunnableImplementor`
I/monodroid-gref(12405): Disposing handle 0x40517468
I/monodroid-gref(12405): -g- grefc 107 gwrefc 0 handle 0x40517468/L from    at Java.Lang.Object.Dispose(System.Object instance, IntPtr handle, IntPtr key_handle, JObjectRefType handle_type)
I/monodroid-gref(12405):    at Java.Lang.Object.Dispose()
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor.Run()
I/monodroid-gref(12405):    at Java.Lang.IRunnableInvoker.n_Run(IntPtr jnienv, IntPtr native__this)
I/monodroid-gref(12405):    at System.Object.c200fe6f-ac33-441b-a3a0-47659e3f6750(IntPtr , IntPtr )
I/monodroid-gref(27679): +w+ grefc 1916 gwrefc 296 obj-handle 0x406b2b98/G -> new-handle 0xde68f4bf/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1915 gwrefc 294 handle 0xde691aaf/W from take_global_ref_jni
```

Il existe quatre messages des conséquences :

-  La création de référence globale : ce sont les lignes qui commencent par *+ g +* et fournit une trace de pile pour le chemin de code de création.
-  Destruction de référence globale : ce sont les lignes qui commencent par *- g-* et peut fournir une trace de pile pour le chemin d’accès du code suppression de la référence globale. Si le garbage collector supprime le gref, aucune trace de la pile ne sera fournie.
-  Faible globales font référence à la création : ce sont les lignes qui commencent par *+ w +* .
-  Faible global référence destruction : il s’agit des lignes qui commencent par *-w-* .


Dans tous les messages, le *grefc* valeur est le nombre de références globales qui Xamarin.Android a créé, tandis que la *grefwc* valeur correspond au nombre de références faibles globales qui Xamarin.Android a créé. Le *gérer* ou *obj-handle* valeur est la valeur du handle JNI et le caractère situé après le '  */* » est le type de valeur du handle :   */l* pour référence locale, */G* pour les références globales, et *Wn* pour les références faibles globales.

Dans le cadre du processus de catalogue global, les références globales (+ g +) sont converties en références globaux faibles (à l’origine a + w + et - g-), un catalogue global Java côté est lancé, puis la référence globale faible est vérifiée pour voir si elles ont été collectées. Si elle est toujours actif, un nouveau gref est créée autour de la référence faible (+ g +, -w-), sinon la destruction de la référence faible (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Instance Java est créé et encapsulé par un MCW

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>Un catalogue global est en cours d’exécution...

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>Objet est toujours actif, en tant que handle ! = null
## <a name="wref-turned-back-into-a-gref"></a>wref retournées dans un gref

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>Objet est inactif, en tant que handle == null
## <a name="wref-is-freed-no-new-gref-created"></a>wref est libéré, aucune nouvelle gref créé

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

Il existe une ride « intéressante » ici : sur la cible est en cours d’exécution Android antérieure à 4.0, la valeur de gref est égale à l’adresse de l’objet Java dans la mémoire de l’exécution Android. (Autrement dit, le catalogue global est un non mobiles classique, collecteur et il est distribuant des références directes aux objets.) Par conséquent, après un + g + + w +, - g-, + g +, - w-séquence, la gref résultant aura la même valeur que la valeur gref d’origine. Cela rend grepping journaux assez simple.

Android 4.0, toutefois, a un collecteur de déplacement et n’est plus transmet les références directes au runtime Android objets ordinateur virtuel. Par conséquent, après un + g + + w +, - g-, + g +, - w-séquence, la valeur gref *seront différents*. Si l’objet survit à plusieurs catalogues globaux, il passe par plusieurs valeurs gref, rend plus difficile de déterminer où une instance a été réellement allouée à partir de.

### <a name="querying-programmatically"></a>Interrogation par programmation

Vous pouvez interroger les nombres de GREF et de WREF en interrogeant le `JniRuntime` objet.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` -Nombre de références global

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` -Nombre de références faible



## <a name="offline-activation"></a>Activation hors connexion

Si vous n’êtes pas parvenu à activer Xamarin.Android sur Windows ou Impossible d’installer la version complète de Xamarin.Android sur Mac OS X, consultez la [Activation hors connexion](~/android/get-started/installation/index.md) page.



## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>Ne peut pas mettre à niveau vers contient/entreprise à partir de la version d’évaluation de compte

Si vous acheté récemment Xamarin.Android et démarrée précédemment une version d’évaluation de Xamarin.Android, vous devrez peut-être effectuer les étapes suivantes pour obtenir cette modification de la licence sélectionnée par Visual Studio pour Mac ou Visual Studio.

-  Fermez Visual Studio pour Mac/Visual Studio
-  Supprimez tous les fichiers ~/Library/MonoAndroid sur Mac ou %PROGRAMDATA%\Mono Android\License\ pour Windows
-  Ouvrez à nouveau Visual Studio pour Mac/Visual Studio et générer un projet Xamarin.Android


Cela doit disposer de haut et en cours d’exécution. Si vous continuez à rencontrer des problèmes, vous pouvez éventuellement essayer un [Activation hors connexion](~/android/get-started/installation/index.md) pour effectuer l’activation de votre station de travail.



## <a name="receiving-activation-incomplete-error-message"></a>Réception de « Message d’erreur incomplet d’Activation

Ce problème peut se produire lors de l’utilisation de Xamarin.Android pour Visual Studio. Pour résoudre ce problème, envoyez les journaux à partir de l’emplacement suivant pour  *contact@xamarin.com* .

-  Emplacement du journal : **%LocalAppData%\\Xamarin\\journaux**




## <a name="receiving-error-retrieving-update-information-error-message"></a>Message d’erreur « Erreur de récupération des informations de mise à jour »

De temps à autre, une mise à jour échoue avec l’erreur suivante, ce qui se produit souvent lors de la vérification des mises à jour :

La plupart du temps, cette erreur peut être résolue simplement en vous connectant en dehors de votre compte Xamarin, puis de nouveau la journalisation dans.

Pour ce faire, Veuillez trouver votre plateforme de choix ci-dessous et suivez les étapes :

**Sur Mac :**
1. Ouvrez Visual Studio pour Mac
2. Sélectionnez Visual Studio pour Mac > compte...
3. Cliquez sur se déconnecter
4. Cliquez sur se connecter
5. Entrez vos informations d’identification
6. Vérification des mises à jour

**Sur les PC à l’aide de Visual Studio :**
1. Ouvrir Visual Studio
2. Sélectionnez Outils > compte Xamarin
3. Cliquez sur se déconnecter
4. Cliquez sur se connecter
5. Entrez vos informations d’identification
6. Vérification des mises à jour

Si ce message d’erreur continue d’apparaître, envoyez un e-mail  **contact@xamarin.com** .




## <a name="android-debug-logs"></a>Journaux de débogage Android

Le [les journaux de débogage Android](~/android/deploy-test/debugging/android-debug-log.md) peut fournir un contexte supplémentaire concernant des erreurs d’exécution que vous voyez.



## <a name="floating-point-performance-is-terrible"></a>Les performances à virgule flottante sont horribles !

Vous pouvez également « mon application s’exécute 10 fois plus rapides avec la version Debug qu’avec la version Release ! »

Xamarin.Android prend en charge plusieurs périphériques ABIs : *armeabi*, *armeabi-v7a*, et *x86*. APPAREIL ABIs peuvent être spécifiés dans **propriétés du projet > onglet Application > architectures prises en charge**.

Les versions Debug utilisent un package Android fournit tous les ABIs et donc utilisera l’ABI plus rapide pour l’appareil cible.

Versions Release contiendra uniquement les ABIs sélectionnés dans l’onglet Propriétés du projet. Plusieurs peut être sélectionné.

*armeabi* est la valeur par défaut ABI et a la plus grande prise en charge de périphérique. *Toutefois*, armeabi ne prend en charge les appareils multiprocesseur et à virgule flottante, matériel amont autres choses. Par conséquent, les applications à l’aide de l’exécution de la version armeabi seront liées à un seul cœur et utilisera une implémentation soft-float. Ces deux éléments peuvent contribuer à la baisse des performances de votre application.

Si votre application nécessite la performance acceptable à virgule flottante (par exemple, les jeux), vous devez activer la *armeabi-v7a* ABI. Vous pouvez souhaiter prennent uniquement en charge la *armeabi-v7a* runtime, bien que cela signifie qu’anciens appareils qui prennent uniquement en charge *armeabi* ne pourra pas exécuter votre application.



## <a name="could-not-locate-android-sdk"></a>Kit de développement logiciel Android est introuvable.

Il n’y a 2 téléchargements disponibles à partir de Google pour le Kit de développement logiciel Android pour Windows.
Si vous choisissez le programme d’installation .exe, il écrit des clés de Registre qui indiquent à Xamarin.Android où il a été installé. Si vous choisissez le fichier .zip et décompressez vous-même, Xamarin.Android ne sait pas où rechercher le Kit de développement. Vous savez Xamarin.Android où le Kit de développement dans Visual Studio en accédant à **Outils > Options > Xamarin > Paramètres Android**:

[![Emplacement du SDK Android dans Xamarin Android paramètres](troubleshooting-images/01a.png)](troubleshooting-images/01a.png#lightbox)



## <a name="ide-does-not-display-target-device"></a>IDE n’affiche pas le périphérique cible

Parfois vous va tenter de déployer votre application sur un périphérique, mais le périphérique que vous voulez procéder au déploiement n’est pas indiqué dans la boîte de dialogue Sélectionner l’unité. Cela peut se produire quand le Android Debug Bridge décide d’accéder en vacances.

Pour diagnostiquer ce problème, recherchez le [adb programme](~/android/deploy-test/debugging/android-debug-log.md), puis exécutez :

```shell
adb devices
```

Si votre appareil n’est pas présent, vous devez redémarrer le serveur Android Debug Bridge afin que votre appareil peut être trouvé :

```shell
adb kill-server
adb start-server
```

Peut empêcher le logiciel de synchronisation de HTC **adb start-server** de fonctionner correctement. Si le **adb start-server** commande ne s’imprime pas le port utilisé par elle démarre sur, quittez le logiciel de synchronisation de HTC et essayez de redémarrer le serveur adb.


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>L’exécutable de la tâche spécifiée « keytool » n’a pas pu être exécuté.

Cela signifie que le chemin d’accès ne contient pas le répertoire où se trouve le répertoire bin de du Kit de développement Java. Vérifiez que vous avez suivi ces étapes à partir de la [Installation](~/android/get-started/installation/index.md) guide.


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe ou aresgen.exe s’est arrêté avec le code 1

Pour vous aider à résoudre ce problème, accédez à Visual Studio et modifier le niveau de détail de MSBuild pour ce faire, sélectionnez : **Outils > Options > projet** et **Solutions > Générer** et **exécuter > Commentaires de la sortie générer MSBuild Project** et définir cette valeur sur **Normal**.

Reconstruire, puis vérifiez le volet de sortie de Visual Studio, qui doit contenir le message d’erreur.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>Ne contient pas suffisamment d’espace libre sur le périphérique pour déployer le package

Cela se produit lorsque vous ne démarrez l’émulateur à partir de Visual Studio. Lors du démarrage de l’émulateur en dehors de Visual Studio, vous devez passer le `-partition-size 512` options, par exemple,

```shell
emulator -partition-size 512 -avd MonoDroid
```

Assurez-vous que vous utilisez le nom correct de simulateur, c'est-à-dire [le nom que vous avez utilisé lors de la configuration du simulateur](~/android/get-started/installation/windows.md#device).


## <a name="installfailedinvalidapk-when-installing-a-package"></a>INSTALLER\_n’a pas pu\_non valide\_APK lors de l’installation d’un package

Les noms de package Android *doit* contient un point («*.*'). Modifier le nom de votre package afin qu’il contienne une période.

-   Dans Visual Studio :
    -   Cliquez avec le bouton droit sur votre projet > Propriétés
    -   Cliquez sur l’onglet manifeste Android sur la gauche.
    -   Mettre à jour le champ de nom de Package.
        -   Si vous voyez le message &ldquo;AndroidManifest.xml non trouvé. Cliquez pour ajouter une. &rdquo;ensuite mettre à jour le champ de nom de Package et cliquez sur le lien.
-   Dans Visual Studio pour Mac :
    -   Cliquez avec le bouton droit sur votre projet > Options.
    -   Accédez à la Build / section Application Android.
    -   Modifiez le champ de nom de Package de contenir un '.'.




## <a name="installfailedmissingsharedlibrary-when-installing-a-package"></a>INSTALLER\_n’a pas pu\_manquant\_SHARED\_bibliothèque lors de l’installation d’un package

Une bibliothèque « partagée » dans ce contexte est *pas* une bibliothèque partagée native (*libfoo.so*) de fichiers ; il est à la place une bibliothèque qui doit être installée séparément sur le périphérique cible, tels que Google Maps.

Le package Android spécifie les bibliothèques partagées sont requises avec le `<uses-library/>` élément. Si un *requis* bibliothèque n’est pas présente sur l’appareil cible (par exemple, `//uses-library/@android:required` est *true*, qui est la valeur par défaut), puis l’installation du package échoue avec *installer\_ Échec de\_manquant\_SHARED\_bibliothèque*.

Pour déterminer les bibliothèques partagées sont requis, voir la *généré*
**AndroidManifest.xml** fichier (par exemple, **obj\\déboguer\\android \\AndroidManifest.xml**) et recherchez le `<uses-library/>` éléments. `<uses-library/>` éléments peuvent être ajoutés manuellement de votre projet **propriétés\\AndroidManifest.xml** fichier et via le [UsesLibraryAttribute des attributs personnalisés](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/).

Par exemple, ajoutez une référence d’assembly à *Mono.Android.GoogleMaps.dll* ajoute implicitement un `<uses-library/>` pour la bibliothèque partagée Google Maps.



## <a name="installfailedupdateincompatible-when-installing-a-package"></a>INSTALLER\_n’a pas pu\_mise à jour\_incompatibles lors de l’installation d’un package

Les packages Android ont trois conditions :

-   Ils doivent contenir un '.' (consultez les entrées précédentes)
-   Ils doivent avoir un nom de package de chaîne unique (par conséquent, la convention inverse-tld vue dans les noms d’application Android, par exemple, com.android.chrome pour l’application de Chrome)
-   Lors de la mise à niveau de packages, le package doit avoir la même clé de signature.

Par conséquent, imaginons le scénario :

1.  Générer et de déployer votre application comme une application de débogage
2.  Vous modifiez la clé de signature, par exemple, à utiliser comme une application de la mise en production (ou parce que vous n’aimez pas la valeur par défaut fournie par débogue la clé de signature)
3.  Vous installez votre application sans le supprimer tout d’abord, par exemple Déboguer > Exécuter sans débogage dans Visual Studio


Dans ce cas, l’installation du package échoue avec une installation\_n’a pas pu\_mise à jour\_erreur INCOMPATIBLE, car le nom du package n’a pas changé pendant la signature de la clé a. Le [journal de débogage Android](~/android/deploy-test/debugging/android-debug-log.md) contient également un message similaire à :

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

Pour corriger cette erreur, supprimez complètement l’application à partir de votre appareil avant de réinstaller.


## <a name="installfaileduidchanged-when-installing-a-package"></a>INSTALLER\_n’a pas pu\_UID\_modifié lors de l’installation d’un package

Lorsqu’un package Android est installé, il est attribué un *id utilisateur* (UID).
*Parfois*, pour des raisons actuellement inconnus, lors de l’installation via une application déjà installée, l’installation échoue avec `INSTALL_FAILED_UID_CHANGED`:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

Pour contourner ce problème, *complètement désinstaller* package Android par l’installation de l’application à partir de l’interface graphique utilisateur de la cible Android, ou à l’aide de `adb`:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**N’utilisez pas** `adb uninstall -k`, comme ceci va *conserver* données d’application et ainsi conserver l’UID en conflit sur l’appareil cible.



## <a name="release-apps-fail-to-launch-on-device"></a>Les applications de mise en production échouent du lancement sur l’appareil

La sortie du journal de débogage Android contient un message semblable à :

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

Dans ce cas, il existe deux causes possibles :

1.  Fichier .apk ne fournit pas un ABI qui prend en charge de l’appareil cible.
    Par exemple, le .apk contient uniquement des fichiers binaires d’armeabi-v7a, et l’appareil cible prend uniquement en charge armeabi.

2.  Un [bogue Android](http://code.google.com/p/android/issues/detail?id=21670). Si c’est le cas, désinstallez l’application, cross vos doigts et réinstallez l’application.

Pour résoudre un (1), modifier les Options/propriétés du projet et [ajouter la prise en charge de l’ABI requis à la liste de prise en charge de ABIs](~/android/app-fundamentals/cpu-architectures.md). Pour déterminer quels ABI, vous devez ajouter, exécutez la commande adb suivante sur votre périphérique cible :

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

La sortie contient le réplica principal (et secondaire facultatif) ABIs.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>La propriété OutPath n’est pas définie pour le projet &ldquo;MyApp.csproj&rdquo;

Cela signifie généralement que vous avez un ordinateur HP et la variable d’environnement &ldquo;plateforme&rdquo; a été défini sur une valeur CN ou HPD. Il est en conflit avec la propriété de plateforme MSBuild est généralement définie sur &ldquo;Any CPU&rdquo; ou &ldquo;x86&rdquo;. Vous devez supprimer cette variable d’environnement à partir de votre ordinateur avant que MSBuild puisse fonctionner :

-   Panneau de configuration > système > Avancé > Variables d’environnement

Redémarrez Visual Studio ou Visual Studio pour Mac et recommencez à reconstruire. Éléments devraient désormais fonctionner comme prévu.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException : mono.android.runtime.JavaObject ne peut pas être converti en cours...

Xamarin.Android 4.x ne marshaler correctement correctement les types génériques imbriqués. Par exemple, considérez le C suivant\# à l’aide de code [SimpleExpandableListAdapter](https://developer.xamarin.com/api/type/Android.Widget.SimpleExpandableListAdapter/):


```csharp
// BAD CODE; DO NOT USE
var groupData = new List<IDictionary<string, object>> () {
        new Dictionary<string, object> {
                { "NAME", "Group 1" },
                { "IS_EVEN", "This group is odd" },
        },
};
var childData = new List<IList<IDictionary<string, object>>> () {
        new List<IDictionary<string, object>> {
                new Dictionary<string, object> {
                        { "NAME", "Child 1" },
                        { "IS_EVEN", "This group is odd" },
                },
        },
};
mAdapter = new SimpleExpandableListAdapter (
        this,
        groupData,
        Android.Resource.Layout.SimpleExpandableListItem1,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
        childData,
        Android.Resource.Layout.SimpleExpandableListItem2,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
);
```


Le problème est que Xamarin.Android incorrectement marshale les types génériques imbriqués. Le `List<IDictionary<string, object>>` est marshalé en un [java.lang.ArrrayList](https://developer.xamarin.com/api/type/Java.Util.ArrayList/), mais la `ArrayList` est contenant `mono.android.runtime.JavaObject` instances (qui référence le `Dictionary<string, object>` instances) au lieu d’un élément qui implémente [java.util.Map](https://developer.xamarin.com/api/type/Java.Util.IMap/), se traduisant par l’exception suivante :

```shell
E/AndroidRuntime( 2991): FATAL EXCEPTION: main
E/AndroidRuntime( 2991): java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to java.util.Map
E/AndroidRuntime( 2991):        at android.widget.SimpleExpandableListAdapter.getGroupView(SimpleExpandableListAdapter.java:278)
E/AndroidRuntime( 2991):        at android.widget.ExpandableListConnector.getView(ExpandableListConnector.java:446)
E/AndroidRuntime( 2991):        at android.widget.AbsListView.obtainView(AbsListView.java:2271)
E/AndroidRuntime( 2991):        at android.widget.ListView.makeAndAddView(ListView.java:1769)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillDown(ListView.java:672)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillFromTop(ListView.java:733)
E/AndroidRuntime( 2991):        at android.widget.ListView.layoutChildren(ListView.java:1622)
```

La solution de contournement consiste à utiliser les [des types de Collection de Java](~/android/internals/api-design.md) au lieu du `System.Collections.Generic` types pour le &ldquo;interne&rdquo; types. Cela entraîne des types Java appropriés lors du marshaling des instances. (Le code suivant est plus complexe que nécessaire afin de réduire les durées de vie gref. Il peut être simplifié à la modification du code d’origine via `s/List/JavaList/g` et `s/Dictionary/JavaDictionary/g` si les durées de vie gref ne sont pas un problème.)

```csharp
// insert good code here
using (var groupData = new JavaList<IDictionary<string, object>> ()) {
    using (var groupEntry = new JavaDictionary<string, object> ()) {
        groupEntry.Add ("NAME", "Group 1");
        groupEntry.Add ("IS_EVEN", "This group is odd");
        groupData.Add (groupEntry);
    }
    using (var childData = new JavaList<IList<IDictionary<string, object>>> ()) {
        using (var childEntry = new JavaList<IDictionary<string, object>> ())
        using (var childEntryDict = new JavaDictionary<string, object> ()) {
            childEntryDict.Add ("NAME", "Child 1");
            childEntryDict.Add ("IS_EVEN", "This child is odd.");
            childEntry.Add (childEntryDict);
            childData.Add (childEntry);
        }
        mAdapter = new SimpleExpandableListAdapter (
            this,
            groupData,
            Android.Resource.Layout.SimpleExpandableListItem1,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
            childData,
            Android.Resource.Layout.SimpleExpandableListItem2,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
        );
    }
}
```

[Ce problème sera résolu dans une prochaine version](https://bugzilla.xamarin.com/show_bug.cgi?id=5401).


## <a name="unexpected-nullreferenceexceptions"></a>Exceptions NullReferenceException inattendue

Parfois le [journal de débogage Android](~/android/deploy-test/debugging/android-debug-log.md) fera mention NullReferenceExceptions qui &ldquo;ne peut pas se produire,&rdquo; ou provenir Mono pour le code de runtime de Android peu de temps avant l’application de matrices :

```shell
E/mono(15202): Unhandled Exception: System.NullReferenceException: Object reference not set to an instance of an object
E/mono(15202):   at Java.Lang.Object.GetObject (IntPtr handle, System.Type type, Boolean owned)
E/mono(15202):   at Java.Lang.Object._GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Java.Lang.Object.GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Android.Views.View+IOnTouchListenerAdapter.n_OnTouch_Landroid_view_View_Landroid_view_MotionEvent_(IntPtr jnienv, IntPtr native__this, IntPtr native_v, IntPtr native_e)
E/mono(15202):   at (wrapper dynamic-method) object:b039cbb0-15e9-4f47-87ce-442060701362 (intptr,intptr,intptr,intptr)
```

ou

```shell
E/mono    ( 4176): Unhandled Exception:
E/mono    ( 4176): System.NullReferenceException: Object reference not set to an instance of an object
E/mono    ( 4176): at Android.Runtime.JNIEnv.NewString (string)
E/mono    ( 4176): at Android.Util.Log.Info (string,string)
```

Cela peut se produire lorsque le runtime Android décide d’annuler le processus, qui peut se produire pour différentes raisons, y compris en appuyant sur la limite de la cible GREF ou faire quelque chose &ldquo;incorrect&rdquo; avec JNI.

Pour savoir si c’est le cas, vérifiez le journal de débogage Android pour un message à partir de votre processus similaire à :

```shell
E/dalvikvm(  123): VM aborting
```


## <a name="abort-due-to-global-reference-exhaustion"></a>Abandon en raison de l’épuisement de référence globale

Couche JNI du runtime Android prend uniquement en charge un nombre limité de références d’objet JNI soit valide à un moment donné dans le temps. Lorsque cette limite est dépassée, panne.

Le GREF (*référence globale*) limite est de références 2000 dans l’émulateur, et fait référence à ~ 52000 sur du matériel.

Vous savez que vous commencez à créer trop GREFs lorsque vous consultez des messages tels que cela dans le journal de débogage Android :

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

Lorsque vous atteignez la limite GREF, un message tel que le texte suivant s’affiche :

```shell
D/dalvikvm(  602): GREF has increased to 2001
W/dalvikvm(  602): Last 10 entries in JNI global reference table:
W/dalvikvm(  602):  1991: 0x4057eff8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1992: 0x4057f010 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1993: 0x40698e70 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1994: 0x40698e88 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1995: 0x40698ea0 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1996: 0x406981f0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1997: 0x40698208 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1998: 0x40698220 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1999: 0x406956a8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  2000: 0x406956c0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602): JNI global reference table summary (2001 entries):
W/dalvikvm(  602):    51 of Ljava/lang/Class; 164B (41 unique)
W/dalvikvm(  602):    46 of Ljava/lang/Class; 188B (17 unique)
W/dalvikvm(  602):     6 of Ljava/lang/Class; 212B (6 unique)
W/dalvikvm(  602):    11 of Ljava/lang/Class; 236B (7 unique)
W/dalvikvm(  602):     3 of Ljava/lang/Class; 260B (3 unique)
W/dalvikvm(  602):     4 of Ljava/lang/Class; 284B (2 unique)
W/dalvikvm(  602):     8 of Ljava/lang/Class; 308B (6 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 316B
W/dalvikvm(  602):     4 of Ljava/lang/Class; 332B (3 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 356B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 380B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 452B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 476B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 500B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 548B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 572B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 596B (2 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 692B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 956B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1004B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1148B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 1172B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1316B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3452B
W/dalvikvm(  602):     1 of Ljava/lang/String; 28B
W/dalvikvm(  602):     2 of Ldalvik/system/VMRuntime; 12B (1 unique)
W/dalvikvm(  602):    10 of Ljava/lang/ref/WeakReference; 28B (10 unique)
W/dalvikvm(  602):     1 of Ldalvik/system/PathClassLoader; 44B
W/dalvikvm(  602):  1553 of Landroid/graphics/Point; 20B (1553 unique)
W/dalvikvm(  602):   261 of Landroid/graphics/Point; 28B (261 unique)
W/dalvikvm(  602):     1 of Landroid/view/MotionEvent; 100B
W/dalvikvm(  602):     1 of Landroid/app/ActivityThread$ApplicationThread; 28B
W/dalvikvm(  602):     1 of Landroid/content/ContentProvider$Transport; 28B
W/dalvikvm(  602):     1 of Landroid/view/Surface$CompatibleCanvas; 44B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$ControlledInputConnectionWrapper; 36B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$1; 12B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$W; 28B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/view/accessibility/AccessibilityManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout$LayoutParams; 44B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout; 332B
W/dalvikvm(  602):     2 of Lorg/apache/harmony/xnet/provider/jsse/TrustManagerImpl; 28B (1 unique)
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$MyWindow; 36B
W/dalvikvm(  602):     1 of Ltouchtest/RenderThread; 92B
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$3; 12B
W/dalvikvm(  602):     1 of Ltouchtest/DrawingView; 412B
W/dalvikvm(  602):     1 of Ltouchtest/Activity1; 180B
W/dalvikvm(  602): Memory held directly by tracked refs is 75624 bytes
E/dalvikvm(  602): Excessive JNI global references (2001)
E/dalvikvm(  602): VM aborting
```


Dans l’exemple ci-dessus (qui, en fait, provient de [bogue 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)) le problème est que Android.Graphics.Point trop d’instances sont en cours de création, consultez [commentaire \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2) pour obtenir la liste des correctifs pour Ce bogue particulier.

En règle générale, une solution utile consiste à trouver le type a trop d’instances allouées &ndash; Android.Graphics.Point dans le fichier de vidage ci-dessus &ndash; recherchez où ils sont créés correctement dans votre code source et la suppression d’eux (afin que leurs Durée de vie Java-Object est réduite). Cela n’est pas toujours approprié (\#685215 est multithread, la solution triviale évite l’appel Dispose), mais c’est la première chose à prendre en compte.

Vous pouvez activer [GREF journalisation](~/android/troubleshooting/index.md) lorsque GREFs sont créés et leur nombre.


## <a name="abort-due-to-jni-type-mismatch"></a>Abandon en raison d’une incompatibilité de type JNI

Si vous écriture code JNI, il est possible que les types ne correspondront pas correctement, par exemple, si vous tentez d’appeler `java.lang.Runnable.run` sur un type qui n’implémente pas `java.lang.Runnable`. Lorsque cela se produit, il y aura un message semblable à celle-ci dans le journal de débogage Android :

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>Prise en charge de Code dynamique

### <a name="dynamic-code-does-not-compile"></a>Code dynamique ne se compile pas

Pour utiliser les C\# dynamique dans votre application ou une bibliothèque, vous devez ajouter System.Core.dll, Microsoft.CSharp.dll et Mono.CSharp.dll à votre projet.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>Dans la version Release, MissingMethodException se produit pour le code dynamique au moment de l’exécution.

-   Il est probable que votre projet d’application n’a pas de référence à System.Core.dll, Microsoft.CSharp.dll ou Mono.CSharp.dll. Assurez-vous que ces assemblys sont référencés.

    -   Gardez à l’esprit que le code dynamique toujours les coûts. Si vous avez besoin d’un code efficace, envisagez d’utiliser ne pas de code dynamique.

-   Dans la première version préliminaire, ces assemblys ont été exclus, sauf si les types dans chaque assembly sont explicitement utilisés par le code d’application. Consultez les rubriques suivantes pour une solution de contournement : [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>Les projets générés avec les pannes AOA + LLVM sur x86 périphériques

Lorsque vous déployez une application générée avec [AOA + LLVM](~/android/deploy-test/release-prep/index.md) sur les périphériques x86, vous pouvez voir un message d’erreur semblable au suivant :

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

Il s’agit d’un problème connu comme indiqué dans [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111). La solution de contournement consiste à désactiver les LLVM.
