<properties 
    pageTitle="SQL sintaksa i SQL upit za DocumentDB | Microsoft Azure" 
    description="Saznajte više o SQL sintaksa i koncepata baze podataka SQL upite za DocumentDB, NoSQL baze podataka. SQL može koristiti kao jezik upita JSON u DocumentDB." 
    keywords="SQL sintaksa, sql upita, sql upita, json jezik upita, koncepata baze podataka i funkcije zbrajanja sql upita"
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="sql-query-and-sql-syntax-in-documentdb"></a>SQL upit i SQL sintaksa u DocumentDB
Microsoft Azure DocumentDB podržava upite dokumenata pomoću SQL (Structured Query Language) kao JSON jezika za upite. DocumentDB je doista sheme bez. By virtue of njegov izvršenja podatkovni model JSON izravno u modul baze podataka omogućuje automatsko indeksiranje JSON dokumenata bez eksplicitnih sheme i stvaranja sekundarnog indeksa. 

Prilikom dizajniranja jezik upita za DocumentDB ćemo ima dvije ciljeve na umu:

-   Umjesto inventing novi jezik upita JSON, ne možemo željeli podržava SQL. SQL je jedan od jezika upita poznate i popularne. DocumentDB SQL nudi formalno model programiranja obogaćenog upita putem JSON dokumenata.
-   Kao JSON dokument baze podataka može izvršavanja JavaScript izravno u modul baze podataka, ne možemo htjeli koristiti model programiranja, JavaScript kao temelj za naše jezik upita. DocumentDB SQL je s korijenom u JavaScript, Vrsta sustava, procjenu izraza i funkcije poziva. Ovaj u uključivanje omogućuje prirodnim model programiranja za relacijske projekcija, hijerarhijsku navigaciju preko JSON dokumente, prošao na samostalnom spojevi, prostorno upita i poziva korisnički definirane funkcije (UDF-ove) napisali u potpunosti JavaScript među druge značajke. 

Ne možemo smatrate da ove mogućnosti ključ za smanjivanje friction između aplikacija i baze podataka te presudne za razvojne inženjere produktivnost.

Preporučujemo da uvodna sljedeće videozapisu gdje Aravind Ramachandran prikazuje DocumentDB, mogućnosti za upite, i tako što ste posjetili naše [Playground upita](http://www.documentdb.com/sql/demo), gdje možete isprobati DocumentDB i izvoditi SQL upite na našem skup podataka.

> [AZURE.VIDEO dataexposedqueryingdocumentdb]

Zatim, vratite se u ovom se članku gdje ćemo ćete počinju vodič za SQL upit koji će vas kroz nekoliko jednostavnih JSON dokumenata i SQL naredbe.

## <a name="getting-started-with-sql-commands-in-documentdb"></a>Uvod u SQL naredbi u DocumentDB
Da biste vidjeli DocumentDB SQL na poslu, recimo da počinju s nekoliko jednostavnih JSON dokumenata i voditi kroz neke jednostavne upite odabiranja ga. Razmislite o te dvije JSON dokumente o dva linije. Imajte na umu da s DocumentDB, ne možemo ne morate stvarati sheme ni sekundarne indeksi izričito. Jednostavno moramo da biste umetnuli JSON dokumenata za zbirku DocumentDB i naknadno upita. Ovdje imamo jednostavne JSON dokumenta za Andersen obitelji, elemenata nadređenih, djece (i njihov kućni LJUBIMCI), adresa i registracija podaci. Dokument sadrži nizove, brojeve, Booleove vrijednosti, polja i ugniježđene svojstva. 

**Dokument**  

    {
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }


Evo drugom dokumentu s jednog Neupadljiva razlika – `givenName` i `familyName` koriste se umjesto `firstName` i `lastName`.

**Dokument**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                 "givenName": "Lisa", 
                 "gender": "female", 
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "creationDate": 1431620462,
        "isRegistered": false
    }



Sada Pokušajmo nekoliko upita na temelju tih podataka da biste razumjeli neke ključne aspekte DocumentDB SQL. Na primjer, sljedeći upit će vratiti dokumente koje odgovara polju id `AndersenFamily`. Budući da je u `SELECT *`, izlaz upita je cijeli dokument JSON:

**Upit**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Rezultati**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Sada razmislite o kutije moramo preoblikovati JSON Izlaz u drugi oblik. Ovaj upit projekata novi objekt JSON s dva odabranog polja, naziv i Grad, kada se adresa – grad ima isti naziv kao stanje. U ovom slučaju, "Zagreb, NY" odgovara.

**Upit**   

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Rezultati**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


Sljedeći upit vraća sva imena navedenog djece u liniji čiji je id odgovara `WakefieldFamily` poredane po gradu prebivališta.

**Upit**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Rezultati**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Želimo da biste privukli pažnju na nekoliko vrijedno pažnje aspekte jezik upita DocumentDB primjera smo ste dosad vidjeti:  
 
