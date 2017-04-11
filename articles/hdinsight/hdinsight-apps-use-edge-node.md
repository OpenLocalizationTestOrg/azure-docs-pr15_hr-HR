<properties
    pageTitle="Korištenje prazan rub čvorove u HDInsight | Microsoft Azure"
    description="Kako Dodavanje čvora rub ampty HDInsight klaster koje je moguće koristiti kao klijenta i testiranje/glavno računalo HDInsight aplikacija."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="use-empty-edge-nodes-in-hdinsight"></a>Korištenje prazan rub čvorove u HDInsight

Upute za dodavanje čvora prazan rub sustavom Linux HDInsight klaster. U prazan rub čvor je Linux virtualnog računala s klijentskih alata instalacije i konfiguracije kao na headnodes, ali ne hadoop servise. Čvor rub možete koristiti za pristup klaster klijentske aplikacije i testiranje hosting klijentske aplikacije. 

Možete dodati u prazan rub čvor postojeće HDInsight klaster, novi klaster prilikom stvaranja klaster. Dodavanje je prazan rub čvor obavlja pomoću predloška Azure Voditelj resursa.  Sljedeći primjer pokazuje kako to učiniti pomoću predloška:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "[variables('clusterApiVersion')]",
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "[parameters('edgeNodeSize')]"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Kao što je prikazano u uzorku, po želji možete nazvati na [Akcije skripte](hdinsight-hadoop-customize-cluster-linux.md) dodatnu konfiguraciju, kao što su instalacije [Apache nijanse](hdinsight-hadoop-hue-linux.md) u čvor ruba.

Nakon stvaranja na rub čvor možete povezati pomoću SSH čvor rub i pokrenuti klijentskih alata za pristup klaster Hadoop u HDInsight.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Dodavanje čvora rub postojeće klaster

U ovom ćete odjeljku pomoću predloška Voditelj resursa za dodavanje čvora rub postojeće klaster za HDInsight.  Voditelj resursa predložak nalazi se u [GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode). Voditelj resursa predložak poziva skriptu akcija skriptu koja se nalazi na https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Skripta ne izvršiti nijednu akciju.  To je da bismo pokazali pokrenuti skriptu akcija iz predloška Voditelj resursa.

**Dodavanje čvora prazan rub postojeće klaster**

