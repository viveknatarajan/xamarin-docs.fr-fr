---
title: À l’aide de l’infrastructure de langage naturel avec Xamarin.iOS
description: Ce document décrit l’infrastructure de langage naturel. Introduit dans iOS 12, l’infrastructure de langage naturel est l’API iOS préféré à utiliser pour la reconnaissance de langage, l’identification des parties du discours et reconnaissance d’entité nommée.
ms.prod: xamarin
ms.assetid: 126C8764-F873-4EB9-98A3-D82AB5689111
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/20/2018
ms.openlocfilehash: 0b3fb7d467ae64e2cbfdb61644b1537bc5ae1161
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233066"
---
# <a name="using-the-natural-language-framework-with-xamarinios"></a>À l’aide de l’infrastructure de langage naturel avec Xamarin.iOS

Introduit dans iOS 12, l’infrastructure de langage naturel permet le traitement en langage naturel sur l’appareil. Il prend en charge la reconnaissance de langage, la création de jetons et le balisage. Création de jetons fractionne le texte en son composant mots, phrases ou des paragraphes ; balisage identifie les parties du discours, personnes, lieux et les organisations.

L’infrastructure de langage naturel pouvez également utiliser des modèles personnalisés Core ML pour classer et identifier le texte dans des contextes spécifiques.

Le [NSLinguisticTagger](xref:Foundation.NSLinguisticTagger) classe est toujours disponible. Toutefois, l’infrastructure de langage naturel est le mécanisme par défaut à utiliser pour le traitement en langage naturel.

## <a name="sample-app-xamarinnl"></a>Exemple d’application : XamarinNL

