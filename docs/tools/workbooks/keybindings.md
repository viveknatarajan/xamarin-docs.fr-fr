---
title: "Raccourcis de clavier de l’éditeur de classeurs Xamarin"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: cbbf070a9c94221d98dedcd1df884a897ff7df3e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Raccourcis de clavier de l’éditeur de classeurs Xamarin

## <a name="the-return-key-and-its-nuances"></a>La clé de retour et ses nuances

Le tableau suivant décrit les différentes combinaisons de touches pour la création de promotions et de l’exécution de code. Nous avons choisi pour choisir des combinaisons de touches pratique et cohérentes qui vous sont familières et fluide.

<table>
  <tr>
    <th>Liaison de clé</th>
    <th>Cellule de code</th>
    <th>Cellule de markdown</th>
  </tr>
  <tr>
    <td><kbd>Return</kbd></td>
    <td>
      <p>Si le point d’insertion est à la fin de la mémoire tampon de cellule et la cellule peut être analysée avec succès, il sera exécuté résultats s’affichent sous la mémoire tampon et une nouvelle cellule de code sera insérée et Active la cellule après la cellule exécutée.</p>
      
      <p>Si l’analyse n’est pas réussie, une nouvelle ligne est insérée dans la mémoire tampon. Les diagnostics du compilateur ne sera pas générés si l’analyse n’a pas réussie.</p>
    </td>
    <td>
      <p>
        <kbd>Retourner</kbd> a un comportement différent selon le contexte de promotions au point d’insertion.
      </p>
      <ul>
        <li>
Si le point d’insertion se trouve dans un bloc de code Markdown, une nouvelle ligne littéral est insérée.
        </li>
        <li>
Si le point d’insertion est dans un bloc de liste de promotions, créer un nouvel élément de liste ou fractionner l’élément de liste actuel.
        </li>
        <li>
Si le point d’insertion se trouve dans un autre type de bloc de promotions, créer un nouveau bloc de paragraphe ou fractionner le bloc actuel.
        </li>
    </td>
  </tr>
  <tr>
    <td>
      <dl>
        <dt>Mac</dt>
        <dd><kbd>Command‑return</kbd></dd>
        <dt>Win</dt>
        <dd><kbd>Control‑return</kbd></dd>
      </dl>
    </td>
    <td>
      <p>Tente toujours d’analyser et d’exécuter le contenu des cellules. Si la compilation réussit, les résultats (y compris les exceptions d’exécution) s’affichent sous la mémoire tampon, et s’il n’y a aucune cellule suivants, un nouveau sera créé et le focus.</p>
      
      <p>S’il existe des erreurs de compilation, diagnostics s’affiche et la mémoire tampon reste ayant le focus avec le signe inchangé.</p>
    </td>
    <td>
Insère et les vues d’une nouvelle cellule de code après la cellule active de promotions.
    </td>
  </tr>
  <tr>
    <td>
      <dl>
        <dt>Mac</dt>
        <dd><kbd>Command‑Shift‑return</kbd></dd>
        <dt>Win</dt>
        <dd><kbd>Control‑Shift‑return</kbd></dd>
      </dl>
    </td>
    <td>
Insère et les vues d’une nouvelle cellule markdown après la cellule active.
    </td>
    <td>
Même comportement que <kbd>de retour</kbd>
    </td>
  </tr>
  <tr>
    <td><kbd>Shift‑return</kbd></td>
    <td>
Toujours insère une nouvelle ligne, quelle que soit l’emplacement du signe insertion ou de contenu.
    </td>
    <td>
Insère un saut de ligne dans le bloc Markdown actuel.
    </td>
  </tr>
</table>
