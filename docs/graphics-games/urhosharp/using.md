---
title: À l’aide de UrhoSharp pour générer un jeu 3D
description: Ce document fournit une vue d’ensemble de UrhoSharp, décrivant les scènes, composants, formes, appareils photo, actions, l’entrée d’utilisateur, son et bien plus encore.
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: eb1e93e47528e801da08f402f452e0e8ce5014d8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784037"
---
# <a name="using-urhosharp-to-build-a-3d-game"></a>À l’aide de UrhoSharp pour générer un jeu 3D

Avant de créer votre premier jeu, vous souhaitez obtenir être familiarisé avec les concepts de base : comment le programme d’installation de votre scène charger les ressources (il contient une illustration) et comment créer des interactions simples pour votre jeu.

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>En arrière-plan, les nœuds, les composants et les appareils photo

Le modèle de la scène peut être décrit comme un graphique basé sur le composant de la scène. La séquence se compose d’une hiérarchie de nœuds de la scène, à partir du nœud racine, qui représente également la scène entière. Chaque [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/) a une transformation 3D (position, rotation et l’échelle), un nom, un ID, ainsi qu’un nombre arbitraire de composants.  Composants mettre un nœud à la durée de vie, elles permettent d’ajouter une représentation visuelle ([`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)), ils peuvent émettre son ([`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource)), qu’ils puissent fournir une limite de collision, et ainsi de suite.

