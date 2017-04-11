<properties 
    pageTitle="Kako koristiti bilježenje rezultata profilima u pretraživanju Azure | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka" 
    description="Precizno podesite rangiranje kroz bilježenje rezultata profila u pretraživanju Azure, na glavnom računalu pretraživanja u oblaku na Microsoft Azure pretraživanja." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="how-to-use-scoring-profiles-in-azure-search"></a>Kako koristiti profile bilježenja rezultata pretraživanja Azure

Bilježenja rezultata Profili su značajke Microsoft Azure pretraživanja koje omogućuju izračuna rezultati pretraživanja, određujući način rangiranja stavki na popisu rezultata pretraživanja. Možete shvatiti bilježenje rezultata profili kao način važnosti modela po povećanje stavke koje odgovaraju kriterijima unaprijed definirane. Na primjer, pretpostavimo aplikacija je online hotelski rezervacija web-mjesto sustava. Po povećanje na `location` polja pretraživanja koje uključuju termina kao Seattle rezultirat će veći brojčane pokazatelje za stavke koje imaju Seattle na `location` polja. Imajte na umu da možete imati više od jednog bilježenja rezultata profila, ili ništa uopće, ako je zadani bilježenje rezultata potrebne za svoju aplikaciju.

Da biste lakše Eksperimentirajte s profilima bilježenja rezultata, možete preuzeti ogledne aplikacije koja koristi bilježenja rezultata profila da biste promijenili redoslijed rank rezultata pretraživanja. Uzorak velik ni složen kao alat za učenje je aplikacija konzole – možda nisu vrlo realističan za razvoj aplikacija stvarnog života – ali korisno. 

Primjer aplikacije pokazuje bilježenja rezultata ponašanja pomoću fictional podataka, pod nazivom na `musicstoreindex`. Koristite aplikaciju uzorka olakšava izmjena bilježenja rezultata profili i upita, a zatim pogledajte odmah efekti na redoslijedu ranga kada se izvršava program.

<a id="sub-1"></a>
## <a name="prerequisites"></a>Preduvjeti

Primjer aplikacije napisan C# pomoću Visual Studio 2013. Ako još nemate kopiju Visual Studio, pokušajte besplatne [vizualne 2013 Studio Express edition](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) .

Trebat će vam Azure pretplate i na servis Azure pretraživanja da biste dovršili vodič. Potražite u članku [Stvaranje servisa za pretraživanje na portalu](search-create-service-portal.md) za pomoć pri postavljanju servis.

[AZURE. UKLJUČITI [potreban vam je račun za Azure da biste dovršili ovaj Praktični vodič:](../../includes/free-trial-note.md)]

<a id="sub-2"></a>
## <a name="download-the-sample-application"></a>Preuzimanje oglednih aplikacije

