---
title: Introduction aux MonoTouch.Dialog
description: "Le MonoTouch.Dialog (MT. D) Outils d’analyse est une infrastructure indispensable pour le développement de l’interface utilisateur dans Xamarin.iOS rapide d’application. MT. D vous permet de rapidement et facilement définir l’interface utilisateur à l’aide d’une approche déclarative, au lieu de contrôleurs de navigation, les tables, etc. fastidieuses d’application complexe. En outre, MT. D a un ensemble flexible d’API qui permettent aux développeurs avec un contrôle complet ou remet approche, ainsi que des fonctionnalités supplémentaires telles que l’image d’arrière-plan tirer pour actualiser le chargement, rechercher la prise en charge et la génération dynamique de l’interface utilisateur via les données JSON. Ce guide présente les différentes façons de travailler avec MT. D et puis explore profondeur d’usage avancés."
ms.topic: article
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b279f3e643e008e88b8ad086c400d992427c6df4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-monotouchdialog"></a>Introduction aux MonoTouch.Dialog

_Le MonoTouch.Dialog (MT. D) Outils d’analyse est une infrastructure indispensable pour le développement de l’interface utilisateur dans Xamarin.iOS rapide d’application. MT. D vous permet de rapidement et facilement définir l’interface utilisateur à l’aide d’une approche déclarative, au lieu de contrôleurs de navigation, les tables, etc. fastidieuses d’application complexe. En outre, MT. D a un ensemble flexible d’API qui permettent aux développeurs avec un contrôle complet ou remet approche, ainsi que des fonctionnalités supplémentaires telles que l’image d’arrière-plan tirer pour actualiser le chargement, rechercher la prise en charge et la génération dynamique de l’interface utilisateur via les données JSON. Ce guide présente les différentes façons de travailler avec MT. D et puis explore profondeur d’usage avancés._


MonoTouch.Dialog, appelé MT. D pour faire court, est un kit de développement de l’interface utilisateur rapid qui permet aux développeurs de créer des écrans de l’application et de navigation à l’aide des informations plutôt que de créer des contrôleurs de la vue, les tables, etc. fastidieuses. Par conséquent, il fournit une simplification significative de réduction de développement et le code de l’interface utilisateur. Par exemple, considérez la capture d’écran suivante :

 [ ![](images/image1.png "Par exemple, considérez cette capture d’écran")](images/image1.png)

