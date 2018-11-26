---
ms.topic: include
ms.openlocfilehash: e4dfd1ac12f3010939d483381a785091d71599ed
ms.sourcegitcommit: 676c5a6795ab4896ccd1b288424bf2040b1208aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/25/2018
ms.locfileid: "52294806"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Vitesse de capteur](xref:Xamarin.Essentials.SensorSpeed)

- **Le plus rapide** : obtenir les données de capteur aussi rapidement que possible (aucune garantie de retour sur le thread d’interface utilisateur).
- **Jeu** : débit adapté aux jeux (aucune garantie de retour sur le thread d’interface utilisateur).
- **Normal** : débit par défaut adapté aux changements d’orientation de l’écran.
- **UI** : débit adapté à une interface utilisateur général.

Si votre gestionnaire d’événements ne garantit pas l’exécution sur le thread d’interface utilisateur et si le gestionnaire d’événements doit accéder aux éléments de l’interface utilisateur, utilisez la méthode [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) pour exécuter ce code sur le thread d’interface utilisateur.