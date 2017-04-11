<properties 
   pageTitle="Što je upravitelj snimka StorSimple? | Microsoft Azure"
   description="U članku se opisuje upravitelju StorSimple snimke, njegova arhitektura i njegove značajke."
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
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="what-is-storsimple-snapshot-manager"></a>Što je upravitelj snimku StorSimple?

## <a name="overview"></a>Pregled

Upravitelj snimka StorSimple je Microsoft Management Console (BLOG) dodatku koji olakšava zaštitu podataka i upravljanje sigurnosne kopije u okruženju sustava Microsoft Azure StorSimple. Pomoću upravitelja StorSimple snimke, upravljanje Microsoft Azure StorSimple podataka u centru za podatke i u oblaku kao rješenje jedan integrirani prostora za pohranu, stoga Pojednostavnjivanje postupaka sigurnosnog kopiranja i smanjenju troškova.

U ovom pregled predstavlja upravitelju StorSimple snimke, u članku se opisuje njegove značajke i objašnjava njegova uloga u sustavu Microsoft Azure StorSimple. 

Pregled cijelog sustava Microsoft Azure StorSimple, uključujući StorSimple uređaj, servis upravitelja StorSimple, StorSimple snimku upravitelj i StorSimple prilagodnik za SharePoint, potražite u članku [StorSimple 8000 niz: hibridnog oblaka za pohranu rješenje](storsimple-overview.md). 
 
>[AZURE.NOTE] 
>
>- Upravitelj snimku StorSimple ne možete koristiti da biste upravljali Microsoft Azure StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualne uređaji).
>
>- Ako planirate instalirati StorSimple ažuriranju 2 na uređaju StorSimple, obavezno preuzmite najnoviju verziju programa StorSimple snimke Manager i instalirajte ga **prije instalacije StorSimple ažuriranju 2**. Najnoviju verziju programa StorSimple snimke Manager nije kompatibilna i radi sve objavljene verzije sustava Microsoft Azure StorSimple. Ako koristite prethodnu verziju StorSimple snimke upravitelja, morat ćete ažurirati (ne morate deinstalirati prethodnu verziju prije instalirati novu verziju).

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>Svrha StorSimple snimke upravitelj i arhitekture

Upravitelj snimka StorSimple nudi konzolu za središnje upravljanje koje možete koristiti da biste stvorili dosljedan, točke u vrijeme sigurnosne kopije lokalno i podataka iz oblaka. Na primjer, možete koristiti na konzoli za:

- Konfiguriranje, sigurnosno kopiranje i brisanje količine.
- Konfiguriranje glasnoću grupe da biste bili sigurni sigurnosne kopije podataka je aplikacija dosljedni.
- Upravljanje pravilnicima za sigurnosno kopiranje tako da se podaci se sigurnosno kopira unaprijed određenim raspored.
- Stvaranje lokalne i snimke, što može biti pohranjen u oblaku i koristi za oporavak Izrada u oblaku.

Snimka Upravitelj StorSimple dohvaćanja na popisu aplikacija registrirane kod davatelja VSS na glavnom računalu. Zatim, da biste stvorili aplikacije dosljedan sigurnosne kopije, ga provjerava količine koristi aplikacija i predlaže glasnoću grupe da biste konfigurirali. Upravitelj snimka StorSimple koristi te grupe jedinica za stvaranje sigurnosne kopije koje su u skladu aplikacije. (Dosljednost računala postoji kada sve povezane datoteke i baza podataka sinkroniziraju i predstavljaju true stanje aplikacije na određenom mjestu u vremenu). 

