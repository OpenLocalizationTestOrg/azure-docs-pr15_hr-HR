<properties
   pageTitle="Izrada oporavak i visoke dostupnosti za Azure aplikacije | Microsoft Azure"
   description="Tehnički pregled i dubinu informacije o dizajniranju aplikacije za oporavak visoke dostupnosti i Izrada aplikacija utemeljena na Microsoft Azure."
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

#<a name="disaster-recovery-and-high-availability-for-applications-built-on-microsoft-azure"></a>Izrada oporavak i visoke dostupnosti za aplikacije koje se temelji na Microsoft Azure

##<a name="introduction"></a>Uvod

U ovom članku fokus je na visoke dostupnosti za aplikacije koji se izvodi u Azure. Cjelokupan Strategije za visoke dostupnosti obuhvaća i aspekt Izrada oporavak. Planiranje neuspješne i disasters u oblaku morate brzo prepoznavanje na pogreške. Zatim implementirati strategije koji odgovara vašem odstupanje za nedostupnost aplikacije. Uz to, imate uzmite u obzir opseg gubitka podataka aplikacije možete tolerate bez uzrokuje š tvrtke rezultate kao što se Obnovi.

Većina tvrtki da su se pripremiti za privremene i veliki pogreške. Međutim, prije odgovorili to pitanje za sebe, ne tvrtke Proba te pogreške? Testirate oporavak baze podataka da biste bili sigurni da imate ispravne procese na mjestu? Vjerojatno ne. To je zato uspješno Izrada oporavak započinje s mnogo planiranja i architecting implementaciju ti procesi. Baš kao i mnoge druge koje nisu funkcionalno preduvjeti, kao što su sigurnost, Izrada oporavak rijetko dohvaća unaprijed analize i vrijeme alokacija bit će potrebno. Osim toga, većina proizvođača nemate proračuna za geografski raspodijeljeno regije suvišnih kapaciteta. Zbog toga ključnih aplikacije čak i zaštita njihove privatnosti ovise često Izuzeto iz proper Izrada oporavak planiranja.

Oblak platforme, kao što su Azure, navedite geografski raspršenim područja svijeta. Ove platforme omogućuje i mogućnosti koje podržavaju dostupnosti i različite Izrada oporavak scenarije. Sada možete dobiti svaka aplikacija ključnih oblaka zaštita njihove privatnosti ovise krajnji razmatranje za provjeru Izrada sustava. Azure ima stabilnosti i oporavak Izrada ugrađen u mnogim njegove servise. Pažljivo te značajke platforme za učenje i dodatku izjavi s strategije aplikacije.

