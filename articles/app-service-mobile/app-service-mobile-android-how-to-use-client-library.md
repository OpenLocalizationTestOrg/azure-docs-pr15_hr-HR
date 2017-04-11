<properties
    pageTitle="Upute za korištenje sa sustavom Android klijentska biblioteka za mobilne aplikacije"
    description="Upute za korištenje sa sustavom Android klijent SDK za Azure mobilne aplikacije."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>


# <a name="how-to-use-the-android-client-library-for-mobile-apps"></a>Kako koristiti biblioteku Android klijenta za mobilne aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ovaj vodič pokazuje kako koristiti sa sustavom Android klijent SDK za mobilne aplikacije za primjenu uobičajeni scenariji, kao što su:

- Slanje upita za podatke (Umetanje, ažuriranje i brisanje).
- Provjera autentičnosti.
- Obradu pogreške.
- Prilagodba klijent. 

I ne obuhvaća više razina dive u uobičajeni klijent kod koji se koristi u većinu mobilne aplikacije.

Ovaj vodič usredotočuje se na klijentskoj strani SDK za Android.  Dodatne informacije o poslužiteljsko SDK-ovi za mobilne aplikacije potražite u članku [Rad s pozadinskom .NET SDK] [ 10] ili [pomoću pozadinskog Node.js SDK][11].

## <a name="reference-documentation"></a>Dokumentacija reference

Možete pronaći [Javadocs API reference] [ 12] za biblioteku Android klijenta na GitHub.

## <a name="supported-platforms"></a>Podržane platforme

Azure Android SDK mobilna aplikacija podržava API razine 19 do 24 (KitKat do Nougat).  

Provjera autentičnosti "poslužitelj tijek" koristi s prikaza za izloženi korisničkog Sučelja. Ako je uređaj neće moći prikazati korisničko Sučelje prikaza, zatim druge načine provjere autentičnosti potrebni su koji se nalazi izvan opsega proizvoda.  U ovom SDK nije prikladna za nadzor vrsta ili na sličan način ograničena uređaja.

## <a name="setup-and-prerequisites"></a>Postavljanje i preduvjeti

Dovršite vodič za [brzi početak rada mobilne aplikacije](app-service-mobile-android-get-started.md) .  Ovaj zadatak omogućuje čitanje svih stara requisites za razvoj aplikacija za mobilne Azure zadovolje.  Za brzi početak rada olakšava konfiguriranje računa i stvaranje prve mobilnu aplikaciju pozadinskog sustava.

Ako odlučite ne da biste dovršili vodič za brzi početak rada, poduzmite sljedeće korake:

