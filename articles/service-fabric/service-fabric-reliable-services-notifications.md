<properties
   pageTitle="Pouzdani servisi obavijesti | Microsoft Azure"
   description="Konceptualni dokumentaciju za servis tkanina pouzdanog servisa obavijesti"
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="reliable-services-notifications"></a>Pouzdani servisi obavijesti

Obavijesti da bi klijenti mogli pratiti promjene koje vrše na objekt koji ih zanima. Dvije vrste objekata podržava obavijesti: *Pouzdanog Upravitelj stanje* i *Pouzdan rječnika*.

Najčešćih razloga za korištenje obavijesti su:

- Sastavni materialized prikaza, kao što su sekundarne ili pridružuje filtriranih prikaza stanja replike. Primjer je sortirani indeks sve tipke koje su u rječniku pouzdan.
- Slanje nadzora podatke, kao što je broj korisnika koji su dodani u zadnjem satu.

Obavijesti su pokrenuta kao dio primjene operacije. Zbog toga obavijesti trebali biste provesti jednako brzo kao što je moguće i sinkrono događaji bi trebalo sadržavati skupi operacije.

## <a name="reliable-state-manager-notifications"></a>Pouzdan stanje Upravitelj obavijesti

Upravitelj pouzdanog stanje omogućuje obavijesti o događajima za sljedeće:

- Transakcije
    - Izvršavanje
- Upravitelj stanja
    - Ponovno stvaranje
    - Dodavanje pouzdanog stanja
    - Uklanjanje pouzdanog stanja

Upravitelj pouzdanog stanje prati trenutni inflight transakcije. Samo promjena u stanje transakcije koji uzrokuje obavijesti da bi biti pokrenuta je transakcija se izvršenom.

Upravitelj pouzdanog stanje održava zbirka pouzdanog stanja kao što su pouzdani rječnik i pouzdan red. Upravitelj pouzdanog stanje aktivira obavijesti o promjenama zbirku: pouzdanog stanje je dodati ili ukloniti ili se izgraditi cijelu zbirku.
Pouzdan Upravitelj Stanje zbirke se izgraditi u tri slučajevima:

- Oporavak: Prilikom pokretanja programa replike ga oporavlja prethodno stanje s diska. Na kraju oporavak koristi **NotifyStateManagerChangedEventArgs** početka događaja koje sadrži skup oporavljene pouzdanog stanja.
- Potpuna Kopiraj: prije kopiju možete uključiti skup konfiguracije, sadrži sastavljanja. Ponekad zahtijeva cijelu kopiju pouzdanog stanje Upravitelj stanje iz primarnog replike zatvoriti neaktivnosti sekundarnog replike. Upravitelj pouzdanog stanje na sekundarnog replike koristi **NotifyStateManagerChangedEventArgs** početka događaja koje sadrži skup pouzdanog stanja koja je dobivene iz primarnog replike.
- Vraćanje: U slučajevima oporavak Izrada replike stanje može ih vratiti iz sigurnosne kopije putem **RestoreAsync**. U tim slučajevima pouzdanog Upravitelj stanje na primarni replike koristi **NotifyStateManagerChangedEventArgs** početka događaja koje sadrži skup pouzdanog stanja koji je vraćen iz sigurnosne kopije.

Da biste registrirali za transakcije obavijesti i/ili stanje Upravitelj obavijesti, morate registrirati s događajima **TransactionChanged** ili **StateManagerChanged** pouzdanog Upravitelj stanje. Uobičajeni mjesto da biste registrirali s ove rukovatelja događajima je Graditelj s praćenjem stanja servisa. Kada se registrirate za Graditelj, nećete ništa sve obavijest koja uzrokuje promjena tijekom života **IReliableStateManager**.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

Rukovatelj događajima **TransactionChanged** koristi **NotifyTransactionChangedEventArgs** možete unijeti detalje o događaju. Sadrži svojstvo akcija (na primjer, **NotifyTransactionChangedAction.Commit**), koji određuje vrstu promjene. Sadrži i svojstvo transakcije koje nudi referenca transakcije koje se promijenile.

>[AZURE.NOTE] Danas, događaji **TransactionChanged** potenciju su samo ako je transakcija izvršenja. Akcije pa je iznos jednak **NotifyTransactionChangedAction.Commit**. No u budućnosti se događaji možda potenciju za ostale vrste promjena stanja transakcije. Preporučujemo da Provjera akcije i obrada događaj samo ako je onaj koji ste i očekivali.

Slijedi primjer **TransactionChanged** događajima.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

Rukovatelj događajima **StateManagerChanged** koristi **NotifyStateManagerChangedEventArgs** možete unijeti detalje o događaju.
**NotifyStateManagerChangedEventArgs** sadrži dvije podklase: **NotifyStateManagerRebuildEventArgs** i **NotifyStateManagerSingleEntityChangedEventArgs**.
Koristite svojstvo akcija u **NotifyStateManagerChangedEventArgs** ubacivanje **NotifyStateManagerChangedEventArgs** na odgovarajuće podklase:

- **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
- **NotifyStateManagerChangedAction.Add** i **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Slijedi rukovatelja primjer **StateManagerChanged** obavijesti.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Pouzdan rječnika obavijesti

Pouzdan rječnika omogućuje obavijesti o događajima za sljedeće:

- Izvođenje: Pozvan kada **ReliableDictionary** je obnoviti stanje iz oporavljene ili kopirali lokalne saveznu državu ili sigurnosnu kopiju.
- Očisti: Pozvan kada je stanje **ReliableDictionary** izbrisano metodom **ClearAsync** .
- Dodavanje: Pozvan kada stavku dodana **ReliableDictionary**.
- Ažuriranje: Naziva se kada stavku **IReliableDictionary** ažuriran.
- Uklanjanje: Naziva se kada izbrisane stavke u **IReliableDictionary** .

Da biste dobili pouzdanog rječnika obavijesti, morate registrirati s događajima **DictionaryChanged** na **IReliableDictionary**. Uobičajeni mjesto da biste registrirali s ove rukovatelja događajima se **ReliableStateManager.StateManagerChanged** dodavanje obavijesti.
Registracija kada dodate **IReliableDictionary** **IReliableStateManager** osigurava da nećete ništa obavijesti.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

>[AZURE.NOTE] **ProcessStateManagerSingleEntityNotification** je metoda uzorka koja u prethodnom primjeru **OnStateManagerChangedHandler** poziva.

Prethodni kod postavlja sučelje **IReliableNotificationAsyncCallback** , zajedno s **DictionaryChanged**. Budući da **NotifyDictionaryRebuildEventArgs** sadrži sučelje **IAsyncEnumerable** – koji treba moguće numerirati asinkrono – ponovno stvaranje obavijesti su pokrenuta kroz **RebuildNotificationAsyncCallback** umjesto **OnDictionaryChangedHandler**.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

>[AZURE.NOTE] Prethodni kod kao dio obrade obavijesti izvođenje najprije održavaju Zbrojeno stanje isključen. Jer pouzdanog zbirke je u tijeku izgraditi novi stanjem, sve prethodne obavijesti su nevažnih.

Rukovatelj događajima **DictionaryChanged** koristi **NotifyDictionaryChangedEventArgs** možete unijeti detalje o događaju.
**NotifyDictionaryChangedEventArgs** ima pet podklase. Pomoću svojstva akcija u **NotifyDictionaryChangedEventArgs** anketu **NotifyDictionaryChangedEventArgs** na odgovarajuće podklase:

- **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
- **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
- **NotifyDictionaryChangedAction.Add** i **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
- **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
- **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Preporuke

- *Provedite* jednako brzo kao moguće dovršetka obavijesti o događajima.
- *Ne* izvršiti sve operacije skupi (na primjer, / i operacije) kao dio Istodobni događaji.
- *Provjeriti vrsta akcije prije postupka događaja.* Nove vrste akcija može biti dodan u budućnosti.

Evo nekih Napomena koje valja imati na umu:

- Obavijesti su pokrenuta kao dio izvršavanje operacija. Na primjer, obavijest Vrati pokrenuta će se kao posljednji korak postupak vraćanja. Vraćanje se ne završi dok obrađuje događaj obavijesti.
- Budući da su obavijesti pokrenuta kao dio zatvaranje operacije, klijentima potražite u članku obavijesti za lokalno izvršenja operacije. I jer operacije su zajamčeno samo lokalno izvršenja (drugim riječima, prijavljeni,) možda ili se možda neće biti poništiti u budućnosti.
- Na putu Ponovi poništeno jedan obavijest je pokrenuta za svaku primijenjena operaciju. To znači da ako transakcije T1 obuhvaća Create(X), Delete(X) i Create(X), prikazat će se jedan obavijesti za stvaranje X, jedan za brisanje i jedan za ponovno stvaranje tim redoslijedom.
- Za transakcije koje sadrže više operacija operacije primjenjuju se redoslijedom kojim su primio poruku na primarni replike od korisnika.
- Kao dio obrade false tijeku, neki postupci mogu poništiti. Obavijesti su potenciju za operacije poništavanje, vraćanje stanje replike stabilan točku. Jedna od razlika važnih obavijesti o Poništi je koji se pridružuje događaja koji sadrži duplicirane tipke. Ako, na primjer, ako transakcije T1 je u tijeku je poništavanje, vidjet ćete jedan obavijesti da biste je Delete(X).

## <a name="next-steps"></a>Daljnji koraci

- [Pouzdan zbirki](service-fabric-work-with-reliable-collections.md)
- [Pouzdani servisi brzi početak](service-fabric-reliable-services-quick-start.md)
- [Pouzdani servisi sigurnosnog kopiranja i vraćanja (Izrada oporavak)](service-fabric-reliable-services-backup-restore.md)
- [Referenca za razvojne inženjere za zbirke pouzdanog mjesta](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
