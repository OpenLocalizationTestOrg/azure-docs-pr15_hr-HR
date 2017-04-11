<properties 
   pageTitle="PowerShell za upravljanje uređajima StorSimple | Microsoft Azure"
   description="Saznajte kako upravljati uređajem StorSimple pomoću komponente Windows PowerShell za StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli@microsoft.com" />

# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>Pomoću komponente Windows PowerShell za StorSimple administriranje uređaju

## <a name="overview"></a>Pregled

Windows PowerShell za StorSimple sadrži sučelje naredbenog retka koje možete koristiti da biste upravljali Microsoft Azure StorSimple uređaj. Kao što je naziv Predloži je sučeljem naredbenog retka komponente Windows PowerShell temelji, koja je ugrađena u ograničenog runspace. Iz perspektive korisnika u naredbenom retku, ograničenog runspace prikazuje se kao ograničeni verziju sustava Windows PowerShell. Zadržavanje neke od osnovnih mogućnosti komponente Windows PowerShell, ovo sučelje ima dodatne namjenski cmdletima koji se usmjerena prema upravljanje Microsoft Azure StorSimple uređajem. 

U ovom se članku opisuje komponente Windows PowerShell za StorSimple značajke, uključujući kako se možete povezati s ovog sučelja i sadrži veze na detaljne postupke ili tijekovi rada koje možete izvršiti pomoću ovog sučelja. Tijekovi rada obuhvaćaju da biste registrirali uređaj, konfiguriranje sučelja mrežom na uređaju, instalirajte ažuriranja koja zahtijevaju uređaju da biste se u načinu održavanja, promijenite stanje uređaja i otklanjanje poteškoća na koje možete naići.

Kad pročitate članak će se moći:

- Povezivanje s uređajem StorSimple pomoću komponente Windows PowerShell za StorSimple.

- Upravljati pomoću komponente Windows PowerShell za StorSimple StorSimple uređaj.

- Zatražite pomoć u ljusci Windows PowerShell za StorSimple.

>[AZURE.NOTE]   

