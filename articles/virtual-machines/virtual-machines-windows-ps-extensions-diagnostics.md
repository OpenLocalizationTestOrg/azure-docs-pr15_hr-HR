<properties
    pageTitle="Pomoću komponente PowerShell da biste omogućili dijagnostika Azure u virtualnog računala sa sustavom Windows | Microsoft Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    description="Saznajte kako pomoću ljuske PowerShell da biste omogućili dijagnostika Azure u virtualnog računala sa sustavom Windows"
    authors="sbtron"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>


# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Korištenje ljuske PowerShell da biste omogućili dijagnostika Azure u virtualnog računala sa sustavom Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure Dijagnostika je mogućnost unutar Azure koja omogućuje skup dijagnostičkih podataka na distribuiranih aplikacije. Proširenje Dijagnostika možete koristiti da biste prikupili dijagnostičke podatke kao što su zapisnika aplikacije ili mjerača performansi iz programa Azure virtualnog računala (VM) sa sustavom Windows. U ovom se članku opisuje kako omogućiti nastavak Dijagnostika na VM pomoću komponente Windows PowerShell. Preduvjeti koja su potrebna za u ovom se članku potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>Omogućivanje proširenje Dijagnostika ako koristite model implementacije Voditelj resursa

Proširenje Dijagnostika možete omogućiti prilikom stvaranja Windows VM kroz model implementacije Voditelj resursa Azure dodavanjem konfiguracije proširenje predložak Voditelj resursa. Potražite u članku [Stvaranje virtualnog računala za Windows nadzora i Dijagnostika pomoću predloška Azure Voditelj resursa](virtual-machines-windows-extensions-diagnostics-template.md).

Da biste omogućili proširenje Dijagnostika na postojeće VM stvoren putem model implementacije Voditelj resursa, možete pomoću cmdleta [Skup AzureRMVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt603499.aspx) PowerShell kao što je prikazano u nastavku.


    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* je put do datoteke koja sadrži konfiguracija Dijagnostika u XML, kao što je opisano u [uzorka](#sample-diagnostics-configuration) ispod.  

Ako konfiguracijska datoteka Dijagnostika određuje **StorageAccount** element naziv računa za pohranu, skripte *Postavljanje AzureRMVMDiagnosticsExtension* će automatski postaviti proširenje Dijagnostika slanje dijagnostičkih podataka u taj račun za pohranu. To funkcionira, mora biti iste pretplate na kao na VM račun za pohranu.

Ako nema **StorageAccount** nije naveden u konfiguraciji Dijagnostika, morate proći u parametru *StorageAccountName* cmdletu. Ako parametar *StorageAccountName* nije naveden, zatim cmdlet će uvijek koristi račun za pohranu koji je naveden u parametru, a ne onaj koji je naveden u konfiguracijskoj datoteci Dijagnostika.

Ako je račun za dijagnostiku prostora za pohranu u neku drugu pretplatu na iz na VM, morate izričito prenesite parametri *StorageAccountName* i *StorageAccountKey* cmdletu. Parametar *StorageAccountKey* nije potrebna kada je račun za dijagnostiku prostora za pohranu u okviru iste pretplate kao cmdlet možete automatski upita, a zatim postavite vrijednost ključa prilikom omogućivanja proširenja za dijagnostiku. Međutim, ako je račun za dijagnostiku prostora za pohranu u neku drugu pretplatu, zatim cmdlet možda nećete moći automatski dohvatiti tipku i morate izričito navesti ključ kroz parametar *StorageAccountKey* .  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Kada proširenje Dijagnostika omogućena na na VM, možete dobiti trenutne postavke pomoću cmdleta [Get-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603678.aspx) .

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

Cmdlet vraća *PublicSettings*, koja sadrži XML konfiguraciju u obliku Base64 kodiran. Da biste pročitali XML, morate ga dekodiranje.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

Cmdlet [Ukloni AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603782.aspx) se može koristiti za nastavak Dijagnostika uklanjanje na VM.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>Omogućivanje proširenje Dijagnostika ako koristite model klasični implementacije

[Postavljanje AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) cmdlet možete koristiti da biste omogućili nastavkom Dijagnostika na VM koje stvarate kroz model klasični implementacije. Sljedeći primjer pokazuje kako stvoriti novi VM kroz model klasični implementacije s nastavkom Dijagnostika omogućena.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Da biste omogućili proširenje Dijagnostika na postojeće VM stvoren putem model klasični implementacije, najprije pomoću cmdleta [Get-AzureVM](https://msdn.microsoft.com/library/mt589152.aspx) da biste dobili VM konfiguracije. Zatim ažurirajte konfiguracija VM da uvrstite proširenje Dijagnostika pomoću cmdleta [Skup AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) . Na kraju, primijeniti ažurirani konfiguraciju u VM pomoću [Ažuriranje AzureVM](https://msdn.microsoft.com/library/mt589121.aspx).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Konfiguriranje Dijagnostika uzorka

Sljedeće XML može se koristiti za dijagnostiku javno konfiguraciju s iznad skripti. Tu konfiguraciju uzorak će prenijeti različite mjerača performansi za pohranu račun Dijagnostika, uz pogreške iz aplikacije, sigurnost i sustava kanala u zapisnika događaja sustava Windows i sve pogreške iz infrastrukture zapisnici Dijagnostika.

Konfiguracija mora se ažurirati obuhvaćaju sljedeće:

- Atribut *resourceID* elementa **metriku** mora se ažurirati Identifikator resursa za na VM.
    - ID resursa koje se mogu konstruirana pomoću sljedećeg uzorka: "/resourceGroups//pretplate / {*ID pretplate za pretplatu za na VM*} {*naziv resourcegroup na VM*} / providers/Microsoft.Compute/virtualMachines/ {*naziv VM*}".
    - Ako, na primjer, ako ID pretplate za pretplatu na kojem se pokreće na VM **11111111-1111-1111-1111-111111111111**, naziv grupe resursa za grupu resursa je **MyResourceGroup**i naziv VM je **MyWindowsVM**, zatim vrijednost *resourceID* bi biti:

        ```
        <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
        ```

    - Dodatne informacije o tome kako metriku generiraju ovisno o konfiguraciji mjerača i metrike performanse, pročitajte članak [Azure Dijagnostika metriku tablice u prostor za pohranu](virtual-machines-windows-extensions-diagnostics-template.md#wadmetrics-tables-in-storage).

- **StorageAccount** element mora se ažurirati naziv računa spremišta Dijagnostika.

    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Daljnji koraci
- Dodatne informacije o korištenju mogućnost Dijagnostika Azure i drugih tehnika za otklanjanje poteškoća potražite u članku [Omogućavanje dijagnostike Azure servise u Oblaku i virtualnih računala](../cloud-services/cloud-services-dotnet-diagnostics.md).
- [Dijagnostika konfiguracije sheme](https://msdn.microsoft.com/library/azure/mt634524.aspx) objašnjava različite XML konfiguracija mogućnosti za nastavak Dijagnostika.
