<properties
    pageTitle="Dodavanje poveznika DB2 u aplikacijama za logiku | Microsoft Azure"
    description="Pregled poveznik DB2 s parametrima REST API-JA"
    services=""
    documentationCenter="" 
    authors="gplarsen"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/26/2016"
   ms.author="plarsen"/>


# <a name="get-started-with-the-db2-connector"></a>Početak rada s DB2 poveznika
Microsoft poveznik za DB2 povezuje logike aplikacije resurse pohranjene u bazom podataka IBM DB2. Ovaj poveznik obuhvaća klijentski možete komunicirati s udaljenog računala poslužitelja DB2 putem mreže TCP/IP. To obuhvaća oblaka baze podataka, kao što su IBM Bluemix dashDB ili IBM DB2 za Windows koji se izvodi u Azure virtualizacije i lokalne baze podataka koje koriste pristupnik lokalnim podacima. Pogledajte na [podržani popis](connectors-create-api-db2.md#supported-db2-platforms-and-versions) IBM DB2 platforme i verzija (u ovoj temi).

>[AZURE.NOTE] Ovu verziju članka primjenjuje se na logike aplikacije Općenito dostupan (GA). 

Poveznik za DB2 podržava sljedeće postupke baze podataka:

- Popis tablica baze podataka
- Čitanje pomoću odaberite jedan redak
- Čitanje svih redaka pomoću odaberite
- Dodavanje jednog retka pomoću Umetanje
- Korištenje ažuriranje ALTER jedan redak
- Uklanjanje jednog retka pomoću Izbriši

U ovoj se temi objašnjava korištenje poveznik u aplikaciji logike za proces postupaka baze podataka.

Dodatne informacije o aplikacijama logike potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="available-actions"></a>Dostupne akcije
Poveznik za DB2 podržava sljedeće radnje logike aplikacije:

- GetTables
- GetRow
- GetRows
- InsertRow
- UpdateRow
- DeleteRow


## <a name="list-tables"></a>Popis tablica
Stvaranje aplikacije logike za sve operacije sastoji se od Communicator izvršiti putem portala sustava Microsoft Azure.

U aplikaciji logike možete dodati akciju popisa tablica u bazi podataka DB2. Akcija upućuje poveznik za obradu naredbe sheme DB2, kao što su `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Stvaranje logike aplikacije
1.  **Azure pokretanje ploče**, odaberite **+** (znak plus), **Web + Mobile**, a zatim **Logike aplikacije**.
2.  Unesite **naziv**, kao što su `Db2getTables`, **pretplatu**, **grupa resursa**, **mjesto**i **Planiranje usluga aplikacije**. Odaberite **Prikvači na nadzornu ploču**, a zatim odaberite **Stvori**.

### <a name="add-a-trigger-and-action"></a>Dodavanje okidača i akcija
1.  U **Dizajner logike aplikacije**odaberite **Prazan LogicApp** **predložaka** popisa.
2.  Na popisu **okidača** odaberite **Ponavljanje**. 
3.  U okidača **ponavljanja** odaberite **Uredi**odaberite **Učestalost** padajućeg popisa odaberite **dan**, a zatim postavljanje **intervala** da upišete **7**.  
4.  Potvrdite okvir **+ Novi korak** , a zatim odaberite **Dodaj akciju**.
5.  Na popisu **Akcija** upišite `db2` u okvir **pretraživanje unesite dodatne akcije** dijaloški okvir Uređivanje, a zatim odaberite **DB2 - biste tablica (pretpregled)**.

    ![](./media/connectors-create-api-db2/Db2connectorActions.png)  

6.  U oknu za konfiguraciju **DB2 - biste tablica** odaberite **potvrdni okvir** da biste omogućili **Povezivanje putem pristupnika lokalnim podacima**. Imajte na umu da postavke promijeniti iz oblaka za lokalne.
    - Unesite vrijednost za **poslužitelj**u obliku adresu ili pseudonim broj priključka zarez. Ako, na primjer, upišite `ibmserver01:50000`.
    - Unesite vrijednost za **bazu podataka**. Ako, na primjer, upišite `nwind`.
    - Odaberite vrijednosti za **provjeru autentičnosti**. Na primjer, odaberite **osnovne**.
    - Unesite vrijednost za **korisničko ime**. Ako, na primjer, upišite `db2admin`.
    - Unesite vrijednost za **lozinke**. Ako, na primjer, upišite `Password1`.
    - Odaberite vrijednost za **pristupnik**. Na primjer, odaberite **datagateway01**.
7. Odaberite **Stvori**, a zatim odaberite **Spremi**. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

8.  Odabir stavke prve naveden (najnovija cilja) u plohu **Db2getTables** , unutar popisa **sve izvodi** u odjeljku **Sažetak**.
9.  U plohu **logike aplikacije pokrenuti** odaberite **Pokreni pojedinosti**. Na popisu **Akcija** odaberite **Get_tables**. Potražite u članku vrijednost za **Stanje**koje treba **je uspio**. Odaberite **unosa vezu** da biste pogledali ulaza. Odaberite **Šalje vezu**, a pogledajte izlaze; koji treba obuhvaćaju popis tablica.

    ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>Stvaranje veze
Ovaj poveznik podržava veze baza podataka nalazi na lokalni i u oblak pomoću sljedećih svojstva veze. 

Svojstvo | Opis
--- | ---
poslužitelj | Obavezan. Prihvaća vrijednost niza koji predstavlja TCP/IP adrese ili pseudonim u obliku IPv4 i IPv6, nakon čega (dvotočka zarezom), broj priključka TCP/IP. 
baze podataka | Obavezan. Prihvaća niz vrijednost koja predstavlja na DRDA relacijske baze podataka naziv (RDBNAM). DB2 za z-OS prihvaća 16-bajtni niza (baze podataka se naziva IBM DB2 za mjesto z-OS). DB2 za i5/OS prihvaća do 18 bajta niz (baza podataka IBM DB2 za naziva i relacijske baze podataka). DB2 za LUW prihvaća u 8-bajtnim niz.
Provjera autentičnosti | Neobavezno. Prihvaća stavke vrijednosti na popis, Basic i Windows (kerberos). 
korisničko ime | Obavezan. Prihvaća vrijednost niza. DB2 za z-OS prihvaća u 8-bajtnim niz. DB2 za i prihvaća 10-bajtni niz. DB2 Linux ili UNIX prihvaća u 8-bajtnim niz. DB2 za Windows prihvaća 30-bajtni niz.
lozinke | Obavezan. Prihvaća vrijednost niza.
pristupnik | Obavezan. Prihvaća vrijednost stavke popisa koji predstavlja pristupnik za lokalnim podacima definirani logike aplikacije u grupi za pohranu.  

## <a name="create-the-on-premises-gateway-connection"></a>Stvaranje pristupnika veze s lokalnim
Ovaj poveznik možete pristupiti lokalnog DB2 bazi podataka programa pomoću pristupnika lokalnog. Potražite u temama pristupnika dodatne informacije. 

1. U oknu za konfiguraciju **pristupnika** odaberite **potvrdni okvir** da biste omogućili **Povezivanje putem pristupnika**. Imajte na umu da postavke promijeniti iz oblaka za lokalne.
2. Unesite vrijednost za **poslužitelj**u obliku adresu ili pseudonim broj priključka zarez. Ako, na primjer, upišite `ibmserver01:50000`.
3. Unesite vrijednost za **bazu podataka**. Ako, na primjer, upišite `nwind`.
4. Odaberite vrijednosti za **provjeru autentičnosti**. Na primjer, odaberite **osnovne**.
5. Unesite vrijednost za **korisničko ime**. Ako, na primjer, upišite `db2admin`.
6. Unesite vrijednost za **lozinke**. Ako, na primjer, upišite `Password1`.
7. Odaberite vrijednost za **pristupnik**. Na primjer, odaberite **datagateway01**.
8. Odaberite **Stvori** da biste nastavili. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>Stvaranje veza s oblakom
Ovaj poveznik možete pristupiti oblaka DB2 baze podataka. 

1. U oknu za konfiguraciju **pristupnika** ostavite **okvir** onemogućene (nekliknutih) **Povezivanje putem pristupnika**. 
2. Unesite vrijednost za **naziv veze**. Ako, na primjer, upišite `hisdemo2`.
3. Unesite vrijednost za **naziv poslužitelja DB2**, u obliku adresu ili pseudonim broj priključka zarez. Ako, na primjer, upišite `hisdemo2.cloudapp.net:50000`.
3. Unesite vrijednost za **naziv DB2 baze podataka**. Ako, na primjer, upišite `nwind`.
4. Unesite vrijednost za **korisničko ime**. Ako, na primjer, upišite `db2admin`.
5. Unesite vrijednost za **lozinke**. Ako, na primjer, upišite `Password1`.
6. Odaberite **Stvori** da biste nastavili. 

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Dohvaćanje svih redaka pomoću odaberite
Možete definirati logičku akciju aplikacije da bi dohvatili sve retke u tablici DB2. To upućuje poveznik za obradu DB2 odaberite naredbu, kao što su `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Stvaranje logike aplikacije
1.  **Azure pokretanje ploče**, odaberite **+** (znak plus), **Web + Mobile**, a zatim **Logike aplikacije**.
2.  Unesite **naziv**, kao što su `Db2getRows`, **pretplatu**, **grupa resursa**, **mjesto**i **Planiranje usluga aplikacije**. Odaberite **Prikvači na nadzornu ploču**, a zatim odaberite **Stvori**.

### <a name="add-a-trigger-and-action"></a>Dodavanje okidača i akcija
1.  U **Dizajner logike aplikacije**odaberite **Prazan LogicApp** **predložaka** popisa.
2.  Na popisu **okidača** odaberite **Ponavljanje**. 
3.  U okidača **ponavljanja** odaberite **Uređivanje**, odaberite **Učestalost** padajućeg popisa odaberite **dan**, a zatim **Interval** da upišete **7**. 
4.  Potvrdite okvir **+ Novi korak** , a zatim odaberite **Dodaj akciju**.
5.  Na popisu **Akcija** upišite `db2` u okvir **pretraživanje unesite dodatne akcije** dijaloški okvir Uređivanje, a zatim odaberite **DB2 - dobiti redaka (pretpregled)**.
6. U akciji **dobiti redaka (pretpregled)** odaberite **vezu za promjenu**.
7. U oknu za konfiguriranje **veze** odaberite **Stvori novi**. 

    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
  
8. U oknu za konfiguraciju **pristupnika** ostavite **okvir** onemogućene (nekliknutih) **Povezivanje putem pristupnika**.
    - Unesite vrijednost za **naziv veze**. Ako, na primjer, upišite `HISDEMO2`.
    - Unesite vrijednost za **naziv poslužitelja DB2**, u obliku adresu ili pseudonim broj priključka zarez. Ako, na primjer, upišite `HISDEMO2.cloudapp.net:50000`.
    - Unesite vrijednost za **naziv DB2 baze podataka**. Ako, na primjer, upišite `NWIND`.
    - Unesite vrijednost za **korisničko ime**. Ako, na primjer, upišite `db2admin`.
    - Unesite vrijednost za **lozinke**. Ako, na primjer, upišite `Password1`.
9. Odaberite **Stvori** da biste nastavili.

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

10. Na popisu **naziv tablice** odaberite na **strelicu prema dolje**, a zatim **PODRUČJE**.
11. Po želji odaberite **Prikaži dodatne mogućnosti** da biste odredili mogućnosti upita.
12. Odaberite **Spremi**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)

