---
title: "Résumé du chapitre 13. Bitmaps"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 2e511f2ebf75b065469a9051ee5797ac58c147f3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-13-bitmaps"></a>Résumé du chapitre 13. Bitmaps

Le Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) élément affiche une image bitmap. Toutes les plateformes Xamarin.Forms prend en charge les formats de fichier JPEG, PNG, GIF et BMP.

Les images bitmap dans Xamarin.Forms provenant de quatre décimales :

- Sur le web, tel que spécifié par une URL
- Incorporé en tant que ressource dans la bibliothèque de classes Portable courantes
- Incorporé en tant que ressource dans les projets d’application de plateforme
- À partir de n’importe quel emplacement qui peuvent être référencées par .NET `Stream` de l’objet, y compris `MemoryStream`

Ressources bitmap dans la bibliothèque PCL sont indépendantes de la plate-forme, tandis que les ressources bitmap dans les projets de plateforme sont spécifiques à la plateforme.

La bitmap est spécifiée en définissant le [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) propriété du `Image` à un objet de type [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/), une classe abstraite avec trois dérivés :

- [`UriImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) Pour accéder à une image bitmap sur le web basé sur un `Uri` objet défini à sa [ `Uri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.Uri/) propriété
- [`FileImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/) Pour accéder à une image bitmap stockée dans un projet d’application de plateforme basée sur un chemin d’accès du dossier et le fichier défini sur sa [ `File` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FileImageSource.File/) propriété
- [`StreamImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/) pour charger une bitmap à l’aide de .NET `Stream` l’objet spécifié en retournant un `Stream` à partir d’un `Func` défini sur sa [ `Stream` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StreamImageSource.Stream/) propriété

Vous pouvez également (et plus fréquemment), vous pouvez utiliser les méthodes statiques suivantes de la `ImageSource` de classe, qui retournent toutes `ImageSource` objets :

- [`ImageSource.FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) Pour accéder à une image bitmap sur le web basé sur un `Uri` objet
- [`ImageSource.FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) Pour accéder à une image bitmap stockée en tant que ressource incorporée dans l’application de bibliothèque de classes portables, ou [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Type/) ou [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Reflection.Assembly/) pour accéder à une image bitmap dans un autre assembly source
- [`ImageSource.FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) Pour accéder à une image bitmap à partir d’un projet d’application de plateforme
- [`ImageSource.FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) pour charger une image bitmap basée sur un `Stream` objet

Il n’existe aucun équivalent de classe de la `Image.FromResource` méthodes. La `UriImageSource` classe est utile si vous avez besoin de contrôler la mise en cache. La `FileImageSource` classe est utile en XAML. `StreamImageSource` est utile pour le chargement asynchrone de `Stream` des objets, alors que `ImageSource.FromStream` est synchrone.

## <a name="platform-independent-bitmaps"></a>Bitmaps indépendant de la plateforme

Le [ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) projet charge une image bitmap sur le web à l’aide de `ImageSource.FromUri`. Le `Image` élément est défini sur le `Content` propriété de la `ContentPage`, donc il est limité à la taille de la page. Quelle que soit la taille de l’image bitmap, une contrainte `Image` élément est étendu à la taille de son conteneur et le bitmap s’affiche dans sa taille maximale dans le `Image` élément tout en conservant les proportions de l’image bitmap. Zones de la `Image` au-delà de l’image bitmap peut être en couleur avec [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/).

Le [ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) exemple est semblable, mais définit simplement le `Source` URL à la propriété. La conversion est contrôlée par le [ `ImageSourceConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSourceConverter/) classe.

### <a name="fit-and-fill"></a>Ajuster et remplissage

Vous pouvez contrôler la manière dont l’image bitmap est étirée en définissant le [ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) propriété de la `Image` à un des membres suivants de la [ `Aspect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect/) énumération :

- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/): respecte les proportions (par défaut)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/): remplit la zone, ne respecte pas les proportions
- [`AspectFill`](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect.AspectFill/): remplit la zone mais respecte les proportions, faire partie de l’image bitmap de rognage

