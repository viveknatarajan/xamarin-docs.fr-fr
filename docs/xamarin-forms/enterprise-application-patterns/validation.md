---
title: Validation dans les applications d’entreprise
description: Ce chapitre explique comment l’application mobile eShopOnContainers effectue la validation des entrées d’utilisateur. Cela inclut en spécifiant des règles de validation, déclencher la validation et l’affichage des erreurs de validation.
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 30c507a1b78600ef1b9a96e37f88904daaf82987
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528570"
---
# <a name="validation-in-enterprise-apps"></a>Validation dans les applications d’entreprise

N’importe quelle application qui accepte les entrées d’utilisateurs doit garantir que l’entrée est valide. Une application peut, par exemple, recherchez d’entrée qui contienne uniquement des caractères dans une plage particulière, est d’une certaine longueur ou correspond à un format particulier. Sans validation, un utilisateur peut fournir des données d’origine de l’application en échec. Validation applique des règles d’entreprise et empêche une personne malveillante d’injecter des données malveillantes.

Dans le contexte de Model-View-ViewModel (MVVM) de modèle, un modèle de vue ou modèle souvent devront effectuer la validation de données et de signaler des erreurs de validation à la vue afin que l’utilisateur pour les corriger. L’application mobile eShopOnContainers effectue la validation côté client synchrone d’afficher les propriétés de modèle et informe l’utilisateur des éventuelles erreurs de validation en surbrillance le contrôle qui contient les données non valides et en affichant des messages d’erreur qui informent l’utilisateur de la raison pour laquelle les données ne sont pas valides. Figure 6-1 montre les classes impliquées dans la validation dans l’application mobile eShopOnContainers.

