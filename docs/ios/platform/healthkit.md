---
title: HealthKit
description: "HealthKit est une infrastructure introduite dans iOS 8 qui fournit un magasin de données centralisé et coordonné sécurisé pour les informations relatives au contrôle d’intégrité. Le système d’exploitation garantit la confidentialité et la sécurité des informations d’intégrité et, avec l’application de contrôle d’intégrité, un tableau de bord pour l’utilisateur. Avec l’autorisation de l’utilisateur, les applications peuvent lire et écrire une grande variété d’informations d’intégrité."
ms.topic: article
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: e7075b67db94b6bf603bd96c637c9f7724ae1519
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="healthkit"></a>HealthKit

_HealthKit est une infrastructure introduite dans iOS 8 qui fournit un magasin de données centralisé et coordonné sécurisé pour les informations relatives au contrôle d’intégrité. Le système d’exploitation garantit la confidentialité et la sécurité des informations d’intégrité et, avec l’application de contrôle d’intégrité, un tableau de bord pour l’utilisateur. Avec l’autorisation de l’utilisateur, les applications peuvent lire et écrire une grande variété d’informations d’intégrité._

Kit de contrôle d’intégrité fournit une banque de données sécurisé pour les informations relatives à la santé de l’utilisateur. Contrôle d’intégrité Kit applications peuvent, avec l’autorisation explicite de l’utilisateur, lire et écrire cette banque de données et recevoir des notifications lorsque les données pertinentes sont ajoutées. Les applications peuvent présenter les données, ou l’utilisateur peut utiliser application de contrôle d’intégrité de l’Apple fournie pour afficher un tableau de bord de toutes leurs données.

Étant donné que les données liées au contrôle d’intégrité sont sensible et crucial, Kit de contrôle d’intégrité est fortement typé, avec des unités de mesure et une association explicite avec le type d’informations en cours d’enregistrement (par exemple, GLYCEMIE ou rythme cardiaque). En outre, les applications de contrôle d’intégrité Kit doivent utiliser des droits explicites, doit demander l’accès pour les types particuliers d’informations et l’utilisateur doit accorder explicitement l’accès à ces types de données de l’application.

Cet article vous présente :

- Exigences de sécurité du Kit de contrôle d’intégrité, y compris l’approvisionnement d’application et de la demande utilisateur l’autorisation d’accéder à la base de données d’intégrité Kit ;
- Système de type du Kit de contrôle d’intégrité, ce qui réduit le risque de mal application ou une mauvaise interprétation des données ;
- L’écriture dans le magasin de données d’intégrité Kit partagée, à l’échelle du système.

Cet article ne traite pas des rubriques plus avancées, telles que l’interrogation de la base de données, de conversion entre les unités de mesure ou de recevoir des notifications de nouvelles données.

Dans cet article, nous allons créer un exemple d’application pour enregistrer le rythme cardiaque de l’utilisateur :

