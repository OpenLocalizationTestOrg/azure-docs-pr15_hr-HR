<properties
    pageTitle="Rad s pouzdanog zbirke | Microsoft Azure"
    description="Saznajte najbolje prakse za rad s pouzdanog zbirke."
    services="service-fabric"
    documentationCenter=".net"
    authors="JeffreyRichter"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="03/28/2016"
    ms.author="jeffreyr" />

# <a name="working-with-reliable-collections"></a>Rad s pouzdanog zbirki

Servis tkanina nudi s praćenjem stanja programiranje model dostupna .NET programerima putem pouzdanog zbirke. Konkretno, servis tkanina pouzdanog rječnik i nudi klase pouzdanog reda čekanja. Prilikom korištenja ove klase stanje particije (za skalabilnost) te replicirati (dostupnost) i transakcije unutar particije (za ACID semantiku). Pogledajmo pogledajte uobičajenog korištenja objekta pouzdanog rječnik i potražite u članku što njegov zapravo način.

~~~
retry:
try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to  
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) { 
   await Task.Delay(100, cancellationToken); goto retry; 
}
~~~

Sve operacije na objekte pouzdanog rječnik (osim ClearAsync koje ne možete poništiti), potreban je ITransaction objekt. Taj objekt je pridružen bilo i sve promjene prilikom pokušaja provjerite sve pouzdane rječnik i/ili pouzdanog red čekanja objekte unutar jedna particija. Nabava sustava ITransaction objekt tako da nazovete particija je način CreateTransaction StateManager korisnika.
 
U gore navedeni kod objekt ITransaction prosljeđuje pouzdanog rječnika AddAsync korakom. Interno, metode rječnik koji prihvaća ključa potrebno čitanje/pisanje lock povezan s ključem. Ako način mijenja vrijednost ključa sustava, metodu uzima pisanje zaključavanja na tipku i ako metodu samo čita iz na tipku vrijednosti, zatim zaključanu za čitanje je primijenjena na tipku. Budući da AddAsync mijenja vrijednost ključa sustava na novu, uneseni vrijednost, koristi se zaključavanje pisanje na tipku. Tako, pokušajte 2 (ili više) niti se zbrajaju vrijednosti s istim ključem u isto vrijeme, niti će zaključati na pisanje i druge niti će blokirati. Prema zadanim postavkama metode blokirati do 4 sekunde zaključati; Nakon 4 sekunde metode vratiti na TimeoutException. Način overloads postoji vam omogućuje proslijedite vrijednost eksplicitnih vremenskog ograničenja ako biste radije.
 
Obično, pisanje koda Upoznajte da biste na TimeoutException toga ga i ponovni cijeli postupak (kao što je prikazano gore navedeni kod). U kodu Moje jednostavne li se samo poziva Task.Delay prosljeđivanje 100 milisekundi svaki put. No u stvari, možda ga bolje umjesto toga koristite neke vrste eksponencijalne odgode za isključivanje natrag.
 
Kada je zaključavanje dobivene, AddAsync dodaje referenci objekta ključ i vrijednost Interna privremene rječnik koji je povezan s objektom ITransaction. To možete učiniti da vam pruži semantiku čitanje – vaše – vlasnik – zapisivanja. To jest, nakon poziva AddAsync, kasnije poziv TryGetValueAsync (pomoću na isti objekt ITransaction) će vratiti vrijednost čak i ako se još ne izvršenom transakcije. Nakon toga AddAsync serializes ključ i vrijednost objekata bajt polja i dodaje tih polja bajt datoteku zapisnika na lokalni čvor. Na kraju, AddAsync šalje polja bajt sekundarnog replike tako da imaju iste podatke ključa vrijednosti. Čak i ako se informacije ključa vrijednosti napisan datoteku zapisnika, podaci se smatra dio rječnik dok transakcije koje su pridružene unesene. 

U gore navedeni kod poziv CommitAsync pohranjuje sve operacije transakcije. Konkretno, ga dodaje Potvrdi informacije u datoteku zapisnika na lokalni čvor i i šalje zapis potvrdi sekundarnog replike. Kada je odgovorio kvorum (većina) na replike, sve promjene podataka smatra se trajno i sve zaključavanja povezan s tipkama koje su podešavati putem objekt ITransaction objavljivanja tako da druge niti/transakcije možete upravljati isti tipki i njihovih vrijednosti.

