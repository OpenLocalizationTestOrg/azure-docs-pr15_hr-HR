<properties
   pageTitle="Migriranje podataka u SQL Data Warehouse | Microsoft Azure"
   description="Savjeti za migriranju podataka Azure SQL Data Warehouse za razvoj rješenja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="migrate-your-data"></a>Migracija podataka
Podatke možete premjestiti iz različitih izvora u SQL Data Warehouse pomoću alata za različite.  Kopiraj ADF, SSIS i bcp sve se poslužite da biste postigli taj cilj. Međutim, kao količinu podataka povećava trebali biste razmisliti o raščlaniti postupak migracije podataka u korake. To affords prilike da biste optimizirali svakog koraka za performanse i za resilience da biste bili sigurni migracije izglađenim podataka.

U ovom se članku najprije opisuju scenariji jednostavne migracije ADF Kopiraj, SSIS i bcp. Zatim slajd izgledati malo dublju u način migracije mogu se optimizirati.

## <a name="azure-data-factory-adf-copy"></a>Azure kopiju podataka tvorničke (ADF)
[Kopiraj ADF][] je dio [Tvorničke Azure podataka][]. Kopiraj ADF možete koristiti za izvoz podataka u plošnu datoteke residing na lokalno spremište na udaljenom paušalni datoteke unutar blobova platforme Azure ili izravno u SQL Data Warehouse.

Ako podatke koji se pokreće u paušalni datoteke, a zatim će najprije morate prenijeti blobova platforme Azure prostora za pohranu prije započinjanja opterećenjem u SQL Data Warehouse. Kada se podaci se prenose u spremište blobova platforme Azure možete ponovno koristiti [ADF kopiju][] za slanje podataka u SQL Data Warehouse.

PolyBase nudi mogućnost visokih performansi učitavanja podataka. Međutim, koji znači pomoću dva alata umjesto jednog. Ako je potrebno najbolje performanse, zatim koristite PolyBase. Ako želite da se sučelje za jedan alat (i podaci nisu pretraživanje velikog) ADF je odgovor.

> [AZURE.NOTE] PolyBase zahtijeva podatkovne datoteke u UTF-8. Ovo je zadana ADF Kopiraj kodiranje tako da nema ništa da biste promijenili. To je samo podsjetnik neće se promijeniti zadano ponašanje ADF Kopiraj.

Lakši putem u sljedećem članku za neke odlične [primjere ADF][].

## <a name="integration-services"></a>Servisima za integraciju ##
Integracija Services (SSIS) Napredna i prilagodljivo izdvojiti Pretvorba i učitavanje (ETL) alat je koji podržava složenih tijekova rada i transformaciju podataka te nekoliko mogućnosti učitavanja podataka. Pomoću SSIS jednostavno prenijeti podatke za Azure ili kao dio šira migracije.

> [AZURE.NOTE] SSIS možete izvesti UTF-8 bez oznaku redoslijed bajt u datoteci. Da biste konfigurirali to prvo koristite komponentu izvedenih stupaca za pretvaranje podataka znak u tijeku da biste koristili 65001 UTF-8 kodne stranice. Kada pretvorite stupce zapisivanje podataka prilagodnik odredište nehijerarhijskom datotekom jamči da 65001 i odabran je kao kodna stranica za datoteku.

SSIS povezuje s SQL Data Warehouse, baš kao i želite povezati implementacije sustava SQL Server. Međutim, veze morat ćete koristiti za ADO.NET Upravitelja veze. Koje treba i pobrinuti da biste konfigurirali "koristi skupno insert kada su dostupni" postavku Maksimiziraj propusnost. Pogledajte članak [prilagodnik ADO.NET odredište][] da biste saznali više o tom svojstvu

> [AZURE.NOTE] Povezivanje s Azure SQL Data Warehouse pomoću OLEDB nije podržana.

