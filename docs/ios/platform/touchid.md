---
title: ID tactile
description: "Touch ID est une technologie d’authentification d’empreintes digitales d’Apple."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 7ee2af392a00e045b1992d189a15d7a0ee04b02f
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="touch-id"></a>ID tactile

ID de contact a été introduite dans iOS 7 comme un moyen d’authentifier l’utilisateur, semblable à un code secret. Toutefois, il a été limité à déverrouiller l’appareil, à l’aide de l’App Store, à l’aide d’iTunes et l’authentification de la chaîne de la clé iCloud uniquement.

Il existe désormais deux façons d’utiliser Touch ID comme mécanisme d’authentification dans une application iOS 8 à l’aide de l’API d’authentification Local. Actuellement, il n’est pas possible d’utiliser l’authentification locale pour l’authentification à distance.

Pour bien comprendre Touch ID et sa valeur, nous devons examiner trousseau Services et la signifient de ces nouvelles modifications des données de l’utilisateur. Trousseau d’accès a également été développée sur dans iOS 8 via l’utilisation de la nouvelle fonctionnalité de listes de contrôle d’accès (ACL).

## <a name="keychain--secure-enclave"></a>Trousseau & Enclave sécurisé

Chaîne de clé est une base de données volumineux qui fournit le stockage sécurisé pour les mots de passe, les clés, les certificats et les notes pour un ID Apple individuels Dans iOS 8 une application toujours a accès à ses propres éléments trousseau unique et ne peut pas accéder à tous les éléments de trousseau d’autres applications. Cela diffère du système d’exploitation X où la chaîne de clé est déverrouillée avec un mot de passe, en permettant à n’importe quelle application Keychain compatible avec les Services d’utiliser la chaîne de clé. Dans cet article, nous nous concentrerons sur le fonctionne de la chaîne de clé dans iOS 8.

Chaîne de clé est une base de données spécialisée, où chaque ligne est appelé un _trousseau élément_. Chaque élément est décrit par les attributs de trousseau et est composé de valeurs chiffrées. Pour permettre une utilisation efficace de trousseau, il est optimisé pour les petits éléments, ou _secrets_.
Chaque élément de trousseau est protégé par le code d’accès aux utilisateurs et un secret d’appareil unique. Éléments de la chaîne de clé doivent être protégés même quand les utilisateurs ne sont pas de leurs appareils. Ceci est implémenté dans iOS en autorisant uniquement les éléments soient disponibles lorsque l’appareil est déverrouillé, lorsque l’appareil est verrouillé, ils deviennent indisponibles. Ils peuvent également être stockées dans une sauvegarde chiffrée. Une des fonctionnalités clés de trousseau consiste à appliquer le contrôle d’accès ; une application a accès à sa partie de la chaîne de clé, et ne pourront pas toutes les autres applications. Le diagramme ci-dessous illustre la façon dont une application interagit avec la chaîne de clé :

[![](touchid-images/image1.png "Ce diagramme illustre la façon dont une application interagit avec la chaîne de clé")](touchid-images/image1.png)

### <a name="secure-enclave"></a>Enclave sécurisé

La chaîne de clé ne peut pas déchiffrer l’élément trousseau par lui-même ; au lieu de cela, il est effectué dans le *Enclave de sécuriser*. L’enclave sécurisé est un coprocesseur au sein de la puce A7 qui est chargée de déterminer une correspondance à partir des données d’empreinte digitale du capteur Touch ID sur une impression inscrite. Il sera ensuite déchiffrer l’élément trousseau et retourner la clé secrète déchiffrée à la chaîne de clé.

### <a name="working-with-keychain"></a>Utilisation de trousseau

Tout d’abord votre application doit rechercher dans la chaîne de clé pour voir s’il existe un mot de passe. S’il n’existe pas, vous devrez demander un mot de passe, l’utilisateur n’est pas continuellement invité. Si le mot de passe doit être mis à jour, inviter l’utilisateur à un nouveau mot de passe et passez la valeur mise à jour à la chaîne de clé.


