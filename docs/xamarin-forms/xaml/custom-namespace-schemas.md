---
title: Schémas de Namespace personnalisé XAML dans Xamarin.Forms
description: Un schéma d’espace de noms personnalisé XAML peut être défini avec la classe XmlnsDefinitionAttribute, qui spécifie un mappage entre une URL personnalisée et un ou plusieurs espaces de noms CLR. Le schéma de l’espace de noms personnalisé peut ensuite être utilisé dans les déclarations d’espace de noms XAML.
ms.prod: xamarin
ms.assetid: FDF201A1-8C35-4569-A728-F9B0A0C5B31A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/21/2018
ms.openlocfilehash: 8167ff00d3e4d7167772f6f5a578da6197c0d72d
ms.sourcegitcommit: 93c9fe61eb2cdfa530960b4253eb85161894c882
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55832211"
---
# <a name="xaml-custom-namespace-schemas-in-xamarinforms"></a>Schémas de Namespace personnalisé XAML dans Xamarin.Forms

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/XAML/CustomNamespaceSchemas/)

Types dans une bibliothèque peuvent être référencées dans XAML en déclarant un espace de noms XAML pour la bibliothèque, avec la déclaration d’espace de noms en spécifiant le nom d’espace de noms Common Language Runtime (CLR) et un nom d’assembly :

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:MyCompany.Controls;assembly="MyCompany.Controls">
    ...
</ContentPage>
```

Toutefois, en spécifiant un nom d’espace de noms et assembly CLR dans un `xmlns` définition peut s’avérer difficile et sujet aux erreurs. En outre, plusieurs déclarations d’espace de noms XAML peuvent être nécessaire si la bibliothèque contient des types dans plusieurs espaces de noms.

Une autre approche consiste à définir un schéma de l’espace de noms personnalisé, tel que `http://mycompany.com/schemas/controls`, qui est mappée à un ou plusieurs espaces de noms CLR. Cela permet une déclaration d’espace de noms XAML unique référencer tous les types dans un assembly, même si elles se trouvent dans différents espaces de noms. Il permet également une déclaration d’espace de noms XAML unique vers des types de référence dans plusieurs assemblys.

Pour plus d’informations sur les espaces de noms XAML, consultez [espaces de noms XAML dans Xamarin.Forms](namespaces.md).

## <a name="defining-a-custom-namespace-schema"></a>Définition d’un schéma de l’espace de noms personnalisé

L’exemple d’application contienne une bibliothèque qui expose certains contrôles simples, comme `CircleButton`:

```csharp
using Xamarin.Forms;

namespace MyCompany.Controls
{
    public class CircleButton : Button
    {
        ...
    }
}
```

Tous les contrôles dans la bibliothèque se trouvent dans le `MyCompany.Controls` espace de noms. Ces contrôles peuvent être exposées à un assembly appelant via un schéma de l’espace de noms personnalisé.

Un schéma de l’espace de noms personnalisé est défini avec la `XmlnsDefinitionAttribute` (classe), qui spécifie le mappage entre un espace de noms XAML et un ou plusieurs espaces de noms CLR. Le `XmlnsDefinitionAttribute` accepte deux arguments : le nom d’espace de noms XAML et le nom d’espace de noms CLR. Le nom d’espace de noms XAML est stocké dans le `XmlnsDefinitionAttribute.XmlNamespace` propriété et le nom d’espace de noms CLR est stocké dans le `XmlnsDefinitionAttribute.ClrNamespace` propriété.

> [!NOTE]
> Le `XmlnsDefinitionAttribute` classe a également une propriété nommée `AssemblyName`, qui peut éventuellement être défini sur le nom de l’assembly. Cela est uniquement requis lorsqu’un espace de noms CLR référencé à partir d’un `XmlnsDefinitionAttribute` se trouve dans un assembly externe.

Le `XmlnsDefinitionAttribute` doivent être définies au niveau de l’assembly dans le projet qui contient les espaces de noms CLR qui sera mappé dans le schéma de l’espace de noms personnalisé. L’exemple suivant montre le **AssemblyInfo.cs** fichier à partir de l’exemple d’application :

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

