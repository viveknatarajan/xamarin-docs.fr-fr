---
title: "Modifications apportées à StoreKit"
description: "iOS 6 présente deux modifications apportées à l’API du Kit de magasin : la capacité à afficher iTunes (et le magasin d’applications/iBookstore) option où Apple hébergera vos fichiers téléchargeables acheter des produits à partir de votre application et une nouveau dans l’application. Ce document explique comment implémenter ces fonctionnalités avec Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 253D37D7-44C7-D012-3641-E15DC41C2699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: cbaa389e4a115be2face2b72db6108c836676dc7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="changes-to-storekit"></a>Modifications apportées à StoreKit

_iOS 6 présente deux modifications apportées à l’API du Kit de magasin : la capacité à afficher iTunes (et le magasin d’applications/iBookstore) option où Apple hébergera vos fichiers téléchargeables acheter des produits à partir de votre application et une nouveau dans l’application. Ce document explique comment implémenter ces fonctionnalités avec Xamarin.iOS._

Les principales modifications au magasin de Kit dans iOS6 sont ces deux nouvelles fonctionnalités :

-   **Affichage de contenu dans l’application et achats** – aux utilisateurs d’acheter et télécharger les applications, de musique, la documentation et autres iTunes de contenu sans quitter votre application. Vous pouvez également lier à vos propres applications pour promouvoir des achats ou simplement encourager les évaluations et les révisions. 
-   **Acheter le contenu hébergé dans l’application** – stocke et remettre le contenu associé à vos produits dans l’application bon, ce qui élimine le besoin d’un serveur distinct héberger vos fichiers automatiquement prend en charge le téléchargement en arrière-plan et vous permet de l’Apple écrire moins de code. 


Il est recommandé que ce document être lue conjointement avec le Xamarin.iOS existant [In-App Purchase](~/ios/platform/in-app-purchasing/index.md) documentation.

## <a name="requirements"></a>Configuration requise

Les fonctions de magasin Kit abordées dans ce document requièrent iOS 6 et Xcode 4.5, ainsi que de Xamarin.iOS 6.0.


## <a name="in-app-content-display--purchasing"></a>Affichage de contenu dans l’application et achats

La nouvelle fonctionnalité d’achat dans l’application dans iOS permet aux utilisateurs d’afficher des informations sur les produits et acheter ou télécharger le produit à partir de votre application.
Les applications devront précédemment déclencher iTunes, l’App Store ou l’iBookstore, ce qui entraînerait de l’utilisateur en laissant l’application d’origine. Cette nouvelle fonctionnalité renvoie automatiquement l’utilisateur à votre application lorsqu’elles sont effectuées.

 [ ![](changes-to-storekit-images/image1.png "Retour automatique à une application après l’achat")](changes-to-storekit-images/image1.png)

Il existe plusieurs scénarios où cela peut être utile, notamment (liste non exhaustive) :

-   **Inciter les utilisateurs à évaluer votre application** – vous pouvez ouvrir la page magasin de l’application afin que l’utilisateur peut évaluer et examiner votre application sans quitter ce dernier. 
-   **Promotion d’entre les applications** – autoriser l’utilisateur de voir les autres applications que vous publiez, avec la possibilité d’acheter/télécharger immédiatement. 
-   **Aider les utilisateurs à rechercher et télécharger le contenu** : aider les utilisateurs à acheter le contenu que votre application recherche, gère ou agrégats (par exemple). une application liées à la musique pourrait fournissent une sélection de chansons et chaque morceau d’être achetés à partir de l’application). 


Une fois la `SKStoreProductViewController` l’affichage de l’utilisateur peut interagir avec les informations de produit comme s’ils étaient dans l’iBookstore, l’App Store ou iTunes. Cela concerne :

-  Affichage des captures d’écran (pour les applications),
-  Les morceaux d’échantillonnage ou vidéo (pour la musique, émissions TV et des films),
-  Révisions de lecture (et l’écriture),
-  Achat & téléchargement, ce qui se passe entièrement dans le Kit de magasin et le contrôleur de la vue. Votre application n’a pas besoin de code supplémentaire pour ce faire. 


Sachez que certaines options dans le `SKStoreProductViewController` met toujours en vigueur de l’utilisateur pour laisser votre application et ouvrez l’application de la Boutique adéquate, telles qu’un clic sur **produits connexes** ou d’une application **prise en charge** lien.

### <a name="skstoreproductviewcontroller"></a>SKStoreProductViewController

L’API pour afficher un produit dans n’importe quelle application est très simple : il suffit que vous créez et affichez un `SKStoreProductViewController`. Suivez ces étapes pour créer et afficher un produit :

1.  Créer un `StoreProductParameters` objet pour passer des paramètres pour le contrôleur de la vue, y compris le `productId` dans le constructeur. 
1.  Instanciez le `SKProductViewController` . Attribuer à un champ de niveau classe. 
1.  Affecter un gestionnaire pour le contrôleur de vue `Finished` événement, qui doit ignorer le contrôleur de la vue. Cet événement est appelé lorsque l’utilisateur appuie sur Annuler ; ou sinon finalise une transaction à l’intérieur du contrôleur de la vue. 
1.  Appelez le `LoadProduct` méthode en passant la `StoreProductParameters` et un gestionnaire d’achèvement. Le Gestionnaire d’achèvement doit vérifier que la demande de produit a été correctement et si tel est le cas, présente le `SKProductViewController` modal. Gestion des erreurs doit être ajoutée au cas où le produit ne peut pas être récupéré. 

