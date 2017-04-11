<properties
    pageTitle="Replicirati lokalnog VMware virtualnim strojevima ili fizičke poslužitelji za sekundarnog web-mjesta | Microsoft Azure"
    description="Pomoću ovog članka za replikaciju VMware VMs ili Windows/Linux fizičke poslužitelje sekundarne web-mjesto s oporavak Azure web-mjesta."
    services="site-recovery"
    documentationCenter=""
    authors="nsoneji"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="nisoneji"/>


# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Replicirati lokalnog VMware virtualnim strojevima ili fizičke poslužitelje sekundarnog web-mjesta


## <a name="overview"></a>Pregled

InMage Scout u oporavak Azure web-mjesta omogućuje u stvarnom vremenu replikacije između lokalnog VMware web-mjesta. InMage Scout je sve obuhvaćeno pretplate servisa Azure oporavak web-mjesta.


## <a name="prerequisites"></a>Preduvjeti

**Račun za Azure**: potreban vam je račun za [Microsoft Azure](https://azure.microsoft.com/) . Možete početi s [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). [Dodatne informacije](https://azure.microsoft.com/pricing/details/site-recovery/) o cijenama oporavak web-mjesta.


## <a name="step-1-create-a-vault"></a>Korak 1: Stvaranje na zbirke ključeva
1. Prijavite se na [portal za Azure](https://portal.azure.com).
2. Kliknite Novo > Upravljanje > sigurnosno kopiranje i vraćanje web-mjesta (OMS). Osim toga, možete kliknuti Pregledaj > oporavak servisa sigurnog > Dodaj.
3. U odjeljku **naziv** navedite neslužbeni naziv da biste odredili na zbirke ključeva. Ako imate više pretplata, odaberite jedan od njih.
4. U **grupi resursa** stvorite novu grupu resursa ili odaberite postojeći. Navedite Azure područja da biste dovršili obavezna polja. 
5.  **Mjesto**, odaberite regiji u zbirke ključeva. Da biste provjerili podržanih regija, potražite u članku [Azure web-mjesta oporavak cijene](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Ako želite brzo pristupiti na sigurnog na nadzornoj ploči kliknite Prikvači na nadzornu ploču, a zatim kliknite Stvori.
6. Nove zbirke ključeva pojavit će se na nadzornoj ploči > sve resurse i na glavnom servisima za oporavak sefovi plohu.

## <a name="step-2-configure-the-vault-and-download-inmage-scout-components"></a>Korak 2: Konfiguriranje na sigurnog i preuzeti InMage Scout komponente
7. Oporavak Services sefovi plohu odaberite vaše zbirke ključeva, a zatim postavke.
8. U odjeljku **Postavke** > **Početak rada** kliknite **Oporavak web-mjesta** > korak 1: **Priprema infrastrukture** > **zaštitu cilj**.
9. U **cilju zaštite** odaberite oporavak web-mjesta pa odaberite da, s VMware vSphere Hypervisor. Zatim kliknite u redu.
10. U **Scout postavljanje**, kliknite preuzimanje da biste preuzeli GA InMage Scout 8.0.1 softver i registracija ključ. Postavljanje datoteke za sve potrebne komponente su u .zip preuzete datoteke.


## <a name="step-3-install-component-updates"></a>Korak 3: Instalirajte ažuriranja komponente

Saznajte više o najnovijim [ažuriranjima](#updates). Ažuriranje datoteka na poslužiteljima ćete instalirajte sljedećim redoslijedom:

1. Poslužitelj RX ako postoji
2. Konfiguracija poslužitelja
3. Postupak poslužitelja
3. Glavni ciljnim poslužiteljima
4. vContinuum poslužitelja
5. Izvorni poslužitelj (Windows i Linux Server)

Instalirajte ažuriranja na sljedeći način:

1. Preuzmite [Ažuriranje](https://aka.ms/asr-scout-update4) .zip datoteku. U ovom .zip datoteka sadrži sljedeće datoteke:

    - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz
    - CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
    - UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe
    - UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
    - vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe
    - UA update4 bitova za RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz 
    
2. Izdvajanje .zip datoteke.<br>
3. **Poslužitelj za RX**: kopiranje **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** s poslužiteljem RX i izdvojite ga. U mapi izdvojene pokrenuti **Instaliraj**.<br>
4. **Za poslužitelj za konfiguraciju poslužiteljski proces**: kopiranje **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** na poslužitelj za konfiguraciju i poslužitelj za postupak. Pokrenite je dvoklikom.<br>
5. **Za Windows osnovnih poslužitelju ciljni**: da biste ažurirali agent za Sjedinjeno komuniciranje, kopirajte **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** osnovne odredišni poslužitelj. Dvokliknite da biste ga pokrenuti. Imajte na umu da agent za Sjedinjeno komuniciranje je primjenjivo s izvorišnim poslužiteljem. Instalirajte ga na izvornom poslužitelju kao dobro, kao što je rečeno kasnije u ovom popisu.<br>
7. **Za poslužitelj vContinuum**: kopiranje **vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe** vContinuum poslužitelj.  Pripazite da ne zatvorite čarobnjak za vContinuum. Dvokliknite datoteku da biste ga pokrenuti.<br>
8. **Za Linux osnovne odredišni poslužitelj**: da biste ažurirali agent za Sjedinjeno komuniciranje, kopirajte **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** u glavni odredišni poslužitelj i izdvojite ga. U mapi izdvojene pokrenuti **Instaliraj**.<br>
9. **Za Windows izvora poslužitelja**: da biste ažurirali agent za Sjedinjeno komuniciranje, kopirajte **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** s izvorišnim poslužiteljem. Dvokliknite da biste ga pokrenuti.<br>
10. **Izvorni poslužitelj za Linux**: da biste ažurirali agent za Sjedinjeno komuniciranje, kopirajte odgovarajući verziju datoteke UA Linux poslužitelj i izdvojite ga. U mapi izdvojene pokrenuti **Instaliraj**.  Primjer: RHEL 6.7 64 bitnu poslužitelja, kopirajte **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** na poslužitelj i izdvojite ga. U mapi izdvojene pokrenuti **Instaliraj**.

## <a name="step-4-set-up-replication"></a>Korak 4: Postavljanje replikacije
1. Postavljanje ponavljanja između izvorišnog web-mjesta i ciljnog VMware web-mjesta.
2. Upute, koristite InMage Scout dokumentaciju koja se preuzima s proizvodom. Osim toga, u dokumentaciji možete pristupiti na sljedeći način:

    - [Napomene](https://aka.ms/asr-scout-release-notes)
    - [Matrica kompatibilnosti](https://aka.ms/asr-scout-cm)
    - [Vodič za korisnika](https://aka.ms/asr-scout-user-guide)
    - [RX korisničkom priručniku](https://aka.ms/asr-scout-rx-user-guide)
    - [Vodič za brzi instalacije](https://aka.ms/asr-scout-quick-install-guide)


## <a name="updates"></a>Ažuriranja

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure web-mjesta oporavak Scout 8.0.1 ažuriranje 4
Ažuriranje scout 4 je kumulativnim ažuriranjem. Ima ispravaka update1 prije pomaka update3 i sljedeće nove popravci programskih pogrešaka i poboljšanja.

**Nova podrška za platforme** 

- Podrška za dodan je za vCenter/vSphere 6.0, 6.1 i 6.2
- Podrška za dodan je za sljedeće Linux operacijski sustavi
    - Crvena je vaša Enterprise Linux (RHEL) 7.0, 7.1 i 7,2 
    - CentOS 7.0, 7.1 i 7,2
    - Crvena je vaša Enterprise Linux (RHEL) 6.8
    - CentOS 6.8

>[AZURE.NOTE]
>
> RHEL/CentOS 7 64-bitni **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** je isporučen osnovni Scout GA paket **InMage_Scout_Standard_8.0.1 GA.zip**. Preuzmite paket Scout GA s portala kao što je rečeno [Korak 1](site-recovery-vmware-to-vmware.md#Step 1: Create a vault).

**Popravci programskih pogrešaka i poboljšanja** 

- Poboljšani zatvaranja zadužen za sljedeće operacijskim sustavom Linux i clones sprječavanju neželjene ponovno sinkronizirati problema s.
    - Crvena je vaša Enterprise Linux (RHEL) 6.x
    - Oracle Linux (OL) 6.x
- Za Linux, dovršite pristup mapi dozvole u direktoriju instalacije agent za Sjedinjeno komuniciranje sada su ograničeni samo na lokalni korisnik.
- U sustavu Windows prekoračenja problem prilikom izdavanja uobičajenih raspodijeljeno dosljednost Knjižna oznaka na intenzivnog učitati raspodijeljeno aplikacije kao što su klastere SQL točke i zajedničko korištenje.
- Dodane zapisnika povezane popravak u CX osnovni installer.
- Veza za preuzimanje vCLI 6.0 VMware dodaje se osnovni instalacijskog programa Windows matrica cilj.
- Dodaje više provjerava i zapisnicima promjene konfiguracije mreže tijekom prebacivanje i DR bušilice.
- Ponekad zadržavanja podataka nije prijavljen da biste na CX.  
- Količinsko Promijeni veličinu postupak pomoću čarobnjaka za vContinuum je za fizičke klaster ne uspijeva kada se dogodilo Smanjivanje glasnoće izvora.
- Zaštita klaster nije uspjela uz poruku o pogrešci "Nije pronađeno potpis na disku" kada je grupnom disku PRDM disk.
- cxps prijenos poslužitelja rušenje zbog iznimke izvan raspona. 
- Naziv poslužitelja i IP stupaca sada su mijenjati u automatske instalacije stranica čarobnjaka za vContinuum.
- Poboljšanja RX API-JA
    - Nudi pet najnovije dostupne uobičajenih dosljednost točke (samo zajamčena oznake).
    - Nudi kapacitetu i Detalji slobodnog prostora za sve zaštićene uređaje.
    - Omogućuje Scout upravljački program stanje na izvorni poslužitelj. 
    
>[AZURE.NOTE] 
>
>- Osnovni paket **InMage_Scout_Standard_8.0.1_GA.zip** sada je ažurirao CX osnovni installer **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** i Windows matrica cilj osnovni installer **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Za sve nove instalaciju pomoću nove CX i Windows matrica cilj GA bitova.
>- Ažuriranje 4 se izravno primjenjuju na 8.0.1 ZŽ.
>- Poslužitelj za konfiguraciju i RX ažuriranja ne može biti vraćeni natrag nakon mogu se primijeniti na sustav.

### <a name="azure-site-recovery-scout-801-update-3"></a>Ažuriranje Scout 8.0.1 oporavak Azure web-mjesta 3
Ažuriranje 3 obuhvaća sljedeće popravci programskih pogrešaka i poboljšanja:

- Poslužitelj za konfiguraciju i RX uspjeti da biste registrirali sigurnog oporavak web-mjesta kad postanu iza proxy poslužitelj.
- Broj sati za oporavak točke cilj (RPO) ne ispunjava kriterij početak ne ažurira se u izvješće o stanju.
- Poslužitelj za konfiguraciju se sinkronizira s RX kada ESX hardver pojedinosti ili mreže pojedinosti sadrže znakove UTF-8.
- Kontrolera domena u sustavu Windows Server 2008 R2 se neće pokrenuti nakon oporavka.
- Izvanmrežna sinkronizacija ne funkcionira prema očekivanjima.
- Nakon prebacivanje virtualnog računala (VM) replikacije par brisanja zapne u korisničkom Sučelju CX na dulje vrijeme, a korisnici ne mogu dovršiti u failback ili nastavili postupak.
- Ukupni snimke operacije koje obavljaju posao dosljednost ste optimizirana pomažu smanjenju aplikacije prekida vezu kao što je SQL klijente.
- Performanse dosljednost alat (VACP.exe) poboljšan smanjivanjem memorije nužan za stvaranje snimki u sustavu Windows.
- Na automatske instalirajte servisa ruši prilikom lozinka je veće od 16 znakova.
- vContinuum je provjera i upita za novu vjerodajnice vCenter kada mijenjaju vjerodajnice.
- Na Linux, predmemorije upravitelju osnovne cilj (cachemgr) se ne preuzimanje datoteke s poslužitelja postupak zbog čega replikacije par ograničavanje.
- Kada redoslijed fizičke prebacivanje klaster (MSCS) na disku nije jednak na sve čvorove, replikacija nije postavljen za neke od količine klaster.
<br/>Imajte na umu da klaster mora biti reprotected da iskoristite prednost ovaj popravak.  
- Funkcija SMTP ne funkcionira prema očekivanjima nakon RX će se nadograditi iz Scout 7.1 Scout 8.0.1.
- Dodatne stat su dodani u zapisniku za vraćanje postupak da biste pratili vrijeme je potrebno da biste dovršili ga.
- Podrška za dodan je za operacijske sustave Linux na izvornom poslužitelju:
    - Ažuriranje crveno je vaša Enterprise Linux (RHEL) 6 7
    - CentOS 6 ažuriranje 7
- CX i korisničkog Sučelja RX sada možete prikazati obavijest o za uparivanje prelazi u način rada bitmapa.
- Sljedeće sigurnosne popravke su dodani u RX:

**Opis problema**|**Postupci za implementaciju**
---|---
Autorizacija zaobići putem neovlašteno mijenjanje parametra|Ograničeni pristup nije primjenjivo korisnicima.
Krivotvorina zahtjev za web-mjesta|Implementirana pojam stranice tokena koje generira slučajno na svakoj stranici. <br/>Uz to, vidjet ćete: <li> Postoji samo jednu prijavu instancu za istoga korisnika.</li><li>Neće funkcionirati osvježavanja – bit ćete preusmjereni na nadzornu ploču.</li>
Prijenos zlonamjerni datoteke|Ograničena datoteka za određene proširenja. Dopuštene programski dodaci: 7z, aiff, asf, avi, bmp, csv, dokumenta, docx, ozna, flv, gif, gz, gzip, jpeg, jpg, zapisnika mid mov, mp3, mp4, mpc, mpeg, mpg, ods odt-a, pdf, png, ppt, pptx, pxd, qt, RAM-a, rar, upravitelj resursa, rmi, rmvb, rtf, sdc, sitd, swf, sxc, sxw, tar, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml, a zip.
Stalni skriptiranje web-mjesta | Dodaje unos provjere valjanosti.


>[AZURE.NOTE]
>
>-  Kumulativna su sva ažuriranja za oporavak web-mjesta. Ažuriranje 3 sadrži sve popravke Update 1 i 2 ažuriranja. Ažuriranje 3 se izravno primjenjuju na 8.0.1 ZŽ.
>-  Poslužitelj za konfiguraciju i RX ažuriranja ne može biti vraćeni natrag nakon mogu se primijeniti na sustav.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure web-mjesta oporavak Scout 8.0.1 ažuriranje 2 (03 Ažuriraj Pro 15)

Rješenja u ažuriranju 2 obuhvaćaju sljedeće:

- **Poslužitelj za konfiguraciju**: rješavanje problema koji se spriječio 31 dan besplatne mjerne značajku rada očekivani kada poslužitelj za konfiguraciju je registriran u oporavak web-mjesta.
- **Agent za Unified**: rješavanje problema u 1 ažuriranja koja je rezultirala ažuriranja ne instaliraju na glavni ciljnom poslužitelju kada je nadogradili verziju 8.0 i 8.0.1.


### <a name="azure-site-recovery-scout-801-update-1"></a>Azure web-mjesta oporavak Scout 8.0.1 ažuriranje 1

Ažuriranje 1 sadrži sljedeće popravci programskih pogrešaka i nove značajke:

- 31 dan besplatne zaštite po instanca poslužitelja. Omogućuje vam testiranje funkcionalnosti ili postavljanje programa dokaz-od-pojam.
    - Sve operacije na poslužitelju, uključujući prebacivanje i failback, su besplatno prvi 31 dan, počevši od vremena na poslužitelju najprije zaštićene upravljanjem Scout oporavak web-mjesta.
    - Iz 32nd dana Nadalje, sve zaštićene poslužitelje naplatit će vam brzinom standardne instancu za zaštitu oporavak Azure web-mjesta na web-mjesto za klijenta u vlasništvu.
    - U bilo kojem trenutku broj zaštićeni poslužitelja koje su trenutno naplatiti dostupna je na stranici nadzorne ploče sigurnog oporavak Azure web-mjesta.
- Podrška za vSphere sučelje naredbenog retka (vCLI) dodaju 5,5 ažuriranje 2.
- Podrška za dodali za operacijske sustave Linux na izvornom poslužitelju:
    - RHEL 6 ažuriranje 6
    - RHEL 5 ažuriranje 11
    - Ažuriranje centOS 6 6
    - Ažuriranje centOS 5 11
- Pogreška popravke kojima se riješiti sljedeća pitanja:
    - Registracija sigurnog neće uspjeti za konfiguraciju poslužitelja ili RX poslužitelj.
    - Klaster količine neće se prikazivati prema očekivanjima kada su grupirani virtualnim strojevima reprotected kada ih životopis.
    - Failback ne uspijeva kada osnovne odredišni poslužitelj nalazi na drugom poslužitelju ESXi iz lokalnog radnog virtualnih računala.
    - Konfiguriranje dozvola za datoteku mijenjaju prilikom nadogradnje na 8.0.1 koje utječu na zaštitu i operacije.
    - Prag resynchronization nije provesti prema očekivanjima, koji vodi replikacije nedosljedno ponašanje.
    - Postavke RPO se ne prikazuju ispravno sučelje za konfiguraciju poslužitelja. Vrijednosti koje nisu komprimirane podatka neispravno prikazuje Komprimirana vrijednost.
    -  Uklanjanje postupak ne briše očekivani u čarobnjaku za vContinuum i replikacija nije izbrisano putem sučelja za konfiguraciju poslužitelja.
    -  U čarobnjaku za vContinuum disk je automatski poništeni kada kliknete **pojedinosti** u prikazu na disku tijekom zaštitu MSCS virtualnih računala.
    - Tijekom fizičke-na-virtualne scenarij (P2V) potrebna HP servisa, kao što su CIMnotify i CqMgHost, ne premještaju ručno u oporavak virtualnog računala. Rezultira dodatne pokretanja.
    - Zaštita Linux virtualnog računala ne uspijeva kada postoji više od 26 diskova osnovne odredišni poslužitelj.

## <a name="next-steps"></a>Daljnji koraci

Objavite pitanja na [forumu servisa Azure oporavak](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).
