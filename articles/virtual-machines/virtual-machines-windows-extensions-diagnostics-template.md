<properties
    pageTitle="Stvaranje Windows virtualnog računala nadzora i Dijagnostika pomoću predloška Azure resursima | Microsoft Azure"
    description="Pomoću upravitelja predloška Azure resursa da biste stvorili novi Windows virtualnog računala s nastavkom Azure Dijagnostika."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sbtron"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>

# <a name="create-a-windows-virtual-machine-with-monitoring-and-diagnostics-using-azure-resource-manager-template"></a>Stvaranje Windows virtualnog računala nadzora i Dijagnostika pomoću predloška Azure Voditelj resursa

Proširenje Dijagnostika Azure nudi za nadzor i Dijagnostika mogućnosti na sa sustavom Windows temelji Azure virtualnog računala. Ove mogućnosti na virtualnog računala možete omogućiti tako da uvrstite proširenje kao dio Upravitelj predloška azure resursa. Dodatne informacije o umetanju sve kućne kao dio predloška virtualnog računala potražite u članku [Za izradu predložaka resursima Azure s datotečnim nastavcima VM](virtual-machines-windows-extensions-authoring-templates.md) . U ovom se članku opisuje kako dodati proširenje Dijagnostika Azure windows predlošku virtualnog računala.  
  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>Dodavanje proširenje Azure Dijagnostika definiciju VM resursa 

Da biste omogućili proširenje Dijagnostika na Windows virtualnog računala morate dodati proširenje kao VM resursa u predlošku upravitelj resursa.

Za jednostavne upravitelj resursa na temelju virtualnog računala dodavanje konfiguracije proširenje polja *Resursi* za virtualnog računala: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Drugi uobičajeni konvencija je dodajte konfiguracije nastavak na Korijenski čvor resursi predloška umjesto definiranje u odjeljku čvor resursi virtualnog računala. Na taj se način morate izričito odredite hijerarhijskih odnosa između proširenje i virtualnog računala s vrijednostima *naziv* i *Vrsta* . Ako, na primjer: 
  
    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

Proširenje uvijek je povezan s virtualnog računala, možete izravno definirati u odjeljku čvor resursa virtualnog računala izravno ili definiranje razini osnovni i pomoću hijerarhijskih konvencija imenovanja povezivati s virtualnog računala.

Za skupove skaliranje virtualnog računala konfiguracije proširenja naveden je u svojstvu *extensionProfile* *VirtualMachineProfile*.
   
Svojstvo *izdavača* vrijednošću **Microsoft.Azure.Diagnostics** i svojstvo *Vrsta* s vrijednošću **IaaSDiagnostics** identificirati samo proširenje Dijagnostika Azure.

Vrijednost svojstva za *naziv* mogu se odnositi na datotečni nastavak u grupu resursa. Postavljanje posebno **Microsoft.Insights.VMDiagnosticsSettings** će se omogućiti da bi se mogle jednostavno prepoznati tako da na Azure klasični portala portala osiguravanje koji nadzora grafikoni prikazuju se ispravno na portalu za Azure klasični.

*TypeHandlerVersion* određuje verziju kućni broj koji želite koristiti. Postavljanje *autoUpgradeMinorVersion* radne verzije **True** osigurava da dobit ćete najnovije radne verzije kućni broj koji je dostupan. Preporučuje se postavljanje *autoUpgradeMinorVersion* da biste uvijek biti **zadovoljen** tako da se uvijek se koriste najnovije dostupne Dijagnostika nastavak novim značajkama i popravci programskih pogrešaka. 

