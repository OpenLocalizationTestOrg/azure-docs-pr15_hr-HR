<properties
    pageTitle="Automatsko skaliranje Windows virtualnog računala skaliranje postavlja | Microsoft Azure"
    description="Postavljanje autoscaling za Windows virtualnog računala skaliranje skup pomoću komponente PowerShell Azure"
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

# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Automatski promijeniti omjer strojeva u skupu skaliranje virtualnog računala

Skupovi skaliranje virtualnog računala olakšavaju uvođenje i upravljanje identične virtualnim strojevima u skupu. Skaliranje skupove omogućavaju sloj iznimno skalabilni i prilagoditi računalnim hyperscale aplikacije, a oni podržavaju slika platformu sustava Windows, Linux platformu slike, prilagođene slike i proširenja. Dodatne informacije o skupovima skaliranje potražite u članku [Postavlja skaliranje virtualnog računala](virtual-machine-scale-sets-overview.md).

Pomoću ovog praktičnog vodiča prikazuje kako stvoriti skaliranje skup virtualnim strojevima sa sustavom Windows i automatski promijeniti omjer strojeva u skupu. Stvaranje skale i postavljene skaliranja stvaranjem predložak Voditelj resursa Azure i implementacija ga pomoću komponente PowerShell Azure. Dodatne informacije o predlošcima potražite u članku [Predlošci Voditelj resursa za Azure autorizaciju](../resource-group-authoring-templates.md). Da biste saznali više o automatsko skaliranje skupova mjerilo, potražite u članku [Automatsko skaliranje i postavlja skaliranje virtualnog računala](virtual-machine-scale-sets-autoscale-overview.md).

U ovom se članku implementacija sljedeće resurse i nastavaka:

- Microsoft.Storage/storageAccounts
- Microsoft.Network/virtualNetworks
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/loadBalancers
- Microsoft.Network/networkInterfaces
- Microsoft.Compute/virtualMachines
- Microsoft.Compute/virtualMachineScaleSets
- Microsoft.Insights.VMDiagnosticsSettings
- Microsoft.Insights/autoscaleSettings

Dodatne informacije o resursima resursima potražite u članku [izračunati Azure, mreža i davatelja usluga za pohranu u odjeljku upravitelj resursa Azure](../virtual-machines/virtual-machines-windows-compare-deployment-models.md).

## <a name="step-1-install-azure-powershell"></a>Korak 1: Instalacija Azure komponente PowerShell

Kako [instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) informacije o instaliranju najnovije verziji Azure PowerShell, odaberete pretplatu, i prva prijava u Azure.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Korak 2: Stvaranje grupa resursa i račun za pohranu

1. **Stvaranje grupe resursa** – resursi za sve mora biti implementirano u grupu resursa. Da biste stvorili grupu resursa pod nazivom **vmsstestrg1**koristiti [Novo AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) .

2. **Stvaranje računa za pohranu** – taj račun za pohranu se pohranjuju predložak. Stvaranje računa za pohranu pod nazivom **vmsstestsa**koristiti [Novo AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) .

## <a name="step-3-create-the-template"></a>Korak 3: Stvaranje predloška
Predložak programa Azure Voditelj resursa omogućuje uvođenje i upravljanje Azure resursi zajedno pomoću opis JSON resurse i pridruženi implementacije parametara.

1. U uređivač omiljene stvoriti datoteku C:\VMSSTemplate.json i početne JSON strukturu za podršku predložak.

        {
          "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
          ]
        }

2. Parametri uvijek nisu potrebni, no oni omogućuju unos vrijednosti kad je implementiran u predložak. Dodavanje parametara u odjeljku nadređeni element parametre koje ste dodali u predložak.

        "vmName": { "type": "string" },
        "vmSSName": { "type": "string" },
        "instanceCount": { "type": "string" },
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" },
        "resourcePrefix": { "type": "string" }

    - Postavite naziv zasebnom virtualnog računala koja se koristi za pristup strojeva u skale.
    - Naziv računa spremišta gdje je predložak pohranjen.
    - Postavite broj virtualnih računala da biste u skale prethodno stvorili.
    - Ime i lozinku za administratorski račun na virtualnim računalima.
    - Postavite prefiks naziv resursa koje su stvorene za podršku skalu.

3. Varijable može se koristiti u predlošku da biste odredili vrijednosti koje se često mijenjaju ili vrijednosti koje je potrebno stvoriti od kombinacije vrijednosti parametara. Dodajte ove varijable u odjeljku nadređeni element varijabli koje ste dodali u predložak.

        "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
        "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
        "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
        "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
        "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
        "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
        "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
        "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
        "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
          "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
        "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

  - DNS nazive koji koriste sučelje mreže.
    - IP adresa nazivi i prefiksi virtualne mreže i podmreže.
    - Nazivi i identifikatore virtualne mreže učitati opterećenja i sučelje mreže.
    - Postavljanje naziva računa za pohranu za račune koji je povezan s računala u skale.
    - Postavke za proširenje Dijagnostika koji je instaliran na virtualnim računalima. Dodatne informacije o proširenje Dijagnostika potražite u članku [Stvaranje Windows virtualnog računala nadzora i Dijagnostika pomoću predloška Azure Voditelj resursa](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).

4. Dodajte račun resursa za pohranu pod resurse nadređeni element koji ste dodali u predložak. Ovaj predložak koristi petlja za stvaranje preporučene pet računa za pohranu gdje se pohranjuju diskova operacijski sustav i dijagnostičkih podataka. Ovaj skup poslovnih subjekata podržavaju do 100 virtualnim strojevima u skupu skaliranje koja je trenutno maksimum. Svaki račun za pohranu pod nazivom s pokazivačem slovo koje je određeno u varijable u kombinaciji s prefiksom koje unesete u parametre za predložak.

        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. Dodajte virtualne mrežni resurs. Dodatne informacije potražite u članku [Davatelja resursa mreže](../virtual-network/resource-groups-networking.md).

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "subnet1",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },

6. Dodajte javni resursi adresa IP koje koriste opterećenja i sučelje mreže.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP1')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName1')]"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP2')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName2')]"
            }
          }
        },

7. Dodavanje resursa raspoređivača opterećenja koji koristi skaliranje skupa. Dodatne informacije potražite u članku [Azure Voditelj resursa podrške za raspoređivača opterećenja](../load-balancer/load-balancer-arm.md).

        {
          "apiVersion": "2015-06-15",
          "name": "[variables('loadBalancerName')]",
          "type": "Microsoft.Network/loadBalancers",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
          ],
          "properties": {
            "frontendIPConfigurations": [
              {
                "name": "loadBalancerFrontEnd",
                "properties": {
                  "publicIPAddress": {
                    "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
                  }
                }
              }
            ],
            "backendAddressPools": [ { "name": "bepool1" } ],
            "inboundNatPools": [
              {
                "name": "natpool1",
                "properties": {
                  "frontendIPConfiguration": {
                    "id": "[variables('frontEndIPConfigID')]"
                  },
                  "protocol": "tcp",
                  "frontendPortRangeStart": 50000,
                  "frontendPortRangeEnd": 50500,
                  "backendPort": 3389
                }
              }
            ]
          }
        },

8. Dodajte mrežnom resursu sučelja koji se koristi zasebnom virtualnog računala. Jer strojeva u skupu skaliranje nije moguće pristupiti putem javne IP adrese, odvojene virtualnog računala stvara se u istu virtualne mreže za daljinski pristup strojeva.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[variables('nicName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
            "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
                  },
                  "subnet": {
                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                  }
                }
              }
            ]
          }
        },

9. Dodajte zasebnom virtualnog računala u istoj mreži kao skup mjerilo.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[parameters('vmName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "storageLoop",
            "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_A1" },
            "osProfile": {
              "computername": "[parameters('vmName')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "[concat(parameters('resourcePrefix'), 'os1')]",
                "vhd": {
                  "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"        
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
                }
              ]
            }
          }
        },

10. Dodavanje skale virtualnog računala postavljanje resursa i navedite nastavak Dijagnostika koji je instaliran na sve virtualnim strojevima u skupu mjerilo. Mnoge od postavki za ovaj resurs su slične resursa virtualnog računala. Glavne razlike su element kapaciteta koji navodi broj virtualnim strojevima u skup mjerilo, a upgradePolicy koja navodi kako se ažuriranja nastaju na virtualnim računalima. Postavljanje skaliranje nije stvoren dok se stvaraju se svi računi za pohranu kao što je određeno elementom dependsOn.

            {
              "type": "Microsoft.Compute/virtualMachineScaleSets",
              "apiVersion": "2016-03-30",
              "name": "[parameters('vmSSName')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "storageLoop",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
              ],
              "sku": {
                "name": "Standard_A1",
                "tier": "Standard",
                "capacity": "[parameters('instanceCount')]"
              },
              "properties": {
                "upgradePolicy": {
                  "mode": "Manual"
                },
                "virtualMachineProfile": {
                  "storageProfile": {
                    "osDisk": {
                      "vhdContainers": [
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
                      ],
                      "name": "vmssosdisk",
                      "caching": "ReadOnly",
                      "createOption": "FromImage"
                    },
                    "imageReference": {
                      "publisher": "MicrosoftWindowsServer",
                      "offer": "WindowsServer",
                      "sku": "2012-R2-Datacenter",
                      "version": "latest"
                    }
                  },
                  "osProfile": {
                    "computerNamePrefix": "[parameters('vmSSName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                  },
                  "networkProfile": {
                    "networkInterfaceConfigurations": [
                      {
                        "name": "networkconfig1",
                        "properties": {
                          "primary": "true",
                          "ipConfigurations": [
                            {
                              "name": "ip1",
                              "properties": {
                                "subnet": {
                                  "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                                },
                                "loadBalancerBackendAddressPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                                  }
                                ],
                                "loadBalancerInboundNatPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                                  }
                                ]
                              }
                            }
                          ]
                        }
                      }
                    ]
                  },
                  "extensionProfile": {
                    "extensions": [
                      {
                        "name": "Microsoft.Insights.VMDiagnosticsSettings",
                        "properties": {
                          "publisher": "Microsoft.Azure.Diagnostics",
                          "type": "IaaSDiagnostics",
                          "typeHandlerVersion": "1.5",
                          "autoUpgradeMinorVersion": true,
                          "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                            "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                          },
                          "protectedSettings": {
                            "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                          }
                        }
                      }
                    ]
                  }
                }
              }
            },

