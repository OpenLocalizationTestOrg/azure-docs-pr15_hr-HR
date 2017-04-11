<properties
    pageTitle="Upute za korištenje spremišta blobova (objekt spremište) iz Ruby | Microsoft Azure"
    description="Pohranite nestrukturirane podatke u oblaku s Azure blobova (spremište objekta)."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-blob-storage-from-ruby"></a>Upute za korištenje spremišta blobova iz Ruby

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Pregled

Azure blobova je servis koji pohranjuje nestrukturirane podatke u oblaku kao objekata/blob-ova. Spremište blobova platforme možete pohraniti sve vrste teksta ili binarne podatke, kao što su dokument, medijske datoteke ili program za instalaciju aplikacije. Spremište blobova platforme naziva se i spremište objekata.

Ovaj vodič vidjet ćete kako izvršiti uobičajeni scenariji pomoću spremište blobova platforme. Primjere se pišu pomoću Ruby API-JA. Scenariji u kojima je moguće uvrstiti **prijenos, popis, preuzimanje,** i **Brisanje** blob-ova.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Stvaranje Ruby aplikacije

Stvaranje Ruby aplikacije. Upute potražite u članku [Ruby tračnicama web-aplikacije na VM programa Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-to-access-storage"></a>Konfiguriranje aplikacija za pristup za pohranu

Da biste koristili Azure prostora za pohranu, morate preuzeti i koristiti Ruby azure paket, što obuhvaća skup praktičnost biblioteke koje komunikaciju OSTALE servise za pohranu.

### <a name="use-rubygems-to-obtain-the-package"></a>Korištenje RubyGems da biste dobili pakiranje

1. Korištenje naredbenog retka sučelja kao što je **PowerShell** (Windows), **Terminal** (Mac) ili **tulum** (Unix).

2. Upišite "gem Instaliraj azure" u prozoru naredbu da biste instalirali gem i ovisnosti.

### <a name="import-the-package"></a>Uvoz paketa

Korištenje omiljene uređivač, dodajte sljedeće na vrh Ruby datoteku koju namjeravate koristiti za pohranu:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Postavljanje veze sustava Azure prostora za pohranu

Modul azure će čitati varijable okruženja **AZURE\_prostora za POHRANU\_RAČUN** i **AZURE\_prostora za POHRANU\_ACCESS_KEY** informacije potrebne za povezivanje s računom Azure prostora za pohranu. Ako te varijable okruženja su postavljena, morate navesti informacije o računu prije korištenja **Azure::Blob::BlobService** s sljedeći kod:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"


Da biste dobili te vrijednosti iz na klasični ili Voditelj resursa za pohranu računa na portalu za Azure:

1. Prijavite se na [portal za Azure](https://portal.azure.com).
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

## <a name="create-a-container"></a>Stvaranje spremnika

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Objekt **Azure::Blob::BlobService** omogućuje rad s spremnika i blob-ova. Da biste stvorili spremniku, koristite na **Stvaranje\_container()** način.

Sljedeći primjer koda stvara spremnik ili ispis više pogrešku ako je bilo.

    azure_blob_service = Azure::Blob::BlobService.new
    begin
      container = azure_blob_service.create_container("test-container")
    rescue
      puts $!
    end

Ako želite objaviti datoteke u spremniku, možete postaviti kontejner s dozvolama.

Možete jednostavno izmijeniti u <strong>Stvaranje\_container()</strong> poziva za prosljeđivanje na **: javno\_pristup\_razinu** mogućnost:

    container = azure_blob_service.create_container("test-container",
      :public_access_level => "<public access level>")


Valjane vrijednosti za na **: javno\_pristup\_razinu** su mogućnosti:

* **blob:** Određuje cijela javno čitanje kontejner i blob podataka. Klijenti možete numerirati blob-ova unutar kontejnera putem anonimni zahtjev, ali nije moguće numerirati spremnika subjekta prostora za pohranu.

* **spremnik:** Određuje javno pristup za čitanje za blob-ova. Blob podataka u ovom spremniku je moguće čitati putem anonimni zahtjev, ali spremnik podataka nije dostupna. Klijenti nije moguće numerirati blob polja unutar kontejnera putem anonimni zahtjev.

Umjesto toga možete izmijeniti razinu pristupa za javno spremnika pomoću **Postavljanje\_spremnik\_acl()** način da biste odredili razinu pristupa javnosti.

Sljedeći primjer koda mijenja razinu pristupa javno **spremniku**:

    azure_blob_service.set_container_acl('test-container', "container")

## <a name="upload-a-blob-into-a-container"></a>Prijenos blob u spremniku

Da biste prenijeli sadržaj na blob, koristite na **Stvaranje\_blok\_blob()** način da biste stvorili blob-om, korištenje datoteka ili niz kao sadržaj blob-om.

Sljedeći kod prenosite datoteke **test.png** kao novi blob pod nazivom "slike-blob" u spremniku.

    content = File.open("test.png", "rb") { |file| file.read }
    blob = azure_blob_service.create_block_blob(container.name,
      "image-blob", content)
    puts blob.name

## <a name="list-the-blobs-in-a-container"></a>Popis blob-ova u spremniku

Da biste dobili popis spremnike pomoću metode **list_containers()** .
Da biste popis blob-ova u spremniku, koristite **popis\_blobs()** način.

To proizvodi URL-ove sve blob-ova u spremnicima za račun.

    containers = azure_blob_service.list_containers()
    containers.each do |container|
      blobs = azure_blob_service.list_blobs(container.name)
      blobs.each do |blob|
        puts blob.name
      end
    end

## <a name="download-blobs"></a>Preuzimanje blob-ova

Da biste preuzeli blob-ova, koristite na **dobiti\_blob()** način za dohvaćanje sadržaj.

Sljedeći primjer koda pokazuje se korištenje **Dohvati\_blob()** da biste preuzeli sadržaj "slike blob" i pisanje lokalnu datoteku.

    blob, content = azure_blob_service.get_blob(container.name,"image-blob")
    File.open("download.png","wb") {|f| f.write(content)}

## <a name="delete-a-blob"></a>Brisanje Blob
Na kraju, da biste izbrisali blob, koristite na **Brisanje\_blob()** način. Sljedeći primjer koda pokazuje kako izbrisati blob.

    azure_blob_service.delete_blob(container.name, "image-blob")

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o složenije zadatke za pohranu, slijedite ove veze:

- [Blog tima za Azure prostora za pohranu](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) spremište na GitHub
- [Prijenos podataka s AzCopy naredbenog retka uslužni](storage-use-azcopy.md)
