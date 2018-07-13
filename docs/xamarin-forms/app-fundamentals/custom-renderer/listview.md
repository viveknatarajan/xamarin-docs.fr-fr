---
title: Personnalisation d’un ListView
description: Un ListView Xamarin.Forms est une vue qui affiche une collection de données sous la forme d’une liste verticale. Cet article montre comment créer un convertisseur personnalisé qui encapsule des contrôles de liste de spécifique à la plateforme et les dispositions de cellule native, ce qui permet de mieux contrôler les performances du contrôle de liste natif.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: b3b73d542faebdb8ab85c989d7812368f4f3ffac
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997482"
---
# <a name="customizing-a-listview"></a>Personnalisation d’un ListView

_Un ListView Xamarin.Forms est une vue qui affiche une collection de données sous la forme d’une liste verticale. Cet article montre comment créer un convertisseur personnalisé qui encapsule des contrôles de liste de spécifique à la plateforme et les dispositions de cellule native, ce qui permet de mieux contrôler les performances du contrôle de liste natif._

Chaque vue Xamarin.Forms a un convertisseur qui accompagne cet article pour chaque plateforme qui crée une instance d’un contrôle natif. Quand un [ `ListView` ](xref:Xamarin.Forms.ListView) est restitué par une application Xamarin.Forms, dans iOS le `ListViewRenderer` classe est instanciée, ce qui instancie à son tour native `UITableView` contrôle. Sur la plateforme Android, le `ListViewRenderer` classe instancie native `ListView` contrôle. Sur la plateforme de Windows universelle (UWP), le `ListViewRenderer` classe instancie native `ListView` contrôle. Pour plus d’informations sur les classes de contrôle natif correspondant aux contrôles Xamarin.Forms et le convertisseur, consultez [convertisseur des Classes de Base et des contrôles natifs](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Le diagramme suivant illustre la relation entre la [ `ListView` ](xref:Xamarin.Forms.ListView) contrôle et les contrôles natifs correspondants qui l’implémentent :

![](listview-images/listview-classes.png "Relation entre le contrôle ListView et l’implémentation des contrôles natifs")

Le processus de rendu peut être exploitée pour implémenter les personnalisations spécifiques à la plateforme en créant un convertisseur personnalisé pour un [ `ListView` ](xref:Xamarin.Forms.ListView) sur chaque plateforme. Le processus pour effectuer cette opération est la suivante :

1. [Créer](#Creating_the_Custom_ListView_Control) un contrôle personnalisé Xamarin.Forms.
1. [Consommer](#Consuming_the_Custom_Control) le contrôle personnalisé à partir de Xamarin.Forms.
1. [Créer](#Creating_the_Custom_Renderer_on_each_Platform) le convertisseur personnalisé pour le contrôle sur chaque plateforme.

Chaque élément maintenant nous reviendrons à son tour, pour implémenter un `NativeListView` convertisseur qui tire parti des contrôles de liste de spécifique à la plateforme et les dispositions de cellule natif. Ce scénario est utile lors du portage d’une application native existante qui contient la liste et le code de cellule peut être réutilisé. En outre, il autorise la personnalisation détaillée des fonctionnalités de contrôle de liste qui peut affecter les performances, telles que la virtualisation des données.

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>Création du contrôle ListView personnalisé

Personnalisé [ `ListView` ](xref:Xamarin.Forms.ListView) contrôle peut être créé en sous-classant la `ListView` classe, comme indiqué dans l’exemple de code suivant :

```csharp
public class NativeListView : ListView
{
  public static readonly BindableProperty ItemsProperty =
    BindableProperty.Create ("Items", typeof(IEnumerable<DataSource>), typeof(NativeListView), new List<DataSource> ());

  public IEnumerable<DataSource> Items {
    get { return (IEnumerable<DataSource>)GetValue (ItemsProperty); }
    set { SetValue (ItemsProperty, value); }
  }

  public event EventHandler<SelectedItemChangedEventArgs> ItemSelected;

  public void NotifyItemSelected (object item)
  {
    if (ItemSelected != null) {
      ItemSelected (this, new SelectedItemChangedEventArgs (item));
    }
  }
}
```

Le `NativeListView` est créé dans le projet de bibliothèque .NET Standard et définit l’API pour le contrôle personnalisé. Ce contrôle expose un `Items` propriété qui est utilisée pour remplir la `ListView` avec les données et qui peut être lié aux données de pour afficher à des fins. Elle expose également une `ItemSelected` événement est déclenché chaque fois qu’un élément est sélectionné dans un contrôle de liste natif spécifique à la plateforme. Pour plus d’informations sur la liaison de données, consultez [Notions de base de la liaison de données](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Utilisation du contrôle personnalisé

Le `NativeListView` contrôle personnalisé qui peut être référencé dans le Xaml dans le projet de bibliothèque .NET Standard en déclarant un espace de noms pour son emplacement et en utilisant le préfixe d’espace de noms sur le contrôle. Le code suivant montre l’exemple comment la `NativeListView` contrôle personnalisé peut être consommé par une page XAML :

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*" />
            </Grid.RowDefinitions>
          <Label Text="{x:Static local:App.Description}" HorizontalTextAlignment="Center" />
            <local:NativeListView Grid.Row="1" x:Name="nativeListView" ItemSelected="OnItemSelected" VerticalOptions="FillAndExpand" />
          </Grid>
      </ContentPage.Content>
</ContentPage>
```

Le `local` préfixe d’espace de noms peut être n’importe quel nom. Toutefois, le `clr-namespace` et `assembly` valeurs doivent correspondre les détails du contrôle personnalisé. Une fois que l’espace de noms est déclaré, le préfixe est utilisé pour référencer le contrôle personnalisé.

Le code suivant montre l’exemple comment la `NativeListView` contrôle personnalisé peut être consommé par une page c# :

```csharp
public class MainPageCS : ContentPage
{
    NativeListView nativeListView;

    public MainPageCS()
    {
        nativeListView = new NativeListView
        {
            Items = DataSource.GetList(),
            VerticalOptions = LayoutOptions.FillAndExpand
        };

        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
                Padding = new Thickness(0, 20, 0, 0);
                break;
            case Device.Android:
            case Device.UWP:
                Padding = new Thickness(0);
                break;
        }

        Content = new Grid
        {
            RowDefinitions = {
                new RowDefinition { Height = GridLength.Auto },
                new RowDefinition { Height = new GridLength (1, GridUnitType.Star) }
            },
            Children = {
                new Label { Text = App.Description, HorizontalTextAlignment = TextAlignment.Center },
                nativeListView
            }
        };
        nativeListView.ItemSelected += OnItemSelected;
    }
    ...
}
```

Le `NativeListView` contrôle personnalisé utilise des convertisseurs personnalisés spécifiques à la plateforme pour afficher une liste de données, qui sont remplies par le biais du `Items` propriété. Chaque ligne dans la liste contient trois éléments de données : un nom, une catégorie et un nom de fichier image. La disposition de chaque ligne dans la liste est définie par le convertisseur personnalisé spécifique à la plateforme.

> [!NOTE]
> Étant donné que le `NativeListView` contrôle personnalisé sera restitué à l’aide de contrôles de liste de spécifique à la plateforme qui incluent la capacité de défilement, le contrôle personnalisé ne doit pas être hébergé dans les contrôles de disposition à défilement telles que la [ `ScrollView` ](xref:Xamarin.Forms.ScrollView).

Un convertisseur personnalisé peut maintenant être ajouté à chaque projet d’application pour créer des contrôles de liste de spécifique à la plateforme et les dispositions de cellule natif.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Création du convertisseur personnalisé sur chaque plateforme

Le processus de création de la classe de convertisseur personnalisé est comme suit :

1. Créer une sous-classe de la `ListViewRenderer` classe qui restitue le contrôle personnalisé.
1. Remplacer le `OnElementChanged` méthode qui restitue le contrôle et écriture une logique personnalisée pour le personnaliser. Cette méthode est appelée lorsque le correspondantes Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) est créé.
1. Ajouter un `ExportRenderer` d’attribut à la classe de convertisseur personnalisé pour indiquer qu’il est utilisée pour restituer le contrôle personnalisé Xamarin.Forms. Cet attribut est utilisé pour inscrire le renderer personnalisé avec Xamarin.Forms.

> [!NOTE]
> Il est facultatif pour fournir un convertisseur personnalisé dans chaque projet de plateforme. Si un convertisseur personnalisé n’est pas inscrit, puis le convertisseur par défaut pour la classe de base de la cellule servira.

Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](listview-images/solution-structure.png "Responsabilités de projet de convertisseur NativeListView personnalisé")

Le `NativeListView` contrôle personnalisé est affiché par les classes de renderer spécifique à la plateforme, qui dérivent toutes de la `ListViewRenderer` classe pour chaque plateforme. Ainsi, chaque `NativeListView` contrôle personnalisé rendu avec les contrôles de liste de spécifique à la plateforme et les dispositions de cellule natif, comme indiqué dans les captures d’écran suivante :

![](listview-images/screenshots.png "NativeListView sur chaque plateforme")

Le `ListViewRenderer` classe expose le `OnElementChanged` (méthode), qui est appelé lorsque le contrôle personnalisé Xamarin.Forms est créé pour restituer le contrôle natif correspondant. Cette méthode prend un `ElementChangedEventArgs` paramètre, qui contient `OldElement` et `NewElement` propriétés. Ces propriétés représentent l’élément Xamarin.Forms qui le convertisseur *a été* associée et l’élément Xamarin.Forms qui le convertisseur *est* attaché, respectivement. Dans l’exemple d’application, le `OldElement` propriété sera `null` et `NewElement` propriété contiendra une référence à la `NativeListView` instance.

Une version substituée de la `OnElementChanged` (méthode), dans chaque classe de renderer spécifique à la plateforme, est l’endroit où effectuer la personnalisation du contrôle natif. Une référence typée au contrôle natif utilisé sur la plateforme sont accessibles via le `Control` propriété. En outre, une référence au contrôle Xamarin.Forms qui est rendu peut être obtenue via la `Element` propriété.

Être vigilant lors de l’abonnement aux gestionnaires d’événements dans le `OnElementChanged` (méthode), comme illustré dans l’exemple de code suivant :

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

Le contrôle natif doit uniquement être configuré et abonner aux gestionnaires d’événements lorsque le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms. De même, les gestionnaires d’événements qui ont été abonnés doivent être désabonnés d’uniquement quand l’élément auquel le renderer est attaché aux modifications. Adopter cette approche vous aidera à créer un convertisseur personnalisé qui ne connaît pas les fuites de mémoire.

Une version substituée de la `OnElementPropertyChanged` , dans chaque classe de renderer spécifique à la plateforme, est le lieu pour répondre aux modifications de propriété pouvant être liée sur le contrôle personnalisé Xamarin.Forms. Une vérification de la propriété est modifiée doit toujours être effectuée, comme ce remplacement peut être appelé plusieurs fois.

Chaque classe de convertisseur personnalisé est décorée avec un `ExportRenderer` attribut qui inscrit le convertisseur avec Xamarin.Forms. L’attribut accepte deux paramètres : le nom de type du contrôle personnalisé Xamarin.Forms en cours d’affichage et le nom de type du convertisseur personnalisé. Le `assembly` préfixe à l’attribut spécifie que l’attribut s’applique à la totalité de l’assembly.

Les sections suivantes décrivent l’implémentation de chaque classe de convertisseur personnalisé spécifique à la plateforme.

### <a name="creating-the-custom-renderer-on-ios"></a>Création du convertisseur personnalisé sur iOS

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme iOS :

```csharp
[assembly: ExportRenderer (typeof(NativeListView), typeof(NativeiOSListViewRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSListViewRenderer : ListViewRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null) {
                // Unsubscribe
            }

            if (e.NewElement != null) {
                Control.Source = new NativeiOSListViewSource (e.NewElement as NativeListView);
            }
        }
    }
}
```

Le `UITableView` contrôle est configuré en créant une instance de la `NativeiOSListViewSource` classe, la condition que le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms. Cette classe fournit des données à la `UITableView` contrôle en substituant la `RowsInSection` et `GetCell` méthodes à partir de la `UITableViewSource` classe et en exposant un `Items` propriété qui contient la liste des données à afficher. La classe fournit également un `RowSelected` substitution de méthode qui appelle le `ItemSelected` événement fourni par le `NativeListView` contrôle personnalisé. Pour plus d’informations sur la méthode substitue, consultez [sous-classement UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). Le `GetCell` méthode retourne un `UITableCellView` qui est rempli avec des données pour chaque ligne dans la liste et est illustré dans l’exemple de code suivant :

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
  // request a recycled cell to save memory
  NativeiOSListViewCell cell = tableView.DequeueReusableCell (cellIdentifier) as NativeiOSListViewCell;

  // if there are no cells to reuse, create a new one
  if (cell == null) {
    cell = new NativeiOSListViewCell (cellIdentifier);
  }

  if (String.IsNullOrWhiteSpace (tableItems [indexPath.Row].ImageFilename)) {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , null);
  } else {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , UIImage.FromFile ("Images/" + tableItems [indexPath.Row].ImageFilename + ".jpg"));
  }

  return cell;
}
```

Cette méthode crée un `NativeiOSListViewCell` instance pour chaque ligne de données qui seront affichera sur l’écran. Le `NativeiOSCell` instance définit la disposition de chaque cellule et les données de la cellule. Lorsqu’une cellule disparaît de l’écran en raison de défilement, la cellule sera disponible pour une réutilisation. Cela évite de gaspiller la mémoire en veillant à ce qu’il existe uniquement `NativeiOSCell` instances pour les données affichées sur l’écran, plutôt que toutes les données dans la liste. Pour plus d’informations sur la réutilisation de la cellule, consultez [réutilisation des cellules](~/ios/user-interface/controls/tables/populating-a-table-with-data.md). Le `GetCell` méthode lit également le `ImageFilename` propriété de chaque ligne de données, à condition qu’il existe et lit l’image et la stocke en tant qu’un `UIImage` instance, avant la mise à jour le `NativeiOSListViewCell` de l’instance avec les données (nom, catégorie et image) pour la ligne.

Le `NativeiOSListViewCell` classe définit la disposition pour chaque cellule et est illustré dans l’exemple de code suivant :

```csharp
public class NativeiOSListViewCell : UITableViewCell
{
  UILabel headingLabel, subheadingLabel;
  UIImageView imageView;

  public NativeiOSListViewCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
  {
    SelectionStyle = UITableViewCellSelectionStyle.Gray;

    ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);

    imageView = new UIImageView ();

    headingLabel = new UILabel () {
      Font = UIFont.FromName ("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB (127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    subheadingLabel = new UILabel () {
      Font = UIFont.FromName ("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB (38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add (headingLabel);
    ContentView.Add (subheadingLabel);
    ContentView.Add (imageView);
  }

  public void UpdateCell (string caption, string subtitle, UIImage image)
  {
    headingLabel.Text = caption;
    subheadingLabel.Text = subtitle;
    imageView.Image = image;
  }

  public override void LayoutSubviews ()
  {
    base.LayoutSubviews ();

    headingLabel.Frame = new CoreGraphics.CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
    subheadingLabel.Frame = new CoreGraphics.CGRect (100, 18, 100, 20);
    imageView.Frame = new CoreGraphics.CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Cette classe définit les contrôles permettant de restituer le contenu de la cellule et leur disposition. Le `NativeiOSListViewCell` constructeur crée des instances de `UILabel` et `UIImageView` contrôle et initialise leur apparence. Ces contrôles sont utilisés pour afficher les données de chaque ligne, avec le `UpdateCell` méthode utilisée pour définir ces données sur le `UILabel` et `UIImageView` instances. L’emplacement de ces instances est défini par l’élément substitué `LayoutSubviews` méthode, en spécifiant leurs coordonnées au sein de la cellule.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Réponse à une modification de propriété sur le contrôle personnalisé

Si le `NativeListView.Items` propriété change, en raison des éléments ajoutés ou supprimés de la liste, le convertisseur personnalisé doit répondre en affichant les modifications. Cela peut être effectué en substituant le `OnElementPropertyChanged` (méthode), ce qui est illustré dans l’exemple de code suivant :

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

La méthode crée une nouvelle instance de la `NativeiOSListViewSource` classe qui fournit des données à la `UITableView` contrôler, à condition que le peut être liée `NativeListView.Items` propriété a été modifiée.

### <a name="creating-the-custom-renderer-on-android"></a>Création du convertisseur personnalisé sur Android

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme Android :

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeAndroidListViewRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidListViewRenderer : ListViewRenderer
    {
        Context _context;

        public NativeAndroidListViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // unsubscribe
                Control.ItemClick -= OnItemClick;
            }

            if (e.NewElement != null)
            {
                // subscribe
                Control.Adapter = new NativeAndroidListViewAdapter(_context as Android.App.Activity, e.NewElement as NativeListView);
                Control.ItemClick += OnItemClick;
            }
        }
        ...

        void OnItemClick(object sender, Android.Widget.AdapterView.ItemClickEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(((NativeListView)Element).Items.ToList()[e.Position - 1]);
        }
    }
}
```

Natif `ListView` contrôle est configuré à condition que le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms. Cette configuration implique la création d’une instance de la `NativeAndroidListViewAdapter` classe qui fournit des données natif `ListView` contrôler et l’inscription d’un gestionnaire d’événements pour traiter la `ItemClick` événement. À son tour, appelle ce gestionnaire le `ItemSelected` événement fourni par le `NativeListView` contrôle personnalisé. Le `ItemClick` désabonné d’événement si l’élément Xamarin.Forms le convertisseur est associé aux modifications.

Le `NativeAndroidListViewAdapter` dérive le `BaseAdapter` classe et expose un `Items` propriété qui contient la liste des données à afficher, ainsi que le remplacement le `Count`, `GetView`, `GetItemId`, et `this[int]` méthodes. Pour plus d’informations sur ces substitutions de méthode, consultez [implémentant un ListAdapter](~/android/user-interface/layouts/list-view/populating.md). Le `GetView` méthode retourne une vue pour chaque ligne, rempli avec les données et est illustré dans l’exemple de code suivant :

```csharp
public override View GetView (int position, View convertView, ViewGroup parent)
{
  var item = tableItems [position];

  var view = convertView;
  if (view == null) {
    // no view to re-use, create new
    view = context.LayoutInflater.Inflate (Resource.Layout.NativeAndroidListViewCell, null);
  }
  view.FindViewById<TextView> (Resource.Id.Text1).Text = item.Name;
  view.FindViewById<TextView> (Resource.Id.Text2).Text = item.Category;

  // grab the old image and dispose of it
  if (view.FindViewById<ImageView> (Resource.Id.Image).Drawable != null) {
    using (var image = view.FindViewById<ImageView> (Resource.Id.Image).Drawable as BitmapDrawable) {
      if (image != null) {
        if (image.Bitmap != null) {
          //image.Bitmap.Recycle ();
          image.Bitmap.Dispose ();
        }
      }
    }
  }

  // If a new image is required, display it
  if (!String.IsNullOrWhiteSpace (item.ImageFilename)) {
    context.Resources.GetBitmapAsync (item.ImageFilename).ContinueWith ((t) => {
      var bitmap = t.Result;
      if (bitmap != null) {
        view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (bitmap);
        bitmap.Dispose ();
      }
    }, TaskScheduler.FromCurrentSynchronizationContext ());
  } else {
    // clear the image
    view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (null);
  }

  return view;
}
```

Le `GetView` méthode est appelée pour retourner la cellule doit être restitué, comme un `View`, pour chaque ligne de données dans la liste. Il crée un `View` instance pour chaque ligne de données qui seront affichera sur l’écran, avec l’apparence de la `View` instance en cours de définition dans un fichier de disposition. Lorsqu’une cellule disparaît de l’écran en raison de défilement, la cellule sera disponible pour une réutilisation. Cela évite de gaspiller la mémoire en veillant à ce qu’il existe uniquement `View` instances pour les données affichées sur l’écran, plutôt que toutes les données dans la liste. Pour plus d’informations sur la réutilisation de la vue, consultez [réutilisation d’affichage ligne](~/android/user-interface/layouts/list-view/populating.md).

Le `GetView` méthode remplit également la `View` instance avec les données, et notamment de lire les données d’image à partir du nom de fichier spécifié dans le `ImageFilename` propriété.

La disposition de chaque dispayed cellule par natif `ListView` est défini dans le `NativeAndroidListViewCell.axml` fichier de disposition, qui est augmenté la `LayoutInflater.Inflate` (méthode). L’exemple de code suivant montre la définition de disposition :

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="8dp"
    android:background="@drawable/CustomSelector">
    <LinearLayout
        android:id="@+id/Text"
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
            android:textStyle="italic" />
        <TextView
            android:id="@+id/Text2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="14dip"
            android:textColor="#FF267F00"
            android:paddingLeft="100dip" />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout>
```

Cette disposition Spécifie que deux `TextView` contrôles et un `ImageView` contrôle servent à afficher le contenu de la cellule. Les deux `TextView` contrôles sont orientés verticalement dans une `LinearLayout` contrôle, tous les contrôles de contenu dans un `RelativeLayout`.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Réponse à une modification de propriété sur le contrôle personnalisé

Si le `NativeListView.Items` propriété change, en raison des éléments ajoutés ou supprimés de la liste, le convertisseur personnalisé doit répondre en affichant les modifications. Cela peut être effectué en substituant le `OnElementPropertyChanged` (méthode), ce qui est illustré dans l’exemple de code suivant :

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

La méthode crée une nouvelle instance de la `NativeAndroidListViewAdapter` classe qui fournit des données natif `ListView` contrôler, à condition que le peut être liée `NativeListView.Items` propriété a été modifiée.

### <a name="creating-the-custom-renderer-on-uwp"></a>Création du convertisseur personnalisé sur UWP

L’exemple de code suivant montre le convertisseur personnalisé pour UWP :

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeUWPListViewRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPListViewRenderer : ListViewRenderer
    {
        ListView listView;

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            listView = Control as ListView;

            if (e.OldElement != null)
            {
                // Unsubscribe
                listView.SelectionChanged -= OnSelectedItemChanged;
            }

            if (e.NewElement != null)
            {
                listView.SelectionMode = ListViewSelectionMode.Single;
                listView.IsItemClickEnabled = false;
                listView.ItemsSource = ((NativeListView)e.NewElement).Items;             
                listView.ItemTemplate = App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
                // Subscribe
                listView.SelectionChanged += OnSelectedItemChanged;
            }  
        }

        void OnSelectedItemChanged(object sender, SelectionChangedEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(listView.SelectedItem);
        }
    }
}
```

Natif `ListView` contrôle est configuré à condition que le convertisseur personnalisé est attaché à un nouvel élément Xamarin.Forms. Cette configuration implique de définir comment natif `ListView` contrôle répond aux éléments en cours sélectionnés, remplir les données affichées par le contrôle, définir l’apparence et le contenu de chaque cellule et l’inscription d’un gestionnaire d’événements pour traiter la `SelectionChanged` événement. À son tour, appelle ce gestionnaire le `ItemSelected` événement fourni par le `NativeListView` contrôle personnalisé. Le `SelectionChanged` désabonné d’événement si l’élément Xamarin.Forms le convertisseur est associé aux modifications.

L’apparence et le contenu de chaque natif `ListView` cellule sont définis par un `DataTemplate` nommé `ListViewItemTemplate`. Cela `DataTemplate` sont stockées dans le dictionnaire de ressources de niveau application et est illustré dans l’exemple de code suivant :

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="#DAFF7F">
        <Grid.Resources>
            <local:ConcatImageExtensionConverter x:Name="ConcatImageExtensionConverter" />
        </Grid.Resources>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.40*" />
            <ColumnDefinition Width="0.40*"/>
            <ColumnDefinition Width="0.20*" />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.ColumnSpan="2" Foreground="#7F3300" FontStyle="Italic" FontSize="22" VerticalAlignment="Top" Text="{Binding Name}" />
        <TextBlock Grid.RowSpan="2" Grid.Column="1" Foreground="#267F00" FontWeight="Bold" FontSize="12" VerticalAlignment="Bottom" Text="{Binding Category}" />
        <Image Grid.RowSpan="2" Grid.Column="2" HorizontalAlignment="Left" VerticalAlignment="Center" Source="{Binding ImageFilename, Converter={StaticResource ConcatImageExtensionConverter}}" Width="50" Height="50" />
        <Line Grid.Row="1" Grid.ColumnSpan="3" X1="0" X2="1" Margin="30,20,0,0" StrokeThickness="1" Stroke="LightGray" Stretch="Fill" VerticalAlignment="Bottom" />
    </Grid>
</DataTemplate>
```

Le `DataTemplate` spécifie les contrôles permettant d’afficher le contenu de la cellule et leur disposition et l’apparence. Deux `TextBlock` contrôles et un `Image` contrôle sont utilisés pour afficher le contenu de la cellule via la liaison de données. En outre, une instance de la `ConcatImageExtensionConverter` est utilisé pour concaténer le `.jpg` extension de fichier à chaque nom de fichier image. Cela garantit que le `Image` contrôle peut charger et afficher l’image lorsqu’il est `Source` propriété est définie.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>Réponse à une modification de propriété sur le contrôle personnalisé

Si le `NativeListView.Items` propriété change, en raison des éléments ajoutés ou supprimés de la liste, le convertisseur personnalisé doit répondre en affichant les modifications. Cela peut être effectué en substituant le `OnElementPropertyChanged` (méthode), ce qui est illustré dans l’exemple de code suivant :

```csharp
protected override void OnElementPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
    base.OnElementPropertyChanged(sender, e);

    if (e.PropertyName == NativeListView.ItemsProperty.PropertyName)
    {
        listView.ItemsSource = ((NativeListView)Element).Items;
    }
}
```

La méthode remplit natif `ListView` contrôle avec les données modifiées, à condition que le peut être liée `NativeListView.Items` propriété a été modifiée.

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer un convertisseur personnalisé qui encapsule des contrôles de liste de spécifique à la plateforme et les dispositions de cellule native, ce qui permet de mieux contrôler les performances du contrôle de liste natif.


## <a name="related-links"></a>Liens associés

- [CustomRendererListView (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
