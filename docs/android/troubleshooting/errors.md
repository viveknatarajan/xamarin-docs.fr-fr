---
title: Matrice des erreurs Xamarin.Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c3aab44fcb0522b762aaa101de0aeba08016b641
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-errors-matrix"></a>Matrice des erreurs Xamarin.Android

## <a name="errors-reference"></a>Référence des erreurs

Ce document fournit des informations sur les différents codes d’erreur à partir de Xamarin.

<table>
    <thead>
        <tr>
            <td>Category</td>
            <td>Description</td>
        </tr>
    </thead>
    <tbody>
        <tr><td>XA0xxx</td><td><code>mandroid</code> erreurs</td></tr>
        <tr><td>XA1xxx</td><td>Copie d’un fichier / liens symboliques (projet liée) erreurs</td></tr>
        <tr><td>XA2xxx</td><td>Erreurs de l'éditeur de liens</td></tr>
        <tr><td>XA3xxx</td><td>Erreurs d’AOA</td></tr>
        <tr><td>XA4xxx</td><td>Erreurs de génération de code</td></tr>
        <tr><td>XA5xxx</td><td>GCC et les erreurs de la chaîne d’outils</td></tr>
        <tr><td>XA6xxx</td><td><code>mandroid</code> Erreurs d’outils internes</td></tr>
        <tr><td>XA7xxx</td><td>Réservée</td></tr>
        <tr><td>XA8xxx</td><td>Réservée</td></tr>
        <tr><td>XA9xxx</td><td>Erreurs de licence</td></tr>
    </tbody>
</table>

## <a name="error-codes"></a>Codes d'erreur

### <a name="xa0xxx-errors"></a>Erreurs de XA0xxx

