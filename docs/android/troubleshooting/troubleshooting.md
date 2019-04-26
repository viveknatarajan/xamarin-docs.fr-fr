---
title: Conseils de dépannage
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: b2f11bd09e1b1b3fd7af29a026229494a081ad11
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61085038"
---
# <a name="troubleshooting-tips"></a>Conseils de dépannage


## <a name="getting-diagnostic-information"></a>Obtention d’informations de Diagnostic

Xamarin.Android a quelques endroits à rechercher lors du suivi des bogues différents.
Elles incluent notamment :

1.  Sortie MSBuild de diagnostic.
2.  Journaux de déploiement de périphérique.
3.  Sortie du journal de débogage Android.


<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>Sortie MSBuild de diagnostic

MSBuild de diagnostic peut contenir des informations supplémentaires relatives à la création de package et peut contenir des informations de déploiement de package.

Pour activer la sortie MSBuild de diagnostic dans Visual Studio :

1.  Cliquez sur **Outils > Options...**
2.  Dans l’arborescence à gauche, sélectionnez **projets et Solutions > Générer et exécuter**
3.  Dans le volet de droite, la valeur est la liste déroulante de MSBuild build sortie commentaires Diagnostic
4.  Cliquez sur **OK**.
5.  Nettoyez et regénérez votre package.
6.  Sortie de diagnostic est visible dans le panneau Sortie.


Pour activer la sortie MSBuild de diagnostic dans Visual Studio pour Mac/OS x :

1.  Cliquez sur **Visual Studio pour Mac > Préférences...**
2.  Dans l’arborescence à gauche, sélectionnez **projets > Générer**
3.  Dans le volet de droite, la valeur est le niveau de détail du journal déroulante Diagnostic
4.  Cliquez sur **OK**.
5.  Redémarrer Visual Studio pour Mac
6.  Nettoyez et regénérez votre package.
7.  Sortie de diagnostic est visible dans le panneau erreurs (**Afficher > panneaux > erreurs** ), en cliquant sur le bouton de la sortie de génération.




## <a name="device-deployment-logs"></a>Journaux de déploiement de périphérique

Pour activer la journalisation de déploiement de périphérique dans Visual Studio :

1.  **Outils > Options...**>
2.  Dans l’arborescence à gauche, sélectionnez **Xamarin > Paramètres Android**
3.  Dans le volet de droite, activez le [X] **journalisation du débogage extension (écrit monodroid.log sur votre ordinateur de bureau)** case à cocher.
4.  Messages du journal sont écrits dans le fichier monodroid.log sur votre bureau.


Visual Studio pour Mac écrit toujours les journaux de déploiement de périphérique. Recherche les est légèrement plus difficile ; un *AndroidUtils* fichier journal est créé pour chaque jour + le temps qui produit un déploiement, par exemple : **AndroidTools-2012-10-24_12-35-45.log**.

-  Sur Windows, les fichiers journaux sont écrits dans `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`.
-  Sur OS X, les fichiers journaux sont écrits dans `$HOME/Library/Logs/XamarinStudio-{VERSION}`.




## <a name="android-debug-log-output"></a>Sortie de journal de débogage Android

