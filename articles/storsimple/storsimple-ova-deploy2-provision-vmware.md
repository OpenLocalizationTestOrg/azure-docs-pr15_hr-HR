<properties
   pageTitle="Implementacija StorSimple virtualne polja - dodjele resursa u VMware"
   description="Pomoću ovog praktičnog vodiča drugi u nizu za implementaciju StorSimple virtualne polja tiče dodjeljivanje virtualnog uređaja u VMware."
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
   ms.date="04/12/2016"
   ms.author="alkohli"/>


# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-vmware"></a>Implementacija StorSimple virtualne polja – Dodjela virtualne polja u VMware

![](./media/storsimple-ova-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Pregled 
Ovaj vodič za dodjelu resursa primjenjuje se na StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualne uređaje ili StorSimple virtualne uređaje) izvodi ožujku 2016 Općenito dostupan (GA) izdanje. Pomoću ovog praktičnog vodiča opisuje Dodjela resursa i povezati s StorSimple virtualne polja na glavno računalo u kojem se izvodi VMware ESXi 5,5 i iznad. Ovaj se članak odnosi na implementaciju StorSimple virtualne polja u Azure klasični portal, kao i Microsoft Azure državne Cloud.

Će potrebne administratorske ovlasti za dodjelu resursa i povezati se virtualnog uređaja. Postavljanje dodjele resursa i početne može potrajati oko 10 minuta da biste dovršili.


## <a name="provisioning-prerequisites"></a>Preduvjeti za dodjelu resursa

Ovdje ćete pronaći preduvjeti Dodjela virtualnog uređaja na glavno računalo u kojem se izvodi VMware ESXi 5,5 i iznad.

### <a name="for-the-storsimple-manager-service"></a>Upravitelj StorSimple servisa

Prije početka, provjerite:

-   Dovršite sve korake u [odjeljku Priprema portal za StorSimple virtualne polja](storsimple-ova-deploy1-portal-prep.md).

-   Slika virtualnog uređaja za VMware ste preuzeli s portala za Azure. Dodatne informacije potražite u članku [korak 3: preuzimanje slika virtualnog uređaja](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

### <a name="for-the-storsimple-virtual-device"></a>Za StorSimple virtualnog uređaja 

Prije implementacije virtualnog uređaja, učinite sljedeće:

-   Kojima imate pristup glavnog računala sustava izvodi Hyper-V (2008 R2 ili noviji) koje mogu koristiti u Dodjela resursa za uređaj.

-   Sustav glavnog računala je moći odvojiti Dodjela virtualnog uređaja u sljedećim resursima:

    -   Najmanje 4 jezgri.

    -   Najmanje 8 GB RAM-a.

    -   Sučelje za jedne mreže.

    -   500 GB virtualne disk za podatke sustava.

### <a name="for-the-network-in-datacenter"></a>Za mrežu u podatkovnim centrom 

Prije početka, provjerite:

-   Možete pregledati umrežavanje preduvjeti za implementaciju StorSimple virtualnog uređaja i konfigurirali mreže podatkovnog centra u skladu sa zahtjevima. Dodatne informacije potražite u članku [sistemski preduvjeti za StorSimple virtualne polja](storsimple-ova-system-requirements.md).

## <a name="step-by-step-provisioning"></a>Korak po korak dodjele resursa 

Dodjela resursa i povezati virtualnog uređaja, morat ćete poduzeti sljedeće korake:

1.  Glavno računalo sustava provjerite ima li dovoljno resursa za zadovoljava preduvjete minimalne virtualnog uređaja.

2.  Dodjela virtualnog uređaja u vašem hypervisor.

3.  Pokrenite virtualnog uređaja, a zatim se s IP adresom.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Korak 1: Provjerite sustava glavno računalo zadovoljava preduvjete minimalne virtualnog uređaja

Da biste stvorili virtualnog uređaja, morate:

-   Pristup glavnog računala sustava radi VMware ESXi poslužitelja 5,5 i iznad.

-   VMware vSphere klijent u vašem sustavu za upravljanje ESXi glavnog računala.

    -   Najmanje 4 jezgri.

    -   Najmanje 8 GB RAM-a.

    -   Jedan mrežno sučelje povezani s mrežom instaliranog usmjeravanje prometa na Internetu. Minimalna propusnosti internetske mora biti 5 MB/s da biste omogućili radi optimalnog rad uređaja.

    -   500 GB virtualne disk za podatke.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Korak 2: Dodjela virtualnog uređaja u hypervisor

Izvršite sljedeće korake Dodjela virtualnog uređaja u vašem hypervisor.

1.  Kopiranje slike virtualnog uređaja u sustavu. Ovo je slike koju ste preuzeli putem portala za Azure klasični. 
    1.  Provjerite je li to najnovije slikovna datoteka koju ste preuzeli. Ako ste prethodno preuzeli slike, preuzmite ga da biste bili sigurni da imate najnovija slike. Najnovije slika sadrži dvije datoteke (umjesto jednog).
    2.  Zabilježite mjesto koju ste kopirali sliku kao što je namjeravate koristiti ovo u nastavku postupak.

2.  Prijavite se u ESXi poslužitelja pomoću klijentskog programa vSphere. Morat ćete imati administratorske ovlasti da biste stvorili virtualnog računala.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image1.png)

