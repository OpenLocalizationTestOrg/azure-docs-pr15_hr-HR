<properties 
    pageTitle="Prilagodba klastere Hadoop za proces timu podataka znanstvenog | Microsoft Azure" 
    description="Popularne module za Python postao dostupan u prilagođene klastere Azure HDInsight Hadoop."
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
    ms.date="09/19/2016" 
    ms.author="hangzh;bradsev" />

# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Prilagodba klastere Azure HDInsight Hadoop za proces znanstvenog timu podataka 

U ovom se članku opisuje kako Prilagodba programa HDInsight Hadoop klaster instalacijom Anaconda 64-bitni (Python 2.7) na svakom čvor kada je klaster dodjeli kao servis HDInsight. Također se prikazuje kako pristupiti headnode slanje prilagođenih zadataka klaster. Ovu prilagodbu olakšava mnoge popularne moduli Python uvrštene u Anaconda radi lakšeg dostupni za upotrebu u korisnički definirane funkcije (UDF-ove) namijenjene za obradu grozd zapisa u klasteru. Upute o postupku koji se koristi u ovom scenariju, potražite u članku [Slanje grozd upita](machine-learning-data-science-move-hive-tables.md#submit).

Na izborniku ispod veze na temu koja opisuje kako postaviti različitim okruženjima znanstvenog podataka koristi [Proces znanstvenog timu podataka (TDSP)](data-science-process-overview.md).

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]


## <a name="customize"></a>Prilagodba klaster Hadoop Azure HDInsight

Da biste stvorili prilagođeni HDInsight Hadoop klaster, korisnici morati prijaviti na [**Klasični Portal Azure**](https://manage.windowsazure.com/), u donjem lijevom kutu kliknite **Novo** , a zatim odaberite podatkovne usluge -> HDINSIGHT -> **Stvaranje Prilagođeno** da biste otvorili prozor s **Detaljima klaster** . 

![Stvaranje radnog prostora](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Unos naziva klaster će biti stvoren na stranici za konfiguraciju 1 i prihvatite zadane vrijednosti za druga polja. Kliknite strelicu da biste prešli na sljedeću stranicu konfiguracije. 

![Stvaranje radnog prostora](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Na stranici za konfiguraciju 2, unesite broj **ČVOROVE podataka**, odaberite **PODRUČJE/VIRTUALNE MREŽE**i odaberite veličine **GLAVE ČVOR** i **ČVOR podataka**. Kliknite strelicu da biste prešli na sljedeću stranicu konfiguracije.

>[AZURE.NOTE] **MREŽNI REGIJE/VIRTUALNE** mora biti jednak regija računa za pohranu koji će se koristiti za HDInsight Hadoop klaster. U suprotnom u četvrtom stranicu za konfiguriranje računa za pohranu koje korisnici se neće prikazivati na padajućem popisu **Naziv RAČUNA**.

![Stvaranje radnog prostora](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

Na stranici za konfiguraciju 3, navedite korisničko ime i lozinku za klaster HDInsight Hadoop. **Ne šalji** odaberite _Enter Metastore grozd/Oozie_. Nakon toga kliknite strelicu da biste prešli na sljedeću stranicu konfiguracije. 

![Stvaranje radnog prostora](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Na stranici za konfiguraciju 4, navedite naziv računa za pohranu, zadani spremnik klaster HDInsight Hadoop. Ako korisnicima odaberite _Stvaranje kontejnera za zadano_ **ZADANI SPREMNIK** padajućem popisu, stvorit će se kontejnera s istim nazivom kao klaster. Kliknite strelicu da biste prešli na posljednju stranicu za konfiguraciju.

![Stvaranje radnog prostora](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

Na posljednjoj stranici Konfiguracija **Skripte akcije** , kliknite gumb **Dodaj akciju skripte** i tekstna polja ispunili sljedeće vrijednosti.
 
* **Ime** - bilo koji niz kao naziv ovu akciju skripte. 
* **Vrsta ČVORA** – odaberite **sve čvorove**. 
* **SKRIPTA URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
    * *publicscripts* je javno spremnik na računu za pohranu 
    * *getgoing* koristimo za zajedničko korištenje datoteka skriptu PowerShell da biste olakšali rad korisnika u Azure. 
* **Parametri** - (ostavite prazno)

Na kraju, kliknite kvačicu za početak stvaranja prilagođenih klaster HDInsight Hadoop. 

![Stvaranje radnog prostora](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Pristup čvora glave Hadoop klaster

Korisnici moraju Omogućivanje daljinskog pristupa klaster Hadoop u Azure prije čvor glavni klaster Hadoop mogu pristupiti putem RDP. 

1. Prijavite se u [**Klasične Portal Azure**](https://manage.windowsazure.com/), odaberite **HDInsight** na lijevoj strani, odaberite svoj klaster Hadoop s popisa klastere, kliknite karticu **KONFIGURACIJA** , a zatim **Omogući UDALJENE** ikona pri dnu stranice.
    
    ![Stvaranje radnog prostora](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)

2. U prozoru za **Konfiguriranje udaljene radne površine** unesite polja KORISNIČKO ime i lozinku pa odaberite datum isteka za daljinski pristup. Kliknite kvačicu da biste omogućili daljinski pristup čvor glavni klaster Hadoop.

    ![Stvaranje radnog prostora](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)
    
>[AZURE.NOTE] Korisničko ime i lozinku za daljinski pristup nisu korisničko ime i lozinku koje koristite prilikom stvaranja klaster Hadoop. To su zasebnom skup vjerodajnica. Osim toga, datum isteka daljinski pristup mora biti unutar 7 dana od trenutnog datuma.

Kada je omogućen daljinski pristup, kliknite **POVEŽI se** pri dnu stranice udaljene u glavni čvor. Se prijavite na glavni čvor klaster Hadoop tako da unesete vjerodajnice za daljinski pristup korisnika koje ste prethodno naveli.

![Stvaranje radnog prostora](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

Daljnji koraci u postupku naprednom analitikom mapiraju se u [Timu podataka znanstvenog procesa (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) , a može sadržavati korake koji premještanje podataka u HDInsight, obradu i je li uzorak u Priprema za učenje iz podataka s Azure strojnog učenja.

Upute o tome kako pristupiti Python module koji su uključeni u Anaconda iz čvor glavni klaster u korisnički definirane funkcije (UDF-ove) koji se koriste za obradu grozd zapisa pohranjenih u klasteru potražite u članku [upute za slanje grozd upita](machine-learning-data-science-move-hive-tables.md#submit) .

 
