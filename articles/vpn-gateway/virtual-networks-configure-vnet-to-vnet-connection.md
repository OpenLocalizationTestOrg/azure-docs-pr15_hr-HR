<properties
   pageTitle="Konfiguriranje VNet VNet veze za uvođenje klasičnog model | Microsoft Azure"
   description="Kako se povezati Azure virtualne mreže Suradnja pomoću programa PowerShell i Azure klasični portal."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>


# <a name="configure-a-vnet-to-vnet-connection-for-the-classic-deployment-model"></a>Konfiguriranje VNet VNet veze za uvođenje klasičnog model

> [AZURE.SELECTOR]
- [Voditelj resursa – Portal za Azure](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Voditelj resursa – PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klasični – klasični Portal](virtual-networks-configure-vnet-to-vnet-connection.md)



U ovom se članku vodit će vas kroz korake za stvaranje i povezivanje virtualne mreže Suradnja pomoću model klasični implementacije (poznat i kao servis za upravljanje). Sljedeće korake pomoću portala za Azure klasični da biste stvorili VNets i pristupnika i PowerShell konfigurirati vezu za VNet VNet. Ne možete konfigurirati vezu na portalu.

![VNet VNet povezivanje dijagram](./media/virtual-networks-configure-vnet-to-vnet-connection/v2vclassic.png)


### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Uvođenje modela i postupci za VNet VNet veze

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Sljedeća tablica prikazuje trenutno dostupno implementacije modela i postupci za VNet VNet konfiguracije. Kada članak s navedeni koraci za konfiguraciju postane dostupan, ne možemo povezati izravno iz u ovoj su tablici.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>O vezama VNet VNet

Povezivanje virtualne mreže s drugom virtualne mreže (VNet-VNet) slično je povezivanje virtualne mreže na mjesto lokalnog web-mjesta. Pristupnik za VPN obje vrste povezivanja koristite sigurne tunelom pomoću IPsec-IKE. 

U web-mjesto različite pretplate i u okvir za različite regije može biti VNets povežete. Možete kombinirati VNet za VNet komunikacije s konfiguracijama više web-mjesta. Omogućuje uspostavljanje topologija mreže kojima se kombiniraju više lokacija povezivanje s povezivanjem inter-virtualne mreže.


### <a name="why-connect-virtual-networks"></a>Zašto se povezati virtualne mreže?

Preporučujemo vam da biste se povezali virtualne mreže iz sljedećih razloga:

- **Unakrsni zemlj zalihosti regija i prisutnosti za zemlj.**
    - Možete postaviti vlastitu zemlj replikacije ili sinkronizaciju s sigurno povezivanje bez prelaska na mjesto na Internetu krajnje točke.
    - S Azure opterećenja i Microsoft ili drugih proizvođača klasteriranja tehnologiju, možete postaviti iznimno dostupna radno opterećenje s zemlj zalihosti preko više Azure regija. Primjer važno je da biste postavili SQL uvijek na s grupama dostupnost šire preko više područja Azure.

- **Regionalne više razina aplikacije s istaknuti odvajanja granice**
    - Unutar iste područja, možete postaviti više razina aplikacije s više VNets povezani s istaknuti odvajanja i sigurnu komunikaciju među sloju.

- **Unakrsni pretplatu, komunikacije među tvrtke ili ustanove u Azure**
    - Ako imate više pretplata Azure, možete se povezati radnih opterećenja iz različitih pretplata zajedno sigurno između virtualne mreže.
    - Za velike i usluga, možete omogućiti unakrsno organizacije komunikaciju s sigurne VPN tehnologija unutar Azure.

### <a name="vnet-to-vnet-faq-for-classic-vnets"></a>Najčešća pitanja vezana uz VNet VNet za klasični VNets

- Virtualne mreže mogu biti u istom ili drugom pretplate.

- Virtualne mreže mogu biti u istom ili drugom Azure područja (mjesta).

