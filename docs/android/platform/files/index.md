---
title: Accès au fichier avec Xamarin.Android
description: Ce guide explique le chemin d’accès dans Xamarin.Android
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/23/2018
ms.openlocfilehash: 476f1c50a2f1a4199dfaf1996fc9c16615b40598
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116795"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>Stockage de fichier et l’accès avec Xamarin.Android

Une exigence courante pour les applications Android est de manipuler des fichiers &ndash; l’enregistrement des images, de téléchargement de documents ou d’exportation des données à partager avec d’autres programmes. Android (qui est basé sur Linux) prend en charge en fournissant un espace de stockage de fichiers. Android regroupe le système de fichiers dans les deux différents types de stockage :

* **Stockage interne** &ndash; il s’agit d’une partie du système de fichiers qui sont accessibles uniquement par l’application ou le système d’exploitation.
* **Stockage externe** &ndash; il s’agit d’une partition pour le stockage de fichiers accessible par toutes les applications, l’utilisateur et éventuellement d’autres appareils. Sur certains appareils, un stockage externe peut être amovible (par exemple, une carte SD).

Ces regroupements sont conceptuels uniquement et sans nécessairement référence à une partition unique ou un répertoire sur l’appareil. Un appareil Android fournira toujours la partition pour le stockage interne et de stockage externe. Il est possible que certains appareils peuvent avoir plusieurs partitions qui sont considérés comme étant un stockage externe. Quelle que soit la partition, les API pour la lecture, écriture ou la création de fichiers est le même. Il existe deux ensembles d’API qu’une application Xamarin.Android peut utiliser pour l’accès de fichier :

1. **Les API .NET (fourni par Mono et encapsulé par Xamarin.Android)** &ndash; ces inclut le [assistances de système de fichiers](~/essentials/file-system-helpers.md?context=xamarin/android) fourni par [Xamarin.Essentials](~/essentials/index.md?context=xamarin/android). Les API .NET fournissent la meilleure compatibilité inter-plateformes et par conséquent l’objectif de ce guide sur ces API.
1. **L’accès de fichier Java natif API (fourni par Java et encapsulé par Xamarin.Android)** &ndash; Java fournit sa propre API pour lire et écrire des fichiers. Ceux-ci sont une alternative complètement acceptable pour les API .NET, mais sont spécifiques à Android et ne conviennent pas pour les applications qui sont destinées à être inter-plateformes.

Lecture et écriture dans des fichiers sont presque identique dans Xamarin.Android, car il s’agit à n’importe quel autre application .NET. L’application Xamarin.Android détermine le chemin d’accès au fichier qui sera manipulé, puis utilise standard .NET idiomes pour l’accès de fichier. Étant donné que les chemins d’accès réels au stockage interne et externe peuvent varier d’un périphérique à l’autre ou à partir de la version d’Android vers la version Android, il est déconseillé de coder en dur le chemin d’accès aux fichiers. Au lieu de cela, utilisez les APIs Xamarin.Android pour déterminer le chemin d’accès aux fichiers. De cette façon, les API .NET pour lire et écrire des fichiers expose les API Android natives qui vous aident à déterminer le chemin d’accès aux fichiers sur le stockage interne et externe.

Avant d’aborder les API ayant eu accès au fichier, il est important de comprendre certains détails concernant le stockage interne et externe. Ce sujet sera abordé dans la section suivante.

## <a name="internal-vs-external-storage"></a>Stockage externe et interne

Sur le plan conceptuel, le stockage interne et externe sont très similaires &ndash; qu’ils sont les deux emplacements à laquelle une application Xamarin.Android peut enregistrer des fichiers. Cette similarité peut prêter à confuse pour les développeurs qui ne connaissent pas Android comme il n’est pas clair lorsqu’une application doit utiliser un stockage interne vs un stockage externe.

