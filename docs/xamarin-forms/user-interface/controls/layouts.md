---
title: Dispositions de Xamarin.Forms
description: "Présentation de Xamarin.Forms est utilisés pour composer des contrôles d’interface utilisateur dans les structures logiques."
ms.topic: article
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: ecea0f55360fcde7a50c52bb33c45a2c5fff5eeb
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-layouts"></a>Dispositions de Xamarin.Forms

_Présentation de Xamarin.Forms est utilisés pour composer des contrôles d’interface utilisateur dans les structures logiques._

<style>.tableimg {max-largeur : aucun ! important ;}</style>

## <a name="layouts"></a>Mises en page

Le [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) classe dans Xamarin.Forms est un sous-type spécialisé de vue, qui agit comme un conteneur pour d’autres dispositions ou des vues. Il contient généralement une logique pour définir la position et la taille des éléments enfants dans les applications de Xamarin.Forms.

 [ ![](layouts-images/layouts-sml.png "Types de mise en page Xamarin.Forms")](layouts-images/layouts.png "Types de disposition de Xamarin.Forms")

<br clear="all" />

Xamarin.Forms prend en charge :

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Type</strong>
    </th>
    <th>
      <strong>Description</strong>
    </th>
    <th style="min-width:400px">
      <strong>Screenshot</strong>
    </th>
  </thead>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a>
    </td>
    <td valign="top">
Un gestionnaire de disposition pour les vues basées sur un modèle. Utilisé dans un <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/">ControlTemplate</a> pour marquer où s’affiche le contenu à présenter.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/Templates/ControlTemplates/SimpleTheme/SimpleTheme/App.xaml"><img src="layouts-images/ContentPresenter.png" title="Exemple de ContentPresenter" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a>
    </td>
    <td valign="top">
Un élément avec un contenu unique. ContentView a très peu d’utiliser ses propres. Son objectif est de servir d’une classe de base pour les vues composées définie par l’utilisateur.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentViewDemoPage.cs"><img src="layouts-images/ContentView.png" title="Exemple de ContentView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">Frame</a>
    </td>
    <td valign="top">
Un élément contenant un seul enfant, avec certaines options de trame. Frame ont une valeur par défaut <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/">Xamarin.Forms.Layout.Padding</a> de 20.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/FrameDemoPage.cs"><img src="layouts-images/Frame.png" title="Exemple de frame" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>
    </td>
    <td valign="top">
Un élément capable de défilement s’il s’agit de contenu requiert.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ScrollViewDemoPage.cs"><img src="layouts-images/ScrollView.png" title="Exemple de ScrollView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a>
    </td>
    <td valign="top">
Un élément qui affiche le contenu avec un modèle de contrôle et de la classe de base pour <a href=""/api/type/Xamarin.Forms.ContentView/">ContentView</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="layouts-images/TemplatedView.png" title="Exemple de TemplatedView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a>
    </td>
    <td valign="top">
Positionne les éléments enfants en position demandée absolue. Utilisateur affecté les ancres et les limites définit la position et la taille du contrôle.
    </td>
    <td valign="top">
      <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/AbsoluteLayoutDemoPage.cs"><img src="layouts-images/AbsoluteLayout.png" title="Exemple de DispositionAbsolue" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Grid</a>
    </td>
    <td valign="top">
Une disposition contenant des vues organisés en lignes et colonnes.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/GridDemoPage.cs"><img src="layouts-images/Grid.png" title="Exemple de grille" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/%601">disposition</a> qui utilise <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/">contrainte</a>s à disposition de ses enfants.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/RelativeLayoutDemoPage.cs"><img src="layouts-images/RelativeLayout.png" title="Exemple de RelativeLayout" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/">disposition</a> qui positionne les éléments enfants dans une ligne unique qui peut être orientée verticalement ou horizontalement. Cette disposition définira l’enfant limites automatiquement pendant le cycle de mise en page. Utilisateur affecté limites sera remplacée et par conséquent ne doit pas être défini sur un élément enfant par l’utilisateur.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StackLayoutDemoPage.cs"><img src="layouts-images/StackLayout.png" title="Exemple de StackLayout" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Liens associés

- [Introduction à Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [La galerie de Xamarin.Forms (exemple)](https://developer.xamarin.com/samples/FormsGallery/)
- [Exemples Xamarin.Forms](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Documentation de l’API de Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms)
