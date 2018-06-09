---
title: Performances de ListView
description: Bien que ListView est un puissant pour afficher les données, il présente certaines limitations. Cet article explique comment s’assurer de hautes performances avec une liste de Xamarin.Forms dans une application.
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: 4803a612e2b06e458f2859dbbbd30b970f0fc8ea
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244901"
---
# <a name="listview-performance"></a>Performances de ListView

Lorsque vous écrivez des applications mobiles, des performances est importante. Les utilisateurs ont en droit d’attendre le défilement régulier et le temps de chargement rapide. Ne pas répondre aux attentes de vos utilisateurs coût de contrôle d’accès dans le magasin d’applications ou dans le cas d’une application métier-, coût votre temps à l’entreprise et l’argent.

Bien que [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) est une vue puissante pour afficher les données, il présente certaines limitations. Performances de défilement peut être impactées lors de l’utilisation de cellules personnalisés, en particulier lorsque qu’ils contiennent des hiérarchies imbriquées ou utilisent certaines dispositions qui nécessitent beaucoup de mesure. Heureusement, il existe des techniques que vous pouvez utiliser afin d’éviter des performances médiocres.

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>Stratégie de mise en cache

ListViews sont souvent utilisées pour afficher davantage de données que vous pouvez ajuster à l’écran. Considérez une application de la musique, par exemple. Une bibliothèque de chansons peut avoir des milliers d’entrées. L’approche simple, qui serait pour créer une ligne pour chaque morceau aurait des performances médiocres. Cette approche gaspille de la mémoire précieuse et peut ralentir le défilement pour une analyse. Une autre approche consiste à créer et détruire des lignes que les données devient visible. Cela requiert l’instanciation constante et le nettoyage des objets de vue, qui peut être très lent.

Pour économiser la mémoire, natif [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) équivalents pour chaque plateforme possèdent des fonctionnalités intégrées pour réutiliser des lignes. Seules les cellules visibles sur l’écran sont chargées en mémoire et le **contenu** est chargé dans les cellules existantes. Cela empêche l’application d’avoir à instancier des milliers d’objets, de gagner du temps et mémoire.

Permet de Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) cellule réutilisation via la [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) énumération, qui a les valeurs suivantes :

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> La plateforme Windows universelle (UWP) ignore le [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) mise en cache de la stratégie, car elle utilise toujours la mise en cache pour améliorer les performances. Par conséquent, par défaut il se comporte comme si le [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) mise en cache de la stratégie est appliquée.

### <a name="retainelement"></a>RetainElement

Le [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) mise en cache de stratégie spécifie que le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) génère une cellule pour chaque élément dans la liste, et est la valeur par défaut `ListView` comportement. En règle générale, il doit être utilisé dans les circonstances suivantes :

- Lorsque chaque cellule a un grand nombre de liaisons (20-30).
- Lorsque le modèle de cellule change fréquemment.
- Lorsque test révèle que la `RecycleElement` mise en cache des résultats de stratégie dans une vitesse d’exécution réduite.

Il est important de reconnaître les conséquences de la [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) mise en cache de stratégie lorsque vous travaillez avec des cellules personnalisées. Code d’initialisation de cellule devrez exécuter pour la création de chaque cellule, qui peut être plusieurs fois par seconde. Dans ce cas, les techniques de mise en page qui ont été correctement sur une page, comme l’utilisation de plusieurs imbriqués [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) instances, deviennent des goulots d’étranglement de performances lorsqu’ils sont le programme d’installation et de destruction en temps réel en tant que l’utilisateur fait défiler.

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

Le [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) mise en cache de stratégie spécifie que le [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) va tenter de réduire sa vitesse encombrement et l’exécution de la mémoire par le recyclage des cellules de la liste. Ce mode n’offre pas toujours une amélioration des performances et de test doit être effectué pour déterminer les améliorations. Toutefois, il est généralement le choix par défaut et doit être utilisé dans les circonstances suivantes :

