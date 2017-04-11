<properties
   pageTitle="Planiranje kapaciteta klaster servisa tkanina | Microsoft Azure"
   description="Pitanja vezana uz planiranje kapaciteta, servis tkanina klaster. Razine Nodetypes, rok trajanja i pouzdanosti"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Planiranje pitanja vezana uz servis tkanina klaster kapaciteta

Za implementaciju bilo koji radni planiranje kapaciteta je važnih koraka. Evo nekoliko stavki koje imate treba uzeti u obzir u sklopu tog procesa.

- Broj vrsta čvor svoj klaster mora započeti s
- Svojstva svih vrsta čvora (veličina, primarni, internet nasuprotne broj VMs, itd.)
- Osobina pouzdanosti i rok trajanja klaster

Javite nam Kratak pregled svaku od tih stavki.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Broj vrsta čvor svoj klaster mora započeti s

Najprije morate znate koja će se koristiti za što se događa klaster stvarate i vrste aplikacija za planiranje za implementaciju u ovoj grupi. Ako niste Očisti na svrhu klaster, nisu najvjerojatnije još spreman za unos postupak planiranja kapaciteta.

Uspostavljanje broj vrsta čvor svoj klaster mora započeti s.  Svaka vrsta čvor mapirani da biste postavili na skaliranje virtualnog računala. Svaka vrsta čvor pa možete skalirana prema gore ili dolje neovisno o imaju različite skupove otvorene priključke i može imati kapacitet različitih metriku. Pa odluka broj vrsta čvor zapravo dolazi prema dolje da biste Imajte na umu sljedeće:

- Aplikacija ima više usluga i učinite ih moraju biti javno ili Internetu? Uobičajeni aplikacije sadrže sučelja pristupnika servisa koja prima unos od klijenta i jednu ili više pozadinskih servise koje komunikaciju s pristupne servisima. Stoga u tom slučaju kraju imate barem dvije vrste čvor.

- Moram li servisa (koji čine aplikacija) infrastruktura za različite potrebe kao što su veći RAM-a ili noviji procesora ciklusa? Na primjer, Javite nam pretpostavlja da sadrži aplikaciju koju želite uvesti sučelja usluge i usluge pozadinske. Servis za sučelja mogu se izvoditi na manje VMs (kao što su D2 VM veličina) koje imaju priključke otvorite s Internetom.  Servis za pozadinsku no ćete morati usko izračunavanje i mora se izvoditi na veću VMs (Ako je veličina VM kao što su D4, D6, D15) koji nisu internet okrenuta.

 U ovom primjeru, iako možete odlučiti da biste stavili sve servise na jedan čvor vrsta, preporučujemo da stavite ih u klaster dvije vrste čvor.  Time se omogućuje za svaku vrstu čvor da bi distinct svojstva kao što su internetska veza ili veličina VM. Broj VMs može biti skalirana zasebno, kao i.  

- Budući da nije moguće predviđanje u budućnosti, izabrati činjenice znate i odlučiti na broj vrsta čvor potrebne aplikacija započeti s. Uvijek možete dodati ili ukloniti vrste čvor kasnije. Servis tkanina klaster, morate imati barem jedan čvor vrsta.

## <a name="the-properties-of-each-node-type"></a>Svojstva svaku vrstu čvor

**Vrsta čvora** mogu vidjeti kao ekvivalentne ulogama u servise u Oblaku. Vrste čvor definirati veličina VM, broj VMs i njihova svojstva. Svaka vrsta čvora koji je definiran na servis tkanina postavljena kao zasebna virtualnog računala skaliranje skup. Skupovi skaliranje VM su programa Azure izračunati resursa možete koristiti za upravljanje zbirka virtualnim strojevima kao skup i. Se definira kao distinct skaliranje VM postavlja svaku vrstu čvor pa možete skalirana prema gore ili prema dolje neovisno o imati različite skupove otvorene priključke, a možete imati metriku kapacitet različitih.

Svoj klaster može imati više od jedne vrste čvor, ali Vrsta primarnog čvora (prve koju ste definirali na portalu) morate imati barem pet VMs za klastere koje se koriste za proizvodnju radnih opterećenja (ili najmanje tri VMs za klastere test). Ako stvarate klaster pomoću predloška Voditelj resursa, ćete pronaći **je primarni** atribut u odjeljku Vrsta definiciju čvor. Vrsta primarnog čvor je vrsta čvora mjesto na kojem se nalazi servisa sustava tkanina servisa.  

### <a name="primary-node-type"></a>Vrsta primarnog čvora
Za klaster s više vrsta čvor, morat ćete odabrati jednu od njih biti primarni. Slijedi nekoliko osobina primarni čvor vrste:

- Minimalna veličina VMs za vrstu primarni čvor određen je rok trajanja sloju odaberete. Zadano za sloju rok trajanja je bronzani. Pomaknite se prema dolje pojedinosti o vrijednosti može potrajati i što je rok trajanja sloju.  

- Najmanji broj VMs za vrstu primarni čvor ovisi o pouzdanosti sloju odaberete. Zadane postavke pouzdanosti sloju je Srebrno. Pomaknite se prema dolje pojedinosti o što je razina pouzdanosti i vrijednosti može potrajati.

- Servisa sustava tkanina servisa (na primjer, servis upravitelja klaster ili servis za pohranu slika) spremaju se na unesite primarni čvor i pa pouzdanosti i rok trajanja klaster određen u pouzdanosti sloju i rok trajanja sloju vrijednost za vrstu primarni čvor odaberete.

![Snimka zaslona s klaster koji sadrži dvije vrste čvor ][SystemServices]


### <a name="non-primary-node-type"></a>Vrsta osobe koje nisu primarni čvora
Klaster s više vrsta čvor, postoji primarni čvor vrsta i ostale su-primarni. Slijedi nekoliko osobina vrste web-primarni čvor:

- Minimalna veličina VMs za tu vrstu čvor određen je rok trajanja sloju odaberete. Zadano za sloju rok trajanja je bronzani. Pomaknite se prema dolje pojedinosti o vrijednosti može potrajati i što je rok trajanja sloju.  

- Najmanji broj VMs za tu vrstu čvor može biti nešto. No trebali biste odabrati taj broj utemeljen na broju replike aplikacije i servise koje želite pokrenuti u ovoj vrsti čvor. Nakon što ste implementiran klaster možete povećati broj VMs čvor slovima.


## <a name="the-durability-characteristics-of-the-cluster"></a>Rok trajanja karakteristike klaster

Rok trajanja sloju koristi za označavanje u sustavu ovlasti za vaše VMs imaju podlozi Azure infrastrukture. U vrsti primarni čvor tom ovlasti omogućuje servisa tkanina pauziranje svaki VM razine infrastrukture zahtjev (primjerice VM ponovno pokrenite računalo, VM reimage ili VM migracije) utjecati na kvorum preduvjeti za servise sustava i s praćenjem stanja servisa. Vrste-primarni čvor u tom ovlasti omogućuje servisa tkanina pauziranje svaki zahtjev razine infrastrukture VM poput VM ponovno pokrenite računalo, VM reimage, VM migracije itd., utjecati na kvorum zahtjevi za s praćenjem stanja servisa radi u njoj.

Ta se ovlast izražen je u sljedeće vrijednosti:

- Zlatna - infrastrukture zadacima koje će biti privremeno trajanja dva sata po UD

- Srebrno - infrastrukture zadacima koje će biti privremeno trajanja 30 minuta po UD (to trenutno nije omogućen za korištenje. Jednom omogućeno to će biti dostupne na svim standardni VMs jedan core i noviji).

- Bronzani - nema ovlasti. To je zadana postavka.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Pouzdanost karakteristike klaster

Razina pouzdanosti koristi se za određivanje broja replike sistemskih servisa koji želite pokrenuti u ovoj grupi na unesite primarni čvor. Dodatne na broj replike, više pouzdanog servisa sustava su u svoj klaster.  

Razina pouzdanosti možete poduzeti sljedeće vrijednosti.

- Platinaste – pokretanje servisa sustava pomoću cilj replike postaviti broj od 9

- Zlatna – pokretanje servisa sustava pomoću cilj replike postaviti broj 7

- Srebrno – pokretanje servisa sustava pomoću cilj replike postaviti broj 5

- Bronzani – pokretanje servisa sustava pomoću cilj replike postaviti broj 3

>[AZURE.NOTE] Razina pouzdanosti odaberete određuje najmanji broj čvorove mora imati vrstu sustava primarni čvor. Razina pouzdanosti ima bez sa slikom na veličinu Maks klaster. Tako da imate 20 čvor klaster koji se izvodi na bronzani pouzdanosti.

 Možete odabrati da biste ažurirali pouzdanosti klaster iz jednog sloju u drugu. To će pokrenuti nadogradnje klaster potrebne da biste promijenili replike servisi sustava postavite count. Pričekajte nadogradnje u tijeku da biste dovršili prije unošenja ostale promjene klaster, kao što su dodavanje čvorove itd.  Možete pratiti napredak nadogradnje servisa tkanina Explorer ili tako da pokrenete [Get-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt126012.aspx)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci

Kada završite s planiranja kapaciteta i postavljanje klaster, pročitajte sljedeće:
- [Sigurnost servisa tkanina klaster](service-fabric-cluster-security.md)
- [Tkanina servis stanja modela Uvod](service-fabric-health-introduction.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
