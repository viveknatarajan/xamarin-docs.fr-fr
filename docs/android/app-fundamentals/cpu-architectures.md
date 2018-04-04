---
title: Architectures de processeur
description: Xamarin.Android prend en charge plusieurs architectures d’UC, y compris les appareils 32 bits et 64 bits. Cet article explique comment cibler une application sur un ou plusieurs des architectures pris en charge par Android de l’UC.
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: dea5aaa16891893f649d5ec56f3e6b1ee9a18683
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="cpu-architectures"></a>Architectures de processeur

_Xamarin.Android prend en charge plusieurs architectures d’UC, y compris les appareils 32 bits et 64 bits. Cet article explique comment cibler une application sur un ou plusieurs des architectures pris en charge par Android de l’UC._

## <a name="cpu-architectures-overview"></a>Vue d’ensemble des Architectures de processeur

Lorsque vous préparez votre application pour cette version, vous devez spécifier les architectures d’UC de la plateforme votre application prend en charge. Un même APK peut contenir du code machine permettant la prise en charge de plusieurs architectures différentes. Chaque collection de code spécifique à l’architecture est associée un *Interface binaire d’Application* (ABI). Chaque ABI définit la façon dont ce code machine est censé interagir avec Android au moment de l’exécution.
Pour plus d’informations sur le fonctionnement, consultez [multicœur appareils &amp; Xamarin.Android](~/android/deploy-test/multicore-devices.md).


## <a name="how-to-specify-supported-architectures"></a>Comment spécifier des Architectures prises en charge

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

En règle générale, vous sélectionnez explicitement une architecture (ou architectures) lorsque votre application est configurée pour **version**. Lorsque votre application est configurée pour **déboguer**, le **utiliser le Runtime partagé** et **utiliser le déploiement rapide** options sont activées, lequel désactiver la sélection de l’architecture explicite.

Dans Visual Studio, double-cliquez sur **propriétés** sous votre projet dans **l’Explorateur de solutions** et sélectionnez le **Options Android** page. Cliquez sur le **empaquetage** onglet et vérifiez que **utiliser le Runtime partagé** est désactivée (désactivation de cette option vous permet de sélectionner explicitement les ABIs pour prendre en charge). Cliquez sur le **avancé** onglet et, sous **propriétés avancées**, vérifiez les architectures que vous souhaitez prendre en charge :

[![Sélection armeabi et armeabi-v7a](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

En règle générale, vous sélectionnez explicitement une architecture (ou architectures) lorsque votre application est configurée pour **version**. Lorsque votre application est configurée pour **déboguer**, le **partagés runtime Mono** et **rapide du déploiement de l’assembly** options sont activées, ce qui empêche l’architecture explicite sélection.

Dans Visual Studio pour Mac, localisez votre projet dans le **Solution** de remplissage, cliquez sur l’icône d’engrenage en regard de votre projet et sélectionnez **Options**. Dans le **Options du projet** boîte de dialogue, cliquez sur **générer Android**. Cliquez sur le **général** onglet et vérifiez que **partagés runtime Mono** est désactivée (désactivation de cette option vous permet de sélectionner explicitement les ABIs pour prendre en charge). Cliquez sur le **avancé** onglet et, sous **ABIs de prise en charge**, vérifiez les ABIs pour les architectures que vous souhaitez prendre en charge :

[![Sélection armeabi et armeabi-v7a](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png#lightbox)

-----


Xamarin.Android prend en charge les architectures suivantes :

-   **armeabi** &ndash; ARM-unités centrales qui prennent en charge au moins le jeu d’instructions ARMv5TE. Notez que `armeabi` n’est pas thread-safe et ne doit pas être utilisée sur les appareils multiprocesseur.

-   **armeabi-v7a** &ndash; unités centrales ARM avec les opérations à virgule flottante matérielle et de plusieurs unités de processeur (SMP). Notez que `armeabi-v7a` code machine ne fonctionnera pas sur les appareils ARMv5.

-   **arm64-v8a** &ndash; unités centrales, selon l’architecture de ARMv8 64 bits.

-   **x86** &ndash; processeurs qui prennent en charge la x86 (ou IA-32) jeu d’instructions. Ce jeu d’instructions est équivalent à celle de Pentium Pro, y compris les instructions MMX, SSE, SSE2 et SSE3.

-   **x86_64** processeurs qui prennent en charge la x86 64 bits (également appelé *x64* et *AMD64*) jeu d’instructions.

Par défaut est Xamarin.Android `armeabi-v7a` pour **version** génère. Ce paramètre offre de bien meilleures performances que `armeabi`. Si vous ciblez une plateforme ARM de 64 bits (par exemple, le 9 Nexus), sélectionnez `arm64-v8a`. Si vous déployez votre application à x86 appareil, sélectionnez `x86`. Si l’appareil cible x86 utilise une architecture de processeur 64 bits, sélectionnez `x86_64`.

## <a name="targeting-multiple-platforms"></a>Ciblage de plateformes multiples

Pour cibler plusieurs architectures d’UC, vous pouvez sélectionner plusieurs ABI (au détriment de la taille de fichier APK). Vous pouvez utiliser la **générer un package (.apk) par ABI sélectionné** option (décrit dans [propriétés du jeu d’empaquetage](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)) pour créer un fichier APK distinct pour chaque architecture prise en charge.

Vous n’êtes pas obligé de sélectionner **arm64-v8a** ou **x86_64** pour cibler des périphériques 64 bits ; prise en charge 64 bits n’est pas nécessaire pour exécuter votre application sur un matériel 64 bits. Par exemple, les appareils ARM 64 bits (tels que les [Nexus 9](http://www.google.com/nexus/9/)) peuvent exécuter des applications configurées pour `armeabi-v7a`. Le principal avantage de permettre la prise en charge 64 bits est de permettre à votre application d’adresser plus de mémoire.

> [!NOTE]
> fonctionnalité expérimentale est actuellement prise en charge du runtime 64 bits. N’oubliez pas que les runtimes 64 bits sont *pas* requis pour exécuter votre application sur des périphériques 64 bits. 

## <a name="additional-information"></a>Informations supplémentaires

Dans certaines situations, vous devrez peut-être créer un APK distinct pour chaque architecture (afin de réduire la taille de votre fichier APK, ou parce que votre application a des bibliothèques partagées qui sont spécifiques à une architecture d’UC spécifique).
Pour plus d’informations sur cette approche, consultez [APKs de ABI spécifique Build](~/android/deploy-test/building-apps/abi-specific-apks.md).
