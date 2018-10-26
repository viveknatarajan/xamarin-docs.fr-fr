---
title: Large couleur dans Xamarin.iOS
description: Ce document aborde large couleur et comment elle peut être utilisée dans une application Xamarin.iOS ou Xamarin.Mac. Il fournit également une vue d’ensemble de nombreux concepts liées à la couleur importantes telles que les espaces de couleurs, les canaux et les bases de données primaires.
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: f139bcceda12752e43a3a8330fa0a0e038e539f9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121306"
---
# <a name="wide-color-in-xamarinios"></a>Large couleur dans Xamarin.iOS

_Cet article décrit la couleur de large et comment elle peut être utilisée dans une application Xamarin.iOS ou Xamarin.Mac._

iOS 10 et macOS Sierra améliore la prise en charge pour les formats de pixel de la plage étendue et des espaces d’une gamme de couleurs dans tout le système, y compris les infrastructures telles que graphismes de base, l’Image de Core, complète et AVFoundation. Prise en charge pour les appareils avec des couleurs large est davantage facilité en fournissant ce comportement dans toute la pile de graphique entière.

## <a name="about-wide-color"></a>À propos de large couleur

Comme indiqué ci-dessus, iOS 10 et macOS Sierra améliore la prise en charge pour les formats de pixel de la plage étendue et des espaces d’une gamme de couleurs dans l’ensemble du système, notamment des infrastructures telles que graphismes de base, l’Image de Core, complète et AVFoundation. Prise en charge pour les appareils avec des couleurs large est davantage facilité en fournissant ce comportement dans toute la pile de graphique entière.

Dans les années 90 Apple créé ColorSync pour gérer la couleur de traitement sur le Mac. Ils ont également aidé trouvé l’International Color Consortium (ICC) pour créer et promouvoir un ensemble de normes pour la gestion des couleurs sur du matériel de l’ordinateur. Travail d’Apple avec l’ICC a été inclus dans ColorSync et elle a été intégrée à cœur d’OS X (maintenant appelé macOS).

Apple a également été à la pointe de la technologie d’affichage avec le matériel tels que la rétine afficher, le nouveau P3 et afficher d’espace colorimétrique P3 (publiée en 2015 dans iMac) et le TrueTone affiche les professionnels de l’iPad, iPhone 7 et iPhone 7 Plus.

Large couleur dans macOS Sierra et iOS 10, Apple révolutionne la que macOS et iOS couleur de la poignée pour tirer pleinement parti de ces nouvelles technologies d’affichage.

## <a name="core-color-concepts"></a>Principaux Concepts de couleur

Les concepts de couleur core suivants doivent être abordés avant de passer à étudier plus en couleur dans macOS et iOS :

### <a name="color-space"></a>Espace de couleurs

Un espace de couleurs est un environnement dans lequel les couleurs peuvent être représentés et comparées. Il peut être un espace dimensionnel d’un à quatre qui est défini par l’intensité de ses composants de couleur. 

