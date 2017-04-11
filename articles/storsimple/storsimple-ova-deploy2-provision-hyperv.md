<properties
   pageTitle="Implementacija StorSimple virtualne polja - dodjele resursa u Hyper-V"
   description="Pomoću ovog praktičnog vodiča drugi StorSimple virtualne polja implementacije tiče dodjeljivanje virtualnog uređaja u Hyper-V."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/11/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-hyper-v"></a>Implementacija StorSimple virtualne polja – Dodjela virtualne polja u Hyper-V

![](./media/storsimple-ova-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Pregled

Ovaj vodič za dodjelu resursa odnosi na Microsoft Azure StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualne uređaja ili StorSimple virtualne uređajima) izvodi ožujku 2016 Općenito dostupan (GA) izdanje. Pomoću ovog praktičnog vodiča se opisuje Dodjela StorSimple virtualne polja na glavno računalo sustava Hyper-V sustavom Windows Server 2012 R2, Windows Server 2012 ili Windows Server 2008 R2. Ovaj se članak odnosi na implementaciju StorSimple virtualne polja u Azure klasični portal, kao i Microsoft Azure državne Cloud.

Će potrebne administratorske ovlasti za dodjelu resursa i konfiguriranje virtualnog uređaja. Postavljanje dodjele resursa i početne može potrajati oko 10 minuta da biste dovršili.


## <a name="provisioning-prerequisites"></a>Preduvjeti za dodjelu resursa

Ovdje ćete pronaći preduvjeti Dodjela virtualnog uređaja u sustavu glavno računalo Hyper-V sustavom Windows Server 2012 R2, Windows Server 2012 ili Windows Server 2008 R2.

### <a name="for-the-storsimple-manager-service"></a>Upravitelj StorSimple servisa

Prije početka, provjerite:

-   Dovršite sve korake u [odjeljku Priprema portal za StorSimple virtualne polja](storsimple-ova-deploy1-portal-prep.md).

-   Slika virtualnog uređaja za Hyper-V ste preuzeli s portala za Azure. Dodatne informacije potražite u članku [korak 3: preuzimanje slika virtualnog uređaja](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

    > [AZURE.IMPORTANT] Softver koji se izvodi na polja virtualne StorSimple može se koristiti samo u kombinaciji sa servisom Storsimple Manager.

### <a name="for-the-storsimple-virtual-device"></a>Za StorSimple virtualnog uređaja

Prije implementacije virtualnog uređaja, učinite sljedeće:

-   Imate pristup glavnog računala sustava izvodi Hyper-V na Windows Server 2008 R2 ili novijem koje se mogu koristiti u Dodjela resursa za uređaj.

-   Sustav glavnog računala je moći odvojiti Dodjela virtualnog uređaja u sljedećim resursima:

    -   Najmanje 4 jezgri.

    -   Najmanje 8 GB RAM-a.

    -   Sučelje za jedne mreže.

    -   500 GB virtualne disk za podatke sustava.

### <a name="for-the-network-in-the-datacenter"></a>Za mrežu u s podatkovnim centrom

Prije početka, pregledajte umrežavanje preduvjete za implementaciju StorSimple virtualnog uređaja i komponenta konfiguriranje mrežom podatkovnog centra. Dodatne informacije potražite u članku [preduvjeti za umrežavanje StorSimple virtualne polja](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Korak po korak dodjele resursa

Dodjela resursa i povezati virtualnog uređaja, morat ćete poduzeti sljedeće korake:

1.  Glavno računalo sustava provjerite ima li dovoljno resursa za zadovoljava preduvjete minimalne virtualnog uređaja.

2.  Dodjela virtualnog uređaja u vašem hypervisor.

3.  Pokrenite virtualnog uređaja, a zatim se s IP adresom.

Svaki od ovih koraka je objašnjeno u sljedećim odjeljcima.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements"></a>Korak 1: Provjerite ispunjava li glavno računalo sistemske preduvjete minimalne virtualnog uređaja

Da biste stvorili virtualnog uređaja, morate:

-   Uloga Hyper-V instalirana na Windows Server 2012 R2, Windows Server 2012 ili Windows Server 2008 R2 SP1.

-   Upravitelj Hyper-V Microsoft na klijentskom računalu Microsoft Windows povezani s glavnim računalom.

Koje morate provjerite je li podlozi hardver (sustav glavnog računala) na kojem stvarate virtualnog uređaja moći odvojiti u sljedećim resursima za virtualnog uređaja:

- Najmanje 4 jezgri.
- Najmanje 8 GB RAM-a.
- Sučelje za jedne mreže.
- 500 GB virtualne disk za podatke sustava.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Korak 2: Dodjela virtualnog uređaja u hypervisor

Izvršite sljedeće korake Dodjela uređaj u vašem hypervisor.

#### <a name="to-provision-a-virtual-device"></a>Dodjela virtualnog uređaja

1.  Na glavnom sustava Windows Server kopirajte sliku virtualnog uređaja na lokalni pogon. Ovo je slika (VHD ili VHDX) koji ste preuzeli putem portala za Azure. Zabilježite mjesto koju ste kopirali sliku kao što je namjeravate koristiti ovo u nastavku postupak.

2.  Otvorite **Upravitelj poslužitelja**. U gornjem desnom kutu kliknite **Alati** , a zatim odaberite **Upravitelj Hyper-V**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image1.png)

    Ako koristite sustav Windows Server 2008 R2, otvorite upravitelj Hyper-V. U upravitelju poslužitelja kliknite **uloge > Hyper-V > Upravitelj Hyper-V**.

1.  U **Upravitelju Hyper-V**, u oknu opseg desnom tipkom miša kliknite čvor vašeg sustava da biste otvorili na kontekstnom izborniku, a zatim kliknite **Nova** > **virtualnog računala**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image2.png)

