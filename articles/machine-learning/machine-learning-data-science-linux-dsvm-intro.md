<properties
    pageTitle="Dodjela Linux podataka znanstvenog virtualnog računala | Microsoft Azure"
    description="Konfiguriranje i stvorite Linux podataka znanstvenog virtualnog računala na Azure učiniti analize i učenje na računalu."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />

# <a name="provision-the-linux-data-science-virtual-machine"></a>Dodjela Linux podataka znanstvenog virtualnog računala

Linux podataka znanstvenog virtualnog računala je Azure virtualnog računala koja se dobiva sa skup unaprijed instalirani alati. Ti Alati obično koristi za radi analize podataka i učenje na računalu. Ključne softverske komponente uključene su:

- Microsoft R poslužitelju za razvojne inženjere Edition
- Anaconda Python raspodjele (verzije 2.7 i 3,5), uključujući biblioteke analizu popularne podataka
- JupyterHub - višekorisnička poslužitelju bilježnice Jupyter R, Python, Julia jezgre za podršku
- Explorer Azure prostora za pohranu
- Azure sučelje naredbenog retka (EŽA) za upravljanje Azure resursi
- PostgresSQL baze podataka
- Alati za učenje računala
    - [Komplet alata za računalne mreže (CNTK)](https://github.com/Microsoft/CNTK): obuhvaća više razina učenje softver alata iz Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): na brz strojnog učenja sustava podrške tehnike kao što su mreži, raspršivanje, allreduce, smanjenja, learning2search, Aktivno, i interaktivnih Upoznavanje.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): alat omogućuje brz i točne boosted stabla implementacije.
    - [Rattle](http://rattle.togaware.com/) (u R analitičkih alata za dodatne jednostavno): alat koji omogućuje prvi koraci s analize podataka i strojnog učenja u R jednostavno s Istraživanje GUI na temelju podataka i Modeliranje s automatsko generiranje koda R.
- Azure SDK u Java, Python, node.js, Ruby, i
- Biblioteka u R i Python za korištenje u Azure strojnog učenja i drugim servisima za Azure
- Alati za razvoj i uređivači (Eclipse, Emacs, gedit, smjer)

Način podataka znanstvenog uključuje iterating na niz zadataka:

1. Pronalaženje, učitavanje i unaprijed obrada podataka
2. Izrada i testiranje modela
3. Implementacija modela za potrošnje u Inteligentna aplikacije

Fizičari podataka pomoću alata za različite da biste dovršili ove zadatke. Možete biti vrlo dugo trajati, da biste pronašli odgovarajuće verzije softvera i da biste preuzeli, Kompiliranje i instalirati ove verzije.

Linux podataka znanstvenog virtualnog računala možete olakšalo njihovo ovaj teret značajno. Koristi se za jump-start analize projekta. Omogućuje rad na zadacima na različitim jezicima, uključujući R, Python, SQL, Java i C++. Eclipse nudi programa IDE razviti i testiranje kod koji je jednostavno je za korištenje. Azure SDK obuhvaćeno na VM omogućuje sastavljanje aplikacije pomoću različite servise za na Linux za platformu Microsoft cloud. Uz to, imate pristup za druge jezike poput fonetski zapis, Perl, PHP i node.js i unaprijed instaliranih.

Postoje bez softver naknade za sliku VM znanstvenog podataka. Plaćanje samo na Azure hardver korištenje naknada koje su kažnjeni ovisno o veličini virtualnog računala koje dodijelite sa slikom VM. Dodatne informacije o računalnim naknade možete pronaći na [VM popis stranica na Azure Marketplace ](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).


## <a name="prerequisites"></a>Preduvjeti

Da biste mogli stvarati Linux podataka znanstvenog virtualnog računala, morate imati sljedeće:

- **Mogući Azure pretplatu**: da biste dobili, potražite u članku [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/free/).
- **Račun za pohranu programa Azure**: da biste je stvorili, potražite u članku [Stvaranje računa Azure prostora za pohranu](storage-create-storage-account.md#create-a-storage-account). Osim toga, računa za pohranu moguće stvoriti tijekom procesa stvaranja VM, ako ne želite koristiti postojeći račun.


## <a name="create-your-linux-data-science-virtual-machine"></a>Stvaranje vaše podatke Linux znanstvenog virtualnog računala

Evo nekoliko koraka da biste stvorili instance programa na Linux podataka znanstvenog virtualnog računala:

1.  Dođite do virtualnog računala unosa za [Azure portal](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm).
2.   Kliknite **Stvori** (pri dnu) da biste otvorili čarobnjak. ![konfiguriranje-podataka – znanstvenog-vm](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3.   Sljedeći odjeljci sadrže unosa za svaku korake u čarobnjaku (označuju s desne strane na prethodnoj slici) koristi za stvaranje Microsoft podataka znanstvenog virtualnog računala. Evo unosa potrebne za konfiguriranje svaki od ovih koraka:

    na. **Osnove**:

  - **Naziv**: naziv poslužitelja znanstvenog podataka koje stvarate.
  - **Korisničko ime**: prvi račun prijavu ID-a.
  - **Lozinka**: prvi lozinku računa (umjesto možete koristiti SSH javni ključ lozinku).
  - **Pretplatu**: Ako imate više pretplata, odaberite onaj s kojim je računalo da biste stvorili i naplatiti. Morate imati ovlasti za stvaranje resursa za ovu pretplatu.
  - **Grupa resursa**: Stvorite novu ili postojeću grupu.
  - **Lokacija**: Odaberite podatkovnom centru koji je najviše odgovara. Obično je podatkovnom centru koji ima većinu podataka ili je najsličniji fizičke lokaciji za najbrži pristup mreži.

    b. **Veličina**:

  - Odaberite jednu od vrste poslužitelja koji ispunjava funkcionalni zahtjevi i ograničenja troškova. Odaberite **Prikaži sve** da biste vidjeli dodatne mogućnosti VM veličina.

    c. **Postavke**:

  - **Na disku vrsta**: odaberite **Premium** ako biste radije pune stanje pogon (SSD). U suprotnom, odaberite **standardnu**.
  - **Račun za pohranu**: Stvaranje novog računa Azure prostora za pohranu u pretplatu ili korištenje postojećeg na istom mjestu koje ste odabrali na **Osnove** koraku čarobnjaka.
  - **Drugi parametri**: U većini slučajeva, samo pomoću zadane vrijednosti. Treba uzeti u obzir koje nisu zadane vrijednosti, postavite pokazivač miša Informativna vezu pomoć na određena polja.

    d. **Sažetak**:

  - Provjerite je li sve podatke koje ste unijeli točan.

    e. **Kupnja**:

  - Da biste započeli s dodjelom resursa, kliknite **kupite**. Vezu možete pronaći na uvjete transakcije. Na VM ne sadrži sve dodatne troškove izvan računalnim veličina poslužitelja koje ste odabrali u koraku **Veličina** .

Za dodjelu resursa, morate poduzeti oko 10 do 20 minuta. Na portalu za Azure prikazuje se status na dodjele resursa.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Kako pristupiti Linux podataka znanstvenog virtualnog računala

Nakon stvaranja na VM se možete prijaviti na njega pomoću SSH. Pomoću vjerodajnica računa koji ste stvorili u odjeljku **Osnove** korak 3 ljuske sučelja tekst. U sustavu Windows, možete preuzeti alat za klijent SSH kao što je [Putty](http://www.putty.org). Ako biste radije grafički radne površine (X sustava Windows), možete koristiti X11 prosljeđivanja na Putty ili instalacija klijentskih programa X2Go.

>[AZURE.NOTE] Klijent X2Go izvršiti znatno bolje nego X11 prosljeđivanja u testiranja. Preporučujemo korištenje X2Go klijent za grafičkog sučelja za stolna računala.


## <a name="installing-and-configuring-x2go-client"></a>Instaliranje i konfiguriranje X2Go klijenta

Linux VM je već dodijeljenu s poslužiteljem X2Go i spreman za prihvaćanje veza klijenta. Da biste povezali Linux VM grafički radnu površinu, učinite sljedeće na klijentu:

1. Preuzmite i instalirajte X2Go klijent za svoju platformu klijenta s [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Pokrenite klijent X2Go pa odaberite **Novu sesiju**. Otvara konfiguracije prozor s više kartica. Unesite sljedećih parametara konfiguracije:
    * **Kartica sesije**:
        - **Glavno računalo**: naziv glavnog računala ili IP adrese sustava VM znanstvenog Linux podataka.
        - **Prijava**: korisničko ime na Linux VM.
        - **SSH priključka**: ostaviti na 22, zadana vrijednost.
        - **Vrsta sesiju**: promijenite vrijednost XFCE. Trenutno Linux VM podržava samo XFCE radne površine.
    * **Kartica medijskih sadržaja**: možete isključiti zvuk podršku i klijent ispis ako ne morate ih koristiti.
    * **Mapa zajednički se koristi**: Ako želite direktorija iz na klijentskom računalu postavljena na Linux VM, dodajte direktorija klijentskog računala koju želite zajednički koristiti s VM na toj kartici.

Nakon prijave da biste na VM pomoću SSH klijenta ili XFCE grafički radne površine putem klijentskog programa X2Go, spremni ste za početak rada s alatima koji su instalirani i konfigurirali na VM. Na XFCE, vidjet ćete prečaci izbornika aplikacije i ikona na radnoj površini za mnoge alate.


## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Na Linux podataka znanstvenog virtualne računalu instalirani alati

### <a name="microsoft-r-open"></a>Otvori u programu Microsoft R
R je jedna od najpopularnijih jezike za analizu podataka i strojnog učenja. Ako želite koristiti R za vaše analize u VM ima Microsoft R Otvori (MRO) s na matematičke otklanjanje biblioteke (MKL). Na MKL optimizira matematičkih operacija zajednička analytical algoritama. MRO je 100 posto kompatibilan s CRAN R, a neki od biblioteke R objavljenim u članku CRAN moguće je instalirati na na MRO. Možete urediti programe R u jednom od uređivači zadane, kao što su smjer, Emacs ili gedit. Možete preuzeti i koristiti druge IDEs, kao što su [RStudio](http://www.rstudio.com). Praktičan, jednostavnu skriptu (installRStudio.sh) navedeni su u direktoriju **/dsvm/tools** koja se instalira RStudio. Ako koristite Emacs uređivača bilješke na Emacs pakiranje je DRESA (Emacs čitanja statistike), koji olakšava rad s datotekama R u uređivaču Emacs je instalirana.

Da biste pokrenuli R, samo upišite **R** u ljusci. Tako ćete doći do interaktivno okruženje. Za razvoj programa R, obično pomoću uređivača kao što su Emacs ili smjer ili gedit, a zatim pokrenite skripte unutar R. Ako instalirate RStudio, imate potpuni grafički IDE okruženje za razvoj programa R.

Postoji skripte R za instalaciju [paketa vrha 20 R](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) po želji. Ova skripta se može pokrenuti kada radite u sučelje interaktivne R, koje se može unijeti (kao što je rečeno) tako da upišete **R** u ljusci.  

### <a name="python"></a>Python
Za razvoj pomoću Python Anaconda Python raspodjele 2.7 i 3.5 je instaliran. Ta se distribucija sadrži osnovni Python uz oko 300 najpopularnijih paketa analize matematičke, inženjerska i podataka. Možete koristiti uređivači teksta zadane. Osim toga, možete koristiti Spyder, Python IDE koji je vezanoj instalaciji s Anaconda Python distribucije. Spyder mora grafički radne površine ili X11 prosljeđivanje. Prečac do Spyder navedeni su u grafički za stolna računala.

Budući da imamo Python 2.7 i 3.5, morate posebno aktivirati željenoj verziji Python želite raditi u trenutnoj sesiji. Aktivacija postavlja varijabla put željenoj verziji Python.

Da biste aktivirali Python 2.7, iz ljuske pokrenite na sljedeći način:

    source /anaconda/bin/activate root

Python 2.7 je instalirana na */anaconda/bin*.

Da biste aktivirali Python 3.5, iz ljuske pokrenite na sljedeći način:

    source /anaconda/bin/activate py35


Python 3.5 je instalirana na */anaconda/envs/py35/bin*.

Da biste aktiviranja Python interaktivne sesije, samo upišite **python** u ljusci. Ako su na grafičkog sučelja ili imaju X11 prosljeđivanje skup prema gore, možete upisati **spyder** da biste pokrenuli Python IDE.

### <a name="jupyter-notebook"></a>Jupyter bilježnice

Distribuciji Anaconda i u sklopu Jupyter bilježnice, okruženje za zajedničko korištenje s kodom i analize. Bilježnice Jupyter se pristupa putem JupyterHub. Prijavite koristeći lokalne Linux korisničko ime i lozinku.

Poslužitelj bilježnice Jupyter unaprijed konfiguriran s Python 2, Python 3 i R jezgre. Postoji ikonu na radnoj površini pod nazivom "Jupyter bilježnice" da biste pokrenuli preglednika za pristup poslužitelju bilježnice. Ako ste na VM putem SSH ili X2Go klijenta, možete i posjetiti [https://localhost:8000 /](https://localhost:8000/) za pristup poslužitelju Jupyter bilježnice.

>[AZURE.NOTE] Ako se sva upozorenja certifikat i dalje.

Poslužitelj Jupyter bilježnice možete pristupiti iz glavnog računala. Samo upišite *https://\<VM DNS naziv ili IP adresa\>: 8000 /*

>[AZURE.NOTE] Priključak 8000 otvara se u vatrozidu po zadanom kada je na VM dodjeli.

Ne možemo ste pakirat uzorka bilježnica – jedan u Python, a druga u R. Vidjet ćete vezu da biste primjere na početnoj stranici bilježnicu nakon autentičnost bilježnicu Jupyter koristeći lokalne Linux korisničko ime i lozinku. Možete stvoriti novu bilježnicu tako da odaberete **Novo**, a zatim otklanjanje odgovarajući jezik. Ako ne vidite gumb **Novo** , kliknite ikonu **Jupyter** na gornjem lijevom da biste prešli na početnu stranicu bilježnice poslužitelja.


### <a name="ides-and-editors"></a>IDEs i uređivača

Koje su vam sljedeće mogućnosti uređivača nekoliko kod. To obuhvaća smjer/VIM, Emacs, gEdit i Eclipse. gEdit i Eclipse grafički uređivači i morate morate biti prijavljeni u grafički radna površina da biste ih koristiti. Te uređivači ste radne površine i aplikacija za izbornik prečaca za pokretanje ih.

**VIM** i **Emacs** su temeljenom na tekstu uređivači. Na Emacs, ne možemo ste instalirali paket dodatak pod nazivom Emacs čitanja Statistika (DRESA) koji olakšava rad s R u uređivaču Emacs. Dodatne informacije pronaći ćete na [DRESA](http://ess.r-project.org/).

**Eclipse** je Otvori izvor, extensible IDE koji podržava više jezika. Razvojni inženjeri edition Java je instanca instalirano na VM. Nema dostupnih za nekoliko popularne jezike koje je moguće instalirati da biste proširili okruženje Eclipse dodataka. Imamo i dodatak instalirali u drugom Eclipse naziva **Azure komplet alata za Eclipse**. Omogućuje stvaranje, razvoj, testiranje i implementacija Azure aplikacije koje se koriste Eclipse platforme koje podržava jezike poput Java. Postoji programa **Azure SDK Java** koja omogućuje pristup različite Azure servisi Java okruženja. Dodatne informacije o Azure komplet alata za Eclipse pronaći ćete na [Azure komplet alata za Eclipse](../azure-toolkit-for-eclipse.md).

**LaTex** je instalirana pomoću značajke pakiranja texlive uz Emacs dodatak [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) paketa, koji olakšava stvaranje dokumenata LaTex unutar Emacs.  

### <a name="databases"></a>Baza podataka

#### <a name="postgres"></a>Postgres
Otvori izvor baze podataka **Postgres** dostupna je na VM, servise i initdb koji su već obavljeni. I dalje potrebnih za stvaranje baze podataka i korisnicima. Dodatne informacije potražite u [dokumentaciji Postgres](https://www.postgresql.org/docs/).  

####  <a name="graphical-sql-client"></a>Grafički klijent za SQL
**SQuirrel SQL**, grafički klijent za SQL je dodijeljen povezati s različitim baze podataka (kao što je Microsoft SQL Server, Postgres i MySQL), a da biste pokrenuli SQL upita. To možete pokrenuti iz grafički sesiji radne površine (na primjer pomoću klijentskog programa X2Go). Za pozivanje SQuirrel SQL možete pokrenuti je na ikonu na radnoj površini ili pokrenite sljedeću naredbu na ljuske.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Prije prvog korištenja postavite upravljačke programe i pseudonima baze podataka. Upravljačke programe za JDBC nalaze se na:

*/usr/Share/Java/jdbcdrivers*

Dodatne informacije potražite u članku [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Alati naredbenog retka za pristup Microsoft SQL Server

Paket ODBC upravljački program za SQL Server i dolazi s dva alati naredbenog retka:

**BCP**: skupno utility bcp kopira podataka između instance komponente Microsoft SQL Server i podatkovne datoteke u obliku koji je naveden korisnik. Uslužni bcp poslužite da biste uvezli velik broj novih redaka u tablice sustava SQL Server ili izvoz podataka iz tablica u podatkovne datoteke. Da biste uvezli podatke u tablicu, morate koristiti oblik datoteke stvorene za tu tablicu, ili razumijevanje strukturu tablice i vrste podataka valjanih za njegovih stupaca.

Dodatne informacije potražite u članku [Povezivanje s bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**sqlcmd**: Unesite Transact-SQL naredbe s sqlcmd utility, kao i postupke za sustav, a skripte datoteke u naredbenom retku. Uslužni koristi ODBC izvršiti serije Transact-SQL.

Dodatne informacije potražite u članku [Povezivanje s sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

>[AZURE.NOTE] Postoje neke razlike u uslužni Linux i Windows platformama. Potražite u dokumentaciji detalje.


#### <a name="database-access-libraries"></a>Biblioteka baze podataka programa access

Nema biblioteke dostupne u R i Python baze podataka programa access.

- U R, **RODBC** paketa ili paketa **dplyr** omogućuje upita ili izvoditi SQL naredbe na poslužitelju baze podataka.
- U Python, biblioteka **pyodbc** omogućuje pristup bazi podataka ODBC kao podlozi sloja.  

Da biste pristupili **Postgres**:

- Iz R: Pomoću značajke pakiranja **RPostgreSQL**.
- S Python: Pomoću **psycopg2** biblioteke.


### <a name="azure-tools"></a>Alati za Azure
Sljedeće alate za Azure instaliranih na na VM:

- **Azure sučelje naredbenog retka**: U Azure EŽA omogućuje stvaranje i upravljanje Azure resursima pomoću naredbe ljuske. Pozvati alata za Azure, samo upišite **azure pomoć**. Dodatne informacije potražite u članku [Azure EŽA dokumentaciju stranice](../virtual-machines-command-line-tools.md).
- **Microsoft Azure prostora za pohranu Explorer**: Microsoft Azure prostora za pohranu Explorer je grafički alat koji se koristi da biste pregledali objekte koje ste pohranili na vašem računu Azure prostora za pohranu i za prijenos i preuzimanje podataka i iz Azure blob-ova. Prostor za pohranu Explorer možete pristupiti iz ikonu prečac na radnoj površini. Možete je pozvati iz ljuske upit tako da upišete **StorageExplorer**. Morate biti prijavljeni putem klijentskog programa X2Go ili ste instalirali X11 prosljeđivanje skup prema gore.
- **Biblioteka Azure**: Slijede neki od unaprijed instaliranih biblioteke.

 - **Python**: vezane uz u Azure biblioteke u Python koji su instalirani su **azure**, **azureml**, **pydocumentdb**i **pyodbc**. Prva tri biblioteka omogućuje pristup servisa Azure prostora za pohranu, Azure strojnog učenja i Azure DocumentDB (NoSQL baze podataka na Azure). Četvrti biblioteke pyodbc (uz Microsoft ODBC upravljački program za SQL Server), omogućuje pristup sustava SQL Server, baze podataka SQL Azure i Azure SQL Data Warehouse iz Python pomoću ODBC sučelja. Unesite **točaka popis** da biste vidjeli sve navedene biblioteke. Ne zaboravite pokrenite sljedeću naredbu u Python 2.7 i 3,5 okruženja.
 - **R**: vezane uz u Azure biblioteke u R koji su instalirani su **AzureML** i **RODBC**.
 - **Java**: na popisu biblioteka Azure Java pronaći ćete u imeniku **/dsvm/sdk/AzureSDKJava** na na VM. Ključni biblioteke su Azure prostora za pohranu i upravljanje API-ji, DocumentDB i JDBC upravljačke programe za SQL Server.  

[Portal za Azure](https://portal.azure.com) možete pristupiti iz unaprijed instaliranih preglednika Firefox. Na portalu Azure možete stvoriti, upravljanje i nadzirati Azure resursi.

### <a name="azure-machine-learning"></a>Azure strojnog učenja

Azure strojnog učenja je potpuno upravljanih oblaka servis koji omogućuje sastavljanje, uvođenje i zajedničko korištenje predvidljivu analize rješenja. Sastavljanje eksperimenata i modela iz Azure strojnog učenja Studio. Je moguće pristupiti web-pregledniku na virtualnog računala znanstvenog podataka tako što ste posjetili [Microsoft Azure strojnog učenja](https://studio.azureml.net).

Kada se prijavite Azure strojnog učenja Studio, imate pristup području crtanja na početak gdje možete izraditi logičke toka za strojnog učenja algoritama. Također imaju pristup bilježnice Jupyter hostirane na Azure strojnog učenja i jednostavan rad s eksperimenata u Studio strojnog učenja. Operationalize strojnog učenja modela koji su ugrađeni tako da ih prelamanje u sučelje web-servisa. Omogućuje klijente na bilo koji jezik za pozivanje predviđanja iz strojnog učenja modela. Dodatne informacije potražite u [dokumentaciji strojnog učenja](https://azure.microsoft.com/documentation/services/machine-learning/).

Možete i stvoriti vaše modelima u R ili Python na VM i njegove implementacije u proizvodnje na Azure strojnog učenja. Ne možemo instalirali biblioteke u R (**AzureML**) i Python (**azureml**) da biste omogućili tu funkciju.

Informacije o tome kako uvesti modelima u R i Python u Azure strojnog učenja potražite u članku [deset stvari koje možete obaviti na znanstvenog podataka virtualnog računala](machine-learning-data-science-vm-do-ten-things.md) (u odjeljku posebice "Stvaranje modela pomoću R ili Python i Operationalize ih pomoću Azure strojnog učenja").

>[AZURE.NOTE] Ove upute su pisane za verziju sustava Windows znanstvenog VM podataka. No informacije navedene na implementacije modela za Azure strojnog učenja je dodijeljen Linux VM.

### <a name="machine-learning-tools"></a>Alati za učenje računala

Na VM isporučuje se s nekoliko strojnog učenja alata i algoritmima pomoću kojih unaprijed prevedena i unaprijed instaliran lokalno. To obuhvaća:

* **CNTK** (Računalne mreže alata iz Microsoft Research): obuhvaća više razina učenje alata.
* **Vowpal Wabbit**: brzo online učenje algoritam.
* **xgboost**: alat koji omogućuje algoritama optimizirana, boosted stabla.
* **Python**: Anaconda Python dolazi u paketu strojnog učenja algoritmima s bibliotekama kao što su Scikit dodatne. Možete instalirati druge biblioteke pomoću na `pip install` naredbe.
* **R**: biblioteke bogatih strojnog učenja funkcija dostupna je za R. Neke od biblioteke koje su već instalirani su lm, glm, randomForest, rpart. Druge biblioteke moguće je instalirati tako da pokrenete:

        install.packages(<lib name>)

Evo nekih dodatnih informacija o alatima na popisu prvi tri strojnog učenja.

#### <a name="cntk"></a>CNTK
Ovo je otvaranje izvora, precizno učenje komplet alata. Je alat naredbenog retka (cntk), a već se put.

Da biste pokrenuli osnovni uzorka, u ljusci izvršiti sljedeće naredbe:

    # Copy samples to your home directory and execute cntk
    cp -r /dsvm/tools/CNTK-2016-02-08-Linux-64bit-CPU-Only/Examples/Other/Simple2d cntkdemo
    cd cntkdemo/Data
    cntk configFile=../Config/Simple.cntk

Izlaz model nalazi se u *~/cntkdemo/Output/Models*.

Dodatne informacije potražite u odjeljku CNTK [GitHub](https://github.com/Microsoft/CNTK)i [CNTK wiki](https://github.com/Microsoft/CNTK/wiki).


#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit je strojnog učenja sustav koji koristi tehnike kao što su mreži, raspršivanje, allreduce, smanjenja, learning2search, Aktivno, i interaktivnih Upoznavanje.

Da biste pokrenuli alat za na primjer za vrlo osnovne, učinite sljedeće:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

U taj imenik postoje drugi, veća pokazni programi. Dodatne informacije o VW potražite u [odjeljku GitHub](https://github.com/JohnLangford/vowpal_wabbit)i [wiki Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Ovo je biblioteci koja je namijenjena, a optimizirana algoritama boosted (stabla). Ciljne biblioteke je da biste ograničenja izračuni strojeva na extremes potrebne za pružanje veliki stabla povećanje koji je skalabilni, prijenosni i točne.

Prikazuje se kao naredbenog retka, kao i u biblioteku R.

Da biste koristili ovu biblioteku u R, možete pokrenuti sesiju interaktivne R (samo upisivanjem **R** u ljusci) i učitati biblioteku.

Evo jednostavnog primjera možete pokrenuti u R upit:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Da biste pokrenuli naredbeni redak xgboost, Evo naredbe potrebne za izvršavanje u ljusci:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Datoteke .model zapisuje direktorij naveden. Informacije o ovom primjeru pokazni videozapis možete pronaći [na GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Dodatne informacije o xgboost potražite u članku [xgboost dokumentaciju stranice](https://xgboost.readthedocs.org/en/latest/)i [Github spremište](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rattle
Rattle ( **R** **odgovora**nalytical **T**zbriši alat **T**o **L**stjecanje **E**asily) koristi Istraživanje GUI na temelju podataka i Modeliranje. On predstavlja statističke i vizualnih sažetaka podataka, pretvorbe podataka koji se može se lako katalog modeliran, sastavlja unsupervised i supervised modeli iz podataka, predstavlja performansi modela grafički, i postavlja brojčane pokazatelje nove podatke. Također se stvara kod R replikaciju operacije u korisničkom Sučelju koje možete pokrenuti izravno u R ili koristiti kao početnu točku za daljnje analize.

Da biste pokrenuli Rattle, morate biti u grafički sesiji radne površine prijavu. Na terminal, upišite ```R``` da biste unijeli R okruženju. Kada se zatraži R, unesite sljedeće naredbe:

    library(rattle)
    rattle()

Sada grafičkog sučelja otvara s skup kartica. Evo koraka za brzi početak rada u Rattle potrebne za korištenje uzorka vremenske prognoze skupa podataka i stvaranje modela. U neke korake u nastavku se od vas zatraži da biste automatski instaliranje i učitavanje neke potrebne R paketi koji se ne nalazite u sustavu.

>[AZURE.NOTE] Ako nemate pristup da biste instalirali paket direktorija sustava (Zadana postavka), vidjet ćete upit na prozor konzole programa R da biste instalirali paketa u svojoj osobnoj biblioteci. Odgovorite *y* Ako vidite ove upute.

1. Kliknite **izvršiti**.
2. Dijaloški okvir pojavljuje, s pitanjem želite li vam da biste koristili zadani skup podataka vremenske prognoze primjer. Kliknite **da** da biste učitali u primjeru.
3. Kliknite karticu **modela** .
4. Kliknite **izvršavanje** da biste sastavili stabla odlučivanja.
5. Kliknite Prikaz stabla odluke **Crtanje** .
6. Kliknite izborni gumb **skupa stabala** pa kliknite **izvršavanje** da biste sastavili slučajni skupa stabala.
7. Kliknite karticu **procijeni** .
8. Kliknite izborni gumb **rizika** pa kliknite **izvršavanje** da biste prikazali dva iscrtavaju performanse rizika (kumulativnim).
9. Kliknite karticu **zapisnika** da bi se prikazala R Generiraj kod za prethodne operacije.
(Pogreška u trenutnom izdanju sustava Rattle, morate umetnuti u *#* znak ispred *Izvoz zapisnika...* tekst zapisnika.)
10. Kliknite gumb **Izvezi** da biste spremili datoteku skripte R pod nazivom *weather_script. R* da biste polazne mape.

Možete napustiti Rattle i R. Sada možete izmijeniti generirana skripta R ili ga koristiti kao da bi ga bilo kojem trenutku pokrenuti da biste ponovili sve što je učinjeno unutar Rattle korisničkog Sučelja. Osobito za početnike u R, to je jednostavan način brzo učiniti analizu i učenje u jednostavne grafičkog sučelja tijekom automatsko generiranje koda u R da biste izmijenili i/ili Saznajte na računalu.


## <a name="next-steps"></a>Daljnji koraci
Evo kako možete nastaviti učenja i istraživanje.

* U prikazu [podataka znanstvenog na Linux podataka znanstvenog virtualnog računala](machine-learning-data-science-linux-dsvm-walkthrough.md) pokazuje kako izvršiti nekoliko uobičajenih zadataka znanstvenog podataka u VM znanstvenog podataka Linux dodjeli ovdje. 
* Upoznavanje različite znanstvenog Alati za podatke na znanstvenog podataka VM po isprobavate alate opisane u ovom članku. Možete i pokrenuti *dsvm dodatne informacije* na ljuske unutar virtualnog računala za osnovni Uvod ni pokazivači na dodatne informacije o alatima instalirano na VM.  
* Naučiti kako stvoriti završetka do kraja analytical rješenja sustavno pomoću [Timu podataka znanstvenog postupak](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).
* Posjetiti [Galeriju analize Cortana](http://gallery.cortanaanalytics.com) za strojnog učenja i podataka analize uzorke koje koriste analize paket Cortana.
