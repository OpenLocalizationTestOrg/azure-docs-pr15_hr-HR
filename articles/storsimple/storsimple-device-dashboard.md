<properties
   pageTitle="Koristite upravljačku ploču StorSimple Upravitelj uređaja | Microsoft Azure"
   description="U članku se opisuje na nadzornoj ploči StorSimple Upravitelj servisa uređaj i kako ga koristiti za prikaz metrike pohrane i povezani Pokretači i pronađite serijski broj i IQN."
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

# <a name="use-the-storsimple-manager-device-dashboard"></a>Koristite upravljačku ploču StorSimple Upravitelj uređaja

## <a name="overview"></a>Pregled

Na nadzornoj ploči za StorSimple Upravitelj uređaja omogućuje pregled podataka za određeni uređaj StorSimple, za razliku od nadzorna ploča servisa, koja vam daje informacije o svih uređaja obuhvaćeno rješenje Microsoft Azure StorSimple.

![Stranica nadzorne ploče na uređaju](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Na nadzornoj ploči sadrži sljedeće informacije:

- **Područje grafikona** – možete vidjeti odgovarajuću prostora za pohranu metrika u područje grafikona pri vrhu nadzorne ploče. Na tom grafikonu, možete pogledati metriku za Ukupno primarni prostora za pohranu (količinu podataka napisao domaćini s uređajem) i za pohranu u oblaku ukupni troše uređaju tijekom vremenskog razdoblja.

     U ovom kontekstu *primarni prostora za pohranu* odnosi na ukupnu količinu podataka napisao glavno računalo te se podijeljene po vrsti pogona: *primarni tiered prostora za pohranu* uključuje i lokalne podatke i tiered podataka s oblakom *primarni lokalno prikvačene prostora za pohranu* sadrži samo podaci koji su spremljeni lokalno. *Za pohranu u oblaku*, s druge strane, je mjera ukupni iznos podatke pohranjene u oblaku. To obuhvaća tiered podataka i sigurnosno kopiranje. Imajte na umu podatke pohranjene u oblaku deduplicated i sažeti, dok je primarni prostora za pohranu označava korištene prije no što se podaci deduplicated i sažeti prostora za pohranu. (Tih dvaju brojeva da biste dobili ideju rate sažimanja možete usporediti.) Za obje primarne i za pohranu u oblaku, iznosi prikazani će se temeljiti na praćenje učestalosti konfigurirate. Ako, na primjer, ako odaberete jedan tjedan frequency, zatim grafikon vode podataka za svaki dan u prethodnom tjednu.

     Grafikon možete konfigurirati na sljedeći način:

     - Da biste vidjeli količinu za pohranu u oblaku potrošena tijekom vremena, odaberite mogućnost **ISKORIŠTENO prostora za POHRANU u OBLAKU** . Da biste vidjeli ukupan prostor za pohranu koji je na računalo koje hostira napisao, odaberite željene mogućnosti **PRIMARNI TIERED ISKORIŠTENO prostora za POHRANU** i **PRIMARNI LOKALNO PRIKVAČENE ISKORIŠTENO prostora za POHRANU** . Na slici odabrane su obje mogućnosti; Stoga grafikon prikazuje iznose prostora za pohranu za oblak i primarni prostora za pohranu. Imajte na umu da primarni za pohranu koriste prije instalacije ažuriranja 2 predstavlja redak **PRIMARNI TIERED ISKORIŠTENO prostora za POHRANU** .
     - Koristite padajući izbornik u gornjem desnom kutu grafikona da biste odredili 1 tjedan, 1 mjesec, 3 mjeseca ili godine 1 vremensko razdoblje. Imajte na umu da najviše razine grafikona osvježiti samo jedanput dnevno, a odražavaju stoga ukupni zbrojevi na prethodni dan.

     Dodatne informacije potražite u članku [korištenje servisa StorSimple Upravitelj praćenje StorSimple uređaj](storsimple-monitor-device.md).

- **Pregled korištenja** – u području **Korištenje pregled** možete vidjeti primarni prostora za pohranu koriste, dodijeljenu prostora za pohranu i najveći kapaciteta za svoj uređaj. Usporedbom te korištenje brojeve za maksimalnu količinu prostora za pohranu koji je dostupan možete vidjeti na prvi pogled Ako morate dobiti dodatan prostor za pohranu. Imajte na umu da ova pregled se ažurira svakih 15 minuta i, zbog razlika učestalost ažuriranja mogu prikazati različite brojeve od onih prikazane u području grafikona iznad, koji će se ažurirati svaki dan. Dodatne informacije potražite u članku [korištenje servisa StorSimple Upravitelj praćenje StorSimple uređaj](storsimple-monitor-device.md).


- **Upozorenja** – područje **upozorenja** sadrži pregled upozorenja za svoj uređaj. Upozorenja grupirane prema težinu, uvjet, a ukupan broj upozorenja na svakoj razini težinu. Klikom na upozorenje težinu otvara opsegu prikaz karticu upozorenja da bi se prikazala samo upozorenja te razine težinu za ovaj uređaj.

- **Zadaci** – području **Poslovi** prikazuje rezultat nedavne aktivnosti zadatka. To možete bili ste sustav radi prema očekivanjima ili ga možete obavijesti koje treba poduzeti akciju ispravljanja. Da biste vidjeli dodatne informacije o nedavno dovršene zadatke, kliknite **poslova uspješno zadnja 24 sata**.

- Područje **brzi pogled** na desnoj strani nadzorne ploče sadrži korisne informacije kao što su model uređaja, serijski broj, stanje, opis i broj količine.

Konfiguriranje prebacivanje i prikaz povezanih Pokretači na nadzornoj ploči uređaj.

Uobičajene zadatke koje možete obaviti na ovoj stranici su:

- Prikaz povezanih Pokretači

- Pronalaženje serijski broj uređaja

- Traženje cilja uređaj IQN

## <a name="view-connected-initiators"></a>Prikaz povezanih Pokretači

Možete pogledati Pokretači iSCSI koji su povezani s uređajem klikom na **Prikaz povezani Pokretači** vezu u području **brzi pogled** na nadzornu ploču uređaja. Ova stranica sadrži tablični popis Pokretači koji je uspješno povezati s uređajem. Za svaki Pokretač može vidjeti:

- ISCSI kvalificirani naziv (IQN) od povezanog pokretač.

- Naziv pristup kontrola zapisa (ACR) koji omogućuje ovaj povezanog pokretač.

- IP adrese povezanog pokretač.

- Sučelje za mreže pokretač povezano s na uređaj za pohranu. To možete u rasponu od podataka 0 do 5 podataka.

- Sve količine povezanih pokretač dopuštenog da biste pristupili prema trenutnoj konfiguraciji ACR.

Ako vidite neočekivane Pokretači na ovom popisu, ili ne vidite očekivanih, pregledajte konfiguraciju ACR. Maksimalno 512 Pokretači možete se povezati s uređajem.

## <a name="find-the-device-serial-number"></a>Pronalaženje serijski broj uređaja

Kada konfigurirate Microsoft Multipath/i (MPIO) na uređaju možda je potrebno serijski broj uređaja. Izvršite sljedeće korake da biste pronašli serijski broj uređaja.

#### <a name="to-find-the-device-serial-number"></a>Da biste pronašli serijski broj uređaja

1. Pronađite **uređaje** > **nadzorne ploče**.

2. U desnom oknu na nadzornoj ploči Pronađite područje **sažet** .

3. Pomaknite se prema dolje i pronađite serijski broj.

## <a name="find-the-device-target-iqn"></a>Traženje cilja uređaj IQN

Kada konfigurirate test rukovanja provjere autentičnosti protokolom (CHAP) na uređaju StorSimple možda je potrebno ciljni uređaj IQN. Izvršite sljedeće korake da biste pronašli ciljni uređaj IQN.

### <a name="to-find-the-device-target-iqn"></a>Da biste pronašli ciljni uređaj IQN

1. Pronađite **uređaje** > **nadzorne ploče**.

1. U desnom oknu na nadzornoj ploči Pronađite područje **sažet** .

1. Pomaknite se prema dolje i pronađite cilj IQN.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o [nadzorna ploča za StorSimple Upravitelj servisa](storsimple-service-dashboard.md).
- Dodatne informacije o [korištenju StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).
