<properties
   pageTitle="Kako se povezati s izvorima podataka | Microsoft Azure"
   description="Upute u članku se isticanje kako se povezati s izvorima podataka otkrio pomoću kataloga podataka Azure."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>


# <a name="how-to-connect-to-data-sources"></a>Kako se povezati s izvorima podataka

## <a name="introduction"></a>Uvod
**Katalog podataka za Microsoft Azure** je servis potpuno upravljanih oblaka koja služi kao sustav Registracija i sustava otkrivanje za izvore podataka tvrtke. Drugim riječima, **Katalog podataka Azure** je pomoć osobe otkrivanje, razumijevanje i korištenje izvora podataka i tvrtkama ili ustanovama pomoć da biste dobili dodatne vrijednost iz svoje postojeće podatke. Ključni aspekt scenarij se radi o podacima – kada korisnik otkrije izvor podataka, a možete koristiti njezinu svrhu, sljedeći je korak povezati s izvorom podataka da biste stavili njegove podatke koje želite koristiti.

## <a name="data-source-locations"></a>Mjesta izvora podataka
Tijekom registracije za izvor podataka, **Katalog podataka Azure** prima metapodataka o izvoru podataka. U ovom metapodataka sadrži detalje za mjesto izvora podataka. Detalje o mjesto ovise iz izvora podataka za izvor podataka, ali uvijek sadrže podatke koji su potrebni za povezivanje. Ako, na primjer, mjesto za tablicom sustava SQL Server sadrži naziv poslužitelja, naziv baze podataka, naziv sheme i naziv tablice, dok je mjesto za SQL Server Reporting Services izvješće sadrži naziv poslužitelja i put do izvješća. Druge vrste izvora podataka neće imati mjesta koja odražava strukture i mogućnosti izvorišnog sustava.

## <a name="integrated-client-tools"></a>Integrirano klijentskih alata
Najjednostavniji način za povezivanje s izvorom podataka je za korištenje na "otvorena u..." izbornik na portalu za **Azure kataloga podataka** . Izbornik prikazuje popis mogućnosti za povezivanje s resursa odabranih podataka.
Kada koristite zadani prikaz pločica, izbornik dostupna je na svaku pločicu.

 ![Otvaranje tablice sustava SQL Server u programu Excel iz resursa pločicu podataka](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Prilikom korištenja prikaza popisa, na izborniku dostupan je u traka za pretraživanje pri vrhu prozora portala.

 ![Otvaranje izvješća SQL Server Reporting Services u Report Manager s trake za pretraživanje](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Podržane klijentske aplikacije
Kada se koristi u "otvorena u..." izbornik za izvore podataka na portalu katalog podataka Azure točan klijentska aplikacija mora biti instaliran na klijentskom računalu.

| Otvori u aplikaciji | Datotečni nastavak / protokol | Podržane aplikacije verzije |
| --- | --- | --- |
| Excel | .odc | Excel 2010 ili noviji |
| Excel (prvih 1000) | .odc | Excel 2010 ili noviji |
| Power Query | .xlsx | Instalirani Excel 2016 ili Excel 2010 ili Excel 2013 Power Query za dodatka programa Excel
| Power BI Desktop | .pbix | Power BI Desktop srpanj 2016 ili noviji |
| SQL Server Data Tools | vsweb: / / | Ažuriranje Visual Studio 2013 4 ili noviji sa SQL Server tooling instaliran |
| Upravitelj izvještaja | http:// | Potražite u članku [preduvjeti preglednika za SQL Server Reporting Services](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Podatke, vaše Alati
Mogućnosti dostupne na izborniku će ovise o vrsti podataka imovine odabran. Naravno, sve moguće Alati će biti obuhvaćene na "otvorena u..." izbornik podaci, ali je i dalje jednostavno povezati s izvorom podataka pomoću alata za bilo koji klijent. Podaci resursa odabrana na portalu za **Katalog podataka Azure** dovršeno lokacija prikazuje se u oknu svojstva.

 ![Informacije o vezi za tablicom sustava SQL Server](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

Informacije o pojedinosti veze će se razlikovati od vrsta izvora podataka za vrstu izvora podataka, ali upute koje ste dobili na portalu steći ćete sve što vam treba za povezivanje s izvorom podataka u alatu za bilo koji klijent. Korisnici mogu kopirati pojedinosti veze za izvor podataka koji su otkrio **Azure katalog podataka**, što im omogućava rad s podacima u svojim alat za odabir.

## <a name="connecting-and-data-source-permissions"></a>Povezivanje i podataka iz izvora dozvole
Iako **Azure katalog podataka** omogućuje izvore podataka vidljiv, pristup podacima sam će ostati u odjeljku kontrolu nad podacima izvora vlasnik ili administrator. Otkrivanje izvora podataka u **Katalog podataka Azure** ne daje korisnik sve dozvole za pristup izvoru podataka sam.

Radi lakšeg za korisnike koji otkrivanje izvora podataka, ali imaju dozvolu pristupa njegove podatke, korisnici unijeti podatke u svojstvo zahtjeva za pristupom kada opaske izvora podataka. Podaci koji su navedene – uključujući veze na procesa ili posrednika za pristup izvoru podataka – prikazuju se uz podatke o mjestu izvora na portalu.

 ![Informacije o povezivanju s zahtjeva za pristup upute](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

##<a name="summary"></a>Sažetak
Registracija izvora podataka pomoću **Kataloga podataka Azure** zahvaljujući tih podataka vidljivim kopirate strukturnih i opisni metapodatke iz izvora podataka u servis kataloga. Kada izvora podataka je registrirana, i otkrivanja, korisnici mogu povezati s izvorom podataka s portala za **Katalog podataka Azure** "otvorena u..." " izbornik ili alatima za svoje podatke po izboru.

## <a name="see-also"></a>Vidi također
- [Početak rada s katalog podataka Azure](data-catalog-get-started.md) Praktični vodič detaljne informacije o tome kako se povezati s izvorima podataka.
