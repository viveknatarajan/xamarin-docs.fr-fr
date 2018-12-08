---
title: Résumé du chapitre 13. Bitmaps
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 13. Bitmaps'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 737e242e14778f38405845541b2ca30d27c3cf5a
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53059598"
---
# <a name="summary-of-chapter-13-bitmaps"></a>Résumé du chapitre 13. Bitmaps

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)

> [!NOTE] 
> Notes sur cette page indiquent des zones où Xamarin.Forms est différente de la matière présentée dans le livre.

Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image) élément affiche une bitmap. Toutes les plateformes de Xamarin.Forms prend en charge les formats de fichier JPEG, PNG, GIF et BMP.

Bitmaps dans Xamarin.Forms proviennent de quatre endroits :

- Sur le web tel que spécifié par une URL
- Incorporé comme ressource dans la bibliothèque partagée
- Incorporé comme ressource dans les projets d’application de plateforme
- À partir de n’importe quel endroit qui peuvent être référencées par un .NET `Stream` de l’objet, y compris `MemoryStream`

Ressources bitmap dans la bibliothèque partagée sont indépendant de la plateforme, tandis que les ressources de bitmap dans les projets de plateforme sont spécifiques à la plateforme.

> [!NOTE] 
> Le texte du livre fait référence aux bibliothèques de classes portables, qui ont été remplacés par des bibliothèques .NET Standard. Exemples de code à partir de l’ouvrage a été converti pour utiliser les bibliothèques .NET standard.

La bitmap est spécifiée en définissant le [ `Source` ](xref:Xamarin.Forms.Image.Source) propriété du `Image` à un objet de type [ `ImageSource` ](xref:Xamarin.Forms.ImageSource), une classe abstraite avec trois dérivés :

- [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) Pour accéder à une image bitmap sur le web basé sur un `Uri` objet défini sa [ `Uri` ](xref:Xamarin.Forms.UriImageSource.Uri) propriété
- [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) Pour accéder à une image bitmap stockée dans un projet d’application de plateforme basée sur un chemin d’accès de dossier et le fichier défini sur sa [ `File` ](xref:Xamarin.Forms.FileImageSource.File) propriété
- [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource) pour charger une bitmap à l’aide de .NET `Stream` objet spécifié en retournant un `Stream` à partir d’un `Func` définie sur sa [ `Stream` ](xref:Xamarin.Forms.StreamImageSource.Stream) propriété

Vous pouvez également (et plus fréquemment), vous pouvez utiliser les méthodes statiques suivantes de la `ImageSource` de classe, qui retournent toutes `ImageSource` objets :

- [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) Pour accéder à une image bitmap sur le web basé sur un `Uri` objet
- [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) Pour accéder à une image bitmap stockée en tant que ressource incorporée dans l’application de bibliothèque de classes portable ; [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type)) ou [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly)) pour accéder à une image bitmap dans un autre assembly source
- [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) Pour accéder à une image bitmap à partir d’un projet d’application de plateforme
- [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) pour charger une image bitmap selon un `Stream` objet

Il n’existe aucun équivalent de la classe de la `Image.FromResource` méthodes. Le `UriImageSource` classe est utile si vous avez besoin de contrôler la mise en cache. Le `FileImageSource` classe est utile dans XAML. `StreamImageSource` est utile pour le chargement asynchrone de `Stream` objets, tandis que `ImageSource.FromStream` est synchrone.

## <a name="platform-independent-bitmaps"></a>Bitmaps indépendantes de la plate-forme

Le [ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) projet charge une image bitmap sur le web à l’aide `ImageSource.FromUri`. Le `Image` élément est défini sur le `Content` propriété de la `ContentPage`, donc il est limité à la taille de la page. Quelle que soit la taille de la bitmap, une contrainte `Image` élément est étiré pour la taille de son conteneur et le bitmap s’affiche dans sa taille maximale dans le `Image` élément tout en conservant les proportions de la bitmap. Zones de la `Image` au-delà de l’image bitmap peut être en couleur avec [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor).

Le [ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) exemple est similaire, mais se contente de définir le `Source` URL à la propriété. La conversion est contrôlée par le [ `ImageSourceConverter` ](xref:Xamarin.Forms.ImageSourceConverter) classe.

### <a name="fit-and-fill"></a>Ajuster et remplissage

Vous pouvez contrôler la manière dont l’image bitmap est étirée en définissant le [ `Aspect` ](xref:Xamarin.Forms.Image.Aspect) propriété de la `Image` à un des membres suivants de la [ `Aspect` ](xref:Xamarin.Forms.Aspect) énumération :

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): respecte les proportions (valeur par défaut)
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): remplit la zone, ne respecte pas les proportions
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): remplit la zone mais respecte les proportions, faire partie de l’image bitmap de rognage

