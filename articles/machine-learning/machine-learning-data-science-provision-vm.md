<properties 
    pageTitle="Dodjela resursa za Microsoft Data znanstvenog virtualnog računala | Microsoft Azure" 
    description="Konfiguriranje i stvorite virtualnog računala znanosti za podatke na Azure učiniti analize i učenje na računalu." 
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
    ms.date="09/07/2016" 
    ms.author="bradsev" />


# <a name="provision-the-microsoft-data-science-virtual-machine"></a>Dodjela resursa za Microsoft Data znanstvenog virtualnog računala

Microsoft podatke znanstvenog virtualnog računala je slici prikazan Azure virtualnog računala (VM) prije instalacije i konfiguracije s nekoliko popularnih alatima koji se često koriste za analize podataka i strojnog učenja. Alati za uključene su:

- Microsoft R poslužitelju za razvojne inženjere Edition
- Anaconda Python distribuciju.
- Jupyter bilježnicu (R, Python jezgre)
- Visual Studio zajednice Edition
- Power BI desktop
- SQL Server 2016 za razvojne inženjere Edition
- Alati za učenje računala
    - [Komplet alata za računalne mreže (CNTK)](https://github.com/Microsoft/CNTK): obuhvaća više razina učenje softver alata iz Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): na brz strojnog učenja sustava podrške tehnike kao što su mreži, raspršivanje, allreduce, smanjenja, learning2search, Aktivno, i interaktivnih Upoznavanje.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): alat omogućuje brz i točne boosted stabla implementacije.
    - [Rattle](http://rattle.togaware.com/) (u R analitičkih alata za dodatne jednostavno): alat koji omogućuje prvi koraci s analize podataka i strojnog učenja u R jednostavno s Istraživanje GUI na temelju podataka i Modeliranje s automatsko generiranje koda R.
    - [mxnet](https://github.com/dmlc/mxnet): precizno učenje framework namijenjen učinkovitosti i fleksibilnost
- Biblioteka u R i Python za korištenje u Azure strojnog učenja i drugim servisima za Azure
- Brojka uključujući brojka tulumu za rad s izvornog koda spremištima uključujući GitHub, Team Services za Visual Studio
- Windows priključaka od nekoliko popularnih Linux naredbenog retka uslužni programi (uključujući awk, sed, perl, grep, pretraživanje, wget, zakretanja itd.) dostupne putem naredbenog retka. 


Način znanstvenog podataka obuhvaća iterating na niz zadataka: pronalaženje, učitavanju i unaprijed obradu podataka, stvaranje testiranje modela i implementacija modela za potrošnju u Inteligentna aplikacijama. Fizičari podataka pomoću raznih alata da biste dovršili ove zadatke. To može biti vrlo dugo trajati, da biste pronašli odgovarajuće verzije softvera, a zatim ga preuzmite i instalirajte ih. Microsoft podatke znanstvenog virtualnog računala možete olakšalo njihovo ovaj teret unosom spremni za korištenje slika koje možete dodjeli na Azure alatima sve nekoliko popularnih prije instalacije i konfiguracije. 

Microsoft podatke znanstvenog virtualnog računala jump-starts analize projekta. Omogućuje rad na zadacima u raznim jezicima uključujući R, Python, SQL i C#. Visual Studio nudi programa IDE razviti i testiranje kod koji je jednostavno je za korištenje. Azure SDK obuhvaćeno na VM omogućuje sastavljanje na aplikacije koje koriste različite servise na platformi Microsoft cloud. 

Postoje bez softver naknade za sliku VM znanstvenog podataka. Samo platite naknade za korištenje Azure koje zavisne o veličini virtualnog računala koje resurse. Dodatne informacije o računalnim naknade pronaći ćete u odjeljku pojedinosti o određivanje cijena na stranici [podataka znanstvenog virtualnog računala](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) . 


## <a name="prerequisites"></a>Preduvjeti

Da biste mogli stvarati Microsoft podataka znanstvenog virtualnog računala, morate imati sljedeće:

- **Mogući Azure pretplatu**: da biste dobili, potražite u članku [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

*   **Račun za pohranu programa Azure**: da biste je stvorili, potražite u članku [Stvaranje računa Azure prostora za pohranu](storage-create-storage-account.md#create-a-storage-account). Osim toga, računa za pohranu mogu se kreirati tijekom procesa stvaranja na VM ako ne želite koristiti postojeći račun.


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Stvaranje sustava Microsoft Data znanstvenog virtualnog računala

Evo nekoliko koraka da biste stvorili instance programa sustava Microsoft podataka znanstvenog virtualnog računala:

1.  Dođite do virtualnog računala popis portala za [Azure](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).
2.   Odaberite gumb **Stvori** pri dnu da bi se otvorila u čarobnjak. ![konfiguriranje-podataka – znanstvenog-vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3.   Čarobnjak koji se koristi za stvaranje Microsoft podataka znanstvenog virtualnog računala potreban **unosa** za svaku od **pet koraka** označuju s desne strane slika. Evo unosa potrebne za konfiguriranje svaki od ovih koraka:
    
    1.   **Osnove**
        1.   **Naziv**: naziv poslužitelja znanstvenog podataka koje stvarate.
        2.   **Korisničko ime**: id za prijavu administratorskog računa.
        3.   **Lozinka**: administratorsku lozinku za račun.
        4.   **Pretplatu**: Ako imate više pretplata, odaberite onaj s kojim je računalo da biste stvorili i naplatiti.
        5.   **Grupa resursa**: Stvorite novu ili postojeću grupu.
        6.   **Lokacija**: Odaberite podatkovnom centru koji je najviše odgovara. Obično je podatkovnom centru koji ima većinu podataka ili je najsličniji fizičke lokaciji za najbrži pristup mreži.
         
    2.   **Veličina**: odaberite jednu od vrste poslužitelja koji ispunjava funkcionalni zahtjevi i ograničenja troškova. Veći izbor veličina VM možete dobiti odabirom "Prikaži sve".
    
    3.   **Postavke**:
        1.   **Na disku vrsta**: Odaberite Premium ako biste radije solid-state pogon (SSD), ostali odaberite "Standard".
        2.   **Račun za pohranu**: možete stvoriti novi račun Azure prostora za pohranu u pretplatu ili postojećeg na isto *mjesto* koje ste odabrali na **Osnove** koraku čarobnjaka.
        3.   **Drugi parametri**: obično samo koristite zadane vrijednosti. Informativna vezu pomoć na određena polja možete pokazivač u slučaju da želite uzeti u obzir korištenje koje nisu zadane vrijednosti.

    4.   **Sažetak**: Provjerite jesu li sve podatke koje ste unijeli točni.
    5.   **Kupnja**: kliknite **Kupi** da biste započeli s dodjelom resursa. Vezu možete pronaći na uvjete transakcije. Na VM ne sadrži sve dodatne troškove izvan računalnim veličina poslužitelja koje ste odabrali u koraku **Veličina** . 


>[AZURE.NOTE] Za dodjelu resursa, morate poduzeti oko 10 do 20 minuta. Status za dodjelu resursa prikazuje se na portal za Azure.

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Kako pristupiti Microsoft podataka znanstvenog virtualnog računala

Nakon stvaranja na VM udaljene radne površine možete u njega pomoću administratorskih vjerodajnica računa koje ste konfigurirali u prethodnom odjeljku **Osnove** . 

Kada vaš VM se stvara i dodjeli, spremni ste za početak rada s alatima koji su instalirani i konfigurirali je. Postoje pločica izbornika start i ikona na radnoj površini za mnoge alate. 

## <a name="how-to-create-a-strong-password-on-the-jupyter-notebook-server"></a>Upute za stvaranje neprobojne lozinke na poslužitelju Jupyter bilježnice 

Da biste stvorili vlastiti jaku lozinku za poslužitelj Jupyter bilježnica na računalu instaliran, pokrenite sljedeću naredbu iz naredbenog retka na podataka znanstvenog virtualnog računala.

    c:\anaconda\python.exe -c "import IPython;print IPython.lib.passwd()"

Odaberite jaku lozinku kada se to od vas zatraži.

Vidjet ćete raspršivanje lozinku u obliku "sha1:xxxxxx" u izlaz. Kopirajte ovu lozinku raspršivanje i zamijenite postojeće raspršivanje koji se nalazi u datoteci konfiguracije bilježnicu koja se nalazi na: **C:\ProgramData\jupyter\jupyter_notebook_config.py** s naziv parametra ***c.NotebookApp.password***.

Zamjena samo dijela (xxxxxx) postojeće raspršivanje vrijednosti koji se nalazi unutar navodnika. Ponude i ***sha1:*** prefiks za vrijednost parametra oba morate zadržati.

Naposljetku, morate Zaustavi i ponovno ga pokrenete poslužiteljem Jupyter koji se izvodi na VM kao windows zakazani zadatak pod nazivom **Start_IPython_Notebook**. Ako novu lozinku je korisnik prihvati nakon ponovnog pokretanja ovog zadatka, pokušajte killing svih izvodi python procesa s upravitelja zadataka, a zatim ponovno zakazani zadatak. Također, pokušajte ponovnog pokretanja virtualnog računala.

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Na Microsoft podataka znanstvenog virtualne računalu instalirani alati

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R poslužitelju za razvojne inženjere Edition
Ako želite koristiti R za vaše analize u VM ima Microsoft R poslužitelja za razvojne inženjere edition instaliran. Microsoft R Server je njih svim korisnicima Dopusti deployable enterprise predmete analize ovisno o R koji nije podržan, skalabilni i sigurne. Raznim Statistika velikih skupova podataka, predvidljivu Modeliranje i strojnog učenja mogućnosti za podršku, R poslužitelj podržava široka lepeza analytics – istraživanje, Analiza, vizualizacija i Modeliranje. Pomoću i proširivanje Otvori izvor R, Microsoft R Server nije kompatibilan s skripte R, funkcija i CRAN paketa, da biste analizirali podatke na razini tvrtke. Ograničenja u memoriji Otvori izvor R je adrese i dodavanjem paralelno i chunked obradu podataka. Omogućuje pokretanje analize podataka mnogo veći što stane u glavnom memorije.  Visual Studio zajednice Edition nalazi na na VM sadrži R alate za proširenje za Visual Studio koja omogućuje potpunu IDE za rad s R. Možete preuzeti i koristiti druge IDEs kao i kao što su [RStudio](http://www.rstudio.com). 

### <a name="python"></a>Python
Za razvoj pomoću Python Anaconda Python raspodjele 2.7 i 3.5 je instaliran. Ta se distribucija sadrži osnovni Python uz oko 300 najpopularnijih paketa analize matematičke, inženjerska i podataka. Možete koristiti Python alate za Visual Studio (PTVS) koji je instaliran unutar zajednice 2015 za Visual Studio edition ili jednu od IDEs vezanoj instalaciji s Anaconda kao što su stanje NEAKTIVNOSTI ili Spyder. Možete pokrenuti nešto od sljedećeg pretraživanjem traka za pretraživanje (**osvajaju** + **S** ključa).

>[AZURE.NOTE] Da biste pokažite Python alate za Visual Studio pri Anaconda Python 2.7 i 3.5, morate stvoriti prilagođene okruženja za svaku verziju. Da biste postavili te putova okruženja Visual Studio 2015 zajednice izdanju, idite na **Alati** -> **Python Alati** -> **Python okruženja** , a zatim **+ Prilagođeno**. 

Anaconda Python 2.7 je instaliran u odjeljku C:\Anaconda te Anaconda Python 3.5 se instalira u odjeljku c:\Anaconda\envs\py35. Detaljne upute potražite u članku [PTVS dokumentacije](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) . 

### <a name="jupyter-notebook"></a>Jupyter bilježnice
Anaconda raspodjele i u sklopu Jupyter bilježnice, okruženje za zajedničko korištenje s kodom i analize. Poslužitelj za bilježnicu Jupyter unaprijed konfiguriran s Python 2, Python 3 i R jezgre. Postoji ikonu na radnoj površini pod nazivom "Jupyter bilježnice da biste pokrenuli preglednika za pristup poslužitelju bilježnice. Ako ste na VM putem udaljene radne površine, možete i posjetiti [https://localhost:9999 /](https://localhost:9999/) za pristup bilježnici poslužitelja Jupyter kada prijavljeni na VM.
 
>[AZURE.NOTE] Ako se sva upozorenja certifikat i dalje. 

Ne možemo ste pakirat uzorka bilježnica u Python i R. Bilježnica Jupyter pokazati kako raditi s Microsoft R Server, SQL Server 2016 R Services (analize u bazi podataka), Python i druge tehnologije Azure kada se prijavite u Jupyter. Vidjet ćete vezu da biste primjere na početnoj stranici bilježnicu nakon provjere autentičnosti u bilježnici Jupyter pomoću lozinke koje ste stvorili u prethodnom koraku. 

### <a name="visual-studio-2015-community-edition"></a>Visual Studio 2015 zajednice edition
Visual Studio zajednice edition instalirano na VM. To je besplatna verzija popularne IDE od Microsofta koje možete koristiti za procjenu namjene i malim timovima. Pročitajte članak uvjete licenciranja [ovdje](https://www.visualstudio.com/support/legal/mt171547).  Da biste otvorili Visual Studio, dvokliknite ikonu na radnoj površini ili na izbornik **Start** . Možete i potražiti programi koji imaju **osvajaju** + **S** i unos "Visual Studio". Kada postoje možete stvoriti projekata na jezicima kao što je C#, Python, R, node.js. Dodaci i instaliraju koje ga čine prikladno za rad sa servisa Azure kao što je katalog podataka Azure, Azure HDInsight (Hadoop, Spark) i Lake Azure podataka. 

>[AZURE.NOTE] Može se poruka da je istekao Probni rok. Unesite vjerodajnice za Microsoftov račun ili stvorite novi račun besplatno da biste pristupili Visual Studio izdanje zajednice. 

### <a name="sql-server-2016-developer-edition"></a>Izdanje sustava SQL Server 2016 za razvojne inženjere
Za razvojne inženjere verziju sustava SQL Server 2016 sa servisima R da biste pokrenuli u bazi podataka analize navedeni su na na VM. R servisi omogućuju platforme za razvoj i implementacija Inteligentna aplikacije. R jezik bogata i naprednih i mnogo paketa iz zajednice možete koristiti da biste stvorili modela i generiranje predviđanja za podatke iz sustava SQL Server. Analitički blizu podatke možete zadržati jer R Services (u bazi podataka) integrirati R jezik sustava SQL Server. Time se uklanja troškova i sigurnosni rizik povezan s premještanje podataka.

>[AZURE.NOTE] Za razvojne inženjere izdanja sustava SQL Server 2016 se može koristiti samo za razvoj i testiranje svrhe. Morate li licencu za pokretanje u radni. 

SQL server možete pristupiti tako da pokrenete **SQL Server Management Studio**. Naziv VM unose se kao naziv poslužitelja. Koristi provjeru autentičnosti sustava Windows prilikom prijavljeni kao administrator u sustavu Windows. Kad se nalazite u SQL Server Management Studio možete stvaranje drugim korisnicima, stvorite bazu podataka, uvoz podataka i pokretanje upita za SQL. 

Da biste omogućili analitičke podatke u bazu podataka pomoću Microsoft R, pokrenite sljedeću naredbu kao neko vrijeme akcija u programu SQL Server management studio nakon prijava u ulozi administratora poslužitelja. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 
        
        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure 
Nekoliko Azure alata instaliranih na na VM:

- Postoji prečac na radnoj površini da biste pristupili dokumentaciju Azure SDK. 
- **AzCopy**: služi za premještanje podataka i računa sustava Microsoft Azure prostora za pohranu. Da biste vidjeli korištenje naredbeni redak da biste vidjeli korištenje upišite **Azcopy** . 
- **Microsoft Azure prostora za pohranu Explorer**: koristiti da biste pregledali objekte koji su pohranjeni u račun za Azure prostora za pohranu i prijenos podataka i iz Azure prostora za pohranu. Možete u pretraživanje upišite **Eksplorer za pohranu** ili pronaći na izborniku Start sustava Windows da biste pristupili ovaj alat. 
- **Adlcopy**: koristi da biste premjestili sadržaj Lake Azure podataka. Da biste vidjeli korištenje, upišite **adlcopy** u naredbeni redak. 
- **dtui**: služi za premještanje podataka i iz Azure DocumentDB, NoSQL baze podataka u oblaku. U naredbeni redak upišite **dtui** . 
- **Pristupnik za upravljanje podacima**: omogućuje premještanje podataka između lokalnog izvora podataka i oblaka. Koristi se unutar alata kao što je tvorničke Azure podataka. 
- **Microsoft Azure Powershell**: alat koji se koristi za administriranje Azure resursa u ljusci Powershell skriptnog jezika mora biti instalirana na vaš VM. 

###<a name="power-bi"></a>Power BI

Da biste lakše stvaranje nadzorne ploče i sjajan vizualizacije, **Power BI Desktop** je instaliran. Koristite ovaj alat za izvlačenje podataka iz različitih izvora, da biste izradili nadzornih ploča i izvješća te da biste objavili ih u oblak. Informacije potražite u članku [Power BI](http://powerbi.microsoft.com) web-mjesta. Power BI desktop možete pronaći na izborniku Start. 

>[AZURE.NOTE] Potreban vam je račun za Office 365 da biste pristupili Power BI. 

## <a name="additional-microsoft-development-tools"></a>Dodatne alate za razvoj Microsoft
Da biste otkrili i preuzmite druge alate za razvoj Microsoft može se koristiti [**Microsoft Web platformu Installer**](https://www.microsoft.com/web/downloads/platform.aspx) . Postoji prečac do alata za navedeni su na radnoj površini Microsoft podataka znanstvenog virtualnog računala.  

## <a name="important-directories-on-the-vm"></a>Važno direktorija na na VM

| Stavke                          | Direktorija |
| ------------------------------| ---------------- |
|Konfiguracija poslužitelja za Jupyter bilježnice | C:\ProgramData\jupyter |
|Bilježnice Jupyter uzoraka osnovne mape| c:\dsvm\notebooks|
|Ostale primjere | c:\dsvm\samples|
| Anaconda (zadani: Python 2.7) | c:\Anaconda |
| Anaconda Python 3.5 okruženje | c:\Anaconda\envs\py35|
|R samostalnog poslužitelja instancu direktorija (R zadane instance) | C:\Programske datoteke\Microsoft SQL Server\130\R_SERVER |
| R poslužitelja u bazi podataka instancu direktorija | C:\Programske datoteke\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Razni Alati | c:\dsvm\tools|

>[AZURE.NOTE] Instance programa sustava Microsoft podataka znanstvenog virtualnog računala stvoriti prije 1.5.0 (prije rujan 3, 2016) koristi strukturu malo drugačije direktorija onom u prethodnoj tablici. 

## <a name="next-steps"></a>Daljnji koraci
Evo nekih sljedeće korake da biste nastavili učenja i istraživanje. 

* Upoznavanje različite znanstvenog Alati za podatke na znanstvenog podataka VM klikom na izbornik start, a odjavljivanje alata prikazati na izborniku.
* Pomaknite se do **C:\Programske datoteke\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** za uzoraka korištenja RevoScaleR biblioteke u R koji podržava analize podataka na razini tvrtke.  
* Pročitajte članak: [10 stvari koje možete obaviti na znanstvenog podataka virtualnog računala](http://aka.ms/dsvmtenthings)
* Saznajte kako izraditi end da biste završetka analytical rješenja sustavno koristite [Timu podataka znanstvenog postupak](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Posjetiti [Galeriju obavještavanje Cortana](http://gallery.cortanaintelligence.com) za strojnog učenja i podataka analize uzorke koje koriste obavještavanje paket Cortana. Ne možemo, je navesti ikone na izborniku **Start** i na radnoj površini virtualnog računala u galeriju.

