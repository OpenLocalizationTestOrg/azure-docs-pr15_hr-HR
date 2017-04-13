<properties 
   pageTitle="Testiranje performanse servis u oblaku | Microsoft Azure"
   description="Testirajte performanse neki servis u oblaku pomoću profiler Visual Studio"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="testing-the-performance-of-a-cloud-service"></a>Testiranje performanse servis u oblaku 

##<a name="overview"></a>Pregled

Možete testirati performanse servis u oblaku na sljedeće načine:

- Koristite Azure Dijagnostika za prikupljanje informacija o zahtjeve i veze te da biste pregledali statistiku web-mjesta koji pokazuju kako se servis izvodi iz perspektive klijenta. Da biste započeli, potražite u članku [Konfiguriranje Dijagnostika za servise u Oblaku Azure i virtualnih računala]( http://go.microsoft.com/fwlink/p/?LinkId=623009).

- Da biste dobili detaljnije analizu računalne aspekte načinom servis pomoću profiler Visual Studio. Kao u ovoj se temi opisuju, možete upotrijebiti u profiler za mjerenje performanse, kao što je servis pokreće se u Azure. Informacije o tome kako koristiti u profiler za mjerenje performanse, kao što je servis izvodi lokalno računalnim emulator potražite u članku [testiranje performanse programa Azure oblaka servis lokalno u izračunati Emulator pomoću Visual Studio Profiler](http://go.microsoft.com/fwlink/p/?LinkId=262845).



## <a name="choosing-a-performance-testing-method"></a>Odabir performanse testiranje način

###<a name="use-azure-diagnostics-to-collect"></a>Da biste prikupili pomoću Azure dijagnostiku:###

- Statistika na web-stranice ili servisa, kao što su zahtjeve i veze.

- Statistike o ulogama, kao što su učestalost ponovnog pokretanja uloge.

- Informacije o korištenju memorije, kao što je postotak vremena koja vas vodi prikupljanje smeća ili memorije ukupni postavite izvodi uloge.

###<a name="use-the-visual-studio-profiler-to"></a>Koristite Visual Studio profiler da biste:###

- Odredite koja funkcionira najbolje potrajati.

- Izmjerite koliko je vremena potrebno svaki dio što intenzivno program.

- Usporedba izvješća detalja o performansama za dvije verzije servisa.

- Analiza dodjelu memorije detaljno od razinu alokacija za pojedinačne memorije.

- Analiza istodobnosti probleme s usporednim nitima kod.

Kada koristite na profiler, možete prikupiti podatke kada servis u oblaku izvodi lokalno ili Azure.

###<a name="collect-profiling-data-locally-to"></a>Prikupljanje profiliranja podataka lokalno:###

- Testirajte performanse dio servise u oblaku, kao što su izvođenja određenih radnih uloge koje ne zahtijevaju realističan Simulirani Učitaj.

- Testirajte performanse servis u oblaku u odvajanja nadziranim uvjetima.

- Prije njegove implementacije kod Azure, testirajte performanse servis u oblaku.

- Privatno, testirajte performanse servis u oblaku kojima to neće oštetiti postojeće implementacije.

- Testirajte performanse servisa bez povećavanja naknade za pokretanje u Azure.

###<a name="collect-profiling-data-in-azure-to"></a>Prikupljanje profiliranja podataka u Azure da biste:###

- Testirajte performanse neki servis u oblaku opterećenju Simulirani ili realni.

- Upotrijebite metodu instrumentation prikupljanje profiliranja podataka, kao što je u ovoj se temi opisuju kasnije.

- Testirajte performanse servisa u istom okruženju kao kada servis pokreće se u radnog.

Obično zamjenu opterećenja servisa u oblaku testa pod normalno ili opterećenjem uvjeta.

## <a name="profiling-a-cloud-service-in-azure"></a>Profiliranje neki servis u oblaku u Azure

Kad objavite oblaku iz Visual Studio, možete profila usluge i odredite profiliranja postavke koje će vam informacije koje želite. Za svaku instancu uloge se pokrene profiliranja sesija. Dodatne informacije o tome da biste objavili svoje servisa Visual Studio potražite u članku [Objavljivanje sa servisom Cloud Azure s Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx).

Bolje performanse Profiliranje u Visual Studio shvatili, potražite u članku [Vodič za početnike za Profiliranje performanse](https://msdn.microsoft.com/library/azure/ms182372.aspx) i [Analiza uspješnosti aplikacije pomoću alata za Profiliranje](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

>[AZURE.NOTE] Možete omogućiti IntelliTrace ili Profiliranje Kad objavite servis u oblaku. Ne možete omogućiti i jedno i drugo.

###<a name="profiler-collection-methods"></a>Načini zbirke profiler

Različite zbirke metode možete koristiti za Profiliranje, koji se temelji na vašem probleme s performansama:

- **Uzorkovanje CPU** - Ova metoda prikuplja statistike aplikacije koje su korisne za početne analizu CPU Upotreba problema. Uzorkovanje procesora je predložene način za pokretanje Većina istrage performansi. Postoji niskog utjecaj na aplikaciju koja su Profiliranje kada prikupljanje podataka za stvaranje uzoraka procesora.

- **Instrumentation** -Ova metoda prikuplja podatke detaljnog tempiranja koja je korisna kojoj je žarište analize i analize probleme s performansama ulaza i izlaza. Način instrumentation bilježi svake stavke, Izlaz i poziv funkcije funkcija modula tijekom na Profiliranje pokrenuti. Ta je metoda korisne za prikupljanje tempiranja detaljne informacije o sekcije koda i objašnjenje utjecaja ulazni i izlazni operacija na performanse računala. Ta metoda nije omogućen za računala sa sustavom 32-bitni operacijski sustav. Ta je mogućnost dostupna samo prilikom pokretanja servisa u oblaku u Azure, ne lokalno u emulator računalnim.

- **Dodjelu memorije .NET** - Ova metoda prikuplja podatke za dodjelu memorije .NET Framework pomoću uzorkovanja Profiliranje način. Prikupljene podatke sadrži broj i veličinu dodijeljene objekata.

- **Istodobnosti** - Ova metoda prikuplja resursa Nadmetanje podataka i postupak i niti izvođenja podataka koje su korisne analiza Usporedni i više procesa aplikacijama. Način istodobnosti prikuplja podatke za svaki događaj blokovi izvođenja koda, kao što su kada niti čeka zaključan je pristup aplikaciji resurs koji će se osloboditi. Ova metoda je korisna za analizu Usporedni aplikacije.

- Možete omogućiti i **Sloju interakcije Profiliranje**, koji pruža dodatne informacije o vremena izvođenja sinkrono ADO.NET poziva u funkcijama programa višestruki tiered komunikaciju s jednog ili više baza podataka. Možete prikupiti podataka sloju interakcije s bilo kojom od profiliranja načina. Dodatne informacije o sloju interakcije Profiliranje potražite u članku [Prikaz interakcije sloju](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Konfiguriranje postavki profiliranja

Sljedeća ilustracija prikazuje kako konfigurirati profiliranja postavke u dijaloškom okviru objavljivanje Azure aplikacije.

![Konfiguriranje Profiliranje postavke](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

>[AZURE.NOTE] Da biste omogućili potvrdni okvir **Omogući Profiliranje** , morate imati profiler instalirana na lokalnom računalu koju koristite za objavljivanje na servis u oblaku. Po zadanom se profiler instalira se prilikom instaliranja Visual Studio.

### <a name="to-configure-profiling-settings"></a>Da biste konfigurirali postavke profiliranja

1. U pregledniku rješenja, otvorite izbornik prečaca za Azure projekta, a zatim **Objavi**. Detaljne upute o tome da biste objavili servis u oblaku, potražite u članku [Objavljivanje prikazom oblaka za servis pomoću alata za Azure](http://go.microsoft.com/fwlink/p?LinkId=623012).

1. U dijaloškom okviru **Objavljivanje aplikacija Azure** odaberete karticu **Dodatne postavke** .

1. Da biste omogućili Profiliranje, potvrdite okvir **Omogući Profiliranje** .

1. Da biste konfigurirali postavke profiliranja, odaberite **Postavke** hiperveze. Pojavit će se dijaloški okvir Postavke Profiliranje.

1. Iz gumba **Profiliranje koji način želite li koristiti** mogućnosti odaberite vrstu Profiliranje koje su vam potrebne.

1. Da biste prikupili podatke profiliranja sloju interakcije, potvrdite okvir **Omogući Profiliranje interakcije sloju** .

1. Da biste spremili postavke, odaberite gumb **u redu** .

    Kad objavite ovu aplikaciju, te postavke se koriste za stvaranje profiliranja sesije za svaku ulogu.

## <a name="viewing-profiling-reports"></a>Prikaz Profiliranje izvješća

Sesije profiliranja se stvara za svaku instancu uloga u servis u oblaku. Da biste pogledali profiliranja izvješćima svaki sesije s Visual Studio, možete prikaz prozora preglednika poslužitelja, a zatim odaberite čvor Azure izračunati da biste odabrali instance komponente uloge. Zatim možete pogledati profiliranja izvješće kao što je prikazano na sljedećoj slici.

![Prikaz izvješća o profiliranja s Azure](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Da biste pogledali profiliranja izvješća

1. Da biste prikazali prozor preglednika poslužitelja u Visual Studio, na traci izbornika odaberite prikaz, Explorer poslužitelja.

1. Odaberite čvor Azure izračunati, a zatim odaberite čvor Azure implementaciji servisa za oblak koji ste odabrali profil prilikom objavio Visual Studio.

1. Da biste pogledali profiliranja izvješća za instancu, odaberite ulogu u servisu, otvaranje izbornika prečaca za konkretnu instancu, a zatim **Prikaz izvješća o Profiliranje**.

    Izvješće, datoteku .vsp odmah preuzeli s Azure i Azure zapisnik aktivnosti prikazuje status preuzimanja. Kada preuzimanje završi, profiliranja izvješće prikazat će se na kartici u uređivaču za Visual Studio pod nazivom <Role name> _<Instance Number>_ <identifier>.vsp. Pojavit će se sažeti podaci za izvješće.

1. Da biste prikazali različite poglede izvješće, na popisu trenutni prikaz, odaberite vrstu prikaza koji želite. Dodatne informacije potražite u članku [Profiliranje prikazi Alati za izvješća](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Daljnji koraci

[Ispravljanje pogrešaka servise u Oblaku](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Objavljivanje u Azure Oblaku iz Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

