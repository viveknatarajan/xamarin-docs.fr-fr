---
title: Matrice des erreurs Xamarin.Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: f3721ad661f4b817375b0d625c9b5cc293e6d44c
ms.sourcegitcommit: 4859da8772dbe920fdd653180450e5ddfb436718
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235075"
---
# <a name="xamarinandroid-errors-matrix"></a>Matrice des erreurs Xamarin.Android

## <a name="errors-reference"></a>Référence des erreurs

Ce document fournit des informations sur les différents codes d’erreur à partir de Xamarin.

|Category|Description|
|--- |--- |
|XA0xxx|Erreurs d’Android|
|XA1xxx|Copie d’un fichier / liens symboliques (projet liée) erreurs|
|XA2xxx|Erreurs de l'éditeur de liens|
|XA3xxx|Erreurs de compilation AOT|
|XA4xxx|Erreurs de génération de code|
|XA5xxx|Erreurs de la chaîne d’outils et de GCC|
|XA6xxx|erreurs des outils internes Android|
|XA7xxx|Réservée|
|XA8xxx|Réservée|
|XA9xxx|Erreurs de licence|


## <a name="error-codes"></a>Codes d'erreur

### <a name="xa0xxx-errors"></a>Erreurs de XA0xxx

|Code d'erreur|Description|
|--- |--- |
|XA0000|Erreur inattendue : Veuillez renseigner une [des bogues, rapport](https://github.com/xamarin/xamarin-android/issues/new).|
|XA0001|'-nom_unité a été fourni sans aucune action spécifique au périphérique.|
|XA0002|La variable d’environnement n’a pas pu être analyser '{0}'.|
|XA0003|Nom de l’application '{0}.exe' est en conflit avec un nom d’assembly (.dll) produit ou le Kit de développement logiciel.|
|XA0004|Nouvelle logique de refcounting nécessite sgen doit être activé en trop.|
|XA0005|Le répertoire de sortie '{0}' n’existe pas.|
|XA0006|Il n’existe aucune plateforme de développement à '{0}', utilisez--plateforme = PLAT pour spécifier le Kit de développement|
|XA0007|L’assembly racine '{0}' n’existe pas.|
|XA0008|Vous devez fournir un seul assembly racine uniquement.|
|XA0009|Erreur lors du chargement des assemblys : {0}.|
|XA0010|Les arguments de ligne de commande n’a pas pu être analyser : {0}.|
|XA0011|{0} a été développée pour un runtime plus récent ({1}) que MonoTouch prend en charge.|
|XA0012|Données incomplètes sont fournies pour l’exécution «{0}».|
|XA0013|Prise en charge le profilage de requiert sgen doit être activé en trop.|
|XA0014|iOS {0} ne prend pas en charge la création d’applications ciblant ARMv6.|
|XA0020|N’a pas pu déterminer le chemin d’accès d’Android.|
|XA0100|EmbeddedNativeLibrary '{0}' n’est pas valide dans le projet d’Application Android. Utilisez plutôt AndroidNativeLibrary.|

### <a name="xa1xxx-errors"></a>Erreurs de XA1xxx

|Code d'erreur|Description|
|--- |--- |
|XA1001|Impossible de trouver une application dans le répertoire spécifié.|
|XA1002|Impossible de créer liens symboliques, les fichiers ont été copiés.|
|XA1003|Impossible d’interrompre l’application '{0}'. Vous devrez peut-être arrêter l’application manuellement.|
|XA1004|Impossible d’obtenir la liste des applications installées.|
|XA1005|Impossible d’interrompre l’application '{0}'sur l’appareil'{1}' : {2}. Vous devrez peut-être arrêter l’application manuellement.|
|XA1006|Ne peut pas installer l’application '{0}'sur l’appareil'{1}' : {2}.|
|XA1007|Échec du lancement de l’application '{0}'sur l’appareil'{1}' : {2}. Vous pouvez toujours lancer l’application manuellement en appuyant dessus.|
|XA1008|Impossible de lancer le simulateur : {0}.|
|XA1009|Impossible de copier l’assembly '{0}'en'{1}' : {2}.|
|XA1010|Impossible de charger l’assembly '{0}' : {1}.|
|XA1011|Fichier de ressources manquantes n’a pas pu être ajouter : «{0}».|
|XA1101|N’a pas pu démarrer application.|
|XA1102|A pas pu attacher à l’application (pour l’arrêtiez) : {0}.|
|XA1103|Impossible de détacher.|
|XA1104|Échec d’envoi de paquets : {0}.|
|XA1105|Type de réponse inattendu.|
|XA1106|Impossible d’obtenir la liste des applications sur l’appareil : demande a expiré.|
|XA1107|Lancement de l’application a échoué.|
|XA1201|N’a pas pu charger le simulateur : {0}.|
|XA1301|Bibliothèque native '{0}» ({1}) a été ignoré, car il ne correspond pas à l’architecture(s) de build actuel ({2}).|

### <a name="xa2xxx-errors"></a>Erreurs de XA2xxx

|Code d'erreur|Description|
|--- |--- |
|XA2001|Impossible de lier les assemblys.|
|XA2002|Impossible de résoudre la référence : {0}.|
|XA2003|Option '{0}' va être ignorée car la liaison est désactivée.|
|XA2004|Fichier de définitions de l’éditeur de liens supplémentaires '{0}' n’a pas été trouvé.|
|XA2005|Définitions à partir de '{0}' ne peut pas être analysée.|
|XA2006|Référence à l’élément de métadonnées '{0}» (défini dans '{1}») à partir de '{2}' n’a pas pu être résolu.|

