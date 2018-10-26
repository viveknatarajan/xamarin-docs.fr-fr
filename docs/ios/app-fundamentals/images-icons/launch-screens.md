---
title: Écrans de lancement pour les applications Xamarin.iOS
description: Cet article explique comment créer une écran de lancement d’application pour tous les appareils iOS, à la résolution et l’orientation, à l’aide d’un Storyboard unifiée unique.
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2018
ms.openlocfilehash: 0ec1defa29a4fe85c4ae3e809d8733e68cc268ac
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116919"
---
# <a name="launch-screens-for-xamarinios-apps"></a>Écrans de lancement pour les applications Xamarin.iOS

_Cet article explique comment créer une écran de lancement d’application pour tous les appareils iOS, à la résolution et l’orientation, à l’aide d’un Storyboard unifiée unique._

Avant d’iOS 8, la création d’un écran de lancement pour une application iOS requis au développeur de fournir une ressource d’image pour chacun des différents facteurs de forme de périphérique et résolutions dans lequel l’application peut s’exécuter. Toutefois, depuis la version iOS 8, il a été possible d’utiliser un Storyboard unifiée unique pour créer un écran de lancement qui semble correct dans tous les cas.

Cette courte procédure décrit comment créer un écran de lancement avec soit une table de montage séquentiel fournie par défaut dans un nouveau projet, ou avec un Storyboard ajoutés manuellement à un projet existant. Il montre ensuite comment utiliser le concepteur iOS pour ajouter une vue de l’Image et une étiquette à la table de montage séquentiel pour définir des contraintes sur ces vues et pour vérifier que la table de montage séquentiel semble correct pour différents appareils et les orientations.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>La gestion des écrans de lancement avec des Storyboards

Dans iOS 8 (et versions ultérieures), le développeur peut créer un Storyboard unifiée spécial pour fournir l’écran de lancement au lieu d’utiliser une ou plusieurs images de lancement statique. Lorsque vous créez un table de montage séquentiel de lancement dans le concepteur iOS, utiliser les Classes de taille et de la disposition automatique pour définir différentes dispositions pour les environnements d’affichage différents. À l’aide des Classes de taille et de disposition automatique, le développeur peut créer un écran de lancement unique qui vous semble correct sur tous les appareils et afficher les environnements.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

1. Dans Visual Studio pour Mac, créez un nouveau projet en sélectionnant **fichier > Nouvelle Solution** , puis en choisissant **application avec affichage unique**: 

    ![La fenêtre Nouveau projet, avec une application avec affichage unique sélectionné](launch-screens-images/launch01.png)

    - Par défaut, un nouveau projet comprend un **LaunchScreen.storyboard** fichier qui définit l’interface de l’écran de lancement. 
    - Pour ajouter à la place un Storyboard écran lancer à un projet existant, cliquez sur le nom du projet dans le **panneau solutions** et choisissez **Ajouter > nouveau fichier...**  , puis sélectionnez **écran de lancement**:

    ![La fenêtre Nouveau fichier, avec iOS écran de lancement sélectionné](launch-screens-images/launch01b.png)

    - Nommez le fichier **LaunchScreen** ou un autre nom de votre choix.

2. Configurer le projet pour utiliser la table de montage séquentiel approprié pour son écran de lancement :

    - Double-cliquez sur le **Info.plist** de fichiers dans le **panneau solutions** à ouvrir pour modification.
    - Dans le **Images de lancement** section, assurez-vous que l’option **écran de lancement** est définie sur le nom de la table de montage séquentiel approprié :

    ![Le sélecteur d’écran de lancement dans Info.plist](launch-screens-images/launch02.png)

    - Par défaut, un nouveau projet est configuré pour utiliser **LaunchScreen.storyboard** en tant que son écran de lancement.

3. Ajouter une image à la **Assets.xcassets** ressource catalogue afin qu’il soit disponible pour une utilisation sur l’écran de lancement. Pour plus d’informations, consultez le [Ajout d’Images à une ressource catalogue Image défini](~/ios/app-fundamentals/images-icons/displaying-an-image.md) section de la [affichage d’une Image](~/ios/app-fundamentals/images-icons/displaying-an-image.md) guide.

4. Ouvrez **LaunchScreen.storyboard** pour le modifier en double-cliquant dessus dans le **panneau solutions**.

