<properties
   pageTitle="Prikaz postupaka implementaciju sa servisom PowerShell | Microsoft Azure"
   description="U članku se opisuje kako pomoću ljuske PowerShell za Azure da biste otkrili probleme s resursima implementacije."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/14/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-powershell"></a>Prikaz postupci implementacije sa servisom PowerShell Azure

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure EŽA](resource-manager-troubleshoot-deployments-cli.md)
- [REST API-JA](resource-manager-troubleshoot-deployments-rest.md)

Možete pogledati postupci za implementaciju putem komponente PowerShell Azure. Možda ćete zainteresirani operacije možete pregledavati i dok ste primili pogreške tijekom implementacije tako da u ovom članku fokus je na prikaz postupaka koji nije uspjela. PowerShell sadrži cmdletima koji omogućuju vam olakšava pronalaženje pogrešaka i određivanje potencijalnih rješenja.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Neke pogreške možete izbjeći tako da provjeri predloška i infrastrukture prije implementacije. Možete prijaviti i zahtjev za dodatnim i informacije o odzivu tijekom implementacije koji mogu biti korisne kasnije za otklanjanje poteškoća. Da biste saznali više o provjeri valjanosti i zapisivanje podataka zahtjeva i odgovora, u odjeljku [uvođenja grupu resursa pomoću predloška Azure Voditelj resursa](resource-group-template-deploy.md).

## <a name="use-deployment-operations-to-troubleshoot"></a>Rješavanje problema pomoću postupci implementacije

1. Da biste cjelokupan status implementacije, koristite naredbu **Dohvati AzureRmResourceGroupDeployment** . Možete filtrirati rezultate samo implementacijama koji nije uspjela.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
        
    Nije uspjelo implementacijama koja vraća u sljedećem obliku:
        
        DeploymentName          : Microsoft.Template
        ResourceGroupName       : ExampleGroup
        ProvisioningState       : Failed
        Timestamp               : 6/14/2016 9:55:21 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                    Name                Type                 Value
                    ===============     ===================  ==========
                    storageAccountName  String               tfstorage9855
                    adminUsername       String               tfadmin
                    adminPassword       SecureString
                    dnsNameforLBIP      String               dns
                    vmNamePrefix        String               myVM
                    imagePublisher      String               MicrosoftWindowsServer
                    imageOffer          String               WindowsServer
                    imageSKU            String               2012-R2-Datacenter
                    lbName              String               myLB
                    nicNamePrefix       String               nic
                    publicIPAddressName String               myPublicIP
                    vnetName            String               myVNET
                    vmSize              String               Standard_D1

        Outputs                 :
        DeploymentDebugLogLevel :

2. Svaki implementacije obično sastoji od više operacija, uz svaki postupak koji predstavlja koraka u postupku implementacije. Da biste otkrili čemu je problem s implementacije, obično morate da biste vidjeli detalje o postupci implementacije. Možete vidjeti status operacije s **Get-AzureRmResourceGroupDeploymentOperation**.

        Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName Microsoft.Template
        
    Koja vraća više operacija uz svako polje u sljedećem obliku:
        
        Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
        OperationId    : A3EB2DA598E0A780
        Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                         duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                         serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
        PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                         serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}

3. Da biste vidjeli dodatne detalje o operacije dohvatiti svojstava operacije sa statusom **nije uspjelo** .

        (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
        
    Koji vraća cjelokupan nije uspjelo operacije uz svako polje u sljedećem obliku:
        
        provisioningOperation : Create
        provisioningState     : Failed
        timestamp             : 2016-06-14T21:54:55.1468068Z
        duration              : PT3.1449887S
        trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
        serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
        statusCode            : BadRequest
        statusMessage         : @{error=}
        targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                                Microsoft.Network/publicIPAddresses/myPublicIP;
                                resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}

    Imajte na umu ID praćenja za operaciju. Koristit ćete koji u sljedećem koraku radi fokusiranja na određene operacije.

4. Da biste poruka o stanju određena operacija nije uspio, koristite sljedeću naredbu:

        ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
        
    Koja vraća:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}

## <a name="use-audit-logs-to-troubleshoot"></a>Korištenje zapisnika nadzora za otklanjanje poteškoća

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Da biste vidjeli pogreške za implementaciju, poduzmite sljedeće korake:

1. Da biste dohvatili stavke evidencije, pokrenite naredbu **Get-AzureRmLog** . Parametri **ResourceGroup** i **Status** možete koristiti da biste vratili samo događaji koji nije uspjelo za grupu za pojedinačni resurs. Ako ne odredite vrijeme početka i završetka, vraćaju se stavke za posljednje sat.
Na primjer, za dohvaćanje nije uspjelo operacije proteklih h pokretanje:

        Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed

    Možete navesti određeni vremenski raspon. U sljedećem primjeru, ne možemo ćete potražite neuspjele akcije posljednjeg dana. 

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -Status Failed
      
    Ili možete postaviti na točno određeno vrijeme početka i završetka za neuspjele akcije:

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00 -Status Failed

2. Ako ta naredba Vrati previše stavki i svojstva, možete se fokusirati pokušajima nadzora preuzimanjem svojstvo **Svojstva** . Ne možemo ćete obuhvaćaju parametar **DetailedOutput** da biste vidjeli poruke o pogreškama.

        (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -DetailedOutput).Properties
        
    Svojstva stavke evidencije koja vraća u sljedećem obliku:
        
        Content
        -------
        {} 
        {[statusCode, BadRequest], [statusMessage, {"error":{"code":"DnsRecordInUse","message":"DNS record dns.westus.clouda...
        {[statusCode, BadRequest], [serviceRequestId, a426f689-5d5a-448d-a2f0-9784d14c900a], [statusMessage, {"error":{"code...

3. Na temelju tih rezultata, recimo fokus na drugi element. Rezultate možete dodatno suziti tako da pogledate poruka o stanju za tu stavku.

        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput -StartTime (Get-Date).AddDays(-1)).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
        
    Koja vraća:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}



## <a name="next-steps"></a>Daljnji koraci

- Pomoć za ispravljanje pogrešaka određeni implementaciju, potražite u članku [Riješite uobičajene pogreške prilikom implementacije resurse za Azure s Azure Voditelj resursa](resource-manager-common-deployment-errors.md).
- Dodatne informacije o korištenju zapisnika nadzora praćenje drugih vrsta akcije, potražite u članku [nadzora operacije s Voditelj resursa](resource-group-audit.md).
- Da biste provjerili valjanost uvođenja prije no što je izvršavanja, potražite u članku [uvođenja grupu resursa pomoću predloška Azure Voditelj resursa](resource-group-template-deploy.md).

