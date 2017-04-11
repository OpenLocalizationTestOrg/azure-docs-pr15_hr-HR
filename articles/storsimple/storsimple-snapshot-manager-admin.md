<properties 
   pageTitle="Administracija StorSimple snimke Upravitelj | Microsoft Azure"
   description="Omogućuje pregled i veze na dodatne informacije o StorSimple snimke Upravitelj rješenje administrativnih zadataka i tijekova rada."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-administer-your-storsimple-solution"></a>Korištenje StorSimple snimke upravitelja za administriranje rješenje StorSimple

## <a name="overview"></a>Pregled

Upravitelj snimka StorSimple je Microsoft Management Console (BLOG) dodatku koji olakšava zaštitu podataka i upravljanje sigurnosne kopije u okruženju sustava Microsoft Azure StorSimple. Pomoću upravitelja StorSimple snimke, upravljanje Microsoft Azure StorSimple podataka u centru za podatke i u oblaku kao rješenje jedan integrirani prostora za pohranu, stoga Pojednostavnjivanje postupaka sigurnosnog kopiranja i smanjenju troškova.

Konzola za središnje upravljanje StorSimple snimke upravitelja omogućuje vam stvaranje dosljedan, točke u vrijeme sigurnosne kopije lokalno i podataka u oblaku. Na primjer, možete koristiti na konzoli za:

- Konfiguriranje, sigurnosno kopiranje i brisanje količine.

- Konfiguriranje glasnoću grupe da biste bili sigurni sigurnosne kopije podataka je aplikacija dosljedni.

- Upravljanje pravilnicima za sigurnosno kopiranje tako da se podaci se sigurnosno kopira unaprijed određenim raspored.

- Stvorite neovisno kopije podataka, što može biti pohranjen u oblaku i koristi za oporavak Izrada.

U ovom se članku navode veze na vodiči za koje opisuju StorSimple snimke upravitelj i kako je koristiti za dovršavanje zadataka administracije sustava i tijekova rada.

- Dodatne informacije o StorSimple snimke Upravitelj komponente i arhitekturu potražite u članku [što je upravitelj StorSimple snimke?](storsimple-what-is-snapshot-manager.md) 

- Da biste preuzeli Upravitelj StorSimple snimke, otvorite [Upravitelj snimka StorSimple stranici za preuzimanje](https://www.microsoft.com/download/details.aspx?id=44220).

- Postupak implementacije Upravitelj StorSimple snimke, otvorite [Implementacija Upravitelj StorSimple snimke](storsimple-snapshot-manager-deployment.md).

>[AZURE.NOTE] Upravitelj snimka StorSimple ne možete koristiti da biste upravljali Microsoft Azure StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualne uređaji).

## <a name="storsimple-snapshot-manager-tasks-and-workflows"></a>StorSimple snimke upravitelja zadataka i tijekova rada

Snimka Upravitelj StorSimple možete koristiti za nadzor i upravljanje trenutni, zakazano i dovršene sigurnosne kopije zadataka. Uz to, Upravitelj snimka StorSimple nudi kataloga do 64 dovršene sigurnosne kopije. Katalog možete koristiti da biste pronašli i vraćanje količine ili pojedinačne datoteke. 

| AKO ŽELITE DA BISTE TO UČINILI...  | POMOĆU OVOG PRAKTIČNOG VODIČA... |
|:---------------------------|:----------------------|
|Dodatne informacije o upravitelju za StorSimple snimke | [Što je upravitelj snimka StorSimple?](storsimple-what-is-snapshot-manager.md)|
| Instalacija StorSimple snimke Manager<br>Ponovno instalirajte Manager StorSimple snimke<br>Uklanjanje upravitelja StorSimple snimke| [Implementacija Upravitelj StorSimple snimke](storsimple-snapshot-manager-deployment.md) |
| Korištenje StorSimple snimke upravitelja izbornike i značajke:<ul><li>Traka izbornika</li><li>Alatnoj traci</li><li>Okno dosega</li><li>Okno s rezultatima</li><li>Okno akcije</li><li>Navigacija pomoću tipkovnice i prečaca</li></ul>| [Upravitelj snimka StorSimple korisničkog sučelja](storsimple-use-snapshot-manager.md) |
| Koristite uobičajene uključeni u StorSimple snimke upravitelja značajke za BLOG:<ul><li>Prikaz</li><li>Novi prozor odavde</li><li>Osvježavanje</li><li>Izvoz popisa</li><li>Pomoć</li></ul>| [Korištenje izbornik Akcije za BLOG u programu StorSimple snimke Manager](storsimple-snapshot-manager-mmc-menu.md)
| Dodajte ili zamijenite uređaja<br>Priključite uređaj<br>Provjerite je li uvezena glasnoću grupe<br>Osvježavanje povezanih uređaja<br>Autentičnost uređaja<br>Prikaz detalja o uređaju<br>Brisanje Konfiguracija uređaja<br>Promjena lozinke za uređaj<br>Zamjena nije uspjelo uređaja<br>| [Korištenje upravitelja snimka StorSimple da bi povezao i upravljanje uređajima StorSimple](storsimple-snapshot-manager-manage-devices.md) |
| Postavljanje količine<br>Prikaz informacija o količine<br>Brisanje jedinica<br>Ponovni pregled količine<br>Konfiguriranje i sigurnosno kopiranje osnovne jedinice<br>Konfiguriranje i sigurnosno kopiranje dinamički zrcaljene jedinice| [Korištenje StorSimple snimke upravitelja za prikaz i upravljanje količine](storsimple-snapshot-manager-manage-volumes.md) |
| Prikaz grupa jedinica<br>Stvaranje grupe za glasnoću<br>Stvaranje sigurnosne kopije grupu jedinica<br>Uređivanje grupe jedinica<br>Brisanje grupe za glasnoću | [Korištenje StorSimple snimke upravitelja za stvaranje i upravljanje grupama za glasnoću](storsimple-snapshot-manager-manage-volume-groups.md) |
| Stvaranje sigurnosne kopije pravila <br>Uređivanje sigurnosne kopije pravila<br>Brisanje sigurnosne kopije pravila | [Korištenje StorSimple snimke upravitelja za stvaranje i upravljanje sigurnosne kopije pravila](storsimple-snapshot-manager-manage-backup-policies.md) |
| Prikaz i upravljanje Zakazani zadaci sigurnosnog kopiranja<br>Prikaz i upravljanje nedavne sigurnosne kopije zadataka<br>Prikaz i upravljanje njima trenutno pokrenuti sigurnosno kopiranje poslova | [Korištenje StorSimple snimke upravitelja za prikaz i upravljanje sigurnosne kopije poslova](storsimple-snapshot-manager-manage-backup-jobs.md) |
| Vraćanje jedinica<br>Kloniraj volumen ili grupe jedinica<br>Brisanje sigurnosne kopije<br>Oporavak datoteka<br>Vraćanje baze podataka upravitelja StorSimple snimke| [Korištenje upravitelja snimka StorSimple da biste upravljali katalog sigurnosne kopije](storsimple-snapshot-manager-manage-backup-catalog.md) |

## <a name="next-steps"></a>Daljnji koraci

[Snimka StorSimple Upravitelj preuzimanja](https://www.microsoft.com/download/details.aspx?id=44220).
