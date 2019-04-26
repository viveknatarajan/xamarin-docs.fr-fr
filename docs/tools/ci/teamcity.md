---
title: À l’aide de la ville de l’équipe avec Xamarin
description: Ce guide explique les étapes nécessaires à l’utilisation de TeamCity pour compiler des applications mobiles et envoyez-les à Xamarin Test Cloud.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: lobrien
ms.author: laobri
ms.date: 03/23/2017
ms.openlocfilehash: a3cb79f33b64d933aa8ab4d3555479cc16238992
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61218950"
---
# <a name="using-team-city-with-xamarin"></a>À l’aide de la ville de l’équipe avec Xamarin

_Ce guide explique les étapes nécessaires à l’utilisation de TeamCity pour compiler des applications mobiles et envoyez-les à Xamarin Test Cloud._

Comme indiqué dans le [Introduction à l’intégration continue](~/tools/ci/intro-to-ci.md) guide, l’intégration continue (CI) est une pratique utile lors du développement d’applications mobiles de qualité. Il existe plusieurs options viables pour les logiciels de serveurs d’intégration continue ; Ce guide met l’accent sur [TeamCity](http://www.jetbrains.com/teamcity/) de JetBrains.

Il existe plusieurs permutations différentes d’une installation de TeamCity. Voici une liste de certains d'entre eux :

- **Windows Service** : dans ce scénario, TeamCity démarre lorsque Windows démarre comme un Service Windows. Il doit être associé à un hôte de Build Mac pour compiler des applications iOS.

- **Lancer le démon sur OS X** – point de vue conceptuel, ceci est très semblable à l’exécution sous un Service Windows à l’étape précédente. Par défaut les builds seront exécutera sous le compte racine.

- **Compte d’utilisateur sur OS X** – il est possible d’exécuter TeamCity sous un compte d’utilisateur qui démarre chaque fois que l’utilisateur se connecte.

Les scénarios précédents, en cours d’exécution TeamCity sous un compte d’utilisateur sur OS X est la plus simple et plus facile du programme d’installation.

Il existe plusieurs étapes impliquées dans la définition de TeamCity :

- **L’installation de TeamCity** – l’installation de TeamCity n’est pas abordée dans ce guide. Ce guide suppose que TeamCity est installé et en cours d’exécution sous un compte d’utilisateur. Obtenir des instructions sur [installation TeamCity](http://confluence.jetbrains.com/display/TCD8/Installation) se trouve dans le [TeamCity 8 documentation](http://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) par JetBrains.

- **Préparation du serveur de Build** : cette étape consiste à installer le logiciel nécessaire, outils, et les certificats requis pour créer des applications mobiles et les préparer pour la distribution.

- **Création d’un générer le Script** : cette étape n’est pas strictement nécessaire, mais un script de compilation est une aide utile pour la création d’applications sans assistance. À l’aide d’un script de compilation aideront à résoudre les problèmes de build qui peuvent survenir et offre un moyen de cohérent et reproductible pour créer les fichiers binaires pour la distribution, même si l’intégration continue n’est pas utilisée.

- **Création d’un projet de TeamCity** – une fois les trois étapes précédentes terminées, nous devons créer un projet de TeamCity qui contiendra toutes les métadonnées nécessaires pour récupérer le code source, compiler les projets et soumettre les tests à Xamarin Test Cloud.

## <a name="requirements"></a>Configuration requise

Expérience avec [Xamarin Test Cloud](https://developer.xamarin.com/guides/testcloud) est requis.

Vous êtes familiarisé avec TeamCity 8.1 est requis. L’installation de TeamCity est abordée dans ce document. Il est supposé que TeamCity est installé sur OS X Mavericks et qu’il s’exécute sous un compte d’utilisateur normal et non le compte racine.

Le serveur de build doit être un ordinateur autonome qui exécute OS X, qui est dédié à l’intégration continue. Dans l’idéal, le serveur de builds ne sera pas chargé pour tous les autres rôles, comme un serveur de base de données, un serveur web ou une station de travail de développeur.

> [!IMPORTANT]
> Ce guide ne couvre pas une installation « headless » de Xamarin.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>Préparation du serveur de Build

Une étape essentielle dans la configuration d’un serveur de builds consiste à installer l’ensemble des outils nécessaires, des logiciels et des certificats pour créer des applications mobiles. Il est important que le serveur de builds pouvoir compiler la solution mobile et exécuter des tests. Pour réduire les problèmes de configuration, les logiciels et les outils doivent être installés dans le même compte d’utilisateur qui héberge TeamCity. Voici une liste de ce qui est nécessaire :

1. **Visual Studio pour Mac** : Cela inclut Xamarin.iOS et Xamarin.Android.
2. **Connexion pour le Store du composant Xamarin** – il s’agit d’une étape facultative et est uniquement nécessaire si votre application utilise des composants à partir du magasin du composant Xamarin. Journalisation proactive dans le magasin de composants à ce stade empêchera des problèmes quand une build de TeamCity tente de compiler l’application.
3. **Xcode** – Xcode est nécessaire pour la compilation et l’authentification des applications iOS.
4. **Outils de ligne de commande Xcode** : cette opération est décrite à l’étape 1 de la section Installation de le [la mise à jour de Ruby avec rbenv](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) guide.
5. **Signature d’identité et profils de provisionnement** : importer les certificats et configuration du profil via XCode. Consultez le guide d’Apple sur [exporter les identités de signature et de profils de provisionnement](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) pour plus d’informations.
6. **Android Keystores** : copier les keystores Android requis dans un répertoire que l’utilisateur de TeamCity a accès, par exemple, `~/Documents/keystores/MyAndroidApp1`.
7. **Calabash** – cette étape est facultative si votre application comporte des tests écrits à l’aide de Calabash. Consultez le [Calabash de l’installation sur OS X Mavericks](https://developer.xamarin.com/guides/testcloud/calabash/osx-installation/) guide et le [la mise à jour de Ruby avec rbenv](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) guide pour plus d’informations.

Le diagramme suivant illustre tous ces composants :

![](teamcity-images/image1.png "Ce diagramme illustre tous ces composants")

Une fois que tous les logiciels a été installé, connectez-vous au compte d’utilisateur et vérifiez que tous les logiciels a été correctement installé et qu’il fonctionne. Il doit inclure la solution de compilation et de l’envoi de l’application vers Test Cloud. Cela peut être considérablement simplifié en exécutant le script de génération, comme décrit dans la section suivante.

## <a name="create-a-build-script"></a>Créer un Script de Build

Bien qu’il soit tout à fait possible de TeamCity gérer tous les aspects de la compilation et de soumettre les applications mobiles vers Test Cloud par lui-même, il est fortement recommandé pour créer un script de génération. Un script de génération offre les avantages suivants :

1. **Documentation** – un script de compilation sert à un formulaire de la documentation sur la façon dont le logiciel est construit. Cette opération supprime certaines des « magiques » qui est associé avec le déploiement de l’application et permet aux développeurs de se concentrer sur les fonctionnalités.
1. **La répétabilité** – un script de génération garantit que chaque fois que l’application est compilée et déployée, il se produit dans exactement la même façon, quel que soit le qui ou ce que fait le travail. Cette cohérence reproductible supprime tous les problèmes ou erreurs qui peuvent glissent généralement en raison d’une build exécutée de manière incorrecte ou erreur humaine.
1. **Le contrôle de version** – un script de génération peut être inclus dans le système de contrôle de code source. Cela signifie que les modifications au script de génération peuvent être suivies, surveillées et corrigées si les erreurs ou des imprécisions sont trouvées.
1. **Préparer l’environnement** – un script de build peut inclure une logique pour installer un obligatoire 3e dépendances tierces. Cela garantit que les applications sont générées avec les composants appropriés.

Le script de génération peut être aussi simple qu’un fichier Powershell (sur Windows) ou un script bash (sur OS X). Lorsque vous créez le script de génération, il existe plusieurs options pour les langages de script :

- [**RAKE** ](https://github.com/jimweirich/rake) – il s’agit un langage spécifique domaine (DSL) pour la création de projets, basés sur Ruby. RAKE a l’avantage de la popularité et un riche écosystème de bibliothèques.

- [**psake** ](https://github.com/psake/psake) – il s’agit d’une bibliothèque de Windows Powershell pour la création de logiciels

- [**FALSIFIER** ](http://fsharp.github.io/FAKE/) – il s’agit d’un DSL en dans F# ce qui rend possible d’utiliser les bibliothèques .NET existantes si nécessaire.

Le langage de script est utilisé dépend de vos exigences et préférences. Le [TaskyPro-Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash) exemple contient un exemple d’utilisation Rake comme un [générer le script](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile).

> [!NOTE]
> Il est possible d’utiliser un système de génération basé sur XML tels que MSBuild ou NAnt, mais ces manque l’expressivité et la facilité de maintenance d’une solution DSL qui est dédiée à la création de logiciels.

### <a name="parameterizing-the-build-script"></a>Paramétrer le Script de génération

Le processus de génération et de test de logiciels requiert des informations qui doivent être gardées secrètes. Création d’un APK en particulier peut-être nécessiter un mot de passe pour le magasin de clés et/ou l’alias de clé dans le magasin de clés. De même, Test Cloud nécessite une clé d’API qui est unique à un développeur. Ces types de valeurs doivent être pas durs codé dans le script de génération. Au lieu de cela ils doivent être passés en tant que variables au script de génération.

Moins sensibles sont des valeurs telles qu’iOS ID d’appareil ou l’ID d’appareil Android qui identifient quel Test Cloud doit utiliser pour les appareils de test s’exécute. Il s’agit pas de valeurs qui doivent être protégés, mais elles peuvent changer à partir d’une build à une autre.

Stockage de ces types de variables en dehors du script de compilation rend également plus facile de partager le script de génération d’une organisation, avec les développeurs, par exemple. Les développeurs peuvent utiliser exactement le même script en tant que le serveur de builds, mais peuvent utiliser leur propre keystores et les clés API.

Il existe deux options possibles pour le stockage de ces valeurs sensibles :

- **Un fichier de configuration** : permet de protéger la clé d’API de Cloud de Test cette valeur ne doit pas être vérifiée dans le contrôle de version. Le fichier peut être créé pour chaque ordinateur. Comment les valeurs sont lues à partir de ce fichier varie selon le langage de script utilisé.

- **Variables d’environnement** : ils peuvent être facilement définis par ordinateur et artagés sur quelle que soit le langage de script sous-jacent.

Il existe des avantages et inconvénients de chacun de ces choix. TeamCity fonctionne bien avec les variables d’environnement, afin que ce guide vous recommande de cette technique lors de la création de scripts de compilation.

### <a name="build-steps"></a>Étapes de génération

Le script de génération doit être en mesure d’effectuer les étapes suivantes :

- **Compilez l’Application** : Cela inclut la signature de l’application avec le profil de provisionnement approprié.

- **Envoyer l’Application vers Xamarin Test Cloud** : Cela inclut la signature et zip alignement de l’APK avec le magasin de clés appropriée.

Ces deux étapes sont expliquées plus en détail ci-dessous.

#### <a name="compiling-a-xamarinios-application"></a>Compilation d’une Application Xamarin.iOS

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Compilation d’une Application Xamarin.Android

Pour compiler une application Android, utilisez **xbuild** (ou **msbuild** sur Windows) :

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Notez que pour compiler l’application Xamarin Android **xbuild** utilise le projet et pour générer l’application iOS **xbuild** exige que la solution.

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Envoi de Xamarin.UITests vers Test Cloud

Tests UITest est soumises à l’aide de la `test-cloud.exe` application, comme indiqué dans l’extrait de code suivant :

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

Lorsque le test est exécuté, les résultats des tests seront affichera sous la forme d’un fichier XML de style NUnit appelé **report.xml**. TeamCity affiche les informations dans le journal de génération.

Pour plus d’informations sur la soumission de tests UITest vers Test Cloud, consultez ce guide sur [Xamarin.UITests de préparation pour le chargement](/appcenter/test-cloud/preparing-for-upload/uitest/).

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Envoi de Tests Calabash pour tester le Cloud

Tests de Calabash sont soumises à l’aide de la `test-cloud` gem, comme indiqué dans l’extrait suivant :

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```
Pour soumettre une application Android vers Test Cloud, il est nécessaire de reconstruire tout d’abord le serveur de test APK à l’aide de calabash-android :

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```
Pour plus d’informations sur la soumission de tests de Calabash, veuillez consulter les guide de Xamarin sur [soumission de Tests Calabash vers Test Cloud](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/).

## <a name="creating-a-teamcity-project"></a>Création d’un projet TeamCity

Une fois que TeamCity est installé et Visual Studio pour Mac peut générer votre projet, il est temps de créer un projet dans TeamCity pour générer le projet et l’envoyer vers Test Cloud.

1. Démarré en vous connectant à TeamCity via le navigateur web. Accédez à la racine de projet :

    ![Naviguer vers le projet racine](teamcity-images/image2.png "naviguer vers le projet racine") sous le projet racine, créez un nouveau projet secondaire :

    ![Accédez à la racine de projet en dessous le projet racine, créez un nouveau sous-projet de](teamcity-images/image3.png "Navigate pour le projet en dessous la racine de projet racine, créez un nouveau projet de sous-section")
2. Une fois que le projet a été créé, ajoutez une nouvelle Configuration de Build :

    ![Une fois que le projet a été créé, ajoutez une nouvelle Configuration de Build](teamcity-images/image5.png "une fois que le projet a été créé, ajoutez une nouvelle Configuration de Build")
3. Attacher un projet VCS à la Configuration de Build. Cette opération est effectuée par le biais de l’écran de paramètre de contrôle de Version :

    ![Cette opération est effectuée par le biais de l’écran de paramètre de contrôle de Version](teamcity-images/image6.png "cette opération est effectuée par le biais de l’écran de paramètre de contrôle de Version")

    S’il n’existe aucun projet VCS créé, vous pouvez créer un à partir de la page nouvelle racine VCS indiquée ci-dessous :

    ![S’il n’existe aucun projet VCS créé, vous pouvez créer un à partir de la page nouvelle racine VCS](teamcity-images/image7.png "s’il n’existe aucun projet VCS créé, vous pouvez créer un à partir de la page nouvelle racine VCS")

    Une fois que la racine VCS a été attachée, TeamCity va extraire le projet et réessayez automatiquement détectent des étapes de génération. Si vous êtes familiarisé avec TeamCity, vous pouvez sélectionner une des étapes de génération détectés. Il est possible d’ignorer les étapes de génération détecté pour l’instant.

4. Ensuite, configurez un déclencheur de Build. Cette file d’attente d’une build lorsque certaines conditions sont remplies, par exemple quand un utilisateur valide le code dans le référentiel. La capture d’écran suivante montre comment ajouter un déclencheur de build :

    ![Cette capture d’écran montre comment ajouter un déclencheur de build](teamcity-images/image8.png "cette capture d’écran montre comment ajouter un déclencheur de build") un exemple de configuration d’un déclencheur de build peut être consulté dans la capture d’écran suivante :

    ![Un exemple de configuration d’un déclencheur de build peut être consulté dans cette capture d’écran](teamcity-images/image9.png "un exemple de configuration d’un déclencheur de build peut être consulté dans cette capture d’écran")

5. La section précédente, en paramétrant la générer le Script, recommandé de stocker certaines valeurs en tant que variables d’environnement. Ces variables peuvent être ajoutées à la configuration de build via l’écran Paramètres. Ajoutez les variables pour la clé d’API de Test Cloud, l’ID d’appareil iOS et l’ID d’appareil Android comme indiqué dans la capture d’écran ci-dessous :

    ![Ajoutez les variables pour la clé d’API de Test Cloud, l’ID d’appareil iOS et l’ID d’appareil Android](teamcity-images/image11.png "ajouter les variables pour la clé d’API de Test Cloud, l’ID d’appareil iOS et l’ID d’appareil Android")

6. L’étape finale consiste à ajouter une étape de génération qui appelle le script de build pour compiler l’application et la file d’attente l’application vers Test Cloud. La capture d’écran suivante est un exemple d’une étape de build qui utilise un Rakefile pour générer une application :

    ![Cette capture d’écran est un exemple d’une étape de build qui utilise un Rakefile pour générer une application](teamcity-images/image12.png "cette capture d’écran est un exemple d’une étape de build qui utilise un Rakefile pour générer une application")

7. À ce stade, la configuration de build est terminée. Il est judicieux de déclencher une build pour vérifier que le projet est configuré correctement. Un bon moyen de procéder consiste à valider une modification de petite taille, non significatif dans le référentiel. TeamCity doit détecter la validation et démarrer une build.

8. Une fois la build terminée, examinez le journal de génération et s’il existe des problèmes ou des avertissements à la build qui nécessitent votre attention.

## <a name="summary"></a>Récapitulatif

Ce guide a décrit l’utilisation de TeamCity pour créer des applications Xamarin Mobile puis de les envoyer vers Test Cloud. Nous avons abordé la création d’un script de build pour automatiser le processus de génération. Le script de génération s’occupe de la compilation de l’application, l’envoi vers Test Cloud et attend les résultats

Ensuite, nous avons vu comment créer un projet dans TeamCity file d’attente une build chaque fois qu’un développeur valide le code qui appellera le script de génération.

## <a name="related-links"></a>Liens associés

- [Préparation Xamarin.UITests pieds téléchargement](/appcenter/test-cloud/preparing-for-upload/uitest/)
- [Installation et configuration de TeamCity](http://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
