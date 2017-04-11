<properties 
    pageTitle="Azure implementaciju Mobile radnje za aplikaciju za medijske sadržaje"
    description="Scenarij aplikacije medijskih sadržaja za implementaciju Azure Mobile radnje" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

#<a name="implement-mobile-engagement-with-media-app"></a>Implementacija mobilne radnje pomoću aplikacije za medijske sadržaje

## <a name="overview"></a>Pregled

Nevena je voditelj projekta mobilne tvrtke velike medijske sadržaje. On nove aplikacije koji sadrži vrlo visoka preuzimanje broj nedavno pokrenuo. On sadrži kliknite svoj ciljevi za preuzimanje, ali i dalje njegovog vratili na Investment(ROI) po korisniku zadovoljava njegovog preduvjeti. 

Nevena otkrio već Zašto su pretihi svoj povrat ulaganja. Korisnici često prestali koristiti svoj aplikacija nakon samo dva tjedna, a Većina ih nikad ne vratite. Britanski da biste povećali zadržavanja njegovog aplikacije.

Kada neki velikim početnim testiranje, on zapamtio kada on engages svoj korisnici s automatske obavijesti, oni često da biste nastavili koristiti svoj aplikacije. I korisnici koji su neaktivni često vratit će aplikaciju ovisno o obavijesti on im šalje. Nevena odlučuje ulažete u neke vrste programa radnje za svoj aplikaciju koja koristi napredne ciljanja s automatske obavijesti.

Nevena nedavno pročitao [Azure Mobile radnje - priručnik za početak rada s najbolje prakse](mobile-engagement-getting-started-best-practices.md) i odlučio implementirati preporuke iz vodiča.

##<a name="objectives-and-kpis"></a>Ciljevi i KPI-ja

Nađite se ključne osobe zadužene za Nevena na aplikaciju. Reklame tvrtke generirao kao korisnici zauzeti njegovog medijske sadržaje. Povećavanjem sadržaj potrošena po korisniku Nevena povećava njegovog prihod. Sve pristajete na jednom glavnom cilj: da biste povećali Prodaja reklame po 25%. Stvaraju tvrtke ključnih pokazatelja uspješnosti (KPI) da biste mjere i pogon ovaj cilj

* Broj reklame kliknuli po korisniku
* Koliko stranica artikala posjetili (po korisniku / po sesiji / tjedan / mjesec...)
* Što su omiljene kategorije

Ovisno o Nevena na sastanak s ključne osobe zadužene za on je definirao njegovog ključne pokazatelje uspješnosti tvrtke. On slijedi dio 1 [Azure Mobile radnje - priručnik za početak rada s najbolje prakse](mobile-engagement-getting-started-best-practices.md). 

Nakon toga on stvara KPI-ja sljedeće radnje da biste bili sigurni da se ciljevi dosegne:

* Praćenje zadržavanja preko sljedećih razdoblja: dnevno, tjedno, bi tjedno i mjesečno.
* Broji aktivni korisnici
* Sprema aplikaciju ocjena u aplikaciji

Na temelju preporuke tima za IT sljedeće tehničke KPI-ja dodani odgovaraju na sljedeća pitanja:

* Što je moj korisnički put (koja će se stranica je posjetili, koliko vremena korisnika potrošiti na njemu)
* Broj ruši ili programskih pogrešaka naišao na svakoj sesiji?
* Koje verzije OS koristite korisnike?
* Što je prosječna Veličina zaslona za korisnike?
* Kakvu vrstu internetske veze imaju li korisnicima?

Za svaki KPI on raspoređuje podatke potrebne, a on je zapisa u na odgovarajuće mjesto od postane playbook.

## <a name="engagement-program-and-integration"></a>Korištenje programa i integracija

Sada tu Nevena Završi definiranje njegovog KPI-ja, definiranjem 4 radnje programe i njihovih ciljevi pokrene njegovog faza strategije radnje:    ![][1]

Zatim Nevena dolazi dublju po dovršenog slanje obavijesti za svaki program. Automatske obavijesti definira pet elemenata:

1. Cilj: što je cilj obavijesti
2. Kako će se otvoriti cilj
3. Cilj: koji će primati obavijesti?
4. Sadržaj: Što je tekst i oblik obavijesti (u App/Out od aplikacija)
5. Kada: što je najbolje trenutak da biste poslali te automatske obavijesti

    ![][2]

Dodatne informacije potražite [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Prema dio 2 studiju Nevena koristi odredišnu sekciju za definiranje podataka koje on sadrži da biste prikupili i piše njegovog oznaka namjeravate koji su zajedno s timom IT implementacija rješenja. Nakon jednog tjedna implementacije i testiranja za prihvaćanje korisnika Nevena na kraju pokrenite njegovog programe.

##<a name="program-results"></a>Program rezultata

4 mjeseca kasnije, Ivica pregledava izvedbe programa. Dobro došli Program i tjedni Program sastanak njegovog ciljevima. Smanjuje broj korisnika s samo jedne sesije, koriste dodatne značajke aplikacije i sadrži udvostručuje broj veza po tjednu.

**Neaktivni Program** pomaže Nevena razumjeti tendencies korisnika. Čini se da 15% od neaktivne korisnike vratite se u aplikaciju. No većina od njih ne ostaju aktivne više od 1 mjesec. Nevena foresees potencijalne optimizaciju o ovom slijedu sa dodatne obavijesti i proširivanje njegovog sadržaja odabira.

**Uvod u Program** i ne funkcionira. Povećava unakrsni prodaje, ali nema dovoljno dosegne njegovog ciljeva. Nevena označava da je nema dovoljno podataka da biste odgovarajuće ciljanja i predloži odgovarajući sadržaj. Zaustavlja ovaj program i usredotočuje se na slanje "Lektorsko automatske obavijesti" s Azure Mobile radnje. Svoj novinare već imate a CMS rješenja da biste poslali automatske obavijesti i oni ne želite mijenjati.

Nevena odluči da biste koristili API dosegne koji je na HTTP REST API-JA koji omogućuje upravljanje razgovor kampanje bez potrebe za korištenjem web-AZME sučelja. Na taj se način Nevena možete prikupiti podatke on mora i omogućuje njegovog autorima nastaviti koristiti a CMS rješenja.

Da bi ta značajka funkcionira ispravno, Ivica pita IT timom biti posebno na sljedeće točke:

1. **Sustavi za operaciju** : svi oni imaju vlastite pravila za administriranje automatske obavijesti da Nevena odluči da biste dobili popis sve slučajeve i provjerava ako rukovati API-ji ga.
Npr.: Sustava Android automatske omogućuje širu sliku koja nije slučaj s operacijskim sustavom iOS.

2. **Vremensko razdoblje**: Nevena želi API koji postavite vremenski okvir i postavite na kraj kampanja. Britanski da biste sačuvali korisnika iz bilo kojeg bombing disruptive obavijesti.

3. **Kategorije**: Marketing tim pripremi predložak za svaku vrstu upozorenjem. Nevena pita IT timom da biste postavili kategorije unutar na API-JA.

Nakon neke testovi Nevena je zadovoljni. Uz pomoć ovog API novinare i dalje može slati automatske obavijesti s a CMS i Azure Mobile radnje prikuplja svih behavioral podataka za njih

Nakon tih 4 najprije mjeseci, rezultati odražavaju dobar ukupne performanse i daje pouzdanosti za Nevena i njegovog ploče povrat ulaganja po korisniku povećava po 15%, a zatim mobilne prodaje predstavljaju 17,5% od ukupne prodaje povećava 7.5% samo četiri mjeseca.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