- [Stvaranje mobilnu aplikaciju pozadinskog] [ 13] za uporabu aplikacija za Android.
- U Android Studio [Ažuriranje datoteka Sastavi Gradle](#gradle-build).
- [Omogućivanje internet dozvola](#enable-internet).

###<a name="gradle-build"></a>Ažuriranje datoteke Sastavi Gradle

Promjena obje **build.gradle** datoteke:

1. Dodajte kod razine **build.gradle** datoteku *projekta* unutar oznake *buildscript* :

        buildscript {
            repositories {
                jcenter()
            }
        }

2. Dodajte kod datoteke razine **build.gradle** *Modul aplikacije* unutar oznake *ovisnosti* :

        compile 'com.microsoft.azure:azure-mobile-android:3.1.0'

    Najnoviju verziju trenutno 3.1.0. Podržane verzije su navedene [ovdje][14].

###<a name="enable-internet"></a>Omogućivanje internet dozvola

Da biste pristupili Azure, pokrenite aplikaciju morate imati dozvolu INTERNET koji je omogućen. Ako već nije omogućena, dodajte sljedeći redak koda **AndroidManifest.xml** datoteku:

    <uses-permission android:name="android.permission.INTERNET" />

## <a name="the-basics-deep-dive"></a>Dive precizno osnove

U ovom se odjeljku opisuje neke od kod u aplikaciju za brzi početak rada koja se odnosi na korištenje Azure mobilna aplikacija.  

###<a name="data-object"></a>Definiranje klase podataka klijenta

Da biste pristupili podataka iz tablice SQL Azure, definirajte klase podataka klijenta koji odgovaraju na tablice u mobilnu aplikaciju pozadinski. Primjeri u ovoj temi pretpostavlja tablice pod nazivom **ToDoItem**, koja ima sljedeće stupce:

- ID-a
- tekst
- Dovršavanje

Odgovarajući upisali objekta klijentskoj strani:

    public class ToDoItem {
        private String id;
        private String text;
        private Boolean complete;
    }

Kod nalazi se u datoteku s nazivom **ToDoItem.java**.

Ako SQL Azure tablica sadrži više stupaca, dodajte odgovarajuća polja za ovu klasu.  Na primjer, ako u DTO (objekt prijenos podataka) imali stupca prioritet cijeli broj, a zatim može dodati to polje, zajedno s web-mjesto radi i u okvir za postavljač načine:

    private Integer priority;

    /**
    * Returns the item priority
    */
    public Integer getPriority() {
        return mPriority;
    }
    
    /**
    * Sets the item priority
    *
    * @param priority
    *            priority to set
    */
    public final void setPriority(Integer priority) {
        mPriority = priority;
    }

Da biste saznali kako stvoriti dodatne tablice u vašem pozadinskog mobilne aplikacije, potražite u članku [Kako: definiranje kontroler tablice] [ 15] (.NET pozadinskog) ili [Definiranje tablice pomoću dinamičke sheme] [ 16] (Node.js pozadinskog). Za s pozadinskom Node.js postavku **jednostavno tablice** možete koristiti i [Azure portal].

###<a name="create-client"></a>Kako: Stvaranje kontekstu klijenta

Kod stvara **MobileServiceClient** objekt koji se koristi za pristup vašem pozadinskog mobilnu aplikaciju. Kod dolazi na `onCreate` način klase **aktivnosti** naveden u *AndroidManifest.xml* **POKRETAČ** kategorijama i **GLAVNE** akcija. Brzi početak rada kod odlazi u datoteci **ToDoActivity.java** .

        MobileServiceClient mClient = new MobileServiceClient(
            "MobileAppUrl", // Replace with the Site URL
            this)

U ovom kod zamijenite `MobileAppUrl` s URL-a u pozadinskom mobilnu aplikaciju, koje možete pronaći u [Azure portal] u plohu za vaše pozadinskog mobilnu aplikaciju. Za taj redak kod prikupiti i morate dodati sljedeća naredba **Uvoz** :

    import com.microsoft.windowsazure.mobileservices.*;

###<a name="instantiating"></a>Kako: Stvaranje referenci tablice

Najjednostavniji način upita ili mijenjati podatke u pozadinski je pomoću na *upisali model programiranja*, jer je Java svakako uneseni jezik. Ovaj model sadrži objedinjenog JSON serijalizacije i deserijalizacija pomoću [gson] [ 3] biblioteke prilikom slanja podataka između klijentske objekte i tablica u pozadinski Azure SQL.

Da biste pristupili tablice, prvo stvorite [MobileServiceTable] [ 8] objekt tako da nazovete podršku za **getTable** način na [MobileServiceClient][9].  Ta metoda ima dvije overloads:

    public class MobileServiceClient {
        public <E> MobileServiceTable<E> getTable(Class<E> clazz);
        public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
    }

U sljedećem kodu **mClient** je referenca na objekt MobileServiceClient.  Prvi preopterećenja koristi se gdje naziv klase i naziv tablice jednake i onu u koristi za brzi početak rada:

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);

Drugi preopterećenja koristi se kada se razlikuje od naziv klase naziv tablice: prvi parametar je naziv tablice.

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

###<a name="binding"></a>Kako: povezivanje podataka s korisničkog sučelja

Povezivanje podataka obuhvaća sljedeća tri odjeljka:

- Izvor podataka
- Izgled zaslona
- Prilagodnik ties dviju zajedno.

U našem uzorak koda smo vratiti podatke iz tablice mobilne aplikacije SQL Azure **ToDoItem** u polju. Aktivnost je uobičajeni obrazac za aplikacije za podatke.  Upiti baze podataka često vratiti zbirku redaka koji klijent prima na popisu ili polja. U ovom primjeru polje je izvor podataka.

Šifra navodi izgleda zaslona koja definira prikaz podataka koji će se pojaviti na uređaju.  Dva su vezane uz prilagodnik koji se u kod je datotečni nastavak od u **ArrayAdapter&lt;ToDoItem&gt; ** predmete.

#### <a name="layout"></a>Kako: definiranje izgleda

Izgled definira nekoliko isječci XML kod. Uz postojeći raspored, sljedeći kod predstavlja **Prikaz popisa** želimo popunjavanje s oglednim podacima poslužitelja.

    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>

Prethodni kod atribut *stavkapopisa* određuje id rasporeda za pojedinačni redak na popisu. Kod određuje potvrdnog okvira i njegov pridruženi tekst i dobiti instancirati jednom za svaku stavku na popisu. Ovaj izgled prikaže polje s **ID-a** , a složeniji raspored želite navesti dodatna polja u prikaz. Kod je u datoteci **row_list_to_do.xml** .

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">
        <CheckBox
            android:id="@+id/checkToDoItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/checkbox_text" />
    </LinearLayout>


#### <a name="adapter"></a>Kako: definiranje prilagodnika

Jer je izvor podataka za naše prikaz polja **ToDoItem**ćemo podklase naš prilagodnik iz programa **ArrayAdapter&lt;ToDoItem&gt; ** predmete. U ovom podklase daje prikaza za svaku **ToDoItem** pomoću **row_list_to_do** rasporeda.

U našem kod smo definirati sljedeće klasa koja je proširenje u **ArrayAdapter&lt;E&gt; ** klasa:

    public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {

Nadjačavanje metodu **getView** prilagodnika. Ako, na primjer:

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }

        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });


        return row;
    }