### <a name="embedded-resources"></a>Ressources incorporées

Vous pouvez ajouter un fichier bitmap pour une bibliothèque de classes portable, ou dans un dossier dans la bibliothèque PCL. Attribuez-lui un **Action de génération** de **EmbeddedResource**. Le [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) exemple montre comment utiliser `ImageSource.FromResource` pour charger le fichier. Le nom de ressource passé à la méthode se compose du nom d’assembly, suivi d’un point, suivie du nom de dossier facultatif et un point, suivi par le nom de fichier.

Les jeux de programme le `VerticalOptions` et `HorizontalOptions` propriétés de la `Image` à `LayoutOptions.Center`, ce qui rend le `Image` élément sans contrainte. Le `Image` et l’image bitmap ont la même taille :

- Sur iOS et Android, le `Image` est la taille en pixels de la bitmap. Il existe une correspondance biunivoque entre les pixels du bitmap et de pixels de l’écran.
- Sur la plateforme Windows universelle, le `Image` est la taille en pixels de l’image bitmap en unités indépendantes du périphérique. Sur la plupart des périphériques, chaque pixel de bitmap occupe plusieurs pixels de l’écran.

Le [ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) exemple met un `Image` dans un vertical `StackLayout` dans XAML. Une extension de balisage nommée [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) permet de référencer la ressource incorporée dans XAML. Cette classe charge uniquement des ressources à partir de l’assembly dans lequel il se trouve, donc il ne peut pas être placé dans une bibliothèque.

### <a name="more-on-sizing"></a>Plus d’informations sur le dimensionnement

Il est souvent souhaitable de bitmaps de taille cohérente parmi toutes les plateformes.
Expérimenter **StackedBitmap**, vous pouvez définir un `WidthRequest` sur le `Image` élément dans un vertical `StackLayout` pour que la taille cohérente entre les plateformes, mais vous pouvez réduire uniquement la taille à l’aide de cette technique.

Vous pouvez également définir le `HeightRequest` pour rendre une image de tailles cohérente sur les plateformes, mais la contrainte largeur de l’image bitmap limitera la polyvalence de cette technique. Pour une image dans un vertical `StackLayout`, `HeightRequest` doit être évitée.

La meilleure approche consiste à commencer par une image bitmap plus large que la largeur de téléphone en unités indépendantes du périphérique et définissez `WidthRequest` largeur souhaitée en unités indépendantes du périphérique. Cela est illustré dans le [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) exemple.

Le [ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) affiche le chapitre 7, de Lewis Carroll *aventures d’Alice au pays des merveilles* avec les illustrations d’origine par John Tenniel :