Le code suivant a été utilisé pour définir cet ensemble de l’écran :

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}
        
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;
    [Section("Expense Details")]
  
    [Caption("Description")]
    [Entry]
    public string Details;
        
    [Checkbox]
    public bool IsApproved = true;
    [Caption("Category")]
    public Category ExpenseCategory;
}
```

Lorsque vous travaillez avec des tables dans iOS, il existe souvent une multitude de code répétitive.
Par exemple, chaque fois qu’une table est nécessaire, une source de données est nécessaire pour remplir cette table. Dans une application qui a deux écrans basée sur une table qui sont connectés via un contrôleur de navigation, chaque écran partage un grand nombre du même code.

MT. D qui simplifie en encapsulant tout ce code dans une API générique pour la création de la table. Il fournit une abstraction au-dessus de cette API permet pour un objet déclaratif de syntaxe qui facilite encore la liaison. Par conséquent, il existe deux API dans Mt. D:

-   **API de bas niveau éléments** : *éléments API* est basée sur la création d’une arborescence hiérarchique d’éléments qui représentent des écrans et leurs composants. L’API des éléments offre aux développeurs la plus grande souplesse et contrôle pour la création d’interfaces utilisateur. En outre, l’API d’éléments a prise en charge avancée pour la définition déclarative via JSON, ce qui permet de déclaration extrêmement rapide, ainsi que la génération dynamique à partir d’un serveur de l’interface utilisateur. 
-   **API de réflexion principales** : également connu sous le *liaison**API* , dans les classes qui sont annotés avec des indicateurs de l’interface utilisateur, puis MT. D automatiquement crée des écrans basés sur les objets et fournit une liaison entre ce qui est affichée (et éventuellement modifiée) sur l’écran, et l’objet sous-jacent de stockage.   L’exemple ci-dessus illustre l’utilisation de l’API de réflexion. Cette API ne fournit pas le contrôle précis que les API d’éléments, mais elle réduit la complexité encore davantage par la création automatique de la hiérarchie des éléments en fonction des attributs de classe. 


MT. D est compressé avec un grand ensemble de générées dans les éléments d’interface utilisateur pour la création de l’écran, mais il reconnaît également la nécessité d’éléments personnalisés et des dispositions d’écran Avancé. Par conséquent, l’extensibilité est qu'une première classe proposées cuite dans l’API. Les développeurs peuvent étendre les éléments existants ou créer de nouveaux et puis s’intégrer sans problème.

En outre, MT. D a un nombre de fonctionnalités UX courantes iOS intégrées telles que « tirer pour actualiser » prend en charge, d’image asynchrone du chargement et recherche la prise en charge.

Cet article prendra une vision détaillée de travailler avec MT. D, y compris :

-   **MT. Les composants D** : cela se concentrera sur la compréhension des classes qui composent MT. D pour activer la mise en route rapidement opérationnel. 
-   **Référence des éléments** : une liste complète des éléments intégrés de MT. D. 
-   **L’utilisation avancée** : Ceci concerne les fonctionnalités avancées telles que tirer pour actualiser, recherche, chargement de l’image d’arrière-plan, à l’aide de LINQ pour construire des hiérarchies de l’élément et la création des éléments personnalisés, des cellules, et les contrôleurs pour utilisent avec MT. D. 

## <a name="understanding-the-pieces-of-mtd"></a>Comprendre les éléments de MT. D

Même si vous utilisez la réflexion API, MT. D crée une hiérarchie d’éléments dans les coulisses, comme s’il a été créé via l’API d’éléments directement. En outre, la prise en charge JSON mentionné dans la section précédente crée également des éléments. Pour cette raison, il est important d’avoir assimilé les éléments de MT. D.

MT. D génère des écrans à l’aide de quatre parties suivantes :

-  **DialogViewController**
-  **RootElement**
-  **Section**
-  **Élément**


### <a name="dialogviewcontroller"></a>DialogViewController

A *DialogViewController*, ou *DVC* pour faire court, hérite `UITableViewController` et par conséquent représente un écran avec une table. Gérer peut être placé sur un contrôleur de navigation comme une régulier UITableViewController.

### <a name="rootelement"></a>RootElement

A *RootElement* est le conteneur de niveau supérieur pour les éléments dans un DVC. Il contient des Sections, qui peuvent contenir des éléments. RootElements ne sont pas rendus ; au lieu de cela, ils sont simplement des conteneurs pour ce qui en fait est restitué. Un RootElement est attribuée à un DVC, et le DVC restitue ensuite ses enfants.

### <a name="section"></a>Section

Une section est un groupe de cellules dans une table. Comme avec une section de la table normale, elle peut éventuellement comporter un en-tête et un pied de page peut être du texte ou des affichages personnalisés, comme dans la capture d’écran suivante :

 [ ![](images/image2.png "Comme avec une section de la table normale, elle peut éventuellement comporter un en-tête et un pied de page peut être du texte ou des affichages personnalisés, comme dans cette capture d’écran")](images/image2.png)

### <a name="element"></a>Élément

Un élément représente une réelle de la cellule dans la table. MT. D est compressé avec une grande variété d’éléments qui représentent les différents types de données ou des entrées différentes. Par exemple, les captures d’écran suivantes illustrent certaines des éléments disponibles :

 [ ![](images/image3.png "Par exemple, cette captures d’écran illustrent certaines des éléments disponibles")](images/image3.png)

## <a name="more-on-sections-and-rootelements"></a>Plusieurs Sections et RootElements

Nous allons maintenant aborder RootElements et des Sections plus en détail.

### <a name="rootelements"></a>RootElements

RootElement au moins est requis pour démarrer le processus MonoTouch.Dialog.

Si un RootElement est initialisé avec une valeur de l’élément de la section cette valeur est utilisée pour localiser un élément qui fournit un résumé de la configuration, ce qui est rendu sur le côté droit de l’affichage enfant. Par exemple, la capture d’écran ci-dessous montre une table sur la gauche avec une cellule qui contient le titre de l’écran de détails sur la droite, « Dessert », ainsi que la valeur de la desert sélectionné.

 [ ![](images/image4.png "Cette capture d’écran montre une table sur la gauche avec une cellule qui contient le titre de l’écran de détails sur la droite, Dessert, ainsi que la valeur de la desert sélectionné") ](images/image4.png) [ ![ ] (images/image5.png "cela capture d’écran ci-dessous illustre une table de gauche avec une cellule qui contient le titre de l’écran de détails sur la droite, Dessert, ainsi que la valeur de la desert sélectionné")](images/image5.png)

Éléments racine peuvent également servir à l’intérieur des Sections pour déclencher le chargement d’une nouvelle page de configuration imbriqués, comme indiqué ci-dessus. Lorsqu’il est utilisé dans ce mode la légende fournie est utilisée pendant le rendu à l’intérieur d’une section et est également utilisée comme titre pour la sous-page. Exemple :

```csharp
var root = new RootElement ("Meals") {
    new Section ("Dinner"){
            new RootElement ("Dessert", new RadioGroup ("dessert", 2)) {
                new Section () {
                    new RadioElement ("Ice Cream", "dessert"),
                    new RadioElement ("Milkshake", "dessert"),
                    new RadioElement ("Chocolate Cake", "dessert")
                }
            }
        }
    }
