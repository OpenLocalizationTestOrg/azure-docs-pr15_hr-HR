<properties
    pageTitle="Upravljanje VMs u skupu skaliranje virtualnog računala | Microsoft Azure"
    description="Upravljanje virtualnim strojevima u postaviti pomoću komponente PowerShell Azure skali virtualnog računala."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>Upravljanje virtualnim strojevima u skupu skaliranje virtualnog računala

Upravljajte virtualnim strojevima u vašem skupu skaliranje virtualnog računala pomoću zadataka u ovom članku.

Većinu zadataka koje obuhvaćaju upravljanje virtualnog računala u skupu skaliranje potreban je znate ID instance stroju koji želite upravljati. Možete koristiti [Azure resursa Explorer](https://resources.azure.com) da biste pronašli ID instance virtualnog računala u skupu mjerilo. Pomoću programa Explorer resursa da biste provjerili status zadataka koje ste završili.

Informacije o instalacija najnovije verzije sustava Azure PowerShell, Odabir pretplate i prijave na račun potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

## <a name="display-information-about-a-scale-set"></a>Prikaz informacija o skupu skala

Opće informacije o skupu mjerilo, koji se naziva se i kao prikaz instance možete dobiti. Ili možete doći konkretne podatke, kao što su informacije o resursima u skupu mjerilo.

Zamijeni vrijednosti Ponuđeni naziv ili grupa resursa te mjerilo postavljanje, a zatim pokrenite naredbu:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Vratit će otprilike ovako:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded
    
Ponuđeni vrijednosti zamijenite nazivom resursa grupe i skaliranje skupa. Zamjena *#* s instancu identifikator virtualnog računala koje želite pronaći informacije, a zatim ga pokrenuti:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
        
Vraća nešto kao u ovom primjeru:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded
        
## <a name="start-a-virtual-machine-in-a-scale-set"></a>Pokretanje virtualnog računala u skupu skala

Ponuđeni vrijednosti zamijenite nazivom resursa grupe i skaliranje skupa. Zamjena *#* s identifikatorom virtualnog računala koje želite pokrenuti i koristiti ga:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

U programu Explorer resursa Vidimo stanje instance je **pokrenut**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

Možete pokrenuti na virtualnim strojevima skale postaviti tako da ne koristite parametar - ID instance.
    
## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Zaustavljanje virtualnog računala u skupu skala

Ponuđeni vrijednosti zamijenite nazivom resursa grupe i skaliranje skupa. Zamjena *#* s identifikatorom virtualnog računala koje želite zaustaviti, a zatim ga pokrenuti:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

U programu Explorer resursa, ne možemo možete vidjeti je li stanje instance **deallocated**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]
    
Da biste prekinuli virtualnog računala i ne deallocate, koristite parametar - StayProvisioned. Ako ne koristite parametar - ID instance, možete isključiti sve virtualnim strojevima postavljanje.
    
## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Ponovno pokrenite virtualnog računala u skupu skala

Zamijenite nazivom grupu resursa i skup skaliranje Ponuđeni vrijednosti. Zamjena *#* s identifikatorom virtualnog računala koju želite ponovno pokrenite, a zatim ga pokrenuti:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
    
Ako ne koristite parametar - ID instance možete započeti na virtualnim strojevima u skupu.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>Uklanjanje virtualnog računala iz skupa skala

Ponuđeni vrijednosti zamijenite nazivom grupu resursa i skup mjerilo. Zamjena *#* s identifikatorom virtualnog računala koje želite ukloniti, a zatim ga pokrenuti:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Postavljanje virtualnog računala skaliranje odjednom možete ukloniti ako ne koristite parametar - ID instance.

## <a name="change-the-capacity-of-a-scale-set"></a>Promjena kapaciteta skupa skala

Možete dodati ili ukloniti virtualnim strojevima promjenom kapacitet skup. Pronađite skup skaliranje koji želite promijeniti, postavite kapacitet ono što želite da bude, a zatim ažurirajte skale postavite novu kapaciteta. U tim naredbama zamijenite vrijednosti Ponuđeni naziv grupu resursa i skup mjerilo.

  $vmss = get-AzureRmVmss - ResourceGroupName "naziv grupe resursa" - VMScaleSetName "naziv skupa skaliranje" $vmss.sku.capacity = 5 ažuriranje AzureRmVmss - ResourceGroupName "naziv grupe resursa"-naziv "skaliranje naziv skupa" - VirtualMachineScaleSet $vmss 

Uklanjate li virtualnim strojevima iz skupa mjerilo, najprije se uklanjaju virtualnim strojevima s najvećim ID-a.
