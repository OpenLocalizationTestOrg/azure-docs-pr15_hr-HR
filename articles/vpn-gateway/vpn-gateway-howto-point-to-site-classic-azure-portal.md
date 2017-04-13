<properties
   pageTitle="Konfiguriranje VPN točke web pristupnika veze s mrežom virtualne Azure pomoću portala za Azure | Microsoft Azure"
   description="Sigurno povezivanje s mrežom virtualne Azure stvaranjem točke web VPN veza pristupnika pomoću portala za Azure."
   services="vpn-gateway"
   documentationCenter="na"
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
   ms.date="10/17/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfiguriranje veze točke web VNet pomoću portala za Azure

> [AZURE.SELECTOR]
- [Voditelj resursa – Portal za Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Voditelj resursa – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasični – Portal za Azure](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

U ovom se članku vodit će vas voditi kroz stvaranje na VNet s vezom točke na mjesto u modelu uvođenje klasičnog pomoću portala za Azure. Konfiguraciju točke-na-web-mjesta (P2S) omogućuje stvaranje sigurnu vezu s pojedinačne klijentskog računala da biste virtualne mreže. Povezivanje P2S koristan je kada želite povezati svoje VNet s udaljenog mjesta, kao što su od kuće ili konferencije. Ili, kad imate samo nekoliko klijenti koje morate se povezati s virtualne mreže.

Točke web veze ne zahtijevaju VPN uređaja ili dostupnog javnosti IP adresa za rad. Pokretanjem povezivanja s klijentskom računalu je uspostavljena VPN veza. Dodatne informacije o vezama točke web potražite u članku [Najčešća pitanja vezana uz VPN pristupnika](vpn-gateway-vpn-faq.md#point-to-site-connections) i [O pristupnika VPN-a](vpn-gateway-about-vpngateways.md#point-to-site).

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Uvođenje modela i postupci za P2S veze

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Sljedeća tablica prikazuje dva implementacije modelima i dostupne načinima za P2S konfiguracije. Kada članak s navedeni koraci za konfiguraciju postane dostupan, ne možemo povezati izravno iz u ovoj su tablici.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Osnovni tijek rada 

![Točke-na-dijagram web-mjesta –] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "točka-mjesta")


U sljedećim se odjeljcima će vas voditi kroz korake da biste stvorili sigurna veza točke web virtualne mreže. 

1. Stvaranje virtualne mreže i pristupnika VPN-a
2. Stvaranje certifikata
3. Prijenos .cer datoteka
4. Stvaranje paketa za konfiguriranje klijenta VPN-a
5. Konfiguriranje klijentsko računalo
6. Povezivanje s Azure

### <a name="example-settings"></a>Primjer postavke

Možete koristiti sljedeće postavke primjer:

- **Naziv: VNet1**
- **Adresa prostora: 192.168.0.0/16**
- **Naziv podmreže: sučelju**
- **Raspon adresu podmreže: 192.168.1.0/24**
- **Pretplate:** Ako imate više pretplata, provjerite koristite li ispravnu.
- **Grupa resursa: TestRG**
- **Lokacija: Istočni SAD-a**
- **Vrsta veze: točku mjesta**
- **Prostor adrese za klijent: 172.16.201.0/24**. Klijenti VPN-a koji se povezuju sa VNet koristite ovu vezu točke web primiti IP adresu određeni skup.
- **GatewaySubnet: 192.168.200.0/24**. Naziv "GatewaySubnet" morate koristiti podmreže pristupnika.
- **Veličina:** Odaberite pristupnik SKU koji želite koristiti.
- **Usmjeravanje vrstu: dinamički**

## <a name="vnetvpn"></a>Sekcija 1 – stvaranje virtualne mreže i pristupnika VPN-a

### <a name="createvnet"></a>Dio 1: Stvaranje virtualne mreže

Ako još nemate virtualne mreže, stvorite ga. Snimke zaslona prikazuje se kao primjere. Provjerite vrijednosti zamijeniti vlastitim. Da biste stvorili na VNet pomoću portala za Azure, poduzmite sljedeće korake: 

1. U pregledniku idite na [portal za Azure](http://portal.azure.com) i, ako je potrebno, prijavite se pomoću računa za Azure.

2. Kliknite **Novo**. U polje za **pretraživanje na tržištu** upišite "Virtualne mreže". Pronađite **Virtualne mreže** na popisu vraćeni i kliknite da biste otvorili plohu **Virtualne mreže** .

    ![Traženje plohu virtualne mreže] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png "Traženje plohu virtualne mreže")

3. Pri dnu plohu virtualne mreže, s popisa **Odaberite model implementacije** odaberite **Klasični**pa zatim kliknite **Stvori**.

    ![Odaberite model implementacije] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png "Odaberite model implementacije")

