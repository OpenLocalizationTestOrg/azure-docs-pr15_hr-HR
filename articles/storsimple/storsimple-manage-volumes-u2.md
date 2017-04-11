<properties
   pageTitle="Upravljanje StorSimple količine (U2) | Microsoft Azure"
   description="U članku se objašnjava kako dodati izmjena, praćenje i brisanje StorSimple količine i kako izvanmrežni ih po potrebi."
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
   ms.workload="NA"
   ms.date="10/28/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>Koristite StorSimple Upravitelj servisa za upravljanje količine (ažuriranje 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča objašnjava kako pomoću servisa StorSimple Upravitelj za stvaranje i upravljanje količine na uređaju StorSimple i StorSimple virtualnog uređaja s instaliran ažuriranje 2.

Upravitelj StorSimple servis je datotečni nastavak Azure klasični portalu omogućuje upravljanje rješenje StorSimple putem sučelja Jednostruko web-mjesto. Uz upravljanje količine StorSimple Upravitelj servisa možete koristiti za stvaranje i upravljanje uslugama za StorSimple, prikaz i upravljanje uređajima, pregled upozorenja, i prikaz i upravljanje sigurnosne pravilnike i katalog sigurnosne kopije.

## <a name="volume-types"></a>Jedinica vrsta

StorSimple količine mogu biti:

- **Lokalno prikvačene količine**: podaci u ove jedinice ostaju na lokalnim uređajem StorSimple cijelo vrijeme.
- **Tiered količine**: podataka u ove jedinice možete spill u oblak.

Arhiviranje glasnoću je vrsta tiered jedinice. Veću veličinu bloka Poništavanje duplikacije koristi za arhiviranje količine omogućuje uređaj da biste prenijeli veće segmenata podataka s oblakom. 

