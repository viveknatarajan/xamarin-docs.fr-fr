---
title: Quand et comment dois-je classer un rapport de bogues ?
description: Ce document décrit quand, où et comment pour envoyer un rapport de bogue. Il fournit également les meilleures pratiques qui permettent aux ingénieurs de mieux diagnostiquer le problème de rapport de bogue.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
ms.openlocfilehash: b70fe29a79e099f1141c1295d907b48afaa2c3c7
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351604"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Quand et comment dois-je classer un rapport de bogues ?

Signaler des bogues dans le traqueur de bogues Bugzilla de Xamarin ici : [ https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all ](https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all).

## <a name="file-a-bug-if"></a>Signaler un bogue si...

Vous avez un ensemble d’étapes que vous pensez que les ingénieurs de Xamarin seront en mesure d’utiliser pour reproduire un problème est provoqué par Xamarin.

OU

Vous pouvez soigneusement décrire les symptômes visibles du problème, surtout si vous pouvez également décrire certaines circonstances précises la cause du problème. <sup> [[1]](#note-1)</sup>


## <a name="best-practices-to-help-xamarin-address-bugs-quickly-and-efficiently"></a>Meilleures pratiques pour aider à résoudre des bogues Xamarin rapidement et efficacement


1. <a name="ref-1" />Recherche [Bugzilla](https://bugzilla.xamarin.com/query.cgi?format=specific&amp;bug_status=__all__) et sur le web existant bogues des rapports ou des suggestions d’utilisation qui peuvent résoudre le problème directement.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />Suivez le [bogue directives d’écriture](https://bugzilla.xamarin.com/page.cgi?id=bug-writing.html) pour décrire le problème comme claire et concise que possible, y compris une description de ce qui s’est produite et a été prévu pour se produire.

1. <a name="ref-3" />Inclure les traces de pile pertinentes, le texte du message d’erreur, ou les journaux d’incident. <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />Notez les messages d’erreur important qui s’affichent dans les pièces jointes de la capture d’écran trop en tant que texte brut.

1. <a name="ref-5" />Inclure un cas de test small et autonome qui reproduit le bogue avec peu de code que possible.  Si vous ne pouvez pas reproduire le problème avec un tout nouveau projet (créé à l’aide d’un des modèles intégrés), puis zippez un projet qui illustre le problème et l’attacher au rapport de bogues.  Vérifiez l’exemple de projet aussi simples que possible avant de l’attacher. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />Décrivez l’environnement où le bogue a été trouvé, y compris le système d’exploitation et [versions de Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) et toutes les dépendances.

---

## <a name="additional-details"></a>Détails supplémentaires

1. <a name="note-1" />[*^*](#ref-1) Dans l’idéal, la description des symptômes « visibles » doit inclure suffisamment de détails afin que les autres clients peuvent vérifier si elles rencontrent le même problème (messages d’erreur même, une dégradation des performances même, même trace de la pile d’un incident, _etc.._ ). Pour « circonstances précis », un bon exemple serait si vous pouvez dire quelque chose comme : « j’atteins normalement le problème 75 % du temps, mais si je modifie cette une chose puis me permet d’éviter le problème complètement. » Un autre exemple similaire d’un « circonstance précis » est si la mise à niveau vers une version antérieure de Xamarin s’arrête le problème.

1. <a name="note-2" />[*^*](#ref-2) Comme vous pouvez l’imaginer, des extraits de texte de l’erreur (ou tout autre texte descriptif unique) sont généralement les meilleurs termes de recherche. Si le rapport de bogue existant est incomplète, puis vous avez la possibilité ajouter des détails ou des fichiers à un nouveau, mieux rapport de bogue.

1. <a name="note-3" />[*^*](#ref-3) Une autre bonne question est si le même problème a été signalé pour n’importe quel Java, Objective-C ou Swift applications. Dans ce cas, le problème est très probablement partie d’Android ou iOS lui-même plutôt que de Xamarin.

1. <a name="note-4" />[*^*](#ref-4) Voici quelques exemples d’informations à inclure :

    1. Pour les erreurs qui se produisent lors de la création d’un projet, veuillez inclure le texte complet [sortie de génération diagnostic](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) sur le rapport de bogue.
    
    1. Pour que les erreurs qui se produisent lors de la génération ou du débogage d’un projet iOS à partir de Visual Studio, exécutez _aide > Xamarin > journaux Zip_ after atteindre l’erreur et inclure le fichier .zip sur le rapport de bogue.
    
    1. Pour les exceptions ou se bloque dans les applications Android ou iOS, indiquez les informations pertinentes «[déboguer les journaux pour les applications Xamarin.Android et Xamarin.iOS](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps). »

1. <a name="note-5" />[*^*](#ref-5) Si possible pour votre problème particulier, une excellente option est de recréer le problème en ajoutant un petit nombre de fichiers à partir de votre solution d’origine dans une toute nouvelle solution. L’équipe Xamarin est souvent être en mesure d’identifier les problèmes de même sur les cas de test plus volumineux (en supposant que les étapes de reproduction sont clairement expliqués), mais plus simple des cas de test donnent que les meilleures chances que le bogue sera résolu rapidement.


1. <a name="note-6" />[*^*](#ref-6) S’il s’agit _pas_ possible de reproduire le problème en ajoutant un petit nombre de fichiers à une toute nouvelle solution, puis vous pouvez compresser et attacher le dossier de solution dans son intégralité pour votre application complète. Supprimez le `bin`, `obj`, `Components`, et `packages` dossiers pour rendre le fichier zip de fichiers plus petits. (L’IDE et le processus de génération seront généralement restaurer ou recréer le contenu de ces dossiers en fonction des besoins). Vous pouvez également supprimer autant code et ressources de fichiers à partir du projet que vous le souhaitez, tant que la solution résultante montre toujours le problème d’origine.

