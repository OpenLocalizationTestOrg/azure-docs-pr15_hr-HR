<properties 
   pageTitle="O uređajima VPN-a za web-mjesto VPN pristupnika veze za Azure virtualne mreže | Microsoft Azure"
   description="U ovom se članku govori o VPN uređaji i IPsec parametara za pristupnik za VPN S2S veze i sadrži veze na upute za konfiguriranje i uzorka."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/13/2016"
   ms.author="yushwang;cherylmc" />

# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>O uređajima VPN-a za web-mjesto VPN pristupnika veze

Da biste konfigurirali web-mjesta web-na-mjesta (S2S) VPN vezu potreban je VPN uređaja. Veze web-mjesto koje se može koristiti za stvaranje rješenja hibridnog ili kad god želite sigurnu vezu između lokalne mreže i virtualne mreže. U ovom se članku govori o kompatibilnim uređajima VPN-a i konfiguracija parametara. 

>[AZURE.NOTE] Prilikom konfiguriranja veze web-mjesto za uređaj VPN-a potreban je na IPv4 IP adresu dostupnom javnosti.                                                                                                                                                                               

Ako vaš uređaj ne prikazuje u tablici [uređaja potvrđuju VPN](#devicetable) , u odjeljku [uređaji za osobe koje nisu provjeriti VPN](#additionaldevices) ovog članka. Moguće je možda li uređaj i dalje raditi s Azure. Podrška za uređaje VPN, zatražite od proizvođača uređaja.

**Imajte na umu prilikom pregledavanja tablice stavke:**

- Došlo je promjena terminologija za statički i dinamički usmjeravanje. Vjerojatno ćete naići oba uvjeta. Postoji isto kao i funkcija, samo nazive koji želite promijeniti.
    - Statički usmjeravanje = PolicyBased
    - Dinamični usmjeravanje = RouteBased
- Specifikacije za pristupnik za VPN visoke performanse i RouteBased VPN pristupnika isti su osim ako drugačije. Ako, na primjer, provjerene VPN uređajima koji su kompatibilni s RouteBased VPN pristupnika su i kompatibilan s pristupnika Azure visoke performanse VPN-a. 


## <a name="devicetable"></a>Uređaji provjerene VPN-a 

Ne možemo ste provjeriti valjanost skup standardne VPN uređaja u partnerstvo s dobavljačima uređaja. Svi uređaji linije uređaja koje se nalaze na popisu u nastavku surađivati s Azure VPN pristupnika. Potražite u članku [O pristupnika VPN-a](vpn-gateway-about-vpngateways.md) da biste provjerili vrstu pristupnika koje su vam potrebne za stvaranje rješenja želite konfigurirati. 

Da biste konfigurirali uređaju VPN, pogledajte veze koje odgovaraju liniju odgovarajući uređaj. Podrška za uređaje VPN, zatražite od proizvođača uređaja.



| **Dobavljača**                      | **Uređaj obitelj**                                        | **Minimalna verzija OS-a**                             | **PolicyBased**                                                                                                                                                                                                             | **RouteBased**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Allied Telesis                  | Očisti niz VPN usmjerivača                                    | 2.9.2                                              | dolazim brzo                                                                                                                                                                                                                                          | Nije kompatibilan                                                                                                                                                                                               |
| Barracuda mrežama, Inc.        | Barracuda NextGen vatrozid F-niza             | PolicyBased: 5.4.3, RouteBased: 6.2.0  | [Upute za konfiguriranje](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Upute za konfiguriranje](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda mrežama, Inc.        |  Barracuda NextGen vatrozid – niz X                 | Barracuda vatrozida 6.5 | [Barracuda vatrozid](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Nije kompatibilan                                                                                                                                                                                               |
| Brokatnim                         | Vyatta 5400 vRouter                                      | Virtualna usmjerivač 6.6R3 GA                            | [Upute za konfiguriranje](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Nije kompatibilan                                                                                                                                                                                               |
| Potvrdite točku                     | Pristupnik za sigurnost                                         | R75.40, R75.40VS                                     | [Upute za konfiguriranje](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Upute za konfiguriranje](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8,3                                                | [Uzorci Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Nije kompatibilan                                                                                                                                                                                               |
| Cisco                           | ASR                                                      | IOS 15.1 (PolicyBased) IOS 15.2 (RouteBased)                | [Uzorci Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Uzorci Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 15.0 (PolicyBased) IOS 15.1 (RouteBased *)               | [Uzorci Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Uzorci Cisco *](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | NetScaler MPX, SDX, VPX      |10,1 i noviji                                           | [Upute za integraciju](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html)                                                                                                                                                                            | Nije kompatibilan                                                                                                                                                                                               |
| Dell SonicWALL                  | TZ serije, NSA serije, SuperMassive, E – klase NSA | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Upute - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Upute - SonicOS 5,9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Upute - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Upute - SonicOS 5,9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| F5                              | VELIKI IP niza                                 |           12,0                                            | [Upute za konfiguriranje](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | [Upute za konfiguriranje](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling)                                                                                                                                                                                         |
| Fortinet                        | FortiGate                                                | FortiOS 5.2.7                                      | [Upute za konfiguriranje](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                                                          | [Upute za konfiguriranje](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                  |
| Internet inicijativa Japan (IIJ) | Niz SEIL                                              | SEIL / X 4.60 4.60 SEIL/B1 SEIL/x86 3.20            | [Upute za konfiguriranje](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Nije kompatibilan                                                                                                                                                                                               |
| Tvrtke Juniper                         | SRX                                                      | JunOS 10,2 (PolicyBased) JunOS 11,4 (RouteBased)            | [Uzorci tvrtke Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Uzorci tvrtke Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Tvrtke Juniper                         | J niza                                                 | JunOS 10.4r9 (PolicyBased) JunOS 11,4 (RouteBased)          | [Uzorci tvrtke Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Uzorci tvrtke Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Tvrtke Juniper                         | ISG                                                      | ScreenOS 6,3 (PolicyBased i RouteBased)                  | [Uzorci tvrtke Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Uzorci tvrtke Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Tvrtke Juniper                         | SSG                                                      | ScreenOS 6.2 (PolicyBased i RouteBased)                  | [Uzorci tvrtke Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Uzorci tvrtke Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| Microsoft                       | Usmjeravanje i daljinski pristup servisa                        | Windows Server 2012.                                | Nije kompatibilan                                                                                                                                                                                                                                       | [Microsoftovi uzorci](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Otvaranje AG sustavi | Zaštita njihove privatnosti ovise kontrola sigurnost pristupnika | N/D | [Vodič za instalaciju](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Vodič za instalaciju](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (Uskoro dostupno)                                                                                                                                                                                                                                        | Nije kompatibilan                                                                                                                                                                                               |
| Palo Alto mreža              | Svi uređaji operacijski sustav premještanje     | Premještanje OS 6.1.5 ili noviji (PolicyBased), premještanje OS 7.0.5 ili noviji (RouteBased)       | [Upute za konfiguriranje]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Upute za konfiguriranje](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| Watchguard                      | Sve                                                      | Fireware XTM v11.x                                 | [Upute za konfiguriranje](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Nije kompatibilan                                                                                                                                                                                               |

(*) Niz 7200 ISR usmjerivača podržavaju samo PolicyBased VPN-ovima.

## <a name="additionaldevices"></a>Osobe koje nisu provjeriti uređaji VPN-a

Ako ne vidite uređaj navedenih u tablici uređaja potvrđuju VPN, je i dalje može funkcionirati s vezom za web-mjesto. Provjerite zadovoljava li uređaj VPN minimalne uvjete navedene u odjeljku pristupni zahtjevi članka [O pristupnika VPN-a](vpn-gateway-about-vpngateways.md#gateway-requirements) . Uređaji minimalni zahtjevi za sastanak i surađivati s VPN pristupnika. Dodatne upute za podršku i konfiguraciji zatražite od proizvođača uređaja.


## <a name="editing-device-configuration-samples"></a>Uređivanje uzorka Konfiguracija uređaja

Nakon što preuzmete navedeni ogledni konfiguracije uređaj VPN-a, morat ćete promijeniti neke od vrijednosti u skladu s vizualnim postavke okruženju sustava. 

**Da biste uredili uzorka:**

1. Otvorite uzorka pomoću Blok za pisanje. 
1. Pretraživanje i sve nizove <*teksta*> zamijenite vrijednosti koji se odnose na vaše okruženje. Obavezno uključite < i >. Ako je naveden naziv, mora biti jedinstven naziv koji ste odabrali. Ako naredba ne funkcionira, obratite se dokumentaciji proizvođača uređaja.

| **Ogledni tekst**                | **Promijeni u**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | Odabrani naziv za objekt. Primjer: myOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | Odabrani naziv za objekt. Primjer: myAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | Odabrani naziv za objekt. Primjer: myAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | Odabrani naziv za objekt. Primjer: myIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | Odabrani naziv za objekt. Primjer: myIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Odredite raspon. Primjer: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Navedite masku. Primjer: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | Odredite raspon lokalnog. Primjer: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | Navedite lokalnog masku. Primjer: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | Ove informacije specifične za virtualne mreže i nalazi se na Portal za upravljanje kao **pristupnik IP adresa**. |
| &lt;SP_PresharedKey&gt;                | Ove informacije odnose na virtualne mreže i nalazi se na Portal za upravljanje kao upravljanje ključ.          |



## <a name="ipsec-parameters"></a>IPsec parametara

>[AZURE.NOTE] Iako se pristupnik za VPN Azure podržane su vrijednosti navedene u tablici u nastavku, trenutno ne postoji način za određivanje ili odaberite određenu kombinaciju s VPN pristupnika Azure. Morate navesti ograničenja s lokalnim VPN uređaja. Osim toga, morate škripac MSS pri 1350.

### <a name="ike-phase-1-setup"></a>Postavljanje IKE faza 1

| **Svojstvo**                                       | **PolicyBased** | **Pristupnik za RouteBased i standardni prikaz ili visoke performanse VPN-a** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| IKE verzija                                        | IKEv1                          | IKEv2                                                            |
| Grupa Diffie-Hellman                               | Grupa 2 (1024-bitni)             | Grupa 2 (1024-bitni)                                               |
| Način provjere autentičnosti                              | Zajednički ključ                 | Zajednički ključ                                                   |
| Algoritama šifriranja                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| Raspršivanje algoritam                                  | SHA1(SHA128)                   | SHA1(SHA128), SHA2(SHA256)                                                     |
| Faza 1 sigurnost vijek trajanja pridruživanje (SA) (vrijeme) | 28,800 sekundi                 | 10,800 sekundi                                                   |


### <a name="ike-phase-2-setup"></a>Postavljanje IKE faza 2

| **Svojstvo**                                                             | **PolicyBased**                 | **Pristupnik za RouteBased i standardni prikaz ili visoke performanse VPN-a**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| IKE verzija                                                              | IKEv1                                          | IKEv2                                                              |
| Raspršivanje algoritam                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| Faza 2 sigurnost vijek trajanja pridruživanje (SA) (vrijeme)                        | 3,600 sekundi                                  | 3,600 sekundi                                                                  |
| Faza 2 sigurnost pridruživanje (SA) života (propusnost)                  | 102,400,000 KB                                 | -                                                                  |
| IPsec Pacifička šifriranje i ponude za provjeru autentičnosti (obliku preferenca) | 1. ESP-AES256 2. ESP AES128 3. ESP 3DES 4. N/D | U odjeljku *nudi RouteBased pristupnika IPsec sigurnost pridruživanje (SA)* (dolje) |
| Očuvanje savršen naprijed (PFS)                                            | ne                                             | Nema (*)|
| Otkrivanje smrt ravnopravnih članova                                                      | Nije podržano                                  | Podržana                                                          |

(*) Azure pristupnika kao IKE odzivnik odgovarat PFS DH grupa 1, 2, 5, 14, 24.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>Nudi RouteBased pristupnika IPsec sigurnost pridruživanje (SA)

U sljedećoj su tablici navedeni IPsec Pacifička šifriranje i ponude za provjeru autentičnosti. Ponude su navedene redoslijeda preferenca ponudu moglo prikazivati ili Prihvati.

| **Šifriranje IPsec SA i ponuda za provjeru autentičnosti** | **Azure pristupnika kao pokretača**                               | **Azure pristupnika kao odzivnik**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | SHA ESP AES_256                                              | SHA ESP AES_128                                              |
| 2                                                 | SHA ESP AES_128                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | AH SHA1 s ESP AES_128 s null HMAC                      |
| 5                                                 | AH SHA1 s ESP AES_256 s null HMAC                      | SHA1 AH s ESP 3_DES s null HMAC                        |
| 6                                                 | AH SHA1 s ESP AES_128 s null HMAC                      | MD5 AH s ESP 3_DES s null HMAC, bez vijekom trajanja predloženo |
| 7                                                 | SHA1 AH s ESP 3_DES s null HMAC                        | AH SHA1 s ESP 3_DES SHA1, bez vijekom trajanja                    |
| 8                                                 | MD5 AH s ESP 3_DES s null HMAC, bez vijekom trajanja predloženo | AH MD5 s ESP 3_DES MD5, bez vijekom trajanja                     |
| 9                                                 | AH SHA1 s ESP 3_DES SHA1, bez vijekom trajanja                    | ESP DES MD5                                                  |
| 10                                                | AH MD5 s ESP 3_DES MD5, bez vijekom trajanja                     | ESP DES SHA1, bez vijekom trajanja                                   |
| 11                                                | ESP DES MD5                                                  | AH SHA1 s ESP DES null HMAC, bez vijekom trajanja predloženo        |
| 12                                                | ESP DES SHA1, bez vijekom trajanja                                   | AH MD5 s ESP DES null HMAC, bez vijekom trajanja predloženo        |
| 13                                                | AH SHA1 s ESP DES null HMAC, bez vijekom trajanja predloženo        | AH SHA1 s ESP DES SHA1, bez vijekom trajanja                      |
| 14                                                | AH MD5 s ESP DES null HMAC, bez vijekom trajanja predloženo        | AH MD5 s ESP DES MD5, bez vijekom trajanja                       |
| 15                                                | AH SHA1 s ESP DES SHA1, bez vijekom trajanja                      | ESP SHA, bez vijekom trajanja                                        |
| 16                                                | AH MD5 s ESP DES MD5, bez vijekom trajanja                       | ESP MD5, bez vijekom trajanja                                        |
| 17                                                | -                                                            | AH SHA, bez vijekom trajanja                                         |
| 18                                                | -                                                            | AH MD5, bez vijekom trajanja                                        |


- Možete navesti IPsec ESP NULL šifriranja s RouteBased i visoke performanse VPN pristupnik. Null na temelju šifriranje pružaju zaštitu s podacima na putu, a mogu koristiti samo kad je Maksimalna propusnost i minimalnu kašnjenje je potreban.  Klijenti možete koristiti u VNet VNet komunikacije scenariji ili kada na šifriranje je primijenjena negdje drugdje u rješenje.

- Da biste postigli više lokacija Povezivost putem Interneta, koristite zadane postavke pristupnika Azure VPN s šifriranja i raspršivanje algoritama navedene u tablicama iznad da biste bili sigurni sigurnosti vaše ključnih komunikacije.





