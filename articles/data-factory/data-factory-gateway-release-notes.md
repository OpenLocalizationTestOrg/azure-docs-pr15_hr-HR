<properties 
    pageTitle="Napomene za pristupnik za upravljanje podacima | Tvorničke Azure podataka" 
    description="Podaci pristupnik za upravljanje tory napomene" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="spelluru"/>

# <a name="release-notes-for-data-management-gateway"></a>Napomene za pristupnik za upravljanje podacima
Jedna od izazove za integraciju Moderna podataka je jednostavno premještanje podataka i s lokalnim na cloud. Tvorničke podataka omogućuje Ta integracija objedinjenog s pristupnik za upravljanje podacima koji je agent koje možete instalirati lokalni da biste omogućili hibridnog podataka premještanje.

Potražite u sljedećim člancima detaljne informacije o pristupnik za upravljanje podacima i kako je koristiti: 

- [Pristupnik za upravljanje podacima](data-factory-data-management-gateway.md)
- [Premještanje podataka s lokalnim i pomoću tvorničke Azure podataka u oblaku](data-factory-move-data-between-onprem-and-cloud.md) 

## <a name="current-version-2260721"></a>Trenutna verzija (2.2.6072.1)

- Podržava postavljanje HTTP proxy pristupnika pomoću upravitelja konfiguracije pristupnika. Ako konfiguriran, blobova platforme Azure, Azure tablice, Lake Azure podataka i DB dokument je moguće pristupiti putem HTTP proxy.
- Podržava zaglavlje rukovanje TextFormat pri kopiranju podataka s blobova platforme Azure, spremišta Lake podataka za Azure lokalni datotečni sustav i lokalne HDFS.
- Kopiranje podataka iz dodavanje Blob i stranice Blob uz već podržane Blob bloka podržava.
- Predstavlja novi status pristupnika **Online (Ograničeno)**, što znači da funkciju glavni pristupnika radi osim interaktivne operacija podrška za čarobnjak za kopiranje.
- Poboljšava odličnim registracije pristupnika pomoću ključa za registraciju.

## <a name="earlier-versions"></a>Starije verzije

## <a name="2160401"></a>2.1.6040.1

- Upravljački program za DB2 je sve obuhvaćeno instalacijski paket pristupnika sada. Ne morate instalirati zasebno. 
- Upravljački program za DB2 sada podržava z-OS i DB2 za li (kao / 400) uz platforme već podržane (Linux, Unix i Windows). 
- Podržava korištenje DocumentDB kao izvor ili odredište za lokalne podatke trgovine
- Podržava kopiranje podataka iz/da biste Hladno/tipkovne bloba prostora za pohranu uz račun već podržane općenite namjene prostora za pohranu. 
- Omogućuje povezivanje s lokalnog sustava SQL Server putem pristupnika s ovlastima daljinska prijava.  

## <a name="2060131"></a>2.0.6013.1

- Možete odabrati jezik/kulture tako da koristi pristupnik tijekom instalacije ručno.
- Kada pristupnika radi prema očekivanjima, možete odabrati pristupnika zapisnici o zadnjih sedam dana poslati Microsoftu radi lakšeg otklanjanje problema. Ako pristupnik nije povezan s servisa u oblaku, možete odabrati da biste spremili i arhivirati zapisnike pristupnika.  
- Poboljšanja korisničkog sučelja za upravitelja konfiguracije pristupnika za:
    - Provjerite status pristupnika vidljivije na kartici Polazno.
    - Zato što i pojednostavnjeni kontrole.
