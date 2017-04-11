<properties
    pageTitle="Strujanje izlaze analize: mogućnosti za pohranu, analizu | Microsoft Azure"
    description="Saznajte više o određivanje mogućnosti izlaza podataka strujanje analize, uključujući Power BI za rezultate analize."
    keywords="transformaciju podataka, rezultate analize, a zatim mogućnosti pohrane podataka"
    services="stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage"
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

# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Strujanje izlaze analize: mogućnosti za pohranu, analizu

Kad za izradu analize toka posla, imajte na umu kako će potrošena rezultata. Kako će prilikom prikaza rezultata posao strujanje analize i gdje će spremati ga?

Da biste omogućili raznih aplikacije uzoraka Azure strujanje analize ima različite mogućnosti za spremanje izlazne i prikazivanje rezultate analize. To olakšava prikaz izlaz posao te vam nudi fleksibilnosti pri potrošnje i pohranu izlaza posla u skladištenja ili druge svrhe. Sve izlazne konfiguriran u posao mora postojati posao pokreće i događaje početak slijedi. Na primjer, ako koristite spremište blobova platforme kao u izlaz, posao će ne stvorite račun za pohranu automatski. Potrebne za stvaranje korisnik prije pokretanja posla ASA.

## <a name="azure-data-lake-store"></a>Spremište Lake podataka za Azure