4. Postavke VNet konfigurirati na plohu **Stvaranje virtualne mreže** . U ovom plohu ćete dodati prvu adresu prostor za vaše i adresa raspona jedan podmreže. Nakon što završite stvaranje na VNet, možete vratite i dodavanje dodatnih podmreže i adresu razmake.

    ![Stvaranje virtualne mreže plohu] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png "Stvaranje virtualne mreže plohu")

5. Provjerite je li **pretplatu** ispravnu. Pretplate možete promijeniti pomoću padajući popis.

6. Kliknite **grupu resursa** , a zatim odaberite postojeću grupu resursa ili stvorite novi tako da upišete naziv za novu grupu resursa. Ako stvarate novu grupu, naziv grupe resursa prema vrijednosti planiranog konfiguracije. Dodatne informacije o grupama resursa, posjetite [Azure resursima pregled](azure-resource-manager/resource-group-overview.md#resource-groups).

7. Nakon toga odaberite postavki **lokaciju** svoje VNet. Mjesto određuje koje će se nalaziti resursa koji implementirati ovaj VNet.

8. Ako želite da biste mogli jednostavno pronaći vaše VNet na nadzornu ploču, a zatim kliknite **Stvori**, odaberite **Prikvači na nadzornoj ploči** .
    
    ![Prikvači na nadzornoj ploči] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png "Prikvači na nadzornoj ploči")


9. Nakon što kliknete Stvori, vidjet ćete pločicu na nadzornoj ploči koje odražavaju tijeku vaše VNet. Na pločici mijenja kao što je stvoren je u VNet.

    ![Stvaranje virtualne mreže pločica] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Stvaranje virtualne mreže pločica")

10. Kada stvorite virtualne mreže, možete dodati IP adresu DNS poslužitelja da bi se rukovati razlučivanje naziva. Da biste otvorili postavke za virtualne mreže, kliknite DNS poslužitelji i dodavanje IP adresu DNS poslužitelja koji želite koristiti. Ta postavka nije moguće stvoriti novi DNS poslužitelj. Ne zaboravite da biste dodali DNS poslužitelj koji resursa možete komunicirati s.

Nakon što virtualne mreže, vidjet ćete **stvoreno** navedene u odjeljku **Status** na stranici Mreža Azure klasični portalu.

### <a name="gateway"></a>Dio 2: Stvaranje pristupnika podmreže i dinamični usmjeravanje pristupnika

U ovom ćete koraku stvarate podmreži pristupnika i dinamični usmjeravanje pristupnika. Na portalu Azure za uvođenje klasičnog model stvaranje podmreže pristupnika i pristupnika možete učiniti putem iste blades konfiguracije.

1. Na portalu dođite do virtualne mreže za koji želite da biste stvorili pristupnik.

2. Na plohu za virtualne mreže na plohu **Pregled** , u odjeljku veze VPN-a kliknite **pristupnika**.

    ![Kliknite ovdje da biste stvorili pristupnik] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png "Kliknite ovdje da biste stvorili pristupnik")


3. Na plohu **Novu VPN vezu** odaberite **točke-na-web-mjesta**.

    ![Vrsta veze P2S] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png "Vrsta veze P2S")

4. **Klijent adresnog prostora**, dodajte rasponu IP adresa. To je raspon iz kojeg klijenti VPN primit će IP adresu prilikom povezivanja. Izbrišite rasponu automatski ispunjava i dodajte vlastiti.

    ![Prostor za adrese klijenta] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png "Prostor za adrese klijenta")

5. Potvrdite okvir **Stvori pristupnika odmah** .

    ![Stvaranje pristupnika odmah] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png "Stvaranje pristupnika odmah")

6. Kliknite **konfiguracije pristupnika nije obavezno** da biste otvorili plohu **konfiguracije pristupnika** .

    ![Kliknite neobavezno pristupnika konfiguracija] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png "Kliknite neobavezno pristupnika konfiguracija")