### <a name="example"></a>Exemple

Le *ProductView* de projet dans le *StoreKit* exemple de code pour cet article implémente un `Buy` méthode qui accepte tout produit de l’ID Apple et affiche le `SKStoreProductViewController`. Le code suivant affiche les informations de produit pour un ID Apple donné :

```csharp
void Buy (int productId)
{
    var spp = new StoreProductParameters(productId);
    var productViewController = new SKStoreProductViewController ();
    // must set the Finished handler before displaying the view controller
    productViewController.Finished += (sender, err) => {
        // Apple's docs says to use this method to close the view controller
        this.DismissModalViewControllerAnimated (true);
    };
    productViewController.LoadProduct (spp, (ok, err) => { // ASYNC !!!
        if (ok) {
            PresentModalViewController (productViewController, true);
        } else {
            Console.WriteLine (" failed ");
            if (err != null)
                Console.WriteLine (" with error " + err);
        }
    });
}
```

L’application ressemble à ceci lorsque vous exécutez – télécharger ou acheter produit entièrement dans le `SKStoreProductViewController`:

 [ ![](changes-to-storekit-images/image2.png "L’application ressemble à ceci lors de l’exécution")](changes-to-storekit-images/image2.png)

### <a name="supporting-older-operating-systems"></a>Prise en charge des systèmes d’exploitation plus anciens

L’exemple d’application inclut le code qui montre comment ouvrir l’App Store, iTunes ou l’iBookstore dans les versions antérieures d’iOS. Utilisez le `OpenUrl` méthode pour ouvrir correctement conçu **itunes.com** URL.

