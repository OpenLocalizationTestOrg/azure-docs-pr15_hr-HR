<properties
    pageTitle="Kako koristiti za pohranu datoteka Azure s Python | Microsoft Azure"
    description="Saznajte kako koristiti pohranu datoteka Azure s Python prijenos, popis, preuzimanje i brisanje datoteka."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-azure-file-storage-from-python"></a>Kako koristiti Azure pohrani iz Python

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Pregled

U ovom se članku vidjet ćete kako izvršiti uobičajeni scenariji pomoću za pohranu datoteka. Primjere zapisuju u Python i koristite [Microsoft Azure prostora za pohranu SDK Python]. Scenariji u kojima je moguće uvrstiti prijenos, popis, preuzimanje i brisanje datoteka.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>Stvaranje zajedničke mape

Objekt **FileService** omogućuje rad s dionice, mape i datoteke. Sljedeći kod stvara **FileService** objekt. Dodajte sljedeće pri vrhu bilo koju datoteku Python u kojoj želite li programatski pristup Azure prostora za pohranu.

    from azure.storage.file import FileService

Sljedeći kod stvara objekt **FileService** pomoću računa imenom i računom ključa za pohranu.  Zamijenite 'mojračun' i 'mykey' naziv računa i ključa.

    file_service = **FileService** (account_name='myaccount', account_key='mykey')

U sljedećem primjeru kod **FileService** objekta možete koristiti da biste stvorili zajednički koristi ako ne postoji.

    file_service.create_share('myshare')

## <a name="upload-a-file-into-a-share"></a>Prijenos datoteke u zajedničke mape

Programa Azure prostora za pohranu za zajedničko korištenje datoteka sadrži pri na vrlo najmanje, korijenski direktorij gdje se mogu nalaziti datoteke. U ovom odjeljku ćete saznati kako prenijeti datoteke s lokalno spremište na korijenski direktorij na zajedničko korištenje.

Da biste stvorili datoteku, a zatim prijenos podataka, koristite na **Stvaranje\_datoteke\_iz\_put**, **Stvaranje\_datoteke\_iz\_strujanje**, **Stvaranje\_datoteke\_iz\_bajtova** ili **Stvaranje\_datoteke\_iz\_tekst** metode. Oni načina više razine izvršiti potrebne chunking kada veličina podataka prelazi 64 MB.

**Stvaranje\_datoteke\_iz\_put** prenosi sadržaj datoteke u navedenom parametru path i **Stvaranje\_datoteke\_iz\_strujanje** prenosi sadržaj iz već otvorena datoteka/strujanje. **Stvaranje\_datoteke\_iz\_bajtova** prenosi polja bajtova, i **Stvaranje\_datoteke\_iz\_tekst** prenosi vrijednost određeni tekst koristeći navedeno šifriranje (po zadanom UTF-8).

U sljedećem primjeru prenosi sadržaj datoteke **sunset.png** **myfile** u datoteku.

    from azure.storage.file import ContentSettings
    file_service.create_file_from_path(
        'myshare',
        None, # We want to create this blob in the root directory, so we specify None for the directory_name
        'myfile',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png'))

## <a name="how-to-create-a-directory"></a>Kako: Stvorite direktorij

Prostor za pohranu možete organizirati i postavljanjem datoteke unutar podređenu direktorija umjesto da ih u korijenskom direktoriju. Servis za pohranu datoteka Azure omogućuje stvaranje direktorija proizvoljan broj kao da bi vaš račun. Ispod kod će stvoriti podređene direktorij pod nazivom **sampledir** u korijenskom direktoriju.

    file_service.create_directory('myshare', 'sampledir')

## <a name="how-to-list-files-and-directories-in-a-share"></a>Kako: popis datoteke i mape u zajedničke mape

Na popisu datoteke i mape u zajednički koristiti s **popis\_direktorija\_i\_datoteke** način. Ta metoda vraća na generator. Sljedeći kôd proizvodi **naziv** svaku datoteku i imenik u zajedničko korištenje na konzolu.

    generator = file_service.list_directories_and_files('myshare')
    for file_or_dir in generator:
        print(file_or_dir.name)

## <a name="download-files"></a>Preuzimanje datoteka

Za preuzimanje podataka iz datoteke, koristite **dobiti\_datoteke\_da biste\_put**, **dobiti\_datoteke\_da biste\_strujanje**, **dobiti\_datoteke\_da biste\_bajtova**, ili **dobiti\_datoteke\_da biste\_tekst**. Oni načina više razine izvršiti potrebne chunking kada veličina podataka premašuje 64 MB.

U sljedećem primjeru pokazuje se korištenje **dobiti\_datoteke\_da biste\_put** da biste preuzeli sadržaj **myfile** datoteke i spremanje datoteka **u novom sunset.png** .

    file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')

## <a name="delete-a-file"></a>Brisanje datoteke

Na kraju, da biste izbrisali datoteku, nazovite **delete_file**.

    file_service.delete_file('myshare', None, 'myfile')

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove prostora za pohranu datoteka, slijedite ove veze da biste saznali više.

- [Razvojni centar za Python](/develop/python/)
- [Servise za pohranu Azure REST API-JA](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog tima za Azure prostora za pohranu]
- [Microsoft Azure prostora za pohranu SDK Python]

[Blog tima za Azure prostora za pohranu]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure prostora za pohranu SDK Python]: https://github.com/Azure/azure-storage-python
