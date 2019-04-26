---
title: Remplacement de la barre d’action
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/27/2018
ms.openlocfilehash: 9e9fa1e2651661670f89baac7fcd438b3d14bfb3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61200915"
---
# <a name="replacing-the-action-bar"></a>Remplacement de la barre d’action

## <a name="overview"></a>Vue d'ensemble

Le plus souvent utilisé pour le `Toolbar` consiste à remplacer la barre d’action par défaut avec un personnalisé `Toolbar` (création d’un nouveau projet Android, il utilise la barre d’action par défaut). Étant donné que le `Toolbar` offre la possibilité d’ajouter des logos de marques, titres, les éléments de menu, boutons de navigation et des vues personnalisées de même à la section de barre d’application d’une activité l’interface utilisateur, il offre une mise à niveau importante sur la barre d’action par défaut.

Pour remplacer la barre d’action par défaut d’une application avec un `Toolbar`: 

1.  Créer un thème personnalisé et modifier les propriétés de l’application afin qu’il utilise ce nouveau thème. 

2.  Désactiver la `windowActionBar` d’attribut dans le thème personnalisé et activer la `windowNoTitle` attribut.

3.  Définir une disposition pour le `Toolbar`.

4.  Inclure le `Toolbar` la disposition de l’activité **Main.axml** fichier de disposition. 

5.  Ajouter du code à l’activité `OnCreate` méthode pour localiser le `Toolbar` et appelez `SetActionBar` pour installer le `ToolBar` en tant que la barre d’action.

Les sections suivantes expliquent ce processus en détail. Une application simple est créée et sa barre d’action est remplacé par un texte personnalisé `Toolbar`. 



## <a name="start-an-app-project"></a>Démarrer un projet d’application

Créer un nouveau projet Android appelé **ToolbarFun** (consultez [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) pour plus d’informations sur la création d’un nouveau projet Android). Une fois ce projet est créé, la valeur les niveaux d’API Android minimale et cible **Android 5.0 (API niveau 21 - Lollipop)** ou version ultérieure. Pour plus d’informations sur la définition des niveaux de version d’Android, consultez [présentation des niveaux d’API Android](~/android/app-fundamentals/android-api-levels.md). Lorsque l’application est créée et s’exécuter, il affiche la barre d’action par défaut, comme indiqué dans cette capture d’écran :