Vous pouvez implémenter une vérification de la version pour déterminer le code à exécuter, comme suit :

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (6,0)) {
    // do iOS6+ stuff, using SKStoreProductViewController as shown above
} else {
    // don't do stuff requiring iOS 6.0, use the old syntax 
    // (which will take the user out of your app)
    var nsurl = new NSUrl("http://itunes.apple.com/us/app/angry-birds/id343200656?mt=8");
    UIApplication.SharedApplication.OpenUrl (nsurl);
}
```

### <a name="errors"></a>Erreurs

L’erreur suivante se produit si l’ID Apple que vous utilisez n’est pas valide, ce qui peut prêter à confusion, car elle implique un problème réseau ou d’authentification.

 `Error Domain=SKErrorDomain Code=5 "Cannot connect to iTunes Store"`

### <a name="reading-objective-c-documentation"></a>Lire la Documentation de Objective-C

Les développeurs de la lecture sur le Kit de magasin sur le portail des développeurs d’Apple verront un protocole – [SKStoreProductViewControllerDelegate](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKITunesProductViewControllerDelegate_ProtocolRef/Reference/Reference.html) – nous l’avons vu par rapport à cette nouvelle fonctionnalité. Le protocole de délégué a une seule méthode – productViewControllerDidFinish – a été exposée en tant que le `Finished` événement sur le `SKStoreProductViewController` dans Xamarin.iOS.


## <a name="determining-apple-ids"></a>Détermination des ID Apple

L’ID Apple requis par le `SKStoreProductViewController` est un *nombre* (à ne pas confondre avec l’ID d’ensemble, comme « com.xamarin.mwc2012 »). Il existe plusieurs façons différentes, que vous pouvez déterminer l’ID Apple pour les produits que vous souhaitez afficher, répertoriées ci-dessous :

### <a name="itunesconnect"></a>iTunesConnect

Pour les applications que vous publiez, il est facile de trouver le **ID Apple** dans iTunes Connect :

 [ ![](changes-to-storekit-images/image3.png "Recherche l’ID Apple dans iTunes Connect")](changes-to-storekit-images/image3.png)

 <a name="Search_API" />


### <a name="search-api"></a>API de recherche

Apple offre une API de recherche dynamique pour rechercher tous les produits dans l’App Store, iTunes et l’iBookstore. Vous trouverez des informations sur la façon d’accéder à l’API de recherche dans [ressources d’applications associées à Apple](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html), bien que l’API est accessible à tous (pas simplement des filiales). Le JSON obtenu peut être analysé pour découvrir la `trackId` qui est l’ID Apple à utiliser avec `SKStoreProductViewController`.

Les résultats inclut également les autres métadonnées notamment affichent des informations et les URL de signature graphique qui peuvent être utilisés pour restituer le produit dans votre application.

Voici quelques exemples :

-   **iBooks app*- [http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us](http://itunes.apple.com/search?term=ibooks&amp;entity=software&amp;country=us) 
-   **Point et l’Ibooks kangourou*- [http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entité = livre électronique&amp;pays = us](http://itunes.apple.com/search?term=dot+and+the+kangaroo&amp;entity=ebook&amp;country=us) 


### <a name="enterprise-partner-feed"></a>Flux de partenaire d’entreprise

Apple offre des partenaires agréés avec une image complète des données de leurs produits, sous la forme de fichiers plats des prêts à la base de données téléchargeables. Si vous qualifiez pour l’accès à la [entreprise partenaire d’alimentation](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-enterprise-partner-feed.html) ensuite l’ID Apple pour tous les produits sont accessibles dans le jeu de données.

Notez que de nombreux utilisateurs de l’entreprise de flux de partenaire sont membres de la [applications associées à un programme](http://www.apple.com/itunes/affiliates) qui permet des commissions acquis sur les ventes de produits. `SKStoreProductViewController` ne prend pas en charge les applications associées à l’ID (au moment de l’écriture ; cela peut être ajouté par Apple dans les futures).

### <a name="direct-product-links"></a>Produit liens directs

L’ID Apple pour un produit peut être déduit à partir de son lien URL d’aperçu iTunes.
Dans n’importe quel iTunes lien produit (pour les applications, de musique ou de la documentation) Recherchez la partie de l’URL commençant par `id` et utilisez le numéro qui suit.

Par exemple, le lien direct vers iBooks est

```csharp
http://itunes.apple.com/us/app/ibooks/id364709193?mt=8
```

et l’ID Apple est **364709193**. De même pour l’application MWC2012, le lien direct est

```csharp
http://itunes.apple.com/us/app/mwc-2012-unofficial/id496963922?mt=8
```

et l’ID Apple est **496963922**.

## <a name="in-app-purchase-hosted-content"></a>Dans l’application fournisseur de contenu hébergé

Si vos achats dans l’application se composent d’un contenu téléchargeable (par exemple, la documentation ou autre support, jeu art niveau et de configuration ou autres fichiers volumineux) puis ces fichiers utilisés pour être hébergé sur votre serveur web et applications devaient incorporer du code pour les télécharger en toute sécurité après achat. Dans iOS 6 Apple a introduit une option où ils hébergera vos fichiers sur leurs serveurs, supprime la nécessité d’un serveur distinct. La fonctionnalité est uniquement disponible pour les produits Non consommable (pas consommable ou abonnements). À l’aide du service d’hébergement d’Apple présente les avantages suivants :

-  Enregistrez les coûts d’hébergement et de bande passante.
-  Probablement plus évolutive que vous utilisez tout hôte du serveur. 
-  Moins de code à écrire, étant donné que vous n’êtes pas obligé de créer tout traitement côté serveur. 
-  Téléchargement en arrière-plan est implémenté pour vous.


Remarque : test hébergé contenu dans l’application achat dans iOS que Simulator n’est pas pris en charge, vous devez tester avec un périphérique réel.

### <a name="hosted-content-basics"></a>Principes de base de contenu hébergés

Avant d’iOS 6, ont été deux moyens de fournir un produit (décrite plus en détail dans [Xamarin In-App Purchase](~/ios/platform/in-app-purchasing/index.md) documentation) :

-   **Produits intégrés** – les fonctionnalités qui sont disponibles à l’achat, mais qui sont intégré à l’application (soit en tant que code ou de ressources incorporées). Filtres de photo déverrouillés ou dans le jeu power-ups sont des exemples de produits intégrés. 
-   **Les produits serveur remis** – après l’achat, l’application doit télécharger le contenu à partir d’un serveur que vous activez. Ce contenu est téléchargé lors de l’achat, stocké sur l’appareil et ensuite rendu dans le cadre de la fourniture du produit. La documentation, des problèmes magazines ou des niveaux de jeu se composent de fichiers de bibliothèque et la configuration en arrière-plan sont des exemples. 


Dans iOS 6 Apple offre une variation de produits de remise de serveur : ils hébergera vos fichiers de contenu sur leurs serveurs. Cela rend beaucoup plus simple pour générer des produits de remise de serveur, car vous n’êtes pas obligé de faire fonctionner un serveur distinct et Store Kit fournit les fonctionnalités de téléchargement en arrière-plan que vous aviez précédemment écrire vous-même. Pour tirer parti de l’hébergement de d’Apple, activer l’hébergement du contenu pour les nouveaux produits dans l’application achat et modifiez votre code de magasin Kit pour tirer parti de celui-ci. Les fichiers de contenu de produit sont ensuite créés à l’aide de Xcode et téléchargés sur les serveurs d’Apple pour révision et mise en production.

 [ ![](changes-to-storekit-images/image4.png "Le processus de génération et de remise")](changes-to-storekit-images/image4.png)

À l’aide de l’App Store pour fournir dans l’application d’achat *avec contenu hébergé* nécessite l’installation et la configuration suivante :

-   **iTunes Connect** – vous *doit* ont fourni les vos informations bancaires et les taxes à Apple afin qu’ils peuvent remettre fonds collectées à votre place. Vous pouvez ensuite configurer les produits pour vendre et configurer des comptes d’utilisateur de bac à sable pour tester des achats.  *Vous devez également configurer le contenu hébergé**pour les produits non-consommable que vous souhaitez héberger avec Apple* *.*  
-   **Portail d’approvisionnement iOS** : création d’un identificateur de lot et activation de l’accès de magasin d’applications pour votre application, comme vous le feriez pour n’importe quelle application prenant en charge dans l’application d’achat. 
-   **Stocker Kit** – Ajout de code à votre application pour l’affichage des produits, d’acheter des produits et de restauration des transactions.  *Dans iOS 6 Store Kit gère également le téléchargement de contenu de votre produit, en arrière-plan, avec les mises à jour de progression.* 
-   **Code personnalisé** : pour suivre les achats effectués par les clients et indiquer les produits ou services qu’ils ont achetés. Utiliser les nouvelles classes de magasin Kit iOS 6, comme `SKDownload` pour récupérer le contenu hébergé par Apple. 


Les sections suivantes expliquent comment implémenter le contenu hébergé, de créer et de charger le package pour la gestion de l’achat et de télécharger les processus, à l’aide de l’exemple de code pour cet article.


### <a name="sample-code"></a>Exemple de code

L’exemple de projet *HostedNonConsumables* (dans StoreKitiOS6.zip) montre comment créer une application qui utilise hébergé le contenu. L’application offre deux « chapitres » pour la vente, le contenu pour lequel est hébergé sur des serveurs Apple. Le contenu se compose d’un fichier texte et une image, bien que le contenu beaucoup plus complexe peut être utilisé dans une application réelle.

Avant, pendant et après un achat, l’application se présente ainsi :

 [ ![](changes-to-storekit-images/image5.png "L’application ressemble à ceci avant, pendant et après un achat")](changes-to-storekit-images/image5.png)

Le fichier texte et image sont téléchargés et copiés dans le répertoire Documents de l’application. Consultez le [utilisation de la documentation du système de fichiers](~/ios/app-fundamentals/file-system.md) pour plus d’informations sur les différents annuaires disponibles pour le stockage de l’application.

## <a name="itunes-connect"></a>iTunes Connect

Lors de la création de nouveaux produits qui utiliseront Apple du contenu d’hébergement veillez à sélectionner le **Non consommable** type de produit. Autres types de produits ne prennent pas en charge hébergement du contenu. En outre, vous ne devez pas activer d’hébergement pour le contenu *existant* produits que vous vendez ; activez uniquement sur l’hébergement de contenu pour les nouveaux produits.

 [ ![](changes-to-storekit-images/image6.png "Sélectionnez le type de produit Non consommable")](changes-to-storekit-images/image6.png)

Entrez un **ID de produit**. Cela sera nécessaire ultérieurement lorsque vous créez le contenu de ce produit.

 [ ![](changes-to-storekit-images/image7.png "Entrez un ID de produit")](changes-to-storekit-images/image7.png)

Hébergement du contenu est définie dans la section de détails. Avant l’achat dans l’application passer en ligne, désactivez simplement la case à cocher « Hôte contenu avec Apple » si vous souhaitez annuler (même si vous avez téléchargé du contenu test). Toutefois hébergement du contenu ne peut pas être supprimé après que l’achat dans l’application a été activée.

 [ ![](changes-to-storekit-images/image8.png "Hébergement du contenu auprès d’Apple")](changes-to-storekit-images/image8.png)

Une fois que vous avez activé l’hébergement du contenu, le produit passe **en attente pour le téléchargement** état et afficher ce message :

 [ ![](changes-to-storekit-images/image9.png "Le produit sera Entrez en attente pour l’état de téléchargement et afficher ce message")](changes-to-storekit-images/image9.png)

Le contenu doit maintenant être créé avec Xcode et téléchargées à l’aide de l’outil d’archivage. Instructions pour la création de packages de contenu n’est donné dans la section suivante **création. Les fichiers PKG**.

## <a name="creating-pkg-files"></a>Création. Fichiers PKG

Les fichiers de contenu que vous téléchargez vers Apple doivent respecter les restrictions suivantes :

-  Ne peut pas dépasser 2 Go.
-  Ne peut pas contenir le code exécutable (ou des liens symboliques qui pointent en dehors du contenu). 
-  Doit être formatée correctement (y compris un fichier .plist) et ont une extension de fichier .pkg. Cela se fera automatiquement si vous suivez ces instructions à l’aide de Xcode. 


Vous pouvez ajouter de nombreux fichiers différents et les types de fichiers, tant qu’ils répondent à ces restrictions. Le contenu est compressé avant la remise à votre application et décompressé par magasin Kit avant que votre code n’y accède.

Après avoir téléchargé un package de contenu, il peut être remplacé avec un contenu plus récente. Nouveau contenu doit être téléchargé et soumis pour validation via la procédure normale. Vous devez incrémenter le `ContentVersion` champ dans les packages de contenu mis à jour.

### <a name="xcode-in-app-purchase-content-projects"></a>Projets de contenu d’achat Xcode dans l’application

Création de packages de contenu pour les produits dans l’application achat actuellement requiert Xcode. Il est non OBJECTIVE-C codage ; Xcode est un nouveau type de projet pour ces packages contenant simplement vos fichiers et un fichier plist.

Notre exemple d’application a chapitres pour la vente, chaque package de contenu chapitre contient :

-  un fichier texte, et
-  une image pour représenter le chapitre.


Commencez par sélectionner **fichier > Nouveau projet** dans le menu, puis en choisissant **dans l’application achat contenu**:

 [ ![](changes-to-storekit-images/image10.png "Choisir le contenu de l’achat de dans l’application")](changes-to-storekit-images/image10.png)

Entrez le **Product Name** et **identificateur de la société** telles que la **identificateur de lot** correspond à la **ID de produit** vous avez entré dans iTunes Se connecter pour ce produit.

 [ ![](changes-to-storekit-images/image11.png "Entrez le nom et l’identificateur")](changes-to-storekit-images/image11.png)

Maintenant avoir une valeur vide **dans l’application achat contenu** projet. Vous pouvez cliquer sur et **ajouter des fichiers...** ou les faire glisser dans le **Project Navigator**. Vérifiez que le **ContentVersion** est correcte (elle doit commencer à 1.0, mais si vous souhaitez ultérieurement mettre à jour votre contenu, pensez à incrémenter).

Cette capture d’écran montre Xcode avec les fichiers de contenu inclus dans le projet et les entrées de plist visibles dans la fenêtre principale :

 [ ![](changes-to-storekit-images/image12.png "Cette capture d’écran montre Xcode avec les fichiers de contenu inclus dans le projet et les entrées de plist visibles dans la fenêtre principale")](changes-to-storekit-images/image12.png)

Une fois que vous avez ajouté tous vos fichiers de contenu vous pouvez enregistrer ce projet et modifier à nouveau ultérieurement ou commencer le processus de téléchargement.

## <a name="uploading-pkg-files"></a>Téléchargement. Fichiers PKG

Le moyen le plus simple pour télécharger les packages de contenu est avec le **Xcode Archive outil**. Choisissez **produit > Archive** dans le menu pour commencer :

 ![](changes-to-storekit-images/image13.png "Choisissez Archiven")

Le package de contenu apparaîtra ensuite dans l’archive comme indiqué ci-dessous. Notez l’icône et le type d’archive affichent il s’agit d’un **archives de contenu dans l’application achat**. Cliquez sur **valider...** Pour vérifier notre package de contenu pour les erreurs sans réellement réalisation le téléchargement.

 [ ![](changes-to-storekit-images/image14.png "Valider le package")](changes-to-storekit-images/image14.png)

Connectez-vous avec vos informations d’identification de connexion iTunes :

 [ ![](changes-to-storekit-images/image15.png "Connectez-vous avec vos informations d’identification de connexion iTunes")](changes-to-storekit-images/image15.png)

Choisissez l’application correcte et dans l’application achat pour associer ce contenu avec :

 [ ![](changes-to-storekit-images/image16.png "Choisissez l’application correcte et dans l’application achat pour associer ce contenu avec")](changes-to-storekit-images/image16.png)

Vous devez voir un message comme suit :

 ![](changes-to-storekit-images/image17.png "Un exemple aucun message de problèmes")

À présent passer par un processus similaire, mais en cliquant sur **distribuer...** réellement pour télécharger le contenu.

 [ ![](changes-to-storekit-images/image18.png "Distribuer l’application")](changes-to-storekit-images/image18.png)

Sélectionnez la première option, pour télécharger le contenu :

 ![](changes-to-storekit-images/image19.png "Télécharger le contenu")

Nouveau se connecter :

 [ ![](changes-to-storekit-images/image15.png "Compte de connexion dans")](changes-to-storekit-images/image15.png)

Choisissez l’application correcte et l’enregistrement de bon de dans l’application de télécharger le contenu à :

 [ ![](changes-to-storekit-images/image20.png "Choisissez l’enregistrement de bon d’application et dans l’application")](changes-to-storekit-images/image20.png)

Patientez pendant que vos fichiers sont téléchargés :

 [ ![](changes-to-storekit-images/image21.png "La boîte de dialogue de téléchargement de contenu")](changes-to-storekit-images/image21.png)

Lorsque le téléchargement est terminé, un message s’affiche pour vous informer que le contenu a été envoyé à l’App Store.

 [ ![](changes-to-storekit-images/image22.png "Un exemple de message de réussite du téléchargement")](changes-to-storekit-images/image22.png)

Une fois que qui a été effectuée, lorsque vous revenez à la page du produit dans iTunes Connect, elle affiche les détails du package et se trouver dans **prêt à envoyer** état. Lorsque le produit est dans cet état, vous pouvez commencer à tester dans l’environnement de bac à sable. Vous n’avez pas besoin de « submit » du produit pour les tests dans le bac à sable.

 [ ![](changes-to-storekit-images/image23.png "iTunes Connect, elle affiche les détails du package et être prêt à l’état de l’envoi")](changes-to-storekit-images/image23.png)

Il peut prendre un certain temps (par exemple). quelques minutes) entre le téléchargement de l’archive et l’état de connexion en cours de mise à jour iTunes. Vous pouvez soumettre le produit pour un examen séparément ou soumettre conjointement avec un binaire de l’application. Uniquement après que Apple a officiellement a approuvé le contenu qu’il sera disponible dans la magasin d’applications de production à l’achat dans votre application.

### <a name="pkg-file-format"></a>Format de fichier PKG

Pour créer et télécharger un package de contenu hébergé à l’aide de Xcode et l’outil Archive signifie que vous verrez jamais le contenu du package lui-même. Les fichiers et répertoires dans les packages créés pour l’exemple d’application ressemblent à ceci, avec la `plist` fichier dans la racine et les fichiers du produit dans un `Contents` sous-répertoire :

 [ ![](changes-to-storekit-images/image24.png "Le fichier .plist dans la racine et les fichiers du produit dans un sous-répertoire de contenu")](changes-to-storekit-images/image24.png)

Notez la structure de répertoires du package (en particulier l’emplacement des fichiers dans le `Contents` sous-répertoire) car vous avez besoin comprendre ces informations pour extraire les fichiers à partir du package sur l’appareil.

### <a name="updating-package-content"></a>Mise à jour du contenu du Package

La procédure de mise à jour de contenu après avoir été approuvée :

-  Modifier le projet dans-App Purchase contenu dans Xcode.
-  Augmenter le numéro de version.
-  Télécharger à nouveau à iTunes Connect. Ultérieures acheteurs obtient automatiquement la dernière version, mais les utilisateurs qui disposent déjà de l’ancienne version ne recevront pas de notification. 
-  Votre application est responsable de la notification des utilisateurs et de les encourager à récupérer une version plus récente du contenu. L’application doit également créer une fonction qui télécharge la nouvelle version. Ceci doit être fait à l’aide de la fonctionnalité de restauration du Kit de magasin. 
-  Pour déterminer si une version plus récente existe vous pouvez générer une fonctionnalité à votre application pour extraire SKProducts (par exemple). même les processus qui sont utilisée pour récupérer le prix des produits) et comparer la propriété ContentVersion. 

## <a name="purchasing-overview"></a>Vue d’ensemble de l’achat

Avant de lire cette section, passez en revue les existantes [documentation de In-App Purchase](~/ios/platform/in-app-purchasing/index.md).

La séquence d’événements qui se produit lorsqu’un produit avec le contenu hébergé est achetée et téléchargement est illustré dans ce diagramme :

 [ ![](changes-to-storekit-images/image25.png "La séquence d’événements qui se produit lorsqu’un produit avec le contenu hébergé est acheté et télécharger")](changes-to-storekit-images/image25.png)

1.  Nouveaux produits peuvent être créés dans iTunes Connect avec le contenu hébergé activé. Le contenu réel est construit séparément dans Xcode (comme simplement en faisant glisser des fichiers dans un dossier) et puis archivé et téléchargé dans iTunes (aucun codage n’est nécessaire). Chaque produit est ensuite soumis pour approbation, après quoi il devient disponible à l’achat. Dans l’exemple de code, ces ID de produit sont codés en dur, mais hébergement du contenu auprès d’Apple est plus souple si vous stockez la liste des produits disponibles sur un serveur distant afin qu’il peut être mis à jour lorsque vous soumettez des nouveaux produits et du contenu iTunes Connect. 
1.  Lorsque l’utilisateur achète un produit, une transaction est placée dans la file d’attente de paiement pour le traitement. 
1.  Kit de magasin transfère la demande de bon aux serveurs iTunes pour le traitement. 
1.  Transaction est effectuée sur les serveurs iTunes (par exemple). la charge du client) et un accusé de réception est renvoyé à l’application, avec les informations de produit attachées, notamment s’il est téléchargeable (et dans ce cas, la taille du fichier et autres métadonnées). 
1.  Votre code doit vérifier si le produit est downloadble et si tel est le cas effectuer une demande de téléchargement de contenu qui est également placée dans la file d’attente de paiement. Kit de magasin envoie cette demande aux serveurs iTunes. 
1.  Serveur retourne un fichier de contenu au Kit de magasin, ce qui fournit un rappel pour retourner la progression du téléchargement et le temps restant estimations à votre code. 
1.  Une fois terminé, vous recevez notifié et passé d’un emplacement de fichier dans le dossier du Cache. 
1.  Votre code doit copier les fichiers et les vérifier, enregistrer les États que vous devez vous rappeler que le produit a été acheté. Profitez-en pour définir l’indicateur de sauvegarde correctement sur les nouveaux fichiers (indicateur : si elles proviennent d’un serveur et ne sont jamais modifiées par l’utilisateur, vous devez probablement ignorer leur sauvegarde, étant donné que l’utilisateur peut toujours récupérer les serveurs Apple avenir). 
1.  Appelez FinishTransaction. Cela est IMPORTANT car elle supprime de la transaction à partir de la file d’attente de paiement. Il est également important que vous n’appelez pas FinishTransaction jusqu'à une fois que vous avez copié le contenu du répertoire du Cache. Une fois que vous appelez FinishTransaction, les fichiers mis en cache sont susceptibles d’être purgées rapidement. 


## <a name="implementing-hosted-content-purchase"></a>Implémentation de fournisseur de contenu hébergé

Les informations suivantes doivent être lue conjointement avec le texte complet [documentation des achats dans l’application](~/ios/platform/in-app-purchasing/index.md). Les informations contenues dans ce document se concentre sur les différences entre le contenu hébergé et l’implémentation précédente.


### <a name="classes"></a>Classes

Les classes suivantes ont été ajoutés ou modifiés pour le contenu du support hébergé dans iOS 6 :

-   **SKDownload** – nouvelle classe qui représente un téléchargement en cours. L’API permet plusieurs par produit, toutefois initialement une seule a été implémentée. 
-   **SKProduct** – nouvelles propriétés : `Downloadable` , `ContentVersion` , `ContentLengths` tableau. 
-   **SKPaymentTransaction** – nouvelle propriété ajoutée : `Downloads` , qui contient une collection de `SKDownload` objets si ce produit a hébergé le contenu disponible pour téléchargement. 
-   **SKPaymentQueue** – nouvelle méthode ajoutée : `StartDownloads` . Appelez cette méthode avec `SKDownload` objets afin d’extraire leur contenu hébergé. Téléchargement peut se produire en arrière-plan. 
-   **SKPaymentTransactionObserver** – nouvelle méthode : `UpdateDownloads` . Kit de magasin appelle cette méthode avec des informations de progression sur les opérations de téléchargement en cours. 


Détails de la nouvelle `SKDownload` classe :

-   **Progression** – une valeur comprise entre 0 et 1, ce qui vous permet d’afficher un indicateur de pourcentage d’achèvement à l’utilisateur. N’utilisez pas de progression == 1 pour détecter si le téléchargement est terminé, vérifier état == terminé. 
-   **TimeRemaining** – estimation du téléchargement temps restant, en secondes. -1 signifie qu’il est toujours calcul de l’estimation. 
-   **État** : actif, en attente, terminé, échec, suspendu, annulé. 
-   **ContentURL** – fichier emplacement où le contenu a été placé sur le disque, dans le `Cache` active. Rempli uniquement une fois le téléchargement terminé. 
-   **Erreur** : Vérifiez que cette propriété si l’état est Échec. 


Les interactions entre les classes dans l’exemple de code sont affichées dans ce diagramme (le code spécifique aux achats contenus hébergés est indiqué en vert) :

 [ ![](changes-to-storekit-images/image26.png "Les achats de contenu hébergés apparaît en vert dans ce diagramme")](changes-to-storekit-images/image26.png)

L’exemple de code dans lequel ces classes ont été utilisées est indiqué dans le reste de cette section :

### <a name="custompaymentobserver-skpaymenttransactionobserver"></a>CustomPaymentObserver (SKPaymentTransactionObserver)

Modifier existants `UpdatedTransactions` remplacement pour rechercher les contenu téléchargeable, puis appelez `StartDownloads` si nécessaire :

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
    foreach (SKPaymentTransaction transaction in transactions) {
        switch (transaction.TransactionState) {
        case SKPaymentTransactionState.Purchased:
            // UPDATED FOR iOS 6
            if (transaction.Downloads != null && transaction.Downloads.Length > 0) {
                // Purchase complete, and it has downloads... so download them!
                SKPaymentQueue.DefaultQueue.StartDownloads (transaction.Downloads);
                // CompleteTransaction() call has moved after downloads complete
            } else {
                // complete the transaction now
                theManager.CompleteTransaction(transaction);
            }
            break;
        case SKPaymentTransactionState.Failed:
            theManager.FailedTransaction(transaction);
            break;
        case SKPaymentTransactionState.Restored:
            // TODO: you must decide how to handle restored transactions.
            // Triggering all the downloads at once is not advisable.
            theManager.RestoreTransaction(transaction);
            break;
        default:
            break;
        }
    }
}
```

