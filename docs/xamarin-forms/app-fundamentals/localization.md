---
title: Localisation
description: "Xamarin.Forms applications peuvent être localisées à l’aide de fichiers de ressources .NET."
ms.topic: article
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: ffde89558495c4b9ccb9ec41761b5fc7ca53db38
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="localization"></a>Localisation

_Xamarin.Forms applications peuvent être localisées à l’aide de fichiers de ressources .NET._

## <a name="overview"></a>Vue d'ensemble

Le mécanisme intégré pour la localisation utilise des applications .NET [fichiers RESX](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) et les classes dans le `System.Resources` et `System.Globalization` espaces de noms. Fichiers RESX contenant les chaînes traduites sont incorporés dans l’assembly Xamarin.Forms, ainsi que d’une classe générée par le compilateur qui fournit un accès fortement typé pour les traductions. Le texte de traduction peut ensuite être récupéré dans le code.

Ce document contient les sections suivantes :

**Globalisation de Code de Xamarin.Forms**

* L’ajout et à l’aide des ressources de chaîne dans une application de bibliothèque de classes portables Xamarin.Forms.
* Activation de la détection de langage dans chacune des applications natives.

**Localiser des éléments XAML**

* Localisation de XAML à l’aide un `IMarkupExtension`.
* L’activation de l’extension de balisage dans les applications natives.

**Localisation des éléments spécifiques à la plateforme**

* Localisation des images et le nom de l’application dans les applications natives.

### <a name="sample-code"></a>Exemple de code

Il existe deux exemples associés à ce document :

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization) démonstration très simple des concepts abordés. Les extraits de code ci-dessous sont tout à partir de cet exemple.
* [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized) est une application de travail de base qui utilise ces techniques de localisation.

#### <a name="shared-projects-are-not-recommended"></a>Les projets partagés ne sont pas recommandés

L’exemple TodoLocalized inclut un [démonstration du projet partagé](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/) mais en raison des limitations du système de génération les fichiers de ressources n’obtiennent pas une **. designer.cs** fichier généré, ce qui interrompt la possibilité d’accéder à traduit les chaînes [fortement typée dans le code](~/xamarin-forms/app-fundamentals/localization.md).

Le reste de ce document est lié à des projets à l’aide du modèle de bibliothèque de classes portables Xamarin.Forms.

## <a name="globalizing-xamarinforms-code"></a>Globalisation de Code de Xamarin.Forms

**Globalisation** une application est le processus de fabrication de « world ready ». Cela signifie que l’écriture de code qui est capable d’afficher les différentes langues.

Parmi les éléments clés de globalisation d’une application crée l’interface utilisateur afin qu’il y ait aucun *codé en dur* texte. Au lieu de cela, quoi que ce soit affichée à l’utilisateur doit être récupéré à partir d’un ensemble de chaînes qui ont été traduites dans leur langue.

Dans ce document, nous allons examiner comment utiliser des fichiers RESX pour stocker les chaînes et les récupérer pour l’affichage en fonction des préférences de l’utilisateur.

Les exemples de ciblent les langues anglais, Français, espagnol, allemand, chinois, japonais, russe et portugais (Brésil). Les applications peuvent être traduites dans des langues autant ou aussi peu en fonction des besoins.

### <a name="adding-resources"></a>Ajout de ressources

La première étape de globalisation d’une application de bibliothèque de classes portables Xamarin.Forms ajoute les fichiers de ressources RESX qui permet de stocker tout le texte utilisé dans l’application. Nous devons ajouter un fichier RESX qui contient le texte par défaut, puis ajoutez les fichiers RESX supplémentaires pour chaque langue que nous souhaitons pour prendre en charge.

#### <a name="base-language-resource"></a>Ressource de langue de base

Le fichier de ressources de base (RESX) contient les chaînes de langue par défaut (les exemples supposent que l’anglais est la langue par défaut). Ajoutez le fichier au projet de code commun Xamarin.Forms en cliquant sur le projet et en choisissant **Ajouter > nouveau fichier...** .

Choisissez un nom significatif tel que **AppResources** et appuyez sur **OK**.

[![Ajouter le fichier de ressources](localization-images/resx-new-file-sml.png "boîte de dialogue Nouveau fichier")](localization-images/resx-new-file.png#lightbox "nouvelle boîte de dialogue fichier")

Deux fichiers seront ajoutés au projet :

* **AppResources.resx** fichier où les chaînes traduisibles sont stockées au format XML.
* **AppResources.designer.cs** fichier qui déclare une classe partielle pour contenir des références à tous les éléments créés dans le fichier RESX XML.

L’arborescence de la solution affiche les fichiers liés. Le fichier RESX *doit* être modifié pour ajouter de nouvelles chaînes traduisibles ; le **. designer.cs** fichier devrait *pas* être modifié.

![](localization-images/appresources-tree.png "AppResources.resx File")

##### <a name="string-visibility"></a>Visibilité de la chaîne

Par défaut lorsque fortement typée de références à des chaînes sont générées, elles seront `internal` à l’assembly. C’est parce que l’outil de génération par défaut pour les fichiers RESX génère le **. designer.cs** de fichiers avec `internal` propriétés.

Sélectionnez le **AppResources.resx** de fichiers et d’afficher le **propriétés** configurer de remplissage pour voir où cet outil de génération. La capture d’écran ci-dessous montre le **un outil personnalisé : ResXFileCodeGenerator**.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](localization-images/vs-resx-internal-sml.png "Fenêtre des propriétés de AppResources.Resx")](localization-images/vs-resx-internal.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![](localization-images/xs-resx-internal-sml.png "Liste des propriétés AppResources.Resx")](localization-images/xs-resx-internal.png#lightbox)

-----

Pour rendre les propriétés de chaîne de fortement typée `public`, vous devez modifier manuellement la configuration de **un outil personnalisé : PublicResXFileCodeGenerator**, comme illustré dans la capture d’écran ci-dessous :


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](localization-images/vs-resx-public-sml.png "Fenêtre des propriétés de AppResources.Resx")](localization-images/vs-resx-public.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![](localization-images/xs-resx-internal-sml.png "Liste des propriétés AppResources.Resx")](localization-images/xs-resx-internal.png#lightbox)


[![](localization-images/xs-resx-public-sml.png "Liste des propriétés AppResources.Resx")](localization-images/xs-resx-public.png#lightbox)

-----

Cette modification est facultative et est uniquement nécessaire si vous souhaitez faire référence à des chaînes localisées dans des assemblys différents (par exemple, si vous placez les fichiers RESX dans un autre assembly à votre code). L’exemple de cette rubrique laisse les chaînes `internal` car ils sont définis dans le même assembly Xamarin.Forms PCL où ils sont utilisés.

Vous ne devez définir l’outil personnalisé sur le fichier RESX base, comme indiqué ci-dessus. vous n’avez pas besoin de définir *des* outil de génération sur les fichiers RESX spécifiques présentées dans les sections suivantes.

##### <a name="editing-the-resx-file"></a>Modification du fichier RESX

Malheureusement il n’existe aucun éditeur RESX intégré dans Visual Studio pour Mac. Ajout de nouvelles chaînes traduisibles nécessite l’ajout d’un nouveau code XML `data` élément pour chaque chaîne. Chaque `data` élément peut contenir les éléments suivants :

* `name` attributs (obligatoire) est la clé pour cette chaîne traduisible. Il doit être un nom de propriété c# valid - donc aucun espaces ou caractères spéciaux ne sont autorisés.
* `value` élément (obligatoire), qui est la chaîne réelle qui est affichée dans l’application.
* `comment` élément (facultatif) peut contenir des instructions pour le traducteur qui explique comment cette chaîne est utilisée.
* `xml:space` attribut (facultatif) pour contrôler la façon dont l’espacement dans la chaîne est conservé.

Parmi les exemples `data` éléments sont présentés ici :

```xml
<data name="NotesLabel" xml:space="preserve">
    <value>Notes:</value>
    <comment>label for input field</comment>
</data>
<data name="NotesPlaceholder" xml:space="preserve">
    <value>eg. buy milk</value>
    <comment>example input for notes field</comment>
</data>
<data name="AddButton" xml:space="preserve">
    <value>Add new item</value>
</data>
```

Comme l’application est écrite, chaque partie du texte affiché à l’utilisateur doit être ajouté au fichier de ressources RESX base dans une nouvelle `data` élément. Il est recommandé d’inclure `comment`s autant que possible pour garantir une traduction de haute qualité.

> [!NOTE]
> Visual Studio (y compris l’édition Community gratuite) contient un éditeur de base RESX. Si vous avez accès à un ordinateur Windows, qui peut être un moyen pratique pour ajouter et modifier des chaînes dans les fichiers RESX.

#### <a name="language-specific-resources"></a>Ressources linguistiques

En règle générale, la traduction réelle des chaînes de texte par défaut n’a pas lieu tant que les grands segments de l’application ont été écrits (auquel cas le fichier RESX par défaut contient un grand nombre de chaînes). Il est toujours judicieux d’ajouter les ressources linguistiques au début du cycle de développement, en remplissant avec traduit un texte permettant de tester le code de localisation.

Un fichier RESX supplémentaire est ajouté pour chaque langue que nous souhaitons pour prendre en charge.
Fichiers de ressources linguistiques doivent suivre la convention d’affectation de noms spécifique : utiliser le même nom que les ressources de base de fichiers (par exemple). **AppResources**) suivi d’un point (.), puis le code de langue. Voici quelques exemples simples :

* **AppResources.fr.resx** -Français des traductions.
* **AppResources.es.resx** -traductions de langue espagnole.
* **AppResources.de.resx** -les traductions en allemand.
* **AppResources.ja.resx** -traductions de langue japonaise.
* **AppResources.zh-Hans.resx** - chinois (simplifié) les traductions.
* **AppResources.zh-Hant.resx** - chinois (traditionnel) les traductions.
* **AppResources.pt.resx** -portugais traductions.
* **AppResources.pt-BR.resx** -les traductions portugais (Brésil).

Le modèle général est d’utiliser des codes de langue à deux lettres, mais il existe quelques exemples (telles que le chinois), où un format différent est utilisé et autres exemples (par exemple, portugais (Brésil)) où un identificateur de paramètres régionaux de quatre caractères est requis.

Ces fichiers de ressources spécifiques à une langue *pas* nécessitent un **. designer.cs** de classe partielle afin qu’ils peuvent être ajoutés en tant que les fichiers XML standard, avec le **Action de génération : EmbeddedResource**définie. Cette capture d’écran illustre une solution qui contient les fichiers de ressources linguistiques :

![](localization-images/appresources-langs.png "Fichiers de ressources linguistiques")

Quand une application est développée et le fichier RESX base comporte du texte, vous devez envoyez-le au traducteurs qui convertit chaque `data` élément et retourner un fichier de ressources linguistiques (à l’aide de la convention d’affectation de noms indiquée) à inclure dans l’application. Voici quelques exemples 'traduit' :

**AppResources.es.resx (Spanish)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>Escribir un artículo</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.ja.resx (Japanese)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>新しい項目を追加</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.pt-BR.resx (portugais)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>adicionar novo item</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

Uniquement les `value` élément doit être mis à jour par le traducteur - le `comment` n’est pas destinée à traduire. N’oubliez pas : lorsque vous modifiez des fichiers XML pour échapper les caractères réservés comme `<`, `>`, `&` avec `&lt;`, `&gt;`, et `&amp;` si elles apparaissent dans le `value` ou `comment`.

<a name="incode" />

### <a name="using-resources-in-code"></a>Utilisation des ressources dans le Code

Chaînes dans les fichiers de ressources RESX seront disponibles à utiliser dans votre code d’interface utilisateur à l’aide la `AppResources` classe. Le `name` assignées à chaque chaîne dans le RESX fichier devient une propriété dans la classe qui peut être référencée dans le code de Xamarin.Forms, comme indiqué ci-dessous :

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

L’interface utilisateur sur iOS, Android et les convertisseurs de plateformes Windows en tant que vous attendez, à la différence près qu’il est possible de convertir l’application dans plusieurs langues, car le texte est chargé à partir d’une ressource plutôt que codés en dur. Voici une capture d’écran montrant l’interface utilisateur sur chaque plateforme avant de traduction :

![](localization-images/simple-example-english.png "Interfaces utilisateur de cross-Platform avant la traduction")

### <a name="troubleshooting"></a>Résolution des problèmes

#### <a name="testing-a-specific-language"></a>Test d’une langue spécifique

Il peut être difficile de passer le simulateur ou un appareil pour différentes langues, en particulier lors du développement lorsque vous souhaitez tester rapidement des cultures différentes.

Vous pouvez forcer une langue spécifique doit être chargée en définissant le `Culture` comme indiqué dans cet extrait de code :

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

Avec cette approche, la définition de la culture directement sur la `AppResources` classe – peut également être utilisé pour implémenter un sélecteur de langue à l’intérieur de votre application (plutôt qu’à l’aide des paramètres régionaux de l’appareil).

#### <a name="loading-embedded-resources"></a>Chargement des ressources incorporées.

L’extrait de code suivant est utile lorsque vous tentez de déboguer les problèmes avec les ressources incorporées (par exemple, les fichiers RESX). Ajoutez ce code à votre application (tôt dans le cycle de vie d’application), et il répertorie toutes les ressources incorporées dans l’assembly, en indiquant l’identificateur de ressource complet :

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly; // "EmbeddedImages" should be a class in your app
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

Dans le **AppResources.Designer.cs** fichier (autour de *ligne 33*), complet *nom de gestionnaire de ressources* est spécifié (par exemple). `"UsingResxLocalization.Resx.AppResources"`) semblable au code ci-dessous :

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

