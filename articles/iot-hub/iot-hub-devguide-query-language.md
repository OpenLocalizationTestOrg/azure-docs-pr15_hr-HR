<properties
 pageTitle="Vodič za razvojne inženjere - jezik upita | Microsoft Azure"
 description="Azure IoT koncentrator za razvojne inženjere vodič – opis jezik upita koji se koristi za dohvaćanje informacija o twins, postupcima i zadatke iz centra za IoT"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="reference---query-language-for-twins-and-jobs"></a>Referenca - jezika za upite za twins i zadacima

## <a name="overview"></a>Pregled

Koncentrator IoT nudi Napredna SQL nalik jezik za dohvaćanje informacija o [uređaju twins] [ lnk-twins] i [zadacima][lnk-jobs]. U ovom se članku prikazuje:

* Uvod u glavne značajke jezika upita IoT koncentrator, a
* Detaljan opis jezik.

## <a name="getting-started-with-twin-queries"></a>Uvod u upite twin

[Uređaj twins] [ lnk-twins] mogu sadržavati proizvoljne JSON objekte kao oznake i svojstva. Koncentrator IoT omogućuje twins uređaj upita kao jedan JSON dokument koji sadrži sve informacije o twin.
Pretpostavimo, na primjer, da vaše twins IoT koncentrator ima sljedeću strukturu:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

Koncentrator IoT izlaže twins uređaja kao zbirke dokumenata naziva **uređaja**.
Stoga sljedeći upit dohvaća cijeli skup twins uređaj:

        SELECT * FROM devices

> [AZURE.NOTE] [IoT koncentrator SDK-ovi] [ lnk-hub-sdks] podržava broja stranica velike rezultata.

Omogućuje IoT koncentrator za dohvaćanje twins filtriranje s proizvoljne uvjeta. Ako, primjerice,

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

dohvaća podatke u twins oznakom **location.region** postavljanje **nam**.
Logički operatori usporedbe aritmetički podržani su i kao i, npr.

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

dohvaća sve twins koja se nalazi u SAD-konfiguriran za slanje telemetrijskih rjeđe od svake minute. Zbog praktičnosti, preporučuje se i omogućuje korištenje konstanti polja u kombinaciji s **u** i operatora **NU** (ne u). Ako, primjerice,

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

dohvaća sve twins koji je Wi-Fi veze ili ožičenom povezivanje. Pogledajte [uvjet WHERE] [ lnk-query-where] dio cijelog reference mogućnosti filtriranja.

Grupiranje i agregacija podržani su i. Ako, primjerice,

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Vraća broj uređaja za svaki status telemetrijskih konfiguracije.

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

Gornji primjer ilustrira situaciji gdje tri uređaji prijavljenih uspješno konfiguracije, dva su još uvijek se primjenjuju konfiguraciju i jedan prijavio pogrešku.

### <a name="c-example"></a>Primjer C#

Funkcije upita predstavljeni [C# usluga SDK] [ lnk-hub-sdks] u na **RegistryManager** predmete.
Evo primjera jednostavnog upita:

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page) 
            {
                // do work on twin object
            }
        }

Imajte na umu kako će se objekt **upita** instancirati s veličine stranice (do 1000), a zatim se više stranica moguće dohvatiti tako da nazovete metode **GetNextAsTwinAsync** više puta.
Važno Imajte na umu da se objekt upita izlaže višekratnik je **sljedeći\***, ovisno o deserijalizacija mogućnost potrebnih upit, odnosno twin ili posla objekata ili obični Json koja će se koristiti prilikom korištenja projekcija.

### <a name="node-example"></a>Primjer čvor

Funkcije upita predstavljeni [čvor usluga SDK] [ lnk-hub-sdks] u na objekt **registra** .
Evo primjera jednostavnog upita:

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

Imajte na umu kako će se objekt **upita** instancirati s veličine stranice (do 1000), a zatim se više stranica moguće dohvatiti tako da nazovete metode **nextAsTwin** više puta.
Važno Imajte na umu da se objekt upita izlaže višekratnik je **sljedeći\***, ovisno o deserijalizacija mogućnost potrebnih upit, odnosno twin ili posla objekata ili obični Json koja će se koristiti prilikom korištenja projekcija.

### <a name="limitations"></a>Ograničenja

Trenutno projekcija podržani su samo kada koristite zbrajanja, odnosno koje nisu zbrojene upite možete koristiti samo `SELECT *`. Osim toga, zbrajanja podržani su samo u kombinaciji s grupiranja.

## <a name="getting-started-with-jobs-queries"></a>Uvod u upite poslove

