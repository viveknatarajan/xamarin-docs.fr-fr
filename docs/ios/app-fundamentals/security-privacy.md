---
title: iOS fonctionnalités de sécurité et confidentialité
description: Ce document décrit les fonctionnalités de sécurité et confidentialité d’e/s et explique comment les utiliser avec Xamarin.iOS. Il examine les mises à jour effectuées dans iOS 10 et comment accéder aux données utilisateur privées.
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 1c88a5c16dbe5559f0b551a97a27ebb893991187
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120500"
---
# <a name="ios-security-and-privacy-features"></a>iOS fonctionnalités de sécurité et confidentialité

_Cet article aborde l’utilisation avec la sécurité et confidentialité dans iOS et comment elles affectent une application Xamarin.iOS._

Apple a apporté plusieurs améliorations à la sécurité et confidentialité dans iOS 10 (et supérieur) qui aideront les développeurs à améliorer la sécurité de leurs applications et garantir la confidentialité de l’utilisateur final. Cet article décrit l’implémentation de ces fonctionnalités dans une application Xamarin.iOS.
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>Améliorations générales

Les modifications générales suivantes ont été apportées à la sécurité et confidentialité dans iOS 10 :

- Le Common Data Security Architecture (CDSA) API a été déconseillée et doit être remplacé par l’API SecKey pour générer des clés asymétriques.
- La nouvelle `NSAllowsArbitraryLoadsInWebContent` clé peut être ajouter à la d’une application **Info.plist** de fichiers et autorise les pages web se charge correctement alors que la protection de la sécurité de Transport d’Apple (ATS) est toujours activée pour le reste de l’application. Pour plus d’informations, consultez notre [App Transport Security](~/ios/app-fundamentals/ats.md) documentation.
- Étant donné que le nouveau Presse-papiers dans iOS 10 et macOS Sierra permet à l’utilisateur copier et coller entre les appareils, l’API a été développée pour autoriser un Presse-papiers pour être limité à un appareil spécifique et être horodaté à effacer automatiquement à un moment donné. En outre, les tables de montage nommés ne sont plus conservées et doivent être remplacés par les conteneurs de la table de montage partagés.
- Pour toutes les connexions SSL/TLS, le chiffrement symétrique RC4 est désormais désactivé par défaut. En outre, l’API de Transport sécurisé ne gère plus SSLv3 et il est recommandé que le développeur de cesser d’utiliser le chiffrement SHA-1 et 3DES dès que possible.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>Accès aux données d’utilisateur privé

Applications qui s’exécutent sur iOS 10 (ou version ultérieure) doivent déclarer statiquement leur intention d’accéder à des fonctionnalités spécifiques ou des informations utilisateur en entrant une ou plusieurs clés de confidentialité dans leurs **Info.plist** fichiers expliquent à l’utilisateur pourquoi l’application souhaite obtenir accès.

> [!IMPORTANT]
> Les applications qui ne parviennent pas à fournir les clés requises seront en mode silencieux arrêtées par le système lorsqu’ils tentent d’accéder à l’une des fonctions restreintes ou les informations de l’utilisateur, _sans erreur_! Si une application démarre échoue de façon inattendue sur iOS 10, vous assurer que tous les paramètres requis **Info.plist** ont été spécifiés.

La confidentialité suivante liées aux clés sont disponibles :