Vérifiez le **sortie de l’Application** pour les résultats du code de débogage indiqué ci-dessus, pour vérifier les ressources appropriées sont répertoriés (ie. `"UsingResxLocalization.Resx.AppResources"`).

Si ce n’est pas le cas, la `AppResources` classe sera impossible de charger ses ressources.
Vérifiez la commande suivante pour résoudre les problèmes où les ressources ne peuvent pas être détectées :

* L’espace de noms par défaut pour le projet correspond à l’espace de noms racine dans le **AppResources.Designer.cs** fichier.
* Si le **AppResources.resx** fichier se trouve dans un sous-répertoire, le nom du sous-répertoire doit faire partie de l’espace de noms *et* dans le cadre de l’identificateur de ressource.
* Le **AppResources.resx** fichier a **Action de génération : EmbeddedResource**.
* Le **Options du projet > Code Source > .NET Naming Policies > noms de ressources de type utilisez Visual Studio** est cochée. Vous pouvez l’untick si vous préférez, toutefois les espaces de noms utilisé lors du référencement de vos ressources RESX devez mis à jour dans toute l’application.

#### <a name="doesnt-work-in-debug-mode-android-only"></a>Ne fonctionne pas en mode de débogage (Android uniquement)

Si vous utilisez les chaînes traduites dans vos builds Android version mais ne pas pendant le débogage, avec le bouton droit sur le **projet Android** et sélectionnez **Options > Générer > Générer Android** et vérifiez que le **Rapide du déploiement de l’assembly** n’est pas cochée. Cette option provoque des problèmes avec le chargement des ressources et ne doit pas être utilisée si vous testez des applications localisées.

### <a name="displaying-the-correct-language"></a>Affichage de la langue correcte

Jusqu'à présent, nous avons examiné comment écrire du code des traductions peuvent être fournies, mais pas pour réellement les faire apparaître. Code de Xamarin.Forms peut tirer parti de. Ressources du réseau pour charger les traductions de langue correcte, mais nous avons besoin interroger le système d’exploitation sur chaque plateforme pour déterminer le langage de l’utilisateur a sélectionné.

Étant donné que du code spécifique à la plateforme est nécessaire pour obtenir les préférences de langue de l’utilisateur, utilisez un [service de dépendance](~/xamarin-forms/app-fundamentals/dependency-service/index.md) à exposer ces informations dans l’application Xamarin.Forms et l’implémenter pour chaque plateforme.

