<properties
   pageTitle="Instalirajte ažuriranje 2 na uređaju StorSimple | Microsoft Azure"
   description="U članku se objašnjava kako instalirati StorSimple 8000 niz ažuriranju 2 na uređaju StorSimple 8000 niz."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="install-update-2-on-your-storsimple-device"></a>Instalirajte ažuriranje 2 na uređaju StorSimple

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča objašnjava kako instalirati ažuriranje 2 na uređaju StorSimple izvodi starije verzije softvera putem portala za Azure klasični. Vodič obuhvaća i koraci potrebni za ažuriranje kada pristupnik konfiguriran na mrežnom sučelju osim podataka 0 StorSimple uređaja, a pokušavate da biste ažurirali iz verzije softver 1 prije ažuriranja.

Ažuriranje 2 obuhvaća uređaj softverska ažuriranja, LSI ažuriranja za upravljačke programe i ažuriranja opreme disk. Softver uređaja i ažuriranja LSI koje nisu disruptive ažuriranja i mogu primijeniti putem Azure klasični portal. Ažuriranja opreme disk disruptive ažuriranja, a možete primijeniti samo putem sučelja komponente Windows PowerShell uređaja.

> [AZURE.IMPORTANT]

> -  Možda nećete vidjeti ažuriranju 2 odmah jer moramo fazama uvođenje ažuriranja. Traženje ažuriranja u nekoliko dana ponovno kao to ažuriranje će postati dostupne uskoro.
> - Skup ručno i automatsko prije provjere gotovi prije instalacije radi određivanja stanja uređaj pomoću hardvera stanje i mrežne veze. Te su prije provjere se izvode samo ako Primjena ažuriranja na portalu Azure klasični.
> - Preporučujemo da instalirate ažuriranja za softver i upravljački program putem portala za Azure klasični. Trebali biste samo otvorite sučelja komponente Windows PowerShell uređaja (za instaliranje ažuriranja) ako ne prođu provjeru stara ažuriranje pristupnika na portalu. Ažuriranja može potrajati sati 4-7 da biste instalirali (uključujući ažuriranja za Windows). Način ažuriranja održavanja mora biti instaliran putem sučelja komponente Windows PowerShell uređaja. Kao što su ažuriranja način održavanja disruptive ažuriranja, to će rezultirati jedan prema dolje za svoj uređaj.
> - Ako je pokrenut upravitelju neobavezno StorSimple snimku, provjerite je li da ste nadogradili svoju verziju snimke Upravitelj 2 ažuriranje prije no što ažuriranje uređaj.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-the-azure-classic-portal"></a>Instalirajte ažuriranje 2 putem portala za Azure klasični

Izvršite sljedeće korake da biste ažurirali uređaj da biste [ažurirali 2](storsimple-update2-release-notes.md).


> [AZURE.NOTE]
Ažuriranje 2 Microsoftu povući dodatna dijagnostičke informacije s uređaja. Zbog toga kada naš tim za operacije prepozna uređaje koji se pojave problemi, nismo bolje posebnog za prikupljanje informacija s uređaja i otklanjanja poteškoća. Tako da prihvaća ažuriranju 2, dopustite nam ovaj određene proaktivne podršku.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Provjerite je li uređaj je pokrenut **StorSimple 8000 niz ažuriranju 2 (6.3.9600.17673)**. **Zadnje ažuriranje datuma** moraju se i mijenjati. Vidjet ćete i dostupnosti održavanja način ažuriranja (Ova poruka mogu nastaviti da se prikazuje u roku od 24 sata kada instalirate ažuriranja).

    Održavanje način ažuriranja su disruptive ažuriranja koja rezultiraju nedostupnost uređaja i se mogu primijeniti samo putem sučelja komponente Windows PowerShell uređaja. U nekim slučajevima kad se izvode 1.2 ažuriranje opreme disk možda već ažuran, u tom slučaju morate instalirati ažuriranja način održavanja.

13. Preuzmite način ažuriranja održavanja slijedeći korake navedene u [Da biste preuzeli hitnih](#to-download-hotfixes) da biste potražili i preuzimanje KB3121899, koji instalira disk ažuriranja opreme (ostala ažuriranja mora biti instalirao odmah).

13. Slijedite korake navedene u [instalirati i provjerite je li hitnih način održavanja](#to-install-and-verify-maintenance-mode-hotfixes) instalirati ažuriranja za održavanje načinu rada.


## <a name="install-update-2-as-a-hotfix"></a>Instalirajte ažuriranje 2 kao hitni popravak

Ovaj postupak koristite ako neće uspjeti potvrdite pristupnika prilikom instalacije ažuriranja putem portala za Azure klasični. Imate pristupnik dodijeljene 0 mrežno sučelje koje nisu podataka i uređaju sa verzijom softver prije no što Update 1, ne prođu provjeru.

Verzije softver koju je moguće nadograditi metodom hitni popravak su ažuriranje 0,1, ažuriranje 0,2, i ažuriranje 0,3, Update 1, 1.1 ažuriranja i ažuriranje 1.2. Način hitni popravak obuhvaća sljedeća tri koraka:

- Preuzmite hitnih iz kataloga Microsoft Update.
- Instalirajte i provjerite je li hitnih običan način rada.
- Instalirajte i provjerite je li hitni popravak način rada za održavanje.

Da biste instalirali ažuriranju 2 kao hitni popravak, morate preuzmete i instalirate hitnih sljedeće:

| Redoslijed  | IZ BAZE ZNANJA        | Opis                    | Vrsta ažuriranja  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3121901 | Ažuriranje softvera         |  Uobičajeni     |
| 2      | KB3121900 | Upravljački program za LSI              |  Uobičajeni     |
| 3      | KB3080728 | Popravak Storport </br> Windows Server 2012 R2 |  Uobičajeni     |
| 4      | KB3090322 | Popravak spaceport </br> Windows Server 2012 R2 |  Uobičajeni     |
| 5      | KB3121899 | Na disku opreme           | Održavanje  |


> [AZURE.IMPORTANT]
>
> - Ako je uređaju instaliran izdanje (GA) verzija, obratite se [Microsoftovoj službi za podršku](storsimple-contact-microsoft-support.md) radi jednostavnijeg ažuriranja.
> - Ovaj postupak potrebno izvršiti samo jedanput da biste primijenili ažuriranju 2. Pomoću portala za Azure klasični sljedeće ažuriranje.
> - Svaka instalacija hitni popravak može potrajati otprilike 20 minuta da biste dovršili. Ukupna Instaliraj vrijeme je blizu dva sata.
> - Prije nego počnete koristiti ovaj postupak da biste primijenili ažuriranja, provjerite je li oba kontrolera uređaja koji su na mreži i su sve komponente za hardver dobar.

Izvršite sljedeće korake da biste primijenili to ažuriranje kao hitni popravak.

[AZURE.INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]



## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [ažuriranju 2 izdanje](storsimple-update2-release-notes.md).
