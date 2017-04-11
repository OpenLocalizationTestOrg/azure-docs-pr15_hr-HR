<properties
   pageTitle="Distribucija podataka globalno pomoću DocumentDB | Microsoft Azure"
   description="Saznajte više o zemlj replikacije, prebacivanje i podataka planeta skaliranje oporavak koje su pomoću globalni bazama podataka iz Azure DocumentDB, potpuno upravljanih NoSQL servis za baze podataka."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="kipandya"/>
   
   
# <a name="distribute-data-globally-with-documentdb"></a>Distribucija podataka globalno s DocumentDB

> [AZURE.NOTE] Globalni distribucija baze podataka DocumentDB Općenito dostupan je i automatski omogućena za sve novostvorenu DocumentDB račune. Radimo da biste omogućili globalni raspodjele na sve postojeće račune, ali u privremeni, ako želite da se globalni raspodjele omogućena na svoj račun, [obratite se podršci](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) i smo ćete omogućite je za vas sada.

Azure DocumentDB osmišljene potrebama IoT aplikacije koji se sastoji od globalno raspodijeljeno uređaji i internetske skaliranje aplikacije koje se isporučiti iznimno odredište iskustva korisnicima širom svijeta. Te sustavima baza podataka biti namijenjeno test od zadanih niske latencije pristup aplikacije podataka iz više zemljopisna područja dosljednost i dostupnost jamstva dobro definiranom podataka. Kao globalno distribuirati bazu podataka sustava DocumentDB pojednostavljuje globalni distribuciju podataka pomoću računa potpuno upravljanih, više regija baze podataka koji omogućuju Očisti tradeoffs između dosljednost, dostupnost i performanse, sve s odgovarajućih jamstva. Nudi DocumentDB baze podataka računa i visoke dostupnosti, pojedinačna znamenka ms latencies, a zatim više [razine dobro definiranom dosljednost] [consistency], prozirne regionalne prebacivanje s većim brojem homing API-ji i mogućnost elastically mjerilo propusnost i pohranu diljem svijeta. 

  
## <a name="configuring-multi-region-accounts"></a>Konfiguriranje računa za više područja

Konfiguriranje računa DocumentDB za promjenu veličine diljem svijeta možete riješiti za manje od minute putem [portala za Azure](documentdb-portal-global-replication.md). Sve što trebate napraviti je odaberite željenu razinu desnom dosljednost između nekoliko razina podržani dobro definiranom dosljednost i bilo koji broj Azure područja pridružiti računa baze podataka. DocumentDB dosljednost razine omogućuju Očisti tradeoffs između određenih dosljednost jamstva i performanse. 

![DocumentDB nudi više, dobro definiran (Opuštena) dosljednost modela na raspolaganju][1]

Dobro DocumentDB nudi više definirati (Opuštena) dosljednost modela na raspolaganju.

Odabir razinu desnom dosljednost ovisi o podacima dosljednost jamči vašim potrebama aplikacije. DocumentDB automatski replicira podataka u svim navedenim regijama i jamčiti dosljednost koji ste odabrali za vaš račun baze podataka. 


## <a name="using-multi-region-failover"></a>Korištenje više područja prebacivanje 

Azure DocumentDB je moći računi proziran baze podataka za prebacivanje na više područja Azure – novi [više homing API-ji] [ developingwithmultipleregions] jamstva aplikacije možete nastaviti koristiti logičke krajnje točke te je osigurao tako da na prebacivanje. Prebacivanje upravlja vam, pod uvjetom fleksibilnost rehome bazu podataka računa u događaj bilo koji od raspon uvjeta moguće pogreška se pojaviti, uključujući aplikacije, Infrastruktura, servis ili regionalne pogreške (realni ili Simulirani). U slučaju pogreške regionalnih DocumentDB, servis proziran neće uspjeti putem računa baze podataka, a aplikacija će se i dalje pristupiti podacima bez gubitka dostupnost. Dok DocumentDB nudi [99,99% dostupnost SLA][sla], vaša aplikacija end da biste svojstva dostupnost završetka testirajte obje [programski] simulaciju regionalnih pogreške[ arm] dobro, kao što je putem portala za Azure.


