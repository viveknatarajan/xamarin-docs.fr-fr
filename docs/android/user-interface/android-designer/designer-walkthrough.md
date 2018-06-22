---
title: À l’aide du concepteur Android
description: Cette rubrique est une procédure pas à pas du Concepteur de Xamarin.Android. Il montre comment créer une interface utilisateur pour une application de navigateur petit couleur ; Cette interface utilisateur est créée entièrement dans le concepteur.
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/10/2018
ms.openlocfilehash: 8d1dc410d5336d9c2505a18720cc7f734e838c39
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798630"
---
# <a name="using-the-android-designer"></a>À l’aide du concepteur Android

_Cette rubrique est une procédure pas à pas du Concepteur de Xamarin.Android. Il montre comment créer une interface utilisateur pour une application de navigateur petit couleur ; Cette interface utilisateur est créée entièrement dans le concepteur._


## <a name="overview"></a>Vue d'ensemble

Interfaces utilisateur Android peuvent être créés de façon déclarative à l’aide de fichiers XML, ou par programme en écrivant du code. Le Concepteur de Xamarin.Android permet aux développeurs de créer et modifier des dispositions déclaratives visuellement, sans avoir à gérer l’aspect fastidieux de modification manuelle des fichiers XML. Le concepteur fournit également des commentaires en temps réel, ce qui permet au développeur d’évaluer les changements de l’interface utilisateur sans avoir à redéployer l’application pour un périphérique ou un émulateur. Cela peut accélérer considérablement le développement de l’interface utilisateur Android. Dans cet article, nous vous présentons une procédure pas à pas qui montre comment utiliser le Concepteur de Xamarin.Android créer visuellement une interface utilisateur.


## <a name="walkthrough"></a>Procédure pas à pas

L’objectif de cette procédure pas à pas est d’utiliser le concepteur Android pour créer une interface utilisateur pour une application de navigateur de couleur exemple qui présente une liste de couleurs, leurs noms et leurs valeurs RVB. Nous allons examiner comment ajouter des widgets à l’aire de conception, ainsi que comment organiser ces widgets visuellement. Après cela, nous allons expliquer comment modifier les widgets de manière interactive sur l’aire de conception ou à l’aide du concepteur **remplissage de la propriété**. Enfin, nous allons voir notre conception aspect lors de l’application est exécutée.

C’est parti !


### <a name="creating-a-new-project"></a>Création d'un projet

La première étape consiste à créer un nouveau projet Xamarin.Android.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Lancez Visual Studio et cliquez sur **nouveau projet...**  puis choisissez le **Visual C\# > Android > application Android (Xamarin)** modèle :

