<properties
   pageTitle="Konfiguriranje uloge za Azure Oblaku s Visual Studio | Microsoft Azure"
   description="Saznajte kako postaviti i konfigurirati ulogama za servise u oblaku Azure pomoću Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configure-the-roles-for-an-azure-cloud-service-with-visual-studio"></a>Konfiguriranje uloge za Azure Oblaku s Visual Studio

Azure oblaku može imati jedan ili više radnih ili uloge web. Za svaku ulogu morate definirati kako postaviti uloge i konfigurirati i načinom uloge. Da biste saznali više o ulogama u servise u oblaku, pogledajte videozapis [Uvod u Azure servise u Oblaku](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). Podaci za servis u oblaku pohranjeni su u sljedeće datoteke:

- **ServiceDefinition.csdef**

    Datoteka za definiciju servisa definira runtime postavke za servis u oblaku kao i one koje uloge potrebne, krajnje točke i veličina virtualnog računala. Nema podataka pohranjenih u ovoj datoteci mogu se promijeniti kada je pokrenut vaša uloga.

- **ServiceConfiguration.cscfg**

    Konfiguracijska datoteka servisa konfigurira koliko je instanci uloge koje će se izvoditi i vrijednosti postavke definira uloge. Podatke pohranjene u ovoj datoteci mogu se promijeniti dok se izvodi vaša uloga.

Da biste mogli spremiti različite vrijednosti te postavke za načinom vaša uloga, imate više konfiguracija servisa. Konfiguracija različite servisa možete koristiti za svaki okruženje za implementaciju. Na primjer, možete postaviti nizu za povezivanje računa za pohranu za korištenje emulator lokalno spremište Azure u konfiguraciji s lokalnim servisom stvorite drugi konfiguracija servisa za korištenje Azure prostora za pohranu u oblaku.

Prilikom stvaranja novog servisa Azure oblak u Visual Studio, dva konfiguracija servisa stvaraju se prema zadanim postavkama. Ove konfiguracije dodaju se u Azure projekt. Konfiguracija imenovane su:

- ServiceConfiguration.Cloud.cscfg

- ServiceConfiguration.Local.cscfg

## <a name="configure-an-azure-cloud-service"></a>Konfiguriranje servisa za Azure oblaka

Možete konfigurirati Azure oblaku iz programa Explorer rješenja u Visual Studio, kao što je prikazano na sljedećoj slici.

![Konfiguriranje servisa za oblaka](./media/vs-azure-tools-configure-roles-for-cloud-service/IC713462.png)

### <a name="to-configure-an-azure-cloud-service"></a>Da biste konfigurirali Azure oblaku

1. Da biste konfigurirali ulogama u Azure projekta iz **Programa Explorer rješenja**, otvorite izbornički prečac za ulogu Azure projektu, a zatim odaberite **Svojstva**.

    U uređivaču za Visual Studio prikazat će se stranica s nazivom uloge. Na stranici prikazuju se polja za karticu **Konfiguracija** .

1. Na popisu **Konfiguracija servisa** odaberite naziv konfiguracije servis koji želite urediti.

    Ako želite da biste promijenili cjelokupan konfiguracija servisa za ovu ulogu, možete odabrati **Sve konfiguracije**.

    >[AZURE.IMPORTANT] Ako odaberete konfiguracija određenog servisa, neka svojstva onemogućene su jer se može se postaviti samo za sve konfiguracije. Da biste uredili svojstva, morate odabrati sve konfiguracije.

    Sada možete odabrati na kartici da biste ažurirali sve omogućeno svojstva na taj prikaz.

## <a name="change-the-number-of-role-instances"></a>Promjena broja instanci uloga

