<properties
    pageTitle="Stvaranje pozdrav svijeta servis u Oblaku za Azure u Eclipse"
    description="Saznajte kako stvoriti jednostavan pozdrav svijeta aplikacije pomoću alata za Azure Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->

# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Stvaranje pozdrav svijeta servis u Oblaku za Azure u Eclipse #

Sljedeći koraci vam pokazati kako stvoriti i implementirati osnovne aplikacije JSP za Azure pomoću alata za Azure Eclipse. Za jednostavnosti se prikazuju JSP primjer, ali vrlo slično korake prikladna servlet na Java koliko god se brine Azure implementacije.

Aplikacija će izgledati otprilike ovako:

![][ic600360]

## <a name="prerequisites"></a>Preduvjeti ##

* Na Java za razvojne inženjere komplet (JDK), v 1.7 ili novijim.
* Eclipse IDE za razvojne inženjere EE Java, indigo plava ili noviji. To se može preuzeti iz <http://www.eclipse.org/downloads/>.
* Distribucija utemeljena na web-poslužitelj ili poslužitelj aplikacije, kao što su Apache Tomcat, GlassFish, JBoss poslužitelj aplikacije, Jetty ili IBM® WebSphere® aplikacije Server Liberty Core.
* Azure pretplatu, koje možete dobivene iz <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure alata za Eclipse. Dodatne informacije potražite u članku [Instaliranje alata za Azure za Eclipse][].

## <a name="to-create-a-hello-world-application"></a>Da biste stvorili pozdrav svijeta aplikacije ##

Najprije ćemo ćete pokrenuti sa stvaranjem Java projekta.

*  Pokretanje Eclipse, i na izborniku kliknite **datoteka**, kliknite **Novo**, a zatim **Dinamički Web projekta**. (Ako ne vidite **Dinamički Project Web** navedena kao dostupne projekta nakon klika na **datoteku**, **Novi**, učinite sljedeće: kliknite **datoteka**, kliknite **Novo**, kliknite **projekta...**, proširite **Web**, kliknite **Dinamički Project Web**i kliknite **Dalje**.)
*  Za potrebe ovog praktičnog vodiča, nazovite projekta **MyHelloWorld**. (Provjerite koristiti taj naziv, sljedeće korake ovog praktičnog vodiča očekivati WAR datoteku da biste se s nazivom MyHelloWorld). Zaslon izgledat će otprilike ovako: ![][ic589576]
* Kliknite **Završi**.
* U prikazu programa Project Explorer Eclipse, proširite **MyHelloWorld**. Desnom tipkom miša kliknite **WebContent**, kliknite **Novo**, a zatim kliknite **Datoteke JSP**.
* U dijaloškom okviru **Nova datoteka JSP** naziv datoteke **index.jsp**. Zadrži nadređenu mapu kao **MyHelloWorld/WebContent**, kao što je prikazano na sljedećim mjestima:  ![][ic659262]
* U dijaloškom okviru **Odabir predloška JSP** za potrebe ovog praktičnog vodiča odaberite **Novu datoteku JSP (html)** , a zatim kliknite **Završi**.
* Kada index.jsp datoteka se otvara u Eclipse, dodati tekst koji se dinamički prikazuje **Pozdrav svijete!** unutar postojeći `<body>` element. Vaš ažurirani `<body>` sadržaj prikazivati kao sljedeće:
```
    <body>
    <b><% out.println("Hello World!"); %></b>
    </body>
```
* Spremite index.jsp.

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>Za implementaciju aplikacije da biste Azure, brz i jednostavan način ##

Kad ste spremni ispitati Java web-aplikacije, koristite sljedeći prečac da biste isprobali neku izravno na Azure oblaka.

1. U Eclipse na Project Explorer kliknite **MyHelloWorld**.
1. Na alatnoj traci Eclipse kliknite padajući gumb **Objavi** , a zatim kliknite **Objavi kao Azure servis u Oblaku**
    ![][publishDropdownButton]
