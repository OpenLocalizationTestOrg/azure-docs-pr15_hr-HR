<properties
    pageTitle="Što je virtualnog računala znanosti za podatke? | Microsoft Azure"
    description="Naučite i ključni scenariji značajke, za početak rada s podataka znanstvenog virtualnim strojevima, okruženju i komplet alata za jeste li spremni za analizu."
    keywords="Alati podataka znanstvenog, podataka znanstvenog virtualnog računala, a zatim Alati za znanstvenog podataka, znanstvenog linux podataka"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="bradsev" />


# <a name="introduction-to-the-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Uvod u u oblaku podataka znanstvenog virtualnog računala za Linux i Windows

Podaci znanstvenog virtualnog računala je prilagođene VM slika na posebno za obavljanje znanstvenog podataka tvrtke Microsoft Azure oblaka. Ima mnoge popularne podataka Znanstvena i druge alate za unaprijed instalirana i unaprijed konfiguriran za jump-start stvaranje Inteligentna aplikacija za napredne analize. Nije dostupan u sustavu Windows Server 2012 ili verzije OpenLogic 7,2 CentOS sustavom Linux. 

U ovoj se temi opisuje što možete učiniti s znanstvenog VM podataka se navode ključni scenariji za korištenje na VM, itemizes ključne značajke dostupne u verzijama sustava Windows i Linux te sadrži upute o tome kako početi koristiti ih.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Što sve mogu raditi s podacima znanstvenog virtualne računalu?

Cilj od podataka znanstvenog virtualnog računala je kako bi pružio profesionalce podataka na sve razine vještina i uloge okruženju Znanstvena friction slobodno podataka. U ovom VM sprema dosta vremena koje želite potrošiti ako imali poslednjeg out okruženju usporediti vlastite. Umjesto toga odmah znanstvenog projekta podataka u novostvorenu VM instanci. 

Znanstvena VM podataka je dizajniran i konfigurirati za rad s scenariji za korištenje širokog raspona. Koje možete mijenjati vaše okruženje prema gore ili dolje projekta potrebna promjena. Vi ste moći koristiti Preferirani jezik zadatke znanstvenog podatke za program. Možete instalirati druge alate i prilagoditi sustav odgovaraju potrebama točan.
 
## <a name="key-scenarios"></a>Ključni scenariji
U ovom se odjeljku predlaže neke ključni scenariji za koje se može uvesti znanstvenog VM podataka.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Radnu površinu konfiguriranog analize u oblaku

Znanstvena VM podataka sadrži osnovne konfiguracija za timovima znanstvenog podataka koje želite li svoje lokalne stolna računala zamijenite upravljanih oblaka radnu površinu. U ovom osnovne omogućuje koji sve fizičari podataka na tim dosljedan postavljanje pomoću kojih se potvrda eksperimenata i Promicanje suradnju. Također Spušta troškove smanjivanje teret sustava i spremanje na vrijeme potrebno za procjenu, instalaciju i održavanje različitih softverskih paketa potrebno učiniti napredne analize.  

### <a name="data-science-training-and-education"></a>Obuka znanstvenog podataka i obrazovne ustanove

Voditelje tečajeva za Enterprise i pedagozima koji Podučavanje podataka znanstvenog klase obično omogućuje sliku virtualnog računala da biste bili sigurni da imate studentima dosljedan postavljanje i primjere rad predvidljivije. Znanstvena VM podataka stvara okruženju osvježavati s dosljedan postavljanje koje eases izazove nekompatibilnost programa i podrška. Slučajevi u kojima te okruženja morati biti ugrađeni često, posebice za kraći klase obuka, uvidjeti prednost značajno.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Kapacitet elastic na zahtjev za veliki projekata

Hackathons/natjecanjima znanstvenog podataka ili Modeliranje veliki podataka i istraživanje potreban skalirana out hardver kapacitet, obično kratki trajanje. Znanstvena VM podataka može pridonijeti replicirati okruženje znanstvenog podataka brzo na zahtjev na poslužiteljima skaliranu odgovor koji omogućuju eksperimenata potrebno high-powered računalno resursa koji će se pokrenuti.

### <a name="short-term-experimentation-and-evaluation"></a>Kratkotrajni početak i procjenu

Znanstvena VM podataka može biti koja služi za procjenu ili dodatne alate kao što je Microsoft R Server, SQL Server, Visual Studio tools, Jupyter, duboke učenje / ML Kompleti alata i novih alata popularne u zajednici s minimalnim postavljanje truda. Budući da znanstvenog VM podataka mogu se postaviti brzo, se mogu primijeniti drugi kratkotrajni scenariji za korištenje kao što su replikaciju objavljene eksperimenata izvršavanja pokazni programi, sljedeći vodiči u online sesije i vodiči za konferencije.


## <a name="whats-included-in-the-data-science-vm"></a>Što je sve obuhvaćeno znanstvenog VM podataka?

Podaci znanstvenog virtualnog računala ima mnoge popularne znanstvenog Alati za podatke već instalacije i konfiguracije. Sadrži i alatima koji olakšavaju rad s različitim Azure proizvodi i analize podataka. Možete istraživati i stvoriti predvidljivu modela velikih skupova podataka pomoću poslužitelja za Microsoft R ili pomoću SQL Server 2016. Mnoštvom drugih alata iz zajednice Otvori izvor i Microsoft također su uključeni, kao i uzorak koda i bilježnice. U sljedećoj su tablici itemizes i uspoređuje glavne komponente ugrađene u izdanja sustava Windows i Linux od podataka znanstvenog virtualnog računala.


