<properties
    pageTitle="Kako koristiti reda čekanja za pohranu iz Python | Microsoft Azure"
    description="Saznajte kako pomoću servisa Azure čekanja iz Python za stvaranje i brisanje redove, umetanje, dobiti i brisanje poruka."
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

# <a name="how-to-use-queue-storage-from-python"></a>Kako koristiti reda čekanja za pohranu iz Python

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Pregled

Ovaj vodič prikazuje kako izvesti uobičajeni scenariji putem servisa za pohranu za Azure red. Primjere zapisuju u Python i koristite [Microsoft Azure prostora za pohranu SDK Python]. Scenariji u kojima je moguće uvrstiti **Umetanje**, **peeking**, **Početak**i **Brisanje** reda čekanja poruke, kao i **Stvaranje i brisanje reda čekanja**. Dodatne informacije o redovi, pogledajte odjeljak [daljnji koraci].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Upute: Stvaranje reda čekanja

Objekt **QueueService** omogućuje rad sa redovima. Sljedeći kod stvara **QueueService** objekt. Dodajte sljedeće pri vrhu bilo koju datoteku Python u kojoj želite li programatski pristup za pohranu Azure:

    from azure.storage.queue import QueueService

Sljedeći kod stvara objekt **QueueService** pomoću računa imenom i računom ključa za pohranu. Zamijenite 'mojračun' i 'mykey' naziv računa i ključa.

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## <a name="how-to-insert-a-message-into-a-queue"></a>Upute: Umetanje poruke reda čekanja

Da biste umetnuli poruke u red, koristite na **staviti\_poruke** načina za stvaranje nove poruke, a zatim ga dodati u redu čekanja.

    queue_service.put_message('taskqueue', u'Hello World')


## <a name="how-to-peek-at-the-next-message"></a>Upute: Pogled na sljedeću poruku

Možete pogled na poruku prednjoj strani reda bez uklanjanja iz reda čekanja tako da nazovete podršku za **uvid\_poruke** način. Prema zadanim postavkama **uvid\_poruke** peeks na jednu poruku.

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.content)


## <a name="how-to-dequeue-messages"></a>Upute: Dequeue poruke

Kod uklanja poruke iz reda čekanja u dva koraka. Kada se uključujete **dobiti\_poruke**, dobiti sljedeću poruku u redu čekanja prema zadanim postavkama. Poruke vratio **dobiti\_poruke** nevidljivim ostale kodove čitanju poruka iz ovog reda čekanja. Prema zadanim postavkama, ova poruka ostaje nevidljivi 30 sekundi. Da biste dovršili uklanjanje poruka iz reda čekanja, morate i pozovite **Brisanje\_poruke**. Dva koraka na sljedeći uklanjanja poruke pokazuje da ako kod ne uspije obraditi poruku zbog kvara hardver ili softver, drugoj instanci koda možete se ista poruka i pokušajte ponovno. Vaši se pozivi kod **Brisanje\_poruke** desnom tipkom miša kada poruka bude obrađen.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)

Moguće je prilagoditi dohvaćanja poruke iz reda čekanja na dva načina.
Prvo, možete dobiti skupine poruke (najviše 32). Drugo, možete postaviti invisibility dulje ili kraće vrijeme čekanja dopuštanja kod više ili manje vremena za potpuno obradu svaku poruku. Sljedeći kod primjer koristi u **dobiti\_poruke** načina koji će se 16 poruke jedan poziv. Zatim obrađuje svaku poruku pomoću programa za petlje. Također postavlja ograničenja invisibility na pet minuta za svaku poruku.

    messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)      


## <a name="how-to-change-the-contents-of-a-queued-message"></a>Upute: Promjena sadržaja u redu čekanja poruke

Možete promijeniti sadržaj u poruku na mjestu u redu čekanja. Ako poruka predstavlja zadatak posla, može koristiti ovu značajku da biste ažurirali status zadatak posla. Kod ispod koristi u **Ažuriranje\_poruke** način da biste ažurirali poruke. Vidljivost vremensko ograničenje je postavljeno na 0, što znači da odmah se pojavi poruka, a zatim se ažurira sadržaj.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')

## <a name="how-to-get-the-queue-length"></a>Upute: Dohvaćanje Duljina reda čekanja

U redu čekanja možete dobiti procjena se broj poruka. U **se\_reda čekanja\_metapodataka** način pita usluga reda čekanja da biste se vratili metapodataka o reda čekanja i **approximate_message_count**. Rezultat će biti samo djelomičnog jer poruke možete dodati ili ukloniti nakon usluga reda čekanja odgovori na vaš zahtjev.

    metadata = queue_service.get_queue_metadata('taskqueue')
    count = metadata.approximate_message_count

## <a name="how-to-delete-a-queue"></a>Upute: Brisanje reda čekanja

Da biste izbrisali reda i sve poruke koje se nalaze u njoj, nazovite na **Brisanje\_reda čekanja** način.

    queue_service.delete_queue('taskqueue')

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove reda čekanja prostora za pohranu, slijedite ove veze da biste saznali više.

- [Razvojni centar za Python](/develop/python/)
- [Servise za pohranu Azure REST API-JA](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog tima za Azure prostora za pohranu]
- [Microsoft Azure prostora za pohranu SDK Python]

[Blog tima za Azure prostora za pohranu]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure prostora za pohranu SDK Python]: https://github.com/Azure/azure-storage-python
