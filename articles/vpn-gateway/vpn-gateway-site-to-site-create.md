<properties
   pageTitle="Stvaranje virtualne mreže s vezom za pristupnik za VPN-a web-mjesto pomoću portala za Azure klasični | Microsoft Azure"
   description="Stvaranje na VNet s vezom za pristupnik za VPN S2S za više lokacija i konfiguracije hibridnog pomoću klasične implementacije modela."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-classic-portal"></a>Stvaranje na VNet s vezom za web-mjesto pomoću portala za Azure klasični

> [AZURE.SELECTOR]
- [Voditelj resursa – Portal za Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Voditelj resursa – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasični – klasični Portal](vpn-gateway-site-to-site-create.md)

U ovom se članku vodit će vas voditi kroz stvaranje virtualne mreže i web-mjesto VPN pristupnika vezu lokalne mreže pomoću model klasični implementacije i klasični portal. Veza web-mjesto koje se mogu koristiti za više lokalni i Hibridni konfiguracije.

![Dijagram web-mjesto] (./media/vpn-gateway-site-to-site-create/site2site.png "web-mjesto")


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Uvođenje modela i postupci za veze na web-mjesto

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Sljedeća tablica prikazuje trenutno dostupno implementacije modela i postupci za konfiguracije web-mjesto. Kada članak s navedeni koraci za konfiguraciju postane dostupan, ne možemo povezati izravno iz u ovoj su tablici.

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Dodatna konfiguracija 

Ako se želite povezati VNets zajedno, potražite u članku [Konfiguriranje VNet VNet veze za uvođenje klasičnog model](virtual-networks-configure-vnet-to-vnet-connection.md). Ako želite dodati vezu web-mjesto VNet koji već ima veze, potražite u članku [Dodavanje veze S2S VNet s postojeće veze pristupnika VPN-a](vpn-gateway-multi-site.md).
 
## <a name="before-you-begin"></a>Prije početka

Provjerite imate li sljedeće stavke prije početka konfiguracije.

- Kompatibilni uređaj VPN-a i nekome tko se može konfigurirati. Potražite u članku [o uređajima VPN-a](vpn-gateway-about-vpn-devices.md). Ako ne poznajete konfiguriranje uređaju VPN, ili nisu poznati s IP adresom raspona koji se nalazi u vaše lokalne mreže konfiguracije, potrebno je koordinaciji s nekim tko može zatražiti te detalje za vas.

- Vanjsko dostupnog javnu IP adresu za svoj uređaj VPN-a. U ovom IP adresa nije ga moguće pronaći iza na NAT.

- Azure pretplate. Ako već nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ili znak prema gore za [besplatan račun](https://azure.microsoft.com/pricing/free-trial/).


## <a name="CreateVNet"></a>Stvaranje virtualne mreže

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com/).

2. U donjem lijevom kutu zaslona kliknite **Novo**. U navigacijskom oknu kliknite **Mrežnim servisima**, a zatim **Virtualne mreže**. Kliknite **Stvaranje Prilagođeno** da biste pokrenuli čarobnjak za konfiguraciju.

3. Da biste stvorili vaše VNet, unesite konfiguracijske postavke na sljedećim stranicama:

## <a name="Details"></a>Stranica s detaljima virtualne mreže

Unesite sljedeće podatke:

- **Naziv**: naziv virtualne mreže. Na primjer, *EastUSVNet*. Ovaj naziv virtualne mreže ćete koristiti prilikom implementacije VMs i PaaS slučajevima, tako da ne preporučujemo vam da promijenite naziv previše složeno.
- **Lokacije**: lokaciju izravno povezani s fizičke mjesto (regija) koje želite da se Resursi (VMs) da biste nalaze. Na primjer, ako želite VMs koji implementirati virtualne mreži fizički nalazi u *SAD -a za istočnoazijske*, odaberite mjesto. Ne možete promijeniti regija povezani s mrežom virtualne kad ga stvorite.

## <a name="DNS"></a>DNS poslužitelji i stranice za povezivanje s VPN-a

Unesite sljedeće podatke, a zatim kliknite sljedeće strelicu dolje desno.

