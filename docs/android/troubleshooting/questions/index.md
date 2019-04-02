---
title: Forum aux Questions de Xamarin.Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
---

# <a name="android-frequently-asked-questions"></a>Forum aux Questions de Android

## <a name="installation--setup"></a>Installation et configuration

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Quels packages Android SDK dois-je installer ?](install-android-sdk-packages.md)

L’installation du Kit Android SDK n’inclut pas automatiquement tous les packages requis minimales pour le développement. Bien que les développeurs individuels doivent varient, ce guide décrit les packages qui seront généralement requis pour le développement avec Xamarin.Android.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Où puis-je définir les emplacements de mon Kit SDK Android ?](android-sdk-location.md)

Ce guide décrit les deux les paramètres par défaut du SDK Android, qui doit fonctionner pour la plupart des configurations ; et comment modifier ces valeurs par défaut dans Visual Studio pour Mac ou Visual Studio si nécessaire.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Comment mettre à jour la version du Kit JDK Java ?](update-jdk.md)

Cet article explique comment mettre à jour la version du Kit de développement Java (JDK) sur Windows et Mac.

### <a name="can-i-use-java-development-kit-jdk-version-9-or-laterjdk9-errorsmd"></a>[Puis-je utiliser le Kit de développement Java (JDK) version 9 ou version ultérieure ?](jdk9-errors.md)

Xamarin.Android nécessite le JDK 8 ou Microsoft Mobile OpenJDK. Cet article répertorie certains messages d’erreur courants que vous pouvez voir si JDK 9 ou version ultérieure est installé, ainsi que des instructions pour la vérification de la version du JDK.


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Comment puis-je installer manuellement les bibliothèques de prise en charge Android requises par les packages Xamarin.Android.Support ?](install-android-support-library.md)

Ce guide fournit des exemples d’étapes pour installer le `Xamarin.Android.Support.v4` bibliothèque de prise en charge sur Windows et Mac.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[De quels pilotes USB ai-je besoin pour déboguer Android sur Windows ?](android-drivers-debug-windows.md)

Pour déboguer sur un appareil Android lors du développement de Windows ; Vous devez installer un pilote USB compatible. Le gestionnaire Android SDK inclut « Google USB Driver » par défaut, ce qui ajoute la prise en charge pour les périphériques Nexus.
Autres périphériques nécessitent des pilotes USB publiés par le fabricant du périphérique. Ce guide fournit des informations sur la recherche de ces pilotes, ainsi que les autres méthodes de tests.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Est-il possible de se connecter à des émulateurs Android exécutés sur un Mac à partir d’une machine virtuelle Windows ?](connect-android-emulator-mac-windows.md)

Ce guide traite des méthodes lorsque vous utilisez l’émulateur Android.

## <a name="general-questions"></a>Questions générales

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Comment automatiser un projet de test Android NUnit ?](automate-android-nunit-test.md)

Ce guide décrit les étapes de configuration d’un projet de test Android NUnit _pas_ un projet Xamarin.UITest. Vous pouvez trouver des guides de Xamarin.UITest [ici](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Pourquoi ma version mise en production Android ne peut-elle pas se connecter à Internet ?](android-internet.md)

La cause la plus courante de ce problème est que le **INTERNET** autorisation est automatiquement incluse dans une version debug, mais doit être définie manuellement pour une version Release. Ce guide décrit comment activer l’autorisation sur les versions release.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[Packages NuGet v4 / v13 de prise en charge Xamarin Android plus intelligents](android-support-v4v13-libraries.md)

`Support-v4` et `Support-v13` ne peuvent pas être utilisées ensemble dans la même application, autrement dit, ils s’excluent mutuellement. Il s’agit, car `Support-v13` réellement contient tous les types et l’implémentation de `Support-v4`. Si vous essayez de référence à la fois dans le même projet, vous rencontrerez des erreurs de type en double.

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[Comment résoudre une erreur PathTooLongException ?](path-too-long-exception.md)

Cet article explique comment résoudre un **PathTooLongException** erreur pouvant se produire lors de la génération d’un projet Xamarin.Android.



## <a name="deprecated"></a>Déconseillé

> [!NOTE]
> Les articles ci-dessous s’appliquent aux problèmes qui ont été résolus dans les versions récentes de Xamarin. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de générer la sortie de journal.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[Quelle version de Xamarin.Android ajoutait la prise en charge de Lollipop ?](xa-lollipop.md)

Ce guide a été écrit à l’origine pour la version préliminaire L Android. Xamarin.Android 4.17 ajouté la prise en charge de préversion L Android & Xamarin.Android 4.20 prise en charge Android Lollipop.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat - Aucune ressource trouvée correspondant au nom donné : attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

Cette erreur peut se produire dans les versions antérieures de Xamarin si certains des packages du Kit Android SDK requis sont manquants.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Réglage des paramètres de mémoire Java pour le concepteur Android](android-designer-java-memory.md)

Les paramètres de mémoire par défaut qui sont utilisés lors du démarrage de la `java` traiter d’Android designer peut être incompatible avec certaines configurations système. En commençant par Xamarin Studio 5.7.2.7 et Xamarin pour Visual Studio 3.9.344 ces paramètres peut être personnalisé par projet.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Mon fichier Android Resource.designer.cs ne se met pas à jour](resource-designer-wont-update.md)

Un bogue dans Xamarin.Studio 5.1 endommagé précédemment fichiers .csproj en partiellement ou totalement supprimant le code xml dans le fichier .csproj. Cela entraînerait important système (par exemple, la mise à jour de l’Android Resource.designer.cs) de génération de parties de l’Android échec. À compter de la 5.1.4 stable libérer le 15 juillet, ce bogue a été résolu ; mais dans de nombreux cas le fichier projet doit être réparée manuellement, comme décrit dans ce guide.



