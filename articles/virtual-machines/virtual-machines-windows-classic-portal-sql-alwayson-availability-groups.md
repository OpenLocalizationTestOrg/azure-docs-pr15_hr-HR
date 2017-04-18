<properties
    pageTitle="Konfiguriranje uvijek na grupe dostupnosti u Azure VM – klasični"
    description="Stvaranje grupe sustava uvijek o dostupnosti s Azure virtualnim računalima. Pomoću ovog praktičnog vodiča prvenstveno koristi korisničkog sučelja i alate umjesto skriptiranje."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm---classic"></a>Konfiguriranje uvijek na grupe dostupnosti u Azure VM – klasični

> [AZURE.SELECTOR]
- [Voditelj resursa: predloška](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Voditelj resursa: ručno](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klasični: korisničko Sučelje](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klasični: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Pomoću ovog praktičnog vodiča završetka do kraja pokazuje kako implementirati pomoću SQL Server uvijek sustavom Azure virtualnim strojevima grupe dostupnosti.

Na kraju vodič rješenje SQL Server uvijek na servisu Azure će se sastojati od sljedećih elemenata:

- Virtualne mreže koja sadrži više podmreže, uključujući korisničko sučelje i pozadinske podmreže

- Kontroler domene domenu Active Directory (AD)

- Dva SQL Server VMs implementiran na podmreži pozadinsku i pridruženo domeni AD

- 3 čvor WSFC klaster s modelom kvorum Većina čvor

- Dostupnost grupe sustava s dvije replike sinkrono potvrdi dostupnosti baze podataka

Na slici u nastavku je grafički prikaz rješenja.

![Testiranje Laboratorija arhitektura za AG servisu Azure](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Imajte na umu da je to jedne moguće konfiguracije. Ako, na primjer, možete smanjiti broj VMs dostupnost dvije replike grupe da biste spremili na računalnim sate u Azure pomoću kontrolerom domene kao zrcaljenja zajedničko korištenje datoteka na kvorum klasteru WSFC 2 čvor. Ta metoda smanjuje broj VM jednu od gore konfiguracije.

Pomoću ovog praktičnog vodiča podrazumijeva sljedeće:

- Već imate račun za Azure.

- Dodjela resursa za klasični VM SQL Server iz galerije virtualnog računala pomoću u GUI znate već.

- Već imate pune razumijevanja uvijek na dostupnost grupe. Dodatne informacije potražite u članku [Uvijek na grupe dostupnosti (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Ako vas zanima pomoću uvijek na dostupnost grupe sustava SharePoint, pogledajte i [Konfiguriranje SQL Server 2012 uvijek uključeno dostupnost grupe za SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).

## <a name="create-the-virtual-network-and-domain-controller-server"></a>Stvaranje virtualne mreže i poslužitelja kontroler domene

Započnite pomoću novog računa Azure probno razdoblje. Kad dovršite postavljanje vašeg računa, morate biti na početnom zaslonu Azure klasični portal.

1. Kliknite gumb **Novo** u donjem lijevom kutu stranice, kao što je prikazano u nastavku.

    ![Kliknite Novo na portalu](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)

1. Kliknite **Mrežnim servisima**, a zatim kliknite **Virtualne mreže** , a zatim kliknite **Stvaranje prilagođenih**, kao što je prikazano u nastavku.

    ![Stvaranje virtualne mreže](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)

1. U dijaloškom okviru **Stvaranje VIRTUALNE MREŽA** stvorite novi virtualne mreže prolaskom kroz stranice s postavkama u nastavku. 

  	|Stranica|Postavke|
|---|---|
|Detalji o virtualne mreže|**Naziv = ContosoNET**<br/>**REGIJA = Zapad SAD-a**|
|DNS poslužitelji i povezivanje VPN-a|Ništa|
|Razmaci adresu virtualne mreže|Postavke prikazane su u nastavku snimku zaslona: ![Stvaranje virtualne mreže](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png)|

1. Nakon toga stvorite VM ćete koristiti kao kontrolerom domene (Kontroler). Kliknite **Novo** , a zatim **izračunati**, a zatim **virtualnog računala**, a zatim **Iz galerije**kao što je prikazano u nastavku.

    ![Stvaranje na VM](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)

1. U dijaloškom okviru **Stvaranje odgovora VIRTUALNOG računala** , konfigurirati novu VM prolaskom kroz stranice s postavkama u nastavku. 

  	|Stranica|Postavke|
|---|---|
|Odaberite operacijski sustav virtualnog računala|Podatkovnog centra sustava Windows Server 2012 R2|
|Konfiguracija virtualnog računala|**Datum IZDANJA verzija** = (najnoviji)<br/>**Naziv VIRTUALNOG računala** = ContosoDC<br/>**RAZINA** = STANDARD<br/>**VELIČINA** = A2 (2 jezgri)<br/>**NOVO KORISNIČKO ime** = AzureAdmin<br/>**NOVU lozinku** = Contoso! 000<br/>**Potvrda** = Contoso! 000|
|Konfiguracija virtualnog računala|**Servis u OBLAKU** = Stvori novi servis u oblaku<br/>**OBLAK usluge DNS naziv** = naziv servisa jedinstveni oblaka<br/>**Naziv DNS-a** = jedinstveni naziv (ex: ContosoDC123)<br/>**PODRUČJE/GRUPE AFINITET/VIRTUALNE MREŽE** = ContosoNET<br/>**VIRTUALNA MREŽE PODMREŽE** = Back(10.10.2.0/24)<br/>**RAČUN za POHRANU** = korištenje računa za automatski generirani prostora za pohranu<br/>**Postavljanje dostupnosti** = (ništa)|
|Mogućnosti virtualnog računala|Korištenje zadanih postavki|

Kada završite s konfiguracijom novi VM, pričekajte VM biti provsioned. Ovaj postupak traje dulje vrijeme, a ako kliknete na karticu **virtualnog računala** na portalu za Azure klasični, vidjet ćete ContosoDC cycling stanja iz **početka (Provisioning)** **zaustavljanja**, **Početni**, **pokretanje (Provisioning)**i na kraju **pokrenut**.

Poslužitelj za Kontroler sada uspješno dodijeljeni resursi. Nakon toga će konfigurirati domene servisa Active Directory na ovaj poslužitelj Kontroler.

## <a name="configure-the-domain-controller"></a>Konfiguriranje kontrolerom domene

U sljedećim koracima konfigurirati računalo ContosoDC kao kontroler domene za corp.contoso.com.

1. Na portalu odaberite **ContosoDC** računala. Na kartici **nadzorne ploče** kliknite **Poveži** da biste otvorili datoteku RDP za daljinski pristup za stolna računala.

    ![Povezivanje s računala Vritual](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)

1. Prijavite se pomoću vašeg konfiguriran i lozinku za administratorski račun (**\AzureAdmin**) (**Contoso! 000**).

1. Prema zadanim postavkama, trebali biste prikazati na nadzornoj ploči **Upravitelj poslužitelja** .

1. Kliknite vezu **Dodaj uloga i značajki** na nadzornoj ploči.

    ![Poslužitelj Explorer dodavanje uloge](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)

1. Dok ne dođete do sekcije **Uloge poslužitelja** , odaberite **Dalje** .

1. Odaberite uloge **Servisa Active Directory Domain Services** i **DNS poslužitelj** . Kada se to od vas zatraži, dodajte sve dodatne značajke potrebnih te uloge.

    >[AZURE.NOTE] Dobit ćete Provjera valjanosti upozorenje da postoji statičke IP adrese. Ako želite testirati konfiguraciju, kliknite Nastavi. Za radni scenariji [pomoću komponente PowerShell da biste postavili statičke IP adresu na ovom računalu kontroler domene](./virtual-network/virtual-networks-reserved-private-ip.md).

    ![Dodavanje dijaloški uloge](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)

1. Dok ne dođete do sekcije za **potvrdu** , kliknite **Dalje** . Potvrdite okvir **automatski ako je potrebno ponovno pokrenuti na odredišnom poslužitelju** .

1. Kliknite **Instaliraj**.

1. Nakon značajke završite s instalacijom, vratite se na nadzornoj ploči **Upravitelj poslužitelja** .

1. Odaberite novi **AD DS** mogućnost u lijevom oknu.

1. Kliknite vezu za **Dodatne** na žutoj traci upozorenje.

    ![Dijaloški okvir s AD DS o VM DNS poslužitelja](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)

1. U stupcu **Akcija** u dijaloškom okviru **Sve detalje o zadatku poslužitelja** kliknite **Digni razinu poslužitelja kontroler domene**.

1. U **Čarobnjak Active Directory Domain Services konfiguracije**, koristite sljedeće vrijednosti:

  	|Stranica|Postavka|
|---|---|
|Konfiguraciju implementacije|**Dodavanje skupa stabala novo** = odabrani<br/>**Naziv domene korijenske** = corp.contoso.com|
|Mogućnosti kontroler domene|**Lozinka** = Contoso! 000<br/>**Potvrdite lozinku** = Contoso! 000|

1. Kliknite **Dalje** prođite kroz druge stranice u čarobnjaku. Na stranici **Provjera preduvjeti** , provjerite je li prikazat će se sljedeća poruka: **sve provjere pripremni proslijeđena uspješno**. Imajte na umu da biste trebali pregledati sve odgovarajuće poruke upozorenja, ali je moguće da biste nastavili s instalaciju.

1. Kliknite **Instaliraj**. Virtualnog računala **ContosoDC** će automatski ponovno pokrenuti računalo.

## <a name="configure-domain-accounts"></a>Konfiguriranje računa domene

Daljnji koraci konfiguriranje računa za Active Directory (AD) za kasnije korištenje.

1. Prijavite se vratite u **ContosoDC** računala.

1. U **Upravitelju poslužitelja** odaberite **Alati** , a zatim kliknite **Centar za Administrativni imenika Active**.

    ![Administracijski centar za Active Directory](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)

1. U u **Centar za Administrativni imenika Active** odaberite **corp (lokalno)** u lijevom oknu.

1. U desnom oknu **zadataka** , odaberite **Novo** , a zatim kliknite **korisnika**. Pomoću sljedećih postavki:

  	|Postavka|Vrijednost|
|---|---|
|**Ime**|Instalacija|
|**SamAccountName korisnika**|Instalacija|
|**Lozinke**|Contoso! 000|
|**Potvrdite lozinku**|Contoso! 000|
|**Druge mogućnosti lozinke**|Odabrana|
|**Lozinka nikad ne ističe**|Potvrđen okvir|

1. Kliknite **u redu** da biste stvorili korisnika **instalirati** . Račun će se koristiti za konfiguriranje klaster prebacivanje i grupe dostupnosti.

1. Stvorite dva ostale korisnike s iste korake: **CORP\SQLSvc1** i **CORP\SQLSvc2**. Poslovne subjekte će se koristiti za instance sustava SQL Server. Ćete morati dati **CORP\Install** potrebne dozvole za konfiguriranje Windows servisa prebacivanje Klasteriranje (WSFC).

1. U odjeljku **Centar za Administrativni imenika Active**odaberite **corp (lokalno)** u lijevom oknu. Zatim u desnom oknu **Zadaci** , kliknite **Svojstva**.

    ![Svojstva CORP korisnika](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)

1. Odabir **proširenja**, a zatim kliknite gumb **Dodatno** na kartici **Sigurnost** .

1. U dijaloškom okviru **Napredne postavke sigurnosti za corp** . Kliknite **Dodaj**.

1. Kliknite **Odaberite glavni**. Zatim potražite **CORP\Install**. Kliknite **u redu**.

1. Odaberite dozvole za **čitanje svih svojstava** i **Stvaranje računalne objekte** .

    ![Corp korisničkih dozvola](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)

1. Kliknite **u redu**, a zatim kliknite **u redu** . Zatvorite prozor corp svojstva.

Sad kad ste gotovi s Konfiguriranje servisa Active Directory i objektima korisnika, će stvoriti tri VMs SQL Server i pridružiti ovu domenu.

## <a name="create-the-sql-server-vms"></a>Stvaranje VMs za SQL Server

Nakon toga stvorite tri VMs, uključujući čvor klaster WSFC i dva VMs SQL Server. Da biste stvorili svih na VMs, vratite se na portal Azure klasični, kliknite **Novo**, **izračunati**, **virtualnog računala**, a zatim **Iz galerije**. Zatim pomoću predložaka u tablici u nastavku možete stvoriti na VMs.

|Stranica|VM1|VM2|VM3|
|---|---|---|---|
|Odaberite operacijski sustav virtualnog računala|**Podatkovnog centra sustava Windows Server 2012 R2**|**Enterprise RTM za SQL Server 2014.**|**Enterprise RTM za SQL Server 2014.**|
|Konfiguracija virtualnog računala|**Datum IZDANJA verzija** = (najnoviji)<br/>**Naziv VIRTUALNOG računala** = ContosoWSFCNode<br/>**RAZINA** = STANDARD<br/>**VELIČINA** = A2 (2 jezgri)<br/>**NOVO KORISNIČKO ime** = AzureAdmin<br/>**NOVU lozinku** = Contoso! 000<br/>**Potvrda** = Contoso! 000|**Datum IZDANJA verzija** = (najnoviji)<br/>**Naziv VIRTUALNOG računala** = ContosoSQL1<br/>**RAZINA** = STANDARD<br/>**VELIČINA** = A3 (4 jezgri)<br/>**NOVO KORISNIČKO ime** = AzureAdmin<br/>**NOVU lozinku** = Contoso! 000<br/>**Potvrda** = Contoso! 000|**Datum IZDANJA verzija** = (najnoviji)<br/>**Naziv VIRTUALNOG računala** = ContosoSQL2<br/>**RAZINA** = STANDARD<br/>**VELIČINA** = A3 (4 jezgri)<br/>**NOVO KORISNIČKO ime** = AzureAdmin<br/>**NOVU lozinku** = Contoso! 000<br/>**Potvrda** = Contoso! 000|
|Konfiguracija virtualnog računala|**Servis u OBLAKU** = prethodno stvorena Oblaku jedinstveni naziv za DNS usluge (ex: ContosoDC123)<br/>**PODRUČJE/GRUPE AFINITET/VIRTUALNE MREŽE** = ContosoNET<br/>**VIRTUALNA MREŽE PODMREŽE** = Back(10.10.2.0/24)<br/>**RAČUN za POHRANU** = korištenje računa za automatski generirani prostora za pohranu<br/>**Postavljanje dostupnosti** = Stvori raspoloživost postavljanje<br/>**DOSTUPNOST POSTAVITE naziv** = SQLHADR|**Servis u OBLAKU** = prethodno stvorena Oblaku jedinstveni naziv za DNS usluge (ex: ContosoDC123)<br/>**PODRUČJE/GRUPE AFINITET/VIRTUALNE MREŽE** = ContosoNET<br/>**VIRTUALNA MREŽE PODMREŽE** = Back(10.10.2.0/24)<br/>**RAČUN za POHRANU** = korištenje računa za automatski generirani prostora za pohranu<br/>**Postavljanje dostupnosti** = SQLHADR (možete konfigurirati dostupnost postavljanje nakon stvaranja na računalu. Sve tri strojeva trebaju biti dodijeljeni dostupnosti skupa SQLHADR.)|**Servis u OBLAKU** = prethodno stvorena Oblaku jedinstveni naziv za DNS usluge (ex: ContosoDC123)<br/>**PODRUČJE/GRUPE AFINITET/VIRTUALNE MREŽE** = ContosoNET<br/>**VIRTUALNA MREŽE PODMREŽE** = Back(10.10.2.0/24)<br/>**RAČUN za POHRANU** = korištenje računa za automatski generirani prostora za pohranu<br/>**Postavljanje dostupnosti** = SQLHADR (možete konfigurirati dostupnost postavljanje nakon stvaranja na računalu. Sve tri strojeva trebaju biti dodijeljeni dostupnosti skupa SQLHADR.)|
|Mogućnosti virtualnog računala|Korištenje zadanih postavki|Korištenje zadanih postavki|Korištenje zadanih postavki|

<br/>

>[AZURE.NOTE] Prethodne konfiguracije predlaže STANDARDNE sloju virtualnim strojevima jer OSNOVNI sloju strojeva podržava uravnoteženja krajnje točke trebati kasnije stvorite programa slušače grupe dostupnosti. Osim toga, veličina strojno predložena ovdje namijenjeni su za testiranje grupe dostupnosti u Azure VMs. Najbolje performanse radnih opterećenja radnog potražite u članku preporuke za SQL Server računalu veličine i konfiguracija u [performanse najbolje prakse za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-performance.md).

Kada su tri VMs potpuno dodjeli, morate pridružiti **corp.contoso.com** domene i dajte CORP\Install administratorska prava za strojeva. Da biste to učinili, poduzmite sljedeće korake za svaku od tri VMs.

1. Najprije, promijenite željene adresu DNS poslužitelja. Pokrenite preuzimanje svaki VM udaljene radne površine (RDP) datoteke na lokalnom direktoriju tako da odaberete u VM na popisu, a zatim kliknete gumb **za povezivanje** . Da biste odabrali na VM, kliknite bilo gdje osim na prvu ćeliju u retku, kao što je prikazano u nastavku.

    ![Preuzimanje datoteke RDP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)

1. Pokrenite datoteku RDP koji ste preuzeli i prijavite se u VM pomoću svoje konfiguriran i lozinku za administratorski račun (**BUILTIN\AzureAdmin**) (**Contoso! 000**).

1. Kada ste prijavljeni, trebali biste vidjeti na nadzornoj ploči **Upravitelj poslužitelja** . U lijevom oknu kliknite **Lokalni poslužitelj** .

1. Odaberite vezu **IPv4 adresa dodijelio DHCP, IPv6 omogućen** .

1. U prozoru **Mrežne veze** , odaberite ikonu mreže.

    ![Promjena VM Preferirani DNS poslužitelj](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)

1. Na naredbenoj traci kliknite **Promijeni postavke ove veze** (ovisno o veličini prozora, možda ćete morati kliknite dvostruka strelica udesno da biste vidjeli tu naredbu).

1. Odaberite **Internet Protocol verzije 4 (TCP/IPv4)** , a zatim kliknite Svojstva.

1. Odaberite koristi sljedeće DNS poslužitelj adrese i određivanje **10.10.2.4** u **Preferirani DNS poslužitelj**.

1. Adresa **10.10.2.4** adresa dodijeljena VM u podmreže 10.10.2.0/24 u Azure virtualne mreže, a taj VM **ContosoDC**. Da biste provjerili **ContosoDC**je IP adresa, koristite **nslookup contosodc** u naredbenom retku, kao što je prikazano u nastavku.

    ![Pomoću alata NSLOOKUP pronađite IP adresu za Kontroler](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)

1. Kliknite O**K** , a zatim **Zatvori** da biste potvrdili promjene. Sada se možete uključiti u VM za **corp.contoso.com**.

1. U prozoru **Lokalni poslužitelj** , kliknite vezu **radne GRUPE** .

1. U odjeljku **Naziv računala** , kliknite **Promijeni**.

1. Potvrdite okvir **domenu** , a zatim upišite **corp.contoso.com** u tekstni okvir. Kliknite **u redu**.

1. U dijaloškom okviru skočni prozor **Sigurnost sustava Windows** , navedite vjerodajnice za zadanu domenu administratorski račun (**CORP\AzureAdmin**) i lozinku (**Contoso! 000**).

1. Kada se prikaže poruka "Dobro došli u corp.contoso.com domene", kliknite **u redu**.

1. Kliknite **Zatvori**, a zatim **Ponovno pokrenite sada** u dijaloškom okviru skočni prozor.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-vm"></a>Dodavanje korisnika Corp\Install kao administrator na svakom VM:

1. Pričekajte da se pokrene u VM, a zatim pokretanje RDP datoteku da biste se prijavili u VM pomoću **BUILTIN\AzureAdmin** računa.

1. U **Upravitelju poslužitelja** odaberite **Alati**, a zatim **Upravljanje računalom**.

    ![Upravljanje računalom](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)

1. U prozoru za **Upravljanje računalom** proširite **lokalne korisnike i grupe**, a zatim odaberite **grupe**.

1. Dvokliknite grupu **administratora** .

1. U dijaloškom okviru **Svojstva administratori** , kliknite gumb **Dodaj** .

1. Unesite korisničko **CORP\Install**, a zatim kliknite **u redu**. Kada se pojavi upit za vjerodajnice, koristite račun **AzureAdmin** s na **Contoso! 000** lozinku.

1. Kliknite **u redu** da biste zatvorili dijaloški okvir **Svojstva za administratora** .

### <a name="add-the-failover-clustering-feature-to-each-vm"></a>Dodavanje značajke **Klasteriranja** u svakom VM.

1. Na nadzornoj ploči **Upravitelj poslužitelja** kliknite **Dodaj uloga i značajki**.

1. **Dodavanje uloge i značajke čarobnjak**, kliknite **Dalje** dok se ne prikaže na stranici **značajke** .

1. Odaberite **klasteriranja**. Kada se to od vas zatraži, dodajte druge zavisne značajke.

    ![Dodavanje prebacivanje Klasteriranje značajke za VM](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)

1. Kliknite **Dalje**, a zatim na stranici za **potvrdu** kliknite **Instaliraj** .

1. Nakon dovršetka instalacije značajke **Klasteriranja** kliknite **Zatvori**.

1. Odjavite se iz aplikacije na VM.

1. Ponovite korake u ovom odjeljku za sve tri poslužitelje – **ContosoWSFCNode**, **ContosoSQL1**i **ContosoSQL2**.

VMs za SQL Server su sada dodjeli i pokretanje, no oni su instalirani sa sustavom SQL Server s zadane mogućnosti.

## <a name="create-the-wsfc-cluster"></a>Stvaranje WSFC klaster

U ovom ćete odjeljku stvoriti klaster WSFC koji će biti smješteno grupe dostupnosti će stvoriti kasnije. Tako da sada, trebali biste poduzeti sljedeće za svaku od tri VMs će se koristiti u klaster WSFC:

- Potpuno dodijeljena Azure

- Spojena VM domeni

- Dodane **CORP\Install** za lokalne grupe administratora

- Dodali značajku klasteriranja

Sve to su preduvjeti na svakom VM prije nego što se WSFC klaster.

Osim toga, imajte na umu da Azure virtualne mreže se ponašaju na isti način kao lokalne mreže. Morate stvoriti klaster sljedećim redoslijedom:

1. Stvaranje jednog čvor klaster na jedan od čvorove (**ContosoSQL1**).

1. Izmjena klaster IP adresa koji se ne koriste IP adrese (**10.10.2.101**).

1. Premjesti naziv klaster na Internetu.

1. Dodajte druge čvorove (**ContosoSQL2** i **ContosoWSFCNode**).

Slijedite korake u nastavku da biste izvršili ove zadatke koji potpuno konfigurira klaster.

1. Pokrenite datoteku RDP za **ContosoSQL1** i prijavite se pomoću računa za domenu **CORP\Install**.

1. Na nadzornoj ploči **Upravitelj poslužitelja** odaberite **Alati**, a zatim **Upravitelj klaster prebacivanje**.

1. U lijevom oknu desnom tipkom miša kliknite **Upravitelj prebacivanje klaster**, a zatim **Stvori klaster**, kao što je prikazano u nastavku.

    ![Stvaranje klaster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)

1. U čarobnjaku za stvaranje klaster stvorite jedan čvor klaster prolaskom kroz stranice s postavkama u nastavku:

  	|Stranica|Postavke|
|---|---|
|Prije početka|Korištenje zadanih postavki|
|Odaberite poslužitelje|Upišite **ContosoSQL1** **Unesite naziv poslužitelja** , a zatim kliknite **Dodaj**|
|Upozorenje za provjeru valjanosti|Odaberite **ne. ne zahtijeva podršku tvrtke Microsoft za ovaj klaster i zbog toga ne želite pokrenuti testove provjere valjanosti. Kada kliknem dalje, nastavak stvaranja klaster**.|
|Pristupnu točku za administriranje klaster|Vrsta **Cluster1** u **nazivu klaster**|
|Potvrda|Koristite zadane postavke osim ako ne koristite razmake prostora za pohranu. Pogledajte napomenu pratiti tu tablicu.|

    >[AZURE.WARNING] Ako koristite [Za pohranu razmake](https://technet.microsoft.com/library/hh831739)koje grupira više diskova u grupe za pohranu, morate poništite okvir **dodajte sve uvjete za pohranu za klaster** potvrdnog okvira na stranici za **potvrdu** . Ako ne poništite tu mogućnost, virtualne diskova će biti odvojena tijekom postupka klasteriranja. Kao rezultat, oni se neće prikazivati u Upravitelj diskova ili Explorer dok za to predviđen prostor za pohranu uklonjeni su iz klaster i reattached pomoću komponente PowerShell.

1. U lijevom oknu proširite **Prebacivanje klaster Manager**, a zatim **Cluster1.corp.contoso.com**.

1. U oknu centra pomaknite se prema dolje do odjeljka **Klaster temeljni resursi** i proširivanje na **naziv: Clutser1** pojedinosti. Prikazat će se **naziv** i **IP adrese** resursa u stanju **nije uspjelo** . Resurs IP adresa ne može biti na mreži jer Klaster se dodjeljuje istu IP adresu kao na ovom računalu, koji je dupliciranih adresa.

1. Desnom tipkom miša kliknite neuspjelih resursa **IP adrese** , a zatim kliknite **Svojstva**.

    ![Svojstva klaster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)

1. Odaberite **Statičke IP adrese** i navedite **10.10.2.101** u tekstni okvir adresa. Nakon toga kliknite **u redu**.

1. U odjeljku **Klaster temeljni resursi** desnom tipkom miša kliknite **naziv: Cluster1** i kliknite **Premjesti na mreži**. Zatim, pričekajte dok se ne oba resursi koji su na mreži. Kada se naziv resursa klaster online, on se ažurira poslužitelja Kontroler pomoću novog računa računala AD. Taj račun AD će se koristiti za pokrenuti servis za grupu grupirani dostupnost kasnije.

1. Na kraju, dodajte preostale čvorove klaster. U stablu preglednika desnom tipkom miša kliknite **Cluster1.corp.contoso.com** , a zatim kliknite **Dodaj čvor**, kao što je prikazano u nastavku.

    ![Dodavanje čvora klaster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)

1. U **Čarobnjaku za dodavanje čvor**, kliknite **Dalje**. Na stranici **Odabir poslužitelja** dodajte **ContosoSQL2** i **ContosoWSFCNode** na popis tako da upišete naziv poslužitelja u **Unesite naziv poslužitelja** , a zatim klikom na **Dodaj**. Kada završite, kliknite **Dalje**.

1. Na stranici **Provjera valjanosti upozorenje** kliknite **ne** (u scenariju radnog trebate provesti testova provjere valjanosti). Zatim kliknite **Dalje**.

1. Na stranici za **potvrdu** kliknite **Dalje** da biste dodali čvorove.

    >[AZURE.WARNING] Ako koristite [Za pohranu razmake](https://technet.microsoft.com/library/hh831739)koje grupira više diskova u grupe za pohranu, morate poništite potvrdni okvir za **Dodavanje sve uvjete za pohranu za klaster** . Ako ne poništite tu mogućnost, virtualne diskova će biti odvojena tijekom postupka klasteriranja. Kao rezultat, oni se neće prikazivati u Upravitelj diskova ili Explorer dok za to predviđen prostor za pohranu uklonjeni su iz klaster i reattached pomoću komponente PowerShell.

1. Kada se čvorove dodaju klaster, kliknite **Završi**. Prebacivanje klaster Upravitelj treba sada prikazuju svoj klaster ima tri čvorove te ga naveli u spremniku **čvorove** .

1. Odjavite se iz aplikacije sesiji udaljene radne površine.

## <a name="prepare-the-sql-server-instances-for-availability-group"></a>Priprema instance sustava SQL Server za grupe dostupnosti

U ovom ćete odjeljku će učinite sljedeće na **ContosoSQL1** i **contosoSQL2**:

- Dodavanje prijave za **NT AUTHORITY\System** s potrebne dozvole postavite zadane instance sustava SQL Server

- Dodavanje **CORP\Install** kao uloga sustava zadane instance sustava SQL Server

- Otvorite Vatrozid za daljinski pristup sustava SQL Server

- Omogućite značajku uvijek na grupe dostupnosti

- Promjena računa servisa SQL Server **CORP\SQLSvc1** i **CORP\SQLSvc2**, odnosno

Ove se radnje može izvršiti bilo kojim redoslijedom. Ipak, će korake u nastavku vode kroz njih redoslijedom. Slijedite korake za **ContosoSQL1** i **ContosoSQL2**:

1. Ako vam se niste prijavili iz sesiji udaljene radne površine za na VM, učinite to sada.

1. Pokrenite RDP datoteka za **ContosoSQL1** i **ContosoSQL2** i prijavite se kao **BUILTIN\AzureAdmin**.

1. Najprije dodajte **NT AUTHORITY\System** za prijave u sustav SQL Server i s potrebne dozvole. Pokrenite **SQL Server Management Studio**.

1. Kliknite **Poveži se** povezati s zadane instance sustava SQL Server.

1. U programu **Explorer objekta**, proširite **Sigurnost**, a zatim **prijave**.

1. Desnom tipkom miša kliknite **NT AUTHORITY\System** prijava, a zatim kliknite **Svojstva**.

1. Na stranici **Securables** za lokalni poslužitelj, odaberite **Dodjela** dozvola za sljedeće, a zatim kliknite **u redu**.

    - Promijeniti bilo koje grupe dostupnosti

    - Povezivanje SQL

    - Stanje poslužitelja za prikaz

1. Zatim dodajte **CORP\Install** kao uloga **sustava** zadane instance sustava SQL Server. U programu **Explorer objekta**, desnom tipkom miša kliknite **prijave** , a zatim kliknite **Novi prijava**.

1. Upišite **CORP\Install** **korisničko ime za prijavu**.

1. Na stranici **Uloge poslužitelja** odaberite **sustava**. Nakon toga kliknite **u redu**. Nakon stvaranja prijave, možete ga vidjeti proširenjem **prijave** u **Programu Explorer objekta**.

1. Zatim stvorite pravilo Vatrozid za SQL Server. Na **početnom** zaslonu pokrenite **Vatrozid za Windows s dodatnom sigurnošću**.

1. U lijevom oknu odaberite **Ulazna pravila**. U desnom oknu kliknite **Novo pravilo**.

1. Na stranici **Vrste pravila** odaberite **Program**, a zatim kliknite **Dalje**.

1. Na stranici **programa** odaberite **ovaj put program** , a zatim upišite %ProgramFiles%\Microsoft **SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** u tekstni okvir (Ako pratite ove upute, ali pomoću SQL Server 2012, SQL Server je direktorij **MSSQL11. MSSQLSERVER**). Zatim kliknite **Dalje**.

1. Na stranici **Akcija** zadržati **Dopusti veze** odabrali, a zatim kliknite **Dalje**.

1. Na stranici **profila** , prihvatite zadane postavke, a zatim kliknite **Dalje**.

1. Na stranici **naziv** unesite naziv pravila, primjerice, **SQL Server (Program pravilo)** u tekstni okvir **naziv** , a zatim kliknite **Završi**.

1. Nakon toga omogućite značajku **Uvijek na dostupnost grupe** . Na **početnom** zaslonu pokrenite **Upravitelj konfiguracije SQL poslužitelja**.

1. U stablu preglednika kliknite **Servisa SQL Server**, a zatim desnom tipkom miša kliknite servisa **SQL Server (MSSQLSERVER)** , a zatim kliknite **Svojstva**.

1. Kliknite karticu **Uvijek na visoke dostupnosti** , a zatim odaberite **Omogući uvijek na dostupnost grupe**, kao što je prikazano u nastavku, a zatim **Primijeni**. Kliknite **u redu** u dijaloškom okviru blokatora, a još zatvorite prozor svojstva. Nakon što promijenite račun servisa će se pokrenuti servis sustava SQL Server.

    ![Uvijek Omogući na grupe dostupnosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)

1. Nakon toga možete promijeniti račun servisa SQL Server. Kliknite karticu **Prijava** , a zatim upišite **Naziv računa**, **CORP\SQLSvc1** (za **ContosoSQL1**) ili **CORP\SQLSvc2** (za **ContosoSQL2**), a zatim ispunite i potvrdite lozinku, a zatim **u redu**.

1. U skočnom prozoru, kliknite **da** da biste ponovno pokrenuli servis sustava SQL Server. Nakon ponovnog pokretanja servisa SQL Server promjene u prozoru svojstva su učinkoviti.

1. Odjavite se iz aplikacije u VMs.

## <a name="create-the-availability-group"></a>Stvaranje grupe dostupnosti

Sada ste spremni za konfiguriranje programa grupe dostupnosti. U nastavku je strukture će učiniti:

- Stvaranje nove baze podataka (**MyDB1**) na **ContosoSQL1**

- Potpuno sigurnosno kopiranje i transakcije zapisnika sigurnosnu kopiju baze podataka

- Vraćanje potpunu i prijavite se sigurnosno kopiranje **ContosoSQL2** s mogućnošću **NORECOVERY**

- Stvaranje grupe dostupnosti (**AG1**) s sinkrono potvrdi, automatsko prebacivanje i čitljiv sekundarnog replike

### <a name="create-the-mydb1-database-on-contososql1"></a>Stvaranje baze podataka za MyDB1 na ContosoSQL1:

1. Ako ste nije već prijavljeni iz sesije udaljene radne površine za **ContosoSQL1** i **ContosoSQL2**, učinite to sada.

1. Pokrenite datoteku RDP za **ContosoSQL1** i prijavite se kao **CORP\Install**.

1. U **Eksploreru za datoteke**, u odjeljku * *C:\**, stvorite direktorij pod nazivom * *sigurnosne kopije**. Koristite ovaj Upotrijebi direktorija za sigurnosno kopiranje i vraćanje baze podataka.

1. Desnom tipkom miša kliknite novi direktorij, pokažite na **zajedničko korištenje**, a zatim **određenim osobama**, kao što je prikazano u nastavku.

    ![Stvorite sigurnosnu kopiju mape](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)

1. Dodavanje **CORP\SQLSvc1** i dati dozvolu za **Čitanje/pisanje** , a zatim dodavanje **CORP\SQLSvc2** i joj dodijeliti dozvolu za **čitanje** , kao što je prikazano u nastavku, a zatim **zajedničko korištenje**. Nakon dovršetka postupka za zajedničko korištenje datoteka, kliknite **gotovo**.

    ![Dodjela dozvola za sigurnosnu kopiju mape](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)

1. Nakon toga stvorite bazu podataka. S izbornika **Start** pokretanje **SQL Server Management Studio**, a zatim kliknite **Poveži se** povezati s zadane instance sustava SQL Server.

1. U programu **Explorer objekta**, desnom tipkom miša kliknite **baze podataka** , a zatim kliknite **Novu bazu podataka**.

1. U **Naziv baze podataka**, upišite **MyDB1**, a zatim kliknite **u redu**.

### <a name="take-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Iskoristite potpuno sigurnosnu kopiju MyDB1 i vraćanje na ContosoSQL2:

1. Nakon toga snimljenih potpuno sigurnosnu kopiju baze podataka. U **Programu Explorer objekt**proširite **baza podataka**, a zatim desnom tipkom miša kliknite **MyDB1**, a zatim pokažite na **mogućnost zadaci**, a zatim **Sigurnosno kopiranje**.

1. U odjeljku **izvor** Zadrži **sigurnosne kopije vrsta** postavljena na **punu**. U odjeljku **Odredišni** kliknite **Ukloni** da biste uklonili zadani put datoteke za datoteku sigurnosne kopije.

1. U odjeljku **Odredišni** kliknite **Dodaj**.

1. U tekstni okvir **naziv datoteke** upišite ** \\ContosoSQL1\backup\MyDB1.bak**. Nakon toga kliknite **u redu**, a zatim **u redu** da biste se sigurnosno kopiranje baze podataka. Kada završi postupak sigurnosnog kopiranja, kliknite **u redu** da biste zatvorili dijaloški okvir.

1. Nakon toga snimljenih zapisnik transakcija sigurnosnu kopiju baze podataka. U **Programu Explorer objekt**proširite **baze podataka**, a zatim desnom tipkom miša kliknite **MyDB1**, a zatim pokažite na **mogućnost zadaci**, a zatim **Sigurnosno kopiranje**.

1. U odjeljku vrsta **sigurnosne kopije** odaberite **Zapisnik transakcija**. Zadrži put datoteke **odredište** postavljena na jedan navedena na popisu i kliknite **u redu**. Kada završi postupak sigurnosnog kopiranja, ponovno kliknite **u redu** .

1. Nakon toga ponovno zapisnika kopija punog i transakcije na **ContosoSQL2**. Pokrenite datoteku RDP za **ContosoSQL2** i prijavite se kao **CORP\Install**. Sesiji udaljene radne površine za **ContosoSQL1** ostavite otvoren.

1. S izbornika **Start** pokretanje **SQL Server Management Studio**, a zatim kliknite **Poveži se** povezati s zadane instance sustava SQL Server.

1. U **Programu Explorer objekt**desnom tipkom miša kliknite **baze podataka** , a zatim kliknite **Vrati bazu podataka**.

1. U odjeljku **izvor** odaberite **uređaj**, a zatim kliknite **...** gumb.

1. U **Odaberite uređaji za sigurnosne kopije**, kliknite **Dodaj**.

1. Mjesto datoteke sigurnosne kopije, upišite \\ContosoSQL1\backup, pa kliknite Osvježi, a zatim odaberite MyDB1.bak, a zatim kliknite u redu i zatim kliknite u redu. Sada trebali biste vidjeti potpunu sigurnosnu kopiju i zapisnika sigurnosnu kopiju u sigurnosnoj kopiji postavlja da biste vratili okna.

1. Idite na stranici s mogućnostima, a zatim odaberite Vrati s NORECOVERY u stanju oporavak, a zatim kliknite u redu da biste vratili bazu podataka. Kada završi postupak vraćanja, kliknite u redu.

### <a name="create-the-availability-group"></a>Stvaranje grupe dostupnosti:

1. Vratite se u sesiji udaljene radne površine za **ContosoSQL1**. U programu **Explorer objekta** u SSMS, desnom tipkom miša kliknite **Uvijek na visoke dostupnosti** , a zatim kliknite **Novi dostupnost Čarobnjak za grupu**, kao što je prikazano u nastavku.

    ![Pokretanje čarobnjaka za novo grupe dostupnosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)

1. Na stranici **Uvod** kliknite **Dalje**. Na stranici **Navedite naziv grupe dostupnosti** **AG1** u upišite **naziv grupe dostupnosti**, a zatim kliknite **Dalje** ponovno.

    ![Čarobnjak za za novo AG Navedite naziv AG](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)

1. Na stranici **Odabir baze podataka** odaberite **MyDB1** , a zatim kliknite **Dalje**. Baza podataka zadovoljava preduvjete za grupe sustava dostupnost jer na svrhu primarni replike koje ste poduzeli barem jedan potpuno sigurnosno kopiranje.

    ![Čarobnjak za novo AG, odaberite baze podataka](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)

1. Na stranici **Određivanje replike** kliknite **Dodavanje replike**.

    ![Čarobnjak za za novo AG odredite replike](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)

1. Dijaloški okvir **za povezivanje s poslužiteljem** se skočni prozor. Upišite **ContosoSQL2** u okviru **naziv poslužitelja**, a zatim kliknite **Poveži**.

    ![Čarobnjak za za novo AG povezati s poslužiteljem](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)

1. Na stranici **Određivanje replike** sada trebali biste vidjeti **ContosoSQL2** na popisu **Dostupnih replike**. Konfiguriranje na replike kao što je prikazano u nastavku. Kada završite, kliknite **Dalje**.

    ![Čarobnjak za za novo AG odredite replike (dovršeno)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)

1. Na stranici **Odaberite početne sinkronizacije podataka** odaberite **samo uključiti** , a zatim kliknite **Dalje**. Ste već obavili sinkronizacije podataka ručno kada snimljene sigurnosne kopije punog i transakcije **ContosoSQL1** i ih vratili na **ContosoSQL2**. Umjesto toga možete odabrati ne želite izvršiti sigurnosno kopiranje i vraćanje postupci za bazu podataka, a odaberite **cijeli** da biste omogućili dostupnost grupe Čarobnjak za novo izvođenje sinkronizacije podataka umjesto vas. No to se ne preporučuje za vrlo velike baze podataka koje se nalaze u nekim velike tvrtke.

    ![Čarobnjak za novo AG, sinkronizacija odaberite početni podataka](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)

1. Na stranici **Provjera valjanosti** , kliknite **Dalje**. Ova stranica bi trebala izgledati ovako u nastavku. Postoji upozorenje za konfiguraciju ga slušatelj jer nije konfiguriran u ga slušatelj grupe dostupnosti. Ovo će se upozorenje možete zanemariti jer ovog praktičnog vodiča konfiguriranje na ga slušatelj. Da biste konfigurirali ga slušatelj po dovršetku ovog praktičnog vodiča, potražite u članku [Konfiguriranje programa ILB ga slušatelj za uvijek na dostupnost grupe u Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

    ![AG čarobnjaka za novo provjere valjanosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)

1. Na stranici **Sažetak** kliknite **Završi**, a zatim pričekajte da se čarobnjak konfigurira novu grupu dostupnost. Na stranici **tijek** kliknete **više detalja** da biste pogledali detaljne napredak. Nakon što završite čarobnjaka, provjeri stranice s **rezultatima** provjerite da grupe dostupnosti uspješno stvorili, kao što je prikazano u nastavku, a zatim kliknite **Zatvori** da biste zatvorili čarobnjak.

    ![Čarobnjak za za novo AG rezultata](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)

1. U **Programu Explorer objekt**proširite **Uvijek na visoke dostupnosti**, a zatim proširite **Grupe dostupnosti**. Prikazat će se sada novu grupu dostupnost u ovom spremniku. Desnom tipkom miša kliknite **AG1 (primarni)** i kliknite **Pokaži nadzorne ploče**.

    ![Prikaži AG nadzorne ploče](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)

1. **Uvijek na nadzornoj ploči** trebao bi izgledati slično onome prikazano u nastavku. Možete vidjeti na replike, način prebacivanje svaki replike i stanje sinkronizacije.

    ![AG nadzorne ploče](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)

1. Vratite **Upravitelj poslužitelja**, odaberite **Alati**i pokrenite **Upravitelj klaster prebacivanje**.

1. Proširite **Cluster1.corp.contoso.com**, a zatim **Services i aplikacija**. Odaberite **uloge** i zabilježite uloge grupa za dostupnost **AG1** je stvorena. Imajte na umu da AG1 imati sve IP adresu po baze podataka koja klijenti moguće je povezati s grupe dostupnosti jer, konfigurirati na ga slušatelj. Možete se povezati izravno u primarni čvor za čitanje i pisanje operacije i sekundarne čvor za upite koji su samo za čitanje.

    ![AG u upravitelju za prebacivanje klaster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

>[AZURE.WARNING] Pokušajte uvoza putem grupe dostupnosti iz prebacivanje klaster upravitelja. Sve operacije prebacivanje treba izvršiti iz unutar **Uvijek na nadzornoj ploči** SSMS. Dodatne informacije potražite u članku [ograničenja na pomoću u WSFC prebacivanje klaster Manager s grupama dostupnost](https://msdn.microsoft.com/library/ff929171.aspx).

## <a name="next-steps"></a>Daljnji koraci
Koje su sada uspješno implementirana SQL Server uvijek na stvaranjem granu dostupnost u Azure. Da biste konfigurirali ga slušatelj za ovu grupu dostupnost, potražite u članku [Konfiguriranje programa ILB ga slušatelj za uvijek na dostupnost grupe u Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Druge informacije o korištenju sustava SQL Server u Azure potražite u članku [SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md).