Idite na [Pokazni profila za Azure bilježenje rezultata pretraživanja](https://azuresearchscoringprofiles.codeplex.com/) na codeplex da biste preuzeli aplikacije uzorka opisani u ovom ćete praktičnom vodiču.

Na kartici izvornog koda kliknite **Preuzmi** da biste dobili zip datoteku rješenja. 

 ![][12]

<a id="sub-3"></a>
## <a name="edit-appconfig"></a>Uređivanje app.config

1. Nakon što izdvojite datoteke, otvorite rješenje u Visual Studio da biste uredili konfiguracijskoj datoteci.
1. U pregledniku rješenja, dvokliknite **app.config**. Tu datoteku određuje krajnju točku usluge i `api-key` provjerava autentičnost na temelju zahtjev. Te vrijednosti možete dobiti na portalu klasični.
1. Prijava na [Portal za Azure](https://portal.azure.com).
1. Otvorite nadzorna ploča za servisa za pretraživanje Azure.
1. Kliknite pločicu **Svojstva** da biste kopirali URL servisa
1. Kliknite pločicu **tipke** da biste kopirali u `api-key`.

Kada završite s dodavanjem URL-a i `api-key` postavke aplikacije da biste app.config, trebao bi izgledati ovako:

   ![][11]


<a id="sub-4"></a>
## <a name="explore-the-application"></a>Istražite aplikacije

Gotovo spremni ste za stvaranje i pokretanje aplikacija, ali prije nego što radite, pogledajte JSON datoteke koje se koriste za stvaranje i popunjavanje indeksa.

**Schema.JSON** definira indeks, uključujući bilježenja rezultata profila koji se ističu u ovom pokazni videozapis. Obavijest da definira shemu sva polja koja se koristi u indeksu, uključujući nije moguće pretraživati polja, kao što su `margin`, koje možete koristiti u profilu bilježenja rezultata. Bilježenje rezultata profila sintaksa navedenih u članku [Dodavanje bilježenja rezultata profila za indeks pretraživanja Azure](http://msdn.microsoft.com/library/azure/dn798928.aspx).

**Data1 3.json** sadrži podatke, 246 albuma preko handful od Žanrovi. Podaci su kombinacija stvarni albuma i izvođača informacije s fictional polja kao što su `price` i `margin` koristi za prikaz postupaka za pretraživanje. Podatkovne datoteke u skladu sa indeksa i prenose se na servisu Azure pretraživanja. Kad prenijeti podatke i indeksirati možete izdati upite odabiranja ga.

**Program.CS** izvršava sljedeće postupke:

- Otvara prozor konzole.

- Povezuje se s pretraživanja Azure pomoću usluga URL i `api-key`.

- Briše se `musicstoreindex` ako postoji.

- Stvara novu `musicstoreindex` pomoću datoteke schema.json.

- Popunjava indeksa pomoću podatkovne datoteke.

- Upiti za indeks koristeći četiri upita. Imajte na umu da se bilježenja rezultata profili naveden kao parametra upita. Svi upiti potražite isti termina "najbolje". Prvog upita prikazuje zadani bilježenje rezultata. Preostali tri upita pomoću bilježenja rezultata profila.

<a id="sub-5"></a>
## <a name="build-and-run-the-application"></a>Stvaranje i pokretanje aplikacija

S Internetom ili skupa referenca probleme, stvaranje i pokretanje aplikacije da biste bili sigurni da ne postoje problemi rad odjave. Trebali biste vidjeti aplikacije konzole za otvaranje u pozadini. Sve četiri upite za izvršavanje u nizu bez zaustavljanja. Na mnogim sustavima cijeli program izvršava u odjeljku 15 sekundi. Ako aplikacija konzole uključuje i poruka "dovršeno. Pritisnite enter da biste nastavili", program uspješno dovršena. 

Da biste usporedili upit se pokreće, možete oznaka, kopiranje i lijepljenje rezultata upita s konzole i zalijepiti ih u datoteke programa Excel. 

Sljedeća ilustracija prikazuje rezultate iz na prvi tri upita prema usporednu. Svi upiti koristiti isti pojam za pretraživanje "najbolje", koji se pojavljuje u brojne albuma.

   ![][10]

Prvi upit koristi zadani bilježenje rezultata. Budući da pojam za pretraživanje prikazuje se samo u albuma, a ne kriterij nije naveden, stavke koje se pojavljuju 'najbolje' u naziv albuma vraćaju se redoslijedom kojim servisa za pretraživanje pronalazi ih. 

Drugi upit koristi bilježenja rezultata profila, ali obratite pozornost na to da profil imao utjecaja. Rezultati su jednake onima prvog upita. To je zato bilježenja rezultata profila pojačava polja ('stupca') koja nije germane na upit. Izraz za pretraživanje "najbolje ne postoji u bilo kojem polju 'stupca' bilo kojeg drugog dokumenta. Kada bilježenja rezultata profila nema nikakav učinak, rezultati su isti kao zadani bilježenje rezultata.  

Treći upit je prvi dokaz bilježenje rezultata utjecaj profila. Izraz za pretraživanje i dalje 'najbolje je "tako da radimo s isti skup albuma, ali jer bilježenja rezultata profila pruža dodatne kriterije koji se pojačava"ocjena"i 'Prezime ažurirane', neke stavke su propelled višu na popisu.

Sljedeći ilustracija prikazuje četvrti i posljednji upita boosted po "Marža". Polje "Marža" se ne mogu pretraživati i ne mogu se vratiti u rezultatima pretraživanja. Vrijednost "Marža" ručno je dodana u proračunsku tablicu da biste lakše ilustracija točke stavke veće margine prikazuju se viši položaj na popisu rezultata pretraživanja. 

   ![][9]

Sad kad ste ste experimented s profilima bilježenja rezultata, pokušajte promjena program koristi sintaksu upita za različite, bilježenje rezultata profila ili bogatiji podataka. Veze u sljedećem odjeljku navedite dodatne podatke.

<a id="next-steps"></a>
## <a name="next-steps"></a>Daljnji koraci

Saznajte više o bilježenje rezultata profila. Detalje potražite u članku [Dodavanje bilježenja rezultata profila za indeks pretraživanja Azure](http://msdn.microsoft.com/library/azure/dn798928.aspx) .

Saznajte više o parametara pretraživanja sintaksa i upit. Detalje potražite u članku [Pretraživanje dokumenata (Azure pretraživanje REST API -JA)](http://msdn.microsoft.com/library/azure/dn798927.aspx) .

Morate se korak natrag i dodatne informacije o stvaranje indeksa? [Pogledajte ovaj videozapis](http://channel9.msdn.com/Shows/Cloud+Cover/Cloud-Cover-152-Azure-Search-with-Liam-Cavanagh) da biste shvatili osnove.

<!--Anchors-->
[Prerequisites]: #sub-1
[Download the sample application]: #sub-2
[Edit app.config]: #sub-3
[Explore the application]: #sub-4
[Build and run the application]: #sub-5
[Next steps]: #next-steps

<!--Image references-->
[12]: ./media/search-get-started-scoring-profiles/AzureSearch_CodeplexDownload.PNG
[11]: ./media/search-get-started-scoring-profiles/AzureSearch_Scoring_AppConfig.PNG
[10]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX1.PNG
[9]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX2.PNG 