5. Choisissez un appareil et l’orientation sur lequel afficher un aperçu de l’animation d’écran de lancement dans le concepteur iOS. Ouvrez le panneau de sélection de périphérique dans la barre d’outils en bas et sélectionnez **4 s iPhone** et **Portrait**.

    ![La barre d’outils de sélection de périphérique](launch-screens-images/launch05.png)

    - Notez que la sélection d’un périphérique et l’orientation modifie uniquement la façon dont le concepteur iOS affiche un aperçu de la conception. Quelle que soit la sélection effectuée ici, qui vient d’être ajouté les contraintes sont appliquées dans l’ensemble des appareils et des orientations, sauf si le **modifier les caractéristiques** bouton a été utilisé pour spécifier dans le cas contraire. 

6. Définir le **arrière-plan** couleur de la vue principale du contrôleur d’affichage. Sélectionnez la vue en cliquant sur au milieu du contrôleur d’affichage et ajuster la couleur d’arrière-plan en utilisant la **panneau Propriétés**:

    ![Une vue unique avec une couleur d’arrière-plan violet](launch-screens-images/launch06.png)

7. Ajouter un **affichage de l’Image** à l’écran de lancement et définir sa source **Image**:

    - Faites glisser un **affichage de l’Image** à partir de la **panneau boîte à outils** au centre de la vue.
    - Avec le **affichage de l’Image** sélectionné, dans le **Widget** section de la **panneau Propriétés** définir le **Image** propriété à l’Image déjà défini ajouté à la **Assets.xcassets** catalogue de composants. Repositionner et redimensionner le **affichage de l’Image** selon les besoins :
    
    ![Affichage d’une Image avec son jeu de propriétés d’Image](launch-screens-images/launch07.png)

8. Ajouter un **étiquette** ci-dessous le **affichage de l’Image** et utiliser le **panneau Propriétés** pour définir ses attributs : 

    ![Une étiquette dont la valeur texte et la couleur](launch-screens-images/launch08.png)

9. Basculer vers le Mode d’édition de contrainte en utilisant le bouton droit dans le **barre d’outils de contraintes**:
    
    ![Le bouton du Mode d’édition de contrainte](launch-screens-images/launch09.png)

10. Ajouter des contraintes à la **affichage de l’Image**, en définissant sa hauteur et sa largeur et centrant horizontalement et verticalement :

    ![Affichage d’une Image avec des contraintes de mise en page](launch-screens-images/launch10.png)

    - Pour plus d’informations sur l’ajout de contraintes, consultez [disposition automatique avec le Concepteur de Xamarin pour iOS](~/ios/user-interface/designer/designer-auto-layout.md).

11. Ajouter des contraintes à la **étiquette**centrant horizontalement, en lui attribuant une hauteur et une largeur et positionnement fixe distance verticalement à partir de la **affichage de l’Image**:

    ![Une étiquette avec les contraintes de disposition](launch-screens-images/launch11.png)

12. Autres périphériques et les orientations pour vérifier que la conception se présente comme prévu dans tous les scénarios de test. Dans les cas où les ajustements doivent d’être effectuées pour un appareil spécifique ou l’orientation, utilisez le **modifier les caractéristiques** pour ajouter des contraintes pour les classes de taille spécifique :

    ![L’écran de lancement restitué sous la forme d’un iPhone X à l’aide de l’orientation paysage](launch-screens-images/launch12.png)

13. Enregistrez les modifications dans la table de montage séquentiel. Exécutez l’application sur un simulateur ou un appareil, et l’écran de lancement sera visible que le lancement de l’application.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Créer un nouveau projet. Dans Visual Studio, sélectionnez **fichier > Nouveau > projet > Visual c# > iPhone & iPad > application iOS (Xamarin)**:

    ![La fenêtre Nouveau projet, avec une application iOS (Xamarin) sélectionnée](launch-screens-images/launch01.w157.png)

    Sélectionnez le **application avec affichage unique** modèle, puis cliquez sur **OK**:

    ![Modèle d’application avec affichage unique](launch-screens-images/launch01-2.w157.png)

2. Si **ressources > LaunchScreen.xib** existe dans le **l’Explorateur de solutions**, supprimez-le en cliquant sur le fichier et en choisissant **supprimer**. Ce fichier est remplacé par une table de montage séquentiel à l’étape suivante.

3. Créer une table de montage séquentiel à utiliser en tant que l’écran de lancement. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le projet et choisissez **Ajouter > nouvel élément...**  suivie **Storyboard vide**. Nommez cette table de montage séquentiel **LaunchScreen.storyboard** et cliquez sur **ajouter**:

    ![La fenêtre Ajouter un nouvel élément, avec un Storyboard vide sélectionné](launch-screens-images/launch03.w157.png)

4. Configurer le projet à utiliser **LaunchScreen.storyboard** en tant que son Storyboard écran de lancement :

    - Dans l’**Explorateur de solutions**, double-cliquez sur le fichier **Info.plist** pour l’ouvrir et le modifier. 
    - Sur le **ressources visuelles** onglet, définissez **écran de lancement** à **LaunchScreen**.

    ![Le sélecteur d’écran de lancement dans Info.plist](launch-screens-images/launch04-vs.png)

5. Ajouter une image à un catalogue de ressources dans le projet afin qu’il soit disponible pour une utilisation sur l’écran de lancement :

    - Dans le **l’Explorateur de solutions**, avec le bouton droit sur **catalogues de composants** et sélectionnez **ajouter un catalogue de composants**. Nommez ce nouveau catalogue d’actifs **actifs**:

    ![La fenêtre Ajouter un nouvel élément, avec le catalogue de ressources sélectionné](launch-screens-images/launch05.w157.png)

    - Ajouter un nouvel ensemble d’images le **actifs** catalogue d’actifs, comme décrit dans la [Ajout d’Images à une ressource catalogue Image défini](~/ios/app-fundamentals/images-icons/displaying-an-image.md) section de la [affichage d’une Image](~/ios/app-fundamentals/images-icons/displaying-an-image.md) guide.

6. Ouvrez **LaunchScreen.storyboard** pour le modifier en double-cliquant dessus dans le **l’Explorateur de solutions**.

    - Pour modifier un fichier de Storyboard, Visual Studio nécessite une connexion active à un hôte de build Mac. Consultez le [connexion au Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) guide pour plus d’informations.

7. Choisissez un appareil et l’orientation sur lequel afficher un aperçu de l’animation d’écran de lancement dans le concepteur iOS. Ouvrez le panneau de sélection de périphérique dans la barre d’outils en bas et sélectionnez **4 s iPhone** et **Portrait**: 
 
    ![La barre d’outils de sélection de périphérique](launch-screens-images/launch07-vs.png)

    - Notez que la sélection d’un périphérique et l’orientation modifie uniquement la façon dont le concepteur iOS affiche un aperçu de la conception. Quelle que soit la sélection effectuée ici, qui vient d’être ajouté les contraintes sont appliquées dans l’ensemble des appareils et des orientations, sauf si le **modifier les caractéristiques** bouton a été utilisé pour spécifier dans le cas contraire. 

8. Ajouter un **contrôleur d’affichage** à la table de montage séquentiel en faisant glisser un à partir de la **boîte à outils** sur l’aire de conception : 

    ![Ajouté de contrôleur d’affichage vide à l’aire de conception](launch-screens-images/launch08-vs.png)

9. Définir le **arrière-plan** couleur de la vue principale du contrôleur d’affichage. Sélectionnez la vue en cliquant sur au milieu du contrôleur d’affichage et ajuster la couleur d’arrière-plan en utilisant la **fenêtre Propriétés**:
    
    ![Une vue unique avec une couleur d’arrière-plan violet](launch-screens-images/launch09-vs.png)

10. Ajouter un **affichage de l’Image** à l’écran de lancement et définir sa source **Image**:

    - Faites glisser un **affichage de l’Image** à partir de la **boîte à outils** au centre de la vue.
    - Avec le **affichage de l’Image** toujours sélectionné, dans le **Widget** section de la **fenêtre Propriétés** définir le **Image** propriété à l’ensemble de l’Image déjà ajouté à la **actifs** catalogue de composants. Repositionner et redimensionner le **affichage de l’Image** selon les besoins :
    
    ![Affichage d’une Image avec son jeu de propriétés d’Image](launch-screens-images/launch10-vs.png)

11. Ajouter un **étiquette** ci-dessous le **Image vue**:

    - Faites glisser un **étiquette** à partir de la **boîte à outils** sur la surface de conception, en le plaçant ci-dessous le **affichage de l’Image**.
    - Définir les attributs de la **étiquette** à l’aide de la **fenêtre Propriétés**:

    ![Une étiquette dont la valeur texte et la couleur](launch-screens-images/launch11-vs.png) 

12. Basculer vers le Mode d’édition de contrainte en utilisant le bouton droit dans le **barre d’outils de contraintes**:
    
    ![Le bouton du Mode d’édition de contrainte](launch-screens-images/launch12-vs.png) 

13. Ajouter des contraintes à la **affichage de l’Image**, en définissant sa hauteur et sa largeur et centrant horizontalement et verticalement :

    ![Affichage d’une Image avec des contraintes de mise en page](launch-screens-images/launch13-vs.png) 

    - Pour plus d’informations sur l’ajout de contraintes, consultez [disposition automatique avec le Concepteur de Xamarin pour iOS](~/ios/user-interface/designer/designer-auto-layout.md).

