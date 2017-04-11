<properties
    pageTitle="Stvaranje Hadoop, HBase ili oluja klastere na Linux u HDInsight pomoću zakretanja i Azure REST API-JA | Microsoft Azure"
    description="Saznajte kako stvoriti klastere sustavom Linux HDInsight pomoću zakretanja, predlošci Voditelj resursa Azure i Azure REST API-JA. Određivanje vrste klaster (Hadoop, HBase ili oluja) ili koristite skripte da biste instalirali prilagođene komponente."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>Stvaranje sustavom Linux klastere u HDInsight pomoću zakretanja i Azure REST API-JA

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure REST API-JA omogućuje izvođenje operacija management Services smješten u platforme Azure, uključujući stvaranje nove resurse kao što su klastere sustavom Linux HDInsight. U ovom dokumentu će Saznajte kako stvoriti Voditelj resursa Azure predloške za konfiguriranje HDInsight klaster i pridruženi prostora za pohranu, a zatim pomoću zakretanja uvođenje predloška Azure REST API-JA da biste stvorili novi klaster HDInsight.

> [AZURE.IMPORTANT] Koraci u ovom dokumentu pomoću zadanog broja radnih čvorove (4) za programa klaster HDInsight. Ako planirate na više od 32 tempiranja čvorove na klaster stvaranja ili tako da skaliranje skupine nakon stvaranja, morate odabrati glavni čvor veličina s najmanje 8 jezgri i 14GB RAM-a.
>
> Dodatne informacije o čvor veličine i pridruženi troškova potražite u članku [HDInsight cijene](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Preduvjeti

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Azure EŽA__. Azure EŽA se koristi za stvaranje servisa glavnica, koji se zatim koristi za generiranje tokeni za provjeru autentičnosti zahtjeva za Azure REST API-JA.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __cURL__. Uslužni je dostupan putem upravljanja sustavom paketa ili mogu se preuzeti sa [http://curl.haxx.se/](http://curl.haxx.se/).

    > [AZURE.NOTE] Ako koristite PowerShell za izvođenje naredbe u ovom dokumentu, najprije morate ukloniti na `curl` pseudonim koji stvara prema zadanim postavkama. Pseudonima koristi pozovite-WebRequest, cmdlet ljuske PowerShell, umjesto zakretanja kada koristite na `curl` naredbu iz odzivniku komponente PowerShell sustava, a vratit će pogreške za mnoge naredbi koje se koriste u ovom dokumentu.
    > 
    > Da biste uklonili pseudonima, koristite sljedeće iz odzivniku komponente PowerShell:
    >
    > `Remove-item alias:curl`
    >
    > Kada pseudonim uklonjena, ćete moći koristiti verziju zakretanja koje ste instalirali na računalu.

### <a name="access-control-requirements"></a>Preduvjeti za kontrolu pristupa

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-a-template"></a>Stvaranje predloška

Azure Upravljanje resursima Predlošci su JSON dokumente koje opisuju __grupu resursa__ i sve resursa u njemu (primjerice HDInsight.) Taj se način predložak koji se temelji omogućuje definiranje resursa koje su vam potrebne za HDInsight u jedan predložak i upravljanje promjene u grupu cijela kroz __implementacijama__ Primijeni promjene na grupi.

Predlošci obično navedene na dva dijela; sam predložak, a parametara datoteku koja popuniti vrijednosti određene konfiguracije. Za exmaple, naziv klaster, administrator ime i lozinku. Koristite li izravno REST API-JA, morate ih u jednu datoteku kombinirati. Oblik dokumenta JSON je:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

Ako je, primjerice, slijedi spajanja datoteka predloška i parametre iz [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), čime se sustavom Linux klaster pomoću lozinku radi zaštite SSH korisnički račun.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

U ovom se primjeru koristit će se u koracima u ovom dokumentu. Vrijednosti koje želite koristiti za svoj klaster morate zamijeniti rezervirano mjesto _vrijednosti_ u odjeljku __parametara__ na kraju dokumenta.

##<a name="login-to-your-azure-subscription"></a>Prijava u pretplatu za Azure

Slijedite korake u [pretplatu na Azure s Azure sučelja naredbenog retka (Azure EŽA) za povezivanje](../xplat-cli-connect.md) i povezivanje s pretplate pomoću na `azure login` naredbe.

##<a name="create-a-service-principal"></a>Stvorite glavni servisa

> [AZURE.NOTE] Korake u nastavku su abridged verziju informacija navedenih u odjeljku _servis za provjeru autentičnosti glavni lozinkom - Azure EŽA_ dokumenta [provjere autentičnosti glavni servis pomoću upravitelja za Azure resursa](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli) . Ove korake stvaranje novog servisa glavni koja se može koristiti za provjeru autentičnosti zahtjeva REST API-JA za stvaranje Azure resurse kao što je klaster HDInsight.

1. Iz naredbenog retka terminal sesija ili ljuske, koristite sljedeću naredbu da biste dobili popis pretplate Azure.

        azure account list
        
    Na popisu odaberite pretplatu u koju želite koristiti i zabilježite stupac __ID-a__ . Ovo je __ID pretplate__ koja će se koristiti u većini koraka u ovom dokumentu.

2. Stvorite novu aplikaciju u Azure Active Directory.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    Zamjena vrijednosti u `--name`, `--home-page`, a `--identifier-uris` pomoću vlastitih vrijednosti. Upišite lozinku za novu stavku servisa Active Directory.
    
    > [AZURE.NOTE] Budući da stvarate ovu aplikaciju za provjeru autentičnosti putem servisa glavni, u `--home-page` i `--identifier-uris` vrijednosti ne morate referencu stvarni web-stranice na Internetu; samo moraju biti jedinstveni ji.
    
    Na vraćenih podataka, spremiti vrijednost __ID programa__ .
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. Stvorite glavni pomoću __ID programa__ vrijednost vraća prethodno uslugu.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     Na vraćenih podataka, spremiti vrijednost __Id objekta__ .
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. Dodijeli ulogu __vlasnik__ servisa glavni s vrijednošću __ID objekta__ vraća prethodno. Morate koristiti i __ID pretplate__ koji ste nabavili neke starije verzije.
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    Kada je ta naredba dovrši, servis glavnog sada ima pristup vlasnik navedena pretplata ID.

##<a name="get-an-authentication-token"></a>Početak token za provjeru autentičnosti

1. Koristite sljedeće da biste pronašli __ID klijenta__ za vašu pretplatu.

        azure account show -s <subscription ID>
        
    Na vraćenih podataka pronaći __ID klijenta__.
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. Generiranje novi token pomoću Azure REST API-JA.

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    Zamijenite vrijednosti ili prethodno korišten __TenantID__, __ID programa__i __lozinku__ .

    Ako je zahtjev uspije, dobit ćete odgovor 200 niza i tijelo odgovor će sadržavati JSON dokumenta.

    Dokument JSON vratio zahtjev će sadržavati element pod nazivom __access_token__; vrijednost taj element je token za pristup morate koristiti za provjeru autentičnosti zahtjeva koji se koristi u sljedećim odjeljcima ovog dokumenta.
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##<a name="create-a-resource-group"></a>Stvaranje grupa resursa

Koristite sljedeće da biste stvorili novu grupu resursa. Potrebno je stvoriti grupe prvi put da biste mogli stvarati resurse kao što su klaster HDInsight. 

* Zamijenite __SubscriptionID__ ID pretplate primili prilikom stvaranja Upravitelj servisa.
* Zamijenite __AccessToken__ token za pristup primljene u prethodnom koraku.
* Zamijenite __DataCenterLocation__ koju želite stvoriti grupu resursa i resursima, u centru za podatke. Na primjer, "Jug središnje NAM'. 
* Zamijenite __ResourceGroupName__ naziv koji želite koristiti za ovu grupu:

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

Ako je zahtjev uspije, dobit ćete odgovor 200 niza i tijelo odgovor će sadržavati JSON dokument koji sadrži informacije o grupi. Na `"provisioningState"` element sadržavat će vrijednost `"Succeeded"`.

##<a name="create-a-deployment"></a>Stvaranje implementacije

Koristite sljedeće konfiguracije klaster (predloška i parametar vrijednosti,) u grupu resursa.

* Zamijenite __SubscriptionID__ i __AccessToken__ vrijednosti koristili. 
* Zamijenite __ResourceGroupName__ naziv grupe resursa koji ste stvorili u prethodnom odjeljku.
* Zamijenite __DeploymentName__ naziv koji želite koristiti za ovaj implementacije.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] Ako ste spremili JSON dokumenata koja sadrži predložak i parametara u datoteku, možete koristiti sljedeće umjesto `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Ako je zahtjev uspije, dobit ćete odgovor 200 niza i tijelo odgovor će sadržavati JSON dokument koji sadrži informacije o postupak implementacije.

> [AZURE.IMPORTANT] Imajte na umu poslana implementaciju, ali trenutno nije dovršena. Može potrajati nekoliko minuta, obično oko 15, radi implementacije da biste dovršili.

##<a name="check-the-status-of-a-deployment"></a>Provjera statusa implementacije

Da biste provjerili status implementacije, koristite sljedeće:

* Zamijenite __SubscriptionID__ i __AccessToken__ vrijednosti koristili. 
* Zamijenite __ResourceGroupName__ naziv grupe resursa koji ste stvorili u prethodnom odjeljku.

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

To će vratiti podatke JSON dokument koji sadrži informacije o postupak implementacije. Na `"provisioningState"` element će sadržavati status implementacije; Ako to sadrži vrijednost `"Succeeded"`, a zatim implementacijskih je uspješno dovršena. U ovom trenutku svoj klaster mora biti dostupan za korištenje.

##<a name="next-steps"></a>Daljnji koraci

Sad kad ste stvorili uspješno je HDInsight klaster, koristite sljedeće da biste saznali kako raditi s svoj klaster. 

###<a name="hadoop-clusters"></a>Hadoop klastere

* [Korištenje grozd s HDInsight](hdinsight-use-hive.md)
* [Korištenje Svinja s HDInsight](hdinsight-use-pig.md)
* [Korištenje MapReduce s HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase klastere

* [Početak rada s HBase na HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Razvoj aplikacija Java HBase na HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Klastere oluja

* [Razvoj Java topologija za oluja na HDInsight](hdinsight-storm-develop-java-topology.md)
* [Korištenje komponente Python oluja na HDInsight](hdinsight-storm-develop-python-topology.md)
* [Implementacija i nadzirati topologija s oluja na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