*Postavke* element sadrži konfiguracije svojstva za proširenje koje možete postaviti i čitanje natrag s nastavkom (ponekad se nazivaju javno konfiguracije). Svojstvo *xmlcfg* sadrži xml koji se temelje konfiguracije dijagnostički zapisnici mjerača performansi druge objekte koji se prikupljaju se po agent za dijagnostiku. Dodatne informacije o xml shemu sam potražite u članku [Dijagnostika konfiguracije shemu](https://msdn.microsoft.com/library/azure/dn782207.aspx) . Uobičajeno je za pohranu konfiguracije stvarni xml kao varijabla u predlošku Voditelj resursa Azure i zatim concatenate i base64 kodiranje ih da biste postavili vrijednost za *xmlcfg*. U odjeljku [Dijagnostika konfiguracije varijable](#diagnostics-configuration-variables) znati više o tome kako spremiti xml u varijabli. Svojstvo *storageAccount* određuje naziv računa za pohranu koji će se prenijeti Dijagnostika podataka. 
 
Svojstva *protectedSettings* (ponekad se nazivaju privatne konfiguracije) moguće je postaviti, ali ne može čitati natrag nakon postavljanje. Samo za pisanje prirode *protectedSettings* olakšava korisne za pohranu tajne kao što je ključ za račun za pohranu mjesto staviti Dijagnostika podataka.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Određivanje Dijagnostika za pohranu računa kao parametri 

Na Dijagnostika proširenje json isječak iznad pretpostavlja dva parametri *existingdiagnosticsStorageAccountName* i *existingdiagnosticsStorageResourceGroup* da biste odredili račun za pohranu Dijagnostika pohrane podataka Dijagnostika. Određivanje računa za pohranu Dijagnostika kao parametar olakšava jednostavno promijeniti račun za dijagnostiku prostora za pohranu u različitim okruženjima npr trebali biste pomoću računa za pohranu različite Dijagnostika za testiranje i neku drugu za implementaciju sustava proizvodnje.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
            }
        }

Je najbolji način da biste odredili račun za pohranu Dijagnostika u grupu resursa za različite od grupa resursa za virtualnog računala. Grupa resursa možete se smatra se jedinica implementaciju s vlastitom vijek, virtualnog računala mogu uvesti i ponovno implementirati novih ažuriranja konfiguracija nisu unijete u nju, no preporučujemo vam da biste nastavili spremanje podataka za dijagnostiku u isti račun za pohranu preko tih implementacijama virtualnog računala. Nemate račun za pohranu u različitim resursa omogućuje pohranu računa da biste prihvatili podatke iz različitih virtualnog računala implementacijama jednostavno otklanjanje poteškoća s preko različitih verzija.

>[AZURE.NOTE] Ako stvorite predložak virtualnog računala za windows s Visual Studio zadani račun za pohranu možda postavljen da koristiti isti prostor za pohranu račun kojem prijenosa virtualnog računala VHD. To je da biste pojednostavnili početnog postavljanja sustava VM. Trebali biste ponovno faktor predložak tako da se pomoću računa za različite prostora za pohranu koji se mogu proslijediti u kao parametar. 

## <a name="diagnostics-configuration-variables"></a>Dijagnostika konfiguracije varijable
 
Na Dijagnostika proširenje json isječak iznad definira varijablu *accountid* da biste pojednostavnili početak ključ računa za pohranu za pohranu za dijagnostiku:   
    
    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


Svojstvo *xmlcfg* za nastavak Dijagnostika definirana je više varijabli koje se spajaju zajedno. Su vrijednosti od tih varijabli u xml tako da morate unijeti prespojni znak pravilno prilikom postavljanja json varijabli.

