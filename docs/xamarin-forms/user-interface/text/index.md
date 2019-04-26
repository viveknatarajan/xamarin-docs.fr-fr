---
title: Texte dans Xamarin.Forms
description: Xamarin.Forms a trois vues principales pour travailler avec du texte, et cet article explique comment les utiliser pour entrer et afficher du texte dans les applications Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2018
ms.openlocfilehash: 8754f6573b00334fd228b1c9bd094cd91565976c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61363263"
---
# <a name="text-in-xamarinforms"></a>Texte dans Xamarin.Forms

[![Télécharger l’exemple](~/media/shared/download.png) télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)

_À l’aide de Xamarin.Forms pour entrer ou afficher le texte._

Xamarin.Forms a trois vues principales pour travailler avec du texte :

- **[Étiquette](#Label)**  &mdash; de présentation de texte unique ou multiligne. Peut afficher du texte avec plusieurs options de mise en forme dans la même ligne.
- **[Entrée](#Entry)**  &mdash; de saisie de texte qui n'est qu’une seule ligne. Entrée a un mode de mot de passe.
- **[Éditeur](#Editor)**  &mdash; de saisie de texte qui peut prendre plus d’une ligne.

Apparence du texte peut être modifié à l’aide d’intégrés ou personnalisés [styles](#Styles) et certains contrôles prennent en charge personnalisée [polices](#Fonts).

<a name="Label" />

## <a name="labellabelmd"></a>[Label](label.md)

Le `Label` vue est utilisée pour afficher le texte. Il peut afficher plusieurs lignes de texte ou une seule ligne de texte. `Label` peut présenter texte avec plusieurs options de mise en forme utilisées dans inline. La vue de l’étiquette peut inclure dans un wrapper ou texte tronqué lorsqu’il ne peut pas tenir sur une seule ligne.

![](images/label.png "Exemple d’étiquette")

Consultez le [étiquette](label.md) article pour plus d’informations.

Pour plus d’informations sur la personnalisation de la police utilisée dans une étiquette, consultez [polices](fonts.md).

<a name="Entry" />

## <a name="entryentrymd"></a>[Entrée](entry.md)

`Entry` est utilisé pour accepter l’entrée de texte à ligne unique. `Entry` offre un contrôle sur les polices et couleurs. `Entry` dispose d’un mode de mot de passe et peut afficher du texte de l’espace réservé jusqu'à ce que l’entrée de texte.

![](images/entry.png "Exemple d’entrée")

Consultez le [entrée](entry.md) article pour plus d’informations.

Notez que, contrairement à `Label`, `Entry` ne peut pas avoir de paramètres de police personnalisée.

<a name="Editor" />

## <a name="editoreditormd"></a>[Éditeur](editor.md)

`Editor` est utilisé pour accepter l’entrée de texte multiligne. `Editor` offre un contrôle sur les polices et couleurs.

![](images/editor.png "Exemple d’éditeur")

Consultez le [éditeur](editor.md) article pour plus d’informations.

<a name="Fonts" />

## <a name="fontsfontsmd"></a>[Polices](fonts.md)

De nombreux contrôles prennent en charge les paramètres de police différente à l’aide de polices intégrés sur chaque plateforme, ou des polices personnalisées inclus avec votre application. Consultez le [polices](fonts.md) article pour plus d’informations.

<a name="Styles" />

## <a name="stylesstylesmd"></a>[Styles](styles.md)

Reportez-vous à [travailler avec des styles](~/xamarin-forms/user-interface/styles/index.md) pour savoir comment configurer la police, [couleur](~/xamarin-forms/user-interface/colors.md)et d’autres propriétés d’affichage qui s’appliquent à plusieurs contrôles.

## <a name="related-links"></a>Liens associés

- [Texte (exemple)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
