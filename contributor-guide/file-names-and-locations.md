<properties title="" pageTitle="Nazivi datoteke i mjesta za Azure tehničkih članaka" description="U članku se objašnjava struktura datoteke za članke i konvencije imenovanja morate slijediti kada stvorite novi članak." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="tysonn" />

#<a name="file-names-and-locations-for-azure-technical-articles"></a>Nazivi datoteke i mjesta za Azure tehničkih članaka

U našem tehničke spremišta sadržaja koristimo jednu mapu (mapa **članaka** ) svih članaka. Postoji bez hijerarhija mapa – svih članaka live u strukturi nehijerarhijsku datoteku. Ako stvorite mape s članaka u njima članci nije moguće objaviti.

Umjesto struktura datoteke kao organiziranje načelo koristimo na točno konvencije imenovanja datoteka koji jasno označavaju teme i koje doprinosa pri vidljivost na webu.

Evo što trebate znati:

+ [Pravila]
+ [Uzorak]
+ [Standardni Primjeri]
+ [Sadržaj Marketplace]
+ [Odobrenje naziv datoteke]

##<a name="rules"></a>Pravila

- Nazivi datoteka mogu sadržavati samo mala slova, brojeve i spojnice. 
- Bez razmaka ili interpunkcijske znakove. Koristite spojnice za razdvajanje riječi i brojeva u nazivu datoteke.
- Više od 80 znakova – to je objavljivanje ograničenja u sustavu
- Korištenje akcija glagoli specifični, kao što su razvijati, kupite, sastavljanje, otklanjanje poteškoća s. Nema - nje riječi.
- Nemoj uvrštavati nema small riječi – te, u ili, itd.
- Sve datoteke mora biti u markdown i koristiti datotečni nastavak .md.
- Pravopisa riječi izvan; izbjegavanje neodobreni ili nepotrebne kratice u nazivima datoteka

Kratice i initialisms u nazivima datoteka - određene smjernice:

- Skraćivanje naziva servisa Azure – prvi riječi naziva datoteke mora biti standard, natpisom Azure servisa ili tehnologiju naziv. 
-   Dopusti upravitelja resursa ili arm kao kratice bilo gdje u nazivu datoteke
- Ostale kratica standardni su prihvatljiva prema potrebi u nazivima datoteka. 

##<a name="pattern"></a>Uzorak

Slijede općenite uzorak:

 **Service-Platform-Language-Content-product-Version.md**

Koristite dijelove uzorak koji Primijeni i pregledajte popis članaka u spremištu da biste dobili ideju postojeće imena. Nazive koji ne započinju razvojni platformu ili naziv servisa su vjerojatno sumnjate da i slipped putem.

##<a name="standard-examples"></a>Standardni Primjeri

Evo nekoliko primjera valjani nazivi koji slijede s uzorkom. :

- Oblak-servise-dotnet-neprekinuti-delivery.md
- Mobile-usluge – ios – get-started.md
- documentdb-upravljanje-account.md
- Mobile-Services-dotnet-backend-Get-Started-Settings-Sync.md
- Active-Directory-Java-Authenticate-Users-Access-Control-eclipse.md
- Virtual-machines-Install-Windows-Server-2008r2.md
- Predmemorija aspnet-sesija-stanje-davatelja
- Azure-SDK-dotnet-Release-Notes-2-8
- storsimple-disaster-Recovery-using-Azure-site-Recovery

##<a name="marketplace-content"></a>Sadržaj Marketplace

Da biste razlikovali sadržaj koji usredotočuje se na partnera doprinos Azure marketplace, pokrenite nazivima datoteka s "tržište". Ovaj sadržaj ne smije biti čestih, kao što je većina partnera sadržaja treba stvoriti na web-mjestima za partnere vlastitu.

- Marketplace-mongodb-Virtual-machines-Install-Windows-Server-2008r2.md

##<a name="file-name-approval"></a>Odobrenje naziv datoteke

To je posao naš grupe pregledavatelja istaknuti zahtjev za pregled nazivima datoteka kada novu datoteku šalje se u spremište na prvi put. Istaknuti zahtjev pregledavatelji treba pregledajte naziv datoteke i povratne informacije putem toka komentar istaknuti zahtjeva ako su vam potrebne promjene. Naziv datoteke potrebno ispraviti prije istaknuti zahtjev je korisnik prihvati. Suradnici možete jednostavno automatske ažuriranje na čekanju istaknuti zahtjev.

##<a name="folder-names-in-the-repo"></a>Nazive mapa u na repo

Mape koje se moraju stvarati samo za servise, a naziv datoteke moraju se podudarati slug servisa. Koristite samo slova i crtice, a pomoću sva mala slova. Nabavite odobrenje iz spremišta administrator da biste mogli stvoriti novu mapu koja nije objavljenu usluge.

##<a name="changing-case-in-file-names"></a>Promjena slova u nazivima datoteka

Operacijski sustavi Windows su slova, pa ako morate promijeniti naziv datoteke da biste riješili problem kućište, bolje je promjene substantive, osim ako ih ne možete promijeniti na Linux ili Mac Ako, na primjer:

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

Da biste preimenovali datoteku, koristite sljedeću naredbu:
```
  git mv <articles/service-folder/current-file-name.md> <articles/service-folder/new-file-name>
```

###<a name="contributors-guide-links"></a>Suradnici vodič veze

- [Članak](./../README.md)
- [Indeks članaka upute](./contributor-guide-index.md)


<!--Anchors-->
[Pravila]: #rules
[Uzorak]: #pattern
[Standardni Primjeri]: #standard-examples
[Sadržaj Marketplace]: #marketplace-content
[Odobrenje naziv datoteke]: #file-name-approval
