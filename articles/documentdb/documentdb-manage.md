<properties
    pageTitle="DocumentDB prostora za pohranu i performanse | Microsoft Azure" 
    description="Saznajte više o podataka i prostor za pohranu za pohranu dokumenata u DocumentDB i kako promjena veličine DocumentDB potrebama kapaciteta aplikacije." 
    keywords="Spremanje dokumenta"
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="syamk"/>

# <a name="learn-about-storage-and-predictable-performance-provisioning-in-documentdb"></a>Dodatne informacije o prostora za pohranu i predvidljivi performanse Dodjela resursa u DocumentDB
Azure DocumentDB je potpuno upravljanih, skalabilni vezanima uz dokument NoSQL baze podataka usluga za JSON dokumente. S DocumentDB, ne morate iznajmljivati virtualnim strojevima, implementaciju softvera ili praćenje baze podataka. DocumentDB je kojim upravlja i neprestano nadzire Microsoftovi inženjeri izlaganje svjetske klase dostupnost, performanse i zaštitu podataka.  

Početak rada s DocumentDB tako da [stvorite račun za baze podataka](documentdb-create-account.md) i bazom [DocumentDB baze podataka](documentdb-create-database.md) pomoću [portala za Azure](https://portal.azure.com/). Baza podataka DocumentDB se nude u jedinicama solid-state pogon (SSD) sigurnosno prostora za pohranu i propusnost. Ove jedinice za pohranu su resursi, [Stvaranje zbirki baze podataka](documentdb-create-collection.md) u bazu podataka računa za svaku zbirku s rezerviranim propusnost koje mogu biti skalirana prema gore ili dolje da bi odgovarao zahtjeva vaše aplikacije u bilo kojem trenutku. 

Ako aplikacija prelazi na rezervirane propusnost za jednu ili više zbirki, zahtjevi za ograničen na temelju po zbirke. To znači da zahtjevi za neke aplikacije mreži dok drugi mogu biti ograničio vrijeme.

Ovaj članak sadrži pregled resursa i metriku dostupne za upravljanje kapaciteta i planiranje pohrana podataka. 

## <a name="database-account"></a>Račun baze podataka
Kao Azure pretplatnik, možete Dodjela jedan ili više računa DocumentDB baze podataka da biste upravljali resursa u bazi podataka. Pretplate povezan je s jednom Azure pretplate. 

Računi DocumentDB moguće je putem [portala za Azure](documentdb-create-account.md)ili pomoću [OKVIRA predložak ili Azure EŽA](documentdb-automation-resource-manager-cli.md).

## <a name="databases"></a>Baza podataka
Jedan DocumentDB baze podataka može sadržavati gotovo neograničenu količinu prostora za pohranu dokumenata na grupirane u zbirke. Zbirke pružaju odvajanja performanse – svake zbirke možete nudi propusnost koje je zajedničko korištenje druge zbirke u istu bazu podataka ili račun. Baza podataka za DocumentDB je elastic veličine u rasponu od GBs spremišta TBs od SSD sigurnosno i dodijeljenu propusnost. Za razliku od običnoj bazi podataka RDBMS, baze podataka u DocumentDB je usmjeren na jednom računalu, a mogu obuhvaćati više strojeva ili klastere.  

S DocumentDB, kao što je potrebno da biste skalirali aplikacije, možete stvoriti više zbirki ili baze podataka ili i jedno i drugo. Baza podataka mogu se kreirati putem [portala za Azure](documentdb-create-database.md) ili neki od [DocumentDB SDK-ovi](documentdb-dotnet-samples.md).   

## <a name="database-collections"></a>Zbirke baze podataka
Svaki DocumentDB baze podataka može sadržavati jednu ili više zbirki. Zbirke poslužiti kao particije iznimno dostupna podataka za pohranu dokumenata i obrada. Svaku zbirku možete spremiti dokumente s heterogenih shemom. Automatsko indeksiranje DocumentDB korisnika i mogućnosti upita omogućuju jednostavno filtriranje i dohvat dokumenata. Zbirka nudi opseg za izvršavanje upita i prostor za pohranu dokumenata. Zbirka je transakcija domene za sve dokumente koje se nalaze u njoj. Zbirke su dodijeliti propusnost na temelju vrijednosti postavljena na portalu za Azure ili putem na SDK-ovi. 

Zbirke automatski particije u jednom ili više fizičke poslužitelja tako da DocumentDB. Prilikom stvaranja zbirke možete odrediti dodijeljenu propusnost pomoću zahtjev jedinica po i druga particija ključa svojstvo. Vrijednost tog svojstva DocumentDB upotrebljava za raspodjelu dokumenata među particije i zahtjevi za usmjeravanje kao što su upiti. Vrijednost ključa particije i djeluje kao granicu naslova transakcijom za pohranjene procedure i okidača. Svaku zbirku sadrži rezervirana količinu propusnost specifične za tu zbirku koji se zajednički koristi s drugim zbirkama u isti račun. Stoga skaliranje izvan vaše aplikacije i za pohranu i propusnost. 

Zbirke možete stvoriti putem [portala za Azure](documentdb-create-collection.md) ili neki od [DocumentDB SDK-ovi](documentdb-sdk-dotnet.md).   
 
## <a name="request-units-and-database-operations"></a>Zahtjev za jedinice i postupaka baze podataka

Kada stvorite zbirku, rezervirati propusnost pomoću [zahtjev jedinice (Pravi)](documentdb-request-units.md) u sekundi. Umjesto toga mislim o i upravljanja resursima hardvera možete smatrati **jedinica zahtjev za** jednu mjeru za resurse moraju izvršiti razne operacije baze podataka i usluge zahtjeva aplikacije. Čitanje dokumenta 1 KB troši isti 1 Pravi bez obzira na broj stavki pohranjenih na web-mjesto u zbirci ili u okvir za broj istovremeni zahtjevi izvodi istovremeno. Sve zahtjeve protiv DocumentDB, uključujući složene operacija kao što su SQL upite imaju predvidljivi Pravi vrijednost koja se mogu se odrediti trenutku razvoj. Ako znate veličina dokumenata i učestalost svake operacije (čitanja, pisanja i upita) za podršku za aplikaciju, Dodjela točan iznos propusnost potrebama vaše aplikacije i skaliranje bazu podataka prema gore ili dolje kako se mijenjaju potrebe performansi. 

Svaku zbirku može biti rezervirana s propusnost u blokovima 100 jedinica zahtjev sekundi, iz stotina najviše milijune zahtjev jedinice sekundi. Dodjela resursa propusnost može se prilagoditi tijekom trajanja zbirke prilagoditi promjenom obrada potrebama i dohvaćanje uzorke aplikacije. Dodatne informacije potražite u članku [DocumentDB performanse razine](documentdb-performance-levels.md). 

>[AZURE.IMPORTANT] Zbirke su entiteti za naplatu. Dodjela resursa propusnost zbirke mjere u jedinicama zahtjev sekundi uz ukupan prostor za pohranu za potrošenih u gigabajta određen trošak. 

Koliko je jedinica zahtjev zauzeti će određena operacija kao što su Umetanje, brisanje, upit ili pohranjena procedura izvođenja? Jedinica zahtjev je mjera normaliziranu zahtjeva za obradu trošak. Čitanje dokumenta 1 KB je 1 Pravi, ali zahtjeva za umetanje, zamjena ili brisanje isti dokument zauzeti će dodatne obrade iz servisa i time više jedinica zahtjev. Svaki odgovor servisa sadrži prilagođeno zaglavlje (`x-ms-request-charge`) koji izvješća jedinice zahtjev za zahtjev. Putem [SDK-ovi](documentdb-sdk-dotnet.md)dostupan je i tog zaglavlja. U .NET SDK [RequestCharge](https://msdn.microsoft.com/library/azure/dn933057.aspx#P:Microsoft.Azure.Documents.Client.ResourceResponse`1.RequestCharge) je svojstvo [ResourceResponse](https://msdn.microsoft.com/library/azure/dn799209.aspx) objekta. Ako želite za procjenu vašim potrebama propusnost prije uspostavljanja poziva jednu, možete koristiti [planer kapaciteta](documentdb-request-units.md#estimating-throughput-needs) će vam pomoći u ovom procjeni. 

>[AZURE.NOTE] Osnovni redak 1 jedinice zahtjev za dokument 1 KB odgovara da biste jednostavno VIDJELI dokumenta s [Dosljednost sesiju](documentdb-consistency-levels.md). 

Postoji nekoliko čimbenika utječe jedinice zahtjev za operaciju u odnosu na račun za baze podataka DocumentDB. Sljedećih čimbenika obuhvaćaju sljedeće:

- Veličina dokumenta. Kao što je dokument veličine povećati jedinice potrošena za čitanje ili pisanje i povećava podatke.
- Broj svojstva. Pod pretpostavkom zadani indeksiranjem sva svojstva, jedinice potrošena pisati dokumenta povećava se kao povećava broj svojstva.
- Dosljednost podataka. Prilikom korištenja razine dosljednost podataka Strong ili Bounded Staleness dodatnih jedinica će se utrošiti čitanje dokumenata.
- Indeksirana svojstva. Pravilo indeksa na svakoj zbirci određuje svojstva koja su indeksirati prema zadanim postavkama. Vaš zahtjev jedinica potrošnje možete smanjiti ograničavanjem broja indeksirana svojstva. 
- Indeksiranje dokumenta. Po zadanom svaki dokument će se automatski indeksirati će zauzimaju manje jedinice zahtjeva Ako odlučite da ne želite indeksirati neka vaši dokumenti.

Dodatne informacije potražite u članku [DocumentDB zahtjev jedinice](documentdb-request-units.md). 

Ako, na primjer, Evo tablice koji pokazuje koliko zahtjev jedinice za dodjelu resursa s tri dokumenta s različitim veličinama (1KB, 4KB i 64KB) i na dvije razine različite performansi (čitanja druge 500 + zapisivanja druge 100 i 500 čitanja/sekundi + zapisivanja druge 500). Dosljednost podataka je konfiguriran na sesije i indeksiranja pravilnik nije postavljen tako da ništa.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Veličina dokumenta</strong></p></td>
            <td valign="top"><p><strong>Čitanja/sekundi</strong></p></td>
            <td valign="top"><p><strong>Zapisivanje u sekundi</strong></p></td>
            <td valign="top"><p><strong>Zahtjev za jedinice</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1) + (100* 5) = 1000 Pravi/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *5) + (100* 5) = 3000 Pravi/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1.3) + (100* 7) = 1,350 Pravi/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *1.3) + (500* 7) = 4,150 Pravi/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *10) + (100* 48) = 9,800 Pravi/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *10) + (500* 48) = 29,000 Pravi/s</p></td>
        </tr>
    </tbody>