- Servis u oblaku ili na krajnjoj točki za ujednačavanje opterećenja ne može obuhvaćati putem virtualne mreža, čak i ako su povezani zajedno.

- Međusobno povezivanje više virtualne mreža ne zahtijeva VPN uređaje.

- VNet VNet podržava spojnih virtualne mreže Azure. Ga ne podržava povezivanje virtualnim strojevima ili cloud servise koji su implementiran na virtualne mreže.

- VNet VNet zahtijeva dinamički usmjeravanje pristupnika. Azure statične usmjeravanje pristupnika nisu podržane.

- Veza s mrežom virtualne može se koristiti istodobno s više web-mjesta VPN-ovima. Postoji najviše 10 tunnels VPN-a za pristupnik za VPN virtualne mreže povezivanja s drugim virtualne mreže ili na lokalnim web-mjesta.

- Za to predviđen adresu virtualne mreže i lokalne lokalne mreže web-mjesta morate preklapaju. Razmaci adresa koji se preklapaju može dovesti do stvaranja virtualne mreže ili prijenosa datoteka konfiguracije netcfg uvoza.

- Suvišne tunnels između par virtualne mreže nisu podržani.

- Sve tunnels VPN-a za VNet, uključujući P2S VPN-ovi, zajedničko korištenje raspoložive propusnosti za pristupnik za VPN-a i na istom VPN pristupnika vrijeme aktivnosti SLA u Azure.

- Promet VNet VNet prelazi preko Azure backbone.


## <a name="step1"></a>Korak 1 - planiranje vaše rasponi IP adresa

Važno je da odlučite raspone koji će se koristiti za konfiguriranje virtualne mreže. Za tu konfiguraciju, morate provjeriti da nema raspone VNet preklapa međusobno ili u bilo kojem od lokalne mreže s kojima se povezuju.

Sljedeća tablica prikazuje primjer kako definirati vaše VNets. Korištenje raspona kao većinom samo. Zapišite rasponi za virtualne mreže. Za kasnije korake morate taj podatak.

**Primjer postavke**

|Virtualne mreže  |Prostor adrese               |Regija      |Povezuje se s web-mjesta u lokalnoj mreži|
|:----------------|:---------------------------|:-----------|:-----------------------------|
|VNet1            |VNet1 (10.1.0.0/16)         |SAD Zapad     |VNet2Local (10.2.0.0/16)      |
|VNet2            |VNet2 (10.2.0.0/16)         |Istok Japan  |VNet1Local (10.1.0.0/16)      |
  
## <a name="step-2---create-vnet1"></a>Korak 2 – Stvaranje VNet1

U ovom ćete koraku ćemo stvoriti VNet1. Kada koristite neki od primjera, obavezno zamijeniti vlastitim vrijednosti. Ako vaš VNet već postoji, ne morate učiniti ovaj korak. No morate provjerite ne rasponi IP adresa preklapa s raspone na drugi VNet ili u bilo kojem od VNets na koji se želite povezati.

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com). U ovom se članku klasični portal koristimo jer se neke postavke potrebne konfiguracije još nisu dostupni na portalu za Azure.

2. U donjem lijevom kutu zaslona kliknite **Novo** > **Mrežnim servisima** > **Virtualne mreže** > **Stvaranje Prilagođeno** da biste pokrenuli čarobnjak za konfiguraciju. Dok se pomičete kroz čarobnjak, dodajte određene vrijednosti na svaku stranicu.

### <a name="virtual-network-details"></a>Detalji o virtualne mreže