## <a name="scaling-across-the-planet"></a>Promjena veličine preko na planeta
DocumentDB omogućuje neovisno Dodjela propusnost i globalno zauzeti prostor za pohranu za svaku zbirku DocumentDB na bilo kojem razini preko sva područja povezani s računom za bazu podataka. Zbirka DocumentDB automatski distributed globalno i upravlja u svim regijama povezani s računom za bazu podataka. Zbirke unutar vašeg računa baze podataka mogu distribuirati preko bilo koja od Azure regija u kojoj [DocumentDB servis nije dostupan][serviceregions]. 

Propusnost kupili i prostora za pohranu za svaku zbirku DocumentDB je automatski dodjeli preko sva područja jednako. Time se omogućuje aplikacije da biste jednostavno skaliranje preko globusa [plaćanja samo za propusnost i prostora za pohranu koristite unutar svaki sat][pricing]. Ako, primjerice, ako ste dodjeli 2 milijuna RUs za zbirku DocumentDB, pa svakoj od tih regija povezani s računom za baze podataka može vidjeti 2 milijuna RUs za tu zbirku. To je prikazanom u nastavku.

![Skaliranja propusnost za zbirku DocumentDB preko četiri područja][2]

DocumentDB jamčiti < 10 ms čitanje i pisanje < 15 ms na latencies na P99. Čitanje zahtjeva za nikad ne obuhvaćaju podatkovnog centra granicu jamči [dosljednost preduvjeti odaberete][consistency]. Za zapisivanje uvijek su kvorum izvršenom lokalno prije nego što se oni su označeni za klijente. Svaki račun baze podataka konfiguriran prioritet područja za pisanje. Područje određen s najvećim prioritetom će služiti kao trenutno područje za unos za račun. Sve SDK-ovi proziran usmjeravanje zapisivanja račun baze podataka u trenutno područje za unos. 

Na kraju, budući da je DocumentDB potpuno [sheme agnostic] [ vldb] -nikad ne morate brinuti o upravljanju/ažuriranja sheme ili sekundarne preko više podatkovnim centrima. [SQL upitima] [ sqlqueries] tijekom vašeg računala i modeli podataka nastaviti poslovanja nastaviti s radom. 


## <a name="enabling-global-distribution"></a>Omogućivanje globalni distribuciju. 

Možete odlučiti da bi vaši podaci lokalno ili globalno distributed Pridruživanjem ili jedan ili više područja Azure s na DocumentDB baze podataka računa. Možete dodati ili ukloniti područja na račun za baze podataka u bilo kojem trenutku. Da biste omogućili globalni distribucije pomoću portala za, potražite [u](documentdb-portal-global-replication.md)članku izvođenje DocumentDB globalni baze podataka replikacije pomoću portala za Azure. Da biste omogućili globalni raspodjele programatically, potražite u članku [razvojnom programiranju s računima DocumentDB više područja](documentdb-developing-with-multiple-regions.md).

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o distributing podaci globalno DocumentDB u sljedećim člancima:

* [Dodjeljivanje propusnost i prostora za pohranu za zbirku] [throughputandstorage]
* [Više homing API-ji putem ostalih. .NET, Java, Python i čvor SDK-ovi] [developingwithmultipleregions]
* [Razina dosljednost u DocumentDB] [consistency]
* [Dostupnost SLA] [sla]
* [Upravljanje računom baze podataka] [manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: documentdb-partition-data.md
[consistency]: documentdb-consistency-levels.md
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: documentdb-developing-with-multiple-regions.md
[createaccount]: documentdb-create-account.md
[manageaccount]: documentdb-manage-account.md
[manageaccount-consistency]: documentdb-manage-account.md#consistency
[throughputandstorage]: documentdb-manage.md
[arm]: documentdb-automation-resource-manager-cli.md
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/en-us/regions/#services 
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/ 
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: documentdb-sql-query.md

