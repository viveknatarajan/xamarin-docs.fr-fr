---
title: Vérification de l’état de la batterie
description: Utilisez le DependencyService pour accéder aux informations de batterie en mode natif pour chaque plateforme
ms.prod: xamarin
ms.assetid: CF1C5A73-84ED-407D-BDC5-EB1D83D2D3DB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 3f098e7f403a4f5e9fd924b8745348197cd4f843
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="checking-battery-status"></a>Vérification de l’état de la batterie

Cet article décrit la création d’une application qui vérifie l’état de la batterie. Cet article est basé sur le plug-in de la batterie par James Montemagno. Pour plus d’informations, consultez la [référentiel GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery).

Parce que Xamarin.Forms n’inclut pas de fonctionnalités pour la vérification de l’état actuel de la batterie, cette application doit utiliser [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) pour tirer parti des API natives.  Cet article couvre les étapes suivantes pour l’utilisation de `DependencyService`:

- **[Création de l’Interface](#Creating_the_Interface)**  &ndash; comprendre la façon dont l’interface est créée dans le code partagé.
- **[iOS implémentation](#iOS_Implementation)**  &ndash; apprendre à implémenter l’interface en code natif pour iOS.
- **[Implémentation Android](#Android_Implementation)**  &ndash; apprendre à implémenter l’interface en code natif pour Android.
- **[Implémentation de Windows Phone](#Windows_Phone_Implementation)**  &ndash; apprendre à implémenter l’interface en code natif pour Windows Phone.
- **[Implémentation de la plateforme Windows universelle](#UWPImplementation)**  &ndash; apprendre à implémenter l’interface en code natif pour la plateforme Windows universelle (UWP).
- **[Mise en œuvre dans le Code partagé](#Implementing_in_Shared_Code)**  &ndash; apprendre à utiliser `DependencyService` pour appeler l’implémentation native à partir de code partagé.

Lorsque terminée, l’application à l’aide `DependencyService` aura la structure suivante :

![](battery-info-images/battery-diagram.png "Structure de l’Application DependencyService")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Création de l'interface utilisateur

Tout d’abord, créez une interface dans le code partagé qui exprime la fonctionnalité souhaitée. Dans le cas d’une batterie, la vérification d’application, les informations pertinentes sont le pourcentage de la batterie restante, si l’appareil est en cours de chargement ou non, et comment l’appareil est alimenté :

```csharp
namespace DependencyServiceSample
{
  public enum BatteryStatus
  {
    Charging,
    Discharging,
    Full,
    NotCharging,
    Unknown
  }

  public enum PowerSource
  {
    Battery,
    Ac,
    Usb,
    Wireless,
    Other
  }

  public interface IBattery
  {
    int RemainingChargePercent { get; }
    BatteryStatus Status { get; }
    PowerSource PowerSource { get; }
  }
}
```

Codage de cette interface dans le code partagé permettent à l’application de Xamarin.Forms accéder à l’API de gestion de l’alimentation sur chaque plateforme.

> [!NOTE]
> Classes implémentant l’Interface doivent avoir un constructeur sans paramètre pour travailler avec les `DependencyService`. Les constructeurs ne peuvent pas être définis par les interfaces.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implémentation d’iOS

Le `IBattery` interface doit être implémentée dans chaque projet d’application de plateforme spécifique. L’implémentation iOS utilisera natif [ `UIDevice` ](https://developer.xamarin.com/api/type/UIKit.UIDevice/) API pour accéder aux informations de batterie. Notez que la classe suivante a un constructeur sans paramètre afin que la `DependencyService` peut créer des instances :

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;

namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery
  {
    public BatteryImplementation()
    {
      UIDevice.CurrentDevice.BatteryMonitoringEnabled = true;
    }

    public int RemainingChargePercent
    {
      get
      {
        return (int)(UIDevice.CurrentDevice.BatteryLevel * 100F);
      }
    }

    public BatteryStatus Status
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return BatteryStatus.Charging;
          case UIDeviceBatteryState.Full:
            return BatteryStatus.Full;
          case UIDeviceBatteryState.Unplugged:
            return BatteryStatus.Discharging;
          default:
            return BatteryStatus.Unknown;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Full:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Unplugged:
            return PowerSource.Battery;
          default:
            return PowerSource.Other;
        }
      }
    }
  }
}
```

Enfin, ajoutez ceci `[assembly]` attribut au-dessus de la classe (et en dehors des espaces de noms qui ont été définies), y compris tout `using` instructions :

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;//necessary for registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery {
    ...
```

Cet attribut enregistre la classe en tant qu’implémentation de la `IBattery` interface, ce qui signifie que `DependencyService.Get<IBattery>` peut être utilisé dans le code partagé pour créer une instance de celui-ci :

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implémentation Android

L’implémentation Android utilise le [ `Android.OS.BatteryManager` ](https://developer.xamarin.com/api/type/Android.OS.BatteryManager/) API. Cette implémentation est plus complexe que la version iOS, nécessitant des vérifications pour gérer un manque d’autorisations de la batterie :

```csharp
using System;
using Android;
using Android.Content;
using Android.App;
using Android.OS;
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid;

namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery
  {
    private BatteryBroadcastReceiver batteryReceiver;
    public BatteryImplementation() { }

    public int RemainingChargePercent
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              var level = battery.GetIntExtra(BatteryManager.ExtraLevel, -1);
              var scale = battery.GetIntExtra(BatteryManager.ExtraScale, -1);

              return (int)Math.Floor(level * 100D / scale);
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }

      }
    }

    public DependencyServiceSample.BatteryStatus Status
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;
              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);
              if (isCharging)
                return DependencyServiceSample.BatteryStatus.Charging;

              switch(status)
              {
                case (int)BatteryStatus.Charging:
                  return DependencyServiceSample.BatteryStatus.Charging;
                case (int)BatteryStatus.Discharging:
                  return DependencyServiceSample.BatteryStatus.Discharging;
                case (int)BatteryStatus.Full:
                  return DependencyServiceSample.BatteryStatus.Full;
                case (int)BatteryStatus.NotCharging:
                  return DependencyServiceSample.BatteryStatus.NotCharging;
                default:
                  return DependencyServiceSample.BatteryStatus.Unknown;
              }
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;

              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);

              if (!isCharging)
                return DependencyServiceSample.PowerSource.Battery;
              else if (usbCharge)
                return DependencyServiceSample.PowerSource.Usb;
              else if (acCharge)
                return DependencyServiceSample.PowerSource.Ac;
              else if (wirelessCharge)
                return DependencyServiceSample.PowerSource.Wireless;
              else
                return DependencyServiceSample.PowerSource.Other;
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }
  }
}
```

Ajoutez ceci `[assembly]` attribut au-dessus de la classe (et en dehors des espaces de noms qui ont été définies), y compris tout `using` instructions :

```csharp
...
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery {
    ...
```

Cet attribut enregistre la classe en tant qu’implémentation de la `IBattery` interface, ce qui signifie que `DependencyService.Get<IBattery>` peut être utilisé dans le code partagé peut créer une instance de celui-ci.

<a name="Windows_Phone_Implementation" />

## <a name="windows-phone-implementation"></a>Implémentation de Windows Phone

Cette implémentation est plus limitée que les versions d’Android et iOS, car l’API de la puissance de Windows Phone fournit moins d’informations que les équivalents Android et iOS.

```csharp
using System;
using Windows.ApplicationModel.Core;
using DependencyServiceSample.WinPhone;

namespace DependencyServiceSample.WinPhone
{
  public class BatteryImplementation : IBattery
  {
    private int last;
    private BatteryStatus status = BatteryStatus.Unknown;

    public BatteryImplementation()
    {
      last = DefaultBattery.RemainingChargePercent;
    }

    Windows.Phone.Devices.Power.Battery battery;
    private Windows.Phone.Devices.Power.Battery DefaultBattery
    {
      get { return battery ?? (battery = Windows.Phone.Devices.Power.Battery.GetDefault()); }
    }
    public int RemainingChargePercent
    {
      get
      { return DefaultBattery.RemainingChargePercent; }
    }

    public  BatteryStatus Status
    {
      get { return status; }
    }

    public PowerSource PowerSource
    {
      get
      {
        if (status == BatteryStatus.Full || status == BatteryStatus.Charging)
          return PowerSource.Ac;

        return PowerSource.Battery;
      }
    }
  }
}
```

Ajoutez ceci `[assembly]` attribut au-dessus de la classe (et en dehors des espaces de noms qui ont été définies), y compris tout `using` instructions.

```csharp
using System;
using Windows.ApplicationModel.Core;
using DependencyServiceSample.WinPhone;

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.WinPhone {
  public class BatteryImplementation : IBattery {
    ...
```

Cet attribut enregistre la classe en tant qu’implémentation de la `IBattery` interface, ce qui signifie que `DependencyService.Get<IBattery>` peut être utilisé dans le code partagé pour créer une instance de celui-ci.

<a name="UWPImplementation" />

## <a name="universal-windows-platform-implementation"></a>Mise en œuvre de la plateforme Windows universelle

L’implémentation UWP utilise le `Windows.Devices.Power` API pour obtenir des informations d’état de batterie :

```csharp
using DependencyServiceSample.UWP;
using Xamarin.Forms;

[assembly: Dependency(typeof(BatteryImplementation))]
namespace DependencyServiceSample.UWP
{
    public class BatteryImplementation : IBattery
    {
        private BatteryStatus status = BatteryStatus.Unknown;
        Windows.Devices.Power.Battery battery;

        public BatteryImplementation()
        {
        }

        private Windows.Devices.Power.Battery DefaultBattery
        {
            get
            {
                return battery ?? (battery = Windows.Devices.Power.Battery.AggregateBattery);
            }
        }

        public int RemainingChargePercent
        {
            get
            {
                var finalReport = DefaultBattery.GetReport();
                var finalPercent = -1;

                if (finalReport.RemainingCapacityInMilliwattHours.HasValue && finalReport.FullChargeCapacityInMilliwattHours.HasValue)
                {
                    finalPercent = (int)((finalReport.RemainingCapacityInMilliwattHours.Value /
                        (double)finalReport.FullChargeCapacityInMilliwattHours.Value) * 100);
                }
                return finalPercent;
            }
        }

        public BatteryStatus Status
        {
            get
            {
                var report = DefaultBattery.GetReport();
                var percentage = RemainingChargePercent;

                if (percentage >= 1.0)
                {
                    status = BatteryStatus.Full;
                }
                else if (percentage < 0)
                {
                    status = BatteryStatus.Unknown;
                }
                else
                {
                    switch (report.Status)
                    {
                        case Windows.System.Power.BatteryStatus.Charging:
                            status = BatteryStatus.Charging;
                            break;
                        case Windows.System.Power.BatteryStatus.Discharging:
                            status = BatteryStatus.Discharging;
                            break;
                        case Windows.System.Power.BatteryStatus.Idle:
                            status = BatteryStatus.NotCharging;
                            break;
                        case Windows.System.Power.BatteryStatus.NotPresent:
                            status = BatteryStatus.Unknown;
                            break;
                    }
                }
                return status;
            }
        }

        public PowerSource PowerSource
        {
            get
            {
                if (status == BatteryStatus.Full || status == BatteryStatus.Charging)
                {
                    return PowerSource.Ac;
                }
                return PowerSource.Battery;
            }
        }
    }
}
```

Le `[assembly]` attribut au-dessus de la déclaration d’espace de noms inscrit la classe en tant qu’implémentation de la `IBattery` interface, ce qui signifie que `DependencyService.Get<IBattery>` peut être utilisé dans le code partagé pour créer une instance de celui-ci.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Mise en œuvre dans le Code partagé

Maintenant que l’interface a été implémenté pour chaque plateforme, l’application partagée permettre être écrites pour tirer parti de celui-ci. L’application se compose d’une page avec un bouton qui une fois tapées mises à jour son texte avec l’état actuel de la batterie. Elle utilise le `DependencyService` pour obtenir une instance de la `IBattery` interface. Lors de l’exécution, cette instance correspond à l’implémentation spécifique à la plateforme qui a un accès complet pour le Kit de développement natif.

```csharp
public MainPage ()
{
    var button = new Button {
        Text = "Click for battery info",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    button.Clicked += (sender, e) => {
        var bat = DependencyService.Get<IBattery>();

        switch (bat.PowerSource){
          case PowerSource.Battery:
            button.Text = "Battery - ";
            break;
          case PowerSource.Ac:
            button.Text = "AC - ";
            break;
          case PowerSource.Usb:
            button.Text = "USB - ";
            break;
          case PowerSource.Wireless:
            button.Text = "Wireless - ";
            break;
          case PowerSource.Other:
          default:
            button.Text = "Other - ";
            break;
        }
        switch (bat.Status){
          case BatteryStatus.Charging:
            button.Text += "Charging";
            break;
          case BatteryStatus.Discharging:
            button.Text += "Discharging";
            break;
          case BatteryStatus.NotCharging:
            button.Text += "Not Charging";
            break;
          case BatteryStatus.Full:
            button.Text += "Full";
            break;
          case BatteryStatus.Unknown:
          default:
            button.Text += "Unknown";
            break;
        }
    };
    Content = button;
}
```

Cette application en cours d’exécution sur les plateformes Windows, Android ou iOS et en appuyant sur le bouton entraînera le texte du bouton Mettre à jour pour refléter l’état actuel de la puissance de l’appareil.

![](battery-info-images/battery.png "Exemple d’état de batterie")


## <a name="related-links"></a>Liens associés

- [DependencyService (sample)](https://developer.xamarin.com/samples/DependencyService)
- [À l’aide de DependencyService (exemple)](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Exemples Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
