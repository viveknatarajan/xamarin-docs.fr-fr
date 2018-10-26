---
title: Spinner
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 90b4755cdb4b8248c2b731d070d720076d4dda40
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102995"
---
# <a name="spinner"></a>Spinner

[`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) est un widget qui présente une liste déroulante pour sélectionner des éléments. Ce guide explique comment créer une application simple qui affiche une liste de choix dans un compteur, suivi des modifications qui affichent les autres valeurs associées avec le choix sélectionné.

## <a name="basic-spinner"></a>Compteur de base

Dans la première partie de ce didacticiel, vous allez créer un widget de compteur simple qui affiche une liste de différentes planètes. Lorsqu’une planète est sélectionnée, un message toast affiche l’élément sélectionné :

[![Captures d’écran de l’exemple d’application de HelloSpinner](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png#lightbox)

Démarrer un nouveau projet nommé **HelloSpinner**.

Ouvrez **Resources/Layout/Main.axml** et insérez le code XML suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:padding="10dip"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dip"
        android:text="@string/planet_prompt"
    />
    <Spinner
        android:id="@+id/spinner"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:prompt="@string/planet_prompt"
    />
</LinearLayout>
```

Notez que le [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/)de `android:text` attribut et la [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)de `android:prompt` attribut les deux référence à la même ressource de chaîne. Ce texte se comporte comme un titre pour le widget. Lorsqu’il est appliqué à la [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/), le texte du titre s’affiche dans la boîte de dialogue de sélection qui s’affiche lors de la sélection du widget.

Modifier **Resources/Values/Strings.xml** et modifiez le fichier ressemble à ceci :

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloSpinner</string>
  <string name="planet_prompt">Choose a planet</string>
  <string-array name="planets_array">
    <item>Mercury</item>
    <item>Venus</item>
    <item>Earth</item>
    <item>Mars</item>
    <item>Jupiter</item>
    <item>Saturn</item>
    <item>Uranus</item>
    <item>Neptune</item>
  </string-array>
</resources>
```

La seconde `<string>` élément définit la chaîne de titre référencée par le [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) et [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) dans la disposition ci-dessus.
Le `<string-array>` élément définit la liste de chaînes qui s’affichera sous forme de la liste dans le [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) widget.

Ouvrez maintenant **MainActivity.cs** et ajoutez le code suivant `using` instruction :

```csharp
using System;
```

Ensuite, insérez le code suivant pour le [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))
méthode :

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    Spinner spinner = FindViewById<Spinner> (Resource.Id.spinner);

    spinner.ItemSelected += new EventHandler<AdapterView.ItemSelectedEventArgs> (spinner_ItemSelected);
    var adapter = ArrayAdapter.CreateFromResource (
            this, Resource.Array.planets_array, Android.Resource.Layout.SimpleSpinnerItem);

    adapter.SetDropDownViewResource (Android.Resource.Layout.SimpleSpinnerDropDownItem);
    spinner.Adapter = adapter;
}
```

Après le `Main.axml` disposition est définie comme l’affichage du contenu, le [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) widget est capturé à partir de la mise en page avec [ `FindViewById<>(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/).
Le [`CreateFromResource()`](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.CreateFromResource/p/Android.Content.Context/System.Int32/System.Int32/)
méthode crée ensuite un nouveau [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/), qui lie chaque élément du tableau de chaînes à l’aspect initial pour le [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) (c'est-à-dire comment chaque élément s’affichera dans le compteur lorsque sélectionné) . Le `Resource.Array.planets_array` références d’ID. le `string-array` définie ci-dessus et le `Android.Resource.Layout.SimpleSpinnerItem` ID fait référence à une disposition pour l’apparence de compteur standard, définie par la plateforme.
[`SetDropDownViewResource`](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.SetDropDownViewResource/p/System.Int32/)
est appelée pour définir l’apparence de chaque élément lorsque le widget est ouvert. Enfin, le [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) a la valeur à associer à tous ses éléments avec le [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) en définissant le [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter) propriété.

Désormais fournir une méthode de rappel que le nombre de NOTIFY l’application lorsqu’un élément a été sélectionné à partir de la [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/). Voici à quoi cette méthode doit ressembler :

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

Lorsqu’un élément est sélectionné, l’expéditeur est casté en un [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) afin que les éléments sont accessibles. À l’aide de la `Position` propriété sur le `ItemEventArgs`, vous pouvez rechercher le texte de l’objet sélectionné et l’utiliser pour afficher un [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/).

Exécuter l’application. Il doit ressembler à ceci :

[![Exemple de capture d’écran de compteur avec Mars sélectionnés en tant que la planète](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>À l’aide de paires clé/valeur de compteur

Il est souvent nécessaire d’utiliser `Spinner` pour afficher les valeurs de clés qui sont associés à un type de données utilisées par votre application. Étant donné que `Spinner` ne fonctionne pas directement avec les paires clé/valeur, vous devez stocker séparément de la paire clé/valeur, remplir la `Spinner` avec les valeurs de clé, puis utiliser la position de la clé sélectionnée dans la zone de sélection numérique pour rechercher la valeur de données associées. 

Dans les étapes suivantes, le **HelloSpinner** application est modifiée pour afficher la température moyenne de la planète sélectionnée :

Ajoutez le code suivant `using` instruction **MainActivity.cs**:

```csharp
using System.Collections.Generic;
```

Ajoutez la variable d’instance suivant à la `MainActivity` classe.
Cette liste contiendra des paires clé/valeur pour les planètes et les températures moyennes :

```csharp
private List<KeyValuePair<string, string>> planets;
```

Dans le `OnCreate` (méthode), ajoutez le code suivant avant `adapter` est déclarée :

```csharp
planets = new List<KeyValuePair<string, string>>
{
    new KeyValuePair<string, string>("Mercury", "167 degrees C"),
    new KeyValuePair<string, string>("Venus", "464 degrees C"),
    new KeyValuePair<string, string>("Earth", "15 degrees C"),
    new KeyValuePair<string, string>("Mars", "-65 degrees C"),
    new KeyValuePair<string, string>("Jupiter" , "-110 degrees C"),
    new KeyValuePair<string, string>("Saturn", "-140 degrees C"),
    new KeyValuePair<string, string>("Uranus", "-195 degrees C"),
    new KeyValuePair<string, string>("Neptune", "-200 degrees C")
};
```

Ce code crée un magasin simple planètes et les températures moyennes associés. (Dans une application réelle, une base de données est généralement utilisé pour stocker les clés et leurs données associées.)

Immédiatement après le code ci-dessus, ajoutez les lignes suivantes pour extraire les clés et les placer dans une liste (dans l’ordre) :

```csharp
List<string> planetNames = new List<string>();
foreach (var item in planets)
    planetNames.Add (item.Key);
```

Passer cette liste pour le `ArrayAdapter` constructeur (au lieu du `planets_array` ressources) :

```csharp
var adapter = new ArrayAdapter<string>(this,
    Android.Resource.Layout.SimpleSpinnerItem, planetNames);
```

Modifier `spinner_ItemSelected` afin que la position sélectionnée est utilisée pour rechercher la valeur (la température) associée à la planète sélectionnée :

```csharp
private void spinner_ItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format("The mean temperature for planet {0} is {1}",
        spinner.GetItemAtPosition(e.Position), planets[e.Position].Value);
    Toast.MakeText(this, toast, ToastLength.Long).Show();
}
```

Exécuter l’application. le toast doit ressembler à ceci :

[![Exemple de sélection de la planète affichant la température](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)
   
  

## <a name="resources"></a>Ressources

-   [`Resource.Layout`](https://developer.xamarin.com/api/type/Android.Resource+Layout/) 
-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) 
-   [`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) 

*Certaines parties de cette page sont des modifications basées sur le travail créé et partagé par Android Open Source Project et utilisé conformément aux conditions décrites dans le*
[*licence Creative Commons 2.5 Attribution* ](http://creativecommons.org/licenses/by/2.5/).
