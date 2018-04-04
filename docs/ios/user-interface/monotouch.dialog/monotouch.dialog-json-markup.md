---
title: MonoTouch.Dialog Json Markup
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 02c95f06571a3c242481769846d7cab51450f2ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="monotouchdialog-json-markup"></a>MonoTouch.Dialog Json Markup

Cette page décrit le balisage Json accepté par MonoTouch.Dialog [JsonElement](https://developer.xamarin.com/api/type/MonoTouch.Dialog.JsonElement/)

Commençons par un exemple. Voici un fichier Json complet qui peut être passé dans JsonElement.

```csharp
{     
  "title": "Json Sample",
  "sections": [ 
      {
          "header": "Booleans",
          "footer": "Slider or image-based",
          "id": "first-section",
          "elements": [
              { 
                  "type" : "boolean",
                  "caption" : "Demo of a Boolean",
                  "value"   : true
              }, {
                  "type": "boolean",
                  "caption" : "Boolean using images",
                  "value"   : false,
                  "on"      : "favorite.png",
                  "off"     : "~/favorited.png"
              }, {
                      "type": "root",
                      "title": "Tap for nested controller",
                      "sections": [ {
                         "header": "Nested view!",
                         "elements": [
                           {
                             "type": "boolean",
                             "caption": "Just a boolean",
                             "id": "the-boolean",
                             "value": false
                           },
                           {
                             "type": "string",
                             "caption": "Welcome to the nested controller"
                           }
                         ]
                       }
                     ]
                   }
          ]
      }, {
          "header": "Entries",
          "elements" : [
              {
                  "type": "entry",
                  "caption": "Username",
                  "value": "",
                  "placeholder": "Your account username"
              }
          ]
      }
  ]
}
```

Le balisage ci-dessus génère l’interface utilisateur suivant :

 [![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "L’interface utilisateur créé par le balisage donné")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png#lightbox)

Chaque élément dans l’arborescence peut contenir la propriété `"id"`. Il est possible lors de l’exécution pour référencer les sections individuelles ou des éléments à l’aide de l’indexeur JsonElement. Comme ceci :

```csharp
var jsonElement = JsonElement.FromFile ("demo.json");

var firstSection = jsonElement ["first-section"] as Section;

var theBoolean = jsonElement ["the-boolean"] as BooleanElement
```

 <a name="Root_Element_Syntax" />


## <a name="root-element-syntax"></a>Syntaxe d’élément racine

L’élément racine contient les valeurs suivantes :

-  `title`
-  `sections`(facultatif)


L’élément racine peut apparaître que dans une section en tant qu’élément pour créer un contrôleur imbriqué. Dans ce cas, la propriété supplémentaire `"type"` doit être défini sur `"root"`

 <a name="url" />


### <a name="url"></a>url

Si le `"url"` propriété est définie, si l’utilisateur appuie sur cette RootElement, le code demande un fichier à partir de l’url spécifiée et permettront le contenu affiche les nouvelles informations. Vous pouvez l’utiliser pour créer étendre l’interface utilisateur à partir du serveur en fonction de ce que l’utilisateur appuie sur.

 <a name="group" />


### <a name="group"></a>groupe

Si la valeur, cela définit le nom de groupe pour l’élément racine. Les noms de groupe sont utilisés pour sélectionner un résumé s’affiche en tant que la valeur de l’élément racine à partir d’un des éléments imbriqués dans l’élément. Ceci est la valeur d’une case à cocher ou la valeur d’une case d’option.

 <a name="radioselected" />


### <a name="radioselected"></a>radioselected

Identifie l’élément de case d’option est sélectionnée dans les éléments imbriqués

 <a name="title" />


### <a name="title"></a>titre

S’il est présent, il sera le titre utilisé pour RootElement

 <a name="type" />


### <a name="type"></a>type

Doit avoir la valeur `"root"` lorsque cela s’affiche dans une section (cela est utilisé pour imbriquer des contrôleurs).

 <a name="sections" />


### <a name="sections"></a>sections

Il s’agit d’un tableau Json avec les sections individuelles

 <a name="Section_Syntax" />


## <a name="section-syntax"></a>Syntaxe de la section

La section contient :

-  `header`(facultatif)
-  `footer`(facultatif)
-  Tableau `elements`


 <a name="header" />


### <a name="header"></a>en-tête

Le cas échéant, le texte d’en-tête s’affiche en tant que légende de la section.

 <a name="footer" />


### <a name="footer"></a>Pied de page

Le cas échéant, le pied de page s’affiche en bas de la section.

 <a name="elements" />


### <a name="elements"></a>éléments

Il s’agit d’un tableau d’éléments. Chaque élément doit contenir au moins une clé, le `"type"` clé qui est utilisé pour identifier le type d’élément à créer.
Certains éléments partagent certaines propriétés communes comme `"caption"` et `"value"`. Voici la liste des éléments pris en charge :

-  `string` éléments (à la fois avec et sans style)
-  `entry` lignes (standard ou un mot de passe)
-  `boolean` valeurs (à l’aide des commutateurs ou des images)


Éléments de chaîne qui peuvent être utilisés en tant que boutons en fournissant une méthode à appeler lorsque l’utilisateur appuie sur la cellule ou de l’accesseur,

 <a name="Rendering_Elements" />


## <a name="rendering-elements"></a>Rendu des éléments

Les rendu des éléments sont basés sur le c# StringElement et StyledStringElement et ils peuvent restituer les informations de différentes manières et il est possible d’effectuer le rendu de différentes manières. Les éléments de la plus simple peuvent être créés comme suit :

```csharp
{
        "type": "string",
        "caption": "Json Serializer",
}
```

Cette opération affiche une chaîne simple avec toutes les valeurs par défaut : police, arrière-plan, couleur du texte et décorations. Il est possible de raccorder des actions à ces éléments et les rendre se comportent comme des boutons en définissant le `"ontap"` propriété ou le `"onaccessorytap"` propriétés :

```csharp
{
    "type":    "string",
        "caption": "View Photos",
        "ontap:    "Acme.PhotoLibrary.ShowPhotos"
}
```

Appelle la méthode « ShowPhotos » dans la classe « Acme.PhotoLibrary » ci-dessus. Le `"onaccessorytap"` est similaire, mais il sera uniquement appelé si l’utilisateur appuie sur l’accesseur au lieu d’en appuyant sur la cellule. Pour ce faire, vous devez également définir l’accessoire :

```csharp
{
    "type":     "string",
        "caption":  "View Photos",
        "ontap:     "Acme.PhotoLibrary.ShowPhotos",
        "accessory: "detail-disclosure",
        "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

Rendu des éléments peut afficher deux chaînes en une seule fois, un est la légende et un autre est la valeur. Comment ces chaînes sont rendus varient selon le style, vous pouvez définir cette option à l’aide de la `"style"` propriété. La valeur par défaut affichera la légende à gauche et la valeur située à droite. De style pour plus d’informations, consultez la section. Couleurs sont codées à l’aide du symbole « # » suivi de nombres hexadécimaux qui représentent les valeurs pour les valeurs de rouges, verts, bleus et alphanumériques peut-être. Le contenu peut être encodé sous la forme abrégée (3 ou 4 des chiffres hexadécimaux) qui représente les valeurs RVB ou RVBA. Ou la forme longue (6 ou 8 chiffres) qui représentent les valeurs RVB ou RVBA. La version courte est un raccourci pour l’écriture de la même chiffre hexadécimal à deux reprises. Par conséquent, la constante « #1bc » est interprétés en rouge = 0 x 11, vert = 0xbb et bleu = 0xcc. Si la valeur alpha n’est pas présente, la couleur est opaque. Voici quelques exemples :

```csharp
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory" />


### <a name="accessory"></a>ACCESSOIRE

Détermine le type d’accesseur à afficher dans votre élément de rendu, les valeurs possibles sont :

-  `checkmark`
-  `detail-disclosure`
-  `disclosure-indicator`


Si la valeur n’est pas présente, aucun accessoire n’est indiqué.

 <a name="background" />


### <a name="background"></a>arrière-plan

La propriété d’arrière-plan définit la couleur d’arrière-plan de la cellule. La valeur est soit une URL vers une image (dans ce cas, le Téléchargeur d’image asynchrone est appelé et l’arrière-plan sera mise à jour une fois que l’image est téléchargée) ou il peut être une couleur spécifiée à l’aide de la syntaxe de couleur.

 <a name="caption" />


### <a name="caption"></a>caption

La chaîne principale à afficher sur l’élément de rendu. La police et couleur peuvent être personnalisés en définissant le `"textcolor"` et `"font"` propriétés. Le style de rendu est déterminé par le `"style"` propriété.

 <a name="color_and_detailcolor" />


### <a name="color-and-detailcolor"></a>couleur et detailcolor

La couleur à utiliser pour le texte principal ou le texte détaillé.

 <a name="detailfont_and_font" />


### <a name="detailfont-and-font"></a>DetailFont et la police

La police à utiliser pour la légende ou le texte détaillé. Le format d’une spécification de police est le nom de police suivi éventuellement d’un tiret et de la taille du point.
Spécifications de police valides sont les suivants :

-  "Helvetica"
-  "Helvetica-14"


 <a name="linebreak" />


### <a name="linebreak"></a>linebreak

Détermine la façon dont les lignes sont décomposées. Les valeurs possibles sont :

-  `character-wrap`
-  `clip`
-  `head-truncation`
-  `middle-truncation`
-  `tail-truncation`
-  `word-wrap`


Les deux `character-wrap` et `word-wrap` peut être utilisé avec le `"lines"` propriété est définie sur zéro pour activer l’élément de rendu dans un élément de plusieurs lignes.

 <a name="ontap_and_onaccessorytap" />


### <a name="ontap-and-onaccessorytap"></a>ONTAP et onaccessorytap

Ces propriétés doivent pointer vers un nom de méthode statique qui prend un objet en tant que paramètre dans votre application. Lorsque vous créez votre hiérarchie à l’aide des méthodes JsonDialog.FromFile ou JsonDialog.FromJson, vous pouvez passer une valeur d’objet facultatif. Valeur de cet objet est ensuite passée à vos méthodes. Cela permet de passer un contexte à votre méthode statique. Par exemple :

```csharp
class Foo {
    Foo ()
    {
        root = JsonDialog.FromJson (myJson, this);
    }

    static void Callback (object obj)
    {
        Foo myFoo = (Foo) obj;
        obj.Callback ();
    }
}
```

 <a name="lines" />


### <a name="lines"></a>lignes

Si cela est définie à zéro, il effectue la taille automatique à un élément selon le contenu des chaînes contenues. Pour ce faire, vous devez également définir le `"linebreak"` propriété `"character-wrap"` ou `"word-wrap"`.

 <a name="style" />


### <a name="style"></a>style

Le style détermine le type de style de cellule qui permet de restituer le contenu et qu’ils correspondent aux valeurs d’énumération UITableViewCellStyle.
Les valeurs possibles sont :

-  `"default"`
-  `"value1"`
-  `"value2"`
-  `"subtitle"` : texte avec un sous-titre.


 <a name="subtitle" />


### <a name="subtitle"></a>Sous-titre

La valeur à utiliser pour le sous-titre. Il s’agit d’un raccourci pour définir le style `"subtitle"` et définir le `"value"` propriété dans une chaîne.
Cela à la fois avec une seule entrée.

 <a name="textcolor" />


### <a name="textcolor"></a>TextColor

La couleur à utiliser pour le texte.

 <a name="value" />


### <a name="value"></a>par défaut

La valeur secondaire à afficher sur l’élément de rendu. La disposition de ce qui est affectée par la `"style"` paramètre. La police et couleur peuvent être personnalisés en définissant le `"detailfont"` et `"detailcolor"`.

 <a name="Boolean_Elements" />


## <a name="boolean-elements"></a>Éléments booléennes

Éléments booléennes doivent définir le type sur `"bool"`, peut contenir un `"caption"` pour afficher et `"value"` est définie sur true ou false. Si le `"on"` et `"off"` propriétés sont définies, elles sont supposées être des images. Les images sont résolus par rapport au répertoire de travail actuel dans l’application. Si vous souhaitez référencer relatifs à un groupe de fichiers, vous pouvez utiliser la `"~"` comme raccourci pour représenter le répertoire de l’offre groupée d’application. Par exemple `"~/favorite.png"` sera le favorite.png se trouvant dans le fichier de regroupement. Par exemple :

```csharp
{ 
    "type" : "boolean",
    "caption" : "Demo of a Boolean",
    "value"   : true
},

{
    "type": "boolean",
    "caption" : "Boolean using images",
    "value"   : false,
    "on"      : "favorite.png",
    "off"     : "~/favorited.png"
}
```

 <a name="type" />


### <a name="type"></a>type

Type peut être défini avec la valeur `"boolean"` ou `"checkbox"`. Si la valeur booléenne il utilisera un UISlider ou des images (si les deux `"on"` et `"off"` sont définis). Si la valeur case à cocher, il utilisera une case à cocher. Le `"group"` propriété peut être utilisée pour étiqueter un élément booléen comme appartenant à un groupe particulier. Cela est utile si la conteneur racine possède également un `"group"` la racine de la propriété résume les résultats avec un nombre de toutes les valeurs booléennes (ou les cases à cocher) qui appartiennent au même groupe.

 <a name="Entry_Elements" />


## <a name="entry-elements"></a>Élément d’entrée

Vous utilisez des éléments d’entrée pour autoriser l’utilisateur à entrer des données. Le type de l’élément d’entrée est `"entry"` ou `"password"`. Le `"caption"` propriété est définie pour le texte à afficher sur la droite et le `"value"` est définie à la valeur initiale pour l’entrée de la valeur. Le `"placeholder"` est utilisé pour afficher un indicateur à l’utilisateur pour les entrées vides (il apparaît grisé). Voici quelques exemples :

```csharp
{
        "type": "entry",
        "caption": "Username",
        "value": "",
        "placeholder": "Your account username"
}, {
        "type": "password",
        "caption": "Password",
        "value": "",
        "placeholder": "You password"
}, {
        "type": "entry",
        "caption": "Zip Code",
        "value": "01010",
        "placeholder": "your zip code",
        "keyboard": "numbers"
}, {
        "type": "entry",
        "return-key": "route",
        "caption": "Entry with 'route'",
        "placeholder": "captialization all + no corrections",
        "capitalization": "all",
        "autocorrect": "no"
}
```

 <a name="autocorrect" />


### <a name="autocorrect"></a>autocorrect

Détermine le style de correction automatique à utiliser pour l’entrée. Les valeurs possibles sont true ou false (ou les chaînes `"yes"` et `"no"`).

 <a name="capitalization" />


### <a name="capitalization"></a>mise en majuscules

Le style de mise en majuscules à utiliser pour l’entrée. Les valeurs possibles sont :

-  `all`
-  `none`
-  `sentences`
-  `words`


 <a name="caption" />


### <a name="caption"></a>caption

La légende à utiliser pour l’entrée

 <a name="keyboard" />


### <a name="keyboard"></a>clavier

Le type de clavier à utiliser pour l’entrée de données. Les valeurs possibles sont :

-  `ascii`
-  `decimal`
-  `default`
-  `email`
-  `name`
-  `numbers`
-  `numbers-and-punctuation`
-  `twitter`
-  `url`


 <a name="placeholder" />


### <a name="placeholder"></a>Espace réservé

Le texte d’information qui est affiché lorsque l’entrée a une valeur vide.

 <a name="return-key" />


### <a name="return-key"></a>touche de retour

L’étiquette utilisée pour la touche Retour. Les valeurs possibles sont :

-  `default`
-  `done`
-  `emergencycall`
-  `go`
-  `google`
-  `join`
-  `next`
-  `route`
-  `search`
-  `send`
-  `yahoo`


 <a name="value" />


### <a name="value"></a>par défaut

La valeur initiale pour l’entrée

 <a name="Radio_Elements" />


## <a name="radio-elements"></a>Éléments de cases d’option

Type des éléments de case d’option est `"radio"`. L’élément sélectionné est sélectionnée par le `radioselected` propriété sur son élément conteneur de la racine.
En outre, si une valeur est définie pour le `"group"` propriété, cette case d’option appartient à ce groupe.

 <a name="Date_and_Time_Elements" />


## <a name="date-and-time-elements"></a>Date et heure éléments

Les types d’élément `"datetime"`, `"date"` et `"time"` permettent de restituer les dates dont les heures, dates ou heures. Ces éléments prennent comme paramètres une légende et une valeur. La valeur peut être écrite dans n’importe quel format pris en charge par la fonction .NET DateTime.Parse. Exemple :

```csharp
"header": "Dates and Times",
"elements": [
        {
                "type": "datetime",
                "caption": "Date and Time",
                "value": "Sat, 01 Nov 2008 19:35:00 GMT"
        }, {
                "type": "date",
                "caption": "Date",
                "value": "10/10"
        }, {
                "type": "time",
                "caption": "Time",
                "value": "11:23"
                }                       
]
```

 <a name="Html/Web_Element" />


## <a name="htmlweb-element"></a>Élément HTML/Web

Vous pouvez créer une cellule qui lorsque tapées incorpore un UIWebView qui restitue le contenu d’une URL spécifiée, locaux ou distants à l’aide du `"html"` type. Les deux seules propriétés de cet élément sont `"caption"` et `"url"`:

```csharp
{
        "type": "html",
        "caption": "Miguel's blog",
        "url": "http://tirania.org/blog" 
}
```
