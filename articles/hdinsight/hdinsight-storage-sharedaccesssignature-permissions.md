<properties
pageTitle="Ograničiti pristup HDInsight podataka pomoću zajednički pristup potpisa"
description="Saznajte kako koristiti zajednički pristup potpisa da biste ograničili pristup HDInsight podataka pohranjenih u BLOB-ova Azure prostora za pohranu."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/11/2016"
ms.author="larryfr"/>

#<a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-with-hdinsight"></a>Korištenje Azure prostora za pohranu zajednički pristup potpise ograničiti pristup podacima s HDInsight

HDInsight koristi Azure pohranu blob-ova za pohranu podataka. Dok HDInsight mora imati puni pristup blob koristiti kao zadani prostor za pohranu za klaster, možete ograničiti dozvole za podataka pohranjenih u drugim blob-ova koristi klaster. Na primjer, preporučujemo vam da biste neki podaci samo za čitanje. To možete korištenje potpisa za zajedničko korištenje programa Access.

Zajednički pristup potpisa (SAS) su značajku računi Azure prostora za pohranu, koja omogućuje ograničavanje pristupa podacima. Na primjer, koja omogućuje pristup samo za čitanje podataka. U ovom dokumentu će Saznajte kako koristiti SAS da biste omogućili čitanje i pristup samo za popis blob spremniku iz servisa HDInsight.

##<a name="requirements"></a>Preduvjeti

* Azure pretplate

* C# ili Python. C# kod primjer prikazuje se kao Visual Studio rješenja.

    * Visual Studio moraju biti verzije 2013 ili 2015.
    
    * Python mora biti verzija 2.7 ili novija verzija

* HDInsight sa sustavom Linux skupine ili [Azure PowerShell] [ powershell] – ako imate postojeću sustavom Linux klaster, Ambari možete koristiti da biste dodali potpis programa Access za zajedničko korištenje klaster. Ako nije, Azure PowerShell možete koristiti da biste stvorili novi klaster i dodavanje potpisa pristup zajednički se koristi tijekom stvaranja klaster.

