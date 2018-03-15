---
title: "Introduction à macOS Sierra"
description: "Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans macOS Sierra pour les développeurs de Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 161a5be643ccf5f96b04413cec5956264af6ce60
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-macos-sierra"></a>Introduction à macOS Sierra

Avec le nouveau macOS Sierra, le développeur peut profiter de nouvelles API qui permettre à l’utilisateur final interagir avec leurs applications et les sites Web de façon précédemment indisponibles. Par exemple, Apple permet désormais sites Web afin de donner aux clients la possibilité de payer en toute sécurité via Apple Pay et améliorations apportées à la valorisation de l’infrastructure complète les graphiques d’une application et en calculant potentiels. 

Pour plus d’informations sur macOS Sierra, consultez le site d’Apple [macOS + applications](https://developer.apple.com/macos/) documentation.

<a name="Whats-New-in-macOS-Sierra" />

## <a name="whats-new-in-macos-sierra"></a>Nouveautés dans macOS Sierra

Apple a ajouté plusieurs nouvelles API et sur les services dans macOS Sierra ainsi que de nombreuses améliorations apportées aux fonctionnalités existantes, y compris :

<a name="Apple-File-System" />

### <a name="apple-file-system"></a>Système de fichiers Apple

Avec macOS Sierra, Apple a publié le nouveau système de fichiers Apple comme un système de fichiers moderne pour iOS, Mac OS, tvOS et watchOS. Le système de fichiers Apple a été optimisé pour le stockage Flash et SSD et fournit les fonctionnalités suivantes : chiffrement renforcé, les métadonnées de la copie sur écriture, l’espace de partage, le clonage des fichiers et répertoires, instantanés, dimensionnement du répertoire rapide et atomiques save-safe primitives.

Pour plus d’informations, consultez le site d’Apple [Guide de système de fichiers Apple](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999).

<a name="Apple-Pay-Enhancements" />

### <a name="apple-pay-enhancements"></a>Améliorations de paie Apple

Apple a apporté plusieurs améliorations à Apple Pay dans macOS Sierra permettant aux utilisateurs d’effectuer des paiements sécurisés à partir de sites Web.

Avec macOS Sierra, plusieurs nouvelles API ont été ajoutés qui fonctionnent avec macOS Sierra, iOS et watchOS pour prendre en charge des réseaux de paiement dynamique et un nouvel environnement de test de bac à sable.

macOS Sierra inclut la nouvelle infrastructure ApplePay Javascript qui permet au développeur d’intégrer Apple Pay directement iOS et macOS des sites Web Safari. Pour les sites Web qui prennent en charge d’Apple Pay, l’utilisateur peut autoriser le paiement à l’aide de leur iPhone ou Apple Watch.

Pour plus d’informations, consultez le site d’Apple [ApplePay JS Framework](https://developer.apple.com/reference/applepayjs) référence.

<a name="Building-Modern-macOS-Apps" />

### <a name="building-modern-macos-apps"></a>Génération macOS moderne applications

Applications modernes macOS tels que navigateur web Safari, Pages de traitement de texte et de propagation feuille utilisez ouvrent de nombreuses nouvelles technologies pour présenter une Interface utilisateur unifiée, sensibles au contexte qui effectue avec des éléments d’interface utilisateur classiques tels que les panneaux flottants et plusieurs d’Apple Windows.

[![Un exemple d’une fenêtre à onglets de Mac](images/content08.png)](images/content08.png#lightbox)

Notre [macOS moderne Building applications](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md) guide couvre plusieurs conseils, fonctionnalités et techniques un développeur peut utiliser pour générer une application moderne macOS dans Xamarin.Mac.

<a name="CloudKit-Data-Sharing" />

### <a name="cloudkit-data-sharing"></a>Partage des données CloudKit

Le framework CloudKit a été développé dans macOS Sierra pour autoriser l’utilisateur à partager rapidement et facilement des enregistrements ou les jeux d’enregistrements à partir de leurs bases de données privée iCloud.

CloudKit fournit une interface utilisateur complète pour l’envoi et de l’acceptation d’invitations enregistrements partagées et l’utilisateur dispose d’un contrôle complet en lecture/écriture sur les personnes qui ont accès aux enregistrements.

Pour plus d’informations, consultez le site d’Apple [CloudKit Framework référence](https://developer.apple.com/reference/clockkit) et [CloudKit JS Framework référence](https://developer.apple.com/reference/cloudkitjs).

<a name="Safari-App-Extensions-Support" />

### <a name="safari-app-extensions-support"></a>Prise en charge des Extensions d’application Safari

Extensions d’application Safari permettent à l’application étendre le comportement du navigateur web Safari lors de laquelle il est étroitement intégré macOS Sierra. Étant donné que les Extensions d’application macOS Safari fonctionnent de la même pour iOS Safari Extensions d’application, ils sont faciles à un port d’un système à un autre.

Pour plus d’informations, consultez le site d’Apple [Guide de programmation de l’Extension Safari application](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319).

<a name="Security-and-Privacy-Enhancements" />

### <a name="security-and-privacy-enhancements"></a>Améliorations de la confidentialité et de sécurité

Apple a apporté plusieurs améliorations à la sécurité et confidentialité dans macOS Sierra qui aide l’application à améliorer la sécurité de l’application et de garantir la confidentialité de l’utilisateur final, y compris les éléments suivants :

- La nouvelle `NSAllowsArbitraryLoadsInWebContent` clé peut ajouter à l’application `Info.plist` de fichier et autorise les pages web charger correctement lors de la protection de la sécurité de Transport Apple (ATS) est toujours activée pour le reste de l’application.
- Le Common données sécurité Architecture (CDSA) API a été déconseillée et doit être remplacé par l’API SecKey pour générer des clés asymétriques.
- Pour toutes les connexions SSL/TLS, le chiffrement symétrique RC4 est maintenant désactivé par défaut. En outre, l’API de Transport sécurisé ne prend plus en SSLv3 et il est recommandé que l’application s’arrêter à l’aide d’un chiffrement SHA-1 et 3DES dès que possible.
- Étant donné que le nouveau Presse-papiers dans iOS 10 et macOS Sierra permet à l’utilisateur de copier et coller entre les appareils, l’API a été étendue pour permettre le Presse-papiers afin de pouvoir être limité à un périphérique spécifique et être horodaté à effacer automatiquement à un moment donné. En outre, les tables de montage nommés ne sont plus conservées et doivent être remplacés par les conteneurs de la table de montage partagés.
- Si l’application accède aux données protégées (par exemple, le calendrier de l’utilisateur), il _doit_ déclarer cette intention avec la clé de valeur de chaîne de rôle correct dans son `Info.plist` fichier (`NSCalendarUsageDescription` dans le cas du calendrier).
- Développeur signé applications qui ne sont pas remises via le Mac App Store peuvent désormais tirer parti de CloudKit iCloud trousseau, iCloud lecteur, des notifications push à distance, VPN et MapKit droits.
- macOS Sierra ne prend plus en remettre le code externe ou des données, ainsi que l’application de signataire de code dans son archive zip ou d’une image de disque non signé que le chemin d’accès du runtime n’est pas connu avant l’exécution.

En outre, les applications qui s’exécutent sur macOS Sierra (ou version ultérieure) doit être déclarée statique intention d’accéder à des fonctionnalités spécifiques ou des informations utilisateur en entrant une ou plusieurs clés spécifiques confidentialité dans leurs `Info.plist` fichiers expliquent à l’utilisateur pour lesquelles l’application souhaite obtenir accès.

Étant donné que macOS Sierra partage ces modifications avec iOS 10, consultez notre iOS 10 [sécurité et confidentialité améliorations](~/ios/app-fundamentals/security-privacy.md) guide pour plus d’informations.

<a name="Smart-Card-Driver-Extension-Support" />

### <a name="smart-card-driver-extension-support"></a>Extension de pilotes de carte à puce pris en charge

MacOS Sierra, l’application peut créer `NSExtension` en fonction des pilotes de carte à puce qui permet l’accès en lecture seule au contenu de certains types de cartes à puce. Cette information est ensuite présentée à l’intérieur de la chaîne de clé système (en remplaçant la méthode Common Architecture de sécurité de données déconseillée).

Pour plus d’informations, du Veuillez consultez Apple [CryptoTokenKit Framework référence](https://developer.apple.com/reference/cryptotokenkit).

<a name="Unified-Logging" />

### <a name="unified-logging"></a>Journalisation unifiée

Journalisation unifiée fournit l’application avec une seule API pour tous les niveaux du système de messagerie efficace. Avec la journalisation unifiée l’application possède un contrôle affiné sur plusieurs niveaux de journalisation qui incluent des contrôles de confidentialité et de suivi pour faciliter le débogage de l’activité. 

Journalisation fournit la corrélation de messages automatique lors de l’activité de suivi et de journalisation est utilisée ensemble.

macOS Sierra inclut une nouvelle application Console (dans les Applications/Utilities) est en mesure d’afficher les données du journal à partir de plusieurs sources, notamment les périphériques connectés. Il prend en charge les recherches sous forme de jeton et enregistrés et affiche les connexions entre les messages entre plusieurs processus.

En outre, les messages du journal peuvent être affichés et gérés à l’aide des outils de ligne de commande.

Pour plus d’informations, consultez le site d’Apple [référence de journalisation](https://developer.apple.com/reference/os/1891852-logging).

<a name="Wide-Color" />

### <a name="wide-color"></a>Couleur large

macOS Sierra étend la prise en charge pour les formats de pixel de plage étendue et les espaces d’une gamme de couleurs dans l’ensemble du système, y compris les infrastructures telles que les graphiques de base, l’Image de base, complète et AVFoundation. Prise en charge pour les appareils avec une couleur large affiche est facilité davantage en fournissant ce comportement dans toute la pile de graphique entière.

En outre, `AppKit` a été modifié pour fonctionner dans la nouvelle étendue **sRVB** espace colorimétrique, facilitant le mélange de couleurs dans les gammes de couleur large sans perte de performances significatifs.

Apple offre les meilleures pratiques suivantes lorsque vous travaillez avec des couleurs larges :

- `NSColor` maintenant utilise le sRGB espace de couleurs et ne sera plus limiter les valeurs à la `0.0` à `1.0` plage. Si l’application s’appuie sur le comportement précédent de la limite, il devez modifier pour macOS Sierra.
- Lorsque vous utilisez une API de bas niveau tels que les graphiques de base ou complète pour fournir le traitement d’image, l’application doit utiliser un format de couleur plage étendue espace et de pixels qui prend en charge les valeurs à virgule flottante 16 bits. Le cas échéant, l’application aura à la fixation des valeurs de couleur composant manuellement.
- Graphiques de base, Image Core et complète les nuanceurs de performances fournissent des nouvelles méthodes pour la conversion entre les espaces de deux couleurs.

Pour plus d’informations, consultez notre [Introduction à la couleur large](~/ios/platform/wide-color.md) guide.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Modifications d’infrastructure supplémentaires

Outre les modifications majeures framework et les ajouts répertoriés ci-dessus, Apple a fait de nombreuses modifications supplémentaires framework mineure macOS Sierra.

Pour plus d’informations, consultez notre [des modifications supplémentaires Framework](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md) guide.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>API déconseillées

Les API suivantes ont été déconseillées dans macOS Sierra :

- Le système de fichiers HFS Standard n’est plus pris en charge.

Consultez d’Apple [OS X v10.12 différences d’API](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/APIDiffsMacOS10_12/index.html) documentation pour obtenir la liste complète des deprecations et des modifications.

## <a name="related-links"></a>Liens associés

- [Exemples Mac](https://developer.xamarin.com/samples/mac/)
- [Nouveautés du système d’exploitation X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
