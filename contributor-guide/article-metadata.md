

#<a name="metadata-for-azure-technical-articles"></a>Metapodaci za Azure tehničkih članaka

Azure tehničkih članaka sadrže dva odjeljka metapodatke - u svojstva i oznake sekciji. U odjeljku Svojstva omogućuje neke Automatizacija web-mjesta i sadržaja SEO dok oznake sekcija omogućuje mnogo Interna sadržaja izvješćivanje. Potrebni su obje sekcije.

- [Sintaksa]
- [Korištenje]
- [Atributi i vrijednosti u odjeljku Svojstva]
- [Atributi i vrijednosti u odjeljku oznake]

##<a name="syntax"></a>Sintaksa

U odjeljku Svojstva koristi sljedeću sintaksu:

    <properties
       pageTitle="Page title that displays in search results and the browser tab | Microsoft Azure"
       description="Article description that will be displayed on landing pages and in most search results"
       services="service-name"
       documentationCenter="dev-center-name"
       authors="GitHub-alias-of-only-one-author"
       manager="manager-alias"
       editor=""
       tags="optional"
       keywords="For use by SEO champs only. Separate terms with commas. Check with your SEO champ before you change content in this article containing these terms."/>

U odjeljku oznake koristi sljedeću sintaksu:

    <tags
       ms.service="required"
       ms.devlang="may be required"
       ms.topic="article"
       ms.tgt_pltfrm="may be required"
       ms.workload="na"
       ms.date="mm/dd/yyyy"
       ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

##<a name="usage"></a>Korištenje

- Naziv elementa i nazive atributa su velika i mala slova.
- Na <properties> odjeljak mora biti u prvom retku datoteke.
- Ostavite prazan redak nakon svakog odjeljka metapodataka i prije naslov stranice da biste bili sigurni da naslov stranice pravilno pretvara se u HTML tijekom postupka objavljivanja.

## <a name="attributes-and-values-for-the-properties-section"></a>Atributi i vrijednosti u odjeljku Svojstva

![](./media/article-metadata/checkmark-small.png)**pageTitle**: potrebna; Važno je da biste SEO. Na kartici preglednika i kao naslov u rezultat pretraživanja, pojavit će se tekst za taj atribut. Koristiti 55 do 60 znakove razmacima i uključujući identifikator web-mjesta *| Microsoft Azure* (upišete kao: razmak kanala prostor Microsoft Azure).  Na pageTitle mora biti razlikuje od sustava H1.

![](./media/article-metadata/checkmark-small.png)**Opis**: potrebna; Važno je SEO (važnosti) i web-mjesta. Opis mora biti najmanje 125 znakova dugačak kako 155 znakova razmacima. Namjenu sadržaja da bi korisnici će sigurni trebate li da biste odabrali s popisa rezultata pretraživanja. Vrijednost je:

- Ovaj tekst može se prikazati kao opis ili kratki pregled odlomak u rezultatima pretraživanja na Google.
- Ovaj tekst prikazuje se u [rezultatima članak indeksa](https://azure.microsoft.com/documentation/articles/).

![](./media/article-metadata/checkmark-small.png)**usluga**: potrebni za članke koje se tiču servisa. Ta vrijednost je ofter se nazivaju "slug uslg". Popis svih primjenjivih servisa, odvojenih točkama sa zarezom. Prvi servis koji navedete će pogon navigacijski elemenata stranice i lijevom navigacijskom oknu koji je diplayed sa stranicom.

U člancima navedete vrijednost servisi i vrijednost documentationCenter vrijednost services će pogon navigaciji. Dodatne vrijednosti koje navedete prikazat će se kao oznake u objavljeni članak. Vrijednosti:

- Active directory
- Active directory b2c
- Active directory ds
- aplikacija service\api
- API-upravljanje
- Servis za aplikaciju
- aplikacija servic\mobile
- aplikacija service\web
- aplikacija service\logic
- pristupnik za aplikaciju
- aplikacija uvida
- Automatizacija
- portal za Azure
- Azure upravitelj-resursa
- Azure stogu
- sigurnosno kopiranje
- grupe
- najbolje vježbe
- BizTalk services
- predmemoriju
- CDN
- servisi u oblaku
- katalog podataka
- Podaci tvorničke
- Podaci lake analytics
- Podaci lake spremišta
- devtest Laboratorija
- DNS-a
- documentdb
- expressroute
- događaj koncentratora
- hdinsight
- iot koncentratora
- ključ sigurnog
- raspoređivača opterećenja
- stroj učenje
- Trgovina
- Media services
- Mobilni radnje
- Mobilna usluga
- Višestruka-provjera autentičnosti-
- obavijest o koncentratora
- radu uvida
- Postupci upravljanja paketa
- powerapps
- Upravitelj za oporavak
- redis predmemorije
- RemoteApp
- Upravljanje pravima
- Raspored
- pretraživanje
- Centar za sigurnost
- servis bus
- servis tkanina
- oporavak web-mjesta
- SQL baze podataka
- SQL podataka skladištu
- izvješćivanje o SQL
- prostor za pohranu
- spremište
- storsimple
- strujanje analytics
- promet manager
- virtualnog računala
- virtualne mreže
- Visual-studio-online
- pristupnik za VPN-a
- web-mjesta

![](./media/article-metadata/checkmark-small.png)**documentationCenter**: potrebne za razvojni usmjereni na članke najbolje istaknute kroz Razvojni centar. Navedite jednu Razvojni centar za ili jezika koji se primjenjuje na članak. Vrijednost sastavite popis će pogon navigacijski elemenata stranice. U člancima navedete vrijednost servisi i vrijednost documentationCenter vrijednost services će pogon navigaciji. Vrijednosti:

- **.NET**
- **nodejs**
- **Java**
- **php**
- **Python**
- **Ruby**
- **mobilni**: zastarjela. Zamijenite određene mobilne platforme.
- **ios**: Verifing novu vrijednost
- **android**: Provjera novu vrijednost
- **Windows**: Provjera novu vrijednost
- **xamarin**: Provjera novu vrijednost

![](./media/article-metadata/checkmark-small.png)**autori**: potrebna samo jednu vrijednost. Popis GitHub računa za primarni autor ili članak SME. Taj atribut pogoni na crta uz na objavljeni članak. Prikaži popis samo jedna in spite of množini atribut.

![](./media/article-metadata/checkmark-small.png)**Upravitelj**: potreban je li Microsoft suradnika. Navedeni pseudonim za e-pošte sadržaja objavljivanje upravitelja za područje tehnologije. Ako ste suradnik zajednice, uključiti atribut, ali ga ostavite prazan pa ćemo ga mogli ispuniti.

![](./media/article-metadata/checkmark-small.png)**uređivač**: ne koristi. Ne koristite druge svrhe.

![](./media/article-metadata/checkmark-small.png)**oznaka**: nije obavezno. Uvrstite samo ako želite omogućiti veze u odjeljku članku povratna stranicu članka indeksa (http://azure.microsoft.com/documentation/articles/) prefiltered popis članaka koji odgovara jednoj od odobrene vrijednosti. Ove vrijednosti namjena omogućuje grupiranje sadržaja kada sadržaja grupiranja nije specifične za servis. Ove oznake mogu nuditi označavanje koji označava tehnologije stoga se članak odnosi na. Ova vrijednost **ne** podržava prostoručni oznake ili hashtags; oznake mora biti omogućena na web-mjestu. Možete navesti više oznaka vrijednosti na jedan članak odvojenih točkama sa zarezom. Odobrene vrijednosti su:

  - Arhitektura
  - Azure upravitelj-resursa
  - Azure upravljanja-servisom
  - naplata
  - MySQL

![](./media/article-metadata/checkmark-small.png)**ključne riječi**: nije obavezno. Za korištenje samo champs SEO. Izrazi koji odvojene zarezima. **Prije no što promijenite ili izbrišite sadržaj u ovom članku koje sadrže te uvjete, obratite se vaš champ SEO.** Taj atribut zapisa ključnih riječi SEO champ je i prati da biste poboljšali pretraživanje položaj. Ključne riječi prikazuju se u objavljeni HTML. Provjera valjanosti obavezan atribut.

## <a name="attributes-and-values-for-the-tags-section"></a>Atributi i vrijednosti u odjeljku oznake

![](./media/article-metadata/checkmark-small.png)**MS.Service**: potrebna. Određuje Azure service, alat ili značajku koja se članak odnosi. Jedna vrijednost po stranici.

 Ako na stranici odnosi se na većem broju servisa, odaberite servis na koji je it Većina izravno odnosi; Ako, primjerice, članak koji koristi aplikaciju hostira na web-mjestima da bismo pokazali funkcionalnost servisa Bus treba imati na **servis bus** vrijednost, a ne **web-mjesta**. Ako se na stranicu primjenjuje se jednoliko većem broju servisa, odaberite **više**. Ako se na stranicu ne primjenjuju se na sve usluge (Ta će se rijetko), odaberite **vrijednosti n/d**.

 - **Active directory**
 - **Active directory b2c**
 - **Active directory ds**
 - **API-upravljanje**
 - **aplikacije servisa**: primjenjuje se samo na Općenito konceptualne materijale servis za aplikaciju
 - **aplikacija api-usluge**
 - **aplikacije servisa logike**
 - **aplikacije servisa mobile**
 - **aplikacija web-servisa**
 - **aplikacija uvida**
 - **pristupnik za aplikaciju**
 - **Automatizacija**
 - **Azure upravitelj-resursa**
 - **Azure sigurnosti**
 - **Azure stogu**
 - **sigurnosno kopiranje**
 - **grupe**
 - **najbolje vježbe**
 - **BizTalk services**
 - **naplata**
 - **predmemoriju**
 - **CDN**
 - **servisi u oblaku**
 - **katalog podataka**
 - **Podaci lake spremišta**
 - **Podaci lake analytics**
 - **devtest Laboratorija**
 - **expressroute**
 - **hdinsight**
 - **Internetske mogućnosti**
 - **iot koncentratora**
 - **ključ sigurnog**
 - **stroj učenje**
 - **Marketplace**: članci o Azure marketplace
 - **Media services**
 - **Mobilni radnje**
 - **Mobilna usluga**
 - **Višestruka-provjera autentičnosti-**
 - **više**: stranici odnosi se na većem broju servisa jednako
 - **n/d**: stranice ne primjenjuju se na sve usluge (rijetko)
 - **obavijest o koncentratora**
 - **radu uvida**
 - **powerapps**
 - **Upravitelj za oporavak**
 - **redis predmemorije**
 - **RemoteApp**
 - **Upravljanje pravima**
 - **Raspored**
 - **Centar za sigurnost**
 - **servis bus**
 - **servis tkanina**
 - **oporavak web-mjesta**: prethodno oporavak services
 - **SQL baze podataka**
 - **SQL podataka skladištu**
 - **izvješćivanje o SQL**
 - **prostor za pohranu**
 - **Spremanje**: članci o usluge dostupne u trgovini Azure
 - **storsimple**
 - **promet manager**
 - **virtualnog računala**
 - **virtualne mreže**
 - **Visual-studio-online**
 - **pristupnik za VPN-a**
 - **web-mjesta**

![](./media/article-metadata/checkmark-small.png)**MS.devlang**: potrebna. Određuje programski jezik koji se članak odnosi na. Jedna vrijednost po stranici.

 Ako se na stranicu primjenjuje se jednoliko dva programskog jezika, odaberite **više**. Ako se na stranicu je prvenstveno konceptualni i njezin sadržaj je obično odnosi se na više programskog jezika, odaberite **više**. Ako stranicu je namijenjeno pri razvojnim inženjerima i programskog jezika primjenjivošću kako bi nije odgovarajuće, odaberite **vrijednosti n/d**. Korištenje **rest api** za prepoznavanje REST API-JA referentne teme.

 - **CPP**
 - **dotnet**
 - **Java**
 - **JavaScript**
 - **više**: stranici odnosi se na više programskog jezika jednako.
 - **n/d**: stranica je ciljanja razvojnim inženjerima i nije specifična za sve jezike programiranje.
 - **nodejs**
 - **cilj c**
 - **php**
 - **Python**
 - **REST API-ja**
 - **Ruby**


![](./media/article-metadata/checkmark-small.png)**MS.topic**: potrebna. Specifičnosti upišite temu. Većina nove stranice stvorene od donatora bit će članak ili referenci.

 - **članak**: konceptualnih tema, vodič, vodič za značajku ili druge članak-referenca

 - **kampanja stranice**: samo Azure.com.  Stranice na kojoj je posebno dizajnirane kao odredišna stranica za vanjske kampanje, a nije dio primarnog web-mjesta IA.  Ne mogu koristiti za dokumentaciju članaka ili obični dokument odredišne stranice.  Primjeri: azure.microsoft.com/develop/net/aspnet/; Azure.microsoft.com/develop/Mobile/ios/

 - **Razvojni centar za-Početna-stranice**: samo Azure.com.  Na razvojni centrirali početnoj stranici, npr. / razviti/neto /

 - **Početak rada – članka**: dodijeliti članaka koji su istaknute u odjeljku početak rada i pregled lijevom navigacijskom oknu za uslugu.

 - **glavni članak**: "glavni" vodič namijenjen Uvod u usluge ili značajku koju će posjetitelji rada brzo pomoću servisa i pogone znak kopije slobodno probnu verziju i MSDN aktivacije. Dodjeljivanje tu vrijednost samo na članke koji su istaknute pri vrhu stranice odredišna dokumentacija servisa.

 - **Početna stranica**: gornje razine dokumentaciju početnu stranicu. Samo imamo dvije: azure.microsoft.com/documentation/ i msdn.microsoft.com/library/azure/

 - **indeks stranice**: druge razine odredišne stranice programskog jezika, servise ili značajke. Te su dodijeliti većem Azure.com i biblioteke i se koriste kao točke unosa za konkretne opsegu informacija. Primjeri: http://azure.microsoft.com/develop/mobile/resources-wp8/ http://msdn.microsoft.com/library/azure/jj673460.aspx, http://msdn.microsoft.com/library/azure/hh689864.aspx

 - **infographic stranice**: samo Azure.com. Stranice na kojoj se značajke za pretraživanje u infographic ili postera, na primjer http://azure.microsoft.com/documentation/infographics/windows-azure/

 - **Referenca**: programa API reference stranice (uključujući REST API-JA) ili PowerShell cmdlet referenca stranice

 - **servis za početnu stranicu**: samo Azure.com.  Dokument servisa početnu stranicu, npr. /documentation/services/virtual-machines /

 - **web-mjesta sekcije-Početna-stranice**: samo Azure.com. "Početnu stranicu" za određenu vrstu sadržaja na azure.com Primjeri: http://azure.microsoft.com/documentation/infographics/, http://azure.microsoft.com/documentation/scripts/, http://azure.microsoft.com/documentation/videos/home/, http://azure.microsoft.com/downloads/

 - **stranici video**: samo Azure.com.  Stranice na kojoj se značajke videozapisa, primjerice http://azure.microsoft.com/documentation/videos/azure-webjobs-hosting-testing-net/

![](./media/article-metadata/checkmark-small.png)**MS.tgt_pltfrm**: potrebna. Određuje ciljne platforme primjerice Windows, Linux, Windows Phone, iOS, Android ili posebne predmemorije platformama. Jedna vrijednost po stranici. Ta vrijednost bit će **n/d** za većinu teme osim mobile i virtualnih računala.

 - **predmemorije u uloga**
 - **Predmemorija višekratnik**
 - **Predmemorija redis**
 - **Servis za predmemoriju**
 - **zajedničko korištenje predmemorije**
 - **Command-line sučelja**
 - **ibiza**: sadržaj koji koristi Ibiza portal. Tom se mogućnošću poslužite samo u slučajevima u kojima se spominju je značajka dostupna na portalu Ibiza i portala za trenutni.
 - **mobilni android**: Azure.com samo odmah
 - **mobilni html**: Azure.com samo odmah
 - **mobilni ios**: Azure.com samo odmah
 - **Mobile-uređaju kindle**: Azure.com samo odmah
 - **Mobilni višekratnik**
 - **mobilni nokia x**: Azure.com samo odmah
 - **mobilni phonegap**: Azure.com samo odmah
 - **mobilni sencha**: Azure.com samo odmah
 - **Mobile windows**: Azure.com samo odmah; Univerzalni za Windows
 - **Mobile windows phone**
 - **Mobile windows trgovine**
 - **mobilni xamarin**: Azure.com samo odmah; Xamarin sve platforme
 - **mobilni xamarin android**: Azure.com samo odmah
 - **mobilni xamarin ios**: Azure.com samo odmah
 - **više**: stranici odnosi se na više platforme jednako
 - **n/d**: platformu specifikator nije moguće primijeniti na ovu stranicu
 - **PowerShell**
 - **VM linux**
 - **VM višekratnik**
 - **VM windows**
 - **VM windows sharepoint**
 - **VM-windows-sql-poslužitelj**
 - **Dodavanje veze za vanjskih--Uvod**: služi za identifikaciju grupi stranica za dodavanje veze za VANJSKIH početak rada. Oznaka dodaje 1/12/14.
 - **Dodavanje veze za vanjskih – što-se dogodilo**: služi za identifikaciju stranice Dodavanje veze za VANJSKIH početak rada što se dogodilo. Oznaka dodaje 1/12/14.

![](./media/article-metadata/checkmark-small.png)**MS.workload**: potrebna. Određuje Azure radno opterećenje koja se odnosi na stranicu. Jedna vrijednost samo po članka.

**Ažuriranje 6/8/15** Vrijednost ms.workload mapirani po xls, ne i vrijednosti u datoteci .md. Vrijednost ms.workload je i dalje potreban za provjeru valjanosti dok se ne može se ažurirati značajku. Zakazano koji funkcioniraju je u tijeku.  Koristite **"n/d"** kao vrijednost za sada.

![](./media/article-metadata/checkmark-small.png)**MS.Date**: potrebna. Određuje datum u članku je zadnji put pregledava za važnosti, točnost, ispravne zaslona i veze za rad. Unesite datum u obliku mm/dd/gggg. Datum i pojavit će se na objavljeni članak kao datum zadnje ažuriranje.

![](./media/article-metadata/checkmark-small.png)**MS.AUTHOR**: potrebna. Određuje author(s) povezan s temom. Interna izvješća (primjerice svježina) pomoću te vrijednosti desnom author(s) pridružiti u članku. Da biste naveli više vrijednosti mora odvojite ih zarezom. Microsoft pseudonima ili adrese e-pošte za dovršavanje nisu prihvatljivi. Duljina može biti dulji od 200 znakova.


###<a name="contributors-guide-links"></a>Suradnici vodič veze

- [Članak](./../README.md)
- [Indeks članaka upute](./contributor-guide-index.md)


<!--Anchors-->
[Sintaksa]: #syntax
[Korištenje]: #usage
[Atributi i vrijednosti u odjeljku Svojstva]: #attributes-and-values-for-the-properties-section
[Atributi i vrijednosti u odjeljku oznake]: #attributes-and-values-for-the-tags-section
