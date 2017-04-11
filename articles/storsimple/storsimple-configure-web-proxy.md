<properties 
   pageTitle="Postavljanje web proxy za uređaj StorSimple | Microsoft Azure"
   description="Saznajte kako pomoću komponente Windows PowerShell za StorSimple Konfiguriranje postavki web-proxy poslužitelja za svoj uređaj StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-web-proxy-for-your-storsimple-device"></a>Konfiguriranje web proxy za svoj uređaj StorSimple

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča opisuje kako konfigurirati i prikaz web-postavke proxy poslužitelja za svoj uređaj StorSimple pomoću komponente Windows PowerShell za StorSimple. Postavke proxyja web koriste uređaj StorSimple kada komunicirate s oblakom. Proxy poslužitelj web se koristi da biste dodali razinu zaštite filtra sadržaj, predmemorije uvjeta propusnosti lakoća ili čak i pomoć za analize.

Neobavezni konfiguracija za svoj uređaj StorSimple nije web proxy. Možete konfigurirati web proxy samo putem komponente Windows PowerShell za StorSimple. Konfiguracija je dva koraka na sljedeći način:

1. Najprije konfigurirati postavke proxyja web kroz Čarobnjak za postavljanje ili komponente Windows PowerShell za StorSimple cmdleta.

2. Zatim omogućiti konfigurirani web proxy postavki putem komponente Windows PowerShell cmdleti za StorSimple.

Po dovršetku web-konfiguracija proxy poslužitelja za StorSimple postavke proxyja konfigurirani web možete pogledati u servisa Microsoft Azure StorSimple Manager i komponente Windows PowerShell. 

Kad pročitate ovog praktičnog vodiča, će se moći:

- Konfiguriranje web proxy pomoću čarobnjaka za postavljanje i cmdleta
- Omogućivanje web proxy pomoću cmdleta
- Prikaz web-postavke proxyja Azure klasični portalu
- Otklanjanje pogreške tijekom konfiguracija proxy poslužitelja za web


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Konfiguriranje web proxy putem komponente Windows PowerShell za StorSimple

Koristite neku od sljedeće da biste konfigurirali postavke proxyja web:

- Čarobnjak za postavljanje će vas voditi kroz navedeni koraci za konfiguraciju.

- Cmdleti za u ljusci Windows PowerShell za StorSimple.

Svaki od ovih metoda opisan u sljedećim odjeljcima.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Konfiguriranje web proxy putem čarobnjaka za postavljanje

Možete koristiti Čarobnjak za postavljanje će vas voditi kroz korake za web-konfiguracija proxy poslužitelja. Izvršite sljedeće korake da biste konfigurirali web proxy na uređaju.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Da biste konfigurirali web proxy putem čarobnjaka za postavljanje

1. Na izborniku serijsku konzole odaberite mogućnost 1, **prijavite se pomoću puni pristup** , a **uređaj administratorsku lozinku**. Upišite sljedeću naredbu da biste započeli sesiju Čarobnjak za postavljanje:

    `Invoke-HcsSetupWizard`

2. Ako je ovo prvi put koristite čarobnjak za postavljanje za registraciju uređaja, morat ćete konfigurirati sve potrebne mrežne postavke dok ne dođete do web-konfiguracija proxy poslužitelja. Ako vaš uređaj već registriran, možete prihvatiti sve postavke konfigurirani mreže dok ne dođete do web-konfiguracija proxy poslužitelja. U čarobnjaku za postavljanje kada se to od vas zatraži da biste konfigurirali postavke proxyja web, upišite **da**.

