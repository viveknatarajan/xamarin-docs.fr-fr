---
title: "Pourquoi ne peut pas ma version Release Android se connecter à Internet ?"
ms.topic: article
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 60da469001f8a9641aff7e014add5f58bdb389cb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Pourquoi ne peut pas ma version Release Android se connecter à Internet ?

## <a name="cause"></a>Cause

La cause la plus courante de ce problème est que le **INTERNET** autorisation est automatiquement incluse dans une version debug, mais doivent être définie manuellement pour une version release. Il s’agit, car l’autorisation Internet est utilisée pour autoriser un débogueur à attacher au processus, comme décrit pour « DebugSymbols » [ici](~/android/deploy-test/building-apps/build-process.md).


## <a name="fix"></a>Correctif

Pour résoudre ce problème, vous pouvez exiger l’autorisation Internet dans le manifeste Android. Cela est possible par le biais de l’éditeur de manifeste ou sourcecode du manifeste de :

-   Résoudre dans l’éditeur : dans votre projet Android, accédez à **Propriétés -> AndroidManifest.xml -> autorisations requises** et vérifiez **Internet**

-   Corrigez dans Sourcecode : ouvrir le AndroidManifest dans un éditeur de code source et ajoutez la balise de l’autorisation à l’intérieur de la `<Manifest>` balises :

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
