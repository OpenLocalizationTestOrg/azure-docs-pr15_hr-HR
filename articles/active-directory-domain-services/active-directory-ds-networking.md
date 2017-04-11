<properties
    pageTitle="Azure servisa Active Directory Domain Services: Smjernice za umrežavanje | Microsoft Azure"
    description="Zahtjevi za Azure Active Directory Domain Services za povezivanje s mrežom"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="maheshu"/>

# <a name="networking-considerations-for-azure-ad-domain-services"></a>Zahtjevi za Azure AD domenske servise za povezivanje s mrežom

## <a name="how-to-select-an-azure-virtual-network"></a>Kako odabrati Azure virtualne mreže
Sljedeće se smjernice pomoć pri odabiru virtualne mreže za uporabu Azure servisa Active Directory Domain Services.

### <a name="type-of-azure-virtual-network"></a>Vrsta Azure virtualne mreže

- Možete omogućiti Azure AD domenske servise u klasični Azure virtualne mreže.

- Azure servisa Active Directory Domain Services **ne možete omogućiti u virtualne mreže stvoren pomoću upravitelja resursa Azure**.

- Voditelj resursa sustavom virtualne mreže možete povezati s klasični virtualne mreže u kojima je omogućeno Azure servisa Active Directory Domain Services. Nakon toga, možete koristiti Azure servisa Active Directory Domain Services u utemeljen na resursima virtualne mreže. Dodatne informacije potražite u odjeljku [veza s mrežom](active-directory-ds-networking.md#network-connectivity) .

- **Regionalne virtualne mreže**: Ako namjeravate koristiti postojeće virtualne mreže jesu li regionalne virtualne mreže.

    - Virtualna mreže koje koriste mehanizam naslijeđene afinitet grupe nije moguće koristiti s Azure servisa Active Directory Domain Services.

    - Korištenje servisa Azure AD domene [migrirati naslijeđene virtualne mreže regionalne virtualne mrežama](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).


### <a name="azure-region-for-the-virtual-network"></a>Azure područja za virtualne mreže

- Upravljani domena je uveden u istom Azure području kao virtualne mreže servisa Azure AD domene odaberite da biste omogućili uslugu u.

- Odaberite virtualne mreže programa Azure regija podržava Azure servisa Active Directory Domain Services.

- Posjetite stranicu [servisa Azure po regijama](https://azure.microsoft.com/regions/#services/) znati Azure regije u kojoj je dostupan Azure servisa Active Directory Domain Services.


### <a name="requirements-for-the-virtual-network"></a>Preduvjeti za virtualne mreže

- **Blizina vaše Azure radnih opterećenja**: Odaberite virtualne mrežu koju trenutno hostira/će hostira virtualnim strojevima potreban je pristup Azure servisa Active Directory Domain Services.

- **Prilagođeno/Premjesti-vaše-vlasnik DNS poslužitelji**: provjeriti postoje li nisu prilagođene konfigurirani DNS poslužitelji za virtualne mreže.

- **Postojeći domene s istim nazivom domene**: bili sigurni da neće imati postojeću domenu s istim nazivom domene dostupan na tom virtualne mreže. Na primjer, pretpostavimo imate domenu pod nazivom "contoso.com" već dostupna na odabrani virtualne mreže. Naknadno pokušate omogućiti Azure servisa Active Directory Domain Services upravljanih domene s isti naziv domene (to je "contoso.com") na tom virtualne mreže. Naiđete na pogrešku prilikom pokušaja omogućiti Azure servisa Active Directory Domain Services. To je zbog sukoba imena za naziv domene na tom virtualne mreže. U tom slučaju morate koristiti neki drugi naziv da biste postavili domenu upravljanih Azure servisa Active Directory Domain Services. Umjesto toga poništite Dodjela resursa za postojeću domenu, a zatim nastavite s omogućivanjem Azure servisa Active Directory Domain Services.

> [AZURE.WARNING] Nakon što ste omogućili uslugu ne možete premještati domenske servise različite virtualne mreže.


## <a name="network-security-groups-and-subnet-design"></a>Mrežni sigurnosne grupe i podmreže dizajna
[Mrežni sigurnosnih grupa (NSG)](../virtual-network/virtual-networks-nsg.md) sadrži popis pristup kontrola popisa (ACL) pravila koja dopustiti ili zabraniti mrežni promet na vaše VM instance u virtualne mreže. NSGs može biti pridruženi podmreže ili pojedinačne instance VM unutar tog podmreže. Kada je NSG je pridružen podmreži, ACL pravila primjenjuju se na sve instance VM u tom podmreže. Osim toga, promet za pojedinačne VM može biti ograničena daljnje povezivanjem programa NSG izravno u tom VM.

![Preporučena podmreže dizajna](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)


### <a name="best-practices-for-choosing-a-subnet"></a>Praktični savjeti za odabir podmreži
- Implementacija Azure servisa Active Directory Domain Services u **zasebnom namjenski podmreže** unutar Azure virtualne mreže.

- Primjena NSGs namjenski podmreže za upravljane domene. Ako morate zatvoriti NSGs namjenski podmreži, provjerite je li vam **blokirati priključke za usluge i upravljati domenom**.

- Ograničavanje pretjerano broja IP adrese dostupan u okviru namjenski podmreže za upravljane domene. To ograničenje onemogućuje dostupnost dva kontrolera domena za vašu domenu Upravljani servis.

- **Omogućivanje Azure AD domenske servise u podmreže pristupnik** virtualne mreže.


> [AZURE.WARNING] Kada povežete programa NSG s podmreže u kojem se Azure AD domene Services omogućena, poremetiti mogućnost Microsoftovih servisa i upravljanje domene. Uz to, sinkronizacija između klijentu za Azure AD i upravljane domene je nadziranje prekinuto. **Na SLA ne primjenjuju se na implementacijama gdje je NSG primijenjen koji blokira Azure servisa Active Directory Domain Services s ažuriranjem i upravljanje domenom.**


### <a name="ports-required-for-azure-ad-domain-services"></a>Priključci potrebne za Azure servisa Active Directory Domain Services
Sljedeće priključke su potrebni za Azure AD domenske servise za servisa i održavanje upravljanih domene. Provjerite je li da priključke su blokirane za podmreže u kojima ste omogućili upravljanih domene.

| Broj priključka | Svrha |
|---|---|
| 443 | Sinkronizacija s klijentom Azure AD |
| 3389 | Upravljanje domene |
| 5986 | Upravljanje domene |
| 636 | Sigurnog pristupa LDAP (LDAPS) upravljanih domene |



## <a name="network-connectivity"></a>Veza s mrežom
Upravljani domenom sustava Azure AD domenske servise mogu omogućiti samo unutar jedne mreže klasični virtualne u Azure. Virtualne mreže stvoren pomoću upravitelja resursa Azure nisu podržani.


### <a name="scenarios-for-connecting-azure-networks"></a>Scenariji za povezivanje Azure mreža
Povezivanje Azure virtualne mreže da biste koristili upravljanih domene u nekom od sljedećih situacija implementacije:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Korištenje upravljanih domene u više od jedne Azure na klasični virtualne mreže
Druge Azure klasični virtualne mreže možete povezati s Azure klasični virtualne mreže u kojima ste omogućili Azure servisa Active Directory Domain Services. Ovu VPN vezu omogućuje korištenje upravljanih domene s vašeg radnih opterećenja u upotrebi na drugim mrežama virtualne.

![Povezivanje Classic virtualne mreže](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Korištenje upravljanih domene u utemeljen na resursima virtualne mreže
Voditelj resursa sustavom virtualne mreže možete povezati s Azure klasični virtualne mreže u kojima ste omogućili Azure servisa Active Directory Domain Services. Ovu vezu omogućuje korištenje upravljanih domene za vaše radnih opterećenja u uveden u utemeljenu na resursima virtualne mreže.

![Voditelj resursa za povezivanje classic virtualne mreže](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)


### <a name="network-connection-options"></a>Mogućnosti za mrežne veze

- **VNet VNet veze pomoću web-mjesto VPN veza**: spajanje virtualne mreže na drugu virtualne mreže (VNet-VNet) je slična povezivanje virtualne mreže na mjesto lokalnog web-mjesta. Pristupnik za VPN obje vrste povezivanja koristite sigurne tunelom pomoću IPsec-IKE.

    ![Virtualna mrežna veza pomoću pristupnika VPN-a](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Dodatne informacije o - povezivanje virtualne mreže pomoću pristupnika VPN-a](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)


- **Veza za VNet VNet pomoću peering virtualne mreže**: peering virtualne mreže je mehanizam koja povezuje dvije virtualne mreže na istom području putem mreže Azure backbone. Kada peered, prikazuju se dvije virtualne mreže kao jedan svrhe sve povezivanjem. I dalje upravlja kao zasebna resursa, ali virtualnim strojevima u njima virtualne mogli komunicirati sa međusobno pomoću privatne IP adrese.

    ![Virtualna mrežne veze pomoću peering](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Dodatne informacije o - virtualne mreže peering](../virtual-network/virtual-network-peering-overview.md)



<br>

## <a name="related-content"></a>Srodni sadržaji

- [Peering Azure virtualne mreže](../virtual-network/virtual-network-peering-overview.md)

- [Konfiguriranje VNet VNet veze za uvođenje klasičnog model](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

- [Azure mreže sigurnosne grupe](../virtual-network/virtual-networks-nsg.md)
