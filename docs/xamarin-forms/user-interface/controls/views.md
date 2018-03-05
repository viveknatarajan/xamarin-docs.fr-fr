---
title: Vues de Xamarin.Forms
description: "Xamarin.Forms vues sont les blocs de construction d’interfaces utilisateur de mobile multiplateforme."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: df8c8463b2556035c5369c70cb10dbc3dc6b6743
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-views"></a>Vues de Xamarin.Forms

_Xamarin.Forms vues sont les blocs de construction d’interfaces utilisateur de mobile multiplateforme._

<style>.tableimg {max-largeur : aucun ! important ;}</style>

## <a name="views"></a>Affichages

Xamarin.Forms utilisant le mot *vue* pour faire référence à des objets visuels tels que des boutons, des étiquettes ou des zones de texte - qui peuvent être plus connus en tant que contrôles de widgets.

Ces éléments d’interface utilisateur sont généralement sont des sous-classes de [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).

<br clear="right" />

Xamarin.Forms prend en charge :

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Type</strong>
    </th>
    <th>
      <strong>Description</strong>
    </th>
    <th style="min-width:400px">
      <strong>Screenshot</strong>
    </th>

  </thead>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a>
    </td>
    <td valign="top">
Un contrôle visuel utilisé pour indiquer que quelque chose est en cours. Ce contrôle fournit un indice visuel à l’utilisateur qui se passe, sans informations sur sa progression.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ActivityIndicatorDemoPage.cs"><img src="views-images/ActivityIndicator.png" title="Exemple de ActivityIndicator" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> utilisé pour dessiner un rectangle de couleur Uni. BoxView désigne utile pour les images ou des éléments personnalisés lors de la création de prototypes initiale. BoxView a une demande de taille par défaut de 40 x 40. Si vous avez besoin d’une taille différente, attribuez le <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/">VisualElement.WidthRequest</a> et <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/">VisualElement.HeightRequest</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/BoxViewDemoPage.cs"><img src="views-images/BoxView.png" title="Exemple de BoxView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">Button</a>
    </td>
    <td align="center" valign="top">
Un bouton <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> qui réagit aux événements tactiles.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ButtonDemoPage.cs"><img src="views-images/Button.png" title="Exemple de bouton" class="tableimg">
    </a></td>
  </tr>
  <tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> qui autorise date. La représentation visuelle d’un sélecteur de dates est très similaire à celui de <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">entrée</a>, sauf qu’un contrôle spécial pour la sélection d’une date apparaît à la place d’un clavier </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/DatePickerDemoPage.cs"><img src="views-images/DatePicker.png" title="Exemple de sélecteur de dates" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Éditeur</a>
    </td>
    <td valign="top">
Un contrôle qui peut modifier plusieurs lignes de texte. Pour les entrées de ligne unique, consultez <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">entrée</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EditorDemoPage.cs"><img src="views-images/Editor.png" title="Exemple d’éditeur" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a>
    </td>
    <td valign="top">
Un contrôle qui peut modifier une seule ligne de texte. L’entrée est une entrée de texte de ligne unique. Il est mieux utilisé pour la collecte des petits éléments discrets d’informations, telles que les noms d’utilisateur et mots de passe.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="views-images/Entry.png" title="Exemple d’entrée" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Image</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> qui contient une image.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">API d’image</a>
    <br />
    <a href="~/xamarin-forms/user-interface/images.md">Utilisation des Images</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageDemoPage.cs">Source de démonstration</a>
    </td>
    <td>
    <img src="views-images/Image.png" title="Exemple d’image" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">Label</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> qui affiche le texte dans un format de lecture seule. Une étiquette est utilisée pour afficher les éléments de ligne de texte unique, ainsi que les blocs de plusieurs lignes de texte.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/LabelDemoPage.cs"><img src="views-images/Label.png" title="Exemple d’étiquette" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a>
    </td>
    <td valign="top">
Un <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/">l’annonceAfficher le</a> qui affiche une collection de données sous la forme d’une liste verticale.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">API de ListView</a>
    <br />
    <a href="~/xamarin-forms/user-interface/listview/index.md">Documentation de ListView</a>
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ListViewDemoPage.cs"><img src="views-images/ListView.png" title="Exemple de ListView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/">OpenGLView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> qui affiche le contenu OpenGL.
    <ul>
      <li>Fonctionne uniquement pour les projets iOS et Android (aucune prise en charge de Windows Phone).
      <li>Requiert une référence à la <b>OpenTK-1.0</b> assembly dans les projets iOS et Android.</li>
      <li>Mieux adapté pour l’utiliser dans les projets partagés ; Si utilisé dans une bibliothèque PCL puis un DependencyService sera également requis.</li>
    </ul>
    </td>
    <td>
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/"><img src="views-images/OpenGL.png" title="Exemple de OpenGlView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">Picker</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> contrôle pour la sélection d’un élément dans une liste. La représentation visuelle d’un sélecteur est similaire à un <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">entrée</a>, mais un contrôle de sélecteur apparaît à la place d’un clavier.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/PickerDemoPage.cs"><img src="views-images/Picker.png" title="Exemple de sélecteur" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> contrôle indiquant une progression.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ProgressBarDemoPage.cs"><img src="views-images/ProgressBar.png" title="Exemple de barre de progression de classe ="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> contrôle qui fournit une zone de recherche.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SearchBarDemoPage.cs"><img src="views-images/SearchBar.png" title="Exemple de SearchBar" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Slider</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> contrôle qui transmet une valeur linéaire.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SliderDemoPage.cs"><img src="views-images/Slider.png" title="Exemple de curseur" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Stepper</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> contrôle qui transmet une valeur discrète, limités à une plage.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StepperDemoPage.cs"><img src="views-images/Stepper.png" title="Exemple d’exécution pas à pas" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">Switch</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> contrôle qui fournit une valeur basculée.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchDemoPage.cs"><img src="views-images/Switch.png" title="Exemple de commutateur" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> qui contient les lignes de <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">cellule</a>s.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/tableview.md">Documentation de TableView</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TableViewFormDemoPage.cs">Source de démonstration</a>
    </td>
    <td>
    <img src="views-images/TableViewNewest.png" title="Exemple de TableView" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> contrôle qui fournit l’enlèvement de temps. La représentation visuelle d’un TimePicker est très similaire à celui de <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">entrée</a>, sauf qu’un contrôle spécial pour un temps de prélèvement apparaît à la place d’un clavier.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TimePickerDemoPage.cs"><img src="views-images/TimePicker.png" title="Exemple de TimePicker" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">vue</a> qui présente le contenu HTML.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">Affichage Web API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/webview.md">Documentation du WebView</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/WebViewDemoPage.cs">Source de démonstration</a>
    </td>
    <td>
    <img src="views-images/WebView.png" title="Exemple d’affichage Web" class="tableimg">
    </td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Liens associés

- [Introduction à Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [La galerie de Xamarin.Forms (exemple)](https://developer.xamarin.com/samples/FormsGallery/)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Documentation de l’API de Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
