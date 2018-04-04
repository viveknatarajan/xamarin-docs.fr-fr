---
title: 'Pourquoi ma build iOS échoue avec : code valide iPhone clés de signature est introuvable dans le trousseau ?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 5334e3009906896644caa47c715f912fa379c627
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>Pourquoi ma build iOS échoue avec : code valide iPhone clés de signature est introuvable dans le trousseau ?

## <a name="cause-of-the-error"></a>Cause de l’erreur
Ce message d’erreur se produit lorsque le projet en question est vous recherchez des informations d’identification de signature de code valides, mais ne parvenez pas à les trouver. Signature de code est requise pour les tests et les déploiements sur des appareils iOS physiques ; et aussi Ad-hoc & application stocker les builds. 


### <a name="provisioning-devices"></a>Configuration des appareils
Si vous n’avez pas mis en service un appareil iOS avant, le guide suivant vous guident tout le processus complet et détaillé : [Guide de mise en service des appareils](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>Bogue lorsque vous utilisez le simulateur iOS

> [!NOTE]
> Ce problème a été résolu dans les versions récentes de Xamarin pour Visual Studio. Toutefois, si le problème se produit sur la dernière version du logiciel, veuillez soumettre un [nouveau bogue](~/cross-platform/troubleshooting/questions/howto-file-bug.md) avec votre contrôle de version complet intégral et les informations de la sortie de journal de build.


Il a été un bogue dans Xamarin.Visual Studio 3.11, ce qui a provoqué le projet iOS dans un modèle de Xamarin.Forms pour ajouter que le codesign Entitlements.plist au Simulateur génère ; effet de bloquer le test à l’aide du simulateur.

### <a name="how-to-fix"></a>Comment corriger
Vous pouvez contourner le problème en supprimant le `<CodesignEntitlements>` génère d’indicateur dans le débogage dans le fichier .csproj. Vous pouvez procéder comme suit :

*Avertissement : Les erreurs dans les fichiers .csproj peuvent endommager votre projet, par conséquent, il est judicieux de sauvegarder vos fichiers avant de tenter cette.*

1. Cliquez avec le bouton droit sur le projet iOS dans le volet de la solution et sélectionnez **décharger le projet**
2. Avec le bouton droit, cliquez sur le projet à nouveau, puis sélectionnez **modifier [NomProjet] .csproj**
3. Recherchez les éléments PropertyGroup déboguer, ils doivent démarrer avec des indicateurs qui ressemblent à ceci :
   - Déboguer : `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Mise en production : `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. Dans chacun des builds qui utilisent le simulateur, supprimez ou commentez la propriété suivante : `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Recharger le projet et vous devez être en mesure de déployer vers le simulateur.

### <a name="next-steps"></a>Étapes suivantes
Pour obtenir une assistance pour nous contacter, ou si ce problème persiste même après utilisant les informations ci-dessus, consultez [les options de support sont disponibles pour Xamarin ?](~/cross-platform/troubleshooting/support-options.md) pour plus d’informations sur les options de contact, des suggestions, ainsi que comment entrer un bogue nouveau si nécessaire. 
