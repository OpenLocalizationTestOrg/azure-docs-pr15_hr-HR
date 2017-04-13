<properties
   pageTitle="Povezivanje VNets pomoću model implementacije Voditelj resursa i Azure portal | Microsoft Azure"
   description="Stvorite vezu pristupnika VPN između VNets pomoću upravitelja resursa i Azure portal."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vnet-to-vnet-connection-using-the-azure-portal"></a>Konfiguriranje VNet VNet veze pomoću portala za Azure

> [AZURE.SELECTOR]
- [Voditelj resursa – Portal za Azure](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Voditelj resursa – PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klasični – klasični Portal](virtual-networks-configure-vnet-to-vnet-connection.md)


U ovom se članku vodit će vas kroz korake za stvaranje veze između VNets u modelu implementacije resursima pomoću pristupnik za VPN-a i Azure portal.

Kada koristite Azure portal za povezivanje virtualne mreže, u VNets mora biti iste pretplate. Ako virtualne mreže u različite pretplate, još uvijek ih možete povezati pomoću postupka [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) .

![V2V dijagrama](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Uvođenje modela i postupci za VNet VNet veze

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Sljedeća tablica prikazuje trenutno dostupno implementacije modela i postupci za VNet VNet konfiguracije. Kada članak s navedeni koraci za konfiguraciju postane dostupan, ne možemo povezati izravno iz u ovoj su tablici.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>O vezama VNet VNet

Povezivanje virtualne mreže s drugom virtualne mreže (VNet-VNet) slično je povezivanja s VNet na mjesto lokalnog web-mjesta. Pristupnik za Azure VPN obje vrste povezivanja koristite sigurne tunelom pomoću IPsec-IKE. VNets povežete može biti u različitim područjima ili u različite pretplate.

Možete čak i kombinirati VNet VNet komunikacije s konfiguracijama više web-mjesta. Omogućuje uspostavljanje topologija mreže kojima se kombiniraju-lokacija povezivanje s inter-virtualne mrežne veze, kao što je prikazano na sljedećem su dijagramu:

![O vezama] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "O vezama")

### <a name="why-connect-virtual-networks"></a>Zašto se povezati virtualne mreže?

Preporučujemo vam da biste se povezali virtualne mreže iz sljedećih razloga:

- **Unakrsni zemlj zalihosti regija i prisutnosti za zemlj.**
    - Možete postaviti vlastitu zemlj replikacije ili sinkronizaciju s sigurno povezivanje bez prelaska na mjesto na Internetu krajnje točke.
    - S upraviteljskim promet Azure i opterećenja, možete postaviti iznimno dostupna radno opterećenje s zemlj zalihosti preko više Azure regija. Primjer važno je da biste postavili SQL uvijek na s grupama dostupnost šire preko više područja Azure.

- **Regionalne aplikacije s više razina odvajanja ili administratora granice**
    - Unutar iste područja, možete postaviti više razina aplikacije s više mreža virtualne povezanih zbog odvajanja ili preduvjeti za administrativne.

