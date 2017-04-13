<properties
   pageTitle="Konfiguriranje VPN točke web pristupnika veze s mrežom virtualne Azure pomoću portala za klasični | Microsoft Azure"
   description="Sigurno povezivanje s mrežom virtualne Azure stvaranjem točke web VPN veza pristupnika."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>Konfiguriranje veze točke web VNet pomoću portala za klasični

> [AZURE.SELECTOR]
- [Voditelj resursa – Portal za Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Voditelj resursa – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasični – Portal za Azure](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Klasični – klasični Portal](vpn-gateway-point-to-site-create.md)

Konfiguraciju točke-na-web-mjesta (P2S) omogućuje stvaranje sigurnu vezu s pojedinačne klijentskog računala da biste virtualne mreže. Povezivanje P2S je korisno kada želite povezati svoje VNet s udaljenog mjesta, kao što su od kuće ili konferencije, ili kada imate samo nekoliko klijenti koje morate se povezati s virtualne mreže.

U ovom se članku vodit će vas voditi kroz stvaranje na VNet s vezom točke na mjesto u **model klasični implementacije** pomoću **klasične portal**.

Točke web veze ne zahtijevaju VPN uređaja ili dostupnog javnosti IP adresa za rad. Pokretanjem povezivanja s klijentskom računalu je uspostavljena VPN veza. Dodatne informacije o vezama točke mjesta potražite u članku [Najčešća pitanja vezana uz VPN pristupnika](vpn-gateway-vpn-faq.md#point-to-site-connections) i [Planiranje i dizajna](vpn-gateway-plan-design.md).


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Uvođenje modela i postupci za P2S veze

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Sljedeća tablica prikazuje dva implementacije modelima i dostupne načinima za P2S konfiguracije. Kada članak s navedeni koraci za konfiguraciju postane dostupan, ne možemo povezati izravno iz u ovoj su tablici.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 



## <a name="basic-workflow"></a>Osnovni tijeka rada

![Točke-na-dijagram web-mjesta –] (./media/vpn-gateway-point-to-site-create/p2sclassic.png "točka-mjesta")
 
Sljedeći koraci će vas voditi kroz korake da biste stvorili sigurna veza točke web virtualne mreže. 

Konfiguriranje veze točke na web podijeljene u četiri sekcije. Važno je redoslijed kojim konfigurirate svaki od tih sekcija. Ne preskočite korake ili prebaciti.

- **Sekcija 1** Stvaranje virtualne mreže i VPN pristupnika.
- **Sekcija 2** Stvorite certifikate koji se koriste za provjeru autentičnosti i prenesite ih.
- **Odjeljak 3** Izvoz i instalirajte klijentskih potvrda.
- **Odjeljak 4** Konfiguriranje VPN klijent.

## <a name="vnetvpn"></a>Sekcija 1 – stvaranje virtualne mreže i pristupnika VPN-a

### <a name="part-1-create-a-virtual-network"></a>Dio 1: Stvaranje virtualne mreže

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com/). Ove korake koristite klasični portalu ne Azure portal. Trenutno ne možete stvoriti vezu P2S pomoću portala za Azure.

2. U donjem lijevom kutu zaslona kliknite **Novo**. U navigacijskom oknu kliknite **Mrežnim servisima**, a zatim **Virtualne mreže**. Kliknite **Stvaranje Prilagođeno** da biste pokrenuli čarobnjak za konfiguraciju.

3. Na stranici **Virtualne mreže pojedinosti** unesite sljedeće podatke, a zatim sljedeće strelicu dolje desno.
    - **Naziv**: naziv virtualne mreže. Na primjer, "VNet1". To je naziv koji će uputiti ako pokrenete VMs u ovom VNet.
    - **Lokacije**: lokaciju izravno povezani s fizičke mjesto (regija) koje želite da se Resursi (VMs) da biste nalaze. Na primjer, ako želite VMs koji implementirati virtualne mreži fizički nalazi u SAD-a za istočnoazijske, odaberite mjesto. Ne možete promijeniti regija povezan s mrežom virtualne kad ga stvorite.

