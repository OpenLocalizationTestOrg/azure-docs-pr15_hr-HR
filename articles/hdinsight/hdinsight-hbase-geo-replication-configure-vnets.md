<properties 
   pageTitle="Konfiguriranje VPN veza između dvije virtualne mreže | Microsoft Azure" 
   description="Saznajte kako konfigurirati VPN veze i razlučivanje naziva domene između dvije Azure virtualne mreže, i konfiguriranju zemlj replikacije HBase." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-a-vpn-connection-between-two-azure-virtual-networks"></a>Konfiguriranje VPN veze između dvije Azure virtualne mreže  

> [AZURE.SELECTOR]
- [Konfiguriranje povezivanja s VPN-a](hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfiguriranje DNS-a](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfiguriranje HBase replikacije](hdinsight-hbase-geo-replication.md) 

Povezivanje web-mjesto za Azure virtualne mreže koristi pristupnik VPN omogućuju sigurno tunelom pomoću Ipsec-IKE. Na VNets može biti u različite pretplate i različite regije. VNet možete kombinirati čak i za VNet komunikacije s konfiguracijama više web-mjesta. Postoji nekoliko razloga za VNet za povezivanje VNet:

- Unakrsni zemlj zalihosti regija i prisutnosti za zemlj. 
- Regionalne više razina aplikacije s istaknuti odvajanja granice 
- Unakrsni pretplatu, komunikacije među tvrtke ili ustanove u Azure

Dodatne informacije potražite u članku [Konfiguriranje VNet VNet vezu](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). 

Da biste vidjeli na videozapis:

> [AZURE.VIDEO configure-the-vpn-connectivity-between-two-azure-virtual-networks]

Pomoću ovog praktičnog vodiča je dio [niza] [ hdinsight-hbase-replication] o stvaranju zemlj replikacije HBase. 

- Konfiguriranje VPN veze između dvije virtualne mreže (ovog praktičnog vodiča)
- [Konfiguriranje DNS-a za virtualne mreže][hdinsight-hbase-geo-replication-dns]
- [Konfiguriranje replikacije HBase zemlj.][hdinsight-hbase-geo-replication]

Na sljedećem su dijagramu ilustrira dvije virtualne mreže ćete stvoriti u ovom ćete praktičnom vodiču:

![HDInsight HBase replikacije virtualne mrežni dijagram][img-vnet-diagram]
 

##<a name="prerequisites"></a>Preduvjeti
Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Radne stanice s Azure PowerShell**.

    Prije pokretanja skripte komponente PowerShell Provjerite jeste li povezani u pretplatu Azure pomoću sljedeći cmdlet:

        Add-AzureAccount

    Ako imate više pretplata Azure, koristite sljedeći cmdlet da biste postavili trenutne pretplate:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


>[AZURE.NOTE] Servis za Azure nazive i nazive virtualnog računala mora biti jedinstvena. Naziv koji se koristi u ovom ćete praktičnom vodiču je Contoso-[naziv servisa VM Azure]-[EU-a / sad]. Na primjer, Contoso VNet EU je Azure virtualne mreže u podatkovnom centru Sjeverna Europa; Sad Contoso-DNS-a je DNS poslužitelj VM u podatkovnim centrom za istočnoazijske SAD-a. Morate se pojavljuju s vlastite nazive.
 

##<a name="create-two-azure-vnets"></a>Stvaranje dva VNets Azure



**Da biste stvorili virtualne mreže pod nazivom Contoso VNet EU u Sjevernoj Europi**

1.  Prijava na [Portal za Azure klasični][azure-portal].
2.  Kliknite **NOVO**, **MREŽNIM servisima**, **VIRTUALNE MREŽE**, **PRILAGOĐENE Stvori**.
3.  Unesite:

    - **Naziv**: Contoso-VNet-EU-a
    - **Lokacija**: Sjeverna Europa

        Pomoću ovog praktičnog vodiča koristi podatkovnim centrima Sjeverna Europa i Istočni SAD-a. Možete odabrati vlastite podatkovnim centrima.