> ℹ️ **Remarque**: avec les clés secrètes, vous recevez à partir de la base de données, il n’est pas simplement de meilleure pratique, mais prévu qu’aucun secret n’est conservés dans la mémoire. Vous devez uniquement conserver un secret pour que la durée pendant laquelle il est nécessaire et absolument ne l’attribuez ne pas à une variable globale !




## <a name="keychain-acl-and-touch-id"></a>ID de trousseau ACL et tactile

Liste de contrôle d’accès est un nouvel attribut d’élément de trousseau dans iOS 8 qui décrit les informations concernant ce qui doit se produire pour permettre une opération particulière se produise. Cela peut être sous la forme d’affichage d’une boîte de dialogue alerte ou demander un code secret. ACL vous permet de définir l’accessibilité et l’authentification pour un élément de trousseau. Le diagramme suivant montre comment ce nouvel attribut lie avec le reste de l’élément de la chaîne de clé :

[![](touchid-images/image2.png "Ce diagramme illustre la façon dont ce nouvel attribut lie avec le reste de l’élément de trousseau")](touchid-images/image2.png)

À partir d’iOS 8, il existe désormais une nouvelle stratégie de présence de l’utilisateur, `SecAccessControl`, qui est appliqué par l’enclave sécurisé sur un iPhone 5 s et versions ultérieures. Nous pouvons le voir dans le tableau ci-dessous, simplement comment la configuration de l’appareil peut influencer l’évaluation de stratégie :

<table width="100%" border="1px">
<thead>
<tr>
    <td>Configuration de l’appareil</td>
    <td>Évaluation de la stratégie</td>
    <td>Mécanisme de sauvegarde</td>
</tr>
</thead>
<tbody>
<tr>
    <td>Appareil sans code secret</td>
    <td>Aucun accès</td>
    <td>Aucun.</td>
</tr>
<tr>
    <td>Appareil avec code secret</td>
    <td>Requiert du code d’accès</td>
    <td>Aucun.</td>
</tr>
<tr>
    <td>Appareil avec l’ID de contact</td>
    <td>S’il préfère que Touch ID</td>
    <td>Permet à code d’accès</td>
</tr>
</tbody>
</table>

Toutes les opérations à l’intérieur de l’Enclave sécurisé peuvent s’approuvent mutuellement. Cela signifie que nous pouvons utiliser le résultat de l’authentification Touch ID pour autoriser le déchiffrement d’élément de trousseau. L’Enclave Secure conserve également un compteur des correspondances infructueuses de Touch ID, auquel cas un utilisateur sera a revenir à l’aide du code secret.
Une nouvelle infrastructure dans iOS 8, appelée _l’authentification locale_, prend en charge ce processus d’authentification au sein de l’appareil. Nous allons étudier cela dans la section suivante.

## <a name="local-authentication"></a>Authentification locale

Nous avons établi dans la section précédente, les applications peuvent utiliser l’authentification locale pour authentifier l’utilisateur dans le respect de la stratégie de sécurité qui a été définie sur l’appareil.

Actuellement, l’API fournit uniquement deux fonctions : tout d’abord, il facilite les services existants de trousseau via l’utilisation de la nouvelle Keychain Access Control Lists (ACL). Les données de chaîne de clé peuvent être déverrouillées à l’aide de l’authentification réussie d’une empreinte digitale utilisateurs.

Deuxièmement, LocalAuthentication fournit deux méthodes pour authentifier votre application localement. Les développeurs doivent utiliser `CanEvaluatePolicy` pour déterminer si l’appareil est capable d’accepter Touch ID, puis `EvaluatePolicy` pour démarrer l’opération d’authentification.

