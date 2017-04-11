<properties 
    pageTitle="Otklanjanje poteškoća tvorničke Azure podataka" 
    description="Saznajte kako otkloniti poteškoće vezane uz korištenje tvorničke Azure podataka." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/31/2016" 
    ms.author="spelluru"/>

# <a name="troubleshoot-data-factory-issues"></a>Otklanjanje poteškoća tvorničke podataka
Ovaj članak sadrži savjete za otklanjanje poteškoća ima li problema prilikom korištenja tvorničke Azure podataka. U ovom se članku popis mogućih problema prilikom korištenja servisa, ali pokriva neki problemi i Savjeti za otklanjanje poteškoća na Općenito.   

## <a name="troubleshooting-tips"></a>Savjeti za otklanjanje poteškoća

### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Pogreška: Pretplate nije registriran da biste koristili polje naziva "Microsoft.DataFactory"
Ako vam se prikaže Ova pogreška, davatelja resursa tvorničke Azure podataka nije registriran na vašem računalu. Učinite sljedeće: 

1. Pokrenite Azure PowerShell. 
2. Prijavite se na račun za Azure pomoću sljedeće naredbe.
        Prijava AzureRmAccount 
3. Pokrenite sljedeću naredbu da biste registrirali davatelja tvorničke Azure podataka.
        REGISTER-AzureRmResourceProvider - ProviderNamespace Microsoft.DataFactory

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Problem: Neovlašteno pogreške prilikom izvođenja podataka tvorničke cmdleta
Vjerojatno ne koristite desno Azure poslovni subjekt ili pretplate s Azure PowerShell. Koristite sljedeće Cmdlete da biste odabrali desno Azure računa i pretplatu za uporabu Azure PowerShell. 

1. Prijava-AzureRmAccount - pomoću korisničkog ID-a i lozinke
2. Get-AzureRmSubscription – prikaz svih pretplata za račun. 
3. Odaberite AzureRmSubscription &lt;naziv pretplate&gt; -odaberite desno pretplatu. Koristi iste nešto koristite da biste stvorili podataka tvorničke portala za Azure.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Problem: Nije uspjelo pokretanje podataka postavljanje pristupnika za upravljanje Express na portal za Azure
Postavljanje Express za pristupnik za upravljanje podacima potreban je Internet Explorer ili kompatibilne web-preglednika Microsoft ClickOnce. Ako instalacijski program za Express ne pokreće, učinite nešto od sljedećeg: 

- Koristite Internet Explorer ili kompatibilne web-preglednika Microsoft ClickOnce.

    Ako koristite Chrome, otvorite [spremište web vizualnog okvira](https://chrome.google.com/webstore/)pretraživanje pomoću ključne riječi "ClickOnce", odaberite neku od proširenja ClickOnce i instalirajte ga. 
    
    Isto učinite i za Firefox (Instaliraj dodatak). Kliknite gumb Otvori izbornik na alatnoj traci (tri vodoravne crte u gornjem desnom kutu), kliknite dodaci, pretraživanje pomoću ključne riječi "ClickOnce", odaberite neku od proširenja ClickOnce i instalirajte ga. 

- Pomoću veze za **Ručno postavljanje** prikazuju na istom plohu na portalu. Ovaj pristup koristite da biste preuzeli instalacijsku datoteku i pokrenuti ručno. Nakon instalacije nije uspjela, vidjet ćete dijaloški okvir konfiguracija pristupnika za upravljanje podacima. Kopirajte **ključ** na zaslonu portala i koristiti u upravitelju konfiguracije ručno registrirajte pristupnik sa servisom.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Problem: Nije uspjelo povezivanje s lokalnog sustava SQL Server 
Pokretanje **Upravitelja konfiguracije pristupnika za upravljanje podacima** na pristupnom računalu i koristiti na kartici **Otklanjanje poteškoća** da biste testirali veze sa sustavom SQL Server na pristupnom računalu. [Problema](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) potražite u članku otklanjanje poteškoća pristupnika za savjeta za otklanjanje poteškoća veze/pristupnik probleme vezane uz.   
 

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Problem: Unos isječaka su u radu na čekanje stanja

Isječke možda je u stanju **čekanja** zbog razloge. Jedna od najčešćih razloga jest da **vanjski** svojstvo nije postavljeno na **true**. Bilo koji skup podataka prvenstveno izlaze Azure podataka tvorničke trebale bi biti označene s **vanjskim** svojstvo. Ovo svojstvo upućuje na to da podatke vanjskih i ne sigurnosne po bilo kojem kanali unutar tvorničke podataka. Isječke podataka označavaju se kao **spremni** kada podatke je dostupna u trgovini odgovarajuća. 

Pogledajte u sljedećem primjeru za korištenje **vanjskih** svojstva. Po želji možete navesti **externalData*** kada postavite vanjskih na true.

Članak [skupove podataka](data-factory-create-datasets.md) potražite u članku dodatne informacije o tom svojstvu.
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          }
        }
      }
    }