[![](validation-images/validation.png "Classes de validation dans l’application mobile eShopOnContainers")](validation-images/validation-large.png#lightbox "classes de Validation dans l’application mobile eShopOnContainers")

**Figure 6-1**: les classes de Validation dans l’application mobile eShopOnContainers

Afficher les propriétés de modèle qui nécessite une validation sont de type `ValidatableObject<T>`et chaque `ValidatableObject<T>` instance a des règles de validation ajoutés à son `Validations` propriété. Validation est appelée à partir du modèle de vue en appelant le `Validate` méthode de la `ValidatableObject<T>` instance, qui Récupère la validation des règles et les exécute sur le `ValidatableObject<T>` `Value` propriété. Des erreurs de validation sont placés dans le `Errors` propriété de la `ValidatableObject<T>` instance et le `IsValid` propriété de la `ValidatableObject<T>` instance est mise à jour pour indiquer si la validation a réussi ou échoué.

Notification de modification de propriété est fournie par le `ExtendedBindableObject` (classe) et donc un [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle peut être lié à la `IsValid` propriété de `ValidatableObject<T>` instance dans la classe de modèle de vue d’être averti des ou non les données entrées sont valides.

## <a name="specifying-validation-rules"></a>Spécification des règles de Validation

Règles de validation sont spécifiés en créant une classe qui dérive de la `IValidationRule<T>` interface, ce qui est indiqué dans l’exemple de code suivant :

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Cette interface spécifie qu’une classe de règle de validation doit fournir un `boolean` `Check` méthode qui est utilisé pour effectuer l’opération de validation requise, et un `ValidationMessage` propriété dont la valeur est le message d’erreur de validation qui s’affichera si la validation échoue.

Le code suivant montre l’exemple le `IsNotNullOrEmptyRule<T>` règle de validation, qui est utilisé pour effectuer la validation du nom d’utilisateur et mot de passe entré par l’utilisateur le `LoginView` lors de l’utilisation des services fictifs dans l’application mobile eShopOnContainers :

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

Le `Check` méthode retourne un `boolean` indiquant si l’argument de valeur est `null`, vide ou se compose uniquement d’espaces blancs.

Bien que ne pas utilisée par l’application mobile eShopOnContainers, l’exemple de code suivant montre une règle de validation pour valider les adresses de messagerie :

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

Le `Check` méthode retourne un `boolean` indiquant si l’argument de valeur est une adresse e-mail valide. Pour cela en recherchant l’argument de valeur pour la première occurrence d’un modèle d’expression régulière spécifié dans le `Regex` constructeur. Si le modèle d’expression régulière a été trouvé dans la chaîne d’entrée peut être déterminé en vérifiant la valeur de la `Match` l’objet `Success` propriété.

> [!NOTE]
> Validation de propriété permettre impliquent parfois des propriétés dépendantes. Voici un exemple de propriétés dépendantes : le jeu de valeurs valides pour la propriété A dépend de la valeur particulière qui a été définie dans la propriété B. Pour vérifier que la valeur de propriété A est une des valeurs autorisées impliquerait la récupération de la valeur de propriété B. En outre, lorsque la valeur de propriété B change, la propriété A doit être revalidé.

## <a name="adding-validation-rules-to-a-property"></a>Ajout de règles de Validation à une propriété

Dans l’application mobile eShopOnContainers, afficher les propriétés de modèle qui nécessite une validation sont déclarées comme étant de type `ValidatableObject<T>`, où `T` est le type de données à valider. L’exemple de code suivant montre un exemple de deux de ces propriétés :

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

Pour la validation se produise, les règles de validation doivent être ajoutés à la `Validations` collection de chaque `ValidatableObject<T>` de l’instance, comme illustré dans l’exemple de code suivant :

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

Cette méthode ajoute la `IsNotNullOrEmptyRule<T>` règle de validation pour le `Validations` collection de chaque `ValidatableObject<T>` instance, en spécifiant des valeurs pour la règle de validation `ValidationMessage` propriété qui spécifie le message d’erreur de validation qui s’affichera si la validation échoue.

## <a name="triggering-validation"></a>Déclencher la Validation

L’approche de validation utilisée dans l’application mobile eShopOnContainers permettre déclencher manuellement la validation d’une propriété et automatiquement la validation de déclencheur lorsqu’une propriété change.

### <a name="triggering-validation-manually"></a>Déclencher manuellement la Validation

La validation peut être déclenchée manuellement pour une propriété de modèle de vue. Par exemple, cela se produit dans l’application mobile eShopOnContainers le lorsque l’utilisateur actionne le **connexion** situé dans le `LoginView`, lorsque vous utilisez des services fictifs. La commande délégué appelle le `MockSignInAsync` méthode dans le `LoginViewModel`, qui appelle la validation en exécutant la `Validate` (méthode), ce qui est illustré dans l’exemple de code suivant :

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

Le `Validate` méthode effectue la validation du nom d’utilisateur et mot de passe entré par l’utilisateur le `LoginView`, en appelant la méthode Validate sur chaque `ValidatableObject<T>` instance. L’exemple de code suivant montre la méthode Validate de la `ValidatableObject<T>` classe :

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

Cette méthode efface le `Errors` collection, puis récupère aucune validation des règles qui ont été ajoutés à l’objet `Validations` collection. Le `Check` méthode pour chaque règle de validation récupéré est exécutée et le `ValidationMessage` valeur de propriété pour une règle de validation qui ne parvient pas à valider les données est ajoutée à la `Errors` collection de la `ValidatableObject<T>` instance. Enfin, le `IsValid` propriété est définie, et sa valeur est retournée à la méthode appelante, indiquant si la validation a réussi ou échoué.

### <a name="triggering-validation-when-properties-change"></a>Déclencher la Validation lors de la modification des propriétés

Validation peut également être déclenchée chaque fois qu’une propriété liée change. Par exemple, lorsqu’une liaison bidirectionnelle dans le `LoginView` définit le `UserName` ou `Password` propriété, la validation est déclenchée. L’exemple de code suivant montre comment cela se produit :

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

Le [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle se lie le `UserName.Value` propriété de la `ValidatableObject<T>` instance et du contrôle `Behaviors` collection a un `EventToCommandBehavior` instance ajouté à ce dernier. Ce comportement s’exécute le `ValidateUserNameCommand` en réponse à la [`TextChanged`] événement déclenché sur le `Entry`, qui est déclenché lorsque le texte dans le `Entry` modifications. À son tour, le `ValidateUserNameCommand` délégué exécute le `ValidateUserName` (méthode), qui exécute le `Validate` méthode sur le `ValidatableObject<T>` instance. Par conséquent, chaque fois que l’utilisateur entre un caractère dans la `Entry` contrôle pour le nom d’utilisateur, la validation des données entrées est effectuée.

Pour plus d’informations sur les comportements, consultez [mise en œuvre des comportements](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Affichage des erreurs de Validation

L’application mobile eShopOnContainers informe l’utilisateur des éventuelles erreurs de validation en surbrillance le contrôle qui contient les données non valides d’une ligne rouge et en affichant un message d’erreur qui informe l’utilisateur pourquoi les données ne sont pas valides sous le contrôle contenant le données non valides. Lorsque les données non valides sont résolues, la ligne devient noire et le message d’erreur est supprimé. Figure 6-2 montre la vue de connexion dans l’application mobile eShopOnContainers lorsque des erreurs de validation sont présentes.

![](validation-images/validation-login.png "Affichage des erreurs de validation lors de la connexion")

**Figure 6-2 :** affichage des erreurs de validation lors de la connexion

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Mise en surbrillance d’un contrôle qui contient des données non valides

Le `LineColorBehavior` comportement attaché est utilisé pour mettre en surbrillance [ `Entry` ](xref:Xamarin.Forms.Entry) contrôles où les erreurs de validation se sont produites. Le code suivant montre l’exemple comment la `LineColorBehavior` comportement attachée est attaché à un `Entry` contrôle :

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

Le [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle consomme un style explicite, ce qui est illustré dans l’exemple de code suivant :

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

Ce style définit la `ApplyLineColor` et `LineColor` joint les propriétés de la `LineColorBehavior` attaché comportement sur le [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle. Pour plus d’informations sur les styles, consultez [Styles](~/xamarin-forms/user-interface/styles/index.md).

Lorsque la valeur de la `ApplyLineColor` propriété jointe est l’ensemble ou modifications, la `LineColorBehavior` comportement attaché exécute le `OnApplyLineColorChanged` (méthode), ce qui est illustré dans l’exemple de code suivant :

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

Les paramètres de cette méthode fournissent l’instance du contrôle auquel le comportement est attaché au et les valeurs anciennes et nouvelles de la `ApplyLineColor` propriété jointe. Le `EntryLineColorEffect` classe est ajoutée pour le contrôle [ `Effects` ](xref:Xamarin.Forms.Element.Effects) collection si la `ApplyLineColor` propriété jointe est `true`, sinon il est supprimé à partir du contrôle `Effects` collection. Pour plus d’informations sur les comportements, consultez [mise en œuvre des comportements](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

Le `EntryLineColorEffect` sous-classes le [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) classe et est illustré dans l’exemple de code suivant :

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

Le [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) classe représente un effet indépendant de la plateforme qui encapsule un effet interne qui est spécifique à la plateforme. Cela simplifie le processus de suppression d’effet, car il n’existe aucun accès lors de la compilation pour les informations de type pour un effet spécifique à la plateforme. Le `EntryLineColorEffect` appelle le constructeur de classe de base, en passant un paramètre comportant une concaténation de la résolution de nom de groupe et l’ID unique qui est spécifiée sur chaque classe de l’effet de spécifique à la plateforme.

Le code suivant montre l’exemple le `eShopOnContainers.EntryLineColorEffect` implémentation pour iOS :

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

Le `OnAttached` méthode récupère le contrôle natif pour le Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) contrôler et met à jour de la couleur de ligne en appelant le `UpdateLineColor` (méthode). Le `OnElementPropertyChanged` remplacement répond aux modifications de propriété pouvant être liée sur le `Entry` contrôle en mettant à jour la couleur de ligne si le fichier joint `LineColor` des modifications de propriété, ou la [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) propriété de la `Entry`modifications. Pour plus d’informations sur les effets, consultez [effets](~/xamarin-forms/app-fundamentals/effects/index.md).

Lorsque les données valides sont entrées dans le [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle, il appliquera une ligne noire vers le bas du contrôle, pour indiquer qu’il n’existe aucune erreur de validation. Figure 6-3 montre un exemple.

![](validation-images/validation-blackline.png "Ligne noire n’indiquant aucune erreur de validation")

**Figure 6-3**: ligne noire n’indiquant aucune erreur de validation

Le [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle possède également un [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) ajouté à son [ `Triggers` ](xref:Xamarin.Forms.VisualElement.Triggers) collection. Le code suivant montre l’exemple le `DataTrigger`:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

Cela [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) surveille le `UserName.IsValid` propriété et s’il s’agit de valeur devient `false`, il exécute le [ `Setter` ](xref:Xamarin.Forms.Setter), quelles modifications le `LineColor` attaché propriété de la `LineColorBehavior` attaché comportement en rouge. Figure 6-4 montre un exemple.

![](validation-images/validation-redline.png "Ligne rouge indiquant l’erreur de validation")

**Figure 6-4**: ligne rouge indiquant l’erreur de validation

La ligne dans le [ `Entry` ](xref:Xamarin.Forms.Entry) contrôle reste rouge, tandis que les données entrées ne sont pas valides, sinon elle deviendra noir pour indiquer que les données entrées sont valides.

Pour plus d’informations sur les déclencheurs, consultez [déclencheurs](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Affichage des Messages d’erreur

L’interface utilisateur affiche les messages d’erreur de validation dans les contrôles Label sous chaque contrôle dont les données d’échec de la validation. Le code suivant montre l’exemple le [ `Label` ](xref:Xamarin.Forms.Label) qui affiche un message d’erreur de validation si l’utilisateur n’a pas entré un nom d’utilisateur valide :

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Chaque [ `Label` ](xref:Xamarin.Forms.Label) lie à la `Errors` propriété de l’objet de modèle de vue qui est en cours de validation. Le `Errors` propriété est fournie par le `ValidatableObject<T>` classe et est de type `List<string>`. Étant donné que le `Errors` propriété peut contenir plusieurs erreurs de validation, le `FirstValidationErrorConverter` instance est utilisée pour récupérer la première erreur de la collection pour l’affichage.

## <a name="summary"></a>Récapitulatif

L’application mobile eShopOnContainers effectue la validation côté client synchrone d’afficher les propriétés de modèle et informe l’utilisateur des éventuelles erreurs de validation en surbrillance le contrôle qui contient les données non valides et en affichant des messages d’erreur qui informent l’utilisateur Pourquoi les données ne sont pas valides.

Afficher les propriétés de modèle qui nécessite une validation sont de type `ValidatableObject<T>`et chaque `ValidatableObject<T>` instance a des règles de validation ajoutés à son `Validations` propriété. Validation est appelée à partir du modèle de vue en appelant le `Validate` méthode de la `ValidatableObject<T>` instance, qui Récupère la validation des règles et les exécute sur le `ValidatableObject<T>` `Value` propriété. Des erreurs de validation sont placés dans le `Errors` propriété de la `ValidatableObject<T>`instance et le `IsValid` propriété de la `ValidatableObject<T>` instance est mise à jour pour indiquer si la validation a réussi ou échoué.


## <a name="related-links"></a>Liens associés

- [Téléchargez le livre électronique (PDF de 2 Mo)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (exemple)](https://github.com/dotnet-architecture/eShopOnContainers)