Pour savoir comment utiliser l’infrastructure de langage naturel avec Xamarin.iOS, examinons le [XamarinNL, exemple d’application](https://developer.xamarin.com/samples/monotouch/iOS12/XamarinNL).
Cet exemple d’application montre comment utiliser l’infrastructure de langage naturel pour :

- [Reconnaître les langues](#recognizing-languages).
- [Marquer le texte en mots et phrases](#tokenizing-text-into-words-sentences-and-paragraphs).
- [Balise nommée entités et les parties du discours](#tagging-named-entities-and-parts-of-speech).

## <a name="recognizing-languages"></a>Reconnaissance des langues

Le **module de reconnaissance** onglet de l’exemple d’application montre comment utiliser un [`NLLanguageRecognizer`](https://developer.xamarin.com/api/type/NaturalLanguage.NLLanguageRecognizer/)
pour déterminer la langue pour un bloc de texte.

> [!NOTE]
> Reconnaissance de langage est un type spécifique de la classification de texte. L’infrastructure de langage naturel prend également en charge la classification de texte personnalisé via des modèles Core ML fournie par le développeur. Pour plus d’informations, examinez la [présentation de Framework de langage naturel](https://developer.apple.com/videos/play/wwdc2018/713/) session à partir de WWDC 2018.

### <a name="dominant-language"></a>Langage dominant

Appuyez sur la **langage** bouton pour identifier la langue dominante dans l’entrée utilisateur.

Le `HandleDetermineLanguageButtonTap` méthode de la `LanguageRecognizerViewController` utilise le [`GetDominantLanguage`](https://developer.xamarin.com/api/member/NaturalLanguage.NLLanguageRecognizer.GetDominantLanguage/)
méthode d’un `NLLanguageRecognizer` pour extraire le [`NLLanguage`](https://developer.xamarin.com/api/type/NaturalLanguage.NLLanguage/)
pour la langue principale trouvée dans le texte :

```csharp
partial void HandleDetermineLanguageButtonTap(UIButton sender)
{
    UserInput.ResignFirstResponder();
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        NLLanguage lang = NLLanguageRecognizer.GetDominantLanguage(UserInput.Text);
        DominantLanguageLabel.Text = lang.ToString();
    }
}
```

### <a name="language-probabilities"></a>Probabilités de langage

Appuyez sur la **probabilités de langage** bouton pour extraire une liste des hypothèses de langage pour l’entrée utilisateur.

Le `HandleLanguageProbabilitiesButtonTap` méthode de la `LanguageRecognizerViewController` classe instancie un `NLLanguageRecognizer` et lui demande de [`Process`](https://developer.xamarin.com/api/member/NaturalLanguage.NLLanguageRecognizer.Process/)
texte de l’utilisateur. Il appelle ensuite le module de reconnaissance langage [`GetNativeLanguageHypotheses`](https://developer.xamarin.com/api/member/NaturalLanguage.NLLanguageRecognizer.GetNativeLanguageHypotheses)
méthode qui extrait un dictionnaire de langues et les probabilités associées. Le `LanguageRecognizerTableViewController` classe restitue ensuite ces langages et les probabilités.

```csharp
partial void HandleLanguageProbabilitiesButtonTap(UIButton sender)
{
    UserInput.ResignFirstResponder();
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var recognizer = new NLLanguageRecognizer();
        recognizer.Process(UserInput.Text);
        NSDictionary<NSString, NSNumber> probabilities = recognizer.GetNativeLanguageHypotheses(10);
        PerformSegue(ShowLanguageProbabilitiesSegue, this);
    }
}
```

Potentiels `NLLanguage` valeurs incluent :

- `Amharic`
- `Arabic`
- `Armenian`
- `Bengali`
- `Bulgarian`
- `Burmese`
- `Catalan`
- `Cherokee`
- `Croatian`
- `Czech`
- `Danish`
- `Dutch`
- `English`
- `Finnish`
- `French`
- `Georgian`
- `German`
- `Greek`
- `Gujarati`
- `Hebrew`
- `Hindi`
- `Hungarian`
- `Icelandic`
- `Indonesian`
- `Italian`
- `Japanese`
- `Kannada`
- `Khmer`
- `Korean`
- `Lao`
- `Malay`
- `Malayalam`
- `Marathi`
- `Mongolian`
- `Norwegian`
- `Oriya`
- `Persian`
- `Polish`
- `Portuguese`
- `Punjabi`
- `Romanian`
- `Russian`
- `SimplifiedChinese`
- `Sinhalese`
- `Slovak`
- `Spanish`
- `Swedish`
- `Tamil`
- `Telugu`
- `Thai`
- `Tibetan`
- `TraditionalChinese`
- `Turkish`
- `Ukrainian`
- `Undetermined`
- `Urdu`
- `Vietnamese`

Une liste complète des langues prises en charge est disponible en tant que partie de la [`NLLanguage`](https://developer.xamarin.com/api/type/NaturalLanguage.NLLanguage/)
documentation de l’API d’enum.

## <a name="tokenizing-text-into-words-sentences-and-paragraphs"></a>Création de jetons de texte en mots, des phrases et des paragraphes

Le **Générateur de jetons** onglet de l’exemple d’application montre comment séparer un bloc de texte dans ses mots composant ou phrases avec un [ `NLTokenizer` ](https://developer.xamarin.com/api/type/NaturalLanguage.NLTokenizer/).

Appuyez sur la **mots** ou **phrases** bouton pour extraire une liste de jetons. Chaque jeton est associé à un mot ou une phrase dans le texte d’origine.

`ShowTokens` fractionne l’entrée de l’utilisateur en jetons en appelant le [`GetTokens`](https://developer.xamarin.com/api/member/NaturalLanguage.NLTokenizer.GetTokens/)
méthode d’un `NLTokenizer`. Cette méthode retourne un tableau de [`NSValue`](xref:Foundation.NSValue)
objets, chaque habillage un `NSRange` valeur correspondant à un jeton dans le texte d’origine.

```csharp
void ShowTokens(NLTokenUnit unit)
{
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var tokenizer = new NLTokenizer(unit);
        tokenizer.String = UserInput.Text;
        var range = new NSRange(0, UserInput.Text.Length);
        NSValue[] tokens = tokenizer.GetTokens(range);
        PerformSegue(ShowTokensSegue, this);
    }
}
```

`LanguageTokenizerTableViewController` restitue un jeton unique dans chaque cellule de table. Elle extrait un `NSRange` à partir d’un jeton `NSValue`, recherche la chaîne correspondante dans le texte d’origine et définit une étiquette sur la cellule d’affichage de table :

```csharp
public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
    var cell = TableView.DequeueReusableCell(TokenCell);
    NSRange range = Tokens[indexPath.Row].RangeValue;
    cell.TextLabel.Text = Text.Substring((int)range.Location, (int)range.Length);
    return cell;
}
```

## <a name="tagging-named-entities-and-parts-of-speech"></a>Balisage d’entités nommées et les parties du discours

Le **Baliseur** onglet de l’exemple d’application XamarinNL montre comment utiliser la [`NLTagger`](https://developer.xamarin.com/api/type/NaturalLanguage.NLTagger/)
classe permettant d’associer les catégories et les jetons d’une chaîne d’entrée.
L’infrastructure de langage naturel inclut la prise en charge intégrée pour identifier les personnes, lieux, les organisations et grammaticales.

> [!NOTE]
> L’infrastructure de langage naturel prend également en charge les schémas de balisage personnalisés via des modèles Core ML fournie par le développeur. Pour plus d’informations, examinez la [présentation de Framework de langage naturel](https://developer.apple.com/videos/play/wwdc2018/713/) session à partir de WWDC 2018.

Appuyez sur la **entités nommées** ou **grammaticales** bouton à extraire :

- Un tableau de `NSValue` objets, chaque habillage un `NSRange` d’un jeton dans le texte d’origine.
- Un tableau de [ `NLTag` ](https://developer.xamarin.com/api/type/NaturalLanguage.NLTag/) valeurs – catégories pour le `NSValue` jetons dans le même index de tableau.

Dans `LanguageTaggerViewController`, `HandlePartsOfSpeechButtonTap` et `HandleNamedEntitiesButtonTap` chaque appel `ShowTags`, en passant le long d’un [ `NLTagScheme` ](https://developer.xamarin.com/api/type/NaturalLanguage.NLTagScheme/) – soit `NLTagScheme.LexicalClass` (pour les parties du discours) ou `NLTagScheme.NameType` (pour les entités nommées).

`ShowTags` Crée un `NLTagger`, elle l’instancie avec un tableau de `NLTagScheme` types pour laquelle il sera interrogé (dans ce cas, uniquement le passé dans `NLTagScheme` valeur). Il utilise ensuite le [`GetTags`](https://developer.xamarin.com/api/member/NaturalLanguage.NLTagger.GetTags/)
méthode sur le `NLTagger` pour déterminer les balises pertinentes pour le texte dans l’entrée utilisateur.

```csharp
void ShowTags(NLTagScheme tagScheme)
{
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var tagger = new NLTagger(new NLTagScheme[] { tagScheme });
        var range = new NSRange(0, UserInput.Text.Length);
        tagger.String = UserInput.Text;

        NLTag[] tags = tagger.GetTags(range, NLTokenUnit.Word, tagScheme, NLTaggerOptions.OmitWhitespace, out NSValue[] ranges);
        NSValue[] tokenRanges = ranges;
        detailViewTitle = tagScheme == NLTagScheme.NameType ? "Named Entities" : "Parts of Speech";

        PerformSegue(ShowEntitiesSegue, this);
    }
}
```

Les balises sont ensuite affichées dans une table par le `LanguageTaggerTableViewController`.

Potentiels `NLTag` valeurs incluent :

- `Adjective`
- `Adverb`
- `Classifier`
- `CloseParenthesis`
- `CloseQuote`
- `Conjunction`
- `Dash`
- `Determiner`
- `Idiom`
- `Interjection`
- `Noun`
- `Number`
- `OpenParenthesis`
- `OpenQuote`
- `OrganizationName`
- `Other`
- `OtherPunctuation`
- `OtherWhitespace`
- `OtherWord`
- `ParagraphBreak`
- `Particle`
- `PersonalName`
- `PlaceName`
- `Preposition`
- `Pronoun`
- `Punctuation`
- `SentenceTerminator`
- `Verb`
- `Whitespace`
- `Word`
- `WordJoiner`

Une liste complète des balises prises en charge est disponible en tant que partie de la [`NLTag`](https://developer.xamarin.com/api/type/NaturalLanguage.NLTag/)
documentation de l’API d’enum.

## <a name="related-links"></a>Liens connexes

- [Exemple d’application – XamarinNL](https://developer.xamarin.com/samples/monotouch/iOS12/XamarinNL)
- [Présentation du langage naturel Framework](https://developer.apple.com/videos/play/wwdc2018/713/)
- [Langage naturel (Apple)](https://developer.apple.com/documentation/naturallanguage?language=objc)
