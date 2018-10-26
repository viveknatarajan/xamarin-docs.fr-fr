---
title: 'Pourquoi ma build iOS échoue-t-elle avec : aucun clés de signature de code iPhone valide ne trouvée dans le trousseau ?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: fe267db1f83695b3d0e8f3d828f91e01b56ba8ee
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115430"
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>Pourquoi ma build iOS échoue-t-elle avec : aucun clés de signature de code iPhone valide ne trouvée dans le trousseau ?

## <a name="cause-of-the-error"></a>Cause de l’erreur
Ce message d’erreur se produit lorsque le projet en question a besoin d’informations d’identification de signature de code valides mais ne parvenez pas à les trouver. Signature de code est requise pour le test et de déploiements sur des appareils iOS physiques ; mais aussi Ad-hoc & application stocker les builds. 


### <a name="provisioning-devices"></a>Approvisionnement d’appareils
Si vous n’avez pas configuré un appareil iOS avant, le guide suivant décriront le processus pas à pas complet : [Guide de mise en service des appareils](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>Bogue lors de l’utilisation du simulateur iOS

> [!NOTE]
> Ce problème a été résolu dans les versions récentes de Xamarin pour Visual Studio. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de générer la sortie de journal.


Un bogue s’est produite dans Xamarin.Visual Studio 3.11, qui a provoqué le projet iOS dans un modèle de Xamarin.Forms pour ajouter que le fichier Entitlements.plist à simulateur codesign builds ; ce qui bloque l’utilisation du simulateur de test.

### <a name="how-to-fix"></a>Comment la corriger
Vous pouvez contourner ce problème en supprimant le `<CodesignEntitlements>` indicateur dans le débogage des builds dans le fichier .csproj. Vous pouvez procéder comme suit :

*Avertissement : Erreur dans les fichiers .csproj peut résilier votre projet, il est conseillé de sauvegarder vos fichiers avant de tenter cette.*

1. Cliquez avec le bouton droit sur le projet iOS dans le volet de la solution et sélectionnez **décharger le projet**
2. Avec le bouton droit, cliquez sur le projet à nouveau et sélectionnez **modifier de [ProjectName] .csproj**
3. Recherchez les éléments PropertyGroup déboguer, ils doivent démarrer avec les indicateurs ressemblent à ceci :
   - Débogage : `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Mise en production : `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. Dans chacun des builds qui utilisent le simulateur, supprimez ou commentez à la propriété suivante : `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Recharger le projet et vous pourrez déployer sur le simulateur.

### <a name="next-steps"></a>Étapes suivantes
Pour obtenir une assistance supplémentaire, pour nous contacter, ou si le reste de ce problème même après utilisant les informations ci-dessus, veuillez consulter [quelles options de prise en charge sont disponibles pour Xamarin ?](~/cross-platform/troubleshooting/support-options.md) pour plus d’informations sur les options de contact, des suggestions, ainsi que comment signaler un bogue nouveau si nécessaire. 
