---
title: Notions de base Xamarin.Forms XAML
description: Prise en main de balisage multiplateforme pour appareils mobiles
ms.topic: article
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: a3f3dbbe0f12cfa7cc1fc6606ec8bd48a96e407c
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-xaml-basics"></a>Notions de base Xamarin.Forms XAML

XAML : l’eXtensible Application Markup Language : permet aux développeurs de définir les interfaces utilisateur dans les applications de Xamarin.Forms à l’aide de balisage plutôt que du code. XAML n’est jamais nécessaire dans un programme de Xamarin.Forms, mais il est souvent plus concis et plus visuelle cohérente que le code équivalent et potentiellement compatible avec les outils. XAML est particulièrement bien adapté pour une utilisation avec l’architecture d’application MVVM (Model-View-ViewModel) populaires : code XAML qui définit la vue qui est liée au code de ViewModel par le biais des liaisons de données basée sur XAML.

## <a name="xaml-basics-contents"></a>Contenu des principes de base XAML

* [Vue d’ensemble](#Overview)
* [Partie 1. Prise en main de XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [Partie 2. Syntaxe XAML essentiels](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [Partie 3. Extensions de balisage XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [Partie 4. Principes fondamentaux de liaison de données](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [Partie 5. À partir de la liaison de données à MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

En plus de ces articles principes fondamentaux de XAML, vous pouvez télécharger les chapitres du livre [créer des applications mobiles avec Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "Couverture de livre")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

Rubriques XAML sont traités de manière plus approfondie dans de nombreux chapitres du livre, y compris :

<table style="border:0px; box-shadow:0 0px 0px" cellpadding="0" cellspacing="2" border="0" width="85%">
<tr style="background:#ecf0f1">
  <td style="border:0px;">
    <h4>Chapitre 7. Vs XAML. Code</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf">Télécharger le PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md">Résumé</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Chapitre 8. XAML en harmonie et code</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">Télécharger le PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">Résumé</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Chapitre 10. Extensions de balisage XAML</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf">Télécharger le PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md">Résumé</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Chapitre 18. MVVM</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf">Télécharger le PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md">Résumé</a></td></tr>
</table>

Ces chapitres peuvent être [téléchargé gratuitement](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

<a name="Overview" />

## <a name="overview"></a>Vue d'ensemble

XAML est un langage basé sur XML créé par Microsoft comme alternative au code de programmation pour l’instanciation et initialiser des objets et organiser les objets des hiérarchies parent-enfant. XAML a été adaptée à différentes technologies dans le .NET framework, mais elle a trouvé son utilité plus grande dans la définition de la disposition des interfaces utilisateur dans Windows Presentation Foundation (WPF), Silverlight, Windows Runtime et Universal Windows Platform (UWP).

XAML fait également partie de Xamarin.Forms, l’interface de programmation basée sur en mode natif de multiplateforme pour iOS, Android et UWP des appareils mobiles. Dans le fichier XAML, le développeur de Xamarin.Forms peut définir des interfaces utilisateur à l’aide de toutes les les vues Xamarin.Forms, les dispositions et les pages, en tant que classes bien que personnalisés. Le fichier XAML peut être soit compilé ou incorporé dans le fichier exécutable. Dans les deux cas, les informations XAML sont analysées au moment de la build pour localiser des objets nommés et à nouveau lors de l’exécution pour instancier et initialiser des objets et établir des liens entre ces objets et le code de programmation.

XAML présente plusieurs avantages sur code équivalent :

-  XAML est souvent plus concis et plus lisibles que le code équivalent.
-  La hiérarchie parent-enfant inhérente à XML permet à XAML afin de reproduire avec une plus grande clarté visuelle de la hiérarchie parent-enfant des objets d’interface utilisateur.
-  XAML permettre être facilement écrit par les programmeurs, mais se prête également pour être compatible avec les outils et générées par les outils de conception visuelle.

Bien entendu, il existe également des inconvénients, principalement liés aux limitations qui font partie intégrante des langages de balisage :

-  XAML ne peut pas contenir de code. Tous les gestionnaires d’événements doivent être définis dans un fichier de code.
-  XAML ne peut pas contenir de boucles pour le traitement répétitif. (Toutefois, plusieurs objets visual Xamarin.Forms, plus particulièrement [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) — peut générer plusieurs enfants basées sur les objets dans son `ItemsSource` collection.)
-  XAML ne peut pas contenir de traitement conditionnel (Toutefois, une liaison de données permettre faire référence à un convertisseur de liaison basée sur un code efficace permet un traitement conditionnel.)
-  En général XAML ne peut pas instancier des classes qui ne définissent pas un constructeur sans paramètre. (Toutefois, il est parfois un moyen de contourner cette restriction.)
-  En général XAML ne peut pas appeler des méthodes. (Là encore, cette restriction peut parfois être évitée.)

Il n’est pas encore un concepteur visuel pour la génération de code XAML dans les applications de Xamarin.Forms. Tout code XAML doit être écrit manuellement, mais il existe un [XAML aperçu](~/xamarin-forms/xaml/xaml-previewer.md). Aux programmeurs de nouveau XAML souhaiterez souvent générer et exécuter leurs applications, en particulier après tout ce qui peut ne pas être évidemment correct. Les développeurs de paires avec beaucoup d’expérience en XAML savent qu’expérimentation récompense.

XAML est fondamentalement XML, mais certaines fonctionnalités de syntaxe unique XAML. Les plus importants sont :

- Éléments de propriété
- Propriétés jointes
- Extensions de balisage

Ces fonctionnalités sont *pas* extensions XML. XAML est entièrement conforme XML. Mais ces fonctionnalités de syntaxe XAML utilisent XML de façon unique. Elles sont présentées en détail dans les articles ci-dessous, qui se terminer par une introduction à l’utilisation de XAML pour l’implémentation de MVVM.

## <a name="requirements"></a>Configuration requise

Cet article suppose une connaissance pratique de Xamarin.Forms. Lors de la lecture [présentation de Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) est fortement recommandé.

Cet article suppose également familiarisé avec XML, y compris la compréhension de l’utilisation des déclarations d’espace de noms XML et les termes du contrat *élément*, *balise*, et *attribut*.

Lorsque vous êtes familiarisé avec Xamarin.Forms et XML, commence à lire [partie 1. Prise en main de XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md).



## <a name="related-links"></a>Liens associés

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Une présentation de Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Création d’annuaire d’applications mobiles](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Exemples de Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