- Lorsque chaque cellule a un petit nombre modéré de liaisons.
- Lors de chaque cellule [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) définit toutes les données de cellule.
- Lorsque chaque cellule est en grande partie similaire, et le modèle de cellule qui ne changent pas.

Lors de la virtualisation, la cellule aura son contexte de liaison mis à jour, et par conséquent, si une application utilise ce mode elle doit vérifier que les mises à jour du contexte de liaison sont gérés de façon appropriée. Toutes les données relatives à la cellule doivent provenir du contexte de liaison ou les erreurs de cohérence peuvent se produire. Cela peut être accompli à l’aide de la liaison de données pour afficher les données des cellules. Vous pouvez également les données des cellules doivent être définies dans le `OnBindingContextChanged` remplacer, plutôt que dans le constructeur de la cellule personnalisé, comme illustré dans l’exemple de code suivant :

```csharp
public class CustomCell : ViewCell
{
  Image image = null;

  public CustomCell ()
  {
    image = new Image();
    View = image;
  }

  protected override void OnBindingContextChanged ()
  {
    base.OnBindingContextChanged ();

    var item = BindingContext as ImageItem;
    if (item != null) {
      image.Source = item.ImageUrl;
    }
  }
}
```

Pour plus d’informations, consultez [changements de contexte de liaison](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

Sur iOS et Android, si les cellules utilisent les convertisseurs personnalisés, ils doivent vous assurer que notification de modification de propriété est correctement implémentée. Lorsque les cellules sont réutilisés leurs valeurs de propriété change lorsque le contexte de liaison est mise à jour à celle d’une cellule disponible, avec `PropertyChanged` événements déclenchés. Pour plus d’informations, consultez [personnalisation un ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>RecycleElement avec un DataTemplateSelector

Lorsqu’un [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) utilise un [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) pour sélectionner un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), le [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) mise en cache stratégie de ne pas mettre en cache `DataTemplate`s. Au lieu de cela, un `DataTemplate` est sélectionné pour chaque élément de données dans la liste.

> [!NOTE]
> Le [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) mise en cache de la stratégie a une condition préalable, introduite dans Xamarin.Forms 2.4, qui, lorsqu’un [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) est invité à en sélectionner un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)que chaque `DataTemplate` doit retourner le même [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) type. Par exemple, prenons un [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) avec un `DataTemplateSelector` qui peut retourner `MyDataTemplateA` (où `MyDataTemplateA` retourne un `ViewCell` de type `MyViewCellA`), ou `MyDataTemplateB` (où `MyDataTemplateB`retourne un `ViewCell` de type `MyViewCellB`), lorsque `MyDataTemplateA` est retourné doit retourner `MyViewCellA` ou une exception sera levée.

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

Le [ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) mise en cache de la stratégie s’appuie sur le [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) mise en cache de la stratégie en plus de garantir que quand un [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) utilise un [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) pour sélectionner un [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), `DataTemplate`s sont mises en cache par le type d’élément dans la liste. Par conséquent, `DataTemplate`s sont sélectionnées en une seule fois par type d’élément, au lieu d’une fois par instance d’élément.

> [!NOTE]
> Le [ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) mise en cache de la stratégie est un composant requis qui le `DataTemplate`retournés par le [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) doit utiliser le [ `DataTemplate` ](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) constructeur qui accepte un `Type`.

### <a name="setting-the-caching-strategy"></a>Définition de la stratégie de mise en cache

Le [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) valeur d’énumération est spécifiée avec un [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) surcharge de constructeur, comme indiqué dans l’exemple de code suivant :

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

En XAML, affectez le `CachingStrategy` d’attributs comme indiqué dans le code ci-dessous :

```xaml
<ListView CachingStrategy="RecycleElement">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
              ...
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Cela a le même effet que la définition de l’argument de stratégie de mise en cache dans le constructeur en c# ; Notez qu’il existe aucune `CachingStrategy` propriété sur `ListView`.

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>Définition de la stratégie de mise en cache dans un ListView sous-classé

Définition de la `CachingStrategy` attribut à partir de XAML sur un sous-classé [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ne produira pas le comportement souhaité, étant donné qu’aucun `CachingStrategy` propriété sur `ListView`. En outre, si [XAMLC](~/xamarin-forms/xaml/xamlc.md) est activé, le message d’erreur suivant est généré : **aucune propriété, une propriété pouvant être liée ou un événement trouvée pour 'CachingStrategy'**

La solution à ce problème consiste à spécifier un constructeur sur sous-classé [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) qui accepte un [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) paramètre et la passe la classe de base :

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

Le [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) valeur d’énumération peut être spécifiée à partir de XAML à l’aide de la `x:Arguments` syntaxe :

```xaml
<local:CustomListView>
  <x:Arguments>
    <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
  </x:Arguments>
</local:CustomListView>
```

<a name="improving-performance" />

## <a name="improving-listview-performance"></a>Amélioration des performances de ListView

Il existe de nombreuses techniques pour améliorer les performances d’un `ListView`:

-  Lier le `ItemsSource` propriété un `IList<T>` collection au lieu d’un `IEnumerable<T>` collection, car `IEnumerable<T>` collections ne prennent pas en charge l’accès aléatoire.
-  Utilisez les cellules intégrés (comme `TextCell`  /  `SwitchCell` ) au lieu de `ViewCell` chaque fois que vous pouvez.
-  Utiliser moins d’éléments. Examinons, par exemple à l’aide d’un seul `FormattedString` étiquette au lieu de plusieurs étiquettes.
-  Remplacez le `ListView` avec un `TableView` lors de l’affichage des données non homogène : autrement dit, les données de types différents.
-  Limitez l’utilisation de la [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) (méthode). Si l’utilisation excessive, il sera dégrader les performances.
-  Sur Android, évitez de définir un `ListView`de visibilité de séparateur de ligne ou de la couleur après qu’il a été instancié, qu’elle provoque une baisse des performances.
-  Évitez de modifier la disposition des cellules selon la [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Cela entraîne des coûts importants de mise en page et de l’initialisation.
-  Éviter les hiérarchies de disposition imbriquées. Utilisez `AbsoluteLayout` ou `Grid` pour aider à réduire l’imbrication.
-  Éviter spécifique `LayoutOptions` autre que `Fill` (remplissage est le cher de calcul).
-  Éviter de placer un `ListView` à l’intérieur d’un `ScrollView` pour les raisons suivantes :
  - Le `ListView` implémente son propre défilement.
  - Le `ListView` ne recevront pas les mouvements, lorsqu’ils sont gérés par le parent `ScrollView`.
  - Le `ListView` peut présenter un en-tête personnalisé et le pied de page qui fait défiler les éléments de la liste, potentiellement offre les fonctionnalités qui le `ScrollView` a été utilisé pour. Pour plus d’informations, consultez [en-têtes et pieds de page](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers).
-  Envisagez un convertisseur personnalisé si vous avez besoin d’une conception très spécifique, complexe présentée dans vos cellules.

`AbsoluteLayout` a la possibilité d’effectuer des mises en page sans un appel d’une mesure unique. Cela rend très puissant pour les performances. Si `AbsoluteLayout` ne peut pas être utilisé, pensez à [ `RelativeLayout` ](http://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/). Si vous utilisez `RelativeLayout`, en transmettant les contraintes directement sera beaucoup plus rapide que l’utilisation de l’API de l’expression. C’est parce que l’API de l’expression utilise JIT et sur iOS l’arborescence doit être interprétée, qui est plus long. L’API de l’expression est adaptée aux mises en page où il suffit de disposition initiale et la rotation, mais dans `ListView`, où il est exécuté en permanence pendant le défilement, elle nuit aux performances.

Création d’un convertisseur personnalisé pour un [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ou ses cellules constitue une approche pour réduire l’effet des calculs de disposition sur les performances de défilement. Pour plus d’informations, consultez [personnalisation un ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) et [personnalisation un ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).


## <a name="related-links"></a>Liens associés

- [Affichage de convertisseur personnalisé (exemple)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [ViewCell convertisseur personnalisé (exemple)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)
