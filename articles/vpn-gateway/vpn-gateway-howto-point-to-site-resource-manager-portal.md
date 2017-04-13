<properties 
   pageTitle="Konfiguriranje točke web VPN veze pristupnika virtualne mrežu model implementacije Voditelj resursa i Azure portal | Microsoft Azure"
   description="Sigurno povezivanje s mrežom virtualne Azure stvaranjem točke web VPN veza pristupnika pomoću upravitelja resursa i Azure portal."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfiguriranje veze točke web VNet pomoću portala za Azure

> [AZURE.SELECTOR]
- [Voditelj resursa – Portal za Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Voditelj resursa – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasični – Portal za Azure](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Konfiguraciju točke-na-web-mjesta (P2S) omogućuje stvaranje sigurnu vezu s pojedinačne klijentskog računala da biste virtualne mreže. Povezivanje P2S je korisno kada želite povezati svoje VNet s udaljenog mjesta, kao što su od kuće ili konferencije, ili kada imate samo nekoliko klijenti koje morate se povezati s virtualne mreže. 

Točke web veze ne zahtijevaju VPN uređaja ili dostupnog javnosti IP adresa za rad. Pokretanjem povezivanja s klijentskom računalu je uspostavljena VPN veza. Dodatne informacije o vezama točke mjesta potražite u članku [Najčešća pitanja vezana uz VPN pristupnika](vpn-gateway-vpn-faq.md#point-to-site-connections) i [Planiranje i dizajna](vpn-gateway-plan-design.md).

U ovom se članku vodit će vas voditi kroz stvaranje na VNet s vezom za točke na mjesto u modelu implementaciju upravljanja resursima pomoću portala za Azure.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Uvođenje modela i postupci za P2S veze

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Sljedeća tablica prikazuje dva implementacije modelima i dostupne načinima za P2S konfiguracije. Kada članak s navedeni koraci za konfiguraciju postane dostupan, ne možemo povezati izravno iz u ovoj su tablici.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Osnovni tijeka rada 

![Točke-na-dijagram web-mjesta –] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "točka-mjesta")

### <a name="example"></a>Primjer vrijednosti

- **Naziv: VNet1**
- **Adresa prostora: 192.168.0.0/16**<br>U ovom primjeru koristimo prostor samo jedan adrese. Imate više od jedne adrese prostor za vaše VNet.
- **Naziv podmreže: sučelju**
- **Raspon adresu podmreže: 192.168.1.0/24**
- **Pretplate:** Ako imate više pretplata, provjerite koristite li ispravnu.
- **Grupa resursa: TestRG**
- **Lokacija: Istočni SAD-a**
- **GatewaySubnet: 192.168.200.0/24**
- **Naziv pristupnika virtualne mreže: VNet1GW**
- **Vrsta pristupnika: VPN-a**
- **Vrsta VPN: usmjeravanje-poštu**
- **Javnu IP adresu: VNet1GWpip**
- **Vrsta veze: točku mjesta**
- **Skupna adresa klijenta: 172.16.201.0/24**<br>Klijenti VPN-a koji se povezuju sa VNet koristite ovu vezu točke web primiti IP adresu skupna adresa klijenta.

## <a name="before-beginning"></a>Prije početka

- Provjerite je li Azure pretplate. Ako već nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ili znak prema gore za [besplatan račun](https://azure.microsoft.com/pricing/free-trial/).
    
## <a name="createvnet"></a>Dio 1 – stvaranje virtualne mreže

Ako stvarate tu konfiguraciju kao za vježbu, možete se referirati na [primjer vrijednosti](#example).

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

### <a name="2-add-additional-address-space-and-subnets"></a>2. dodati prostor dodatne adrese i podmreže

Možete dodati prostor dodatne adrese i podmreže vaše VNet kada je stvorena.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

### <a name="3-create-a-gateway-subnet"></a>3. stvaranje podmreži pristupnika

Prije povezivanja virtualne mreže pristupnika, najprije morate stvoriti podmreže pristupnik za virtualne mreže na koju se želite povezati. Ako je to moguće, najbolje je da biste stvorili podmreži pristupnika pomoću blok CIDR /28 ili /27 omogućili dovoljno IP adrese kako bi odgovarao dodatne buduće konfiguracijske preduvjeti.

Snimke zaslona u ovom odjeljku prikazuje se kao primjer reference. Ne zaboravite korištenje GatewaySubnet adresni raspon koji odgovara uz tražene vrijednosti za konfiguraciju.

#### <a name="to-create-a-gateway-subnet"></a>Da biste stvorili podmreži pristupnika

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="dns"></a>4. Navedite DNS poslužitelja (nije obavezno)

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Dio 2 – da biste stvorili pristupnik virtualne mreže

Točka web veze zahtijevaju sljedeće postavke:

- Vrsta pristupnika: VPN-a
- Vrsta VPN: usmjeravanje-poštu

### <a name="to-create-a-virtual-network-gateway"></a>Da biste stvorili pristupnik virtualne mreže

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Dio 3 – Stvaranje certifikata

Azure koristi certifikata za provjeru autentičnosti klijenti VPN-a za VPN-ovi zareza web. Javni certifikat podatke (ne privatni ključ) izvesti kao Base-64 kodirani X.509 .cer datoteka s korijenskim certifikatom generira rješenje certifikat za enterprise ili korijenski samopotpisani certifikat. Zatim uvoza podaci javni certifikat iz korijenski certifikat za Azure. Uz to, morate generiranje potvrdu klijenta iz korijenski certifikat za klijente. Svaki klijent koji želi povezati virtualne mreže pomoću P2S veze, morate imati instaliran certifikat klijenta koji je generirao korijenski certifikat.

### <a name="getcer"></a>1. nabaviti .cer datoteka za korijenski certifikat

Ako koristite rješenje za enterprise, možete koristiti svoje postojeće lanac certifikata. Ako ne koristite CA rješenje enterprise, možete stvoriti korijenski samopotpisanog certifikata. Jedan od načina za stvaranje samopotpisanog certifikata je makecert.

- Ako koristite u sustavu za potvrdu enterprise, nabavite .cer datoteka za korijenski certifikat koji želite koristiti. 

- Ako se ne koriste rješenje certifikat za enterprise, morate stvoriti korijenski samopotpisani certifikat. Upute za Windows 10, može se odnositi na [Rad s samopotpisani korijenskih certifikata za konfiguracije točke na web](vpn-gateway-certificates-point-to-site.md).

1. Da biste nabavili .cer datoteka iz certifikat, otvorite **certmgr.msc** i pronađite korijenski certifikat. Desnom tipkom miša kliknite korijenski samopotpisani certifikat, kliknite **Svi zadaci**, a zatim kliknite **Izvoz**. Otvara se **Čarobnjak za izvoz certifikata**.

2. U čarobnjaku kliknite **Dalje**, odaberite **ne, izvezi privatni ključ**i zatim kliknite **Dalje**.

3. Na stranici **Oblik datoteke za izvoz** odaberite **Osnovni 64 kodirani X.509 (. CER).** Zatim kliknite **Dalje**. 

4. Na u **datoteka za izvoz**, **Pronađite** mjesto na koje želite izvesti certifikata. **Naziv datoteke**, naziv datoteka certifikata. Zatim kliknite **Dalje**.

5. Kliknite **Završi** da biste izvezli certifikata.

### <a name="generateclientcert"></a>2. generiranje potvrdu klijenta

Ili možete stvoriti jedinstvene certifikat za svaki klijent koji će se povezati ili pomoću istog certifikata za druge klijente. Prednost generiranje jedinstvenih klijentskih potvrda je mogućnost da biste opozvali jedan certifikat prema potrebi. U suprotnom, ako svi koriste isti certifikat klijenta i pronaći ćete morati opoziv certifikata za jednog klijenta, morat ćete generiranje i instalirajte novog certifikata za sve klijente koji koriste taj certifikat za provjeru autentičnosti.

- Ako koristite rješenje certifikat za enterprise, generirati potvrdu klijenta s uobičajenih oblika vrijednost za naziv 'name@yourdomain.com', umjesto oblik "name\username domene". 

- Ako koristite samopotpisani certifikat, potražite u članku [Rad s samopotpisani korijenskih certifikata za konfiguracije točke web](vpn-gateway-certificates-point-to-site.md) da biste generirali potvrdu klijenta.

### <a name="exportclientcert"></a>3. Izvoz certifikat klijenta

Klijentski certifikat je potreban za provjeru autentičnosti. Nakon klijentski certifikat za generiranje izvesti. Klijentski certifikat izvezete instalirat će se kasnije na svakom klijentskom računalu.

1. Da biste izvezli potvrdu klijenta, možete koristiti *certmgr.msc*. Desnom tipkom miša kliknite klijentski certifikat koji želite izvesti, kliknite **Svi zadaci**, a zatim **Izvoz**.
2. Izvoz certifikat klijenta s privatni ključ. Ovo je *.pfx* datoteka. Provjerite je li zapis ili sjetiti lozinke (tipka) koju ste postavili za taj certifikat.

## <a name="addresspool"></a>Dio 4 - dodajte skupna adresa klijenta

1. Nakon što pristupnika virtualne mreže, pomaknite se do odjeljka **Postavke** pristupnika plohu virtualne mreže. U odjeljku **Postavke** kliknite **točke-na-web-mjestu konfiguriranje** da biste otvorili plohu **konfiguracije** .

    ![pokažite na plohu web-mjesta] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "pokažite na plohu web-mjesta")

2. **Skupna adresa** nije skup IP adrese iz kojeg klijenti koji se povezuju primit će IP adresa. Dodavanje skupna adresa, a zatim kliknite **Spremi**.

    ![Skupna adresa klijenta] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "Skupna adresa klijenta")

## <a name="uploadfile"></a>Dio 5 - prijenos u korijen .cer datoteka certifikata

Nakon stvaranja pristupnika možete prenijeti .cer datoteka za potvrdu pouzdanom korijenu Azure. Možete prenijeti datoteke veličine do 20 korijenskih certifikata. Privatni ključ za korijenski certifikat prenesite ne Azure. Nakon prijenosa .cer datoteka Azure je koristi za provjeru autentičnosti klijenti koji se povezuju sa virtualne mreže.

1. Dođite do konfiguracije **točke-na-web-mjesta** plohu. U odjeljku **korijenski certifikat** ovaj plohu dodat će .cer datoteke.

    ![pokažite na plohu web-mjesta] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "pokažite na plohu web-mjesta")

