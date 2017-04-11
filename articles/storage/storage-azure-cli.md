<properties
    pageTitle="Azure EŽA pomoću Azure prostora za pohranu | Microsoft Azure"
    description="Saznajte kako koristiti Azure sučelja naredbenog retka (Azure EŽA) s Azure prostora za pohranu za stvaranje i upravljanje računima za pohranu i rad s Azure blob-ova i datotekama. Azure EŽA je alat za različite platforme "
    services="storage"
    documentationCenter="na"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="using-the-azure-cli-with-azure-storage"></a>Azure EŽA pomoću Azure prostora za pohranu

## <a name="overview"></a>Pregled

Azure EŽA pruža skup Otvori izvor različite platforme naredbi za rad s platforme Azure. Nudi velik broj isti funkcionalnosti pronaći [Azure portal](https://portal.azure.com) , kao i funkcije obogaćenog podataka programa access.

U ovom vodiču smo ćete Istražite kako koristiti [Azure sučelje naredbenog retka (Azure EŽA)](../xplat-cli-install.md) za izvođenje različitih razvoj i administracije zadataka s Azure prostora za pohranu. Preporučujemo da preuzeti i instalirati ili nadograditi na najnoviju Azure EŽA prije nego počnete koristiti ovaj vodič.

Ovaj vodič pretpostavlja da razumijete osnovni koncepti Azure prostora za pohranu. Vodič sadrži broj skripte da bismo pokazali korištenje EŽA Azure s Azure prostora za pohranu. Ne zaboravite ažurirati varijable skripte ovisno o vašoj konfiguraciji prije pokretanja svako pismo.

> [AZURE.NOTE] Vodič sadrži Azure EŽA Primjeri naredbi i skripte za račune klasični prostora za pohranu. Potražite u članku EŽA Azure [pomoću EŽA Azure za Mac i Linux, Windows Azure resursa upravljanja pomoću](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) naredbe za račune Voditelj resursa za pohranu.

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>Početak rada s Azure prostora za pohranu i EŽA Azure u pet minuta

Ovaj vodič koristi Ubuntu primjere, ali druge platforme OS provoditi na sličan način.

**Novi korisnik Azure:** Pronađite web-mjesto Microsoft Azure pretplata i u okvir za Microsoftov račun pridružen tom pretplate. Informacije o mogućnostima Azure za kupnju, potražite u članku [Besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/), [Mogućnosti za kupnju](https://azure.microsoft.com/pricing/purchase-options/)i [Nudi člana](https://azure.microsoft.com/pricing/member-offers/) (za članove MSDN, Microsoftovoj partnerskoj mreži i BizSpark i druge Microsoftove programe).

Dodatne informacije o pretplatama Azure potražite u članku [Dodjela administratorskih uloga u Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Nakon stvaranja pretplate na Microsoft Azure i račun:**

1. Preuzmite i instalirajte EŽA Azure slijedite upute navedene u [instalirati EŽA Azure](../xplat-cli-install.md).
2. Kada je instaliran EŽA Azure, moći koristiti naredbu azure iz naredbenog retka sučelja (tulumu, Terminal, naredbeni redak) za pristup naredbama Azure EŽA. Vrsta `azure` naredbu, a trebali biste vidjeti sljedeće izlaz.

    ![Naredba Azure Izlaz][Image1]

3. U sučelju naredbeni redak upišite `azure storage` popis izvršavanja svih naredbi azure prostora za pohranu i početak prvog dojam na radovi EŽA Azure nudi. Možete upisati naziv naredbe s parametrom **– h** (na primjer, `azure storage share create -h`) da biste vidjeli detalje o naredbenoj sintaksi.
4. Sada ćemo će vam dati jednostavnu skriptu koja prikazuje osnovne naredbe Azure EŽA da biste pristupili Azure prostora za pohranu. Skripta najprije zatražit će da biste postavili dvije varijable za svoj račun za pohranu i ključ. Skripta pa će stvoriti novi spremnik na novi račun za pohranu i Prenesi postojeću datoteku slike (blob) za njih. Nakon popise skripta sve blob-ova u njih će je preuzeti slikovne datoteke u odredišni imenik koji postoji na lokalnom računalu.

        #!/bin/bash
        # A simple Azure storage example

        export AZURE_STORAGE_ACCOUNT=<storage_account_name>
        export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

        export container_name=<container_name>
        export blob_name=<blob_name>
        export image_to_upload=<image_to_upload>
        export destination_folder=<destination_folder>

        echo "Creating the container..."
        azure storage container create $container_name

        echo "Uploading the image..."
        azure storage blob upload $image_to_upload $container_name $blob_name

        echo "Listing the blobs..."
        azure storage blob list $container_name

        echo "Downloading the image..."
        azure storage blob download $container_name $blob_name $destination_folder

        echo "Done"

5. Na lokalnom računalu otvorite uređivač željeni tekst (vim na primjer). Upišite iznad skripte u uređivač teksta.

6. Sada ćete morati ažurirati varijable skripte na temelju konfiguracijske postavke.

    - **< storage_account_name >** Nazovite navedenog u skripti ili unesite novi naziv za vaš račun za pohranu. **Važne:** Naziv računa spremišta mora biti jedinstvena u Azure. Mora biti mala slova, previše!

    - **< storage_account_key >** Tipkovni prečac vašeg računa za pohranu.

    - **< container_name >** Nazovite navedenog u skripti ili unesite novi naziv za svoje kontejner.

    - **< image_to_upload >** Unesite put do slike na lokalnom računalu, kao što su: "~ / images/HelloWorld.png".

    - **< destination_folder >** Unesite put do lokalni direktorij za spremanje datoteke preuzete iz Azure prostor za pohranu, kao što su: "~/downloadImages".

7. Kada ste ažurirali potrebne varijable u vim, pritisnite kombinacije tipki "Esc,:, wq!" Da biste spremili skriptu.

8. Da biste pokrenuli ovu skriptu, jednostavno upišite naziv datoteke skripte na konzoli tulumu. Nakon izvođenja Ova skripta, imat ćete lokalne odredišnu mapu koja obuhvaća preuzetih slikovne datoteke. Sljedeće snimka zaslona prikazuje se primjer izlaz:

Nakon izvođenja skripta, imat ćete lokalne odredišnu mapu koja obuhvaća preuzetih slikovne datoteke.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Upravljanje računima za pohranu s EŽA Azure

### <a name="connect-to-your-azure-subscription"></a>Povezivanje s pretplate Azure

Iako bez Azure pretplata će funkcionirati Većina naredbi za pohranu, preporučujemo povezivanje s pretplate s EŽA Azure. Da biste konfigurirali Azure EŽA za rad s pretplatom, slijedite korake u [Povezivanje s Azure pretplate s EŽA Azure](../xplat-cli-connect.md).

### <a name="create-a-new-storage-account"></a>Stvaranje novog računa za pohranu

Da biste koristili Azure prostora za pohranu, morate je račun za pohranu. Nakon što ste konfigurirali računalo povezati s pretplate možete stvoriti na novi račun za Azure prostora za pohranu.

        azure storage account create <account_name>

Naziv računa spremišta mora biti između 3 i 24 znakova i koristiti brojeva i mala slova.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Postavljanje zadanog računa Azure prostora za pohranu u varijable okruženja

Imate više računa za pohranu za pretplatu. Možete odabrati jednu od njih i postavljanje u varijabli okruženja za sve naredbe za pohranu u istu sesiju. Omogućuje vam pokretanje naredbe za pohranu Azure EŽA bez navođenja račun za pohranu i izričito tipki.

        export AZURE_STORAGE_ACCOUNT=<account_name>
        export AZURE_STORAGE_ACCESS_KEY=<key>

Da biste postavili zadani račun za pohranu i pomoću niza za povezivanje. Najprije se niz za povezivanje naredbom:

        azure storage account connectionstring show <account_name>

Zatim kopirajte niz za povezivanje izlazne i postavite ga na varijablu okruženja:

        export AZURE_STORAGE_CONNECTION_STRING=<connection_string>

## <a name="create-and-manage-blobs"></a>Stvaranje i upravljanje blob-ova

Azure blobova je servis za pohranu velike količine nestrukturirane podatke, poput teksta ili binarni podataka koji se može pristupiti iz bilo kojeg mjesta na svijetu putem HTTP ili HTTPS. U ovom se odjeljku pretpostavlja da ste već upoznati s koncepata spremište blobova platforme Azure. Detaljne informacije potražite u članku [Početak rada s Azure blobova pomoću .NET](storage-dotnet-how-to-use-blobs.md) i [Blob servisa koncepata](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Stvaranje spremnika

Svaki blobova platforme Azure pohrane u mora biti u spremniku. Možete stvoriti pomoću spremnik za privatni na `azure storage container create` naredba:

        azure storage container create mycontainer

> [AZURE.NOTE] Postoje tri razine anonimni pristup za čitanje: **isključivanje** **Blob**i **kontejner**. Da biste spriječili anonimni pristup blob-ova, postavite parametar dozvola na **Isključeno**. Prema zadanim postavkama, novi spremnik je privatna te im možete pristupiti samo vlasnik računa. Da biste omogućili anonimni pristup javnim čitanje blob resursa, ali ne spremnik metapodataka ili na popisu blob-ova u željeni spremnik postavljeno parametar dozvola u **Blob** Dopustili potpuni javno čitanje pristup resursima blob spremnik metapodataka i popis blob-ova u željeni spremnik dozvola parametar postaviti **spremniku**. Dodatne informacije potražite u članku [Upravljanje anonimni pristup za čitanje spremnika i blob-ova](storage-manage-access-to-resources.md).

### <a name="upload-a-blob-into-a-container"></a>Prijenos blob u spremniku

Azure blobova podržava bloka blob-ova i blob-Ova stranica. Dodatne informacije potražite u članku [objašnjenje bloka blob-ova, dodavanje blob-ova, i blob-Ova stranica](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Da biste prenijeli blob-ova u spremniku, možete koristiti na `azure storage blob upload`. Prema zadanim postavkama ta se naredba prenosi lokalne datoteke blob blok. Da biste odredili vrstu za blob-om, možete koristiti u `--blobtype` parametar.

        azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob

### <a name="download-blobs-from-a-container"></a>Preuzmite blob-ova iz spremnik

Sljedeći primjer pokazuje kako preuzeti blob-ova iz spremnika.

        azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'

### <a name="copy-blobs"></a>Kopiranje blob-ova

Asinkrono možete kopirati blob polja unutar ili putem računa za pohranu i regija.

Sljedeći primjer pokazuje kako kopirati blob polja s jednog računa za pohranu na drugi. U ovom primjeru ćemo stvoriti spremnik gdje su blob-ova javno, anonimno pristupiti.

    azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

    azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

    azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer

Ovaj primjer izvodi asinkronog Kopiraj. Možete nadzirati status svaki kopiranja tako da pokrenete u `azure storage blob copy show` operacija.

Imajte na umu da izvorišni URL namijenjeno kopiranja mora biti javno dostupnu ili uključiti token SAS (zajednički pristup potpis).

### <a name="delete-a-blob"></a>Brisanje blob

Da biste izbrisali blob, poslužite se ispod naredbe:

        azure storage blob delete mycontainer myBlockBlob2

## <a name="create-and-manage-file-shares"></a>Stvaranje i Upravljanje zajedničkim datotekama

Azure pohrani nudi zajedničkog prostora za pohranu za aplikacije pomoću standardnih SMB protokola. Virtualnim računalima sustava Microsoft Azure i servise u oblaku, kao i lokalne aplikacije, možete zajednički koristiti datoteke podataka putem postavljenu zajedničko korištenje. Možete upravljati zajedničkim datotekama i podataka iz datoteke putem EŽA Azure. Dodatne informacije o pohrani Azure, pročitajte članak [Početak rada s Azure pohrani u sustavu Windows](storage-dotnet-how-to-use-files.md) ili [kako koristiti za pohranu datoteka Azure s Linux](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Stvaranje zajedničke datoteke

Azure zajedničkom je SMB zajedničkom servisu Azure. Sve datoteke i mape moraju se stvoriti u zajedničko korištenje datoteka. Račun može sadržavati neograničen broj dionice, a zajedničke mape možete pohraniti neograničen broj datoteka, do kapaciteta ograničenja prostora za pohranu računa. Sljedeći primjer stvara zajedničko korištenje datoteka s nazivom **myshare**.

        azure storage share create myshare

### <a name="create-a-directory"></a>Stvorite direktorij

Direktorij nudi neobavezno hijerarhijske strukture za Azure zajedničkom. Sljedeći primjer stvara direktorij pod nazivom **myDir** u odjeljku zajedničko korištenje datoteka.

        azure storage directory create myshare myDir

Napomena taj imenik put može sadržavati više razine, *npr.* **na / b**. Međutim, morate Provjerite postoji li sve nadređenog direktorija. Ako, na primjer, za put **na / b**, morate direktorija **na** prvo stvorite, a zatim stvaranje direktorija **b**.

### <a name="upload-a-local-file-to-directory"></a>Prijenos datoteke lokalnog imenika

U sljedećem primjeru prenosite datoteke iz **~/temp/samplefile.txt** **myDir** direktoriju. Uređivanje put datoteke tako da pokazuje na valjanu datoteku na lokalnom računalu:

        azure storage file upload '~/temp/samplefile.txt' myshare myDir

Imajte na umu da datoteka u odjeljku zajedničko korištenje može biti do 1 TB veličine.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Popis datoteka u korijen za zajedničko korištenje ili direktorija

Možete navesti datoteke i direktorijima u korijen zajedničko korištenje ili directory pomoću sljedeće naredbe:

        azure storage file list myshare myDir

Imajte na umu da je naziv direktorija neobavezno za operaciju stavku. Ako se ispusti, naredba navedeni sadržaj korijenski direktorij koji se zajednički koristi.

### <a name="copy-files"></a>Kopiranje datoteka

Počevši od verzije 0.9.8 Azure EŽA, možete kopirati datoteku da biste druge datoteke, datoteke blob ili blob u datoteku. U nastavku ćemo sadrži upute za izvođenje te operacije Kopiraj pomoću naredbi EŽA. Kopiranje datoteke u novi direktorij:

    azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

Da biste kopirali blob direktorij datoteka:

    azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

## <a name="next-steps"></a>Daljnji koraci

Evo nekoliko srodnih članaka i resursi za učenje dodatne informacije o Azure prostora za pohranu.

- [Dokumentacija Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/)
- [Azure prostora za pohranu REST API Reference](https://msdn.microsoft.com/library/azure/dd179355.aspx)


[Image1]: ./media/storage-azure-cli/azure_command.png
