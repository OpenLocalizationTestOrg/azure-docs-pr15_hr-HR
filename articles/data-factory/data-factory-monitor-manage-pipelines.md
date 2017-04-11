<properties 
    pageTitle="Nadzor i upravljanje kanali tvorničke Azure podataka" 
    description="Saznajte kako pomoću portala za Azure i Azure PowerShell za nadzor i upravljanje factories Azure podataka i kanali koje ste stvorili." 
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
    ms.date="09/06/2016" 
    ms.author="spelluru"/>


# <a name="monitor-and-manage-azure-data-factory-pipelines"></a>Nadzor i upravljanje kanali tvorničke Azure podataka
> [AZURE.SELECTOR]
- [Korištenje Azure portal/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
- [Korištenje nadzor i upravljanje aplikacije](data-factory-monitor-manage-app.md)

Servis tvorničke podataka omogućuje prikaz pouzdanog i potpuni premještanje servisa za pohranu, obrada i podataka. Servis nudi nadzor pomaže nadzorne ploče koje možete koristiti za izvođenje sljedećih zadataka: 

- Brzo procijenite stanja kanal završetka do kraja podataka.
- Prepoznavanje problema i poduzmite akciju ispravljanja prema potrebi. 
- Praćenje podataka o podrijetlu. 
- Praćenje odnosa između preko svih izvora podataka.
- Prikaz punog povijesne računovodstveni izvođenja posla, općem stanju sustava i ovisnosti.

U ovom se članku opisuje kako nadzor, upravljanje i vaše kanali za ispravljanje pogrešaka. Također nudi informacije o stvaranju upozorenja i primanje obavijesti o pogreške.

## <a name="understand-pipelines-and-activity-states"></a>Razumijevanje kanali i aktivnosti Američke Države
Pomoću portala za Azure, možete učiniti sljedeće:

- Prikaz na tvorničke podataka kao dijagram
- Prikaz aktivnosti u u kanalu
- Prikaz ulazni i izlazni skupova podataka
- itd. 

Ovo poglavlje sadrži i kako se isječak prijelaze iz jednog stanja u drugom stanje.   

### <a name="navigate-to-your-data-factory"></a>Idite na tvorničke podataka
1.  Prijavite se na [portal za Azure](https://portal.azure.com).
2.  Na izborniku na lijevoj strani kliknite **factories podataka** . Ako ne vidite ga, kliknite **dodatnih servisa >** pa kliknite **factories podataka** **INTELIGENCIJE + ANALIZE** kategorije. 

    ![Pregled svih -> factories podataka](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)

    Trebali biste vidjeti sve podatke factories plohu **factories podataka** . 
4. U factories plohu podataka odaberite tvorničke podataka koji vas zanima.

    ![Odaberite tvorničke podataka](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)  
5.  i trebali biste vidjeti na početnu stranicu (**podataka tvorničke** plohu) tvorničke podataka.

    ![Plohu tvorničke podataka](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Prikaz dijagrama na tvorničke podataka
Dijagram prikaza podataka tvorničke sadrži jedan oknu staklenom za nadzor i upravljanje tvorničke podataka i njegov resursi.

Da biste vidjeli u prikazu dijagrama na tvorničke podataka, kliknite **dijagrama** na tvorničke podataka na početnoj stranici.

![Prikaz dijagrama](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Povećavanje, Smanjivanje, zumiranje Prilagodi, Zumiranje na 100%, zaključavanje izgleda dijagrama i automatski položaj kanali i tablice. Možete vidjeti i podaci o podrijetlu podataka (Prikaži upstream i do stavke iz odabrane stavke).
 

### <a name="activities-inside-a-pipeline"></a>Aktivnosti unutar na kanal 
1. Desnom tipkom miša kliknite kanal, a zatim kliknite **Otvori kanala** da biste vidjeli sve aktivnosti u kanalu uz ulazni i izlazni skupova podataka za aktivnosti. Ova značajka je korisna kada vaš kanal sastoji od više od jedne aktivnosti i želite da biste se informirali o podrijetlu radu jedan kanala.

    ![Izbornik za otvaranje kanal](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)  
2. U sljedećem primjeru vidite dvije aktivnosti u kanalu s unosa i izlaza. Aktivnosti pod naslovom **JoinData** vrste aktivnosti vrste Hive HDInsight i **EgressDataAzure** vrste aktivnosti Kopiraj su u ovom kanalu uzorka. 
    
    ![Aktivnosti unutar na kanal](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png) 
3. Možete se kretati natrag na tvorničke podataka početne stranice tako da kliknete vezu tvorničke podataka u navigaciji u gornjem lijevom kutu.

    ![Vratite se na tvorničke podataka](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-state-of-each-activity-inside-a-pipeline"></a>Prikaz stanja svake aktivnosti unutar na kanal
Trenutno stanje aktivnosti možete vidjeti tako da prikaz statusa svih skupova podataka koje je stvorio aktivnost. 

Na primjer: u sljedećem primjeru **BlobPartitionHiveActivity** pokrenuli uspješno i proizvodi pod nazivom **PartitionedProductsUsageTable**, koja je u stanju **spremno** skup podataka.

![Stanje kanala](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

Dvoklikom **PartitionedProductsUsageTable** u prikazu dijagrama showcases sve isječke koje je stvorio drugi aktivnosti pokreće unutar na kanal. Možete vidjeti je li **BlobPartitionHiveActivity** uspješno svakog mjeseca za zadnji osam mjeseca i proizvesti isječaka u stanju **spremno** .

Skup podataka isječaka na tvorničke podataka može imati jedan od sljedećih statusa:

<table>
<tr>
    <th align="left">Stanje</th><th align="left">Substate</th><th align="left">Opis</th>
</tr>
<tr>
    <td rowspan="8">Čekanje</td><td>ScheduleTime</td><td>Vrijeme je ne dolaze za isječkom da biste pokrenuli.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Upstream ovisnosti su spremni.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Resursi računalnim nisu dostupni.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Sve instance aktivnosti zauzeti izvode drugi isječaka.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Aktivnost je pauziran i isječke ne može se pokrenuti sve dok ga se nastaviti.</td>
</tr>
<tr>
<td>Pokušajte ponovno</td><td>Izvršavanje aktivnosti se ponoviti.</td>
</tr>
<tr>
<td>Provjera valjanosti</td><td>Provjera valjanosti još nije započelo.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Čekanje valjanosti za povlačenje.</td>
</tr>
<tr>
<tr
<td rowspan="2">U tijeku</td><td>Provjera valjanosti</td><td>Provjera valjanosti u tijeku.</td>
</tr>
<td></td>
<td>Isječak obrađuje.</td>
</tr>
<tr>
<td rowspan="4">Nije uspjela</td><td>Prekoračili vremensko ograničenje</td><td>Izvršavanje traje dulje nego je koji je dopušteno mjerodavnim aktivnosti.</td>
</tr>
<tr>
<td>Otkazano</td><td>Otkazao korisničku akciju.</td>
</tr>
<tr>
<td>Provjera valjanosti</td><td>Provjera nije uspjela.</td>
</tr>
<tr>
<td></td><td>Generiranje i/ili provjeriti isječak nije uspjelo.</td>
</tr>
<td>Jeste li spremni</td><td></td><td>Isječak je spremna za potrošnju.</td>
</tr>
<tr>
<td>Preskočeno</td><td></td><td>Isječak nije obrađen.</td>
</tr>
<tr>
<td>Ništa</td><td></td><td>Isječak koji koristi postoje uz neki drugi status, ali vraćena.</td>
</tr>
</table>



Klikom na stavku isječak u plohu **Nedavno ažurirati isječke** možete vidjeti detalje o isječak.

![Detalji o isječkom](./media/data-factory-monitor-manage-pipelines/slice-details.png)
 
Ako isječak izvrši više puta, vidjet ćete više redaka na popisu **aktivnosti izvodi** . Možete pogledati detalje o aktivnosti klikom na Pokreni stavke na popisu **aktivnosti izvodi** . Na popisu prikazuje sve datoteke zapisnika uz poruku o pogrešci ako ih ima. Ova značajka je korisna za pregled i ispravljanje pogrešaka zapisnika bez potrebe za ostavite na tvorničke podataka.

![Aktivnosti pokrenuti pojedinosti](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Ako isječak nije **spreman** stanje, vidjet ćete upstream isječci koji su spremni onemogućuju trenutni isječak izvršavaju na popisu **Upstream isječaka koje su nije spreman** . Ova značajka je korisna kada vaš odsječak je u stanju **čekanja** i želite da biste shvatili upstream ovisnosti čeka isječak.

![Nije spreman upstream isječaka](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>Stanje DataSet dijagrama
Nakon implementacije podataka tvorničke, a na kanali imate valjan aktivni razdoblje, skupu podataka presijeca prijelaza s jednog na drugi. Trenutno stanje isječak slijedi sljedeći dijagram stanja:

![Stanja dijagrama](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

Stanje tijek prijelaza za skup podataka u tvorničke podataka: čekanje -> u-tijeku/u tijeku (Validating) -> spremna/nije uspio

Isječke pokrenuti u stanju **čekanja** za stara uvjete da se zadovolje prije izvršavanja. Nakon toga aktivnost pokreće izvršavanja i isječak prelazi u stanje **U tijeku** . Izvršavanje aktivnosti može biti uspješan ili neće uspjeti. Isječak nosi **spreman**"ili **nije uspjelo** na temelju rezultata izvršavanja. 

Možete vratiti isječak da biste se vratili iz **spreman** ili **nije uspjelo** stanja **čekanja** stanje. Možete označiti u isječak stanja **Preskoči**, što onemogućuje izvršavanje aktivnosti i obraditi isječak.


## <a name="manage-pipelines"></a>Upravljanje kanali
Možete upravljati vaše kanali pomoću komponente PowerShell Azure. Na primjer, možete zaustaviti i nastaviti kanali tako da pokrenete cmdleta ljuske PowerShell Azure. 

### <a name="pause-and-resume-pipelines"></a>Zaustavljanje i nastavak kanali
Možete pauze/obustaviti kanali pomoću cmdleta ljuske Powershell **Suspend AzureRmDataFactoryPipeline** . Ovaj cmdlet je korisno kada ne želite pokrenuti na kanali dok je problem riješen.

Na primjer: na sljedećem zaslonu snimka sadrži su otkrili problem s **PartitionProductsUsagePipeline** u tvorničke **productrecgamalbox1dev** podataka i želimo odgoditi kanal.

![Kanal za biti obustavljeno](./media/data-factory-monitor-manage-pipelines/pipeline-to-be-suspended.png)

Da biste privremeno obustavljanje na kanal, pokrenite sljedeću naredbu komponente PowerShell:

    Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

Ako, na primjer:

    Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 

Kada je problem riješen s **PartitionProductsUsagePipeline**, obustavljenom kanal može se nastaviti ponovnim pokretanjem sljedeće naredbe ljuske PowerShell:

    Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

Ako, na primjer:

    Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 


## <a name="debug-pipelines"></a>Kanali za ispravljanje pogrešaka
Azure podataka tvorničke pruža obogaćenog mogućnosti putem Azure ljuske PowerShell za ispravljanje pogrešaka i otklanjanje poteškoća s kanali i Azure portal.

### <a name="find-errors-in-a-pipeline"></a>Pronalaženje pogrešaka u u kanalu
Pokretanje aktivnosti ne uspije u na kanalu, skup podataka koje je stvorio kanal je u stanju pogreške zbog pogreške. Možete ispraviti pogreške i otklanjanje poteškoća u Azure podataka tvorničke pomoću sljedećih Mehanizmi.

#### <a name="use-azure-portal-to-debug-an-error"></a>Koristite Azure portal za ispravljanje pogrešaka pogreška:

3.  U plohu **TABLICE** kliknite isječak problem čiji je **STATUS** postavljen na **nije uspjelo**.

    ![Tablica plohu s isječkom problem](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
4.  U plohu **ISJEČAK podataka** kliknite aktivnosti koja se nije uspjela pokrenuti.
    
    ![Dataslice poruku o pogrešci](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
5.  U plohu **Pojedinosti pokretanje aktivnosti** možete preuzeti datoteke povezane s obradom HDInsight. Kliknite Preuzmi za Status/stderr da biste preuzeli datoteke zapisnika o pogreškama koji sadrži detalje o pogrešci.

    ![Aktivnosti pokrenuti pojedinosti plohu uz poruku o pogrešci](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)  

#### <a name="use-the-powershell-to-debug-an-error"></a>Korištenje ljuske PowerShell za ispravljanje pogrešaka pogreške
1.  Pokrenite **Azure PowerShell**.
3.  Naredba Pokreni **Get-AzureRmDataFactorySlice** da biste vidjeli isječke i njihovih statusa. Trebali biste vidjeti isječak sa statusom: **nije uspjelo**.       

            Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    
    Ako, na primjer:
        
            Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00

    Zamijenite vrijednost StartDateTime koje ste naveli za postavljanje AzureRmDataFactoryPipelineActivePeriod **StartDateTime** .
4. Pokrenite cmdlet **Get-AzureRmDataFactoryRun** da biste vidjeli detalje o aktivnosti pokrenuti isječak.

        Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] 
        <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    
    Ako, na primjer:

        Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"

    Vrijednost StartDateTime je vrijeme početka isječak pogreške/problem ste zabilježili u prethodnom koraku. Datum-vrijeme mora biti zatvoren unutar dvostrukih navodnika.
5.  Trebali biste vidjeti izlaz s pojedinostima o pogrešci (sličnu ovoj):

            Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
            ResourceGroupName       : ADF
            DataFactoryName         : LogProcessingFactory3
            TableName               : EnrichedGameEventsTable
            ProcessingStartTime     : 10/10/2014 3:04:52 AM
            ProcessingEndTime       : 10/10/2014 3:06:49 AM
            PercentComplete         : 0
            DataSliceStart          : 5/5/2014 12:00:00 AM
            DataSliceEnd            : 5/6/2014 12:00:00 AM
            Status                  : FailedExecution
            Timestamp               : 10/10/2014 3:04:52 AM
            RetryAttempt            : 0
            Properties              : {}
            ErrorMessage            : Pig script failed with exit code '5'. See wasb://     adfjobs@spestore.blob.core.windows.net/PigQuery
                                            Jobs/841b77c9-d56c-48d1-99a3-
                        8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                        more details.
            ActivityName            : PigEnrichLogs
            PipelineName            : EnrichGameLogsPipeline
            Type                    :
    
    
6.  **Spremanje AzureRmDataFactoryLog** cmdlet možete koristiti s potražite u članku iz izlaza i preuzimanje datoteka zapisnika pomoću **-DownloadLogsoption** za cmdlet vrijednost za ID-a.

            Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"


## <a name="rerun-failures-in-a-pipeline"></a>Ponovno pokrenite pogrešaka u u kanalu

### <a name="using-azure-portal"></a>Pomoću portala za Azure

Nakon što rješavanje problema i pogrešaka u na kanalu za ispravljanje pogrešaka, ponovno pokrenite neuspjeha odsječak pogreške a klikom na gumb **Pokreni** na naredbenoj traci.

![Ponovno pokrenite program nije uspio isječkom](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

U slučaju da isječak nije uspjela provjera valjanosti zbog pogreške pravila (za ex: podaci nisu dostupni), možete ispraviti pogreške i ponovno provjeriti tako da kliknete gumb **Provjeri valjanost** na naredbenoj traci.
![Otklanjanje pogrešaka prilikom i provjeru valjanosti](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="using-azure-powershell"></a>Pomoću Azure komponente PowerShell

Pomoću cmdleta skup AzureRmDataFactorySliceStatus možete ponovno pokrenite pogreške. Potražite u temi [Postavljanje AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) sintaksu i druge detalje o cmdletu. 

**Primjer:** Sljedeći primjer postavlja status sve isječke za tablicu 'DAWikiAggregatedData' "čekaju' u tvorničke Azure podataka 'WikiADF'.

Na UpdateType postavljen na UpstreamInPipeline, što znači da statusi svaki isječak za tablicu i sve tablice zavisne (upstream) postavljene na "Čekanje". Ostale moguću vrijednost za taj parametar nije "Osobe".

    Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -TableName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00


## <a name="create-alerts"></a>Stvaranje upozorenja
Azure zapisnika korisnika događaji kada Azure resursa (na primjer, tvorničke podataka) je stvoriti, ažurirati ili izbrisati. Upozorenja možete stvoriti na sljedeće događaje. Tvorničke podataka omogućuje vam da biste zabilježili različite metriku i stvaranje upozorenja na određene parametre. Preporučujemo da koristite događaje u stvarnom vremenu za nadzor i metriku povijesnih svrhe. 

### <a name="alerts-on-events"></a>Upozorenja o događajima
Azure događaja navedite korisne uvida u što se događa u Azure resurse. Azure zapisnika korisnika događaji kada Azure resursa (na primjer, tvorničke podataka) je stvoriti, ažurirati ili izbrisati. Prilikom korištenja podataka tvorničke Azure, događaji generiraju kada:

- Azure tvorničke podataka je stvorena ili ažurirati/izbrisati.
- Obrada podataka (naziva se kao pokreće) rada/dovrši.
- Programa klaster HDInsight na zahtjev je stvorio i ukloniti.

Možete stvoriti upozorenja o događajima ove korisnika i konfigurirati ih za slanje obavijesti e-poštom administrator i dodatnih administratora pretplate. Osim toga, možete odrediti adrese e-pošte dodatnim korisnicima da biste primali obavijesti e-poštom kada su uvjeti zadovoljeni. Ova značajka je korisna kada želite dobiti obavijest na pogreške, a ne želite stalno praćenje tvorničke vaše podatke.

> [AZURE.NOTE] Trenutno portalu ne prikazuj upozorenja o događajima. Koristite [nadzor i upravljanje aplikacije](data-factory-monitor-manage-app.md) da biste vidjeli sva upozorenja.

#### <a name="specifying-an-alert-definition"></a>Određivanje definiciju upozorenja:
Da biste odredili upozorenja definicija, stvarate datoteku JSON s opisom operacije koje želite biti obaviješteni na. U sljedećem primjeru upozorenje šalje obavijest e-pošte za operaciju RunFinished. Da biste se određene, se obavijest e-poštom šalje kada se dovrši izvođenje na tvorničke podataka, a zatim Pokreni nije uspjela (Status = FailedExecution).

    {
        "contentVersion": "1.0.0.0",
         "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters": {},
        "resources": 
        [
            {
                "name": "ADFAlertsSlice",
                "type": "microsoft.insights/alertrules",
                "apiVersion": "2014-04-01",
                "location": "East US",
                "properties": 
                {
                    "name": "ADFAlertsSlice",
                    "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
                    "isEnabled": true,
                    "condition": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                        "dataSource": 
                        {
                            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                            "operationName": "RunFinished",
                            "status": "Failed",
                            "subStatus": "FailedExecution"   
                        }
                    },
                    "action": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails": [ "<your alias>@contoso.com" ]
                    }
                }
            }
        ]
    }

Iz definicije JSON **subStatus** moguće ukloniti ako želite biti obaviješteni određene pogreške.

U ovom se primjeru postavlja upozorenja za sve factories podataka u svoju pretplatu. Ako želite da se upozorenje da je postavljena za konkretnu podatkovnu tvorničke, možete odrediti podataka tvorničke **resourceUri** u **izvor podataka**:

    "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"

Sljedeća tablica sadrži popis dostupnih operacije i statusi (i podređene statusi).

Naziv operacije | Status | Sub status
-------------- | ------ | ----------
RunStarted | Koraci | Pokretanje
RunFinished | Nije uspjelo / uspjela | FailedResourceAllocation<br/><br/>Uspješna<br/><br/>FailedExecution<br/><br/>Prekoračili vremensko ograničenje<br/><br/>< otkazana<br/><br/>FailedValidation<br/><br/>Odbačeni
OnDemandClusterCreateStarted | Koraci
OnDemandClusterCreateSuccessful | Uspješna
OnDemandClusterDeleted | Uspješna

Detalje o JSON elemente koji se koriste u primjeru potražite u članku [Stvaranje upozorenja pravila](https://msdn.microsoft.com/library/azure/dn510366.aspx) . 

#### <a name="deploying-the-alert"></a>Implementacija upozorenje 
Da biste implementirali upozorenja, koristite cmdlet Azure PowerShell: **Novo AzureRmResourceGroupDeployment**, kao što je prikazano u sljedećem primjeru:

    New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  

Nakon implementacije grupu resursa je uspješno dovršena, prikazati sljedeće poruke:

    VERBOSE: 7:00:48 PM - Template is valid.
    WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
    Please update scripts to remove this parameter.
    VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
    VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :

> [AZURE.NOTE] [Stvaranje pravila upozorenje](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API-JA možete koristiti da biste stvorili pravilo upozorenja. Opseg JSON je slično kao primjer JSON.  

#### <a name="retrieving-the-list-of-azure-resource-group-deployments"></a>Dohvaćanje popisa implementacijama grupu resursa za Azure
Da biste dohvatili popis distribuiranih implementacijama Azure grupa resursa, koristite cmdlet: **Get-AzureRmResourceGroupDeployment**, kao što je prikazano u sljedećem primjeru:

    Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :


#### <a name="troubleshooting-user-events"></a>Otklanjanje poteškoća s događaje korisnika


1. Vidjet ćete sve događaje koje generira nakon klika na pločicu **metriku i operacije** .

    ![Pločica metriku i operacije](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)

2. Kliknite pločicu **događaja** da biste pogledali događaje. 

    ![Pločica događaja](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. U plohu **događaja** možete vidjeti detalje o događajima, filtriranje događaja i tako dalje. 

    ![Plohu događaja](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. Kliknite **operacija** na popisu operacija koja uzrokuje pogrešku.
    
    ![Odaberite operaciju](./media/data-factory-monitor-manage-pipelines/select-operation.png) 
5. Kliknite događaj **pogreške** da biste vidjeli detalje o pogrešci.

    ![Pogreška događaja](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)
  

Potražite u članku [Cmdleti za Azure uvid](https://msdn.microsoft.com/library/mt282452.aspx) članak namijenjen cmdleta ljuske PowerShell koju koristite za dodavanje i dobivanje/uklanjanje upozorenja. Evo nekoliko primjera pomoću cmdleta **Get-AlertRule** : 


    PS C:\> get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
        
            Properties :
            Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
            Condition   :
            DataSource :
            EventName             :
            Category              :
            Level                 :
            OperationName         : RunFinished
            ResourceGroupName     :
            ResourceProviderName  :
            ResourceId            :
            Status                : Failed
            SubStatus             : FailedExecution
            Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
            Condition   :
            Description : One or more of the data slices for the Azure Data Factory has failed processing.
            Status      : Enabled
            Name:       : ADFAlertsSlice
            Tags       :
            $type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
            Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
            Location   : West US
            Name       : ADFAlertsSlice
    
    PS C:\> Get-AlertRule -res $resourceGroup

            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
            Location   : West US
            Name       : FailedExecutionRunsWest0
    
            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
            Location   : West US
            Name       : FailedExecutionRunsWest3

    PS C:\> Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
    
            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
            Location   : West US
            Name       : FailedExecutionRunsWest0

Pokrenite sljedeće naredbe potražite pomoć da biste vidjeli detalje i primjeri za cmdlet Get-AlertRule. 

    get-help Get-AlertRule -detailed 
    get-help Get-AlertRule -examples


- Ako vidite upozorenja generiranje događaja na portalu plohu, ali ne primate obavijesti e-poštom, provjerite je li adresa e-pošte naveden je postavljeno na primanje e-poštu od vanjskih pošiljatelja. Upozorenja e-poštu možda je blokirao postavke e-pošte.

### <a name="alerts-on-metrics"></a>Upozorenja na mjerenja
Tvorničke podataka omogućuje vam da biste zabilježili različite metriku i stvaranje upozorenja na određene parametre. Možete nadzirati te stvaranje upozorenja na sljedeće metriku isječke u tvorničke vaše podatke.
 
- Nije uspjelo pokreće
- Uspješan pokreće

Ove metriku korisne su i omogućuju vam da biste dobili pregled cjelokupnog nije uspjelo i uspješno pokrenuti u tvorničke svoje podatke. Metriku su čuje svaki put kada je isječak Pokreni. Pri vrhu sat, te metriku pridružuje i pritisak na račun servisa za pohranu. Dakle, da biste omogućili metriku, postavljanje računa za pohranu.


#### <a name="enabling-metrics"></a>Omogućivanje metriku:
Da biste omogućili metriku, kliknite sljedeće iz plohu tvorničke podataka:

**Nadzor** -> **metriku** -> **dijagnostičkih postavki** -> **dijagnostičkih**

![Veza za dijagnostiku](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

**Dijagnostičke** plohu **na** kliknite i odaberite račun za pohranu i spremiti.

![Dijagnostika plohu](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

Nakon spremanja, može proći do jedan sat za metriku budu vidljive na nadzora plohu jer metriku zbrajanja događa zaračunava.


### <a name="setting-up-alert-on-metrics"></a>Postavljanje upozorenja na metriku:

Kliknite plohu **metriku tvorničke podataka** : 

![Pločica metriku tvorničke podataka](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

Na plohu **metrika** kliknite **+ Dodaj upozorenje** na alatnoj traci. 
![Plohu metričkim tvorničke podataka – dodavanje upozorenja](./media/data-factory-monitor-manage-pipelines/add-alert.png)

Na stranici **Dodavanje upozorenja pravila** pomoću sljedećih koraka, a zatim kliknite **u redu**.
 
- Unesite naziv upozorenje (primjer: nije uspjelo upozorenje).
- Unesite opis upozorenja (primjer: pošaljite poruku e-pošte kada dođe do pogreške).
- Odaberite metriku (nije uspjelo pokreću nasuprot uspješno pokrenuti).
- Određivanje uvjeta i vrijednosti praga.   
- Navedite razdoblje. 
- Odredite hoće li se poruke e-pošte moraju se poslati u vlasnici, suradnici i čitači.
- itd. 

![Plohu metričkim tvorničke podataka – dodavanje upozorenja](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

Kada upozorenja pravilo uspješno dodate, na plohu zatvara i na stranici **metrika** u odjeljku Novo upozorenje. 

![Plohu metričkim tvorničke podataka – dodavanje upozorenja](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

Trebali biste vidjeti i broj upozorenja na pločici **upozorenja** . Kliknite pločicu **upozorenja** .

![Podaci tvorničke metričkim plohu - upozorenja pravila](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

U plohu **upozorenja** vidjeti sve postojeće upozorenja. Da biste dodali upozorenja, na alatnoj traci kliknite **Dodaj upozorenje** .

![Plohu upozorenja pravila](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a>Upozorenja:
Kada upozorenja pravilo zadovoljava uvjet, dobivate poruku upozorenja aktivirani e-pošte. Kada se problem ne riješi, a zatim upozorenja uvjet ne odgovara više, prikazat će se upozorenje riješi e-pošte.

Takvo ponašanje razlikuje se od događajima kojima se šalje obavijest na svaku pogrešku koju upozorenja pravilo preduvjete.

### <a name="deploying-alerts-using-powershell"></a>Implementacija upozorenja pomoću komponente PowerShell
Možete implementirati upozorenja za metriku na isti način kao i za događaje. 

**Definicija upozorenja:**

    {
        "contentVersion" : "1.0.0.0",
        "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters" : {},
        "resources" : [
        {
                "name" : "FailedRunsGreaterThan5",
                "type" : "microsoft.insights/alertrules",
                "apiVersion" : "2014-04-01",
                "location" : "East US",
                "properties" : {
                    "name" : "FailedRunsGreaterThan5",
                    "description" : "Failed Runs greater than 5",
                    "isEnabled" : true,
                    "condition" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                        "dataSource" : {
                            "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                            "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                            "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
    >/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                            "metricName" : "FailedRuns"
                        },
                        "threshold" : 5.0,
                        "windowSize" : "PT3H",
                        "timeAggregation" : "Total"
                    },
                    "action" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails" : ["abhinav.gpt@live.com"]
                    }
                }
            }
        ]
    }
 
Zamijenite subscriptionId, resourceGroupName i dataFactoryName u uzorku odgovarajuće vrijednosti.

*metricName* od sada podržava dvije vrijednosti:
- FailedRuns
- SuccessfulRuns

**Implementacija Upozorenje:**

Da biste implementirali upozorenja, koristite cmdlet Azure PowerShell: **Novo AzureRmResourceGroupDeployment**, kao što je prikazano u sljedećem primjeru:

    New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json

Trebali biste vidjeti praćenja poruke nakon uspješne implementacije:

    VERBOSE: 12:52:47 PM - Template is valid.
    VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
    VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded
    
    
    DeploymentName    : FailedRunsGreaterThan5
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 7/27/2015 7:52:56 PM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           


Cmdlet za **Dodavanje AlertRule** možete koristiti i za implementaciju upozorenja pravilo. [Dodavanje AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) sintaksi detalje i primjeri.  

## <a name="move-data-factory-to-a-different-resource-group-or-subscription"></a>Premještanje podataka tvorničke resursa za različite grupe ili pretplatu
Grupa različitih resursa ili neku drugu pretplatu možete premjestiti podataka tvorničke pomoću **Premještanje** naredba vezana uz na početnoj stranici na tvorničke podataka. 

![Premještanje podataka tvorničke](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Zajedno s tvorničke podataka možete premjestiti i sve povezane resurse (primjerice upozorenja pridružene tvorničke podataka).

![Premještanje minigrafikone na vrpci](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
