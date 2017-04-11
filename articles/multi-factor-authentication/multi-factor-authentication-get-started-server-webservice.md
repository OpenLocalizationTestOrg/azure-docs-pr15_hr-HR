<properties 
    pageTitle="Početak rada MFA poslužitelja mobilne aplikacije web-servisa"
    description="Aplikaciju Azure višestruke provjere autentičnosti nudi mogućnost dodatno unositi do dospijeća.  Omogućuje MFA poslužitelj za korištenje slanja obavijesti korisnicima."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="getting-started-the-mfa-server-mobile-app-web-service"></a>Početak rada MFA poslužitelja mobilne aplikacije web-servisa

Aplikaciju Azure višestruke provjere autentičnosti nudi mogućnost dodatno unositi do dospijeća. Umjesto da postavite automatskog telefonskog poziva ili SMS korisniku tijekom prijavljivanja, Azure višestruka provjera autentičnosti ih gura obavijest aplikaciju Azure višestruke provjere autentičnosti pametnog telefona ili tableta. Korisnik jednostavno dodiruje "Provjere autentičnosti" (ili unosi PIN-a i dodiruje "Provjere autentičnosti") u aplikaciji za prijavu.

Da biste koristili aplikaciju Azure višestruke provjere autentičnosti, sljedeće su potrebni bi aplikaciju uspješno možete komunicirati s mobilne aplikacije web-servisa:

- Pročitajte članak hardverski i softverski preduvjeti za hardverski i softverski preduvjeti
- Morate koristiti v6.0 ili noviji Azure višestruke provjere autentičnosti poslužitelja
- Mobilne aplikacije web-servisa mora biti instaliran na poslužitelju web-mjesto na Internetu koja se koristi Microsoft® Internet Information Services (IIS) IIS 7.x ili noviji.  Dodatne informacije o IIS potražite u članku [IIS.NET](http://www.iis.net/).
- ASP.NET v4.0.30319 provjerite je li instaliran, registrirana te postaviti dopuštene
- Potrebne uloga usluge obuhvaćaju ASP.NET i IIS 6 metabaze kompatibilnosti
- Mobilne aplikacije web-usluge mora imati pristup putem javnog URL-a
- Mobilne aplikacije web-servisa mora biti zaštićen s SSL certifikata.
- SDK za servisa Azure višestruke provjere autentičnosti Web mora biti instaliran u IIS 7.x ili noviji na poslužitelju koji Azure višestruku provjeru autentičnosti poslužitelja
- SDK za servisa Azure višestruke provjere autentičnosti Web mora biti zaštićenim s SSL certifikata.
- Mobilne aplikacije web-servisa moraju imati mogućnost povezivanje s SDK za servisa Azure višestruke provjere autentičnosti Web putem SSL
- Mobilne aplikacije web-servisa mora biti moguće provjeriti autentičnost Azure višestruke provjere autentičnosti Web servisa SDK pomoću vjerodajnica računa servisa koji pripada sigurnosne grupe pod nazivom "PhoneFactor administratori". Ovaj račun servisa i grupe postoje u servisu Active Directory ako Azure višestruku provjeru autentičnosti poslužitelja je pokrenut na poslužitelju domene pridružili. Ovaj račun servisa i grupe postoje lokalno na poslužitelju Azure višestruke provjere autentičnosti ako niste član domene.


Instaliranje korisnički portal na poslužitelju koji nije poslužitelj programa Azure višestruke provjere autentičnosti zahtijeva sljedeća tri koraka:

1. Instalirajte web-servisa SDK
2. Instalacija aplikacije za mobilne uređaje web-servisa
3. Konfiguriranje postavki aplikacije za mobilne uređaje u okvir poslužitelj za Azure višestruke provjere autentičnosti
4. Aktiviranje aplikaciju Azure višestruke provjere autentičnosti za krajnje korisnike

## <a name="install-the-web-service-sdk"></a>Instalirajte web-servisa SDK

Ako SDK za servisa Azure višestruke provjere autentičnosti Web već nije instaliran na poslužitelju Azure višestruke provjere autentičnosti, idite na tom poslužitelju i otvorite Azure višestruku provjeru autentičnosti poslužitelja. Kliknite ikonu Web servisa SDK, kliknite SDK za instaliranje Web servisa... gumb, a zatim slijedite navedene upute. Servis SDK Web mora biti zaštićenim s SSL certifikata. Samopotpisani certifikat je u redu u tu svrhu, ali je moguće uvesti u spremište "Izdavanje korijenskih" račun lokalnog računala na web-poslužitelju korisnički Portal tako da se smatrao pouzdanima taj certifikat prilikom pokretanja SSL veze.

<center>![Postavljanje](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## <a name="install-the-mobile-app-web-service"></a>Instalacija aplikacije za mobilne uređaje web-servisa
Prije instalacije mobilne aplikacije web-servisa, imajte na umu sljedeće:

- Ako Azure višestruke provjere autentičnosti korisnički Portal već instaliran na poslužitelju na mjesto na Internetu, korisničko ime, lozinku i URL-a servisa SDK Web možete kopirati iz datoteke web.config Portal za korisnika.
- Da biste otvorili web-pregledniku na poslužitelj za web-mjesto na Internetu i idite na URL SDK Web servisa koji je unesen u datoteci web.config korisno je. Ako web-pregledniku doći do web-servisa uspješno, ga treba zatražiti vjerodajnice. Unesite korisničko ime i lozinku koje su unijeli u datoteci web.config točno onako kako se prikazuju u datoteci. Provjerite je li se prikazuju bez certifikata upozorenja ili pogreške.
- Ako povratnog proxy ili vatrozid je koji se nalaze ispred web-poslužitelj mobilne aplikacije web-servisa i izvođenje SSL rasteretite, možete uređivati datoteku web.config mobilne aplikacije web-servisa i dodajte sljedeći ključ da biste na <appSettings> sekcije tako da se web-servisa za mobilne aplikacije umjesto https možete koristiti HTTP-a. No SSL i dalje potreban je iz mobilne aplikacije proxy vatrozid i obrnuto. <add key="SSL_REQUIRED" value="false"/>

### <a name="to-install-the-mobile-app-web-service"></a>Da biste instalirali web-servisa za mobilne uređaje

<ol>
<li>Otvorite Windows Explorer na poslužitelju Azure višestruke provjere autentičnosti, a zatim dođite do mape u kojem je instaliran na poslužitelj za Azure multi-factor provjere autentičnosti (primjerice C:\Program Files\Azure višestruka provjera autentičnosti). Odabir 32-bitnu ili 64-bitne verzije sustava Azure multi-factor AuthenticationPhoneAppWebServiceSetup instalacijsku datoteku po potrebi poslužitelja koji će se instalirati mobilne aplikacije web-servisa na. Kopirajte instalacijsku datoteku na poslužitelj za mjesto na Internetu.</li>

<li>Na poslužitelju web-mjesto na Internetu, instalacijska datoteka morate pokrenuti s administratorskim ovlastima. Najjednostavniji način za to je da biste otvorili naredbenog retka kao administrator i prijeđite na mjesto na koje će biti kopirana instalacijsku datoteku.</li>  

<li>Pokrenite multi-factor AuthenticationMobileAppWebServiceSetup instalacijsku datoteku, promijenite web-mjesta po želji te direktorija virtualne kratki naziv kao što su "PA". Naziv kratki virtualnog direktorija preporučuje se jer se korisnici moraju unijeti URL za mobilne aplikacije web-servisa na mobilnom uređaju tijekom aktivacije.</li>

<li>Nakon dovršetka instalacije višestruku AuthenticationMobileAppWebServiceSetup Azure, dođite do C:\inetpub\wwwroot\PA (ili odgovarajući direktorij na temelju naziv virtualnog direktorija), a zatim Uređivanje web.config datoteke.</li>  

<li>Pronađite tipki WEB_SERVICE_SDK_AUTHENTICATION_USERNAME i WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD i postavite vrijednosti na korisničko ime i lozinku za račun servisa koji pripada sigurnosti PhoneFactor administratori grupe (u odjeljku preduvjeti gore). To može biti isti račun koji se koristi kao identitet Azure višestruke provjere autentičnosti korisnički Portal ako koji je već instaliran. Pripazite da unesete korisničko ime i lozinku između navodnim znakovima na kraju retka (vrijednost = "" / >). Preporučuje se da biste koristili kvalificirani korisničko ime (primjerice domena/korisničko ime ili machine\username).</li>  

<li>Pronađite željenu postavku pfMobile App Web Service_pfwssdk_PfWsSdk i promijenite vrijednost "http://localhost:4898/PfWsSdk.asmx" URL SDK servisa Web koji se izvodi na poslužitelju višestruke provjere autentičnosti (npr. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx) Azure. Jer SSL se koristi za tu vezu, jer SSL certifikat će Izdana za naziv poslužitelja i URL koristi moraju se podudarati s nazivom na certifikat mora pozivati SDK servisa Web naziv poslužitelja i ne IP adresa. Ako naziv poslužitelja ne riješite IP adresu poslužitelja mjesto na Internetu, dodajte stavku domaćini datoteku na tom poslužitelju da biste mapirali naziv poslužitelja Azure višestruke provjere autentičnosti na njegovu IP adresu. Nakon što su promjene, spremite datoteku web.config.</li>  

<li>Ako mobilne aplikacije web-servisa je bio instaliran u odjeljku (npr. zadani Web-mjesta) web-mjesto nije već je binded s potvrdom javno prijavljeni, instalirati certifikat na poslužitelju ako nije već instaliran, otvorite upravitelj IIS i povežite certifikata na web-mjesto.</li>  

<li>Otvorite web-preglednika na bilo kojem računalu, a zatim otvorite instaliranim mobilne aplikacije web-servisa URL (npr. https://www.publicwebsite.com/PA). Provjerite je li se prikazuju bez certifikata upozorenja ili pogreške.</li>

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Konfiguriranje postavki aplikacije za mobilne uređaje u okvir poslužitelj za Azure višestruke provjere autentičnosti
Sad kad je instalirana web-servisa za mobilnu aplikaciju, morate konfigurirati Azure višestruke provjere provjeru autentičnosti poslužitelja za rad s portala sustava.

#### <a name="to-configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Konfiguriranje postavki aplikacije za mobilne uređaje u okvir poslužitelj za Azure višestruke provjere autentičnosti

1. Na poslužitelju Azure višestruke provjere autentičnosti, kliknite ikonu korisnički Portal. Korisnici će moći nadzirati njihove metoda provjere autentičnosti na kartici postavke u odjeljku Dopusti korisnicima da biste odabrali način, provjerite mobilnu aplikaciju. Bez Ova funkcija nije omogućena, krajnji korisnici će se morati obratiti stol pomoć da biste dovršili aktivaciju za mobilnu aplikaciju.
2. Potvrdite okvir Dopusti korisnicima da biste aktivirali okvir mobilne aplikacije.
3. Potvrdite okvir Dopusti Registracija korisnika.
4. Kliknite ikonu za mobilnu aplikaciju.
5. Unesite URL koji se koristi s virtualnog direktorija koji je stvoren prilikom instalacije višestruku AuthenticationMobileAppWebServiceSetup Azure. Naziv računa se može unijeti u prostoru. Taj je naziv tvrtke će se prikazivati u mobilnu aplikaciju. Ako ostavite prazno, prikazat će se naziv davatelja provjere autentičnosti višestruku koja je stvorena u Portal za upravljanje Azure.



<center>![Postavljanje](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>
