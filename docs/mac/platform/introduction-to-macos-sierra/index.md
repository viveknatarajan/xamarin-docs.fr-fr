---
title: Introduction à macOS Sierra
description: Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans macOS Sierra pour les développeurs Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 5a944fd8f7dcfdcbb3f025c92b4afac35673416f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111010"
---
# <a name="introduction-to-macos-sierra"></a>Introduction à macOS Sierra

Avec le nouveau macOS Sierra, le développeur peut tirer parti des nouvelles API permettant aux utilisateurs finaux d’interagir avec leurs applications et les sites Web de manières précédemment pas disponibles. Par exemple, Apple permet désormais de sites Web offrir aux clients la possibilité de payer en toute sécurité via Apple Pay et les améliorations apportées à la valorisation de l’infrastructure complète les graphiques d’une application et de l’informatique potentiels. 

Pour plus d’informations sur macOS Sierra, veuillez consulter d’Apple [macOS + applications](https://developer.apple.com/macos/) documentation.

<a name="Whats-New-in-macOS-Sierra" />

## <a name="whats-new-in-macos-sierra"></a>Nouveautés dans macOS Sierra

Apple a ajouté plusieurs nouvelles API et services dans macOS Sierra, ainsi que de nombreuses améliorations aux fonctionnalités existantes, y compris :

<a name="Apple-File-System" />

### <a name="apple-file-system"></a>Système de fichiers Apple

Avec macOS Sierra, Apple a publié le nouveau système de fichiers Apple comme un système de fichiers moderne pour iOS, macOS, tvOS et watchOS. Le système de fichiers Apple a été optimisé pour le stockage Flash et SSD et fournit les fonctionnalités suivantes : chiffrement renforcé, les métadonnées de la copie sur écriture, l’espace de partage, le clonage des fichiers et répertoires, des captures instantanées, dimensionnement du répertoire rapide et atomiques primitives safe-enregistrer.

Pour plus d’informations, consultez le site d’Apple [Guide de système de fichiers Apple](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999).

<a name="Apple-Pay-Enhancements" />

### <a name="apple-pay-enhancements"></a>Apple Pay améliorations

Apple a apporté plusieurs améliorations à Apple Pay dans macOS Sierra permettant aux utilisateurs d’effectuer les paiements sécurisés à partir de sites Web.

Avec macOS Sierra, plusieurs nouvelles API ont été ajoutés qui fonctionnent avec macOS Sierra, iOS et watchOS pour prendre en charge des réseaux de paiement dynamique et un nouvel environnement de test de bac à sable.

macOS Sierra inclut la nouvelle infrastructure ApplePay Javascript qui permet au développeur d’incorporer Apple Pay directement dans iOS et macOS sites Web basés sur Safari. Pour les sites Web qui prennent en charge Apple Pay, l’utilisateur peut autoriser le paiement à l’aide de leur iPhone ou Apple Watch.

Pour plus d’informations, consultez le site d’Apple [ApplePay JS Framework](https://developer.apple.com/reference/applepayjs) référence.

<a name="Building-Modern-macOS-Apps" />

### <a name="building-modern-macos-apps"></a>Création d’applications macOS modernes

Applications macOS modernes tels que le navigateur web Safari, les Pages de traitement de texte et les spread feuille utilisez ouvrent de nombreuses nouvelles technologies visant à présenter une Interface utilisateur unifiée, sensible au contexte qui ne contient plus avec des éléments d’interface utilisateur traditionnels tels que les panneaux flottants et plusieurs d’Apple Windows.

[![Un exemple d’une fenêtre à onglets de Mac](images/content08.png)](images/content08.png#lightbox)

Notre [applications macOS modernes de construction](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md) guide couvre plusieurs conseils, fonctionnalités et techniques un développeur peut utiliser pour créer une application de macOS modernes dans Xamarin.Mac.

<a name="CloudKit-Data-Sharing" />

### <a name="cloudkit-data-sharing"></a>Partage des données de CloudKit

Le framework de CloudKit a été développé dans macOS Sierra pour autoriser l’utilisateur à partager rapidement et facilement des enregistrements ou les jeux d’enregistrements à partir de leurs bases de données privée iCloud.

CloudKit fournit une interface utilisateur complète pour l’envoi et l’acceptation d’invitations enregistrement partagées et l’utilisateur a un contrôle complet en lecture/écriture sur les personnes qui ont accès aux enregistrements.

Pour plus d’informations, consultez le site d’Apple [CloudKit Framework référence](https://developer.apple.com/reference/clockkit) et [CloudKit JS Framework référence](https://developer.apple.com/reference/cloudkitjs).

> [!IMPORTANT]
> Apple [fournit des outils](https://developer.apple.com/support/allowing-users-to-manage-data/) pour aider les développeurs à gérer correctement le Règlement général sur la protection des données (RGPD) de l’Union européenne.

<a name="Safari-App-Extensions-Support" />

### <a name="safari-app-extensions-support"></a>Prise en charge des Extensions d’application Safari

Extensions d’application Safari autoriser l’application étendre le comportement du navigateur web Safari tout en étant étroitement intégré avec macOS Sierra. Dans la mesure où les Extensions d’application macOS Safari fonctionnent de la même pour iOS Safari Extensions d’application, ils sont faciles à un port d’un système vers un autre.

Pour plus d’informations, consultez le site d’Apple [Guide de programmation Safari application Extension](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319).

<a name="Security-and-Privacy-Enhancements" />

### <a name="security-and-privacy-enhancements"></a>Améliorations de la confidentialité et de sécurité

Apple a apporté plusieurs améliorations à la sécurité et confidentialité dans macOS Sierra qui aide l’application à améliorer la sécurité de l’application et de garantir la confidentialité de l’utilisateur final, y compris les éléments suivants :

- La nouvelle `NSAllowsArbitraryLoadsInWebContent` clé peut ajouter à l’application `Info.plist` de fichiers et autorise les pages web se charge correctement alors que la protection de la sécurité de Transport d’Apple (ATS) est toujours activée pour le reste de l’application.
- Le Common Data Security Architecture (CDSA) API a été déconseillée et doit être remplacé par l’API SecKey pour générer des clés asymétriques.
- Pour toutes les connexions SSL/TLS, le chiffrement symétrique RC4 est désormais désactivé par défaut. En outre, l’API de Transport sécurisé n’est plus prend en charge que les SSLv3 et il est recommandé que l’application s’arrêter à l’aide d’un chiffrement SHA-1 et 3DES dès que possible.
- Étant donné que le nouveau Presse-papiers dans iOS 10 et macOS Sierra permet à l’utilisateur copier et coller entre les appareils, l’API a été développée pour autoriser un Presse-papiers pour être limité à un appareil spécifique et être horodaté à effacer automatiquement à un moment donné. En outre, les tables de montage nommés ne sont plus conservées et doivent être remplacés par les conteneurs de la table de montage partagés.
- Si l’application accède à des données protégées (par exemple, le calendrier de l’utilisateur), il _doit_ déclarer cette intention avec la clé de valeur de chaîne de rôle correct dans son `Info.plist` fichier (`NSCalendarUsageDescription` dans le cas du calendrier).
- Développeur Signed applications qui ne sont pas remises via le Store d’application Mac peuvent désormais tirer parti de CloudKit iCloud trousseau, iCloud Drive, notifications push à distance, de droits MapKit et VPN.
- macOS Sierra prend en charge n’est plus le remettre le code externe ou des données, ainsi que l’application de signataire de code dans son archive zip ou d’une image de disque non signé comme le chemin d’accès du runtime n’est pas connu avant l’exécution.

En outre, les applications qui s’exécutent sur macOS Sierra (ou version ultérieure) doit déclarer statiquement leur intention d’accéder à des fonctionnalités spécifiques ou des informations utilisateur en entrant une ou plusieurs clés spécifiques confidentialité dans leurs `Info.plist` fichiers expliquent à l’utilisateur pourquoi l’application souhaite obtenir accès.

Étant donné que macOS Sierra partage ces modifications avec iOS 10, consultez notre iOS 10 [sécurité et confidentialité améliorations](~/ios/app-fundamentals/security-privacy.md) guide pour plus d’informations.

<a name="Smart-Card-Driver-Extension-Support" />

### <a name="smart-card-driver-extension-support"></a>Extension de pilotes de carte à puce pris en charge

Avec macOS Sierra, l’application peut créer `NSExtension` en fonction des pilotes de carte à puce qui autorise l’accès en lecture seule au contenu à partir de certains types de cartes à puce. Cette information est ensuite présentée dans le trousseau d’accès système (en remplaçant la méthode d’Architecture de sécurité de données commune déconseillée).

Pour plus d’informations, de Veuillez consultez Apple [CryptoTokenKit Framework référence](https://developer.apple.com/reference/cryptotokenkit).

<a name="Unified-Logging" />

### <a name="unified-logging"></a>Journalisation unifié

Journalisation unifiée fournit l’application avec une seule API pour la messagerie efficace sur tous les niveaux du système. Avec la journalisation unifiée, l’application a un contrôle affiné sur plusieurs niveaux de journalisation qui incluent des contrôles de confidentialité et de suivi pour faciliter le débogage de l’activité. 

La journalisation fournit la corrélation de message automatique lors de l’activité de suivi et de journalisation est utilisée ensemble.

macOS Sierra inclut une nouvelle application Console (dans les Applications/Utilities) qui est en mesure d’afficher les données du journal à partir de plusieurs sources, y compris les appareils connectés. Il prend en charge sous forme de jeton et enregistrés des recherches et affiche les connexions entre les messages connexes entre plusieurs processus.

En outre, les messages du journal peuvent être affichés et gérés à l’aide des outils de ligne de commande.

Pour plus d’informations, consultez le site d’Apple [référence journalisation](https://developer.apple.com/reference/os/1891852-logging).

<a name="Wide-Color" />

### <a name="wide-color"></a>Large couleur

macOS Sierra étend la prise en charge pour les formats de pixel de la plage étendue et des espaces d’une gamme de couleurs dans tout le système, y compris les infrastructures telles que graphismes de base, l’Image de Core, complète et AVFoundation. Prise en charge pour les appareils avec des couleurs large est davantage facilité en fournissant ce comportement dans toute la pile de graphique entière.

En outre, `AppKit` a été modifié pour fonctionner dans la nouvelle étendue **sRVB** espace colorimétrique, facilitant ainsi de mélanger les couleurs utilisées dans les gammes de couleurs large sans perte de performances significatifs.

Apple offre les meilleures pratiques suivantes lorsque vous travaillez avec des couleurs larges :

- `NSColor` maintenant utilise le sRVB espace de couleurs et ne seront plus fixer des valeurs pour le `0.0` à `1.0` plage. Si l’application s’appuie sur le comportement de pince précédent, il devront être modifiées pour macOS Sierra.
- Lorsque vous utilisez une API de bas niveau tels que les graphismes de base ou complète pour fournir le traitement d’image, l’application doit utiliser un format de couleur plage étendue espace et de pixels qui prend en charge les valeurs à virgule flottante 16 bits. Le cas échéant, l’application aura à fixer manuellement les valeurs de composant de couleur.
- Graphismes de base, Image Core et des nuanceurs de performances de métal fournissent tous des nouvelles méthodes pour la conversion entre les espaces de deux couleurs.

Pour en savoir plus, consultez notre [Introduction à large couleur](~/ios/platform/wide-color.md) guide.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Modifications d’infrastructure supplémentaires

Outre les modifications de l’infrastructure principale et les ajouts répertoriés ci-dessus, Apple a effectué de nombreuses modifications de framework mineures supplémentaires dans macOS Sierra.

Pour en savoir plus, consultez notre [des modifications supplémentaires Framework](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md) guide.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>API déconseillées

Les API suivantes ont été déconseillées dans macOS Sierra :

- Le système de fichiers HFS Standard n’est plus pris en charge.

Consultez d’Apple [v10.12 OS X différences d’API](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/APIDiffsMacOS10_12/index.html) documentation pour obtenir la liste complète des dépréciations et les modifications.

## <a name="related-links"></a>Liens associés

- [Exemples Mac](https://developer.xamarin.com/samples/mac/)
- [Quelles sont les nouveautés dans OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
