---
title: "Étape 1. Inscrire une application pour utiliser Azure Active Directory"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 52d06dc6125f91f98e8f3ee8b4f91ad7b52347a3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>Étape 1. Inscrire une application pour utiliser Azure Active Directory

1. Accédez à [windowsazure.com](https://manage.windowsazure.com) et connectez-vous avec votre Account Microsoft ou un compte d’organisation dans le portail Azure. Si vous n’avez pas un abonnement Azure, vous pouvez obtenir une version d’évaluation de [azure.com](http://www.azure.com)

2. Une fois connecté, accédez à la **Active Directory** section (1) et sélectionnez le répertoire où vous souhaitez enregistrer l’application (2)

  [ ![](register-images/01.-active-directory-in-azure-portal-sml.jpg "section et sélectionnez le répertoire où vous souhaitez enregistrer l’application")](register-images/01.-active-directory-in-azure-portal.jpg)

3. Cliquez sur **ajouter** pour créer la nouvelle application, puis sélectionnez **ajouter une application développée par mon organisation**

  [ ![](register-images/02.-add-new-application-sml.jpg "Ajouter une application développée par mon organisation")](register-images/02.-add-new-application.jpg)

4. Dans l’écran suivant, nommez votre application (par exemple). XAM-DEMO).
  Veillez à sélectionner **Application cliente Native** comme type d’application.

  ![](register-images/03.-app-name.jpg "Vérifiez que vous sélectionnez l’Application cliente Native en tant que le type d’application")

5. Sur l’écran final, vous devez fournir un **URI de redirection* qui est unique à votre application comme il retournera à cet URI lors de l’authentification est terminée.

  ![](register-images/04.-app-redirect.jpg "Sur l’écran final, fournissez un URI de redirection qui est unique à votre application car elle retournera à cet URI lors de l’authentification est terminée")

6. Une fois que l’application est créée, accédez à la **configurer** onglet. Notez le **ID Client** que nous allons utiliser dans notre application plus tard. En outre, dans cet écran, vous pouvez donner à votre application mobile à accéder à Active Directory ou ajouter une autre application, comme les API Web ou Office 365, ce qui peut être utilisé par les applications mobiles une fois que l’authentification est terminée.

    ![](register-images/05.-configure.jpg "En outre, dans cet écran, vous pouvez donner à votre application mobile à accéder à Active Directory ou ajouter une autre application, comme les API Web ou Office 365")



## <a name="related-links"></a>Liens associés

- [Exemple de Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
