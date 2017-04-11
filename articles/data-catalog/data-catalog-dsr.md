<properties
   pageTitle="Azure katalog podataka podržanih izvora podataka | Microsoft Azure"
   description="Specifikacija na trenutno podržanih izvora podataka."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="jstrauss"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-supported-data-sources"></a>Azure katalog podataka podržanih izvora podataka

Korisnici Azure kataloga podataka mogu objavljivati metapodataka pomoću javnog API klika-jednom Registracija alat ili tako da ručno unesete informacije izravno u katalogu podataka web-portal. Sljedeće rešetke ukratko se prikazuju svi izvori koje podržava kataloga danas i objavljivanje mogućnosti za svaki unos.  I navedeni su Alati za vanjske podatke koji se svaki izvor možete pokrenuti iz naših sučelje za portal "Otvori u". Drugi rešetke u članku ima dodatne tehničke specifikacija svaki svojstva veze za izvore podataka.


## <a name="list-of-supported-data-sources"></a>Popis podržanih izvora podataka

<table>

    <tr>
       <td><b>Objekt izvora podataka</b></td>
       <td><b>API-JA</b></td>
       <td><b>Ručni unos</b></td>
       <td><b>Alat za registraciju</b></td>
       <td><b>Alati za otvaranje</b></td>
       <td><b>Bilješke</b></td>
    </tr>

    <tr>
      <td>Imenik Azure podataka Lake spremišta</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Datoteka Azure podataka Lake spremišta</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure spremište blobova platforme</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Prostor za pohranu Azure direktorija</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tablica Azure prostora za pohranu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>

    <tr>
      <td>HDFS direktorija</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HDFS datoteka</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tablicu vrste Hive</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Prikaz grozd</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL tablice</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Prikaz MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tablice baze podataka tvrtke Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Prikaz baze podataka tvrtke Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Ostalo (Generički resursa)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tablica skladištu podataka za SQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Prikaz skladištu podataka za SQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services dimenzije</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services KPI-JA</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services mjera</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tablica programa sustava SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Reporting Services izvješće</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Preglednik</font></td>
      <td><font size=2>Nativnom načinu rada poslužitelji samo. Način rada sustava SharePoint nije podržana.</font></td>
    </tr>

    <tr>
      <td>Tablica sustava SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Prikaz SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tablica Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Prikaz tvrtke Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Prikaz Hana SAP-a</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2>Prikazi izračuna i samo analitički prikazi Atribut prikazi nisu podržani.</font></td>
    </tr>

    <tr>
      <td>Db2 tablice</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Prikaz Db2</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Datotečnom sustavu</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP imenik</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP datoteka</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP izvješća</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP krajnje točke</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP datoteka</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Postavljanje entitet OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData (opis funkcije)</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tablica Postgresql</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Prikaz Postgresql</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Prikaz Hana SAP-a</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td> Objekt Salesforce</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Popis sustava SharePoint </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

</table>