Uz to, postoji uvijek da paket možda se neće krajnji rok za ograničavanje ili mreže problema. Paketi dizajna tako da ih može se nastaviti mjestu pogreške, bez ponavljanje rad završio prije pogreške.

Dodatne informacije potražite u [dokumentaciji SSIS][].

## <a name="bcp"></a>BCP
BCP je naredbenog retka za koji je namijenjen nehijerarhijskom datotekom uvoz i izvoz podataka. Neke transformaciju može obaviti tijekom izvoza podataka. Da biste izvršili jednostavne transformacije pomoću upita možete odabrati i pretvaranje podataka. Nakon izvoza, paušalni datoteke pa je moguće učitati izravno na ciljnom baze podataka SQL Data Warehouse.

> [AZURE.NOTE] Često je dobro Enkapsulacija transformacije koji se koristi tijekom izvoz podataka u prikazu u izvorišnom sustavu. Time se zadržavaju logičku, a postupak je kontrolirani.

Prednosti bcp su:

- Jednostavnosti. naredbe BCP su jednostavni omogućuje stvaranje i izvršavanje
- Postupak može ponovno pokrenuti Učitaj. Jednom izvezene opterećenje može izvršiti bilo koji broj puta.

Ograničenja bcp su:

- BCP funkcionira s pozivu paušalni samo datoteke. Rad s datotekama kao što su xml-a ili JSON
- BCP ne podržava izvoz u UTF-8. To može spriječiti pomoću PolyBase bcp izvezene podatke
- Mogućnosti pretvorbe podataka ograničeni su na samo faze izvoza i su jednostavni u prirode
- BCP nije prilagoditi biti robusne prilikom učitavanja podataka putem Interneta. Bilo koji nestabilnost mreže može uzrokovati pogreška pri učitavanju.
- BCP ovisi o shemi u sustavu ciljna baza podataka prije opterećenja

Dodatne informacije potražite u članku [Korištenje bcp da biste učitali podatke u SQL Data Warehouse][].

## <a name="optimizing-data-migration"></a>Optimiziranje migracije podataka
Budući da se postupak migracije podataka SQLDW mogu učinkovito razdijeliti u tri pojedinačnog koraka:

1. Izvoz izvorišnih podataka
2. Prijenos podataka na Azure
3. Učitavanje u ciljna SQLDW baza podataka

Da biste stvorili postupak migracije robusne, ponovno može pokrenuti i prebacuju koji Maksimizira performanse prilikom svakog koraka možete pojedinačno optimizirati svakog koraka.

## <a name="optimizing-data-load"></a>Optimiziranje učitavanja podataka
Pogled na te obrnutim redoslijedom za trenutak; Najbrži način za učitavanje podataka je putem PolyBase. Optimizacija za učitavanje procesa PolyBase postavlja preduvjeti na prethodne korake da bi se preporučuje se da biste shvatili to određeno. Mogućnosti su sljedeće:

1. Kodiranje podatkovnih datoteka
2. Oblik datoteke s podacima
3. Lokacije podatkovnih datoteka

### <a name="encoding"></a>Kodiranje
PolyBase zahtijeva podatkovnih datoteka koje će biti UTF-8 kodiran. To znači da prilikom izvoza podataka ona mora potvrditi taj zahtjev. Ako podaci sadrže samo osnovni ASCII znakova (koji nije proširen ASCII) pa ove karte izravno na standardnu UTF-8, a ne morate brinuti previše o kodiranje. Međutim, ako podaci sadrže posebne znakove kao što su umlauts, naglaske ili simbola i podataka podržava jezike koji nije latinični zatim morat ćete Izvoz datoteke provjerite jesu li pravilno UTF-8 kodiran.

> [AZURE.NOTE] BCP ne podržava izvoz podataka u UTF-8. Stoga najbolja mogućnost je koristiti servisima za integraciju ili Kopiraj ADF za izvoz podataka. To vrijedi ističu UTF-8 bajt redoslijed oznaku (BOM) nije obavezno u podatkovnoj datoteci.