13. Odabir stavke prve naveden (najnovija cilja) u plohu **Db2getRows** , unutar popisa **sve izvodi** u odjeljku **Sažetak**.
14. U plohu **logike aplikacije pokrenuti** odaberite **Pokreni pojedinosti**. Na popisu **Akcija** odaberite **Get_rows**. Potražite u članku vrijednost za **Stanje**koje treba **je uspio**. Odaberite **unosa vezu** da biste pogledali ulaza. Odaberite **Šalje vezu**, a pogledajte izlaze; koje treba uključivati popis redaka.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Dodavanje jednog retka pomoću Umetanje
Možete definirati logičku akciju aplikacije da biste dodali jedan redak u tablici DB2. Ova akcija upućuje poveznik za obradu DB2 Umetanje naredbe, kao što su `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Stvaranje logike aplikacije
1.  **Azure pokretanje ploče**, odaberite **+** (znak plus), **Web + Mobile**, a zatim **Logike aplikacije**.
2.  Unesite **naziv**, kao što su `Db2insertRow`, **pretplatu**, **grupa resursa**, **mjesto**i **Planiranje usluga aplikacije**. Odaberite **Prikvači na nadzornu ploču**, a zatim odaberite **Stvori**.

### <a name="add-a-trigger-and-action"></a>Dodavanje okidača i akcija
1.  U **Dizajner logike aplikacije**odaberite **Prazan LogicApp** **predložaka** popisa.
2.  Na popisu **okidača** odaberite **Ponavljanje**. 
3.  U okidača **ponavljanja** odaberite **Uređivanje**, odaberite **Učestalost** padajućeg popisa odaberite **dan**, a zatim **Interval** da upišete **7**. 
4.  Potvrdite okvir **+ Novi korak** , a zatim odaberite **Dodaj akciju**.
5.  Na popisu **Akcija** upišite **db2** u okviru Uređivanje **potražite dodatne akcije** , a zatim **DB2 - Umetanje retka (pretpregled)**.
6. U akciji **dobiti redaka (pretpregled)** odaberite **vezu za promjenu**. 
7. U oknu za konfiguriranje **veze** odaberite vezu. Na primjer, odaberite **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Na popisu **naziv tablice** odaberite na **strelicu prema dolje**, a zatim **PODRUČJE**.
9. Unesite vrijednosti za sve potrebne stupaca (pogledajte crvena zvjezdica). Ako, na primjer, upišite `99999` **AREAID**upišite `Area 99999`, pa upišite `102` za **REGIONID**. 
10. Odaberite **Spremi**.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
 
11. Odabir stavke prve naveden (najnovija cilja) u plohu **Db2insertRow** , unutar popisa **sve izvodi** u odjeljku **Sažetak**.
12. U plohu **logike aplikacije pokrenuti** odaberite **Pokreni pojedinosti**. Na popisu **Akcija** odaberite **Get_rows**. Potražite u članku vrijednost za **Stanje**koje treba **je uspio**. Odaberite **unosa vezu** da biste pogledali ulaza. Odaberite **Šalje vezu**, a pogledajte izlaze; koji treba obuhvaćaju novi redak.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Dohvaćanje pomoću odaberite jedan redak
Možete definirati logičku akciju aplikacije radi dohvaćanja jedan redak u tablici DB2. Ova akcija upućuje poveznik za obradu DB2 odaberite gdje naredbu, kao što su `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Stvaranje logike aplikacije
1.  **Azure pokretanje ploče**, odaberite **+** (znak plus), **Web + Mobile**, a zatim **Logike aplikacije**.
2.  Unesite **naziv** (primjerice "**Db2getRow**"), **pretplatu**, **grupa resursa**, **mjesto**i **Planiranje usluga aplikacije**. Odaberite **Prikvači na nadzornu ploču**, a zatim odaberite **Stvori**.

### <a name="add-a-trigger-and-action"></a>Dodavanje okidača i akcija
1.  U **Dizajner logike aplikacije**odaberite **Prazan LogicApp** **predložaka** popisa. 
2.  Na popisu **okidača** odaberite **Ponavljanje**. 
3.  U okidača **ponavljanja** odaberite **Uređivanje**, odaberite **Učestalost** padajućeg popisa odaberite **dan**, a zatim **Interval** da upišete **7**. 
4.  Potvrdite okvir **+ Novi korak** , a zatim odaberite **Dodaj akciju**.
5.  Na popisu **Akcija** upišite **db2** u okviru Uređivanje **potražite dodatne akcije** , a zatim **DB2 - dobiti redaka (pretpregled)**.
6. U akciji **dobiti redaka (pretpregled)** odaberite **vezu za promjenu**. 
7. U oknu konfiguracije **veze** odaberite postojeće veze. Na primjer, odaberite **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Na popisu **naziv tablice** odaberite na **strelicu prema dolje**, a zatim **PODRUČJE**.
9. Unesite vrijednosti za sve potrebne stupaca (pogledajte crvena zvjezdica). Ako, na primjer, upišite `99999` za **AREAID**. 
10. Po želji odaberite **Prikaži dodatne mogućnosti** da biste odredili mogućnosti upita.
11. Odaberite **Spremi**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)

