---
title: Remplissage d’une liste avec des données
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: 83b398faf4fd634b7d492d372524b5fdd00163d0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="populating-a-listview-with-data"></a>Remplissage d’une liste avec des données


## <a name="overview"></a>Vue d'ensemble

Pour ajouter des lignes à un `ListView` vous devez l’ajouter à votre disposition et à implémenter un `IListAdapter` avec des méthodes qui le `ListView` appels remplit elle-même. Android inclut intégré `ListActivity` et `ArrayAdapter` classes que vous pouvez utiliser sans définir de disposition personnalisée XML ou de code. Le `ListActivity` classe crée automatiquement un `ListView` et expose un `ListAdapter` propriété pour fournir les vues de la ligne à afficher via un adaptateur.

Les adaptateurs intégrés prennent un ID de ressource de vue en tant que paramètre utilisée pour chaque ligne. Vous pouvez utiliser des ressources intégrées telles que celles dans `Android.Resource.Layout` vous n’avez pas besoin d’écrire votre propre.


## <a name="using-listactivity-and-arrayadapterltstringgt"></a>À l’aide de ListActivity et ArrayAdapter&lt;chaîne&gt;

L’exemple **BasicTable/HomeScreen.cs** montre comment utiliser ces classes pour afficher un `ListView` dans uniquement quelques lignes de code :

```csharp
[Activity(Label = "BasicTable", MainLauncher = true, Icon = "@drawable/icon")]
public class HomeScreen : ListActivity {
   string[] items;
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       items = new string[] { "Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers" };
       ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItem1, items);
   }
   protected override void OnListItemClick(ListView l, View v, int position, long id)
}
```


### <a name="handling-row-clicks"></a>La gestion des lignes clique sur

Généralement un `ListView` permet également à l’utilisateur toucher une ligne pour effectuer une action (par exemple, chanson, appel d’un contact ou afficher un autre écran). Pour répondre à touche utilisateur doit être une méthode plus implémentée dans le `ListActivity` &ndash; `OnListItemClick` &ndash; comme suit :