Vous pouvez créer vos séquences et les nœuds le programme d’installation à l’aide de la [Urho éditeur](#UrhoEditor), ou vous pouvez effectuer les opérations à partir de votre code c#.  Dans ce document, nous allons examiner les éléments de configuration à l’aide de code, comme elles illustrent les éléments nécessaires pour vous aider à apparaître dans l’écran

Outre la définition de votre scène, vous devez configurer un [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/), il s’agit de l’élément qui détermine quel obtenir apparaît à l’utilisateur.

### <a name="setting-up-your-scene"></a>Configuration de votre scène.

Vous créez généralement ce formulaire votre méthode de démarrage :

```csharp
var scene = new Scene ();
// Create the Octree component to the scene. This is required before
// adding any drawable components, or else nothing will show up. The
// default octree volume will be from -1000, -1000, -1000) to
//(1000, 1000, 1000) in world coordinates; it is also legal to place
// objects outside the volume but their visibility can then not be
// checked in a hierarchically optimizing manner
scene.CreateComponent<Octree> ();
// Create a child scene node (at world origin) and a StaticModel
// component into it. Set the StaticModel to show a simple plane mesh
// with a "stone" material. Note that naming the scene nodes is
// optional. Scale the scene node larger (100 x 100 world units)
var planeNode = scene.CreateChild("Plane");
planeNode.Scale = new Vector3 (100, 1, 100);
var planeObject = planeNode.CreateComponent<StaticModel> ();
planeObject.Model = ResourceCache.GetModel ("Models/Plane.mdl");
planeObject.SetMaterial(ResourceCache.GetMaterial("Materials/StoneTiled.xml"));
```

### <a name="components"></a>Composants

Rendu d’objets 3D, sons, physique et logique sous forme de script des mises à jour sont tous activés en créant des différents composants dans les nœuds en appelant [ `CreateComponent<T>()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateComponent%3CT%3E/p/Urho.CreateMode/System.UInt32/).  Par exemple, le programme d’installation votre nœud et le composant clair comme suit :

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

Nous avons créé ci-dessus un nœud portant le nom «`DirectionalLight`» et définissez une direction pour, mais rien d’autre.  Maintenant, nous pouvons nous tourner du nœud ci-dessus dans un nœud émetteur de lumière, en attachant une [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) composant, avec `CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

Les composants créés dans le `Scene` lui-même jouent un rôle particulier : pour implémenter des fonctionnalités à l’échelle de la scène. Ils doivent être créés avant tous les autres composants et sont les suivantes :

* [`Octree`](https://developer.xamarin.com/api/type/Urho.Octree/): implémente le partitionnement spatial et l’accélération des requêtes de visibilité. Sans cette 3D les objets ne peuvent pas être rendus.
* [`PhysicsWorld`](https://developer.xamarin.com/api/type/Urho.Physics.PhysicsWorld/): implémente la simulation physique. Physique tels que les composants [ `RigidBody` ](https://developer.xamarin.com/api/type/Urho.Physics.RigidBody/) ou [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) ne peut pas fonctionner correctement sans cela.
* [`DebugRenderer`](https://developer.xamarin.com/api/type/Urho.DebugRenderer/): implémente déboguer le rendu de géométrie.

Composants ordinaires, comme [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light), [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera) ou [ `StaticModel` ](https://developer.xamarin.com/api/type/Urho.StaticModel) ne doivent pas être créées directement dans le [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene), mais plutôt dans les nœuds enfants.

La bibliothèque est fourni avec un large éventail de composants que vous pouvez attacher à vos nœuds de leur donner vie : éléments visible par l’utilisateur (modèles), sons, rigides, les formes de collision, appareils photo, sources de lumière, émetteurs de particules et bien plus encore.

### <a name="shapes"></a>Formes

Pour des raisons pratiques, les diverses formes sont disponibles en tant que nœuds simples dans l’espace de noms Urho.Shapes.  Notamment, les zones, domaines, cônes, cylindres et plans.

### <a name="camera-and-viewport"></a>Appareil photo et la fenêtre d’affichage

Tout comme la lumière, les caméras sont des composants, vous devez attacher le composant à un nœud, cette opération s’effectue comme suit :

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

Avec cette option, vous avez créé un appareil photo et vous avez placé l’appareil photo dans le monde 3D, l’étape suivante consiste à informer les `Application` qu’il s’agit de l’appareil photo que vous souhaitez utiliser, cela est effectué avec le code suivant :

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

Et maintenant, vous devez être en mesure de voir les résultats de votre création.

### <a name="identification-and-scene-hierarchy"></a>Hiérarchie d’identification et de la scène

Contrairement aux nœuds, les composants n’ont pas de noms ; composants présents dans le même nœud sont uniquement identifiés par leur type et les index dans la liste des composants du nœud, qui est remplie dans l’ordre de création, par exemple, vous pouvez récupérer le [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light) composant hors du `lightNode` objet ci-dessus comme suit :

```csharp
var myLight = lightNode.GetComponent<Light>();
```

Vous pouvez également obtenir une liste de tous les composants en récupérant le [ `Components` ](https://developer.xamarin.com/api/property/Urho.Node.Components/) propriété qui retourne un `IList<Component>` que vous pouvez utiliser.

Lors de la création, à la fois des nœuds et des composants obtiennent les ID d’entier globale de scène. Ils peuvent être interrogées à partir de la scène en utilisant les fonctions [ `GetNode(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetNode/p/System.UInt32/) et [ `GetComponent(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetComponent/p/System.UInt32/). Cela est beaucoup plus rapide par exemple récursives requêtes de nœud basée sur le nom de la scène.

Il n’existe aucun concept intégré d’une entité ou un objet de jeu ; au lieu de cela, il est sur le programmeur pour décider de la hiérarchie de nœuds et dans les nœuds à placer la logique de script. En règle générale, les libre de déplacer des objets dans le monde 3D doivent être créés en tant qu’enfants du nœud racine. Nœuds peuvent être créés avec ou sans un nom à l’aide de [ `CreateChild()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateChild/p/System.String/Urho.CreateMode/System.UInt32/). L’unicité des noms de nœud n’est pas appliquée.

Chaque fois qu’il existe certaines composition hiérarchique, il est recommandé (et en fait nécessaire, étant donné que les composants n’ont pas leurs propres transformations 3D) pour créer un nœud enfant.

Par exemple si un caractère a été maintenant un objet dans la main, l’objet doit avoir son propre nœud, ce qui serait être apparenté à OS de main du caractère (également un [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/)).  L’exception est la physique [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape), ce qui peut être offsetted et pivotés individuellement en relation avec le nœud.

Notez que [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Node/)du propriétaire de transformation est ignorée intentionnellement une optimisation lors du calcul de transformations world dérivée de nœuds enfants, par conséquent, sa modification n’a aucun effet et il doit être conservé car il s’agit (position à l’origine, pas de rotation Aucune mise à l’échelle.)

[`Scene`](https://developer.xamarin.com/api/type/Urho.Node/) nœuds peuvent être librement à nouveau apparentés. En revanche les composants toujours appartiennent au nœud attaché à, ils ne peuvent pas être déplacés entre les nœuds. Les nœuds et les composants fournissent un [ `Remove()` ](https://developer.xamarin.com/api/member/Urho.Node.Remove()/) (fonction) pour y parvenir sans avoir à passer par le parent. Une fois que le nœud est supprimé, aucune opération sur le nœud ou d’un composant en question n’est sécurisées après l’appel de cette fonction.

Il est également possible de créer un `Node` qui n’appartient pas à une scène. Cela est utile, par exemple, avec un appareil photo déplacement dans une scène qui peut être chargée ou enregistrée, car, puis l’appareil photo n’est pas enregistrée avec la scène réelle et n’est pas détruit lors du chargement de la scène. Toutefois, notez que création de composants de géométrie, physique ou un script à un nœud détaché, puis déplacement dans une scène provoque ces composants ne fonctionnent ne pas correctement.

### <a name="scene-updates"></a>Mises à jour de la scène

Une séquence dont les mises à jour sont activées (valeur par défaut) sera automatiquement être mise à jour à chaque itération de boucle principale.  L’application [ `SceneUpdate` ](https://developer.xamarin.com/api/event/Urho.Scene.SceneUpdate/) Gestionnaire d’événements est appelé sur celui-ci.

Les composants et les nœuds peuvent être exclus de la mise à jour de la scène en désactivant les, consultez [ `Enabled` ](https://developer.xamarin.com/api/member/Urho.Node.Enabled).  Le comportement varie selon le composant spécifique, mais par exemple la désactivation un composant drawable rend également invisibles, tandis que la désactivation d’un composant source audio désactive le volume. Si un nœud est désactivé, tous ses composants sont traités comme étant désactivés quel que soit leur propre état Activer/désactiver.

## <a name="adding-behavior-to-your-components"></a>Ajouter un comportement à vos composants

La meilleure façon de structurer votre jeu doit rendre votre propre composant qui encapsulent un acteur ou un élément sur votre jeu.  Cela rend la fonctionnalité autonomes, des actifs utilisés pour l’afficher, à son comportement.

La façon la plus simple d’ajouter un comportement à un composant consiste à utiliser des actions, qui sont des instructions que vous pouvez en file d’attente et qui sont associés à la programmation async c#.  Cela permet le comportement de votre composant puisse être très haut niveau et rend plus simple à comprendre ce qui se passe.

Ou bien, vous pouvez contrôler exactement ce qui se passe à votre composant en mettant à jour les propriétés de votre composant sur chaque image (présenté dans la section de comportement basé sur le Frame).

### <a name="actions"></a>Actions

Vous pouvez ajouter le comportement aux nœuds très facilement à l’aide d’Actions.  Actions peuvent modifier différentes propriétés de nœud et les exécuter sur une période de temps ou les répéter un nombre de fois avec une courbe d’animation donnée.

Par exemple, considérez un nœud de « cloud » sur votre scène, vous pouvez en fondu comme suit :

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

Actions sont des objets immuables, ce qui vous permet de réutiliser l’action de différents objets de commande.

Un idiome courant consiste à créer une action qui effectue l’opération inverse :

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

L’exemple suivant serait fondu de l’objet pour vous sur une période de trois secondes.  Vous pouvez également exécuter une action après l’autre, par exemple, vous pourriez tout d’abord déplacer le cloud et puis de le masquer :

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

Si vous souhaitez que les deux actions aient lieu en même temps, vous pouvez utiliser l’action parallèle et fournir toutes les actions que vous souhaitez effectuées en parallèle :

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

Dans l’exemple ci-dessus, le nuage déplace et fondu en même temps.

Vous pouvez remarquer que celles-ci sont à l’aide de c# await, qui vous permet de réfléchir linéairement le comportement que vous souhaitez atteindre.

### <a name="basic-actions"></a>Actions de base

Voici les actions prises en charge dans UrhoSharp :

* Déplacement de nœuds : [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo), [ `MoveBy` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveBy), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `BezierTo` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierTo), [ `BezierBy` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierBy) , [`JumpTo`](https://developer.xamarin.com/api/type/Urho.Actions.JumpTo), [`JumpBy`](https://developer.xamarin.com/api/type/Urho.Actions.JumpBy)
* Rotation de nœuds : [ `RotateTo` ](https://developer.xamarin.com/api/type/Urho.Actions.RotateTo), [`RotateBy`](https://developer.xamarin.com/api/type/Urho.Actions.RotateBy)
* Mise à l’échelle de nœuds : [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo), [`ScaleBy`](https://developer.xamarin.com/api/type/Urho.Actions.ScaleBy)
* Atténuation de nœuds : [ `FadeIn` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeIn), [ `FadeTo` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeTo), [ `FadeOut` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeOut), [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [`Blink`](https://developer.xamarin.com/api/type/Urho.Actions.Blink)
* Teinte : [ `TintTo` ](https://developer.xamarin.com/api/type/Urho.Actions.TintTo), [`TintBy`](https://developer.xamarin.com/api/type/Urho.Actions.TintBy)
* Instant : [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [ `Show` ](https://developer.xamarin.com/api/type/Urho.Actions.Show), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `RemoveSelf` ](https://developer.xamarin.com/api/type/Urho.Actions.RemoveSelf), [`ToggleVisibility`](https://developer.xamarin.com/api/type/Urho.Actions.ToggleVisibility)
* Bouclage : [ `Repeat` ](https://developer.xamarin.com/api/type/Urho.Actions.Repeat), [ `RepeatForever` ](https://developer.xamarin.com/api/type/Urho.Actions.RepeatForever), [`ReverseTime`](https://developer.xamarin.com/api/type/Urho.Actions.ReverseTime)

D’autres fonctionnalités avancées incluent la combinaison de la [ `Spawn` ](https://developer.xamarin.com/api/type/Urho.Actions.Spawn) et [ `Sequence` ](https://developer.xamarin.com/api/type/Urho.Actions.Sequence) actions.

### <a name="easing---controlling-the-speed-of-your-actions"></a>Accélération - contrôle de la vitesse de vos Actions

L’accélération est une manière qui indique la façon dont l’animation sera dérouler, et il peut être beaucoup plus agréable vos animations.  Par défaut vos actions aura un comportement linéaire, par exemple un [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo) action aurait un mouvement très automatisé.  Vous pouvez encapsuler vos Actions dans une action d’accélération pour modifier le comportement, par exemple, l’autre qui serait lentement démarrer un mouvement de le, accélérer et lentement à la fin ([`EasyInOut`](https://developer.xamarin.com/api/type/Urho.Actions.EasyInOut)).

Pour cela, en encapsulant une Action existante dans une action d’accélération, par exemple :

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

Il existe plusieurs modes d’accélération, le graphique suivant affiche les différents types d’accélération et leur comportement de la valeur de l’objet contrôlé sur la période de temps, à partir du début à la fin :

![Modes d’accélération](using-images/easing.png "ce graphique montre les différents types d’accélération et leur comportement de la valeur de l’objet de contrôle sur la période de temps")

### <a name="using-actions-and-async-code"></a>À l’aide d’Actions et le Code asynchrone

Dans votre [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component/) sous-classe, vous devez introduire une méthode asynchrone qui prépare le comportement de votre composant et les fonctionnalités des lecteurs pour celle-ci.
Ensuite, vous devez appeler cette méthode à l’aide du langage c# `await` (mot clé) à partir d’une autre partie de votre programme, soit votre `Application.Start` méthode ou en réponse à un point d’utilisateur ou le récit dans votre application.

Exemple :

```csharp
class Robot : Component {
    public bool IsAlive;
    async void Launch ()
    {
        // Dress up our robot
        var cache = Application.ResourceCache;
        var model = node.CreateComponent<StaticModel>();
        model.Model = cache.GetModel ("robot.mdl"));
        model.SetMaterial (cache.GetMaterial ("robot.xml"));
        Node.SetScale (1);

        // Bring the robot into our scene
        await Node.RunActionsAsync(
            new MoveBy(duration: 0.6f, position: new Vector3(0, -2, 0)));
        // Make him move around to avoid the user fire
        MoveRandomly(minX: 1, maxX: 2, minY: -3, maxY: 3, duration: 1.5f);
        // And simultaneously have him shoot at the user
        StartShooting();
    }

    protected async void MoveRandomly (float minX, float maxX,
                                       float minY, float maxY,
                       float duration)
    {
        while (IsAlive){
            var moveAction = new MoveBy(duration,
                new Vector3(RandomHelper.NextRandom(minX, maxX),
                            RandomHelper.NextRandom(minY, maxY), 0));
            await Node.RunActionsAsync(moveAction, moveAction.Reverse());
        }
    }
    protected async void StartShooting()
    {
        while (IsAlive && Node.Components.Count > 0){
            foreach (var weapon in Node.Components.OfType<Weapon>()){
                await weapon.FireAsync(false);
                if (!IsAlive)
                    return;
            }
            await Node.RunActionsAsync(new DelayTime(0.1f));
        }
    }
}
```

Dans la `Launch` méthode au-dessus de trois actions sont démarrés : le robot entre dans la scène, cette action modifie l’emplacement du nœud sur une période de 0,6 secondes.  Dans la mesure où il s’agit d’une option async, cela se produit simultanément en tant que l’instruction suivante qui correspond à l’appel à `MoveRandomly`.  Cette méthode modifie la position de l’automate en parallèle à un emplacement aléatoire.  Cela est obtenue en exécutant les deux actions composées, le déplacement vers un nouvel emplacement et pour revenir à la version d’origine positionner et répétez cette opération tant que le robot reste actif.  Et pour rendre les choses plus intéressant, le robot sera images simultanément.  La résolution de problèmes ne démarrera que toutes les secondes 0,1.

### <a name="frame-based-behavior-programming"></a>Programmation de comportement basé sur le frame

Si vous souhaitez contrôler le comportement de votre composant sur une base d’image par image au lieu d’utiliser des actions, ce que vous feriez consiste à remplacer le [ `OnUpdate` ](https://developer.xamarin.com/api/member/Urho.Component.OnUpdate) méthode de votre [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component) sous-classe.  Cette méthode est appelée une fois par frame et est appelée uniquement si vous définissez la propriété ReceiveSceneUpdates sur true.

L’exemple suivant montre comment créer un `Rotator` composant, qui est alors attaché à un nœud, ce qui entraîne le nœud faire pivoter :

```csharp
class Rotator : Component {
    public Rotator()
    {
        ReceiveSceneUpdates = true;
    }
    public Vector3 RotationSpeed { get; set; }
    protected override void OnUpdate(float timeStep)
    {
        Node.Rotate(new Quaternion(
            RotationSpeed.X * timeStep,
            RotationSpeed.Y * timeStep,
            RotationSpeed.Z * timeStep),
            TransformSpace.Local);
    }
}
```

Et Voici comment vous joignez ce composant à un nœud :

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>Combinaison de Styles

Vous pouvez utiliser le modèle en fonction async/action pour la programmation d’une grande partie du comportement qui est très pratique pour déclencher et oubliez un style de programmation, mais vous pouvez également régler comportement de votre composant pour également exécuter du code de mise à jour sur chaque image.

Par exemple, dans la démonstration SamplyGame cela est utilisé dans le `Enemy` classe encode les actions d’utilise un comportement de base, mais il garantit également que les composants pointent vers l’utilisateur en définissant la direction du nœud avec `Node.LookAt`:

```csharp
    protected override void OnUpdate(SceneUpdateEventArgs args)
    {
        Node.LookAt(
            new Vector3(0, -3, 0),
            new Vector3(0, 1, -1),
            TransformSpace.World);
        base.OnUpdate(args);
    }
```

## <a name="loading-and-saving-scenes"></a>Le chargement et l’enregistrement en arrière-plan

En arrière-plan permettre être chargés et enregistrés au format XML ; consultez les fonctions [ `LoadXml` ](https://developer.xamarin.com/api/member/Urho.Scene.LoadXml) et [ `SaveXML()` ](https://developer.xamarin.com/api/member/Urho.Scene.SaveXml). Lors du chargée d’une scène, tout le contenu existant dans (nœuds enfants et des composants) est tout d’abord supprimé. Nœuds et des composants qui sont marqués temporaires avec le `Temporary` propriété n’est pas enregistrée. Le sérialiseur gère toutes les propriétés et les composants intégrés, mais il n’est pas assez intelligente gérer les propriétés personnalisées et des champs définis dans les sous-classes de votre composant. Toutefois, il fournit deux méthodes virtuelles pour cela :

* [`OnSerialize`](https://developer.xamarin.com/api/member/Urho.Component.OnSerialize) où vous pouvez enregistrer votre états personnalisés pour la sérialisation

* [`OnDeserialized`](https://developer.xamarin.com/api/member/Urho.Component.OnDeserialize) où vous pouvez obtenir votre états personnalisés enregistrés.

En règle générale, un composant personnalisé doit ressembler à ce qui suit :

```csharp
class MyComponent : Component {
    // Constructor needed for deserialization
    public MyComponent(IntPtr handle) : base(handle) { }
    public MyComponent() { }
    // user defined properties (managed state):
    public Quaternion MyRotation { get; set; }
    public string MyName { get; set; }

    public override void OnSerialize(IComponentSerializer ser)
    {
        // register our properties with their names as keys using nameof()
        ser.Serialize(nameof(MyRotation), MyRotation);
        ser.Serialize(nameof(MyName), MyName);
    }

    public override void OnDeserialize(IComponentDeserializer des)
    {
        MyRotation = des.Deserialize<Quaternion>(nameof(MyRotation));
        MyName = des.Deserialize<string>(nameof(MyName));
    }
    // called when the component is attached to some node
    public override void OnAttachedToNode()
    {
        var node = this.Node;
    }
}
```

### <a name="object-prefabs"></a>Objet Prefabs

Juste de chargement ou de l’enregistrement des scènes entiers n’est pas suffisamment flexible pour les jeux où nouveaux objets doivent être créés de façon dynamique. En revanche, la création d’objets complexes et en définissant leurs propriétés dans le code sera également fastidieux. Pour cette raison, il est également possible d’enregistrer un nœud de scène qui inclut ses nœuds enfants, les composants et les attributs. Ces ultérieurement pour des raisons pratiques peuvent être chargées en tant que groupe.  Ce type d’un objet enregistré est souvent appelé PREFABRIQUE polyvalent. Il y a trois manières pour effectuer cette opération :

- Dans le code en appelant [ `Node.SaveXml` ](https://developer.xamarin.com/api/member/Urho.Node.SaveXml) sur le nœud
- Dans l’éditeur, en sélectionnant le nœud dans la fenêtre de la hiérarchie, « enregistrer le nœud en tant que » dans le menu « Fichier ».
- À l’aide de la commande « nœud » dans `AssetImporter`, qui va enregistrer la hiérarchie de nœud de scène et que tous les modèles contiennent dans l’élément multimédia d’entrée (par exemple). un fichier Collada)

Pour instancier le nœud enregistré dans une scène, appelez [ `InstantiateXml()` ](https://developer.xamarin.com/api/member/Urho.Scene.InstantiateXml). Le nœud sera créé en tant qu’enfant de la scène, mais peut être librement apparenté à nouveau par la suite. Rotation de placement du nœud et de position doivent être spécifiés. Le code suivant montre comment instancier PREFABRIQUE polyvalent `Ninja.xm` à une scène avec rotation et de position souhaitée :

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>Événements

UrhoObjects élever un nombre d’événements, ceux-ci sont exposés en tant qu’événements c# sur les différentes classes qui génèrent les.  En plus du langage c#-modèle d’événement de base, il est également possible d’utiliser une la `SubscribeToXXX` méthodes qui vous permettra de vous abonner et conserver un jeton d’abonnement que vous pouvez utiliser ultérieurement pour annuler l’abonnement.  La différence est que le premier autorise plusieurs appelants pour vous abonner, tandis que la deuxième uniquement permet à un utilisateur, mais permet le style de lambda agréable approche à utiliser et encore, permettant à la suppression de l’abonnement.  Ils s’excluent mutuellement.

Lorsque vous vous abonnez à un événement, vous devez fournir une méthode qui accepte un argument avec les arguments d’événement approprié.

Il s’agit par exemple, comment vous abonnez à un événement sur le bouton :

```csharp
public void override Start ()
{
    UI.MouseButtonDown += HandleMouseButtonDown;
}

void HandleMouseButtonDown(MouseButtonDownEventArgs args)
{
    Console.WriteLine ("button pressed");
}
```

Avec le style de l’expression lambda :

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

Parfois, vous souhaitez cesser de recevoir des notifications pour l’événement, dans ce cas, enregistrez la valeur de retour de l’appel à `SubscribeTo` (méthode) et appeler la méthode Unsubscribe dessus :

```csharp
Subscription mouseSub;

public void override Start ()
{
    mouseSub = UI.SubscribeToMouseButtonDown (args => {
    Console.WriteLine ("button pressed");
      mouseSub.Unsubscribe ();
    };
}
```

Le paramètre reçu par le Gestionnaire d’événements est une classe d’arguments des événements fortement typés qui est spécifiques à chaque événement et qui contient la charge utile d’événement.

## <a name="responding-to-user-input"></a>Répondre à l’entrée utilisateur

Vous pouvez vous abonner à différents événements, tels que les séquences de touches vers le bas, en vous abonnant à l’événement et répondre à l’entrée en cours de livraison :

```csharp
Start ()
{
    UI.KeyDown += HandleKeyDown;
}

void HandleKeyDown (KeyDownEventArgs arg)
{
     switch (arg.Key){
     case Key.Esc: Engine.Exit (); return;
}
```

Mais dans de nombreux scénarios, vous souhaitez que les gestionnaires de mise à jour de scène pour vérifier l’état actuel des touches lorsqu’ils sont mis à jour et mettre à jour de votre code en conséquence.  Par exemple, le texte suivant peut servir à mettre à jour l’emplacement de la caméra en fonction de l’entrée de clavier :

```csharp
protected override void OnUpdate(float timeStep)
{
    Input input = Input;
    // Movement speed as world units per second
    const float moveSpeed = 4.0f;
    // Read WASD keys and move the camera scene node to the
    // corresponding direction if they are pressed
    if (input.GetKeyDown(Key.W))
        CameraNode.Translate(Vector3.UnitY * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.S))
        CameraNode.Translate(new Vector3(0.0f, -1.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.A))
        CameraNode.Translate(new Vector3(-1.0f, 0.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.D))
        CameraNode.Translate(Vector3.UnitX * moveSpeed * timeStep, TransformSpace.Local);
}
```

## <a name="resources-assets"></a>Ressources (actifs)

Les ressources incluent la plupart des éléments qui sont chargés à partir de stockage de masse au cours de l’initialisation ou runtime dans UrhoSharp :

- [`Animation`](https://developer.xamarin.com/api/type/Urho.Animation/) -utilisé pour les animations squelettes
- [`Image`](https://developer.xamarin.com/api/type/Urho.Resources.Image) -représente les images stockées dans divers formats graphiques
- [`Model`](https://developer.xamarin.com/api/type/Urho.Model/) -Modèles 3D
- [`Material`](https://developer.xamarin.com/api/type/Urho.Material) -permet de restituer les modèles de documents.
- [`ParticleEffect`](https://developer.xamarin.com/api/type/Urho.ParticleEffect)- [Décrit](http://urho3d.github.io/documentation/1.4/_particles.html) un émetteur de particules fonctionnement, consultez «[particules](#particles)» ci-dessous.
- [`Shader`](https://developer.xamarin.com/api/type/Urho.Shader) -les nuanceurs personnalisés
- [`Sound`](https://developer.xamarin.com/api/type/Urho.Audio.Sound) -sons pour la lecture, consultez «[son](#sound)» ci-dessous.
- [`Technique`](https://developer.xamarin.com/api/type/Urho.Technique/) -techniques de rendu matériel
- [`Texture2D`](https://developer.xamarin.com/api/type/Urho.Urho2D.Texture2D/) -Texture 2D
- [`Texture3D`](https://developer.xamarin.com/api/type/Urho.Texture3D/) -Texture 3D
- [`TextureCube`](https://developer.xamarin.com/api/type/Urho.TextureCube/) : Texture du cube
- `XmlFile`

Ils sont gérés et chargés par le [ `ResourceCache` ](https://developer.xamarin.com/api/type/Urho.Resources.ResourceCache/) sous-système (disponible en tant que [ `Application.ResourceCache` ](https://developer.xamarin.com/api/property/Urho.Application.ResourceCache/)).

Les ressources eux-mêmes sont identifiés par leurs chemins d’accès de fichier, par rapport à des répertoires de la ressource inscrit ou des fichiers de package. Par défaut, le moteur enregistre les répertoires de la ressource `Data` et `CoreData`, ou les packages `Data.pak` et `CoreData.pak` s’ils existent.

Si le chargement d’une ressource échoue, une erreur est consignée et une référence null est retournée.

L’exemple suivant montre une manière classique de l’extraction d’une ressource à partir du cache de ressources.  Dans ce cas, une texture pour un élément d’interface utilisateur, cette méthode utilise le `ResourceCache` propriété à partir de la `Application` classe.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

Ressources peuvent également être créés manuellement et stockées dans le cache de ressources comme s’ils avaient été chargés à partir du disque.

Budgets de mémoire peuvent être définies par le type de ressource : si ressources consomment davantage de mémoire que ne le permet, les ressources plus anciens seront supprimés du cache, sinon en cours d’utilisation plus. Par défaut les budgets mémoire sont définis un nombre illimités.

### <a name="bringing-3d-models-and-images"></a>Mettre les modèles 3D et des Images

Urho3D essaie d’utiliser des formats de fichier existants chaque fois que possible et définir des formats de fichiers personnalisés uniquement en cas d’absolue nécessité comme pour les modèles (*.mdl) et des animations (*.ani). Pour ces types d’éléments multimédias, Urho fournit un convertisseur - [AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) qui peut consommer de nombreux formats 3D courants tels que fbx, dae, 3ds et obj, etc.

Il existe également un complément utile pour Blender [ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) qui peut exporter vos ressources Blender dans le format qui convient pour Urho3D.

### <a name="background-loading-of-resources"></a>Chargement en arrière-plan des ressources

Normalement, lors de la demande de ressources à l’aide d’une de la `ResourceCache`de `Get` (méthode), ils sont chargés immédiatement dans le thread principal, ce qui peut prendre plusieurs millisecondes pour toutes les étapes requises (charger le fichier à partir du disque, analyser des données, télécharger vers le GPU, si nécessaire ) et peut par conséquent entraîner supprime de la fréquence d’images.

Si vous connaissez à l’avance les ressources auxquelles vous avez besoin, vous pouvez demander à être chargés dans un thread d’arrière-plan en appelant `BackgroundLoadResource()`. Vous pouvez vous abonner à l’événement de chargement de la ressource en arrière-plan à l’aide de la `SubscribeToResourceBackgroundLoaded` (méthode). Il indique si le chargement a été réellement une réussite ou échec. En fonction de la ressource, seule une partie du processus de chargement peut être déplacée vers un thread d’arrière-plan, par exemple l’étape de téléchargement GPU en terminant doit toujours se produire dans le thread principal. Notez que si vous appelez la méthodes d’une ressource qui est en file d’attente pour l’arrière-plan du chargement de chargement de la ressource, le thread principal semblera bloquée jusqu'à la fin de son chargement.

La fonctionnalité de chargement de scène asynchrone `LoadAsync()` et `LoadAsyncXML()` a la possibilité de charge en arrière-plan les ressources avant de pouvoir passer à charger le contenu de la scène. Il peut également être utilisé pour charger uniquement les ressources sans modifier la scène, en spécifiant le `LoadMode.ResourcesOnly`. Cela permet de préparer un fichier prefab scène ou l’objet pour l’instanciation rapide.

Pour finir la durée maximale (en millisecondes) consacré à chaque image sur finition arrière-plan chargé de ressources peuvent être configurés en définissant le `FinishBackgroundResourcesMs` propriété sur le `ResourceCache`.

<a name="sound"/>

## <a name="sound"></a>Signal sonore

Son est une partie importante de jeu, et l’infrastructure UrhoSharp offre un moyen de lire des sons dans votre jeu.  Lire des sons en attachant un [ `SoundSource` ](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource/) composant un [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node) et puis lire un fichier nommé à partir de vos ressources.

Voici comment procéder :

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>Particules

PARTICULES offrent un moyen simple d’ajouter certains effets simples et peu coûteuse pour votre application.  Vous pouvez utiliser les particules stockés au format PEX, à l’aide des outils tels que [ http://onebyonedesign.com/flash/particleeditor/ ](http://onebyonedesign.com/flash/particleeditor/).

Particules sont des composants qui peuvent être ajoutés à un nœud.  Vous avez besoin d’appeler du nœud `CreateComponent<ParticleEmitter2D>` méthode pour créer la particule, puis configurer la particule en définissant la propriété d’effet à un effet 2D qui est chargé à partir du cache de ressources.

Par exemple, vous pouvez appeler cette méthode dans votre composant pour afficher des particules sont rendus sous la forme d’un éclatement lorsqu’il atteint :

```csharp
public async void Explode (Component target)
{
    // show a small explosion when the missile reaches an aircraft.
    var cache = Application.ResourceCache;
    var explosionNode = Scene.CreateChild();
    explosionNode.Position = target.Node.WorldPosition;
    explosionNode.SetScale(1f);
    var particle = explosionNode.CreateComponent<ParticleEmitter2D>();
    particle.Effect = cache.GetParticleEffect2D("explosion.pex");
    var scaleAction = new ScaleTo(0.5f, 0f);
    await explosionNode.RunActionsAsync(
        scaleAction, new DelayTime(0.5f));
    explosionNode.Remove();
}
```

Le code ci-dessus crée un nœud d’explosion qui est attaché à votre composant en cours, à l’intérieur de ce nœud explosion nous créons un émetteur de particules 2D et configurez-le en définissant la propriété de l’effet.  Nous exécutons les deux actions, un qui s’adapte le nœud pour être plus petite et un qui laisse avec cette taille de 0,5 secondes.  Ensuite, nous supprimer l’explosion, ce qui supprime également l’effet de particules à partir de l’écran.

La particule ci-dessus s’affiche comme suit lors de l’utilisation d’une texture de la sphère :

![PARTICULES avec une texture sphère](using-images/image-1.png "la particule ci-dessus s’affiche comme suit lors de l’utilisation d’une texture sphère")

Et c’est ce que cela donne si vous utilisez une texture sous forme de blocs :

![PARTICULES avec une texture zone](using-images/image-2.png "et c’est qu’il donne l’impression à l’aide d’une texture sous forme de blocs")

## <a name="multithreading-support"></a>Prise en charge le multithreading

UrhoSharp est une bibliothèque de thread unique.  Cela signifie que vous ne devriez pas à appeler des méthodes dans UrhoSharp à partir d’un thread d’arrière-plan ou risque d’endommager l’état de l’application et probablement bloquer votre application.

Si vous souhaitez exécuter du code en arrière-plan et ensuite mettre à jour des composants Urho sur l’interface utilisateur principale, vous pouvez utiliser la [ `Application.InvokeOnMain(Action)` ](https://developer.xamarin.com/api/member/Urho.Application.InvokeOnMain) (méthode).  En outre, vous pouvez utiliser await de c# et .NET tâche API pour vous assurer que le code est exécuté sur le thread approprié.

## <a name="urhoeditor"></a>UrhoEditor

Vous pouvez télécharger l’éditeur Urho pour votre plateforme dans le [site Web de Urho](http://urho3d.github.io/), accéder aux téléchargements et choisir la version la plus récente.

## <a name="copyrights"></a>Droits d’auteur

Cette documentation contient du contenu d’origine à partir de Xamarin, Inc, mais dessine largement à partir de la documentation open source pour le projet Urho3D et contient des captures d’écran à partir du projet Cocos2D.

## <a name="related-links"></a>Liens associés

- [Planète terre classeur](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [Exploration des coordonnées de classeur](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