7. Kliknite **Konfiguriranje podmreže potrebne postavke** za dodavanje **podmreže pristupnika**. Dok je moguće da biste stvorili pristupnik podmreže što manje /29, preporučujemo da stvorite veće podmreže koja sadrži više adresa tako da odaberete barem /28 ili /27. Tako ćete omogućiti za dovoljno adrese kako bi odgovarao moguće dodatne konfiguracije koji se preporučuje se u budućnosti.

    >[AZURE.IMPORTANT] Kada radite s podmreže pristupnika, nemojte zastavicom mreže sigurnosne grupe (NSG) da biste podmreže pristupnika. Pridruživanje mreži sigurnosne grupe u ovom podmreži može uzrokovati pristupnik za VPN-a da biste prestali funkcionira u skladu s očekivanjima. Dodatne informacije o sigurnosnim grupama s mrežom potražite u članku [što je sigurnosne grupe mreže?](../articles/virtual-network/virtual-networks-nsg.md)

    ![Dodavanje GatewaySubnet] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png "Dodavanje GatewaySubnet")

8. Odaberite **veličinu**pristupnika. Ovo je pristupnika SKU koji ćete koristiti za stvaranje pristupnika za virtualne mreže. Na portalu SKU zadani je **Osnovni**. Dodatne informacije o pristupnika SKU-ove potražite u članku [O postavki za VPN pristupnika](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![Veličina pristupnika](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)

9. Odaberite **Vrstu usmjeravanje** pristupnikom. Konfiguracija P2S zahtijevaju **dinamički** usmjeravanje vrsta. Kada završite konfiguriranje ovaj plohu, kliknite **u redu** .

    ![Konfiguriranje usmjeravanje vrsta] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png "Konfiguriranje usmjeravanje vrsta")

10. Na plohu **Novu VPN vezu** , kliknite **u redu** pri dnu zaslona u plohu da biste započeli stvaranje pristupnika za virtualne mreže. To može potrajati i do 45 minuta. 


## <a name="generatecerts"></a>Odjeljak 2 – Stvaranje certifikata

Azure koristi certifikata za provjeru autentičnosti klijenti VPN-a za VPN-ovi zareza web. Javni certifikat podatke (ne privatni ključ) izvesti kao Base-64 kodirana X.509 .cer datoteka s korijenskim certifikatom generira rješenje certifikat za enterprise ili korijenski samopotpisani certifikat. Zatim uvoza podaci javni certifikat iz korijenski certifikat za Azure. Uz to, morate generiranje potvrdu klijenta iz korijenski certifikat za klijente. Svaki klijent koji želi povezati virtualne mreže pomoću P2S veze, morate imati instaliran certifikat klijenta koji je generirao korijenski certifikat.

### <a name="cer"></a>Dio 1: Dobivanje .cer datoteka za korijenski certifikat


Ako koristite rješenje za enterprise, možete koristiti svoje postojeće lanac certifikata. Ako ne koristite CA rješenje enterprise, možete stvoriti korijenski samopotpisanog certifikata. Jedan od načina za stvaranje samopotpisanog certifikata je makecert.

- Ako koristite u sustavu za potvrdu enterprise, nabavite .cer datoteka za korijenski certifikat koji želite koristiti. 

- Ako se ne koriste rješenje certifikat za enterprise, morate stvoriti korijenski samopotpisani certifikat. Upute za Windows 10, može se odnositi na [Rad s samopotpisani korijenskih certifikata za konfiguracije točke na web](vpn-gateway-certificates-point-to-site.md).

1. Da biste nabavili .cer datoteka iz certifikat, otvorite **certmgr.msc** i pronađite korijenski certifikat. Desnom tipkom miša kliknite korijenski samopotpisani certifikat, kliknite **Svi zadaci**, a zatim kliknite **Izvoz**. Otvara se **Čarobnjak za izvoz certifikata**.

2. U čarobnjaku kliknite **Dalje**, odaberite **ne, izvezi privatni ključ**i zatim kliknite **Dalje**.

3. Na stranici **Oblik datoteke za izvoz** odaberite **Osnovni 64 kodirani X.509 (. CER).** Zatim kliknite **Dalje**. 

4. Na u **datoteka za izvoz**, **Pronađite** mjesto na koje želite izvesti certifikata. **Naziv datoteke**, naziv datoteka certifikata. Zatim kliknite **Dalje**.

5. Kliknite **Završi** da biste izvezli certifikata.


### <a name="genclientcert"></a>Dio 2: Generiranje potvrdu klijenta

