<properties
    pageTitle="Instalacija RStudio s poslužiteljem R na HDInsight (pretpregled) | Microsoft Azure"
    description="Kako se instalira RStudio s poslužiteljem R na HDInsight (pretpregled)."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/16/2016"
   ms.author="jeffstok"/>


# <a name="installing-rstudio-with-r-server-on-hdinsight-preview"></a>Instaliranje RStudio s poslužiteljem R na HDInsight (pretpregled)

Dostupno više integriranih razvojnih okruženja (IDE) R danas, uključujući Microsoftovim nedavno objaviti [R alate za Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS), obitelji alata za stolna računala i poslužitelja s [RStudio](https://www.rstudio.com/products/rstudio-server/)ili Walware korisnika koji se temelji na Eclipse [StatET](http://www.walware.de/goto/statet). Među najraširenijim na Linux je korištenje [RStudio poslužitelja](https://www.rstudio.com/products/rstudio-server/) koji omogućuje IDE za utemeljenima na pregledniku koriste udaljenim klijentima.  Instalacija RStudio poslužitelja na rub čvora programa HDInsight Premium klaster nudi sve funkcije programa IDE za razvoj i izvršavanja skripti R s poslužiteljem R na klaster, a može biti znatno produktivnosti od zadane korištenje konzole za R.

U ovom članku će Saznajte kako instalirati zajednice korisnika (besplatno) verzije RStudio poslužitelja na rub čvor klastere pomoću prilagođene skripte. Ako biste radije komercijalno licenciranu verziju Pro RStudio poslužitelj, morate slijediti upute za instalaciju [RStudio](https://www.rstudio.com/products/rstudio/download-server/)poslužitelja.

> [AZURE.NOTE] Koraci u ovom dokumentu potrebna je poslužitelj za R na HDInsight klaster i neće ispravno funkcionirati ako se koriste za HDInsight klaster gdje R je bio instaliran [Instalirati akcija R skripte](hdinsight-hadoop-r-scripts-linux.md).

## <a name="prerequisites"></a>Preduvjeti

* Azure HDInsight klaster s poslužiteljem R instaliran. Upute potražite u članku [Početak rada s poslužiteljem R na klastere HDInsight](hdinsight-hadoop-r-server-get-started.md).
* Klijent za SSH. Distribucija Linux i Unix ili Macintosh OS X na `ssh` naredba navedeni su s operacijskim sustavom. Za Windows, preporučujemo da [Cygwin](http://www.redhat.com/services/custom/cygwin/) [mogućnost OpenSSH](https://www.youtube.com/watch?v=CwYSvvGaiWU)ili [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  


## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Kliknite pločicu RStudio klaster pomoću prilagođene skripte

1. Odredite čvora rub klaster. Za HDInsight klaster s poslužiteljem R, slijedi konvencije imenovanja glavni čvor i ruba čvor.

    * Lakši čvor-`CLUSTERNAME-ssh.azurehdinsight.net`
    * Čvor rub-`R-Server.CLUSTERNAME-ssh.azurehdinsight.net` 

2. SSH u čvora rub klaster pomoću iznad obrazac imenovanja. 
 
    * Ako se povezujete s Linux klijenta, potražite u članku [Povezivanje sa sustavom Linux HDInsight klaster](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).
    * Ako se povezujete iz klijenta za Windows, potražite u članku [Povezivanje sa sustavom Linux HDInsight klaster pomoću PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster).

3. Kada ste povezani, postaju korijenski korisnika na klaster. U sesiji SSH koristite sljedeću naredbu.

        sudo su -

4. Preuzmite prilagođene skripte da biste instalirali RStudio. Koristite sljedeću naredbu.

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Promijenite dozvole za datoteku za prilagođene skripte i pokrenuti skriptu. Koristite sljedeće naredbe.

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Ako ste koristili u SSH lozinku prilikom stvaranja programa HDInsight klaster s poslužiteljem R, možete preskočiti ovaj korak i prijeđite na sljedeći. Ako ste koristili ključa SSH umjesto da biste stvorili klaster, morate postaviti lozinku za korisnički SSH. Prilikom povezivanja s RStudio trebat će vam ovu lozinku. Pokrenite sljedeće naredbe. Kada se to zatraži **Kerberos trenutnu**lozinku, samo pritisnite **ENTER**.  Imajte na umu da morate zamijeniti `USERNAME` s korisnikom SSH za svoj klaster HDInsight.

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:
        
    Ako ste uspješno postavili lozinku, trebali biste vidjeti poruku ovako.

        passwd: password updated successfully


    Napuštanje sesije SSH.

7. Stvaranje programa tunelom SSH na klaster mapiranjem `localhost:8787` na klasteru HDInsight na klijentskom računalu. Morate stvoriti programa tunelom SSH prije otvaranja novu sesiju u pregledniku.

    * Na Linux client ili zatim klijenta za Windows s [Cygwin](http://www.redhat.com/services/custom/cygwin/) sesija terminal i koristite sljedeću naredbu.

            ssh -L localhost:8787:localhost:8787 USERNAME@R-Server.CLUSTERNAME-ssh.azurehdinsight.net
            
        Zamjena **korisničko ime** s korisnikom SSH za svoj klaster HDInsight i zamijeni **CLUSTERNAME** pod nazivom svoj klaster HDInsight možete koristiti ključ SSH umjesto lozinke dodavanjem`-i id_rsa_key`     

    * Ako koristite Windows klijenta s PuTTY zatim

        1.  Otvorite PuTTY pa unesite podatke za povezivanje. Ako niste upoznati s PuTTY, potražite u članku [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md) da biste saznali kako ga koristiti uz HDInsight.
        2.  U odjeljku **kategorija** na lijevoj strani dijaloškog okvira proširivanje **veza**, proširite **SSH**, a zatim odaberite **Tunnels**.
        3.  Na obrascu **Mogućnosti kontroliranje prosljeđivanje priključka SSH** navedite sljedeće podatke:

            * **Izvorni Priključak** - priključak na klijentskom računalu koju želite proslijediti. Na primjer, **8787**.
            * **Odredište** - odredište na koje je potrebno je mapirati na lokalno klijentsko računalo. Na primjer, **localhost:8787**.

            ![Stvaranje programa tunelom SSH] (./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Stvaranje programa tunelom SSH")

        4. Kliknite **Dodaj** da biste dodali postavke, a zatim kliknite **Otvori** da biste otvorili vezu s SSH.
        5. Kada se to od vas zatraži, prijavite se na poslužitelj. To će uspostaviti sesiju SSH i omogućite na tunelom.

8. Otvorite web-preglednik i unesite sljedeći URL koji se temelji na priključak uneseni u tunelom.

        http://localhost:8787/ 

9. Zatražit će se da biste unijeli SSH korisničko ime i lozinku za povezivanje s klaster. Ako ste koristili ključa SSH prilikom stvaranja klaster, morate unijeti lozinku koju ste stvorili u koraku 5.

    ![Povezivanje s R Studio] (./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Stvaranje programa tunelom SSH")

10. Da biste provjerili hoće li se instalacija RStudio nije uspjela, možete pokrenuti testiranje skriptu koja se izvršava R na temelju MapReduce i Spark klaster. Vratite se na konzolu SSH i unesite sljedeće naredbe da biste preuzeli test skriptu da biste pokrenuli u RStudio.

    * Ako ste stvorili Hadoop klaster s R, koristite sljedeću naredbu.
        
            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r

    * Ako ste stvorili Spark klaster s R, koristite sljedeću naredbu.

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

11. U RStudio, vidjet ćete skripte test koji ste preuzeli. Dvokliknite datoteku da biste je otvorili i odaberite sadržaj datoteke, a zatim kliknite **Pokreni**. Trebali biste vidjeti Izlaz u oknu **konzole** .
 
    ![Testiranje instalacije] (./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Testiranje instalacije")

Neku drugu mogućnost u kojoj se upišite `source(testhdi.r)` ili `source(testhdi_spark.r)` za izvršavanje skriptu.

## <a name="see-also"></a>Vidi također

- [Mogućnosti kontekst za poslužitelj R na klastere HDInsight za izračun](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure mogućnosti prostora za pohranu za poslužitelj R na HDInsight premium](hdinsight-hadoop-r-server-storage.md)


 