```

Dans l’exemple ci-dessus, lorsque l’utilisateur appuie sur « Table », MonoTouch.Dialog sera créer une nouvelle page et y accéder avec la racine est « Dessert » et avoir un groupe de cases d’option avec trois valeurs.

Dans cet exemple particulier, le groupe de cases d’option sélectionne « Gâteau au chocolat » dans la section « Table », car nous avons transmis à la valeur « 2 » pour le RadioGroup. Cela signifie choisir l’élément 3 dans la liste (index zéro).

Appel de la méthode Add ou à l’aide de la syntaxe de l’initialiseur c# 4 ajoute des sections.
Les méthodes d’insertion sont fournies pour insérer des sections avec une animation.

Si vous créez le RootElement avec une instance de groupe (au lieu d’un RadioGroup) la valeur de synthèse de RootElement lorsque affichés dans une Section sera le nombre cumulé de tous les BooleanElements et CheckboxElements qui ont la même clé que la valeur Group.Key.

### <a name="sections"></a>Sections

Sections sont utilisées pour regrouper des éléments dans l’écran et sont les enfants directs valides uniquement de RootElement. Sections peuvent contenir un des éléments standards, y compris les nouvelles RootElements.

RootElements incorporés dans une section sont utilisés pour accéder à un niveau plus profond.

Sections peuvent avoir des en-têtes et pieds de page sous forme de chaînes, ou en tant que UIViews.
En général, vous utiliserez uniquement les chaînes, mais pour créer des interfaces utilisateur personnalisées, vous pouvez utiliser n’importe quel UIView en tant que l’en-tête ou le pied de page. Vous pouvez utiliser une chaîne de procéder comme suit :

```csharp
var section = new Section ("Header", "Footer")
```

Pour utiliser les vues, transmettez simplement les vues au constructeur :

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>Recevoir une notification

#### <a name="handling-nsaction"></a>La gestion des NSAction

MT. Les surfaces D une `NSAction` en tant que délégué pour gérer les rappels.
Par exemple, que vous souhaitez gérer un événement tactile pour une cellule de table créée par MT. D. Lors de la création d’un élément avec MT. D, fournissez simplement un rappel de fonction, comme indiqué ci-dessous :

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>Valeur de l’élément lors de la récupération

Combiné avec le `Element.Value` propriété, le rappel peut récupérer la valeur définie dans d’autres éléments. Considérons par exemple le code suivant :

```csharp
var element = new EntryElement (task.Name, "Enter task description",
        task.Description);
                
