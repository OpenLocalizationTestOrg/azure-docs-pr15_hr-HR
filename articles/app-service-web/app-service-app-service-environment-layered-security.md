<properties 
    pageTitle="Slojeviti sigurnosna arhitektura s okruženjima aplikacije servisa" 
    description="Implementacijom Slojeviti sigurnosna arhitektura s okruženjima aplikacije servisa." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="stefsch"/>   

# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Slojeviti sigurnosna arhitektura s okruženjima aplikacije servisa za implementaciju

## <a name="overview"></a>Pregled ##
 
Budući da aplikacije servisa okruženja nuditi Izolirani runtime okruženja u uveden u virtualne mreže, razvojni inženjeri mogu stvoriti Slojeviti sigurnosna arhitektura na koja omogućuje različiti razine pristup mreži za svaki razina fizičke aplikacije.

Uobičajeni želju je da biste sakrili API natrag završava mogli pristupati Internet i Dopusti samo API-ji obraćanje upstream web-aplikacije.  [Mrežni sigurnosnih grupa (NSGs)] [ NetworkSecurityGroups] može se koristiti u podmreže koja sadrži okruženja aplikacije servisa da biste ograničili pristup javnim API aplikacije.

Dijagramu u nastavku prikazuje se primjer arhitektura s WebAPI temelji aplikacije u uveden u okruženju aplikacije servisa.  Tri instance aplikacije zasebnom web, uveden u tri zasebna aplikacije servisa okruženja, upućivanje poziva pozadinsku aplikaciju iste WebAPI.

![Konceptualni arhitekture][ConceptualArchitecture] 

Zeleni znak plus pokazuju sigurnosne grupe mreže na podmreži koji sadrže "apiase" omogućuje dolazni pozivi iz upstream web-aplikacije, kao dobro pozive iz same.  No isti mrežni sigurnosne grupe izričito onemogućuje se pristup Općenito dolazni promet s Interneta. 

Ostatak u ovoj se temi vodi kroz korake potrebne da biste konfigurirali sigurnosne grupe mreže na podmreži koji sadrže "apiase".

## <a name="determining-the-network-behavior"></a>Određivanje ponašanjem mreže ##
Da biste znali što pravila sigurnosti mreže su vam potrebne, morate odrediti koje klijenti mreže dozvoljen dosegne koja sadrži aplikaciju API servisa okruženje za aplikaciju, a koje klijenti bit će blokirane.

Od [mreže sigurnosnih grupa (NSGs)] [ NetworkSecurityGroups] primjenjuju se na podmreže, i aplikacije servisa okruženja uvode u podmreže, pravila iz programa NSG primjenjuju se na **sve** aplikacije koji se izvode na servis okruženju aplikacije.  Pomoću arhitektura uzorka za ovaj članak kada sigurnosne grupe mreže primjenjuje se na podmreži koji sadrže "apiase", sve aplikacije koji se izvode na "apiase" okruženja aplikacije servisa će biti zaštićen isti skup sigurnosnih pravila. 

- **Određivanje izlaznih IP adresu upstream pozivatelje:**  Što je IP adresa ili adrese upstream pozivatelji?  Ove adrese ćete morati imati izričito dopuštenje za pristup u na NSG.  Budući da se pozivi između aplikacije servisa okruženja smatraju pozive "Internet", to znači da izlaznih IP adresa dodijeljeni ta tri upstream aplikacije servisa okruženja mora imati dopuštenje za pristup u NSG za podmreže "apiase".   Dodatne informacije o određivanju izlaznih IP adresa aplikacije u okruženju servisa aplikacija potražite u članku [Arhitektura mreže] [ NetworkArchitecture] članak.
- **API aplikaciju pozadinskih morat ćete sam poziva?**  Ponekad zanemaren i Neupadljiva točkama je scenarij gdje pozadinsku aplikaciju treba sam poziva.  Ako je potrebno pozadinsku aplikaciju API na okruženja aplikacije servisa da biste nazvali sam, to se i tretira kao poziv "Internet".  Ogledna arhitekture za tu radnju dopuštanja pristupa iz izlaznih IP adresu na "apiase" okruženja aplikacije servisa kao i.

## <a name="setting-up-the-network-security-group"></a>Postavljanje mreže sigurnosne grupe ##
Kada se gube se skup izlaznih IP adrese, sljedeći je korak za sastavljanje sigurnosne grupe mreže.  Mrežni sigurnosne grupe mogu se kreirati za oba Voditelj resursa koji se temelje virtualne mreža, kao i klasični virtualne mreže.  Primjere u nastavku prikazuju stvaranje i konfiguriranje programa NSG na klasični virtualne mreže pomoću komponente Powershell.

Za arhitekturu uzorak u okruženju nalaze se u Jug središnje NAM, pa je prazan NSG stvoriti u tom području:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Najprije se eksplicitnih dopustiti pravila dodan je za Azure upravljanja infrastrukture kako je navedeno u članak na [dolazni promet] [ InboundTraffic] za aplikaciju servisa okruženja.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
Nakon toga dva pravila dodat će se da biste omogućili HTTP i HTTPS pozive iz prvog upstream aplikacije servisa okruženje ("fe1ase").

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Troši sredstvo za ispiranje ga i ponovite za na drugi i treći upstream aplikacije servisa okruženja ("fe2ase" i "fe3ase").

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Na kraju dopustiti pristup izlaznih IP adresu API pozadinske aplikacije servisa okruženje tako da ga možete pozvati u samu.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Nema drugih mreže pravila sigurnosti potrebno je konfigurirati jer svaki NSG skupu zadana pravila blokirati ulaznog pristup s Interneta prema zadanim postavkama.

Cijeli popis pravila u sigurnosnoj grupi mrežni prikazuju se ispod.  Imajte na umu kako posljednje pravilo koje je istaknut blokira ulaznog pristup iz svih pozivatelji osim onih koje je izričito dodijeljen pristup.

![Konfiguriranje NSG][NSGConfiguration] 

U posljednjem koraku je da biste primijenili na NSG podmreže koja sadrži aplikaciju servisa okruženje "apiase".  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

S NSG koji se primjenjuje na podmreži, samo ta tri upstream aplikacije servisa okruženja i okruženje za aplikaciju servisa koji sadrži API pozadinske, dopušteno poziva u okruženju "apiase".


## <a name="additional-links-and-information"></a>Dodatne veze i informacije ##
Svih članaka i kako – da biste je za aplikaciju servisa okruženja dostupne u [PROČITAJME za aplikaciju servisa okruženja](../app-service/app-service-app-service-environments-readme.md).

Informacije o [sigurnosnim grupama s mrežom](../virtual-network/virtual-networks-nsg.md). 

Razumijevanje [izlaznih IP adrese] [ NetworkArchitecture] i aplikacije servisa okruženju.

[Mrežni priključci] [ InboundTraffic] koristi okruženja aplikacije servisa.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
