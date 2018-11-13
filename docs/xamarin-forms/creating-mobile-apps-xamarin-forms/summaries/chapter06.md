---
title: Résumé du chapitre 6. Clics de bouton
description: 'Création d’applications mobiles avec Xamarin.Forms : résumé du chapitre 6. Clics de bouton'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 0f1da94031e658d42205e6346d41b02c5822d992
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563678"
---
# <a name="summary-of-chapter-6-button-clicks"></a>Résumé du chapitre 6. Clics de bouton

Le [ `Button` ](xref:Xamarin.Forms.Button) est la vue qui permet aux utilisateurs de lancer une commande. Un `Button` est identifié par le texte (et éventuellement une image comme illustré dans [chapitre 13, Bitmaps](chapter13.md)). Par conséquent, `Button` définit un grand nombre des propriétés du mêmes comme `Label`:

- [`Text`](xref:Xamarin.Forms.Button.Text)
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)

`Button` définit également trois propriétés qui régissent l’apparence de sa bordure, mais la prise en charge de ces propriétés et leur indépendance mutuelle est spécifiques à la plateforme :

- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) de type `Color`
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) de type `Double`
- [`BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) de type `Double`

`Button` hérite également toutes les propriétés de `VisualElement` et `View`, y compris `BackgroundColor`, `HorizontalOptions`, et `VerticalOptions`.

## <a name="processing-the-click"></a>Traitement de la, cliquez sur

Le `Button` classe définit un [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) événement est déclenché lorsque l’utilisateur actionne le `Button`. Le `Click` gestionnaire est de type `EventHandler`. Le premier argument est le `Button` génère l’événement de l’objet ; le deuxième argument est un `EventArgs` objet qui fournit des informations supplémentaires.

Le [ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) simple illustre `Clicked` gestion.

## <a name="sharing-button-clicks"></a>Bouton de partage clique sur

Plusieurs `Button` vues peuvent partager la même `Clicked` gestionnaire, mais le gestionnaire doit généralement déterminer quelle `Button` est chargé pour un événement particulier. Une approche consiste à stocker les différentes `Button` objets en tant que champs et celle qui déclenche l’événement dans le Gestionnaire de contrôle.

Le [ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) exemple illustre cette technique. Le programme montre aussi comment définir le [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) propriété d’un `Button` à `false` lorsque vous appuyez sur la `Button` n’est plus valide. Désactivé `Button` ne génère pas un `Clicked` événement.

## <a name="anonymous-event-handlers"></a>Gestionnaires d’événements anonyme

Il est possible de définir `Clicked` gestionnaires en tant que fonctions lambda anonyme, comme le [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) montre des exemples. Toutefois, gestionnaires anonymes ne peut pas être partagés sans un code de réflexion désordonnées.

## <a name="distinguishing-views-with-ids"></a>Distinction des vues avec des ID

Plusieurs `Button` objets peuvent également être distingués en définissant le [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) propriété ou [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId) propriété à un `string`. Cette propriété est définie par `Element` , mais elle n’est pas utilisée dans Xamarin.Forms. Elle est destinée à être utilisée uniquement par les programmes d’application.

Le [ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) exemple utilise le même gestionnaire d’événements pour toutes les touches numériques 10 sur un pavé numérique et fait la distinction entre eux avec le `StyleId` propriété :

[![Capture d’écran triple du pavé la plus simple](images/ch06fg04-small.png "calculatrice")](images/ch06fg04-large.png#lightbox "calculatrice")

## <a name="saving-transient-data"></a>Enregistrer les données temporaires

De nombreuses applications ont besoin pour enregistrer les données lorsqu’un programme est arrêté et recharger ces données lorsque le programme démarre à nouveau. Le [ `Application` ](xref:Xamarin.Forms.Application) classe définit plusieurs membres qui aident à votre programme enregistrer et restaurer les données temporaires :

- Le [ `Properties` ](xref:Xamarin.Forms.Application.Properties) propriété est un dictionnaire avec `string` clés et `object` éléments. Le contenu du dictionnaire est automatiquement enregistré dans le stockage local des applications avant l’arrêt du programme et rechargé lorsque le programme démarre.
- Le `Application` classe définit trois méthodes virtuelles protégées qui le programme de standard `App` substitue : [ `OnStart` ](xref:Xamarin.Forms.Application.OnStart), [ `OnSleep` ](xref:Xamarin.Forms.Application.OnSleep), et [ `OnResume` ](xref:Xamarin.Forms.Application.OnResume). Ils se rapportent aux *cycle de vie des applications* événements.
- Le [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) méthode enregistre le contenu du dictionnaire.

Il n’est pas nécessaire d’appeler `SavePropertiesAsync`. Le contenu du dictionnaire est automatiquement enregistré avant l’arrêt du programme et récupéré avant le démarrage du programme. Il est utile lors du test de programme pour enregistrer les données si le programme se bloque.

Également est utile :

- [`Application.Current`](xref:Xamarin.Forms.Application.Current), une propriété statique qui retourne actuel `Application` objet que vous pouvez ensuite utiliser pour obtenir le `Properties` dictionnaire.

La première étape consiste à identifier toutes les variables dans la page que vous souhaitez rendre persistantes lorsque le programme se termine. Si vous connaissez tous les endroits où ces variables changer, vous pouvez simplement les ajouter à la `Properties` dictionnaire à ce stade. Dans le constructeur de la page, vous pouvez définir les variables à partir de la `Properties` dictionnaire si la clé existe.

Un programme plus vaste devrez probablement à traiter les événements de cycle de vie d’application. La plus importante est la `OnSleep` (méthode). Un appel à cette méthode indique que le programme a quitté le premier plan. Peut-être l’utilisateur a appuyé sur le **accueil** bouton sur l’appareil, ou affiche toutes les applications ou s’arrête le téléphone. Un appel à `OnSleep` est la seule notification qu’un programme reçoit avant d’être arrêté. Le programme doit saisir cette occasion pour vous assurer que le `Properties` dictionnaire est à jour.

Un appel à `OnResume` indique que le programme ne s’est pas achevé après le dernier appel à `OnSleep` mais est maintenant en cours d’exécution au premier plan à nouveau. Le programme peut utiliser cette opportunité pour actualiser les connexions internet (par exemple).

Un appel à `OnStart` se produit lors du démarrage du programme. Il n’est pas nécessaire d’attendre jusqu'à ce que l’appel de cette méthode pour accéder à la `Properties` dictionnaire, car le contenu a déjà été restaurée lorsque le `App` constructeur est appelé.

Le [ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) exemple est très similaire à **SimplestKeypad** , à ceci près que le programme utilise le `OnSleep` remplacement pour enregistrer l’entrée de clavier en cours, et le constructeur de la page pour restaurer ces données.

> [!NOTE]
> Une autre approche pour l’enregistrement des paramètres de programme est fournie par le Xamarin.Essentials [préférences](~/essentials/preferences.md) classe.

## <a name="related-links"></a>Liens connexes

- [Chapitre 6 de texte intégral (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Exemples de chapitre 6](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Chapitre 6 F# exemples](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
- [Bouton de Xamarin.Forms](~/xamarin-forms/user-interface/button.md)
