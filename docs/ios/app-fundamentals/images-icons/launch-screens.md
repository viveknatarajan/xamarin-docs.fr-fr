---
title: Lancer des écrans
description: Cet article explique comment créer une écran de lancement d’application pour tous les appareils iOS, à la résolution et l’orientation, à l’aide d’un Storyboard unifié unique.
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/19/2018
ms.openlocfilehash: 991c2f30bcca1969e336f7269ad2a22ce6245b95
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="launch-screens"></a>Lancer des écrans

_Cet article explique comment créer une écran de lancement d’application pour tous les appareils iOS, à la résolution et l’orientation, à l’aide d’un Storyboard unifié unique._

Avant iOS 8, la création d’un écran de lancement pour une application iOS requis au développeur de fournir un composant de l’image pour chacun des différents facteurs de forme de périphérique et les résolutions dans lequel l’application peut s’exécuter. Toutefois, depuis la version d’iOS 8, il a été possible d’utiliser un seul Storyboard unifié pour créer un écran de lancement est correct dans tous les cas.

Cette procédure pas à pas brève décrit comment créer un écran de lancement, soit une table de montage séquentiel fournie par défaut dans un nouveau projet ou avec un plan conceptuel ajoutés manuellement à un projet existant. Il montre ensuite comment utiliser le concepteur iOS pour ajouter une vue de l’Image et d’une étiquette pour la table de montage séquentiel pour définir des contraintes sur ces vues et pour vérifier que le plan conceptuel est correct pour différents appareils et orientations.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>Gestion des écrans de démarrage avec les plans conceptuels

Dans iOS 8 (et versions ultérieures), le développeur peut créer un Storyboard unifiée spécial pour fournir l’écran de lancement, au lieu d’utiliser une ou plusieurs images de démarrage statique. Lorsque vous créez le lancement d’un plan conceptuel dans le concepteur iOS, utilisez les Classes de taille et de mise en page automatique pour définir des dispositions différentes pour les environnements d’affichage différente. À l’aide des Classes de taille et de mise en page automatique, le développeur peut créer un écran de démarrage unique qui semble correct sur tous les appareils et afficher les environnements.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Dans Visual Studio pour Mac, créez un projet en sélectionnant **fichier > Nouvelle Solution** , puis en choisissant **unique afficher l’application**: 

    ![La fenêtre Nouveau projet, avec un seul afficher l’application sélectionnée](launch-screens-images/launch01.png)

    - Par défaut, un nouveau projet comprend un **LaunchScreen.storyboard** fichier qui définit l’interface de l’écran de lancement. 
    - Pour ajouter à la place un Storyboard écran lancer à un projet existant, cliquez sur le nom du projet dans le **Solution remplissage** et choisissez **Ajouter > nouveau fichier...**  , puis sélectionnez **lancer l’écran**:

    ![La fenêtre Nouveau fichier, avec iOS écran lancer sélectionné](launch-screens-images/launch01b.png)

    - Nommez le fichier **LaunchScreen** ou un autre nom de votre choix.

2. Configurer le projet pour utiliser le plan conceptuel approprié pour son écran de lancement :

    - Double-cliquez sur le **Info.plist** de fichiers dans le **Solution remplissage** à ouvrir pour le modifier.
    - Dans le **lancer les Images** section, assurez-vous que **lancer l’écran** est définie sur le nom de la table de montage séquentiel approprié :

    ![Le sélecteur de lancer l’écran dans le fichier Info.plist.](launch-screens-images/launch02.png)

    - Par défaut, un nouveau projet est configuré pour utiliser **LaunchScreen.storyboard** en tant que son écran de lancement.

3. Ajouter une image à la **Assets.xcassets** ressource catalogue afin qu’il soit disponible pour une utilisation sur l’écran de lancement. Pour plus d’informations, consultez la [Ajout d’Images à une ressource catalogue Image défini](~/ios/app-fundamentals/images-icons/displaying-an-image.md) section de la [affichage d’une Image](~/ios/app-fundamentals/images-icons/displaying-an-image.md) guide.

4. Ouvrez **LaunchScreen.storyboard** pour le modifier en double-cliquant dessus dans le **remplissage de la Solution**.