Stockage interne fait référence à la mémoire non volatile Android alloue au système d’exploitation, les fichiers Apk et pour des applications individuelles. Cet espace n’est pas accessible sauf par le système d’exploitation ou les applications. Android alloue un répertoire dans la partition de stockage interne pour chaque application. Lorsque l’application est désinstallée, tous les fichiers sont conservés sur le stockage interne dans ce répertoire sont également supprimés. Stockage interne est idéal pour les fichiers qui sont uniquement accessibles à l’application et qui ne seront pas partagées avec d’autres applications ou aura très peu d’intérêt, une fois que l’application est désinstallée. Sur Android 6.0 ou version ultérieure, les fichiers sur un stockage interne peuvent être automatiquement sauvegardés à l’aide de Google le [fonctionnalité de sauvegarde automatique dans Android 6.0](https://developer.android.com/guide/topics/data/autobackup). Stockage interne présente les inconvénients suivants :

* Les fichiers ne peuvent pas être partagés.
* Fichiers seront supprimés lors de l’application est désinstallée.
* L’espace disponible sur un stockage interne peut-être limité.

Stockage externe fait référence au stockage de fichier qui n’est pas un stockage interne et non accessible exclusivement à une application. L’objectif principal de stockage externe consiste à fournir un emplacement pour placer les fichiers qui sont destinés à être partagés entre les applications ou qui sont trop volumineux pour tenir sur le stockage interne. L’avantage de stockage externe est qu’il a généralement beaucoup plus d’espace pour les fichiers que le stockage interne. Toutefois, le stockage externe n’est pas toujours garanti pour être présent sur un périphérique et peut-être nécessiter une autorisation spéciale de l’utilisateur à y accéder.

> [!NOTE]
> Pour les appareils qui prennent en charge de plusieurs utilisateurs, Android fournira à chaque utilisateur son propre répertoire sur le stockage interne et externe. Ce répertoire n’est pas accessible à d’autres utilisateurs sur l’appareil. Cette séparation est invisible pour les applications tant qu’ils le font pas coder en dur chemins d’accès aux fichiers sur le stockage interne ou externe.

En règle générale, les applications Xamarin.Android doivent préférer l’enregistrement de leurs fichiers sur un stockage interne lorsqu’il est raisonnable et s’appuient sur un stockage externe lorsque des fichiers doivent être partagés avec d’autres applications, sont très volumineux ou doivent être conservés même si l’application est désinstallée. Par exemple, un fichier de configuration est idéal pour un stockage interne car il ne possède pas d’importance, à l’exception à l’application qui le crée.  En revanche, les photos sont un bon candidat pour le stockage externe. Ils peuvent être très volumineux et dans de nombreux cas, l’utilisateur doit pouvoir les partager ou y accéder, même si l’application est désinstallée.

Ce guide met l’accent sur un stockage interne. Veuillez consulter le guide [stockage externe](~/android/platform/files/external-storage.md) pour plus d’informations sur l’utilisation de stockage externe dans une application Xamarin.Android.

## <a name="working-with-internal-storage"></a>Utilisation de stockage interne

Le répertoire de stockage interne pour une application est déterminé par le système d’exploitation et est exposé à des applications Android par le `Android.Content.Context.FilesDir` propriété. Ceci renverra un `Java.IO.File` objet représentant le répertoire Android a dédié exclusivement à l’application.  Par exemple, une application avec le nom du package **com.companyname** le répertoire de stockage interne peut être :

```bash
/data/user/0/com.companyname/files
```

Ce document fait référence dans le répertoire de stockage interne en tant que _interne\_stockage_.

> [!IMPORTANT]
> Le chemin d’accès exact dans le répertoire de stockage interne peut varier à partir de l’appareil à l’appareil et entre les versions d’Android. Pour cette raison, les applications ne doivent pas dur le chemin d’accès dans le répertoire de stockage de fichiers internes de code et à la place utiliser les APIs Xamarin.Android, comme `System.Environment.GetFolderPath()`.

Pour optimiser le partage de code, les applications Xamarin.Android (ou les applications Xamarin.Forms ciblant Xamarin.Android) doivent utiliser le [ `System.Environment.GetFolderPath()` ](xref:System.Environment.GetFolderPath*) (méthode). Dans Xamarin.Android, cette méthode retourne une chaîne d’un répertoire qui est le même emplacement que `Android.Content.Context.FilesDir`. Cette méthode prend un enum, `System.Environment.SpecialFolder`, qui est utilisé pour identifier un ensemble de constantes énumérées qui représentent les chemins d’accès de dossiers spéciaux utilisés par le système d’exploitation. Pas toutes la `System.Environment.SpecialFolder` valeurs mapperont à un répertoire valide sur Xamarin.Android. Le tableau suivant décrit le chemin d’accès peut se produire pour une valeur donnée de `System.Environment.SpecialFolder`:

| System.Environment.SpecialFolder | Chemin d’accès  |
|----------------------|---|
| `ApplicationData` | **_INTERNE\_stockage_/.config** |
| `Desktop` | **_INTERNE\_stockage_  /bureau** |
| `LocalApplicationData` | **_INTERNE\_stockage_/.local/share** |
| `MyComputer` | **_INTERNE\_stockage_/.local/share** |
| `MyDocuments` | **_INTERNE\_STOCKAGE_** |
| `MyMusic` | **_INTERNE\_stockage_/Music** |
| `MyPictures` | **_INTERNE\_stockage_/Music** |
| `MyVideos` | **_INTERNE\_stockage_/Videos** |
| `Personal` | **_INTERNE\_STOCKAGE_** |


### <a name="reading-or-writing-to-files-on-internal-storage"></a>Lecture ou écriture dans des fichiers sur un stockage interne

Un de la [ C# API pour l’écriture](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file) dans un fichier sont suffisantes ; tout ce qui est nécessaire consiste à obtenir le chemin d’accès au fichier qui se trouve dans le répertoire alloué à l’application. Il est fortement recommandé que l’opération asynchrone de versions des API .NET sont utilisées pour réduire les problèmes qui peuvent être associer avec accès au fichier bloquer le thread principal.

Cet extrait de code est un exemple d’écriture d’un entier dans un fichier de texte UTF-8 dans le répertoire de stockage interne d’une application :

```csharp
public async Task SaveCountAsync(int count)
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");
    using (var writer = File.CreateText(backingFile))
    {
        await writer.WriteLineAsync(count.ToString());
    }
}
```

L’extrait de code suivant fournit un moyen de lire une valeur entière qui a été stockée dans un fichier texte :

```csharp
public async Task<int> ReadCountAsync()
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");

    if (backingFile == null || !File.Exists(backingFile))
    {
        return 0;
    }

    var count = 0;
    using (var reader = new StreamReader(backingFile, true))
    {
        string line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (int.TryParse(line, out var newcount))
            {
                count = newcount;
            }
        }
    }

    return count;
}
```

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>À l’aide de Xamarin.Essentials &ndash; assistances de système de fichiers

[Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android) est un ensemble d’API pour écrire du code compatible inter-plateformes. Le [assistances de système de fichiers](~/essentials/file-system-helpers.md?context=xamarin/android) est une classe qui contient une série de programmes d’assistance pour simplifier la localisation des répertoires de cache et les données de l’application. Cet extrait de code fournit un exemple montrant comment rechercher le répertoire de stockage interne et le répertoire de cache pour une application :

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>Masquage des fichiers à partir de la `MediaStore`

Le `MediaStore` est un composant Android qui collecte les données de métadonnées sur les fichiers multimédia (vidéos, musique, images) sur un appareil Android. Son objectif est simplifier le partage de ces fichiers dans toutes les applications Android sur l’appareil.

Fichiers privés ne s’affichera pas en tant que média partageable. Par exemple, si une application enregistre une image dans son stockage externe privé, puis ce fichier ne sera pas être récupéré par le scanneur de média (`MediaStore`).

Fichiers publics seront prélevés par `MediaStore`. Répertoires qui ont un nom de fichier octets zéro **. NOMEDIA** ne seront pas analysés par `MediaStore`.

## <a name="related-links"></a>Liens associés

* [Stockage externe](~/android/platform/files/external-storage.md)
* [Enregistrer les fichiers sur le stockage de l’appareil](https://developer.android.com/training/data-storage/files)
* [Assistances de système de fichiers de Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android)
* [Données utilisateur de sauvegarde avec la sauvegarde automatique](https://developer.android.com/guide/topics/data/autobackup)
* [Stockage mise](https://source.android.com/devices/storage/adoptable)