Ako je potrebno, možete promijeniti glasnoću vrste iz lokalni tiered ili iz tiered lokalnih. Dodatne informacije, prijeđite na odjeljak [Promjena vrste glasnoću](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Lokalno prikvačene jedinicama

Lokalno prikvačene količine su potpuno dodijeljenu količine koje ne sloju podatke u oblak, time osiguravanje lokalne jamčiti za primarni podatke, neovisno o povezivanje oblaka. Podaci na lokalno prikvačene količine nije deduplicated i komprimirane; Međutim, su deduplicated snimke lokalno prikvačene količine. 

Lokalno prikvačene količine su potpuno dodjeli; Dakle, morate imati dovoljno prostora na uređaju kada ih stvarate. Možete Dodjela lokalno prikvačene jedinicama do maksimalne veličine 8 TB na uređaju StorSimple 8100 i 20 TB na uređaju 8600. StorSimple rezervira preostale lokalne prostora na uređaju za snimke, metapodataka i obrada podataka. Možete povećati veličinu lokalno prikvačene jedinice Maksimalna dostupan prostor, ali ne smanjite veličinu jedinica jednom stvorena.

Kada stvorite lokalno prikvačene glasnoće, dostupnog prostora za stvaranje tiered količine se smanjuje. Obrnuto vrijedi i: Ako imate postojeću tiered količine, prostor dostupan za stvaranje lokalno prikvačene količine biti manja od Maksimalna ograničenja naveden iznad. Dodatne informacije o lokalnom količine odnose se na [Najčešća pitanja na lokalno prikvačene količine](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Tiered jedinice

Tiered jedinicama su thinly dodijeljenu jedinicama u kojima često pristupa podacima ostaje lokalne na uređaju i manje često korištenih podataka automatski tiered u oblak. Tanki dodjeljivanje je tehnologija virtualizacije u kojem se dostupan prostor za pohranu pojavljuje premašuje fizičke resurse. Umjesto unaprijed rezervirati dovoljno prostora za pohranu, StorSimple koristi Tanki dodjele resursa želite dodijeliti samo dovoljno prostora za zadovoljava preduvjete za trenutni. Elastic prirode za pohranu u oblaku olakšava takvog jer StorSimple možete povećati ili smanjiti za pohranu u oblaku da bi odgovarao zahtjeva za promjenom.

Ako koristite tiered jedinica za arhiviranje podataka, potvrdite okvir **koristi ovu jedinicu za manje često pristupa arhiviranje podataka** mijenja veličinu bloka Poništavanje duplikacije glasnoću 512 KB. Ako ste odabrali ovu mogućnost, odgovarajući tiered glasnoću koristit će bloka veličine od 64 KB. Veća Poništavanje duplikacije bloka omogućuje uređaj da biste ubrzali prijenos velikih arhiviranje podataka u oblak.

>[AZURE.NOTE] Arhiviranje količine stvorenih prije ažuriranja 2 verziju StorSimple bit će uvezeni kao tiered s arhiviranje poništavati potvrdni okvir.

### <a name="provisioned-capacity"></a>Dodjela resursa kapaciteta

Pogledajte u sljedećoj su tablici kapaciteta Maksimalna Dodjela resursa za svaku vrstu uređaja i glasnoću. (Imajte na umu da lokalno prikvačene količine nisu dostupne na virtualnog uređaja).

|             | Veličina Maksimalna tiered jedinice | Lokalno najviše prikvačene veličina jedinice |
|-------------|----------------------------|------------------------------------|
| **Fizičkih uređaja** |       |       |
| 8100                 | 64 TB | 8 TB |
| 8600                 | 64 TB | 20 TB |
| **Virtualna uređaja**  |       |       |
| 8010                | 30 TB | N/D   |
| 8020               | 64 TB | N/D   |

## <a name="the-volumes-page"></a>Količine stranice

Stranici **količine** omogućuje upravljanje količine prostora za pohranu koji su dodjeli na uređaju Microsoft Azure StorSimple za Pokretači (poslužitelji). Prikazuje popis količine na uređaju StorSimple.

 ![Količine stranice](./media/storsimple-manage-volumes-u2/VolumePage.png)

Jedinica sastoji se od skupa atributima:

- **Naziv glasnoću** – opisni naziv mora biti jedinstven i olakšava prepoznavanje glasnoću. Taj naziv koristi se u nadzorni izvješća Kad filtrirate na određene jedinicu.

- **Status** – može biti mrežnog i izvanmrežnog. Ako jedinica je izvan mreže, nije vidljiv Pokretači (poslužitelji) dopušten pristup za korištenje glasnoću.

- **Kapacitet** – određuje ukupnu količinu podataka koje se mogu pohraniti pokretač (server). Lokalno prikvačene količine potpuno dodjeli i nalaze na uređaju StorSimple. Tiered količine su thinly dodjeli, a zatim je deduplicated podatke. S thinly dodijeljenu količine uređaj ne unaprijed dodijeliti fizičke kapaciteta interno ili na oblaka prema kapaciteta konfigurirani glasnoću. Kapacitet glasnoću je dodijeliti i Potrošena na zahtjev.

- **Vrsta** – označava je li glasnoću **Tiered** (Zadana postavka) ili **lokalno prikvačite**.

- **Sigurnosno kopiranje** – pokazuje postoji li zadane sigurnosne kopije pravila za jedinicu.

- **Pristup** – određuje Pokretači (poslužitelji) dopušten pristup jedinicu. Pokretači koje nisu članovi zapis za kontrolu pristup (ACR) koji je pridružen glasnoću nećete vidjeti glasnoću.

- **Nadzor** – određuje hoće li je u tijeku nadzirati jedinicu. Jedinice će imati nadzor po zadanom omogućena prilikom stvaranja. Nadzor će, međutim, biti onemogućen zbog glasnoću Kloniraj. Da biste omogućili nadzor za jedinicu, slijedite upute u [Monitor jedinicu](#monitor-a-volume). 

Poslužite se uputama u ovom ćete praktičnom vodiču za izvođenje sljedećih zadataka:

- Dodavanje jedinica 
- Izmjena jedinica 
- Promjena vrste glasnoće
- Brisanje jedinica 
- Izvanmrežni jedinica 
- Praćenje jedinica 

## <a name="add-a-volume"></a>Dodavanje jedinica

[Stvara jedinicu](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) tijekom implementacije rješenja za StorSimple. Dodavanje jedinice je slično postupak.

#### <a name="to-add-a-volume"></a>Da biste dodali jedinica

1. Na stranici **uređaja** odaberite uređaj, dvokliknite ga i pa kliknite karticu **Spremnika glasnoću** .

2. Na popisu odaberite spremnik glasnoću i dvokliknite je da biste pristupili jedinicama pridružene spremnik.

3. Kliknite **Dodaj** pri dnu stranice. Dodavanje pokreće se čarobnjak za jedinicu.

     ![Dodavanje čarobnjakom za jedinicu osnovne postavke](./media/storsimple-manage-volumes-u2/TieredVolEx.png)

4. U odjeljku Dodavanje čarobnjakom za jedinicu, u odjeljku **Osnovne postavke**, učinite sljedeće:

  1. Navedite **naziv** glasnoću.
  2. Odaberite **Vrstu korištenje** s padajućeg popisa. Za radnih opterećenja koje su potrebni podaci biti dostupan lokalno na uređaj cijelo vrijeme, odaberite **Lokalno prikvačene**. Za sve ostale vrste podataka, odaberite **Tiered**. (**Tiered** je zadana postavka).
  3. Ako ste odabrali **Tiered** u koraku 2, možete odabrati potvrdni okvir **koristi ovu jedinicu za manje često pristupa arhiviranje podataka** da biste konfigurirali arhiviranje glasnoću.
  4. Unesite **Dodjeli kapaciteta** glasnoću GB ili TB. Maksimalna veličina za svaku vrstu uređaja i glasnoću potražite u članku [Provisioned kapaciteta](#provisioned-capacity) . Pregled **Dostupnog kapaciteta** da biste odredili koliko prostora za pohranu je dostupan na uređaju.

5. Kliknite ikonu sa strelicom![Ikona strelice](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Ako konfigurirate lokalno prikvačene glasnoće, prikazat će se sljedeća poruka.

    ![Promjena jedinica vrste poruka](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
   
5. Kliknite ikonu sa strelicom ![ikona strelice](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)da biste se na stranici **Dodatne postavke** .

    ![Dodavanje glasnoću Čarobnjak za dodatne postavke](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>

6. U odjeljku **Dodatne postavke**, dodavanje novog zapisa pristup kontrole (ACR):
  
  1. Odaberite zapis kontrole pristupa (ACR) s padajućeg popisa. Osim toga, možete dodati nove ACR. ACRs odrediti koje domaćini možete pristupiti diskova voditelju IQN koji se podudaraju s tim naveden u zapisu. Ako ne navedete programa ACR, prikazat će se sljedeća poruka.

        ![Odredite ACR](./media/storsimple-manage-volumes-u2/SpecifyACR.png)

  2. Preporučujemo da odaberete potvrdni okvir **Omogući zadane sigurnosne kopije za ovu jedinicu** .
  3. Kliknite ikonu provjeri ![Ikonu Provjera](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Da biste stvorili glasnoću navedeni postavke.

Nova jedinica je spremna za korištenje.

>[AZURE.NOTE] Ako stvorite lokalno prikvačene jedinicu, a zatim stvorite drugi lokalno prikvačene jedinicu odmah nakon toga stvaranja glasnoću poslovi sekvencijalno pokreću. Prvi zadatak stvaranja glasnoću morate završiti prije sljedeći zadatak stvaranja glasnoću možete početi.

## <a name="modify-a-volume"></a>Izmjena jedinica

Izmjena jedinice kada je potrebno da biste proširili ili promijenili domaćini kojoj pristup glasnoću.

> [AZURE.IMPORTANT] 
>
> - Ako mijenjate veličinu glasnoće na uređaju, veličina jedinice morate promijeniti na glavnom računalu kao i. 
> - Na drugo glavno računalo ovdje opisani su koraci za Windows Server 2012 (2012R2). Razlikovat će se postupci Linux ili drugi operacijski sustavi glavnog računala. Pogledajte upute za operacijski sustav glavno računalo prilikom izmjene glasnoće na računalu koje hostira drugi operacijski sustav. 

#### <a name="to-modify-a-volume"></a>Da biste izmijenili jedinica

1. Na stranici **uređaja** odaberite uređaj, dvokliknite ga i pa kliknite karticu **Spremnika glasnoću** .

2. Na popisu odaberite spremnik glasnoću i dvokliknite je da biste pogledali količine pridružene spremnik.

3. Odaberite jedinicu, a pri dnu stranice kliknite **Izmijeni**. Pokreće se čarobnjak za količinsko Izmijeni.

4. U čarobnjaku jedinicu izmjena, u odjeljku **Osnovne postavke**možete učiniti sljedeće:

  - Uredite **naziv**.
  - Pretvorba **Vrste korištenje** iz lokalno prikvačene na tiered ili iz tiered za lokalno prikvačene (pogledajte članak [Promjena jedinica vrsta](#change-the-volume-type) dodatne informacije).
  - Povećanje **dodjeli kapaciteta**. **Dodjeli kapaciteta** samo se povećati. Ne možete smanjiti jedinicu nakon stvaranja.

5. U odjeljku **Dodatne postavke**možete izmijeniti ACR, pod uvjetom da je izvan mreže. Ako je glasnoće na Internetu, morat ćete izvanmrežni je najprije. Pogledajte korake u [poduzeti glasnoću izvanmrežno](#take-a-volume-offline) prije izmjena na ACR.

    > [AZURE.NOTE] Nije moguće promijeniti mogućnost **Omogući zadane sigurnosne kopije** za jedinicu.

6. Spremite promjene tako da kliknete ikonu Provjera ![ikonu Provjera](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). Azure klasični portal će prikazati poruka o ažuriranju glasnoću. Poruka o uspjehu prikazivanja kada glasnoću uspješno su ažurirani.

7. Ako su Proširivanje jedinice, slijedite sljedeće korake na računalu sa sustavom Windows glavnog računala:

   1. Otvorite **Upravljanje računalom** ->**Disk Management**.
   2. Desnom tipkom miša kliknite **Upravljanje Disk** , a zatim odaberite **Ponovni pregled diskova**.
   3. Na popisu diskova odaberite jedinicu koju ste ažurirali, desnom tipkom miša i odaberite **Jedinica za proširivanje**. Pokreće se čarobnjak za proširivanje jedinica. Kliknite **Dalje**.
   4. Dovršite Čarobnjak za prihvaćanje zadane vrijednosti. Nakon što završite čarobnjak, glasnoću treba prikazati veću veličinu.

    >[AZURE.NOTE] Ako proširite lokalno prikvačene glasnoće, a zatim proširite drugi lokalno odmah nakon toga pokrenuti poslovi proširenja glasnoću sekvencijalno prikvačena glasnoću. Prvi posao proširenja za količinsko morate završiti prije sljedeći posao proširenja glasnoću možete početi.

![Videozapis dostupne](./media/storsimple-manage-volumes-u2/Video_icon.png) **videozapis dostupna**

Da biste pogledali videozapis u kojem se pokazuje kako da biste proširili jedinicu, kliknite [ovdje](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>Promjena vrste glasnoće

Možete promijeniti vrstu jedinice iz tiered za lokalno prikvačene ili iz lokalno prikvačena na tiered. Međutim, pretvorba mora biti Česti pojavljivanje. Razlozi za pretvaranje jedinicu s tiered za lokalno prikvačene su:

- Lokalni jamstva vezanim uz dostupnost podataka i performanse
- Uklanjanja latencies oblaka i problema s povezivanjem oblaka.

Obično su small postojeće jedinice koje želite pristupiti često. Lokalno prikvačene glasnoću je potpuno dodjeli prilikom stvaranja. Ako pretvarate tiered glasnoću lokalno prikvačene jedinicu StorSimple potvrđuje da imate li dovoljno prostora na uređaju prije pokretanja pretvorbe. Ako imate dovoljno prostora, primite poruku o pogrešci, a postupak će se otkazati. 

> [AZURE.NOTE] Prije nego počnete pretvorbe iz tiered za lokalno prikvačene, provjerite je li razmislite o potreban prostor na vašem drugih radnih opterećenja. 

Možda ćete morati promijeniti lokalno prikvačene glasnoću tiered jedinicu ako vam je potrebna dodatan prostor Dodjela druge količine. Kada pretvorite lokalno prikvačene jedinice tiered dostupan kapacitet na uređaju povećava po veličini objavljenu kapaciteta. Ako problema s povezivanjem onemogućuju pretvorbe jedinice iz lokalne vrste tiered vrsti, lokalnu jedinicu imati svojstva tiered jedinice dok se ne pretvorbe. To znači da neki podaci možda ste spilled u oblak. Ove podatke spilled će i dalje proširiti lokalne prostora na uređaju koji ne može osloboditi sve dok ponovno pokrenuti i dovršiti postupak.

>[AZURE.NOTE] Pretvorba jedinica može potrajati neko vrijeme, a ne možete poništiti pretvorbe nakon pokretanja. Glasnoću ostaje online prilikom pretvorbe, i koje možete poduzeti sigurnosne kopije, ali nije moguće proširiti ili vratiti glasnoće tijekom pretvorbe.  

Pretvorbe iz programa tiered lokalno prikvačene jedinicu mogu negativno utjecati na performanse uređaja. Uz to, sljedeće čimbenike mogu povećati na vrijeme potrebno da biste dovršili pretvorbe:

- Postoji nedovoljna propusnost veze.

- Postoji bez trenutno sigurnosno kopiranje.

Da biste minimizirali efekte koje možda sljedećih čimbenika:

- Pregledajte svoje propusnosti ograničavanje pravila i provjerite je li namjenski 40 MB/s propusnosti dostupna.
- Zakazivanje pretvorbe slabog opterećenja Budući sata.
- Stvorite snimku oblaka prije nego što počnete pretvorbe.

Ako pretvarate više jedinica (podrške različitih radnih opterećenja), zatim koje prioritet pretvorbe glasnoću tako da se veće količine prioritet pretvorit će se prvi put. Ako, na primjer, trebali biste pretvorite jedinice u kojima su hostirani virtualnim strojevima (VMs) ili količine s radnih opterećenja sustava SQL prije pretvaranja količine s radnih opterećenja zajedničko korištenje datoteka.

#### <a name="to-change-the-volume-type"></a>Da biste promijenili glasnoću vrste

1. Na stranici **uređaja** odaberite uređaj, dvokliknite ga i pa kliknite karticu **Spremnika glasnoću** .

2. Na popisu odaberite spremnik glasnoću i dvokliknite je da biste pogledali količine pridružene spremnik.

3. Odaberite jedinicu, a pri dnu stranice kliknite **Izmijeni**. Pokreće se čarobnjak za količinsko Izmijeni.

4. Na stranici **Osnovne postavke** promijeniti vrstu korištenje tako da odaberete Nova vrsta s padajućeg popisa **Vrsta korištenje** .

    - Ako mijenjate vrstu **lokalno prikvačene**, StorSimple Provjerite postoji li dovoljno kapaciteta.
    - Ako želite promijeniti vrstu za **Tiered** i jedinica će se koristiti za arhiviranje podataka, odaberite potvrdni okvir **koristi ovu jedinicu za manje često pristupa arhiviranje podataka** .

        ![Potvrdni okvir arhiviranje](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)

5. Kliknite ikonu sa strelicom ![ikonu strelice](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) da biste otvorili stranicu **Dodatne postavke** . Ako konfigurirate lokalno prikvačene glasnoće, pojavljuje se sljedeća poruka.

    ![Promjena jedinica vrsta poruka](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)

6. Kliknite ikonu sa strelicom ![Ikona strelice](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) Da biste nastavili.

7. Kliknite ikonu provjeri ![Ikonu Provjera](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Da biste pokrenuli postupak pretvorbe. Azure portal će prikazati poruka o ažuriranju glasnoću. Poruka o uspjehu prikazivanja kada glasnoću uspješno su ažurirani.

## <a name="take-a-volume-offline"></a>Izvanmrežni jedinica

Možda ćete morati izvanmrežni jedinice prilikom planiranja ga izmijeniti ili ih izbrisati. Kada jedinica je izvan mreže, nije dostupna za čitanje i pisanje pristup. Morat ćete poduzeti izvanmrežno glasnoće na glavnom računalu, kao i na uređaju. 

#### <a name="to-take-a-volume-offline"></a>Da biste izvanmrežni jedinica

1. Provjerite glasnoću dotičnog nije li koristi prije poduzimanja je izvan mreže.

2. Iskoristite glasnoće izvanmrežno na glavno računalo prvi. Time se uklanja potencijalne opasnosti od oštećenja podataka na jedinici. Za određene korake, pogledajte upute za vaš operacijski sustav glavnog računala.

3. Glavno računalo je izvan mreže, proći glasnoće na uređaju izvan mreže pomoću sljedećih koraka:

  1. Na stranici **uređaja** odaberite uređaj, dvokliknite ga i pa kliknite karticu **Spremnika glasnoću** . Na kartici **Glasnoću spremnika** navedeni u tabličnom obliku spremnika glasnoću koje su vezane uz uređaj.
  2. Kontejner glasnoću i kliknite je da bi se prikazao popis svih jedinica unutar spremnik.
  3. Odaberite jedinicu, a zatim kliknite **Preuzimanje izvanmrežnog**.
  4. Kada se zatraži potvrda, kliknite **da**. Glasnoću sada bi trebala biti izvan mreže.

    Nakon jedinica je izvan mreže, mogućnost **Premjesti Online** postaje dostupan.

> [AZURE.NOTE] **Preuzimanje izvanmrežnog** naredba šalje zahtjev uređaju da biste izvanmrežni glasnoću. Ako domaćini još uvijek koriste glasnoće, rezultat prekida veze, ali izvanmrežno poduzimanja glasnoću neće uspjeti. 

## <a name="delete-a-volume"></a>Brisanje jedinica

> [AZURE.IMPORTANT] Jedinice možete izbrisati samo ako je izvan mreže.

Provedite sljedeće korake da biste izbrisali jedinicu.

#### <a name="to-delete-a-volume"></a>Da biste izbrisali jedinica

1. Na stranici **uređaja** odaberite uređaj, dvokliknite ga i pa kliknite karticu **Spremnika glasnoću** .

2. Odaberite kontejner glasnoću koja ima jedinicu koju želite izbrisati. Kliknite kontejner jedinice da biste pristupili stranici **količine** .

3. Sve jedinice povezane s ovom spremniku prikazuju se u tabličnom obliku. Provjerite status jedinicu koju želite izbrisati. Ako jedinicu koju želite izbrisati je izvan mreže, izvanmrežni je najprije slijedite korake iz [poduzeti glasnoću izvanmrežno](#take-a-volume-offline).

4. Kada je izvan mreže, kliknite **Izbriši** pri dnu stranice.

5. Kada se zatraži potvrda, kliknite **da**. Glasnoću sada izbrisat će se i **količine** stranici bit će vidljivo ažurirani popis količine unutar spremnik.

    >[AZURE.NOTE]Ako izbrišete lokalno prikvačene glasnoće, prostora za nove jedinice možda se neće ažurirati odmah. Servis za upravitelja StorSimple povremeno ažurira lokalne dostupnom prostoru. Predlažemo da Pričekajte nekoliko minuta prije no što pokušate stvoriti novu jedinicu.<br> Osim toga, ako izbrišete lokalno prikvačene glasnoću i izbrisati drugi lokalno prikvačena glasnoću odmah nakon toga brisanja glasnoću poslovi sekvencijalno pokreću. Prvi posao brisanja glasnoću morate završiti prije sljedeći posao brisanja glasnoću možete početi.
 
## <a name="monitor-a-volume"></a>Praćenje jedinica

Nadzor glasnoću omogućuje prikupljanje li/O-povezane Statistika za jedinicu. Nadzor po zadanom je omogućena za 32 količine koje ste stvorili. Nadzor dodatne jedinica onemogućen je prema zadanim postavkama. Nadzor kloniranu jedinica također je onemogućen prema zadanim postavkama.

Izvršite sljedeće korake da biste omogućili ili onemogućili nadzor za jedinicu.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Omogućivanje i onemogućivanje nadzor glasnoće

1. Na stranici **uređaja** odaberite uređaj, dvokliknite ga i pa kliknite karticu **Spremnika glasnoću** .

2. Odaberite kontejner jedinicu u kojoj se nalazi glasnoće, a zatim spremnik jedinice da biste pristupili stranici **količine** .

3. Sve jedinice povezane s ovom spremniku navedene su u tabličnom prikazu. Kliknite i odaberite glasnoću ili Kloniraj glasnoću.

4. Pri dnu stranice kliknite **Izmijeni**.

5. U čarobnjaku za izmjenu glasnoću, u odjeljku **Osnovne postavke**odaberite **Omogućivanje** i **Onemogućivanje** s padajućeg popisa **praćenja** .

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [Kloniraj StorSimple jedinicu](storsimple-clone-volume.md).

- Saznajte kako [pomoću upravitelja StorSimple servisa za administraciju uređaju StorSimple](storsimple-manager-service-administration.md).

 
