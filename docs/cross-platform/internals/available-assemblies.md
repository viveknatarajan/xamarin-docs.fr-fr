---
title: Assemblys disponibles
description: Ce document répertorie les assemblys disponibles pour une utilisation dans Xamarin.iOS, Xamarin.Android et Xamarin.Mac. Il contient également des liens vers la documentation sur les bibliothèques .NET Standard et des bibliothèques de classes portables.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: 8882a7cd36eab5e7905585f5de4d6585dfb53439
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672259"
---
# <a name="available-assemblies"></a>Assemblys disponibles

Xamarin.iOS, Xamarin.Android et Xamarin.Mac tous fournis avec plus d’une dizaine d’assemblys. Tout comme Silverlight est un sous-ensemble étendu des assemblys du .NET de bureau, plateformes Xamarin est également un sous-ensemble étendu de plusieurs Silverlight et des assemblys .NET de bureau.

Plateformes Xamarin ne sont pas ABI compatible avec des assemblys existants compilés pour un autre profil. Vous devez recompiler votre code source pour générer des assemblys ciblant le profil approprié (comme vous avez besoin de recompiler le code source pour cibler Silverlight et .NET 3.5 séparément).

Les applications Xamarin.Mac peuvent être compilées dans trois modes : un qui utilise Xamarin d’organisée Mobile Profile, Xamarin.Mac .NET 4.5 Framework qui vous permet de cibler les assemblys de bureau complet existants, et un non pris en charge qui utilise l’API .NET se trouvée dans un système Mono installation. Pour plus d’informations, consultez notre [Frameworks cibles](~/mac/platform/target-framework.md) documentation.

## <a name="net-standard-libraries"></a>Bibliothèques .NET standard

Outre les iOS, Android et Mac Xamarin projets peuvent exploiter les liaisons, [bibliothèques .NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Bibliothèques de classes portables

Projets Xamarin peuvent également consommer [les bibliothèques de classes portables .NET](~/cross-platform/app-fundamentals/pcl.md), bien que cette technologie est déconseillée en faveur de .NET Standard.

## <a name="supported-assemblies"></a>Assemblys pris en charge

Il s’agit d’assemblys disponibles dans le **Gestionnaire de références > assemblys > Framework** (Visual Studio 2017) et **modifier les références > Packages** (Visual Studio pour Mac) et la compatibilité avec les plateformes de Xamarin.

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|Compatibilité d’API|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|Includes CJK, MidEast, Other, Rare, West|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|Fournisseur ADO.NET pour SQLite ; reportez-vous aux restrictions.|✓|✓|✓|
> |Mono.Data.Tds.dll|Prise en charge du protocole TDS ; utilisé pour [System.Data.SqlClient](xref:System.Data.SqlClient) prise en charge dans [System.Data](xref:System.Data).|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|API de chiffrement.|✓|✓|✓|
> |monotouch.dll|Cet assembly contient la liaison à l’API CocoaTouch c#. Cela est uniquement disponible dans les projets iOS Classic.|✓| | |
> |MonoTouch.&#8203;Dialog-1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|L’objet OpenGL/OpenAL orientée services API, étendues pour assurer la prise en charge des appareils iPhone.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), ainsi que des types à partir d’espaces de noms suivants :<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx) , avec [supprimé certaines fonctionnalités](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|Client oData complète.|✓|✓|✓|
> |System.IO.&#8203;Compression| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System.&#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|Pile WCF telle qu’elle est [Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), ainsi que des types à partir d’espaces de noms suivants : <br />Système<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;Transactions.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx); dans le cadre de [System.Data](~/ios/data-cloud/system.data.md) prennent en charge.|✓|✓|✓|
> |System.Web.&#8203;Services.dll|Services Web de base à partir du profil .NET 3.5, avec les fonctionnalités de serveur supprimé.|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](https://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Cet assembly contient la liaison à l’API CocoaTouch c#. Cela est utilisé uniquement dans les projets iOS unifiée.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|Pour les writers de compilateur.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;Drawing.dll|System.Drawing n’est pas pris en charge dans l’API unifiée pour le Xamarin.Mac, .NET 4.5 ou des infrastructures mobiles. Prise en charge System.Drawing peut être ajouté à iOS et macOS à l’aide de la [sysdrawing-coregraphics](https://github.com/mono/sysdrawing-coregraphics) bibliothèque|✓| |✓|
