<properties
    pageTitle="Azure funkcija timer okidača | Microsoft Azure"
    description="Objašnjenje kako pomoću okidača mjerača vremena u funkcijama Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funkcije, Funkcije, događaja obrada dinamički računalnim serverless arhitekture"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-timer-trigger"></a>Azure okidača funkcija timer

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

U ovom se članku objašnjava kako konfigurirati okidača mjerača vremena u funkcijama Azure. Timer pokreće funkcije poziva na temelju raspored, jedanput ili ponavljanje.  

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-timer-trigger"></a>Function.JSON za pokretanje mjerača vremena

Datoteka *function.json* sadrži izraz za raspored. Na primjer, sljedeći raspored pokreće funkciju svake minute:

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Pokretanje mjerača vremena rukuje skaliranje iz više instanci automatski: će imati samo jednu instancu funkcija određeni timer preko sve instance.

## <a name="format-of-schedule-expression"></a>Oblik rasporeda izraza

Izraz raspored je [CRON izraz](http://en.wikipedia.org/wiki/Cron#CRON_expression) koji uključuje 6 polja: `{second} {minute} {hour} {day} {month} {day of the week}`. 

Imajte na umu mnoge izraza cron pronađete online ispustite polje {drugi} da ako kopirate iz jednog od tih morat ćete prilagoditi dodatna polja. 

Evo nekih raspored izraz primjera:

Da biste pokrenuli svakih 5 minuta:

```json
"schedule": "0 */5 * * * *"
```

Da biste pokrenuli jednom pri vrhu svaki sat:

```json
"schedule": "0 0 * * * *",
```

Da biste pokrenuli svaka dva sata:

```json
"schedule": "0 0 */2 * * *",
```

Da biste pokrenuli jednom svaki sat od 9 AM za 5 Poslijepodne:

```json
"schedule": "0 0 9-17 * * *",
```

Da biste aktiviraju 9:30 se svakodnevno:

```json
"schedule": "0 30 9 * * *",
```

Da biste aktiviraju 9:30 se svakodnevni:

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="timer-trigger-c-code-example"></a>Primjer koda okidača C# mjerača vremena

U ovom primjeru koda za C# piše jedan zapisnik svaki put kada se pokrene funkciju.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
