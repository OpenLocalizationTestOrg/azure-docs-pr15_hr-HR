<properties 
    pageTitle="Azure pretraživanja za razvojne inženjere studiju slučaja: Kako WhatToPedia portal za infomedia utemeljena na Microsoft Azure | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka" 
    description="Saznajte kako izraditi programa informacije portal i pronalaženja tražilica pomoću pretraživanja Azure, servis za pretraživanje oblaka hostira za razvojne inženjere." 
    services="search, sql-database,  storage, web-sites" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard"/>

<tags 
    ms.service="search" 
    ms.devlang="NA" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="search" 
    ms.date="08/29/2016" 
    ms.author="heidist"/>

# <a name="azure-search-developer-case-study"></a>Azure pretraživanja za razvojne inženjere studiju slučaja

## <a name="how-whattopediacomhttpwhattopediacom-built-an-infomedia-portal-on-microsoft-azure"></a>Kako [WhatToPedia.com](http://whattopedia.com/) komponenti portal za infomedia na Microsoft Azure

 ![][6]  &nbsp;&nbsp;&nbsp;  <font size="9">Dobra je ideja</font> 


Naš je da biste sastavili portal za informacije koji olakšava shoppers povezati s trgovaca na malo kroz-najrelevantnije, iz djelokruga pretraživanja sučelje sličan način putujete tourists portalima podudaranje prema gore s Hoteli, restorani i zabavu u uncharted područje. 

Portal smo envision ćete održati iznimno visoke kvalitete pretraživanju iznad prodavača podataka na određenom tržištu, pomoć shoppers pronaći služi za pohranu na temelju lokacije i druge amenities se prodavaču nudi. Ne možemo će seed tražilica s početnog skup podataka, ali dublju vrijednost će biti ugrađeni s vremenom uz pomoć pretplatnike prodavača koji oglašavanje informacija o radu svoje tvrtke. Promocije, nove robe, popularne marke, sesije posebne primjene usluge – – sve nekoliko primjera podataka koji dodaje vrijednost naše web-mjesto. Samostalno prijavio te integriranom corpus pretraživanja kada se prodavaču registrira kao pretplatnik podatke. Oglašavanje, uz modela pretplate omogućavaju naš novi poslovni strujanje prihod.

Pretraživanje bit će interakcije model Izraelu korisnika koji se nalazi na čisto oblaka platformi. Svrhu promjenom veličine i niska troškove, gotovo svemu moramo, s portala sučelje za upravljanje izvornim bit će putem internetskog servisa. Korištenje tražilica koja nudi značajke moramo izvan okvira za smo brzo stvaranje aplikacije za pretraživanje, bez potrebe za stvaranje i upravljanje pretraživanja modul nas.

## <a name="what-we-built"></a>Što smo u komponenti