>- Windows PowerShell za StorSimple cmdleta omogućuju upravljanje StorSimple uređaj s konzole za serijski ili daljinski putem komponente Windows PowerShell remoting. Dodatne informacije o svakoj pojedinačne cmdletima koji se može koristiti u ovom sučelja, idite na [Pregled cmdlet za Windows PowerShell za StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

>- Cmdleti za Azure PowerShell StorSimple su drugi skup cmdletima koji omogućuju vam da biste automatizirali StorSimple razini usluge i migracije iz naredbenog retka. Dodatne informacije o Azure PowerShell cmdleti za StorSimple, idite na [Azure StorSimple cmdlet referencu](https://msdn.microsoft.com/library/azure/dn920427.aspx).

Možete pristupiti komponente Windows PowerShell za StorSimple na jedan od sljedećih načina:

- [Povezivanje s StorSimple uređaj serijski konzole](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
- [Daljinsko povezivanje s StorSimple pomoću komponente Windows PowerShell](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)
    

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Povezivanje sa komponente Windows PowerShell za StorSimple putem serijski konzole za uređaj

Možete [preuzeti PuTTY](http://www.putty.org/) ili slične terminal emulacija softver za povezivanje s komponente Windows PowerShell StorSimple. Morate konfigurirati PuTTY isključivo za pristup Microsoft Azure StorSimple uređaja. Sljedeće teme sadrže detaljne upute o tome kako konfigurirati PuTTy i povezati s uređajem. Različite mogućnosti izbornika na konzoli za serijski su i dodataka.

### <a name="putty-settings"></a>PuTTY postavke

Provjerite je li koristiti sljedeće PuTTY postavke za povezivanje s sučelja komponente Windows PowerShell konzoli sustava serijski.

#### <a name="to-configure-putty"></a>Da biste konfigurirali PuTTY

1. U dijaloškom okviru PuTTY **Ponovno konfiguriranje** u oknu **kategorija** odaberite **tipkovnica**.

2. Provjerite jesu li sljedećih mogućnosti (to su zadane postavke kada pokrenite novu sesiju). 

  	|Stavke tipkovnice|Odaberite|
  	|---|---|
  	|Tipka Backspace|Kontrola-? (127)|
  	|Tipke HOME i End|Standardna|
  	|Funkcijske tipke i tipkovnica|ESC [n ~|
  	|Početno stanje tipke sa strelicama|Normalno|
  	|Početno stanje numeričke tipkovnice|Normalno|
  	|Omogućivanje značajki dodatni tipkovnice|Kontrola Alt razlikuje se od AltGr|

    ![Podržani Putty postavke](./media/storsimple-windows-powershell-administration/IC740877.png)

3. Kliknite **Primijeni**.

4. U oknu **kategorija** odaberite **Prijevod**.

5. U okviru popisa **skup znakova Remote** odaberite **UTF-8**.

6. U odjeljku **rukovanje znakova za crtanje**, odaberite **Koristi Unicode crtež kodne točke**. Sljedeća ilustracija prikazuje ispravni PuTTY odabrane mogućnosti.

    ![UTF Putty postavke](./media/storsimple-windows-powershell-administration/IC740878.png)

7. Kliknite **Primijeni**.


Sada možete koristiti PuTTY za povezivanje s konzole serijski uređaj tako da učinite sljedeće korake.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]


### <a name="about-the-serial-console"></a>O konzoli za serijski

Prilikom pristupa komponente Windows PowerShell raspoređene su sučelja uređaja StorSimple kroz serijski konzoli poruke natpis, nakon čega slijedi mogućnosti izbornika. 

Natpis poruka sadrži osnovni StorSimple informacije o uređaju kao što su model, naziv, verzija instaliranog softvera i status kontrolera pristupate. Na sljedećoj je slici prikazan primjer natpis poruke.

![Serijski natpis poruke](./media/storsimple-windows-powershell-administration/IC741098.png)

>[AZURE.IMPORTANT] Natpis poruka možete koristiti da biste odredili li kontrolerom ste povezani s aktivan "ili" pasivni.

Sljedeća slika prikazuje različite mogućnosti runspace koji su dostupni na izborniku serijski konzolu.

![Registracija uređaja 2](./media/storsimple-windows-powershell-administration/IC740906.png)

Na raspolaganju su vam sljedeće postavke:

1. **Prijavite se pomoću potpuni pristup** Ta mogućnost omogućuje povezivanje (s početnim vjerodajnice) runspace **SSAdminConsole** na lokalni kontroleru. (Lokalni kontroler je kontroler koji su trenutno pristupa putem serijski konzole za uređaj StorSimple). Ta mogućnost se može koristiti i da biste omogućili Microsoftove Support da biste pristupili neograničeni runspace (sesije podrška) za otklanjanje poteškoća moguće uređaj. Kada koristite mogućnost 1 da biste se prijavili, možete omogućiti inženjeringom Microsoft Support da biste pristupili neograničeni runspace tako da pokrenete određene cmdlet. Dodatne informacije pogledajte [Stvaranje sesije podrška](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).

2. **Prijavite se u sustav kontroler ravnopravnih članova s potpunim pristupom** Ta je mogućnost isto kao mogućnost 1, osim što se možete povezati (s početnim vjerodajnice) runspace **SSAdminConsole** na kontroleru ravnopravnih članova. Budući da je uređaj StorSimple uređaj visoke dostupnosti s dva kontrolera u konfiguraciji aktivno pasivni, ravnopravnih članova odnosi se na kontroler uređaju koji se pristupa putem konzole za serijski).
Slično mogućnost 1, tu mogućnost također se poslužite da biste omogućili Microsoftove Support da biste pristupili neograničeni runspace na kontroler ravnopravnih članova.

3. **Povezivanje s ograničeni pristup** Ova se mogućnost koristi sučelja komponente Windows PowerShell u načinu rada za ograničeni pristup. Neće tražiti vjerodajnice za pristup. Ta mogućnost povezuje ograničeniji runspace u usporedbi s mogućnosti 1 i 2.  Neke zadatke koji su dostupni putem mogućnost 1 koji **nije* moguće izvesti u ovom runspace su:

    - Vraćanje na tvorničke postavke
    - Promjena lozinke
    - Omogućavanje ili onemogućavanje pristupa za podršku
    - Primjena ažuriranja
    - Instalirajte hitni popravci 
                                                

    >[AZURE.NOTE] **Ako ste zaboravili lozinku administratora uređaja, a ne možete povezati putem mogućnost 1 ili 2, to je željenu mogućnost.**

4. **Promjena jezika** Ta mogućnost omogućuje promjenu jezika prikaza u sučelje komponente Windows PowerShell. Jezici podržani su engleski, japanski, ruski, francuski, korejski Jug, španjolski, talijanskom, njemački, kineski i brazilski portugalski.


## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>Daljinsko povezivanje s StorSimple pomoću komponente Windows PowerShell za StorSimple

Da biste se povezali s uređajem StorSimple možete koristiti remoting komponente Windows PowerShell. Kada se povežete na taj način, nećete vidjeti izbornik. (Vidite izbornik samo ako koristite serijskih konzole na uređaju da biste se povezali. Povezivanje daljinski vas vodi izravno ekvivalent "mogućnost 1 – puni pristup" na konzoli za serijski.) Pomoću komponente Windows PowerShell remoting, povezati s određenim runspace. Možete odrediti i jezik prikaza. 

Jezik prikaza ne ovisi o jeziku koji ste postavili pomoću mogućnosti **Promjena jezika** na izborniku serijsku konzole. Udaljene ljuske PowerShell automatski će obraditi regionalne postavke uređaja s kojeg se povezujete ako ništa nije naveden.

>[AZURE.NOTE] Ako radite s web-mjesto Microsoft Azure virtualne domaćini i u okvir za StorSimple virtualne uređajima pomoću komponente Windows PowerShell remoting i virtualnog glavnog računala za povezivanje s virtualnog uređaja. Ako ste postavili mjesto za zajedničko korištenje na glavnom računalu za spremanje informacija iz sesije komponente Windows PowerShell, imajte na umu da svi glavni sadrži samo korisnici čija je autentičnost provjerena. Dakle, ako ste postavili zajedničko korištenje da biste omogućili pristup svima i povezani ste bez navođenja vjerodajnice, koristit će se Neprovjereni anonimni glavnicu i prikazat će se pogreška. Da biste riješili taj problem, na glavno računalo koje morate omogućiti gosta i zatim dodijelite za goste puni pristup računu za zajedničko korištenje ili morate navesti valjanih se vjerodajnica uz cmdleta ljuske Windows PowerShell.

HTTP ili HTTPS možete koristiti da biste se povezali putem komponente Windows PowerShell remoting. Poslužite se uputama u vodičima za sljedeće:

- [Povezivanje daljinski putem HTTP](storsimple-remote-connect.md#connect-through-http)
- [Uspostavi daljinski HTTPS](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Sigurnosna pitanja vezana uz veze

Kada odlučujete kako se povezati sa sustavom Windows PowerShell za StorSimple, imajte na umu sljedeće:

- Povezivanje izravno s uređaja konzole za serijski sigurna, ali povezivanje konzoli za serijski putem mreže parametri nije. Budite oprezni od sigurnosni rizik prilikom povezivanja s uređajem serijski putem mreže parametri.

- Povezivanje putem sesiju HTTP možda nudi više sigurnost od povezujete putem konzole za serijski putem mreže. Iako to nije najsigurnija metoda, je prihvatljiva pouzdano mrežama.

- Povezivanje putem sesiju HTTPS je na najviše razine sigurnosti i preporučena mogućnost.


## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>Administriranje uređaju StorSimple pomoću komponente Windows PowerShell za StorSimple
Sljedeća tablica prikazuje sažetak uobičajene zadatke upravljanja i složenih tijekova rada koji se mogu obaviti unutar sučelja komponente Windows PowerShell StorSimple uređaja. Dodatne informacije o svaki tijek rada kliknite odgovarajuću stavku u tablici.

#### <a name="windows-powershell-for-storsimple-workflows"></a>Windows PowerShell za StorSimple tijekove rada

|Ako želite da biste to učinili...|Koristite ovaj postupak.|
|---|---|
|Registracija uređaja|[Konfigurirati i registrirati uređaja pomoću komponente Windows PowerShell za StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
|Konfiguriranje web proxy poslužitelja</br>Prikaz web-proxy postavke|[Konfiguriranje web proxy za svoj uređaj StorSimple](storsimple-configure-web-proxy.md)|
|Izmjena postavke sučelja podataka 0 mrežom na uređaju|[Izmjena podataka 0 mrežno sučelje za svoj uređaj StorSimple](storsimple-modify-data-0.md)|
|Zaustavljanje kontroler </br> Pokrenite ili isključiti kontroler </br> Isključite uređaj</br>Ponovno postavljanje uređaja na tvorničke postavke.|[Upravljanje kontrolera uređaja](storsimple-manage-device-controller.md)|
|Instalirajte web-mjesto održavanja način ažuriranja i u okvir za hitne|[Ažuriranje uređaja](storsimple-update-device.md)|
|Unesite održavanja </br>Izlaz iz načina održavanja|[Načini rada s uređajem StorSimple](storsimple-device-modes.md)|
|Stvaranje paketa za podršku</br>Šifriranje i uređivanje paket za podršku|[Stvaranje i upravljanje paket za podršku](storsimple-create-manage-support-package.md)|
|Stvaranje sesije podrška</br>|[Stvaranje sesije podrška u ljusci Windows PowerShell za StorSimple](/storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)
 

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Zatražite pomoć u ljusci Windows PowerShell za StorSimple

U komponente Windows PowerShell za StorSimple cmdlet pomoć je dostupna. U mreži, najnovije verziji ovu pomoć je dostupno i, koju možete koristiti da biste ažurirali pomoć na računalu.

Pristup pomoći u ovo sučelje je sličan onome u ljusci Windows PowerShell pa će funkcionirati Većina cmdleta vezane uz pomoć. Pomoć za Windows PowerShell online možete pronaći u biblioteci TechNet: [skriptiranje pomoću komponente Windows PowerShell](http://go.microsoft.com/fwlink/?LinkID=108518).

Slijedi kratak opis vrste pomoći za ovo sučelje komponente Windows PowerShell, uključujući kako ažurirati pomoć.

#### <a name="to-get-help-for-a-cmdlet"></a>Da biste dobili pomoć za na cmdlet

- Da biste dobili pomoć za cmdlet ni (funkcija), koristite sljedeću naredbu:`Get-Help <cmdlet-name>`

- Da biste Mrežna pomoć za sve cmdlet, koristite cmdlet prethodne s na `-Online` parametar:`Get-Help <cmdlet-name> -Online`

- Pomoć za cijeli možete koristiti u `–Full` parametar i primjera, koristite na `–Examples` parametar.

#### <a name="to-update-help"></a>Da biste ažurirali pomoći

Jednostavno možete ažurirati u pomoći sučelja komponente Windows PowerShell. Izvršite sljedeće korake da biste ažurirali pomoć na računalu.

#### <a name="to-update-cmdlet-help"></a>Da biste ažurirali cmdlet pomoći

1. Pokrenite Windows PowerShell s mogućnošću **Pokreni kao administrator** .

1. U naredbeni redak upišite: `Update-Help`

1. Instalirat će se ažurirane datoteke pomoći.

1. Nakon instalacije datoteke pomoći upišite: `Get-Help Get-Command`. Prikazat će popis cmdleta za koje je dostupna pomoć.


>[AZURE.NOTE] Da biste dobili popis dostupnih cmdleta u na runspace, prijavite se na odgovarajuću mogućnost izbornika i pokretanje u `Get-Command` cmdlet.

## <a name="next-steps"></a>Daljnji koraci
Ako se pojave problemi s uređajem StorSimple prilikom izvršavanja jedan od gore tijekova rada, pročitajte [alata za otklanjanje poteškoća s implementacijama StorSimple](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