Les deux fonctionnalités offrent par l’authentification locale, mais elles ne pas fournir un mécanisme pour l’application ou l’utilisateur pour s’authentifier auprès d’un serveur distant.
L’authentification locale fournit une nouvelle interface utilisateur standard pour l’authentification. Dans le cas Touch ID, il s’agit d’un affichage des alertes avec les deux boutons, comme illustré ci-dessous. Un bouton Annuler, l’application à utiliser les moyens de secours de l’authentification : le code secret. Il existe également un message personnalisé qui doit être défini. Il est conseillé d’utiliser ceci pour expliquer à l’utilisateur pourquoi l’authentification Touch ID est requise.

[![](touchid-images/image12.png "L’alerte d’authentification Touch ID")](touchid-images/image12.png)

### <a name="with-keychain-services"></a>Avec les Services de trousseau

Nous avons un peu plus tôt à la façon dont un élément de trousseau est déchiffré, à l’aide de l’enclave sécurisée pour vérifier notre code secret. Dans iOS 8, nous pouvons utiliser l’authentification locale demander la vérification Touch ID conjointement avec la fonctionnalité de listes de contrôle d’accès, qui fournit l’implémentation de la méthode de secours, ou le mot de passe.
Pour utiliser des ACL que nous devons utiliser la `SecAccessControl` stratégie et en activant l’état de l’appareil en utilisant `SecAccessible.WhenPasscodeSetThisDeviceOnly` ou `SecAccessible.WhenUnlocked`.

#### <a name="considerations-with-acl"></a>Considérations relatives à la liste ACL

Il existe de nombreuses choses que nous devons prendre en compte lors de l’utilisation de la liste ACL avec la chaîne de clé et certains d'entre eux sont répertoriées ci-dessous :

-   Utiliser uniquement avec l’Application de premier plan, si vous appelez des opérations de trousseau sur un thread d’arrière-plan que l’appel échoue.
-   Ajout et la mise à jour des éléments de la chaîne de clé peuvent nécessiter d’authentification.
-   Si une requête retourne plusieurs éléments correspondants dans le trousseau, authentification peut être requise.
-   ACL des éléments protégés sont périphérique uniquement et par conséquent pas synchronisée ou sauvegardée.

### <a name="using-local-authentication-without-keychain-services"></a>À l’aide de l’authentification locale sans les Services de trousseau

L’authentification locale a été créée comme un moyen pour collecter des informations d’identification, telles que le code secret ou Touch ID et pour travailler avec l’Enclave sécurisé à la fin de l’authentification de l’utilisateur. Elle sert de pont entre votre application et l’Enclave sécurisé, qui ne peuvent jamais directement communiquer entre eux. Il peut également être utilisé pour l’évaluation de stratégie pour votre application.

Pour cela, une application appelle l’évaluation de stratégie à l’intérieur de l’authentification locale, qui démarre l’opération à l’intérieur de sécuriser les Enclave. Vous pouvez utiliser cette option pour fournir une authentification à votre application, sans interrogation/accéder directement l’Enclave sécurisé.

[![](touchid-images/image13a.png "À l’aide de l’authentification locale sans les Services de trousseau")](touchid-images/image13a.png)

À l’aide de l’authentification locale dans votre application fournit un moyen simple d’implémentation de la vérification de l’utilisateur, par exemple pour déverrouiller une fonctionnalité uniquement pour les yeux du propriétaire du périphérique, comme les applications bancaires, ou pour le contrôle parental aide pour la personne application. Vous pouvez également l’utiliser comme un moyen d’étendre l’authentification qui existe déjà : les utilisateurs que leurs informations pour être sécurisés, mais ils le souhaitent également avoir le choix.

La sécurité de l’authentification locale diffère de celui de la chaîne de clé. Par exemple, lorsque vous utilisez la chaîne de clé, l’approbation est entre le système d’exploitation et l’Enclave sécurisé. Avec l’authentification locale, il est entre l’application et le système d’exploitation, ce qui signifie que vous avez uniquement accès aux résultats de l’Enclave sécuriser, pas les sécuriser Enclave lui-même.