4. Na stranici **DNS poslužitelji i grupa podaci** , unesite sljedeće podatke, a zatim dalje strelicu na desnoj.
    - **DNS poslužitelji**: Unesite naziv poslužitelja DNS-a i IP adresa ili odaberite prethodno registrirani DNS poslužitelj na izborniku prečaca. Ta postavka nije moguće stvoriti DNS poslužitelj. Omogućuje određivanje DNS poslužitelji koji želite koristiti za razrješavanje imena u pošti virtualne mreže. Ako želite koristiti servis za rješenja Azure zadanog naziva, u ovom se odjeljku ostavite prazno.
    - **VPN Konfiguriraj točke web**: Potvrdite okvir.

5. Na stranici **Povezivanje točke web** navedite rasponu IP adresa iz kojeg klijentima VPN primit će IP adresu nakon uspostave veze. Postoji nekoliko pravila koja se odnose rasponi adresa koje možete odrediti. Nije važno da biste potvrdili da raspon koji navedete ne preklapa s bilo kojeg od raspona koji se nalaze na lokalnu mrežu.

6. Unesite sljedeće podatke, a zatim kliknite strelicu za sljedeći.
 - **Prostor adrese**: Početni IP i CIDR (adresu zbroj).
 - **Dodavanje prostora na adresu**: dodavanje adresa prostora samo ako je potreban za dizajn mreže.

7. Na stranici **Virtualne razmake mreže adrese** Navedite adresu raspona koji želite koristiti za virtualne mreže. To su dinamičke IP adrese (DIPS) koji će se dodijeliti u VMs i druge instance uloge koje implementirati virtualne mreže.<br><br>To je osobito važno da biste odabrali raspon koji se ne preklapa s bilo kojeg od raspona koji se koriste za lokalnu mrežu. Morate se usklađuju s mrežni administrator, koji je možda ćete morati carve izvan raspona IP adrese iz lokalne mreže adresnog prostora možete koristiti za virtualne mreže.

8. Unesite sljedeće podatke, a zatim kliknite kvačicu da biste počeli sa stvaranjem virtualne mreže.
 - **Prostor adrese**: Dodavanje interne IP adresu raspon koji želite koristiti za virtualne mrežu, uključujući početni IP i broj. Nije važno da biste odabrali raspon koji se ne preklapa s bilo kojeg od raspona koji se koriste za lokalnu mrežu. 
 - **Dodavanje podmreže**: dodatni podmreže nisu potrebni, ali želite stvoriti zasebnu podmreže za VMs koji će imati statične DIPS. Ili, možda ćete morati imati vaše VMs podmreži razlikuje se od ostalih instanci uloge.
 - **Dodavanje pristupnika podmreže**: podmreže pristupnika nužan za VPN točke na web. Kliknite da biste dodali podmreže pristupnika. Podmreže pristupnik koristi se samo za pristupnik virtualne mreže.

9. Nakon što virtualne mreže, vidjet ćete **stvoreno** navedene u odjeljku **Status** na stranici Mreža Azure klasični portalu. Nakon što virtualne mreže, možete stvarati dinamične pristupnik za usmjeravanje.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Dio 2: Stvaranje dinamičkih usmjeravanje pristupnika

Vrsta pristupnika mora biti konfigurirana kao dinamički. Statički usmjeravanje pristupnika ne funkcionira s tom značajkom.

1. Azure klasični portalu na stranici **mreža** kliknite virtualne mreže koju ste stvorili i idite na stranicu **nadzorne ploče** .

2. Kliknite **Stvaranje pristupnika**, koja se nalazi pri dnu stranice **nadzorne ploče** . Pojavljuje se poruka koja vas pita **želite li da biste stvorili pristupnik za virtualne mreže "VNet1"**. Kliknite **da** da biste započeli stvaranje pristupnika. To može potrajati oko 15 minuta da biste stvorili pristupnik.

## <a name="generate"></a>Odjeljak 2 – Stvaranje i prijenos certifikata

Certifikati koriste se za provjeru autentičnosti klijenti VPN-a za VPN-ovi zareza web. Možete koristiti s korijenskim certifikatom generira rješenje certifikat za enterprise, ili pomoću samopotpisanog certifikata. Azure možete prenijeti do 20 korijenskih certifikata. Nakon prijenosa .cer datoteka Azure možete koristiti informacije koje se nalaze u njoj za provjeru autentičnosti u klijentskim programima koji imaju potvrde klijenta instaliran. Klijentski certifikat mora biti generiran iz istog certifikata koji predstavlja .cer datoteka.

