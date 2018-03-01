
Quand vous créez des applications iOS dans Visual Studio et sur l’agent de build Mac, le bundle .app n’est pas copié sur la machine Windows. Xamarin Tools pour Visual Studio 7.4 ajoute une nouvelle propriété `CopyAppBundle`, qui permet aux builds d’intégration continue (CI) de copier les bundles .app sur Windows.

Pour utiliser cette fonctionnalité, ajoutez la propriété `CopyAppBundle` au fichier .csproj sous le groupe de propriétés auquel vous souhaitez appliquer la fonctionnalité. L’exemple suivant montre comment copier le bundle .app sur l’ordinateur Windows pour une build de **débogage** ciblant **iPhoneSimulator** :

    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
        <CopyAppBundle>true</CopyAppBundle>
    </PropertyGroup>