### <a name="xa3xxx-errors"></a>Erreurs de XA3xxx

Il s’agit des erreurs de compilation AOT.

|Code d'erreur|Description|
|--- |--- |
|XA3001|Pu pas AOT l’assembly '{0}'.|
|XA3002|Restriction de AOT : méthode '{0}' doit être statique, car elle est décorée avec [MonoPInvokeCallback].|
|XA3003|Options de débogage et--llvm--conflictuelles. Soft-débogage est désactivé.|


### <a name="xa4xxx-errors"></a>Erreurs de XA4xxx

Il s’agit d’erreurs de génération de code.

|Code d'erreur|Description|
|--- |--- |
|XA4001|Le modèle principal n’a pas pu être expansed à «{0}».|
|XA4101|Le bureau d’enregistrement ne peut pas générer une signature pour le type '{0}'.|
|XA4102|Le bureau d’enregistrement trouvé un type non valide '{0}« dans la signature de méthode'{2}'. Utilisez '{1}' à la place.|
|XA4103|Le bureau d’enregistrement trouvé un type non valide '{0}« dans la signature de méthode'{2}' : le type implémente INativeObject, mais n’a pas de constructeur qui prend deux (IntPtr, bool) arguments.|
|XA4104|Le bureau d’enregistrement ne peut pas marshaler la valeur de retour de type «{0}« dans la signature de méthode'{1}'.|
|XA4105|Le bureau d’enregistrement ne peut pas marshaler le paramètre de type '{0}« dans la signature de méthode'{1}'.|
|XA4106|Le bureau d’enregistrement ne peut pas marshaler la valeur de retour pour la structure '{0}« dans la signature de méthode'{1}'.|
|XA4107|Le bureau d’enregistrement ne peut pas marshaler le paramètre de type '{0}« dans la signature de méthode'{1}'.|
|XA4108|Le bureau d’enregistrement ne peut pas obtenir le type de ObjectiveC pour le type managé '{0}'.|
|XA4109|Impossible de compiler le code généré de bureau d’enregistrement. Veuillez soumettre un [des bogues, rapport](http://bugzilla.xamarin.com).|
|XA4110|Le bureau d’enregistrement ne peut pas marshaler le paramètre out de type «{0}« dans la signature de méthode'{1}'.|
|XA4111|Le bureau d’enregistrement ne peut pas générer une signature pour le type '{0}« dans la méthode'{1}'.|
|XA4200|Ne peut générer de ACW pour les types 'claas'.|
|XA4201|Impossible de déterminer le nom JNI pour type {0}.|
|XA4203|Le nom de type spécifié doit être qualifié complet.|
|XA4204|Impossible de résoudre le type d’interface '{0}'. Vérifiez qu'il ne manque aucune référence d'assembly.|
|XA4205|[ExportField] utilisable uniquement sur les méthodes avec des paramètres de 0.|
|XA4206|[Exportation] ne peut pas être utilisée sur un type générique.|
|XA4207|[ExportField] ne peut pas être utilisé sur un type générique.|
|XA4208|[Java.Interop.ExportFieldAttribute] ne peut pas être utilisé sur une méthode retournant void.|
|XA4209|Impossible de créer JavaTypeInfo pour la classe : {0} en raison {1}.|
|XA4210|Vous devez ajouter une référence à Mono.Android.Export.dll lorsque vous utilisez ExportAttribute ou ExportFieldAttribute.|
|XA4211|AndroidManifest.xml //uses-sdk/@android:targetSdkVersion '{0}« est inférieure à $(TargetFrameworkVersion) »{1}'. À l’aide de API -{1} pour la compilation de ACW.|


### <a name="xa5xxx-errors"></a>Erreurs de XA5xxx

|Code d'erreur|Description|
|--- |--- |
|XA5101|Manquant '{0}' compilateur. Installez NDK Android.|
|XA5102|Échec de la conversion à partir de l’assembly en code natif. Veuillez soumettre un [des bogues, rapport](http://bugzilla.xamarin.com).|
|XA5103|Impossible de compiler le fichier '{0}'. Veuillez soumettre un [des bogues, rapport](http://bugzilla.xamarin.com).|
|XA5201|Échec de la liaison natif. Veuillez consulter les indicateurs fournis pour gcc : {0}|
|XA5202|Échec de la liaison natif. Passez en revue le journal de génération.|
|XA5303|Natif liaison Avertissement : {0}.|
|XA5300|Kit de développement logiciel Android introuvable ou pas complètement installé.|
|XA5301|Outil de « supprimer » manquant. Installez Xcode composant « Outils de ligne de commande ».|
|XA5302|Outil de 'dsymutil' manquant. Installez Xcode composant « Outils de ligne de commande ».|
|XA5203|Impossible de générer les symboles de débogage (répertoire dSYM). Passez en revue le journal de génération.|
|XA5204|Impossible de supprimer le fichier binaire final. Passez en revue le journal de génération.|
|XA5205|Outil de 'aapt' manquant. Installez le package d’outils de génération du Kit Android SDK.|
|XA5206|{0}. Répertoire de ressources Android {1} n’existe pas.|
|XA5207|{0}. Fichier bibliothèque Java {1} n’existe pas.|
|XA5208|Échec du téléchargement. Téléchargez {0} et le placer la {1} directory.|
|XA5209|La décompression a échoué. Téléchargez {0} et extrayez-le dans le {1} directory.|
|XA5210|{0}. Le fichier bibliothèque natif {1} n’existe pas.|

### <a name="xa6xxx-errors"></a>Erreurs de XA6xxx

|Code d'erreur|Description|
|--- |--- |
|XA6001|Version en cours d’exécution de Cecil ne prend pas en charge les assembly jointes.|
|XA6002|Ne peut pas supprimer assembly '{0}'.|
|XA6003|UnauthorizedAccessException message.|

### <a name="xa9xxx-errors"></a>Erreurs de XA9xxx

Ces codes d’erreur sont des erreurs de licence et d’activation.


#### <a name="xa9000"></a>XA9000

 **Cause :** licence a expiré

 **Vérifiées pendant :** générer

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|AVERTISSEMENT|AVERTISSEMENT|AVERTISSEMENT|AVERTISSEMENT|AVERTISSEMENT|


#### <a name="xa9001"></a>XA9001

 **Cause :** essai arrivé à expiration

 **Vérifiées pendant :** générer

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|AVERTISSEMENT|AVERTISSEMENT|AVERTISSEMENT|AVERTISSEMENT|AVERTISSEMENT|



#### <a name="xa9002"></a>XA9002

 **Cause :** AndroidJavaSource

 **Vérifiées pendant :** générer

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|OK|OK|OK|OK|

 **Cause :** AndroidJavaLibrary

 **Vérifiées pendant :** générer

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|OK|OK|OK|OK|

 **Si un assembly de liaison a le fichier .jar incorporé, cela est détecté au moment du Package, pas les temps de génération.**

 **Cause :** AndroidNativeLibrary

 **Vérifiées pendant :** générer

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|OK|OK|OK|OK|


#### <a name="xa9003"></a>XA9003

 **Cause :** System.Runtime.Serialization

 **Vérifiées pendant :** Package

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|OK|OK|OK|

 **Cause :** System.ServiceModel.Web

 **Vérifiées pendant :** Package

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|OK|OK|OK|

 **Cause :** Mono.Data.Tds

 **Vérifiées pendant :** générer

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|OK|OK|OK|

 **Elle est référencée par System.Data.dll, qui est autorisé**

 **Cause :** Mono.Android.Export

 **Vérifiées pendant :** générer

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|OK|OK|OK|OK|



#### <a name="xa9004"></a>XA9004

 **Cause :** --profilage

 **Vérifiées pendant :** Package

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|OK|OK|OK|



#### <a name="xa9005"></a>XA9005

 **Cause :** limite de taille (32 Ko).

 **Vérifiées pendant :** Package

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|OK|-|-|-|



#### <a name="xa9006"></a>XA9006

 **Cause :** espace de noms System.Data.SqlClient.

 **Vérifiées pendant :** Package

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|OK|OK|OK|


#### <a name="xa9008"></a>XA9008

 **Cause :** génération à partir de ligne de commande.

 **Vérifiées pendant :** générer

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|OK|OK|OK|


#### <a name="xa9009"></a>XA9009

 **Cause :** numéro de série manquant.

 **Vérifiées pendant :** Untestable

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|ERREUR|ERREUR|ERREUR|


#### <a name="xa9010"></a>XA9010

 **Cause :** ProductId non valide.

 **Vérifiées pendant :** générer

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|ERREUR|ERREUR|ERREUR|

Équivalent à XA9018.



#### <a name="xa9011"></a>XA9011

 **Cause :** n’a pas pu mettre à jour le fichier de licence (pour le nouveau format de fichier).

 **Vérifiées pendant :** Activation

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|ERREUR|ERREUR|ERREUR|

#### <a name="xa9012"></a>XA9012

 **Cause :** aucune connexion internet

 **Vérifiées pendant :** Activation

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|ERREUR|ERREUR|ERREUR|


#### <a name="xa9013"></a>XA9013

 **Cause :** erreur inconnue

 **Vérifiées pendant :** Activation

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|ERREUR|ERREUR|ERREUR|


#### <a name="xa9014"></a>XA9014

 **Cause :** Code d’Activation non valide

 **Vérifiées pendant :** Activation

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|ERREUR|ERREUR|ERREUR|


#### <a name="xa9017"></a>XA9017

 **Cause :** serveur d’Activation ne retourne pas une licence valide.

 **Vérifiées pendant :** Activation

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|ERREUR|ERREUR|ERREUR|ERREUR|ERREUR|


#### <a name="xa9018"></a>XA9018

**Cause :** licence non valide

**Vérifiées pendant :** générer

|Kit de démarrage|Indie|Business(Trial)|Bureau|Entreprise|
|--- |--- |--- |--- |--- |
|-|-|ERREUR|-|-|