Nouvelle méthode substituée `UpdatedDownloads` est indiqué ci-dessous. Kit de magasin appelle cette méthode après `StartDownloads` est déclenchée dans `UpdatedTransactions`. Cette méthode est appelée *plusieurs fois* à intervalles indéterminés pour fournir la progression du téléchargement des et à nouveau lorsque le téléchargement est terminé. Notez la méthode accepte un tableau de `SKDownload` des objets, pour que chaque appel de méthode peut vous fournir l’état de plusieurs téléchargements dans la file d’attente. Comme indiqué dans l’implémentation ci-après que les États de téléchargement sont vérifiées chaque fois et l’action appropriée.

```csharp
// ENTIRELY NEW METHOD IN iOS6
public override void PaymentQueueUpdatedDownloads (SKPaymentQueue queue, SKDownload[] downloads)
{
    Console.WriteLine (" -- PaymentQueueUpdatedDownloads");
    foreach (SKDownload download in downloads) {
        switch (download.DownloadState) {
        case SKDownloadState.Active:
            // TODO: implement a notification to the UI (progress bar or something?)
            Console.WriteLine ("Download progress:" + download.Progress);
            Console.WriteLine ("Time remaining:   " + download.TimeRemaining); // -1 means 'still calculating'
            break;
        case SKDownloadState.Finished:
            Console.WriteLine ("Finished!!!!");
            Console.WriteLine ("Content URL:" + download.ContentUrl);

            // UNPACK HERE! Calls FinishTransaction when it's done
            theManager.SaveDownload (download);

            break;
        case SKDownloadState.Failed:
            Console.WriteLine ("Failed"); // TODO: UI?
            break;
        case SKDownloadState.Cancelled:
            Console.WriteLine ("Cancelled"); // TODO: UI?
            break;
        case SKDownloadState.Paused:
        case SKDownloadState.Waiting:
            break;
        default:
            break;
        }
    }
}
```