Ili možete stvoriti jedinstvene certifikat za svaki klijent koji će se povezati ili pomoću istog certifikata za druge klijente. Prednost generiranje jedinstvenih klijentskih potvrda je mogućnost da biste opozvali jedan certifikat prema potrebi. U suprotnom, ako svi koriste isti certifikat klijenta i pronaći ćete morati opoziv certifikata za jednog klijenta, morat ćete generiranje i instalirajte novog certifikata za sve klijente koji koriste taj certifikat za provjeru autentičnosti.

- Ako koristite rješenje certifikat za enterprise, generirati potvrdu klijenta s uobičajenih oblika vrijednost za naziv 'name@yourdomain.com', umjesto oblik "name\username domene". 

- Ako koristite samopotpisani certifikat, potražite u članku [Rad s samopotpisani korijenskih certifikata za konfiguracije točke web](vpn-gateway-certificates-point-to-site.md) da biste generirali potvrdu klijenta.

### <a name="exportclientcert"></a>Dio 3: Izvoz certifikat klijenta

Instalirajte potvrdu klijenta na svim računalima na kojima se želite povezati virtualne mreže. Klijentski certifikat je potreban za provjeru autentičnosti. Možete automatizirati instalacije certifikat klijenta ili ručno instalirati. Sljedeći koraci prođite kroz izvoz i ručno instaliranje certifikat klijenta.

1. Da biste izvezli potvrdu klijenta, možete koristiti *certmgr.msc*. Desnom tipkom miša kliknite klijentski certifikat koji želite izvesti, kliknite **Svi zadaci**, a zatim **Izvoz**.
2. Izvoz certifikat klijenta s privatni ključ. Ovo je *.pfx* datoteka. Provjerite je li zapis ili sjetiti lozinke (tipka) koju ste postavili za taj certifikat.

## <a name="upload"></a>Odjeljak 3 – prijenos u korijen .cer datoteka certifikata

Nakon stvaranja pristupnika možete prenijeti .cer datoteka za potvrdu pouzdanom korijenu Azure. Možete prenijeti datoteke veličine do 20 korijenskih certifikata. Privatni ključ za korijenski certifikat prenesite ne Azure. Nakon prijenosa .cer datoteka Azure je koristi za provjeru autentičnosti klijenti koji se povezuju sa virtualne mreže.

1. U odjeljku **VPN veze** plohu za vaše VNet kliknite grafiku **klijenti** Otvori **točke-na-web-mjesta VPN vezu** plohu.

    ![Klijenti] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png "Klijenti")

2. Veza **točke-na-web-mjesta** plohu, kliknite **Upravljanje potvrde** da biste otvorili plohu **certifikata** .<br>

    ![Plohu certifikata](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png "Certificates blade")<br><br>


3. Na plohu **potvrde** kliknite da biste otvorili plohu **Prijenos certifikat** **Prijenos** .<br>

    ![Prijenos plohu certifikata](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png "Upload certificates blade")<br>


4. Kliknite grafiku mape da biste potražili .cer datoteka. Odaberite datoteku, a zatim kliknite **u redu**. Osvježite stranicu da biste pogledali prenesene certifikata na plohu **certifikata** .

    ![Prijenos certifikata](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png "Upload certificate")<br>
    

## <a name="vpnclientconfig"></a>Odjeljak 4 – stvaranje paketa za konfiguriranje klijenta VPN-a

Da biste povezali virtualne mreže, također morate konfigurirati VPN klijent. Klijentsko računalo potrebno je potvrdu klijenta i proper konfiguracije paketa za VPN klijentskog radi povezivanja.