1.  Na stranici čarobnjaka za novo virtualnog računala **prije nego što počnete** kliknite **Dalje**.

1.  Na stranici **Navedite naziv i mjesto** navedite **naziv** za virtualnog uređaja. Kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image4.png)

1.  Na stranici **Određivanje generacije** odaberite vrstu slike uređaja, a zatim kliknite **Dalje**. Ova stranica ne pojavljuje ako koristite Windows Server 2008 R2.

    * Ako ste preuzeli .vhdx slika za Windows Server 2012 ili noviji, odaberite **generacije 2** .
    * Ako ste preuzeli .vhd slika za Windows Server 2008 R2 ili noviji, odaberite **generacije 1** .

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image5.png)

1.  Na stranici **Dodjela memorije** odredite **pokretanje memorije** od najmanje **8192 MB**, ne omogućite dinamične memorije, a zatim kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image6.png)

1.  Na stranici **Konfiguracija mreža** odredite virtualna parametar koji je povezan s Internetom, a zatim kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image7.png)

1.  Na stranici **povezivanje virtualne na tvrdom disku** , odaberite **koristi postojeće virtualne tvrdog diska**, navedite mjesto na kojem se slika virtualnog uređaja (.vhdx ili .vhd) i zatim kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image8m.png)

1.  Pregledajte **Sažetak** , a zatim kliknite **Završi** da biste stvorili virtualnog računala. Ali nemojte prebaciti još – i dalje morate dodati neke jezgri procesora i drugi. 

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image9.png)

1.  Da bi odgovarao minimalni preduvjeti, trebat će vam 4 jezgri. Da biste dodali virtualne procesora sustavu glavno računalo u odabran u prozoru **Upravitelja Hyper-V** , u desnom oknu u odjeljku popis **virtualnim strojevima**pronađite virtualnog računala koju ste upravo stvorili. Odaberite i desnom tipkom miša kliknite naziv računala, a zatim **Postavke**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image10.png)

1.  Na stranici **Postavke** u lijevom oknu kliknite **procesor**. U desnom oknu postavite **broja procesora koji virtualne** 4 (ili više). Kliknite **Primijeni**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image11.png)

1.  Da bi odgovarao minimalni preduvjeti i morate dodati na disku virtualne podataka 500 GB. Na stranici **Postavke** :

    1.  U lijevom oknu odaberite **SCSI kontroler**.
    2.  U desnom oknu odaberite **Tvrdi disk,** a zatim kliknite **Dodaj**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image12.png)

