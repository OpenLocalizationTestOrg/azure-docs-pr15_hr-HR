
<properties
    pageTitle="API DocumentDB Java & SDK | Microsoft Azure"
    description="Saznajte više o Java API i SDK uključujući datumi izdanja, umirovljenje datume i promjene između u svakoj verziji jezika Java SDK DocumentDB."
    services="documentdb"
    documentationCenter="java"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>API-ji DocumentDB i SDK-ovi

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [OSTALE](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-java-api-and-sdk"></a>DocumentDB Java API-JA i SDK

<table>
<tr><td>**Preuzimanje SDK**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>
<tr><td>**Dokumentacija API-JA**</td><td>[Java API referentnu dokumentaciju](http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**Doprinos SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**Početak rada**</td><td>[Početak rada s Java SDK](documentdb-java-application.md)</td></tr>
<tr><td>**Trenutni podržane prilikom izvođenja**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Napomene

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)

  - Podrška za BoundedStaleness dosljednost razinu.
  - Podrška za izravno povezivanje za CRUD postupke za particioniranom zbirke.
  - Ispraviti pogrešku u upita baze podataka pomoću SQL.
  - Ispraviti pogrešku u predmemoriji sesiju gdje se sesiju token možda pogrešno postaviti.

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)

  - Podrška za više particija paralelno upita.
  - Podrška za upite gore/ORDER BY za particioniranom zbirke.
  - Podrška za istaknuti dosljednost.
  - Podrška za ime temelje zahtjeve prilikom korištenja izravno povezivanje.
  - Fiksni da bi ActivityId Ostanite dosljedan preko svih ponovne pokušaje zahtjev.
  - Ispraviti pogreške vezane uz predmemoriju sesiju prilikom stvaranja zbirke s istim nazivom.
  - Dodane poligona i vrste podataka LineString prilikom određivanja zbirke indeksiranje pravilnika o zemlj fencing prostorno upita.
  - FIXED probleme s Java dokumenta za Java 1.8.

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
  - Ispraviti pogrešku u PartitionKeyDefinitionMap predmemoriju jedan particija zbirke i ne donošenje dodatni dohvaćanje particija ključa zahtjeva.
  - Ispraviti pogrešku pokušati ne kada je vrijednost ključa netočan particija dani.

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - Dodaje podršku za više područja baze podataka računa.
  - Podrška za automatsko pokušaja ograničeni zahtjevi za s mogućnostima za prilagodbu Maks pokušaj pokušaja i vrijeme čekanja Maks pokušajte ponovno.  U odjeljku RetryOptions i ConnectionPolicy.getRetryOptions().
  - Zastarjeli IPartitionResolver temelji prilagođenog koda za stvaranje particija. Koristite particioniranom zbirke za veću propusnost i prostor za pohranu.

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- Dodane pokušaj pravila službe za podršku za ograničavanje.  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- Dodane vrijeme live (TTL) podršci za dokumente.

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- Implementirani [particioniranom zbirke](documentdb-partition-data.md) i [korisnički definirana performanse razine](documentdb-performance-levels.md).

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- Ispraviti pogrešku u HashPartitionResolver za generiranje raspršivanje vrijednosti u mali endijan biti usklađene s drugim SDK-ovi.

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- Dodavanje raspršivanje & raspon particija resolvers radi jednostavnijeg sharding aplikacije preko više particija.

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- Implementirati Upsert. Novi načini za upsertXXX dodali podržava značajku Upsert.
- Implementacija ID temelji usmjeravanja. Nema javno promjena API-JA, internog sve promjene.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
- Izdanje preskočeno da bi se prikazala broj verzije u poravnanje s drugim SDK-ovi

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- Podržava geoprostornim indeksa
- Provjeri valjanost svojstvo id za sve resurse. ID-a za resurse ne smije sadržavati ?, /, #, \, znakove ili na kraju razmakom.
- Dodaje novi zaglavlje "indeks transformaciju tijeku" ResourceResponse.

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- Implementira V2 indeksiranja pravila

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- GA SDK

## <a name="release--retirement-dates"></a>Izdanje i njegovo povlačenje iz upotrebe datuma
Microsoft će dati obavijesti barem **12 mjeseci** prije povlačenje programa SDK da bi se glačanje prijelaz na verziju noviju/podržane.

Nove značajke i funkcije i optimizacije samo dodaju se u trenutni SDK, kao što je je preporučujemo da uvijek nadogradnju na najnoviju SDK verziju ranijeg kao što je moguće.

Bilo koji zahtjev za DocumentDB pomoću otpisa SDK će odbio servis.

> [AZURE.WARNING]
Sve verzije Azure DocumentDB SDK Java prije verzije **1.0.0** će biti povučena na **veljača 29, 2016**.

<br/>

| Verzija | Datum izdanja | Datum njegovo povlačenje iz upotrebe
| ---     | ---          | ---
| [1.9.1](#1.9.1) | Listopad 28, 2016 |---
| [1.9.0](#1.9.0) | Listopad 03, 2016 |---
| [1.8.1](#1.8.1) | Lipnja 30, 2016 |---
| [1.8.0](#1.8.0) | Lipnja 14, 2016 |---
| [1.7.1](#1.7.1) | Travanj 30, 2016 |---
| [1.7.0](#1.7.0) | Travanj 27, 2016 |---
| [1.6.0](#1.6.0) | Ožujak 29, 2016 |---
| [1.5.1](#1.5.1) | 31. prosinac 2015. |---
| [1.5.0](#1.5.0) | 04. prosinac 2015. |---
| [1.4.0](#1.4.0) | Listopad 05, 2015. |---
| [1.3.0](#1.3.0) | Listopad 05, 2015. |---
| [1.2.0](#1.2.0) | Kolovoz 05, 2015. |---
| [1.1.0](#1.1.0) | 09. srpanj 2015. |---
| [1.0.1](#1.0.1) | 12. svibanj 2015. |---
| [1.0.0](#1.0.0) | 07. Travanj 2015. |---
| 0.9.5-prelease | 09. Ožu 2015. | Veljača 29, 2016
| 0.9.4-prelease | 17. veljača 2015. | Veljača 29, 2016
| 0.9.3-prelease | 13. siječnju 2015. | Veljača 29, 2016
| 0.9.2-prelease | Prosinac 19, 2014. | Veljača 29, 2016
| 0.9.1-prelease | Prosinac 19, 2014. | Veljača 29, 2016
| 0.9.0-prelease | Prosinac 10, 2014. | Veljača 29, 2016

## <a name="faq"></a>NAJČEŠĆA PITANJA
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Vidi također

Da biste saznali više o DocumentDB, pogledajte stranice servisa [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) .