4.  Unesite:

    - **DNS POSLUŽITELJ**: (ostavite prazno) 
    
        Trebat će vam vlastite DNS poslužitelj za razlučivanje imena u virtualne mreže. Dodatne informacije o kada koristite navedene za Azure naziv razlučivosti, kao i kada se koriste DNS poslužitelj potražite u članku [Razrješavanje naziva (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Upute za konfiguriranje razlučivanje naziva između VNets potražite u članku [Konfiguriranje DNS između dvije Azure virtualne mreže][hdinsight-hbase-dns].
  
    - **Konfiguriranje mjesta točke VPN-a**: (poništen)

        Točka mjesta ne odnosi na ovom scenariju.

    - **Konfiguriranje VPN-a web-mjesto**: (poništen)
    
        U podatkovnim centrom za istočnoazijske sad će konfigurirati VPN veza web-mjesto za Azure virtualne mreže.
5.  Unesite:

    -   **Adresa prostora POČETNI IP**: 10.1.0.0
    -   **CIDR PROSTOR na ADRESU**: / 16
    -   **IP podmreže 1 POČETNI**: 10.1.0.0
    -   **CIDR podmreže 1**: / 24

    Prostor adrese možete preklapa s sad virtualne mreže.  

**Da biste stvorili virtualne mreže pod nazivom Contoso VNet EU u Europi za regije zapada**

- Ponavljanje zadnje postupak s sljedeće vrijednosti:

    - **Naziv**: Contoso-VNet-hr
    - **Lokacija**: Istočni SAD-a
     
    - **DNS POSLUŽITELJ**: (ostavite prazno)
    - **Konfiguriranje mjesta točke VPN-a**: (poništen)
    - **Konfiguriranje VPN-a web-mjesto**: (poništen)
     
    - **Adresa prostora POČETNI IP**: 10.2.0.0
    - **CIDR PROSTOR na ADRESU**: / 16
    - **IP podmreže 1 POČETNI**: 10.2.0.0
    - **CIDR podmreže 1**: / 24

















##<a name="configure-a-vpn-connection-between-the-two-vnets"></a>Konfiguriranje VPN veze između dva VNets

###<a name="create-local-networks"></a>Stvaranje lokalne mreže

Kada stvorite VNet VNet konfiguraciju, morate konfigurirati svaki VNet za prepoznavanje međusobno kao web-mjesto lokalne mreže. U ovom odjeljku ćete konfigurirati svaki VNet kao lokalne mreže. Lokalne mreže zajednički koristiti iste razmake IP adresa s odgovarajuće VNet.

![Konfiguriranje Azure VPN-a web-mjesto konfiguracija – azure lokalne mreže][img-vnet-lnet-diagram]


**Da biste stvorili lokalne mreže pod nazivom Contoso EU-LNet-a koji se podudaraju Contoso VNet EU mreže adresnog prostora**

1. Na portalu klasični Azure kliknite **NOVO**, **MREŽNIM servisima**, **VIRTUALNE MREŽE**, **Dodajte LOKALNOJ mreži**.
3. Unesite:

    - **Naziv**: Contoso-LNet-EU-a
    - **IP adresa za VPN UREĐAJ**: 192.168.0.1 (tu adresu ažurirat će se i noviji)

        Obično, koristit ćete stvarni vanjske IP adrese za VPN uređaj. Za VNet do VNet konfiguracija će koristiti VPN pristupnik IP adresa. Given da niste stvorili pristupnika VPN-a za dva VNets još, unesite IP adresu arbitary i vratite se na alata za popravak.
4.  Unesite:

    - **Adresa prostora POČETNI IP:** 10.1.0.0
    - **CIDR prostora na ADRESU:** /16
    
    To morate točno odgovarati raspon koji ste prethodno naveli za Contoso VNet EU.

**Da biste stvorili lokalne mreže pod nazivom Contoso-LNet-hr Contoso-VNet-hr mreže adresnog prostora koji se podudaraju**

- Ponavljanje zadnje postupak sa sljedećih parametara:

    - **Naziv**: Contoso-LNet-hr
    - **IP adresa za VPN UREĐAJ**: 192.168.0.1 (tu adresu ažurirat će se i noviji)
     
    - **Adresa prostora POČETNI IP**: 10.2.0.0
    - **CIDR PROSTOR na ADRESU**: / 16


###<a name="create-vpn-gateways"></a>Stvaranje pristupnika VPN-a

U ovoj konfiguraciji imaju dva dijela. Prvo konfigurirati vezu web-mjesto VNet lokalne mreže, a zatim stvorite dinamičke usmjeravanje VPN-a. VNet za VNet potreban je Azure VPN pristupnika s dinamički usmjeravanje VPN-ovima. Azure statične usmjeravanje VPN-ovi nisu podržani.

**Konfiguriranje veza web-mjesto tvrtke Contoso, VNet i EU Contoso-LNet-hr**

1.  Na portalu klasični Azure u lijevom oknu kliknite **MREŽA**
2.  Kliknite **Contoso VNet EU**.
3.  Kliknite karticu **CONFIGUE** .
4.  Potvrdite okvir **za povezivanje s lokalne mreže**.
5.  U **LOKALNOJ mreži**, odaberite **Contoso-LNet-hr**.
6.  Kliknite **Dodaj pristupnika podmreže** u odjeljku virtualne mreže adresa razmake.
7.  Kliknite **SPREMI**.
8.  Kliknite **u redu** da biste potvrdili.


**Da biste stvorili pristupnik VPN-a za Contoso VNet EU**

1.  Na portalu klasični Azure karticu **nadzorne PLOČE** .
4.  Kliknite **Stvaranje PRISTUPNIKA** na dno stranice, a zatim **Dinamički usmjeravanja**.
5.  Kliknite **da** da biste potvrdili. Obratite pozornost na to grafiku pristupnika na stranici mijenja žutu i piše stvaranje pristupnika. Traje otprilike 15 minuta da biste stvorili pristupnik.

    Kada je stanje pristupnika Promijeni povezivanje, IP adresa za svaki pristupnik bit će vidljivi na nadzornoj ploči. Zapišite IP adresu koja odgovara za svaku VNet Pobrinite se da nećete ih kombinirati prema gore. To su IP adrese koja će se koristiti prilikom uređivanja IP adresama rezervirano mjesto za uređaj VPN-a u lokalne mreže.

6.  Napravite njezinu kopiju **PRISTUPNIK IP adresa**. Će vam trebati da biste konfigurirali VPN pristupnik IP adresa za Contoso-VNet-EU-a u sljedećem odjeljku.

**Da biste stvorili pristupnik VPN-a za Contoso VNet EU**

- Ponovite postupak zadnje dvije za konfiguriranje povezivanja s web-mjesto tvrtke Contoso-VNet-hr Contoso LNet EU i stvaranje pristupnika za VPN-a za Contoso-Vnet-hr. Kada završite, imat ćete VPN pristupnik IP adresa za Contoso-VNet-hr.


### <a name="set-the-vpn-device-ip-addresses-for-local-networks"></a>Postavljanje uređaja VPN-a IP adrese za lokalne mreže
U odjeljku zadnju stvaranje pristupnika za VPN-a za svaki unos u VNets. Imate dobio IP adresa pristupnika VPN-a. Sada možete vratiti da biste konfigurirali lokalne mreže VPN uređaj IP adrese.

**Da biste konfigurirali VPN uređaj IP adresa za Contoso LNet EU** 

1.  Na portalu klasični Azure u lijevom oknu kliknite **MREŽAMA** .
2.  Kliknite **LOKALNE MREŽE** s vrha.
3.  Kliknite **Contoso LNet EU**, a zatim kliknite **Uredi** na dnu.
4.  Ažurirajte **VPN UREĐAJ IP adresa**.  To je adresa koju dohvaćate s kartica nadzorna PLOČA Contoso VNET EU.
5.  Kliknite gumb desno.
6.  Kliknite gumb Provjeri.

**Da biste konfigurirali VPN uređaj IP adresa za Contoso-LNet-hr** 

- Ponovite zadnju postupak da biste konfigurirali VPN uređaj IP adresa za Contoso-LNet-hr.

###<a name="set-vnet-gateway-keys"></a>Postavljanje VNet pristupnika tipke

Pristupnik Vnet koriste zajednički ključ za provjeru autentičnosti veza između virtualne mreže. Ključ ne može konfigurirati na portalu klasični Azure. Morate koristiti PowerShell i .NET SDK.

**Da biste postavili tipki**

1. Iz vaše radne stanice, otvorite **Windows Očisti filtar** ili konzole za Windows PowerShell.
2. Ažurirajte parametara u ovu pratite skriptu i pokrenite je:

        Add-AuzreAccount
        Select-AzureSubscription -[AzureSubscriptionName]
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-EU -LocalNetworkSiteName Contoso-LNet-US -SharedKey A1b2C3D4
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-US -LocalNetworkSiteName Contoso-LNet-EU -SharedKey A1b2C3D4 


##<a name="check-the-vpn-connection"></a>Provjerite vezu VPN-a 

Bez sve VMs u uveden u VNets možete koristiti vizualne dijagram virtualne mreže stranice nadzorne ploče VNet na portalu klasični Azure da biste provjerili stanje veze:

![HDInsight HBase replikacije virtualne mreže stanje veze VPN-a][img-vpn-status]
  



##<a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču ste naučili kako konfigurirati VPN veza između dvije Azure virtualne mreže. Ostali dva članci u nizu pokrivaju:

- [Konfiguriranje DNS-a između dvije Azure virtualne mreže][hdinsight-hbase-geo-replication-dns]
- [Konfiguriranje replikacije HBase zemlj.][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com

[powershell-install]: ../install-configure-powershell.md



[hdinsight-hbase-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-dns]: hdinsight-hbase-geo-replication-configure-dns.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-diagram.png
[img-vnet-lnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-lnet-diagram.png
[img-vpn-status]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-status.png 