1. Ako ne postoji još, stvorite je klaster HDInsight.  U odjeljku [Hadoop Praktični vodič: početak rada s operacijskim sustavom Linux Hadoop u HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Kliknite na sljedećoj slici se prijaviti Azure i otvorite predložak Azure Voditelj resursa na portalu za Azure. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Konfigurirajte sljedeća svojstva:

    - CLUSTERNAME: Unesite naziv postojeće klaster za HDInsight.
    - EDGENODESIZE: Odaberite jednu od VM veličine.
    - EDGENODEPREFIX: Zadana vrijednost je **Novi**.  Koristite zadanu vrijednost, čvor naziva rub je **Novi edgenode**.  Možete prilagoditi prefiks na portalu. Možete prilagoditi i ime i prezime iz predloška.


4. Kliknite **u redu** da biste spremili promjene.
5. U **grupi resursa**, odaberite grupu resursa.
6. Kliknite **Pregled pravne uvjete**, a zatim **za kupnju**.
7. Odaberite **Prikvači na nadzornu ploču**, a zatim kliknite **Stvori**.

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Dodavanje čvora rub prilikom stvaranja klaster

U ovom odjeljku da biste stvorili HDInsight klaster na rub čvor pomoću predloška Voditelj resursa.  Predložak Voditelj resursa moguće je pronaći u javnim [spremnik spremište blobova platforme Azure](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json). Voditelj resursa predložak poziva skriptu akcija skriptu koja se nalazi na https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Skripta ne izvršiti nijednu akciju.  To je da bismo pokazali pokrenuti skriptu akcija iz predloška Voditelj resursa.

**Dodavanje čvora prazan rub postojeće klaster**

1. Ako ne postoji još, stvorite je klaster HDInsight.  U odjeljku [Hadoop Praktični vodič: početak rada s operacijskim sustavom Linux Hadoop u HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Kliknite na sljedećoj slici se prijaviti Azure i otvorite predložak Azure Voditelj resursa na portalu za Azure. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Konfigurirajte sljedeća svojstva:
        
    - CLUSTERNAME: Unesite naziv nove klaster da biste stvorili.
    - CLUSTERLOGINUSERNAME: Unesite korisničko ime Hadoop HTTP.  Zadani naziv je **administrator**.
    - CLUSTERLOGINPASSWORD: Unesite lozinku za korisnika Hadoop HTTP.
    - SSHUSERNAME: Unesite korisničko ime SSH. Zadani naziv je **sshuser**.
    - SSHPASSWORD: Unesite lozinku za korisnika SSH.
    - LOKACIJA: Unesite mjesto naziv grupe resursa, klaster i zadani račun za pohranu.
    - CLUSTERTYPE: Zadana je vrijednost **hadoop**.
    - CLUSTERWORKERNODECOUNT: Zadana je vrijednost 2.
    - EDGENODESIZE: Odaberite jednu od VM veličine.
    - EDGENODEPREFIX: Zadana vrijednost je **Novi**.  Koristite zadanu vrijednost, čvor naziva rub je **Novi edgenode**.  Možete prilagoditi prefiks na portalu. Možete prilagoditi i ime i prezime iz predloška.

4. Kliknite **u redu** da biste spremili promjene.
5. U **grupi resursa**, unesite novi naziv za grupu resursa.
6. Kliknite **Pregled pravne uvjete**, a zatim **za kupnju**.
7. Odaberite **Prikvači na nadzornu ploču**, a zatim kliknite **Stvori**. 


## <a name="access-an-edge-node"></a>Pristup na rub čvor

Rub ssh krajnje je čvor <EdgeNodeName>. <ClusterName>-ssh.azurehdinsight.net:22.  Na primjer, nove-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Pojavit će se čvor rub kao aplikaciju na portalu Azure.  Na portalu daje informacije da biste pristupili čvor rub pomoću SSH.

**Da biste provjerili krajnjoj točki SSH čvor rub**

1. Prijavite se [portal za Azure](https://portal.azure.com).
2. Otvorite klaster HDInsight pomoću programa čvor ruba.
3. Kliknite **aplikacije** iz plohu klaster. Prikazat će čvor ruba.  Zadani naziv je **Novi edgenode**.
4. Kliknite rub čvor. Prikazat će krajnja točka SSH.

**Da biste koristili grozd na rub čvor**

5. Korištenje SSH za povezivanje s čvor ruba.  Potražite u članku [Korištenje SSH s operacijskim sustavom Linux Hadoop na HDInsight Linux, Unix, ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md) ili [koristite SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
6. Nakon što ste povezali čvor rub pomoću SSH, koristite sljedeću naredbu da biste otvorili konzolu grozd:

        hive
7. Pokrenite sljedeću naredbu da biste prikazali grozd tablice u klasteru:

        show tables;

## <a name="delete-an-edge-node"></a>Brisanje na rub čvora

Na rubu čvor možete izbrisati s portala za Azure.

**Da biste pristupili na rub čvor**

1. Prijavite se [portal za Azure](https://portal.azure.com).
2. Otvorite klaster HDInsight pomoću programa čvor ruba.
3. Kliknite **aplikacije** iz plohu klaster. Prikazat će se popis čvorove rub.  
4. Desnom tipkom miša kliknite čvor rub koji želite izbrisati, a zatim kliknite **Izbriši**.
5. Kliknite **da** da biste potvrdili.

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku ste naučili kako Dodavanje čvora rub i pristup čvor ruba. Dodatne informacije potražite u sljedećim člancima:

- [Instalacija HDInsight aplikacije](hdinsight-apps-install-applications.md): Saznajte kako instalirati aplikaciju HDInsight vaše klastere.
- [Instalacija prilagođene aplikacije HDInsight](hdinsight-apps-install-custom-applications.md): uvođenje HDInsight objavu aplikacija HDInsight.
- [Objavljivanje HDInsight aplikacije](hdinsight-apps-publish-applications.md): Saznajte kako objaviti prilagođenih aplikacija HDInsight Azure Marketplace.
- [MSDN: instalirati aplikaciju za HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saznajte kako odrediti HDInsight aplikacije.
- [Prilagodba Linux sustavom HDInsight klastere pomoću skripte akcije](hdinsight-hadoop-customize-cluster-linux.md): Saznajte kako koristiti akciju skriptu da biste instalirali dodatne aplikacije.
- [Stvaranje Linux sustavom Hadoop klaster u HDInsight pomoću predložaka Voditelj resursa](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Naučite poziva resursima predloške za stvaranje klastere HDInsight.