### <a name="inapppurchasemanager-skproductsrequestdelegate"></a>InAppPurchaseManager (SKProductsRequestDelegate)

Cette classe contient une nouvelle méthode `SaveDownload` qui est appelée une fois chaque téléchargement terminé avec succès.

Le contenu hébergé a été téléchargé avec succès et décompressé dans le `Cache` active. La structure de la. Fichier PKG nécessite tous les fichiers à enregistrer dans un `Contents` sous-répertoire, donc le code suivant extrait les fichiers depuis le `Contents` sous-répertoire.

Le code itère sur tous les fichiers dans le package de contenu et les copie dans le `Documents` répertoire dans un sous-dossier nommé pour le `ProductIdentifier`. Enfin, il appelle `CompleteTransaction`, qui appelle `FinishTransaction` pour supprimer la transaction à partir de la file d’attente de paiement.

```csharp
// ENTIRELY NEW METHOD IN iOS 6
public void SaveDownload (SKDownload download)
{
    var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
    var targetfolder = System.IO.Path.Combine (documentsPath, download.Transaction.Payment.ProductIdentifier);
    // targetfolder will be "/Documents/com.xamarin.storekitdoc.montouchimages/" or something like that
    if (!System.IO.Directory.Exists (targetfolder))
        System.IO.Directory.CreateDirectory (targetfolder);
    foreach (var file in System.IO.Directory.EnumerateFiles 
             (System.IO.Path.Combine(download.ContentUrl.Path, "Contents"))) { // Contents directory is the default in .PKG files
        var fileName = file.Substring (file.LastIndexOf ("/") + 1);
        var newFilePath = System.IO.Path.Combine(targetfolder, fileName);
        if (!System.IO.File.Exists(newFilePath)) // HACK: this won't support new versions...
            System.IO.File.Copy (file, newFilePath);
        else
            Console.WriteLine ("already exists " + newFilePath);
    }
    CompleteTransaction (download.Transaction); // so it gets 'finished'
}
```