### <a name="embedded-resources"></a>Ressources incorporées

Vous pouvez ajouter un fichier bitmap à une bibliothèque PCL, ou dans un dossier dans la bibliothèque PCL. Lui donner un **Action de génération** de **EmbeddedResource**. Le [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) exemple montre comment utiliser `ImageSource.FromResource` pour charger le fichier. Le nom de ressource passé à la méthode se compose du nom d’assembly, suivi d’un point, suivie du nom de dossier facultatif et d’un point suivi par le nom de fichier.

Les jeux de programme le `VerticalOptions` et `HorizontalOptions` propriétés de la `Image` à `LayoutOptions.Center`, ce qui rend le `Image` élément sans contrainte. Le `Image` et l’image bitmap ont la même taille :

- Sur iOS et Android, le `Image` est la taille en pixels de l’image bitmap. Il existe un mappage entre les pixels du bitmap et de pixels de l’écran.
- Sur les plateformes Windows Runtime, le `Image` est la taille en pixels de la bitmap dans les unités indépendantes du périphérique. Sur la plupart des périphériques, chaque pixel de la bitmap occupe plusieurs pixels de l’écran.

Le [ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) exemple met un `Image` verticalement `StackLayout` en XAML. Une extension de balisage nommée [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) permet de faire référence à la ressource incorporée dans XAML. Cette classe charge uniquement des ressources à partir de l’assembly dans lequel elle se trouve, donc il ne peut pas être placé dans une bibliothèque.

### <a name="more-on-sizing"></a>Plus d’informations sur le dimensionnement

Il est souvent souhaitable de bitmaps de taille cohérente parmi toutes les plateformes.
Expérimenter **StackedBitmap**, vous pouvez définir un `WidthRequest` sur la `Image` élément verticalement `StackLayout` pour que la taille cohérente parmi les plateformes, mais vous pouvez réduire uniquement la taille à l’aide de cette technique.

Vous pouvez également définir la `HeightRequest` pour que l’image d’une taille cohérente sur les plateformes, mais la largeur limitée de la bitmap limitera la polyvalence de cette technique. Pour une image verticalement `StackLayout`, `HeightRequest` doit être évitée.

La meilleure approche consiste à commencer par une image bitmap plus large que la largeur de téléphone en unités indépendantes du périphérique et définissez `WidthRequest` à la largeur souhaitée en unités indépendantes du périphérique. Cela est illustré dans le [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) exemple.

Le [ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) affiche chapitre 7 de Lewis Carroll *Adventures d’Alice dans Wonderland* avec les illustrations d’origine par John Tenniel :

[![Capture d’écran de triple des tiers de thé mad](images/ch13fg16-small.png "Mad texte du livre thé tiers Hatters")](images/ch13fg16-large.png "texte du livre Mad Hatters thé tiers")

### <a name="browsing-and-waiting"></a>Navigation et en attente

Le [ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) exemple autorise l’utilisateur à parcourir parmi les images stockées sur le site web de Xamarin. Il utilise le .NET `WebRequest` classe pour télécharger un fichier JSON avec la liste des images bitmap.

