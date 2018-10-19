---
title: Contrôles Android (Widgets)
description: Blocs de construction pour la création d’Interfaces utilisateur de Xamarin.Android
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 842fb1df2c9cc1aaf1a106687179a3730c2503bd
ms.sourcegitcommit: 7e4070bc104d612b6754ea35dd5a49c5c3d45f4a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "32436711"
---
# <a name="android-controls-widgets"></a>Contrôles Android (Widgets)

Xamarin.Android expose tous les contrôles d’interface utilisateur natives (widgets) fournis par Android. Ces contrôles peuvent être facilement ajoutés pour les applications Xamarin.Android à l’aide d’Android Designer ou par programmation via des fichiers de disposition XML. Quelle que soit la méthode choisie, Xamarin.Android expose toutes les propriétés de l’objet interface utilisateur et les méthodes en c#. Les sections suivantes présentent des contrôles d’interface utilisateur Android les plus courants et expliquent comment les intégrer dans les applications Xamarin.Android.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[Barre d’action](~/android/user-interface/controls/action-bar.md) 

`ActionBar` est une barre d’outils qui affiche le titre de l’activité, les interfaces de navigation et les autres éléments interactifs. En règle générale, la barre d’action apparaît en haut de la fenêtre de l’activité.

![Exemple ActionBar](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[Saisie semi-automatique](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` est un élément d’affichage de texte modifiable qui affiche des suggestions de saisie semi-automatique automatiquement pendant que l’utilisateur tape. La liste de suggestions s’affiche dans la liste déroulante à partir de laquelle l’utilisateur peut choisir un élément à remplacer le contenu de la zone d’édition avec.

![Exemple de saisie semi-automatique](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[Boutons](~/android/user-interface/controls/buttons/index.md)

Boutons sont des éléments d’interface utilisateur que l’utilisateur appuie sur une action à effectuer.

![Boutons de l’exemple](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Calendar](~/android/user-interface/controls/calendar.md)

Le `Calendar` classe est utilisée pour la conversion d’une instance spécifique de temps (une valeur de milliseconde décalée par rapport à l’époque) à des valeurs telles qu’année, mois, heure, jour du mois et la date de la semaine suivante.
`Calendar` prend en charge une multitude d’options d’interaction avec les données de calendrier, y compris la possibilité de lire et écrire des événements, les participants et les rappels. En utilisant le fournisseur de calendrier dans votre application, les données que vous ajoutez via l’API seront affiche dans l’application Calendrier intégré qui est fourni avec Android.

![Calendrier de l’exemple](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` est un composant de l’interface utilisateur qui présente le contenu de texte et image dans les vues qui ressemblent à des cartes. `CardView` est implémenté comme un `FrameLayout` widget avec des angles arrondis et une ombre. En règle générale, un `CardView` est utilisé pour présenter un élément de ligne unique dans un `ListView` ou `GridView` afficher le groupe.

![Affichage de carte d’exemple](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[Modifier le texte](~/android/user-interface/controls/edit-text.md)

`EditText` est un élément d’interface utilisateur qui est utilisé pour la saisie et la modification de texte.

![Exemple de texte de modification](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[Galerie](~/android/user-interface/controls/gallery.md)

`Gallery` est un widget de disposition qui est utilisé pour afficher des éléments dans une liste à défilement horizontal ; il positionne la sélection actuelle dans le centre de la vue.

![Galerie d’exemples](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[Barre de navigation](~/android/user-interface/controls/navigation-bar.md)

Le *barre de Navigation* fournit des contrôles de navigation sur les appareils qui n’incluent pas de boutons matériels pour **accueil**, **retour**, et **Menu**.

![Barre de Navigation d’exemple](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[Sélecteurs](~/android/user-interface/controls/pickers/index.md)

*Sélecteurs* sont des éléments d’interface utilisateur qui autorise l’utilisateur à choisir une date ou une heure à l’aide des boîtes de dialogue qui sont fournis par Android.

![Sélecteur d’exemple](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[Menu contextuel](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` est utilisé pour afficher les menus contextuels qui sont attachés à une vue particulière.

![Menu contextuel d’exemple](images/popup-menu.png)


## <a name="ratingbarandroiduser-interfacecontrolsratingbarmd"></a>[RatingBar](~/android/user-interface/controls/ratingbar.md)

Un `RatingBar` est un élément d’interface utilisateur qui affiche une évaluation dans étoiles.

![Exemple d’un RatingBar](ratingbar-images/01-ratingbar.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[Spinner](~/android/user-interface/controls/spinner.md)

`Spinner` est un élément d’interface utilisateur qui offre un moyen rapide de sélectionner une valeur à partir d’un ensemble. Il est simmilar à une liste déroulante. 

![Compteur d’exemple](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[Commutateur](~/android/user-interface/controls/switch.md)

`Switch` est un élément d’interface utilisateur qui permet à un utilisateur de basculer entre deux États, comme sous ou hors tension. Le `Switch` valeur par défaut est OFF.

![Commutateur de l’exemple](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` est une vue qui utilise le rendu 2D à accélération matérielle pour permettre à une vidéo ou un flux de contenu OpenGL à afficher.

![Exemple de vue de Texture](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[ToolBar](~/android/user-interface/controls/tool-bar/index.md)

Le `Toolbar` widget (introduite dans Android 5.0 Lollipop) peut être considéré comme une généralisation de l’interface de barre d’action &ndash; il est destiné à remplacer la barre d’action. Le `Toolbar` peuvent être utilisées partout dans une disposition de l’application, et il est plus personnalisable que la barre d’action.

![Barre d’outils de l’exemple](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

Le `ViewPager` est un gestionnaire de disposition qui permet à l’utilisateur à renverser left et right via les pages de données.

![Exemple ViewPager](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` est un élément d’interface utilisateur qui vous permet de créer votre propre fenêtre d’affichage des pages web (ou même développer un navigateur terminé).

![Exemple de vue Web](images/web-view.png)