Lorsque `FinishTransaction` est appelée téléchargé les fichiers sont n’est plus garanties dans le `Cache` active. Tous les fichiers doivent être copiés avant d’appeler `FinishTransaction`.


## <a name="other-considerations"></a>Autres considérations

L’exemple de code ci-dessus illustre une implémentation simple de l’achat de contenu hébergé. Il existe quelques points supplémentaires que vous devez prendre en compte :

### <a name="detecting-updated-content"></a>Détection de contenu mis à jour

Bien qu’il soit possible de mettre à jour vos packages de contenu hébergés, Store Kit ne fournit pas d’aucun mécanisme pour distribuer ces mises à jour sur les utilisateurs qui ont déjà téléchargé et acheté le produit. Pour implémenter cette fonctionnalité, votre code peut vérifier le nouveau `SKProduct.ContentVersion` propriété (si le `SKProduct` est `Downloadable`) régulièrement et de détecter si la valeur est incrémentée. Vous pouvez également générer un système de notification de transmission.

### <a name="installing-updated-content-versions"></a>L’installation des Versions de contenu mis à jour

L’exemple de code ci-dessus ignore la copie des fichiers si le fichier existe déjà. Cela n’est pas judicieux si vous souhaitez prendre en charge des versions plus récentes du contenu en cours de téléchargement.

