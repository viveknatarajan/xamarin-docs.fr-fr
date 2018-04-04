---
title: Projets d’installation Windows
description: Ajout de nouveaux projets Windows à une solution Xamarin.Forms existante
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 0ad8dedc2e92005473f8836c3cdd590ce4cab5ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="setup-windows-projects"></a>Projets d’installation Windows

_Ajout de nouveaux projets Windows à une solution Xamarin.Forms existante_

Anciens projets Xamarin.Forms (ou celles créées sur Mac OS&nbsp;X) n’aura pas ces réglages de projets Windows.

Cela signifie que vous devez ajouter manuellement ces types de projet pour créer des applications Windows 8.1, Windows Phone 8.1 et Windows 10 (UWP).

## <a name="add-a-windows-81-app"></a>Ajouter une application Windows 8.1

* Si vous avez utilisé le modèle de bibliothèque de classes portables, [mettre à jour le profil](#pcl), puis
* [Ajouter une application Windows 8.1](~/xamarin-forms/platform/windows/installation/tablet.md) facteurs de forme tablet/bureau.

## <a name="add-a-windows-phone-81-app"></a>Ajouter un Windows Phone 8.1 app

* Si vous avez utilisé le modèle de bibliothèque de classes portables, [mettre à jour le profil](#pcl), puis
* [Ajouter un Windows Phone 8.1 app](~/xamarin-forms/platform/windows/installation/phone.md)

## <a name="add-a-universal-windows-platform-uwp-app"></a>Ajouter un Universal Windows Platform (UWP) application

* Génération [UWP](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx) applications nécessite Visual Studio 2015 s’exécutant sur Windows 10.
* Si vous avez utilisé le modèle de bibliothèque de classes portables, [mettre à jour le profil](#pcl), puis
* [Ajouter un Windows Universal application de plateforme](~/xamarin-forms/platform/windows/installation/universal.md)

<a name="pcl" />

### <a name="update-your-pcl-profile"></a>Mettre à jour votre profil de bibliothèque de classes portables

Si votre application Xamarin.Forms existante utilisé le modèle de bibliothèque de classes Portable (PCL), vous devez mettre à jour son profil.

1. **avec le bouton droit > Propriétés** (vos paramètres existants peuvent varier.)

  ![](images/targets.png "Cibles de la bibliothèque de classes portables")

2. Cliquez sur le **modification...**  bouton

3. Vérifiez le **Windows 8** et **Windows Phone 8.1** options sont sélectionnées (et **Windows Phone Silveright** est *décochée*) :

  ![](images/pcl.png "Options de la bibliothèque PCL cible")

4. Appuyez sur **OK** et enregistrez les modifications.

Cela équivaut à **profil 111** si vous configurez votre bibliothèque de classes portables dans Visual Studio pour Mac à l’aide de la liste déroulante.

  ![](images/pcl-xs.png "Profil de bibliothèque PCL 111")

**Remarque :** si votre solution contient toujours un projet Windows Phone 8 Silverlight, la bibliothèque PCL doit être définie à 259 du profil. Prise en charge Windows Phone 8 Silverlight est déconseillé, il est recommandé de remplacer avec les types de projet affichés sur cette page.
