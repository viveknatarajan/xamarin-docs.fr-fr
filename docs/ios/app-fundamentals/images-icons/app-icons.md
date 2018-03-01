---
title: "Icônes d’application"
description: "Cet article explique comment y compris et la gestion d’un composant de l’image dans une application Xamarin.iOS à utiliser comme icône de l’application."
ms.topic: article
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: 6de57e9523ff336c2e06e39903280db9c9ab95fa
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="application-icons"></a>Icônes d’application

_Cet article explique comment y compris et la gestion d’un composant de l’image dans une application Xamarin.iOS à utiliser comme icône de l’application._

Les rubriques suivantes sont traitées en détail :

* [Application, actualités et des icônes de paramètres](#icon-types) -les différents types d’icônes requis pour une application iOS.
* [Gestion des icônes avec Asset catalogues](#managing) - gestion des icônes d’application à l’aide de catalogues d’élément multimédia.
* [iTunes illustration](#itunes) -fournissant l’iTunes requis une illustration de la méthode Ad Hoc de la livraison de votre application.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>Application, les actualités et les icônes de paramètres

De la même façon qu’une application Xamarin.iOS pouvez utiliser des ressources d’image pour les contrôles d’interface utilisateur et sous forme d’icônes de document, les composants de l’image peuvent servir pour fournir des icônes de l’Application. Les captures d’écran suivantes à partir d’un iPad illustre les trois utilisations des icônes dans iOS :

- **Icône de l’application** -toutes les applications iOS doivent définir une icône d’application. Voici l’icône appuieront par l’utilisateur à partir de l’écran d’accueil pour lancer l’application iOS. En outre, cette icône est utilisée par le centre de jeu, le cas échéant. Exemple : 

    [ ![](app-icons-images/000.png "Icône de l’application")](app-icons-images/000-full.png)
- **Mettre en évidence icône** - chaque fois que l’utilisateur entre le nom d’une application dans une recherche Spotlight, cette icône s’affiche. Exemple : 

    [ ![](app-icons-images/000a.png "Icône de la galerie")](app-icons-images/000a-full.png)
- **Icône des paramètres** - si l’utilisateur entre le **paramètres** application sur son appareil iOS, cette icône s’affichera à la fin de la **paramètres** liste pour l’application. Exemple : 

    [ ![](app-icons-images/000b.png "Icône des paramètres")](app-icons-images/000b-full.png)

Les tailles d’image de ressource et les résolutions suivant seront nécessaire pour prendre en charge tous les types d’icône requis par une application Xamarin.iOS cibler iOS 5 via iOS 9 (ou version ultérieure) :

<table cellpadding="7" cellspacing="0" width="100%">
    <tr valign="top">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="5" align="center" bgcolor="#F0F0F0"><b>iPhone</b></td>
    </tr>
    <tr valign="center">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 5 et 6</b></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 7 et 8</b></td>
        <td align="center" bgcolor="#F9F9F9"><b>iOS 9 et 10<b><br/><i>(iPhone 6 et 7 Plus)</i></td>
    </tr>
    <tr valign="top" bgcolor="#F0F0F0">
        <td width="200" align="center"><b>Type d’icône</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>3x</b></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Icône d’application</td>
        <td align="center">57 x 57</td>
        <td align="center">114x114</td>
        <td align="center" style="color:#BBBBBB;">60x60<sup>(1)</sup></td>
        <td align="center">120x120</td>
        <td align="center">180x180</td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Spotlight</td>
        <td align="center">29 x 29</td>
        <td align="center">58x58</td>
        <td align="center" style="color:#BBBBBB;">40x40<sup>(2)</sup></td>
        <td align="center">80x80</td>
        <td align="center">120x120</td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Paramètres</td>
        <td align="center" style="color:#BBBBBB;">29x29<sup>(3)(4)</sup></td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(3)(4)</sup></td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center">87x87</td>
    </tr>
</table>

<table cellpadding="7" cellspacing="0" width="100%">
    <tr valign="top">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="5" align="center" bgcolor="#F0F0F0"><b>iPad</b></td>
    </tr>
    <tr valign="center">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 5 et 6</b></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 7 et 8</b></td>
        <td colspan="1" align="center" bgcolor="#F9F9F9"><b>iOS&nbsp;9 & 10</b></td>
    </tr>
    <tr valign="top" bgcolor="#F0F0F0">
        <td width="200" align="center"><b>Type d’icône</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>2x<br/>iPad&nbsp;Pro</b></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Icône d’application</td>
        <td align="center">72 x 72</td>
        <td align="center">144x144</td>
        <td align="center">76 x 76</td>
        <td align="center">152x152</td>
        <td align="center">167x167<sup>(6)</sup></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Spotlight</td>
        <td align="center">50 x 50</td>
        <td align="center">100x100</td>
        <td align="center">40 x 40</td>
        <td align="center">80x80</td>
        <td align="center" style="color:#BBBBBB;">120x120<sup>(5)</sup></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Paramètres</td>
        <td align="center" style="color:#BBBBBB;">29x29<sup>(3)(5)</sup></td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(3)(5)</sup></td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(5)</sup></td>
    </tr>
</table>

1. _Les deux Visual Studio pour Mac et Xcode plus en charge la définition de 1 image x pour iOS 7._
2. _Définition d’une image de 1 x pour iOS 7 n’est pas pris en charge lors de l’utilisation des catalogues d’élément multimédia._
3. _iOS 7 et 8 utiliser les mêmes tailles d’image comme iOS 5 et 6._
4. _Utilise les images et les tailles de même que l’icône de la galerie._
5. _Utilise les mêmes icônes de taille comme l’iPhone._
6. _Prise en charge uniquement avec l’Image de catalogues Asset._

Pour plus d’informations sur les icônes, consultez le site d’Apple [icône et des tailles d’Image](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) documentation.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>Gestion des icônes avec des catalogues d’actif

Pour les icônes, spéciale `AppIcons` ensemble d’images peut être ajouté à la `Assets.xcassets` fichier dans le projet de l’application. La version de l’image doit prendre en charge toutes les résolutions sont inclus dans le _xcasset_ et regroupées. Un éditeur spécial dans Visual Studio pour Mac permet au développeur d’inclure et d’installation de ces images de graphique.

Pour utiliser un catalogue, procédez comme suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
2. Faites défiler jusqu'à la **icônes de l’application** section.
3. À partir de la **Source** déroulante liste, vérifiez que **AppIcons** est sélectionnée : 

    ![](app-icons-images/migrate01.png "Vérifiez que AppIcons est sélectionné.")
4. À partir de la **l’Explorateur de solutions**, double-cliquez sur le `Assets.xcassets` fichier à ouvrir pour le modifier : 

    ![](app-icons-images/asset01.png "Le fichier Assets.xcassets dans l’Explorateur de solutions")
5. Sélectionnez `AppIcons` à partir de la liste des composants pour afficher le `Icon Editor`: 

    ![](app-icons-images/asset02.png "L’éditeur AppIcons")
6. Soit cliquez sur le type d’icône donné et sélectionnez un fichier image pour le type/la taille requise ou faites glisser dans l’image à partir d’un dossier et déposez-la sur la taille souhaitée.
7. Cliquez sur le **ouvrir** bouton à inclure l’image dans le projet et la définir dans le xcasset.
8. Répétez cette opération pour toutes les images requises.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Double-cliquez sur le **Info.plist** de fichiers dans le **l’Explorateur de solutions**:

    ![](app-icons-images/icon01w.png "Select Info.plist")
2. Cliquez sur le **visuelles** onglet, puis cliquez sur le **utilisez catalogue** sous **icônes de l’application**: 

    ![](app-icons-images/icon02w.png "Sélectionnez l’onglet composants visuels")
4. À partir de la **l’Explorateur de solutions**, développez le **catalogue** dossier : 

    ![](app-icons-images/image009.png "Développez le dossier de catalogue")
5. Double-cliquez sur le **Media** fichier pour l’ouvrir dans l’éditeur : 

    ![](app-icons-images/image010.png "Ouvrez le fichier de média dans l’éditeur")
6. Sous le **l’Explorateur de propriétés** le développeur peut sélectionner les différents types et les tailles d’icônes requis.
7. Cliquez sur le type d’icône donné et sélectionnez un fichier image pour le type/la taille requise.
8. Cliquez sur le **ouvrir** bouton à inclure l’image dans le projet et la définir dans le xcasset.
9. Répétez cette opération pour toutes les images requises.

-----

Il s’agit de la méthode préférée de, y compris et la gestion des ressources d’image qui servira à fournir des icônes de l’Application, des actualités et des paramètres pour une application.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Migration de Info.plist vers les catalogues Asset

Pour une application Xamarin.iOS existante en utilisant le `Info.plist` pour gérer ses icônes de fichiers, il est fortement recommandé que le développeur le basculer à utiliser le `AppIcons` composant de l’Image à l’intérieur de la `Assets.xcassets`.

Effectuez ce qui suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
2. Faites défiler jusqu'à la **icônes de l’application** section.
3. À partir de la **Source** liste déroulante, sélectionnez **migrer vers les catalogues Asset**: 

    ![](app-icons-images/migrate02.png "Sélectionnez migrer vers les catalogues Asset")
4. Les icônes existantes définies dans le `Info.plist` fichier vont être migré vers un `AppIcons` Image définie ajouté à `Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "L’Image AppIcons définie dans le Assets.xcassets")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
2. Cliquez sur l’iPhone section icônes : 

    ![](app-icons-images/image007.png "Éditeur d’icônes Rhe iPhone")
3. Faites défiler jusqu'à la **icônes** section.
4. À partir de la **catalogue** liste déroulante, sélectionnez **utilisez Asset catalogues**.
5. Les icônes existantes définies dans le `Info.plist` fichier vont être migré vers un `Images` ensemble ajouté à `Assets.xcassets`.
6. Enregistrez les changements dans le fichier `Info.plist`.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>Conception graphique iTunes

Si vous utilisez la méthode Ad Hoc de la livraison de l’application (soit, pour les utilisateurs d’entreprise ou bêta sur les appareils réels), le développeur doit également inclure un 512 x 512 et une image de 1024 x 1024 qui permet de représenter l’application dans iTunes.

Pour spécifier les illustrations iTunes, effectuez les tâches suivantes :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
2. Faites défiler vers le **iTunes illustration** section de l’éditeur : 

    ![](app-icons-images/itunes01.png "Faites défiler jusqu'à la section illustration de l’éditeur iTunes")
3. Pour toute image manquant, cliquez sur la miniature dans l’éditeur, sélectionnez le fichier image de l’illustration iTunes souhaitée à partir de la boîte de dialogue Ouvrir un fichier et cliquez sur le **OK** bouton.
4. Répétez cette étape jusqu'à ce que toutes nécessitent images ont été spécifiées pour l’application.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.

2. Cliquez sur le **visuelles** onglet et développez le **iTunes illustration**: 

    ![](app-icons-images/itunes01w.png "Modification iTunes illustration dans Visual Studio")
4. Pour toute image manquant, cliquez sur la miniature dans l’éditeur, sélectionnez le fichier image de l’illustration iTunes souhaitée à partir de la boîte de dialogue Ouvrir un fichier et cliquez sur le **ouvrir** bouton.
5. Répétez cette étape jusqu'à ce que toutes nécessitent images ont été spécifiées pour l’application.

-----

## <a name="related-links"></a>Liens associés

- [Utilisation d’Images (exemple)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Icône personnalisée et les instructions de création d’Image](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
