<properties
    pageTitle="Praćenje aplikacija u aplikacije servisa za Azure"
    description="Saznajte kako praćenje aplikacija u aplikacije servisa za Azure pomoću portala za Azure."
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="byvinyal"/>

# <a name="how-to-monitor-apps-in-azure-app-service"></a>Kako: praćenje aplikacija u aplikacije servisa za Azure

[Aplikacije servisa za](http://go.microsoft.com/fwlink/?LinkId=529714) nudi ugrađeni nadzora funkcije [Azure Portal](https://portal.azure.com).
To obuhvaća mogućnost da biste pregledali **kvotama** i **metriku** za aplikaciju, kao i aplikacije servisa za tarifu, postavljanje **upozorenja** i čak i **Skaliranje** automatski na temelju tih metriku.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Kvota za razumijevanje i metrike

### <a name="quotas"></a>Kvota

Aplikacije koje se nalaze u aplikacije servisa za podliježe određene *ograničenja* resursa koji se mogu koristiti. Ograničenja definira **aplikacije servisa za planiranje** pridružene aplikaciju.

Ako aplikacija nalazi se u **slobodan** ili **zajednički se koristi** plan, ograničenja resursa aplikaciju, poslužite se definiraju tako da **kvote**.

Ako aplikacija je smještena u **Osnovni**, **Standardno** ili **Premium** plan, a zatim ograničenja na resurse koji se mogu koristiti postavlja **veličinu** (male, Srednja, Large) i **instancu count** (1, 2, 3,...) **aplikacije servisa za planiranje**.

**Kvota** za **slobodno** ili **zajednički se koristi** aplikacija su:

* **CPU(short)**
   * Iznos procesora dopuštena za ovu aplikaciju u interval 3 minute. U ovom kvote ponovno postavlja svake 3 minute.
* **CPU(Day)**
   * Ukupni iznos procesora dopuštena za ovu aplikaciju u dana. U ovom kvote ponovno postavlja svaka 24 sata od ponoći UTC-a.
* **Memorije**
   * Ukupni iznos memorije dopuštena za ovu aplikaciju.
* **Propusnosti**
   * Ukupni iznos odlazne propusnosti dopuštena za ovu aplikaciju u dana.
   U ovom kvote ponovno postavlja svaka 24 sata od ponoći UTC-a.
* **Datotečnom sustavu**
   * Ukupni iznos dopušten prostor za pohranu.

Samo kvote odgovarajuće aplikacije koje se nalaze na **Osnovni**, **standardne** i tarife **Premium** je **datotečnom sustavu**.

Dodatne informacije o određenim kvote, ograničenja i značajke koje su dostupne u drugu aplikaciju servisa SKU-ove nalazi se ovdje: [Ograničenja servisa pretplatu za Azure](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Provođenje kvote

Ako aplikacija u njegova korištenja premašuje **procesora (short)**, **Procesor (dan)**ili ograničenja **propusnosti** zatim aplikacije prekinut će se sve dok ponovno postavlja kvote. Za to vrijeme svih dolaznih zahtjeva rezultirat će **HTTP 403**.
![][http403]

Ako se prekorači ograničenja **memorije** aplikacije, aplikacija će se ponovno rada.

Ako je premašena kvota za **datotečnom sustavu** , zatim bilo napisati operacija neće uspjeti, a to uključuje zapisivanje zapisnicima.

Kvota možete povećati ili ukloniti iz aplikacije programa nadogradnje aplikacije servisa za plan.

### <a name="metrics"></a>Mjerenja

**Metriku** nalaze informacije o aplikaciji ili ponašanje aplikacije servisa za planiranje.

Za **aplikaciju**, metriku dostupne su:

* **Prosječno vrijeme odaziva**
   * Prosječno vrijeme za aplikaciju da bi služio zahtjeve u milisekundama.
* **Prosječna memorija radni skup**
   * Prosječni iznos memorije u MiBs koristi aplikaciju.
* **Vrijeme procesora**
   * Iznos procesora u sekundama troše aplikaciju. Dodatne informacije o tome metrike, pogledajte: [procesora vrijeme Dodavanje veze za vanjskih procesora postotka](#cpu-time-vs-cpu-percentage)
* **Podaci u**
   * Iznos dolazne propusnosti koju koriste aplikacije u MiBs.
* **Podataka**
   * Iznos odlazne propusnosti koju koriste aplikaciju u MiBs.
* **HTTP 2xx**
   * Broj zahtjeva rezultira http kod stanja > = 200, ali < 300.
* **HTTP 3xx**
   * Broj zahtjeva rezultira http kod stanja > = 300, ali < 400.
* **HTTP 401**
   * Broj zahtjeva za rezultira HTTP 401 Šifra stanja.
* **HTTP 403**
   * Broj zahtjeva za rezultira Šifra stanja HTTP 403.
* **HTTP 404**
   * Broj zahtjeva za rezultira Šifra stanja HTTP 404.
* **HTTP 406**
   * Broj zahtjeva za rezultira HTTP 406 Šifra stanja.
* **HTTP 4xx**
   * Broj zahtjeva rezultira http kod stanja > = 400, ali < 500.
* **HTTP poslužitelj pogreške**
   * Broj zahtjeva rezultira http kod stanja > = 500, ali < 600.
* **Radni skup memorije**
   * Trenutni količinu memorije koristi za aplikacije u MiBs.
* **Zahtjevi za**
   * Ukupan broj zahtjeva bez obzira na njihov dobivene HTTP kod stanja.

Za **aplikacije servisa za planiranje**, metriku dostupne su:

>[AZURE.NOTE] Aplikacije servisa za planiranje metriku su dostupne samo za planove **Osnovni**, **standardne** i **Premium** SKU-om.

* **Postotak CPU-a**
   * Prosječna procesora koristi preko sve instance plan.
* **Postotak memorije**
   * Prosječna memorije koristi preko sve instance plan.
* **Podaci u**
   * Prosječna dolazne propusnost preko sve instance plan.
* **Podataka**
   * Prosjek odlazne propusnost preko svih pojavljivanja plan.
* **Duljina reda čekanja na disku**
   * Prosječan broj čitanje i pisanje zahtjevi za koje su u redu čekanja za pohranu. Duljina reda čekanja za visoke disk je oznaka aplikaciju koja možda usporava zbog viškom disk/i.
* **Duljina reda čekanja http**
   * Prosječan broj zahtjeva HTTP koja je imala trebate sjesti u redu čekanja prije nego što se ispunjena. U velikom ili rastuće HTTP Duljina reda čekanja je simptom plan pod velikim opterećenjem.

### <a name="cpu-time-vs-cpu-percentage"></a>Postotak za dodavanje veze za vanjskih procesora procesora vrijeme
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Postoje 2 metriku koja odražava procesora. **Vrijeme procesora** i **postotak Procesor**

**Vrijeme procesora** je korisno za aplikacije smješten u tarife **slobodno** ili **zajednički se koristi** Budući da jedan od njihovih kvote definiran u minutama procesora koristi aplikaciju.

**Postotak procesora** s druge strane je korisno za aplikacije smješten u **Osnovni**, **standardne** i **premium** tarife jer se skalirana izgleda i ovu metriku je dobro oznaka cjelokupan korištenje preko sve instance.

##<a name="metrics-granularity-and-retention-policy"></a>Metriku preciznosti i pravilnika o zadržavanju

Metriku za računala i aplikaciju tarifa za servis su prijavljeni i pridružuje servis sa sljedećim granularnosti i pravilnika o zadržavanju:

 * **Minute** preciznosti metriku se zadržavaju **48** sati
 * **Sat** preciznosti metriku se zadržavaju **30** dana
 * **Dan** preciznosti metriku se zadržavaju za **90 dana**

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Nadzor kvotama i mjernih podataka na portalu za Azure.

Možete pregledati status različite **kvotama** i **metriku** utjecaja programa [Azure Portal](https://portal.azure.com).

![][quotas]
**Kvota** pronaći ćete u odjeljku postavke >**kvote**. Na UX omogućuje vam da biste pregledali: (1) kvote naziv, (2) njegov interval za ponovno postavljanje, (3) trenutno ograničenje i (4) trenutnu vrijednost.

![][metrics]
**Metriku** može biti pristup izravno iz plohu resursa. Možete prilagoditi i na grafikon: (1) **kliknite** na, a (2) odaberite **Uređivanje grafikona**.
Na tom mjestu možete promijeniti (3) **vremenski raspon**, (4) **vrstu grafikona**i (5) **metriku** da bi se prikazao.  

Dodatne informacije o metriku ovdje: [metriku servisa Monitor](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Upozorenja i automatsko skaliranje
Metriku za tarifu aplikacije ili servisa aplikacija se može priključiti do upozorenja, da biste saznali više o tome potražite u članku [primanje upozorenja](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Aplikacije servisa za aplikacije smješten u osnovni, standardno ili premium aplikacije servisa za tarife podržava **Automatsko skaliranje**. Time konfiguriranje pravila koja praćenje metriku aplikacije servisa za planiranje i možete povećati ili smanjiti broj instanci pruža dodatne resurse prema potrebi ili spremanja novac kada je aplikacija prekoračenja dodjele resursa. Dodatne informacije o automatsko skaliranje ovdje: [kako mjerilo](../monitoring-and-diagnostics/insights-how-to-scale.md) , a Saznajte [najbolje prakse za Azure Monitor autoscaling](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="whats-changed"></a>Što se promijenilo
* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