Android écrira le nombre de messages à la [journal de débogage Android](~/android/deploy-test/debugging/android-debug-log.md).
Xamarin.Android utilise les propriétés système Android pour contrôler la génération de messages supplémentaires dans le journal de débogage Android. Les propriétés système Android peuvent être définies via la *setprop* commande au sein de la [Android Debug Bridge (adb)](https://developer.android.com/guide/developing/tools/adb.html):

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

Propriétés système sont lus pendant le démarrage du processus et doivent donc être défini avant que l’application est lancée ou l’application doit être redémarrée après la modification des propriétés du système.



### <a name="xamarinandroid-system-properties"></a>Propriétés système de Xamarin.Android

Xamarin.Android prend en charge les propriétés système suivantes :

-   *debug.mono.debug*: Si une chaîne non vide, cela équivaut à `*mono-debug*`.

-   *debug.mono.env*: Séparées par une barre verticale (\« *|*') liste des variables d’environnement à exporter pendant le démarrage de l’application, *avant* mono a été initialisé. Cela permet de définir des variables d’environnement que la journalisation mono contrôle.

    - *Remarque* : Dans la mesure où la valeur est *|*'-séparés, la valeur doit avoir un niveau supplémentaire de guillemets, comme le \`*interpréteur de commandes adb*\` commande supprime un jeu de guillemets.

    - *Remarque* : Les valeurs de propriété système Android peuvent excéder 92 caractères.

    - Exemple :

            adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"

-   *debug.mono.log*: Une virgule (*,*') liste des composants qui doit afficher des messages supplémentaires dans le journal de débogage Android. Par défaut, rien n’est défini. Composants :

    -   *tous les*: Tous les messages d’impression
    -   *GC*: Imprimer les messages associés au GC.
    -   *gref*: Imprimer les messages d’allocation et la désallocation de référence (faible, global).
    -   *lref*: Imprimer les messages d’allocation et la désallocation de référence locale.

    *Remarque*: il s’agit de *extrêmement* verbose. N’activez pas, sauf si vous devez vraiment.

-   *Debug.mono.trace*: Permet de définir le [mono--trace](http://docs.go-mono.com/?link=man%3amono(1)) `=PROPERTY_VALUE` paramètre.



## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android ne peut pas résoudre System.ValueTuple

Cette erreur se produit en raison d’une incompatibilité avec Visual Studio.

- **Visual Studio 2017 Update 1** (version 15.1 ou une version antérieure) est compatible uniquement avec les **System.ValueTuple NuGet 4.3.0** (ou une version antérieure).

- **Visual Studio 2017 Update 2** (version 15.2 ou une version ultérieure) est compatible uniquement avec les **System.ValueTuple NuGet 4.3.1** (ou version ultérieure).

Choisissez le package System.ValueTuple NuGet correct qui correspond à votre installation de Visual Studio 2017.


## <a name="gc-messages"></a>Messages de GC

GC composant peuvent être affichés en définissant la propriété système debug.mono.log sur une valeur qui contient le catalogue global.

Messages de catalogue global sont générés chaque fois que le garbage collector s’exécute et fournit des informations sur la quantité fonctionnent le GC a :

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

Des informations supplémentaires telles que des informations de minutage GC peuvent être générées en définissant le `MONO_LOG_LEVEL` variable d’environnement `debug`:

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

Il en résulte dans les messages Mono supplémentaires (un grand nombre de), y compris ces trois des conséquences :

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

Dans le `GC_BRIDGE` message, `num-objects` est le nombre d’objets de pont envisage de cette étape, et `num_hash_entries` est le nombre d’objets traités au cours de cet appel du code de pont.

Dans le `GC_MINOR` et `GC_MAJOR` messages, `total` est la quantité de temps pendant que le monde est suspendu (aucun threads n’exécutent), tandis que `bridge` est la quantité de temps nécessaire dans le code (qui porte sur la machine virtuelle Java) de traitement du pont. Le monde est *pas* en pause pendant l’exécution de traitement du pont.

 *En général*, plus la valeur de `num_hash_entries`, le plus de temps qui le `bridge` prendront de collections et la plus grande le `total` sera de temps passé à collecter.



## <a name="global-reference-messages"></a>Messages de référence globale

Pour activer la référence globale loggig (GREF), journalisation, le *debug.mono.log* propriété système doit contenir *gref*, par exemple :

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android utilise les références globales Android pour fournir des mappages entre les instances de Java et les instances gérées associés, comme lors de l’appel de méthode Java qu'une instance Java doit être fourni à Java.

Malheureusement, les émulateurs Android autorisent uniquement les références de global 2000 d’exister à la fois. Matériel possède une quantité limite plus élevée de 52000 références globales. La limite inférieure peut être problématique lors de l’exécution des applications sur l’émulateur, par conséquent, en sachant *où* le provient de l’instance de peut être très utile.

 *Remarque*: le nombre de référence globale est interne à Xamarin.Android et pas (et ne peut pas) incluent des références globales par les autres bibliothèques natives chargés dans le processus. Utilisez le décompte de références global comme une estimation.

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

-  La création de référence globale : ce sont les lignes qui commencent par *+ g +* et fournira une trace de pile pour le chemin de code de création.
-  Destruction de référence globale : ce sont les lignes qui commencent par *- g-* et peut fournir une trace de pile pour le chemin d’accès du code en cours de suppression de la référence globale. Si le garbage collector est en cours de suppression de la gref, aucune trace de la pile ne sera fourni.
-  Faible globales font référence à la création : ce sont les lignes qui commencent par *+ w +* .
-  Destruction de référence faible global : il s’agit des lignes qui commencent par *-w-* .


Dans tous les messages, le *grefc* valeur est le nombre de références globales qui Xamarin.Android a créé, tandis que le *grefwc* valeur est le nombre de références faibles globales qui Xamarin.Android a créé. Le *gérer* ou *obj-handle* valeur est la valeur du handle JNI et le caractère situé après le */* est le type de valeur du handle :   */l* pour référence locale, */G* pour les références globales, et */W* pour les références faibles globales.

Dans le cadre du processus de GC, globales références (+ g +) sont converties en références globaux faibles (à l’origine a + w + et - g-), un GC côté Java est lancé, la référence faible globale est vérifiée pour voir si elles ont été collectées. Si elle est toujours actif, un nouveau gref est créé autour de la référence faible (+ g +, -w-), sinon la référence faible est détruite (-w).

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Instance de Java est créé et encapsulé par un MCW

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
## <a name="wref-turned-back-into-a-gref"></a>wref converti en un gref

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

Il existe une « intéressante » ombre au tableau ici : sur les cibles en cours d’exécution Android antérieures à 4.0, la valeur de gref est égale à l’adresse de l’objet Java dans la mémoire du runtime Android. (Autrement dit, le garbage collector est un déplacement non classique, collecteur et il est distribuant des références directes à ces objets.) Par conséquent, après un + g +, + w +, - g-, + g +, - w-séquence, le gref résultante aura la même valeur que la valeur gref d’origine. Cela rend grepping via les journaux assez simple.

Android 4.0, cependant, a un collecteur de déplacement et qu’il délivre ne sont plus les références directes au runtime Android objets ordinateur virtuel. Par conséquent, après un + g +, + w +, - g-, + g +, - w-séquence, la valeur gref *sera différente*. Si l’objet survit à plusieurs catalogues globaux, il passera par plusieurs valeurs gref, cela rend plus difficile de déterminer où une instance a été réellement allouée à partir de.

### <a name="querying-programmatically"></a>Interrogation par programmation

Vous pouvez interroger le nombre GREF et de WREF en interrogeant le `JniRuntime` objet.

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` -Nombre de références global

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` -Nombre de références faible



## <a name="android-debug-logs"></a>Journaux de débogage Android

Le [les journaux de débogage Android](~/android/deploy-test/debugging/android-debug-log.md) peut fournir un contexte supplémentaire concernant des erreurs d’exécution que vous voyez.



## <a name="floating-point-performance-is-terrible"></a>Les performances à virgule flottante sont terrible !

Vous pouvez également « mon application s’exécute 10 fois plus rapide avec la version Debug qu’avec la version Release ! »

Xamarin.Android prend en charge plusieurs appareils ABI : *armeabi*, *armeabi-v7a*, et *x86*. APPAREIL ABI peut être spécifié dans **propriétés du projet > onglet Application > architectures prises en charge**.

Les versions Debug utilisent un package Android qui fournit toutes les ABI et par conséquent utilisera l’ABI le plus rapide pour l’appareil cible.

Versions Release inclura uniquement l’ABI sélectionnée dans l’onglet Propriétés du projet. Plusieurs peuvent être sélectionné.

*armeabi* est la valeur par défaut ABI et a la plus large prise en charge du périphérique. *Toutefois*, armeabi ne prend en charge les appareils multiprocesseur et à virgule flottante, matériel amont autres choses. Par conséquent, les applications à l’aide de l’exécution de la mise en production armeabi seront associées à un seul cœur et utiliseront une implémentation de soft-float. Ces deux éléments peuvent contribuer à la baisse des performances pour votre application.

Si votre application requiert des performances à virgule flottante décent (par exemple, les jeux), vous devez activer le *armeabi-v7a* ABI. Vous pouvez souhaiter prennent uniquement en charge la *armeabi-v7a* runtime, bien que cela signifie que des appareils plus anciens qui prennent uniquement en charge *armeabi* ne pourront pas exécuter votre application.



## <a name="could-not-locate-android-sdk"></a>Kit de développement logiciel Android est introuvable.

Il n’y a 2 téléchargements disponibles à partir de Google pour le Kit de développement logiciel Android pour Windows.
Si vous choisissez le programme d’installation .exe, il écrit les clés de Registre qui indiquent Xamarin.Android où il a été installé. Si vous choisissez le fichier .zip et décompressez vous-même, Xamarin.Android ne sait pas où rechercher le Kit de développement. Vous pouvez communiquer Xamarin.Android où le Kit de développement dans Visual Studio en accédant à **Outils > Options > Xamarin > Paramètres Android**:

[![Emplacement d’Android SDK dans les paramètres de Xamarin Android](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)



## <a name="ide-does-not-display-target-device"></a>IDE n’affiche pas l’appareil cible

Parfois vous va tenter de déployer votre application sur un appareil, mais l’appareil que vous souhaitez déployer sur n’apparaît pas dans la boîte de dialogue Sélectionner un appareil. Cela peut se produire quand Android Debug Bridge décide d’aller en vacances.

Pour diagnostiquer ce problème, recherchez le [adb programme](~/android/deploy-test/debugging/android-debug-log.md), puis exécutez :

```shell
adb devices
```

Si votre appareil n’est pas présent, vous devez redémarrer le serveur de Android Debug Bridge afin que vous pouvez trouver votre appareil :

```shell
adb kill-server
adb start-server
```

Synchronisation de HTC logiciels peut-être empêcher **start-serveur adb** de fonctionner correctement. Si le **start-serveur adb** commande ne s’imprime pas le port utilisé par elle démarre sur, quittez le logiciel de synchronisation de HTC et essayez de redémarrer le serveur adb.


## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>La tâche exécutable spécifiée « keytool » n’a pas pu être exécutée.

Cela signifie que votre chemin d’accès ne contient pas le répertoire où se trouve le répertoire bin du Kit de développement Java. Vérifiez que vous avez suivi ces étapes à partir de la [Installation](~/android/get-started/installation/index.md) guide.


## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe ou aresgen.exe s’est arrêté avec le code 1

Pour vous aider à résoudre ce problème, accédez à Visual Studio et modifier le niveau de détail MSBuild, pour ce faire, sélectionnez : **Outils > Options > projet** et **Solutions > Générer** et **exécuter > commentaires sortie de génération de projet MSBuild** et définir cette valeur sur **Normal**.

Régénérez et vérifiez le volet de sortie de Visual Studio, qui doit contenir le message d’erreur.

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>Ne comporte pas suffisamment d’espace libre sur le périphérique pour déployer le package

Cela se produit lorsque vous ne démarrez pas l’émulateur à partir de Visual Studio. Lorsque vous démarrez l’émulateur en dehors de Visual Studio, vous devez transmettre le `-partition-size 512` options, par exemple,

```shell
emulator -partition-size 512 -avd MonoDroid
```

Vérifiez que vous utilisez le nom correct de simulateur, c'est-à-dire [le nom que vous avez utilisé lorsque vous configurez le simulateur](~/android/get-started/installation/windows.md#device).


## <a name="installfailedinvalidapk-when-installing-a-package"></a>INSTALLER\_échec\_non valide\_APK lors de l’installation d’un package

Les noms de package Android *doit* contiennent un point (*.*'). Modifier le nom de votre package afin qu’il contienne une période.

-   Dans Visual Studio :
    -   Cliquez avec le bouton droit sur votre projet > Propriétés
    -   Cliquez sur l’onglet manifeste Android sur la gauche.
    -   Mettre à jour le champ de nom de Package.
        -   Si vous voyez le message &ldquo;AndroidManifest.xml non trouvé. Cliquez pour ajouter un. &rdquo;puis mettez à jour le champ de nom de Package et cliquez sur le lien.
-   Dans Visual Studio pour Mac :
    -   Cliquez avec le bouton droit sur votre projet > Options.
    -   Accédez à la Build / section Application Android.
    -   Modifiez le champ de nom de Package pour contenir un '.'.




## <a name="installfailedmissingsharedlibrary-when-installing-a-package"></a>INSTALLER\_échec\_manquant\_partagé\_bibliothèque lors de l’installation d’un package

Une « bibliothèque partagée » dans ce contexte est *pas* une bibliothèque partagée native (*libfoo.so*) de fichiers ; c’est à la place une bibliothèque qui doit être installée séparément sur le périphérique cible, tels que Google Maps.

Le package Android spécifie les bibliothèques partagées sont nécessaires avec le `<uses-library/>` élément. Si un *requis* bibliothèque n’est pas présente sur l’appareil cible (par exemple, `//uses-library/@android:required` est *true*, qui est la valeur par défaut), puis l’installation du package échoue avec *installer\_ Échec de\_manquant\_partagé\_bibliothèque*.

Pour déterminer les bibliothèques partagées sont requises, voir la *généré*
**AndroidManifest.xml** fichier (par exemple, **obj\\déboguer\\android \\AndroidManifest.xml**) et recherchez le `<uses-library/>` éléments. `<uses-library/>` éléments peuvent être ajoutés manuellement dans votre projet **propriétés\\AndroidManifest.xml** fichier et via le [attribut personnalisé UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/).

Par exemple, ajoutez une référence d’assembly *Mono.Android.GoogleMaps.dll* ajoute implicitement une `<uses-library/>` pour la bibliothèque partagée Google Maps.



## <a name="installfailedupdateincompatible-when-installing-a-package"></a>INSTALLER\_échec\_mise à jour\_incompatibles lors de l’installation d’un package

Packages Android ont trois conditions :

-   Ils doivent contenir un '.' (consultez les entrées précédentes)
-   Ils doivent avoir un nom de package de chaîne unique (par conséquent, la convention inverse-tld vue dans les noms de l’application Android, par exemple, com.android.chrome pour l’application Chrome)
-   Lors de la mise à niveau de packages, le package doit avoir la même clé de signature.

Par conséquent, imaginons le scénario :

1.  Build et de déployer votre application comme une application de débogage
2.  Vous modifiez la clé de signature, par exemple à utiliser comme une application de mise en production (ou parce que vous n’aimez pas par défaut-fourni par le débogue clé de signature)
3.  Vous installez votre application sans le supprimer tout d’abord, par exemple Déboguer > Démarrer sans débogage dans Visual Studio


Dans ce cas, l’installation du package échoue avec une installation\_échec\_mise à jour\_erreur INCOMPATIBLE, car le nom du package n’a pas changé pendant la signature de la clé a. Le [journal de débogage Android](~/android/deploy-test/debugging/android-debug-log.md) contiendra également un message similaire à :

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

Pour corriger cette erreur, supprimez complètement l’application à partir de votre appareil avant de réinstaller.


## <a name="installfaileduidchanged-when-installing-a-package"></a>INSTALLER\_échec\_UID\_modifié lors de l’installation d’un package

Lorsqu’un package Android est installé, il reçoit un *id utilisateur* (UID).
*Parfois*, pour des raisons actuellement inconnus, lors de l’installation sur une application déjà installée, l’installation échoue avec `INSTALL_FAILED_UID_CHANGED`:

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

Pour contourner ce problème, *désinstaller entièrement* le package Android, soit en installant l’application à partir de l’interface graphique utilisateur de la cible Android, ou à l’aide de `adb`:

```shell
$ adb uninstall @PACKAGE_NAME@
```

**N’utilisez pas** `adb uninstall -k`, comme cela sera *conserver* données d’application et donc préserver l’UID en conflit sur l’appareil cible.



## <a name="release-apps-fail-to-launch-on-device"></a>Applications de mise en production ne parviennent pas à lancer sur périphérique

La sortie de journal de débogage Android contient un message similaire à :

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

Dans ce cas, il existe deux causes possibles :

1.  Le fichier .apk ne fournit pas une ABI qui prend en charge de l’appareil cible.
    Par exemple, le fichier .apk contient uniquement les fichiers binaires armeabi-v7a, et l’appareil cible prend uniquement en charge armeabi.

2.  Un [bogue Android](http://code.google.com/p/android/issues/detail?id=21670). Si c’est le cas, désinstallez l’application cross vos doigts et réinstallez l’application.

Pour corriger les (1), modifier les Options/propriétés du projet et [ajouter la prise en charge de l’ABI nécessaire à la liste des ABI prises en charge](~/android/app-fundamentals/cpu-architectures.md). Pour déterminer quelle ABI, vous devez ajouter, exécutez la commande d’adb suivantes sur votre appareil cible :

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

La sortie contient le réplica principal (et secondaire facultatif) ABI.

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>La propriété OutPath n’est pas définie pour le projet &ldquo;MyApp.csproj&rdquo;

Cela signifie généralement que vous avez un ordinateur HP et la variable d’environnement &ldquo;plateforme&rdquo; a été défini sur quelque chose comme MCD ou HPD. Cela est en conflit avec la propriété de la plateforme MSBuild qui est généralement définie sur &ldquo;Any CPU&rdquo; ou &ldquo;x86&rdquo;. Vous devez supprimer cette variable d’environnement à partir de votre ordinateur avant que MSBuild puisse fonctionner :

-   Panneau de configuration > système > Avancé > Variables d’environnement

Redémarrez Visual Studio ou Visual Studio pour Mac et tentez de régénérer. Choses devraient désormais fonctionner comme prévu.

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>java.lang.ClassCastException : mono.android.runtime.JavaObject ne peut pas être casté en...

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


Le problème est que Xamarin.Android marshale incorrectement des types génériques imbriqués. Le `List<IDictionary<string, object>>` est marshalé à un [java.lang.ArrrayList](https://developer.xamarin.com/api/type/Java.Util.ArrayList/), mais la `ArrayList` est contenant `mono.android.runtime.JavaObject` instances (lequel référencer le `Dictionary<string, object>` instances) au lieu de quelque chose qui implémente [java.util.Map](https://developer.xamarin.com/api/type/Java.Util.IMap/), se traduisant par l’exception suivante :

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

La solution de contournement consiste à utiliser fourni [types de collections de Java](~/android/internals/api-design.md) au lieu du `System.Collections.Generic` types pour le &ldquo;interne&rdquo; types. Ainsi, les types Java appropriés lors du marshaling les instances. (Le code suivant est plus complexe que nécessaire afin de réduire les durées de vie gref. Il peut être simplifié à la modification du code d’origine via `s/List/JavaList/g` et `s/Dictionary/JavaDictionary/g` si les durées de vie gref ne sont pas un problème.)

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


## <a name="unexpected-nullreferenceexceptions"></a>NullReferenceExceptions inattendues

Parfois le [journal de débogage Android](~/android/deploy-test/debugging/android-debug-log.md) mentionne NullReferenceExceptions qui &ldquo;ne peut pas se produire,&rdquo; ou proviennent de Mono pour le code de runtime Android peu de temps avant l’application de matrices :

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

Cela peut se produire quand le runtime Android décide d’annuler le processus, qui peut se produire pour différentes raisons, parmi lesquelles atteindre la limite GREF de la cible ou de faire quelque chose &ldquo;incorrect&rdquo; JNI.

Pour voir si c’est le cas, consultez le journal de débogage Android pour un message à partir de votre processus similaire à :

```shell
E/dalvikvm(  123): VM aborting
```


## <a name="abort-due-to-global-reference-exhaustion"></a>Abandon en raison de l’épuisement de la référence globale

Couche JNI du runtime Android prend uniquement en charge un nombre limité de références d’objet JNI soit valide à un moment donné dans le temps. Lorsque cette limite est dépassée, panne.

Le GREF (*référence globale*) limite est de références de 2000 dans l’émulateur, et fait référence à ~ 52000 sur du matériel.

Vous savez que vous commencez à créer trop GREFs lorsque vous voyez des messages tels que cela dans le journal de débogage Android :

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

Lorsque vous atteignez la limite GREF, un message semblable à celui-ci s’affiche :

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


Dans l’exemple ci-dessus (qui, au passage, provient de [bogues 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)) le problème qui est trop Android.Graphics.Point instances sont créées ; consultez [commentaire \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2) pour obtenir la liste des correctifs pour Ce bogue particulier.

En règle générale, une solution utile consiste à trouver le type a trop d’instances allouée &ndash; Android.Graphics.Point dans le vidage ci-dessus &ndash; puis trouver où elles sont créées en conséquence dans votre code source et dispose d’eux (afin que leurs Durée de vie Java-Object est raccourcie). Cela n’est pas toujours approprié (\#685215 est multithread, la solution triviale évite l’appel Dispose), mais c’est la première chose à prendre en compte.

Vous pouvez activer [GREF journalisation](~/android/troubleshooting/index.md) pour voir quand GREFs sont créés et combien existe.


## <a name="abort-due-to-jni-type-mismatch"></a>Abandon en raison d’une incompatibilité de type JNI

Si vous écriture code JNI, il est possible que les types ne correspondront pas correctement, par exemple, si vous tentez d’appeler `java.lang.Runnable.run` sur un type qui n’implémente pas `java.lang.Runnable`. Lorsque cela se produit, il y aura un message similaire à celle-ci dans le journal de débogage Android :

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>Prise en charge du Code dynamique

### <a name="dynamic-code-does-not-compile"></a>Code dynamique n’est pas compilé.

Utiliser C\# dynamique dans votre application ou une bibliothèque, vous devez ajouter System.Core.dll, Microsoft.CSharp.dll et Mono.CSharp.dll à votre projet.

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>Dans la version Release, MissingMethodException se produit pour le code dynamique au moment de l’exécution.

-   Il est probable que votre projet d’application n’a pas de références à System.Core.dll, Microsoft.CSharp.dll ou Mono.CSharp.dll. Assurez-vous que ces assemblys sont référencés.

    -   N’oubliez pas que le code dynamique toujours les coûts. Si vous avez besoin d’un code efficace, envisagez d’utiliser ne pas de code dynamique.

-   Dans la première version préliminaire, ces assemblys ont été exclus, sauf si les types dans chaque assembly sont explicitement utilisés par le code d’application. Consultez la rubrique suivante pour résoudre ce problème : [http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)


## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>Projets créés avec l’incident d’AOT + LLVM sur x86 appareils

Lorsque vous déployez une application générée avec [AOT + LLVM](~/android/deploy-test/release-prep/index.md) sur les appareils x86, vous pouvez voir un message d’erreur semblable au suivant :

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (amarin.bug56111)
```

Il s’agit d’un problème connu comme indiqué dans [56111](https://bugzilla.xamarin.com/show_bug.cgi?id=56111). La solution de contournement consiste à désactiver la LLVM.
