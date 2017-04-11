<properties 
   pageTitle="Prilagođeno zapisnike u prijava analitiku | Microsoft Azure"
   description="Prijava analitiku možete prikupiti događaje iz tekstnih datoteka na računala sa sustavom Windows i Linux.  U ovom se članku opisuje kako definiranje nove prilagođene zapisnika i pojedinosti zapisa koji se stvaraju u spremištu OMS."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-logs-in-log-analytics"></a>Prilagođeni zapisnika u zapisnik Analytics

Prilagođeno zapisnika izvor podataka u zapisnik Analytics omogućuje prikupljanje događaje iz tekstnih datoteka na računala sa sustavom Windows i Linux. Mnoge aplikacije prijaviti informacije tekstne datoteke umjesto standardne zapisivanje servise kao što su zapisnika događaja sustava Windows ili Syslog.  Kada koji se prikupljaju, možete analizirati svaki zapis u zapisniku u pojedinačna polja pomoću funkcije [Prilagođenih polja](log-analytics-custom-fields.md) zapisnika analize.

![Prikupljanje zapisnika prilagođene](media/log-analytics-data-sources-custom-logs/overview.png)

Datoteke zapisnika prikupljeni moraju se podudarati sljedeće kriterije.

- Zapisnik mora imati jednim zapisom po retku ili pomoću vremenske oznake koje odgovaraju na jedan od sljedećih oblika na početku svake stavke.

    GGGG-MM-DD HH <br>
  M/D/GGGG HH: MM: SS AM/PM <br>
  Pon DD, YYYY HH
    
- Datoteka zapisnika mora ne dopuštaju kružne ažuriranja gdje datoteku prebrisat će nove stavke. 

## <a name="defining-a-custom-log"></a>Definiranje prilagođenog zapisnika

Koristite sljedeći postupak da biste odredili prilagođene datoteke zapisnika.  Pomaknite se na kraju ovog članka vodič uzorka dodavanja prilagođene zapisnika.

### <a name="step-1-open-the-custom-log-wizard"></a>Korak 1. Otvaranje čarobnjaka za prilagođene zapisnika

Čarobnjak za prilagođene zapisnika izvodi na portalu OMS i omogućuje definiranje nove prilagođene zapisnika da biste prikupili.

1.  Na portalu OMS idite na **Postavke**.
2.  Kliknite **Podaci** , a zatim **Prilagođeno zapisnika**.
3.  Prema zadanim postavkama, sve promjene konfiguracije automatski pomiču se za sve agenata.  Za agente Linux konfiguracijska datoteka se šalje prikupljanje podataka Fluentd.  Ako želite da biste izmijenili ovu datoteku ručno na svaki pojedini agent Linux, poništite okvir *Primijeni ispod konfiguracije Moje strojeva Linux*.
4.  Kliknite **Dodaj +** da biste otvorili čarobnjak za prilagođene zapisnika.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Korak 2. Prijenos i analizirati zapisnik uzorka

Najprije prijenos uzorka prilagođene zapisnika.  Čarobnjak će raščlaniti i prikazati unose u datoteci za provjeru.  Prijava analitiku će koristiti graničnik koji navedete za identifikaciju svakog zapisa.

**Novi redak** je zadani razdjelnik i koristi se za datoteke zapisnika s jednim zapisom po retku.  Ako se pokrene u retku s datumom i vremenom u neku od dostupnih oblika, možete odrediti **vremenske oznake** graničnik koji podržava stavke koje se protežu na više od jednog retka. 

Ako se koristi vremensku oznaku razdjelnika, svojstvo TimeGenerated svaki zapis koji je spremljen u OMS biti će popunjen datuma/vremena naveden za taj unos u datoteke zapisnika.  Ako se koristi novi redak razdjelnika, TimeGenerated se popunjava s datumom i vremenom prijava analitiku prikupljeni je stavka. 

>[AZURE.NOTE]Prijava analitiku trenutno tretira datuma/vremena koji se prikupljaju iz zapisnika pomoću razdjelnika vremenske oznake kao UTC-a.  To će uskoro promijeniti da biste koristili vremensku zonu na agenta. 
 
1.  Kliknite **Pregledaj** , a zatim otvorite oglednu datoteku.  Imajte na umu da to može gumb može biti označene **Odabir datoteke** u nekim se preglednicima.
2.  Kliknite **Dalje**. 
3.  Čarobnjak za zapisnika prilagođena će prijenos datoteke i popis zapisa koji je označava.
4.  Promjena graničnika koji se koristi za prepoznavanje novi zapis, a zatim odaberite razdjelnik koji najbolje prepoznaje zapise u datoteci zapisnika.
5.  Kliknite **Dalje**.

### <a name="step-3-add-log-collection-paths"></a>Korak 3. Dodavanje putova prikupljanje zapisnika

