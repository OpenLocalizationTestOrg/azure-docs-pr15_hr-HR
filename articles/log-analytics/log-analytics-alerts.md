<properties 
   pageTitle="Upozorenja u zapisnik analize | Microsoft Azure"
   description="Upozorenja u zapisnik analize prepoznavanje važne informacije u vašem spremište OMS i možete doći obavijesti o problemima ili pozivanje akcije da biste ih ispravili.  U ovom se članku opisuje kako stvoriti pravilo upozorenja i detalje o različite akcije ih može potrajati."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="bwren" />

# <a name="alerts-in-log-analytics"></a>Upozorenja u zapisnik Analytics

Upozorenja u zapisnik analize odredite važne informacije u vašem OMS spremište.  Upozorenja pravila automatski pokrenuti zapisnika pretraživanja prema raspored i stvaranje upozorenja zapis ako rezultate odgovaraju određenom kriteriju.  Pravilo može pokrenuti jednu ili više akcija doći obavijesti o Upozorenje programa ili aktiviranja drugi proces automatski.   

![Prijava analitiku upozorenja](media/log-analytics-alerts/overview.png)

## <a name="creating-an-alert-rule"></a>Stvaranje pravila za upozorenja
Da biste stvorili pravilo upozorenja, pokrenite stvaranjem zapisnika pretraživanja za zapise koji treba pozivanje upozorenje.  Gumb **upozorenje** tada će biti dostupna tako da možete stvoriti i konfigurirati upozorenja pravilo.

1.  Na stranici pregled OMS kliknite **Zapisnika pretraživanja**.
2.  Stvorite novi upit za pretraživanje zapisnika ili odaberite spremljenu zapisnika pretraživanja. 
3.  Kliknite **upozorenje** pri vrhu stranice da biste otvorili zaslon **Dodavanje upozorenja pravila** .
4. Pogledajte tablice ispod detalje na izborniku mogućnosti da biste konfigurirali upozorenje.
5. Kada unesete prozoru vremena za upozorenja pravilo, prikazat će se broj postojeće zapise koji zadovoljavaju kriterij pretraživanja za taj doba dana.  To može pomoći Odredite učestalost koji će vam broj rezultata koji ste i očekivali.
4.  Kliknite **Spremi** da biste dovršili pravilo za upozorenja.  Će započeti odmah pokrenuti.


![Dodavanje pravila za upozorenja](media/log-analytics-alerts/add-alert-rule.png)

| Svojstvo | Opis |
|:--|:--|
| **Informacija o upozorenjima** | |
| Ime |  Jedinstveni naziv za prepoznavanje upozorenja pravilo. |
| Težinu | Težinu upozorenja koje je stvorio ovo pravilo. |
| Upit za pretraživanje | Odaberite **Koristi trenutni upit za pretraživanje** da biste koristili trenutni upit ili s popisa odaberite postojeći spremljeno pretraživanje.  Sintaksa upita prikazuje se u tekstni okvir gdje je možete izmijeniti prema potrebi.  |
| Doba dana | Određuje vremenski raspon za upit.  Upit vraća samo zapise koje su stvorene u ovaj raspon trenutnog vremena.  To može biti bilo koja vrijednost između 5 minuta i 24 sata.  Mora biti veći od ili jednako učestalost upozorenja.  <br><br> Na primjer, ako prozor vrijeme postavljeno na 60 minuta, a upit se pokreće pri 1:15 poslije Podne, samo zapise između 12:15 poslije Podne i 1:15 poslije Podne će se vratiti. |
| **Raspored** |     
| Prag | Kriteriji za kada stvoriti upozorenje.  Ako broj zapisa je vratio upit udovoljava ovaj kriterij stvara se upozorenje. |
| Učestalost upozorenja | Određuje koliko često izvoditi upit.  Može biti bilo koja vrijednost između 5 minuta i 24 sata.  Mora biti jednaka nuli ili manja od prozora vremena. |
| Izostavi upozorenja | Kada uključite potiskivanje upozorenja pravila za definirani vremensko razdoblje nakon stvaranja Novo upozorenje onemogućene su akcije pravila.  Pravilo se izvodi i će stvoriti upozorenje zapisa ako je ispunjen kriterij.  To je da biste omogućili vrijeme da biste ispravili problem bez pokretanja duplicirane akcije. |
| **Akcija** | |
| Obavijesti e-pošte | Odredite **da** ako želite biti poslana kada se pokrene upozorenja e-pošte. |
| Predmet    | Predmet u e-pošte.  Ne možete izmijeniti u tijelu poruke. |
| Primatelji | Adrese e-pošte primateljima.  Ako navedete više od jedne adrese, adrese razdvojite točkom sa zarezom (;). |
| Webhook | Odredite **da** ako želite pozvati s webhook kada se pokrene upozorenje. |
| Webhook URL-a | URL za webhook. |
| Sadrže prilagođene JSON tereta | Odaberite ovu mogućnost ako želite zamijeniti zadani opseg prilagođene tereta. |
| Unesite svoje prilagođene JSON tereta | Prilagođeni opseg na webhook.  Pojedinosti potražite u članku. |
| Runbook | Odredite **da** ako želite započeti programa automatizacije Azure runbook kada se pokrene upozorenje. |
| Odaberite na runbook | Odaberite runbook da biste započeli s runbooks na računu za automatizaciju konfiguriran u rješenje automatizaciju. |
| Mada | Odaberite **Azure** da biste pokrenuli u runbook u Azure oblaka.  Odaberite **Hibridnog tempiranja** pokrenuti na runbook [Hibridnog Runbook suradnika](..\automation\automation-hybrid-runbook-worker.md) u lokalnom okruženju. |