Sve datoteke kodirana pomoću UTF-16 ćete morati ponovno pisanje ***prethodnog*** prijenos podataka.

### <a name="format-of-data-files"></a>Oblik datoteke s podacima
PolyBase mandates kraj retka fixed \n ili novi redak. Podatkovne datoteke moraju slijediti ovaj standardno. Nepostojanja ograničenja na terminatori niz ili stupac.

Morat ćete odrediti svaki stupac u datoteku kao dio vanjske tablice u PolyBase. Provjerite jesu li svi stupci izvezene potrebne i da se vrsta sukladnosti standarde koje je potrebno.

Pogledajte natrag na [migrirati sheme] članak namijenjen detalja podržane vrste podataka.

### <a name="location-of-data-files"></a>Lokacije podatkovnih datoteka
SQL Data Warehouse koristi PolyBase da biste učitali podatke iz spremišta blobova platforme Azure isključivo. Zbog toga podatke morate najprije prenesene u spremište blobova platforme.

## <a name="optimizing-data-transfer"></a>Optimiziranje prijenos podataka
Jedna od najmanju dijelove migracije podataka je prijenos podataka Azure. Ne samo propusnost mreže mogu problem, ali i pouzdanosti mreži može utjecati spriječiti napredak. Prema zadanim postavkama Migracija podataka na Azure je putem interneta pa vjerojatno prijenos pogrešaka koje su se pojavile vjerojatno razumno. Međutim, te pogreške možda ćete morati ponovno slanje ili u cijelosti ili djelomično podataka.

Srećom imate nekoliko mogućnosti da biste poboljšali brzinu i resilience taj postupak:

### <a name="expressroute"></a>[ExpressRoute][]
Preporučujemo vam da razmislite o korištenju [ExpressRoute][] za brzinu prijenosa. [ExpressRoute][] pruža utvrđene privatne veze Azure tako da se veza otvorite putem javnog Interneta. Ovo je po nisu means obavezno korak. Međutim, ga poboljšat ćete propusnost kada odaberite podatke za Azure iz lokalnog ili funkcijom zajednički mjesto.

Prednosti korištenja [ExpressRoute][] su:

1. Povećana pouzdanost
2. Brzinom mreže
3. LOWER latenciju mreže
4. veća zaštita mreže

[ExpressRoute][] je korisno za broj scenariji; ne samo migracije.

Zanimaju? Dodatne informacije i cijene provjerite potražite u [dokumentaciji ExpressRoute][].

### <a name="azure-import-and-export-service"></a>Azure uvoz i izvoz servisa
Azure uvoz i izvoz servis je namijenjen velikim (GB ++) da biste pretraživanje velikog (TB ++) prijenos podataka u Azure podataka prijenosa. To uključuje pisanja podataka diskova i dostavu ih u Centar za Azure podataka. Sadržaj diska zatim bit će učitana u BLOB polja za pohranu Azure u vaše ime.

Prikaz visoke razine izvoz uvoza je na sljedeći način:

1. Konfiguriranje programa spremnik spremište blobova platforme Azure prima podatke
2. Izvoz podataka u lokalno spremište
2. Kopirajte podatke pomoću [Azure uvoz/izvoz alata] 3,5 inča SATA II III tvrdom diskovnih pogona
3. Stvaranje posao uvoza pomoću Azure uvoz i izvoz uslugu koja omogućuje datoteke dnevnika osnovu [Azure uvoz/izvoz alata]
4. Isporuka se diskova centar za nominated Azure podataka
5. Podataka prenose se u vašem spremnik spremište blobova platforme Azure
6. Učitavanje podatke u SQLDW pomoću PolyBase

