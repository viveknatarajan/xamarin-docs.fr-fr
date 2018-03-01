---
title: "Questions fréquemment posées"
ms.topic: article
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 1477d991fadfe381032e4009fa0bf0faf0518b8e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="frequently-asked-questions"></a>Questions fréquemment posées

## <a name="general-questions"></a>Questions générales

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Puis-je utiliser une machine virtuelle Mac avec Xamarin ?](mac-vm.md)
Oui, mais uniquement sur le matériel Mac.

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Comment puis-je rétrograder Xcode ?](downgrade-xcode.md)
Ce guide fournit des liens permettant d’accéder aux versions précédentes de Xcode, ainsi que la version la plus récente.

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[Où puis-je définir mes emplacements du Kit de développement logiciel iOS ?](ios-sdk.md)
Pour la plupart des utilisateurs ils sont définis aux emplacements appropriés automatiquement. Ce guide répertorie les emplacements du Kit de développement logiciel par défaut et comment les modifier si nécessaire.

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[Comment puis-je réactiver des options pour développeurs après la mise à jour iOS ?](update-developer-options.md)
Un bogue iOS peut provoquer les options de développeur à disparaître après la mise à jour des versions d’iOS, il a été observée lors du passage à iOS 8.x. Ce guide décrit comment les options peuvent être réactivées.

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[Emplacement de l’utilisateur ne fonctionne ne pas dans iOS 8](ios8-user-location.md)
Ce guide vous explique comment modifier le fichier info.plist pour activer l’emplacement de l’utilisateur dans iOS 8.

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[Où puis-je trouver le fichier .dSYM symbolicate journaux iOS ?](symbolicate-ios-crash.md)
Ce guide décrit les étapes de base pour symbolicating journaux iOS pour aider à diagnostiquer les incidents. Il contient également des liens vers des ressources supplémentaires pour les plus avancées techniques de symbolication & informations sur l’interprétation des journaux iOS.


### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Comment définir des variables d’environnement Mono Runtime pour les projets iOS dans Xamarin Studio ?](xs-mono-runtime.md)
Si vous avez besoin définir des variables d’environnement n’importe quel runtime pour Mono, elles peuvent être définies dans le **Options du projet > Exécuter > Général** page.

## <a name="publishing-questions"></a>Questions de publication

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[Erreur lors de l’envoi à l’App Store : « Regroupement non valide - Options ne pas autorisés à incorporer dans bitcode sont détectés dans l’envoi »](invalid-bundle-bitcode.md)

Soumettre des applications qui _nécessitent_ bitcode, telles que des applications watchOS et tvOS, doit être réalisée avec Xcode 9.

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[Puis-je modifier le chemin de sortie du fichier IPA ?](ipa-output-path.md)
À compter de 7 du Cycle de Xamarin, vous pouvez utiliser cibles de MSBuild personnalisées d’effectuer cette opération.

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[Comment puis-je copier des fichiers de sortie IPA dans le dossier de dépôt TFS ?](ipa-tfs.md)
Oui, ce guide décrit comment.

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[Puis-je ajouter des fichiers à ou supprimer des fichiers à partir d’un fichier IPA après l’avoir généré dans Visual Studio ?](modify-ipa.md)
Oui, il est possible, mais elle nécessite généralement que vous signer à nouveau le `.app` regroupement après avoir apporté la modification. Notez que la modification du `.ipa` fichier n’est pas nécessaire dans une utilisation normale. Cet article est fourni uniquement à titre d’information.

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Il est possible de créer une archive .xcarchive à partir de Visual Studio ?](create-xcarchive.md)
À compter de Xamarin 4, il est possible de créer un `.xcarchive` à partir de Windows en définissant le `ArchiveOnBuild` propriété `true`.

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[Pourquoi mon envoi de l’application échoue avec : « Interdit les chemins d’accès (« iTunesMetadata.plist »), consultez... » ?](itunesmetadata-disallowed-paths.md)
Cette erreur est le résultat d’une modification du processus de vérification App Store d’Apple. Cette erreur spécifique est _pas_ liées à une version particulière de Xamarin, vous avez installé, par conséquent, la rétrogradation sera _pas_ aide. Liens de ce guide pour plus d’informations sur la façon de résoudre le problème.


