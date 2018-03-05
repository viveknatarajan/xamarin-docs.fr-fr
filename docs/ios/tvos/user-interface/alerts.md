---
title: Utilisation des alertes
description: "Cet article traite de travailler avec UIAlertController pour afficher un message d’alerte à l’utilisateur dans Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 593e17096d92d9a9764545149991817fa616e2fc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-alerts"></a>Utilisation des alertes

_Cet article traite de travailler avec UIAlertController pour afficher un message d’alerte à l’utilisateur dans Xamarin.tvOS._


Si vous avez besoin attirer l’attention de l’utilisateur tvOS ou demander l’autorisation d’effectuer une action destructrice (par exemple, la suppression d’un fichier), vous pouvez présenter un message d’alerte à l’aide de la `UIAlertViewController`:

[ ![](alerts-images/alert01.png "Un exemple UIAlertViewController")](alerts-images/alert01.png)

Si plus de l’affichage d’un message, vous pouvez ajouter des boutons et des champs de texte à une alerte pour autoriser l’utilisateur à répondre aux actions et fournir des commentaires.

<a name="About-Alerts" />

## <a name="about-alerts"></a>À propos des alertes

Comme indiqué ci-dessus, les alertes sont utilisées pour obtenir l’attention des utilisateurs et de les informer de l’état de vos commentaires de l’application ou de la demande. Alertes doivent présenter un titre, elles peuvent éventuellement avoir un message et un ou plusieurs boutons ou des champs de texte.

[ ![](alerts-images/alert04.png "Une alerte de l’exemple")](alerts-images/alert04.png)

Apple a les suggestions suivantes pour l’utilisation des alertes :

- **Utilisez les alertes avec parcimonie** -alertes interrompre le flux de l’utilisateur avec l’application et interrompre l’expérience utilisateur et par conséquent, doivent uniquement servir pour des situations importantes telles que les notifications d’erreur, les achats dans l’application et les actions de destruction. 
- **Fournit des options utiles** - si l’alerte présente les options à l’utilisateur, votre doit s’assurer que chaque option offre des informations critiques et fournit des actions utiles pour l’utilisateur.

<a name="Alert-Titles-and-Messages" />

### <a name="alert-titles-and-messages"></a>Les titres et les Messages d’alerte

Apple a les suggestions suivantes pour présenter le titre et le Message facultatif d’une alerte :

- **Utiliser des titres de Multiword** -du titre de l’alerte un doit obtenir le point de la situation sur clairement tout en restant simple. Un titre de mot unique fournit rarement suffisamment d’informations.
- **Utilisez des titres descriptifs qui ne nécessitent pas d’un Message** - dans la mesure du possible, envisagez de rendre le titre de l’alerte descriptif suffisent à ce que le texte du Message facultatif n’est pas obligatoire. 
- **Vérifiez le Message d’une courte, une phrase complète** : si le Message facultatif est nécessaire pour obtenir le point de l’alerte sur, conserver aussi simples que possible et le rendre une phrase complète avec la casse appropriée et la ponctuation.

<a name="Alert-Buttons" />

### <a name="alert-buttons"></a>Boutons d’alerte

Apple a la suggestion suivante pour ajouter des boutons à une alerte :

- **Limite à deux boutons** - dans la mesure du possible, limitez l’alerte à un maximum de deux boutons. Les alertes de bouton unique fournissent plus d’informations, mais aucune action. Deux alertes de bouton fournissent un simple Oui/non choix d’action.
- **Utilisez Succinct, titres logique** -Simple une ou deux word titres qui décrivent clairement l’action du bouton fonctionnent le mieux. Pour plus d’informations, consultez notre [utilisation des boutons](~/ios/tvos/user-interface/buttons.md) documentation.
- **Marquer clairement les boutons Destructive** - pour les boutons qui exécutent une action destructrice (par exemple, la suppression d’un fichier) les marquent clairement avec les `UIAlertActionStyle.Destructive` style.

<a name="Displaying-an-Alert" />

## <a name="displaying-an-alert"></a>Affiche une alerte

Pour afficher une alerte, vous créez une instance de la `UIAlertViewController` et le configurer en ajoutant des Actions (boutons) et en sélectionnant le style de l’alerte. Par exemple, le code suivant affiche une alerte OK/Annuler :

```csharp
const string title = "A Short Title is Best";
const string message = "A message should be a short, complete sentence.";
const string acceptButtonTitle = "OK";
const string cancelButtonTitle = "Cancel";
const string deleteButtonTitle = "Delete";
...
        
var alertController = UIAlertController.Create (title, message, UIAlertControllerStyle.Alert);

// Create the action.
var acceptAction = UIAlertAction.Create (acceptButtonTitle, UIAlertActionStyle.Default, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

var cancelAction = UIAlertAction.Create (cancelButtonTitle, UIAlertActionStyle.Cancel, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

// Add the actions.
alertController.AddAction (acceptAction);
alertController.AddAction (cancelAction);
PresentViewController (alertController, true, null);
```

Examinons ce code en détail. Tout d’abord, nous créer une alerte avec le titre donné et le message :

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