-   Budući da DocumentDB SQL funkcionira na JSON vrijednosti, bavi ga stabla obliku entiteti umjesto retke i stupce. Zbog toga jezik možete uputiti čvorove stabla na proizvoljne dubine, poput `Node1.Node2.Node3…..Nodem`, slično relacijski SQL koje upućuju na dva dijela referencu `<table>.<column>`.   
-   Structured query jezik funkcionira s sheme manje podataka. Zbog toga sustav vrsta treba dinamički vezana. Isti izraz može dati različitih vrsta na različite dokumente. Rezultat upita nije valjana vrijednost JSON, ali se ne zajamčeno nepromjenjivi sheme.  
-   DocumentDB podržava samo točno JSON dokumenata. To znači da su ograničene radnju samo za vrste JSON vrsta sustava i izrazima. Pogledajte [JSON specifikacija](http://www.json.org/) više pojedinosti.  
-   Zbirka DocumentDB je spremnik sheme slobodno JSON dokumenata. Odnose u entiteti podataka unutar i preko dokumenata u zbirci implicitno snimaju zaustavljanja i ne primarni ključ i vanjski ključ odnosa. To je važno razmjer vrijednosti ističu in light of spojeva intra dokument koji je opisan u nastavku ovog članka.

## <a name="documentdb-indexing"></a>Indeksiranje DocumentDB

Prije nego što smo biste DocumentDB SQL sintaksa, to vrijedi Istraživanje indeksiranja dizajna u DocumentDB. 

Svrha indekse baze podataka je posluživanje upita u svoje raznim obrazaca i oblici s potrošnje minimalne resursa (kao što su procesora i ulaza i izlaza) tijekom koja omogućuje dobar propusnost i Latencija niske. Često za odabir desnog indeks za slanje upita baze podataka potreban je mnogo planiranja i početak. Taj se način predstavlja test za sheme manje gdje podaci ne odgovaraju točno shemu i razvoja hitro baze podataka. 

Zbog toga kada smo dizajniran DocumentDB indeksiranje podsustav, ne možemo postaviti sljedeće ciljeve:

-   Indeksiranje dokumenata bez sheme: indeksiranja podsustav zahtijevaju sve informacije o shemi ili provjerite sve pretpostavke o sheme dokumenata. 

-   Podrška za učinkovitiji, obogaćenog hijerarhijski i relacijskih upita: indeks podržava jezik upita DocumentDB učinkovito, uključujući podršku za hijerarhijski i relacijski projekcija.

-   Podrška za dosljedan upita in face of osigurale trajne količinu zapisivanja: za visoke pisanje propusnost radnih opterećenja s dosljedan upita, indeks ažurira postupno učinkovito i Internetu in the face of osigurale trajne količinu zapisivanja. Ažuriranje indeksa dosljedne je ključnih posluživanje upita na razini dosljednosti u kojoj korisnik konfiguriran servis dokumenta.

-   Podrška za više tenancy: Zadana modela rezervacija temelji radi postizanja resursa preko klijenata, ažuriranja indeksa se izvode u okviru proračuna resursa sustava (CPU-a, memorije i operacije ulaza i izlaza sekundi) dodijeljeno po replike. 

-   Prostor za pohranu učinkovitosti: učinkovitosti trošak indirektni prostora za pohranu na disku indeksa je bounded i predvidljivi. To je presudne jer DocumentDB omogućuje programiranje da biste trošak temelji tradeoffs između indeks indirektni odnosu performanse upita.  

Pogledajte [primjere DocumentDB](https://github.com/Azure/azure-documentdb-net) na MSDN-za primjerima pokazuje kako konfigurirati indeksiranja pravila za zbirku. Pogledajmo sada biste detalje o DocumentDB SQL sintaksa.


## <a name="basics-of-a-documentdb-sql-query"></a>Osnove DocumentDB SQL upita
Svaki upit sastoji se od uvjetu SELECT i neobavezna šalje i uvjeta WHERE po standarda ANSI SQL. Obično za svaki upit izvora u uvjetu FROM je videouređaja. Zatim se primjenjuje filtar u uvjetu WHERE na izvorišnog web-mjesta za dohvaćanje podskup JSON dokumenata. Na kraju, uvjetu SELECT koristi se za project tražene JSON vrijednosti na popisu odaberite.
    
    SELECT [TOP <top_expression>] <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a name="from-clause"></a>IZ uvjet
Na `FROM <from_specification>` uvjet nije obavezno osim ako je izvor filtrirani ili predviđena kasnije u upitu. Svrha ovaj uvjet je da biste odredili na kojem morate raditi na upit izvora podataka. Obično je cijelu zbirku izvorišnog web-mjesta, ali nešto umjesto toga odredite podskup zbirke. 

Upit kao `SELECT * FROM Families` upućuje na to da cijelu zbirku linije izvorišnog web-mjesta na kojem želite Nabrajanje. Posebne identifikator KORIJENSKI može se koristiti za predstavljanje zbirke umjesto korištenja naziv zbirke. Sljedeći popis sadrži pravila koja se primjenjuju po upita:

- Zbirka može biti sukobima, kao što su `SELECT f.id FROM Families AS f` ili jednostavno `SELECT f.id FROM Families f`. Ovdje `f` je ekvivalent `Families`. `AS`Neobavezna ključna riječ za alias je identifikator.

-   Napomena to jedanput sukobima, izvornog izvora ne može se povezati. Na primjer, `SELECT Families.id FROM Families f` je sintaktički neispravna jer više nije moguće razriješiti identifikator "Linije".

-   Sva svojstva koja moraju se pozivati mora biti potpuno kvalificiran. U Izostanak adherence izričite shemu, to je određeno da biste izbjegli sve višeznačnih povezivanja. Stoga `SELECT id FROM Families f` je sintaktički neispravna od svojstvo `id` nije povezana.
    
### <a name="sub-documents"></a>Dodatni dokumenata
Izvorišnog web-mjesta mogu biti lošije i manje podskupu. Ako, primjerice, da biste numeriranje samo podmjesta stabla u svakom dokumentu podređenu korijenski može pa postati izvora, kao što je prikazano u sljedećem primjeru.

**Upit**

    SELECT * 
    FROM Families.children

**Rezultati**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

U gornjem primjeru koristi polja kao izvor, objekt može koristiti i kao izvor, što je što je prikazano u sljedećem primjeru. Bilo koji valjani JSON vrijednost (ne nedefinirana) koje možete pronaći u izvoru smatrat će se za uključivanje u rezultatu upita. Ako nemate neke linije programa `address.state` vrijednost, oni će izuzeti u rezultatu upita.

**Upit**

    SELECT * 
    FROM Families.address.state

**Rezultati**

    [
      "WA", 
      "NY"
    ]


## <a name="where-clause"></a>Uvjet WHERE
Uvjet WHERE (**`WHERE <filter_condition>`**) nije obavezno. Navodi uvjete koji JSON dokumente koje ste dobili od izvorišnog web-mjesta mora zadovoljavati da bi se dio rezultata. Bilo koji dokument JSON moraju rezultirati navedeni uvjeti "TRUE" smatraju rezultata. Uvjet WHERE sloj indeks koristi da bi se odredili apsolutne najmanju podskup izvorni dokumenti mogu biti dio rezultat. 

Sljedeći upit zatraži dokumente koji sadrže naziv svojstva čija je vrijednost `AndersenFamily`. Bilo kojim drugim dokumentom koji imaju svojstvu naziv ili mjesto vrijednost ne odgovara `AndersenFamily` isključen. 

**Upit**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Rezultati**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


U prethodnom primjeru prikazivao jednostavne jednakosti upita. DocumentDB SQL podržava i jedan od brojnih skalarna izraza. Najčešće korišteni su binarni i unarni izraze. Svojstvo reference iz izvorišnog objekta JSON su i valjani izraza. 

Sljedeće operatore binarni trenutno podržava i može se koristiti u upitima kao što je prikazano u sljedećim primjerima:  
<table>
<tr>
<td>Aritmetička</td> 
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitwise</td>    
<td>|, &, ^, <<, >>, >>> (desni shift nula ispuna) </td>
</tr>
<tr>
<td>Logička</td>
<td>PA JE ILI NE</td>
</tr>
<tr>
<td>Usporedba</td> 
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Niz</td> 
<td>|| (spajanje)</td>
</tr>
</table>  

Pogledajmo s nekim upita pomoću binarni operatori.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1
    
    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5
    
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Unarni operatora +,-, ~ nisu podržani su i i može se koristiti unutar upita kao što je prikazano u sljedećem primjeru:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1
    
    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Osim binarne datoteke i unarni Operatori referenci svojstvo i dopušteno. Na primjer, `SELECT * FROM Families f WHERE f.isRegistered` vraća JSON dokument koji sadrži svojstvo `isRegistered` gdje svojstvo vrijednost jednaka na JSON `true` vrijednost. Sve ostale vrijednosti (false, vrijednost null, definirana, `<number>`, `<string>`, `<object>`, `<array>`, etc.) potencijalnih klijenata u izvorišnom dokumentu koja se izuzeti iz rezultata. 

### <a name="equality-and-comparison-operators"></a>Operatori jednakosti i usporedbe
Sljedeća tablica prikazuje rezultat s usporedbom jednakosti u DocumentDB SQL između dvije JSON sve.
<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>OP</strong>
         </td>
         <td valign="top">
            <strong>Nedefinirano</strong>
         </td>
         <td valign="top">
            <strong>Null</strong>
         </td>
         <td valign="top">
            <strong>Booleove vrijednosti</strong>
         </td>
         <td valign="top">
            <strong>Broj</strong>
         </td>
         <td valign="top">
            <strong>Niz</strong>
         </td>
         <td valign="top">
            <strong>Objekt</strong>
         </td>
         <td valign="top">
            <strong>Polja</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Nedefinirano<strong>
         </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Null<strong>
         </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
            <strong>ok</strong>
         </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Booleove vrijednosti<strong>
         </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
            <strong>ok</strong>
         </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Broj<strong>
         </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
            <strong>ok</strong>
         </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Niz<strong>
         </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
            <strong>ok</strong>
         </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Objekt<strong>
         </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
            <strong>ok</strong>
         </td>
         <td valign="top">
Nedefinirano </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Polja<strong>
         </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
Nedefinirano </td>
         <td valign="top">
            <strong>ok</strong>
         </td>
      </tr>
   </tbody>
</table>

Za ostale operatori usporedbe, kao što su >, > =,! =, < i < = sljedeća pravila primjenjuju:   

-   Usporedba različitim vrstama rezultira Nedefinirano.
-   Usporedba između dvaju objekata ili dva polja rezultira Nedefinirano.   

Ako je rezultat skalarni izraz u filtru Nedefinirana, odgovarajući dokument nije sadržan u rezultatu jer ne Nedefinirano logično equate na "true".

### <a name="between-keyword"></a>IZMEĐU ključnih riječi
Da biste izrazili upite odabiranja raspone vrijednosti kao što su u ANSI SQL možete koristiti i između ključnu riječ. IZMEĐU može se koristiti protiv nizove ili brojeve.

Ako, na primjer, ovaj upit vraća svih obitelji dokumenata u kojoj se prvi podređeni klase se između 1-5 (obje uključivo). 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

Za razliku od u ANSI SQL vam može poslužiti između uvjet u uvjetu FROM kao u sljedećem primjeru.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Za vrijeme izvođenja brže upita, imajte na umu da biste stvorili indeksiranja pravila koja koristi vrstu indeks raspona na temelju bilo koji numerički svojstava/putova koji su filtrirani u uvjetu između. 

Glavna razlika između pomoću između u DocumentDB i ANSI SQL je da možete express raspon upite odabiranja svojstva koriste različite vrste – na primjer, možda "klase" biti broj (5) u nekim dokumenata i nizova u drugima ("grade4"). U tim slučajevima, kao što su u JavaScript usporedbe između dvije različite vrste rezultata u "Nedefinirano" i dokument će se preskočiti.

### <a name="logical-and-or-and-not-operators"></a>Logički (AND, OR i ne) operatora
Logički operatori rade na Booleove vrijednosti. Logička istine tablice za sljedeće operatore prikazuju se u tablicama u nastavku.

OR|TRUE|FALSE|Nedefinirano
---|---|---|---
TRUE|TRUE|TRUE|TRUE
FALSE|TRUE|FALSE|Nedefinirano
Nedefinirano|TRUE|Nedefinirano|Nedefinirano

I|TRUE|FALSE|Nedefinirano
---|---|---|---
TRUE|TRUE|FALSE|Nedefinirano
FALSE|FALSE|FALSE|FALSE
Nedefinirano|Nedefinirano|FALSE|Nedefinirano

NE|  |
---|---
TRUE|FALSE
FALSE|TRUE
Nedefinirano|Nedefinirano

### <a name="in-keyword"></a>U ključnu riječ
Ključne riječi u mogu se provjerite podudara li određenu vrijednost bilo koja vrijednost na popisu. Na primjer, ovaj upit vraća sve dokumente obitelj pri čemu id neku od "WakefieldFamily" ili "AndersenFamily". 
 
    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

U ovom se primjeru vraća sve dokumente gdje stanje u kojem je bilo koji od navedenih vrijednosti.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Ternary (?) i operatora spojenog (?)
Operatori Ternary i komponentama može se koristiti za sastavljanje uvjetnih izraza, slično popularne programiranja jezike poput C# i JavaScript. 

Operator Ternary (?) može biti vrlo je korisno kada izgradnje nova svojstva JSON u hodu. Na primjer, sada možete napisati upite za klasifikaciju razine za predmete u Ljudski čitljiv obrazac kao što je početna/posrednik/dodatno kao što je prikazano u nastavku.
 
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Možete ugnijezditi pozive na operator like u upitu u nastavku.
 
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Kao s drugim operatora upita ako nedostaju referentne svojstva Uvjetni izraz u bilo kojem dokumentu ili vrste uspoređuje se razlikuju, zatim te dokumente će se izuzeti u rezultatima upita.

Operator spojenog (?) može se koristiti za učinkovito traženje prisutnosti svojstvo (poznatog definiran) u dokumentu. To je korisno kada upita prema djelomično strukturirani ili podataka koriste različite vrste. Na primjer, ovaj upit vraća u "Prezime" ako postoji ili "Prezime" Ako ne postoji.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a name="quoted-property-accessor"></a>Ponuđeni svojstvo pristupnika
Možete pristupiti i svojstva pomoću operatora Ponuđeni svojstvo `[]`. Na primjer, `SELECT c.grade` i `SELECT c["grade"]` se ne razlikuju. Ova sintaksa koristan je kada morate escape svojstvo koje sadrži razmake, posebne znakove ili će se dogoditi da biste zajednički koristili isti naziv kao SQL ključnu riječ ili rezervirane riječi.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a name="select-clause"></a>Uvjet SELECT
Uvjet SELECT (**`SELECT <select_list>`**) je obavezna i određuje koje će se vrijednosti dohvaća iz upita, jednako kao i u ANSI SQL. Premjestite na faza projekcije, gdje se dohvaćaju vrijednosti navedene JSON i konstruirana novi objekt JSON, za svaki unos proslijeđena premjestite je prosljeđuju se podskup koji filtriran pri vrhu izvorni dokumenti. 

Sljedeći primjer prikazuje uobičajeni upita s ODABIRANJEM. 

**Upit**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Rezultati**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Ugniježđene svojstva
U sljedećem primjeru, ne možemo su ste aktivirali projektor dva ugniježđene svojstva `f.address.state` i `f.address.city`.

**Upit**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Rezultati**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Projekcija podržava JSON izrazi kao što je prikazano u sljedećem primjeru.

**Upit**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Rezultati**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Pogledajmo ulogu `$1` ovdje. Na `SELECT` uvjet potrebne za stvaranje JSON objekta i koristimo jer nema ključ nije naveden, implicitno argument varijabilnih nazive koji počinju sa `$1`. Ako, na primjer, ovaj upit vraća dvije varijable implicitno argument označena `$1` i `$2`.

**Upit**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Rezultati**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Pseudonimu
Sada ćemo proširivanje gornjem primjeru s eksplicitnih pseudonimu vrijednosti. KAKAV jest ključnih riječi za pseudonimu. Imajte na umu da je neobavezno kao što je prikazano dok ste aktivirali projektor druge vrijednosti kao `NameInfo`. 

U slučaju da upit sadrži dva svojstva s istim nazivom, pseudonimu mora koristiti da biste preimenovali jedno ili oboje od svojstava tako da se oni su disambiguated u rezultatu predviđeno.

**Upit**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Rezultati**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Skalarna izraza
Osim reference svojstvo uvjetu SELECT podržava skalarna izraze konstante aritmetički izraze, logički izrazi, itd. Na primjer, Evo jednostavnog upita "Pozdrav svijeta".

**Upit**

    SELECT "Hello World"

**Rezultati**

    [{
      "$1": "Hello World"
    }]


Evo primjera složenije koji koristi skalarni izraz.

**Upit**

    SELECT ((2 + 11 % 7)-2)/3   

**Rezultati**

    [{
      "$1": 1.33333
    }]


U sljedećem primjeru rezultat skalarni izraz je na Booleove vrijednosti.

**Upit**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f 

**Rezultati**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Stvaranje objekta i polja
Drugi ključne značajke programa DocumentDB SQL je stvaranja polja/objekata. U prethodnom primjeru, imajte na umu koju smo stvorili novi objekt JSON. Isto tako, jedan također možete sastaviti polja kao što je prikazano u sljedećim primjerima.

**Upit**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f 

**Rezultati**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a name="value-keyword"></a>VRIJEDNOST ključnih riječi
Ključna riječ **vrijednost** omogućuje da biste se vratili JSON vrijednost. Na primjer, upit koji je prikazano u nastavku vraća na skalar `"Hello World"` umjesto `{$1: "Hello World"}`.

**Upit**

    SELECT VALUE "Hello World"

**Rezultati**

    [
      "Hello World"
    ]


Sljedeći upit vraća vrijednost JSON bez na `"address"` natpisa u rezultatima.

**Upit**

    SELECT VALUE f.address
    FROM Families f 

**Rezultati**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

U sljedećem primjeru proširuje tako da prikazuje kako se vratiti JSON jednostavne vrijednosti (lista razina stabla JSON). 

**Upit**

    SELECT VALUE f.address.state
    FROM Families f 

**Rezultati**

    [
      "WA",
      "NY"
    ]


###<a name="-operator"></a>* Operator
Posebne operator (*) podržana u projekt dokument kao-je. Kada se koristi, mora biti samo Predviđeni polja. Iako upita kao što su `SELECT * FROM Families f` valjana, `SELECT VALUE * FROM Families f ` i `SELECT *, f.id FROM Families f ` nisu valjani.

**Upit**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Rezultati**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

###<a name="top-operator"></a>GORNJI Operator
Da biste ograničili broj vrijednosti iz upita može se koristiti GORNJU ključnih riječi. Pri VRHU se koristi u kombinaciji s uvjet ORDER BY, skup rezultata ograničen je na prvi broj N uređeni vrijednosti; u suprotnom vraća prvi N broj rezultata u Nedefinirano reda. Preporučenim načinom rada u naredbi SELECT uvijek pomoću uvjet ORDER BY s uvjetu TOP. To je jedini način predvidljivije određuju koji su reci utječe VRHA. 


**Upit**

    SELECT TOP 1 * 
    FROM Families f 

**Rezultati**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

VRH može se koristiti s konstantu (kao što je prikazano gore) ili vrijednost varijable korištenju s parametrima upita. Dodatne informacije potražite s parametrima upita u nastavku.

## <a name="order-by-clause"></a>Uvjet ORDER BY
Kao što su u ANSI SQL možete uključiti neobavezna uvjet Order By tijekom postavljanja upita. Uvjet možete uključiti neobavezni argument ASC/DESC za određivanje redoslijeda u kojima mora biti dohvaćeni rezultate. Za detaljnije pogledati Order By, pogledajte [DocumentDB redoslijed po prikazu](documentdb-orderby.md).

Na primjer, Evo upit koji dohvaća linije redoslijedom naziva koje postoji grad.

**Upit**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city
    
**Rezultati**
    
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"   
      }
    ]

