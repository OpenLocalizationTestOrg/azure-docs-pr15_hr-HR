<properties
   pageTitle="Pregledavanje i upravljanje resursima za pohranu pomoću programa Explorer poslužitelja | Microsoft Azure"
   description="Pregledavanje i upravljanje resursima za pohranu pomoću programa Explorer poslužitelja"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Pregledavanje i upravljanje resursima za pohranu pomoću programa Explorer poslužitelja

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Pregled
Ako ste instalirali Azure alata za Microsoft Visual Studio, možete pogledati blob, reda čekanja i tablica s podacima iz računa za pohranu za Azure. Čvor Azure prostora za pohranu u programu Explorer poslužitelja prikazuje podatke u svoj račun emulator lokalno spremište i drugih računa Azure prostora za pohranu.

Da biste pogledali Explorer poslužitelja u Visual Studio, na traci izbornika, odaberite **Prikaz** **Poslužitelja Explorer**. Prostor za pohranu čvor prikazuju se svi računi za pohranu koji postoje ispod svakog Azure pretplate/certifikat kojima ste povezani. Ako vaš račun za pohranu ne prikazuje, možete ga dodati pomoću navedenih u uputama [u nastavku ovog članka](#add-storage-accounts-by-using-server-explorer).

Pokretanje u Azure SDK 2.7, vam može poslužiti novi Explorer oblaka za prikaz i upravljanje Azure resurse. Dodatne informacije potražite u članku [Upravljanje resursima Azure pomoću programa Explorer oblaka](./vs-azure-tools-resources-managing-with-cloud-explorer.md) .


## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Prikaz i upravljanje resursa za pohranu u Visual Studio

Poslužitelj Explorer automatski prikazuje popis blob-ova, redovima i tablice na vašem računu emulator prostora za pohranu. Emulator račun za pohranu nalazi u programu Explorer poslužitelja u odjeljku čvor prostora za pohranu kao čvor **razvoj** .

Da biste vidjeli resursa za pohranu emulator poslovnog subjekta, proširite čvor **razvoj** . Ako emulator prostora za pohranu nije pokrenut kada Proširite čvor **razvoj** , ona će se automatski pokrenuti. To može potrajati nekoliko sekundi. Možete nastaviti raditi u druga područja sustava Visual Studio dok se pokreće emulator prostora za pohranu.

Da biste pogledali resursa za pohranu računa, proširite čvor račun za pohranu u programu Explorer poslužitelja. Sljedeće podređene čvorove prikazivati:

- Blob-ova

- Reda čekanja

- Tablica

## <a name="work-with-blob-resources"></a>Rad s resursima blobova platforme

Čvor blob polja prikazuje popis spremnika za račun odabrane prostora za pohranu. Blob spremnika sadrže blob datoteke, a možete organizirati te blob-ova u mape i podmape. Dodatne informacije potražite u članku [upute za korištenje spremišta blobova iz .NET](./storage/storage-dotnet-how-to-use-blobs.md) .

### <a name="to-create-a-blob-container"></a>Stvaranje kontejnera za blobova platforme

1. Otvaranje izbornika prečaca za čvor **blob-ova** , a zatim odaberite **Stvaranje spremnika Blob**.

1. Unesite naziv novog spremnika u dijaloškom okviru **Stvaranje spremnika Blob** , a zatim odaberite **u redu**

    >[AZURE.NOTE] Naziv spremnika blob moraju započinjati broja (0 do 9) ili mala slova (a do z).

### <a name="to-delete-a-blob-container"></a>Da biste izbrisali spremniku blobova platforme

- Otvaranje izbornika prečaca za spremnik blob koji želite ukloniti, a zatim odaberite **Izbriši**.

### <a name="to-display-a-list-of-the-items-contained-in-a-blob-container"></a>Da biste prikazali popis stavki koje se nalaze u spremniku blobova platforme

- Otvaranje izbornika prečaca blob spremnik ime na popisu, a zatim odaberite **Prikaz Blob kontejner**.

    Kada pregledavate sadržaja spremnika blob, pojavljuje se na kartici poznati kao spremnik blob.

    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

    Pomoću gumba u gornjem desnom kutu prikaza spremnik blob možete izvesti sljedeće radnje na blob-ova:

    - Unesite vrijednost za filtar i primijenite ga

    - Osvježavanje popisa blob-ova u spremniku

    - Prijenos datoteke

    - Brisanje blob

      >[AZURE.NOTE] Brisanje datoteke iz spremnika blob ne briše datoteku temeljnog; samo ga uklanja iz kontejner s blob.

    - Otvorite blob

    - Spremanje blob na lokalnom računalu

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Da biste stvorili mapu i podmape u spremniku blobova platforme

1. Odaberite kontejner s blob u programu Explorer poslužitelja. U prozoru spremnik odaberite gumb **Prenesi Blob** .

    ![Prijenos datoteke u mapu blobova platforme](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

1. U dijaloškom okviru **Prijenos nove datoteke** , odaberite gumb **Pregledaj** da biste odredili datoteku koju želite prenijeti, a zatim unesite naziv mape u okvir **mapa (neobavezno)** .

    Podmape u spremniku mape možete dodati tako da slijedite isti postupak. Ako ne odredite naziv mape, datoteku moguće prenijeti na najvišoj razini spremnika blob. Datoteka će se prikazivati u određenu mapu u spremniku.

    ![Mapa dodaje spremniku blobova platforme](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Dvokliknite mapu ili pritisnite tipku ENTER da biste vidjeli sadržaj mape. Kad se nalazite u kontejner u mapi, možete se kretati natrag u korijenskom direktoriju spremnik odabirom gumba **Open Directory nadređenog** (strelica gore).

### <a name="to-delete-a-container-folder"></a>Brisanje mape spremnik

 - Izbrišite sve datoteke u mapi

    >[AZURE.NOTE] Budući da su mape u blob spremnika virtualne mape, ne možete stvoriti praznu mapu niti možete izbrisati mapu da biste izbrisali sadržaj datoteke. Imate da biste izbrisali čitav sadržaj mape da biste izbrisali mapu.

### <a name="to-filter-blobs-in-a-container"></a>Da biste filtrirali blob-ova u spremniku

Možete filtrirati blob polja koje se prikazuju navođenjem zajednički prefiks.

Ako, primjerice, unesite prefiks `hello` u tekstu filtar okvir, a zatim odaberite **izvrši** (****!) gumb, prikazuju se samo blob-ova koji počinju s "Pozdrav".

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)


>[AZURE.NOTE] Polje filtra velika i mala slova, a ne podržava filtriranje sa zamjenskim znakovima. Blob polja mogu se filtrirati prema prefiks. Ako koristite razdjelnika da biste organizirali blob-ova u hijerarhiji virtualne prefiks može obuhvaćati razdjelnika. Na primjer, filtriranje na prefiks HelloFabric / vraća sve blob-ova počevši od tog niza.

### <a name="to-download-blob-data"></a>Da biste preuzeli blob podataka

- U **Programu Explorer poslužitelja**otvaranje izbornika prečaca za jednu ili više blob-ova i zatim odaberite **Otvori**, odaberite naziv blob i zatim odaberite gumb za **Otvaranje** ili dvokliknite naziv blob.

    U prozoru **Azure zapisnik aktivnosti** prikazat će se tijeku preuzimanja blob.

    Otvorit će se blob-om u uređivaču zadana za tu vrstu datoteke. Ako operacijski sustav prepoznaje vrstu datoteke, datoteka se otvara u lokalno instalirane aplikacije; u suprotnom se od vas zatraži da biste odabrali aplikacije koja odgovara vrsti datoteke blob-om. Lokalna datoteka koja je stvorena prilikom preuzimanja blob označena je samo za čitanje.

    Predmemorirani lokalno i provjerava u odnosu na blob vrijeme zadnje izmjene u servisu Blob blob podataka. Ako blob-om je ažurirana od zadnjeg preuzimanja, ona će se preuzeti ponovno; u suprotnom blob-om će se učitati na lokalni disk. Prema zadanim postavkama blob preuzimaju se u privremenom direktoriju. Da biste preuzeli blob-ova za određeni direktorij, otvaranje izbornika prečaca za odabranu blob imena, a zatim odaberite **Spremi kao**. Kada spremite blob na taj način, blob datoteka se otvara i lokalna datoteka je stvorena pomoću atribute čitanja i pisanja.

### <a name="to-upload-blobs"></a>Da biste prenijeli blob-ova

- Odaberite gumb **Prenesi Blob** kada je otvoreno za prikaz u prikazu spremnik blob spremnik.

    Možete odabrati da biste prenijeli jednu ili više datoteka, a ne možete prenositi datoteke bilo koje vrste. **Zapisnik aktivnosti Azure** prikazuje napredak prijenos. Dodatne informacije o radu s blob podataka potražite u članku [upute za korištenje usluga spremišta blobova platforme Azure u .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>Da biste pogledali zapisnike prenijeti blob-ova

- Ako koristite Azure Dijagnostika za zapisivanje podataka iz aplikacije za Azure i zapisnika ste prenijeli na račun servisa za pohranu, vidjet ćete spremnika koje su stvorene pomoću Azure za ove zapisnika. Prikaz tih zapisnicima u programu Explorer poslužitelja jednostavan je način da biste otklonili poteškoće s aplikacijom, osobito ako je uveden u Azure. Dodatne informacije o Azure Dijagnostika potražite u članku [Prikupljanje podataka zapisivanje prema korištenjem dijagnostike Azure](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>Da biste dobili URL-a za blob

- Otvaranje izbornika prečaca u blob, a zatim odaberite **Kopiraj URL-a**.

### <a name="to-edit-a-blob"></a>Da biste uredili blob

- Odaberite blob-om, a zatim gumb **Otvori Blob** .

    Datoteka je preuzeti na privremeno mjesto i otvoriti na lokalnom računalu. Blob-om morate ponovno prenijeti nakon što napravite promjene.

## <a name="work-with-queue-resources"></a>Rad s resursima reda čekanja

Redovi servise za pohranu nalaze se u račun za Azure prostora za pohranu i pomoću njih da bi vaše oblaka uloge servisa za komunikaciju s međusobno i ostale servise po poruci prosljeđivanje mehanizam. Red čekanja možete programatski pristup kroz neki servis u oblaku i putem web-servisa za vanjski klijenti. Red čekanja možete pristupiti i izravno pomoću programa Explorer poslužitelja u Visual Studio.

Prilikom razvoja neki servis u oblaku, koji koristi redovi, možda ćete morati pomoću programa Visual Studio stvaranje redovima i raditi s njima interaktivno dok razvoj i testiranje kod.

U Explorer poslužitelja možete prikaz redova u račun za pohranu, stvaranje i redovima, otvorite red čekanja da biste prikazali njegov poruke, i dodavanje poruke u red. Kada otvorite red čekanja za prikaz, pregledavate pojedinačne poruke, a možete izvoditi sljedeće radnje u redu čekanja pomoću gumba u gornjem lijevom kutu:

- Osvježavanje prikaza reda čekanja

- Dodavanje poruke u redu čekanja

- Dequeue prvu poruku.

- Čišćenje cijele reda čekanja

Sljedeća slika prikazuje red čekanja koji sadrži dvije poruke.

![Prikaz reda čekanja](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Dodatne informacije o prostora za pohranu servisi redovi, u odjeljku [Kako: koristite servis za pohranu reda čekanja](http://go.microsoft.com/fwlink/?LinkID=264702). Informacije o web-servisa za pohranu servisa redove potražite u članku [Reda čekanja servisa koncepata](http://go.microsoft.com/fwlink/?LinkId=264788). Informacije o slanju poruka čekanja za servise za pohranu pomoću Visual Studio, potražite u članku [Slanje poruke čekanja za servise za pohranu](https://msdn.microsoft.com/library/azure/jj649344.aspx).

>[AZURE.NOTE] Redovi servise za pohranu se razlikuju od servisa bus redova. Dodatne informacije o redovima bus servisa potražite u članku servis Bus redovi, tema i pretplate.

## <a name="work-with-table-resources"></a>Rad s resursima za tablice

Servis za pohranu tablice Azure pohranjuje velikih količina strukturiranih podataka. Servis je NoSQL datastore koji prihvaća autentičnost pozive s unutrašnje i vanjske Azure oblaka. Azure tablice su idealna za pohranu strukturirane, koji nisu relacijskih podataka.

### <a name="to-create-a-table"></a>Stvaranje tablice

1. U programu Explorer poslužitelj, odaberite čvor **tablice** za pohranu računa pa odaberite **Create Table**.

1. U dijaloškom okviru **Stvaranje tablice** unesite naziv tablice.

### <a name="to-view-table-data"></a>Da biste pogledali podatke iz tablice

1. U programu Explorer poslužitelj, otvorite čvor **Azure** , a zatim otvorite čvor **prostora za pohranu** .

1. Otvorite čvor račun za pohranu koji vas zanima, a zatim otvorite čvor **tablice** da biste vidjeli popis tablica za račun za pohranu.

1. Otvaranje izbornika prečaca za tablicu, a zatim odaberite **Prikaz tablice**.

    ![Azure tablice u pregledniku rješenja](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

Tablice je organiziran prema entiteti (prikazan u recima) i svojstva (prikazan u stupcima). Na primjer, Sljedeća ilustracija prikazuje entiteti naveden u **Dizajnera tablice**:

### <a name="to-edit-table-data"></a>Da biste uredili podatke iz tablice

1. U **Dizajnera tablice**otvorite izbornički prečac za entitet (jedan redak) ili svojstva (u jednu ćeliju), a zatim odaberite **Uređivanje**.

    ![Dodavanje ili uređivanje entitet tablice](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

    Entiteti u jednoj tablici nisu imati isti skup svojstva (stupce). Imajte na umu sljedeća ograničenja na prikaz i uređivanje podataka u tablici.
    - Ne možete prikazati ni uređivati binarne podatke (Vrsta byte[]), ali možete pohraniti u tablici.

    - Ne možete uređivati vrijednosti **PartitionKey** ili **RowKey** jer spremište tablica u Azure ne podržava postupak.

    - Ne možete stvoriti svojstvo vremenske oznake, servise za pohranu Azure koristiti svojstva s tim nazivom.

    - Ako unesete vrijednost datuma i vremena, morate slijediti oblik koji je odgovarajuće regionalne i jezične postavke vašeg računala (na primjer, MM/DD/GGGG hh [AM | Poslijepodne] za engleski SAD-a).

### <a name="to-add-entities"></a>Da biste dodali entiteti

1. U **Dizajnera tablice**, odaberite gumb **Dodaj entitet** , koja se nalazi pri u gornjem desnom kutu prikaz tablice.

    ![Dodavanje entitet](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. U dijaloškom okviru **Dodavanje entitet** , unesite vrijednosti svojstva **PartitionKey** i **RowKey** .

    ![Entitet dijaloški okvir Dodavanje](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Pažljivo unesite vrijednosti jer ih nije moguće promijeniti nakon što zatvorite dijaloški okvir, osim ako izbrišete entitet, a ponovno dodajte.

### <a name="to-filter-entities"></a>Da biste filtrirali entiteti

Možete prilagoditi skup entiteti koji se pojavljuju u tablicu ako koristite Sastavljač upita.

1. Da biste otvorili Sastavljač upita, otvorite tablicu za prikaz.

1. Odaberite gumb krajnjih desnih na alatnoj traci za prikaz tablice.

    Pojavit će se dijaloški okvir **Sastavljač upita** . Sljedeća ilustracija prikazuje upit koji je ugrađen u sastavljaču upita.

    ![Sastavljač upita](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)

1. Kad završite stvaranje upita, zatvorite dijaloški okvir. Kao filtar WCF Data Services, pojavit će se izvedenog obrasca tekst upita u tekstni okvir.

1. Da biste pokrenuli upit, odaberite ikonu Zeleni trokut.

    Možete filtrirati i entitet podatke koji se pojavljuju u **Dizajnera tablice** ako unesete filtar niza WCF Data Services izravno u polje filtra. Ove vrste niz je slična je SQL uvjet WHERE, ali se šalje na poslužitelj kao HTTP zahtjev. Upute za sastavljanje filtar niza potražite u članku [Izgradnje nizove filtar za dizajnera tablice](https://msdn.microsoft.com/library/azure/ff683669.aspx).

    Sljedeća ilustracija prikazuje primjer niz valjani filtra:

    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Osvježavanje podataka za pohranu

Kada poslužitelj Explorer povezuje ili dohvaća podatke s računa za pohranu, je možda proći nekoliko minuta za dovršetak operacije. Ako ne možete povezati postupka možda istek vremena. Dok se dohvaća podatke, možete nastaviti raditi na druge dijelove Visual Studio. Da biste prekinuli postupak ako traje predugo, odaberite gumb za **Prekid osvježavanja** na alatnoj traci Explorer poslužitelja.

#### <a name="to-refresh-blob-container-data"></a>Da biste osvježili podatke spremnik blobova platforme

- Odaberite čvor **blob-ova** ispod **prostora za pohranu** , a zatim odaberite gumb **Osvježi** na alatnoj traci Explorer poslužitelja.

- Da biste osvježili popis blob-ova koji se prikazuje, odaberite gumb **izvrši** .

#### <a name="to-refresh-table-data"></a>Da biste osvježili podatke u tablici

- Odaberite čvor **tablice** ispod **prostora za pohranu** , a zatim odaberite gumb **Osvježi** .

- Da biste osvježili popis entiteti koji se prikazuje u **Dizajnera tablice**, odaberite gumb **izvrši** na **Dizajnera tablice**.

#### <a name="to-refresh-queue-data"></a>Da biste osvježili podatke reda čekanja

- Odaberite čvor **redovi** , a zatim odaberite gumb **Osvježi** .

#### <a name="to-refresh-all-items-in-a-storage-account"></a>Da biste osvježili sve stavke u račun za pohranu

- Odaberite naziv računa, a zatim odaberite gumb **Osvježi** na alatnoj traci za Explorer poslužitelja.

### <a name="add-storage-accounts-by-using-server-explorer"></a>Dodavanje računa za pohranu pomoću programa Explorer poslužitelja

Dva su načina za dodavanje računa za pohranu pomoću programa Explorer poslužitelja. Stvorite novi račun za pohranu u pretplatu za Azure ili možete priložiti postojećeg računa za pohranu.

#### <a name="to-create-a-new-storage-account-by-using-server-explorer"></a>Da biste stvorili novi prostor za pohranu račun pomoću programa Explorer poslužitelja

1. U Eksploreru za poslužitelj otvaranje izbornika prečaca za čvor prostora za pohranu, a zatim Stvori račun za pohranu.

    ![Stvaranje novog računa Azure prostora za pohranu](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. Odaberite ili unesite sljedeće podatke za novi račun za pohranu u dijaloškom okviru **Stvaranje računa za pohranu** .

    - Azure pretplatu u koju želite dodati račun za pohranu.

    - Naziv koji želite koristiti za novi račun za pohranu.

    - Regija ili afinitet grupe (primjerice Zapad SAD-a ili istočnoazijski).

    - Vrste replikacije koji želite koristiti za račun za pohranu, kao što su suvišnih zemlj.

1. Odaberite **Stvori**.

    Pojavit će se novi račun za pohranu na popisu **prostora za pohranu** u pregledniku rješenja.

#### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Da biste priložili postojećeg računa za pohranu pomoću programa Explorer poslužitelja

1. U Eksploreru za poslužitelj otvaranje izbornika prečaca za čvor Azure prostora za pohranu, a zatim **Priložite vanjskog prostora za pohranu**.

    ![Dodavanje postojećeg računa za pohranu](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)

1. Odaberite ili unesite sljedeće podatke za novi račun za pohranu u dijaloškom okviru **Stvaranje računa za pohranu** .

    - Naziv postojećeg računa za pohranu koju želite priložiti. Možete unijeti naziv ili odaberite s popisa.

    - Ključ za račun odabrane prostora za pohranu. Ta vrijednost obično prikazuje se za vas kada odaberete račun za pohranu. Ako želite da se Visual Studio zapamtiti ključ za račun za pohranu, potvrdite okvir ključni Zapamti račun.

    - Protokol za povezivanje s računom za pohranu, kao što su HTTP, HTTPS ili prilagođeni krajnje točke. Dodatne informacije o prilagođene krajnje točke potražite u članku [upute za konfiguriranje nizu za povezivanje](https://msdn.microsoft.com/library/azure/ee758697.aspx) .

### <a name="to-view-the-secondary-endpoints"></a>Da biste prikazali sekundarnu krajnje točke

- Ako ste stvorili za pohranu računa pomoću mogućnosti za **Pristup za čitanje zemlj suvišnih** replikacije, možete vidjeti njegove sekundarne krajnje točke. Otvaranje izbornika prečaca za naziv računa, a zatim odaberite **Svojstva**.

    ![Prostor za pohranu sekundarne krajnje točke](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Da biste uklonili račun za pohranu iz programa Explorer poslužitelja

- U Eksploreru za poslužitelj za otvaranje izbornika prečaca za naziv računa, a zatim odaberite **Izbriši**. Ako ste izbrisali s računom za pohranu, ukida se izgubiti spremljenu ključa za taj račun.

    >[AZURE.NOTE] Ako izbrišete prostora za pohranu računa iz programa Explorer poslužitelj, ne utječe na vaš račun za pohranu ili sve podatke koje sadrži; jednostavno uklanja referencu iz programa Explorer poslužitelja. Da biste trajno izbrisali s računom za pohranu, koristite [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o tome pomoću servisa Azure prostora za pohranu, u odjeljku [pristup Azure servise za pohranu](https://msdn.microsoft.com/library/azure/ee405490.aspx).
