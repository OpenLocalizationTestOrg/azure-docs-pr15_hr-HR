<properties
    pageTitle="Sigurne aplikaciju u aplikacije servisa za Azure"
    description="Saznajte kako web app, pozadinskog mobilne aplikacije ili aplikaciji API-JA za Azure aplikacije servisa za sigurnu."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="01/12/2016"
    ms.author="cephalin"/>


#<a name="secure-an-app-in-azure-app-service"></a>Sigurne aplikaciju u aplikacije servisa za Azure

U ovom se članku pojednostavnjuje početak rada na Zaštita vaše web-aplikacijama, pozadinskog mobilne aplikacije ili aplikaciji Azure aplikacije servisa za API-JA. 

Sigurnost u aplikacije servisa za Azure sastoji se od dvije razine: 

- **Infrastruktura i platformu sigurnosti** - koje vam šalju pouzdane Azure da bi servisi morate zapravo pokrenuti stvari sigurno u oblaku.
- **Sigurnost aplikacije** - morate sigurno dizajnirati samoj aplikaciji. To obuhvaća kako integrirati Azure Active Directory, kako upravljati potvrde i kako provjeriti da sigurno možete razgovarati s različitim servisima. 

#### <a name="infrastructure-and-platform-security"></a>Infrastruktura i platformu sigurnosti
Budući da se održavaju aplikacije servisa Azure VMs prostora za pohranu, mrežne veze, okviri web, upravljanje i značajki integracije i još mnogo toga je aktivno osigurani i hardened prolaze kroz vigorous usklađenost i provjerava na temelju neprekinuti li:

- Aplikacija aplikacije servisa za su Izolirani iz obje s Internetom i ostali korisnici Azure resursi.
- Komunikacija tajne (primjerice nizovi veze) između aplikacije servisa za aplikacije i drugih Azure resursa (npr. SQL baza podataka) u grupu resursa prekorači Azure, a ne Unakrsna sve ograničenja mreže. Uvijek su šifrirane tajne.
- Svu komunikaciju između aplikacije servisa za aplikacije i vanjskim izvorima, kao što su upravljanje PowerShell, sučelje naredbenog retka, Azure SDK-ovi, REST API-ji i veze hibridnog pravilno šifrirane.
- 24-satni prijetnju upravljanje štiti resursi za servisnu aplikaciju od zlonamjernog softvera, distributed uskraćivanje usluge (DDoS), Muškarac-u-u – Srednje (MITM) i drugih prijetnji. 

Dodatne informacije o sigurnosti za infrastrukturu i platformu u Azure potražite u članku [Centar za pouzdanost Azure](/support/trust-center/security/).

#### <a name="application-security"></a>Sigurnost aplikacije

Dok je Azure odgovoran za osiguravanje infrastrukturu i platformu koja pokreće se aplikacija je odgovornost radi zaštite vaše same aplikacije. Drugim riječima, morate razviti, uvođenje i upravljanje kod aplikacije i sadržaj na siguran način. Bez to, kod aplikacije ili sadržaj može biti podložno prijetnji kao što su:

- Unos SQL
- Sesije hijacking
- Unakrsno – web-mjesta – skriptiranje
- MITM razini aplikacije
- DDoS razini aplikacije

