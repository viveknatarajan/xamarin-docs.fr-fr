---
title: Principes de base de ressource Android
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: f6be1001e5d3455a94e677f1bb5dc52ca574b873
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764890"
---
# <a name="android-resource-basics"></a>Principes de base de ressource Android

Presque toutes les applications Android aura une sorte de ressources. au minimum, ils ont souvent les dispositions d’interface utilisateur sous la forme de fichiers XML. Lors de la création d’une application de Xamarin.Android, les ressources par défaut sont le programme d’installation par le modèle de projet Xamarin.Android :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Fichiers de ressources](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Fichiers de ressources](android-resource-basics-images/01-resource-files-xs.png)
 
-----

Les cinq fichiers qui composent les ressources par défaut ont été créés dans le dossier de ressources :

-  **Icon.png** &ndash; l’icône par défaut pour l’application

-  **Main.axml** &ndash; le fichier de mise en page de l’interface de l’utilisateur par défaut pour une application. Notez que lors de Android utilise le **.xml** Xamarin.Android de l’extension de fichier, utilise le **.axml** extension de fichier.

-  **Strings.XML** &ndash; une table de chaînes pour aider à la localisation de l’application

-  **AboutResources.txt** &ndash; cela n’est pas nécessaire et peut être supprimé en toute sécurité. Il fournit simplement une vue d’ensemble de haut niveau du dossier ressources et les fichiers qu’il contient.

-  **Resource.Designer.cs** &ndash; ce fichier est automatiquement généré et géré par Xamarin.Android et contient l’unique ID affecté à chaque ressource. Il s’agit très similaires ou identiques dans l’objet dans le fichier R.java ayant une application Android écrite en Java. Il est automatiquement créé par les outils de Xamarin.Android et va être régénérée de temps à autre.


## <a name="creating-and-accessing-resources"></a>Création et l’accès aux ressources

Création de ressources est aussi simple que l’ajout de fichiers dans le répertoire du type de ressource en question. La capture d’écran ci-dessous montre les ressources de chaîne pour les paramètres régionaux allemands ont été ajoutés à un projet. Lorsque **Strings.xml** a été ajouté au fichier, le **Action de génération** est automatiquement définie pour **AndroidResource** par les outils de Xamarin.Android :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Action pour Strings.xml AndroidResource la valeur de génération](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![Action pour Strings.xml AndroidResource la valeur de génération](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

Ainsi, les outils de Xamarin.Android compiler correctement et d’incorporer les ressources dans le fichier APK. Si pour une raison quelconque le **Action de génération** n’est pas définie **ressource Android**, puis les fichiers soient exclus APK et toute tentative de chargement ou d’accéder aux ressources se produit une erreur d’exécution et le application se bloque.

En outre, il est important de noter que pendant que Android prend uniquement en charge les noms de fichiers en minuscules pour les éléments de ressource, Xamarin.Android est un peu plus indulgent ; Il prend en charge les noms de caractères majuscules et minuscules. La convention de nom d’image consiste à utiliser des minuscules avec des traits de soulignement comme séparateurs (par exemple, **mon\_image\_name.png**). Notez que les noms de ressources ne peut pas être traitées si des tirets ou des espaces sont utilisés comme séparateurs.

Une fois que les ressources ont été ajoutés à un projet, il existe deux façons de les utiliser dans une application &ndash; par programmation (à l’intérieur de code) ou à partir de fichiers XML.


## <a name="referencing-resources-programmatically"></a>Référence à des ressources par programmation

Pour accéder par programme à ces fichiers, ils sont attribués à un ID de ressource unique. Cet ID de ressource est un entier défini dans une classe spéciale appelée `Resource`, qui se trouve dans le fichier **Resource.designer.cs**, et ressemble à ceci :

```csharp
public partial class Resource
{
    public partial class Attribute
    {
    }
    public partial class Drawable {
        public const int Icon=0x7f020000;
    }
    public partial class Id
    {
        public const int Textview=0x7f050000;
    }
    public partial class Layout
    {
        public const int Main=0x7f030000;
    }
    public partial class String
    {
        public const int App_Name=0x7f040001;
        public const int Hello=0x7f040000;
    }
}
```

Chaque ID de ressource est contenue dans une classe imbriquée qui correspond au type de ressource. Par exemple, lorsque le fichier **Icon.png** a été ajouté au projet, la mise à jour de Xamarin.Android le `Resource` (classe), création d’une classe imbriquée appelée `Drawable` avec une constante interne nommé `Icon`.
Cela permet au fichier **Icon.png** référencées dans le code comme `Resource.Drawable.Icon`. La `Resource` classe ne doit pas être modifié manuellement, comme toutes les modifications apportées à ce dernier seront remplacées par Xamarin.Android.

Lorsque vous référencez des ressources par programme (dans le code), ils sont accessibles via la hiérarchie de classes de ressources qui utilise la syntaxe suivante :

```xml
@[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **PackageName** &ndash; le package qui fournit la ressource et est uniquement requis lorsque les ressources à partir d’autres packages sont utilisées.

-  **ResourceType** &ndash; c’est le type de ressource imbriquée qui se trouve dans la classe de ressource décrite ci-dessus.

-  **Nom de la ressource** &ndash; il s’agit du nom de fichier de la ressource (sans l’extension) ou la valeur de l’attribut android : name pour les ressources qui sont dans un élément XML.


## <a name="referencing-resources-from-xml"></a>Référence à des ressources à partir de XML

Accès aux ressources dans un fichier XML par un qui suit une syntaxe spéciale :

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>.
```

-  **PackageName** &ndash; le package qui fournit la ressource et est uniquement requis lorsque les ressources à partir d’autres packages sont utilisées.

-  **ResourceType** &ndash; c’est le type de ressource imbriquée qui se trouve dans la classe de ressource.

-  **Nom de la ressource** &ndash; le nom de fichier de la ressource (*sans* l’extension de fichier) ou la valeur de la `android:name` attribut pour les ressources qui sont dans un élément XML.

Par exemple, le contenu d’un fichier de mise en page, **Main.axml**, sont les suivantes :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent">
    <ImageView android:id="@+id/myImage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/flag" />
</LinearLayout>
```

Cet exemple comprend une [ `ImageView` ](https://developer.xamarin.com/recipes/android/controls/imageview) qui nécessite une ressource drawable nommée **indicateur**. Le `ImageView` a son `src` attribut la valeur **@drawable/flag**. Lorsque l’activité démarre, Android ressemblera à l’intérieur du répertoire **ressource/Drawable** pour un fichier nommé **flag.png** (l’extension de fichier peut être un autre format d’image, comme **flag.jpg**) et de charger ce fichier et l’afficher dans le `ImageView`.
Lorsque cette application est exécutée, elle serait similaire à l’image suivante :

![ImageView localisée](android-resource-basics-images/03-localized-screenshot.png)

