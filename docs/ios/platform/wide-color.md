---
title: Couleur large
description: "Cet article traite des couleurs large et comment il peut être utilisé dans une application Xamarin.iOS ou Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 34f2e249c5474997b2e2e179285f455c425739cf
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="wide-color"></a>Couleur large

_Cet article traite des couleurs large et comment il peut être utilisé dans une application Xamarin.iOS ou Xamarin.Mac._

iOS 10 et macOS Sierra améliore la prise en charge pour les formats de pixel de plage étendue et les espaces d’une gamme de couleurs dans l’ensemble du système, y compris les infrastructures telles que les graphiques de base, l’Image de base, complète et AVFoundation. Prise en charge pour les appareils avec une couleur large affiche est facilité davantage en fournissant ce comportement dans toute la pile de graphique entière.

## <a name="about-wide-color"></a>À propos de la couleur large

Comme indiqué ci-dessus, iOS 10 et macOS Sierra améliore la prise en charge pour les formats de pixel de plage étendue et les espaces d’une gamme de couleurs dans l’ensemble du système, notamment les infrastructures telles que les graphiques de base, l’Image de base, complète et AVFoundation. Prise en charge pour les appareils avec une couleur large affiche est facilité davantage en fournissant ce comportement dans toute la pile de graphique entière.

Dans les années 90 Apple créé ColorSync pour gérer les couleurs de traitement sur le Mac. Ils ont également aidé trouvé l’International Color Consortium (ICC) pour créer et promouvoir un ensemble de normes pour la gestion des couleurs sur le matériel de l’ordinateur. Travail d’Apple avec l’ICC a été incluse dans ColorSync et elle a été intégrée à la base de OS X (maintenant appelé macOS).

Apple a également été à la pointe de la technologie d’affichage avec le matériel, telles que la rétine afficher, la nouvelle P3 et afficher P3 couleur espace (diffusées dans iMac 2015) et le TrueTone affiche des professionnels de l’iPad, iPhone 7 iPhone 7 Plus.

Avec une couleur large dans macOS Sierra et iOS 10, Apple change gestion de couleur pour tirer pleinement parti de ces nouvelles technologies affichage macOS et iOS.

## <a name="core-color-concepts"></a>Principaux Concepts de couleur

Les concepts de couleur de base suivants doivent être couverts avant d’effectuer une présentation approfondie du couleur dans macOS et iOS :

### <a name="color-space"></a>Espace de couleurs

Un espace de couleurs est un environnement dans lequel les couleurs peuvent être représentés et comparées. Il peut être un espace de dimensions d’un à quatre est défini par l’intensité de ses composants de couleur. 

