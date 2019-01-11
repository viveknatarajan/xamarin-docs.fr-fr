---
title: iPad modale Style de présentation de Page
description: Caractéristiques de la plateforme vous autorisons à utiliser les fonctionnalités qui est disponible uniquement sur une plateforme spécifique, sans avoir à implémenter des convertisseurs personnalisés ou des effets. Cet article explique comment utiliser les jeux de spécifique à la plateforme iOS le style de présentation d’une page modale sur un iPad.
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: b99898301ed6469b6e0d62ae0077b96aa9c4f3eb
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209084"
---
# <a name="ipad-modal-page-presentation-style"></a>iPad modale Style de présentation de Page

[![Télécharger l’exemple](~/media/shared/download.png) Télécharger l’exemple](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Cette spécifiques à la plateforme iOS est utilisé pour définir le style de présentation d’une page modale sur un iPad. Elle est consommée dans XAML en définissant le `Page.ModalPresentationStyle` propriété pouvant être liée à un `UIModalPresentationStyle` valeur d’énumération :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="FormSheet">
    ...
</ContentPage>
```

Vous pouvez également, il peut être consommé à partir de c# à l’aide de l’API fluent :

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.FormSheet);
        ...
    }
}
```

Le `Page.On<iOS>` méthode spécifie que cette plateforme spécifique s’exécute uniquement sur iOS. Le `Page.SetModalPresentationStyle` (méthode), dans le [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espace de noms, est utilisé pour définir le style de présentation modale sur un [ `Page` ](xref:Xamarin.Forms.Page) en spécifiant une des opérations suivantes `UIModalPresentationStyle` énumération valeurs :

- `FullScreen`, qui définit le style de présentation modale pour englober la totalité de l’écran. Par défaut, les pages modales sont affichés à l’aide de ce style de présentation.
- `FormSheet`, qui définit le style de présentation modale centrée sur et inférieure à l’écran.

En outre, le `GetModalPresentationStyle` méthode peut être utilisée pour récupérer la valeur actuelle de la `UIModalPresentationStyle` énumération est appliquée à la [ `Page` ](xref:Xamarin.Forms.Page).

Le résultat est que le style de présentation modale sur un [ `Page` ](xref:Xamarin.Forms.Page) peuvent être définies :

[![](page-presentation-style-images/modal-presentation-style-small.png "Styles de présentation modale sur un iPad")](page-presentation-style-images/modal-presentation-style-large.png#lightbox "modale Styles de présentation sur un iPad")

> [!NOTE]
> Les pages qui utilisent cette plateforme spécifique pour définir le style de présentation modale doivent utiliser la navigation modale. Pour plus d’informations, consultez [Pages modales Xamarin.Forms](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## <a name="related-links"></a>Liens connexes

- [PlatformSpecifics (exemple)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Création de caractéristiques de la plateforme](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