1. Ako objavljujete ovu aplikaciju Azure prvi put, a niste stvorili projekta programa Azure implementacije za ovu aplikaciju prije, projektu programa Azure implementacije umjesto vas automatski stvarati. Trebali biste vidjeti sljedeći upit koji se navode značajke pakiranja JDK i poslužitelj aplikacije koje će biti automatski implementirano da biste pokrenuli aplikaciju.
    ![][ic789598]

    Taj se način prečac omogućuje brz i jednostavan način da biste testirali vaše aplikacije u Azure bez potrebe za konfiguriranje određene poslužitelja ili JDK koji se razlikuje od zadane vrijednosti. Ako ste zadovoljni zadane vrijednosti, možete pritisnite **u redu** za nastavak sa sljedećim koracima.
    Međutim, ako želite promijeniti u JDK ili application server za korištenje aplikacije, to možete učiniti kasnije uređivanjem projekta Azure implementaciju koji je automatski stvoriti ili možete odmah kliknite **Odustani** , a zatim pročitajte **Azure o implementaciji projekata odjeljak** ovog praktičnog vodiča.
1. U dijaloškom okviru **Objavi na Azure** :
    1. Ako nema pretplate na popisu **Pretplata** odaberite još, slijedite ove korake da biste uvezli informacija o pretplati:
        1. Kliknite **Uvezi iz datoteke za OBJAVLJIVANJE postavke**.
        1. U dijaloškom okviru **Informacije o uvozu pretplate** kliknite **Preuzmi datoteku OBJAVI postavke**. Ako niste prijavljeni u račun za Azure, zatražit će se da biste se prijavili. Potom ćete morati Spremi programa Azure objavljivanje datoteka postavki. Spremite je na lokalno računalo.
        1. I dalje u dijaloškom okviru **Informacije o uvozu pretplate** kliknite gumb **Pregledaj** , odaberite datoteku postavke Objavi koju ste spremili na lokalno računalo u prethodnom koraku pa kliknite **Otvori**. Zaslon trebao bi izgledati otprilike ovako: ![][ic644267]
        1. Kliknite **u redu**.
    1. Za **pretplate**odaberite pretplatu u koju želite koristiti za implementaciju sustava.
    1. Za **pohranu računa**odaberite račun za pohranu koji želite koristiti ili kliknite **Novo** da biste stvorili novi račun za pohranu.
    1. Za **naziv servisa**, odaberite servisa u oblaku koji želite koristiti ili kliknite **Novo** da biste stvorili novi servis u oblaku.
    1. Za **Ciljnu OS**, odaberite verziju operacijski sustav koji želite koristiti za implementaciju sustava.
    1. **Cilj okruženje**za potrebe ovog praktičnog vodiča odaberite **pripremna**. (Kada ste spremni za implementaciju web-mjestu radnog, promijenit ćete to da biste **radni**.)
    1. Neobavezno: Provjerite da **prebrisali prethodno implementacije** je li potvrđen okvir ako želite da novi uvođenja da biste automatski prebrisali prethodno implementacije. Kada omogućite tu mogućnost, će sučelja "409 sukoba" problemi s pri objavljivanju na isto mjesto.
        Imajte na umu da dijaloški okvir **Objavi u Azure** sadrži odjeljak za **Daljinski pristup**. Prema zadanom daljinski pristup nije omogućena, a ne možemo neće moći ga u ovom primjeru. Omogućivanje daljinskog pristupa unijet korisničko ime i lozinku za korištenje prilikom daljinski prijave u. Dodatne informacije o daljinski pristup potražite u članku [Omogućivanje daljinskog pristupa za Azure implementacije u Eclipse][].
        **Objavi na Azure** dijalog izgledat će otprilike ovako: ![][ic719488]
