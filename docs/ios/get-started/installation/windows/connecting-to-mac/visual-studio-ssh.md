---
title: Où est mon Mac ?
description: Instructions de configuration de votre Mac pour que Visual Studio génère des projets Xamarin.iOS
ms.prod: xamarin
ms.assetid: 4f792690-b3b1-4d56-a5e2-363b4f246059
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: f3e2988c9700549db24ad69277df9e412c99caed
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="wheres-my-mac"></a>Où est mon Mac ?

_Instructions de configuration de votre Mac pour que Visual Studio génère des projets Xamarin.iOS_

La version **stable** actuelle de Xamarin pour Visual Studio interagit avec votre Mac différemment des versions précédentes[^](#earlier-versions).

Pour utiliser un Mac comme Mac Agent Xamarin, vous devez activer **Session à distance** sur votre Mac.

1. Ouvrez *Spotlight* (`Cmd-Space`) et recherchez **Session à distance**, puis sélectionnez le résultat **Partage**. Les **Préférences Système** s’affichent dans le panneau **Partage**.

  ![](visual-studio-ssh-images/spotlight.png "Recherche Spotlight pour la session à distance")

2. Cochez l’option **Session à distance** dans la liste **Service** à gauche afin de permettre à Xamarin pour Visual Studio de se connecter au Mac.

  ![](visual-studio-ssh-images/sharing.png "Cocher l’option Session à distance dans la liste Service")

3. Vérifiez que l’option **Session à distance** est définie pour autoriser l’accès pour **Tous les utilisateurs**, ou que votre nom d’utilisateur ou groupe Mac figure dans la liste des utilisateurs autorisés affichée à droite.

Votre Mac doit maintenant être détectable par Visual Studio s’il se trouve sur le même réseau.
Quand Visual Studio tente de se connecter au Mac, vous êtes invité à vous connecter à partir de Visual Studio à l’aide de vos informations d’identification d’utilisateur Mac.

## <a name="where-can-i-find-more-information"></a>Comment en savoir plus ?

Plusieurs guides listés ci-dessous sont disponibles pour vous aider à configurer et à comprendre le nouvel agent :

- [Connexion à votre Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [Résolution des problèmes](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin University - Mac Agent Xamarin](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)

<a name="earlier-versions" />

## <a name="migrating-from-previous-versions"></a>Migration à partir de versions antérieures

Si vous avez utilisé des versions antérieures de Xamarin pour Visual Studio, vous devez être familiarisé avec l’application **hôte de build Xamarin.iOS** qui devait être démarrée sur votre Mac, puis *couplée* par le biais d’un code PIN avec votre instance de Visual Studio.

L’application hôte de build n’est plus nécessaire avec cette version de Xamarin pour Visual Studio.
