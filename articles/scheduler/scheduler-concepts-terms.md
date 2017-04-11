<properties
 pageTitle="Raspored koncepti, uvjeti i entiteti | Microsoft Azure"
 description="Azure koncepti raspored, terminologija i hijerarhije entitet, uključujući zadacima i posao zbirke.  Prikazuje potpun primjera zakazani zadatak."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>Raspored koncepti, terminologija + entitet hijerarhije

## <a name="scheduler-entity-hierarchy"></a>Raspored entitet hijerarhije

U sljedećoj su tablici opisuju glavni resursa koji prikazuje ili koristi API rasporeda:

|Resurs | Opis |
|---|---|
|**Zbirka posla**|Zbirka posla sadrži grupu zadataka i održava postavke, kvota i Reguliranje koje zajednički koriste poslove unutar zbirke. Zbirka posao je stvorio vlasnik pretplate i grupa poslove zajedno na temelju ograničenja korištenja ili aplikacije. To je ograničeno na određenu regiju. Omogućuje provođenje kvote da biste ograničili korištenje sve zadatke u toj zbirci. Na kvote obuhvaćaju MaxJobs i MaxRecurrence.|
|**Zadatak**|Posao definira jedan ponavljajući akcija, s jednostavnih i složenih Strategije za izvršavanje. Akcije mogu obuhvaćati HTTP, reda čekanja za pohranu, red čekanja bus servisu ili servisne zahtjeve za temu bus.|
|**Povijest zadatka**|Dosadašnje iskustvo predstavlja detalje za izvršavanje posla. Sadrži uspjeh nasuprot pogreške, kao i sve detalje odgovor.|

## <a name="scheduler-entity-management"></a>Upravljanje rasporedom entitet

Visoke razine na raspored i upravljanje servisom API izložiti sljedeće postupke za resursima:

|Mogućnost|Opis i URI adresa|
|---|---|
|**Upravljanje zbirkom posla**|DOBILI, postavljanje i brisanje podrška za stvaranje i izmjenu zbirke posla i zadataka koji se nalaze therein. Zbirka zadatak je spremnik za zadatke i karte da biste kvotama i zajedničke postavke. Primjeri kvote, što je opisano u nastavku, su maksimalan broj zadacima i najmanji intervala ponavljanja. <p>Postavljanje i brisanje:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>POJAVLJUJE SE:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p>
|**Upravljanje**|Početak, STAVITE, OBJAVITE, ZAKRPU i brisanje podršku za stvaranje i mijenjanje zadataka. Sve zadatke mora pripadati zbirka posao koja već postoji, pa nema implicitno stvaranja. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p>|
|**Upravljanje povijest**|PODRŠKA za dohvaćanje 60 dana povijest izvođenja zadatka, kao što je posao Proteklog vremena i posao izvođenja rezultate. Dodaje podršku parametra niza upita za filtriranje na temelju stanje i status. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p>|

## <a name="job-types"></a>Vrsta zadatka

Postoje različite vrste poslovi: poslove HTTP (uključujući HTTPS zadacima koji podržavaju SSL), zadataka reda čekanja za pohranu, zadataka reda čekanja bus servisa i servisa bus tema zadatke. HTTP poslovi su idealna ako imate krajnje postojeće radno opterećenje ili usluge. Prostor za pohranu zadataka reda čekanja možete koristiti da biste objavili poruke u redove prostora za pohranu, pa ti poslovi idealne su za radnih opterećenja koji koristite za pohranu redova. Isto tako, zadacima servisa bus idealne su za radnih opterećenja koji koriste servis bus redovima i teme.

## <a name="the-job-entity-in-detail"></a>"Posao" entitet detaljno

Osnovni razine zakazani posao sadrži nekoliko dijelova:

- Akcija za izvođenje kada aktivira timer posla  

- (Neobavezno) Vrijeme pokretanje posla  

- (Neobavezno) Kada i kako često ponovite zadatak  

- (Neobavezno) Akcija pokreću ne uspijete primarna akcija  

Interno, zakazani posao sadrži i-ovi sustava podatke kao što su sljedeće zakazano vrijeme izvođenja.

Sljedeći kod nudi potpun primjera zakazani zadatak. Detalji o nalaze se u sljedećim odjeljcima.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Kako se vidi u uzorka zakazani zadatak iznad, definicija zadatka sadrži nekoliko dijelova:

- Vrijeme početka ("startTime")  

- Akcije ("Akcije"), koji sadrži akciju pogreške ("errorAction")

- Ponavljanje ("Ponavljanje")  

- Stanje ("stanja")  

- Status ("status")  

- Ponovite pravila ("retryPolicy")  

Provjerimo svaki od ovih detaljno:

## <a name="starttime"></a>startTime