Ensuite, pour chaque bouton que vous souhaitez afficher dans l’alerte, nous créer une Action définissant le titre du bouton, son style et l’action que nous voulons prendre si le bouton est activé :

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ => 
    // Do something when the button is pressed
    ...
);
```

Le `UIAlertActionStyle` enum permettent de définir le style du bouton comme une des opérations suivantes :

- **Par défaut** -le bouton est le bouton par défaut sélectionné lorsque l’alerte s’affiche.
- **Annuler** -le bouton est le bouton Annuler pour l’alerte.
- **Destructeur** -met en surbrillance le bouton comme une action destructrice, telles que la suppression d’un fichier. Actuellement, tvOS restitue le destructeur avec un arrière-plan rouge.

Le `AddAction` méthode ajoute l’action donnée à la `UIAlertViewController` et enfin la `PresentViewController (alertController, true, null)` méthode affiche l’alerte donné à l’utilisateur.

<a name="Adding-Text-Fields" />

## <a name="adding-text-fields"></a>Ajout de champs de texte

Outre l’ajout des Actions (boutons) à l’alerte, vous pouvez ajouter des champs de texte pour autoriser l’utilisateur à saisir des informations telles que l’ID d’utilisateur et mots de passe de l’alerte :

[ ![](alerts-images/alert02.png "Champ de texte dans une alerte")](alerts-images/alert02.png)

Si l’utilisateur sélectionne le champ de texte, le clavier standard tvOS s’affiche ce qui leur permet d’entrer une valeur pour le champ :

[ ![](alerts-images/alert03.png "Saisie de texte")](alerts-images/alert03.png)

Le code suivant affiche une alerte OK/Annuler avec un seul champ de texte pour entrer une valeur :

```csharp
UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
UITextField field = null;

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;

    // Initialize field
    field.Placeholder = placeholder;
    field.Text = text;
    field.AutocorrectionType = UITextAutocorrectionType.No;
    field.KeyboardType = UIKeyboardType.Default;
    field.ReturnKeyType = UIReturnKeyType.Done;
    field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

});

// Add cancel button
alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
    // User canceled, do something
    ...
}));

// Add ok button
alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
    // User selected ok, do something
    ...
}));

// Display the alert
controller.PresentViewController(alert,true,null);
```

Le `AddTextField` méthode ajoute un nouveau champ de texte à l’alerte que vous pouvez ensuite configurer en définissant des propriétés telles que l’espace réservé du texte (le texte qui apparaît lorsque le champ est vide), la valeur de texte par défaut et le type de clavier. Exemple :

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

Afin que nous pouvons agir sur la valeur du champ de texte plus tard, nous sommes également enregistrer une copie de l’aide du code suivant :

```csharp
UITextField field = null;
...

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;
    ...
});
```

Une fois que l’utilisateur a entré une valeur dans le champ de texte, nous pouvons utiliser le `field` variable pour accéder à cette valeur.

<a name="Alert-View-Controller-Helper-Class" />

## <a name="alert-view-controller-helper-class"></a>Classe d’assistance de contrôleur affichage des alertes

Étant donné que l’affichage des types simples, courants d’alertes à l’aide de `UIAlertViewController` peut entraîner un peu de code en double, vous pouvez utiliser une classe d’assistance afin de réduire la quantité de code répétitif. Exemple :

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;

namespace UIKit
{
    /// <summary>
    /// Alert view controller is a reusable helper class that makes working with <c>UIAlertViewController</c> alerts
    /// easier in a tvOS app.
    /// </summary>
    public class AlertViewController
    {
        #region Static Methods
        public static UIAlertController PresentOKAlert(string title, string description, UIViewController controller) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Configure the alert
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(action) => {}));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentOKCancelAlert(string title, string description, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentDestructiveAlert(string title, string description, string destructiveAction, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create(destructiveAction,UIAlertActionStyle.Destructive,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentTextInputAlert(string title, string description, string placeholder, string text, UIViewController controller, AlertTextInputDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
            UITextField field = null;

            // Add and configure text field
            alert.AddTextField ((textField) => {
                // Save the field
                field = textField;

                // Initialize field
                field.Placeholder = placeholder;
                field.Text = text;
                field.AutocorrectionType = UITextAutocorrectionType.No;
                field.KeyboardType = UIKeyboardType.Default;
                field.ReturnKeyType = UIReturnKeyType.Done;
                field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

            });

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false,"");
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null && field !=null) {
                    action(true, field.Text);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }
        #endregion

        #region Delegates
        public delegate void AlertOKCancelDelegate(bool OK);
        public delegate void AlertTextInputDelegate(bool OK, string text);
        #endregion
    }
}
```

À l’aide de cette classe, en affichant et en réponse aux alertes simples peuvent être effectuées comme suit :

```csharp
#region Custom Actions
partial void DisplayDestructiveAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentDestructiveAlert("A Short Title is Best","The message should be a short, complete sentence.","Delete",this, (ok) => {
        Console.WriteLine("Destructive Alert: The user selected {0}",ok);
    });
}

partial void DisplayOkCancelAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKCancelAlert("A Short Title is Best","The message should be a short, complete sentence.",this, (ok) => {
        Console.WriteLine("OK/Cancel Alert: The user selected {0}",ok);
    });
}

partial void DisplaySimpleAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKAlert("A Short Title is Best","The message should be a short, complete sentence.",this);
}

partial void DisplayTextInputAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentTextInputAlert("A Short Title is Best","The message should be a short, complete sentence.","placeholder", "", this, (ok, text) => {
        Console.WriteLine("Text Input Alert: The user selected {0} and entered `{1}`",ok,text);
    });
}
#endregion
```


<a name="Summary" />

## <a name="summary"></a>Récapitulatif

Cet article a traité l’utilisation de `UIAlertController` pour afficher un message d’alerte à l’utilisateur dans Xamarin.tvOS. Tout d’abord, il a montré comment afficher une alerte simple et ajouter des boutons. Ensuite, elle a montré comment ajouter des champs de texte à une alerte. Enfin, elle a montré comment utiliser une classe d’assistance afin de réduire la quantité de code répétitif requis pour afficher une alerte.



## <a name="related-links"></a>Liens associés

- [Exemples tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS les Guides Interface humaine](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guide de programmation d’application de tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
