---
title: "Nouveau système de comptage de référence"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: dd4aff683b0cfb797147d32f282be1aab4c30667
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="new-reference-counting-system"></a>Nouveau système de comptage de référence

Xamarin.iOS 9.2.1 a introduit une meilleure décompte de références système à toutes les applications par défaut. Il peut être utilisé pour éliminer de nombreux problèmes de mémoire qui étaient difficiles à détecter et corriger les versions antérieures de Xamarin.iOS.

## <a name="enabling-the-new-reference-counting-system"></a>L’activation de la nouvelle référence système de comptage

À compter de Xamarin 9.2.1 ref nouveau système de comptage est activé pour **tous les** applications par défaut.

Si vous développez une application existante, vous pouvez vérifier le fichier .csproj pour vous assurer que toutes les occurrences de `MtouchUseRefCounting` ont la valeur `true`, comme ci-dessous :

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

Si elle est définie sur `false` votre application ne l’utiliserez pas les nouveaux outils.

### <a name="using-older-versions-of-xamarin"></a>À l’aide de versions antérieures de Xamarin

Xamarin.iOS 7.2.1). et ci-dessus propose un aperçu amélioré notre nouvelle du comptage de références système.

**API classique :**

Pour activer ce nouveau système de comptage de référence, vérifiez le **le décompte d’extension** case à cocher se trouve dans le **avancé** onglet de votre projet **les options de Build iOS** , comme indiqué ci-dessous : 

[ ![](newrefcount-images/image1.png "Activer le nouveau système de comptage de référence")](newrefcount-images/image1.png)

Notez que ces options ont été supprimées dans les versions plus récentes de Visual Studio pour Mac.

 **[API unifiée :](~/cross-platform/macios/unified/index.md)**

 Décompte nouvelle extension est nécessaire pour l’API unifiée et doit être activé par défaut. Les versions antérieures de votre IDE ne peuvent pas avoir cette valeur automatiquement vérifiée et vous devrez placer vous-même une vérification par celui-ci.

    
> [!IMPORTANT]
> **Remarque :** une version antérieure de cette fonctionnalité depuis autour MonoTouch 5.2 mais elle était uniquement disponible pour **sgen** comme un aperçu expérimentale. Cette nouvelle version améliorée est désormais également disponible pour le **Boehm** le garbage collector.


Il historiquement deux types d’objets gérés par Xamarin.iOS : celles qui ont été simplement un wrapper autour d’un objet natif (objets homologues) et ceux qui étendu ou incorporé de nouvelles fonctionnalités (objets dérivés) - généralement en conservant l’état de la mémoire supplémentaire. Auparavant, il était possible que nous avons augmenter un objet homologue avec état (par exemple en ajoutant un gestionnaire d’événement c#), mais que nous permettent de l’objet accédez non référencés et puis collectées. Cela peut entraîner un blocage ultérieurement (par exemple, si le runtime Objective-C rappelé dans l’objet managé).

Le nouveau système met automatiquement à niveau les objets homologues dans des objets qui sont gérés par le runtime lorsqu’elles stockent les informations supplémentaires.

Cette approche résout différentes pannes qui se sont produits dans des situations telles que celle-ci :

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

Sans l’extension de nombre de référence, ce code bloquait car `cell` devient pouvant être collecté et donc son `TouchDown` délégué, qui convertit en un pointeur en suspens.

L’extension de nombre de référence garantit l’objet managé reste actif et empêche sa collection, autant de l’objet natif est conservée par le code natif.

Le nouveau système supprime également la nécessité de *la plupart des* privé champs utilisés dans les liaisons - qui est l’approche par défaut afin de maintenir l’instance de la sauvegarde. L’éditeur de liens managé est assez intelligente pour supprimer tous ceux *inutiles* extension du nombre de champs à partir d’applications à l’aide de la nouvelle référence.

Cela signifie que les instances de chaque objet géré consomment moins de mémoire qu’avant. Elle résout également un problème connexe où certains champs de stockage contiendrait les références qui n’étaient pas nécessaire en plus par le runtime Objective-C, ce qui permet de libérer de la mémoire.
