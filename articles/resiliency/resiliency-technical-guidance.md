<properties
   pageTitle="Indeks Tehnički vodič za otpornost | Microsoft Azure"
   description="Indeks tehničkih članaka na razumijevanje i dizajniranje prebacuju, Visoko dostupna, pogreške aplikacije, kao i planiranje Izrada oporavak i tvrtke continuity"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance"></a>Tehnički vodič Azure otpornost

##<a name="introduction"></a>Uvod

Dvije vrste znanja visoke dostupnosti i Izrada oporavak zahtjeve za sastanak potrebno:

- Detaljne tehničke razumijevanje mogućnosti platformu oblaka
- Znanje o tome kako pravilno mijenjanje arhitekture raspodijeljeno servisa

Ovaj niz članke pokriva na bivši: mogućnosti i ograničenja platforme Azure vezana uz otpornost (ponekad se zove i poslovanje). Ako vas zanima drugu mogućnost, pogledajte članak niz filtriran na [oporavak Izrada i visoke dostupnosti za Azure aplikacije](https://aka.ms/drtechguide). Iako ovaj članak niz dodiruje uzoraka arhitektura i dizajna, koji nije fokus niz. Upute za dizajn, možete pokušati pronaći materijala u odjeljku [Dodatne resurse](#additional-resources) .

Podaci organizirani u sljedećim člancima:

- [Oporavak iz lokalne pogreške](resiliency-technical-guidance-recovery-local-failures.md).
Fizički hardverski (Ako, na primjer, pogona, poslužiteljima i mrežne uređaje) možete neće uspjeti. Resursi mogu biti iscrpljen prilikom učitavanja krivina. U ovom se članku opisuju mogućnosti koje Azure nudi da biste zadržali visoke dostupnosti ispunjavate sljedeće uvjete.

- [Oporavak iz programa prekidu Azure regija razini usluge](resiliency-technical-guidance-recovery-loss-azure-region.md).
Primamo neuspjeha se rijetko, ali su theoretically moguće. Cijeli područjima može postati Izolirani zbog pogrešaka u mreži ili ih možete fizički oštećen s prirodnim disasters. U ovom se članku objašnjava kako pomoću Azure da biste stvorili aplikacije koje se protežu na geografski raznih područja.

- [Oporavak lokalnim za Azure](resiliency-technical-guidance-recovery-on-premises-azure.md).
Oblak znatno dizajnirao economics oporavak Izrada Omogućivanje tvrtke ili ustanove da biste koristili Azure uspostaviti drugog web-mjesta za oporavak. To možete učiniti na razlomak troška Izrada i održavanje sekundarne podatkovnog centra. U ovom se članku objašnjava mogućnosti koje Azure nudi za proširivanje lokalnog podatkovnog centra u oblak.

- [Oporavak od oštećenja podataka ili nenamjerno brisanje](resiliency-technical-guidance-recovery-data-corruption.md).
Aplikacija može imati programskih pogrešaka oštećena podatke. Operatori možete izbrisati pogrešno važnih podataka. U ovom se članku objašnjava što Azure nudi za sigurnosno kopiranje podataka i vraćanje na prethodnu točku vremena.

##<a name="additional-resources"></a>Dodatni resursi

- [Izrada oporavak i visoke dostupnosti za aplikacije koje se temelji na Microsoft Azure](resiliency-disaster-recovery-high-availability-azure-applications.md).
Ovaj se članak odnosi detaljne pregled dostupnosti i Izrada oporavak. Pokriva test ručno replikacije za pregled i transakcijskih podataka. Konačni odjeljcima sažetaka različite vrste topologija oporavak Izrada koji obuhvaćaju Azure područja za najvišu razinu dostupnost.

- [Kontrolni popis za visoku dostupnost](resiliency-high-availability-checklist.md).
Ovaj se članak odnosi na popis značajki, servise i dizajne koji vam mogu pomoći povećati stabilnosti i dostupnost aplikacije.

- [Upute za otpornost servisa Microsoft Azure](resiliency-service-guidance-index.md).
U ovom se članku je indeks servisa Azure i navode veze na upute za oporavak Izrada i upute za dizajn.

- [Pregled: u Oblaku tvrtke continuity i baza podataka Izrada oporavak s bazom podataka SQL](../sql-database/sql-database-business-continuity.md).
Ovaj članak sadrži baze podataka SQL Azure tehnike dostupnost. Primarno se pogoni na strategije sigurnosnog kopiranja i vraćanja. Ako koristite baze podataka SQL Azure u servis u oblaku, trebali biste pregledajte ovog članka i njegova povezani resursi.

- [Visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim strojevima sa sustavom Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).
U ovom se članku govori o dostupnosti mogućnosti koje možete istražiti kada koristite infrastrukture kao service (IaaS) za hostiranje servisa baze podataka. Govori o grupe dostupnosti AlwaysOn, baze podataka, zapisnika dostavu i sigurnosnog kopiranja i vraćanja. Nekoliko vodiče pokazati kako koristiti sljedeće postupke.

- [Najbolje prakse za dizajniranje veliki usluge na servise u Oblaku Azure](https://azure.microsoft.com//blog/best-practices-for-designing-large-scale-services-on-windows-azure/).
U ovom se članku usredotočuje se na razvoj arhitekturi iznimno skalabilni oblaka. Mnoge tehnika kojima se koriste za poboljšanje skalabilnost poboljšati i dostupnost. Osim toga, ako aplikacija nije moguće skaliranje povećana opterećenju, skalabilnost postaje dostupnost problem.

- [Sigurnosno kopiranje i vraćanje za SQL Server na virtualnim strojevima sa sustavom Azure](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
Ovaj članak sadrži Tehnički vodič za sigurnosno kopiranje i vraćanje Microsoft SQL Server pokrenut na virtualnim strojevima sa sustavom Azure.

- [Failsafe: smjernicama prebacuju oblaka arhitekturi](https://channel9.msdn.com/Series/FailSafe).
Ovaj članak sadrži upute za stvaranje arhitekturi prebacuju oblaka vodič za implementaciju te arhitekturi na Microsoftove tehnologije i recepti za implementaciju ove arhitekturi određene scenarijima za.

- [Tehnička Studije slučaja: pomoću tehnologije oblaka da biste poboljšali oporavak Izrada](https://www.microsoft.com/itshowcase/Article/Content/737/Using-cloud-technologies-to-improve-disaster-recovery).
Ovog slučaja prikazuje način na koji Microsoft IT koristi Azure da biste poboljšali Izrada oporavak.

##<a name="next-steps"></a>Daljnji koraci

Ovaj je članak dio niza filtriran na Tehnički vodič za Azure otpornost. Ako vas zanimaju druge članke u ovoj seriji za čitanje, možete početi s [oporavak iz lokalne pogreške](resiliency-technical-guidance-recovery-local-failures.md).
