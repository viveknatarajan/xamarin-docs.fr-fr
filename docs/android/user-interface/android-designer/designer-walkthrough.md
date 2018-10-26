---
title: L’aide du Concepteur de Xamarin.Android
description: Cet article est une procédure pas à pas du Concepteur de Xamarin.Android. Il montre comment créer une interface utilisateur pour une application de navigateur petit couleur ; Cette interface utilisateur est créée entièrement dans le concepteur.
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 1174fe5cb417d4977fd6519086e6c4942e74c10b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120268"
---
# <a name="using-the-xamarinandroid-designer"></a>L’aide du Concepteur de Xamarin.Android

_Cet article est une procédure pas à pas du Concepteur de Xamarin.Android. Il montre comment créer une interface utilisateur pour une application de navigateur petit couleur ; Cette interface utilisateur est créée entièrement dans le concepteur._


## <a name="overview"></a>Vue d'ensemble

Interfaces utilisateur Android peuvent être créés de façon déclarative à l’aide de fichiers XML ou par programmation en écrivant du code. Le Concepteur de Xamarin.Android permet aux développeurs de créer et modifier les dispositions déclaratives visuellement, sans nécessiter de modification manuelle des fichiers XML. Le concepteur fournit également des commentaires en temps réel qui permet au développeur d’évaluer les changements de l’interface utilisateur sans avoir à redéployer l’application sur un appareil ou un émulateur. Ces fonctionnalités de concepteur peuvent accélérer considérablement le développement d’interface utilisateur Android.
Cet article montre comment utiliser le Concepteur de Xamarin.Android pour créer visuellement une interface utilisateur.

## <a name="walkthrough"></a>Procédure pas à pas

L’objectif de cette procédure pas à pas consiste à utiliser le concepteur Android pour créer une interface utilisateur pour une application de navigateur exemple couleur. L’application de navigateur couleur présente une liste de couleurs, leurs noms et leurs valeurs RVB. Vous allez apprendre à ajouter des widgets à le **aire de conception** , ainsi que comment organiser visuellement ces widgets. Après cela, vous allez apprendre à modifier les widgets de manière interactive sur le **aire de conception** ou à l’aide du concepteur **propriétés** volet. Enfin, vous verrez comment la conception se présente l’application s’exécute sur un appareil ou un émulateur.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="creating-a-new-project"></a>Création d’un projet

La première étape consiste à créer un nouveau projet Xamarin.Android. Lancez Visual Studio, cliquez sur **nouveau projet...** , puis choisissez le **Visual C\# > Android > application Android (Xamarin)** modèle.
Nommez la nouvelle application **DesignerWalkthrough** et cliquez sur **OK**.