12. Odabir stavke prve naveden (najnovija cilja) u plohu **Db2getRow** , unutar popisa **sve izvodi** u odjeljku **Sažetak**.
13. U plohu **logike aplikacije pokrenuti** odaberite **Pokreni pojedinosti**. Na popisu **Akcija** odaberite **Get_rows**. Potražite u članku vrijednost za **Stanje**koje treba **je uspio**. Odaberite **unosa vezu** da biste pogledali ulaza. Odaberite **Šalje vezu**, a pogledajte izlaze; koji mora sadržavati redak.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Promjena jednog retka pomoću ažuriranja
Možete definirati logičku akciju aplikacije da biste promijenili jedan redak u tablici DB2. Ova akcija upućuje poveznik za obradu Ažuriranje DB2 naredbu, kao što su `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Stvaranje logike aplikacije
1.  **Azure pokretanje ploče**, odaberite **+** (znak plus), **Web + Mobile**, a zatim **Logike aplikacije**.
2.  Unesite **naziv**, kao što su `Db2updateRow`, **pretplatu**, **grupa resursa**, **mjesto**i **Planiranje usluga aplikacije**. Odaberite **Prikvači na nadzornu ploču**, a zatim odaberite **Stvori**.

### <a name="add-a-trigger-and-action"></a>Dodavanje okidača i akcija
1.  U **Dizajner logike aplikacije**odaberite **Prazan LogicApp** **predložaka** popisa.
2.  Na popisu **okidača** odaberite **Ponavljanje**. 
3.  U okidača **ponavljanja** odaberite **Uređivanje**, odaberite **Učestalost** padajućeg popisa odaberite **dan**, a zatim **Interval** da upišete **7**. 
4.  Potvrdite okvir **+ Novi korak** , a zatim odaberite **Dodaj akciju**.
5.  Na popisu **Akcija** upišite **db2** u okviru Uređivanje **potražite dodatne akcije** , a zatim **DB2 – ažuriranje retka (pretpregled)**.
6. U akciji **dobiti redaka (pretpregled)** odaberite **vezu za promjenu**. 
7. U oknu konfiguracije **veze** odaberite da biste odabrali postojeću vezu. Na primjer, odaberite **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Na popisu **naziv tablice** odaberite na **strelicu prema dolje**, a zatim **PODRUČJE**.
9. Unesite vrijednosti za sve potrebne stupaca (pogledajte crvena zvjezdica). Ako, na primjer, upišite `99999` **AREAID**upišite `Updated 99999`, pa upišite `102` za **REGIONID**. 
10. Odaberite **Spremi**. 

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)

11. Odabir stavke prve naveden (najnovija cilja) u plohu **Db2updateRow** , unutar popisa **sve izvodi** u odjeljku **Sažetak**.
12. U plohu **logike aplikacije pokrenuti** odaberite **Pokreni pojedinosti**. Na popisu **Akcija** odaberite **Get_rows**. Potražite u članku vrijednost za **Stanje**koje treba **je uspio**. Odaberite **unosa vezu** da biste pogledali ulaza. Odaberite **Šalje vezu**, a pogledajte izlaze; koji treba obuhvaćaju novi redak.

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Uklanjanje jednog retka pomoću Izbriši
Možete definirati logičku akciju aplikacije da biste uklonili jedan redak u tablici DB2. Ova akcija upućuje poveznik za obradu naredbe Izbriši DB2, kao što su `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Stvaranje logike aplikacije
1.  **Azure pokretanje ploče**, odaberite **+** (znak plus), **Web + Mobile**, a zatim **Logike aplikacije**.
2.  Unesite **naziv**, kao što su `Db2deleteRow`, **pretplatu**, **grupa resursa**, **mjesto**i **Planiranje usluga aplikacije**. Odaberite **Prikvači na nadzornu ploču**, a zatim odaberite **Stvori**.

### <a name="add-a-trigger-and-action"></a>Dodavanje okidača i akcija
1.  U **Dizajner logike aplikacije**odaberite **Prazan LogicApp** **predložaka** popisa. 
2.  Na popisu **okidača** odaberite **Ponavljanje**. 
3.  U okidača **ponavljanja** odaberite **Uređivanje**, odaberite **Učestalost** padajućeg popisa odaberite **dan**, a zatim **Interval** da upišete **7**. 
4.  Potvrdite okvir **+ Novi korak** , a zatim odaberite **Dodaj akciju**.
5.  Na popisu **Akcije** odaberite **db2** u okviru Uređivanje **potražite dodatne akcije** , a zatim **DB2 – brisanje retka (pretpregled)**.
6. U akciji **dobiti redaka (pretpregled)** odaberite **vezu za promjenu**. 
7. U oknu konfiguracije **veze** odaberite postojeće veze. Na primjer, odaberite **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. Na popisu **naziv tablice** odaberite na **strelicu prema dolje**, a zatim **PODRUČJE**.
9. Unesite vrijednosti za sve potrebne stupaca (pogledajte crvena zvjezdica). Ako, na primjer, upišite `99999` za **AREAID**. 
10. Odaberite **Spremi**. 

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)

11. Odabir stavke prve naveden (najnovija cilja) u plohu **Db2deleteRow** , unutar popisa **sve izvodi** u odjeljku **Sažetak**.
12. U plohu **logike aplikacije pokrenuti** odaberite **Pokreni pojedinosti**. Na popisu **Akcija** odaberite **Get_rows**. Potražite u članku vrijednost za **Stanje**koje treba **je uspio**. Odaberite **unosa vezu** da biste pogledali ulaza. Odaberite **Šalje vezu**, a pogledajte izlaze; koji treba obuhvaćaju izbrisani redak.

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="technical-details"></a>Tehničke pojedinosti