var taskElement = new RootElement (task.Name){
        new Section () { element },
        new Section () { 
                new DateElement ("Due Date", task.DueDate)
        },
        new Section ("Demo Retrieving Element Value") {
                new StringElement ("Output Task Description", 
                        delegate { Console.WriteLine (element.Value); })
        }
};
```

Ce code crée une interface utilisateur, comme indiqué ci-dessous. Pour obtenir une description complète de cet exemple, consultez la [API de procédure pas à pas les éléments](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) didacticiel.

 [ ![](images/image6.png "Combiné avec la propriété Element.Value, le rappel peut récupérer la valeur définie dans d’autres éléments")](images/image6.png)

Lorsque l’utilisateur appuie sur la cellule inférieure du tableau, le code dans la fonction anonyme s’exécute, en écrivant la valeur à partir de la `element` d’instance pour le **sortie de l’Application** pavé dans Visual Studio pour Mac.

## <a name="built-in-elements"></a>Éléments intégrés

MT. D est fourni avec un nombre d’éléments de cellule de tableau prédéfinis connu en tant qu’éléments.
Ces éléments sont utilisés pour afficher une variété de types différents dans les cellules de table tels que des chaînes, nombres à virgule flottante, dates et même des images, citer que quelques. Chaque élément prend en charge d’afficher le type de données en conséquence. Par exemple, un élément booléen affichera un commutateur pour activer/désactiver sa valeur. De même, un élément de type float affichera un curseur pour modifier la valeur float.

Il existe des éléments plus complexes pour prendre en charge les types de données plus riches comme des images et html. Par exemple, un élément html, qui ouvre un UIWebView pour charger une page web lorsque sélectionné, affiche une légende dans la cellule du tableau.

### <a name="working-with-element-values"></a>Utilisation des valeurs d’élément

Les éléments qui sont utilisés pour capturer l’entrée d’utilisateur exposent un public `Value` propriété qui contient la valeur actuelle de l’élément à tout moment. Il est automatiquement mis à jour l’utilisateur utilise l’application.

C’est le comportement de tous les éléments qui font partie de MonoTouch.Dialog, mais il n’est pas requis pour les éléments créés par l’utilisateur.

### <a name="string-element"></a>Élément de chaîne

A `StringElement` montre une légende sur le côté gauche d’une cellule de tableau et la valeur de chaîne sur le côté droit de la cellule.

 [ ![](images/image7.png "Un StringElement montre une légende sur le côté gauche d’une cellule de tableau et la valeur de chaîne sur le côté droit de la cellule")](images/image7.png)

Pour utiliser un `StringElement` comme un bouton, fournissez un délégué.

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [ ![](images/image8.png "Pour utiliser un StringElement comme un bouton, fournissez un délégué")](images/image8.png)

### <a name="styled-string-element"></a>Élément de chaîne de style

A `StyledStringElement` permet de chaînes à être présentés à l’aide de deux styles de cellules de tableau prédéfinis ou avec mise en forme personnalisée.

 [ ![](images/image9.png "Un StyledStringElement permet de chaînes à être présentés à l’aide de deux styles de cellules de tableau prédéfinis ou avec mise en forme personnalisée")](images/image9.png)

Le `StyledStringElement` dérive de la classe `StringElement`, mais permet aux développeurs personnaliser un certain nombre de propriétés, telles que la police, la couleur du texte, couleur de cellule, mode de saut de ligne, le nombre de lignes à afficher, et indique si un accessoire doit être affiché.

### <a name="multiline-element"></a>Élément multiligne

 [ ![](images/image10.png "Élément multiligne")](images/image10.png)

### <a name="entry-element"></a>Élément d’entrée

Le `EntryElement`, comme le nom l’indique, est utilisée pour obtenir l’entrée d’utilisateur. Il prend en charge les chaînes ordinaires ou mots de passe, où les caractères sont masqués.

 [ ![](images/image11.png "Le EntryElement est utilisée pour obtenir l’entrée d’utilisateur")](images/image11.png)

Il est initialisé avec trois valeurs :

-  La légende pour l’entrée qui sera affichée à l’utilisateur.
-  Texte d’espace réservé (c’est le texte de sortie grisée qui fournit une indication à l’utilisateur). 
-  La valeur du texte.


L’espace réservé et la valeur peuvent être null. Toutefois, la légende est requise.

À tout moment, l’accès à sa valeur de propriété peut récupérer la valeur de la `EntryElement`.

En outre le `KeyboardType` propriété peut être définie au moment de la création du style de type clavier souhaité pour l’entrée de données. Cela peut servir à configurer le clavier en utilisant les valeurs de `UIKeyboardType` comme indiqué ci-dessous :

-  Numérique
-  Téléphone
-  URL
-  Messagerie


### <a name="boolean-element"></a>Élément booléen

 [ ![](images/image12.png "Élément booléen")](images/image12.png)

### <a name="checkbox-element"></a>Élément de case à cocher

 [ ![](images/image13.png "Élément de case à cocher")](images/image13.png)

### <a name="radio-element"></a>Élément de case d’option

A `RadioElement` nécessite un `RadioGroup` doit être spécifié dans le `RootElement`.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [ ![](images/image14.png "Un RadioElement nécessite un RadioGroup dans RootElement")](images/image14.png)

 `RootElements` sont également utilisés pour coordonner les éléments de la case d’option. Le `RadioElement` membres peuvent s’étendre sur plusieurs Sections (par exemple pour implémenter quelque chose de similaire au sélecteur de ton anneau et distincts de sonneries personnalisées à partir du système sonneries). La vue Résumé affiche l’élément de case d’option qui est actuellement sélectionné. Pour l’utiliser, vous devez créer le `RootElement` avec le constructeur de groupe, comme suit :

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

Le nom du groupe dans `RadioGroup` est utilisé pour déterminer la valeur sélectionnée dans la page conteneur (le cas échéant) et la valeur, qui est égal à zéro dans ce cas, l’index du premier élément sélectionné.

### <a name="badge-element"></a>Élément de badge

 [ ![](images/image15.png "Élément de badge")](images/image15.png)

### <a name="float-element"></a>Élément de type float

 [ ![](images/image16.png "Élément de type float")](images/image16.png)

### <a name="activity-element"></a>Élément d’activité

 [ ![](images/image17.png "Élément d’activité")](images/image17.png)

### <a name="date-element"></a>Élément de date

 ![](images/image18.png "Élément de date")

Lorsque la cellule correspondant à la DateElement est sélectionnée, un sélecteur de dates est présenté comme indiqué ci-dessous :

 [ ![](images/image19.png "Lorsque la cellule correspondant à la DateElement est sélectionnée, un sélecteur de dates est présenté comme")](images/image19.png)

### <a name="time-element"></a>Élément Time

 [ ![](images/image20.png "Élément Time")](images/image20.png)

Lorsque la cellule correspondant à la TimeElement est sélectionnée, un sélecteur de temps est présenté comme indiqué ci-dessous :

 [ ![](images/image21.png "Lorsque la cellule correspondant à la TimeElement est sélectionnée, un sélecteur de temps est présenté comme")](images/image21.png)

### <a name="datetime-element"></a>Élément de date/heure

 [ ![](images/image22.png "Élément de date/heure")](images/image22.png)

Lorsque la cellule correspondant à la DateTimeElement est sélectionnée, un sélecteur de date/heure est présenté comme indiqué ci-dessous :

 [ ![](images/image23.png "Lorsque la cellule correspondant à la DateTimeElement est sélectionnée, un sélecteur de date/heure est présenté comme")](images/image23.png)

### <a name="html-element"></a>Élément HTML

 [ ![](images/image24.png "Élément HTML")](images/image24.png)

Le `HTMLElement` affiche la valeur de son `Caption` propriété dans la cellule du tableau. Lorsqu’il sélectionnée, le `Url` assignée à l’élément est chargé dans un `UIWebView` contrôler comme indiqué ci-dessous :

 [ ![](images/image25.png "Lorsqu’il est sélectionné, l’Url attribuée à l’élément est chargé dans un contrôle UIWebView comme indiqué ci-dessous")](images/image25.png)

### <a name="message-element"></a>Élément message

 [ ![](images/image26.png "Élément message")](images/image26.png)

### <a name="load-more-element"></a>Charger plus d’élément

Utilisez cet élément pour permettre aux utilisateurs de charger les éléments plus dans votre liste. Vous pouvez personnaliser la normale et légendes de chargement, ainsi que la police et couleur du texte.
Le `UIActivity` indicateur démarre l’animation, et la légende de chargement s’affiche quand un utilisateur appuie sur la cellule, puis le `NSAction` passé dans le constructeur est exécuté. Une fois votre code dans le `NSAction` est terminé, le `UIActivity` indicateur s’arrête l’animation et la légende normale s’affiche de nouveau.

### <a name="uiview-element"></a>Élément de UIView

En outre, personnalisés `UIView` peuvent être affichés à l’aide de la `UIViewElement`.

### <a name="owner-drawn-element"></a>Élément de Owner-Drawn

Cet élément doit être sous-classé car il s’agit d’une classe abstraite. Vous devez substituer la `Height(RectangleF bounds)` méthode dans laquelle vous devez retourner la hauteur de l’élément, ainsi que `Draw(RectangleF bounds, CGContext context, UIView view)` dans qui vous devez effectuer toutes les votre dessin personnalisé dans les limites donnés, en utilisant les paramètres de contexte et de la vue. Cet élément est l’essentiel de sous-classement un `UIView`et le placer dans la cellule à retourner, et vous vous retrouvez n’ayant besoin d’implémenter deux remplacements simples. Vous pouvez voir un exemple d’implémentation meilleures dans l’exemple d’application dans le `DemoOwnerDrawnElement.cs` fichier.

Voici un exemple très simple d’implémentation de la classe :

```csharp
public class SampleOwnerDrawnElement : OwnerDrawnElement
 {
    public SampleOwnerDrawnElement (string text) : base(UITableViewCellStyle.Default, "sampleOwnerDrawnElement")
    {
        this.Text = text;
    }

    public string Text
    {
        get;set;    
    }

    public override void Draw (RectangleF bounds, CGContext context, UIView view)
    {
        UIColor.White.SetFill();
        context.FillRect(bounds);

        UIColor.Black.SetColor();   
        view.DrawString(this.Text, new RectangleF(10, 15, bounds.Width - 20, bounds.Height - 30), UIFont.BoldSystemFontOfSize(14.0f), UILineBreakMode.TailTruncation);
    }

    public override float Height (RectangleF bounds)
    {
        return 44.0f;
    }
 }
