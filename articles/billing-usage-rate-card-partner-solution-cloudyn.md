<properties
   pageTitle="Korištenje sustava Microsoft Azure i Omogući API-ji RateCard Cloudyn ITFM za kupce | Microsoft Azure"
   description="Pruža jedinstvenu perspektivu iz Microsoft Azure naplata partnera Cloudyn, na svoja iskustva Integracija Azure naplata API-ji u svoje proizvoda.  To je posebno korisno za Azure i Cloudyn korisnike koji žele pomoću/pokušaja Cloudyn za Azure servise."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Korištenje sustava Microsoft Azure i Omogući API-ji RateCard Cloudyn ITFM za korisnike

Za privatno pretpregled nove korištenja resursa sustava Microsoft Azure i RateCard API-ji nije odabran Cloudyn, Microsoft partner razvoj i vodeći dobavljač mogućnosti upravljanja oblaka.  Korištenje API omogućuje pristup Procijenjena Azure potrošnje podataka za pretplatu. RateCard API nudi cjelovit podatke o cijenama sve Azure servisa za korisnike koji nisu vrste "Enterprise-ugovor - EA". Integrirani zajedno, te API-ji osnova potpune informacije za unos u alate za IT financijske Management (ITFM) kao što su oni koje ste dobili od Cloudyn.

## <a name="introduction"></a>Uvod

So-called "množenja" podataka od korištenja API-JA s podacima iz RateCard API (korištenje [jedinice] cijena [$unit] = detaljne korištenje i trošak) stvara najviše zrnastog, točni i pouzdane informacije o naplati dostupne za Azure danas.

![Pregled ITFM][1]

