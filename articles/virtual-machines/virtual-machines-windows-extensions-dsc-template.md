<properties
   pageTitle="Želite stanja konfiguracije resursa Upravitelj predložak | Microsoft Azure"
   description="Definicija predloška resursima za konfiguraciju stanje želji u Azure s primjerima i otklanjanje poteškoća"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>Windows VMSS i želji stanje konfiguracije s predlošcima Voditelj resursa za Azure
U ovom se članku opisuje predložak resursima za [Rukovatelj proširenje želji stanje konfiguracije](virtual-machines-windows-extensions-dsc-overview.md). 

## <a name="template-example-for-a-windows-vm"></a>Primjer predloška za VM za Windows

Sljedeći isječak ulazi u odjeljku resursa predloška.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }

```

## <a name="template-example-for-windows-vmss"></a>Primjer predloška za VMSS za Windows

VMSS čvor ima sekciju "Svojstva" u "VirtualMachineProfile", "extensionProfile" atribut. DSC dodaje se u odjeljku "proširenja". 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="detailed-settings-information"></a>Informacije o postavkama detaljne

Sljedeća shema je za postavke dio Azure DSC datotečni nastavak u predložak Azure Voditelj resursa.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Pojedinosti
| Naziv svojstva | Vrsta | Opis |
| --- | --- | --- |
| settings.wmfVersion | niz | Određuje verziju paketa Windows Management Framework koja je potrebno instalirati na vašem VM. Postavljanje ovo svojstvo na 'najnovije instalira najčešće ažuriranu verziju WMF. Moguće samo trenutne vrijednosti za to svojstvo su **'4.0', '5.0', ' 5.0PP' i 'najnoviji'**. Ove mogućim vrijednostima podliježe ažuriranja. Zadana je vrijednost "najnovije".|
| Settings.Configuration.URL | niz | Određuje mjesto na URL iz koje će se preuzeti DSC konfiguracijskoj datoteci zip. Ako navedeni URL za pristup zahtijeva SAS tokena, morate je svojstvo protectedSettings.configurationUrlSasToken postavite na vrijednost token za SAS. Ako je definiran settings.configuration.script i/ili settings.configuration.function potreban je to svojstvo. |
| Settings.Configuration.Script | niz | Određuje naziv datoteke skriptu koja sadrži definiciju DSC konfiguracije. Ova skripta mora biti u korijenskoj mapi zip datoteke preuzete iz određen svojstvo configuration.url URL-a. Ako je definiran settings.configuration.url i/ili settings.configuration.script potreban je to svojstvo. |
| Settings.Configuration.Function | niz | Određuje naziv DSC konfiguracije. Konfiguriranje pod nazivom moraju se nalaziti u skripti definira configuration.script. Ako je definiran settings.configuration.url i/ili settings.configuration.function potreban je to svojstvo. |
| settings.configurationArguments | Zbirka | Definira parametre želite proslijediti DSC konfiguracije. Ovo svojstvo je šifriran. |
| settings.configurationData.url | niz | Određuje URL iz koje će se preuzeti konfiguracijskoj datoteci podataka (.pds1) da biste koristili kao unos konfiguraciju DSC. Ako navedeni URL za pristup zahtijeva SAS tokena, morate je svojstvo protectedSettings.configurationDataUrlSasToken postavite na vrijednost token za SAS.|
| settings.privacy.dataEnabled | niz | Omogućuje ili onemogućuje telemetrijskih zbirke. Samo moguće vrijednosti za to svojstvo su **'Omogući', "Onemogući", ", ili $null**. Ostavite ovo svojstvo prazna ni null omogućuje telemetrijskih. Zadana vrijednost je ". [Dodatne informacije](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings | Zbirka | Definira alternativnih mjesta iz kojih da biste preuzeli na WMF. [Dodatne informacije](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments | Zbirka | Definira parametre želite proslijediti DSC konfiguracije. Ovo svojstvo je šifriran. |
| protectedSettings.configurationUrlSasToken | niz | Određuje token SAS da biste pristupili definira configuration.url URL-a. Ovo svojstvo je šifriran. |
| protectedSettings.configurationDataUrlSasToken | niz | Određuje token SAS da biste pristupili definira configurationData.url URL-a. Ovo svojstvo je šifriran. |

## <a name="settings-vs-protectedsettings"></a>Postavke nasuprot ProtectedSettings
Sve postavke spremaju se u tekstnoj datoteci postavke na na VM.
Svojstva u odjeljku "postavke" su javno svojstva jer nije šifrirana u tekstnoj datoteci postavke.
Svojstva u odjeljku "protectedSettings" šifrirane pomoću certifikata i ne prikazuju se u običan tekst u ovoj datoteci na na VM.

Ako su konfiguraciju potrebne vjerodajnice, možete biti uvrštene u protectedSettings:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Primjer

U sljedećem primjeru rezultat u odjeljku "Prvi koraci" [DSC proširenje rukovatelj implicitno](virtual-machines-windows-extensions-dsc-overview.md).
Ovaj primjer koristi predloške resursima umjesto cmdleta za implementaciju datotečni nastavak. Spremanje konfiguracije "IisInstall.ps1", postavite ga na. Poštanski broj datoteka, a zatim prijenos datoteke u pristupiti URL-u. U ovom se primjeru koristi spremište blobova platforme Azure, ali je moguće preuzeti. ZIP datoteke s bilo kojeg mjesta proizvoljne.

U predlošku Voditelj resursa Azure sljedeći kod upućuje VM da biste preuzeli ispravnu datoteku i pokrenite odgovarajuće funkcije PowerShell:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Ažuriranja u starijem obliku zapisa
Sve postavke u u starijem obliku zapisa (koji sadrži javno svojstva ModulesUrl, ConfigurationFunction, SasToken ili svojstva) automatski prilagoditi u trenutni oblik i pokrenite baš kao i prije.

Sljedeća shema je što prethodne postavke sheme bile:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Evo kako prilagođavati im starijem obliku zapisa u trenutni oblik:

| Naziv svojstva | Prethodni ekvivalent sheme |
| --- | --- |
| settings.wmfVersion | postavke. WMFVersion |
| Settings.Configuration.URL | postavke. ModulesUrl |
| Settings.Configuration.Script | Prvi dio postavke. ConfigurationFunction (prije '\\\\") |
| Settings.Configuration.Function | Drugi dio postavke. ConfigurationFunction (nakon '\\\\") |
| settings.configurationArguments | postavke. Svojstva |
| settings.configurationData.url | protectedSettings.DataBlobUri (bez SAS token) |
| settings.privacy.dataEnabled | postavke. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings | postavke. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments | protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken | postavke. SasToken |
| protectedSettings.configurationDataUrlSasToken | SAS tokena iz protectedSettings.DataBlobUri |


## <a name="troubleshooting---error-code-1100"></a>Otklanjanje poteškoća – Šifra pogreške 1100
Kod pogreške 1100 znači da postoji problem s korisničkom unosu u proširenja DSC.
Tekst te pogreške je varijabla i može se promijeniti.
Slijede pogrešaka može naići i kako ih popravljate.

### <a name="invalid-values"></a>Vrijednosti koje nisu valjane
"Privacy.dataCollection je {0}. Samo moguće vrijednosti su ","Omogući"i 'Onemogući'" "WmfVersion je {0}. Moguće vrijednosti su... i 'najnovije' "

Problem: Navedeni vrijednost nije dopuštena.

Rješenje: Promijenite vrijednost koja nije valjana u valjana vrijednost. Pogledajte tablicu u odjeljku Detalji.

### <a name="invalid-url"></a>URL koji nije valjan
"ConfigurationData.url je {0}. Ovo nije valjana URL""DataBlobUri je {0}. Ovo nije valjana URL""Configuration.url je {0}. Ovo nije valjana URL"

Problem: A navedeni su URL-a nije valjan.

Rješenje: Provjerite sve navedene URL. Provjerite jesu li sve URL-ove raščlanjuje valjana mjesta proširenja pristupite na udaljenom računalu.

### <a name="invalid-configurationargument-type"></a>Vrsta ConfigurationArgument nije valjana
"Nije valjan configurationArguments upišite {0}"

Problem: Svojstvo ConfigurationArguments ne može odrediti Hashtable objekt. 

Rješenje: Provjerite svoje ConfigurationArguments svojstvo tablicu raspršivanja. Slijedite navedene u ovom primjeru prethodnoj oblik. Pogledajte ponuda, zareze i zagrade.

### <a name="duplicate-configurationarguments"></a>Dupliciranje ConfigurationArguments
"Nije pronađena duplicirane argumente"{0}"u javno i zaštićenim configurationArguments"

Problem: ConfigurationArguments u javnom postavke i ConfigurationArguments u postavki zaštićenog sadrže neka svojstva s istim nazivom.

Rješenje: Poništili duplicirane svojstva.

### <a name="missing-properties"></a>Svojstva koja nedostaju
"Configuration.function potreban je navedena configuration.url ili configuration.module"

"Configuration.url potreban je navedena je taj configuration.script"

"Configuration.script potreban je navedena je taj configuration.url"

"Configuration.url potreban je navedena je taj configuration.function"

"ConfigurationUrlSasToken potreban je navedena je taj configuration.url"

"ConfigurationDataUrlSasToken potreban je navedena je taj configurationData.url"

Problem: Definirani svojstvo mora drugi svojstava koja nedostaje.

Rješenja: 
- Navedite nedostaje svojstvo.
- Uklonite svojstvo koje treba nedostaje svojstvo.


## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o DSC i skaliranje virtualnog računala postavlja [Pomoću virtualnog računala skaliranje postavlja s nastavkom DSC Azure](../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Dodatne detalje potražite na [Upravljanje vjerodajnica za sigurno DSC korisnika](virtual-machines-windows-extensions-dsc-credentials.md). 

Dodatne informacije o događajima proširenje Azure DSC potražite u članku [Uvod u proširenje rukovatelj Azure želji stanje konfiguracije](virtual-machines-windows-extensions-dsc-overview.md). 

Dodatne informacije o DSC PowerShell, [Posjetite centar za dokumentaciju PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 