Ne možemo stvoriti instancu klase u našem aktivnosti koja na sljedeći način:

    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);

Drugi parametar za Graditelj ToDoItemAdapter je referenca na raspored. Ne možemo sada možete stvoriti **Prikaz popisa** i dodjeljivati prilagodnik za **Prikaz popisa**.

    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);

### <a name="api"></a>Struktura API-JA

Postupci tablice mobilne aplikacije i prilagođene API poziva su asinkronog. Korištenje objekata [budućnost] i [AsyncTask] za asinkronog metode koje obuhvaćaju upite, umeće, ažuriranja i brisanja. Korištenje futures olakšava obavlja više operacija na pozadini niti bez potrebe za Postupanje s više ugniježđene pozive.

Pregledajte datoteku **ToDoActivity.java** u programu project za brzi početak rada sa sustavom Android s [portala za Azure] primjer.

#### <a name="use-adapter"></a>Kako: korištenje prilagodnika

Sada ste spremni za korištenje povezivanje podataka. Sljedeći kod prikazuje kako doći do stavke u tablici i lokalnog prilagodnika unosi vraćene stavke.

    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }

Nazovite prilagodnik bilo kojem trenutku izmjena **ToDoItem** tablice. Budući da se izmjene gotovi na temelju zapisa po zapisa, rukovati jednim retkom umjesto zbirku. Kada umetnete stavku, pozivanje način za **Dodavanje** na prilagodnik; Prilikom brisanja, nazovite način **ukloniti** .

##<a name="querying"></a>Kako: podaci iz vaše pozadinskog mobilnu aplikaciju za upite

U ovom se odjeljku opisuje kako izdati upite za mobilnu aplikaciju pozadinski, što obuhvaća sljedeće zadatke:

- [Vraćanje svih stavki]
- [Filtriranje vraćenih podataka]
- [Sortiranje vraćenih podataka]
- [Vraćanje podataka na stranicama]
- [Odabrati određene stupce]
- [Načini CONCATENATE upita](#chaining)

### <a name="showAll"></a>Kako: vratiti sve stavke iz tablice

Sljedeći upit vraća sve stavke u tablici **ToDoItem** .

    List<ToDoItem> results = mToDoTable.execute().get();

Varijabla *rezultata* vraća skup iz upita u obliku na popisu rezultata.

### <a name="filtering"></a>Kako: filtar vraćenih podataka

Sljedeće izvršavanje upita vraća sve stavke iz tablice **ToDoItem** gdje **Dovršeno** jednako je **false**.

    List<ToDoItem> result = mToDoTable.where()
                                .field("complete").eq(false)
                                .execute().get();

**mToDoTable** je referenca na tablicu mobilnu uslugu koju smo stvorili prethodno.

Definiranje filtra pomoću metode poziva **gdje** u referenci tablice. Način **gdje** slijedi slijedi onaj koji određuje logički predikata metode **polja** . Mogući načini predikata obuhvaćaju **eq** (jednako), **No** (nije jednako), **gt** (veće od), **stranice** (veće od ili jednako), **lt** (manji od), **s** (manje od ili jednako). Načini omogućuju usporedite broj i niz polja za određene vrijednosti.

Možete filtrirati prema datumima. Na sljedeće načine omogućuju usporedite cijelu podatkovno polje ili dijelova rukom datum: **godinu**, **mjesec**, **dan**, **sat**, **minuta**i **drugi**. Sljedeći primjer dodaje filtra za stavke čiji *Datum krajnjeg roka* jednak 2013.

    mToDoTable.where().year("due").eq(2013).execute().get();

Na sljedeće načine podržava složenih filtara na poljima niz: **startsWith**, **endsWith**, **Slika**, **podniz**, **indexOf**, **zamijeniti**, **toLower**, **toUpper**, **obrezivanja**i **duljine**. U sljedećem primjeru filtre za retke tablice gdje stupca *teksta* započinje "PRI0."

    mToDoTable.where().startsWith("text", "PRI0").execute().get();

Na sljedeće načine operator podržani su na brojčana polja: **Dodavanje** **sub**, **mul**, **div**, **mod**, **floor**, **ceiling**te **zaokružiti broj**. U sljedećem primjeru filtre za koju je **trajanje** paran broj redaka tablice.

    mToDoTable.where().field("duration").mod(2).eq(0).execute().get();

Predikati možete kombinirati s metodama logička: **i**, **ili** i **ne**. U sljedećem primjeru spaja dva prethodnim primjerima.

    mToDoTable.where().year("due").eq(2013).and().startsWith("text", "PRI0")
                .execute().get();

Grupiranje i ugnijezditi logički operatori:

    mToDoTable.where()
                .year("due").eq(2013)
                    .and
                (startsWith("text", "PRI0").or().field("duration").gt(10))
                .execute().get();

Detaljnije raspravu i primjeri za filtriranje, potražite u članku [Upoznavanje izobilje Android klijentski model upita](http://hashtagfail.com/post/46493261719/mobile-services-android-querying).

### <a name="sorting"></a>Kako: sortiranja vraćenih podataka

Sljedeći kod vraća sve stavke iz tablice **ToDoItems** Sortirano uzlazno po *tekstno* polje. *mToDoTable* je referenca pozadinskog tablicu koju ste prethodno stvorili:

    mToDoTable.orderBy("text", QueryOrder.Ascending).execute().get();

Prvi parametar **orderBy** način je niz jednako naziv polja po kojem želite sortirati. Drugi parametar koristi Enumeracije **QueryOrder** da odredite želite li da biste sortirali uzlazno ili silazno.  Ako filtrirate metodom ***gdje*** , metodu ***gdje*** se mora pozvati prije metodu ***orderBy*** .

### <a name="paging"></a>Kako: vratiti podatke na stranicama

Prvi primjer prikazuje način da biste odabrali gornju pet stavki iz tablice. Upit vraća stavke iz tablice **ToDoItems**. **mToDoTable** je referenca pozadinskog tablicu koju ste prethodno stvorili:

    List<ToDoItem> result = mToDoTable.top(5).execute().get();


Evo upit koji dovodi do prvih pet stavke i zatim vraća sljedeći pet:

    mToDoTable.skip(5).top(5).execute().get();

### <a name="selecting"></a>Kako: odabrati određene stupce

Sljedeći kod prikazuje kako se vratiti sve stavke iz tablice **ToDoItems**, ali samo prikazuje polja **potpune** i **tekst** . **mToDoTable** je referenca na tablicu pozadinskog koju smo stvorili prethodno.

    List<ToDoItemNarrow> result = mToDoTable.select("complete", "text").execute().get();

Parametri za funkciju odaberite se niz nazivi stupaca u tablici koju želite vratiti.

**Odaberite** način mora slijedite metoda kao što su **mjesto** i **orderBy**. Možete biti prikazane na načine broja stranica kao što su **vrha**.

### <a name="chaining"></a>Kako: Concatenate metode upita

Možete se spajaju metode u upita pozadinskog tablice. Ulančavanje metode upita možete odabrati određene stupce filtrirane retke koji su sortirani i Straničeno. Možete stvarati složene logičke filtre.
Svaki način upit vraća objekt upita. Da biste završili niz metode i zapravo pokrenuli upit, nazovite metodu **izvršiti** . Ako, na primjer:

    mToDoTable.where()
        .year("due").eq(2013)
        .and().startsWith("text", "PRI0")
        .or().field("duration").gt(10)
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .top(20)
        .execute().get();

Metode ulančana upita mora naručiti se na sljedeći način:

1. Načini za filtriranje (**mjesto**).
2. Sortiranje metode (**orderBy**).
3. Metode za odabir (**Odaberite**).
4. Označavanje stranica metode (**Preskoči** i **gore**).

##<a name="inserting"></a>Kako: Umetanje podataka pozadinski

Stvaranje instance instanca klase *ToDoItem* i postavite svojstva.

    ToDoItem item = new ToDoItem();
    item.text = "Test Program";
    item.complete = false;

Da biste umetnuli objekt koristiti **insert()** :

    ToDoItem entity = mToDoTable.insert(item).get();

Podudaranja entitet vraćenog podaci umetnuta u tablici pozadinskog uključuju ID i druge vrijednosti postavljena na pozadinski.

Tablica mobilne aplikacije potreban je stupcem primarnog ključa s nazivom **ID-a**. Prema zadanim postavkama, u ovom stupcu je niz. Zadana vrijednost stupca ID je GUID.  Možete unijeti druge jedinstvene vrijednosti, kao što su adrese e-pošte ili korisnička imena. Kada je vrijednost niza ID-a nije naveden za umetnute zapis, pozadinski stvara novi GUID.

Niz ID vrijednosti navedite sljedeće prednosti:

+ ID-a mogu generirati bez trajanjem putovanja u bazu podataka.
+ Zapisi koji su lakše spojiti iz različitih tablica ili baze podataka.
+ ID vrijednosti bolje integrirati s logike aplikacije.

Niz ID vrijednosti su **potrebna** podrška za izvanmrežnu sinkronizaciju.

##<a name="updating"></a>Kako: ažuriranje podataka u mobilnoj aplikaciji

Da biste ažurirali podatke u tablici, način **update()** prenesite novi objekt.

    mToDoTable.update(item).get();

U ovom primjeru *stavka* je reference na redak u tablici *ToDoItem* , koji su se pojavili neke promjene.
Redak s istom **id** će se ažurirati.

##<a name="deleting"></a>Kako: brisanje podataka u mobilnoj aplikaciji

Sljedeći kod prikazuje kako izbrisati podatke iz tablice navođenjem objekt s podacima.

    mToDoTable.delete(item);

Možete i izbrisati stavku navođenjem polje **id** retka koji želite izbrisati.

    String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
    mToDoTable.delete(myRowId);

##<a name="lookup"></a>Kako: traženje određene stavke

Traženje stavke s poljem određene **id** metodom **lookUp()** :

    ToDoItem result = mToDoTable
                        .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
                        .get();

##<a name="untyped"></a>Kako: rad s podacima untyped

Untyped model programiranja daje točan kontrolu nad serijalizacije JSON.  Postoje neki uobičajeni scenariji kojima možda želite koristiti untyped model programiranja. Na primjer, ako pozadinskog tablica sadrži više stupaca, a potrebno je samo referentni podskup stupce.  Upisani modela potrebno je definirati stupaca sve mobilne aplikacije tablice u svojoj učionici podataka.  

Većina API poziva za pristup podacima je slična upisani programiranja pozive. Glavna razlika je da u modelu untyped pozovete metode na objekt **MobileServiceJsonTable** , umjesto **MobileServiceTable** objekt.

### <a name="json_instance"></a>Kako: Stvaranje instance komponente untyped tablice

Slične upisani model pokrenete početak referenci tablice, ali u tom slučaju je **MobileServicesJsonTable** objekt. Nabavite referencu tako da nazovete na **getTable** način na instance komponente klijent:

    private MobileServiceJsonTable mJsonToDoTable;
    //...
    mJsonToDoTable = mClient.getTable("ToDoItem");

Nakon stvaranja instance komponente **MobileServiceJsonTable**gotovo ima isti API dostupni kao upisani model programiranja. U nekim slučajevima metode potrajati untyped parametar umjesto upisani parametra.

### <a name="json_insert"></a>Kako: Umetanje untyped tablice

Sljedeći kod prikazuje kako umetnut. Prvi je korak da biste stvorili [JsonObject][1], koji je dio [gson] [ 3] biblioteke.

    JsonObject jsonItem = new JsonObject();
    jsonItem.addProperty("text", "Wake up");
    jsonItem.addProperty("complete", false);

Nakon toga pomoću **insert()** umetnite untyped objekata u tablicu.

    mJsonToDoTable.insert(jsonItem).get();

Ako vam je potrebna da biste dobili ID umetnuti objekt, upotrijebite metodu **getAsJsonPrimitive()** .

    jsonItem.getAsJsonPrimitive("id").getAsInt());

### <a name="json_delete"></a>Kako: izbrisati iz untyped tablice

Sljedeći kod prikazuje kako izbrisati instancu, u ovom slučaju istoj instanci **JsonObject** koja je stvorena u primjeru prethodnih *Umetanje* . Kod jednak s upisani slova, ali metoda ima različitih potpisa jer se poziva **JsonObject**.

         mToDoTable.delete(jsonItem);

Možete i izbrisati instancu pomoću njegov ID:

         mToDoTable.delete(ID);

### <a name="json_get"></a>Kako: vraća sve retke iz untyped tablice

Sljedeći kod prikazuje kako dohvatiti cijelu tablicu. Budući da koristite JSON tablice, samo dio stupaca u tablici možete selektivno dohvatiti.

    public void showAllUntyped(View view) {
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final JsonElement result = mJsonToDoTable.execute().get();
                    final JsonArray results = result.getAsJsonArray();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (JsonElement item : results) {
                                String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                                String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                                Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                                ToDoItem mToDoItem = new ToDoItem();
                                mToDoItem.setId(ID);
                                mToDoItem.setText(mText);
                                mToDoItem.setComplete(mComplete);
                                mAdapter.add(mToDoItem);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
    }

Isti skup filtriranja, filtriranje i označavanje stranica metode koje su dostupne za upisani model dostupni su za untyped model kao i.

##<a name="custom-api"></a>Kako: poziva prilagođene API-JA

Prilagođeni API omogućuje vam da biste odredili prilagođene krajnje točke koji izlažu funkcionalnosti poslužitelja koji ne mapiranje umetnut, ažuriranje, brisanje i pročitajte operacija. Pomoću prilagođenih API-JA možete imati više kontrole nad poruke, uključujući čitanja i postavljanje HTTP zaglavlja poruka i definiranje obliku tijela poruke osim JSON.

Android klijenta, nazovite **invokeApi** način da biste nazvali krajnju točku prilagođene API. Sljedeći primjer prikazuje način da biste nazvali krajnje API pod nazivom **completeAll**, koja vraća zbirke klase pod nazivom **MarkAllResult**.

    public void completeItem(View view) {

        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll", MarkAllResult.class );

            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }

                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();
                }
            });
        }