```

### <a name="json-element"></a>Élément JSON

Le `JsonElement` est une sous-classe de `RootElement` qui étend un `RootElement` pour être en mesure de charger le contenu de l’enfant imbriqué à partir d’une url locale ou distante.

Le `JsonElement` est un `RootElement` qui peut être instancié sous deux formes. Une version crée un `RootElement` qui charge le contenu à la demande. Ceux-ci sont créés à l’aide de la `JsonElement` constructeurs qui acceptent un argument supplémentaire à la fin, l’url à charger le contenu à partir de :

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

L’autre formulaire crée les données à partir d’un fichier local ou un `System.Json.JsonObject` que vous avez déjà analysé :

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

Pour plus d’informations sur l’utilisation de JSON avec MT. D, consultez le [procédure pas à pas de JSON élément](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) didacticiel.

## <a name="other-features"></a>Autres fonctionnalités

### <a name="pull-to-refresh-support"></a>Prise en charge de l’actualisation de l’extraction

 *Extraire-à-* *Actualiser* un effet visuel se trouve à l’origine dans le *Tweetie2* application, qui est devenue un effet populaires entre plusieurs applications.

Pour ajouter une prise en charge par extraction à l’actualisation automatique à vos boîtes de dialogue, il vous suffit de faire deux choses : raccorder un gestionnaire d’événements pour être averti lorsque l’utilisateur extrait les données et d’avertir le `DialogViewController` lorsque les données ont été chargées pour revenir à son état par défaut.

Raccorder une notification est simple ; simplement se connecter à la `RefreshRequested` événement sur le `DialogViewController`, comme suit :

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

Ensuite, dans votre méthode `OnUserRequestedRefresh`, vous souhaitez le chargement de certaines données de la file d’attente, demander des données à partir d’Internet ou lancer un thread pour calculer les données. Une fois que les données ont été chargées, vous devez informer les `DialogViewController` les nouvelles données, et pour restaurer la vue dans son état par défaut, cela en appelant `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>Prise en charge de la recherche

