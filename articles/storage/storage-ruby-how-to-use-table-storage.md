<properties
    pageTitle="Kako koristiti Azure spremišta tablica iz Ruby | Microsoft Azure"
    description="Pohranite strukturiranih podataka u oblak pomoću tablice Azure prostor za pohranu, NoSQL izvor podataka."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor=""/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-ruby"></a>Kako koristiti Azure spremišta tablica iz Ruby

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Pregled

Ovaj vodič prikazuje kako izvesti uobičajeni scenariji pomoću servisa Azure tablice. Primjere se pišu pomoću Ruby API-JA. Scenariji u kojima je moguće uvrstiti **Stvaranje i brisanje tablice, umetanje i postavljanje upita entiteti u tablici**.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Stvaranje Ruby aplikacije

Upute za stvaranje Ruby aplikacije potražite [Ruby tračnicama web-aplikacije na programa Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).


## <a name="configure-your-application-to-access-storage"></a>Konfiguriranje aplikacija za pristup za pohranu

Da biste koristili Azure prostora za pohranu, morate preuzeti i koristiti Ruby azure paket koji sadrži skup praktičnost biblioteke koje komunikaciju sa servisima za pohranu OSTALE.

### <a name="use-rubygems-to-obtain-the-package"></a>Korištenje RubyGems da biste dobili pakiranje

1. Korištenje naredbenog retka sučelja kao što je **PowerShell** (Windows), **Terminal** (Mac) ili **tulum** (Unix).

2. U prozoru naredbu da biste instalirali gem i ovisnosti vrstu **gem instalacija azure** .

### <a name="import-the-package"></a>Uvoz paketa

Korištenje omiljene uređivač, dodajte sljedeće na vrh Ruby datoteku koju namjeravate koristiti za pohranu:

    require "azure"

## <a name="set-up-an-azure-storage-connection"></a>Postavljanje veze sustava Azure prostora za pohranu

Modul azure će čitati varijable okruženja **AZURE\_prostora za POHRANU\_RAČUN** i **AZURE\_prostora za POHRANU\_PRISTUP\_KLJUČ** informacije potrebne za povezivanje s računom za pohranu Azure. Ako te varijable okruženja su postavljena, morate navesti podatke o računu prije korištenja **Azure::TableService** s sljedeći kod:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"

Da biste dobili te vrijednosti iz na klasični ili Voditelj resursa za pohranu računa na portalu za Azure:

1. Prijavite se na [Portal za Azure](https://portal.azure.com).
2. Idite na račun za pohranu koji želite koristiti.
3. U plohu postavke s desne strane, pritisnite **Tipke za pristup**.
4. U tipke plohu Access koji će se prikazati vidjet ćete tipkovnog 1 i 2 tipka za pristup. Možete koristiti bilo koju od njih. 
5. Kliknite ikonu Kopiraj da biste kopirali tipku u međuspremnik. 

Da biste dobili te vrijednosti s računa za klasični prostora za pohranu na portalu klasični Azure:

1. Prijavite se na [Klasični Azure portal](https://manage.windowsazure.com).
2. Idite na račun za pohranu koji želite koristiti.
3. Kliknite **Upravljanje PRISTUPNIH TIPKI** pri dnu navigacijskog okna.
4. U skočnom dijaloški vidjet ćete naziv računa za pohranu, access primarni ključ i sekundarne tipkovni prečac. Tipkovni prečac, poslužite se primarni jedan ili sekundarni jedan. 
5. Kliknite ikonu Kopiraj da biste kopirali tipku u međuspremnik.

## <a name="create-a-table"></a>Stvaranje tablice

Objekt **Azure::TableService** omogućuje rad s tablicama i entiteti. Da biste stvorili tablicu, koristite na **Stvaranje\_table()** način. Sljedeći primjer stvara tablice ili ispis više pogrešku ako je bilo.

    azure_table_service = Azure::TableService.new
    begin
      azure_table_service.create_table("testtable")
    rescue
      puts $!
    end

## <a name="add-an-entity-to-a-table"></a>Dodavanje entitet u tablicu

Da biste dodali entitet, stvorite predmemorije objekta koji definira entitet svojstva. Imajte na umu da za svaki entitet morate navesti **PartitionKey** i **RowKey**. Te su odredišnih stilova na entiteti, i vrijednosti koje možete mu mnogo brže od ostalih svojstava. Azure prostora za pohranu koristi **PartitionKey** automatski distribuirati entiteti tablice iznad čvorove mnogo prostora za pohranu. Entiteti s istom **PartitionKey** pohranjuju se na istom čvor. **RowKey** je jedinstveni ID entitet unutar particije pripada.

    entity = { "content" => "test entity",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.insert_entity("testtable", entity)

## <a name="update-an-entity"></a>Ažuriranje entitet

Postoje dostupna da biste ažurirali postojeći entitet više načina:

* **Ažuriranje\_entity():** Ažurirajte postojeći entitet zamjenjujući ga.
* **pisma\_entity():** Ažurira postojeći entitet spajanjem nove vrijednosti nekretnina u postojeći entitet.
* **Umetanje\_ili\_pisma\_entity():** Ažurira postojeći entitet zamjenjujući ga. Ako postoji bez entitet, novi će se umetnuti:
* **Umetanje\_ili\_zamjena\_entity():** Ažurira postojeći entitet spajanjem nove vrijednosti nekretnina u postojeći entitet. Ako postoji bez entitet, umetnut će se novi.

Sljedeći primjer prikazuje ažuriranje entitet pomoću **Ažuriranje\_entity()**:

    entity = { "content" => "test entity with updated content",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.update_entity("testtable", entity)

S **Ažuriranje\_entity()** i **pisma\_entity()**, ako je onaj koji želite ažurirati ne postoji, a zatim postupak ažuriranja neće uspjeti. Stoga želite pohraniti entitet, bez obzira na to jesu li ga već postoji, umjesto toga koristite **Umetanje\_ili\_Zamijeni\_entity()** ili **Umetanje\_ili\_pisma\_entity()**.

## <a name="work-with-groups-of-entities"></a>Rad s grupama entiteti

Ponekad je li bolje slanje više operacije zajedno u grupe da biste bili sigurni atomske obrade na poslužitelju. Da biste izvršili koji, najprije stvorite **grupe** objekt i zatim koristite na **izvršavanje\_batch()** način na **TableService**. Sljedeći primjer prikazuje slanje dva entiteti s RowKey 2 i 3 u grupu. Obratite pozornost na to da ga samo radi entiteti s istom PartitionKey.

    azure_table_service = Azure::TableService.new
    batch = Azure::Storage::Table::Batch.new("testtable",
      "test-partition-key") do
      insert "2", { "content" => "new content 2" }
      insert "3", { "content" => "new content 3" }
    end
    results = azure_table_service.execute_batch(batch)

## <a name="query-for-an-entity"></a>Upit za entitet

Da biste upit entitet u tablici, koristite na **dobiti\_entity()** metoda prosljeđivanjem naziv tablice, **PartitionKey** i **RowKey**.

    result = azure_table_service.get_entity("testtable", "test-partition-key",
      "1")

## <a name="query-a-set-of-entities"></a>Upit skupa entiteti

Upit skupa entiteti u tablici, stvaranje predmemorije objekta upita i koristiti u **upita\_entities()** način. Sljedeći primjer prikazuje početak entiteti s istom **PartitionKey**:

    query = { :filter => "PartitionKey eq 'test-partition-key'" }
    result, token = azure_table_service.query_entities("testtable", query)

> [AZURE.NOTE] Ako skup rezultata je prevelika za jedan upit da biste se vratili nastavka token vratit će se koje možete koristiti za dohvat sljedeće stranice.

## <a name="query-a-subset-of-entity-properties"></a>Podskup entitet svojstva za upite

Upit u tablicu možete dohvatiti samo nekoliko svojstva iz entitet. Ovu tehniku pod nazivom "projekcija", smanjuje propusnost i poboljšati performanse upita, posebice za velike entiteti. Korištenje uvjetu SELECT i prenesite imena svojstva koje želite prenijeti klijentu.

    query = { :filter => "PartitionKey eq 'test-partition-key'",
      :select => ["content"] }
    result, token = azure_table_service.query_entities("testtable", query)

## <a name="delete-an-entity"></a>Brisanje entitet

Da biste izbrisali entitet, koristite na **Brisanje\_entity()** način. Morate proći naziv tablice koja sadrži entitet, PartitionKey i RowKey entitet.

        azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## <a name="delete-a-table"></a>Brisanje tablice

Da biste izbrisali tablicu, koristite na **Brisanje\_table()** metodu i lozinka naziv tablice koju želite izbrisati.

        azure_table_service.delete_table("testtable")

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o složenije zadatke za pohranu, slijedite ove veze:

- [Blog tima za Azure prostora za pohranu](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) spremište na GitHub
