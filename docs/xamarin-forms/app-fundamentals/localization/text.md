---
title: Chaîne et la localisation de l’Image
description: Les applications Xamarin.Forms peuvent être localisées à l’aide de fichiers de ressources .NET.
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: 09fe3587e4e435383822e50bd12616747b807f82
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108455"
---
# <a name="localization"></a>Localisation

_Les applications Xamarin.Forms peuvent être localisées à l’aide de fichiers de ressources .NET._

## <a name="overview"></a>Vue d'ensemble

Le mécanisme intégré pour la localisation utilise des applications .NET [fichiers RESX](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) et les classes dans le `System.Resources` et `System.Globalization` espaces de noms. Les fichiers RESX contenant les chaînes traduites sont incorporés dans l’assembly de Xamarin.Forms, ainsi qu’une classe générée par le compilateur qui fournit l’accès fortement typé pour les traductions. Le texte traduit peut ensuite être récupéré dans le code.

### <a name="sample-code"></a>Exemple de code

Il existe deux exemples associés à ce document :

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization) très simple démonstration des concepts étudiés. Les extraits de code ci-dessous sont tout à partir de cet exemple.
* [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized) est une application de travail de base qui utilise ces techniques de localisation.

#### <a name="shared-projects-are-not-recommended"></a>Projets partagés ne sont pas recommandés

L’exemple TodoLocalized inclut un [démonstration de projet partagé](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/) mais en raison des limitations du système de génération les fichiers de ressources n’obtiennent pas une **. designer.cs** fichier généré, ce qui élimine la possibilité d’accéder à chaînes traduites fortement typée dans le code.

Le reste de ce document est lié aux projets utilisant le modèle de bibliothèque Xamarin.Forms .NET Standard.

## <a name="globalizing-xamarinforms-code"></a>Le Code Xamarin.Forms de globalisation

**Globalisation** une application est le processus de fabrication de « world ready ». Cela signifie que d’écrire du code qui est capable d’afficher différentes langues.

Un des éléments clés de globalisation d’une application crée l’interface utilisateur afin qu’il y ait aucune *codées en dur* texte. Au lieu de cela, quoi que ce soit affiché à l’utilisateur doit être récupéré à partir d’un ensemble de chaînes qui ont été traduits dans leur langue.

Dans ce document, nous allons examiner comment utiliser les fichiers RESX pour stocker ces chaînes et les récupérer pour l’affichage en fonction des préférences de l’utilisateur.

Les exemples de ciblent les langues anglais, Français, espagnol, allemand, chinois, japonais, russe et portugais (Brésil). Les applications peuvent être traduites dans les langues aussi peu ou autant que nécessaire.

> [!NOTE]
> Sur la plateforme Windows universelle, les fichiers RESW doivent être utilisés pour la localisation de notification push, plutôt que les fichiers RESX. Pour plus d’informations, consultez [UWP localisation](/windows/uwp/design/globalizing/globalizing-portal/).

### <a name="adding-resources"></a>Ajout de ressources

La première étape de globalisation d’une application de bibliothèque Xamarin.Forms .NET Standard ajoute les fichiers de ressource RESX qui servira à stocker tout le texte utilisé dans l’application. Nous devons ajouter un fichier RESX qui contient le texte par défaut et ajoutez les fichiers RESX supplémentaires pour chaque langue que nous souhaitons pour prendre en charge.

#### <a name="base-language-resource"></a>Ressource de langue de base

Le fichier de ressources de base (RESX) contient les chaînes de langue par défaut (les exemples supposent qu’anglais est la langue par défaut). Ajoutez le fichier au projet de code commun Xamarin.Forms en effectuant un clic droit sur le projet et en choisissant **Ajouter > nouveau fichier...** .

Choisissez un nom explicite tel que **AppResources** et appuyez sur **OK**.