Tout d’abord définir une interface pour exposer la culture par défaut, semblable au code ci-dessous :

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

En second lieu, utilisez la [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) dans les Xamarin.Forms `App` classe pour appeler l’interface et définir notre culture de ressources RESX sur la valeur correcte. Notez que nous n’avez pas besoin de définir manuellement cette valeur pour Windows Phone et la plateforme Windows universelle, depuis l’infrastructure de ressources automatiquement reconnaît la langue sélectionnée sur ces plateformes.

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

La ressource `Culture` doit être définie lors de l’application du premier charge de sorte que les chaînes de langue sont utilisées. Vous pouvez éventuellement mettre à jour cette valeur en fonction des événements spécifiques à une plateforme qui peut être déclenché sur iOS ou Android si l’utilisateur met à jour leurs préférences de langue pendant l’exécution de l’application.

Les implémentations pour les `ILocalize` interface figurent dans le [Code spécifique à la plateforme](#Platform-specific_Code) section ci-dessous. Ces implémentations de tirer parti de cette `PlatformCulture` classe d’assistance :

```csharp
public class PlatformCulture
{
    public PlatformCulture (string platformCultureString)
    {
        if (String.IsNullOrEmpty(platformCultureString))
        {
            throw new ArgumentException("Expected culture identifier", "platformCultureString"); // in C# 6 use nameof(platformCultureString)
        }
        PlatformString = platformCultureString.Replace("_", "-"); // .NET expects dash, not underscore
        var dashIndex = PlatformString.IndexOf("-", StringComparison.Ordinal);
        if (dashIndex > 0)
        {
            var parts = PlatformString.Split('-');
            LanguageCode = parts[0];
            LocaleCode = parts[1];
        }
        else
        {
            LanguageCode = PlatformString;
            LocaleCode = "";
        }
    }
    public string PlatformString { get; private set; }
    public string LanguageCode { get; private set; }
    public string LocaleCode { get; private set; }
    public override string ToString()
    {
        return PlatformString;
    }
}
```

<a name="Platform-specific_Code" />

### <a name="platform-specific-code"></a>Code spécifique à la plateforme

Le code pour détecter la langue d’affichage doit être spécifique à la plateforme, car iOS, Android et les plateformes Windows exposent ces informations de manière légèrement différente. Le code pour le `ILocalize` service de dépendance est fourni ci-dessous pour chaque plateforme, ainsi que les exigences spécifiques à la plateforme supplémentaires pour garantir le texte localisé est rendu correctement.

Le code spécifique à la plateforme doit également gérer les cas où le système d’exploitation permet à l’utilisateur configurer un identificateur de paramètres régionaux qui ne prend pas en charge. De NET `CultureInfo` classe. Code personnalisé doit être écrit pour détecter les paramètres régionaux non pris en charge et de remplacer le meilleur dans ces cas. Paramètres régionaux de NET compatibles.

#### <a name="ios-application-project"></a>Projet d’application iOS

les utilisateurs iOS sélectionner leur langue préférée séparément à partir de la date et l’heure de mise en forme de la culture. Pour charger les ressources appropriées pour localiser une application de Xamarin.Forms nous suffit pour interroger le `NSLocale.PreferredLanguages` tableau pour le premier élément.

L’implémentation suivante de la `ILocalize` service de dépendance doit être placé dans le projet d’application iOS. Dans la mesure où les iOS utilise des traits de soulignement au lieu des tirets (qui est la représentation sous forme standard .NET) le code remplace le trait de soulignement avant d’instancier la `CultureInfo` classe :

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.iOS.Localize))]

