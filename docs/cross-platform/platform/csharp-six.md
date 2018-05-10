---
title: 6 nouvelles fonctionnalités c# vue d’ensemble
description: La dernière version du langage c# – version 6 – continue à faire évoluer la langue pour avoir moins de code réutilisable, améliorer la clarté et la cohérence plus. Syntaxe d’initialisation du nettoyage, la possibilité d’utiliser await dans les blocs catch/finally et la valeur null-condition ? opérateur sont particulièrement utiles.
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.custom: xamu-video
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: e120b917a106d01067e073219bbe2688c2811448
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="c-6-new-features-overview"></a>6 nouvelles fonctionnalités c# vue d’ensemble

_La dernière version du langage c# – version 6 – continue à faire évoluer la langue pour avoir moins de code réutilisable, améliorer la clarté et la cohérence plus. Syntaxe d’initialisation du nettoyage, la possibilité d’utiliser await dans les blocs catch/finally et la valeur null-condition ? opérateur sont particulièrement utiles._

Ce document présente les nouvelles fonctionnalités de c# 6. Il est entièrement pris en charge par le compilateur mono et les développeurs peuvent démarrer à l’aide des nouvelles fonctionnalités entre toutes les plateformes cibles de Xamarin.

Cet article inclut brèves extraits du code c# 6 qui illustrent l’utilisation de base.
L’exemple d’application est un programme de ligne de commande qui s’exécute sur toutes les plateformes cibles de Xamarin et exerce les différentes fonctionnalités.


