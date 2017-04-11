<properties
   pageTitle="Izvlačenja podataka SQL u Azure događaj koncentratora | Microsoft Azure"
   description="Pregled događaja koncentratora uvoz iz uzorka SQL"
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor=""/>

<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="pulling-data-from-sql-into-an-azure-event-hub"></a>Izvlačenja podataka iz SQL u koncentrator za događaj programa Azure

Uobičajeni Arhitektura programa za obradu podataka u stvarnom vremenu uključuje najprije je margina programa Azure koncentrator za događaj. Možda je scenariju IoT, kao što su praćenje promet na različitim Rasteže highway, ili scenarij igraće, nadzor akcije na horde frenzied contestants ili enterprise scenarij, kao što su praćenje sastavnih occupancy. U tim slučajevima proizvođača podataka su obično vanjski objekata proizvodnje vremenski niz podataka koje morate prikupiti Analitika, spremanje i zakon o tome, a možda ste uložiti mnogo truda u sastavnih out infrastrukture ovih procesa. Što vam ne želite li prikupiti podatke iz nešto kao što su baze podataka umjesto izvor strujanja podataka i koristite u kombinaciji s drugim strujanja podataka? U ovom slučaju mjesto na koje želite koristiti Azure strujanje analize, udaljene Explorer podataka (RDX) ili neki drugi alat za analizu i djelovanje na sporo promjene podataka iz Microsoft Dynamics AX ili prilagođene funkcije program za upravljanje sustava u obzir? Da biste riješili taj problem, ne možemo ste napisali i Otvori-izvorni termini small oblaka uzorak koji možete izmijeniti i implementirati koji će izvlačenje podataka iz tablice SQL i automatske programa Azure događaj koncentrator da biste koristili kao ulaz u do analytical aplikacija. Shvatite da se ova rijetko scenarij i suprotan što obično učiniti s koncentratora za događaj. Međutim, Utvrdite li sami u slučaju gdje je što vam je potrebno učiniti, možete pronaći kod u Azure uzoraka galeriji [ovdje](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-import-from-sql/).  

Imajte na umu da je kod u ovom primjeru upravo to, uzorka. To je **nije** namijenjen da se u aplikaciju radnog i bez pokušaja uvedena da biste prikladna za upotrebu u takvim okruženju – je stricly DIY, za razvojne inženjere odnosila, primjer. Morate pregledati razne sigurnost, performanse i funkcionalnost i troškova čimbenika prije podmirenje na arhitekturi do kraja do kraja.

## <a name="application-structure"></a>Strukturu aplikacije

Aplikacija napisan C# i readme.md datoteka u uzorku sadrži sve potrebne informacije za izmjenu, stvaranje i objavljivanje aplikacija. Sljedeći odjeljci sadrže više razine pregled funkcija aplikacije.

Ne možemo počinju pretpostavci kojima imate pristup tablice SQL Azure. Također morat ćete ste postavili programa Azure koncentrator za događaj i znate veza niz potrebne za pristup.

Kada je rješenje SqlToEventHub pokrene, čita konfiguracijska datoteka (App.config) da biste dobili broj značajki, kao što je vidljivo u datoteci readme.md. To su vrlo self-explanatory, kao što su naziv podatkovne tablice i itd i nema potrebe za rehash objašnjenja ovdje. 

Kada aplikacija pročitao konfiguracijskoj datoteci, ga dolazi u petlji, čitanje tablica sustava SQL i margina zapisa koncentrator događaja, a zatim čekanje interval korisnički definirane mirovanja prije prelaska na se sve ponovno. Što su sudjelovanje:

1. Aplikacija se temelje pretpostavci tablica sustava SQL ažurira neke vanjske proces, a želite poslati svim i samo ažuriranja koncentratora za događaj.
2. Tablica SQL mora imati polja koje sadrži jedinstven i povećavati broj, na primjer, numer zapis. To može biti jednostavno kao polje pod nazivom "Id" ili nešto drugo koji se povećava kao ono ažurira tu bazu podataka dodaje zapise kao što su "Creation_time" ili "Sequence_number". Aplikacija bilješki i pohranjuje vrijednost tog polja u iteracija. U svakom kasnije prolazni petlje aplikacije zapravo upiti tablice za sve zapise gdje vrijednost tog polja premašuje vrijednost prikazivalo zadnji put do petlje. Ne možemo zovete ovaj posljednja vrijednost "pomaka".
3. Aplikacija se stvaraju tablice "TableOffsets" prilikom pokretanja prema gore za pohranu na pomake. Tablice stvara se pomoću upit "CreateOffsetTableQuery" definiran u konfiguracijskoj datoteci. 
4. Postoji nekoliko upiti koji se koriste za rad s offset tablicu, definirana u konfiguracijskoj datoteci kao "OffsetQuery", "UpdateOffsetQuery" i "InsertOffsetQuery". Mijenjati te.
5. Na kraju, upit "DataQuery" definiran u konfiguracijskoj datoteci je upit koji će se izvoditi da biste izvukli zapise iz tablice SQL. Trenutno ograničen je prvih 1000 zapisima u svaki prolazni petlje radi optimizacije – ako, na primjer, dodati 25,000 zapisa u bazu podataka nakon zadnjeg upita, može proći neko vrijeme za izvršavanje upita. Ograničavanje upita 1000 zapisa svaki put, upiti su ubrzava. Odaberite prvih 1000 jednostavno ih gura uzastopni serijama od 1000 zapisa koncentrator za događaj.    

## <a name="next-steps"></a>Daljnji koraci

Da biste implementirali rješenje, Kloniraj ili preuzeti aplikaciju SqlToEventHub, uredite datoteku App.config, je i na kraju i objavljivanje. Nakon što ste objavili aplikaciju, možete vidjeti je pokrenut na portalu Azure klasični u odjeljku servise u Oblaku i praćenje događaja dolaze na koncentratora za događaj. Imajte na umu da učestalost biti određen dvije stvari: učestalost ažuriranja sustava SQL tablice te interval mirovanja koje ste naveli u konfiguracijskoj datoteci aplikacije.