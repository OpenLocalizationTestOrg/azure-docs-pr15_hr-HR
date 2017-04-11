<properties
   pageTitle="Pomoću poveznika Informix u Microsoft Azure aplikacije servisa | Microsoft Azure"
   description="Kako pomoću poveznika Informix pomoću okidača logike aplikacije i akcije"
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

# <a name="informix-connector"></a>Poveznik za Informix
>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2014. – 12-01 – pregled shema verziju.

Microsoft poveznik za Informix je aplikaciju API-JA za povezivanje aplikacije putem aplikacije servisa za Azure resurse pohranjene u bazi podataka programa IBM Informix. Poveznik obuhvaća Microsoft Client da biste se povezali s udaljenim računalima Informix server putem mrežne veze TCP/IP, uključujući Azure hibridnog veze s lokalnim Informix poslužitelja koja koristi prijenos Bus servisa Azure. Poveznik podržava sljedeće postupke baze podataka:

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


## <a name="create-the-informix-connector"></a>Stvaranje poveznika Informix
U sljedećem primjeru možete definirati poveznik unutar logike aplikacije ili iz trgovine Azure npr.:  

1. Azure startboard odaberite **trgovine**.
2. U plohu **sve** **informix** upišite u okvir **Pretraži sve** , a zatim pritisnite tipku enter.
3. U okvir pretražite sve rezultate okna, odaberite **Informix poveznik**.
4. Opis plohu Informix poveznik odaberite **Stvori**.
5. U paket plohu poveznik Informix unesite naziv (primjerice "InformixConnectorNewOrders"), Plan za aplikaciju servisa i druga svojstva.
6. Odaberite **postavke paketa**, a zatim unesite sljedeće postavke paketa.

    Ime | Obavezno |  Opis
--- | --- | ---
ConnectionString | Da | Niz za povezivanje Informix klijenta (npr., "mrežna adresa = naziv poslužitelja; Mrežni priključak = 9089; Korisnički ID = korisničko ime; Lozinka = lozinku; početnog kataloga = nwind; zadana sheme = informix ").
Tablica | Da | Popis s vrijednostima odvojenima zarezom tablicu, prikaz i pseudonim nazivi koji su potrebni za OData operacije, a da biste generirali swagger dokumentaciju primjerima (npr. "NEWORDERS").
Postupci | Da | Popis s vrijednostima odvojenima zarezom postupak i funkcija imena (npr. "SPORDERID").
Lokalnu verziju | ne | Implementacija lokalnog pomoću Azure preusmjeravanja Bus servisa.
ServiceBusConnectionString | ne | Azure preusmjeravanja servisa Bus niz za povezivanje.
PollToCheckData | ne | Odaberite broj izjava za uporabu okidač logike aplikacije (npr. "odaberite broj (\*) iz NEWORDERS gdje DATUMISPORUKE IS NULL").
PollToReadData | ne | Naredba SELECT za uporabu okidač logike aplikacije (npr. "odaberite \* iz NEWORDERS gdje DATUMISPORUKE je NULL za ažuriranje").
PollToAlterData | ne | Ažuriranje ili naredba DELETE za uporabu okidač logike aplikacije (npr. "Postavljanje DATUMISPORUKE NEWORDERS za ažuriranje = trenutni datum gdje trenutni od &lt;POKAZIVAČ&gt;").

7. Odaberite **u redu**, a zatim odaberite **Stvori**.
8. Po dovršetku postavke paketa izgleda otprilike ovako:  
![][1]


## <a name="logic-app-with-informix-connector-action-to-add-data"></a>Logika aplikacije akcijom Informix poveznik za dodavanje podataka ##
Možete definirati logičku akciju aplikacije da biste dodali podatke tablice programa Informix pomoću Umetanje API-JA ili objavu entitet OData operacija. Na primjer, možete umetnuti novi zapis narudžbe kupaca, za umetanje SQL izjava odnosu tablica definirana pomoću stupca identiteta vraćanja vrijednosti identiteta ili redaka aplikaciju logike (Odabir ORDID iz KONAČNE TABLICE (UMETNUTI u NEWORDERS (IDKLIJENTA, IMEZAISPORUKU, SHIPADDR, GRADZAISPORUKU, SHIPREG, SHIPZIP) vrijednosti (?,?,?,?,?,?))).

> [AZURE.TIP] Informix veze "*Objava da biste EntitySet*" vraća vrijednosti stupca identiteta i vraća "*Umetanje API -JA*" utječe na redaka

1. Azure startboard odaberite **+** (znak plus), **Web + Mobile**, a zatim **logike aplikacije**.
2. Unesite naziv (primjerice "NewOrdersInformix"), Plan za aplikaciju servisa, druga svojstva, a zatim odaberite **Stvori**.
3. U Azure startboard odaberite na logiku aplikaciju koju ste upravo stvorili, **Postavke**, a zatim **okidača i akcija**.
4. U plohu okidača i akcija odaberite **Stvaranje ispočetka** logike aplikacije predložaka.
5. Na ploči API aplikacije odaberite **Ponavljanje**, postavite na učestalost i interval, a zatim **potvrdite**.
6. Na ploči API aplikacije odaberite **Informix poveznika**, proširite popis operacije da biste odabrali **umetnuli NEWORDER**.
7. Proširite popis parametara unesite sljedeće vrijednosti:  

    Ime | Vrijednost
--- | --- 
IDKLIJENTA | 10042
SHIPID | 10000
IMEZAISPORUKU | Spremište Kountry drži K 
SHIPADDR | 12 orkestra Terrace
GRADZAISPORUKU | Walla Walla 
SHIPREG | WA
SHIPZIP | 99362 

8. Odaberite **kvačicu** da biste spremili postavke Akcije, a zatim **Spremi**.
9. Postavke trebao bi izgledati ovako:  
![][3]  
10. Na popisu **Svi izvodi** u odjeljku **operacija**odaberite stavku naveden na prvi (najnovija cilja). 
11. U plohu **logike aplikacije pokrenuti** odaberite stavku **AKCIJE** **informixconnectorneworders**.
12. U plohu **logičku akciju aplikacije** odaberite **VEZU UNOSA**. Poveznik za Informix koristi ulaza za obradu s parametrima naredbi INSERT.
13. U plohu **logičku akciju aplikacije** odaberite **IZLAZE VEZU**. Ulaza trebao bi izgledati ovako:  
![][4]

#### <a name="what-you-need-to-know"></a>Što trebate znati

- Poveznik skraćuje nazive tablica Informix kada i logike aplikacije akcija imena. Ako, na primjer, operacija **umetnuli NEWORDERS** odbacuje da biste **umetnuli NEWORDER**.
- Kada spremite aplikaciju logike **okidača i akcija**, logike aplikacije obrađuje postupak. Možda postoje kašnjenje od određenog broja sekundi (primjerice 3 do 5 sekunde) prije logike aplikacije obrađuje postupak. Po želji, kliknite **Izvedi sada** za obradu operacije.
- Poveznik za Informix definira EntitySet članovi atributa, uključujući li član koji odgovara stupca Informix je zadani ili generira stupaca (npr. identitet). Logika app prikazat će se crvena zvjezdica pokraj naziva ID člana EntitySet za označavanje Informix stupce s obaveznim vrijednosti. Ne treba unesite vrijednost za člana ORDID koji odgovara Informix identiteta stupca. Možda unesite vrijednosti za neobavezno članovima (STAVKI, ORDDATE, REQDATE, SHIPID, VOZARINA, SHIPCTRY), koje odgovaraju Informix stupaca sa zadanim vrijednostima. 
- Poveznik za Informix vraća logike aplikaciju odgovor na Objavi u EntitySet koji sadrži vrijednosti za stupce identiteta, dobivena iz SQLDARD DRDA (SQL podataka područje odgovor podataka) na Pripremljeni iskaz SQL Umetanje. Poslužitelj za Informix vratio umetnute vrijednosti za stupce sa zadanim vrijednostima.  


## <a name="logic-app-with-informix-connector-action-to-add-bulk-data"></a>Logika aplikacije akcijom Informix poveznik za masovno dodavanje podataka ##
Možete definirati logičku akciju aplikacije da biste dodali podatke u tablicu Informix pomoću operacije API masovno Umetanje. Ako, na primjer, možete umetnuti dva nova kupca redoslijed zapisa, tako da obrade Umetanje SQL izjava pomoću polja retka vrijednosti na temelju tablice definirana pomoću stupca identiteta vraćanje redaka aplikaciju logike (Odabir ORDID iz KONAČNE TABLICE (UMETNUTI u NEWORDERS (IDKLIJENTA, IMEZAISPORUKU, SHIPADDR, GRADZAISPORUKU, SHIPREG, SHIPZIP) vrijednosti (?,?,?,?,?,?))).