1.  U klijentu vSphere, u odjeljku zaliha u lijevom oknu odaberite poslužitelj ESXi.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image2.png)

1.  Će najprije prenesete na VMDK ESXi poslužitelj. Idite na karticu **Konfiguracija** u desnom oknu. U odjeljku **hardver**odaberite **prostora za pohranu**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image3.png)

1.  U desnom oknu u odjeljku **Datastores**, odaberite mjesto na koje želite prenijeti na VMDK spremištu podataka. Spremištu podataka mora imati dovoljno slobodnog prostora za diskova OS i podatke.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image4.png)

1.  Desnom tipkom miša kliknite i odaberite **Pregledaj Datastore**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image5.png)

1.  Pojavit će se u prozoru **Preglednika Datastore** .

    ![](./media/storsimple-ova-deploy2-provision-vmware/image6.png)

1.  Na alatnoj traci kliknite ![](./media/storsimple-ova-deploy2-provision-vmware/image7.png) ikonu da biste stvorili novu mapu. Navedite naziv mape i zabilježite ga. Ovaj naziv mape će koristiti kasnije prilikom stvaranja virtualnog računala (preporučuje se preporučenim načinom rada). Kliknite **u redu**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image8.png)

1.  U lijevom oknu web- **Pregledniku Datastore**pojavit će se nova mapa.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image9.png)

1.  Kliknite ikonu za prijenos ![](./media/storsimple-ova-deploy2-provision-vmware/image10.png) , a zatim odaberite **Prenesi datoteku**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image11.png)

1.  Trebali biste sada Pregledaj i pokažite na datoteke VMDK koji ste preuzeli. Pojavit će se dvije datoteke. Odaberite datoteku koju želite prenijeti.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image12m.png)

1.  Kliknite **Otvori**. Prijenos datoteke VMDK navedeni spremištu podataka se odmah pokrenuti. Može potrajati nekoliko minuta datoteke za prijenos.


1.  Nakon dovršetka prijenos, vidjet ćete datoteke u spremištu podataka u mapi koju ste stvorili. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image14.png)

    Sada ćete prijenos drugi VMDK datoteke u istom spremištu podataka.

1.  Vratite se u prozor vSphere klijenta. S poslužiteljem ESXi odabrana, desnom tipkom miša, a zatim odaberite **Novi virtualnog računala**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image15.png)

1.  Pojavit će se prozor za **Stvaranje novog virtualnog računala** . Na stranici **Konfiguracija** , odaberite mogućnost **Prilagođeno** . Kliknite **Dalje**.
    ![](./media/storsimple-ova-deploy2-provision-vmware/image16.png)

2.  Na stranici **naziva i mjesta** Navedite naziv virtualnog računala. Taj naziv mora odgovarati naziv mape (preporučena preporučenim načinom rada) koju ste naveli u prethodnom koraku 8.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image17.png)

1.  Na stranici **za pohranu** , odaberite datastore koji želite koristiti Dodjela vaše VM.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image18.png)

1.  Na stranici **Virtualnog računala verzija** odaberite **virtualnog računala verzija: 8**. Imajte na umu da verzije 8 do 11 podržano.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image19.png)

1.  Na stranici **goste operacijski sustav** , odaberite **goste operacijski sustav** kao **Windows**. Za **verziju**s padajućeg popisa odaberite **Microsoft Windows Server 2012 (64-bitni)**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image20.png)