Način **invokeApi** naziva na klijentu šalje zahtjev za objavu nove prilagođene API-JA. Rezultat koji je vratio prilagođene API prikazuje se u dijaloškom okviru poruke, kao što su sve pogreške. Druge verzije programa **invokeApi** omogućuju neobavezno slanje objekta u tijelu zahtjeva, navedite način HTTP i slanje parametara upita sa zahtjevom za. Untyped verzijama **invokeApi** prikazuje se kao i.

##<a name="authentication"></a>Kako: dodavanje provjere autentičnosti za aplikaciju

Vodiči za detaljno već opisuju kako dodati te značajke.

Aplikacije servisa za podržava [provjere autentičnosti korisnika aplikacije](app-service-mobile-android-get-started-users.md) pomoću raznih vanjskih identiteta: Facebook, Google, Microsoftov Account, Twitter i Azure Active Directory. Postavljanje dozvola za tablice ograničavanja pristupa za određene operacije samo korisnicima čija je autentičnost provjerena. Identitet korisnika čija je autentičnost provjerena možete koristiti i implementaciju pravila za provjeru autentičnosti u vašem pozadinskog.

Podržane su dvije tokova provjere autentičnosti: na **poslužitelju** i **klijent** tijek. Tijek server nudi najjednostavniji sučelje za provjeru autentičnosti kao ovisi web-sučelja davatelji identiteta.  Bez dodatnih SDK-ovi su potrebna za primjenu provjera autentičnosti poslužitelja tijek. Provjera autentičnosti poslužitelja tijek ne nudi precizno integraciju u mobilnom uređaju i preporučuje samo dokaz pojam slučajevi.