Morate definirati jedan ili više putova na agent gdje možete pronaći prilagođene zapisnika.  Ili možete unijeti određene put i naziv datoteke zapisnika ili put možete odrediti zamjenske naziv.  To podržava aplikacija koje stvorite novu datoteku, svaki dan ili kada jedne datoteke dosegne određenu veličinu.  Možete unijeti i više putova za u jednu datoteku zapisnika.

Ako, na primjer, aplikacije stvoriti datoteku zapisnika svaki dan s datumom uključeni u nazivu kao log20100316.txt. Uzorak za pretraživanje kao zapisnik možda *zapisnika\*.txt* koji primjenjivati bilo koje datoteke zapisnika praćenja aplikacija je imenovanja shemu.

Sljedeća tablica sadrži Primjeri valjanih da biste naveli drugi zapisničke datoteke. 

| Opis | Put |
|:--|:--|
| Sve datoteke u *C:\Logs* s nastavkom .txt na agent za Windows | C:\Logs\\\*.txt |
| Sve datoteke u *C:\Logs* pod nazivom počevši od zapisniku i nastavkom .txt na agent za Windows | C:\Logs\log\*.txt |
| Sve datoteke u */var/log/audit* s nastavkom .txt na Linux agent | /VAR/log/audit/*.txt |
| Sve datoteke u */var/log/audit* pod nazivom počevši od zapisniku i nastavkom .txt na Linux agent | /VAR/log/audit/log\*.txt |
  

1.  Odaberite Windows ili Linux da biste odredili koji oblik put dodajete.
2.  Upišite put i klikom na **+** gumb.
3.  Ponovite postupak za sve dodatne putovi.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>Korak 4. Upišite naziv i opis u zapisnik

Naziv koji navedete će se koristiti za vrste zapisa prethodno opisan.  Uvijek će završiti s _CL za razlikovanje kao prilagođeni zapisnika.

1.  Upišite naziv za zapisnik.  U ** \_Očisti** sufiks dobivate automatski.
2.  Po želji dodajte **Opis**.
3.  Kliknite **Dalje** da biste spremili definiciju prilagođenog zapisnika.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>Korak 5. Provjerite valjanost su koji se prikupljaju prilagođene zapisnika
Ga može potrajati sata za početne podatke iz nove prilagođene zapisnika prikazuju analitički zapisnika.  Započinje prikupljanje stavki iz zapisnika pronađen u putu koje ste naveli od točke koje ste definirali prilagođene zapisnika.  Će zadržati stavke koje ste prenijeli tijekom stvaranja prilagođenih zapisnik, ali ga će prikupiti već postojeće stavke u datoteke zapisnika koje pronađe.

Kada prijava analitiku pokrene prikupljanje iz prilagođene zapisnika, njegov zapisa bit će dostupni pomoću zapisnika pretraživanja.  Koristite naziv koji ste dodijelili prilagođene zapisnika kao **vrstu** u upitu.

>[AZURE.NOTE] Ako je svojstvo RawData nedostaje iz pretraživanja, možda ćete morati zatvoriti i ponovno otvorite preglednik.

### <a name="step-6-parse-the-custom-log-entries"></a>Korak 6. Analizirati stavke prilagođene evidencije

Stavka cijelu evidencije će se spremiti u jedno svojstvo naziva **RawData**.  Vjerojatno ćete odvojite različite dijelove informacija u svaku stavku u pojedinačne svojstva pohranjena u zapisu.  Učinite ovo pomoću funkcije [Prilagođenih polja](log-analytics-custom-fields.md) zapisnika analize.

Detaljne upute za Raščlanjivanje stavku prilagođene zapisnika nije ovdje naveden.  Pogledajte ove informacije potražite u dokumentaciji [Prilagođena polja](log-analytics-custom-fields.md) .

## <a name="disabling-a-custom-log"></a>Onemogućivanje prilagođene zapisnika

Nije moguće ukloniti definiciju prilagođenog zapisnika kada je stvorena, ali ga možete onemogućiti tako da uklonite sve njegove putova zbirke.

1.  Na portalu OMS idite na **Postavke**.
2.  Kliknite **Podaci** , a zatim **Prilagođeno zapisnika**.
3.  Uz definiciju prilagođenog zapisnika da biste onemogućili kliknite **Detalji** .
4.  Uklanjanje svih putove zbirke definiciju prilagođenog zapisnika.


## <a name="data-collection"></a>Prikupljanje podataka

Prijava analitiku će prikupiti nove stavke iz svakog prilagođene zapisnika otprilike svakih 5 minuta.  Agenta će u svaku datoteku zapisnika koja prikuplja iz zapis umjesto nje.  Ako agenta vodi izvan mreže za neko vrijeme, zatim prijava analitiku će prikupiti stavke s mjesta na kojem je zadnji put stali, čak i ako se one stavke stvorene dok agenta je izvan mreže.

