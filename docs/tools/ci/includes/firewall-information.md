## <a name="firewall-configuration"></a>Configuration du pare-feu

Dans l’ordre pour les tests à être envoyées à Xamarin Test Cloud, l’ordinateur envoyant les tests doit être en mesure de communiquer avec les serveurs de Test Cloud. Pare-feu doivent être configurés pour autoriser le trafic réseau vers et depuis les serveurs situés dans **testcloud.xamarin.com** sur les ports 80 et 443. Ce point de terminaison est géré par le système DNS et l’adresse IP est susceptible de changer. 

Dans certaines situations, un test (ou un appareil exécutant le test) doit communiquer avec des serveurs web protégés par un pare-feu. Dans ce scénario, le pare-feu doit être configuré pour autoriser le trafic provenant des adresses IP suivantes :

* **195.249.159.238**
* **195.249.159.239**