namespace UsingResxLocalization.iOS
{
public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo ()
        {
            var netLanguage = "en";
            if (NSLocale.PreferredLanguages.Length > 0)
            {
                var pref = NSLocale.PreferredLanguages [0];
                netLanguage = iOSToDotnetLanguage(pref);
            }
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string iOSToDotnetLanguage(string iOSLanguage)
        {
            var netLanguage = iOSLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (iOSLanguage)
            {
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage (PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "pt":
                    netLanguage = "pt-PT"; // fallback to Portuguese (Portugal)
                    break;
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> Le `try/catch` blocs dans le `GetCurrentCultureInfo` méthode simulent le comportement de secours généralement utilisé avec les spécificateurs de paramètres régionaux : si la correspondance exacte est introuvable, recherchez une correspondance étroite basée uniquement sur la langue (premier bloc de caractères dans les paramètres régionaux).
>
> Dans le cas de Xamarin.Forms, certains paramètres régionaux sont valides dans iOS, mais ne correspondent pas à un élément valide `CultureInfo` dans .NET et le code ci-dessus tente de gérer cette situation.
>
> Par exemple, les e/s **Paramètres > Général langage &amp; région** écran vous permet de définir votre téléphone **langage** à **anglais** mais la  **Région** à **Espagne**, ce qui aboutit à une chaîne de paramètres régionaux de `"en-ES"`. Lorsque le `CultureInfo` Échec de la création, le code revienne à simplement les deux premières lettres pour sélectionner la langue d’affichage.
>
> Les développeurs doivent modifier le `iOSToDotnetLanguage` et `ToDotnetFallbackLanguage` méthodes pour gérer les cas spécifiques requises pour les langues prises en charge.


Il existe certains éléments d’interface utilisateur définis par le système qui sont automatiquement converties par iOS, telles que la **fait** bouton sur le `Picker` contrôle. Pour forcer iOS pour traduire ces éléments que nous avons besoin pour indiquer quelles langues que nous prenons en charge dans les **Info.plist** fichier. Vous pouvez ajouter ces valeurs via **Info.plist > Source** comme indiqué ici :

![Clés de localisation dans Info.plist](localization-images/info-plist.png "localisation des clés dans le fichier Info.plist.")

Vous pouvez également ouvrir le **Info.plist** dans un éditeur XML et de modifier directement les valeurs :

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>de</string>
    <string>es</string>
    <string>fr</string>
    <string>ja</string>
    <string>pt</string> <!-- Brazil -->
    <string>pt-PT</string> <!-- Portugal -->
    <string>ru</string>
    <string>zh-Hans</string>
    <string>zh-Hant</string>
</array>
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
```

Une fois que vous avez implémenté le service de dépendance et mise à jour **Info.plist**, l’application iOS sera en mesure d’afficher le texte localisé.

> [!NOTE]
> Notez que traite Apple portugais légèrement différemment que vous attendez.
> À partir de [leurs documents](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2): _« utiliser pt en tant que l’ID de langue portugais qu’il est utilisé au Brésil et pt-PT en tant que l’ID de langue portugais qu’il est utilisé dans Portugal »_.
> Cela signifie que lorsque portugais est choisi dans les paramètres régionaux non standard, la langue de secours sera portugais (Brésil) sur iOS, sauf si le code est écrit pour modifier ce comportement (tels que le `ToDotnetFallbackLanguage` ci-dessus).

#### <a name="android-application-project"></a>Projet d’application Android

Android expose le paramètres régionaux actuellement sélectionné via `Java.Util.Locale.Default`et utilise également un séparateur de trait de soulignement au lieu d’un tiret (qui est remplacé par le code suivant). Ajoutez cette implémentation de service de dépendance pour le projet d’application Android :

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.Android.Localize))]

namespace UsingResxLocalization.Android
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale(CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo()
        {
            var netLanguage = "en";
            var androidLocale = Java.Util.Locale.Default;
            netLanguage = AndroidToDotnetLanguage(androidLocale.ToString().Replace("_", "-"));
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string AndroidToDotnetLanguage(string androidLanguage)
        {
            var netLanguage = androidLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (androidLanguage)
            {
                case "ms-BN":   // "Malaysian (Brunei)" not supported .NET culture
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "in-ID":  // "Indonesian (Indonesia)" has different code in  .NET
                    netLanguage = "id-ID"; // correct code for .NET
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage(PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> Le `try/catch` blocs dans le `GetCurrentCultureInfo` méthode simulent le comportement de secours généralement utilisé avec les spécificateurs de paramètres régionaux : si la correspondance exacte est introuvable, recherchez une correspondance étroite basée uniquement sur la langue (premier bloc de caractères dans les paramètres régionaux).
>
> Dans le cas de Xamarin.Forms, certains paramètres régionaux sont valides dans Android, mais ne correspondent pas à un élément valide `CultureInfo` dans .NET et le code ci-dessus tente de gérer cette situation.
>
> Les développeurs doivent modifier le `iOSToDotnetLanguage` et `ToDotnetFallbackLanguage` méthodes pour gérer les cas spécifiques requises pour les langues prises en charge.


Une fois ce code a été ajouté au projet d’application Android, il sera en mesure d’afficher automatiquement les chaînes traduites.

> [!NOTE]
>️ **Avertissement :** si vous utilisez les chaînes traduites dans vos builds Android version mais ne pas pendant le débogage, avec le bouton droit sur le **projet Android** et sélectionnez **Options > Générer > Android Build** et vérifiez que le **rapide du déploiement de l’assembly** n’est pas cochée. Cette option provoque des problèmes avec le chargement des ressources et ne doit pas être utilisée si vous testez des applications localisées.

#### <a name="windows-application-projects"></a>Projets d’Application Windows

Projets Windows 8.1 et la plateforme Windows universelle (UWP) ne nécessitent pas le service de dépendance : ces plates-formes automatiquement correctement culture de la ressource.

Implémentation de l’extension de balisage XAML décrite plus loin dans ce document peut-être nécessiter la `ILocalize` implémentation ci-dessous pour Windows Phone.

##### <a name="windows-phone-80"></a>Windows Phone 8.0

Bien que ne pas utilisé dans le `App` de classe, voici l’implémentation de Windows Phone pour la `ILocalize` service de dépendance. Ajouter cette classe au projet d’application Windows Phone ; elle sera nécessaire si l’implémentation de l’extension de balisage XAML décrite plus loin :

```csharp
[assembly: Dependency(typeof(UsingResxLocalization.WinPhone.Localize))]

namespace UsingResxLocalization.WinPhone
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci) { }
        public System.Globalization.CultureInfo GetCurrentCultureInfo ()
        {
            return System.Threading.Thread.CurrentThread.CurrentUICulture;
        }
    }
}

```

Les projets Windows Phone 8.0 doivent être correctement configurés pour le texte localisé à afficher.
Langues prises en charge doivent être sélectionnées dans les Options de projet *et* le **WMAppManifest.xml** fichiers.
Si ces paramètres ne sont pas mis à jour les ressources RESX localisées ne seront pas chargés.

##### <a name="project-options"></a>Options du projet

Avec le bouton droit sur le projet Windows Phone et sélectionnez **propriétés**. Dans le **Application** onglet graduation la **prise en charge des Cultures** qui prend en charge de l’application :

[![](localization-images/winphone-projectproperties-sml.png "Propriétés - Cultures prises en charge de projet")](localization-images/winphone-projectproperties.png#lightbox "propriétés - Cultures prises en charge de projet")

##### <a name="wmappmanifestxml"></a>WMAppManifest.xml

Développez le nœud Propriétés dans le projet Windows Phone et double-cliquez sur le **WMAppManifest.xml** fichier. Cliquez sur le **empaquetage** onglet et cochez toutes les langues prises en charge par l’application.

[![](localization-images/winphone-wmappmanifest-sml.png "WMAppManifest.xml - les langues prises en charge")](localization-images/winphone-wmappmanifest.png#lightbox "WMAppManifest.xml - les langues prises en charge")

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

Développez le nœud Propriétés dans le projet de bibliothèque de classes Portable (PCL) et double-cliquez sur le **AssemblyInfo.cs** fichier. Ajoutez la ligne suivante au fichier pour définir le langage d’assembly de ressources neutres anglais :

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

Cela permet d’informer le Gestionnaire de ressources de culture par défaut de l’application, par conséquent, en s’assurant que les chaînes définies dans le fichier RESX neutre de langue (**AppResources.resx**) s’affiche lorsque l’application s’exécute dans un, les paramètres régionaux anglais.

### <a name="example"></a>Exemple

Après la mise à jour les projets spécifiques à une plateforme comme indiqué ci-dessus et recompiler l’application avec les fichiers RESX traduits, traductions mises à jour sera disponibles dans chaque application. Voici une capture d’écran à partir de l’exemple de code traduit en chinois simplifié :

![](localization-images/simple-example-hans.png "Interfaces utilisateur de cross-Platform traduit en chinois simplifié")

## <a name="localizing-xaml"></a>Localiser des éléments XAML

Lorsque la création d’une interface utilisateur de Xamarin.Forms dans XAML le balisage devrait être semblable à, avec des chaînes incorporées directement dans le code XML :

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

Dans l’idéal, nous avons traduire des contrôles d’interface utilisateur directement dans le XAML, nous pouvons faire en créant un *extension de balisage*. Vous trouverez ci-dessous le code pour une extension de balisage qui expose les ressources RESX en XAML. Cette classe doit être ajoutée au code commun Xamarin.Forms (ainsi que les pages XAML) :

```csharp
using System;
using System.Globalization;
using System.Reflection;
using System.Resources;
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace UsingResxLocalization
{
    // You exclude the 'Extension' suffix when using in Xaml markup
    [ContentProperty ("Text")]
    public class TranslateExtension : IMarkupExtension
    {
        readonly CultureInfo ci;
        const string ResourceId = "UsingResxLocalization.Resx.AppResources";

        private static readonly Lazy<ResourceManager> ResMgr = new Lazy<ResourceManager>(()=> new ResourceManager(ResourceId
                                                                                                                  , typeof(TranslateExtension).GetTypeInfo().Assembly));

        public TranslateExtension()
        {
            if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
            {
                ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
            }
        }

        public string Text { get; set; }

        public object ProvideValue (IServiceProvider serviceProvider)
        {
            if (Text == null)
                return "";

            var translation = ResMgr.Value.GetString(Text, ci);

            if (translation == null)
            {
                #if DEBUG
                throw new ArgumentException(
                    String.Format("Key '{0}' was not found in resources '{1}' for culture '{2}'.", Text, ResourceId, ci.Name),
                    "Text");
                #else
                translation = Text; // returns the key, which GETS DISPLAYED TO THE USER
                #endif
            }
            return translation;
        }
    }
}
```

Les puces suivantes expliquent les éléments importants dans le code ci-dessus :

* La classe est nommée `TranslateExtension`, mais par convention, nous pouvons faire joue **traduire** dans notre balisage.
* La classe implémente `IMarkupExtension`, ce qui est requis par Xamarin.Forms pour qu’il fonctionne.
* `"UsingResxLocalization.Resx.AppResources"` est l’identificateur de ressource pour nos ressources RESX. Il est composé de notre espace de noms par défaut, le dossier dans lequel se trouvent les fichiers de ressources et le nom du fichier RESX par défaut.
* Le `ResourceManager` classe est créée à l’aide de `typeof(TranslateExtension)` pour déterminer l’assembly actuel pour charger les ressources à partir de.
* `ci` utilise le service de dépendance pour obtenir le langage choisi de l’utilisateur du système d’exploitation natif.
* `GetString` est la méthode qui Récupère la chaîne traduite réelle à partir des fichiers de ressources. Sur Windows Phone 8.1 et la plateforme Windows universelle, `ci` est null, car le `ILocalize` interface n’est pas implémentée sur ces plateformes. Cela revient à appeler la `GetString` méthode avec uniquement le premier paramètre. Au lieu de cela, l’infrastructure de ressources reconnaît automatiquement les paramètres régionaux et récupère la chaîne traduite dans le fichier RESX approprié.
* Gestion des erreurs a été incluses pour vous aider à déboguer des ressources manquantes en levant une exception (dans `DEBUG` mode uniquement).

L’extrait de code XAML suivant montre comment utiliser l’extension de balisage. Il existe deux étapes pour qu’il fonctionne :

1. Déclarer personnalisé `xmlns:i18n` espace de noms dans le nœud racine. Le `namespace` et `assembly` doivent correspondre les paramètres du projet exactement - dans cet exemple montre comment ils sont identiques, mais peut être différents dans votre projet.
2. Utilisez `{Binding}` syntaxe sur les attributs qui serait normalement contiennent du texte pour appeler le `Translate` extension de balisage. La clé de ressource est le seul paramètre requis.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        x:Class="UsingResxLocalization.FirstPageXaml"
        xmlns:i18n="clr-namespace:UsingResxLocalization;assembly=UsingResxLocalization">
    <StackLayout Padding="0, 20, 0, 0">
        <Label Text="{i18n:Translate NotesLabel}" />
        <Entry Placeholder="{i18n:Translate NotesPlaceholder}" />
        <Button Text="{i18n:Translate AddButton}" />
    </StackLayout>
</ContentPage>
```

Ce qui suit la syntaxe plus détaillée est également valide pour l’extension de balisage :

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>Localisation des éléments spécifiques à la plateforme

Bien que nous pouvons gérer la traduction de l’interface utilisateur dans le code de Xamarin.Forms, il existe certains éléments doivent être effectuées dans chaque projet spécifique à la plateforme. Cette section décrit comment localiser :

* Application Name
* Images

L’exemple de projet inclut une image localisée appelée **flag.png**, qui est référencé dans c# comme suit :

```csharp
var flag = new Image();
switch (Device.RuntimePlatform)
{
    case Device.iOS:
    case Device.Android:
        flag.Source = ImageSource.FromFile("flag.png");
        break;
    case Device.UWP:
        flag.Source = ImageSource.FromFile("Assets/Images/flag.png");
        break;
}
```

L’image de l’indicateur est également référencé dans le code XAML comme suit :

```xaml
<Image>
  <Image.Source>
    <OnPlatform x:TypeArguments="ImageSource">
        <On Platform="iOS, Android" Value="flag.png" />
        <On Platform="UWP" Value="Assets/Images/flag.png" />
    </OnPlatform>
  </Image.Source>
</Image>
```

Toutes les plateformes résoudra automatiquement les références d’image ces versions localisées des images, tant que les structures de projet expliqués ci-dessous sont implémentées.

### <a name="ios-application-project"></a>Projet d’application iOS

iOS utilise une norme d’affectation de noms appelée projets de localisation ou **.lproj** répertoires destinés à contenir des ressources d’image et chaîne. Ces répertoires peuvent contenir des versions localisées d’images utilisé dans l’application, ainsi que les **InfoPlist.strings** fichier qui peut être utilisé pour localiser le nom de l’application.

#### <a name="images"></a>Images

Cette capture d’écran montre l’exemple d’application iOS avec spécifiques au langage **.lproj** répertoires. Le répertoire espagnol appelé **es.lproj**, contient les versions localisées de l’image par défaut, ainsi que **flag.png**:

![](localization-images/ios-resources.png "Répertoires de projet de localisation d’iOS")

Chaque répertoire contient une copie de **flag.png**localisée pour cette langue. Si aucune image n’est fourni, le système d’exploitation utilise par défaut l’image dans le répertoire de la langue par défaut. Pour la prise en charge de rétine complète, vous devez fournir  **@2x**  et  **@3x**  copies de chaque image.

#### <a name="app-name"></a>Nom de l’application

Le contenu de la **InfoPlist.strings** est une seule clé-valeur pour configurer le nom de l’application :

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

Lorsque l’application est exécutée, le nom de l’application et de l’image à la fois localisés :

![](localization-images/ios-imageicon.png "iOS application exemple de texte et de localisation des images")

### <a name="android-application-project"></a>Projet d’application Android

Android suit un schéma différent pour le stockage des images localisées à l’aide de différents **drawable** et **chaînes** répertoires avec un suffixe de code de langue. Lorsqu’un code de paramètres régionaux de quatre lettres est requis (par exemple, zh-TW ou pt-BR), notez que Android requiert **r** après le tiret/précédant le code des paramètres régionaux (par exemple). zh-rTW ou pt-rBR).

#### <a name="images"></a>Images

Cette capture d’écran montre le Android exemple avec un certains localisée drawables et les chaînes :

![](localization-images/android-resources.png "Android localisée Drawables et les répertoires de chaîne")

Notez que Android n’utilisant pas zh-Hans et zh-Hant codes simplifié et chinois traditionnel ; au lieu de cela, il prend uniquement en charge les codes de pays spécifiques zh-CN et zh-TW.

Pour prendre en charge les images de résolution différente pour les écrans haute densités, créer des dossiers de langue supplémentaire avec `-*dpi` suffixes, tel que **drawables-es-mdpi**, **drawables-es-xdpi**, **drawables-es-xxdpi**, etc. Consultez [fournissant des ressources Android Alternative](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources) pour plus d’informations.

#### <a name="app-name"></a>Nom de l’application

Le contenu de la **strings.xml** est une seule clé-valeur pour configurer le nom de l’application :

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

Mise à jour la **MainActivity.cs** dans le projet d’application Android afin que le `Label` fait référence à des chaînes XML.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

Maintenant, l’application localise le nom de l’application et l’image. Voici une capture d’écran du résultat (en espagnol) :

![](localization-images/android-imageicon.png "Texte de l’application exemple Android et la localisation des images")

### <a name="windows-phone-80-application-project"></a>Projet d’Application Windows Phone 8.0

Windows Phone n’a pas un moyen intégré simple de sélectionner une image localisée spécifique, ni pour la localisation du nom de l’application.

#### <a name="images"></a>Images

Pour contourner cette limitation de l’exemple fournit une suggestion comment vous pouvez implémenter localisés lors du chargement d’image à l’aide un [convertisseur personnalisé](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) pour la `Image` contrôle.

Le code de convertisseur personnalisé est présenté ci-dessous - si la source est un `FileImageSource` puis extrait le nom de fichier et génère un chemin d’accès à une image localisé à l’aide du `CurrentUICulture`. Certains langages imposent un traitement spécial permettant de secours fonctionnent comme prévu ; dans l’exemple, la valeur par défaut est d’utiliser uniquement le code de langue à deux lettres, sauf dans certains cas spéciaux :

```csharp
using System.IO;
using Xamarin.Forms;
using Xamarin.Forms.Platform.WinPhone;

[assembly: ExportRenderer(typeof(Image), typeof(UsingResxLocalization.WinPhone.LocalizedImageRenderer))]
namespace UsingResxLocalization.WinPhone
{
    public class LocalizedImageRenderer : ImageRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Image> e)
        {
            base.OnElementChanged(e);

            if (e.NewElement != null)
            {
                var s = e.NewElement.Source as FileImageSource;
                if (s != null)
                {
                    var fileName = s.File;
                    string ci = System.Threading.Thread.CurrentThread.CurrentUICulture.ToString();
                    // you might need some custom logic here to support particular cultures and fallbacks
                    if (ci == "pt-BR") {
                        // use the complete string 'as is'
                    } else if (ci == "zh-CN") {
                         // we could have named the image directories differently,
                         // but this keeps them consisent with RESX file naming
                        ci = "zh-Hans";
                    } else if (ci == "zh-TW" || ci == "zh-HK") {
                        ci = "zh-Hant";
                    } else {
                        // for all others, just use the two-character language code
                        ci = System.Threading.Thread.CurrentThread.CurrentUICulture.TwoLetterISOLanguageName;
                    }
                    e.NewElement.Source = Path.Combine("Assets/" + ci + "/" + fileName);
                }
            }
        }
    }
}
```

Ce code fonctionne avec les images localisées dans la structure du répertoire indiqué ci-dessous. Vous êtes invités à modifier le code pour répondre à vos besoins de localisation spécifique (par exemple, la gestion des paramètres régionaux plus spécifiques et entrant lorsque les images ne sont pas disponibles) :

![](localization-images/winphone-resources.png "WinPhone localisée la Structure de répertoire d’Images")

Désormais, Windows Phone localise l’image. Voici une capture d’écran du résultat (en espagnol et en chinois simplifié) :

![](localization-images/winphone-image-sml.png "Texte de l’application exemple WinPhone et la localisation des images")

#### <a name="app-name"></a>Nom de l’application

Reportez-vous à la documentation de Microsoft pour [localiser le titre d’application Windows Phone 8.0](http://msdn.microsoft.com/library/windows/apps/ff967550(v=vs.105).aspx).

### <a name="windows-phone-81-and-universal-windows-platform-application-projects"></a>Windows Phone 8.1 et Universal Windows Platform les projets d’Application

Windows Phone 8.1 et la plateforme Windows universelle toutes deux possèdent une infrastructure de ressource qui simplifie la localisation des images et le nom de l’application.

#### <a name="images"></a>Images

Images peuvent être localisées en les plaçant dans un dossier de ressource spécifique, comme illustré dans la capture d’écran suivante :

![](localization-images/uwp-image-folder-structure.png "WinPhone 8.1 et la Structure de dossiers de localisation Image UWP")

Lors de l’exécution, l’infrastructure de ressources Windows sélectionnera l’image appropriée, selon les paramètres régionaux de l’utilisateur.

#### <a name="app-name"></a>Nom de l’application

Reportez-vous à la documentation de Microsoft pour [applications du Windows Store de Windows 8.1 : localiser les informations qui décrivent votre application aux utilisateurs](https://msdn.microsoft.com/library/windows/apps/hh454044.aspx) et [du chargement des chaînes à partir du manifeste d’application](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323.aspx#loading_strings_from_the_app_manifest.).

## <a name="summary"></a>Récapitulatif

Xamarin.Forms applications peuvent être localisées à l’aide de fichiers RESX et classes de globalisation .NET. En dehors d’une petite quantité de code spécifique à la plate-forme pour détecter la langue par défaut de l’utilisateur, la majorité de l’effort de localisation est centralisé dans le code commun.

En règle générale, les images sont traitées de façon spécifique à la plateforme pour tirer parti de la prise en charge des résolution multiples fournie dans iOS et Android. Windows Phone requiert un code personnalisé pour localiser des images d’une manière cross-platform-nom convivial de la ; exemple de code a été fourni pour ajouter cette fonctionnalité.


## <a name="related-links"></a>Liens associés

- [Exemple de localisation RESX](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized exemple d’application](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [Localisation interplateforme](~/cross-platform/app-fundamentals/localization.md)
- [Localisation d’iOS](~/ios/app-fundamentals/localization/index.md)
- [Localisation Android](~/android/app-fundamentals/localization.md)
- [À l’aide de la classe CultureInfo (MSDN)](http://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [Recherche et utilisation des ressources pour une Culture spécifique (MSDN)](http://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
