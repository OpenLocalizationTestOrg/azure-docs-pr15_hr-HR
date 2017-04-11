<properties
   pageTitle="Instalacija ažuriranja 2.2 na uređaju StorSimple | Microsoft Azure"
   description="U članku se objašnjava kako instalirati StorSimple 8000 niz ažuriranje 2.2 na uređaju StorSimple 8000 niz."
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
   ms.date="08/02/2016"
   ms.author="alkohli" />

# <a name="install-update-22-on-your-storsimple-device"></a>Instaliranje ažuriranja 2.2 na uređaj StorSimple

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča objašnjava kako instalirati ažuriranje 2.2 na uređaj s StorSimple pokrenut starije verzije softvera putem portala za Azure klasični, a metodom hitni popravak. Način hitni popravak koristi se kada pristupnik konfiguriran na mrežnom sučelju osim podataka 0 StorSimple uređaja i pokušavate da biste ažurirali iz verzije softver 1 prije ažuriranja.

Ažuriranje 2.2 obuhvaća softver uređaja, WMI i iSCSI ažuriranja. Ako ažuriranjem iz verzija 2.1, samo uređaj ažuriranje softvera morat ćete zatvoriti. Ako ažuriranjem iz verzije 2 prije ažuriranja, koje se i potreban da biste primijenili LSI upravljački program, Spaceport, Storport i ažuriranja opreme disk. Na uređaju softver, WMI, iSCSI, LSI upravljački program, Spaceport i Storport rješenja koje nisu disruptive ažuriranja i mogu primijeniti putem portala za Azure klasični. Ažuriranja opreme disk disruptive ažuriranja, a možete primijeniti samo putem sučelja komponente Windows PowerShell uređaja. 

> [AZURE.IMPORTANT]

> - Skup ručno i automatsko prije provjere gotovi prije instalacije radi određivanja stanja uređaj pomoću hardvera stanje i mrežne veze. Ove stara provjerava se izvode samo ako Primjena ažuriranja na portalu Azure klasični.
> - Preporučujemo da instalirate ažuriranja za softver i upravljački program putem portala za Azure klasični. Trebali biste samo otvorite sučelja komponente Windows PowerShell uređaja (za instaliranje ažuriranja) ako ne prođu provjeru stara ažuriranje pristupnika na portalu. Ovisno o verziji ažurirate iz, ažuriranja može potrajati 1,5 2,5 sati da biste instalirali. Način ažuriranja održavanja mora biti instaliran putem sučelja komponente Windows PowerShell uređaja. Kao što su ažuriranja način održavanja disruptive ažuriranja, to će rezultirati dolje vremena za svoj uređaj.
> - Ako je pokrenut upravitelju neobavezno StorSimple snimku, provjerite je li da ste nadogradili svoju verziju snimke upravitelja za ažuriranje 2.2 prije no što ažuriranje uređaj.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-22-via-the-azure-classic-portal"></a>Instalacija ažuriranja 2.2 putem portala za Azure klasični

Izvršite sljedeće korake da biste ažurirali uređaj da biste [ažurirali 2.2](storsimple-update21-release-notes.md).


> [AZURE.NOTE]
Ako primjenjujete ažuriranju 2 ili kasnije (uključujući ažuriranje 2,1), Microsoft će moći povući dodatna dijagnostičke informacije s uređaja. Zbog toga kada naš tim za operacije prepozna uređaje koji se pojave problemi, nismo bolje posebnog za prikupljanje informacija s uređaja i otklanjanja poteškoća. Tako da prihvaća ažuriranje 2 ili novije dopustite nam ovaj određene proaktivne podršku.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Provjerite je li uređaj je pokrenut **StorSimple 8000 niz ažuriranje 2.2 (6.3.9600.17708)**. **Zadnje ažuriranje datuma** moraju se i mijenjati. 

    Ako ažurirate iz verzije prije ažuriranju 2, vidjet ćete i način ažuriranja održavanja dostupnih (Ova poruka mogu nastaviti da se prikazuje u roku od 24 sata kada instalirate ažuriranja).

    Održavanje način ažuriranja su disruptive ažuriranja koja rezultiraju nedostupnost uređaja i se mogu primijeniti samo putem sučelja komponente Windows PowerShell uređaja. U nekim slučajevima kad se izvode 1.2 ažuriranje opreme disk možda već ažuran, u tom slučaju morate instalirati ažuriranja način održavanja.

    Ako ažurirate iz ažuriranju 2, uređaj treba sada ažurni. Možete preskočiti preostale korake.