Da biste riješili problem, svojstvo **vanjskih** i u odjeljku neobavezno **externalData** dodati definiciju JSON unos tablice, a zatim ponovno stvorite tablicu. 

### <a name="problem-hybrid-copy-operation-fails"></a>Problem: Hibridnog Kopiraj ne uspije
Potražite u članku [Otklanjanje poteškoća pristupnika](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) korake za otklanjanje poteškoća s kopiranjem iz pomoću pristupnika za upravljanje podacima na lokalno spremište podataka. 

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Problem: Osvježavati HDInsight dodjele resursa ne uspije
Kada putem servisa za povezane vrste HDInsightOnDemand, morate navesti linkedServiceName koja upućuje na blobova Azure. Servis tvorničke podataka koristi ovaj prostor za pohranu za spremanje zapisnika i datoteke za podršku za svoj klaster HDInsight na zahtjev.  Ponekad dodjeljivanje od programa klaster HDInsight na zahtjev neuspješan uz sljedeću pogrešku:

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

Ta pogreška znači obično neće se nalaziti na račun za pohranu koji je naveden u na linkedServiceName na istom mjestu centar podataka kojima se događa HDInsight dodjele resursa. Primjer: Ako na tvorničke podataka u Zapad SAD a Azure prostora za pohranu u SAD-a za istočnoazijske, na zahtjev dodjele resursa neće uspjeti u Zapad SAD-a.

Uz to, postoji drugi additionalLinkedServiceNames svojstvo JSON gdje dodatni prostor za pohranu računi možda je li navedena u HDInsight na zahtjev. Te dodatni prostor za pohranu povezani računi mora biti na isto mjesto kao klaster HDInsight ili ne uspijeva uz poruku o pogrešci isti.

### <a name="problem-custom-net-activity-fails"></a>Problem: Prilagođene aktivnosti .NET ne uspije
U odjeljku [ispravljanje pogrešaka kanal s prilagođenom aktivnosti](data-factory-use-custom-activities.md#debug-the-pipeline) detaljne upute. 

## <a name="use-azure-portal-to-troubleshoot"></a>Otklanjanje poteškoća s pomoću portala za Azure 

### <a name="using-portal-blades"></a>Korištenje portala blades
Upute potražite u članku [Monitor kanal](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) . 

### <a name="using-monitor-and-manage-app"></a>Korištenje monitora i aplikacija za upravljanje
Potražite u članku [nadzor i upravljanje podacima tvorničke kanali pomoću nadzor i upravljanje aplikacije](data-factory-monitor-manage-app.md) detalje. 

## <a name="use-azure-powershell-to-troubleshoot"></a>Korištenje ljuske PowerShell Azure za rješavanje problema

### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Korištenje ljuske PowerShell Azure za otklanjanje poteškoća s pogreškom  
Detalje potražite u članku [praćenje podataka tvorničke kanali pomoću komponente PowerShell Azure](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) . 


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 