1.  Na stranici **tvrdom disku** , odaberite mogućnost **virtualne tvrdog diska** , a zatim kliknite **Novo**. Započet će **Virtualne na tvrdom disku Čarobnjak za novo**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image13.png)

1.  Na stranici čarobnjaka novi virtualni tvrdi Disk **prije nego što počnete** kliknite **Dalje**.

1.  Na **stranici odaberite oblik na disku**, prihvatite zadana mogućnost **VHDX** oblikovanja. Kliknite **Dalje**. Ako radite u sustavu Windows Server 2012 R2 ili Windows Server 2008 R2 neće se prikazati ovaj zaslon.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image15.png)

1.  Na **stranici Odabir vrste Disk**, postaviti vrstu virtualne tvrdi disk kao **dinamično proširivanje** (preporučeno). Ako odaberete **Fixed veličina** diska, i funkcionirat će, no možda ćete morati pričekati na dulje vrijeme. Preporučujemo da koristite mogućnost **Differencing** . Kliknite **Dalje**. Imajte na umu da **dinamično proširivanje** zadan u sustavu Windows Server 2012 R2 i Windows Server 2012. U sustavu Windows Server 2008 R2 zadani je **Fixed veličina**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image16.png)

1.  Na stranici **Navedite naziv i mjesto** omogućavaju na **naziv** kao i **mjesto** (možete pregledavati na jednu) podatkovni disk. Kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image17.png)

1.  Na stranici **Konfiguracija na disku** odaberite mogućnost **Stvori novi prazan virtualne tvrdi disk** , a zatim odredite veličinu kao **500 GB** (ili više njih). Dok je 500 GB minimalni preduvjet, uvijek možete Dodjela veći disk. Imajte na umu da ne možete proširiti ili Smanji disk jednom dodjeli. Dodatne informacije o veličini diska za dodjelu resursa, pregledajte sekcije za promjenu veličine u dokumentu [najbolje prakse](storsimple-ova-best-practices.md#configuration-best-practices) . Kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image18.png)

1.  Na stranici **Sažetak** pregledajte pojedinosti diska virtualne podataka i ako su zadovoljena, kliknite **Završi** da biste stvorili disk. Čarobnjak će se zatvoriti i virtualne na tvrdom disku dodat će se na vaše računalo.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image19.png)

2.  Vratit ćete na stranici **Postavke** . Kliknite **u redu** da biste zatvorili stranicu **Postavke** i vratili u prozor upravitelja Hyper-V.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Korak 3: Pokretanje virtualnog uređaja i dobiti na IP

Izvršite sljedeće korake da biste pokrenuli virtualnog uređaja i s njim povezati.

#### <a name="to-start-the-virtual-device"></a>Da biste pokrenuli virtualnog uređaja

1.  Pokrenite virtualnog uređaja.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image21.png)

1.  Kada na uređaju instaliran, odaberite željeni uređaj, desnom tipkom miša kliknite i odaberite **Poveži**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image22.png)

1.  Možda ćete morati pričekati 5 10 minuta za uređaj bude spreman. Poruka o stanju prikazuje se na konzolu za označavanje napredak. Kada je uređaj spreman, idite na **Akcije**. Pritisnite `Ctrl + Alt + Delete` da biste se prijavili u virtualnog uređaja. Zadani korisnik je *StorSimpleAdmin* i lozinka zadani je *Password1*.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image23.png)

1.  Sigurnosnih vam razloga administratorsku lozinku uređaj istječe pri prvom zapisnika. Od vas će se zatražiti da biste promijenili lozinku.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image24.png)

    Unesite lozinku koja sadrži najmanje 8 znakova. Lozinka mora zadovoljavati najmanje 3 iz preduvjeti 4: velika slova, mala slova, numeričke i posebnih znakova. Ponovno unesite lozinku da biste je potvrdili. Bit ćete obaviješteni da je lozinka promijenila.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image25.png)

