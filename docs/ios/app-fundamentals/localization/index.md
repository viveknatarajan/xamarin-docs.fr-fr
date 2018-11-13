---
title: Localisation dans Xamarin.iOS
description: Ce document décrit les fonctionnalités de localisation iOS et comment utiliser ces fonctionnalités dans les applications Xamarin.iOS. Il aborde la langue, paramètres régionaux, les fichiers de chaînes, images de lancement et bien plus encore.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/28/2017
ms.openlocfilehash: 906489aa3947df24662cbbd0473333caccc032c7
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527259"
---
# <a name="localization-in-xamarinios"></a>Localisation dans Xamarin.iOS

_Ce document décrit les fonctionnalités de localisation du Kit de développement logiciel iOS et comment y accéder avec Xamarin._

Reportez-vous à la [encodages d’internationalisation](encodings.md) pour obtenir des instructions sur l’ajout de pages de code/jeux de caractères dans les applications qui doivent traiter des données non-Unicode.

## <a name="ios-platform-features"></a>fonctionnalités de la plateforme iOS

Cette section décrit certaines des fonctionnalités de localisation dans iOS. Passez à la [section suivante](#basics) pour afficher des fichiers et des exemples de code spécifique.

### <a name="language"></a>Langue

Les utilisateurs choisir leur langue dans le **paramètres** application. Ce paramètre affecte les chaînes de langue et les images affichées par le système d’exploitation et les applications. 

Pour déterminer la langue utilisée dans une application, obtenir le premier élément du `NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Cette valeur sera un code de langue comme `en` pour l’anglais, `es` pour l’espagnol, `ja` pour le japonais, etc. La valeur retournée est limitée à un des localisations pris en charge par l’application (à l’aide de règles de secours pour déterminer la meilleure correspondance).

Code d’application ne doit pas toujours cette valeur – Xamarin et iOS les deux fournissent des fonctionnalités permettant de fournir automatiquement la chaîne appropriée ou la ressource de langue de l’utilisateur. Ces fonctionnalités sont décrites dans le reste de ce document.

> [!NOTE]
> Utilisez `NSLocale.PreferredLanguages` pour déterminer les préférences de langue de l’utilisateur, quel que soit les localisations pris en charge par l’application. Les valeurs retournées par cette méthode modifiée dans iOS 9 ; consultez [TN2418 de Note technique](https://developer.apple.com/library/content/technotes/tn2418/_index.html) pour plus d’informations.

### <a name="locale"></a>Paramètres régionaux

Les utilisateurs choisissent leur paramètres régionaux dans le **paramètres** application. Ce paramètre affecte la façon que les dates, heures, nombres et des devises sont mis en forme.

Cela permet aux utilisateurs de choisir si ils voient les formats d’heure de 12 heures ou 24 heures, si leur séparateur décimal est une virgule ou un point et l’ordre du jour, mois et année dans l’affichage de la date.

Avec Xamarin, vous avez accès aux classes de ces deux Apple iOS (`NSNumberFormatter`), ainsi que les classes .NET dans System.Globalization. Les développeurs doivent déterminer qui convient mieux à leurs besoins, qu’il sont a différentes fonctionnalités disponibles dans chacun. En particulier, si vous êtes récupération et affichage des prix d’achat dans l’application à l’aide de StoreKit vous devez utiliser les classes de mise en forme d’Apple pour les informations de prix retournées.

Les paramètres régionaux actuels peuvent être interrogée à l’aide de deux manières :

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

La première valeur peut être mis en cache par le système d’exploitation et par conséquent, ne peut pas toujours refléter aux paramètres régionaux actuellement sélectionnés de l’utilisateur. Utilisez la deuxième valeur pour obtenir les paramètres régionaux actuellement sélectionnés.

> [!NOTE]
> Mono (le runtime .NET sur laquelle repose Xamarin.iOS) et iOS d’Apple API ne prennent pas en charge les ensembles identiques de combinaisons de langue/région.
> Pour cette raison, il est possible de sélectionner une combinaison de langue/région dans iOS **paramètres** application qui ne mappe pas à une valeur valide dans Mono. Par exemple, la définition de langage d’un iPhone vers l’anglais et sa région à l’Espagne entraîne les API suivantes générer des valeurs différentes :
> 
> - `CurrentThead.CurrentCulture`: en-US (API de Mono)
> - `CurrentThread.CurrentUICulture`: en-US (API de Mono)
> - `NSLocale.CurrentLocale.LocaleIdentifier`: en_ES (API Apple)
>
> Étant donné que Mono utilise `CurrentThread.CurrentUICulture` pour sélectionner des ressources et `CurrentThread.CurrentCulture` pour formater les dates et devises, localisation basée sur les Mono (par exemple, avec les fichiers .resx) peut ne pas fournir les résultats attendus pour ces combinaisons de langue/région. Dans ces situations, s’appuient sur les API d’Apple pour localiser si nécessaire.

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS génère une `NSCurrentLocaleDidChangeNotification` lorsque l’utilisateur met à jour les paramètres régionaux. Les applications peuvent écouter pour cette notification pendant qu’ils sont en cours d’exécution et que vous pouvant apporter les modifications nécessaires à l’interface utilisateur.

## <a name="localization-basics-in-ios"></a>Principes fondamentaux de la localisation dans iOS

Les fonctionnalités suivantes d’e/s sont facilement exploitées dans Xamarin pour fournir des ressources localisées pour l’affichage à l’utilisateur. Reportez-vous à la [TaskyL10n exemple](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) pour voir comment implémenter ces idées.

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Spécifiant la valeur par défaut et les langues prises en charge dans le fichier Info.plist

Dans [technique Q & A QA1828 : comment iOS détermine la langue pour votre application](https://developer.apple.com/library/content/qa/qa1828/_index.html), Apple décrit le mode de sélection d’une langue à utiliser dans une application iOS. Les facteurs suivants un impact sur la langue s’affiche :

- L’utilisateur privilégié du langues (trouvée dans le **paramètres** application)
- Les localisations fourni avec l’application (dossiers .lproj)
- `CFBundleDevelopmentRegion` (**Info.plist** valeur qui spécifie la langue par défaut pour l’application)
- `CFBundleLocalizations` (**Info.plist** tableau spécifiant les localisations tout pris en charge)

Comme indiqué dans la Technical questions et réponses, `CFBundleDevelopmentRegion` représente la région par défaut et la langue d’une application. Si l’application ne gère pas explicitement une des langues par défaut d’un utilisateur, il utilisera la langue spécifiée par ce champ. 

> [!IMPORTANT]
> iOS 11 s’applique ce mécanisme de sélection de langue plus strictement que les versions précédentes du système d’exploitation. Pour cette raison, n’importe quelle application iOS 11 qui ne déclare pas explicitement son localisations pris en charge : soit en y compris les dossiers .lproj ou en définissant une valeur pour `CFBundleLocalizations` – peut d’afficher une autre langue dans iOS 11 qu’elle le faisait dans iOS 10.

Si `CFBundleDevelopmentRegion` n’a pas été spécifié dans le **Info.plist** fichier, les outils de build Xamarin.iOS utilisent actuellement une valeur par défaut de `en_US`. Bien que cela puisse changer dans une version ultérieure, cela signifie que la langue par défaut est l’anglaise.

Pour vous assurer que votre application sélectionne une langue attendue, procédez comme suit :

- Spécifiez une langue par défaut. Ouvrez **Info.plist** et utiliser le **Source** vue pour définir une valeur pour le `CFBundleDevelopmentRegion` clé ; dans XML, il doit ressembler à ce qui suit :

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

Cet exemple utilise « es » pour spécifier que lorsqu’aucun d’un utilisateur préféré langues sont prises en charge, par défaut vers l’espagnol.

- Déclarer des localisations tout pris en charge. Dans **Info.plist**, utilisez le **Source** vue pour définir un tableau pour le `CFBundleLocalizations` de la clé ; dans XML, il doit ressembler à ce qui suit :

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

Les applications Xamarin.iOS qui ont été localisées à l’aide de mécanismes de .NET tels que les fichiers .resx doivent fournir ces **Info.plist** également des valeurs.

Pour plus d’informations sur ces **Info.plist** clés, examinons d’Apple [référence de clé de liste de propriété d’informations](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html).

### <a name="getlocalizedstring-method"></a>GetLocalizedString (méthode)

Le `NSBundle.MainBundle.GetLocalizedString` méthode recherche le texte localisé qui a été stocké dans **.strings** fichiers dans le projet. Ces fichiers sont organisés par langage, dans les répertoires spécialement nommées avec un **.lproj** suffixe (Notez la première lettre de l’extension est un minuscule « L »).

#### <a name="strings-file-locations"></a>emplacements des fichiers .strings

- **Base.lproj** est le répertoire qui contient les ressources pour la langue par défaut.
  Il se trouve souvent dans la racine du projet (mais peut également être placé dans le **ressources** dossier).
- **&lt;langage&gt;.lproj** répertoires sont créés pour chaque langue prise en charge, généralement dans le **ressources** dossier.

Il peut y avoir un nombre de différents **.strings** fichiers dans chaque répertoire de langues :

- **Localizable.Strings** – la liste principale de texte localisé.
- **InfoPlist.strings** : certaines clés spécifiques sont autorisés dans ce fichier pour traduire des choses telles que le nom de l’application.
- **< nom de table de montage séquentiel > .strings** – fichier facultatif qui contient des traductions pour les éléments d’interface utilisateur dans une table de montage séquentiel.

Le **Action de génération** pour ces fichiers doivent être **groupe de ressources**.

#### <a name="strings-file-format"></a>format de fichier .strings

La syntaxe pour les valeurs de chaîne localisée est :

```console
/* comment */
"key"="localized-value";
```

Vous devez échapper les caractères suivants dans les chaînes :

* `\"` guillemet
* `\\` barre oblique inverse
* `\n` saut de ligne

Il s’agit d’un exemple **es/Localizable.strings** (p. ex. Fichier espagnol) à partir de l’exemple :

```console
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="images"></a>Images

Pour localiser une image dans iOS :

1. Reportez-vous à l’image dans le code, par exemple :

    ```csharp
    UIImage.FromBundle("flag");
    ```

2. Placez le fichier d’image par défaut **flag.png** dans **Base.lproj** (le répertoire de langage de développement natif).

3. Vous pouvez également placer des versions localisées de l’image dans **.lproj** dossiers pour chaque langue (par exemple). **es.lproj**, **ja.lproj**). Utiliser le même nom de fichier **flag.png** dans chaque répertoire de langues.

Si une image n’est pas présente pour un langage particulier, iOS revenir vers le dossier de langue par défaut et la charger à partir de là.

#### <a name="launch-images"></a>Images de lancement

Utiliser les conventions d’affectation de noms standards pour les images de lancement (et XIB ou Storyboard pour modèles iPhone 6) lorsque vous les placez dans le **.lproj** répertoires pour chaque langue.

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Nom de l’application

Placer un **InfoPlist.strings** de fichiers dans un **.lproj** directory vous permet de remplacer des valeurs à partir de l’application **Info.plist**, y compris le nom de l’application :

```console
"CFBundleDisplayName" = "LeónTodo";
```

Autres clés que vous pouvez utiliser pour [localiser les chaînes de spécifique à l’application](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) sont :

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

### <a name="dates-and-times"></a>Dates et heures

Bien qu’il soit possible d’utiliser les fonctions de date et d’heure intégrées de .NET (ainsi que l’actuel `CultureInfo`) pour formater les dates et heures pour les paramètres régionaux, ce serait ignorer spécifiques-paramètres utilisateur (qui peuvent être définies séparément à partir de la langue).

Utiliser l’iOS `NSDateFormatter` pour produire une sortie qui correspond à la préférence des paramètres régionaux de l’utilisateur. L’exemple de code suivant montre la date de base et l’heure de mise en forme d’options :

```csharp
var date = NSDate.Now;
var df = new NSDateFormatter ();
df.DateStyle = NSDateFormatterStyle.Full;
df.TimeStyle = NSDateFormatterStyle.Long;
Debug.WriteLine ("Full,Long: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Short;
df.TimeStyle = NSDateFormatterStyle.Short;
Debug.WriteLine ("Short,Short: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Medium;
df.TimeStyle = NSDateFormatterStyle.None;
Debug.WriteLine ("Medium,None: " + df.StringFor(date));
```

Résultats pour l’anglais des États-Unis :

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

Résultats pour l’espagnol d’Espagne :

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Reportez-vous à l’Apple [Date formateurs](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) documentation pour plus d’informations. Lorsque vous testez des paramètres régionaux format de date et heure, vérifiez les deux **iPhone langage** et **région** paramètres.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Mise en page de droite à gauche (DÀG)

iOS offre un certain nombre de fonctionnalités pour vous aider à créer des applications prenant en charge de droite à gauche :

- Du utiliser automatiquement la disposition `leading` et `trailing` attributs pour l’alignement du contrôle (ce qui correspond à gauche et droite pour l’anglais, mais est inversé pour les langues de droite à gauche).
  Le [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) contrôle est particulièrement utile pour la disposition des contrôles pour prendre en charge de droite à gauche.
- Utilisez `TextAlignment = UITextAlignment.Natural` pour l’alignement de texte (qui restent pour la plupart des langages, mais juste pour RTL).
- `UINavigationController` fait pivoter le bouton Précédent et inverse la direction de balayage automatiquement.

L’émission des captures d’écran suivante le [exemple Tasky localisée](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) en arabe et hébreu (bien que l’anglais a été entré dans les champs) :

[![](images/rtl-ar-sml.png "Localisation en arabe")](images/rtl-ar.png#lightbox "Arabic") 

[![](images/rtl-he-sml.png "Localisation en hébreu")](images/rtl-he.png#lightbox "Hebrew")

iOS inverse automatiquement le `UINavigationController`, et les autres contrôles sont placés à l’intérieur `UIStackView` ou est aligné avec la configuration automatique.
Texte de droite à gauche est localisé à l’aide de **.strings** fichiers dans la même façon que le texte de la rétention à long terme.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Localisation de l’interface utilisateur dans le code

Le [Tasky (localisé dans le code)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) montre comment localiser une application dans laquelle l’interface utilisateur est créé dans le code (plutôt que des XIB ou des tables de montage séquentiel).

### <a name="project-structure"></a>Structure de projet

![](images/solution-code.png "Arborescence de ressources")

### <a name="localizablestrings-file"></a>Fichier de Localizable.Strings

Comme décrit ci-dessus, le **Localizable.strings** format de fichier se compose de paires clé-valeur. La clé décrit l’objectif de la chaîne et la valeur est le texte traduit à utiliser dans l’application.

Espagnol (**es**) localisations de l’exemple sont présentées ci-dessous :

```console
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="performing-the-localization"></a>Exécution de la localisation

Dans le code d’application, dans la mesure du texte d’affichage d’une interface utilisateur est défini (qu’il s’agit de texte d’une étiquette, ou espace réservé d’une entrée, etc.) le code utilise l’iOS `GetLocalizedString` fonction pour récupérer la traduction à afficher :

```csharp
var localizedString = NSBundle.MainBundle.GetLocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>Localisation d’interfaces utilisateur de storyboard

L’exemple [Tasky (storyboard localisée)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) montre comment localiser le texte sur les contrôles dans une table de montage séquentiel.

### <a name="project-structure"></a>Structure du projet

Le **Base.lproj** directory contient la table de montage séquentiel et doit également contenir des images utilisées dans l’application.

Contient les autres répertoires de langage un **Localizable.strings** fichier pour les ressources de chaîne référencée dans le code, ainsi qu’un **MainStoryboard.strings** fichier qui contient les traductions de texte dans le table de montage séquentiel.

![](images/solution-storyboard.png "Arborescence de ressources")

Les répertoires de langue doivent contenir une copie de toutes les images qui ont été localisées, pour remplacer celle présente dans **Base.lproj**.

### <a name="object-id--localization-id"></a>ID d’objet / ID de localisation

Lors de la création et la modification des contrôles dans un storyboard, sélectionnez chaque contrôle et vérifier l’ID à utiliser pour la localisation :

- Dans Visual Studio pour Mac, il se trouve dans le **panneau Propriétés** et est appelé **ID de localisation**.
- Dans Xcode, elle est appelée **ID d’objet**.

Cette valeur de chaîne a souvent un formulaire tels que « NF3-h8-xmR », comme illustré dans la capture d’écran suivante :

![](images/xs-designer-localization-id.png "Vue Xcode de localisation de la table de montage séquentiel")

Cette valeur est utilisée dans le **.strings** fichier pour affecter un texte traduit automatiquement à chaque contrôle.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Le format du fichier de traduction de table de montage séquentiel est similaire à la **Localizable.strings** de fichiers, à ceci près que la clé (la valeur sur la gauche) ne peut pas être défini par l’utilisateur, mais au lieu de cela doit avoir un format très spécifique : `ObjectID.property`.

Dans l’exemple **Mainstoryboard.strings** vous trouverez ci-dessous `UITextField`s ont un `placeholder` propriété de texte qui peut être localisée ; `UILabel`s ont un `text` propriété ; et `UIButton`texte par défaut de s est définie à l’aide de `normalTitle`:

```console
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> À l’aide d’une table de montage séquentiel avec les classes de taille, vous risquez de traductions n’apparaissent pas dans l’application. [Notes de publication d’Apple Xcode](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) indiquent qu’une table de montage séquentiel ou un XIB pas localiser correctement si trois éléments sont remplies : il utilise des classes de taille, la localisation de base et la cible de génération sont définies sur universel, et la build cible iOS 7.0. La solution consiste à dupliquer votre fichier de chaînes d’animation dans deux fichiers identiques : **MainStoryboard~iphone.strings** et **MainStoryboard~ipad.strings**, comme illustré dans la capture d’écran suivante :
> 
> ![](images/xs-dup-strings.png "Fichiers de chaînes")

<a name="appstore" />

## <a name="app-store-listing"></a>Annonce de l’App Store

Suit le Forum aux questions d’Apple sur [App Store localisation](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) pour entrer des traductions pour chaque pays que votre application est mis en vente. Notez leur avertissement les traductions seront affiche uniquement si votre application contient un texte localisé **.lproj** répertoire pour la langue.

## <a name="summary"></a>Récapitulatif

Cet article aborde les principes fondamentaux de la localisation d’applications iOS à l’aide des fonctions de gestion et de la table de montage séquentiel des ressources intégrées.

Vous pouvez en savoir plus sur i18n et L10n pour les applications iOS, Android et inter-plateformes (y compris Xamarin.Forms) dans [ce guide inter-plateformes](~/cross-platform/app-fundamentals/localization.md).

## <a name="related-links"></a>Liens associés

- [Tasky (localisé dans le code) (exemple)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (storyboard localisée) (exemple)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Guide de la localisation d’Apple](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Vue d’ensemble de la localisation inter-plateformes](~/cross-platform/app-fundamentals/localization.md)
- [Localisation de Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Localisation Android](~/android/app-fundamentals/localization.md)
