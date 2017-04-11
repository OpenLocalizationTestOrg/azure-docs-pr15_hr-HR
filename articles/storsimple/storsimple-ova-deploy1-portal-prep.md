<properties
   pageTitle="Implementacija StorSimple virtualne polja 1 – Priprema portala"
   description="Prvi vodič za implementaciju StorSimple virtualne polja tiče Priprema portal"
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
   ms.date="05/24/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---prepare-the-portal"></a>Implementacija StorSimple virtualne polja – Priprema portala

![](./media/storsimple-ova-deploy1-portal-prep/getstarted4.png)

## <a name="overview"></a>Pregled

Ovaj se članak odnosi Microsoft Azure StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualnog uređaja ili StorSimple virtualnog uređaja) izvodi ožujku 2016 Općenito dostupan (GA) izdanje. Ovo je prvog članka u nizu vodiči za implementaciju potrebne za potpuno implementaciju sustava virtualne polja kao poslužitelj datoteka ili poslužitelju komponente iSCSI. U ovom se članku opisuju priprema potrebne za stvaranje i konfiguriranje servisa za Upravitelj StorSimple prije dodjele resursa virtualne polja. U ovom se članku je povezana i odgovor na kontrolni popis za konfiguraciju implementacije kao i konfiguracija preduvjeti.

Trebat će vam administratorske ovlasti da biste dovršili postupak instalacije i konfiguracije. Preporučujemo da pregledate kontrolni popis za konfiguraciju implementacije prije nego što počnete. Priprema portala se manje od 10 minuta.

Informacije objaviti u ovom članku odnose se na implementaciju StorSimple virtualne polja u Azure klasični portal, kao i Microsoft Azure državne Cloud.

### <a name="get-started"></a>Početak rada

Tijek rada za implementaciju sastoji se od Priprema portala za dodjeljivanje virtualne polja u virtualiziranom okruženju i dovršavanje postavljanja. Da biste započeli implementaciju StorSimple virtualne polja kao poslužitelj datoteka ili poslužitelju komponente iSCSI, morat ćete potražite u sljedećim resursima tabulated (članke i videozapise).

#### <a name="deployment-articles"></a>Članci implementacije

Potražite u sljedećim člancima propisanim redoslijedom za implementaciju sustava StorSimple virtualne polja.

| **#** | **U ovom ćete koraku**                          | **To ćete učiniti...**                                                         | **Pomoću tih dokumenata.**|
|------|-------------------------------------------|--------------------------------------------------------------------------------|------------------------|
|1.   | **Postavljanje portala za Azure klasični**       | Stvaranje i konfiguriranje servisa za Upravitelj StorSimple prije dodjele resursa StorSimple virtualnog uređaja.  |[Priprema portala](storsimple-ova-deploy1-portal-prep.md)|
|2.   | **Dodjela virtualne polja**           | Dodjela Hyper-V i povezati StorSimple virtualnog uređaja u sustavu glavno računalo Hyper-V sustavom Windows Server 2012 R2, Windows Server 2012 ili Windows Server 2008 R2. <br></br> <br></br> VMware, Dodjela resursa i povezivanje s StorSimple lokalne virtualne uređaj na glavno računalo u kojem se izvodi VMware ESXi 5,5 i iznad.<br></br>| [Dodjela virtualne polja u Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) <br></br> <br></br> [Dodjela virtualne polja u VMware](storsimple-ova-deploy2-provision-vmware.md)|
|3.    | **Postavljanje virtualne polja**              | Za poslužitelj datoteka izvođenje početnog postavljanja, registrirati poslužitelj datoteka StorSimple i dovršili postavljanje uređaja. Zatim možete Dodjela SMB zajedničko korištenje. <br></br> <br></br> Za poslužitelj za iSCSI izvođenje početnog postavljanja, registrirati iSCSI poslužitelja StorSimple i dovršili postavljanje uređaja. Zatim možete Dodjela iSCSI količine.| [Postavljanje virtualne polja kao datotečnom poslužitelju](storsimple-ova-deploy3-fs-setup.md)<br></br> <br></br>[Postavljanje virtualne polja kao iSCSI poslužitelj](storsimple-ova-deploy3-iscsi-setup.md)|

#### <a name="deployment-videos"></a>Videozapisi za implementaciju

| **U ovom koraku...** |  **Pogledajte ovaj videozapis.**|
|----------------|-------------|
| Detaljne upute za početak rada s StorSimple virtualne polja. | [Početak rada s StorSimple virtualne polja](https://azure.microsoft.com/documentation/videos/get-started-with-the-storsimple-virtual-array/)|
| Detaljne upute za dodjeljivanje StorSimple virtualne polja u Hyper-V.|[Stvaranje polja za virtualne StorSimple](https://azure.microsoft.com/documentation/videos/create-a-storsimple-virtual-array/) |
|Detaljne upute za konfiguriranje i registrirati StorSimple polja za virtualne|[Konfiguriranje polja za virtualne StorSimple](https://azure.microsoft.com/documentation/videos/configure-a-storsimple-virtual-array/)|
|Detaljne upute za stvaranje zajedničkih mapa, sigurnosno kopiranje dionice i vraćanje podataka na StorSimple virtualne polja konfigurirati kao datotečnom poslužitelju|[Korištenje polja virtualne StorSimple](https://azure.microsoft.com/documentation/videos/use-the-storsimple-virtual-array/)|
|Detaljne upute za prebacivanje i Izrada oporavak polja za virtualne StorSimple|[StorSimple virtualne polja Izrada oporavak](https://azure.microsoft.com/documentation/videos/storsimple-virtual-array-disaster-recovery/)

Sada možete početi da biste postavili Azure klasični portal.

## <a name="configuration-checklist"></a>Kontrolni popis za konfiguraciju

Kontrolni popis za konfiguraciju opisuje informacije koje morate prikupiti prije no što konfigurirate softvera na vašem uređaju StorSimple. Priprema te podatke na vrijeme će vam pomoći pojednostavnjenje postupka implementacije StorSimple uređaja u svom okruženju. Ovisno o tome virtualnog uređaja StorSimple će biti implementirano kao poslužitelj datoteka ili poslužitelju komponente iSCSI, trebat će vam neku od sljedeće kontrolne popise.

-   Preuzmite [kontrolnog popisa konfiguracija StorSimple virtualne polja datoteke poslužitelja](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).

-   Preuzmite [StorSimple virtualne polja iSCSI kontrolni popis za konfiguraciju poslužitelja](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Preduvjeti

Ovdje ćete pronaći konfiguracije preduvjeti za uslugu StorSimple upravitelj, StorSimple virtualnog uređaja i s mrežom podatkovnog centra.

### <a name="for-the-storsimple-manager-service"></a>Upravitelj StorSimple servisa

Prije početka, provjerite:

-   Imate Microsoftov račun pomoću vjerodajnica programa access.

-   Imate račun za pohranu Microsoft Azure pomoću vjerodajnica programa access.

-   Pretplate na Microsoft Azure mora biti omogućen za servis upravitelja StorSimple.

### <a name="for-the-storsimple-virtual-device"></a>Za StorSimple virtualnog uređaja

Prije implementacije virtualnog uređaja, provjerite:

-   Imate pristup sustavu glavno računalo izvodi Hyper-V na Windows Server 2008 R2 ili novijem ili VMware (ESXi 5,5 ili noviji) koji se mogu koristiti u Dodjela resursa za uređaj.

-   Sustav glavnog računala je moći odvojiti Dodjela virtualnog uređaja u sljedećim resursima:

    -   Najmanje 4 jezgri.

    -   Najmanje 8 GB RAM-a.

    -   Sučelje za jedne mreže.

    -   500 GB virtualne disk za podatke sustava.

### <a name="for-the-datacenter-network"></a>Za mrežu podatkovnim centrom

Prije početka, provjerite:

-   Mreža u vašem podatkovnog centra bude konfigurirana po umrežavanje preduvjeti za svoj uređaj StorSimple. Dodatne informacije potražite u članku [Sistemski preduvjeti za virtualne polja StorSimple](storsimple-ova-system-requirements.md).

-   Vaš uređaj virtualne StorSimple ima namjenski 5 MB/s internetska propusnost (ili više njih) dostupne cijelo vrijeme. U ovom propusnosti treba moguće zajednički koristiti s drugim aplikacijama.

## <a name="step-by-step-preparation"></a>Priprema korak po korak

Koristite sljedeće detaljne upute za pripremu portalom StorSimple Upravitelj servisa.

## <a name="step-1-create-a-new-service"></a>Korak 1: Stvaranje novog servisa

Instancu servisa StorSimple Manager možete upravljati više StorSimple 1200 uređaja. Izvršite sljedeće korake da biste stvorili novu instancu StorSimple Upravitelj servisa. Ako imate postojeću StorSimple Upravitelj servisa za upravljanje uređajima 1200, preskočite ovaj korak i prijeđite na [Step2: Registracija ključ servis](#step-2-get-the-service-registration-key).

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

> [AZURE.IMPORTANT]
>
> Ako ne uspijete omogućite automatsko stvaranje računa za pohranu sa servisima, morat ćete stvoriti barem jedan račun za pohranu kada uspješno stvorite servisa.
>

> - Ako niste stvorili račun za pohranu automatski, idite na [Konfiguracija novi prostor za pohranu račun za servis](#optional-step-configure-a-new-storage-account-for-the-service) detaljne upute.
>

> - Ako ste omogućili automatsko stvaranje računa za pohranu, idite na [Korak 2: Registracija ključ servis](#step-2-get-the-service-registration-key).


## <a name="step-2-get-the-service-registration-key"></a>Korak 2: Dobivanje ključa za registraciju servisa


Kada je servis StorSimple Manager s radom, morat ćete dobiti ključa za registraciju servisa. U ovom ključ se koristi da biste registrirali i uređaju StorSimple povezivanje sa servisom.

[Azure klasični portal](https://manage.windowsazure.com/)poduzeti sljedeće korake.


[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

> [AZURE.NOTE]
>
> Servis ključa za registraciju se koristi za registraciju uređaja upravitelj StorSimple koje je potrebno da biste registrirali sa servisima StorSimple Upravitelj.

## <a name="step-3-download-the-virtual-device-image"></a>Korak 3: Preuzimanje slika virtualnog uređaja

Nakon što ste ključa za registraciju servisa, morat ćete preuzeti slike odgovarajuće virtualnog uređaja Dodjela virtualnog uređaja u sustavu glavnog računala. Slika virtualnog uređaja su operacijski sustav, a mogu se preuzeti sa stranice brzi početak rada na portalu za Azure klasični.

> [AZURE.IMPORTANT] Softver koji se izvodi na polja virtualne StorSimple može se koristiti samo u kombinaciji sa servisom Storsimple Manager.


[Azure klasični portal](https://manage.windowsazure.com/)poduzeti sljedeće korake.

#### <a name="to-get-the-virtual-device-image"></a>Da biste sliku virtualnog uređaja

1.  Na stranici **upravitelja StorSimple servisa** kliknite servis koji ste stvorili. To će vas odvesti na stranicu za **Brzo pokretanje** . (Možete kliknuti ikonu kratke ![](./media/storsimple-ova-deploy1-portal-prep/image8.png) da biste pristupili stranici **Brzi početak rada** u bilo kojem trenutku.)

1.  Kliknite vezu koja odgovara sliku koju želite preuzeti iz Microsoft Download Center. Slikovne datoteke su približno 4.8 GB.

    -   VHDX za Hyper-V na Windows Server 2012 i novijim verzijama

    -   VHD za Hyper-V na Windows Server 2008 R2 i novijim verzijama

    -   VMDK VMWare ESXi 5,5 i novijim verzijama

2.  Preuzmite i raspakirajte datoteku na lokalni pogon, čime bilješku o kojem se nalazi raspakiranu datoteku paketa datoteka.

![Ikona videoprikaz:](./media/storsimple-ova-deploy1-portal-prep/video_icon.png) **videozapis dostupna**

Pogledajte videozapis detaljne upute za početak rada s StorSimple virtualne polja.

> [AZURE.VIDEO get-started-with-the-storsimple-virtual-array]



## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Neobavezan korak: Konfiguriranje novog računa za pohranu za servis

Ovaj korak nije obavezan koje je potrebno izvršiti samo ako ne uspijete omogućiti automatsko stvaranje računa za pohranu sa servisima.

Ako je potrebno stvoriti račun za Azure prostora za pohranu u nekoj drugoj regiji potražite u članku [upute za stvaranje računa za pohranu](storage-create-storage-account.md#create-a-storage-account) za detaljne upute.

[Azure klasični portal](https://manage.windowsazure.com/) na stranici upravitelja StorSimple servisa da biste dodali račun za pohranu postojeće Microsoft Azure poduzeti sljedeće korake.

#### <a name="to-add-a-storage-account"></a>Da biste dodali račun za pohranu

1.  Servis Upravitelj StorSimple odredišne stranice, odaberite uslugu, a zatim je dvokliknite. To će vas odvesti na stranicu za **Brzo pokretanje** . Odaberite stranicu **Konfiguriraj** .

2.  Kliknite **Dodavanje/uređivanje prostora za pohranu račun**. U dijaloškom okviru **Dodavanje/uređivanje prostora za pohranu računa** učinite sljedeće:

    1.  Kliknite **Dodaj novi**.

    1.  Navedite naziv za vaš račun za pohranu.

    1.  Navedite primarni **Tipkovni prečac** za vaš račun sustava Microsoft Azure prostora za pohranu.

    1.  Odaberite **način rada Omogućivanje SSL** stvaranje sigurnog kanala za mrežnu komunikaciju između uređaja i s oblakom. Poništite potvrdni okvir **Omogući SSL način** samo ako radite u privatnu oblak.

    1.  Kliknite ikonu provjeri ![](./media/storsimple-ova-deploy1-portal-prep/image7.png). Bit ćete obaviješteni Nakon uspješnog stvaranja računa za pohranu.

        ![](./media/storsimple-ova-deploy1-portal-prep/image11.png)

1.  Na stranici **Konfiguracija** u odjeljku **Računi za pohranu**prikazat će se račun novostvorenu prostora za pohranu. Kliknite **Spremi** da biste spremili novostvorenu prostora za pohranu računa. Kliknite **u redu** kada se to od vas zatraži potvrdu.


## <a name="next-step"></a>Sljedeći korak

Sljedeći je korak Dodjela virtualnog računala za svoj uređaj virtualne StorSimple. Ovisno o operacijskom sustavu glavno računalo, pročitajte članak detaljne upute:

-   [Dodjela resursa StorSimple virtualne polja u Hyper-V](storsimple-ova-deploy2-provision-hyperv.md)

-   [Dodjela resursa StorSimple virtualne polja u VMware](storsimple-ova-deploy2-provision-vmware.md)