Tijek klijent omogućuje dublju Integracija s mogućnosti specifične za uređaj, kao što su jedinstvenu prijavu kao ovisi SDK-ovi koje ste dobili od davatelja identiteta.  Na primjer, možete integrirati Facebook SDK u mobilnu aplikaciju.  Mobilnu verziju klijentskog swaps u aplikaciju servisa Facebook i potvrditi vaše prijave prije zamjena natrag u mobilnoj aplikaciji.

Za omogućivanje provjere autentičnosti u svojoj aplikaciji potrebno četiri koraka:

- Registracija aplikacije za provjeru autentičnosti s davateljem identiteta.
- Konfiguriranje pozadinskog sustava aplikacije servisa.
- Tablica Zhe korisnicima čija je autentičnost provjerena samo na pozadinskog aplikacije servisa.
- Dodavanje koda za provjeru autentičnosti aplikacije.

Postavljanje dozvola za tablice ograničavanja pristupa za određene operacije samo korisnicima čija je autentičnost provjerena. Da biste izmijenili zahtjeva možete koristiti i SID korisnika čija je autentičnost provjerena.  Dodatne informacije pročitajte članak [Početak rada s provjerom autentičnosti] i u dokumentaciji o poslužitelju SDK NEMODERIRANA.

### <a name="caching"></a>Kako: dodavanje koda za provjeru autentičnosti za aplikaciju

Sljedeći kod pokretanja procesa poslužitelja tijek prijavu pomoću davatelja usluga Google:

    MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google);

Nabavite ID prijavljenih korisnika iz **MobileServiceUser** metodom **getUserId** . Primjeri kako koristiti Futures da biste nazvali asinkronog prijava API-ji, potražite u članku [Početak rada s provjerom autentičnosti].

### <a name="caching"></a>Kako: tokeni za provjeru autentičnosti predmemorije

Tokeni za provjeru autentičnosti predmemorije potreban za lokalnu pohranu korisnički ID i token za provjeru autentičnosti, na uređaju. Kada se sljedeći put pokrene aplikaciju, provjerite predmemorije, a ako postoje te vrijednosti, možete preskočiti prijava postupak i rehydrate klijenta s tim podacima. No je osjetljive podatke, a on će biti pohranjene šifrirana za sigurnost u slučaju da prima krađe telefona.

Možete vidjeti cijeli primjera kako na tokeni za provjeru autentičnosti predmemorije u [odjeljku tokeni za provjeru autentičnosti predmemorije][7].

Kada pokušate koristiti istekle token dobijete odgovor *401 neovlašteno* . Možete učiniti pomoću filtara pogrešaka za provjeru autentičnosti.  Filtri intercept zahtjevi za pozadinski aplikacije servisa. Kod filtra testiraju odgovor za na 401, pokreće postupak prijave i primijenila zahtjeva koji je generirao na 401. 

