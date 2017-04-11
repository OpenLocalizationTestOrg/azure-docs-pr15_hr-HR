<properties 
    pageTitle="Implementacija korisnički portal Azure višestruke provjere autentičnosti poslužitelja"
    description="To je stranica Azure višestruke provjere autentičnosti koji opisuje način za početak rada s Azure MFA i korisnički portal."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="deploying-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Implementacija korisnički portal Azure višestruke provjere autentičnosti poslužitelja

Korisnički Portal omogućuje administrator da biste instalirali i konfigurirali korisnički Portal Azure višestruke provjere autentičnosti. Portal za korisnika je u IIS web-mjesto koje korisnicima omogućuje Primjena Azure višestruke provjere autentičnosti i održavanje njihove račune. Korisnik možda promijeniti njegov telefonski broj, promijeniti njihov PIN-a ili zaobići Azure višestruka provjera autentičnosti tijekom njihova sljedeći znak na.

Korisnici će prijavite se na portal korisnika pomoću njihovih normalni korisničko ime i lozinku te će dovršiti poziva Azure višestruke provjere autentičnosti ili sigurnosna pitanja da biste dovršili njihove provjere autentičnosti. Ako je registracija korisnika, korisnik će konfigurirati njihov broj telefona i PIN-a prva se prijaviti na Portal za korisnika.

Korisnik Portal za administratore mogu postaviti i imaju dozvolu za dodavanje novih korisnika i ažuriranje postojećim korisnicima.

