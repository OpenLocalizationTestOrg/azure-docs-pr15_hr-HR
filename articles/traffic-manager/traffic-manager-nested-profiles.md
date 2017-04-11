<properties
    pageTitle="Ugniježđene funkcije promet Upravitelj profili | Microsoft Azure"
    description="U ovom se članku objašnjava značajku 'Ugniježđene funkcije profili' Azure promet upravitelja"
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

# <a name="nested-traffic-manager-profiles"></a>Ugniježđene promet upravitelj profila

Upravitelj promet uključuje raspon promet usmjeravanje metode koje omogućuju vam da biste odredili način promet Upravitelj odabire koje krajnjoj točki trebale primiti promet iz svakog krajnjeg korisnika. Dodatne informacije potražite u članku [Upravitelj promet promet usmjeravanje metode](traffic-manager-routing-methods.md).

Svaki profil promet Upravitelj određuje jedan način promet usmjeravanja. No postoje scenariji koje je potrebno više sofisticirane promet usmjeravanje od postupka nudi jedan profil Upravitelj promet. Možete ugnijezditi profili Upravitelj promet za kombiniranje prednosti više postupaka promet usmjeravanja. Ugniježđene profili jednostavan način možete nadjačati zadano ponašanje Upravitelj promet za podršku za veće i složenije implementacijama aplikacije.

Sljedeći primjeri pokazuju kako koristiti ugniježđene promet upravitelj profila u različitim scenarijima.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Primjer 1: Kombiniranje 'Performanse' i 'Weighted' usmjeravanje prometa

Pretpostavimo da ste implementirati aplikaciju u sljedećim regijama Azure: Zapad SAD-a, Zapad Europe i istočnoazijski. Pomoću promet Upravitelj 'Performanse' promet usmjeravanje metoda distribucija promet na najbliži korisniku regiju.

![Upravitelj promet jednog profila][1]

Sada, pretpostavimo da želite testirati ažuriranje na servisu prije nego što je široko vodoravnim dodatne informacije. Koji želite koristiti 'ponderiranog način promet usmjeravanje usmjerite small postotak promet za implementaciju sustava test. Postavljanje testnog uvođenja duž postojeće implementacije radnog u Europi Zapad.

Ne možete kombinirati obje 'Weighted' i ' performanse promet-usmjeravanju u jedan profil. Da biste podržavaju taj scenarij, stvorite profil Upravitelj promet pomoću dva Zapad Europe krajnjih točaka i metodu promet usmjeravanje 'Weighted'. Nakon toga dodajte ovaj profil "podređeni" kao krajnje profil "nadređenog". Profil nadređenog i dalje koristi način promet usmjeravanje performanse i sadrži druge globalni implementacijama kao krajnje točke.

Na sljedećem su dijagramu ilustrira u ovom primjeru:

![Ugniježđene promet upravitelj profila][2]

U ovoj konfiguraciji promet usmjerenog putem profila nadređenog distribuira promet preko područja normalno. Unutar Zapad Europe ugniježđene profila distribuira promet za krajnje točke Proizvodnja i testiranje prema debljine dodijeljeni.