2. Provjerite je li izvesti korijenski certifikat kao Base-64 kodirani X.509 (.cer) datoteke. Morate izvesti u tom obliku tako da se potvrda možete otvoriti pomoću uređivač teksta.
3. Otvorite certifikat s nekom uređivaču teksta kao što je blok za pisanje. Kopirajte sljedeću sekciju:

    ![podatke o certifikatu] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "podatke o certifikatu")

4. Zalijepite podatke o certifikatu u odjeljku **Javne podatke o certifikatu** portala. Stavite naziv certifikata u prostoru za **naziv** , a zatim kliknite **Spremi**. Možete dodati do 20 pouzdani korijenski certifikati.

    ![Prijenos certifikata] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "Prijenos certifikata")

## <a name="clientconfig"></a>Dio 6 - preuzimanje i instalaciju paketa konfiguracije klijenta VPN-a

Klijenti za povezivanje s Azure pomoću P2S mora imati i potvrdu klijenta i paket VPN klijentskog konfiguracije instaliran. VPN klijentskog konfiguracije paketa dostupni su za klijente sustava Windows. 

Klijent paketa VPN sadrži informacije da biste konfigurirali VPN klijentski softver koji je ugrađen u sustavu Windows. Konfiguracija nije specifična za VPN-a koji želite povezati. Paket instalirati dodatnog softvera. [Najčešća pitanja vezana uz VPN pristupnika](vpn-gateway-vpn-faq.md#point-to-site-connections) dodatne informacije potražite u članku.

1. O konfiguraciji **točke-na-web-mjesta** plohu, kliknite **Preuzimanje VPN klijent** da biste otvorili plohu **Preuzimanje VPN klijent** .

    ![Preuzimanje VPN klijent] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "Preuzimanje VPN klijent")