Sav sadržaj stavku zapisnika zapisuju za jedno svojstvo naziva **RawData**.  To možete analizirati u više svojstava koja možete analizirati i pretraživati odvojeno definiranjem [Prilagođena polja](log-analytics-custom-fields.md) nakon što stvorite prilagođenu zapisnika.


## <a name="custom-log-record-properties"></a>Prilagođeni zapisnika svojstvima zapisa

Prilagođeni zapisnika zapisi imaju vrstu uz naziv zapisnika koje navodite i svojstva u tablici u nastavku.

| Svojstvo | Opis |
|:--|:--|
| TimeGenerated | Datum i vrijeme kad je zapis prikupljene putem prijava analitiku.  Ako u zapisniku koristi razdjelnika vremena Ovo je vrijeme prikupljenih unosa. |
| SourceSystem  | Vrsta agent je prikupljenih zapis. <br> Povezivanje OpsManager – agent za Windows, bilo izravno ili SCOM <br> Linux – svi agenti Linux  |
| RawData             | Cijeli tekst prikupljene stavke. |
| ManagementGroupName | Naziv grupe za upravljanje SCOM agenata.  Za ostale agenata to je AOI -\<ID radnog prostora\> |


## <a name="log-searches-with-custom-log-records"></a>Zapisnika pretraživanja sa zapisima prilagođene zapisnika

Zapisi iz prilagođene zapisnika spremaju se u spremištu OMS baš kao i zapisi iz drugih izvora podataka.  Oni će imati vrstu odgovarajuće ime koje unesete prilikom definiranja zapisnik, da biste mogli koristiti svojstvo vrsta u rezultatima pretraživanja za dohvaćanje zapisa koji se prikupljaju iz određene zapisnika.

Sljedeća tablica sadrži različite Primjeri zapisnika pretraživanja koja se dohvaćanje zapisa s prilagođene zapisnika.

| Upit | Opis |
|:--|:--|
| Vrsta = MyApp_CL | Svi događaji iz prilagođene prijavite imenovani MyApp_CL. |
| Vrsta = MyApp_CL Severity_CF = pogreške | Svi događaji iz prilagođene zapisnika pod nazivom MyApp_CL je vrijednost *pogreška* u prilagođeno polje pod nazivom *Severity_CF*. |




## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Vodič za uzorak dodavanja prilagođene zapisnika

U sljedećoj sekciji vodi kroz primjera stvaranje prilagođenih zapisnika.  Zapisnik uzorka koji se prikupljaju sadrži jednu stavku u svakom retku počevši od datuma i vremena i zatim zarez Razgraničena polja za kod, status i poruku.  Nekoliko oglednih stavki možete prikazano u nastavku.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Prijenos i analizirati zapisnik uzorka

Ne možemo navedite neke datoteke zapisnika i možete vidjeti događaje koje će biti prikupljanje.  U ovom slučaju novi redak je dovoljno razdjelnika.  Ako jedan unos u zapisniku kroz može obuhvaćati više redaka, vremenska oznaka razdjelnika potrebni koja će se koristiti.

![Prijenos i analizirati zapisnik uzorka](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Dodavanje putova prikupljanje zapisnika

Datoteka zapisnika će se nalaziti u *C:\MyApp\Logs*.  Svaki dan s nazivom koji sadrži datum u uzorku *appYYYYMMDD.log*će stvoriti nove datoteke.  Uzorak potrebne za ovaj zapisnik bio *C:\MyApp\Logs\\\*.log*.

![Put prikupljanje zapisnika](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Upišite naziv i opis u zapisnik

Koristimo naziv *MyApp_CL* i vrstu u **Opis**.

![Naziv zapisnika](media/log-analytics-data-sources-custom-logs/log-name.png)


### <a name="validate-that-the-custom-logs-are-being-collected"></a>Provjerite valjanost su koji se prikupljaju prilagođene zapisnika

Koristimo upit *Vrsta = MyApp_CL* za vraćanje svih zapisa iz prikupljene zapisnika.

![Zapisnik upita bez prilagođena polja](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>Analizirati stavke prilagođene evidencije

Koristimo prilagođenih polja da biste definirali *EventTime*, *kod*, *Status*i polja *poruke* , a ne možemo možete vidjeti razlike u zapisima koje je vratio upit.

![Zapisnik upit s prilagođenim poljima](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Daljnji koraci

- Pomoću [Prilagođenih polja](log-analytics-custom-fields.md) analizirati stavke u zapisniku prilagođene u pojedinačna polja.
- Informirajte se o [zapisniku pretraživanja](log-analytics-log-searches.md) radi analize podataka prikupljenih iz izvora podataka i rješenja. 