Sur l’objet de sécurité, il est extrêmement important de savoir que **aucun accès** à doigts inscrits ou des images d’empreintes digitales. L’Enclave sécurisé est le propriétaire de ces informations, et par conséquent, aucun autre composant du système n’a accès.

Pour utiliser Touch ID sans trousseau en tirant parti de l’API d’authentification Local, il existe certaines fonctions que nous pouvons utiliser. Ceux-ci sont décrits ci-dessous :

*   `CanEvaluatePolicy` – Cette simplement vérifie si l’appareil est capable d’accepter un ID tactile.
*   `EvaluatePolicy` -Il démarre l’opération d’authentification et affiche l’interface utilisateur et retourne un `true` ou `false` réponse.
*   `DeviceOwnerAuthenticationWithBiometrics` – Il s’agit de la stratégie qui peut être utilisée pour afficher l’écran ID tactile. Il est important de noter qu’ici aucun mécanisme de secours code secret, au lieu de cela, vous devez implémenter ce secours dans votre application pour permettre aux utilisateurs d’ignorer l’authentification ID tactile.

Il existe quelques avertissements à l’aide de l’authentification locale, qui sont répertoriées ci-dessous :

*   Comme avec trousseau, il peut uniquement être exécuté au premier plan. Appeler sur un thread d’arrière-plan provoquer son échec.
*   Gardez à l’esprit que l’évaluation de stratégie peut échouer. Un bouton de code secret doit être implémentée comme un repli.
*   Vous devez fournir un `localizedReason` pour expliquer pourquoi l’authentification est nécessaire. Cela permet d’établir la confiance à l’utilisateur.

Ensuite, dans la section ci-dessous, nous allons examiner comment implémenter l’API tenant ces avertissements.

## <a name="adding-touch-id-to-your-application"></a>Ajout de Touch ID à votre application

Dans les sections précédentes, nous avons étudié la théorie des accès et l’authentification à l’aide de la chaîne de clé et l’authentification locale. Nous allons maintenant examiner comment vous pouvez intégrer des ID tactile dans votre application.

### <a name="walkthrough"></a>Procédure pas à pas

