---
title: Comment puis-je installer manuellement les bibliothèques de prise en charge Android requises par les packages Xamarin.Android.Support ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 84ee33fe174c01656144e55bc3cbba7c773950fd
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120643"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Comment puis-je installer manuellement les bibliothèques de prise en charge Android requises par les packages Xamarin.Android.Support ?

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Exemples d’étapes pour Xamarin.Android.Support.v4 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Télécharger le package NuGet de Xamarin.Android.Support souhaité (par exemple en l’installant avec le Gestionnaire de package NuGet).

Utilisez `ildasm` pour vérifier quelle version de **android_m2repository.zip** le package NuGet a besoin :

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```
Exemple de sortie :

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

Télécharger **android\_m2repository.zip** à partir de Google à l’aide de l’URL retournée à partir de **ildasm**. Vous pouvez également vérifier la version de la _Android Support Repository_ actuellement installée dans le gestionnaire Android SDK :

![« Gestionnaire android SDK montrant le référentiel de prise en charge Android version 32 installé »](install-android-support-library-images/sdk-extras.png)

Si la version correspond à celui que vous avez besoin pour le package NuGet, puis il est inutile de télécharger quoi que ce soit de nouveau. Vous pouvez à la place nouveau compresser existant **m2repository** répertoire se trouve sous **extras\\android** dans le _chemin d’accès du Kit de développement logiciel_ (comme indiqué en haut de l’Android Fenêtre Gestionnaire de kit de développement logiciel).

Calculer le hachage MD5 de l’URL retournée par **ildasm**. Mettre en forme la chaîne résultante pour utiliser toutes les lettres majuscules et sans espaces. Par exemple, ajuster la `$url` variable en tant que nécessaire, puis exécutez les 2 lignes suivantes (selon [l’original C# code à partir de Xamarin.Android](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)) dans PowerShell :

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```
Exemple de sortie :

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

Copie **android\_m2repository.zip** dans le **% LocalAppData%\\Xamarin\\zips\\**  dossier. Renommez le fichier pour utiliser le hachage MD5 du hachage MD5 précédente étape de calcul. Exemple :

**%LocalAppData%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(Facultatif) Décompressez le fichier dans **% LocalAppData%\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\contenu\\**  (création d’un **contenu\\m2repository** sous-répertoire). Si vous ignorez cette étape, la première build qui utilise la bibliothèque prendra un peu plus de temps, car il doit effectuer cette étape.
Le numéro de version du sous-répertoire (**23.4.0.0** dans cet exemple) n’est pas tout à fait identique à la version du package NuGet. Vous pouvez utiliser `ildasm` pour trouver le numéro de version correct :

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```
Exemple de sortie :

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio pour Mac](#tab/macos)

Télécharger le package NuGet de Xamarin.Android.Support souhaité (par exemple en l’installant avec le Gestionnaire de package NuGet).

Double-cliquez sur le _Xamarin.Android.Support.v4_ assembly sous le _références_ section du projet Android dans Visual Studio pour Mac ouvrir l’assembly dans le navigateur de l’Assembly. Vérifiez que le _langage_ liste déroulante est définie sur _C#_ et sélectionnez le niveau supérieur _Xamarin.Android.Support.v4_ assembly à partir de l’arborescence de navigation du navigateur de l’Assembly . Recherchez le `SourceUrl` propriété sous un de le `IncludeAndroidResourcesFrom` ou `JavaLibraryReference` attributs :

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

Télécharger **android\_m2repository.zip** à partir de Google à l’aide de la `SourceUrl` retourné à partir de **ildasm**. Vous pouvez également vérifier la version de la _Android Support Repository_ actuellement installée dans le gestionnaire Android SDK :

![« Gestionnaire android SDK montrant le référentiel de prise en charge Android version 32 installé »](install-android-support-library-images/sdk-extras.png)

Si la version correspond à celui que vous avez besoin pour le package NuGet, puis il est inutile de télécharger quoi que ce soit de nouveau. Vous pouvez à la place nouveau compresser existant **m2repository** répertoire se trouve sous **extras/android** dans le _chemin d’accès du Kit de développement logiciel_ (comme indiqué en haut de la fenêtre Gestionnaire Android SDK) .

Calculer le hachage MD5 de l’URL retournée par **ildasm**. Mettre en forme la chaîne résultante pour utiliser toutes les lettres majuscules et sans espaces. Par exemple, ajuster la chaîne d’URL selon vos besoins et puis exécutez la commande suivante une **Terminal.app** invite de commandes :

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

Une autre option consiste à utiliser le `csharp` interpréteur pour exécuter [le même C# code Xamarin.Android lui-même utilise](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208).
Pour ce faire, vous devez ajuster le `url` variable en tant que nécessaire, puis exécutez la commande suivante dans une **Terminal.app** invite de commandes :

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```
Exemple de sortie :

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

Copie **android\_m2repository.zip** à la **$HOME/.local/share/Xamarin/zips/** dossier. Renommez le fichier pour utiliser le hachage MD5 du hachage MD5 précédente étape de calcul. Exemple :

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(Facultatif) Décompressez le fichier dans : 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(création d’un **contenu/m2repository** sous-répertoire). Si vous ignorez cette étape, la première build qui utilise la bibliothèque prendra un peu plus de temps, car il doit effectuer cette étape.

Le numéro de version du sous-répertoire (**23.4.0.0** dans cet exemple) n’est pas tout à fait identique à la version du package NuGet. Comme dans le **ildasm** étape plus tôt, vous pouvez utiliser le navigateur de l’Assembly dans Visual Studio pour Mac pour trouver le numéro de version correct. Recherchez le `Version` propriété sous un de le `IncludeAndroidResourcesFrom` ou `JavaLibraryReference` attributs :

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----


## <a name="additional-references"></a>Références supplémentaires

- [Bogue 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) – imprécis « échoué du téléchargement. Téléchargez {0} et le placer la {1} directory. » et « installez le paquet : '{0}' disponibles dans le programme d’installation du Kit de développement logiciel « messages d’erreur liés aux packages Xamarin.Android.Support

### <a name="next-steps"></a>Étapes suivantes

Ce document décrit le comportement actuel depuis août 2016. La technique décrite dans ce document ne fait pas partie de la suite de test stable pour Xamarin, donc elle peut décomposer à l’avenir.

Pour obtenir une assistance supplémentaire, pour nous contacter, ou si le reste de ce problème même après utilisant les informations ci-dessus, veuillez consulter [quelles options de prise en charge sont disponibles pour Xamarin ?](~/cross-platform/troubleshooting/support-options.md) pour plus d’informations sur les options de contact, des suggestions, ainsi que comment signaler un bogue nouveau si nécessaire.