<table>
    <thead>
        <tr><td>Code d'erreur</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA0000</td><td>Erreur inattendue : Veuillez remplir un rapport de bogue à <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA0001</td><td>'-nom_unité a été fourni sans qu’aucune action spécifique au périphérique</td></tr>
        <tr><td>XA0002</td><td>N’a pas pu analyser la variable d’environnement '{0}'.</td></tr>
        <tr><td>XA0003</td><td>Application nom '.exe {0}' est en conflit avec un nom d’assembly (.dll) SDK ou du produit.</td></tr>
        <tr><td>XA0004</td><td>La nouvelle logique de refcounting nécessite sgen doit être activée en trop.</td></tr>
        <tr><td>XA0005</td><td>Le répertoire de sortie '{0}' n’existe pas</td></tr>
        <tr><td>XA0006</td><td>Il n’existe aucune plateforme de développement à '{0}', utilisez--platform = PLAT pour spécifier le Kit de développement</td></tr>
        <tr><td>XA0007</td><td>L’assembly racine '{0}' n’existe pas</td></tr>
        <tr><td>XA0008</td><td>Vous devez fournir un assembly de racine</td></tr>
        <tr><td>XA0009</td><td>Erreur lors du chargement des assemblys : {0}</td></tr>
        <tr><td>XA0010</td><td>Les arguments de ligne de commande n’a pas pu être analyser : {0}</td></tr>
        <tr><td>XA0011</td><td>{0} a été créée avec un runtime plus récent ({{1}) que MonoTouch prend en charge</td></tr>
        <tr><td>XA0012</td><td>Des données incomplètes sont fournies pour l’exécution `{0}`.</td></tr>
        <tr><td>XA0013</td><td>Prise en charge du profilage requiert sgen doit être activée en trop</td></tr>
        <tr><td>XA0014</td><td>{0} iOS ne prend pas en charge la génération d’applications ciblant ARMv6</td></tr>
        <tr><td>XA0020</td><td>N’a pas pu déterminer le chemin d’accès de mandroid.</td></tr>
        <tr><td>XA0100</td><td>EmbeddedNativeLibrary '{0}' n’est pas valide dans le projet d’Application Android. Utilisez plutôt AndroidNativeLibrary.</td></tr>
    </tbody>
</table>

### <a name="xa1xxx-errors"></a>XA1xxx Errors

<table>
    <thead>
        <tr><td>Code d'erreur</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA1001</td><td>Impossible de trouver une application dans le répertoire spécifié</td></tr>
        <tr><td>XA1002</td><td>Impossible de créer des liens symboliques, de fichiers ont été copiés.</td></tr>
        <tr><td>XA1003</td><td>Impossible d’interrompre l’application '{0}'. Vous devrez peut-être supprimer l’application manuellement.</td></tr>
        <tr><td>XA1004</td><td>Impossible d’obtenir la liste des applications installées.</td></tr>
        <tr><td>XA1005</td><td>Impossible d’interrompre l’application '{0}' sur l’appareil « {{1} » : {{2}. Vous devrez peut-être supprimer l’application manuellement.</td></tr>
        <tr><td>XA1006</td><td>Pas pu installer l’application '{0}' sur l’appareil « {{1} » : {{2}.</td></tr>
        <tr><td>XA1007</td><td>Échec du lancement de l’application '{0}' sur l’appareil « {{1} » : {{2}. Vous pouvez toujours lancer l’application manuellement en appuyant sur dessus.</td></tr>
        <tr><td>XA1008</td><td>Impossible de lancer le simulateur : {0}</td></tr>
        <tr><td>XA1009</td><td>Pas pu copier l’assembly '{0}' à '{1}' : {{2}</td></tr>
        <tr><td>XA1010</td><td>Impossible de charger l’assembly '{0}' : {1}</td></tr>
        <tr><td>XA1011</td><td>Impossible d’ajouter le fichier de ressources manquant : '{0}'</td></tr>
        <tr><td>XA1101</td><td>Application n’a pas pu être démarrer</td></tr>
        <tr><td>XA1102</td><td>N’a pas pu attacher à l’application (pour les supprimer) : {0}</td></tr>
        <tr><td>XA1103</td><td>Impossible de détacher</td></tr>
        <tr><td>XA1104</td><td>Impossible d’envoyer le paquet : {0}</td></tr>
        <tr><td>XA1105</td><td>Type de réponse inattendu</td></tr>
        <tr><td>XA1106</td><td>Impossible d’obtenir la liste des applications sur l’appareil : demande a expiré.</td></tr>
        <tr><td>XA1107</td><td>Échec du lancement de l’application</td></tr>
        <tr><td>XA1201</td><td>N’a pas pu charger le simulateur : {0}</td></tr>
        <tr><td>XA1301</td><td>La bibliothèque native `{0}` ({{1}) a été ignoré, car il ne correspond pas à l’architecture(s) de build actuel ({{2})</td></tr>
    </tbody>
</table>

### <a name="xa2xxx-errors"></a>XA2xxx Errors

<table>
    <thead>
        <tr><td>Code d'erreur</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA2001</td><td>Impossible de lier les assemblys</td></tr>
        <tr><td>XA2002</td><td>Impossible de résoudre la référence : {0}</td></tr>
        <tr><td>XA2003</td><td>Option '{0}' sera ignorée, car la liaison est désactivée.</td></tr>
        <tr><td>XA2004</td><td>Fichier de définitions de l’éditeur de liens supplémentaires '{0}' n’a pas pu être localisé.</td></tr>
        <tr><td>XA2005</td><td>Les définitions à partir de '{0}' n’a pas pu être analysées.</td></tr>
        <tr><td>XA2006</td><td>Référence à l’élément de métadonnées (défini dans '{1}') ' {0}' à partir de « {{2} » n’a pas pu être résolu.</td></tr>
    </tbody>
</table>

### <a name="xa3xxx-errors"></a>XA3xxx Errors

Voici les erreurs AOA.

<table>
    <thead>
        <tr><td>Code d'erreur</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA3001</td><td>Impossible de pas AOA l’assembly '{0}'</td></tr>
        <tr><td>XA3002</td><td>Restriction d’AOA : méthode '{0}' doit être statique, car elle est décorée avec [MonoPInvokeCallback].</td></tr>
        <tr><td>XA3003</td><td>Options de débogage et--llvm--conflictuelles. Soft-débogage est désactivé.</td></tr>
    </tbody>
</table>

### <a name="xa4xxx-errors"></a>XA4xxx Errors

Il s’agit des erreurs de génération de code.

<table>
    <thead>
        <tr><td>Code d'erreur</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA4001</td><td>Le modèle principal n’a pas pu être expansed à `{0}`.</td></tr>
        <tr><td>XA4101</td><td>Le bureau d’enregistrement ne peut pas générer une signature de type `{0}`.</td></tr>
        <tr><td>XA4102</td><td>Le bureau d’enregistrement de trouver un type non valide `{0}` dans la signature de méthode `{2}`. Utilisez plutôt `{1}`.</td></tr>
        <tr><td>XA4103</td><td>Le bureau d’enregistrement de trouver un type non valide `{0}` dans la signature de méthode `{2}`: le type implémente INativeObject, mais n’a pas de constructeur qui accepte deux (IntPtr, bool) arguments</td></tr>
        <tr><td>XA4104</td><td>Le bureau d’enregistrement ne peut pas marshaler de la valeur de retour de type `{0}` dans la signature de méthode `{1}`.</td></tr>
        <tr><td>XA4105</td><td>Le bureau d’enregistrement ne peut pas marshaler le paramètre de type `{0}` dans la signature de méthode `{1}`.</td></tr>
        <tr><td>XA4106</td><td>Le bureau d’enregistrement ne peut pas marshaler de la valeur de retour pour la structure `{0}` dans la signature de méthode `{1}`.</td></tr>
        <tr><td>XA4107</td><td>Le bureau d’enregistrement ne peut pas marshaler le paramètre de type `{0}` dans la signature de méthode `{1}`.</td></tr>
        <tr><td>XA4108</td><td>Le bureau d’enregistrement ne peut pas obtenir le type de ObjectiveC pour le type managé `{0}`.</td></tr>
        <tr><td>XA4109</td><td>Impossible de compiler le code généré de bureau d’enregistrement. Veuillez archiver un rapport de bogue à <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA4110</td><td>Le bureau d’enregistrement ne peut pas marshaler le paramètre out de type `{0}` dans la signature de méthode `{1}`.</td></tr>
        <tr><td>XA4111</td><td>Le bureau d’enregistrement ne peut pas générer une signature pour le type `{0}' in method `{{1} '.</td></tr>
        <tr><td>XA4200</td><td>Peut uniquement générer de ACW pour les types 'claas'.</td></tr>
        <tr><td>XA4201</td><td>Impossible de déterminer le nom JNI du type {0}.</td></tr>
        <tr><td>XA4203</td><td>Nom du type spécifié doit être qualifié complet.</td></tr>
        <tr><td>XA4204</td><td>Impossible de résoudre le type d’interface '{0}'. Vérifiez qu'il ne manque aucune référence d'assembly.</td></tr>
        <tr><td>XA4205</td><td>[ExportField] peut uniquement être utilisé sur les méthodes avec 0 paramètre.</td></tr>
        <tr><td>XA4206</td><td>[Exporter] ne peut pas être utilisé sur un type générique.</td></tr>
        <tr><td>XA4207</td><td>[ExportField] ne peut pas être utilisé sur un type générique.</td></tr>
        <tr><td>XA4208</td><td>[Java.Interop.ExportFieldAttribute] ne peut pas être utilisé sur une méthode retournant void.</td></tr>
        <tr><td>XA4209</td><td>Impossible de créer JavaTypeInfo pour la classe : {0} en raison de {{1}</td></tr>
        <tr><td>XA4210</td><td>Vous devez ajouter une référence à Mono.Android.Export.dll lorsque vous utilisez ExportAttribute ou ExportFieldAttribute.</td></tr>
        <tr><td>XA4211</td><td>AndroidManifest.xml //uses-sdk/@android:targetSdkVersion '{0}' est inférieure à $(TargetFrameworkVersion) « {{1} ». À l’aide de API-\ {1\\} pour la compilation ACW.</td></tr>
    </tbody>
</table>

### <a name="xa5xxx-errors"></a>XA5xxx Errors

<table>
    <thead>
        <tr><td>Code d'erreur</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA5101</td><td>Compilateur '{0}' manquant Installez le Kit NDK Android.</td></tr>
        <tr><td>XA5102</td><td>Échec de la conversion à partir de l’assembly en code natif. Veuillez archiver un rapport de bogue à <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5103</td><td>Impossible de compiler le fichier '{0}'. Veuillez archiver un rapport de bogue à <a href="http://bugzilla.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5201</td><td>Échec de la liaison natif. Passez en revue les indicateurs fournis à gcc : {0}</td></tr>
        <tr><td>XA5202</td><td>Échec de la liaison natif. Passez en revue le journal de génération.</td></tr>
        <tr><td>XA5303</td><td>Natif liaison Avertissement : {0}</td></tr>
        <tr><td>XA5300</td><td>Kit de développement logiciel Android introuvable ou pas entièrement installé.</td></tr>
        <tr><td>XA5301</td><td>Outil de « supprimer » manquant. Installez Xcode composant « Outils de ligne de commande »</td></tr>
        <tr><td>XA5302</td><td>Outil de 'dsymutil' manquant. Installez Xcode composant « Outils de ligne de commande »</td></tr>
        <tr><td>XA5203</td><td>Impossible de générer les symboles de débogage (répertoire dSYM). Passez en revue le journal de génération.</td></tr>
        <tr><td>XA5204</td><td>Impossible de supprimer le fichier binaire final. Passez en revue le journal de génération.</td></tr>
        <tr><td>XA5205</td><td>Outil de 'aapt' manquant. Installez le package d’outils de génération du SDK Android.</td></tr>
        <tr><td>XA5206</td><td>{0}. {{1} du répertoire de ressources Android n’existe pas.</td></tr>
        <tr><td>XA5207</td><td>{0}. {{1} de fichier de bibliothèque Java n’existe pas.</td></tr>
        <tr><td>XA5208</td><td>Échec du téléchargement. Téléchargez le {0} et placez-le dans le répertoire {{1}.</td></tr>
        <tr><td>XA5209</td><td>Échec de la décompression. Téléchargez le {0} et extraire vers le répertoire {{1}.</td></tr>
        <tr><td>XA5210</td><td>{0}. {{1} du fichier de bibliothèque native n’existe pas.</td></tr>
    </tbody>
</table>

### <a name="xa6xxx-errors"></a>Erreurs de XA6xxx

<table>
    <thead>
        <tr><td>Code d'erreur</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA6001</td><td>Exécution de la version de Cecil ne prend pas en charge les assembly suppression</td></tr>
        <tr><td>XA6002</td><td>Ne peut pas supprimer assembly `{0}`.</td></tr>
        <tr><td>XA6003</td><td>UnauthorizedAccessException message]</td></tr>
    </tbody>
</table>

### <a name="xa9xxx-errors"></a>XA9xxx Errors

Ce code d’erreur est des erreurs de licence et l’activation.

 <a name="xa9000" />


#### <a name="xa9000"></a>XA9000

 **Cause :** licence a expiré

 **Vérifiées pendant :** générer

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>AVERTISSEMENT</td>
            <td>AVERTISSEMENT</td>
            <td>AVERTISSEMENT</td>
            <td>AVERTISSEMENT</td>
            <td>AVERTISSEMENT</td>
        </tr>
    </tbody>
</table>

 <a name="XA9001" />


#### <a name="xa9001"></a>XA9001

 **Cause :** version d’évaluation a expiré.

 **Vérifiées pendant :** générer

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>AVERTISSEMENT</td>
            <td>AVERTISSEMENT</td>
            <td>AVERTISSEMENT</td>
            <td>AVERTISSEMENT</td>
            <td>AVERTISSEMENT</td>
        </tr>
    </tbody>
</table>

 <a name="XA9002" />


#### <a name="xa9002"></a>XA9002

 **Cause :** AndroidJavaSource

 **Vérifiées pendant :** générer

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Cause :** AndroidJavaLibrary

 **Vérifiées pendant :** générer

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Si un assembly de liaison a la .jar incorporé, cela est détecté au moment du Package, pas les temps de génération.**

 **Cause :** AndroidNativeLibrary

 **Vérifiées pendant :** générer

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9003" />


#### <a name="xa9003"></a>XA9003

 **Cause :** System.Runtime.Serialization

 **Vérifiées pendant :** Package

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Cause:** System.ServiceModel.Web

 **Vérifiées pendant :** Package

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Cause :** Mono.Data.Tds

 **Vérifiées pendant :** générer

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Elle est référencée par System.Data.dll, qui est autorisé**

 **Cause :** Mono.Android.Export

 **Vérifiées pendant :** générer

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9004" />


#### <a name="xa9004"></a>XA9004

 **Cause :** --de profilage

 **Vérifiées pendant :** Package

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9005" />


#### <a name="xa9005"></a>XA9005

 **Cause :** limite de taille (32 Ko)

 **Vérifiées pendant :** Package

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>OK</td>
            <td>-</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>

 <a name="XA9006" />


#### <a name="xa9006"></a>XA9006

 **Cause :** espace de noms System.Data.SqlClient

 **Vérifiées pendant :** Package

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9008" />


#### <a name="xa9008"></a>XA9008

 **Cause :** génération à partir de ligne de commande

 **Vérifiées pendant :** générer

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9009" />


#### <a name="xa9009"></a>XA9009

 **Cause :** numéro de série manquant

 **Vérifiées pendant :** qui

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9010" />


#### <a name="xa9010"></a>XA9010

 **Cause :** ProductId non valide

 **Vérifiées pendant :** générer

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
        </tr>
    </tbody>
</table>

Équivalent à XA9018

 <a name="XA9011" />


#### <a name="xa9011"></a>XA9011

 **Cause :** n’a pas pu mettre à jour le fichier de licence (vers le nouveau format de fichier)

 **Vérifiées pendant :** Activation

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9012" />


#### <a name="xa9012"></a>XA9012

 **Cause :** aucun internet

 **Vérifiées pendant :** Activation

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9013" />


#### <a name="xa9013"></a>XA9013

 **Cause :** erreur inconnue

 **Vérifiées pendant :** Activation

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9014" />


#### <a name="xa9014"></a>XA9014

 **Cause :** Code d’Activation non valide

 **Vérifiées pendant :** Activation

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9017" />


#### <a name="xa9017"></a>XA9017

 **Cause :** le serveur d’Activation ne retourne pas une licence valide

 **Vérifiées pendant :** Activation

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
            <td>ERREUR</td>
        </tr>
    </tbody>
</table>

<a name="XA9018" />

#### <a name="xa9018"></a>XA9018

**Cause :** licence non valide

**Vérifiées pendant :** générer

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Contient</th>
            <th>Business(Trial)</th>
            <th>Bureau</th>
            <th>Entreprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-</td>
            <td>-</td>
            <td>ERREUR</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>