<center>![Postavljanje](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## <a name="deploying-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>Implementacija korisnički portal na istom poslužitelju kao poslužitelj za Azure višestruke provjere autentičnosti

Sljedeće stara requisites su potrebni za instalaciju Portal korisnicima na istom poslužitelju kao poslužitelj za Azure višestruke provjere autentičnosti:

- IIS nije potrebno je instalirati uključujući asp.net i IIS 6 meta osnovni kompatibilnost (za IIS 7 ili noviji)
- Prijavljenog korisnik mora imati administratorska prava na računalu i domene ako je primjenjivo.  To je zato račun potrebne dozvole za stvaranje sigurnosnih grupa servisa Active Directory.

### <a name="to-deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Za implementaciju korisnički portal Azure višestruke provjere autentičnosti poslužitelja

1. Unutar Azure višestruke provjere autentičnosti poslužitelja: kliknite ikonu korisnički Portal na lijevom izborniku, kliknite instaliranje korisnički Portal.
1. Kliknite Dalje.
1. Kliknite Dalje.
1. Ako je računalo priključeno na domenu i konfiguriranje servisa Active Directory za osiguravanje komunikacije između korisnika Portal i servisa Azure višestruka provjera autentičnosti nije potpun, prikazat će se korak servisa Active Directory. Kliknite gumb Dalje da biste automatski dovršili konfiguraciju.
1. Kliknite Dalje.
1. Kliknite Dalje.
1. Kliknite Zatvori.
1. Otvorite web-preglednika na bilo kojem računalu, a zatim otvorite URL instaliranim korisnički Portal (npr. https://www.publicwebsite.com/MultiFactorAuth). Provjerite je li se prikazuju bez certifikata upozorenja ili pogreške.

<center>![Postavljanje](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploying-the-azure-multi-factor-authentication-server-user-portal-on-a-separate-server"></a>Implementacija Azure višestruka provjera autentičnosti poslužitelja korisnički Portal na istom poslužitelju

Da biste koristili aplikaciju Azure višestruke provjere autentičnosti, sljedeće su potrebni bi aplikaciju uspješno možete komunicirati s portala za korisnika:

Pročitajte članak hardverski i softverski preduvjeti za hardverski i softverski preduvjeti:

- Morate koristiti v6.0 ili noviji Azure višestruke provjere autentičnosti poslužitelja.
- Korisnički Portal mora biti instaliran na poslužitelju web-mjesto na Internetu koja se koristi Microsoft® Internet Information Services (IIS) 6.x, IIS 7.x ili noviji.
- Prilikom korištenja IIS 6.x, ASP.NET v2.0.50727 provjerite je li instaliran, registrirana te postaviti dopuštene.
- Obavezno servisi uloga prilikom korištenja IIS 7.x ili noviji uključiti ASP.NET i IIS 6 metabaze kompatibilnosti.
- Korisnički Portal mora biti zaštićen s SSL certifikata.
- SDK za servisa Azure višestruke provjere autentičnosti Web mora biti instaliran u IIS 6.x, IIS 7.x ili noviji na poslužitelju Azure višestruku provjeru autentičnosti poslužitelja na kojem je instaliran.
- SDK za servisa Azure višestruke provjere autentičnosti Web mora biti zaštićen s SSL certifikata.
- Korisnički Portal moraju moći povezati s SDK za servisa Azure višestruke provjere autentičnosti Web putem SSL.
- Korisnički Portal mora biti moguće provjeriti autentičnost Azure višestruke provjere autentičnosti Web servisa SDK pomoću vjerodajnica računa servisa koji pripada sigurnosne grupe pod nazivom "PhoneFactor administratori". Ovaj račun servisa i grupe postoje u servisu Active Directory ako Azure višestruku provjeru autentičnosti poslužitelja je pokrenut na poslužitelju domene pridružili. Ovaj račun servisa i grupe postoje lokalno na poslužitelju Azure višestruke provjere autentičnosti ako niste član domene.

Instaliranje korisnički portal na poslužitelju koji nije poslužitelj programa Azure višestruke provjere autentičnosti zahtijeva sljedeća tri koraka:

1. Instalirajte web-servisa SDK
2. Instalacija portal za korisnika
3. Konfiguriranje korisničkih postavki portala Server Azure višestruka provjera autentičnosti


### <a name="install-the-web-service-sdk"></a>Instalirajte web-servisa SDK

Ako SDK za servisa Azure višestruke provjere autentičnosti Web već nije instaliran na poslužitelju Azure višestruke provjere autentičnosti, idite na tom poslužitelju i otvorite Azure višestruku provjeru autentičnosti poslužitelja. Kliknite ikonu Web servisa SDK, kliknite SDK za instaliranje Web servisa... gumb, a zatim slijedite navedene upute. Servis SDK Web mora biti zaštićen s SSL certifikata. Samopotpisani certifikat je u redu u tu svrhu, ali je moguće uvesti u spremište "Izdavanje korijenskih" račun lokalnog računala na web-poslužitelju korisnički Portal tako da se smatrao pouzdanima taj certifikat kada pokretanje SSL veze.

<center>![Postavljanje](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="install-the-user-portal"></a>Instalacija portal za korisnika

Prije instalacije korisnički portal na istom poslužitelju, imajte na umu sljedeće:

- Da biste otvorili web-pregledniku na poslužitelj za web-mjesto na Internetu i idite na URL SDK Web servisa koji je unesen u datoteci web.config korisno je. Ako web-pregledniku doći do web-servisa uspješno, ga treba zatražiti vjerodajnice. Unesite korisničko ime i lozinku koje su unijeli u datoteci web.config točno onako kako se prikazuju u datoteci. Provjerite je li se prikazuju bez certifikata upozorenja ili pogreške.
- Ako povratnog proxy ili vatrozid je koji se nalaze ispred web-poslužitelj korisnički Portal i izvođenje SSL rasteretite, možete uređivati datoteku web.config korisnički Portal i dodajte sljedeći ključ da biste na <appSettings> sekcije tako da se korisnički Portal umjesto https možete koristiti HTTP-a. <add key="SSL_REQUIRED" value="false"/>

#### <a name="to-install-the-user-portal"></a>Da biste instalirali portal za korisnika

1. Otvorite Windows Explorer na poslužitelju Azure višestruke provjere autentičnosti poslužitelja, a zatim dođite do mape u kojem je instaliran na poslužitelj za Azure multi-factor provjere autentičnosti (npr. C:\Program Files\Multi dvofaktorska analiza varijance provjeru autentičnosti poslužitelja). Odabir 32-bitnu ili 64-bitne verzije sustava MultiFactorAuthenticationUserPortalSetup instalacijsku datoteku po potrebi poslužitelja koji korisnički Portal instalirat će se na. Kopirajte instalacijsku datoteku na poslužitelj za mjesto na Internetu.
2. Na poslužitelju web-mjesto na Internetu, instalacijska datoteka morate pokrenuti s administratorskim ovlastima. Najjednostavniji način za to je da biste otvorili naredbenog retka kao administrator i prijeđite na mjesto na koje će biti kopirana instalacijsku datoteku.
3. Pokrenite MultiFactorAuthenticationUserPortalSetup64 instalacijsku datoteku, promijenite naziv web-mjesta i virtualnog direktorija po želji.
4. Nakon dovršetka instalacije korisnički Portal, dođite do C:\inetpub\wwwroot\MultiFactorAuth (ili odgovarajući direktorij na temelju naziv virtualnog direktorija), a zatim Uređivanje web.config datoteke.
5. Pronađite ključ USE_WEB_SERVICE_SDK i promijenite vrijednost iz false u true. Pronađite tipki WEB_SERVICE_SDK_AUTHENTICATION_USERNAME i WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD i postavite vrijednosti na korisničko ime i lozinku za račun servisa koji pripada sigurnosti PhoneFactor administratori grupe (u odjeljku preduvjeti gore). Pripazite da unesete korisničko ime i lozinku između navodnim znakovima na kraju retka (vrijednost = "" / >). Preporučuje se da biste koristili kvalificirani korisničko ime (primjerice domena/korisničko ime ili machine\username)
6. Pronađite željenu postavku pfup_pfwssdk_PfWsSdk i promijenite vrijednost "http://localhost:4898/PfWsSdk.asmx" URL SDK servisa Web koji se izvodi na poslužitelju višestruke provjere autentičnosti (npr. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx) Azure. Jer SSL se koristi za tu vezu, jer SSL certifikat će Izdana za naziv poslužitelja i URL koristi moraju se podudarati s nazivom na certifikat mora pozivati SDK servisa Web naziv poslužitelja i ne IP adresa. Ako naziv poslužitelja ne riješite IP adresu poslužitelja mjesto na Internetu, dodajte stavku domaćini datoteku na tom poslužitelju da biste mapirali naziv poslužitelja Azure višestruke provjere autentičnosti na njegovu IP adresu. Nakon što su promjene, spremite datoteku web.config.
7. Ako je korisnički Portal je bio instaliran u odjeljku (npr. zadani Web-mjesta) web-mjesto nije već je binded s potvrdom javno prijavljeni, instalirati certifikat na poslužitelju ako nije već instaliran, otvorite upravitelj IIS i povežite certifikata na web-mjesto.
8. Otvorite web-preglednika na bilo kojem računalu, a zatim otvorite URL instaliranim korisnički Portal (npr. https://www.publicwebsite.com/MultiFactorAuth). Provjerite je li se prikazuju bez certifikata upozorenja ili pogreške.



## <a name="configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Konfiguriranje korisničkih postavki portala u okvir poslužitelj za Azure višestruke provjere autentičnosti
Sad kad je instaliran portalu, morate konfigurirati Azure višestruke provjere provjeru autentičnosti poslužitelja za rad s portala sustava.

Azure višestruka provjera autentičnosti poslužitelja nudi nekoliko mogućnosti za portal za korisnika.  Sljedeća tablica sadrži popis tih mogućnosti i programa explaination od koje se koriste za.

Korisničke postavke portala|Opis|
:------------- | :------------- |
URL portala za korisnika| Omogućuje unesite URL gdje se nalazi na portal.
Primarni provjere autentičnosti| Omogućuje vam da biste naveli vrstu provjere autentičnosti za korištenje prilikom prijave na portal.  Provjera autentičnosti sustava Windows, polumjer ili LDAP.
Korisnici mogu prijaviti|Korisnicima omogućuje portala korisnika na stranici prijava unesite korisničko ime i lozinku.  Ako je odabrana, okvire će biti zasivljen.
Dopusti Registracija korisnika|Omogućuje korisniku da biste uvrstili u višestruka provjera autentičnosti prihvaćanjem ih instalacijskom zaslonu koji traži dodatne informacije, kao što je telefonski broj.  Upit za sigurnosne kopije telefona korisnicima omogućuje određivanje sekundarne telefonskog broja.  Upitaj koji će proizvođača OATH token omogućuje korisnicima da biste odredili 3 tokena OATH proizvođača.
Korisnicima omogućuje da pokrenu zaobilazak One-Time| To korisnicima omogućuje pokretanje jednokratni sadržaje.  Ako korisnik skupove to se to može potrajati utječe na sljedeći put korisnikove prijave u.  Upitaj koji će zaobilazak sekundi korisniku daje okvir tako da ih možete promijeniti zadani 300 sekundi.  U suprotnom jednokratan zaobilazak samo dobro je 300 sekundi.
Korisnicima omogućuje odabir načina| Omogućuje korisnicima da biste odredili način njihova primarnog kontakta.  To se može biti telefonski poziv, SMS-om, mobilne aplikacije ili OATH token.
Korisnicima omogućuje odabir jezika|  Omogućuje korisniku da biste promijenili jezik koji se koristi za telefonski poziv, SMS-om, mobilne aplikacije ili OATH token.
Dopusti korisnicima da biste aktivirali aplikacije za mobilne uređaje| Korisnicima omogućuje stvaranje sustava aktivacije kod da biste dovršili postupak aktivacije mobilne aplikacije koja se koristi s poslužiteljem.  Možete postaviti i broj uređaja ih možete aktivirati na.  Između 1 i 10.
Sigurnosna pitanja namijenjen pričuvne|Omogućuje vam korištenje sigurnosna pitanja u slučaju da ne uspije višestruke provjere autentičnosti.  Možete navesti broj sigurnosna pitanja koja se mora uspješno odgovoriti.
Korisnici mogu pridružiti token OATH drugih proizvođača| Korisnicima omogućuje određivanje OATH token drugih proizvođača.
Korištenje OATH token za pričuvne|Omogućuje korištenje token za OATH u događaja višestruke provjere autentičnosti nije uspjelo.  Možete odrediti i vremensko ograničenje sesije u minutama.
Omogućite zapisivanje|Omogućuje bilježenje u zapisnik portala za korisnika.  Datoteke zapisnika nalaze se na: C:\Program Files\Multi dvofaktorska analiza varijance provjere autentičnosti Server\Logs.

Većinu tih postavki su vidljive kada su te značajke omogućene korisnika i predznaku korisnika u korisnički portal.

![Korisničke postavke portala](./media/multi-factor-authentication-get-started-portal/portalsettings.png)



### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Konfiguriranje korisničkih postavki portala u okvir poslužitelj za Azure višestruke provjere autentičnosti




1. Na poslužitelju Azure višestruke provjere autentičnosti, kliknite ikonu korisnički Portal. Na kartici postavke unesite URL portala za korisnika u tekstni okvir URL portala za korisnika. Ovaj URL umetnut će se poruke e-pošte koje se šalju korisnicima prilikom uvoza na poslužitelj za Azure višestruke provjere autentičnosti ako je omogućeno funkcionalnosti e-pošte.
2. Odaberite postavke koje želite koristiti na portalu za korisnika. Ako, na primjer, ako korisnici će moći nadzirati njihove metoda provjere autentičnosti, provjerite je li uz metode možete birati provjereno Dopusti korisnicima da biste odabrali način.
3. Kliknite vezu za pomoć u gornjem desnom kutu pomoć objašnjenje bilo koju od postavki prikaza.

<center>![Postavljanje](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Kartica za administratore
Ta kartica jednostavno omogućuje da biste dodali korisnike koji će imati administratorske ovlasti.  Prilikom dodavanja administrator, možete precizno podešavanje dozvole koje primaju.  Na taj način možete biti sigurni da biste samo dodijelili potrebne dozvole administratora.  Jednostavno kliknite gumb Dodaj i zatim odaberite i korisnika i njihove dozvole, a zatim kliknite Dodaj.

![Korisnik portala za administratore](./media/multi-factor-authentication-get-started-portal/admin.png)


## <a name="security-questions"></a>Sigurnosna pitanja
Ta kartica omogućuje određivanje sigurnosna pitanja na koja korisnika morat ćete navesti odgovore na odaberete sigurnosna pitanja koristi za pričuvni mogućnost.  U sklopu Azure Authenticaton Server multi-factor zadana pitanja na koje možete koristiti.  Možete promijeniti redoslijed ili dodavanje vlastita pitanja.  Prilikom dodavanja vlastita pitanja, možete odrediti na jeziku koji biste željeli te pitanje da se prikazuju kao i.

![Korisnik portala sigurnosna pitanja](./media/multi-factor-authentication-get-started-portal/secquestion.png)


## <a name="passed-sessions"></a>Proslijeđeni sesije

## <a name="saml"></a>SAML
Omogućuje postavljanje korisnički portal za prihvaćanje zahtjeva davatelja identiteta SAML pomoću.  Možete navesti vremensko ograničenje sesije, navedite certifikat za provjeru i zapisnika out preusmjeravanje URL-a.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Pouzdani IP-ovi
Ta kartica omogućuje vam da navedete jednu IP adresa ili rasponi IP adresa koji se može dodati tako da ako korisnik prijavi neku od ovih IP adrese, zatim višestruka provjera autentičnosti je zaobiđena.

![Korisnički portal pouzdana IP-ovi](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Registracija korisnika za samostalno
Ako želite da korisnici za prijavu i registrirati morate potvrdite okvir Dopusti korisnicima da biste se prijavili u i omogućivanje mogućnosti Registracija korisnika. Imajte na umu da odaberete postavke utjecat će na sučelje za prijavu korisnika.

Na primjer, kada korisnik prijavljuje Portal za korisnika i kliknuti gumb prijava, oni zatim uzimaju se na stranicu za postavljanje programa Azure višestruke provjere autentičnosti korisnika.  Ovisno o tome kako ste konfigurirali Azure višestruke provjere autentičnosti, korisnik moći odaberite njihove način provjere autentičnosti.  

Odaberite način provjere autentičnosti govorne poziva ili je unaprijed konfigurirana za korištenje taj način, stranici tražit će od korisnika unositi primarnog telefonskog broja i nastavak ako je primjenjivo.  Oni možda također će moći unesite sigurnosne kopije telefonski broj.  

![Korisnički portal pouzdana IP-ovi](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Ako korisnik mora koristiti PIN-a dok ih autentičnost, stranice će i zatražiti da biste unijeli PIN-a.  Kada unesete njihove brojeve telefona i PIN-a (Ako je primjenjivo), klika na Nazovi Me sada gumb za provjeru autentičnosti.  Azure višestruka provjera autentičnosti će izvesti nazvati autentičnosti korisnika primarni telefonski broj.  Korisnik mora odgovor na telefonski poziv i unesite njihove PIN-a (Ako je primjenjivo) i pritisnite # da biste premjestili na sljedeći korak u self-sklopu postupka.   

Ako korisnik odabire način provjere autentičnosti SMS tekst ili je unaprijed konfigurirana za korištenje taj način, stranici tražit će od korisnika za njihov broj mobilnog telefona.  Ako korisnik mora koristiti PIN-a dok ih provjeru autentičnosti, stranice će i zatražiti da biste unijeli PIN-a.  Nakon unosa njihov broj telefona i PIN-a (Ako je primjenjivo), klika na tekst Me sada gumb za provjeru autentičnosti.  Azure višestruka provjera autentičnosti će izvesti SMS provjere autentičnosti korisnika mobilni telefon.  Korisnik mora primati SMS koja sadrži jedan-vrijeme-pristupni izraz (OTP) i odgovor na poruku s tom OTP plus njihove PIN-a ako je primjenjivo) da biste premjestili na sljedeći korak u self-sklopu postupka.

![Korisnički portal SMS PORUKE](./media/multi-factor-authentication-get-started-portal/text.png)   

Ako korisnik odabire način provjere autentičnosti mobilne aplikacije ili je unaprijed konfigurirana za korištenje taj način, stranici tražit će od korisnika da biste instalirali aplikaciju Azure višestruke provjere autentičnosti na svoj uređaj i generiranje koda za aktivaciju.  Kada instalirate aplikaciju Azure višestruke provjere autentičnosti, korisnik klikne gumb generiranje koda za aktivaciju.    

>[AZURE.NOTE]Da biste koristili aplikaciju Azure višestruke provjere autentičnosti, korisnik mora omogućiti slanje obavijesti za svoj uređaj.

Na stranici prikazuju se zatim web-mjesto kod sustava aktivacije i u okvir za URL zajedno sa slike crtičnog koda.  Ako korisnik mora koristiti PIN-a dok ih provjeru autentičnosti, stranice će i zatražiti da biste unijeli PIN-a.  Korisnik unosi kod aktivacije i URL-a u aplikaciju za Azure višestruke provjere autentičnosti ili koristi skenera crtičnog koda za skeniranje slike crtičnog koda i klikne gumb aktiviraj.    

Po dovršetku aktivaciju korisnik klikne gumb autentičnost Me sada.  Azure višestruka provjera autentičnosti će izvesti je provjera autentičnosti za mobilne aplikacije za korisnika.  Korisnik morate unijeti njihove PIN-a (Ako je primjenjivo) i pritisnite gumb za provjeru autentičnosti u svoje mobilne aplikacije za premještanje na sljedeći korak u self-sklopu postupka.  


Ako administratori ste konfigurirali Azure višestruke provjere provjeru autentičnosti poslužitelja za prikupljanje sigurnosna pitanja i odgovore, korisnik se zatim preusmjereni na stranicu sigurnosna pitanja.  Korisnik mora odaberite četiri sigurnosna pitanja i navedite odgovore na njihove odabrane pitanja.    

![Korisnik portala sigurnosna pitanja](./media/multi-factor-authentication-get-started-portal/secq.png)  

Self-Registracija korisnik je dovršen, a korisnik prijavljen je na portalu za korisnika.  Korisnicima možete prijaviti natrag u korisnički Portal u bilo kojem trenutku u budućnosti da biste promijenili svoje telefonske brojeve, PIN-ove, metoda provjere autentičnosti i sigurnosna pitanja ako njihove administratori.
