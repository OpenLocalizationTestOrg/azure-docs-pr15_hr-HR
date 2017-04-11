<properties
    pageTitle="Pitanja vezana uz performanse za Azure promet Manager | Microsoft Azure"
    description="Objašnjenje učinka na promet upravitelj i testiranju performanse web-mjesta koristite Upravitelj promet"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="performance-considerations-for-traffic-manager"></a>Pitanja vezana uz performanse za Upravitelj promet

Ova stranica objašnjava pitanja vezana uz performanse na pomoću upravitelja promet. Zamislite sljedeće:

Imate instance web-mjesta unutar područja WestUS i EastAsia. Neke instance ne daje Provjera stanja za Upravitelj probni promet. Promet za aplikaciju je preusmjereni na dobar regija. Ovaj prebacivanje se očekuje, ali performanse mogu biti problem na temelju Latencija promet sada putovanje daleka regiju.

## <a name="how-traffic-manager-works"></a>Kako funkcionira Upravitelj promet

Samo utjecaj na performanse koje Upravitelj promet na vaše web-mjesto je početne vrijednosti DNS-a. Zahtjev za DNS-a za naziv profila promet Upravitelj upravlja Microsoftovih DNS poslužitelja korijenski koji hostira trafficmanager.net zone. Upravitelj promet popunjava, a redovito obnavlja temelju promet Upravitelj pravila, a rezultati probni DNS korijenski poslužitelja tvrtke Microsoft. Pa čak i tijekom početne vrijednosti za DNS-a nema DNS upite šalju upravitelju promet.

Upravitelj promet sastoji se od nekoliko komponenti: naziv DNS poslužitelji, servis za API-JA, sloj prostora za pohranu i krajnje nadzor servisa. Komponenta za servis promet Upravitelj ne uspije, postoji nikakav učinak na DNS naziv pridružene promet upravitelj profila. Mijenjaju zapisa u Microsoftovih DNS poslužitelja. No, nadzor krajnjoj točki i ažuriranje DNS-a ne događa. Stoga promet Upravitelj nije moguće ažurirati DNS-a na web-mjestu prebacivanje kada funkcionira primarni web-mjesta.

Razrješavanje DNS naziva je brz i predmemoriraju rezultate. Brzina početne vrijednosti za DNS ovisi o DNS poslužitelji klijentskog programa koji se koristi za razlučivanje imena. Klijent mogu provoditi DNS pretraživanja unutar ms ~ 50. Rezultati pretraživanja predmemoriraju za trajanje na DNS vrijeme važenja (TTL). Zadani TTL za Upravitelj promet je 300 sekundi.

Promet tijeka kroz Upravitelj promet. Kada se dovrši traženje DNS-a, klijent je IP adresa za instance komponente web-mjesta. Klijent povezuje izravno s te adrese i proći kroz Upravitelj promet. Promet Upravitelj pravila odaberete ne utječe na performanse DNS-a. Međutim, performanse usmjeravanje-metode negativno može utjecati sučelje za aplikacije. Ako, na primjer, ako pravilo preusmjerava promet s Sjeverna Amerika instancu smješten u Azija, latenciju mreže za te sesije možda problem s performansama.

## <a name="measuring-traffic-manager-performance"></a>Mjerenje promet Upravitelj performansi

Postoji nekoliko web-mjesta možete koristiti da biste shvatili izvedbe i ponašanja promet upravitelj profila. Mnoge od tih web-mjesta su besplatno, no možda ograničenja. Neki web-mjesta imaju poboljšane nadzor i izvješćivanje o naknadu.

Alati na ove latencies DNS mjere za web-mjesta i prikaz riješi IP adrese za klijenta mjesta diljem svijeta. Većinu tih alata predmemoriju DNS rezultate. Stoga alata za prikaz cijelog traženje DNS prilikom svakog pokretanja test. Kada testiranje s vlastitim klijenta, samo primijetiti cijelog vrijednosti performansi za DNS jednom tijekom trajanja TTL.

## <a name="sample-tools-to-measure-dns-performance"></a>Ogledna alate za mjerenje performanse DNS-a

- [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS nudi brojne alate performansi. Alat za usporedbu DNS prikazuje koliko dugo traje Razrješavanje DNS naziva i koje usporedbu s drugim davateljima usluga DNS-a.

- [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    Jedna alata za najjednostavnije je WebSitePulse. Unesite URL-a da biste vidjeli DNS razlučivost vrijeme, prvi bajt, posljednje bajt i druge statistiku o izvedbi. Na raspolaganju su vam tri različite test mjesta. U ovom primjeru, pogledajte prvi izvođenja prikazuje DNS pretraživanja potrebno je 0.204 sec.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Jer su predmemorirani rezultate, drugi test za krajnju točku isti promet Upravitelj DNS pretraživanja vodi 0.002 sec.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

- [CA aplikacije stilova sintetičkih monitora](https://asm.ca.com/en/checkit.php)

    Prijašnji alat za Watchmouse provjere web-mjesta, ovo web-mjesto pokazati vrijeme Razrješavanje DNS-a iz većeg broja zemljopisna područja istodobno. Unesite URL-a da biste vidjeli vrijeme Razrješavanje DNS-a, vrijeme veze i brzinu iz nekoliko zemljopisna mjesta. Ovaj test možete koristiti da biste vidjeli koji servis vraća za različite mjesta diljem svijeta.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

- [Pingdom](http://tools.pingdom.com/)

    Ovaj alat sadrži statistiku o izvedbi za svaki element web-stranice. Na kartici analiza stranica prikazuje postotak vrijeme utrošeno na DNS-a pretraživanja.

- [Što je moj DNS-a?](http://www.whatsmydns.net/)

    Ovo web-mjesto ne DNS pretraživanja s 20 različitih mjesta i prikazuje rezultate na karti.

- [Istražujte web-sučelja](http://www.digwebinterface.com)

    Ovo web-mjesto prikazuje detaljnije informacije DNS uključujući CNAMEs i na zapisima. Provjerite je li provjera 'Colorize Izlaz' i "Stat" u odjeljku mogućnosti i odaberite "Sve" u odjeljku poslužitelje naziva.

## <a name="next-steps"></a>Daljnji koraci

[O upravitelju promet promet usmjeravanje metode](traffic-manager-routing-methods.md)

[Testirajte postavke upravitelja promet](traffic-manager-testing-settings.md)

[Operacije na Upravitelj promet (REST API referenca)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure promet Upravitelj cmdleta](http://go.microsoft.com/fwlink/p/?LinkId=400769)
