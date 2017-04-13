<properties 
   pageTitle="Ubrzana umrežavanje za virtualnog računala - PowerShell | Microsoft Azure"
   description="Saznajte kako konfigurirati ubrzana umrežavanje Azure virtualnog računala pomoću komponente PowerShell."
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
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Ubrzanom mreže za virtualnog računala

> [AZURE.SELECTOR]
- [Portal za Azure](virtual-network-accelerated-networking-portal.md)
- [PowerShell](virtual-network-accelerated-networking-powershell.md)

Ubrzanom umrežavanje omogućuje jedan korijenski/i virtualizacije (SR IOV) da biste virtualnog računala (VM) znatno poboljšati njegovih mrežnih performansi. Ovaj put visokih performansi zaobilazi glavno računalo s datapath smanjivanje Latencija, razrješava zahtjeve i procesora za korištenje s najzahtjevnijih radnih opterećenja mreže na podržane vrste VM. U ovom se članku objašnjava kako pomoću ljuske PowerShell Azure konfiguriranje ubrzana umrežavanje u model implementacije Azure Voditelj resursa. Možete stvoriti i u VM s ubrzana povezivanje s mrežom pomoću portala za Azure. Da biste saznali kako to učiniti, kliknite okvir Azure Portal pri vrhu ovog članka.

Sljedeća slika prikazuje komunikaciju između dva virtualnim strojevima (VM) sa ili bez ubrzana umrežavanje:

![Usporedba](./media/virtual-network-accelerated-networking-powershell/image1.png)