</table>

Upiti, pohranjene procedure i okidača zauzimaju zahtjev jedinice složenost postupaka koji se izvode na temelju. Kao što je razvoj aplikacija, provjeri u zaglavlju zahtjev obavijesti da biste bolje razumjeli kako svaka operacija koristi kapaciteta jedinica zahtjev.  


## <a name="choice-of-consistency-level-and-throughput"></a>Odabir razine dosljednost i propusnost
Odabir zadane razine dosljednost ima utjecaj na propusnost i Latencija. Zadana razina dosljednost možete postaviti i programski i putem portala za Azure. Možete nadjačati i dosljednost razinu na temelju po zahtjev. Prema zadanim postavkama razinu dosljednost je postavljena na **sesiju**, koji pruža monotonic čitanja/pisanja i čitati vaše jamstva za pisanje. Sesije dosljednost sjajan je za korisnika usmjereni aplikacije i njihovi idealna saldo dosljednost i performanse gubitke.    

Upute o promjeni razinu zaštite dosljednost portala za Azure potražite u članku [Upravljanje računom DocumentDB](documentdb-manage-account.md#consistency). Ili dodatne informacije o razinama dosljednost potražite u članku [Korištenje dosljednost razine](documentdb-consistency-levels.md).

## <a name="provisioned-document-storage-and-index-overhead"></a>Dodjela resursa dokumenta indirektni spremišta i indeksa
DocumentDB podržava stvaranja i jednom i particioniranom zbirke. Svaki particije u DocumentDB podržava veličine do 10 GB prostora za pohranu SSD sigurnosno. 10GB prostora za pohranu dokumenata uključuje dokumenata i prostora za pohranu za indeks. Prema zadanim postavkama DocumentDB zbirke je konfiguriran za sve dokumente automatski indeksirati bez izričito sekundarne indeksi ni shemu. Na temelju aplikacije koje koriste DocumentDB indirektni uobičajeni indeks je između 2-20%. Indeksiranje tehnologija koristi DocumentDB osigurava da bez obzira na to vrijednosti svojstava, indeks indirektnog nije veća od više od 80% veličina dokumente sa zadanim postavkama. 

Po zadanom sve dokumente se indeksirati DocumentDB automatski. Međutim, ako želite precizno indirektni indeks, možete odabrati da biste uklonili određene dokumente iz indeksiraju prilikom umetanja ili zamjene dokumenta, kao što je opisano u [DocumentDB indeksiranja pravila](documentdb-indexing-policies.md). Možete konfigurirati zbirke DocumentDB da biste isključili sve dokumente unutar zbirke iz indeksiraju. Možete konfigurirati i DocumentDB zbirke selektivno indeksirati samo određenih svojstva ili putova sa zamjenskim znakovima JSON dokumenata, kao što je opisano u [Konfiguracija indeksiranja pravila zbirke](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection). Osim svojstva ili dokumente i poboljšava propusnost pisanje – što znači da će zauzeti manje jedinice zahtjev.   

## <a name="next-steps"></a>Daljnji koraci

Daljnje informacije o funkcioniranje DocumentDB, potražite u članku [Partitioning i promjena veličine u Azure DocumentDB](documentdb-partition-data.md).

Upute za praćenje performansi razine portala za Azure, potražite [Monitor DocumentDB računa](documentdb-monitor-accounts.md). Dodatne informacije o odabiru performanse razine za zbirke potražite u članku [performanse razine u DocumentDB](documentdb-performance-levels.md).
 
