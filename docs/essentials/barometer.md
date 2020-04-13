---
title: 'Xamarin.Essentials: Barometer'
description: A classe Barometer no Xamarin.Essentials permite que você monitore o sensor do barômetro do dispositivo. Esse sensor mede a pressão.
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 5a496fc06732be1cf104cfbaffa8ff4b68c8f564
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/13/2020
ms.locfileid: "69976433"
---
# <a name="xamarinessentials-barometer"></a>Xamarin.Essentials: Barometer

A classe **Barometer** permite que você monitore o sensor do barômetro do dispositivo. Esse sensor mede a pressão.

## <a name="get-started"></a>Introdução

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-barometer"></a>Uso do Barometer

Adicione uma referência ao Xamarin.Essentials na classe:

```csharp
using Xamarin.Essentials;
```

A funcionalidade Barometer funciona chamando os métodos `Start` e `Stop` para escutar as alterações nas leituras de pressão do barômetro em hectopascais. Todas as alterações são enviadas de volta por meio do evento `ReadingChanged`. Veja um exemplo de uso:

```csharp

public class BarometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public BarometerTest()
    {
        // Register for reading changes.
        Barometer.ReadingChanged += Barometer_ReadingChanged;
    }

    void Barometer_ReadingChanged(object sender, BarometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Pressure
        Console.WriteLine($"Reading: Pressure: {data.PressureInHectopascals} hectopascals");
    }

    public void ToggleBarometer()
    {
        try
        {
            if (Barometer.IsMonitoring)
              Barometer.Stop();
            else
              Barometer.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>Particularidades de implementação da plataforma

# <a name="android"></a>[Android](#tab/android)

Sem detalhes da implementação específica da plataforma.

# <a name="ios"></a>[iOS](#tab/ios)

Essa API usa [CMAltimeter](https://developer.apple.com/documentation/coremotion/cmaltimeter#//apple_ref/occ/cl/CMAltimeter) para monitorar as alterações de pressão. Esse recurso de hardware foi adicionado ao iPhone 6 e dispositivos mais recentes. Uma `FeatureNotSupportedException` será gerada em dispositivos que não suportam o altímetro.

`SensorSpeed` não é usado, pois não é compatível com o iOS.

# <a name="uwp"></a>[UWP](#tab/uwp)

Sem detalhes da implementação específica da plataforma.

-----

## <a name="api"></a>API

- [Código-fonte do Barometer](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Barometer)
- [Documentação da API do Barometer](xref:Xamarin.Essentials.Barometer)
