<properties
    pageTitle="Vodič za brzi početak: strojnog učenja preporuke API | Microsoft Azure"
    description="Azure strojnog učenja preporuke – vodič za brzi početak rada"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-cognitive-services-recommendations-api"></a>Vodič za brzi početak rada za Kognitivne API preporuke za usluge

U ovom dokumentu opisuje kako na onboard servisa i aplikaciju koju želite koristiti [API preporuke](http://go.microsoft.com/fwlink/?LinkId=759710).
Možete pronaći dodatne informacije o API preporuke i drugim servisima za Kognitivne [ovdje](http://go.microsoft.com/fwlink/?LinkId=759709). U ovom vodiču možete pronaći [Preporuke API Reference](http://go.microsoft.com/fwlink/?LinkId=759348) pri ruci.


<a name="Overview"></a>
## <a name="general-overview"></a>Općenito pregled

Ovaj je dokument Postupni vodič. Naš cilj je za vas voditi kroz korake potrebne uvježbati modela i postavite pokazivač na resurse koji vam omogućuje da zauzeti modela iz okruženja radnog.

Ovu vježbu se 30 minuta.

Da biste koristili [Preporuke API](http://go.microsoft.com/fwlink/?LinkId=759710), morate poduzeti sljedeće korake:

1. Stvaranje modela – model je spremnik podataka o korištenju, katalog podataka i preporuke modela.
1. Uvoz kataloga podataka – katalog sadrži metapodatke informacije o stavkama.
1. Uvoz podataka o korištenju - podataka o korištenju možete prenijeti u jedan od načina 2 (ili oboje):
  -  Prijenos datoteke koja sadrži podatke o korištenju.
  -  Slanje podataka acquisition događaja.
  Obično prijenos datoteke korištenje da biste mogli da biste stvorili modelu početne preporuke (Samopokretanje) i koristiti dok sustav prikuplja dovoljno podataka pomoću acquisition oblik podataka.
1. Sastavljanje preporuke model – to je asinkrona operacija u kojem sustava preporuke uzima sve podatke o korištenju i stvara preporuke modela. Ovaj postupak može potrajati nekoliko minuta ili nekoliko sati, ovisno o veličini podatke i konfiguracije parametara Sastavi. Kada pokretanje sastavljanje, dobit ćete ID-a Sastavi. Koristite da biste provjerili kada je završen postupka Sastavi prije pokretanja za preporuke.
1. Korištenje preporuke - Get preporuke za određenu stavku ili popis stavki.

<a name="GetStarted"></a>
### <a name="lets-get-started"></a>Počnimo!

Započet će stvaranje modela preporuke. Zatim ćemo ćete će vas voditi o korištenju rezultate generira model za power preporuke na web-mjestu sustava e-trgovine.

<a name="Ex1Task1"></a>
#### <a name="task-1---signing-up-for-the-recommendations-api"></a>1 – zadatak potpisivanja za preporuke API-JA ####

U ovom ćete zadatku će registrirati za servis za preporuke API-JA i stvaranje modela preporuke.

1. Idite na **Portal za Azure** na [http://portal.azure.com/](http://portal.azure.com/) i prijavite se pomoću računa za Azure.

1.  Kliknite **+ Novo**.

1. Odaberite mogućnost **Obavještavanje** .

1. Odaberite proizvod **Kognitivne API servisa** .
Za ovaj proizvod će vam omogućiti da biste pokrenuli pretplatu na servise za kognitivne API-ji (nominalne analize tekst, računalo vidom, itd.). Danas mi sadrži API preporuke.

1. Na stranici odredišna Kognitivne API servisa unesite **naziv računa** za vašu pretplatu za preporuke. (Za instace: "MyRecommendations"). Taj naziv mora imati sve razmake.

1. **API vrsta**, odaberite **preporuke**.

1. Na **sloju određivanje cijena**, možete odabrati neku tarifu. Može odabrati sloju **slobodno** za 10 000 transakcije/mjesec. Ovo je plan besplatne tako da bude dobar način za početak korištenja probne verzije sustava. Kada otvorite radni preporučujemo da razmislite o glasnoću zahtjev i sukladno tome promijeniti vrstu plana. (Napomena: imate samo jedan besplatne sloju pretplate istodobno)

1. Odaberite **Grupu resursa**ili stvorite novi Ako još nemate jedno mjesto.

1. Ostale elemente u dijaloškom okviru Stvaranje može se promijeniti. Ne možemo treba pokazuju da davatelja resursa danas je podržana samo iz Sjedinjenih Američkih Država centre za podatke.

1. Kada ste gotovi s bilo kojeg odabira, kliknite **Stvori**.

1. Pričekajte nekoliko minuta resursa koje želite uvesti.
Nakon što je uvedena možete prijeđite na odjeljak **tipke** u plohu **Postavke** za gdje će pružati primarnih i sekundarnih ključ za korištenje na API-JA.  Koliko će vam je potrebno prilikom stvaranja prvi će se model, kopirajte primarni ključ.

<a name="Ex1Task2"></a>
#### <a name="task-2---did-you-bring-your-data"></a>Zadatak 2 – jeste li prenijeti podatke? ####

Preporuke API ćete saznati iz kataloga i svoje transakcije omogućili preporuke dobar proizvoda. To znači da morate sažetak sadržaja uz dobar podataka o proizvoda (smo Nazivanje tog datoteke **kataloga** ) i skup transakcije dovoljna da bi pronalaženje zanimljivih uzoraka potrošnje (smo poziva **Korištenje**).

1. Obično bi upit transakcije baze podataka za te dijelovi informacija.
U slučaju nemate ih pri ruci, ne možemo je navesti oglednih podataka na temelju podataka Microsoft Store transakcije.

 Podatke možete preuzeti iz [ovdje](http://aka.ms/RecoSampleData). Kopiranje i otpakiravanje u MsStoreData.Zip u mapu na lokalnom računalu.

 > **Bilješke:** Uzorak koda koji će preuzeti i pokrenuti u zadatka 3 sadrži ogledne podatke koji su već ugrađen unutar njega – da bi ovaj zadatak nije obavezan.  Koje rečeno, ovaj zadatak omogućuje preuzimanje realističniji skupova podataka i omogućuju vam da biste shvatili ulaza u bolje API preporuke.

1.  Sada ćemo pogledajte datoteka kataloga. Pomaknite se do mjesta na koju ste kopirali podatke.
 Otvorite datoteku kataloga u **Bloku za pisanje**.

 Primijetit ćete da je datoteka kataloga vrlo jednostavne. Ima sljedeći oblik`<itemid>,<item name>,<product category>`

 >  AAA-04294, DwnLd Online E34 OfficeLangPack 2013 32 64, Office <br>
 > AAA-04303, DwnLd Online postavljanje OfficeLangPack 2013 32 64, Office  <br>
 > C9F 00168, KRUSELL Kiruna zrcaljenje naslovnica za Nokia Lumia 635 - Camel, dodatna oprema

 Ne možemo treba pokazuju da datoteke kataloga može biti puno bogatiji, na primjer možete dodati metapodatke o proizvodima (smo poziva te *značajke stavke*). U odjeljku [oblik kataloga](http://go.microsoft.com/fwlink/?LinkID=760716) API Reference više pojedinosti trebali biste vidjeti na oblik kataloga.

1. Pogledajmo isto pomoću podataka o korištenju. Primijetit ćete da je rok za korištenje u obliku `<User Id>,<Item Id>,<Time Stamp>,<Event>`.

  > 00037FFEA61FCA16, 288186200, 2015-08/04T11:02:52, 0003BFFDD4C2148C kupnju 297833400, 2015-08/04T11:02:50, 0003BFFDD4C2118D kupnju 297833300, 2015-08/04T11:02:40, 00030000D16C4237 za kupnju 297833300, 2015-08/04T11:02:37, 0003BFFDD4C20B63 kupnju 297833400, 2015-08/04T11:02:12, 00037FFEC8567FB8 kupnju 297833400, 2015-08/04T11:02:04, za kupnju

Obratite pozornost na to jesu li prva tri elemenata obavezno. Vrsta događaja nije obavezno. Pročitajte članak [Korištenje oblik](http://go.microsoft.com/fwlink/?LinkID=760712) dodatne informacije o ovoj temi.

 > **Koliko je podataka potrebno?**
 <p>
Dobro je zaista ovisi o podataka o korištenju sam. Sustav Pratit će kada korisnici kupite različite stavke. Za neke izgradi kao što su FBT je važno je znati koje se nabavljaju iste transakcije. (Ne možemo nazovite *zajednički pojavljivanja*). Dobar pravilo palca je za većinu stavki u 20 transakcije ili više njih, pa ako imate 10 000 stavki u katalogu, bi preporučujemo da imate barem 20 puta taj broj transakcije ili spremate 200,000 transakcije.
I opet, to je pravilo palca. Morat ćete Poigrajte se sa svojim podacima.
</p>

<a name="Ex1Task3"></a>
####Zadatak 3 – stvaranje modela preporuke####

Sad kad imate račun, a imate podatke, stvaranje prvi će se model.

U ovom ćete zadatku će koristiti uzorka aplikacija za izradu prvi će se model.

1. Najprije svih trebali imati na umu [Preporuke API Reference](http://go.microsoft.com/fwlink/?LinkId=759348).

1. Preuzmite [Aplikaciju uzorak](http://go.microsoft.com/fwlink/?LinkID=759344) u lokalnu mapu.

1. Otvorite u Visual Studio **RecommendationsSample.sln** rješenja koja se nalazi u mapi **C#** .

1. Otvorite datoteku **SampleApp.cs** . Napomena da sljedeće korake u datoteci:
 + Stvaranje modela
 + Dodavanje datoteke kataloga
 + Dodavanje korištenje datoteke
 + Pokretanje Sastavi modela
 + Dobiti preporuku na temelju par stavke
<p></p>

1. Zamijenite vrijednost polja **AccountKey** tipku od 1 zadatak.

1. Prolaziti kroz rješenje, a vidjet ćete kako se model stvara.

1. Pokušajte zamjene kataloga i korištenje datoteke da biste stvorili novi model za Microsoft Store ili za preporuke book preuzeti. Morat ćete promijeniti naziv modela, a stavke za koje je zahtjev preporuke.

1. Pri stvaranju model uzeti u obzir **ID modela** će je potrebno prilikom traženja preporuke u vašem radnom okruženju.

>  Saznajte više o sastavljanje vrste te upute za procjenu kvalitete izgradi [ovdje](cognitive-services-recommendations-buildtypes.md).

<a name="Ex1Task4"></a>
### <a name="putting-your-model-in-production"></a>Stavljanje modela u radnog! ###

Sad kad znate kako stvoriti model i zauzeti preporuke, sljedeći je korak staviti u proizvodnje na web-mjestu, mobilne aplikacije ili integrirati u sustavu CRM ili ERP.
Pogrešno, svaki od tih implementacije bio drukčiji. Budući da su API preporuke zatražene kao web-usluge, ćete moći jednostavno nazovite ga iz bilo kojeg od tih različitim okruženjima.

**Važne:**  Ako želite da biste prikazali preporuke putem javne klijentskog (na primjer, vaše ecommerce web-mjesto), potrebno je stvoriti proxy poslužitelj za preporuke. To je važno da bi se ključ za API je izložen vanjskim entiteti (potencijalno nepouzdanog).

Evo nekoliko savjeta od mjesta gdje možete koristiti preporuke:

### <a name="product-page"></a>Stranica proizvoda

**Preporuke za stavku**
<p>Ako model put je uvježbavan na kupnju podacima, će omogućiti klijenta s *ciljem otkrivanja proizvoda koji su vjerojatno biti zanimljive osobama koje ste kupili stavke izvora*.</p>
<p>Ako model je put uvježbavan na podatke, kliknite omogućuje kupac da biste *otkrili proizvoda koji su vjerojatno će biti posjećena osobe koje ste posjetili izvorna stavka*. Ove vrste modela može vratiti slične stavke.</p>

**Često kupili zajedno preporuke**
<p>A često kupili zajedno Sastavi nije obučavanju članova, da biste mogli primati skupova stavki vjerojatno će biti kupili zajedno s tu stavku.</p>

### <a name="check-out-page"></a>Odjava stranice

**Preporuke za stavku**
<p>Preporuke model može potrajati kao unos popis stavki. Da bi se sve stavke u košaricu može proći kao ulaz preporuke.
U ovom slučaju modela dat će preporuke koje zanimaju navedene sve stavke u košaricu.
</p>

### <a name="landing-page"></a>Odredišna stranica

**Preporuke za korisnika**
<p>
Preporuke modela mogu preuzeti u obliku unesite id korisnika.  Pružanje personalizirane preporuke za korisnika koji je naveden to će koristiti povijest transakcije taj korisnik.
</p>

Pogledajte [Početak dokumentaciju za preporuke stavke](http://go.microsoft.com/fwlink/?LinkID=760719).

<a name="Ex1Task6"></a>
### <a name="whats-next"></a>Što je sljedeće?
Čestitamo ako ste je to daleko! Da biste saznali više možete posjetiti dovršeno [Preporuke API Reference](http://go.microsoft.com/fwlink/?LinkId=759348) ako imate pitanja, ne hesitate da se obratite nam se namlapi@microsoft.com
