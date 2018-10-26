---
title: Core NFC dans Xamarin.iOS
description: Ce document décrit comment lire près de balises de communication de champ dans Xamarin.iOS à l’aide de l’API introduits dans iOS 11.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: lobrien
ms.author: laobri
ms.date: 09/25/2017
ms.openlocfilehash: 6888f7147796d3c00752d10387c19d0d9f269cad
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106817"
---
# <a name="core-nfc-in-xamarinios"></a>Core NFC dans Xamarin.iOS

_Balises de lecture Communication NFC (Near Field) à l’aide d’iOS 11_

CoreNFC est une nouvelle infrastructure dans iOS 11 qui fournit l’accès à la _Communication en champ proche_ radio (NFC) pour lire des balises à partir d’applications. Elle fonctionne sur iPhone 7, Plus de 7, 8, 8 Plus et X.

Le lecteur de balise NFC sur les appareils iOS prend en charge tous les types de balises NFC 1 à 5 qui contiennent _Format d’échange de données NFC_ plus d’informations (NDEF).

Il existe certaines restrictions à connaître :

- CoreNFC prend uniquement en charge (pas l’écriture ou la mise en forme) de lecture d’étiquette.
- Analyses de balise doivent être initiée par l’utilisateur et le délai d’expiration après 60 secondes.
- Les applications doivent être visibles au premier plan pour l’analyse.
- CoreNFC peut uniquement être testée sur des appareils réels (et non sur le simulateur).

Cette page décrit la configuration requise pour utiliser CoreNFC et montre comment utiliser l’API en utilisant le [« NFCTagReader » exemple de code](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/).

## <a name="configuration"></a>Configuration

Pour activer CoreNFC, vous devez configurer trois éléments dans votre projet :

- Un **Info.plist** clé de confidentialité.
- Un **Entitlements.plist** entrée.
- Un profil de provisionnement avec **la lecture des balises NFC** fonctionnalité.

### <a name="infoplist"></a>Info.plist

Ajouter le **NFCReaderUsageDescription** clé de confidentialité et du texte, ce qui est affiché pour l’utilisateur pendant que l’analyse est en cours. Utiliser un message approprié pour votre application (par exemple, expliquer l’objectif de l’analyse) :

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

Votre application doit demander le **près de champ Communications la lecture des balises** coupler de fonctionnalité à l’aide de la clé/valeur suivante dans votre **Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>Profil de provisionnement

Créer un nouveau **ID d’application** et vérifiez que le **la lecture des balises NFC** service est coché :

[![Page de portail nouvel ID d’application de développeur avec la lecture des balises NFC sélectionné](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

Vous devez ensuite créer un nouveau profil de provisionnement pour cet ID d’application, puis téléchargez et installez-le sur votre développement Mac.

## <a name="reading-a-tag"></a>Lecture d’une balise

Une fois que votre projet est configuré, ajoutez `using CoreNFC;` vers le haut du fichier et suivez ces trois étapes pour implémenter NFC baliser les fonctionnalités de lecture :

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. Implémentez `INFCNdefReaderSessionDelegate`

L’interface comporte deux méthodes à implémenter :

- `DidDetect` – Appelée lorsqu’une balise est lu avec succès.
- `DidInvalidate` : Appelée quand une erreur se produit ou le délai d’attente deuxième 60 est atteinte.

#### <a name="diddetect"></a>DidDetect

Dans l’exemple de code, chaque message analysé est ajouté à une vue de table :

```csharp
public void DidDetect(NFCNdefReaderSession session, NFCNdefMessage[] messages)
{
    foreach (NFCNdefMessage msg in messages)
    {  // adds the messages to a list view
        DetectedMessages.Add(msg);
    }
    DispatchQueue.MainQueue.DispatchAsync(() =>
    {
        this.TableView.ReloadData();
    });
}
```

Cette méthode peut être appelée plusieurs fois (et vous pouvez transmettre un tableau de messages) si la session autorise plusieurs lectures de balise. Ce paramètre est défini à l’aide de la troisième paramètre de la `Start` (méthode) (expliqué dans [étape 2](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

Invalidation peut se produire pour plusieurs raisons :

- Une erreur s’est produite lors de l’analyse.
- L’application a cessé d’être au premier plan.
- L’utilisateur a choisi d’annuler l’analyse.
- L’analyse a été annulée par l’application.

Le code ci-dessous montre comment gérer une erreur :

```csharp
public void DidInvalidate(NFCNdefReaderSession session, NSError error)
{
    var readerError = (NFCReaderError)(long)error.Code;
    if (readerError != NFCReaderError.ReaderSessionInvalidationErrorFirstNDEFTagRead &&
        readerError != NFCReaderError.ReaderSessionInvalidationErrorUserCanceled)
    {
      // some error handling
    }
}
```

Une fois qu’une session a été invalidée, un nouvel objet session doit être créé pour relancer l’analyse.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. Démarrer un `NFCNdefReaderSession`

L’analyse doit commencer par une demande de l’utilisateur, comme un sur le bouton.
Le code suivant crée et démarre une session d’analyse :

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

Les paramètres pour le `NFCNdefReaderSession` constructeur sont les suivantes :

- `delegate` – Une implémentation de `INFCNdefReaderSessionDelegate`. Dans l’exemple de code, le délégué est implémenté dans le contrôleur d’affichage table, par conséquent `this` est utilisé comme paramètre délégué.
- `queue` – La file d’attente que les rappels sont gérés sur. Il peut être `null`, auquel cas vous devez utiliser le `DispatchQueue.MainQueue` lors de la mise à jour des contrôles d’interface utilisateur (comme indiqué dans l’exemple).
- `invalidateAfterFirstRead` – Lorsque `true`, l’analyse s’arrête après la première analyse réussie ; lorsque `false` analyse continue et plusieurs résultats retournés jusqu'à ce que l’analyse est annulée ou que le délai d’attente deuxième 60 soit atteint.


### <a name="3-cancel-the-scanning-session"></a>3. Annuler la session d’analyse

L’utilisateur peut annuler la session d’analyse via un bouton fournie par le système dans l’interface utilisateur :

![Bouton Annuler lors de l’analyse](corenfc-images/scan-cancel-sml.png)

L’application peut annuler par programmation de l’analyse en appelant le `InvalidateSession` méthode :

```csharp
Session.InvalidateSession();
```

Dans des deux cas, le délégué `DidInvalidate` méthode sera appelée.

## <a name="summary"></a>Récapitulatif

CoreNFC permet à votre application lire des données à partir de balises NFC. Il prend en charge la lecture d’une variété de formats de balise (types NDEF 1 à 5), mais ne prend pas en charge l’écriture ou de mise en forme.


## <a name="related-links"></a>Liens associés

- [NFCTagReader (exemple)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [Présentation de Core NFC (WWDC) (vidéo)](https://developer.apple.com/videos/play/wwdc2017/718/)