[![Triple capture d’écran de partie de thé mad](images/ch13fg16-small.png "Mad texte du livre tiers de thé Hatters")](images/ch13fg16-large.png#lightbox "texte du livre Mad Hatters tiers de thé")

### <a name="browsing-and-waiting"></a>Navigation et en attente

Le [ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) exemple permet à l’utilisateur de parcourir les images stockées sur le site web de Xamarin. Il utilise le .NET [ `WebRequest` ](xref:System.Net.WebRequest) classe pour télécharger un fichier JSON avec la liste des images bitmap.

> [!NOTE]
> Xamarin.Forms les programmes doivent utiliser [ `HttpClient` ](xref:System.Net.Http.HttpClient) plutôt que [ `WebRequest` ](xref:System.Net.WebRequest) pour accéder aux fichiers via internet. 

Le programme utilise une [ `ActivityIndicator` ](xref:Xamarin.Forms.ActivityIndicator) pour indiquer que quelque chose se passe. Comme chaque bitmap est le chargement, en lecture seule [ `IsLoading` ](xref:Xamarin.Forms.Image.IsLoading) propriété du `Image` est `true`. Le `IsLoading` propriété repose sur une propriété pouvant être liée, par conséquent, un `PropertyChanged` événement est déclenché lorsque cette propriété est modifiée. Le programme attache un gestionnaire à cet événement et utilise le paramètre actuel de `IsLoaded` pour définir le [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) propriété de la `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Diffusion en continu de bitmaps

Le `ImageSource.FromStream` méthode crée un `ImageSource` basé sur .NET `Stream`. La méthode doit être passée un `Func` objet retourne un `Stream` objet.

### <a name="accessing-the-streams"></a>L’accès à des flux de données

Le [ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) exemple montre comment utiliser le `ImaageSource.FromStream` méthode pour charger une image bitmap stockée en tant que ressource incorporée et pour charger une image bitmap sur le web.

### <a name="generating-bitmaps-at-run-time"></a>Générer des images bitmap au moment de l’exécution

Toutes les plateformes de Xamarin.Forms prend en charge le format de fichier non compressé BMP, qui est facile à construire dans le code, puis stockez dans un `MemoryStream`. Cette technique permet la création de façon algorithmique des bitmaps lors de l’exécution, tel qu’implémenté dans le [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) classe dans le **Xamrin.FormsBook.Toolkit** bibliothèque.

« Faire il vous-même le » [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) exemple illustre l’utilisation de `BmpMaker` pour créer une image bitmap avec une image de dégradé.

## <a name="platform-specific-bitmaps"></a>Bitmaps spécifiques à la plateforme

Toutes les plateformes de Xamarin.Forms permettent de stocker des images bitmap dans les assemblys d’application de plateforme. Lorsque extrait par une application Xamarin.Forms, ces bitmaps de plateforme sont de type [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource). Vous les utilisez pour :

- le [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon) propriété de [`MenuItem`](xref:Xamarin.Forms.MenuItem)
- le [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon) propriété de [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)
- le [ `Image` ](xref:Xamarin.Forms.Button) propriété de `Button`

Les assemblys de plateforme contiennent déjà des bitmaps pour les icônes et écrans de démarrage :

- Dans le projet iOS, dans le **ressources** dossier
- Dans le projet Android, dans les sous-dossiers de le **ressources** dossier
- Dans les projets Windows, dans le **actifs** dossier (bien que les plateformes Windows ne limitent pas les bitmaps à ce dossier)

Le [ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) exemple utilise du code pour afficher une icône dans les projets d’application de plateforme.

### <a name="bitmap-resolutions"></a>Résolutions de bitmap

Toutes les plateformes permettent de stocker plusieurs versions des images bitmap pour les résolutions de périphérique différent. Lors de l’exécution, la version correcte est chargée en fonction de la résolution de l’appareil de l’écran.

Sur iOS, ces bitmaps sont différenciées par le suffixe du nom de fichier :

- Pas de suffixe pour les appareils de 160 DPI (1 pixel à l’unité indépendante du périphérique)
- '@2x' suffixe pour les appareils de 320 DPI (2 pixels à le DIU)
- '@3x' suffixe pour les appareils de 480 PPP (de 3 pixels pour le DIU)

Une image bitmap destinée à être affiché sous forme d’un pouce carré existerait dans trois versions :

- MyImage.jpg à 160 pixels carrés
- MyImage@2x.jpg à 320 pixels carrés
- MyImage@3x.jpg à 480 pixels carrés

Le programme fait référence à cette image bitmap en tant que MyImage.jpg, mais la version correcte est récupérée pendant l’exécution en fonction de la résolution de l’écran. Quand elles sont libres, l’image bitmap restitue toujours à 160 unités indépendantes du périphérique.

Pour Android, les bitmaps sont stockés dans différents sous-dossiers situés de le **ressources** dossier :

- drawable-ldpi (faible PPP) pour les appareils de 120 DPI (0,75 pixels pour le DIU)
- drawable-mdpi (moyenne) pour les appareils de 160 DPI (1 pixel à la DIU)
- drawable-hdpi (élevé) pour les appareils de 240 DPI (1,5 pixels pour le DIU)
- drawable-xhdpi (très élevée) pour les appareils de 320 DPI (2 pixels à le DIU)
- drawable-xxhdpi (très très élevée) pour les appareils de 480 PPP (de 3 pixels pour le DIU)
- drawable-xxxhdpi (trois points forts supplémentaires) pour les appareils de 640 PPP (4 pixels pour le DIU)

Pour une image bitmap destinée à être rendu à un pouce carré, les différentes versions de l’image bitmap auront le même nom mais une taille différente et être stockées dans ces dossiers :

- drawable-ldpi/MyImage.jpg à 120 pixels carrés
- drawable-mdpi/MyImage.jpg à 160 pixels carrés
- drawable-hdpi/MyImage.jpg à 240 pixels carrés
- drawable-xhdpi/MyImage.jpg à 320 pixels carrés
- drawable-xxhdpi/MyImage.jpg à 480 pixels carrés
- drawable-xxxhdpi/MyImage.jpg à 640 pixels carrés

La bitmap est toujours le rendu à 160 unités indépendantes du périphérique. (Le modèle de solution Xamarin.Forms standard inclut uniquement le hdpi, xhdpi et les dossiers xxhdpi.)

Le projet UWP prend en charge un schéma d’affectation de noms bitmap comprenant un facteur d’échelle en pixels indépendants du périphérique unitaire sous forme de pourcentage, par exemple :

- MyImage.scale-200.jpg à 320 pixels carrés

Seuls certains pourcentages sont valides. Les exemples de programmes de ce livre d’incluent uniquement les images avec **mise à l’échelle-200** suffixes, mais les modèles de solution Xamarin.Forms actuels incluent **mise à l’échelle-100**, **mise à l’échelle-125**, **mise à l’échelle-150**, et **mise à l’échelle-400**.

Lors de l’ajout de bitmaps pour les projets de plateforme, le **Action de génération** doit être :

- iOS : **BundleResource**
- Android : **AndroidResource**
- UWP : **contenu**

Le [ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) exemple crée deux objets de type bouton consistant en `Image` éléments avec un `TapGestureRecognizer` installé. Il est prévu que les objets soient carré d’un pouce. Le `Source` propriété du `Image` est définie à l’aide de `OnPlatform` et `On` objets à référencer les noms de fichiers potentiellement différents sur les plateformes. Les images bitmap incluent les numéros d’indiquant leur taille en pixels, pour voir quelle bitmap de taille est récupéré et affiché.

### <a name="toolbars-and-their-icons"></a>Barres d’outils et leurs icônes

Une des principales utilisations des bitmaps de spécifique à la plateforme est la barre d’outils de Xamarin.Forms, qui est construite en ajoutant [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) des objets sur le [ `ToolbarItems` ](xref:Xamarin.Forms.Page.ToolbarItems) collection définie par `Page`. `ToobarItem` dérive de [ `MenuItem` ](xref:Xamarin.Forms.MenuItem) dont elle hérite des propriétés.

Le plus important `ToolbarItem` sont des propriétés :

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) pour le texte qui peut s’afficher en fonction de la plateforme et `Order`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) de type `FileImageSource` pour l’image qui peut s’afficher en fonction de la plateforme et `Order`
- [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) de type [ `ToolbarItemOrder` ](xref:Xamarin.Forms.ToolbarItemOrder), une énumération avec trois membres, [ `Default` ](xref:Xamarin.Forms.ToolbarItemOrder.Default), [ `Primary` ](xref:Xamarin.Forms.ToolbarItemOrder.Primary), et [ `Secondary` ](xref:Xamarin.Forms.ToolbarItemOrder.Secondary).

Le nombre de `Primary` éléments doivent être limités à trois ou quatre. Vous devez inclure un `Text` définissant pour tous les éléments. Pour la plupart des plateformes, uniquement le `Primary` éléments nécessitent une `Icon` mais nécessite Windows 8.1 une `Icon` pour tous les éléments. Les icônes doivent être des unités indépendantes du périphérique 32 carrées. Le `FileImageSource` type indique qu’ils sont spécifiques à la plateforme.

Le `ToolbarItem` se déclenche un [ `Clicked` ](xref:Xamarin.Forms.MenuItem.Clicked) événement lorsque l’utilisateur appuie dessus, comme un `Button`. `ToolbarItem` prend également en charge [ `Command` ](xref:Xamarin.Forms.MenuItem.Command) et [ `CommandParameter` ](xref:Xamarin.Forms.MenuItem.CommandParameter) propriétés souvent utilisées dans le cadre de MVVM. (Consultez [chapitre 18, MVVM](chapter18.md)).

IOS et Android exigent qu’une page qui affiche une barre d’outils un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) ou une page faite par un `NavigationPage`. Le [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) jeux du programme le `MainPage` propriété de son `App` classe à la [ `NavigationPage` constructeur](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page)) avec un `ContentPage` argument et montre le Gestionnaire d’événements et de la construction d’une barre d’outils.

### <a name="button-images"></a>Images de bouton

Vous pouvez également utiliser des images bitmap spécifiques à la plateforme pour définir le [ `Image` ](xref:Xamarin.Forms.Button.Image) propriété de `Button` à une image bitmap du carré 32 unités indépendantes du périphérique, tel qu’indiqué par le [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) exemple.

> [!NOTE]
> L’utilisation d’images sur les boutons a été améliorée. Consultez [à l’aide de bitmaps avec des boutons](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons).

## <a name="related-links"></a>Liens connexes

- [Chapitre 13, texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Exemples de chapitre 13](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Utilisation d’images](~/xamarin-forms/user-interface/images.md)
- [À l’aide de bitmaps avec des boutons](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
