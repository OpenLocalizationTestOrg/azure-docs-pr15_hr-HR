<properties 
   pageTitle="Konfiguriranje točke web VPN veze pristupnika virtualne mrežom pomoću modela implementaciju upravljanja resursima | Microsoft Azure"
   description="Sigurno povezivanje s mrežom virtualne Azure stvaranjem točke web VPN veza pristupnika."
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
   ms.date="10/17/2016"
   ms.author="cherylmc" />

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Konfiguriranje veze točke web VNet pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Voditelj resursa – Portal za Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Voditelj resursa – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasični – Portal za Azure](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Konfiguraciju točke-na-web-mjesta (P2S) omogućuje stvaranje sigurnu vezu s pojedinačne klijentskog računala da biste virtualne mreže. Povezivanje P2S je korisno kada želite povezati svoje VNet s udaljenog mjesta, kao što su od kuće ili konferencije, ili kada imate samo nekoliko klijenti koje morate se povezati s virtualne mreže. 

Točke web veze ne zahtijevaju VPN uređaja ili dostupnog javnosti IP adresa za rad. Pokretanjem povezivanja s klijentskom računalu je uspostavljena VPN veza. Dodatne informacije o vezama točke mjesta potražite u članku [Najčešća pitanja vezana uz VPN pristupnika](vpn-gateway-vpn-faq.md#point-to-site-connections) i [Planiranje i dizajna](vpn-gateway-plan-design.md). 

U ovom se članku vodit će vas voditi kroz stvaranje na VNet s vezom za točke na mjesto u modelu implementaciju upravljanja resursima pomoću komponente PowerShell.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Uvođenje modela i postupci za P2S veze

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Sljedeća tablica prikazuje dva implementacije modelima i dostupne načinima za P2S konfiguracije. Kada članak s navedeni koraci za konfiguraciju postane dostupan, ne možemo povezati izravno iz u ovoj su tablici.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Osnovni tijek rada 

![Točke-na-dijagram web-mjesta –] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "točka-mjesta")

U ovom slučaju morate stvoriti virtualne mreže s vezom za točke na web. Upute i pomoći će vam stvaranje certifikata koji su potrebni za tu konfiguraciju. Povezivanje P2S sastoji se od sljedećih stavki: A VNet s pristupnika VPN, u korijen .cer datoteka certifikata (javni ključ), potvrdu klijenta i konfiguracija VPN-a na klijentskom računalu. 

Koristite sljedeće vrijednosti za tu konfiguraciju. Ne možemo postaviti varijabli u sekcija [1](#declare) članka. Možete ili kao u walk-through, slijedite korake i koristiti vrijednosti bez promjene ih ili ih u skladu s vizualnim okruženju sustava. 

### <a name="example"></a>Primjer vrijednosti

- **Naziv: VNet1**
- **Adresa prostora: 192.168.0.0/16** i **10.254.0.0/16**<br>U ovom primjeru koristimo prostor više od jedne adrese da biste ilustrirali tu konfiguraciju funkcioniraju s više adresa razmake. Međutim, višestruki razmaci adrese nisu potrebni za tu konfiguraciju.
- **Naziv podmreže: FrontEnd**
    - **Raspon adresu podmreže: 192.168.1.0/24**
- **Naziv podmreže: pozadinskog**
    - **Raspon adresu podmreže: 10.254.1.0/24**
- **Naziv podmreže: GatewaySubnet**<br>Naziv podmreže *GatewaySubnet* je obavezna za pristupnik za VPN-a za rad.
    - **Raspon adresu podmreže: 192.168.200.0/24** 
- **Skupna adresa za VPN klijentskog: 172.16.201.0/24**<br>Klijenti VPN-a koji se povezuju sa VNet koristite ovu vezu točke web primiti IP adresu skupna adresa klijenta za VPN-a.
- **Pretplate:** Ako imate više pretplata, provjerite koristite li ispravnu.
- **Grupa resursa: TestRG**
- **Lokacija: Istočni SAD-a**
- **DNS poslužitelj: IP adresa** DNS poslužitelja koji želite koristiti za razlučivanje naziva.
- **Naziv GW: Vnet1GW**
- **Javni naziv IP: VNet1GWPIP**
- **VpnType: RouteBased**

## <a name="before-beginning"></a>Prije početka

- Provjerite je li Azure pretplate. Ako već nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ili znak prema gore za [besplatan račun](https://azure.microsoft.com/pricing/free-trial/).
    
- Instalirajte najnoviju verziju programa Azure resursima PowerShell cmdleti. Dodatne informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) . Prilikom rada sa servisom PowerShell za tu konfiguraciju, provjerite je li da se izvodi kao administrator. 

## <a name="declare"></a>Dio 1 - zapisnika u i postavljanje varijable

U ovom ćete odjeljku prijavite i deklarirati vrijednosti koji se koriste za tu konfiguraciju. Declared vrijednosti se koriste u ogledne skripte. Promijenite vrijednosti u skladu s vizualnim vlastitu okruženju. Ili možete koristiti declared vrijednosti, a zatim poduzmite korake kao za vježbu.

1. Na konzoli za PowerShell prijavite se na račun za Azure. Ovaj cmdlet traži vjerodajnice za prijavu za vaš račun za Azure. Nakon prijave u, ona preuzima postavki računa tako da su dostupni za Azure PowerShell.

        Login-AzureRmAccount 

2. Dobit ćete popis pretplate Azure.

        Get-AzureRmSubscription

3. Navedite pretplatu u koju želite koristiti. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Deklariranje varijabli koje želite koristiti. Koristite sljedeće ogledne zamjena vrijednosti za vlastitu kada je to potrebno.

        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"
        
## <a name="ConfigureVNet"></a>Dio 2 – konfiguriranje na VNet 

1. Stvorite grupu resursa.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. Stvaranje konfiguracije podmreže za virtualne mreže imenovanja *FrontEnd*, *pozadinskog sustava*i *GatewaySubnet*. Ove prefiksi mora biti dio prostor VNet adrese koje ste deklarirane.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. Stvaranje virtualne mreže. Navedeni DNS poslužitelja mora biti DNS poslužitelj koji može odrediti imena za resurse povezujete. U ovom se primjeru koristi javnu IP adresa. Ne zaboravite pomoću vlastitih vrijednosti.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Navedite varijable za virtualne mreže koju ste stvorili.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Zahtjev za dinamično dodijeljena javnu IP adresa. U ovom IP adresa nije potrebno pristupnika ispravno funkcionirao. Naknadno povezati pristupnik IP konfiguracije pristupnika.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="Certificates"></a>Dio 3 – certifikata

Azure koristi certifikata za provjeru autentičnosti klijenti VPN-a za VPN-ovi zareza web. Javni certifikat podatke (ne privatni ključ) izvesti kao Base-64 kodirana X.509 .cer datoteka s korijenskim certifikatom generira rješenje certifikat za enterprise ili korijenski samopotpisani certifikat. Zatim uvoza podaci javni certifikat iz korijenski certifikat za Azure. Uz to, morate generiranje potvrdu klijenta iz korijenski certifikat za klijente. Svaki klijent koji želi povezati virtualne mreže pomoću P2S veze, morate imati instaliran certifikat klijenta koji je generirao korijenski certifikat.

### <a name="cer"></a>1. nabaviti .cer datoteka za korijenski certifikat

Morat ćete Pribavljanje podataka javni certifikat za korijenski certifikat koji želite koristiti.

- Ako koristite u sustavu za potvrdu enterprise, nabavite .cer datoteka za korijenski certifikat koji želite koristiti. 

- Ako se ne koriste rješenje certifikat za enterprise, morate stvoriti korijenski samopotpisani certifikat. Upute za Windows 10, može se odnositi na [Rad s samopotpisani korijenskih certifikata za konfiguracije točke na web](vpn-gateway-certificates-point-to-site.md).


1. Da biste nabavili .cer datoteka iz certifikat, otvorite **certmgr.msc** i pronađite korijenski certifikat. Desnom tipkom miša kliknite korijenski samopotpisani certifikat, kliknite **Svi zadaci**, a zatim kliknite **Izvoz**. Otvara se **Čarobnjak za izvoz certifikata**.

2. U čarobnjaku kliknite **Dalje**, odaberite **ne, izvezi privatni ključ**i zatim kliknite **Dalje**.

3. Na stranici **Oblik datoteke za izvoz** odaberite **osnova 64 kodirati X.509 (. CER).** Zatim kliknite **Dalje**. 

4. Na u **datoteka za izvoz**, **Pronađite** mjesto na koje želite izvesti certifikata. **Naziv datoteke**, naziv datoteka certifikata. Zatim kliknite **Dalje**.

5. Kliknite **Završi** da biste izvezli certifikata.

### <a name="generate"></a>2. generiranje certifikat klijenta

Nakon toga generirati potvrde klijenta. Ili možete stvoriti jedinstvene certifikat za svaki klijent koji će se povezati ili pomoću istog certifikata za druge klijente. Prednost generiranje jedinstvenih klijentskih potvrda je mogućnost da biste opozvali jedan certifikat prema potrebi. U suprotnom, ako svi koriste isti certifikat klijenta i pronaći ćete morati opoziv certifikata za jednog klijenta, morat ćete generiranje i instalirajte novog certifikata za sve klijente koji koriste certifikat za provjeru autentičnosti. Klijent certifikati su instalirani na svakom klijentskom računalu u nastavku ovu vježbu.

- Ako koristite rješenje certifikat za enterprise, generirati potvrdu klijenta s uobičajenih oblika vrijednost za naziv 'name@yourdomain.com', umjesto NetBIOS "Domena\korisničkoime" Oblikovanje. 

- Ako koristite samopotpisani certifikat, potražite u članku [Rad s samopotpisani korijenskih certifikata za konfiguracije točke web](vpn-gateway-certificates-point-to-site.md) da biste generirali potvrdu klijenta.

### <a name="exportclientcert"></a>3. Izvoz certifikat klijenta

Klijentski certifikat je potreban za provjeru autentičnosti. Nakon klijentski certifikat za generiranje izvesti. Klijentski certifikat izvezete instalirat će se kasnije na svakom klijentskom računalu.

1. Da biste izvezli potvrdu klijenta, možete koristiti *certmgr.msc*. Desnom tipkom miša kliknite klijentski certifikat koji želite izvesti, kliknite **Svi zadaci**, a zatim **Izvoz**.
2. Izvoz certifikat klijenta s privatni ključ. Ovo je *.pfx* datoteka. Provjerite je li zapis ili sjetiti lozinke (tipka) koju ste postavili za taj certifikat.

### <a name="upload"></a>4. prijenos u korijen .cer datoteka certifikata

Deklariranje varijablu naziv certifikata, vrijednost zamjenjuje vlastiti:

        $P2SRootCertName = "Mycertificatename.cer"

Dodavanje podataka javni certifikat za korijenski certifikat Azure. Možete prenijeti datoteke veličine do 20 korijenskih certifikata. Privatni ključ za korijenski certifikat prenesite ne Azure. Nakon prijenosa .cer datoteka Azure je koristi za provjeru autentičnosti klijenti koji se povezuju sa virtualne mreže. 

Put datoteke zamijeniti vlastitim, a zatim pokrenite Cmdlete.
    
        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="creategateway"></a>Dio 4 – stvaranje pristupnika za VPN-a

Konfiguriranje i stvaranje pristupnika virtualne mreže za vaše VNet. *-GatewayType* mora biti **VPN-a** i *-VpnType* mora biti **RouteBased**. To može potrajati i do 45 minuta.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="clientconfig"></a>Dio 5 - preuzimanje paketa za konfiguriranje klijenta VPN-a

Klijenti za povezivanje s Azure pomoću P2S mora imati potvrdu klijenta i paket VPN klijentskog konfiguracije instaliran. VPN klijentskog konfiguracije paketa dostupni su za klijente sustava Windows. Paket za VPN klijentskog sadrži informacije da biste konfigurirali VPN klijentski softver koji je ugrađena u Windows i specifičnu za VPN-a koji želite povezati. Paket instalirati dodatnog softvera. [Najčešća pitanja vezana uz VPN pristupnika](vpn-gateway-vpn-faq.md#point-to-site-connections) dodatne informacije potražite u članku.

1. Nakon stvaranja pristupnika možete preuzeti paket konfiguracije klijenta. U ovom se primjeru preuzimanje paketa za 64-bitni klijente. Ako želite preuzeti klijent za 32-bitne "Amd64" zamijenite 'x86'. VPN klijent možete preuzeti i pomoću portala za Azure.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. Cmdlet ljuske PowerShell vraća URL veze. Ovo je primjer koje vraćeni URL izgleda kao što su:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Kopirajte i zalijepite vezu pomoću koje se vraćaju u web-preglednik da biste preuzeli paket. Zatim instalirajte paket na klijentskom računalu. Ako se pojavi skočni prozor SmartScreen, kliknite **Dodatne informacije**, **svejedno izvesti** da biste instalirali paket.

4. Na klijentskom računalu, idite na **Postavke mreže** , a zatim kliknite **VPN-a**. Prikazat će se veza na popisu. Ona će se prikazati naziv virtualne mreže koji će se povezati s i izgleda slično kao u ovom primjeru: 

    ![VPN klijent] (./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN klijent")


## <a name="clientcertificate"></a>Dio 6 – instalacija certifikat klijenta

Svako klijentsko računalo mora imati klijentski certifikat za provjeru autentičnosti. Prilikom instaliranja certifikat klijenta, morate lozinku stvorenu prilikom izvoza certifikat klijenta.

1. Kopirajte .pfx datoteka na klijentskom računalu.
2. Dvokliknite .pfx datoteka da biste ga instalirali. Izmjena mjesto instalacije.

## <a name="connect"></a>Dio 7 – povezati Azure

1. Za povezivanje s VNet, na klijentskom računalu otvorite VPN vezama i pronađite VPN vezu koju ste stvorili. To se naziva isti naziv kao virtualne mreže. Kliknite **Poveži**. Na skočnom može se pojaviti poruka koja se odnosi na pomoću certifikata. Ako se to dogodi, kliknite **Nastavi** da biste koristili dodatnim ovlastima. 

2. Na stranici stanja **veze** kliknite **Poveži** da biste pokrenuli vezu. Ako se prikaže zaslon za **Odabir certifikata** , provjerite je li certifikat klijenta koji prikazuje onaj koji želite koristiti za povezivanje. Ako nije, pomoću strelice padajućeg izbornika odaberite odgovarajući certifikat, a zatim kliknite **u redu**.

    ![VPN klijentskog veze] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN klijentskog veze")

3. Sada moraju uspostaviti vezu.

    ![Uspostaviti vezu] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Uspostaviti vezu")

## <a name="verify"></a>Dio 8 - provjerite je li vaša veza

1. Da biste provjerili je li VPN veza aktivna, otvorite povećane naredbeni redak i pokrenite *ipconfig/sve*.

2. Prikaz rezultata. Obratite pozornost na to da je IP adresa koji ste primili jedan od adrese unutar točke na web adresu skup klijenta za VPN-a koji ste naveli u konfiguraciji. Rezultati moraju biti nešto slično sljedećemu:
    
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="addremovecert"></a>Da biste dodali ili uklonili pouzdani korijenski certifikat

Certifikati koriste se za provjeru autentičnosti klijenti VPN-a za VPN-ovi zareza web. Sljedeći koraci vode kroz Dodavanje i uklanjanje korijenskih certifikata. Prilikom dodavanja datoteke Base64 kodirani X.509 (.cer) Azure su koja upozorava Azure pouzdanosti korijenski certifikat koji predstavlja datoteku. 

Možete dodati ili ukloniti pouzdanih korijenskih certifikata pomoću programa PowerShell ili na portalu za Azure. Ako želite da biste to učinili pomoću portala za Azure, idite na vaše **virtualne mreže pristupnika > Postavke > konfiguracija točke stranice > korijenski certifikati**. Sljedeći koraci će vas voditi kroz sljedeće zadatke pomoću komponente PowerShell. 

### <a name="add-a-trusted-root-certificate"></a>Dodavanje pouzdanom korijenu certifikata

Možete dodati do 20 pouzdani korijenski certifikat .cer datoteke Azure. Slijedite korake u nastavku da biste dodali s korijenskim certifikatom.

1. Stvaranje i Priprema novi korijenski certifikat koji ćete dodati Azure. Izvoz javni ključ kao Base-64 kodirani X.509 (. CER) i otvorite uređivač teksta. Zatim kopirajte samo u odjeljku prikazano u nastavku. 
 
    Kopirajte vrijednosti, kao što je prikazano u sljedećem primjeru:

    ![certifikat] (./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "certifikat")
    
2. Navedite naziv certifikata i podaci o ključu kao varijabla. Zamijenite podatke vlastitih, kao što je prikazano u nastavku primjer:

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Dodajte novi korijenski certifikat. Istovremeno možete dodati samo jednu certifikata.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Možete provjeriti da novi certifikat pravilno dodani pomoću sljedeći cmdlet.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Da biste uklonili pouzdani korijenski certifikat

Pouzdani korijenski certifikat možete ukloniti iz Azure. Prilikom uklanjanja pouzdana ustanova klijentskih potvrda koje su stvorene iz korijenski certifikat više moći povezati Azure putem točke na web. Ako želite da se klijentima omogućuje povezivanje, koje su im potrebne da biste instalirali novi klijent certifikat koji je generirao certifikat pouzdanim Azure.

1. Da biste uklonili pouzdani korijenski certifikat, izmijenite sljedeći primjer:

    Deklariranje varijabli.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Uklanjanje certifikata.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Koristite sljedeći cmdlet da biste provjerili uspješno ukloniti certifikata. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="revoke"></a>Da biste upravljali popisom pravilnika klijent opozvanih certifikata

Možete opozvati klijentskih potvrda. Popisi opozvanih certifikata omogućuje selektivno odbiti točke web povezivanje koji se temelji na pojedinačne klijentskih potvrda. Ako uklonite .cer za potvrdu korijenski iz Azure, opoziva pristupa za sve klijentskih potvrda generira/prijavljeni po opozvanih korijenski certifikat. Ako želite opozvati certifikat za određeni klijent, ne korijenu, to možete učiniti. Na taj način druge potvrde koje su stvorene iz korijenski certifikat i dalje će biti valjane.

Praktična obuka za zajednički je koristiti korijenski certifikat za upravljanje pristupom na razinama tima ili organizacije tijekom korištenja klijent opozvanih certifikata za kontrolu pristupa preciznije na pojedinačnim korisnicima.

### <a name="revoke-a-client-certificate"></a>Oduzimanje potvrdu klijenta

1. Pronađite otisak prsta klijentsku potvrdu da biste opozvali.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Dodajte na otisak prsta na popis opozvanih otisak prsta.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Provjerite je li na otisak prsta dodan popisi opozvanih certifikata. Morate dodati jedan otisak prsta odjednom.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Ponovna aktivacija potvrdu klijenta

Klijentski certifikat možete obnoviti u uklanjanjem na otisak prsta na popisu klijenta opozvanih certifikata.

1.  Uklanjanje na otisak prsta s popisa otisak prsta klijent opozvanih certifikata.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Potvrdite okvir ako su na otisak prsta ukloniti s popisa opozvanih.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Daljnji koraci

Virtualne mreže možete dodati virtualnog računala. Upute potražite u članku [Stvaranje virtualnog računala](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .