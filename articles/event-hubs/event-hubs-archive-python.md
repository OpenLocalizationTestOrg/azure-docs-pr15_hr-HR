<properties
    pageTitle="Azure događaj koncentratora arhiva vodič | Microsoft Azure"
    description="Primjer koji koristi Azure Python SDK da bismo pokazali pomoću značajke arhiva koncentratora za događaj."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="event-hubs-archive-walkthrough-python"></a>Vodič za arhiviranje koncentratora događaj: Python

Arhiviranje koncentratorima događaj je nova značajka koncentratorima događaj koji omogućuje vam automatski izlaganje toka podataka u koncentratora za događaj s poslovnim subjektom spremište blobova platforme Azure po izboru. To olakšava da biste izvršili skupna obrada na podatke u stvarnom vremenu strujanje. U ovom se članku opisuje kako koristiti događaj koncentratorima arhiva s Python. Dodatne informacije o arhiva koncentratorima događaja potražite u [članku pregled](event-hubs-archive-overview.md).

Ovaj primjer koristi Azure Python SDK da bismo pokazali pomoću značajke arhiva. Na sender.py šalje Simulirani okolini telemetrijskih koncentratorima događaja u JSON OSNOVNI oblik. Koncentrator za događaj je konfiguriran za korištenje značajke arhiva pisati podatke u bloba prostora za pohranu u grupama. Na archivereader.py zatim čita te blob-ova i stvara datoteku s dodavanjem po uređaja i zapisuje podatke u .csv datoteke.

Što će obaviti

1.  Stvaranje računa za spremište blobova platforme Azure i spremnik blob u njoj, pomoću portala za Azure

2.  Stvaranje programa događaja koncentrator prostor naziva, pomoću portala za Azure

3.  Stvaranje koncentratora za događaj sa značajkom arhiva omogućen, pomoću portala za Azure

4.  Slanje podataka u središtu događaj s Python skripte

5.  Čitanje datoteke iz arhive i proces ih s drugom Python skripte

Preduvjeti

1.  Python 2.7.x

2.  Azure pretplate

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Stvorite račun za pohranu za Azure

1.  Prijavite se na [portal za Azure][].

2.  U lijevom navigacijskom oknu na portalu kliknite Novo, a zatim kliknite podataka + prostor za pohranu pa kliknite račun za pohranu.

3.  Ispunite polja u plohu račun za pohranu, a zatim kliknite **Stvori**.

    ![][1]

4.  Kada se prikaže **Implementacijama uspjelo** poruku, kliknite na novi račun za pohranu i u plohu **Essentials** kliknite **blob-ova**. Kad se otvori plohu **Blob servisa** , kliknite **+ spremnik** pri vrhu. Naziv spremnik **arhiviranje**, a zatim zatvorite plohu **bloba servisa** .

5.  Pritisnite **tipke za pristup** u lijevom plohu i kopirajte naziv računa za pohranu i vrijednost **ključ1**. Spremite te vrijednosti za Blok za pisanje ili neke druge privremeno mjesto.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Stvaranje Python skriptu da biste poslali događaje koncentratora za događaj

1.  Otvorite omiljene Python uređivač, kao što su [Visual Studio kod][].

2.  Stvoriti skriptu pod nazivom **sender.py**. Ova skripta će poslati 200 događaje koncentratora za događaj. To su jednostavni okolini čitanja šalje u JSON.

3.  Zalijepite sljedeći kod u sender.py:

    ```
    import uuid
    import datetime
    import random
    import json
    from azure.servicebus import ServiceBusService
    
    sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    for y in range(0,20):
        for dev in devices:
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading)
            sbs.send\_event('myhub', s)
        print y
    ```
4.  Ažurirajte prethodni kod polja naziva i vrijednosti ključa koje ste dobili prilikom stvaranja naziva događaja koncentratora.

## <a name="create-a-python-script-to-read-your-archive-files"></a>Stvaranje Python skripta za čitanje arhivskim datotekama

1.  Ispunite na plohu i kliknite **Stvori**.

2.  Stvoriti skriptu pod nazivom **archivereader.py**. Ova skripta će pročitajte arhivskim datotekama i stvaranje datoteka po uređaj za zapisivanje podataka samo za taj uređaj.

3.  Zalijepite sljedeći kod u archivereader.py:

    ```
    import os
    import string
    import json
    import avro.schema
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    from azure.storage.blob import BlockBlobService
    
    def processBlob(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed\_json = json.loads(reading["Body"])
            if not 'id' in parsed\_json:
                return
            if not dict.has\_key(parsed\_json['id']):
            list = []
            dict[parsed\_json['id']] = list
        else:
            list = dict[parsed\_json['id']]
            list.append(parsed\_json)
        reader.close()
        for device in dict.keys():
            deviceFile = open(device + '.csv', "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\\n')

    def startProcessing(accountName, key, container):
        print 'Processor started using path: ' + os.getcwd()
        block\_blob\_service = BlockBlobService(account\_name=accountName, account\_key=key)
        generator = block\_blob\_service.list\_blobs(container)
        for blob in generator:
            if blob.properties.content\_length != 0:
                print('Downloaded a non empty blob: ' + blob.name)
                cleanName = string.replace(blob.name, '/', '\_')
                block\_blob\_service.get\_blob\_to\_path(container, blob.name, cleanName)
                processBlob(cleanName)
                os.remove(cleanName)
            block\_blob\_service.delete\_blob(container, blob.name)
    startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'archive')
    ```

4.  Ne zaboravite da biste zalijepili odgovarajuće vrijednosti za naziv računa za pohranu i ključ u poziv `startProcessing`.

## <a name="run-the-scripts"></a>Pokrenite skripte

1.  Otvorite naredbeni redak koji sadrži Python u svoju putanju, a zatim pokrenite te naredbe za instaliranje Python pripremni paketa:

    ```
    pip install azure-storage
    pip install azure-servicebus
    pip install avro
    ```
  
    Ako imate stariju verziju ili azure pohranu ili azure možda ćete morati koristiti u **– Nadogradnja** mogućnost

    Također morate pokrenuti na sljedeći način (nije potrebno u većini sustava):

    ```
    pip install cryptography
    ```

2.  Promjena direktorija na mjestu na kojem ste spremili sender.py i archivereader.py i pokrenite sljedeću naredbu:

    ```
    start python sender.py
    ```
    
    Time ćete novi proces Python da biste pokrenuli pošiljatelj.

3. Sada Pričekajte nekoliko minuta za arhiviranje da biste pokrenuli. U prozoru za naredbe izvorne pa upišite sljedeću naredbu:

    ```
    python archivereader.py
    ```

Ovog arhiva procesora koristi lokalni direktorij da biste preuzeli sve blob polja s računa za pohranu/kontejner. Će obraditi bilo koje nisu prazne i napišite rezultate kao .csv datoteke u lokalnom direktoriju.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o događajima koncentratora potražite na sljedećim vezama:

- [Pregled događaja koncentratorima arhiviranje][]
- U dovršena [Ogledna aplikacije koji koristi događaj koncentratorima][].
- Primjer [Skaliranje out događaj obradu s događaj koncentratorima][] .
- [Pregled događaja koncentratora][]
 

[Portal za Azure]: https://portal.azure.com/
[Pregled događaja koncentratora arhiviranje]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/
[Kod za Visual Studio]: https://code.visualstudio.com/
[Pregled događaja koncentratora]: event-hubs-overview.md
[primjer aplikacije koja koristi događaj koncentratora]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Promjena veličine izgleda događaj obrade s koncentratorima događaja]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
