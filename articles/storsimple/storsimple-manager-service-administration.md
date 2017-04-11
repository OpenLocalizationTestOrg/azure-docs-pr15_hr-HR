<properties 
   pageTitle="Upravitelj StorSimple za administraciju servisa | Microsoft Azure"
   description="Saznajte kako upravljati uređajem StorSimple pomoću upravitelja StorSimple servisa Azure klasični portalu."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>Pomoću upravitelja StorSimple servisa upravljati uređajem StorSimple

## <a name="overview"></a>Pregled

U ovom se članku opisuju sučelje servisa StorSimple Manager uključujući kako povezati ga, različite mogućnosti i veze izvan određene tijekova rada koji se mogu obaviti putem ovog korisničkog Sučelja. Ovaj vodič se primjenjuju na oba; fizičke StorSimple i virtualnog uređaja.

Kad pročitate članak ćete naučiti:

- Povezivanje sa servisom StorSimple Manager
- Otvorite upravitelj UI StorSimple
- Administriranje uređaju StorSimple putem upravitelja StorSimple usluge


## <a name="connect-to-storsimple-manager-service"></a>Povezivanje sa servisom StorSimple Manager

Servis za upravitelja StorSimple pokreće se u Microsoft Azure i povezuje s više StorSimple uređaja. Središnje Microsoft Azure klasični portal izvodi u pregledniku omogućuju upravljanje sljedećim uređajima. Povezivanje sa servisom StorSimple upravitelj, učinite sljedeće:

#### <a name="to-connect-to-the-service"></a>Da biste se povezali sa servisom