Na stranici virtualne mreže pojedinosti unesite sljedeće podatke:

  ![Detalji o virtualne mreže](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

  - **Ime** - naziv virtualne mreže. Na primjer, VNet1.
  - **Mjesto** – kada stvarate virtualne mreže, povezujete se s Azure lokaciji (regiji). Ako, na primjer, ako želite da se vaša VMs koje su uvedene na virtualne mrežu da biste fizički nalaze u Zapad SAD-a, odaberite to mjesto. Ne možete promijeniti mjesto povezani s mrežom virtualne kad ga stvorite.

### <a name="dns-servers-and-vpn-connectivity"></a>DNS poslužitelji i povezivanje VPN-a

Na stranici DNS poslužitelji i grupa podaci, unesite sljedeće podatke, a zatim sljedeće strelicu dolje desno.

  ![DNS poslužitelji i povezivanje VPN-a](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)  

- **DNS poslužitelji** - unesite naziv poslužitelja za DNS-a i IP adresa ili na padajućem izborniku odaberite prethodno registrirani DNS poslužitelj. Ta postavka nije moguće stvoriti DNS poslužitelj. Omogućuje određivanje DNS poslužitelji koji želite koristiti za razrješavanje imena u pošti virtualne mreže. Ako želite imati razlučivost naziv između virtualne mreže, morate konfigurirati vlastite DNS poslužitelj umjesto korištenja razlučivanje naziva koju je dodijelio Azure.
- Nemojte odabrati neki od potvrdne okvire za povezivanje P2S ili S2S. Kliknite strelicu dolje desno da biste prešli na sljedeći zaslon.

### <a name="virtual-network-address-spaces"></a>Razmaci adresu virtualne mreže

Na stranici virtualne razmake mreže adrese Navedite adresu raspona koji želite koristiti za virtualne mreže. To su dinamičke IP adrese (DIPS) koji će se dodijeliti u VMs i druge instance uloge koje implementirati virtualne mreže. 

Ako stvarate VNet koji će imati veze s mrežom na lokalni, osobito važno je da biste odabrali raspon koji se ne preklapa s bilo kojeg od raspona koji se koriste za lokalnu mrežu. U tom slučaju morate usklađivanju administratora mreže. Administrator mreže možda morati carve izvan raspona IP adrese iz lokalne mreže adresnog prostora možete koristiti za svoje VNet.

  ![Virtualna razmake mreže adresu stranice](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

  - **Prostor adrese** – uključujući vremena i pokretanje IP adresa. Provjerite je li da za to predviđen adresu navedete ne preklapa s bilo kojom razmake adresu na lokalnu mrežu. U ovom primjeru koristimo 10.1.0.0/16 VNet1.
  - **Dodavanje podmreže** – uključujući vremena i pokretanje IP adresa. Dodatni podmreže nisu potrebni, ali želite stvoriti zasebnu podmreže za VMs koji će imati statične DIPS. Ili, možda ćete morati imati vaše VMs podmreži razlikuje se od ostalih instanci ulogu.
 
Da biste stvorili započet će na donjem desnom dijelu stranice i virtualne mreže **kliknite kvačicu** . Kada završi, prikazat će se "Stvara" na stranici Mreža naveden u odjeljku Status.

## <a name="step-3---create-vnet2"></a>Korak 3 – stvaranje VNet2

Nakon toga ponovite prethodne korake da biste stvorili drugi VNet. U noviji koracima će se povezati dva VNets. Možete se referirati na [primjerima postavki](#step1) u koraku 1. Ako vaš VNet već postoji, ne morate učiniti ovaj korak. Međutim, morate da biste potvrdili da se rasponi IP adresa ne preklapa s nekim drugim VNets ili lokalne mreže koji želite povezati.

## <a name="step-4---add-the-local-network-sites"></a>Korak 4 - dodajte lokalne mreže web-mjesta

Kada stvorite VNet VNet konfiguracije, morate konfigurirati lokalne mreže web-mjesta, koji se prikazuju se na stranici **Lokalne mreže** portala. Azure koristi postavke određene u svakom web-mjestu lokalne mreže da biste saznali kako želite usmjeriti promet između na VNets. Odredite naziv koji želite koristiti za upućivanje na svakom web-mjestu lokalnoj mreži. Preporučuje se da biste koristili nešto opisne, označavanja vrijednost na padajućem popisu u noviji korake.

Na primjer, VNet1 povezuje u lokalnoj mreži web-mjesta koje ste stvorili pod nazivom "VNet2Local". Postavke za VNet2Local sadrže prefiksi adresa za VNet2 i javna IP adresa za pristupnik za VNet2. VNet2 povezuje u lokalnoj mreži web-mjesta stvarate pod nazivom "VNet1Local", koja sadrži adresu prefiksi VNet1 i javnu IP adresu VNet1 pristupnika.

### <a name="localnet"></a>Mjesto dodati u lokalnoj mreži VNet1Local

1. U donjem lijevom kutu zaslona kliknite **Novo** > **Mrežnim servisima** > **Virtualne mreže** > **Dodavanje lokalnoj mreži**.

2. Na stranici **određivanje pojedinosti lokalne mreže** za **naziv**unesite naziv koji želite koristiti za označavanje mrežu s kojom se želite povezati. U ovom primjeru možete koristiti "VNet1Local" da biste se pozvali na rasponi IP adresa i pristupnik za VNet1.

3. Za **VPN uređaj IP adrese (neobavezno)**, navedite bilo koji valjani javnu IP adresa. Obično, koristit ćete stvarni vanjske IP adrese za VPN uređaj. Za konfiguracije VNet VNet pomoću javnu IP adresu koja je dodijeljena pristupnik za vaše VNet. No given da ste pristupnika niste stvorili, možete odrediti bilo koji valjani javnu IP adresu kao rezervirano mjesto. Ne, polje ostavite prazno – nije obavezna za tu konfiguraciju. U noviji koraka, vratite se u te postavke i konfiguriranje ih pomoću odgovarajuće pristupnik IP adrese kada Azure stvara ga. Kliknite strelicu da bi prešli na sljedećem zaslonu.

4. Na stranici **Navedite adresu stranice**, unesite IP adresa raspon i adresu broj za VNet1. To morate točno odgovarati raspona koji je konfiguriran za VNet1. Azure koristi rasponi IP adresa koji navedete da biste usmjerili promet za VNet1. Kliknite kvačicu da biste stvorili lokalnu mrežu.

### <a name="add-the-local-network-site-vnet2local"></a>Mjesto dodati u lokalnoj mreži VNet2Local

Pomoću gore navedene korake da biste stvorili lokalnu mrežu web-mjesta "VNet2Local". Možete se referirati na vrijednosti u [primjerima postavki](#step1) u koraku 1 ako je potrebno.

### <a name="configure-each-vnet-to-point-to-a-local-network"></a>Konfiguriranje svaki VNet tako da pokazuje na lokalnu mrežu

Svaki VNet moraju pokazivati na odgovarajući lokalnu mrežu koju želite usmjeriti promet na. 

#### <a name="for-vnet1"></a>Za VNet1

1. Idite na stranicu **Konfiguriraj** za virtualne mreže **VNet1**. 
2. U odjeljku Povezivost s web-mjesto, odaberite "Povezivanje s lokalnom mrežom", a zatim **VNet2Local** kao lokalne mreže na padajućem izborniku. 
3. Spremanje postavki.

#### <a name="for-vnet2"></a>Za VNet2

1. Idite na stranicu **Konfiguriraj** za virtualne mreže **VNet2**. 
2. U odjeljku Povezivost s web-mjesto, odaberite "Povezivanje s lokalnom mrežom", a zatim odaberite **VNet1Local** na padajućem izborniku kao lokalne mreže. 
3. Spremanje postavki.

## <a name="step-5---configure-a-gateway-for-each-vnet"></a>Drugi korak 5 - konfiguracije pristupnika za svaku VNet

Konfiguriranje pristupnika dinamički usmjeravanje za svaki virtualne mreže. Tu konfiguraciju ne podržava statične usmjeravanje pristupnika. Ako koristite VNets koje ste prethodno konfigurirali i koji već imaju pristupnika dinamički usmjeravanja, ne morate učiniti ovaj korak. Ako vaš pristupnika statične usmjeravanja, morate ih izbrisati i ponovno ih stvorite kao pristupnika dinamički usmjeravanja. Ako izbrišete pristupnika, dobiti objavio javnu IP adresu dodijeljena i morate vratiti i konfigurirajte bilo koju od lokalne mreže i uređaje VPN javnu IP adresu za novi pristupnik.

1. Na stranici **mreža** , provjerite je li u stupcu stanje za virtualne mreže **stvorili**.

2. U stupcu **naziv** kliknite naziv virtualne mreže. U ovom primjeru koristimo "VNet1".

3. Na stranici **nadzorne ploče** obratite pozornost na ovom VNet ne postoji još konfiguriran pristupnik. Vidjet ćete tu promjenu kao proći kroz korake da biste konfigurirali pristupnikom statusa.

4. Pri dnu stranice kliknite **Stvaranje pristupnika** i **Dinamični usmjeravanja**. Kada u sustavu od vas traži da biste potvrdili da želite da pristupnik stvorili, kliknite da.

    ![Vrsta pristupnika](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)  

5. Kada se stvara pristupnikom uočite grafiku pristupnika na stranici mijenja žutu piše "Stvaranje pristupnika". Traje otprilike 30 minuta za pristupnik za stvaranje.

6. Ponovite iste korake za VNet2. Ne morate prvi pristupnika VNet da biste dovršili prije nego što počnete da biste stvorili pristupnik za vaše VNet.

7. Kada je stanje pristupnika Promijeni "Povezivanje", vidljiv na nadzornoj ploči je javnu IP adresa za svaki pristupnik. Zapišite IP adresu koja odgovara za svaku VNet Pobrinite se da nećete ih kombinirati prema gore. To su IP adresa koji se koriste prilikom uređivanja IP adresama rezervirano mjesto za uređaj VPN-a za svaki lokalnoj mreži.

## <a name="step-6---edit-the-local-network"></a>Korak 6 - uređivanje lokalne mreže

1. Na stranici **Lokalne mreže** kliknite naziv naziv lokalne mreže koju želite urediti, a zatim kliknite **Uređivanje** pri dnu stranice. Za **VPN uređaj IP adresa**za unos IP adresa pristupnika koji odgovara na VNet. Ako, na primjer, VNet1Local, staviti u dodijeljene VNet1 IP adresa pristupnika. Zatim kliknite strelicu pri dnu stranice.

2. Na stranicu za **navođenje adrese prostora** kliknite kvačicu u donjem desnom bez ikakvih promjena.

## <a name="step-7---create-the-vpn-connection"></a>Korak 7 – stvaranje VPN veza

Kada dovršite prethodne korake postavite tipki zajednički IPsec-IKE i stvorite vezu. U ovom se skup koraka koristi PowerShell i ne može konfigurirati na portalu. Dodatne informacije o instaliranju cmdleta Azure PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) . Obavezno preuzmite najnoviju verziju cmdleti za upravljanje servisa (US). 

1. Otvorite Windows PowerShell i prijavite se.

        Add-AzureAccount

2. Odaberite pretplatu u koje se nalaze na VNets u.

        Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
        Select-AzureSubscription -SubscriptionName "<Subscription Name>"

3. Stvaranje veze. U primjerima obratite pozornost na to da zajednički ključ je točno onaj. Zajednički ključ uvijek mora podudarati.


    VNet1 VNet2 vezu

        Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2Local -SharedKey A1b2C3D4

    VNet2 VNet1 vezu

        Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1Local -SharedKey A1b2C3D4

4. Pričekajte veza za pokretanje. Kada je pokrenut pristupnik, kao što je sljedeća grafika izgleda pristupnika.

    ![Stanje pristupnika - povezani](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)  

    [AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="next-steps"></a>Daljnji koraci

Možete dodati virtualnim strojevima virtualne mreže. Potražite u [dokumentaciji virtualnim strojevima](https://azure.microsoft.com/documentation/services/virtual-machines/) dodatne informacije.



[1]: ../hdinsight-hbase-geo-replication-configure-vnets.md
[2]: http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Configure-the-VPN-connectivity-between-two-Azure-virtual-networks
 
