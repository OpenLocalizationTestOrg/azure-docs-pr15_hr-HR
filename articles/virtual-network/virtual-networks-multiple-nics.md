<properties
   pageTitle="Stvaranje na VM s više NIC-ovi"
   description="Upute za stvaranje i konfiguriranje vms s više NIC-ovi"
   services="virtual-network, virtual-machines"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management,azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="create-a-vm-with-multiple-nics"></a>Stvaranje na VM s više NIC-ovi

Možete stvoriti virtualnim strojevima (VMs) u Azure i priložite svaki od vašeg VMs više mreža sučelja (NIC-ovi). Višestruki NIC za preduvjet je mnogo mreže virtualne aparata, kao što su isporuka aplikacija i WAN optimizaciju rješenja. Višestruki NIC nudi dodatne funkcije za mrežni promet upravljanja, uključujući odvajanja prometa između na prednju završili NIC i sigurnosno end NIC(s) ili odvojenosti prometa ravnini podataka iz upravljanje ravnini promet.

![Višestruki NIC za VM](./media/virtual-networks-multiple-nics/IC757773.png)

Na slici iznad prikazuje VM s tri NIC-ovi, svaki povezani s različitim podmreže.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]

- Mjesto na Internetu VIP (klasični implementacijama) je podržana samo NIC. "Zadano" Postoji samo jedan VIP za IP NIC. zadani
- Trenutačno instancu razinu javnu IP (LPIP) adrese (klasični implementacijama) nisu podržani za višestruki NIC VMs.
- Redoslijed mrežne kartice iz unutar na VM bit će slučajni, a može promijeniti i preko Azure infrastrukture ažuriranja. Međutim, IP adresa i odgovarajuće ethernet MAC adresa će ostati. Na primjer, pretpostavimo **Eth1** je IP adresa 10.1.0.100 i MAC adresa 00-0D-3A-B0-39-0D; Nakon ažuriranja Azure Infrastruktura i ponovno pokrenite računalo, nije moguće promijeniti **Eth2**, ali na IP i MAC uparivanje će ostati. Kada je pokrenut klijenta, redoslijed NIC će ostati.
- Adresa za svaki NIC na svakom VM moraju nalaziti u podmreži, više NIC-ovi na jednom VM svaki može dodijeliti adrese koje se nalaze u istoj podmreži.
- Veličina VM određuje broj NIC-ovi koje možete stvoriti za na VM. Referenca za [Windows Server](../virtual-machines/virtual-machines-windows-sizes.md) i [Linux](../virtual-machines/virtual-machines-linux-sizes.md) VM veličine članke da biste odredili koliko NIC-ovi svaki veličina VM podržava. 

## <a name="network-security-groups-nsgs"></a>Mrežni sigurnosnih grupa (NSGs)
U implementacije Voditelj resursa, sve NIC na na VM može biti povezan s na mreži sigurnosne grupe (NSG), uključujući bilo koje NIC-ovi na VM koja ima više NIC-ovi omogućena. Ako je NIC je dodijeljen adresu unutar podmreže na kojoj je pridružen programa NSG podmreži, zatim pravila u NSG na podmreži se primijeniti i na taj NIC. Osim podmreže Pridruživanjem NSGs, možete povezati s NIC pomoću programa NSG.

Ako je pridružen podmreži programa NSG i NIC unutar tog podmreži pridruženo pojedinačno programa NSG, pridruženi NSG pravila primjenjuju **tijek narudžbe** ovisno o smjeru promet predugački pojavljivanje ili iščezavanje NIC-a:

- **Dolazni prometa** čije odredište je u pitanju NIC teče najprije kroz podmreži, pokretanje pravila u podmreže NSG prije prosljeđivanje u NIC-a, a zatim pokretanje na NIC NSG pravila.
- **Izlazni promet** čiji je izvor u pitanju NIC teče najprije izgleda iz NIC, pokretanje pravila na NIC NSG prije koje prolazi kroz podmreži, a zatim pokretanje na podmreži NSG pravila.

Dodatne informacije o [Sigurnosnim grupama s mreže](virtual-networks-nsg.md) i način na koji se primjenjuju na temelju pridruživanja podmreže, VMs i NIC-ovi...

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a>Kako konfigurirati višestruki NIC VM klasični implementacije

Upute u nastavku će vam pomoći da više NIC VM koja sadrži 3 NIC-ovi: Zadana SLIKA i dva dodatna NIC-ovi. Navedeni koraci za konfiguraciju će stvoriti VM biti podešena prema servisa konfiguracije datoteke fragment ispod:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


Potreban vam je sljedeće preduvjete prije no što pokušate pokrenuti naredbe ljuske PowerShell u primjeru.