5. Choisissez un périphérique et l’orientation sur lequel afficher un aperçu de l’animation d’écran de lancement dans le concepteur iOS. Ouvrez le volet de sélection de périphérique dans la barre d’outils en bas et sélectionnez **4 s les iPhone** et **Portrait**.

    ![La barre d’outils de sélection de périphérique](launch-screens-images/launch05.png)

    - Notez que la sélection d’un périphérique et l’orientation modifie uniquement la façon dont le concepteur iOS affiche un aperçu de la conception. Quel que soit le choix effectué ici, qui vient d’être ajouté les contraintes sont appliquées sur tous les périphériques et les orientations, sauf si le **modifier les caractéristiques** bouton a été utilisé pour spécifier dans le cas contraire. 

6. Définir le **arrière-plan** couleur de la vue principale de vue du contrôleur. Sélectionnez la vue en cliquant sur au milieu de la vue de contrôleur et d’ajuster la couleur d’arrière-plan à l’aide du **propriétés remplissage**:

    ![Une vue unique avec une couleur d’arrière-plan violet](launch-screens-images/launch06.png)

7. Ajouter un **affichage Image** à l’écran de lancement et définissez sa source **Image**:

    - Faites glisser un **affichage Image** à partir de la **remplissage de la boîte à outils** au centre de la vue.
    - Avec la **vue de l’Image** sélectionné, dans le **Widget** section de la **propriétés remplissage** définir le **Image** propriété à l’Image déjà défini ajouté à la **Assets.xcassets** catalogue. Repositionnez et la taille de la **Image vue** selon les besoins :
    
    ![Affichage d’une Image avec son jeu de propriétés d’Image](launch-screens-images/launch07.png)

8. Ajouter un **étiquette** ci-dessous le **Image vue** et utiliser le **propriétés remplissage** pour définir ses attributs : 

    ![Une étiquette dont la couleur du texte et la valeur](launch-screens-images/launch08.png)

9. Basculez vers le Mode d’édition de contrainte en utilisant le bouton droit dans le **barre d’outils de contraintes**:
    
    ![Le bouton de Mode de modification de contrainte](launch-screens-images/launch09.png)

10. Ajouter des contraintes à la **affichage Image**, en définissant sa hauteur et sa largeur et il Centrer horizontalement et verticalement :

    ![Affichage d’une Image avec des contraintes de mise en page](launch-screens-images/launch10.png)

    - Pour plus d’informations sur l’ajout de contraintes, consultez [disposition automatique avec le Concepteur de Xamarin pour iOS](~/ios/user-interface/designer/designer-auto-layout.md).

11. Ajouter des contraintes à la **étiquette**, il Centrer horizontalement, en lui donnant une hauteur et la largeur et positionnement fixe verticalement à distance à partir de la **vue de l’Image**:

    ![Une étiquette avec des contraintes de mise en page](launch-screens-images/launch11.png)

12. Autres périphériques et les orientations pour vérifier que la conception est comme prévu dans tous les scénarios de test. Dans les cas où les ajustements doivent être effectuées pour un périphérique spécifique ou d’une orientation, utilisez le **modifier les caractéristiques** bouton pour ajouter des contraintes pour les classes de taille spécifique :

    ![L’écran de lancement restitué sous la forme d’un iPhone X à l’aide de l’orientation paysage](launch-screens-images/launch12.png)

13. Enregistrez les modifications dans la table de montage séquentiel. Exécuter l’application sur un périphérique ou un simulateur et l’écran de lancement seront visible comme le lancement de l’application.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Créer un nouveau projet. Dans Visual Studio, sélectionnez **fichier > Nouveau > projet**, puis choisissez **unique afficher l’application (iPhone)**:
    
    ![La fenêtre Nouveau projet, avec une seule application de vue (iPhone) sélectionnée](launch-screens-images/launch01-vs.png)

    - Nommez le projet, choisissez un emplacement, puis sélectionnez **OK**.

2. Si **ressources > LaunchScreen.xib** existe dans le **l’Explorateur de solutions**, supprimez-le en cliquant sur le fichier et en choisissant **supprimer**. Ce fichier est remplacé par un plan conceptuel à l’étape suivante.

3. Créer un plan conceptuel à utiliser comme écran de lancement. Dans le **l’Explorateur de solutions**, avec le bouton droit sur le projet et choisissez **Ajouter > nouvel élément...**  suivie **Storyboard vide**. Nom de ce plan conceptuel **LaunchScreen.storyboard** et cliquez sur **ajouter**:

    ![La fenêtre Ajouter un nouvel élément, avec Storyboard vide sélectionné](launch-screens-images/launch03-vs.png)

4. Configurer le projet à utiliser **LaunchScreen.storyboard** en tant que son Storyboard d’écran de lancement :

    - Dans l’**Explorateur de solutions**, double-cliquez sur le fichier **Info.plist** pour l’ouvrir et le modifier. 
    - Sur le **visuelles** onglet, définissez **lancer l’écran** à **LaunchScreen**.

    ![Le sélecteur de lancer l’écran dans le fichier Info.plist.](launch-screens-images/launch04-vs.png)

5. Ajouter une image à un catalogue dans le projet afin qu’il soit disponible pour une utilisation sur l’écran de lancement :

    - Dans le **l’Explorateur de solutions**, avec le bouton droit sur **Asset catalogues** et sélectionnez **ajouter le catalogue Asset**. Nom de ce nouveau catalogue **actifs**:

    ![La fenêtre Ajouter un nouvel élément, avec le catalogue sélectionné](launch-screens-images/launch05-vs.png)

    - Ajouter un nouvel ensemble d’images le **actifs** catalogue, comme décrit dans la [Ajout d’Images à une ressource catalogue Image défini](~/ios/app-fundamentals/images-icons/displaying-an-image.md) section de la [afficher une Image](~/ios/app-fundamentals/images-icons/displaying-an-image.md) guide.

6. Ouvrez **LaunchScreen.storyboard** pour le modifier en double-cliquant dessus dans le **l’Explorateur de solutions**.

    - Pour modifier un fichier d’animation, Visual Studio a besoin d’une connexion active à un hôte de build Mac. Consultez le [connexion au Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) guide pour plus d’informations.

7. Choisissez un périphérique et l’orientation sur lequel afficher un aperçu de l’animation d’écran de lancement dans le concepteur iOS. Ouvrez le volet de sélection de périphérique dans la barre d’outils en bas et sélectionnez **4 s les iPhone** et **Portrait**: 
 
    ![La barre d’outils de sélection de périphérique](launch-screens-images/launch07-vs.png)

    - Notez que la sélection d’un périphérique et l’orientation modifie uniquement la façon dont le concepteur iOS affiche un aperçu de la conception. Quel que soit le choix effectué ici, qui vient d’être ajouté les contraintes sont appliquées sur tous les périphériques et les orientations, sauf si le **modifier les caractéristiques** bouton a été utilisé pour spécifier dans le cas contraire. 

8. Ajouter un **View Controller** à l’animation en faisant glisser un à partir de la **boîte à outils** sur l’aire de conception : 

    ![Vide View Controller ajouté à l’aire de conception](launch-screens-images/launch08-vs.png)

9. Définir le **arrière-plan** couleur de la vue principale de vue du contrôleur. Sélectionnez la vue en cliquant sur au milieu de la vue de contrôleur et d’ajuster la couleur d’arrière-plan à l’aide du **fenêtre Propriétés**:
    
    ![Une vue unique avec une couleur d’arrière-plan violet](launch-screens-images/launch09-vs.png)

10. Ajouter un **affichage Image** à l’écran de lancement et définissez sa source **Image**:

    - Faites glisser un **affichage Image** à partir de la **boîte à outils** au centre de la vue.
    - Avec la **vue de l’Image** toujours sélectionné, dans le **Widget** section de la **fenêtre Propriétés** définir le **Image** propriété à l’ensemble de l’Image déjà ajouté à la **actifs** catalogue. Repositionnez et la taille de la **Image vue** selon les besoins :
    
    ![Affichage d’une Image avec son jeu de propriétés d’Image](launch-screens-images/launch10-vs.png)

11. Ajouter un **étiquette** sous le **affichage de l’Image**:

    - Faites glisser un **étiquette** à partir de la **boîte à outils** sur l’aire de conception, sa sélection élective ci-dessous le **vue de l’Image**.
    - Définir des attributs pour le **étiquette** à l’aide de la **fenêtre Propriétés**:

    ![Une étiquette dont la couleur du texte et la valeur](launch-screens-images/launch11-vs.png) 

12. Basculez vers le Mode d’édition de contrainte en utilisant le bouton droit dans le **barre d’outils de contraintes**:
    
    ![Le bouton de Mode de modification de contrainte](launch-screens-images/launch12-vs.png) 

13. Ajouter des contraintes à la **affichage Image**, en définissant sa hauteur et sa largeur et il Centrer horizontalement et verticalement :

    ![Affichage d’une Image avec des contraintes de mise en page](launch-screens-images/launch13-vs.png) 

    - Pour plus d’informations sur l’ajout de contraintes, consultez [disposition automatique avec le Concepteur de Xamarin pour iOS](~/ios/user-interface/designer/designer-auto-layout.md).

