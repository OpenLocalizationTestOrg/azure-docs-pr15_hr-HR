<properties
   pageTitle="Ažuriranje uređaju StorSimple | Microsoft Azure"
   description="U članku se objašnjava kako koristiti značajku StorSimple update da biste instalirali običnu i održavanje način ažuriranja i hitnih."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="06/28/2016"
   ms.author="v-sharos" />

# <a name="update-your-storsimple-8000-series-device"></a>Ažuriranje uređaju StorSimple 8000 niza

## <a name="overview"></a>Pregled

Značajke ažuriranja StorSimple omogućuju jednostavno ažurnima StorSimple uređaj. Ovisno o vrsti ažuriranja, možete primijeniti ažuriranja na uređaju putem portala za Azure klasični ili putem sučelja komponente Windows PowerShell. Pomoću ovog praktičnog vodiča opisuju vrste ažuriranja i kako se instalira svaku od njih.

Možete primijeniti dvije vrste uređaja ažuriranja: 

- Uobičajeni (ili običnom načinu rada) ažuriranja
- Održavanje način ažuriranja

Možete instalirati običnog ažuriranja putem Azure klasični portal ili komponente Windows PowerShell; Međutim, morate koristiti komponente Windows PowerShell za instalaciju ažuriranja način održavanja. 

Svaka vrsta ažuriranja je opisana zasebno, ispod.

### <a name="regular-updates"></a>Uobičajeni ažuriranja

Uobičajeni ažuriranjima koje nisu disruptive ažuriranja koja je moguće je instalirati kada uređaj u običnom načinu rada. Sljedeća ažuriranja primjenjuju se putem web-mjestu Microsoft Update svaki kontrolerom uređaja. 

> [AZURE.IMPORTANT] Kontroler prebacivanje može doći do tijekom postupka ažuriranja. Međutim, to neće utjecati na dostupnost sustava ili postupak.

- Detalje o instalacija pravilnim ažuriranja putem portala za Azure klasični potražite u članku [pravilnim ažuriranja za instalaciju putem Azure klasični portal(#install-regular-updates-via-the-azure-classic-portal).

- Možete instalirati i obične ažuriranja putem komponente Windows PowerShell za StorSimple. Detalje potražite u članku [Instaliranje pravilnim ažuriranja putem komponente Windows PowerShell za StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Održavanje način ažuriranja

Održavanje način ažuriranja su disruptive ažuriranja kao što su nadogradnje firmver disk. Sljedeća ažuriranja potrebna staviti u načinu za održavanje uređaja. Detalje potražite u članku [Korak 2: Unesite održavanja](#step2). Azure klasični portal ne možete koristiti da biste instalirali održavanja način ažuriranja. Umjesto toga, morate koristiti Windows PowerShell za StorSimple. 

Informacije o instaliranju održavanja način ažuriranja potražite u članku [Instalacija održavanja način ažuriranja putem komponente Windows PowerShell za StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [AZURE.IMPORTANT] Održavanje način ažuriranja morate zatvoriti odvojeno svaki kontroler. 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Uobičajeni ažuriranja putem portala za Azure klasični

Azure klasični portal možete koristiti da biste primijenili ažuriranja StorSimple uređaj.

[AZURE.INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Instalacija ažuriranja običnog putem komponente Windows PowerShell za StorSimple

Osim toga, možete koristiti Windows PowerShell za StorSimple da biste primijenili ažuriranja običnu (običan način rada).

> [AZURE.IMPORTANT] Iako možete instalirati običnog ažuriranja pomoću komponente Windows PowerShell za StorSimple, preporučujemo da instalirate običnog ažuriranja putem portala za Azure klasični. Počevši od Update 1, stara provjere provest će se prije instalacije ažuriranja na portalu. Te prije provjere će preempt pogrešaka i provjerite jednostavnije sučelje. 

[AZURE.INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Instalacija ažuriranja način održavanja putem komponente Windows PowerShell za StorSimple

Pomoću komponente Windows PowerShell za StorSimple ažuriranje održavanja način rada s uređajem StorSimple. Sve zahtjeve za/i pauzirana u tom načinu rada. Servise kao što je koji nije uklonjiv radnom memorijom (NVRAM) ili klasteriranja servisa i zaustaviti. Oba kontrolera su sustava kada se uključite ili izlazak iz njega u tom načinu rada. Nakon što izađete taj način, sve servise će nastaviti i mora biti dobar. (To može potrajati nekoliko minuta.)

Ako je potrebno primijeniti održavanja način ažuriranja će primati upozorenja putem portala za Azure klasični imate ažuriranja koja je potrebno je instalirati. Upozorenje neće sadržavati upute za korištenje komponente Windows PowerShell za StorSimple instalirati ažuriranja. Nakon što ažurirate uređaju, koristite isti postupak da biste promijenili uređaj na uobičajeni način. Detaljne upute potražite u članku [koraku 4: održavanje izlaz](#step4).

> [AZURE.IMPORTANT] 
> 
> - Prije nego što unesete održavanja, provjerite jesu li oba kontrolera uređaj dobar tako da Provjera **Stanja hardver** na stranice za **Održavanje** Azure klasični portalu. Ako je kontrolerom dobar, obratite se Microsoft Support za sljedeće korake. Dodatne informacije potražite u članku kontakt Microsoftovoj podršci. 
> - Kada ste u načinu održavanja, morate primijeniti ažuriranje na jedan kontroler, a zatim na s kontrolerom.

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Korak 1: Povezivanje s konzole za serijski<a name="step1">

Najprije pomoću aplikacija kao što su PuTTY da biste pristupili konzole za serijski. Sljedeći postupak objašnjava kako koristiti PuTTY za povezivanje s konzole za serijski.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Korak 2: Unesite održavanja<a name="step2">

Kada se povežete s konzole, određuju ima li ažuriranja za instalaciju i unesite održavanja da biste ih instalirali.

[AZURE.INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Korak 3: Instalirajte ažuriranja<a name="step3">

Nakon toga instalirati ažuriranja.

[AZURE.INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]
 
### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Korak 4: Izlaz održavanja<a name="step4">

Na kraju, zatvorite održavanja.

[AZURE.INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Instalacija hitnih putem komponente Windows PowerShell za StorSimple

Za razliku od ažuriranja za Microsoft Azure StorSimple hitnih instaliraju iz zajedničke mape. Kao i kod ažuriranja, postoje dvije vrste hitnih: 

- Uobičajeni hitnih 
- Hitne način održavanja  

Sljedeći postupci objašnjavaju kako koristiti Windows PowerShell za StorSimple da biste instalirali običnu i održavanje način hitnih.

[AZURE.INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[AZURE.INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Što se događa ažuriranja Ako započnete tvorničke Vrati izvorne postavke uređaja?

Ako je uređaj vratiti na tvorničke postavke, zatim sva ažuriranja gube. Kada uređaj tvorničke Vrati je registrirana i konfigurirali, morat ćete ručno instaliranje ažuriranja putem Azure klasični portal i/ili Windows PowerShell za StorSimple. Dodatne informacije o tvorničke Vrati potražite u članku [izvorne postavke uređaja na tvorničke postavke](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o [korištenju komponente Windows PowerShell za StorSimple za administriranje StorSimple uređaj](storsimple-windows-powershell-administration.md).
- Dodatne informacije o [korištenju StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).