Ako CommitAsync je pod nazivom (obično zbog se iznimka ne), Rashodovano dobiva ITransaction objekt. Nakon rashoda nepotvrđenim ITransaction objekt servisa tkanina dodaje prekid informacije lokalne čvor datoteke zapisnika i ništa ne mora biti poslana u neku od sekundarnog replike. A zatim objavljivanja sve zaključavanja povezan s tipkama koje su podešavati putem transakcije.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Uobičajene probleme i kako izbjeći ih
Sad kad objasniti kako funkcioniraju pouzdanog zbirke interno, pogledajmo na nekoliko uobičajenih zlouporabe od njih. Potražite kod u nastavku:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes, 
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
~~~

Rad s pravilnim .NET rječnik, možete dodati ključa/vrijednosti u rječnik i promijenite vrijednosti svojstva (primjerice LastLogin). Međutim, kod će neće funkcionirati ispravno pouzdanog rječnika. Imajte na umu iz starije rasprave poziv AddAsync serializes objekte ključa vrijednosti s bajt polja i sprema poljima lokalna datoteka i također im šalje sekundarnog replike. Ako naknadno promijeniti svojstvo, mijenja vrijednost svojstvo u memoriji samo; ne utječe lokalne datoteke ili podataka koji se šalju da biste na replike. Ako se ruši se proces, što je u memoriji se Expression.Error odmah. Kada se pokrene novi proces ili neki drugi replike postaje primarni, zatim stare svojstvo vrijednost je ono što je dostupno. 

Nije moguće stress dovoljno kako je jednostavno uspostaviti vrste pogrešku gore navedenoj sintaksi. Možete i samo saznat ćete o pogrešku ako/kada postupak funkcionira. Najbolji način pisanja koda jednostavno je da biste Zrcalili dva retka:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync(); 
}
~~~

Evo još jednog primjera koji se prikazuje česta pogreška:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync(); 
   }
}
~~~

Ponovno običnog rječnici .NET, gore navedeni kod dobro funkcionira i uobičajenih uzorak: programer koristi ključ za traženje vrijednosti. Ako postoji vrijednost, programer mijenja vrijednost neko svojstvo. Međutim, s pouzdanog zbirke kod prikazuje isti problem kao već spominju: __morate ne mijenjate objekta kada ste dali pouzdanog zbirke.__
 
Najbolji način da biste ažurirali vrijednost u zbirku pouzdanog je upućivanje postojećoj vrijednosti i razmotrite objekt koji upućuje referenca immutable. Zatim stvorite novi objekt koji je točan kopiju izvornog objekta. Sada možete izmijeniti stanja taj novi objekt i pisati novi objekt u zbirku tako da ga može vidjeti serijalizirati u poljima bajt dodan lokalne datoteke i poslane na replike. Nakon potvrđivanja promjene objekata u memoriji, lokalne datoteke i sve replike imaju isti upravo u ono stanje. Sve dobro je!

Kod u nastavku prikazuje najbolji način da biste ažurirali vrijednost u zbirku pouzdane:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire 
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync(); 
   }
}
~~~

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Definiranje vrsta immutable podataka da biste spriječili programerskom pogreške

Najbolje željeli bismo alata za Kompiliranje pogrešaka prilikom slučajno proizvesti kod koji mutates stanje koje morate uzeti u obzir immutable objekt. No compiler C# nema mogućnost da biste to učinili. Tako, da biste izbjegli potencijalne programerskom programskih pogrešaka, preporučujemo da definirate vrsta koristite s pouzdanog zbirke biti immutable vrste. Konkretno, to znači da bi s core vrijednost vrste (kao što su brojevi [Int32 UInt64, itd.], datuma i vremena, Guid, vremenski raspon i sličnog). Možete i Naravno, možete se poslužiti niz. Preporučuje se da biste izbjegli svojstva zbirke kao Serijalizacija i prekidanja serije ih možete često može usporavati rad. Međutim, ako želite koristiti svojstva zbirke preporučujemo korištenje. Neto-immutable zbirke biblioteka (System.Collections.Immutable). Ova biblioteka dostupna je za preuzimanje http://nuget.org. Preporučujemo i zapečatite razrede i unese polja samo za čitanje kad god je moguće.

