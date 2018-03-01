---
title: "Création de ressources pour les différents écrans"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/12/2017
ms.openlocfilehash: aeb7115e3c7521f6679e8802eb759d7e56ba1cfe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-resources-for-varying-screens"></a>Création de ressources pour les différents écrans

Android lui-même s’exécute sur de nombreux appareils différents, chacun possédant une grande variété de résolutions, les tailles d’écran et les densités de l’écran. Android effectuera la mise à l’échelle et le redimensionnement pour que votre application fonctionne sur ces appareils, mais cela peut entraîner une expérience utilisateur optimale. Par exemple, les images peuvent apparaître flous, images peuvent occuper trop (ou n’est pas suffisant) espace à l’écran qui provoque la position des éléments d’interface dans la disposition sera se chevaucher ou être trop éloignées.

<a name="Concepts" />

## <a name="concepts"></a>Concepts

Quelques termes et concepts sont importants à comprendre pour prendre en charge de plusieurs écrans.

- **Taille de l’écran** &ndash; la quantité d’espace physique pour l’affichage de votre application

- **Écran de densité** &ndash; le nombre de pixels dans une zone donnée de l’écran. Unité de mesure par défaut est points par pouce (PPP).

- **Résolution** &ndash; le nombre total de pixels sur l’écran. Lorsque vous développez des applications, la résolution n’est pas aussi importante que la taille de l’écran et la densité.

- **Indépendant de la densité de pixels (dp)** &ndash; il s’agit d’une unité virtuelle pour autoriser les dispositions d’être conçue indépendamment de la densité. Pour convertir un point de distribution en pixels de l’écran, la formule suivante est utilisée :

    px &equals; dp &times; PPP &divide; 160

- **Orientation** &ndash; l’orientation de l’écran est considéré comme paysage lorsqu’il est plus large que verticales. En revanche, l’orientation portrait est lorsque l’écran est plus haut que large. L’orientation peut changer pendant la durée de vie d’une application que l’utilisateur fait pivoter l’appareil.

Notez que les trois premières de ces concepts liés entre &ndash; augmentation de la résolution sans augmenter la densité augmentera la taille de l’écran. Toutefois si la densité et la résolution sont augmentées la taille d’écran peut rester inchangée. Cette relation entre la taille de l’écran, la densité et résolution compliquer très rapidement la prise en charge de l’écran.

Pour vous aider à gérer cette complexité, l’infrastructure Android préfère utiliser *indépendant de la densité de pixels (dp)* pour les dispositions d’écran. À l’aide de pixels indépendants de densité, les éléments d’interface utilisateur seront affiche à l’utilisateur d’avoir la même taille physique sur les écrans de densité différente.

<a name="Supporting_Various_Screen_Sizes_and_Densities" />

## <a name="supporting-various-screen-sizes-and-densities"></a>Prise en charge différentes tailles d’écran et les densités

Android gère la plupart du travail pour afficher les mises en page correctement pour chaque configuration de l’écran. Toutefois, il existe des actions que vous peuvent entreprendre pour aider le système.

L’utilisation de pixels indépendants du densité au lieu de pixels réels de disposition est suffisante dans la plupart des cas, pour garantir l’indépendance de densité.
Android évoluera le drawables lors de l’exécution à la taille appropriée.
Toutefois, il est possible que cette mise à l’échelle entraînera bitmaps flou. Pour éviter ce problème, il peut être nécessaire de fournir des ressources pour les différentes densités de remplacement. Lors de la conception des périphériques pour plusieurs résolutions et des densités écran il s’avère plus facile pour commencer la résolution plus élevée ou la densité de l’image et ensuite à l’échelle. Cela empêchera flou ni distorsion qui peut-être résulter de l’opération de redimensionnement.

<a name="Declare_the_Screen_Size_the_Application_Supports" />

### <a name="declare-the-screen-size-the-application-supports"></a>Déclarez la taille d’écran de l’Application prend en charge

Déclaration de la taille d’écran garantit que seuls les appareils pris en charge peuvent télécharger l’application. Cela est accompli en définissant le [prend en charge les écrans](http://developer.android.com/guide/topics/manifest/supports-screens-element.html) élément dans le **AndroidManifest.xml** fichier. Cet élément est utilisé pour spécifier les tailles d’écran sont pris en charge par l’application. Est considéré comme un écran de prendre en charge l’application puisse correctement de dispositions pour remplir l’écran. À l’aide de cet élément de manifeste, l’application s’afficheront pas dans [ *Google Play* ](https://play.google.com/) pour les appareils qui ne respectent pas les spécifications de l’écran. Toutefois, l’application s’exécutera toujours sur les appareils avec des écrans non pris en charge, mais les dispositions peuvent apparaître floues et granularité.

Pour ce faire, dans Xamarin.Android, il faut d’abord ajouter un **AndroidManifest.xml** s’il n’existe pas de fichier au projet :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Manifeste Android](resources-for-varying-screens-images/01-android-manifest-vs-sml.png)](resources-for-varying-screens-images/01-android-manifest-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Manifeste Android](resources-for-varying-screens-images/01-android-manifest-xs-sml.png)](resources-for-varying-screens-images/01-android-manifest-xs.png)

