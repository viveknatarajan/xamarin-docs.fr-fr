---
title: Vérification de l’état de la batterie
description: Cet article explique comment utiliser la classe Xamarin.Forms DependencyService pour accéder aux informations de batterie en mode natif pour chaque plateforme.
ms.prod: xamarin
ms.assetid: CF1C5A73-84ED-407D-BDC5-EB1D83D2D3DB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: cbb4a01ac2c6d933fe40a0b3c2571d1fe3ce75c0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998392"
---
# <a name="checking-battery-status"></a>Vérification de l’état de la batterie

Cet article présente de la création d’une application qui vérifie l’état de la batterie. Cet article est basé sur le plug-in de la batterie par James Montemagno. Pour plus d’informations, consultez le [référentiel GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery).

Étant donné que Xamarin.Forms n’inclut pas la fonctionnalité permettant de vérifier l’état actuel de la batterie, cette application devra utiliser [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) pour tirer parti des API natives.  Cet article décrit les étapes suivantes pour l’utilisation de `DependencyService`:

- **[Création de l’Interface](#Creating_the_Interface)**  &ndash; comprendre comment l’interface est créée dans le code partagé.
- **[iOS implémentation](#iOS_Implementation)**  &ndash; Apprenez à implémenter l’interface en code natif pour iOS.
- **[Implémentation Android](#Android_Implementation)**  &ndash; Apprenez à implémenter l’interface en code natif pour Android.
- **[Implémentation de la plateforme Windows universelle](#UWPImplementation)**  &ndash; Apprenez à implémenter l’interface en code natif pour la plateforme Windows universelle (UWP).
- **[Mise en œuvre dans le Code partagé](#Implementing_in_Shared_Code)**  &ndash; Apprenez à utiliser `DependencyService` pour appeler l’implémentation native à partir de code partagé.

Lorsque terminée, l’application à l’aide `DependencyService` aura la structure suivante :

![](battery-info-images/battery-diagram.png "Structure de l’Application DependencyService")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Création de l'interface utilisateur

Tout d’abord, créez une interface dans le code partagé qui exprime la fonctionnalité souhaitée. Dans le cas d’une batterie, la vérification d’application, les informations pertinentes sont le pourcentage de batterie restante, si l’appareil est ou non de chargement, et comment l’appareil est alimenté :

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

Codage par rapport à cette interface dans le code partagé permettent à l’application de Xamarin.Forms accéder à l’API de gestion de l’alimentation sur chaque plateforme.

> [!NOTE]
> Classes implémentant l’Interface doivent avoir un constructeur sans paramètre pour travailler avec le `DependencyService`. Les constructeurs ne peuvent pas être définis par les interfaces.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implémentation d’iOS

Le `IBattery` interface doit être implémentée dans chaque projet d’application de plateforme spécifique. L’implémentation d’iOS utilisera natif [ `UIDevice` ](https://developer.xamarin.com/api/type/UIKit.UIDevice/) API pour accéder aux informations de batterie. Notez que la classe suivante a un constructeur sans paramètre afin que le `DependencyService` peut créer des instances :

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

Enfin, ajoutez ce `[assembly]` attribut au-dessus de la classe (et en dehors des espaces de noms qui ont été définies), y compris tout `using` instructions :

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

Cet attribut inscrit la classe en tant qu’implémentation de la `IBattery` interface, ce qui signifie que `DependencyService.Get<IBattery>` peut être utilisé dans le code partagé pour créer une instance de celui-ci :

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implémentation Android

L’implémentation Android utilise le [ `Android.OS.BatteryManager` ](https://developer.xamarin.com/api/type/Android.OS.BatteryManager/) API. Cette implémentation est plus complexe que la version iOS, nécessitant des vérifications pour gérer le manque d’autorisations de la batterie :

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

Ajoutez ce `[assembly]` attribut au-dessus de la classe (et en dehors des espaces de noms qui ont été définies), y compris tout `using` instructions :

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

Cet attribut inscrit la classe en tant qu’implémentation de la `IBattery` interface, ce qui signifie que `DependencyService.Get<IBattery>` peut être utilisé dans le code partagé peut créer une instance de celle-ci.

<a name="UWPImplementation" />

## <a name="universal-windows-platform-implementation"></a>Implémentation de plateforme Windows universelle

L’implémentation UWP utilise la `Windows.Devices.Power` API pour obtenir des informations d’état de la batterie :

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

Maintenant que l’interface a été implémenté pour chaque plateforme, l’application partagée peut être écrite pour tirer parti de celui-ci. L’application se compose d’une page avec un bouton qu’au moment où l’appui sur les mises à jour son texte avec l’état actuel de la batterie. Il utilise le `DependencyService` pour obtenir une instance de la `IBattery` interface. Lors de l’exécution, cette instance sera l’implémentation spécifique à la plateforme qui a un accès complet pour le Kit de développement natif.

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

Exécutez cette application sur iOS, Android, ou UWP et en appuyant sur le bouton entraîne la mise à jour pour refléter l’état actuel de la puissance de l’appareil comme texte du bouton.

![](battery-info-images/battery.png "Exemple d’état de la batterie")


## <a name="related-links"></a>Liens associés

- [DependencyService (exemple)](https://developer.xamarin.com/samples/DependencyService)
- [À l’aide de DependencyService (exemple)](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Exemples Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
