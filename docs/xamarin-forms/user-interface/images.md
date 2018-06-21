---
title: Images de Xamarin.Forms
description: Images peuvent être partagées entre les plateformes avec Xamarin.Forms, ils peuvent être chargés spécifiquement pour chaque plateforme, ou ils peuvent être téléchargés pour l’affichage.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: b0fd644f1f3b49a949a3a9ba9aca4c0770f17013
ms.sourcegitcommit: c2d1249cb67b877ee0d9cb8d095ec66fd51d8c31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291349"
---
# <a name="images-in-xamarinforms"></a>Images de Xamarin.Forms

_Images peuvent être partagées entre les plateformes avec Xamarin.Forms, ils peuvent être chargés spécifiquement pour chaque plateforme, ou ils peuvent être téléchargés pour l’affichage._

Les images sont une partie essentielle de la navigation dans l’application, l’utilisation et la personnalisation. Applications de Xamarin.Forms doivent être en mesure de partager des images sur toutes les plateformes, mais peut également afficher des images différentes sur chaque plateforme.

Les images spécifiques à la plateforme sont également requis pour les icônes et écrans de démarrage ; ceux-ci doivent être configurés sur une base de chaque plateforme.

Ce document traite des sujets suivants :

- [ **Images locales** ](#Local_Images) -affichage des images fourni avec l’application, y compris les résolutions natives comme iOS rétine, Android ou UWP versions haute résolution d’une image de résolution.
- [ **Images incorporées** ](#Embedded_Images) -affichage des images incorporées en tant que ressource d’assembly.
- [ **Télécharger des images** ](#Downloading_Images) - télécharger et afficher des images.
- [ **Icônes et écrans de démarrage** ](#Icons_and_splashscreens) -les icônes spécifiques à la plateforme et les images de démarrage.

## <a name="displaying-images"></a>Afficher des Images

Xamarin.Forms utilisant le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) pour afficher des images sur une page. Il possède deux propriétés importantes :

- [`Source`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) -Un [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) instance, fichier, Uri ou une ressource, qui définit l’image à afficher.
- [`Aspect`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) -La taille de l’image dans les limites, qu'il est affiché dans (que ce soit pour stretch, rogner ou letterbox).

[`ImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) instances peuvent être obtenues à l’aide des méthodes statiques pour chaque type de source de l’image :

- [`FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) -Requiert un nom de fichier ou le chemin d’accès qui peut être résolu sur chaque plateforme.
- [`FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) -Requiert un objet Uri, par exemple.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) -Requiert un identificateur de ressource dans un fichier image incorporé dans l’application ou le projet de bibliothèque .NET Standard avec un **Build Action : EmbeddedResource**.
- [`FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) -Requiert un flux qui fournit des données d’image.

Le [ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) propriété détermine comment l’image est redimensionnée pour s’ajuster à la zone d’affichage :

- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/) -Étire l’image pour complètement et exactement remplir la zone d’affichage. Cela peut entraîner l’image en cours altérée.
- [`AspectFill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFill/) -Découpe l’image qui remplit la zone d’affichage tout en conservant l’aspect (ie. sans distorsion).
- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/) -Partout l’image (si nécessaire) afin que l’image entière s’adapte à la zone d’affichage, avec un espace vide ajouté vers le haut/bas ou les côtés selon si l’image est largeur ou hauteur.

Les images peuvent être chargées à partir d’un [fichier local](#Local_Images_in_Xaml), un [ressource incorporée](#embedded_images), ou [téléchargé](#Downloading_Images).

<a name="Local_Images" />

## <a name="local-images"></a>Images locales

Fichiers image peuvent être ajoutées à chaque projet d’application et référencés à partir du code Xamarin.Forms partagé. Pour utiliser une seule image sur toutes les applications, *le même nom de fichier doit être utilisé sur chaque plateforme*, et il doit être un nom de ressource Android valide (ie. uniquement des lettres minuscules, des chiffres, le trait de soulignement et la période sont autorisées).

- **iOS** - le moyen de gérer et prendre en charge les images étant iOS 9 à utiliser de préférence **Asset catalogues Image**, qui doit contenir toutes les versions d’une image qui sont nécessaires pour prendre en charge de différents appareils et facteurs pour mettre à l’échelle un application. Pour plus d’informations, consultez [Ajout d’Images à une ressource catalogue Image défini](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- **Android** -placer des images dans le **drawable/ressources** répertoire **Action de génération : AndroidResource**. Versions haute et basse résolution d’une image peuvent être fournies (dans correctement nommé **ressources** sous-répertoires comme **drawable-ldpi**, **drawable-hdpi**et **drawable-xhdpi**).
- **Plateforme Windows universelle (UWP)** -placer des images dans le répertoire racine de l’application avec **Action de génération : contenu**.

> [!IMPORTANT]
> Avant d’iOS 9, les images ont été généralement placés dans le **ressources** dossier avec **Action de génération : BundleResource**. Toutefois, cette méthode d’utilisation des images dans une application iOS a été déconseillée par Apple. Pour plus d’informations, consultez [tailles d’Image et les noms de fichiers](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Permet du code XAML suivant charger et afficher l’image sur toutes les plateformes adhérant à ces règles pour le nom et la sélection élective :

```xaml
<Image Source="waterfront.jpg" />
```

Le code c# équivalent est comme suit :

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

Les captures d’écran suivantes affichent le résultat de l’affichage d’une image locale sur chaque plateforme :

[![ImageSource local](images-images/local-sml.png "exemple d’Application, afficher une Image de Local")](images-images/local.png#lightbox "exemple d’Application affichant une Image locale")

Pour plus de souplesse les `Device.RuntimePlatform` propriété peut être utilisée pour sélectionner un autre fichier image ou un chemin d’accès pour certains ou l’ensemble des plateformes, comme illustré dans cet exemple de code :

```csharp
image.Source = Device.RuntimePlatform == Device.Android ? ImageSource.FromFile("waterfront.jpg") : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Pour utiliser le même nom de fichier image sur toutes les plateformes, le nom doit être valide sur toutes les plateformes. Android drawables ont de restrictions d’affectation de noms : uniquement des lettres minuscules, nombres, trait de soulignement et période sont autorisés – et pour une compatibilité multiplateforme cela doit être suivi sur toutes les autres plates-formes trop. Le nom de fichier d’exemple **waterfront.png** suit les règles, mais les exemples de noms de fichiers non valides sont « eau front.png », « WaterFront.png », « eau-front.png » et « wåterfront.png ».

<a name="Native_Resolutions" />

### <a name="native-resolutions-retina-and-high-dpi"></a>Résolutions natives (rétine et haute résolution)

iOS, Android et UWP incluent la prise en charge des résolutions différentes, où le système d’exploitation choisit l’image appropriée lors de l’exécution selon les fonctions du périphérique. Xamarin.Forms utilisant API des plateformes natives pour le chargement des images locales, afin qu’il prend automatiquement en charge les résolutions autre si les fichiers sont correctement nommées et situés dans le projet.

La meilleure façon de gérer les images étant iOS 9 à faire glisser des images pour chaque résolution requise pour l’ensemble d’images de catalogue asset approprié. Pour plus d’informations, consultez [Ajout d’Images à une ressource catalogue Image défini](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Avant d’iOS 9, versions rétine de l’image peut être placées dans le **ressources** dossier - deux et trois fois la résolution avec un **@2x** ou **@3x**suffixes sur le nom de fichier avant l’extension de fichier (par exemple). **myimage@2x.png**). Toutefois, cette méthode d’utilisation des images dans une application iOS a été déconseillée par Apple. Pour plus d’informations, consultez [tailles d’Image et les noms de fichiers](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Les images de résolution autre Android doivent être placés dans [spécialement nommés de répertoires](http://developer.android.com/guide/practices/screens_support.html) dans le projet Android, comme illustré dans la capture d’écran suivante :

[![Emplacement de l’Image de résolution de plusieurs Android](images-images/xs-highdpisolution-sml.png "emplacement de l’Image de résolution de plusieurs Android")](images-images/xs-highdpisolution.png#lightbox "emplacement de l’Image de résolution de plusieurs Android")

Noms des fichiers image UWP [peuvent être suivis du suffixe `.scale-xxx` avant l’extension de fichier](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), où `xxx` est le pourcentage de mise à l’échelle appliqué à l’élément multimédia, par exemple, **myimage.scale-200.png**. Les images peuvent ensuite faire référence dans le code ou XAML sans le modificateur de mise à l’échelle, par exemple, immédiatement **myimage.png**. La plateforme sélectionne l’échelle des ressources appropriées le plus proche basée sur la résolution actuelle de l’affichage.

### <a name="additional-controls-that-display-images"></a>Autres contrôles qui affichent des Images

Certains contrôles ont des propriétés qui affichent une image, telles que :

- [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) -Toute page de type qui dérive de `Page` a [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) et [ `BackgroundImage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.BackgroundImage/) propriétés, ce qui peuvent être assignées à une référence de fichier local. Dans certaines circonstances, par exemple quand un [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) affiche un [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), l’icône s’affiche si pris en charge par la plateforme.

  > [!IMPORTANT]
  > Sur iOS, le [ `Page.Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) propriété ne peut pas être remplie à partir d’une image dans un ensemble d’images de catalogue asset. Charger à la place, les images d’icône pour le `Page.Icon` propriété à partir de la **ressources** dossier dans le projet iOS.

- [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) -A un [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) propriété qui peut être définie sur une référence de fichier local.
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) -A un [ `ImageSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ImageCell.ImageSource/) propriété qui peut être définie à une image est récupérée à partir d’un fichier local, une ressource incorporée ou un URI.

<a name="embedded_images" />

## <a name="embedded-images"></a>Images incorporées

Images incorporées sont également inclus dans une application (comme des images locales), mais au lieu d’avoir une copie de l’image dans la structure de fichiers de chaque application l’image fichier est incorporé dans l’assembly en tant que ressource. Cette méthode de distribution d’images est particulièrement adaptée à la création de composants, comme l’image est fournie avec le code.

Pour incorporer une image dans un projet, avec le bouton droit pour ajouter de nouveaux éléments et sélectionnez l’image/s que vous souhaitez ajouter. Par défaut, l’image aura **Action de génération : aucun**; il doit être défini **Action de génération : EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](images-images/vs-buildaction.png "Définir l’Action de génération : EmbeddedResource")

Le **Action de génération** peuvent être affichées et modifiées dans le **propriétés** fenêtre pour un fichier.

Dans cet exemple est l’ID de ressource **WorkingWithImages.beach.jpg**.
L’IDE a généré ce comportement par défaut en concaténant le **par défaut Namespace** pour ce projet avec le nom de fichier, à l’aide d’un point (.) entre chaque valeur.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![](images-images/xs-buildaction.png "Définir l’Action de génération : EmbeddedResource")

**Action de génération** peuvent également être affichées et modifiées dans le **propriétés** remplissage pour un fichier.
Cette zone affiche la **ID de ressource** qui est utilisé pour référencer la ressource dans le code. Dans la capture d’écran ci-dessous, le **ID de ressource** est **WorkingWithImages.beach.jpg**.
L’IDE a généré ce comportement par défaut en concaténant le **par défaut Namespace** pour ce projet avec le nom de fichier, à l’aide d’un point (.) entre chaque valeur.
Cet ID peut être modifié dans le **propriétés** remplissage, mais pour ces exemples de la valeur **WorkingWithImages.beach.jpg** sera utilisé.

![](images-images/xs-embeddedproperties.png "Remplissage des propriétés EmbeddedResource")

-----

Si vous placez des images incorporées dans des dossiers au sein de votre projet, les noms de dossiers sont également séparés par des points (.) dans l’ID de ressource. Déplacement de la **beach.jpg** l’image dans un dossier appelé **MyImages** aboutirait à un ID de ressource de **WorkingWithImages.MyImages.beach.jpg**

Le code pour charger une image incorporée transmet simplement le **ID de ressource** à la [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) méthode comme indiqué ci-dessous :

```csharp
var embeddedImage = new Image { Source = ImageSource.FromResource("WorkingWithImages.beach.jpg", typeof(EmbeddedImages).GetTypeInfo().Assembly) };
```

> [!NOTE]
> Pour prendre en charge d’affichage des images incorporées dans le mode de mise en production sur la plateforme Windows universelle, il est nécessaire d’utiliser la surcharge de `ImageSource.FromResource` qui spécifie l’assembly source dans laquelle effectuer la recherche de l’image.

Actuellement, il n’existe aucune conversion implicite pour les identificateurs de ressources. Au lieu de cela, vous devez utiliser [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) ou `new ResourceImageSource()` pour charger des images incorporées.

Les captures d’écran suivantes affichent le résultat de l’affichage d’une image incorporée sur chaque plateforme :

[![ResourceImageSource](images-images/resource-sml.png "exemple d’Application affichant une Image incorporée")](images-images/resource.png#lightbox "exemple d’Application affichant une Image incorporée")

<a name="Embedded_Images_in_Xaml" />

### <a name="using-xaml"></a>À l’aide de XAML

Étant donné qu’aucun convertisseur de type intégré de `string` à `ResourceImageSource`, ces types d’images ne peut pas être chargés en mode natif par le code XAML. Au lieu de cela, une simple extension de balisage XAML personnalisée peut être écrites pour charger des images à l’aide un **ID de ressource** spécifié en XAML :

```csharp
[ContentProperty (nameof(Source))]
public class ImageResourceExtension : IMarkupExtension
{
 public string Source { get; set; }

 public object ProvideValue (IServiceProvider serviceProvider)
 {
   if (Source == null)
   {
     return null;
   }

   // Do your translation lookup here, using whatever method you require
   var imageSource = ImageSource.FromResource(Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);

   return imageSource;
 }
}
```

> [!NOTE]
> Pour prendre en charge d’affichage des images incorporées dans le mode de mise en production sur la plateforme Windows universelle, il est nécessaire d’utiliser la surcharge de `ImageSource.FromResource` qui spécifie l’assembly source dans laquelle effectuer la recherche de l’image.

Pour utiliser cette extension, ajoutez une personnalisée `xmlns` vers le code XAML, en utilisant les valeurs correctes de l’espace de noms et d’assembly pour le projet. La source de l’image peut être définie à l’aide de cette syntaxe : `{local:ImageResource WorkingWithImages.beach.jpg}`. Un exemple de code XAML complet est indiqué ci-dessous :

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage
   xmlns="http://xamarin.com/schemas/2014/forms"
   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
   xmlns:local="clr-namespace:WorkingWithImages;assembly=WorkingWithImages"
   x:Class="WorkingWithImages.EmbeddedImagesXaml">
 <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
   <!-- use a custom Markup Extension -->
   <Image Source="{local:ImageResource WorkingWithImages.beach.jpg}" />
 </StackLayout>
</ContentPage>
```

### <a name="troubleshooting-embedded-images"></a>Dépannage des Images incorporées

<a name="Debugging_Embedded_Images" />

#### <a name="debugging-code"></a>Débogage du code

Comme il est parfois difficile à comprendre pourquoi une ressource image particulière n’est pas en cours de chargement, le débogage code suivant peut être ajouté temporairement à une application afin de vérifier que les ressources sont correctement configurés. Il ne produira pas connus de toutes les ressources incorporées dans l’assembly donné à la <span class="UIItem">Console</span> pour déboguer les problèmes de chargement de la ressource.

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

#### <a name="images-embedded-in-other-projects"></a>Images incorporées dans d’autres projets

Par défaut, le `ImageSource.FromResource` méthode recherche uniquement pour les images dans le même assembly que le code appelant le `ImageSource.FromResource` (méthode). En utilisant le code de débogage ci-dessus, vous pouvez déterminer les assemblys qui contiennent une ressource spécifique en modifiant le `typeof()` instruction à un `Type` connu dans chaque assembly.

Toutefois, l’assembly source en cours de recherche une image incorporée peut être spécifié en tant qu’argument à la `ImageSource.FromResource` méthode :

```csharp
var imageSource = ImageSource.FromResource("filename.png", typeof(MyClass).GetTypeInfo().Assembly);
```

<a name="Downloading_Images" />

## <a name="downloading-images"></a>Télécharger des Images

Les images peuvent être téléchargés automatiquement pour l’affichage, comme indiqué dans le code XAML suivant :

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://xamarin.com/content/images/pages/forms/example-app.png" />
    <Label Text="example-app.png gets downloaded from xamarin.com" />
  </StackLayout>
</ContentPage>
```

Le code c# équivalent est comme suit :

```csharp
var webImage = new Image { Source = ImageSource.FromUri(new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")) };
```

Le [ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) méthode requiert un `Uri` de l’objet et retourne un nouveau [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) qui lit à partir de la `Uri`.

Il existe également une conversion implicite pour les chaînes d’URI, afin de l’exemple suivant fonctionne également :

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

Les captures d’écran suivantes affichent le résultat de l’affichage d’une image à distance sur chaque plateforme :

[![Téléchargé ImageSource](images-images/download-sml.png "exemple d’Application affichant une Image téléchargée")](images-images/download.png#lightbox "exemple d’Application affichant une Image téléchargée")

<a name="Image_Caching" />

### <a name="downloaded-image-caching"></a>La mise en cache de l’Image téléchargée

A [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) prend également en charge la mise en cache des images téléchargées, configurés via les propriétés suivantes :

- [`CachingEnabled`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) -Si la mise en cache est activée (`true` par défaut).
- [`CacheValidity`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) -A `TimeSpan` qui définit la durée pendant laquelle l’image sera stockée localement.

La mise en cache est activée par défaut et stocke l’image localement pendant 24 heures. Pour désactiver la mise en cache pour une image particulière, instanciez la source d’image comme suit :

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri="http://server.com/image" };
```

Pour définir une période de cache spécifique (par exemple, 5 jours) instancier la source d’image comme suit :

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

Mise en cache intégrée rend très facile de prendre en charge des scénarios tels que le défilement des listes d’images, dans lequel vous pouvez définir (ou) une image dans chaque cellule et permettent de s’occuper de rechargement de l’image lorsque vous faites défiler la cellule en vue du cache intégré.

<a name="Icons_and_splashscreens" />

## <a name="icons-and-splashscreens"></a>Icônes et écrans de démarrage

Alors que ne pas liées à la [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) affichage, les icônes d’application et les écrans de démarrage sont également une utilisation importante des images dans les projets de Xamarin.Forms.

Icônes et écrans de démarrage pour les applications de Xamarin.Forms se fait dans les projets d’application. Cela signifie que la génération correctement dimensionné images pour iOS, Android et UWP. Ces images doivent être nommés et situées en fonction des besoins de chacune des plateformes.

## <a name="icons"></a>Icônes

Consultez le [iOS utilisation des Images](~/ios/app-fundamentals/images-icons/index.md), [Google iconographie](http://developer.android.com/design/style/iconography.html), et [des recommandations pour les actifs de mosaïques et d’icônes](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) pour plus d’informations sur la création de ces ressources d’application.

## <a name="splashscreens"></a>Écrans de démarrage

IOS et les applications UWP nécessitent un écran de démarrage (également appelé une image de démarrage écran ou la valeur par défaut).

Reportez-vous à la documentation pour [iOS utilisation des Images](~/ios/app-fundamentals/images-icons/index.md) et [écrans de démarrage](/windows/uwp/launch-resume/splash-screens/) sur le centre de développement Windows.

## <a name="summary"></a>Récapitulatif

Xamarin.Forms offre plusieurs façons d’inclure des images dans une application multiplateforme, ce qui permet de la même image à utiliser sur plusieurs plateformes ou pour les images spécifiques à une plateforme de spécifier. Images téléchargées sont également automatiquement mis en cache, automatisation un scénario courant de codage.

Images d’icône et écran de démarrage d’application sont configurés et configuré que pour les applications non-Xamarin.Forms - suivent les mêmes recommandations que celui utilisée pour des applications spécifiques à la plateforme.

## <a name="related-links"></a>Liens associés

- [WorkingWithImages (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)
- [iOS utilisation des Images](~/ios/app-fundamentals/images-icons/index.md)
- [Iconographie Android](http://developer.android.com/design/style/iconography.html)
- [Recommandations pour les actifs de mosaïques et d’icônes](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