- **DNS poslužitelji**: Unesite naziv poslužitelja DNS-a i IP adresa ili odaberite prethodno registrirani DNS poslužitelj na izborniku prečaca. Ta postavka nije moguće stvoriti DNS poslužitelj. Omogućuje određivanje DNS poslužitelji koji želite koristiti za razrješavanje imena u pošti virtualne mreže.
- **Konfiguriranje VPN web-mjesto**: Potvrdite okvir za **Konfiguriranje VPN-a web-mjesto**.
- **Lokalne mreže**: lokalne mreže predstavlja fizička lokalne lokacije. Možete odabrati lokalne mreže koju ste prethodno stvorili ili stvorite novu lokalnu mrežu. Međutim, ako odaberete da biste koristili lokalne mreže koju ste prethodno stvorili, otvorite stranicu za konfiguraciju **Lokalne mreže** i provjerite je li VPN uređaju IP adresa (javno dostupnog IPv4 adresa) za uređaj VPN točne.

## <a name="Connectivity"></a>Povezivanje web-mjesto stranice

Ako stvarate novu lokalnu mrežu, prikazat će se stranica za **Povezivanje s web-mjesto** . Ako želite li koristiti lokalne mreže koju ste prethodno stvorili, ova stranica će se pojaviti u čarobnjaku za, a možete premjestiti na sljedeću sekciju.

Unesite sljedeće podatke, a zatim kliknite strelicu za sljedeći.

-   **Naziv**: naziv koji želite uputiti poziv svog lokalnog (lokalno) mreže web-mjesta.
-   **IP adresa za VPN uređaj**: javno dostupnog IPv4 adresa uređaja lokalnog VPN-a koji koristite za povezivanje s Azure. Uređaj VPN-a ne može se nalaziti iza na NAT.
-   **Prostor adrese**: obuhvaćaju početni IP i CIDR (adresu zbroj). Navedite adresu range(s) koje želite slati putem virtualne mreže pristupnik vaše lokalne lokalne lokacije. Ako odredišnu IP adresu pada unutar raspona koje ste odredili, usmjeravanja putem virtualne mreže pristupnika.
-   **Dodavanje prostora na adresu**: Ako imate više adresa raspona koji želite slati putem virtualne mreže pristupnika, navedite raspon svaku dodatnu adresu. Možete dodati ili ukloniti raspona kasnije na stranici **Lokalnoj mreži** .

## <a name="Address"></a>Virtualne mreže adresu razmake stranice

Navedite adresu raspona koji želite koristiti za virtualne mreže. To su dinamičke IP adrese (DIPS) koji će se dodijeliti u VMs i druge instance uloge koje implementirati virtualne mreže.

To je osobito važno da biste odabrali raspon koji se ne preklapa s bilo kojeg od raspona koji se koriste za lokalnu mrežu. Morate se usklađuju s administratora mreže. Administrator mreže možda morati carve izvan raspona IP adrese iz lokalne mreže adresnog prostora možete koristiti za virtualne mreže.

Unesite sljedeće podatke, a zatim kliknite kvačicu dolje desno da biste konfigurirali mreže.

- **Prostor adrese**: obuhvaćaju početni vremena i IP adresa. Provjerite je li da za to predviđen adresu navedete ne preklapa neku adresu razmake koje imate lokalne mreže.
- **Dodavanje podmreži**: vremena i uključiti početni IP adresa. Dodatni podmreže nisu potrebni, ali želite stvoriti zasebnu podmreže za VMs koji će imati statične DIPS. Ili, možda ćete morati imati vaše VMs podmreži razlikuje se od ostalih instanci ulogu.
- **Dodavanje pristupnog podmreži**: kliknite da biste dodali podmreži pristupnika. Pristupnik podmreži koristi se samo za pristupnik za virtualne mreže te je potreban za tu konfiguraciju.

Kliknite kvačicu pri dnu stranice, a virtualne mreže će početi da biste stvorili. Kada završi, prikazat će se **stvoreno** navedene u odjeljku **Status** na stranici **mreža** na portalu klasični Azure. Nakon stvaranja na VNet pa možete konfigurirati pristupnik za virtualne mreže.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="VNetGateway"></a>Konfigurirati pristupnik za virtualne mreže

Konfiguriranje pristupnika virtualne mreže da biste stvorili sigurnu vezu web-mjesto. Potražite u članku [Konfiguriranje pristupnik virtualne mreže na portalu za Azure klasični](vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="next-steps"></a>Daljnji koraci

Nakon dovršetka vezu, možete dodati virtualnim strojevima virtualne mreže. Potražite dodatne informacije potražite u dokumentaciji [virtualnih računala](https://azure.microsoft.com/documentation/services/virtual-machines/) .
