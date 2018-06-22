---
title: Questions fréquemment posées
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 5a36c6ab14fdc7bfc5916456670be9c8fe4476ff
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34049881"
---
# <a name="frequently-asked-questions"></a>Questions fréquemment posées


## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Puis-je mettre à jour le modèle par défaut Xamarin.Forms vers un package NuGet plus récent ?](update-forms-template.md)
Ce guide utilise le modèle de bibliothèque Standard de .NET Xamarin.Forms par exemple, mais la même méthode générale fonctionne également pour le modèle de projet partagé de Xamarin.Forms. 

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Pourquoi le concepteur Visual Studio XAML ne fonctionne-t-il pas pour les fichiers XAML de Xamarin.Forms ?](forms-xaml-designer.md)
Xamarin.Forms ne prend actuellement en charge les concepteurs visuels pour les fichiers XAML.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedlyandroid-linkassemblies-errormd"></a>[Erreur de génération Android : échec inattendu de la tâche « LinkAssemblies »](android-linkassemblies-error.md)
Vous pouvez voir un message d’erreur `The "LinkAssemblies" task failed unexpectedly` lorsque la génération d’un projet Xamarin.Android qui utilise les formulaires. Cela se produit lorsque l’éditeur de liens est active (en général sur un *version* build pour réduire la taille du package d’application) ; et il se produit car les cibles Android ne sont pas mis à jour vers la dernière version de framework. 


## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>[« Pourquoi mon projet Xamarin.Forms.Maps Android n’échoue avec COMPILETODALVIK : erreur inattendue de niveau supérieur ? »](maps-compiletodalvik-error.md)
Cette erreur peut se produire dans la zone d’erreur de Visual Studio pour Mac ou dans la fenêtre de sortie de génération de Visual Studio ; dans les projets Android à l’aide de Xamarin.Forms.Maps. Il est généralement résolu en augmentant la taille du tas Java pour votre projet Xamarin.Android.

