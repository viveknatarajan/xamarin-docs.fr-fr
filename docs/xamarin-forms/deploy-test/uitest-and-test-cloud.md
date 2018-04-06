---
title: Automatiser les tests Xamarin.Forms avec Xamarin.UITest et App Center
description: Le composant UITest de Xamarin peut être utilisé avec Xamarin.Forms pour écrire des tests d’interface utilisateur à exécuter dans le cloud sur des centaines d’appareils.
ms.prod: xamarin
ms.assetid: b674db3d-c526-4e31-a9f4-b6d6528ce7a9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/31/2016
ms.openlocfilehash: f671f31dcc0aaf339a7c9f3d6465f4777a8f7f7c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="automate-xamarinforms-testing-with-xamarinuitest-and-app-center"></a>Automatiser les tests Xamarin.Forms avec Xamarin.UITest et App Center

_Le composant UITest de Xamarin peut être utilisé avec Xamarin.Forms pour écrire des tests d’interface utilisateur à exécuter dans le cloud sur des centaines d’appareils._

## <a name="overview"></a>Vue d'ensemble

**[App Center Test](/appcenter/test-cloud/)** permet aux développeurs d’écrire des tests d’interface utilisateur automatisés pour les applications Android et iOS. Avec quelques modifications mineures, les applications Xamarin.Forms peuvent être testées à l’aide de Xamarin.UITest, avec le partage du même code de test. Cet article présente des conseils spécifiques pour que Xamarin.UITest fonctionne avec Xamarin.Forms.

Ce guide part du principe que vous connaissez Xamarin.UITest. Les guides suivants sont recommandés pour se familiariser avec Xamarin.UITest :

- [Introduction à App Center Test](/appcenter/test-cloud/)
- [Introduction à UITest](/appcenter/test-cloud/preparing-for-upload/uitest/)

Une fois qu’un projet UITest a été ajouté à une solution Xamarin.Forms, les étapes de l’écriture et de l’exécution des tests pour une application Xamarin.Forms sont les mêmes que pour une application Xamarin.Android ou Xamarin.iOS.

## <a name="requirements"></a>Configuration requise

Reportez-vous à [Xamarin.UITest](/appcenter/test-cloud/uitest/) pour vérifier que votre projet est prêt pour les tests d’interface utilisateur automatisés.

## <a name="adding-uitest-support-to-xamarinforms-apps"></a>Ajout de la prise en charge d’UITest aux applications Xamarin.Forms

UITest automatise l’interface utilisateur en activant des contrôles à l’écran et en effectuant des entrées partout où un utilisateur interagit normalement avec l’application. Pour activer des tests qui peuvent *appuyer sur un bouton* ou *entrer du texte dans une zone*, le code de test a besoin d’un moyen d’identifier les contrôles à l’écran.

Pour permettre au code UITest de référencer des contrôles, chaque contrôle a besoin d’un identificateur unique. Dans Xamarin.Forms, la méthode recommandée pour définir cet identificateur est d’utiliser la propriété `AutomationId` comme indiqué ci-dessous :

```csharp
var b = new Button {
    Text = "Click me",
    AutomationId = "MyButton"
};
var l = new Label {
    Text = "Hello, Xamarin.Forms!",
    AutomationId = "MyLabel"
};
```

La propriété `AutomationId` peut également être définie en XAML :

```xaml
<Button x:Name="b" AutomationId="MyButton" Text="Click me"/>
<Label x:Name="l" AutomationId="MyLabel" Text="Hello, Xamarin.Forms!" />
```

Une valeur `AutomationId` unique doit être ajoutée à tous les contrôles requis pour les tests (notamment les boutons, entrées de texte et étiquettes dont la valeur peut être interrogée).

> [!NOTE]
> Notez qu’une exception `InvalidOperationException` est levée si plusieurs tentatives de définition de la propriété `AutomationId` d’un élément `Element` sont effectuées.

### <a name="ios-application-project"></a>Projet d’application iOS

