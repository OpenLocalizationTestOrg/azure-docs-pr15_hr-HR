<properties
    pageTitle="Uvod u sinkronizaciju podataka za SQL baza podataka"
    description="Pomoću ovog praktičnog vodiča pojednostavnjuje početak rada za sinkronizaciju podataka SQL Azure (pretpregled)."
    services="sql-database"
    documentationCenter=""
    authors="jennieHubbard"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/11/2016"
    ms.author="jhubbard"/>


#<a name="getting-started-with-azure-sql-data-sync-preview"></a>Uvod u sinkronizaciju podataka Azure SQL (pretpregled)
Pomoću ovog praktičnog vodiča saznat ćete osnove sinkronizacije podataka SQL Azure koje pomoću portala za klasični Azure.

Pomoću ovog praktičnog vodiča pretpostavlja minimalnog prethodnih doživljaj sustava SQL Server i baze podataka SQL Azure. U ovom ćete praktičnom vodiču Stvorite grupu sinkronizaciju hibridnog (SQL Server i baze podataka SQL instanci) u potpunosti konfigurirati i sinkroniziranje rasporedu postavite.

> [AZURE.NOTE] Dovršavanje tehničku dokumentaciju postavljanje podataka sinkronizacije SQL Azure, prethodno koja se nalazi na MSDN-u, nije dostupan kao u .pdf. Preuzmite ga [ovdje](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

## <a name="step-1-connect-to-the-azure-sql-database"></a>Korak 1: Povezivanje s bazom podataka sustava Azure SQL

1. Prijava na [Portal za klasični](http://manage.windowsazure.com).

2. U lijevom oknu kliknite **Baze podataka SQL** .

3. Kliknite **SINKRONIZIRAJ** pri dnu stranice. Kada kliknete SINKRONIZACIJU, pojavit će se popis prikazuje što možete dodati – **Nova grupa za sinkronizaciju** , a **Novi Agent za sinkronizaciju**.

4. Da biste pokrenuli čarobnjak za nove agenta za sinkronizaciju podataka SQL, kliknite **Novi Agent za sinkronizaciju**.

5. Ako još niste dodali agent prije, **kliknite Preuzmi je ovdje**.

    ![Slika1](./media/sql-database-get-started-sql-data-sync/SQLDatabaseScreen-Figure1.PNG)


## <a name="step-2-add-a-client-agent"></a>Korak 2: Dodavanje Agent za klijenta
Ovaj korak potreban je samo ako je namjeravate imati programa lokalne baze podataka SQL Server uključeni u vašoj grupi sinkronizaciju. Ako vašoj grupi sinkronizaciju sadrži samo instance SQL baze podataka, prijeđite na drugi korak 4.

<a id="InstallRequiredSoftware"></a>
### <a name="step-2a-install-the-required-software"></a>Drugi korak 2a: instalirajte potrebni softver
Provjerite imate li instalirano na računalu na koje instalirate Agent za klijent sljedeće.

- **.NET framework 4.0**

 Instalirajte .NET Framework 4.0 [ovdje](http://go.microsoft.com/fwlink/?linkid=205836).

- **Microsoft SQL Server 2008 R2 SP1 CLR vrste sustava (x86)**

 Instalirajte Microsoft SQL Server 2008 R2 SP1 sustava CLR vrsta (x86) [ovdje](http://www.microsoft.com/download/en/details.aspx?id=26728)

- **Upravljanje objektima (x86) zajednički koristiti Microsoft SQL Server 2008 R2 SP1**

 Instalirajte Microsoft SQL Server 2008 R2 SP1 zajednički se koristi upravljanje objekata (x86) [ovdje](http://www.microsoft.com/download/en/details.aspx?id=26728)



<a id="InstallClient"></a>
### <a name="step-2b-install-a-new-client-agent"></a>Drugi korak 2b: instalacija novi klijent Agent

Slijedite upute u [instalirati Agent za klijenta (Sinkroniziranje podataka SQL)](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf) da biste instalirali agenta.



<a id="RegisterSSDb"></a>
### <a name="step-2c-finish-the-new-sql-data-sync-agent-wizard"></a>Korak 2c: čarobnjaka novi agenta sinkronizacije podataka za SQL

1.  Vratite se u čarobnjak za novu agenta sinkronizacije podataka za SQL.
2.  Dodijelite agenta smisleni naziv.
3.  Na padajućem izborniku odaberite **PODRUČJE** (podatkovnog centra) za hostiranje Ovaj agent.
4.  Na padajućem popisu odaberite **PRETPLATU** za hostiranje Ovaj agent.
5.  Kliknite strelicu desno.



## <a name="step-3-register-a-sql-server-database-with-the-client-agent"></a>Korak 3: Registrirati baze podataka SQL Server s Agent za klijenta

Kada je instaliran Agent za klijenta, Registrirajte se svaki lokalne baze podataka SQL Server koju namjeravate uključiti u grupu za sinkronizaciju s agenta.
Da biste registrirali baze podataka s agenta, slijedite upute na [registrirati baze podataka SQL Server pomoću Agent za klijenta](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).



## <a name="step-4-create-a-sync-group"></a>Korak 4: Stvaranje grupe za sinkronizaciju


<a id="StartNewSGWizard"></a>
### <a name="step-4a-start-the-new-sync-group-wizard"></a>Drugi korak 4a: pokretanje čarobnjaka za novu grupu za sinkronizaciju

1.  Vratite se na [Portal za klasični](http://manage.windowsazure.com).
2.  Kliknite **baze podataka SQL**.
3.  Kliknite **Dodavanje SINKRONIZIRAJ** pri dnu stranice, a zatim odaberite Nova grupa za sinkronizaciju s na ladica.

    ![Slika2](./media/sql-database-get-started-sql-data-sync/NewSyncGroup-Figure2.png)



<a id=""></a>
### <a name="step-4b-enter-the-basic-settings"></a>Drugi korak 4b: unesite osnovne postavke


1.  Unesite smisleni naziv za grupu za sinkronizaciju.
2.  Na padajućem izborniku odaberite **PODRUČJE** (Data Center) za hostiranje ovu grupu za sinkronizaciju.
3. Kliknite strelicu desno.

    ![Image3](./media/sql-database-get-started-sql-data-sync/NewSyncGroupName-Figure3.PNG)


<a id="DefineHubDB"></a>
### <a name="step-4c-define-the-sync-hub"></a>Korak 4c: definiranje koncentratora za sinkronizaciju

1. Na padajućem popisu odaberite instancu sustava SQL baze podataka da bi služio kao koncentrator za sinkronizaciju grupe.
2. Unesite vjerodajnice za ovu instancu baze podataka SQL - **KONCENTRATOR korisničko ime** i **Lozinku KONCENTRATORA**.
3. Pričekajte SQL sinkronizacije podataka da biste potvrdili korisničko ime i lozinku. Prikazat će se pojaviti s desne strane lozinku kada su potvrditi vjerodajnice Zelena kvačica.
4. Na padajućem popisu odaberite pravila za **Rješavanje SUKOBA** .

 **Središte Wins** - promjene zapisan pisanja baze podataka koncentrator baza podataka za referencu, pisanju preko promjene u istom referencu zapis baze podataka. Functionally, to znači da se prvi promjena zapisivanje središtu propagira u druge baze podataka.


 **Klijent Wins** - promjene zapisuje središtu se prebrisati promjene u bazama podataka za referencu. Functionally, to znači da je Zadnja promjena zapisivanje središtu nešto čuvaju i prenose se na drugim bazama podataka.

5.  Kliknite strelicu desno.

    ![Image4](./media/sql-database-get-started-sql-data-sync/NewSyncGroupHub-Figure4.PNG)

<a id="AddRefDB"></a>
### <a name="step-4d-add-a-reference-database"></a>Drugi korak 4d: dodavanje reference baze podataka


Ponovite ovaj korak za svaku dodatnu bazu podataka koju želite dodati u grupu za sinkronizaciju.

1. Na padajućem izborniku odaberite bazu podataka da biste dodali.

    Baza podataka na padajućem popisu obuhvaćaju oba baze podataka SQL Server registriranih s agent i instance SQL baze podataka.
2.  Unesite vjerodajnice za Ova baza podataka – **korisničko ime** i **lozinku**.
3.  Na padajućem popisu odaberite **SMJER SINKRONIZIRAJ** tu bazu podataka.

    **Dvosmjerni** - promjene u bazi podataka za referencu zapisuju u koncentratoru bazu podataka, a promjene u bazi podataka koncentrator zapisuju u bazu podataka za referencu.

    **Sinkronizacija iz koncentratora** - baza podataka prima ažuriranja iz koncentratora. Pošaljite promjene u središtu.

    **Sinkronizacija s koncentrator** - baza podataka šalje ažuriranja koncentrator. Promjene u središtu se zapisuju se ova baza podataka.

4.  Da biste završili sa stvaranjem grupi sinkronizacije, kliknite kvačicu u donjem desnom kutu čarobnjaka. Pričekajte SQL sinkroniziranje podataka da biste potvrdili vjerodajnice. Zelena kvačica označava da su potvrdili vjerodajnice.

5.  Kliknite kvačicu drugi put. To vratiti na stranicu **SINKRONIZACIJE** u odjeljku baze podataka SQL. Ovu grupu Sinkroniziraj sada nalazi se sa svojim sinkronizaciju grupe i agente.

    ![Image5](./media/sql-database-get-started-sql-data-sync/NewSyncGroupReference-Figure5.PNG)


## <a name="step-5-define-the-data-to-sync"></a>Korak 5: Definiranje podatke koje želite sinkronizirati

Sinkroniziranje podataka za Azure SQL omogućuje odabir tablice i stupce koje želite sinkronizirati. Ako želite filtrirati stupac tako da se koje samo reci s određenim vrijednostima (kao što su dob > = 65) sinkronizirati, pomoću portala za sinkronizaciju podataka SQL pri Azure i u dokumentaciji pri odaberite na tablica, stupaca i redaka za Sinkroniziraj za definiranje podataka za sinkronizaciju.

1.  Vratite se na [Portal za klasični](http://manage.windowsazure.com).
2.  Kliknite **baze podataka SQL**.
3.  Kliknite karticu **Sinkronizacija** .
4.  Kliknite naziv grupe za sinkronizaciju.
5.  Kliknite karticu **Sinkronizacija pravila** .
6.  Odaberite bazu podataka koju želite unijeti grupi sheme sinkronizaciju.
7.  Kliknite strelicu desno.
8.  Kliknite **OSVJEŽI SHEMU**.
9.  Za svaku tablicu u bazi podataka, odaberite stupce koje želite obuhvatiti u sinkronizacijom.
    - Stupci s nepodržane vrste podataka nije moguće odabrati.
    - Ako je odabran bez stupaca u tablici, tablici nije uključen u grupi sinkronizaciju.
    - Da biste poništili odabir i sve tablice, kliknite ODABERI pri dnu zaslona.
10. Kliknite **SPREMI**, a zatim pričekajte grupi sinkronizaciju da biste završili dodjele resursa.
11. Da biste se vratili na stranicu odredišne sinkronizacije podataka, kliknite Natrag strelicu u gornjem lijevom kutu zaslona (iznad naziva grupe sinkronizaciju).

    ![Image6](./media/sql-database-get-started-sql-data-sync/NewSyncGroupSyncRules-Figure6.PNG)

## <a name="step-6-configure-your-sync-group"></a>Korak 6: Konfiguriranje vašoj grupi sinkronizacije

Uvijek možete sinkronizirati grupu za sinkronizaciju tako da kliknete pri dnu stranice odredišna sinkronizacije podataka.
Da biste sinkronizirali prema rasporedu, konfigurirati grupu tako sinkronizaciju.

1.  Vratite se na [Portal za klasični](http://manage.windowsazure.com).
2.  Kliknite **baze podataka SQL**.
3.  Kliknite karticu **Sinkronizacija** .
4.  Kliknite naziv grupe za sinkronizaciju.
5.  Kliknite karticu **KONFIGURACIJA** .
6.  **AUTOMATSKU SINKRONIZACIJU**
    - Da biste konfigurirali grupi sinkronizaciju da biste sinkronizirali na skup učestalost, kliknite **Dalje**. Možete se i dalje sinkronizirati na zahtjev tako da kliknete SINKRONIZACIJU.
    - Kliknite **ISKLJUČI** da biste konfigurirali grupu za sinkronizaciju da biste sinkronizirali samo kada kliknite SINKRONIZIRAJ.
7.  **UČESTALOST SINKRONIZACIJE**
    - Ako je AUTOMATSKU SINKRONIZACIJU, postavite učestalost sinkronizacije. Učestalost mora biti između pet minuta i 1 mjesec.
8.  Kliknite **SPREMI**.

![Image7](./media/sql-database-get-started-sql-data-sync/NewSyncGroupConfigure-Figure7.PNG)

Čestitamo. Stvorite grupu za sinkronizaciju koja sadrži instancu komponente SQL baze podataka i baze podataka SQL Server.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o SQL baze podataka i sinkroniziranje podataka za SQL potražite u članku:

* [Preuzimanje dovrši tehničku dokumentaciju sinkronizacije podataka za SQL](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)
* [Pregled SQL baze podataka](sql-database-technical-overview.md)
* [Upravljanje bazom podataka životni ciklus](https://msdn.microsoft.com/library/jj907294.aspx)
 

 