"startTime" je vrijeme početka i omogućuje pozivatelja da biste odredili pomak vremenske zone na žičani u [obliku ISO 8601](http://en.wikipedia.org/wiki/ISO_8601).

## <a name="action-and-erroraction"></a>Akcija i errorAction

"Akcije" akciju poziva za svaku pojavu i opisuje vrstu poziva servisa. Akciju je što će se izvršavati na navedeni raspored. Raspored podržava HTTP, reda čekanja za pohranu, servis bus teme i servisa bus reda čekanja akcije.

Akcija u primjeru iznad je HTTP akcija. U nastavku je primjeru postupka reda čekanja za pohranu:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Slijedi primjer postupka tema bus servisa.

  "Akcije": {"Vrsta": "serviceBusTopic", "serviceBusTopicMessage": {"topicPath": "t1"  
      "polje naziva": "mySBNamespace", "transportType": "netMessaging" / / može biti netMessaging ili AMQP "provjere autentičnosti": {"sasKeyName": "QPolicy", "Vrsta": "sharedAccessKey"}, "poruke": "Neke poruke", "brokeredMessageProperties": {}, "customMessageProperties": {"appname": "FromScheduler"}},}

Slijedi primjer postupka reda čekanja bus servisa:


  "Akcije": {"serviceBusQueueMessage": {"queueName": "q1"  
      "polje naziva": "mySBNamespace", "transportType": "netMessaging" / / može biti netMessaging ili AMQP "provjere autentičnosti": {  
        "sasKeyName": "QPolicy", "Vrsta": "sharedAccessKey"}, "poruke": "Neke poruke",  
      "brokeredMessageProperties": {}, "customMessageProperties": {"appname": "FromScheduler"}}, "Vrsta": "serviceBusQueue"}

"errorAction" je rukovatelj pogreške, akcije pozvati kada ne uspije primarna akcija. Možete koristiti ovu varijablu poziva krajnje obradu pogreške ili slanje obavijesti o korisniku. Moguće je koristiti za komuniciranje sekundarne krajnje točke u slučaju da primarni nije dostupan (npr., u slučaju Izrada na web-mjestu krajnju točku) ili se može koristiti za obavještavanje pogrešku zadužen za krajnje točke. Baš kao i primarna akcija akciju pogreške može biti jednostavnih i složenih logike koji se temelji na druge akcije. Da biste saznali kako stvoriti SAS tokena, pogledajte [Stvaranje i pristup potpis zajednički koristiti](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="recurrence"></a>Ponavljanje

Ponavljanje sastoji se od nekoliko dijelova:

- Učestalost: Jedan od minute, sat, dan, tjedan, mjesec, godine  

- Interval: Interval na navedeni učestalost ponavljanja  

- Propisanim raspored: odredite minuta, sate, radne dane, mjesece i monthdays ponavljanja  

- Broj: Broj ponavljanja  

- Završetak: poslovi će se izvoditi nakon navedeni završno vrijeme  

Zadatak ponavlja ako sadrži ponavljajućeg objekt naveden u njegova definicija JSON. Ako su navedeni broj i endTime, dovršetka pravila koja se pojavljuje najprije je na snazi.

## <a name="state"></a>Stanje

Stanje posla jedan je od četiri vrijednosti: omogućeno, onemogućeno, Dovršeno ili pojavila. Možete POHRANITI ili ZAKRPU zadataka da biste ih ažurirati stanje omogućeno ili onemogućene. Ako je zadatak dovršen ili pojavila, to je završno stanje koje nije moguće ažurirati (iako posao se i dalje mogu izbrisati). Primjer svojstvo Stanje je na sljedeći način:


        "state": "disabled", // enabled, disabled, completed, or faulted
Dovršeni i pojavila se brišu nakon 60 dana.

## <a name="status"></a>status

Kada je pokrenut raspored posla, vratit će se informacije o trenutnom statusu zadatka. Taj objekt nije moguće postaviti korisnik – je postavljen sustav. Međutim, je obuhvaćen u objekt posla (umjesto zasebnom povezane resursa) tako da se jednostavno nešto možete dobiti status zadatka.

Status zadatka obuhvaća vrijeme izvođenja prethodne (ako ih ima), u vrijeme sljedeće zakazano izvršavanja (za zadatke u tijeku), a zbroj izvršavanja zadatka.

## <a name="retrypolicy"></a>retryPolicy

Ako web Scheduler ne uspije, moguće je da biste odredili pravila Ponovi da biste odredili želite li i kako akciju ponoviti. To ovisi o **retryType** objekta – je postavljen na **ništa** ako nema pravila Ponovi, kao što je prikazano gore. Postavite **Fiksno** ako postoji pravila Ponovi.

Da biste postavili pravila Ponovi, možda naveli dvije dodatne postavke: pokušaj interval (**retryInterval**) i broj ponovne pokušaje (**retryCount**).

Pokušajte ponovno, navedenim intervalom s objektom **retryInterval** je interval između ponovne pokušaje. Zadana je vrijednost 30 sekundi, njegov minimalnu vrijednost konfigurirati 15 sekundi, a maksimalne vrijednosti 18 mjeseci. U zbirkama besplatne posao postoje minimalnu konfigurirati vrijednost od jednog sata.  Definirana je u obliku ISO 8601. Isto tako, je vrijednost broj ponovne pokušaje naveden s objektom **retryCount** ; koliko je puta na pokušaj pokušava je. Zadana je vrijednost 4 i maksimalne vrijednosti iznosi 20\. i **retryInterval** i **retryCount** nisu obavezni. Dobiju zadane vrijednosti ako **retryType** postavljena na **Fiksno** i bez određeni su klauzulom izričito.

## <a name="see-also"></a>Vidi također

 [Što je raspored?](scheduler-intro.md)

 [Početak rada s raspored na portalu za Azure](scheduler-get-started-portal.md)

 [Tarife i naplata u rasporedu Azure](scheduler-plans-billing.md)

 [Kako izraditi složene analize i naprednih ponavljanja s Azure raspored](scheduler-advanced-complexity.md)

 [Referenca za Azure raspored REST API-JA](https://msdn.microsoft.com/library/mt629143)

 [Azure referenca cmdleta ljuske PowerShell za raspored](scheduler-powershell-reference.md)

 [Azure visoku dostupnost raspored i pouzdanosti](scheduler-high-availability-reliability.md)

 [Azure ograničenja raspored, zadanih vrijednosti i šifre pogreške](scheduler-limits-defaults-errors.md)

 [Azure izlaznu provjeru autentičnosti rasporeda](scheduler-outbound-authentication.md)
