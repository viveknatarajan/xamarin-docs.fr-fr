---
title: Il est possible de se connecter à des émulateurs Android fonctionne sur un Mac à partir d’une machine virtuelle Windows ?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2018
ms.openlocfilehash: f94c0966dd67940fc201df84a311db422d77b542
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935199"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Il est possible de se connecter à des émulateurs Android fonctionne sur un Mac à partir d’une machine virtuelle Windows ?

Pour vous connecter à l’émulateur Android en cours d’exécution sur un Mac à partir d’une machine virtuelle Windows, procédez comme suit :

1.  Démarrer l’émulateur sur le Mac.

2.  Tuer le `adb` serveur sur le Mac :

    ```bash
    adb kill-server
    ```

3.  Notez que l’émulateur est à l’écoute sur les ports TCP 2 sur l’interface réseau de bouclage :

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    Le port impair est celui utilisé pour se connecter à `adb`. Voir aussi [ http://developer.android.com/tools/devices/emulator.html#emulatornetworking ](http://developer.android.com/tools/devices/emulator.html#emulatornetworking).

4.  _Option 1_: utilisez [ `nc` ](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html) de trafic entrant vers l’avant les paquets TCP reçu en externe sur le port 5555 (ou tout autre port que vous aimez) au port portant un numéro impair dans l’interface de bouclage (**127.0.0.1 5555** dans cet exemple), et pour transférer les paquets sortants dans l’autre sens :

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    Tant que la `nc` commandes restent en cours d’exécution dans une fenêtre de Terminal Server, les paquets sont transmises comme prévu. Vous pouvez taper CTRL-C dans la fenêtre de Terminal pour quitter le `nc` commandes une fois que vous avez terminé à l’aide de l’émulateur.

    (Option 1 est généralement plus facile que Option 2, en particulier si **Préférences système > sécurité et confidentialité > pare-feu** est activé.) 

    _Option 2_: utilisez [ `pfctl` ](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html) pour rediriger les paquets TCP port `5555` (ou tout autre port que vous le souhaitez) sur le [mise en réseau partagé](http://kb.parallels.com/en/4948) interface au port portant un numéro impair dans la interface de bouclage (`127.0.0.1:5555` dans cet exemple) :

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    Cette commande définit à l’aide de réacheminement de port le `pf packet filter` service système. Les sauts de ligne sont importants. Veillez à conserver intacte lors de la copie-collage. Vous devrez également ajuster le nom de l’interface à partir de *vmnet8* si vous êtes à l’aide de Parallels. `vmnet8` est le nom de l’offre spéciale *périphérique NAT* pour le *mise en réseau partagé* en mode de Fusion de VMWare. L’interface réseau approprié dans Parallels est susceptible de [vnic0](http://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm).

5.  Se connecter à l’émulateur à partir de l’ordinateur Windows :

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    Remplacez « ip-adresse-de-le mac » avec l’adresse IP du Mac, comme par exemple répertoriés par `ifconfig vmnet8 | grep 'inet '`. Si nécessaire, remplacez `5555` avec l’autre port que vous le souhaitez à partir de l’étape 4\. (Remarque : pour obtenir un accès en ligne de commande à `adb` est [ **Outils > Android > invite de commandes Adb Android** ](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) dans Visual Studio.)

### <a name="alternate-technique-using-ssh"></a>Un autre à l’aide de la technique `ssh`

Si vous avez activé _connexion distante_ sur le Mac, vous pouvez ensuite utiliser `ssh` pour se connecter à l’émulateur de réacheminement de port.

1.  Installer un client SSH sur Windows. Une option consiste à installer [Git pour Windows](https://git-for-windows.github.io/). Le `ssh` commande sera ensuite disponible dans le **interpréteur de commandes Git** invite de commandes.

2.  Suivez les étapes 1 à 3 ci-dessus pour démarrer l’émulateur, tuer le `adb` serveur sur le Mac et identifier les ports de l’émulateur.

3.  Exécutez `ssh` sur Windows pour configurer le transfert de port bidirectionnel entre un port local sur Windows (`localhost:15555` dans cet exemple) et le port impair l’émulateur sur l’interface de bouclage du Mac (`127.0.0.1:5555` dans cet exemple) :

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    Remplacez `mac-username` avec votre nom d’utilisateur Mac par `whoami`. Remplacez `ip-address-of-the-mac` avec l’adresse IP du Mac.

4.  Se connecter à l’émulateur à l’aide du port local sur Windows :

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (Remarque : un moyen facile de ligne de commande d’accéder aux `adb` est [ **Outils > Android > invite de commandes Adb Android** dans Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

Une petite attention : Si vous utilisez le port `5555` pour le port local, `adb` pense que que l’émulateur est en cours d’exécution localement sur Windows. Cela n’entraîne aucun problème dans Visual Studio, mais dans Visual Studio pour Mac, il entraîne quitter immédiatement après le lancement de l’application.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>À l’aide de l’autre technique `adb -H` n’est pas pris en charge

En théorie, une autre approche consisterait à utiliser `adb`de fonction intégrée pour se connecter à un `adb` server s’exécutant sur un ordinateur distant (voir, par exemple [ http://stackoverflow.com/a/18551325 ](http://stackoverflow.com/a/18551325)).
Mais les extensions Xamarin.Android IDE ne fournissent pas possible de configurer cette option.

## <a name="contact-information"></a>Informations de contact

Ce document décrit le comportement actuel à partir de mars 2016. La technique décrite dans ce document ne fait pas partie de la suite de test stable pour Xamarin, donc elle pourrait arrêter dans le futur.

Si vous remarquez que la technique ne fonctionne plus, ou si vous constatez que toutes les autres erreurs dans le document, n’hésitez pas à ajouter à la discussion sur le thread de forum suivant : [ http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm ](http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm).
Merci.