[Poslovi] [ lnk-jobs] omogućuje dopušta izvršavanje operacija skupa uređaja. Svaki uređaj twin sadrži podatke poslova koja sudjeluje u zbirci naziva **zadatke**.
Logički,

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                { 
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

Trenutno je zbirku queriable kao **devices.jobs** u IoT koncentrator jezika za upite.

Na primjer, da biste sve zadatke (proteklih i zakazani) koje utječu na jedan uređaj, možete koristiti sljedeći upit:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Imajte na umu kako ovaj upit sadrži status specifične za uređaj (i vjerojatno odgovor Izravni metoda) svakog posla vraća.
Također je moguće filtrirati s proizvoljne Booleova uvjeta na sva svojstva objekata u zbirci **devices.jobs** .
Na primjer, sljedeći upit:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

dohvaća sve dovršene twin ažuriranje zadatke za uređaj **myDeviceId** koje su stvorene nakon rujan 2016.

Također je moguće dohvatiti ishoda po uređaj jedan posla.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Ograničenja
Trenutno ne podržavaju upite na **devices.jobs** :

* Projekcija, odnosno samo `SELECT *` je moguće;
* Uvjeti koji se odnose na uređaju twin osim svojstva zadatka, kao što je prikazano gore;
* Peforming zbrajanja, primjerice zbroj, Prosj, Grupiraj po.

## <a name="basics-of-an-iot-hub-query"></a>Osnove koncentrator IoT upita

Svaki upit IoT koncentrator sastoji se POTVRDITE i iz rečenice i neobavezni WHERE i GROUP BY rečenice. Svaki upit se izvodi u zbirci JSON dokumenata, primjerice twins uređaja. U uvjetu FROM označava zbirke dokumenata da biste se iterated na (**uređaja** ili **devices.jobs**). Nakon toga će se primjenjuje filtar u uvjetu WHERE. U slučaju zbrajanja, rezultati ovaj korak grupiraju se kao naveden u uvjet GROUP BY te za svaku grupu, redak se generira kao što je određeno u uvjetu SELECT.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>IZ uvjet

Uvjet **iz < from_specification >** možete pretpostavlja samo dvije vrijednosti: **šalje uređaji**twins uređaj upita ili **FROM devices.jobs**, upit posla po uređaj detalja.

## <a name="where-clause"></a>Uvjet WHERE

U **gdje < filter_condition >** uvjet nije obavezno. Navodi uvjete koji JSON dokumenata u zbirci FROM mora zadovoljavati da bi se dio rezultata. Bilo koji dokument JSON moraju rezultirati navedeni uvjeti "TRUE" da bi bio uvršten u rezultate.

Dopuštene uvjeta opisana su u odjeljku [izraza i odredbe][lnk-query-expressions].

## <a name="select-clause"></a>Uvjet SELECT

Uvjet SELECT (**Odaberite < select_list >**) je obavezna i određuje koje će se vrijednosti dohvaća iz upita. Određuje JSON vrijednosti koje se koriste za stvaranje novih objekata JSON za svaki element filtrirani (i po želji grupirane) podskup zbirke FROM faza projekcije stvara novi objekt JSON, konstruirana s vrijednostima koje su navedene u uvjetu SELECT.

To je gramatike uvjetu SELECT:

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

gdje **attribute_name** se odnosi na neko svojstvo JSON dokumenta iz zbirke. Primjeri odabir rečenice pronaći ćete u [Uvod u upite twin] [ lnk-query-getstarted] sekciju.

Trenutno odabrano rečenice razlikuje se od **Odaberite \* ** podržani su samo u zbrajanja upite na twins.

## <a name="group-by-clause"></a>Uvjet GROUP BY

Uvjet **GROUP BY < group_specification >** je neobavezan korak koji se izvršavaju nakon navedeni u uvjetu WHERE, a prije projekcije naveden u okviru odaberite filtar. Je grupa dokumente koji se temelje na vrijednost atributa. Te grupe se koriste za generiranje zbroja vrijednosti kao što je navedeno u uvjetu SELECT.

Primjer upita pomoću GROUP BY je:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Formalno sintaksa GROUP BY je:

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

gdje **attribute_name** se odnosi na neko svojstvo JSON dokumenta iz zbirke. 

Trenutno uvjet GROUP BY podržano je samo kada ispitivanje twins.

## <a name="expressions-and-conditions"></a>Izrazi i uvjeta

Visoke razine, *izraz*:

* Procjenjuje instancu vrste JSON (odnosno Booleove vrijednosti, broj, niz, polja ili objekta), a
* Definira rukovanje podatke koji dolaze iz dokumenta JSON uređaja i konstante pomoću ugrađenih operatore i funkcije.

*Uvjeti* su izraza koji se procjenjuje kao Booleove vrijednosti, sve odnosno konstantu drugačije nego Booleova **true** smatra se kao **false** (uključivanje **null**, **definirana**, sve instance objekta ili polja, bilo koji niz znakova i jasno na Booleove **false**).

Sintaksa izraza je:

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant> 
            | <number_constant> 
            | <string_constant> 
            | <array_constant> 

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

pri čemu je:

| Simbol | Definicija |
| -------------- | -----------------|
| attribute_name | Svojstvo dokumenta JSON u zbirci POŠILJATELJ. |
| unary_operator | Bilo koji unarni operator po operatori sekcije. |
| binary_operator | Bilo koji binarni operator po operatori sekcije. |
| decimal_literal | Prikaz koji se izražena u decimalni notacije. |
| hexadecimal_literal | Broj izražen nizom "0 x" nakon čega slijedi niz heksadecimalni znamenke. |
| string_literal | Niz literala su Unicode nizovi predstavlja niz nula ili više Unicode znakova ili nizove escape. Niz literala bit će u jednostrukim navodnicima (apostrof: ') ili dvostruki navodnik (navodnik: "). Dopuštene escapes: `\'`, `\"`, `\\`, `\uXXXX` za Unicode znakova koji definira 4 znamenke u heksadecimalni. |

### <a name="operators"></a>Operatori

Podržane su sljedeće operatore:

| Obitelj | Operatori |
| -------------- | -----------------|
| Aritmetička | +,-,*,/,% |
| Logička | PA JE ILI NE |
| Usporedba |  =,! = <>;, < = > = <> |

## <a name="next-steps"></a>Daljnji koraci

Upute za izvođenje upita u aplikacijama pomoću [IoT koncentrator SDK-ovi][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md