---
title: Prise en charge linguistique
description: Concepts et fonctionnalités de l’application multi-plateforme.
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: f22bb1b37f3e03f63c9e47cf71d5df57ade0042f
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="language-support"></a>Prise en charge linguistique

Cette section contient des documents qui expliquent des fonctionnalités d’application multiplateforme plus avancées et les concepts.

## <a name="c"></a>C# 
###  <a name="async-support-overviewcross-platformplatformasyncmd"></a>[Vue d’ensemble de la prise en charge asynchrone](~/cross-platform/platform/async.md)

La version 5 du langage c# a introduit deux nouveaux mots-clés pour exprimer des opérations asynchrones : async et await. Ces mots clés vous permettent d’écrire du code simple qui utilise la bibliothèque parallèle de tâches pour exécuter des opérations longues (par exemple, l’accès réseau) dans un autre thread et d’accéder facilement les résultats à la fin. Les dernières versions de Xamarin.iOS et Xamarin.Android prennent en charge async et await : ce document fournit des explications et un exemple d’utilisation de la nouvelle syntaxe avec Xamarin.

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[Fonctionnalités du langage C# 6](~/cross-platform/platform/csharp-six.md)

La dernière version du langage c# – version 6 – continue à faire évoluer la langue pour avoir moins de code réutilisable, améliorer la clarté et la cohérence plus. La syntaxe d’initialisation plus propre, la possibilité d’utiliser `await` dans `catch/finally` blocs et la valeur null-condition `?` opérateur sont particulièrement utiles.

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

Génération d’applications mobiles avec F # et Xamarin.

##  <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[Portable Visual Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Visual Studio prend en charge la création de bibliothèques de classes portables à l’aide de Visual Basic.NET qui peuvent ensuite être incorporées dans les applications Xamarin. Cet article explique comment créer une nouvelle bibliothèque de classes portables Visual Basic et alors l’utiliser dans un exemple d’application Xamarin.iOS, Xamarin.Android et Windows Phone.

##  <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Vues HTML de génération à l’aide de modèles Razor](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin permet aux développeurs de tirer parti du moteur de création de modèles Razor, introduit initialement avec ASP.NET MVC, ainsi que c# facilement combiner des données avec HTML, Javascript et CSS sans vous soucier de la création manuelle de chaînes HTML dans le code.
Cet article explique comment utiliser des modèles Razor avec Xamarin pour Android et iOS.