Cjelokupnu raspravu od sigurnosna pitanja vezana uz za web-aplikacijama izlazi iz ovog dokumenta. Kao početnu točku za daljnje smjernice o Zaštita vaše aplikacije potražite u članku na [Otvorene Web aplikacije sigurnost projekta (OWASP)](https://www.owasp.org/index.php/Main_Page), posebno u [prvih 10 projekta.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), kojoj se nalaze na trenutnu gornje 10 ključnih web aplikacije sigurnost pogreške utvrdili OWASP članova.

## <a name="perform-penetration-testing-on-your-app"></a>Izvođenje penetration testiranje na aplikaciju

Najlakši načina da niste započeli testiranje slabe točke na aplikacije servisa za aplikaciju je koristiti [Integracija s Tinfoil sigurnost](/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) da biste izvršili jednim klikom slabe skeniranje na aplikaciju. Prikaz rezultata testa u izvješću jednostavno-na-razumijevanje i Saznajte kako ispraviti svaki slabe s detaljnim uputama.

Ako biste radije izvršili testiranja penetration ili želite koristiti neki drugi paket skenera ili davatelj usluge, morate slijedite [postupak testiranja odobrenje Azure penetration](https://security-forms.azure.com/penetration-testing/terms) i dobili prethodnog odobravanje za testove željeni penetration.

##<a name="https"></a>Sigurne komunikacije s klijentima

Ako koristite na ** \*. azurewebsites.net** stvorene za aplikaciju programa aplikacije servisa, naziv domene možete odmah koristiti HTTPS, kao i SSL certifikat navedeni su za sve ** \*. azurewebsites.net** naziva domena. Ako web-mjesta koristi [Prilagođeni naziv domene](web-sites-custom-domain-name.md), možete prenijeti SSL certifikata da biste [omogućili HTTPS](web-sites-configure-ssl-certificate.md) za prilagođenu domenu.

Omogućivanje [HTTPS](https://en.wikipedia.org/wiki/HTTPS) pridonosi zaštiti protiv napada MITM komunikacije između aplikacije i njezine korisnike.

## <a name="secure-data-tier"></a>Sigurne sloju podataka

Aplikacije servisa za vrlo integriran je s bazom podataka SQL tako da sve nizove veze šifrirane pločom i samo dešifrirati na VM pokrenute aplikacije *i* samo prilikom pokretanja aplikacije. Osim toga, baze podataka SQL Azure sadrži brojne značajke sigurnosti za zaštitu podataka aplikacije iz cyber prijetnji, uključujući [šifriranje na ostale](https://msdn.microsoft.com/library/dn948096.aspx), [Uvijek šifrirane](https://msdn.microsoft.com/library/mt163865.aspx), [Dinamični Masking podataka](../sql-database/sql-database-dynamic-data-masking-get-started.md)i [Otkrivanje prijetnju](../sql-database/sql-database-threat-detection-get-started.md). Ako imate povjerljive podatke ili zahtjeve za usklađenosti, potražite u članku [Zaštita baze podataka sustava SQL](../sql-database/sql-database-security.md) dodatne informacije o tome kako zaštitu podataka.

Ako koristite davatelja usluge drugih proizvođača baze podataka, kao što su ClearDB, trebali biste pogledajte s vašeg davatelja usluge dokumentaciju izravno na sigurnost najbolje prakse.  

##<a name="develop"></a>Razvoj i implementaciju

### <a name="publishing-profiles-and-publish-settings"></a>Objavljivanje profili i postavke objavljivanja

Prilikom razvoja aplikacije, izvođenje zadataka upravljanja ili automatiziranje zadataka pomoću uslužni programi kao što su **Visual Studio**, **Matrica Web**, **Azure PowerShell** ili **Azure sučelje naredbenog retka (Azure EŽA)**, koristite *postavke objavljivanja* datoteke ili *Objavljivanje profila*. Obje vrste datoteka uspješnoj Azure, a trebali biste sigurnu vezu da biste spriječili neovlašteni pristup.

* Datoteka **postavki objavljivanja** sadrži

    * Identifikacijskog Broja za Azure pretplate

    * Upravljanje certifikat koji vam omogućuje izvođenje zadataka upravljanja za svoje pretplate *bez potrebe da navede korisničko ime ili lozinku*.

* **Objavljivanje profila** datoteka sadrži

    * Podaci za objavljivanje u aplikaciju

Ako koristite program koji koristi Objavi postavke datoteku ili datoteku profila za objavljivanje, Uvoz datoteke koja sadrži postavke objavljivanja ili profila u program, a zatim **Izbriši** datoteke. Ako morate zadržati datoteku da biste zajednički koristili s drugim korisnicima rad na projektu, na primjer, spremite ga na sigurnom mjestu kao što su *šifrirane* direktorija s ograničenim dozvolama.

Uz to, provjerite je li su zaštićene uvezene vjerodajnice. Ako, na primjer, **Azure PowerShell** i **sučelje Azure naredbenog retka (Azure EŽA)** pohrane uvezenih podataka u **početnom direktorija** (*~* na sustave Linux ili OS X i */users/yourusername* u sustavima Windows.) Radi dodatne sigurnosti, možda želite **šifrirati** ovim mjestima pomoću alata za šifriranje dostupna za vaš operacijski sustav.

### <a name="configuration-settings-and-connection-strings"></a>Postavke konfiguracije i nizove veze
Je uobičajeno za pohranu nizove veze, vjerodajnice za provjeru autentičnosti i druge povjerljive informacije u datotekama konfiguracije. Nažalost, te datoteke mogu biti vidljiva na web-mjestu ili prijavi u javnom spremište će se ti podaci. Jednostavno pretraživanje na [GitHub](https://github.com), na primjer, možete otkriti nebrojene konfiguracije datoteke s prikazanog tajne u javnom spremištima.

Najbolje je da bi ti podaci iz aplikacije programa konfiguracijske datoteke. Aplikacije servisa omogućuje spremanje informacija o konfiguraciji kao dio okruženje za izvođenje kao **postavki aplikacije** i **nizove veze**. U aplikaciji prilikom izvođenja kroz *varijable okruženja* za većinu programskog jezika ponudit će se vrijednosti. Za aplikacije za .NET te vrijednosti su umetnutog u konfiguraciji .NET tijekom rada. Osim ovih situacija ove postavke konfiguracije će ostati šifrirane osim ako prikaz ili ih pomoću [Portala za Azure](https://portal.azure.com) ili uslužni programi kao što su PowerShell ili EŽA Azure konfigurirali. 

Pohranjivanje informacija o konfiguraciji u aplikacije servisa za zahvaljujući aplikacije administratora za zaključavanje povjerljivih podataka za aplikacije radnog. Razvojnim inženjerima u zaseban skup konfiguracijske postavke za razvoj aplikacija, a zatim postavke možete automatski zamijenila postavkama konfiguriran u aplikacije servisa. Čak ni razvojnim inženjerima morate znati tajne konfigurirana za aplikaciju proizvodnje. Dodatne informacije o konfiguriranju postavki aplikacije i nizove veze u servisu aplikacije potražite u članku [Konfiguriranje web-aplikacije](web-sites-configure.md).

### <a name="ftps"></a>FTPS

Aplikacije servisa za Azure omogućuje sigurno FTP pristup datotečnog sustava za aplikaciju putem **FTPS**. To vam omogućuje da sigurno pristupali kod aplikacije na web-aplikacije, kao i Dijagnostika zapisnika. Preporučuje se da uvijek koristite FTPS umjesto FTP. 

Vezu FTPS aplikacije pronaći ćete na sljedeći način:

1. Otvorite [Portal za Azure](https://portal.azure.com).
2. Odaberite **Pregledaj sve**.
3. **Pregled** plohu odaberite **Aplikaciju servisa**.
4. Plohu **Aplikacije servisa** odaberite željenu aplikaciju.
5. Plohu aplikacije, odaberite **sve postavke**.
6. Plohu **Postavke** odaberite **Svojstva**.
7. Veza FTP i FTPS su navedeni su na plohu **Postavke** . 

Dodatne informacije o FTPS potražite u članku [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o sigurnosti Azure platforme informacije na izvješća na **incident vezan uz sigurnost ili zloupotreba**ili obavijestiti Microsoft da vam izvoditi **testiranje penetration** web-mjesta u odjeljku Sigurnost u [Centru za pouzdanost Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/).

Dodatne informacije o **web.config** ili **applicationhost.config** datoteke u aplikacijama za aplikacije servisa za potražite u članku [mogućnosti konfiguracije otključana u aplikacije servisa za Azure web-aplikacijama](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Informacije o zapisivanje informacija za aplikacije servisa za aplikacije koje mogu biti korisne u otkrivanje napada, potražite u članku [uključite Zapisivanje dijagnostičkih podataka](web-sites-enable-diagnostic-log.md).

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvorite short-lived starter aplikaciju u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="whats-changed"></a>Što se promijenilo

* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)
