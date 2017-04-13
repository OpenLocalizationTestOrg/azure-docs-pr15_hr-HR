<properties 
   pageTitle="Stvaranje samopotpisane potvrde za virtualne mreže točke na web-lokacija veze pomoću makecert | Microsoft Azure"
   description="U ovom se članku navode koraci za korištenje makecert za stvaranje samopotpisane potvrde na Windows 10."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# <a name="working-with-self-signed-certificates-for-point-to-site-connections"></a>Rad s samopotpisane potvrde za točku web veze

U ovom se članku olakšava stvaranje samopotpisanog certifikata pomoću **makecert**, a zatim generiranje klijentskih potvrda iz njih. Koraci su napisani za makecert u sustavu Windows 10. Da biste stvorili certifikata koji su kompatibilni s vezama P2S provjere Makecert. 

Za veze P2S željeni način za certifikate jest korištenje rješenje enterprise certifikat, a obavezno za izdavanje certifikata klijenta s uobičajenih oblika vrijednost za naziv 'name@yourdomain.com', umjesto oblik "Name\username NetBIOS domene".

Ako nemate rješenje za enterprise, samopotpisani certifikat je potrebno da biste omogućili P2S klijentima omogućuje povezivanje virtualne mrežom. Radimo na umu da je izostavljen je makecert, ali je još uvijek valjan načina za stvaranje samopotpisane potvrde koji su kompatibilni s P2S veze. Radimo na drugo rješenje za stvaranje samopotpisane potvrde, ali trenutno makecert je željeni način.

## <a name="create-a-self-signed-certificate"></a>Stvaranje samopotpisane potvrde

Makecert je jedan od načina stvaranja samopotpisanog certifikata. Sljedeći koraci će vas voditi kroz stvaranje samopotpisanog certifikata pomoću makecert. Ove korake nisu određeni model implementacije. Su valjane za Voditelj resursa i classic.

### <a name="to-create-a-self-signed-certificate"></a>Stvaranje samopotpisane potvrde