[![Ajouter le fichier de ressources](text-images/resx-new-file-sml.png "boîte de dialogue Nouveau fichier")](text-images/resx-new-file.png#lightbox "boîte de dialogue Nouveau fichier")

Deux fichiers sont ajoutés au projet :

* **AppResources.resx** fichier où les chaînes traduisibles sont stockées dans un format XML.
* **AppResources.designer.cs** fichier qui déclare une classe partielle pour contenir des références à tous les éléments créés dans le fichier RESX XML.

L’arborescence de la solution affiche les fichiers liés. Le fichier RESX *doit* être modifiée pour ajouter de nouvelles chaînes traduisibles ; le **. designer.cs** fichier devrait *pas* modifiables.

![](text-images/appresources-tree.png "Fichier de AppResources.resx")

##### <a name="string-visibility"></a>Visibilité de la chaîne

Par défaut lorsque fortement typée des références à des chaînes sont générées, elles seront `internal` à l’assembly. Il s’agit, car l’outil de génération par défaut pour les fichiers RESX génère le **. designer.cs** de fichiers avec `internal` propriétés.

Sélectionnez le **AppResources.resx** de fichiers et afficher le **propriétés** pad pour voir où cet outil de génération est configurer. La capture d’écran ci-dessous montre le **un outil personnalisé : ResXFileCodeGenerator**.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-internal-sml.png "Fenêtre de propriétés pour AppResources.Resx")](text-images/vs-resx-internal.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "Panneau Propriétés pour AppResources.Resx")](text-images/xs-resx-internal.png#lightbox)

-----

Pour rendre les propriétés de chaîne fortement typée `public`, vous devez modifier manuellement la configuration de **un outil personnalisé : PublicResXFileCodeGenerator**, comme illustré dans la capture d’écran ci-dessous :


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](text-images/vs-resx-public-sml.png "Fenêtre de propriétés pour AppResources.Resx")](text-images/vs-resx-public.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

[![](text-images/xs-resx-internal-sml.png "Panneau Propriétés pour AppResources.Resx")](text-images/xs-resx-internal.png#lightbox)


[![](text-images/xs-resx-public-sml.png "Panneau Propriétés pour AppResources.Resx")](text-images/xs-resx-public.png#lightbox)

-----

Cette modification est facultative et est uniquement nécessaire si vous souhaitez référencer des chaînes localisées dans des assemblys différents (par exemple, si vous devez placer les fichiers RESX dans un autre assembly votre code). L’exemple de cette rubrique laisse les chaînes `internal` , car elles sont définies dans le même assembly de bibliothèque Xamarin.Forms .NET Standard où ils sont utilisés.

Vous devez uniquement définir l’outil personnalisé sur le fichier RESX base, comme indiqué ci-dessus. vous n’avez pas besoin de définir *n’importe quel* outil de build pour les fichiers RESX spécifiques au langage abordées dans les sections suivantes.

##### <a name="editing-the-resx-file"></a>Modification du fichier RESX

Malheureusement il n’existe aucun éditeur RESX intégré dans Visual Studio pour Mac. Ajout de nouvelles chaînes traduisibles nécessite l’ajout d’un nouveau code XML `data` élément pour chaque chaîne. Chaque `data` élément peut contenir les éléments suivants :

* `name` attribut (obligatoire) est la clé pour cette chaîne traduisible. Il doit être valide C# nom de la propriété - donc aucun espace ni caractère spéciaux n’est autorisés.
* `value` élément (obligatoire), qui est la chaîne réelle qui s’affiche dans l’application.
* `comment` élément (facultatif) peut contenir des instructions pour le traducteur qui explique comment cette chaîne est utilisée.
* `xml:space` attribut (facultatif) pour contrôler la façon dont l’espacement dans la chaîne est conservé.

Quelques exemples `data` éléments sont présentés ici :

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

Comme l’application est écrite, chaque élément de texte affiché à l’utilisateur doit être ajouté au fichier de ressources RESX base dans une nouvelle `data` élément. Il est recommandé d’inclure `comment`s autant que possible garantir une traduction de haute qualité.

> [!NOTE]
> Visual Studio (y compris l’édition Community gratuite) contient un éditeur RESX de base. Si vous avez accès à un ordinateur Windows, qui peut être un moyen pratique d’ajouter et modifier des chaînes dans les fichiers RESX.

#### <a name="language-specific-resources"></a>Ressources linguistiques

En règle générale, la traduction réelle des chaînes de texte par défaut ne sont pas se produire jusqu'à ce que les grands segments de l’application ont été écrits (auquel cas le fichier RESX par défaut contient un grand nombre de chaînes). Il est toujours judicieux d’ajouter les ressources linguistiques au début du cycle de développement, le remplissage éventuel avec traduit le texte afin de tester le code de localisation.

Un fichier RESX supplémentaire est ajouté pour chaque langue que nous souhaitons pour prendre en charge.
Fichiers de ressources linguistiques doivent suivre une convention d’affectation de noms spécifique : utiliser le même nom de fichier que les ressources de base de fichiers (par exemple). **AppResources**) suivie d’un point (.), puis le code de langue. Voici quelques exemples simples :

* **AppResources.fr.resx** -Français des traductions.
* **AppResources.es.resx** -traductions de l’espagnol.
* **AppResources.de.resx** -les traductions en langue allemande.
* **AppResources.ja.resx** -traductions de langue japonaise.
* **AppResources.zh-Hans.resx** - chinois (simplifié) les traductions.
* **AppResources.zh-Hant.resx** - chinois (traditionnel) les traductions.
* **AppResources.pt.resx** -portugais traductions.
* **AppResources.pt-BR.resx** -traductions de portugais (Brésil).

Le modèle général est d’utiliser des codes de langue à deux lettres, mais il existe quelques exemples (telles que le chinois) où un autre format est utilisé et autres exemples (par exemple, portugais (Brésil)) où un identificateur de paramètres régionaux de quatre caractères est requis.

Ces fichiers de ressources spécifiques à la langue *ne le faites pas* nécessitent un **. designer.cs** de classe partielle pour pouvoir être ajoutés en tant que fichiers XML standard, avec le **Action de génération : EmbeddedResource**définie. Cette capture d’écran montre une solution contenant les fichiers de ressources spécifiques à la langue :

![](text-images/appresources-langs.png "Fichiers de ressources linguistiques")

Comme une application est développée et le fichier RESX base comporte du texte, vous devez l’enverra au traducteurs qui convertit chaque `data` élément et retournent un fichier de ressources linguistiques (à l’aide de la convention d’affectation de noms indiquée) à inclure dans l’application. Certains exemples 'traduit' sont présentées ci-dessous :

**AppResources.es.resx (espagnol)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>Escribir un artículo</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.ja.resx (japonais)**

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

Uniquement les `value` élément doit être mis à jour par le traducteur - le `comment` n’est pas destinée à être traduits. N’oubliez pas : lors de la modification des fichiers XML pour échapper les caractères réservés tels que `<`, `>`, `&` avec `&lt;`, `&gt;`, et `&amp;` si elles apparaissent dans le `value` ou `comment`.

<a name="incode" />

### <a name="using-resources-in-code"></a>Utilisation des ressources dans le Code

Chaînes dans les fichiers de ressources RESX seront disponibles à utiliser dans votre code d’interface utilisateur à l’aide la `AppResources` classe. Le `name` affecté à chaque chaîne dans le RESX fichier devient une propriété sur cette classe qui peut être référencée dans le code Xamarin.Forms comme indiqué ci-dessous :

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

L’interface utilisateur sur iOS, Android et les convertisseurs de plateforme universelle Windows (UWP) en tant que vous pouvez l’imaginer, à la différence près qu’il est possible de traduire l’application en plusieurs langues, car le texte est chargé à partir d’une ressource plutôt que d’être codées en dur. Voici une capture d’écran montrant l’interface utilisateur sur chaque plateforme avant la traduction :

![](text-images/simple-example-english.png "Interfaces utilisateur multiplateformes avant la traduction")

### <a name="troubleshooting"></a>Résolution des problèmes

#### <a name="testing-a-specific-language"></a>Test d’une langue spécifique

Il peut être difficile de passer le simulateur ou un appareil aux différentes langues, en particulier au cours du développement lorsque vous souhaitez tester rapidement des cultures différentes.

Vous pouvez forcer une langue spécifique à charger en définissant le `Culture` comme indiqué dans cet extrait de code :

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

Avec cette approche, la définition de la culture directement sur le `AppResources` classe – peuvent également être utilisées pour implémenter un sélecteur de langue à l’intérieur de votre application (plutôt qu’à l’aide des paramètres régionaux de l’appareil).

#### <a name="loading-embedded-resources"></a>Chargement des ressources incorporées.

L’extrait de code suivant est utile lorsque vous tentez de déboguer les problèmes avec les ressources incorporées (par exemple, les fichiers RESX). Ajoutez ce code à votre application (au début de son cycle de vie) et il répertorie toutes les ressources incorporées dans l’assembly, montrant l’identificateur de ressource complet :

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

Dans le **AppResources.Designer.cs** fichier (autour de *ligne 33*), la version complète *nom de gestionnaire de ressources* est spécifié (par exemple). `"UsingResxLocalization.Resx.AppResources"`) comme le code ci-dessous :

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

Vérifier le **sortie de l’Application** pour les résultats de la déboguer du code ci-dessus, pour confirmer les ressources adéquates sont répertoriés (p. ex. `"UsingResxLocalization.Resx.AppResources"`).

Si ce n’est pas le cas, la `AppResources` classe sera impossible de charger ses ressources.
Vérifiez la commande suivante pour résoudre les problèmes où les ressources est introuvable :

* L’espace de noms par défaut pour le projet correspond à l’espace de noms racine dans le **AppResources.Designer.cs** fichier.
* Si le **AppResources.resx** fichier se trouve dans un sous-répertoire, le nom du sous-répertoire doit faire partie de l’espace de noms *et* dans le cadre de l’identificateur de ressource.
* Le **AppResources.resx** fichier a **Action de génération : EmbeddedResource**.
* Le **Options du projet > Code Source > stratégies de noms .NET > noms de ressources de style utilisez Visual Studio** est coché. Vous pouvez l’untick si vous préférez, toutefois les espaces de noms utilisé pour référencer vos ressources RESX devez mis à jour dans toute l’application.

#### <a name="doesnt-work-in-debug-mode-android-only"></a>Ne fonctionne pas en mode de débogage (Android uniquement)

Si vous utilisez les chaînes traduites dans vos builds Android version, mais ne pas pendant le débogage, avec le bouton droit sur le **projet Android** et sélectionnez **Options > Build > Build Android** et vérifiez que le **Déploiement d’assembly rapide** n’est pas cochée. Cette option provoque des problèmes avec le chargement des ressources et ne doit pas être utilisée si vous testez des applications localisées.

### <a name="displaying-the-correct-language"></a>Affichage de la langue appropriée

Jusqu'à présent, nous avons examiné comment écrire du code afin que les traductions peuvent être fournies, mais pas pour réellement les faire apparaître. Le code Xamarin.Forms peut tirer parti. Ressources de NET pour charger les traductions de la langue correcte, mais nous avons besoin interroger le système d’exploitation sur chaque plateforme pour déterminer le langage de l’utilisateur a sélectionné.

Étant donné que du code spécifique à la plateforme est nécessaire pour obtenir les préférences de langue de l’utilisateur, utilisez un [service de dépendance](~/xamarin-forms/app-fundamentals/dependency-service/index.md) pour exposer ces informations dans l’application Xamarin.Forms et de l’implémenter pour chaque plateforme.

Tout d’abord, définissez une interface pour exposer la culture par défaut, comme le code ci-dessous :

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

Ensuite, utilisez le [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) dans Xamarin.Forms `App` classe pour appeler l’interface et de définir notre culture de ressources RESX à la valeur correcte. Notez que nous n’avez pas besoin de définir manuellement cette valeur pour la plateforme Windows universelle, depuis l’infrastructure de ressources automatiquement reconnaît la langue sélectionnée sur ces plateformes.

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

La ressource `Culture` doit être définie lors de l’application charge tout d’abord, afin que les chaînes de langue sont utilisées. Vous pouvez éventuellement mettre à jour cette valeur en fonction des événements spécifiques à la plateforme qui peut être déclenché sur iOS ou Android si l’utilisateur met à jour leurs préférences linguistiques pendant l’exécution de l’application.

Les implémentations pour les `ILocalize` interface figurent dans le [Code spécifique à la plateforme](#Platform-specific_Code) section ci-dessous. Ces implémentations tirer parti de cette `PlatformCulture` classe d’assistance :

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

Le code pour détecter la langue à afficher doit être spécifique à la plateforme, car iOS, Android et UWP exposent ces informations de manière légèrement différente. Le code pour le `ILocalize` service de dépendance n’est fourni ci-dessous pour chaque plateforme, ainsi que des exigences supplémentaires spécifiques à la plateforme pour garantir le texte localisé s’affiche correctement.

Le code spécifique à la plateforme doit également gérer les cas où le système d’exploitation permet à l’utilisateur configurer un identificateur de paramètres régionaux qui ne prend pas en charge. NET `CultureInfo` classe. Dans ces cas, un code personnalisé doit être écrite pour détecter les paramètres régionaux non pris en charge et remplacez le meilleur. Paramètres régionaux NET compatibles.

#### <a name="ios-application-project"></a>Projet d’application iOS

les utilisateurs iOS sélectionner leur langue préférée séparément à partir de la date et l’heure de mise en forme de la culture. Pour charger les ressources correctes pour localiser une application Xamarin.Forms, il nous faut interroger le `NSLocale.PreferredLanguages` tableau pour le premier élément.

L’implémentation suivante de la `ILocalize` service de dépendance doit être placé dans le projet d’application iOS. Étant donné qu’iOS utilise des traits de soulignement au lieu des tirets (qui est la représentation standard de .NET) le code remplace le trait de soulignement avant d’instancier le `CultureInfo` classe :

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
                case "ms-SG":    // "Malaysian (Singapore)" not supported .NET culture
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
> Le `try/catch` blocs dans le `GetCurrentCultureInfo` méthode imiter le comportement de secours généralement utilisé avec les spécificateurs de paramètres régionaux : si la correspondance exacte est introuvable, recherchez une correspondance étroite basé uniquement sur le langage (premier bloc de caractères dans les paramètres régionaux).
>
> Dans le cas de Xamarin.Forms, certains paramètres régionaux sont valides dans iOS, mais ne correspondent pas à valide `CultureInfo` dans .NET et le code ci-dessus tente de gérer cette situation.
>
> Par exemple, iOS **Paramètres > Général langage &amp; région** écran vous permet de définir votre téléphone **langage** à **anglais** mais le  **Région** à **Espagne**, ce qui aboutit à une chaîne de paramètres régionaux de `"en-ES"`. Lorsque le `CultureInfo` Échec de la création, le code utilise alors simplement les deux premières lettres pour sélectionner la langue d’affichage.
>
> Les développeurs doivent modifier le `iOSToDotnetLanguage` et `ToDotnetFallbackLanguage` méthodes pour gérer les cas spécifiques requis pour les langues prises en charge.


Il existe certains éléments d’interface utilisateur définie par le système qui sont automatiquement converties par iOS, tel que le **fait** bouton sur le `Picker` contrôle. Pour forcer iOS pour traduire ces éléments, nous devons indiquer les langues dans lesquelles nous prenons en charge dans les **Info.plist** fichier. Vous pouvez ajouter ces valeurs via **Info.plist > Source** comme indiqué ici :

![Clés de localisation dans Info.plist](text-images/info-plist.png "clés de localisation dans Info.plist")

Vous pouvez également ouvrir le **Info.plist** fichier dans un éditeur XML et modifier les valeurs directement :

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

Une fois que vous avez implémenté le service de dépendance et mis à jour **Info.plist**, l’application iOS sera en mesure d’afficher du texte localisé.

> [!NOTE]
> Notez que traite Apple portugais légèrement différemment que vous pouvez vous y attendre.
> À partir de [leurs docs](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2): _« utiliser pt en tant que l’ID de langue portugais qu’il est utilisé au Brésil et pt-PT en tant que l’ID de langue portugais car elle est utilisée au Portugal »_.
> Cela signifie que, lorsque portugais est choisi dans les paramètres régionaux non standard, la langue de secours sera portugais (Brésil) sur iOS, à moins que le code est écrit pour modifier ce comportement (tel que le `ToDotnetFallbackLanguage` ci-dessus).

Pour plus d’informations sur la localisation d’iOS, consultez [iOS localisation](~/ios/app-fundamentals/localization/index.md).

#### <a name="android-application-project"></a>Projet d’application Android

Android expose les paramètres régionaux actuellement sélectionnée par le biais de `Java.Util.Locale.Default`et utilise également un séparateur de trait de soulignement au lieu d’un tiret (qui est remplacé par le code suivant). Ajoutez cette implémentation de service de dépendance pour le projet d’application Android :

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
> Le `try/catch` blocs dans le `GetCurrentCultureInfo` méthode imiter le comportement de secours généralement utilisé avec les spécificateurs de paramètres régionaux : si la correspondance exacte est introuvable, recherchez une correspondance étroite basé uniquement sur le langage (premier bloc de caractères dans les paramètres régionaux).
>
> Dans le cas de Xamarin.Forms, certains paramètres régionaux sont valides dans Android, mais ne correspondent pas à valide `CultureInfo` dans .NET et le code ci-dessus tente de gérer cette situation.
>
> Les développeurs doivent modifier le `iOSToDotnetLanguage` et `ToDotnetFallbackLanguage` méthodes pour gérer les cas spécifiques requis pour les langues prises en charge.

Une fois ce code a été ajouté au projet d’application Android, il sera en mesure d’afficher automatiquement les chaînes traduites.

> [!NOTE]
>️ **Avertissement :** si vous utilisez les chaînes traduites dans vos builds Android version, mais ne pas pendant le débogage, avec le bouton droit sur le **projet Android** et sélectionnez **Options > Build > Android Build** et vérifiez que le **déploiement d’assembly rapide** n’est pas cochée. Cette option provoque des problèmes avec le chargement des ressources et ne doit pas être utilisée si vous testez des applications localisées.

Pour plus d’informations sur la localisation Android, consultez [localisation Android](~/android/app-fundamentals/localization.md).

#### <a name="universal-windows-platform"></a>Plateforme Windows universelle

Les projets Universal Windows Platform (UWP) ne nécessitent pas le service de dépendance. Au lieu de cela, cette plateforme définit automatiquement les culture de la ressource correctement.

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

Développez le nœud Propriétés dans le projet de bibliothèque .NET Standard et double-cliquez sur le **AssemblyInfo.cs** fichier. Ajoutez la ligne suivante au fichier pour définir le langage d’assembly de ressources neutres à l’anglais :

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

Cela informe le Gestionnaire de ressources de culture par défaut de l’application, par conséquent s’assurer que les chaînes définies dans le fichier RESX neutre de langue (**AppResources.resx**) s’affiche lorsque l’application est en cours d’exécution dans un les paramètres régionaux anglais.

### <a name="example"></a>Exemple

Après la mise à jour les projets spécifiques à la plateforme comme indiqué ci-dessus et recompiler l’application avec les fichiers RESX traduits, les traductions de mise à jour sera disponibles dans chaque application. Voici une capture d’écran à partir de l’exemple de code traduit en chinois simplifié :

![](text-images/simple-example-hans.png "Interfaces utilisateur multiplateformes traduit en chinois simplifié")

Pour plus d’informations sur la localisation UWP, consultez [UWP localisation](/windows/uwp/design/globalizing/globalizing-portal/).

## <a name="localizing-xaml"></a>Localisation de XAML

Lorsque la création d’une interface utilisateur de Xamarin.Forms dans XAML le balisage ressemblerait à ceci, avec des chaînes incorporées directement dans le code XML :

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

Dans l’idéal, nous aurions pu traduire des contrôles d’interface utilisateur directement dans le XAML, ce que nous pouvons faire en créant un *extension de balisage*. Vous trouverez ci-dessous le code pour une extension de balisage qui expose les ressources RESX au XAML. Cette classe doit être ajoutée au code Xamarin.Forms courants (ainsi que les pages XAML) :

```csharp
using System;
using System.Globalization;
using System.Reflection;
using System.Resources;
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace UsingResxLocalization
{
    // You exclude the 'Extension' suffix when using in XAML
    [ContentProperty("Text")]
    public class TranslateExtension : IMarkupExtension
    {
        readonly CultureInfo ci = null;
        const string ResourceId = "UsingResxLocalization.Resx.AppResources";

        static readonly Lazy<ResourceManager> ResMgr = new Lazy<ResourceManager>(
            () => new ResourceManager(ResourceId, IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly));

        public string Text { get; set; }

        public TranslateExtension()
        {
            if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
            {
                ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
            }
        }

        public object ProvideValue(IServiceProvider serviceProvider)
        {
            if (Text == null)
                return string.Empty;

            var translation = ResMgr.Value.GetString(Text, ci);
            if (translation == null)
            {
#if DEBUG
                throw new ArgumentException(
                    string.Format("Key '{0}' was not found in resources '{1}' for culture '{2}'.", Text, ResourceId, ci.Name),
                    "Text");
#else
                translation = Text; // HACK: returns the key, which GETS DISPLAYED TO THE USER
#endif
            }
            return translation;
        }
    }
}
```

Les puces ci-après expliquent les éléments importants dans le code ci-dessus :

* La classe est nommée `TranslateExtension`, mais par convention, nous pouvons appeler joue **Translate** dans notre balisage.
* La classe implémente `IMarkupExtension`, qui est requis par Xamarin.Forms pour qu’il fonctionne.
* `"UsingResxLocalization.Resx.AppResources"` est l’identificateur de ressource pour nos ressources RESX. Il se compose de notre espace de noms par défaut, le dossier dans lequel se trouvent les fichiers de ressources et le nom de fichier RESX par défaut.
* Le `ResourceManager` classe est créée à l’aide de `IntrospectionExtensions.GetTypeInfo(typeof(TranslateExtension)).Assembly)` pour déterminer l’assembly actuel pour charger les ressources à partir et mis en cache dans la méthode statique `ResMgr` champ. Il est créé comme un `Lazy` tapez afin que sa création est différée jusqu'à ce qu’il est tout d’abord utilisé dans le `ProvideValue` (méthode).
* `ci` utilise le service de dépendance pour obtenir le langage choisi de l’utilisateur à partir du système d’exploitation natif.
* `GetString` est la méthode qui Récupère la chaîne traduite réelle à partir des fichiers de ressources. Sur la plateforme Windows universelle, `ci` sera null, car le `ILocalize` interface n’est pas implémentée sur ces plateformes. Cela équivaut à appeler le `GetString` méthode avec uniquement le premier paramètre. Au lieu de cela, l’infrastructure de ressources reconnaît automatiquement les paramètres régionaux et récupère la chaîne traduite dans le fichier RESX approprié.
* Gestion des erreurs a été incluses pour aider à déboguer les ressources manquantes en levant une exception (dans `DEBUG` mode uniquement).

L’extrait de code XAML suivant montre comment utiliser l’extension de balisage. Il existe deux étapes pour qu’il fonctionne :

1. Déclarer personnalisé `xmlns:i18n` espace de noms dans le nœud racine. Le `namespace` et `assembly` doit correspondre exactement - dans cet exemple montre comment ils sont identiques, mais peut être différents dans votre projet, les paramètres de projet.
2. Utilisez `{Binding}` syntaxe d’attributs, qui contient le texte d’appeler normalement le `Translate` extension de balisage. La clé de ressource est le seul paramètre requis.

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

## <a name="localizing-platform-specific-elements"></a>Localisation d’éléments spécifiques à la plateforme

Bien que nous pouvons gérer la traduction de l’interface utilisateur dans le code Xamarin.Forms, il existe quelques éléments qui doivent être effectuées dans chaque projet spécifique à la plateforme. Cette section décrit comment localiser :

* Application Name
* Images

L’exemple de projet inclut une image localisée appelée **flag.png**, qui est référencé dans C# comme suit :

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

L’image de l’indicateur est également référencé dans le XAML comme ceci :

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

Toutes les plateformes résoudra automatiquement les références d’image comme celles-ci à des versions localisées des images, tant que les structures de projet expliqués ci-dessous sont implémentées.

### <a name="ios-application-project"></a>Projet d’application iOS

iOS utilise une norme d’affectation de noms appelée des projets de localisation ou **.lproj** répertoires destinés à contenir des ressources d’image et chaîne. Ces répertoires peuvent contenir des versions localisées des images utilisées dans l’application et également le **InfoPlist.strings** fichier qui peut être utilisé pour localiser le nom de l’application. Pour plus d’informations sur la localisation d’iOS, consultez [iOS localisation](~/ios/app-fundamentals/localization/index.md).

#### <a name="images"></a>Images

Cette capture d’écran montre l’exemple d’application iOS avec spécifiques au langage **.lproj** répertoires. Le répertoire espagnol appelé **es.lproj**, contient les versions localisées de l’image par défaut, ainsi que **flag.png**:

![](text-images/ios-resources.png "Répertoires de projet de localisation d’iOS")

Chaque répertoire de langues contient une copie de **flag.png**, localisée pour cette langue. Si aucune image n’est fourni, le système d’exploitation par défaut est l’image dans le répertoire de la langue par défaut. Pour une prise en charge complète de Retina, vous devez fournir **@2x** et **@3x** copies de chaque image.

#### <a name="app-name"></a>Nom de l’application

Le contenu de la **InfoPlist.strings** est une seule clé-valeur pour configurer le nom de l’application :

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

Lorsque l’application est exécutée, le nom de l’application et l’image sont tous deux localisés :

![](text-images/ios-imageicon.png "iOS texte de l’application échantillon et de localisation de l’Image")

### <a name="android-application-project"></a>Projet d’application Android

Android suit un schéma différent pour stocker les images localisées à l’aide de différents **drawable** et **chaînes** répertoires avec un suffixe de code de langue. Lorsqu’un code de paramètres régionaux de quatre lettres est requis (par exemple zh-TW ou pt-BR), notez qu’Android nécessite un autre **r** suivantes dash/précèdent les paramètres régionaux de code (par exemple). zh-rTW ou pt-rBR). Pour plus d’informations sur la localisation Android, consultez [localisation Android](~/android/app-fundamentals/localization.md).

#### <a name="images"></a>Images

Cette capture d’écran montre le Android exemple avec un certains localisé drawables et chaînes :

![](text-images/android-resources.png "Android localisé Drawables et les répertoires de chaîne")

Notez qu’Android n’utilise pas zh-Hans et zh-Hant codes simplifié et chinois traditionnel ; au lieu de cela, il prend uniquement en charge les codes de pays spécifiques zh-CN et zh-TW.

Pour prendre en charge les images de résolution différente pour les écrans à haute densité, créer des dossiers de langue supplémentaire avec `-*dpi` suffixes, tel que **drawables-es-mdpi**, **drawables-es-xdpi**, **drawables-es-xxdpi**, etc. Consultez [fournissant des ressources Android Alternative](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources) pour plus d’informations.

#### <a name="app-name"></a>Nom de l’application

Le contenu de la **strings.xml** est une seule clé-valeur pour configurer le nom de l’application :

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

Mise à jour le **MainActivity.cs** dans le projet d’application Android afin que le `Label` fait référence aux chaînes XML.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

L’application localise maintenant le nom de l’application et l’image. Voici une capture d’écran du résultat (en espagnol) :

![](text-images/android-imageicon.png "Texte de l’application exemple Android et de localisation de l’Image")

### <a name="universal-windows-platform-application-projects"></a>Projets d’Application plateforme Windows universelle

La plateforme Windows universelle possède une infrastructure de ressource qui simplifie la localisation des images et le nom de l’application. Pour plus d’informations sur la localisation UWP, consultez [UWP localisation](/windows/uwp/design/globalizing/globalizing-portal/).

#### <a name="images"></a>Images

Images peuvent être localisées en les plaçant dans un dossier spécifique à la ressource, comme illustré dans la capture d’écran suivante :

![](text-images/uwp-image-folder-structure.png "Structure de dossiers de localisation Image UWP")

Lors de l’exécution, l’infrastructure de ressource Windows sélectionnera l’image appropriée selon les paramètres régionaux de l’utilisateur.

## <a name="summary"></a>Récapitulatif

Les applications Xamarin.Forms peuvent être localisées à l’aide de fichiers RESX et classes de globalisation .NET. En dehors d’une petite quantité de code spécifique à la plateforme pour détecter la langue que l’utilisateur préfère, la plupart des efforts de localisation est centralisée dans le code commun.

En règle générale, les images sont traitées de manière spécifique à la plateforme pour tirer parti de la prise en charge de résolution multiples fournie dans iOS et Android.

## <a name="related-links"></a>Liens associés

- [Exemple de localisation RESX](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized exemple d’application](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [Localisation d’inter-plateformes](~/cross-platform/app-fundamentals/localization.md)
- [Localisation d’iOS](~/ios/app-fundamentals/localization/index.md)
- [Localisation Android](~/android/app-fundamentals/localization.md)
- [Localisation de UWP](/windows/uwp/design/globalizing/globalizing-portal/)
- [À l’aide de la classe CultureInfo (MSDN)](http://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [Recherche et utilisation de ressources pour une Culture spécifique (MSDN)](http://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)
