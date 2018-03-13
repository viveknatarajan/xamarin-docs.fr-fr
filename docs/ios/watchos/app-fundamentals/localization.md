---
title: Utilisation de la localisation
description: Adaptation de vos applications watchOS pour plusieurs langues
ms.topic: article
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 9ad3499a232e5f2b2ef362f772ed0197e71e6bee
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-localization"></a>Utilisation de la localisation

_Adaptation de vos applications watchOS pour plusieurs langues_

![](localization-images/both-languages-sml.png "Apple Watch affichage du contenu localisé")

les applications watchOS sont localisées dans les méthodes d’e/s standard :

- À l’aide de **ID de localisation** sur les éléments de la table de montage séquentiel,
- **.Strings** fichiers associés à la table de montage séquentiel, et
- **Localizable.Strings** fichiers texte utilisé dans le code.

Les animations de la valeur par défaut et les ressources se trouvent dans un **Base** répertoire et les traductions spécifiques au langage et autres ressources sont stockées dans **.lproj** répertoires.
iOS et le système d’exploitation espion utiliseront automatiquement sélection de la langue de l’utilisateur à charger les chaînes et les ressources.

Car une application Apple Watch comporte deux parties : l’application de surveillance et Extension Watch - chaîne localisée des ressources sont nécessaires dans deux emplacements, selon la façon dont ils sont utilisés.

Le texte localisé et les ressources seront *différents* dans l’application de surveillance et de l’extension de surveillance.

## <a name="watch-app"></a>Surveiller l’application

L’application de surveillance contient la table de montage séquentiel qui décrit l’interface utilisateur de l’application. Tous les contrôles (tels que `Label` et `Image`) que la prise en charge de localisation ont un **ID de localisation**.

Chaque spécifiques au langage **.lproj** répertoire doit contenir **.strings** fichiers avec les traductions pour chaque élément (à l’aide de la **ID de localisation**), ainsi que des images référencé par le plan conceptuel.

## <a name="watch-extension"></a>Extension de surveillance

L’extension de surveillance est sur lequel s’exécute le code de votre application. Tout texte qui s’affiche à l’utilisateur à partir du code doit être localisée dans l’extension et non dans l’application de surveillance.

L’extension doit aussi contenir spécifiques au langage **.lproj** répertoires, mais la **.strings** fichiers nécessitent uniquement des traductions pour le texte qui est utilisé dans votre code.

## <a name="globalizing-the-watch-solution"></a>Globalisation de la Solution de surveillance

La globalisation est le processus de création d’une application localisable.
Pour les applications de surveillance cela signifie que la conception de la table de montage séquentiel avec des longueurs de texte différentes à l’esprit, vous être assuré de chaque disposition de l’écran ajuste appropriée selon le texte s’affiche. Vous devez également voulez assurer toutes les chaînes référencés dans le code d’extension espion peuvent être traduites à l’aide de la `LocalizedString` (méthode).

### <a name="watch-app"></a>Surveiller l’application

Par défaut, l’application de surveillance n’est pas configurée pour la localisation. Vous devez déplacer le fichier d’animation par défaut et créer certains autres répertoires pour vos traductions :

1. Créer **Base.lproj** active et déplacer le **Interface.storyboard** dans celui-ci.

2. Créer  **<language>.lproj** répertoires pour chaque langue que vous souhaitez prendre en charge.

3. Le **.lproj** répertoires doivent contenir un **Interface.strings** fichier texte (le nom de fichier doit correspondre au nom de la storboard). Vous pouvez éventuellement placer toutes les images qui requièrent la localisation dans ces répertoires.

Le projet d’application espion ressemble à ceci une fois ces modifications ont été apportées (uniquement en anglais et espagnol language fichiers ont été ajoutés) :

  ![](localization-images/watchapp-solution.png "Le projet d’application Espion avec les fichiers de langue anglais et espagnol")

#### <a name="storyboard-text"></a>Texte de la table de montage séquentiel

Lorsque vous modifiez le plan conceptuel, sélectionnez chaque élément et l’avis de le **ID de localisation** qui s’affiche dans le **propriétés** remplissage :

  [![](localization-images/storyboard-sml.png "L’ID de localisation qui s’affiche dans la zone de propriétés")](localization-images/storyboard.png#lightbox)

Dans le **Base.lproj** dossier, créez des paires clé-valeur comme indiqué ci-dessous, où la clé est formée par la **ID de localisation** et un nom de propriété sur le contrôle, jointe à un point (`.`).

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

Notez que dans cet exemple qui un **ID de localisation** peut être une simple chaîne numérique (par exemple). « 0 », « 1 », etc.) ou une chaîne plus complexe (par exemple, « AgC-eL-Hgc »). `Label` les contrôles ont un `Text` propriété et `Button`s ont un `Title` propriété se reflète dans la configuration de leurs valeurs localisées sont - veillez à utiliser le nom de propriété minuscules comme indiqué dans l’exemple ci-dessus.

Lorsque le plan conceptuel est rendu sur la surveillance, les valeurs correctes seront extrait automatiquement et affichés en fonction de la langue sélectionnée par l’utilisateur.

#### <a name="storyboard-images"></a>Images d’animation

L’exemple de solution inclut également un  **gradient@2x.png**  image dans chaque dossier de langue. Cette image peut être différente pour chaque langue (par exemple). Il peut avoir incorporé texte nécessitant une conversion, ou utilisez localisée iconographie).

