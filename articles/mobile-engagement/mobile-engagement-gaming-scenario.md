<properties 
    pageTitle="Azure implementaciju radnje mobilne aplikacije igraće"
    description="Igraće scenarij aplikacije za primjenu Azure Mobile radnje" 
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

#<a name="implement-mobile-engagement-with-gaming-app"></a>Implementacija mobilne radnje pomoću igraće aplikacije

## <a name="overview"></a>Pregled

Igraće pokretanja je pokrenuo nove ribolova temelji role-play/strategije igraće aplikacije. Igra je već s radom 6 mjeseci. Ova igra velikog uspjeha, koji sadrži milijune preuzimanja i na zadržavanje je vrlo visoka u usporedbi s drugim aplikacijama igraće pokretanja. Na sastanku tromjesečni pregled zainteresiranih strana pristajete koje su im potrebne da biste povećali prosječni prihod po korisniku (ARPU). Paketi u utakmica Premium su dostupni kao posebne ponude. Ove igre paketi korisnicima da biste nadogradili izgled i performanse nad ribolova crte i lures ili tackles na igre. Međutim, Prodaja paketa su nisku. Pa odluče prvi put da biste analizirali iskustva kupca pomoću alata za analize i zatim za razvoj programa radnje program da biste povećali prodaje pomoću napredne segmente.

Koji se temelji na [Azure Mobile radnje - priručnik za početak rada s najbolje prakse](mobile-engagement-getting-started-best-practices.md) stvaraju stvaranje strategije radnje.

##<a name="objectives-and-kpis"></a>Ciljevi i KPI-ja

Nađite se ključne osobe zadužene za igre. Sve pristajete na jednom glavnom cilj – da biste povećali Prodaja paketa premium s 15%. Stvaraju tvrtke ključnih pokazatelja uspješnosti (KPI) da biste mjere i pogon ovaj cilj

* Na kojoj razini igre su te pakete kupljenje?
* Što je prihod po korisniku, po sesiji, tjedan i mjesec?
* Što su vrste omiljene kupnju?

Dio 1 [Uvodni vodič](mobile-engagement-getting-started-best-practices.md) objašnjava kako definirajte ciljeve i KPI-ja. 

S tvrtke KPI-sada definirane, proizvod upravitelj mobilne stvara radnje KPI-ja da biste odredili novi korisnik trendova i zadržavanje.

* Praćenje zadržavanja i korištenje svim sljedećih razdoblja: dnevno, svaki 2 dana, tjedno, mjesečno i svaki 3 mjeseca
* Broji aktivnih korisnika
* Ocjena aplikacija u trgovini

Na temelju preporuke tima za IT sljedeće tehničke KPI-ja dodani odgovaraju na sljedeća pitanja:

* Što je moj korisnički put (koja će se stranica je posjetili, koliko vremena korisnici potrošiti na njemu)
* Broj ruši ili programskih pogrešaka naišao na po sesiji
* Koje verzije OS koristite korisnike?
* Što je prosječna Veličina zaslona za korisnike?
* Kakvu vrstu internetska veza imaju li korisnicima?

Za svaki KPI-JA proizvoda upravitelj mobilne određuje podatke Zasad mora i gdje se nalazi u svoj playbook.

## <a name="engagement-program-and-integration"></a>Korištenje programa i integracija

Prije stvaranje programa dodatne radnje Director projekta Mobile zaduženi za projekt mora imati precizno objašnjenje kako i kada proizvodi potrošiti korisnici.

Nakon 3 mjeseca projekta Director Mobile prikupljenih dovoljno podataka da biste poboljšali njegovog prodaje u aplikaciji automatske obavijesti. On Pratit će koji:

* U prvu nabavu obično događa na razini 14. Za 90% tim slučajevima, Nabava je novi legendary oružje $3.
* U tim slučajevima 80%, korisnici koji ste napravili kupnju, nastavite sa proizvoda i provjerite dodatne kupi.
* Korisnici koji su prošli razinu 20, početi provedu više od $10/ tjedna.
* Korisnici često za kupnju paketa premium na razini 16, 24 i 32.

Zahvaljujući ovu analizu projekta Director Mobile odluči da biste stvorili određene automatske obavijesti nizove da biste povećali aplikacije Prodaja. On stvara tri nizove automatske koja on poziva: Dobro došli program, Program prodaje i neaktivne Program. Dodatne informacije potražite [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)
    ![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
