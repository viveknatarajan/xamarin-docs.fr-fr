---
title: Compression de fichiers dans Xamarin.iOS
description: Ce document décrit comment utiliser l’API libcompression dans Xamarin.iOS. Il aborde DÉGONFLAGE, gonfler, et les différents algorithmes pris en charge.
ms.prod: xamarin
ms.assetid: 94D05DAB-01E8-4C62-9CEF-9D6417EEA8EB
ms.technology: xamarin-ios
author: mandel-macaque
ms.author: mandel
ms.date: 03/04/2019
ms.openlocfilehash: f7a1df65047fd8040dd40e9f7f057d6bfe6dea61
ms.sourcegitcommit: 4c97f5d73be7eb2da153a85183be4258b6b11ca6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58290924"
---
# <a name="file-compression-in-xamarinios"></a>Compression de fichiers dans Xamarin.iOS

Les applications Xamarin ciblant iOS 9.0 ou macOS 10.11 (et versions ultérieures) peuvent utiliser le _Compression Framework_ pour compresser (Encoder) et décompresser (décoder) données. Xamarin.iOS fournit cette infrastructure de l’API Stream. L’infrastructure de la compression permet aux développeurs d’interagir avec le compresser et décompresser les données comme si elles étaient des flux normales sans la nécessité d’utiliser des rappels ou des délégués.

L’infrastructure de la compression prend en charge les algorithmes suivants :

* LZ4
* LZ4 brutes
* Lzfse
* Lzma
* Zlib

À l’aide de l’infrastructure de la compression permet aux développeurs d’effectuer les opérations de compression sans bibliothèques tierces ni de packages NuGet. Cela réduit les dépendances externes et garantit que les opérations de compression seront être pris en charge sur toutes les plateformes (à condition qu’ils la configuration minimale requise du système d’exploitation).

## <a name="general-file-decompression"></a>Décompression de fichiers généraux

L’infrastructure de la compression utilise un API de flux dans Xamarin.iOS et Xamarin.Mac. Cette API signifie que pour compresser les données, le développeur peut utiliser les modèles normales utilisés dans d’autres API d’e/s dans .NET. L’exemple suivant montre comment décompresser les données avec l’infrastructure de la compression, qui est similaire à l’API dans le `System.IO.Compression.DeflateStream` API :

```csharp
// sample zlib data
static byte [] compressed_data = { 0xf3, 0x48, 0xcd, 0xc9, 0xc9, 0xe7, 0x02, 0x00 };

using (var backing = new MemoryStream (compressed_data)) // compress data to read
using (var decompressing = new CompressionStream (backing, CompressionMode.Decompress, CompressionAlgorithm.Zlib)) // create decompression stream with the correct algorithm
using (var reader = new StreamReader (decompressing))
{
    // perform the required stream operations
    Console.WriteLine (reader.ReadLine ());
}
```

Le `CompressionStream` implémente le `IDisposable` interface, comme les autres `System.IO.Streams`, de sorte que les développeurs doivent s’assurer que les ressources sont libérées une fois qu’ils ne sont plus nécessaires.

## <a name="general-file-compression"></a>Compression de fichiers généraux

L’API de la Compression permet également aux développeurs de compresser les données suivant la même API. Données peuvent être compressées à l’aide d’un des algorithmes fournis indiqués dans le `CompressionAlgorithm` énumérateur.

```csharp
// sample method that copies the data from the source stream to the destination stream
static void CopyStream (Stream src, Stream dest)
{
    byte[] array = new byte[1024];
    int bytes_read;
    bytes_read = src.Read (array, 0, 1024);
    while (bytes_read != 0) {
        dest.Write (array, 0, bytes_read);
        bytes_read = src.Read (array, 0, 1024);
    }
}

static void CompressExample ()
{
    // create some sample data to compress
    byte [] data = new byte[100000];
    for (int i = 0; i < 100000; i++) {
        data[i] = (byte) i;
    }

    using (var dataStream = new MemoryStream (data)) // stream that contains the data to compress
    using (var backing = new MemoryStream ()) // stream in which the compress data will be written
    using (var compressing = new CompressionStream (backing, CompressionMode.Compress, CompressionAlgorithm.Zlib, true))
    {
        // copy the data to the compressing stream
        CopyStream (dataStream, compressing);
        // at this point, the CompressionStream contains all the data from the dataStream but compressed
        // using the zlib algorithm
    }
```

## <a name="async-support"></a>Prise en charge asynchrone

Le `CompressionStream` prend en charge toutes les opérations asynchrones sont prises en charge par le `System.IO.DeflateStream`, ce qui signifie que les développeurs peuvent utiliser le mot clé async pour effectuer les opérations de compression/décompression sans bloquer le thread d’interface utilisateur.