## <a name="actions"></a>Akcija
Akciju je postupak provodi definirano u aplikaciji logike tijeka rada. Poveznik za baze podataka DB2 obuhvaća sljedeće radnje. 

|Akcija|Opis|
|--- | ---|
|[GetRow](connectors-create-api-db2.md#get-row)|Dohvaća podatke u jedan redak iz tablice DB2|
|[GetRows](connectors-create-api-db2.md#get-rows)|Dohvaća podatke u recima iz tablice DB2|
|[InsertRow](connectors-create-api-db2.md#insert-row)|Umeće novi redak u tablici DB2|
|[DeleteRow](connectors-create-api-db2.md#delete-row)|Brisanje retka iz tablice DB2|
|[GetTables](connectors-create-api-db2.md#get-tables)|Dohvaća podatke u tablice iz podataka DB2|
|[UpdateRow](connectors-create-api-db2.md#update-row)|Ažurira postojeći redak u tablici DB2|

### <a name="action-details"></a>Detalji o akcija

U ovom ćete odjeljku potražite u članku određene detalje o svaku akciju, uključujući obavezan ili nije unos svojstva i sve odgovarajuće izlaz pridružene poveznik.

#### <a name="get-row"></a>Početak retka 
Dohvaća podatke u jedan redak iz tablice DB2.  

| Naziv svojstva| Zaslonsko ime |Opis|
| ---|---|---|
|Tablica * | Naziv tablice |Naziv tablice DB2|
|ID * | Identifikatora retka |Jedinstveni identifikator retka koji želite dohvatiti|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Stavke

| Naziv svojstva | Vrsta podataka |
|---|---|
|ItemInternalId|niz|


#### <a name="get-rows"></a>Početak retka 
Dohvaća podatke u recima iz tablice DB2.  

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Naziv tablice|Naziv tablice DB2|
|$skip|Preskoči numeriranje|Broj stavki da biste preskočili (zadani = 0)|
|$top|Maksimalna Get Count|Maksimalan broj stavki za dohvaćanje (zadani = 256)|
|$filter|Filtriranje upita|Upit ODATA filtar da biste ograničili broj stavki|
|$orderby|Poredaj po|Upit orderBy ODATA za određivanje redoslijeda stavki|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
ItemsList

| Naziv svojstva | Vrsta podataka |
|---|---|
|vrijednost|polja|


#### <a name="insert-row"></a>Umetanje retka 
Umeće novi redak u tablicu DB2.  

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Naziv tablice|Naziv tablice DB2|
|stavke *|Redak|Redak za umetanje u navedenoj tablici u DB2|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Stavke

| Naziv svojstva | Vrsta podataka |
|---|---|
|ItemInternalId|niz|


#### <a name="delete-row"></a>Brisanje retka 
Brisanje retka iz tablice DB2.  

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Naziv tablice|Naziv tablice DB2|
|ID *|Identifikatora retka|Jedinstveni identifikator retka koji želite izbrisati|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Ništa.

#### <a name="get-tables"></a>Početak tablice 
Dohvaća podatke u tablice iz podataka DB2.  

Nema parametara za ovaj poziv. 

##### <a name="output-details"></a>Detalji o Izlaz 
TablesList

| Naziv svojstva | Vrsta podataka |
|---|---|
|vrijednost|polja|

#### <a name="update-row"></a>Ažuriranje retka 
Ažurira postojeći redak u tablici DB2.  

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Tablica *|Naziv tablice|Naziv tablice DB2|
|ID *|Identifikatora retka|Jedinstveni identifikator retka koji želite ažurirati|
|stavke *|Redak|Redak s ažuriranim vrijednosti|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz  
Stavke

| Naziv svojstva | Vrsta podataka |
|---|---|
|ItemInternalId|niz|


### <a name="http-responses"></a>HTTP odgovora

Kada se upućivanje poziva za različite akcije, mogla bi vam se određene odgovore. Sljedeća tablica prikazuje odgovore i njihovi opisi:  

|Ime|Opis|
|---|---|
|200|ok|
|202|Prihvatili|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|


## <a name="supported-db2-platforms-and-versions"></a>Podržane platforme DB2 i verzije
Ovaj poveznik podržava na sljedeće IBM DB2 platforme i verzije, kao i IBM DB2 kompatibilne proizvoda (npr. IBM Bluemix dashDB) koji podržava Distributed relacijske baze podataka arhitektura (DRDA) SQL pristup Manager (SQLAM) verziju 10 i 11:

- IBM DB2 za z/OS 11.1
- IBM DB2 za 10,1/OS z
- IBM DB2 za i 7,3
- IBM DB2 za i 7,2
- IBM DB2 za i 7.1
- IBM DB2 za LUW 11
- IBM DB2 za LUW 10.5


## <a name="next-steps"></a>Daljnji koraci

[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md). Istražite ostale dostupne poveznika u logiku aplikacija u našem [popisu API-ji](apis-list.md).

