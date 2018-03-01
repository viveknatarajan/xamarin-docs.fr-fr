---
title: "Résumé du chapitre 6. Clics de bouton"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 44dbb4d2cdc573e721bb772fdd05d392c90b746a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-6-button-clicks"></a>Résumé du chapitre 6. Clics de bouton

Le [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) est la vue qui permet aux utilisateurs de lancer une commande. A `Button` est identifié par le texte (et éventuellement une image comme dans [chapitre 13, Bitmaps](chapter13.md)). Par conséquent, `Button` définit un grand nombre des mêmes propriétés en tant que `Label`:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/)
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontFamily/)
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/)
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontAttributes/)
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/)

`Button` définit également les trois propriétés qui régissent l’apparence de la bordure, mais la prise en charge de ces propriétés et leur indépendance mutuelle est spécifiques à la plateforme :

- [`BorderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderColor/) de type `Color`
- [`BorderWidth`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderWidth/) de type `Double`
- [`BorderRadius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/) de type `Double`

`Button` hérite également de toutes les propriétés de `VisualElement` et `View`, y compris `BackgroundColor`, `HorizontalOptions`, et `VerticalOptions`.

## <a name="processing-the-click"></a>Le traitement de la, cliquez sur

Le `Button` classe définit un [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) événement est déclenché lorsque l’utilisateur clique sur le `Button`. Le `Click` gestionnaire est de type `EventHandler`. Le premier argument est le `Button` génère l’événement de l’objet ; le deuxième argument est un `EventArgs` objet qui fournit des informations supplémentaires.

Le [ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) simple illustre `Clicked` gestion.

## <a name="sharing-button-clicks"></a>Bouton de partage clique sur

Plusieurs `Button` vues peuvent partager le même `Clicked` gestionnaire, mais le gestionnaire doit généralement déterminer quelle `Button` est chargé pour un événement particulier. Une approche consiste à stocker les différentes `Button` objets en tant que champs et de vérification de l’application déclenche l’événement dans le gestionnaire.

Le [ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) exemple illustre cette technique. Le programme montre également comment définir le [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) propriété d’un `Button` à `false` lorsque vous appuyez sur la `Button` n’est plus valide. A désactivé `Button` ne génère pas un `Clicked` événement.

## <a name="anonymous-event-handlers"></a>Gestionnaires d’événements anonyme

Il est possible de définir `Clicked` gestionnaires en tant que les fonctions lambda anonyme, comme le [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) illustre. Toutefois, gestionnaires anonymes ne peut pas être partagés sans code réflexion désordonnées.

## <a name="distinguishing-views-with-ids"></a>Distinction entre les vues avec des ID

Plusieurs `Button` objets peuvent également être distinguées en définissant le [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) propriété ou [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) propriété à un `string`. Cette propriété est définie par `Element` , mais elle n’est pas utilisée dans Xamarin.Forms. Elle est destinée à être utilisée uniquement par les programmes d’application.

Le [ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) exemple utilise le même gestionnaire d’événements pour toutes les clés de nombre 10 sur un pavé numérique et fait la distinction entre eux avec le `StyleId` propriété :

[![Capture d’écran de triple du pavé la plus simple](images/ch06fg04-small.png "calculatrice")](images/ch06fg04-large.png "calculatrice")

## <a name="saving-transient-data"></a>L’enregistrement de données temporaires

De nombreuses applications ont besoin pour enregistrer des données lorsqu’un programme est arrêté et recharger les données lorsque le programme démarre à nouveau. Le [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) classe définit plusieurs membres qui aident à votre programme enregistrer et restaurer les données temporaires :

- Le [ `Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) propriété est un dictionnaire avec `string` clés et `object` éléments. Le contenu du dictionnaire est automatiquement enregistré dans le stockage local d’application avant l’arrêt du programme et rechargé lorsque le programme démarre.
- Le `Application` classe définit trois méthodes virtuelles protégées qui le programme de standard `App` substitue : [ `OnStart` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnStart()/), [ `OnSleep` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnSleep()/), et [ `OnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnResume()/). Ils désignent les *cycle de vie application* événements.
- Le [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) méthode enregistre le contenu du dictionnaire.

Il n’est pas nécessaire d’appeler `SavePropertiesAsync`. Le contenu du dictionnaire est automatiquement enregistré avant l’arrêt du programme et récupéré avant le démarrage du programme. Il est utile lors du test de programme pour enregistrer les données si le programme se bloque.

Également utile est :

- [`Application.Current`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Current/), une propriété statique qui renvoie l’actuel `Application` objet que vous pouvez ensuite utiliser pour obtenir le `Properties` dictionnaire.

La première étape consiste à identifier toutes les variables dans la page que vous souhaitez rendre persistantes lorsque le programme se termine. Si vous connaissez tous les emplacements où ces variables changer, vous pouvez simplement les ajouter à la `Properties` dictionnaire à ce stade. Dans le constructeur de la page, vous pouvez définir les variables à partir de la `Properties` dictionnaire si la clé existe.

Un programme plus vaste aurez probablement besoin traiter les événements de cycle de vie d’application. Le plus important est le `OnSleep` (méthode). Un appel à cette méthode indique que le programme a quitté le premier plan. Peut-être que l’utilisateur a appuyé sur le **accueil** bouton sur l’appareil ou affiche toutes les applications en cours d’arrêt du téléphone. Un appel à `OnSleep` est la seule notification qu’un programme reçoit avant d’être arrêté. Le programme doit prendre cette opportunité pour vous assurer que le `Properties` dictionnaire est à jour.

Un appel à `OnResume` indique que le programme ne s’est pas achevé, suivant le dernier appel à `OnSleep` mais est en cours d’exécution au premier plan à nouveau. Le programme peut utiliser cette opportunité pour actualiser les connexions internet (par exemple).

Un appel à `OnStart` se produit lors du démarrage du programme. Il n’est pas nécessaire d’attendre jusqu'à ce que l’appel de cette méthode pour accéder à la `Properties` dictionnaire, car le contenu a déjà été restauré lorsque la `App` constructeur est appelé.

Le [ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) exemple est très semblable à **SimplestKeypad** , sauf que le programme utilise le `OnSleep` override pour enregistrer l’entrée de clavier en cours, et le constructeur de la page à restaurer ces données.



## <a name="related-links"></a>Liens associés

- [Recherche en texte intégral chapitre 6 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Exemples de chapitre 6](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Exemples de chapitre 6 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