- Kopirajte podatke iz spremišta pomoću [kod slobodno Kopiraj alat za pretpregled](data-factory-copy-data-wizard-tutorial.md). Pročitajte članak [Kopirana bez postavljanja kopiranje](data-factory-copy-activity-performance.md#staged-copy) detalje o toj značajci Općenito. 
- Pristupnik za upravljanje podacima na ingress podatke izravno iz lokalnog sustava SQL Server baze podataka možete koristiti u Azure strojnog učenja.
- Poboljšanja performansi
    - Poboljšanje performansi na prikaz sheme/pretpregled protiv SQL Server u alatu za pretpregled kod slobodno Kopiraj.



## <a name="11259531"></a>1.12.5953.1
- Popravci programskih pogrešaka

## <a name="11159181"></a>1.11.5918.1

- Maksimalna veličina zapisnika događaja pristupnika sadrži je veća od 1 MB 40 MB.
- U slučaju da je ponovno pokretanje računala potreban tijekom automatsko ažuriranje pristupnika, prikazat će se dijaloški okvir s upozorenjem. Možete odabrati da biste ponovno pokrenuli desno zatim ili noviji. 
- U slučaju da automatsko ažuriranje ne uspije, pristupnika installer ponovnih pokušaja automatsko ažuriranje triput pri maksimum.
- Poboljšanja performansi
    - Poboljšanja performansi učitavanja velikih tablica iz lokalnog poslužitelja u scenariju kod osloboditi Kopiraj.
- Popravci programskih pogrešaka

## <a name="11058921"></a>1.10.5892.1

- Poboljšanja performansi
- Popravci programskih pogrešaka

## <a name="1958652"></a>1.9.5865.2

- Nula mogućnost za dodir automatsko ažuriranje
- Nova ikona palete pokazateljima stanja pristupnika
- Mogućnost "Ažuriraj odmah" iz klijenta
- Mogućnost da biste postavili vrijeme raspored ažuriranja
- Skriptu PowerShell za prebacivanja automatsko ažuriranje uključivanje/isključivanje
- Podrška za JSON OSNOVNI oblik  
- Poboljšanja performansi
- Popravci programskih pogrešaka

## <a name="1858221"></a>1.8.5822.1

- Poboljšanje sučelje za otklanjanje poteškoća
- Poboljšanja performansi
- Popravci programskih pogrešaka

### <a name="1757951"></a>1.7.5795.1

- Poboljšanja performansi
- Popravci programskih pogrešaka

### <a name="1757641"></a>1.7.5764.1

- Poboljšanja performansi
- Popravci programskih pogrešaka

### <a name="1657351"></a>1.6.5735.1

- Podrška za lokalni izvor primatelj HDFS
- Poboljšanja performansi
- Popravci programskih pogrešaka

### <a name="1656961"></a>1.6.5696.1

- Poboljšanja performansi
- Popravci programskih pogrešaka

### <a name="1656761"></a>1.6.5676.1

- Dijagnostičke alate za podršku na Upravitelj konfiguracije
- Podrška za stupce tablice za izvore tablične podatke za Azure podataka tvorničke
- Podrška za SQL DW za tvorničke Azure podataka
- Podrška Reclusive u BlobSource i FileSource za tvorničke Azure podataka
- Podrška za CopyBehavior – MergeFiles, PreserveHierarchy i FlattenHierarchy u BlobSink i FileSink binarni kopija za tvorničke Azure podataka
- Podržava Kopiraj aktivnosti izvješćivanje o tijeku za tvorničke Azure podataka
- Podrška za izvor povezivanje Provjera valjanosti podataka za tvorničke Azure podataka
- Popravci programskih pogrešaka


### <a name="1656721"></a>1.6.5672.1

- Naziv tablice podrška za ODBC izvora podataka za tvorničke Azure podataka
- Poboljšanja performansi
- Popravci programskih pogrešaka

### <a name="1656581"></a>1.6.5658.1

- Datoteke podrške sita za tvorničke Azure podataka
- Podrška za čuvanje hijerarhije u binarni kopiji za tvorničke Azure podataka
- Kopiraj aktivnosti Idempotency: podrška za tvorničke Azure podataka
- Popravci programskih pogrešaka

### <a name="1656401"></a>1.6.5640.1

- 3 više izvora podataka: podrška za Azure tvorničke podataka (ODBC OData, HDFS)
- Podrška za ponudu znakom analizator csv za Azure podataka tvorničke
- Podrška za sažimanje (BZip2)
- Popravci programskih pogrešaka

### <a name="1556121"></a>1.5.5612.1

- Podržava pet relacijske baze podataka za Azure podataka tvorničke (MySQL, PostgreSQL, DB2, Teradata i Sybase)
- Podrška za sažimanje (Gzip i Deflate)
- Poboljšanja performansi
- Popravci programskih pogrešaka


### <a name="1455491"></a>1.4.5549.1

- Dodajte Oracle podršku za izvor podataka za Azure podataka tvorničke
- Poboljšanja performansi
- Popravci programskih pogrešaka

### <a name="1454921"></a>1.4.5492.1

- Sjedinjene binarni koji podržava servisa Microsoft Azure podataka tvorničke i Office 365 Power BI
- Suzite postupka konfiguriranje korisničkog Sučelja i Registracija
- Azure tvorničke podataka – podrška za izvor podataka za SQL Server Azure Ingress i izlazne

### <a name="1253031"></a>1.2.5303.1

-   Rješavanje problema vremenskog ograničenja za podršku više dugotrajan podatkovne veze za izvor. 
    
### <a name="1155268"></a>1.1.5526.8

- Zahtijeva .NET Framework 4.5.1 kao na preduvjeta tijekom postavljanja.

### <a name="1051442"></a>1.0.5144.2

- Bez promjena koje utječu na tvorničke podataka Azure scenarijima. 