* Primjer datoteke iz [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Ovo spremište sadrži sljedeće:

    * Projekta za Visual Studio koji možete stvoriti spremnik za pohranu, pohranjene pravila i SAS za korištenje sa servisa HDInsight
    
    * Python skriptu koja možete stvoriti spremnik za pohranu, pohranjene pravila i SAS za korištenje s HDInsight
    
    * PowerShell skriptu koja možete stvoriti novu HDInsight klaster i konfigurirajte ga tako da biste koristili u SAS.

##<a name="shared-access-signatures"></a>Zajednički pristup potpisa

Postoje dva oblika potpisa za zajedničko korištenje programa Access:

* Ad hoc: vrijeme početka, vrijeme isteka i dozvole za na SAS su sve navedeno na SAS URI (ili izričitu, u slučaju gdje je ispušten vrijeme početka).

* Spremljeni pravilnik o pristupu: pravilnik pohranjene pristupa je definiran na spremniku resursa – blob spremnik, tablice, reda čekanja ili zajedničko korištenje datoteka - i može se koristiti za upravljanje ograničenjima za jedan ili više potpisa zajednički pristup. Kada SAS pridružiti pohranjene pristup pravila, u SAS nasljeđuje ograničenja - vrijeme početka, vrijeme isteka i - definiranih dozvola za pravilnik pohranjene pristupa.

Razlika između dva oblika je važno da jedan scenarij ključa: opoziva. SAS je URL-a, da svatko tko primi na SAS mogli koristiti, bez obzira na to koji ste tražili je počinje sa. Ako SAS je objavljen javno, može se koristiti svatko na svijetu. SAS koju je distribuirao vrijedi dok dogodit će se nešto od četiri stvari:

1. Vrijeme isteka navedenog u SAS zbirka dostigne.

2. Vrijeme isteka navedenog pravilnik pohranjene pristupa upućuje na SAS zbirka dostigne (Ako se poziva pravilnik pohranjene pristupa i ako je naveden u vrijeme isteka). To može dogoditi zbog minutama interval ili jer ste izmijenili pravila pohranjene access da bi se vrijeme isteka u prošlosti, što je jedan od načina da biste opozvali na SAS.

3. Pravilnik pohranjene pristupa upućuje na SAS izbriše, koji je drugi način za otkazivanje na SAS. Imajte na umu da ako ponovno stvoriti pravilnik pohranjene pristupa s istim nazivom svih tokena SAS postojeće će ponovno moraju biti valjane prema dozvole povezane s to pravilo pohranjene pristup (pod pretpostavkom koje vrijeme isteka na na SAS nije prošao). Ako su intending da biste opozvali na SAS, obavezno koristite neki drugi naziv ako ponovno stvoriti pravilnik o pristupu s vrijeme isteka u budućnosti.

4. Ključ za račun koji je korišten za stvaranje na SAS je regenerated. Imajte na umu da to učinite uzrokovat će sve aplikacije komponente neće uspjeti dok se ne ažuriraju da biste koristili druge ključ valjani računa ili ključ upravo regenerated računa pomoću tog ključ za račun.

> [AZURE.IMPORTANT] Zajednički pristup potpis URI povezan s ključ računa koji se koriste za stvaranje potpis, a pridruženog pohranjene pravilnik o pristupu (ako ih ima). Ako nema pohranjene pristup pravila nije naveden, jedini način da biste opozvali zajednički pristup potpis je da biste promijenili ključ za račun. 

Preporučuje se da uvijek koristite pravila pohranjene access tako da možete opozvati potpisa ili produžite datum isteka prema potrebi. Koraci u ovom dokumentu pohranjene pravilnike za pristup da biste generirali SAS.

Dodatne informacije o zajednički pristup potpisa potražite u članku [objašnjenje SAS modela](../storage/storage-dotnet-shared-access-signature-part-1.md).

##<a name="create-a-stored-policy-and-generate-a-sas"></a>Stvaranje pravila spremljene i generiranje SAS

Trenutno spremljene pravila morate stvoriti programski. Možete pronaći i C# i Python primjer Stvaranje spremljene pravila i SAS kod [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).

###<a name="create-a-stored-policy-and-sas-using-c"></a>Stvaranje pravila spremljene i SAS pomoću C\#

1. Otvorite rješenje u Visual Studio.

2. U pregledniku rješenja, desnom tipkom miša kliknite na projektu __SASToken__ , a zatim odaberite __Svojstva__.

3. Odaberite __Postavke__ , a zatim dodajte vrijednosti za sljedeće stavke:

    * StorageConnectionString: Niza za povezivanje za pohranu račun koji želite stvoriti pravila spremljene i SAS za. Mora biti u obliku `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` gdje `myaccount` je naziv vašeg računa za pohranu i `mykey` je ključ za račun za pohranu.
    
    * Naziv spremnika: Spremnik na računu za pohranu koji želite ograničiti pristup.
    
    * SASPolicyName: Naziv da biste koristili spremljene pravila koja će se stvoriti.
    
    * FileToUpload: Put do datoteke koje će se prenijeti u spremniku.
    
4. Pokrenite projekt. Pojavit će se prozor konzole i informacije sličnu ovoj prikazat će se kada je generiran na SAS:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
        
    Spremite pravila token SAS kako ćete to kada je račun za pohranu Pridruživanjem svoj klaster HDInsight. Već, morat ćete naziv računa za pohranu i naziv spremnika.
    
###<a name="create-a-stored-policy-and-sas-using-python"></a>Stvaranje pravila spremljene i SAS pomoću Python

1. Otvorite datoteku SASToken.py i promijenite sljedeće vrijednosti:

    * pravila\_naziv: naziv koji će se koristiti za spremljene pravila koja će se stvoriti.
    
    * prostor za pohranu\_račun\_naziv: naziv vašeg računa za pohranu.
    
    * prostor za pohranu\_račun\_ključ: ključ za račun za pohranu.
    
    * prostor za pohranu\_spremnik\_naziv: spremnik u račun za pohranu koji želite ograničiti pristup.
    
    * primjer\_datoteke\_put: put do datoteke koje će se prenijeti u spremniku

2. Pokrenite skriptu. Ga prikazat će se token SAS sličnu ovoj kada se dovrši skripte:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
    
    Spremite pravila token SAS kako ćete to kada je račun za pohranu Pridruživanjem svoj klaster HDInsight. Već, morat ćete naziv računa za pohranu i naziv spremnika.
    
##<a name="use-the-sas-with-hdinsight"></a>Korištenje SAS s HDInsight

Prilikom stvaranja programa HDInsight klaster, morate navesti primarni prostora za pohranu računa te po želji možete navesti račune dodatni prostor za pohranu. Obje metode dodavanja prostora za pohranu potrebno puni pristup račune za pohranu i spremnika koji se koriste.

Da bi se pristup potpis zajednički koristite da biste ograničili pristup spremniku, morate dodati prilagođeni unos konfiguracije __core web-mjesta__ za klaster.

* Za klastere __utemeljen na sustavu Windows__ ili __sustavom Linux__ HDInsight, to možete tijekom stvaranja klaster pomoću komponente PowerShell.

* Za __sustavom Linux__ HDInsight klastere promijeniti konfiguraciju nakon stvaranja klaster pomoću Ambari.

###<a name="create-a-new-cluster-that-uses-the-sas"></a>Stvaranje nove klaster koji koristi u SAS

Primjer stvaranje klaster za HDInsight koji koristi u SAS obuhvaćeno na `CreateCluster` imeničkog u spremištu. Da biste koristili, poduzmite sljedeće korake:

1. Otvaranje u `CreateCluster\HDInsightSAS.ps1` datoteku u uređivaču teksta i mijenjati sljedeće vrijednosti na početku dokumenta.

        # Replace 'mycluster' with the name of the cluster to be created
        $clusterName = 'mycluster'
        # Valid values are 'Linux' and 'Windows'
        $osType = 'Linux'
        # Replace 'myresourcegroup' with the name of the group to be created
        $resourceGroupName = 'myresourcegroup'
        # Replace with the Azure data center you want to the cluster to live in
        $location = 'North Europe'
        # Replace with the name of the default storage account to be created
        $defaultStorageAccountName = 'mystorageaccount'
        # Replace with the name of the SAS container created earlier
        $SASContainerName = 'sascontainer'
        # Replace with the name of the SAS storage account created earlier
        $SASStorageAccountName = 'sasaccount'
        # Replace with the SAS token generated earlier
        $SASToken = 'sastoken'
        # Set the number of worker nodes in the cluster
        $clusterSizeInNodes = 2
        
    Ako, na primjer, promijeniti `'mycluster'` naziv klaster koju želite stvoriti. Vrijednosti SAS moraju podudarati s vrijednostima iz prethodnih koraka prilikom stvaranja računa za pohranu i SAS token.
    
    Kada ste promijenili vrijednosti, spremite datoteku.

1. Otvorite novi upit Azure PowerShell. Ako nisu poznati Azure PowerShell ili ste instalirali, pročitajte članak [instalirati i konfigurirati Azure PowerShell][powershell].

2. Iz upita, koristite sljedeće za provjeru autentičnosti Azure pretplatu:

        Login-AzureRmAccount
    
    Kada se to od vas zatraži, prijavite se pomoću računa za vašu pretplatu Azure.
    
    Ako je vašu prijavu povezana s više pretplata Azure, možda ćete morati koristiti `Select-AzureRmSubscription` da biste odabrali pretplatu koju želite koristiti.

2. Iz upita, promijeniti imenika na `CreateCluster` direktorija koja sadrži datoteku HDInsightSAS.ps1. Zatim pokretanje izvješća pomoću sljedeće skripte
        
        .\HDInsightSAS.ps1
    
    Prilikom pokretanja skripte je će se prijaviti izlazna odzivniku komponente PowerShell kao stvara resursa račune grupe i prostora za pohranu. Zatim ponudit će vam da biste unijeli HTTP korisnički klaster HDInsight. Ovo je korisnički račun koji se koristi za zaštitu pristupa HTTP/s klaster.
    
    Ako stvarate klaster sa sustavom Linux i zatražit će se za SSH korisničko ime i lozinku. Koristi se za daljinsko prijavu klaster.
    
    > [AZURE.IMPORTANT] Kada se pojavi upit za HTTP/s ili SSH korisničko ime i lozinku, morate unijeti lozinku koja zadovoljava sljedeće kriterije:
    >
    > - Mora biti najmanje 10 znakova
    > - Mora sadržavati najmanje jednu znamenku
    > - Mora sadržavati najmanje jedan znak koji nisu alfanumerički
    > - Mora sadržavati najmanje jedan gornjem ili malim slovom


Trebat će neko vrijeme za ovu skriptu da biste dovršili, obično oko 15 minuta. Kada završi skriptu bez sve pogreške, klaster je stvorena.

###<a name="update-an-existing-cluster-to-use-the-sas"></a>Ažuriranje postojećih klaster da biste koristili u SAS

Ako imate postojeću sustavom Linux klaster, možete dodati na SAS konfiguracije __core web-mjesta__ pomoću sljedećih koraka:

1. Otvorite web Ambari korisničkog Sučelja za svoj klaster. Adresa na ovoj stranici nije https://YOURCLUSTERNAME.azurehdinsight.net. Kada se to od vas zatraži, autentičnost klaster koristi naziv admin (administrator) i lozinke koje se koriste prilikom stvaranja klaster.

2. Na lijevoj strani web Ambari korisničkog Sučelja odaberite __HDFS__ , a zatim odaberite karticu __Configs__ sredini stranice.

3. Odaberite karticu __Dodatno__ , a zatim se pomaknite dok ne pronađete u odjeljku __Prilagođeno core web-mjesta__ .

4. Proširite odjeljak __Prilagođeno core web-mjesta__ , a zatim se pomaknite do kraja i odaberite željenu vezu __... Dodaj svojstvo__ . Pomoću sljedeće vrijednosti za polja __ključa__ i __vrijednosti__ :

    * __Ključ__: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
    
    * __Vrijednost__: U SAS vratio ste već pokrenuli aplikaciju C# ili Python
    
    __Naziv SPREMNIKA__ zamijenite nazivom spremnik koji se koristi s C# ili SAS aplikacije. Zamijenite __STORAGEACCOUNTNAME__ naziv računa za pohranu koji ste koristili.

5. Kliknite gumb __Dodaj__ da biste spremili ovaj ključ i vrijednost, a zatim kliknite gumb __Spremi__ da biste spremili promjene konfiguracije. Kada se to od vas zatraži, dodajte opis promjena ("Dodavanje SAS prostora za pohranu pristupa", primjerice), a zatim __Spremi__.

    Kada dovršite promjene, kliknite __u redu__ .

    > [AZURE.IMPORTANT] Time promjene konfiguracije, ali morate ponovno pokrenuti nekoliko usluga prije promjene stupa na snagu.

6. Na webu Ambari korisničkog Sučelja na popisu s lijeve strane odaberite __HDFS__ , a zatim __Ponovno pokrenite sve__ iz __Servisa akcije__ padajućeg popisa na desnoj strani. Kada se to od vas zatraži, odaberite __Uključi održavanja__ i odaberite __Conform ponovno pokrenite sve".

    Ponovite taj postupak za MapReduce2 i YARN stavke s popisa na lijevoj strani stranice.

7. Kada ih ponovnog pokretanja, odaberite svaki od njih te onemogućite održavanja iz __Servisa akcije__ padajući popis.

##<a name="test-restricted-access"></a>Testiranje ograničeni pristup

Da biste potvrdili da ste ograničeni pristup, pomoću sljedećih načina:

* Da biste postigli __utemeljen na sustavu Windows__ klastere HDInsight koristite udaljene radne površine povezati klaster. Dodatne informacije potražite u članku [Connecto za HDInsight pomoću RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp) .

    Nakon uspostave pomoću ikone __Hadoop naredbenog retka__ na stolnom računalu otvorite naredbeni redak.

* Da biste postigli __sustavom Linux__ klastere HDInsight koristite SSH za povezivanje s klaster. Potražite u nekom od sljedeće informacije o korištenju SSH s operacijskim sustavom Linux klastere:

    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight Linux, OS X i Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
Nakon uspostave klaster, poduzmite sljedeće korake da biste provjerili možete li samo čitanju i popis stavki na računu za pohranu SAS:

1. U naredbeni redak upišite sljedeće. __SASCONTAINER__ zamijenite nazivom spremnika stvorili za račun SAS prostora za pohranu. Zamijenite __SASACCOUNTNAME__ naziv računa za pohranu koji se koristi za na SAS:

        hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    
    To će se popis sadržaj kontejneru, trebali biste uključiti datoteku koju je učitana kontejner i SAS stvaranja.
    
2. Koristite sljedeće da biste potvrdili da može čitati sadržaj datoteke. Zamjena __SASCONTAINER__ i __SASACCOUNTNAME__ kao u prethodnom koraku. Zamijenite __FILENAME__ naziv datoteke koja se prikazuje u prethodne naredbe:

        hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
        
    To će se popis sadržaj datoteke.
    
3. Da biste datoteku preuzeli na lokalnom sustavu datoteka, koristite sljedeće:

        hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    
    To će preuzimanje datoteke u lokalnu datoteku pod nazivom __testfile.txt__.

4. Da biste prenijeli lokalne datoteke na novu datoteku pod nazivom __testupload.txt__ na SAS prostor za pohranu, koristite sljedeće:

        hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    
    Prikazat će se poruka slična sljedećoj:
    
        put: java.io.IOException
        
    Ta se pogreška pojavljuje jer je mjesto za pohranu čitanja + samo s popisa. Da biste stavili podatke na zadani prostor za pohranu za klaster, što je snimanje, koristite sljedeće:
    
        hdfs dfs -put testfile.txt wasbs:///testupload.txt
        
    Ovaj put postupak treba uspješno dovršena.
    
##<a name="troubleshooting"></a>Otklanjanje poteškoća

###<a name="a-task-was-canceled"></a>Zadatak je otkazana

__Simptomi__: prilikom stvaranja klaster pomoću skriptu PowerShell, možete dobiti sljedeću poruku o pogrešci:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

__Uzrok__: ta se pogreška može dogoditi ako koristite lozinke za administratore/HTTP korisnika za klaster ili (u slučaju sustavom Linux klastere,) SSH korisnika.

__Razlučivost__: koristite lozinku koja zadovoljava sljedeće kriterije:

- Mora biti najmanje 10 znakova
- Mora sadržavati najmanje jednu znamenku
- Mora sadržavati najmanje jedan znak koji nisu alfanumerički
- Mora sadržavati najmanje jedan gornjem ili malim slovom

##<a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili kako dodati prostor za pohranu ograničeni pristup svoj klaster HDInsight, dodatne drugi načini raditi s podacima na svoj klaster:

* [Korištenje grozd s HDInsight](hdinsight-use-hive.md)

* [Korištenje Svinja s HDInsight](hdinsight-use-pig.md)

* [Korištenje MapReduce s HDInsight](hdinsight-use-mapreduce.md)

[powershell]: ../powershell-install-configure.md
