---
title: Polices
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: d4ad9dde4004440985ff247d2f986ede385f981f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="fonts"></a>Polices


## <a name="overview"></a>Vue d'ensemble

À partir de niveau de l’API 26, le SDK Android autorise les polices devant être traitée en tant que ressources, tout comme des dispositions ou drawables. Le [Android prise en charge de bibliothèque 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) sera backport la nouvelle police de l’API pour les applications qui ciblent le niveau de l’API 14 ou une version ultérieure.

Après avoir ciblé API 26 ou l’installation de la bibliothèque de prise en charge Android v26, il existe deux façons d’utiliser des polices dans une application Android :

1. **Package de la police en tant que Android ressource** &ndash; ainsi que la police est toujours disponible pour l’application, mais augmente la taille de APK. 
2. **Télécharger les polices** &ndash; Android prend également en charge le téléchargement d’une police dans un _fournisseur de police_. Le fournisseur de police vérifie si la police est déjà sur l’appareil. Si nécessaire, la police sera téléchargée et mis en cache sur l’appareil. Cette police peut être partagée entre plusieurs applications.

Polices similaires (ou une police qui peut-être avoir plusieurs styles différents) peut-être être regroupée en _familles de polices_. Cela permet aux développeurs de spécifier certains attributs de la police, telles que son poids et Android sélectionne automatiquement la police appropriée à partir de la famille de polices.

