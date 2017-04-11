<properties
    pageTitle="Klijentsko šifriranja s Java za pohranu Microsoft Azure | Microsoft Azure"
    description="U biblioteci klijenta za Azure prostora za pohranu za Java podržava klijentsko šifriranja i integracija s sigurnog Azure ključ za Maksimalna sigurnosti za pohranu Azure aplikacije."
    services="storage"
    documentationCenter="java"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-java-for-microsoft-azure-storage"></a>Klijentsko šifriranja s Java za pohranu Microsoft Azure   

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Pregled  

[Azure prostora za pohranu klijenta biblioteke za Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) podržava šifriranje podataka u klijentskim aplikacijama prije prijenosa Azure prostora za pohranu i dešifriranja podataka tijekom preuzimanja klijentskog programa. Biblioteku podržava i integracija s [Sigurnog Azure ključ](https://azure.microsoft.com/services/key-vault/) za upravljanje ključevima za račun za pohranu.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Šifriranje i dešifriranje putem tehnika omotnice    
Procesi šifriranje i dešifriranje slijedite postupak omotnice.  

### <a name="encryption-via-the-envelope-technique"></a>Šifriranje putem tehnika omotnice  
Šifriranje putem omotnica postupak funkcionira na sljedeći način:  

1.  Biblioteka klijentski Azure spremišta generira ključ šifriranja sadržaja (CEK), što je ključ simetričnu one-time se koristi.  

2.  Korisnički podaci je šifriran pomoću ovog CEK.   

3.  Na CEK pa je omotan (šifrirane) pomoću ključa za šifriranje ključa (KEK). Na KEK označena ključa identifikator i možete biti asimetričnim ključa par ili simetričnu ključ i možete se upravlja lokalno ili spremljene u sefovi ključ Azure.  
Biblioteka za pohranu klijenta sam nikad ima pristup KEK. Biblioteku poziva algoritam ključa prelamanje koju je dodijelio sigurnog ključ. Da biste koristili prilagođeni davatelja usluga za ključne prelamanje/unwrapping po želji možete odabrati korisnika.  

4.  Šifrirani podaci pa prenesene sa servisom Azure prostora za pohranu. Tipku prelomljeni uz neke dodatne šifriranje metapodatke pohranjenih kao metapodatke (s blob) ili nulama sa šifriranim (reda čekanja poruke i entiteti tablice).

### <a name="decryption-via-the-envelope-technique"></a>Dešifriranje putem tehnika omotnice  
Dešifriranje putem omotnica postupak funkcionira na sljedeći način:  

1.  Klijentska biblioteka pretpostavlja da korisnik je Upravljanje ključem za šifriranje ključa (KEK) bilo lokalno ili u sefovi ključ Azure. Korisnik ne morate znati određenog ključ koji je korišten za šifriranje. Umjesto toga ključa Razrješavanje koja se odnosi različite ključa identifikatora tipke mogu biti postavljanje sustavu i koristiti.  

2.  Klijentska biblioteka preuzimanje šifriranih podataka uz svaki materijal šifriranja koja je spremljena na servis.  

3.  Ključ za prelomljeni sadržaja šifriranje (CEK) je, a zatim unwrapped (dešifriranu) pomoću ključa šifriranja ključ (KEK). Ovdje ponovno biblioteku klijent neće imati pristup KEK. Jednostavno poziva prilagođena ili unwrapping algoritam sigurnog ključ davatelja.  

4.  Ključ za šifriranje sadržaja (CEK) koristi se za dešifriranje šifrirane korisničke podatke.

## <a name="encryption-mechanism"></a>Mehanizam za šifriranje  
Klijentska biblioteka za pohranu koristi [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) da biste šifrirali korisničkih podataka. Konkretno, [Ulančavanje snaga bloka (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) način rada s AES. Oba servisa funkcionira drugačije, pa ne možemo obrađuje svaku od njih ovdje.

### <a name="blobs"></a>Blob-ova  
Klijentska biblioteka trenutno podržava šifriranje samo cijelu blob polja. Konkretno, podržava šifriranje kad korisnici rabe **Prijenos** * metode ili * *openOutputStream** način. Za podržane su preuzimanja, oba dovršavanje i datotekama za preuzimanje raspon.  

Tijekom šifriranja, klijentska biblioteka će generiranje s izravnim Inicijalizacija vektorski (IV) od 16 bajta, zajedno s izravnim šifriranja sadržaja ključ (CEK) 32 bajtova i izvoditi omotnica šifriranje blob podataka pomoću ovih informacija. Prelomljeni CEK i neke dodatne šifriranje metapodatke zatim pohraniti kao bloba metapodataka uz Šifrirani blob na servis.

>[AZURE.WARNING] Ako su uređivanje ili prijenosom vlastite metapodataka za blob-om, koje je potrebno provjeriti zadržava se ova metapodataka. Ako prenesete novi metapodataka bez ti metapodaci, prelomljeni CEK, IV i drugih metapodataka bit će izgubljeni i sadržaj blob nikad neće biti veličina ponovno.

Preuzimanje šifriranih blob obuhvaća dohvaćanje sadržaj cijele blob pomoću * *Preuzimanje*/openInputStream** praktičnost metode. Prelomljeni CEK je unwrapped i koristiti zajedno s IV (spremljen kao blob metapodataka u tom slučaju) da biste se vratili dešifriranu podataka korisnicima.

Preuzimanje proizvoljne raspona (**downloadRange*** metoda) u Šifrirani blob obuhvaća prilagodbu raspona koje ste dobili od korisnika da biste dobili mali dodatnih podataka koji se mogu koristiti za uspješno dešifriranje Traženi raspon.  

Sve bloba vrste (blokirati blob-ova, stranica blob-ova i dodavanje blob-ova) moguće šifrirane/dešifrirati pomoću ovu shemu.

### <a name="queues"></a>Reda čekanja  
Budući da se poruke reda čekanja može biti bilo koje oblika, klijentska biblioteka definira prilagođeni oblik koji uključuje vektorski inicijalizaciju (IV) i tipku šifrirane šifriranja sadržaja (CEK) u tekst poruke.  

Tijekom šifriranja, biblioteka klijenta generira slučajni IV 16 bajtova zajedno s izravnim CEK 32 bajtova i izvodi omotnica šifriranje tekst poruke reda čekanja pomoću ovih informacija. Prelomljeni CEK i neke dodatne šifriranje metapodatke zatim dodaju red šifriranu poruku. Ova izmijenjene poruka (prikazano u nastavku) se pohranjuje na servis.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Tijekom dešifriranja, tipku prelomljeni dobivenih iz poruke red i unwrapped. Na IV je dobivenih iz reda čekanja poruku i koristiti uz tipku unwrapped dešifrirati podatke o poruci red. Imajte na umu da metapodataka za šifriranje small (u odjeljku 500 bajta), tako da tijekom Brojanje prema 64KB ograničenje čekanja poruke, utjecaja biti moguće upravljati.

### <a name="tables"></a>Tablica  
Klijentska biblioteka podržava šifriranje entitet svojstava za umetanje i zamijenite operacije.

>[AZURE.NOTE] Spajanje trenutno nije podržano. Budući da podskup svojstava možda šifrirane prethodno pomoću drugi ključ, jednostavno spajanju nova svojstva i ažuriranje metapodatke rezultirat će gubitak podataka. Spajanje ili zahtijeva upućivanje poziva dodatnih servisa da biste pročitali postojećih entitet iz servisa, ili pomoću novog ključa po svojstvu koje su oba nije prikladna za boljih performansi.

Šifriranje podataka tablice funkcionira na sljedeći način:  

1.  Korisnici navedite svojstva šifriranje.  

2.  Klijentska biblioteka generira na slučajni Inicijalizacija vektorski (IV) 16 bajtova zajedno s izravnim sadržaja ključa za šifriranje (CEK) 32 bajtova za svaki entitet, a izvodi omotnica šifriranje pojedine svojstava šifriranje po dijelovima koji potječu novi IV po svojstvu. Svojstvo šifrirane pohranjen kao binarne podatke.  

3.  Prelomljeni CEK i neke dodatne šifriranje metapodatke zatim pohranjenih kao rezervirana dva dodatna svojstva. Prvi rezervirane svojstvo (_ClientEncryptionMetadata1) je svojstvo niz koji sadrži informacije o IV, verzije i prelomljeni ključ. Binarni svojstvo koja sadrži informacije o svojstvima šifrirane je drugi rezervirano svojstvo (_ClientEncryptionMetadata2). Informacije u drugom svojstvo (_ClientEncryptionMetadata2) se sama šifrirane.  

4.  Zbog ove dodatne rezervirane svojstva potrebna za šifriranje, korisnici sad mogla imati samo 250 prilagođenih svojstava umjesto 252. Ukupna veličina entitet mora biti manja od 1MB.  

    Imajte na umu da samo niz svojstava mogu biti šifrirane. Ako su druge vrste svojstava šifriranje, morate se pretvaraju u nizove. Šifrirana nizovi pohranjuju se na servis kao binarni svojstva, a pretvaranja natrag u nizove nakon dešifriranje.

    Za tablice, osim pravila šifriranja korisnici morate navesti svojstva šifriranje. To možete učiniti navođenjem ili atribut [šifriranje] (za POCO entiteti proizlaziti iz TableEntity) ili je Razrješavanje šifriranje u mogućnostima zahtjev. Razrješavanje za šifriranje je ovlaštenika koji zauzima particija ključ, ključ retka i naziv svojstva i vraća Booleove vrijednosti koji označava hoće li biti svojstvo šifrirane. Tijekom šifriranja, klijentska biblioteka će koristiti taj podatak odlučiti hoće li biti svojstvo šifrirane tijekom pisanja u žičani. Delegat također nudi mogućnost nastanka logike oko kako šifrirane svojstva. (Na primjer, ako je X, zatim šifriranje svojstvo odgovora; u suprotnom šifriranje svojstva A i B.) Imajte na umu da nije potrebno navedite informacije tijekom čitanja ili slanje upita entiteti.

### <a name="batch-operations"></a>Obradu operacije  
U grupe operacija, iste KEK koristit će se preko svih redaka u tom skupne operacije jer klijentska biblioteka omogućuje samo jedan objekt mogućnosti (i zato jedan pravila/KEK) po skupne operacije. Međutim, klijentska biblioteka interno generirat će novi slučajni IV i izravnim CEK po retku u seriji. Korisnicima možete odabrati i šifriranje različita svojstva za svaku operaciju u seriji definiranjem takvo ponašanje u Razrješavanje šifriranje.

### <a name="queries"></a>Upiti  
Za izvođenje operacije upita, morate navesti ključ Razrješavanje koji je možete riješiti tipke u skupu rezultata. Ako je entitet, koje se nalaze u rezultatu upita ne može riješiti s davateljem, klijentska biblioteka će vratiti pogrešku. Za bilo koji upit koji izvršava projekcija strani poslužitelja klijentska biblioteka dodat će svojstava metapodataka posebno šifriranja (_ClientEncryptionMetadata1 i _ClientEncryptionMetadata2) po zadanom odabrane stupce.

## <a name="azure-key-vault"></a>Azure sigurnog ključa  
Azure ključ sigurnog olakšava zaštitu tipke šifriranja i tajne koristi oblaka aplikacija i servisa. Pomoću sigurnog ključ Azure korisnicima Šifrirajte ključeva i tajne (primjerice provjeru autentičnosti tipki, tipke račun za pohranu, ključeva za šifriranje podataka. PFX datotekama i lozinke) pomoću tipki zaštićene hardver sigurnost Module (HSMs). Dodatne informacije potražite u članku [što je sigurnog ključ Azure?](../key-vault/key-vault-whatis.md).

Klijentska biblioteka za pohranu koristi biblioteka core sigurnog ključ da bi se omogućuje zajednički framework Azure upravljanja tipke. Korisnicima se i dodatnih pogodnosti korištenja biblioteka proširenja sigurnog ključ. Biblioteka proširenja nudi korisne funkcije oko jednostavni i objedinjenog Symmetric/RSA lokalne i ključa davatelji oblaka, kao i s zbrajanja i predmemoriranja.

### <a name="interface-and-dependencies"></a>Sučelje i ovisnosti  
Postoje tri paketa sigurnog ključ:  

- Azure keyvault core sadrži IKey i IKeyResolver. To je small paket s bez ovisnosti. Klijent biblioteka za pohranu za Java se definira kao ovisnosti.
- Azure keyvault sadrži ključ sigurnog OSTALE klijenta.  
- Azure keyvault proširenja sadrži nastavak kod koji uključuje implementacije šifriranja algoritmima pomoću programa RSAKey i na SymmetricKey. Ovisi o prostori Core i KeyVault i funkcija da biste definirali zbrajanja Razrješavanje (kada korisnici žele jedinstvene prijave pomoću više ključa) i predmemoriranja ključa prevoditelja. Iako klijentska biblioteka za pohranu izravno ovisi o pakirati, ako želite da korisnici da biste koristili sigurnog Azure ključ za pohranu ključ ili zauzeti lokalnoj i cloud šifrirane davatelji pomoću sigurnog ključ extensions, morat ćete taj paket.  

  Ključ sigurnog namijenjen je jakih matrica tipke i regulacije ograničenja po ključ sigurnog dizajnirani Ovime na umu. Prilikom izvršavanja klijentsko šifriranja s ključ sigurnog Preferirani modela je tipkama simetričnu matrica spremljen kao tajne u sigurnog ključ i predmemorirani lokalno. Korisnici moraju učinite sljedeće:  

1.  Stvaranje tajna izvanmrežno, a zatim prenese na sigurnog ključ.  

2.  Na tajna osnovni identifikator možete koristiti kao parametar riješiti trenutnu verziju tajna za šifriranje i predmemoriju lokalno taj podatak. Korištenje CachingKeyResolver za predmemoriranje; korisnici ne očekujete da će implementirati vlastite predmemoriranje logike.  

3.  Koristite predmemoriranja Razrješavanje kao ulaz prilikom stvaranja pravila za šifriranje.
Dodatne informacije vezane uz korištenje ključ sigurnog pronaći ćete u primjere koda za šifriranje. <fix URL>  

## <a name="best-practices"></a>Najbolje prakse  
Podrška za šifriranje dostupna je samo u biblioteci za pohranu klijenta za Java.

>[AZURE.IMPORTANT] Imajte na umu sljedeće važne stavke prilikom korištenja klijentsko šifriranja:
>  
>- Kada čitanju ili pisanje Šifrirani blob, koristiti cijeli blob prijenos i raspon/cijeli blob preuzimanje naredbama. Izbjegavajte pisanje na Šifrirani blob pomoću protokola operacija poput staviti blok, postavite popis blokiranih, pisanje stranice, Očisti stranice ili bloka Dodavanje u suprotnom možda oštećena Šifrirani blob i učinite je pronašao.
>
>- Za tablice, slično ograničenja postoji. Pazite da ne ažuriraju šifrirane svojstva bez ažuriranja metapodataka za šifriranje.  
>
>- Ako ste postavili metapodataka na Šifrirani blob, može prebrisati potrebne za dešifriranje, budući da se postavka metapodataka nije additive metapodataka vezane uz šifriranje. To vrijedi i za snimke; Izbjegavajte navodeći metapodataka prilikom stvaranja snimke Šifrirani blob. Ako metapodataka mora biti postavljeno, provjerite jeste li pozvati metodu **downloadAttributes** da biste pristupili trenutne metapodatke šifriranje, najprije i izbjegavajte Istodobni zapisivanja dok postavljen metapodataka.  
>
>- Omogućivanje zastavicu **requireEncryption** u zadane mogućnosti zahtjeva za korisnike koji treba funkcioniraju samo s šifrirane podatke. Dodatne informacije potražite u nastavku.  

## <a name="client-api--interface"></a>Klijentski API / sučelja  
Prilikom stvaranja objekta EncryptionPolicy korisnicima možete unijeti samo ključ (implementacijom IKey), samo na Razrješavanje (implementacijom IKeyResolver) ili oboje. IKey je vrsta osnovni ključa koja je označena pomoću ključa identifikator i koji omogućuje logiku za prelamanje/unwrapping. IKeyResolver koristi se za rješavanje ključa tijekom postupka dešifriranja. Definira metodu ResolveKey koji vraća programa IKey navedene ključne identifikator. To omogućuje korisnicima da biste odabrali jednoliko više tipki se upravlja na više mjesta.

- Za šifriranje, uvijek koristi tipku i Izostanak ključ uzrokovat će pogrešku.  
- Za dešifriranje:  
    - Ključni Razrješavanje se poziva Ako navedeno da biste dobili ključ. Ako se Razrješavanje nije naveden, ali nema mapiranja za identifikator ključa, pogreška će se Expression.Error.  
    - Ako Razrješavanje nije naveden, ali je navedeno ključ, tipku koristit će se ako njegov identifikator odgovara potrebna identifikator ključa. Ako je identifikator ne odgovara, pogreška će se Expression.Error.  

      [Šifriranje uzoraka](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) <fix URL>along demonstrirati detaljnije scenarij završetka do kraja blob-ova, redovima i tablice, uz integraciju sigurnog ključ.

### <a name="requireencryption-mode"></a>Način RequireEncryption  
Po želji, korisnici mogu omogućiti način operacija, gdje svi prijenosi i preuzimanja mora biti šifrirana. U tom načinu pokušaja prijenos podataka bez pravilo šifriranje ili preuzimanje podataka koji nisu šifrirani na servis neće uspjeti na klijentskom računalu. Zastavica **requireEncryption** mogućnosti objekta zahtjev kontrole takvo ponašanje. Ako aplikacija će šifriranje svih objekata pohranjenih na Azure prostora za pohranu, možete postaviti svojstvo **requireEncryption** na zadane mogućnosti zahtjeva za klijentski objekt servisa.   

Na primjer, pomoću **CloudBlobClient.getDefaultRequestOptions().setRequireEncryption(true)** šifriranje potrebno za sve blob operacije obavljene putem klijenta objekt.

### <a name="blob-service-encryption"></a>Šifriranje blob servisa  
Stvaranje objekta **BlobEncryptionPolicy** i postavite ga u mogućnostima zahtjev (po API-JA ili na razini klijenta pomoću **DefaultRequestOptions**). Što još će se rukovati klijentska biblioteka interno.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

    // Set the encryption policy on the request options.
    BlobRequestOptions options = new BlobRequestOptions();
    options.setEncryptionPolicy(policy);

    // Upload the encrypted contents to the blob.
    blob.upload(stream, size, null, options, null);

    // Download and decrypt the encrypted contents from the blob.
    ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
    blob.download(outputStream, null, options, null);

### <a name="queue-service-encryption"></a>Red čekanja servisa šifriranje  
Stvaranje objekta **QueueEncryptionPolicy** i postavite ga u mogućnostima zahtjev (po API-JA ili na razini klijenta pomoću **DefaultRequestOptions**). Što još će se rukovati klijentska biblioteka interno.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

    // Add message
    QueueRequestOptions options = new QueueRequestOptions();
    options.setEncryptionPolicy(policy);

    queue.addMessage(message, 0, 0, options, null);

    // Retrieve message
    CloudQueueMessage retrMessage = queue.retrieveMessage(30, options, null);

### <a name="table-service-encryption"></a>Šifriranje servisa tablice  
Osim stvaranja pravila šifriranje i postavljanje mogućnosti zahtjeva, morate navesti **EncryptionResolver** u **TableRequestOptions**, ili postaviti atribut [šifriranje] u entitet dobavljač i postavljač.

### <a name="using-the-resolver"></a>Korištenje na prevoditelja  

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

    TableRequestOptions options = new TableRequestOptions()
    options.setEncryptionPolicy(policy);
    options.setEncryptionResolver(new EncryptionResolver() {
        public boolean encryptionResolver(String pk, String rk, String key) {
            if (key == "foo")
            {
                return true;
            }
            return false;
        }
    });

    // Insert Entity
    currentTable.execute(TableOperation.insert(ent), options, null);

    // Retrieve Entity
    // No need to specify an encryption resolver for retrieve
    TableRequestOptions retrieveOptions = new TableRequestOptions()
    retrieveOptions.setEncryptionPolicy(policy);

    TableOperation operation = TableOperation.retrieve(ent.PartitionKey, ent.RowKey, DynamicTableEntity.class);
    TableResult result = currentTable.execute(operation, retrieveOptions, null);

### <a name="using-attributes"></a>Korištenje atributa  
Kao što je rečeno iznad, ako je entitet implementira TableEntity, a zatim svojstva dobavljač i postavljač možete ukrašene s atributom [šifriranje] umjesto navodeći **EncryptionResolver**.

    private string encryptedProperty1;

    @Encrypt
    public String getEncryptedProperty1 () {
        return this.encryptedProperty1;
    }

    @Encrypt
    public void setEncryptedProperty1(final String encryptedProperty1) {
        this.encryptedProperty1 = encryptedProperty1;
    }

## <a name="encryption-and-performance"></a>Šifriranje i performanse  
Imajte na umu da šifriranje rezultate za pohranu podataka u indirektni dodatne performansi. Mora biti generirana sadržaja ključ i IV, sadržaj mora biti šifrirane i dodatnih meta-podataka mora biti oblikovani i prenijeti. U ovom indirektni ovise o količinu podataka koji se šifrirane. Preporučujemo da korisnici uvijek testirajte svoje aplikacije performanse prilikom razvoja.

## <a name="next-steps"></a>Daljnji koraci  

- Preuzimanje [Azure prostora za pohranu klijenta biblioteke za Java Maven paketa](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)  
- Preuzimanje [Azure prostora za pohranu klijenta biblioteka za izvorni kod programskog jezika Java iz GitHub](https://github.com/Azure/azure-storage-java)   
- Preuzmite Maven biblioteka Azure sigurnog ključ za Java Maven paketa:
    - [Temeljni](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault-core) paketa
    - Paket [klijenta](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault)
- Posjetite [Azure ključ sigurnog dokumentacija](../key-vault/key-vault-whatis.md)  
