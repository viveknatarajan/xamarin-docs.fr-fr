---
title: Résolution des problèmes
description: Problèmes connus avec Xamarin Player Live et comment les résoudre.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: 147ce43d3fe764f71f27dce46b699142dfb99872
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="troubleshooting"></a>Résolution des problèmes

![Fonctionnalité d’aperçu](~/media/shared/preview.png)

Cet article décrit certains problèmes courants et fournit les étapes pour les corriger.


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Appareil mobile ne se connecte pas après analyse code-barres (ou une entrée de code)

Se produit lorsque l’appareil mobile en cours d’exécution Xamarin Live Player n’est pas sur le même réseau que l’ordinateur qui exécute l’IDE. Passez en revue les éléments suivants :

- Vérifiez que l’appareil et l’ordinateur sont sur le même réseau Wi-Fi.
  - Si l’ordinateur est également connecté à un réseau câblé, essayez de le débrancher la connexion câblée.
- Le réseau peut être correctement sécurisé (par exemple, certains réseaux d’entreprise), bloquant les ports requis par le lecteur Xamarin Live.
- Fermez l’application Xamarin Player de Live et redémarrez-le.


## <a name="error-while-trying-to-deploy-message-in-ide"></a>Message « Erreur lors de la tentative de déploiement » dans l’IDE

**« IOException : Impossible de lire les données à partir de la connexion de transport : susceptibles de bloquer l’opération sur un socket non bloquant »**

Cette erreur se produite souvent lorsque le périphérique mobile en cours d’exécution Xamarin Live Player ne se trouve pas sur le même réseau que l’ordinateur exécutant Visual Studio ; Cela se produit souvent lorsque vous vous connectez à un appareil qui a été associé précédemment avec succès.

* Vérifiez que l’appareil et l’ordinateur sont sur le même réseau Wi-Fi.
* Le réseau peut être correctement sécurisé (par exemple, certains réseaux d’entreprise), bloquant les ports requis par le lecteur Xamarin Live. Les ports suivants sont requis pour le lecteur de Live Xamarin :
  * 37847 – accès réseau interne 
  * 8090 – accès au réseau externe

## <a name="manually-configure-device"></a>Configurer manuellement le périphérique

Si vous ne pouvez pas connecter à votre appareil via le Wi-Fi. vous pouvez tenter de configurer manuellement votre appareil via le fichier de configuration, en procédant comme suit :

**Étape 1 : Ouvrez le fichier de configuration**

Accédez à votre dossier de données d’application :

* Windows : **%userprofile%\AppData\Roaming**
* macOS : **~/Users/$USER/.config**

Dans ce dossier, vous trouverez **PlayerDeviceList.xml** s’il n’existe pas, vous devrez créer un.

**Étape 2 : Obtenir l’adresse IP**

Dans l’application Xamarin Live Player, accédez à **sur > Test de connexion > Démarrer le Test connexion**.

Prenez note de l’adresse IP, vous devez l’adresse IP répertoriée quand vous configurez votre appareil.

**Étape 3 : Obtenir le code de jumelage**

À l’intérieur du drainage Xamarin Live Player **paire** ou **paire nouveau**, puis appuyez sur **entrez manuellement**. Un code numérique s’affichera, que vous devez mettre à jour le fichier de configuration.

**Étape 4 : Générer des GUID**

Accédez à : https://www.guidgenerator.com/online-guid-generator.aspx et générer un nouveau guid et assurez-vous majuscules sur.


**Étape 5 : Configurer le périphérique**

Ouvrez le **PlayerDeviceList.xml** vers le haut dans un éditeur tel que Visual Studio ou Visual Studio Code. Vous devez configurer votre appareil manuellement dans ce fichier. Par défaut, le fichier doit contenir le vide suivant `Devices` élément XML :

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Ajouter un appareil iOS :**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>iPhone Player</Name>
<Platform>iOS</Platform>
<AndroidApiLevel>0</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:36:03.9492291Z</LastConnectTimeUtc>
</PlayerDevice>
```


**Ajoutez un appareil Android :**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>Android Player</Name>
<Platform>Android</Platform>
<AndroidApiLevel>24</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:34:42.2332328Z</LastConnectTimeUtc>
</PlayerDevice>
```

**Fermez et rouvrez Visual Studio.** Votre appareil doit s’afficher dans la liste.


## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>Message de « espace de noms ou le type est introuvable » dans l’IDE

Vérifiez que vous avez sélectionné un **projet de démarrage** qui correspond à votre type d’appareil (iOS ou Android) et que la configuration correspond à ce type de périphérique (par exemple). **Déboguer | iPhone simulateur** pour iOS).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>Message « Constructeur sur le type 'InterpretedXamarin.Forms.Button' non trouvé » dans le lecteur

Certaines classes système ne peut pas être substituées, par exemple :

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>« MainActivity.cs : 'Resource.Layout' ne contient pas de définition pour 'Main' »

Cette erreur se produit pour les projets Android avec des interfaces utilisateur définis dans les fichiers AXML.
Les fichiers AXML ne sont pas pris en charge actuellement dans le lecteur Xamarin Live.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Barre d’outils et des onglets Android restituent correctement à l’aide de Xamarin.Forms

Projets de Xamarin.Forms Android doivent utiliser « Toolbar.axml » et « Tabbar.axml » pour les noms des fichiers de mise en page pertinentes. Le modèle par défaut utilise ces noms ; renommant entraîne des problèmes de rendu.


Veuillez signaler d’autres problèmes sur [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Liens associés

- [Limitations](~/tools/live-player/limitations.md)
- [Installation](~/tools/live-player/install.md)