14. Ajouter des contraintes à la **étiquette**centrant horizontalement, en lui attribuant une hauteur et une largeur et positionnement fixe distance verticalement à partir de la **affichage de l’Image**:
    
    ![Une étiquette avec les contraintes de disposition](launch-screens-images/launch14-vs.png) 

15. Autres périphériques et les orientations pour vérifier que la conception se présente comme prévu dans tous les scénarios de test. Dans les cas où les ajustements doivent d’être effectuées pour un appareil spécifique ou l’orientation, utilisez le **modifier les caractéristiques** pour ajouter des contraintes pour les classes de taille spécifique :

    ![L’écran de lancement restitué sous la forme d’un iPhone X à l’aide de l’orientation paysage](launch-screens-images/launch15-vs.png) 

16. Enregistrez les modifications dans la table de montage séquentiel. Exécutez l’application sur un simulateur ou un appareil, et l’écran de lancement sera visible que le lancement de l’application.

-----

> [!NOTE]
> Une table de montage séquentiel utilisé comme un écran de lancement _doit_ incluent les éléments d’interface utilisateur uniquement simples et intégrées et **ne peut pas** effectuer des calculs ou dériver une classe personnalisée.

Pour plus d’informations sur la création d’un écran de lancement avec un Storyboard unifiée, consultez le [écrans de lancement dynamique](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) section de la [Storyboards unifiés](~/ios/user-interface/storyboards/unified-storyboards.md) guide.

## <a name="migrating-to-launch-screen-storyboards"></a>Migration de Storyboards de l’écran de lancement

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Lors de la mise à jour une application existante pour utiliser des tables de montage séquentiel pour ses écrans de lancement, avec le bouton droit cliquez sur le **nom_projet** dans le **l’Explorateur de solutions** et sélectionnez **ajouter**  >  **Nouveau fichier...** . Sélectionnez **iOS** > **écran de lancement** et cliquez sur le **New** bouton :

![](launch-screens-images/storyboard02.png "Sélectionner un écran de lancement d’iOS")

Ensuite, double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour modification. Sous **écran de lancement**, sélectionnez le nouveau fichier de plan conceptuel créé ci-dessus.

![](launch-screens-images/storyboard09.png "Sélectionnez le nouveau fichier de plan conceptuel créé ci-dessus")


Pour utiliser le nouveau plan conceptuel en tant qu’un écran de démarrage, procédez comme suit :

1. Double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour modification.
2. Faites défiler vers le **des Images de lancement universelle** section de l’éditeur, ouvrez le **écran de lancement** liste déroulante et sélectionnez le nom du plan conceptuel créé ci-dessus : 

    ![](launch-screens-images/storyboard08.png "Définition de l’écran de démarrage sur le plan conceptuel")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Avec le bouton droit sur le nom du projet dans le **l’Explorateur de solutions** et sélectionnez **ajouter** > **nouveau fichier...** : 

    ![](launch-screens-images/image012.png "Ajouter un nouveau fichier")
2. Entrez un nom de l’écran de lancement, cliquez sur le **ajouter** bouton : 

    ![](launch-screens-images/image013.png "Entrez un nom pour l’écran de démarrage")
3. Dans le **l’Explorateur de solutions**, double-cliquez sur le fichier de plan conceptuel qui vient d’être créé pour l’ouvrir pour modification.
4. Vérifiez que le **catégorie de taille** a la valeur **tout : n’importe quel** et le **afficher en tant que** est **générique**: 

    ![](launch-screens-images/image016.png "Assurez-vous que la classe de la taille est définie sur any : n’importe quel et l’afficher en tant qu’est générique")
5. Assembly de l’écran de démarrage à partir de Classes de taille, les éléments d’interface utilisateur simples (tel que `UIImageView`) et les images que vous avez inclus dans le bundle de l’application : 

    ![](launch-screens-images/image017.png "Assembly de l’écran de démarrage dans le concepteur iOS")
6. Enregistrez les modifications dans la table de montage séquentiel.

-----

## <a name="related-links"></a>Liens associés

- [Écrans de lancement dynamique (exemple)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Storyboards unifiés](~/ios/user-interface/storyboards/unified-storyboards.md)
- [Principes de base du concepteur iOS](~/ios/user-interface/designer/index.md)
- [Ajout d’Images à une Image de catalogue Asset définie](~/ios/app-fundamentals/images-icons/displaying-an-image.md#adding-images-to-an-asset-catalog-image-set)
- [Disposition automatique avec le Concepteur de Xamarin pour iOS](~/ios/user-interface/designer/designer-auto-layout.md)
- [Indications de l’Interface humaine : Écran de lancement](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