Définissez simplement l’image **Image** propriété dans la table de montage séquentiel et de l’image appropriée s’affichera sur la surveillance en fonction de la langue sélectionnée par l’utilisateur.

![](localization-images/storyboard-image.png "Définir les images de la propriété Image dans la table de montage séquentiel")

Remarque : étant donné que toutes les observations Apple ont rétine, uniquement le  **@2x**  version de l’image est obligatoire. Vous n’avez pas besoin de spécifier  **@2x**  dans le plan conceptuel.

### <a name="watch-extension"></a>Extension de surveillance

L’extension de surveillance requiert une structure de répertoires similaire pour prendre en charge la localisation, cependant il n’existe aucun plan conceptuel. Les chaînes localisées dans l’extension sont uniquement ceux référencés par le code c#.

![](localization-images/watchextension-solution.png "La structure de répertoire d’extension espion pour prendre en charge la localisation")

#### <a name="strings-in-code"></a>Chaînes dans le Code

Le **Localizable.strings** fichier a une structure légèrement différente que si elle a été associé à un plan conceptuel. Dans ce cas, nous pouvons choisir n’importe quelle chaîne « clé » ; Les recommandations d’Apple sont d’utiliser une clé qui reflète le texte qui sera affiché dans la langue par défaut :

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

Le `NSBundle.MainBundle.LocalizedString` méthode est utilisée pour résoudre les chaînes dans leurs équivalents traduits, comme indiqué dans le code ci-dessous.

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>Images dans le Code

Les images sont remplies par le code peuvent être définies de deux manières.

1. Vous pouvez modifier un `Image` contrôle en définissant sa valeur le nom de chaîne d’une image qui déjà existe dans l’application de surveillance, par exemple,

  ```csharp
  displayImage.SetImage("gradient"); // image in Watch App (as shown above)
  ```

2. Vous pouvez déplacer une image à partir de l’extension pour la surveillance à l’aide de `FromBundle` et l’application va choisir automatiquement l’image appropriée pour la sélection de la langue de l’utilisateur. Dans l’exemple de solution, il existe une image  **language@2x.png**  dans chaque langue de dossier et il est affiché sur `DetailController` utilisant le code suivant :

  ```csharp
  using (var image = UIImage.FromBundle ("language")) {
    displayImage.SetImage (image);
  }
  ```

  Notez que vous n’avez pas besoin de spécifier le  **@2x**  lorsque vous faites référence au nom de fichier de l’image.

La seconde méthode est également applicable si vous téléchargez une image à partir d’un serveur distant à restituer sur l’observation ; Toutefois dans ce cas vous devez vous assurer que l’image que vous téléchargez est correctement localisé en fonction des préférences de l’utilisateur.

## <a name="localization"></a>Localisation

Une fois que vous avez configuré votre solution, traducteurs allez devoir traiter votre **.strings** fichiers et les images pour chaque langue que vous souhaitez prendre en charge.

Vous pouvez créer autant **.lproj** répertoires en tant que vous avez besoin de (une pour chaque langue prise en charge). Ils sont nommés à l’aide des codes de langue, tel que **en**, **es**, **de**, **ja**, **pt-BR**, etc. (pour l’anglais Espagnol, allemand, japonais et portugais (Brésil) respectivement).

L’exemple joint utilise (généré par l’ordinateur) des traductions pour illustrer comment localiser une application watchOS.

### <a name="watch-app"></a>Surveiller l’application

Ces valeurs sont utilisées pour traduire l’interface utilisateur définie dans le plan conceptuel de l’application de surveillance. Le *clé* valeur est une combinaison de chaque contrôle de table de montage séquentiel **ID de localisation** et la propriété en cours de conversion.

Il est recommandé d’ajouter des commentaires qui contient le texte d’origine dans le fichier afin que les convertisseurs de savoir quelle doit être la traduction.

#### <a name="eslprojinterfacestrings"></a>es.lproj/Interface.strings

Les chaînes de langue Espagnol (traduits) pour le plan conceptuel sont indiquées ci-dessous. Il est utile ajouter des commentaires pour chaque ligne, car il est difficile de savoir ce que le **ID de localisation** fait référence à sinon :

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>Extension de surveillance

Ces valeurs sont utilisées dans le code pour convertir les informations avant d’être affichées à l’utilisateur. Le *clé* est sélectionnée par le développeur lors de leur écriture de code et contient généralement la chaîne réelle doivent être converties.

#### <a name="eslprojlocalizablestrings-file"></a>fichier de es.lproj/Localizable.strings

Les chaînes de langage Spansish (traduit) :

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>Test

La méthode pour modifier les préférences de langue diffère entre les périphériques physiques et le simulateur.

### <a name="simulator"></a>Simulateur

Sur le simulateur, sélectionnez la langue à tester à l’aide de l’e/s **paramètres** application (l’icône grise vitesses dans l’écran d’accueil simulateur).

  ![](localization-images/sim-settings-sml.png "Les paramètres de localisation d’application iOS")

### <a name="watch-device"></a>Dispositif de surveillance

Lors du test avec un espion, modifier la langue de la surveillance dans le **Apple Watch** application sur l’iPhone apparié.

  ![](localization-images/phone-settings-sml.png "Modifier la langue de la surveillance dans l’application de l’Apple Watch sur l’iPhone apparié")



## <a name="related-links"></a>Liens associés

- [WatchLocalization (exemple)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchLocalization/)
