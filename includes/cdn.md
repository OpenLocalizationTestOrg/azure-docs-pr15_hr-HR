# <a name="using-cdn-for-azure"></a>Pomoću CDN za Azure

Azure sadržaj isporuke mreže (CDN) nudi razvojnim inženjerima globalni rješenje za sadržaj velikom propusnošću po predmemoriranje blob-ova i statički sadržaj računalnim instanci pri fizičke čvorove u Sjedinjenim Američkim Državama, Europa, Azija, Australija i Južna Amerika. Trenutni popis CDN čvor mjesta potražite u članku [Azure CDN čvor mjesta].

Ovaj zadatak obuhvaća sljedeće korake:

* [Korak 1: Stvaranje računa za pohranu](#Step1)
* [Korak 2: Stvaranje CDN krajnju točku za vaš račun za pohranu](#Step2)
* [Korak 3: Pristupanje CDN sadržaja](#Step3)
* [Korak 4: Uklanjanje vaša CDN sadržaja](#Step4)

Prednosti korištenja CDN u predmemoriju Azure podaci obuhvaćaju sljedeće:

-   Bolje performanse i korisničko sučelje za krajnje korisnike koji su far from izvora sadržaja i koristite aplikacije gdje mnogo 'internet trips"potrebne za učitavanje sadržaja
-   Velike raspodijeljeno skaliranje radi bolje rukovati trenutačno učitavanja visoka, recimo, na početku događaja kao što su proizvoda

Postojeće korisnike CDN sada možete koristiti Azure CDN [Azure klasični portal]. Na CDN je značajka dodatak za svoju pretplatu i ima zasebnu [tarifu za naplatu].

<a id="Step1"> </a>
<h2>Korak 1: Stvaranje računa za pohranu</h2>

Da biste stvorili novi račun za pohranu za pretplatu na Azure pomoću sljedećeg postupka. Račun za pohranu omogućuje pristup servise za pohranu Azure. Račun za pohranu predstavlja najvišu razinu naziva za pristup svim komponente servisa Azure pohrane: bloba, reda čekanja services, i tablice services. Dodatne informacije o servisima Azure prostora za pohranu potražite u članku [Korištenje Azure servise za pohranu](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Stvaranje računa za pohranu, mora biti administrator servisa ili zajednički administratora za povezane pretplatu.

> [AZURE.NOTE] Informacije o izvođenju ovog postupka pomoću upravljanja API-JA servisa Azure, potražite u članku referenca temi [Stvaranje računa za pohranu](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) .

**Stvaranje računa za pohranu za pretplatu na Azure**

1.  Prijavite se u [Azure klasični portal].
2.  U donjem lijevom kutu kliknite **Novo**. U dijaloškom okviru **Novo** odaberite **Data Services**, a zatim kliknite **prostora za pohranu**, **Brzo stvaranje**.

    Pojavit će se dijaloški okvir **Stvaranje računa za pohranu** .

    ![Stvaranje računa za pohranu][create-new-storage-account]

4. U polje **URL** upišite naziv poddomene. Stavka može sadržavati od 3 do 24 mala slova i brojeva.

    Ta vrijednost postaje naziv glavnog računala unutar URI koji se koristi za adresa Blob, reda čekanja ili tablicu resursi za pretplatu. Da biste riješili spremnik resursa u servisu Blob, koristite URI u sljedećem obliku, gdje * &lt;StorageAccountLabel&gt; * se odnosi na vrijednost koju ste upisali u **Unesite URL**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Važne:** Natpis URL poddomene račun za pohranu URI za obrasce i mora biti jedinstven među svim servisima smještene u Azure.

    Ta vrijednost služi i kao naziv tog računa za pohranu na portalu ili kada programatski pristup ovog računa.

5.  S padajućeg popisa **Regije/afinitet grupe** odaberite regija ili afinitet grupe za račun za pohranu. Odaberite grupu afinitet umjesto područja ako želite servise za pohranu u istu centar za podatke s drugih servisa Windows Azure koji koristite. To možete poboljšati performanse, a ne naknade nastaje za izlazne.  

    **Bilješke:** Da biste stvorili grupu afinitet, otvorite područje **Postavke** Portal za upravljanje, kliknite **afinitet grupe**i pa kliknite **Dodaj grupu afinitet** ili **Dodaj**. Stvaranje i upravljanje grupama afinitet pomoću API za upravljanje servisa Windows Azure. Dodatne informacije potražite u članku [postupci za grupe afinitet].

6. S padajućeg popisa **pretplate** odaberite pretplatu u koju se s koristit će se račun za pohranu.
7.  Kliknite **Stvori račun za pohranu**. Postupak stvaranja računa za pohranu može potrajati nekoliko minuta.
8.  Da biste provjerili uspješno stvorili račun za pohranu, provjerite prikazuje li se na račun u navedene za **pohranu** sa statusom **Online**stavke.

<a id="Step2"> </a>
<h2>Korak 2: Stvaranje CDN krajnju točku za vaš račun za pohranu</h2>

Kada omogućite CDN pristup računu za pohranu ili hostira servis, svi objekti javno dostupna su uvjete za predmemoriranje rub CDN. Ako mijenjate objekt koji se trenutno predmemorirani u na CDN, novi sadržaj neće biti dostupne putem na CDN dok se CDN osvježava njezin sadržaj kada istekne predmemorirani sadržaja vrijeme važenja razdoblja.

**Da biste stvorili krajnju CDN za vaš račun za pohranu**

1. [Azure klasični portal]u navigacijskom oknu kliknite **CDN**.

2. Na vrpci kliknite **Novo**. U dijaloškom okviru **Novo** odaberite **Aplikaciju servisa**, a zatim **CDN**, a zatim **Brzo stvaranje**.

3. Na padajućem popisu **Domenu izvor** odaberite račun za pohranu koji ste stvorili u prethodnom odjeljku na popisu računa dostupan prostor za pohranu. 

4. Kliknite gumb **Stvori** da biste stvorili novi krajnjoj točki.

5. Nakon stvaranja krajnju točku, prikazuje se popis krajnje točke za pretplatu. Prikaz popisa prikazuje se URL koristi za pristup predmemoriranog sadržaja, kao i domenu izvor. 

    Domena polazište je na mjesto s kojeg se CDN predmemorira sadržaj. Domenu izvor može biti prostora za pohranu račun ili servis u oblaku; račun za pohranu koristi se za potrebe ovog primjera. Edge poslužitelji sastavljanja predmemorijom postavke koje ste odredili ili na zadani heuristike predmemoriranja mreže predmemoriju je prostora za pohranu sadržaja. 


    > [AZURE.NOTE] Konfiguriranje stvorene za krajnju točku odmah nije dostupna; To može potrajati i do 60 minuta za registraciju za prijenos putem mreže CDN. Korisnici koji pokušate koristiti naziv domene CDN odmah primiti Šifra stanja 400 (neispravan zahtjev) dok ne bude dostupno putem na CDN sadržaj.

<a id="Step3"> </a>
<h2>Korak 3: Access CDN sadržaja</h2> 

Da biste pristupili predmemoriranog sadržaja na na CDN, koristite navedeni na portalu CDN URL. Adresa za predmemorirani blob bit će otprilike ovako:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>
<h2>Korak 4: Uklanjanje sadržaja u CDN</h2>

Ako više ne želite predmemorije objekta u na Azure sadržaja isporuku mrežom (CDN), možete učiniti nešto od sljedećeg postupka:

-   Za Azure blobova platforme, možete izbrisati blob-om s javnim spremnik.
-   Možete unijeti spremnik privatno umjesto Tenis. Dodatne informacije potražite u članku [Ograničiti pristup spremnika i blob-ova](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) .
-   Možete onemogućiti ili brisanje krajnje točke CDN pomoću portala za upravljanje.
-   Možete izmijeniti na glavnom računalu servisu više odgovora na zahtjeve za objekt.

Objekt već spremljen u na CDN će ostati predmemorirani do isteka razdoblja vrijeme važenja objekta. Kada istekne razdoblje vrijeme važenja na CDN će provjeriti je li krajnju točku CDN još uvijek valjan i i dalje anonimno pristupiti objekta. Ako nije, zatim objekt više spremit će se.

Mogućnost odmah Očisti sadržaj trenutno nije podržano na Portal za upravljanje Azure. Obratite se [Azure podržava](https://azure.microsoft.com/support/options/) ako je potrebno odmah Očisti sadržaj. 

## <a name="additional-resources"></a>Dodatni resursi

-   [Stvaranje grupe sustava afinitet u Azure]
-   [Kako: upravljanje računima za pohranu za pretplatu na Azure]
-   [O upravljanju servisa API-JA]
-   [Upute za mapiranje CDN sadržaj prilagođene domene]

  [Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
  [Azure CDN čvor mjesta]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
  [Azure klasični portal]: https://manage.windowsazure.com/
  [plan za naplatu]: /pricing/calculator/?scenario=full
  [Stvaranje grupe sustava afinitet u Azure]: http://msdn.microsoft.com/library/azure/ee460798.aspx
  [Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
  [O Upravljanje servisom API-JA]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
  [Upute za mapiranje CDN sadržaj prilagođene domene]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
