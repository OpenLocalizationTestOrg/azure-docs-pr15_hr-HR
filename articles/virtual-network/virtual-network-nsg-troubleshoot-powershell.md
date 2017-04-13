<properties 
   pageTitle="Otklanjanje poteškoća s mrežom sigurnosnim grupama – PowerShell | Microsoft Azure"
   description="Saznajte kako otkloniti poteškoće s mrežom sigurnosne grupe u modelu implementaciju upravljanja resursima Azure pomoću komponente PowerShell Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Otklanjanje poteškoća s mrežom sigurnosnim grupama pomoću komponente PowerShell Azure

> [AZURE.SELECTOR]
- [Portal za Azure](virtual-network-nsg-troubleshoot-portal.md)
- [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Ako ste konfigurirali mreže sigurnosnih grupa (NSGs) virtualnog računala (VM) i dolazi do problema s povezivanjem VM, ovaj članak sadrži pregled Dijagnostika mogućnosti za NSGs pomoći u rješavanju problema Dodatno.

NSGs omogućuju upravljanje vrstama prometa taj tijek i virtualnim strojevima (VMs). NSGs primjenjuje se na podmreže programa Azure virtualne mreže (VNet), mreže sučelja (NIC) ili oboje. Učinkovita pravila koja se primjenjuju na NIC su prikupljene pravila koja postoji u NSGs primjenjuje na NIC i podmreže je povezano s. Pravila preko tih NSGs ponekad možete međusobno su u sukobu i utjecati na VM mrežne veze.  

Sva pravila učinkovitih sigurnost možete pogledati s NSGs, kao što je primijenjena na vašem VM NIC-ovi. U ovom se članku objašnjava otklanjanje problema s povezivanjem VM pomoću tih pravila u model implementacije Azure Voditelj resursa. Ako niste upoznati s VNet i NSG koncepti, pročitajte u člancima pregled [virtualne mreže](virtual-networks-overview.md) i [mreže sigurnosne grupe](virtual-networks-nsg.md) .

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Pomoću učinkovitih pravila sigurnost da biste otklonili VM promet tijek

Scenarij u kojem se nalazi iza su primjeri uobičajenih problema s vezom:

VM pod nazivom *VM1* je dio podmreže pod nazivom *Subnet1* unutar VNet pod nazivom *WestUS VNet1*. Pokušaj povezivanja s VM RDP putem TCP priključka 3389 neće uspjeti. NSGs primjenjuju se na NIC *VM1 NIC1* i podmreže *Subnet1*. Promet TCP priključka 3389 je dopušteno NSG pridružene mrežno sučelje *VM1 NIC1*, no pomoću naredbe ping TCP da ne uspije priključak 3389 VM1 korisnika.

Dok je u ovom se primjeru koriste TCP priključak 3389, mogu se sljedeće korake da biste odredili neuspjeha ulazni i izlazni vezu putem bilo kojeg priključka.

## <a name="detailed-troubleshooting-steps"></a>Detaljne korake za otklanjanje poteškoća
Izvršite sljedeće korake za otklanjanje poteškoća s NSGs za na VM:

1. Započnite sesiju ljuske PowerShell Azure i prijavite se na Azure. Ako niste upoznati s komponentom Azure PowerShell, pročitajte članak [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

2. Unesite sljedeću naredbu da biste se vratili sve NSG pravila koja se primjenjuju na NIC pod nazivom *VM1 NIC1* u grupi resursa *RG1*:

        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Ako ne znate naziv na NIC, unesite sljedeću naredbu za dohvaćanje imena svih NIC-ovi u grupu resursa: 
    
    >`Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`

    Sljedeći tekst je uzorka izlaza učinkovitih pravila za *VM1 NIC1* NIC vratiti:

        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
        
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]

    Imajte na umu sljedeće informacije u ispisu:

    - Postoje dva odjeljka **NetworkSecurityGroup** : jedna je pridružen podmreže (*Subnet1*) i jedan je povezan s NIC (*VM1 NIC1*). U ovom primjeru programa NSG je primijenjen na svaki.
    - **Pridruživanje** prikazuje resursa (podmreže ili NIC) navedeni NSG pridružena. Ako je resursa NSG premjestiti/razdruženo neposredno prije izvođenja ove naredbe, možda ćete morati pričekati nekoliko sekundi prije nego što se promjene odražavaju u rezultatu naredbe. 
    - Nazive pravilo koje su prefaced s *defaultSecurityRules*: stvara se kada programa NSG, nekoliko pravila zadane sigurnosne stvaraju se u njoj. Nije moguće ukloniti zadana pravila, ali ih možete nadjačati veći prioritet pravila.
     U članku [Pregled NSG](virtual-networks-nsg.md#default-rules) da biste saznali više o NSG zadana sigurnosna pravila.
    - **ExpandedAddressPrefix** proširuje prefiksi adresa za NSG zadane oznake. Oznake predstavljaju prefiksi više adresa. Proširenje oznake mogu biti korisni kad otklanjanje poteškoća s povezivanjem VM iz određene adrese prefiksi. Ako, na primjer, s VNET peering, VIRTUAL_NETWORK oznaka proširiti i prikazati peered VNet prefiksi u prethodni rezultat.

        >[AZURE.NOTE] Naredba samo prikazuje efektivnu pravila ako je NSG vezan uz podmreži, na NIC, ili oboje. Na VM može imati više NIC-ovi s različitim NSGs primijeniti. Pri otklanjanju, pokrenite naredbu za svaku NIC.
        
3. Da biste olakšali filtriranje putem veći broj NSG pravila, unesite sljedeće naredbe za daljnje rješavanje problema s: 

        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView

    Prikaz rešetki se primjenjuje filtar za RDP promet (TCP priključak 3389), kao što je prikazano na sljedećoj slici:

    ![Popis pravila](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
    
4. Kao što vidite u prikazu rešetke, postoje i omogućuju i Nemoj dopustiti pravila za RDP. Izlaz iz korak 2 pokazuje da pravilo *DenyRDP* NSG primjenjuje se na podmreži. Za ulazna pravila NSGs primjenjuje se na podmreži obrađuju prvi put. Ako se pronađe podudarajući kontakt, NSG primjenjuje se na mrežnom sučelju nije obrađen. U ovom slučaju pravilo *DenyRDP* iz podmreži blokira RDP da biste VM (**VM1**).

    >[AZURE.NOTE] Na VM može imati više NIC-ovi pridružen. Svaki može biti povezani drugoj podmreži. Budući da u odnosu na NIC će se izvoditi naredbi u prethodnim koracima, važno je da biste bili sigurni da navedete NIC imate nije uspjelo povezivanje. Ako niste sigurni, uvijek možete pokrenuti naredbe protiv svaki NIC priložiti u VM.

5. Da biste RDP u VM1 promijenite pravilo *Uskratiti RDP (3389)* na *Dopusti RDP(3389)* u **Subnet1 NSG** NSG. Provjerite je li TCP priključak 3389 otvorilo otvaranje veze na VM sustava RDP ili pomoću alata PsPing. Dodatne informacije o PsPing prema [stranici za preuzimanje PsPing](https://technet.microsoft.com/sysinternals/psping.aspx) za čitanje

    Možete i ukloniti pravila iz programa NSG pomoću informacija u Izlaz iz sljedeću naredbu:

        Get-Help *-AzureRmNetworkSecurityRuleConfig
        

## <a name="considerations"></a>Razmatranja

Pri otklanjanju poteškoća s povezivanjem Imajte na umu sljedeće:

- Zadana pravila NSG će blokirati ulaznog pristup putem Interneta, a zatim samo dopušta VNet dolazni promet. Želite li dopustiti pristup ulaznog s Interneta, po potrebi treba izričito dodati pravila.
- Ako postoje NSG pravila sigurnosti uzrokuje veza s mrežom na VM uvoza, problem može biti krajnji rok za:
    - Vatrozid radi unutar na VM operacijski sustav
    - Usmjerava konfigurirana za virtualne aparata ili promet na lokalni. Internetski promet može biti preusmjereni na lokalni putem prisilno tuneliranje. RDP/SSH veze sustava s Interneta na VM možda neće funkcionirati uz tu postavku, ovisno o način postupanja s lokalnim mrežnog hardvera u ovom promet. Pročitajte članak [Otklanjanje poteškoća usmjerava](virtual-network-routes-troubleshoot-powershell.md) da biste saznali kako utvrditi probleme usmjeravanje koji impeding tijek prometa i na VM. 
- Ako ste peered VNets, po zadanom, VIRTUAL_NETWORK oznaka automatski se proširiti za uključivanje prefiksi peered VNets. Ove prefiksi možete pogledati na popisu **ExpandedAddressPrefix** za otklanjanje poteškoća vezanih uz VNet peering povezivanje. 
- Učinkovite sigurnosna pravila prikazuju se samo ako je riječ o programa NSG vezane uz na VM NIC i ili podmreže. 
- Ako postoje bez NSGs pridružene NIC-a ili podmreže i imate javnu IP adresa dodijeljena vaše VM, svi priključci bit će otvoren za ulazni i izlazni pristup. Ako je na VM javnu IP adresu, primjene NSGs NIC ili podmreže preporučuje se.  