Pour exécuter des tests sur iOS, le [paquet NuGet Xamarin.TestCloud.Agent](https://www.nuget.org/packages/Xamarin.TestCloud.Agent/) doit être ajouté au projet. Une fois qu’il a été ajouté, copiez le code suivant dans la méthode `AppDelegate.FinishedLaunching` :

```csharp
#if ENABLE_TEST_CLOUD
// requires Xamarin Test Cloud Agent
Xamarin.Calabash.Start();
#endif
```

L’assembly Calabash utilise des API Apple non publiques qui entraînent le rejet des applications par l’App Store. Toutefois, l’éditeur de liens Xamarin.iOS supprimera l’assembly Calabash de la dernière IPA s’il n’est pas explicitement référencé à partir du code.

> [!NOTE]
>  Les builds de version n’ayant pas la variable de compilateur `ENABLE_TEST_CLOUD`, l’assembly Calabash sera supprimé du bundle d’applications. Toutefois, la directive du compilateur est définie pour les versions Debug, ce qui empêche l’éditeur de liens de supprimer l’assembly.

La capture d’écran suivante affiche la variable de compilateur `ENABLE_TEST_CLOUD` définie pour les versions Debug :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](uitest-and-test-cloud-images/12-compiler-directive-vs.png "Options de génération")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

![](uitest-and-test-cloud-images/11-compiler-directive-xs.png "Options du compilateur")

-----

### <a name="android-application-project"></a>Projet d’application Android

Contrairement à iOS, les projets Android n’ont pas besoin de code de démarrage spécial.

## <a name="writing-uitests"></a>Écriture de tests UITest

Pour plus d’informations sur l’écriture de tests UITest, consultez [Documentation UITest](/appcenter/test-cloud/preparing-for-upload/uitest/). Les étapes ci-dessous sont un récapitulatif décrivant plus précisément comment la [démonstration Xamarin.Forms **UsingUITest**](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/) est générée.

### <a name="use-automationid-in-the-xamarinforms-ui"></a>Utiliser AutomationId dans l’interface utilisateur Xamarin.Forms

Avant de pouvoir écrire tout test UITest, l’interface utilisateur de l’application Xamarin.Forms doit pouvoir contenir des scripts. Vérifiez que tous les contrôles dans l’interface utilisateur ont une valeur `AutomationId` afin de pouvoir être référencés dans le code de test.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

### <a name="adding-a-uitest-project-to-a-new-solution"></a>Ajout d’un projet UITest à une nouvelle solution

Quand vous créez un projet Xamarin.Forms à l’aide de Visual Studio pour Mac, un nouveau projet UITest peut être ajouté à la solution en sélectionnant **Xamarin Test Cloud : Ajouter un projet de test d’interface utilisateur automatisé** :

![](uitest-and-test-cloud-images/01-new-solution-xs.png "Configurer votre nouveau projet")

La nouvelle solution est automatiquement configurée pour exécuter Xamarin.UITests sur l’application Xamarin.Forms.

-----

#### <a name="referring-to-the-automationid-in-uitests"></a>Référence à AutomationId dans les tests UITest

Lors de l’écriture de tests UITest, la valeur `AutomationId` est exposée différemment sur chaque plateforme :

- **iOS** utilise le champ `id`.
- **Android** utilise le champ `label`.

Pour écrire des tests UITest multiplateformes qui recherchent la valeur `AutomationId` à la fois sur iOS et Android, utilisez la requête de test `Marked` :

```csharp
app.Query(c=>c.Marked("MyButton"))
```

La forme abrégée `app.Query("MyButton")` fonctionne également.

### <a name="adding-a-uitest-project-to-an-existing-solution"></a>Ajout d’un projet UITest à une solution existante

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio propose un modèle qui permet d’ajouter un projet Xamarin.UITest à une solution Xamarin.Forms existante :

1. Cliquez avec le bouton droit sur la solution, puis sélectionnez **Fichier > Nouveau projet**.
1. Dans les modèles **Visual C#**, sélectionnez la catégorie **Test**. Sélectionnez le modèle **Application de test d’interface utilisateur > Multiplateforme** :

    ![](uitest-and-test-cloud-images/08-new-uitest-project-vs.png "Ajouter un nouveau projet")

    Vous ajoutez ainsi un nouveau projet avec les paquets NuGet **NUnit**, **Xamarin.UITest** et **NUnitTestAdapter** à la solution :

    ![](uitest-and-test-cloud-images/09-new-uitest-project-xs.png "Gestionnaire de package NuGet")

    **NUnitTestAdapter** est un Test Runner tiers qui permet à Visual Studio d’exécuter des tests NUnit à partir de Visual Studio.

    Le nouveau projet comporte également deux classes. **AppInitializer** contient du code pour aider à initialiser et configurer les tests. L’autre classe, **Tests**, contient un code réutilisable pour aider à démarrer les tests UITest.

