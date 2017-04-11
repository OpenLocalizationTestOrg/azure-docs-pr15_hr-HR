<properties 
   pageTitle="Daljinsko povezivanje s uređajem StorSimple | Microsoft Azure"
   description="U članku se objašnjava kako konfigurirati uređaj za daljinsko upravljanje i kako se povezati sa komponente Windows PowerShell za StorSimple putem HTTP ili HTTPS."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/21/2016"
   ms.author="alkohli" />

# <a name="connect-remotely-to-your-storsimple-device"></a>Daljinsko povezivanje s uređajem StorSimple

## <a name="overview"></a>Pregled

Da biste se povezali s uređajem StorSimple možete koristiti remoting komponente Windows PowerShell. Kada se povežete na taj način, nećete vidjeti izbornik. (Vidite izbornik samo ako koristite serijskih konzole na uređaju da biste se povezali.) Pomoću komponente Windows PowerShell remoting, povezati s određenim runspace. Možete odrediti i jezik prikaza. 

Dodatne informacije o korištenju remoting komponente Windows PowerShell za upravljanje uređaju, idite na [Korištenje komponente Windows PowerShell za StorSimple za administriranje uređaju StorSimple](storsimple-windows-powershell-administration.md).

Pomoću ovog praktičnog vodiča objašnjava kako konfigurirati uređaj za daljinsko upravljanje, a zatim kako se povezati sa sustavom Windows PowerShell za StorSimple. HTTP ili HTTPS možete koristiti da biste se povezali putem komponente Windows PowerShell remoting. No kada odlučujete kako se povezati sa sustavom Windows PowerShell za StorSimple, imajte na umu sljedeće: 

- Povezivanje izravno s uređaja konzole za serijski sigurna, ali povezivanje konzoli za serijski putem mreže parametri nije. Budite oprezni od sigurnosni rizik pri povezivanju s uređaja konzole za serijski putem mreže parametri. 

- Povezivanje putem sesiju HTTP možda nudi više sigurnost od povezujete putem konzole za serijski putem mreže. Iako to nije najsigurnija metoda, je prihvatljiva pouzdanih mrežama. 

- Povezivanje putem sesiju HTTPS s samopotpisani certifikat je na najsigurnija i preporučena mogućnost.

Možete se povezati daljinsko sučelje komponente Windows PowerShell. Međutim, daljinski pristup s uređajem StorSimple putem sučelja komponente Windows PowerShell nije omogućen po zadanom. Morate najprije omogućiti daljinskog upravljanja na uređaju, a zatim na klijentskom računalu koja se koristi za pristup uređaju.

Koraci opisani u ovom članku su izvesti na glavno računalo u kojem se izvodi Windows Server 2012 R2.

## <a name="connect-through-http"></a>Povezivanje putem HTTP-a

Povezivanje Windows PowerShell za StorSimple kroz sesiju HTTP nudi više sigurnost od povezujete putem konzole za serijski StorSimple uređaja. Iako to nije najsigurnija metoda, je prihvatljiva pouzdano mrežama.

Možete koristiti portala za Azure klasični ili serijski konzole za konfiguriranje daljinskog upravljanja. Odaberite neku od sljedećih postupaka:

