---
title: Personnaliser l’apparence d’une liste
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: d1b6c663be5745455f332afc11c185869579fde3
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732903"
---
# <a name="customizing-a-listviews-appearance"></a>Personnaliser l’apparence d’une liste


## <a name="overview"></a>Vue d'ensemble

L’apparence d’un contrôle ListView est dictée par la disposition des lignes affichées. Pour modifier l’apparence d’un `ListView`, utilisez une disposition de ligne différentes.


## <a name="built-in-row-views"></a>Vues de ligne intégré

Il existe des douze vues intégrées qui peuvent être référencés à l’aide de **Android.Resource.Layout**:

- **TestListItem** &ndash; une seule ligne de texte avec mise en forme minimale.

- **SimpleListItem1** &ndash; une seule ligne de texte.

- **SimpleListItem2** &ndash; deux lignes de texte.

- **SimpleSelectableListItem** &ndash; une seule ligne de texte qui prend en charge la sélection multiple ou unique (ajoutée dans le niveau de l’API 11).

- **SimpleListItemActivated1** &ndash; similaire à SimpleListItem1, mais la couleur d’arrière-plan indique quand une ligne est sélectionnée (ajouté dans le niveau de l’API 11).

- **SimpleListItemActivated2** &ndash; similaire à SimpleListItem2, mais la couleur d’arrière-plan indique quand une ligne est sélectionnée (ajouté dans le niveau de l’API 11).

- **SimpleListItemChecked** &ndash; affiche les coches pour indiquer la sélection.

- **SimpleListItemMultipleChoice** &ndash; affiche les cases à cocher pour indiquer la sélection à choix multiple.

- **SimpleListItemSingleChoice** &ndash; affiche les cases pour indiquer la sélection s’excluent mutuellement.

- **TwoLineListItem** &ndash; deux lignes de texte.

- **ActivityListItem** &ndash; une seule ligne de texte avec une image.

- **SimpleExpandableListItem** &ndash; groupes de lignes par catégories et chaque groupe peuvent être développés ou réduits.

Chaque vue de ligne intégré possède un style intégré associé. Ces captures d’écran montrent comment chaque vue s’affiche :