[![Application vide Android](designer-walkthrough-images/vs/01-android-app-sml.w157.png)](designer-walkthrough-images/vs/01-android-app.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Lancez Visual Studio pour Mac et cliquez sur **nouvelle Solution...** . Choisissez le **application Android** modèle et cliquez sur **suivant**:

[![Application vide Android](designer-walkthrough-images/xs/01-android-app-sml.png)](designer-walkthrough-images/xs/01-android-app.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Nommez la nouvelle application **DesignerWalkthrough** et cliquez sur **OK**.

[![Application de nom](designer-walkthrough-images/vs/02-name-app-sml.png)](designer-walkthrough-images/vs/02-name-app.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Nommez la nouvelle application **DesignerWalkthrough**. Sous **plateformes cibles**, sélectionnez **les plus récentes et plus grand** et cliquez sur **suivant**:

[![Application de nom](designer-walkthrough-images/xs/02-designer-walkthrough-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough.png#lightbox)

Dans l’écran suivant de la boîte de dialogue, cliquez sur **créer**.

-----



### <a name="adding-a-layout"></a>Ajout d’une mise en page

Nous allons créer un **LinearLayout** que nous allons utiliser pour contenir des éléments d’interface de nos utilisateurs.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dans Visual Studio, cliquez sur **ressources/disposition** dans les **l’Explorateur de solutions** et sélectionnez **Ajouter > nouvel élément...** . Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **disposition Android**. Nommez le fichier **ListItem.axml** et cliquez sur **ajouter**:

[![Nouvelle disposition](designer-walkthrough-images/vs/03-new-layout-sml.w157.png)](designer-walkthrough-images/vs/03-new-layout.w157.png#lightbox)

La nouvelle **ListItem** disposition s’affiche dans le concepteur :

[![Affichage du Concepteur](designer-walkthrough-images/vs/04-designer-view-sml.png)](designer-walkthrough-images/vs/04-designer-view.png#lightbox)

Cliquez sur le **Source** onglet en bas du concepteur pour afficher la source XML pour cette présentation :

[![Concepteur XML](designer-walkthrough-images/vs/05-designer-xml-sml.png)](designer-walkthrough-images/vs/05-designer-xml.png#lightbox)

À partir de la **vue** menu, cliquez sur **autres fenêtres > structure du Document** pour ouvrir le **structure du Document**. Le **structure du Document** montre que la disposition contienne actuellement un seul **LinearLayout** widget :

[![Structure du document](designer-walkthrough-images/vs/06-document-outline-sml.png)](designer-walkthrough-images/vs/06-document-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Dans Visual Studio pour Mac, cliquez sur **ressources/disposition** dans les **Solution** de remplissage, puis sélectionnez **Ajouter > nouveau fichier...** . Dans le **nouveau fichier** boîte de dialogue, sélectionnez **Android > disposition**. Nommez le fichier **ListItem** et cliquez sur **nouveau**:

[![Nouvelle disposition](designer-walkthrough-images/xs/03-new-layout-sml.png)](designer-walkthrough-images/xs/03-new-layout.png#lightbox)

La nouvelle **ListItem** disposition s’affiche dans le concepteur :

[![Affichage du Concepteur](designer-walkthrough-images/xs/04-designer-view-sml.png)](designer-walkthrough-images/xs/04-designer-view.png#lightbox)

Cliquez sur le **Source** onglet en bas du concepteur pour afficher la source XML pour cette présentation. Lorsque vous cliquez sur le **structure du Document** onglet à droite, il indique que la disposition contienne actuellement un seul **LinearLayout** widget :

[![Concepteur XML](designer-walkthrough-images/xs/05-designer-xml-sml.png)](designer-walkthrough-images/xs/05-designer-xml.png#lightbox)

-----



### <a name="creating-the-list-item-user-interface"></a>Création de l’Interface utilisateur d’élément liste

Ensuite, nous allons créer l’interface utilisateur pour l’application de navigateur de couleur.
Cliquez sur le **concepteur** tab pour revenir à l’aire de conception.
Dans le **boîte à outils**, faites défiler jusqu'à la **Images & Media** section et consulter chaque élément jusqu'à ce que vous trouviez une *ImageView*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Recherchez ImageView](designer-walkthrough-images/vs/07-locate-imageview-sml.png)](designer-walkthrough-images/vs/07-locate-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Recherchez ImageView](designer-walkthrough-images/xs/06-locate-imageview-sml.png)](designer-walkthrough-images/xs/06-locate-imageview.png#lightbox)

-----

Ou bien, vous pouvez entrer *ImageView* dans la barre de recherche pour localiser le `ImageView` widget :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Recherche de ImageView](designer-walkthrough-images/vs/08-imageview-search-sml.png)](designer-walkthrough-images/vs/08-imageview-search.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Recherche de ImageView](designer-walkthrough-images/xs/07-imageview-search-sml.png)](designer-walkthrough-images/xs/07-imageview-search.png#lightbox)

-----

Faites glisser ce `ImageView` sur l’aire de conception :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView sur la zone de dessin](designer-walkthrough-images/vs/09-imageview-on-canvas-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![ImageView sur la zone de dessin](designer-walkthrough-images/xs/08-imageview-on-canvas-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas.png#lightbox)

-----

Cela `ImageView` servira pour afficher un échantillon de couleur dans notre application de navigateur de couleur.

Ensuite, faites glisser un `LinearLayout (Vertical)` widget à partir de la **boîte à outils** dans le concepteur. Notez qu’un contour bleu indique les limites de l’élément ajouté `LinearLayout`et le **structure du Document** indique qu’il se trouve juste en dessous `imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Contour bleu](designer-walkthrough-images/vs/10-blue-outline-sml.png)](designer-walkthrough-images/vs/10-blue-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Contour bleu](designer-walkthrough-images/xs/10-blue-outline-sml.png)](designer-walkthrough-images/xs/10-blue-outline.png#lightbox)

-----

Lorsque vous sélectionnez le `ImageView` dans le concepteur, déplace le contour bleu entourer le `ImageView`; dans le **structure du Document**, la sélection se déplace vers `imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Sélectionnez ImageView](designer-walkthrough-images/vs/11-select-imageview-sml.png)](designer-walkthrough-images/vs/11-select-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Sélectionnez ImageView](designer-walkthrough-images/xs/11-select-imageview-sml.png)](designer-walkthrough-images/xs/11-select-imageview.png#lightbox)

-----

Ensuite, faites glisser un `Text (Large)` widget à partir de la **boîte à outils** dans l’ajoutées récemment `LinearLayout`. Notez que le concepteur utilise vert met en surbrillance pour indiquer où le nouveau widget sera inséré :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Met en évidence les vert](designer-walkthrough-images/vs/12-green-highlight-sml.png)](designer-walkthrough-images/vs/12-green-highlight.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Met en évidence les vert](designer-walkthrough-images/xs/12-green-highlight-sml.png)](designer-walkthrough-images/xs/12-green-highlight.png#lightbox)

-----

Ensuite, ajoutez un `Text (Small)` widget ci-dessous le `Text (Large)` widget :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Ajouter un widget de texte de petite taille](designer-walkthrough-images/vs/13-add-small-text-sml.png)](designer-walkthrough-images/vs/13-add-small-text.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Ajouter un widget de texte de petite taille](designer-walkthrough-images/xs/13-add-small-text-sml.png)](designer-walkthrough-images/xs/13-add-small-text.png#lightbox)

-----

À ce stade, le concepteur doit ressembler à la capture d’écran suivante :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Disposition de concepteur](designer-walkthrough-images/vs/14-raw-layout-sml.png)](designer-walkthrough-images/vs/14-raw-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Disposition de concepteur](designer-walkthrough-images/xs/14-raw-layout-sml.png)](designer-walkthrough-images/xs/14-raw-layout.png#lightbox)

-----

Si les deux `textView` widgets ne sont pas à l’intérieur `linearLayout1`, vous pouvez les faire glisser à `linearLayout1` dans les **structure du Document** et placez-les afin qu’ils s’affichent comme illustré dans la capture d’écran précédente (en retrait sous `linearLayout1`).



### <a name="arranging-the-user-interface"></a>Réorganisation de l’Interface utilisateur

Nous allons modifier l’interface utilisateur pour afficher les `ImageView` sur la gauche, avec les deux `TextView` widgets empilées à droite de la `ImageView`.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Sélectionnez le contrôle `ImageView`.

2.  Dans le **fenêtre Propriétés**, cliquez sur le **par catégorie** trier icône et faites défiler jusqu'à la **disposition - ViewGroup** section.

3.  Modifier la `layout_width` à `wrap_content`:

![Définir le contenu de type wrap](designer-walkthrough-images/vs/15-wrap-content.png "Set wrap content")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1.  Avec la `ImageView` sélectionnée, cliquez sur le **propriétés** onglet.

2.  Sous le **propriétés** , cliquez sur **disposition**.

3.  Faites défiler jusqu'à **ViewGroup** et modifiez le `Width` à `wrap_content`:

[![Contenu de type wrap de jeu](designer-walkthrough-images/xs/15-wrap-content-sml.png)](designer-walkthrough-images/xs/15-wrap-content.png#lightbox)

-----

Une autre façon de modifier le `Width` paramètre consiste à cliquer sur le triangle dans la partie droite du widget pour activer/désactiver le son paramètre de largeur à `wrap_content`:

[![Faites glisser pour définir la largeur](designer-walkthrough-images/xs/16-width-arrow-sml.png)](designer-walkthrough-images/xs/16-width-arrow.png#lightbox)

Cliquez de nouveau sur le triangle retourne le `Width` à `match_parent`.

Ensuite, basculez vers le **structure du Document** et la racine `LinearLayout`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Sélectionnez racine LinearLayout](designer-walkthrough-images/vs/16-root-linearlayout-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Sélectionnez racine LinearLayout](designer-walkthrough-images/xs/17-root-linearlayout-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout.png#lightbox)

-----
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Avec la racine de `LinearLayout` sélectionnée, revenez à la **propriétés** fenêtre, cliquez sur le **alphabétique** trier icône et faites défiler jusqu'à ce que vous trouviez `orientation`. Modifier la `orientation` à `horizontal`:

![Sélectionnez l’orientation horizontale](designer-walkthrough-images/vs/17-horizontal-orientation.png "sélectionnez orientation horizontale")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Avec la racine de `LinearLayout` sélectionnée, revenez à la **propriétés** onglet et cliquez sur **Widget**. Modifier la `Orientation` à `horizontal`:

[![Sélectionnez l’orientation horizontale](designer-walkthrough-images/xs/18-horizontal-orientation-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation.png#lightbox)

-----

À ce stade, le concepteur doit ressembler à la capture d’écran suivante :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Disposition de concepteur](designer-walkthrough-images/vs/18-designer-layout-sml.png)](designer-walkthrough-images/vs/18-designer-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Disposition de concepteur](designer-walkthrough-images/xs/19-designer-layout-sml.png)](designer-walkthrough-images/xs/19-designer-layout.png#lightbox)

-----


### <a name="modifying-the-spacing"></a>Modification de l’espacement

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ensuite, nous allons modifier les paramètres de remplissage et marge dans l’interface utilisateur pour fournir davantage d’espace entre les widgets. Sélectionnez le `ImageView`, cliquez sur le **par catégorie** icône de recherche dans les **propriétés** fenêtre et faites défiler jusqu'à la **disposition** section. Modifier la `Min Height` à `70dp`, le `Min Width` à `50dp`et le `padding` à `10dp`. Cela s’applique la marge intérieure autour de tous les côtés de la `ImageView` et elongates de verticalement :

[![Définir le remplissage](designer-walkthrough-images/vs/19-padding-widths-sml.png)](designer-walkthrough-images/vs/19-padding-widths.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Ensuite, nous allons modifier les paramètres de remplissage et marge dans l’interface utilisateur pour fournir davantage d’espace entre les widgets. Sélectionnez le `ImageView` et cliquez sur le **disposition** onglet sous **propriétés**. Modifier la `Padding` à `10dp`, le `Min Width` à `50dp`et le `Min Height` à `70dp`. Cela s’applique la marge intérieure autour de tous les côtés de la `ImageView` et elongates de verticalement :

[![Définir le remplissage](designer-walkthrough-images/xs/20-padding-widths-sml.png)](designer-walkthrough-images/xs/20-padding-widths.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bas, gauche, droite et en haut de la marge intérieure des paramètres peut être définie indépendamment en entrant des valeurs dans le `paddingBottom`, `paddingLeft`, `paddingRight`, et `paddingTop` des champs, respectivement.
Par exemple, définissez la `paddingLeft` au champ `5dp` et `paddingBottom`, `paddingRight`, et `paddingTop` champs à `10dp`:

[![Paramètres de remplissage personnalisée](designer-walkthrough-images/vs/20-custom-padding-sml.png)](designer-walkthrough-images/vs/20-custom-padding.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

En haut, droite, bas et à gauche remplissage des paramètres peuvent être définies indépendamment en entrant des valeurs dans le `Top`, `Right`, `Bottom`, et `Left` remplir les champs, respectivement. Par exemple, définissez la `Left` à la valeur de remplissage `5dp` et `Top`, `Right`, et `Bottom` pour les valeurs de remplissage `10dp`. Notez que le `Padding` paramètre passe à une liste séparée par des virgules des valeurs suivantes :

[![Paramètres de remplissage personnalisée](designer-walkthrough-images/xs/21-custom-padding-sml.png)](designer-walkthrough-images/xs/21-custom-padding.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ensuite, ajuster la position de la `LinearLayout` widget qui contient les deux `TextView` widgets. Dans le **structure du Document**, sélectionnez `linearLayout1`. Dans le **propriétés** fenêtre, faites défiler vers le **disposition - ViewGroup** section. Définissez `layout_marginBottom`, `layout_marginLeft`, `layout_marginRight`, et `layout_marginTop` à `5dp`, `5dp`, `0dp`, et `5dp` respectivement :

[![Définir les marges](designer-walkthrough-images/vs/21-margins-sml.png)](designer-walkthrough-images/vs/21-margins.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Ensuite, ajuster la position de la `LinearLayout` widget qui contient les deux `TextView` widgets. Dans le **structure du Document**, sélectionnez `linearLayout1`. Dans le **propriétés** volet, sélectionnez le **disposition** onglet. Faites défiler jusqu'à la **ViewGroup** section et définir le `Left`, `Top`, `Right`, et `Bottom` marges à `5dp`, `5dp`, `0dp`, et `5dp` respectivement :

[![Définir les marges](designer-walkthrough-images/xs/22-margins-sml.png)](designer-walkthrough-images/xs/22-margins.png#lightbox)

-----



### <a name="removing-the-default-image"></a>Suppression de l’Image par défaut

Étant donné que nous utilisons le `ImageView` pour afficher des couleurs (plutôt que des images), nous allons supprimer la source de l’image par défaut ajoutée par le modèle.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Sélectionnez le contrôle `ImageView`.

2.  Dans **propriétés**, recherchez le `src` champ.

3.  Désactivez le `src` paramètre afin qu’il soit vide :

![Désactivez le paramètre src ImageView](designer-walkthrough-images/vs/22-clear-img-src.png "désactivez le paramètre src ImageView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1.  Sélectionnez le contrôle `ImageView`.

2.  Cliquez sur le **Widget** onglet sous **propriétés**.

3.  Désactivez le `Src` paramètre afin qu’il soit vide :

[![Désactivez le paramètre src ImageView](designer-walkthrough-images/xs/23-clear-src-sml.png)](designer-walkthrough-images/xs/23-clear-src.png#lightbox)

-----


### <a name="adding-a-listview-container"></a>Ajout d’un conteneur de ListView

Maintenant que le **ListItem** disposition définie, nous allons ajouter un `ListView` pour la mise en page principale. Cela `ListView` contient une liste de **ListItems**.
Dans le **boîte à outils**, recherchez le `ListView` widget et faites-le glisser sur l’aire de conception. Le `ListView` dans le concepteur sera vide à l’exception des lignes bleues qui montrer sa bordure lorsqu’il est sélectionné. Vous pouvez afficher le **structure du Document** pour vérifier que le **ListView** a été ajouté correctement :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Nouvelle ListView](designer-walkthrough-images/vs/23-new-listview.png "nouveau ListView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Nouvelle ListView](designer-walkthrough-images/xs/24-new-listview-sml.png)](designer-walkthrough-images/xs/24-new-listview.png#lightbox)

-----

Par défaut, le `ListView` reçoit un `Id` valeur `@+id/listView1`.
Ouvrez le **Widget** onglet sous **propriétés** et modifiez le `Id` à `@+id/myListView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Renommer des id à myListView](designer-walkthrough-images/vs/24-change-id.png "id de changement de nom à myListView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

[![Renommer des id à myListView](designer-walkthrough-images/xs/25-change-id-sml.png)](designer-walkthrough-images/xs/25-change-id.png#lightbox)

-----

À ce stade, votre interface utilisateur est prêt à utiliser.



### <a name="running-the-application"></a>Exécution de l'application


Ouvrez **MainActivity.cs** et remplacez son code par les éléments suivants :

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity (Label = "DesignerWalkthrough", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem> ();
        ListView listView;

        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);

            SetContentView (Resource.Layout.Main);
            listView = FindViewById<ListView> (Resource.Id.myListView);

            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.DarkRed,
                                               ColorName = "Dark Red", Code = "8B0000" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.SlateBlue,
                                               ColorName = "Slate Blue", Code = "6A5ACD" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.ForestGreen,
                                               ColorName = "Forest Green", Code = "228B22" });

            listView.Adapter = new ColorAdapter (this, colorItems);
        }
    }
    public class ColorAdapter : BaseAdapter<ColorItem>
    {
        List<ColorItem> items;
        Activity context;
        public ColorAdapter(Activity context, List<ColorItem> items)
            : base()
        {
            this.context = context;
            this.items = items;
        }
        public override long GetItemId(int position)
        {
            return position;
        }
        public override ColorItem this[int position]
        {
            get { return items[position]; }
        }
        public override int Count
        {
            get { return items.Count; }
        }
        public override View GetView (int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.ListItem, null);
            view.FindViewById<TextView>(Resource.Id.textView1).Text = item.ColorName;
            view.FindViewById<TextView>(Resource.Id.textView2).Text = item.Code;
            view.FindViewById<ImageView>(Resource.Id.imageView1).SetBackgroundColor(item.Color);

            return view;
        }
    }

    public class ColorItem
    {
        public string ColorName { get; set; }
        public string Code { get; set; }
        public Android.Graphics.Color Color { get; set; }
    }
}

```

Ce code utilise une personnalisée `ListView` adaptateur pour charger les informations de couleur et d’afficher ces données dans l’interface utilisateur que nous venons de créer. Pour conserver cet exemple court, les informations de couleur sont codé en dur dans une liste, mais l’adaptateur peut être modifié pour extraire des informations de couleur à partir d’une source de données ou calculer à la volée. Pour plus d’informations sur `ListView` adaptateurs, consultez [ListViews et adaptateurs](~/android/user-interface/layouts/list-view/index.md).

Générez et exécutez l’application. La capture d’écran suivante est un exemple de la façon dont l’application s’affiche lors de l’exécution sur un appareil :

[![Capture d’écran finale](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)



## <a name="summary"></a>Récapitulatif

Dans cet article, nous avons passé en revue l’utilisation du Concepteur de Xamarin.Android dans Visual Studio pour Mac pour créer une interface utilisateur. Ensuite, nous avons expliqué comment créer l’interface pour un seul élément dans une liste.
Avant cela, nous avons examiné comment ajouter des widgets et disposer visuellement, ainsi que comment assigner des ressources et définir plusieurs propriétés sur ces widgets. En conclusion, nous illustre la façon dont l’interface que nous avons créé dans le concepteur s’exécute dans un exemple d’application.