Pour prendre en charge la recherche, définissez la `EnableSearch` propriété sur votre `DialogViewController`. Vous pouvez également définir le `SearchPlaceholder` propriété à utiliser comme du texte de filigrane dans la barre de recherche.

Recherche modifiera le contenu de la vue en tant que types de l’utilisateur. Il recherche les champs visibles et indique celles utilisées pour l’utilisateur. Le `DialogViewController` expose trois méthodes pour lancer, arrêter ou déclencher une nouvelle opération de filtre sur les résultats par programme. Ces méthodes sont répertoriées ci-dessous :

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


Le système est extensible, vous pouvez modifier ce comportement si vous le souhaitez.

### <a name="background-image-loading"></a>Chargement de l’Image d’arrière-plan

MonoTouch.Dialog incorpore le [TweetStation](https://github.com/migueldeicaza/TweetStation) le chargeur d’image de l’application. Le chargeur d’image peut être utilisé pour charger des images en arrière-plan, prend en charge la mise en cache et peut informer votre code lorsque l’image a été chargée.

Il sera également limiter le nombre de connexions réseau sortantes.

Le chargeur d’image est implémenté dans le `ImageLoader` (classe), vous devez simplement est l’appel de la `DefaultRequestImage` (méthode), vous devez fournir l’Uri pour l’image que vous souhaitez charger, ainsi que d’une instance de la `IImageUpdated` interface qui sera appelé quand l’image à haute disponibilité s été chargé.

Par exemple le code suivant charge une image à partir d’une Url dans un `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

La classe ImageLoader expose une méthode de Purge que vous pouvez appeler lorsque vous souhaitez libérer toutes les images qui sont actuellement mis en cache en mémoire. Le code actuel a un cache pour les images de 50. Si vous souhaitez utiliser une taille de cache différents (par exemple, si vous attendez les images trop importante pour que les 50 images serait trop), vous pouvez uniquement créer des instances de ImageLoader et passer le nombre d’images que vous souhaitez conserver dans le cache.

## <a name="using-linq-to-create-element-hierarchy"></a>Pour créer la hiérarchie des éléments à l’aide de LINQ

Via l’utilisation intelligente de la syntaxe d’initialisation LINQ et C#, LINQ peut être utilisé pour créer une hiérarchie de l’élément. Par exemple, le code suivant crée un écran à partir des tableaux de chaînes et gère la cellule sélection via une fonction anonyme qui a été passée dans chaque `StringElement`:

```csharp
var rootElement = new RootElement ("LINQ root element") {
from x in new string [] { "one", "two", "three" }
select new Section (x) {
from y in "Hello:World".Split (':')
select (Element) new StringElement (y,
delegate { Debug.WriteLine("cell tapped"); })
}
};
```

Il peut facilement être associé à un magasin de données XML ou des données à partir d’une base de données pour créer des applications complexes presque entièrement à partir des données.

## <a name="extending-mtd"></a>Extension MT. D

### <a name="creating-custom-elements"></a>Création d’éléments personnalisés

Vous pouvez créer votre propre élément par héritage à partir d’un élément existant ou en dérivant de la classe d’élément racine.

Pour créer votre propre élément, vous devez substituer les méthodes suivantes :

```csharp
// To release any heavy resources that you might have
    void Dispose (bool disposing);

    // To retrieve the UITableViewCell for your element
    // you would need to prepare the cell to be reused, in the
    // same way that UITableView expects reusable cells to work
    UITableViewCell GetCell (UITableView tv)

    // To retrieve a "summary" that can be used with
    // a root element to render a summary one level up.  
    string Summary ()
    // To detect when the user has tapped on the cell
    void Selected (DialogViewController dvc, UITableView tableView, NSIndexPath path)
    // If you support search, to probe if the cell matches the user input
    bool Matches (string text)
```

Si votre élément peut avoir une taille variable, vous devez implémenter la `IElementSizing` interface, qui contient une méthode :

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

Si vous envisagez d’implémenter votre `GetCell` en appelant `base.GetCell(tv)` et personnalisation de la cellule renvoyée, vous devez également substituer la `CellKey` propriété à retourner une clé qui sera unique à votre élément, comme suit :

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

Cela fonctionne pour la plupart des éléments, mais pas pour le `StringElement` et `StyledStringElement` comme ceux utilisent leur propre jeu de clés pour différents scénarios de rendu. Vous devez répliquer le code dans ces classes.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (gérer)

La réflexion et l’API d’éléments utilisent le même `DialogViewController`. Parfois, vous souhaitez personnaliser l’apparence de la vue ou vous souhaiterez peut-être utiliser certaines fonctionnalités de la `UITableViewController` qui aller au-delà de la création d’interfaces utilisateur de base.

Le `DialogViewController` est simplement une sous-classe de la `UITableViewController` et vous pouvez le personnaliser de la même façon que vous personnalisez un `UITableViewController`.

Par exemple, si vous souhaitez modifier le style de la liste soit `Grouped` ou `Plain`, vous pouvez définir cette valeur en modifiant la propriété lorsque vous créez le contrôleur, comme suit :

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

Pour plus d’informations avancées sur les personnalisations de la `DialogViewController`, telles que la définition de son arrière-plan, vous le feriez sous-classe il et que vous remplacez la bonne méthodes, comme indiqué dans l’exemple ci-dessous :

```csharp
class SpiffyDialogViewController : DialogViewController {
    UIImage image;

    public SpiffyDialogViewController (RootElement root, bool pushing, UIImage image) 
        : base (root, pushing) 
    {
        this.image = image;
    }

    public override LoadView ()
    {
        base.LoadView ();
        var color = UIColor.FromPatternImage(image);
        TableView.BackgroundColor = UIColor.Clear;
        ParentViewController.View.BackgroundColor = color;
    }
}
```

Un autre point de personnalisation est les méthodes virtuelles suivantes dans le `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

Cette méthode doit retourner une sous-classe de `DialogViewController.Source` pour les cas où votre cellules de tailles égales ou une sous-classe de `DialogViewController.SizingSource` si vos cellules sont inégaux.

Ce remplacement permet de capturer de la `UITableViewSource` méthodes. Par exemple, [TweetStation](https://github.com/migueldeicaza/TweetStation) utilise pour effectuer le suivi lorsque l’utilisateur a l’objet d’un défilement vers le haut et mettre à jour en conséquence le nombre de tweets non lus.

## <a name="validation"></a>Validation

Éléments ne fournissent pas la validation eux-mêmes en tant que les modèles sont bien adaptés pour les pages web et applications de bureau ne correspondent pas directement sur le modèle d’interaction iPhone.

Si vous souhaitez effectuer une validation de données, vous devez le faire lorsque l’utilisateur déclenche une action avec les données entrées. Par exemple un <span class="ui">fait</span> ou <span class="ui">suivant</span> bouton sur la barre d’outils supérieure, ou certaines `StringElement` utilisé comme un bouton pour accéder à l’étape suivante.

Il s’agit d’exécuter une validation d’entrée de base, alors peut-être plus compliquée validation vérifie la validité d’une combinaison utilisateur/mot de passe avec un serveur.

Comment vous informer l’utilisateur d’une erreur est spécifique à l’application. Vous pouvez afficher un `UIAlertView` ou afficher un indicateur.

## <a name="summary"></a>Récapitulatif

Cet article couvert beaucoup d’informations sur MonoTouch.Dialog. Elle décrit les principes fondamentaux de la procédure MT. D fonctionne et couvert les différents composants qui composent MT. D. Il a également montré la large gamme d’éléments et les personnalisations de table pris en charge par MT. D et décrit comment MT. D peut être étendu avec des éléments personnalisés. En outre, elle expliquée la prise en charge JSON dans Mt. D qui permet de créer dynamiquement des éléments à partir de JSON.


## <a name="related-links"></a>Liens associés

- [Capture - Miguel de Icaza crée un écran de connexion iOS avec MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Capture - créer facilement des interfaces utilisateur iOS avec MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Procédure pas à pas : création d’une application à l’aide de l’API Elements](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Procédure pas à pas : création d’une application à l’aide de l’API Reflection](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Procédure pas à pas : utilisation d’un élément JSON pour créer une interface utilisateur](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Balisage de MonoTouch.Dialog JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Boîte de dialogue MonoTouch sur Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Référence de classe de UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Référence de classe de UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