11. Dodavanje resursa autoscaleSettings koja definira kako prilagođava ovisno o korištenju procesor na računalima u skupu skaliranje skup mjerilo.

            {
              "type": "Microsoft.Insights/autoscaleSettings",
              "apiVersion": "2015-04-01",
              "name": "[concat(parameters('resourcePrefix'),'as1')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              ],
              "properties": {
                "enabled": true,
                "name": "[concat(parameters('resourcePrefix'),'as1')]",
                "profiles": [
                  {
                    "name": "Profile1",
                    "capacity": {
                      "minimum": "1",
                      "maximum": "10",
                      "default": "1"
                    },
                    "rules": [
                      {
                        "metricTrigger": {
                          "metricName": "\\Processor(_Total)\\% Processor Time",
                          "metricNamespace": "",
                          "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                          "timeGrain": "PT1M",
                          "statistic": "Average",
                          "timeWindow": "PT5M",
                          "timeAggregation": "Average",
                          "operator": "GreaterThan",
                          "threshold": 50.0
                        },
                        "scaleAction": {
                          "direction": "Increase",
                          "type": "ChangeCount",
                          "value": "1",
                          "cooldown": "PT5M"
                        }
                      }
                    ]
                  }
                ],
                "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              }
            }

    Za ovaj vodič su važne ove vrijednosti:

    - **metricName** - tu vrijednost je ista kao brojač performanse koji ste definirali u wadperfcounter varijabli. Koristite tu varijablu, proširenje Dijagnostika prikuplja u **Processor(_Total)\% procesor vrijeme** brojač.
    - **metricResourceUri** - tu vrijednost je identifikator resursa skupa skaliranje virtualnog računala.
    - **timeGrain** – u ovom je vrijednost preciznosti metrike koji se prikupljaju. U ovom predlošku je postavljen na jednu minutu.
    - **statistike** – tu vrijednost određuje kako se spajaju metriku kako bi odgovarao Automatska akcija skaliranja. Moguće vrijednosti su: prosjek, minimum, maksimum. U ovom predlošku koji se prikupljaju average ukupni procesora korištenje virtualnih računala.
    - **timeWindow** – tu vrijednost je raspon vrijeme u kojem prikupljaju se podaci instance. Mora biti između pet minuta i 12 sati.
    - **timeAggregation** – tu vrijednost određuje kako treba podatke koji se prikupljaju se kombinirati s vremenom. Zadana vrijednost je prosjek. Moguće vrijednosti su: prosjek, Minimum, maksimum, posljednji, ukupni zbroj, broj.
    - **operator** – tu vrijednost je operator koji se koristi za usporedbu metričke podatke i praga. Moguće vrijednosti su: jednako NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    - **prag** – u ovom je vrijednost koja se pokreće akcija mjerilo. U ovom predlošku strojeva dodaju skale postavljanje kada je prosječna procesora među strojeva u postavljanje veće od 50%.
    - **smjer** – tu vrijednost određuje akciju koja se koristi se kada se postiže vrijednosti praga. Moguće vrijednosti su povećajte ili smanjite. U ovom predlošku broj virtualnim strojevima u skupu skaliranje povećava se ako je prag veće od 50%, u prozoru definirani vremena.
    - **Vrsta** – tu vrijednost je vrsta akcije koje bi se trebale provoditi i mora biti postavljeno na ChangeCount.
    - **vrijednost** – tu vrijednost je broj virtualnim strojevima koji su dodani ili uklonjeni iz skupa mjerilo. Ta vrijednost mora biti 1 ili noviji. Zadana vrijednost je 1. U ovom predlošku broj računalima skale postavio povećava 1 kada se ispuni praga.
    - **cooldown** – tu vrijednost je vrijeme čekanja nakon zadnje akcije skaliranja prije no što se pojavljuje na sljedeću akciju. Ta vrijednost mora biti između jednu minutu i jedan tjedan.

12. Spremite datoteku predloška.    

## <a name="step-4-upload-the-template-to-storage"></a>Korak 4: Prijenos predloška za pohranu

Predložak se možete prenijeti pod uvjetom da znate naziv i primarni ključ računa za pohranu koji ste stvorili u koraku 1.

1.  U prozoru Microsoft Azure PowerShell postavite varijabla koji određuje naziv računa za pohranu koji ste stvorili u koraku 1.

            $storageAccountName = "vmstestsa"

2.  Postavljanje varijable koja određuje primarni ključ računa za pohranu.

            $storageAccountKey = "<primary-account-key>"

    Taj ključ možete dobiti klikom na ikonu ključa prilikom prikaza računa resursa za pohranu na portalu za Azure.

3.  Stvaranje objekta kontekst za pohranu računa koji se koristi za provjeru valjanosti operacije s računom za pohranu.

            $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

4.  Stvaranje kontejnera za spremanje predloška.

            $containerName = "templates"
            New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob

5.  Prijenos datoteke predloška u novi spremnik.

            $blobName = "VMSSTemplate.json"
            $fileName = "C:\" + $BlobName
            Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx

## <a name="step-5-deploy-the-template"></a>Korak 5: Uvođenje predloška

Sad kad ste stvorili predložak, možete početi implementacija resurse. Da biste pokrenuli postupak, koristite sljedeću naredbu:

    New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"

Kada pritisnete tipku unesite, morat ćete unijeti vrijednosti za varijable ste dodijelili. Unesite sljedeće vrijednosti:

    vmName: vmsstestvm1
      vmSSName: vmsstest1
      instanceCount: 5
      adminUserName: vmadmin1
      adminPassword: VMpass1
      resourcePrefix: vmsstest

Traje otprilike 15 minuta za sve resurse uspješno uvesti.

>[AZURE.NOTE] Mogućnost portal sustava možete koristiti i za implementaciju resurse. Pomoću ove veze: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"

## <a name="step-6-monitor-resources"></a>Koraku 6: Resursi za Monitor

Prikazat će vam neke informacije o skupovima mjerilo za virtualnog računala pomoću ovih metoda:

 - Portal za Azure – trenutno možete dobiti tijekom ograničenog vremenskog podataka pomoću portala za.
 - [Azure resursa Explorer](https://resources.azure.com/) – ovaj alat je najbolje za istraživanje trenutno stanje skupa mjerilo. Slijedite ovaj put i trebali biste vidjeti prikaz instancu skupa skaliranje koji ste stvorili:

        subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

 - Azure PowerShell – koristite sljedeću naredbu da biste dobili neke informacije:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

        Or

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView

 - Povežite se s zasebnom virtualnog računala kao što bi bilo kojeg uređaja, a zatim daljinski pristupate virtualnim strojevima u skale postavljanje praćenje pojedinačnih procesa.

>[AZURE.NOTE] Na Dovršeno REST API-JA za nabavljanje informacije o skupovima skaliranje pronaći ćete u [Postavlja skaliranje virtualnog računala](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>7 korak: Uklanjanje resursa

Budući da vam se naplatiti za resurse koji se koriste u Azure, uvijek je dobro da biste izbrisali resursa koje više nisu potrebne. Ne morate zasebno brisanje svakog resursa iz grupe resursa. Možete izbrisati grupu resursa i njegovih resursa se automatski brišu.

    Remove-AzureRmResourceGroup -Name vmsstestrg1

Ako želite zadržati grupu resursa možete izbrisati skale postavite samo.

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"

## <a name="next-steps"></a>Daljnji koraci

- Upravljanje skaliranje postavljanje koji ste upravo stvorili pomoću informacija u [Upravljanje virtualnim strojevima u skupu na skaliranje virtualnog računala](virtual-machine-scale-sets-windows-manage.md).
- Dodatne informacije o okomito skaliranje tako da pročitate [Okomiti automatsko skaliranje sa skupovima skaliranje virtualnog računala](virtual-machine-scale-sets-vertical-scale-reprovision.md)
- Pronalaženje Primjeri Azure Monitor nadzor značajke u [Azure Monitor PowerShell Brzi start uzorka](../monitoring-and-diagnostics/insights-powershell-samples.md)
- Informirajte se o značajkama obavijesti u [Korištenje automatsko skaliranje akcija za slanje e-pošte i webhook upozorenja u Azure monitora](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
- Saznajte kako [zapisnike nadzora za korištenje za slanje e-pošte i webhook upozorenja monitoru Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