Le programme utilise une [ `ActivityIndicator` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) pour indiquer que quelque chose se passe. Comme chaque bitmap est le chargement, en lecture seule [ `IsLoading` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.IsLoading/) propriété du `Image` est `true`. Le `IsLoading` propriété est soutenue par une propriété pouvant être liée, par conséquent, un `PropertyChanged` événement est déclenché lorsque cette propriété est modifiée. Le programme attache un gestionnaire pour cet événement et utilise le paramètre actuel de `IsLoaded` pour définir le [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) propriété de la `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Les bitmaps de diffusion en continu

Le `ImageSource.FromStream` méthode crée un `ImageSource` basé sur .NET `Stream`. Doit être passé à la méthode un `Func` objet retourne un `Stream` objet.

### <a name="accessing-the-streams"></a>L’accès à des flux de données

Le [ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) exemple montre comment utiliser la `ImaageSource.FromStream` méthode pour charger une bitmap stockée sous la forme d’une ressource incorporée et charger une image bitmap sur le web.

### <a name="generating-bitmaps-at-run-time"></a>Génération d’images bitmap au moment de l’exécution

Toutes les plateformes Xamarin.Forms prend en charge le format de fichier BMP non compressé, facile à construire dans le code, puis stockez dans un `MemoryStream`. Cette technique permet la création par un algorithme de bitmaps lors de l’exécution, tel qu’implémenté dans le [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) classe dans le **Xamrin.FormsBook.Toolkit** bibliothèque.

Le « faire il vous-même » [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) exemple illustre l’utilisation de `BmpMaker` pour créer une image bitmap avec une image de dégradé.

## <a name="platform-specific-bitmaps"></a>Bitmaps spécifique à la plateforme

Toutes les plateformes Xamarin.Forms autorisent le stockage des images bitmap dans les assemblys d’application de plateforme. Lorsque récupéré par une application de Xamarin.Forms, ces bitmaps de plateforme sont de type [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/). Vous les utilisez pour :

- le [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) propriété de [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)
- le [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) propriété de [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)
- le [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) propriété de `Button`

Les assemblys de plateforme contiennent déjà des bitmaps pour les icônes et écrans de démarrage :

- Dans le projet iOS, dans le **ressources** dossier
- Dans le projet Android, dans les sous-dossiers de le **ressources** dossier
- Dans les projets Windows, dans le **actifs** dossier (bien que les plateformes Windows ne limitent pas les bitmaps à ce dossier)

Le [ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) exemple utilise du code pour afficher une icône dans les projets d’application de plateforme.

### <a name="bitmap-resolutions"></a>Résolutions de bitmap

Toutes les plateformes permettent de stocker plusieurs versions des images bitmap pour les résolutions de différents périphériques. Lors de l’exécution, la version correcte est chargée en fonction de la résolution de l’écran de l’appareil.

Sur iOS, ces bitmaps sont différenciées par un suffixe de nom de fichier :

- Pas de suffixe pour les appareils PPP 160 (1 pixel à l’unité indépendante du périphérique)
- '@2x' suffixe pour les appareils de 320 DPI (2 pixels à le DIU)
- '@3x' suffixe pour les appareils PPP 480 (3 pixels pour la DIU)

Une image bitmap prévue pour être affiché sous la forme un pouce carré existent dans les trois versions :

- MyImage.jpg à 160 pixels carrés
- MyImage@2x.jpg à 320 pixels carrés
- MyImage@3x.jpg à 480 pixels carrés

Le programme fait référence à cette image bitmap en tant que MyImage.jpg, mais la version correcte est récupérée pendant l’exécution en fonction de la résolution de l’écran. Lorsque libres, l’image bitmap est toujours le rendu à 160 unités indépendantes du périphérique.

Pour Android, les bitmaps sont stockés dans les sous-dossiers de le **ressources** dossier :

- drawable-ldpi (basse DPI) pour les appareils 120 PPP (0,75 pixels à la DIU)
- drawable-mdpi (moyen) pour les appareils PPP 160 (1 pixel à la DIU)
- drawable-hdpi (élevée) pour les appareils de 240 points par pouce (1,5 pixels à la DIU)
- drawable-xhdpi (très élevé) pour les appareils de 320 DPI (2 pixels à le DIU)
- drawable-xxhdpi (très très élevé) pour les appareils PPP 480 (3 pixels pour la DIU)
- drawable-xxxhdpi (trois maximales supplémentaires) pour les appareils PPP 640 (4 pixels à la DIU)

Pour une image bitmap destinée à être rendu à un pouce carré, les différentes versions de l’image bitmap auront le même nom mais une taille différente et être stockées dans ces dossiers :

- drawable-ldpi/MyImage.jpg à 120 pixels carrés
- drawable-mdpi/MyImage.jpg à 160 pixels carrés
- drawable-hdpi/MyImage.jpg à 240 pixels carrés
- drawable-xhdpi/MyImage.jpg au carré de 320 pixels
- drawable-xxhdpi/MyImage.jpg à 480 pixels carrés
- drawable-xxxhdpi/MyImage.jpg à 640 pixels carrés

La bitmap est toujours le rendu à 160 unités indépendantes du périphérique. (Le modèle de solution Xamarin.Forms standard inclut uniquement les hdpi, xhdpi et les dossiers xxhdpi.)

Les projets Windows Runtime prend en charge une image bitmap d’affectation de noms schéma constitué d’un facteur d’échelle en pixels indépendants du périphérique unitaire sous forme de pourcentage, par exemple :

- MyImage.scale-200.jpg à 320 pixels carrés

Seuls certains pourcentages sont valides. Les exemples de programmes de ce livre d’incluent uniquement les images avec **200 à l’échelle** suffixes, mais les modèles de solution Xamarin.Forms actuels incluent **échelle-100**, **125 à l’échelle**, **échelle-150**, et **400 à l’échelle**. 

Lors de l’ajout de bitmaps pour les projets de plateforme, le **Action de génération** doit être :

- iOS : **BundleResource**
- Android : **AndroidResource**
- Windows Runtime : **contenu**

Le [ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) exemple crée deux objets de type bouton consistant `Image` éléments avec un `TapGestureRecognizer` installé. Il est prévu que les objets soient un pouce carré. Le `Source` propriété du `Image` est définie à l’aide de `OnPlatform` et `On` objets pour faire référence à des noms de fichiers potentiellement différents sur les plateformes. Les images bitmap incluent les numéros d’indiquant leur taille en pixels, afin de voir le bitmap de la taille est récupéré et rendu.

### <a name="toolbars-and-their-icons"></a>Barres d’outils et les icônes

Une des principales utilisations des bitmaps de plateforme spécifique est la barre d’outils de Xamarin.Forms, qui est construite en ajoutant [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) des objets sur le [ `ToolbarItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.ToolbarItems/) collection définie par `Page`. `ToobarItem` dérive de [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) dont elle hérite de certaines propriétés.