I ovdje upit koji dohvaća linije redoslijedom datum stvaranja, koji je pohranjen kao broj koji predstavlja na epoch put i.e, proteklo vrijeme od Sij 1, 1970 u sekundi.

**Upit**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC
    
**Rezultati**
    
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472  
      }
    ]
    
## <a name="advanced-database-concepts-and-sql-queries"></a>Koncepti napredne baze podataka i SQL upita
### <a name="iteration"></a>Broj ponavljanja
Novi konstrukciju dodan je putem ključnih riječi **u** u SQL DocumentDB nudi podršku za iterating putem JSON polja. Izvor iz pruža podršku za iteracije. Započnimo s u sljedećem primjeru:

**Upit**

    SELECT * 
    FROM Families.children

**Rezultati**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Sada Pogledajmo drugi upit koji se izvodi iteracija preko djece u zbirci. Imajte na umu razlike u izlazna polja. U ovom se primjeru dijeli `children` te poravnava rezultata u jednom polju.  

**Upit**

    SELECT * 
    FROM c IN Families.children

**Rezultati**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

To se može dodatno koristiti da biste filtrirali na svaku pojedinu stavku polja, kao što je prikazano u sljedećem primjeru.

**Upit**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Rezultati**  

    [{
      "givenName": "Lisa"
    }]

