<properties 
    pageTitle="Uvod u strujanje analize | Microsoft Azure" 
    description="Saznajte više o strujanje analize, servis za upravljane koji olakšava analize strujanja podataka s Interneta od stvari (IoT) u stvarnom vremenu." 
    keywords="Analitički kao servis, upravlja services, obrada strujanje, strujanje analize, što je strujanje analytics"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="what-is-stream-analytics"></a>Što je strujanje analize?

Azure strujanje analize je modul obrada potpuno upravljanih troškova učinkovitih događaja u stvarnom vremenu koji olakšava da biste otključali precizno uvida iz podataka. Analitički strujanje olakšava postavljanje u stvarnom vremenu computations analitičke podatke strujanje s uređaja, senzori, web-mjesta, društvene mreže, aplikacije, Infrastruktura sustava i više.

Uz samo nekoliko klikova na portalu za Azure možete autor strujanje analize posla koji određuje unos izvor podataka za strujanje, primatelj izlaz za rezultate vaš posao i transformaciju podataka izražena u SQL nalik jezik. Možete nadzirati te prilagoditi skaliranje/brzinu posla na portalu za Azure skaliranja iz nekoliko kilobajta na gigabajta ili više događaja koji se obrađuju sekundi.

Analitički strujanje upravlja godinama rada Microsoft Research u razvoju vrlo je počeo gledati strujanje modula za obradu osjetljivi vremena, kao i integracije jezik za intuitivno specifikacija kao što su.

## <a name="what-can-i-use-stream-analytics-for"></a>Što mogu koristiti analize strujanje?
Veliku količine podataka su slijedi na visok brzinu pokazivač na žičani danas. Tvrtkama ili ustanovama koje možete postupak i djelovanje na strujanje podatke u stvarnom vremenu možete znatno poboljšati učinkovitosti i same razlikovati na tržištu. Scenariji u stvarnom vremenu strujanje analize nalazi se preko svih delatnosti: personalizirane, u stvarnom vremenu burzovni trgovina analize i upozorenja nudi financijske usluge tvrtke; Otkrivanje prijevarama dostupne su u stvarnom vremenu; Zaštita servisa podataka i identiteta pouzdan ingestion i analiza podataka generira senzori i actuators ugrađene fizičke objektima (Internet stvari ili IoT); web-Analitika clickstream; i aplikacija za upravljanje (CRM) odnosa kupca izdavanja upozorenja kada smanjena je funkcionalnost iskustva kupca unutar vremenskog okvira. Tvrtke tražite na najčešće fleksibilne, pouzdano i učinkovit način da biste učinili takvo analize podataka u stvarnom vremenu strujanje događaja da u svijetu iznimno konkurencije Moderna tvrtke.

## <a name="key-capabilities-and-benefits"></a>Ključne mogućnosti i pogodnosti paketa
-   **Jednostavnost:** Analitički strujanje podržava model jednostavne, deklarativno upit za s opisom transformacije. Da biste optimizirali za jednostavnost korištenja, strujanje analize koristi varijantu T SQL te uklanja potrebe za kupce baviti tehničke complexities toka obrade sustavi. Pomoću [analize strujanje jezika za upite](https://msdn.microsoft.com/library/azure/dn834998.aspx) u uređivaču upita u pregledniku, dobit ćete intelli smislu samodovršetka će vam pomoći možete brzo i jednostavno implementirati vrijeme niza upita, uključujući vremenski sustavom spojevi, prozorima zbrajanja, vremenski filtara i drugih uobičajenih operacija kao što su spojevi, zbrajanja, projekcija i filtri. Osim toga, upita u pregledniku testiranje u odnosu na oglednu datoteku za slanje omogućuje brz i iteracije razvoj.  

-   **Skalabilnost:** Analitički strujanje se može obrade visoke događaj propusnost do 1GB/sekundi. Integracija sa [Azure događaj koncentratora](https://azure.microsoft.com/services/event-hubs/) i [Azure IoT koncentratora](https://azure.microsoft.com/services/iot-hub/) omogućuju rješenje ingest milijune događaja po drugi dolazi iz povezanih uređaja, clickstreams, te datoteka nekoliko zapisnika. Da biste to postigli, strujanje analize upravlja stvaranje particija mogućnost koncentratora događaja koje možete yield 1 MB/s po particije. Korisnici će moći particija izračuni u broj logičke korake u sklopu definicije upita, svaka s mogućnost da biste se dodatno particije da biste povećali skalabilnost.  

-   **Pouzdanost, repeatability i brzo oporavak:** Servis za upravljane u oblaku, strujanje analize pomaže u sprječavanju gubitka podataka i njihovi poslovanje u slučaju pogreške pomoću ugrađenih oporavak mogućnosti. Uz mogućnost interno održavanje stanje, servis nudi kontrolirani rezultate jamči da je moguće arhivirati događaja i ponovna primjena obrade u budućnosti, uvijek početak iste rezultate. Time se omogućuje klijente vratite se u vremenu i istražiti computations pri izvođenju analizu korijenskog uzroka analizu "što ako", itd.  

-   **Niskog troškova:** Kao servis u oblaku, strujanje analize optimiziran je za davanje korisnicima na vrlo malo troškova za brz početak i održavanje rješenja u stvarnom vremenu analize. Servis je ugrađen za plaćanje tijekom rada utemeljene na korištenje strujanje jedinica i količinu podataka koji se obrađuju sustav. Korištenje izvodi se ovisno o količinu događaje obrađuju i iznos računalnim power dodjeli unutar klaster učiniti odgovarajući poslovi strujanje analize.  

-   **Referencu podataka:** Strujanje Analytics omogućuje korisnicima možete navesti i koristiti referentnih podataka. To se može biti povijesne ili jednostavno nisu-strujanja podataka koje rjeđe vremenom mijenja. Sustav pojednostavljuje korištenje referentnih podataka tretirati kao što je bilo koji drugi ulazne događaj strujanje da biste se pridružili s drugim događajima strujanja ingested u stvarnom vremenu za izvođenje transformacije.  

-   **Korisnički definirane funkcije:** Analitički strujanje ima Integracija s Azure strojnog učenja da biste definirali funkcija pozive u servis strojnog učenja kao dio analize strujanje upita. Proširuje mogućnosti analize strujanje odražava postojeća rješenja Azure strojnog učenja. Dodatne informacije o tome pogledajte [Vodič za integraciju strojnog učenja](stream-analytics-machine-learning-integration-tutorial.md).

-   **Povezivanje:** Analitika strujanje povezuje izravno koncentratorima događaj Azure i Azure IoT koncentratora za strujanje ingestion i servis blobova platforme Azure ingest proteklih podataka. Rezultate možete piše s strujanje analize blob polja za pohranu Azure ili tablice, baze podataka SQL Azure, Azure podataka Lake trgovine, DocumentDB, koncentratora događaj, Azure servisa Bus teme ili redovima i Power BI, gdje možete pa ga vizualizacije, dodatno obradili tijekova rada, koristi u obrade analize putem [Servisa Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) ili ponovno obrađuju kao niz događaja u. Kada se koristi događaj koncentratora moguće je da biste sastavili većeg broja strujanja analize zajedno s drugim izvorima podataka i obrada modula bez gubitka strujanje prirode na computations.  

## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci
Koje ste je uvedena strujanje analize, servis za upravljane tok analize podataka s Interneta stvari. Da biste saznali više o taj servis, pogledajte:

- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)