Bez ubrzana umrežavanje sav mrežni promet i na VM morate prolaziti glavno računalo, a parametar virtualne. Parametar virtualne nudi sve Uspostava pravila, kao što su mreže sigurnosne grupe, popise kontrole pristupa, odvajanja i ostale usluge mrežom virtualiziranom za mrežni promet. Da biste saznali više, pročitajte članak [virtualizacije Hyper-V mreže i virtualne promjenu](https://technet.microsoft.com/library/jj945275.aspx) .

Ubrzana umrežavanje mrežni promet stigne na kartici mrežom (NIC), a zatim proslijediti na VM. Sva pravila mreže virtualne skretnica primjenjuje bez ubrzana umrežavanje su prenijeti i primijeniti u hardveru. Primjena pravila u hardveru omogućuje NIC-a za prosljeđivanje mrežni promet izravno u VM, zaobilaženje glavno računalo, a parametar virtualne uz zadržavanje svih pravila primjenjuju na glavnom računalu.

Prednosti ubrzana umrežavanje primjenjuju se samo na VM koji je omogućen na. Da biste postigli najbolje rezultate, idealna da biste omogućili ovu značajku na najmanje dva VMs povezan s istom VNet je.  Kada komuniciram preko VNets ili povezivanje lokalnih, ta značajka ima minimalnog utjecaj na ukupna Latencija.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Prednosti

- **Donjem kašnjenje / veći pakete po drugoj (pps):** Uklanjanjem virtualne prebaci na datapath uklanja vrijeme provodite pakete u glavnom računalu za obradu pravila i povećava broj pakete koji može obraditi unutar na VM.
- **Smanjene razrješava zahtjeve:** Obrada virtualne promjenu ovisi o količinu pravilo koje valja zatvoriti i radno opterećenje procesora koji se način obrade. Rasteretite Uspostava pravila za hardver uklanja te raznolikosti tako da isporučuje pakete izravno u VM, uklanjanjem voditelju VM komunikacije i sve interrupts softver i parametri konteksta.
- **Smanjiti procesora:** Zaobilaženje parametar virtualne u glavnom potencijalnih klijenata za manje procesora za obradu mrežni promet.

## <a name="limitations"></a>Ograničenja

Sljedeća ograničenja postoje kada koristite ovu mogućnost:
 
- **Mreže stvaranje sučelja:** Ubrzanom mreže je moguće omogućiti samo za novo sučelje mreže.  Ne može se omogućiti na postojeće mrežnom sučelju.
- **Stvaranja VM:** Mrežno sučelje s ubrzanom mrežom omogućeno mogu samo priložiti u VM stvaranja na VM. Mrežno sučelje ne može se priložiti postojeće VM.
- **Regije:** Koje se nude Zapad središnje NAM i regije zapada Europe Azure područja samo. Skup regija u budućnosti će se proširiti.
- **Podržani operacijski sustav:** Microsoft Windows Server 2012 R2 i Windows Server 2016 Tehnički pretpregled 5. Uskoro će biti dodan službe za podršku Linux i Windows Server 2012.
- **Veličina VM:** Standard_D15_v2 i Standard_DS15_v2 su jedini podržane VM instancu veličine. Dodatne informacije potražite u članku [Windows VM veličine](../virtual-machines/virtual-machines-windows-sizes.md) . Postavljanje veličine podržani VM instanci će se proširiti u budućnosti.

Promjene ove ograničenja će se objaviti na stranici [Azure virtualne umrežavanje ažuriranja](https://azure.microsoft.com/updates/accelerated-networking-in-preview) .

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Stvaranje Windows VM s ubrzanom mrežom

1. Otvorite naredbeni redak PowerShell i dovršili preostale korake u ovom odjeljku unutar jedne sesije PowerShell. Ako još nemate PowerShell instalacije i konfiguracije, slijedite korake u članku [upute za instalaciju i konfiguriranje Azure PowerShell](../powershell-install-configure.md) .
2. Da biste registrirali za pretpregled, poslati poruku e-pošte [Ubrzana umrežavanje pretplate](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) s identifikacijskog Broja za pretplatu, a koristi. Dovršite preostale korake tek kada primite poruku e-pošte koja obavještava da ste prihvatili u pretpregledu.
3. Registrirajte se mogućnost uz pretplatu tako da unesete sljedeće naredbe:

        Register-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingFeature -ProviderNamespace Microsoft.Network
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network

4. Zamijenite *westcentralus* ime nekog drugog mjesta podržava tu mogućnost na popisu u odjeljku [ograničenja](#limitations) ovog članka (po želji). Unesite sljedeću naredbu za postavljanje varijable lokacije:

        $locName = "westcentralus"

5. Zamijenite *RG1* naziv za grupu resursa koje će sadržavati novo sučelje mrežu i unesite sljedeće naredbe ga stvoriti:

        $rgName = "RG1"
        New-AzureRmResourceGroup -Name $rgName -Location $locName

6. Promijenite *VM1 NIC1* što želite nazvati sučelje mreže, a zatim unesite sljedeću naredbu:

        $NICName = "VM1-NIC1"

7. Mrežno sučelje mora biti povezano s podmreže unutar postojeću Azure virtualne mrežu (VNet) na istom mjestu i [pretplate](../azure-glossary-cloud-terminology.md#subscription) kao mrežnog sučelja. Dodatne informacije o VNets tako da pročitate članak [Pregled virtualne mreže](virtual-networks-overview.md) ako niste upoznati s njima. Da biste stvorili na VNet, slijedite korake u članku [Stvaranje na VNet](virtual-networks-create-vnet-arm-ps.md) . Promijenite "vrijednosti" sljedeće $Variables naziv VNet i podmreže se želite povezati mrežno sučelje za.

        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"

    Ako ne znate naziv postojeće VNet na mjestu koje ste odabrali u koraku 3, unesite sljedeće naredbe:
        
        $VirtualNetworks = Get-AzureRmVirtualNetwork
        $VirtualNetworks | Where-Object {$_.Location -eq $locName} | Format-Table Name, Location
        
    Ako na popisu vraćena je prazan, potrebnih za stvaranje na VNet na mjestu. Da biste stvorili na VNet, slijedite korake u članku [Stvaranje virtualne mreže](virtual-networks-create-vnet-arm-ps.md) .

    Dohvaćanje naziva podmreže unutar na VNet, upišite sljedeće naredbe i zamjena *Subnet1* prethodno pod nazivom podmreži:
        
        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

8. Unesite sljedeće naredbe za dohvaćanje VNet i podmreže i dodijeliti ih varijabli.

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

9. Odredite na postojeće javno IP adresa resurs koji može biti povezan mrežnog sučelja da bi mogao povezati s njim putem Interneta. Ako ne želite da biste pristupili VM s mrežnog sučelja putem Interneta, možete preskočiti ovaj korak. Bez javnu IP adresu, morate se povezati s na VM iz drugog VM povezan s istom VNet. 

    Promijenite *PIP1* naziv na postojeće javno IP adresa resurs koji postoji na lokaciji stvarate mrežnog sučelja u i koji nije trenutno povezan s drugog mrežnog sučelja. Ako je potrebno, promijenite $rgName naziv grupe resursa javno resursa IP adresa postoji i unesite sljedeću naredbu:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $rgName

    Ako ne znate naziv na postojeće javno IP adresa resursa, unesite sljedeće naredbe:

        $PublicIPAddresses = Get-AzureRmPublicIPAddress
        $PublicIPAddresses | Where-Object { $_.Location -eq $locName } |Format-Table Name, Location, IPAddress, IpConfiguration

    Ako stupac **IPConfiguration** ima nema vrijednosti za izlaz vraća, javno resursa IP adresa nije povezan s postojećeg mrežnog sučelja i može se koristiti. Ako na popisu prazna ili postoje bez dostupnih javnu IP adresa resursa, možete stvoriti pomoću naredbe nova AzureRmPublicIPAddress.

    >[AZURE.NOTE] Javnu IP adrese imati nominal naknadu. Saznajte više o cijene po stranici [cijene IP adresa](https://azure.microsoft.com/pricing/details/ip-addresses) za čitanje.
10. Ako niste odlučili dodajte javno resurs IP adresa sučelja, uklonite *- PublicIPAddress $PIP1* na kraju naredbu prati. Stvaranje mrežnog sučelja s mrežom ubrzanom tako da upišete sljedeću naredbu:

        $nic = New-AzureRmNetworkInterface -Location $locName -Name $NICName -ResourceGroupName $rgName -Subnet $Subnet -EnableAcceleratedNetworking -PublicIpAddress $PIP1 

11. Dodijeliti mrežnog sučelja programa VM prilikom pisanja na VM slijedeći upute u koracima 3 i 6 članka [Stvaranje na VM](../virtual-machines/virtual-machines-windows-ps-create.md) . U koraku 6-2, zamijenite *Standard_A1* nešto veličina VM navedene u odjeljku [ograničenja](#limitations) ovog članka.

    >[AZURE.NOTE] Ako ste promijenili *naziv* $locName, $rgName ili $nic varijable u ovom članku, koraku 6 stvaranje VM članku neće uspjeti. Međutim, možete promijeniti *vrijednosti* varijabli.

12. Nakon stvaranja na VM, preuzmite [ubrzana umrežavanje upravljački program](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), povezati s VM i pokrenite instalacijski program upravljački program unutar na VM.

13. Desnom tipkom miša pritisnite gumb Windows, a zatim kliknite **Upravitelj uređaja**. **Mellanox ConnectX 3 virtualne funkcija Ethernet prilagodnika** provjerite prikazuje li se u odjeljku mogućnosti **mrežni** kada Proširi, kao što je prikazano na sljedećoj slici:

    ![Upravitelj uređaja](./media/virtual-network-accelerated-networking-powershell/image2.png)