<properties
    pageTitle="Upute za korištenje spremišta tablica iz Python | Microsoft Azure"
    description="Pohranite strukturiranih podataka u oblak pomoću tablice Azure prostor za pohranu, NoSQL izvor podataka."
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


# <a name="how-to-use-table-storage-from-python"></a>Upute za korištenje spremišta tablica iz Python

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Pregled

Ovaj vodič prikazuje kako izvoditi uobičajene scenarije pomoću servisa za pohranu Azure tablice. Primjere zapisuju u Python i koristite [Microsoft Azure prostora za pohranu SDK Python]. Obuhvaćene scenariji obuhvaćaju stvaranje i brisanje tablice, osim umetanja i postavljanje upita entiteti u tablici.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Stvaranje tablice

Objekt **TableService** vam omogućuje rad sa servisima tablice. Sljedeći kod stvara **TableService** objekt. Dodavanje koda za pri vrhu bilo koje datoteke Python u kojoj želite li programatski pristup Azure prostora za pohranu:

    from azure.storage.table import TableService, Entity

Sljedeći kod stvara objekt **TableService** pomoću prostora za pohranu imenom i računom ključ računa.  Zamijenite 'mojračun' i 'mykey' naziv računa i ključa.

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## <a name="add-an-entity-to-a-table"></a>Dodavanje entitet u tablicu

Da biste dodali entitet, najprije stvorite rječnika ili entitet koji određuje entitet svojstvo naziva i vrijednosti. Imajte na umu da za svaki entitet, morate navesti **PartitionKey** i **RowKey**. To su jedinstveni identifikatori vaše entiteti. Upit možete poslati mnogo brže upit ostalih svojstava možete poslati pomoću ove vrijednosti. Sustav koristi **PartitionKey** automatski distribuirati entiteti tablice iznad čvorove mnogo prostora za pohranu.
Entiteti koji imaju isti **PartitionKey** pohranjuju se na istom čvor. **RowKey** je jedinstveni ID entitet unutar particije koji pripada.

Da biste tablicu dodali entitet, prenesite rječnika objekta u **Umetanje\_entitet** način.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

Prenositi instanca klase **entitet** da biste na **Umetanje\_entitet** način.

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## <a name="update-an-entity"></a>Ažuriranje entitet

Kod prikazuje kako zamijeniti staru verziju postojeći entitet ažuriranu verziju.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

Ako ne postoji entitet koji ažurira, zatim postupak ažuriranja neće uspjeti. Ako želite pohraniti entitet, bez obzira na to jesu li postojala prije, koristite **Umetanje\_ili\_replace_entity**.
U sljedećem primjeru prvi poziv će zamijeniti postojeći entitet. Drugi poziv će umetnite novi entitet, jer nema entitet s navedenom **PartitionKey** i **RowKey** postoji u tablici.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

## <a name="change-a-group-of-entities"></a>Promjena grupe entiteti

Ponekad je li bolje slanje više operacije zajedno u grupe da biste bili sigurni atomske obrade na poslužitelju. Da biste izvršili, koristite **TableBatch** predmete. Kada želite slanja skupine, poziva **Potvrdi\_obradu**. Imajte na umu da se svi entiteti mora biti u istoj particije da bi se može promijeniti kao niz. U primjeru u nastavku zbraja dvije entiteti u grupu.

    from azure.storage.table import TableBatch
    batch = TableBatch()
    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    batch.insert_entity(task10)
    batch.insert_entity(task11)
    table_service.commit_batch('tasktable', batch)

Serija može se koristiti i s sintaksa Upravitelj contex:

    task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
    task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

    with table_service.batch('tasktable') as batch:
        batch.insert_entity(task12)
        batch.insert_entity(task13)


## <a name="query-for-an-entity"></a>Upit za entitet

Da biste upit entitet u tablici, koristite na **dobiti\_entitet** način prosljeđivanjem **PartitionKey** i **RowKey**.

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## <a name="query-a-set-of-entities"></a>Upit skupa entiteti

U ovom se primjeru pronalazi sve zadatke u Seattle na temelju **PartitionKey**.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## <a name="query-a-subset-of-entity-properties"></a>Podskup entitet svojstva za upite

Upit u tablicu možete dohvatiti samo nekoliko svojstva iz entitet.
Ovu tehniku naziva *projekcije*, smanjuje propusnost i poboljšati performanse upita, posebice za velike entiteti. Koristite parametar **Odaberite** i prenesite imena svojstva koje želite prenijeti klijentu.

Upit u sljedeći kod vraća samo opise entiteti u tablici.

[AZURE.NOTE] Sljedeći isječak funkcionira samo u odnosu na servis za pohranu u oblaku. To ne podržava emulator prostora za pohranu.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
    for task in tasks:
        print(task.description)

## <a name="delete-an-entity"></a>Brisanje entitet

Možete izbrisati entitet pomoću ključ particija i redaka.

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## <a name="delete-a-table"></a>Brisanje tablice

Sljedeći kod briše tablicu s računom za pohranu.

    table_service.delete_table('tasktable')

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove spremište tablica, slijedite ove veze da biste saznali više.

- [Razvojni centar za Python](/develop/python/)
- [Servise za pohranu Azure REST API-JA](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog tima za Azure prostora za pohranu]
- [Microsoft Azure prostora za pohranu SDK Python]

[Blog tima za pohranu za Azure]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure prostora za pohranu SDK Python]: https://github.com/Azure/azure-storage-python
