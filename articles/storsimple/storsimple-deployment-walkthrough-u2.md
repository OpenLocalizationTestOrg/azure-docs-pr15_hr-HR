<properties 
   pageTitle="Implementacija uređaju StorSimple (ažuriranje 2) | Microsoft Azure"
   description="U članku se opisuje korake i najbolje prakse za implementaciju ažuriranju 2 za StorSimple uređaja i usluga."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="deploy-your-on-premises-storsimple-device-update-2"></a>Implementacija StorSimple uređaju lokalnog (ažuriranje 2)

> [AZURE.SELECTOR]
- [Ažuriranje 2 i noviji](../articles/storsimple/storsimple-deployment-walkthrough-u2.md)
- [Ažuriranje 1](../articles/storsimple/storsimple-deployment-walkthrough-u1.md)
- [Izdanje GA](../articles/storsimple/storsimple-deployment-walkthrough.md)

## <a name="overview"></a>Pregled

Dobro došli u Microsoft Azure StorSimple uređaj implementacije. Ovim praktičnim vodičima implementacije primjenjuju se na StorSimple 8000 niz ažuriranju 2. Ovaj niz vodiči za obuhvaća kontrolni popis za konfiguraciju, preduvjeti za konfiguraciju i detaljne konfiguracijske korake za svoj uređaj StorSimple.

Informacije u ovim praktičnim vodičima pretpostavlja da pregledava mjere opreza sigurnost i raspakirane, racked i cabled StorSimple uređaj. Ako i dalje morate izvesti te zadatke, započnite pregled [mjere opreza sigurnost](storsimple-safety.md). Slijedite upute određeni uređaj otpakiravanje, za bicikle postavljanja i kabelski uređaj.

- [Otpakiravanje, za bicikle postavljanja i kabelski vaše 8100](storsimple-8100-hardware-installation.md)
- [Otpakiravanje, za bicikle postavljanja i kabelski vaše 8600](storsimple-8600-hardware-installation.md)

Trebat će vam administratorske ovlasti da biste dovršili postupak instalacije i konfiguracije. Preporučujemo da pregledate kontrolni popis za konfiguraciju prije nego što počnete. Postupak implementacije i konfiguracija može potrajati neko vrijeme da biste dovršili.

