<properties 
    pageTitle="Pomoću tipki SSH sa sustavom Windows za Linux VMs | Microsoft Azure" 
    description="Saznajte kako stvoriti i povezati Linux virtualnog računala na Azure pomoću tipke SSH na računalu sa sustavom Windows." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="squillace" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="rasquill"/>

# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Upute za korištenje SSH tipke sa sustavom Windows na Azure

> [AZURE.SELECTOR]
- [Windows](virtual-machines-linux-ssh-from-windows.md)
- [Linux/Mac](virtual-machines-linux-mac-create-ssh-keys.md)

Kada se povežete s Linux virtualnim strojevima (VMs) u Azure, poslužite se [javni ključ šifriranja](https://wikipedia.org/wiki/Public-key_cryptography) omogućuju sigurnije način da biste se prijavili vašem VM Linux. Tom se postupku podrazumijeva u javnim i privatnim ključ sustava exchange pomoću naredbe ljuske za sigurnu (SSH) za provjeru autentičnosti sami umjesto korisničko ime i lozinku. U lozinkama se podložno grube sile napada, osobito na mjesto na Internetu VMs kao što je web-poslužiteljima. Ovaj članak sadrži pregled SSH tipke i upute za generiranje odgovarajuće tipke na računalu sa sustavom Windows.


## <a name="overview-of-ssh-and-keys"></a>Pregled SSH i ključevi

Možete sigurno prijave u vašem VM Linux pomoću javni i privatni ključevi:

- **Javni ključ** nalazi se na vaše VM Linux ili bilo koji servis koji želite koristiti s šifriranja javni ključ.
- **Privatni ključ** je što izlaganja vaš VM Linux kada se prijavite, potvrdite svoj identitet. Zaštita ovaj privatni ključ. Ne je zajednički koristiti.

Ove javni i privatni ključevi može se koristiti na više VMs i usluge. Dvije vrste ključeva nije potrebno za svaku VM ili servis koji želite pristupiti. Detaljnije pregled, potražite u članku [šifriranja javni ključ](https://wikipedia.org/wiki/Public-key_cryptography).

SSH je protokol za šifriranu vezu koja omogućuje sigurne prijave putem nezaštićenu veza. To je zadani protokol veze za Linux VMs smješten u Azure. Iako SSH sam nudi šifriranu vezu, pomoću lozinke SSH veza i dalje ostavlja na VM podložno grube sile napada ili guessing lozinki. Više siguran i Preferirani, način povezivanja VM pomoću SSH je pomoću ove javni i privatni ključevi, poznata i kao SSH tipke.

Ako ne želite koristiti SSH tipke, možete i dalje prijave u vašem Linux VMs pomoću lozinke. Ako vaš VM je izložen putem Interneta, možda će biti dovoljni pomoću lozinke. Međutim, i dalje morate za upravljanje lozinkama za svaki VM Linux i održavanje dobar lozinki i postupci, kao što su minimalnu duljinu lozinke i redovito ažuriranje. Korištenje tipkovnih SSH smanjuje složenost upravljanje pojedinačnim vjerodajnice preko više VMs.


## <a name="windows-packages-and-ssh-clients"></a>Paketi za Windows i SSH klijenti

Povezivanje s i upravljanje Linux VMs u Azure pomoću **ssh** klijenta. Računala sa sustavom Windows obično nemaju **ssh** klijent za instaliran. Uobičajeni Windows SSH klijenti možete instalirati obuhvaćeni paketi za sljedeće:

- [Brojka za Windows](https://git-for-windows.github.io/)
- [puTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [MobaXterm](http://mobaxterm.mobatek.net/)
- [Cygwin](https://cygwin.com/)

> [AZURE.NOTE] Najnovije Obljetnica ažuriranje za Windows 10 obuhvaća tulum za Windows. Ova značajka omogućuje vam pokretanje podsustav Windows za pristup i Linux uslužni programi kao što je klijent za SSH. Tulum Windows je i dalje u razvoju te se smatra beta-izdanje. Dodatne informacije o tulum za Windows potražite u članku [tulum na Ubuntu u sustavu Windows](https://msdn.microsoft.com/commandline/wsl/about).


## <a name="which-key-files-do-you-need-to-create"></a>Ključne datoteke potrebne za stvaranje?

Azure zahtijeva barem 2048-bitni, **ssh-rsa** oblikujte javni i privatni ključevi. Ako upravljate Azure resurse pomoću model implementacije klasični, morate da biste generirali u PEM (`.pem` datoteka).

Evo nekoliko scenariji za implementaciju i vrste datoteka koje koristite u svakoj:

1. **ssh-rsa** tipke su potrebni za sve implementacije pomoću [portala za Azure](https://portal.azure.com)i resursima implementacijama pomoću [Azure EŽA](../xplat-cli-install.md).
    - Ove tipke obično su sve Većina ljudi treba.
2. `.pem`Da biste stvorili VMs pomoću [portala za klasični](https://manage.windowsazure.com)potrebna je datoteka. Ove tipke podržani su i u implementacijama klasični koje koriste [Azure EŽA](../xplat-cli-install.md).
    - Samo potrebnih za stvaranje ove dodatne tipke i certifikati ako sami upravljate resursa stvorene pomoću modela implementacije klasični.


## <a name="install-git-for-windows"></a>Instalacija brojka za Windows

U prethodnom odjeljku naveden nekoliko paketa koji sadrže na `openssl` alat za Windows. Ovaj alat je potrebno da biste stvorili javni i privatni ključevi. Sljedeći primjeri detaljno upute za instalaciju i korištenje **Brojka za Windows**, iako možete odabrati bez obzira paketa radije. **Brojka za Windows** omogućuje vam pristup nekim dodatnim softverom Otvori izvor ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) alatima i uslužnih programa koji mogu biti korisni tijekom rada s Linux VMs.

1. Preuzmite i instalirajte **Brojka za Windows** od sljedećih mjesta: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).

2. Prihvatite zadane mogućnosti tijekom postupka instalacije osim ako izričito morate ih mijenjati.

3. Pokretanje **Tulumu brojka** s **izbornik Start** > **brojka** > **Brojka tulumu**. Na konzoli izgleda slično sljedećem primjeru:

    ![Brojka za Windows tulum shell](./media/virtual-machines-linux-ssh-from-windows/git-bash-window.png)


## <a name="create-a-private-key"></a>Stvaranje privatni ključ

1. U prozoru **Brojka tulumu** , koristite `openssl.exe` da biste stvorili privatni ključ. Sljedeći primjer stvara ključ naziva `myPrivateKey` i potvrdu pod nazivom `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    Rezultat izgleda slično sljedećem primjeru:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

2. Odgovaranje upite za naziv države, mjesto, naziv tvrtke ili ustanove, itd.

3. Novi privatni ključ i certifikat stvaraju se u trenutnom radnom direktoriju. Za najbolje prakse za sigurnost, postavite dozvole na privatni ključ bi samo je mogli pristupati:

    ```bash
    chmod 0600 myPrivateKey
    ```

4. Ako trebate Upravljanje resursima klasični, pretvorite u `myCert.pem` da biste `myCert.cer` (DER kodirana X509 certifikat). Izvođenje ovog neobavezan korak samo ako je potrebno posebno Upravljanje resursima starije klasični. 

    Pretvaranje certifikata pomoću sljedeće naredbe:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Stvaranje privatni ključ za PuTTY

PuTTY je uobičajenih SSH klijent za Windows. Vi ste inženjer može koristiti bilo koji klijent SSH koji želite. Da biste koristili PuTTY, morate stvoriti dodatnu vrstu ključa - u PuTTY privatni ključ (PPK). Ako ne želite koristiti PuTTY, preskočite ovaj odjeljak.

Sljedeći primjer stvara ovaj dodatne privatni ključ namijenjenu PuTTY koristiti:

1. Koristite **Tulumu brojka** privatni ključ pretvoriti u RSA privatni ključ koji mogu razumjeti PuTTYgen. Sljedeći primjer stvara ključ naziva `myPrivateKey_rsa` iz postojeće ključa pod nazivom `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Za najbolje prakse za sigurnost, postavite dozvole na privatni ključ bi samo je mogli pristupati:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```

2. Preuzmite i pokrenite PuTTYgen od sljedećih mjesta: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

3. Kliknite izbornik: **datoteka** > **opterećenja privatni ključ**

4. Pronađite privatni ključ (`myPrivateKey_rsa` u prethodnom primjeru). Zadani imenik prilikom pokretanja **Brojka tulumu** `C:\Users\%username%`. Promijenite filtar datoteku da biste prikazali **sve datoteke (\*.\*)**:

    ![Učitavanje postojeće privatni ključ u PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/load-private-key.png)

5. Kliknite **Otvori**. Upit označava da će se ključ je uspješno uvezeni:

    ![Uspješno uvezeni ključ PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/successfully-imported-key.png)

6. Kliknite **u redu** da biste zatvorili upit.

7. Javni ključ prikazan je pri vrhu prozora **PuTTYgen** . Kopirajte i ovaj javni ključ zalijepite portala za Azure ili Voditelj resursa Azure predloška prilikom stvaranja Linux VM. Možete kliknuti i **Spremanje javni ključ** možete spremiti kopiju s računalom:

    ![Spremanje PuTTY javni ključ datoteke](./media/virtual-machines-linux-ssh-from-windows/save-public-key.png)

    Sljedeći primjer pokazuje kako bi kopirajte i zalijepite ovu javni ključ u Azure portal prilikom stvaranja Linux VM. Javni ključ obično pohranjuje `~/.ssh/authorized_keys` na vaše nove VM.

    ![Koristite javni ključ kada stvarate na VM na portalu za Azure](./media/virtual-machines-linux-ssh-from-windows/use-public-key-azure-portal.png)

7. Vratite se u **PuTTYgen**, kliknite **Spremi privatni ključ**:

    ![Spremanje datoteke PuTTY privatni ključ](./media/virtual-machines-linux-ssh-from-windows/save-ppk-file.png)

    > [AZURE.WARNING] Odzivnik vas pita želite li nastaviti bez unosa pristupni ključ. Pristupni izraz je kao što su lozinke priložiti privatni ključ. Čak i ako netko da biste dobili privatni ključ, oni i dalje želite neće biti moguće provjeriti autentičnost korištenje samo ključa. Koje su im želite potrebne pristupni izraz. Bez pristupni izraz, ako netko dohvaća privatni ključ se možete prijaviti u VM ni servis koji koristi te tipke. Preporučujemo vam da stvorite pristupni izraz. No ako zaboravite pristupni izraz, ne postoji način da je vratite.

    Ako želite da biste unijeli pristupni izraz, kliknite **ne**, unesite pristupni izraz u glavnom prozoru PuTTYgen i zatim ponovno kliknite **Spremi privatni ključ** . U suprotnom, kliknite **da** da biste nastavili bez osiguravanja neobavezna pristupni izraz.

8. Unesite naziv i mjesto na koje želite spremiti datoteku PPK.


## <a name="use-putty-to-ssh-to-a-linux-machine"></a>Korištenje Putty za SSH Linux računalo

Ponovno PuTTY je uobičajenih SSH klijent za Windows. Vi ste slobodno bilo koji klijent SSH koji želite koristiti. Sljedeći koraci detaljno kako koristiti privatni ključ za provjeru s vašeg VM Azure pomoću SSH. Koraci su slične u drugim SSH ključa klijentima pomoću koje je potrebno da biste učitali privatni ključ za provjeru autentičnosti SSH veze.

1. Preuzimanje i pokretanje putty od sljedećih mjesta: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Unesite naziv glavnog računala ili IP adrese sustava VM s portala za Azure:

    ![Otvaranje nove PuTTY veze](./media/virtual-machines-linux-ssh-from-windows/putty-new-connection.png)

3. Prije nego što odaberete **Otvori**, kliknite **vezu** > **SSH** > kartica**provjere autentičnosti** . Pronađite i odaberite privatni ključ:

    ![Odaberite PuTTY privatni ključ za provjeru autentičnosti](./media/virtual-machines-linux-ssh-from-windows/putty-auth-dialog.png)

4. Kliknite **Otvori** da biste se povezali virtualnog računala
 

## <a name="next-steps"></a>Daljnji koraci
Možete stvoriti i javni i privatni ključevi [pomoću OS X i Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Dodatne informacije o tulum za Windows i pogodnosti koje se pojavljuju OSS Alati dostupni na računalu sa sustavom Windows potražite u članku [tulum na Ubuntu u sustavu Windows](https://msdn.microsoft.com/commandline/wsl/about).

Ako imate problema prilikom korištenja SSH povezati svoje VMs Linux, potražite u članku [Otklanjanje poteškoća s SSH veze do VM Azure Linux](virtual-machines-linux-troubleshoot-ssh-connection.md).