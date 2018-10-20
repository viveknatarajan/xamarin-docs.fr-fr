---
ms.topic: include
ms.openlocfilehash: e4dfd1ac12f3010939d483381a785091d71599ed
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353267"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Vitesse de capteur](xref:Xamarin.Essentials.SensorSpeed)

- **Le plus rapide** : obtenir les données de capteur aussi rapidement que possible (ne pas renvoie toujours sur le thread d’interface utilisateur).
- **Jeu** – taux approprié pour les jeux (ne pas renvoie toujours sur le thread d’interface utilisateur).
- **Normal** – taux par défaut approprié pour les modifications de l’orientation d’écran.
- **L’interface utilisateur** – taux approprié pour l’interface utilisateur général.

Si votre gestionnaire d’événements n’est pas garanti à exécuter sur le thread d’interface utilisateur et si le Gestionnaire d’événements doit accéder aux éléments d’interface utilisateur, utilisez la [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) méthode à exécuter ce code sur le thread d’interface utilisateur.