> [AZURE.NOTE] Informacije o implementaciji StorSimple objaviti na web-mjestu Microsoft Azure odnosi se na StorSimple 8000 samo niz uređaja. Dovršavanje informacije o uređajima 7000 niz: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 7000 niz implementacije informacije potražite u članku [StorSimple sustava vodič za brzi početak rada](http://onlinehelp.storsimple.com/111_Appliance/).

## <a name="deployment-steps"></a>Koraci za implementaciju

Izvođenje korake potrebne za konfiguriranje StorSimple uređaj i povežite ga s StorSimple Upravitelj usluge. Osim potrebne korake postoje dodatni koraci i postupci potrebni su vam tijekom uvođenja. Upute za detaljne uvođenje pokazuje kada provoditi svaki od ovih koraka nije obavezno.


| Korak                                                                                   | Opis                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **PREDUVJETI**                                                                      | To morate izvršiti u Priprema nadolazeće implementacije.                                                                                        |
| [Kontrolni popis za konfiguraciju implementacije](#deployment-configuration-checklist)                                                     | Prikupljanje i bilježenje podataka istovjetan i tijekom uvođenja pomoću navedeni popis za provjeru.                                                                       |
| [Preduvjeti za implementaciju](#deployment-prerequisites)                                                               | Te provjeru okruženje spremna je za implementaciju.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **KORAK PO KORAK IMPLEMENTACIJE**                                                                   | Za implementaciju uređaj StorSimple radnog potrebni su ove korake.                                                                                      |
| [Korak 1: Stvaranje novog servisa](#step-1-create-a-new-service)                                                         | Postavljanje upravljanja oblaka i prostora za pohranu za svoj uređaj StorSimple. *Preskoči ovaj korak ako imate postojeći servis za druge uređaje StorSimple*.                |
| [Korak 2: Dobivanje ključa za registraciju servisa](#step-2-get-the-service-registration-key)                                               | Pomoću ovog ključa registrirati i povezivanje uređaja StorSimple pomoću servisa za upravljanje.                                                                         |
| [Korak 3: Konfigurirati i registrirati uređaja putem komponente Windows PowerShell za StorSimple](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)    | Povežete s mrežom, a registrirali s Azure da biste dovršili instalaciju putem servisa za upravljanje.                                            |
| [Korak 4: Dovršavanje minimalne uređaja](#step-4-complete-minimum-device-setupd)</br>[Neobavezno: Ažuriranje StorSimple uređaj](#scan-for-and-apply-updates)      | Pomoću servisa za upravljanje da biste dovršili postavljanje uređaja i omogućili da biste postigli prostora za pohranu.                                                                      |
| [5 korak: Stvaranje spremnika glasnoće](#step-5-create-a-volume-container)                                                      | Stvaranje spremnika jedinicama dodjele resursa. Spremnik glasnoću je račun za pohranu, propusnosti te postavke šifriranja za sve količine koje se nalaze u njoj.    |
| [Korak 6: Stvaranje jedinice](#step-6-create-a-volume)                                                                | Dodjela resursa za pohranu volume(s) na uređaju StorSimple za poslužitelja.                                                                                        |
| [Korak 7: Postavljanje, pokrenuti i oblikovati jedinica](#step-7-mount-initialize-and-format-a-volume)</br>[Neobavezno: Konfiguriranje MPIO](storsimple-configure-mpio-windows-server.md)            | Povezivanje vaši poslužitelji za pohranu iSCSI nudi uređaj. Ako želite konfigurirati MPIO da bi vaši poslužitelji možete tolerate vezu, mreže i sučelje nije uspjelo.                                                                                                                                                              |
| [Korak 8: Poduzeti sigurnosne kopije](#step-8-take-a-backup)                                                                  | Postavljanje sigurnosnih kopija pravilnika za zaštitu podataka                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **OSTALE PROCEDURE**                                                                   | Možda ćete morati pogledajte ove postupke kao implementaciju rješenja.                                                                                        |
| [Konfiguriranje novog računa za pohranu za servis](#configure-a-new-storage-account-for-the-service)                                      |                                                                                                                                                               |
| [Korištenje PuTTY za povezivanje s konzole serijski uređaj](#use-putty-to-connect-to-the-device-serial-console)                                    |                                                                                                                                                               |
| [Početak IQN glavnog računala za Windows Server](#get-the-iqn-of-a-windows-server-host)                                                   |                                                                                                                                                               |
| [Stvaranje ručno sigurnosno kopiranje](#create-a-manual-backup)                                                                 | 


## <a name="deployment-configuration-checklist"></a>Kontrolni popis za konfiguraciju implementacije

Prije implementacije uređaju, morat ćete prikupljanje informacija da biste konfigurirali softvera na vašem uređaju StorSimple. Priprema neke od tih informacija na vrijeme će vam pomoći pojednostavnjenje postupka implementacije StorSimple uređaja u svom okruženju. Preuzimanje i korištenje navedeni popis za provjeru Napomena dolje Detalji o konfiguraciji implementacija uređaj.

- [Preuzmite StorSimple kontrolni popis za konfiguraciju implementacije](http://www.microsoft.com/download/details.aspx?id=49159)


## <a name="deployment-prerequisites"></a>Preduvjeti za implementaciju

U sljedećim odjeljcima objašnjavaju konfiguracije preduvjeti za servis za Upravitelj StorSimple i StorSimple uređaj.

### <a name="for-the-storsimple-manager-service"></a>Upravitelj StorSimple servisa

Prije početka, provjerite:

- Imate Microsoftov račun pomoću vjerodajnica programa access.

- Imate račun za pohranu Microsoft Azure pomoću vjerodajnica programa access.

- Pretplate na Microsoft Azure je omogućena za StorSimple Upravitelj servisa. Putem [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/), trebali biste kupili pretplatu.

- Imate pristup terminal emulacija softver kao što su PuTTY.

### <a name="for-the-device-in-the-datacenter"></a>Za uređaj u s podatkovnim centrom

Prije konfiguriranja uređaj, provjerite je li uređaj je potpuno raspakirane, postavljen na na bicikle i potpuno cabled za power, mreže i serijskog access, kao što je opisano u:

-  [Otpakiravanje, za bicikle postavljanja i kabelski 8100 uređaja](storsimple-8100-hardware-installation.md)
-  [Otpakiravanje, za bicikle postavljanja i kabelski uređaju 8600](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>Za mrežu u s podatkovnim centrom

Prije početka, provjerite:

- Da biste dopustili iSCSI i cloud promet kao što je opisano u [umrežavanje preduvjeti za svoj uređaj StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)otvaraju se priključaka u vatrozidu za podatkovnog centra.


## <a name="step-by-step-deployment"></a>Korak po korak implementacije

Pomoću sljedećih detaljne upute za implementaciju StorSimple uređaj s podatkovnim centrom.

## <a name="step-1-create-a-new-service"></a>Korak 1: Stvaranje novog servisa

Upravitelj StorSimple servisa možete upravljati više StorSimple uređaja. Izvršite sljedeće korake da biste stvorili novu instancu StorSimple Upravitelj servisa.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [AZURE.IMPORTANT] Ako ne uspijete omogućite automatsko stvaranje računa za pohranu sa servisima, morat ćete stvoriti barem jedan račun za pohranu kada uspješno stvorite servisa. Taj račun za pohranu koristit će se prilikom stvaranja spremniku glasnoću. 
>
> * Ako niste stvorili račun za pohranu automatski, idite na [Konfiguracija novi prostor za pohranu račun za servis](#configure-a-new-storage-account-for-the-service) detaljne upute. 
> * Ako ste omogućili automatsko stvaranje računa za pohranu, idite na [Korak 2: Registracija ključ servis](#step-2-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Korak 2: Dobivanje ključa za registraciju servisa

Kada je servis StorSimple Manager s radom, morat ćete dobiti ključa za registraciju servisa. U ovom ključ se koristi da biste registrirali i uređaju StorSimple povezivanje sa servisom.

Na portalu za upravljanje poduzeti sljedeće korake.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Korak 3: Konfigurirati i registrirati uređaja putem komponente Windows PowerShell za StorSimple

Pomoću komponente Windows PowerShell za StorSimple da biste dovršili početnog postavljanja uređaju StorSimple kao što je opisano u nastavku. Morat ćete koristiti softver terminal emulacija da biste dovršili ovaj korak. Dodatne informacije potražite u članku [Korištenje PuTTY za povezivanje s konzole serijski uređaj](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Korak 4: Dovršavanje minimalne uređaja

Za konfiguraciju minimalne uređaj StorSimple uređaja koje su potrebne za: 

- Postavite sekundarne DNS poslužitelj.
- Omogućivanje iSCSI barem jedan mrežni sučelje.
- Dodijelite nepromjenjivi IP adresa za oba kontrolera.

Na portalu za upravljanje da biste dovršili postavljanje minimalne uređaju poduzeti sljedeće korake.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Korak 5: Stvaranje spremnika glasnoće

Spremnik glasnoću je račun za pohranu, propusnosti te postavke šifriranja za sve količine koje se nalaze u njoj. Morat ćete stvaranje spremnika glasnoću prije no što počnete dodjeljivanje količine na uređaju StorSimple. 

Na portalu za upravljanje stvaranje kontejnera za količinsko poduzeti sljedeće korake.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Korak 6: Stvaranje jedinice

Kada stvorite spremniku glasnoće, Dodjela resursa jedinice za pohranu na uređaju StorSimple za poslužitelja. Na portalu za upravljanje da biste stvorili jedinicu poduzeti sljedeće korake.

> [AZURE.IMPORTANT] Upravitelj StorSimple možete stvoriti i zavisnim i potpuno dodijeljenu količine. No ne možete stvoriti djelomično dodijeljenu količine. 

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Korak 7: Postavljanje, pokrenuti i oblikovati jedinica

Sljedeći koraci se izvode na vašem računalu koje hostira Windows Server. 


> [AZURE.IMPORTANT]

> - Visoke dostupnosti rješenje StorSimple, preporučujemo konfiguriranje MPIO na poslužiteljima glavnog računala (nije obavezno) prije konfiguriranja iSCSI. Konfiguracija MPIO na poslužiteljima glavno računalo će osigurati možete tolerate poslužitelji vezu, mreži ili sučelje nije uspjelo.

> - MPIO i iSCSI instalacije i konfiguracije upute o glavnog računala za Windows Server, idite na [Konfiguriranje MPIO za svoj uređaj StorSimple](storsimple-configure-mpio-windows-server.md). To će obuhvaća korake postavljanja, pokrenuti i oblikovati StorSimple količine.

> - MPIO i iSCSI instalacije i konfiguracije upute na glavnom računalu Linux, potražite u odjeljku [Konfiguriranje MPIO za vaše StorSimple Linux glavnog računala](storsimple-configure-mpio-on-linux.md)

Ako odlučite da ne konfiguriranje MPIO, izvedite sljedeće korake za postavljanje, pokrenuti i oblikovanje diskova StorSimple na glavnom računalu Windows Server.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Korak 8: Poduzeti sigurnosne kopije

Sigurnosno kopiranje točke u vrijeme zaštiti od količine i poboljšati mogućnost oporavka tijekom minimiziranje i vraćanje vremena. Dvije vrste sigurnosnog kopiranja možete provoditi na uređaju StorSimple: lokalne snimke i oblaka snimke. Svaki od tih vrsta sigurnosne kopije može biti **zakazano** ili **ručno**. 

Na portalu za upravljanje da biste stvorili zakazano sigurnosno kopiranje poduzeti sljedeće korake.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Ručno može potrajati sigurnosne kopije u bilo kojem trenutku. Postupak, otvorite članak [Stvaranje ručno sigurnosno kopiranje](#create-a-manual-backup). 

## <a name="configure-a-new-storage-account-for-the-service"></a>Konfiguriranje novog računa za pohranu za servis

Ovo je neobavezan korak koji morate izvesti samo ako ne uspijete omogućiti automatsko stvaranje računa za pohranu sa servisima. Za stvaranje spremnika glasnoću StorSimple potreban je račun sustava Microsoft Azure prostora za pohranu.

Ako vam je potrebna stvorite račun za Azure prostora za pohranu u nekoj drugoj regiji, pročitajte članak [O Azure prostora za pohranu računi](../storage/storage-create-storage-account.md) detaljne upute.

Na portalu za upravljanje, na stranici **upravitelja StorSimple servisa** poduzeti sljedeće korake.

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Korištenje PuTTY za povezivanje s konzole serijski uređaj

Da biste povezali komponente Windows PowerShell za StorSimple, morate koristiti softver terminal emulacija kao što su PuTTY. Možete koristiti PuTTY prilikom pristupa uređaj izravno putem konzole za serijski ili tako da otvorite telnet sesije s udaljenog računala.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]


## <a name="scan-for-and-apply-updates"></a>Traženje i Primjena ažuriranja

Ažuriranje uređaju može potrajati nekoliko sati. Izvođenje sljedeće korake za traženje i Primjena ažuriranja na uređaju.
<!--can take 1-4 hours--> 

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Da biste ažurirali uređaju

1.  Na stranici **Brzi Start** uređaja kliknite **uređaji**. Odaberite uređaj fizički, kliknite **Održavanje** , a zatim kliknite **Pregled ažuriranja**.  

2.  Stvoren je zadatak da biste pregledali dostupna ažuriranja. Ako su dostupna ažuriranja, **Skeniranje ažuriranja** se mijenja u **Instalirati ažuriranja**. Kliknite **Instaliraj ažuriranja**. 

3.  Stvorit će se na zadatak ažuriranja. Praćenje statusa ažuriranje tako da odete **zadatke**.

    > [AZURE.NOTE] Kada se pokrene zadatak ažuriranja, ga odmah prikazuje status kao 50 posto. Promjena statusa na 100% tek nakon dovršetka posla ažuriranja. Postoji nema u stvarnom vremenu za postupku ažuriranja.

4.  Nakon uspješno ažurira uređaj, omogućite podataka 2 i 3 podataka sučelje mreže ako oni su onemogućena.

<!-- In step 2, you may be requested to disable Data 2 and Data 3 prior to installing the updates. You must disable these network interfaces or the updates may fail.-->

## <a name="get-the-iqn-of-a-windows-server-host"></a>Početak IQN glavnog računala za Windows Server

Izvršite sljedeće korake da biste dobili iSCSI kvalificirani naziv (IQN) Windows glavnog računala sa sustavom Windows Server® 2012.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Stvaranje ručno sigurnosno kopiranje

Na portalu za upravljanje da biste stvorili ručno sigurnosnu kopiju sustava na zahtjev za jednu na uređaju StorSimple poduzeti sljedeće korake.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]


## <a name="next-steps"></a>Daljnji koraci

- Konfiguriranje [virtualnog uređaja](storsimple-virtual-device-u2.md).

- Koristite [Upravitelj StorSimple servisa](storsimple-manager-service-administration.md) za upravljanje StorSimple uređaj.
 
