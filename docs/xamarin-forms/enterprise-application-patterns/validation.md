---
title: Validation
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 80c78d359761c4383f9abf9338a995e3cc486968
ms.sourcegitcommit: a69439ad4c9fd0abe759143687d3b23582573d90
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2018
---
# <a name="validation"></a>Validation

N’importe quelle application qui accepte les entrées d’utilisateurs doit s’assurer que l’entrée est valide. Une application peut, par exemple, vérifier pour l’entrée qui contienne uniquement des caractères dans une plage particulière, est d’une certaine longueur ou correspond à un format particulier. Sans validation, un utilisateur peut fournir des données qui entraîne l’échec de l’application. Validation applique des règles d’entreprise et empêche une personne malveillante d’injecter des données malveillantes.

Dans le contexte de modèle ViewModel (MVVM) de modèle, un modèle d’affichage ou modèle sera souvent requise pour effectuer la validation des données et de signaler des erreurs de validation à la vue afin que l’utilisateur de les corriger. L’application mobile eShopOnContainers effectue la validation côté client synchrone d’afficher les propriétés de modèle et avertit l’utilisateur de toutes les erreurs de validation en mettant en surbrillance le contrôle qui contient les données non valides et en affichant les messages d’erreur informent l’utilisateur de la raison pour laquelle les données ne sont pas valides. Figure 6-1 affiche les classes impliquées dans l’exécution de la validation dans l’application mobile eShopOnContainers.