Vrstu informacija o korisniku pokazuje kako odrediti vrstu immutable iskoristi ispunjavaju prethodno navedene preporuke.

~~~
[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;
 
   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }
 
   [DataMember]
   public readonly String Email;
 
   // Ideally, this would be a readonly field but it can't be because OnDeserialized 
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
~~~

Vrsta ID stavke je immutable vrste kao što je prikazano ovdje:

~~~
[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
~~~

## <a name="schema-versioning-upgrades"></a>Verzije shema (nadogradnje)

Interno, pouzdanog zbirke serijalizirati pomoću objekte. Neto-DataContractSerializer. Serijalizirani objekti se ista i na lokalni disk primarni replike i šalju na sekundarnog replike. Kako na servisu stari, je vjerojatno ćete htjeti promijeniti vrstu podataka (sheme) potreban je na servisu. Upravljanje verzijama podataka pomoću sjajno Oprez morate pristupiti. Najprije i u prvom redu, morate uvijek ćete moći ukloniti serijski broj stare podatke. Konkretno, to znači da kod deserijalizacija mora biti neizmjerno kompatibilna: 333 verziju servisa koda moraju imati mogućnost da biste upravljali podacima koji se smješta u pouzdanog zbirci verzija 1 kod usluge prije 5 godina.

Osim toga, kod usluge je nadograđena jednu domenu nadogradnje odjednom. Tako, tijekom nadogradnje, imate dvije različite verzije sustava servisa kod koji se izvodi istodobno. Koje morate izbjegnete novu verziju kod usluge koristiti novu shemu, kao što je stare verzije kod usluge možda nećete moći učiniti novu shemu. Kada je to moguće, trebali biste dizajnirati u svakoj verziji servisa biti unaprijed kompatibilan s verzijom 1. Konkretno, to znači da V1 kod usluge trebali biste moći jednostavno Zanemari sve elemente sheme ga izričito ne rukovati. Međutim, morate ga moći spremiti podatke ne izričito zna i jednostavno pisanje ponovno out prilikom ažuriranja rječnika ključ ili vrijednost. 

>[AZURE.WARNING] Dok mijenjate shemi ključ osigurati da su stabilan kod raspršivanje svoj ključ i algoritmi jednako. Ako promijenite način bilo koju od tih algoritmi funkcioniranje, nećete moći ponovno ikad potražite ključ unutar pouzdanog rječnik.
  
Osim toga, možete izvršiti što se obično naziva 2 faza nadogradnju. U nadogradnjom 2 faza Nadogradnja na servisu s V1 V2: V2 sadrži kod koji ne zna baviti novi promjena sheme, ali ne izvrši kod. Kada kod V2 čita V1 podatke, pristajete na njemu i zapisuje V1 podatke. Nakon toga po dovršetku nadogradnje preko svih nadogradnje domene koje možete je došlo do signala za pokrenute instance V2 dovršetka nadogradnje. (Jedan pokrenuta za signal Time se uvođenju konfiguracije nadogradnje; to je što čini to 2 faza nadogradnju.) Sada V2 instanci možete pročitati podatke V1, pretvorite ga u V2 podaci, na njoj raditi i pisanje kao V2 podaci. Kada druge instance pročitali V2 podaci, ne trebaju pretvaranja, oni samo na njoj raditi i pisanje podataka i V2.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o stvaranju prosljeđivanje podataka kompatibilne ugovora, potražite u članku [Kompatibilni podataka ugovora](https://msdn.microsoft.com/library/ms731083.aspx).

Saznajte najbolje postupke na ugovore podataka za rad s verzijama, potražite u članku [Određivanje verzije ugovora podataka](https://msdn.microsoft.com/library/ms731138.aspx). 

Da biste saznali kako implementirati ugovore pogreške podataka verzija, potražite u članku [Pogreške verziju serijalizacije pozive](https://msdn.microsoft.com/library/ms733734.aspx). 

Da biste saznali kako dokumentu daju strukturu podataka koji se mogu biti uskladiva preko više verzija, potražite u članku [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
