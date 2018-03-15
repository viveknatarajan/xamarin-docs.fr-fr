---
title: Partie 2 - Architecture
ms.topic: article
ms.prod: xamarin
ms.assetid: 2176DB2D-E84A-3757-CFAB-04A586068D50
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: 25cd374ef2b3026f5ac7242ee076c8593ce806da
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2018
---
# <a name="part-2---architecture"></a>Partie 2 - Architecture

Un principe clé de la création des applications multiplateformes est de créer une architecture qui se prête à une optimisation de partage entre les plateformes de code. Respectez les principes suivants de service de programmation orientée objet permet de générer une application bien conçue :

-   **Encapsulation** – garantir que classes et couches architecturales même uniquement exposent une API minimale qui effectue son requis fonctionne et masque les détails d’implémentation. Un niveau de la classe, cela signifie que les objets se comportent comme « cases noires » et que la consommation du code n’a pas besoin de savoir comment ils accomplir leurs tâches. Au niveau de l’architecture, il signifie implémenter des modèles comme Façade qui encouragent une API simplifiée qui orchestre les interactions plus complexes pour le compte du code dans les couches plus abstraites. Cela signifie (que le code de l’interface utilisateur par exemple) ne doit être chargé pour l’affichage des écrans et d’accepter l’entrée d’utilisateur ; et jamais interagir directement avec la base de données. De même le code d’accès aux données doit uniquement lire et écrire dans la base de données, mais jamais interagir directement avec les boutons ou d’étiquettes.
-   **Séparation des responsabilités** – Vérifiez que chaque composant (les deux à l’architecture et au niveau de la classe) a un objectif clair et bien défini. Chaque composant doit effectuer uniquement à ses tâches définies et exposer cette fonctionnalité via une API qui est accessible aux autres classes qui doivent l’utiliser.
-   **Le polymorphisme** – programmation à une interface (ou une classe abstraite) qui prend en charge plusieurs implémentations signifie que code de base peut être écrite partagé entre les plateformes, lors de l’interaction toujours avec des fonctionnalités spécifiques à la plateforme.


Le résultat naturel est une application modélisée d’après le monde réel ou des entités abstraites avec des couches logiques distincts. Séparer le code en couches rendre les applications plus facile à comprendre, à tester et à gérer. Il est recommandé que le code dans chaque couche être physiquement séparé (soit dans les répertoires ou des projets même distincts pour les très grandes applications), ainsi que logiquement séparé (à l’aide des espaces de noms).

 <a name="Typical_Application_Layers" />


## <a name="typical-application-layers"></a>Couches Application classique

Tout au long de ce document et les études de cas, nous nous référons aux couches six application suivantes :

-   **Couche données** – persistance des données Non volatile, susceptible d’être une base de données SQLite mais peut être implémenté avec les fichiers XML ou tout autre mécanisme approprié.
-   **Couche d’accès aux données** – Wrapper autour de la couche données qui fournit la création, lecture, mise à jour, l’accès aux données sans exposer les détails d’implémentation à l’appelant Supprimer (CRUD). Par exemple, la couche DAL peut contenir des instructions SQL pour interroger ou mettre à jour les données, mais le code de référencement n’a pas besoin de cette information.
-   **Couche métier** – (parfois appelé la couche de logique métier ou de la couche BLL) contient les définitions d’entité métier (le modèle) et la logique métier. Candidat pour le modèle de Façade métier.
-   **Couche d’accès aux services** – utilisé pour accéder aux services dans le cloud : à partir des services web complexe (WCF REST, JSON,) pour la récupération simple des données et des images à partir des serveurs distants. Encapsule le comportement de mise en réseau et fournit une API simple pour être consommés par les couches Application et de l’interface utilisateur.
-   **Couche d’application** – Code est généralement spécifiques à la plateforme (généralement partagé entre les plateformes) ou de code qui est spécifique à l’application (non réutilisable généralement). Un bon test si vous devez placer le code dans la couche d’Application par rapport à la couche d’interface utilisateur est (a) pour déterminer si la classe possède des contrôles d’affichage réel ou (b) si elle peut être partagé entre plusieurs écrans ou des périphériques (par exemple). iPhone et iPad).
-   **Couche d’Interface (interface utilisateur) utilisateur** : la couche orientés utilisateur, contient les écrans, widgets et les contrôleurs qui gèrent les.


