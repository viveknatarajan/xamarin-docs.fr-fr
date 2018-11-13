---
title: Raccourcis clavier de Xamarin classeurs éditeur
description: Ce document décrit les raccourcis clavier disponibles pour une utilisation dans l’éditeur de Xamarin Workbooks. En particulier, il examine les différentes façons de qu'utiliser de la touche Retour.
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 87af9f824117b20250c02a3e070652607626de44
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526141"
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Raccourcis clavier de Xamarin classeurs éditeur

## <a name="the-return-key-and-its-nuances"></a>La touche Retour et ses nuances

Le tableau suivant décrit les différentes combinaisons de touches pour la création de markdown et de l’exécution de code. Nous avons pris le plus grand soin de choisir des combinaisons de touches réalistes et cohérents qui sont familiers et fluide.

|Combinaison de touches|Cellule de code|Cellule de markdown|
|--- |--- |--- |
|<kbd>Return</kbd>|<p>Si le signe insertion est à la fin de la mémoire tampon de cellule et la cellule peut être analysée avec succès, elle est exécutée et résultats s’affiche au-dessous de la mémoire tampon, et une nouvelle cellule de code est insérée et concentre la cellule après la cellule exécutée.</p><p>Si l’analyse n’est pas réussie, une nouvelle ligne est insérée dans la mémoire tampon. Diagnostics du compilateur ne sont pas émis si l’analyse n’a pas réussie.</p>|<p><kbd>Retourner</kbd> a un comportement différent en fonction du contexte de Markdown au point d’insertion.</p><ul><li>Si le signe insertion est dans un bloc de code Markdown, une nouvelle ligne littéral est insérée.</li><li>Si le signe insertion est dans un bloc de liste de promotions, créer un nouvel élément de liste ou fractionner l’élément de liste actuel.</li><li>Si le signe insertion est dans un autre type de bloc de Markdown, créez un nouveau bloc de paragraphe ou fractionner le bloc actuel.</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑return</kbd></dd><dt>Win</dt><dd><kbd>Control‑return</kbd></dd></dl>|<p>Tente toujours d’analyser et d’exécuter le contenu de la cellule. Si la compilation réussit, les résultats (y compris les exceptions d’exécution) s’afficheront sous la mémoire tampon, et s’il n’y a aucune cellule suivants, un nouveau est créé et concentré.</p><p>S’il existe des erreurs de compilation, diagnostics s’affiche et la mémoire tampon tout en continuant avec la position du signe insertion inchangée.</p>|Insère et se concentre une nouvelle cellule de code après la cellule active de markdown.|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑return</kbd><dd><dt>Win</dt><dd><kbd>Control‑Shift‑return</kbd></dd></dl>|Insère et met l’accent une nouvelle cellule markdown après la cellule active.|Même comportement que <kbd>retourner</kbd>|
|<kbd>Shift‑return</kbd>|Toujours insère une nouvelle ligne, quelle que soit l’emplacement du signe insertion ou de contenu.|Insère un saut de ligne dans le bloc Markdown actuel.|