13. Preuzmite način ažuriranja održavanja slijedeći korake navedene u [Da biste preuzeli hitnih](#to-download-hotfixes) da biste potražili i preuzimanje KB3121899, koji instalira disk ažuriranja opreme (ostala ažuriranja mora biti instalirao odmah).

13. Slijedite korake navedene u [instalirati i provjerite je li hitnih način održavanja](#to-install-and-verify-maintenance-mode-hotfixes) instalirati ažuriranja za održavanje načinu rada. 

  

## <a name="install-update-22-as-a-hotfix"></a>Instalacija ažuriranja 2.2 kao hitni popravak

Ovaj postupak koristite ako neće uspjeti potvrdite pristupnika prilikom instalacije ažuriranja putem portala za Azure klasični. Imate pristupnik dodijeljene 0 mrežno sučelje koje nisu podataka i uređaju sa verzijom softver prije no što Update 1, ne prođu provjeru.

Verzija softvera koju je moguće nadograditi metodom hitni popravak su:

- Ažuriranje 0,1, 0,2, a zatim 0,3
- Ažuriranje 1, 1.1, 1.2
- Ažuriranje 2, 2.1 

> [AZURE.IMPORTANT]
>
> - Ako vaš uređaj radi izdanje (GA) verzija, obratite se [Microsoftovoj službi za podršku](storsimple-contact-microsoft-support.md) radi jednostavnijeg ažuriranja.

Način hitni popravak obuhvaća sljedeća tri koraka:

- Preuzmite hitnih iz kataloga Microsoft Update.
- Instalirajte i provjerite je li hitnih običan način rada.
- Instalacija i provjerite hitni popravak za održavanje načinu rada (samo kada ažurirate od 2 softvera stara ažuriranje).

#### <a name="download-updates-for-a-device-running-update-21-software"></a>Preuzimanje ažuriranja za uređaj pokrenut softver 2.1 za ažuriranje

**Ako je na uređaju instaliran ažuriranje 2.1**, morate preuzeti samo uređaj softver ažuriranje KB3179904. Instalirajte samo binarne datoteke prefaced s "sve hcsmdssoftwareudpate". Instalacija na Cis i ažuriranje agent strategiju minimalnog preuzimanja prefaced s `all-cismdsagentupdatebundle`. Pogreške rezultirat će pogrešku da biste to učinili. Ovo je koji nisu disruptive ažuriranja, IO će neće biti nadziranje prekinuto i uređaj neće imati sve isključiti.


#### <a name="download-updates-for-a-device-running-update-2-software"></a>Preuzimanje ažuriranja za uređaj pokrenut softver ažuriranju 2

**Ako je na uređaju instaliran ažuriranju 2**, morate preuzeti i instalirati sljedeće hitnih propisanim redoslijedom:

| Redoslijed  | KB        | Opis                    | Vrsta ažuriranja  | Put instalirati |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3179904 | Ažuriranje softvera i #42;  |  Uobičajeni <br></br>Osobe koje nisu disruptive     | ~ 45 minuta |
| 2.      | KB3146621 | iSCSI paketa | Uobičajeni <br></br>Osobe koje nisu disruptive  | ~ 20 minuta |
| 3.      | KB3103616 | WMI paketa |  Uobičajeni <br></br>Osobe koje nisu disruptive      | ~ 12 minuta |


 & #42;  Ažuriranje *bilješke, softver sastoji se od dva binarne datoteke: ažuriranje programa uređaj prefaced s `all-hcsmdssoftwareupdate` i Cis i strategiju minimalnog preuzimanja agent prefaced s `all-cismdsagentupdatebundle`. Ažuriranje softvera uređaj mora biti instaliran prije agent Cis i strategiju minimalnog preuzimanja. Ponovno pokrenite i aktivni kontroler putem na `Restart-HcsController` ažuriranje cmdlet nakon primjene agent Cis i strategiju minimalnog preuzimanja (i prije primjene preostalih ažurira).* 

#### <a name="download-updates-for-a-device-running-pre-update-2-software"></a>Preuzimanje ažuriranja za uređaj pokretanjem stara ažuriranje 2 softvera

**Ako je na uređaju instaliran verzije 0,2, 0,3, 1.0 i 1.1**, morate preuzeti i upravljački program za instalaciju na LSI i opreme ažuriranje uz softver, iSCSI i WMI ažuriranja. To ažuriranje već je instalirana ako su pokrenuti ažuriranje 1.2 ili 2. 
 
| Redoslijed  | KB        | Opis                    | Vrsta ažuriranja  | Put instalirati |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3121900 | Upravljački program za LSI i opreme             |  Uobičajeni <br></br>Osobe koje nisu disruptive      | ~ 20 minuta |


<br></br>
**Ako je na uređaju instaliran verzije 0,2, 0,3, 1.0, 1.1 i 1.2**, morate preuzeti i instalirati na Spaceport i popravak Storport. Te su već instalirani ako su pokrenuti ažuriranju 2.

| Redoslijed  | KB        | Opis                    | Vrsta ažuriranja  | Put instalirati |
|--------|-----------|-------------------------|------------- |-------------|
| 5.      | KB3090322 | Popravak spaceport </br> Windows Server 2012 R2 |  Uobičajeni <br></br>Osobe koje nisu disruptive      | ~ 20 minuta |
| 6.      | KB3080728 | Popravak Storport </br> Windows Server 2012 R2 |  Uobičajeni <br></br>Osobe koje nisu disruptive      | ~ 20 minuta |



<br></br>
Možda ćete morati instalirati ažuriranja opreme disk. Možete provjeriti tako da pokrenete trebate ažuriranja opreme disk na `Get-HcsFirmwareVersion` cmdlet. Ako koristite sljedeće verzije firmver: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, a zatim ne morate instalirati sljedeća ažuriranja.


| Redoslijed  | KB        | Opis                    | Vrsta ažuriranja  | Put instalirati |
|--------|-----------|-------------------------|------------- |-------------|
| 7.      | KB3121899 | Na disku opreme              |  Održavanje <br></br>Disruptive      | ~ 30 minuta |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Ovaj postupak potrebno izvršiti samo jedanput da biste primijenili ažuriranja 2.2. Pomoću portala za Azure klasični sljedeće ažuriranje.
> - Ako ažuriranjem iz ažuriranju 2, vrijeme ukupni Instaliraj je blizu 1,5 sati.
> - Prije nego počnete koristiti ovaj postupak da biste primijenili ažuriranja, provjerite je li da kontrolera uređaja koji su na mreži i su sve komponente za hardver dobar.

Izvršite sljedeće korake da biste preuzeli i instalirali hitnih.

[AZURE.INCLUDE [storsimple-install-update21-hotfix](../../includes/storsimple-install-update21-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Daljnji koraci

Saznajte više o [izdanju 2.1 ažuriranje](storsimple-update21-release-notes.md).