1. Kliknite **Objavi** da biste objavili pripremna okruženje.
    Kada se to od vas zatraži da biste izvršili cijelog Sastavi, kliknite **da**. To može potrajati nekoliko minuta za prvu Sastavi.
    **Zapisnik aktivnosti Azure** će se prikazivati u sekciji Eclipse karticama prikaza.
    ![][ic719489]
   Ovaj zapisnik, kao i u prikazu **konzole** , možete koristiti da biste vidjeli tijeku implementaciju sustava. Alternative je prijaviti na [Portal za upravljanje Azure][]i praćenje stanja pomoću sekcija **Servise u Oblaku** .
1. Kada uspješno je implementiran implementaciju sustava, **Zapisnik aktivnosti Azure** prikazivat će se status **objavljena**. Kliknite **objavljena**, kao što je prikazano na sljedećoj slici i pregledniku otvorit će se instance komponente implementaciju sustava.
    ![][ic719490]

Budući da je implementacije pripremna okruženje, naziv DNS bit će se u obliku http://&lt;*guid*&gt;. cloudapp.net pa će URL-a sadrže DNS naziva plus sufiks aplikacije. Na primjer, http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. (Dio **MyHelloWorld** je velika i mala slova). Naziv DNS možete vidjeti i ako kliknete naziv implementacije u Portal za upravljanje platforme Azure (unutar servisa u Oblaku dio portal za upravljanje).

Iako ovaj walk-through je za implementaciju pripremna okruženje, implementacije radnog slijedi iste korake, osim u dijaloški okvir **Objavi u Azure** , odaberite **radnog** umjesto **pripremna** **okruženje cilj**. Implementacija radnog rezultate u URL-a na temelju DNS naziv po izboru, umjesto GUID kako je koristiti za pripremna.

>[AZURE.WARNING] Sada ste implementiran Azure aplikacije u oblak. Međutim, prije nastavka, shvatite da distribuiranih aplikacije, čak i ako nije pokrenut, će nastaviti ako skupi vrijeme za naplatu za vašu pretplatu. Stoga iznimno važno je izbrisati neželjene implementacijama iz pretplate Azure.

## <a name="about-azure-deployment-projects"></a>O projektima Azure implementacije ##

Da biste implementirali jednu ili više jezika Java aplikacija za Azure, potrebno je za implementaciju projekt Azure. Reprodukcije uloga u "package" koje je potrebno omotan u da bi se objaviti Azure aplikacija.

Osim o aplikacija, projekta programa Azure implementacije sadrži i podatke o drugim ključne komponente uvođenja, većina važnije: spremnik za poslužitelj aplikacije da biste pokrenuli web app u te runtime Java da bi se pokrenuti na. Azure podržava velik izbor Java runtimes i poslužitelji aplikacija Java možete odabrati.

Iako se primjeru koristi ovdje uvelike pojednostavljuje u obrazovne svrhe, projekta programa Azure implementacije može sadržavati i druge informacije o konfiguraciji važne koji vam omogućuje stvaranje servise u oblaku gotovo arbitrarily složene, prilagodljivi, vrlo dostupna, više razina s aplikacijama sustava. Možete omogućiti **sesiju afinitet ("ljepljive sesije")**, **brzo predmemoriranja**, **daljinsko uklanjanje programskih pogrešaka**, **SSL rasteretite**, **vatrozid/priključak usmjeravanja**, **daljinski pristup**i broj druge napredne mogućnosti.

Ako ste dovršili prethodnom odjeljku ovog praktičnog vodiča ("za implementaciju aplikacije da biste Azure, brz i jednostavan način"), prikazat će se sada novi projekt Azure implementacije u Project Explorer automatski generira za vas i pod nazivom "**MyHelloWorld_onAzure**".

Nije moguće ste i pokrenuli ovog praktičnog vodiča tako da najprije stvorite prazan Azure implementacije projekta sami, a zatim dodate vaše aplikacije u nju. Na dulja je postupak, ali vam daje veću kontrolu nad Početna konfiguracija od početka.