U ovom se članku strukture arhitektura potrebne korake koje morate poduzeti da biste Izrada dokaz Azure implementacije. Zatim možete implementirati većeg poslovnog procesa continuity. Poslovni plan continuity je vodič za nastavak operacije š uvjetima. To može biti pogreška tehnologijom, kao što su downed servisa ili prirodnim Izrada, kao što su oluja ili power prekida. Otpornost aplikacije za disasters je samo podskup veće Izrada postupka oporavka, kao što je opisano u ovom dokumentu NIST: [Contingency vodič za planiranje za informacijske tehnologije sustave](https://www.fismacenter.com/sp800-34.pdf).

U sljedećim se odjeljcima definirati različite razine pogrešaka, tehnike baviti ih i arhitekturi koji podržavaju ove tehnike. Ove informacije unese procesa oporavak Izrada i postupaka, da biste bili sigurni strategije Izrada oporavak radi pravilno i učinkovito.

##<a name="characteristics-of-resilient-cloud-applications"></a>Značajke aplikacije prebacuju oblaka

Dobro architected aplikaciju možete withstand mogućnost neuspjeha tactical razine, a možete i tolerate strateški sistemskih pogreške na razini regija. U sljedećim se odjeljcima definirati terminologija koji se koristi u cijelom dokumentu na koje želite opisuju razne aspekte servise u oblaku prebacuju.

###<a name="high-availability"></a>Visoke dostupnosti

Iznimno dostupna oblaka aplikacije implementira Strategije za apsorbira nedostupnosti ovisnosti, kao što su upravljani usluga koje se nude platforma oblaka. Bez obzira na moguće neuspjeha platformu oblaka mogućnosti takvog omogućuje aplikacije da biste nastavili nastave očekivani funkcionirati i koje nisu funkcionalnim systemic karakteristike. To opisana detaljnije u nizu videozapisa kanala 9 [Failsafe: smjernicama prebacuju oblaka arhitekturi](https://channel9.msdn.com/Series/FailSafe).

Kada implementirate aplikaciju, morate uzeti u obzir vjerojatnost mogućnost prekida. Uz to, razmislite o utjecaja programa nedostupnosti će se nalaziti na aplikaciju iz perspektive tvrtke prije čitanje duboke Strategije implementacije. Bez krajnji obzir utjecaj tvrtke i vjerojatnost odlazak uvjet rizika implementacije može biti skupi i potencijalno nepotrebne.

Razmislite o Samopogonjeni analogy za visoke dostupnosti. Čak i kvalitetu dijelova i nadređenog inženjering ne sprječava Povremeni pogreške. Ako, na primjer, kada automobil dobije paušalni guma, auto i dalje pokreće, no ga radi s su smanjene funkcije. Ako planu za ovu potencijalne pojavu, koristite nešto gume rezervnih te Tanki obrubljena dok ne dođete do popravak pogon. Iako rezervnih guma ne dopušta brzo brzine, i dalje možete upravljati vozila dok se ne Zamijeni na guma. Isto tako, servis u oblaku tarife za mogućem gubitku mogućnosti možete spriječiti relativno manji problem da dolje cijelu aplikaciju. To vrijedi čak i ako se na servis u oblaku morate pokrenuti s su smanjene funkcije.

Postoji nekoliko ključa karakteristike servise u oblaku iznimno dostupna: dostupnost, skalabilnost i odstupanje kvara. Iako se međusobno su povezana te karakteristike, važno je da biste shvatili svaki i kako pridonose cjelokupan dostupnost rješenja.

###<a name="availability"></a>Dostupnost

Dostupne aplikacije smatra dostupnost njene podlozi Infrastruktura i ovisne servise. Dostupne aplikacije ukloniti jedan točke neuspjeh putem zalihosti i prebacuju dizajna. Kada proširili opseg treba uzeti u obzir dostupnosti u Azure, važno je da biste shvatili pojam učinkovitih raspoloživost platforme. Učinkovita dostupnost smatra razinu ugovore usluge (SLA) svakog zavisne servisa i njihov kumulativni učinak na dostupnost ukupni sustava.

Dostupnost sustava je mjera Postotak vremena prozora sustav će moći raditi. Ako, na primjer, dostupnost SLA barem dvije instance programa weba ili tempiranja uloga u Azure je postotak 99.95 (izvan 100 posto). Ne Izmjerite performanse i funkcionalnost servise te uloge. Međutim, učinkovitih raspoloživost servis u oblaku utječe i razne SLA od druge ovisne servise. Više premještanje dijelova u sustavu više Oprez koje morate poduzeti da bi aplikacija možete resiliently udovoljava dostupnost preduvjetima njegov krajnjim korisnicima.

Razmislite o sljedećim SLA Azure servisa koji koristi servisa Azure: računalnim, baze podataka SQL Azure i Azure prostora za pohranu.

|Servis za Azure|SLA   |Potencijalne minuta nedostupnost/mjesec (30 dana)|
|:------------|:-----|:----------------------------------------:|
|Izračun      |99.95%|21.6 minuta                              |
|SQL baze podataka |99,99%|4.3 minuta                              |
|Prostor za pohranu      |99.90%|43.2 minuta                              |

Morate plan za sve servise za potencijalno pomicanje prema dolje po različite vremenske. U ovom primjeru pojednostavljeni ukupan broj minuta mjesečno koji aplikacije mogu se prema dolje je 108 minuta. Tijekom mjeseca od 30 dana ima Ukupno 43,200 minuta. 108 minuta je postotak.25 ukupan broj minuta tijekom mjeseca od 30 dana (u minutama 43,200). Tako ćete dobiti učinkovitih raspoloživost postotak 99.75 za servis u oblaku.

Međutim, pomoću dostupnost tehnike opisani u ovom dokumentu možete poboljšati to. Ako, na primjer, ako dizajniranja aplikacije da biste nastavili pri bazom podataka sustava SQL nije dostupan, možete koji ukloniti iz jednadžbu. Možda će to znači da se aplikacija pokreće ima smanjene mogućnosti pa postoje i poslovnim zahtjevima treba uzeti u obzir. Popis svih Azure SLA potražite u članku [Ugovore o razini usluge](https://azure.microsoft.com/support/legal/sla/).

###<a name="scalability"></a>Skalabilnost

Skalabilnost izravno utječu na dostupnost. Aplikacija koja ne uspije povećana opterećenju više nije dostupan. Da bi odgovarao povećana zahtjev uz dosljedan rezultate, u sustavu windows prihvatljiva vrijeme su skalabilni aplikacije. Kada je sustav skalabilni, ona mijenja veličinu vodoravno ili okomito da biste upravljali povećanja u učitavanja uz zadržavanje dosljedan performansi. Osnovni rječnikom, vodoravno skaliranje dodaje više strojeva iste veličine (procesor, memorije i propusnosti) tijekom okomitog skaliranja povećava veličinu postojeće strojeva. Za Azure, imate okomiti mogućnosti skaliranja za odabir raznih veličina računala za računalnim. Promjena veličine strojno zauzima ponovno implementacije. Stoga rješenja za najčešće fleksibilne namijenjena je vodoravno skaliranje. Ovo je posebno točno za računalnim, jer je jednostavno možete povećati broj pokrenute instance sve weba ili tempiranja uloge. Dodatne instance rukovati povećana prometa na web-Azure portal, skripte komponente PowerShell ili kod. Temeljna ovaj odluku o povećanja u određenim nadziranim metriku. U ovom scenariju performanse korisničkog ili metriku ne budu izloženi uočljivijih padajuće opterećenju. Obično uloge web- a tempiranja pohranite vanjsko sve stanje. Time se omogućuje za fleksibilne uravnoteženja opterećenja i graceful rukovanje promjene broji instance. Vodoravno skaliranje i dobro funkcionira s servisa, kao što su Azure prostora za pohranu koji sadrže okomito skaliranje tiered mogućnosti.

Oblak implementacijama će se prikazivati kao skup skaliranje jedinice. Time se omogućuje aplikacija biti elastic u održavanje potrebama propusnost krajnjim korisnicima. Promjena veličine jedinice su lakše vizualizirali na razini poslužitelja web- a aplikacije. To je zato Azure već sadrži bez praćenja stanja računalnim čvorove putem web- a tempiranja uloge. Dodavanje više izračunati skaliranje jedinice implementacijskih će uzrokovati bilo koju aplikaciju stanje upravljanja strani efekata, jer su skaliranje jedinice računalnim bez praćenja stanja. Prostor za pohranu skaliranje – jedinica je odgovorni za upravljanje particija podataka (strukturirane i nestrukturirane). Promjena veličine jedinice za pohranu Primjeri particija Azure tablice, spremnik blobova platforme Azure i baze podataka SQL Azure. Čak i korištenje više prostora za pohranu Azure računa ima Izravni utjecaj na skalabilnost aplikacije. Morate dizajnirati iznimno skalabilni oblaku ugraditi više prostora za pohranu skaliranje-jedinica. Na primjer, ako aplikacija koristi relacijskih podataka, preko nekoliko baze podataka SQL particija podatke. Na taj način omogućuje pohranu praćenja skaliranje jedinica model elastic računalnim. Isto tako, za pohranu Azure omogućuje podataka particija sheme koje je potrebno namjerne dizajna potrebama propusnost računalnim sloja. Popis najbolje prakse za dizajniranje servise u oblaku skalabilni, potražite u članku [Najbolje prakse za usluge dizajna Large-Scale na servise u Oblaku Azure](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

###<a name="fault-tolerance"></a>Odstupanje kvara

Aplikacija treba pretpostavlja da svaka mogućnost zavisne oblaka može upućivat trenutku u vremenu. Aplikacija za pogreške kvara otkriva i Potezi oko nije uspjelo elemenata, da biste nastavili i vraćati ispravne rezultate unutar određenog vremenskog razdoblja. Stanja tranzitne pogreške, kvara pogreške sustav će koristiti pravila Ponovi. Za više ozbiljne kvarove aplikacija može otkriti probleme i neće uspjeti putem zamjenski hardver ili hitnom slučaju tarife dok je ispraviti pogreške. Pouzdan aplikacije ispravno možete upravljati neuspjeh jedan ili više dijelova i nastaviti radi ispravno. Kvara pogreške aplikacije mogu koristiti jednu ili više strategije dizajna, kao što su zalihosti, replikacije ili su smanjene funkcije.

##<a name="disaster-recovery"></a>Izrada oporavak

Implementacija oblaka prestat možda funkcionirati zbog systemic nedostupnosti ovisne servise ili podlozi infrastrukture. U odjeljku takve uvjeta poslovni plan continuity pokreće postupka oporavka Izrada. Ovaj postupak obično uključuje operacije osoblje i automatiziranog postupke da biste ponovno aktivirali aplikaciju dostupna regija. Potreban je prijenos korisnike aplikacije, podataka i usluge na novo područje. To uključuje i korištenje medij ili tijeku replikacije.

Razmislite o prethodne analogy koji uspoređuje visoke dostupnosti mogućnost da biste oporavili s nehijerarhijskom guma pomoću programa rezervne. Nasuprot tome, Izrada oporavak obuhvaća korake snimanja nakon rušenja automobila, gdje Auto se više ne može raditi. U tom slučaju, najbolje je rješenje da biste pronašli učinkovit način da biste promijenili Automobili, tako da nazovete Servis za putovanje ili prijatelju. U ovom scenariju postoji vjerojatno će biti dulji zastoja u otvaranju na natrag na putu. Nema više složenosti popravka i vraćanje izvorne vozila. Na isti način je Izrada oporavak na drugoj regiji složeni zadatak koji se obično uključuje neke nedostupnost i mogućem gubitku podataka. Da biste bolje razumjeli i procjenu Strategije oporavka Izrada, važno je da biste definirali dva uvjeta: oporavak vrijeme cilj (RTO) i oporavak pokažite cilj (RPO).

###<a name="recovery-time-objective"></a>Oporavak vrijeme cilj

U RTO je maksimalnu količinu vrijeme dodijeljeno za vraćanju funkcionalnosti aplikacije. To se temelji na zahtjeve za tvrtke, a povezana je s važnost aplikacije. Ključne poslovne aplikacije potreban je malo RTO.

###<a name="recovery-point-objective"></a>Oporavak točke cilj

U RPO je prozor prihvatljiva vrijeme od gubitka podataka zbog postupka oporavka. Ako, na primjer, ako je na RPO jedan sat, morate potpuno sigurnosno kopiranje ili replicirati podatke barem svaki sat. Kada Premjesti gore aplikacije u alternativni regiji sigurnosne kopije podataka možda nedostaje jedan sat podataka. Kao što su RTO, ključnih aplikacije ciljne mnogo manje RPO.

##<a name="checklist"></a>Kontrolni popis

Pogledajmo sažimati ključne točke koje ste je obuhvaćeno članak (i njezin srodnih članaka na [visoke dostupnosti](./resiliency-high-availability-azure-applications.md) i [oporavak Izrada](./resiliency-disaster-recovery-azure-applications.md) za Azure aplikacije). Ovaj sažetak će služiti kao kontrolnog popisa stavki morate imati na umu za vlastitu dostupnosti i Izrada oporavak planiranja. To su najbolje prakse koje su korisne za kupce traže da biste dobili ozbiljne o implementacijom uspješno rješenja.

1. Vođenje rizika za svaku aplikaciju jer se svaki može različito tretiraju. Neke aplikacije su više ključnih od ostalih, a želite obostrano extra cijena za mijenjanje arhitekture za oporavak Izrada.
1. Taj podatak upotrijebiti da biste definirali RTO i RPO za svaku aplikaciju.
1. Dizajn pogreške, počevši od arhitektura aplikacije.
1. Najbolje prakse za visoke dostupnosti implementirati tijekom opterećenja trošak, složenosti i rizik.
1. Implementacija Izrada oporavak tarife i procesa.
  * Razmislite o pogreške koje se protežu na razinu modul usidrili za dovršavanje oblaka prekida.
  * Uspostavljanje sigurnosne kopije Strategije za sve reference i transakcijskih podatke.
  * Odaberite oporavak arhitekturu Izrada više web-mjesta.
1. Definiranje određene vlasnika Izrada oporavak procesa, automatizacija i testiranje. Vlasnik trebali biste upravljanje i vlasnik cijeli postupak.
1. Dokumenata procese tako da ih je lako kontrolirani. Iako jednog vlasnika, većem broju osoba trebali biste moći razumijevanje i slijedite postupke u hitno treba.
1. Obuka osoblja za implementaciju postupka.
1. Pomoću običnog Izrada simulations za obuku i provjere valjanosti postupka.

##<a name="summary"></a>Sažetak

Kada hardver ili aplikacije ne unutar Azure, tehnike i Strategije za upravljanje zapisima razlikuju od kada dođe do pogreške na lokalni sustavi. Glavni razlog za to je da oblaka rješenja obično uključeno više ovisnosti infrastrukture koja se raspodijeliti Azure regija i upravljati kao zasebna services. Morate baviti djelomične neuspjeha Upotreba tehnika visoke dostupnosti. Da biste upravljali raznih pogrešaka, vjerojatno zbog Izrada događaj, pomoću Strategije oporavka Izrada.

Azure otkriva i rukuje više pogrešaka, ali postoje razne vrste pogreške koje je potrebno strategije specifičnim aplikacijama. Morate aktivno Priprema za i upravljanje pogrešaka aplikacije, servise i podataka.

Kada stvarate vaše aplikacije dostupnosti i Izrada oporavak plan, razmislite o rezultate tvrtke pogreške aplikacije. Definiranje procesa, pravila i postupke da biste vratili ključnih sustavi nakon do teškog oštećenja događaj traje, Planiranje i izvršenja. A kad uspostavite planovi, ne možete spriječiti da postoji. Morate redovito analizu, testirajte i neprestano poboljšanje tarife koji se temelji na vašem portfelja aplikacija, poslovne potrebe i tehnologije dostupne. Azure nudi nove mogućnosti i podiže novi izazove pri stvaranju robusne aplikacije withstand pogreške.

##<a name="additional-resources"></a>Dodatni resursi

[Visoke dostupnosti za aplikacije koje se temelji na Microsoft Azure](resiliency-high-availability-azure-applications.md)

[Izrada oporavak za aplikacije koje se temelji na Microsoft Azure](resiliency-disaster-recovery-azure-applications.md)

[Tehnički vodič Azure otpornost](resiliency-technical-guidance.md)

[Pregled: U Oblaku tvrtke continuity i baza podataka Izrada oporavak SQL baze podataka](../sql-database/sql-database-business-continuity.md)

[Visoke dostupnosti i Izrada oporavak za SQL Server na virtualnim strojevima sa sustavom Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)

[Failsafe: Smjernicama arhitekturi prebacuju oblaka](https://channel9.msdn.com/Series/FailSafe)

[Najbolje prakse za dizajniranje veliki usluge na servise u Oblaku Azure](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)

##<a name="next-steps"></a>Daljnji koraci

Ovaj je članak dio niza članaka filtriran na oporavak Izrada i visoke dostupnosti za Azure aplikacije. Sljedeći članak u ovoj seriji je [visoke dostupnosti za aplikacije koje se temelji na Microsoft Azure](resiliency-high-availability-azure-applications.md).
