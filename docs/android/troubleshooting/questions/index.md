---
title: "Questions fréquemment posées"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: d6d1a5f659317267859164181216177857a67fd2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="frequently-asked-questions"></a>Questions fréquemment posées

## <a name="installation--setup"></a>Installation et configuration

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Les packages du SDK Android dois-je installer ?](install-android-sdk-packages.md)

Installation du SDK Android n’inclut pas automatiquement tous les packages requis minimales pour le développement. Bien que les développeurs individuels doivent varier, ce guide décrit les packages qui seront généralement requis pour le développement avec Xamarin.Android.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Où puis-je définir mes emplacements du SDK Android ?](android-sdk-location.md)

Ce guide décrit à la fois les paramètres par défaut du SDK Android, qui doit fonctionner pour la plupart des configurations ; et comment modifier ces valeurs par défaut dans Visual Studio pour Mac ou Visual Studio, si nécessaire.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[Comment mettre à jour la version du Kit de développement Java (JDK) ?](update-jdk.md)

Cet article explique comment mettre à jour la version du Kit de développement Java (JDK) sur Windows et Mac.

### <a name="can-i-use-java-development-kit-jdk-version-9jdk9-errorsmd"></a>[Puis-je utiliser le Kit de développement Java (JDK) version 9 ?](jdk9-errors.md)

Xamarin.Android requiert JDK 8 au lieu de JDK 9. Cet article répertorie certains messages d’erreur courants que vous pouvez rencontrer si JDK 9 est installé, ainsi que les instructions pour la vérification de la version du JDK.


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Comment installer manuellement les bibliothèques Android prennent en charge requis par les packages Xamarin.Android.Support ?](install-android-support-library.md)

Ce guide fournit des exemples d’étapes pour installer le `Xamarin.Android.Support.v4` bibliothèque de prise en charge sur Windows et Mac.

### <a name="how-do-i-install-google-play-services-in-an-emulatorinstall-gpsmd"></a>[Comment installer les Services Google Play dans un émulateur ?](install-gps.md)

Ce guide liens vers des informations sur l’installation des Services Google Play dans l’émulateur Android de Visual Studio.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Les pilotes USB ai-je besoin pour déboguer Android sur Windows ?](android-drivers-debug-windows.md)

Pour déboguer sur un appareil Android lors du développement de Windows ; Vous devez installer un pilote USB compatible. Le Gestionnaire du Kit de développement logiciel Android inclut le « pilote USB Google » par défaut, ce qui ajoute la prise en charge pour les appareils Nexus.
Autres périphériques nécessitent des pilotes USB spécifiquement publiés par le fabricant du périphérique. Ce guide fournit des informations sur la recherche de ces pilotes ainsi que les autres méthodes de tests.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Il est possible de se connecter à des émulateurs Android fonctionne sur un Mac à partir d’une machine virtuelle Windows ?](connect-android-emulator-mac-windows.md)

Ce guide couvre les méthodes lors de l’utilisation d’un émulateur Android de Google.

## <a name="general-questions"></a>Questions générales

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Comment pour automatiser un projet de Test de NUnit Android ?](automate-android-nunit-test.md)

Ce guide couvre les étapes de configuration d’un projet de test NUnit Android, _pas_ un projet Xamarin.UITest. Repères de Xamarin.UITest se trouve [ici](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

### <a name="how-do-i-enable-intellisense-in-android-axml-filesenable-axml-intellisensemd"></a>[Comment faire pour activer Intellisense dans les fichiers .axml Android ?](enable-axml-intellisense.md)

Ce guide décrit comment activer Intellisense de Visual Studio pour les fichiers .axml Android.

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Pourquoi ne peut pas ma version Release Android se connecter à Internet ?](android-internet.md)

La cause la plus courante de ce problème est que le **INTERNET** autorisation est automatiquement incluse dans une version debug, mais doivent être définie manuellement pour une version release. Ce guide décrit comment activer l’autorisation sur les versions release.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[Plus intelligente Xamarin Android prennent en charge v4 / v13 les Packages NuGet](android-support-v4v13-libraries.md)

`Support-v4` et `Support-v13` ne peuvent pas être utilisées ensemble dans la même application, autrement dit, ils s’excluent mutuellement. C’est parce que `Support-v13` réellement contient tous les types et l’implémentation de `Support-v4`. Si vous essayez font référence à la fois dans le même projet, vous rencontrerez des erreurs de type en double.


## <a name="deprecated"></a>Déconseillé

> [!NOTE]
> **Remarque :** les articles ci-dessous s’appliquent à des problèmes qui ont été résolus dans les versions récentes de Xamarin. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de la sortie de journal de build.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[Quelle version de Xamarin.Android prise en charge l’interface Lollipop ?](xa-lollipop.md)

Ce guide a été écrit à l’origine pour la version préliminaire L Android. Xamarin.Android 4.17 ajoutés Android prennent en charge préliminaire de L & Xamarin.Android 4.20 ajoutés Android prennent en charge l’interface Lollipop.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat - aucune ressource trouvée qui correspond au nom donné : attr 'android : actionModeShareDrawable'](missing-action-mode-share-drawable.md)

Cette erreur peut se produire dans les versions antérieures de Xamarin si certains les pacakages du SDK Android requis sont manquants.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Réglage des paramètres de mémoire de Java pour le concepteur Android](android-designer-java-memory.md)

Les paramètres de mémoire par défaut qui sont utilisées lors du démarrage de la `java` traiter pour le concepteur Android risque d’être incompatible avec certaines configurations système. À partir de Xamarin Studio 5.7.2.7 et Xamarin pour Visual Studio 3.9.344 ces paramètres peut être personnalisé par projet.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Mon fichier Resource.designer.cs Android ne met pas à jour](resource-designer-wont-update.md)

Un bogue dans Xamarin.Studio 5.1 endommagé précédemment fichiers .csproj en partiellement ou totalement supprimant le code xml dans le fichier .csproj. Cela entraînerait important système (par exemple, la mise à jour le Resource.designer.cs Android) de génération de parties de la Android échec. À compter de la 5.1.4 stable libérer le 15 juillet, ce bogue a été résolu ; mais dans de nombreux cas le fichier projet doit être réparé manuellement, comme décrit dans ce guide.



