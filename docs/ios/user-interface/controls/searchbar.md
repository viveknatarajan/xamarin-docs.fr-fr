---
title: Barre de recherche
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 4ea39f6f935adba2c035884c625c57c0039b1165
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="search-bar"></a>Barre de recherche

Le UISearchBar est utilisé pour une recherche dans une liste de valeurs. 

Elle contient trois composants principaux : 

- Un champ utilisé pour entrer du texte. Les utilisateurs peuvent utiliser cette option pour entrer leur terme de recherche.
- Bouton Effacer, supprimer n’importe quel texte à partir du champ de recherche.
- Un bouton Annuler pour quitter la fonction de recherche.

![Barre de recherche](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>Implémentation de la barre de recherche

Pour implémenter la recherche du début de barre en instanciant un :

```csharp
searchBar = new UISearchBar();
```

Et le placer ensuite. L’exemple ci-dessous montre comment le placer dans la barre de navigation ou dans le HeaderView d’une Table :

```csharp
NavigationItem.TitleView = searchBar;

\\or

TableView.TableHeaderView = searchBar;
```

Définition des propriétés sur la barre de recherche :

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![Propriétés de la barre de recherche](searchbar-images/image6.png)

Déclencher la `SearchButtonClicked` événement lorsque le bouton de recherche est enfoncé. Cela permet d’appeler votre logique de recherche :

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

Pour plus d’informations sur la gestion de la présentation de la barre de recherche et les résultats de recherche, reportez-vous à la [recherche contrôleur ](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/) recette.

## <a name="using-the-search-bar-in-the-designer"></a>À l’aide de la barre de recherche dans le Concepteur

Le concepteur propose deux options pour l’implémentation d’une barre de recherche dans le Concepteur

- Barre de recherche
- Barre de recherche avec le contrôleur d’affichage de recherche (déconseillé)

![Contrôles de barre de recherche dans le Concepteur](searchbar-images/image2.png)

Utilisez le panneau de propriété pour définir des propriétés sur la barre de recherche

![Concepteur de propriétés de barre de recherche](searchbar-images/image3.png)

Ces propriétés sont expliquées ci-dessous :

- **Invite de texte, l’espace réservé,** – ces propriétés sont utilisées pour proposer et indiquer comment les utilisateurs doivent utiliser la barre de recherche. Par exemple, si votre application affiche une liste des magasins, vous pouvez utiliser la propriété prompt pour signaler que les utilisateurs peuvent « entrer une ville, le nom de l’article ou le Code postal »
- **Rechercher le Style** – vous pouvez définir la barre de recherche pour être **Prominent** ou **minimale**. À l’aide de l’importante colorer tout le reste à l’écran, à l’exception de la recherche de la barre, provoquant le focus doit être dessiné à la barre de recherche. La barre de recherche de style minimale s’intégreront avec son environnement.
- **Fonctionnalités** : l’activation de ces propriétés affiche uniquement l’élément d’interface utilisateur. La fonctionnalité doit être implémentée pour ces en déclenchant l’événement correct comme indiqué dans le [docs de l’API de barre de recherche](https://developer.xamarin.com/api/type/UIKit.UISearchBar/)
    - Affiche les résultats de recherche / bouton signets : affiche une icône de résultats de la recherche ou les signets dans la barre de recherche
    - Affiche le bouton Annuler – permet aux utilisateurs de quitter la fonction de recherche. Il est recommandé que cette option est sélectionnée.
    - Indique à la barre d’outils que cela permet aux utilisateurs de limiter l’étendue de la recherche. Par exemple, lors de la recherche dans l’application de musique l’utilisateur peut sélectionner qu’ils souhaitent rechercher Apple musique ou leur bibliothèque pour une chanson particulière ou d’un artiste. Pour afficher les différentes options, ajoutez un tableau de titres pour le **ScopeBarTitles** propriété.
    ![Titres de portée de barre de recherche](searchbar-images/image4.png)

- **Comportement du texte** – ces options sont utilisées pour adresser la façon dont l’entrée d’utilisateur est mis en forme lorsqu’il tape. Mise en majuscules définira le début de chaque mot ou une phrase, ou tous les caractères en majuscules. Correction et la correction orthographique avec invitent l’utilisateur avec des suggestions de mots en tapant.
- **Clavier** : contrôle le style de clavier affiché pour l’entrée, et par conséquent, les clés sont disponibles sur le clavier. Cela inclut le pavé numérique, remplissage du téléphone, par courrier électronique, URL, ainsi que d’autres options.
- **Apparence** : contrôle le style d’apparence du clavier et seront soit sombre ou clair à thème.
- **Touche retour** : modifier l’étiquette sur la touche Retour afin de mieux refléter l’action à entreprendre. Valeurs prises en charge incluent Go, jointure, suivant, itinéraire, terminé et la recherche.
- **Sécuriser** – indique si l’entrée est masquée (comme une entrée de mot de passe).

## <a name="related-links"></a>Liens associés

- [Contrôleur de recherche](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