14. Ajouter des contraintes à la **étiquette**, il Centrer horizontalement, en lui donnant une hauteur et la largeur et positionnement fixe verticalement à distance à partir de la **vue de l’Image**:
    
    ![Une étiquette avec des contraintes de mise en page](launch-screens-images/launch14-vs.png) 

15. Autres périphériques et les orientations pour vérifier que la conception est comme prévu dans tous les scénarios de test. Dans les cas où les ajustements doivent être effectuées pour un périphérique spécifique ou d’une orientation, utilisez le **modifier les caractéristiques** bouton pour ajouter des contraintes pour les classes de taille spécifique :

    ![L’écran de lancement restitué sous la forme d’un iPhone X à l’aide de l’orientation paysage](launch-screens-images/launch15-vs.png) 

16. Enregistrez les modifications dans la table de montage séquentiel. Exécuter l’application sur un périphérique ou un simulateur et l’écran de lancement seront visible comme le lancement de l’application.

-----

> [!NOTE]
> Une table de montage séquentiel utilisé comme un écran de lancement _doit_ incluent les éléments d’interface utilisateur uniquement simples et intégrés et **ne peut pas** effectuer des calculs ou dériver une classe personnalisée.

Pour plus d’informations sur la création d’un écran de lancement avec un Storyboard unifiée, consultez le [dynamique écrans lancer](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) section de la [unifiée des animations](~/ios/user-interface/storyboards/unified-storyboards.md) guide.

## <a name="migrating-to-launch-screen-storyboards"></a>Migration de lancer des animations d’écran

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Lors de la mise à jour d’une application existante pour utiliser des animations pour les écrans de son lancement, bouton droit sur le **nom du projet** dans les **l’Explorateur de solutions** et sélectionnez **ajouter**  >  **Nouveau fichier...** . Sélectionnez **iOS** > **lancer l’écran** et cliquez sur le **nouveau** bouton :

![](launch-screens-images/storyboard02.png "Sélectionnez un écran de lancement iOS")

Ensuite, double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier. Sous **lancer l’écran**, sélectionnez le nouveau fichier de plan conceptuel créé ci-dessus.

![](launch-screens-images/storyboard09.png "Sélectionnez le nouveau fichier de plan conceptuel créé ci-dessus")


Pour utiliser le nouveau Storyboard comme un écran de démarrage, procédez comme suit :

1. Double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
2. Faites défiler vers le **Universal Images lancer** section de l’éditeur, ouvrez le **lancer l’écran** liste déroulante et sélectionnez le nom du plan conceptuel créé ci-dessus : 

    ![](launch-screens-images/storyboard08.png "Définition de l’écran d’installation sur le plan conceptuel")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Avec le bouton droit sur le nom du projet dans le **l’Explorateur de solutions** et sélectionnez **ajouter** > **nouveau fichier...** : 

    ![](launch-screens-images/image012.png "Ajouter le nouveau fichier")
2. Entrez un nom pour l’écran d’installation, cliquez sur le **ajouter** bouton : 

    ![](launch-screens-images/image013.png "Entrez un nom pour l’écran d’installation")
3. Dans le **l’Explorateur de solutions**, double-cliquez sur le fichier d’animation qui vient d’être créé pour l’ouvrir pour le modifier.
4. Vérifiez que le **taille classe** a la valeur **n’importe quel : n’importe quel** et le **en tant que vue** est **générique**: 

    ![](launch-screens-images/image016.png "Assurez-vous que la classe de la taille est définie sur any : tout et la vue est générique")
5. L’écran d’installation à partir des Classes de taille, les éléments d’interface utilisateur simples d’assembly (tel que `UIImageView`) et les images que vous avez inclus dans l’offre groupée de l’application : 

    ![](launch-screens-images/image017.png "Assembly de l’écran d’installation dans le concepteur iOS")
6. Enregistrez les modifications dans la table de montage séquentiel.

-----

## <a name="related-links"></a>Liens associés

- [Écrans de lancement dynamique (exemple)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Storyboards unifiés](~/ios/user-interface/storyboards/unified-storyboards.md)
- [Principes de base du concepteur iOS](~/ios/user-interface/designer/index.md)
- [Ajout d’Images à une Image du catalogue Asset défini](~/ios/app-fundamentals/images-icons/displaying-an-image.md#asset-catalogs)
- [Disposition automatique avec le Concepteur de Xamarin pour iOS](~/ios/user-interface/designer/designer-auto-layout.md)
- [Recommandations de l’Interface utilisateur : Écran initial](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