Le plus important `ToolbarItem` propriétés sont :

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) pour le texte peut apparaître en fonction de la plateforme et `Order`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) de type `FileImageSource` pour l’image qui peut s’afficher en fonction de la plateforme et `Order`
- [`Order`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Order/) de type [ `ToolbarItemOrder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItemOrder/), une énumération avec les trois membres, [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Default/), [ `Primary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Primary/), et [ `Secondary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Secondary/).

Le nombre de `Primary` éléments doivent être limités à trois ou quatre. Vous devez inclure un `Text` paramètre pour tous les éléments. Pour la plupart des plateformes, uniquement le `Primary` éléments nécessitent une `Icon` , mais Windows 8.1 nécessite une `Icon` pour tous les éléments. Les icônes doivent avoir 32 unités indépendantes du périphérique carrées. Le `FileImageSource` type indique qu’ils sont spécifiques à la plateforme.

Le `ToolbarItem` se déclenche un [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) événement lorsque tapées, comme un `Button`. `ToolbarItem` prend également en charge [ `Command` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Command/) et [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.CommandParameter/) propriétés souvent utilisées conjointement avec MVVM. (Consultez [chapitre 18, MVVM](chapter18.md)).

IOS et Android nécessitent qu’une page qui affiche une barre d’outils un [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) ou une page de navigation en un `NavigationPage`. Le [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) programme définit le `MainPage` propriété de son `App` classe le [ `NavigationPage` constructeur](https://developer.xamarin.com/api/constructor/Xamarin.Forms.NavigationPage.NavigationPage/p/Xamarin.Forms.Page/) avec un `ContentPage` argument et illustre la construction et des événements du Gestionnaire d’une barre d’outils.

### <a name="button-images"></a>Images des boutons

Vous pouvez également utiliser les bitmaps spécifique à la plateforme pour définir le [ `Image` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Image/) propriété de `Button` à une image bitmap du carré 32 unités indépendantes du périphérique, comme illustré dans le [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) exemple.



## <a name="related-links"></a>Liens associés

- [Chapitre 13 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Exemples de chapitre 13](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Utilisation d’images](~/xamarin-forms/user-interface/images.md)
