---
title: "Entrée de texte"
ms.topic: article
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: ce1014616d0cf5f6cd5228d69976dfeca546b382
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="text-input"></a>Entrée de texte

Accepter une entrée de texte utilisateur est effectuée avec le `UITextField` pour les entrées de ligne simple et UITextView pour le texte modifiable contenant plusieurs lignes. Vous pouvez faire glisser un de ces contrôles sur un écran et double-cliquez sur pour définir le texte initial.

Les captures d’écran ci-dessous montrent les icônes pour ces contrôles, situés dans la zone de boîte à outils dans Visual Studio pour Mac :

 [ ![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png)

 [ ![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png)

Une fois que vous avez nommé la sortie et enregistré le fichier d’animation, Visual Studio pour Mac met à jour la `.designer.cs` classe partielle et vous pouvez ajouter du code c# qui référence le contrôle à votre fichier de classe. Chaque contrôle possède ses propres propriétés uniques et les événements qui sont accessibles dans votre code c#.

 <a name="UITextField" />


## <a name="uitextfield"></a>UITextField

Le `UITextField` contrôle est plus souvent utilisé pour accepter une seule ligne d’entrée de texte tel qu’un nom d’utilisateur ou le mot de passe. Certaines des options disponibles pour la personnalisation du contrôle sont affichés ici :

 [ ![](text-input-images/image15a.png "Propriétés de UITextField")](text-input-images/image15a.png)

Ces contrôles sont expliquées ci-dessous :

-  **Espace réservé** : ce paramètre est facultatif. Si la valeur, il est affiché lorsque le champ de texte est vide, généralement pour expliquer à l’utilisateur quelles entrée est attendue.
-  **Bouton Effacer** : ce paramètre détermine quand le bouton Effacer standard (le cercle gris avec (X)) s’affiche dans le champ de texte, comme un moyen de l’utilisateur d’effacer le texte rapidement. Il peut être masqué définitivement, définitivement visibles ou indiqué, selon si le champ est en cours de modification.
-  **Taille de police minimale** et **ajuster pour ajuster** – autorise la taille de police être ajusté automatiquement pour ajuster le texte plus longue et empêcher la troncation, mais limitée non inférieure à la taille spécifiée.
-  **Mise en majuscules** – s’il faut mettre automatiquement en majuscule mots, des phrases ou toutes les entrées.
-  **Correction** : indique si la vérification orthographique et les suggestions sont activées.
-  **Clavier** : contrôle le style de clavier affiché pour l’entrée, et par conséquent, les clés sont disponibles sur le clavier. Cela inclut le pavé numérique, remplissage du téléphone, par courrier électronique, URL, ainsi que d’autres options.
-  **Apparence** : contrôle le style d’apparence du clavier et seront soit sombre ou clair à thème.
-  **Touche retour** : modifier l’étiquette sur la touche Retour afin de mieux refléter l’action à entreprendre. Valeurs prises en charge incluent Go, jointure, suivant, itinéraire, terminé et la recherche.
-  **Sécuriser** – indique si l’entrée est masquée (comme une entrée de mot de passe).


Si un UITextField appelé `textfield1` a été ajoutée à un écran avec le concepteur, vous pouvez définir ou modifier ses propriétés en c# comme suit :

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

Fournit des énumérations Xamarin.iOS lorsque cela est approprié pour faciliter la sélectionner les paramètres de votre choix, comme le `UIKeyboardType` et `UIReturnKeyType` dans l’extrait de code ci-dessus.

### <a name="display-text-programmatically"></a>Afficher du texte par programme

Si vous ne souhaitez pas concevoir votre écran avec le concepteur ou si vous souhaitez ajouter dynamiquement du texte lors de l’exécution, vous pouvez créer et afficher un UITextField par programmation dans le `ViewDidLoad` (méthode) d’un contrôleur de la vue comme suit :

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />


## <a name="uitextview"></a>UITextView

Le `UITextView` contrôle peut être utilisé pour afficher le texte en lecture seule ou d’accepter l’entrée de texte multiligne. Il possède de nombreuses les mêmes options que le `UITextField` (telles que mise en majuscules, Correction, etc.).

 [ ![](text-input-images/image16a.png "Propriétés de UITextView")](text-input-images/image16a.png)

Propriétés spécifiques sont les suivantes :

-  **Comportement** : indique si le texte est modifiable ou en lecture seule.
-  **Détection** – détecte et convertit les données de chaîne en éléments interactifs tels que les numéros de téléphone qui peuvent déclencher un appel, les adresses qui deviennent des liens vers les cartes, les URL qui s’ouvrent dans Safari ou des dates et heures qui deviennent des événements dans le calendrier.


Si un UITextView a été ajouté à un écran avec le concepteur, vous pouvez définir ou modifier ses propriétés comme suit :

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```



## <a name="related-links"></a>Liens associés

- [Contrôles (exemple)](https://developer.xamarin.com/samples/Controls/)