[![Captures d’écran de TestListItem, SimpleSelectableListItem, SimpleListitem1 et SimpleListItem2](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[![Captures d’écran de SimpleListItemActivated1, SimpleListItemActivated2, SimpleListItemChecked et SimpleListItemMultipleChecked](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[![Captures d’écran de SimpleListItemSingleChoice, TwoLineListItem, ActivityListItem et SimpleExpandableListItem](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

Le **BuiltInViews/HomeScreenAdapter.cs** exemple de fichier (dans le **BuiltInViews** solution) contient le code pour produire les écrans d’élément de liste non extensible. La vue est définie le `GetView` méthode comme suit :

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

Propriétés de la vue peuvent être définies en référençant les identificateurs de contrôle standard `Text1`, `Text2` et `Icon` sous `Android.Resource.Id` (ne définissez pas les propriétés de la vue ne contient pas, ou une exception sera levée) :

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

Le **BuiltInExpandableViews/ExpandableScreenAdapter.cs** exemple de fichier (dans le **BuiltInViews** solution) contient le code pour produire l’écran SimpleExpandableListItem. L’affichage du groupe est définie dans le `GetGroupView` méthode comme suit :

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

La vue enfant est définie dans le `GetChildView` méthode comme suit :

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

Les propriétés de l’affichage du groupe et de la vue enfant peuvent ensuite être définies en faisant référence à la norme `Text1` et `Text2` contrôle des identificateurs comme indiqué ci-dessus. La capture d’écran SimpleExpandableListItem (illustré ci-dessus) fournit un exemple d’un groupe d’une ligne (SimpleExpandableListItem1) et une vue de deux lignes enfants (SimpleExpandableListItem2). Ou bien, l’affichage du groupe peut être configuré pour les deux lignes (SimpleExpandableListItem2) et la vue enfant peut être configurée pour une seule ligne (SimpleExpandableListItem1), ou les deux groupe vue et vue enfant peut avoir le même nombre de lignes. 



## <a name="accessories"></a>Accessoires

Lignes peuvent avoir des accessoires ajoutées à droite de la vue pour indiquer l’état de sélection :

- **SimpleListItemChecked** &ndash; crée une liste à sélection unique avec une vérification de l’indicateur.

- **SimpleListItemSingleChoice** &ndash; crée des listes de type de bouton radio où seul choix est possible.

- **SimpleListItemMultipleChoice** &ndash; crée des listes de type case à cocher où plusieurs choix est possibles.

Les accessoires susmentionnés sont illustrées dans les écrans suivants, dans leur ordre respectif :

[![Captures d’écran de SimpleListItemChecked, SimpleListItemSingleChoice et SimpleListItemMultipleChoice avec accessoires](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

Pour afficher l’un de ces passe accessoires l’ID de ressource de disposition requises pour l’adaptateur puis définie manuellement l’état de sélection pour les lignes requises. Cette ligne de code montre comment créer et affecter un `Adapter` en utilisant l’une de ces mises en page :

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

Le `ListView` lui-même prend en charge différents modes de sélection, quel que soit l’accessoire affiché. Pour éviter toute confusion, utilisez `Single` en mode de sélection avec `SingleChoice` accessoires et les `Checked` ou `Multiple` mode avec la `MultipleChoice` style. Le mode de sélection est contrôlé par le `ChoiceMode` propriété de la `ListView`.


### <a name="handling-api-level"></a>Niveau de la gestion des API

Les versions antérieures de Xamarin.Android implémentaient des énumérations en tant que propriétés de l’entier. La version la plus récente a introduit les types d’énumération .NET appropriés qui le rend beaucoup plus facile de découvrir les options possibles.

Selon le niveau de l’API que vous ciblez, `ChoiceMode` est un entier ou une énumération. L’exemple de fichier **AccessoryViews/HomeScreen.cs** a un bloc commenté si vous souhaitez cibler les API personnage en pain :

```csharp
// For targeting Gingerbread the ChoiceMode is an int, otherwise it is an
// enumeration.

lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // 1
//lv.ChoiceMode = Android.Widget.ChoiceMode.Multiple; // 2
//lv.ChoiceMode = Android.Widget.ChoiceMode.None; // 0

// Use this block if targeting Gingerbread or lower
/*
lv.ChoiceMode = 1; // Single
//lv.ChoiceMode = 0; // none
//lv.ChoiceMode = 2; // Multiple
//lv.ChoiceMode = 3; // MultipleModal
*/
```


### <a name="selecting-items-programmatically"></a>Sélection d’éléments par programmation

Définir manuellement les éléments qui sont « sélectionnés » s’effectue avec la `SetItemChecked` (méthode) (il peut être appelée plusieurs fois pour la sélection multiple) :

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

Le code doit également détecter des sélections uniques différemment des sélections multiples. Pour déterminer quelle ligne a été sélectionné dans `Single` l’utilisation du mode le `CheckedItemPosition` propriété entière :

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

Pour déterminer les lignes qui ont été sélectionnées dans `Multiple` mode, vous devez effectuer une boucle sur le `CheckedItemPositions` `SparseBooleanArray`. Un tableau creux étant un dictionnaire qui contient uniquement des entrées où la valeur a été modifiée, vous devez traverser l’intégralité du tableau recherchez `true` valeurs savoir ce qui a été sélectionné dans la liste comme illustré dans l’extrait de code suivant :

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```


## <a name="creating-custom-row-layouts"></a>Créer des dispositions de ligne personnalisée

Les vues de ligne intégré quatre sont très simples. Pour afficher des dispositions plus complexes (par exemple, une liste de messages électroniques ou les informations de contact tweet) une vue personnalisée est requise. Vues personnalisées sont généralement déclarés en tant que fichiers AXML le **ressources/disposition** directory et puis chargé à l’aide de leur Id par un adaptateur personnalisé de la ressource. La vue peut contenir n’importe quel nombre de classes d’affichage (par exemple, les éléments TextViews, ImageViews et d’autres contrôles) avec mise en page, les polices et couleurs personnalisées.

Cet exemple diffère des exemples précédents de plusieurs façons :

-  Hérite de `Activity` , et non `ListActivity` . Vous pouvez personnaliser les lignes pour toute `ListView` , mais d’autres contrôles peuvent également être inclus dans un `Activity` mise en page (par exemple, un titre, des boutons ou des autres éléments d’interface utilisateur). Cet exemple ajoute un en-tête ci-dessus le `ListView` pour illustrer.

-  Nécessite un fichier de disposition AXML de l’écran ; dans les exemples précédents le `ListActivity` ne nécessite pas un fichier de disposition. Ce AXML contient un `ListView` déclaration de contrôle.

-  Nécessite un fichier de mise en page AXML pour restituer chaque ligne. Ce fichier AXML contient les contrôles de texte et image avec les paramètres de couleur et de police personnalisée.

-  Utilise un fichier XML de sélecteur personnalisé facultatif pour définir l’apparence de la ligne lorsqu’il est sélectionné.

-  Le `Adapter` implémentation retourne une disposition personnalisée à partir de la `GetView` remplacer.

-  `ItemClick` doit être déclaré différemment (un gestionnaire d’événements est attaché à `ListView.ItemClick` au lieu d’une substitution de `OnListItemClick` dans `ListActivity`).


Ces modifications sont détaillées ci-dessous, en commençant par la création de l’activité et la vue de ligne personnalisée et puis portant sur les modifications apportées à l’adaptateur et l’activité afin de les afficher.


### <a name="adding-a-listview-to-an-activity-layout"></a>Ajout d’un contrôle ListView à une disposition d’activité

Étant donné que `HomeScreen` n’hérite plus de `ListActivity` qu’il n’a pas une vue par défaut, un fichier de AXML disposition doit être créé pour les afficher de l’écran d’accueil. Pour cet exemple, la vue aura un titre (à l’aide un `TextView`) et un `ListView` pour afficher des données. La mise en page est défini dans le **Resources/Layout/HomeScreen.axml** fichier qui est indiqué ici :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent">
    <TextView android:id="@+id/Heading"
        android:text="Vegetable Groups"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:background="#00000000"
        android:textSize="30dp"
        android:textColor="#FF267F00"
        android:textStyle="bold"
        android:padding="5dp"
    />
    <ListView android:id="@+id/List"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:cacheColorHint="#FFDAFF7F"
    />
</LinearLayout>
```

L’avantage d’utiliser un `Activity` avec une disposition personnalisée (au lieu d’un `ListActivity`) réside dans leur capacité à ajouter des contrôles supplémentaires à l’écran, telles que l’en-tête `TextView` dans cet exemple.


### <a name="creating-a-custom-row-layout"></a>Création d’une disposition de ligne personnalisée

Un autre fichier de disposition AXML est requise pour contenir la disposition personnalisée pour chaque ligne qui s’affiche dans la liste. Dans cet exemple la ligne aura un arrière-plan vert, la marron texte et l’image d’aligné à droite. Le balisage XML Android pour déclarer cette disposition est décrite dans **Resources/Layout/CustomView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout  xmlns:android="http://schemas.android.com/apk/res/android"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
   android:background="#FFDAFF7F"
   android:padding="8dp">
    <LinearLayout android:id="@+id/Text"
       android:orientation="vertical"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:paddingLeft="10dip">
        <TextView
         android:id="@+id/Text1"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textColor="#FF7F3300"
         android:textSize="20dip"
         android:textStyle="italic"
         />
        <TextView
         android:id="@+id/Text2"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textSize="14dip"
         android:textColor="#FF267F00"
         android:paddingLeft="100dip"
         />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout >
```

Lors d’une disposition de ligne personnalisée peut contenir de nombreux contrôles différents, le défilement de performances peut être affecté par des conceptions complexes et à l’aide de l’image (en particulier si elles doivent être chargées sur le réseau). Voir l’article de Google pour plus d’informations sur les problèmes de performances défilement.


### <a name="referencing-a-custom-row-view"></a>Faisant référence à une vue de ligne personnalisée

L’implémentation de l’exemple d’adaptateur personnalisé est dans `HomeScreenAdapter.cs`. La méthode clé est `GetView` où il charge le AXML personnalisé à l’aide de l’ID de ressource `Resource.Layout.CustomView`, puis définit les propriétés sur chacun des contrôles dans la vue avant de le renvoyer. La classe d’adaptateur complet est indiquée :

```csharp
public class HomeScreenAdapter : BaseAdapter<TableItem> {
   List<TableItem> items;
   Activity context;
   public HomeScreenAdapter(Activity context, List<TableItem> items)
       : base()
   {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
   {
       return position;
   }
   public override TableItem this[int position]
   {
       get { return items[position]; }
   }
   public override int Count
   {
       get { return items.Count; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       var item = items[position];
       View view = convertView;
       if (view == null) // no view to re-use, create new
           view = context.LayoutInflater.Inflate(Resource.Layout.CustomView, null);
       view.FindViewById<TextView>(Resource.Id.Text1).Text = item.Heading;
       view.FindViewById<TextView>(Resource.Id.Text2).Text = item.SubHeading;
       view.FindViewById<ImageView>(Resource.Id.Image).SetImageResource(item.ImageResourceId);
       return view;
   }
}
```


### <a name="referencing-the-custom-listview-in-the-activity"></a>Référencement de ListView personnalisé dans l’activité

Étant donné que la `HomeScreen` classe hérite désormais `Activity`, un `ListView` champ est déclaré dans la classe pour contenir une référence au contrôle déclaré dans le AXML :

```csharp
ListView listView;
```

La classe doit charger puis la disposition de l’activité personnalisée AXML à l’aide de la `SetContentView` (méthode). Il peut ensuite rechercher le `ListView` contrôle dans la disposition crée et assigne ensuite l’adaptateur et assigne l’événements Click. Le code de la méthode OnCreate est illustré ici :

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

Enfin le `ItemClick` gestionnaire doit être défini ; dans ce cas il affiche simplement un `Toast` message :

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

L’écran ressemble à ceci :

[![Capture d’écran de la CustomRowView résultant](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)



### <a name="customizing-the-row-selector-color"></a>Personnalisation de la couleur de sélecteur de ligne

Lorsqu’une ligne est affectée, il doit être surlignée pour les commentaires de l’utilisateur. Lorsqu’une vue personnalisée spécifie comme couleur d’arrière-plan en tant que **CustomView.axml** ne, il remplace également la mise en surbrillance de la sélection. Cette ligne de code dans **CustomView.axml** définit l’arrière-plan vert, mais il signifie également il n’existe aucun indicateur visuel lorsque la ligne est couvertes :

```xml
android:background="#FFDAFF7F"
```

Pour réactiver le comportement de mise en surbrillance et également pour personnaliser la couleur qui est utilisée, définissez l’attribut en arrière-plan à un sélecteur personnalisé à la place. Le sélecteur déclare la couleur d’arrière-plan par défaut ainsi que la couleur de surbrillance. Le fichier **Resources/Drawable/CustomSelector.xml** contient la déclaration suivante :

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
<item android:state_pressed="false"
  android:state_selected="false"
  android:drawable="@color/cellback" />
<item android:state_pressed="true" >
  <shape>
     <gradient
      android:startColor="#E77A26"
        android:endColor="#E77A26"
        android:angle="270" />
  </shape>
</item>
<item android:state_selected="true"
  android:state_pressed="false"
  android:drawable="@color/cellback" />
</selector>
```

Pour référencer le sélecteur personnalisé, modifiez l’attribut en arrière-plan dans **CustomView.axml** à :

```xml
android:background="@drawable/CustomSelector"
```

Une ligne sélectionnée et correspondants `Toast` message se présente comme suit :

[![Une ligne sélectionnée en orange, avec le message de Toast affichant le nom de la ligne sélectionnée](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)



### <a name="preventing-flickering-on-custom-layouts"></a>Empêche le scintillement dans les dispositions personnalisées

Android tente d’améliorer les performances de `ListView` de défilement en mettant en cache les informations de disposition. Si vous avez la durée pendant laquelle le défilement des listes de données, vous devez également définir la la `android:cacheColorHint` propriété sur le `ListView` déclaration de définition de AXML de l’activité (pour la même valeur de couleur que l’arrière-plan de votre disposition ligne personnalisée). Pour inclure cet indicateur peut provoquer 'scintillement' en tant que l’utilisateur fait défiler d’une liste avec les couleurs d’arrière-plan de ligne personnalisée.



## <a name="related-links"></a>Liens associés

- [BuiltInViews (exemple)](https://developer.xamarin.com/samples/BuiltInViews/)
- [AccessoryViews (exemple)](https://developer.xamarin.com/samples/AccessoryViews/)
- [CustomRowView (exemple)](https://developer.xamarin.com/samples/CustomRowView/)