Sljedeće opisuje xml konfiguracije dijagnostiku koja prikuplja standardne sustava mjerača razine performansi uz neke zapisnici infrastruktura za dijagnostiku i zapisnike događaja sustava windows. To je unijeti prespojni znak i ispravno oblikovana tako da se konfiguraciju moguće zalijepiti izravno u odjeljku varijable predloška. U odjeljku [Dijagnostika konfiguracije sheme](https://msdn.microsoft.com/library/azure/dn782207.aspx) za više Ljudski čitljiv primjera xml konfiguracije.
    
        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

Xml čvor metriku definicija u konfiguraciji iznad je element važne konfiguracija definira načina pridružuje i spremanja mjerača performansi definirane ranije u xml u *PerformanceCounter* čvor. 

> [AZURE.IMPORTANT] Ove metriku pogon nadzor grafikone i upozorenja na portalu za Azure.  Čvor **metriku** s *resourceID* i **MetricAggregation** moraju biti obuhvaćene konfiguracija Dijagnostika za vaše VM ako želite vidjeti VM nadzora podataka na portalu za Azure. 

Slijedi primjer xml za metriku definicije: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

Atribut *resourceID* služi kao jedinstvena identifikacija virtualnog računala u vašoj pretplati. Provjerite jeste li funkcije subscription() i resourceGroup() tako da se predložak automatski ažurira te vrijednosti na temelju pretplatu i implementacije u grupu resursa.

Ako stvarate više virtualnim računalima petlji zatim morat ćete popunjava vrijednost *resourceID* s funkcijom copyIndex() pravilno razlikovati svake pojedine VM. Vrijednost *xmlCfg* se ažurirati podržava to na sljedeći način:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

Vrijednost MetricAggregation *PT1H* i *PT1M* označavaju prikupljene putem nekoliko minuta i prikupljene putem jednog sata.

## <a name="wadmetrics-tables-in-storage"></a>Tablica WADMetrics u prostor za pohranu

Konfiguracija metriku iznad će stvaranje tablice na vašem računu za pohranu Dijagnostika sa sljedećim konvencije imenovanja:

- **WADMetrics** : standardnim prefiksom za sve tablice WADMetrics
- **PT1H** ili **PT1M** : označava da tablica sadrži više od jednog sata ili 1 minutu prikupljanje podataka
- **P10D** : označava tablice koje će sadržavati podatke za 10 dana iz tablice pokretanja prikupljanja podataka
- **V2S** : konstanta niza
- **GGGGMMDD** : datum na kojoj se tablici rada prikupljanje podataka

Primjer: *WADMetricsPT1HP10DV2S20151108* će sadržavati metriku podataka koji se pridružuje putem čas 10 dana početka 11 riječi studeni 2015    

Svakoj tablici WADMetrics će sadržavati sljedeće stupce:

- **PartitionKey**: U partitionkey konstruirana na temelju vrijednosti *resourceID* za jedinstvenu identifikaciju VM resursa. za npr.: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
- **RowKey** : slijedi oblik <Descending time tick>:<Performance Counter Name>. Silazni izračun crtičnih vremena je crtice na osi Maks vrijeme minus vrijeme početka razdoblja za zbrajanje. Npr. Ako razdoblje uzorka rada na 10 riječi studeni 2015 i bio 00:00Hrs UTC-a, a zatim izračuna: DateTime.MaxValue.Ticks - (novi DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Crtice na osi). Za memorije dostupne bajtova performanse brojač tipku retka će izgledati kao što su: 2519551871999999999__:005CMemory:005CAvailable:0020 bajtova
- **CounterName** : naziv mjerača performansi. To odgovara *counterSpecifier* definirano u konfiguracijskoj xml.
- **Maksimalna** : najveću vrijednost mjerača performansi tijekom razdoblja za zbrajanje.
- **Minimalna** : minimalna vrijednost mjerača performansi tijekom razdoblja za zbrajanje.
- **Ukupno** : zbroja svih vrijednosti mjerača performansi prijavljenih tijekom razdoblja zbrajanja.
- **Count** : ukupan broj vrijednosti potrebnom za brojač performansi.
- **Prosječna** : average (Ukupni zbroj na broj) vrijednost mjerača performansi tijekom razdoblja zbrajanja.


## <a name="next-steps"></a>Daljnji koraci

- Gotovi ogledni predloška virtualnog računala za Windows s nastavkom Dijagnostika potražite u članku [201 vm – nadzor-Dijagnostika-proširenja](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
- Uvođenje predloška upravitelj resursa pomoću [Azure PowerShell](virtual-machines-windows-ps-manage.md) ili [Azure naredbenog retka](virtual-machines-linux-cli-deploy-templates.md)
- Dodatne informacije o [predlošcima za izradu Voditelj resursa za Azure](../resource-group-authoring-templates.md)