## <a name="manage-alert-rules"></a>Upravljanje pravilima upozorenja
Na izborniku **upozorenja** u zapisnik analize **Postavke**možete dohvatiti popis svih upozorenja pravila.  

![Upravljanje upozorenjima](./media/log-analytics-alerts/configure.png)

1. Na konzoli OMS odaberite pločicu **Postavke** .
2. Odaberite **upozorenja**.

Na raspolaganju vam više akcija iz ovog prikaza.

- Onemogućite pravilo tako da odaberete **isključivanje** pokraj njega.
- Uređivanje upozorenja pravilo tako da kliknete ikonu olovke pokraj njega.
- Uklanjanje upozorenja pravilo tako da kliknete ikonu **X** pokraj njega. 


## <a name="setting-time-windows"></a>Postavljanje vremena sustava windows 

### <a name="event-alerts"></a>Primanje obavijesti o događajima

Događaji obuhvaćaju izvorima podataka kao što su zapisnika događaja sustava Windows, Syslog, i prilagođena zapisnike.  Možda želite stvoriti upozorenje dobiva stvaranja određene pogreške događaj ili kada na više pogrešaka događaje stvaraju se u prozoru za određeno vrijeme.

Upozorenje na jedan događaj, postavite broj rezultata da biste veći od 0 i učestalost i termin na 5 minuta.  Koje će pokrenuti upit svakih 5 minuta i provjerite pojavu jedan događaj koji je stvoren od posljednjeg pokretanja upita.  Dulji učestalost možda odgađanje vremena između događaja koji se prikupljaju i upozorenje stvoren.

Neke aplikacije mogu prijaviti Povremeni pogreške koje nisu nužno Potenciranje upozorenja.  Aplikacije, na primjer, možda ponovite postupak koji stvara događaj pogreške i uspjeti kada se sljedeći put.  U ovom slučaju, možda ne želite stvoriti upozorenje osim ako više događaje stvaraju se u prozoru za određeno vrijeme.  

U nekim slučajevima, preporučujemo vam da stvorite upozorenje programa za događaj.  Na primjer, procesa možda Zapiši običnog da biste naznačili ispravno funkcionira.  Ako ne zapisuje jednu od ovih događaja u prozoru za određeno vrijeme, zatim upozorenja mora biti stvoren.  U ovom slučaju ćete postaviti praga na *manji od 1*.

### <a name="performance-alerts"></a>Performanse upozorenja

[Performanse podaci](log-analytics-data-sources-performance-counters.md) se pohranjuju kao zapise u spremištu OMS slično događaja.  Vrijednost svakog zapisa je prosjek mjeri putem prethodne 30 minuta.  Ako želite upozorenje kada performanse brojač premaši određeni prag, zatim taj prag želite uvrstiti u upit.

Na primjer, ako ste željeli upozorenje kada se pokrene procesor više od 90% za 30 minuta, koristite upit kao što su *Vrsta = mjerača performansi ObjectName = CounterName procesor = "% procesor vrijeme" CounterValue > 90* i praga za upozorenja pravila *veći od 0*.  

 Budući da [performanse zapisi](log-analytics-data-sources-performance-counters.md) se pridružuje svakih 30 minuta bez obzira na to učestalost prikupite svaki brojač, prozor vrijeme manja od 30 minuta može vratiti nijedan zapis.  Postavka vremena prozora za 30 minuta će osigurati da ćete dobiti jedan zapis za svaki povezani izvor koji predstavlja prosjeka tijekom tog razdoblja.