1. Ajoutez une référence de projet à partir du projet UITest au projet Xamarin.Android :

    ![](uitest-and-test-cloud-images/10-test-apps-vs.png "Gestionnaire de références de projet")

    Cela permet à **NUnitTestAdapter** d’exécuter les tests UITest pour l’application Android à partir de Visual Studio.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio pour Mac](#tab/vsmac)

Il est possible d’ajouter un nouveau projet Xamarin.UITest à une solution existante manuellement :

1. Commencez par ajouter un nouveau projet en sélectionnant la solution et en cliquant sur **Fichier > Ajouter un nouveau projet**. Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Multiplateforme > Tests > Xamarin Test Cloud > Application de test d’interface utilisateur** :

    ![](uitest-and-test-cloud-images/02-new-uitest-project-xs.png "Choisir un modèle")

    Vous ajoutez ainsi un nouveau projet qui contient déjà les paquets NuGet **NUnit** et **Xamarin.UITest** dans la solution :

    ![](uitest-and-test-cloud-images/03-new-uitest-project-xs.png "Paquets NuGet UITest Xamarin")

    Le nouveau projet comporte également deux classes. **AppInitializer** contient du code pour aider à initialiser et configurer les tests. L’autre classe, **Tests**, contient un code réutilisable pour aider à démarrer les tests UITest.

1. Sélectionnez **Vue > Blocs > Tests unitaires** pour afficher le bloc Tests unitaires. Développez **UsingUITest > UsingUITest.UITests > Applications de test** :

    ![](uitest-and-test-cloud-images/04-unit-test-pad-xs.png "Bloc Tests unitaires")

1. Cliquez avec le bouton droit sur **Applications de test**, cliquez sur **Ajouter un projet d’application**, puis sélectionnez les projets iOS et Android dans la boîte de dialogue qui s’affiche :

    ![](uitest-and-test-cloud-images/05-add-test-apps-xs.png "Boîte de dialogue Applications de test")

    Le bloc **Tests unitaires** doit maintenant comporter une référence aux projets iOS et Android. Cela permettra à Visual Studio pour Mac Test Runner d’exécuter des tests UITest localement sur les deux projets Xamarin.Forms.

#### <a name="adding-uitest-to-the-ios-app"></a>Ajout d’UITest à l’application iOS

Quelques modifications supplémentaires doivent être apportées à l’application iOS avant que Xamarin.UITest ne fonctionne :

1. Ajoutez le paquet NuGet **Xamarin.TestCloud.Agent**. Cliquez avec le bouton droit sur **Paquets**, sélectionnez **Ajouter des paquets**, recherchez **Xamarin.TestCloud.Agent** dans NuGet et ajoutez-le au projet Xamarin.iOS :

    ![](uitest-and-test-cloud-images/07-add-test-cloud-agent-xs.png "Ajouter les paquets NuGet")

1. Modifiez la méthode `FinishedLaunching` de la classe **AppDelegate** pour initialiser Xamarin.TestCloud.Agent au démarrage de l’application iOS et pour définir la propriété `AutomationId` des vues. La méthode `FinishedLaunching` doit ressembler à l’exemple de code suivant :

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
        #if ENABLE_TEST_CLOUD
        Xamarin.Calabash.Start();
        #endif

        global::Xamarin.Forms.Forms.Init();

        LoadApplication(new App());

        return base.FinishedLaunching(app, options);
}
```

-----

Après avoir ajouté Xamarin.UITest à la solution Xamarin.Forms, il est possible de créer des tests UITest, de les exécuter localement et de les envoyer à Xamarin Test Cloud.

## <a name="summary"></a>Récapitulatif

Les applications Xamarin.Forms peuvent être facilement testées avec **Xamarin.UITest** à l’aide d’un mécanisme simple pour exposer `AutomationId` comme identificateur de vue unique pour l’automatisation de test. Une fois qu’un projet UITest a été ajouté à une solution Xamarin.Forms, les étapes de l’écriture et de l’exécution des tests pour une application Xamarin.Forms sont les mêmes que pour une application Xamarin.Android ou Xamarin.iOS.

Pour plus d’informations sur la façon d’envoyer des tests à App Center Test, consultez [Envoi de tests UITest](/appcenter/test-cloud/preparing-for-upload/uitest/). Pour plus d’informations sur UITest, consultez [Documentation App Center Test](/appcenter/test-cloud/).


## <a name="related-links"></a>Liens associés

- [UITestSample](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/)
- [Exemples Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
- [Test App Center](/appcenter/test-cloud/)
- [Xamarin.UITest](/appcenter/test-cloud/uitest/)
- [NUnit](http://www.nunit.org)