Nous allons donc étudier d’ajouter certains authentification ID tactile à notre application. Dans cette procédure pas à pas nous allons utiliser la [Table de la table de montage séquentiel](https://developer.xamarin.com/samples/StoryboardTable/) exemple. Vous souhaitez vous assurer que seuls le propriétaire de l’appareil peut ajouter un élément à cette liste, nous ne voulons encombrer par toute personne permettant d’ajouter un élément !

1.  Télécharger l’exemple et l’exécuter dans Visual Studio pour Mac.
2.  Double-cliquez sur `MainStoryboard.Storyboard` pour ouvrir l’exemple dans le concepteur iOS. Avec cet exemple, nous souhaitons ajouter un nouvel écran à notre application, qui contrôle l’authentification. Cela passe précède la `MasterViewController`.
3.  Faites glisser une nouvelle **View Controller** à partir de la **boîte à outils** à la **aire de conception**. Définir en tant que le **racine View Controller** par **Ctrl + glisser** à partir de la **Navigation contrôleur**:

    [![](touchid-images/image4.png "Définissez le contrôleur d’affichage racine")](touchid-images/image4.png)
4.  Nommez le nouveau contrôleur de vue `AuthenticationViewController`.
5.  Ensuite, faites glisser un bouton et placez-le sur le `AuthenticationViewController`. Appelez cela `AuthenticateButton`et lui donner le texte `Add a Chore`.
6.  Créer un événement sur le `AuthenticateButton` appelée `AuthenticateMe`.
7.  Créer un manuel segue à partir de `AuthenticationViewController` en cliquant sur la barre noire au bas et **Ctrl + glisser** dans la barre d’outils pour le `MasterViewController` et en choisissant **push** (ou **afficher** Si vous utilisez les classes de taille) :

    [![](touchid-images/image5.png "Faites glisser la barre vers le MasterViewController et en choisissant push ou afficher")](touchid-images/image6.png)
8.  Cliquez sur les récents segue et lui donner l’identificateur `AuthenticationSegue`, comme illustré ci-dessous :

    [![](touchid-images/image7.png "La valeur de l’identificateur segue AuthenticationSegue")](touchid-images/image7.png)
9.  Ajoutez le code suivant à `AuthenticationViewController` :

    ```
    partial void AuthenticateMe (UIButton sender)
        {
            var context = new LAContext();
            NSError AuthError;
            var myReason = new NSString("To add a new chore");


            if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError)){
                var replyHandler = new LAContextReplyHandler((success, error) => {

                    this.InvokeOnMainThread(()=>{
                        if(success){
                            Console.WriteLine("You logged in!");
                            PerformSegue("AuthenticationSegue", this);
                        }
                        else{
                            //Show fallback mechanism here
                        }
                    });

                });
                context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, myReason, replyHandler);
            };
        }
    ```

Il s’agit de tout le code que vous avez besoin pour implémenter l’authentification de Touch ID à l’aide de l’authentification locale. Les lignes en surbrillance dans l’image ci-dessous illustrent l’utilisation de l’authentification locale :

[![](touchid-images/image8.png "Les lignes en surbrillance illustrent l’utilisation de l’authentification locale")](touchid-images/image8.png)

Tout d’abord, nous devons spécifier si l’appareil est capable d’accepter Touch ID d’entrée, à l’aide de la `CanEvaluatePolicy` et en passant la stratégie `DeviceOwnerAuthenticationWithBiometrics`. Si cela est vrai, que nous pouvons afficher l’interface utilisateur de ID tactile à l’aide de `EvaluatePolicy`. Il existe trois éléments d’information que nous devez transmettre dans `EvaluatePolicy` – la stratégie proprement dite, une chaîne expliquant pourquoi l’authentification est nécessaire et un gestionnaire de réponse. Le Gestionnaire de réponse indique à l’application de ce qu’il doit faire en cas d’une authentification réussie ou non. Examinons de plus près le Gestionnaire de réponse :

[![](touchid-images/image9.png "Le Gestionnaire de réponse")](touchid-images/image9.png)

Le Gestionnaire de réponse est spécifié de type `LAContextReplyHandler`, qui accepte le succès des paramètres – un `bool` valeur et un `NSError` appelée `error`. Si elle réussit, voici où nous allons effectuer réellement tout ce qui se que nous voulons authentifier – dans ce cas afficher l’écran qui nous ajoute une nouvelle tâche. N’oubliez pas parmi les caractéristiques de l’authentification locale est qu’il doit être exécuté sur le premier plan, assurez-vous d’utiliser `InvokeOnMainThread`:

[![](touchid-images/image10.png "Utilisez InvokeOnMainThread pour l’authentification locale")](touchid-images/image10.png)

Enfin, lorsque l’authentification a réussi, nous voulons transition vers le `MasterViewController`. Le `PerformSegue` méthode peut être utilisée pour ce faire :

[![](touchid-images/image11.png "Appelez la méthode PerformSegue à passer à la MasterViewController")](touchid-images/image11.png)

## <a name="summary"></a>Récapitulatif
Dans ce guide, nous avons étudié trousseau et comment cela fonctionne dans iOS. Nous avons exploré également le trousseau ACL et des modifications apportées à ce iOS. Ensuite, nous avons pris examiner l’infrastructure de l’authentification locale, qui est une nouveauté dans iOS 8 et effectue la recherche à l’implémentation d’une authentification Touch ID dans notre application.

## <a name="related-links"></a>Liens associés

- [Exemple d’ID tactile](https://developer.xamarin.com/samples/StoryboardTable_LocalAuthentication)
- [Exemple de trousseau WWDC](https://developer.xamarin.com/samples/KeychainTouchID/)
- [Keychain (sample)](https://developer.xamarin.com/samples/Keychain/)
