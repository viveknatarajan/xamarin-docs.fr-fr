---
title: Réglage des paramètres de mémoire de Java pour le concepteur Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 32d98efd644fb033785fbae0d9689494e42b2809
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Réglage des paramètres de mémoire de Java pour le concepteur Android

Les paramètres de mémoire par défaut qui sont utilisées lors du démarrage de la `java` traiter pour le concepteur Android risque d’être incompatible avec certaines configurations système.

En commençant par Xamarin Studio 5.7.2.7 (et version ultérieure, Visual Studio pour Mac) et Xamarin pour Visual Studio 3.9.344 ces paramètres peuvent être personnalisés par projet.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Nouvelles propriétés du concepteur Android et les options Java correspondantes

Les noms de propriété suivantes correspondent aux java indiquée [option de ligne de commande](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Ouvrez votre solution dans Visual Studio.

2.  Sélectionnez chaque projet Android, un par un dans l’Explorateur de solutions, cliquez sur [afficher tous les fichiers](https://msdn.microsoft.com/en-us/library/4afxey9h.aspx) deux fois sur chaque projet. Vous pouvez ignorer les projets qui ne contiennent pas les `.axml` les fichiers de mise en page. Cette étape garantit que chaque répertoire du projet contient un `.csproj.user` fichier.

3.  Quittez Visual Studio.

4.  Recherchez le `.csproj.user` fichier pour chacun des projets de l’étape 2.

5.  Modifier chaque `.csproj.user` fichier dans un éditeur de texte.

6.  Ajouter tout ou partie des nouvelles propriétés de mémoire concepteur Android au sein d’un `<PropertyGroup>` élément. Vous pouvez utiliser un existant `<PropertyGroup>` ou créez-en un. Voici un exemple complet `.csproj.user` fichier qui inclut tous les attributs de 3 est défini sur leurs valeurs par défaut :

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
       <PropertyGroup>
         <ProjectView>ProjectFiles</ProjectView>
       </PropertyGroup>
       <PropertyGroup>
         <AndroidDesignerJavaRendererMinMemory>128m</AndroidDesignerJavaRendererMinMemory>
         <AndroidDesignerJavaRendererMaxMemory>750m</AndroidDesignerJavaRendererMaxMemory>
         <AndroidDesignerJavaRendererPermSize>350m</AndroidDesignerJavaRendererPermSize>
       </PropertyGroup>
    </Project>
    ```

7.  Enregistrez et fermez toutes les mises à jour `.csproj.user` fichiers.

8.  Redémarrez Visual Studio et rouvrez votre solution.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1.  Ouvrez votre solution dans Visual Studio pour Mac garantir le répertoire de solution contient un `.userprefs` fichier.

2.  Quittez Visual Studio pour Mac.

3.  Recherchez le `.userprefs` fichier dans le répertoire de solution.

4.  Modifier la `.userprefs` fichier dans un éditeur de texte.

5.  Localisez l’élément XML existant au format suivant. La dernière partie de ce nom d’élément correspond au nom de votre projet : « AndroidApplication1 » dans cet exemple :

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6.  Si le `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` élément n’existe pas, créez-le n’importe où dans la forme `<Properties>` élément. Veillez à remplacer « AndroidApplication1 » avec le nom de votre projet.

7.  Ajouter tout ou partie des nouvelles propriétés de mémoire concepteur Android en tant qu’attributs sur l’élément. Voici un exemple complet `.userprefs` fichier qui inclut tous les attributs de 3 est défini sur leurs valeurs par défaut :

    ```xml
    <Properties StartupItem="AndroidApplication1\AndroidApplication1.csproj">
      <MonoDevelop.Ide.Workspace ActiveConfiguration="Debug" PreferredExecutionTarget="Android.SelectDevice" />
      <MonoDevelop.Ide.Workbench />
      <MonoDevelop.Ide.DebuggingService.Breakpoints>
        <BreakpointStore />
      </MonoDevelop.Ide.DebuggingService.Breakpoints>
      <MonoDevelop.Ide.DebuggingService.PinnedWatches />
      <MonoDevelop.Ide.ItemProperties.AndroidApplication1 AndroidDesignerJavaRendererMinMemory="128m" AndroidDesignerJavaRendererMaxMemory="750m" AndroidDesignerJavaRendererPermSize="350m" />
    </Properties>
    ```

8.  Répétez les étapes 5 à 7 pour chaque projet Android dans la solution qui contient des `.axml` des fichiers de mise en page. (Autrement dit, ajoutez-en un `<MonoDevelop.Ide.ItemProperties.ProjectName>` élément pour chaque projet.)

9.  Enregistrez et fermez le `.userprefs` fichier.

10. Redémarrez Visual Studio pour Mac et rouvrir votre solution.

-----