1. Azure startboard odaberite **+** (znak plus), **Web + Mobile**, a zatim **logike aplikacije**.
2. Unesite naziv (primjerice "NewOrdersBulkInformix"), Plan za aplikaciju servisa, druga svojstva, a zatim odaberite **Stvori**.
3. U Azure startboard odaberite na logiku aplikaciju koju ste upravo stvorili, **Postavke**, a zatim **okidača i akcija**.
4. U plohu okidača i akcija odaberite **Stvaranje ispočetka** logike aplikacije predložaka.
5. Na ploči API aplikacije odaberite **Ponavljanje**, postavite na učestalost i interval, a zatim **potvrdite**.
6. Na ploči API aplikacije odaberite **Informix poveznika**, proširite popis operacije da biste odabrali **Masovno Umetanje NOVO**.
7. Unesite vrijednost za **retke** kao polja. Na primjer, kopirajte i zalijepite sljedeće:  

    ```
    [{"custid":10081,"shipid":10000,"shipname":"Trail's Head Gourmet Provisioners","shipaddr":"722 DaVinci Blvd.","shipcity":"Kirkland","shipreg":"WA","shipzip":"98034"},{"custid":10088,"shipid":10000,"shipname":"White Clover Markets","shipaddr":"305 14th Ave. S. Suite 3B","shipcity":"Seattle","shipreg":"WA","shipzip":"98128","shipctry":"USA"}]
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

- Poveznik skraćuje nazive tablica Informix kada i logike aplikacije akcija imena. Ako, na primjer, operacija **Masovno Umetanje NEWORDERS** skraćuju se na **Skupno umetnuli novi**.
- Možda je baze podataka tvrtke Informix velika i mala slova u nazive tablica i stupaca. Ako, na primjer, umetanje skupno operacija polja nazive stupaca možda će biti potrebno da bude navedena u mala slova ("IdKlijenta") umjesto pisani velikim slovima ("IDKLIJENTA").
- Po ispuštanje stupce identiteta (npr. ORDID), koji stupaca (npr. DATUMISPORUKE) i stupce sa zadanim vrijednostima (npr. ORDDATE, REQDATE, SHIPID, VOZARINA, SHIPCTRY), baze podataka tvrtke Informix generira vrijednosti.
- Navođenjem "Danas" i "sutra" Informix poveznik generira "Trenutni datum" i "Trenutni datum + 1 dana" funkcije (npr. REQDATE). 


## <a name="logic-app-with-informix-connector-trigger-to-read-alter-or-delete-data"></a>Logika aplikaciju pomoću okidača poveznik Informix za čitanje, izmjena ili brisanje podataka ##
Možete definirati okidač aplikacije logike ankete i čitanje podataka iz tablice Informix pomoću operacije složenih podataka ankete API-JA. Na primjer, možete pročitati jednu ili više novog klijenta redoslijed zapisa, vraćanje zapise aplikaciju logike. Postavke paketa aplikacije Informix veze trebao bi izgledati ovako:

    Postavljanje aplikacije | Vrijednost
--- | --- | ---
PollToCheckData | Odaberite broj (\*) iz NEWORDERS gdje je NULL DATUMISPORUKE
PollToReadData | Odaberite \* iz NEWORDERS gdje je NULL za ažuriranje DATUMISPORUKE
PollToAlterData | <no value specified>


Osim toga, možete definirati okidač aplikacije logike ankete, čitati i mijenjati podatke u tablici programa Informix pomoću operacije složenih podataka ankete API-JA. Na primjer, možete pročitati jednu ili više novi kupca redoslijed zapisa, ažurirati s vrijednostima retka povratkom odabranih (prije ažuriranje) zapisa na aplikaciju logike. Postavke paketa aplikacije Informix veze trebao bi izgledati ovako:

    Postavljanje aplikacije | Vrijednost
--- | --- | ---
PollToCheckData | Odaberite broj (\*) iz NEWORDERS gdje je NULL DATUMISPORUKE
PollToReadData | Odaberite \* iz NEWORDERS gdje je NULL za ažuriranje DATUMISPORUKE
PollToAlterData | Ažuriranje NEWORDERS SKUP DATUMISPORUKE = DANAŠNJI datum gdje trenutni SLOBODE &lt;POKAZIVAČA&gt;


Nadalje, možete definirati okidač aplikacije logike ankete, čitati i uklanjanje podataka iz tablice Informix pomoću operacije složenih podataka ankete API-JA. Na primjer, možete pročitati jednu ili više novi kupca redoslijed zapisa, izbrisati retke, vraćanje odabranog (prije brisanja) zapisa aplikaciju logike. Postavke paketa aplikacije Informix veze trebao bi izgledati ovako:

    Postavljanje aplikacije | Vrijednost
--- | --- | ---
PollToCheckData | Odaberite broj (\*) iz NEWORDERS gdje je NULL DATUMISPORUKE
PollToReadData | Odaberite \* iz NEWORDERS gdje je NULL za ažuriranje DATUMISPORUKE
PollToAlterData | Brisanje NEWORDERS gdje trenutni SLOBODE &lt;POKAZIVAČA&gt;

U ovom primjeru logike aplikacija će ankete, čitanje, ažuriranje i ponovno čitanje podataka u tablici Informix.

1. Azure startboard odaberite **+** (znak plus), **Web + Mobile**, a zatim **logike aplikacije**.
2. Unesite naziv (primjerice "ShipOrdersInformix"), Plan za aplikaciju servisa, druga svojstva, a zatim odaberite **Stvori**.
3. U Azure startboard odaberite na logiku aplikaciju koju ste upravo stvorili, **Postavke**, a zatim **okidača i akcija**.
4. U plohu okidača i akcija odaberite **Stvaranje ispočetka** logike aplikacije predložaka.
5. Na ploči API aplikacije odaberite **Informix poveznika**, postavite na učestalost i interval, a zatim **potvrdite**.
6. Na ploči API aplikacije odaberite **Informix poveznika**, proširite popis operacije da biste odabrali **Odaberite NEWORDERS**.
7. Odaberite **kvačicu** da biste spremili postavke Akcije, a zatim **Spremi**. Postavke trebao bi izgledati ovako:  
![][10]
8. Kliknite da biste zatvorili plohu **okidača i akcija** , a zatim kliknite da biste zatvorili plohu **Postavke** .
9. Na popisu **Svi izvodi** u odjeljku **operacija**kliknite stavku naveden na prvi (najnovija cilja).
10. Plohu **logike aplikacije pokrenuti** kliknite stavku **AKCIJE** .
11. U plohu **logičku aplikacije akciju** kliknite **IZLAZE VEZU**. Na izlaze trebao bi izgledati ovako:  
![][11]


## <a name="logic-app-with-informix-connector-action-to-remove-data"></a>Logika aplikacije s akcijom poveznik Informix da biste uklonili podatke ##
Možete definirati logičku akciju aplikacije da biste uklonili podatke iz tablice Informix pomoću API Izbriši ili objavu entitet OData operacija. Na primjer, možete umetnuti novi zapis narudžbe kupaca, za umetanje SQL izjava odnosu tablica definirana pomoću stupca identiteta vraćanja vrijednosti identiteta ili redaka aplikaciju logike (Odabir ORDID iz KONAČNE TABLICE (UMETNUTI u NEWORDERS (IDKLIJENTA, IMEZAISPORUKU, SHIPADDR, GRADZAISPORUKU, SHIPREG, SHIPZIP) vrijednosti (?,?,?,?,?,?))).

## <a name="create-logic-app-using-informix-connector-to-remove-data"></a>Stvaranje aplikacije logike Informix poveznika protokola da biste uklonili podatke ##
Možete stvoriti novu aplikaciju logike iz iz trgovine Azure i pomoću poveznika Informix kao akciju da biste uklonili narudžbe korisnika. Na primjer, možete koristiti Informix poveznik uvjetno postupak brisanja za obradu Brisanje SQL naredbu (izbrisati iz NEWORDERS gdje ORDID > = 10000).

1. Na izborniku koncentrator panela za **pokretanje** Azure kliknite **+** (znak plus), kliknite **Web + Mobile**, a zatim kliknite **logike aplikacije**. 
2. U plohu **aplikaciju za stvaranje logike** upišite **naziv**, primjerice **RemoveOrdersInformix**.
3. Odaberite ili definirajte vrijednosti za ostale postavke (npr. tarifa za servis, grupa resursa).
4. Postavke trebao bi izgledati ovako. Kliknite **Stvaranje**:  
![][12]
5. U plohu **Postavke** kliknite **okidača i akcija**.
6. U plohu **okidača i akcija** , na popisu **logike aplikacije predlošci** kliknite **Stvori ispočetka**.
7. U plohu **okidača i akcija** , u oknu **Aplikacije API** unutar grupe resursa, kliknite **Ponavljanje**.
8. Na površini za dizajniranje aplikacije logike kliknite stavku **ponavljanja** , **Učestalost** i **Interval**, primjerice **dana** i **1**, postavite, a zatim kliknite **kvačicu** da biste spremili postavke stavke ponavljanja.
9. U plohu **okidača i akcija** , u oknu **Aplikacije API** unutar grupe resursa, kliknite **Informix poveznik**.
10. Na površini za dizajniranje aplikacije logike kliknite stavku akcije **Informix poveznika** , kliknite tri točke (****...) da biste proširili popis operacije pa kliknite **uvjetno izbrisati iz N**.
11. Na stavku Akcije poveznik Informix upišite **ordid Spoji 10000** za **izraz koji određuje podskup stavki**.
12. Kliknite **kvačicu** da biste spremili postavke Akcije, a zatim kliknite **Spremi**. Postavke trebao bi izgledati ovako:  
![][13]
13. Kliknite da biste zatvorili plohu **okidača i akcija** , a zatim kliknite da biste zatvorili plohu **Postavke** .
14. Na popisu **Svi izvodi** u odjeljku **operacija**kliknite stavku naveden na prvi (najnovija cilja).
15. Plohu **logike aplikacije pokrenuti** kliknite stavku **AKCIJE** .
16. U plohu **logičku aplikacije akciju** kliknite **IZLAZE VEZU**. Na izlaze trebao bi izgledati ovako:  
![][14]

**Bilješke:** Logika aplikacije dizajner skraćuje nazive tablica. Primjer operacije **uvjetno izbrisati iz NEWORDERS** skraćuju se na **uvjetno izbrisati iz N**.


> [AZURE.TIP] Koristite sljedeće SQL naredbe za stvaranje primjera tablice i pohranjene procedure. 

Možete stvoriti NEWORDERS ogledne tablice pomoću sljedećih Informix SQL DDL naredbe:
 
    create table neworders (  
        ordid serial(10000) unique ,  
        custid int not null ,  
        empid int not null default 10000 ,  
        orddate date not null default today ,  
        reqdate date default today ,  
        shipdate date ,  
        shipid int not null default 10000 ,  
        freight decimal (9,2) not null default 0.00 ,  
        shipname char (40) not null ,  
        shipaddr char (60) not null ,  
        shipcity char (20) not null ,  
        shipreg char (15) not null ,  
        shipzip char (10) not null ,  
        shipctry char (15) not null default ''USA'' 
        )


Možete stvoriti postupak SPORDERID pohranjene uzorka pomoću sljedeće naredbe Informix DDL:
 
    create procedure sporderid ( ord_id int)  
        returning int, int, int, date, date, date, int, decimal (9,2), char (40), char (60), char (20), char (15), char (10), char (15)  
        define xordid, xcustid, xempid, xshipid int;  
        define xorddate, xreqdate, xshipdate date;  
        define xfreight decimal (9,2);  
        define xshipname char (40);  
        define xshipaddr char (60);  
        define xshipcity char (20);  
        define xshipreg, xshipctry char (15);  
        define xshipzip char (10);  
        select ordid, custid, empid, orddate, reqdate, shipdate, shipid, freight, shipname, shipaddr, shipcity, shipreg, shipzip, shipctry  
            into xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry  
            from neworders where ordid = ord_id;  
        return xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry;  
    end procedure; 


## <a name="hybrid-configuration-optional"></a>Hibridna konfiguracija (neobavezno)

> [AZURE.NOTE] Ovaj korak potreban je samo ako koristite DB2 poveznik lokalnog iza vatrozida.

Pomoću upravitelja konfiguracije hibridnog aplikacije servisa za sigurno povezivanje s lokalnim sustavom. Ako poveznik koristi za lokalni IBM DB2 poslužitelja za Windows, potreban je Upravitelja veze hibridnog.

Pogledajte odjeljak [Korištenje upravitelja veze hibridnog](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>Bolje iskorištavanje vaše poveznika
Sad kad je poveznik stvorili, možete ga dodati u tijeku rada poslovne logike aplikacija. U odjeljku [koje su aplikacije logike?](app-service-logic-what-are-logic-apps.md).

Stvaranje aplikacija API pomoću REST API-ji. U odjeljku [poveznika i API aplikacije Reference](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Možete pregledati i performanse Statistika i kontrola sigurnost da biste poveznik. Potražite u članku [Upravljanje i nadzirati ugrađene aplikacije API -JA i poveznike](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-informix/ApiApp_InformixConnector_Create.png
[2]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Create.png
[3]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_TriggersActions.png
[4]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Outputs.png
[5]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Create.png
[6]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_TriggersActions.png
[7]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Inputs.png
[8]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Outputs.png
[9]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Create.png
[10]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_TriggersActions.png
[11]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Outputs.png
[12]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Create.png
[13]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_TriggersActions.png
[14]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Outputs.png


