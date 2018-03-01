## <a name="firewall-configuration"></a>Configuration du pare-feu

Dans l’ordre pour les tests à soumettre à Xamarin Test Cloud, l’ordinateur envoyant des tests doit être en mesure de communiquer avec les serveurs de Test Cloud. Pare-feu doivent être configurés pour autoriser le trafic réseau vers et depuis les serveurs situés dans **testcloud.xamarin.com** sur les ports 80 et 443. Ce point de terminaison est géré par le système DNS et l’adresse IP est susceptible de changer. 

Dans certains cas, un test (ou un périphérique qui exécute le test) doit communiquer avec les serveurs web protégés par un pare-feu. Dans ce scénario, le pare-feu doit être configuré pour autoriser le trafic entre les adresses IP suivantes :

* **195.249.159.238**
* **195.249.159.239**
