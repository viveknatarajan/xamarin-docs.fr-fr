---
title: PassKit dans Xamarin.iOS
description: Portefeuille est une application iOS de système qui stocke et affiche les codes-barres et autres informations pour lier des transactions des clients sur leur téléphone avec « réels ».
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 0a4fd39e312cf96ac59eae97b1212f001c4ef799
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788346"
---
# <a name="passkit-in-xamarinios"></a>PassKit dans Xamarin.iOS

_Portefeuille est une application iOS de système qui stocke et affiche les codes-barres et autres informations pour lier des transactions des clients sur leur téléphone avec « réels »._

Portefeuille est une application pour iPhone et iPod touche avec iOS 6. Il stocke et affiche les codes-barres et autres informations pour lier des transactions des clients sur leur téléphone avec « réels ». Passe générés par les vendeurs et envoyée au client par courrier électronique, URL ou à partir d’une application iOS de partenaire. Portefeuille stocke et organise tous les tests sur un téléphone et affiche des rappels de réussite sur l’écran de verrouillage en fonction de la date/heure ou de l’emplacement de l’appareil.

Ce document présente le portefeuille, à l’aide de l’API du Kit de transmettre avec Xamarin.iOS et explique comment implémenter des Passes sur votre serveur.

 [![](passkit-images/image1.png "Le portefeuille stocke et d’organise tous les tests sur un téléphone")](passkit-images/image1.png#lightbox)


## <a name="requirements"></a>Configuration requise

Les fonctions de magasin Kit abordées dans ce document requièrent iOS 6 et Xcode 4.5, ainsi que de Xamarin.iOS 6.0.


## <a name="introduction"></a>Introduction

Le problème de clés qui résout de passer le Kit est la distribution et la gestion de codes-barres. Certains exemples concrets d’utilisation de codes-barres sont actuellement sont les suivantes :

-   **Achat en ligne les tickets de film** – clients sont généralement envoyées par courrier électronique un code-barres représentant leurs tickets. Ce code-barres imprimé et nécessaire pour le cinéma à analyser pour l’entrée.
-   **Cartes de fidélité** – clients comportent un nombre de cartes spécifiques aux magasins différentes dans leur portefeuille sac, pour l’affichage et analyse quand ils achètent des produits.
-   **Coupons** – Coupons sont distribuées par courrier électronique, en tant que pages web imprimables, via partout et de codes-barres dans les journaux et magazines. Clients de les mettre dans un magasin pour l’analyse pour la réception des marchandises, des services ou des remises en retour.
-   **Embarquement** – similaire à l’achat d’un ticket de film.


Kit de test offre une alternative pour chacun de ces scénarios :

-   **Les tickets de film** – après l’achat, le client ajoute un test de Ticket d’événement (par courrier électronique ou un lien de site Web). En tant que l’heure pour l’une des approches film, le test s’afficheront automatiquement sur l’écran de verrouillage en guise de rappel, et à l’arrivée sur le cinéma le test est facilement récupéré et affiché de portefeuille pour l’analyse.
-   **Cartes de fidélité** – plutôt que (ou en plus) en fournissant une carte physique, magasins peuvent émettre (par courrier électronique ou après une connexion de site Web) un passage de la carte de magasin. Le magasin peut fournir des fonctionnalités supplémentaires telles que la mise à jour le solde du compte sur le test via des notifications push, et à l’aide des services de géolocalisation la passe pourrait s’affichent automatiquement sur l’écran de verrouillage lorsque le client se trouve un emplacement de magasin.
-   **Coupons** – Coupon Passes peuvent facilement être générées avec des caractéristiques uniques pour faciliter le suivi et distribués via les liens de messagerie ou le site Web. Coupons téléchargés peuvent apparaître automatiquement sur l’écran de verrouillage lorsque l’utilisateur est près d’un emplacement spécifique, ou à une date donnée (par exemple, lorsque la date d’expiration est proche). Étant donné que les bons sont stockés sur le téléphone de l’utilisateur, elles sont toujours pratiques et ne pas égarés. Coupons peuvent encourager les clients à télécharger des applications d’accompagnement, car les liens de l’App Store peuvent être incorporées dans le test, l’augmentation de l’engagement avec le client.
-   **Embarquement** – après un processus de vérification en ligne, le client doit recevoir leurs passe embarquement par courrier électronique ou un lien. Une application auxiliaire fournie par le fournisseur de transport Impossible d’inclure le processus d’archivage et permettent également au client d’exécuter des fonctions supplémentaires comme choisir leur siège ou un repas. Le fournisseur de transport peut utiliser des notifications push pour mettre à jour le test si le transport est retardé ou annulé. En tant que la durée d’embarquement approches le test seront affiche sur l’écran de verrouillage en guise de rappel et pour fournir un accès rapide à l’étape.


Fondamentalement, passez le Kit fournit un moyen simple et pratique pour stocker et afficher les codes-barres sur vos appareils tactiles iPhone ou iPod. Avec le plus de temps et l’intégration d’écran de verrouillage emplacement, des notifications push et Application Compagnon intègrent offre un foundation pour les ventes très sophistiquées, tickets et de facturation des services.


## <a name="pass-kit-ecosystem"></a>Passez l’écosystème de Kit

Kit de test n’est pas simplement une API dans CocoaTouch, au lieu de cela il fait partie d’un plus grand écosystème d’applications, données et services qui facilitent le partage sécurisé et gestion des codes-barres et autres données. Ce diagramme de haut niveau montre les différentes entités qui peuvent être impliquées dans la création et à l’aide de Passes :

 [![](passkit-images/image2.png "Ce diagramme de haut niveau indique les entités impliquées dans la création et à l’aide de Passes")](passkit-images/image2.png#lightbox)

Chaque partie de l’écosystème a un rôle clairement défini :

-   **Portefeuille** – application iOS intégrées d’Apple (pour les fonctions tactiles iPhone et iPod) qui stocke et affiche les Passes. Il s’agit du seul emplacement Passes sont rendus pour une utilisation dans le monde réel (ie le code-barres s’affiche, ainsi que toutes les données localisées dans le test).
-   **Le Guide des applications** – les applications iOS 6 générées par les fournisseurs de test pour étendre les fonctionnalités des passes émettre, telles que l’ajout de valeur à une carte de magasin, la modification le siège d’un passage d’embarquement ou une autre fonction d’entreprise spécifiques. Applications d’accompagnement ne sont pas requises pour un test être utile.
-   **Votre serveur** – un serveur sécurisé où les passes peuvent être générées et signés pour la distribution. Votre application mobile peut se connecter à votre serveur pour générer des passes de nouveau ou demander des mises à jour de passe existant. Vous pouvez éventuellement implémenter l’API du service web qui appelle à mettre à jour passe portefeuille.
-   **Les serveurs APNS** : votre serveur a la possibilité de notifier le portefeuille des mises à jour à un test sur un appareil donné à l’aide de APNS. Une notification d’émission à portefeuille qui contacte ensuite votre serveur pour les détails de la modification. Applications d’accompagnement n’avez pas besoin d’implémenter des APNS pour cette fonctionnalité (ils peuvent écouter le `PKPassLibraryDidChangeNotification` ).
-   **Conduit applications** – les Applications qui ne manipulent directement passe (par exemple, les applications associées, de le faire), mais ce qui peut améliorer leur utilité en reconnaissant les Passes et qui permet de les ajouter à un portefeuille. Les clients de messagerie, les navigateurs de réseaux sociaux et les autres applications d’agrégation de données peuvent tous rencontrer les pièces jointes ou des liens vers les passes.


L’écosystème complet semble complexe, il est important de noter que certains composants sont facultatifs et les implémentations de passer un Kit beaucoup plus simples possible.

## <a name="what-is-a-pass"></a>Qu’est un test ?

Un test est une collection de données représentant un ticket, la promotion ou la carte. Il peut être prévu pour une utilisation unique par une personne (et par conséquent contenir des détails tels qu’une allocation de nombre et le siège de vol), ou il peut un jeton d’utiliser plusieurs qui peut être partagé par n’importe quel nombre d’utilisateurs (par exemple, un coupon de remise). Une description détaillée est disponible dans Apple [sur des fichiers passer](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html) document.


### <a name="types"></a>Types

Actuellement cinq types pris en charge, qui peuvent être distingués dans l’application de portefeuille par la mise en page et le bord supérieur de la passe :

-  **Événement Ticket** – découpage semi-circulaire petit.
-   **Test d’intégration** – bandes coupées de côté, spécifique au transport icône peuvent être spécifiées (par exemple). bus, train, avion).
-   **Stocker la carte** – arrondi supérieur, comme une carte de crédit.
-  **Coupon** – perforée en haut.
-  **Générique** : identique à la carte de magasin, haut arrondi.


Les types de cinq passe sont affichés dans cette capture d’écran (dans l’ordre : coupon, générique, stocker la carte, passez d’embarquement et ticket d’événement) :

 [![](passkit-images/image3.png "Les types de cinq passe sont affichés dans cette capture d’écran")](passkit-images/image3.png#lightbox)

### <a name="file-structure"></a>Structure de fichiers

Un fichier de test est en fait une archive ZIP avec un **.pkpass** extension, contenant certains JSON des fichiers spécifiques (obligatoires), une variété d’image des fichiers (facultatif), ainsi que des chaînes localisées (également facultative).

-   **pass.JSON** : obligatoire. Contient toutes les informations pour le test.
-   **manifest.JSON** : obligatoire. Contient des hachages de SHA1 pour chaque fichier lors du passage à l’exception du fichier de signature et de ce fichier (manifest.json).
-   **signature** : obligatoire. Créé en signant le `manifest.json` fichier avec le certificat généré dans le portail d’approvisionnement iOS.
-  **logo.png** – facultatif.
-  **Background.png** – facultatif.
-  **Icon.png** – facultatif.
-  **Fichiers de chaînes localisables** – facultatif.


Structure de répertoires d’un fichier de test est indiqué ci-dessous (c’est le contenu de l’archive ZIP) :

 [![](passkit-images/image4.png "Structure de répertoires d’un fichier de test est illustrée ici")](passkit-images/image4.png#lightbox)

### <a name="passjson"></a>pass.JSON

JSON est le format, car les passes sont généralement créés sur un serveur, cela signifie que le code de génération est indépendant de la plateforme sur le serveur. Les trois composantes clés des informations dans chaque étape sont :

-   **teamIdentifier** – cette opération lie tous les tests que vous générez pour votre compte de l’App Store. Cette valeur est visible dans le portail d’approvisionnement iOS.
-   **passTypeIdentifier** – Registre dans le portail d’approvisionnement au groupe passe ensemble (si vous produisez plusieurs types). Par exemple, un café-restaurant peut créer un Type de passer de la carte de magasin pour permettre à leurs clients bénéficier de crédits de fidélité, mais également un Type de passer de Coupon distinct pour créer et distribuer des bons de réduction. Ce même café-restaurant peut même contenir des événements de la musique live et émettre Passes de Ticket d’événement pour les.
-   **serialNumber** – une chaîne unique dans cette `passTypeidentifier` . La valeur est opaque à portefeuille, mais il est importante pour le suivi des tests spécifiques lors de la communication avec votre serveur.


Il existe un grand nombre d’autres clés JSON dans chaque étape, un exemple de ce qui est indiqué ci-dessous :

``` 
{
   "passTypeIdentifier":"com.xamarin.passkitdoc.banana",  //Type Identifier (iOS Provisioning Portal)
   "formatVersion":1,                                     //Always 1 (for now)
   "organizationName":"Xamarin",                          //The name which appears on push notifications
   "serialNumber":"12345436XYZ",                          //A number for you to identify this pass
   "teamIdentifier":"XXXAAA1234",                         //Your Team ID
   "description":"Xamarin Demo",                          //
   "foregroundColor":"rgb(54,80,255)",                    //color of the data text (note the syntax)
   "backgroundColor":"rgb(209,255,247)",                  //color of the background
   "labelColor":"rgb(255,15,15)",                         //color of label text and icons
   "logoText":"Banana ",                                  //Text that appears next to logo on top
   "barcode":{                                            //Specification of the barcode (optional)
      "format":"PKBarcodeFormatQR",                       //Format can be QR, Text, Aztec, PDF417
      "message":"FREE-BANANA",                            //What to encode in barcode
      "messageEncoding":"iso-8859-1"                      //Encoding of the message
   },
   "relevantDate":"2012-09-15T15:15Z",                    //When to show pass on screen. ISO8601 formatted.
  /* The following fields are specific to which type of pass. The name of this object specifies the type, e.g., boardingPass below implies this is a boarding pass. Other options include storeCard, generic, coupon, and eventTicket */
   "boardingPass":{
/*headerFields, primaryFields, secondaryFields, and auxiliaryFields are arrays of field object. Each field has a key, label, and value*/
      "headerFields":[          //Header fields appear next to logoText
         {
            "key":"h1-label",   //Must be unique. Used by iOS apps to get the data.
            "label":"H1-label", //Label of the field
            "value":"H1"        //The actual data in the field
         },
         {
            "key":"h2-label",
            "label":"H2-label",
            "value":"H2"
         }
      ],
      "primaryFields":[       //Appearance differs based on pass type
         {
            "key":"p1-label",
            "label":"P1-label",
            "value":"P1"
         }
      ],
      "secondaryFields":[     //Typically appear below primaryFields
         {
            "key":"s1-label",
            "label":"S1-label",
            "value":"S1"
         }
      ],
      "auxiliaryFields":[    //Appear below secondary fields
         {
            "key":"a1-label",
            "label":"A1-label",
            "value":"A1"
         }
      ],
      "transitType":"PKTransitTypeAir"  //Only present in boradingPass type. Value can
                                        //Air, Bus, Boat, or Train. Impacts the picture
                                        //that shows in the middle of the pass.
   }
}
```

### <a name="barcodes"></a>Codes-barres

Les formats 2D uniquement sont pris en charge : PDF417, Aztec, QR. Apple que codes-barres 1D sont inadaptées à l’analyse sur un écran de téléphone lumineux de revendications.

Texte de remplacement affiché ci-dessous le code-barres est facultatif, certains commerces souhaitez être en mesure de lire le type, ou manuellement.

L’encodage ISO-8859-1 est la vérification courante, le codage utilisé par les systèmes d’analyse qui seront lue Passes.

### <a name="relevancy-lock-screen"></a>Pertinence (écran de verrouillage)

Il existe deux types de données qui peuvent provoquer un passage à afficher sur l’écran de verrouillage :

 **Emplacement**

Jusqu'à 10 sites peut être spécifié dans un test, par exemple stocke un client visite fréquemment ou à l’emplacement d’un cinéma ou un aéroport. Un client peut définir ces emplacements via une application auxiliaire ou le fournisseur peut déterminer les données d’utilisation de (si collectées avec l’autorisation du client).

Lorsque le test s’affiche sur l’écran de verrouillage, une limite est calculée afin que lorsque l’utilisateur quitte la zone de la passe est masquée à partir de l’écran de verrouillage. Le rayon est lié pour passer de style pour prévenir les abus.

 **Date et heure**

Qu’une seule date/heure peuvent être spécifié dans un test. La date et l’heure est utile pour déclencher des rappels auprès de l’écran de verrouillage d’embarquement et les tickets de l’événement.

Peut être mis à jour par envoi de données ou via les API PassKit, afin que la date/heure peut être mis à jour dans le cas d’un ticket de plusieurs fois (par exemple, un ticket de la saison à un théâtre ou un complexe sportif).

### <a name="localization"></a>Localisation

Conversion d’un test dans plusieurs langues est similaire à la localisation d’une application iOS : créer des répertoires spécifiques avec langage le `.lproj` extension et placer les éléments localisés à l’intérieur. Traduction de texte doit être entrées dans un `pass.strings` de fichiers, alors que les images localisées doivent avoir le même nom que l’image, elles remplacent à la racine de test.

## <a name="security"></a>Sécurité

Passes sont signés avec un certificat privé que vous générez dans le portail d’approvisionnement iOS. Les étapes pour signer le test sont :

1.  Calculer le hachage SHA1 pour chaque fichier dans le répertoire de test (n’incluez pas le `manifest.json` ou `signature` fichier, qui ne doit exister tout de même à ce stade).
1.  Écrire `manifest.json` comme une liste de clé/valeur JSON de chaque nom de fichier avec son hachage.
1.  Utiliser le certificat pour signer le `manifest.json` de fichiers et d’écrire le résultat dans un fichier appelé `signature` .
1.  COMPRESSER le tout et donnez au fichier qui en résulte un `.pkpass` extension de fichier.


Étant donné que votre clé privée est nécessaire pour signer le test, ce processus doit être effectué sur un serveur sécurisé que vous contrôlez. NE pas distribuer vos clés pour essayer de générer des passes dans une application.

 
## <a name="configuration-and-setup"></a>Installation et configuration

Cette section contient des instructions pour aider à configurer vos détails de l’approvisionnement et de créer votre premier passage.

### <a name="provisioning-passkit"></a>Approvisionnement PassKit

Par ordre d’un passage à entrer l’App Store, il doit être lié à un compte de développeur. Cette option implique deux étapes :

1.  Le test doit être inscrit à l’aide d’un identificateur unique, appelé l’ID de passer un Type.
1.  Un certificat valide doit être généré pour le test de signature numérique du développeur.

Pour créer un faire passer un ID de Type suit.


<a name="create-passid"/>

#### <a name="create-a-pass-type-id"></a>Créer un ID de Type Pass

La première étape consiste à configurer un ID de Type passer pour chaque autre _type_ de passe d’être pris en charge. Le transmettre l’ID (ou l’identificateur de passer un Type) crée un identificateur unique pour le test. Nous allons utiliser cet ID pour lier la passe de votre compte de développeur à l’aide d’un certificat.

1. Dans le [section certificats, les identificateurs et les profils du portail d’approvisionnement iOS](https://developer.apple.com/account/overview.action), accédez à **identificateurs** et sélectionnez **passer un ID de Type** . Puis sélectionnez le **+** bouton permettant de créer un nouveau type de test : [ ![ ] (passkit-images/passid.png "créer un nouveau type de test")](passkit-images/passid.png#lightbox)

2.   Fournir un **Description** (nom) et **identificateur** (chaîne unique) pour le test. Notez que tous les ID de Type passer doit commencer par la chaîne `pass.` dans cet exemple, nous utilisons `pass.com.xamarin.coupon.banana` : [ ![ ] (passkit-images/register.png "fournir une Description et un identificateur")](passkit-images/register.png#lightbox)


3.   Confirmer l’ID de passer en appuyant sur la **inscrire** bouton.


<a name="generate" />

#### <a name="generate-a-certificate"></a>Générer un certificat

Pour créer un nouveau certificat pour ce code de Type passer, procédez comme suit :

1.  Sélectionnez l’ID de passer nouvellement créé dans la liste, puis cliquez sur **modifier** : [ ![ ] (passkit-images/pass-done.png "sélectionnez le nouvel ID de passer dans la liste")](passkit-images/pass-done.png#lightbox)

    Ensuite, sélectionnez **créer un certificat...** :

    [![](passkit-images/cert-dist.png "Sélectionnez Créer le certificat")](passkit-images/cert-dist.png#lightbox)


2.  Suivez les étapes pour créer un certificat de demande (signature).
  
3. Appuyez sur la **continuer** sur le portail des développeurs et téléchargez le conseiller pour générer votre certificat.

4. Téléchargez le certificat et double-cliquez dessus pour l’installer dans votre chaîne de clé.


Maintenant que nous avons créé un certificat pour ce code de Type passer, la section suivante décrit comment créer un test manuellement.

Pour plus d’informations sur la configuration du portefeuille, reportez-vous à la [utilisation des fonctionnalités](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) guide.

 <a name="Create_a_Pass_Manually" />

### <a name="create-a-pass-manually"></a>Créer un test manuellement

Maintenant que nous avons créé le Type passer, nous pouvons créer manuellement un test pour tester sur le simulateur ou un périphérique. Les étapes pour créer un test sont :

-  Créez un répertoire contenant les fichiers de test.
-  Créez un fichier pass.json qui contient toutes les données requises.
-  Inclure des images dans le dossier (si nécessaire).
-  Calculer le hachage SHA1 pour chaque fichier dans le dossier et écriture manifest.json.
-  Manifest.json de connexion avec le fichier .p12 certificat téléchargé.
-  COMPRESSER le contenu du répertoire et renommez avec l’extension de .pkpass.


Il existe certains fichiers sources dans l’exemple de code pour cet article peut être utilisé pour générer un test. Utiliser les fichiers dans le `CouponBanana.raw` répertoire du répertoire CreateAPassManually. Les fichiers suivants sont présents :

 [![](passkit-images/image18.png "Ces fichiers sont présentes")](passkit-images/image18.png#lightbox)

Ouvrez pass.json et modifier le fichier JSON. Vous devez au moins mettre à jour le `passTypeIdentifier` et `teamIdentifer` pour correspondre à votre compte de développeur d’Apple.

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

Vous devez ensuite calculer les hachages de chaque fichier et créer le `manifest.json` fichier. Il ressemble à ceci lorsque vous avez terminé :

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

Ensuite, une signature doit être générée pour ce fichier à l’aide du certificat (.p12 fichier) qui a été généré pour ce code de Type de test.

 <a name="Signing_On_a_Mac" />


#### <a name="signing-on-a-mac"></a>La signature sur un Mac

Téléchargez le **matériaux de prise en charge de valeur initiale de portefeuille** à partir de la [Apple télécharge](https://developer.apple.com/downloads/index.action?name=Passbook) site. Utilisez le `signpass` outil à transformer votre dossier d’un passage (Ceci permet également de calculer le SHA1 hache et code postal de la sortie dans un fichier .pkpass).

 <a name="Signing_On_a_PC" />


#### <a name="signing-on-a-pc"></a>La signature sur un PC

Dans l’exemple de code de cet article, il est un projet appelé `signpassnet` .NET qui s’exécute sur Windows. Tente de se reproduire outil d’Apple, mais il inclut le code de validation beaucoup moins.

 <a name="Testing" />


#### <a name="testing"></a>Test

Si vous deviez examiner la sortie de ces outils (en définissant le nom de fichier par .zip, puis en l’ouvrant), vous visualiserez les fichiers suivants (Notez l’ajout de la `manifest.json` et `signature` fichiers) :

 [![](passkit-images/image19.png "Examen de la sortie de ces outils")](passkit-images/image19.png#lightbox)

Une fois que vous avez signé, compressés et renommer le fichier (par exemple). pour `BananaCoupon.pkpass`) vous pouvez faire glisser dans le simulateur pour tester ou envoyer à vous-même à récupérer sur un périphérique réel. Vous devez voir un écran à **ajouter** le passage, comme suit :

 [![](passkit-images/image20.png "Ajouter l’écran de test")](passkit-images/image20.png#lightbox)

Ce processus serait normalement être automatisé sur un serveur, la création de pass toutefois manuelle peut être une option pour les petites entreprises qui créez uniquement des coupons qui ne nécessitent pas la prise en charge d’un serveur principal.

 <a name="Wallet" />

## <a name="wallet"></a>Wallet

Portefeuille est la partie centrale de l’écosystème de passer un Kit. Cette capture d’écran montre le portefeuille vide et l’aspect que la liste de test et les passe individuels :

 [![](passkit-images/image21.png "Cette capture d’écran montre le portefeuille vide et l’aspect que la liste de test et les passe individuels")](passkit-images/image21.png#lightbox)

Fonctionnalités de portefeuille :

-  Il est le seul endroit Passes sont rendus avec leurs code-barres pour l’analyse.
-  Utilisateur peut modifier les paramètres des mises à jour. S’il est activé, des notifications push peuvent déclencher des mises à jour les données dans le test.
-  Utilisateur peut activer ou désactiver l’intégration de l’écran de verrouillage. S’il est activé, cette permet la réussite d’afficheront automatiquement sur son écran de verrouillage, basée sur les données pertinentes du temps et l’emplacement incorporées dans le test de.
-  Le verso de la passe prend en charge tirer pour actualiser, si une URL de serveur web est fournie dans le JSON passer.
-  Le Guide applications peut être ouvert (ou téléchargé) si l’ID de l’application est fourni dans le JSON passer.
-  Passe peut être supprimée (avec une animation de destruction jolie).


 <a name="Getting_Passes_into_Wallet" />

## <a name="adding-passes-into-wallet"></a>Ajout de Passes en portefeuille

Passe peut être ajoutées à un portefeuille de plusieurs manières :

* **Conduit applications** : elles ne manipulent pas passe directement, elles simplement de chargement des fichiers de test et de présentent à l’utilisateur la possibilité de les ajouter à un portefeuille. 

* **Le Guide des applications** : ceux-ci sont écrites par les fournisseurs pour distribuer des Passes et proposent des fonctionnalités supplémentaires pour parcourir ou les modifier. Les applications Xamarin.iOS ont un accès complet à l’API Kit passer pour créer et manipuler des passes. Passe peut ensuite être ajouté à l’aide de portefeuille le `PKAddPassesViewController`. Ce processus est décrit plus en détail dans les **Applications d’accompagnement** section de ce document.

### <a name="conduit-applications"></a>Applications de canal

Applications de canal sont des applications intermédiaires qui recevront les transmet au nom d’un utilisateur et doivent être programmées pour identifier leur type de contenu et de fournir des fonctionnalités à ajouter dans le portefeuille. Voici quelques exemples d’applications de canal :

-   **Messagerie** – reconnaît une passe de la pièce jointe.
-   **Safari** – reconnaît l’en-tête Content-Type passer un clic sur un lien de passer une URL.
-   **Autres applications personnalisées** – n’importe quelle application qui reçoivent des pièces jointes ou ouvrir des liens (clients de médias sociaux, lecteurs de messagerie, etc.).


Cette capture d’écran montre comment **Mail** dans iOS 6 reconnaît une pièce jointe de test et (lors d’un contact) propose de **ajouter** à portefeuille.

 [![](passkit-images/image22.png "Cette capture d’écran montre comment la messagerie dans iOS 6 reconnaît une pièce jointe de test")](passkit-images/image22.png#lightbox)

 [![](passkit-images/image23.png "Cette capture d’écran montre comment le courrier fournit ajouter une pièce jointe de test de portefeuille")](passkit-images/image23.png#lightbox)

Si vous générez une application qui peut être un tuyau pour passe, ils peuvent être identifiés par :

-  **Extension de fichier** -.pkpass
-  **Type MIME** -application/vnd.apple.pkpass
-  **Utilitaire** – com.apple.pkpass


L’opération de base d’une application de canal consiste à récupérer le fichier de test et appeler passer Kit `PKAddPassesViewController` pour permettre à l’utilisateur la possibilité d’ajouter le test à son portefeuille. L’implémentation de ce contrôleur d’affichage est disponible dans la section suivante sur **accompagnement Applications**.

Applications de canal n’avez pas besoin soit configuré pour un ID de Type passer spécifique de la même façon que les Applications Compagnon.

## <a name="companion-applications"></a>Applications d’accompagnement

Une application Compagnon fournit des fonctionnalités supplémentaires pour l’utilisation des passes, y compris la création d’un passage, mise à jour des informations associées à un test ainsi sinon passe associé à l’application.

Applications d’accompagnement ne doivent pas tenter de dupliquer les fonctionnalités de portefeuille. Ils ne sont pas destinées à afficher des Passes pour l’analyse.

Cette suite de cette section décrit comment générer une application auxiliaire base qui interagit avec le Kit de passer.

### <a name="provisioning"></a>Provisionnement

Portefeuille étant une technologie de stockage, l’application doit être configuré séparément et ne peut pas utiliser de profil de configuration de l’équipe ou Wildcard application ID. Reportez-vous à la [utilisation des fonctionnalités](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) guide pour créer un ID d’application et la configuration de profil unique pour l’application de portefeuille.

### <a name="entitlements"></a>Droits d’accès

Le **Entitlements.plist** fichier doit être inclus dans des dernières Xamarin.iOS projet. Pour ajouter un nouveau fichier Entitlements.plist, suivez les étapes de la [utilisation des droits](~/ios/deploy-test/provisioning/entitlements.md) guide.

Pour définir vos droits procédez comme suit :

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Double-cliquez sur le **Entitlements.plist** fichier dans la zone de la Solution pour ouvrir l’éditeur de Entitlements.plist :

![](passkit-images/image31.png "Éditeur de Entitlements.plst")

Sous la section portefeuille, sélectionnez le **activer un portefeuille** option

![](passkit-images/image32.png "Activer les droits de portefeuille")


L’option par défaut est de votre application permettre à que tous les passent des types. Toutefois, il est possible limiter votre application et autoriser uniquement un sous-ensemble de types de test d’équipe. Pour activer ce, sélectionnez le **sous-ensemble autoriser d’équipe passez des types** et entrez l’identificateur de type pass du sous-ensemble que vous souhaitez autoriser.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Double-cliquez sur le **Entitlements.plist** fichier à ouvrir le fichier source XML.

Pour ajouter le droit de portefeuille, définissez la **propriété** à `Passbook Identifiers` dans la liste déroulante, qui définira automatiquement le **Type** `Array`. Ensuite, définissez la chaîne **valeur** à `$(TeamIdentifierPrefix)*`:

![](passkit-images/image33.png "Activer les droits de portefeuille")

Avec cette valeur de chaîne, votre application accepte tous les types de passes. Pour limiter votre application et autoriser uniquement un sous-ensemble de types de test d’équipe, définissez la valeur de chaîne sur :

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

Où `pass.$(CFBundleIdentifier)` est l’ID de passer qui a été créé [ci-dessus](~/ios/platform/passkit.md)

-----

### <a name="debugging"></a>Débogage

Si vous avez des problèmes de déploiement de votre application, vérifiez que vous utilisez la bonne **profil de préparation** et que le `Entitlements.plist` est sélectionné comme le **les droits personnalisés** fichier dans le **iPhone signature d’offre groupée** options.

Si vous rencontrez cette erreur lors du déploiement :

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

le `pass-type-identifiers` tableau des droits d’accès est incorrect (ou ne correspond pas à la **profil de préparation**). Vérifiez les ID de Type passer et votre ID d’équipe sont corrects.

 <a name="Classes" />

## <a name="classes"></a>Classes

Les classes de passer un Kit suivantes sont disponibles pour les applications accéder aux étapes :

-  **PKPass** – une instance d’une étape.
-  **PKPassLibrary** – fournit l’API pour accéder à la passe sur l’appareil.
-  **PKAddPassesViewController** – permet d’afficher une passe de l’utilisateur à enregistrer dans le portefeuille.
-  **PKAddPassesViewControllerDelegate** – permet aux développeurs de Xamarin.iOS


## <a name="example"></a>Exemple

Référence au projet PassLibrary dans l’exemple de code pour cet article. Il illustre les fonctions courantes suivantes qui peuvent être nécessaires dans une Application auxiliaire de portefeuille :

### <a name="check-that-wallet-is-available"></a>Vérifiez que le portefeuille est disponible

Portefeuille n’est pas disponible sur un iPad, les applications doivent vérifier avant de tenter d’accéder aux fonctionnalités du Kit de passer.

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>Création d’une Instance de la bibliothèque Pass

La bibliothèque de passer le Kit n’est pas un singleton, les applications doivent créer et stocker et pour accéder à l’API du Kit de passer de l’instance.

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>Obtenir la liste des étapes

Applications peuvent demander une liste de tests à partir de la bibliothèque. Cette liste est filtrée automatiquement passer Kit, afin que vous ne pouvez voir que les tests qui ont été créés par votre ID d’équipe et qui sont répertoriées dans vos droits.

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

Notez que le simulateur ne filtre pas la liste des Passes retourné, afin de cette méthode doit toujours être testée sur des appareils réels. Cette liste peut être affichée dans un UITableView, l’application exemple doit ressembler à cela deux coupons ont été ajoutés :

 [![](passkit-images/image29.png "L’apparence d’application exemple comme suit après ont ajouté les deux coupons")](passkit-images/image29.png#lightbox)


### <a name="displaying-passes"></a>Affichage des Passes

Un ensemble limité d’informations est disponible pour le rendu des passes dans les applications d’accompagnement.

Choisissez à partir de ce jeu de propriétés standards pour afficher des listes de passes, contrairement à l’exemple de code.

```csharp
string passInfo =
                "Desc:" + pass.LocalizedDescription
                + "\nOrg:" + pass.OrganizationName
                + "\nID:" + pass.PassTypeIdentifier
                + "\nDate:" + pass.RelevantDate
                + "\nWSUrl:" + pass.WebServiceUrl
                + "\n#" + pass.SerialNumber
                + "\nPassUrl:" + pass.PassUrl;
```

Cette chaîne est affichée comme une alerte dans l’exemple :

 [![](passkit-images/image30.png "L’alerte du Coupon est sélectionné dans l’exemple")](passkit-images/image30.png#lightbox)

Vous pouvez également utiliser le `LocalizedValueForFieldKey()` méthode pour récupérer des données à partir des champs dans les passes que vous avez conçu (étant donné que vous connaîtrez les champs doit être présent). L’exemple de code ne montre pas cela.

### <a name="loading-a-pass-from-a-file"></a>Chargement d’un passage d’un fichier

Car un passage peut uniquement être ajouté à portefeuille avec l’autorisation de l’utilisateur, un contrôleur de vue doit être présenté pour leur permettre de décider. Ce code est utilisé dans le **ajouter** bouton dans l’exemple, pour charger un test avant génération qui est incorporé dans l’application (vous devez le remplacer par un que vous avez signé) :

```csharp
NSData nsdata;
using ( FileStream oStream = File.Open (newFilePath, FileMode.Open ) ) {
        nsdata = NSData.FromStream ( oStream );
}
var err = new NSError(new NSString("42"), -42);
var newPass = new PKPass(nsdata,out err);
var pkapvc = new PKAddPassesViewController(newPass);
NavigationController.PresentModalViewController (pkapvc, true);
```

Le test est présenté avec **ajouter** et **Annuler** options :

 [![](passkit-images/image20.png "La passe présentée avec les options Ajouter et annuler")](passkit-images/image20.png#lightbox)

### <a name="replace-an-existing-pass"></a>Remplacer un test existant

Remplacement d’un test existant ne nécessite pas l’autorisation de l’utilisateur, mais il échoue si le test n’existe pas déjà.

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>Modification d’un test

PKPass n’est pas mutable, donc vous ne pouvez pas mettre à jour les objets de passe dans votre code. Pour modifier les données dans un test d’une application doit avoir accès à un serveur web que vous peuvent conserver un enregistrement de Passes et générer un nouveau fichier de test avec les valeurs mises à jour que l’application peut télécharger.

Création de fichiers de test doit être effectuée sur un serveur, car les Passes doivent être signées avec un certificat qui doit rester privées et sécurisées.

Une fois qu’un fichier de test mis à jour a été généré, utilisez le `Replace` méthode permettant de remplacer les anciennes données sur l’appareil.

### <a name="display-a-pass-for-scanning"></a>Afficher un test pour l’analyse

Comme indiqué précédemment, seul portefeuille peut afficher un test pour l’analyse. Un test peut être affiché à l’aide de la `OpenUrl` méthode comme indiqué :

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>Recevoir des Notifications de modifications

Applications peuvent écouter les modifications apportées à la bibliothèque de passer à l’aide de la `PKPassLibraryDidChangeNotification`. Modifications peut être dû à des notifications de déclencher des mises à jour en arrière-plan, il est conseillé pour écouter dans votre application.

```csharp
noteCenter = NSNotificationCenter.DefaultCenter.AddObserver (PKPassLibrary.DidChangeNotification, (not) => {
    BeginInvokeOnMainThread (() => {
        new UIAlertView("Pass Library Changed", "Notification Received", null, "OK", null).Show();
        // refresh the list
        var passlist = library.GetPasses ();
        table.Source = new TableSource (passlist, library);
        table.ReloadData ();
    });
}, library);  // IMPORTANT: must pass the library in
```

Il est important de passer une instance de la bibliothèque lors de l’inscription pour la notification car PKPassLibrary n’est pas un singleton.

## <a name="server-processing"></a>Traitement du serveur

Une présentation détaillée de la création d’une application de serveur pour prendre en charge de transmettre un Kit est dépasse le cadre de cet article de présentation.

Le code .NET fourni dans le *signpassnet* exemple peut être utilisé comme base pour une méthode côté serveur qui peut générer des Passes.

Vue [WWDC vidéo : présentation de livret, partie 2](https://developer.apple.com/videos/wwdc/2012/?include=309#309) minutes 27 : 00 pour plus d’informations.

### <a name="other-resources"></a>Autres ressources

Consultez [-dotnet livret](https://github.com/tomasmcguinness/dotnet-passbook) ouvrir le code source c# de côté serveur.

## <a name="push-notifications"></a>Notifications Push

Une présentation détaillée de l’utilisation des notifications push pour mettre à jour des Passes est dépasse le cadre de cet article de présentation.

Vous êtes nécessaire pour implémenter l’API REST de type défini par Apple pour répondre aux requêtes web à partir de portefeuille lorsque des mises à jour sont requises. Le code .NET fourni dans le *signpassnet* exemple peut servir comme base pour générer des Passes de nouveau à la suite de ces demandes.

Vue [WWDC vidéo : présentation de livret, partie 2](https://developer.apple.com/videos/wwdc/2012/?include=309#309) minutes 27 : 00 pour plus d’informations.

## <a name="summary"></a>Récapitulatif

Cet article a introduit le Kit de passer, décrit quelques-unes des raisons pour lesquelles il est utile et décrit les différentes parties qui doivent être implémentées pour une solution complète de passer un Kit. Il décrit les étapes requises pour configurer un compte Apple Developer pour créer des Passes, le processus pour effectuer un passage manuellement et également comment accéder aux API du Kit de passer d’une application Xamarin.iOS.

## <a name="related-links"></a>Liens associés

- [CreateAPassManually (exemple)](https://developer.xamarin.com/samples/PassKit/)
- [Exemple de PassKit](https://developer.xamarin.com/samples/monotouch/PassKit/)
- [Introduction à iOS 6](~/ios/platform/introduction-to-ios6/index.md)
- [Guide de programmation livret](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Conceptual/PassKit_PG/Chapters/Introduction.html)
- [Livret pour les développeurs](https://developer.apple.com/passbook/)
- [À propos des fichiers de test](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [Passer la référence de Framework Kit](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Passer la référence de Framework Kit](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Livret une référence de Service Web](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
