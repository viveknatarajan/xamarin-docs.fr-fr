---
title: "À l’aide de la ville de l’équipe avec Xamarin"
description: "Ce guide traite des étapes de l’utilisation de TeamCity pour compiler des applications mobiles et les envoyer au Xamarin Test Cloud."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: caff1fb834ade35e68eb19683e87788a4aa70740
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="using-team-city-with-xamarin"></a>À l’aide de la ville de l’équipe avec Xamarin

_Ce guide traite des étapes de l’utilisation de TeamCity pour compiler des applications mobiles et les envoyer au Xamarin Test Cloud._

Comme indiqué dans le [Introduction à l’intégration continue](~/tools/ci/intro-to-ci.md) guide, l’intégration continue (CI) est une pratique utile lors du développement d’applications mobiles de qualité. Il existe plusieurs options viables pour le logiciel de serveur d’intégration continue ; Ce guide met l’accent sur [TeamCity](http://www.jetbrains.com/teamcity/) de JetBrains.

Il existe plusieurs permutations différentes d’une installation TeamCity. Voici une liste de certains d'entre eux :

- **Service Windows** : dans ce scénario, il démarre TeamCity lorsque Windows démarre comme un Service Windows. Il doit être associé à un hôte de Build Mac pour compiler des applications iOS.

- **Lancement du démon sur OS X** – point de vue conceptuel, ceci est très similaire à l’exécution en tant que Service Windows à l’étape précédente. Par défaut les builds seront exécute sous le compte racine.

- **Compte d’utilisateur sur OS X** – il est possible d’exécuter TeamCity sous un compte d’utilisateur qui démarre chaque fois que l’utilisateur se connecte.

Les scénarios précédents, en cours d’exécution TeamCity sous un compte d’utilisateur sur OS X est la plus simple et la plus simple pour le programme d’installation.

Il existe plusieurs étapes impliquées dans la définition de TeamCity :

- **Installation TeamCity** – l’installation de TeamCity n’est pas abordée dans ce guide. Ce guide suppose que TeamCity est installé et en cours d’exécution sous un compte d’utilisateur. Obtenir des instructions sur [installation TeamCity](http://confluence.jetbrains.com/display/TCD8/Installation) se trouvent dans le [TeamCity 8 documentation](http://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) par JetBrains.

- **Préparation du serveur de Build** : cette étape consiste à installer le logiciel nécessaire, outils, et les certificats requis pour générer des applications mobiles et les préparer pour la distribution.

- **Création d’un Script générer** : cette étape n’est pas strictement nécessaire, mais un script de compilation est une aide utile pour générer des applications sans assistance. À l’aide d’un script de compilation aidera à résoudre les problèmes de génération qui peuvent survenir et fournit un moyen cohérent et reproductible pour créer les fichiers binaires pour la distribution, même si l’intégration continue n’est pas utilisée.

- **Création d’un projet de TeamCity** – une fois que les trois étapes précédentes terminées, nous devons créer un projet de TeamCity qui contient toutes les métadonnées nécessaires pour récupérer le code source, les projets de compilation et envoyer les tests à Xamarin Test Cloud.

# <a name="requirements"></a>Configuration requise

Expérience avec [Xamarin Test Cloud](https://developer.xamarin.com/guides/testcloud) est requis.

Vous êtes familiarisé avec TeamCity 8.1 est requis. L’installation de TeamCity est abordée dans ce document. Il est supposé que TeamCity est installé sur OS X Mavericks et qu’il s’exécute sous un compte d’utilisateur normal et non le compte racine.

Le serveur de build doit être un ordinateur autonome, OS X, qui est dédié à l’intégration continue en cours d’exécution. Dans l’idéal, le serveur de builds ne sera pas chargé pour tous les autres rôles, comme un serveur de base de données, un serveur web ou une station de travail de développement.

> [!IMPORTANT]
> Ce guide ne couvre pas une installation sans « affichage » de Xamarin.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>Préparation du serveur de Build

Une étape importante de la configuration d’un serveur de builds est d’installer les outils nécessaires, les logiciels et les certificats pour générer des applications mobiles. Il est important que le serveur de builds être en mesure de compiler la solution mobile et exécuter des tests. Pour réduire les problèmes de configuration, les logiciels et les outils doivent être installés dans le même compte d’utilisateur qui héberge TeamCity. Voici une liste des éléments requis :

1. **Visual Studio pour Mac** : Cela inclut Xamarin.iOS et Xamarin.Android.
2. **Connexion au magasin de composant Xamarin** : cette étape est facultative et est uniquement nécessaire si votre application utilise des composants à partir du magasin de composant Xamarin. Journalisation proactive dans le magasin de composants à ce stade empêchera les problèmes lorsqu’une build TeamCity tente de compiler l’application.
3. **Xcode** – Xcode est nécessaire pour des applications iOS de compilation et d’authentification.
4. **Outils de ligne de commande Xcode** : cet exemple est décrit à l’étape 1 de la section Installation de le [Ruby de mise à jour avec rbenv](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) guide.
5. **Signature des identités et des profils de configuration** : importer les certificats et configuration du profil via XCode. Consultez le guide d’Apple sur [exportation des identités de signature et les profils de configuration](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) pour plus d’informations.
6. **Android Keystores** – copier les keystores Android requis dans un répertoire que l’utilisateur TeamCity a accès, par exemple, `~/Documents/keystores/MyAndroidApp1`.
7. **Calabash** : cette étape est facultative si votre application comporte des tests écrits à l’aide de Calabash. Consultez le [Calabash de l’installation sur OS X Mavericks](https://developer.xamarin.com/guides/testcloud/calabash/osx-installation/) guide et [Ruby de mise à jour avec rbenv](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) guide pour plus d’informations.

Le diagramme suivant illustre tous ces composants :

![](teamcity-images/image1.png "Ce diagramme illustre tous ces composants.")

Une fois que tous les logiciels a été installé, connectez-vous au compte d’utilisateur et vérifier que tous les logiciels a été correctement installé et fonctionne. Il doit inclure la compilation de la solution et l’envoi de l’application au Cloud de Test. Cela peut être considérablement simplifié en exécutant le script de compilation, comme décrit dans la section suivante.

## <a name="create-a-build-script"></a>Créer un Script de compilation

Bien qu’il soit possible pour TeamCity gérer tous les aspects de la compilation et la soumission des applications mobiles pour Test Cloud par lui-même, il est fortement recommandé pour créer un script de compilation. Un script de génération offre les avantages suivants :

1. **Documentation** – un script de compilation sert de documentation sur la façon dont le logiciel est construit. Cette opération supprime certaines des « magique » qui est associé avec le déploiement de l’application et permet aux développeurs de se concentrer sur les fonctionnalités.
1. **Répétabilité** – un script de compilation garantit que chaque fois que l’application est compilée et déployée, il se produit dans exactement de la même manière, quelle que soit la qui ou ce que fait le travail. Cette cohérence repeatable supprime tous les problèmes ou les erreurs qui peuvent glissent en raison d’une build exécutée de manière incorrecte ou erreur humaine.
1. **Le contrôle de version** – un script de compilation peut être inclus dans le système de contrôle de code source. Cela signifie que les modifications au script de compilation peuvent être suivies, analysées et corrigées si les erreurs ou des imprécisions sont trouvées.
1. **Préparer l’environnement** – un script de compilation peut inclure une logique pour installer tout 3e dépendances tierces. Cela garantit que les applications sont générées avec les composants appropriés.

Le script de compilation peut être aussi simple que d’un fichier de Powershell (sous Windows) ou un script d’interpréteur de commandes (sur OS X). Lorsque vous créez le script de compilation, il existe plusieurs options pour les langages de script :

- [**Inclinaison** ](https://github.com/jimweirich/rake) – il s’agit d’un langage spécifique de domaine (DSL) pour générer les projets, basés sur Ruby. Inclinaison a l’avantage de la popularité et d’un écosystème complet des bibliothèques.

- [**psake** ](https://github.com/psake/psake) – il s’agit d’une bibliothèque de Windows Powershell pour la création de logiciels

- [**FAUX** ](http://fsharp.github.io/FAKE/) – il s’agit d’une DSL basée en F #, ce qui rend possible d’utiliser les bibliothèques .NET existantes si nécessaire.

Le langage de script est utilisé dépend de vos préférences et la configuration requise. Le [TaskyPro-Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash) exemple contient un exemple d’utilisation d’inclinaison comme un [générer le script](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile).


> [!NOTE]
> Il est possible d’utiliser un système de génération basé sur XML tels que MSBuild ou NAnt, mais ces manque la simplicité et la facilité de maintenance du DSL dédié à la création de logiciels.

### <a name="parameterizing-the-build-script"></a>Paramétrage du Script de compilation

Le processus de génération et de tester les logiciels nécessite des informations qui doivent être gardées secrètes. En particulier en créant un APK nécessiter un mot de passe pour le magasin de clés et/ou les alias de clé dans le magasin de clés. De même, Test Cloud nécessite une clé d’API qui est unique à un développeur. Ces types de valeurs doivent être pas durs codé dans le script de compilation. Au lieu de cela il doivent être passés en tant que variables au script de compilation.

Moins sensibles sont des valeurs telles que l’iOS ID d’appareil ou l’ID d’appareil Android qui identifient quel Test Cloud doit utiliser pour les appareils de test s’exécute. Ils ne sont pas des valeurs qui doivent être protégés, mais elles peuvent changer à partir de build à build.

Stockage de ces types de variables en dehors du script de compilation rend également plus facile de partager le script de compilation d’une organisation, avec les développeurs, par exemple. Les développeurs peuvent utiliser exactement le même script en tant que le serveur de builds, mais peuvent utiliser leur propre keystores et les clés API.

Il existe deux options possibles pour le stockage de ces valeurs sensibles :

- **Un fichier de configuration** : permet de protéger la clé de l’API de Test Cloud cette valeur ne doit pas être vérifiée dans le contrôle de version. Le fichier peut être créé pour chaque ordinateur. Comment les valeurs sont lues à partir de ce fichier dépendent du langage de script utilisé.

- **Variables d’environnement** : ils peuvent facilement être définis sur un ordinateur donné et le Custom quel que soit le langage de script sous-jacent.

Présente des avantages et inconvénients de chacun de ces choix. TeamCity fonctionne bien avec les variables d’environnement, afin que ce guide recommande cette technique lors de la création de scripts de compilation.

### <a name="build-steps"></a>Étapes de génération

Le script de génération doit être en mesure d’effectuer les étapes suivantes :

- **Compilez l’Application** : Cela inclut la signature de l’application avec le profil de configuration approprié.

- **Soumettre la demande à Xamarin Test Cloud** : Cela inclut la signature et zip alignement APK avec le magasin de clés appropriée.

Ces deux étapes seront expliquées plus en détail ci-dessous.

#### <a name="compiling-a-xamarinios-application"></a>La compilation d’une Application Xamarin.iOS

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>La compilation d’une Application Xamarin.Android

Pour compiler une application Android, utilisez **xbuild** (ou **msbuild** sur Windows) :

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Notez que pour compiler l’application Xamarin Android **xbuild** utilise le projet et pour générer l’application iOS **xbuild** exige que la solution.

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Envoi de Xamarin.UITests au Cloud de Test

UITests sont envoyées à l’aide de la `test-cloud.exe` application, comme indiqué dans l’extrait de code suivant :

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

Lorsque le test est exécuté, les résultats des tests seront affichera sous la forme d’un fichier XML de style NUnit appelé **report.xml**. TeamCity affiche les informations dans le journal de génération.

Pour plus d’informations sur comment soumettre UITests au Cloud de Test, consultez le guide de Xamarin sur [soumettant des Tests Test Cloud](https://developer.xamarin.com/guides/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/).

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Envoi de Tests Calabash pour tester le Cloud

Tests de Calabash sont envoyées à l’aide de la `test-cloud` marque, comme indiqué dans l’extrait de code suivant :

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```
Pour envoyer une application Android pour tester le Cloud, il est nécessaire de reconstruire tout d’abord que le serveur de test APK calabash-android à l’aide de :

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```
Pour plus d’informations sur la soumission des tests de Calabash, consultez le guide de Xamarin sur [soumettant des Tests Test Cloud Calabash](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/).

## <a name="creating-a-teamcity-project"></a>Création d’un projet TeamCity

Une fois que TeamCity est installé et que Visual Studio pour Mac peuvent générer votre projet, il est temps pour créer un projet dans TeamCity pour générer le projet et les soumettre au Test Cloud.

1. Démarré en vous connectant à TeamCity via le navigateur web. Accédez au projet racine :

    ![](teamcity-images/image2.png "Accédez au projet racine") sous le projet racine, créez un projet secondaire :

    ![](teamcity-images/image3.png "Accédez au projet sous la racine de projet racine, créez un nouveau projet secondaire")
2. Une fois que le projet a été créé, ajoutez une nouvelle Configuration de Build :

    ![](teamcity-images/image5.png "Une fois que le projet a été créé, ajoutez une nouvelle Configuration de Build")
3. Association d’un projet VCS à la Configuration de Build. Cette opération s’effectue via l’écran de paramètre de contrôle de Version :

    ![](teamcity-images/image6.png "Cette opération s’effectue via l’écran de paramètre de contrôle de Version")

    S’il n’existe aucun projet VCS créé, vous pouvez créer un à partir de la page nouvelle racine VCS indiquée ci-dessous :

    ![](teamcity-images/image7.png "S’il n’existe aucun projet VCS créé, vous pouvez créer un à partir de la page nouvelle racine VCS")

    Une fois que la racine VCS a été attachée, TeamCity va extraire le projet et réessayez automatiquement détectent les étapes de génération. Si vous êtes familiarisé avec TeamCity, vous pouvez sélectionner une des étapes de génération détectés. Il est possible d’ignorer les étapes de génération détecté pour l’instant.

4. Ensuite, configurez un déclencheur de Build. Cette file d’attente d’une build lorsque certaines conditions sont remplies, par exemple quand un utilisateur valide le code dans le référentiel. La capture d’écran suivante montre comment ajouter un déclencheur de build :

    ![](teamcity-images/image8.png "Cette capture d’écran montre comment ajouter un déclencheur de build") un exemple de configuration d’un déclencheur de génération peut être consulté dans la capture d’écran suivante :

    ![](teamcity-images/image9.png "Un exemple de configuration d’un déclencheur de génération peut être consulté dans cette capture d’écran")

5. La section précédente, le Script de génération, le paramétrage recommandé de stocker des valeurs en tant que variables d’environnement. Ces variables peuvent être ajoutées à la configuration de build via l’écran Paramètres. Ajoutez les variables pour la clé d’API de Test Cloud, l’ID de périphérique iOS et l’ID d’appareil Android comme indiqué dans la capture d’écran ci-dessous :

    ![](teamcity-images/image11.png "Ajoutez les variables pour l’ID d’appareil Android, l’ID de périphérique iOS et la clé d’API de Test Cloud")

6. L’étape finale consiste à ajouter une étape de génération qui appelle le script de compilation pour compiler l’application et la file d’attente l’application Test Cloud. La capture d’écran suivante est un exemple d’une étape de génération qui utilise un Rakefile pour générer une application :

    ![](teamcity-images/image12.png "Cette capture d’écran est un exemple d’une étape de génération qui utilise un Rakefile pour générer une application")

7. À ce stade, la configuration de build est terminée. Il est judicieux de déclencher une build pour vérifier que le projet est correctement configuré. Une bonne solution pour ce faire est de valider une modification de petite taille, non significatif dans le référentiel. TeamCity doit détecter la validation et démarrer une build.

8. Une fois la build terminée, examinez le journal de génération et s’il existe des problèmes avec la build, les avertissements qui requièrent votre attention.

## <a name="summary"></a>Récapitulatif

Ce guide expliqué comment TeamCity permet de générer des applications mobiles de Xamarin et les envoyer au Test Cloud. Nous avons abordé la création d’un script de compilation pour automatiser le processus de génération. Prend en charge de la compilation de l’application, l’envoi au Cloud de Test et attend les résultats du script de compilation

Ensuite, nous avons abordé la création d’un projet dans TeamCity file d’attente une build chaque fois qu’un développeur valide le code qui appelle le script de compilation.

## <a name="related-links"></a>Liens associés

- [Envoi de Tests à Xamarin Test Cloud (UITest)](https://developer.xamarin.com~/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/)
- [Envoi de Tests à Xamarin Test Cloud (Calabash)](https://developer.xamarin.com~/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/)
- [Installation et configuration TeamCity](http://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