### <a name="joins"></a>Spaja
U relacijske baze podataka, važno je potrebno da biste se uključili u tablicama. To je logički corollary za dizajniranje normaliziranu sheme. Contrary to, DocumentDB bavi Denormalizirani podatkovnog modela sheme slobodno dokumenata. Ovo je logički ekvivalent u "koja se sama uključivanje".

Vidjet ćete da sintaksa koji podržava jezik je < from_source1 > UKLJUČI < from_source2 > UKLJUČI... Uključivanje < from_sourceN >. Ukupni, vraća skup **N**- redni (n-torke vrijednostima **N** ). Svaki n-torke sadrži vrijednosti koje je stvorio iterating sve zbirke pseudonima putem njihove odgovarajući skupove. Drugim riječima, to je potpuna unakrsni umnožak skupova sudjelovanje u spoja.

Sljedeći primjeri pokazuju kako funkcionira JOIN uvjet. U sljedećem primjeru rezultat je prazno od unakrsni umnožak pojedinog dokumenta iz izvora i prazan skup je prazan.

**Upit**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Rezultati**  

    [{
    }]


U sljedećem primjeru je između korijenska mapa za dokumente i `children` podređenu korijen. Je unakrsni umnožak između dva JSON objekata. Fact djece je je polje nije važeća SPOJA jer smo radite jedan korijena koji je podređenih polja. Dakle rezultat sadrži samo dva rezultata, budući da unakrsni umnožak svaki dokumenta s polja rezultira točno samo jedan dokument.

**Upit**

    SELECT f.id
    FROM Families f
    JOIN f.children
 
**Rezultati**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


Sljedeći primjer prikazuje više običan spoja:

**Upit**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Rezultati**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



Je prvo što biste Imajte na umu da se `from_source` **Uključivanje** uvjet je programa iterator. Da, tijek u tom slučaju je na sljedeći način:  

-   Proširite svaki podređeni element **c** u polju.
-   Primjena unakrsni umnožak s korijenu dokument **f** svaki podređeni element **c** je vide u prvom koraku.
-   Na kraju projekta u korijen **f** naziv svojstva objekta samostalno. 

Prvi dokument (`AndersenFamily`) sadrži samo jedan podređeni element tako da se skup rezultata sadrži samo jedan objekt koji odgovaraju na ovaj dokument. Drugi dokument (`WakefieldFamily`) sadrži dvije djece. Tako, unakrsni umnožak daje zasebni objekt za svako dijete na taj način rezultira dva objekta, jedan za svaki podređeni odgovaraju na ovaj dokument. Imajte na umu da korijenski polja u oba dokumenta bit će istovremeno, kao što biste očekivali u unakrsni umnožak.

Uslužni realni SPOJ je redni obrazac iz unakrsni umnožak u obliku koji se inače teško projekta. Osim toga, kao što smo ćete vidjeti u primjeru u nastavku, nije moguće filtrirati prema kombinaciju n-torke koji omogućuje korisnika odabrali uvjeta ukupni zadovoljeni tako da se redni.

**Upit**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
 
**Rezultati**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



