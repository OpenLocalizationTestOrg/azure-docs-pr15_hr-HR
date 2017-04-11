## <a name="download-install-and-register-the-azure-backup-agent"></a>Preuzimanje, instaliranje i registrirati agent za sigurnosno kopiranje Azure

Kada stvorite sigurnosnu kopiju Azure sigurnog, agent trebala biti instalirana na svim sustava Windows strojeva (Windows Server, Windows klijent, upravitelja za zaštitu podataka centar sustava poslužitelja ili pokrenite poslužitelj za Azure sigurnosne kopije) koja omogućuje sigurnosno kopiranje podataka i aplikacijama Azure.

1. Prijava na [Portal za upravljanje](https://manage.windowsazure.com/)

2. Kliknite **Oporavak servise**, a zatim odaberite sigurnog sigurnosne kopije koju želite da biste registrirali s poslužiteljem. Pojavljuje se stranica brzi početak rada za tu sigurnosno kopiranje zbirke ključeva.

    ![Brzi početak rada](./media/backup-install-agent/quickstart.png)

3. Na stranici za brzo pokretanje kliknite mogućnost **za Windows Server ili upravitelja za zaštitu podataka centar sustava ili Windows klijenta** u odjeljku **Agent za preuzimanje**. Kliknite **Spremi** da biste ga kopirali na lokalnom računalu.

    ![Spremanje agent](./media/backup-install-agent/agent.png)

4. Kad instalirate agenta, dvaput pritisnite MARSAgentInstaller.exe da biste pokrenuli instalaciju agent za Azure sigurnosnu kopiju. Odaberite instalacijsku mapu i privremeno mape potreban za agenta. Mjesto predmemorije naveden, morate imati slobodnog prostora koji je barem 5% sigurnosne kopije podataka.

5.  Ako koristite proxy poslužitelj za povezivanje s Internetom, na zaslonu za **konfiguraciju Proxy** unesite detalje proxy poslužitelj. Ako koristite čija je autentičnost provjerena proxy poslužitelj, unesite korisničko ime i lozinku pojedinosti u ovaj zaslon.

6.  Agent za sigurnosne kopije Azure instalira .NET Framework 4,5 i komponente Windows PowerShell (ako ona još nije dostupan) da biste dovršili instalaciju.

7.  Kad instalirate agenta, kliknite gumb **nastavili za registraciju** da biste nastavili s tijekom rada.

    ![Registrirajte se](./media/backup-install-agent/register.png)

8. Na zaslonu za vjerodajnice sigurnog pronađite i odaberite datoteku sigurnog vjerodajnica koje već preuzeli.

    ![Sigurnog vjerodajnice](./media/backup-install-agent/vc.png)

    Datoteka vjerodajnice sigurnog vrijedi samo za 48 sati (nakon preuzimanja s portala sustava). Ako naići na sve pogreške u ovaj zaslon (npr. "sigurnog vjerodajnice datoteke navedene istekla"), prijavite se na portal za Azure i ponovno preuzeti datoteku sigurnog vjerodajnice.

    Provjerite je li datoteka vjerodajnice sigurnog dostupna na mjestu na kojem se može pristupiti putem aplikacije za postavljanje. Ako naiđete na pristup povezane pogreške, kopirajte sigurnog vjerodajnice datoteku da biste privremeno mjesto u ovom računalu, a zatim ponovite postupak.

    Ako naiđete na pogrešku koji nisu valjani sigurnog vjerodajnica (npr. "danih koji nisu valjani sigurnog vjerodajnica") datoteka je oštećena ili se ne najnovije vjerodajnice pridružili servis za oporavak. Ponovite operaciju nakon preuzimanja nove zbirke ključeva vjerodajnica na portalu. Ta se pogreška obično je vidjeti ako korisnik klikne na mogućnost **preuzimanja sigurnog vjerodajnica** Azure portalu brzi redom. U ovom slučaju samo drugi sigurnog vjerodajnica datoteka je valjan.

9. Na zaslonu **Postavke šifriranja** možete generirati pristupni izraz ili pružaju pristupni izraz (najmanje 16 znakova). Imajte na umu da biste spremili pristupni izraz u sigurnom mjestu.

    ![Šifriranje](./media/backup-install-agent/encryption.png)

    > [AZURE.WARNING] Ako izgubite ili zaboravili; pristupni izraz Microsoft ne može pomoći u oporavak sigurnosne kopije podataka. Krajnji korisnik vlasništvu pristupni izraz za šifriranje i Microsoft nema uvid u pristupni izraz koristi krajnjeg korisnika. Spremite datoteku na sigurnom mjestu kao što je potrebno tijekom operacije za oporavak.

10. Kada kliknete gumb **Završi** , stroj uspješno je registrirana na sigurnog i sada ste spremni za početak sigurnosnom do Microsoft Azure.

11. Kada koristite Microsoft Azure Backup samostalnu možete izmijeniti postavke određene tijekom tijeka rada za registraciju klikom na mogućnost **Promijeni svojstva** u blog poravnanja Azure sigurnosnu kopiju u.

    ![Promjena svojstava](./media/backup-install-agent/change.png)

    Umjesto toga kada koristite Upravitelj zaštite podataka, možete izmijeniti postavke određene tijekom tijeka rada za registraciju klikom na mogućnost **Konfiguracija** tako da odaberete **Online** na kartici **Upravljanje** .

    ![Konfiguriranje Azure sigurnosnog kopiranja](./media/backup-install-agent/configure.png)