3. **Web-URL proxy poslužitelj**, navedite IP adresa ili puni naziv domene (FQDN) web-proxy poslužitelja i broj priključka TCP koji biste željeli uređaj da biste koristili kada komunicirate s oblakom. Pomoću sljedećih oblika:

    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`

    Prema zadanim postavkama nije naveden broj priključka TCP 8080.

4. Odaberite vrstu provjere autentičnosti kao **NTLM**, **Osnovno**ili **ništa**. Osnovni je najnesigurnija provjere autentičnosti za konfiguraciju proxy poslužitelja. NT LAN Manager (NTLM) je vrlo sigurne i složene provjere autentičnosti protokol koji koristi tri način razmjenu sustav (ponekad četiri ako je potrebna dodatna integritet) za provjeru autentičnosti korisnika. Zadana provjera autentičnosti je NTLM. Dodatne informacije potražite u članku [Osnovni](http://hc.apache.org/httpclient-3.x/authentication.html) i [NTLM provjeru autentičnosti](http://hc.apache.org/httpclient-3.x/authentication.html). 

    > [AZURE.IMPORTANT] **U servisu StorSimple Upravitelj grafikoni nadzora uređaj ne funkcioniraju kad je osnovni ili NTLM je omogućena provjera autentičnosti u konfiguraciji proxy poslužitelja za uređaj. Za nadzor grafikone za rad, morat ćete provjerite je li autentičnosti postavljen na NIŠTA.**

5. Ako koristite provjeru autentičnosti, navedite **Korisničko ime Proxy Web** i **Web Proxy lozinku**. Također ćete potvrdite lozinku.

    ![Konfiguriranje Web Proxy na StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

Ako su Registracija uređaja prvi put, nastavite sa registracije. Ako vaš uređaj već registriran, čarobnjak će se zatvoriti. Konfigurirane postavke će se spremiti.

Web proxy sada i bit će omogućena. Možete preskočiti korak [omogućiti web proxy](#enable-web-proxy) i prijeći izravno [Postavke proxyja web prikaza na portalu za Azure klasični](#view-web-proxy-settings-in-the-azure-classic-portal).


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Konfiguriranje web proxy poslužitelj putem komponente Windows PowerShell cmdleti za StorSimple

Drugi način za konfiguriranje postavki za web-proxy je putem komponente Windows PowerShell za cmdleta StorSimple. Izvršite sljedeće korake da biste konfigurirali web proxy.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Da biste konfigurirali web proxy putem cmdleta

1. Na izborniku serijski konzole odaberite mogućnost 1, **prijavite se pomoću puni pristup**. Kada se to od vas zatraži, unesite **uređaj administratorsku lozinku**. Lozinka zadani je `Password1`.

2. U naredbeni redak upišite:

    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`

    Navedite i potvrdite lozinku kada se to od vas zatraži, kao što je prikazano u nastavku.

    ![Konfiguriranje Web Proxy na StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

Web proxy sada je konfiguriran i mora biti omogućeno.

## <a name="enable-web-proxy"></a>Omogućivanje web proxy poslužitelja

Web proxy onemogućeno je prema zadanim postavkama. Nakon konfigurirali postavke proxyja web na uređaju StorSimple, morate koristiti Windows PowerShell za StorSimple da biste omogućili postavke proxyja web.

> [AZURE.NOTE] **Ovaj korak nije bit će potrebno ako ste koristili Čarobnjak za postavljanje da biste konfigurirali web proxy. Web proxy automatski po zadanom je omogućena nakon sesije Čarobnjak za postavljanje.**

U ljusci Windows PowerShell za StorSimple da biste omogućili web proxy na uređaju, učinite sljedeće:

#### <a name="to-enable-web-proxy"></a>Da biste omogućili web proxy poslužitelja

1. Na izborniku serijski konzole odaberite mogućnost 1, **prijavite se pomoću puni pristup**. Kada se to od vas zatraži, unesite **uređaj administratorsku lozinku**. Lozinka zadani je `Password1`.

2. U naredbeni redak upišite:

    `Enable-HcsWebProxy`

    Sada ste omogućili web-konfiguracija proxy na uređaju StorSimple.

    ![Konfiguriranje Web Proxy na StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-classic-portal"></a>Prikaz web-postavke proxyja Azure klasični portalu

Postavke proxyja web konfigurirane putem sučelja komponente Windows PowerShell, a nije moguće promijeniti iz unutar klasični portal. No možete pregledavati te konfigurirane postavke na portalu klasični. Izvršite sljedeće korake da biste pogledali web proxy.

#### <a name="to-view-web-proxy-settings"></a>Da biste pogledali postavke proxy poslužitelja za web
1. Dođite do **servis upravitelja StorSimple > uređaji**. Odaberite i kliknite uređaj, a zatim prijeđite na **Konfiguriraj**.
1. Pomaknite se prema dolje na stranici **Konfiguracija** odjeljak **Web postavke proxyja** . Postavke proxyja konfigurirani web možete pogledati na uređaju StorSimple kao što je prikazano u nastavku.

    ![Prikaz Web Proxy portalu za upravljanje](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)
 
## <a name="errors-during-web-proxy-configuration"></a>Pogreške tijekom konfiguracija proxy poslužitelja za web

Ako ste web-postavke proxyja pogrešno konfigurirana, poruke o pogreškama prikazat će se korisniku u ljusci Windows PowerShell za StorSimple. U sljedećoj se tablici objašnjavaju neke od tih poruka o pogrešci, njihove mogućih uzroka i preporučene akcije.

|Serijski ne.|Kod pogreške HRESULT|Korijenski mogući uzrok|Preporučene akcije|
|:---|:---|:---|:---|
|1.|0x80070001|Naredba Pokreni je s kontrolerom pasivni i se ne možete komunicirati s kontrolerom active.|Pokrenite naredbu na aktivni kontroleru. Da biste pokrenuli naredbu s kontrolerom pasivni, morat ćete riješili problem s povezivanjem s pasivni aktivni kontrolerom. Morat ćete sudjelovati Microsoft Support ako je ovo je veza prekinuta.|
|2.|0x800710dd - identifikator operacije nije valjan|Postavke proxyja nisu podržane na StorSimple virtualnog uređaja.|Postavke proxyja nisu podržane na StorSimple virtualnog uređaja. Te se samo može konfigurirati na StorSimple fizički uređaj.|
|3.|0x80070057 - parametar koji nije valjan|Jedan od parametara za postavke proxyja nije valjan.|URI nije naveden u ispravnom obliku. Pomoću sljedećih oblika:`http://<IP address or FQDN of the web proxy server>:<TCP port number>`|
|4.|0x800706ba - RPC poslužitelj nije dostupan|Uzrok je nešto od sljedećeg:</br></br>Klaster nije prema gore.</br></br>DataPath servis nije pokrenut.</br></br>Iz pasivni kontroler pokretanja naredbe i se ne možete komunicirati s kontrolerom aktivna.|Provjerite sudjelovati Microsoftove Support da biste bili sigurni da skupine prema gore i datapath servis pokrenut.</br></br>Pokrenite naredbu s kontrolerom aktivna. Ako želite da se izvodi naredbu s kontrolerom pasivni, morat ćete osigurati kontrolerom pasivni možete komunicirati s kontrolerom active. Morat ćete sudjelovati Microsoft Support ako je ovo veza prekinuta.|
|5.|0x800706be - RPC poziva nije uspjela|Klaster nije dostupno.|Provjerite sudjelovati Microsoftove Support da biste bili sigurni da je klaster prema gore.|
|6.|0x8007138f - klaster resurs nije pronađen|Platforme servisa klaster resursa nije pronađen. To se može dogoditi kada se instalacija nije proper.|Možda ćete morati izvršiti tvorničke vratiti na uređaju. Možda ćete morati stvoriti resurs platforme. Daljnji koraci zatražite od Microsoft Support.|
|7.|0x8007138c - klaster resursa nije putem Interneta|Platforme ili datapath klaster resursa nisu u mreži.|Obratite se Microsoftovoj Support kako biste bili sigurni jesu li servis resursa datapath i platformu online.|

> [AZURE.NOTE] 
> 
> -  Iznad popisa poruka o pogrešci nije iscrpan. 
> - Pogreške vezane uz postavke proxyja web neće biti prikazano u Azure klasični portal na servisu StorSimple Manager. Ako je došlo do problema s proxyjem web po dovršetku konfiguracije, status uređaja promijenit će se na **izvanmrežno** na portalu klasični. |

## <a name="next-steps"></a>Daljnji koraci

- Ako se pojave problemi pri implementaciji uređaju ili konfiguriranju postavke proxyja web pročitajte [Otklanjanje poteškoća s uređaja uvođenja StorSimple](storsimple-troubleshoot-deployment.md).

- Da biste saznali kako koristiti servis StorSimple Manager, otvorite [Upravitelj StorSimple servis za administriranje StorSimple uređaj](storsimple-manager-service-administration.md).
