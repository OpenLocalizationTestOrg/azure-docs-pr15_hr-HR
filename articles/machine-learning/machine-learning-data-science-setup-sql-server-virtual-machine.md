<properties
    pageTitle="Postavljanje sustava SQL Server virtualnog računala kao poslužitelj IPython bilježnice | Microsoft Azure"
    description="Postavite prema gore na podataka znanstvenog virtualnog računala s SQL Server i IPython poslužitelja."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev" 
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Postavljanje programa Azure SQL Server virtualnog računala kao bilježnicu IPython poslužitelj za napredne analize

U ovoj se temi objašnjava Dodjela resursa i konfiguriranje SQL Server virtualnog računala koja će se koristiti kao dio okruženju Znanstvena oblaku podataka. Windows virtualnog računala je konfiguriran za korištenje alata kao što su IPython bilježnice, Explorer Azure prostora za pohranu i AzCopy kao i drugi uslužni programi koji se koriste za projekte znanstvenog podataka za podršku. Azure Explorer prostora za pohranu i AzCopy, na primjer, omogućuju praktičan načina za prijenos podataka u spremište blobova platforme Azure s lokalnog računala ili da biste preuzeli na lokalno računalo iz spremišta blobova.

Galerija Azure virtualnog računala sadrži nekoliko slike koje sadrže Microsoft SQL Server. Odaberite SQL Server VM sliku koja je prikladna za vaše potrebe podataka. Preporučena slike su:

- SQL Server 2012 SP2 Enterprise za male da biste podatke srednje veličine
- SQL Server 2012 SP2 Enterprise optimizirana radnih opterećenja DataWarehousing za veliko da biste vrlo velike podataka

 > [AZURE.NOTE] SQL Server 2012 SP2 Enterprise slika **obuhvaćaju podatkovni disk**. Morat ćete dodati i/ili priložiti jedan ili više virtualne tvrdom disku radi pohrane podataka. Kada stvorite Azure virtualnog računala, ima na disku za operacijski sustav mapirani pogon C i privremene disk mapirati pogonu D. Nemojte koristiti pogonu D da biste pohranili podatke. Kao što je naziv podrazumijeva, nudi privremenu pohranu. Nudi zalihosti ni sigurnosne kopije jer ga se ne nalaze u Azure prostora za pohranu.


##<a name="Provision"></a>Povezivanje Classic portalu Azure i dodjela virtualnog računala sustava SQL Server

