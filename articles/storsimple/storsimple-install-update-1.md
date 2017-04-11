<properties
   pageTitle="Instalacija ažuriranja 1.2 na uređaju StorSimple | Microsoft Azure"
   description="U članku se objašnjava kako instalirati StorSimple 8000 niz Update 1.2 na uređaju StorSimple 8000 niz."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/22/2016"
   ms.author="alkohli" />

# <a name="install-update-12-on-your-storsimple-device"></a>Instaliranje ažuriranja 1.2 na uređaj StorSimple

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča objašnjava kako instalirati ažuriranje 1.2 na uređaju StorSimple verzijom softver prije no što Update 1. Vodič obuhvaća i dodatni koraci potrebni za ažuriranje kada pristupnik konfiguriran na mrežnom sučelju osim podataka 0 StorSimple uređaja.

Ažuriranje 1.2 obuhvaća uređaj softverska ažuriranja, LSI ažuriranja za upravljačke programe i ažuriranja opreme disk. Softvera i LSI ažuriranja za upravljačke programe koje nisu disruptive ažuriranja, a mogu se primijeniti putem portala za Azure klasični. Ažuriranja opreme disk disruptive ažuriranja, a možete primijeniti samo putem sučelja komponente Windows PowerShell uređaja.

Ovisno o tome koju verziju uređaj je pokrenut, možete odrediti ako Update 1.2 će se primijeniti. Verzija softvera uređaja možete provjeriti tako da odete do odjeljka **brzi pogled** uređaju **nadzorne ploče**.

</br>

| Ako je instalirana verzija programa...   | Što se događa na portalu?                              |
|---------------------------------|--------------------------------------------------------------|
| Izdanje - GA                    | Ako koristite verziju (GA), ne primjenjuju se to ažuriranje. Imajte [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) radi ažuriranja uređaja.|
| Ažuriranje 0,1                      | Portal primjenjuje Update 1.2.                                |
| Ažuriranje 0,2                      | Portal primjenjuje Update 1.2.                                |
| Ažuriranje 0,3                      | Portal primjenjuje Update 1.2.                                |
| Ažuriranje 1                        | To ažuriranje neće biti dostupne.                           |
| Ažuriranje 1.1                      | To ažuriranje neće biti dostupne.                           |

</br>

> [AZURE.IMPORTANT]

> -  Možda nećete vidjeti Update 1.2 odmah jer moramo fazama uvođenje ažuriranja. Traženje ažuriranja u nekoliko dana ponovno kao to ažuriranje će postati dostupne uskoro.
> - To ažuriranje sadrži skup ručno i automatsko stara Provjera radi određivanja stanja uređaj pomoću hardvera stanje i mrežne veze. Ove stara provjerava se izvode samo ako Primjena ažuriranja na portalu Azure klasični.
> - Preporučujemo da instalirate ažuriranja za softver i upravljački program putem portala za Azure klasični. Trebali biste samo otvorite sučelja komponente Windows PowerShell uređaja (za instaliranje ažuriranja) ako ne prođu provjeru stara ažuriranje pristupnika na portalu. Ažuriranja može potrajati 5 10 sati da biste instalirali (uključujući ažuriranja za Windows). Način ažuriranja održavanja mora biti instaliran putem sučelja komponente Windows PowerShell uređaja. Kao što su ažuriranja način održavanja disruptive ažuriranja, to će rezultirati dolje vremena za svoj uređaj.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Instalacija ažuriranja 1.2 putem portala za Azure klasični

Izvršite sljedeće korake da biste ažurirali uređaj da biste [ažurirali 1.2](storsimple-update1-release-notes.md). Ovaj postupak koristite samo ako je konfiguriran na sučelje 0 mreže podataka na uređaju pristupnik.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Provjerite je li uređaj je pokrenut **StorSimple 8000 niz Update 1.2 (6.3.9600.17584)**. **Zadnje ažuriranje datuma** moraju se i mijenjati. Vidjet ćete i dostupnosti održavanja način ažuriranja (Ova poruka mogu nastaviti da se prikazuje u roku od 24 sata kada instalirate ažuriranja).

    Održavanje način ažuriranja su disruptive ažuriranja koja rezultiraju nedostupnost uređaja i se mogu primijeniti samo putem sučelja komponente Windows PowerShell uređaja.

    ![Stranice za održavanje] (./media/storsimple-install-update-1/InstallUpdate12_10M.png "Stranice za održavanje")