U ovom se primjeru je nastavkom prirodnim u prethodnom primjeru, a izvodi dvostruki spoja. Unakrsni umnožak tako, mogu se prikazati kao sljedeći pseudo-kod.

    for-each(Family f in Families)
    {   
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`sadrži jedan podređeni element koji imaju jedan ljubimac. Tako, unakrsni umnožak rezultira jednog retka (1*1*1) iz ovog obitelj. WakefieldFamily no ima dvije djece, ali samo jedan podređeni "Jesse" ima kućni LJUBIMCI. Jesse kroz ima 2 kućni LJUBIMCI. Dakle unakrsni umnožak rezultira 1*1*2 = 2 redaka iz ovog obitelj.

U sljedećem primjeru ima li dodatnog filtra na `pet`. To uključuje sve redni gdje ime ljubimac nije "Sjene". Obratite pozornost na to da možemo sastavljanje redni iz polja, filtar na bilo kojoj od elemenata n-torke, i project bilo koju kombinaciju elemenata. 

**Upit**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Rezultati**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a name="javascript-integration"></a>Integracija JavaScript
DocumentDB sadrži programiranje model za izvođenje logike aplikacije JavaScript temelji izravno na zbirkama pohranjene procedure i okidača. Time se omogućuje za oba:

-   Mogućnost za web-mjesto visoke performanse transakcijskih CRUD operacije i u okvir za upite odabiranja dokumenata u zbirci by virtue of integraciju sa servisom precizno JavaScript izvođenja izravno u modul baze podataka. 
-   Prirodnim Modeliranje tijek kontrola, varijabilnih određivanje opsega, i dodjela i integracija iznimku rukovanja primitives s transakcije baze podataka. Dodatne informacije o podršci za DocumentDB za integraciju JavaScript pogledajte dokumentaciju programibilnost strani poslužitelja JavaScript.

###<a name="user-defined-functions-udfs"></a>Korisnički definirane funkcije (UDF)
Vrstama već definiran u ovom članku, DocumentDB SQL pruža podršku za korisnički definirane funkcije (UDF). Posebno skalarna UDF podržani su gdje možete za razvojne inženjere prenesite nula ili više argumenata i vraćaju rezultat jedan argument natrag. Svaki od tih argumenata provjeravaju se za pravne JSON vrijednosti.  

Prošireni DocumentDB SQL sintaksa za podršku logike prilagođenih aplikacija pomoću ove korisnički definirane funkcije. Korisnički definiranih funkcija biti registriran u DocumentDB, a zatim se pozivati kao dio SQL upita. Zapravo, na UDF-ove exquisitely dizajnirani treba pozvati upiti. Kao corollary na taj odabir, UDF-ove nemaju pristup Kontekstni objekt koji imaju druge JavaScript vrsta (pohranjene procedure i okidača). Budući da upiti se izvoditi samo za čitanje, oni možete pokrenuti na primarni ili sekundarnog replike. Stoga UDF-ovi su osmišljeni tako da biste pokrenuli na sekundarnog replike za razliku od drugih vrsta JavaScript.

Slijedi primjer kako se na UDF biti registriran na DocumentDB bazu podataka, posebice u odjeljku zbirke dokumenata.

   
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };
       
       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  
                                                                             
U prethodnom primjeru stvara UDF čiji je naziv `REGEX_MATCH`. Prihvati dviju vrijednosti niza JSON `input` i `pattern` i provjere Ako prvi podudaranja s uzorkom naveden u drugi korištenjem funkcije string.match() JavaScript korisnika.


Sada možete koristiti ovaj UDF u upitu s projekcije. Korisnički definiranih funkcija mora biti kvalificirani s prefiksom velika i mala slova "udf." Kada pozvane iz upita. 

>[AZURE.NOTE] Prije no što 3/17/2015 DocumentDB podržani UDF pozive bez "udf". Prefiks kao što su odaberite REGEX_MATCH(). Ovaj pozivanja uzorak zastarjela je.  

**Upit**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Rezultati**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

Na UDF možete koristiti unutar filtra kao što je prikazano u primjeru u nastavku, također pogodni s "udf". prefiks:

**Upit**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Rezultati**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


Zapravo, UDF-ove valjani skalarna izraza koji se, a mogu se koristiti u projekcija i filtri. 

Da biste proširili na potenciju UDF-ove, pogledajmo drugi primjer sa sustava SharePoint:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                switch (city) {
                    case 'seattle':
                        return 520;
                    case 'NY':
                        return 410;
                    case 'Chicago':
                        return 673;
                    default:
                        return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);
    
    
U nastavku je primjer u kojem se exercises na UDF.

**Upit**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f 

**Rezultati**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Kao u prethodnim primjerima demonstracije, UDF-ove power jezika JavaScript integrirati SQL DocumentDB omogućuje obogaćenog sučelja tipkovnicu da biste učinili složene proceduralne, uvjetno logike uz pomoć inbuilt JavaScript izvođenje sposobnosti.

DocumentDB SQL daje argumenti na UDF-ove za svaki dokument u izvoru na trenutnu fazu (WHERE ili uvjetu SELECT) obradu na UDF. Rezultat je umetnut u cjelokupan kanalu izvođenja jednostavno. Ako svojstva naziva tako da na UDF parametara nisu dostupne u vrijednost JSON, parametar smatrati Nedefinirana i zato poziva UDF potpuno preskočeno. Na sličan način ako je rezultat na UDF Nedefinirano, on nije uključen u rezultatu. 

Ukratko, UDF-ovi su odličan Alati da biste učinili složene poslovne logike u sklopu upita.

### <a name="operator-evaluation"></a>Operator procjenu
DocumentDB, crta parallels s JavaScript operatori i njegov semantiku procjenu po virtue su JSON baze podataka. Dok DocumentDB pokuša da biste sačuvali JavaScript semantiku pomoću JSON podršku, u nekim slučajevima deviates operacija izračuna.

U DocumentDB SQL, za razliku od u tradicionalni SQL vrste vrijednosti često nije poznat dok vrijednosti zapravo su dohvaćeni iz baze podataka. Da biste učinkovito izvršavanje upita, većina operatore tretiraju izričite vrsta. 

DocumentDB SQL ne izvođenje IMPLICITNIH pretvorbi, za razliku od JavaScript. Ako, primjerice, kao što su upita `SELECT * FROM Person p WHERE p.Age = 21` odgovara dokumente koji sadrže dob svojstva u programu čija je vrijednost 21. Bilo koji dokument čije je svojstvo dob odgovara niz "21" ili nekog drugog vjerojatno beskonačno varijacije kao što su "021", "21.0", "0021", "00021", itd se ne podudaraju. Time se u suprotnom JavaScript gdje su implicitno prenijeti u brojeve vrijednosti niza (ex na temelju operatora: ==). Ova mogućnost je presudne za učinkovito indeks koji se podudaraju u DocumentDB SQL. 

## <a name="parameterized-sql-queries"></a>S parametrima SQL upita
DocumentDB podržava upiti s parametrima izražen s na poznate @ notacije. S parametrima SQL nudi robusne rukovanja i escaping korisnika za unos, sprječava nenamjerno izlaganja podataka putem SQL unos. 

Na primjer, pisanje upita koji vodi prezime i adresa stanja kao parametar, a zatim izvršite za različite vrijednosti prezime i adresa stanje ovisno o korisničkom unosu.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Ovaj zahtjev zatim može poslati DocumentDB kao slogova parametarski upit JSON kao što je prikazano u nastavku.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Argument na VRH moguće je postaviti pomoću upita s parametrima kao što je prikazano u nastavku.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Vrijednosti parametara može biti bilo koji valjani JSON (nizove, brojevi, Booleove vrijednosti, vrijednost null, čak i polja ili ugnijezditi JSON). Također jer je DocumentDB sheme manje, parametara su ne provjerava prema najvišoj bilo koju vrstu.

##<a name="built-in-functions"></a>Ugrađene funkcije
DocumentDB podržava i broj ugrađene funkcije za uobičajene operacije koje je moguće koristiti unutar upita kao što su korisnički definirane funkcije (UDF-ove).

<table>
<tr>
<td>Matematičkih funkcija</td> 
<td>ABS, CEILING, EXP, FLOOR, ZAPISNIKA, LOG10, POWER, ROUND, znak, SQRT, KVADRATA, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, STUPNJEVA, PI, RADIJANA, SIN i TAN</td>
</tr>
<tr>
<td>Vrsta provjere funkcije</td>    
<td>IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED i IS_PRIMITIVE</td>
</tr>
<tr>
<td>Funkcije niza</td>   
<td>SLIKA, sadrži, ENDSWITH, INDEX_OF, lijevo, DULJINA, DONJEM, funkcije LTRIM, ZAMIJENI, REPLICIRATI, OBRNUTO, desno, RTRIM, STARTSWITH, PODNIZ i GORNJEM</td>
</tr>
<tr>
<td>Funkcije polja</td>    
<td>ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH i ARRAY_SLICE</td>
</tr>
<tr>
<td>Prostorno funkcije</td>  
<td>ST_DISTANCE, ST_WITHIN, ST_ISVALID i ST_ISVALIDDETAILED</td>
</tr>
</table>  

Ako trenutno koristite korisnički definirane funkcije (UDF) za koje ugrađene funkcije sada je dostupan, koristite funkciju odgovarajuće ugrađene kao ona će biti brže da biste pokrenuli i učinkovitiji. 

### <a name="mathematical-functions"></a>Matematičkih funkcija
Matematičke funkcije svaki izračuni, obično na temelju ulazne vrijednosti koje su navedene kao argumenti i vratiti numeričku vrijednost. Ovdje je tablica podržani ugrađene funkcije matematičke.

<table>
<tr>
<td><strong>Korištenje</strong></td>
<td><strong>Opis</strong></td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_abs">ABS (num_expr)</a></td> 
<td>Vraća apsolutnu vrijednost (pozitivno) navedeni numerički izraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ceiling">CEILING (num_expr)</a></td> 
<td>Vraća najmanju vrijednost cijeli broj veći od ili jednako, navedeni numerički izraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_floor">FLOOR (num_expr)</a></td> 
<td>Manja od ili jednaka navedeni numerički izraz vraća najveći cijeli broj.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_exp">EXP (num_expr)</a></td> 
<td>Vraća eksponent navedeni numerički izraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log">LOG (num_expr [, base])</a></td> 
<td>Vraća prirodni logaritam navedeni numerički izraz ili logaritam pomoću navedenoj bazi</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log10">LOG10 (num_expr)</a></td> 
<td>Vraća vrijednost logaritamsku bazu 10 navedeni numerički izraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_round">Zaokruživanje (num_expr)</a></td> 
<td>Vraća brojčanu vrijednost, zaokružen na najbliži cijeli broj.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_trunc">TRUNC (num_expr)</a></td> 
<td>Vraća brojčanu vrijednost odbacuje decimalni dio broja na najbližu vrijednost cijelog broja.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sqrt">SQRT (num_expr)</a></td>   
<td>Vraća drugi korijen od navedenog numerički izraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_square">KVADRAT (num_expr)</a></td>   
<td>Vraća kvadrat navedeni numerički izraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_power">POWER (num_expr, num_expr)</a></td>   
<td>Vraća potenciju navedenu numerički izraz naveden vrijednost.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sign">ZNAK (num_expr)</a></td>   
<td>Vraća predznak vrijednost (-1, 0, 1) navedeni numerički izraz.</td>
</tr>
<tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_acos">ACOS (num_expr)</a></td>   
<td>Vraća u kut izražen u radijanima, čiji je kosinus navedeni numerički izraz; Arkus-kosinus naziva.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_asin">ASIN (num_expr)</a></td>   
<td>Vraća nagib, izražen u radijanima, čiji je sinus navedeni numerički izraz. Ovo se zove i arkus-sinus.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atan">ATAN (num_expr)</a></td>   
<td>Vraća nagib, izražen u radijanima, čiji je tangens navedeni numerički izraz. Ovo se zove i arkus-tangens.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atn2">ATN2 (num_expr)</a></td>   
<td>Vraća kut, izražen u radijanima, između pozitivan osi i ray iz izvor točku (y; x), gdje x i y vrijednosti s dva izraza navedenu vrijednost s pomičnim zarezom.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cos">COS (num_expr)</a></td> 
<td>Vraća trigonometrijske kosinus kuta navedenog u radijanima u navedeni izraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cot">COT (num_expr)</a></td> 
<td>Vraća trigonometrijske kotangens kuta navedenog u radijanima u navedenom numerički izraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_degrees">DEGREES (num_expr)</a></td> 
<td>Vraća odgovarajući kut u stupnjevima za kuta navedenog u radijanima.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_pi">PI)</a></td>   
<td>Vraća vrijednost konstante pi.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_radians">RADIANS (num_expr)</a></td> 
<td>Vraća radijana prilikom unosa numerički izraz u stupnjevima.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sin">SIN (num_expr)</a></td> 
<td>Vraća trigonometrijske sinus kuta navedenog u radijanima u navedeni izraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_tan">TAN (num_expr)</a></td> 
<td>Vraća tangens unos izraza u navedeni izraz.</td>
</tr>

</table> 

Na primjer, sada možete pokrenuti upita kao što je sljedeća:

**Upit**

    SELECT VALUE ABS(-4)

**Rezultati**

    [4]

Glavna razlika između funkcije DocumentDB korisnika u usporedbi s ANSI SQL je su dizajnirani za rad s sheme manje i mješovitih sheme podataka. Ako, na primjer, ako imate otvoren dokument koji svojstvo veličina nedostaje ili je kao što su numerička vrijednost "Nepoznato", a zatim dokument je preskočio, umjesto vraćanja pojavila se pogreška.

### <a name="type-checking-functions"></a>Vrsta provjere funkcije
Vrsta provjere funkcije omogućuju vam provjeru upišite izraz u SQL upitima. Vrsta provjere funkcije se može koristiti da biste odredili vrstu svojstva unutar dokumenata u hodu kada je varijable i nepoznatih. Ovdje je tablica podržana vrsta ugrađene funkcije provjere.

<table>
<tr>
  <td><strong>Korištenje</strong></td>
  <td><strong>Opis</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (izraz)</a></td>
  <td>Vraća Booleov koja označava je li vrstu vrijednosti polja.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (izraz)</a></td>
  <td>Vraća Booleov koja označava je li vrstu vrijednosti na Booleove vrijednosti.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (izraz)</a></td>
  <td>Vraća Booleove vrijednosti je koji označava ako je vrsta vrijednosti null.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (izraz)</a></td>
  <td>Vraća Booleov upućuje ako vrstu vrijednost broja.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (izraz)</a></td>
  <td>Vraća Booleov koja označava je li vrsti vrijednosti JSON objekta.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (izraz)</a></td>
  <td>Vraća Booleov upućuje ako vrstu vrijednost niza.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (izraz)</a></td>
  <td>Vraća Booleove vrijednosti je koji označava Ako svojstvo dodijeljeno vrijednost.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (izraz)</a></td>
  <td>Vraća Booleov ukazujući da je vrstu vrijednosti niza, broj, Booleove vrijednosti ili null.</td>
</tr>

</table>

Pomoću ove funkcije, možete odmah pokrenuti upita kao što je sljedeća:

**Upit**

    SELECT VALUE IS_NUMBER(-4)

**Rezultati**

    [true]

### <a name="string-functions"></a>Funkcije niza
Sljedeće Skalarne funkcije izvođenje operacije na vrijednost unos niza i vratite niza, numeričkih ili Booleova vrijednost. Evo tablice niz ugrađene funkcije.

Korištenje|Opis
---|---
[DULJINA (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length)|Vraća broj znakova navedenim nizom izraza
[SLIKA (str_expr str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)|Vraća niz koji je rezultat Ulančavanje dva ili više vrijednosti niza.
[PODNIZ (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring)|Vraća dio nizovni izraz.
[STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith)|Vraća Booleova koji označava hoće li prvi nizovni izraz završava drugi
[ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith)|Vraća Booleova koji označava hoće li prvi nizovni izraz završava drugi
[SADRŽI (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains)|Vraća Booleova koji označava hoće li prvi nizovni izraz sadrži drugi.
[INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of)|Vraća početnog položaja prvog pojavljivanja drugi nizovni izraz u prvom navedeni nizovni izraz ili -1 Ako niz nije pronađen.
[LEFT (str_expr; num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left)|Vraća lijevi dio niza s određenog broja znakova.
[DESNO (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right)|Vraća desni dio niza s određenog broja znakova.
[Funkcije LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim)|Nizovni izraz koji vraća nakon uklanja početne prazne vrijednosti.
[RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim)|Vraća nizovni izraz nakon rezanja sve praznine.
[LOWER (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower)|Vraća nizovni izraz nakon pretvaranja podataka znak velika slova u mala slova.
[UPPER (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper)|Vraća nizovni izraz nakon pretvaranja podataka malih slova u velika slova.
[ZAMIJENI (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace)|Zamjenjuje sva pojavljivanja vrijednosti navedenim nizom drugu vrijednost niza.
[REPLICIRATI (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replicate)|Vrijednost niza ponavlja određeni broj puta.
[OBRNUTO (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse)|Vraća obrnutim redoslijedom vrijednost niza.

Pomoću ove funkcije, možete odmah pokrenuti upita kao što je sljedeća. Na primjer, možete se vratiti na Prezime velikim slovima na sljedeći način:

**Upit**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Rezultati**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Ili spajanje nizova kao u ovom primjeru:

**Upit**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Rezultati**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Funkcije niza može se koristiti u uvjetu WHERE radi filtriranja rezultata, kao u sljedećem primjeru:

**Upit**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Rezultati**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Funkcije polja
Sljedeće Skalarne funkcije izvođenje operacije na ulazne vrijednosti polja i povratna numerički, vrijednost Booleove vrijednosti ili polju. Slijedi popis polja ugrađene funkcije:

Korištenje|Opis
---|---
[ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length)|Vraća broj elemenata u određenom polju izraza.
[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)|Vraća polja koja je rezultat Ulančavanje dvije ili više vrijednosti polja.
[ARRAY_CONTAINS (arr_expr, izraz)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)|Vraća Booleove vrijednosti je koji označava sadrži li polje određenu vrijednost.
[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)|Vraća dio izraza polja.

Funkcije polja se može koristiti za rukovanje polja unutar JSON. Na primjer, Evo upit koji vraća sve dokumente koje nešto elemenata nadređenih "Robin Wakefield". 

**Upit**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Rezultati**

    [{
      "id": "WakefieldFamily"
    }]

Evo još jednog primjera koji koristi ARRAY_LENGTH da biste dobili broj djece po obitelj.

**Upit**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Rezultati**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Prostorno funkcije

DocumentDB podržava sljedeće ugrađene funkcije Otvori geoprostornim Consortium (OGC) za ispitivanje geoprostornim. Dodatne informacije o geoprostornim podržavaju u DocumentDB, pročitajte članak [Rad s geoprostornim podacima u Azure DocumentDB](documentdb-geospatial.md). 

<table>
<tr>
  <td><strong>Korištenje</strong></td>
  <td><strong>Opis</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Vraća udaljenost između dva izraza točke GeoJSON.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Vraća Booleov izraz koji označava je li se unutar GeoJSON poligona u drugom argumentu točke GeoJSON naveden u prvom argumentu.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Vraća Booleova vrijednost koja označava je li navedena GeoJSON točka ili poligona izraz valjana.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Vraća vrijednost JSON koja sadrži na Booleove vrijednosti vrijednost ako navedeni GeoJSON točka ili poligona izraz nije valjan, a koji nisu valjani, k tome razloga kao vrijednost niza.</td>
</tr>
</table>

Prostorno funkcije se mogu koristiti za izvođenje Blizina querries protiv prostorno podataka. Na primjer, Evo upit koji vraća sve obitelji dokumente koje se nalaze u 30 km na određeno mjesto pomoću ST_DISTANCE ugrađene funkcije. 

**Upit**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Rezultati**

    [{
      "id": "WakefieldFamily"
    }]

Ako uvrstite prostorno indeksiranje u indeksiranja pravilnika, zatim "udaljenost upite" će posluživanje učinkovito pomoću indeksa. Dodatne informacije o prostorno indeksiranje, pogledajte odjeljak u nastavku. Ako još nemate prostorno indeks za navedeni putovi prostorno upite možete izvršiti i dalje navođenjem `x-ms-documentdb-query-enable-scan` zahtjev zaglavlja s Postavi vrijednost na "true". U .NET, to možete učiniti prosljeđivanjem neobavezni argument **FeedOptions** upita [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) postavite na true. 

Da biste provjerili je li točku leži unutar poligona mogu se ST_WITHIN. Najčešće višekutnike koriste se za predstavljanje granice kao poštanskim brojevima, stanje granica ili prirodnim formations. Ponovno uključite li prostorno indeksiranje u indeksiranja pravilnika, zatim "u" upita će posluživanje učinkovito kroz indeksa. 

Argumenti poligona u ST_WITHIN može sadržavati samo jedan Nazovi, odnosno na višekutnike ne smije sadržavati rupe u njima. Provjerite [DocumentDB ograničenja](documentdb-limits.md) za maksimalni broj točaka dopušteni u poligona za ST_WITHIN upit.

**Upit**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Rezultati**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Sličan način nepodudaran vrste funkcionira u upitu DocumentDB ako mjesto vrijednost koja je navedena u neki argument nije oštećen ili nije valjana, a zatim će rezultirati **Nedefinirano** i provjeriti u odnosu dokument da biste se preskočiti u rezultatima upita. Ako je upit ne vraća rezultate, pokrenite ST_ISVALIDDETAILED za ispravljanje pogrešaka Zašto vrsta spatail nije valjana.     

ST_ISVALID i ST_ISVALIDDETAILED možete koristiti da biste provjerili prostorno objekt vrijedi. Ako, na primjer, sljedeći upit provjere valjanosti točku s za vrijeme odsutnosti raspon zemljopisnu širinu vrijednost (-132.8). ST_ISVALID vraća samo Booleova vrijednost, a ST_ISVALIDDETAILED vraća na Booleove vrijednosti i niz koji sadrži razloga zašto smatra koji nisu valjani.

**Upit**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Rezultati**

    [{
      "$1": false
    }]

Ove funkcije mogu se koristiti i da biste provjerili valjanost višekutnike. Na primjer, ovdje koristimo ST_ISVALIDDETAILED da biste provjerili valjanost poligona koji je zatvoren. 

**Upit**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Rezultati**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
Koji se prelama prostorno funkcije i SQL sintaksa za DocumentDB. Sada ćemo pogledajte kako LINQ upita radi te ga interakciju s vidjet ćete da sintaksa smo vidjeli dosad.

## <a name="linq-to-documentdb-sql"></a>LINQ za DocumentDB SQL
LINQ je model programiranja .NET koji izražava izračuni kao upite na strujanja objekata. DocumentDB nudi klijent strani biblioteke sučelje s LINQ po olakšavanja pretvorbe između JSON i .NET objekte i mapiranje iz podskup LINQ upite u DocumentDB upite. 

Na slici u nastavku prikazuje arhitekturu programa potpore LINQ upita pomoću DocumentDB.  Klijenti DocumentDB razvojnim inženjerima možete stvoriti izravno upiti davatelja DocumentDB upita koji se zatim prevodi LINQ upita u upitu DocumentDB **IQueryable** objekt. Upit zatim prosljeđuje DocumentDB poslužitelj za dohvaćanje skup rezultata u obliku JSON. Opseg vraćenih rezultata deserijalizirati su u .NET objekata na klijentskoj strani.

![Arhitektura programa potpore LINQ upita pomoću DocumentDB - SQL sintaksa, JSON jezik upita, koncepata baze podataka i SQL upita][1]
 


### <a name="net-and-json-mapping"></a>.NET i JSON mapiranja
Prirodno je mapiranja između .NET objekte i dokumente JSON - svakog člana polja podataka mapirani JSON objekt, pri čemu je naziv polja mapirana dio "tipke" na objekt a dio "vrijednost" rekurzivno mapirati vrijednost dio objekta. Razmislite o sljedećem primjeru. Stvaranje objekta obitelji možete mapirati JSON dokumenta kao što je prikazano u nastavku. I obratno, JSON dokument je mapirati natrag na .NET objekt.

**C# klase**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };
    
    public struct Parent
    {
        public string familyName;
        public string givenName;
    };
    
    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };
    
    public class Pet
    {
        public string givenName;
    };
    
    public class Address
    {
        public string state;
        public string county;
        public string city;
    };
    
    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>LINQ u SQL prijevod
Davatelj usluga za upit DocumentDB izvodi najbolji trud mapiranje iz upita LINQ u DocumentDB SQL upita. U sljedećim opis pretpostavkom da je čitač sadrži osnovni dobro poznate značajke LINQ.

Najprije za sustav vrsta podržavamo sve JSON jednostavne vrste – numeričke vrste Booleove vrijednosti, niza i null. Podržani su samo ove vrste JSON. Podržani su sljedeći skalarna izrazi.

-   Konstante – sadrži ove konstante vrsta jednostavne podataka prilikom procjene upita.

-   Svojstvo/polja index izrazima – ovi izrazi odnose se na svojstva objekta ili element polja.

        family.Id;
        family.children[0].familyName;
        family.children[0].grade;
        family.children[n].grade; //n is an int variable

-   Aritmetički izrazi – uključuju uobičajene aritmetičke izraze numeričkih i Booleove vrijednosti. Cjelovit popis priručniku specifikacija SQL.

        2 * family.children[0].grade;
        x + y;

-   Nizovni izraz usporedbe - uključuju Usporedba vrijednost niza s nekom vrijednosti konstanta niza.  
 
        mother.familyName == "Smith";
        child.givenName == s; //s is a string variable

-   Objekt/polja stvaranje izraza - ovi izrazi povrata vrsta složenih vrijednost ili anonimne vrsta objekta ili polje poput objekata. Može se ugnijezditi te vrijednosti.

        new Parent { familyName = "Smith", givenName = "Joe" };
        new { first = 1, second = 2 }; //an anonymous type with 2 fields              
        new int[] { 3, child.grade, 5 };

### <a name="list-of-supported-linq-operators"></a>Popis podržanih LINQ operatora
Slijedi popis podržanih LINQ operatora u sklopu DocumentDB .NET SDK davatelju LINQ.

-   **Odaberite**: projekcija prevođenje da biste odabrali SQL uključujući konstrukciju objekta
-   **Gdje**: filtri prevesti u u SQL WHERE, a podržavaju prijevoda između & &, mapa i! Operatori za SQL
-   **SelectMany**: omogućuje unwinding polja u uvjetu SQL JOIN. Može se koristiti za lanac/ugnijezditi izraza za filtriranje na polju elemenata
-   **OrderBy i OrderByDescending**: prevodi da biste ORDER BY uzlazno/silazno:
-   **CompareTo**: prevodi usporedbe raspon. Obično koristi za nizovima jer ne postanu usporediti u .NET
-   **Preuzimanje**: prevodi na VRH SQL za ograničavanje rezultata upita
-   **Matematičke funkcije**: podržava prevođenje s. Asin Abs, a zatim na Acos, neto korisnika, Atan, Ceiling Cos Exp, Floor, zapisnika, Log10, Pow, Round, znak, Sin, Sqrt, Tan, Truncate ekvivalentne SQL ugrađene funkcije.
-   **Funkcije niza**: podržava prevođenje s. Neto, slika, sadrži, EndsWith, IndexOf, broj, ToLower, TrimStart, Zamijeni, obrnuto, TrimEnd, StartsWith, podniz, ToUpper ekvivalentnu SQL ugrađene funkcije.
-   **Funkcija polja**: podržava prevođenje s. Neto, slika, sadrži i broj ekvivalentan SQL ugrađene funkcije.
-   **Funkcija proširenje geoprostornim**: podržava prevođenje s graničnik metode udaljenost unutar IsValid i IsValidDetailed na ekvivalentnu SQL ugrađene funkcije.
-   **Korisnički definirana funkcija proširenje funkcija**: podržava prevođenje s metodu graničnik UserDefinedFunctionProvider.Invoke odgovarajuće korisniku definirana funkcija.
-   **Razno**: podržava prijevod komponentama i uvjetno operatore. Možete prevesti sadrži niz sadrži, ARRAY_CONTAINS ili u SQL ovisno o kontekstu.

### <a name="sql-query-operators"></a>Operatori SQL upita
Evo nekoliko primjera koji pokazuju kako neka standardnih operatora upita LINQ prevedene prema dolje do DocumentDB upite.

#### <a name="select-operator"></a>Odabir operatora
Vidjet ćete da sintaksa je `input.Select(x => f(x))`, pri čemu `f` je skalarni izraz.

**LINQ lambda izraz**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ lambda izraz**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ lambda izraz**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany operator
Vidjet ćete da sintaksa je `input.SelectMany(x => f(x))`, pri čemu `f` je skalarni izraz koji vraća vrstu zbirke.

**LINQ lambda izraz**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Gdje operator
Vidjet ćete da sintaksa je `input.Where(x => f(x))`, pri čemu `f` je skalarni izraz koji vraća Booleova vrijednost.

**LINQ lambda izraz**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ lambda izraz**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Složeni SQL upita
Iznad operatori mogu biti sastavljene za jače upita. Budući da DocumentDB podržava ugniježđene zbirke, slaganja možete se spajaju ili ugniježđene funkcije.

#### <a name="concatenation"></a>Spajanje 

Sintaksa nije `input(.|.SelectMany())(.Select()|.Where())*`. Povezanih upita možete početi s programa neobavezno `SelectMany` query slijedi više `Select` ili `Where` operatore.


**LINQ lambda izraz**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ lambda izraz**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ lambda izraz**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
            
**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ lambda izraz**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Ugnježđivanje

Sintaksa nije `input.SelectMany(x=>x.Q())` gdje se nalazi pitanja na `Select`, `SelectMany`, ili `Where` operator.

U upitu ugniježđene unutarnji upita primjenjuje se na svaki element vanjski zbirke. Jedan važna značajka je unutarnji upita možete uputiti polja elemenata u vanjski zbirke kao što su Interna spajanja.

**LINQ lambda izraz**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ lambda izraz**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));
            
**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ lambda izraz**
            
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a name="executing-sql-queries"></a>Izvođenje SQL upita
DocumentDB izlaže resursi kroz REST API-JA koje možete pozvati tako da možete upućivanje zahtjeva za HTTP/HTTPS bilo koji jezik. Uz to, DocumentDB nudi programiranje biblioteke za popularne jezike poput .NET, Node.js, jezika JavaScript i Python. REST API-JA različite biblioteke sve podrška i postavljanje upita pomoću SQL. .NET SDK podržava LINQ uz SQL upita.

Sljedeći primjeri pokazuju kako stvoriti upita i pošaljite ga s računom DocumentDB baze podataka.

### <a name="rest-api"></a>REST API-JA
DocumentDB nudi programa open model RESTful programiranja putem HTTP-a. Računi za bazu podataka možete dodjeli koristite Azure pretplatu. Model resursa DocumentDB sastoji se od skupa resursa u odjeljku račun u bazi podataka koje je moguće adresirati korištenje logičnog i stabilan URI. Skup resursa se nazivaju se sažetak sadržaja u ovom dokumentu. Račun za baze podataka sastoji se od skupa baze podataka, svaki koja sadrži više zbirki svaki koje u Uključi sadrže dokumenata, UDF-ove i druge vrste resursa.

Model osnovni interakcija sa sljedećim resursima se putem glagoli HTTP GET, STAVI, objavu i brisanje s njihovim standardne tumačenja. Glagolski objavu se koristi za stvaranje novog resursa, za izvođenje pohranjena procedura ili za izdavanje DocumentDB upita. Upiti se uvijek pročitajte operacije s bez strani efektima.

Sljedeći primjeri pokazuju objavu DocumentDB upita u odnosu na zbirci web koji sadrži dvije oglednih dokumenata smo dosad pregleda. Upit sadrži Jednostavni filtar u svojstvu JSON naziv. Imajte na umu korištenje na `x-ms-documentdb-isquery` i vrsta sadržaja: `application/query+json` zaglavlja da biste označili da je postupak upitu.


**Zahtjev**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }
    

**Rezultati**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


Drugi primjer prikazuje složeniji upit koji vraća više rezultata iz spoja.

**Zahtjev**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json
    
    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Rezultati**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Ako rezultate upita ne stane u jednu stranicu rezultata, a zatim REST API-JA vraća token nastavka kroz na `x-ms-continuation-token` zaglavlja odgovora. Klijenti možete paginate rezultate uključivanjem zaglavlje sljedeće rezultate. Broj rezultata po stranici može kontrolirati i putem na `x-ms-max-item-count` broj zaglavlja.

Da biste upravljali pravilima dosljednost podataka za upite, poslužite se `x-ms-consistency-level` zaglavlja kao što je sve zahtjeve za REST API-JA. Dosljednost sesije je potrebna i jeku najnoviji `x-ms-session-token` zaglavlje kolačića u zahtjevu za upit. Imajte na umu da traženih zbirke indeksiranja pravila također možete utjecati dosljednost rezultata upita. Uz zadane postavke pravilnika indeksiranja, za zbirke indeksa uvijek je trenutnim sadržaj dokumenta i rezultata upita odgovarat će dosljednost odabrali za podatke. Ako je indeksiranja pravila Opuštena da biste Lazy, upiti mogu vratiti zastarjele rezultate. Dodatne informacije potražite u priručniku [DocumentDB dosljednost razine] [consistency-levels].

Ako je konfiguriran indeksiranja pravila u zbirci ne podržava navedeni upit, DocumentDB poslužitelj vraća 400 "neispravan zahtjev". Vraća se za raspon upite odabiranja putova konfiguriran za pretraživanja raspršivanje (jednakosti) i putove izričito izuzeti iz indeksiranja. Na `x-ms-documentdb-query-enable-scan` zaglavlja možete navesti da bi upit da biste izvršili skeniranje kada indeksa nije dostupan.

### <a name="c-net-sdk"></a>C# (.NET) SDK
.NET SDK podržava LINQ i SQL upita. Sljedeći primjer pokazuje kako izvoditi upit Jednostavni filtar uvedene ranije u ovom dokumentu.


    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Ovaj primjer uspoređuje dva svojstva testira u svakom dokumentu i koristi anonimno projekcija. 


    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Sljedeći primjer prikazuje spojeva izražen kroz LINQ SelectMany.


    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }
    
    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



Klijent .NET zadanu ponovno izračunava automatski kroz sve stranice rezultata upita u blokovima foreach kao što je prikazano gore. Mogućnosti upita u odjeljku REST API-JA dostupne su i u pomoću .NET SDK na `FeedOptions` i `FeedResponse` klase u način CreateDocumentQuery. Broj stranica može se kontrolirati pomoću na `MaxItemCount` postavku. 

Eksplicitno također možete kontrolirati broja stranica stvaranjem `IDocumentQueryable` pomoću na `IQueryable` objekt, zatim tako da pročitate u` ResponseContinuationToken` vrijednosti i prosljeđivanje ih sigurnosno kao `RequestContinuationToken` u `FeedOptions`. `EnableScanInQuery`moguće je postaviti za omogućivanje preglede kada upit ne može podržati konfigurirani indeksiranja pravila. Particioniranom zbirke, možete koristiti `PartitionKey` za izvođenje upita u odnosu na jednom particija (iako DocumentDB možete automatski izdvojiti to iz tekst upita), i `EnableCrossPartitionQuery` da biste pokrenuli upite koje možda morate pokrenuti s više particija. 

Pogledajte [primjere DocumentDB .NET](https://github.com/Azure/azure-documentdb-net) za više uzoraka koja sadrži upite. 

### <a name="javascript-server-side-api"></a>API poslužiteljsko JavaScript 
DocumentDB sadrži programiranje model za izvođenje logike aplikacije JavaScript temelji izravno na zbirkama pomoću pohranjene procedure i okidača. Logika JavaScript registrirana na razini zbirke možete zatim izdati postupaka baze podataka za operacije na dokumentima određene zbirke. Te operacije su umotan u razine okolne ACID transakcije.

U sljedećem primjeru pokazalo kako se koristi u queryDocuments na poslužitelju JavaScript API da bi upiti s unutrašnje pohranjene procedure i okidača.


    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()
    
        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);
    
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);
    
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a name="aggregate-functions"></a>Funkcije zbrajanja

Ugrađena podrška za funkcije zbrajanja nalazi funkcionira, ali ako vam je potrebna broj ili zbroj funkcionalnost u aplikacijom, možete postići isti rezultat na različite načine.  

Na čitanje put:

- Dohvaćanje podataka i način brojanja lokalno možete izvršiti funkcije zbrajanja. Se preporučuje korištenje projekcije jeftino upita kao što su `SELECT VALUE 1` umjesto cijelog dokumenta kao što su `SELECT * FROM c`. Omogućuje Maksimiziranje broj dokumenata koji se obrađuju na svakoj stranici rezultata, čime ćete izbjegavanje dodatnih spajanja servis ako je potrebno.
- Pohranjena procedura možete koristiti i da biste minimizirali latenciju mreže na koji se ponavljaju trips round. Uzorak pohranjena procedura koja izračunava broj za upit s određenom filtra potražite u članku [Count.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/stored-procedures/Count.js). Pohranjena procedura korisnicima da biste spojili obogaćenog poslovne logike uz način zbrajanja u učinkovit način možete omogućiti.

Na putu pisanje:

- Drugi uobičajeni obrazac je unaprijed aggregate rezultira put "pisanje". Ovo je osobito privlačne kad jedinica "čitanje" zahtjeva za je veće od "pisanje" zahtjeva za. Jednom unaprijed zbroja, rezultati su dostupni s jednom točkom čitanju.  Da biste unaprijed zbrajanja u DocumentDB najbolje je da biste postavili okidača koja se poziva sa svakom "pisanje" i ažurirati metapodatke dokument koji sadrži najnovijih rezultata za upit koji je u tijeku materialized. Na primjer, provjerite pogledajte uzorka [UpdateaMetadata.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/triggers/UpdateMetadata.js) kojom se ažuriraju minSize, od maxSize i totalSize metapodataka dokumenta za zbirku. Uzorak mogu se proširiti da biste ažurirali brojač, sum, itd.

##<a name="references"></a>Reference
1.  [Uvod u Azure DocumentDB][introduction]
2.  [Specifikacija DocumentDB SQL](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3.  [Uzorci DocumentDB .NET](https://github.com/Azure/azure-documentdb-net)
4.  [DocumentDB dosljednosti razine][consistency-levels]
5.  ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6.  JSON [http://json.org/](http://json.org/)
7.  Specifikacija JavaScript [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8.  LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9.  Tehnika procjene upita velikih baza podataka [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. U sustavima paralelni relacijske baze podataka, pritisnite društva IEEE računala, za obradu 1994 upita
11. Tan lu, Ooi, upit obrada u sustavima paralelni relacijske baze podataka, pritisnite društva IEEE računalo, 1994.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, promjena Tomkins: Svinja latinica: ne tako-vanjski jezik za obradu podataka, SIGMOD 2008.
13.     G. Graefe. Okvir Cascades za optimizaciju upita. Eng. IEEE podataka Bull., 18(3): 1995.


[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: documentdb-introduction.md
[consistency-levels]: documentdb-consistency-levels.md
 