Une alternative peut consister à copier le contenu dans un dossier nommé pour la version et effectuer le suivi de ce qui est la version actuelle (par exemple). dans `NSUserDefaults` ou partout où vous stockez des enregistrements d’achat).

### <a name="restoring-transactions"></a>Restauration des Transactions

Lorsque `SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions` est appelée, Store Kit retourne toutes les transactions précédentes pour l’utilisateur. Si elles ont acheté un grand nombre d’éléments, ou si chaque fournisseur a les packages de contenu très volumineux, puis la restauration peut entraîner un trafic réseau important que tous les éléments Obtient la file d’attente pour le téléchargement à la fois.

Envisagez le suivi de si un produit a été acheté séparément le téléchargement du package de contenu associé.

### <a name="pausing-restarting-and-canceling-downloads"></a>Suspendre, redémarrer et annuler des téléchargements

Bien que l’exemple de code ne présente pas cette fonctionnalité, il est possible de suspendre et redémarrer les téléchargements de contenu hébergés. Le `SKPaymentQueue.DefaultQueue` a des méthodes pour `PauseDownloads`, `ResumeDownloads` et `CancelDownloads`.

Si le code appelle `FinishTransaction` sur la file d’attente de paiement avant le téléchargement en cours `Finished` alors que le téléchargement est automatiquement annulé.