1. Na računalu s operacijskim sustavom Windows 10, preuzmite i instalirajte [Windows Software Development Kit (SDK) za Windows 10](https://dev.windows.com/en-us/downloads/windows-10-sdk).

2. Nakon instalacije možete pronaći utility makecert.exe u odjeljku ovaj put: C:\Program Files (x86) \Windows Kits\10\bin\<arh >. 
        
    Primjer:`C:\Program Files (x86)\Windows Kits\10\bin\x64`

3. Zatim stvorite i instalirajte certifikat u spremištu osobnih certifikata na vašem računalu. Sljedeći primjer stvara odgovarajuće *.cer* datoteka koje prenijeti na Azure prilikom konfiguriranja P2S. Pokrenite sljedeću naredbu, kao administrator. Zamijenite *ARMP2SRootCert* i *ARMP2SRootCert.cer* naziv koji želite koristiti za potvrdu.<br><br>Certifikat će se nalaziti u potvrde - trenutni User\Personal\Certificates.

        makecert -sky exchange -r -n "CN=ARMP2SRootCert" -pe -a sha1 -len 2048 -ss My "ARMP2SRootCert.cer"


###  <a name="rootpublickey"></a>Da biste dobili javni ključ

U sklopu konfiguracije pristupnika VPN-a za točku web veze, javni ključ za korijenski certifikat prenošenja Azure.

1. Da biste nabavili .cer datoteka iz certifikata, otvorite **certmgr.msc**. Desnom tipkom miša kliknite korijenski samopotpisani certifikat, kliknite **Svi zadaci**, a zatim kliknite **Izvoz**. Otvara se **Čarobnjak za izvoz certifikata**.

2. U čarobnjaku kliknite **Dalje**, odaberite **ne, izvezi privatni ključ**i zatim kliknite **Dalje**.

3. Na stranici **Oblik datoteke za izvoz** odaberite **Osnovni 64 kodirani X.509 (. CER).** Zatim kliknite **Dalje**. 

4. Na u **datoteka za izvoz**, **Pronađite** mjesto na koje želite izvesti certifikata. **Naziv datoteke**, naziv datoteka certifikata. Zatim kliknite **Dalje**.

5. Kliknite **Završi** da biste izvezli certifikata.

 
### <a name="export-the-self-signed-certificate-optional"></a>Izvezite samopotpisani certifikat (nije obavezno)

Trebali biste izvezite samopotpisani certifikat i spremite ga na sigurno. Ako morate biti, možete kasnije ga instalirati na drugo računalo i generiranje više klijentskih potvrda ili neki drugi .cer datoteka za izvoz. Bilo kojem računalu s potvrdu klijenta instaliran te i konfigurirati s početnim VPN klijentskog programa postavke možete se povezati s mrežom virtualne putem P2S. Zbog toga želite da biste bili sigurni da klijentskih potvrda su generirani i instalirati samo kad je potreban i koji su samopotpisani certifikat sigurno spremljeni.

Da biste izvezli samopotpisani certifikat kao na .pfx, odaberite korijenski certifikat, a zatim koristite kao što je opisano u [Izvoz potvrdu klijenta](#clientkey) iste korake da biste izvezli.

## <a name="create-and-install-client-certificates"></a>Stvaranje i instalacija klijentskih potvrda

Nemojte instalirati samopotpisani certifikat izravno na klijentskom računalu. Morate da biste generirali potvrdu klijenta s samopotpisani certifikat. Možete, a zatim izvoz i instalirati klijentski certifikat na klijentskom računalu. Sljedeći koraci nisu određeni model implementacije. Su valjane za Voditelj resursa i classic.

### <a name="part-1---generate-a-client-certificate-from-a-self-signed-certificate"></a>Dio 1 - generiranje potvrdu klijenta iz samopotpisanog certifikata

Sljedeći koraci će vas voditi kroz jedan od načina za generiranje potvrdu klijenta s samopotpisanog certifikata. Više klijentskih potvrda može generirati iz istog certifikata. Svaki klijent certifikata pa je moguće izvoziti i instalirani na klijentskom računalu. 

1. Na istom računalu koje ste koristili za stvaranje samopotpisanog certifikata, otvorite naredbeni redak kao administrator.

2. U ovom primjeru "ARMP2SRootCert" se odnosi na samopotpisani certifikat koji ste generira. 
    - Promijenite *"ARMP2SRootCert"* naziv samopotpisani korijenske mape koje generiraju klijentska potvrda iz. 
    - Promijenite naziv koji želite generirati klijentski certifikat koji se *ClientCertificateName* . 


    Izmjena i pokrenite uzorka za generiranje potvrdu klijenta. Ako pokrenete u sljedećem primjeru bez izmjenom, rezultat je potvrdu klijenta s nazivom ClientCertificateName u spremištu osobnih certifikata koji je generirao korijenski certifikat ARMP2SRootCert.

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "ARMP2SRootCert" -is my -a sha1

4. Svi certifikati su pohranjeni u 'Certifikati - trenutni User\Personal\Certificates' spremili na računalu. Možete generirati kao što je mnogo klijentskih potvrda prema potrebi na temelju ovog postupka.

### <a name="clientkey"></a>Dio 2 - izvoz potvrdu klijenta

1. Da biste izvezli potvrdu klijenta, otvorite **certmgr.msc**. Desnom tipkom miša kliknite klijentski certifikat koji želite izvesti, kliknite **Svi zadaci**, a zatim **Izvoz**. Otvara se **Čarobnjak za izvoz certifikata**.

2. U čarobnjaku za kliknite **Dalje**, a zatim odaberite **Da, izvezi privatni ključ**pa kliknite **Dalje**.

3. Na stranici **Oblik datoteke za izvoz** možete ostaviti zadanih odabran. Zatim kliknite **Dalje**. 
 
4. Na stranici **Sigurnost** morate zaštiti privatni ključ. Ako odaberete da biste koristili lozinku, provjerite je li zapis ili Zapamti lozinku koju ste postavili za taj certifikat. Zatim kliknite **Dalje**.

5. Na u **datoteka za izvoz**, **Pronađite** mjesto na koje želite izvesti certifikata. **Naziv datoteke**, naziv datoteka certifikata. Zatim kliknite **Dalje**.

6. Kliknite **Završi** da biste izvezli certifikata.  

### <a name="part-3---install-a-client-certificate"></a>Dio 3 – Instaliraj potvrdu klijenta

Svaki klijent koji želite povezati sa virtualne mreže pomoću veze točke na web mora imati instaliran klijentski certifikat. Taj certifikat je uz potrebne paketa Konfiguriranje VPN-a. Sljedeći koraci će vas voditi kroz ručno instaliranje certifikat klijenta.

1. Pronađite i kopirajte *.pfx* datoteka na klijentskom računalu. Na klijentskom računalu dvokliknite *.pfx* datoteka da biste instalirali. Ostavite **Mjesto spremišta** kao **Trenutnog korisnika**, a zatim kliknite **Dalje**.

2. **Datoteka** za uvoz stranice ne unesite željene promjene. Kliknite **Dalje**.

3. Na stranici **zaštitu privatnog ključa** unesite lozinku za potvrdu ako ste onaj koji se koristi ili provjerite jesu li sigurnosni upravitelj koji se instalira certifikat točni, a zatim kliknite **Dalje**.

4. Na stranici **Spremište certifikata** ostavite zadano mjesto, a zatim kliknite **Dalje**.

5. Kliknite **Završi**. Na stranici **Sigurnosno upozorenje** za instaliranje certifikata, kliknite **da**. Certifikat sada uspješno uvezeni.

## <a name="next-steps"></a>Daljnji koraci

Nastavite s konfiguracijom točke na web. 

- **Voditelj resursa** implementacije modela upute potražite u članku [Konfiguriranje veze točke web VNet pomoću komponente PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md). 
- **Klasični** koraka model implementacije, potražite u članku [VNet pomoću portala za klasični VPN vezu konfiguriranje točke-na-mjesto](vpn-gateway-point-to-site-create.md).