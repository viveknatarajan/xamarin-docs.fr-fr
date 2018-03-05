---
title: Complications
description: "watchOS permet aux développeurs d’écrire des complications personnalisées pour les faces espion"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/03/2017
ms.openlocfilehash: a13de7fbb4b6e1f9fa2853ce599f3a038a5e4040
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="complications"></a>Complications

_watchOS permet aux développeurs d’écrire des complications personnalisées pour les faces espion_

Cette page explique les différents types de complications disponibles et l’ajout de le des complications à votre application watchOS 3.

Notez que chaque application watchOS peut avoir uniquement une complication.

Commencez par lire [les documents d’Apple](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html) pour déterminer si votre application est adaptée à le des complications. Il existe 5 `CLKComplicationFamily` types d’affichage de choisir à partir de :

[ ![](complications-images/all-complications-sml.png "Les types de CLKComplicationFamily 5 disponibles : circulaire petite, petite modulaire, modulaire volumineux, des petites, stylistiquement volumineux")](complications-images/all-complications.png)

Les applications peuvent implémenter qu’un seul style, ou toutes les cinq, en fonction des données affichées.
Vous pouvez également en charge voyage, fournissant des valeurs pour les heures passées et/ou l’utilisateur se trouve la couronne numérique.

<a name="adding" />

## <a name="adding-a-complication"></a>Ajout d’une Complication

### <a name="configuration"></a>Configuration

Complications peuvent être ajoutées à une application espion lors de la création ou ajoutées manuellement à une solution existante.

### <a name="add-new-project"></a>Ajouter un nouveau projet...

Le **ajouter un nouveau projet...**  Assistant inclut une case à cocher Créer une classe de contrôleur complication et configurer automatiquement le **Info.plist** fichier :

![](complications-images/file-new-project-sml.png "La case à cocher Inclure la Complication")

### <a name="existing-projects"></a>Projets existants

Pour ajouter le des complications à un projet existant :

1. Créer un nouveau **ComplicationController.cs** fichier de classe et implémenter `CLKComplicationDataSource`.
2. Configurer l’application **Info.plist** pour exposer la complication et l’identité les familles de complications sont pris en charge.

Ces étapes sont décrites plus en détail ci-dessous.

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>Classe de CLKComplicationDataSource

Le modèle c# suivant comprend les méthodes minimales requises pour implémenter un `CLKComplicationDataSource`.

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

Suivez les [l’écriture de le des complications](#writing) obtenir des instructions pour ajouter du code à cette classe.

### <a name="infoplist"></a>Info.plist

L’extension de surveillance **Info.plist** fichier doit spécifier le nom de la `CLKComplicationDataSource` et les familles de complication que vous souhaitez prendre en charge :

[ ![](complications-images/complications-config-sml.png "Les types de famille complication")](complications-images/complications-config.png)

Le **classe Source de données** entrée liste affiche les noms de classe qui sous-classe `CLKComplicationDataSource` sous-classe qui inclut votre logique de la complication.

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

Toutes les fonctionnalités de la complication sont implémentée dans une classe unique, la substitution de méthodes à partir de la `CLKComplicationDataSource` classe abstraite (qui implémente le `ICLKComplicationDataSource` interface).

### <a name="required-methods"></a>Méthodes requises

Vous devez implémenter les méthodes suivantes pour la complication exécuter :

- `GetPlaceholderTemplate` -L’affichage statique utilisé lors de la configuration ou lors de l’application ne peut pas fournir une valeur de retour.
- `GetCurrentTimelineEntry` -Permet de calculer l’affichage correct lorsque la complication est en cours d’exécution.
- `GetSupportedTimeTravelDirections` -Retourne les options de `CLKComplicationTimeTravelDirections` comme `None`, `Forward`, `Backward`, ou `Forward | Backward`.

### <a name="privacy"></a>Confidentialité

Complications qui affichent des données personnelles

* `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` Ou `HideOnLockScreen`

Si cette méthode retourne `HideOnLockScreen` la complication affiche soit une icône ou le nom de l’application (et non les données) lorsque la surveillance est verrouillée.

### <a name="updates"></a>Mises à jour

- `GetNextRequestedUpdateDate` : Retourne une heure lorsque le système d’exploitation doit interroger ensuite l’application pour afficher les données des complications mis à jour.

Vous pouvez également forcer une mise à jour à partir de votre application iOS.

### <a name="supporting-time-travel"></a>Prise en charge de voyage

Prise en charge des temps de déplacement est facultative et contrôlée par le `GetSupportedTimeTravelDirections` (méthode). Si elle retourne `Forward`, `Backward`, ou `Forward | Backward` vous devez implémenter les méthodes suivantes

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>L’écriture d’une Complication

Plage de complications à partir de données simples afficher image complexe et de rendu de données avec prise en charge de voyage. Le code ci-dessous montre comment générer une complication simple, modèle unique.

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>Exemple de code

Cet exemple prend uniquement en charge la `UtilitarianLarge` modèle, donc peut être sélectionné uniquement sur les faces espion spécifiques qui prennent en charge ce type de difficulté. Lorsque *en sélectionnant* complications sur un espion, il affiche **COMPLICATION Mes** et à quel moment *en cours d’exécution* il affiche le texte **MINUTE _heure_**   (avec une partie du temps).

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

## <a name="complication-templates"></a>Modèles de la complication

Il existe plusieurs modèles différents pour chaque style complication.
Le **anneau** modèles vous permettent d’afficher un anneau de style de progression autour de la complication, ce qui peut être utilisé pour afficher sous forme graphique de progression ou une autre valeur.

[Documents de CLKComplicationTemplate d’Apple](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>Petit cercle

Ces noms de classe de modèle sont toutes le préfixe `CLKComplicationTemplateCircularSmall`:

- **RingImage** -afficher une seule image, avec une boucle de progression autour de lui.
- **RingText** -afficher une seule ligne de texte, avec une boucle de progression autour de lui.
- **SimpleImage** -affichent simplement une petite image unique.
- **SimpleText** -affichent simplement un petit extrait de texte.
- **StackImage** -afficher une image et une ligne de texte, un au-dessus de l’autre
- **StackText** -afficher deux lignes de texte.

### <a name="modular-small"></a>Petite modulaire

Ces noms de classe de modèle sont toutes le préfixe `CLKComplicationTemplateModularSmall`:

- **ColumnsText** -afficher une petite grille de valeurs de texte (2 lignes et 2 colonnes).
- **RingImage** -afficher une seule image, avec une boucle de progression autour de lui.
- **RingText** -afficher une seule ligne de texte, avec une boucle de progression autour de lui.
- **SimpleImage** -affichent simplement une petite image unique.
- **SimpleText** -affichent simplement un petit extrait de texte.
- **StackImage** -afficher une image et une ligne de texte, un au-dessus de l’autre
- **StackText** -afficher deux lignes de texte.

### <a name="modular-large"></a>Modulaire volumineux

Ces noms de classe de modèle sont toutes le préfixe `CLKComplicationTemplateModularLarge`:

- **Colonnes** -afficher une grille de 3 lignes avec 2 colonnes, y compris éventuellement l’image à gauche de chaque ligne.
- **StandardBody** -affiche une chaîne d’en-tête en gras, avec deux lignes de texte brut. L’en-tête peut aussi afficher une image sur la gauche.
- **Table** -affiche une chaîne d’en-tête en gras, avec une grille de 2 x 2 du texte situé sous celui-ci. L’en-tête peut aussi afficher une image sur la gauche.
- **TallBody** -affiche une chaîne d’en-tête en gras, avec une plus grande police seule ligne de texte en dessous.

### <a name="utilitarian-small"></a>Petite stylistiquement

Ces noms de classe de modèle sont toutes le préfixe `CLKComplicationTemplateUtilitarianSmall`:

- **Plat** -affiche une image et du texte sur une seule ligne (le texte doit être court).
- **RingImage** -afficher une seule image, avec une boucle de progression autour de lui.
- **RingText** -afficher une seule ligne de texte, avec une boucle de progression autour de lui.
- **Carré** -afficher une image carrée (40px ou 44px carré pour la 38 mm ou 42 mm Apple Watch respectivement).

### <a name="utilitarian-large"></a>Stylistiquement volumineux

Il existe un seul modèle pour ce style complication : `CLKComplicationTemplateUtilitarianLargeFlat`.
Il affiche une seule image et du texte, sur une seule ligne.



## <a name="related-links"></a>Liens associés

- [Documents d’Apple](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [Vidéo WWDC](https://developer.apple.com/videos/play/wwdc2015-209/)