Troše te API-ji sadrži ključne informacije o korištenju klijenata i troškove, što omogućuje Cloudyn za analizu računi kupca jednostavne, programski način i obavljanje zadataka za različite ITFM svojih klijenata.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>Integriranje Cloudyn s RateCard i način korištenja API-ji
RateCard API zahtijeva nekoliko ulaznih parametara – kao što su regije informacije, valute i regionalnu shemu –, ali ona najvažnije je OfferDurableID koji određuje vrstu Azure koja nudi kupca koristi (Pay-as-you-Go, tarife naslijeđene izvršenja 6 i 12-mjesec, MSDN ponuda, mreže Microsoftovih Partnera ponuda, promotivne ponude i drugima). Na OfferDurableID pronaći ćete u [korištenju Azure i portal za naplatu](https://account.windowsazure.com/Subscriptions), u odjeljku "Nude ID" za dani pretplatu.

Nakon registracije za servise [Cloudyn za Azure](https://www.cloudyn.com/microsoft-azure/) klijentima možete dodati kodu OfferDurableID koji omogućuje Cloudyn da biste izvukli njihove odgovarajuće cijene podataka putem RateCard API-JA.  Moguće je pronaći informacije o različitim vrstama ponuda za jednu stranicu s [Detaljima ponuda za Microsoft Azure](https://azure.microsoft.com/support/legal/offer-details/) .

![Pregled modul Cloudyn ITFM][2]

Koristi Cloudyn korištenje i RateCard API-ji, osim performanse API Azure da biste stvorili dodatne Slojevi vizualizacije, analize, upozorenjem, izvješćivanje, cijena te upravljanje njime s akcijama preporuke ponuda Azure kupci alata ITFM pouzdanog enterprise oblaka.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Cloudyn ITFM koristite slučajeva omogućiti korištenje i RateCard API Integracija
Uobičajeni Cloudyn ITFM koristite slučajeva omogućiti korištenje i RateCard API-ji obuhvaćaju:

+ Omogućuje **Analize troškova** - cloud troškove nepotpuno prema dolje do bilo koji izvorni identifikacijski dimenzije (davatelj usluga, servis, računa, područja itd.). Korištenje Azure i RateCard API-ji postaviti jednostavan zadatak, unosom Većina zrnastog analitički upotrebe i trošak po račun koji se zatim grupirane i filtrirani prema Cloudyn i prikazuje korisniku, u obrascu grafika ili tablični oblik.

![Cijena analitički tortni grafikon][3]

+ **360 dodijeljeni trošak** – financije omogućuje i IT voditelji steknite analitički stvarni trošak, upravljačke programe i trendova njihove implementacije oblaka. Dodatno se omogućuje upravitelji jednostavno pridružiti troškovima za implementaciju poslovne jedinice, odjela, regije i više pruža neviđen uvida u oblak troškove pa olakšavanja enterprise chargebacks i showbacks. Korištenje Azure i RateCard API-ji služe kao ulaz Cloudyn je trošak dodijeljeni modul koji dopuna API-ji definiranjem metode i poslovne logike za dodjeljivanje nestrukturirane ili untaggable resursa.

![Grafikon troškova dodijeljeni 360][4]

+ **Promjenu veličine cost-Effective** - nudi desno veličine preporuke za underutilized virtualnim strojevima tako smanjuju troškove klijenta na velikih ili previše dodijeljenu strojeva. To radi tako da Provjera virtualnog računala procesora i metriku RAM-a (putem performanse API-JA), radno vrijeme izvođenja (putem korištenja API-JA) i trošak (putem RateCard API-JA). Cloudyn zatim daje preporuke desno promjenu veličine na temelju underutilized procesora ili RAM-a resursa (performanse) i izračunava Procijenjena računanje množenjem cijena delta (RateCard) između VMs tako da stvarni vrijeme-korištenjem (korištenje) underutilized računala.

![Stvarni trošak promjenu veličine][5]

+ **Preporuke Porting oblaka** - nudi financijske savjete na oblaku porting. Ispituje korisničke trenutni troškove resursa oblaka što implementirate na glavne oblaka dobavljače i uspoređuje trošak implementacije sustava ekvivalentan na Azure. Zatim pruža zrnastog, po – resursa, financijski sustavom porting preporuke za Azure. Nakon assessing ekvivalentan implementacije potrebna Azure (koja se temelji na performanse metriku i korisničke Preference), Cloudyn koristi RateCard API-JA za procjenu trošak ekvivalentan implementacije na Azure.

+ **Izvješća o izvedbi** – omogućeni u sklopu Azure na performanse API-JA, ta izvješća pružaju polja značajki iz Upotreba procesora i RAM-a za optimizaciju preporuke. Ispod je instanci Upotreba izvješća primjer, prezentaciju analitički instanci po average procesora.

![Izvješća o izvedbi][6]

+ **Upravitelj kategorija** – napredne značajke u Cloudyn koji objedinjuje redoslijed resursima unorganized oblaka. Korisnicima se nudi pri slobode za stvaranje vlastite jedinstvene kategorije (oznake) za učinkovita mjerenje i izvješćivanje o pogreškama koji se nalazi u retku s poslovanje. Dodatno, korisnici mogu jednostavno regulira i kategoriziranje nedosljedno označavanje (odnosno pravopisnih pogrešaka i druge nepodudaranja) i automatski prepoznati nestrukturirane resursi za attribution točne trošak.

![Upravitelj kategorija][7]

## <a name="video"></a>Videozapis

Ovdje je kratak videozapis koji pokazuje kako Azure klijenta možete koristiti Cloudyn za Azure i Azure naplata API-ji, da bi se dobio uvida s svoje podatke za Azure potrošnje.

> [AZURE.VIDEO cloudyn-provides-cloud-itfm-tools-via-microsoft-azure-apis]


## <a name="next-steps"></a>Daljnji koraci

+ Pokrenite besplatnu probnu verziju [Cloudyn za Azure](https://www.cloudyn.com/microsoft-azure/) da biste vidjeli kako možete nabaviti trošak prozirnost s prilagođene izvješćivanje i analitiku za implementaciju sustava Microsoft Azure oblaka.
+ Potražite u članku [rast uvida u sustava Microsoft Azure resursa potrošnje](billing-usage-rate-card-overview.md) pregled korištenja resursa Azure i API-ji RateCard.
+ Pogledajte [Azure naplata REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) za dodatne informacije o oba API koje su dio skupa API-ji dobili Azure resursa Upravitelj.
+ Ako želite da biste prikazali desno uzorak koda, pogledajte naše Microsoft Azure naplata API primjere koda na [Primjere koda Azure](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>uči više
+ Dodatne informacije o ponudama za Microsoft Azure Enterprise ugovor (EA), posjetite [licenciranje Azure za tvrtki] (https://azure.microsoft.com/pricing/enterprise-agreement/)
+ Potražite u članku [Pregled upravljanja resursima Azure](azure-resource-manager/resource-group-overview.md) da biste saznali više o Voditelj resursa Azure.
+ Dodatne informacije o paket alata potrebne za pomoć u pokušavaju poznavati oblaka provedu, pogledajte članak Gartner [Tržištu vodič za IT financijske Management (ITFM) alate](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