- **Confidentialité - utilisation d’Apple Music Description** (`NSAppleMusicUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder à la bibliothèque multimédia de l’utilisateur.
- **Confidentialité - Description de l’utilisation de périphériques Bluetooth** (`NSBluetoothPeripheralUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder Bluetooth sur l’appareil de l’utilisateur.
- **Confidentialité - Description de l’utilisation des calendriers** (`NSCalendarsUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder au calendrier de l’utilisateur.
- **Confidentialité - Description de l’utilisation de caméra** (`NSCameraUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder à photo l’appareil.
- **Confidentialité - Description de l’utilisation de Contacts** (`NSContactsUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder aux contacts de l’utilisateur.
- **Confidentialité - Description de l’utilisation du partage d’intégrité** (`NSHealthShareUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder aux données de contrôle d’intégrité de l’utilisateur. Pour plus d’informations, consultez le site d’Apple [référence de classe HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Confidentialité - Description de l’utilisation de mise à jour d’intégrité** (`NSHealthUpdateUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite modifier les données de contrôle d’intégrité de l’utilisateur. Pour plus d’informations, consultez le site d’Apple [référence de classe HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Confidentialité - Description de l’utilisation de HomeKit** (`NSHomeKitUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder aux données de Configuration de l’utilisateur HomeKit.
- **Confidentialité - Description d’emplacement toujours l’utilisation** (`NSLocationAlwaysUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application veut aient toujours accès à l’emplacement de l’utilisateur.
- [Déconseillé] **Confidentialité - Description de l’utilisation d’emplacement** (`NSLocationUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder à l’emplacement de l’utilisateur. *Remarque : Cette clé a été déconseillée dans iOS 8 (et supérieur). Utilisez `NSLocationAlwaysUsageDescription` ou `NSLocationWhenInUseUsageDescription` à la place.*
- **Confidentialité - emplacement lors de la Description de l’utilisation utilisation** (`NSLocationWhenInUseUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder à l’emplacement de l’utilisateur pendant son exécution.
- [Déconseillé] **Confidentialité - Description de l’utilisation de bibliothèque multimédia** -permet au développeur décrire la raison pour laquelle l’application souhaite accéder à la bibliothèque multimédia de l’utilisateur. *Remarque : Cette clé a été déconseillée dans iOS 8 (et supérieur). Utilisez `NSAppleMusicUsageDescription` à la place.*
- **Confidentialité - Description de l’utilisation de Microphone** (`NSMicrophoneUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder au microphone d’appareils.
- **Confidentialité - Description de l’utilisation de mouvement** (`NSMotionUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder accéléromètre de l’appareil.
- **Confidentialité - Description de l’utilisation de bibliothèque de photos** (`NSPhotoLibraryUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder à la bibliothèque de photos de l’utilisateur.
- **Confidentialité - Description de l’utilisation de rappels** (`NSRemindersUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder à des rappels de l’utilisateur.
- **Confidentialité - Description de l’utilisation de Siri** (`NSSiriUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite envoyer des données de l’utilisateur à Siri.
- **Confidentialité - Description de l’utilisation de la reconnaissance vocale** (`NSSpeechRecognitionUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite envoyer des données de l’utilisateur aux serveurs de reconnaissance vocale d’Apple.
- **Confidentialité - Description de l’utilisation du fournisseur TV** (`NSVideoSubscriberAccountUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder le compte d’utilisateur TV fournisseur.

Pour plus d’informations sur l’utilisation de **Info.plist** clés, consultez d’Apple [référence de clé de liste de propriété d’informations](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1).

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>Définition des clés de confidentialité

Prenons l’exemple suivant de l’accès à HomeKit sur iOS 10 (et supérieur), le développeur doit ajouter la `NSHomeKitUsageDescription` clé à l’application **Info.plist** de fichiers et de fournir une chaîne de déclaration pourquoi l’application souhaite accéder à HomeKit l’utilisateur base de données. Cette chaîne sera présentée à l’utilisateur la première exécution de l’application :

![Un exemple d’alerte NSHomeKitUsageDescription](security-privacy-images/info01.png "NSHomeKitUsageDescription un exemple d’alerte")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin.iOS pour Visual Studio actuellement ne prend pas en charge la modification de la **Info.plist** éditeur de manifeste de clés de confidentialité à partir de l’e/s par défaut. Au lieu de cela, vous devrez utiliser l’éditeur PList générique, par conséquent, procédez comme suit :

1. Avec le bouton droit sur le **Info.plist** de fichiers dans le **l’Explorateur de solutions** et sélectionnez **ouvrir avec...** .
2. Sélectionnez le **éditeur PList de générique** dans la liste des programmes pour ouvrir le fichier, puis cliquez sur **OK**.

    ![Sélectionnez l’éditeur PList générique](security-privacy-images/InfoEditorSelectionVs.png "sélectionnez l’éditeur PList générique")
3. Cliquez sur le **+** bouton sur la dernière ligne dans l’éditeur pour ajouter une nouvelle entrée à la liste. Cette méthode sera appelée « Propriété personnalisé », avec le Type défini sur `String` et une valeur vide.
4. Cliquez sur le nom de propriété et une liste déroulante s’affiche.
5. Dans la liste déroulante, sélectionnez une clé de confidentialité (tel que **confidentialité - Description de l’utilisation de HomeKit**) : 

    ![Sélectionnez une clé de confidentialité](security-privacy-images/InfoPListEditorSelectKey.png "sélectionner une clé de confidentialité")
6. Entrez une description dans la colonne de valeur pour la raison pour laquelle l’application souhaite accéder aux informations de fonctionnalité ou un utilisateur données : 

    ![Entrez une description](security-privacy-images/InfoPListSetValue.png "Entrez une description")
7. Enregistrez les modifications dans le fichier.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Pour définir toutes les clés de confidentialité, procédez comme suit :

1. Dans l’**Explorateur de solutions**, double-cliquez sur le fichier **Info.plist** pour l’ouvrir et le modifier.
2. En bas de l’écran, basculez vers le **Source** vue.
3. Ajouter un nouveau **entrée** à la liste.
4. Dans la liste déroulante, sélectionnez une clé de confidentialité (tel que **confidentialité - Description de l’utilisation de HomeKit**) : 

    ![Sélectionnez une clé de confidentialité](security-privacy-images/info02.png "sélectionner une clé de confidentialité")
5. Entrez une description pour la raison pour laquelle l’application souhaite accéder aux informations de fonctionnalité ou un utilisateur données : 

    ![Entrez une description](security-privacy-images/info03.png "Entrez une description")
6. Enregistrez les modifications dans le fichier.

-----

> [!IMPORTANT]
> Dans l’exemple indiqué ci-dessus, définissez le `NSHomeKitUsageDescription` clé dans le **Info.plist** fichier entraînerait l’application _échouent silencieusement_ (en cours de fermeture par le système lors de l’exécution) sans erreur quand exécuter dans iOS 10 ( ou version ultérieure).

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a présenté la sécurité et confidentialité apportées qu’Apple a dans iOS 10 et leur impact sur une application Xamarin.iOS.

## <a name="related-links"></a>Liens associés

- [Exemples iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