-----


**AndroidManifest.xml** est ajouté à la **propriétés** active. Le fichier est modifié pour inclure [prend en charge les écrans](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![L’ajout d’écrans de prend en charge](resources-for-varying-screens-images/02-adding-supports-screens-vs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![L’ajout d’écrans de prend en charge](resources-for-varying-screens-images/02-adding-supports-screens-xs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-xs.png)

-----


<a name="Provide_Alternate_Layouts_for_Different_Screen_Sizes" />

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>Fournir différentes dispositions pour différentes tailles d’écran

Bien que Android est redimensionné en fonction de la taille d’écran, ce n’est peut-être pas suffisant dans certains cas. Il peut être souhaitable d’augmenter la taille de certains éléments d’interface utilisateur sur un écran plus grand, ou pour modifier le positionnement des éléments de l’interface utilisateur pour un écran de petite taille.

À partir de 13 de niveau API (Android 3.2), les tailles d’écran sont déconseillées au profit de l’utilisation du logiciel*N*qualificateur de point de distribution. Ce nouveau qualificateur déclare que la quantité d’espace, une disposition donnée a besoin. Il est fortement recommandé que les applications qui sont destinées à s’exécuter sur Android 3.2 ou version ultérieure doivent être à l’aide de ces qualificateurs plus récente.

Par exemple, si une mise en page nécessaire une 700dp minimale de la largeur d’écran, la mise en page à l’autre va passer dans un dossier **sw700dp de disposition**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dossier de disposition pour la largeur d’écran 700dp](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Dossier de disposition pour la largeur d’écran 700dp](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----


En règle générale, sont des nombres pour différents appareils :

- **Téléphone classique** &ndash; 320dp : un téléphone classique

- **Une tablette 5"/ « tweener » appareil** &ndash; 480dp : telles que la Samsung Note

- **Une tablette 7"** &ndash; 600dp : par exemple le Barnes &amp; Nook Noble

- **Une tablette 10 »** &ndash; 720dp : par exemple le Motorola Xoom

Pour les applications que les API cible niveaux jusqu'à 12 (Android 3.1), les dispositions doivent entrer dans les répertoires qui utilisent les qualificateurs **petit**/**normal**/**volumineux**  / **xlarge** comme généralisations de différentes tailles d’écran qui sont disponibles dans la plupart des périphériques. Par exemple, dans l’image ci-dessous, il existe des ressources de remplacement pour les quatre différentes tailles d’écran :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Autres ressources pour les quatre tailles d’écran](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Autres ressources pour les quatre tailles d’écran](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

Voici une comparaison de comparer les qualificateurs de taille d’écran de niveau 13 pre-API antérieures à indépendant de la densité de pixels :

- 426dp x 320dp est **petit**

- 470dp x 320dp est **normal**

- 640dp x 480dp est **volumineux**

- est de 960dp x 720dp **xlarge**

L’écran plus récente dimensionner les qualificateurs dans le niveau de l’API 13 et les ont une priorité plus élevée que les qualificateurs d’écran antérieures de niveaux d’API 12 et inférieures.
Pour les applications qui couvrent les anciens et les nouveaux niveaux d’API, il peut être nécessaire de créer des ressources de remplacement à l’aide de deux ensembles de qualificateurs comme indiqué dans la capture d’écran suivante :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ressources de remplacement avec les deux qualificateurs](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Ressources de remplacement avec les deux qualificateurs](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----


<a name="Provide_Different_Bitmaps_for_Different_Screen_Densities" />

### <a name="provide-different-bitmaps-for-different-screen-densities"></a>Prévoir des Bitmaps différentes densités d’écran différente

Bien que Android évoluera bitmaps en tant que nécessaire pour un périphérique, les bitmaps eux-mêmes ne peuvent pas élégante mettre à l’échelle vers le haut ou vers le bas : elles peuvent être floue ou floues. En fournissant des bitmaps appropriés pour la densité de l’écran s’atténuer ce problème.

Par exemple, l’image ci-dessous est un exemple de disposition et l’apparence des problèmes qui peuvent se produire lorsque la densité spécifiez ressources ne sont pas fournies.

![Captures d’écran sans les ressources de densité](resources-for-varying-screens-images/06-density-not-provided.png)

Comparez cela à une disposition qui est conçue pour des ressources spécifiques de densité :

![Captures d’écran avec des ressources spécifiques à une densité](resources-for-varying-screens-images/07-density-specific-resources.png)

<a name="Create_Varying_Density_Resources_with_Android_Asset_Studio" />

### <a name="create-varying-density-resources-with-android-asset-studio"></a>Créer des ressources de densité avec Asset Android Studio

La création de ces bitmaps des densités différentes peut être un peu fastidieuse. Par conséquent, Google a créé un utilitaire en ligne, ce qui peut réduire certains fastidieuses impliquées dans la création de ces bitmaps appelé le [ **Android Studio Asset**](https://romannurik.github.io/AndroidAssetStudio/).

[![Asset Android Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png)

Ce site Web atout de la création d’images bitmap qui ciblent les densités d’écran courants quatre en fournissant une image. Android Studio actif puis crée les bitmaps avec certaines personnalisations et pouvoir être téléchargé sous la forme d’un fichier zip.

<a name="Tips_for_Multiple_Screens" />

## <a name="tips-for-multiple-screens"></a>Conseils pour les écrans multiples

Android s’exécute sur un nombre déconcertante d’appareils, et la combinaison de tailles d’écran et la densité de l’écran peut sembler insurmontable. Les conseils suivants peuvent aider à réduire les efforts nécessaires pour prendre en charge de différents appareils :

- **Conception et développer pour ce que vous avez besoin uniquement** &ndash; de nombreux différents types d’appareils très utiles, mais certains existent dans les facteurs de forme rares qui peuvent prendre beaucoup d’efforts pour concevoir et développer pour. Le [ **taille de l’écran et la densité** ](http://developer.android.com/resources/dashboard/screens.html) tableau de bord est une page fournie par Google qui fournit des données sur la répartition de la matrice de densité de taille/écran écran. Cette répartition fournit un aperçu sur l’effort de développement de prise en charge des écrans.

- **Utilisez STD plutôt que de Pixels** -Pixels devient problématiques lors de modifications de densité d’écran. Ne pas coder en dur les valeurs de pixels. Évitez les pixels en faveur du point de distribution (indépendant de la densité de pixels).

- **Éviter** [DispositionAbsolue](https://developer.xamarin.com/api/type/Android.Widget.AbsoluteLayout/)
  **possibles, là où** &ndash; il est déconseillé dans le niveau 3 (Android 1.5) de l’API et génère des dispositions fragiles. Elle ne doit pas être utilisée. Au lieu de cela, que vous essayez d’utiliser les widgets de mise en page plus souples comme [ **LinearLayout**](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/), [ **RelativeLayout**](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), ou la nouvelle [ **GridLayout**](https://developer.xamarin.com/api/type/Android.Widget.GridLayout/).

- **Choisir une disposition de l’orientation par défaut** &ndash; , par exemple, au lieu de fournir des ressources autre **terrestres de disposition** et **disposition-port**, placer les ressources pour Paysage dans **disposition**et les ressources pour le mode portrait dans **disposition-port**.

- **Utiliser LayoutParams Height et Width** - lors de la définition des éléments d’interface utilisateur dans un fichier de mise en forme XML, une application Android à l’aide de la **wrap_content** et **fill_parent** valeurs ont plus de succès assurer un aspect approprié sur les différents appareils que l’utilisation des unités indépendantes pixel ou densité. Ces valeurs de dimension entraînent Android à l’échelle les ressources bitmap comme il convient. Pour cette même raison, les unités indépendantes de la densité sont mieux réservées quand spécifiant les marges et marge intérieure des éléments d’interface utilisateur.

<a name="Testing_Multiple_Screens" />

## <a name="testing-multiple-screens"></a>Test de plusieurs écrans

Une application Android doit être testée par rapport à toutes les configurations de prendre en charge. Dans l’idéal, les appareils doivent être testés sur les appareils réels eux-mêmes, mais cela n’est pas possible ou pratique dans de nombreux cas.
Dans ce cas, l’utilisation de l’émulateur et le programme d’installation de périphériques virtuels Android pour chaque configuration de l’appareil sera utile.

Le SDK Android fournit certains émulateur apparences peuvent servir à créer de AVD répliqueront la taille, la densité et la résolution de nombreux appareils.
De même, la plupart des fournisseurs de matériel fournissent des apparences de leurs appareils.

Une autre option consiste à utiliser les services d’un tiers de test de service.
Ces services prend un APK, exécutez-le sur différents appareils et ensuite fournir des commentaires de la façon dont l’application a fonctionné.