[![](validation-images/validation.png "Classes de validation dans l’application mobile eShopOnContainers")](validation-images/validation-large.png#lightbox "classes de Validation dans l’application mobile eShopOnContainers")

**Figure 6-1**: les classes de Validation dans l’application mobile eShopOnContainers

Afficher les propriétés de modèle qui nécessitent une validation sont de type `ValidatableObject<T>`et chaque `ValidatableObject<T>` instance possède des règles de validation ajoutés à ses `Validations` propriété. Validation est appelée à partir du modèle de vue en appelant le `Validate` méthode de la `ValidatableObject<T>` instance, ce qui Récupère la validation des règles et les exécute sur le `ValidatableObject<T>` `Value` propriété. Des erreurs de validation sont placés dans le `Errors` propriété de la `ValidatableObject<T>` instance et le `IsValid` propriété de la `ValidatableObject<T>` instance est mise à jour pour indiquer si la validation a réussi ou échoué.

Notification de modification de propriété est fournie par le `ExtendedBindableObject` (classe) et donc un [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle peut être lié à la `IsValid` propriété de `ValidatableObject<T>` instance dans la classe de modèle de vue d’être averti des ou non les données d’entrée sont valides.

## <a name="specifying-validation-rules"></a>Spécification des règles de Validation

Règles de validation sont spécifiés en créant une classe qui dérive de la `IValidationRule<T>` interface, ce qui est indiqué dans l’exemple de code suivant :

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Cette interface spécifie qu’une classe de règle de validation doit fournir un `boolean` `Check` méthode qui est utilisée pour effectuer la validation requise, et un `ValidationMessage` propriété dont la valeur est le message d’erreur de validation qui s’affichera si la validation échoue.

Montre l’exemple de code suit le `IsNotNullOrEmptyRule<T>` règle de validation, qui est utilisé pour effectuer la validation des nom d’utilisateur et mot de passe entré par l’utilisateur le `LoginView` lors de l’utilisation de services fictives dans l’application mobile eShopOnContainers :

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

Le `Check` méthode retourne un `boolean` indiquant si l’argument de valeur est `null`, vide ou se compose uniquement d’espaces blancs.

Bien que ne pas utilisée par l’application mobile eShopOnContainers, l’exemple de code suivant montre une règle de validation pour valider les adresses de messagerie :

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

Le `Check` méthode retourne un `boolean` qui indique si l’argument de valeur est une adresse de messagerie valide. Pour cela, la recherche de l’argument de valeur pour la première occurrence d’un modèle d’expression régulière spécifiée dans le `Regex` constructeur. Indique si le modèle d’expression régulière a été trouvé dans la chaîne d’entrée peut être déterminé en vérifiant la valeur de la `Match` l’objet `Success` propriété.

> [!NOTE]
> Validation de propriété peut parfois impliquer des propriétés dépendantes. Un exemple de propriétés dépendantes est lorsque l’ensemble des valeurs valides pour la propriété A dépend de la valeur particulière qui a été définie dans la propriété B. Pour vérifier que la valeur de propriété A une des valeurs autorisées implique la récupération de la valeur de propriété B. En outre, lorsque la valeur de propriété B change, la propriété A doit être revalidée.

## <a name="adding-validation-rules-to-a-property"></a>Ajout de règles de Validation à une propriété

Dans l’application mobile eShopOnContainers, afficher les propriétés de modèle qui nécessitent une validation sont déclarées comme étant de type `ValidatableObject<T>`, où `T` est le type de données à valider. L’exemple de code suivant montre un exemple de deux de ces propriétés :

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

Pour effectuer la validation, les règles de validation doivent être ajoutés à la `Validations` collection de chaque `ValidatableObject<T>` de l’instance, comme illustré dans l’exemple de code suivant :

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

Cette méthode ajoute la `IsNotNullOrEmptyRule<T>` règle de validation pour le `Validations` collection de chaque `ValidatableObject<T>` instance, en spécifiant des valeurs pour la règle de validation `ValidationMessage` propriété qui spécifie le message d’erreur de validation qui s’affichera si la validation échoue.

## <a name="triggering-validation"></a>Déclencher la Validation

L’approche de validation utilisée dans l’application mobile eShopOnContainers peut déclencher manuellement la validation d’une propriété et automatiquement la validation de déclencheur lorsqu’une propriété change.

### <a name="triggering-validation-manually"></a>Déclencher manuellement la Validation

La validation peut être déclenchée manuellement pour une propriété de modèle de vue. Par exemple, cela se produit dans l’application mobile eShopOnContainers le lorsque l’utilisateur clique sur le **connexion** bouton sur le `LoginView`, lors de l’utilisation des services fictives. La commande délégué appelle la `MockSignInAsync` méthode dans le `LoginViewModel`, qui appelle la validation par l’exécution de la `Validate` méthode, qui est indiqué dans l’exemple de code suivant :

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

Le `Validate` méthode exécute la validation des nom d’utilisateur et mot de passe entré par l’utilisateur le `LoginView`, en appelant la méthode Validate sur chaque `ValidatableObject<T>` instance. L’exemple de code suivant affiche la méthode de validation à partir de la `ValidatableObject<T>` classe :

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

Cette méthode efface le `Errors` collection, puis récupère aucune validation des règles qui ont été ajoutés à l’objet `Validations` collection. Le `Check` méthode pour chaque règle de validation récupéré est exécutée et le `ValidationMessage` valeur de propriété pour une règle de validation qui ne parvient pas à valider les données est ajoutée à la `Errors` collection de la `ValidatableObject<T>` instance. Enfin, le `IsValid` propriété est définie, et sa valeur est retournée à la méthode appelante, indiquant si la validation a réussi ou échoué.

### <a name="triggering-validation-when-properties-change"></a>Déclencher la Validation lors de la modification des propriétés

La validation peut aussi être déclenchée chaque fois qu’une propriété liée change. Par exemple, lorsqu’une liaison bidirectionnelle dans le `LoginView` définit le `UserName` ou `Password` propriété, la validation est déclenchée. L’exemple de code suivant montre comment cela se produit :

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

Le [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle est lié à la `UserName.Value` propriété de la `ValidatableObject<T>` instance et du contrôle `Behaviors` comporte une `EventToCommandBehavior` instance ajouté à celle-ci. Ce comportement s’exécute le `ValidateUserNameCommand` en réponse à la [`TextChanged`] déclenchement sur le `Entry`, qui est déclenché lorsque le texte dans le `Entry` modifications. À son tour, le `ValidateUserNameCommand` délégué exécute le `ValidateUserName` (méthode), qui exécute le `Validate` méthode sur le `ValidatableObject<T>` instance. Par conséquent, chaque fois que l’utilisateur entre un caractère dans le `Entry` contrôle pour le nom d’utilisateur, la validation des données d’entrée est effectuée.

Pour plus d’informations sur les comportements, consultez [implémentation de comportements](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Affichage des erreurs de Validation

L’application mobile eShopOnContainers informe l’utilisateur de toutes les erreurs de validation en mettant en surbrillance le contrôle qui contient les données non valides d’une ligne rouge et en affichant un message d’erreur informe l’utilisateur pourquoi les données ne seront pas valides sous le contrôle contenant le données non valides. Lorsque les données non valides sont résolues, la ligne devient noire et le message d’erreur est supprimé. Figure 6-2 indique la LoginView dans l’application mobile eShopOnContainers lorsque des erreurs de validation sont présentes.

![](validation-images/validation-login.png "Affichage des erreurs de validation lors de la connexion")

**Figure 6-2 :** affichage des erreurs de validation lors de la connexion

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Mise en surbrillance d’un contrôle qui contient des données non valides

Le `LineColorBehavior` comportement attaché est utilisé pour mettre en surbrillance [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôles où les erreurs de validation se sont produites. Le code suivant montre d’exemple comment la `LineColorBehavior` comportement attachée est attaché à une `Entry` contrôle :

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

Le [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle consomme un style explicite, ce qui est indiqué dans l’exemple de code suivant :

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

Ce style définit la `ApplyLineColor` et `LineColor` joint les propriétés de la `LineColorBehavior` attaché comportement sur le [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle. Pour plus d’informations sur les styles, consultez [Styles](~/xamarin-forms/user-interface/styles/index.md).

Lorsque la valeur de la `ApplyLineColor` propriété jointe est set ou les modifications, la `LineColorBehavior` comportement attaché exécute le `OnApplyLineColorChanged` (méthode), qui est affichée dans l’exemple de code suivant :

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

Les paramètres de cette méthode fournissent l’instance du contrôle que le comportement est attaché et les valeurs anciennes et nouvelles de la `ApplyLineColor` propriété jointe. Le `EntryLineColorEffect` classe est ajoutée au contrôle [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) collection si la `ApplyLineColor` propriété jointe est `true`, dans le cas contraire, il est supprimé à partir du contrôle `Effects` collection. Pour plus d’informations sur les comportements, consultez [implémentation de comportements](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

Le `EntryLineColorEffect` sous-classes le [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) classe et est indiqué dans l’exemple de code suivant :

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

Le [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) classe représente un effet indépendant de la plateforme qui encapsule un effet interne qui est spécifique à la plateforme. Cela simplifie le processus de suppression d’effet, car il n’existe pas lors de la compilation d’accès aux informations de type pour un effet spécifique à la plateforme. Le `EntryLineColorEffect` appelle le constructeur de classe de base, en passant un paramètre comportant une concaténation de la résolution de nom de groupe et l’ID unique qui est spécifié sur chaque classe de l’effet de la plateforme spécifique.

Le code suivant exemple illustre le `eShopOnContainers.EntryLineColorEffect` implémentation pour iOS :

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

Le `OnAttached` méthode récupère le contrôle natif pour le Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) de contrôler et de mises à jour de la couleur de ligne en appelant le `UpdateLineColor` (méthode). Le `OnElementPropertyChanged` remplacement répond aux modifications de propriété pouvant être liée sur le `Entry` contrôle en mettant à jour la couleur de ligne si le fichier joint `LineColor` de propriété est modifiée, ou le [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) propriété de la `Entry`modifications. Pour plus d’informations sur les effets, consultez [effets](~/xamarin-forms/app-fundamentals/effects/index.md).

Lorsque les données valides sont entrées dans le [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) (contrôle), il applique une ligne noire vers le bas du contrôle, pour indiquer qu’il n’existe aucune erreur de validation. Figure 6-3 montre un exemple.

![](validation-images/validation-blackline.png "Ligne noire n’indiquant aucune erreur de validation")

**Figure 6-3**: ligne noire n’indiquant aucune erreur de validation

Le [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle possède également un [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) ajouté à son [ `Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) collection. Le code suivant exemple illustre le `DataTrigger`:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

Cela [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) moniteurs le `UserName.IsValid` propriété et s’il s’agit de valeur devient `false`, il exécute la [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/), les modifications qui le `LineColor` attaché propriété de la `LineColorBehavior` jointe comportement en rouge. Figure 6-4 montre un exemple.

![](validation-images/validation-redline.png "Ligne rouge indiquant l’erreur de validation")

**Figure 6-4**: ligne rouge indiquant l’erreur de validation

La ligne dans le [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) contrôle reste rouge, tandis que les données d’entrée ne seront pas valides, sinon elle deviendra noir pour indiquer que les données d’entrée soient valides.

Pour plus d’informations sur les déclencheurs, consultez [déclencheurs](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Affichage des Messages d’erreur

L’interface utilisateur affiche des messages d’erreur de validation dans les contrôles d’étiquette sous chaque contrôle dont les données d’échec de la validation. Le code suivant exemple illustre le [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) qui affiche un message d’erreur de validation si l’utilisateur n’a pas entré un nom d’utilisateur valide :

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Chaque [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) lie à la `Errors` propriété de l’objet de modèle de vue qui est en cours de validation. Le `Errors` propriété est fournie par le `ValidatableObject<T>` classe et est de type `List<string>`. Étant donné que la `Errors` propriété peut contenir plusieurs erreurs de validation, le `FirstValidationErrorConverter` instance est utilisée pour récupérer la première erreur de la collection pour l’affichage.

## <a name="summary"></a>Récapitulatif

L’application mobile eShopOnContainers effectue la validation côté client synchrone d’afficher les propriétés de modèle et avertit l’utilisateur de toutes les erreurs de validation en mettant en surbrillance le contrôle qui contient les données non valides et en affichant les messages d’erreur informent l’utilisateur Pourquoi les données ne sont pas valides.

Afficher les propriétés de modèle qui nécessitent une validation sont de type `ValidatableObject<T>`et chaque `ValidatableObject<T>` instance possède des règles de validation ajoutés à ses `Validations` propriété. Validation est appelée à partir du modèle de vue en appelant le `Validate` méthode de la `ValidatableObject<T>` instance, ce qui Récupère la validation des règles et les exécute sur le `ValidatableObject<T>` `Value` propriété. Des erreurs de validation sont placés dans le `Errors` propriété de la `ValidatableObject<T>`instance et le `IsValid` propriété de la `ValidatableObject<T>` instance est mise à jour pour indiquer si la validation a réussi ou échoué.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (exemple)](https://github.com/dotnet-architecture/eShopOnContainers)
