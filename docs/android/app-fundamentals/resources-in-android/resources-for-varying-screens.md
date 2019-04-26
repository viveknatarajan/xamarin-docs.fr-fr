---
title: Création de ressources pour différents écrans
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/28/2018
ms.openlocfilehash: 63cbe556783ffe22512ff5312817d522120bd15e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013541"
---
# <a name="creating-resources-for-varying-screens"></a>Création de ressources pour différents écrans

Android lui-même s’exécute sur différents appareils, chacun ayant un large éventail de solutions, les tailles d’écran et les densités d’écran. Android effectuera la mise à l’échelle et la redimensionner pour que votre application fonctionne sur ces appareils, mais cela peut entraîner une expérience utilisateur optimale. Par exemple, images pourraient floues, ou ils peuvent être positionnés comme prévu sur une vue.


## <a name="concepts"></a>Concepts

Quelques termes et concepts sont importants à comprendre pour prendre en charge de plusieurs écrans.

- **Taille de l’écran** &ndash; la quantité d’espace physique pour l’affichage de votre application

- **Écran de densité** &ndash; le nombre de pixels dans toute zone donnée sur l’écran. L’unité de mesure classique est points par pouce (PPP).

- **Résolution** &ndash; le nombre total de pixels sur l’écran. Lors du développement d’applications, la résolution n’est pas aussi importante que la densité et la taille de l’écran.

- **Pixels indépendants de densité (dp)** &ndash; une unité virtuelle pour autoriser les mises en page à être conçu indépendant de densité. Cette formule est utilisée pour convertir les dp en pixels de l’écran :

    px &equals; dp &times; dpi &divide; 160

- **Orientation** &ndash; l’orientation de l’écran est considéré comme paysage lorsqu’il est plus large que haute. En revanche, orientation portrait est lorsque l’écran est plus haut que large. L’orientation peut changer pendant la durée de vie d’une application que l’utilisateur fait pivoter l’appareil.

Notez que les trois premiers de ces concepts sont reliés &ndash; augmentation de la résolution sans en augmentant la densité augmentera la taille d’écran. Toutefois si la densité et la résolution sont augmentées, la taille d’écran peut rester inchangée. Cette relation entre la taille de l’écran, densité et la résolution compliquer rapidement la prise en charge de l’écran.

Pour aider à gérer cette complexité, le framework Android préfère utiliser *pixels indépendants de densité (dp)* pour les dispositions d’écran. À l’aide de pixels indépendants de densité, les éléments d’interface utilisateur seront affiche pour l’utilisateur dispose de la même taille physique sur les écrans de densité différente.


## <a name="supporting-various-screen-sizes-and-densities"></a>Prise en charge de différentes tailles d’écran et les densités

Android gère la plupart du travail pour afficher les dispositions correctement pour chaque configuration de l’écran. Toutefois, il existe certaines actions qui peuvent être effectuées pour aider le système.

L’utilisation de pixels à densité indépendante au lieu des pixels réels dans les dispositions est suffisante dans la plupart des cas pour garantir l’indépendance de densité.
Android étendra la drawables lors de l’exécution à la taille appropriée.
Toutefois, il est possible que la mise à l’échelle entraînera bitmaps flou. Pour contourner ce problème, fournissez des ressources de remplacement pour les différentes densités. Lorsque les appareils de conception pour plusieurs résolutions et densités d’écran, il s’avère plus facile pour commencer la résolution plus élevée ou la densité des images et réduire.


### <a name="declare-the-supported-screen-size"></a>Déclarer la taille d’écran prises en charge

