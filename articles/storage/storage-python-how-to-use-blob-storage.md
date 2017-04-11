<properties
    pageTitle="Kako koristiti spremište blobova platforme Azure (objekt spremište) iz Python | Microsoft Azure"
    description="Pohranite nestrukturirane podatke u oblaku s Azure blobova (spremište objekta)."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-azure-blob-storage-from-python"></a>Kako koristiti Azure blobova iz Python

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Pregled

Azure blobova je servis koji pohranjuje nestrukturirane podatke u oblaku kao objekata/blob-ova. Spremište blobova platforme možete pohraniti sve vrste teksta ili binarne podatke, kao što su dokument, medijske datoteke ili program za instalaciju aplikacije. Spremište blobova platforme naziva se i spremište objekata.

U ovom se članku vidjet ćete kako izvršiti uobičajeni scenariji pomoću spremište blobova platforme. Primjere zapisuju u Python i koristite [Microsoft Azure prostora za pohranu SDK Python]. Scenariji u kojima je moguće uvrstiti prijenos, popis, preuzimanje i brisanje blob-ova.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a>Stvaranje spremnika

Ovisno o vrsti blob koju želite koristiti, stvorite **BlockBlobService**, **AppendBlobService**ili **PageBlobService** objekt. Sljedeći kod koristi **BlockBlobService** objekt. Dodajte sljedeće pri vrhu bilo koju datoteku Python u kojoj želite li programatski pristup blobova platforme Azure blok.

    from azure.storage.blob import BlockBlobService

Sljedeći kod stvara objekt **BlockBlobService** pomoću računa imenom i računom ključa za pohranu.  Zamijenite 'mojračun' i 'mykey' naziv računa i ključa.

    block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

U sljedećem primjeru kod **BlockBlobService** objekta možete koristiti da biste stvorili spremnik ako ne postoji.

    block_blob_service.create_container('mycontainer')

Po zadanom je novi spremnik privatnima, tako da odredite pristup ključ za pohranu (kao što ste prethodno poduzeli) da biste preuzeli blob polja s ovom spremniku. Ako želite da bi blob polja unutar spremnik dostupni svima, možete stvaranje spremnika i prenesite razinu pristupa javno pomoću koda za sljedeće.

    from azure.storage.blob import PublicAccess
    block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)

Osim toga, možete izmijeniti spremnik kada stvorite pomoću sljedeći kod.

    block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)

Nakon ta promjena svi na internetu mogu vidjeti blob-ova u spremniku javno, ali samo možete izmijeniti ili ih izbrisati.

## <a name="upload-a-blob-into-a-container"></a>Prijenos blob u spremniku

Da biste stvorili blob blok i prijenos podataka, koristite na **Stvaranje\_blob\_iz\_put**, **Stvaranje\_blob\_iz\_strujanje**, **Stvaranje\_blob\_iz\_bajtova** ili **Stvaranje\_blob\_iz\_tekst** metode. Oni načina više razine izvršiti potrebne chunking kada veličina podataka premašuje 64 MB.

**Stvaranje\_blob\_iz\_put** prenosi sadržaj datoteke u navedenom parametru path i **Stvaranje\_blob\_iz\_strujanje** prenosi sadržaj iz već otvorena datoteka/strujanje. **Stvaranje\_blob\_iz\_bajtova** prenosi polja bajtova, i **Stvaranje\_blob\_iz\_tekst** prenosi vrijednost određeni tekst koristeći navedeno šifriranje (po zadanom UTF-8).

U sljedećem primjeru prenosi sadržaj datoteke **sunset.png** u **myblob** blob-om.

    from azure.storage.blob import ContentSettings
    block_blob_service.create_blob_from_path(
        'mycontainer',
        'myblockblob',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png')
                )

## <a name="list-the-blobs-in-a-container"></a>Popis blob-ova u spremniku

Da biste dobili popis blob-ova u spremniku za korištenje na **popis\_blob-ova** način. Ta metoda vraća na generator. Sljedeći kôd proizvodi **naziv** svaki blob u spremniku na konzolu.

    generator = block_blob_service.list_blobs('mycontainer')
    for blob in generator:
        print(blob.name)

## <a name="download-blobs"></a>Preuzimanje blob-ova

Za preuzimanje podataka iz blob, koristite **dobiti\_blob\_da biste\_put**, **dobiti\_blob\_da biste\_strujanje**, **dobiti\_blob\_da biste\_bajtova**, ili **dobiti\_blob\_da biste\_tekst**. Oni načina više razine izvršiti potrebne chunking kada veličina podataka premašuje 64 MB.

U sljedećem primjeru pokazuje se korištenje **dobiti\_blob\_da biste\_put** da biste preuzeli sadržaj **myblob** blob i spremanje datoteka **u novom sunset.png** .

    block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')

## <a name="delete-a-blob"></a>Brisanje blob

Na kraju, da biste izbrisali blob, nazovite **delete_blob**.

    block_blob_service.delete_blob('mycontainer', 'myblockblob')

## <a name="writing-to-an-append-blob"></a>Pisanje programa blob s dodavanjem

U upit s dodavanjem blob optimiziran je za dodavanjem operacije, kao što su zapisivanje. Kao što je blok blob, u upit s dodavanjem blob sastoji se od blokova, no prilikom dodavanja novog bloka programa dodavanjem blob je uvijek dodaje na kraj blob-om. Ne možete ažurirati ili izbrisati postojeće blok u programa dodavanjem blob. Blokiranje ID-a za programa dodavanjem blob su vidljiva kao što je blok blob se nalazili.

Svaki blok u programa dodavanjem blob može biti različitih veličina, maksimalno 4 MB do i programa dodavanjem blob može sadržavati najviše 50.000 blokova. Maksimalna veličina programa dodavanjem blob stoga je malo više od 195 GB (4 MB X 50.000 blokova).

U primjeru u nastavku stvara novi upit s dodavanjem blob i dodaje neke podatke, simulaciju operacija jednostavne zapisivanja.

    from azure.storage.blob import AppendBlobService
    append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

    # The same containers can hold all types of blobs
    append_blob_service.create_container('mycontainer')

    # Append blobs must be created before they are appended to
    append_blob_service.create_blob('mycontainer', 'myappendblob')
    append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

    append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove blobova, slijedite ove veze da biste saznali više.

- [Razvojni centar za Python](/develop/python/)
- [Servise za pohranu Azure REST API-JA](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog tima za Azure prostora za pohranu]
- [Microsoft Azure prostora za pohranu SDK Python]

[Blog tima za Azure prostora za pohranu]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure prostora za pohranu SDK Python]: https://github.com/Azure/azure-storage-python
