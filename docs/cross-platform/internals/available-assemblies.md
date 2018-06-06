---
title: Assemblys disponibles
description: Ce document répertorie les assemblys disponibles pour une utilisation dans Xamarin.iOS, Xamarin.Android et Xamarin.Mac. Il contient également des liens vers la documentation sur les bibliothèques .NET Standard et des bibliothèques de classes portables.
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: b73a818d3864c7c4d1d776e104d95090e87f5877
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781905"
---
# <a name="available-assemblies"></a>Assemblys disponibles

Xamarin.iOS, Xamarin.Android et Xamarin.Mac tous fournis avec les assemblys avec une douzaine. Tout comme Silverlight est un sous-ensemble étendu de bureau assemblys .NET, Xamarin plateformes est également un sous-ensemble étendu de plusieurs Silverlight et les assemblys .NET du bureau.

Xamarin plateformes ne sont pas ABI compatible avec les assemblys existants compilés pour un autre profil. Vous devez recompiler votre code source pour générer des assemblys ciblant le profil correct (tout comme vous devez recompiler le code source pour cibler Silverlight et .NET 3.5 de séparément).

Les applications Xamarin.Mac peuvent être compilées en trois modes : une qui utilise Xamarin d’organisé Mobile Profile, Xamarin.Mac .NET 4.5 Framework qui vous permet de cibler les assemblys de bureau à distance complets existants et un non pris en charge qui utilise l’API .NET utilisé dans un système Mono installation. Pour plus d’informations, consultez notre [infrastructure cible](~/mac/platform/target-framework.md) documentation.


## <a name="net-standard-libraries"></a>Bibliothèques .NET standards

Outre l’iOS, Android et Mac liaisons, les projets peuvent consommer de Xamarin [bibliothèques .NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Bibliothèques de classes portables
 
Les projets Xamarin peuvent également consommer [les bibliothèques de classes portables .NET](~/cross-platform/app-fundamentals/pcl.md), bien que cette technologie est déconseillée en faveur de .NET Standard.

## <a name="supported-assemblies"></a>Assemblys pris en charge

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|Compatibilité d’API|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|Inclut CJK, MidEast, autres, soit Rare, ouest|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|Fournisseur ADO.NET pour SQLite ; consultez les limitations.|✓|✓|✓|
> |Mono.Data.Tds.dll|Prise en charge du protocole TDS ; utilisé pour [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) prise en charge dans [System.Data](https://developer.xamarin.com/api/namespace/System.Data/).|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|API de chiffrement.|✓|✓|✓|
> |monotouch.dll|Cet assembly contient la liaison à l’API CocoaTouch c#. Cela est uniquement disponible dans les projets iOS classique.|✓| | |
> |MonoTouch. &#8203;1.dll de la boîte de dialogue| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|L’objet OpenGL/OpenAL orientés API étendu pour assurer la prise en charge des appareils iPhone.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), ainsi que les types d’espaces de noms suivants :<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx) , avec [certaines fonctionnalités supprimées](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|Client oData complète.|✓|✓|✓|
> |System.IO.&#8203;Compression| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System.&#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|Pile de WCF en tant que présents dans [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), ainsi que les types d’espaces de noms suivants : <br />Système<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;Transactions.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); dans le cadre du [System.Data](~/ios/data-cloud/system.data.md) prend en charge.|✓|✓|✓|
> |System.Web.&#8203;Services.dll|Base des services Web à partir du profil .NET 3.5, avec les fonctionnalités de serveur supprimé.|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Cet assembly contient la liaison à l’API CocoaTouch c#. Cela est utilisé uniquement dans les projets iOS unifié.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|Pour les writers de compilateur.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;Drawing.dll|System.Drawing API - API classique uniquement. System.Drawing n’est pas pris en charge dans l’API unifiée pour les infrastructures Mobile Xamarin.Mac .NET 4.5. Prise en charge System.Drawing peut être ajouté pour iOS et OS X à l’aide de la [sysdrawing-coregraphics](https://github.com/mono/sysdrawing-coregraphics) bibliothèque|✓| |✓|