Sigurnosno kopiranje StorSimple snimke Upravitelj se pojavljuju u obliku rastuća snimke koje snimite samo promjene nakon zadnje sigurnosne kopije. Kao rezultat sigurnosne kopije potrebno manje prostora za pohranu i može se stvara i vratiti brzo. Upravitelj snimka StorSimple koristi u Windows glasnoću sjene Kopiraj servisa (VSS) da biste bili sigurni da snimke snimiti aplikacije ujednačenim podacima. (Dodatne informacije potražite povratak integracije s odjeljak Windows glasnoću sjene Kopiraj servis.) Pomoću upravitelja StorSimple snimke, možete stvoriti sigurnosnu kopiju rasporeda ili poduzeti odmah sigurnosne kopije prema potrebi. Ako morate vratiti podatke iz sigurnosne kopije, Upravitelj snimka StorSimple omogućuje birate iz kataloga lokalne ili oblaka snimke. Azure StorSimple vraća samo podatke koje je potrebno prema potrebi, koja sprječava kašnjenja u dostupnost podataka tijekom operacije vraćanja.)

![Upravitelj snimka StorSimple arhitekture](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**Upravitelj snimka StorSimple arhitekture** 

## <a name="support-for-multiple-volume-types"></a>Podrška za više vrsta glasnoće

Pomoću upravitelja snimka StorSimple možete konfigurirati i sigurnosno kopiranje sljedeće vrste količine: 

- **Osnovni količine** – osnovne jedinice je jedna particija na osnovni disk. 

- **Jednostavan količine** – jednostavna jedinica je dinamički disk koji sadrži prostor na disku iz jedne dinamički disk. Jednostavna jedinica sastoji se od jedno područje na disku ili više područja koja su međusobno povezivati na istom disku. (Stvorite jednostavan količine samo na dinamičkih diskova.) Jednostavan količine nisu kvara pogreške.

- **Dinamičke jedinice** – dinamički disk je jedinica stvorena na dinamički disk. Dinamičkih diskova koristite bazu podataka za praćenje informacija o jedinicama koje se nalaze na dinamičkih diskova na računalu. 

- **Dinamični jedinicama s zrcaljenje** – dinamičke jedinice s zrcaljenje ugrađenih na arhitekturi RAID 1. 1 RAID identične podaci se upisuju na dva ili više disku proizvodnje skupu zrcaljene. Potvrda čitanja se može riješiti tako da sve disk koji sadrži tražene podatke.

- **Zajedničko korištenje klaster količine** – s klaster zajednički količine (CSVs), više čvorove u prebacivanje klaster možete istodobno čitanje ili pisanje isti disk. Prebacivanje iz jednog čvor na drugi čvor javlja se brzo, bez promjene u pogon vlasništva ili postavljanje, dismounting i uklanjanje jedinicu. 

>[AZURE.IMPORTANT] Izradite CSVs i koje nisu-CSVs u istom snimku. Miješanje CSVs i koje nisu-CSVs u snimku nije podržana. 
 
Pomoću upravitelja snimka StorSimple vraćanje čitave jedinice grupe ili Kloniraj pojedinačne količine i oporavak pojedinačne datoteke.

- [Količine i grupama jedinica](#volumes-and-volume-groups) 
- [Vrsta sigurnosne kopije i sigurnosne kopije pravila](#backup-types-and-backup-policies) 

Dodatne informacije o StorSimple snimke značajkama i kako ih koristiti potražite u članku [Upravitelj snimka StorSimple korisničkog sučelja](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Količine i grupama jedinica

Pomoću upravitelja StorSimple snimke, stvaranje količine i konfigurirati ih u grupe jedinica. 

Upravitelj snimka StorSimple koristi grupe jedinica za stvaranje sigurnosne kopije koje su u skladu aplikacije. Aplikacija dosljednost postoji kada sve povezane datoteke i baza podataka sinkroniziraju i predstavljaju true stanje aplikacije na određenom mjestu u vremenu. Grupa jedinica (koji se nazivaju i *dosljednost grupe*) čine sigurnosnu kopiju ili vraćanje posao.

Grupa jedinica nisu jednaki spremnika glasnoću. Spremnik glasnoću sadrži jednu ili više jedinicama zajednički koristite s računom za pohranu oblak i druge atribute, kao što su šifriranje i propusnosti. Jedna jedinica spremnik može sadržavati do 256 thinly dodijeljenu StorSimple količine. Dodatne informacije o jedinici spremnika otvorite [Upravljanje vaše spremnika glasnoću](storsimple-manage-volume-containers.md). Jedinice grupe su zbirke količine koja konfigurirati da biste olakšali operacije sigurnosnog kopiranja. Ako ste odabrali dvije jedinice koji pripadaju spremnika drugi pogon, smjestiti u jednu grupu, a zatim stvorite sigurnosne kopije pravila za tu grupu jedinica, svakoj sigurnosno će se u željeni spremnik odgovarajuće jedinice pomoću računa za odgovarajuću prostora za pohranu.

>[AZURE.NOTE] Sve jedinice u grupi jedinica mora potjecati iz usluga za jednu oblaka.

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Integracija sa servisom Kopiraj sjene jedinica za Windows

Upravitelj snimka StorSimple koristi u Windows glasnoću sjene Kopiraj servisa (VSS) da biste snimili aplikacije ujednačenim podacima. VSS olakšava dosljednost aplikacije po komunikaciju s VSS umu aplikacije koordiniranje stvaranja rastuće snimke. VSS osigurava da aplikacije su privremeno Neaktivni ili mirovanja, kada uzimaju se snimke. 

Implementacija StorSimple snimke Upravitelj VSS funkcionira s SQL Server i generički NTFS jedinice. Postupak je na sljedeći način: 

1. Podnositelja zahtjeva, a to je najčešće upravljanje podacima i rješenja za zaštitu (kao što su StorSimple snimke Upravitelj) ili program za sigurnosno kopiranje, poziva VSS te vas pita da Prikupite informacije iz writer softver u ciljnu aplikaciju.

2. VSS kontakta komponentu napisao dohvatiti opis podataka. U writer vraća opis odabranog podataka za sigurnosno kopiranje. 

3. VSS signale writer Priprema računala za sigurnosno kopiranje. U writer Priprema podataka za sigurnosno kopiranje s povećanim Otvori transakcije ažuriranje zapisnicima transakcija i tako dalje, a zatim obavještava VSS.

4. VSS upućuje writer da biste privremeno zaustavili služi za pohranu podataka aplikacije i provjerite je li da nema podataka o napisanih jedinici dok je stvorena je kopija sjene. Ovaj korak osigurava dosljednost podataka i traje više od 60 sekundi.

5. VSS upućuje davatelja da biste stvorili kopiju sjene. Davatelji, što može biti softvera i hardvera utemeljen na, upravljanje količine koje se trenutno izvode i stvaranje kopije sjene na zahtjev. Davatelj stvara kopiju sjene, a obavještava VSS kada se dovrši.

6. VSS kontakta napisao će obavijestiti aplikacije koje možete nastaviti/i i da biste potvrdili da/i pauzirano je uspješno tijekom stvaranja Kopiraj sjene. 

7. Ako kopiju je bio uspješan, VSS vraća kopiji radnog mjesta podnositelja zahtjeva. 

8. Ako podatke napisan dok je stvorena je kopija sjene, sigurnosno kopiranje bit će koje nisu usklađene. VSS briše Kopiraj sjene i obavještava podnositelja zahtjeva. Podnositelja zahtjeva možete automatski ponovite postupak sigurnosnog kopiranja ili obavijestite administratora da biste pokušali ponovno kasnije.

Pogledajte na sljedećoj slici.

![Postupak VSS](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Proces Windows glasnoću sjene Kopiraj usluge** 

## <a name="backup-types-and-backup-policies"></a>Vrsta sigurnosne kopije i sigurnosne kopije pravila

Pomoću upravitelja StorSimple snimke, možete izraditi sigurnosnu kopiju podataka i spremiti lokalno i u oblaku. Pomoću upravitelja snimka StorSimple možete odmah sigurnosno kopiranje podataka ili sigurnosne kopije pravila možete koristiti radi stvaranja rasporeda za automatsko pisanje sigurnosne kopije. Sigurnosne kopije pravila omogućuju da odredite koliko snimaka će biti zadržani. 

### <a name="backup-types"></a>Vrsta sigurnosne kopije

Upravitelj snimka StorSimple možete koristiti da biste stvorili sljedeće vrste sigurnosne kopije:

- **Lokalni snimaka** – lokalne snimke su kopije glasnoću podataka pohranjenih na uređaju StorSimple točke u vrijeme. Obično ovu vrstu sigurnosne kopije možete biti stvoren sustavu i brzo vratiti. Lokalni snimke stanja možete koristiti kao lokalnu sigurnosnu kopiju.

- **Oblak snimaka** – oblaka snimke su točke u vrijeme kopije glasnoću podatke pohranjene u oblaku. Oblak snimku jednako snimku replicirati na drugu, službenoj prostora za pohranu. Brze snimke oblaka su osobito je korisna u slučajevima Izrada oporavak.

### <a name="on-demand-and-scheduled-backups"></a>Na zahtjev i zakazani sigurnosne kopije

Pomoću upravitelja StorSimple snimke, možete pokrenuti jednokratni sigurnosnu kopiju će biti stvoren odmah ili sigurnosne kopije pravila omogućuje zakazivanje ponavljajućeg operacije sigurnosnog kopiranja.

Sigurnosne kopije pravila je skupa automatiziranog pravila koje možete koristiti za planiranje redovitog sigurnosnog kopiranja. Sigurnosne kopije pravila omogućuje da odredite učestalost i parametara za poduzimanja snimaka određene glasnoću grupe. Možete koristiti pravila za određivanje datume početka i isteka, vremena, učestalost i preduvjeti za zadržavanja za oba lokalno i brze snimke u oblaku. Pravilo primjenjuje se odmah nakon što ga definirate. 

Pomoću upravitelja snimka StorSimple da biste konfigurirali ili konfigurirajte sigurnosne kopije pravila kad god je potrebno. 

Konfigurirajte sljedeće informacije za svaki sigurnosne kopije pravila koja stvarate:

- **Naziv** – jedinstveni naziv odabrana sigurnosne kopije pravila.

- **Vrsta** – vrstu sigurnosne kopije pravila; Lokalni snimka ili oblaka snimke.

- **Grupa jedinica** – grupi glasnoću koji je dodijeljen odabrana sigurnosne kopije pravila.

- **Zadržavanje** – broj sigurnosne kopije za zadržavanje. Ako potvrdite okvir **sve** sve sigurnosne kopije se zadržavaju dok maksimalni broj sigurnosne kopije po jedinici dosegne, pri čemu pravilo će neće uspjeti i generiranje poruka o pogrešci. Osim toga, možete odrediti broj kopija da biste zadržali (između 1 i 64).

- **Datum** – datum stvaranja sigurnosne kopije pravila.

Informacije o konfiguriranju sigurnosne kopije pravila, idite na [Korištenje StorSimple snimke upravitelja za stvaranje i upravljanje sigurnosne kopije pravila](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Sigurnosno kopiranje nadzor i upravljanje njima

Snimka Upravitelj StorSimple možete koristiti za nadzor i upravljanje nadolazeće, zakazano i dovršenom sigurnosne kopije zadataka. Uz to, Upravitelj snimka StorSimple nudi kataloga do 64 dovršene sigurnosne kopije. Katalog možete koristiti da biste pronašli i vraćanje količine ili pojedinačne datoteke. 

Informacije o nadzoru sigurnosne kopije zadataka, idite na [Korištenje StorSimple snimke upravitelja za prikaz i upravljanje zadacima sigurnosne kopije](storsimple-snapshot-manager-manage-backup-jobs.md).


## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o [korištenju StorSimple snimku Upravitelj administriranje StorSimple rješenje](storsimple-snapshot-manager-admin.md).

- Preuzmite [Upravitelj StorSimple snimke](https://www.microsoft.com/download/details.aspx?id=44220).