[assembly: Preserve]
[assembly: XmlnsDefinition("http://mycompany.com/schemas/controls", "MyCompany.Controls")]
```

Ce code crée un schéma d’espace de noms personnalisé qui mappe le `http://mycompany.com/schemas/controls` URL vers le `MyCompany.Controls` espace de noms CLR. En outre, le `Preserve` attribut est spécifié sur l’assembly, pour garantir que l’éditeur de liens conserve tous les types dans l’assembly.

> [!IMPORTANT]
> Le `Preserve` attribut doit être appliqué aux classes dans l’assembly qui sont mappés par le biais du schéma de l’espace de noms personnalisé, ou appliqués à la totalité de l’assembly.

Le schéma de l’espace de noms personnalisé peut ensuite être utilisé pour la résolution de type dans les fichiers XAML.

## <a name="consuming-a-custom-namespace-schema"></a>Utilisation d’un schéma de l’espace de noms personnalisé

Pour utiliser des types à partir du schéma de l’espace de noms personnalisé, le compilateur XAML nécessite qu’il existe une référence de code à partir de l’assembly qui utilise les types, à l’assembly qui définit les types. Cela est possible en ajoutant une classe contenant un `Init` (méthode) à l’assembly qui définit les types qui seront consommées via XAML :

```csharp
namespace MyCompany.Controls
{
    public static class Controls
    {
        public static void Init()
        {
        }
    }
}
```

Le `Init` méthode peut ensuite être appelée à partir de l’assembly qui consomme des types à partir du schéma de l’espace de noms personnalisé :

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

namespace CustomNamespaceSchemaDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            Controls.Init();
            InitializeComponent();
        }
    }
}
```

> [!WARNING]
> Absence d’une telle référence de code entraîne le compilateur XAML l’impossibilité de localiser l’assembly contenant les types de schéma d’espace de noms personnalisé.

Pour consommer le `CircleButton` contrôle, un espace de noms XAML est déclaré, avec la déclaration d’espace de noms en spécifiant l’URL de schéma d’espace de noms personnalisé :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:controls="http://mycompany.com/schemas/controls"
             x:Class="CustomNamespaceSchemaDemo.MainPage">
    <StackLayout Margin="20,35,20,20">
        ...
        <controls:CircleButton Text="+"
                               BackgroundColor="Fuchsia"
                               BorderColor="Black"
                               CircleDiameter="100" />
        <controls:CircleButton Text="-"
                               BackgroundColor="Teal"
                               BorderColor="Silver"
                               CircleDiameter="70" />
        ...
    </StackLayout>
</ContentPage>
```

`CircleButton` instances peuvent ensuite être ajoutées à la [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) en les déclarant avec le `controls` préfixe d’espace de noms.

Pour rechercher l’espace de noms personnalisé les types de schéma, Xamarin.Forms recherche les assemblys référencés pour `XmlnsDefinitionAttribute` instances. Si le `xmlns` attribut pour un élément dans un fichier XAML correspond à la `XmlNamespace` valeur de propriété dans un `XmlnsDefinitionAttribute`, Xamarin.Forms tentera d’utiliser le `XmlnsDefinitionAttribute.ClrNamespace` valeur de propriété pour la résolution du type. Si la résolution de type échoue, Xamarin.Forms continue d’essayer de résolution de type en fonction de n’importe quel outil de correspondance supplémentaire `XmlnsDefinitionAttribute` instances.

Le résultat est que deux `CircleButton` instances sont affichées :

![Cercle boutons](custom-namespace-schemas-images/circle-buttons.png "entourer de boutons")

## <a name="related-links"></a>Liens connexes

- [Schémas de Namespace personnalisés (exemple)](https://developer.xamarin.com/samples/xamarin-forms/XAML/CustomNamespaceSchemas/)
- [Espaces de noms XAML dans Xamarin.Forms](namespaces.md)
