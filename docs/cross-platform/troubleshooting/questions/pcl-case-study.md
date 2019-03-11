---
title: Résoudre les problèmes liés à System.Diagnostics.Tracing et flux de données TPL
description: 'Étude de cas PCL : Comment résoudre des problèmes liés à System.Diagnostics.Tracing pour le package NuGet Microsoft TPL Dataflow ?'
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
author: asb3993
ms.author: amburns
ms.openlocfilehash: d9aa85b946f20addb7d69c559bff68c6b1f75429
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669841"
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>Étude de cas PCL : Comment résoudre des problèmes liés à System.Diagnostics.Tracing pour le package NuGet Microsoft TPL Dataflow ?

> [!IMPORTANT]
> Cet exemple particulier de `System.Diagnostic.Tracing` n’émet plus de toutes les erreurs par défaut dans les dernières versions de Xamarin. Bien que la solution de contournement suggérée continueront de fonctionner, notez que certains des bogues mentionnés dans la section « couches d’erreurs » ont été résolus.
> En outre, vous devez noter que .NET Standard est désormais le meilleur moyen de l’implémentation des API de .NET inter-plateformes.

## <a name="summary"></a>Récapitulatif

Xamarin.iOS et Xamarin.Android n’implémentent pas 100 % de chaque profil PCL acceptant en tant que références. Pour plus de commodité pratique dans Visual Studio pour Mac, Visual Studio et le Gestionnaire de package NuGet, projets Xamarin autorisent l’utilisation de plusieurs profils qui ont uniquement des _incomplète_ implémentations. Par exemple, ni Xamarin.iOS ni Xamarin.Android actuellement inclut une implémentation complète des types dans la bibliothèque de classes portable « System.Diagnostics.Tracing » espace de noms. Cette limitation mène à trois couches d’erreurs en tentant d’utiliser la valeur par défaut `portable-net45+win8+wpa81` version du package NuGet Microsoft TPL Dataflow.

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>Solution de contournement : Basculer le projet d’application pour faire référence à la `portable-net45+win8+wp8+wpa81` version de la bibliothèque de flux de données TPL

(Cela permet d’éviter toutes les trois couches d’erreurs et fonctionne pour toutes les versions récentes de Xamarin.)

1. Ouvrez le projet d’application **.csproj** fichier dans un éditeur de texte.

2. Recherchez la ligne qui ressemble à ceci :

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. Modification `portable-net45+win8+wpa81` à `portable-net45+win8+wp8+wpa81` (`+wp8` est ajouté) :

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>Explication

Le `portable-net45+win8+wp8+wpa81` version de la bibliothèque ne fait pas référence à **System.Diagnostics.Tracing.dll** _tout_, elle évite complètement tous les trois couches de problèmes.

### <a name="limitations"></a>Limitations

- Le `portable-net45+win8+wp8+wpa81` version de la bibliothèque peut ne pas inclure de 100 % des fonctionnalités de la `portable-net45+win8+wpa81` version.

- Le Gestionnaire de package NuGet installe le `portable-net45+win8+wpa81` version du package NuGet de bibliothèque de classes portable par défaut, donc vous devez ajuster manuellement la référence.

## <a name="details-about-the-three-layers-of-errors"></a>Plus d’informations sur les trois couches d’erreurs

1. Le **System.Diagnostics.Tracing.dll** assembly de façade est actuellement absent de toutes les versions de Mac de Xamarin.Android (non publics bogue 34888) et d’absence de toutes les versions de Xamarin.iOS inférieures à 9.0 (ou inférieures à XamarinVS 3.11.1443 sur Windows) (résolu dans [bogue 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388)). Ce problème entraîne une des erreurs suivantes en fonction de la cible de déploiement et de l’éditeur de liens paramètres :

    - Xamarin.Android.Common.targets: Erreur : Exception lors du chargement des assemblys : System.IO.FileNotFoundException: Could not load assembly 'System.Diagnostics.Tracing, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'. Par exemple, il n’existe pas dans Mono pour Android profil ?

    - Impossible de charger le fichier ou l’assembly 'System.Diagnostics.Tracing' ou une de ses dépendances. Le système ne trouve pas le fichier spécifié. (System.IO.FileNotFoundException)

    - MTOUCH : erreur MT3001 : Pu pas AOT l’assembly ' / Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll '

    - MTOUCH : erreur MT2002 : Impossible de résoudre l’assembly : 'System.Diagnostics.Tracing, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'