Da biste poboljšali performanse servis u oblaku, možete promijeniti broj instanci uloge koje koristite, ovisno o broju korisnika i učitavanje očekivani za odgovarajućom ulogom. Zasebnom virtualnog računala stvara se za svaku instancu uloge prilikom pokretanja servisa u oblaku u Azure. To će utjecati na naplatu implementacije ovaj servis u oblaku. Dodatne informacije o naplati potražite u članku [objašnjenje računa za Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-number-of-instances-for-a-role"></a>Da biste promijenili broj instanci za uloge

1. Odaberite karticu **Konfiguracija** .

1. Na popisu **Konfiguracija servisa** odaberite konfiguracija servisa koji želite ažurirati.

    >[AZURE.NOTE] Možete postaviti da se broj instanci konfiguracija određene servisa ili za sve konfiguracije servisa.

1. U tekstni okvir **Instance broj** unesite broj instanci koje želite započeti ta uloga.

    >[AZURE.NOTE] Svaku instancu se izvodi na zasebnom virtualnog računala prilikom objavljivanja servis u oblaku Azure.

1. Odaberite gumb **Spremi** na alatnoj traci da biste spremili promjene na konfiguracijska datoteka servisa.

## <a name="manage-connection-strings-for-storage-accounts"></a>Upravljanje niza veze za račune za pohranu

Dodavanje, uklanjanje ili izmjena nizove veze za vaš servis konfiguracije. Možda ćete nizovi drugu vezu za različite servisne konfiguracije. Na primjer, možda ćete niza za povezivanje s lokalnom za konfiguraciju lokalnim servisom koja ima vrijednost `UseDevelopmentStorage=true`. Možda želite konfigurirati konfiguracija servisa oblak koji koristi račun za pohranu u Azure.

>[AZURE.WARNING] Kada unesete Azure prostora za pohranu ključa informacija o računu za niza za povezivanje računa za pohranu, ti podaci se spremaju lokalno u datoteci konfiguracije servisa. Međutim, ti podaci nije pohranjena kao šifrirane tekst.

Pomoću neku drugu vrijednost za svaku konfiguracija servisa ne morate koristiti drugu vezu nizove u servis u oblaku i izmjena kod prilikom objavljivanja servis u oblaku Azure. Možete koristiti isti naziv za niz za povezivanje u kodu i vrijednost će se razlikovati ovisno o konfiguraciji servis koji ste odabrali prilikom stvaranja servis u oblaku ili ga objavite.

### <a name="to-manage-connection-strings-for-storage-accounts"></a>Da biste upravljali niza veze za račune za pohranu

1. Odaberite karticu **Postavke** .

1. Na popisu **Konfiguracija servisa** odaberite konfiguracija servisa koji želite ažurirati.

    >[AZURE.NOTE] Možete ažurirati niza veze za konfiguraciju određenog servisa, no ako trebate dodati ili izbrisati niza za povezivanje morate odabrati sve konfiguracije.

1. Da biste dodali niza za povezivanje, odaberite gumb **Dodaj postavku** . Nova stavka se dodaje na popis.

1. U tekstni okvir **naziv** upišite naziv koji želite koristiti za niz za povezivanje.

1. Na padajućem popisu **Vrsta** odaberite **Niz za povezivanje**.

1. Da biste promijenili vrijednost niza za povezivanje, odaberite gumb tri točke (...). Pojavit će se dijaloški okvir **Stvaranje niza za povezivanje za pohranu** .

1. Da biste koristili račun emulator lokalno spremište, odaberite gumb mogućnosti **emulator Microsoft Azure prostora za pohranu** , a zatim gumb **u redu** .

1. Da biste koristili račun za pohranu Azure, odaberite gumb mogućnosti **pretplate** , a zatim odaberite željeni prostora za pohranu račun.

1. Da biste koristili prilagođeni vjerodajnice, odaberite gumb mogućnosti **ručno unijeli vjerodajnice** . Unesite naziv računa za pohranu i primarni ili drugi ključ. Informacije o stvorite račun za pohranu i unijeti detalje za pohranu račun u dijaloškom okviru **Stvaranje niza za povezivanje za pohranu** potražite u članku [Priprema za objavu ili uvođenje Azure aplikacije Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Da biste izbrisali niz za povezivanje, odaberite niz za povezivanje, a zatim gumb **Ukloni postavku** .

1. Odaberite ikonu **Spremi** na alatnoj traci da biste spremili promjene na konfiguracijska datoteka servisa.

1. Da biste pristupili niz za povezivanje u konfiguracijskoj datoteci usluga, morate dobiti vrijednost postavke konfiguracije. Sljedeći kod prikazuje primjer u kojem je stvorena blobova i podataka prenijeli pomoću niza za povezivanje `MyConnectionString` iz konfiguracijska datoteka servisa kada korisnik odabere **Button1** na stranici zadano.aspx u ulozi web za Azure oblaku. Dodajte sljedeće pomoću naredbe za Default.aspx.cs:

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Otvorite Default.aspx.cs u prikazu dizajna, a zatim dodajte gumb iz alatnog okvira. Dodati sljedeći kod da biste na `Button1_Click` način. Kod koristi `GetConfigurationSettingValue` da biste dobili vrijednost iz datoteke konfiguracija servisa za niz za povezivanje. Zatim blob stvara se u račun za pohranu koji su navedeni u nizu za povezivanje `MyConnectionString` i na kraju program dodaje tekst blob-om.

    ```
    protected void Button1_Click(object sender, EventArgs e)
    {
        // Setup the connection to Azure Storage
        var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("MyConnectionString"));
        var blobClient = storageAccount.CreateCloudBlobClient();
        // Get and create the container
        var blobContainer = blobClient.GetContainerReference("quicklap");
        blobContainer.CreateIfNotExists();
        // upload a text blob
        var blob = blobContainer.GetBlockBlobReference(Guid.NewGuid().ToString());
        blob.UploadText("Hello Azure");

    }
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Dodavanje prilagođene postavke za korištenje na servisu Azure oblaka

Prilagođene postavke u datoteci konfiguracije servisa omogućuju dodavanje naziv i vrijednost niza za konfiguraciju određene servisa. Možete koristiti tu postavku da biste konfigurirali značajku u servis u oblaku vrijednost postavke za čitanje i korištenjem tu vrijednost za kontrolu logike u kodu. Možete promijeniti te vrijednosti za konfiguraciju servisa bez potrebe za ponovno stvaranje paketa za servis ili kada je pokrenut servis u oblaku. Kod možete potražiti obavijesti prilikom promjene postavki. Pogledajte [RoleEnvironment.Changing događaj](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Dodavanje, uklanjanje ili izmjena prilagođenih postavki za vaš servis konfiguracije. Možda ćete različite vrijednosti te nizova za različite servisne konfiguracije.

Koristite neku drugu vrijednost za svaku konfiguracija servisa, ne morate koristiti različite nizove u servis u oblaku i izmjena kod prilikom objavljivanja servis u oblaku Azure. Možete koristiti isti naziv niza u kodu i vrijednost će se razlikovati ovisno o konfiguraciji servis koji ste odabrali prilikom stvaranja servis u oblaku ili ga objavite.

### <a name="to-add-custom-settings-to-use-in-your-azure-cloud-service"></a>Da biste dodali prilagođene postavke za korištenje na servisu Azure oblaka

1. Odaberite karticu **Postavke** .

1. Na popisu **Konfiguracija servisa** odaberite konfiguracija servisa koji želite ažurirati.

    >[AZURE.NOTE] Možete ažurirati nizova za konfiguraciju određenog servisa, no ako trebate dodati ili izbrisati niza, morate odabrati **Sve konfiguracije**.

1. Da biste dodali niza, odaberite gumb **Dodaj postavku** . Nova stavka se dodaje na popis.

1. U tekstni okvir **naziv** upišite naziv koji želite koristiti za niz.

1. Na padajućem popisu **Vrsta** odaberite **niz**.

1. Da biste dodali ili promijenili vrijednost niza, u tekstni okvir **vrijednost** upišite novu vrijednost.

1. Da biste izbrisali niz, odaberite niz, a zatim gumb **Ukloni postavku** .

1. Odaberite gumb **Spremi** na alatnoj traci da biste spremili promjene na konfiguracijska datoteka servisa.

1. Da biste pristupili niza u konfiguracijskoj datoteci usluga, morate dobiti vrijednost postavke konfiguracije.

    Morate provjeriti jesu li sljedeće pomoću naredbe već dodaju se Default.aspx.cs kao iz prethodnog postupka.

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Dodati sljedeći kod da biste na `Button1_Click` način da biste pristupili ovog niza na isti način možete pristupiti niza za povezivanje. Kod zatim možete izvršiti neke određene kod na temelju vrijednosti niza postavke servisa konfiguracijske datoteke koja se koristi.

    ```
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("MySetting");
    if (settingValue == “ThisValue”)
    {
    // Perform these lines of code
    }
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Upravljanje lokalno spremište za svaku instancu uloga

Možete dodati lokalni sustav pohrani za svaku instancu uloge. Pohranite lokalne podatke koje morate pristupiti druge uloge. Ovdje je moguće pohraniti sve podatke koje su vam potrebne da biste spremili u tablicu, blob ili SQL baze podataka za pohranu. Ako, na primjer, možete koristiti ovaj lokalno spremište podatke u predmemoriju koje možda ćete se morati ponovno koristiti. U ovom pohranjene podatke nije moguće pristupiti druge instance uloge. 

Lokalno spremište postavke primjenjuju se na sve usluge konfiguracije. Možete samo dodavanje, uklanjanje ili izmjena lokalno spremište za sve konfiguracije servisa.

### <a name="to-manage-local-storage-for-each-role-instance"></a>Da biste upravljali lokalno spremište za svaku instancu uloga

1. Odaberite karticu **Lokalno spremište** .

1. Na popisu **Konfiguracija servisa** , odaberite **Sve konfiguracije**.

1. Da biste dodali unos lokalno spremište, odaberite gumb **Dodaj lokalno spremište** . Nova stavka se dodaje na popis.

1. U tekstni okvir **naziv** upišite naziv koji želite koristiti za ovaj lokalno spremište.

1. U tekstni okvir **Veličina** unesite veličinu u MB koje su vam potrebne za ovu lokalno spremište.

1. Da biste uklonili podatke u ovom lokalno spremište kada je virtualnog računala za ta uloga brisanja, potvrdite okvir **koša za čišćenje na ulozi** .

1. Da biste uredili postojeću lokalno spremište stavku, odaberite redak koji je potrebno ažurirati. Zatim možete urediti polja, kao što je opisano u prethodnim koracima.

1. Da biste izbrisali unos u lokalno spremište, odaberite stavku prostora za pohranu na popisu, a zatim gumb **Ukloni lokalno spremište** .

1. Da biste spremili promjene konfiguracije datoteke servisa, odaberite ikonu **Spremi** na alatnoj traci.

1. Da biste pristupili lokalno spremište koje ste dodali u konfiguracijskoj datoteci usluga, morate dobiti vrijednost postavke za konfiguriranje lokalnog resursa. Pomoću sljedeće retke koda pristup tu vrijednost i stvoriti datoteku pod nazivom **MyStorageTest.txt** i napišite redak test podataka u tu datoteku. Možete dodati kod u na `Button_Click` metode koju ste koristili u prethodno postupci:

1. Morate provjeriti jesu li sljedeće pomoću naredbe dodaju se Default.aspx.cs:

    ```
    using System.IO;
    using System.Text;
    ```

1. Dodati sljedeći kod da biste na `Button1_Click` način. Stvara datoteku u lokalno spremište i zapisuje test podatke u tu datoteku.

    ```
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("LocalStorage1");

    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyStorageTest.txt" };
    String filePath = Path.Combine(paths);

    using (FileStream writeStream = File.Create(filePath))
    {
          Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
          writeStream.Write(textToWrite, 0, textToWrite.Length);
    }
    ```

1. (Neobavezno) Da biste vidjeli ove datoteke koju ste stvorili kada pokrenete servis u oblaku lokalno, poduzmite sljedeće korake:

  1. Pokrenite ulogu web i odaberite **Button1** li kod unutar `Button1_Click` dobiva naziva.

  1. U području obavijesti otvorite izbornik prečaca za Azure ikona pa odaberite **Pokaži izračunati Emulator korisničkog Sučelja**. Pojavit će se dijaloški okvir **Za izračun Emulator Azure** .

  1. Odaberite ulogu za web.

  1. Na traci izbornika odaberite **Alati** **Otvori lokalno spremište**. Pojavit će se prozor programa Windows Explorer.

  1. Na traci izbornika u tekstni okvir za **pretraživanje** unesite **MyStorageTest.txt** , a zatim odaberite **Enter** da biste pokrenuli pretraživanje.

    Datoteka prikazuje se u rezultatima pretraživanja.

  1. Da biste pogledali sadržaj datoteke, otvorite izbornik prečaca za datoteku i odaberite **Otvori**.

## <a name="collect-cloud-service-diagnostics"></a>Prikupljanje Dijagnostika servisa oblaka

Možete prikupiti podatke za dijagnostiku servisa Azure oblaka. Ove podatke dodaje se na račun za pohranu. Možda ćete nizovi drugu vezu za različite servisne konfiguracije. Ako, na primjer, možda ćete lokalno spremište računa za konfiguriranje lokalni servis koji ima vrijednost UseDevelopmentStorage = true. Možda želite konfigurirati konfiguracija servisa oblak koji koristi račun za pohranu u Azure. Dodatne informacije o Azure Dijagnostika potražite u članku prikupljanje podataka zapisivanje prema korištenjem dijagnostike Azure.

>[AZURE.NOTE] Konfiguracija lokalnog servisa već konfigurirana za korištenje Lokalni resursi. Ako koristite konfiguracija servisa oblaka za objavljivanje na servisu Azure oblaka, niz za povezivanje koji navedete Kada objavljujete koristi se za dijagnostiku niz za povezivanje osim ako ste naveli niza za povezivanje. Ako je paket sustava u oblaku pomoću Visual Studio, neće se promijeniti niz za povezivanje u konfiguraciju servisa.

### <a name="to-collect-cloud-service-diagnostics"></a>Da biste prikupili Dijagnostika servisa oblaka

1. Odaberite karticu **Konfiguracija** .

1. Na popisu **Konfiguracija servisa** odaberite konfiguracija servisa koji želite ažurirati ili odaberite **Sve konfiguracije**.

    >[AZURE.NOTE] Možete ažurirati račun za pohranu za konfiguraciju određenog servisa, ali ako želite omogućiti ili onemogućiti Dijagnostika morate odabrati sve konfiguracije.

1. Da biste omogućili dijagnostika, odaberite potvrdni okvir **Omogući Dijagnostika** .

1. Da biste promijenili vrijednost za račun za pohranu, odaberite gumb tri točke (...).

    Pojavit će se dijaloški okvir **Stvaranje niza za povezivanje za pohranu** .

1. Da biste koristili niza za povezivanje s lokalnom, odaberite mogućnost emulator Azure pohrane, a zatim gumb **u redu** .

1. Da biste koristili za pohranu račun povezan s pretplatom Azure, odaberite mogućnost **pretplate** .

1. Da biste koristili račun za pohranu niz lokalnu vezu, odaberite mogućnost **ručno unijeli vjerodajnice** .

    Dodatne informacije o stvorite račun za pohranu i unijeti detalje za pohranu račun u dijaloškom okviru **Stvaranje niza za povezivanje za pohranu** potražite u odjeljku [Priprema za objavu ili uvođenje Azure aplikacije Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Odaberite račun za pohranu koji želite koristiti u polje **naziv računa**.

    Ako ste ručno unesete vjerodajnice za račun za pohranu kopirajte ili unesite primarni ključ u **ključ za račun**. Ovaj ključ može kopirati iz [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885). Da biste kopirali ključ, slijedite korake u nastavku iz prikaza **Za pohranu računi** [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885):
    
  1. Odaberite račun za pohranu koji želite koristiti za servis u oblaku.

  1. Odaberite gumb **Upravljanje pristupnih tipki** koji se nalazi pri dnu zaslona. Pojavit će se dijaloški okvir **Upravljanje pristupnih tipki** .

  1. Da biste kopirali tipka za pristup, odaberite gumb **Kopiraj u međuspremnik** . Sada možete zalijepiti tu tipku u polje **ključ za račun** .

1. Da biste koristili račun za pohranu koje ste omogućili, kao niz za povezivanje za dijagnostiku (i predmemoriranja) prilikom objavljivanja servis u oblaku Azure, odaberite potvrdni okvir **Ažuriraj razvoj prostora za pohranu nizove veze za dijagnostiku i Međuspremanje s Azure prostora za pohranu vjerodajnica pri objavljivanju Azure računa** .

1. Odaberite gumb **Spremi** na alatnoj traci da biste spremili promjene na konfiguracijska datoteka servisa.

## <a name="change-the-size-of-the-virtual-machine-used-for-each-role"></a>Promjena veličine virtualnog računala koja se koristi za svaku ulogu

Postavite veličinu virtualnog računala za svaku ulogu. Možete postaviti samo ovu veličinu za sve konfiguracije servisa. Ako odaberete manji stroj, zatim manje jezgri procesora, memorije i na lokalnom disku za pohranu Dodijeljeno. Dodijeljeno propusnosti je manji. Dodatne informacije o te veličine i resursima dodijeliti potražite u članku [veličine za servise u Oblaku](cloud-services/cloud-services-sizes-specs.md).

Resursi potrebni za svaki virtualnog računala u Azure utječe na trošak pokrenut servis u oblaku u Azure. Dodatne informacije o naplati Azure potražite u članku [objašnjenje računa za Microsoft Azure](billing/billing-understand-your-bill.md).

### <a name="to-change-the-size-of-the-virtual-machine"></a>Da biste promijenili veličinu virtualnog računala

1. Odaberite karticu **Konfiguracija** .

1. Na popisu **Konfiguracija servisa** , odaberite **Sve konfiguracije**.

1. Da biste odabrali veličinu virtualnog računala za ta uloga, odaberite odgovarajuću veličinu s popisa **VM veličina** .

1. Odaberite gumb **Spremi** na alatnoj traci da biste spremili promjene na konfiguracijska datoteka servisa.

## <a name="manage-endpoints-and-certificates-for-your-roles"></a>Upravljanje krajnje točke i certifikati za svoje uloge

Konfiguriranje umrežavanje krajnje točke navođenjem protokol broj priključka i, za HTTPS, informacije o certifikatu SSL. Izdanja prije lipnja 2012 podržava HTTP, HTTPS i TCP. Izdanje lipnja 2012 podržava te protokoli i UDP. UDP ne možete koristiti za unos krajnje točke u emulator računalnim. Možete koristiti taj protokol samo za interne krajnje točke.

Da biste poboljšali sigurnost servisa Azure oblak, možete stvoriti krajnje točke koje koriste HTTPS protokola. Na primjer, ako imate neki servis u oblaku koja se koristi tako da klijenti za narudžbenice, želite provjerite je li svoje podatke o sigurne tako da putem SSL-a.

Možete dodati i krajnje točke koje se mogu koristiti interno ili kao vanjski. Vanjski krajnje točke nazivaju se unos krajnje točke. Krajnje točke unosa omogućuje druge pristupnoj točci korisnicima na servis u oblaku. Ako imate WCF servisa, možda ćete morati izložiti krajnje Interna web uloge da koristite za pristup taj servis.

>[AZURE.IMPORTANT] Možete ažurirati samo krajnje točke za sve konfiguracije servisa.

Ako dodate HTTPS krajnje točke, morate koristiti SSL certifikata. Da biste to učinili možete pridružiti certifikati vaša uloga za sve konfiguracije servisa i upotrijebite te za vaše krajnje točke.

>[AZURE.IMPORTANT] Ove potvrde su isporučen na servisu. Ne morate prenijeti certifikatima zasebno Azure putem [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

Sve upravljanje certifikate koji se pridružiti konfiguracija u servis primjenjuju samo kada je servis u oblaku pokreće se u Azure. Kada servis u oblaku pokreće se u lokalnom platforme, koristi se standardni certifikat koji upravlja emulator Azure računalnim.

### <a name="to-add-a-certificate-to-a-role"></a>Da biste dodali certifikat u ulogu

1. Odaberite karticu **potvrde** .

1. Na popisu **Konfiguracija servisa** , odaberite **Sve konfiguracije**.

    >[AZURE.NOTE] Da biste dodali ili uklonili certifikata, morate odabrati sve konfiguracije. Ako je potrebno možete ažurirati ime i otisak prsta za konfiguraciju određene servisa.

1. Da biste dodali certifikat za ta uloga, odaberite gumb **Dodaj certifikata** . Nova stavka se dodaje na popis.

1. U tekstni okvir **naziv** unesite naziv certifikata.

1. Na popisu **Mjesto spremišta** odaberite mjesto za certifikat koji želite dodati.

1. Na popisu **Imena pohranu** odaberite trgovinu koju želite koristiti da biste odabrali certifikat.

1. Da biste dodali certifikata, odaberite gumb tri točke (...). Pojavit će se dijaloški okvir **Sigurnost sustava Windows** .

1. Odaberite certifikat koji želite koristiti s popisa, a zatim odaberite gumb **u redu** .

    >[AZURE.NOTE] Kada dodate certifikat iz spremišta certifikata, sve Srednja certifikati automatski se dodaju postavki konfiguriranje umjesto vas. Da bi se ispravno konfiguriranje usluge za SSL, za Azure morate moguće prenijeti ove Srednja potvrde.

1. Da biste izbrisali certifikat, odaberite certifikat, a zatim gumb **Ukloni certifikata** .

1. Odaberite ikonu **Spremi** na alatnoj traci da biste spremili promjene na datoteke za konfiguraciju servisa.

### <a name="to-manage-endpoints-for-a-role"></a>Da biste upravljali krajnje točke za uloge

1. Odaberite karticu **krajnje točke** .

1. Na popisu **Konfiguracija servisa** , odaberite **Sve konfiguracije**.

1. Da biste dodali krajnje, odaberite gumb **Dodaj krajnjoj točki** . Nova stavka se dodaje na popis.

1. U tekstni okvir **naziv** upišite naziv koji želite koristiti za ovu krajnju točku.

1. Odabir vrste krajnje točke koje su vam potrebne s popisa **Vrsta** .

1. Na popisu **protokol** odaberite protokol za krajnje točke koje su vam potrebne.

1. Ako je krajnje unos u tekstni okvir **Javno priključak** unesite javno priključak koji želite koristiti.

1. U tekstnom okviru **Priključak privatno** upišite privatne priključak za korištenje.

1. Krajnja točka zahtijeva https protokola, na popisu **Naziv SSL certifikata** odaberite certifikat koji ćete koristiti.

    >[AZURE.NOTE] Popis prikazuje certifikata koji ste dodali za ta uloga na kartici **potvrde** .

1. Odaberite gumb **Spremi** na alatnoj traci da biste spremili promjene na datoteke za konfiguraciju servisa.

## <a name="next-steps"></a>Daljnji koraci
Saznajte više o Azure projekata u Visual Studio tako da pročitate [Konfiguriranje projekta programa Azure](vs-azure-tools-configuring-an-azure-project.md). Dodatne informacije o shemi servisa oblak tako da pročitate [Referenca sheme](https://msdn.microsoft.com/library/azure/dd179398).