Kada profila nadređenog koristi metodu promet usmjeravanje 'Performanse', svaki krajnjoj točki mora biti dodijeljena mjesto. Mjesto se dodjeljuje prilikom konfiguriranja krajnju točku. Odaberite područje Azure najbližeg implementaciju sustava. Azure područja su vrijednosti mjesto podržava latencije tablicu Internet. Dodatne informacije potražite u članku [promet Upravitelj 'Performanse' metodu promet usmjeravanja](traffic-manager-routing-methods.md#performance-traffic-routing-method).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Primjer 2: Krajnje točke nadzor ugniježđene profile

Upravitelj promet aktivno nadzire funkcioniranje svaki servis krajnje točke. Ako je krajnje dobro, promet Upravitelj usmjerava korisnicima zamjenski krajnje točke da biste sačuvali dostupnost usluge. Ovo krajnja točka za nadzor i prebacivanje ponašanje primjenjuje se na sve načine promet usmjeravanja. Dodatne informacije potražite u članku [Upravitelj promet krajnjoj točki praćenja](traffic-manager-monitoring.md). Krajnja točka za nadzor funkcionira na drugačiji način za ugniježđene profile. S ugniježđenim profilima profila nadređene ne izvršiti provjeru stanja na podređeni izravno. Umjesto toga stanja krajnje točke profila podređene se koristi za izračunavanje stanje podređeni profila. Ove informacije o stanju upisuje kroz hijerarhiju ugniježđene profila. Nadređeni profil to pridružuje stanja da biste odredili za Izravni promet dijete profil. U odjeljku [Najčešća pitanja vezana uz](#faq) ovog članka za sve detalje nadzor stanja ugniježđene profila.

Povratak u prethodnom primjeru, pretpostavimo da radnog implementacije u Europi Zapad neće uspjeti. Prema zadanim postavkama profila 'podređeni' usmjerava sav promet za implementaciju test. Ako testnog uvođenja i ne uspije, nadređenog profila određuje da profila podređene trebale primiti promet jer su krajnje točke za sve podređene dobro. Nakon toga profila nadređenog distribuira promet na područja.

![Prebacivanje ugniježđene profila (zadano ponašanje)][3]

Možda ste zadovoljni s ovom rasporedu. Ili možda brinuti da sve promet Zapad Europe sada će testnog uvođenja umjesto ograničeni podskup promet. Bez obzira na stanje testnog uvođenja, želite neće uspjeti pokazivač na područja ako ne uspije radnog implementacije u Europi Zapad. Da biste omogućili ovu prebacivanje, možete odrediti parametar 'MinChildEndpoints' prilikom konfiguriranja profila podređene kao krajnje točke u profilu nadređenog. Parametar određuje najmanji broj dostupnih krajnje točke u profilu podređenih. Zadana vrijednost je "1". U ovom scenariju postavite vrijednost MinChildEndpoints 2. Ispod prag, profil nadređenog smatra cijelu podređeni profila da bi vam biti dostupan i usmjerava promet na druge krajnje točke.

Sljedeća slika prikazuje tu konfiguraciju:

![Ugniježđene funkcije profila prebacivanje s 'MinChildEndpoints' = 2][4]

>[AZURE.NOTE]
>Metodu promet usmjeravanje 'Prioritet' distribuira sve promet na jednom krajnjoj točki. Stoga je malo svrhu MinChildEndpoints postavka koji nije "1" dijete profila.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Primjer 3: Prioritet prebacivanje regijama usmjeravanje prometa "Performanse"

Zadano ponašanje za metodu promet usmjeravanje 'Performanse' osmišljene su da biste izbjegli previše učitavanje dalje najbliži krajnjoj točki i uzrokuje kaskadnih niz neuspjeha. Ako krajnje ne uspije, sve promet koji želite imati su usmjereni na tom krajnje točke se jednoliko raspodijeliti na druge krajnje točke sva područja.

![Promet 'Performanse' usmjeravanje sa zadanom prebacivanje][5]

Međutim, pretpostavimo da radije promet prebacivanje Zapad Europe Zapad SAD-a, a samo usmjerili promet drugim regijama kada obje krajnje točke nisu dostupne. Možete stvoriti rješenja pomoću profila dijete metodom "Prioriteta" promet usmjeravanja.

![Promet 'Performanse' usmjeravanje s preferential prebacivanje][6]

Budući da krajnju točku Zapad Europe ima viši prioritet od krajnje Zapad SAD-a, sav promet se šalje krajnju točku Zapad Europe oba krajnje točke koji su na mreži. Ako ne uspije Zapad Europe, njegov promet je preusmjereni na Zapad SAD-a. Pomoću ugniježđene profila promet je preusmjereni na istočnoazijski samo kada Zapad Europe i Zapad sad neće uspjeti.

Ponovite ovaj uzorak za sve regije. Zamijenite sve tri krajnje točke u profilu nadređenog tri profila podređene svaki pruža prioritet prebacivanje niz.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Primjer 4: Kontroliranje 'Performanse' promet usmjeravanje između više krajnje točke u istom području

Pretpostavimo da metodu promet usmjeravanja se koristi u profil koji sadrži više od jedne krajnjoj točki unutar određenog područja 'performanse". Prema zadanim postavkama, promet usmjereni na to područje distribuira ravnomjerno preko svih krajnje točke dostupne u tom području.

![Promet 'Performanse' usmjeravanje prometa u regiji raspodjele (zadano ponašanje)][7]

Umjesto dodavati više krajnje točke u Europi Zapad, te krajnje točke su obuhvaćeni podređene zaseban profil. Podređeni profil dodaje nadređenog kao krajnju točku samo u Europi Zapad. Postavke na profilu podređeni možete kontrolirati promet-razdiobu sa Zapad Europe omogućivanjem utemeljenu na prioritet ili ponderiranog promet usmjeravanje unutar to područje.

![Promet 'Performanse' usmjeravanje s raspodjelom prilagođene promet u regiji][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Primjer 5: Postavke nadzora za po krajnjoj točki

Pretpostavimo da koristite Upravitelj promet provođenju migriranja promet s na naslijeđen lokalnog web-mjesta za nove oblaku verzije smješten u Azure. Stari web-mjesta koji želite koristiti na početnoj stranici URI praćenje stanja web-mjesta. No za verziju za nove oblaku su implementaciju prilagođene stranice za nadzor (put "/ monitor.aspx") koji uključuje dodatne provjere.

![Upravitelj promet krajnje nadzor (zadano ponašanje)][9]

Postavke nadzora u profilu promet Upravitelj primjenjuju se sve krajnje točke u jedan profil. S ugniježđenim profila, koristite profil različitim podređeni po web-mjestu da biste definirali različite postavke nadzora.

![Upravitelj promet krajnje nadzor s postavkama po krajnje točke][10]

## <a name="faq"></a>NAJČEŠĆA PITANJA

### <a name="how-do-i-configure-nested-profiles"></a>Kako konfigurirati ugniježđene profila?

Ugniježđene promet upravitelj profila mogu konfigurirati pomoću upravitelja resursa Azure i klasični Azure REST API-ji, različite platforme Azure EŽA naredbe i cmdleta ljuske PowerShell Azure. Također podržane putem na novom portalu Azure. Nije podržana na portalu klasični.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Koliko Slojevi ugnježđivanjem ne promet Upravitelj podržavati?

Možete ugnijezditi profili do 10 razina dubine. "Petlje nije dopušteno.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Mogu kombinirati druge vrste krajnje točke s profilima ugniježđene podređeni, u jednak profilu promet Manager?

Da. Nema ograničenja na kako spojiti krajnje točke različitih vrsta unutar profil.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Kako model za naplatu primjenjiv za ugniježđene profila?

Nema više negativan cijene utjecaj korištenje ugniježđenih profila.

Upravitelj promet naplatu sadrži dvije komponente: provjere stanja krajnjoj točki i milijune DNS upite

- Provjera stanja krajnjoj točki: postoji bez naknade profila podređene kada konfigurirati kao krajnje točke u profilu nadređenog. Nadzor od krajnje točke u profilu dijete se naplaćuju na uobičajen način.
- DNS upite: svaki upit samo jednom broje. Upit poslan nadređenog profila koji vraća krajnje iz profila podređene broje se od nadređenog profila samo.

Sve pojedinosti potražite u članku [Upravitelj promet cijene stranica](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Je li utjecaj performansi za ugniježđene profila?

ne. Postoji utjecaj na performanse nastale kada se koristi ugniježđene profila.

Upravitelj promet odrednice interno prolaziti hijerarhiji profila tijekom obrade svaki upit DNS. DNS upit nadređene profil možete primati odgovor DNS-a za krajnje iz podređene profila. Jedan CNAME zapis koristi li koristite jedan profil ili ugniježđene profila. Nema potrebe da biste stvorili CNAME zapis za svaki profil u hijerarhiji.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Kako Upravitelj promet izračunati stanja ugniježđene krajnje točke u profilu nadređenog?

Profil nadređenog ne izravno izvršiti provjeru stanja na podređeni. Umjesto toga stanja krajnje točke profila dijete se koriste za izračun stanje dijete profila. Ove informacije upisuje kroz hijerarhiju ugniježđene profila radi određivanja stanja ugniježđene krajnjoj točki. Nadređeni profila koristi ovaj Zbrojeno stanja da biste odredili hoće li se promet biti preusmjereni dijete.

U sljedećoj tablici opisane ponašanje promet upravitelja stanja provjerava ugniježđene krajnjoj točki.

|Status podređeni profil monitora|Status nadređenog Monitor krajnje točke|Bilješke|
|---|---|---|
|Onemogućeno. Podređeni profila je onemogućen.|Zaustavi|Prekinut je stanje krajnjoj točki nadređenog li omogućeni. Stanje onemogućen je rezervirana za koji označava onemogućeni krajnje točke u profilu nadređenog.|
|Smanjena. Najmanje jedan podređeni element profila krajnje je u stanju Degraded.| Online: broj Online krajnje točke u profilu podređeni barem je vrijednost MinChildEndpoints.<BR>CheckingEndpoint: broj Online plus CheckingEndpoint krajnje točke u profilu dijete barem je vrijednost MinChildEndpoints.<BR>Smanjena: inače.|Promet se usmjeriti krajnje statusa CheckingEndpoint. Ako je postavljen je MinChildEndpoints prevelika, smanjena je uvijek krajnju točku.|
|Putem Interneta. Najmanje jedan podređeni element profila krajnje je mrežni rad. Nema krajnje je u stanju Degraded.|Pogledajte gore.||
|CheckingEndpoints. Najmanje jedan podređeni element profila krajnje je 'CheckingEndpoint'. Krajnje točke su 'Online' ili 'smanjena|Isto kao prethodna.||
|Neaktivni. Sve podređene profila krajnje točke su onemogućena ili zaustavljen, odnosno ovaj profil sadrži krajnje točke.|Zaustavi||


## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [funkcioniranje Upravitelj promet](traffic-manager-how-traffic-manager-works.md)

Saznajte kako [stvoriti promet upravitelj profila](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png