Analitički strujanje podržava [Lake spremišta podataka za Azure](https://azure.microsoft.com/services/data-lake-store/). U ovom prostora za pohranu omogućuje vam da biste pohranili podatke veličinu, vrstu i ingestion brzinu domena i exploratory analitičkih. U isto vrijeme stvaranja i konfiguracija spremišta podataka Lake izlaza podržano je samo u portalu klasični Azure. Dodatno, strujanje analize mora biti ovlašteni da biste pristupili Lake pohrane podataka. Detalje o autorizacije i kako se registrirati za pretpregled podataka Lake trgovine (Ako je potrebno) se spominju u [članku izlaz Lake podataka](stream-analytics-data-lake-output.md).

### <a name="authorize-an-azure-data-lake-store"></a>Autorizirajte je iz trgovine Azure podataka Lake

Pohrana podataka Lake odabrano kao na Izlaz na portalu za upravljanje Azure zatražit će se da biste autorizirali veze za postojeće spremište Lake podataka.  

![Autorizirajte spremišta Lake podataka](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Zatim popunite svojstva za izlaz iz trgovine Lake podataka kao što se vidi ispod:

![Autorizirajte spremišta Lake podataka](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

U tablici u nastavku navedeni nazivi svojstava i njihov opis potrebne za stvaranje spremišta podataka Lake izlaz.

<table>
<tbody>
<tr>
<td><B>NAZIV SVOJSTVA</B></td>
<td><B>OPIS</B></td>
</tr>
<tr>
<td>Pseudonim za izlaz</td>
<td>Ovo je neslužbeni naziv koji se koristi u upitima za usmjeravanje izlaz upita u ovom Lake spremišta podataka.</td>
</tr>
<tr>
<td>Naziv računa</td>
<td>Naziv računa za pohranu Lake podataka koje šaljete na izlaz. Primit ćete s padajućeg popisa računa spremišta podataka Lake kojoj prijavljeni na portal korisnik ima pristup.</td>
</tr>
<tr>
<td>Put prefiks uzorak [<I>neobavezni</I>]</td>
<td>Put datoteke za pisanje datoteka unutar navedeni račun pohrane podataka Lake. <BR>{date}, {time}<BR>Primjer 1: folder1/zapisnika / {date} / {vremena}<BR>Primjer 2: folder1/zapisnika / {date}</td>
</tr>
<tr>
<td>Oblik datuma [<I>neobavezni</I>]</td>
<td>Ako se u put prefiks koristi token datum, možete odabrati oblik datuma u kojima su organizirane datotekama. Primjer: Gggg/MM/DD</td>
</tr>
<tr>
<td>Oblik vremena [<I>neobavezni</I>]</td>
<td>Ako se u put prefiks koristi token vrijeme, navedite oblik vremena u kojem su organizirane datotekama. Trenutno je jedini podržani vrijednost HH.</td>
</tr>
<tr>
<td>Događaj serijaliziranog oblika</td>
<td>Oblik serijalizacije za izlazne podatke. Podržane su JSON, CSV i Avro.</td>
</tr>
<tr>
<td>Kodiranje</td>
<td>Ako je oblik CSV ili JSON, kodiranja mora biti navedena. UTF-8 trenutno samo podržani oblik kodiranja.</td>
</tr>
<tr>
<td>Graničnik</td>
<td>Samo primjenjivo za serijalizacije CSV. Analitički strujanje podržava nekoliko uobičajenih razdjelnici za Serijalizacija CSV podatke. Podržani vrijednosti su zarezom sa zarezom, razmak, uvlaka i okomitu crtu.</td>
</tr>
<tr>
<td>Oblikovanje</td>
<td>Samo primjenjivo za serijalizacije JSON. Redak odvojene određuje izlaz će se oblikovati tako da svaki objekt JSON odvojene novi redak. Polja određuje izlaz će se oblikovati kao polje JSON objekata.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Obnavljanje autorizacije spremišta Lake podataka

Morat ćete ponovno autentičnost računa spremišta podataka Lake ako lozinku promijenjena jer je stvoren ili zadnje autentičnost posla.

![Autorizirajte spremišta Lake podataka](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  


## <a name="sql-database"></a>SQL baze podataka

[Baze podataka SQL Azure](https://azure.microsoft.com/services/sql-database/) može se koristiti kao na izlaz za podatke koji su relacijski u prirode ili aplikacije koje ovise o sadržaj koji se nalazi u relacijske baze podataka. Strujanje analize poslove će pisati u postojeću tablicu u bazi podataka SQL Azure.  Imajte na umu da shemu tablica mora u potpunosti odgovarati polja i njihove vrste se izlaza iz posla. [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) možete i navesti kao programa izlazna preko baze podataka SQL izlaz mogućnost kao i (to je značajke pretpregleda). U tablici u nastavku navedeni nazivi svojstava i njihov opis za stvaranje baze podataka SQL izlaz.

| Naziv svojstva | Opis |
|---------------|-------------|
| Pseudonim za izlaz | Ovo je neslužbeni naziv koji se koristi u upitima za usmjeravanje izlaz upita s bazom podataka. |
| Baze podataka | Naziv baze podataka koje šaljete na Izlaz |
| Naziv poslužitelja | Naziv poslužitelja baze podataka SQL |
| Korisničko ime | Korisničko ime koje ima pristup pisati u bazi podataka |
| Lozinke | Lozinku za povezivanje s bazom podataka |
| Tablica | Naziv tablice koju će biti napisani izlaz. Naziv tablice nije velika i mala slova i shema u ovoj su tablici moraju se podudarati točno broju polja i njihove vrste generira izlaz posao. |

> [AZURE.NOTE] Trenutno nuditi baze podataka SQL Azure nije podržana za posao Izlaz u strujanje analize. Međutim, nije podržan programa Azure virtualne računalo sa sustavom SQL Server s bazom podataka povezan. Ovo je mogu se promijeniti u buduća izdanja.

## <a name="blob-storage"></a>Spremište blobova platforme

Spremište blobova platforme nudi učinkovit i skalabilni rješenja za pohranu velikih količina podataka nestrukturirane u oblaku.  Uvod u spremište blobova platforme Azure i njegova korištenja potražite u dokumentaciji pri [korištenju blob-ova](../storage/storage-dotnet-how-to-use-blobs.md).

U tablici u nastavku navedeni nazivi svojstava i njihov opis za stvaranje blob izlaz.

<table>
<tbody>
<tr>
<td>NAZIV SVOJSTVA</td>
<td>OPIS</td>
</tr>
<tr>
<td>Pseudonim za izlaz</td>
<td>Ovo je neslužbeni naziv koji se koristi u upitima za usmjeravanje izlaz upita u ovom blobova.</td>
</tr>
<tr>
<td>Račun za pohranu</td>
<td>Naziv računa za pohranu koju šaljete na izlaz.</td>
</tr>
<tr>
<td>Ključ za račun za pohranu</td>
<td>Tajna tipku povezanu s računom za pohranu.</td>
</tr>
<tr>
<td>Spremnik za pohranu</td>
<td>Spremnici sadrže logičke grupiranja za pohranjene na servisu Microsoft Azure Blob blob-ova. Kada prenesete blob Blob servisa, morate navesti spremnik za taj blob.</td>
</tr>
<tr>
<td>Put prefiks uzorak [neobavezni]</td>
<td>Put datoteke za pisanje blob-ova unutar navedeni kontejnera.<BR>U parametru path, možete koristiti jednu ili više instanci sljedeće 2 varijable Odredite učestalost koji se pišu blob-ova:<BR>{date}, {time}<BR>Primjer 1: cluster1/zapisnika / {date} / {vremena}<BR>Primjer 2: cluster1/zapisnika / {date}</td>
</tr>
<tr>
<td>Oblik datuma [neobavezni]</td>
<td>Ako se u put prefiks koristi token datum, možete odabrati oblik datuma u kojima su organizirane datotekama. Primjer: Gggg/MM/DD</td>
</tr>
<tr>
<td>Oblik vremena [neobavezni]</td>
<td>Ako se u put prefiks koristi token vrijeme, navedite oblik vremena u kojem su organizirane datotekama. Trenutno je jedini podržani vrijednost HH.</td>
</tr>
<tr>
<td>Događaj serijaliziranog oblika</td>
<td>Oblik serijalizacije za izlazne podatke.  Podržane su JSON, CSV i Avro.</td>
</tr>
<tr>
<td>Šifriranje</td>
<td>Ako je oblik CSV ili JSON, kodiranja mora biti navedena. UTF-8 trenutno samo podržani oblik kodiranja.</td>
</tr>
<tr>
<td>Graničnik</td>
<td>Samo primjenjivo za serijalizacije CSV. Analitički strujanje podržava nekoliko uobičajenih razdjelnici za Serijalizacija CSV podatke. Podržani vrijednosti su zarezom sa zarezom, razmak, uvlaka i okomitu crtu.</td>
</tr>
<tr>
<td>Oblikovanje</td>
<td>Samo primjenjivo za serijalizacije JSON. Redak odvojene određuje izlaz će se oblikovati tako da svaki objekt JSON odvojene novi redak. Polja određuje izlaz će se oblikovati kao polje JSON objekata.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Koncentrator za događaj

[Događaj koncentratora](https://azure.microsoft.com/services/event-hubs/) je vrlo skalabilni objavljivanja-pretplate ingestor događaj. To možete prikupiti milijune događaje u sekundi.  Jedan koncentratora za događaj kao rezultat je prilikom izlaza posao strujanje analize bit će unos drugog strujanje zadatka.

Postoji nekoliko parametre koje su vam potrebne da biste konfigurirali strujanja podataka događaja koncentrator kao u izlaz.

| Naziv svojstva | Opis |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pseudonim za izlaz | Ovo je neslužbeni naziv koji se koristi u upitima za usmjeravanje izlaz upita koncentrator ovaj događaj. |
| Polje naziva Bus servisa | Prostor za naziv Bus servis je spremnik za skup poruka entiteti. Kada ste stvorili novi događaj koncentrator, i stvara servisa Bus prostor naziva |
| Koncentrator za događaj | Naziv događaja koncentrator Izlaz |
| Naziv pravila koncentrator događaja | Pravilnik zajednički pristup, koji se može stvoriti na kartici događaj koncentrator konfiguriranje. Svaki pravila zajednički pristup će imati naziv, dozvole koje ste postavili i pristupne ključeve |
| Ključ pravila koncentrator za događaj | Zajedničko korištenje programa Access ključa koji se koristi za pristup prostora za naziv Bus servis za provjeru autentičnosti |
| Stupac ključa particija [neobavezni] | Ovaj stupac sadrži particija ključ za izlaz koncentratora za događaj. |
| Događaj serijaliziranog oblika | Oblik serijalizacije za izlazne podatke.  Podržane su JSON, CSV i Avro. |
| Šifriranje | CSV i JSON, UTF-8 trenutno samo podržani oblik kodiranja |
| Graničnik | Samo primjenjivo za serijalizacije CSV. Analitički strujanje podržava nekoliko uobičajenih graničnike Serijalizacija podataka u obliku CSV. Podržani vrijednosti su zarezom sa zarezom, razmak, uvlaka i okomitu crtu. |
| Oblikovanje | Samo odgovarajuće vrste JSON. Redak odvojene određuje izlaz će se oblikovati tako da svaki objekt JSON odvojene novi redak. Polja određuje izlaz će se oblikovati kao polje JSON objekata. |

## <a name="power-bi"></a>Power BI

[Power BI](https://powerbi.microsoft.com/) može se koristiti kao na izlaz za posao strujanje Analytics za sučelje za obogaćenog vizualizacije Analiza rezultata. Tu mogućnost može se koristiti za radu nadzorne ploče, generiranje izvješća i metriku utemeljenima na izvješćivanje.

### <a name="authorize-a-power-bi-account"></a>Autorizirajte na račun servisa Power BI

1.  Power BI odabrano kao na Izlaz na portalu za upravljanje Azure zatražit će se da biste autorizirali postojećem korisniku Power BI ili da biste stvorili novi račun servisa Power BI.  

    ![Autorizirajte korisnika za Power BI](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2.  Ako vam ne još postoji, a zatim kliknite autorizirali sada, stvorite novi račun.  Zaslon kao što je sljedeća raspoređene.  

    ![Račun za Azure Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3.  U ovom ćete koraku omogućavaju računa tvrtke ili škole dopuštanja izlaz Power BI. Ako već niste prijavljeni na Power BI, odaberite prijavite se odmah. Račun tvrtke ili obrazovne ustanove koji koristite za Power BI mogu se razlikovati od Azure pretplate račun pomoću kojeg ste trenutno prijavljeni s.

### <a name="configure-the-power-bi-output-properties"></a>Konfiguriranje izlazne svojstava Power BI

Nakon što dodate račun servisa Power BI autentičnost, možete konfigurirati svojstva za izlaz Power BI. U tablici u nastavku je popis imena svojstvo i njihov opis da biste konfigurirali izlaz Power BI.

| Naziv svojstva | Opis |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pseudonim za izlaz | Ovo je neslužbeni naziv koji se koristi u upitima za usmjeravanje izlaz upita u ovom PowerBI izlaz. |
| Grupa radnog prostora | Da biste omogućili zajedničko korištenje podataka s drugim korisnicima za Power BI možete odabrati grupe unutar računa za Power BI ili ako ne želite pisati u grupu, odaberite "Moje radnog prostora".  Ažuriranje postojeću grupu potrebna obnovom provjere autentičnosti za Power BI. | 
| Naziv skupa podataka | Navedite naziv skupa podataka koji želi za izlaz Power BI za korištenje |
| Naziv tablice | Navedite naziv tablice u odjeljku dataset izlaz Power BI. Trenutno Power BI Izlaz iz strujanje Analitika poslova možete imati samo jednu tablicu u skup podataka |

Walk-through konfiguriranja Power BI izlazne i nadzorne ploče, potražite u članku [Azure strujanje analize i Power BI](stream-analytics-power-bi-dashboard.md) .

> [AZURE.NOTE] Eksplicitno stvoriti skup podataka i tablica na nadzornoj ploči Power BI. Skup podataka i tablicu će biti automatski popunjavaju kada se pokrene posao i posao pokreće pumping Izlaz u Power BI. Imajte na umu da ako posao upit ne generira rezultate, skup podataka i tablicu neće stvoriti. Također imajte na umu da ako Power BI već imate skup podataka i tablicu s istim nazivom kao navedena u ovom zadatku strujanje analize, postojeće podatke će prebrisati.

### <a name="renew-power-bi-authorization"></a>Obnavljanje autorizacije Power BI

Morat ćete ponovno autentičnost računa za Power BI ako lozinku promijenjena jer je stvoren ili zadnje autentičnost posla. Ako je višestruke provjere autentičnosti (MFA) konfiguriran na klijentu za Azure Active Directory (AAD) i morat ćete obnoviti autorizaciju Power BI svaki dva tjedna. Simptoma tog problema je bez izlaz posla i "provjere autentičnosti korisnika pogreška" u zapisnicima postupak:

  ![Power BI osvježavanja tokena pogreške](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Da biste riješili taj problem, zaustavljanje pokrenute posla, a zatim odaberite izlaz Power BI.  Kliknite vezu "Obnavljanje autorizacije" pa ponovno pokrenite posla s prekinuli posljednjeg da biste izbjegli gubitak podataka.

  ![Power BI obnoviti autorizacije](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Spremište tablica

[Spremište tablica Azure](../storage/storage-introduction.md) nudi iznimno dostupan, massively skalabilni prostor za pohranu, tako da se aplikacija može automatski promijeniti omjer da bi odgovarao zahtjev za korisnika. Spremište tablica je Microsoftov NoSQL ključ/atribut pohrane koja omogućuje korištenje strukturiranih podataka s manje ograničenja u shemi. Azure spremište tablica se može koristiti za spremanje podataka za postojanost i učinkovito dohvaćanja.

U tablici u nastavku navedeni nazivi svojstava i njihov opis za stvaranje tablice izlaz.

| Naziv svojstva | Opis |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pseudonim za izlaz | Ovo je neslužbeni naziv koji se koristi u upitima za usmjeravanje izlaz upita u ovom spremište tablica. |
| Račun za pohranu | Naziv računa za pohranu koju šaljete na izlaz. |
| Ključ za račun za pohranu | Tipkovni prečac povezanu s računom za pohranu. |
| Naziv tablice | Naziv tablice. U tablici stvorit će se ako ne postoji. |
| Ključ partition | Naziv izlazni stupac koji sadrži tipku particije. Ključ particija je jedinstveni identifikator particije unutar određene tablice koja je prvi dio je entitet primarni ključ. To je vrijednost niza koji može biti do 1 KB veličine. |
| Ključ retka | Naziv izlazni stupac koji sadrži tipku retka. Redak ključno je jedinstveni identifikator za entitet unutar zadanog particije. To čini drugi dio je entitet primarni ključ. Redak ključno je vrijednost niza koji može biti do 1 KB veličine. |
| Veličina grupe | Broj zapisa za skupne operacije. Obično zadana vrijednost je dovoljno za većinu zadataka, pogledajte [tablicu skupne operacije specifikacija](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) više pojedinosti o izmjeni tu postavku. |

## <a name="service-bus-queues"></a>Servis Bus reda čekanja

[Servis Bus redova](https://msdn.microsoft.com/library/azure/hh367516.aspx) nude na prvi u prvi Out (FIFO) isporuku poruke jedan ili više konkurentnog njezinoj. Obično, poruke se očekuje zaprimanja i obradili primatelje vremenski redoslijedom u kojem su dodane u redu čekanja, a svakoj poruci je prima i obrađuje potrošača samo jedne poruke.

U tablici u nastavku navedeni nazivi svojstava i njihov opis za stvaranje reda čekanja izlaz.

| Naziv svojstva | Opis |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pseudonim za izlaz | Ovo je neslužbeni naziv koji se koristi u upitima za usmjeravanje izlaz upita ovaj servis Bus red. |
| Polje naziva Bus servisa | Prostor za naziv Bus servis je spremnik za skup poruka entiteti. |
| Naziv reda čekanja | Naziv reda čekanja Bus servisa. |
| Naziv pravila reda čekanja | Kada stvorite red, na kartici konfiguriranje reda čekanja možete stvoriti i zajednički pristup pravila. Svaki pravila zajednički pristup će imati naziv dozvole koje ste postavili i pristupne ključeve. |
| Red čekanja pravila ključ | Zajedničko korištenje programa Access ključa koji se koristi za pristup prostora za naziv Bus servis za provjeru autentičnosti |
| Događaj serijaliziranog oblika | Oblik serijalizacije za izlazne podatke.  Podržane su JSON, CSV i Avro. |
| Šifriranje | CSV i JSON, UTF-8 trenutno samo podržani oblik kodiranja |
| Graničnik | Samo primjenjivo za serijalizacije CSV. Analitički strujanje podržava nekoliko uobičajenih graničnike Serijalizacija podataka u obliku CSV. Podržani vrijednosti su zarezom sa zarezom, razmak, uvlaka i okomitu crtu. |
| Oblikovanje | Samo odgovarajuće vrste JSON. Redak odvojene određuje izlaz će se oblikovati tako da svaki objekt JSON odvojene novi redak. Polja određuje izlaz će se oblikovati kao polje JSON objekata. |

## <a name="service-bus-topics"></a>Servis Bus teme

Dok je servis Bus redova omogućuje način komunikacije jedan na jedan od pošiljatelja tekstnog okvira, [Servis Bus teme](https://msdn.microsoft.com/library/azure/hh367516.aspx) sadrže obrazac jedan-prema-više komunikacije.

U tablici u nastavku navedeni nazivi svojstava i njihov opis za stvaranje tablice izlaz.

| Naziv svojstva | Opis |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pseudonim za izlaz | Ovo je neslužbeni naziv koji se koristi u upitima za usmjeravanje izlaz upita u ovoj se temi Bus servisa. |
| Polje naziva Bus servisa | Prostor za naziv Bus servis je spremnik za skup poruka entiteti. Kada ste stvorili novi događaj koncentrator, i stvara servisa Bus prostor naziva |
| Naziv teme | Tema se poruka entiteti, slično koncentratora za događaj i redova. Osmišljene su prikupljanje tokove događaj iz nekoliko različitih uređaja i usluga. Pri stvaranju temu ga je zadan naziv. Poruke poslane na temu neće biti dostupne ako pretplatu je stvorili, pa provjerite postoji jedan ili više pretplate u odjeljku teme |
| Naziv pravila teme | Kada stvorite temu, možete stvoriti i zajednički pristup pravila na kartici konfiguriranje temu. Svaki pravila zajednički pristup će imati naziv, dozvole koje ste postavili i pristupne ključeve |
| Ključ pravilnik o temi | Zajedničko korištenje programa Access ključa koji se koristi za pristup prostora za naziv Bus servis za provjeru autentičnosti |
| Događaj serijaliziranog oblika | Oblik serijalizacije za izlazne podatke.  Podržane su JSON, CSV i Avro. |
| Šifriranje | Ako je oblik CSV ili JSON, kodiranja mora biti navedena. UTF-8 trenutno samo podržani oblik kodiranja |
| Graničnik | Samo primjenjivo za serijalizacije CSV. Analitički strujanje podržava nekoliko uobičajenih graničnike Serijalizacija podataka u obliku CSV. Podržani vrijednosti su zarezom sa zarezom, razmak, uvlaka i okomitu crtu. |

## <a name="documentdb"></a>DocumentDB

[Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) je potpuno upravlja NoSQL dokument baze podataka usluga koje nudi upita i transakcije putem sheme slobodno podataka, predvidljivi i pouzdane performanse i brz razvoj.

U tablici u nastavku navedeni nazivi svojstava i njihov opis za stvaranje DocumentDB izlaz.

<table>
<tbody>
<tr>
<td>NAZIV SVOJSTVA</td>
<td>OPIS</td>
</tr>
<tr>
<td>Naziv računa</td>
<td>Naziv računa DocumentDB.  To može biti krajnja točka za račun.</td>
</tr>
<tr>
<td>Ključ za račun</td>
<td>Zajednički pristup ključ za račun DocumentDB.</td>
</tr>
<tr>
<td>Baze podataka</td>
<td>Naziv baze podataka DocumentDB.</td>
</tr>
<tr>
<td>Uzorak naziv zbirke</td>
<td>Uzorak naziv zbirke za zbirke koja će se koristiti. Oblik naziva zbirke možete konstruirana pomoću token neobavezno {particija} gdje će se particije Počni od 0.<BR>Npr. Na followings su valjanih unosa:<BR>MyCollection {particija}<BR>MyCollection<BR>Imajte na umu da zbirke mora postojati strujanje analize posao pokreće i ne stvorit će se automatski.</td>
</tr>
<tr>
<td>Ključ partition</td>
<td>Naziv polja u stavkama događaja izlaz koristi se za određivanje ključ za particija izlazna preko zbirke.</td>
</tr>
<tr>
<td>ID-a dokumenta</td>
<td>Naziv polja u izlaz događaje koje umetanje ili ažuriranje operacije koristi se za određivanje primarnog ključa temelje se na.</td>
</tr>
</tbody>
</table>


## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci
Koje ste je uvedena strujanje analize, servis za upravljane tok analize podataka s Interneta stvari. Da biste saznali više o taj servis, pogledajte:

- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