1.  Na stranici **CPU-ovi** prilagodite **broj virtualne sockets** i **broj jezgri po virtualne socket** tako da je **ukupan broj jezgri** 4 (ili više). Kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image21.png)

1.  Na stranici **memorije** navedite 8 GB (ili više) RAM-a. Kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image22.png)

1.  Na stranici **mreže** navedite broj sučelje mreže. Minimalni preduvjet je sučelje jedne mreže.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image23.png)

1.  Na stranici **SCSI kontroler** prihvatite zadane **LSI logike SAS kontroler**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image24.png)

1.  Na stranici **Odaberite Disk** , odaberite **Koristi postojeći virtualni disk**. Kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image25.png)

1.  Na stranici **Odaberite postojeći Disk** , u odjeljku **Put datoteke na disku**, kliknite **Pregledaj**. Otvorit će se dijaloški okvir **Pregledavanje Datastores** . Pomaknite se do mjesta na kojem ste prenijeli na VMDK. Sada ćete vidjeti samo jedne datoteke u spremištu podataka kao što su bile spojene dvije datoteke koje ste prethodno prenijeli. Odaberite datoteku, a zatim kliknite **u redu**. Kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image26.png)

1.  Na stranici **Napredne mogućnosti** prihvatite zadane, a zatim kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image27.png)

1.  Na stranici **spreman za dovršetak** pregledajte sve postavke pridružene novi virtualnog računala. Potvrdite okvir **Uređivanje postavki virtualnog računala prije dovršetka**. Kliknite **Nastavi**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image28.png)

1.  Na stranici **Virtualnim strojevima svojstva** na kartici **hardver** pronađite hardver uređaja. Odaberite **novi tvrdi Disk**. Kliknite **Dodaj**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image29.png)

1.  Tako ćete prikazati **Dodaj hardver** prozora. Na stranici **Vrste uređaja** u odjeljku **Odabir vrste uređaja koje želite dodati**, odaberite **tvrdi Disk** , a zatim kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image30.png)

1.  Na stranici **Odaberite Disk** , odaberite **Stvori novi virtualni disk**. Kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image31.png)

1.  Na stranici **Stvaranje na disku** promijeniti **Veličinu diska** 500 GB (ili više). U odjeljku **Dodjela resursa na disku**, odaberite **Tanki dodjele resursa**. Kliknite **Dalje**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image32.png)

1.  Na stranici **Napredne mogućnosti** prihvatite zadani.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image33.png)

1.  Na stranici **spreman za dovršetak** pregledajte mogućnosti diska. Kliknite **Završi**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image34.png)

1.  Sada će se vratiti na stranici Svojstva virtualnog računala. Dodaje novi tvrdi disk virtualnog računala. Kliknite **Završi**.
  
    ![](./media/storsimple-ova-deploy2-provision-vmware/image35.png)

2.  Uz računalo virtualne odabran u desnom oknu pronađite karticu **Sažetak** . Pregled postavki virtualnog računala.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image36.png)

Sada je dodjeli virtualnog računala. Sljedeći je korak da biste power na ovom računalu i dobili IP adresa.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Korak 3: Pokretanje virtualnog uređaja i dobiti na IP

Izvršite sljedeće korake da biste pokrenuli virtualnog uređaja i s njim povezati.

#### <a name="to-start-the-virtual-device"></a>Da biste pokrenuli virtualnog uređaja

1.  Pokrenite virtualnog uređaja. U vSphere Upravitelj konfiguracije, u lijevom oknu odaberite uređaj, a zatim desnom tipkom miša kliknite da bi se prikazala na kontekstnom izborniku. Odaberite **Power** , a zatim odaberite **uključeno**. Power treba na virtualnog računala. U oknu **Nedavne zadaci** dna vSphere klijenta možete prikazati stanje.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image37.png)

1.  Zadaci postavljanja će potrajati nekoliko minuta. Kada na uređaju instaliran, otvorite karticu **konzole** . Pošaljite Ctrl + Alt + Delete da biste se prijavili u uređaj. Možete pokažite kursorom na prozor konzole, a zatim pritisnite Ctrl + Alt + Insert. Zadani korisnik je *StorSimpleAdmin* i lozinka zadani je *Password1*.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image38.png)