[![Application vide Android](designer-walkthrough-images/vs/01-android-app-w158-sml.png)](designer-walkthrough-images/vs/01-android-app-w158.png#lightbox)

Dans le **application Android** boîte de dialogue, choisissez **application vide** et cliquez sur **OK**:

[![En sélectionnant le modèle application vide Android](designer-walkthrough-images/vs/02-blank-app-w158-sml.png)](designer-walkthrough-images/vs/02-blank-app-w158.png#lightbox)


### <a name="adding-a-layout"></a>Ajout d’une disposition

L’étape suivante consiste à créer un **LinearLayout** qui contiendra l’utilisateur des éléments d’interface. Avec le bouton droit **ressources/disposition** dans le **l’Explorateur de solutions** et sélectionnez **Ajouter > nouvel élément...** . Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **disposition Android**. Nommez le fichier **list_item** et cliquez sur **ajouter**:

[![Nouvelle disposition](designer-walkthrough-images/vs/03-new-layout-w158-sml.png)](designer-walkthrough-images/vs/03-new-layout-w158.png#lightbox)

La nouvelle **list_item** mise en page est affiché dans le concepteur. Remarquez que les deux volets sont affichent &ndash; le *aire de conception* pour le **list_item** est visible dans le volet gauche tandis que sa source XML est affiché dans le volet droit. Vous pouvez échanger les positions de la **aire de conception** et **Source** volets en cliquant sur le **échange volets** icône située entre les deux volets :

[![Vue du Concepteur](designer-walkthrough-images/vs/04-designer-view-w158-sml.png)](designer-walkthrough-images/vs/04-designer-view-w158.png#lightbox)

À partir de la **vue** menu, cliquez sur **Other Windows > structure du Document** pour ouvrir le **structure du Document**. Le **structure du Document** montre que la disposition contienne actuellement un seul **LinearLayout** widget :

[![Structure du document](designer-walkthrough-images/vs/06-document-outline-w158-sml.png)](designer-walkthrough-images/vs/06-document-outline-w158.png#lightbox)

L’étape suivante consiste à créer l’interface utilisateur pour l’application de navigateur de couleur dans cet `LinearLayout`.

### <a name="creating-the-list-item-user-interface"></a>Création de l’interface utilisateur de l’élément de liste

Si le **boîte à outils** volet ne s’affiche pas, cliquez sur le **boîte à outils** onglet sur la gauche. Dans le **boîte à outils**, faites défiler jusqu'à la **Images & Media** section et descendez plus bas jusqu'à ce que vous trouviez un `ImageView`:

[![Recherchez ImageView](designer-walkthrough-images/vs/07-locate-imageview-w158-sml.png)](designer-walkthrough-images/vs/07-locate-imageview-w158.png#lightbox)

Alternativement, vous pouvez entrer *ImageView* dans la barre de recherche pour localiser le `ImageView`:

[![Recherche de ImageView](designer-walkthrough-images/vs/08-imageview-search-w158-sml.png)](designer-walkthrough-images/vs/08-imageview-search-w158.png#lightbox)

Faites glisser ce `ImageView` sur l’aire de conception (cela `ImageView` doit être utilisée pour afficher un échantillon de couleur dans l’application de navigateur couleur) :

[![ImageView sur la zone de dessin](designer-walkthrough-images/vs/09-imageview-on-canvas-w158-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas-w158.png#lightbox)

Ensuite, faites glisser un `LinearLayout (Vertical)` widget à partir de la **boîte à outils** dans le concepteur. Un contour bleu indique les limites de l’ajout `LinearLayout`. Le **structure du Document** montre qu’il est un enfant de `LinearLayout`, qui se trouve sous `imageView1 (ImageView)`:

[![Contour bleu](designer-walkthrough-images/vs/10-blue-outline-w158-sml.png)](designer-walkthrough-images/vs/10-blue-outline-w158.png#lightbox)

Lorsque vous sélectionnez le `ImageView` dans le concepteur, déplace le contour bleu entourer le `ImageView`. En outre, la sélection se déplace vers `imageView1 (ImageView)` dans le **structure du Document**:

[![Sélectionnez ImageView](designer-walkthrough-images/vs/11-select-imageview-w158-sml.png)](designer-walkthrough-images/vs/11-select-imageview-w158.png#lightbox)

Ensuite, faites glisser un `Text (Large)` widget à partir de la **boîte à outils** dans le récemment ajoutés `LinearLayout`. Notez que le concepteur utilise vert met en surbrillance pour indiquer où le nouveau widget sera inséré :

[![Met en évidence les vert](designer-walkthrough-images/vs/12-green-highlight-w158-sml.png)](designer-walkthrough-images/vs/12-green-highlight-w158.png#lightbox)

Ensuite, ajoutez un `Text (Small)` widget ci-dessous le `Text (Large)` widget :

[![Ajouter un widget de texte de petite taille](designer-walkthrough-images/vs/13-add-small-text-w158-sml.png)](designer-walkthrough-images/vs/13-add-small-text-w158.png#lightbox)

À ce stade, l’aire du concepteur doit ressembler à la capture d’écran suivante :

[![Disposition de concepteur](designer-walkthrough-images/vs/14-raw-layout-w158-sml.png)](designer-walkthrough-images/vs/14-raw-layout-w158.png#lightbox)

Si les deux `textView` widgets ne sont pas à l’intérieur `linearLayout1`, vous pouvez les faire glisser à `linearLayout1` dans le **structure du Document** et placez-les afin qu’ils s’affichent comme indiqué dans la capture d’écran précédente (en retrait sous `linearLayout1`).


### <a name="arranging-the-user-interface"></a>Réorganisation de l’interface utilisateur

L’étape suivante consiste à modifier l’interface utilisateur pour afficher le `ImageView` sur la gauche, avec les deux `TextView` widgets empilés à droite de la `ImageView`.

1.  Sélectionnez le contrôle `ImageView`.

2.  Dans le **fenêtre Propriétés**, entrez *largeur* dans la recherche et recherchez **largeur de présentation**.

3.  Modifier le **largeur de présentation** à `wrap_content`:

![Contenu de type wrap de jeu](designer-walkthrough-images/vs/15-wrap-content-w158.png)

Une autre façon de modifier le `Width` paramètre consiste à cliquer sur le triangle dans la partie droite du widget pour activer/désactiver le son paramètre de largeur à `wrap_content`:

![Faites glisser pour définir la largeur](designer-walkthrough-images/vs/15b-width-arrow-w158.png)

En cliquant sur le triangle à nouveau le `Width` à `match_parent`. Ensuite, accédez à la **structure du Document** volet et sélectionnez la racine `LinearLayout`:

[![Sélectionnez la racine LinearLayout](designer-walkthrough-images/vs/16-root-linearlayout-w158-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout-w158.png#lightbox)

Avec la racine `LinearLayout` sélectionnée, revenez à la **propriétés** volet, entrez *orientation* dans la recherche et recherchez le **Orientation** paramètre. Modification **Orientation** à `horizontal`:

![Sélectionnez l’orientation horizontale](designer-walkthrough-images/vs/17-horizontal-orientation-w158.png)

À ce stade, l’aire du concepteur doit ressembler à la capture d’écran suivante.
Notez que le `TextView` widgets ont été déplacées vers la droite de la `ImageView`:

[![Disposition de concepteur](designer-walkthrough-images/vs/18-designer-layout-w158-sml.png)](designer-walkthrough-images/vs/18-designer-layout-w158.png#lightbox)

### <a name="modifying-the-spacing"></a>Modification de l’espacement

L’étape suivante consiste à modifier les paramètres padding et margin dans l’interface utilisateur pour fournir davantage d’espace entre les widgets. Sélectionnez le `ImageView` sur l’aire de conception. Dans le **propriétés** volet, entrez `min` dans la zone de recherche. Entrez `70dp` pour **Min hauteur** et `50dp` pour **largeur Min**:

[![Définir la hauteur et la largeur](designer-walkthrough-images/vs/18b-set-height-width-sml.png)](designer-walkthrough-images/vs/18b-set-height-width.png#lightbox)

Dans le **propriétés** volet, entrez `padding` dans la recherche de zone et entrez `10dp` pour **remplissage**. Ces `minHeight`, `minWidth` et `padding` ajouter des paramètres de marge intérieure autour de tous les côtés de la `ImageView` et allonge de verticalement. Notez que la mise en page XML lorsque vous entrez ces valeurs :

[![Définir une marge intérieure](designer-walkthrough-images/vs/19-padding-widths-w158-sml.png)](designer-walkthrough-images/vs/19-padding-widths-w158.png#lightbox)

La partie inférieure, gauche, avec le bouton droit et en haut de la marge intérieure de paramètres peut être définie indépendamment en entrant des valeurs dans le **bas Padding**, **gauche Padding**, **marge de droite**et  **Marge intérieure haut** champs, respectivement.
Par exemple, définissez la **marge de gauche** champ `5dp` et le **bas Padding**, **droite Padding**, et **haut Padding** champs pour `10dp`:

[![Paramètres de remplissage personnalisée](designer-walkthrough-images/vs/20-custom-padding-w158-sml.png)](designer-walkthrough-images/vs/20-custom-padding-w158.png#lightbox)

Ensuite, ajustez la position de la `LinearLayout` widget qui contient les deux `TextView` widgets. Dans le **structure du Document**, sélectionnez `linearLayout1`. Dans le **propriétés** fenêtre, entrez `margin` dans la zone de recherche. Définissez **bas de marge de disposition**, **gauche de marge de disposition**, et **haut de marge de disposition** à `5dp`. Définissez **disposition marge droite** à `0dp`:

[![Définir les marges](designer-walkthrough-images/vs/21-margins-w158-sml.png)](designer-walkthrough-images/vs/21-margins-w158.png#lightbox)

### <a name="removing-the-default-image"></a>Suppression de l’image par défaut

Étant donné que le `ImageView` est utilisé pour afficher les couleurs (plutôt que des images), l’étape suivante consiste à supprimer de la source de l’image par défaut ajoutée par le modèle.

1.  Sélectionnez le `ImageView` sur le **aire du concepteur**.

2.  Dans **propriétés**, entrez *src* dans la zone de recherche.

3.  Cliquez sur le petit carré vers la droite de la **Src** propriété paramètre et sélectionnez **réinitialiser**:

[![Désactivez le paramètre src ImageView](designer-walkthrough-images/vs/22-clear-img-src-w158-sml.png)](designer-walkthrough-images/vs/22-clear-img-src-w158.png#lightbox)

Cette opération supprime `android:src="@android:drawable/ic_menu_gallery"` à partir de la source XML pour ce `ImageView`.

### <a name="adding-a-listview-container"></a>Ajout d’un conteneur de ListView

Maintenant que le **list_item** disposition est définie, l’étape suivante consiste à ajouter un `ListView` à la disposition principale. Cela `ListView` contient une liste de **list_item**. 

Dans le **l’Explorateur de solutions**, ouvrez **Resources/layout/activity_main.axml**. Dans le **boîte à outils**, localisez le `ListView` widget et faites-le glisser sur le **aire de conception**. Le `ListView` dans le concepteur sera vide à l’exception des lignes bleues mettant en évidence sa bordure lorsqu’il est sélectionné. Vous pouvez afficher le **structure du Document** pour vérifier que le **ListView** a été ajouté correctement :

[![Nouveau ListView](designer-walkthrough-images/vs/23-new-listview-w158-sml.png)](designer-walkthrough-images/vs/23-new-listview-w158.png#lightbox)

Par défaut, le `ListView` reçoit un `Id` valeur `@+id/listView1`.
Tandis que `listView1` est toujours sélectionné dans le **structure du Document**, ouvrez le **propriétés** volet, cliquez sur **organiser par**, puis sélectionnez **catégorie**.
Ouvrez **principal**, localisez le **Id** propriété et remplacez sa valeur par `@+id/myListView`:

[![Renommer des id à myListView](designer-walkthrough-images/vs/24-change-id-w158-sml.png)](designer-walkthrough-images/vs/24-change-id-w158.png#lightbox)

À ce stade, l’interface utilisateur est prêt à être utilisé.

### <a name="running-the-application"></a>Exécution de l’application

Ouvrez **MainActivity.cs** et remplacez son code par le suivant :

```csharp
using Android.App;
using Android.Widget;
using Android.Views;
using Android.OS;
using Android.Support.V7.App;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        List<ColorItem> colorItems = new List<ColorItem>();
        ListView listView;

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);
            listView = FindViewById<ListView>(Resource.Id.myListView);

            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.DarkRed,
                ColorName = "Dark Red",
                Code = "8B0000"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.SlateBlue,
                ColorName = "Slate Blue",
                Code = "6A5ACD"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.ForestGreen,
                ColorName = "Forest Green",
                Code = "228B22"
            });

            listView.Adapter = new ColorAdapter(this, colorItems);
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
        public override View GetView(int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.list_item, null);
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

Ce code utilise un personnalisé `ListView` adaptateur à charger les informations de couleur et afficher ces données dans l’interface utilisateur qui vient d’être créé. Pour simplifier cet exemple court, les informations de couleur sont codé en dur dans une liste, mais l’adaptateur peut être modifié pour extraire des informations de couleur à partir d’une source de données ou être calculé à la volée. Pour plus d’informations sur `ListView` adaptateurs, consultez [ListView](~/android/user-interface/layouts/list-view/index.md).

Générez et exécutez l’application. La capture d’écran suivante est un exemple de la façon dont l’application s’affiche lors de l’exécution sur un appareil :

[![Capture d’écran finale](designer-walkthrough-images/vs/25-final-screenshot-sml.png)](designer-walkthrough-images/vs/25-final-screenshot.png#lightbox)



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

### <a name="creating-a-new-project"></a>Création d’un projet

La première étape consiste à créer un nouveau projet Xamarin.Android.

Lancez Visual Studio pour Mac, puis cliquez sur **nouveau projet...** . Choisissez le **application Android** modèle et cliquez sur **suivant**:

[![Application vide Android](designer-walkthrough-images/xs/01-android-app-m75-sml.png)](designer-walkthrough-images/xs/01-android-app-m75.png#lightbox)

Nommez la nouvelle application **DesignerWalkthrough**. Sous **plateformes cibles**, sélectionnez **et le plus récent** et cliquez sur **suivant**:

[![Application de nom](designer-walkthrough-images/xs/02-designer-walkthrough-m75-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough-m75.png#lightbox)

Dans l’écran suivant de la boîte de dialogue, cliquez sur **créer**.

### <a name="adding-a-layout"></a>Ajout d’une disposition

L’étape suivante consiste à créer un **LinearLayout** qui contiendra l’utilisateur des éléments d’interface.

Dans Visual Studio pour Mac, cliquez sur **ressources/disposition** dans le **Solution** de remplissage et sélectionnez **Ajouter > nouveau fichier...** . Dans le **nouveau fichier** boîte de dialogue, sélectionnez **Android > disposition**. Nommez le fichier **list_item** et cliquez sur **New**:

[![Nouvelle disposition](designer-walkthrough-images/xs/03-new-layout-m75-sml.png)](designer-walkthrough-images/xs/03-new-layout-m75.png#lightbox)

Une fois que ce fichier est ajouté, la nouvelle **list_item** mise en page s’affiche sur le **aire de conception** (si vous voyez le message, *ce projet contient des ressources qui n’ont pas été compilées avec succès, rendu susceptibles d’être affecté*, cliquez sur **Générer > générer tout** pour générer le projet) :

[![Vue du Concepteur](designer-walkthrough-images/xs/04-designer-view-m75-sml.png)](designer-walkthrough-images/xs/04-designer-view-m75.png#lightbox)

Cliquez sur le **Source** onglet en bas du concepteur pour afficher la source XML pour cette présentation. Lorsque vous cliquez sur le **structure du Document** onglet à droite, il montre que la disposition contienne actuellement un seul **LinearLayout** widget :

[![Concepteur XML](designer-walkthrough-images/xs/05-designer-xml-m75-sml.png)](designer-walkthrough-images/xs/05-designer-xml-m75.png#lightbox)

L’étape suivante consiste à créer l’interface utilisateur pour l’application de navigateur de couleur.

### <a name="creating-the-list-item-user-interface"></a>Création de l’interface utilisateur de l’élément de liste

Cliquez sur le **concepteur** onglet au bas de l’écran pour revenir à la **aire du concepteur**. Dans le **boîte à outils** volet à droite, faites défiler jusqu'à la **Images & Media** section et recherchez `ImageView`:

[![Recherchez ImageView](designer-walkthrough-images/xs/06-locate-imageview-m75-sml.png)](designer-walkthrough-images/xs/06-locate-imageview-m75.png#lightbox)

Alternativement, vous pouvez entrer *ImageView* dans la barre de recherche pour localiser le `ImageView`:

[![Recherche de ImageView](designer-walkthrough-images/xs/07-imageview-search-m75-sml.png)](designer-walkthrough-images/xs/07-imageview-search-m75.png#lightbox)

Faites glisser ce `ImageView` sur le **aire de conception** (cela `ImageView` doit être utilisée pour afficher un échantillon de couleur dans l’application de navigateur couleur) :

[![ImageView sur la zone de dessin](designer-walkthrough-images/xs/08-imageview-on-canvas-m75-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas-m75.png#lightbox)

Ensuite, faites glisser un `LinearLayout (Vertical)` widget à partir de la **boîte à outils** dans le **aire de conception**. Un contour bleu indique les limites de l’ajout `LinearLayout`. Le **structure du Document** montre qu’il est un enfant de `LinearLayout`, située sous `imageView1 (ImageView)`:

[![Contour bleu](designer-walkthrough-images/xs/10-blue-outline-m75-sml.png)](designer-walkthrough-images/xs/10-blue-outline-m75.png#lightbox)

Lorsque vous sélectionnez le `ImageView` dans le concepteur, déplace le contour bleu entourer le `ImageView`. En outre, la sélection se déplace vers `imageView1 (ImageView)` dans le **structure du Document**:

[![Sélectionnez ImageView](designer-walkthrough-images/xs/11-select-imageview-m75-sml.png)](designer-walkthrough-images/xs/11-select-imageview-m75.png#lightbox)

Ensuite, faites glisser un `Text (Large)` widget à partir de la **boîte à outils** dans le récemment ajoutés `LinearLayout`. Notez que lorsque vous faites glisser la souris sur le **aire de conception**, il met en évidence où le nouveau widget sera inséré.
Le `Text (Large)` widget doit se trouver à l’intérieur de `linearLayout1` comme illustré ici :

[![Ajout du widget Text (Large)](designer-walkthrough-images/xs/12-green-highlight-m75-sml.png)](designer-walkthrough-images/xs/12-green-highlight-m75.png#lightbox)

Ensuite, ajoutez un `Text (Small)` widget ci-dessous le `Text (Large)` widget. À ce stade, le **aire de conception** doit ressembler à la capture d’écran suivante :

[![Ajouter un widget de texte de petite taille](designer-walkthrough-images/xs/13-add-small-text-m75-sml.png)](designer-walkthrough-images/xs/13-add-small-text-m75.png#lightbox)

Si les deux `textView` widgets ne sont pas à l’intérieur `linearLayout1`, vous pouvez les faire glisser à `linearLayout1` dans le **structure du Document** et placez-les afin qu’ils apparaissent comme indiqué dans la capture d’écran précédente (mis en retrait sous `linearLayout1`).


### <a name="arranging-the-user-interface"></a>Réorganisation de l’interface utilisateur

L’étape suivante consiste à modifier l’interface utilisateur pour afficher le `ImageView` sur la gauche, avec les deux `TextView` widgets empilés à droite de la `ImageView`.

1.  Avec le `ImageView` sélectionnée, cliquez sur le **propriétés** onglet.

2.  Juste en dessous du **propriétés** , cliquez sur **disposition**.

3.  Faites défiler jusqu'à **ViewGroup** et modifiez le `Width` à `wrap_content`:

[![Contenu de type wrap de jeu](designer-walkthrough-images/xs/15-wrap-content-m75-sml.png)](designer-walkthrough-images/xs/15-wrap-content-m75.png#lightbox)

Une autre façon de modifier le `Width` paramètre consiste à cliquer sur le triangle dans la partie droite du widget pour activer/désactiver le son paramètre de largeur à `wrap_content`:

[![Faites glisser pour définir la largeur](designer-walkthrough-images/xs/16-width-arrow-m75-sml.png)](designer-walkthrough-images/xs/16-width-arrow-m75.png#lightbox)

En cliquant sur le triangle à nouveau le `Width` à `match_parent`. Ensuite, accédez à la **structure du Document** volet et sélectionnez la racine `LinearLayout`:

[![Sélectionnez la racine LinearLayout](designer-walkthrough-images/xs/17-root-linearlayout-m75-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout-m75.png#lightbox)

Avec la racine `LinearLayout` sélectionnée, revenez à la **propriétés** onglet et cliquez sur **Widget**. Modifier le `Orientation` à `horizontal` comme indiqué ci-dessous. À ce stade, le **aire de conception** doit ressembler à la capture d’écran suivante. Notez que le `TextView` widgets ont été déplacées vers la droite de la `ImageView`:

[![Sélectionnez l’orientation horizontale](designer-walkthrough-images/xs/18-horizontal-orientation-m75-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation-m75.png#lightbox)


### <a name="modifying-the-spacing"></a>Modification de l’espacement

L’étape suivante consiste à modifier les paramètres de remplissage et marge dans l’interface utilisateur pour fournir davantage d’espace entre les widgets. Sélectionnez le `ImageView` et cliquez sur le **disposition** onglet sous **propriétés**. Modifier le `Min Width` à `50dp`, le `Min Height` à `70dp`et le `Padding` à `10dp`.
Cela s’applique la marge intérieure autour de tous les côtés de la `ImageView` et elongates de verticalement :

[![Définir une marge intérieure](designer-walkthrough-images/xs/20-padding-widths-m75-sml.png)](designer-walkthrough-images/xs/20-padding-widths-m75.png#lightbox)

Le haut, droite, bas et paramètres de remplissage à gauche peuvent être définis indépendamment en entrant des valeurs dans le `Top`, `Right`, `Bottom`, et `Left` remplir les champs, respectivement. Par exemple, définissez la `Left` à la valeur de remplissage `5dp` et `Top`, `Right`, et `Bottom` pour les valeurs de remplissage `10dp`. Notez que le `Padding` paramètre devient une liste séparée par des virgules des valeurs suivantes :

[![Paramètres de remplissage personnalisée](designer-walkthrough-images/xs/21-custom-padding-m75-sml.png)](designer-walkthrough-images/xs/21-custom-padding-m75.png#lightbox)

Ensuite, ajustez la position de la `LinearLayout` widget qui contient les deux `TextView` widgets. Dans le **structure du Document**, sélectionnez `linearLayout1`. Dans le **propriétés** volet, sélectionnez le **disposition** onglet. Faites défiler jusqu'à la **ViewGroup** section et définissez le `Left`, `Top`, `Right`, et `Bottom` marges à `5dp`, `5dp`, `0dp`, et `5dp` respectivement :

[![Définir les marges](designer-walkthrough-images/xs/22-margins-m75-sml.png)](designer-walkthrough-images/xs/22-margins-m75.png#lightbox)

### <a name="removing-the-default-image"></a>Suppression de l’image par défaut

Étant donné que le `ImageView` est utilisé pour afficher les couleurs (plutôt que des images), l’étape suivante consiste à supprimer de la source de l’image par défaut ajoutée par le modèle.

1.  Sélectionnez le contrôle `ImageView`.

2.  Cliquez sur le **Widget** onglet sous **propriétés**.

3.  Effacer la `Src` paramètre afin qu’il soit vide :

[![Désactivez le paramètre src ImageView](designer-walkthrough-images/xs/23-clear-src-m75-sml.png)](designer-walkthrough-images/xs/23-clear-src-m75.png#lightbox)

Cette opération supprime `android:src="@android:drawable/ic_menu_gallery"` à partir de la source XML pour ce `ImageView`.

### <a name="adding-a-listview-container"></a>Ajout d’un conteneur de ListView

Maintenant que le **list_item** disposition est définie, l’étape suivante consiste à ajouter un `ListView` à la disposition principale. Cela `ListView` contient une liste de **list_item**. 

Dans le **l’Explorateur de solutions**, ouvrez **Resources/layout/Main.axml**.
Cliquez sur le `Button` widget (le cas échéant) et supprimez-le. Dans le **boîte à outils**, localisez le `ListView` widget et faites-le glisser sur le **aire de conception**.
Le `ListView` dans le concepteur sera vide à l’exception des lignes bleues mettant en évidence sa bordure lorsqu’il est sélectionné. Vous pouvez afficher le **structure du Document** pour vérifier que le **ListView** a été ajouté correctement :

[![Nouveau ListView](designer-walkthrough-images/xs/24-new-listview-m75-sml.png)](designer-walkthrough-images/xs/24-new-listview-m75.png#lightbox)

Par défaut, le `ListView` reçoit un `Id` valeur `@+id/listView1`.
Tandis que `listView1` est toujours sélectionné dans le **structure du Document**, ouvrez le **propriétés** volet, cliquez sur **organiser par**, puis sélectionnez **catégorie**.
Ouvrez **principal**, localisez le **Id** propriété et remplacez sa valeur par `@+id/myListView`:

[![Renommer des id à myListView](designer-walkthrough-images/xs/25-change-id-m75-sml.png)](designer-walkthrough-images/xs/25-change-id-m75.png#lightbox)

À ce stade, l’interface utilisateur est prêt à être utilisé.

### <a name="running-the-application"></a>Exécution de l’application

Ouvrez **MainActivity.cs** et remplacez son code par le suivant :

```csharp
using Android.App;
using Android.Widget;
using Android.Views;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity(Label = "@string/app_name", MainLauncher = true)]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem>();
        ListView listView;

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.Main);
            listView = FindViewById<ListView>(Resource.Id.myListView);

            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.DarkRed,
                ColorName = "Dark Red",
                Code = "8B0000"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.SlateBlue,
                ColorName = "Slate Blue",
                Code = "6A5ACD"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.ForestGreen,
                ColorName = "Forest Green",
                Code = "228B22"
            });

            listView.Adapter = new ColorAdapter(this, colorItems);
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
        public override View GetView(int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.list_item, null);
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

Ce code utilise un personnalisé `ListView` adaptateur à charger les informations de couleur et afficher ces données dans l’interface utilisateur qui vient d’être créé. Pour simplifier cet exemple court, les informations de couleur sont codé en dur dans une liste, mais l’adaptateur peut être modifié pour extraire des informations de couleur à partir d’une source de données ou être calculé à la volée. Pour plus d’informations sur `ListView` adaptateurs, consultez [ListView](~/android/user-interface/layouts/list-view/index.md).

Générez et exécutez l’application. La capture d’écran suivante est un exemple de la façon dont l’application s’affiche lors de l’exécution sur un appareil :

[![Capture d’écran finale](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)

-----


## <a name="summary"></a>Récapitulatif

Cet article a présenté le processus d’utilisation du Concepteur de Xamarin.Android dans Visual Studio pour créer une interface utilisateur pour une application de base.
Il vous a montré comment créer l’interface pour un seul élément dans une liste, et il illustre comment ajouter des widgets et leur disposition visuellement.
Il explique également comment assigner des ressources, puis définissez différentes propriétés sur ces widgets.