La bibliothèque de prise en charge Android v26 sera prise en charge de backport de polices à niveau de l’API 26. Lorsque vous ciblez les niveaux d’API plus anciens, il est nécessaire de déclarer le `app` espace de noms XML et nommer les différents attributs de police à l’aide de la `android:` espace de noms et le `app:` espace de noms. Si seul le `android:` l’espace de noms, puis les polices ne sera pas affichée des périphériques qui exécutent le niveau de l’API inférieur ou égal à 25. Par exemple, cet extrait de code XML déclare un nouveau [ _famille de polices_ ](#font_families) ressource qui fonctionne dans le niveau de l’API 14 et versions ultérieures :

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

     <font  android:font="@font/sourcesanspro_regular" 
            android:fontStyle="normal" 
            android:fontWeight="400"
            app:font="@font/sourcesanspro_regular" 
            app:fontStyle="normal" 
            app:fontWeight="400" />

</font-family>    
```

Tant que d’une manière appropriée, les polices sont fournies à une application Android, peuvent être appliquées à un widget de l’interface utilisateur en définissant le [ `fontFamily` attribut](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily). Par exemple, l’extrait de code suivant montre comment afficher une police dans un contrôle TextView :

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/caveat_bold"
    app:fontFamily="@font/caveat_bold"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Ce guide sera tout d’abord expliquent comment utiliser des polices comme une ressource Android et passez à expliquer comment télécharger les polices lors de l’exécution.


## <a name="fonts-as-a-resource"></a>Polices en tant que ressource

Empaquetage d’une police dans un fichier APK Android garantit qu’il est toujours disponible pour l’application. Un fichier de polices (soit un. TTF ou un. Fichier OTF) est ajouté à une application de Xamarin.Android comme toute autre ressource, en copiant les fichiers dans un sous-répertoire dans le **ressources** dossier d’un projet Xamarin.Android. Ressources de polices sont conservées dans un **police** du sous-répertoire de la **ressources** dossier du projet. 


> [!NOTE]
>  Les polices doivent avoir un **Action de génération** de **AndroidResource** ou qu’ils ne seront pas compressés dans APK finale. L’action de génération doit être automatiquement définie par l’IDE.

Lorsqu’il existe plusieurs fichiers de polices similaires (par exemple, la même police avec différents poids ou les styles), il est possible de les regrouper dans une famille de polices.

<a name="font_families" />

### <a name="font-families"></a>Familles de polices

Une famille de polices est un jeu de polices qui ont des styles et le poids. Par exemple, il peut être des fichiers de polices distincts des polices de caractères gras ou italiques. La famille de polices est définie par `font` éléments dans un fichier XML qui est conservée dans le **ressources/police** active. Chaque famille de polices doit avoir son propre fichier XML.

Pour créer une famille de polices, tout d’abord ajouter toutes les polices à la **ressources/police** dossier. Ensuite, créez un nouveau fichier XML dans le dossier des polices pour la famille de polices. Ce fichier XML aura une racine `font-family` élément qui contient un ou plusieurs `font` éléments. Chaque `font` élément déclare les attributs d’une police. 

Le code XML suivant est un exemple d’une famille de polices pour le _Sources SAN Pro_ police qui définit les nombreux poids de police différente. Elle est enregistrée en tant que fichier dans le **ressources/police** dossier nommé **sourcesanspro.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto">
    <font android:font="@font/sourcesanspro_regular" 
          android:fontStyle="normal" 
          android:fontWeight="400"
          app:font="@font/sourcesanspro_" 
          app:fontStyle="normal" 
          app:fontWeight="400" />
    <font android:font="@font/sourcesanspro_bold" 
          android:fontStyle="normal" 
          android:fontWeight="800" 
          app:font="@font/sourcesanspro_bold" 
          app:fontStyle="normal" 
          app:fontWeight="800" />
    <font android:font="@font/sourcesanspro_italic" 
          android:fontStyle="italic" 
          android:fontWeight="400"
          app:font="@font/sourcesanspro_italic" 
          app:fontStyle="italic" 
          app:fontWeight="400" />
</font-family>
```

Le `fontStyle` attribut a deux valeurs possibles :

* **normal** &ndash; une police normale
* **italique** &ndash; une police en italique

Le `fontWeight` attribut correspond à la feuille de style `font-weight` d’attribut et fait référence à l’épaisseur de la police. Il s’agit d’une valeur dans la plage de 100 à 900. La liste suivante décrit les valeurs de poids de police courantes et leur nom :

* **Dynamique** &ndash; 100
* **Très clair** &ndash; 200
* **Lumière** &ndash; 300
* **Normal** &ndash; 400
* **Support** &ndash; 500
* **Semi gras** &ndash; 600
* **Bold** &ndash; 700
* **Extra Bold** &ndash; 800
* **Noir** &ndash; 900

Une fois qu’une famille de polices a été définie, il peut être utilisé de manière déclarative en définissant le `fontFamily`, `textStyle`, et `fontWeight` les attributs dans le fichier de disposition.  Par exemple, l’extrait de code XML suivant définit une police de 400 poids (normale) et un style de texte en italique :

```xml
<TextView
    android:text="Sans Source Pro semi-bold italic, 600 weight, italic"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:fontFamily="@font/sourcesanspro"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_horizontal"
    android:fontWeight="600"
    android:textStyle="italic"
    />
```


### <a name="programmatically-assigning-fonts"></a>Assignation par programme des polices

Polices peuvent être définies par programme à l’aide de la [ `Resources.GetFont` ](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int)) méthode pour récupérer un [ `Typeface` ](https://developer.android.com/reference/android/graphics/Typeface.html) objet. De nombreuses vues ont un `TypeFace` propriété qui peut être utilisée pour affecter la police pour le widget. Cet extrait de code montre comment définir par programme la police sur TextView :

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

Le `GetFont` méthode chargera automatiquement la la première police au sein d’une famille de polices.  Pour charger une police qui correspond à un style spécifique, utilisez le `Typeface.Create` (méthode). Cette méthode tente de charger une police qui correspond au style spécifié. Par exemple, cet extrait de code essaie de charger un gras `Typeface` objet à partir d’une famille de polices qui est définie dans **ressources/polices**:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```


## <a name="downloading-fonts"></a>Téléchargement de polices

Au lieu d’empaquetage de polices en tant que ressource d’application, Android permettre télécharger des polices à partir d’une source distante. Cela aura un effet souhaitable de réduire la taille de APK. 

Les polices sont téléchargées à l’aide d’un _fournisseur de police_. Il s’agit d’un fournisseur de contenu spécialisé qui gère le téléchargement et la mise en cache de polices à toutes les applications sur l’appareil. 8.0 Android comprend un fournisseur de police pour télécharger des polices à partir de la [Google police référentiel](http://fonts.google.com). Ce fournisseur de la police par défaut est utilisées sur le niveau d’API avec la bibliothèque de prise en charge Android v26 14.
 
 Lorsqu’une application effectue une demande pour une police, le fournisseur de la police tout d’abord vérifie si la police est déjà sur l’appareil. Si ce n’est pas le cas, il tente alors de télécharger la police. Si la police ne peut pas être téléchargé, puis Android utilise la police système par défaut. Une fois que la police a été téléchargée, il est disponible pour toutes les applications sur l’appareil, et pas seulement l’application qui a effectué la demande initiale.

Lorsqu’une demande est faite pour télécharger une police, l’application n’interroge pas directement le fournisseur de la police. Au lieu de cela, les applications utilisent une instance de la [ `FontsContract` ](https://developer.android.com/reference/android/provider/FontsContract.html) API (ou le [ `FontsContractCompat` ](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) si le 26 de bibliothèque de prise en charge est utilisé).  

8.0 Android prend en charge le téléchargement de polices de deux manières différentes :

1. **Déclarer des polices à télécharger en tant que ressource** &ndash; une application peut déclarer des polices téléchargeables pour Android via des fichiers de ressources XML. Ces fichiers contiennent toutes les métadonnées qui Android doit télécharger de façon asynchrone les polices lorsque l’application démarre et les mettre en cache sur l’appareil.
2. **Par programme** &ndash; API au niveau de l’API Android 26 permettre à une application télécharger les polices par programme, tandis que l’application est en cours d’exécution. Applications créera un `FontRequest` de l’objet d’une police donnée et transmettre cet objet à la `FontsContract` classe. Le `FontsContract` prend le `FontRequest` et récupère la police à partir d’un _fournisseur de police_. Android télécharge de façon synchrone la police. Un exemple de création d’un `FontRequest` s’affichera plus loin dans ce guide.

Quelle que soit l’approche qui est utilisée, les fichiers de ressources qui doivent être ajoutés à l’application de Xamarin.Android avant les polices peuvent être téléchargés. Tout d’abord, la police doit être déclarée dans un fichier XML dans le **ressources/police** active en tant que partie d’une famille de polices. Cet extrait de code est un exemple montrant comment télécharger les polices à partir de la [collection de polices de Google Open Source](https://fonts.google.com) l’aide du fournisseur de police par défaut qui est fourni avec Android 8.0 (ou bibliothèque de prise en charge v26) :

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto"
             android:fontProviderAuthority="com.google.android.gms.fonts" 
             android:fontProviderPackage="com.google.android.gms" 
             android:fontProviderQuery="VT323" 
             android:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
             app:fontProviderAuthority="com.google.android.gms.fonts" 
             app:fontProviderPackage="com.google.android.gms" 
             app:fontProviderQuery="VT323"
             app:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
    >
</font-family>
```

Le `font-family` élément contient les attributs suivants, en déclarant les informations requises pour que Android pour télécharger les polices :
 
1. **fontProviderAuthority** &ndash; l’autorité du fournisseur de police à utiliser pour la demande.
2. **fontPackage** &ndash; le package pour le fournisseur de la police à utiliser pour la demande. Cela permet de vérifier l’identité du fournisseur.
3. **fontQuery** &ndash; il s’agit d’une chaîne qui aide le fournisseur de la police de localiser la police demandée. Pour plus d’informations sur la requête de police sont spécifiques au fournisseur de la police. Le [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) classe dans le [polices téléchargeables](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) exemple d’application fournit des informations sur le format de requête pour les polices de la Collection Source ouvert de Google polices.
4. **fontProviderCerts** &ndash; avec la liste des ensembles de hachages pour les certificats que le fournisseur doit être signé avec un tableau des ressources.

Une fois les polices sont définis, il peut être nécessaire de fournir des informations sur la _les certificats de police_ impliqués dans le téléchargement.


### <a name="font-certificates"></a>Certificats de police

Si le fournisseur de la police n’est pas préinstallé sur l’appareil, ou si l’application utilise le `Xamarin.Android.Support.Compat` la bibliothèque, Android requiert des certificats de sécurité du fournisseur de police. Ces certificats sont listés dans un fichier de ressources de tableau est conservé dans **ressources/valeurs** active. 

Par exemple, le code XML suivant est nommé **Resources/values/fonts_cert.xml** et stocke les certificats pour le fournisseur de police de Google : 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="com_google_android_gms_fonts_certs">
        <item>@array/com_google_android_gms_fonts_certs_dev</item>
        <item>@array/com_google_android_gms_fonts_certs_prod</item>
    </array>
    <string-array name="com_google_android_gms_fonts_certs_dev">
        <item>
            MIIEqDCCA5CgAwIBAgIJANWFuGx90071MA0GCSqGSIb3DQEBBAUAMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTAeFw0wODA0MTUyMzM2NTZaFw0zNTA5MDEyMzM2NTZaMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBANbOLggKv+IxTdGNs8/TGFy0PTP6DHThvbbR24kT9ixcOd9W+EaBPWW+wPPKQmsHxajtWjmQwWfna8mZuSeJS48LIgAZlKkpFeVyxW0qMBujb8X8ETrWy550NaFtI6t9+u7hZeTfHwqNvacKhp1RbE6dBRGWynwMVX8XW8N1+UjFaq6GCJukT4qmpN2afb8sCjUigq0GuMwYXrFVee74bQgLHWGJwPmvmLHC69EH6kWr22ijx4OKXlSIx2xT1AsSHee70w5iDBiK4aph27yH3TxkXy9V89TDdexAcKk/cVHYNnDBapcavl7y0RiQ4biu8ymM8Ga/nmzhRKya6G0cGw8CAQOjgfwwgfkwHQYDVR0OBBYEFI0cxb6VTEM8YYY6FbBMvAPyT+CyMIHJBgNVHSMEgcEwgb6AFI0cxb6VTEM8YYY6FbBMvAPyT+CyoYGapIGXMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbYIJANWFuGx90071MAwGA1UdEwQFMAMBAf8wDQYJKoZIhvcNAQEEBQADggEBABnTDPEF+3iSP0wNfdIjIz1AlnrPzgAIHVvXxunW7SBrDhEglQZBbKJEk5kT0mtKoOD1JMrSu1xuTKEBahWRbqHsXclaXjoBADb0kkjVEJu/Lh5hgYZnOjvlba8Ld7HCKePCVePoTJBdI4fvugnL8TsgK05aIskyY0hKI9L8KfqfGTl1lzOv2KoWD0KWwtAWPoGChZxmQ+nBli+gwYMzM1vAkP+aayLe0a1EQimlOalO762r0GXO0ks+UeXde2Z4e+8S/pf7pITEI/tP+MxJTALw9QUWEv9lKTk+jkbqxbsh8nfBUapfKqYn0eidpwq2AzVp3juYl7//fKnaPhJD9gs=
        </item>
    </string-array>
    <string-array name="com_google_android_gms_fonts_certs_prod">
        <item>
            MIIEQzCCAyugAwIBAgIJAMLgh0ZkSjCNMA0GCSqGSIb3DQEBBAUAMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDAeFw0wODA4MjEyMzEzMzRaFw0zNjAxMDcyMzEzMzRaMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBAKtWLgDYO6IIrgqWbxJOKdoR8qtW0I9Y4sypEwPpt1TTcvZApxsdyxMJZ2JORland2qSGT2y5b+3JKkedxiLDmpHpDsz2WCbdxgxRczfey5YZnTJ4VZbH0xqWVW/8lGmPav5xVwnIiJS6HXk+BVKZF+JcWjAsb/GEuq/eFdpuzSqeYTcfi6idkyugwfYwXFU1+5fZKUaRKYCwkkFQVfcAs1fXA5V+++FGfvjJ/CxURaSxaBvGdGDhfXE28LWuT9ozCl5xw4Yq5OGazvV24mZVSoOO0yZ31j7kYvtwYK6NeADwbSxDdJEqO4k//0zOHKrUiGYXtqw/A0LFFtqoZKFjnkCAQOjgdkwgdYwHQYDVR0OBBYEFMd9jMIhF1Ylmn/Tgt9r45jk14alMIGmBgNVHSMEgZ4wgZuAFMd9jMIhF1Ylmn/Tgt9r45jk14aloXikdjB0MQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEUMBIGA1UEChMLR29vZ2xlIEluYy4xEDAOBgNVBAsTB0FuZHJvaWQxEDAOBgNVBAMTB0FuZHJvaWSCCQDC4IdGZEowjTAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBBAUAA4IBAQBt0lLO74UwLDYKqs6Tm8/yzKkEu116FmH4rkaymUIE0P9KaMftGlMexFlaYjzmB2OxZyl6euNXEsQH8gjwyxCUKRJNexBiGcCEyj6z+a1fuHHvkiaai+KL8W1EyNmgjmyy8AW7P+LLlkR+ho5zEHatRbM/YAnqGcFh5iZBqpknHf1SKMXFh4dd239FJ1jWYfbMDMy3NS5CTMQ2XFI1MvcyUTdZPErjQfTbQe3aDQsQcafEQPD+nqActifKZ0Np0IS9L9kR/wbNvyz6ENwPiTrjV2KRkEjH78ZMcUQXg0L3BYHJ3lc69Vs5Ddf9uUGGMYldX3WfMBEmh/9iFBDAaTCK
        </item>
    </string-array>
</resources>
```

Ces fichiers de ressources en place, l’application est capable de télécharger les polices.


### <a name="declaring-downloadable-fonts-as-resources"></a>Déclarer des polices à télécharger en tant que ressources

En répertoriant les polices téléchargeables dans le **AndroidManifest.XML**, Android asynchrone télécharge les polices au premier démarrage de l’application. La police d’eux-mêmes sont répertoriés dans un fichier de ressources de tableau, semblable à celle-ci : 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```        

Pour télécharger ces polices, ils doivent être déclarés dans **AndroidManifest.XML** en ajoutant `meta-data` en tant qu’enfant de le `application` élément. Par exemple, si les polices téléchargeables sont déclarés dans un fichier de ressources à **Resources/values/downloadable_fonts.xml**, puis cet extrait de code devra être ajouté au manifeste : 

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```


### <a name="downloading-a-font-with-the-font-apis"></a>Téléchargement d’une police avec les API de police

Il est possible de programmer le téléchargement d’une police en instanciant une [ `FontRequest` ](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html) objet et en passant à la `FontContractCompat.RequestFont` (méthode). Le `FontContractCompat.RequestFont` méthode Vérifiez tout d’abord si la police existe déjà sur l’appareil et ensuite si nécessaire arrivera asynchrone requête du fournisseur de la police et essayez de télécharger la police pour l’application. Si `FontRequest` ne peut pas télécharger de la police, Android utilise la police système par défaut. 

A `FontRequest` objet contient des informations qui servira par le fournisseur de la police pour rechercher et télécharger une police. A `FontRequest` nécessite quatre éléments d’information :

1. **Autorité de fournisseur de police** &ndash; l’autorité du fournisseur de police à utiliser pour la demande.
2. **Package de polices** &ndash; le package pour le fournisseur de la police à utiliser pour la demande. Cela permet de vérifier l’identité du fournisseur.
3. **Requête de police** &ndash; il s’agit d’une chaîne qui aide le fournisseur de la police de localiser la police demandée. Pour plus d’informations sur la requête de police sont spécifiques au fournisseur de la police. Les détails de la chaîne sont spécifiques au fournisseur de la police. Le [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) classe dans le [polices téléchargeables](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) exemple d’application fournit des informations sur le format de requête pour les polices de la Collection Source ouvert de Google polices.
4. **Certificats de fournisseur de police** &ndash; avec la liste des ensembles de hachages pour les certificats que le fournisseur doit être signé avec un tableau des ressources. 

Cet extrait de code est un exemple d’instanciation d’un nouvel `FontRequest` objet :

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

Dans l’extrait de code précédent `FontToDownload` est une requête qui vous aide à la police de la collection de polices de Google Open Source. 

Avant de passer le `FontRequest` à la `FontContractCompat.RequestFont` (méthode), il existe deux objets qui doivent être créés :

* **`FontsContractCompat.FontRequestCallback`** &ndash; Il s’agit d’une classe abstraite qui doit être étendue. Il s’agit d’un rappel qui sera appelé lorsque `RequestFont` est terminé. Une application de Xamarin.Android doit sous-classer `FontsContractCompat.FontRequestCallback` et remplacez le `OnTypefaceRequestFailed` et `OnTypefaceRetrieved`, qui fournit les actions à entreprendre lorsque le téléchargement échoue ou réussit respectivement.
* **`Handler`** &ndash; Il s’agit d’un `Handler` qui sera utilisé par `RequestFont` pour télécharger la police sur un thread, si nécessaire. Polices doivent **pas** être téléchargé sur le thread d’interface utilisateur.  

Cet extrait de code est un exemple d’une classe c# qui permet de télécharger façon asynchrone une police à partir de la collection de polices de Google Open Source. Il implémente la `FontRequestCallback` de l’interface et déclenche un événement c# lorsque `FontRequest` terminée. 


```csharp
public class FontDownloadHelper : FontsContractCompat.FontRequestCallback
{
    // A very simple font query; replace as necessary
    public static readonly String FontToDownload = "Courgette";
    
    Android.OS.Handler Handler = null;

    public event EventHandler<FontDownloadEventArg> FontDownloaded = delegate
    {
        // just an empty delegate to avoid null reference exceptions.  
    };


    public void DownloadFonts(Context context)
    {
        FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms",FontToDownload , Resource.Array.com_google_android_gms_fonts_certs);
        FontsContractCompat.RequestFont(context, request, this, GetHandlerThreadHandler());
    }

    public override void OnTypefaceRequestFailed(int reason)
    {
        base.OnTypefaceRequestFailed(reason);
        FontDownloaded(this, new FontDownloadEventArg(null));
    }

    public override void OnTypefaceRetrieved(Android.Graphics.Typeface typeface)
    {
        base.OnTypefaceRetrieved(typeface);
        FontDownloaded(this, new FontDownloadEventArg(typeface));
    }
    
    Handler GetHandlerThreadHandler()
    {
        if (Handler == null)
        {
            HandlerThread handlerThread = new HandlerThread("fonts");
            handlerThread.Start();
            Handler = new Handler(handlerThread.Looper);
        }
        return Handler;
    }
}

public class FontDownloadEventArg : EventArgs
{
    public FontDownloadEventArg(Android.Graphics.Typeface typeface)
    {
        Typeface = typeface;
    }
    public Android.Graphics.Typeface Typeface { get; private set; }
    public bool RequestFailed
    {
        get
        {
            return Typeface != null;
        }
    }
}
```



Pour utiliser cette application d’assistance, un nouveau `FontDownloadHelper` est créé, et un gestionnaire d’événements est attribué :  
```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) => 
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```


## <a name="summary"></a>Récapitulatif

Ce guide décrit les nouvelles API dans Android 8.0 pour prendre en charge les polices à télécharger et les polices en tant que ressources. Il décrit comment incorporer des polices existants dans un fichier APK et leur utilisation dans une disposition. Il décrit également comment 8.0 Android prend en charge le téléchargement de polices à partir d’un fournisseur de la police, par programme ou en déclarant les métadonnées police dans les fichiers de ressources. 


## <a name="related-links"></a>Liens associés

- [fontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [FontsContractCompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources.GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [Typeface](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Prise en charge Android bibliothèque 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Utilisation de polices dans Android](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [Spécification de poids de police CSS](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Collection de polices de Google Open Source](https://fonts.google.com/)
- [Source Sans Pro](https://fonts.google.com/specimen/Source+Sans+Pro)
