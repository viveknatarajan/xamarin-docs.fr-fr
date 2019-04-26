---
title: Nouveau décompte de références système dans Xamarin.iOS
description: Ce document décrit améliorée décompte de Xamarin de références système, activée par défaut dans toutes les applications Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/25/2015
ms.openlocfilehash: 8c7b1a88284156cb5d4261f18d5659ed66dfaf64
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61037182"
---
# <a name="new-reference-counting-system-in-xamarinios"></a>Nouveau décompte de références système dans Xamarin.iOS

Xamarin.iOS 9.2.1 a introduit l’améliorée de comptage de références système à toutes les applications par défaut. Il peut être utilisé pour éliminer de nombreux problèmes de mémoire qui étaient difficiles à suivre et corriger dans les versions antérieures de Xamarin.iOS.

## <a name="enabling-the-new-reference-counting-system"></a>L’activation de la nouvelle référence de système de comptage

À compter de Xamarin 9.2.1 la référence de nouveau système d’inventaire est activée pour **tous les** applications par défaut.

Si vous développez une application existante, vous pouvez vérifier le fichier .csproj pour vous assurer que toutes les occurrences de `MtouchUseRefCounting` sont définies sur `true`, comme ci-dessous :

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

Si elle est définie sur `false` votre application n’utilisera pas les nouveaux outils.

### <a name="using-older-versions-of-xamarin"></a>À l’aide de versions antérieures de Xamarin

Xamarin.iOS 7.2.1 et un aperçu de notre nouveau décompte de références système amélioré des fonctionnalités ci-dessus.

**API classique :**

Pour activer ce nouveau système de comptage de référence, cochez la **utiliser l’extension de comptage de références** case à cocher se trouve dans le **avancé** onglet de votre projet **des options Build iOS** , comme indiqué ci-dessous : 

[![](newrefcount-images/image1.png "Activer le nouveau système de comptage de référence")](newrefcount-images/image1.png#lightbox)

Notez que ces options ont été supprimées dans les versions plus récentes de Visual Studio pour Mac.

 **[API unifiée :](~/cross-platform/macios/unified/index.md)**

 La nouvelle extension de décompte est requise pour l’API unifiée et doit être activée par défaut. Les versions antérieures de votre IDE ne peuvent pas avoir cette valeur automatiquement vérifiée et il se peut que vous deviez placer vous-même une vérification par celui-ci.

    
> [!IMPORTANT]
> Une version antérieure de cette fonctionnalité depuis autour MonoTouch 5.2, mais était uniquement disponible pour **sgen** en tant qu’un aperçu expérimental. Cette nouvelle version améliorée est désormais également disponible pour le **Boehm** RÉCUPÉRATEUR de mémoire.


Par le passé ont été deux types d’objets gérés par Xamarin.iOS : ceux qui ont été simplement un wrapper autour d’un objet natif (objets homologues) et celles qui étendu ou incorporé (objets dérivés) - de nouvelles fonctionnalités généralement en conservant l’état de la mémoire supplémentaire. Auparavant, il était possible que nous aurions pu augmenter un objet homologue avec état (par exemple en ajoutant un C# Gestionnaire d’événements), mais que nous laissons l’objet accédez non référencés et puis collectées. Cela peut provoquer un incident par la suite (par exemple, si le runtime Objective-C rappelé dans l’objet managé).

Le nouveau système met automatiquement à niveau les objets homologues dans des objets qui sont gérés par le runtime lorsqu’ils stockent des informations supplémentaires.

Cette action a résolu plusieurs incidents qui se sont produits dans des situations comme celle-ci :

```csharp
class MyTableSource : UITableViewSource {
   public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath) {
        var cell = tableView.DequeueReusableCell ("myId");
        if (cell != null)
                return cell;

        cell = new UITableViewCell (UITableViewCellStyle.Default, "myId");
        var txt = new UITextField ();
        txt.TouchDown += delegate { Console.WriteLine ("...."); };
        cell.ContentView.AddSubview (txt);
        return cell;
   }
}
```

Sans l’extension de comptage de référence, ce code se bloque, car `cell` devient pouvant être collecté et donc sa `TouchDown` délégué, qui est traduit en un pointeur non résolue.

L’extension de nombre de référence garantit l’objet managé reste actif et empêche sa collection, fournie par l’objet natif est conservée par le code natif.

Le nouveau système supprime également la nécessité de *la plupart des* privé champs utilisés dans les liaisons - qui est l’approche par défaut afin de maintenir l’instance de stockage. L’éditeur de liens managé est suffisamment intelligent pour supprimer tous ceux *inutiles* extension compter les champs à partir d’applications à l’aide de la nouvelle référence.

Cela signifie que les instances de chaque objet managé consomment moins de mémoire qu’avant. Il permet également de résoudre un problème connexe, où certains champs de stockage contiendrait les références qui ont été est devenu inutile par le runtime Objective-C, ce qui complique la libérer de la mémoire.