U ovom odjeljku će učinite sljedeće:

- Nabavite .cer datoteka za s korijenskim certifikatom. To može biti samopotpisani certifikat, ili pomoću certifikata sustava enterprise.
- Prijenos datoteke .cer Azure.
- Stvaranje certifikata za klijenta.

### <a name="root"></a>Dio 1: Dobivanje .cer datoteka za korijenski certifikat

Ako koristite u sustavu za potvrdu enterprise, nabavite .cer datoteka za korijenski certifikat koji želite koristiti. U [dijelu 3](#createclientcert)generiranje klijentskih potvrda iz korijenski certifikat.

Ako se ne koriste rješenje certifikat za enterprise, morat ćete stvoriti korijenski samopotpisani certifikat. Upute za Windows 10, može se odnositi na [Rad s samopotpisani korijenskih certifikata za konfiguracije točke na web](vpn-gateway-certificates-point-to-site.md). U članku vodit će vas kroz korištenje makecert za generiranje samopotpisani certifikat, a zatim izvezite .cer datoteka.

### <a name="upload"></a>2.dio: Prenesi .cer datoteka certifikata za korijenske Azure klasični portal

Dodavanje pouzdanog potvrde Azure. Prilikom dodavanja datoteke Base64 kodirani X.509 (.cer) Azure su koja upozorava Azure pouzdanosti korijenski certifikat koji predstavlja datoteku.

1. Azure klasični portalu na stranici **Potvrda** virtualne mreže, kliknite **Prenesi s korijenskim certifikatom**.

2. Na stranici **Potvrda za prijenos** pronađite .cer korijenski certifikat, a zatim kvačicu.

### <a name="createclientcert"></a>Dio 3: Generiranje potvrdu klijenta

Nakon toga generirati potvrde klijenta. Ili možete stvoriti jedinstvene certifikat za svaki klijent koji će se povezati ili pomoću istog certifikata za druge klijente. Prednost generiranje jedinstvenih klijentskih potvrda je mogućnost da biste opozvali jedan certifikat prema potrebi. U suprotnom, ako svi koriste isti certifikat klijenta i pronaći ćete morati opoziv certifikata za jednog klijenta, morat ćete generiranje i instalirajte novog certifikata za sve klijente koji koriste certifikat za provjeru autentičnosti.

- Ako koristite rješenje certifikat za enterprise, generirati potvrdu klijenta s uobičajenih oblika vrijednost za naziv 'name@yourdomain.com', umjesto NetBIOS "Domena\korisničkoime" Oblikovanje. 

- Ako koristite samopotpisani certifikat, potražite u članku [Rad s samopotpisani korijenskih certifikata za konfiguracije točke web](vpn-gateway-certificates-point-to-site.md) da biste generirali potvrdu klijenta.

## <a name="installclientcert"></a>Odjeljak 3 – izvoziti i instaliraj certifikat klijenta

Instalirajte potvrdu klijenta na svim računalima na kojima se želite povezati virtualne mreže. Klijentski certifikat je potreban za provjeru autentičnosti. Možete automatizirati instalacije certifikat klijenta ili ručno instalirati. Sljedeći koraci prođite kroz izvoz i ručno instaliranje certifikat klijenta.

1. Da biste izvezli potvrdu klijenta, možete koristiti *certmgr.msc*. Desnom tipkom miša kliknite klijentski certifikat koji želite izvesti, kliknite **Svi zadaci**, a zatim **Izvoz**.
2. Izvoz certifikat klijenta s privatni ključ. Ovo je *.pfx* datoteka. Provjerite je li zapis ili sjetiti lozinke (tipka) koju ste postavili za taj certifikat.
3. Kopirajte *.pfx* datoteka na klijentskom računalu. Na klijentskom računalu dvokliknite *.pfx* datoteka da biste ga instalirali. Unesite lozinku za primitku. Izmjena mjesto instalacije.

## <a name="vpnclientconfig"></a>Odjeljak 4 - Konfiguriranje VPN klijent

Da biste povezali virtualne mreže, također morate konfigurirati VPN klijent. Klijent zahtijeva potvrdu klijenta i proper VPN konfiguracije klijenta radi povezivanja. Da biste konfigurirali VPN klijent, poduzeti sljedeće korake, redoslijedom.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Dio 1: Stvaranje paketa za konfiguriranje klijenta VPN-a

1. Azure klasični portalu na stranicu **nadzorne ploče** za mrežu virtualne idite na izborniku brzi pogled u desnom kutu. Na popisu klijentski operacijski sustavi koje podržava potražite u članku veze [točke-na-web-mjesta](vpn-gateway-vpn-faq.md#point-to-site-connections) odjeljku najčešća pitanja vezana uz pristupnika VPN-a. Paket za VPN klijentskog sadrži informacije o konfiguraciji Konfiguriranje VPN klijentski softver ugrađen u sustavu Windows. Paket instalirati dodatnog softvera. Postavke vrijede virtualne mrežu s kojom se želite povezati.<br><br>Odaberite preuzmite paket koji odgovara klijent operacijski sustav na kojem će se instalirati:
 - Za 32-bitne klijenata, odaberite **preuzmite paket za VPN klijentskog 32-bitni**.
 - Za 64-bitni klijenata, odaberite **preuzmite paket za VPN klijentskog 64-bitni**.

2. U svega nekoliko minuta da biste stvorili pakiranju klijenta. Kada završi paket mogu preuzeti datoteku. *.Exe* datoteku koju preuzimate možete sigurno pohranjuje na lokalnom računalu.

3. Kada generiranje i preuzmite paket VPN klijenta s portala za Azure klasični, možete instalirati paket klijenta na klijentskom računalu iz koje želite povezati virtualne mreže. Ako planirate instalirati paket za VPN klijentskog više klijentskim računalima, provjerite imaju li svaki korisnik i potvrdu klijenta instaliran.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>2.dio: Instalaciju paketa Konfiguriranje VPN-a na klijentskom računalu

1. Lokalno kopirajte konfiguracijska datoteka na računalo na kojem želite povezati virtualne mreže, a zatim dvokliknite .exe datoteke. 

2. Kada je instaliran paket, mogu početi VPN veza. Konfiguriranje paketa je potpisao Microsoft. Možda želite potpisati paketa pomoću usluge potpisivanja vaše tvrtke ili ustanove ili ga potpišete sami pomoću [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327). Nije u redu da biste pomoću značajke pakiranja bez prijave. Međutim, ako je paket nije prijavljen, pojavljuje se upozorenje pri instalaciji paketa.

3. Na klijentskom računalu, idite na **Postavke mreže** , a zatim kliknite **VPN-a**. Prikazat će se veza na popisu. Ona će se prikazati naziv virtualne mreže je da će se povezati s i će izgledati ovako: 

    ![VPN klijent] (./media/vpn-gateway-point-to-site-create/vpn.png "VPN klijent")


### <a name="part-3-connect-to-azure"></a>Dio 3: Povezivanje s Azure

1. Za povezivanje s VNet, na klijentskom računalu otvorite VPN vezama i pronađite VPN vezu koju ste stvorili. To se naziva isti naziv kao virtualne mreže. Kliknite **Poveži**. Poruka u skočnom prozoru može prikazati koje se odnosi na pomoću certifikata. Ako se to dogodi, kliknite **Nastavi** da biste koristili dodatnim ovlastima. 

2. Na stranici stanja **veze** kliknite **Poveži** da biste pokrenuli vezu. Ako se prikaže zaslon za **Odabir certifikata** , provjerite je li certifikat klijenta koji prikazuje onaj koji želite koristiti za povezivanje. Ako nije, pomoću strelice padajućeg izbornika odaberite odgovarajući certifikat, a zatim kliknite **u redu**.

    ![VPN klijent 2] (./media/vpn-gateway-point-to-site-create/clientconnect.png "VPN klijentskog veze")

3. Sada moraju uspostaviti vezu.

    ![VPN klijent 3] (./media/vpn-gateway-point-to-site-create/connected.png "VPN veza klijent 2")

### <a name="part-4-verify-the-vpn-connection"></a>Dio 4: Provjerite je li VPN veza

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

Ako želite dodatne informacije o virtualne mreže, posjetite stranicu [Virtualne dokumentaciji mreže](https://azure.microsoft.com/documentation/services/virtual-network/) .
