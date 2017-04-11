<properties
    pageTitle="Dodavanje sustava Office 365 Outlook connector u aplikacijama za logiku | Microsoft Azure"
    description="Stvaranje aplikacije logike s poveznik za Office 365 da biste omogućili interakcija sa sustavom Office 365. Na primjer: Stvaranje, uređivanje i ažuriranje kontakata i stavki kalendara."
    services=""    
    documentationCenter=""     
    authors="MandiOhlinger"    
    manager="anneta"    
    editor="" 
    tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="10/18/2016"
ms.author="mandia"/>

# <a name="get-started-with-the-office-365-outlook-connector"></a>Uvod u Office 365 Outlook connector 

Office 365 Outlook connector omogućuje interakciju s programom Outlook u sustavu Office 365. Koristite ovaj poveznik za stvaranje, uređivanje i ažuriranje kontakata i stavki kalendara i dobiti, slanje i odgovaranje na e-pošte.

S programom Outlook sustava Office 365, koji:

- Stvaranje tijekova rada pomoću značajke e-pošte i kalendara u Office 365. 
- Uporaba okidača za pokretanje tijeka rada kada je na nove e-pošte, prilikom ažuriranja stavke kalendara i drugo.
- Korištenje akcija da biste poslali poruku e-pošte, stvorite novi događaj u kalendaru i drugo. Na primjer, kada je novi objekt u Salesforce (okidač), pošaljite poruku e-pošte u Office 365 Outlook (akcije). 

U ovoj se temi pokazuje kako koristiti Office 365 Outlook connector u aplikaciji logike pa se navode okidača i akcija.

>[AZURE.NOTE] Ovu verziju članka primjenjuje se na logike aplikacije Općenito dostupan (GA).

Da biste saznali više o aplikacijama logike, potražite u članku [što su logike aplikacije](../app-service-logic/app-service-logic-what-are-logic-apps.md) i [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-office-365"></a>Povezivanje sa sustavom Office 365

Logika aplikacije možete pristupiti bilo koji servis, prvo stvorite *vezu* sa servisom. Veze navedene veze između logike aplikacije i drugih servisa. Na primjer, da biste se povezali s programom Outlook sustava Office 365, najprije morate *Povezivanje*sustava Office 365. Da biste stvorili vezu, unesite vjerodajnice obično koristite za pristup servisu koji se želite povezati. Stoga s programom Outlook sustava Office 365, unesite vjerodajnice za račun sustava Office 365 da biste stvorili vezu.


## <a name="create-the-connection"></a>Stvaranje veze

>[AZURE.INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]

## <a name="use-a-trigger"></a>Koristite okidač

Okidač je događaja koji se mogu koristiti za pokretanje tijeka rada definirano u aplikaciji logike. Okidača "ankete" usluge na interval i učestalost koju želite. [Dodatne informacije o okidača](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. U aplikaciji logike upišite "office 365" dobit ćete popis s okidačima:  

    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)

2. Odaberite **Office 365 Outlook - prilikom nadolazeći događaj prije pokretanja**. Ako se veza već postoji, odaberite kalendar s padajućeg popisa.

    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)

    Ako se od vas zatraži prijavite, unesite znak u detalja da biste stvorili vezu. [Stvaranje veza](connectors-create-api-office365-outlook.md#create-the-connection) u nastavku navedeni koraci. 

    > [AZURE.NOTE] U ovom primjeru aplikaciju logike se pokreće kada se ažurira događaja u kalendaru. Da biste vidjeli rezultate ovog okidača, dodajte drugih akcija koje vam šalje poruku s tekstom. Ako, na primjer, dodajte akciju Twilio *poslati poruku* te tekstova kada događaj u kalendaru se pokreće 15 minuta. 

3. Odaberite gumb **Uredi** , a zatim postavite **Učestalost** i **Interval** vrijednosti. Ako, na primjer, ako želite da se okidača za svakih 15 minuta, pa **Učestalost** postavite **minutu**i postavite **Interval** **15**. 

    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)

4. **Spremite** promjene (gornjem lijevom kutu na alatnoj traci). Pokrenite aplikaciju logike se sprema i možda je automatski omogućena.


## <a name="use-an-action"></a>Koristite akciju

Akciju je postupak provodi definirano u aplikaciji logike tijeka rada. [Dodatne informacije o akcije](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Odaberite znak plus. Pogledajte nekoliko mogućnosti: **Dodaj akciju**, **Dodaj uvjet**ili neke **Dodatne** mogućnosti.

    ![](./media/connectors-create-api-office365-outlook/add-action.png)

2. Odaberite **Dodaj akciju**.

3. U tekstni okvir upišite "office 365" da biste dobili popis dostupnih akcija.

    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 

4. U našem primjeru odaberite **Office 365 Outlook - stvorite kontakt**. Ako se veza već postoji, odaberite **Mapu ID**, **ime**i druga svojstva:  

    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)

    Ako su za podatke o vezi od vas zatraži, unesite detalje da biste stvorili vezu. [Stvaranje veze](connectors-create-api-office365-outlook.md#create-the-connection) u ovoj se temi opisuju tih svojstava. 

    > [AZURE.NOTE] U ovom primjeru ćemo stvoriti novi kontakt u programu Outlook za Office 365. Izlaz iz drugog okidača možete koristiti da biste stvorili kontakt. Na primjer, dodati okidača SalesForce *stvaranja objekta* . Zatim dodajte akciju Office 365 Outlook *Stvori kontakt* koji koristi SalesForce polja da biste stvorili novi novi kontakt u sustavu Office 365. 

5. **Spremite** promjene (gornjem lijevom kutu na alatnoj traci). Pokrenite aplikaciju logike se sprema i možda je automatski omogućena.


## <a name="technical-details"></a>Tehničke pojedinosti

Evo detalja o okidačima, akcije i odgovore koje podržava tu vezu:

## <a name="office-365-triggers"></a>Okidača za Office 365

|Pokretanje | Opis|
|--- | ---|
|[Kada je nadolazeći događaj početni uskoro](connectors-create-api-office365-outlook.md#when-an-upcoming-event-is-starting-soon)|Ovaj postupak pokrene u tijeku pokretanja nadolazećih događaja.|
|[Kada pristigne nova e-pošta](connectors-create-api-office365-outlook.md#when-a-new-email-arrives)|Ovaj postupak pokrene u tijeku kada pristigne nova e-pošta|
|[Pri stvaranju novog događaja](connectors-create-api-office365-outlook.md#when-a-new-event-is-created)|Ovaj postupak pokrene u tijeku prilikom stvaranja novog događaja u kalendaru.|
|[Izmjene događaja](connectors-create-api-office365-outlook.md#when-an-event-is-modified)|Ovaj postupak pokrene u tijeku prilikom izmjene događaja u kalendaru.|


## <a name="office-365-actions"></a>Akcije za Office 365

|Akcija|Opis|
|--- | ---|
|[Početak poruke e-pošte](connectors-create-api-office365-outlook.md#get-emails)|Ovaj postupak može vidjeti poruke e-pošte iz mape.|
|[Slanje poruke e-pošte](connectors-create-api-office365-outlook.md#send-an-email)|Ovaj postupak šalje poruku e-pošte.|
|[Brisanje e-pošte](connectors-create-api-office365-outlook.md#delete-email)|Ovaj postupak briše poruke e-pošte tako da ID-a.|
|[Označi kao pročitano](connectors-create-api-office365-outlook.md#mark-as-read)|Ovaj postupak označava poruku e-pošte pročitanim.|
|[Odgovaranje na e-pošte](connectors-create-api-office365-outlook.md#reply-to-email)|Ovaj postupak odgovore na poruku e-pošte.|
|[Početak privitka](connectors-create-api-office365-outlook.md#get-attachment)|Ovaj postupak prima privitak e-pošte putem ID-a.|
|[Slanje e-pošte s mogućnostima](connectors-create-api-office365-outlook.md#send-email-with-options)|Ovaj postupak šalje poruku e-pošte s više mogućnosti, a čeka primatelju odgovor na poruke uz neku od mogućnosti.|
|[Slanje e-pošte za odobrenje](connectors-create-api-office365-outlook.md#send-approval-email)|Ovaj postupak šalje poruku e-pošte za odobrenje i čeka se odgovor primatelja.|
|[Početak kalendara](connectors-create-api-office365-outlook.md#get-calendars)|Ovaj postupak popis dostupnih kalendara.|
|[Početak događaja](connectors-create-api-office365-outlook.md#get-events)|Ovaj postupak može vidjeti događaje iz kalendara.|
|[Stvaranje događaja](connectors-create-api-office365-outlook.md#create-event)|Ovaj postupak stvara novi događaj u kalendaru.|
|[Početak događaja](connectors-create-api-office365-outlook.md#get-event)|Ovaj postupak može vidjeti određene događaj iz kalendara.|
|[Brisanje događaja](connectors-create-api-office365-outlook.md#delete-event)|Ovaj postupak brisanje događaja u kalendaru.|
|[Ažuriranje događaja](connectors-create-api-office365-outlook.md#update-event)|Ovaj postupak ažurira događaja u kalendaru.|
|[Dohvaćanje mape s kontaktima](connectors-create-api-office365-outlook.md#get-contact-folders)|Ovaj postupak popis dostupnih kontakata mape.|
|[Dohvaćanje kontakata](connectors-create-api-office365-outlook.md#get-contacts)|Ovaj postupak može vidjeti kontakte iz mape Kontakti.|
|[Stvaranje kontakta](connectors-create-api-office365-outlook.md#create-contact)|Ovaj postupak stvara novi kontakt u mapi Kontakti.|
|[Dohvaćanje kontakata](connectors-create-api-office365-outlook.md#get-contact)|Ovaj postupak može vidjeti određeni kontakt iz mape Kontakti.|
|[Brisanje kontakta](connectors-create-api-office365-outlook.md#delete-contact)|Ovaj postupak briše kontakta iz mape Kontakti.|
|[Ažuriranje kontakta](connectors-create-api-office365-outlook.md#update-contact)|Ovaj postupak ažuriranja kontakta u mapi Kontakti.|

### <a name="trigger-and-action-details"></a>Detalji o okidača i akcija

U ovom ćete odjeljku potražite u članku određene detalje o svakom okidača i akcija, uključujući obavezan ili nije unos svojstva i sve odgovarajuće izlaz pridružene poveznik.

#### <a name="when-an-upcoming-event-is-starting-soon"></a>Kada je nadolazeći događaj početni uskoro
Ovaj postupak pokrene u tijeku pokretanja nadolazećih događaja. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id kalendara|Jedinstveni identifikator kalendara|
|lookAheadTimeInMinutes|Izgled dovršena prije sljedeće operacije vremena|Da biste potražili predstojeće događaje na unaprijed vrijeme (u minutama)|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
CalendarItemsList: Popis stavki kalendara

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|vrijednost|polja|Popis stavki kalendara|


#### <a name="get-emails"></a>Početak poruke e-pošte
Ovaj postupak može vidjeti poruke e-pošte iz mape. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|folderPath|Put do mape|Put do mape u radi dohvaćanja poruke e-pošte (zadani: 'Ulazna pošta')|
|vrh|Vrh|Broj e-pošte za dohvaćanje (zadani: 10)|
|fetchOnlyUnread|Dohvaćanje samo nepročitanih poruka|Dohvaćanje samo nepročitanih poruka e-pošte?|
|includeAttachments|Sadrže privitke|Ako je postavljen na true, privici će biti dohvaćeni i zajedno s e-pošte|
|searchQuery|Upit za pretraživanje|Upit za pretraživanje da biste filtrirali poruke e-pošte|
|Preskoči|Preskoči|Broj e-pošte da biste preskočili (zadani: 0)|
|skipToken|Preskoči tokena|Preskakanje token dohvaćanje nove stranice|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
ReceiveMessage: Primiti poruku e-pošte

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|Iz|niz|Iz|
|Da biste|niz|Da biste|
|Predmet|niz|Predmet|
|Tijelo|niz|Tijelo|
|Važnost|niz|Važnost|
|HasAttachment|Booleove vrijednosti|S privitkom|
|ID-a|niz|Id poruke|
|IsRead|Booleove vrijednosti|Je za čitanje|
|DateTimeReceived|niz|Datum vrijeme zaprimanja|
|Privici|polja|Privici|
|Kopija|niz|Odredite razdvojenih točkom sa zarezom kao što su adrese e-poštesomeone@contoso.com|
|Skrivena kopija|niz|Odredite razdvojenih točkom sa zarezom kao što su adrese e-poštesomeone@contoso.com|
|IsHtml|Booleove vrijednosti|Upozorenje|


#### <a name="send-an-email"></a>Slanje poruke e-pošte
Ovaj postupak šalje poruku e-pošte. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|PorukaEPošte *|E-pošte|E-pošte|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Ništa.

#### <a name="delete-email"></a>Brisanje e-pošte
Ovaj postupak briše poruke e-pošte tako da ID-a. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|messageId *|Id poruke|ID e-pošte da biste izbrisali|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Ništa.

#### <a name="mark-as-read"></a>Označi kao pročitano
Ovaj postupak označava poruku e-pošte pročitanim. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|messageId *|Id poruke|ID e-pošte označiti kao čitanja|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Ništa.


#### <a name="reply-to-email"></a>Odgovaranje na e-pošte
Ovaj postupak odgovore na poruku e-pošte. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|messageId *|Id poruke|ID e-pošte da biste odgovorili|
|komentar *|Komentar|Odgovor komentar|
|replyAll|Odgovori svima|Odgovaranje na svim primateljima|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Ništa.


#### <a name="get-attachment"></a>Početak privitka
Ovaj postupak prima privitak e-pošte putem ID-a. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|messageId *|Id poruke|ID e-pošte|
|attachmentId *|Id privitka|ID da biste preuzeli privitak|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Ništa.


#### <a name="when-a-new-email-arrives"></a>Kada pristigne nova e-pošta
Ovaj postupak pokrene u tijeku kada pristigne nova e-pošta.

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|folderPath|Put do mape|Mape e-pošte za dohvaćanje (zadani: Ulazna pošta)|
|Da biste|Da biste|Adrese primatelja e-pošte|
|iz|Iz|S adrese|
|važnost|Važnost|Važnost e-pošte (najviša, normalno, najniža) (zadani: normalno)|
|fetchOnlyWithAttachment|Ima privitke|Dohvaćanje samo poruke e-pošte s privitkom|
|includeAttachments|Sadrže privitke|Sadrže privitke|
|subjectFilter|Filtar za predmet|Niz za pretraživanje u predmetu|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
TriggerBatchResponse [ReceiveMessage]

| Naziv svojstva | Vrsta podataka |
|---|---|
|vrijednost|polja|


#### <a name="send-email-with-options"></a>Slanje e-pošte s mogućnostima
Ovaj postupak šalje poruku e-pošte s više mogućnosti, a čeka primatelju odgovor na poruke uz neku od mogućnosti. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|optionsEmailSubscription *|Zahtjev za pretplatu za mogućnosti e-pošte|Zahtjev za pretplatu za mogućnosti e-pošte|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
SubscriptionResponse: Model za pretplatu e-pošte za odobrenje

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|ID-a|niz|ID pretplate|
|resurs|niz|Resursa zahtjev za pretplatu|
|notificationType|niz|Vrsta obavijesti|
|notificationUrl|niz|Obavijest o URL-a|


#### <a name="send-approval-email"></a>Slanje e-pošte za odobrenje
Ovaj postupak šalje poruku e-pošte za odobrenje i čeka se odgovor primatelja. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|approvalEmailSubscription *|Zahtjev za pretplatu za e-poštu za odobrenje|Zahtjev za pretplatu za e-poštu za odobrenje|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
SubscriptionResponse: Model za pretplatu e-pošte za odobrenje

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|ID-a|niz|ID pretplate|
|resurs|niz|Resursa zahtjev za pretplatu|
|notificationType|niz|Vrsta obavijesti|
|notificationUrl|niz|Obavijest o URL-a|


#### <a name="get-calendars"></a>Početak kalendara
Ovaj postupak popis dostupnih kalendara. 

Nema parametara za ovaj poziv.

##### <a name="output-details"></a>Detalji o Izlaz
TablesList

| Naziv svojstva | Vrsta podataka |
|---|---|
|vrijednost|polja|


#### <a name="get-events"></a>Početak događaja
Ovaj postupak može vidjeti događaje iz kalendara. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id kalendara|Odaberite kalendar|
|$filter|Filtar upita|Upit ODATA filtar da biste ograničili stavke vraćaju|
|$orderby|Poredaj po|Upit orderBy ODATA za određivanje redoslijeda stavki|
|$skip|Preskoči numeriranje|Broj stavki da biste preskočili (zadani = 0)|
|$top|Maksimalna Get Count|Maksimalan broj stavki za dohvaćanje (zadani = 256)|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
CalendarEventList: Popis stavki kalendara

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|vrijednost|polja|Popis stavki kalendara|


#### <a name="create-event"></a>Stvaranje događaja
Ovaj postupak stvara novi događaj u kalendaru. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id kalendara|Odaberite kalendar|
|stavke *|Stavke|Stvaranje događaja|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
CalendarEvent: Poveznik za određeni kalendar modela klasa događaja.

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|ID-a|niz|Jedinstveni identifikator željeni događaj.|
|Sudionicima|polja|Popis sudionika za događaj.|
|Tijelo|nije definirano|Tijelo poruke povezanog s događajem.|
|BodyPreview|niz|Pretpregled poruka povezanog s događajem.|
|Kategorije|polja|Kategorije povezanog s događajem.|
|ChangeKey|niz|Određuje verziju objekta događaja. Svaki put kada se promijeni događaja, kao i se mijenja ChangeKey.|
|DateTimeCreated|niz|Datum i vrijeme stvaranja događaja.|
|DateTimeLastModified|niz|Datum i vrijeme zadnje izmjene događaj.|
|END|niz|Vrijeme završetka događaja.|
|EndTimeZone|niz|Navodi vremensku zonu sastanka završno vrijeme. Ta vrijednost mora biti kako je definirano u sustavu Windows (primjer: "Po pacifičkom standardnom vremenu").|
|HasAttachments|Booleove vrijednosti|Postavite na true ako događaj ima privitke.|
|Važnost|niz|Važnost događaja: Nisko, normalno ili visoka.|
|IsAllDay|Booleove vrijednosti|Postavite na true ako događaj traje cijeli dan.|
|IsCancelled|Booleove vrijednosti|Postavite na true ako događaj otkazan.|
|IsOrganizer|Booleove vrijednosti|Postavite na true ako je pošiljatelj poruke organizatora.|
|Mjesto|nije definirano|Mjesto događaja.|
|Organizator|nije definirano|Organizator događaj.|
|Ponavljanje|nije definirano|Uzorak ponavljanja za događaj.|
|Podsjetnik|cijeli broj|Vrijeme u minutama prije početka događaja kao podsjetnik.|
|ResponseRequested|Booleove vrijednosti|Postavite na true li pošiljatelju odgovor kada se događaj prihvatili ili odbili.|
|ResponseStatus|nije definirano|Navodi vrstu odgovora poslane kao odgovor na poruku događaj.|
|SeriesMasterId|niz|Jedinstveni identifikator za događaj u nizu matrice.|
|Pokaži kao|niz|Prikazuje slobodno ili zauzeto.|
|Pokretanje|niz|Vrijeme početka događaja.|
|StartTimeZone|niz|Određuje vremenske zone sastanka vrijeme početka. Ta vrijednost mora biti kako je definirano u sustavu Windows (primjer: "Po pacifičkom standardnom vremenu").|
|Predmet|niz|Predmet događaj.|
|Vrsta|niz|Vrsta događaja: instancu, pojavljivanje, iznimku ni na matricu niz.|
|Vezu|niz|Pretpregled poruka povezanog s događajem.|


#### <a name="get-event"></a>Početak događaja
Ovaj postupak može vidjeti određene događaj iz kalendara. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id kalendara|Odaberite kalendar|
|ID *|Id stavke|Odaberite događaj|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
CalendarEvent: Poveznik za određeni kalendar modela klasa događaja.

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|ID-a|niz|Jedinstveni identifikator željeni događaj.|
|Sudionicima|polja|Popis sudionika za događaj.|
|Tijelo|nije definirano|Tijelo poruke povezanog s događajem.|
|BodyPreview|niz|Pretpregled poruka povezanog s događajem.|
|Kategorije|polja|Kategorije povezanog s događajem.|
|ChangeKey|niz|Određuje verziju objekta događaja. Svaki put kada se promijeni događaja, kao i se mijenja ChangeKey.|
|DateTimeCreated|niz|Datum i vrijeme stvaranja događaja.|
|DateTimeLastModified|niz|Datum i vrijeme zadnje izmjene događaj.|
|END|niz|Vrijeme završetka događaja.|
|EndTimeZone|niz|Navodi vremensku zonu sastanka završno vrijeme. Ta vrijednost mora biti kako je definirano u sustavu Windows (primjer: "Po pacifičkom standardnom vremenu").|
|HasAttachments|Booleove vrijednosti|Postavite na true ako događaj ima privitke.|
|Važnost|niz|Važnost događaja: Nisko, normalno ili visoka.|
|IsAllDay|Booleove vrijednosti|Postavite na true ako događaj traje cijeli dan.|
|IsCancelled|Booleove vrijednosti|Postavite na true ako događaj otkazan.|
|IsOrganizer|Booleove vrijednosti|Postavite na true ako je pošiljatelj poruke organizatora.|
|Mjesto|nije definirano|Mjesto događaja.|
|Organizator|nije definirano|Organizator događaj.|
|Ponavljanje|nije definirano|Uzorak ponavljanja za događaj.|
|Podsjetnik|cijeli broj|Vrijeme u minutama prije početka događaja kao podsjetnik.|
|ResponseRequested|Booleove vrijednosti|Postavite na true li pošiljatelju odgovor kada se događaj prihvatili ili odbili.|
|ResponseStatus|nije definirano|Navodi vrstu odgovora poslane kao odgovor na poruku događaj.|
|SeriesMasterId|niz|Jedinstveni identifikator za događaj u nizu matrice.|
|Pokaži kao|niz|Prikazuje slobodno ili zauzeto.|
|Pokretanje|niz|Vrijeme početka događaja.|
|StartTimeZone|niz|Određuje vremenske zone sastanka vrijeme početka. Ta vrijednost mora biti kako je definirano u sustavu Windows (primjer: "Po pacifičkom standardnom vremenu").|
|Predmet|niz|Predmet događaj.|
|Vrsta|niz|Vrsta događaja: instancu, pojavljivanje, iznimku ni na matricu niz.|
|Vezu|niz|Pretpregled poruka povezanog s događajem.|


#### <a name="delete-event"></a>Brisanje događaja
Ovaj postupak brisanje događaja u kalendaru. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id kalendara|Odaberite kalendar|
|ID *|ID-a|Odaberite događaj|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Ništa.


#### <a name="update-event"></a>Ažuriranje događaja
Ovaj postupak ažurira događaja u kalendaru. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id kalendara|Odaberite kalendar|
|ID *|ID-a|Odaberite događaj|
|stavke *|Stavke|Događaj za ažuriranje|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
CalendarEvent: Poveznik za određeni kalendar modela klasa događaja.

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|ID-a|niz|Jedinstveni identifikator željeni događaj.|
|Sudionicima|polja|Popis sudionika za događaj.|
|Tijelo|nije definirano|Tijelo poruke povezanog s događajem.|
|BodyPreview|niz|Pretpregled poruka povezanog s događajem.|
|Kategorije|polja|Kategorije povezanog s događajem.|
|ChangeKey|niz|Određuje verziju objekta događaja. Svaki put kada se promijeni događaja, kao i se mijenja ChangeKey.|
|DateTimeCreated|niz|Datum i vrijeme stvaranja događaja.|
|DateTimeLastModified|niz|Datum i vrijeme zadnje izmjene događaj.|
|END|niz|Vrijeme završetka događaja.|
|EndTimeZone|niz|Navodi vremensku zonu sastanka završno vrijeme. Ta vrijednost mora biti kako je definirano u sustavu Windows (primjer: "Po pacifičkom standardnom vremenu").|
|HasAttachments|Booleove vrijednosti|Postavite na true ako događaj ima privitke.|
|Važnost|niz|Važnost događaja: Nisko, normalno ili visoka.|
|IsAllDay|Booleove vrijednosti|Postavite na true ako događaj traje cijeli dan.|
|IsCancelled|Booleove vrijednosti|Postavite na true ako događaj otkazan.|
|IsOrganizer|Booleove vrijednosti|Postavite na true ako je pošiljatelj poruke organizatora.|
|Mjesto|nije definirano|Mjesto događaja.|
|Organizator|nije definirano|Organizator događaj.|
|Ponavljanje|nije definirano|Uzorak ponavljanja za događaj.|
|Podsjetnik|cijeli broj|Vrijeme u minutama prije početka događaja kao podsjetnik.|
|ResponseRequested|Booleove vrijednosti|Postavite na true li pošiljatelju odgovor kada se događaj prihvatili ili odbili.|
|ResponseStatus|nije definirano|Navodi vrstu odgovora poslane kao odgovor na poruku događaj.|
|SeriesMasterId|niz|Jedinstveni identifikator za događaj u nizu matrice.|
|Pokaži kao|niz|Prikazuje slobodno ili zauzeto.|
|Pokretanje|niz|Vrijeme početka događaja.|
|StartTimeZone|niz|Određuje vremenske zone sastanka vrijeme početka. Ta vrijednost mora biti kako je definirano u sustavu Windows (primjer: "Po pacifičkom standardnom vremenu").|
|Predmet|niz|Predmet događaj.|
|Vrsta|niz|Vrsta događaja: instancu, pojavljivanje, iznimku ni na matricu niz.|
|Vezu|niz|Pretpregled poruka povezanog s događajem.|


#### <a name="when-a-new-event-is-created"></a>Pri stvaranju novog događaja
Ovaj postupak pokrene u tijeku prilikom stvaranja novog događaja u kalendaru. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id kalendara|Odaberite kalendar|
|$filter|Filtar upita|Upit ODATA filtar da biste ograničili stavke vraćaju|
|$orderby|Poredaj po|Upit orderBy ODATA za određivanje redoslijeda stavki|
|$skip|Preskoči numeriranje|Broj stavki da biste preskočili (zadani = 0)|
|$top|Maksimalna Get Count|Maksimalan broj stavki za dohvaćanje (zadani = 256)|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
CalendarItemsList: Popis stavki kalendara

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|vrijednost|polja|Popis stavki kalendara|


#### <a name="when-an-event-is-modified"></a>Izmjene događaja
Ovaj postupak pokrene u tijeku prilikom izmjene događaja u kalendaru. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id kalendara|Odaberite kalendar|
|$filter|Filtar upita|Upit ODATA filtar da biste ograničili stavke vraćaju|
|$orderby|Poredaj po|Upit orderBy ODATA za određivanje redoslijeda stavki|
|$skip|Preskoči numeriranje|Broj stavki da biste preskočili (zadani = 0)|
|$top|Maksimalna Get Count|Maksimalan broj stavki za dohvaćanje (zadani = 256)|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
CalendarItemsList: Popis stavki kalendara


| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|vrijednost|polja|Popis stavki kalendara|


#### <a name="get-contact-folders"></a>Dohvaćanje mape s kontaktima
Ovaj postupak popis dostupnih kontakata mape. 

Nema parametara za ovaj poziv.

##### <a name="output-details"></a>Detalji o Izlaz
TablesList

| Naziv svojstva | Vrsta podataka |
|---|---|
|vrijednost|polja|


#### <a name="get-contacts"></a>Dohvaćanje kontakata
Ovaj postupak može vidjeti kontakte iz mape Kontakti. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id mape|Jedinstveni identifikator mapu s kontaktima za dohvaćanje|
|$filter|Filtar upita|Upit ODATA filtar da biste ograničili stavke vraćaju|
|$orderby|Poredaj po|Upit orderBy ODATA za određivanje redoslijeda stavki|
|$skip|Preskoči numeriranje|Broj stavki da biste preskočili (zadani = 0)|
|$top|Maksimalna Get Count|Maksimalan broj stavki za dohvaćanje (zadani = 256)|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
ContactList: Popis kontakata

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|vrijednost|polja|Popis kontakata|


#### <a name="create-contact"></a>Stvaranje kontakta
Ovaj postupak stvara novi kontakt u mapi Kontakti. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id mape|Odaberite mapu s kontaktima|
|stavke *|Stavke|Stvaranje kontakta|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Kontakt: kontakta

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|ID-a|niz|Jedinstveni identifikator kontakta.|
|ParentFolderId|niz|ID nadređene mape kontakta|
|Rođendan|niz|Rođendan kontakta.|
|FileAs|niz|U odjeljku spremljena ime kontakta.|
|Riješiti problem|niz|Zaslonsko ime kontakta.|
|GivenName|niz|Ime kontakta.|
|Inicijali|niz|Inicijale kontakta.|
|MiddleName|niz|Srednje ime kontakta.|
|Nadimak|niz|Nadimak kontakta.|
|Prezime|niz|Prezime kontakta.|
|Naslov|niz|Naslov kontakta.|
|Generiranje|niz|Generiranje kontakta.|
|EmailAddresses|polja|Adrese e-pošte kontakta.|
|ImAddresses|polja|Kontakta izravnih poruka (IM) adrese.|
|JobTitle|niz|Poslovna titula kontakta.|
|NazivTvrtke|niz|Naziv tvrtke kontakta.|
|Odjelu|niz|Odjel kontakta.|
|OfficeLocation|niz|Mjesto kontakta office.|
|Struka|niz|Struka kontakta.|
|BusinessHomePage|niz|Početna stranica business kontakta.|
|AssistantName|niz|Naziv pomoćnika kontakta.|
|Upravitelj|niz|Naziv Upravitelj kontakta.|
|HomePhones|polja|Brojevi Kućni telefon kontakta.|
|BusinessPhones|polja|Kontakta tvrtke telefonskih brojeva|
|MobilePhone1|niz|Broj mobilnog telefona kontakta.|
|HomeAddress|nije definirano|Kućna adresa kontakta.|
|BusinessAddress|nije definirano|Adresa tvrtke kontakta.|
|OtherAddress|nije definirano|Druge adrese za kontakt.|
|YomiCompanyName|niz|Ime fonetski japanski tvrtke kontakta.|
|YomiGivenName|niz|Fonetski japanski navedenog naziv (ime) kontakta.|
|YomiSurname|niz|Prezime fonetski Japanski (Prezime) kontakta|
|Kategorije|polja|Kategorije povezane s kontaktom.|
|ChangeKey|niz|Služi za identifikaciju verziju objekta događaja|
|DateTimeCreated|niz|Vrijeme je stvoren kontakt.|
|DateTimeLastModified|niz|Vrijeme izmjene kontakt.|


#### <a name="get-contact"></a>Dohvaćanje kontakata
Ovaj postupak može vidjeti određeni kontakt iz mape Kontakti. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id mape|Odaberite mapu s kontaktima|
|ID *|Id stavke|Jedinstveni identifikator kontakta za dohvaćanje|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Kontakt: kontakta

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|ID-a|niz|Jedinstveni identifikator kontakta.|
|ParentFolderId|niz|ID nadređene mape kontakta|
|Rođendan|niz|Rođendan kontakta.|
|FileAs|niz|U odjeljku spremljena ime kontakta.|
|Riješiti problem|niz|Zaslonsko ime kontakta.|
|GivenName|niz|Ime kontakta.|
|Inicijali|niz|Inicijale kontakta.|
|MiddleName|niz|Srednje ime kontakta.|
|Nadimak|niz|Nadimak kontakta.|
|Prezime|niz|Prezime kontakta.|
|Naslov|niz|Naslov kontakta.|
|Generiranje|niz|Generiranje kontakta.|
|EmailAddresses|polja|Adrese e-pošte kontakta.|
|ImAddresses|polja|Kontakta izravnih poruka (IM) adrese.|
|JobTitle|niz|Poslovna titula kontakta.|
|NazivTvrtke|niz|Naziv tvrtke kontakta.|
|Odjelu|niz|Odjel kontakta.|
|OfficeLocation|niz|Mjesto kontakta office.|
|Struka|niz|Struka kontakta.|
|BusinessHomePage|niz|Početna stranica business kontakta.|
|AssistantName|niz|Naziv pomoćnika kontakta.|
|Upravitelj|niz|Naziv Upravitelj kontakta.|
|HomePhones|polja|Brojevi Kućni telefon kontakta.|
|BusinessPhones|polja|Kontakta tvrtke telefonskih brojeva|
|MobilePhone1|niz|Broj mobilnog telefona kontakta.|
|HomeAddress|nije definirano|Kućna adresa kontakta.|
|BusinessAddress|nije definirano|Adresa tvrtke kontakta.|
|OtherAddress|nije definirano|Druge adrese za kontakt.|
|YomiCompanyName|niz|Ime fonetski japanski tvrtke kontakta.|
|YomiGivenName|niz|Fonetski japanski navedenog naziv (ime) kontakta.|
|YomiSurname|niz|Prezime fonetski Japanski (Prezime) kontakta|
|Kategorije|polja|Kategorije povezane s kontaktom.|
|ChangeKey|niz|Služi za identifikaciju verziju objekta događaja|
|DateTimeCreated|niz|Vrijeme je stvoren kontakt.|
|DateTimeLastModified|niz|Vrijeme izmjene kontakt.|


#### <a name="delete-contact"></a>Brisanje kontakta
Ovaj postupak briše kontakta iz mape Kontakti. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id mape|Odaberite mapu s kontaktima|
|ID *|ID-a|Jedinstveni identifikator kontakta da biste izbrisali|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Ništa.


#### <a name="update-contact"></a>Ažuriranje kontakta
Ovaj postupak ažuriranja kontakta u mapi Kontakti. 

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Id mape|Odaberite mapu s kontaktima|
|ID *|ID-a|Jedinstveni identifikator kontakta za ažuriranje|
|stavke *|Stavke|Stavci kontakta za ažuriranje|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Kontakt: kontakta

| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|ID-a|niz|Jedinstveni identifikator kontakta.|
|ParentFolderId|niz|ID nadređene mape kontakta|
|Rođendan|niz|Rođendan kontakta.|
|FileAs|niz|U odjeljku spremljena ime kontakta.|
|Riješiti problem|niz|Zaslonsko ime kontakta.|
|GivenName|niz|Ime kontakta.|
|Inicijali|niz|Inicijale kontakta.|
|MiddleName|niz|Srednje ime kontakta.|
|Nadimak|niz|Nadimak kontakta.|
|Prezime|niz|Prezime kontakta.|
|Naslov|niz|Naslov kontakta.|
|Generiranje|niz|Generiranje kontakta.|
|EmailAddresses|polja|Adrese e-pošte kontakta.|
|ImAddresses|polja|Kontakta izravnih poruka (IM) adrese.|
|JobTitle|niz|Poslovna titula kontakta.|
|NazivTvrtke|niz|Naziv tvrtke kontakta.|
|Odjelu|niz|Odjel kontakta.|
|OfficeLocation|niz|Mjesto kontakta office.|
|Struka|niz|Struka kontakta.|
|BusinessHomePage|niz|Početna stranica business kontakta.|
|AssistantName|niz|Naziv pomoćnika kontakta.|
|Upravitelj|niz|Naziv Upravitelj kontakta.|
|HomePhones|polja|Brojevi Kućni telefon kontakta.|
|BusinessPhones|polja|Kontakta tvrtke telefonskih brojeva|
|MobilePhone1|niz|Broj mobilnog telefona kontakta.|
|HomeAddress|nije definirano|Kućna adresa kontakta.|
|BusinessAddress|nije definirano|Adresa tvrtke kontakta.|
|OtherAddress|nije definirano|Druge adrese za kontakt.|
|YomiCompanyName|niz|Ime fonetski japanski tvrtke kontakta.|
|YomiGivenName|niz|Fonetski japanski navedenog naziv (ime) kontakta.|
|YomiSurname|niz|Prezime fonetski Japanski (Prezime) kontakta|
|Kategorije|polja|Kategorije povezane s kontaktom.|
|ChangeKey|niz|Služi za identifikaciju verziju objekta događaja|
|DateTimeCreated|niz|Vrijeme je stvoren kontakt.|
|DateTimeLastModified|niz|Vrijeme izmjene kontakt.|



## <a name="http-responses"></a>HTTP odgovora

Akcije i okidača iznad možete se vratiti jedan ili više sljedećih HTTP kodovima stanja: 

|Ime|Opis|
|---|---|
|200|ok|
|202|Prihvaćen|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="next-steps"></a>Daljnji koraci

[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md). Istražite ostale dostupne poveznika u logiku aplikacija u našem [popisu API-ji](apis-list.md).