Da biste stvorili novi projekt Azure implementacije ispočetka, kliknite gumb **Novi projekt implementacije Azure** ![][ic710876].

Neovisno o tome radite već postojeći projekt Azure implementaciju, ili stvaranje jednog od nule, prikazuje vam se da biste promijenili njegove postavke konfiguracije i komponente, kao što su u JDK ili poslužitelj aplikacije ravnomjerno jednostavno u bilo kojem trenutku.

Da biste promijenili JDK, ili poslužitelju aplikacije ili na popisu aplikacija u postojeći projekt Azure implementacije:

1. Proširite čvor projekta (npr. **MyHelloWorld_onAzure**) u programu Project Explorer
2. Desnom tipkom miša kliknite **WorkerRole1**
3. Proširite podizbornik **Azure** na kontekstnom izborniku
4. Kliknite **konfiguraciju poslužitelja**

Bez obzira je li ste pokrenuli korake za konfiguraciju poslužitelja tako da uredite postojeći projekt Azure implementacije kao što je prikazano gore ili stvorite novi od nule, prikazat će se iste vrste dijalozi omogućujući vam da biste konfigurirali JDK, poslužitelj i aplikacija komponente. Da biste saznali kako promijeniti postavke u tim dijaloškim okvirima, primjerice da biste promijenili JDK, poslužitelj aplikacije i dodavanje i uklanjanje aplikacija uvođenja više u članku [Svojstva za konfiguraciju poslužitelja][] .

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>Samo u sustavu Windows: za implementaciju aplikacije da biste emulator računalnim ##

>[AZURE.NOTE] Azure emulator dostupna je samo u sustavu Windows. Ako koristite operacijski sustav koji nije Windows, preskočite ovaj odjeljak.

Ako ste stvorili novi projekt Azure implementacije slijedeći korake opisane neke starije verzije, odnosno implicitno objavljivanjem na aplikaciju Azure, JDK i aplikacije poslužitelji su konfigurirane u oblak, ali ne i za lokalne emulacija. Da biste pripremili projekta za testiranje u lokalnom emulator, slijedite ove korake:

1. U Eclipse na Project Explorer kliknite **MyHelloWorld_onAzure**.
1. Desnom tipkom miša kliknite **WorkerRole1**.
1. Proširite podizbornik **Azure** na kontekstnom izborniku.
1. Kliknite **konfiguraciju poslužitelja**.
1. Na kartici **JDK** potvrdite ako kompleta alata za unaprijed konfigurirao zadani lokalni JDK umjesto vas. Ako ne ili ako želite promijeniti pretpostavljenom zadane postavke, provjerite je li **koristiti JDK iz ovaj put do datoteke za testiranje lokalno** je potvrđen okvir i željeno mjesto instalacije JDK nije naveden. Ako želite promijeniti, kliknite gumb **Pregledaj** , a pomoću kontrola Pregledaj, odaberite mjesto direktorija JDK da biste koristili.
1. Kliknite karticu **poslužitelja** .
1. U tekstni okvir **put lokalni poslužitelj** , pri dnu dijaloškog okvira, unesite put lokalno instaliran poslužitelja koja odgovara vrsti i glavna verzija broj poslužitelju koji je odabran pri vrhu dijaloškog okvira, u odjeljku potvrdni okvir **uvođenja na poslužitelju te vrste** . Ako želite koristiti različite vrste ili glavna verzija poslužitelja aplikacija, najprije promijenite odabir u odjeljku taj potvrdni okvir.
1. Kliknite **u redu**.
1. Na alatnoj traci Eclipse kliknite gumb **pokreću se u Azure Emulator** ![][ic710879]. Ako gumb **pokreću se u Azure Emulator** nije omogućena, provjerite je li **MyHelloWorld_onAzure** odabrana u Eclipse na Project Explorer te provjerite imaju li Eclipse na Project Explorer fokus kao trenutnom prozoru. To najprije pokrenut će cijeli Sastavi projekta i pokretanje Java web-aplikaciju u emulator računalnim. (Bilješke da ovisno o karakteristikama performanse računala, prvi sastavljanje može potrajati od nekoliko sekundi za nekoliko minuta, ali kasnije sastavlja će se brže.) Nakon dovršetka u prvom koraku Sastavi, zatražit će se tako da Windows korisnički račun kontrole (kontrolu korisničkih RAČUNA) da biste omogućili ovu naredbu da biste uredili na vašem računalu. Kliknite **da**.

