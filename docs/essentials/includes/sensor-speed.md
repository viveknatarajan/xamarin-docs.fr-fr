---
ms.topic: include
ms.openlocfilehash: 5c11c94956f8d56c66c50a9a480177c5b77c2643
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947432"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Vitesse de capteur](xref:Xamarin.Essentials.SensorSpeed)

- **Le plus rapide** : obtenir les données de capteur aussi rapidement que possible (ne pas renvoie toujours sur le thread d’interface utilisateur).
- **Jeu** – taux approprié pour les jeux (ne pas renvoie toujours sur le thread d’interface utilisateur).
- **Normal** – taux par défaut approprié pour les modifications de l’orientation d’écran.
- **L’interface utilisateur** – taux approprié pour l’interface utilisateur général.

Si votre gestionnaire d’événements n’est pas garanti à exécuter sur le thread d’interface utilisateur et si le Gestionnaire d’événements doit accéder aux éléments d’interface utilisateur, utilisez la [ `MainThread.BeginInvokeOnMainThread` ](~/essentials/main-thread.md) méthode à exécuter ce code sur le thread d’interface utilisateur.