Ako vam je potrebna podrška za dodatne izvore, pošaljite zahtjev značajku pomoću [forum Azure kataloga podataka](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


<br>
<br>
## <a name="data-source-reference-specification"></a>Specifikacija Referenca izvora podataka
> [AZURE.NOTE] Stupac "DSL struktura" u sljedeće tablice samo popise svojstva veze za spremnika svojstava "address" koje koristi katalog podataka Azure (odnosno spremnika svojstava "address" može sadržavati druga svojstva veza izvora podataka koji Azure katalog podataka ne riješi, ali ne koristi.)
<table>
    <tr>
       <td><b>Vrsta izvora</b></td>
       <td><b>Vrsta resursa</b></td>
       <td><b>Vrstama objekta</b></td>
       <td><b>DSL strukture<b></td>
    </tr>
    <tr>
      <td>Spremište Lake podataka za Azure</td>
      <td>Spremnik</td>
      <td>Lake podataka</td>
      <td>
        <font size=2>protokol: webhdfs <br>Provjera autentičnosti: {osnovna oauth} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>Spremište Lake podataka za Azure</td>
      <td>Tablica</td>
      <td>Direktorija, datoteka</td>
      <td>
        <font size=2>protokol: webhdfs <br>Provjera autentičnosti: {osnovna oauth} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>Azure prostora za pohranu</td>
      <td>Spremnik</td>
      <td>Spremnik</td>
      <td>
        <font size=2>protokol: azure blob-ova <br>Provjera autentičnosti: {azure--tipkovnog} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domene <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;računa <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;spremnik</font>
      </td>
    </tr>
    <tr>
      <td>Azure prostora za pohranu</td>
      <td>Tablica</td>
      <td>Blob, direktorija</td>
      <td>
        <font size=2>protokol: azure blob-ova <br>Provjera autentičnosti: {azure--tipkovnog} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domene <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;računa <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;spremnik <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ime</font>
      </td>
    </tr>
    <tr>
      <td>Azure prostora za pohranu</td>
      <td>Spremnik</td>
      <td>Spremnik</td>
      <td>
        <font size=2>protokol: azure tablice <br>Provjera autentičnosti: {azure--tipkovnog} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domene <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;računa</font>
      </td>
    </tr>
    <tr>
      <td>Azure prostora za pohranu</td>
      <td>Tablica</td>
      <td>Tablica</td>
      <td>
        <font size=2>protokol: azure tablice <br>Provjera autentičnosti: {azure--tipkovnog} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domene <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;računa <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ime</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Spremnik</td>
      <td>Virtualna klaster</td>
      <td>
        <font size=2>protokol: cosmos <br>Provjera autentičnosti: {osnovni, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Tablica</td>
      <td>Postavljanje strujanje strujanje prikaz</td>
      <td>
        <font size=2>protokol: cosmos <br>Provjera autentičnosti: {osnovni, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Spremnik</td>
      <td>Web-mjesta</td>
      <td>
        <font size=2>protokol: http <br>Provjera autentičnosti: {ništa, basic, windows, a zatim oauth} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Izvješće</td>
      <td>Izvješća i nadzorne ploče</td>
      <td>
        <font size=2>protokol: http <br>Provjera autentičnosti: {ništa, basic, windows, a zatim oauth} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Spremnik</td>
      <td>Baze podataka</td>
      <td>
        <font size=2>protokol: db2 <br>Provjera autentičnosti: {osnovni, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Tablica</td>
      <td>Prikazu tablice</td>
      <td>
        <font size=2>protokol: db2 <br>Provjera autentičnosti: {osnovna windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;shema</font>
      </td>
    </tr>
    <tr>
      <td>Datotečnom sustavu</td>
      <td>Tablica</td>
      <td>Datoteka</td>
      <td>
        <font size=2>protokol: datoteka <br>Provjera autentičnosti: {ništa, osnovna, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;put</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Tablica</td>
      <td>Direktorija, datoteka</td>
      <td>
        <font size=2>protokol: ftp <br>Provjera autentičnosti: {ništa, osnovna, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Spremnik</td>
      <td>Klaster</td>
      <td>
        <font size=2>protokol: webhdfs <br>Provjera autentičnosti: {osnovna oauth} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Tablica</td>
      <td>Direktorija, datoteka</td>
      <td>
        <font size=2>protokol: webhdfs <br>Provjera autentičnosti: {osnovna oauth} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>Grozd</td>
      <td>Spremnik</td>
      <td>Baze podataka</td>
      <td>
        <font size=2>protokol: grozd <br>Provjera autentičnosti: {username hdinsight, osnovna, nijedno} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Grozd</td>
      <td>Tablica</td>
      <td>Prikazu tablice</td>
      <td>
        <font size=2>protokol: grozd <br>Provjera autentičnosti: {username hdinsight, osnovna, nijedno} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Spremnik</td>
      <td>Web-mjesta</td>
      <td>
        <font size=2>protokol: http <br>Provjera autentičnosti: {ništa, basic, windows, a zatim oauth} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Izvješće</td>
      <td>Izvješća i nadzorne ploče</td>
      <td>
        <font size=2>protokol: http <br>Provjera autentičnosti: {ništa, basic, windows, a zatim oauth} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Tablica</td>
      <td>Završna točka, datoteka</td>
      <td>
        <font size=2>protokol: http <br>Provjera autentičnosti: {ništa, basic, windows, a zatim oauth} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Spremnik</td>
      <td>Baze podataka</td>
      <td>
        <font size=2>protokol: mysql <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Tablica</td>
      <td>Prikazu tablice</td>
      <td>
        <font size=2>protokol: mysql <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Spremnik</td>
      <td>Spremnik entitet</td>
      <td>
        <font size=2>protokol: odata <br>Provjera autentičnosti: {ništa, osnovna, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Tablica</td>
      <td>Postavljanje entitet, funkcija</td>
      <td>
        <font size=2>protokol: odata <br>Provjera autentičnosti: {ništa, osnovna, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;resurs</font>
      </td>
    </tr>
    <tr>
      <td>Baze podataka tvrtke Oracle</td>
      <td>Spremnik</td>
      <td>Baze podataka</td>
      <td>
        <font size=2>protokol: oracle <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka</font>
      </td>
    </tr>
    <tr>
      <td>Baze podataka tvrtke Oracle</td>
      <td>Tablica</td>
      <td>Prikazu tablice</td>
      <td>
        <font size=2>protokol: oracle <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;shema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>Spremnik</td>
      <td>Baze podataka</td>
      <td>
        <font size=2>protokol: postgresql <br>Provjera autentičnosti: {osnovni, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>Tablica</td>
      <td>Prikazu tablice</td>
      <td>
        <font size=2>protokol: postgresql <br>Provjera autentičnosti: {osnovni, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;shema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Spremnik</td>
      <td>Web-mjesta</td>
      <td>
        <font size=2>protokol: http <br>Provjera autentičnosti: {ništa, basic, windows, a zatim oauth} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Izvješće</td>
      <td>Izvješća i nadzorne ploče</td>
      <td>
        <font size=2>protokol: http <br>Provjera autentičnosti: {ništa, basic, windows, a zatim oauth} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Tablica</td>
      <td>Hibridno funkcioniranje podataka</td>
      <td>
        <font size=2>protokol: dodatka power query <br>Provjera autentičnosti: {oauth} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>Salesforce</td>
      <td>Tablica</td>
      <td>Objekt</td>
      <td>
        <font size=2>protokol: salesforce com <br>Provjera autentičnosti: {osnovni, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;klase <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemName</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Spremnik</td>
      <td>Poslužitelj</td>
      <td>
        <font size=2>protokol: sap hana sql <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Tablica</td>
      <td>Prikaz</td>
      <td>
        <font size=2>protokol: sap hana sql <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;shema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Tablica</td>
      <td>Popis</td>
      <td>
        <font size=2>protokol: popis sustava sharepoint <br>Provjera autentičnosti: {osnovni, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Naredba</td>
      <td>Pohranjena procedura</td>
      <td>
        <font size=2>protokol: tds <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;shema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>TableValuedFunction</td>
      <td>Funkcije s tabličnim vrijednostima</td>
      <td>
        <font size=2>protokol: tds <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;shema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Spremnik</td>
      <td>Baze podataka</td>
      <td>
        <font size=2>protokol: tds <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Tablica</td>
      <td>Prikazu tablice</td>
      <td>
        <font size=2>protokol: tds <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;shema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Naredba</td>
      <td>Pohranjena procedura</td>
      <td>
        <font size=2>protokol: tds <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;shema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Funkcije s tabličnim vrijednostima</td>
      <td>
        <font size=2>protokol: tds <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;shema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Spremnik</td>
      <td>Baze podataka</td>
      <td>
        <font size=2>protokol: tds <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Tablica</td>
      <td>Prikazu tablice</td>
      <td>
        <font size=2>protokol: tds <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;shema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services višedimenzionalnog</td>
      <td>Spremnik</td>
      <td>Model</td>
      <td>
        <font size=2>protokol: komponente analysis services <br>Provjera autentičnosti: {windows, osnovna i anonimna, nijedno} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services višedimenzionalnog</td>
      <td>KPI-JA</td>
      <td>KPI-JA</td>
      <td>
        <font size=2>protokol: komponente analysis services <br>Provjera autentičnosti: {windows, osnovna i anonimna, nijedno} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vrstu objekta: {ključnih pokazatelja Uspješnosti}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services višedimenzionalne</td>
      <td>Mjera</td>
      <td>Mjera</td>
      <td>
        <font size=2>protokol: komponente analysis services <br>Provjera autentičnosti: {windows, osnovna i anonimna, nijedno} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vrstu objekta: {mjere}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services višedimenzionalne</td>
      <td>Tablica</td>
      <td>Dimenzije</td>
      <td>
        <font size=2>protokol: komponente analysis services <br>Provjera autentičnosti: {windows, osnovna i anonimna, nijedno} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vrstu objekta: {dimenzije}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services Tabular</td>
      <td>Spremnik</td>
      <td>Model</td>
      <td>
        <font size=2>protokol: komponente analysis services <br>Provjera autentičnosti: {windows, osnovna i anonimna, nijedno} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services Tabular</td>
      <td>KPI-JA</td>
      <td>KPI-JA</td>
      <td>
        <font size=2>protokol: komponente analysis services <br>Provjera autentičnosti: {windows, osnovna i anonimna, nijedno} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vrstu objekta: {ključnih pokazatelja Uspješnosti}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services Tabular</td>
      <td>Mjera</td>
      <td>Mjera</td>
      <td>
        <font size=2>protokol: komponente analysis services <br>Provjera autentičnosti: {windows, osnovna i anonimna, nijedno} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vrstu objekta: {mjere}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services Tabular</td>
      <td>Tablica</td>
      <td>Tablica</td>
      <td>
        <font size=2>protokol: komponente analysis services <br>Provjera autentičnosti: {windows, osnovna i anonimna, nijedno} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vrstu objekta: {tablice}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Spremnik</td>
      <td>Poslužitelj</td>
      <td>
        <font size=2>protokol: komponente reporting services <br>Provjera autentičnosti: {windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verzija: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Izvješće</td>
      <td>Izvješće</td>
      <td>
        <font size=2>protokol: komponente reporting services <br>Provjera autentičnosti: {windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;put <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verzija: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Spremnik</td>
      <td>Baze podataka</td>
      <td>
        <font size=2>protokol: teradata <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Tablica</td>
      <td>Prikazu tablice</td>
      <td>
        <font size=2>protokol: teradata <br>Provjera autentičnosti: {protokol, windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;poslužitelj <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baze podataka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Spremnik</td>
      <td>Model</td>
      <td>
        <font size="2">protokol: mssql strategiju minimalnog preuzimanja <br>Provjera autentičnosti: {windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verzija</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Tablica</td>
      <td>Entitet</td>
      <td>
        <font size="2">protokol: mssql strategiju minimalnog preuzimanja <br>Provjera autentičnosti: {windows} <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-a <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verzija <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;entitet</font>
      </td>
    </tr>
    <tr>
      <td>Ostalo (ne i neka od navedenog)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>protokol: generic resursa <br>adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;idstavke</font>
      </td>
    </tr>
</table>
