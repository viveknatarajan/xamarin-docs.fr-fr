---
title: 'Procédure pas à pas : utilisation de WCF'
description: Cette procédure pas à pas couvre la façon dont une application mobile développée avec Xamarin peut consommer un service web WCF à l’aide de la classe BasicHttpBinding.
ms.prod: xamarin
ms.assetid: D0E83342-2E79-4D25-BD57-43718F5628C4
author: asb3993
ms.author: amburns
ms.date: 02/17/2018
ms.openlocfilehash: b0502028985cf35498a5811ec88b9e32d74d0311
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="walkthrough---working-with-wcf"></a>Procédure pas à pas : utilisation de WCF

_Cette procédure pas à pas couvre la façon dont une application mobile développée avec Xamarin peut consommer un service web WCF à l’aide de la classe BasicHttpBinding._


Il s’agit d’une exigence courante pour les applications mobiles être en mesure de communiquer avec les systèmes back-end. Il existe plusieurs options pour les infrastructures de serveur principal, un et les choix [Windows Communication Foundation](http://msdn.microsoft.com/library/ms731082.aspx) (WCF). Cette procédure pas à pas fournit un exemple de la façon dont une application mobile Xamarin peut consommer un service WCF à l’aide la `BasicHttpBinding` classe. La procédure pas à pas comprend les rubriques suivantes :

1.  **Créer un Service WCF** -dans cette section, nous allons créer un service WCF très simple à deux méthodes. La première méthode prendra un paramètre de chaîne, tandis que l’autre méthode prendra un objet c#. Cette section traite également comment configurer la station de travail du développeur pour autoriser l’accès à distance pour le service WCF.
1.  **Créer une Application de Xamarin.Android** -une fois que le service WCF a été créé, nous allons créer une application de Xamarin.Android simple qui utilise le service WCF. Cette section décrit comment créer une classe de proxy de service WCF pour faciliter la communication avec le service WCF.
1.  **Créer une Application Xamarin.iOS** -la dernière partie de ce didacticiel implique la création d’une application Xamarin.iOS simple qui utilise le service WCF.

<a name="Requirements" />

## <a name="requirements"></a>Spécifications

Cette procédure pas à pas suppose que vous disposez des connaissances sur la création et l’utilisation des services WCF.

<a name="Creating_a_WCF_Service" />

## <a name="creating-a-wcf-service"></a>Création d’un Service WCF

La première tâche nous consiste à créer un service WCF pour des applications mobiles à communiquer avec.

1. Lancez Visual Studio 2017 et créez un nouveau projet.
1. Dans le **nouveau projet** boîte de dialogue, sélectionnez le **WCF > bibliothèque du Service WCF** modèle et le nom de la solution `HelloWorldService`:

    ![](walkthrough-working-with-wcf-images/new-wcf-service.png "Créer une nouvelle bibliothèque de service WCF")

1. Dans **l’Explorateur de solutions**, ajoutez une nouvelle classe nommée `HelloWorldData` au projet :

    ```csharp
        using System.Runtime.Serialization;

        namespace HelloWorldService
        {
            [DataContract]
            public class HelloWorldData
            {
                [DataMember]
                public bool SayHello { get; set; }

                [DataMember]
                public string Name { get; set; }

                public HelloWorldData()
                {
                    Name = "Hello ";
                    SayHello = false;
                }
            }
        }
    ```


1. Dans **l’Explorateur de solutions**, renommez `IService1.cs` à `IHelloWorldService.cs`et renommez `Service1.cs` à `HelloWorldService.cs`.
1. Dans **l’Explorateur de solutions**, ouvrez `IHelloWorldService.cs` et remplacez le code par le code suivant :

    ```csharp
        using System.ServiceModel;

        namespace HelloWorldService
        {
            [ServiceContract]
            public interface IHelloWorldService
            {
                [OperationContract]
                string SayHelloTo(string name);

                [OperationContract]
                HelloWorldData GetHelloData(HelloWorldData helloWorldData);
            }
        }
    ```
  
    Ce service propose deux méthodes : une qui prend une chaîne pour un paramètre et l’autre qui prend un objet .NET.

1. Dans **l’Explorateur de solutions**, ouvrez `HelloWorldService.cs` et remplacez le code par le code suivant :

    ```csharp
        using System;

        namespace HelloWorldService
        {
            public class HelloWorldService : IHelloWorldService
            {
                public HelloWorldData GetHelloData(HelloWorldData helloWorldData)
                {
                    if (helloWorldData == null)
                        throw new ArgumentException("helloWorldData");

                    if (helloWorldData.SayHello)
                        helloWorldData.Name = "Hello World to {helloWorldData.Name}";

                    return helloWorldData;
                }

                public string SayHelloTo(string name)
                {
                    return "Hello World to you, {name}";
                }
            }
        }
    ```

1. Dans **l’Explorateur de solutions**, ouvrez `App.config`, mettre à jour le `name` attribut de la `<service>` nœud, la `contract` attribut de la `<endpoint>` nœud et le `baseAddress` attribut de la `<add>` nœud :

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            ...
            <services>
              <service name="HelloWorldService.HelloWorldService">
                <endpoint address="" binding="basicHttpBinding" contract="HelloWorldService.IHelloWorldService">
                  <identity>
                    <dns value="localhost" />
                  </identity>
                </endpoint>
                <endpoint address="mex" binding="mexHttpBinding" contract="IMetadataExchange" />
                <host>
                  <baseAddresses>
                    <add baseAddress="http://localhost:8733/Design_Time_Addresses/HelloWorldService/" />
                  </baseAddresses>
                </host>
              </service>
            </services>
            ...
        </configuration>
    ```

1. Générez et exécutez le service WCF. Le service est hébergé par le client test WCF :

    ![](walkthrough-working-with-wcf-images/hosted-wcf-service.png "Service WCF en cours d’exécution dans le client test")

1. Avec le client test WCF en cours d’exécution, lancer un navigateur et accédez au point de terminaison pour le service WCF :

    ![](walkthrough-working-with-wcf-images/wcf-service-browser.png "Page informations de navigateur de service WCF")

> [!IMPORTANT]
> La section suivante est nécessaire uniquement si vous devez accepter les connexions à distance sur une station de travail Windows 10. La section peut être ignorée si vous disposez d’une autre plateforme sur laquelle déployer le service WCF.

<a name="Allow_Remote_Access_to_IIS_Express" />

## <a name="configuring-remote-access-to-iis-express"></a>Configuration de l’accès à distance pour IIS Express

Hébergement d’un service WCF localement ne convient lorsque les connexions sont fournis uniquement à partir de l’ordinateur local. Toutefois, les appareils distants (par exemple, un appareil Android ou un iPhone) n’aura pas l’accès à un service WCF local. Par conséquent, cette section explique comment configurer Windows 10 et IIS Express pour accepter les connexions à distance :

1.  **Configurer IIS Express connexions accepter à distance** -cette étape implique la modification du fichier de configuration pour IIS Express accepter les connexions à distance sur un port spécifique, puis en définissant une règle pour IIS Express accepter le trafic entrant.
1.  **Ajouter une Exception au pare-feu Windows** -vous devez ouvrir un port via le pare-feu Windows des applications à distance peuvent utiliser pour communiquer avec le service WCF.

    Vous devez connaître l’adresse IP de votre station de travail. Dans le cadre de cet exemple, nous supposons que notre station de travail a l’adresse IP 192.168.1.143.

1. Commençons par la configuration d’IIS Express pour écouter les demandes externes. Vous pouvez le faire en modifiant le fichier de configuration pour IIS Express à `[solutiondirectory]\.vs\config\applicationhost.config`, comme illustré dans la capture d’écran suivante :

    [![](walkthrough-working-with-wcf-images/image05.png "Vous pouvez le faire en modifiant le fichier de configuration pour IIS Express à solutiondirectory.vsconfigapplicationhost.config, comme indiqué dans cette capture d’écran")](walkthrough-working-with-wcf-images/image05.png#lightbox)

    Recherchez le `site` élément portant le nom `HelloWorldWcfHost`. Il doit ressembler à l’extrait de code XML suivant :

    ```xml
        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
            </bindings>
        </site>
    ```
 
    Nous devons ajouter une autre `binding` ouvrir le port 8734 pour le trafic externe. Ajoutez le code XML suivant à la `bindings` élément, en remplaçant l’adresse IP avec votre propre adresse IP :

    ```xml
    <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
    ```
    
    Cette opération va configurer IIS Express pour accepter le trafic HTTP à partir de n’importe quelle adresse IP à distance sur le port 8734 sur l’adresse IP externe de l’ordinateur. Cela ci-dessus extrait de code suppose que l’adresse IP de l’ordinateur exécutant IIS Express est 192.168.1.143. Après la modification, le `bindings` élément doit se présenter comme suit :

    ```xml
        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
                <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
            </bindings>
        </site>
    ```

1. Ensuite, nous avons besoin de configurer IIS Express accepter les connexions entrantes sur le port 8734. Démarrage jusqu'à une invite de commandes d’administration et exécutez la commande suivante :

    `> netsh http add urlacl url=http://192.168.1.143:9608/ user=everyone`

1. L’étape finale consiste à configurer les pare-feu Windows pour autoriser le trafic externe sur le port 8734. À partir d’une invite de commandes d’administration, exécutez la commande suivante :

    `> netsh advfirewall firewall add rule name="IISExpressXamarin" dir=in protocol=tcp localport=8734 profile=private remoteip=localsubnet action=allow`

    Cette commande autorise le trafic entrant sur le port 8734 de tous les appareils sur le même sous-réseau que la station de travail Windows 10.

Vous avez créé un service WCF très simple hébergé dans IIS Express qui accepte les connexions entrantes à partir d’autres appareils ou des ordinateurs sur votre sous-réseau. Vous pouvez tester cette sortie à votre application en cours d’exécution en accédant à `http://localhost:8733/Design_Time_Addresses/HelloWorldService/` sur votre station de travail et `http://192.168.1.143:8734/Design_Time_Addresses/HelloWorldService/` à partir d’un autre ordinateur sur votre sous-réseau.

Pour permettre à IIS Express à conserver en cours d’exécution et desservant le service, désactivez le **Modifier & Continuer** option *propriétés du projet > Web > débogueurs*.

## <a name="creating-a-proxy-for-the-web-service"></a>Création d’un Proxy pour le Service Web

Un service web proxy doit être créé pour le service WCF, avant d’une application peut utiliser le service. Cela peut être accompli de la façon suivante :

1. Ajouter une bibliothèque de classes Standard .NET nommé `HelloWorldServiceProxy`et supprimer toutes les classes dans le projet.
1. Exécutez le `HelloWorldService` projet.
1. Avec la `HelloWorldService` en cours d’exécution du projet, ajoutez une nouvelle **Service connecté** au projet, à l’aide de la **fournisseur de référence de Service Microsoft WCF Web**.
1. Dans le **point de terminaison de Service** onglet de la **configurer la référence de Service Web WCF** boîte de dialogue, cliquez sur le **Discover** bouton, supprimer `mex` à partir de la fin de la détection point de terminaison dans le **URI** liste déroulante, entrez `HelloWorldServiceProxy` en tant que le **Namespace**, puis cliquez sur le **suivant** bouton.
1. Dans le **les Options de Type de données** onglet de la **configurer la référence de Service Web WCF** boîte de dialogue, acceptez les valeurs par défaut en cliquant sur le **suivant** bouton.
1. Dans le **Options du Client** onglet de la **configurer la référence de Service Web WCF** boîte de dialogue, vérifiez que le **Public** case à cocher est sélectionnée, puis cliquez sur le **terminer**  bouton.
1. Générer le `HelloWorldServiceProxy` projet.

> [!NOTE]
> Une alternative à la création du proxy à l’aide du fournisseur Microsoft WCF Web Service référence dans Visual Studio 2017 est d’utiliser le service Model Metadata Utility Tool (svcutil.exe). Pour plus d’informations, consultez [ServiceModel Metadata Utility Tool (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Creating_a_Xamarin_Android_Application" />

## <a name="creating-a-xamarinandroid-application"></a>Création d’une Application Xamarin.Android

Le proxy du service WCF peut être consommé par une application de Xamarin.Android, comme suit :

1. Dans Visual Studio, ajoutez un nouveau projet Android vide à la solution et nommez-le `HelloWorld.Android`.
1. Dans le `HelloWorld.Android` de projet, ajoutez une référence à la `HelloWorldServiceProxy` projet et une référence à la `System.ServiceModel` espace de noms.
1. Dans **l’Explorateur de solutions**, ouvrez `Resources/layout/main.axml` et remplacez le XML existant par le code XML suivant :

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                  android:orientation="vertical"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent">
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/sayHelloWorldButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/say_hello_world" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/sayHelloWorldTextView" />
            </LinearLayout>
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/getHelloWorldDataButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/get_hello_world_data" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/getHelloWorldDataTextView" />
            </LinearLayout>
        </LinearLayout>
    ```
    
    Les captures d’écran suivante montre l’interface utilisateur dans le concepteur :

    [![](walkthrough-working-with-wcf-images/image09.png "Il s’agit d’une capture d’écran de l’aperçu de cette interface utilisateur dans le Concepteur")](walkthrough-working-with-wcf-images/image09.png#lightbox)
    
1. Dans **l’Explorateur de solutions**, ouvrez `Resources/values/Strings.xml` et ajoutez le code XML suivant :

    ```xml
    <string name="say_hello_world">Say Hello World</string>
    <string name="get_hello_world_data">Get Hello World data</string>
    ```
    
1. Dans **l’Explorateur de solutions**, ouvrez `MainActivity.cs` et remplacez le code existant par le code suivant :

    ```csharp
        [Activity(Label = "HelloWorld.Android", MainLauncher = true)]
        public class MainActivity : Activity
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");

            HelloWorldServiceClient _client;
            Button _getHelloWorldDataButton;
            TextView _getHelloWorldDataTextView;
            Button _sayHelloWorldButton;
            TextView _sayHelloWorldTextView;
            ...
        }
    ```

    Remplacez `<insert_WCF_service_endpoint_here>` avec l’adresse de votre point de terminaison WCF.

1. Dans `MainActivity.cs`, modifiez le `OnCreate` méthode afin qu’il contienne le code suivant :

    ```csharp
        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            InitializeHelloWorldServiceClient();

            // This button will invoke the GetHelloWorldData - the method that takes a C# object as a parameter.
            _getHelloWorldDataButton = FindViewById<Button>(Resource.Id.getHelloWorldDataButton);
            _getHelloWorldDataButton.Click += GetHelloWorldDataButtonOnClick;
            _getHelloWorldDataTextView = FindViewById<TextView>(Resource.Id.getHelloWorldDataTextView);

            // This button will invoke SayHelloWorld - this method takes a simple string as a parameter.
            _sayHelloWorldButton = FindViewById<Button>(Resource.Id.sayHelloWorldButton);
            _sayHelloWorldButton.Click += SayHelloWorldButtonOnClick;
            _sayHelloWorldTextView = FindViewById<TextView>(Resource.Id.sayHelloWorldTextView);
        }
    ```
    
    Le code ci-dessus initialise les variables d’instance de la classe et relie des gestionnaires d’événements.

1. Dans `MainActivity.cs`, instanciez la classe proxy cliente en ajoutant les deux méthodes suivantes :

    ```csharp
        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }
    ```
    
    Le code ci-dessus instancie et initialise un `HelloWorldServiceClient` objet.

1. Dans `MainActivity.cs`, ajouter des gestionnaires d’événements pour les deux boutons de la `Activity`:

    ```csharp
        async void GetHelloWorldDataButtonOnClick(object sender, EventArgs e)
        {
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            _getHelloWorldDataTextView.Text = "Waiting for WCF...";
            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                _getHelloWorldDataTextView.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButtonOnClick(object sender, EventArgs e)
        {
            _sayHelloWorldTextView.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                _sayHelloWorldTextView.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    ```
  
1. Exécutez l’application, assurez-vous que le service WCF est en cours d’exécution et cliquez sur les deux boutons. L’application appelle WCF de façon asynchrone, à condition que le `Endpoint` champ est défini correctement :

    [![](walkthrough-working-with-wcf-images/image08.png "Dans les 30 secondes, une réponse doit être reçue à partir de chaque méthode WCF et notre application doit ressembler à cette capture d’écran")](walkthrough-working-with-wcf-images/image08.png#lightbox)

<a name="Creating_a_Xamarin_iOS_Application" />

## <a name="creating-a-xamarinios-application"></a>Création d’une Application Xamarin.iOS

Le proxy du service WCF peut être consommé par une application Xamarin.iOS, comme suit :

1. Dans Visual Studio, ajoutez un nouveau iPhone **Application vue unique** de projet à la solution et nommez-le `HelloWorld.iOS`.
1. Dans le `HelloWorld.iOS` de projet, ajoutez une référence à la `HelloWorldServiceProxy` projet et une référence à la `System.ServiceModel` espace de noms.
1. Dans **l’Explorateur de solutions**, double-cliquez sur `Main.storyboard` pour ouvrir le fichier dans le concepteur iOS. Ensuite, ajoutez le code suivant `UIButton` et `UITextView` contrôles :

    ||Name|Titre|
    |--- |--- |--- |
    |`UIButton`|`sayHelloWorldButton`|Par exemple « Hello, World »|
    |`UITextView`|`sayHelloWorldText`||
    |`UIButton`|`getHelloWorldDataButton`|Obtenir « Hello, World » données|
    |`UITextView`|`getHelloWorldDataText`||

    Après avoir ajouté les contrôles, l’interface utilisateur doit ressembler à la capture d’écran suivante :

    [![](walkthrough-working-with-wcf-images/image12.png "Après avoir ajouté les contrôles, l’interface utilisateur doit ressembler à cette capture d’écran")](walkthrough-working-with-wcf-images/image12.png#lightbox)

1. Dans **l’Explorateur de solutions**, ouvrez `ViewController.cs` et ajoutez le code suivant :

    ```xml
        public partial class ViewController : UIViewController
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");
            HelloWorldServiceClient _client;
            ...
        }
    ```
  
    Remplacez `<insert_WCF_service_endpoint_here>` avec l’adresse de votre point de terminaison WCF.

1. Dans `ViewController.cs`, mettre à jour le `ViewDidLoad` méthode afin qu’elle ressemble à ceci :

    ```csharp
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();
            InitializeHelloWorldServiceClient();

            getHelloWorldDataButton.TouchUpInside += GetHelloWorldDataButton_TouchUpInside;
            sayHelloWorldButton.TouchUpInside += SayHelloWorldButton_TouchUpInside;
        }
    ```
  
1. Dans `ViewController.cs`, ajoutez le `InitializeHelloWorldServiceClient` et `CreateBasicHttpBinding` méthodes :

    ```csharp
        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }
    ```
  
1. Dans `ViewController.cs`, ajouter des gestionnaires d’événements pour le `TouchUpInside` événements sur les deux `UIButton` instances :

    ```csharp
        async void GetHelloWorldDataButton_TouchUpInside(object sender, EventArgs e)
        {
            getHelloWorldDataText.Text = "Waiting for WCF...";
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                getHelloWorldDataText.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButton_TouchUpInside(object sender, EventArgs e)
        {
            sayHelloWorldText.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                sayHelloWorldText.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    ```

1. Exécutez l’application, assurez-vous que le service WCF est en cours d’exécution et cliquez sur les deux boutons. L’application appelle WCF de façon asynchrone, à condition que le `Endpoint` champ est défini correctement :

    [![](walkthrough-working-with-wcf-images/image10.png "Dans les 30 secondes, une réponse doit être reçue à partir de chaque méthode WCF et notre application doit ressembler à cette capture d’écran")](walkthrough-working-with-wcf-images/image10.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Ce didacticiel expliqué comment travailler avec un service WCF dans une application mobile à l’aide de Xamarin.Android et Xamarin.iOS. Il vous a montré comment créer un service WCF et explique comment configurer Windows 10 et IIS Express pour accepter les connexions à partir d’appareils distants. Il explique comment générer un client de proxy WCF et vous a montré comment utiliser le client du proxy dans les applications Xamarin.Android et Xamarin.iOS.


## <a name="related-links"></a>Liens associés

- [HelloWorld (exemple)](https://developer.xamarin.com/samples/mobile/WCF-Walkthrough/)
- [Développement d’Applications orientées Service avec WCF](https://docs.microsoft.com/dotnet/framework/wcf/index)
- [Comment : créer un Client Windows Communication Foundation](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [Service Model Metadata Utility Tool (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
