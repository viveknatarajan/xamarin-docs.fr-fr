---
title: Personnalisation d’une ViewCell
description: Une Xamarin.Forms ViewCell est une cellule qui peut être ajoutée à un ListView ou à TableView, qui contient une vue définie par le développeur. Cet article montre comment créer un convertisseur personnalisé pour d’une ViewCell qui est hébergé à l’intérieur d’un contrôle ListView de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 3cb4d7f152e0f9540275f12f0ade568cd0552784
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935574"
---
# <a name="customizing-a-viewcell"></a>Personnalisation d’une ViewCell

_Une Xamarin.Forms ViewCell est une cellule qui peut être ajoutée à un ListView ou à TableView, qui contient une vue définie par le développeur. Cet article montre comment créer un convertisseur personnalisé pour d’une ViewCell qui est hébergé à l’intérieur d’un contrôle ListView de Xamarin.Forms. Cela arrête les calculs de dispositions Xamarin.Forms d’être appelée à plusieurs reprises lors du défilement du ListView._

Toutes les cellules Xamarin.Forms a un convertisseur qui accompagne cet article pour chaque plateforme qui crée une instance d’un contrôle natif. Quand un [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) est restitué par une application Xamarin.Forms, dans iOS le `ViewCellRenderer` classe est instanciée, ce qui instancie à son tour native `UITableViewCell` contrôle. Sur la plateforme Android, le `ViewCellRenderer` classe instancie native `View` contrôle. Sur la plateforme de Windows universelle (UWP), le `ViewCellRenderer` classe instancie native `DataTemplate`. Pour plus d’informations sur les classes de contrôle natif correspondant aux contrôles Xamarin.Forms et le convertisseur, consultez [convertisseur des Classes de Base et des contrôles natifs](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Le diagramme suivant illustre la relation entre la [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) et les contrôles natifs correspondants qui l’implémentent :

![](viewcell-images/viewcell-classes.png "Relation entre le contrôle ViewCell et l’implémentation des contrôles natifs")

Le processus de rendu peut être exploitée pour implémenter les personnalisations spécifiques à la plateforme en créant un convertisseur personnalisé pour un [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) sur chaque plateforme. Le processus pour effectuer cette opération est la suivante :

1. [Créer](#Creating_the_Custom_Cell) une cellule personnalisée Xamarin.Forms.
1. [Consommer](#Consuming_the_Custom_Cell) la cellule personnalisée à partir de Xamarin.Forms.
1. [Créer](#Creating_the_Custom_Renderer_on_each_Platform) le convertisseur personnalisé pour la cellule sur chaque plateforme.

Chaque élément maintenant nous reviendrons à son tour, pour implémenter un `NativeCell` convertisseur qui tire parti d’une disposition spécifique à la plateforme pour chaque cellule hébergé à l’intérieur d’un Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) contrôle. Cela arrête les calculs de dispositions Xamarin.Forms d’être appelée à plusieurs reprises au cours de `ListView` le défilement.

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>Création de la cellule personnalisée

Un contrôle de cellule personnalisée peut être créé en sous-classant la [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) classe, comme indiqué dans l’exemple de code suivant :

```csharp
public class NativeCell : ViewCell
{
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(NativeCell), "");

  public string Name {
    get { return (string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }

  public static readonly BindableProperty CategoryProperty =
    BindableProperty.Create ("Category", typeof(string), typeof(NativeCell), "");

  public string Category {
    get { return (string)GetValue (CategoryProperty); }
    set { SetValue (CategoryProperty, value); }
  }

  public static readonly BindableProperty ImageFilenameProperty =
    BindableProperty.Create ("ImageFilename", typeof(string), typeof(NativeCell), "");

  public string ImageFilename {
    get { return (string)GetValue (ImageFilenameProperty); }
    set { SetValue (ImageFilenameProperty, value); }
  }
}
```
Le `NativeCell` classe est créée dans le projet de bibliothèque .NET Standard et définit l’API pour la cellule personnalisée. La cellule personnalisée expose `Name`, `Category`, et `ImageFilename` propriétés qui peuvent être affichées via la liaison de données. Pour plus d’informations sur la liaison de données, consultez [Notions de base de la liaison de données](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>Utilisation de la cellule personnalisée

Le `NativeCell` cellule personnalisée peut être référencée dans Xaml dans le projet de bibliothèque .NET Standard en déclarant un espace de noms pour son emplacement et en utilisant le préfixe d’espace de noms sur l’élément de cellule personnalisé. Le code suivant montre l’exemple comment la `NativeCell` cellule personnalisée peut être consommée par une page XAML :

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <StackLayout>
              <Label Text="Xamarin.Forms native cell" HorizontalTextAlignment="Center" />
              <ListView x:Name="listView" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                  <ListView.ItemTemplate>
                      <DataTemplate>
                          <local:NativeCell Name="{Binding Name}" Category="{Binding Category}" ImageFilename="{Binding ImageFilename}" />
                      </DataTemplate>
                  </ListView.ItemTemplate>
              </ListView>
          </StackLayout>
      </ContentPage.Content>
</ContentPage>
```

Le `local` préfixe d’espace de noms peut être n’importe quel nom. Toutefois, le `clr-namespace` et `assembly` valeurs doivent correspondre les détails du contrôle personnalisé. Une fois que l’espace de noms est déclaré, le préfixe est utilisé pour faire référence à la cellule personnalisée.

Le code suivant montre l’exemple comment la `NativeCell` cellule personnalisée peut être consommée par une page c# :

```csharp
public class NativeCellPageCS : ContentPage
{
    ListView listView;

    public NativeCellPageCS()
    {
        listView = new ListView(ListViewCachingStrategy.RecycleElement)
        {
            ItemsSource = DataSource.GetList(),
            ItemTemplate = new DataTemplate(() =>
            {
                var nativeCell = new NativeCell();
                nativeCell.SetBinding(NativeCell.NameProperty, "Name");
                nativeCell.SetBinding(NativeCell.CategoryProperty, "Category");
                nativeCell.SetBinding(NativeCell.ImageFilenameProperty, "ImageFilename");

                return nativeCell;
            })
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

        Content = new StackLayout
        {
            Children = {
                new Label { Text = "Xamarin.Forms native cell", HorizontalTextAlignment = TextAlignment.Center },
                listView
            }
        };
        listView.ItemSelected += OnItemSelected;
    }
    ...
}
```

Un Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) contrôle est utilisé pour afficher une liste de données, qui sont remplies par le biais du [ `ItemSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemsSource/) propriété. Le [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) stratégie de mise en cache tente de réduire le `ListView` encombrement mémoire et l’exécution de la vitesse en recyclant les cellules de la liste. Pour plus d’informations, consultez [stratégie de mise en cache](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

Chaque ligne dans la liste contient trois éléments de données : un nom, une catégorie et un nom de fichier image. La disposition de chaque ligne dans la liste est définie par le `DataTemplate` qui est référencé par le biais du [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemTemplate/) propriété pouvant être liée. Le `DataTemplate` définit que chaque ligne de données dans la liste sera un `NativeCell` qui affiche son `Name`, `Category`, et `ImageFilename` propriétés via la liaison de données. Pour plus d’informations sur la `ListView` du contrôle, consultez [ListView](~/xamarin-forms/user-interface/listview/index.md).

Un convertisseur personnalisé peut maintenant être ajouté à chaque projet d’application pour personnaliser la disposition de la plateforme spécifique pour chaque cellule.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Création du convertisseur personnalisé sur chaque plateforme

Le processus de création de la classe de convertisseur personnalisé est comme suit :

1. Créer une sous-classe de la `ViewCellRenderer` classe qui rend la cellule personnalisée.
1. Substituez la méthode spécifique à la plateforme qui restitue la cellule personnalisée et écrire la logique pour le personnaliser.
1. Ajouter un `ExportRenderer` d’attribut à la classe de convertisseur personnalisé pour indiquer qu’il est utilisée pour rendre la cellule personnalisée Xamarin.Forms. Cet attribut est utilisé pour inscrire le renderer personnalisé avec Xamarin.Forms.

> [!NOTE]
> Pour la plupart des éléments de Xamarin.Forms, il est facultatif pour fournir un convertisseur personnalisé dans chaque projet de plateforme. Si un convertisseur personnalisé n’est pas inscrit, le convertisseur par défaut pour la classe de base du contrôle sera être utilisé. Toutefois, les convertisseurs personnalisés sont nécessaires dans chaque projet de plateforme lors du rendu d’un [ViewCell](xref:Xamarin.Forms.ViewCell) élément.

Le diagramme suivant illustre les responsabilités de chaque projet dans l’exemple d’application, ainsi que les relations entre eux :

![](viewcell-images/solution-structure.png "Responsabilités de projet de convertisseur NativeCell personnalisé")

Le `NativeCell` cellule personnalisée est restitué par les classes de renderer spécifique à la plateforme, qui dérivent toutes de la `ViewCellRenderer` classe pour chaque plateforme. Ainsi, chaque `NativeCell` personnalisé cellule rendue avec une disposition spécifique à la plateforme, comme indiqué dans les captures d’écran suivante :

![](viewcell-images/screenshots.png "NativeCell sur chaque plateforme")

Le `ViewCellRenderer` classe expose des méthodes spécifiques à la plateforme pour le rendu de la cellule personnalisée. Il s’agit le `GetCell` méthode sur la plateforme iOS, le `GetCellCore` méthode sur la plateforme Android et le `GetTemplate` méthode sur UWP.

Chaque classe de convertisseur personnalisé est décorée avec un `ExportRenderer` attribut qui inscrit le convertisseur avec Xamarin.Forms. L’attribut accepte deux paramètres : le nom de type de la cellule de Xamarin.Forms en cours d’affichage et le nom de type du convertisseur personnalisé. Le `assembly` préfixe à l’attribut spécifie que l’attribut s’applique à la totalité de l’assembly.

Les sections suivantes décrivent l’implémentation de chaque classe de convertisseur personnalisé spécifique à la plateforme.

### <a name="creating-the-custom-renderer-on-ios"></a>Création du convertisseur personnalisé sur iOS

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme iOS :

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeiOSCellRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        NativeiOSCell cell;

        public override UITableViewCell GetCell(Cell item, UITableViewCell reusableCell, UITableView tv)
        {
            var nativeCell = (NativeCell)item;

            cell = reusableCell as NativeiOSCell;
            if (cell == null)
                cell = new NativeiOSCell(item.GetType().FullName, nativeCell);
            else
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;
            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

Le `GetCell` méthode est appelée pour générer chaque cellule à afficher. Chaque cellule est une `NativeiOSCell` instance, qui définit la disposition de la cellule et de ses données. L’opération de la `GetCell` méthode dépend de la [ `ListView` ](xref:Xamarin.Forms.ListView) mise en cache de la stratégie :

- Lorsque le [ `ListView` ](xref:Xamarin.Forms.ListView) mise en cache de la stratégie est [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement), le `GetCell` méthode sera appelée pour chaque cellule. Un `NativeiOSCell` instance sera créée pour chaque `NativeCell` instance initialement affiché sur l’écran. Lorsque l’utilisateur fait défiler le `ListView`, `NativeiOSCell` instances sera utilisées à nouveau. Pour plus d’informations sur la réutilisation de cellule iOS, consultez [réutilisation des cellules](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

  > [!NOTE]
  > Ce code de convertisseur personnalisé effectue certaines cellules nouveau utiliser même lorsque le [ `ListView` ](xref:Xamarin.Forms.ListView) est définie pour conserver des cellules.

  Les données affichées par chaque `NativeiOSCell` instance, si nouvellement créé ou réutilisées, sera actualisée avec les données à partir de chaque `NativeCell` de l’instance par le `UpdateCell` (méthode).

  > [!NOTE]
  > Le `OnNativeCellPropertyChanged` méthode ne sera jamais appelé lorsque le [ `ListView` ](xref:Xamarin.Forms.ListView) mise en cache de la stratégie est définie pour conserver des cellules.

- Lorsque le [ `ListView` ](xref:Xamarin.Forms.ListView) mise en cache de la stratégie est [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), le `GetCell` méthode sera appelée pour chaque cellule d’affichage initial sur l’écran. Un `NativeiOSCell` instance sera créée pour chaque `NativeCell` instance initialement affiché sur l’écran. Les données affichées par chaque `NativeiOSCell` instance sera actualisée avec les données à partir de la `NativeCell` de l’instance par le `UpdateCell` (méthode). Toutefois, le `GetCell` méthode n’être invoquée en tant que l’utilisateur fait défiler le `ListView`. Au lieu de cela, le `NativeiOSCell` instances sera utilisées à nouveau. `PropertyChanged` événements seront déclenchés sur le `NativeCell` lorsque ses données sont modifiées, l’instance et le `OnNativeCellPropertyChanged` Gestionnaire d’événements met à jour les données dans chaque réutilisées `NativeiOSCell` instance.

Le code suivant montre l’exemple le `OnNativeCellPropertyChanged` méthode qui a appelé quand un `PropertyChanged` événement est déclenché :

```csharp
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingLabel.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingLabel.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.CellImageView.Image = cell.GetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Cette méthode met à jour des données affichées par réutilisées `NativeiOSCell` instances. Un contrôle pour la propriété qui est modifiée est effectué, comme la méthode peut être appelée plusieurs fois.

Le `NativeiOSCell` classe définit la disposition pour chaque cellule et est illustré dans l’exemple de code suivant :

```csharp
internal class NativeiOSCell : UITableViewCell, INativeElementView
{
  public UILabel HeadingLabel { get; set; }
  public UILabel SubheadingLabel { get; set; }
  public UIImageView CellImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeiOSCell(string cellId, NativeCell cell) : base(UITableViewCellStyle.Default, cellId)
  {
    NativeCell = cell;

    SelectionStyle = UITableViewCellSelectionStyle.Gray;
    ContentView.BackgroundColor = UIColor.FromRGB(255, 255, 224);
    CellImageView = new UIImageView();

    HeadingLabel = new UILabel()
    {
      Font = UIFont.FromName("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB(127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    SubheadingLabel = new UILabel()
    {
      Font = UIFont.FromName("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB(38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add(HeadingLabel);
    ContentView.Add(SubheadingLabel);
    ContentView.Add(CellImageView);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingLabel.Text = cell.Name;
    SubheadingLabel.Text = cell.Category;
    CellImageView.Image = GetImage(cell.ImageFilename);
  }

  public UIImage GetImage(string filename)
  {
    return (!string.IsNullOrWhiteSpace(filename)) ? UIImage.FromFile("Images/" + filename + ".jpg") : null;
  }

  public override void LayoutSubviews()
  {
    base.LayoutSubviews();

    HeadingLabel.Frame = new CGRect(5, 4, ContentView.Bounds.Width - 63, 25);
    SubheadingLabel.Frame = new CGRect(100, 18, 100, 20);
    CellImageView.Frame = new CGRect(ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

Cette classe définit les contrôles permettant de restituer le contenu de la cellule et leur disposition. Implémente la classe le [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) interface, ce qui est requis quand le [ `ListView` ](xref:Xamarin.Forms.ListView) utilise le [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) mise en cache de la stratégie. Cette interface spécifie que la classe doit implémenter la [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) propriété, qui doit retourner les données de cellule personnalisée pour les cellules recyclés.

Le `NativeiOSCell` constructeur initialise l’apparence de la `HeadingLabel`, `SubheadingLabel`, et `CellImageView` propriétés. Ces propriétés sont utilisées pour afficher les données stockées dans le `NativeCell` instance, avec le `UpdateCell` méthode appelée pour définir la valeur de chaque propriété. En outre, lorsque le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) utilise le [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) mise en cache de la stratégie, les données affichées par le `HeadingLabel`, `SubheadingLabel`, et `CellImageView` propriétés peuvent être mise à jour par le `OnNativeCellPropertyChanged` méthode dans le convertisseur personnalisé.

Disposition de la cellule est effectuée par le `LayoutSubviews` remplacer, qui définit les coordonnées de `HeadingLabel`, `SubheadingLabel`, et `CellImageView` au sein de la cellule.

### <a name="creating-the-custom-renderer-on-android"></a>Création du convertisseur personnalisé sur Android

L’exemple de code suivant montre le convertisseur personnalisé pour la plateforme Android :

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeAndroidCellRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        NativeAndroidCell cell;

        protected override Android.Views.View GetCellCore(Cell item, Android.Views.View convertView, ViewGroup parent, Context context)
        {
            var nativeCell = (NativeCell)item;
            Console.WriteLine("\t\t" + nativeCell.Name);

            cell = convertView as NativeAndroidCell;
            if (cell == null)
            {
                cell = new NativeAndroidCell(context, nativeCell);
            }
            else
            {
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;
            }

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;

            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

Le `GetCellCore` méthode est appelée pour générer chaque cellule à afficher. Chaque cellule est une `NativeAndroidCell` instance, qui définit la disposition de la cellule et de ses données. L’opération de la `GetCellCore` méthode dépend de la [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) mise en cache de la stratégie :

- Lorsque le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) mise en cache de la stratégie est [ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement), le `GetCellCore` méthode sera appelée pour chaque cellule. Un `NativeAndroidCell` sera créé pour chaque `NativeCell` instance initialement affiché sur l’écran. Lorsque l’utilisateur fait défiler le `ListView`, `NativeAndroidCell` instances sera utilisées à nouveau. Pour plus d’informations sur la cellule Android réutiliser, consultez [réutilisation d’affichage ligne](~/android/user-interface/layouts/list-view/populating.md).

  > [!NOTE]
  > Notez que ce code de convertisseur personnalisé effectuera une cellule réutiliser même lorsque le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est définie pour conserver des cellules.

  Les données affichées par chaque `NativeAndroidCell` instance, si nouvellement créé ou réutilisées, sera actualisée avec les données à partir de chaque `NativeCell` de l’instance par le `UpdateCell` (méthode).

  > [!NOTE]
  > Notez que pendant la `OnNativeCellPropertyChanged` méthode sera appelée lorsque le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est définie pour conserver des cellules, il ne met pas à jour le `NativeAndroidCell` les valeurs de propriété.

- Lorsque le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) mise en cache de la stratégie est [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement), le `GetCellCore` méthode sera appelée pour chaque cellule d’affichage initial sur l’écran. Un `NativeAndroidCell` instance sera créée pour chaque `NativeCell` instance initialement affiché sur l’écran. Les données affichées par chaque `NativeAndroidCell` instance sera actualisée avec les données à partir de la `NativeCell` de l’instance par le `UpdateCell` (méthode). Toutefois, le `GetCellCore` méthode n’être invoquée en tant que l’utilisateur fait défiler le `ListView`. Au lieu de cela, le `NativeAndroidCell` instances sera utilisées à nouveau.  `PropertyChanged` événements seront déclenchés sur le `NativeCell` lorsque ses données sont modifiées, l’instance et le `OnNativeCellPropertyChanged` Gestionnaire d’événements met à jour les données dans chaque réutilisées `NativeAndroidCell` instance.

Le code suivant montre l’exemple le `OnNativeCellPropertyChanged` méthode qui a appelé quand un `PropertyChanged` événement est déclenché :

```csharp
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingTextView.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingTextView.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.SetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

Cette méthode met à jour des données affichées par réutilisées `NativeAndroidCell` instances. Un contrôle pour la propriété qui est modifiée est effectué, comme la méthode peut être appelée plusieurs fois.

Le `NativeAndroidCell` classe définit la disposition pour chaque cellule et est illustré dans l’exemple de code suivant :

```csharp
internal class NativeAndroidCell : LinearLayout, INativeElementView
{
  public TextView HeadingTextView { get; set; }
  public TextView SubheadingTextView { get; set; }
  public ImageView ImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeAndroidCell(Context context, NativeCell cell) : base(context)
  {
    NativeCell = cell;

    var view = (context as Activity).LayoutInflater.Inflate(Resource.Layout.NativeAndroidCell, null);
    HeadingTextView = view.FindViewById<TextView>(Resource.Id.HeadingText);
    SubheadingTextView = view.FindViewById<TextView>(Resource.Id.SubheadingText);
    ImageView = view.FindViewById<ImageView>(Resource.Id.Image);

    AddView(view);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingTextView.Text = cell.Name;
    SubheadingTextView.Text = cell.Category;

    // Dispose of the old image
    if (ImageView.Drawable != null)
    {
      using (var image = ImageView.Drawable as BitmapDrawable)
      {
        if (image != null)
        {
          if (image.Bitmap != null)
          {
            image.Bitmap.Dispose();
          }
        }
      }
    }

    SetImage(cell.ImageFilename);
  }

  public void SetImage(string filename)
  {
    if (!string.IsNullOrWhiteSpace(filename))
    {
      // Display new image
      Context.Resources.GetBitmapAsync(filename).ContinueWith((t) =>
      {
        var bitmap = t.Result;
        if (bitmap != null)
        {
          ImageView.SetImageBitmap(bitmap);
          bitmap.Dispose();
        }
      }, TaskScheduler.FromCurrentSynchronizationContext());
    }
    else
    {
      // Clear the image
      ImageView.SetImageBitmap(null);
    }
  }
}
```

Cette classe définit les contrôles permettant de restituer le contenu de la cellule et leur disposition. Implémente la classe le [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) interface, ce qui est requis quand le [ `ListView` ](xref:Xamarin.Forms.ListView) utilise le [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) mise en cache de la stratégie. Cette interface spécifie que la classe doit implémenter la [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) propriété, qui doit retourner les données de cellule personnalisée pour les cellules recyclés.

Le `NativeAndroidCell` constructeur augmente la `NativeAndroidCell` mise en page et l’initialise le `HeadingTextView`, `SubheadingTextView`, et `ImageView` propriétés aux contrôles dans la disposition d’augmentation. Ces propriétés sont utilisées pour afficher les données stockées dans le `NativeCell` instance, avec le `UpdateCell` méthode appelée pour définir la valeur de chaque propriété. En outre, lorsque le [ `ListView` ](xref:Xamarin.Forms.ListView) utilise le [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) mise en cache de la stratégie, les données affichées par le `HeadingTextView`, `SubheadingTextView`, et `ImageView` propriétés peuvent être mise à jour par le `OnNativeCellPropertyChanged` méthode dans le convertisseur personnalisé.

L’exemple de code suivant montre la définition de mise en page pour le `NativeAndroidCell.axml` fichier de disposition :

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
            android:id="@+id/HeadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/SubheadingText"
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

### <a name="creating-the-custom-renderer-on-uwp"></a>Création du convertisseur personnalisé sur UWP

L’exemple de code suivant montre le convertisseur personnalisé pour UWP :

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeUWPCellRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPCellRenderer : ViewCellRenderer
    {
        public override Windows.UI.Xaml.DataTemplate GetTemplate(Cell cell)
        {
            return App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
        }
    }
}
```

Le `GetTemplate` méthode est appelée pour retourner la cellule doit être restitué pour chaque ligne de données dans la liste. Il crée un `DataTemplate` pour chaque `NativeCell` instance qui s’affichera sur l’écran, avec la `DataTemplate` définir l’apparence et le contenu de la cellule.

Le `DataTemplate` est stocké dans le dictionnaire de ressources de niveau application et est illustré dans l’exemple de code suivant :

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="LightYellow">
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

## <a name="summary"></a>Récapitulatif

Cet article a montré comment créer un convertisseur personnalisé pour un [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) qui est hébergé à l’intérieur d’un Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) contrôle. Cela arrête les calculs de dispositions Xamarin.Forms d’être appelée à plusieurs reprises au cours de `ListView` le défilement.


## <a name="related-links"></a>Liens associés

- [Performances de ListView](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