> [!VIDEO https://youtube.com/embed/7UdV7zGPfMU]

**Quelles sont les nouveautés dans c# 6, par [Xamarin University](https://university.xamarin.com/)**


## <a name="development-environment"></a>Environnement de développement

### <a name="mac"></a>Mac

* **Visual Studio pour Mac** prend en charge pour c# 6 : vous pouvez générer et compiler des applications Xamarin à l’aide des fonctionnalités de c# 6.
  En savoir plus sur [Visual Studio pour Mac](https://docs.microsoft.com/visualstudio/mac/).

### <a name="windows"></a>Windows

* **Visual Studio 2015 et 2017** et versions ultérieures prennent en charge complète c# 6. Les versions antérieures de Visual Studio ne prendra pas en charge c# 6.

* **Xamarin Studio pour Windows** ne prend pas en charge les fonctionnalités de c# 6 dans l’éditeur.



## <a name="compiler"></a>Compilateur

Le compilateur Mono C# 6 est inclus dans Mono 4.0 et versions ultérieur, qui est [disponible gratuitement en téléchargement](http://www.mono-project.com/download/).
Visual Studio pour Mac met automatiquement à jour l’installation Mono sur votre système.

Les utilisateurs de Windows doivent avoir [Visual Studio 2015 ou 2017 ^](https://www.visualstudio.com/) installé pour compiler le code c# 6 (même si vous choisissez Xamarin Studio pour Windows en tant que votre interface IDE).

^ ou *[Microsoft Build Tools 2015](http://www.microsoft.com/download/details.aspx?id=48159)* pour la commande de ligne compilation ou serveurs de builds, par exemple.

## <a name="using-c-6"></a>À l’aide de C# 6

Le compilateur C# 6 est utilisé dans toutes les versions récentes de Visual Studio pour Mac.
Ceux qui utilisent des compilateurs de ligne de commande doivent confirmer que `mcs --version` retourne 4.0 ou version ultérieure.
Visual Studio pour les utilisateurs Mac permettre vérifier s’ils ont Mono 4 (ou ultérieur) installé en faisant référence à **sur Visual Studio pour Mac > Visual Studio pour Mac > Afficher les détails**.

## <a name="less-boilerplate"></a>Moins de code réutilisable
### <a name="using-static"></a>using static
Énumérations et certaines classes telles que `System.Math`, sont principalement les détenteurs de valeurs statiques et de fonctions. En c# 6, vous pouvez importer tous les membres statiques d’un type avec un seul `using static` instruction. Comparer une fonction trigonométrique classique dans c# 5 et 6 c# :

```csharp
// Classic C#
class MyClass
{
    public static Tuple<double,double> SolarAngleOld(double latitude, double declination, double hourAngle)
    {
        var tmp = Math.Sin (latitude) * Math.Sin (declination) + Math.Cos (latitude) * Math.Cos (declination) * Math.Cos (hourAngle);
        return Tuple.Create (Math.Asin (tmp), Math.Acos (tmp));
    }
}

// C# 6
using static System.Math;

class MyClass
{
    public static Tuple<double, double> SolarAngleNew(double latitude, double declination, double hourAngle)
    {
        var tmp = Asin (latitude) * Sin (declination) + Cos (latitude) * Cos (declination) * Cos (hourAngle);
        return Tuple.Create (Asin (tmp), Acos (tmp));
    }
}
```

`using static` ne pas rendre public `const` les champs, tels que `Math.PI` et `Math.E`, directement accessibles :

```csharp
for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... 
//PI is const, not static, so requires Math.PI
```

### <a name="using-static-with-extension-methods"></a>à l’aide de statique avec les méthodes d’Extension

Le `using static` fonctionnalité opère un peu différemment avec les méthodes d’extension. Bien que les méthodes d’extension sont écrits à l’aide de `static`, ils incompréhensible sans une instance sur laquelle opérer. Par conséquent, lorsque `using static` est utilisé avec un type qui définit les méthodes d’extension, les méthodes d’extension sont disponibles sur leur type de cible (la méthode `this` type). Par exemple, `using static System.Linq.Enumerable` peut être utilisé pour étendre l’API de `IEnumerable<T>` objets sans devoir ouvrir dans tous les types LINQ :

```csharp
using static System.Linq.Enumerable;
using static System.String;

class Program
{
    static void Main()
    {
        var values = new int[] { 1, 2, 3, 4 };
        var evenValues = values.Where (i => i % 2 == 0);
        System.Console.WriteLine (Join(",", evenValues));
    }
}
```

L’exemple précédent montre la différence de comportement : la méthode d’extension `Enumerable.Where` est associé à la baie, lors de la méthode statique `String.Join` peut être appelé sans référence à la `String` type.

### <a name="nameof-expressions"></a>Expressions de nom
Parfois, vous souhaitez faire référence au nom vous avez donné à une variable ou un champ. En c# 6, `nameof(someVariableOrFieldOrType)` retourne la chaîne `"someVariableOrFieldOrType"`. Par exemple, lorsque vous levez une `ArgumentException` vous êtes probablement en nom d’argument qui n’est pas valide :

```csharp
throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))
```

L’avantage principal de `nameof` expressions est qu’ils sont de type contrôlé et sont compatibles avec alimenté par l’outil de refactorisation. La vérification de type de `nameof` expressions est particulièrement utile dans les situations où un `string` est utilisé pour associer des types de manière dynamique. Par exemple, dans iOS un `string` est utilisé pour spécifier le type utilisé pour le prototype `UITableViewCell` des objets dans un `UITableView`. `nameof` peut garantir que cette association n’échoue pas en raison d’une faute d’orthographe ou de la refactorisation mal :

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
    cell.TextLabel.Text = objects [indexPath.Row].ToString ();
    return cell;
}
```

Bien que vous pouvez passer un nom complet `nameof`, uniquement le dernier élément (après la dernière `.`) est retournée. Par exemple, vous pouvez ajouter une liaison de données dans Xamarin.Forms :

```csharp
var myReactiveInstance = new ReactiveType ();
var myLabelOld.BindingContext = myReactiveInstance;
var myLabelNew.BindingContext = myReactiveInstance;
var myLabelOld.SetBinding (Label.TextProperty, "StringField");
var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));
```

Les deux appels à `SetBinding` sont en passant des valeurs identiques : `nameof(ReactiveType.StringField)` est `"StringField"`, et non pas `"ReactiveType.StringField"` comme prévu initialement.

## <a name="null-conditional-operator"></a>Opérateur de NULL-condition
Mises à jour précédentes c# a introduit les concepts de types nullable et l’opérateur de fusion null `??` pour réduire la quantité de code réutilisable lors de la gestion des valeurs NULL. C# 6 est toujours ce thème avec l’opérateur « null-condition » `?.`. Lorsqu’il est utilisé sur un objet dans la partie droite d’une expression, l’opérateur conditionnel-null retourne la valeur du membre si l’objet n’est pas `null` et `null` sinon :

```csharp
var ss = new string[] { "Foo", null };
var length0 = ss [0]?.Length; // 3
var length1 = ss [1]?.Length; // null
var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]
```

(Les deux `length0` et `length1` sont déduits pour être de type `int?`)

La dernière ligne dans l’exemple précédent montre la `?` opérateur null-condition en combinaison avec la `??` opérateur de fusion null. Le nouvel opérateur null-condition c# 6 retourne `null` sur l’élément 2 du tableau, à partir de laquelle l’opérateur de fusion null intervient et fournit un 0 pour la `lengths` de tableau (si qui est approprié ou ne figure pas, bien entendu, problème spécifique).

L’opérateur conditionnel à la valeur null doit réduire considérablement la quantité nécessaire de la vérification de la valeur null réutilisable dans nombreuses applications.

Il existe des limitations sur l’opérateur conditionnel à la valeur null en raison d’ambiguïtés. Vous ne pouvez pas suivre immédiatement un `?` avec une liste d’arguments entre parenthèses, comme vous peut espérer que faire avec un délégué :

```csharp
SomeDelegate?("Some Argument") // Not allowed
```

Toutefois, `Invoke` peut être utilisé pour séparer les `?` à partir de la liste d’arguments et est toujours d’une amélioration marquée sur un `null`-vérification du bloc de code réutilisable :

```csharp
public event EventHandler HandoffOccurred;
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    HandoffOccurred?.Invoke (this, userActivity.UserInfo);
    return true;
}
```

## <a name="string-interpolation"></a>Interpolation de chaîne
Le `String.Format` fonction a utilisé traditionnellement les indices comme espaces réservés dans la chaîne de format, par exemple, `String.Format("Expected: {0} Received: {1}.", expected, received`). Bien sûr, l’ajout d’une nouvelle valeur a toujours impliqué une tâche peu gênante de comptage des arguments, renumérotation des espaces réservés et insérer le nouvel argument dans la séquence appropriée dans la liste d’arguments.

Nouvelle fonctionnalité d’interpolation de chaîne C# 6 améliore considérablement `String.Format`. Maintenant, vous pouvez nommer directement de variables dans une chaîne de préfixe avec un `$`. Par exemple :

```csharp
$"Expected: {expected} Received: {received}."
```

Les variables sont, bien entendu, vérifiées et une variable mal orthographiée ou non disponible provoquera une erreur du compilateur.

Les espaces réservés n’avez pas besoin d’être des variables simples, ils peuvent être n’importe quelle expression. Dans ces espaces réservés, vous pouvez utiliser des guillemets *sans* les soumissions d’échappement. Par exemple, notez le `"s"` dans l’exemple suivant :

```csharp
var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"
```

Interpolation de chaîne prend en charge l’alignement et la syntaxe de mise en forme de `String.Format`. Comme indiqué précédemment `{index, alignment:format}`, en c# 6, vous écrivez `{placeholder, alignment:format}`:

```csharp
using static System.Linq.Enumerable;
using System;

class Program
{
    static void Main ()
    {
        var values = new int[] { 1, 2, 3, 4, 12, 123456 };
        foreach (var s in values.Select (i => $"The value is { i,10:N2}.")) {
            Console.WriteLine (s);
        }
    Console.WriteLine ($"Minimum is { values.Min(i => i):N2}.");
    }
}
```
résultats dans :

    The value is       1.00.
    The value is       2.00.
    The value is       3.00.
    The value is       4.00.
    The value is      12.00.
    The value is 123,456.00.
    Minimum is 1.00.

Interpolation de chaîne est syntaxique pour `String.Format`: il ne peut pas être utilisé avec `@""` littéraux de chaîne et n’est pas compatible avec `const`, même si aucun des espaces réservés ne sont utilisés :

```csharp
const string s = $"Foo"; //Error : const requires value
```

Dans le cas courant utilisation de la création d’arguments de fonction avec interpolation de chaîne de configuration, vous devez toujours faire attention à la séquence d’échappement, le codage et les problèmes de la culture. Les requêtes SQL et les URL sont bien entendu, critiques expurgation. Comme avec `String.Format`, utilisations d’interpolation de chaîne le `CultureInfo.CurrentCulture`. À l’aide de `CultureInfo.InvariantCulture` est un peu plus longue :

```csharp
Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"
```

## <a name="initialization"></a>Initialisation

C# 6 fournit plusieurs façons concise pour spécifier les propriétés, les champs et les membres.

### <a name="auto-property-initialization"></a>Initialisation de la propriété automatique

Des propriétés automatiques peuvent maintenant être initialisées de la même manière concise en tant que champs. Auto-propriétés immuables peuvent être écrites avec uniquement un accesseur get :

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
```

Dans le constructeur, vous pouvez définir la valeur d’une propriété d’auto getter uniquement :

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
    public string Description { get; }

    public ToDo (string description)
    {
        this.Description = description; //Can assign (only in constructor!)
    }
```

Cette initialisation de propriétés automatique est une fonctionnalité générale de l’enregistrement de l’espace et un boon pour les développeurs qui souhaitent mettre en évidence immuabilité dans leurs objets.

### <a name="index-initializers"></a>Initialiseurs d’index.

C# 6 introduit des initialiseurs d’index, ce qui vous permet de définir la clé et la valeur de type ayant un indexeur. En règle générale, il s’agit de `Dictionary`-style de structures de données :

```csharp
partial void ActivateHandoffClicked (WatchKit.WKInterfaceButton sender)
{
    var userInfo = new NSMutableDictionary {
        ["Created"] = NSDate.Now,
        ["Due"] = NSDate.Now.AddSeconds(60 * 60 * 24),
        ["Task"] = Description
    };
    UpdateUserActivity ("com.xamarin.ToDo.edit", userInfo, null);
    statusLabel.SetText ("Check phone");
}
```

### <a name="expression-bodied-function-members"></a>Expression-pulvérisés des fonctions membres

Les fonctions lambda ont plusieurs avantages, dont est simplement économiser de l’espace. De même, membres de classe de pulvérisés d’expression autorise les petites fonctions d’exprimer un peu plus succinctement était possible dans les versions antérieures de c# 6.

Pulvérisés d’expression de fonctions membres utilisent la syntaxe de flèche lambda plutôt que la syntaxe de bloc traditionnelle :

```csharp
public override string ToString () => $"{FirstName} {LastName}";
```

Notez que la syntaxe de la flèche orientée vers le lambda n’utilise pas explicite `return`. Pour les fonctions qui retournent `void`, l’expression doit être également une instruction :

```csharp
public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");
```

Expression-pulvérisés les membres sont toujours soumis à la règle qui `async` est pris en charge pour les méthodes, mais pas les propriétés :

```csharp
//A method, so async is valid
public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
//The following property will not compile
public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;
```

## <a name="exceptions"></a>Exceptions

Il n’existe aucun deux façons concernant : gestion des exceptions est difficile d’obtenir le droit. Nouvelles fonctionnalités de c# 6 effectuer la gestion des exceptions plus flexible et plus cohérente.

### <a name="exception-filters"></a>Filtres d’exception

Par définition, les exceptions se produisent dans des circonstances inhabituelles, et il peut être très difficile pour la raison et le code sur *tous les* les façons d’une exception d’un type particulier peut se produire. C# 6 introduit la possibilité de protéger un gestionnaire d’exécution avec un filtre évalué par le runtime. Cela est fait en ajoutant un `when (bool)` modèle après la normale `catch(ExceptionType)` déclaration. Dans l’exemple suivant, un filtre distingue une erreur d’analyse relatives à la `date` paramètre plutôt que d’autres erreurs d’analyse.

```csharp
public void ExceptionFilters(string aFloat, string date, string anInt)
{
    try
    {
        var f = Double.Parse(aFloat);
        var d = DateTime.Parse(date);
        var n = Int32.Parse(anInt);
    } catch (FormatException e) when (e.Message.IndexOf("DateTime") > -1) {
        Console.WriteLine ($"Problem parsing \"{nameof(date)}\" argument");
    } catch (FormatException x) {
        Console.WriteLine ("Problem parsing some other argument");
    }
}
```

### <a name="await-in-catchfinally"></a>await catch... finally...

Le `async` les fonctions introduites dans c# 5 ont été un modificateur de jeu pour la langue. Dans c# 5, `await` n’est pas autorisé dans `catch` et `finally` bloque, ennuyeux avec la valeur de la `async/await` fonctionnalité. C# 6 supprime cette limitation, ce qui permet des résultats asynchrones d’être constamment attendue par le biais du programme comme indiqué dans l’extrait de code suivant :

```csharp
async void SomeMethod()
{
    try {
        //...etc...
    } catch (Exception x) {
        var diagnosticData = await GenerateDiagnosticsAsync (x);
        Logger.log (diagnosticData);
    } finally {
        await someObject.FinalizeAsync ();
    }
}
```

## <a name="summary"></a>Récapitulatif

Le langage c# continue d’évoluer afin d’améliorer la productivité des développeurs tout en promouvant les bonnes pratiques et outils de prise en charge. Ce document a fourni une vue d’ensemble des nouvelles fonctionnalités du langage c# 6 et a démontré brièvement la façon dont ils sont utilisés.

## <a name="related-links"></a>Liens associés

- [Nouvelles fonctionnalités de langage dans c# 6](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)
- [À l’aide de c# 6 classeur](https://developer.xamarin.com/workbooks/csharp/csharp6/csharp6.workbook)