1.  Sigurnosnih vam razloga administratorsku lozinku uređaj istječe pri prvom zapisnika. Od vas će se zatražiti da biste promijenili lozinku.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image39.png)

1.  Unesite lozinku koja sadrži najmanje 8 znakova. Lozinka mora sadržavati 3 od 4 te preduvjete: velika slova, mala slova, numeričke i posebnih znakova. Ponovno unesite lozinku da biste je potvrdili. Bit ćete obaviješteni da je lozinka promijenila.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image40.png)

1.  Kada uspješno Promijeni lozinku, ponovno možda virtualnog uređaja. Pričekajte ponovno pokrenite računalo da biste dovršili. Uz traku napretka može se prikazati konzole za Windows PowerShell uređaja.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image41.png)

1.  6-8 koraci primjenjuju se samo prilikom pokretanja prema gore u okruženju koje nisu DHCP. Ako ste u okruženju DHCP, preskočite ove korake i prijeđite na 9. Ako pokrenuto gore uređaj koji nisu DHCP okruženja, vidjet ćete na sljedećem zaslonu. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image42m.png)

    Sada ćete konfigurirati s mrežom.

1.  Korištenje na `Get-HcsIpAddress` naredba popis sučelje mreže omogućena na virtualnog uređaja. Ako vaš uređaj ima jedne mreže sučelja omogućena, zadani naziv dodijeljene ovom sučelju je `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image43m.png)

1.  Korištenje na `Set-HcsIpAddress` cmdlet da biste konfigurirali s mrežom. Primjer prikazano u nastavku:


    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-vmware/image44.png)

1.  Nakon dovršetka početnog postavljanja i uređaja nije pokrenuto prema gore, vidjet ćete tekst za natpis uređaja. Zabilježite IP adrese i URL prikazan u okvir tekst za natpis za upravljanje uređaj. Povežite se s webom UI virtualnog uređaja i dovršite postavljanje lokalne i registracija će koristiti ovu IP adresu.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image45.png)


1. (Neobavezno) Izvršite taj korak samo ako implementirate uređaju u oblak državne. Sada će omogućiti način Sjedinjenih Američkih Država Savezna informacije Processing Standard (FIPS) na uređaju. Standardna FIPS 140 definira šifriranja algoritama za korištenje odobri NAM Savezna državne računalne sustava za zaštitu osjetljivih podataka.
    1. Da biste omogućili način FIPS, pokrenite sljedeći cmdlet:
        
        `Enter-HcsFIPSMode`

    2. Nakon što ste omogućili način FIPS tako da se šifriranja provjere snagu ponovnog pokretanja uređaja.

        > [AZURE.NOTE] Možete omogućiti ili onemogućiti Način FIPS na uređaju. Naizmjenični uređaj između FIPS i koje nisu FIPS način nije podržana.


Ako vaš uređaj ne zadovoljava preduvjete minimalne konfiguracije, vidjet ćete pogrešku u okvir tekst za natpis (prikazano u nastavku). Morat ćete promijeniti Konfiguracija uređaja tako da ga nema dovoljno resursa da bi odgovarao minimalni preduvjeti. Možete ponovno pokrenuti i povezati s uređajem. Pogledajte preduvjete minimalne konfiguraciju u [Korak 1: Provjerite ispunjava li glavno računalo sistemske preduvjete minimalne virtualnog uređaja](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-vmware/image46.png)

Druge pogreške licem tijekom početne konfiguracije putem lokalnog weba korisničkog Sučelja, pogledajte sljedeće tijekove rada [Upravljanje virtualne polja vašem StorSimple](storsimple-ova-web-ui-admin.md)korištenja lokalnog web korisničkog Sučelja.

-   Pokrenite Dijagnostički testovi za [Otklanjanje poteškoća s postavljanjem korisničkog Sučelja web](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).

-   [Generiraj zapisnika paketa i prikaz datoteka zapisnika](storsimple-ova-web-ui-admin.md#generate-a-log-package)...

## <a name="next-steps"></a>Daljnji koraci

-   [Postavite svoje StorSimple virtualne polja kao datotečnom poslužitelju](storsimple-ova-deploy3-fs-setup.md)

-   [Postavite svoje StorSimple virtualne polja kao iSCSSI poslužiteljem](storsimple-ova-deploy3-iscsi-setup.md)

