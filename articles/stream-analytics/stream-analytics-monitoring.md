<properties 
    pageTitle="Razumijevanje strujanje analize posao nadzor | Microsoft Azure" 
    description="Objašnjenje strujanje analize posao nadzora" 
    keywords="monitor upita"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Razumijevanje strujanje analize posao nadzor i kako nadzirati upita

## <a name="introduction-the-monitor-page"></a>Uvod: Monitor stranice

Portal za upravljanje Azure i Azure Portal ponuditi metriku ključnog učinka koji se mogu koristiti i praćenje otkloniti poteškoće s performansama vašeg upita i posla. 

Na portalu za upravljanje Azure kliknite na kartici **Monitor** izvodi analize toka posla da biste vidjeli ove metriku. Postoji odgodu na 1 minutu u metriku performanse prikazuju se na stranici Monitor.  

  ![Nadzor posao nadzorne ploče](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

Na portalu za Azure pronađite posao strujanje analize zanima prikazuju metriku i prikaz odjeljku **nadzor** .  

  ![Portal za nadzor Azure posao nadzorne ploče](./media/stream-analytics-monitoring/06-stream-analytics-monitoring.png)  

Prvi put posao strujanje analize se stvara u regiji, morat ćete konfigurirati Dijagnostika za to područje. Da biste to učinili, kliknite bilo gdje u odjeljku **nadzor** i plohu **Dijagnostika** pojavit će se. Ovdje možete omogućiti Dijagnostika i navedite račun za pohranu za praćenje podataka.  

  ![Konfiguriranje Portal Azure upita dijagnostiku](./media/stream-analytics-monitoring/07-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Dostupno za strujanje analize mjerenja


| Mjerenja | Definicija |
|--------|-------------|
| Upotreba SU % | Upotreba jedinice strujanje dodijeljene posao na kartici skaliranje posla. Trebali biste ovog pokazatelja dosegne 80% ili iznad je visoke vjerojatnost da obrada događaja možda odgođeno ili prekinuli upućivanje tijek. |
| Unos događaja | Količinu podataka koji se primio poslu strujanje analize u broj događaja. To se može koristiti za provjeru valjanosti koje se šalju događaja na ulazni izvor. |
| Izlaz događaja | Količinu podataka koji se šalju posla strujanje analize ciljnoj Izlaz u broj događaja. |
| Procedure događaja | Broj događaja koji ste primili iz narudžbe koji su prekine ili dobili usklađena pečata na temelju pravila za redoslijed događaj. To se neće utjecati tako da konfiguracije postavku prozor odstupanje redoslijed vrsta izlaz. |
| Pogreške pri pretvaranju podataka | Broj pogreške pri pretvaranju podataka nastale analize toka posla. |
| Pogreške prilikom izvođenja | Broj pogreške koje će se dogoditi prilikom izvođenja analize toka posla. |
| Najnovije unos događaja | Broj događaja stiže kašnjenja s izvorišnog web-mjesta koja ili ispušteno ili njihovim vremenske oznake prilagođen, ovisno o konfiguraciji pravila za redoslijed događaj postavke kašnjenja kašnjenja odstupanje prozora. |

## <a name="customizing-monitoring-in-the-azure-management-portal"></a>Prilagođavanje praćenja na portalu za upravljanje Azure ##

Do 6 metriku može se prikazati na grafikonu.

Za prebacivanje između relativnih vrijednosti (Završna vrijednost samo za svaki metriku) i apsolutne vrijednosti (prikaza osi Y), odaberite zavisne ili apsolutna pri vrhu grafikon.

  ![Upit monitor relativni apsolutne](./media/stream-analytics-monitoring/02-stream-analytics-monitoring.png)  

Metriku moguće je prikazati u grafikonu Monitor zbrajanja od jednog sata, 12 sati, 24 sata ili 7 dana.

Da biste promijenili vremenski raspon grafikona prikazuje metriku odaberite 1 sat, 24 sata ili 7 dana pri vrhu grafikona.

  ![Upit monitor vremenskog mjerila](./media/stream-analytics-monitoring/03-stream-analytics-monitoring.png)  

Možete postaviti pravila koja se može vas obavijesti putem e-pošte u slučaju da posao presjek definirani praga. 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Prilagodba nadzor na portalu za Azure ##

Možete prilagoditi vrstu grafikona, metrike koji se prikazuje, i vremenski raspon u postavkama uređivanje grafikona. Detalje potražite u članku [kako prilagoditi nadzor](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Azure portala upita Monitor vremenskog mjerila](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  

## <a name="job-status"></a>Status zadatka

Stanje analize strujanje poslova moguće je prikazati u Azure klasični portala za koje vam se prikazuje popis zadataka. Na popisu zadataka možete vidjeti klikom na ikonu strujanje analize na portalu klasični Azure.

| Status | Definicija |
|--------|------------|
| Stvorili | Zadatak je stvoren, no nije pokrenut. |
| Pokretanje | Korisnik nakon klika na izborniku Start posla i pokreće posao |
| Pokretanje | Zadatak koji se dodjeljuje obrada unos ili one koje čekaju obradu unos. Ako posao pokazuje stanja za pokretanje bez proizvodnje Izlaz, vjerojatno je da termin obradu podataka je velika i logika upita je složene. Drugog razloga možda je da trenutno ne postoji podataka koja se šalje na posao. |
| Zaustavljanje | Korisnik nakon klika na Prestani posla, a zaustavlja se posao. |
| Zaustavi | Zadatak je zaustavljena. |
| Smanjena | Ovaj status ukazuje da posao strujanje Analytics došlo je do pogreške tranzitne (za ex. Unos/izlaz pogreške, obradu pogrešaka, pogreške pri pretvaranju itd.). Posao i dalje instaliran, ali je mnogo pogreške generira. Ovaj zadatak treba obratiti pozornost klijenta i klijenta možete vidjeti u zapisnicima operacije pogreške. |
| Nije uspjela | To označava da posao nije uspjela zbog pogrešaka i obrada zaustavljena. Klijent mora pogleda zapisnike operacije da bi se pogreške za ispravljanje pogrešaka. |
| Brisanje | To označava da je brisanja zadatka. |

## <a name="diagnosis"></a>Dijagnostika

Na portalu za upravljanje Azure na nadzornoj ploči posla nudi informacije o tamo gdje vam je potreban za Dijagnostika, odnosno unosa proizvodi i/ili operacije prijavu. Klikom na vezu da biste prešli na odgovarajuće mjesto možete pogledati na Dijagnostika.

  ![Upit monitor pogreške](./media/stream-analytics-monitoring/04-stream-analytics-monitoring.png)  

Klikom na ulazni i izlazni resursa pruža podrobne dijagnostičke informacije. To je osvježiti najnovijim informacijama Dijagnostika dok se izvodi posao.

  ![Dijagnostika upita](./media/stream-analytics-monitoring/05-stream-analytics-monitoring.png)  

## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