[![Capture d’écran d’un SimpleListItem](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

À présent l’utilisateur peut se toucher une ligne et un `Toast` alerte s’affiche :

[![Capture d’écran de Toast qui s’affiche lorsqu’une ligne est affectée.](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)


## <a name="implementing-a-listadapter"></a>Implémentation d’un ListAdapter

`ArrayAdapter<string>` est très utile en raison de sa simplicité, mais elle extrêmement limitée. Toutefois, souvent, vous avez une collection d’entités commerciales, plutôt que simplement les chaînes que vous souhaitez lier.
Par exemple, si vos données se composent d’une collection de classes d’employé, vous aurez peut-être à la liste simplement afficher les noms de chaque employé. Pour personnaliser le comportement d’un `ListView` pour contrôler les données qui s’affiche vous devez implémenter une sous-classe de `BaseAdapter` remplaçant les quatre éléments suivants :

-   **Nombre de** &ndash; pour indiquer le contrôle le nombre de lignes dans les données.

-   **GetView** &ndash; pour retourner une vue pour chaque ligne, rempli avec des données.
    Cette méthode possède un paramètre pour le `ListView` pour passer d’une ligne existante et inutilisée pour une réutilisation.

-   **GetItemId** &ndash; renvoyer un identificateur de ligne (en général, la ligne number, bien qu’il peut être toute valeur long que vous le souhaitez).

-   **Cet [int]** indexeur &ndash; pour retourner les données associées à un numéro de ligne spécifique.

L’exemple de code dans **BasicTableAdapter/HomeScreenAdapter.cs** montre comment créer une sous-classe `BaseAdapter`:

```csharp
public class HomeScreenAdapter : BaseAdapter<string> {
   string[] items;
   Activity context;
   public HomeScreenAdapter(Activity context, string[] items) : base() {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
  {
       return position;
   }
   public override string this[int position] {  
       get { return items[position]; }
   }
   public override int Count {
       get { return items.Length; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       View view = convertView; // re-use an existing view, if one is available
      if (view == null) // otherwise create a new one
           view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
       view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
       return view;
   }
}
```


### <a name="using-a-custom-adapter"></a>À l’aide d’un adaptateur personnalisé

À l’aide de l’adaptateur personnalisé est similaire à la fonction intégrée `ArrayAdapter`, en passant un `context` et `string[]` des valeurs à afficher :

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

Étant donné que cet exemple utilise la même disposition de ligne (`SimpleListItem1`) l’application résultante sera identique à l’exemple précédent.


### <a name="row-view-re-use"></a>Réutilisation de vue lignes

Dans cet exemple, il existe seulement six éléments. Étant donné que l’écran peut s’ajuster à huit, aucune ligne réutiliser ne requis. Lors de l’affichage des centaines, voire des milliers de lignes, toutefois, il serait un gaspillage de la mémoire pour créer des centaines de milliers de `View` objets huit uniquement lors de l’adapter à l’écran à la fois. Pour éviter cette situation, lorsqu’une ligne disparaît de l’écran de que la vue est placée dans une file d’attente pour une réutilisation. Comme l’utilisateur fait défiler, le `ListView` appelle `GetView` pour demander de nouveaux affichages pour afficher &ndash; si disponible Il passe un affichage inutilisé dans le `convertView` paramètre. Si cette valeur est null, votre code doit créer une nouvelle instance de la vue, sinon vous pouvez redéfinir les propriétés de cet objet et les réutiliser.

Le `GetView` méthode doit suivre le modèle pour réutiliser les vues de ligne :

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

Implémentations d’adaptateur personnalisé doivent *toujours* réutiliser le `convertView` objet avant de créer de nouveaux affichages pour s’assurer qu’ils n’exécutent pas la mémoire lors de l’affichage de longues listes.

Certaines implémentations d’adaptateur (telles que la `CursorAdapter`) n’avez pas un `GetView` méthode, au lieu de cela, ils nécessitent deux méthodes `NewView` et `BindView` qui applique la réutilisation de ligne en séparant les responsabilités de `GetView` en deux méthodes. Il existe un `CursorAdapter` exemple plus loin dans le document.


## <a name="enabling-fast-scrolling"></a>L’activation de défilement rapide

Défilement rapide vous aide à l’utilisateur de faire défiler les longues listes en fournissant un handle supplémentaire qui agit comme une barre de défilement pour accéder directement à une partie de la liste. Cette capture d’écran montre la poignée de défilement rapide :

[![Capture d’écran de défilement rapide avec un descripteur de défilement](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

À l’origine de la poignée de défilement rapide à afficher est aussi simple que le paramètre de la `FastScrollEnabled` propriété `true`:

```csharp
ListView.FastScrollEnabled = true;
```


### <a name="adding-a-section-index"></a>Ajout d’un Index de Section

Un index de la section fournit des commentaires supplémentaires pour les utilisateurs lorsqu’ils sont fast-défiler une longue liste &ndash; il montre quels qu’ils se trouvent à 'section'. À l’index de section s’affichent la sous-classe de l’adaptateur doit implémenter la `ISectionIndexer` interface pour fournir le texte de l’index selon les lignes affichées :

[![Capture d’écran de H figurant au-dessus de section qui commence par H](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

Pour implémenter `ISectionIndexer` vous devez ajouter les trois méthodes à une carte :

-   **GetSections** &ndash; fournit la liste complète de la section titres d’index qui pourrait être affichées. Cette méthode requiert un tableau d’objets Java, par conséquent, le code doit créer un `Java.Lang.Object[]` à partir d’une collection .NET. Dans notre exemple, il retourne une liste des caractères initiales dans la liste en tant que `Java.Lang.String` .

-   **GetPositionForSection** &ndash; retourne la première position de ligne pour un index de section donnée.

-   **GetSectionForPosition** &ndash; retourne l’index de section à afficher pour une ligne donnée.


L’exemple `SectionIndex/HomeScreenAdapter.cs` fichier implémente ces méthodes, ainsi que du code supplémentaire dans le constructeur. Le constructeur crée l’index de la section par une boucle sur chaque ligne et extraire le premier caractère du titre de la (les éléments doivent être déjà triées pour cela).

```csharp
alphaIndex = new Dictionary<string, int>();
for (int i = 0; i < items.Length; i++) { // loop through items
   var key = items[i][0].ToString();
   if (!alphaIndex.ContainsKey(key))
       alphaIndex.Add(key, i); // add each 'new' letter to the index
}
sections = new string[alphaIndex.Keys.Count];
alphaIndex.Keys.CopyTo(sections, 0); // convert letters list to string[]

// Interface requires a Java.Lang.Object[], so we create one here
sectionsObjects = new Java.Lang.Object[sections.Length];
for (int i = 0; i < sections.Length; i++) {
   sectionsObjects[i] = new Java.Lang.String(sections[i]);
}
```

Avec les structures de données créés, le `ISectionIndexer` méthodes sont très simples :

```csharp
public Java.Lang.Object[] GetSections()
{
   return sectionsObjects;
}
public int GetPositionForSection(int section)
{
   return alphaIndexer[sections[section]];
}
public int GetSectionForPosition(int position)
{   // this method isn't called in this example, but code is provided for completeness
    int prevSection = 0;
    for (int i = 0; i < sections.Length; i++)
    {
        if (GetPositionForSection(i) > position)
        {
            break;
        }
        prevSection = i;
    }
    return prevSection;
}
```

Les titres d’index de section n’avez pas besoin de mappage 1:1 pour vos sections réelles. C’est pourquoi la `GetPositionForSection` méthode existe.
`GetPositionForSection` vous donne la possibilité de mapper les index sont dans votre liste d’index pour toutes les sections sont dans votre affichage de liste. Par exemple, vous avez peut-être un « z » dans votre index, mais vous ne disposez pas d’une section de la table pour chaque lettre, au lieu de mappage de « z » à 26, il peut mapper à 25 ou 24 ou quel que soit l’index de section « z » doit correspondre au.



## <a name="related-links"></a>Liens associés

- [BasicTableAndroid (exemple)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (exemple)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [FastScroll (sample)](https://developer.xamarin.com/samples/FastScroll/)