[![](wide-color-images/color00.png "Un espace de couleurs")](wide-color-images/color00.png#lightbox)

### <a name="color-channels"></a>Les canaux de couleur

Les composants de couleur peuvent également être appelés canaux de couleur. Certaines représentations familiers serait les espaces RVB, gris espaces, espaces CMJN ou appareil indépendants espaces. 

[![](wide-color-images/color02.png "Les composants de couleur peuvent également être appelés canaux de couleur")](wide-color-images/color02.png#lightbox)

### <a name="color-primaries"></a>Couleurs primaires

Couleurs primaires fournissent le système de coordonnées qui est utilisé pour comparer et le calcul des couleurs. Couleurs primaires appartiennent généralement les plus intense version de la couleur donnée peut être générée dans le canal de couleur.

[![](wide-color-images/color01.png "Le système de coordonnées qui est utilisé pour comparer et couleurs de calcul fournissent des couleurs primaires")](wide-color-images/color01.png#lightbox)

Dans le cas de l’espace de couleurs RVB représenté ci-dessus, les couleurs primaires sont où le `1.0` coordonnées sont ancrées (tel que `[1.0, 0.0, 0.0]` pour le rouge).

### <a name="color-gamut"></a>Gamme de couleurs

Gamme de couleurs fait référence à toutes les couleurs qui peuvent être définies comme une combinaison des canaux de couleurs individuels au sein d’un espace de couleurs donné.

[![](wide-color-images/color03.png "Exemple de gamme de couleurs")](wide-color-images/color03.png#lightbox)

## <a name="what-is-wide-color"></a>Quelle est la couleur large

Avant de traiter la rubrique de couleur large, une discussion doit être avait sur la norme actuelle pour la couleur, de l’espace de couleurs RVB Standard (sRVB). Il est l’espace de couleur plus largement utilisé dans le calcul aujourd'hui et l’espace de couleurs par défaut pour iOS et macOS.

L’espace colorimétrique sRGB a les propriétés suivantes :

- Il est basé sur la norme ITU-R BT.709.
- Il a une valeur approximative Gamma de 2,2.
- Il représente les conditions d’éclairage classique (D65).

Étant donné que sRGB est largement qu'utilisé dans l’industrie, un développeur peut faire des hypothèses qui la couleur spécifiée sera fidèlement représentée sur n’importe quel appareil, sur qu'il est affiché. Toutefois, cela peut être pas toujours le cas. En outre, il existe des plusieurs couleurs qui ne tiennent pas dans l’espace colorimétrique sRGB et c’est pourquoi, ne peut pas être représentés dans celui-ci.

Par exemple, textile nombreux conçues avec les threads à l’aide des nombreux encre et colorants entrant en dehors de sRGB. Également de nombreux produits qu’une personne rencontre dans leur vie quotidienne sont créés avec des couleurs vives qui se trouvent en dehors de l’espace colorimétrique sRGB. Parmi les plus intéressantes de couleurs ne peut pas être représenté en sRVB relèvent par nature, couchers de soleil, feuilles d’automne, fleurs exotiques et eaux tropicaux.

Les utilisateurs qui ont été capture d’images numériques au format brut peuvent avoir des images sur leurs appareils qui contiennent toutes les données de cette couleur, même s’il ne peut pas être correctement représentée dans l’espace colorimétrique sRGB et, par conséquent, ne peut pas s’afficher correctement sur l’écran.

### <a name="the-display-p3-color-space"></a>L’espace de couleurs d’affichage P3

2015, Apple a publié des nouveaux produits (iMac et iPad Pro 9.7") qui fournissent le nouvel espace de couleur P3 complet pour gérer les problèmes créés par l’espace colorimétrique sRGB.

[![](wide-color-images/color04.png "Le nouvel espace de couleur P3 affichage")](wide-color-images/color04.png#lightbox)

L’espace de couleurs d’affichage P3 a les propriétés suivantes :

- Prend en charge un espace de couleurs large pour les plateformes matérielles modernes.
- Selon la norme SMPTE DCI-P3. DCI-P3 a été conçu pour les projecteurs numériques, mais a été modifié par Apple pour prendre en charge les analyses.
- Il a une valeur approximative Gamma de 2,2.
- Il représente les conditions d’éclairage classique (D65).

En fonction d’Apple, les utilisateurs déplacez leurs flux de travail sur leurs plateformes mobiles. Résoudre les problèmes de présentation de couleur présentés par sRGB dans les périphériques mobiles professionnels (iPad professionnels de l’informatique), requis plus simplement, y compris un affichage couleur large. La solution a été mise à niveau de l’étalonnage de fabrique, pour chaque périphérique a été étalonné à la fabrique de garantir que d’un périphérique à affichage des couleurs est précise et cohérente.

Une autre solution, est l’inclusion complète, à l’échelle du système de gestion des couleurs développé dans iOS 10 et macOS Sierra Apple. 

### <a name="the-extended-range-srgb-color-space"></a>L’espace colorimétrique de plage étendue sRGB

La gestion des couleurs de l’échelle du système iOS 10 nouvelle doit tenir compte de toutes les applications iOS existants qui sont créées et ajustées pour sRVB. Il a été conçu pour vous assurer qu’il n’a pas les performances soit couleur d’une application ou représentation sous forme de ces applications existantes.

Pour gérer cette situation, Apple a inclus l’espace colorimétrique sRGB de plage étendue dans iOS 10 (et macOS Sierra).

L’espace colorimétrique de plage étendue sRGB a les propriétés suivantes :

- Comprend tous le même sRGB primaires.
- Il a une valeur approximative Gamma de 2,2.
- Il représente les conditions d’éclairage classique (D65).
- Il prend en charge les valeurs négatives et les valeurs supérieur à un (1).

En permettant à des valeurs inférieures à zéro et supérieur à un, le sRGB plage étendue Qu'espace colorimétrique permet non seulement pour les applications existantes à présentes couleurs sRVB sans dégradation des performances ou de distorsion, mais il permet à l’espace colorimétrique représenter n’importe quelle couleur à l’intérieur de l’élément visible spectre. Tout ceci s’effectue tout en conservant les mêmes points d’ancrage en tant que l’espace colorimétrique sRGB.

### <a name="extended-range-srgb-in-action"></a>SRVB de plage étendue en Action

Pour voir comment les valeurs en dehors de zéro et un fonctionnent dans l’espace colorimétrique de plage étendue sRGB, prenons l’exemple suivant de la de la plus saturée rouge disponible dans l’espace de couleurs d’affichage P3 :

[![](wide-color-images/color05.png "Fonctionnement des valeurs en dehors de zéro et un dans l’espace colorimétrique de plage étendue sRGB")](wide-color-images/color05.png#lightbox)

Dans l’affichage, P3, cette couleur est représentée en tant que `[1.0, 0.0, 0.0]` et sRVB de plage étendue il serait `[1.358, -0.074, -0.012]`. Les valeurs sRGB étant complètes contenue à l’intérieur d’affichage P3 et les valeurs d’affichage P3 disposer « en dehors de » des plages sRVB.

Pour le matériel physique qui autorise les valeurs de pixel atteindre d’une valeur positive extrêmes des valeurs négatives extrêmes, il peut afficher n’importe quelle couleur disponible dans le spectre visible, et ces valeurs peuvent être représentées dans l’espace colorimétrique de plage étendue sRGB.

### <a name="device-pixel-formats"></a>Formats de Pixel de périphérique 

L’espace colorimétrique a été en grande partie de sRGB normaliser sur l’utilisation d’un Format de Pixel de 8 bits étant principalement 8 bits par canal de couleur insuffisant décrire les couleurs de sRGB. Ce n’est pas parfaite mais suffisant, et elle constitue un bon compromis entre l’utilisation du processeur et de mémoire pour afficher des images.

Étant donné que l’affichage P3 peut représenter les coordonnées de la couleur en dehors de l’espace de couleurs sRGB, elle nécessite 16 bits par canal de couleur afin de représenter correctement les couleurs avec l’espace colorimétrique de plage étendue sRGB.

## <a name="system-wide-wide-color-support"></a>Prise en charge de la couleur de large à l’échelle du système

Pour prendre en charge les couleurs de large et large gamme de couleurs à l’intérieur d’iOS 10 et macOS Sierra, Apple a étendu les infrastructures suivantes pour tirer pleinement parti de l’espace colorimétrique de plage étendue sRGB et P3 d’affichage :

- UIKit (pour iOS uniquement)
- SceneKit
- Graphique de base
- ImageIO
- Image de base
- WebKit
- SpriteKit
- Animation de base
- AppKit (pour macOS uniquement)

En outre, affichage Retina prise en charge a été améliorée pour l’espace colorimétrique sRGB de plage étendue et affichage P3 affiche.

Couleur de large est pris en charge et peut être utilisé dans les types de contenu d’application suivants :

- Ressources d’image statique inclus dans le lot d’applications.
- Document et réseau en fonction des ressources d’images.
- Support avancé tels que Live des Photos ou des images au format brut.
- Images graphiques nuanceur texture 3D.

## <a name="solving-the-color-problem"></a>Résolution du problème de couleur

Le contenu affiché dans une application peut provenir d’un large éventail de sources de riches en couleurs. En outre, ce contenu peut être affiché sur un large éventail de périphériques, chacun avec leur propre gamme de fonctions d’affichage de couleur.

Une application iOS 10 établit un pont entre la différence entre ces deux problèmes en utilisant le nouveau système intégré, couleur gérer système. Ce système garantit qu’une image a le même aspect sur un appareil iOS, quel que soit le quel espace de couleur de l’image a été encodée dans.

Gestion des couleurs démarre avec chaque image ayant un espace de couleurs associé (ou le profil de couleurs). Ces informations sont utilisées dans les _processus de mise en correspondance de couleur_ où les couleurs dans l’image source sont mis en correspondance avec les couleurs dans le périphérique de sortie. Étant donné que chaque pixel de l’image doit être mis en correspondance de la couleur, il peut prendre beaucoup de temps et placez une contrainte lors de processeur son.

En raison de la nature de la _processus de mise en correspondance de couleur_, cette conversion peut être potentiellement « avec perte » si le périphérique de sortie a une gamme plus petite que l’image source.

Heureusement, les calculs nécessaires à la _processus de mise en correspondance de couleur_ permettre facilement être accélération matérielle (soit sur l’UC ou au GPU) et Apple permet de s’assurer qu’il fonctionne automatiquement par la création de la prise en charge des systèmes de base de Quartz 2D, ColorSync et l’Animation de base. Pour le contenu correctement balisé, aucun codage n’est nécessaire pour tirer parti de ces fonctionnalités.

Gestion des couleurs a été pris en charge sur chaque plateforme comme suit :

- **macOS** -macOS a été géré depuis le début de couleur.
- **iOS** -iOS a pris en charge la gestion des couleurs automatique depuis iOS 9.3 (et versions ultérieures).

### <a name="designing-for-wide-gamut"></a>Conception pour une large gamme de couleurs

Apple a la suggestion suivante pour la conception et à l’aide de la largeur de couleur, contenu dans des applications iOS et macOS large gamme de l’image :

- Uniquement utiliser le contenu de la large gamme dans Vérifiez sense dans l’application, ils ne doivent pas automatiquement être utilisées partout.
- Utiliser uniquement du contenu de la large gamme où couleurs vives améliorer l’expérience utilisateur.
- Dans n’est pas nécessaire de modifier tout le contenu à P3 pour les applications existantes.

Chaîne d’outils d’Apple rend prenant en charge pour le contenu de l’image large gamme un progressive de participer, prise en charge de couleur large dans une application n’est pas une situation tout ou rien.

### <a name="upgrading-existing-content-to-wide-color"></a>La mise à niveau de contenu existant de couleur large

Apple a les suggestions suivantes pour mettre à niveau le contenu existant de l’image couleur large :

- Ne pas simplement » affecter » un profil P3 pour le contenu de l’application d’édition d’image. Cela remappe simplement la couleur de contenu vers le nouvel espace de couleurs avec des résultats inattendus, tels qu’étirement les couleurs pour tenir dans le nouvel espace modifiant ainsi l’image existante.
- Vous devez le contenu de l’image à « convertir » pour le profil complet P3 à l’aide d’une application d’édition d’image.

### <a name="file-formats-and-color-profiles"></a>Formats de fichier et les profils de couleurs

Apple a les suggestions suivantes pour les formats de fichier et les profils de couleurs utilisées dans le contenu de l’image de l’application couleur large :

- Utiliser le profil de couleurs « Affichage P3 » pour les espaces de travail RVB.
- Utilisez 16 bits par mode de canal de couleur.
- Utiliser un iMac en retard 2015 (ou version ultérieure) pour prévisualiser avec précision le contenu de l’image.
- Exporter des ressources d’image en tant que fichiers PNG de 16 bits avec un profil « Affichage P3 » ICC incorporé.

> [!IMPORTANT]
> À l’aide de la **enregistrer pour le Web** ou **exporter les ressources** fonctionnalités se trouvent dans le logiciel d’édition d’image les plus populaires _ne seront pas_ fonctionnent pour les images du large couleur étant donné que ces fonctionnalités n’ont pas été mise à jour prend encore en charge les spécifications de format de fichier requis.

### <a name="supporting-wide-color-with-asset-catalogs"></a>Prise en charge de couleur large avec des catalogues d’actif

Dans iOS 10 et macOS Sierra, Apple a étendu les catalogues Asset utilisé pour inclure et classer le contenu d’image statique dans une offre groupée de l’application pour prendre en charge les couleurs étendue.

À l’aide de catalogues d’Asset offrent les avantages suivants à une application :

- Ils fournissent la meilleure option de déploiement de ressources d’image statique.
- Ils prennent en charge de correction de couleur automatique.
- Ils prennent en charge l’optimisation de format de pixel automatique.
- Ils prennent en charge de segmentation de l’application et de réduction de l’application qui garantit qu’uniquement le contenu qui est de get pertinentes livré au périphérique de l’utilisateur final.

Apple a fait les améliorations suivantes aux catalogues de ressource pour la prise en charge de couleur large :

- Ils prennent en charge le contenu de la source de 16 bits (par canal de couleur).
- Ils prennent en charge le contenu catalogage par gamme d’affichage. Le contenu peut être marqué pour le sRVB ou un affichage P3 gammes.

Le développeur a trois options pour la prise en charge des couleurs large dans leurs applications :

1. **Ne rien faire** -étant donné que le contenu de couleurs étendue doit être utilisé uniquement dans les situations où l’application a besoin pour afficher les couleurs vives (où ils améliorer l’expérience utilisateur), contenu en dehors de cette exigence doit être laissé en tant que-est. Il continue à s’afficher correctement dans toutes les situations de matériel.
2. **Mise à niveau du contenu existant pour affichage P3** -le développeur à remplacer le contenu de l’image dans leur catalogue avec un nouveau fichier au format P3 mis à niveau et la marquer en tant que tel dans l’éditeur de ressources. Au moment de la génération, une image dérivés sRGB sera générée à partir de ces ressources.
3. **Fournir optimisé le contenu de la ressource** -dans ce cas, le développeur fournit une sRGB 8 bits et une vision d’affichage P3 16 bits de chaque image dans le catalogue (avec balises correctement dans l’éditeur de ressources).

### <a name="asset-catalog-deployment"></a>Déploiement du catalogue Asset

Le processus suivant se produit lorsque le développeur soumet une application à l’App Store avec des catalogues Asset qui incluent du contenu d’image couleur large :

- Lorsque l’application est déployée à l’utilisateur final, application découpage garantit que la variante de contenu appropriée est remise à l’appareil de l’utilisateur.
- Sur les appareils qui ne prennent pas en charge les couleurs large, n’a aucun coût de la charge utile pour inclure le contenu de la couleur de large, il n’est jamais fourni à l’appareil.
- `NSImage` sur macOS Sierra (et versions ultérieures) sélectionne automatiquement la meilleure représentation contenue pour l’affichage de matériel.
- Le contenu affiché est actualisé automatiquement quand ou si le matériel des périphériques affiche les caractéristiques de la modification.

### <a name="asset-catalog-storage"></a>Stockage des catalogues Asset

Stockage des catalogues Asset a les propriétés et les implications d’une application suivantes :

- Au moment de la génération, Apple tente d’optimiser le stockage de contenu de l’image via les conversions d’image de haute qualité.
- 16 bits sont utilisés par les canaux de couleur pour le contenu de la couleur de large.
- Contenu de la compression d’image dépendante est utilisée pour réduire les tailles de contenu du livrable. Nouvelle compression « perte » ont été ajoutées pour optimiser davantage les tailles de contenu.

## <a name="colors-in-user-interfaces"></a>Couleurs dans les Interfaces utilisateur

Lorsque vous travaillez avec les couleurs dans une Interface utilisateur, la plupart des pixels sur l’écran sont dans une couleur unie. En outre, la plupart de ces pixels ne proviennent pas actifs d’images mais est dessinée directement par l’application (ou par le système d’exploitation pour le compte de l’application).

Large gamme de couleurs peut présenter plusieurs difficultés lors de l’utilisation du niveau de l’interface utilisateur :

- **Communication des couleurs** : lors de la distinction entre les couleurs entre les concepteurs et développeurs est généralement un _supposé_ sRGB espace colorimétrique impliqué. Pour une couleur peut être communiquée en tant que `rgb(128, 45, 56)` ou `#FF0456`. Dans une conception de la large gamme de couleurs, ces représentations ne fournissent pas suffisamment d’informations pour représenter avec la couleur spécifiée, la couleur d’espace de travail doit également être inclus. Apple suggère à l’aide de `P3(128, 45, 56)` et `P3#FF0456` à la place. 
- **Sélection de couleurs** - plus de la modification de l’image courante et la conception logicielle concernées par les mêmes limitations que l’espace colorimétrique sRGB lors de l’utilisation de leurs sélecteurs de couleurs intégrée. Le concepteur doit vérifier qu’ils sont dans l’espace colorimétrique des « Affichage P3 » dans le sélecteur de couleurs lorsque vous travaillez avec des conceptions de couleur large.
- **Codage en couleurs** - `NSColor` (macOS) et `UIColor` (iOS & tvOS) ont de nouvelles méthodes pratiques pour la génération de couleurs de P3 directement et les deux ont été étendues pour prendre en charge les couleurs dans la plage étendue espace colorimétrique sRGB également.
- **Stockage des couleurs** -soyez vigilant lorsque le stockage large gamme de couleurs de couleurs dans le document d’une application. Lorsque 8 bits par canal de couleur fonctionné pour l’espace de couleurs sRGB, 16 bits par canal de couleur doit être utilisé pour les couleurs larges. Le développeur doit également surveiller les instances de l’espace de couleurs donné (depuis tout traditionnellement sRGB uniquement).

## <a name="colors-on-the-web"></a>Couleurs sur le Web

Soyez vigilant lorsque vous travaillez avec une large couleur dans les pages web et sur les appareils qui prennent en charge d’affichage des couleurs large. Si tout le contenu de l’image qui a été inclus dans le site Web a été correctement identifiée, iOS et macOS automatiquement correspondance de couleurs et les afficher correctement.

Requêtes de média sont également disponibles pour vous aider à résoudre des ressources entre des périphériques compatibles P3 et sRVB :

```xml
<picture>
    <source srcset="monkey-p3.jpg" media="(color-gamut: p3)">
    <source srcset="monkey-rpg.jpg">
</picture>
```

Apple a également une proposition WebKit qui permettra de CSS dans les autres espaces de couleurs en dehors de l’espace sRVB donné.

## <a name="rendering-off-screen-images-in-app"></a>Rendu des Images hors écran dans une application

Selon le type d’application en cours de création, il peut permettre aux utilisateurs d’inclure le contenu de l’image qu’ils ont collectées à partir d’internet ou créer le contenu de l’image directement à l’intérieur de l’application (par exemple, un vecteur d’application de dessin par exemple).

Dans ces deux cas, l’application peut rendre l’imagerie requis hors écran couleur large à l’aide des fonctionnalités améliorées ajoutées pour iOS et macOS.

### <a name="drawing-wide-color-in-ios"></a>Couleur de dessin large dans iOS

Avant d’expliquer comment dessiner correctement une image couleur large dans iOS 10, examinons le code de dessin iOS courantes suivantes :

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    UIGraphics.BeginImageContext (size);

    ...
    
    UIGraphics.EndImageContext ();
    return image = UIGraphics.GetImageFromCurrentImageContext ();
}
```

Il existe des problèmes avec le code standard qui devront être réglés _avant_ il peut être utilisé pour dessiner une image couleur large. Le `UIGraphics.BeginImageContext (size)` méthode utilisée pour démarrer le dessin d’image iOS présente les limitations suivantes :

- Il ne peut pas créer des contextes d’image avec plus de 8 bits par canal de couleur.
- Il ne peut pas représenter des couleurs dans l’espace colorimétrique de plage étendue sRGB.
- Il n’a pas la possibilité de fournir une interface pour créer des contextes dans un espace de couleurs sRGB non en raison des routines de C bas niveau qui est appelées en arrière-plan.

Pour gérer ces limitations et dessiner une image couleur large dans iOS 10, utilisez le code suivant à la place :

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    var render = new UIGraphicsImageRenderer (size);

    var image = render.CreateImage ((UIGraphicsImageRendererContext context) => {
        var bounds = context.Format.Bounds;
        CGRect slice;
        CGRect remainder;
        bounds.Divide (bounds.Width / 2, CGRectEdge.MinXEdge, out slice, out remainder);

        var redP3 = UIColor.FromDisplayP3 (1.0F, 0.0F, 0.0F, 1.0F);
        redP3.SetColor ();
        context.FillRect (slice);

        var redRGB = UIColor.Red;
        redRGB.SetColor ();
        context.FillRect (remainder);
    });

    // Return results
    return image;
}
```

La nouvelle `UIGraphicsImageRenderer` classe crée un nouveau contexte d’image qui est capable de gérer l’espace colorimétrique de plage étendue sRGB et il présente les caractéristiques suivantes :

- Il est entièrement géré par défaut de couleur.
- Il prend en charge l’espace de couleurs de sRGB plage étendue par défaut.
- Il décide intelligemment si elle doit être rendu le sRVB ou plage étendue sRGB espace colorimétrique basée sur les fonctionnalités de l’appareil iOS qui l’application est en cours d’exécution.
- Il gère automatiquement et entièrement le contexte de l’image (`CGContext`) durée de vie pour le développeur n’a pas à vous soucier d’appel commencent et se terminent les commandes de contexte.
- Il est compatible avec le `UIGraphics.GetCurrentContext()` (méthode).

Le `CreateImage` méthode de la `UIGraphicsImageRenderer` classe est appelé pour créer une image couleur large et passé avec le contexte de l’image à dessiner dans un gestionnaire d’achèvement. Tout le dessin est effectué à l’intérieur de ce gestionnaire d’achèvement comme suit :

- Le `UIColor.FromDisplayP3` méthode crée une nouvelle couleur rouge saturée dans la large gamme espace de couleurs d’affichage P3 et il est utilisé pour dessiner la première moitié du rectangle. 
- La seconde moitié du rectangle est dessiné dans le sRGB normal entièrement saturée couleur rouge pour la comparaison.

### <a name="drawing-wide-color-in-macos"></a>Couleur de dessin large dans macOS

La `NSImage` classe a été développée dans macOS Sierra pour prendre en charge le dessin d’images du large couleur. Exemple :

```csharp
var size = CGSize(250,250);
var wideColorImage = new NSImage(size, false, (drawRect) =>{
    var rects = drawRect.Divide(drawRect.Size.Width/2,CGRect.MinXEdge);
    
    var color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    var path = new NSBezierPath(rects.Slice).Fill();
    
    color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    path = new NSBezierPath(rects.Remainder).Fill();
    
    // Return modified
    return true;
});
```

## <a name="rendering-on-screen-images-in-app"></a>Rendu à l’écran de l’image dans l’application

Pour restituer des images du large couleur à l’écran, le processus fonctionne similaire au dessin d’une image hors écran couleur large pour macOS et iOS présentées ci-dessus.

### <a name="rendering-on-screen-in-ios"></a>Rendu à l’écran dans iOS

Lorsque l’application a besoin d’afficher une image en couleur large à l’écran dans iOS, substituer la `Draw` méthode de le `UIView` en question comme d’habitude. Exemple :

```csharp
using System;
using UIKit;
using CoreGraphics;

namespace MonkeyTalk
{
    public class MonkeyView : UIView
    {
        public MonkeyView ()
        {
        }

        public override void Draw (CGRect rect)
        {
            base.Draw (rect);

            // Draw the image to screen
            ...
        }
    }
}
``` 

Comme iOS 10 fait avec la `UIGraphicsImageRenderer` classe illustrée ci-dessus, il décide intelligemment si elle doit être rendu le sRVB ou plage étendue sRGB espace colorimétrique basée sur les fonctionnalités de l’appareil iOS qui l’application est en cours d’exécution lorsque le `Draw` méthode est appelée. En outre, la `UIImageView` a été géré depuis iOS 9.3 par couleur.

Si l’application a besoin de connaître la façon dont le rendu est effectué sur un `UIView` ou `UIViewController`, il peut vérifier le nouveau `DisplayGamut` propriété de la `UITraitCollection` classe. Cette valeur sera une `UIDisplayGamut` enum des opérations suivantes :

- P3
- SRVB
- Non spécifié

Si l’application souhaite de contrôle qui espace colorimétrique est utilisé pour dessiner une image, il peut utiliser un nouveau `ContentsFormat` propriété de la `CALayer` pour spécifier l’espace de couleurs souhaité. Cette valeur peut être un `CAContentsFormat` enum des opérations suivantes :

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>Rendu à l’écran dans macOS

Lorsque l’application a besoin d’afficher une image en couleur large à l’écran dans macOS, substituer la `DrawRect` méthode de le `NSView` en question comme d’habitude. Exemple :

```csharp
using System;
using AppKit;
using CoreGraphics;
using CoreAnimation;

namespace MonkeyTalkMac
{
    public class MonkeyView : NSView
    {
        public MonkeyView ()
        {
        }

        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Draw graphics into the view
            ...
        }
    }
}
```

Là encore, il décide intelligemment si elle doit être rendu le sRVB ou plage étendue sRGB espace colorimétrique basée sur les fonctionnalités du matériel Mac que l’application est en cours d’exécution lorsque le `DrawRect` méthode est appelée.

Si l’application souhaite de contrôle qui espace colorimétrique est utilisé pour dessiner une image, il peut utiliser un nouveau `DepthLimit` propriété de la `NSWindow` classe pour spécifier l’espace de couleurs souhaité. Cette valeur peut être un `NSWindowDepth` enum des opérations suivantes :

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb

## <a name="summary"></a>Récapitulatif

Cet article a couvert couleur large et les méthodes peuvent être implémenté et utilisé à l’intérieur d’une application Xamarin.iOS ou Xamarin.Mac.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