2. Odaberite ispravan paket za vaš klijent, a zatim kliknite **Preuzmi**. Za 64-bitni klijenata, odaberite **AMD64**. Za 32-bitne klijenata, odaberite **x86**.

3. Instalirajte paket na klijentskom računalu. Ako se pojavi skočni prozor SmartScreen, kliknite **Dodatne informacije**, **svejedno izvesti** da biste instalirali paket.

4. Na klijentskom računalu, idite na **Postavke mreže** , a zatim kliknite **VPN-a**. Prikazat će se veza na popisu. Ona će se prikazati naziv virtualne mreže koji će se povezati s i izgleda slično kao u ovom primjeru: 

    ![VPN klijent] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "VPN klijent")

## <a name="installclientcert"></a>Dio 7 – Instaliraj certifikat klijenta

Svako klijentsko računalo mora imati klijentski certifikat za provjeru autentičnosti. Prilikom instaliranja certifikat klijenta, morate lozinku stvorenu prilikom izvoza certifikat klijenta.

1. Kopirajte .pfx datoteka na klijentskom računalu.
2. Dvokliknite .pfx datoteka da biste ga instalirali. Izmjena mjesto instalacije.

## <a name="connect"></a>Dio 8 - povezati Azure

1. Za povezivanje s VNet, na klijentskom računalu otvorite VPN vezama i pronađite VPN vezu koju ste stvorili. To se naziva isti naziv kao virtualne mreže. Kliknite **Poveži**. Poruka u skočnom prozoru može prikazati koje se odnosi na pomoću certifikata. Ako se to dogodi, kliknite **Nastavi** da biste koristili dodatnim ovlastima. 

