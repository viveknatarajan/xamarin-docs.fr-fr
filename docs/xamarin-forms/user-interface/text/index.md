---
title: Texte
description: "À l’aide de Xamarin.Forms pour entrer ou afficher du texte."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 55d13589e4241e9f4e29aea9a55346a8f514f208
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="text"></a>Texte

_À l’aide de Xamarin.Forms pour entrer ou afficher du texte._

Xamarin.Forms propose trois vues principales pour travailler avec du texte :

- **[Étiquette](#Label)**  &mdash; pour la présentation de texte unique ou plusieurs ligne. Peut afficher plusieurs options de mise en forme dans la même ligne.
- **[Entrée](#Entry)**  &mdash; de saisie de texte n'est qu’une seule ligne. Entrée comporte un mode de mot de passe.
- **[Éditeur de](#Editor)**  &mdash; de saisie de texte qui peut prendre plus d’une ligne.

Apparence de texte peut être modifié à l’aide intégrée ou personnalisée [styles](#Styles) et certains contrôles prennent en charge personnalisée [polices](#Fonts).

<a name="Label" />

## <a name="labellabelmd"></a>[Label](label.md)

Le `Label` vue est utilisée pour afficher le texte. Il peut afficher plusieurs lignes de texte ou d’une seule ligne de texte. `Label` plusieurs options de mise en forme utilisées dans inline peut présenter texte. L’affichage de l’étiquette peut encapsuler ou texte tronqué lorsqu’il ne peut pas tenir sur une seule ligne.

![](images/label.png "Exemple d’étiquette")

Consultez le [étiquette](label.md) article pour plus d’informations.

Pour plus d’informations sur la personnalisation de la police utilisée dans une étiquette, consultez [polices](fonts.md).

<a name="Entry" />

## <a name="entryentrymd"></a>[Entrée](entry.md)

`Entry` est utilisé pour accepter l’entrée de ligne de texte unique. `Entry` offres de contrôlent les couleurs, mais ne peut pas avoir personnalisé de polices. `Entry` contient un mode de mot de passe et peuvent afficher le texte d’espace réservé jusqu'à ce que l’entrée de texte.

![](images/entry.png "Exemple d’entrée")

Consultez le [entrée](entry.md) article pour plus d’informations.

Notez que, contrairement à `Label`, `Entry` ne peut pas avoir de paramètres de police personnalisée.

<a name="Editor" />

## <a name="editoreditormd"></a>[Éditeur](editor.md)

`Editor` est utilisé pour accepter l’entrée de texte multiligne. `Editor` peut avoir une couleur d’arrière-plan personnalisée, mais la couleur du texte et de police ne peut pas être modifiée.

![](images/editor.png "Exemple d’éditeur")

Consultez le [éditeur](editor.md) article pour plus d’informations.

<a name="Fonts" />

## <a name="fontsfontsmd"></a>[Polices](fonts.md)

Le `Label` contrôle prend en charge les paramètres de police différente à l’aide de polices intégrés sur chaque plateforme ou des polices personnalisées inclus avec votre application. Consultez le [polices](fonts.md) article pour plus d’informations.

<a name="Styles" />

## <a name="stylesstylesmd"></a>[Styles](styles.md)

Reportez-vous à [travailler avec des styles](~/xamarin-forms/user-interface/styles/index.md) pour savoir comment configurer la police, [couleur](~/xamarin-forms/user-interface/colors.md)et d’autres propriétés d’affichage qui s’appliquent à plusieurs contrôles.



## <a name="related-links"></a>Liens associés

- [Texte (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
