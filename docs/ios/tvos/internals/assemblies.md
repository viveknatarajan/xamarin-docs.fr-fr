---
title: Assemblys
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 7d0ee27cfa2ae153ef481f943402f5fcfc5d04e4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="assemblies"></a>Assemblys

## <a name="supported-assemblies"></a>Assemblys pris en charge

Il s’agit d’une liste des assemblys pris en charge par Xamarin pour vos applications Xamarin.tvOS. Vous trouverez ci-dessous la liste détaillée de ces.  Certaines omissions notables incluent `System.EnterpriseServices`, la pile ASP.NET et Windows.Forms.

|Assembly|Ajouté|Compatibilité d’API|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|Pour les writers de compilateur.|
|Mono.Data.Sqlite.dll|1.2|Fournisseur ADO.NET pour SQLite ; consultez [limitations](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|Prise en charge du protocole TDS ; utilisé pour [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) prise en charge dans [System.Data](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1.0|API de chiffrement.|
|monotouch.dll|1.0|Cet assembly contient la [liaison c# à l’API CocoaTouch](https://developer.xamarin.com/api/root/ios-unified/).|
|mscorlib.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|L’objet OpenGL/OpenAL orientée services API, [étendue pour prendre en charge des appareils iPhone](https://developer.xamarin.com/api/namespace/OpenGLES/).|
|System.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), ainsi que les types d’espaces de noms suivants : <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx), [avec certaines fonctionnalités supprimées](~/ios/data-cloud/system.data.md).|
|System.Data.Service.Client.dll|3.x|Client oData complète.|
|System.Drawing|1.0|System.Drawing API - API classique uniquement.<br />_System.Drawing n’est pas pris en charge dans l’API unifiée pour les infrastructures Mobile Xamarin.Mac .NET 4.5._|
|System.Json.dll|1.1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) qu’à présent dans la pile [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), ainsi que les types d’espaces de noms suivants : <ul><li>Système</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); dans le cadre du [System.Data](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) prend en charge.|
|System.Web.Services|1.1|[Base des services Web](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) à partir du profil .NET 3.5, avec les fonctionnalités de serveur supprimé.|
|System.Xml.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>Bibliothèques de classes portables

Outre les liaisons Mac, peuvent consommer Xamarin.tvOS [les bibliothèques de classes portables .NET](~/cross-platform/app-fundamentals/pcl.md).



## <a name="related-links"></a>Liens associés

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