WhatToPedia je tvrtka infomedia pokretanja. Ne možemo ugrađeni [WhatToPedia.com](http://whattopedia.com/) – – trenutno u test tržištu u Europi sjeverni čiji je datum Idi live 2. veljača 2015.. Naš baze klijenata je prvenstveno kavanama cigle i mortar koje moraju jeftin mrežne prisutnosti lako za upravljanje i održavanje.

Naš zadatak je privući shoppers kroz sučelje za sjajno internetsko pretraživanje, povećanje rezultata koji se temelji na grad ili susjedstvo, marke imena pohranu ili spremanje vrsta. Attracting shoppers sadrži efekta Val motivating trgovaca na malo Pretplaćivanje na našem web-mjesta portala. Pretplate se naplaćuju, jeftin brzinom.

 ![][7] 

Nakon registracije za pretplatu, prodavača preuzme njihove postojeći profil (stvorio prethodno nam iz kupljeni podataka) ažuriranje uz dodatne podatke o promocije, istaknute marke ili objava. Sesije značajke, kao što su jezike kojima se govori, valutama prihvaćen, tax-free košarice može biti koja se sama prijavljenim da biste bolje privući shoppers koji tražite te amenities.

## <a name="who-we-are"></a>Tko smo

Moje ime je pošiljatelj Segato (Microsoft Consulting), a li radili s Jovan Boelling, potencijalni klijent programiranje na WhatToPedia za dizajniranje rješenja. 

WhatToPedia je pokretanja test marketinški njezinu novu portala tvrtke Švedska, gdje se najčešće 60,000 lanaca cigle i mortar SMEs (male i srednje veličine velike tvrtke). Budući da bismo znali osoba košarice u Europi čitanja više jezika, a ima više valuta, ne možemo sastavljanje rješenja koja bi odgovarao višejezične potrošačka. Ne možemo potrebne i pronaći tražilica koja podržava naš višejezične preduvjeti u pretraživanju Azure.

Azure pretraživanje je na veze igra za naše projekt. Prije no što dostupnost Azure pretraživanja, ne možemo potrošeni dosta energiju radi kroz kinks izgradnje našim tražilica. Pretraživanje Azure pojavljuju kao na mrežni servis najvećih hurdle tehnička i administrativnih uklonjen iz naših rješenja, koji namijenjena pristup oglašavanje brže i Robusniji sučelje za pretraživanje.  

## <a name="how-we-did-it"></a>Kako ćemo niste

Naš vidom je da biste sastavili dovršeno infrastrukture koji se temelji na servise u oblaku samo. Microsoft nije odabran kao strateški platformu jer je davatelj usluga koje nude potrebne services (za suradnju i razvoj), promjena veličine na zahtjev i jeftin cijene.
 
### <a name="high-level-components"></a>Više razine komponente

Ne možemo ugrađeni tvrtke, a ne samo web-mjesta. Podrške cijelu trud potreban širok raspon alata i aplikacija. Ne možemo prihvatile Visual Studio i Visual Studio Team Services za razvoj internetski servis tima Foundation (TFS) za kontrolu izvorišnog i upravljanje scrum, Office 365 za komunikaciju i suradnju i naravno Microsoft Azure za sve operacije vezane uz web-mjesta za pohranu. Uz Visual Studio na IDE navedeni su izravno dodjele resursa za Azure, uz integraciju TFS Online nudi programa pojačavanje dodatne produktivnost.

Dijagramu u nastavku prikazuje s više razine komponente koje se koriste u WhatToPedia infrastrukture.

   ![][8]

### <a name="how-we-use-microsoft-azure"></a>Kako koristimo Microsoft Azure

Pogled na zeleni okvire u prethodno dijagram, vidjet ćete da je WhatToPedia rješenja utemeljena na tih servisa:

- [Azure pretraživanja](https://azure.microsoft.com/services/search/)
- [MVC 4 koristio Azure web-mjesta](https://azure.microsoft.com/services/websites/)
- [Azure WebJobs troši](../app-service-web/websites-webjobs-resources.md)
- [Baze podataka Azure SQL](https://azure.microsoft.com/services/sql-database/)
- [Spremište blobova PLATFORME Azure](https://azure.microsoft.com/services/storage/)
- [Isporuke SendGrid e-pošte](https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/)

Vrlo pozivanje rješenja je web-mjesto podataka i u okvir za pretraživanje. Dolje je prikazanom protok podataka od davatelja prodavača završetka klijentu:

  ![][9]

Pohrana podataka primarni je distributer i računovodstvenih podataka u bazi podataka SQL Azure. To se sastoji od početne dataset plus podatke specifične za prodavača dodane tijekom vremena. Priznanje za Azure WebJob da biste objavili ažuriranja iz baze podataka SQL corpus pretraživanja u pretraživanju Azure.

### <a name="presentation-layer"></a>Prezentacija sloja

Portal je programa Azure web-mjesto, implementirana u MVC 4 i [Na Twitteru samopokretanja programa](http://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29). Ne možemo odabrali MVC jer nudi mnogo čišćenje pristup u HTML od ASP.NET utemeljenu na obrascima razvoj. Da biste izbjegli stvaranje aplikacije za više uređaja i održavanje više mobilne platforme, na Twitteru samopokretanja programa nije odabran za podršku na svim uređajima i platformama.

### <a name="authentication-permissions-and-sensitive-data"></a>Provjera autentičnosti, dozvolama i povjerljive podatke

Shoppers anonimno potražite na web-mjesta. Kao takve, bez prijave uvjete shoppers niti ne možemo pohraniti sve korisničke podatke. 

Trgovaca na malo su različite priče. Ovdje, ne možemo pohranite informacije o profilu dostupnog javnosti, podatke o naplati i medijske sadržaje koji žele da biste otkrili na web-mjestu. Svakog prodavača koji je pretplata na web-mjesto se Prijava korisnika, koristi za provjeru autentičnosti korisnika prije uspostavljanja ažuriranja pretplatnika profil.  Ne možemo sigurno pohranjuje sve podatke za pretplatnike u baze podataka SQL Azure i blobova PLATFORME Azure prostora za pohranu.
Ne možemo uključila za modelu provjere autentičnosti na temelju .NET s provjeru autentičnosti utemeljenu na obrascima. Ne možemo odabrali odlučile za njegov jednostavnosti; niste moramo uloge, podrška za korisničko Sučelje i ostale druge značajke koje se isporučuju uz druge pristupa. 

Da biste bili sigurni da trgovaca na malo vide samo podatke koji pripada ih, koju smo stvorili prodavača ID-a za svakog prodavača koje se naknadno na svim za čitanje i pisanje operacije koje su potrebne podatke specifične za prodavača. Na taj se način smo pronašli da niste nije potrebna da biste dodijelili dozvole za bazu podataka za pojedinačne trgovaca na malo. Sve trgovaca na malo raditi s sustav u odjeljku uloge jednu bazu podataka, ID prodavača kao naš tehnika odvajanja podataka.

Budući da našu tvrtke sve o do efekata (vožnju više tvrtke da biste trgovaca na malo, stvaranje incentive Oglasite i pretplata), a zatim ćemo možete crtati crtu zadužen za nabavu s Interneta. Kao takve, neće pronaći košarici na našem web-mjestu koje pojednostavljuje naš sigurnosne preduvjete. 

Drugi simplification ćemo zapošljavanju moram spoljni operacije plativih naš naplatom i računima. Po usmjeravanje kupca informacije o plaćanju izravno drugih proizvođača ([SveaWebPay](http://www.sveawebpay.se/)), možemo smanjiti rizika Pridruživanjem pohranu i štiti osjetljive podatke u našem služi za pohranu podataka. 

### <a name="search-engine"></a>Tražilica

Temeljni naš rješenje je tražilica utemeljena na servis za pretraživanje Azure. Prethodno, ne možemo ugrađena prilagođena tražilica, ali tijekom tog postupka smo Rač složenosti trud je vrlo visoka uistinu i koji se to od vas zatraži nam treba uzeti u obzir druge alternative. 

Osnovni značajke koje su najvažnije nam dio:

- Filtri
- Slojevito navigacije
- Povećanje rezultata
- Kretanje AJAX stranicama

Pretraživanje u programu internet ne unese nam sljedeći videozapis koji inspired nam isprobajte Azure pretraživanja: [Precizno Dive pri TechEd Europe](http://channel9.msdn.com/events/TechEd/Europe/2014/DBI-B410) 

Nakon gledanja videozapisa, ne možemo su spremni za sastavljanje predložak koji se temelji na što smo vidjeli. Jer smo već imate podatkovnog modela u MVC, stvaranje na predložak jednostavne jer je podaci sadržani uvjeta pretraživanja i smo imali već uspjeli članku preduvjeti za kako ćemo željeli sortirati, fasete, i filtriranje podataka. 

Ovo je kako ćemo komponenti u predložak.

**Konfiguriranje usluge pretraživanja Azure**

1. Prijavite se na Portal za klasični Azure i dodali servis za pretraživanje u našem pretplate. Koristi zajedničku verzije (besplatno naš pretplate).
2. Stvaranje indeksa. Za predložak, koristi portal korisničkog Sučelja za određivanje polja za pretraživanje i stvaranje bilježenja rezultata profila. Naš bilježenja rezultata profila temelji se na podataka o lokaciji: država | Grad | adresa (pogledajte: dodavanje bilježenja rezultata profili).
3. Kopirajte URL servisa i administratore api ključ naš konfiguracijske datoteke. Ovaj ključ je na stranici za pretraživanje servisa na portalu pa se koristi za provjeru autentičnosti servis.
    
**Razvoj posao indeksiranje pretraživanja – konzole za Windows**

1. Saznajte sve distributere iz baze podataka.
2. Poziv API servisa Azure pretraživanja da biste prenijeli distributere jedan po jedan (potražite u članku: http://msdn.microsoft.com/library/azure/dn798930.aspx).
3. Svojstvo postaviti u bazi podataka koji je indeksiran prodavača rastuće indeksiranja. To ćemo niste dodavanjem programa 'indeksiranje' polja koje pohranjuje status indeksa svakog profila (označena ili nije). 

Potražite dodatak za isječak koda koji stvara zadatak za indeksiranje.

**Razvoj web-Portal pretraživanje – MVC**

1. Pozivanje Azure servisa za pretraživanje da biste dohvatili sve dokumente iz pretraživanja (potražite u članku: http://msdn.microsoft.com/library/azure/dn798927.aspx)
2. Izdvajanje pratiti odgovor servisa za pretraživanje (pomoću json.net http://james.newtonking.com/json)
   - Rezultati
   - Pozornici
   - Broj rezultata
   - Razvoj korisničko sučelje za prikaz rezultata pretraživanja, pozornici i broji (smo već imate to).

Ovo je kod koji se koristi da biste dobili rezultate iz pretraživanja Azure:

    string requestUrl = 
    string.Format("https://{0}.search.windows.net/indexes/profiles/docs?searchMode=all&$count=true&search={1}&facet=city,count:20&facet=category&$top=10&$skip={2}&api-version=2014-07-31-Preview{3}", Config.SearchServiceName, EscapeODataString(q), skip, filter);
      var response = client.GetAsync(requestUrl).Result; //  + Uri.EscapeDataString("hennes")
      response.EnsureSuccessStatusCode();
     dynamic json = JsonConvert.DeserializeObject(response.Content.ReadAsStringAsync().Result);

**Povećanje po lokaciji**

Vjerojatno najvažnije zahtjev za provjerite je li predložak dio dodavanja ključna riječ za pretraživanje mjesta na upit. Ključan naš portal za ako korisnik unese naziv grada u pretraživanju upit, koje prodavača u određenom gradu želite rangirati parametar veća od prodavača pojavljuju grad ključne riječi u opisu. Za taj zahtjev za rangiranje veće od ostala polja polja Grad koristi bilježenja rezultata profila.

**Više jezika za podršku**

Ne možemo potreban za prikaz ispravne rezultate na jezicima koji se ispravno, a sadrže mogućnost traženja iste rezultate na različitim jezicima. Su dvije strane tog problema: 

- Traženje riječi u više jezika
- Prikaz rezultata pretraživanja u željeni jezik

Ne možemo riješiti dio prezentacije dodavanjem dokumenta za svaki jezik lokalizirane tekstom i svojstvo s jezikom. Kada korisnik unese pojam za pretraživanje ne možemo korisnika `$filter` odlučila izraza za filtriranje na jezik korisnika.

Svaki dokumenata ima skrivene svojstvo pod nazivom "gradova" utemeljena na vrsta zbirke. Ovo svojstvo pohranjuje nazive gradova u sve jezike, što korisniku za pretraživanje na više jezika.

###<a name="data-storage"></a>Pohrana podataka

Svi podaci (profila, pretplate i računovodstveni) se pohranjuju u bazi podataka SQL. Sve medijske datoteke spremaju se u spremište blobova PLATFORME Azure, uključujući slike i videozapise koje ste dobili od se prodavaču. Korištenje zasebne blobova izolira efekata prijenosa datoteka; datoteka nikad se Suradnja mingled u web-mjesta tako da mi ne morate ponovno stvaranje web-mjesta kad god dodat ćemo datoteke.

Važno prednost naš dizajna za pohranu je više razvojni inženjeri mogu dijeliti jedan razvoj prostora za pohranu. Neki preduvjeti za WhatToPedia project je moći stvoriti razvojno okruženje za 15 minuta, uključujući podatke za prodavača, slika i videozapisa. Dohvaćanje najnovije podatke iz TFS Online, pokretanje SQL skripte i pokrenut posao uvoza, okruženje za dovršavanje mogu biti stood čas uopće. Praksa poboljšava i pripremna postupak.

###<a name="webjobs"></a>WebJobs

Da biste ažurirali podatke u indeks koristimo Azure WebJobs. Stvaranjem posao indeksiranje pretraživanja indeksiranja dio je vrlo lako integrirati u našem rješenja. Samo promjena kod ćemo proizvela je kako bi odgovarao posao Indeksiranje je da biste dodali programa `Indexed` polja u našem podatkovni model da biste naznačili stanje indeksa. Prilikom dodavanja ili ažurirati, novi profil u `Indexed` je polje postavljeno na false. Isto vrijedi i ako se prodavaču od promjene upisom podataka profila putem portala sustava.  

Posao izgleda za sve retke koji se pojavljuju `Indexed` postavite na false. Kada pronađe retku, dokument objavljuje Azure pretraživanja, a zatim na `Indexed` polje postavljeno na true. Imamo niste plan za dodavanje i ažuriranje podataka jer je servis za pretraživanje Azure zapravo vodi brigu o to. Ako dodate dokument koji se već nalazi, servis će to ažuriranje automatski.

Sve zadatke web razvijena su kao konzole aplikacije koje možete prenijeti na Azure web-mjesta kao ZIP datoteke, raspakirana, a zatim zakazano.

Zadatak je zakazano izvođenje svakih 5 minuta web zakazani zadatak. Ne možemo izračunava se da je servisu trebalo približno tri minute za prijenos 3000 dokumenata koji je u našem preduvjeti. 

> [AZURE.NOTE] Postoji značajka indeksiranje predložak koji je nedavno uvedena u pretraživanju Azure. Ta značajka prekasno isporučen za nam da biste koristili u našem prvo izdanje, ali čini se da biste riješili isti problem koristi naš indeksiranje posla, koji je da biste automatizirali operacija učitavanja podataka.


###<a name="backup-strategy"></a>Strategije za sigurnosne kopije

Ne možemo dizajniran višestruki tiered sigurnosne kopije strategije se može oporaviti iz raspona scenarije iz Katastrofalna pogreška, prema dolje do oporavak pojedinačne transakcije. Resursi za zaštitu uvrstiti tri vrste podataka (web-mjesta, pretplatnički podataka i medijske datoteke). 

Najprije držanjem izvornog koda web-mjesta u TFS Online bismo znali da ako funkcionira web-mjesto, ne možemo možete ponovno stvaranje je ponovnim objavljivanjem iz TFS. 

Pretplatnički podataka u bazi podataka SQL Azure je najčešće osjetljive resursa. Ne možemo natrag to pomoću ugrađene značajke (pogledajte [i vraćanja sigurnosnih kopija za baze podataka SQL Azure](http://msdn.microsoft.com/library/azure/jj650016.aspx)). Raspored sigurnosnog kopiranja je potpuna sigurnosna kopija baze podataka jednom tjedno, razlikovno sigurnosne kopije baze podataka jednom dnevno, a zapisnik transakcija sigurnosne kopije svakih 5 minuta.  Zadana veličina podataka, to je rješenje više prikladno za naše podatke odmah i predviđene jedinice.

Treći, ne možemo spremiti sliku i videodatoteke u spremište blobova PLATFORME Azure. Ne možemo su i dalje procjene ultimate sigurnosne kopije plan za podatke, preporučuje se Cloudberry Explorer za Azure kao potencijalne rješenje. Zasad koristimo u WebJob da biste kopirali slika i videozapisa na drugo mjesto.

##<a name="what-we-learned"></a>Ono što smo naučili

Jer smo već imate podatke, je jednostavno uspostaviti dokaz pojam. Unutar sata, ne možemo imali predložak s pozornici mjerača, označavanje stranica, rangiranih profila i rezultata pretraživanja. Rezultati pretraživanja su pa precizno, smo odlučili da biste uklonili neke filtri prezentirati završetka klijentu. 

Najvećih iznenađenje za nam je brzinu nije Saznajte Azure pretraživanja i koliko smo imate natrag. Doslovno, ne možemo uspostaviti dokaz pojam u nekoliko sati (pogledajte napomenu u nastavku), zamjena 500 redaka koda 3 retka koda u sučelju aplikacije (plus novi WebJob) i dobivanje boljih rezultata. 

Prethodno, naš kod implementirana broja stranica, broji i druge ponašanja standardne za pretraživanje. Pomoću pretraživanja Azure, rezultati smo vratiti obuhvatiti pristupa pretraživanja, pozornici, označavanje stranica podataka, broji – svi sadržaji ćemo potrebne i su potrebe za navesti nas. Također se uključiti povećanje i ugrađene slojevito navigacije, a niste imamo u našem izvorne rješenja.

Maksimalnog test tijekom implementacije je da je Pretpreglednu verziju i traženje podataka i zajedničke sadržaje je teško. Nakon uspostave nekoliko točke smo pronašli je putem servisa za pretraživanje Azure vrlo jednostavne zbog njezin oblik REST API-JA i JSON. Ne možemo poziva framework izravno iz dodataka za najčešće Otvori izvor kao što je JQuery JSON.Net pa ćemo pomoću alata kao što je Fiddler za brz početak i ispravljanje pogrešaka. 

> [AZURE.NOTE] Osim pojavljuju podataka prepped ga pomaže da sadržaj nam već izgradnju u predložak razumjeti kako pretraživanja radi tehnologije, čime nam produktivnosti i više appreciative od ugrađenih značajki. Ako vam je potrebna steći potrebna znanja na izgradnji upita za pretraživanje, slojevito navigacije, filtri, itd možete očekivati prototyping na dulje. 

###<a name="controlling-facets-in-the-search-presentation-page"></a>Kontroliranje pozornici na stranici prezentacije pretraživanja

Jedna od naše learnings tijekom dokaz-od-pojam nije planiranje pažljivo upfront pozornici. Nakon učitavanja velike količine podataka u rješenje, smo vidjeli je li sheer količinu pozornici prevelika za predstavljanje korisnicima. 

To ćemo otkloniti pomoću constraining parametar fasete count. Parametar count nameće teško ograničenje broja pozornici vrate korisniku. Moguće je pronaći vezu koja obuhvaća rasprave parametra count [ovdje](search-faceted-navigation.md).

###<a name="webjobs-for-scheduling-tasks"></a>WebJobs za zakazivanje zadataka

Azure pretraživanja nije samo pleasant iznenađenje za us. Ne možemo otkrio je pomoću WebJobs da biste automatizirali naš operacija učitavanja podataka za pretraživanje Azure vastly nadređenog naš pristupa prethodni koji entailed pomoću namjenski VM pokrenut raspored Windows s troši o ažuriranju indeks pretraživanja. WebJobs je jednostavnije da biste konfigurirali i jednostavnijim za ispravljanje pogrešaka i naravno približno jeftinijim od morati platiti namjenski VM.

###<a name="azure-blob-storage-explorer-for-updating-images"></a>Azure Explorer spremišta blobova PLATFORME za ažuriranje slike

Ne možemo pronaći biti vrlo koristan u upravljanje ažuriranjima slika i videozapisa na web-mjesto pomoću programa [Explorer za spremište blobova PLATFORME Azure](https://azurestorageexplorer.codeplex.com/) (dostupno na codeplex). Ne možemo ga koristiti kao alat za razvojne inženjere ručno ažurirati slike i videozapise koji su dio naš glavnog web-mjesta. Ne možemo pronaći da bi bio fleksibilnije od implementacija promjene na portal sustava, a uklanja dovršeno test iteracije po potrebi smo da biste ažurirali sliku. 

##<a name="a-few-final-words"></a>Konačni kratak

Hvala sjajno osobe na WhatToPedia za dopustite nam da biste zajednički koristili svoje priče!  

Nadamo ovog slučaja pronaći korisne. Idite da biste koristili pretraživanje Azure se preporučuje nekoliko resursa koji će vam duž brže:

- [Forum MSDN trakom za pretraživanje Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch)
- [StackOverflow ima oznake](http://stackoverflow.com/questions/tagged/azure-search)
- [Dokumentacija stranice na Azure.com](https://azure.microsoft.com/documentation/services/search/)
- [Azure dokumentaciju za pretraživanje na MSDN-u](http://msdn.microsoft.com/library/azure/dn798933.aspx)


##<a name="appendix-search-indexer-webjob"></a>Dodatak: WebJob za indeksiranje pretraživanja

Sljedeći kod sastavlja indeksiranje spomenute u odjeljku na izgradnji u predložak.

        static void Main(string[] args)
        {
            int success = 0;
            int errors = 0;

            Log.Write("Starting job","", System.Diagnostics.TraceLevel.Info);

            var serviceName = ConfigurationManager.AppSettings["SearchServiceName"];
            var serviceKey = ConfigurationManager.AppSettings["SearchServiceKey"];

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("api-key", serviceKey);

            var db = new DB(Config.ConectionString);

            var recreateIndex = false;
            Boolean.TryParse(ConfigurationManager.AppSettings["RecreateIndex"], out recreateIndex);

            if(recreateIndex)
            {
                Log.Write("Recreating index and set all to no index", "", System.Diagnostics.TraceLevel.Info);
                db.SetAllToNotIndexed();
                RecreateIndex(serviceName, client);
            }            
            
            var profiles = db.Profiles.Where(p=>!p.Indexed).ToList();

            Log.Write(string.Format("Indexing {0} profiles",profiles.Count),"", System.Diagnostics.TraceLevel.Info);

            var cities = db.Cities.ToList();
            var categories = db.Tags.Where(p=>p.ParentId==null).ToList();            

            foreach (var profile in profiles)
            {
                Log.Write(string.Format("Indexing profile {0}", profile.Name),"",profile.ProfileId,0,System.Diagnostics.TraceLevel.Verbose);

                try
                {
                    var city = cities.Where(p => p.CityId == profile.CityId);
                    var category = categories.Where(p => p.TagId == profile.CategoryId);

                    var cityse = city.Where(p => p.Lang == "se").FirstOrDefault();
                    var cityen = city.Where(p => p.Lang == "en").FirstOrDefault();
                    var categoryse = category.Where(p => p.Lang == "se").FirstOrDefault();
                    var categoryen = category.Where(p => p.Lang == "en").FirstOrDefault();

                    var citysename = cityse == null ? "" : cityse.Name;
                    var cityenname = cityen == null ? "" : cityen.Name;
                    var categorysename = categoryse == null ? "" : categoryse.Name;
                    var categoryenname = categoryen == null ? "" : categoryen.Name;

                    var tags = db.GetTagsFromProfile(profile.ProfileId);

                    var batch = new
                    {
                        value = new[] 
                    { 
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_en",
                            profileid = profile.ProfileId.ToString(),
                            city = cityenname,
                            category = categoryenname,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "en",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        },
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_se",
                            profileid = profile.ProfileId.ToString(),
                            city = citysename,
                            category = categorysename,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "se",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        }
                    },
                    };

                    var response = client.PostAsync("https://" + serviceName + ".search.windows.net/indexes/profiles/docs/index?api-version=2014-10-20-Preview", new StringContent(JsonConvert.SerializeObject(batch), Encoding.UTF8, "application/json")).Result;
                    response.EnsureSuccessStatusCode();

                    db.Entry(profile).State = System.Data.Entity.EntityState.Modified;
                    profile.Indexed = true;
                    db.SaveChanges();
                    success++;
                }
                catch(Exception ex)
                {
                    Log.Write("Error indexing profile", ex.Message, profile.ProfileId, 0, System.Diagnostics.TraceLevel.Verbose);
                    errors++;
                }
            }
            if(errors > 0)
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Error);
            }
            else
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Info);
            }
            
        }

        static void RecreateIndex( string ServiceName, HttpClient client)
        {
            var index = new
            {
                name = "profiles",
                fields = new[] 
                { 
                    new { name = "id",              type = "Edm.String",         key = true,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "profileid",       type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "cityid",          type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "city",            type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = true,  facetable = true, retrievable = true,  suggestions = true  },
                    new { name = "category",        type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = false, facetable = true, retrievable = true,  suggestions = false  },
                    new { name = "address",         type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "email",           type = "Edm.String",         key = false, searchable = true,  filterable = false, sortable = true, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "name",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true, suggestions = true },
                    new { name = "lang",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = false,  facetable = false,  retrievable = true, suggestions = false },
                    new { name = "brands",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descen",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descse",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "orgnumber",       type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "phone",           type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "zip",             type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "cities",          type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                   new { name = "categories",      type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                    new { name = "tags",            type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false }
                    
                }
            };

            var url = "https://" + ServiceName + ".search.windows.net/indexes/?api-version=2014-10-20-Preview";

            var deleteUrl = "https://" + ServiceName + ".search.windows.net/indexes/profiles?api-version=2014-10-20-Preview";

            try
            {
                var deleteResponseIndex = client.DeleteAsync(deleteUrl).Result;
                deleteResponseIndex.EnsureSuccessStatusCode();
            }
            catch (Exception ex)
            {

            }

            var responseIndex = client.PostAsync(url, new StringContent(JsonConvert.SerializeObject(index), Encoding.UTF8, "application/json")).Result;
            responseIndex.EnsureSuccessStatusCode();            
          


<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Subheading 4]: #subheading-4
[Subheading 5]: #subheading-5
[Next steps]: #next-steps

<!--Image references-->
[6]: ./media/search-dev-case-study-whattopedia/lightbulb.png
[7]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Search-Bageri.png
[8]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Stack.png
[9]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Archicture.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: ../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
 
