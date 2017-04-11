<properties 
    pageTitle="Podatkovne funkcije na tvorničke i sistemske varijable | Microsoft Azure" 
    description="Daje popis funkcije tvorničke Azure podataka i sistemske varijable" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"
    services="data-factory"
/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---functions-and-system-variables"></a>Azure podataka tvorničke - funkcije i sistemske varijable
Ovaj članak sadrži informacije o funkcijama i varijable podržava tvorničke Azure podataka.
  
## <a name="data-factory-system-variables"></a>Podaci tvorničke sistemske varijable

Naziv varijable | Opis | Opseg objekta | Opseg JSON i korištenje slučajeva
------------- | ----------- | ------------ | ------------------------
WindowStart | Početak vremenski interval za trenutne aktivnosti pokrenuti prozora | aktivnosti | <ol><li>Navedite upite za odabir podataka. Pročitajte članke poveznik navedenih u članku [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md) .</li><li>Prenesite parametara grozd skripte (primjer je prikazan gore).</li>
WindowEnd | Kraj vremenski interval za trenutne aktivnosti pokrenuti prozora | aktivnosti | Isto kao prethodna
SliceStart | Početak vremenski interval za isječkom podataka se proizvodi | aktivnosti<br/>skup podataka | <ol><li>Navedite putevima dinamički mapa i nazivi datoteka dok radite s [Blobova platforme Azure](data-factory-azure-blob-connector.md) i [skupova podataka u datotečnom sustavu](data-factory-onprem-file-system-connector.md).</li><li>Određivanje unosa ovisnosti pomoću funkcije tvorničke podataka u zbirci unosa aktivnosti.</li></ol>
SliceEnd | Kraj vremenski interval za trenutni isječak podatke koji su se proizvodi | aktivnosti<br/>skup podataka | Isto kao prethodna. 

> [AZURE.NOTE] Trenutno tvorničke podataka potreban je da odgovara rasporedu točno naveden u aktivnosti raspored naveden u dostupnosti skupa podataka za izlaz. To znači da WindowStart, WindowEnd i SliceStart i SliceEnd uvijek mapirajte isti vremenskog razdoblja, a jedan izlazni isječak.
 
## <a name="data-factory-functions"></a>Funkcije tvorničke podataka 

Koristite funkcije na tvorničke podataka s gore spomenuti sistemske varijable u svrhu sljedeće:

1.  Određivanje upita odabira podataka (pogledajte članke poveznik pozivaju na nju u članku [Aktivnosti premještanje podataka](data-factory-data-movement-activities.md) .

    Sintaksa za pozivanje funkciju tvorničke podataka nije: ** $$ ** za upite za odabir podataka i druga svojstva u aktivnosti i skupova podataka.  
2. Određivanje unosa ovisnosti pomoću funkcije tvorničke podataka u aktivnosti ulazi zbirke (pogledajte primjer iznad).

    $ nije potreban za određivanje ovisnosti za unos izraza.   

U sljedeći primjer svojstvo **sqlReaderQuery** u datoteci JSON se dodjeljuje vrijednost koju vraća funkcija **Text.Format** . Ovaj primjer koristi i varijablu sustav pod nazivom **WindowStart**, što predstavlja vrijeme početka aktivnosti pokrenuti prozora.
    
    {
        "Type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
    }

### <a name="functions"></a>Funkcija

U sljedećim tablicama navedeni sve funkcije tvorničke Azure podataka:

Kategorija | Funkcija | Parametri | Opis
-------- | -------- | ---------- | ----------- 
Vrijeme | AddHours(X,Y) | X: datuma i vremena <br/><br/>Y: int | Dodaje Y vremena u određenom vremenskom X. <br/><br/>Primjer: 9/5/2013 12:00:00 PM + 2 sata = 9/5/2013 2:00:00 PM
Vrijeme | AddMinutes(X,Y) | X: datuma i vremena <br/><br/>Y: int | Dodaje Y minuta X.<br/><br/>Primjer: 9/15/2013 12:00:00 PM + 15 minuta = 9/15/2013 12:15:00 PM
Vrijeme | StartOfHour(X) | X: datuma i vremena | Dohvaća vrijeme početka h predstavlja komponentu sat x. <br/><br/>Primjer: StartOfHour 9/15/2013 05:10:23 PM je 9/15/2013 05:00:00 PM
Datum | AddDays(X,Y) | X: datuma i vremena<br/><br/>Y: int | Dodaje Y dana X.<br/><br/>Primjer: 9/15/2013 12:00:00 PM + 2 dana = 9/17/2013 12:00:00 PM
Datum | AddMonths(X,Y) | X: datuma i vremena<br/><br/>Y: int | Dodaje Y mjeseci X.<br/><br/>Primjer: 9/15/2013 12:00:00 PM + 1 mjesec = 10/15/2013 12:00:00 PM 
Datum | AddQuarters(X,Y) | X: datuma i vremena <br/><br/>Y: int | Dodaje Y * 3 mjeseca do X.<br/><br/>Primjer: 9/15/2013 12:00:00 PM + 1 tromjesečje = 12/15/2013 12:00:00 PM
Datum | AddWeeks(X,Y) | X: datuma i vremena<br/><br/>Y: int | Dodaje Y * 7 dana do X<br/><br/>Primjer: 9/15/2013 12:00:00 PM + 1 dan = 9/22/2013 12:00:00 PM
Datum | AddYears(X,Y) | X: datuma i vremena<br/><br/>Y: int | Dodaje Y godina X.<br/><br/>Primjer: 9/15/2013 12:00:00 PM + 1 godine = 9/15/2014 12:00:00 PM
Datum | Day(X) | X: datuma i vremena | Dohvaća komponentu danom x.<br/><br/>Primjer: Dan 9/15/2013 12:00:00 Prikazano je 9. 
Datum | DayOfWeek(X) | X: datuma i vremena | Dohvaća dan tjedna komponenta X.<br/><br/>Primjer: DayOfWeek 9/15/2013 12:00:00 Prikazano je nedjelja.
Datum | DayOfYear(X) | X: datuma i vremena | Dohvaća dan godine predstavljene komponentu godine od X.<br/><br/>Primjeri:<br/>1/12/2015: dan 335 2015.<br/>31/12/2015: dan 365 2015.<br/>31/12/2016: dan 366 2016 (Skok godina)
Datum | DaysInMonth(X) | X: datuma i vremena | Dohvaća dana u mjesecu predstavlja komponentu mjesec parametra X.<br/><br/>Primjer: DaysInMonth 9/15/2013 su 30 jer se od 30 dana u mjesecu rujan.
Datum | EndOfDay(X) | X: datuma i vremena | Dohvaća datum-vrijeme koji predstavlja dana (dan komponenta) x.<br/><br/>Primjer: EndOfDay 9/15/2013 05:10:23 PM je 9/15/2013 11:59:59 poslije Podne.
Datum | EndOfMonth(X) | X: datuma i vremena | Dohvaća kraju mjeseca predstavlja mjesec komponente parametra X. <br/><br/>Primjer: EndOfMonth 9/15/2013 05:10:23 PM je 30/9/2013 11:59:59 poslije Podne (datum vrijeme koji predstavlja Kraj mjeseca za rujan)
Datum | StartOfDay(X) | X: datuma i vremena | Dohvaća početni dan predstavlja dan komponente parametra X.<br/><br/>Primjer: StartOfDay 9/15/2013 05:10:23 PM je 9/15/2013 12:00:00 se.
Datum i vrijeme | FROM(X) | X: niz | Raščlaniti niz X na datum vrijeme.
Datum i vrijeme | Ticks(X) | X: datuma i vremena | Dohvaća na crtice na osi svojstvo parametra X. Jedan crtičnih iznosi 100 nanosekundi. Vrijednost tog svojstva predstavlja broj crtice na osi koje su proteklih od ponoći 12:00:00 siječanj 1 0001. 
Tekst | Format(X) | X: varijablu niza | Oblikovati tekst.

#### <a name="textformat-example"></a>Primjer Text.Format

    "defines": { 
        "Year" : "$$Text.Format('{0:yyyy}',WindowStart)",
        "Month" : "$$Text.Format('{0:MM}',WindowStart)",
        "Day" : "$$Text.Format('{0:dd}',WindowStart)",
        "Hour" : "$$Text.Format('{0:hh}',WindowStart)"
    }

[Prilagođeni datum i vrijeme Oblikovanje nizova](https://msdn.microsoft.com/library/8kb3ddd4.aspx) tema potražite u članku koji opisuje razne mogućnosti oblikovanja možete koristiti (na primjer: gg nasuprot gggg). 

> [AZURE.NOTE] Kada koristite funkcije unutar druge funkcije, ne trebate koristiti **$$** prefiks za funkciju unutarnji. Na primjer: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\"i spoji RowKey \\' {0:yyyy-MM-dd hh}\\", Time.AddHours (SliceStart -6)). U ovom primjeru, primijetit ćete da **$$** prefiks koji se koristi za funkciju **Time.AddHours** . 
  

