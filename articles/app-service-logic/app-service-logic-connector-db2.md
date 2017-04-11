<properties
   pageTitle="Pomoću poveznika DB2 u servis za aplikaciju Microsoft Azure | Microsoft Azure"
   description="Kako pomoću poveznika DB2 pomoću okidača logike aplikacije i akcije"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="gplarsen"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="plarsen"/>

# <a name="db2-connector"></a>Poveznik za DB2
>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2014. – 12-01 – pregled shema verziju.

Microsoft poveznik za DB2 je aplikaciju API-JA za povezivanje aplikacije putem aplikacije servisa za Azure resurse pohranjene u bazom podataka IBM DB2. Poveznik obuhvaća Microsoft Client da biste se povezali s udaljenim računalima DB2 server putem mrežne veze TCP/IP, uključujući Azure hibridnog veze s lokalnim DB2 poslužitelji pomoću prijenos Bus servisa Azure poveznik podržava sljedeće postupke baze podataka:

- Odaberite retke čitanje pomoću
- Anketa da biste pročitali redaka pomoću odaberite COUNT slijedi odaberite
- Dodavanje jednog retka ili više redaka (skupno) koji se pomoću Umetanje
- Promijeniti jedan redak ili više redaka (skupno) koji se pomoću ažuriranja
- Uklanjanje jednog retka ili više redaka (skupno) koji se pomoću Izbriši
- Čitanje mijenjanja redaka pomoću odaberite POKAZIVAČ iza koje slijedi ažuriranje gdje trenutni od POKAZIVAČ
- Čitanje da biste uklonili redaka pomoću odaberite POKAZIVAČ iza koje slijedi ažuriranje gdje trenutni od POKAZIVAČ
- Pokrenite postupak s ulazni i izlazni parametra, vraća vrijednost SkupRezultata, pomoću POZIVA
- Prilagođene naredbe i složeni operacije pomoću odaberite Umetanje, ažuriranje, brisanje

## <a name="triggers-and-actions"></a>Okidača i akcija
Poveznik podržava sljedeće okidača logike aplikacije i akcije:

Okidača | Akcija
--- | ---
<ul><li>Ankete podataka</li></ul> | <ul><li>Umetanje skupno</li><li>Umetanje</li><li>Skupno ažuriranje</li><li>Ažuriranje</li><li>Poziv</li><li>Masovno brisanje</li><li>Brisanje</li><li>Odaberite</li><li>Uvjetno ažuriranja</li><li>Objava da biste EntitySet</li><li>Uvjetno Izbriši</li><li>Odaberite jedan entitet</li><li>Brisanje</li><li>Upsert za EntitySet</li><li>Prilagođene naredbe</li><li>Složeni operacije</li></ul>


## <a name="create-the-db2-connector"></a>Stvaranje DB2 poveznika
U sljedećem primjeru možete definirati poveznik unutar logike aplikacije ili iz trgovine Azure npr.:  

1. Azure startboard odaberite **trgovine**.
2. U plohu **sve** upišite **db2** u okviru **Pretraži sve** , a zatim pritisnite tipku enter.
3. U okvir pretražite sve rezultate okna, odaberite **DB2 poveznik**.
4. Opis plohu poveznik DB2 odaberite **Stvori**.
5. U paket plohu poveznik DB2 unesite naziv (primjerice "Db2ConnectorNewOrders"), Plan za aplikaciju servisa i druga svojstva.
6. Odaberite **postavke paketa**i unesite sljedeće postavke paketa:  

    Ime | Obavezno |  Opis
--- | --- | ---
ConnectionString | Da | Niz za povezivanje DB2 klijenta (npr., "mrežna adresa = naziv poslužitelja; Mrežni priključak = 50000; Korisnički ID = korisničko ime; Lozinka = lozinku; početnog kataloga = UZORKA; Pakiranje zbirke = NWIND; zadana sheme = NWIND ").
Tablica | Da | Popis s vrijednostima odvojenima zarezom tablicu, prikaz i pseudonim nazivi koji su potrebni za OData operacije, a da biste generirali swagger dokumentaciju primjerima (npr. "*NEWORDERS*").
Postupci | Da | Popis s vrijednostima odvojenima zarezom postupak i funkcija imena (npr. "SPORDERID").
Lokalnu verziju | ne | Implementacija lokalnog pomoću Azure preusmjeravanja Bus servisa.
ServiceBusConnectionString | ne | Azure preusmjeravanja servisa Bus niz za povezivanje.
PollToCheckData | ne | Odaberite broj izjava za uporabu okidač logike aplikacije (npr. "odaberite broj (\*) iz NEWORDERS gdje DATUMISPORUKE IS NULL").
PollToReadData | ne | Naredba SELECT za uporabu okidač logike aplikacije (npr. "odaberite \* iz NEWORDERS gdje DATUMISPORUKE je NULL za ažuriranje").
PollToAlterData | ne | Ažuriranje ili naredba DELETE za uporabu okidač logike aplikacije (npr. "Postavljanje DATUMISPORUKE NEWORDERS za ažuriranje = trenutni datum gdje trenutni od &lt;POKAZIVAČ&gt;").

7. Odaberite **u redu**, a zatim odaberite **Stvori**.
8. Po dovršetku postavke paketa izgleda otprilike ovako:  
![][1]


## <a name="logic-app-with-db2-connector-action-to-add-data"></a>Logika aplikacije akcijom DB2 poveznik za dodavanje podataka ##
Možete definirati logičku akciju aplikacije da biste dodali podatke tablice DB2 pomoću Umetanje API-JA ili objavu entitet OData operaciju. Ako, na primjer, možete umetnuti novi zapis narudžbe kupaca, za umetanje SQL izjava odnosu tablica definirana pomoću stupca identiteta vraćanja vrijednosti identiteta ili redaka aplikaciju logike (Odabir ORDID iz KONAČNE TABLICE (UMETNUTI u NWIND. NEWORDERS (IDKLIJENTA, IMEZAISPORUKU, SHIPADDR, GRADZAISPORUKU, SHIPREG, SHIPZIP) VRIJEDNOSTI (?,?,?,?,?,?))).

> [AZURE.TIP] DB2 vezu "*Objava da biste EntitySet*" vraća vrijednosti stupca identiteta i vraća "*Umetanje API -JA*" utječe na redaka

1. Azure startboard odaberite **+** (znak plus), **Web + Mobile**, a zatim **logike aplikacije**.
2. Unesite naziv (primjerice "NewOrdersDb2"), Plan za aplikaciju servisa, druga svojstva, a zatim odaberite **Stvori**.
3. U Azure startboard odaberite na logiku aplikaciju koju ste upravo stvorili, **Postavke**, a zatim **okidača i akcija**.
4. U plohu okidača i akcija odaberite **Stvaranje ispočetka** logike aplikacije predložaka.
5. Na ploči API aplikacije odaberite **Ponavljanje**, postavite na učestalost i interval, a zatim **potvrdite**.
6. Na ploči API aplikacije odaberite **DB2 poveznika**, proširite popis operacije da biste odabrali **umetnuli NEWORDER**.
7. Proširite popis parametara unesite sljedeće vrijednosti:  

    Ime | Vrijednost
--- | --- 
IDKLIJENTA | 10042
IMEZAISPORUKU | Spremište Kountry drži K 
SHIPADDR | 12 orkestra Terrace
GRADZAISPORUKU | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Odaberite **kvačicu** da biste spremili postavke Akcije, a zatim **Spremi**.
9. Postavke trebao bi izgledati ovako:  
![][3]

10. Na popisu **Svi izvodi** u odjeljku **operacija**odaberite stavku naveden na prvi (najnovija cilja). 
11. U plohu **logike aplikacije pokrenuti** odaberite stavku **AKCIJE** **db2connectorneworders**.
12. U plohu **logičku akciju aplikacije** odaberite **VEZU UNOSA**. Poveznik za DB2 koristi ulaza za obradu s parametrima naredbi INSERT.
13. U plohu **logičku akciju aplikacije** odaberite **IZLAZE VEZU**. Ulaza trebao bi izgledati ovako:  
![][4]

#### <a name="what-you-need-to-know"></a>Što trebate znati

- Poveznik skraćuje nazive tablica DB2 kada i logike aplikacije akcija imena. Ako, na primjer, operacija **umetnuli NEWORDERS** odbacuje da biste **umetnuli NEWORDER**.
- Kada spremite aplikaciju logike **okidača i akcija**, logike aplikacije obrađuje postupak. Možda postoje kašnjenje od određenog broja sekundi (primjerice 3 do 5 sekunde) prije logike aplikacije obrađuje postupak. Po želji, kliknite **Izvedi sada** za obradu operacije.
- Poveznik za DB2 definira EntitySet članovi atributa, uključujući li član koji odgovara stupcu DB2 je zadani ili generira stupaca (npr. identitet). Logika app prikazat će se crvena zvjezdica pokraj naziva ID člana EntitySet za označavanje DB2 stupce s obaveznim vrijednosti. Ne treba unesite vrijednost za člana ORDID koji odgovara stupcu DB2 identitet. Možda unesite vrijednosti za neobavezno članovima (STAVKI, ORDDATE, REQDATE, SHIPID, VOZARINA, SHIPCTRY), koje odgovaraju DB2 stupce sa zadanim vrijednostima. 
- Poveznik za DB2 vraća logike aplikaciju odgovor na Objavi u EntitySet koji sadrži vrijednosti za stupce identiteta, dobivena iz SQLDARD DRDA (SQL podataka područje odgovor podataka) na Pripremljeni iskaz SQL Umetanje. Poslužitelj DB2 vratite umetnute vrijednosti za stupce sa zadanim vrijednostima.  


## <a name="logic-app-with-db2-connector-action-to-add-bulk-data"></a>Logika aplikacije akcijom DB2 poveznik za masovno dodavanje podataka ##
Možete definirati logičku akciju aplikacije da biste dodali podacima DB2 tablice pomoću operacije API masovno Umetanje. Ako, na primjer, možete umetnuti dva nova kupca redoslijed zapisa, tako da obrade Umetanje SQL izjava pomoću polja retka vrijednosti na temelju tablice definirana pomoću stupca identiteta vraćanje redaka aplikaciju logike (Odabir ORDID iz KONAČNE TABLICE (UMETNUTI u NWIND. NEWORDERS (IDKLIJENTA, IMEZAISPORUKU, SHIPADDR, GRADZAISPORUKU, SHIPREG, SHIPZIP) VRIJEDNOSTI (?,?,?,?,?,?))).

1. Azure startboard odaberite **+** (znak plus), **Web + Mobile**, a zatim **logike aplikacije**.
2. Unesite naziv (primjerice "NewOrdersBulkDb2"), Plan za aplikaciju servisa, druga svojstva, a zatim odaberite **Stvori**.
3. U Azure startboard odaberite na logiku aplikaciju koju ste upravo stvorili, **Postavke**, a zatim **okidača i akcija**.
4. U plohu okidača i akcija odaberite **Stvaranje ispočetka** logike aplikacije predložaka.
5. Na ploči API aplikacije odaberite **Ponavljanje**, postavite na učestalost i interval, a zatim **potvrdite**.
6. Na ploči API aplikacije odaberite **DB2 poveznika**, proširite popis operacije da biste odabrali **Masovno Umetanje NOVO**.
7. Unesite vrijednost za **retke** kao polja. Na primjer, kopirajte i zalijepite sljedeće:

    ```
    [{"CUSTID":10081,"SHIPNAME":"Trail's Head Gourmet Provisioners","SHIPADDR":"722 DaVinci Blvd.","SHIPCITY":"Kirkland","SHIPREG":"WA","SHIPZIP":"98034"},{"CUSTID":10088,"SHIPNAME":"White Clover Markets","SHIPADDR":"305 14th Ave. S. Suite 3B","SHIPCITY":"Seattle","SHIPREG":"WA","SHIPZIP":"98128","SHIPCTRY":"USA"}]
    ```

8. Odaberite **kvačicu** da biste spremili postavke Akcije, a zatim **Spremi**. Postavke trebao bi izgledati ovako:  
![][6]

9. Na popisu **Svi izvodi** u odjeljku **operacija**kliknite stavku naveden na prvi (najnovija cilja).
10. Plohu **logike aplikacije pokrenuti** kliknite stavku **AKCIJE** .
11. U plohu **logičku aplikacije akciju** kliknite **VEZU UNOSA**. Na izlaze trebao bi izgledati ovako:  
[][7]
12. U plohu **logičku aplikacije akciju** kliknite **IZLAZE VEZU**. Na izlaze trebao bi izgledati ovako:  
![][8]

#### <a name="what-you-need-to-know"></a>Što trebate znati

- Poveznik skraćuje nazive tablica DB2 kada i logike aplikacije akcija imena. Ako, na primjer, operacija **Masovno Umetanje NEWORDERS** skraćuju se na **Skupno umetnuli novi**.
- Po ispuštanje identiteta stupaca (npr. ORDID), koji stupaca (npr. DATUMISPORUKE) i stupci sa zadanim vrijednostima (npr. ORDDATE, REQDATE, SHIPID, VOZARINA, SHIPCTRY), podataka DB2 generira vrijednosti.
- Određivanjem "Danas" i "sutra" DB2 poveznik generira "Trenutni datum" i "Trenutni datum + 1 dana" funkcije (npr. REQDATE). 


## <a name="logic-app-with-db2-connector-trigger-to-read-alter-or-delete-data"></a>Logika aplikaciju pomoću okidača poveznik DB2 za čitanje, izmjena ili brisanje podataka ##
Možete definirati okidač aplikacije logike ankete i čitati podatke iz tablice DB2 pomoću operacije složenih podataka ankete API-JA. Na primjer, možete pročitati jednu ili više novog klijenta redoslijed zapisa, vraćanje zapise aplikaciju logike. Postavke paketa aplikacije veze DB2 trebao bi izgledati na sljedeći način:

    Postavljanje aplikacije | Vrijednost
--- | --- | ---
PollToCheckData | Odaberite broj (\*) iz NEWORDERS gdje je NULL DATUMISPORUKE
PollToReadData | Odaberite \* iz NEWORDERS gdje je NULL za ažuriranje DATUMISPORUKE
PollToAlterData | <no value specified>


Osim toga, možete definirati okidač aplikacije logike ankete, čitati i mijenjati podatke u tablici DB2 pomoću operacije složenih podataka ankete API-JA. Na primjer, možete pročitati jednu ili više novi kupca redoslijed zapisa, ažurirati s vrijednostima retka povratkom odabranih (prije ažuriranje) zapisa na aplikaciju logike. Postavke paketa aplikacije veze DB2 trebao bi izgledati na sljedeći način:

    Postavljanje aplikacije | Vrijednost
--- | --- | ---
PollToCheckData | Odaberite broj (\*) iz NEWORDERS gdje je NULL DATUMISPORUKE
PollToReadData | Odaberite \* iz NEWORDERS gdje je NULL za ažuriranje DATUMISPORUKE
PollToAlterData | Ažuriranje NEWORDERS SKUP DATUMISPORUKE = DANAŠNJI datum gdje trenutni SLOBODE &lt;POKAZIVAČA&gt;


Nadalje, možete definirati okidač aplikacije logike ankete, čitati i uklanjanje podataka iz tablice DB2 pomoću operacije složenih podataka ankete API-JA. Na primjer, možete pročitati jednu ili više novi kupca redoslijed zapisa, izbrisati retke, vraćanje odabranog (prije brisanja) zapisa aplikaciju logike. Postavke paketa aplikacije veze DB2 trebao bi izgledati na sljedeći način:

    Postavljanje aplikacije | Vrijednost
--- | --- | ---
PollToCheckData | Odaberite broj (\*) iz NEWORDERS gdje je NULL DATUMISPORUKE
PollToReadData | Odaberite \* iz NEWORDERS gdje je NULL za ažuriranje DATUMISPORUKE
PollToAlterData | Brisanje NEWORDERS gdje trenutni SLOBODE &lt;POKAZIVAČA&gt;

U ovom primjeru logike aplikacija će ankete, čitanje, ažuriranje i ponovno čitanje podataka u tablici DB2.

1. Azure startboard odaberite **+** (znak plus), **Web + Mobile**, a zatim **logike aplikacije**.
2. Unesite naziv (primjerice "ShipOrdersDb2"), Plan za aplikaciju servisa, druga svojstva, a zatim odaberite **Stvori**.
3. U Azure startboard odaberite na logiku aplikaciju koju ste upravo stvorili, **Postavke**, a zatim **okidača i akcija**.
4. U plohu okidača i akcija odaberite **Stvaranje ispočetka** logike aplikacije predložaka.
5. Na ploči API aplikacije odaberite **DB2 poveznika**, postavite na učestalost i interval, a zatim **potvrdite**.
6. Na ploči API aplikacije odaberite **DB2 poveznika**, proširite popis operacije da biste odabrali **Odaberite NEWORDERS**.
7. Odaberite **kvačicu** da biste spremili postavke Akcije, a zatim **Spremi**. Postavke trebao bi izgledati ovako:  
![][10]  
8. Kliknite da biste zatvorili plohu **okidača i akcija** , a zatim kliknite da biste zatvorili plohu **Postavke** .
9. Na popisu **Svi izvodi** u odjeljku **operacija**kliknite stavku naveden na prvi (najnovija cilja).
10. Plohu **logike aplikacije pokrenuti** kliknite stavku **AKCIJE** .
11. U plohu **logičku aplikacije akciju** kliknite **IZLAZE VEZU**. Na izlaze trebao bi izgledati ovako:  
![][11]


## <a name="logic-app-with-db2-connector-action-to-remove-data"></a>Logika aplikacije s akcijom poveznik DB2 da biste uklonili podatke ##
Možete definirati logičku akciju aplikacije da biste uklonili podatke iz tablice DB2 pomoću API Izbriši ili objavu entitet OData operacija. Ako, na primjer, možete umetnuti novi zapis narudžbe kupaca, za umetanje SQL izjava odnosu tablica definirana pomoću stupca identiteta vraćanja vrijednosti identiteta ili redaka aplikaciju logike (Odabir ORDID iz KONAČNE TABLICE (UMETNUTI u NWIND. NEWORDERS (IDKLIJENTA, IMEZAISPORUKU, SHIPADDR, GRADZAISPORUKU, SHIPREG, SHIPZIP) VRIJEDNOSTI (?,?,?,?,?,?))).

## <a name="create-logic-app-using-db2-connector-to-remove-data"></a>Stvaranje aplikacije logike DB2 poveznika protokola da biste uklonili podatke ##
Možete stvoriti novu aplikaciju logike iz iz trgovine Azure i pomoću poveznika DB2 kao akciju da biste uklonili narudžbe korisnika. Na primjer, možete koristiti DB2 poveznik uvjetno postupak brisanja za obradu Brisanje SQL naredbu (izbrisati iz NEWORDERS gdje ORDID > = 10000).

1. Na izborniku koncentrator panela za **pokretanje** Azure kliknite **+** (znak plus), kliknite **Web + Mobile**, a zatim kliknite **logike aplikacije**. 
2. U plohu **aplikaciju za stvaranje logike** upišite **naziv**, primjerice **RemoveOrdersDb2**.
3. Odaberite ili definirajte vrijednosti za ostale postavke (npr. tarifa za servis, grupa resursa).
4. Postavke trebao bi izgledati ovako. Kliknite **Stvaranje**:  
![][12]  
5. U plohu **Postavke** kliknite **okidača i akcija**.
6. U plohu **okidača i akcija** , na popisu **logike aplikacije predlošci** kliknite **Stvori ispočetka**.
7. U plohu **okidača i akcija** , u oknu **Aplikacije API** unutar grupe resursa, kliknite **Ponavljanje**.
8. Na površini za dizajniranje aplikacije logike kliknite stavku **ponavljanja** , **Učestalost** i **Interval**, primjerice **dana** i **1**, postavite, a zatim kliknite **kvačicu** da biste spremili postavke stavke ponavljanja.
9. U plohu **okidača i akcija** , u oknu **Aplikacije API** unutar grupe resursa, kliknite **DB2 poveznik**.
10. Na površini za dizajniranje aplikacije logike kliknite stavku akcije **DB2 poveznika** , kliknite tri točke (****...) da biste proširili popis operacije pa kliknite **uvjetno izbrisati iz N**.
11. Na stavci DB2 poveznik akciju, upišite **ORDID Spoji 10000** **izraz koji određuje podskup stavki**.
12. Kliknite **kvačicu** da biste spremili postavke Akcije, a zatim kliknite **Spremi**. Postavke trebao bi izgledati ovako:  
![][13]  
13. Kliknite da biste zatvorili plohu **okidača i akcija** , a zatim kliknite da biste zatvorili plohu **Postavke** .
14. Na popisu **Svi izvodi** u odjeljku **operacija**kliknite stavku naveden na prvi (najnovija cilja).
15. Plohu **logike aplikacije pokrenuti** kliknite stavku **AKCIJE** .
16. U plohu **logičku aplikacije akciju** kliknite **IZLAZE VEZU**. Na izlaze trebao bi izgledati ovako:  
![][14]

**Bilješke:** Logika aplikacije dizajner skraćuje nazive tablica. Na primjer, operacija **uvjetno izbrisati iz NEWORDERS** skraćuju se na **uvjetno izbrisati iz N**.


> [AZURE.TIP] Koristite sljedeće SQL naredbe za stvaranje primjera tablice i pohranjene procedure. 

Možete stvoriti NEWORDERS ogledne tablice pomoću sljedećih DB2 SQL DDL naredbe:
 
    CREATE TABLE ORDERS (  
        ORDID INT NOT NULL GENERATED BY DEFAULT AS IDENTITY (START WITH 10000, INCREMENT BY 1) ,  
        CUSTID INT NOT NULL ,  
        EMPID INT NOT NULL DEFAULT 10000 ,  
        ORDDATE DATE NOT NULL DEFAULT CURRENT DATE ,  
        REQDATE DATE DEFAULT CURRENT DATE ,  
        SHIPDATE DATE ,  
        SHIPID INT NOT NULL DEFAULT 10000,  
        FREIGHT DECIMAL (9,2) NOT NULL DEFAULT 0.00 ,  
        SHIPNAME CHAR (40) NOT NULL ,  
        SHIPADDR CHAR (60) NOT NULL ,  
        SHIPCITY CHAR (20) NOT NULL ,  
        SHIPREG CHAR (15) NOT NULL ,  
        SHIPZIP CHAR (10) NOT NULL ,  
        SHIPCTRY CHAR (15) NOT NULL DEFAULT 'USA' ,  
        PRIMARY KEY(ORDID)  
        )  
 
    CREATE UNIQUE INDEX XORDID ON ORDERS (ORDID ASC)  



Možete stvoriti postupak SPOERID pohranjene uzorka pomoću sljedeće naredbe DB2 DDL:
 
    CREATE OR REPLACE PROCEDURE NWIND.SPORDERID (IN ORDERID VARCHAR(128))  
        DYNAMIC RESULT SETS 1  
    P1: BEGIN  
        DECLARE CURSOR1 CURSOR WITH RETURN FOR  
            SELECT * FROM NWIND.NEWORDERS  
                WHERE ORDID = ORDERID;  
        OPEN CURSOR1;  
    END P1  
    ') 


## <a name="hybrid-configuration-optional"></a>Hibridna konfiguracija (nije obavezno)

> [AZURE.NOTE] Ovaj korak potreban je samo ako koristite DB2 poveznik lokalnog iza vatrozida.

Pomoću upravitelja konfiguracije hibridnog aplikacije servisa za sigurno povezivanje s lokalnim sustavom. Ako poveznik koristi za lokalni IBM DB2 poslužitelja za Windows, potreban je Upravitelja veze hibridnog.

Pogledajte odjeljak [Korištenje upravitelja veze hibridnog](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Bolje iskorištavanje vaše poveznika
Sad kad je poveznik stvorili, možete ga dodati u tijeku rada poslovne logike aplikacija. U odjeljku [koje su aplikacije logike?](app-service-logic-what-are-logic-apps.md).

Stvaranje aplikacija API pomoću REST API-ji. U odjeljku [poveznika i API aplikacije Reference](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Možete pregledati i performanse Statistika i kontrola sigurnost da biste poveznik. Potražite u članku [Upravljanje i nadzirati ugrađene aplikacije API -JA i poveznike](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-db2/ApiApp_Db2Connector_Create.png
[2]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Create.png
[3]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_TriggersActions.png
[4]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Outputs.png
[5]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Create.png
[6]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_TriggersActions.png
[7]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Inputs.png
[8]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Outputs.png
[9]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Create.png
[10]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_TriggersActions.png
[11]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Outputs.png
[12]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Create.png
[13]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_TriggersActions.png
[14]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Outputs.png