## <a name="adal"></a>Kako: provjere autentičnosti korisnika završavaju na provjera autentičnosti biblioteke imenika Active Directory

U Active Directory provjera autentičnosti biblioteke (ADAL) možete koristiti da biste se prijavili korisnicima u aplikaciji pomoću servisa Azure Active Directory. Korištenje prijave tijek klijent je često preporučuje korištenje na `loginAsync()` načine, kao što je pruža više nativni UX dojam i omogućuje dodatne prilagodbe.

1. Konfiguriranje sustava pozadinski mobilne aplikacije za prijavu AAD slijedeći [upute za konfiguriranje aplikacije servisa za prijavu na servisu Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md) vodič. Provjerite jeste li dovršili neobavezan korak registriranja aplikacija za nativni klijent.

2. Instalirajte ADAL izmjenom build.gradle datoteku da biste uključili sljedeće definicije:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

3. Dodavanje koda za sljedeće aplikacije, napravite sljedeće zamjena:

* **Umetanje IZDAVANJE ovdje** zamijenite nazivom klijenta dodjeli aplikacije. Oblik mora biti https://login.windows.net/contoso.onmicrosoft.com. Ta vrijednost mogu kopirati na kartici domena u vašem Azure Active Directory [Azure klasični portalu].

* **Umetanje-RESURSA – ID-ovdje** zamijenite ID klijenta za vaše pozadinskog mobilne aplikacije. ID klijenta možete dobiti na kartici **Dodatno** u odjeljku **Azure Active Directory postavke** na portalu.

* **Umetanje-KLIJENT-ID-ovdje** zamijenite ID klijenta koji ste kopirali iz aplikacije za nativni klijent.

* **Umetanje-PREUSMJERAVANJE-URI-ovdje** zamijenite krajnje _/.auth/login/done_ web-mjesta, pomoću HTTPS shemu. Ta vrijednost mora biti sličan _https://contoso.azurewebsites.net/.auth/login/done_.

        private AuthenticationContext mContext;

        private void authenticate() {
            String authority = "INSERT-AUTHORITY-HERE";
            String resourceId = "INSERT-RESOURCE-ID-HERE";
            String clientId = "INSERT-CLIENT-ID-HERE";
            String redirectUri = "INSERT-REDIRECT-URI-HERE";
            try {
                mContext = new AuthenticationContext(this, authority, true);
                mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
            } catch (Exception exc) {
                exc.printStackTrace();
            }
        }

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    Log.d(TAG, "Cancelled");
                } else {
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    Log.d(TAG, "Token is empty");
                } else {
                    try {
                        JSONObject payload = new JSONObject();
                        payload.put("access_token", result.getAccessToken());
                        ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                            @Override
                            public void onFailure(Throwable exc) {
                                exc.printStackTrace();
                            }
                            @Override
                            public void onSuccess(MobileServiceUser user) {
                                Log.d(TAG, "Login Complete");
                            }
                        });
                    }
                    catch (Exception exc){
                        Log.d(TAG, "Authentication error:" + exc.getMessage());
                    }
                }
            }
        };

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
            }
        }

## <a name="how-to-add-push-notification-to-your-app"></a>Kako: dodavanje automatske obavijesti u aplikaciju

Možete [pročitajte pregled] [ 6] koji opisuje kako Microsoft Azure obavijesti koncentratora podržava razna automatske obavijesti.  U [ovom ćete praktičnom vodiču][5], automatske obavijesti šalju se u svim uređajima svaki put kada se umetne zapis.

## <a name="how-to-add-offline-sync-to-your-app"></a>Kako: dodavanje izvanmrežnu sinkronizaciju aplikacije

Praktični vodič za brzi početak rada sadrži kod koji implementira izvanmrežnu sinkronizaciju. Potražite kod mjestu s komentarima:

    // Offline Sync

Po uncommenting sljedeće retke koda možete implementirati izvanmrežnu sinkronizaciju i slično kao kod možete dodati druge aplikacije Mobile kod.

##<a name="customizing"></a>Kako: Prilagodba klijenta

Koje možete prilagoditi zadano ponašanje klijenta na nekoliko načina.

### <a name="headers"></a>Kako: Prilagodba zahtjev za zaglavlja

