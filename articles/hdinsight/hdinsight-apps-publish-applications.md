<properties
    pageTitle="Objaviti HDInsight | Microsoft Azure"
    description="Saznajte kako stvoriti i objaviti HDInsight."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/18/2016"
    ms.author="jgao"/>

# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>Objaviti u trgovine Windows Azure HDInsight

Zatvaranje HDInsight aplikacija je koju korisnici mogu instalirati na sustavom Linux HDInsight klaster. Ti programi mogu biti razvijene od Microsofta, neovisno proizvođači (Neovisni) ili sami. U ovom se članku će Saznajte kako objaviti aplikacije u trgovine Windows Azure HDInsight.  Opće informacije o objavljivanju u trgovine Windows Azure, potražite u članku [Objavljivanje ponude Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).

HDInsight aplikacije pomoću modela *Ponijeti vaše vlastite licence (BYOL)* , pri čemu aplikacije davatelja odgovoran za licenciranje aplikacija krajnjim korisnicima, a krajnjim korisnicima su samo naplaćuje Azure za resurse stvorili, kao što su klaster HDInsight i svoje VMs/čvorove. Trenutačno naplatom za same aplikacije ne radi kroz Azure.

Druge aplikacije HDInsight povezane članka:

- [Instalacija HDInsight aplikacije](hdinsight-apps-install-applications.md): Saznajte kako instalirati aplikaciju HDInsight vaše klastere.
- [Instalacija prilagođene aplikacije HDInsight](hdinsight-apps-install-custom-applications.md): Saznajte kako instalirati i testirati prilagođenih aplikacija HDInsight.

 
## <a name="prerequisites"></a>Preduvjeti

Da biste poslali prilagođene aplikacije u trgovinu, morate imati stvara i testira prilagođenu aplikaciju. Potražite u sljedećim člancima:

- [Instalacija prilagođene aplikacije HDInsight](hdinsight-apps-install-custom-applications.md): Saznajte kako instalirati i testirati prilagođenih aplikacija HDInsight.

Morate se registrirati račun za razvojne inženjere. Potražite u članku [Objavljivanje ponude Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) i [Stvaranje računa za Microsoft za razvojne inženjere](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Definiranje aplikacije

Dva su koraka koji je uključen za objavljivanje programa Azure Marketplace.  Najprije definirate **createUiDef.json** datoteku da biste naznačili koje klastere program kompatibilan sa; a zatim objavite predložak s portala za Azure. Slijedi oglednu datoteku createUiDef.json.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


|Polje  | Opis   | Moguće vrijednosti|
|-------|---------------|----------------|
|vrste  | Vrste klaster program kompatibilan sa sustavom.    |Hadoop, HBase, oluja, Spark (ili bilo koju kombinaciju te)|
|razine  | Razine klaster program kompatibilan sa sustavom.    |Standardni, Premium (ili oboje)|
|verzija|  Vrsta klaster HDInsight program kompatibilan sa sustavom.    |3.4|

## <a name="package-application"></a>Paket aplikacije

Stvaranje zip datoteku koja sadrži sve datoteke potrebne za instalaciju aplikacije HDInsight. Trebat će vam zip datoteke u [aplikaciji za objavljivanje](#publish-application).

- [createUiDefinition.json](#define-application).
- mainTemplate.json. Uzorak pri [instalirati prilagođene aplikacije servisa HDInsight](hdinsight-apps-install-custom-applications.md)potražite u članku.

    >[AZURE.IMPORTANT] Naziv imena skriptu aplikaciju Instaliraj mora biti jedinstvena za određeni klaster s oblikovanjem u nastavku. Uz to bilo instalacija i Deinstalacija akcije skripte mora biti idempotent, što znači da se skripte možete pozvati repeatly proizvodnju isti rezultat.
    
    >   Naziv":" [Slika (' nijanse Instaliraj v0 ','-', uniquestring('applicationName')] "
        
    >Imajte na umu postoje tri dijela script naziv:
        
    >   1. Skripta naziv prefiks, koji moraju sadržavati naziv računala ili naziv odgovarajuću aplikaciju.
    >   2. A "-" zbog čitljivosti.
    >   3. Funkcija jedinstveni niz s nazivom računala kao parametar.

    >   Primjer je iznad završava gore postanete: nijanse-Instaliraj-v0-4wkahss55hlas na popisu akcija postojanog skripte. Teret za JSON uzorka, potražite u članku [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).

- Obavezni skripti.

> [AZURE.NOTE] Datoteka aplikacije (uključujući web-aplikacije datoteke ako postoji neki) može se nalaziti na bilo koji javno dostupnu krajnjoj točki.

## <a name="publish-application"></a>Objavljivanje aplikacije

Poduzmite sljedeće korake da biste objavili HDInsight aplikacije:

1. Prijavite se [portal za objavljivanje Azure](https://publish.windowsazure.com/).
2. Kliknite **Predlošci rješenja** s lijeve strane da biste stvorili novi predložak rješenja.
3. Unesite naslov, a zatim kliknite **Stvori novi predložak rješenja**.
3. Kliknite **Razvojni centar za stvaranje računa i uključiti Azure program** da biste registrirali tvrtke, ako to još niste učinili.  Potražite u članku [Stvaranje računa za Microsoft za razvojne inženjere](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
4. Kliknite **Definiranje neke topologija za početak rada**. Predložak rješenje je "parent" na sve njegove topologija. Možete definirati više topologija u jedan predložak ponuda/rješenja. Ponude je pritisak na pripremna, pritisak sa svim njegov topologija. 
4. Unesite naziv topologije, a zatim kliknite znak plus.
5. Unesite novu verziju, a zatim kliknite znak plus.
6. Prijenos datoteke zip pripremljeni u [aplikaciji paketa](#package-application).  
7. Kliknite **zahtjev certifikata**. Tim za certifikata Microsoft će pregledajte datoteke i potvrditi topologije.

## <a name="next-steps"></a>Daljnji koraci

- [Instalacija HDInsight aplikacije](hdinsight-apps-install-applications.md): Saznajte kako instalirati aplikaciju HDInsight vaše klastere.
- [Instalacija prilagođene aplikacije HDInsight](hdinsight-apps-install-custom-applications.md): uvođenje HDInsight nepotvrđen objavljene aplikacije HDInsight.
- [Prilagodba Linux temelje HDInsight klastere pomoću skripte akcije](hdinsight-hadoop-customize-cluster-linux.md): Saznajte kako koristiti akciju skriptu da biste instalirali dodatne aplikacije.
- [Utemeljen na stvaranje Linux Hadoop klaster u HDInsight pomoću predložaka Voditelj resursa Azure](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Saznajte kako poziva resursima predloške za stvaranje klastere HDInsight.
- [Korištenje prazan rub čvorove u HDInsight](hdinsight-apps-use-edge-node.md): Saznajte kako pomoću programa prazan rub čvor za pristup HDInsight klaster HDInsight aplikacije i testiranje hosting HDInsight aplikacije.

