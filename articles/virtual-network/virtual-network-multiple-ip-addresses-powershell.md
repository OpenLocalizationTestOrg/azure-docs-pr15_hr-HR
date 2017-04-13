<properties
   pageTitle="Više IP adresa za virtualnim strojevima - PowerShell | Microsoft Azure"
   description="Saznajte kako dodijeliti više IP adresa virtualnog računala pomoću komponente PowerShell Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="jdial;annahar" />


# <a name="assign-multiple-ip-addresses-to-virtual-machines"></a>Dodjela više IP adresa virtualnim strojevima

Programa virtualnog računala Azure (VM) moguće je jedan ili više mreža sučelja (NIC) pridružen. Bilo koji NIC može imati jednu ili više javna ili privatna IP adresa dodijeljena. Ako niste upoznati s IP adresa u Azure, pročitajte članak [IP adrese u Azure](virtual-network-ip-addresses-overview-arm.md) da biste saznali više o njima. U ovom se članku objašnjava kako pomoću ljuske PowerShell Azure dodijelite više IP adresa VM u modelu implementaciju upravljanja resursima Azure.

Dodjeljivanje više IP adresa u VM omogućuje sljedeće mogućnosti:

- U kojem se nalazi više web-mjesta ili usluge s ugovorima o različitim IP adresama i SSL certifikata na jednom poslužitelju.
- Služi kao mreža virtualne uređaj, kao što je vatrozid ili raspoređivača učitavanja.
- Mogućnost da biste dodali bilo koji od privatne IP adresa za bilo koju od na NIC-ovi programa Azure raspoređivača opterećenja pozadinske resurse. U prošlosti, samo primarni IP adresa za primarni NIC nije moguće dodati pozadinsku skup.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

Da biste registrirali za pretpregled, poslati poruku e-pošte [Većem broju IP -ovi](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) s identifikacijskog Broja za pretplatu, a koristi.

## <a name="scenario"></a>Scenarij

U ovom se članku ćete povezati tri IP konfiguracija mrežnog sučelja.
Sljedeći primjer konfiguracije bit će stvoren i dodijeljene NIC koji će imati tri privatne IP adrese i jednu javnu IP adresu dodijeljena:

- IPConfig-1: Dinamičku privatne IP adresu (zadano) i javnu IP adrese iz pod nazivom PIP1 javno resursa za IP adresa.
- IPConfig-2: Statičke privatne IP adrese i javnu IP adresu.
- IPConfig-3: Dinamički privatne IP adrese i javnu IP adresu.

    ![Slika zamjenski tekst](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

Ovaj scenarij pretpostavlja da imate grupu resursa pod nazivom *RG1* u kojem postoji VNet pod nazivom *VNet1* i podmreže pod nazivom *Subnet1*. Dodatno, pretpostavlja se imate VM naziva *VM1*, mrežno sučelje naziva *VM1 NIC1* povezan s njom i javnu IP adresu naziva *PIP1*.

[U ovom se članku](./virtual-machines/virtual-machines-windows-ps-create.md ) vodi kroz stvaranje resursa spomenutih u slučaju da ih prije niste stvorili.

## <a name = "create"></a>Stvaranje na VM s više IP adresa

1. Otvorite naredbeni redak PowerShell i dovršili preostale korake u ovom odjeljku unutar jedne sesije PowerShell. Ako još nemate PowerShell instalacije i konfiguracije, slijedite korake u članku [upute za instalaciju i konfiguriranje Azure PowerShell](../powershell-install-configure.md) .

2. Promijenite "vrijednosti" sljedeće $Variables Azure [mjesto](https://azure.microsoft.com/regions) virtualne mreže je u nazivu [grupe resursa](../azure-resource-manager/resource-group-overview.md#resource-groups), VNet unutar grupe resursa, podmreže se želite povezati NIC da biste i naziv u NIC. Dovršite korake da biste dodali više IP adresa u bilo kojem NIC priložiti VM, kao što su vam potrebne.

        $Location = "westcentralus"
        $RgName   = "RG1"
        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"
        $NicName     = "VM1-NIC1"
        $PIP = "PIP"

    Ako ne znate naziv postojećeg Azure mjesta ili grupe resursa, upišite sljedeće naredbe:

        Get-AzureRmLocation      | Format-Table Location
        Get-AzureRmResourceGroup | Format-Table ResourceGroupName

    <a name="subnet"></a>NIC-a mora biti povezano s podmreže unutar postojeću Azure virtualne mrežu (VNet). Sljedeća tri odjeljka: NIC, podmreže i VNet, morate sve postoji u istom regija i [pretplate](../azure-glossary-cloud-terminology.md#subscription).  Ako niste upoznati s VNets, pročitajte članak [Pregled virtualne mreže](virtual-networks-overview.md) da biste saznali više o njima ili pročitajte članak [Stvaranje na VNet](virtual-networks-create-vnet-arm-ps.md) da biste saznali kako ga stvoriti.

    Ako ne znate naziv postojeće VNet, unesite sljedeću naredbu i zamijeniti *VNet1* prethodne varijable naziv na VNet:

        Get-AzureRmVirtualNetwork | Format-Table Name

    Ako na popisu vraćena je prazan, morate stvoriti na VNet. Dodatne informacije potražite u članku [Stvaranje virtualne mreže](virtual-networks-create-vnet-arm-ps.md) .

    Upišite sljedeće naredbe možete dobiti naziv postojeće podmreže unutar na VNet i zamijeniti *Subnet1* iznad naziva podmreži:

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

4. Unesite sljedeću naredbu za dohvaćanje podmreži i dodjeljivanje tjednog prikaza kalendara.

        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

5. <a name="ipconfigs"></a>Definiranje IP konfiguracije koje želite dodijeliti na NIC. Svaki konfiguracije može imati jednu statički ili dinamički privatne IP adresu, a jedan pridružene javnu IP adresa resursa statički ili dinamički adresu.

    Dodavanje i uklanjanje bilo koji broj konfiguracije koji slijede ovisno o tome koliko IP adrese koje želite pridružiti NIC-a i postavke koje želite konfigurirati.

    **IPConfig-1**

    Promijenite vrijednost *PIP1* naziv na postojeće javno IP adresa resurs koji postoji na lokaciji stvarate NIC u i koji nije trenutno povezan s drugom NIC. Postoji promjena *RG1* naziv grupe resursa javno resursa IP adresa. Promijenite *IPConfig 1* na naziv koji želite dati prvi IP konfiguracija. Unesite sljedeće naredbe:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $RgName

        $IpConfigName1 = "IPConfig-1"
        $IPConfig1     = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName1 -Subnet $Subnet -PublicIpAddress $PIP1 -Primary

    Napomena u *-primarni* prijelaz. Kada dodijelite više IP konfiguracije na NIC, kao u *primarni*mora biti dodijeljena jedne konfiguracije. Ako ne znate naziv na postojeće javno IP adresa resursa, unesite sljedeću naredbu: Get-AzureRMPublicIPAddress | Oblikovanje tablice naziv, mjesto, IPAdresa, IpConfiguration

    Ako stupac **IPConfiguration** ima nema vrijednosti za izlaz vraća, javno resursa IP adresa nije povezan s postojećeg NIC i može se koristiti. Ako na popisu prazna ili postoje bez dostupnih javnu IP adresa resursa, možete stvoriti pomoću naredbe **Nova AzureRmPublicIPAddress** .

    >[AZURE.NOTE] Javnu IP adrese imati nominal naknadu. Dodatne informacije o IP adresi cijene, pročitajte stranicu [cijene za IP adresa](https://azure.microsoft.com/pricing/details/ip-addresses) .

    **IPConfig-2**

    Naziv koji želite dati drugi IP konfiguracija i promijenite *10.0.0.5* na koji se ne koriste valjane IP adresa za podmreže dodjeljujete NIC da biste promijeniti *IPConfig 2* . Unesite sljedeće naredbe:

        $IPConfigName2 = "IPConfig-2"
        $IPAddress = 10.0.0.5

        $IPConfig2 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName2 -Subnet $Subnet -PrivateIpAddress $IPAddress

    Unesite sljedeću naredbu, ako ne znate u IP adresa raspon dodijeljene podmreži:

        $VNet.Subnets | Format-Table Name, AddressPrefix

    **IPConfig-3**

    Promijenite naziv koji želite dati treći IP konfiguracija, a zatim unesite sljedeće naredbe *IPConfig 3* :

        $IPConfigName3 = "IPConfig-3"
        $IPConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet

    >[AZURE.NOTE] Možete dodijeliti do 250 privatne IP adrese na NIC. Postoji ograničenje broja javnu IP adresa koji se mogu koristiti u pretplatu. Da biste saznali više, pročitajte članak [ograničava Azure](../azure-subscription-service-limits.md#networking-limits---azure-resource-manager) .

6. Stvaranje NIC pomoću konfiguracije IP definirano u prethodnom koraku.

        $nic = New-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName -Location $Location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3

7. NIC-a priložiti prilikom pisanja na VM tako da slijedite korake u članku [Stvaranje na VM](../virtual-machines/virtual-machines-windows-ps-create.md) . Iako u članku stvara VM sa sustavom Windows Server, upute su jednake za VM Linux, osim odaberete neki drugi operacijski sustav. Provedite korake od 1 do 3 članka. Preskočite korake 4 i 5, a zatim dovršite 6 stvaranje VM članka.

    >[AZURE.WARNING] Korak 6 stvaranje VM članak neće uspjeti ako promijenili varijabla pod nazivom $nic nešto drugo u koraku 6 ovog članka ili niste dovršili prethodne korake ovog članka.

8. Prikaz privatnih IP adrese te DHCP Azure dodijeljene NIC-a i dodijeljene NIC-a tako da upišete sljedeću naredbu javno resursa za IP adresa:

        $nic.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary

9. <a name="os"></a>Ručno dodavanje sve privatne IP adrese (uključujući primarni) TCP/IP konfiguracija u operacijskom sustavu. 

**Windows**

1. U naredbeni redak upišite *ipconfig/sve*.  Možete vidjeti samo *primarni* privatne IP adrese (putem DHCP).
2. Sljedeće upišite *ncpa.cpl* u prozor naredbenog retka. Otvorit će se novi prozor.
3. Otvorite svojstva za **Lokalne područje veze**.
4. Dvaput kliknite verziju Internet Protocol 4 (IPv4)
5. Odaberite **koristi sljedeće IP adrese** , a zatim unesite sljedeće vrijednosti:
    - **IP adresa**: unesite *primarni* privatne IP adrese
    - **Masku**: Postavi ovisno o vašoj podmreži. Na primjer, ako je podmreži na /24 podmreže zatim maska podmreže je 255.255.255.0.
    - **Zadani pristupnik**: prvi IP adresa u podmreži. Ako je vašoj podmreži 10.0.0.0/24, IP adresa pristupnika je 10.0.0.1.
    - Kliknite **koristi sljedeće adrese DNS poslužitelja** , a zatim unesite sljedeće vrijednosti:
        - **Preferirani DNS poslužitelj:** Ako ne koristite vlastitu DNS poslužitelj, unesite 168.63.129.16.  Ako ste, upišite IP adresu DNS poslužitelja.
    - Kliknite gumb **Dodatno** i dodavanje dodatnih IP adresa. Dodavanje svih sekundarne privatne IP adrese navedene u koraku 8 NIC-a s istoj podmreži navedena za primarni IP adresa.
    - Kliknite **u redu** da biste zatvorili out TCP/IP postavke, a zatim **u redu** da biste zatvorili postavke prilagodnika. To će uspostaviti RDP veza.

6. U naredbeni redak upišite *ipconfig/sve*. Prikazuju se sve IP adrese koje ste dodali, a zatim DHCP isključen.


**Linux (Ubuntu)**

1. Otvaranje terminal prozora.
2. Provjerite jeste li vi korijenski korisnika. Ako niste, to možete učiniti pomoću sljedeće naredbe:

            sudo -i
3. Ažurirajte konfiguracijska datoteka sučelje mreže (Ako 'eth0').
    - Zadržavanje postojećih stavke retka za dhcp. Kao što je se prije nalazila na starije to će konfigurirati primarni IP adresa.
    - Dodajte konfiguracija za dodatne statičke IP adresu pomoću sljedeće naredbe:

                cd /etc/network/interfaces.d/
                ls

        Trebali biste vidjeti .cfg datoteke.
4. Otvorite datoteku: smjer *naziv datoteke*.

    Trebali biste vidjeti sljedeće retke na kraju datoteku:

            auto eth0
            iface eth0 inet dhcp
5. Dodajte sljedeće retke nakon retke koji postoji u ovoj datoteci:

            iface eth0 inet static
            address <your private IP address here>
6. Spremite datoteku pomoću sljedeće naredbe:

            :wq
7.  Vraćanje izvornih postavki sučelje mreže pomoću sljedeće naredbe:

            sudo ifdown eth0 && sudo ifup eth0

    >[AZURE.IMPORTANT] Pokrenite ifdown i ifup u istom retku ako koristite daljinske veze.
8. Provjerite je li se s IP adresom dodaje se sučelje mreže pomoću sljedeće naredbe:

            ip addr list eth0

    Prikazat će se s IP adresom dodani kao dio popisa.

**Linux (Redhat, CentOS i drugima)**

1. Otvaranje terminal prozora.
2. Provjerite jeste li vi korijenski korisnika. Ako niste, to možete učiniti pomoću sljedeće naredbe:

            sudo -i
3. Unesite lozinku, a zatim slijedite upute kako se to od vas zatraži. Kada korisnik korijen, idite na mrežnu mapu skripte pomoću sljedeće naredbe:

            cd /etc/sysconfig/network-scripts
4. Popis povezanih ifcfg datoteke pomoću sljedeće naredbe:

            ls ifcfg-*

    Trebali biste vidjeti *ifcfg eth0* kao neke datoteke.
5. Kopiranje datoteke *ifcfg eth0* i nazovite ih *ifcfg-eth0:0* pomoću sljedeće naredbe:

            cp ifcfg-eth0 ifcfg-eth0:0
6. Uređivanje u *ifcfg-eth0:0* datoteke pomoću sljedeće naredbe:

            vi ifcfg-eth1
7. Promjena uređaj da biste odgovarajuće ime u datoteci. *eth0:0* u ovom slučaju, pomoću sljedeće naredbe:

            DEVICE=eth0:0
8. Promjena u *IPADDR = YourPrivateIPAddress* redak u skladu s IP adresom.
9. Spremite datoteku pomoću sljedeće naredbe:

            :wq
10. Ponovno pokrenite mrežnim servisima i provjerite jesu li promjene uspješno ponovnim pokretanjem sljedeće naredbe:

            /etc/init.d/network restart
            Ipconfig

    Trebali biste vidjeti IP adresa koji ste dodali, *eth0:0*, na popisu vraća.

## <a name="add"></a>Dodavanje IP adrese u postojeći VM

Izvršite sljedeće korake da biste dodali dodatne IP adrese u postojeći NIC:

1. Otvorite naredbeni redak PowerShell i dovršili preostale korake u ovom odjeljku unutar jedne sesije PowerShell. Ako još nemate PowerShell instalacije i konfiguracije, slijedite korake u članku [upute za instalaciju i konfiguriranje Azure PowerShell](../powershell-install-configure.md) .

2. Promijenite "vrijednosti" sljedeće $Variables naziv NIC koju želite dodati IP adrese i grupa resursa i mjesto NIC-a postoji u:

        $NicName     = "RG1-VM1-NIC1"
        $RgName   = "RG1"
        $NicLocation = "westcentralus"

    Ako ne znate naziv NIC koji želite promijeniti, unesite sljedeće naredbe, promijenite vrijednosti u prethodno varijable:

        Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location

3. Stvaranje tjednog prikaza kalendara i postavite ga na postojeće NIC tako da upišete sljedeću naredbu:

        $nic = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName

4. Dohvaćanje ID podmreže NIC-a povezan s povećanim [korak 3](#subnet) od stvaranje na VM s više sekcija IP adrese ovog članka.

5. Stvaranje konfiguracije IP koju želite dodati na mrežu slijedeći upute u [koraku 5](#ipconfigs) stranice za stvaranje na VM s više sekcija IP adrese ovog članka.

6. Promijenite *$IPConfigName4* naziv IP konfiguracija koju ste stvorili u prethodnom koraku. Da biste dodali konfiguraciju, unesite sljedeću naredbu:

        Add-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName4 -NetworkInterface $nic -Subnet $Subnet1

7. Da biste postavili NIC u konfiguraciji IP, unesite sljedeću naredbu:

        Set-AzureRmNetworkInterface -NetworkInterface $nic

8. Dodavanje IP adrese koje ste dodali NIC operacijski sustav VM slijedeći upute u [koraku 9](#os) stvaranje VM s više sekcija IP adrese ovog članka.
