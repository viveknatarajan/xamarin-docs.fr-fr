---
title: Résolution des erreurs d’Installation de bibliothèque
description: Dans certains cas, vous obtiendrez des erreurs lors de l’installation des bibliothèques de prise en charge Android. Ce guide fournit des solutions de contournement pour certaines erreurs courantes.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2018
ms.openlocfilehash: a54c69ff708ff7438ef1a8fd14c17e77b5375039
ms.sourcegitcommit: f52aa66de4d07bc00931ac8af791d4c33ee1ea04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="resolving-library-installation-errors"></a>Résolution des erreurs d’Installation de bibliothèque

_Dans certains cas, vous obtiendrez des erreurs lors de l’installation des bibliothèques de prise en charge Android. Ce guide fournit des solutions de contournement pour certaines erreurs courantes._

## <a name="overview"></a>Vue d'ensemble

Lors de la génération d’un projet d’application de Xamarin.Android, vous pouvez obtenir des erreurs de build lorsque Visual Studio ou Visual Studio pour Mac essayez de télécharger et installer les bibliothèques de dépendance. Nombre de ces erreurs sont dus à des problèmes de connectivité réseau, l’endommagement du fichier ou des problèmes de contrôle de version. Ce guide décrit les erreurs d’installation de bibliothèque prise en charge plus courantes et fournit les étapes pour résoudre ces problèmes et obtenir votre projet d’application réexécuter la génération. 

 
 
## <a name="errors-while-downloading-m2repository"></a>Erreurs lors du téléchargement de m2Repository

Vous pouvez voir **m2repository** erreurs lors du référencement d’un package NuGet des services des bibliothèques de prise en charge Android ou Google Play. Le message d’erreur est semblable au suivant :

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

Cet exemple concerne **android\_m2repository\_r16**, mais vous pouvez voir ce message d’erreur même pour une autre version, tel que **android\_m2repository\_r18**  ou **android\_m2repository\_r25**. 



### <a name="automatic-recovery-from-m2repository-errors"></a>Récupération automatique à partir d’erreurs de m2repository 

Souvent, ce problème peut être résolu par la suppression de la bibliothèque problématique et de reconstruction en fonction de ces étapes : 

1. Accédez au répertoire de bibliothèque de prise en charge sur votre ordinateur :

    -   Sous Windows, les bibliothèques de prise en charge sont trouvent dans **C:\\utilisateurs\\_nom d’utilisateur_\\AppData\\Local\\Xamarin**. 

    -   Sur Mac OS X, les bibliothèques de prise en charge sont trouvent dans **/Users/_nom d’utilisateur_/.local/share/Xamarin**. 