[![Capture d’écran de la barre d’action par défaut](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)



## <a name="create-a-custom-theme"></a>Créer un thème personnalisé

Ouvrez le **ressources/valeurs** directory et créer un nouveau fichier appelé **styles.xml**. Remplacez son contenu par le code XML suivant : 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="@android:style/Theme.Material.Light.DarkActionBar">
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowActionBar">false</item>
    <item name="android:colorPrimary">#5A8622</item>
  </style>
</resources>
```

Ce fichier XML définit un nouveau thème personnalisé appelé **MyTheme** qui est basé sur le **Theme.Material.Light.DarkActionBar** thème dans l’interface (lollipop). Le `windowNoTitle` attribut a la valeur `true` pour masquer la barre de titre : 

```xml
<item name="android:windowNoTitle">true</item>
```

Pour afficher la barre d’outils personnalisée, la valeur par défaut `ActionBar` doit être désactivé : 

```xml
<item name="android:windowActionBar">false</item>
```

Un vert olive `colorPrimary` paramètre est utilisé pour la couleur d’arrière-plan de la barre d’outils : 
 
```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>Appliquer le thème personnalisé

Modifier **Properties/Androidmanifest.XML** et ajoutez le code suivant `android:theme` attribut le `<application>` élément afin que l’application utilise le `MyTheme` thème personnalisé : 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

Pour plus d’informations sur l’application d’un thème personnalisé à une application, consultez [à l’aide des thèmes personnalisés](~/android/user-interface/material-theme.md#customtheme). 



## <a name="define-a-toolbar-layout"></a>Définir une disposition de barre d’outils

Dans le **ressources/disposition** répertoire, créez un nouveau fichier appelé **toolbar.xml**. Remplacez son contenu par le code XML suivant : 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?android:attr/actionBarSize"
    android:background="?android:attr/colorPrimary"
    android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"/>
```

Ce fichier XML définit personnalisé `Toolbar` qui remplace la barre d’action par défaut. La hauteur minimale de la `Toolbar` est défini sur la taille de la barre d’action qu’il remplace : 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

La couleur d’arrière-plan de la `Toolbar` est défini sur la couleur vert olive définie précédemment dans **styles.xml**:

```csharp
android:background="?android:attr/colorPrimary"
```

Interface (lollipop), à partir de la `android:theme` attribut peut être utilisé pour définir le style une vue spécifique. Le `ThemeOverlay.Material` thèmes introduites dans l’interface (lollipop) vous permettent de superposer la valeur par défaut `Theme.Material` thèmes, en remplaçant les attributs pertinents pour les rendre clair ou sombre. Dans cet exemple, le `Toolbar` utilise un thème foncé afin que son contenu est de couleur claire : 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

Ce paramètre est utilisé afin que les éléments de menu contraste avec la couleur d’arrière-plan plus sombre.



## <a name="include-the-toolbar-layout"></a>Inclure la barre d’outils disposition

Modifier le fichier de disposition **Resources/layout/Main.axml** et remplacez son contenu par le code XML suivant :

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
</RelativeLayout>
```

Cette disposition inclut le `Toolbar` définies dans **toolbar.xml** et utilise un `RelativeLayout` pour spécifier que le `Toolbar` doit être placé au sommet de l’interface utilisateur (au-dessus du bouton). 



## <a name="find-and-activate-the-toolbar"></a>Rechercher et d’activer la barre d’outils

Modifier **MainActivity.cs** et ajoutez le code suivant à l’aide d’instruction :

```csharp
using Android.Views;
```

En outre, ajoutez les lignes de code suivantes à la fin de la `OnCreate` méthode :

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

Ce code recherche les `Toolbar` et appelle `SetActionBar` afin que le `Toolbar` prendra sur les caractéristiques de barre d’action par défaut. Le titre de la barre d’outils est modifié pour **ma barre d’outils**. Comme indiqué dans cet exemple de code, le `ToolBar` peut être directement référencés comme une barre d’action. Compiler et exécuter cette application &ndash; personnalisée `Toolbar` est affiché au lieu de la barre d’action par défaut : 

[![Capture d’écran de la barre d’outils personnalisée avec le modèle de couleurs vert](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

Notez que le `Toolbar` le style indépendamment de la `Theme.Material.Light.DarkActionBar` thème qui est appliqué au reste de l’application. 

Si une exception se produit pendant l’exécution de l’application, consultez le [dépannage](#troubleshooting) section ci-dessous.

 
## <a name="add-menu-items"></a>Ajouter des éléments de Menu 

Dans cette section, les menus sont ajoutés à la `Toolbar`. La zone supérieure droite de la `ToolBar` est réservé pour les éléments de menu &ndash; chaque élément de menu (également appelé un *élément action*) peut réaliser une action au sein de l’activité en cours, ou elle peut effectuer une action pour le compte de l’application entière. 

Pour ajouter des menus pour le `Toolbar`: 

1.  Ajouter des icônes de menu (le cas échéant) à la `mipmap-` dossiers de projet d’application. Google fournit un ensemble d’icônes du menu gratuit sur le [icônes Material](https://design.google.com/icons/) page. 

2.  Définir le contenu des éléments de menu en ajoutant un nouveau fichier de ressource de menu sous **/menu ressources**. 

3.  Implémentez le `OnCreateOptionsMenu` méthode de l’activité &ndash; cette méthode augmente les éléments de menu. 

4.  Implémentez le `OnOptionsItemSelected` méthode de l’activité &ndash; cette méthode effectue une action lorsque l’utilisateur appuie sur un élément de menu. 

Les sections suivantes illustrent ce processus en détail en ajoutant **modifier** et **enregistrer** des éléments de menu à personnalisée `Toolbar`. 



### <a name="install-menu-icons"></a>Installer les icônes du Menu

Poursuivre le `ToolbarFun` exemple d’application, ajoutez des icônes de menu au projet d’application. Télécharger [icônes de barre d’outils](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true), décompressez et copiez le contenu de l’extrait *mipmap -* dossiers au projet *mipmap -* dossiers sous **ToolbarFun / Ressources** et inclure chaque fichier d’icône ajouté dans le projet.


### <a name="define-a-menu-resource"></a>Définir une ressource de Menu

Créer un nouveau **menu** sous-répertoire **ressources**. Dans le **menu** sous-répertoire, créer un nouveau fichier de ressource de menu appelé **top_menus.xml** et remplacez son contenu par le code XML suivant : 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       android:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       android:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       android:showAsAction="never"
       android:title="Preferences" />
</menu>
```

Ce code XML crée trois éléments de menu :

-   Un **modifier** élément de menu qui utilise le `ic_action_content_create.png` icône (un crayon). 

-   Un **enregistrer** élément de menu qui utilise le `ic_action_content_save.png` icône (une disquette). 

-   Un **préférences** élément de menu qui ne possède pas d’icône.

Le `showAsAction` les attributs de la **modifier** et **enregistrer** les éléments de menu `ifRoom` &ndash; avec ce paramètre, ces éléments de menu à afficher dans le `Toolbar` s’il existe suffisamment d’espace pour les afficher. Le **préférences** ensembles d’éléments de menu `showAsAction` à `never` &ndash; cela entraîne le **préférences** menu apparaît à la *dépassement de capacité* menu (trois points verticaux). 


### <a name="implement-oncreateoptionsmenu"></a>Implémenter OnCreateOptionsMenu

Ajoutez la méthode suivante à **MainActivity.cs**:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android appelle la `OnCreateOptionsMenu` méthode afin que l’application peut spécifier la ressource de menu pour une activité. Dans cette méthode, le **top_menus.xml** ressource est agrandie dans le passé `menu`. Ce code provoque la nouvelle **modifier**, **enregistrer**, et **préférences** des éléments de menu à afficher dans le `Toolbar`. 



### <a name="implement-onoptionsitemselected"></a>Implémenter OnOptionsItemSelected

Ajoutez la méthode suivante à **MainActivity.cs**:

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

Lorsqu’un utilisateur appuie sur un élément de menu, Android appelle la `OnOptionsItemSelected` méthode et passe dans l’élément de menu qui a été sélectionné. Dans cet exemple, l’implémentation affiche simplement un toast pour indiquer que l'on a cliqué sur quel élément de menu. 

Générer et exécuter `ToolbarFun` pour voir les nouveaux éléments de menu dans la barre d’outils. Le `Toolbar` affiche maintenant les trois icônes du menu comme indiqué dans cette capture d’écran : 

[![Diagramme illustrant des emplacements de modifier, enregistrer et d’éléments de menu de dépassement de capacité](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

Lorsqu’un utilisateur appuie sur le **modifier** élément de menu, un toast s’affiche pour indiquer que le `OnOptionsItemSelected` méthode a été appelée : 

[![Capture d’écran de Toast affiché lors de la modification d’un élément est activé par un clic](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

Lorsqu’un utilisateur appuie sur le menu de dépassement de capacité, le **préférences** élément de menu est affiché. En règle générale, les actions moins courantes doivent être placées dans le menu de dépassement de capacité &ndash; cet exemple utilise le menu de dépassement de capacité pour **préférences** , car il n’est pas aussi souvent utilisé en tant que **modifier** et  **Enregistrer**: 

[![Élément de menu de capture d’écran de préférences s’affiche dans le menu de dépassement de capacité](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Pour plus d’informations sur les menus Android, consultez le développeur Android [Menus](https://developer.android.com/guide/topics/ui/menus.html) rubrique. 
 

## <a name="troubleshooting"></a>Résolution des problèmes

Les conseils suivants peuvent aider à déboguer les problèmes qui peuvent se produire lors du remplacement de la barre d’action par une barre d’outils.

### <a name="activity-already-has-an-action-bar"></a>Activité a déjà une barre d’Action

Si l’application n’est pas correctement configurée pour utiliser un thème personnalisé, comme expliqué dans [appliquer le thème personnalisé](#apply-the-custom-theme), l’exception suivante peut se produire lors de l’exécution de l’application :

![Erreur qui peut se produire lorsque le thème personnalisé n’est pas utilisé.](replacing-the-action-bar-images/03-theme-not-defined.png)

En outre, un message d’erreur semblable à celui-ci peut être généré : _Java.Lang.IllegalStateException: Cette activité a déjà une barre d’action fournie à l’intérieur de la fenêtre._ 

Pour corriger cette erreur, vérifiez que le `android:theme` d’attribut pour le thème personnalisé est ajouté à `<application>` (dans **Properties/Androidmanifest.XML**) comme décrit précédemment dans [appliquer le thème personnalisé](#apply-the-custom-theme). En outre, cette erreur peut se produire si le `Toolbar` mise en page ou un thème personnalisé n’est pas configuré correctement.


## <a name="related-links"></a>Liens associés

- [La barre d’outils de lollipop (exemple)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [La barre d’outils de AppCompat (exemple)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