|**Izdanje sustava Windows** | **Linux Edition** |
|----------------|---------------|
|Microsoft R poslužitelju za razvojne inženjere Edition | Microsoft R poslužitelju za razvojne inženjere Edition |
|Anaconda Python 2.7, 3.5 | Anaconda Python 2.7, 3.5 |
|Poslužitelj Jupyter bilježnice (R, Python) | JupyterHub: Više korisnika Jupyter bilježnice (R, Python, Julia) |
|SQL Server 2016 za razvojne inženjere Edition: Skalabilni analize u bazu podataka sa servisima R | Postgres, SQuirreL SQL (alat baze podataka), upravljačke programe za SQL Server i naredbenog retka (bcp, sqlcmd) |
|Visual Studio zajednice Edition 2015 (IDE) </br> -Azure HDInsight (Hadoop), Lake podataka SQL Server Data Alati </br> -Node.js, Python i R alate za Visual Studio |IDEs i uređivača </br> -Eclipse s dodatak kompleta alata za Azure </br> -Emacs (s DRESA, auctex) gedit |
|Power BI desktop | -- |
|Alati za učenje računala </br> -Integracija s Azure strojnog učenja </br> -CNTK (precizno učenje/AI) </br> -Xgboost (Popularno ML alata u natjecanjima znanstvenog podataka) </br> -Wabbit Vowpal (brzo online učenik) </br> -Rattle (vizualni Brzi start podataka i alat za analize) </br> -Mxnet (precizno učenje/AI) | Alati za učenje računala </br> -Integracije s Azure strojnog učenja </br> -CNTK (precizno učenje/AI) </br> -Xgboost (Popularno ML alata u natjecanjima znanstvenog podataka) </br> -Wabbit Vowpal (brzo online učenik) </br> -Rattle (vizualni Brzi start podataka i alat za analize)  |
| SDK-ovi za pristup Azure i Cortana obavještavanje paket servisa | SDK-ovi za pristup Azure i Cortana obavještavanje paket servisa |
| Alati za premještanje podataka i upravljanja resursima Azure i velikih skupova podataka: Explorer Azure prostora za pohranu, EŽA, PowerShell, AdlCopy (Azure podataka Lake), AzCopy, dtui (za DocumentDB), pristupnik za upravljanje podacima | Alati za premještanje podataka i upravljanja resursima Azure i velikih skupova podataka: Explorer Azure prostora za pohranu, EŽA |
| Brojka, dodatak Team Services za Visual Studio | Brojka |
| Windows priključak od najpopularnijih Linux/Unix naredbenog retka uslužni programi dostupni putem GitBash/naredbeni redak | -- |



## <a name="how-to-get-started-with-the-windows-data-science-vm"></a>Upute za početak rada s VM znanstvenog podataka za Windows

- Stvaranje instance komponente VM u sustavu Windows tako da odete na [ovu stranicu](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) , a zatim odaberete zeleni gumb **Stvaranje virtualnog računala** .
- Prijavite se da biste na VM s udaljene radne površine pomoću vjerodajnica koje ste naveli prilikom stvaranja na VM.
- Da biste otkrili i pokretanje alata koji su dostupni, kliknite izbornik **Start** .


## <a name="get-started-with-the-linux-data-science-vm"></a>Početak rada s VM znanosti za Linux podataka

- Stvaranje pojave na VM na Linux (OpenLogic CentOS sustavom) tako da odete na [ovu stranicu](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) , a zatim odaberete gumb **Stvaranje virtualnog računala** .
- Prijavite se VM SSH klijenta, kao što su Putty ili SSH naredbi pomoću vjerodajnica koje ste naveli kada ste stvorili u VM.
- U promptu ljuske unesite dsvm dodatne informacije.
- Grafički radna površina, preuzmite X2Go klijent za svoju platformu klijent [ovdje](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) i slijedite upute u dokumentu Linux podataka znanstvenog VM [dodjele Linux podataka znanstvenog virtualnog računala](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).


## <a name="next-steps"></a>Daljnji koraci

### <a name="for-the-windows-data-science-vm"></a>Za podatke znanstvenog Windows VM

- Dodatne informacije o tome kako pokrenuti određene Alati dostupni na verzije sustava Windows potražite u članku [Dodjeljivanje Microsoft podataka znanstvenog virtualnog računala](machine-learning-data-science-provision-vm.md) i
-  Dodatne informacije o tome kako izvršiti razne zadatke potrebne za znanstvenog projekta podataka na Windows VM potražite u članku [deset stvari koje možete obaviti na znanstvenog podataka virtualnog računala](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>Za znanstvenog Linux podataka VM

- Dodatne informacije o tome kako pokrenuti određene Alati dostupni na verziju Linux potražite u članku [Dodjeljivanje Linux podataka znanstvenog virtualnog računala](machine-learning-data-science-linux-dsvm-intro.md).
- Objašnjenje koji pokazuje kako izvršiti nekoliko uobičajenih podataka znanstvenog zadataka s Linux VM potražite u članku [znanstvenog podataka na Linux podataka znanstvenog virtualnog računala](machine-learning-data-science-linux-dsvm-walkthrough.md).