>[AZURE.IMPORTANT] Ako ne vidite zatraži kontrolu korisničkih RAČUNA, potvrdite programska traka sustava Windows za ikonu kontrolu korisničkih RAČUNA, a zatim ga najprije kliknite. Ponekad u kontrolu korisničkih RAČUNA upit neće se prikazivati kao prvu prozor, ali je vidljiv samo kao ikona na programskoj traci.

1. Pregledajte izlaz emulator računalnim korisničkog Sučelja da biste odredili ima li problema s vašim projektom. Ovisno o sadržaj uvođenja može potrajati nekoliko minuta za svoju aplikaciju potpuno rada unutar emulator računalnim.
1. Pokretanje preglednika i korištenje URL-a `http://localhost:8080/MyHelloWorld` kao adresu (u `MyHelloWorld` dio URL-a je velika i mala slova). Trebali biste vidjeti MyHelloWorld aplikacije (Izlaz index.jsp), slično kao na sljedećoj slici: ![][ic589579]

Kada ste spremni da biste prestali vaše aplikacije u emulator računalnim, na alatnoj traci Eclipse kliknite gumb **Vrati Emulator Azure** ![][ic710880].

## <a name="to-delete-your-deployment"></a>Da biste izbrisali implementaciju sustava ##

Da biste izbrisali uvođenja unutar komplet alata za Azure za Eclipse, provjerite je li **MyHelloWorld_onAzure** odabrana u Eclipse na Project Explorer, provjerite je li Eclipse Project Explorer sadrži trenutnom prozoru fokus, a zatim kliknite gumb **Poništi objavu** ![][ic710883], na alatnoj traci Eclipse. (Nije obavljate isti postupak po **MyHelloWorld_onAzure** u Eclipse na Project Explorer desnom tipkom miša, klikom **Azure** , a zatim kliknite **Undeploy iz oblaka Azure**.) Time će se prikazati dijaloški okvir **Poništavanje objave Azure projekta** .

![][ic719491]

Odaberite servis pretplate i oblaka koja sadrži implementaciju sustava, odaberite uvođenja koje želite izbrisati, a zatim **Poništi objavu**.

(Alternative korištenje kompleta alata za da biste izbrisali implementacijskih je koristiti **Servise u Oblaku** dio Portal za upravljanje Azure: idite na implementaciju sustava, odaberite ga, a zatim gumb **Izbriši** . To će zaustaviti, a zatim izbrišite uvođenje. Ako samo želite prestati implementaciju, a ne i izbrisati, kliknite gumb **Zaustavi** umjesto gumb **Izbriši** , ali kao što je rečeno iznad, ako nehotice izbrišete implementaciju, naknade za naplatu bit će ako skupi za implementaciju sustava čak i ako je zaustavljen).

## <a name="see-also"></a>Vidi također ##

[Azure komplet alata za Eclipse][]

[Instalacija alata za Azure za Eclipse][] 

[Što je novo u Azure komplet alata za Eclipse][]

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java][].

<!-- URL List -->

[Razvojni centar za Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547
[Portal za upravljanje Azure]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure komplet alata za Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Omogućivanje daljinskog pristupa za Azure implementacije u Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699538
[Instalacija alata za Azure za Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Svojstva poslužitelja za konfiguraciju]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Što je novo u Azure komplet alata za Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png