1.  Prijavite se na [Azure klasični Portal](http://manage.windowsazure.com/) pomoću računa.
    Ako nemate račun za Azure, posjetite [Azure slobodno probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

2.  Azure klasični portala u donjem lijevom web-stranice, kliknite **+ NOVO**, kliknite **IZRAČUNATI**, kliknite **VIRTUALNOG računala**, a zatim **Iz GALERIJE**.

3.  Na stranici **Stvaranje virtualnog računala** odaberite sliku virtualnog računala koji sadrži SQL Server na temelju potreba podataka, a zatim dalje strelicu u donjem desnom kutu stranice. Najnovije informacije na podržani slike sustava SQL Server Azure, potražite u članku [Uvod u SQL Server na virtualnim strojevima sa sustavom Azure](http://go.microsoft.com/fwlink/p/?LinkId=294720) teme u dokumentaciji za [SQL Server na virtualnim strojevima sa sustavom Azure](http://go.microsoft.com/fwlink/p/?LinkId=294719) postavljanje.

    ![Odaberite SQL Server VM][1]

4.  Na prvoj stranici **Konfiguracija virtualnog računala** , navedite sljedeće podatke:

    -   Navedite **naziv VIRTUALNOG računala**.
    -   U okviru **NOVO KORISNIČKO ime** upišite jedinstveni korisničko ime za VM lokalnog administratorskog računa.
    -   U okvir za **NOVU lozinku** upišite jaku lozinku. Dodatne informacije potražite u članku [Strong lozinke](http://msdn.microsoft.com/library/ms161962.aspx).
    -   U okvir **Potvrda LOZINKE** ponovno upišite lozinku.
    -   S padajućeg popisa odaberite odgovarajuću **veličinu** .

     > [AZURE.NOTE] Veličina virtualnog računala navedena je prilikom dodjele resursa: Najmanja veličina preporučuje radnih opterećenja radni se u ćeliji A2. Minimalna veličina preporučenih za virtualnog računala jest A3 prilikom korištenja SQL Server Enterprise Edition. Odaberite A3 ili noviji prilikom korištenja SQL Server Enterprise Edition. Odaberite A4 prilikom korištenja SQL Server 2012 ili optimizirane za 2014 Enterprise transakcijskih radnih opterećenja slika.
Odaberite A7 prilikom korištenja SQL Server 2012 ili optimizirane za Enterprise 2014 za podatke s radnih opterećenja skladištenje slike. Broj diskova podataka možete konfigurirati koliko je ograničenje veličine odabran. Najnovije informacije o veličinama dostupna virtualnog računala i broj diskova podataka koje možete pridružiti virtualnog računala, potražite u članku [Veličine virtualnog računala za Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Informacije o cijenama, potražite u članku [Virtualne Macines cijene](https://azure.microsoft.com/pricing/details/virtual-machines/).

    Kliknite Dalje strelicu dolje desno da biste nastavili.

    ![Konfiguriranje VM][2]

5.  Na drugoj stranici za **konfiguraciju virtualnog računala** konfigurirati resursa za povezivanje s mrežom, za pohranu i dostupnost:

    -   U okviru **Servis u Oblaku** odaberite **Stvori novi servis u oblaku**.
    -   U okviru **Naziv DNS servis oblaka** navedite prvi dio DNS naziv po izboru, tako da se dovrši ime u obliku **TESTNAME.cloudapp.net**
    -   U okviru **REGIJE/AFINITET GRUPE/VIRTUALNE MREŽE** , odaberite područje u kojem će se nalaziti na ovoj se slici virtualne.
    -   U **Račun za pohranu**, odaberite postojeći račun za pohranu ili odaberite jednu od automatski generirani.
    -   U okviru **Postavi DOSTUPNOST** odaberite **(ništa)**.
    -   Pročitajte i prihvatite podatke o cijenama.

6.  U odjeljku **krajnje točke** kliknite prazan padajućem popisu u odjeljku **naziv**i odaberite **MSSQL** , zatim unesite broj priključka instance modul baze podataka (**1433** za zadane instance).

7.  Sustava SQL Server VM također može poslužiti kao IPython poslužitelj bilježnica, koji će se konfigurirati u noviji koraku.
    Dodajte krajnju točku da biste odredili priključak koji želite koristiti za poslužitelj IPython bilježnice. Unesite naziv u stupcu **naziv** , odaberite broj priključka izboru za javne priključak i 9999 privatne priključka.

    Kliknite Dalje strelicu dolje desno da biste nastavili.

    ![Odaberite MSSQL i IPython priključci][3]

8.  Prihvatite zadana mogućnost **instalacije VM agent** potvrđenu i kliknite u kvačicu u donjem desnom kutu zaslona u čarobnjaku da biste dovršili VM dodjeljivanje postupak.

    `![Konačni mogućnosti VM][4]

9.  Čekanje dok Azure pripremi virtualnog računala. Očekivati status virtualnog računala da biste nastavili putem:

    -   Pokretanje (dodjele resursa)
    -   Zaustavi
    -   Pokretanje (dodjele resursa)
    -   Pokretanje (dodjele resursa)
    -   Pokretanje


##<a name="RemoteDesktop"></a>Otvorite virtualnog računala pomoću udaljene radne površine i dovršili postavljanje

1.  Prilikom dodjele resursa dovrši, kliknite naziv virtualnog računala da biste otvorili stranicu nadzorne PLOČE. Pri dnu stranice kliknite **Poveži**.

2.  Odaberite da biste otvorili datoteku rpd pomoću programa Windows udaljene radne površine (`%windir%\system32\mstsc.exe`).

3.  U dijaloškom okviru **Sigurnost sustava Windows** omogućavaju lozinku lokalnog administratorskog računa koji ste naveli u prethodnom koraku.
    (Vas možda zatražiti da biste provjerili vjerodajnice virtualnog računala.)

4.  Prvo se prijavite na ovom računalu virtualne nekoliko postupaka možda će biti potrebno da biste dovršili, uključujući postavke radne površine, ažuriranja sustava Windows i obavljanjem Početna konfiguracija zadatke sustava Windows (sysprep). Po završetku Windows sysprep zadatke konfiguracije dovršetka instalacije sustava SQL Server. Ove zadatke može uzrokovati Odgoda od nekoliko minuta dok se dovrše. `SELECT @@SERVERNAME`dok se ne dovrši instalacijski program za SQL Server i SQL Server Management Studio možda neće biti visable na početnoj stranici, može vratiti točan naziv.

Kada ste povezani s virtualnog računala s udaljenog računala sa sustavom Windows, virtualnog računala radi vrlo nalik bilo kojem računalu. Povezivanje s zadane instance sustava SQL Server s SQL Server Management Studio (koja se izvodi na virtualnog računala) na uobičajen način.


##<a name="InstallIPython"></a>Instalacija IPython bilježnice i druge alate za podršku

Da biste konfigurirali novi VM poslužitelja SQL služi kao poslužitelj IPython bilježnice i njihovu instalaciju dodatne alate za podršku takvih AzCopy, Explorer Azure prostora za pohranu, koristan Python znanstvenog podataka paketi i drugim korisnicima, posebne prilagodbe skripte navedeni su vam. Da biste instalirali:

- Desnom tipkom miša kliknite ikonu **Start sustava Windows** , a zatim kliknite **naredbeni redak (administratora)**
- Kopirajte sljedeće naredbe i zalijepite u naredbenom retku.

        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

- Kada se to od vas zatraži, unesite lozinku po izboru za poslužitelj IPython bilježnice.
- Skripta za prilagodbu automatizira nekoliko postupaka nakon instalacije, što obuhvaća:
    + Instalacija i postavljanje bilježnice IPython poslužitelja
    + Otvaranje TCP priključaka u vatrozidu za Windows za krajnje točke ste ranije stvorili:
    + Za SQL Server remote connectivity
    + Za udaljene veza s poslužiteljem IPython bilježnice
    + Pribavljanje oglednih IPython bilježnice i SQL skripte
    + Preuzimanje i instaliranje korisne paketa Python znanstvenog podataka
    + Preuzimanje i instaliranje Azure alate kao što su AzCopy i Explorer prostora za pohranu za Azure  
<br>
- Možete pristupiti i pokretanje IPython bilježnice s bilo kojeg lokalnom ili udaljenom preglednika pomoću URL-a obrasca `https://<virtual_machine_DNS_name>:<port>`, pri čemu priključak je priključak javne IPython koji ste odabrali prilikom dodjele resursa virtualnog računala.
- Poslužitelj za IPython bilježnica se izvodi kao pozadinu servisa i će se ponovno pokrenuti automatski prilikom ponovnog pokretanja virtualnog računala.

##<a name="Optional"></a>Prilaganje podatkovni disk prema potrebi

Ako sliku VM sadrži diskova podataka, odnosno diskova samo pogon C (OS disk) i pogonu D (Privremene disk), morate dodati jedan ili više diskova podataka radi pohrane podataka. Slika VM za SQL Server 2012 SP2 Enterprise optimizirana radnih opterećenja DataWarehousing dolazi unaprijed konfigurirana pomoću dodatnih diskova za SQL Server podataka i datoteka zapisnika.

 > [AZURE.NOTE] Nemojte koristiti pogonu D da biste pohranili podatke. Kao što je naziv podrazumijeva, nudi privremenu pohranu. Nudi zalihosti ni sigurnosne kopije jer ga se ne nalaze u Azure prostora za pohranu.

Da biste priložili diskova dodatne podatke, slijedite korake opisane u [načinu prilaganja podatkovni Disk na Windows virtualnog računala](virtual-machines-windows-classic-attach-disk.md), koji će vas voditi kroz:

1. Prilaganje prazan diskove virtualnog računala dodjeli u starijim koracima
2. Pokretanje nove diskove u virtualnog računala


##<a name="SSMS"></a>Povezivanje s SQL Server Management Studio i omogućivanje kombiniranim načina provjere autentičnosti

Modul za baze podataka SQL Server ne možete koristiti provjeru autentičnosti sustava Windows bez okruženja domene. Da biste povezali modul baze podataka s drugog računala, konfigurirati SQL Server koriste različite verzije načina provjere autentičnosti. Kombinirani načina provjere autentičnosti omogućuje provjeru autentičnosti sustava SQL Server i provjeru autentičnosti sustava Windows. Način provjere autentičnosti SQL je potrebna lozinka ingest podatke izravno iz baze podataka programa SQL Server VM [Azure strojnog učenja Studio](https://studio.azureml.net) modul uvoz podataka.

1.  Dok ste povezani s virtualnog računala pomoću udaljene radne površine, koristite okno Windows **pretraživanje** , a zatim upišite **SQL Server Management Studio** (vrijeme SMSS). Kliknite da biste pokrenuli SQL Server Management Studio (SSMS). Preporučujemo vam da biste dodali prečac SSMS na radnoj površini za buduću upotrebu.

    ![Pokretanje SSMS][5]

    Kada prvi put otvorite Management Studio ga morate stvoriti Management Studio okruženje za korisnike. To može potrajati nekoliko sekundi dok se.

2.  Prilikom otvaranja, Management Studio prikazuje dijaloški okvir **za povezivanje s poslužiteljem** . U okvir **naziv poslužitelja** unesite naziv virtualnog računala možete povezati modul baze podataka pomoću programa Explorer objekta.
    (Umjesto naziva virtualnog računala vam može poslužiti **(lokalno)** ili jedno razdoblje kao **naziv poslužitelja**. Odaberite **Provjeru autentičnosti sustava Windows**, a ostavite ** *vaše\_VM\_naziv*\\vaše\_lokalni\_administrator** u okvir **korisničko ime** . Kliknite **Poveži**.

    ![Povezivanje s poslužiteljem][6]

    <br>

     > [AZURE.TIP] Možete promijeniti način provjere autentičnosti sustava SQL Server pomoću Windows Promjena ključa registra ili pomoću SQL Server Management Studio. Da biste promijenili način provjere autentičnosti pomoću Promjena ključa registra, pokrenite **Novi upit** i izvršiti sljedeće skripte:

        USE master
        go

        EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
        go


    Da biste promijenili način provjere autentičnosti pomoću SQL Server management Studio učinite sljedeće:

3.  U **SQL Server Management Studio objekt Explorer**desnom tipkom miša kliknite naziv instance sustava SQL Server (naziv virtualnog računala), a zatim **Svojstva**.

    ![Svojstva poslužitelja][7]

4.  Na stranici **Sigurnost** , u odjeljku **Provjera autentičnosti poslužitelja**odaberite **SQL Server i načina provjere autentičnosti sustava Windows**, a zatim **u redu**.

    ![Odaberite način provjere autentičnosti][8]

5.  U dijaloškom okviru **SQL Server Management Studio** kliknite **u redu** da biste potvrdili obavezne da biste ponovno pokrenuli SQL Server.

6.  U programu **Explorer objekta**, desnom tipkom miša kliknite na poslužitelju, a zatim **ponovno pokrenite**. (Ako je instaliran Agent servisa SQL Server, on mora se ponovno pokrenuti.)

    ![Ponovno pokretanje računala][9]

7.  U dijaloškom okviru **SQL Server Management Studio** kliknite **da** da biste pristajete da želite ponovno pokrenuti SQL Server.

##<a name="Logins"></a>Stvaranje prijave za provjeru autentičnosti sustava SQL Server

Da biste povezali modul baze podataka s drugog računala, morate stvoriti barem jedan prijava za provjeru autentičnosti sustava SQL Server.  

Programski mogu stvoriti novi prijave u sustav SQL Server ili SQL Server Management Studio. Da biste stvorili novi korisnik sustava s provjerom autentičnosti SQL programski, pokrenite **Novi upit** i izvoditi sljedeću skriptu. Zamjena < novo korisničko ime\> i < novu lozinku\> uz odabir *korisničko ime* i *lozinku*. 


    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Pravilnik za lozinke po potrebi prilagodite (na primjer kod isključuje pravila isteka Provjera i lozinku). Dodatne informacije o prijave u sustav SQL Server potražite u članku [Stvaranje prijave](http://msdn.microsoft.com/library/aa337562.aspx).  

Da biste stvorili novu SQL Server prijave pomoću SQL Server Management Studio učinite sljedeće:

1.  U **SQL Server Management Studio objekt Explorer**proširite mapu u kojoj želite stvoriti novi prijava instanca poslužitelja.

2.  Desnom tipkom miša kliknite mapu **Sigurnost** , pokažite na **Novo**i odaberite **Prijava...**.

    ![Novi prijava][10]

3.  U dijaloškom okviru **prijavu – novi** na **glavnu** stranicu, unesite naziv novog korisnika u okvir **korisničko ime za prijavu** .

4.  Odaberite **provjeru autentičnosti sustava SQL Server**.

5.  U okvir **Lozinka** unesite lozinku za novog korisnika. Ponovno unesite lozinku u okvir **Potvrdite lozinku** .

6.  Da biste nametnuli mogućnosti pravila lozinke za složenosti i provođenja, odaberite **Nametni pravila za lozinke** (preporučeno). To je zadana mogućnost kada je odabrana provjera autentičnosti sustava SQL Server.

7.  Da biste nametnuli mogućnosti lozinke pravilnika za istek, odaberite **Nametni Istek lozinke** (preporučeno). Nametanje Pravilnik za lozinke mora biti odabran da biste omogućili ovaj potvrdni okvir. To je zadana mogućnost kada je odabrana provjera autentičnosti sustava SQL Server.

8.  Da biste nametnuli korisnika da biste stvorili novu lozinku nakon prve koristi se za prijavu, odaberite **korisnik mora promijeniti lozinku prilikom sljedeće prijave** (preporučeno ako je ovo prijava za nekoga da biste koristili. Ako je prijave za osobnu upotrebu, nemojte odabrati tu mogućnost.) Nametanje Istek lozinke mora biti odabran da biste omogućili ovaj potvrdni okvir. To je zadana mogućnost kada je odabrana provjera autentičnosti sustava SQL Server.

9.  Na popisu **zadanu bazu podataka** , odaberite zadanu bazu podataka za prijavu. **glavni** je zadana postavka za tu mogućnost. Ako još niste stvorili baze podataka za korisnika, ostavite postavljeno na **glavni**.

10. Na popisu **zadani jezik** ostavite **zadani** kao vrijednost.

    ![Svojstva za prijavu][11]

11. Ako je ovo prvi prijava stvarate, preporučujemo vam da biste odredili ovaj prijavi kao administrator sustava SQL Server. Ako je tako, na stranici **Uloge poslužitelja** provjerite **sustava**.

    > [AZURE.IMPORTANT] Članovi s ulogom fixed poslužitelja imaju potpunu kontrolu nad modul baze podataka. Sigurnosnih vam razloga, trebali biste pažljivo ograničiti članstvu ta uloga.

    ![sustava][12]

12. Kliknite u redu.

##<a name="DNS"></a>Određivanje DNS naziva virtualnog računala

Da biste povezali modul za baze podataka SQL Server s nekog drugog računala, znate naziv upravljanje pravima na informacije (DNS-Domain Name System) virtualnog računala.

(To je naziv s Internetom koristi za prepoznavanje virtualnog računala. Poslužite se s IP adresom, ali može se s IP adresom promijeniti prilikom Azure premješta resursi za zalihosti ili održavanje. Naziv DNS-a bit će stabilan jer može biti preusmjereni na novu IP adresu.)

1.  Na portalu klasični Azure (ili u prethodnom koraku), odaberite **VIRTUALNIH računala**.

2.  Na stranici **INSTANCE VIRTUALNOG računala** , u stupcu **Naziv DNS** pronađite i kopirajte DNS naziv virtualnog računala koja se pojavljuje preceded **http://**. (Korisničkog sučelja mogu prikazati cijeli naziv, ali na njega, kliknite desnom tipkom miša i odaberite Kopiraj.)

##<a name="cde"></a>Povezivanje s modul baze podataka s drugog računala

1.  Na računalo povezano s Internetom, otvorite SQL Server Management Studio.

2.  U dijaloškom okviru **za povezivanje s poslužitelja** ili **Povezivanje s modul baze podataka** u okviru **naziv poslužitelja** unesite naziv DNS virtualnog računala (određen u prethodni zadatak) i s javnim krajnjoj točki broj priključka u obliku *DNSName, portnumber* kao što su **tutorialtestVM.cloudapp.net,57500**.

3.  U okviru **Provjera autentičnosti** odaberite **Provjera autentičnosti SQL poslužitelja**.

4.  U okvir za **prijavu** upišite naziv prijavu koju ste stvorili u ranijim zadatka.

5.  U okvir **Lozinka** upišite lozinku za prijavu koje ste stvorili u ranijim zadatka.

6.  Kliknite **Poveži**.

##<a name="amlconnect"></a>Povezivanje s modul baze podataka iz Azure strojnog učenja

U noviji faza procesa znanstvenog timu podataka omogućuje stvaranje i implementaciju strojnog učenja modelima koristit će [Azure strojnog učenja Studio](https://studio.azureml.net) . Da biste podatke iz baza podataka sustava SQL Server VM ingest izravno u Azure strojnog učenja za obuku ili bilježenje rezultata, pomoću modula za **Uvoz podataka** u novi eksperiment [Azure strojnog učenja Studio](https://studio.azureml.net) . U ovoj se temi opisana su u odjeljku Dodatne detalje putem veze vodič timu podataka znanstvenog postupak. Uvod, potražite u članku [što je Azure strojnog učenja Studio?](machine-learning-what-is-ml-studio.md).

2.  U oknu **Svojstva** [modul za uvoz podataka](https://msdn.microsoft.com/library/azure/dn905997.aspx)odaberite **Baze podataka SQL Azure** s padajućeg popisa **Izvor podataka** .

3.  U tekstni okvir **naziv poslužitelja baze podataka** unesite`tcp:<DNS name of your virtual machine>,1433`

4.  Unesite korisničko ime SQL u tekstni okvir **naziv poslužitelja korisničkog računa** .

5.  Unesite lozinku za sql korisnika u tekstni okvir **Lozinka korisničkog računa poslužitelja** .

    ![Azure ML uvoz podataka][13]

##<a name="shutdown"></a>Isključivanje i deallocate virtualnog računala kada nije u upotrebi

Azure virtualnim strojevima su cjenovnom kao **platiti samo koje koristite**. Da biste bili sigurni da vam se ne koji se naplaćuju kada ne koristite virtualnog računala, on mora biti u stanju **zaustavljen (Deallocated)** .

> [AZURE.NOTE] Isključuje virtualnog računala od unutar (pomoću mogućnosti power Windows), je zaustavljena na VM, ali ostaje dodijeliti. Da bi vam se ne koji se naplaćuju, uvijek zaustavite virtualnim strojevima s [Azure klasični Portal](http://manage.windowsazure.com/). VM putem komponente Powershell možete isključiti i tako da nazovete ShutdownRoleOperation s "PostShutdownAction" jednako "StoppedDeallocated".

Da biste isključili i deallocate virtualnog računala:

1. Prijavite se na [Azure klasični Portal](http://manage.windowsazure.com/) pomoću računa.  

2. Na lijevoj navigacijskoj traci odaberite **VIRTUALNIM RAČUNALIMA** .

3. Na popisu virtualnim strojevima kliknite naziv virtualnog računala, a zatim idite na stranicu **nadzorne PLOČE** .

4. Pri dnu stranice kliknite **isključivanje**.

![Isključivanje VM][15]

Virtualnog računala se deallocated, ali ne briše. U bilo kojem trenutku na portalu klasični Azure možda početi virtualnog računala.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Vaš VM Azure SQL Server spremna je za korištenje: što slijedi?

Sada je spremna za korištenje u vašem vježbe znanstvenog podataka virtualnog računala. Virtualnog računala i spremna je za korištenje kao poslužitelj IPython bilježnicu za istraživanje i obrada podataka i druge zadatke u kombinaciji s Azure strojnog učenja i timu podataka znanstvenog procesa (TDSP).

Sljedeći koraka u postupku znanstvenog podataka mapiraju se u [Timu podataka znanstvenog proces](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) , a može sadržavati korake koji premještanje podataka u HDInsight, obradu i je li uzorak u Priprema za učenje iz podataka s Azure strojnog učenja.


[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png
 
