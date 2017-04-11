<properties
   pageTitle="Stvaranje paketa za podršku StorSimple | Microsoft Azure"
   description="Saznajte kako stvoriti, dešifriranje i uređivanje paket podrška za svoj uređaj StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />


# <a name="create-and-manage-a-storsimple-support-package"></a>Stvaranje i upravljanje paket StorSimple podrška

## <a name="overview"></a>Pregled

Paket za podršku StorSimple je mehanizam za jednostavan upotrebu koje prikuplja sve relevantne zapisnika radi jednostavnijeg Microsoft Support eventualne probleme s uređajem StorSimple za otklanjanje poteškoća. Zapisnike prikupljene su šifrirane i sažeti.

Pomoću ovog praktičnog vodiča sadrži detaljne upute za stvaranje i upravljanje paketa za podršku.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Stvoriti i prenijeti paket za podršku na portalu za Azure klasični

Možete stvoriti i prenijeti paket za podršku na web-mjestu Microsoft Support putem stranice za **Održavanje** servisa Azure klasični portalu.

> [AZURE.NOTE] Prijenos zahtijeva pristupni ključ za podršku. Inženjer za podršku mora sadržavati to vam u poruku e-pošte.

Paket službe za podršku šifrirane i sažeti (.cab datoteka) se stvara i prenijeti na web-mjesto za podršku. Inženjer za podršku potom možete dohvatiti paket s web-mjesta podrška za otklanjanje poteškoća.

Na portalu klasični stvaranje paketa za podršku poduzeti sljedeće korake.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>Stvaranje paketa za podršku na portalu za Azure klasični

1. Odaberite **uređaji** > **održavanja**.

2. U odjeljku **podržava paket** odaberite **Stvori i prenijeti paket za podršku**.

3. U dijaloškom okviru **Stvaranje i prijenos paket za podršku** , učinite sljedeće:

    ![Stvaranje paketa za podršku](./media/storsimple-create-manage-support-package/IC740923.png)

    - U tekstni okvir **Pristupni ključ za podršku** unesite pristupni ključ. Microsoftov inženjer za podršku treba slati ovaj pristupni ključ u e-pošte.

    - Potvrdite okvir za davanje dozvole da biste automatski prenosili paketa za podršku na web-mjesto Microsoft Support.

    - Kliknite ikonu provjeri ![Ikonu Provjera](./media/storsimple-create-manage-support-package/IC740895.png).


## <a name="manually-create-a-support-package"></a>Ručno stvaranje paketa za podršku

U nekim slučajevima morat ćete ručno stvaranje paketa za podršku putem komponente Windows PowerShell za StorSimple. Ako, na primjer:

- Ako trebate ukloniti povjerljive podatke iz datoteka zapisnika prije zajedničkog korištenja Microsoft Support.

- Ako imate poteškoća s prijenosom paket zbog problema s povezivanjem.

Pakiranju ručno stvoreni podršku možete zajednički koristiti s Microsoft Support putem e-pošte. Izvršite sljedeće korake da biste stvorili podršku paketa u Windows PowerShell za StorSimple.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Da biste stvorili podršku paketa u Windows PowerShell za StorSimple

1. Da biste započeli sesiju komponente Windows PowerShell kao administrator na udaljenom računalu koji se koristi za povezivanje s uređajem StorSimple, unesite sljedeću naredbu:

    `Start PowerShell`

2. U sesiji komponente Windows PowerShell povezati s konzole za SSAdmin uređaju:

    - U naredbeni redak upišite:

        `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`

    1. U dijaloškom okviru koji će se otvoriti unesite lozinku administratora uređaja. Lozinka zadani je:

        `Password1`

        ![PowerShell vjerodajnica dijaloškog okvira](./media/storsimple-create-manage-support-package/IC740962.png)

    2. Odaberite **u redu**.
    1. U naredbeni redak upišite:

        `Enter-PSSession $MS`

3. U sesiji koji će se otvoriti unesite odgovarajuću naredbu.

    - Za zajedničke mreže koje su zaštićene lozinkom, unesite:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`

        (Jer je paket za podršku šifriran) će zatražiti lozinka, put do zajedničku mrežnu mapu, a pristupni izraz za šifriranje. Paket za podršku zatim stvorene u određenu mapu.

    - Za zajedničke koji nisu zaštićeni lozinkom, ne morate na `-Credential` parametar. Unesite sljedeće:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`

        Za obje kontrolera u zajedničkoj mapi Navedeni mrežni stvara se paket za podršku. Je šifrirane, komprimirani datoteku koja se može poslati Microsoft Support za otklanjanje poteškoća. Dodatne informacije potražite u članku [Microsoftove podrške za kontakt](storsimple-contact-microsoft-support.md).


### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>Izvoz HcsSupportPackage cmdlet parametre
Sljedećih parametara možete koristiti s cmdlet izvoz HcsSupportPackage.

| Parametar            | Obavezno/neobavezno | Opis                                                                                                                                                             |
|----------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-Path`                 | Obavezno          | Pomoću navedite mjesto na zajedničku mrežnu mapu u kojoj se smješta u paketu za podršku.                                                                 |
| `-EncryptionPassphrase` | Obavezno          | Koristite pristupni izraz za šifriranje paketa za podršku.                                                                                                        |
| `-Credential`           | Neobavezno          | Pomoću dati pristup vjerodajnice za zajedničku mrežnu mapu.                                                                                        |
| `-Force`                | Neobavezno          | Koristite da biste preskočili korak za potvrdu šifriranja pristupni izraz.                                                                                                                |
| `-PackageTag`           | Neobavezno          | Koristite da biste odredili direktorij u odjeljku *put* smješten u paketu za podršku. Zadana vrijednost je [naziv uređaja]-[trenutni datum i time:yyyy-MM-dd-HH-mm-ss].       |
| `-Scope`                | Neobavezno          | Odredite kao **klaster** (zadano) stvaranje paketa za podršku za oba kontrolera. Ako želite pakirati samo za trenutni kontroler, navedite **kontroler**. |


## <a name="edit-a-support-package"></a>Uređivanje paket za podršku

Nakon generirao paket za podršku, možda ćete morati urediti paket za uklanjanje povjerljivih podataka. Možete uključiti nazivi jedinica, uređaj IP adresa i sigurnosne kopije imena iz datoteke zapisnika.

> [AZURE.IMPORTANT] Moguće je uređivati samo paket za podršku koji je generirana putem komponente Windows PowerShell za StorSimple. Paket stvorene na portalu Azure klasični sa servisom StorSimple Manager nije moguće uređivati.

Da biste uredili paket podršku prije prijenosa na web-mjestu Microsoft Support, najprije dešifriranje paketa za podršku, uređivanje datoteke i zatim ponovno šifriranje je. Izvršite sljedeće korake.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Da biste uredili podršku paketa u Windows PowerShell za StorSimple

1. Stvaranje paketa za podršku ranije opisane, u kojem se [pakirati podrška u ljusci Windows PowerShell za StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).

2. [Preuzimanje skriptu](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokalno na klijentu.

3. Uvoz modul Windows PowerShell. Odredite put lokalnu mapu u koju ste preuzeli skriptu. Da biste uvezli modul, unesite:

    `Import-module <Path to the folder that contains the Windows PowerShell script>`

4. Sve datoteke su *.aes* datoteke koje su spojene i šifrirane. Da biste dekomprimiranje i dešifrirati datoteke, unesite:

    `Open-HcsSupportPackage <Path to the folder that contains support package files>`

    Imajte na umu da stvarni datotečna proširenja prikazat će se za sve datoteke.

    ![Uređivanje paket za podršku](./media/storsimple-create-manage-support-package/IC750706.png)

5. Kada se od vas zatraži za šifriranje pristupni izraz, unesite pristupni izraz koji se koriste prilikom stvaranja paketa za podršku.

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:EncryptionPassphrase: ****

6. Pronađite mapu koja sadrži datoteke zapisnika. Jer su datoteke zapisnika sada dekomprimira i dešifrirati, one će imaju izvornu datotečne nastavke. Izmjena te datoteke da biste uklonili sve informacije specifične za klijenta, kao što su nazivi jedinica i uređaja IP adrese, a zatim spremite datoteke.

7. Zatvaranje datoteke komprimirati gzip i šifriranje AES 256. To se odnosi na brzinu i sigurnost u prijenos paketa za podršku putem mreže. Da biste spojili i šifriranje datoteke unesite sljedeće:

    `Close-HcsSupportPackage <Path to the folder that contains support package files>`

    ![Uređivanje paket za podršku](./media/storsimple-create-manage-support-package/IC750707.png)

8. Kada se to od vas zatraži, navedite pristupni izraz za šifriranje paketa izmijenjene podrška.

        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****

9. Zapišite novi pristupni izraz tako da biste mogli razmijeniti s Microsoft Support primitku.


### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Primjer: Uređivanje datoteka u paketu za podršku na zajedničkom mjestu zaštićene lozinkom

Sljedeći primjer pokazuje kako dešifrirati, uređivanje i ponovno Šifriraj paket za podršku.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [koristiti paketi za podršku i zapisnika uređaj da biste otklonili implementaciju sustava uređaja](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).

- Saznajte kako [koristiti StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).
