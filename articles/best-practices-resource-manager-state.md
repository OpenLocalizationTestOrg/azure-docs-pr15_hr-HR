<properties
    pageTitle="Rukovanje stanje resursima predlošci | Microsoft Azure"
    description="Prikazuje preporučuje postupke za korištenje složene objekte za zajedničko korištenje podataka o stanju s Voditelj resursa Azure predloške i povezane predloške"
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="tomfitz"/>

# <a name="sharing-state-in-azure-resource-manager-templates"></a>Zajedničko korištenje stanja u predlošcima Voditelj resursa za Azure

Ova tema prikazuje najbolje prakse za upravljanje i zajedničko korištenje stanje unutar predložaka. Parametri i varijable prikazano u nastavku su primjeri vrste objekata možete definirati na praktičan način organiziranja preduvjeti za implementaciju. U ovim se primjerima mogli implementirati vlastite objekte s vrijednosti nekretnina smislene okruženju sustava.

U ovoj se temi je dio veći koje. Da biste pročitali cijeli papira, preuzmite [svjetske klase resursa Upravitelj predložaka pitanja vezana uz i dokazano prakse](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).


## <a name="provide-standard-configuration-settings"></a>Navedite standardne konfiguracijske postavke

Umjesto nude predloška koji nudi ukupni fleksibilnost i nebrojene varijacije uobičajeni obrazac je pružanje odabira poznatih konfiguracija. Zapravo, korisnici mogu odabrati standardne veličine majica kao što su izdvojeni mali, Srednje i velike. Još primjera veličina majica su ponuda proizvoda, kao što su zajednice edition ili enterprise edition. U drugim slučajevima možda će biti specifičnih za radno opterećenje konfiguracije tehnologije – kao što su karte smanjiti ili bez sql.

Uz složene objekte, možete stvoriti varijabli koje sadrže zbirke podataka, poznatih i kao "svojstvo vrećica" i korištenje tih podataka na pogon deklariranje resursa u predlošku. Taj se način omogućuje dobro, poznati konfiguracije različitim veličinama koje su prethodno konfigurirali za korisnike. Bez poznatih konfiguracija korisnike predloška morate odrediti klaster za promjenu veličine na vlastitu, Faktor u ograničenja resursa platforme i matematičke operacije da biste odredili dobivene particija račune za pohranu i ostali resursi (zbog klaster veličina i resursa ograničenjima). Osim toga bolje iskustvo za klijenta, ćete lakše nekoliko poznatih konfiguracija podržavaju i omogućuje vam održavanje višu razinu gustoće.

Sljedeći primjer prikazuje način da biste definirali varijabli koje sadrže složene objekte za predstavljanje zbirke podataka. Zbirke definirati vrijednosti koje se koriste za veličina virtualnog računala, mrežne postavke, postavke operacijskog sustava i postavke dostupnost.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Obratite pozornost na to varijablom **tshirtSize** spaja veličine majica odvija pomoću parametar (**mala**, **Srednje** **velika**) da biste tekst **tshirtSize**. Pomoću ove varijable dohvatiti varijablu pridruženog složene objekta s majice veličine.

