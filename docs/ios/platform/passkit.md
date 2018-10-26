---
title: PassKit dans Xamarin.iOS
description: L’application Wallet permet aux utilisateurs de stocker des passes numériques sur leurs appareils à iOS. Le framework PassKit permet aux développeurs d’interagir par programmation avec les passes.
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2018
ms.openlocfilehash: d1c640bef41e875b3bb427d657c9c239e4c3e16d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121397"
---
# <a name="passkit-in-xamarinios"></a>PassKit dans Xamarin.iOS

L’application de portefeuille iOS permet aux utilisateurs de stocker des passes numériques sur leurs appareils.
Ces passes générés par les commerçants et envoyées au client par courrier électronique, URL, ou via l’application iOS de partenaire. Ces passes peuvent représenter plusieurs choses, à partir de tickets de film aux cartes de fidélité à embarquement. Le framework PassKit permet aux développeurs d’interagir par programmation avec les passes.

Ce document présente Wallet et à l’aide de l’API de PassKit avec Xamarin.iOS.

 [![](passkit-images/image1.png "Le portefeuille stocke et d’organise toutes les étapes sur un téléphone")](passkit-images/image1.png#lightbox)

## <a name="requirements"></a>Configuration requise

Les fonctionnalités de PassKit décrites dans ce document nécessitent iOS 6 et Xcode 4.5, ainsi que de Xamarin.iOS 6.0.

## <a name="introduction"></a>Introduction

Le problème de clés qui résout de PassKit est la distribution et la gestion de codes-barres. Voici quelques exemples réels d’utilisation de codes-barres sont actuellement :

-   **Achète des tickets de film en ligne** : les clients sont généralement envoyé par courrier électronique un code-barres représentant leurs tickets. Ce code-barres est imprimé et dirigé vers le cinéma pour être analysés pour l’entrée.
-   **Cartes de fidélité** – clients comportent un nombre de différentes cartes spécifiques aux magasins dans leur portefeuille ou les cordons, pour l’affichage et analyse quand ils achètent des marchandises.
-   **Coupons** – Coupons sont distribués par courrier électronique, en tant que pages web imprimable, via partout et comme les codes-barres dans les journaux et magazines. Les clients les mène à un magasin pour l’analyse, pour recevoir en retour de marchandises, des services ou des remises.
-   **Embarquement** – similaire à l’achat d’un ticket de film.

PassKit offre une alternative pour chacun de ces scénarios :

-   **Tickets de film** – après l’achat, le client ajoute une passe de ticket d’événement (via e-mail ou un lien de site Web). En tant que l’heure pour les approches de film, le test s’affichent automatiquement sur l’écran de verrouillage en guise de rappel, et à l’arrivée au cinéma le pass est facilement récupéré et affiché dans le portefeuille pour l’analyse.
-   **Cartes de fidélité** – au lieu de (ou en plus) en fournissant une carte physique, magasins peuvent émettre (par e-mail ou une fois la connexion de site Web) une passe de la carte Store. Le magasin peut fournir des fonctionnalités supplémentaires telles que la mise à jour le solde du compte lors de la passe par le biais de notifications push, et à l’aide des services de géolocalisation la passe peut automatiquement s’affichent sur l’écran de verrouillage lorsque le client est près d’un emplacement de magasin.
-   **Coupons** – passes de Coupon peuvent facilement être générés avec des caractéristiques uniques pour faciliter le suivi et distribués via les liens de messagerie ou un site Web. Coupons téléchargés peuvent automatiquement s’affichent sur l’écran de verrouillage lorsque l’utilisateur est près d’un emplacement spécifique, et/ou à une date donnée (par exemple, lorsque la date d’expiration approche). Étant donné que les coupons sont stockés sur le téléphone de l’utilisateur, elles sont toujours pratique et ne pas égarés. Coupons peuvent encourager les clients à télécharger des applications Compagnon, car les liens de l’App Store peuvent être incorporés dans le passage, augmenter l’engagement avec le client.
-   **Embarquement** – après un processus de vérification en ligne, le client recevrait leur carte d’embarquement par courrier électronique ou un lien. Une application Compagnon fournies par le fournisseur de transport peut inclure le processus d’archivage et permettent également au client d’exécuter des fonctions supplémentaires comme choisir leur siège ou un repas. Le fournisseur de transport peut utiliser des notifications push pour mettre à jour de la passe si le transport est retardé ou annulé. Comme les approches de temps embarquement la passe s’affiche sur l’écran de verrouillage en guise de rappel et pour fournir un accès rapide pour le test.

Fondamentalement, PassKit fournit un moyen simple et pratique pour stocker et afficher les codes-barres sur votre appareil iOS. Avec le plus de temps et l’intégration d’écran de verrouillage emplacement, notifications push et Application Compagnon intègrent offre un foundation pour les ventes très sophistiqués, la création de tickets et la facturation des services.

## <a name="passkit-ecosystem"></a>Écosystème de PassKit

PassKit n’est pas simplement une API au sein de CocoaTouch, au lieu de cela il fait partie d’un plus grand écosystème d’applications, données et services qui facilitent le partage sécurisé et gestion de codes-barres et autres données. Ce diagramme montre les différentes entités qui peuvent être impliquées dans la création et à l’aide de passes :

 [![](passkit-images/image2.png "Ce diagramme de haut niveau montre les entités impliquées dans la création et à l’aide de passes")](passkit-images/image2.png#lightbox)

Chaque partie de l’écosystème a un rôle clairement définies :

-   **Wallet** – application iOS intégrée de d’Apple qui stocke et affiche les passes. Il s’agit du seul emplacement passes sont rendus pour une utilisation dans le monde réel (ie le code-barres s’affiche, ainsi que toutes les données localisées lors du passage).
-   **Le Guide des applications** – les applications iOS 6 créées par passer des fournisseurs pour étendre les fonctionnalités des passes émettre, telles que l’ajout de valeur à une carte de magasin, modification du siège sur une carte d’embarquement ou autre fonction d’entreprise spécifiques. Applications Compagnon ne sont pas requises pour un laissez-passer pour être utile.
-   **Votre serveur** – un serveur sécurisé où passe peut être généré et signé pour la distribution. Votre application Compagnon peuvent se connecter à votre serveur pour générer des passes de nouveau ou demander des mises à jour de passe existant. Vous pouvez éventuellement implémenter le web passe de l’API de service Wallet appellerait pour mettre à jour.
-   **Les serveurs APNS** – votre serveur a la possibilité de notifier le portefeuille des mises à jour à un passage sur un appareil donné à l’aide de APNS. Une notification push à Wallet qui prendra alors contact avec votre serveur pour les détails de la modification. Applications Compagnon n’avez pas besoin d’implémenter APNS pour cette fonctionnalité (ils peuvent écouter le `PKPassLibraryDidChangeNotification` ).
-   **Conduit applications** : les Applications qui ne manipulent directement les passe (par exemple, les applications Compagnon font), mais qui permet d’améliorer leur utilité en reconnaissant les passes et leur permettant à ajouter au Wallet. Clients de messagerie, les navigateurs de réseau social et les autres applications d’agrégation de données peuvent tous rencontrer les pièces jointes ou des liens vers des passes.

Tout l’écosystème paraisse complexe, il est important de noter que certains composants sont facultatifs et les implémentations de PassKit beaucoup plus simples sont possibles.

## <a name="what-is-a-pass"></a>Qu’est un Pass ?

Un test est une collection de données représentant un ticket, coupon ou une carte. Elle est prévue pour une seule utilisation par une personne (et par conséquent contenir des détails tels que d’une allocation de nombre et le siège de vol), ou il peut un jeton d’utiliser plusieurs qui peut être partagé par n’importe quel nombre d’utilisateurs (par exemple, un coupon de remise). Une description détaillée est disponible dans d’Apple [propos des fichiers de transmettre](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html) document.

### <a name="types"></a>Types

Actuellement cinq types pris en charge, qui peuvent être distingués dans l’application Wallet par la mise en page et le bord supérieur de la passe :

-  **Événement Ticket** – découpage semi-circulaire petit.
-   **Intégration Pass** – crans dans l’icône de côté, spécifique au transport peuvent être spécifiés (par exemple). bus, train, avion).
-   **Store carte** – arrondi supérieur, comme une carte de crédit ou de débit.
-  **Coupon** – perforées en haut.
-  **Générique** – même en tant que carte de Store, haut arrondi.


Les types de cinq passes sont affichés dans cette capture d’écran (dans l’ordre : coupon, générique, stocker la carte, carte d’embarquement et de ticket d’événement) :

 [![](passkit-images/image3.png "Les types de cinq passes sont affichés dans cette capture d’écran")](passkit-images/image3.png#lightbox)

### <a name="file-structure"></a>Structure de fichiers

Un fichier pass est en fait une archive ZIP contenant un **.pkpass** , contenant certaines spécifiques fichiers JSON (obligatoires), une variété d’image fichiers (facultatif), ainsi que des chaînes localisées (également facultatif).

-   **pass.JSON** – requis. Contient toutes les informations pour le test.
-   **manifest.JSON** – requis. Contient des hachages de SHA1 pour chaque fichier lors du passage à l’exception du fichier de signature et de ce fichier (manifest.json).
-   **signature** – requis. Créée en signant le `manifest.json` fichier avec le certificat généré dans le portail de provisionnement iOS.
-  **logo.png** – facultatif.
-  **Background.png** – facultatif.
-  **Icon.png** – facultatif.
-  **Fichiers de chaînes localisables** – facultatif.

Structure de répertoires d’un fichier pass est indiquée ci-dessous (c’est le contenu de l’archive ZIP) :

 [![](passkit-images/image4.png "Structure de répertoires d’un fichier pass est illustré ici")](passkit-images/image4.png#lightbox)

### <a name="passjson"></a>pass.JSON

JSON est le format, car les passes sont généralement créés sur un serveur : cela signifie que le code de génération est indépendant de la plateforme sur le serveur. Les trois éléments clés des informations dans chaque pass sont :

-   **teamIdentifier** – cette opération lie toutes les étapes que vous générez pour votre compte de l’App Store. Cette valeur est visible dans le portail de provisionnement iOS.
-   **passTypeIdentifier** – Inscrivez-vous dans le portail de provisionnement au groupe passe ensemble (si vous produire plusieurs types). Par exemple, un café peut créer un type de passe de carte de magasin pour permettre aux gagner des crédits de fidélité de leurs clients, mais également un bon d’achat séparé passer type pour créer et distribuer des bons de réduction. Ce même café peut même contenir des événements de musique en direct et émettre les passes de Ticket d’événement pour ceux.
-   **serialNumber** – une chaîne unique dans cette `passTypeidentifier` . La valeur est opaque pour Wallet, mais il est importante pour le suivi des passages spécifiques lors de la communication avec votre serveur.

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

Formats uniquement 2D sont pris en charge : PDF417, Aztec, QR. Apple des revendications que codes-barres 1D sont inadaptées à l’analyse sur un écran de téléphone rétroéclairé.

Texte de remplacement affiché ci-dessous le code-barres est facultatif, certains commerçants souhaitez être en mesure de lire/type manuellement.

Encodage ISO-8859-1 est la vérification courante, le codage utilisé par les systèmes d’analyse qui liront les passes.

### <a name="relevancy-lock-screen"></a>Pertinence (écran de verrouillage)

Il existe deux types de données qui peuvent provoquer un passage à afficher sur l’écran de verrouillage :

 **Emplacement**

Jusqu'à 10 sites peut être spécifié en un passage, par exemple, les magasins un client visite fréquemment, ou l’emplacement d’un aéroport ou de cinéma. Un client peut définir ces emplacements via une application Compagnon ou le fournisseur peut déterminer les données d’utilisation (si collectées avec l’autorisation du client).

Lorsque le test s’affiche sur l’écran de verrouillage, une limite est calculée afin que lorsque l’utilisateur laisse la zone de la passe est masquée à partir de l’écran de verrouillage. Le rayon est lié pour passer de style pour éviter les abus.

 **Date et heure**

Qu’une seule date/heure peut être spécifié en un passage. La date et l’heure est utile pour déclencher des rappels d’écran de verrouillage d’embarquement et de tickets de l’événement.

Peut être mis à jour par envoi de données ou via l’API PassKit, afin que la date/heure peut être mis à jour dans le cas d’un ticket de plusieurs fois (par exemple, un ticket de saison sur un théâtre ou un complexe sportif).

### <a name="localization"></a>Localisation

Traduire un passage en plusieurs langues est similaire à la localisation d’une application iOS : créer des répertoires spécifiques avec langue le `.lproj` extension et placer les éléments localisés à l’intérieur. Traductions de texte doivent être entrées dans un `pass.strings` de fichiers, tandis que des images localisées doivent présenter le même nom que l’image, elles remplacent à la racine du Pass.

## <a name="security"></a>Sécurité

Passes sont signés avec un certificat privé que vous générez dans le portail de provisionnement iOS. Les étapes de la passe de connexion sont :

1.  Calculer un hachage SHA1 pour chaque fichier dans le répertoire pass (n’incluez pas le `manifest.json` ou `signature` fichier, qui ne doit exister quand même à ce stade).
1.  Écrire `manifest.json` comme une liste de clé/valeur JSON de chaque nom de fichier avec son hachage.
1.  Utiliser le certificat pour signer le `manifest.json` de fichiers et écrire le résultat dans un fichier appelé `signature` .
1.  ZIPPER l’et attribuez au fichier qui en résulte un `.pkpass` extension de fichier.


Étant donné que votre clé privée est nécessaire pour signer le pass, ce processus doit uniquement être effectué sur un serveur sécurisé que vous contrôlez. NE distribuez pas vos clés afin d’essayer de générer des passes dans une application.

 
## <a name="configuration-and-setup"></a>Installation et configuration

Cette section contient des instructions pour aider à configurer vos détails de l’approvisionnement et de créer votre première tentative.

### <a name="provisioning-passkit"></a>Approvisionnement de PassKit

Dans l’ordre pour un passage à entrer l’App Store, il doit être lié à un compte de développeur. Cette option implique deux étapes :

1.  Le test doit être inscrite à l’aide d’un identificateur unique, appelé l’ID de passer un Type.
1.  Un certificat valide doit être généré pour signer la passe de la signature numérique du développeur.

Pour créer un, procédez comme ID de Type passe ce qui suit.

#### <a name="create-a-pass-type-id"></a>Créer un ID de Type Pass

La première étape consiste à configurer un ID de Type de passe pour chaque autre _type_ de passe d’être pris en charge. Le transmettre un ID (ou l’identificateur de Type passer) crée un identificateur unique pour le test. Nous allons utiliser cet ID pour lier la passe de votre compte de développeur à l’aide d’un certificat.

1. Dans le [section certificats, identificateurs et profils du portail de provisionnement iOS](https://developer.apple.com/account/overview.action), accédez à **identificateurs** et sélectionnez **passer un ID de Type** . Puis sélectionnez le **+** bouton pour créer un nouveau type de pass : [ ![](passkit-images/passid.png "créer un nouveau type de pass")](passkit-images/passid.png#lightbox)

2.   Fournir un **Description** (nom) et **identificateur** (chaîne unique) pour le test. Notez que tous les ID de Type passer doit commencer par la chaîne `pass.` dans cet exemple, nous utilisons `pass.com.xamarin.coupon.banana` : [ ![](passkit-images/register.png "entrer une Description et un identificateur")](passkit-images/register.png#lightbox)


3.   Confirmer l’ID de passe en appuyant sur la **inscrire** bouton.

#### <a name="generate-a-certificate"></a>Générer un certificat

Pour créer un nouveau certificat pour cet ID de Type de passe, procédez comme suit :

1.  Sélectionnez l’ID de passe nouvellement créé dans la liste, puis cliquez sur **modifier** : [ ![](passkit-images/pass-done.png "sélectionnez le nouvel ID de passer dans la liste")](passkit-images/pass-done.png#lightbox)

    Ensuite, sélectionnez **Create Certificate...** :

    [![](passkit-images/cert-dist.png "Sélectionnez Créer le certificat")](passkit-images/cert-dist.png#lightbox)


2.  Suivez les étapes pour créer une demande (signature de certificat).
  
3. Appuyez sur la **continuer** bouton sur le portail des développeurs et chargez-la pour générer votre certificat.

4. Téléchargez le certificat, puis double-cliquez dessus pour l’installer dans votre trousseau.


Maintenant que nous avons créé un certificat pour cet ID de Type passer, la section suivante décrit comment créer un test manuellement.

Pour plus d’informations sur le provisionnement pour Wallet, reportez-vous à la [utilisation des fonctionnalités](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) guide.

### <a name="create-a-pass-manually"></a>Créer un test manuellement

Maintenant que nous avons créé le Type passer, nous pouvons créer manuellement un laissez-passer pour tester sur le simulateur ou un appareil. Les étapes pour créer un test sont :

-  Créez un répertoire contenant les fichiers pass.
-  Créez un fichier pass.json qui contient toutes les données requises.
-  Inclure des images dans le dossier (si nécessaire).
-  Calculer les codes de hachage SHA1 pour chaque fichier dans le dossier et d’écriture à manifest.json.
-  Vous connecter à manifest.json avec le fichier .p12 de certificat téléchargé.
-  COMPRESSER le contenu du répertoire et renommer avec l’extension de .pkpass.


Il existe certains fichiers sources dans le [exemple de code](https://developer.xamarin.com/samples/monotouch/PassKit/) pour cet article peut être utilisé pour générer un test. Utilisez les fichiers dans le `CouponBanana.raw` répertoire du répertoire CreateAPassManually. Les fichiers suivants sont présents :

 [![](passkit-images/image18.png "Ces fichiers sont présents")](passkit-images/image18.png#lightbox)

Ouvrez pass.json et modifier le script JSON. Vous devez au moins mettre à jour le `passTypeIdentifier` et `teamIdentifer` pour correspondre à votre compte de développeur Apple.

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

Vous devez ensuite calculer les hachages pour chaque fichier et créer le `manifest.json` fichier. Il ressemble à ceci lorsque vous avez terminé :

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

Ensuite, une signature doit être générée pour ce fichier en utilisant le certificat (fichier .p12) qui a été généré pour cet ID de Type Pass.

#### <a name="signing-on-a-mac"></a>La signature sur un Mac

Télécharger le **matériaux de prise en charge de valeur initiale de Wallet** à partir de la [Apple télécharge](https://developer.apple.com/downloads/index.action?name=Passbook) site. Utilisez le `signpass` outil pour transformer votre dossier en un passage (cela calcule également le SHA1 hache et code postal de la sortie dans un fichier .pkpass).

#### <a name="testing"></a>Test

Si vous deviez examiner la sortie de ces outils (en définissant le nom de fichier par .zip, puis en l’ouvrant), vous voyez les fichiers suivants (Notez l’ajout de la `manifest.json` et `signature` fichiers) :

 [![](passkit-images/image19.png "Examen de la sortie de ces outils")](passkit-images/image19.png#lightbox)

Une fois que vous avez signé, compressés et renommé le fichier (par exemple). à `BananaCoupon.pkpass`) vous pouvez faire glisser dans le simulateur pour tester ou par courrier électronique afin de récupérer sur un appareil réel. Vous devez voir un écran à **ajouter** la passe, comme suit :

 [![](passkit-images/image20.png "Ajouter l’écran de réussite")](passkit-images/image20.png#lightbox)

Ce processus serait normalement être automatisé sur un serveur, la création de pass toutefois manuelle peut être une option pour les petites entreprises qui créent uniquement des coupons qui ne nécessitent pas la prise en charge d’un serveur principal.

## <a name="wallet"></a>Wallet

Wallet est l’élément central de l’écosystème de PassKit. Cette capture d’écran montre le Wallet vide et l’aspect de la liste de pass et passe individuels :

 [![](passkit-images/image21.png "Cette capture d’écran montre le Wallet vide et l’aspect de la liste de pass et passe individuels")](passkit-images/image21.png#lightbox)

Fonctionnalités du portefeuille sont :

-  Il est le seul endroit passes sont rendus avec leurs code-barres pour l’analyse.
-  Utilisateur peut modifier les paramètres des mises à jour. Si activé, notifications push peuvent déclencher des mises à jour les données lors du passage.
-  Utilisateur peut activer ou désactiver l’intégration de l’écran de verrouillage. Si activé, cela permet la réussite d’apparaissent automatiquement sur leur écran de verrouillage, selon les données de temps et l’emplacement pertinentes incorporées lors du passage.
-  Le verso de la passe prend en charge les extraire pour actualiser, si une URL de serveur web est fournie lors du passage JSON.
-  Le Guide des applications pouvant être ouvert (ou téléchargé) si l’ID de l’application est fourni lors du passage JSON.
-  Passes peuvent être supprimés (avec une animation de destruction adorable).

## <a name="adding-passes-into-wallet"></a>Ajout de Passes en Wallet

Passes peuvent être ajoutées au service Wallet, comme suit :

* **Conduit applications** – ceux-ci ne manipulent pas de passes directement, elles simplement de chargement des fichiers de passage et de présentent à l’utilisateur avec la possibilité d’ajouter au Wallet. 

* **Le Guide des applications** : ceux-ci sont écrits par les fournisseurs pour distribuer les passes et proposent des fonctionnalités supplémentaires pour parcourir ou les modifier. Les applications Xamarin.iOS ont un accès complet à l’API de PassKit pour créer et manipuler des passes. Passes peuvent ensuite être ajoutées à l’utilisation de Wallet le `PKAddPassesViewController`. Ce processus est décrit plus en détail dans le **les Applications Compagnon** section de ce document.

### <a name="conduit-applications"></a>Applications conduit

Les applications conduit sont des applications intermédiaires qui peuvent recevoir des passes pour le compte d’un utilisateur et doivent être programmées pour reconnaître leur type de contenu et fournir des fonctionnalités à ajouter au Wallet. Voici quelques exemples d’applications de canal d’échange :

-   **Messagerie** – reconnaît une passe de la pièce jointe.
-   **Safari** – reconnaît la passe de Content-Type, un clic sur un lien d’URL pass.
-   **Autres applications personnalisées** – n’importe quelle application qui reçoivent des pièces jointes ou ouvrir des liens (clients de médias sociaux, les lecteurs de messagerie, etc.).


Cette capture d’écran montre comment **Mail** dans iOS 6 reconnaît une pièce jointe pass et (lors d’un contact) offre aux **ajouter** à Wallet.

 [![](passkit-images/image22.png "Cette capture d’écran montre comment la messagerie dans iOS 6 reconnaît une pièce jointe pass")](passkit-images/image22.png#lightbox)

 [![](passkit-images/image23.png "Cette capture d’écran montre comment offre de messagerie ajouter une pièce jointe pass pour Wallet")](passkit-images/image23.png#lightbox)

Si vous générez une application qui peut être un conduit de pass, ils peuvent être reconnues par :

-  **Extension de fichier** -.pkpass
-  **Type MIME** -application/vnd.apple.pkpass
-  **UTI** – com.apple.pkpass


L’opération de base d’une application conduit consiste à récupérer le fichier pass et appeler de PassKit `PKAddPassesViewController` pour donner à l’utilisateur la possibilité d’ajouter la passe dans leur portefeuille. L’implémentation de ce contrôleur d’affichage est couverte dans la section suivante sur **Compagnon Applications**.

Les Applications conduit n’avez pas besoin être approvisionné pour un ID de Type de passage spécifique de la même façon que les Applications Compagnon.

## <a name="companion-applications"></a>Le Guide des Applications

Une application Compagnon fournit des fonctionnalités supplémentaires pour travailler avec les passes, y compris la création d’un passage, la mise à jour des informations associées à un test ainsi que sinon passe associé à l’application.

Les applications Compagnon ne doivent pas tenter de dupliquer les fonctionnalités de portefeuille. Ils ne sont pas destinées à afficher des passes pour l’analyse.

Cette suite de cette section décrit comment générer une application Compagnon base qui interagit avec PassKit.

### <a name="provisioning"></a>Provisionnement

Étant donné que Wallet est une technologie de stockage, l’application doit être configuré séparément et ne pouvez pas utiliser de profil de provisionnement d’équipe ou Wildcard application ID. Reportez-vous à la [utilisation des fonctionnalités](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) guide pour créer un profil de provisionnement et un ID d’application uniques pour l’application Wallet.

### <a name="entitlements"></a>Droits

Le **Entitlements.plist** fichier doit être inclus dans toutes les récente projet Xamarin.iOS. Pour ajouter un nouveau fichier Entitlements.plist, suivez les étapes de la [utilisation de droits](~/ios/deploy-test/provisioning/entitlements.md) guide.

Pour définir les droits procédez comme suit :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Double-cliquez sur le **Entitlements.plist** fichier dans le panneau de solutions pour ouvrir l’éditeur Entitlements.plist :

![](passkit-images/image31.png "Éditeur de Entitlements.plst")

Dans la section Wallet, sélectionnez le **Wallet activer** option

![](passkit-images/image32.png "Activer les droits de portefeuille")


L’option par défaut est de votre application permettre à que tous les passent des types. Toutefois, il est possible de limiter votre application et autoriser uniquement un sous-ensemble de types de passes d’équipe. Pour activer ce, sélectionnez le **sous-ensemble autoriser de l’équipe de passer des types** et entrez l’identificateur de type pass du sous-ensemble que vous souhaitez autoriser.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Double-cliquez sur le **Entitlements.plist** fichier à ouvrir le fichier source XML.

Pour ajouter le droit de Wallet, définissez le **propriété** à `Passbook Identifiers` dans la liste déroulante, qui définira automatiquement le **Type** `Array`. Ensuite, définissez la chaîne **valeur** à `$(TeamIdentifierPrefix)*`:

![](passkit-images/image33.png "Activer les droits de portefeuille")

Avec cette valeur de chaîne, votre application accepte tous les types de passes. Pour limiter votre application et autoriser uniquement un sous-ensemble de types de passes d’équipe, la valeur est la valeur de chaîne :

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

Où `pass.$(CFBundleIdentifier)` est l’ID de passe qui a été créé [ci-dessus](~/ios/platform/passkit.md)

-----

### <a name="debugging"></a>Débogage

Si vous rencontrez des problèmes de déploiement de votre application, vérifiez que vous utilisez les bonnes **profil de provisionnement** et que le `Entitlements.plist` est sélectionné comme le **droits personnalisés** fichier dans le **iPhone signature du Bundle** options.

Si vous rencontrez cette erreur lors du déploiement :

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

le `pass-type-identifiers` tableau des droits est incorrecte (ou ne correspond pas à la **profil de provisionnement**). Vérifiez les ID de Type passer et votre ID d’équipe sont corrects.

## <a name="classes"></a>Classes

Les classes de PassKit suivantes sont disponibles pour les applications d’accéder aux passages :

-  **PKPass** – une instance d’une étape.
-  **PKPassLibrary** – fournit l’API pour accéder à des passes sur l’appareil.
-  **PKAddPassesViewController** – utilisé pour afficher une passe de l’utilisateur à enregistrer dans leur portefeuille.
-  **PKAddPassesViewControllerDelegate** – permet aux développeurs de Xamarin.iOS

## <a name="example"></a>Exemple

Référence au projet PassLibrary dans le [exemple de code](https://developer.xamarin.com/samples/monotouch/PassKit/) pour cet article. Il illustre les fonctions courantes suivantes qui seraient nécessaires dans une Application Compagnon de portefeuille :

### <a name="check-that-wallet-is-available"></a>Vérifiez que Wallet est disponible

Wallet n’est pas disponible sur l’iPad, donc les applications doivent vérifier avant de tenter d’accéder aux fonctionnalités de PassKit.

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>Création d’une Instance de la bibliothèque Pass

La bibliothèque de PassKit n’est pas un singleton, les applications doivent créer et stocker et de l’instance pour accéder à l’API de PassKit.

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>Obtenir la liste des Passes

Applications peuvent demander une liste de passes à partir de la bibliothèque. Cette liste est automatiquement filtrée par PassKit, afin que vous pouvez uniquement voir les passes qui ont été créés par votre ID d’équipe et qui sont répertoriées dans vos droits.

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

Notez que le simulateur ne filtre pas dans la liste des passes renvoyé, donc cette méthode doit toujours être testée sur des appareils réels. Cette liste peut être affichée dans un UITableView. Le [exemple d’application](https://developer.xamarin.com/samples/monotouch/PassKit/) ressemble à ceci une fois que deux bons d’achat ont été ajoutés :

 [![](passkit-images/image29.png "L’apparence d’application exemple comme suit après ont ajouté des deux coupons")](passkit-images/image29.png#lightbox)

### <a name="displaying-passes"></a>Affichage des Passes

Un ensemble limité d’informations est disponible pour le rendu de passes d’applications Compagnon.

Choisissez à partir de ce jeu de propriétés standard pour afficher des listes de passes, comme le fait de l’exemple de code.

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

Cette chaîne est affichée comme une alerte dans le [exemple](https://developer.xamarin.com/samples/monotouch/PassKit/):

 [![](passkit-images/image30.png "L’alerte sélectionnée du Coupon dans l’exemple")](passkit-images/image30.png#lightbox)

Vous pouvez également utiliser le `LocalizedValueForFieldKey()` méthode pour récupérer des données à partir des champs dans les passes que vous avez conçu (étant donné que vous savez ce que les champs doit être présent). L’exemple de code n’affiche pas cela.

### <a name="loading-a-pass-from-a-file"></a>Chargement d’un passage d’un fichier

Car un passage peut uniquement être ajouté au service Wallet avec l’autorisation de l’utilisateur, un contrôleur d’affichage doit être présenté pour leur permettre de décider. Ce code est utilisé dans le **ajouter** bouton dans l’exemple, pour charger un passage avant génération qui est incorporé dans l’application (vous devez le remplacer par un que vous êtes connecté) :

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

Remplacement d’un test existant ne nécessite pas l’autorisation l’utilisateur, mais elle échouera si le test n’existe pas déjà.

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>Modification d’un passage

PKPass n’est pas mutable, vous ne pouvez pas mettre à jour passe les objets dans votre code. Pour modifier les données dans un test, une application doit avoir accès à un serveur web qui peut conserver un enregistrement de passes et générer un nouveau fichier pass avec les valeurs mises à jour que l’application peut télécharger.

Création d’un fichier pass doit être effectuée sur un serveur, car les passes doivent être signées avec un certificat qui doit rester privées et sécurisées.

Une fois un fichier de mise à jour passe a été généré, utilisez le `Replace` méthode pour remplacer les anciennes données sur l’appareil.

### <a name="display-a-pass-for-scanning"></a>Afficher un Pass pour l’analyse

Comme indiqué précédemment, seuls Wallet peut afficher un pass pour l’analyse. Un passage peut être affiché à l’aide de la `OpenUrl` méthode comme indiqué :

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>Réception des Notifications des modifications

Applications peuvent écouter les modifications apportées à la bibliothèque de passer à l’aide de la `PKPassLibraryDidChangeNotification`. Modifications pourraient être provoquées par les notifications de déclencher des mises à jour en arrière-plan, par conséquent, il est conseillé pour écouter dans votre application.

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

Une présentation détaillée de la création d’une application serveur pour prendre en charge de PassKit est abordée dans cet article d’introduction.

Consultez [le livret dotnet](https://github.com/tomasmcguinness/dotnet-passbook) open source C# code côté serveur.

## <a name="push-notifications"></a>Notifications Push

Une présentation détaillée de l’utilisation de notifications push pour mettre à jour les passes est abordée dans cet article d’introduction.

Vous serez nécessaire pour implémenter l’API REST de type défini par Apple pour répondre aux requêtes web à partir de Wallet lorsque les mises à jour sont requises.

Consultez d’Apple [un passage de la mise à jour](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/Updating.html#//apple_ref/doc/uid/TP40012195-CH5-SW1) guide pour plus d’informations.

## <a name="summary"></a>Récapitulatif

Cet article introduit PassKit, décrit quelques-unes des raisons pourquoi il est utile et décrit les différentes parties qui doivent être implémentées pour une solution complète de PassKit. Il décrit les étapes requises pour configurer votre compte de développeur Apple pour créer des passes, le processus de faire un passage manuellement et également comment accéder aux APIs PassKit à partir d’une application Xamarin.iOS.

## <a name="related-links"></a>Liens associés

- [Wallet pour les développeurs](https://developer.apple.com/wallet/)
- [Exemple de PassKit](https://developer.xamarin.com/samples/monotouch/PassKit/)
- [Guide du développeur Wallet](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/index.html#//apple_ref/doc/uid/TP40012195-CH1-SW1)
- [Frameworks : Apple Pay et Wallet (vidéos WWDC)](https://developer.apple.com/videos/frameworks/apple-pay-and-wallet)
- [Référence du Framework PassKit](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Le livret une référence de Service Web](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
- [Sur les fichiers de Pass](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [dotnet-livret](https://github.com/tomasmcguinness/dotnet-passbook), une bibliothèque open source pour la génération de packages de portefeuille d’iOS
- [Introduction à iOS 6](~/ios/platform/introduction-to-ios6/index.md)
