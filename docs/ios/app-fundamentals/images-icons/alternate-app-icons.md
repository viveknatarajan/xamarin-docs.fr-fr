---
title: "Icônes de l’autre application"
description: "Cet article décrit à l’aide des icônes de l’autre application dans Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: ff24a1411a7ddf2ca78c7997f1dc37744013ece4
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="alternate-app-icons"></a>Icônes de l’autre application

_Cet article décrit à l’aide des icônes de l’autre application dans Xamarin.iOS._

Apple a ajouté plusieurs améliorations à iOS 10.3 qui permettent à une application de gérer son icône :

 - `ApplicationIconBadgeNumber` -Obtient ou définit le badge de l’icône de l’application dans le Springboard.
 - `SupportsAlternateIcons` -If `true` l’application a un autre jeu d’icônes.
 - `AlternateIconName` -Retourne le nom de l’icône de remplacement actuellement sélectionné ou `null` si vous utilisez l’icône du premier.
 - `SetAlternameIconName` -Utilisez cette méthode pour basculer l’icône de l’application sur l’icône de remplacement donné.

![](alternate-app-icons-images/icons04.png "Un échantillon d’alerte lorsqu’une application modifie son icône")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Ajouter des icônes d’autre à un projet Xamarin.iOS

Pour permettre à une application basculer vers une autre icône, une collection d’images d’icône sera doivent être incluses dans le projet d’application Xamarin.iOS. Ces images ne peut pas être ajoutés au projet à l’aide du type `Assets.xcassets` (méthode), ils doivent être ajoutés à la **ressources** dossier directement.

Effectuez ce qui suit :

1. Sélectionnez les images d’icône obligatoire dans un dossier, sélectionnez tous les et faites-les glisser jusqu'à la **ressources** dossier dans le **l’Explorateur de solutions**:

    ![](alternate-app-icons-images/icons00.png "Sélectionnez les images des icônes à partir d’un dossier")

2. Lorsque vous y êtes invité, sélectionnez **copie**, **utilisent la même action pour tous les fichiers sélectionnés** et cliquez sur le **OK** bouton :

    ![](alternate-app-icons-images/icons02.png "Le fichier à ajouter à la boîte de dialogue dossier")

3. Le **ressources** dossier doit ressembler à ce qui suit s’est terminée :

    ![](alternate-app-icons-images/icons01.png "Le dossier de ressources doit ressembler à ceci")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Modification du fichier Info.plist

Avec les images requises ajoutées à la **ressources** dossier, le [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) clé devra être ajouté au projet **Info.plist** fichier. Cette clé définit le nom de l’icône Nouveau et les images qui le composent.

Effectuez ce qui suit :

1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier **Info.plist** pour l’ouvrir et le modifier.
2. Basculez vers le **Source** vue.
3. Ajouter un **regrouper les icônes** de clé et de laisser le **Type** la valeur **dictionnaire**.
4. Ajouter un `CFBundleAlternateIcons` de clé et définissez la **Type** à **dictionnaire**.
5. Ajouter un `AppIcons2` de clé et définissez la **Type** à **dictionnaire**. Ce sera le nom du nouvel ensemble d’icône autre application.
6. Ajouter un `CFBundleIconFiles` de clé et définissez la **Type** à **tableau**
7. Ajouter une nouvelle chaîne à la `CFBundleIconFiles` tableau pour chaque fichier d’icône en laissant l’extension et le `@2x`, `@3x`, les suffixes etc. (exemple `100_icon`). Répétez cette étape pour chaque fichier qui compose le jeu d’icônes de remplacement.
8. Ajouter un `UIPrerenderedIcon` clé pour le `AppIcons2` dictionnaire, définissez la **Type** à **booléenne** et à la valeur **non**.
9. Enregistrez les modifications dans le fichier.

Résultant **Info.plist** fichier doit ressembler à ce qui suit s’est terminée :

![](alternate-app-icons-images/icons03.png "Le fichier Info.plist complet")

Ou, comme cela est si ouvert dans un éditeur de texte :

```xml
<key>CFBundleIcons</key>
<dict>
    <key>CFBundleAlternateIcons</key>
    <dict>
        <key>AppIcon2</key>
        <dict>
            <key>CFBundleIconFiles</key>
            <array>
                <string>100_icon</string>
                <string>114_icon</string>
                <string>120_icon</string>
                <string>144_icon</string>
                <string>152_icon</string>
                <string>167_icon</string>
                <string>180_icon</string>
                <string>29_icon</string>
                <string>40_icon</string>
                <string>50_icon</string>
                <string>512_icon</string>
                <string>57_icon</string>
                <string>58_icon</string>
                <string>72_icon</string>
                <string>76_icon</string>
                <string>80_icon</string>
                <string>87_icon</string>
            </array>
            <key>UIPrerenderedIcon</key>
            <false/>
        </dict>
    </dict>
</dict>
```

<a name="Managing-the-Apps-Icon" />

## <a name="managing-the-apps-icon"></a>Icône de l’application de la gestion des 

Avec les images d’icône inclus dans le projet Xamarin.iOS et **Info.plist** fichier correctement configuré, le développeur peut utiliser une des nombreuses nouvelles fonctionnalités ajoutées à iOS 10.3 pour contrôler l’icône de l’application.

Le `SupportsAlternateIcons` propriété de la `UIApplication` classe permet au développeur de voir si une application prend en charge les autres icônes. Exemple :

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

Le `ApplicationIconBadgeNumber` propriété de la `UIApplication` classe permet au développeur d’obtenir ou définir le nombre de badge actuelle de l’icône d’application dans le Springboard. La valeur par défaut est zéro (0). Exemple :

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

Le `AlternateIconName` propriété de la `UIApplication` classe permet au développeur d’obtenir le nom de l’icône d’application autre actuellement sélectionné ou la valeur `null` si l’application est à l’aide de l’icône du premier. Exemple :

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

Le `SetAlternameIconName` propriété de la `UIApplication` classe permet au développeur de modifier l’icône d’application. Passez le nom de l’icône pour sélectionner ou `null` pour revenir à l’icône du premier. Exemple :

```csharp
partial void UsePrimaryIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName (null, (err) => {
        Console.WriteLine ("Set Primary Icon: {0}", err);
    });
}

partial void UseAlternateIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName ("AppIcon2", (err) => {
        Console.WriteLine ("Set Alternate Icon: {0}", err);
    });
}
```

Lorsque l’application est exécutée et que l’utilisateur sélectionner une autre icône, une alerte à ce qui suit s’affiche :

![](alternate-app-icons-images/icons04.png "Un échantillon d’alerte lorsqu’une application modifie son icône")

Si l’utilisateur bascule sur l’icône du premier, il s’affichera une alerte comme suit :

![](alternate-app-icons-images/icons05.png "Un échantillon d’alerte lorsqu’une application passe à l’icône principal")

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert Ajout d’icônes de l’autre application à un projet de Xamarin.iOS et leur utilisation à l’intérieur de l’application.



## <a name="related-links"></a>Liens associés

- [iOSTenThree exemple](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