[![](wide-color-images/color00.png "Un espace de couleurs")](wide-color-images/color00.png#lightbox)

### <a name="color-channels"></a>Canaux de couleur

Les composants de couleur peuvent également être appelés des canaux de couleur. Certaines représentations familiers serait RVB espaces, des espaces de gris, des espaces CMJN ou des espaces indépendants appareil. 

[![](wide-color-images/color02.png "Les composants de couleur peuvent également être appelés des canaux de couleur")](wide-color-images/color02.png#lightbox)

### <a name="color-primaries"></a>Couleurs primaires

Couleurs primaires fournissent le système de coordonnées qui est utilisé pour comparer et de couleurs de calcul. Couleurs primaires situent généralement les plus intense version de la couleur donnée qui peut être générée dans le canal de couleur.

[![](wide-color-images/color01.png "Couleurs primaires fournissent le système de coordonnées qui est utilisé pour comparer et de calcul des couleurs")](wide-color-images/color01.png#lightbox)

Dans le cas de l’espace de couleurs RVB représenté ci-dessus, les couleurs primaires sont où le `1.0` coordonnées sont ancrées (tel que `[1.0, 0.0, 0.0]` pour le rouge).

### <a name="color-gamut"></a>Gamme de couleurs

Gamme de couleurs fait référence à toutes les couleurs qui peuvent être définis comme une combinaison des canaux de couleurs individuelles au sein d’un espace de couleurs donné.

[![](wide-color-images/color03.png "Exemple de gamme de couleur")](wide-color-images/color03.png#lightbox)

## <a name="what-is-wide-color"></a>Qu’est large couleur

Avant d’aborder le sujet de la couleur large, une discussion doit être avait environ le standard en cours, de couleur, l’espace de couleurs RVB Standard (sRVB). Il est l’espace de couleurs plus largement utilisés dans le calcul dès aujourd'hui et l’espace de couleurs par défaut pour iOS et macOS.

L’espace de couleurs sRGB a les propriétés suivantes :

- Il est basé sur la norme ITU-R BT.709.
- Il a une valeur approximative Gamma de 2,2.
- Il représente les conditions d’éclairage classique (D65).

Dans la mesure où sRVB est largement qu'utilisé dans le secteur, un développeur peut formuler des hypothèses qui la couleur spécifiée sera fidèlement représentée sur n’importe quel appareil que qui s’affiche sous. Toutefois, cela peut être pas toujours le cas. En outre, il existe plusieurs couleurs qui ne tiennent pas dans l’espace de couleurs sRVB et c’est pourquoi, ne peut pas être représentées dans celui-ci.

Par exemple, nombreux textile est conçus avec des threads à l’aide de nombreux encres et colorants tomber en dehors de sRVB. Également de nombreux produits rencontre d’une personne dans leur vie quotidienne sont créés avec des couleurs vives qui se situent en dehors de l’espace de couleurs sRGB. Parmi les plus convaincantes de couleurs qui ne peut pas être représentées dans sRVB sont les choses par nature comme couchers de soleil, feuilles d’automne, fleurs exotiques et eaux tropicales.

Les utilisateurs qui ont été capture d’images numériques au format brut peuvent avoir des images sur leurs appareils qui contiennent toutes les données de cette couleur, même s’il ne peut pas être correctement représentée dans l’espace de couleurs sRVB et c’est pourquoi ne peut pas être correctement affichée sur l’écran.

### <a name="the-display-p3-color-space"></a>L’espace de couleurs Display P3

En 2015, Apple publié de nouveaux produits (iMac et iPad Pro 9,7") qui fournissent de l’espace de couleurs de P3 complet pour gérer les problèmes créés par l’espace de couleurs sRGB.

[![](wide-color-images/color04.png "L’espace de couleurs nouveau Display P3")](wide-color-images/color04.png#lightbox)

L’espace de couleurs Display P3 a les propriétés suivantes :

- Prend en charge un espace de couleurs large pour les plateformes matérielles modernes.
- Selon la norme SMPTE DCI-P3. DCI-P3 a été conçu pour les projecteurs numériques mais a été modifié par Apple pour prendre en charge les analyses.
- Il a une valeur approximative Gamma de 2,2.
- Il représente les conditions d’éclairage classique (D65).

En fonction d’Apple, les utilisateurs sont déplacés leurs flux de travail vers leurs plateformes mobiles. Résoudre les problèmes de présentation de couleur présentés par sRGB dans les appareils mobiles professionnels (iPad professionnels), requis plus que simplement, y compris un affichage couleur large. Une des solutions a été mise à niveau de l’étalonnage de fabrique, pour chaque périphérique a été étalonné à la fabrique garantir que d’un périphérique à un autre affichage couleur est exactes et cohérentes.

Une autre solution, est l’inclusion complète, à l’échelle du système de gestion des couleurs qui Apple a intégré à iOS 10 et macOS Sierra. 

### <a name="the-extended-range-srgb-color-space"></a>L’espace de couleurs de plage étendue sRVB

La nouvelle gestion des couleurs de l’échelle du système iOS 10 a pour prendre en compte toutes les applications iOS existantes qui sont créées et paramétrées pour sRVB. Il a été conçu pour vous assurer qu’il n’a pas les performances soit couleur d’une application ou représentation sous forme de ces applications existantes.

Pour gérer cette situation, Apple a inclus la plage étendue sRVB espace colorimétrique dans iOS 10 (et macOS Sierra).

L’espace de couleurs de plage étendue sRVB a les propriétés suivantes :

- Dispose de toutes le bases de données primaires de sRVB même.
- Il a une valeur approximative Gamma de 2,2.
- Il représente les conditions d’éclairage classique (D65).
- Il prend en charge les valeurs négatives et les valeurs supérieur à un (1).

En permettant à des valeurs inférieures à zéro et supérieur à un, la plage étendue sRVB Qu'espace colorimétrique permet non seulement pour les applications existantes à présentes couleurs sRVB sans dégradation des performances ou de distorsion, mais il permet à l’espace de couleurs représenter n’importe quelle couleur à l’intérieur de l’élément visible spectre. Tout ceci est accompli tout en conservant les mêmes points d’ancrage en tant que l’espace de couleurs sRGB.

### <a name="extended-range-srgb-in-action"></a>Plage étendue sRVB en Action

Pour voir comment les valeurs situées en dehors de zéro et un fonctionnent dans l’espace de couleurs de plage étendue sRVB, prenons l’exemple suivant de la de la plus saturée rouge disponible dans l’espace de couleurs Display P3 :

[![](wide-color-images/color05.png "Fonctionnement des valeurs situées en dehors de zéro et un dans l’espace de couleurs de plage étendue sRVB")](wide-color-images/color05.png#lightbox)

Dans Display P3, cette couleur est représentée en tant que `[1.0, 0.0, 0.0]` et dans la plage étendue sRVB, il serait `[1.358, -0.074, -0.012]`. Les valeurs sRVB étant complets relation contenant-contenu à l’intérieur de Display P3 et les valeurs de Display P3 mise en page « extérieur » des plages sRVB.

Pour le matériel physique qui autorise des valeurs de pixel passer d’extrême positif aux valeurs négatives extrêmes, il peut afficher n’importe quelle couleur disponible dans le spectre visible et ces valeurs peuvent être représentés dans l’espace de couleurs de plage étendue sRVB.

### <a name="device-pixel-formats"></a>Formats de Pixel de périphérique 

Normaliser les le sRVB espace colorimétrique n’a été en grande partie sur l’utilisation d’un Format de Pixel de 8 bits, étant donné que les 8 bits par canal de couleur est principalement suffisamment pour décrire les couleurs sRVB. Ce n’est pas parfait, mais suffisamment bon, et vous offre un bon compromis entre l’utilisation du processeur et de mémoire pour afficher des images.

Étant donné que Display P3 peuvent représenter des coordonnées de la couleur en dehors de l’espace de couleurs sRVB, elle nécessite 16 bits par canal de couleur afin de représenter correctement les couleurs avec la plage étendue sRVB espace colorimétrique.

## <a name="system-wide-wide-color-support"></a>Prise en charge de l’échelle du système large couleur

Pour prendre en charge large couleur et large gamme de couleurs à l’intérieur d’iOS 10 et macOS Sierra, Apple a étendu les frameworks suivants pour tirer pleinement parti de l’espace de couleurs de plage étendue sRVB et Display P3 :

- UIKit (pour iOS uniquement)
- SceneKit
- Graphismes de base
- ImageIO
- Image de base
- WebKit
- SpriteKit
- Core Animation
- AppKit (pour macOS uniquement)

Par ailleurs, affichage Retina prise en charge a été améliorée pour l’espace de couleurs de plage étendue sRVB et Display P3 affiche.

Large couleur est pris en charge et peut être utilisé dans les types de contenu d’application suivants :

- Ressources d’image statique inclus dans le bundle d’applications.
- Document et réseau en fonction des ressources d’images.
- Multimédia avancée comme Live des Photos ou des images au format brut.
- Images de la texture de nuanceur graphique 3D.

## <a name="solving-the-color-problem"></a>Résolution du problème de couleur

Le contenu affiché dans une application peut provenir d’un large éventail de sources de riches en couleur. En outre, ce contenu peut être affiché sur un large éventail d’appareils, chacun avec leur propre plage de capacités d’affichage de couleur.

Une application iOS 10 établit un pont entre la différence entre ces deux problèmes en utilisant le nouveau système intégré, couleur gérer système. Ce système garantit qu’une image a le même aspect sur n’importe quel appareil iOS, peu importe quel espace de couleurs, l’image a été codée dans.

Gestion des couleurs commence par chaque image ayant un espace de couleurs associé (ou le profil de couleurs). Ces informations sont utilisées dans le _processus de mise en correspondance de couleur_ où les couleurs dans l’image source sont mis en correspondance avec les couleurs dans le périphérique de sortie. Étant donné que chaque pixel de l’image doit être mis en correspondance de la couleur, il peut prendre beaucoup de temps et placer une contrainte sur le processeur de l’appareil.

En raison de la nature de la _processus de mise en correspondance de couleur_, cette conversion peut être potentiellement » avec perte » si le périphérique de sortie a une gamme plus petite que l’image source.

Heureusement, les calculs qui entrent dans le _processus de mise en correspondance de couleur_ peut facilement être avec accélération matérielle (soit sur l’UC ou GPU) et Apple permet de s’assurer qu’il fonctionne automatiquement en créant la prise en charge dans les systèmes de base telles que Quartz 2D, ColorSync et Core Animation. Pour le contenu correctement avec balises, aucun code n’est nécessaire pour tirer parti de ces fonctionnalités.

Gestion des couleurs a été pris en charge sur chaque plateforme comme suit :

- **macOS** -macOS a été géré depuis le début de couleur.
- **iOS** -iOS a pris en charge la gestion des couleurs automatique depuis iOS 9.3 (et versions ultérieures).

### <a name="designing-for-wide-gamut"></a>Conception pour une large gamme de couleurs

Apple a la suggestion suivante pour la conception et à l’aide de la largeur de couleur, contenu de l’image large gamme de couleurs dans les applications iOS et macOS :

- Utiliser uniquement le contenu de large gamme où dans Vérifiez sens dans l’application, ne doivent pas automatiquement être utilisés partout.
- Utiliser uniquement du contenu de la large gamme de couleurs où les couleurs vives améliorera l’expérience utilisateur.
- Dans n’est pas nécessaire de modifier tout le contenu à P3 pour les applications existantes.

Chaîne d’outils d’Apple permet de prendre en charge pour le contenu d’image large gamme un progressive participer, prenant en charge la large couleur dans une application n’est pas une situation tout ou rien.

### <a name="upgrading-existing-content-to-wide-color"></a>La mise à niveau de contenu existant à large couleur

Apple a les suggestions suivantes pour la mise à niveau de contenu d’image existant à large couleur :

- Ne simplement « attribuer » un profil P3 à du contenu de l’application d’édition d’image. Cela sera remapper simplement la contenu au nouvel espace de couleurs avec des résultats inattendus, tels qu’étirer les couleurs pour tenir dans le nouvel espace, modifiant ainsi l’image de couleur existante.
- Le contenu d’image devrez « convertis » au profil Display P3 à l’aide d’une application d’édition d’image.

### <a name="file-formats-and-color-profiles"></a>Formats de fichier et les profils de couleurs

Apple a les suggestions suivantes pour les formats de fichier et les profils de couleurs utilisés dans large couleur image contenu l’application :

- Utiliser le profil de couleurs « Display P3 » pour les espaces de travail de RVB.
- Utiliser une application 16 bits par mode de canal de couleur.
- Utiliser un iMac Late 2015 (ou version ultérieure) pour prévisualiser avec précision le contenu de l’image.
- Exporter les ressources d’image en tant que fichiers PNG de 16 bits avec un profil « Display P3 » ICC incorporé.

> [!IMPORTANT]
> À l’aide de la **enregistrer pour le Web** ou **exporter les ressources** fonctionnalités se trouvent dans le logiciel d’édition d’images populaires _ne sera pas_ Professionnel pour les images couleur large dans la mesure où ces fonctionnalités n’ont pas été mise à jour pour prendre en charge les spécifications de format de fichier requis encore.

### <a name="supporting-wide-color-with-asset-catalogs"></a>Large couleur avec les catalogues de composants de prise en charge

Dans iOS 10 et macOS Sierra, Apple a étendu les catalogues de composants utilisé pour inclure et classer les contenus d’image statique dans le bundle de l’application pour prendre en charge la large couleur.

À l’aide de catalogues de composants offrent les avantages suivants à une application :

- Ils fournissent la meilleure option de déploiement pour les ressources d’image statique.
- Ils prennent en charge automatiquement des couleurs.
- Ils prennent en charge l’optimisation de format de pixel automatique.
- Ils prennent en charge le découpage de l’application et la réduction de l’application qui garantit que seul le contenu qui est pertinente de get livré à l’appareil de l’utilisateur final.

Apple a fait les améliorations suivantes pour les catalogues de composants pour la prise en charge de la couleur large :

- Ils prennent en charge le contenu de la source de 16 bits (par canal de couleur).
- Ils prennent en charge contenu catalogage par gamme d’affichage. Contenu peut être marqué pour le sRVB ou gammes de Display P3.

Le développeur a trois options pour prendre en charge le contenu de large couleur dans leurs applications :

1. **Ne rien faire** -étant donné que le contenu de large couleur doit uniquement être utilisé dans les situations où l’application a besoin pour afficher les couleurs vives (où ils améliorera l’expérience utilisateur), le contenu en dehors de cette exigence doit être conservé en tant que-est. Il continue à s’afficher correctement dans toutes les situations de matériel.
2. **Mise à niveau de contenu existant vers Display P3** -cela nécessite au développeur de remplacer le contenu d’image existant dans le catalogue de leurs ressources avec un nouveau fichier mis à niveau dans le format de P3 et marquez-la en tant que tel dans l’éditeur de ressources. Au moment de la génération, une image dérivés sRVB sera générée à partir de ces ressources.
3. **Fournir optimisé le contenu de la ressource** -dans ce cas, le développeur fournira un sRVB de 8 bits et une vision de Display P3 16 bits de chaque image dans le catalogue de composants (avec balises correctement dans l’éditeur de ressources).

### <a name="asset-catalog-deployment"></a>Déploiement du catalogue Asset

Les éléments suivants seront produit lorsque le développeur soumet une application à l’App Store avec les catalogues de composants qui incluent le contenu de l’image large couleur :

- Quand l’application est déployée à l’utilisateur final, découpage de l’application garantit que la variante de contenu appropriée est remise à l’appareil de l’utilisateur.
- Sur les appareils qui ne prennent pas en charge les couleurs étendue, il n’existe aucun coût de la charge utile pour inclure le contenu de large couleur, comme il n’est jamais renvoyé à l’appareil.
- `NSImage` sur macOS Sierra (et versions ultérieures) sélectionne automatiquement la meilleure représentation contenue pour l’affichage de votre matériel.
- Le contenu affiché est actualisé automatiquement lorsqu’ou si le matériel des appareils affiche la modification de caractéristiques.

### <a name="asset-catalog-storage"></a>Stockage de catalogue Asset

Stockage de catalogue Asset a les propriétés et les implications d’une application suivantes :

- Au moment de la génération, Apple tente d’optimiser le stockage de contenu de l’image par le biais de la conversion des images haute qualité.
- 16-bits sont utilisés par le canal de couleur pour le contenu de large couleur.
- Contenu de compression d’image dépendante est utilisée pour réduire les tailles de contenu livrable. Nouveau compressions avec « pertes » ont été ajoutées pour optimiser davantage les tailles de contenu.

## <a name="colors-in-user-interfaces"></a>Couleurs dans les Interfaces utilisateur

Lorsque vous travaillez avec les couleurs dans une Interface utilisateur, la plupart des pixels sur l’écran sont dans une couleur unie. En outre, la plupart de ces pixels ne viennent pas de ressources d’images, mais est dessinée directement par l’application (ou par le système d’exploitation pour le compte de l’application).

Large gamme de couleurs peut présenter plusieurs difficultés lors de l’utilisation du niveau de l’interface utilisateur :

- **Communication couleurs** - quand nous parlons des couleurs entre les concepteurs et développeurs, il y a généralement un _supposé_ sRVB espace colorimétrique impliqué. Communiquée une couleur peut être en tant que `rgb(128, 45, 56)` ou `#FF0456`. Dans une conception de la large gamme de couleurs, ces représentations ne fournissent pas suffisamment d’informations pour représenter avec précision la couleur spécifiée, l’espace colorimétrique de travail doit également être inclus. Apple suggère d’utiliser `P3(128, 45, 56)` et `P3#FF0456` à la place. 
- **Sélection des couleurs** - la plupart de retouche d’images populaires et de conception logicielle pèchent par les mêmes limitations que l’espace de couleurs sRGB lors de l’utilisation de leurs sélecteurs de couleurs intégrée. Le concepteur doit vérifier qu’ils sont dans l’espace de couleurs « Display P3 » dans le sélecteur de couleurs lorsque vous travaillez avec des conceptions de couleur large.
- **Codage de couleurs** : les `NSColor` (macOS) et `UIColor` (iOS et tvOS) ont de nouvelles méthodes pratiques pour générer directement des couleurs de P3 et les deux ont été étendues pour prendre en charge les couleurs dans la plage étendue sRVB espace colorimétrique également.
- **Stockage des couleurs** -soyez vigilant lorsque stockage large éventail des couleurs dans les documents d’une application. Où 8 bits par canal de couleur se sont déroulées correctement pour l’espace de couleurs sRVB, 16 bits par canal de couleur doit être utilisé pour les couleurs de larges. Le développeur doit également surveiller les instances de l’espace de couleurs donné (étant donné que tout était traditionnellement sRVB uniquement).

## <a name="colors-on-the-web"></a>Couleurs sur le Web

Être vigilant lorsque vous travaillez avec une large couleur dans les pages web et sur les appareils qui prennent en charge d’affichage large couleur. Si tout le contenu d’image qui a été inclus dans le site Web a été correctement légendés, iOS et macOS automatiquement correspondance de couleurs et les afficher correctement.

Les requêtes de média sont également disponibles pour vous aider à résoudre des ressources entre les appareils compatibles P3 et sRVB :

```xml
<picture>
    <source srcset="monkey-p3.jpg" media="(color-gamut: p3)">
    <source srcset="monkey-rpg.jpg">
</picture>
```

Apple a également une proposition WebKit qui permettra de CSS doit être spécifié dans les autres espaces de couleurs en dehors de l’espace sRVB donné.

## <a name="rendering-off-screen-images-in-app"></a>Rendu des Images hors écran dans une application

Selon le type d’application en cours de création, elle peut autoriser l’utilisateur à inclure le contenu de l’image qu’ils ont collectées à partir d’internet ou créer le contenu de l’image directement à l’intérieur de l’application (par exemple, un vecteur d’application de dessin par exemple).

Dans ces deux cas, l’application peut rendre l’imagerie requise hors écran large couleur à l’aide des fonctionnalités améliorées ajoutées pour iOS et macOS.

### <a name="drawing-wide-color-in-ios"></a>Large couleur de dessin dans iOS

Avant d’aborder la façon de dessiner correctement une image large couleur dans iOS 10, examinons le code de dessin iOS courants suivants :

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

Il existe des problèmes avec le code standard qui devront être adressés _avant_ il peut être utilisé pour dessiner une image large couleur. Le `UIGraphics.BeginImageContext (size)` méthode utilisée pour démarrer le dessin d’image iOS présente les limitations suivantes :

- Il ne peut pas créer des contextes d’image avec plus de 8 bits par canal de couleur.
- Il ne peut pas représenter des couleurs dans la plage étendue sRVB espace colorimétrique.
- Il n’a pas la possibilité de fournir une interface pour créer des contextes dans un espace de couleurs sRVB non en raison des routines de C bas niveau qui est appelées en arrière-plan.

Pour gérer ces limitations et dessiner une image large couleur dans iOS 10, utilisez le code suivant à la place :

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

La nouvelle `UIGraphicsImageRenderer` classe crée un nouveau contexte de l’image qui est capable de gérer l’espace de couleurs de plage étendue sRVB et il présente les caractéristiques suivantes :

- Il est entièrement géré par défaut de couleur.
- Il prend en charge la plage étendue sRVB espace de couleurs par défaut.
- Il décide intelligemment si elle doit être rendu le sRVB ou une plage étendue sRVB espace colorimétrique basée sur les fonctionnalités de l’appareil iOS que l’application est en cours d’exécution.
- Il gère automatiquement et entièrement le contexte de l’image (`CGContext`) durée de vie pour le développeur n’a pas à vous soucier d’appel commencent et se terminent les commandes de contexte.
- Il est compatible avec le `UIGraphics.GetCurrentContext()` (méthode).

Le `CreateImage` méthode de la `UIGraphicsImageRenderer` classe est appelée pour créer une image de couleur large et passée avec le contexte de l’image à dessiner dans un gestionnaire d’achèvement. Tout le dessin est effectué à l’intérieur de ce gestionnaire d’achèvement comme suit :

- Le `UIColor.FromDisplayP3` méthode crée une nouvelle couleur rouge saturée dans la gamme large espace de couleurs Display P3 et il est utilisé pour dessiner la première moitié du rectangle. 
- La seconde moitié du rectangle est dessiné dans le sRVB normal entièrement saturé de couleur rouge pour la comparaison.

### <a name="drawing-wide-color-in-macos"></a>Large couleur de dessin dans macOS

Le `NSImage` classe a été développée dans macOS Sierra pour prendre en charge le dessin d’images de couleur large. Exemple :

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

## <a name="rendering-on-screen-images-in-app"></a>À l’écran de rendu d’Images dans l’application

Pour restituer des images couleur large à l’écran, le processus est similaire au dessin d’une image hors écran large couleur pour macOS et iOS présenté ci-dessus.

### <a name="rendering-on-screen-in-ios"></a>Rendu à l’écran dans iOS

Lorsque l’application doit afficher une image dans un large couleur à l’écran dans iOS, remplacer le `Draw` méthode de la `UIView` en question comme d’habitude. Exemple :

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

Comme le fait avec iOS 10 le `UIGraphicsImageRenderer` classe illustrée ci-dessus, il décide intelligemment s’il doit être rendu le sRVB ou une plage étendue sRVB espace colorimétrique basée sur les fonctionnalités de l’appareil iOS que l’application est en cours d’exécution sur le le `Draw` méthode est appelée. En outre, le `UIImageView` a été géré depuis iOS 9.3 de couleur.

Si l’application doit savoir comment rendu est effectué sur un `UIView` ou `UIViewController`, il peut vérifier le nouveau `DisplayGamut` propriété de la `UITraitCollection` classe. Cette valeur sera une `UIDisplayGamut` enum des opérations suivantes :

- P3
- SRVB
- Non spécifié

Si l’application souhaite de contrôle qui espace colorimétrique est utilisé pour dessiner une image, il peut utiliser un nouveau `ContentsFormat` propriété de la `CALayer` pour spécifier l’espace de couleurs souhaité. Cette valeur peut être un `CAContentsFormat` enum des opérations suivantes :

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>Rendu à l’écran dans macOS

Lorsque l’application doit afficher une image dans un large couleur à l’écran dans macOS, remplacer le `DrawRect` méthode de la `NSView` en question comme d’habitude. Exemple :

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

Là encore, il décide intelligemment s’il doit être rendu le sRVB ou une plage étendue sRVB espace de couleurs en fonction des capacités du matériel Mac que l’application est en cours d’exécution sur le le `DrawRect` méthode est appelée.

Si l’application souhaite de contrôle qui espace colorimétrique est utilisé pour dessiner une image, il peut utiliser un nouveau `DepthLimit` propriété de la `NSWindow` classe pour spécifier l’espace de couleurs souhaité. Cette valeur peut être un `NSWindowDepth` enum des opérations suivantes :

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb

## <a name="summary"></a>Récapitulatif

Cet article a présenté large couleur et les méthodes qu’il peut être implémentée et utilisée à l’intérieur d’une application Xamarin.iOS ou Xamarin.Mac.



## <a name="related-links"></a>Liens associés

- [Exemples iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