## <a name="alert-actions"></a>Akcije za upozorenje

Osim stvaranja upozorenja zapisa, možete konfigurirati upozorenja pravilo kojim se automatski pokrenuti jednu ili više akcija.  Akcije možete doći obavijesti o upozorenja ili Pozivanje neke proces kojim se pokuša da biste ispravili problem otkrivenog.  U sljedećim odjeljcima opisuju akcije koje su trenutno dostupne.

### <a name="email-actions"></a>Akcije e-pošte
Akcije e-pošte poslati poruku e-pošte s pojedinostima upozorenje jednog ili više primatelja.  Možete odrediti predmet e-pošte, ali je sadržaj je Klasični oblik konstruirana po prijava analitiku.  Uključuje sažetak informacija, kao što su naziv s upozorenjem o uz detalje o najviše deset zapisa vratilo pretraživanje zapisnika.  Također sadrži vezu zapisnika pretraživanja u zapisnik analize koji će se vratiti na čitav skup zapisa iz tog upita.   Je pošiljatelj e-pošte *paket tima za Microsoft operacije upravljanje &lt; noreply@oms.microsoft.com *. 


### <a name="webhook-actions"></a>Webhook akcije

Akcije Webhook omogućuju pozivanje vanjski postupak kroz jedan zahtjev HTTP POST.  Trebali biste servisa koja se poziva podržava webhooks i odredite kako će koristiti bilo koji tereta primi.  Nije moguće poziva i REST API-JA koji ne posebno podržava webhooks dok god je zahtjev u obliku koji možete koristiti u API-JA.  Primjeri korištenja programa webhook kao odgovor na upozorenja koristite servis kao što je [Prazan hod](http://slack.com) da biste poslali poruku s pojedinostima o upozorenja ili pri stvaranju incident u servis kao što je [PagerDuty](http://pagerduty.com/).  

Dovršavanje vodič stvaranje upozorenja pravilo pomoću webhook da biste nazvali uzorka servisa dostupna je na [Webhooks u prijava analitiku upozorenja](log-analytics-alerts-webhooks.md).

Webhooks obuhvaćaju URL-a i tereta oblikovane JSON koji je podataka koji se šalju na servis za vanjske.  Prema zadanim postavkama, opseg neće sadržavati stavku vrijednosti u tablici u nastavku.  Možete odabrati da biste zamijenili ovaj tereta prilagođene jednu od svojih.  Varijable u tom slučaju možete koristiti u tablici za svaku od parametara da biste dodali njihove vrijednosti u svoje prilagođene tereta.


| Parametar | Varijabla | Opis |
|:--|:--|:--|
| AlertRuleName | #alertrulename | Naziv upozorenja pravila. |
| AlertThresholdOperator | #thresholdoperator | Operator prag upozorenja pravila.  *Veće* ili *manje od*. |
| AlertThresholdValue | #thresholdvalue | Vrijednosti praga upozorenja pravila. |
| LinkToSearchResults | #linktosearchresults | Veza na prijava analitiku zapisnika pretraživanja koji vraća zapise iz upit koji je stvorio upozorenje. |
| ResultCount  | #searchresultcount | Broj zapisa u rezultatima pretraživanja. |
| SearchIntervalEndtimeUtc  | #searchintervalendtimeutc | Vrijeme završetka za upit u obliku UTC-a. |
| SearchIntervalInSeconds | #searchinterval | Doba dana upozorenja pravilo. |
| SearchIntervalStartTimeUtc  | #searchintervalstarttimeutc | Početno vrijeme upita u obliku UTC-a. |
| SearchQuery | #searchquery | Zapisnika pretraživanja upit koji se koristi pravilom upozorenja. |
| SearchResults | Potražite u nastavku | Zapisi koji se vratio upit u JSON OSNOVNI oblik.  Ograničen je na prvo 5000 zapisa. |
| WorkspaceID | #workspaceid | ID OMS radnog prostora. |


Na primjer, može odrediti sljedeće prilagođeni opseg koja sadrži jedan parametar zove *tekst*.  Servis za ovaj webhook poziva biste očekivali taj parametar.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

U ovom primjeru tereta želite riješiti nešto ovako kada se šalje na webhook.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Da biste obuhvatili prilagođene tereta rezultata pretraživanja, dodajte sljedeći redak kao svojstvo najviše razine u json opseg.  

    "IncludeSearchResults":true

Na primjer, da biste stvorili prilagođeni tereta koja sadrži samo upozorenja ime i rezultata pretraživanja, možete koristiti sljedeće. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Možete voditi kroz dovršeno primjera stvaranje upozorenja pravilo pomoću webhook da biste pokrenuli vanjskog servisa na [Prijava analitiku upozorenja webhook uzorka](log-analytics-alerts-webhooks.md).

### <a name="runbook-actions"></a>Runbook akcije

Akcije Runbook pokrenuti na runbook u automatizaciji Azure.  Da biste koristili ovu vrstu akcije, morate imati [rješenja za automatizaciju](log-analytics-add-solutions.md) instalacije i konfiguracije u radnom prostoru OMS.  Ako nemate instaliran prilikom stvaranja novog pravila upozorenja, prikazuje se veza na njegove instalacije.  Na raspolaganju su vam runbooks na računu za automatizaciju koje ste konfigurirali u rješenje za automatizaciju.

Akcije Runbook pokrenite runbook pomoću [webhook](../automation/automation-webhooks.md).  Kada stvorite pravilo za upozorenja, automatski stvorit će nove webhook za na runbook s nazivom **OMS upozorenje olakšava** slijedi GUID.  

Ne možete izravno popuniti parametre na kompilacije, ali [$WebhookData parametar](../automation/automation-webhooks.md) neće sadržavati stavku detalje upozorenja, uključujući rezultate zapisnika pretraživanja u kojem je stvorena.  Na runbook će potrebno definirati **$WebhookData** kao parametar za njega da biste pristupili svojstva upozorenja.  Upozorenja podataka dostupna je u JSON osnovni oblik u jedno svojstvo naziva **SearchResults** u svojstvu **RequestBody** **$WebhookData**.  Imat će sa svojstvima u tablici u nastavku.


| Čvor | Opis |
|:--|:--|
| ID-a         | Put i GUID pretraživanja. |
| __metadata | Informacije o upozorenje uključujući broj zapisa i status rezultata pretraživanja. |
|  vrijednost     |  Zasebne stavke za svaki zapis u rezultatima pretraživanja.  Detalje o stavci će odgovarati svojstava i vrijednosti zapisa.   |

Ako, na primjer, sljedeće runbook bi izdvojili zapise rezultata pretraživanja zapisnika i dodjeljivati različita svojstva ovise o vrsti svaki zapis.  Imajte na umu da se pokreće na runbook pretvaranjem **RequestBody** iz json tako da ga se radili s kao objekt u ljusci PowerShell.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value
    
    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer
        
        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }
        
        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="alert-records"></a>Upozorenja zapisa

