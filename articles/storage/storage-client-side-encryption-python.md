<properties
    pageTitle="Klijentsko šifriranja s Python za pohranu Microsoft Azure | Microsoft Azure"
    description="U biblioteci klijent za Azure prostora za pohranu za Python podržava šifriranje klijentsko maksimalni sigurnost za pohranu Azure aplikacije."
    services="storage"
    documentationCenter="python"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Klijentsko šifriranja s Python za pohranu Microsoft Azure

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Pregled

[Azure prostora za pohranu klijenta biblioteku Python](https://pypi.python.org/pypi/azure-storage) podržava šifriranje podataka u klijentskim aplikacijama prije prijenosa Azure prostora za pohranu i dešifriranje podataka tijekom preuzimanja klijentu.

>[AZURE.NOTE] Biblioteka Python prostora za pohranu Azure je u pretpregledu.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Šifriranje i dešifriranje putem tehnika omotnice
Procesi šifriranje i dešifriranje slijedite postupak omotnice.

### <a name="encryption-via-the-envelope-technique"></a>Šifriranje putem tehnika omotnice
Šifriranje putem omotnica postupak funkcionira na sljedeći način:

1.  Biblioteka klijentski Azure spremišta generira ključ šifriranja sadržaja (CEK), što je ključ simetričnu one-time se koristi.

2.  Korisnički podaci je šifriran pomoću ovog CEK.

3.  Na CEK pa je omotan (šifrirane) pomoću ključa za šifriranje ključa (KEK). Na KEK označena identifikatora ključa, a može biti asimetričnim ključa par ili simetričnu ključ koji se upravlja lokalno.
Biblioteka za pohranu klijenta sam nikad ima pristup KEK. Biblioteku poziva algoritam ključa prelamanje kojemu tako da na KEK. Da biste koristili prilagođeni davatelja usluga za ključne prelamanje/unwrapping po želji možete odabrati korisnika.

4.  Šifrirani podaci pa prenesene sa servisom Azure prostora za pohranu. Tipku prelomljeni uz neke dodatne šifriranje metapodatke pohranjenih kao metapodatke (s blob) ili nulama sa šifriranim (reda čekanja poruke i entiteti tablice).

### <a name="decryption-via-the-envelope-technique"></a>Dešifriranje putem tehnika omotnice
Dešifriranje putem omotnica postupak funkcionira na sljedeći način:

1.  Klijentska biblioteka pretpostavlja da korisnik je Upravljanje ključem za šifriranje ključ (KEK) lokalno. Korisnik ne morate znati određenog ključ koji je korišten za šifriranje. Umjesto toga ključa Razrješavanje koja se odnosi različite ključa identifikatora tipke, možete se postaviti sustavu i koristi.

2.  Klijentska biblioteka preuzimanje šifriranih podataka uz svaki materijal šifriranja koja je spremljena na servis.

3.  Ključ za prelomljeni sadržaja šifriranje (CEK) je, a zatim unwrapped (dešifriranu) pomoću ključa šifriranja ključ (KEK). Ovdje ponovno biblioteku klijent neće imati pristup KEK. Samo se poziva unwrapping algoritam prilagođene davatelja.

4.  Ključ za šifriranje sadržaja (CEK) koristi se za dešifriranje šifrirane korisničke podatke.

## <a name="encryption-mechanism"></a>Mehanizam za šifriranje  
Klijentska biblioteka za pohranu koristi [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) da biste šifrirali korisničkih podataka. Konkretno, [Ulančavanje snaga bloka (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) način rada s AES. Oba servisa funkcionira drugačije, pa ne možemo obrađuje svaku od njih ovdje.

### <a name="blobs"></a>Blob-ova
Klijentska biblioteka trenutno podržava šifriranje samo cijelu blob polja. Konkretno, podržava šifriranje kad korisnici rabe **Stvaranje*** metode. Podržani su preuzimanja, oba dovršavanje i datotekama za preuzimanje raspon te parallelization prijenos i preuzimanje je dostupna.

Tijekom šifriranja, klijentska biblioteka će generiranje s izravnim Inicijalizacija vektorska (IV) od 16 bajta, zajedno s izravnim šifriranja sadržaja ključ (CEK) 32 bajtova i izvršiti šifriranje omotnice blob podataka pomoću ovih informacija. Prelomljeni CEK i neke dodatne šifriranje metapodatke zatim pohraniti kao bloba metapodataka uz Šifrirani blob na servis.

>[AZURE.WARNING] Ako su uređivanje ili prijenosom vlastite metapodataka za blob-om, koje je potrebno provjeriti zadržava se ova metapodataka. Ako prenesete novi metapodataka bez ti metapodaci, prelomljeni CEK, IV i drugih metapodataka bit će izgubljeni i sadržaj blob nikad neće biti veličina ponovno.

Preuzimanje šifriranih blob obuhvaća dohvaćanje sadržaj cijele blob pomoću **dobiti*** praktičnost metode. Prelomljeni CEK je unwrapped i koristiti zajedno s IV (spremljen kao blob metapodataka u tom slučaju) da biste se vratili dešifriranu podataka korisnicima.

Preuzimanje proizvoljne raspona (**dobiti*** metode s parametrima raspon proslijeđen) u Šifrirani blob obuhvaća prilagodbu raspona koje ste dobili od korisnika da biste dobili mali dodatnih podataka koji se mogu koristiti za uspješno dešifriranje Traženi raspon.

Blokiranje blob-ova i blob-Ova stranica samo mogu biti šifrirane/dešifrira pomoću ovu shemu. Trenutno ne postoji podrška za šifriranje dodavanje blob-ova.

### <a name="queues"></a>Reda čekanja
Budući da se poruke reda čekanja može biti bilo koje oblika, klijentska biblioteka definira prilagođeni oblik koji uključuje vektorski inicijalizaciju (IV) i tipku šifrirane šifriranja sadržaja (CEK) u tekst poruke.

Tijekom šifriranja, biblioteka klijenta generira slučajni IV 16 bajtova zajedno s izravnim CEK 32 bajtova i izvodi omotnica šifriranje tekst poruke reda čekanja pomoću ovih informacija. Prelomljeni CEK i neke dodatne šifriranje metapodatke zatim dodaju red šifriranu poruku. Ova izmijenjene poruka (prikazano u nastavku) se pohranjuje na servis.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Tijekom dešifriranja, tipku prelomljeni dobivenih iz reda čekanja poruke i unwrapped. Na IV je dobivenih iz reda čekanja poruku i koristiti uz tipku unwrapped dešifrirati podatke o poruci red. Imajte na umu da metapodataka za šifriranje small (u odjeljku 500 bajtova), tako da tijekom Brojanje prema ograničenje 64KB reda čekanja poruka, utjecaja biti moguće upravljati.

### <a name="tables"></a>Tablica
Klijentska biblioteka podržava šifriranje entitet svojstava za umetanje i zamijenite operacije.

>[AZURE.NOTE] Spajanje trenutno nije podržano. Budući da podskup svojstava možda šifrirane prethodno pomoću drugi ključ, jednostavno spajanje nova svojstva i ažuriranje metapodatke rezultirat će gubitak podataka. Spajanje ili zahtijeva upućivanje poziva dodatnih servisa da biste pročitali postojećih entitet iz servisa, ili pomoću novog ključa po svojstvu koje su oba nije prikladna za boljih performansi.

Šifriranje podataka tablice funkcionira na sljedeći način:

1.  Korisnici odredili svojstva šifriranje.

2.  Klijentska biblioteka generira na slučajni Inicijalizacija vektorski (IV) 16 bajtova zajedno s izravnim sadržaja ključa za šifriranje (CEK) 32 bajtova za svaki entitet, a izvodi omotnica šifriranje pojedine svojstava šifriranje po dijelovima koji potječu novi IV po svojstvu. Svojstvo šifrirane pohranjen kao binarne podatke.

3.  Prelomljeni CEK i neke dodatne šifriranje metapodatke zatim pohranjenih kao rezervirana dva dodatna svojstva. Prvi rezervirane svojstvo (\_ClientEncryptionMetadata1) je svojstvo niz koji sadrži informacije o IV, verzije i prelomljeni ključ. Drugi rezervirane svojstvo (\_ClientEncryptionMetadata2) je binarni svojstvo koja sadrži informacije o svojstvima šifrirane. Informacije u drugom svojstvo (\_ClientEncryptionMetadata2) je i sama šifrirane.

4.  Zbog ove dodatne rezervirane svojstva potrebna za šifriranje, korisnici sad mogla imati samo 250 prilagođenih svojstava umjesto 252. Ukupnu veličinu entitet mora biti manja od 1MB.

    Imajte na umu da se mogu šifrirane samo niz svojstava. Ako su druge vrste svojstava šifriranje, morate se pretvaraju u nizove. Šifrirane nizovi pohranjuju se na servis kao binarni svojstva, a mogu biti pretvoreni natrag nizovi (neobrađenog nizovi, EntityProperties s vrstom EdmType.STRING) nakon dešifriranje.

    Za tablice, osim pravila šifriranja korisnici morate navesti svojstva šifriranje. Možete učiniti i spremanje tih svojstava u TableEntity objekte s vrsta postavljena na EdmType.STRING i šifriranje postavljen na true ili postavljanje na encryption_resolver_function na tableservice objektu. Razrješavanje za šifriranje je funkcija koja se koristi particije ključ, ključ retka i naziv svojstva i vraća Booleove vrijednosti koji označava hoće li biti svojstvo šifrirane. Tijekom šifriranja, klijentska biblioteka će koristiti taj podatak odlučiti hoće li biti svojstvo šifrirane tijekom pisanja u žičani. Delegat također nudi mogućnost nastanka logike oko kako šifrirane svojstava. (Na primjer, ako je X, zatim šifriranje svojstvo odgovora; u suprotnom šifriranje svojstva A i B.) Imajte na umu da nije potrebno navedite informacije tijekom čitanja ili slanje upita entiteti.

### <a name="batch-operations"></a>Obradu operacije
Jedan pravila šifriranja odnosi se na sve retke u seriji. Klijentska biblioteka interno generirat će novi slučajni IV i izravnim CEK po retku u seriji. Korisnicima možete odabrati i šifriranje različita svojstva za svaku operaciju u seriji definiranjem takvo ponašanje u Razrješavanje šifriranje.
Ako seriji stvaranja kao upravitelj kontekst metodom batch() tableservice pravila za šifriranje s tableservice automatski će se primijeniti na obradu. Ako seriji stvorili izričito tako da nazovete podršku za Graditelj, pravila za šifriranje mora biti proslijeđena kao parametar i lijevo nepromijenjenu za vrijeme trajanja grupe.
Imajte na umu da su entiteti šifrirane kao što je umetnuti u grupe pomoću pravila za šifriranje u seriji (entiteti nisu šifrirane prilikom prosljeđivanja grupe putem pravila za šifriranje s tableservice).

### <a name="queries"></a>Upiti
Za izvođenje operacije upita, morate navesti ključ Razrješavanje koji je možete riješiti tipke u skupu rezultata. Ako je entitet, koje se nalaze u rezultatu upita ne može riješiti s davateljem, klijentska biblioteka će vratiti pogrešku. Za bilo koji upit koji izvršava projekcija strani poslužitelja klijentska biblioteka dodat će svojstava metapodataka posebno šifriranja (\_ClientEncryptionMetadata1 i \_ClientEncryptionMetadata2) po zadanom odabrane stupce.

>[AZURE.IMPORTANT] Imajte na umu sljedeće važne stavke prilikom korištenja klijentsko šifriranja:
>
>- Kada čitanju ili pisanje Šifrirani blob, koristiti cijeli blob prijenos i raspon/cijeli blob preuzimanje naredbama. Izbjegavajte pisanje na Šifrirani blob pomoću protokola operacija poput staviti blok, postavite popis blokova, pisanje stranice ili Očisti stranice; u suprotnom možda oštećena Šifrirani blob i učinite je pronašao.
>
>- Za tablice, slično ograničenja postoji. Pazite da ne ažuriraju šifrirane svojstva bez ažuriranja metapodataka za šifriranje.
>
>- Ako ste postavili metapodataka na Šifrirani blob, može prebrisati potrebne za dešifriranje, budući da se postavka metapodataka nije additive metapodataka vezane uz šifriranje. To vrijedi i za snimke; Izbjegavajte navodeći metapodataka prilikom stvaranja snimke Šifrirani blob. Ako metapodataka mora biti postavljeno, provjerite jeste li pozvati metodu **get_blob_metadata** da biste pristupili trenutne metapodatke šifriranje, najprije i izbjegavajte Istodobni zapisivanja dok postavljen metapodataka.
>
>- Omogućivanje zastavicu **require_encryption** na objekt servisa za korisnike koji treba funkcioniraju samo s šifrirane podatke. Dodatne informacije potražite u nastavku.

Klijentska biblioteka za pohranu očekuje navedeni KEK i ključa Razrješavanje implementaciju sljedeće sučelja. [Azure ključ sigurnog](https://azure.microsoft.com/services/key-vault/) podrška za upravljanje Python KEK čeka i će integriran u tu biblioteku kada završi.


## <a name="client-api--interface"></a>Klijentski API / sučelja
Nakon stvaranja objekt servisa za pohranu (odnosno blockblobservice) korisnik može dodijeliti vrijednosti polja koja čine pravilo šifriranja: key_encryption_key, key_resolver_function, i require_encryption. Korisnicima možete unijeti samo KEK, samo prevoditelja ili i jedno i drugo. key_encryption_key je vrsta osnovni ključa koja je označena pomoću ključa identifikator i koji omogućuje logiku za prelamanje/unwrapping. key_resolver_function koristi se za rješavanje ključa tijekom postupka dešifriranja. Vraća valjani KEK dali identifikatora ključa. To omogućuje korisnicima da biste odabrali jednoliko više tipki se upravlja na više mjesta.

Na KEK morate provesti sljedećih metoda uspješno šifriranje podataka:
- wrap_key(cek): prelama navedeni CEK (bajtova) pomoću algoritam korisnika po izboru. Vraća prelomljeni ključ.
- get_key_wrap_algorithm(): vraća algoritam koji se koristi da bi se tipke.
- get_kid(): vraća identifikator ključa niz za ovaj KEK.
Na KEK morate provesti sljedećih metoda uspješno dešifrirati podataka:
- unwrap_key (cek, algoritam): vraća unwrapped obliku navedenom CEK pomoću niza navedena algoritma.
- get_kid(): vraća niz ključa ID-a za ovaj KEK.

Ključni Razrješavanje barem morate provesti onaj koji, navedene ključne id, vraća odgovarajuće KEK implementacijom iznad sučelja. Samo ta je metoda želite dodijeliti key_resolver_function svojstva objekta servisa.

- Za šifriranje, uvijek koristi tipku i Izostanak ključ uzrokovat će pogrešku.
- Za dešifriranje:
    - Ključni Razrješavanje se poziva Ako navedeno da biste dobili ključ. Ako se Razrješavanje nije naveden, ali nema mapiranja za identifikator ključa, pogreška će se Expression.Error.
    - Ako Razrješavanje nije naveden, ali je navedeno ključ, tipku koristit će se ako njegov identifikator odgovara potrebna identifikator ključa. Ako je identifikator ne odgovara, pogreška će se Expression.Error.

      Šifriranje uzoraka u azure.storage.samples <fix URL>demonstrirati detaljnije završetka do kraja scenarij za blob-ova, redovima i tablice.
        Ogledni implementacije KEK i ključa Razrješavanje navedene na ogledne datoteke kao KeyWrapper i KeyResolver redom.

### <a name="requireencryption-mode"></a>Način RequireEncryption
Po želji, korisnici mogu omogućiti način operacija, gdje svi prijenosi i preuzimanja mora biti šifrirana. U tom načinu pokušaja prijenos podataka bez pravilo šifriranje ili preuzimanje podataka koji nisu šifrirani na servis neće uspjeti na klijentskom računalu. Zastavica **require_encryption** na objekt servisa kontrole takvo ponašanje.

### <a name="blob-service-encryption"></a>Šifriranje blob servisa
Postavljanje šifriranja pravila polja na blockblobservice objektu. Što još će se rukovati klijentska biblioteka interno.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_block_blob_service.key_encryption_key = kek
    my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

    # Upload the encrypted contents to the blob.
    my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

    # Download and decrypt the encrypted contents from the blob.
    blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)

### <a name="queue-service-encryption"></a>Red čekanja servisa šifriranje
Postavljanje šifriranja pravila polja na queueservice objektu. Što još će se rukovati klijentska biblioteka interno.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_queue_service.key_encryption_key = kek
    my_queue_service.key_resolver_funcion = key_resolver.resolve_key

    # Add message
    my_queue_service.put_message(queue_name, content)

    # Retrieve message
    retrieved_message_list = my_queue_service.get_messages(queue_name)

### <a name="table-service-encryption"></a>Šifriranje servisa tablice
Osim stvaranja pravila šifriranje i postavljanje mogućnosti zahtjeva, morate navesti **encryption_resolver_function** **tableservice**, ili postaviti atribut šifriranje u EntityProperty.

### <a name="using-the-resolver"></a>Korištenje na prevoditelja

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Define the encryption resolver_function.
    def my_encryption_resolver(pk, rk, property_name):
            if property_name == 'foo':
                    return True
            return False

    # Set the KEK and key resolver on the service object.
    my_table_service.key_encryption_key = kek
    my_table_service.key_resolver_funcion = key_resolver.resolve_key
    my_table_service.encryption_resolver_function = my_encryption_resolver

    # Insert Entity
    my_table_service.insert_entity(table_name, entity)

    # Retrieve Entity
    # Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
    my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])

### <a name="using-attributes"></a>Korištenje atributa
Kao što je rečeno iznad svojstvo možda obilježena za šifriranje pohrana u objekt EntityProperty i postavljanjem polja za šifriranje.

    encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)

## <a name="encryption-and-performance"></a>Šifriranje i performanse
Imajte na umu da šifriranje rezultate za pohranu podataka u indirektni dodatne performansi. Sadržaja ključ i IV se generirati, mora biti šifrirane sadržaj i dodatne metapodatke moraju biti oblikovani i prenijeti. U ovom indirektni ovise o količinu podataka šifriranje. Preporučujemo da korisnici uvijek testirajte svoje aplikacije performanse prilikom razvoja.

## <a name="next-steps"></a>Daljnji koraci

- Preuzimanje [Klijentska biblioteka za pohranu Azure paketa Java PyPi](https://pypi.python.org/pypi/azure-storage)
- Preuzimanje [Azure prostora za pohranu klijenta biblioteka za Python izvora koda iz GitHub](https://github.com/Azure/azure-storage-python)
