---
title: Messages d’erreur Xamarin.Mac (mmp)
description: Ce document répertorie les erreurs générées par mmp, l’outil utilisé pour empaqueter des assemblys compilés dans une application Mac exécutable.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/27/2018
ms.openlocfilehash: 640d1adc048bec167508d8c288b62d498f061b0d
ms.sourcegitcommit: 4859da8772dbe920fdd653180450e5ddfb436718
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234971"
---
# <a name="xamarinmac-error-messages-mmp"></a>Messages d’erreur Xamarin.Mac (mmp)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx : messages d’erreur mmp

Par exemple, paramètres, environnement, manque d’outils.

<a name="MM0000" />

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpsgithubcomxamarinxamarin-maciosissuesnew"></a>MM0000 : Une erreur inattendue : le bogue signalez à https://github.com/xamarin/xamarin-macios/issues/new

Une condition d’erreur inattendue s’est produite. Veuillez [un rapport de bogue](https://github.com/xamarin/xamarin-macios/issues/new) avec autant d’informations que possible, notamment :

* Build complète des journaux, avec un maximum de commentaires (par exemple, `-v -v -v -v` dans le **arguments mmp supplémentaires**) ;
* Un cas de test minimale pour reproduire l’erreur ; et
* Toutes les informations de version

Le moyen le plus simple pour obtenir des informations de version exacte consiste à utiliser le **Xamarin Studio** menu, **sur Xamarin Studio** élément, **afficher les détails** bouton et copier/coller la version informations (vous pouvez utiliser la **copier les informations** bouton).

<a name="MM0001" />

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001 : Cette version de Xamarin.Mac nécessite Mono {0} (la version actuelle de Mono est {1}). Mettez à jour le Mono.framework à partir de http://mono-project.com/Downloads

<a name="MM0003" />

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003 : Nom de l’Application '{0}.exe' est en conflit avec un nom d’assembly (.dll) produit ou le Kit de développement logiciel.

<a name="MM0007" />

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007 : L’assembly racine '{0}' n’existe pas

<a name="MM0008" />

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008 : Vous devez fournir un seul assembly racine uniquement, elle est trouvé {0} assemblys : '{1}'

<a name="MM0010" />

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010 : N’a pas pu analyser les arguments de ligne de commande : {0}

<!-- 0013 is unused -->

<a name="MM0016" />

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016 : L’option '{0}' a été déconseillée.

<a name="MM0017" />

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017 : Vous devez fournir un assembly de racine

<a name="MM0018" />

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018 : Argument de ligne de commande inconnu : '{0}'

<a name="MM0020" />

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020 : Les options valides pour '{0}« sont »{1}».

<a name="MM0023" />

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023 : Nom de l’Application '{0}.exe' est en conflit avec un autre assembly de l’utilisateur.

<a name="MM0026" />

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026 : N’a pas pu analyser l’argument de ligne de commande '{0}' : {1}

<a name="MM0043" />

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043 : Le garbage collector de Boehm n’est pas pris en charge. Le RÉCUPÉRATEUR de mémoire SGen a été sélectionné à la place.

<a name="MM0050" />

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050 : Impossible d’indiquer un assembly de racine si--non-racine-assembly est passé.

<a name="MM0051" />

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051 : Un répertoire de sortie (--sortie) est requis si--non-racine-assembly est passé.

<a name="MM0053" />

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053 : Impossible de désactiver nouveau refcount avec l’API unifiée.

<a name="MM0056" />

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056 : Impossible de trouver Xcode à notre emplacements par défaut. Installez Xcode, ou passez un chemin d’accès personnalisé à l’aide de--sdkroot =<path>

<a name="MM0059" />

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059 : N’a pas pu trouver le Xcode actuellement sélectionnée sur le système : {0};

<a name="MM0060" />

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060 : Xcode actuellement sélectionné est introuvable sur le système. 'xcode-select--print-path' retourné '{0}', mais ce répertoire n’existe pas.

<a name="MM0068" />

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068 : La valeur non valide pour le framework cible : {0}.

<a name="MM0071" />

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071 : Plateforme inconnue : *. Cela indique généralement un bogue dans Xamarin.Mac ; Veuillez soumettre un rapport de bogue à https://bugzilla.xamarin.com avec un cas de test.

Cela indique généralement un bogue dans Xamarin.Mac ; Veuillez soumettre un rapport de bogue à [ https://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) avec un cas de test.

<a name="MM0079" />

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079 : Erreur interne - aucun fichier exécutable a été copiée dans le bundle d’applications. Veuillez contacter 'support@xamarin.com'

<a name="MM0080" />

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080 : La désactivation de NewRefCount,--new-refcount:false, est déconseillée.

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091" />

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091 : Cette version de Xamarin.Mac nécessite le * Kit de développement logiciel (livré avec Xcode *). Soit mise à niveau de Xcode pour obtenir les fichiers d’en-tête requis ou utiliser le bureau d’enregistrement dynamique ou la valeur est le comportement de l’éditeur de liens managé lien plateforme ou le lien les SDK Framework uniquement (éviter les nouvelles API).

Xamarin.Mac requiert les fichiers d’en-tête, à partir de la version SDK spécifié dans le message d’erreur, pour générer votre application avec le bureau d’enregistrement statique. La méthode recommandée pour corriger cette erreur consiste à mettre à niveau de Xcode pour obtenir le Kit de développement logiciel requis, cela inclut tous les fichiers d’en-tête requis. Si vous disposez de plusieurs versions de Xcode installée, ou que vous souhaitez utiliser un Xcode dans un emplacement non défini par défaut, veillez à définir l’emplacement de Xcode correct dans les préférences de l’IDE.

Une solution potentielle, alternative, consiste à activer l’éditeur de liens géré. Cela supprimera inutilisé API, y compris, dans la plupart des cas, la nouvelle API où les fichiers d’en-tête sont manquantes (ou incomplets). Cela ne fonctionne pas si votre projet utilise des API qui a été introduit dans un kit SDK plus récent que celui votre Xcode fournit toutefois.

Une solution alternative ensuite potentielle, est à utiliser le bureau d’enregistrement dynamique à la place. Cela s’imposer un coût de démarrage en inscrivant dynamiquement des types mais la spécification de fichier d’en-tête. 

Une solution de dernière-paille consisterait à utiliser une version antérieure de Xamarin.Mac, qui prend en charge le SDK de votre projet requiert.

<a name="MM0097" />

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097 : fichier de machine.config '{0}' est introuvable.

<a name="MM0098" />

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098 : Compilation AOT est uniquement disponible sur unifiés

<a name="MM0099" />

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MM0099 : Erreur interne {0}. Veuillez soumettre un rapport de bogue avec un cas de test (http://bugzilla.xamarin.com).

<a name="MM0114" />

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114 : Compilation AOT hybride compilation nécessite tous les assemblys AOT compilé.

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx : copie d’un fichier / liens symboliques (projet lié)

<a name="MM1034" />

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034 : N’a pas pu créer le lien symbolique '{file}' -> '{'target} : erreur {number}

### <a name="mm14xx-product-assemblies"></a>MM14xx : Assemblages de produit

<a name="MM1401" />

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401 : Requis '{0}' assembly est manquant dans les références

<a name="MM1402" />

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402 : L’assembly '{0}' n’est pas compatible avec cet outil

<a name="MM1403" />

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403 : {0} '{1}' est introuvable. Framework cible '{0}' est inutilisable pour empaqueter l’application.

<a name="MM1404" />

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404 : Framework cible '{0}' n’est pas valide.

<a name="MM1405" />

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405 : useFullXamMacFramework doit toujours cibler le framework .NET 4.5, pas '{0}' qui n’est pas valide

<a name="MM1406" />

#### <a name="mm1406-target-framework-0-is-invalid-when-targetting-xamarinmac-45-net-framwork"></a>MM1406 : Framework cible '{0}' n’est pas valide lorsque ciblant .NET 4.5 de Xamarin.Mac framwork.

<a name="MM1407" />

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407 : Incompatibilité entre la référence de Xamarin.Mac '{0}« et un framework cible sélectionné »{1}».

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx : Erreurs Assembly collecte (ne nécessitant ne pas l’éditeur de liens)

<a name="MM1501" />

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501 : Impossible de résoudre le référence : {0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600" />

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600 : Pas une Mach-O bibliothèque dynamique (en-tête inconnu ' 0 x{0}») : {1}.

<a name="MM1601" />

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601 : Pas une bibliothèque statique (en-tête inconnu '{0}») : {1}.

<a name="MM1602" />

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602 : Pas une Mach-O bibliothèque dynamique (en-tête inconnu ' 0 x{0}») : {1}.

<a name="MM1603" />

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603 : Le format inconnu pour fat entrée à la position {0} dans {1}.

<a name="MM1604" />

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604 : Type de fichier {0} n’est pas un fichier MachO ({1}).

## <a name="mm2xxx-linker"></a>MM2xxx : l’éditeur de liens

### <a name="mm20xx-linker-general-errors"></a>MM20xx : Erreurs de l’éditeur de liens (général)

<a name="MM2001" />

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001 : Impossible de lier les assemblys

<a name="MM2002" />

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002 : Impossible de résoudre le référence : {0}

<a name="MM2003" />

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003 : Option '{0}' va être ignorée car la liaison est désactivée.

<a name="MM2004" />

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004 : Fichier de définitions de l’éditeur de liens supplémentaires '{0}' n’a pas été trouvé.

<a name="MM2005" />

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005 : Les définitions à partir de '{0}' ne peut pas être analysée.

<a name="MM2006" />

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006 : Bibliothèque Native '{0}' a été référencé mais est introuvable.

<a name="MM2007" />

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007 : API unifiée de Xamarin.Mac par rapport à un profil .NET complet ne gère pas de liaison. Passer l’indicateur - nolink.

<a name="MM2009" />

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009 : Référencé par {0}.{1}     ** Ce message est lié à MM2006 **

<a name="MM2010" />

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010 : HttpMessageHandler inconnu `{0}`. Les valeurs valides sont HttpClientHandler (valeur par défaut), CFNetworkHandler ou NSUrlSessionHandler

<a name="MM2011" />

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011 : TLSProvider inconnu '{0}.  Les valeurs valides sont par défaut ou appletls

<a name="MM2012" />

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012 : Premier seule {0} de {1} « Référencée par « avertissements affichés. ** Ce message lié à 2009 \*\*

<a name="MM2013" />

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013 : Impossible de résoudre la référence à «{0}«, référencé dans »{1}». L’application n’inclut pas l’assembly référencé et risque d’échouer lors de l’exécution.

<a name="MM2014" />

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014 : Les Extensions de Xamarin.Mac ne prennent pas en charge le liaison. La demande de liaison est ignorée. ** Ce message est obsolète dans XM 3.6 et + \*\*

<!-- 2015 used by mtouch -->

<a name="MM2016" />

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016 : TlsProvider non valide `{0}` option. La seule valeur valide `{1}` sera utilisé.

<a name="MM2017" />

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017 : N’a pas pu traiter la description XML : {0}

<a name="MM202x" />

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x : Échec de l’optimiseur de liaison traitement `...`.

<a name="MM2100" />

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100 : Xamarin.Mac classique API ne prend pas en charge la liaison de plateforme.

<a name="MM2103" />

#### <a name="mm2103-error-processing-assembly--"></a>MM2103 : Traitement de l’erreur assembly '\*' : *

Une erreur inattendue s’est produite lors du traitement d’un assembly.

L’assembly qui provoque le problème est nommé dans le message d’erreur. Pour résoudre ce problème à l’assembly devrez être fournie dans un [des bogues, rapport](https://bugzilla.xamarin.com) ainsi que d’un journal de génération complète avec le niveau de détail activée (par exemple, `-v -v -v -v` dans le **arguments mtouch supplémentaires**).

<a name="MM2104" />

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104 : Impossible de lier l’assembly '{0}' car il est en mode mixte.

Les assemblys en mode mixte ne peuvent pas être traités par l’éditeur de liens.

Consultez https://msdn.microsoft.com/library/x0w2664k.aspx pour plus d’informations sur les assemblys en mode mixte.

## <a name="mm3xxx-aot"></a>MM3xxx : AOA

### <a name="mm30xx-aot-general-errors"></a>MM30xx : Erreurs de compilation AOT (général)

<a name="MM3001" />

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001 : Ne peut pas AOT l’assembly '{0}'

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009" />

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009 : AOT de '{0}' a été demandée mais n’a été trouvé

<a name="MM3010" />

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010 : Exclusion AOA de '{0}' a été demandée mais n’a été trouvé

## <a name="mm4xxx-code-generation"></a>MM4xxx : génération de code

### <a name="mm40xx-driverm"></a>MM40xx : driver.m

<a name="MM4001" />

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001 : Impossible de développer le modèle principal à `{0}`.

### <a name="mm41xx-registrar"></a>MM41xx : bureau d’enregistrement

<a name="MM4134" />

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134 : Votre application est à l’aide de la «{0}' framework, qui n’est pas inclus dans le SDK de MacOS que vous utilisez pour générer votre application (cette infrastructure a été introduite dans OSX {2}, tandis que vous créez avec MacOS {1} SDK.) Cette configuration n’est pas pris en charge avec le bureau d’enregistrement statique (pass--registrar : dynamiques comme argument mmp supplémentaires dans Build Mac possibilité de votre projet de sélectionner). Sélectionnez également un SDK plus récente dans les options de Build Mac de votre application.

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx : GCC et la chaîne d’outils

### <a name="mm51xx-compilation"></a>MM51xx : compilation

<a name="MM5101" />

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101 : Manquant '{0}' compilateur. Installez Xcode composant « Outils de ligne de commande ».

<!-- 5102 used by mtouch -->

<a name="MM5103" />

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103 : Échec de compilation. Code d’erreur - {0}. Veuillez soumettre un rapport de bogue à http://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx : liaison

<a name="MM5202" />

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202 : Mono.framework MDK est manquant. Installez le MDK pour votre version Mono.framework à partir de http://mono-project.com/Downloads

<a name="MM5203" />

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203 : Impossible de trouver libxammac.a, probablement en raison d’une installation endommagée de Xamarin.Mac. Réinstallez Xamarin.Mac.

<a name="MM5204" />

#### <a name="mm5204-invalid-architecture-x8664-is-only-supported-on-non-classic-profiles"></a>MM5204 : Architecture non valide. x86_64 est uniquement pris en charge sur les profils non classique.

<a name="MM5205" />

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x8664-when---profilemobile"></a>MM5205 : Architecture non valide '{0}'. Architectures valides sont i386 et x86_64 (profil lorsque--= mobile).

<a name="MM5218" />

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218 : Impossible d’ignorer le symbole dynamique {symbole} (--symbole dynamique ignorer = {symbol}), car il n’a pas été détecté comme un symbole dynamique.

Consultez le [mtouch équivalent avertissement](~/ios/troubleshooting/mtouch-errors.md#MT5218).

<!-- 5206 used by mtouch -->
<!-- 5207 used by mtouch -->
<!-- 5208 used by mtouch -->
<!-- 5209 used by mtouch -->
<!-- 5210 used by mtouch -->
<!-- 5211 used by mtouch -->
<!-- 5212 used by mtouch -->
<!-- 5213 used by mtouch -->
<!-- 5214 used by mtouch -->
<!-- 5215 used by mtouch -->
<!-- 5216 used by mtouch -->
<!-- 5217 used by mtouch -->

### <a name="mm53xx-other-tools"></a>MM53xx : autres outils

<a name="MM5301" />

#### <a name="mm5301-pkg-config-could-not-be-found-please-install-the-monoframework-from-httpmono-projectcomdownloads"></a>MM5301 : pkg-config est introuvable. Installez le Mono.framework à partir de http://mono-project.com/Downloads

<!-- 5302 used by mtouch -->
<!-- 5303 used by mtouch -->
<!-- 5304 used by mtouch -->

<a name="MM5305" />

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305 : Outil de 'otool' manquant. Installez Xcode composant « Outils de ligne de commande »

<a name="MM5306" />

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306 : Les dépendances manquantes. Installez Xcode composant « Outils de ligne de commande »

<a name="MM5308" />

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308 : Contrat de licence Xcode peut ne pas avoir été acceptée.  Veuillez lancer Xcode.

<a name="MM5309" />

#### <a name="mm5309-native-linking-failed-with-error-code-1--check-build-log-for-details"></a>MM5309 : Liaison natif a échoué avec le code d’erreur 1.  Vérifiez le journal de génération pour plus d’informations.

<a name="MM5310" />

#### <a name="mm5310-installnametool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310 : install_name_tool a échoué avec un code d’erreur «{0}». Vérifiez le journal de génération pour plus d’informations.

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx : runtime

### <a name="mm800x-misc"></a>MM800x : divers

<!-- 8000 used by mtouch -->
<!-- 8001 used by mtouch -->
<!-- 8002 used by mtouch -->
<!-- 8003 used by mtouch -->
<!-- 8004 used by mtouch -->
<!-- 8005 used by mtouch -->
<!-- 8006 used by mtouch -->
<!-- 8007 used by mtouch -->
<!-- 8008 used by mtouch -->
<!-- 8009 used by mtouch -->
<!-- 8010 used by mtouch -->
<!-- 8011 used by mtouch -->
<!-- 8012 used by mtouch -->
<!-- 8013 used by mtouch -->
<!-- 8014 used by mtouch -->
<!-- 8015 used by mtouch -->
<!-- 8016 used by mtouch -->

<a name="MM8017" />

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017 : Le garbage collector de Boehm n’est pas pris en charge. Utilisez plutôt la SGen.

