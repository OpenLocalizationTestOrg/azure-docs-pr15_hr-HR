<properties
   pageTitle="Promjena veličine svoj klaster ACS s EŽA Azure | Microsoft Azure"
   description="Upute za promjenu veličine svoj klaster servisa Azure spremnik pomoću EŽA Azure."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, spremnika, Micro-servisima, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="timlt"/>

# <a name="scale-an-azure-container-service"></a>Promjena veličine na servis Azure spremnik

Mogu se mijenjati veličinu out broj čvorove vaše servisa Azure spremnik (ACS) sadrži pomoću alata za Azure EŽA. Kada koristite Azure EŽA za promjenu veličine, alat za vratiti na novu datoteku konfiguracije koji predstavlja se promjene primijeniti spremniku.

## <a name="about-the-command"></a>O naredbi

Azure EŽA mora biti u načinu Azure Voditelj resursa za interakciju s spremnika Azure. Koje možete prijeći u način rada Voditelj resursa tako da nazovete `azure config mode arm`. Na `acs` naredba ima podređene-naredbu pod nazivom `scale` koji ne sve operacije skaliranje spremnik servisa. Možete zatražiti pomoć o raznim parametre koristi skaliranje naredba tako da pokrenete `azure acs scale --help`, koji proizvodi nešto slično sljedećemu:

```azurecli
azure acs scale --help

help:    The operation to scale a container service.
help:
help:    Usage: acs scale [options] <resource-group> <name> <new-agent-count>
help:
help:    Options:
help:      -h, --help                               output usage information
help:      -v, --verbose                            use verbose output
help:      -vv                                      more verbose with debug output
help:      --json                                   use json output
help:      -g, --resource-group <resource-group>    resource-group
help:      -n, --name <name>                        name
help:      -o, --new-agent-count <new-agent-count>  New agent count
help:      -s, --subscription <subscription>        The subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="use-the-command-to-scale"></a>Pomoću naredbe za promjenu veličine

Da biste skalirali spremnik servisa, najprije morate znati **grupa resursa** , a zatim **naziv servisa Azure spremnik (ACS)**, kao i novi broj agenata. Pomoću manje ili veće količine možete skaliranje dolje ili prema gore odnosno.

Preporučujemo vam da znate što trenutni broj agenata servisa za spremnik prije no što promijenite li. Korištenje na `azure acs show <resource group> <ACS name>` naredbu da biste se vratili u config ACS. Imajte na umu <mark>broj</mark> rezultata.

#### <a name="see-current-count"></a>U odjeljku trenutni broj

```azurecli
azure acs show containers-test containerservice-containers-test

info:    Executing command acs show
data:
data:     Id                 : /subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test
data:     Name               : containerservice-containers-test
data:     Type               : Microsoft.ContainerService/ContainerServices
data:     Location           : westus
data:     ProvisioningState  : Succeeded
data:     OrchestratorProfile
data:       OrchestratorType : DCOS
data:     MasterProfile
data:       Count            : 1
data:       DnsPrefix        : myprefixmgmt
data:       Fqdn             : myprefixmgmt.westus.cloudapp.azure.com
data:     AgentPoolProfiles
data:       #0
data:         Name           : agentpools
data:         <mark>Count          : 1</mark>
data:         VmSize         : Standard_D2
data:         DnsPrefix      : myprefixagents
data:         Fqdn           : myprefixagents.westus.cloudapp.azure.com
data:     LinuxProfile
data:       AdminUsername    : azureuser
data:       Ssh
data:         PublicKeys
data:           #0
data:             KeyData    : ssh-rsa <ENCODED VALUE>
data:     DiagnosticsProfile
data:       VmDiagnostics
data:         Enabled        : true
data:         StorageUri     : https://<storageid>.blob.core.windows.net/
```  

#### <a name="scale-to-new-count"></a>Skaliranje novi broj

Budući da je vjerojatno je već self-evident, skaliranja servis spremnik tako da nazovete podršku `azure acs scale` i unošenju podataka za **grupu resursa**, **naziv ACS**i **agent za brojanje**. Kada promjena veličine spremnik servisa Azure EŽA vraća JSON niz koji predstavlja Nova konfiguracija spremnik servisa, uključujući novi broj agent.

```azurecli
azure acs scale containers-test containerservice-containers-test 10

info:    Executing command acs scale
data:    {
data:        id: '/subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test',
data:        name: 'containerservice-containers-test',
data:        type: 'Microsoft.ContainerService/ContainerServices',
data:        location: 'westus',
data:        provisioningState: 'Succeeded',
data:        orchestratorProfile: { orchestratorType: 'DCOS' },
data:        masterProfile: {
data:            count: 1,
data:            dnsPrefix: 'myprefixmgmt',
data:            fqdn: 'myprefixmgmt.westus.cloudapp.azure.com'
data:        },
data:        agentPoolProfiles: [
data:            {
data:                name: 'agentpools',
data:                <mark>count: 10</mark>,
data:                vmSize: 'Standard_D2',
data:                dnsPrefix: 'myprefixagents',
data:                fqdn: 'myprefixagents.westus.cloudapp.azure.com'
data:            }
data:        ],
data:        linuxProfile: {
data:            adminUsername: 'azureuser',
data:            ssh: {
data:                publicKeys: [
data:                    { keyData: 'ssh-rsa <ENCODED VALUE>' }
data:                ]
data:            }
data:        },
data:        diagnosticsProfile: {
data:            vmDiagnostics: { enabled: true, storageUri: 'https://<storageid>.blob.core.windows.net/' }
data:        }
data:    }
info:    acs scale command OK
``` 

## <a name="next-steps"></a>Daljnji koraci

- [Implementacija klaster](container-service-deployment.md)