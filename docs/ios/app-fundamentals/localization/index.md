---
title: Localisation dans Xamarin.iOS
description: Ce document décrit les fonctionnalités de localisation iOS et comment utiliser ces fonctionnalités dans les applications Xamarin.iOS. Elle décrit le langage, paramètres régionaux, les fichiers de chaînes, les images de démarrage et bien plus encore.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: 06758fd8fac62a63c309b173738a8ee889716143
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785264"
---
# <a name="localization-in-xamarinios"></a>Localisation dans Xamarin.iOS

_Ce document couvre les fonctionnalités de localisation du Kit de développement logiciel iOS et comment y accéder avec Xamarin._

Reportez-vous à la [internationalisation encodages](encodings.md) pour obtenir des instructions sur l’inclusion de pages de codes/jeux de caractères dans les applications qui doivent traiter les données non-Unicode.

## <a name="ios-platform-features"></a>Fonctionnalités de la plateforme iOS

Cette section décrit certaines des fonctionnalités de localisation dans iOS. Passez à la [section suivante](#basics) pour afficher des fichiers et des exemples de code spécifique.

### <a name="language"></a>Langue

Les utilisateurs choisir leur langage dans le **paramètres** application. Ce paramètre affecte les chaînes de langue et les images affichées par le système d’exploitation et les applications. 

Pour déterminer la langue utilisée dans une application, obtenir le premier élément de `NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Cette valeur doit être un code de langue tel que `en` pour l’anglais, `es` pour l’espagnol, `ja` pour le japonais, etc. La valeur retournée est limitée à un des localisations pris en charge par l’application (à l’aide de règles de secours afin de déterminer la meilleure correspondance).

Code de l’application n’a pas toujours besoin rechercher cette valeur – Xamarin et iOS à la fois fournissent des fonctionnalités qui aident à fournir automatiquement la bonne chaîne ou la ressource pour la langue de l’utilisateur. Ces fonctionnalités sont décrites dans le reste de ce document.

> [!NOTE]
> Utilisez `NSLocale.PreferredLanguages` pour déterminer les préférences de langue de l’utilisateur, quel que soit les localisations pris en charge par l’application. Les valeurs retournées par cette méthode a changé dans iOS 9. consultez [TN2418 de Note technique](https://developer.apple.com/library/content/technotes/tn2418/_index.html) pour plus d’informations.

### <a name="locale"></a>Paramètres régionaux

Les utilisateurs choisissent les paramètres régionaux dans les **paramètres** application. Ce paramètre affecte la façon que les dates, heures, nombres et des devises sont formatées.

Cela permet aux utilisateurs de choisir si elles consultez formats d’heure 12 heures ou 24 heures leur séparateur décimal est une virgule ou un point et l’ordre du jour, mois et année dans l’affichage de la date.

Avec Xamarin, vous avez accès aux classes d’e/s à la fois Apple (`NSNumberFormatter`), ainsi que les classes .NET Framework dans System.Globalization. Les développeurs doivent déterminer ce qui est mieux adapté à leurs besoins, qu’il sont a différentes fonctionnalités disponibles dans chaque. En particulier, si vous êtes la récupération et affichant les prix d’achat de dans l’application à l’aide de StoreKit, vous devez utiliser les classes de mise en forme d’Apple pour les informations de prix retournées.

Les paramètres régionaux actuels peuvent être interrogé à l’aide de deux façons :

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

La première valeur peut être mis en cache par le système d’exploitation et par conséquent, ne peut pas toujours refléter actuellement sélectionné aux paramètres régionaux de l’utilisateur. La deuxième valeur permet d’obtenir les paramètres régionaux actuellement sélectionnés.

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS génère une `NSCurrentLocaleDidChangeNotification` lorsque l’utilisateur met à jour les paramètres régionaux. Les applications peuvent écouter pour cette notification pendant qu’ils sont en cours d’exécution et apportez les modifications appropriées à l’interface utilisateur.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>Principes fondamentaux de la localisation dans iOS

Les fonctionnalités suivantes d’iOS facilement tirer parties dans Xamarin pour fournir des ressources localisées pour l’affichage à l’utilisateur. Reportez-vous à la [TaskyL10n exemple](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) pour savoir comment implémenter ces idées.

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Par défaut de spécification et les langues prises en charge dans le fichier Info.plist.

Dans [technique Q & A QA1828 : comment iOS détermine la langue de votre application](https://developer.apple.com/library/content/qa/qa1828/_index.html), Apple décrit le mode de sélection d’une langue à utiliser dans une application iOS. Les facteurs suivants un impact sur la langue s’affiche :

- L’utilisateur par défaut de le langues (trouvé dans le **paramètres** application)
- Les localisations fourni avec l’application (dossiers .lproj)
- `CFBundleDevelopmentRegion` (**Info.plist** valeur qui spécifie la langue par défaut pour l’application)
- `CFBundleLocalizations` (**Info.plist** tableau spécifiant localisations toutes prises en charge)

Comme indiqué dans les techniques de Q & r, `CFBundleDevelopmentRegion` représente la région par défaut et la langue d’une application. Si l’application ne gère pas explicitement une des langues par défaut de l’utilisateur, il utilise la langue spécifiée par ce champ. 

> [!IMPORTANT]
> iOS 11 applique ce mécanisme de sélection de langue plus strictement que les versions précédentes du système d’exploitation. Pour cette raison, n’importe quelle application iOS 11 qui ne déclare pas explicitement ses localisations pris en charge : soit en incluant les dossiers .lproj ou de définir une valeur pour `CFBundleLocalizations` – peut d’afficher une langue différente dans iOS 11 qu’avec iOS 10.

Si `CFBundleDevelopmentRegion` n’a pas été spécifié dans le **Info.plist** fichier, les outils de génération Xamarin.iOS utilisent actuellement une valeur par défaut de `en_US`. Cela peut être modifié dans une version ultérieure, cela signifie que la langue par défaut est l’anglaise.

Pour vous assurer que votre application sélectionne une langue attendue, procédez comme suit :

- Spécifiez une langue par défaut. Ouvrir **Info.plist** et utiliser le **Source** vue pour définir une valeur pour le `CFBundleDevelopmentRegion` de la clé ; dans le XML, il doit ressembler à ce qui suit :

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

Cet exemple utilise « es » pour spécifier que lorsqu’aucun d’un utilisateur par défaut langues sont prises en charge, par défaut pour l’espagnol.

- Déclarer les localisations toutes prises en charge. Dans **Info.plist**, utiliser le **Source** vue pour définir un tableau pour le `CFBundleLocalizations` de la clé ; dans le XML, il doit ressembler à ce qui suit :

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

Applications Xamarin.iOS qui ont été localisées à l’aide de mécanismes de .NET tels que les fichiers .resx doivent fournir ces **Info.plist** également des valeurs.

Pour plus d’informations sur ces **Info.plist** clés, examinons d’Apple [référence de clé de liste de propriété informations](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html).

### <a name="localizedstring-method"></a>Méthode LocalizedString

Le `NSBundle.MainBundle.LocalizedString` méthode recherche le texte localisé qui a été stocké dans **.strings** fichiers dans le projet. Ces fichiers sont organisés par langue, dans les répertoires spéciale nommées avec un **.lproj** suffixe.

#### <a name="strings-file-locations"></a>emplacements des fichiers .strings

- **Base.lproj** est le répertoire qui contient des ressources pour la langue par défaut.
  Il se situe généralement à la racine du projet (mais peut également être placée dans le **ressources** dossier).
- **<language>.lproj** répertoires sont créés pour chaque langue prise en charge, généralement dans le **ressources** dossier.

Il peut y avoir un certain nombre de **.strings** fichiers dans chaque répertoire de langue :

- **Localizable.Strings** – la liste principale de texte localisé.
- **InfoPlist.strings** – certaines clés spécifiques sont autorisés dans ce fichier pour convertir les éléments tels que le nom de l’application.
- **< nom de table de montage séquentiel > .strings** – fichier facultatif qui contient des traductions pour les éléments d’interface utilisateur dans une table de montage séquentiel.

Le **Action de génération** pour ces fichiers doivent être **groupe de ressources**.

#### <a name="strings-file-format"></a>format de fichier .strings

La syntaxe pour les valeurs de chaîne localisée est :

```console
/* comment */
"key"="localized-value";
```

Vous devez d’échappement dans les chaînes de caractères suivants :

* `\"`  guillemet
* `\\`  Barre oblique inverse
* `\n`  saut de ligne

Il s’agit d’un exemple **es/Localizable.strings** (c'est-à-dire,). Fichier espagnol) à partir de l’exemple :

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

3. Vous pouvez également placer des versions localisées de l’image dans **.lproj** dossiers pour chaque langue (par exemple). **es.lproj**, **ja.lproj**). Utiliser le même nom de fichier **flag.png** dans chaque répertoire de langage.

Si une image n’est pas présente pour une langue particulière, iOS revenir vers le dossier de langue par défaut et la charger à partir de là.


#### <a name="launch-images"></a>Lancer des Images

Utilisez les conventions d’affectation de noms standards pour les images de démarrage (XIB et table de montage séquentiel pour les modèles iPhone 6) lorsque vous les placez dans le **.lproj** répertoires pour chaque langue.

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Nom de l’application

Placer une **InfoPlist.strings** de fichiers dans un **.lproj** répertoire vous permet de remplacer des valeurs à partir de l’application **Info.plist**, y compris le nom de l’application :

```console
"CFBundleDisplayName" = "LeónTodo";
```

Autres touches que vous pouvez utiliser pour [localiser des chaînes spécifiques à l’application](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) sont :

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

### <a name="dates-and-times"></a>Dates et heures

Bien qu’il soit possible d’utiliser les fonctions de date et d’heure .NET intégrées (ainsi que l’actuel `CultureInfo`) pour formater les dates et heures pour les paramètres régionaux, cela serait ignorer les paramètres régionaux spécifiques-paramètres utilisateur (qui peuvent être définies séparément à partir de la langue).

Utiliser l’iOS `NSDateFormatter` pour produire une sortie qui correspond aux préférences des paramètres régionaux de l’utilisateur. L’exemple de code suivant montre la date de base et l’heure de mise en forme d’options :

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

Résultats pour l’espagnol en Espagne :

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Reportez-vous à l’Apple [Date formateurs](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) documentation pour plus d’informations. Lorsque vous testez régionaux date et heure de mise en forme, cochez à la fois **iPhone langage** et **région** paramètres.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Disposition de droite à gauche (RTL)

iOS offre un certain nombre de fonctionnalités pour aider à la création d’applications prenant en charge de droite à gauche :

* Du utiliser automatiquement la disposition `leading` et `trailing` attributs pour l’alignement du contrôle (qui correspond à gauche et droite pour l’anglais, mais est inversée pour les langues RTL).
  Le [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) contrôle est particulièrement utile pour la disposition des contrôles pour prendre en charge de droite à gauche.
* Utilisez `TextAlignment = UITextAlignment.Natural` pour l’alignement de texte (ce qui reste pour la plupart des langages, mais à droite pour RTL).
* `UINavigationController` renverse le bouton Précédent et inverse le sens de balayage automatiquement.

L’émission des captures d’écran suivante le [exemple Tasky localisée](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) en arabe et hébreu (bien que l’anglais a été entré dans les champs) :

[![](images/rtl-ar-sml.png "Localisation en arabe")](images/rtl-ar.png#lightbox "Arabic") 

[![](images/rtl-he-sml.png "Localisation en hébreu")](images/rtl-he.png#lightbox "Hebrew")

iOS inverse automatiquement le `UINavigationController`, et les autres contrôles sont placés à l’intérieur de `UIStackView` ou aligné avec disposition automatique.
Texte de droite à gauche est localisé à l’aide de **.strings** les fichiers de la même façon sous forme de texte LTR.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Localisation de l’interface utilisateur dans le Code

Le [Tasky (localisé dans le code)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) montre comment localiser une application dans laquelle l’interface utilisateur est créé dans le code (plutôt que XIBs ou storyboards).

### <a name="project-structure"></a>Structure de projet

![](images/solution-code.png "Arborescence des ressources")

### <a name="localizablestrings-file"></a>Fichier de Localizable.Strings

Comme décrit ci-dessus, le **Localizable.strings** format de fichier se compose de paires clé-valeur. La clé décrit l’objectif de la chaîne et la valeur est le texte traduit à utiliser dans l’application.

Espagnol (**es**) localisations de l’exemple sont indiquées ci-dessous :

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

Dans le code d’application, dans la mesure du texte d’affichage d’une interface utilisateur est défini (qu’il s’agit d’une étiquette, ou espace réservé de d’entrée, etc.) le code utilise le fichier iOS `LocalizedString` fonction pour extraire la traduction appropriée pour afficher :

```csharp
var localizedString = NSBundle.MainBundle.LocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>Localisation d’interfaces utilisateur de table de montage séquentiel

L’exemple [Tasky (storyboard localisée)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) montre comment localiser le texte sur les contrôles dans une table de montage séquentiel.

### <a name="project-structure"></a>Structure de projet

Le **Base.lproj** répertoire contient le plan conceptuel et doit également contenir des images utilisées dans l’application.

Les autres répertoires de langage contiennent un **Localizable.strings** fichier de toutes les ressources de chaîne référencée dans le code, ainsi qu’un **MainStoryboard.strings** fichier qui contient des traductions pour le texte dans le table de montage séquentiel.

![](images/solution-storyboard.png "Arborescence des ressources")

Les répertoires de langue doivent contenir une copie de toutes les images qui ont été localisées, pour remplacer celle présente dans **Base.lproj**.

### <a name="object-id--localization-id"></a>ID d’objet / ID de localisation

Lors de la création et la modification des contrôles dans une table de montage séquentiel, sélectionnez chaque contrôle et vérifier l’ID à utiliser pour la localisation :

* Dans Visual Studio pour Mac, il se trouve dans le **propriétés remplissage** et est appelé **ID de localisation**.
* dans Xcode, elle est appelée **ID d’objet**.

Cette valeur de chaîne a souvent un formulaire tel que « NF3-h8-xmR », comme indiqué dans la capture d’écran suivante :

![](images/xs-designer-localization-id.png "Vue Xcode de localisation de la table de montage séquentiel")

Cette valeur est utilisée dans les **.strings** fichier comme texte traduit automatiquement à chaque contrôle.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Le format du fichier de traduction de plan conceptuel est similaire à la **Localizable.strings** de fichiers, à ceci près que la clé (la valeur sur la gauche) ne peut pas être définie par l’utilisateur, mais au lieu de cela doit y avoir un format très spécifique : `ObjectID.property`.

Dans l’exemple **Mainstoryboard.strings** ci-dessous, vous pouvez voir `UITextField`s ont un `placeholder` propriété de texte qui peut être localisée ; `UILabel`s ont un `text` propriété ; et `UIButton`texte par défaut de s est définie à l’aide de `normalTitle`:

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
> À l’aide d’une table de montage séquentiel avec les classes de taille peut entraîner des traductions qui n’apparaissent pas dans l’application. [Notes de publication d’Apple Xcode](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) indiquent qu’un storyboard ou les XIB sera localisez pas correctement si trois éléments sont remplies : il utilise des classes de taille, la localisation de base et la cible build sont définies à l’étendue universelle et la génération cible iOS 7.0. La solution consiste à dupliquer votre fichier de chaînes d’animation dans deux fichiers identiques : **MainStoryboard~iphone.strings** et **MainStoryboard~ipad.strings**, comme illustré dans la capture d’écran suivante :
> 
> ![](images/xs-dup-strings.png "Fichiers de chaînes")

<a name="appstore" />

## <a name="app-store-listing"></a>Annonce de l’App Store

Suit le Forum aux questions d’Apple sur [App Store localisation](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) pour entrer des traductions pour chaque pays que votre application est en vente. Notez leur avertissement les traductions seront affiche uniquement si votre application contient un texte localisé **.lproj** répertoire pour la langue.

## <a name="summary"></a>Récapitulatif

Cet article décrit les principes fondamentaux de la localisation d’applications iOS utilisant les fonctionnalités de gestion et de la table de montage séquentiel des ressources intégrées.

Vous pouvez en savoir plus sur i18n et L10n pour les applications iOS, Android et inter-plateformes (y compris les Xamarin.Forms) dans [ce guide inter-plateformes](~/cross-platform/app-fundamentals/localization.md).

## <a name="related-links"></a>Liens associés

- [Tasky (localisé dans le code) (exemple)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (storyboard localisée) (exemple)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Guide de localisation d’Apple](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Vue d’ensemble de la localisation d’inter-plateformes](~/cross-platform/app-fundamentals/localization.md)
- [Localisation de Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Localisation Android](~/android/app-fundamentals/localization.md)
