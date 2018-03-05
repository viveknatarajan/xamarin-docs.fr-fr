---
title: "e/s de fonctionnalités de sécurité et confidentialité"
description: "Cet article décrit le travail avec la sécurité et confidentialité dans iOS et comment ils affectent une application Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: acdcdc2b76a995ca324532c6a034b2fdf8e21db5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="ios-security-and-privacy-features"></a>e/s de fonctionnalités de sécurité et confidentialité

_Cet article décrit le travail avec la sécurité et confidentialité dans iOS et comment ils affectent une application Xamarin.iOS._

Apple a apporté plusieurs améliorations qui aideront le développeur d’améliorer la sécurité de leurs applications et de garantir la confidentialité de l’utilisateur final pour la sécurité et confidentialité dans iOS 10 (et supérieur). Cet article décrit l’implémentation de ces fonctionnalités dans une application Xamarin.iOS.

Les rubriques suivantes sont traitées en détail :

- [Améliorations générales](#General-Enhancements)
- [Accès aux données d’utilisateur privé](#Accessing-Private-User-Data)
    - [Définition de clés de confidentialité](#Setting-Privacy-Keys)
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>Améliorations générales

Les modifications générales suivantes ont été apportées à la sécurité et confidentialité dans iOS 10 :

- Le Common données sécurité Architecture (CDSA) API a été déconseillée et doit être remplacé par l’API SecKey pour générer des clés asymétriques.
- La nouvelle `NSAllowsArbitraryLoadsInWebContent` clé peut être ajouter à la d’une application `Info.plist` de fichier et autorise les pages web charger correctement lors de la protection de la sécurité de Transport Apple (ATS) est toujours activée pour le reste de l’application. Pour plus d’informations, consultez notre [sécurité du Transport application](~/ios/app-fundamentals/ats.md) documentation.
- Étant donné que le nouveau Presse-papiers dans iOS 10 et macOS Sierra permet à l’utilisateur de copier et coller entre les appareils, l’API a été étendue pour permettre le Presse-papiers afin de pouvoir être limité à un périphérique spécifique et être horodaté à effacer automatiquement à un moment donné. En outre, les tables de montage nommés ne sont plus conservées et doivent être remplacés par les conteneurs de la table de montage partagés.
- Pour toutes les connexions SSL/TLS, le chiffrement symétrique RC4 est maintenant désactivé par défaut. En outre, l’API de Transport sécurisé ne prend plus en SSLv3 et il est recommandé que le développeur de s’arrêter à l’aide d’un chiffrement SHA-1 et 3DES dès que possible.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>Accès aux données d’utilisateur privé

Applications qui s’exécutent sur iOS 10 (ou version ultérieure) doivent déclarer statiquement intention d’accéder à des fonctionnalités spécifiques ou des informations utilisateur en entrant une ou plusieurs clés de confidentialité dans leurs `Info.plist` fichiers expliquent à l’utilisateur pour lesquelles l’application souhaite accéder.

> [!IMPORTANT]
> **Remarque** les applications qui ne parviennent pas à fournir les clés requises va être en mode silencieux interrompues par le système lorsqu’ils tentent d’accéder à l’une des fonctions restreintes ou les informations de l’utilisateur, _sans erreur_! Si une application démarre Échec inattendu sur iOS 10, assurez-vous que toutes les requis `Info.plist` ont été spécifiés.

La confidentialité suivante liées au clés sont disponibles :

- **Confidentialité - l’utilisation de la musique d’Apple Description** (`NSAppleMusicUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder à la bibliothèque multimédia de l’utilisateur.
- **Confidentialité - Description de l’utilisation de périphériques Bluetooth** (`NSBluetoothPeripheralUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder Bluetooth sur l’appareil de l’utilisateur.
- **Confidentialité - Description de l’utilisation de calendriers** (`NSCalendarsUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application veut accéder au calendrier de l’utilisateur.
- **Confidentialité - Description de l’utilisation de la caméra** (`NSCameraUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder à photo l’appareil.
- **Confidentialité - Description de l’utilisation de Contacts** (`NSContactsUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder aux contacts de l’utilisateur.
- **Confidentialité - Description de l’utilisation du partage de contrôle d’intégrité** (`NSHealthShareUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder aux données de contrôle d’intégrité de l’utilisateur. Pour plus d’informations, consultez le site d’Apple [référence de classe HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Confidentialité - Description de l’utilisation de mise à jour d’intégrité** (`NSHealthUpdateUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite modifier les données de contrôle d’intégrité de l’utilisateur. Pour plus d’informations, consultez le site d’Apple [référence de classe HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Confidentialité - Description de l’utilisation de HomeKit** (`NSHomeKitUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder aux données de Configuration de l’utilisateur HomeKit.
- **Confidentialité - toujours l’utilisation de Description de l’emplacement** (`NSLocationAlwaysUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite aient toujours accès à l’emplacement de l’utilisateur.
- [Deprecated] **Confidentialité - Description de l’utilisation de l’emplacement** (`NSLocationUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder à l’emplacement de l’utilisateur. *Remarque : Cette clé a été déconseillée dans iOS 8 (et supérieur). Utilisez `NSLocationAlwaysUsageDescription` ou `NSLocationWhenInUseUsageDescription` à la place.*
- **Confidentialité - emplacement lors de la Description de l’utilisation d’utilisation** (`NSLocationWhenInUseUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder à l’emplacement de l’utilisateur en cours d’exécution.
- [Deprecated] **Confidentialité - Description de l’utilisation de la bibliothèque Media** -permet au développeur décrire la raison pour laquelle l’application souhaite accéder à la bibliothèque multimédia de l’utilisateur. *Remarque : Cette clé a été déconseillée dans iOS 8 (et supérieur). Utilisez `NSAppleMusicUsageDescription` à la place.*
- **Confidentialité - Description de l’utilisation de Microphone** (`NSMicrophoneUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application veut accéder au microphone de périphériques.
- **Confidentialité - Description de l’utilisation de mouvement** (`NSMotionUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder accéléromètre de l’appareil.
- **Confidentialité - Description de l’utilisation de la bibliothèque Photo** (`NSPhotoLibraryUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder à la bibliothèque de photo de l’utilisateur.
- **Confidentialité - Description de l’utilisation de rappels** (`NSRemindersUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite accéder à des rappels de l’utilisateur.
- **Confidentialité - Description de l’utilisation de Siri** (`NSSiriUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite envoyer des données de l’utilisateur Siri.
- **Confidentialité - Description de l’utilisation de la reconnaissance vocale** (`NSSpeechRecognitionUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite envoyer des données de l’utilisateur aux serveurs de reconnaissance vocale d’Apple.
- **Confidentialité - Description de l’utilisation du fournisseur TV** (`NSVideoSubscriberAccountUsageDescription`)-permet au développeur décrire la raison pour laquelle l’application souhaite le compte d’utilisateur TV fournisseur d’accès.

Pour plus d’informations sur l’utilisation de `Info.plist` clés, consultez Apple [référence de clé de liste de propriété informations](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1).

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>Définition de clés de confidentialité

Prenons l’exemple suivant de l’accès HomeKit sur iOS 10 (et supérieur), le développeur doit ajouter la `NSHomeKitUsageDescription` clé pour l’application `Info.plist` de fichier et fournir une chaîne de déclaration pourquoi l’application souhaite accéder à la base de données de l’utilisateur HomeKit. Cette chaîne s’affiche à l’heure de l’utilisateur à la première qu'exécution de l’application :

[ ![](security-privacy-images/info01.png "Une alerte de NSHomeKitUsageDescription exemple")](security-privacy-images/info01.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.iOS for Visual Studio actuel ne prend pas en charge la modification de l’amélioration de la sécurité `Info.plist` paramètres à partir de l’IDE, donc, la solution de contournement suivante est requise :

1. Ouvrez le `Info.plist` fichier dans un éditeur de texte externe.
2. Avant la dernière `</dict>` nœud, ajoutez le nœud suivant : `<key>NSHomeKitUsageDescription</key>`
3. Ajoutez le nœud suivant pour fournir la description : `<string>Allows the app to control HomeKit enabled devices.</string>`
4. Le `Info.plist` fichier doit se présenter comme suit : 

    [ ![](security-privacy-images/info02vs.png "Le fichier Info.plist doit ressembler à ce qui suit")](security-privacy-images/info02vs.png)
4. Enregistrez les modifications dans le fichier.
5. Revenez à Visual Studio et la recompilation de l’application.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour définir toutes les clés de confidentialité, procédez comme suit :

1. Double-cliquez sur le `Info.plist` de fichiers dans le **l’Explorateur de solutions** à ouvrir pour le modifier.
2. Au bas de l’écran, basculez vers le **Source** vue.
3. Ajouter un nouveau **entrée** à la liste.
4. Dans la liste déroulante, sélectionnez une clé privée (tel que **confidentialité - Description de l’utilisation de HomeKit**) : 

    [ ![](security-privacy-images/info02.png "Sélectionnez une clé privée")](security-privacy-images/info02.png)
5. Entrez une description pour la raison pour laquelle l’application souhaite accéder aux informations de fonctionnalité ou un utilisateur données : 

    [ ![](security-privacy-images/info03.png "Entrez une description")](security-privacy-images/info03.png)
6. Enregistrez les modifications dans le fichier.

-----

> [!IMPORTANT]
> **Remarque :** dans l’exemple fourni ci-dessus, Échec de définition de la `NSHomeKitUsageDescription` clé dans le `Info.plist` fichier entraînerait l’application _en mode silencieux défaillant_ (qui est fermé par le système lors de l’exécution) sans erreur lorsque exécutez dans iOS 10 (ou supérieur).

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a couvert la sécurité et la confidentialité modifications qu’Apple iOS 10 et leur impact sur une application Xamarin.iOS.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