Dodatne informacije o vezama VNet VNet potražite u članku [Najčešća pitanja vezana uz VNet VNet](#faq) pri kraju ovog članka.

### <a name="values"></a>Primjer postavke

Kada koristite sljedeće korake kao za vježbu, možete koristiti ogledne vrijednosti konfiguracije. Na primjer, višestruki razmaci adresu koristimo za svaku VNet. Konfiguracija VNet VNet, međutim, nije potrebna višestruki razmaci adresu.

**Vrijednosti za TestVNet1:**

- Naziv VNet: TestVNet1
- Adresa prostora: 10.11.0.0/16
    - Naziv podmreže: sučelju
    - Raspon adresu podmreže: 10.11.0.0/24
- Grupa resursa: TestRG1
- Lokacija: Istočni SAD-a
- Adresa prostora: 10.12.0.0/16
    - Naziv podmreže: pozadinskog
    - Raspon adresu podmreže: 10.12.0.0/24
- Naziv pristupnika podmreže: GatewaySubnet (će Samoispuna na portalu)
    - Pristupnik podmreže adresni raspon: 10.11.255.0/27
- DNS poslužitelj: Koristi IP adresu DNS poslužitelja
- Naziv pristupnika virtualne mreže: TestVNet1GW
- Vrsta pristupnika: VPN-a
- Vrsta VPN: usmjeravanje-poštu
- SKU: Odaberite SKU pristupnika koji želite koristiti
- Javnu IP adresa ime: TestVNet1GWIP
- Veza vrijednosti:
    - Naziv: TestVNet1toTestVNet4
    - Zajednički ključ: možete stvoriti zajednički ključ sami. U ovom primjeru ćemo koristiti abc123. Važno je da prilikom stvaranja veze između sustava VNets, vrijednost se mora podudarati.

**Vrijednosti za TestVNet4:**

- Naziv VNet: TestVNet4
- Adresa prostora: 10.41.0.0/16
    - Naziv podmreže: sučelju
    - Raspon adresu podmreže: 10.41.0.0/24
- Grupa resursa: TestRG1
- Lokacija: Zapad SAD-a
- Adresa prostora: 10.42.0.0/16
    - Naziv podmreže: pozadinskog
    - Raspon adresu podmreže: 10.42.0.0/24
- Naziv GatewaySubnet: GatewaySubnet (će Samoispuna na portalu)
    - Raspon adresu GatewaySubnet: 10.41.255.0/27
- DNS poslužitelj: Koristi IP adresu DNS poslužitelja
- Naziv pristupnika virtualne mreže: TestVNet4GW
- Vrsta pristupnika: VPN-a
- Vrsta VPN: usmjeravanje-poštu
- SKU: Odaberite SKU pristupnika koji želite koristiti
- Javnu IP adresa ime: TestVNet4GWIP
- Veza vrijednosti:
    - Naziv: TestVNet4toTestVNet1
    - Zajednički ključ: možete stvoriti zajednički ključ sami. U ovom primjeru ćemo koristiti abc123. Važno je da prilikom stvaranja veze između sustava VNets, vrijednost se mora podudarati.


## <a name="CreatVNet"></a>1. Stvaranje i konfiguriranje TestVNet1

Ako već imate VNet, provjerite jesu li postavke kompatibilne s dizajnom pristupnika VPN-a. Posebno obratite pažnju na bilo kojem podmreže koji može s njime preklapati s drugim mrežama. Ako imate podmreže koji se preklapaju, vezu neće pravilno funkcionirati. Ako je vaš VNet konfiguriran ispravnim postavkama, možete početi korake u odjeljku [Navedite DNS poslužitelj](#dns) .

### <a name="to-create-a-virtual-network"></a>Da biste stvorili virtualne mreže

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. dodavanje dodatnih adresa prostora i stvaranje podmreže

Možete dodati prostor dodatne adrese i stvaranje podmreže nakon što vaše VNet.
[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. stvaranje podmreži pristupnika

Prije povezivanja virtualne mreže pristupnika, najprije morate stvoriti podmreže pristupnik za virtualne mreže na koju se želite povezati. Ako je to moguće, najbolje je da biste stvorili podmreži pristupnika pomoću blok CIDR /28 ili /27 omogućili dovoljno IP adrese kako bi odgovarao dodatne buduće konfiguracijske preduvjeti.

Ako stvarate tu konfiguraciju kao programa vježbu, pročitajte ove [Postavke primjer](#values) prilikom stvaranja vašoj podmreži pristupnika.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Da biste stvorili podmreži pristupnika

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="DNSServer"></a>4. Navedite DNS poslužitelja (nije obavezno)

Ako želite imati razlučivost naziv virtualnim strojevima koje su uvedene na vašem VNets, navedite DNS poslužitelj.

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]


## <a name="VNetGateway"></a>5. stvaranje pristupnika virtualne mreže

U ovom ćete koraku stvoriti pristupnika virtualne mreže za vaše VNet. Ovaj korak može potrajati i do 45 minuta. Ako stvarate tu konfiguraciju kao za vježbu, možete se referirati na [primjerima postavki](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Da biste stvorili pristupnik virtualne mreže

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


## <a name="CreateTestVNet4"></a>6. Stvaranje i konfiguriranje TestVNet4

Nakon što ste konfigurirali TestVNet1, stvorite TestVNet4 ponovite prethodne korake zamjenu vrijednosti s onima TestVNet4. Ne morate pričekati da završite stvaranje prije konfiguriranja TestVNet4 pristupnika virtualne mreže za TestVNet1. Ako koristite vlastitu vrijednosti, provjerite je li da za to predviđen adresu ne preklapa s bilo kojom VNets koji želite povezati.


## <a name="TestVNet1Connection"></a>7. konfigurirati vezu TestVNet1

Kada ste dovršili pristupnika virtualne mreže za TestVNet1 i TestVNet4, možete je stvoriti virtualne mreže pristupnika veze. U ovom ćete odjeljku će stvorite vezu s VNet1 VNet4.

1. U **svim resursima**, dođite do pristupnika virtualne mreže za vaše VNet. Na primjer, **TestVNet1GW**. Kliknite **TestVNet1GW** da biste otvorili pristupnika plohu virtualne mreže.

    ![Plohu veze] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Plohu veze")

2. Kliknite **+ Dodaj** da biste otvorili plohu **Dodaj vezu** .

3. Na plohu **Dodavanje veze** u polje Naziv upišite naziv za vezu. Na primjer, **TestVNet1toTestVNet4**.

    ![Naziv veze] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Naziv veze")

4. Za **vrstu veze**. na padajućem izborniku odaberite **VNet VNet** .

5. Vrijednost polja **prvi pristupnika virtualne mreže** automatski ispunjava jer stvarate ovu vezu s pristupnika navedeni virtualne mreže.

6. Polje **drugi pristupnika virtualne mreže** je pristupnik virtualne mreže za VNet koju želite stvoriti vezu. Kliknite da biste otvorili plohu **Odaberite virtualne mreže pristupnika** **Odaberite drugi pristupnika virtualne mreže** .

    ![Dodavanje veze] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Dodavanje veze")

7. Prikaz pristupnika virtualne mreže koji su navedeni na ovom plohu. Obratite pozornost na to navedene su samo virtualne mreže pristupnika koje se nalaze u svoju pretplatu. Ako se želite povezati pristupnika virtualne mreže koja nije u svoju pretplatu pomoću [komponente PowerShell članka](vpn-gateway-vnet-vnet-rm-ps.md). 

8. Kliknite pristupnika virtualne mreže koju želite povezati.
 
9. U polju **zajedničko ključ** upišite zajednički ključ za vezu. Možete generirati ili stvaranje sljedećeg ključa. U vezi s web-mjesto ključa će biti potpuno isto za lokalni uređaj i virtualne mrežnu vezu pristupnika. Koncept slično je ovdje, osim koji umjesto povezivanja s uređajem VPN, se povezujete s drugom pristupnika virtualne mreže.

    ![Zajednički se koristi ključ] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Zajednički se koristi ključ")

10. Kliknite **u redu** pri dnu zaslona u plohu da biste spremili promjene.

## <a name="TestVNet4Connection"></a>8. konfigurirati vezu TestVNet4

Nakon toga stvorite vezu s TestVNet4 TestVNet1. Koristite iste metode koju ste koristili da biste stvorili vezu iz TestVNet1 TestVNet4. Provjerite koristite li iste zajednički ključ.

## <a name="VerifyConnection"></a>9. Provjerite je li vaša veza

Provjerite stanje veze. Za svaki pristupnik virtualne mreže, učinite sljedeće:

1. Pronađite plohu pristupnika virtualne mreže. Na primjer, **TestVNet4GW**. 
2. Na pristupnika plohu virtualne mreže kliknite **veze** da biste pogledali plohu veze za pristupnik za virtualne mreže.

Prikaz veze i provjerite je li status. Nakon stvaranja veze, vidjet ćete **je uspjelo** i **povezan** kao vrijednosti stanja.

![Uspješna] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Uspješna")

Dvokliknite svaku vezu s zasebno da biste pogledali dodatne informacije o vezu.

![Osnove] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Osnove")

## <a name="faq"></a>Najčešća pitanja vezana uz VNet VNet

Dodatne informacije o vezama VNet VNet u detaljima najčešća Pitanja.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Daljnji koraci

Nakon dovršetka vezu, možete dodati virtualnim strojevima virtualne mreže. Upute potražite u članku [Stvaranje virtualnog računala](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
