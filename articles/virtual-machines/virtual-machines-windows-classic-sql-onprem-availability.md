<properties
    pageTitle="Proširivanje lokalnog uvijek na dostupnost grupe za Azure | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča koristi resursa koje su stvorene pomoću model klasični implementacije, a u članku se opisuje kako koristiti Čarobnjak za dodavanje replike u SQL Server Management Studio (SSMS) da biste dodali programa uvijek na grupe dostupnosti replike u Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>Proširivanje lokalnog uvijek na dostupnost grupe za Azure

Uvijek na dostupnost grupe omogućuju visoke dostupnosti za grupe baze podataka tako da dodate sekundarnog replike. Dopusti te replike ne uspijeva putem baze podataka u slučaju. Osim toga oni mogu se offload čitanje radnih opterećenja ili sigurnosne kopije zadataka.

Možete proširiti grupe dostupnosti lokalnog Microsoft Azure VMs Azure s SQL poslužiteljem za dodjelu resursa i zatim ih dodate kao replike vaše lokalne dostupnost grupe.

Pomoću ovog praktičnog vodiča podrazumijeva sljedeće:

- Aktivnu pretplatu Azure. Možete se [prijaviti se za besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

- Postojeće uvijek na dostupnost grupe lokalnog. Dodatne informacije o dostupnosti grupe potražite u članku [Uvijek na dostupnost grupe](https://msdn.microsoft.com/library/hh510230.aspx).

- Povezivanje između lokalne mreže i Azure virtualne mreže. Dodatne informacije o stvaranju virtualne mreže potražite u članku [Konfiguriranje VPN-a web-mjesto na portalu za Azure klasični](../vpn-gateway/vpn-gateway-site-to-site-create.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="add-azure-replica-wizard"></a>Azure replike Čarobnjak za dodavanje

U ovom se odjeljku objašnjava koristiti **Čarobnjak za dodavanje replike Azure** da biste proširili rješenje uvijek na dostupnost grupe da biste uključili Azure replike.

1. Iz programa SQL Server Management Studio proširite **Uvijek na visoke dostupnosti** > **Grupe dostupnosti** > **[naziv grupe dostupnosti]**.

1. Desnom tipkom miša kliknite **Replike dostupnosti**, a zatim kliknite **Dodaj replike**.

1. Prema zadanim postavkama, prikazuje se **Dodavanje replike dostupnost Čarobnjak za grupu** . Kliknite **Dalje**.  Ako ste odabrali **Pokaži ovu stranicu** mogućnost pri dnu stranice tijekom prethodne pokretanje čarobnjaka, prikazat će se ne ovaj zaslon.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)

1. Će se morati povezati postojeće sekundarnog replike. Možete kliknuti na **povezivanje...** pokraj svake replike, ili kliknuti **Povezivanje svih...** pri dnu zaslona. Nakon provjere autentičnosti, kliknite **Dalje** da biste prešli na sljedećem zaslonu.

1. Na stranici **Određivanje replike** više kartice su navedene na vrhu: **replike** **krajnje točke**, **Sigurnosno kopiranje preference**te **ga Slušatelj**. Na kartici **replike** kliknite **Dodavanje Azure replike...** Da biste pokrenuli čarobnjak za dodavanje Azure replike.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)

1. Ako ste instalirali prije, odaberite postojeći certifikat Azure upravljanja s lokalnom spremištu certifikata za Windows. Odaberite ili unesite id Azure pretplatu ako ste koristili prije. Kliknite Preuzmi da biste preuzeli i instalirali potvrdu Azure upravljanja i preuzeti popis pretplata pomoću račun za Azure.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)

1. Svako polje na stranici s vrijednostima koje će se koristiti za stvaranje na Azure virtualnog računala (VM) koji će se hostira replike će popuniti.

  	|Postavka|Opis|