13. Preuzmite način ažuriranja održavanja slijedeći korake navedene u [Da biste preuzeli hitnih]( #to-download-hotfixes) da biste potražili i preuzimanje KB3063416, koji instalira disk ažuriranja opreme (ostala ažuriranja mora biti instalirao odmah).

13. Slijedite korake navedene u [instalirati i provjerite je li hitnih način održavanja](#to-install-and-verify-maintenance-mode-hotfixes) instalirati ažuriranja za održavanje načinu rada.

14. Azure klasični portalu idite na stranicu za **Održavanje** i pri dnu stranice, kliknite **Pregled ažuriranja** za traženje ažuriranja sustava Windows, a zatim kliknite **Instalirajte ažuriranja**. Završite nakon primjene svih ažuriranja uspješno instaliran.



## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Instalacija ažuriranja 1.2 na uređaju koji je konfiguriran za 0 mrežno sučelje koje nisu podataka pristupnik

Koristite ovaj postupak samo ako uspjeti potvrdite pristupnika prilikom instalacije ažuriranja putem Azure klasični portal. Imate pristupnik dodijeljene 0 mrežno sučelje koje nisu podataka i uređaju sa verzijom softver prije no što Update 1, ne prođu provjeru. Ako vaš uređaj pristupnika na 0 mrežno sučelje koje nisu podataka, možete ažurirati uređaju izravno na portalu Azure klasični. Potražite u članku [Instalacija ažuriranje 1.2 putem Azure klasični portal](#install-update-1.2-via-the-azure-classic-portal).

Verzija softvera koju je moguće nadograditi koristite taj način su ažuriranja 0,1, ažuriranje 0,2 i ažuriranje 0,3.


> [AZURE.IMPORTANT]
>
> - Ako vaš uređaj radi izdanje (GA) verzija, obratite se [Microsoftovoj službi za podršku](storsimple-contact-microsoft-support.md) radi jednostavnijeg ažuriranja.
> - Ovaj postupak potrebno izvršiti samo jedanput da biste primijenili Update 1.2. Pomoću portala za Azure klasični sljedeće ažuriranje.

Ako vaš uređaj pokrenut je stara Update 1 softver, a sadrži pristupnik za mrežno sučelje osim podataka 0, možete primijeniti Update 1.2 u sljedeća dva načina:

- **Mogućnost 1**: preuzmite i primijenite ga pomoću na `Start-HcsHotfix` cmdlet putem sučelja komponente Windows PowerShell uređaja. To je preporučeni način. **Ovaj postupak koristite da biste primijenili Update 1.2 ako vaš uređaj radi ažuriranja 1.0 ili ažuriranje 1.1.**

- **Mogućnost 2**: Uklanjanje konfiguracije pristupnika i instalirajte ažuriranje izravno na portalu Azure klasični.


Detaljne upute za svaku od njih isporučuju se u sljedećim odjeljcima.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>Mogućnost 1: Korištenje komponente Windows PowerShell za StorSimple da biste primijenili Update 1.2 kao hitni popravak

Koristite ovaj postupak samo ako su pokrenuti ažuriranje 0,1, 0,2, 0,3 i ako vaš Provjera pristupnik nije uspjela prilikom instalacije ažuriranja s portala za Azure klasični. Ako ste instalirali softver izdanje (GA), imajte [Microsoftovoj službi za podršku](storsimple-contact-microsoft-support.md) da biste ažurirali uređaj.

Da biste instalirali Update 1.2 kao hitni popravak, morate preuzmete i instalirate hitnih sljedeće:

| Redoslijed  | KB        | Opis             | Vrsta ažuriranja  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3063418 | Ažuriranje softvera         |  Uobičajeni     |
| 2      | KB3043005 | Ažuriranje kontroler LSI SAS |  Uobičajeni     |
| 3      | KB3063416 | Na disku opreme           | Održavanje  |

Prije nego počnete koristiti ovaj postupak da biste primijenili ažuriranja, provjerite:

- Oba kontrolera uređaja koji su na mreži.

Izvršite sljedeće korake da biste primijenili Update 1.2. **Ažuriranja može potrajati oko dva sata da biste dovršili (približno 30 minuta za softver, 30 minuta za upravljački program, 45 minuta za firmver disk).**

[AZURE.INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]


## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>Mogućnost 2: Pomoću portala za Azure klasični da biste primijenili Update 1.2 nakon uklanjanja konfiguracije pristupnika

Ovaj postupak primjenjuje samo na uređajima StorSimple koji koriste verziju softvera prije no što Update 1, a imate pristupnik postavljena na mrežnom sučelju osim podataka 0. Morat ćete isključite postavku pristupnika prije primjene ažuriranja.

Ažuriranje može potrajati nekoliko sati. Ako vaš hosts u različitim podmreže, uklanjanjem konfiguraciju pristupnika na sučelja iSCSI može rezultirati isključiti. Preporučujemo da konfigurirate podataka 0 za iSCSI promet da biste smanjili na nedostupnost.

Izvršite sljedeće korake da biste onemogućili sučelje mreže s pristupnika, a zatim primijenite ažuriranje.

[AZURE.INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]


## <a name="next-steps"></a>Daljnji koraci

Saznajte više o [izdanju Update 1.2](storsimple-update1-release-notes.md).
