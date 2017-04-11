<properties
    pageTitle="Kako koristiti kontrole pristupa (Java) | Microsoft Azure"
    description="Saznajte kako razviti i koristiti kontrolu pristupa s Java u Azure."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-authenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Upute za provjeru autentičnosti korisnika weba uslugom kontrola pristupa Azure pomoću Eclipse

Ovaj vodič vidjet ćete kako koristiti u Azure pristup kontrola servisa (ACS) unutar komplet alata za Azure za Eclipse. Dodatne informacije o ACS potražite u odjeljku [sljedeće korake](#next_steps) .

> [AZURE.NOTE]
> Kontrola filtar Azure Access Services je pretpregled tehnologije zajednice. Kao predizdanje softver, on je formally Microsoft ne podržava.

## <a name="what-is-acs"></a>Što je ACS?

Većina razvojnim inženjerima nisu stručnjaka za identitet i obično ne želite potrošiti vrijeme razvijanje provjere autentičnosti i autorizacije mehanizme za svoje aplikacije i servise. ACS je Azure servis koji omogućuje jednostavan način provjere autentičnosti korisnika koji moraju imati pristup web-aplikacijama i servisima bez potrebe za logiku složene provjere autentičnosti varijance u kodu.

Sljedeće su značajke dostupne u ACS:

-   Integracija s programom Windows Foundation identiteta (WIF).
-   Podrška za davatelji popularnih web identiteta (IP-ovi) uključujući Windows Live ID, Google, Yahoo! i Facebook.
-   Podrška za Active Directory Federation Services (AD FS) 2.0.
-   Programa protokola Open Data (OData) – temelji servisa za upravljanje koja omogućuje programatski pristup ACS postavke.
-   Portal za upravljanje koji omogućuje Administrativni pristup ACS postavke.

Dodatne informacije o ACS potražite u članku [2.0 Service kontrole programa Access][].

## <a name="concepts"></a>Koncepti

Azure ACS se temelji na upravitelji utemeljene na zahtjevima identiteta – dosljednog pristup za stvaranje Mehanizmi za provjeru autentičnosti za aplikacije koji se izvodi lokalno i u oblaku. Identitet utemeljene na zahtjevima omogućuje uobičajenih aplikacija i servisa potrebne informacije koje su im potrebne o identitetu korisnicima unutar svoje tvrtke ili ustanove, u drugim tvrtkama ili ustanovama i na Internetu.

Za dovršenje zadatka u ovom vodiču upoznati konceptima sljedeće:

**Klijent** – u kontekstu ovaj vodič s uputama, to je preglednika koji se pokušava ostvariti pristup web-aplikacije.

**Aplikacije za zabavu (to) Relying** – aplikacije programa to je web-mjesto ili servis koji outsources provjera autentičnosti za jednu vanjskog izvora. U žargon identiteta, ne možemo izgovorite na to smatra pouzdanima te za izdavanje certifikata. Ovaj vodič objašnjava kako konfigurirati aplikaciju pouzdanost ACS.

**Tokena** - token zbirka je sigurnost podataka koja se obično izdaje nakon uspješne provjere autentičnosti korisnika. Sadrži skup *zahtjevima*, atribute korisnika čija je autentičnost provjerena. Zahtjev može se odnositi na korisničko ime, identifikator za ulogu korisnik pripada, dobi korisnika i tako dalje. Token je obično digitalno potpisan, što znači da ga možete uvijek biti izvorni termini natrag na njegov izdavač i njezin sadržaj ne neovlašteno. Korisnik poboljšava se pristup aplikaciji za to tako da prezentaciju valjani token izdala za izdavanje certifikata koje aplikacije to smatra pouzdanima.

**Davatelja identiteta (IP)** – An IP je za izdavanje certifikata koje potvrđuje identitetima korisnika i problemi sigurnosnih tokena. Stvarni posao od izdavanja tokeni je implementirana iako posebno servisa naziva sigurnosni Token servisa (STS). Uobičajeni IP-ovi Primjeri Windows Live ID, Facebook, tvrtke spremištima korisnika (kao što je Active Directory) i tako dalje.
Kada ACS konfiguriran pouzdanosti IP, sustav će prihvatiti i provjeriti tokena koje izdaje te IP. ACS pouzdani više IP-ovi odjednom, što znači da kada aplikacija smatra pouzdanima ACS, možete trenutačno ponuditi aplikacije da biste sve autorizirane korisnike iz svih na IP-ovi koje smatra pouzdanima ACS u vaše ime.

**Vanjski pristup davatelja (FP)** – IP-ovi Izravni korisnika, i autentičnost ih pomoću vjerodajnica kao i problema zahtjevima o znaju o njima. Na davatelja vanjski pristup (FP) se različite vrste izdavanje: umjesto izravno provjere autentičnosti korisnika, on se ponaša kao privremene i Brokerske djelatnosti autentičnosti između jedan to i jednu ili više IP-ovi. IP-ovi i FPs problema sigurnosnih tokena, dakle oba koriste sigurnosni Token Services (STS). ACS je jedan FP.

**Modul pravila ACS** – logike koji se koristi za pretvorbu dolazne tokena iz pouzdanog IP-ovi za tokeni Predviđeni da se koriste na to je codified u obliku jednostavne zahtjevima transformaciju pravila. ACS značajke modul pravila koja vodi brigu o primjene bilo kakve transformaciju logike koje ste naveli za vaše to.

**Prostor naziva ACS** - prostor naziva je particija najviše razine od ACS koju koristite za organiziranje postavki. Prostor naziva sadrži popis IP-ovi smatrate pouzdanim, to aplikacije koji se želite služiti, pravila očekujete pravilo modul za obradu dolazne tokeni s i tako dalje. Prostor naziva izlaže različite krajnje točke koja će se koristiti aplikaciju i programer da biste dobili ACS za izvođenje svoju funkciju.

Sljedeća slika prikazuje kako funkcionira provjera autentičnosti ACS s web-aplikacije:

![Dijagram toka ACS][acs_flow]

1.  Klijent (u ovom slučaju pregledniku) zahtijeva stranicu iz na to.
2.  Budući da zahtjev nije još provjere autentičnosti na to preusmjerava korisnika za izdavanje certifikata koje se smatra pouzdanima, koji je ACS. Na ACS predstavlja korisnika s mogućnošću IP-ovi koje je odredio za ovaj to. Korisnik odabere odgovarajuće IP.
3.  Klijent pretraživanjem dolazi do stranice na IP provjere autentičnosti i traži od korisnika da biste se prijavili.
4.  Nakon što klijent provjere autentičnosti (na primjer, identitet unose se vjerodajnice) na IP problemi sigurnosnog tokena.
5.  Nakon izdavanja sigurnosnog tokena na IP preusmjerava klijent ACS i klijent šalje sigurnosni token izdala na IP ACS.
6.  ACS provjerava sigurnosni token izdala IP, unosa identitet zahtjeve u ovaj token u modul pravila ACS, izračunava zahtjeve za identitetom izlazne i problemi novi sigurnosni token koji sadrži ove zahtjevima za izlaz.
7.  ACS preusmjerava klijent za to. Klijent šalje novi sigurnosni token izdala ACS na za to. Na to Provjeri valjanost potpisa na sigurnosni token izdala ACS, Provjeri valjanost zahtjevima u ovaj token i vraća stranicu koja je zatraženo.

## <a name="prerequisites"></a>Preduvjeti

Dovršavanje zadataka u ovom vodiču, će vam sljedeće:

- Na Java za razvojne inženjere Kit (JDK), v 1,6 ili noviji.
- Eclipse IDE za razvojne inženjere EE Java, indigo plava ili noviji. To se može preuzeti iz <http://www.eclipse.org/downloads/>. 
- Distribucija utemeljena na web-poslužitelj ili poslužitelj aplikacije, kao što su Apache Tomcat, GlassFish, JBoss poslužitelj aplikacije ili Jetty.
- Azure pretplatu, koje možete dobivene iz <http://www.microsoft.com/windowsazure/offers/>.
- Komplet alata za Azure za Eclipse, Travanj 2014 otpustite ili noviji. Dodatne informacije potražite u članku [Instaliranje alata za Azure za Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx).
- Certifikat X.509 će se koristiti za svoju aplikaciju. Morat ćete certifikat u javni certifikat (.cer) i Razmjena osobnih podataka (. Oblik PFX). (Mogućnosti za stvaranje ovog certifikata bit će opisane u nastavku ovog praktičnog vodiča).
- Poznavanje programa na Azure izračunati emulator i implementaciju tehnike koji su prikazani na [Stvaranje pozdrav svijeta aplikacije za Azure u Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx).

## <a name="create-an-acs-namespace"></a>Stvaranje programa ACS prostor naziva

Da biste počeli koristiti pristup kontrola servisa (ACS) u Azure, morate stvoriti programa ACS prostor naziva. Prostor naziva daje jedinstveni opseg adresiranja ACS resursa iz aplikacije.

1. Prijava na [Portal za upravljanje Azure][].
2. Kliknite **servisa Active Directory**. 
3. Da biste stvorili novi prostor za naziv kontrole pristupa, kliknite **Novo**, kliknite **Aplikacije servisa**, kliknite **Kontrolu pristupa**, a zatim **Brzo stvaranje**. 
4. Unesite naziv za prostor za naziv. Azure potvrđuje da je jedinstveni naziv.
5. Odaberite područje u kojem se koristi naziva. Da biste postigli najbolje performanse, koristite područje u kojem su implementacija aplikacije.
6. Ako imate više pretplata, odaberite pretplatu u koju želite koristiti za ACS naziva.
7. Kliknite **Stvori**.

Azure stvara i aktivira naziva. Pričekajte dok se ne **aktivan status novi prostor za naziv prije nastavka** . 

## <a name="add-identity-providers"></a>Dodavanje davatelji identiteta

U ovom ćete zadatku dodavanje IP-ovi će se koristiti za to aplikacija za provjeru autentičnosti. Demonstracija svrhe ovaj zadatak prikazuje kako dodati Windows Live kao IP, ali ne može koristiti bilo koji od na IP-ovi naveden na portalu za upravljanje ACS.


1.  [Portal za upravljanje Azure][]kliknite **Servisa Active Directory**, odaberite prostor naziva za kontrolu pristupa, a zatim **Upravljanje**. Otvorit će se na Portal za upravljanje ACS.
2.  U lijevom navigacijskom oknu Portal za upravljanje ACS kliknite **davatelji identiteta**.
3.  Windows Live ID po zadanom je omogućena, a nije moguće izbrisati. Za potrebe ovog praktičnog vodiča, koristi se samo Windows Live ID-a. Ovaj zaslon je gdje možete dodati druge IP-ovi, tako da kliknete gumb **Dodaj** .

Windows Live ID-a sada je omogućen kao IP za prostor za naziv ACS. Zatim navedite Java web-aplikacije (će biti stvoren kasnije) kao za to.

## <a name="add-a-relying-party-application"></a>Dodavanje relying proizvođača aplikacije

U ovom ćete zadatku konfigurirati ACS prepoznati web-aplikacije Java kao valjani to aplikacija.

1.  Na portalu za upravljanje ACS kliknite **Relying proizvođača aplikacije**.
2.  Na stranici **Potrebe za oslanjanjem proizvođača aplikacije** kliknite **Dodaj**.
3.  Na stranici **Dodavanje proizvođača aplikacije za potrebe za oslanjanjem** , učinite sljedeće:
    1.  U odjeljak **naziv**upišite naziv za to. Za potrebe ovog praktičnog vodiča, upišite **Azure Web App**.
    2.  U **načinu rada**, odaberite **unesite postavke ručno**.
    3.  U **područje**upišite URI na koju se odnosi sigurnosni token izdala ACS. Ovaj zadatak upišite **http://localhost:8080 /**.
        ![Potrebe za oslanjanjem lokalni proizvođača za korištenje u računalnim emulator][relying_party_realm_emulator]
    4.  Povratak u **URL-** u, upišite URL-a na koji ACS vraća sigurnosni token. Za taj zadatak upišite **http://localhost:8080/MyACSHelloWorld/index.jsp**
        ![Relying strana vratili URL-a za korištenje u računalnim emulator][relying_party_return_url_emulator]
    5.  Prihvatite zadane vrijednosti u ostatku polja.

4.  Kliknite **Spremi**.

Sada uspješno ste konfigurirali web-aplikacije Java prilikom pokretanja u emulator Azure računalnim (pri http://localhost:8080 /) da biste se na to u prostor za naziv ACS. Nakon toga stvorite pravila koja koristi ACS obrade zahtjeva za na to.

## <a name="create-rules"></a>Stvaranje pravila

U ovom ćete zadatku definirati pravila koji utječu na način zahtjevima prenose se s IP-ovi sustava to. Za potrebe ovog vodiča ne možemo jednostavno konfigurirati ACS da biste kopirali Vrsta unosa zahtjeva i vrijednosti izravno u token Izlaz bez filtriranja ili izmjena ih.

1.  Na glavnoj stranici Portal za upravljanje ACS kliknite **pravila grupe**.
2.  Na stranici **Pravila grupe** , kliknite **Zadana pravila grupe za Azure web-aplikacije**.
3.  Na stranici **Uređivanje pravila grupe** kliknite **Generiraj**.
4.  Na na **pravila za generiranje: zadane grupe pravila za Azure web-aplikaciji** stranica, provjerite je li potvrđen okvir Windows Live ID, a zatim kliknite **Generiraj**.    
5.  Na stranici **Uređivanje pravila grupe** , kliknite **Spremi**.

## <a name="upload-a-certificate-to-your-acs-namespace"></a>Prijenos certifikat prostora za naziv ACS

U ovom ćete zadatku prenijeti na. PFX certifikat koji će se koristiti za potpisivanje tokena zahtjeve stvorio prostora za naziv ACS.

1.  Na glavnoj stranici Portal za upravljanje ACS kliknite **certifikata i ključeva**.
2.  Na stranici **potvrda i tipke** , kliknite **Dodaj** iznad **Tokena potpisivanja**.
3.  Na stranici **Dodavanje tokena potpisni certifikat ili ključ** :
    1. U odjeljku **korišteni za** kliknite **Potrebe za oslanjanjem aplikacija** , a zatim odaberite **Azure Web App** (koji ste prethodno nazivu relying proizvođača aplikacije).
    2. U odjeljku **Vrsta** odaberite **Certifikat X.509**.
    3. U odjeljku **potvrde** kliknite gumb Pregledaj, a zatim otvorite datoteku certifikat X.509 koji želite koristiti. Ta će se na. Datoteka PFX. Odaberite datoteku, kliknite **Otvori**, a zatim u tekstni okvir **Lozinka** unesite lozinku za potvrdu. Imajte na umu da u svrhe testiranja možda koristi samostalno-signed-certifikat. Da biste stvorili samopotpisani certifikat, koristite gumb **Novo** u dijaloškom okviru **ACS filtar biblioteke** (opisan u nastavku), ili uslužni **encutil.exe** iz sustava [project web-mjesto][] Starter Kit Azure za Java.
    4. Provjerite da **Provjerite primarni** je li potvrđen okvir. Na stranicu **Dodavanje tokena potpisni certifikat ili ključ** trebao bi izgledati otprilike ovako.
        ![Dodavanje certifikata za potpisivanje tokena][add_token_signing_cert]
    5. Kliknite **Spremi** da biste spremili postavke, a zatim zatvorite stranicu **Dodavanje tokena potpisni certifikat ili ključ** .

Nakon toga pregledajte podatke na stranici integraciju aplikacija i kopirajte URI koji morat ćete konfigurirati Java web-aplikacije da biste koristili ACS.

## <a name="review-the-application-integration-page"></a>Pregledajte stranicu integraciju aplikacija

Možete pronaći sve podatke i kod nužnih za konfiguriranje jezika Java web-aplikacije (to aplikacija) za rad s ACS na stranici portala za upravljanje ACS integraciju aplikacija. Ove informacije ćete prilikom konfiguriranja web-aplikacije Java za vanjsko provjeru autentičnosti.

1.  Na portalu za upravljanje ACS kliknite **integraciju aplikacija**.  
2.  Na stranici **Integraciju aplikacija** kliknite **Stranica za prijavu**.
3.  Na stranici **Prijava stranice Integracija** kliknite **Azure Web App**.

U na **Integracija stranice za prijavu: Azure web-aplikacije** stranica, URL-a na popisu **mogućnost 1: veza na stranicu s ACS hostira prijava** koristit će se u web-aplikacije Java. Kada dodate biblioteku Azure pristup kontrola servisa filtra u aplikaciji Java trebat će vam ta vrijednost.

## <a name="create-a-java-web-application"></a>Stvaranje web-aplikacije Java
1. Unutar Eclipse, na izborniku kliknite **datoteka**, kliknite **Novo**, a zatim **Dinamički Web projekta**. (Ako ne vidite **Dinamički Project Web** navedena kao dostupne projekta nakon klika na **datoteku**, **Novi**, učinite sljedeće: kliknite **datoteka**, kliknite **Novo**, kliknite **projekt**, proširite **Web**, kliknite **Dinamički Project Web**i kliknite **Dalje**.) Za potrebe ovog praktičnog vodiča, nazovite projekta **MyACSHelloWorld**. (Provjerite koristiti taj naziv, sljedeće korake ovog praktičnog vodiča očekivati WAR datoteku da biste se s nazivom MyACSHelloWorld). Zaslon izgledat će otprilike ovako:

    ![Stvaranje projekta pozdrav svijeta za ACS exampple][create_acs_hello_world]

    Kliknite **Završi**.
2. U prikazu programa Project Explorer Eclipse, proširite **MyACSHelloWorld**. Desnom tipkom miša kliknite **WebContent**, kliknite **Novo**, a zatim kliknite **Datoteke JSP**.
3. U dijaloškom okviru **Nova datoteka JSP** naziv datoteke **index.jsp**. Zadrži nadređenu mapu kao MyACSHelloWorld/WebContent, kao što je prikazano na sljedećim mjestima:

    ![Dodavanje datoteke JSP, primjerice ACS][add_jsp_file_acs]

    Kliknite **Dalje**.

4. U dijaloškom okviru **Odabir predloška JSP** odaberite **Novu datoteku JSP (html)** , a zatim kliknite **Završi**.
5. Kada index.jsp datoteka se otvara u Eclipse, dodajte tekst za prikaz **Pozdrav ACS svijete!** unutar postojeći `<body>` element. Vaš ažurirani `<body>` sadržaj prikazivati kao sljedeće:

        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
    
    Spremite index.jsp.
  
## <a name="add-the-acs-filter-library-to-your-application"></a>Dodavanje biblioteke ACS filtar u aplikaciji

1. U Eclipse na Project Explorer desnom tipkom miša kliknite **MyACSHelloWorld**, kliknite **Sastavi put**, a zatim **Konfiguriranje sastavljanje put**.
2. U dijaloškom okviru **Sastavljanje put Java** kliknite karticu **Biblioteka** .
3. Kliknite **Dodavanje biblioteke**.
4. Kliknite **Filtar servisa Azure pristup kontrole (po MS Otvori Tehnički)** , a zatim kliknite **Dalje**. Prikazat će se dijaloški okvir **Azure pristup kontrola servisa filtar** .  (Polje **mjesto** možda drugi put, ovisno o tome koje ste instalirali Eclipse, a broj verzije mogu se razlikovati, ovisno o softverska ažuriranja).

    ![Dodavanje biblioteke ACS filtra][add_acs_filter_lib]

5. Pomoću preglednika otvoriti stranicu **Integracija prijava stranice** portala za upravljanje kopirajte URL na popisu u **mogućnost 1: veza na stranicu s ACS hostira prijava** polja i zalijepite ih u polje **Krajnja točka za provjeru autentičnosti ACS** Eclipse dijaloškog okvira.
6. Pomoću preglednika otvoriti stranicu **Uređivanje potrebe za oslanjanjem aplikacija** Portal za upravljanje, kopirajte URL koji je naveden u polju **Lokalni** web-mjesta i zalijepite u polje **Potrebe za oslanjanjem lokalni strana** Eclipse dijaloškog okvira.
7. U odjeljku **Sigurnost** dijaloškog okvira Eclipse, ako želite koristiti postojeći certifikat, kliknite **Pregledaj**, pronađite certifikat koji želite koristiti, odaberite ga i kliknite **Otvori**. Ili, ako želite da biste stvorili novu potvrdu, kliknite **Novo** da biste prikazali dijaloški okvir **Novi certifikat** , zatim navedite lozinku, naziv .cer datoteke i ime .pfx datoteka certifikata.
8. Provjerite **Ugradi certifikat u datoteci WAR**. Ugrađivanje certifikata na taj način obuhvaća ga u implementaciji sustava bez potrebe da ga ručno dodati kao komponentu. (Ako umjesto toga morate spremiti certifikat vanjsko WAR datoteke, možete dodavati polja kao uloga komponenta i poništite **Ugradi certifikat u datoteci WAR**.)
9. [Neobavezni] Ostavi **veze zahtijevaju HTTPS** potvrđen. Ako postavite tu mogućnost, morate pristupiti aplikaciji pomoću HTTPS protokola. Ako ne želite da je potrebna HTTPS veze, poništite tu mogućnost.
10. Za implementaciju računalnim emulator postavki **Filtra ACS Azure** će izgledati otprilike ovako.

    ![Azure postavke filtra ACS implementacije za emulator računalnim][add_acs_filter_lib_emulator]

11. Kliknite **Završi**.
12. Kliknite **da** kada upit s u dijaloški okvir koja govori da će stvoriti datoteku web.xml.
13. Kliknite **u redu** da biste zatvorili dijaloški okvir **Sastavljanje put Java** .

## <a name="deploy-to-the-compute-emulator"></a>Implementacija emulator računalnim

1. U Eclipse na Project Explorer desnom tipkom miša kliknite **MyACSHelloWorld**, kliknite **Azure**, a zatim **paket za Azure**.
2. U odjeljku **naziva projekta**unesite **MyAzureACSProject** , a zatim kliknite **Dalje**.
3. Odaberite na JDK i poslužitelj aplikacije. (Ove korake možete pronaći detaljno u ovom praktičnom vodiču za [Stvaranje pozdrav svijeta aplikacije za Azure u Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) ).
4. Kliknite **Završi**.
5. Kliknite gumb za **pokretanje u Azure Emulator** .
6. Kada web-aplikacije Java počne u emulator računalnim, zatvorite sve instance preglednika (tako da sve trenutne sesije preglednika ne ometaju prijavu testiranju ACS).
7. Pokrenite aplikaciju otvaranjem <> http://localhost:8080/MyACSHelloWorld/u pregledniku ( <> ili/https://localhost:8080/MyACSHelloWorld/ako je potvrđen okvir **zahtijevaju HTTPS veze**). Trebali biste upit za prijavu na Windows Live ID, a zatim koje treba uzeti povrata URL naveden relying proizvođača aplikacije.
99.  Kada ste gotovi s pregledom aplikaciju, kliknite gumb **Vrati Emulator Azure** .

## <a name="deploy-to-azure"></a>Implementacija Azure

Za implementaciju Azure, morat ćete promijeniti relying lokalni proizvođača i vratili URL-a za prostor za naziv ACS.

1. Unutar portala za upravljanje Azure, na stranici **Uređivanje potrebe za oslanjanjem aplikacija** izmijeniti **Lokalni** će se URL distribuiranih web-mjesta. **Primjer** zamijenite DNS ime koje ste naveli za implementaciju sustava.

    ![Potrebe za oslanjanjem lokalni proizvođača za korištenje u proizvodnje][relying_party_realm_production]

2. Izmjena **Vratili URL** će se URL aplikacije. **Primjer** zamijenite DNS ime koje ste naveli za implementaciju sustava.

    ![Potrebe za oslanjanjem strana povrata URL-a za korištenje u radnog][relying_party_return_url_production]

3. Kliknite **Spremi** da biste spremili ažurirane odgovaranje strana lokalni i povratna promjene URL-a.
4. **Prijava stranice Integracija** stranice ostati otvoren u pregledniku, morat ćete uskoro kopirati iz nje.
5. U Eclipse na Project Explorer desnom tipkom miša kliknite **MyACSHelloWorld**, kliknite **Sastavi put**, a zatim **Konfiguriranje sastavljanje put**.
6. Kliknite karticu **Biblioteka** , kliknite **Azure pristup kontrola servisa filtar**, a zatim **Uređivanje**.
7. Pomoću preglednika otvoriti stranicu **Integracija prijava stranice** portala za upravljanje kopirajte URL na popisu u **mogućnost 1: veza na stranicu s ACS hostira prijava** polja i zalijepite ih u polje **Krajnja točka za provjeru autentičnosti ACS** Eclipse dijaloškog okvira.
8. Pomoću preglednika otvoriti stranicu **Uređivanje potrebe za oslanjanjem aplikacija** Portal za upravljanje, kopirajte URL koji je naveden u polju **Lokalni** web-mjesta i zalijepite u polje **Potrebe za oslanjanjem lokalni strana** Eclipse dijaloškog okvira.
9. U odjeljku **Sigurnost** dijaloškog okvira Eclipse, ako želite koristiti postojeći certifikat, kliknite **Pregledaj**, pronađite certifikat koji želite koristiti, odaberite ga i kliknite **Otvori**. Ili, ako želite da biste stvorili novu potvrdu, kliknite **Novo** da biste prikazali dijaloški okvir **Novi certifikat** , zatim navedite lozinku, naziv .cer datoteke i ime .pfx datoteka certifikata.
10. Zadrži **Ugradi certifikat u datoteci WAR** potvrđen, uz pretpostavku da želite ugraditi certifikat u datoteci WAR.
11. [Neobavezni] Ostavi **veze zahtijevaju HTTPS** potvrđen. Ako postavite tu mogućnost, morate pristupiti aplikaciji pomoću HTTPS protokola. Ako ne želite da je potrebna HTTPS veze, poništite tu mogućnost.
12. Za implementaciju Azure postavke filtra ACS Azure će izgledati otprilike ovako.

    ![Azure postavke filtra ACS radnog implementacije][add_acs_filter_lib_production]

13. Kliknite **Završi** da biste zatvorili dijaloški okvir za **Uređivanje biblioteke** .
14. Kliknite **u redu** da biste zatvorili dijaloški okvir **Svojstva za MyACSHelloWorld** .
15. U Eclipse, kliknite gumb **Objavi oblak Azure** . Odgovaranje na upite, slično kao dovršen u odjeljku **za implementaciju aplikacije da biste Azure** temi [Stvaranje pozdrav svijeta aplikacije za Azure u Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) . 

Kada je postavila web-aplikacije zatvorite sve otvorene preglednika sesije, pokrenite web-aplikacije i koje treba zatražiti da se prijavite pomoću vjerodajnica za Windows Live ID, nakon čega slijedi koja se šalje na povrat URL relying proizvođača aplikacije.

Kada završite pomoću aplikacije za ACS pozdrav svijetu, imajte na umu da biste izbrisali implementacije (možete saznati kako izbrisati implementacije u temi [Stvaranje pozdrav svijeta aplikacije za Azure u Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) ).


## <a name="next_steps"></a>Daljnji koraci

Do analize od na sigurnost pridruživanju oznake jezika (SAML) vratio ACS u aplikaciji potražite [u][]članku prikaz SAML vratio servis za kontrolu pristupa Azure. Da biste dodatno Istraživanje ACS na funkcionalnost i Eksperimentirajte s više sofisticirane scenariji, potražite u članku [2.0 Service kontrole programa Access][].

Osim toga, u ovom se primjeru koristi mogućnost **Ugradi certifikat u datoteci WAR** . Ta mogućnost olakšava implementacija certifikata. Ako umjesto toga želite staviti potpisnog certifikata WAR datoteku, koristite sljedeći postupak:

1. U odjeljku **Sigurnost** dijaloškog okvira **Azure pristup kontrola servisa filtar** upišite $ **{env. JAVA_HOME}/mycert.cer** i poništite potvrdni okvir za **ugradnju certifikat u datoteci WAR**. (Mycert.cer Ako prilagodite naziv datoteke certifikata razlikuje.) Kliknite **Završi** da biste zatvorili dijaloški okvir.
2. Kopiranje certifikat kao komponenta u implementaciji sustava: U Eclipse na Project Explorer proširite **MyAzureACSProject**, desnom tipkom miša kliknite **WorkerRole1**, kliknite **Svojstva**, proširite **Azure ulogu**i kliknite **komponente**.
3. Kliknite **Dodaj**.
4. U dijaloškom okviru **Dodavanje komponente** :
    1. U odjeljku **Uvoz** :
        1. Pomoću gumba **datoteku** da biste došli do certifikat koji želite koristiti. 
        2. **Način**, odaberite **Kopiraj**.
    2. U odjeljku **Naziv**kliknite tekstni okvir i prihvatite zadani naziv.
    3. U odjeljku **uvođenja** :
        1. **Način**, odaberite **Kopiraj**.
        2. Da bi **direktorij**, upišite **JAVA_HOME %**.
    4. **Dodavanje komponente** dijalog trebao bi izgledati otprilike ovako.

        ![Dodavanje komponente certifikata][add_cert_component]

    5. Kliknite **u redu**.

Sada certifikata želite uvrstiti u implementaciji sustava. Imajte na umu da bez obzira na to jesu li ugrađeni certifikat u datoteci WAR ili dodati kao komponenta za implementaciju sustava, morate prenijeti certifikat za prostor za naziv kao što je opisano u odjeljku [Prijenos certifikata radi prostora za naziv ACS][] .

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Prijenos certifikat prostora za naziv ACS]: #upload-certificate
[Review the Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add the ACS Filter library to your application]: #add_acs_filter_library
[Deploy to the compute emulator]: #deploy_compute_emulator
[Deploy to Azure]: #deploy_azure
[Next steps]: #next_steps
[web-mjesto projekta]: http://wastarterkit4java.codeplex.com/releases/view/61026
[Kako prikazati SAML vratio servis za kontrolu pristupa Azure]: /en-us/develop/java/how-to-guides/view-saml-returned-by-acs/
[Servis za kontrolu pristupa 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Portal za upravljanje Azure]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png
 
