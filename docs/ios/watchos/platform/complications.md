---
title: watchOS Complications dans Xamarin
description: Ce document décrit comment utiliser des complications watchOS dans Xamarin. Il explique comment ajouter une complication, écriture d’une complication, modèles et fournit des exemples de code.
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/03/2017
ms.openlocfilehash: 85b0c9b0688e9fb310a8f427018a02fe629404bb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61225517"
---
# <a name="watchos-complications-in-xamarin"></a>watchOS Complications dans Xamarin

_watchOS permet aux développeurs d’écrire des complications personnalisées pour les visages d’espion_

Cette page explique les différents types de complications disponibles et comment ajouter une complication à votre application watchOS 3.

Notez que chaque application watchOS peut avoir uniquement une complication.

Commencez par lire [documentation d’Apple](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html) pour déterminer si votre application est adaptée à une complication. Il existe 5 `CLKComplicationFamily` types d’affichage au choix :

[![](complications-images/all-complications-sml.png "Les types CLKComplicationFamily 5 disponibles : Petite taille circulaire, modulaire de petite taille, modulaire de petite taille importante, Normal, normal volumineux")](complications-images/all-complications.png#lightbox)

Applications peuvent implémenter qu’un seul style, ou toutes les cinq, selon les données affichées.
Vous pouvez également en charge voyage, fournissant des valeurs pour les heures passées ou futures l’utilisateur se trouve la couronne numérique.

<a name="adding" />

## <a name="adding-a-complication"></a>Ajout d’une Complication

### <a name="configuration"></a>Configuration

Complications peuvent être ajoutées à une application watch lors de la création ou ajoutées manuellement à une solution existante.

### <a name="add-new-project"></a>Ajouter un nouveau projet...

Le **ajouter un nouveau projet...**  Assistant inclut une case à cocher Créer une classe de contrôleur de complication automatiquement et configurer le **Info.plist** fichier :

![](complications-images/file-new-project-sml.png "La case à cocher Inclure une Complication")

### <a name="existing-projects"></a>Projets existants

Pour ajouter une complication à un projet existant :

1. Créer un nouveau **ComplicationController.cs** fichier de classe et de mettre en œuvre `CLKComplicationDataSource`.
2. Configurer l’application **Info.plist** pour exposer la complication et l’identité les familles de complication sont pris en charge.

Ces étapes sont décrites plus en détail ci-dessous.

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>Classe de CLKComplicationDataSource

Ce qui suit C# modèle inclut le nombre minimal de méthodes requis pour implémenter un `CLKComplicationDataSource`.

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
  public ComplicationController ()
  {
    }
  public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
    }
  public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
    }
  public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
    }
}
```

Suivez le [écriture d’une complication](#writing) obtenir des instructions pour ajouter du code à cette classe.

### <a name="infoplist"></a>Info.plist

L’extension watch **Info.plist** fichier doit spécifier le nom de la `CLKComplicationDataSource` et les familles de complication que vous souhaitez prendre en charge :

[![](complications-images/complications-config-sml.png "Les types de famille de complication")](complications-images/complications-config.png#lightbox)

Le **classe Source de données** entrée liste affiche les noms de classe qui sous-classe `CLKComplicationDataSource` sous-classe qui inclut votre logique de complication.

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

Toutes les fonctionnalités de complication sont implémentée dans une classe unique, en remplaçant les méthodes à partir de la `CLKComplicationDataSource` classe abstraite (qui implémente le `ICLKComplicationDataSource` interface).

### <a name="required-methods"></a>Méthodes requises

Vous devez implémenter les méthodes suivantes pour la complication exécuter :

- `GetPlaceholderTemplate` -Retourne l’affichage statique utilisé lors de la configuration ou lors de l’application ne peut pas fournir une valeur.
- `GetCurrentTimelineEntry` -Permet de calculer l’affichage correct lors de l’exécution de la complication.
- `GetSupportedTimeTravelDirections` -Retourne les options de `CLKComplicationTimeTravelDirections` comme `None`, `Forward`, `Backward`, ou `Forward | Backward`.

### <a name="privacy"></a>Confidentialité

Complications qui affichent des données personnelles

* `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` ou `HideOnLockScreen`

Si cette méthode retourne `HideOnLockScreen` la complication indique soit une icône ou le nom de l’application (et non les données) quand l’observation est verrouillée.

### <a name="updates"></a>Mises à jour

- `GetNextRequestedUpdateDate` -Retournent une heure lorsque le système d’exploitation doit interroger ensuite l’application pour afficher les données mises à jour complication.

Vous pouvez également forcer une mise à jour à partir de votre application iOS.

### <a name="supporting-time-travel"></a>Prise en charge de voyage

Prise en charge de temps voyage est facultatif et contrôlée par le `GetSupportedTimeTravelDirections` (méthode). Si elle retourne `Forward`, `Backward`, ou `Forward | Backward` puis vous devez implémenter les méthodes suivantes

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>Écriture d’une Complication

Plage de complications liées à partir de données simples afficher image complexe et de rendu de données avec prise en charge de voyage. Le code ci-dessous montre comment créer une complication simple et unique modèle.

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>Exemple de code

Cet exemple prend uniquement en charge la `UtilitarianLarge` modèle, donc ne peut être sélectionné sur les visages espion spécifiques qui prennent en charge ce type de complication. Lorsque *en sélectionnant* complications sur un espion, il affiche **COMPLICATION Mes** et à quel moment *en cours d’exécution* il affiche le texte **MINUTE _heure_**   (avec une partie du temps).

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
    public ComplicationController ()
    {
    }
    public ComplicationController (IntPtr p) : base (p)
    {
    }
    public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
        CLKComplicationTimelineEntry entry = null;
    var complicationDisplay = "MINUTE " + DateTime.Now.Minute.ToString(); // text to display on watch face
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge)
        {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText(complicationDisplay); // dynamic display
            entry = CLKComplicationTimelineEntry.Create(NSDate.Now, textTemplate);
        } else {
            Console.WriteLine("Complication family timeline not supported (" + complication.Family + ")");
        }
        handler (entry);
    }
    public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
        CLKComplicationTemplate template = null;
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge) {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat ();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText ("MY COMPLICATION"); // static display
            template = textTemplate;
        } else {
            Console.WriteLine ("Complication family placeholder not not supported (" + complication.Family + ")");
        }
        handler (template);
    }
    public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
        handler (CLKComplicationTimeTravelDirections.None);
    }
}
```