- [Omogućivanje daljinskog upravljanja putem HTTP-a pomoću portala za Azure klasični](#use-the-azure-classic-portal-to-enable-remote-management-over-http)

- [Pomoću serijskog konzole možete omogućiti daljinsko upravljanje putem HTTP-a](#use-the-serial-console-to-enable-remote-management-over-http)

Kada omogućite daljinsko upravljanje sljedećim postupkom da biste pripremili klijent za daljinske veze.

- [Priprema klijenta za udaljene](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-http"></a>Omogućivanje daljinskog upravljanja putem HTTP-a pomoću portala za Azure klasični 

Na portalu Azure klasični da biste omogućili daljinsko upravljanje putem HTTP-a poduzeti sljedeće korake.

#### <a name="to-enable-remote-management-through-the-azure-classic-portal"></a>Omogućivanje daljinskog upravljanja putem Azure klasični portala

1. Pristup **uređaji** > **Konfiguriraj** za svoj uređaj.

2. Pomaknite se prema dolje do odjeljka **Daljinskog upravljanja** .

3. **Omogućivanje daljinskog upravljanja** postavljeno na **da**.

4. Sada možete povezati putem HTTP. (Zadano je da biste se povezali putem HTTP.) Provjerite je li odabran HTTP-a.

    >[AZURE.NOTE] Povezivanje putem HTTP-a je prihvatljiva samo na pouzdano mrežama.

6. Kliknite **Spremi** pri dnu stranice.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>Pomoću serijskog konzole možete omogućiti daljinsko upravljanje putem HTTP-a

Na konzoli serijski uređaj da biste omogućili daljinsko upravljanje poduzeti sljedeće korake.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Da biste omogućili daljinsko upravljanje kroz serijski konzole za uređaj

1. Na izborniku serijski konzole odaberite mogućnost 1. Dodatne informacije o korištenju konzole za serijski na uređaju otvorite [Windows PowerShell za StorSimple putem uređaja serijski konzole za povezivanje](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. U naredbeni redak upišite:`Enable-HcsRemoteManagement –AllowHttp`

3. Bit ćete obaviješteni o sigurnosni Nedostaci korištenja HTTP da biste se povezali s uređajem. Kada se to od vas zatraži, potvrdite tako da upišete **Y**.

4. Provjerite je li omogućena HTTP tako da upišete:`Get-HcsSystem`

5. Provjerite je li pokazuje da polje **RemoteManagementMode** **HttpsAndHttpEnabled**. Sljedeća ilustracija prikazuje ove postavke u PuTTY.

     ![Serijski HTTPS i HTTP omogućeno](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>Priprema klijenta za udaljene

Na klijentskom računalu da biste omogućili daljinsko upravljanje poduzeti sljedeće korake.

#### <a name="to-prepare-the-client-for-remote-connection"></a>Priprema klijenta za udaljene

1. Stvaranje sesije komponente Windows PowerShell kao administrator.

2. Upišite sljedeću naredbu da biste dodali IP adresa uređaja StorSimple popis pouzdanih domaćini klijentskog programa: 

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

     Zamjena <*device_ip*> IP adresa uređaja; Ako, na primjer: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Upišite sljedeću naredbu da biste spremili vjerodajnice uređaj u varijablu: 

     *$cred = get-vjerodajnica*

4. U dijaloškom okviru koji će se prikazati:

    1. Unesite korisničko ime u ovom obliku: *device_ip\SSAdmin*.
    2. Unesite lozinku administratora uređaja koji je postavljen kad je uređaj konfiguriran pomoću čarobnjaka za postavljanje. Lozinka zadani je *Password1*.

7. Stvaranje sesije komponente Windows PowerShell na uređaju tako da upišete sljedeću naredbu:

     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`

     >[AZURE.NOTE] Da biste stvorili sesije komponente Windows PowerShell za korištenje StorSimple virtualnog uređaja, dodavanje u `–Port` parametar i navedite priključak javno koje ste konfigurirali u Remoting za StorSimple virtualne uređaj.

     Sada mora imati aktivan udaljene komponente Windows PowerShell sesiju na uređaju.

    ![PowerShell remoting putem HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>Povezivanje putem HTTP

Povezivanje Windows PowerShell za StorSimple kroz sesiju HTTPS je siguran i preporučeni način daljinski povezivanja s uređajem Microsoft Azure StorSimple. Sljedećih postupaka objašnjavaju kako postaviti serijski konzole i klijentskog računala da bi mogli koristiti HTTPS za povezivanje s komponente Windows PowerShell StorSimple.

Možete koristiti portala za Azure klasični ili serijski konzole za konfiguriranje daljinskog upravljanja. Odaberite neku od sljedećih postupaka:

- [Omogućivanje daljinskog upravljanja putem HTTP pomoću portala za Azure klasični](#use-the-azure-classic-portal-to-enable-remote-management-over-https)

- [Pomoću serijskog konzole možete omogućiti daljinsko upravljanje putem HTTP](#use-the-serial-console-to-enable-remote-management-over-https)

Kada omogućite daljinsko upravljanje, koristite sljedeće postupke za pripremu glavno računalo za daljinsko upravljanje i povezivanje na uređaju s udaljenog računala.

- [Priprema glavno računalo za daljinsko upravljanje](#prepare-the-host-for-remote-management)

- [Povezivanje s uređaja s udaljenog računala](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-https"></a>Omogućivanje daljinskog upravljanja putem HTTP pomoću portala za Azure klasični

Na portalu Azure klasični da biste omogućili daljinsko upravljanje putem HTTP poduzeti sljedeće korake.

#### <a name="to-enable-remote-management-over-https-from-the-azure-classic-portal"></a>Da biste omogućili daljinsko upravljanje putem HTTP s portala za Azure klasični

1. Pristup **uređaji** > **Konfiguriraj** za svoj uređaj.

2. Pomaknite se prema dolje do odjeljka **Daljinskog upravljanja** .

3. **Omogućivanje daljinskog upravljanja** postavljeno na **da**.

4. Sada možete povezati pomoću HTTPS. (Zadano je da biste se povezali putem HTTP.) Provjerite je li odabran HTTPS. 

5. Kliknite **Preuzmite certifikat za daljinsko upravljanje**. Navedite mjesto na koje želite spremiti datoteku. Morat ćete instalirati certifikat na računalu klijentskog ili glavno računalo koje ćete koristiti za povezivanje s uređaja.

6. Kliknite **Spremi** pri dnu stranice.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>Pomoću serijskog konzole možete omogućiti daljinsko upravljanje putem HTTP

Na konzoli serijski uređaj da biste omogućili daljinsko upravljanje poduzeti sljedeće korake.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Da biste omogućili daljinsko upravljanje kroz serijski konzole za uređaj

1. Na izborniku serijski konzole odaberite mogućnost 1. Dodatne informacije o korištenju konzole za serijski na uređaju otvorite [Windows PowerShell za StorSimple putem uređaja serijski konzole za povezivanje](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. U naredbeni redak upišite: 

     `Enable-HcsRemoteManagement`

    To trebate omogućiti HTTPS na uređaju.

3. Provjerite da je omogućena HTTPS tako da upišete: 

     `Get-HcsSystem`

    Pripazite da **RemoteManagementMode** prikazuje **HttpsEnabled**. Sljedeća ilustracija prikazuje ove postavke u PuTTY.

     ![Serijski HTTPS omogućeno](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)

4. Mapiraj `Get-HcsSystem`, kopirajte serijski broj uređaja i spremite je za kasnije korištenje.

    >[AZURE.NOTE] Serijski broj karte nazivu CN certifikata.

5. Daljinsko upravljanje certifikat možete dobiti tako da upišete: 
 
     `Get-HcsRemoteManagementCert`

    Pojavit će se certifikat otprilike ovako.

    ![Daljinsko upravljanje gumba](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)

5. Kopirajte podatke na certifikatu iz **---Početak certifikat---** da biste **---ZAVRŠILI certifikat---** u uređivaču teksta kao što je blok za pisanje, a zatim ga spremite kao .cer datoteka. (Će kopirate datoteku glavno računalo za udaljene prilikom pripreme domaćin.)

    >[AZURE.NOTE] Da biste generirali nove certifikata, koristite na `Set-HcsRemoteManagementCert` cmdlet.

### <a name="prepare-the-host-for-remote-management"></a>Priprema glavno računalo za daljinsko upravljanje

Da biste pripremili glavno računalo za udaljenu koji koristi sesiju HTTPS, učinite sljedeće:

- [Uvoz .cer datoteka u spremište korijenskih klijenta ili udaljenog glavnog računala](#to-import-the-certificate-on-the-remote-host).

- [Dodavanje uređaja serijske brojeve u datoteku glavnog računala na udaljenom računalu koje hostira](#to-add-device-serial-numbers-to-the-remote-host).

Svaki od tih postupaka je opisan u nastavku.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Da biste uvezli certifikata na udaljenom računalu koje hostira

1. Desnom tipkom miša kliknite .cer datoteka, a zatim odaberite **Instalacija certifikata**. To će se pokrenuti čarobnjak za uvoz certifikata.

    ![Čarobnjak za uvoz certifikata 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)

2. **Mjesto spremišta**, odaberite **Lokalno računalo**pa zatim kliknite **Dalje**.

3. Odaberite **postavite sve certifikate u sljedeće spremište**, a zatim **Pregledaj**. Dođite do Spremište korijenskih vaše udaljenog računala koje hostira, a zatim kliknite **Dalje**.

    ![Čarobnjak za uvoz certifikata 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)

4. Kliknite **Završi**. Pojavljuje se poruka koja vas obavještava da je uvoz uspješan.

    ![Čarobnjak za uvoz certifikata 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Da biste dodali uređaj serijske brojeve udaljeno glavno računalo

1. Pokrenite Blok za pisanje kao administrator, a zatim otvorite datoteku hosts nalazi se na \Windows\System32\Drivers\etc.

2. Dodavanje sljedeće tri stavke u datoteku glavnog računala: **podataka 0 IP adresa**, **0 fiksnim IP adresu**i **kontroler 1 fiksnim IP adresa**.

3. Unesite serijski broj uređaja koje ste ranije spremili. Mapirajte ovo se s IP adresom kao što je prikazano na sljedećoj slici. Kontroler 0 i 1 kontroler, dodavanje **Controller0** i **Controller1** na kraju serijski broj (CN naziv).

    ![Dodajte naziv CN u hosts datoteka](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)

4. Spremite datoteku domaćini.

### <a name="connect-to-the-device-from-the-remote-host"></a>Povezivanje s uređaja s udaljenog računala

Unos znakova pomoću komponente Windows PowerShell i SSL sesiju SSAdmin na uređaju s udaljenog glavnog računala ili klijenta. Sesije SSAdmin mapira mogućnost 1 na izborniku [serijski konzole](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console) uređaja.

Na računalu s kojeg želite da bude udaljene komponente Windows PowerShell, slijedite ovaj postupak.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Da biste unijeli sesiju SSAdmin na uređaju pomoću komponente Windows PowerShell i SSL

1. Stvaranje sesije komponente Windows PowerShell kao administrator.

2. Dodajte IP adresa uređaja klijenta pouzdanih domaćini tako da upišete:

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

    Gdje je <*device_ip*> IP adresa uređaja; Ako, na primjer: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Stvorite novu vjerodajnice tako da upišete: 

     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`

    Gdje je <*IP ciljni uređaj*> IP adresu podataka 0 za svoj uređaj na primjer, **10.126.173.90** kao što je prikazano u prethodnom slika u datoteci Hosts. Osim toga, navedite administratorsku lozinku za svoj uređaj.

4. Stvaranje sesije tako da upišete:

     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`

    Za parametar - ComputerName cmdlet Navedite <*serijski broj ciljni uređaj*>. U ovom serijski broj je pridruženo se s IP adresom podataka 0 u datoteci hosts na udaljenom računalu koje hostira; na primjer, **SHX0991003G44MT** kao što je prikazano na sljedećoj slici.

5. Vrsta: 

     `Enter-PSSession $session`

6. Morat ćete Pričekajte nekoliko minuta, a zatim koji će se povezati s uređajem putem HTTP putem SSL. Vidjet ćete poruku koja označava da su povezani s uređajem.

    ![PowerShell remoting pomoću HTTPS i SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o [korištenju komponente Windows PowerShell za administriranje StorSimple uređaj](storsimple-windows-powershell-administration.md).

- Dodatne informacije o [korištenju StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).