1.  Kada uspješno Promijeni lozinku, možda će početi virtualnog uređaja. Pričekajte uređaj da biste pokrenuli.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image26.png)

    Konzole za Windows PowerShell uređaja prikazat će se zajedno s traku napretka.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image27.png)

1.  6-8 koraci primjenjuju se samo prilikom pokretanja prema gore u okruženju koje nisu DHCP. Ako ste u okruženju DHCP, preskočite ove korake i prijeđite na 9. Ako pokrenuto gore uređaj koji nisu DHCP okruženja, vidjet ćete na sljedećem zaslonu.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image28m.png)

    Sada ćete konfigurirati s mrežom.

1.  Korištenje na `Get-HcsIpAddress` naredba popis sučelje mreže omogućena na virtualnog uređaja. Ako vaš uređaj ima jedne mreže sučelja omogućena, zadani naziv dodijeljen ovo sučelje je `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image29m.png)

1.  Korištenje na `Set-HcsIpAddress` cmdlet da biste konfigurirali s mrežom. Primjer prikazano u nastavku:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image30.png)

1.  Nakon dovršetka početnog postavljanja i uređaja nije pokrenuto prema gore, vidjet ćete tekst za natpis uređaja. Zabilježite IP adrese i URL prikazan u okvir tekst za natpis za upravljanje uređaj. Povežite se s webom UI virtualnog uređaja i dovršite postavljanje lokalne i registracija će koristiti ovu IP adresu.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image31m.png)



1. (Neobavezno) Izvršite taj korak samo ako implementirate uređaj državne oblaka. Sada će omogućiti način Sjedinjenih Američkih Država Savezna informacije Processing Standard (FIPS) na uređaju. Standardna FIPS 140 definira šifriranja algoritama za korištenje odobri NAM Savezna državne računalne sustava za zaštitu osjetljivih podataka.
    1. Da biste omogućili način FIPS, pokrenite sljedeći cmdlet:

        `Enter-HcsFIPSMode`

    2. Nakon što ste omogućili način FIPS tako da se šifriranja provjere valjanosti snagu ponovnog pokretanja uređaja.

        > [AZURE.NOTE] Možete omogućiti ili onemogućiti Način FIPS na uređaju. Naizmjenični uređaj između FIPS i koje nisu FIPS način nije podržana.

Ako vaš uređaj ne zadovoljava preduvjete minimalne konfiguracije, vidjet ćete pogrešku u okvir tekst za natpis (prikazano u nastavku). Morat ćete promijeniti Konfiguracija uređaja tako da ga nema dovoljno resursa da bi odgovarao minimalni preduvjeti. Možete ponovno pokrenuti i povezati s uređajem. Pogledajte preduvjete minimalne konfiguraciju u [Korak 1: Provjerite ispunjava li glavno računalo sistemske preduvjete minimalne virtualnog uređaja](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-hyperv/image32.png)

Ako licem druge pogreške tijekom početne konfiguracije putem lokalnog weba korisničkog Sučelja, pročitajte sljedeće tijekove rada [Upravljanje virtualne polja vašem StorSimple](storsimple-ova-web-ui-admin.md)korištenja lokalnog web korisničkog Sučelja.

-   Pokrenite Dijagnostički testovi za [Otklanjanje poteškoća s postavljanjem korisničkog Sučelja web](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).

-   [Generiraj zapisnika paketa i prikaz datoteka zapisnika](storsimple-ova-web-ui-admin.md#generate-a-log-package).

![Ikona videoprikaz:](./media/storsimple-ova-deploy2-provision-hyperv/video_icon.png)  **videozapis dostupna**

Pogledajte videozapis da biste vidjeli kako Dodjela resursa StorSimple virtualne polja u Hyper-V.

> [AZURE.VIDEO create-a-storsimple-virtual-array]

## <a name="next-steps"></a>Daljnji koraci

-   [Postavite svoje StorSimple virtualne polja kao poslužitelj datoteka](storsimple-ova-deploy3-fs-setup.md)

-   [Postavite svoje StorSimple virtualne polja kao iSCSSI poslužiteljem](storsimple-ova-deploy3-iscsi-setup.md)
