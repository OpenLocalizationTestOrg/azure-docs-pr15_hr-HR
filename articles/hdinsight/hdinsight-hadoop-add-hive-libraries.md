<properties
pageTitle="Dodavanje biblioteka grozd tijekom stvaranja klaster HDInsight | Azure"
description="Saznajte kako dodati grozd biblioteke (posudu datoteke) na HDInsight klaster tijekom stvaranja klaster."
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
ms.date="09/20/2016"
ms.author="larryfr"/>

#<a name="add-hive-libraries-during-hdinsight-cluster-creation"></a>Dodavanje biblioteka grozd tijekom stvaranja HDInsight klaster

Ako imate biblioteke koje često koristite s grozd na HDInsight, ovaj dokument sadrži informacije o korištenju skripte akcija unaprijed učitati biblioteke tijekom stvaranja klaster. Ovime biblioteke globalno dostupne u grozd (ne morate učitati ih pomoću [Dodavanje POSUDU](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) .)

##<a name="how-it-works"></a>Kako funkcionira

Prilikom stvaranja klaster, po želji možete navesti akciju skriptu koja se izvršava skripte na čvorove klaster dok se ne stvara. Skripta u ovom dokumentu prihvaća jedan parametar koji je WASB mjesto koje sadrži biblioteke (spremljen kao datoteke posudu) koje će biti unaprijed učitani.

Tijekom stvaranja klaster skriptu zbraja datoteke, kopira ih da biste na `/usr/lib/customhivelibs/` direktorij na glave i tempiranja čvorove ih dodaje u na `hive.aux.jars.path` svojstvo u na `core-site.xml` datoteku. Na koji se temelji na Linux klastere i obnavlja na `hive-env.sh` datoteku na mjesto datoteke.

> [AZURE.NOTE] Korištenje akcije skripte u ovom članku stavlja biblioteke u sljedećim scenarijima:
>
> * __HDInsight Linux temelje__ - prilikom korištenja __vrste Hive naredbenog retka__, __WebHCat__ (Templeton) i __HiveServer2__.
> * __HDInsight utemeljen na sustavu Windows__ – prilikom korištenja __vrste Hive naredbenog retka__ i __WebHCat__ (Templeton).

##<a name="the-script"></a>Skripta

__Mjesto za skripte__

Za __klastere sustavom Linux__: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

Za __klastere utemeljen na sustavu Windows__: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

__Preduvjeti__

* Skripte potrebno primijeniti __čvorove glave__ i __čvorove tempiranja__.

* Staklenke koji želite instalirati mora biti pohranjena u spremište blobova platforme Azure u __jedan spremnik__. 

* Račun za pohranu koji sadrži biblioteku posudu datoteke __mora__ biti povezana s klaster HDInsight tijekom stvaranja. To je moguće napraviti na jedan od dva načina:

    * Ako je u spremniku na zadani račun za pohranu za klaster.
    
    * Ako je u spremniku na je spremnik povezane prostora za pohranu. Na primjer, na portalu možete koristiti __Neobavezno konfiguracija__ __računa za pohranu povezani__ da biste dodali dodatan prostor za pohranu.

* Put WASB spremniku mora biti naveden kao parametar skripte akciju. Na primjer, ako su na staklenke spremljene u kontejner __libs__ na račun za pohranu pod nazivom __mystorage__, parametar bio __wasbs://libs@mystorage.blob.core.windows.net/__.

    > [AZURE.NOTE] Ovaj dokument pretpostavlja da ste već stvorili račun za pohranu, bloba spremnik i prenijeti datoteke. 
    >
    > Ako niste stvorili račun za pohranu, učinite to pomoću [portala za Azure](https://portal.azure.com). Da biste stvorili novi spremnik u računa i prijenos datoteka možete koristiti utility kao što su [Explorer Azure prostora za pohranu](http://storageexplorer.com/) .

##<a name="create-a-cluster-using-the-script"></a>Stvaranje klaster pomoću skripte

> [AZURE.NOTE] Sljedeći koraci stvorite novi klaster sustavom Linux HDInsight. Da biste stvorili novi klaster utemeljen na sustavu Windows, odaberite __Windows__ kao klaster OS prilikom stvaranja klaster, a pomoću skripte sustava Windows (PowerShell) umjesto skripte tulumu.
> 
> Da biste stvorili klaster pomoću ovu skriptu možete koristiti i Azure PowerShell i HDInsight .NET SDK. Dodatne informacije o korištenju ove metode potražite u članku [Prilagodba HDInsight klastere s akcijama skripte](hdinsight-hadoop-customize-cluster-linux.md).

1. Pokretanje dodjeljivanje klaster pomoću koraka u [klastere HDInsight dodjele resursa na Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), ali ne dovrši dodjele resursa.

2. Na plohu **Neobavezno konfiguracije** odaberite **Akcije skripte**i navedite podatke kao što je prikazano u nastavku:

    * __Naziv__: Upišite neslužbeni naziv za skripte akciju.
    * __SKRIPTA URI__: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh
    * __LAKŠI__: Odaberite ovu mogućnost
    * __TEMPIRANJA__: Odaberite ovu mogućnost.
    * __ZOOKEEPER__:, polje ostavite prazno.
    * __Parametri__: Unesite adresu WASB spremnik i pohranu račun koji sadrži na staklenke. Na primjer, __wasbs://libs@mystorage.blob.core.windows.net/__.

3. Pri dnu **Akcije skripte**, pomoću gumba **Odaberite** spremanje konfiguracije.

4. Na plohu **Neobavezno konfiguracije** odaberite __Povezane poslovne subjekte prostora za pohranu__ , a zatim __Dodaj ključa za pohranu__ vezu. Odaberite račun za pohranu koji sadrži na staklenke, a zatim pomoću gumba __Odaberite__ da biste spremili postavke i vratili plohu __Neobavezno konfiguracije__ .

5. Pomoću gumba **Odaberite** pri dnu plohu **Neobavezno konfiguracije** da biste spremili informacije o konfiguraciji nije obavezno.

6. Nastavite dodjeljivanje klaster, kao što je opisano u [klastere HDInsight dodjele resursa na Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

Nakon stvaranja klaster završi, moći koristiti staklenke dodati putem ovu skriptu iz grozd bez potrebe za korištenjem na `ADD JAR` izjava.

##<a name="next-steps"></a>Daljnji koraci

U ovom dokumentu ste naučili kako dodati grozd biblioteke koje se nalaze u datotekama posudu HDInsight klaster tijekom stvaranja klaster. Dodatne informacije o radu s grozd potražite u članku [Korištenje vrste Hive s HDInsight](hdinsight-use-hive.md)