Paket za VPN klijentskog sadrži informacije o konfiguraciji Konfiguriranje VPN klijentski softver ugrađen u sustavu Windows. Paket instalirati dodatnog softvera. Postavke vrijede virtualne mrežu s kojom se želite povezati. Na popisu klijentski operacijski sustavi koje podržava potražite u članku veze [točke-na-web-mjesta](vpn-gateway-vpn-faq.md#point-to-site-connections) odjeljku najčešća pitanja vezana uz pristupnika VPN-a. 

### <a name="to-generate-the-vpn-client-configuration-package"></a>Da biste generirali paketom konfiguracije klijenta VPN-a

1. Na portalu Azure u plohu **Pregled** za VNet, **VPN veze**, kliknite grafiku klijent Otvori **točke-na-web-mjesta VPN vezu** plohu.
2. Pri vrhu na **točke-na-mjestu VPN vezu** plohu, kliknite preuzmite paket koji odgovara klijent operacijski sustav na kojem će se instalirati:

 - Za 64-bitni klijenata, odaberite **VPN klijent (64-bitni)**.
 - Za 32-bitne klijenata, odaberite **VPN klijent (32-bitni)**.

    ![Preuzmite paket konfiguracije klijenta za VPN-a](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png "Download VPN client configuration package")<br>


3. Vidjet ćete poruku da Azure generira paketom konfiguracije klijenta VPN-a za virtualne mreže. Nakon nekoliko minuta generira paketa i prikazat će se poruka na lokalnom računalu paket sadrži preuzeta. Spremite datoteku paketa konfiguracije. To će instalirati na svakom klijentskom računalu koji će se povezati s virtualne mreže pomoću P2S.


## <a name="clientconfiguration"></a>Odjeljak 5 - konfiguriranje klijentsko računalo

### <a name="part-1-install-the-client-certificate"></a>Dio 1: Instalacija certifikat klijenta

Svako klijentsko računalo mora imati klijentski certifikat za provjeru autentičnosti. Prilikom instaliranja certifikat klijenta, morate lozinku stvorenu prilikom izvoza certifikat klijenta.

1. Kopirajte .pfx datoteka na klijentskom računalu.
2. Dvokliknite .pfx datoteka da biste ga instalirali. Izmjena mjesto instalacije.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Dio 2: Instalaciju paketa konfiguracije klijenta VPN-a

Možete koristiti isti konfiguracije paketa za VPN klijentskog na svakom klijentskom računalu pod uvjetom da se verzija arhitektura za klijentski program.

1. Lokalno kopirajte konfiguracijska datoteka na računalo na kojem želite povezati virtualne mreže, a zatim dvokliknite .exe datoteke. 

2. Kada je instaliran paket, mogu početi VPN veza. Konfiguriranje paketa je potpisao Microsoft. Možda želite potpisati paketa putem servisa za potpisivanje vaše tvrtke ili ustanove ili ga potpišete sami pomoću [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327). Nije u redu da biste pomoću značajke pakiranja bez prijave. Međutim, ako je paket nije prijavljen, pojavljuje se upozorenje pri instalaciji paketa.

3. Na klijentskom računalu, idite na **Postavke mreže** , a zatim kliknite **VPN-a**. Prikazat će se veza na popisu. Prikazuje naziv virtualne mreže je da će se povezati s i će izgledati ovako: 

    ![VPN klijent] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png "VNet VPN klijent")

## <a name="connect"></a>Odjeljak 6 - povezati Azure

### <a name="connect-to-your-vnet"></a>Povezivanje s vašeg VNet

1. Za povezivanje s VNet, na klijentskom računalu otvorite VPN vezama i pronađite VPN vezu koju ste stvorili. To se naziva isti naziv kao virtualne mreže. Kliknite **Poveži**. Na skočnom može se pojaviti poruka koja se odnosi na pomoću certifikata. Ako se to dogodi, kliknite **Nastavi** da biste koristili dodatnim ovlastima. 

2. Na stranici stanja **veze** kliknite **Poveži** da biste pokrenuli vezu. Ako se prikaže zaslon za **Odabir certifikata** , provjerite je li certifikat klijenta koji prikazuje onaj koji želite koristiti za povezivanje. Ako nije, pomoću strelice padajućeg izbornika odaberite odgovarajući certifikat, a zatim kliknite **u redu**.

    ![Povezivanje] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png "VPN klijentskog veze")

3. Sada moraju uspostaviti vezu.

    ![Uspostavljeno veze] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png "Uspostaviti vezu")

### <a name="verify-the-vpn-connection"></a>Provjerite je li VPN veza

1. Da biste provjerili je li VPN veza aktivna, otvorite povećane naredbeni redak i pokrenite *ipconfig/sve*.
2. Prikaz rezultata. Obratite pozornost na to da je IP adresa koji ste primili jedan od adrese unutar raspona adresu povezivanje točke web koji ste naveli kada stvarate vaše VNet. Rezultati moraju biti nešto slično sljedećemu:

Primjer:



    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="next-steps"></a>Daljnji koraci

Možete dodati virtualnim strojevima virtualne mreže. Saznajte [kako stvoriti prilagođene virtualnog računala](../virtual-machines/virtual-machines-windows-classic-createportal.md).