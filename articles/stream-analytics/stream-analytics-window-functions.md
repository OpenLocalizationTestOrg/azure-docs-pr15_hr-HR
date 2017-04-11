<properties
    pageTitle="Uvod u prozor analize strujanje funkcije | Microsoft Azure"
    description="Saznajte više o tri funkcije prozora u strujanje analize (tumbling, skakutanje, animiranog)."
    keywords="tumbling prozoru animiranog prozoru skakutanje prozora"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="introduction-to-stream-analytics-window-functions"></a>Uvod u funkcije strujanje analize prozora

U mnogo stvarnom vremenu strujanje scenariji, je potrebne za izvođenje operacije samo na podataka koji se nalaze u vremenski windows. Ugrađena podrška za prikaz u prozorima funkcije je ključna značajka Azure strujanje analize koja se pomiče na igle po za razvojne inženjere produktivnost u složene strujanje obrada poslova za izradu. Analitički strujanje omogućuje razvojnim inženjerima [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) i [**Sliding**](https://msdn.microsoft.com/library/dn835051.aspx) windows za izvođenje vremenski operacija na strujanja podataka. To vrijedi sudjelovanje sve operacije [prozor](https://msdn.microsoft.com/library/dn835019.aspx) izlaz je rezultata na **Kraj** prozora. Izlaz iz prozora bit će jedan događaj na temelju koristi funkciju zbrajanja. Događaj sadržavat će vremenska oznaka kraja prozora, a sve funkcije prozor definiranih fiksne duljine. Na kraju je imati na umu da se sve funkcije prozor treba koristiti u uvjet [**GROUP**](https://msdn.microsoft.com/library/dn835023.aspx) BY.

![Prozor analize strujanje funkcije koncepti](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Prozor tumbling

Tumbling prozoru funkcije koriste se za fazi tijeku podataka u segmenata različita vremena i izvršavanje funkcija, kao što su primjeru u nastavku. Ključni differentiators prozora Tumbling su da se ponavlja, preklapaju i događaja ne pripadaju više tumbling prozora.

![Funkcije analize prozora za strujanje tumbling Uvod](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Prozor skakutanje

Skakutanje prozor funkcije skoka unaprijed u vremenu tako da fiksnog razdoblja. Možda ćete lako smatrati ih Tumbling prozori koji se mogu preklapati, tako da se događaji pripadati više Hopping skup rezultata prozora. Da bi prozor Hopping jednak je Tumbling prozor nešto bi jednostavno odredite veličinu skoka da je isti kao veličinu prozora. 

![Prozor analize strujanje funkcije skakutanje Uvod](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Prozor klizača

Klizača funkcije prozor, za razliku od Tumbling ili Hopping prozora daju do izlazne **samo** kada se pojavi događaj. Svaki prozor će imati barem jedan događaj i prozora neprestano premješta prema naprijed po € (epsilon). Kao što je Windows skakutanje događaje pripadati više animiranog prozora.

![Prozor analize strujanje funkcije klizača Uvod](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Pristup pomoći za funkcije prozora

Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