Une application ne doit pas nécessairement contienne toutes les couches, par exemple la couche d’accès de Service n’existerait pas dans une application qui n’accèdent pas aux ressources réseau. Une application très simple peut fusionner la couche données et la couche d’accès aux données, car les opérations sont extrêmement base.

 <a name="Common_Mobile_Software_Patterns" />


## <a name="common-mobile-software-patterns"></a>Modèles courants de logiciel Mobile

Modèles sont une méthode établie pour capturer périodique des solutions aux problèmes courants. Il existe quelques modèles qui sont utiles comprendre à créer des applications mobiles/compréhensible facile à gérer.

-   **Modèle, vue, ViewModel (MVVM)** : The Model-View-ViewModel modèle est souvent utilisée avec les infrastructures qui prennent en charge la liaison de données, telles que Xamarin.Forms. Il a été rendue populaire par XAML compatible kits de développement logiciel Windows Presentation Foundation (WPF) et Silverlight ; où le ViewModel agit en tant qu’intermédiaire entre les données (modèle) et d’une interface utilisateur (vue) via des commandes et la liaison de données.
-   **Vue, contrôleur MVC (Model)** – un modèle commun et souvent mal compris, MVC est souvent utilisé lors de la création d’Interfaces utilisateur et fournit une séparation entre la définition d’un écran de l’interface utilisateur (vue), le moteur derrière lui qui gère interaction (contrôleur) et les données qui remplit (modèle). Le modèle est en fait une partie facultative, et par conséquent, le cœur de comprendre ce modèle se trouve dans la vue et le contrôleur. MVC est une approche courante pour les applications iOS.
-   **Façade métier** – également appelé modèle de gestionnaire, fournit un point simplifié d’entrée de travail complexes. Par exemple, dans une application de suivi des tâches, vous pouvez avoir un `TaskManager` classe avec des méthodes telles que `GetAllTasks()` , `GetTask(taskID)` , `SaveTask (task)` , etc. La `TaskManager` classe fournit une Façade pour le fonctionnement interne de réellement l’enregistrement/la récupération d’objets de tâches.
-   **Singleton** – modèle le Singleton fournit un moyen dans lesquels une seule instance d’un objet particulier peut jamais exister. Par exemple, lorsque vous utilisez SQLite dans les applications mobiles, vous souhaitez qu’une seule instance de la base de données. L’utilisation du modèle de Singleton est un moyen simple de s’en assurer.
-   **Fournisseur** – un modèle inventé par Microsoft (sans doute semblable à base d’Injection de dépendance, ou stratégie) visant à encourager la réutilisation de code entre les applications Silverlight, WPF et Windows Forms. Code partagé peut être écrites par rapport à une interface ou une classe abstraite et spécifique à la plateforme des implémentations concrètes sont écrites et transmies lorsque le code est utilisé.
-   **Async** – à ne pas confondre avec le mot clé Async, l’opération asynchrone de modèle est utilisé lorsque le travail à long terme doit être exécutée sans conserver de l’interface utilisateur ou le traitement en cours. Dans sa forme la plus simple, le modèle asynchrone décrit simplement que les tâches de longue doivent être lancés dans un autre thread, ou comme abstraction de thread tel qu’une tâche lors du thread actuel continue à traiter, attend une réponse du processus en arrière-plan , puis met à jour l’interface utilisateur lorsque les données et ou état est retourné.


Chacun des modèles sera examinée plus en détail que leur utilisation pratique est illustrée dans les études de cas. Wikipedia a plus les descriptions détaillées de la [MVVM](https://en.wikipedia.org/wiki/Model–view–viewmodel), [MVC](https://en.wikipedia.org/wiki/Model–view–controller), [façade](http://en.wikipedia.org/wiki/Facade_pattern), [Singleton](http://en.wikipedia.org/wiki/Singleton_pattern), [stratégie](http://en.wikipedia.org/wiki/Strategy_pattern)et [fournisseur](http://en.wikipedia.org/wiki/Provider_model) modèles (et de [modèles de conception](http://en.wikipedia.org/wiki/Design_Patterns) généralement).
