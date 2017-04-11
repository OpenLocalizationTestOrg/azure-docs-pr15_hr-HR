<properties
   pageTitle="Upravljanje diskova StorSimple | Microsoft Azure"
   description="U članku se objašnjava kako dodati izmjena, praćenje i brisanje StorSimple količine i kako izvanmrežni ih po potrebi."
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
   ms.date="05/11/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>Korištenje upravitelja StorSimple servisa za upravljanje količine

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča objašnjava kako pomoću servisa StorSimple Upravitelj za stvaranje i upravljanje količine na uređaju StorSimple i StorSimple virtualnog uređaja.

Upravitelj StorSimple servis je datotečni nastavak Azure klasični portal omogućuje upravljanje rješenje StorSimple putem sučelja Jednostruko web-mjesto. Uz upravljanje količine StorSimple Upravitelj servisa možete koristiti za stvaranje i upravljanje uslugama za StorSimple, prikaz i upravljanje uređajima, pregled upozorenja, i prikaz i upravljanje sigurnosne pravilnike i katalog sigurnosne kopije.

> [AZURE.NOTE] Azure StorSimple možete stvoriti thinly dodijeljenu količine. Ne možete stvoriti potpuno dodijeljenu ili djelomično dodijeljenu jedinicama na sustavu Azure StorSimple.
>
> Tanki dodjeljivanje je tehnologija virtualizacije u kojem se dostupan prostor za pohranu pojavljuje premašuje fizičke resurse. Umjesto unaprijed rezervirati dovoljno prostora za pohranu Azure StorSimple koristi Tanki dodjele resursa želite dodijeliti samo dovoljno prostora za zadovoljava preduvjete za trenutni. Elastic prirode za pohranu u oblaku olakšava takvog jer Azure StorSimple možete povećati ili smanjiti za pohranu u oblaku da bi odgovarao zahtjeva za promjenom.

## <a name="the-volumes-page"></a>Količine stranice