2. Actuel [Mono implémentation des types dans « System.Diagnostics.Tracing »](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) manque certaines surcharges de méthode ([bogue 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Ce problème entraîne les erreurs de l’éditeur de liens suivantes lors de la création d’une application Xamarin :

    - / Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets : erreur : Erreur d’exécution de tâches LinkAssemblies : erreur XA2006 : Référence aux métadonnées d’élément « System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[]) » (défini dans ' System.Threading.Tasks.Dataflow, Version = 4.5.24.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a ») à partir de ' System.Threading.Tasks.Dataflow, Version = 4.5.24.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a » n’a pas pu être résolu.

    - MTOUCH : erreur MT2002 : Impossible de résoudre la référence « System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[]) » à partir de « System.Diagnostics.Tracing, Version = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a »

3. Actuel [Mono implémentation des types dans « System.Diagnostics.Tracing »](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) est actuellement un _vide_ implémentation « factice » ([bogue 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)). Toute tentative d’utiliser ces méthodes au moment de l’exécution par conséquent peut produire des résultats inattendus. Pour le _particulier_ cas de la bibliothèque de flux de données de la bibliothèque parallèle de tâches de Microsoft, il semble que les appels à `WriteEvent(System.Int32,System.Object[])` ne sont pas indispensables pour la majeure partie du comportement de la bibliothèque, par conséquent, le correctif pour « couche 2 » ([bogue 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337), ajout vide implémentations) sera probablement suffisantes pour la plupart des cas d’utilisation de flux de données TPL de Microsoft.

## <a name="questions--answers"></a>Questions et réponses

### <a name="i-was-able-to-leave-linking-enabled-with-the-codeportable-net45win8wpa81code-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>J’ai pu laisser liaison activé avec le <code>portable-net45+win8+wpa81</code> version de la bibliothèque sur les versions antérieures de Xamarin.iOS ou Xamarin.Android. Comment fonctionne ?

#### <a name="answer"></a>Réponse

Il est _possible_ pour obtenir de la build terminée « réussi » (avec la liaison activé) dans les versions antérieures de Xamarin.iOS ou Xamarin.Android sur Mac si vous incluez une référence à la `System.Diagnostics.Tracing.dll` _font référence à l’assembly_ \[1\] plutôt que _assembly de façade_ \[2], mais ce n’est malheureusement pas une solution de contournement « correcte ». Assemblys de référence sont uniquement destinés à être utilisé lors de la génération _bibliothèques portables_, code non spécifique à la plateforme telles que des applications. Tentative de _exécuter_ le code contenu dans les assemblys de référence (plutôt que simplement build contre lui) est susceptible de produire des résultats inattendus. Le correctif approprié sera pour l’équipe Mono ajouter le manquant `WriteEvent(System.Int32,System.Object[])` surcharger pour le [ `EventSource` ](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) type ([bogue 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Pour désormais la meilleure option consiste à passer à la `portable-net45+win8+wp8+wpa81` version de la bibliothèque de flux de données TPL de Microsoft, comme indiqué dans la section solution de contournement ci-dessus.

(Pour toute personne qui peut être lu cet article lorsque vous voyez une réponse plus ancienne et plus directe connexe du site StackOverflow (<https://stackoverflow.com/a/23591322/2561894>), notez que la distinction entre les assemblys de référence et d’assembly de façade a été _pas_ mentionnée ici.)

**\[1\] emplacements « Assembly de référence »**

Windows : `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono) : `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] les emplacements « Assembly de façade »**

Windows : `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono) : `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>Il aidera si j’ajoute manuellement une référence à l’assembly de façade « System.Diagnostics.Tracing » ?

_En particulier peux résoudre le problème à l’aide de ces 2 étapes ?_

1. _Copie le `System.Diagnostics.Tracing.dll` assembly de façade dans le dossier de projet d’application à partir d’un des emplacements suivants :_

    Windows : `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono) : `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Ajoutez une référence à l’assembly de façade dans le projet d’application Xamarin.iOS ou Xamarin.Android._

#### <a name="answer"></a>Réponse

Non, cela ne permettra pas.

- Pour Xamarin.iOS 9.0 ou n’importe quelle version récente de Xamarin.Android sur Windows, cette solution de contournement est strictement redondante et peut-être provoquer des erreurs de compilation semblable à « un assembly 'System.Diagnostics.Tracing' avec la même identité a déjà été importé. ».

- Pour Xamarin.iOS 8.10 ou inférieure ou Xamarin.Android sur Mac, cette solution de contournement aidera mais _uniquement_ pour le problème d’assembly manquant « couche 1 ». Il sera _pas_ résoudre les erreurs de l’éditeur de liens « couche 2 », donc il n’est pas une solution complète.

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>Puis-je utiliser le [package NuGet de System.Diagnostics.Tracing](https://www.nuget.org/packages/System.Diagnostics.Tracing/) pour résoudre le problème ?

#### <a name="answer"></a>Réponse

Non, le package de « System.Diagnostics.Tracing » NuGet 3.0 inclut uniquement des implémentations spécifiques à la plateforme pour : « DNXCore50 » et « netcore50 ». Il explicitement _omet_ implémentations pour Xamarin.Android (« MonoAndroid ») et Xamarin.iOS (« MonoTouch » et « xamarinios »). Cela signifie que l’installation du package a _aucun effet_ pour les projets Xamarin.Android et Xamarin.iOS. Le package NuGet part du principe que ces plateformes fournissent leurs _propre_ implémentation des types. Cette hypothèse est « correcte » dans le sens où Mono a _un_ implémentation de l’espace de noms, mais comme nous l’avons vu sous points \#2 et \#3 de « Plus d’informations sur les trois couches d’erreurs » ci-dessus, le implémentation est incomplète. Ainsi, le correctif approprié sera pour l’équipe Mono résoudre [bogue 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337) et [bogue 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890).

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir une assistance supplémentaire, pour nous contacter, ou si le reste de ce problème même après utilisant les informations ci-dessus, veuillez consulter [quelles options de prise en charge sont disponibles pour Xamarin ?](~/cross-platform/troubleshooting/support-options.md) pour plus d’informations sur les options de contact, des suggestions, ainsi que comment signaler un bogue nouveau si nécessaire.