## <a name="diagnosing-specific-error-messages"></a>Diagnostiquer les Messages d’erreur spécifiques

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[Erreur du concepteur avec RegisterServicePort d’iOS](error-registerserviceport.md)
Erreurs avec `RegisterServicePort` et messages d’erreur semblables comme ci-dessus sont généralement un problème avec les logiciels espions/programmes malveillants sur l’ordinateur. Ce guide décrit en détail confirmant le diagnostic et les informations sur la suppression les logiciels espions/programmes malveillants.

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[Pourquoi ma build iOS échoue avec : code valide iPhone clés de signature est introuvable dans le trousseau ?](no-codesigning-keys.md)
Ce message d’erreur se produit lorsque le projet en question est vous recherchez des informations d’identification de signature de code valides, mais ne parvenez pas à les trouver. Signature de code est requise pour les tests et les déploiements sur des appareils iOS physiques ; et aussi Ad-hoc & application stocker les builds.

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[Pourquoi mon application iOS 9 n’échoue avec : System.Exception : Impossible de marshaler l’objet Objective-C ?](exception-marshal-obj-c.md)
Modifications de l’API dans iOS 9 demander un constructeur de rappel à utiliser lors de l’appel de code non managé, comme l’API sous-jacente maintenant s’attend.

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[Erreur d’exécution : le fichier d’assembly mscorlib.dll est introuvable ou ne peut pas être chargé](error-mscorlib-not-found.md)
Ce problème se produit lorsque le *masqué* `.monotouch-32` et `.monotouch-64` dossiers sont absents de la `.xcarchive` pour la signature / création IPA, déclenchement de l’erreur d’exécution.

## <a name="deprecated"></a>Déconseillé

> [!IMPORTANT]
> Les articles ci-dessous s’appliquent à des problèmes qui ont été résolus dans les versions récentes de Xamarin. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de la sortie de journal de build.



### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[Fichier IPA est de 0 octet](ipa-zero-bytes.md)
A des problèmes connus dans les versions précédentes de Xamarin qui peut entraîner le fichier IPA sur Windows à 0 octet.

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[Une erreur IBTool : Impossible d’effectuer l’opération.](error-ibtool.md)
Apple [fixe](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html) cela `ibtool` bogue dans Xcode 6.1.1, par conséquent, la mise à niveau à Xcode 6.1.1 ou une version ultérieure est le plus simple.

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[Erreur MT1009 : N’a pas pu copier l’assembly](error-mt1009.md)
Cela affecte les utilisateurs Xamarin.iOS 7.2.6 en cours d’exécution. Ce problème est en raison d’autorisations de fichier nécessitant des privilèges plus élevés lorsque Xamarin.iOS est installé avec un autre compte d’utilisateur puis le compte de principal.

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[System.Exception AMDeviceNotificationSubscribe retourné...](exception-amddevicenotificationsubscribe.md)
Ce message peut s’afficher dans une boîte de dialogue d’erreur lorsque vous démarrez Visual Studio pour Mac ou dans le `mtbserver.log` fichier. Notez qu’il s’agit d’un problème courant. Si Visual Studio rencontre des problèmes de connexion à l’hôte de build Mac, il existe d’autres erreurs qui sont plus susceptibles d’apparaître dans le `mtbserver.log` fichier.

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[MDocArchiveToMsxDocConverter.exe introuvable du serveur. BaseCommand.OnRequest](mdocarchivetomsxdocconverter-not-found.md)
Cette erreur peut apparaître dans le `Mac Server Log` dans Visual Studio.