1. Dođite do [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

1. Koristi vjerodajnice za Microsoftov račun, prijavite se na klasični portal Microsoft Azure (koja se nalazi u gornjem desnom kutu okna).

1. Pomaknite se prema dolje u lijevom navigacijskom oknu da biste pristupili StorSimple Upravitelj servisa.


## <a name="navigate-storsimple-manager-service-ui"></a>Otvorite upravitelj StorSimple servisa korisničkog Sučelja

Navigacijske hijerarhije StorSimple Upravitelj servisa korisničkog Sučelja prikazuju se u tablici u nastavku.

- Odredišna stranica za **Upravitelj StorSimple** vodi vas na odnosi se na svim uređajima unutar servisa korisničkog Sučelja razini usluge stranice.

- Stranice **uređaja** vodi vas na razini uređaja – korisničkog Sučelja stranice odnosi se na određeni uređaj.

- **Količinsko spremnika** stranice vodi vas na stranicu glasnoću koji prikazuje sve jedinice povezane s uređajem.


#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Upravitelj StorSimple servisa navigacijsku hijerarhiju

|Odredišna stranica|Stranica o razini usluge|Stranica razini uređaja|Stranica razini uređaja|
|---|---|---|---|
|Servis upravitelja StorSimple|Nadzorna ploča servisa|Nadzorna ploča za uređaj||
||Uređaji →|Monitora|
||Katalog|Containers→ glasnoće|Jedinice|
||Konfiguriranje (servis)|Sigurnosne kopije pravila||
||Zadaci|Konfiguriranje (uređaj)|
||Upozorenja|Održavanje|

![Videozapis dostupne](./media/storsimple-manager-service-administration/Video_icon.png) **videozapis dostupna**

Da biste pogledali videozapis u kojem se vodit će vas kroz korisničko sučelje za usluge StorSimple upravitelj, kliknite [ovdje](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>Administriranje StorSimple uređaja koji koristi servis upravitelja StorSimple

Sljedeća tablica prikazuje sažetak uobičajene zadatke upravljanja i složenih tijekova rada koji se mogu obaviti unutar StorSimple Upravitelj servisa korisničkog Sučelja. Zadaci su organizirane temeljeni na stranicama korisničkog Sučelja na kojem se pokreće.

Dodatne informacije o svaki tijek rada kliknite odgovarajući postupak u tablici.

#### <a name="storsimple-manager-workflows"></a>Upravitelj StorSimple tijekova rada

|Ako želite da biste to učinili...|Prijelaz na ovoj stranici korisničkog Sučelja...|Koristite ovaj postupak.|
|---|---|---|
|Stvaranje servisa</br>Brisanje servisa</br>Dobivanje ključa za registraciju</br>Obnovi ključa za registraciju|Servis upravitelja StorSimple|[Implementacija StorSimple Upravitelj servisa](storsimple-manage-service.md)
|Promjena ključa za šifriranje podataka usluge</br>U zapisnicima operacije|StorSimple Upravitelj servisa → nadzorne ploče|[Koristite upravljačku ploču StorSimple Upravitelj servisa](storsimple-service-dashboard.md)|
|Deaktiviranje uređaja</br>Brisanje uređaja|Upravitelj StorSimple servisa → uređaja|[Deaktiviranje i brisanje uređaja](storsimple-deactivate-and-delete-device.md)|
|Dodatne informacije o prebacivanje Izrada oporavak i uređaja</br>Prebacivanje na uređaj fizički</br>Prebacivanje na virtualnog uređaja</br>Oporavak Izrada tvrtke continuity (BCDR)|Upravitelj StorSimple servisa → uređaje|[Prebacivanje i Izrada oporavak za svoj uređaj StorSimple](storsimple-device-failover-disaster-recovery.md)|
|Popis kopija za jedinicu</br>Odaberite skup sigurnosnih kopija</br>Brisanje sigurnosne kopije skupa|StorSimple Upravitelj servisa → sigurnosne kopije kataloga|[Upravljanje sigurnosne kopije](storsimple-manage-backup-catalog.md)|
|Kloniraj jedinica|StorSimple Upravitelj servisa → sigurnosne kopije kataloga|[Kloniraj jedinica](storsimple-clone-volume.md)|
|Vraćanje sigurnosne kopije skup|StorSimple Upravitelj servisa → sigurnosne kopije kataloga|[Vraćanje sigurnosne kopije postavljanje](storsimple-restore-from-backup-set.md)|
|O računima za pohranu</br>Dodavanje računa za pohranu</br>Uređivanje prostora za pohranu računa</br>Brisanje računa za pohranu</br>Ključni zakretanje račune za pohranu|Konfiguriranje → StorSimple Upravitelj servisa|[Upravljanje računima za pohranu](storsimple-manage-storage-accounts.md)|
|O predlošcima propusnosti</br>Dodavanje predloška propusnosti</br>Uređivanje predloška propusnosti</br>Brisanje predloška propusnosti</br>Koristili zadani predložak propusnosti</br>Stvaranje cjelodnevnog propusnosti predloška koji će se pokrenuti u određeno vrijeme|Konfiguriranje → StorSimple Upravitelj servisa|[Upravljanje predlošcima propusnosti](storsimple-manage-bandwidth-templates.md)|
|Zapisi za kontrolu pristupa</br>Stvaranje zapisa kontrole programa access</br>Uređivanje zapisa kontrole programa access</br>Brisanje zapisa kontrole programa access|Konfiguriranje → StorSimple Upravitelj servisa|[Upravljanje zapisima za kontrolu pristupa](storsimple-manage-acrs.md)|
|Prikaz detalja o posla</br>Otkazivanje posla|Upravitelj StorSimple servisa → poslove|[Upravljanje zadacima](storsimple-manage-jobs.md)
|Primanje upozorenja</br>Upravljanje upozorenjima</br>Pregled upozorenja|Upravitelj StorSimple service → upozorenja|[Prikaz i upravljanje upozorenjima StorSimple](storsimple-manage-alerts.md)
|Prikaz povezanih Pokretači</br>Pronalaženje serijski broj uređaja</br>Traženje cilja IQN|Uređaji StorSimple Upravitelj servisa → → nadzorne ploče|[Koristite upravljačku ploču StorSimple uređaja](storsimple-device-dashboard.md)|
|Stvaranje grafikona nadzora|Uređaji StorSimple Upravitelj servisa → → praćenje|[Praćenje StorSimple uređaj](storsimple-monitor-device.md)|
|Dodavanje spremnika glasnoće</br>Izmjena spremniku glasnoće</br>Izbrišite spremnik glasnoće|StorSimple → servisa Upravitelj uređaja → glasnoću spremnika|[Upravljanje spremnika glasnoće](storsimple-manage-volume-containers.md)|
|Dodavanje jedinica</br>Izmjena jedinica</br>Izvanmrežni jedinica</br>Brisanje jedinica</br>Praćenje jedinica|StorSimple Upravitelj servisa → uređaji → glasnoću spremnika → jedinicama|[Upravljanje količine](storsimple-manage-volumes.md)|
|Izmjena postavke audiouređaja</br>Izmjena postavke vremena</br>Izmjena postavki DNS.md</br>Konfiguriranje sučelje mreže|Uređaji StorSimple Upravitelj servisa → → konfiguriranje|[Izmjena Konfiguracija uređaja za svoj uređaj StorSimple](storsimple-modify-device-config.md)|
|Prikaz web-proxy postavke|Uređaji StorSimple Upravitelj servisa → → konfiguriranje|[Konfiguriranje web proxy za svoj uređaj](storsimple-configure-web-proxy.md)|
|Izmjena uređaju administratorsku lozinku</br>Upravitelj snimku StorSimple lozinke za izmjenu|Uređaji StorSimple Upravitelj servisa → → konfiguriranje|[Promjena lozinke StorSimple](storsimple-change-passwords.md)|
|Konfiguriranje daljinskog upravljanja|Uređaji StorSimple Upravitelj servisa → → konfiguriranje|[Daljinsko povezivanje s uređajem StorSimple](storsimple-remote-connect.md)|
|Konfiguriranje postavki upozorenja|Uređaji StorSimple Upravitelj servisa → → konfiguriranje|[Prikaz i upravljanje upozorenjima StorSimple](storsimple-manage-alerts.md)|
|Konfiguriranje CHAP za svoj uređaj StorSimple|Uređaji StorSimple Upravitelj servisa → → konfiguriranje|[Konfiguriranje CHAP za svoj uređaj StorSimple](storsimple-configure-chap.md)|
|Dodavanje sigurnosne kopije pravila</br>Dodavanje ili izmjena rasporeda</br>Brisanje sigurnosne kopije pravila</br>Iskoristite ručno sigurnosno kopiranje</br>Stvaranje prilagođene sigurnosne kopije pravila s više količine i rasporedima|Upravitelj StorSimple servisa → uređaji → sigurnosne kopije pravila|[Upravljanje pravilnicima za sigurnosne kopije](storsimple-manage-backup-policies.md)|
|Zaustavljanje kontrolera uređaja</br>Ponovno pokrenite uređaj kontrolera</br>Isključite uređaj kontrolera</br>Ponovno postavljanje uređaja na tvorničke zadane postavke</br>(Noviji su za lokalni uređaj samo)|Uređaji StorSimple Upravitelj servisa → → održavanja|[Upravljanje kontroler StorSimple uređaja](storsimple-manage-device-controller.md)|
|Dodatne informacije o StorSimple hardverske komponente</br>Status hardver monitora</br>(Noviji su za lokalni uređaj samo)|Uređaji StorSimple Upravitelj servisa → → održavanja|[Monitor hardverske komponente](storsimple-monitor-hardware-status.md)|
|Stvaranje paketa za podršku|Uređaji StorSimple Upravitelj servisa → → održavanja|[Stvaranje i upravljanje paket za podršku](storsimple-create-manage-support-package.md)|
|Instalacija ažuriranja|Uređaji StorSimple Upravitelj servisa → → održavanja|[Ažuriranje uređaja](storsimple-update-device.md)|


##<a name="next-steps"></a>Daljnji koraci
Ako se pojave problemi sa svakodnevnim uređaju StorSimple ili neke njegove hardverske komponente, pogledajte:

- [Otklanjanje poteškoća s radu uređaja](storsimple-troubleshoot-operational-device.md)
- [Korištenje StorSimple nadzor LEDs pokazatelja](storsimple-monitoring-indicators.md)

Ako ne možete riješiti probleme i potrebnih za stvaranje zahtjeva za uslugu, pogledajte [kontakt Microsoftovoj](storsimple-contact-microsoft-support.md)podršci.
