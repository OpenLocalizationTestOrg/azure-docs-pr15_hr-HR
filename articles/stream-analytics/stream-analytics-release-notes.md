<properties 
    pageTitle="Analitički strujanje napomene | Microsoft Azure" 
    description="Strujanje analize napomene" 
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

#<a name="stream-analytics-release-notes"></a>Napomene o strujanje Analytics

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Napomene uz izdanje 04/15/2016 strujanje Analytics ##

Ovo izdanje sadrži sljedeće ažuriranje.

Naslov | Opis
---|---
Općenito dostupan za Power BI  | [Proizvodi Power BI](stream-analytics-power-bi-dashboard.md) sada obično su dostupne. Uklonjena je isteka autorizacije 90 dana za Power BI. Dodatne informacije o slučajevima gdje autorizacije mora biti obnovljena potražite u odjeljku [Obnavljanje autorizacije](stream-analytics-power-bi-dashboard.md#Renew-authorization) stvaranja nadzorne ploče komponente Power BI.

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Napomene uz izdanje 03/03/2016 strujanje Analytics ##

Ovo izdanje sadrži sljedeće ažuriranje.

Naslov | Opis
---|---
Nove stavke strujanje analize Query Language  | SAQL sada ima [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "GetType MSDN stranice"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "TRY_CAST MSDN stranice") i [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "REGEXMATCH MSDN stranice").

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Napomene uz izdanje 10/12/2015 strujanje Analytics ##

Ovo izdanje sadrži sljedeće ažuriranje.

Naslov | Opis
---|---
Ažuriranje verziju REST API-JA | Ažurirana verzija REST API-JA 2015 10 01. Detalji o pronaći ćete na MSDN- [Strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx) i [integraciju strojnog učenja u strujanje analize](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md).
Azure strojnog učenja Integracija | S ovom izdanju dolazi podrške za Azure strojnog učenja korisnički definirane funkcije. Pogledajte [vodič](stream-analytics-machine-learning-integration-tutorial.md) za dodatne informacije, kao i [Objava Općenito bloga](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx).

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Napomene uz izdanje 11/12/2015 strujanje Analytics ##

Ovo izdanje sadrži sljedeće ažuriranje.

Naslov | Opis
---|---
Novo ponašanje odaberite | Odaberite u strujanje analize proširen da biste omogućili * kao svojstvo pristupnika ugniježđene zapisa. Dodatne informacije pogledajte [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "Složene vrste podataka").

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Napomene uz izdanje 22/10/2015 strujanje Analytics ##

Ovo izdanje sadrži sljedeća ažuriranja.

Naslov | Opis
---|---
Značajke jezika dodatne upita | Strujanje analize je proširen jezik upita tako da obuhvaća sljedeće značajke: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx) [CEILING](https://msdn.microsoft.com/library/azure/mt605286.aspx), [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [FLOOR](https://msdn.microsoft.com/library/azure/mt605240.aspx), [POWER](https://msdn.microsoft.com/library/azure/mt605287.aspx), [znak](https://msdn.microsoft.com/library/azure/mt605290.aspx), [KVADRAT](https://msdn.microsoft.com/library/azure/mt605288.aspx)te [SQRT](https://msdn.microsoft.com/library/azure/mt605238.aspx).
Uklanja zbrajanja ograničenja  | U ovom izdanju uklanja ograničenje od 15 zbrajanja u upitu. Sada je bez ograničenja broja zbrajanja po upitu.
Dodane značajke GRUPE tako da System.Timestamp | Funkcija [GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) sada omogućuje window_type ili [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx).
Dodane POMAK za Tumbling i skakutanje sustava windows | Po zadanom su [Tumbling](https://msdn.microsoft.com/library/azure/dn835055.aspx) i [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) sustav windows poravnati protiv nula vremena (1/1/0001 12:00:00 se UTC-a). Novi (neobavezno) parametar 'offsetsize' omogućuje određivanje prilagođenih offset (ili poravnanje).


## <a name="notes-for-09292015-release-of-stream-analytics"></a>Napomene uz izdanje 09/29/2015 strujanje Analytics ##

Ovo izdanje sadrži sljedeća ažuriranja.

Naslov | Opis
---|---
Azure IoT paket javno pretpregled | Strujanje analize je sve obuhvaćeno javno pretpregled paket IoT Azure.
Integracija Azure Portal | Osim nastavak prisutnosti na portalu za upravljanje Azure strujanje analize sada je integriran u [Azure Portal](https://azure.microsoft.com/overview/preview-portal/). Napomena da strujanje analize funkcionalnost na portalu za pretpregled trenutno podskup funkcije koje se nude portala za upravljanje Azure bez podrške za testiranje upita u pregledniku, Power BI izlaz konfiguracija i pregledavanje ili pri stvaranju novi ulazni i izlazni resursi u pretplatama kojima imate pristup.
Podrška za izlaz DocumentDB | Strujanje Analitika poslova sada možete izlazni oblik [DocumentDB](https://azure.microsoft.com/services/documentdb/).
Podrška za unos IoT koncentratora | Strujanje Analitika poslova sada možete ingest podatke iz koncentratora IoT.
Vremenska OZNAKA tako da za heterogenih događaje | Kada jedan podatkovni strujanje sadrži više vrsta događaja koji se pojavljuju vremenske oznake u različitim poljima, sada možete koristiti [Vremenske oznake tako da](http://msdn.microsoft.com/library/mt573293.aspx) s izrazima da biste odredili polja različite vremenske oznake u svakom slučaju.

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Napomene uz izdanje 09/10/2015 strujanje Analytics ##

Ovo izdanje sadrži sljedeća ažuriranja.

Naslov|Opis
---|---
Podrška za PowerBI grupe|Da biste omogućili zajedničko korištenje podataka s drugim korisnicima za Power BI, zadacima strujanje analize sada možete zapisivati [PowerBI](stream-analytics-define-outputs.md#power-bi) grupama unutar računa za Power BI.

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Napomene uz izdanje 08/20/2015 strujanje Analytics ##

Ovo izdanje sadrži sljedeća ažuriranja.

Naslov|Opis
---|---
Dodana ZADNJE (funkcija) |[Zadnji](http://msdn.microsoft.com/library/mt421186.aspx) funkcija sada je dostupna na strujanje analize zadataka, što vam omogućuje da dohvatiti najnovije događaja u događaj: toka podataka unutar zadanog vremenskog razdoblja.
Nove funkcije polja|Funkcije polja [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) i [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) su dostupne.
Nove funkcije zapisa|Zapis funkcije [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) i [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) su dostupne.

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Napomene uz izdanje 30/07/2015 strujanje Analytics ##

Ovo izdanje sadrži sljedeća ažuriranja.

Naslov|Opis
---|---
Power BI pomoću Id-a samostalne iz Azure ID-a|Ova značajka omogućuje [Izlaz Power BI](stream-analytics-power-bi-dashboard.md) za poslove ASA u odjeljku bilo koju vrstu računa Azure (Live ID-a ili pomoću Id-a). Uz to, možete imati jednu pomoću Id-a za svoj račun za Azure i koristite neku drugu za dopuštanja izlaz Power BI.
Podrška za servis Bus redovi Izlaz|[Servis Bus redova](stream-analytics-define-outputs.md#service-bus-queues) izlaze sada su dostupne u zadacima strujanje analize.
Podrška za izlaz servisa Bus teme|[Servis Bus teme](stream-analytics-define-outputs.md#service-bus-topics) izlaze sada su dostupne u zadacima strujanje analize.

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Napomene uz izdanje 09/07/2015 strujanje Analytics ##

Ovo izdanje sadrži sljedeća ažuriranja.


Naslov|Opis
---|---
Prilagođeno bloba particija Izlaz|Izlaze spremište blobova platforme omogućiti mogućnost Odredite učestalost te izlaz zapisuju blob-ova i strukturu i oblikovanje struktura mapa za izlazne podatke put. 

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Napomene uz izdanje 03/05/2015 strujanje Analytics ##

Ovo izdanje sadrži sljedeća ažuriranja.


Naslov|Opis
---|---
Povećana Maksimalna vrijednost za izlaz slobode redoslijed odstupanje prozora|Maksimalna veličina redoslijed vrsta iz prozora odstupanje sada je 59:59 (mm: SS)
JSON izlazni oblik: Redak zarezom ili polju|Sada postoji mogućnost kada bilježiti blobova ili događaj koncentrator za slanje u obliku ili matrice JSON objekata ili razdvajanje JSON objekte s novi redak. 

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Napomene uz izdanje 04/16/2015 strujanje Analytics ##


Naslov|Opis
---|---
Odgoda konfiguracija računa za pohranu za Azure|Prilikom stvaranja posao strujanje analize u regiji prvi put, zatražit će se da biste stvorili novi račun za pohranu ili odredite postojeći račun za praćenje zadataka strujanje analize u tom području. Zbog kašnjenje u konfiguriranju nadzor stvaranje Promijeni posao strujanje analize u istom području sljedećih 30 minuta tražit će od za određivanje drugi račun za pohranu umjesto prikazuje nedavno konfigurirani onom u račun za nadzor prostora za pohranu padajući popis. Da biste izbjegli stvaranje nepotrebne računa za pohranu, Pričekajte 30 minuta nakon stvaranja posao u regiji prvo prije dodjele resursa dodatne poslova u tom području.
Nadogradnja posla|Trenutačno analize strujanje ne podržava uživo uređivanja definicije ili konfiguracije izvodi posla. Da biste promijenili unos, izlazna, upit, skala ili konfiguracije izvodi posla, najprije morate zaustaviti posao.
Vrste podataka i nenamjerna unos izvora|Ako se naredba CREATE TABLE ne koristi, vrstu unosa proizlazi iz ulaznog oblika, primjerice sva polja iz CSV niz. Polja moraju biti izričito pretvoriti u ispravnu vrstu pomoću funkcije GLUMCIMA da biste izbjegli pogreške uslijed neodgovarajuće vrste.
Polja koja nedostaju su outputted kao vrijednosti null|Pozivate polja koje se ne nalazi u izvoru unos rezultirat će vrijednosti null u događaj izlaz.
POMOĆU naredbe mora prethoditi izjava ODABIRA|U upitu, Izjava ODABIRA moraju pratiti podupiti definirani pomoću naredbe.
Problem – memorije|Ponovno pokrenite strujanje analize poslove s velikim odstupanje za izlaz redoslijed događaja i/ili složene upite održavanje veliku količinu stanje može uzrokovati izvođenje iz memorije, rezultira posao zadatka. Početak i kraj operacije će biti vidljiv u zapisnicima operacija posla. Da biste izbjegli taj problem, skaliranje upita out preko više particija. U buduće izdanje to ograničenje će se spomenuti tako da Smanji performanse negativno utjecati na značajku zadaci umjesto ponovnog pokretanja ih.
Veliki blob unosa bez vremenske oznake tereta može uzrokovati problem – memorije|Troše velike datoteke iz spremišta blobova može uzrokovati strujanje analize poslove pad Ako polje s vremenskom oznakom nije naveden putem vremenske oznake tako da. Da biste izbjegli taj problem, veličina Zadrži svaki blob manje od 10MB.
SQL baze podataka događaja glasnoću ograničenje|Kada koristite baze podataka SQL kao u ciljni Izlaz, vrlo visoka količine podataka za izlaz može uzrokovati posao strujanje analize istek vremena. Da biste riješili taj problem, smanjite izlaznu glasnoću pomoću zbrajanja ili operatori za filtriranje ili odaberite spremište blobova platforme Azure ili događaj koncentratora kao u ciljni izlaz umjesto toga.
PowerBI skupove podataka može sadržavati samo jednu tablicu|PowerBI ne podržava više od jedne tablice u zadani skup podataka.

## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