Déclaration de la taille d’écran garantit que seuls les appareils pris en charge peuvent télécharger l’application. Cela s’effectue en définissant le [prend en charge-écrans](https://developer.android.com/guide/topics/manifest/supports-screens-element.html) élément dans le **AndroidManifest.xml** fichier. Cet élément est utilisé pour spécifier les tailles d’écran sont prises en charge par l’application. Un écran donné est censé être pris en charge si l’application peut placer correctement ses dispositions pour remplir l’écran. À l’aide de cet élément de manifeste, l’application s’afficheront pas dans [ *Google Play* ](https://play.google.com/) pour les appareils qui ne respectent pas les spécifications de l’écran. Toutefois, l’application s’exécutera sur les périphériques avec écrans non pris en charge, mais les dispositions peuvent sembler floues et sans aspect pixélisé.

Écran pris en charge sixes sont déclarés dans le **Properites/AndroidManifest.xml** fichier de la solution :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-sml.w1581.png)](resources-for-varying-screens-images/01-android-manifest.w1581.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-sml.m761.png)](resources-for-varying-screens-images/01-android-manifest.m761.png#lightbox)

-----

Modifier **AndroidManifest.xml** à inclure [prend en charge-écrans](https://developer.android.com/guide/topics/manifest/supports-screens-element.html):

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="HelloWorld.HelloWorld">
      <uses-sdk android:minSdkVersion="21" android:targetSdkVersion="27" />
      <supports-screens android:resizable="true"
                        android:smallScreens="true"
                        android:normalScreens="true"
                        android:largeScreens="true" />
      <application android:allowBackup="true"
                   android:icon="@mipmap/ic_launcher"
                   android:label="@string/app_name"
                   android:roundIcon="@mipmap/ic_launcher_round"
                   android:supportsRtl="true" android:theme="@style/AppTheme">
  </application>
</manifest>
```

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>Fournir d’autres dispositions pour différentes tailles d’écran


Autres dispositions vous permettent de personnaliser une vue pour une taille d’écran spécifique, modifier le positionnement ou la taille des éléments d’interface utilisateur de composant.

À compter de 13 de niveau API (Android 3.2), les tailles d’écran sont déconseillées au profit de l’à l’aide de la sw*N*qualificateur de point de distribution. Ce nouveau qualificateur déclare que la quantité d’espace une disposition donnée a besoin. Il est recommandé que les applications qui sont destinées à exécuter sur Android 3.2 ou version ultérieure doivent être à l’aide de ces qualificateurs plus récente.

Par exemple, si une mise en page requis un minimum de 700 point de distribution de la largeur d’écran, la mise en page à l’autre se trouve dans un dossier **sw700dp de disposition**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Dossier de disposition pour la largeur d’écran de 700 dp](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![Dossier de disposition pour la largeur d’écran de 700 dp](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----


En règle générale, voici quelques chiffres pour différents appareils :

- **Téléphone standard** &ndash; 320 dp : un téléphone standard

- **Une tablette 5 » / « tweener » appareil** &ndash; 480 dp : telles que la Note de Samsung

- **Une tablette 7"** &ndash; 600 dp : par exemple le Barnes &amp; Nook Noble

- **Une tablette 10 »** &ndash; 720 dp : telles que le Motorola Xoom

Pour les applications que les API cible niveaux jusqu'à 12 (Android 3.1), les dispositions doivent être regroupées dans les répertoires qui utilisent les qualificateurs **petit**/**normal**/**volumineux**  / **très grand** comme généralisations des différentes tailles d’écran qui sont disponibles dans la plupart des périphériques. Par exemple, dans l’image ci-dessous, il existe des ressources de remplacement pour les quatre différentes tailles d’écran :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Autres ressources pour les quatre tailles d’écran](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![Autres ressources pour les quatre tailles d’écran](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

Voici une comparaison de comparer les qualificateurs de taille d’écran de niveau 13 pre-API antérieures à pixels indépendants de densité :

- 426 dp x 320 dp est **petit**

- 470 dp x 320 dp est **normal**

- 640 dp x 480 dp est **volumineux**

- 960 dp x 720 dp est **très grand**

L’écran plus récente qualificateurs dans le niveau d’API 13 de la taille et d’avoir une priorité plus élevée que les qualificateurs d’écran plus anciennes de niveaux d’API 12 et versions antérieures.
Pour les applications qui couvrent les anciennes et les nouveaux niveaux d’API, il peut être nécessaire de créer d’autres ressources à l’aide de ces deux ensembles de qualificateurs comme indiqué dans la capture d’écran suivante :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Autres ressources avec les deux qualificateurs](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

![Autres ressources avec les deux qualificateurs](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----



### <a name="provide-different-bitmaps-for-different-screen-densities"></a>Fournir des bitmaps différentes des densités d’écran

Bien que Android sera l’échelle des bitmaps en fonction des besoins pour un appareil, les bitmaps eux-mêmes ne peuvent pas élégante à l’échelle vers le haut ou vers le bas : elles peuvent devenir floue ou floues. Fournissant des bitmaps appropriés pour la densité de l’écran s’atténuer ce problème.

Par exemple, l’image ci-dessous est un exemple de disposition et l’apparence des problèmes qui peuvent se produire lorsque densité-spécifiez ressources ne sont pas fournies.

![Captures d’écran sans les ressources de densité](resources-for-varying-screens-images/06-density-not-provided.png)

Comparez cela à une disposition qui est conçue pour les ressources spécifiques à la densité :

![Captures d’écran avec des ressources spécifiques à la densité](resources-for-varying-screens-images/07-density-specific-resources.png)


### <a name="create-varying-density-resources-with-android-asset-studio"></a>Créer des différentes ressources de densité avec Android Studio Asset

La création de ces bitmaps de différentes densités peut être un peu fastidieuse. Par conséquent, Google a créé un utilitaire en ligne qui peut réduire certains fastidieuses impliquées dans la création de ces bitmaps appelé le [ **Android Asset Studio**](https://romannurik.github.io/AndroidAssetStudio/).

[![Android Asset Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

Ce site Web indiqueront la création de bitmaps qui ciblent les quatre densités d’écran courants en fournissant une image. Android Asset Studio sera puis créez les bitmaps avec certaines personnalisations et puis les autoriser à être téléchargé dans un fichier zip.


## <a name="tips-for-multiple-screens"></a>Conseils pour plusieurs écrans

Android s’exécute sur un nombre déconcertante d’appareils, et la combinaison de tailles d’écran et densités d’écran peut sembler insurmontable. Les conseils suivants peuvent aider à réduire l’effort nécessaire pour prendre en charge de différents appareils :

- **Seulement concevoir et développer ce dont vous avez besoin** &ndash; il existe de nombreux périphériques ici, mais certains existent dans les facteurs de forme rares qui peuvent prendre des efforts considérables pour concevoir et développer pour. Le [ **taille de l’écran et la densité** ](https://developer.android.com/resources/dashboard/screens.html) tableau de bord est une page fournie par Google qui fournit des données sur la répartition de la matrice de densité de tailles d’écran/écran. Cette répartition fournit un aperçu sur l’effort de développement de la prise en charge des écrans.

- **Utiliser des points de distribution plutôt que de Pixels** -Pixels devient problématiques en tant que les modifications de densité d’écran. Faire pas coder en dur les valeurs de pixel. Évitez les pixels en faveur de point de distribution (pixels indépendants de densité).

- **Éviter** [DispositionAbsolue](https://developer.xamarin.com/api/type/Android.Widget.AbsoluteLayout/)
   **, là où Possible** &ndash; il est déconseillé dans le niveau d’API 3 (Android 1.5) et entraîne des dispositions fragiles. Il ne doit pas être utilisé. Au lieu de cela, essayez d’utiliser les widgets de mise en page plus souples comme [ **LinearLayout**](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/), [ **RelativeLayout**](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), ou au nouveau [ **GridLayout**](https://developer.xamarin.com/api/type/Android.Widget.GridLayout/).

- **Choisir une orientation de disposition par défaut** &ndash; , par exemple, au lieu de fournir des ressources autre **land de disposition** et **disposition-port**, placez les ressources pour mode paysage dans **disposition**et les ressources pour portrait dans **disposition-port**.

- **Utiliser LayoutParams Height et Width** - lors de la définition des éléments d’interface utilisateur dans un fichier XML de disposition, une application Android à l’aide la **wrap_content** et **fill_parent** valeurs ont plus de succès garantir un bon coup de œil au sein de différents appareils que l’utilisation de pixels ou unités indépendantes de la densité. Ces valeurs de dimension entraînent Android à l’échelle les ressources bitmap comme il convient. Pour cette raison même, indépendants de densité unités qui sont mieux réservées pour le moment en spécifiant les marges et marges intérieures des éléments d’interface utilisateur.


## <a name="testing-multiple-screens"></a>Test de plusieurs écrans

Une application Android doit être testée par rapport à toutes les configurations qui seront pris en charge. Dans l’idéal, les appareils doivent être testées sur les périphériques réels eux-mêmes, mais cela n’est pas possible ou pratique dans de nombreux cas.
Dans ce cas, l’utilisation de l’émulateur et le programme d’installation des appareils virtuels Android pour chaque configuration de l’appareil sera utile.

Le SDK Android fournit certaines émulateur apparences peuvent être utilisées pour créer les AVD répliquera la taille, la densité et la résolution de nombreux appareils.
De même, la plupart des fournisseurs de matériel fournissent des apparences pour leurs appareils.

Une autre option consiste à utiliser les services d’un tiers de service de test.
Ces services seront prendre un APK, exécutez-le sur différents appareils, puis fournir des commentaires de la façon dont l’application a fonctionné.
