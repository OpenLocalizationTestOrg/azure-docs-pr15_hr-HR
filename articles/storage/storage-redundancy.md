<properties
    pageTitle="Azure prostora za pohranu replikacijom | Microsoft Azure"
    description="Podatke na računu sustava Microsoft Azure prostora za pohranu je replicirati rok trajanja i visoke dostupnosti. Mogućnosti replikacije obuhvaćaju lokalno suvišnih prostora za pohranu (LRS), zone suvišnih prostora za pohranu (ZRS), zemlj suvišnih prostora za pohranu (GRS) i pristup za čitanje zemlj suvišnih prostora za pohranu (RA GRS)."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="azure-storage-replication"></a>Azure replikacijom prostora za pohranu

Podatke na računu sustava Microsoft Azure prostora za pohranu uvijek replicirati da bi se rok trajanja i visoke dostupnosti. Replikacija kopira podatke, u centru za iste podatke ili na drugi podatkovnog centra, ovisno o tome koju mogućnost replikacije odaberete. Replikacija štiti podatke, a zadržava aplikacije gore-vremenom slučaju tranzitne hardverske pogreške. Ako vaši podaci je replicirati na drugom podatkovnog centra, koje i štiti podatke prema Katastrofalna pogreška u primarnom mjestu.

Replikacija osigurava ispunjava li vaš račun za pohranu [Razini usluge ugovor (SLA) za pohranu](https://azure.microsoft.com/support/legal/sla/storage/) čak i in the face of pogreške. Potražite u članku jamčiti SLA informacije o Azure prostora za pohranu za rok trajanja i dostupnost. 

Kada stvorite račun za pohranu, možete odabrati jednu od sljedećih mogućnosti replikacije:  

- [Lokalno suvišnih prostora za pohranu (LRS)](#locally-redundant-storage)
- [Zone suvišnih prostora za pohranu (ZRS)](#zone-redundant-storage)
- [Zemlj suvišnih prostora za pohranu (GRS)](#geo-redundant-storage)
- [Pristup za čitanje zemlj suvišnih prostora za pohranu (RA GRS)](#read-access-geo-redundant-storage)

Pristup za čitanje zemlj suvišnih prostora za pohranu (RA GRS) je zadana mogućnost prilikom stvaranja novog računa za pohranu.

Sljedeća tablica sadrži kratak pregled razlike između LRS, ZRS, GRS i RA GRS prilikom sljedeće odjeljke adresa za svaku vrstu replikacijom detaljno.

| Strategije replikacijom                                                               | LRS | ZRS | GRS | RA GRS |
|:----------------------------------------------------------------------------------|:---|:---|:---|:------|
| Preko više podatkovnim centrima ponavlja se podataka.                                     | ne  | Da | Da | Da    |
| Podatke možete čitati s sekundarne mjesto i iz primarno mjesto. | ne  | ne  | ne  | Da    |
| Broj primjeraka podataka koji se održava na zasebnom čvorove.                             | 3   | 3   | 6   | 6      |

Pročitajte članak [Cijene za Azure prostora za pohranu](https://azure.microsoft.com/pricing/details/storage/) za informacije o cijenama za različite zalihosti mogućnosti.

>[AZURE.NOTE] Prostor za pohranu Premium podržava samo lokalno suvišnih prostora za pohranu (LRS). Informacije o Premium za pohranu potražite u članku [prostora za pohranu Premium: visokih performansi prostor za pohranu radnih opterećenja virtualnog računala Azure](storage-premium-storage.md).

## <a name="locally-redundant-storage"></a>Lokalno suvišnih prostora za pohranu

Lokalno suvišnih prostora za pohranu (LRS) replicira podataka triput unutar jedinice skaliranje prostora za pohranu koji se nalazi u podatkovnog centra u području u kojem ste stvorili račun za pohranu. Zahtjev za pisanje uspješno vraća samo kada zapisan sve tri replike. Ove tri replike svaki se nalaziti u zasebnom kvara domene i nadogradnje domene unutar jedinice skaliranje jedan prostora za pohranu. 

Skaliranje jedinica za pohranu je zbirka nosači za od čvorove prostora za pohranu. Grupa čvorove koji predstavljaju fizičke jedinica pogreške i može se smatrati čvorove pripadaju isti fizičke bicikle je kvara domene (FD). Nadogradnja domene (UD) je grupa čvorove koje su nadograditi zajedno tijekom nadogradnje servisa (uvođenje). Tri replike širi preko UDs i FDs unutar prostora za pohranu skaliranje jedinice da biste bili sigurni da podataka dostupna je čak i ako kvara hardvera utječe na jednom za bicikle ili kad su čvorove ažuriran tijekom u uvođenje. 

LRS je najniže mogućnost trošak i nudi najmanje rok trajanja u usporedbi s druge mogućnosti. U slučaju Izrada razine podatkovnog centra (fire, flooding etc.) sve tri replike može biti izgubljene ili nepopravljiva. Da biste smanjili taj rizik zemlj suvišnih prostora za pohranu (GRS) preporučuje se za većinu aplikacija.

Lokalno suvišnih prostora za pohranu biti poželjno u određenim slučajevima: 

- Pruža najveće Maksimalna propusnost mogućnostima replikacije Azure prostora za pohranu.

- Ako aplikacija sprema podatke koji možete jednostavno rekonstruirana, mogu se odabrati za LRS.

- Neke aplikacije su ograničeni replikaciju podataka samo unutar države zbog preduvjeti za upravljanje podacima. Područje upareni mogla druge zemlje; Dodatne informacije o parove regija, pogledajte [Azure područja](https://azure.microsoft.com/regions/) .

## <a name="zone-redundant-storage"></a>Zone suvišnih prostora za pohranu

Zone suvišne prostora za pohranu (ZRS) asinkrono replicira podataka u podatkovnim centrima u jednom ili dvjema regije osim pohrani tri replike slično LRS, što pruža veći rok trajanja od LRS. Podaci spremljeni u ZRS je durable čak i ako je primarni podatkovnog centra dostupne ili nepopravljiva.
Korisnici koji su namjeravate koristiti ZRS trebali imati na umu koji: 

- ZRS dostupna je samo za blokiranje blob-ova u glavnu svrhu prostora za pohranu računi, a je podržana samo u verzijama servisa za pohranu 2014. 02 14 i noviji. 

- Budući da asinkronog replikacijom uključuje odgode, slučaju lokalne Izrada moguće je da promjene koje još je replicirati na sekundarnom izgubit će se ako iz primarnih nije moguće oporaviti podatke.

- Replike možda neće biti dostupno dok Microsoft pokreće prebacivanje sekundarnom.

- Računi ZRS nije moguće pretvoriti kasnije LRS ili GRS. Isto tako, postojeći račun LRS ili GRS nije moguće pretvoriti ZRS račun.

- ZRS računi nemaju metriku ili mogućnost zapisivanje. 

## <a name="geo-redundant-storage"></a>Prostor za pohranu suvišnih zemlj.

Zemlj suvišnih prostora za pohranu (GRS) replicira podataka na sekundarnu regiju koju je stotine milja izvan primarni regija. Ako je vaš račun za pohranu GRS omogućena, pa se podaci nalaze durable čak i ako potpuni regionalne prekida ili Izrada u kojem primarni regija nije koje se mogu vratiti.

Za pohranu račun s GRS omogućena, ažuriranje najprije pridaje primarni regiju, gdje je replicirati triput. Zatim ažuriranje asinkrono je replicirati na sekundarnom regiju, gdje je i ponavlja se triput. 

GRS na primarnih i sekundarnih područja upravljanje replike na zasebnom kvara domene i nadogradnja domene unutar skaliranje jedinica za pohranu kao što je opisano s LRS.

Pitanja vezana uz:

- Budući da asinkronog replikacije obuhvaća kašnjenje u slučaju regionalne Izrada moguće je da promjene koje još je replicirati na sekundarnom regija izgubit će se ako iz primarnog regija nije moguće oporaviti podatke.

- Replike nije dostupna ako Microsoft pokreće prebacivanje na sekundarnom regiju.

- Ako aplikacija želi čitanje iz sekundarne regija korisnika trebate omogućiti RA GRS. 

Kada stvorite račun za pohranu, odaberite primarni područja za račun. Sekundarni regija određuje na temelju primarni regija, a nije moguće promijeniti. Sljedeća tablica prikazuje uparivanja primarnih i sekundarnih regija.

| Primarni             | Sekundarni           |
|---------------------|---------------------|
| Sjeverna središnje SAD-a    | Južna središnje SAD-a    |
| Južna središnje SAD-a    | Sjeverna središnje SAD-a    |
| Istočni SAD-a             | Zapad SAD-a             |
| Zapad SAD-a             | Istočni SAD-a             |
| Istok SAD 2           | Središnje SAD-a          |
| Središnje SAD-a          | SAD Istok 2           |
| Sjeverna Europa        | Europa Zapad         |
| Europa Zapad         | Sjeverna Europa        |
| Južna istočnoazijski     | Istočnoazijski           |
| Istočnoazijski           | Južna istočnoazijski     |
| Istočni Kina          | Sjeverna Kina         |
| Sjeverna Kina         | Istočni Kina          |
| Istok Japan          | Japan Zapad          |
| Japan Zapad          | Istok Japan          |
| Južna Brazil        | Južna središnje SAD-a    |
| Istok Australija      | Australija Jugoistok |
| Australija Jugoistok | Istok Australija      |
| Indija Južna         | Središnja Indija       |
| Središnja Indija       | Indija Južna         |
| SAD Gov Iowa         | SAD Gov Virginia     |
| SAD Gov Virginia     | SAD Gov Iowa         |
| Središnja Kanada      | Istok Kanada          |
| Istok Kanada         | Središnja Kanada      |
| Velika Britanija Zapad             | Južna velika Britanija            |
| Južna velika Britanija            | Velika Britanija Zapad             |
| Središnja Njemačka     | Njemačka Sjeveroistok   |
| Njemačka Sjeveroistok   | Središnja Njemačka     |
| Zapad sad 2           | Središnje Zapad SAD-a     |
| Središnje Zapad SAD-a     | Zapad sad 2           |

Najnovije informacije o područja podržava Azure potražite u članku [Azure područja](https://azure.microsoft.com/regions/).
 
## <a name="read-access-geo-redundant-storage"></a>Pristup za čitanje zemlj suvišne prostora za pohranu

Pristup za čitanje zemlj suvišnih prostora za pohranu (RA GRS) Maksimizira dostupnosti za vaš račun za pohranu unosom pristup podacima sekundarne mjestu, osim replikacije preko dva područja koje ste dobili od GRS samo za čitanje. 

Kada omogućite pristup samo za čitanje na podatke u području sekundarne, podataka dostupna je na sekundarnom krajnje točke, osim primarnog krajnja točka za vaš račun za pohranu. Sekundarni krajnjoj točki slična je funkciji primarni krajnje točke, ali dodaje sufiks `–secondary` u naziv računa. Na primjer, ako je primarni krajnja točka za servis Blob `myaccount.blob.core.windows.net`, a zatim se na sekundarnom krajnje točke `myaccount-secondary.blob.core.windows.net`. Tipkovni prečaci za vaš račun za pohranu jednake za oba primarnih i sekundarnih krajnjih točaka.

Pitanja vezana uz:

- Aplikacija je da biste upravljali koje krajnje je interakcija s prilikom korištenja RA GRS. 

- RA GRS namijenjen predvidjeli visoku dostupnost. Skalabilnost uputama, pregledajte [popis za provjeru uspješnosti](storage-performance-checklist.md).

## <a name="next-steps"></a>Daljnji koraci

- [Cijene Azure prostora za pohranu](https://azure.microsoft.com/pricing/details/storage/)
- [O računima Azure prostora za pohranu](storage-create-storage-account.md)
- [Skalabilnost Azure prostora za pohranu i ciljeve](storage-scalability-targets.md)
- [Mogućnosti programa Microsoft Azure pohrane zalihosti i čitanje pristup suvišnih prostora za pohranu zemlj.](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
- [SOSP papira - Azure prostora za pohranu: Vrlo dostupan servis U Oblaku prostora za pohranu s istaknuti dosljednosti](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  