|---|---|
|**Slika**|Odaberite željenu kombinaciju OS i SQL Server|
|**Veličina VM**|Odaberite veličinu VM koji najbolje odgovara vašim potrebama tvrtke|
|**Naziv VM**|Navedite jedinstveni naziv za novu VM. Naziv mora sadržavati između 3 i 15 znakova, možete sadržavati samo slova, brojeve i crtice, morate počinju slovima i završavati slovo ili broj.|
|**Korisničko ime VM**|Navedite korisničko ime koje će postati administratorski račun na na VM|
|**VM administratorsku lozinku**|Odredite lozinku za novi račun|
|**Potvrdite lozinku**|Potvrdite lozinku novog računa|
|**Virtualne mreže**|Navedite Azure virtualne mreže novu VM trebali biste koristiti. Dodatne informacije o virtualne mreže potražite u članku [Pregled virtualne mreže](../virtual-network/virtual-networks-overview.md).|
|**Podmreže virtualne mreže**|Odredite virtualna mreža podmreži koje biste trebali koristiti novi VM|
|**Domene**|Provjerite je li vrijednost unaprijed popunjena za domenu ispravna|
|**Domena korisničko ime**|Odredite račun koji se nalazi u lokalne grupe administratora na čvorove lokalne klaster|
|**Lozinke**|Navedite naziv domene korisnika|

1. Kliknite **u redu** da biste provjerili valjanost postavki implementacije.

1. Pravne uvjete prikazuju se sljedeće. Pročitajte i, ako prihvaćate ove uvjete, kliknite **u redu** .

1. Ponovno se prikazuje stranica **Odredite replike** . Provjerite postavke za nove Azure replike na karticama **replike**, **krajnje točke**i **Postavki za sigurnosno kopiranje** . Promijenite postavke u skladu s potrebama tvrtke.  Dodatne informacije o parametre koje se nalaze na karticama potražite u članku [Određivanje replike stranice (dostupnost grupe čarobnjak/Dodaj replike Čarobnjak za novo)](https://msdn.microsoft.com/library/hh213088.aspx). Imajte na umu slušače nije moguće stvoriti pomoću kartice ga Slušatelj za dostupnost grupe koje sadrže Azure replike. Osim toga, ako je ga slušatelj već stvorili prije pokretanja čarobnjaka, primit ćete poruku da nije podržan u Azure. Ne možemo izgledat će pri stvaranju slušače u odjeljku **Stvaranje programa ga Slušatelj grupe dostupnosti** .

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)

1. Kliknite **Dalje**.

1. Odaberite način sinkronizacije podataka koji želite koristiti na stranici **Odaberite početne sinkronizacije podataka** , a zatim kliknite **Dalje**. Za većinu scenarije, odaberite **Potpune sinkronizacije podataka**. Dodatne informacije o metode sinkronizacije podataka potražite u članku [Odaberite početni sinkronizacije stranicu podataka (uvijek na dostupnost grupe čarobnjaci)](https://msdn.microsoft.com/library/hh231021.aspx).

1. Pregledajte rezultate na stranici **Provjera valjanosti** . Ispravljanje preostala probleme i ponovno pokrenite provjeru prema potrebi. Kliknite **Dalje**.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)

1. Pregledajte postavke na stranici **Sažetak** , a zatim kliknite **Završi**.

1. Započinje postupak za dodjelu resursa. Kada se čarobnjak dovrši uspješno, kliknite **Zatvori** da biste izađete iz čarobnjaka.

>[AZURE.NOTE] Čarobnjak za dodavanje Azure replike stvara datoteku zapisnika u Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Datoteka zapisnika se može koristiti za otklanjanje poteškoća s implementacijama nije uspjelo Azure replike. Ako ne uspije čarobnjaka za izvođenje akcija, sve prethodne operacije su vraćen, uključujući brisanje dodijeljenu VM.

## <a name="create-an-availability-group-listener"></a>Stvaranje programa ga slušatelj grupe dostupnosti

Nakon stvaranja grupe dostupnosti potrebno stvoriti ga slušatelj za klijente za povezivanje s replike. Slušače izravno dolazne veze primarni ili sekundarnog replike samo za čitanje. Dodatne informacije o slušače potražite u članku [Konfiguriranje programa ILB ga za slušatelj uvijek na dostupnost grupe u Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

## <a name="next-steps"></a>Daljnji koraci

Osim pomoću **Čarobnjaka za dodavanje replike Azure** da biste proširili uvijek na dostupnost grupu za Azure, možda i premjestite neke radnih opterećenja sustava SQL Server u potpunosti Azure. Početak rada potražite u članku [Dodjeljivanje SQL Server virtualnog računala na Azure](virtual-machines-windows-portal-sql-server-provision.md).

Druge teme vezane uz izvodi SQL Server u Azure VMs, potražite u članku [SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md).
