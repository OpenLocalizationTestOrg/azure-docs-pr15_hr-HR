<properties 
    pageTitle="Kako Zapiši Azure koncentratora događaja u upravljanju API Azure | Microsoft Azure" 
    description="Saznajte kako Zapiši Azure koncentratora događaja u upravljanju Azure API-JA." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Kako Zapiši Azure koncentratora događaja u upravljanju API Azure

Azure koncentratora događaj je servis ingress iznimno skalabilni podataka koje možete ingest milijune događaje u sekundi da bi mogli obraditi i analizirajte pretraživanje velikog količine podataka koje je stvorio povezanih uređaja i aplikacije. Događaj koncentratora služi kao na "prednju kontaktiraju –" na kanal događaja, a kada prikupljanja podataka u koncentratora za događaj ga možete pretvoriti i spremljene pomoću bilo kojeg davatelja usluga u stvarnom vremenu analize ili prilagodnika grupnog slanja promjena/prostora za pohranu. Događaj koncentratora decouples radnog strujanje događaja s potrošnje događaje, tako da događaja koje korisnici mogu pristupiti događaje vlastite rasporedu.

U ovom se članku je pomoćnom videozapis [Integrirati upravljanje API Azure s koncentratorima događaja](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) i opisuje kako Zapiši API upravljanja pomoću Azure koncentratora za događaj.

## <a name="create-an-azure-event-hub"></a>Stvaranje koncentratora za Azure događaja

Da biste stvorili novi događaj koncentrator, prijave [Azure klasični portal](https://manage.windowsazure.com) i kliknite **Novo**->**Aplikacije servisa**->**Bus servisa**->**Događaj koncentrator**->**Brzo stvaranje**. Unesite naziv događaja koncentrator, regiju, odaberite pretplatu, a prostor naziva. Ako već niste stvorili prostor naziva možete stvoriti tako da upišete naziv u tekstni okvir **prostor naziva** . Kada su konfigurirana svojstva cjelokupno sve, kliknite **Stvori novi događaj koncentrator** za stvaranje koncentratora za događaj.

![Stvaranje događaja koncentratora][create-event-hub]

Zatim prijeđite na karticu **Konfiguracija** za nove koncentratora za događaj i stvaranje dva **zajednički pristup pravila**. Dodijelite naziv najprije jedan **Slanje** i dajte dozvole za **Slanje** .

![Slanje pravila][sending-policy]

Naziv drugog jedan **primanje**, dajte mu **preslušali** dozvole i kliknite **Spremi**.

![Primanje pravila][receiving-policy]

Svaki pravilo zajednički pristup omogućuje aplikacije za slanje i primanje događaja i iz koncentratora za događaj. Da biste pristupili niza veze za tim pravilima, idite na karticu **nadzorne ploče** središtu događaja, a zatim kliknite **informacije o vezi**.

![Niz za povezivanje][event-hub-dashboard]

Niz za povezivanje **Slanje** koristi se kada je bilježenje u zapisnik događaja, a niz veze za **primanje** služi prilikom preuzimanja događaje iz centra za događaj.

![Niz za povezivanje][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Stvaranje programa zapisivaču upravljanja API-JA

Sad kad ste koncentratora za događaj, sljedeći je korak konfiguriranje [zapisivaču](https://msdn.microsoft.com/library/azure/mt592020.aspx) na servisu za upravljanje API tako da ga možete Zapiši koncentrator za događaj.

Upravljanje API bilježe tipkanje konfigurirane su [API upravljanje REST API -JA](http://aka.ms/smapi). Prije korištenja REST API-JA prvi put, pregledajte [preduvjeti](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) i bili sigurni da imate [omogućen pristup REST API -JA](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI).

Da biste stvorili na zapisivaču, provjerite zahtjev HTTP POSTAVITE pomoću sljedećih URL predloška.

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   Zamjena `{your service}` pod nazivom vaše instanca servisa za upravljanje API-JA.
-   Zamjena `{new logger name}` s željeni naziv za svoje nove zapisivaču. Pozivati taj naziv prilikom konfiguriranja [zapisnika eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) pravila

Dodajte sljedeća zaglavljima zahtjev.

-   Vrsta sadržaja: aplikacija json
-   Autorizacije: SharedAccessSignature uid =...
    -   Upute za generiranje na `SharedAccessSignature` potražite u članku [Azure API upravljanje REST API autentičnosti](https://msdn.microsoft.com/library/azure/dn798668.aspx).

Navedite tijelu zahtjeva pomoću predloška za sljedeće.

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `type`mora biti postavljeno na `AzureEventHub`.
-   `description`nudi neobavezan opis zapisivaču i može biti niza nulte duljine po želji.
-   `credentials`sadrži na `name` i `connectionString` od koncentratora za događaj Azure.

Kada unesete zahtjev, ako zapisivaču je stvorena kod stanja `201 Created` vraća. 

>[AZURE.NOTE] Druge moguće povrata šifre i njihovih razloge, potražite u članku [Stvaranje na zapisivaču](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT). Da biste vidjeli kako izvesti ostalih operacija kao što su popisa, ažuriranje i brisanje, potražite u dokumentaciji entitet [zapisivaču](https://msdn.microsoft.com/library/azure/mt592020.aspx) .

## <a name="configure-log-to-eventhubs-policies"></a>Konfiguriranje pravilnika o eventhubs zapisnika

Kada vaš zapisivaču konfiguriran u odjeljku Upravljanje API-JA, možete konfigurirati police zapisnika eventhubs da biste se prijavili željeni događaja. Pravila zapisnika eventhubs može se koristiti u odjeljku ulazna pravila ili u odjeljku izlazni pravila.

Konfiguriranje pravilnika Prijava na [portal za Azure klasični](https://manage.windowsazure.com)otvorite servis za upravljanje API pa kliknite **portal za publisher** ili **Upravljanje** da biste pristupili portal za publisher.

![Portal za Publisher][publisher-portal]

Na izborniku Upravljanje API-JA na lijevoj strani kliknite **pravila** , odaberite željeni proizvoda i API pa kliknite **Dodaj pravilo**. U ovom primjeru smo pravilo na dodajte **Jeka API -JA** u proizvodu **neograničeno** .

![Dodavanje pravila][add-policy]

Postavite kursor u na `inbound` pravila sekcije, a zatim kliknite **zapisnik EventHub** pravila da biste umetnuli u `log-to-eventhub` predložak izjave pravila.

![Uređivač pravila][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

Zamjena `logger-id` s nazivom zapisivaču API upravljanje ste konfigurirali u prethodnom koraku.

Možete koristiti bilo koji izraz koji vraća niz kao vrijednost na `log-to-eventhub` element. U ovom primjeru prijavljen je niz koji sadrži datum i vrijeme, naziv usluge, id zahtjeva, zahtjev ip adresa i naziv operacije.

Kliknite **Spremi** da biste spremili konfiguracija ažurirane pravila. Čim se sprema se pravila je aktivna i događaje prijavljeni određenu koncentrator za događaj.

## <a name="next-steps"></a>Daljnji koraci

-   Dodatne informacije o Azure događaj koncentratora
    -   [Početak rada s koncentratorima Azure događaja](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Primanje poruka uz EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Programiranje vodič koncentratora za događaj](../event-hubs/event-hubs-programming-guide.md)
-   Dodatne informacije o Integracija upravljanje API-JA i koncentratora događaja
    -   [Referenca entitet zapisivaču](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [Referenca za zapisnik eventhub pravila](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [Praćenje API-ji s Azure API upravljanja, koncentratora za događaj i Runscope](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Pogledajte videozapis vodič

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]


[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png