- Azure pretplate.
- Konfigurirani virtualne mreže. Dodatne informacije o VNets potražite u članku [Pregled virtualne mreže](virtual-networks-overview.md) .
- Najnoviju verziju programa Azure PowerShell preuzeli i instalirali. Saznajte [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

Da biste stvorili na VM više NIC-ovi, slijedite ove korake:

1. Odaberite sliku VM iz galerije slika Azure VM. Imajte na umu da slike često mijenja i dostupne su po regijama. Slika naveden u primjeru u nastavku možda promijeniti ili možda ne može u vašoj regiji, stoga svakako navedite sliku vam je potrebna.

        $image = Get-AzureVMImage `
            -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"

1. Stvaranje VM konfiguracije.

        $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
            -Image $image.ImageName –AvailabilitySetName "MyAVSet"

1. Stvaranje prijava u zadanom administratora.

        Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
            -Password "<YourAdminPassword>"

1. Dodavanje dodatnih NIC-ovi VM konfiguracije.

        Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
            -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
        Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
            -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm

1. Navedite podmreži i IP adresa za zadanu NIC.

        Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
        Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm

1. Stvaranje na VM u virtualne mreže.

        New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm

>[AZURE.NOTE] VNet koju navedete mora postojati (kao što je rečeno u preduvjete). U primjeru u nastavku određuje virtualne mreže pod nazivom **MultiNIC VNet**.

## <a name="limitations"></a>Ograničenja

Sljedeća ograničenja primjenjuju prilikom korištenja značajke NIC više:

- Višestruki NIC VMs moraju se stvoriti u Azure virtualne mreže (VNets). Osobe koje nisu VNet VMs ne može konfigurirati s više NIC-ovi.
- Sve VMs u skupu dostupnost morate koristiti ili više SLIKA ili jedan NIC. Ne može postojati više NIC VMs i jedan NIC VMs unutar skupa dostupnost. Primijenite ista pravila za VMs u oblaku.
- VM s jednom NIC ne može konfigurirati s više NIC-ovi (i obratno) nakon što je uvedena bez brisanja i ponovno stvaranje.


## <a name="secondary-nics-access-to-other-subnets"></a>Sekundarni NIC-ovi pristup drugim podmreže

Prema zadanim postavkama sekundarne NIC-ovi će nije konfiguriran zadani pristupnik, zbog koji će biti ograničeno tijek prometa na sekundarnom mrežne kartice tako da bude u istoj podmreži. Ako korisnicima želite omogućiti sekundarne NIC-ovi za razgovor izvan vlastite podmreže, moraju da biste dodali stavku u tablici usmjeravanja da biste konfigurirali pristupnika prema uputama u nastavku.

>[AZURE.NOTE] VMs stvorena prije srpanj 2015 možda je konfiguriran za sve NIC-ovi zadani pristupnik. Zadani pristupnik za sekundarne NIC-ovi će se ukloniti sve dok su te VMs sustava. U operacijskim sustavima koji koristi loš glavno računalo usmjeravanje model, kao što su Linux, internetska veza možete prekinuti promet ingress i izlazne koristite različite NIC-ovi.

### <a name="configure-windows-vms"></a>Konfiguriranje VMs za Windows

Recimo da imate VM Windows s dvije NIC-ovi na sljedeći način:

- Primarna NIC IP adresa: 192.168.1.4
- Sekundarni NIC IP adresa: 192.168.2.5

U tablici usmjeravanje IPv4 ovaj VM izgleda ovako:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Obratite pozornost na to da zadani smjer (0.0.0.0) dostupna je samo za primarni NIC. Neće moći pristup resursima izvan podmreži za sekundarne NIC-a, kao što se vidi ispod:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

Da biste dodali zadani smjer na sekundarnom NIC-a, slijedite ove korake:

1. Iz naredbenog retka, pokrenite naredbu dolje da biste odredili broja indeksa za sekundarne NIC:

        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================

2. Obratite pozornost na drugom unosu u tablici, s indeksa 27 (u ovom primjeru).
3. Iz naredbenog retka, pokrenite naredbu **Dodaj usmjeravanje** kao što je prikazano u nastavku. U ovom primjeru navodite 192.168.2.1 kao zadani pristupnik za sekundarne NIC:

        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27

4. Da biste testirali povezivanje, vratite se u naredbeni redak, a zatim pokušajte pomoću naredbe ping drugoj podmreži iz sekundarne NIC kao što je prikazano int eh primjeru u nastavku:

        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128

5. Usmjeravanje tablice da biste provjerili novododani usmjeravanje možete provjeriti i kao što je prikazano u nastavku:

        C:\Users\Administrator>route print

        ...

        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Konfiguriranje Linux VMs

Linux VMs jer zadano ponašanje koriste loš hostira usmjeravanje, preporučujemo jesu li sekundarne mrežne kartice ograničeno na promet tokova samo unutar iste podmreže. No ako neke scenarije potražnje povezivanje izvan podmreži, korisnici trebali biste omogućite pravilo temelji usmjeravanja da biste bili sigurni da promet ingress i izlazne koristi iste NIC.

## <a name="next-steps"></a>Daljnji koraci

- Implementacija [MultiNIC VMs u scenariju 2 sloja aplikacije resursima implementacije](virtual-network-deploy-multinic-arm-template.md).
- Implementacija [MultiNIC VMs u scenariju 2 sloja aplikacije klasični implementacije](virtual-network-deploy-multinic-classic-ps.md).