Stranici **količine** omogućuje upravljanje količine prostora za pohranu koji su dodjeli na uređaju Microsoft Azure StorSimple za Pokretači (poslužitelji). Prikazuje popis količine na uređaju StorSimple.

 ![Količine stranice](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

Jedinica sastoji se od skupa atributima:

- **Naziv** – opisni naziv mora biti jedinstven i omogućuje otkrivanje glasnoću. Taj naziv koristi se u nadzorni izvješća Kad filtrirate na određene jedinicu.

- **Status** – može biti mrežnog i izvanmrežnog. Ako jedinica ako je izvan mreže, nije vidljiv Pokretači (poslužitelji) dopušten pristup za korištenje glasnoću.

- **Kapacitet** – određuje veličinu glasnoće, kao što je izgledati po pokretač (server). Kapacitet određuje ukupnu količinu podataka koje se mogu pohraniti pokretač (server). Količine su thinly dodjeli pa je deduplicated podataka. To podrazumijeva da uređaj ne unaprijed dodijeliti fizičke kapaciteta interno ili na oblaka prema kapaciteta konfigurirani glasnoću. Kapacitet glasnoću je dodijeliti i Potrošena na zahtjev.

- **Vrsta** – jedinica vrsta može biti tiered ili arhiviranje (podređene vrste tiered)

- **Pristup** – određuje Pokretači (poslužitelji) dopušten pristup jedinicu. Pokretači koje nisu članovi zapis za kontrolu pristup (ACR) koji je pridružen glasnoću nećete vidjeti glasnoću.

- **Nadzor** – određuje hoće li je u tijeku nadzirati jedinicu. Jedinice će imati nadzor po zadanom omogućena prilikom stvaranja. Nadzor će, međutim, biti onemogućen zbog glasnoću Kloniraj. Da biste omogućili nadzor za jedinicu, slijedite upute u Monitor jedinicu.

Uobičajene zadatke povezane s jedinice su:

- Dodavanje jedinica
- Izmjena jedinica
- Brisanje jedinica
- Izvanmrežni jedinica
- Praćenje jedinica

## <a name="add-a-volume"></a>Dodavanje jedinica

[Stvara jedinicu](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) tijekom implementacije rješenja za StorSimple. Dodavanje jedinice je slično postupak.

### <a name="to-add-a-volume"></a>Da biste dodali jedinica

1. Na stranici **uređaja** odaberite uređaj, dvokliknite ga i pa kliknite karticu **Spremnika glasnoću** .

2. Kontejner glasnoće, a zatim kliknite strelicu u odgovarajućem retku da biste pristupili količine pridružene spremnik.

3. Kliknite **Dodaj** pri dnu stranice. Dodavanje pokreće se čarobnjak za jedinicu.

     ![Dodavanje čarobnjakom za jedinicu osnovne postavke](./media/storsimple-manage-volumes/AddVolume1.png)

4. U odjeljku Dodavanje čarobnjakom za jedinicu, u odjeljku **Osnovne postavke**, učinite sljedeće:

  1. Navedite **naziv** glasnoću.
  2. Navedite **Dodjeli kapaciteta** glasnoću GB ili TB. Kapacitet mora biti između 1 GB i 64 TB za fizički uređaj. Maksimalna kapacitet koja može biti dodijeljena glasnoću na StorSimple virtualnog uređaja je 30 TB.
  3. Odaberite **Vrstu korištenje** glasnoću. Ako koristite tiered jedinica za arhiviranje podataka, potvrdite okvir **koristi ovu jedinicu za manje često pristupa arhiviranje podataka** mijenja veličinu bloka Poništavanje duplikacije glasnoću 512 KB. Ako ste odabrali ovu mogućnost, odgovarajući tiered glasnoću koristit će bloka veličine od 64 KB. Veća Poništavanje duplikacije bloka omogućuje uređaj da biste ubrzali prijenos velikih arhiviranje podataka u oblak. (Tiered količine su prije se zvao primarni količine.)
  5. Kliknite ikonu sa strelicom ![ikonu strelice](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)da biste otvorili stranicu **Dodatne postavke** .

        ![Dodavanje glasnoću Čarobnjak za dodatne postavke](./media/storsimple-manage-volumes/AddVolume2.png)

5. U odjeljku **Dodatne postavke**, dodavanje novog zapisa pristup kontrole (ACR):

  1. Odaberite zapis kontrole pristupa (ACR) s padajućeg popisa. Osim toga, možete dodati nove ACR. ACRs odrediti koje domaćini možete pristupiti diskova voditelju IQN koji se podudaraju s tim naveden u zapisu.
  2. Preporučujemo da omogućite zadane sigurnosne kopije tako da potvrdite okvir **Omogući zadane sigurnosne kopije za ovu jedinicu** .
   3. Kliknite ikonu provjeri ![Ikonu Provjera](./media/storsimple-manage-volumes/HCS_CheckIcon.png) Da biste stvorili glasnoću navedeni postavke.

Nova jedinica je spremna za korištenje.

## <a name="modify-a-volume"></a>Izmjena jedinica

Izmjena jedinice kada je potrebno da biste proširili ili promijenili domaćini kojoj pristup glasnoću.

> [AZURE.IMPORTANT]
>
> - Ako mijenjate veličinu glasnoće na uređaju, veličina jedinice morate promijeniti na glavnom računalu kao i.
> - Na drugo glavno računalo ovdje opisani su koraci za Windows Server 2012 (2012R2). Razlikovat će se postupci Linux ili drugi operacijski sustavi glavnog računala. Pogledajte upute za operacijski sustav glavno računalo prilikom izmjene glasnoće na računalu koje hostira drugi operacijski sustav.

### <a name="to-modify-a-volume"></a>Da biste izmijenili jedinica

1. Na stranici **uređaja** odaberite uređaj, dvokliknite ga i pa kliknite karticu **Spremnik glasnoću** . Ova stranica daje popis u tabličnom obliku spremnika glasnoću koje su vezane uz uređaj.

2. Kontejner glasnoću i kliknite je da bi se prikazao popis svih jedinica unutar spremnik.

3. Na stranici **količine** odaberite jedinicu, a zatim kliknite **Izmijeni**.

4. U čarobnjaku jedinicu izmjena, u odjeljku **Osnovne postavke**možete učiniti sljedeće:

  - Ako želite da biste izmijenili tiered glasnoću arhiviranje jedinicu tako da potvrdite okvir **koristi ovu jedinicu za manje često pristupa arhiviranje podataka** da biste promijenili veličinu bloka Poništavanje duplikacije glasnoću 512 KB uređivati **naziv** i **Vrsta** .
  - Povećanje **dodjeli kapaciteta**. **Dodjeli kapaciteta** samo se povećati. Ne možete smanjiti jedinicu nakon stvaranja.

    > [AZURE.NOTE] Spremnik jedinicu nije moguće promijeniti nakon što vam je dodijeljen jedinicu.

5. U odjeljku **Dodatne postavke**, možete učiniti sljedeće:

  - Izmjena ACRs, pod uvjetom da je izvan mreže. Ako je glasnoće na Internetu, morat ćete izvanmrežni je najprije. Pogledajte korake u [poduzeti glasnoću izvanmrežno](#take-a-volume-offline) prije izmjena na ACR.
  - Izmjena popisa ACRs nakon glasnoću je izvan mreže.

    > [AZURE.NOTE] Nije moguće promijeniti mogućnost **Omogući zadane sigurnosne kopije za ovu jedinicu** za jedinicu.

6. Spremite promjene tako da kliknete ikonu Provjera ![ikonu Provjera](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Azure klasični portal će prikazati poruka o ažuriranju glasnoću. Poruka o uspjehu prikazivanja kada glasnoću uspješno su ažurirani.

7. Ako su Proširivanje jedinice, slijedite sljedeće korake na računalu sa sustavom Windows glavnog računala:

   1. Otvorite **Upravljanje računalom** ->**Disk Management**.
   2. Desnom tipkom miša kliknite **Upravljanje Disk** , a zatim odaberite **Ponovni pregled diskova**.
   3. Na popisu diskova odaberite jedinicu koju ste ažurirali, desnom tipkom miša i odaberite **Jedinica za proširivanje**. Pokreće se čarobnjak za proširivanje jedinica. Kliknite **Dalje**.
   4. Dovršite Čarobnjak za prihvaćanje zadane vrijednosti. Nakon što završite čarobnjak, glasnoću treba prikazati veću veličinu.

![Videozapis dostupne](./media/storsimple-manage-volumes/Video_icon.png) **videozapis dostupna**

Da biste pogledali videozapis u kojem se pokazuje kako da biste proširili jedinicu, kliknite [ovdje](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>Izvanmrežni jedinica

Možda ćete morati izvanmrežni jedinice prilikom planiranja ga izmijeniti ili ih izbrisati. Kada jedinica je izvan mreže, nije dostupna za čitanje i pisanje pristup. Morat ćete poduzeti izvanmrežno glasnoće na glavnom računalu, kao i na uređaju. Izvršite sljedeće korake da biste izvanmrežni jedinicu.

### <a name="to-take-a-volume-offline"></a>Da biste izvanmrežni jedinica

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

### <a name="to-delete-a-volume"></a>Da biste izbrisali jedinica

1. Na stranici **uređaja** odaberite uređaj, dvokliknite ga i pa kliknite karticu **Spremnika glasnoću** .

2. Odaberite kontejner glasnoću koja ima jedinicu koju želite izbrisati. Kliknite kontejner jedinice da biste pristupili stranici **količine** .

3. Sve jedinice povezane s ovom spremniku prikazuju se u tabličnom obliku. Provjerite status jedinicu koju želite izbrisati. Ako jedinicu koju želite izbrisati je izvan mreže, izvanmrežni je najprije slijedite korake iz [poduzeti glasnoću izvanmrežno](#take-a-volume-offline).

4. Kada je izvan mreže, kliknite **Izbriši** pri dnu stranice.

5. Kada se zatraži potvrda, kliknite **da**. Glasnoću sada izbrisat će se i **količine** stranici bit će vidljivo ažurirani popis količine unutar spremnik.

## <a name="monitor-a-volume"></a>Praćenje jedinica

Nadzor glasnoću omogućuje prikupljanje li/O-povezane Statistika za jedinicu. Nadzor po zadanom je omogućena za 32 količine koje ste stvorili. Nadzor dodatne jedinica onemogućen je prema zadanim postavkama. Nadzor kloniranu jedinica također je onemogućen prema zadanim postavkama.

Izvršite sljedeće korake da biste omogućili ili onemogućili nadzor za jedinicu.

### <a name="to-enable-or-disable-volume-monitoring"></a>Omogućivanje i onemogućivanje nadzor glasnoće

1. Na stranici **uređaja** odaberite uređaj, dvokliknite ga i pa kliknite karticu **Spremnika glasnoću** .

2. Odaberite kontejner jedinicu u kojoj se nalazi glasnoće, a zatim spremnik jedinice da biste pristupili stranici **količine** .

3. Sve jedinice povezane s ovom spremniku navedene su u tabličnom prikazu. Kliknite i odaberite glasnoću ili Kloniraj glasnoću.

4. Pri dnu stranice kliknite **Izmijeni**.

5. U čarobnjaku za izmjenu glasnoću, u odjeljku **Osnovne postavke**odaberite **Omogućivanje** i **Onemogućivanje** s padajućeg popisa **praćenja** .

    ![Izmjena glasnoću osnovne postavke](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)


## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [Kloniraj StorSimple jedinicu](storsimple-clone-volume.md).

- Saznajte kako [koristiti StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).