### <a name="azcopy-utility"></a>[AZCopy][] utility
Uslužni [AZCopy][] je odličan alat za dohvaćanje podataka koji se prenose u BLOB polja za pohranu Azure. Služi za male (MB ++) da biste vrlo velike (GB ++) podataka prijenosa. [AZCopy] i dizajniran omogućuje dobar prebacuju propusnost kada prijenos podataka Azure i tako da je odličan odabir koraka za prijenos podataka. Jednom prenesene možete učitali podatke pomoću PolyBase u SQL Data Warehouse. AZCopy možete ugraditi i u vašem SSIS paketa pomoću zadatka programa "Izvođenje postupka".

Da biste koristili AZCopy najprije morat ćete preuzeti i instalirati ga. Nema [verzija][] i [verziju za pretpregled][] .

Da biste prenijeli datoteke iz datotečnog sustava će trebati naredbu poput u nastavku:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Sažetak više razine postupak može biti:

1. Konfiguriranje programa Azure spremišta blobova platforme spremnik prima podatke
2. Izvoz podataka u lokalno spremište
3. AZCopy podataka u spremniku spremište blobova platforme Azure
4. Učitavanje podatke u SQL Data Warehouse pomoću PolyBase

Potpuna dokumentaciju dostupna: [AZCopy][].

## <a name="optimizing-data-export"></a>Optimiziranje izvoz podataka
Osim jamči da u skladu s izvozom preduvjeti podsjeća PolyBase možete rješenja da biste optimizirali izvoza podataka za poboljšanje postupka Dodatno.

> [AZURE.NOTE] PolyBase potrebno podataka u obliku UTF-8 je vjerojatno će koristiti bcp obaviti izvoz podataka. BCP ne podržava bilježiti podatkovne datoteke da biste UTF-8. SSIS ili Kopiraj ADF su mnogo bolje prikladniji izvođenje ove vrste izvoz podataka.

### <a name="data-compression"></a>Sažimanje podataka
PolyBase može čitati gzip sažete podatke. Ako se mogućnost da biste spojili podatke gzip datoteke će smanjiti količinu podataka koji se pomiču putem mreže.

### <a name="multiple-files"></a>Više datoteka
Prekidanje gore velike tablice u nekoliko datoteka pomaže da biste poboljšali brzinu izvoza, ali pomaže s re-startability prijenos i mogućnost upravljanja cjelokupan podataka jednom u spremište blobova platforme Azure. Jedna od mnoge bolje značajke PolyBase jest da bit će pročitati sve datoteke u mapi i tretira kao jednu tablicu. Stoga je dobro izdvojiti datoteke za svaku tablicu u svoju mapu.

PolyBase podržava i značajku naziva "rekurzivne mapu prijelaz". Tu značajku možete koristiti da biste dodatno poboljšali tvrtke ili ustanove izvezenih podataka da biste poboljšali upravljanja s podacima.

Da biste saznali više o učitavanja podataka s PolyBase, potražite u članku [Korištenje PolyBase da biste učitali podatke u SQL Data Warehouse][].


## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o migraciji potražite u članku [Migracija rješenje za SQL Data Warehouse][].
Dodatne savjete za razvoj potražite u članku [Pregled razvoj][].

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/storage-use-azcopy.md
[Kopiraj ADF]: ../data-factory/data-factory-data-movement-activities.md 
[Uzorci ADF]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[Pregled razvoj]: sql-data-warehouse-overview-develop.md
[Prenesite rješenje SQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Korištenje bcp da biste učitali podatke u SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[Korištenje PolyBase da biste učitali podatke u SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Tvorničke Azure podataka]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute dokumentacija]: http://azure.microsoft.com/documentation/services/expressroute/

[verzija proizvoda]: http://aka.ms/downloadazcopy/
[Pretpregled izdanja]: http://aka.ms/downloadazcopypr/
[ADO.NET odredište prilagodnika]: https://msdn.microsoft.com/library/bb934041.aspx
[Dokumentacija za SSIS]: https://msdn.microsoft.com/library/ms141026.aspx
