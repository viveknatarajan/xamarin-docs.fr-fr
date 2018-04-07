---
title: Des erreurs de licences spécifiques
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D26BDF2D-923B-4BC1-841A-74583155DB71
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 8592fe5381c974e999477d0ef6ca6ebdd8b38cc4
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2018
---
# <a name="some-specific-licensing-errors"></a>Des erreurs de licences spécifiques

> [!IMPORTANT]
> Les informations ci-dessous ne s’applique pas aux utilisateurs MSDN, car elles sont des problèmes spécifiques à des licences Xamarin hérités. Si vous êtes un utilisateur MSDN et que vous voyez des erreurs semblables à celles ci-dessous, veuillez réessayer [mise à jour vers la dernière version de Xamarin](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) & faire référence à ce [guide des Options de licences](~/cross-platform/get-started/requirements.md).



## <a name="invalid-license"></a>« Licence non valide »

> Licence non valide. Pour réactiver Xamarin.Android (XA9999) Impossible de déterminer l’édition de licence. (XA9010)

Ces messages peuvent apparaître dans certaines circonstances.

-   La licence en cours sur le disque est peut-être hors de synchronisation avec les informations utilisateur en cours sur l’ordinateur. [Actualiser les fichiers de licence](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) pour résoudre ce problème dans de nombreux cas.

-   L’ordinateur peut avoir des activations en double en conflit 2. Ce type de licence est n’est plus utilisé par Xamarin. Si vous rencontrez des problèmes qui semblent être associé à cette erreur, veuillez [prise en charge de messagerie](https://www.xamarin.com/support).

-   Le compte Xamarin ne peut pas encore être invité à une licence Xamarin. Voir aussi : [l’équipe de gestion des licences](~/cross-platform/troubleshooting/legacy-licenses/team-management.md).

## <a name="failed-to-load-android-entitlements"></a>« Échec du chargement des droits Android »

> Erreur de Mono.VisualStudio.Shell.ShellPackage : 0 : échec du chargement des droits Android : licence non valide. Pour réactiver Xamarin.Android (XA9999) Mono.VisualStudio.Shell.ShellPackage erreur : 0 : Échec de la mise à jour de la licence : licence non valide. Pour réactiver Xamarin.Android (XA9999)

Il s’agit d’une version antérieure du problème « Licence non valide » ci-dessus. Les mêmes étapes de dépannage s’appliquent.

## <a name="this-version-was-released-after-your-subscription-expired"></a>« Cette version a été publiée après l’expiration de votre abonnement »

> Erreur XA9000 : Cette version a été publiée après l’expiration de votre abonnement (11/11/2014 5:11:41 PM). (XA9000) (Mobile.Android.Utilities) erreur XA9010 : Impossible de déterminer l’édition de licence. (XA9010) (Mobile.Android.Utilities)

Ces messages apparaissent généralement dans deux situations :

-   Les licences sur le disque sont obsolètes. [Actualiser les fichiers de licence](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) pour résoudre ce problème dans de nombreux cas.

-   L’abonnement Xamarin n’est plus active, et la version actuellement installée de Xamarin est plus récente que la date d’expiration de l’abonnement. Dans ce cas la correction appropriée consiste à [rétrograder](http://kb.xamarin.com/customer/portal/articles/1699777) vers une version de Xamarin qui a été publiée avant l’expiration de l’abonnement. N’hésitez pas à envoyer un courrier électronique à [ hello@xamarin.com ](mailto:hello@xamarin.com) pour demander la version valide la plus récente pour votre abonnement. Si l’erreur persiste après la rétrogradation, veillez à essayer d’actualiser les fichiers de licence de trop.

## <a name="additional-references"></a>Références supplémentaires

-   [Comment actualiser des licences](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)
-   [La rétrogradation de Xamarin](http://kb.xamarin.com/customer/portal/articles/1699777-downgrading)
-   [Liste des codes d’erreur Xamarin.Android](~/android/troubleshooting/errors.md)
-   [Liste des codes d’erreur MonoTouch (Xamarin.iOS)](~/ios/troubleshooting/mtouch-errors.md)

### <a name="next-steps"></a>Étapes suivantes
Pour obtenir une assistance pour nous contacter, ou si ce problème persiste même après utilisant les informations ci-dessus, consultez [les options de support sont disponibles pour Xamarin ?](~/cross-platform/troubleshooting/support-options.md) pour plus d’informations sur les options de contact, des suggestions, ainsi que comment entrer un bogue nouveau si nécessaire.