Upozorenja zapisi stvorio upozorenja pravila u zapisniku analize imati **vrstu** **upozorenja** i **SourceSystem** **OMS**.  U tablici u nastavku imaju svojstva.

| Svojstvo | Opis |
|:--|:--|
| Vrsta          | *Upozorenje* |
| SourceSystem  | *OMS* |
| AlertSeverity | Razina težinu upozorenja. |
| AlertName     | Naziv upozorenja. |
| Upit         | Tekst upita koji je pokrenut.  |
| QueryExecutionEndTime   | Kraj vremenski raspon za upit. |
| QueryExecutionStartTime | Početak vremenski raspon za upit.  |
| TimeGenerated | Datum i vrijeme stvaranja se upozorenje. |

Postoje druge vrste upozorenja zapise stvorene [rješenje upravljanja upozorenja](log-analytics-solution-alert-management.md) i [izvozi Power BI](log-analytics-powerbi.md).  Te su sve **vrste** **upozorenja** , ali se razlikuju po **SourceSystem**.




## <a name="next-steps"></a>Daljnji koraci

- Instalacija [rješenja upravljanja upozorenja](log-analytics-solution-alert-management.md) da biste analizirali upozorenja stvorene u zapisnik analize uz upozorenja koji se prikupljaju iz sustava centra operacije Manager (SCOM).
- Dodatne informacije o [zapisniku pretraživanja](log-analytics-log-searches.md) možete generirati upozorenja.
- Slijedite upute za [Konfiguriranje programa webook](log-analytics-alerts-webhooks.md) s upozorenja pravilo.  
- Saznajte kako napisati [runbooks u automatizaciji Azure](https://azure.microsoft.com/documentation/services/automation) remediate probleme otkrije upozorenjima.