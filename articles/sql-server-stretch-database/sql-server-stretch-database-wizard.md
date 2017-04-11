<properties
    pageTitle="Početak rada tako da pokrenete bazu podataka za omogućivanje rastezanje Čarobnjak za | Microsoft Azure"
    description="Saznajte kako konfigurirati baze podataka za bazu podataka rastezanje tako da pokrenete bazu podataka za omogućivanje rastezanje čarobnjak."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Početak rada tako da pokrenete bazu podataka omogućiti za korištenje čarobnjaka za rastezanje

Da biste konfigurirali baze podataka za bazu podataka rastezanje, pokrenite baze podataka za omogućivanje rastezanje čarobnjak.  U ovoj se temi opisuju informacije koje morate unijeti i mogućnosti koje morate napraviti u čarobnjaku.

Dodatne informacije o rastezanje baze podataka potražite u članku [Rastezanje baze podataka](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Kasnije, Ako onemogućite rastezanje baze podataka, imajte na umu da onemogućivanje rastezanje baze podataka za tablicu ili za bazu podataka ne briše udaljene objekta. Ako želite da biste izbrisali udaljenoj tablici ili udaljenom bazom podataka, morate ga ispustite pomoću portala za upravljanje Azure. Remote objekte i dalje plaćati Azure troškove dok se ne možete ih izbrisati ručno. 

## <a name="launch-the-wizard"></a>Pokretanje čarobnjaka

1.  SQL Server Management Studio u programu Explorer objekta, odaberite bazu podataka na kojoj želite omogućiti rastezanje.

2.  Desnom\-kliknite i odaberite **Zadaci**i zatim odaberite **rastezanje**, a zatim **omogućiti** da biste pokrenuli čarobnjak.

## <a name="Intro"></a>Uvod
Pregledajte Svrha čarobnjaka i preduvjete.

Preduvjeti za važne obuhvaćaju sljedeće:

-   Morate biti administrator da biste promijenili postavke baze podataka.
-   Morate imati pretplatu na Microsoft Azure.
-   SQL Server je da biste mogli komunicirati s udaljeni poslužitelj za Azure.

![Uvodna stranica čarobnjaka za rastezanje baze podataka][StretchWizardImage1]

## <a name="Tables"></a>Odabir tablice
Odaberite tablice s kojima želite omogućiti za rastezanje.

Tablice s puno redaka pojavljuju se pri vrhu sortiranog popisa. Prije nego što se prikazuje u čarobnjaku na popisu tablica, ona ih analizira za vrste podataka koje trenutno nisu podržani rastezanje baze podataka.

![Odaberite tablice stranica čarobnjaka za rastezanje baze podataka][StretchWizardImage2]

|Stupac|Opis|
|----------|---------------|
|(bez naslova)|Potvrdite okvir u ovom stupcu da biste omogućili odabranu tablicu za rastezanje.|
|**Ime**|Određuje naziv stupca u tablici.|
|(bez naslova)|U ovom stupcu mogu predstavljati upozorenje koji se ne\'t nećete moći omogućiti odabranu tablicu za rastezanje. Također mogu predstavljati tog problema koji sprječava omogućiti odabranu tablicu za rastezanje \- , na primjer, jer se u tablici koriste nepodržane vrste. Prijeđite pokazivačem preko simbol da biste prikazali dodatne informacije u zaslonskom opisu. Dodatne informacije potražite u članku [ograničenja za rastezanje bazu podataka](sql-server-stretch-database-limitations.md).|
|**Rastegnuto**|Upućuje li tablici već omogućeno za rastezanje.|
|**Migriranje**|Možete migrirati cijelu tablicu (**Cijelu tablicu**) ili možete navesti filtar na postojeći stupac u tablici. Ako želite koristiti funkciju drugačiji filtar da biste odabrali redaka koji želite migrirati, iskaz ALTER TABLE da biste odredili funkcija filter nakon što izađete iz čarobnjaka za pokretanje. Dodatne informacije o funkciji filtra potražite u članku [Odaberite retke za migriranje pomoću funkcije Filtar](sql-server-stretch-database-predicate-function.md). Da biste vidjeli dodatne informacije o primjeni funkciju, potražite u članku [Omogućivanje rastezanje baze podataka za tablicu](sql-server-stretch-database-enable-table.md) ili [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Reci**|Određuje broj redaka u tablici.|
|**Veličina (KB)**|Određuje veličinu tablice u KB.|

## <a name="Filter"></a>Filtar retka po izboru

Ako želite unijeti funkciju filtar da biste odabrali retke za migriranje, učinite sljedeće na stranici **Odabir tablice** .

1.  Na popisu **odaberite tablice koje želite rastezanje** kliknite **Cijelu tablicu** u retku za tablicu. Otvara se dijaloški okvir **Odabir redaka na rastezanje** .

    ![Definiranje funkcija filter][StretchWizardImage2a]

2.  U dijaloškom okviru **Odaberite retke na rastezanje** odaberite **Odaberite retke**.

3.  U **polje Naziv**Navedite naziv funkcije Filtar.

4.  Za uvjet **gdje** odaberite stupca iz tablice, odaberite operator i navesti vrijednost.

5. Kliknite **Provjeri** da biste testirali funkciju. Ako funkcija vraća rezultate iz tablice – to jest, ako postoje reci će se migrirati koji ispunjavaju uvjet - test izvješća **uspješno**.

    >   [AZURE.NOTE] Tekstni okvir koji se prikazuje upit filtar je samo za čitanje. Ne možete uređivati upit u tekstni okvir.

6.  Kliknite gotovo da biste se vratili na stranicu za **Odabir tablice** .

Funkcija filter se stvara u sustavu SQL Server samo kada čarobnjaka. Tek onda, možete se vratiti na stranicu **odaberite tablice** da biste promijenili ili preimenovanje funkcija filter.

![Odaberite stranicu tablice nakon definiranja funkcija filter][StretchWizardImage2b]

Ako želite koristiti neku drugu vrstu funkcija filter da biste odabrali retke za migriranje, učinite nešto od sljedećeg.  

-   Zatvorite čarobnjak i pokrenite iskaz ALTER TABLE da biste omogućili rastezanje tablice i da biste odredili funkcija filter. Dodatne informacije potražite u članku [Omogućivanje rastezanje baze podataka za tablicu](sql-server-stretch-database-enable-table.md).  

-   Iskaz ALTER TABLE da biste odredili funkcija filter nakon što izađete iz čarobnjaka za pokretanje. Potrebni koraci potražite u članku [Dodavanje funkcija filter kada pokrenete čarobnjak](sql-server-stretch-database-predicate-function.md#addafterwiz).

## <a name="Configure"></a>Konfiguriranje Azure implementaciju

1.  Prijavite se na Microsoft Azure pomoću Microsoftova računa.

    ![Prijavite se u Azure - Čarobnjak za rastezanje baze podataka][StretchWizardImage3]

2.  Odaberite postojeću Azure pretplatu za rastezanje baze podataka.

3.  Odaberite Azure regiju.
    -   Ako stvorite novi poslužitelj, poslužitelj se stvara u tom području.  
    -   Ako imate postojeći poslužitelji unutar odabranog područja, čarobnjak popisuje ih kada odaberete **postojeći poslužitelja**.

    Da biste minimizirali Latencija, odaberite Azure regije u kojoj se nalazi SQL Server. Dodatne informacije o područja potražite u članku [Azure područja](https://azure.microsoft.com/regions/).

4.  Odredite želite li koristiti poslužitelj za postojeću ili stvoriti novi poslužitelj za Azure.

    Ako je Active Directory u sustavu SQL Server pridružene s Azure Active Directory, po želji možete pomoću računa za vanjsko servisa za SQL Server možete komunicirati s udaljeni poslužitelj za Azure. Da biste vidjeli dodatne informacije o preduvjetima za tu mogućnost, potražite u članku [Izmjena baze podataka POSTAVITE mogućnosti (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Stvaranje novog poslužitelja**

        1.  Stvaranje prijava i lozinka za administratora poslužitelja.

        2.  Ako želite, pomoću računa za vanjsko servisa za SQL Server možete komunicirati s udaljeni poslužitelj za Azure.

        ![Stvaranje novog Azure poslužitelja - Čarobnjak za rastezanje baze podataka][StretchWizardImage4]

    -   **Postojeći poslužitelj**

        1.  Odaberite postojeći poslužitelj za Azure.

        2.  Odaberite način provjere autentičnosti.

            -   Ako ste odabrali **Provjera autentičnosti sustava SQL Server**, navedite prijava administrator i lozinku.

            -   Odaberite **Active Directory integrirane provjere** da biste koristili račun pridruženim servisa za SQL Server možete komunicirati s udaljeni poslužitelj za Azure. Ako odabrani poslužitelj nije integriran s Azure Active Directory, ta mogućnost ne prikazuje.

        ![Odaberite postojeći Azure poslužitelj - Čarobnjak za rastezanje baze podataka][StretchWizardImage5]

## <a name="Credentials"></a>Sigurna vjerodajnice
Morate imati glavni ključ baze podataka radi zaštite vjerodajnice za bazu podataka rastezanje koristi za povezivanje s udaljenom bazom podataka.  

Ako već postoji glavni ključ za bazu podataka, unesite lozinku za nju.  

![Vjerodajnica za sigurno stranica čarobnjaka za rastezanje baze podataka][StretchWizardImage6b]

Ako baza podataka nije postojeće glavni ključ, unesite jaku lozinku da biste stvorili matricu ključ za baze podataka.  

![Vjerodajnica za sigurno stranica čarobnjaka za rastezanje baze podataka][StretchWizardImage6]

Dodatne informacije o glavni ključ baze podataka potražite u članku [Stvaranje MATRICE KLJUČ (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) i [Stvaranje osnovne ključ za baze podataka](https://msdn.microsoft.com/library/aa337551.aspx). Dodatne informacije o vjerodajnica koje čarobnjak stvara potražite u članku [Stvaranje baze podataka iz DJELOKRUGA VJERODAJNICA (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Odaberite IP adresa
Da biste stvorili pravilo vatrozid na Azure koji omogućuje komunikaciju SQL Server s udaljeni poslužitelj za Azure pomoću rasponu podmreže IP adresa (preporučeno) ili javnu IP adresu sustava SQL Server.

IP adresa ili adrese koje se nalaze na ovoj stranici objasnite Azure server da biste omogućili ulazne podatke, upita i upravljanje operacije pokrenuo SQL Server da biste proći kroz vatrozid za Azure. Čarobnjak ne mijenja ništa u postavke vatrozida na SQL Server.

![Odaberite IP adresa stranica čarobnjaka za rastezanje baze podataka][StretchWizardImage7]

## <a name="Summary"></a>Sažetak
Pregledajte vrijednosti koje ste unijeli i mogućnosti koje ste odabrali u čarobnjaku i Procijenjena troškova Azure. Zatim odaberite **Završi** da biste omogućili rastezanje.

![Stranica sa sažetkom čarobnjaka rastezanje baze podataka][StretchWizardImage8]

## <a name="Results"></a>Rezultati
Pregledajte rezultate.

Praćenje statusa migracije podataka potražite u članku [monitora i otklanjanje poteškoća s migracije podataka (rastegnuto baza podataka)](sql-server-stretch-database-monitor.md).

![Stranice s rezultatima čarobnjaka rastezanje baze podataka][StretchWizardImage9]

## <a name="KnownIssues"></a>Čarobnjak za otklanjanje poteškoća
**Čarobnjak za rastezanje baze podataka nije uspjelo.**
Ako baza podataka rastezanje još nije omogućena na razini poslužitelja, a pokrenuti čarobnjak za bez sustav administratorske dozvole da biste ga omogućili, čarobnjak neće uspjeti. Zatražite od administratora sustava da biste omogućili rastezanje baze podataka na instancu lokalni poslužitelj, a zatim ponovno pokrenite čarobnjak. Dodatne informacije potražite u članku [pripremni: dozvole za omogućivanje rastezanje baze podataka na poslužitelju](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## <a name="next-steps"></a>Daljnji koraci
Omogućivanje dodatnih tablica za bazu podataka rastezanje. Nadzor migraciju podataka i upravljanje rastezanje\-omogućeno baze podataka i tablice.

-   [Omogućivanje rastezanje baze podataka za tablicu](sql-server-stretch-database-enable-table.md) da biste omogućili dodatne tablice.

-   [Monitora i otklanjanje poteškoća s migracije podataka](sql-server-stretch-database-monitor.md) da biste vidjeli status migracije podataka.

-   [Zaustavljanje i nastavak rastezanje baze podataka](sql-server-stretch-database-pause.md)

-   [Upravljanje i rješavanje problema s rastezanje baze podataka](sql-server-stretch-database-manage.md)

-   [Sigurnosno kopiranje baze podataka rastezanje omogućeno](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Vidi također

[Omogućivanje rastezanje baze podataka za baze podataka](sql-server-stretch-database-enable-database.md)

[Omogućivanje rastezanje baze podataka za tablicu](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png