<a name="templates" />

## <a name="complication-templates"></a>Modèles de complication

Il existe plusieurs modèles différents pour chaque style complication.
Le **anneau** les modèles vous permettent d’afficher un anneau de progression du style autour de la complication, ce qui peut servir à afficher sous forme graphique de progression ou une autre valeur.

[Documentation de CLKComplicationTemplate d’Apple](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>Petite taille circulaire

Ces noms de classe de modèle sont toutes le préfixe `CLKComplicationTemplateCircularSmall`:

- **RingImage** -afficher une image unique, avec une boucle de progression autour d’elle.
- **RingText** -afficher une seule ligne de texte, avec une boucle de progression autour d’elle.
- **SimpleImage** -affichent simplement une petite image unique.
- **SimpleText** -simplement afficher un petit extrait de texte.
- **StackImage** -afficher une image et une ligne de texte, un au-dessus de l’autre
- **StackText** -afficher deux lignes de texte.

### <a name="modular-small"></a>Modulaire de petite taille

Ces noms de classe de modèle sont toutes le préfixe `CLKComplicationTemplateModularSmall`:

- **ColumnsText** -afficher une petite grille de valeurs de texte (2 lignes et 2 colonnes).
- **RingImage** -afficher une image unique, avec une boucle de progression autour d’elle.
- **RingText** -afficher une seule ligne de texte, avec une boucle de progression autour d’elle.
- **SimpleImage** -affichent simplement une petite image unique.
- **SimpleText** -simplement afficher un petit extrait de texte.
- **StackImage** -afficher une image et une ligne de texte, un au-dessus de l’autre
- **StackText** -afficher deux lignes de texte.

### <a name="modular-large"></a>Modulaire de grande taille

Ces noms de classe de modèle sont toutes le préfixe `CLKComplicationTemplateModularLarge`:

- **Colonnes** -afficher une grille de 3 lignes avec 2 colonnes, en incluant éventuellement une image à gauche de chaque ligne.
- **StandardBody** -affiche une chaîne d’en-tête en gras, avec deux lignes de texte brut. L’en-tête peut éventuellement afficher une image sur la gauche.
- **Table** -affiche une chaîne d’en-tête en gras, avec une grille de 2 x 2 de texte en dessous. L’en-tête peut éventuellement afficher une image sur la gauche.
- **TallBody** -affiche une chaîne d’en-tête en gras, avec une plus grande police seule ligne de texte en dessous.

### <a name="utilitarian-small"></a>Petit utilitaire

Ces noms de classe de modèle sont toutes le préfixe `CLKComplicationTemplateUtilitarianSmall`:

- **Plat** -affiche une image et du texte sur une seule ligne (le texte doit être court).
- **RingImage** -afficher une image unique, avec une boucle de progression autour d’elle.
- **RingText** -afficher une seule ligne de texte, avec une boucle de progression autour d’elle.
- **Carré** -afficher une image carrée (40px ou 44px carré pour la 38 mm ou 42 mm Apple Watch respectivement).

### <a name="utilitarian-large"></a>Utilitaire volumineux

Il existe un seul modèle pour ce style de complication : `CLKComplicationTemplateUtilitarianLargeFlat`.
Il affiche une seule image et du texte, sur une ligne unique.



## <a name="related-links"></a>Liens associés

- [Documentation d’Apple](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [Vidéo WWDC](https://developer.apple.com/videos/play/wwdc2015-209/)
