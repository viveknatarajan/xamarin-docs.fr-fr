---
title: Introduction à iOS 10
description: Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans iOS 10 pour les développeurs de Xamarin.iOS.
ms.prod: xamarin
ms.assetid: FB91DFFE-CF5E-4253-92CB-78A6371259D9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: c3bee0f15016394005a67e98cd8435e6d63b3ac6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30786463"
---
# <a name="introduction-to-ios-10"></a>Introduction à iOS 10

_Cet article présente tous les nouveaux et modifiés API et fonctionnalités disponibles dans iOS 10 pour les développeurs de Xamarin.iOS._

## <a name="introducing-ios-10"></a>Présentation d’iOS 10

Avec la nouvelle iOS SDK 10, Apple a inclus nouvelles API et services qui permettent au développeur de créer des catégories d’applications et fonctionnalités. Une application iOS peut à présent étendre les applications Siri, téléphone, Messages et des cartes pour fournir des fonctionnalités de riches et attrayantes à l’utilisateur final qui n’était pas précédemment disponible.

Pour plus d’informations sur iOS 10, consultez le site d’Apple [iOS + applications](https://developer.apple.com/ios/) documentation.

## <a name="whats-new-in-ios-10"></a>Nouveautés dans iOS 10

Apple a ajouté plusieurs nouvelles API et sur les services dans iOS 10 ainsi que de nombreuses améliorations apportées aux fonctionnalités existantes, y compris :


## <a name="adapting-to-the-true-tone-display"></a>Adaptation à l’affichage de la tonalité True

La technologie d’Apple True ton affiche utilise le capteur de lumière ambiant dans un appareil iOS afin d’ajuster dynamiquement la couleur et l’intensité de l’affichage pour faire correspondre les conditions d’éclairage actuel. iOS 10 propose les nouvelles [UIWhitePointAdaptivityStyle](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW31) clé qui peut être ajouté à l’application `Info.plist` de fichier et qui contrôle comment ton True applique les décalages de couleurs standard. 

Les valeurs suivantes sont disponibles :

- `UIWhitePointAdaptivityStyleStandard` **Par défaut** -utilisez l’adaptabilité de point de blanc standard.
- `UIWhitePointAdaptivityStyleReading` -Utilisé pour les applications de focus de la lecture.
- `UIWhitePointAdaptivityStyleGame` -Utilisé pour les applications d’axée sur le jeu.
- `UIWhitePointAdaptivityStyleVideo` -Utilisé pour les applications vidéo axée sur le serveur.
- `UIWhitePointAdaptivityStylePhoto` -Utilisé pour la photographie axée sur les applications où la fidélité des couleurs est plus importante que les réglages de l’environnement de point de blanc.

<a name="app-extensions" />

## <a name="app-extensions"></a>Extensions d’application

Apple a fourni plusieurs nouveaux Points d’Extension d’application dans iOS 10 :

- Répertoire de l’appel
- Modes de l’interface utilisateur et des intentions
- Messages
- Contenu de la notification
- Notification Services
- Pack de vignette

En outre, 3e partie clavier application Extensions ont les améliorations suivantes :

- La nouvelle `DocumentInputMode` propriété de la `UITextDocumentProxy` classe peut déterminer la langue d’entrée d’un document et autoriser l’extension de clavier pour se conformer à cette langue.
- La nouvelle `HandleInputModeList` (méthode) permet d’afficher le menu du sélecteur de clavier du système en réponse à la clé de Globe en cours tapées l’extension de clavier.

Pour plus d’informations, consultez notre [Introduction aux Extensions](~/ios/platform/extensions.md), [intégration de l’application Message](~/ios/platform/message-app-integration/index.md), [présentation Proactive des Suggestions](~/ios/platform/search/proactive-suggestions.md), [ Introduction à SiriKit](~/ios/platform/sirikit/index.md), [Introduction aux Notifications à l’utilisateur](~/ios/platform/user-notifications/index.md) et d’Apple [Guide de programmation d’Extension d’application](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

## <a name="app-search-enhancements"></a>Améliorations de la recherche de l’application

Spotlight Core dans iOS 10 apporte plusieurs améliorations à la recherche de l’application tel que :

- **Popularité de lien ciblé Crowdsourced (avec confidentialité différentielle)** -permet de promouvoir le contenu lié en profondeur l’application dans les résultats de la recherche.
- **Dans l’application recherche** -utiliser la nouvelle `CSSearchQuery` classe afin de fournir la possibilité de recherche Spotlight dans l’application similaire au fonctionnement des applications de messagerie, les Messages et les Notes.
- **Rechercher la Continuation** : permet à un utilisateur de lancer une recherche dans la galerie ou Safari, puis ouvrir une application et continuer la recherche.
- **Visualisation des résultats de la Validation** -Apple [outil de Validation API application recherche](https://search.developer.apple.com/appsearch-validation-tool) affiche maintenant une représentation visuelle du balisage d’un site Web et lien profond lors de la réalisation des tests.
- **Message d’application de partage Image** -permet des images dans l’application populaires fournis pour le partage dans les Messages (via une Extension d’application Message) pour qu’il apparaisse dans les recherches de la galerie.

Pour plus d’informations, consultez notre [application recherche améliorations](~/ios/platform/search/app-search-enhancements.md) guide.

## <a name="apple-pay-enhancements"></a>Améliorations de paie Apple

Apple a apporté plusieurs améliorations à Apple Pay dans iOS 10 permettant à l’utilisateur pour les paiements sécurisés à partir de sites Web et via l’interaction avec Siri et des mappages.

Avec iOS 10, plusieurs nouvelles API ont été ajoutés qui fonctionnent avec iOS et watchOS pour prendre en charge des réseaux de paiement dynamique et un nouvel environnement de test de bac à sable.

En outre, le framework PassKit a été développé pour prendre en charge d’Apple Pay en dehors de `UIKit` et de permettre aux émetteurs de carte présenter leurs cartes à partir de leurs applications.

Pour plus d’informations, consultez notre [améliorations de payer Apple](~/ios/platform/apple-pay.md) guide.

## <a name="alternate-app-icons"></a>Icônes de l’autre application

Apple a ajouté plusieurs améliorations à iOS 10.3 qui permettent à une application de gérer son icône :

 - `ApplicationIconBadgeNumber` -Obtient ou définit le badge de l’icône de l’application dans le Springboard.
 - `SupportsAlternateIcons` -If `true` l’application a un autre jeu d’icônes.
 - `AlternateIconName` -Retourne le nom de l’icône de remplacement actuellement sélectionné ou `null` si vous utilisez l’icône du premier.
 - `SetAlternameIconName` -Utilisez cette méthode pour basculer l’icône de l’application sur l’icône de remplacement donné.

Pour plus d’informations, consultez notre [autres icônes de l’application](~/ios/app-fundamentals/images-icons/alternate-app-icons.md) guide.


## <a name="introduction-to-callkit"></a>Introduction aux CallKit

La nouvelle API CallKit dans iOS 10 offre un moyen pour les applications VOIP pour l’intégrer à l’interface utilisateur de l’iPhone et fournir une interface familière et expérience à l’utilisateur final. Avec cette API, les utilisateurs peuvent afficher et interagir avec les appels VOIP à partir de l’écran de verrouillage du périphérique iOS et gérer des contacts à l’aide de l’application de téléphone **favoris** et **récents** vues.

En outre, l’API CallKit fournit la possibilité de créer des Extensions d’application qui peut associer un numéro de téléphone d’un nom (ID de l’appelant) ou indiquer le système lorsqu’un nombre doit être bloqué (appel de blocage).

Pour plus d’informations, consultez notre [présentation Callkit](~/ios/platform/callkit.md) guide.

## <a name="message-app-integration"></a>Intégration d’application de message

iOS 10 permet l’inclusion d’une Extension d’application de Message dans la solution Xamarin.iOS qui s’intègre à la **Messages** application et affiche le nombre de nouvelles fonctionnalités à l’utilisateur. L’extension peut envoyer des messages interactifs, texte, autocollants et fichiers multimédias. Deux types d’Extension d’application de Message sont disponibles :

- **Les packs d’autocollant** -contient une collection d’autocollants l’utilisateur peut ajouter à un message. Packs d’étiquette peuvent être créés sans écrire de code.
- **iMessage application** -peut présenter une Interface utilisateur personnalisée au sein de l’application de Messages pour la sélection des autocollants, saisie de texte, y compris les fichiers de support (avec des conversions de type facultatif) et création, modification et envoyer des messages de l’interaction.

Pour plus d’informations, consultez notre [intégration de l’application Message](~/ios/platform/message-app-integration/index.md) guide.

## <a name="news-publisher-enhancements"></a>Améliorations de serveur de publication de News

Avec iOS 10, Apple magazines principales et des nouvelles organisations blogueurs et des éditeurs indépendants pour vous inscrire et produit et de fournir du contenu à l’application de News d’Apple. Pour plus d’informations, consultez le site d’Apple [nouvelles ressources](https://newsresources.apple.com/) documentation.

## <a name="providing-haptic-feedback"></a>Envoi de commentaires tactile

Sur l’iPhone 7 et iPhone 7 Plus, Apple a inclus les nouvelles réponses HAPTIQUES qui fournissent des méthodes supplémentaires pour prendre part aux physiquement de l’utilisateur. Utiliser les nouvelles options de rétroaction tactile pour obtenir l’attention des utilisateurs et de renforcer leurs actions.

Plusieurs éléments d’interface utilisateur intégrées déjà commentaires tactiles tels que les sélecteurs, les commutateurs et les curseurs. iOS 10 ajoute à présent la possibilité de déclencher par programme HAPTIQUES à l’aide d’une sous-classe concrète de la `UIFeedbackGenerator` classe.

Pour plus d’informations, consultez notre [en fournissant des commentaires tactiles](~/ios/user-interface/ios-ui/haptic-feedback.md) guide.

## <a name="proactive-suggestions"></a>Suggestions proactive

iOS 10 présente les nouvelles façons d’implication conduite à une application en autorisant le système de façon proactive présente des informations utiles automatiquement à l’utilisateur au moment opportun. Comme d’iOS 9 fourni la possibilité d’ajouter une recherche approfondie à l’application à l’aide de la galerie, remise et des Suggestions de Siri, avec iOS 10, qu'une application peut exposer des fonctionnalités qui peuvent être présentées à l’utilisateur par le système à partir des emplacements suivants :

- Le sélecteur d’application
- L’écran de verrouillage
- CarPlay
- Maps
- Interactions de Siri
- Suggestions de QuickType 

Une application expose cette fonctionnalité dans le système à l’aide d’une collection de technologies telles que [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), balisage web, Core Spotlight, MapKit, lecteur multimédia et UIKit.

Pour plus d’informations, consultez notre [présentation Proactive des Suggestions](~/ios/platform/search/proactive-suggestions.md) guide.

## <a name="request-app-review"></a>Demande de révision de l’application

Vous débutez avec iOS 10.3, le `RequestReview()` méthode permet à une application iOS demander à l’utilisateur à évaluer ou réviser. Alors que cette méthode peut être appelée à tout moment où il est préférable de l’expérience utilisateur, le processus de révision est régi et géré par la stratégie de l’App Store. Par conséquent, cette méthode peut ou peut ne pas affiche une alerte et ne doit jamais être appelée en réponse à une action de l’utilisateur, par exemple en appuyant sur un bouton.

Pour plus d’informations, consultez notre [demander une révision application](~/ios/platform/request-app-review.md) guide.

## <a name="security-and-privacy-enhancements"></a>Améliorations de la confidentialité et de sécurité

Apple a apporté plusieurs améliorations à la sécurité et confidentialité dans iOS 10 qui aideront le développeur d’améliorer la sécurité de leurs applications et de garantir la confidentialité de l’utilisateur final.

Par conséquent, les applications qui s’exécutent sur iOS 10 (ou version ultérieure) doivent déclarer statiquement intention d’accéder à des fonctionnalités spécifiques ou des informations utilisateur en entrant une ou plusieurs clés spécifiques confidentialité dans leurs `Info.plist` fichiers expliquent à l’utilisateur pour lesquelles l’application souhaite accéder.

Pour plus d’informations, consultez notre [sécurité et confidentialité améliorations](~/ios/app-fundamentals/security-privacy.md) guide.

## <a name="sirikit"></a>SiriKit

Nouveau pour iOS 10, SiriKit permet à une application Xamarin.iOS fournir des services qui sont accessibles à l’utilisateur à l’aide de Siri sur un appareil iOS. Cette fonctionnalité est fournie dans un ou plusieurs Extension d’application à l’aide de la nouvelle **intentions** et **intentions UI** infrastructures.

SiriKit prend en charge les domaines de service suivants :

- Audio ou vidéo.
- Un cas de réservation.
- La gestion de sommeil.
- Gestion des messages.
- Recherche des photos.
- Envoyer ou recevoir des paiements.

Lorsque l’utilisateur effectue une demande de Siri impliquant un des services de l’Extension application, SiriKit envoie l’extension un **intention** objet qui décrit la demande de l’utilisateur, ainsi que toutes les données de prise en charge. l’Extension d’application, puis génère les **réponse** de l’objet pour la donnée **intention**, détaillant la façon dont l’extension peut gérer la demande.

Tandis que Siri traite généralement toutes les interactions de l’utilisateur, l’Extension d’application peut utiliser le **l’interface utilisateur de l’intention de** framework pour présenter une riche, personnalisé Interface utilisateur présentant l’homologation de l’application et des informations supplémentaires.

Pour plus d’informations, consultez notre [présentation SiriKit](~/ios/platform/sirikit/index.md) guide.

## <a name="speech-recognition"></a>Reconnaissance vocale

iOS 10 inclut une nouvelle API de reconnaissance vocale qui permet à l’application de la reconnaissance vocale continue de prendre en charge et la transcription de la reconnaissance vocale (direct ou enregistrés flux audio) en texte.

Étant donné que la reconnaissance vocale nécessite la transmission et le stockage temporaire des données sur les serveurs Apple, l’application _doit_ demander l’autorisation de l’utilisateur à effectuer une reconnaissance en incluant le `NSSpeechRecognitionUsageDescription` clé dans son `Info.plist` fichier et en appelant le `SFSpeechRecognizer.RequestAutorization` (méthode).

Pour plus d’informations, consultez notre [présentation de la reconnaissance vocale](~/ios/platform/speech.md) guide.

## <a name="user-notifications"></a>Notifications à l’utilisateur

Nouveau pour iOS 10, la Notification utilisateur framework permet la remise et le traitement des notifications locale et distantes. À l’aide de cette infrastructure, l’application ou une Extension d’application permettre planifier la remise de notifications locales en spécifiant un ensemble de conditions comme emplacement ou l’heure du jour.

En outre, l’application ou une extension peut recevoir (et éventuellement modifier) les notifications à la fois locale et distantes qu’ils sont remis à l’appareil iOS de l’utilisateur.

La nouvelle infrastructure d’interface utilisateur de Notification utilisateur permet à l’application ou une Extension d’application pour personnaliser l’apparence des notifications à la fois locale et distantes lorsqu’ils sont présentés à l’utilisateur.

Pour plus d’informations, consultez notre [utilisateur Notifications Framework](~/ios/platform/user-notifications/index.md) guide.

## <a name="video-subscriber-account"></a>Compte de l’abonné vidéo

Nouveau pour iOS 10, la structure du compte d’abonné vidéo permet aux applications cette prise en charge authentifié de diffusion en continu ou la vidéo à la demande pour s’authentifier avec leur fournisseur TV câble ou satellite à l’aide d’une seul-expérience de connexion pour l’utilisateur final.

## <a name="wide-color"></a>Couleur large

iOS 10 étend la prise en charge pour les formats de pixel de plage étendue et les espaces d’une gamme de couleurs dans l’ensemble du système, y compris les infrastructures telles que les graphiques de base, l’Image de base, complète et AVFoundation. Prise en charge pour les appareils avec une couleur large affiche est facilité davantage en fournissant ce comportement dans toute la pile de graphique entière.

En outre, [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) a été modifié pour fonctionner dans la nouvelle étendue **sRVB** espace colorimétrique, facilitant le mélange de couleurs dans les gammes de couleur large sans perte de performances significatifs.

Apple offre les meilleures pratiques suivantes lorsque vous travaillez avec des couleurs larges :

- [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/) maintenant utilise le sRGB espace de couleurs et ne sera plus limiter les valeurs à la `0.0` à `1.0` plage. Si l’application s’appuie sur le comportement précédent de la limite, elle devrez modifiables pour iOS 10.
- L’environnement de dessin sera configuré pour l’espace de couleurs sRGB lors de l’exécution personnalisée `UIView` de dessin sur un iPad Pro.
- Si l’application effectue le rendu personnalisé de `UIImages`, utilisez la nouvelle [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) classe pour spécifier l’utilisation des formats de plage étendue ou une plage de standard.
- Lorsque vous utilisez une API de bas niveau tels que les graphiques de base ou complète pour fournir le traitement d’image, le développeur doit utiliser un format de couleur plage étendue espace et de pixels qui prend en charge les valeurs à virgule flottante 16 bits. Le cas échéant, le développeur aura à la fixation des valeurs de couleur composant manuellement.
- Graphiques de base, Image Core et complète les nuanceurs de performances fournissent des nouvelles méthodes pour la conversion entre les espaces de deux couleurs.

Pour plus d’informations, consultez notre [Introduction à la couleur large](~/ios/platform/wide-color.md) guide.

## <a name="widget-enhancements"></a>Améliorations du widget

Apple a introduit plusieurs améliorations au système de Widget pour vous assurer que les widgets apparaîtront sur n’importe quel arrière-plan qui existe sur le nouveau iOS 10 écran de verrouillage. Le [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) propriété a été déconseillée et a été remplacée par la nouvelle [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) ou [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect) propriétés. En outre, les widgets contiennent désormais un [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) propriété qui permet au développeur décrire la quantité de contenu est disponible et permet à l’utilisateur développer et réduire le contenu.

Pour plus d’informations, consultez notre [recherche et améliorations du Widget d’écran accueil](~/ios/platform/search/widgets.md) guide.

## <a name="additional-framework-changes"></a>Modifications d’infrastructure supplémentaires

Outre les modifications majeures framework et les ajouts répertoriés ci-dessus, Apple a modifié nombreux framework mineure supplémentaires dans iOS 10.

Pour plus d’informations, consultez notre [des modifications supplémentaires Framework](~/ios/platform/introduction-to-ios10/additional-framework-changes.md) guide.

## <a name="deprecated-apis"></a>API déconseillées

Les API suivantes ont été déconseillées dans iOS 10 :

- Le `CKDiscoverAllContactsOperation`, `CKDiscoveredUserInfo`, `CKDiscoverUserInfosOperation` et `CKFetchRecordChangesOperation` classes ont été déconseillées dans CloudKit pour iOS 10. Utilisez le [CKDiscoverAllUserIdentitiesOperation](https://developer.xamarin.com/api/type/CloudKit.CKDiscoverUserIdentitiesOperation/), [CKUserIdentity](https://developer.xamarin.com/api/type/CloudKit.CKUserIdentity/) et [CKFetchRecordZoneChangesOperation](https://developer.xamarin.com/api/type/CloudKit.CKFetchRecordZoneChangesOperation/) classes (qui prennent en charge le partage des enregistrements) à la place.
- Plusieurs [CKSubscription](https://developer.apple.com/reference/cloudkit/cksubscription) API (tels que les abonnements basés sur une requête et zone) ont été déconseillées. Utilisez le [CKRecordZoneSubscription](https://developer.xamarin.com/api/type/CloudKit.CKRecordZoneSubscription/) et [CKQuerySubscription](https://developer.xamarin.com/api/type/CloudKit.CKQuerySubscription/) API à la place.
- [NSPersistentStoreCoordnator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) symboles associés au contenu omniprésent ont été déconseillées.
- `ADBannerView`, `ADInterstitialAd` et les symboles dans le [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) classe ont été déconseillées.
- [SKUniform](https://developer.apple.com/reference/spritekit/skuniform) symboles relatifs aux valeurs à virgule flottante ont été déconseillées.
- Le `UILocalNotification`, `UIMutableUserNotificationAction`, `UIMutableUserNotificationCategory`, `UIUserNotificationAction`, `UIUserNotificationCategory` et `UIUserNotificationSettings` classes de UIKit ont été déconseillées. Utilisez le [Notifications utilisateur](#User-Notifications) framework à la place.
- Le `HandleActionForLocalNotification`, `HandleActionForRemoteNotification`, `DidReceiveLocalNotification` et `DidReceiveRemoteNotification` WatchKit méthodes ont été déconseillées. Utilisez le `HandleActionForNotification` et `DidReceiveNotification` méthodes à la place.
- Le `DidReceiveLocalNotification` et `DidReceiveRemoteNotification` méthodes de la [WKExtensionDelegate](https://developer.apple.com/reference/watchkit/wkextensiondelegate) ont été déconseillées. Créez une instance de [UNUserNotificationCenterDelegate](https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate) qui implémente les méthodes appropriées et assignez-la à la `Delegate` propriété de la [UNUserNotificationCenter](https://developer.apple.com/reference/usernotifications/unusernotificationcenter) objet.
- Le **Game Center App** a été déconseillé et supprimé à partir d’iOS. Si l’application utilise GameKit, il _doit_ présenter son propre interface pour afficher les fonctionnalités GameKit telles qu’ajouterons, etc.

Consultez d’Apple [iOS 9.3 aux différences d’API iOS 10.0](https://developer.apple.com/library/prerelease/content/releasenotes/General/iOS10APIDiffs/index.html) documentation pour obtenir la liste complète des deprecations.



## <a name="related-links"></a>Liens associés

- [Exemples d’iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Quelles sont les nouveautés dans iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