2. Recherchez le dossier de bibliothèque et la version correspondant au message d’erreur. Par exemple, le dossier de bibliothèque et la version pour le message d’erreur ci-dessus se trouve à **Android.Support.v4\\22.2.1**:

    [![Emplacement du dossier exemple pour 22.2.1 prend en charge la bibliothèque](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. Supprimez le contenu du dossier de version. Veillez à supprimer la **.zip** fichier, ainsi que les **contenu** et **incorporé** sous-répertoires de ce dossier. Pour l’exemple de message d’erreur ci-dessus, les fichiers et sous-répertoires indiqués dans cette capture d’écran (**contenu**, **incorporé**, et **android_m2repository_r16.zip**) sont à Supprimer :

    [![Contenu d’exemple de 22.2.1 prend en charge le dossier de bibliothèque](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   Notez qu’il est important de supprimer la *entière* contenu de ce dossier. Bien que ce dossier peut contenir initialement le « manquant » **android\_m2repository\_r16.zip** fichier, ce fichier peut avoir été partiellement téléchargés ou endommagé.

4. Régénérez le projet &ndash; cela entraîne le processus de génération de nouveau télécharger la bibliothèque manquante.

Dans la plupart des cas, ces étapes seront résoudre l’erreur de génération et vous permettent de continuer. Si vous supprimez cette bibliothèque ne résout pas l’erreur de build, vous devez télécharger et installer manuellement le **android\_m2repository\_r_nn_.zip** comme décrit dans la section suivante du fichier. 



### <a name="manually-downloading-m2repository"></a>Téléchargement manuel m2repository

Si vous avez essayé à l’aide de la procédure de récupération automatique ci-dessus et que vous avez toujours des erreurs de build, vous pouvez télécharger manuellement les **android\_m2repository\_r_nn_.zip** fichier (à l’aide d’un navigateur web) et l’installer en fonction de les étapes suivantes. Cette procédure est également utile si vous n’avez pas d’accès à internet sur votre ordinateur de développement, mais vous êtes en mesure de télécharger l’archive à l’aide d’un autre ordinateur. 

1.  Téléchargez le **android\_m2repository\_r_nn_.zip** fichier qui correspond au message d’erreur &ndash; des liens sont fournis dans la liste suivante (ainsi que le hachage MD5 correspondant de chaque lien URL) :

    -   [Android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    -   [Android\_m2repository\_r32.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    -   [Android\_m2repository\_r31.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    -   [Android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    -   [Android\_m2repository\_r29.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    -   [Android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    -   [Android\_m2repository\_r27.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    -   [Android\_m2repository\_r26.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    -   [Android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    -   [Android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    -   [Android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    -   [Android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    -   [Android\_m2repository\_r21.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    -   [Android\_m2repository\_r20.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    -   [Android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    -   [Android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    -   [Android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    -   [Android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    Si le **m2repository** archive ne figure pas dans ce tableau, vous pouvez créer l’URL de téléchargement en ajoutant le préfixe **https://dl-ssl.google.com/android/repository/** au nom de la **m2repository** à télécharger. Par exemple, utilisez  **https://dl-ssl.google.com/android/repository/android \_m2repository\_r10.zip** télécharger **android\_m2repository\_r10.zip**.

2.  Renommez le fichier pour le hachage MD5 correspondant de l’URL de téléchargement, comme indiqué dans le tableau ci-dessus. Par exemple, si vous avez téléchargé **android\_m2repository\_r25.zip**, renommez-le **0B3F1796C97C707339FB13AE8507AF50.zip**. Si le hachage MD5 pour l’URL de téléchargement du fichier téléchargé n’est pas affiché dans la table, vous pouvez utiliser un [en ligne MD5 Générateur](http://www.webconfs.com/online-md5-generator.php) pour convertir l’URL en une chaîne de hachage MD5. 

3.  Copiez le fichier dans le Xamarin **compresse** dossier : 

    -   Sous Windows, ce dossier se trouve dans **C:\\utilisateurs\\***nom d’utilisateur***\\AppData\\Local\\Xamarin\\compresse**. 

    -   Sur Mac OS X, ce dossier se trouve dans **/Users/***nom d’utilisateur***/.local/share/Xamarin/zips**. 

    Par exemple, la capture d’écran suivante illustre le résultat lorsque **android\_m2repository\_r16.zip** est téléchargé et renommé le hachage MD5 de son URL de téléchargement sur Windows :

    [![Exemple du référentiel r16.zip est renommé 0595E577D19D31708195A83087881EE6.zip](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)


Si cette procédure ne résout pas l’erreur de build, vous devez télécharger manuellement les **android\_m2repository\_r_nn_.zip** de fichiers, le décompresser et installer son contenu comme décrit dans la section suivante. 


### <a name="manually-downloading-and-installing-m2repository-files"></a>Manuellement télécharger et installer les fichiers m2repository

Le processus entièrement manuel de récupération à partir de **m2repository** entraîne des erreurs du téléchargement le **android\_m2repository\_r_nn_.zip** fichier (à l’aide d’un navigateur web), décompression Il et copier son contenu dans le répertoire de bibliothèque de prise en charge sur votre ordinateur. Dans l’exemple suivant, nous allons restaurer à partir de ce message d’erreur : 

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

Utilisez les étapes suivantes pour télécharger **m2repository** et installer son contenu :

1.  Supprimez le contenu du dossier de bibliothèque correspondant au message d’erreur. Par exemple, dans le message d’erreur ci-dessus supprimer le contenu de **C:\\utilisateurs\\***nom d’utilisateur***\\AppData\\Local\\Xamarin\\ Android.Support.v4\\23.1.1.0**. 
    Comme décrit précédemment, vous devez supprimer tout le contenu de ce répertoire :

    [![Suppression du contenu, incorporé et les dossiers android_m2repository le 23.1.1.0 dossier](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2.  Téléchargez le **android\_m2repository\_r_nn_.zip** fichier à partir de Google qui correspond à l’erreur de message (voir le tableau dans la section précédente pour les liens).

3.  Extraire cet **.zip** archive à n’importe quel emplacement (tel que le bureau). Il doit créer un répertoire qui correspond au nom de la **.zip** archive. Dans ce répertoire, vous devez trouver un sous-répertoire appelé **m2repository**: 

    [![dossier m2repository trouvé dans l’archive zip extraits](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4.  Dans le répertoire des bibliothèques avec version qui vous purgées à l’étape 1, recréer les **contenu** et **embedded** dans les sous-répertoires. Par exemple, la capture d’écran suivante illustre **contenu** et **incorporé** sous-répertoires en cours de création dans le **23.1.1.0** dossier **android \_m2repository\_r25.zip**: 

    [![Créer du contenu et dossiers incorporées dans le 23.1.1.0 dossier](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5.  Copie **m2repository** à partir de l’extrait **.zip** dans les **contenu** active que vous avez créé à l’étape précédente : 

    [![Capture d’écran de m2repository copiés dans le dossier de 23.1.1.0/content](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6.  Dans l’extrait **.zip** active, cliquez sur Parcourir pour **m2repository\\com\\android\\prennent en charge\\prise en charge-v4** et ouvrez le dossier correspondant le numéro de version créé précédemment (dans cet exemple, **23.1.1**) :

    [![Liste des fichiers contenus dans le dossier support-v4/23.1.1 exemple](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7.  Copiez tous les fichiers dans ce dossier pour le **incorporé** répertoire créé à l’étape 4 :

    [![Exemple de fichiers copiés dans le dossier 23.1.1.0/embedded](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8.  Vérifiez que tous les fichiers sont copiés. Le **incorporé** active doit maintenant contenir des fichiers tels que **.jar**, **.aar**, et **.pom**.

9.  Décompressez le contenu de tous les extraits **.aar** les fichiers vers le **incorporé** active. Sous Windows, ajoutez un **.zip** extension pour le **.aar** de fichier, ouvrez-le et copiez le contenu à la **incorporé** active.
    Sur macOS, décompressez le **.aar** fichier à l’aide de la **décompresser** dans le Terminal (par exemple, **décompresser file.aar**).

À ce stade, vous avez installé manuellement les composants manquants et votre projet doit être généré sans erreurs. Dans le cas contraire, vérifiez que vous avez téléchargé le **m2repository** **.zip** archiver la version qui correspond exactement à la version du message d’erreur, puis vérifiez que vous avez installé son contenu dans le Corrigez les emplacements comme décrit dans les étapes ci-dessus. 



## <a name="summary"></a>Récapitulatif 

Cet article a expliqué comment récupérer les erreurs courantes qui peuvent avoir lieu pendant le téléchargement automatique et l’installation des bibliothèques de dépendance. Il décrit comment supprimer la bibliothèque problématique et régénérez le projet comme un moyen de retélécharger et réinstaller la bibliothèque. Il décrit comment télécharger la bibliothèque et l’installer dans le **compresse** dossier. Il décrit également une procédure plus complexe pour télécharger et installer les fichiers nécessaires afin de contourner les problèmes qui ne peut pas être résolus via automatique manuellement. 
