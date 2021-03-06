---
ms.openlocfilehash: aee7c7eb494af76bb450038453c19ee1ed83f2d3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61388821"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Lancez Visual Studio et créez une application Xamarin.Forms vide nommée **WebServiceTutorial**. Assurez-vous que l’application utilise bien .NET Standard comme mécanisme de code partagé.

    > [!IMPORTANT]
    > Pour les extraits C# et XAML de ce didacticiel, la solution doit se nommer **WebServiceTutorial**. L’utilisation d’un autre nom entraîne des erreurs de build quand vous copiez le code à partir de ce didacticiel dans la solution.

    Pour plus d’informations sur la bibliothèque .NET Standard créée, consultez la rubrique [Anatomie d’une application Xamarin.Forms](~/get-started/first-app/index.md) dans l’article [Immersion dans Xamarin.Forms - démarrage rapide](~/get-started/first-app/index.md).

1. Dans **l’Explorateur de solutions**, sélectionnez le projet **WebServiceTutorial**, cliquez avec le bouton droit et sélectionnez **Gérer les packages NuGet...**  :

    ![Capture d’écran de l’élément de menu Ajouter des Packages NuGet sélectionné](../images/vs/add-nuget-packages.png "Élément de menu Ajouter des Packages NuGet")

1. Dans le **Gestionnaire de package NuGet**, sélectionnez l’onglet **Parcourir**, recherchez et sélectionnez le package **Newtonsoft.Json**, puis cliquez sur le bouton **Installer** pour l’ajouter au projet :

    ![Capture d’écran du package NuGet Newtonsoft.Json dans le Gestionnaire de package NuGet](../images/vs/add-package.png "Package NuGet Newtonsoft.Json")

    Ce package sera utilisé pour incorporer la désérialisation JSON dans l’application.

1. Générez la solution pour vérifier qu’elle ne contient aucune erreur.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

1. Lancez Visual Studio pour Mac et créez une application Xamarin.Forms vide nommée **WebServiceTutorial**. Assurez-vous que l’application utilise bien .NET Standard comme mécanisme de code partagé.

    > [!IMPORTANT]
    > Pour les extraits C# et XAML de ce didacticiel, la solution doit se nommer **WebServiceTutorial**. L’utilisation d’un autre nom entraîne des erreurs de build quand vous copiez le code à partir de ce didacticiel dans la solution.

    Pour plus d’informations sur la bibliothèque .NET Standard créée, consultez la rubrique [Anatomie d’une application Xamarin.Forms](~/get-started/first-app/index.md) dans l’article [Immersion dans Xamarin.Forms - démarrage rapide](~/get-started/first-app/index.md).

1. Dans **Panneau Solutions**, sélectionnez le projet **WebServiceTutorial**, cliquez avec le bouton droit et sélectionnez **Gérer les packages NuGet...**  :

    ![Capture d’écran de l’élément de menu Ajouter des Packages NuGet sélectionné](../images/vsmac/add-nuget-packages.png "Élément de menu Ajouter des Packages NuGet")

1. Dans la fenêtre **Ajouter des packages**, recherchez et sélectionnez le package NuGet **Newtonsoft.Json**, puis cliquez sur le bouton **Ajouter un package** pour l’ajouter au projet :

    ![Capture d’écran du package NuGet Newtonsoft.Json dans le Gestionnaire de package NuGet](../images/vsmac/add-package.png "Package NuGet Newtonsoft.Json")

    Ce package sera utilisé pour incorporer la désérialisation JSON dans l’application.

1. Générez la solution pour vérifier qu’elle ne contient aucune erreur.
