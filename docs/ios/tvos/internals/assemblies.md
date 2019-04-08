---
title: Pris en charge des assemblys par Xamarin pour tvOS
description: Afin d’aider à clarifier les fonctionnalités disponibles pour les applications tvOS, ce document fournit une liste d’assemblys pris en charge par Xamarin pour le développement de tvOS.
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: df50b4280335001f2d27ff23a91e4098eed3ba99
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870207"
---
# <a name="assemblies-supported-by-xamarin-for-tvos"></a>Pris en charge des assemblys par Xamarin pour tvOS

## <a name="supported-assemblies"></a>Assemblys pris en charge

Il s’agit d’une liste des assemblys pris en charge par Xamarin pour vos applications Xamarin.tvOS. Vous trouverez ci-dessous la liste détaillée de ces.  Certaines omissions notables incluent `System.EnterpriseServices`, la pile ASP.NET et Windows.Forms.

|Assembly|Added|Compatibilité d’API|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|Pour les writers de compilateur.|
|Mono.Data.Sqlite.dll|1.2|Fournisseur ADO.NET pour SQLite ; consultez [limitations](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|Prise en charge du protocole TDS ; utilisé pour [System.Data.SqlClient](xref:System.Data.SqlClient) prise en charge dans [System.Data](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1.0|API de chiffrement.|
|monotouch.dll|1.0|Cet assembly contient la [liaison C# à l’API CocoaTouch](https://docs.microsoft.com/dotnet/api/?view=xamarinios-10.8).|
|mscorlib.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|L’objet OpenGL/OpenAL orientée services API, [étendus pour fournir la prise en charge des appareils iPhone](xref:OpenGLES).|
|System.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), ainsi que des types à partir d’espaces de noms suivants : <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx), [avec certaines fonctionnalités supprimées](~/ios/data-cloud/system.data.md).|
|System.Data.Service.Client.dll|3.x|Client oData complète.|
|System.Drawing|1.0|System.Drawing API - API classique uniquement.<br />_System.Drawing n’est pas pris en charge dans l’API unifiée pour le Xamarin.Mac .NET 4.5 ou les infrastructures mobiles._|
|System.Json.dll|1.1|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) qu’à présent dans la pile [Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), ainsi que des types à partir d’espaces de noms suivants : <ul><li>Système</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx); dans le cadre de [System.Data](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) prennent en charge.|
|System.Web.Services|1.1|[Services Web de base](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) du profil .NET 3.5, avec les fonctionnalités de serveur supprimé.|
|System.Xml.dll|1.0|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>Bibliothèques de classes portables

Outre les liaisons de Mac, peuvent consommer Xamarin.tvOS [les bibliothèques de classes portables .NET](~/cross-platform/app-fundamentals/pcl.md).

## <a name="related-links"></a>Liens associés

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application pour tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