### <a name="setting-the-skip-backup-flag-on-the-downloaded-content"></a>Définition de l’indicateur de SKIP de sauvegarde sur le contenu téléchargé

Instructions de sauvegarde iCloud d’Apple suggère que le contenu non-utilisateur qui est facilement restauré à partir d’un serveur doit *pas* être sauvegardé (car il utilise inutilement l’espace de stockage iCloud). Reportez-vous à la [fonctionne avec le système de fichiers](~/ios/app-fundamentals/file-system.md) documentation pour plus d’informations sur la définition de l’attribut de sauvegarde.

## <a name="summary"></a>Récapitulatif

Cet article a introduit deux nouvelles fonctionnalités du Kit de magasin dans iOS6 : achat iTunes et autre contenu à partir de votre application et en utilisant le serveur d’Apple pour héberger votre propre achats dans l’application. Cette introduction doit être lue conjointement avec le [documentation de In-App Purchase](~/ios/platform/in-app-purchasing/index.md) pour une couverture complète de l’implémentation de la fonctionnalité de magasin Kit.

## <a name="related-links"></a>Liens associés

- [StoreKit (sample)](https://developer.xamarin.com/samples/StoreKit/)
- [Achats dans l’application](~/ios/platform/in-app-purchasing/index.md)
- [Référence StoreKit Framework](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/StoreKit_Collection/_index.html)
- [Référence de classe de SKStoreProductViewController](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKITunesProductViewController_Ref/SKStoreProductViewController.html)
- [iTunes référence des API de recherche](http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)
- [SKDownload](https://developer.apple.com/library/prerelease/ios/#documentation/StoreKit/Reference/SKDownload_Ref/Introduction/Introduction.html)
- [SKPaymentQueue](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKPaymentQueue_Class/Reference/Reference.html#/apple_ref/occ/instm/SKPaymentQueue/cancelDownloads:)
- [SKProduct](https://developer.apple.com/library/prerelease/ios/documentation/StoreKit/Reference/SKProduct_Reference/Reference/Reference.html#/apple_ref/occ/instp/SKProduct/downloadable)
- [WWDC vidéo : Vente des produits avec le Kit de Store](https://developer.apple.com/videos/wwdc/2012/?include=302#302)
