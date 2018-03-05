---
title: "Icônes de Document personnalisées"
description: "Cet article explique comment y compris et la gestion d’un composant de l’image dans une application Xamarin.iOS à utiliser comme une icône de Type de Document personnalisé."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A3F3C94-2578-4F53-9B8E-25714F48BDD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/23/2017
ms.openlocfilehash: 582fcbacbf1959e05773babb1219817ba319a937
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="custom-document-icons"></a>Icônes de Document personnalisées

_Cet article explique comment y compris et la gestion d’un composant de l’image dans une application Xamarin.iOS à utiliser comme une icône de Type de Document personnalisé._

Si une application Xamarin.iOS prend en charge le chargement d’un type de document particulier, le développeur peut fournir des icônes que le système utilisera lorsqu’il rencontre ce type de document, par exemple quand un utilisateur appuie sur une pièce jointe dans le *Application de messagerie* en tant que illustrée ici :

 [ ![](custom-document-types-images/17.png "Un exemple d’icônes de type de document")](custom-document-types-images/17.png)

Le développeur peut ajouter des informations de type de document pour un format de fichier l’application est capable d’ouvrir en incluant des entrées de dictionnaire pour la `CFBundleTypeName` chaîne et `LSItemContentTypes` tableau dans l’application `Info.plist`. Les icônes pour le type de document aller dans le `CFBundleTypeIconFiles` tableau. Si une icône de document n’est pas fournie, iOS sera dériver l’une de l’icône d’application.
Icônes peuvent être fournies pour plusieurs tailles, optimisés pour les différentes résolutions d’appareil. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Pour attribuer ces valeurs dans Visual Studio pour Mac, utilisez le **Types de documents** sous le **avancé** onglet sur le `Info.plist` l’éditeur pour ajouter le type de document et de lui assigner des icônes. Par exemple, voici une capture d’écran montrant l’enregistrement pour la prise en charge PDF :

 [ ![](custom-document-types-images/18.png "La section Types de documents sous l’onglet Avancé de l’éditeur 'Info.plist'")](custom-document-types-images/18.png)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pour attribuer à ces valeurs dans Visual Studio, utilisez le **Types de documents** sous le **avancé** onglet sur le `Info.plist`:

 ![](custom-document-types-images/doc01w.png "Ouvrez la section Types de documents sous l’onglet Avancé")

Cliquez sur le **ajouter un Type de Document** bouton et renseignez les champs obligatoires :

![](custom-document-types-images/doc02w.png "L’écran Ajouter un Type de Document")

-----


Pour plus d’informations sur les types de document, consultez Apple [uniforme référence d’identificateurs de Type](http://developer.apple.com/library/ios/#documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html) et [Document Interaction programmation rubriques pour iOS](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html).


## <a name="related-links"></a>Liens associés

- [Utilisation d’Images (exemple)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Icône personnalisée et les instructions de création d’Image](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
