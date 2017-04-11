<properties
    pageTitle="Instalacija Azure AD povezivanje Health Agent | Microsoft Azure"
    description="To je stranica stanja povezivanje Azure AD koji opisuje instalacije agent za AD FS i sinkronizacija."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-agent-installation"></a>Stanje Agent za instalaciju za Azure AD Connect

Ovaj dokument vodit će vas kroz instaliranje i konfiguriranje Azure agenata AD povezivanje stanja. Na agenata možete preuzeti iz [ovdje](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent).

##  <a name="requirements"></a>Preduvjeti
U sljedećoj su tablici je popis preduvjetima za korištenje Azure AD povezivanje stanja.

| Preduvjet | Opis|
| ----------- | ---------- |
|Azure AD Premium| Povežite zdravlje Azure AD je značajka Azure AD Premium i zahtijeva Azure AD Premium. </br></br>Dodatne informacije potražite u članku [Uvod u rad s Azure AD Premium](active-directory-get-started-premium.md) </br>Da biste započeli besplatnu 30-dnevnu probnu verziju, pročitajte članak [pokretanje probnu verziju.](https://azure.microsoft.com/trial/get-started-active-directory/)|
|Morate biti globalni administrator sustava Azure AD za početak rada s povežite zdravlje Azure AD|Prema zadanim postavkama, samo globalni administratori možete instalirati i konfigurirati agenata stanja za početak rada, pristup portalu i izvršiti sve operacije unutar Azure AD povežite zdravlje. Dodatne informacije potražite u članku [administriranje Azure AD direktorija](active-directory-administer.md). <br><br> Pomoću kontrola pristupa temelji uloga možete dopustiti pristup Azure AD povežite zdravlje drugim korisnicima u tvrtki ili ustanovi. Dodatne informacije potražite u članku [uloga temelji kontrole pristupa za Azure AD povežite zdravlje.](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**Važne:** Račun koji se koristi prilikom instalacije sustava agenata mora biti račun tvrtke ili obrazovne ustanove. Ne može biti Microsoftova računa. Dodatne informacije potražite u članku [registrirati za Azure kao tvrtke ili ustanove](sign-up-organization.md)
|Na svakom poslužitelju ciljni je instaliran na Azure AD povezivanje Health Agent| Povežite zdravlje Azure AD potrebna je instalacija agent na ciljnim poslužiteljima, možete unijeti podatke koje je pregledavati na portalu. </br></br>Na primjer, za dohvaćanje podataka iz infrastruktura za lokalni AD FS, agenta mora biti instaliran na poslužiteljima AD FS, Proxy za AD FS i Proxy aplikacije za Web. Isto tako, da biste dobili podataka lokalnih AD DS infrastruktura za agenta mora biti instalirana na kontrolera domena. </br></br>**Važne:** Račun koji se koristi prilikom instalacije sustava agenata mora biti račun tvrtke ili obrazovne ustanove. Ne može biti Microsoftova računa. Dodatne informacije potražite u članku [registrirati za Azure kao tvrtke ili ustanove](sign-up-organization.md)|
|Izlazni veza s krajnje točke servisa Azure|Tijekom instalacije i izvođenja, agenta potrebna je veza s krajnje točke servisa Azure AD povezivanje stanja. Ako je blokirana izlaznog povezivanje, provjerite sljedeće krajnje točke dodaju na popis dopuštenih: </br></br><li>& #42;. blob.Core.Windows.NET </li><li>& #42;. Queue.Core.Windows.NET</li><li>adhsprodwus.servicebus.Windows.NET - priključka: 5671 </li><li>https://Management.Azure.com </li><li>https://S1.adhybridhealth.Azure.com/</li><li>https://policykeyservice.DC.ad.MSFT.NET/</li><li>https://login.Windows.NET</li><li>https://login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li> |
|Priključaka u vatrozidu na poslužitelju koji se izvodi agenta.| Agenta zahtijeva sljedeće priključaka u vatrozidu biti otvoreni da bi agent za komunikaciju s krajnje točke servisa Azure AD stanja.</br></br><li>TCP/UDP priključak 443</li><li>TCP/UDP priključaka 5671</li>
|Dopusti sljedeće web-mjesta ako je omogućena Poboljšana zaštita preglednika Internet Explorer| Ako je omogućena Poboljšana zaštita preglednika Internet Explorer, sljedeći web-mjesta morate dopustiti na poslužitelju na kojem će imati instaliran agent.</br></br><li>https://login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li><li>https://login.Windows.NET</li><li>Vanjskom poslužitelju za tvrtku ili ustanovu vjerovati Azure Active Directory. Na primjer: https://sts.contoso.com</li>




## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>Instaliranje Azure AD povezivanje Health Agent za AD FS
Da biste pokrenuli instalaciju agent, dvokliknite .exe datoteke koju ste preuzeli. Na prvom zaslonu kliknite Instaliraj.

![Provjera stanja za Azure AD Connect](./media/active-directory-aadconnect-health-requirements/install1.png)

Po dovršetku instalacije kliknite Konfiguriraj odmah.

![Provjera stanja za Azure AD Connect](./media/active-directory-aadconnect-health-requirements/install2.png)

U naredbeni redak se pokrene, Slijedi nekoliko PowerShell koji se izvršava Register AzureADConnectHealthADFSAgent. Kada se to od vas zatraži da biste se prijavili Azure, nastaviti i prijavite se.

![Provjera stanja za Azure AD Connect](./media/active-directory-aadconnect-health-requirements/install3.png)


Nakon prijave PowerShell će i dalje. Kada se dovrši, možete zatvoriti PowerShell i konfiguracija je dovršena.

Sada usluge počne automatski dopuštanja agent prikupljanje podataka i praćenje. Ako ste nije zadovoljen navedene u ranijim odlomcima stara requisites, upozorenja će se pojaviti u prozoru PowerShell. Ne zaboravite da biste dovršili [preduvjeti](active-directory-aadconnect-health-agent-install.md#requirements) prije instalacije agenta. Snimka zaslona za sljedeće je primjera te pogreške.

![Provjera stanja za Azure AD Connect](./media/active-directory-aadconnect-health-requirements/install4.png)

Da biste provjerili agenta je instaliran, potražite sljedeće usluge na poslužitelju. Ako ste dovršili konfiguraciju, oni mora već biti pokrenut. U suprotnom, oni su zaustavljena konfiguraciju dok se ne dovrši.

- Stanje AD FS dijagnostički servis za Azure AD Connect
- Stanje servisa AD FS uvida servisa za Azure AD Connect
- Stanje AD FS nadzor servis za Azure AD Connect

![Provjera stanja za Azure AD Connect](./media/active-directory-aadconnect-health-requirements/install5.png)


### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Agent za instalaciju na Windows Server 2008 R2 poslužitelja

Koraci za Windows Server 2008 R2 poslužitelje:

1. Provjerite je li poslužitelj izvodi pri Service Pack 1 ili noviji.
1. Isključite IE ESC za instalaciju agent:
1. Instalirajte Windows PowerShell 4.0 na svim poslužiteljima ispred instalacije agent za stanje servisa Active Directory. Da biste instalirali Windows PowerShell 4.0:
 - Instalirajte [Microsoft .NET Framework 4,5](https://www.microsoft.com/download/details.aspx?id=40779) putem sljedeće veze da biste preuzeli izvanmrežne instalacijski program.
 - Instalacija Očisti filtar (iz značajke u sustavu Windows)
 - Instalacija na [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
 - Instalacija programa Internet Explorer verzije 10 ili noviji na poslužitelju. (Obavezno stanja servis za provjeru autentičnosti, pomoću Azure administratorskih vjerodajnica.)
1. Dodatne informacije o instaliranju sustava Windows PowerShell 4.0 na Windows Server 2008 R2 potražite u članku wiki [ovdje](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Omogućivanje nadzora za AD FS

> [AZURE.NOTE] U ovom se odjeljku odnosi se samo na poslužiteljima vanjski pristup za AD FS.

U redoslijedu za značajku analize korištenja da biste prikupili i analiza podataka agent za Azure AD povežite zdravlje mora informacije iz zapisnika nadzora za AD FS. Ti zapisnici nisu omogućeni prema zadanim postavkama. Da biste omogućili nadzor AD FS, a da biste pronašli zapisnika nadzora AD FS na poslužitelje servisa AD FS, koristite sljedeće postupke.

#### <a name="to-enable-auditing-for-ad-fs-20"></a>Da biste omogućili nadzor za AD FS 2.0

1. Kliknite **Start**, pokažite na **programi**, pokažite na **Stavku Administrativni alati**, a zatim **Lokalna sigurnosna pravila**.
2. Dođite do mape u **Sigurnost Settings\Local Policies\User Rights Management** , a zatim dvokliknite revizije sigurnost Generiraj.
3. Na kartici **Lokalne sigurnosne postavke** , provjerite je li navedena je račun za AD FS 2.0 servisa. Ako ga nema, kliknite **Dodaj korisnika ili grupe** i dodali na popis, a zatim **u redu**.
4. Da biste omogućili nadzor, otvorite naredbeni redak s dodatnim ovlastima i pokrenite sljedeću naredbu:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Zatvorite Lokalna sigurnosna pravila, a zatim otvorite dodatak za upravljanje. Da biste otvorili dodatak za upravljanje, kliknite **Start**, pokažite na **programi**, pokažite na **Stavku Administrativni alati**, a zatim AD FS 2.0 upravljanje.
6. U oknu akcije kliknite Uređivanje svojstava servisne vanjski pristup.
7. U dijaloškom okviru **Svojstva servisa za vanjski pristup** karticu **događaja** .
8. Odaberite potvrdne okvire **uspjeh nadzora** i **nadzora nije uspjelo** .
9. Kliknite **u redu**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Da biste omogućili nadzor za AD FS na Windows Server 2012 R2

1. Otvorite **Lokalna sigurnosna pravila** tako da otvorite **Upravitelj poslužitelja** na početnom zaslonu ili Upravitelj poslužitelja na programskoj traci na radnoj površini, a zatim kliknite **Alati/lokalne sigurnosna pravila**.
2. Dođite do mape u **Dodjelu prava za sigurnost Settings\Local Policies\User** , a zatim dvokliknite **revizije Generiraj sigurnost**.
3. Na kartici **Lokalne sigurnosne postavke** , provjerite je li navedena je račun servisa AD FS. Ako ga nema, kliknite **Dodaj korisnika ili grupe** i dodali na popis, a zatim **u redu**.
4. Da biste omogućili nadzor, otvorite naredbeni redak s dodatnim ovlastima i pokrenite sljedeću naredbu:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Zatvorite **Lokalna sigurnosna pravila**, a zatim otvorite dodatak za **AD FS Management** (u upravitelju poslužitelja, kliknite Alati, a zatim odaberite upravljanje AD FS).
6. U oknu akcije kliknite **Uređivanje svojstava servisne vanjski pristup**.
7. U dijaloškom okviru svojstva servisa za vanjski pristup karticu **događaja** .
8. Potvrdite okvire **uspjeh nadzora i Neuspjeh nadzora** , a zatim kliknite **u redu**.






#### <a name="to-locate-the-ad-fs-audit-logs"></a>Da biste pronašli AD FS nadzornog zapisnika


1. Otvorite **Preglednik događaja**.
2. Otvorite zapisnici sustava Windows pa odaberite **Zaštita**.
3. Na desnoj strani kliknite **Filtar trenutnog zapisnika**.
4. U odjeljku izvor događaj, odaberite **Nadzor za AD FS**.

![AD FS zapisnika nadzora](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] Ako postoji pravilnika grupe koji je onemogućivanje AD FS nadzor, zatim Azure Agent AD povežite zdravlje ne može prikupiti podatke. Provjerite je li nemate pravilnika grupe koji mogu onemogućiti nadzor.

[//]: # (Start of Agent Proxy Configuration Section)

## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>Instaliranje agent za Azure AD povezivanje stanje sinkronizacije
Agent za Azure AD povezivanje stanja sinkronizacije u ažuran Azure AD Connect se instalira automatski. Da biste koristili Azure AD Connect za sinkronizaciju, morate preuzeti najnoviju verziju Azure AD Connect i instalirajte ga. Možete preuzeti najnoviju verziju [ovdje](http://www.microsoft.com/download/details.aspx?id=47594).

Da biste provjerili agenta je instaliran, potražite sljedeće usluge na poslužitelju. Ako ste dovršili konfiguraciju, oni mora već biti pokrenut. U suprotnom, oni su zaustavljena konfiguraciju dok se ne dovrši.

- Stanje sinkronizacije uvida servisa za Azure AD Connect
- Stanje sinkronizacije servisa za nadzor za Azure AD Connect

![Provjerite je li za Azure AD Connect stanje sinkronizacije](./media/active-directory-aadconnect-health-sync/services.png)

> [AZURE.NOTE] Imajte na umu da pomoću Azure AD povežite zdravlje zahtijeva Azure AD Premium. Ako nemate Azure AD Premium niste u mogućnosti da biste dovršili konfiguraciju na portalu za Azure. Dodatne informacije potražite u članku [preduvjeti stranice](active-directory-aadconnect-health-agent-install.md#requirements).


## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Stanje povezivanje ručno Azure AD za registraciju za sinkronizaciju
Ne uspijete povezati stanja Azure AD za sinkronizaciju agent za registraciju nakon uspješno instalacije Azure AD Connect, koristite sljedeću naredbu komponente PowerShell da biste ručno registrirali agenta.

>[AZURE.IMPORTANT] Pomoću ove naredbe ljuske PowerShell samo je potrebna ako Registracija agent prekida se nakon instalacije Azure AD Connect.

Sljedeće komponente PowerShell naredba je potrebna samo prilikom registracije agent stanja ne uspije čak i nakon uspješne instalacije i konfiguracije Azure AD Connect. Azure AD povežite zdravlje usluge započet će nakon agenta uspješno registrirani.

Možete ručno registrirati agent za povezivanje stanja Azure AD za sinkronizaciju pomoću naredbe ljuske PowerShell za sljedeće:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

Naredba uzima pratiti parametara:

- AttributeFiltering: $true (zadano) – ako Azure AD Connect se sinkronizira atribut zadani postavite i prilagođena je tako da koriste atribut filtrirani skup. u suprotnom $false.
- StagingMode: $false (zadano) – ako poslužitelj za Azure AD Connect nije u pripremna načinu $true ako je poslužitelj konfiguriran u pripremna načinu rada.

Kada se to od vas zatraži za provjeru autentičnosti trebali biste koristiti isti poslovni globalni administrator (kao što su admin@domain.onmicrosoft.com) koji je korišten za konfiguriranje Azure AD Connect.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>Instaliranje Azure AD povezivanje Health Agent za AD DS
Da biste pokrenuli instalaciju agent, dvokliknite .exe datoteke koju ste preuzeli. Na prvom zaslonu kliknite Instaliraj.

![Provjera stanja za Azure AD Connect](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Po dovršetku instalacije kliknite Konfiguriraj odmah.

![Provjera stanja za Azure AD Connect](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

U naredbeni redak se pokrene, Slijedi nekoliko PowerShell koji se izvršava Register AzureADConnectHealthADDSAgent. Kada se to od vas zatraži da biste se prijavili Azure, nastaviti i prijavite se.

![Provjera stanja za Azure AD Connect](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Nakon prijave PowerShell će i dalje. Kada se dovrši, možete zatvoriti PowerShell i konfiguracija je dovršena.

Sada usluge počne automatski dopuštanja agent prikupljanje podataka i praćenje. Ako ste nije zadovoljen navedene u ranijim odlomcima stara requisites, upozorenja će se pojaviti u prozoru PowerShell. Ne zaboravite da biste dovršili [preduvjeti](active-directory-aadconnect-health-agent-install.md#requirements) prije instalacije agenta. Snimka zaslona za sljedeće je primjera te pogreške.

![Provjerite je li za Azure AD Connect stanja za AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Da biste provjerili agenta je instaliran, potražite sljedeće usluge na kontrolerom domene.

- Servis stanja AD DS Uvidi za Azure AD Connect
- Stanje AD DS nadzor servisa za Azure AD Connect

Ako ste dovršili konfiguraciju, trebali biste već pokrenut tih servisa. U suprotnom, oni su zaustavljena konfiguraciju dok se ne dovrši.

![Provjera stanja za Azure AD Connect](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds-on-server-core"></a>Instaliranje Azure AD povezivanje Health Agent za AD DS na Server Core.
Nakon instalacije .exe datoteke, dovršite postupak registracije pomoću sljedeće naredbe ljuske PowerShell:

`Register-AzureADConnectHealthADDSAgent -Credential $cred`

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Konfiguriranje Azure AD povežite zdravlje agenata da biste koristili HTTP Proxy
Možete konfigurirati Azure AD povežite zdravlje agenata da biste radili s HTTP Proxy.

>[AZURE.NOTE]
- Korištenje "Netsh WinHttp postavljanje ProxyServerAddress" nije podržano agenta pomoću System.Net zahtjeva web umjesto servisi Microsoft Windows HTTP.
- Http Proxy adresu konfigurirani koristi se za prolazni šifriranih poruka Https.
- Čija je autentičnost provjerena proxy poslužitelji (pomoću HTTPBasic) nisu podržane.

### <a name="change-health-agent-proxy-configuration"></a>Promjena stanja Agent Proxy konfiguracija
Imate sljedeće mogućnosti da biste konfigurirali Azure AD povezivanje Health Agent za korištenje HTTP Proxy.

>[AZURE.NOTE]Sve servise za Azure AD povezivanje Health Agent mora ponovno pokrenuti redoslijedom za postavke proxyja ažurirati. Pokrenite sljedeću naredbu:<br>
Restart-Service AdHealth *

#### <a name="import-existing-proxy-settings"></a>Uvoz postojeće proxy postavke

##### <a name="import-from-internet-explorer"></a>Uvoz iz preglednika Internet Explorer
Internet Explorer HTTP proxy postavke moguće je uvesti koriste Azure agenata AD povezivanje stanja. Na svim poslužiteljima koji se izvode Health agent, pokrenite sljedeću naredbu komponente PowerShell:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Uvoz iz WinHTTP
Postavke proxyja WinHTTP moguće je uvesti koriste Azure agenata AD povezivanje stanja. Na svim poslužiteljima koji se izvode Health agent, pokrenite sljedeću naredbu komponente PowerShell:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Ručno određivanje Proxy adrese
Proxy poslužitelj ručno možete odrediti na svim poslužiteljima koji se izvode Health Agent izvršavanjem sljedeće naredbe ljuske PowerShell:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Primjer: *myproxyserver skup AzureAdConnectHealthProxySettings - HttpsProxyAddress: 443*

- "adresa" može biti naziv poslužitelja resolvable DNS-a ili IPv4 adresa
- "priključak" može biti ispušten. Ako se ispusti, a zatim 443 odabere kao zadani je priključak.

#### <a name="clear-existing-proxy-configuration"></a>Očisti postojeće konfiguracija proxy poslužitelja
Uklonite postojeće konfiguraciju proxy tako da pokretanjem sljedeće naredbe:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Čitanje trenutne postavke proxy poslužitelja
Na trenutno konfigurirane postavke proxy poslužitelja možete pročitati pokretanjem sljedeće naredbe:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Testiranje veza sa sustavom Azure AD povežite zdravlje usluge
Moguće je da možda dođe do problema koji uzrokuju agent za Azure AD povežite zdravlje izgubiti vezu sa servisom Azure AD povezivanje stanja. To obuhvaća problema s mrežom, problemi dozvola ili drugih razloge.

Ako je agenta nije moguće poslati podatke sa servisom Azure AD povežite zdravlje dulje od dva sata, je naznačeno s sljedeće upozorenje na portalu: "Stanje servisa podaci nisu ažurirani." Možete provjeriti je li problematične Azure AD Connect Health agent moći prenositi podataka sa servisom Azure AD povežite zdravlje ponovnim pokretanjem sljedeće naredbe ljuske PowerShell:

    Test-AzureADConnectHealthConnectivity -Role ADFS

Parametar uloga trenutno uzima sljedeće vrijednosti:

- ADFS
- Sinkronizacija
- DODAJE

Zastavice - ShowResults u naredbi možete koristiti da biste pogledali detaljne zapisnika. Koristite u sljedećem primjeru:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

>[AZURE.NOTE]Da biste koristili alat za povezivanje, najprije morate dovršiti agent za registraciju. Ako se ne mogu da biste dovršili registraciju agent, provjerite je li ispunjenim [preduvjeti](active-directory-aadconnect-health-agent-install.md#requirements) za Azure AD povezivanje o stanju. Ovaj test povezivanja provodi po zadanom tijekom agent registracije.



## <a name="related-links"></a>Srodne veze

* [Azure AD Connect stanja](active-directory-aadconnect-health.md)
* [Stanje postupci za Azure AD Connect](active-directory-aadconnect-health-operations.md)
* [Korištenje Azure AD povezivanje stanja AD fs](active-directory-aadconnect-health-adfs.md)
* [Korištenje stanja povezivanje Azure AD za sinkronizaciju](active-directory-aadconnect-health-sync.md)
* [Korištenje Azure AD povezivanje stanje s AD DS](active-directory-aadconnect-health-adds.md)
* [Najčešća pitanja o stanju za Azure AD Connect](active-directory-aadconnect-health-faq.md)
* [Povijest verzija stanja za Azure AD Connect](active-directory-aadconnect-health-version-history.md)