Ne možete referencirati tih varijabli kasnije u predlošku. Mogućnost referentni pod nazivom varijabli i njihovim svojstvima pojednostavljuje sintaksa predložak, a olakšava razumijevanje kontekst. U sljedećem primjeru definira resursa za implementaciju pomoću objekata prikazuje prethodno da biste postavili vrijednosti. Na primjer, veličina VM je postavio dohvaćanja vrijednosti za `variables('tshirtSize').vmSize` dok vrijednost za veličinu diska dohvaća iz `variables('tshirtSize').diskSize`. Osim toga, URI za predložak povezane postavljen za vrijednost za `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a>Prenesite stanja u predložak

Zajedničko korištenje stanja u predložak kroz parametre koje ste naveli izravno tijekom implementacije.

U sljedećoj su tablici navedene najčešće korištenih parametara u predlošcima.

Ime | Vrijednost | Opis
---- | ----- | -----------
mjesto    | Niz s popisa ograničenog područja za Azure   | Mjesto gdje su implementiran resurse.
storageAccountNamePrefix    | Niz    | Jedinstveni naziv DNS-a za račun za pohranu na mjesto na kojem se nalazi na VM diskova
Naziv domene  | Niz    | Naziv domene javno dostupnu jumpbox VM u obliku: **{domainName}. { mjesto}.cloudapp.com** , primjerice: **mydomainname.westus.cloudapp.azure.com**
adminUsername   | Niz    | Korisničko ime za na VMs
adminPassword   | Niz    | Lozinku za na VMs
tshirtSize  | Niz s ograničenog popisa ponuđenih veličina s majice   | Veličina jedinice imenovani mjerilo za dodjelu resursa. Na primjer, "Mali", "Srednji", "Veliko"
virtualNetworkName  | Niz    | Naziv virtualne mreže da korisnik želi koristiti.
enableJumpbox   | Niz s ograničenog popisa (omogućeno/onemogućeno) | Parametar koji određuje želite li omogućiti jumpbox za okruženje. Vrijednosti: "omogućeno", "onemogućeno"

Parametar **tshirtSize** koji se koriste u prethodnom odjeljku se definira kao:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a>Prenesite stanje povezane predložaka

Prilikom povezivanja s povezane predlošci, često koristite kombinacije statičnu i generira varijabli.

### <a name="static-variables"></a>Statički varijable

Statički varijable često se koriste za osnovne vrijednosti, kao što su URL-ove, koji se koriste u cijeloj predloška.

U sljedećim isječak predložak `templateBaseUrl` određuje mjesta korijena predloška u GitHub. U sljedećem retku sastavlja nova varijabla `sharedTemplateUrl` koji spaja osnovni URL s poznatim naziv predloška zajedničkim resursima. Ispod tom retku varijable složene objekta se koristi za pohranu veličine majica, pri čemu je osnovni URL spajaju mjesto predložaka poznati konfiguracije a pohranjene u na `vmTemplate` svojstvo.

Prednost takvog je da ako se mijenja se mjesto predložaka, samo morate promijeniti statičnog varijablom na jednom mjestu, koje prolazi kroz povezane predložaka.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>Generirani varijable

Osim statične varijable više varijabli generiraju se dinamički. U ovom se odjeljku označava neke od uobičajenih vrsta generirani varijabli.

#### <a name="tshirtsize"></a>tshirtSize

Poznajete ova generirani varijabla iz primjera iznad.

#### <a name="networksettings"></a>networkSettings

U kapaciteta, mogućnost ili predložak završetka do kraja opsegu rješenje povezane predlošci obično stvarate resursi koji postoje na mreži. Jedan jednostavne pristup jest korištenje složenih objekata za pohranu postavke mreže i prenesite povezane predložaka.

Primjer komunikacije postavke mreže mogu vidjeti ispod.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings

Resursa stvorene povezane predložaka često spremaju se u skupu dostupnost. U sljedećem primjeru je naveden naziv skupa dostupnosti, a možete zbrojiti kvara domene i ažuriranje domene da biste koristili.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Ako trebate više skupova dostupnosti (na primjer, jedan za osnovne čvorove), a drugi za čvorove podataka, možete koristiti naziv kao prefiks, navedite višestruke skupove dostupnost ili slijedite model ranije prikazane za stvaranje tjednog prikaza kalendara za određene veličine majica.

#### <a name="storagesettings"></a>storageSettings

Prostor za pohranu pojedinosti često se zajednički koriste s predlošcima za povezane. U primjeru u nastavku *storageSettings* objekt navedeni Detalji o nazive računa i spremnik za pohranu.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings

Povezane predlošcima, možda ćete morati nasuprot operacijski sustav postavke za različite vrste čvorove različitih konfiguracija poznate vrste. Složenih objekata jednostavan je način za pohranu i zajedničko korištenje informacija operacijski sustav, a i olakšava podržava više mogućnosti za operacijski sustav radi implementacije.

Sljedeći primjer prikazuje u objektu *osSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings

Generirani varijable *machineSettings* je složene objekt koji sadrži kombinacije core varijable za stvaranje na VM. Varijable obuhvaćaju korisničko ime administratora i lozinku, a zatim prefiks imena VM i referencu slika operacijski sustav.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Imajte na umu *osImageReference* dohvaća vrijednosti iz varijable *osSettings* definirano u glavnom predložak. To znači da se jednostavno možete promijeniti operacijski sustav na VM – potpuno ili na temelju preferenca potrošača predložak.

#### <a name="vmscripts"></a>vmScripts

Objekt *vmScripts* sadrži detalje o skripte da biste preuzeli i pokrenuli na instancu VM, uključujući izvan i unutar reference. Izvan reference obuhvaćaju Infrastruktura. Reference unutar obuhvaćaju instaliran softver i konfiguraciji.

Svojstvo *scriptsToDownload* koristite da biste dobili popis skripte da biste preuzeli na VM. Taj objekt sadrži i reference na Argumenti naredbenog retka za različite vrste akcije. Ove se radnje obuhvaćaju izvođenje zadane instalacije za svaki pojedinačne čvor, instalacija koja se pokreće kada sve čvorove uvode i sve dodatne skripte koji mogu biti specifična za zadani predložak.

U ovom se primjeru je predložak koji se koristi za implementaciju MongoDB koji su potrebni za Arbitar izlaganje visoke dostupnosti. Dodana *arbiterNodeInstallCommand* *vmScripts* da biste instalirali Arbitar.

U odjeljku varijable je gdje pronaći varijabli koje definiraju određeni tekst za izvršavanje skripti pomoću odgovarajuće vrijednosti.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Vraćanje stanje iz predloška

Ne samo što prođete podatke u predložak koji možete omogućiti zajedničko korištenje podataka pozivanja predložak. U odjeljku **Proizvodi** povezani predloška možete unijeti parove ključa vrijednosti koje se može trošiti predložak izvora.

Sljedeći primjer prikazuje način za prosljeđivanje privatne IP adrese generira predloška povezani.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

U glavnom predložak tih podataka možete koristiti s sljedeću sintaksu:

    "[reference('master-node').outputs.masterip.value]"

Koristite ovaj izraz u odjeljku izlaze ili sekciji resursi glavni predloška. Izraz ne možete koristiti u odjeljku varijable jer ovisi runtime stanje. Da biste vratili vrijednost iz glavnog predloška, koristite:

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }
     
Primjer korištenja odjeljka izlaze povezane predložak da biste se vratili diskova podataka za virtualnog računala, potražite u članku [Stvaranje više diskova podataka za virtualnog računala](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine).

## <a name="define-authentication-settings-for-virtual-machine"></a>Definiranje postavke provjere autentičnosti za virtualnog računala

Možete koristiti isti uzorak prikazani prethodno konfiguracijske postavke da biste odredili postavke provjere autentičnosti za virtualnog računala. Stvaranje parametra za Prenos u vrstu provjere autentičnosti.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

Dodati varijable za na različite vrste provjere autentičnosti i varijablu za spremanje koju vrstu koristi se ova implementacije na temelju vrijednosti parametra.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Pri definiranju virtualnog računala, postavite **osProfile** varijabli koje ste stvorili.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Daljnji koraci
- Da biste saznali više o sekcije predloška, potražite u članku [Stvaranje predložaka Azure Voditelj resursa](resource-group-authoring-templates.md)
- Da biste vidjeli funkcije koje se nalaze u predložak, potražite u odjeljku [Azure resursima predložak funkcije](resource-group-template-functions.md)