Konfiguriranje **ServiceFilter** da biste dodali prilagođeno zaglavlje HTTP svaki zahtjev:

    private class CustomHeaderFilter implements ServiceFilter {

        @Override
        public ListenableFuture<ServiceFilterResponse> handleRequest(
                    ServiceFilterRequest request,
                    NextServiceFilterCallback next) {

            runOnUiThread(new Runnable() {

                @Override
                public void run() {
                    request.addHeader("My-Header", "Value");                    }
            });

            SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
            try {
                ServiceFilterResponse response = next.onNext(request).get();
                result.set(response);
            } catch (Exception exc) {
                result.setException(exc);
            }
        }

### <a name="serialization"></a>Kako: Prilagodba serijalizacije

Klijent pretpostavlja da se nazivi tablica, nazivi stupaca i podataka vrste na pozadinskog sve odgovara točno podaci objekata definiranih u klijentu. Može biti bilo koji broj od razloga zašto možda ne odgovaraju nazive poslužitelja i klijenta. U slučaju, možda ćete morati učinite sljedeće vrste prilagodbe:

- Nazivi stupaca koji se koristi u tablici aplikacije servisa ne odgovaraju nazivima u klijentu koji koristite.
- Korištenje programa aplikacije servisa za tablicu koja sadrži neki drugi naziv od predmete je karte u klijentu.
- Uključite automatsko svojstvo velika i mala slova.
- Dodati složene svojstva objekta.

### <a name="columns"></a>Kako: mapiranje različite nazive postupak klijentske i poslužiteljske

Pretpostavimo da klijent kodu programskog jezika Java nazivima standardne Java stil za svojstva objekta **ToDoItem** , kao što je sljedeća svojstva:

- mId
- mText
- mComplete
- mDuration

Serijalizirati imena klijenata u JSON naziva koji se podudaraju nazive stupaca tablice **ToDoItem** na poslužitelju. Sljedeći kod koristi [gson] [ 3] biblioteke za obilježavanje svojstva:

    @com.google.gson.annotations.SerializedName("text")
    private String mText;

    @com.google.gson.annotations.SerializedName("id")
    private int mId;

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;

    @com.google.gson.annotations.SerializedName("duration")
    private String mDuration;

### <a name="table"></a>Kako: mapiranje nazive različitih tablica između klijenta i pozadinski

Mapiranje naziv tablice klijent naziv tablice različitih mobilnih usluga pomoću programa nadjačavanje [getTable()] [ 4] metoda:

    mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

### <a name="conversions"></a>Kako: automatizirati mapiranjima naziva stupaca

Možete navesti strategije pretvorbe koji se primjenjuje na svaki stupac pomoću [gson] [ 3] API-JA. Biblioteka Android klijent koristi [gson] [ 3] u pozadini serijalizirati Java objekte s podacima JSON prije slanja podataka aplikacije servisa za Azure.  Sljedeći kod koristi metodu **setFieldNamingStrategy()** da biste postavili na strategije. U ovom se primjeru će izbrisati znak inicijala (do "m"), a zatim malih sljedeći znak za svaki naziv polja. Na primjer, ga želite pretvoriti "mId" u "id."

    client.setGsonBuilder(
        MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(new FieldNamingStrategy() {
            public String translateName(Field field) {
                String name = field.getName();
                return Character.toLowerCase(name.charAt(1))
                    + name.substring(2);
                }
            });

Prije korištenja **MobileServiceClient**mora izvršiti kod.

### <a name="complex"></a>Kako: pohrana je svojstva objekta ili polja u tablicu

Dosad našeg primjera serijalizacije ste uključen jednostavne vrste kao što su decimale i nizova.  Jednostavne vrste jednostavno serijalizirati u JSON.  Ako želimo dodati složene objekt koji se automatski ne serijalizirati JSON, ne možemo potrebne metodu serijalizacije JSON.  Da biste vidjeli primjera kako unijeti prilagođeni serijalizacije JSON, pregledajte bloga [Prilagodba serijalizacije pomoću biblioteke gson u klijent za mobilne usluge Android][2].

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[The API structure]: #api
[How to: Query data from a mobile service]: #querying
[Vraćanje svih stavki]: #showAll
[Filtriranje vraćenih podataka]: #filtering
[Sortiranje vraćenih podataka]: #sorting
[Vraćanje podataka na stranicama]: #paging
[Odabrati određene stupce]: #selecting
[How to: Concatenate query methods]: #chaining
[How to: Bind data to the user interface]: #binding
[How to: Define the layout]: #layout
[How to: Define the adapter]: #adapter
[How to: Use the adapter]: #use-adapter
[How to: Insert data into a mobile service]: #inserting
[How to: update data in a mobile service]: #updating
[How to: Delete data in a mobile service]: #deleting
[How to: Look up a specific item]: #lookup
[How to: Work with untyped data]: #untyped
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching
[How to: Handle errors]: #errors
[How to: Design unit tests]: #tests
[How to: Customize the client]: #customizing
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next Steps]: #next-steps
[Setup and Prerequisites]: #setup

<!-- Images. -->

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Portal za Azure]: https://portal.azure.com
[Početak rada s provjerom autentičnosti]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[Budućnost]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html