2. Na stranici stanja **veze** kliknite **Poveži** da biste pokrenuli vezu. Ako se prikaže zaslon za **Odabir certifikata** , provjerite je li certifikat klijenta koji prikazuje onaj koji želite koristiti za povezivanje. Ako nije, pomoću strelice padajućeg izbornika odaberite odgovarajući certifikat, a zatim kliknite **u redu**.

    ![VPN klijent 2] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN klijentskog veze")

3. Sada moraju uspostaviti vezu.

    ![VPN klijent 3] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN veza klijent 2")

## <a name="verify"></a>Dio 9 – provjerite je li vaša veza

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

## <a name="add"></a>Da biste dodali ili uklonili pouzdani korijenski certifikati

Pouzdani korijenski certifikat možete ukloniti iz Azure. Prilikom uklanjanja pouzdana ustanova klijentskih potvrda koje su stvorene iz korijenski certifikat više moći povezati Azure putem točke na web. Ako želite da se klijentima omogućuje povezivanje, koje su im potrebne da biste instalirali novi klijent certifikat koji je generirao certifikat pouzdanim Azure.

Na popisu opozvanih klijentskih potvrda o konfiguraciji **točke-na-web-mjesta možete upravljati** plohu. Ovo je plohu koju ste koristili za [Prijenos pouzdani korijenski certifikat](#uploadfile).

## <a name="revokeclient"></a>Da biste upravljali popisom pravilnika klijent opozvanih certifikata

Možete opozvati klijentskih potvrda. Popisi opozvanih certifikata omogućuje selektivno uskratiti točke web povezivanje koji se temelji na pojedinačne klijentskih potvrda. Ako uklonite .cer za potvrdu korijenski iz Azure, opoziva pristupa za sve klijentskih potvrda generira/prijavljeni po opozvanih korijenski certifikat. Ako želite opozvati certifikat za određeni klijent, ne korijenu, to možete učiniti. Na taj način druge potvrde koje su stvorene iz korijenski certifikat i dalje će biti valjane. 

Praktična obuka za zajednički je koristiti korijenski certifikat za upravljanje pristupom na razinama tima ili organizacije tijekom korištenja klijent opozvanih certifikata za kontrolu pristupa preciznije na pojedinačnim korisnicima.

Na popisu opozvanih klijentskih potvrda o konfiguraciji **točke-na-web-mjesta možete upravljati** plohu. Ovo je plohu koju ste koristili za [Prijenos pouzdani korijenski certifikat](#uploadfile).


## <a name="next-steps"></a>Daljnji koraci

Virtualne mreže možete dodati virtualnog računala. Upute potražite u članku [Stvaranje virtualnog računala](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .