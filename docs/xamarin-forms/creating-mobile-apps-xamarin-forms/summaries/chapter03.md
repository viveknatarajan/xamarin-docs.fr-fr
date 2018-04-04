---
title: Résumé du chapitre 3. Plus de texte
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d4ea9b9da2788ba0cfc9534532fedb5fe83e78a8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Résumé du chapitre 3. Plus de texte

Ce chapitre explore le [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) afficher de manière plus approfondie, y compris les couleurs, polices et la mise en forme.

## <a name="wrapping-paragraphs"></a>Habillage des paragraphes

Lorsque le [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) propriété du `Label` contient du texte long, `Label` encapsule automatiquement à la ligne comme indiqué par le [ **Baskervilles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) exemple. Vous pouvez incorporer des codes Unicode tels que « \u2014 » de la cadratin ou de caractères C# comme '\r' arrêt dans une nouvelle ligne.

Lorsque le [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) et [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) propriétés d’un `Label` sont définies sur `LayoutOptions.Fill`, la taille globale du `Label` est régie par l’espace qui son conteneur met à disposition. Le `Label` est dite *contraint*. La taille de la `Label` est la taille de son conteneur.

Lorsque le `HorizontalOptions` et `VerticalOptions` propriétés sont définies à des valeurs autres que `LayoutOptions.Fill`, la taille de la `Label` est régie par l’espace nécessaire pour restituer le texte, jusqu'à la taille de son conteneur rend disponible pour le `Label`. Le `Label` est dite *sans contrainte* et détermine sa taille.

(Remarque : les termes du contrat *contraint* et *sans contrainte* peut être contre-intuitif, car une vue sans contrainte est généralement inférieure à une vue limitée. En outre, ces termes ne sont pas utilisés régulièrement dans les premiers chapitres du livre.)

Une vue comme une `Label` peuvent être limitées dans une dimension et sans contrainte dans l’autre. Un `Label` sera uniquement habiller le texte sur plusieurs lignes si elle est contraint horizontalement.

Si un `Label` est contraint, il peut-être occuper beaucoup plus d’espace requis pour le texte. Le texte peut être placé dans la zone globale de la `Label`. Définir le [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) propriété vers un membre de la [ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) énumération ([`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/), ou [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)) pour contrôler l’alignement de toutes les lignes du paragraphe. La valeur par défaut est `Start` et aligne à gauche du texte.

Définir le [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) propriété vers un membre de la `TextAlignment` énumération pour positionner le texte en haut, centre ou en bas de la zone occupée par le `Label`.

Définir le [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/) propriété vers un membre de la [ `LineBreakMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) énumération ([`WordWrap`](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.WordWrap/), [ `CharacterWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.CharacterWrap/), [ `NoWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.NoWrap/), [ `HeadTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.HeadTruncation/), [ `MiddleTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.MiddleTruncation/), ou [ `TailTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.TailTruncation/)) à contrôle comment le plusieurs lignes dans un saut de paragraphe ou sont tronqués.

## <a name="text-and-background-colors"></a>Couleurs de texte et d’arrière-plan

Définir le [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) et [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) propriétés de `Label` à [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) valeurs pour contrôler la couleur du texte et d’arrière-plan.

Le `BackgroundColor` s’applique à l’arrière-plan de l’intégralité de la zone occupée par le `Label`. Selon le `HorizontalOptions` et `VerticalOptions` propriétés, que la taille peut être beaucoup plus volumineuses que la zone requise pour afficher le texte. Vous pouvez utiliser des couleurs pour faire des essais avec différentes valeurs de `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, et `VerticalTextAlignment` pour voir comment elles affectent la taille et la position de la `Label`et la taille et la position du texte dans le `Label`.

## <a name="the-color-structure"></a>La structure de couleur

Le [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) structure vous permet de spécifier les couleurs en tant que valeurs de rouge-vert-bleu (RVB) ou des valeurs de Teinte-Saturation-Luminosité (TSL), ou avec un nom de couleur. Un canal Alpha est également disponible pour indiquer la transparence.

Utilisez un `Color` constructeur pour spécifier :

- un [nuance de gris](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/)
- un [valeur RVB](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/)
- un [valeur RVB avec une transparence](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/)

Les arguments sont `double` valeurs comprises entre 0 et 1.

Vous pouvez également utiliser plusieurs méthodes statiques pour créer `Color` valeurs :

- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/) pour `double` valeurs RVB à partir de 0 à 1
- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Int32/System.Int32/System.Int32/) pour les valeurs RVB d’entier comprise entre 0 et 255
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Double/System.Double/System.Double/System.Double/) pour `double` les valeurs RVB avec une transparence
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) pour les valeurs RVB entier avec une transparence
- [`Color.FromHsla`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) pour `double` les valeurs TSL avec une transparence
- [`Color.FromUint`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromUint/p/System.UInt32/) pour un `uint` valeur calculée comme (B + 256 * (G + 256 * (R + 256 * A)))
- [`Color.FromHex`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) pour un `string` format de chiffres hexadécimaux sous la forme « #AARRGGBB » ou « #RRGGBB » ou « #ARGB » ou « #RGB », où chaque lettre correspond à un chiffre hexadécimal pour la valeur alpha, rouge, verts et bleus de canaux. Cette méthode est principale utilisé pour les conversions de couleur XAML, comme indiqué dans [chapitre 7, XAML et code](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

Une fois créé, un `Color` valeur est immuable. Les caractéristiques de la couleur peuvent être obtenus à partir des propriétés suivantes :

- [`R`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/)
- [`G`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/)
- [`B`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/)
- [`A`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/)
- [`Hue`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Hue/)
- [`Saturation`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Saturation/)
- [`Luminosity`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Luminosity/)

Il s’agit de tous les `double` valeurs comprises entre 0 et 1.

`Color` définit également 240 champs statiques publics en lecture seule des couleurs courantes. Au moment de que l’annuaire a été écrit, des couleurs courantes uniquement 17 n’étaient disponibles.

Un autre champ statique public en lecture seule définit une couleur avec tous les canaux de couleur définies sur zéro :

- [`Color.Transparent`](https://developer.xamarin.com/api/field/Xamarin.Forms.Color.Transparent/)

Plusieurs méthodes d’instance autoriser la modification d’une couleur existante pour créer une nouvelle couleur :

- [`AddLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.AddLuminosity/p/System.Double/)
- [`MultiplyAlpha`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.MultiplyAlpha/p/System.Double/)
- [`WithHue`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithHue/p/System.Double/)
- [`WithLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithLuminosity/p/System.Double/)
- [`WithSaturation`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithSaturation/p/System.Double/)

Enfin, deux propriétés statiques en lecture seule définissent la valeur de couleur spécial :

- [`Color.Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), la valeur tous les canaux &ndash;1
- [`Color.Accent`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Accent/)

`Color.Default` Permet d’appliquer le modèle de couleurs de la plateforme, et par conséquent a une signification différente dans différents contextes sur différentes plateformes. Par défaut, les jeux de couleurs de plateforme sont :

- iOS : texte foncé sur un arrière-plan clair
- Android : Clair sur un arrière-plan foncé (dans le livre) sombre texte ou sur un arrière-plan clair (pour la conception de documents via AppCompat dans le **master** branche du référentiel de code d’exemple)
- UWP : Texte de foncé sur un arrière-plan clair
- Windows 8.1 : Texte en clair sur un arrière-plan sombre
- Windows Phone 8.1 : Texte en clair sur un arrière-plan sombre

Le `Color.Accent` valeur des résultats dans une couleur spécifique à la plateforme (et parfois sélectionnables par l’utilisateur) qui est visible sur un arrière-plan sombre ou clair.

## <a name="changing-the-application-color-scheme"></a>Changer les couleurs de l’application

Les différentes plateformes ont un modèle de couleurs par défaut, comme indiqué dans la liste ci-dessus.

Lorsque vous ciblez Android, il est possible de basculer vers un schéma dark-sur-light en spécifiant un thème clair dans le fichier Android.Manifest.xml ou en [AppCompat d’ajout et de documents de conception](~/xamarin-forms/platform/android/appcompat.md).

Pour les plateformes Windows, le thème de couleur est généralement sélectionné par l’utilisateur, mais vous pouvez ajouter un `RequestedTheme` attribut la valeur `Light` ou `Dark` dans le fichier App.xaml de la plate-forme. Par défaut, le fichier App.xaml dans le projet UWP contient un `RequestedTheme` attribut la valeur `Light`.

## <a name="font-sizes-and-attributes"></a>Attributs et les tailles de police

Définir le [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontFamily/) propriété du `Label` à une chaîne telle que « Times Roman » pour sélectionner une famille de polices. Toutefois, vous devez spécifier une famille de polices qui est pris en charge sur la plateforme spécifique, et les plateformes ne sont pas cohérents à cet égard.

Définir le [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontSize/) propriété du `Label` à un `double` pour spécifier la hauteur approximative de la police. Consultez [chapitre 5, vous traitez des tailles](chapter05.md), pour plus d’informations sur le choix intelligemment les tailles de police.

Vous pouvez également obtenir un plusieurs prédéfini dépend de la plateforme de tailles de police. Statiques [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) (méthode) et [surcharge](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/Xamarin.Forms.Element/) retournent un `double` valeur de taille de police appropriée pour la plateforme basée sur les membres de la [ `NamedSize` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NamedSize/)énumération ([`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Default/), [ `Micro` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Micro/), [ `Small` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Small/), [ `Medium` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Medium/),  et [ `Large` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Large/)). La valeur retournée par la `Medium` membre n’est pas nécessairement identique à `Default`. Le [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) exemple affiche le texte avec ces tailles.

Définir le [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontAttributes/) propriété du `Label` à un membre de ces [ `FontAttributes` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FontAttributes/) énumération, [ `Bold` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Bold/), [ `Italic` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Italic/), ou [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.None/). Vous pouvez combiner la `Bold` et `Italic` membres avec le langage c# opérateur de bits OR.

## <a name="formatted-text"></a>Texte mis en forme

Dans tous les exemples jusqu'à présent, l’intégralité du texte affiché par le `Label` a été mis en forme uniformément. Pour faire varier la mise en forme dans une chaîne de texte, ne définissez pas le `Text` propriété du `Label`. Au lieu de cela, définissez la [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) propriété à un objet de type [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/).

`FormattedString` a un [ `Spans` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FormattedString.Spans/) propriété qui est une collection de [ `Span` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Span/) objets. Chaque `Span` objet possède son propre [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.Text/), [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontFamily/), [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontSize/), [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontAttributes/), [ `ForegroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.ForegroundColor/), et [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.BackgroundColor/) propriétés.

Le [ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) exemple montre comment utiliser le `FormattedText` propriété pour une ligne unique de texte, et [ **VariableFormattedParagraph** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) illustre la technique d’un paragraphe entier, comme illustré ici :

[![Capture d’écran de triple de la variable au format paragraphe](images/ch03fg06-small.png "Variable au format texte d’étiquette")](images/ch03fg06-large.png#lightbox "Variable au format texte d’étiquette")

Le [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) programme utilise une seule `Label` et un `FormattedString` objet pour afficher toutes les tailles de police nommées pour chaque plateforme.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 3 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Exemples de chapitre 3](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Exemples de chapitre 3 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Label](~/xamarin-forms/user-interface/text/label.md)
- [Utilisation des couleurs](~/xamarin-forms/user-interface/colors.md)