[![](healthkit-images/image01.png "Un exemple d’application pour enregistrer le rythme cardiaque utilisateurs")](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>Spécifications

Les éléments suivants sont requis pour terminer les étapes présentées dans cet article :

- **Xcode 7 et iOS 8 (ou version ultérieure)** – Apple dernière Xcode et iOS API doivent être installés et configurés sur l’ordinateur du développeur.
- **Visual Studio pour Mac ou Visual Studio** : la version la plus récente de Visual Studio pour Mac doit être installée et configurée sur l’ordinateur du développeur.
- **iOS 8 (ou version ultérieure) périphérique** – un appareil iOS en cours d’exécution la dernière version d’iOS 8 ou supérieur pour le test.

> [!IMPORTANT]
> Kit de contrôle d’intégrité a été introduite dans iOS 8. Actuellement, Kit de contrôle d’intégrité n’est pas disponible sur le simulateur iOS, et le débogage requiert la connexion à un appareil iOS physiques.




## <a name="creating-and-provisioning-a-health-kit-app"></a>Création et configuration d’une application du Kit de contrôle d’intégrité
Une application iOS 8 de Xamarin peut utiliser l’API HealthKit, il doit être correctement configuré et mis en service. Cette section décrit les étapes requises pour configurer correctement votre Application Xamarin.

Contrôle d’intégrité Kit applications requièrent :

- Explicite **ID d’application**.
- A **profil de préparation** associée à cet explicite **ID d’application** et **Kit de contrôle d’intégrité** autorisations.
- Un `Entitlements.plist` avec un `com.apple.developer.healthkit` propriété de type `Boolean` la valeur `Yes`.
- Un `Info.plist` dont `UIRequiredDeviceCapabilities` clé contient une entrée avec la `String` valeur `healthkit`.
- Le `Info.plist` doit également avoir des entrées de confidentialité-explication approprié : un `String` explication pour la clé `NSHealthUpdateUsageDescription` si l’application doit écrire des données et un `String` explication pour la clé `NSHealthShareUsageDescription` si l’application va lire le Kit de contrôle d’intégrité données.

Pour plus d’informations sur la configuration d’une application iOS, le [vos appareils](~/ios/get-started/installation/device-provisioning/index.md) article dans Xamarin **mise en route** série décrit la relation entre les certificats de développeur, ID de l’application, Profils de configuration et les droits de l’application.

<a name="explicit-appid" />

### <a name="explicit-app-id-and-provisioning-profile"></a>ID d’application explicite et de la configuration du profil

La création d’une liste explicite **ID d’application** et appropriées **profil de préparation** est effectuée au sein d’Apple [iOS Dev Center](https://developer.apple.com/devcenter/ios/index.action). 

Votre actuel **ID d’application** sont répertoriés dans le [certificats, les identificateurs et les profils](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) section du centre de développement. Souvent, cette liste affiche **ID** les valeurs de `*`, qui indique que le **ID d’application** - **nom** peut être utilisé avec n’importe quel nombre de suffixes. Ces *Wildcard application IDs* ne peut pas être utilisé avec le Kit de contrôle d’intégrité.
 
Pour créer un texte explicite **ID d’application**, cliquez sur le  **+**  bouton dans l’angle supérieur droit pour accéder à la **inscrire iOS ID d’application** page :


[![](healthkit-images/image02.png "L’inscription d’une application sur le portail des développeurs Apple")](healthkit-images/image02.png#lightbox)

Comme indiqué dans l’image ci-dessus, après la création d’une description de l’application, utilisez le **ID d’application explicite** section pour créer un ID de votre application. Dans le **des Services d’application** section, vérifiez **Kit de contrôle d’intégrité** dans les **activer les Services** section.

Lorsque vous avez terminé, appuyez sur la **continuer** bouton pour enregistrer le **ID d’application** dans votre compte. Vous s’affiche de nouveau la **profils, des identificateurs et des certificats** page. Cliquez sur **profils de configuration** pour accéder à la liste de vos profils de configuration en cours, puis cliquez sur le  **+**  situé dans l’angle supérieur droit pour accéder à la **ajouter iOS Profil de configuration** page. Sélectionnez le **iOS le développement d’applications** , puis cliquez sur **continuer** pour accéder à la **Sélectionnez ID d’application** page. Ici, sélectionnez explicites **ID d’application** que vous avez spécifié précédemment :


[![](healthkit-images/image03.png "Sélectionnez l’ID d’application explicite")](healthkit-images/image03.png#lightbox)

Cliquez sur **continuer** et professionnels via les autres écrans, vous permet de spécifier votre **certificat de développeur**, **ou des périphériques**et un **nom** pour ce **profil de configuration**:

[![](healthkit-images/image04.png "Générer le profil de configuration")](healthkit-images/image04.png#lightbox)

Cliquez sur **générer** et attend la création de votre profil. Téléchargez le fichier et double-cliquez dessus pour l’installer dans Xcode. Vous pouvez vérifier son installation sous **Xcode > Préférences > comptes > Afficher les détails...** Vous devez voir votre profil de configuration installés juste, et elle doit avoir l’icône pour le Kit de contrôle d’intégrité et d’autres services spéciaux dans son **droits** ligne :

[![](healthkit-images/image05.png "Affichage du profil dans Xcode")](healthkit-images/image05.png#lightbox)

<a name="associating-appid" />

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>Association de l’ID d’application et de configuration du profil avec votre application Xamarin.iOS

Une fois que vous avez créé et installé appropriée **profil de préparation** comme décrit, il serait normalement temps pour créer une solution dans Visual Studio pour Mac ou Visual Studio. Accès au Kit de contrôle d’intégrité est disponible pour n’importe quel iOS c# ou un projet F #.

Plutôt que décrivent le processus de création d’un projet d’iOS 8 Xamarin manuellement, ouvrez l’exemple d’application associé à cet article (qui inclut un plan conceptuel prédéfinies et du code). Pour associer l’exemple d’application avec votre Kit de contrôle d’intégrité activé **profil de préparation**, dans le **Solution remplissage**, avec le bouton droit sur votre projet et affichez ses **Options** boîte de dialogue. Basculez vers le **iOS Application** panneau et entrez explicites **ID d’application** vous avez créé précédemment en tant que l’application **identificateur de lot**:

[![](healthkit-images/image06.png "Entrez l’ID d’application explicite")](healthkit-images/image06.png#lightbox)

Passez maintenant à la **signature d’offre groupée iOS** Panneau de configuration. Votre récemment installé **profil de préparation**, avec son association à explicites **ID d’application**, seront désormais disponibles en tant que le **profil de préparation**:

[![](healthkit-images/image07.png "Sélectionnez le profil de configuration")](healthkit-images/image07.png#lightbox)

Si le **profil de préparation** n’est pas disponible, vérifiez attentivement le **identificateur de lot** dans les **iOS Application** panneau par rapport à celui figurant le **iOS Centre de développement** et que le **profil de préparation** est installé (**Xcode > Préférences > comptes > Afficher les détails...** ).

Lorsque le contrôle d’intégrité Kit activé **profil de préparation** est sélectionnée, cliquez sur **OK** pour fermer la boîte de dialogue Options du projet.

### <a name="entitlementsplist-and-infoplist-values"></a>Entitlements.plist et valeurs du fichier Info.plist.

L’exemple d’application inclut un `Entitlements.plist` fichier (ce qui est nécessaire pour que le Kit de contrôle d’intégrité des applications activé) et pas inclus dans chaque modèle de projet. Si votre projet n’inclut pas de droits d’accès, avec le bouton droit sur votre projet, choisissez **fichier > nouveau fichier... > iOS > Entitlements.plist** en ajouter un manuellement.

Enfin, votre `Entitlements.plist` doit avoir la paire clé / valeur suivante :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.HealthKit</key>
    <true/>
</dict>
</plist>

```

De même, la `Info.plist` pour l’application doit avoir la valeur `healthkit` associé à la `UIRequiredDeviceCapabilities` clé :

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

L’exemple d’application fourni avec cet article inclut un préconfigurés `Entitlements.plist` qui inclut toutes les clés requises.

<a name="programming" />

## <a name="programming-health-kit"></a>Kit de contrôle d’intégrité de programmation

La banque de données de contrôle d’intégrité Kit est un magasin de données privé, spécifiques à l’utilisateur qui est partagé entre les applications. Étant donné que les informations d’intégrité sont donc sensibles, l’utilisateur des étapes positif pour autoriser l’accès aux données. Cet accès peut être partiels (écriture, mais pas en lecture, l’accès pour certains types de données, mais pas d’autres, etc.) et peuvent être révoqués à tout moment. Applications de Kit de contrôle d’intégrité doivent être écrite avec précaution, à condition que de nombreux utilisateurs seront à stocker leurs informations de contrôle d’intégrité.

Les données d’intégrité Kit sont limitées à Apple des types spécifiés. Ces types sont strictement définis : certaines, telles que type de sang sont limités aux valeurs particulières d’une énumération Apple fourni, tandis que d’autres combinent une magnitude avec une unité de mesure (par exemple, g, calories et litre (s)). Même les données qui partagent une unité compatible sont distinguent par leurs `HKObjectType`; par exemple, le système de type intercepte une tentative erronée pour stocker un `HKQuantityTypeIdentifier.NumberOfTimesFallen` valeur à un champ attendu une `HKQuantityTypeIdentifier.FlightsClimbed` même si les deux utilisent le` HKUnit.Count` unité de mesure.

Les types stockables dans le magasin de données de contrôle d’intégrité Kit sont tous des sous-classes de `HKObjectType`. `HKCharacteristicType` les objets stockent biologiques sexe, le Type de sang et Date de naissance. Plus courant, sont `HKSampleType` objets qui représentent des données échantillonnées à un moment spécifique ou sur une période de temps. 

[![](healthkit-images/image08.png "Graphique d’objets HKSampleType")](healthkit-images/image08.png#lightbox)

`HKSampleType` est abstraite et possède quatre sous-classe concrète. Il existe actuellement qu’un seul type de `HKCategoryType` données, qui sont l’analyse de mise en veille. La grande majorité des données dans le Kit de contrôle d’intégrité sont de type `HKQuantityType` et stockent leurs données dans `HKQuantitySample` objets qui sont créés à l’aide du modèle de design Factory familier :

[![](healthkit-images/image09.png "La grande majorité des données dans le Kit de contrôle d’intégrité sont de type HKQuantityType et stocker leurs données dans des objets HKQuantitySample")](healthkit-images/image09.png#lightbox)

`HKQuantityType` types comprise entre `HKQuantityTypeIdentifier.ActiveEnergyBurned` à `HKQuantityTypeIdentifier.StepCount`. 

<a name="requesting-permission" />

### <a name="requesting-permission-from-the-user"></a>Demander l’autorisation de l’utilisateur

Les utilisateurs finaux des étapes positif pour permettre à une application lire ou écrire des données de contrôle d’intégrité Kit. Cela est réalisé via l’application de contrôle d’intégrité est préinstallée sur les appareils iOS 8. La première fois qu’une application de contrôle d’intégrité Kit est exécutée, l’utilisateur est présenté avec un système contrôlé **contrôle d’intégrité accès** boîte de dialogue :

[![](healthkit-images/image10.png "L’utilisateur voit une boîte de dialogue contrôle d’intégrité accès contrôlés par le système")](healthkit-images/image10.png#lightbox)

Une version ultérieure, l’utilisateur peut modifier les autorisations à l’aide de l’application de contrôle d’intégrité **Sources** boîte de dialogue :

[![](healthkit-images/image11.png "L’utilisateur peut modifier les autorisations à l’aide de la boîte de dialogue de contrôle d’intégrité applications Sources")](healthkit-images/image11.png#lightbox)

Étant donné que les informations de contrôle d’intégrité sont extrêmement sensibles, les développeurs d’applications doivent écrire leurs programmes avec précaution, dans l’attente que les autorisations seront refusées et modifiées pendant l’exécution de l’application. L’idiome plus courante consiste à demander des autorisations dans le `UIApplicationDelegate.OnActivated` (méthode), puis modifier l’interface utilisateur comme il convient.

### <a name="permissions-walkthrough"></a>Procédure pas à pas des autorisations

Dans votre projet mis en service de contrôle d’intégrité Kit, ouvrez le `AppDelegate.cs` fichier. Notez l’instruction en utilisant `HealthKit`; en haut du fichier.


Le code suivant fait référence à des autorisations de contrôle d’intégrité Kit :

```csharp
private HKHealthStore healthKitStore = new HKHealthStore ();

public override void OnActivated (UIApplication application)
{
        ValidateAuthorization ();
}

private void ValidateAuthorization ()
{
        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
        var heartRateType = HKObjectType.GetQuantityType (heartRateId);
        var typesToWrite = new NSSet (new [] { heartRateType });
        var typesToRead = new NSSet ();
        healthKitStore.RequestAuthorizationToShare (
                typesToWrite, 
                typesToRead, 
                ReactToHealthCarePermissions);
}

void ReactToHealthCarePermissions (bool success, NSError error)
{
        var access = healthKitStore.GetAuthorizationStatus (HKObjectType.GetQuantityType (HKQuantityTypeIdentifierKey.HeartRate));
        if (access.HasFlag (HKAuthorizationStatus.SharingAuthorized)) {
                HeartRateModel.Instance.Enabled = true;
        } else {
                HeartRateModel.Instance.Enabled = false;
        }
}

```

Tout le code dans ces méthodes a pu être effectuée en ligne `OnActivated`, mais l’exemple d’application utilise des méthodes distinctes pour que leur objectif plus claire : `ValidateAuthorization()` comporte les étapes nécessaires pour demander l’accès aux types écrits spécifique (et en lecture, si vous le souhaitez l’application) et `ReactToHealthCarePermissions()` est un rappel qui est activé une fois que l’utilisateur interagisse avec la boîte de dialogue autorisations dans le Health.app.

Le travail de `ValidateAuthorization()` consiste à créer le jeu de `HKObjectTypes` que l’application écrit et demander l’autorisation de mettre à jour ces données. Dans l’exemple d’application, le `HKObjectType` est pour la clé `KHQuantityTypeIdentifierKey.HeartRate`. Ce type est ajouté à l’ensemble `typesToWrite`, tandis que le jeu de `typesToRead` est vide. Ces jeux et une référence à la `ReactToHealthCarePermissions()` rappel, est transmis à `HKHealthStore.RequestAuthorizationToShare()`.

Le `ReactToHealthCarePermissions()` rappel sera appelé une fois que l’utilisateur interagisse avec la boîte de dialogue Autorisations et est passé de deux types d’informations : un `bool` valeur qui sera `true` si l’utilisateur interagisse avec la boîte de dialogue Autorisations et un `NSError`qui, si non null, indique un type de l’erreur associée à la présentation de la boîte de dialogue autorisations.

> [!IMPORTANT]
> D’identifier clairement les arguments de cette fonction : le _réussite_ et _erreur_ paramètres n’indiquent pas si l’utilisateur a obtenu l’autorisation d’accéder aux données de contrôle d’intégrité Kit ! Elles indiquent uniquement que l’utilisateur a été donné la possibilité d’autoriser l’accès aux données.

Pour vérifier si l’application a accès aux données, le `HKHealthStore.GetAuthorizationStatus()` est utilisée, transmettant `HKQuantityTypeIdentifierKey.HeartRate`. Selon l’état retourné, l’application active ou désactive la possibilité d’entrer des données. Il n’existe aucune expérience utilisateur standard pour traiter un refus d’accès et de nombreuses options sont disponibles. Dans l’exemple d’application, l’état est défini sur un `HeartRateModel` objet singleton qui, à son tour, déclenche les événements pertinents.

## <a name="model-view-and-controller"></a>Modèle, vue et contrôleur

Pour passer en revue les `HeartRateModel` objet singleton, ouvrez le `HeartRateModel.cs` fichier :

```csharp
using System;
using HealthKit;
using Foundation;

namespace HKWork
{
        public class GenericEventArgs<T> : EventArgs
        {
                public T Value { get; protected set; }
                public DateTime Time { get; protected set; }

                public GenericEventArgs (T value)
                {
                        this.Value = value;
                        Time = DateTime.Now;
                }
        }

        public delegate void GenericEventHandler<T> (object sender,GenericEventArgs<T> args);

        public sealed class HeartRateModel : NSObject
        {
                private static volatile HeartRateModel singleton;
                private static object syncRoot = new Object ();

                private HeartRateModel ()
                {
                }

                public static HeartRateModel Instance {
                        get {
                                //Double-check lazy initialization
                                if (singleton == null) {
                                        lock (syncRoot) {
                                                if (singleton == null) {
                                                        singleton = new HeartRateModel ();
                                                }
                                        }
                                }

                                return singleton;
                        }
                }

                private bool enabled = false;

                public event GenericEventHandler<bool> EnabledChanged;
                public event GenericEventHandler<String> ErrorMessageChanged;
                public event GenericEventHandler<Double> HeartRateStored;

                public bool Enabled { 
                        get { return enabled; }
                        set {
                                if (enabled != value) {
                                        enabled = value;
                                        InvokeOnMainThread(() => EnabledChanged (this, new GenericEventArgs<bool>(value)));
                                }
                        }
                }

                public void PermissionsError(string msg)
                {
                        Enabled = false;
                        InvokeOnMainThread(() => ErrorMessageChanged (this, new GenericEventArgs<string>(msg)));
                }

                //Converts its argument into a strongly-typed quantity representing the value in beats-per-minute
                public HKQuantity HeartRateInBeatsPerMinute(ushort beatsPerMinute)
                {
                        var heartRateUnitType = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        var quantity = HKQuantity.FromQuantity (heartRateUnitType, beatsPerMinute);

                        return quantity;
                }
                        
                public void StoreHeartRate(HKQuantity quantity)
                {
                        var bpm = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        //Confirm that the value passed in is of a valid type (can be converted to beats-per-minute)
                        if (! quantity.IsCompatible(bpm))
                        {
                                InvokeOnMainThread(() => ErrorMessageChanged(this, new GenericEventArgs<string> ("Units must be compatible with BPM")));
                        }

                        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
                        var heartRateQuantityType = HKQuantityType.GetQuantityType (heartRateId);
                        var heartRateSample = HKQuantitySample.FromType (heartRateQuantityType, quantity, new NSDate (), new NSDate (), new HKMetadata());

                        using (var healthKitStore = new HKHealthStore ()) {
                                healthKitStore.SaveObject (heartRateSample, (success, error) => {
                                        InvokeOnMainThread (() => {
                                                if (success) {
                                                        HeartRateStored(this, new GenericEventArgs<Double>(quantity.GetDoubleValue(bpm)));
                                                } else {
                                                        ErrorMessageChanged(this, new GenericEventArgs<string>("Save failed"));
                                                }
                                                if (error != null) {
                                                        //If there's some kind of error, disable 
                                                        Enabled = false;
                                                        ErrorMessageChanged (this, new GenericEventArgs<string>(error.ToString()));
                                                }
                                        });
                                });
                        }
                }
        }
}

```

La première section est un code réutilisable pour la création de gestionnaires d’événements génériques. La première partie de la `HeartRateModel` classe est également souvent utilisé pour la création d’un objet singleton de thread-safe.

Ensuite, `HeartRateModel` expose des 3 événements : 

- `EnabledChanged` -Indique que le stockage du rythme cardiaque a été activé ou désactivé (Notez que le stockage est initialement désactivé). 
- `ErrorMessageChanged` -Pour cet exemple d’application, nous avons un modèle de gestion des erreurs très simple : une chaîne avec la dernière erreur. 
- `HeartRateStored` -Lorsqu’un rythme cardiaque est stocké dans la base de données de contrôle d’intégrité Kit.

Notez que le chaque fois que ces événements sont déclenchés, elle est effectuée via `NSObject.InvokeOnMainThread()`, ce qui permet aux abonnés de mettre à jour l’interface utilisateur. Vous pouvez également les événements pourraient être décrits comme déclenché sur les threads d’arrière-plan et la responsabilité d’assurer la compatibilité peut rester à leurs gestionnaires de. Considérations relatives à la thread sont importants dans les applications du Kit de contrôle d’intégrité, car de nombreuses fonctions, telles que la demande d’autorisation, sont asynchrones et exécutent leurs rappels sur des threads non principal.

Le code spécifique du Kit de contrôle d’intégrité dans `HeartRateModel` est dans les deux fonctions `HeartRateInBeatsPerMinute()` et `StoreHeartRate()`. 

`HeartRateInBeatsPerMinute()` Convertit son argument en un Kit d’intégrité fortement typée `HKQuantity`. Le type de la quantité est spécifiée par le `HKQuantityTypeIdentifierKey.HeartRate` et les unités de la quantité sont `HKUnit.Count` divisé par `HKUnit.Minute` (en d’autres termes, l’unité est *temps par minute*). 

Le `StoreHeartRate()` fonction prend un `HKQuantity` (dans l’exemple d’application, il est créé par `HeartRateInBeatsPerMinute()` ). Pour valider ses données, il utilise le `HKQuantity.IsCompatible()` (méthode), qui retourne `true` si les unités de l’objet peuvent être converties en unités de l’argument. Si la quantité a été créée avec `HeartRateInBeatsPerMinute()` Ceci retournera évidemment `true`, mais elle retourne également `true` si la quantité a été créée en tant que, par exemple, *temps par heure*. En général, `HKQuantity.IsCompatible()` peut être utilisée pour valider de masse, la distance et énergie qui l’utilisateur ou un périphérique peut entrer ou afficher dans un système de mesure (par exemple, en unités impériales), mais qui peuvent être stockées dans un autre système (par exemple, les unités de mesure). 

Une fois que la compatibilité de la quantité a été validée, le `HKQuantitySample.FromType()` méthode de fabrique est utilisé pour créer un fortement typée `heartRateSample` objet. `HKSample` objets dont la date de début et de fin ; pour les lectures instantanées, ces valeurs doivent être identiques, comme dans l’exemple. L’exemple n’affecte pas les données de la clé-valeur sa `HKMetadata` argument, mais un seul peut utiliser code tel que le code suivant pour spécifier l’emplacement du capteur :

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

Une fois le `heartRateSample` a été créé, le code crée une nouvelle connexion à la base de données avec les en bloc. Dans ce bloc, la `HKHealthStore.SaveObject()` méthode tente de l’écriture asynchrone dans la base de données. L’appel résultant à l’expression lambda déclenche des événements pertinents, soit `HeartRateStored` ou `ErrorMessageChanged`.

Maintenant que le modèle a été programmé, il est temps pour voir comment le contrôleur reflète l’état du modèle. Ouvrez le `HKWorkViewController.cs` fichier. Le constructeur simplement relie le `HeartRateModel` singleton aux méthodes de gestion des événements (là encore, cela a pu être effectuée en ligne avec les expressions lambda, mais des méthodes distinctes que l’objectif un peu plus évidente) :

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

Voici les gestionnaires appropriés :

```csharp
void OnEnabledChanged (object sender, GenericEventArgs<bool> args)
{
        StoreData.Enabled = args.Value;
        PermissionsLabel.Text = args.Value ? "Ready to record" : "Not authorized to store data.";
        PermissionsLabel.SizeToFit ();
}

void OnErrorMessageChanged (object sender, GenericEventArgs<string> args)
{
        PermissionsLabel.Text = args.Value;
}

void OnHeartBeatStored (object sender, GenericEventArgs<double> args)
{
        PermissionsLabel.Text = String.Format ("Stored {0} BPM", args.Value);
}

```

Évidemment, dans une application avec un seul contrôleur, il serait possible d’éviter la création d’un objet de modèle distinct et l’utilisation d’événements de flux de contrôle, mais l’utilisation d’objets de modèle est plus appropriée pour les applications du monde réel.

## <a name="running-the-sample-app"></a>L’exemple d’application en cours d’exécution

Le simulateur iOS ne prend pas en charge le Kit de contrôle d’intégrité. Le débogage doit être effectué sur un appareil physique exécutant iOS 8.

Attachez un appareil de développement correctement mis en service iOS 8 sur votre système. Sélectionnez en tant que la cible de déploiement dans Visual Studio pour Mac et dans le menu Choisissez **exécuter > débogage**.

> [!IMPORTANT]
> Les erreurs relatives à la configuration s’affichera à ce stade. Pour résoudre les erreurs, vérifiez la création et la configuration d’une section de contrôle d’intégrité Kit App ci-dessus. Les composants sont : 
>
> - **Centre de développement d’iOS** -ID d’application explicite & Kit de contrôle d’intégrité activé le profil de préparation. 
> - **Options de projet** -identificateur de lot (ID d’application explicite) et le profil de préparation.
> - **Le code source** -Entitlements.plist & Info.plist

En supposant que les dispositions ont été définies correctement, votre application démarre. Lorsqu’il atteint son `OnActivated` (méthode), il demande d’autorisation du Kit de contrôle d’intégrité. La première fois que cela est rencontrée par le système d’exploitation, l’utilisateur s’affiche la boîte de dialogue suivante :


[![](healthkit-images/image12.png "L’utilisateur s’affiche cette boîte de dialogue")](healthkit-images/image12.png#lightbox)

Activer votre application pour mettre à jour les données de taux de cœur et que votre application s’affiche à nouveau. Le `ReactToHealthCarePermissions` rappel sera activé de façon asynchrone. Cela entraîne le `HeartRateModel’s` `Enabled` propriété à modifier, ce qui déclenchera le `EnabledChanged` événement, ce qui provoque le `HKPermissionsViewController.OnEnabledChanged()` Gestionnaire d’événements à exécuter, qui permet la `StoreData` bouton. Le diagramme suivant illustre la séquence :


[![](healthkit-images/image13.png "Ce diagramme illustre la séquence d’événements")](healthkit-images/image13.png#lightbox)

Appuyez sur la **enregistrement** bouton. Cela entraîne le `StoreData_TouchUpInside()` gestionnaire à exécuter, qui tente d’analyser la valeur de la `heartRate` champ de texte, convertir en un `HKQuantity` via décrite précédemment `HeartRateModel.HeartRateInBeatsPerMinute()` la fonction et passer cette quantité à `HeartRateModel.StoreHeartRate()`. Comme indiqué précédemment, cela va tenter de stocker les données et génère soit un `HeartRateStored` ou `ErrorMessageChanged` événement.

Double-cliquez sur le **accueil** bouton sur votre appareil, puis ouvrez l’application de contrôle d’intégrité. Cliquez sur le **Sources** onglet et vous verrez l’exemple d’application répertorié. Choisissez-le, refuser l’autorisation à mettre à jour les données de taux de cœur. Double-cliquez sur le **accueil** bouton et revenez à votre application. Une fois encore, `ReactToHealthCarePermissions()` est appelé, mais cette fois, car l’accès est refusé, le **StoreData** bouton est désactivé (Notez que cela se produit en mode asynchrone et la modification dans l’interface utilisateur peut-être être visible par l’utilisateur final).

## <a name="advanced-topics"></a>Rubriques avancées

Base de données lit les données dans le Kit de contrôle d’intégrité est très similaire à l’écriture de données : Spécifie les types de données un tente d’accéder, d’autorisation des demandes, et si cette autorisation est accordée, les données sont disponibles, avec la conversion automatique compatible unités mesure.

Il existe un nombre des fonctions de requête plus sophistiquées qui permettent d’en fonction de prédicat des requêtes et les requêtes qui effectuent des mises à jour lors de la mise à jour de données pertinentes. 

Les développeurs d’applications de contrôle d’intégrité Kit doivent consulter la section de Kit de contrôle d’intégrité de d’Apple [application passez en revue les instructions](https://developer.apple.com/app-store/review/guidelines/#healthkit).

Une fois la sécurité et les modèles de système de type sont compris, stocker et lire les données dans la base de données de contrôle d’intégrité Kit partagé sont assez simple. De nombreuses fonctions dans le Kit de contrôle d’intégrité fonctionnent de façon asynchrone et les développeurs d’applications doivent écrire leurs programmes de manière appropriée.

Au moment de la rédaction de cet article, il n’existe actuellement pas d’équivalent dans le Kit de contrôle d’intégrité dans Android ou Windows Phone.

## <a name="summary"></a>Récapitulatif

Dans cet article, que nous avons vu comment Kit de contrôle d’intégrité permet aux applications stocker, récupérer et intégrité du partage informations connexes, tout en fournissant une application de contrôle d’intégrité standard qui permet l’accès utilisateur et le contrôle de ces données. 

Nous avons également vu comment l’intégrité des données, la sécurité et confidentialité substituez des problèmes liés à des informations relatives au contrôle d’intégrité et d’applications à l’aide du Kit de contrôle d’intégrité doivent traiter l’augmentation de la complexité de l’application Gestion aspects (approvisionnement), (type de contrôle d’intégrité du Kit de codage système) et l’utilisateur expérience (contrôle utilisateur des autorisations via les boîtes de dialogue système et application de contrôle d’intégrité). 

Enfin, nous avons aborder une implémentation simple du Kit de contrôle d’intégrité à l’aide de l’application d’exemple inclus qui écrit des données de pulsation dans le magasin de contrôle d’intégrité Kit et qui possède une conception asynchrone prenant en charge.

## <a name="related-links"></a>Liens associés

- [HKWork (sample)](https://developer.xamarin.com/samples/monotouch/ios8/IntroToHealthKit/)
- [Introduction à iOS 8](~/ios/platform/introduction-to-ios8.md)
