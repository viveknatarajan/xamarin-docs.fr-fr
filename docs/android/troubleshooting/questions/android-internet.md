---
title: Pourquoi la build de ma version Android ne peut pas se connecter à Internet ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: cd27d5c884086cd0fade4364851039fd0cd915a0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60945459"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Pourquoi la build de ma version Android ne peut pas se connecter à Internet ?

## <a name="cause"></a>Cause

La cause la plus courante de ce problème est que le **INTERNET** autorisation est automatiquement incluse dans une version debug, mais doit être définie manuellement pour une version Release. Il s’agit, car l’autorisation Internet est utilisée pour autoriser un débogueur à attacher au processus, comme décrit pour « DebugSymbols » [ici](~/android/deploy-test/building-apps/build-process.md).


## <a name="fix"></a>Corriger

Pour résoudre ce problème, vous pouvez demander l’autorisation d’Internet dans le manifeste Android. Cela est possible par le biais de l’éditeur de manifeste ou de code source du manifeste :

-   Corriger dans l’éditeur : Dans votre projet Android, accédez à **Propriétés -> AndroidManifest.xml -> autorisations requises** et vérifiez **Internet**

-   Corriger dans le code source : Ouvrez le AndroidManifest dans un éditeur de source et ajoutez la balise de l’autorisation à l’intérieur de la `<Manifest>` balises :

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
