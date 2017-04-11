<properties
    pageTitle="Vraćanje jednu tablicu iz sigurnosne kopije baze podataka SQL Azure | Microsoft Azure"
    description="Saznajte kako vratiti jednu tablicu iz sigurnosne kopije baze podataka SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>


# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Kako vratiti jednu tablicu iz programa sigurnosne kopije baze podataka SQL Azure

Možda ćete naići situacija u kojima slučajno izmijenio neki podaci u bazi podataka sustava SQL i sada želite oporaviti jednu tablicu problematične. U ovom se članku opisuje kako vratiti jednu tablicu u bazi podataka iz jedne baze podataka SQL [Automatsko sigurnosno kopiranje](sql-database-automated-backups.md).

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Koraci za pripremu: preimenovanje tablice i vraćanje kopiju baze podataka
1. Pronađite tablicu u bazi podataka Azure SQL koji želite zamijeniti vraćene Kopiraj. Da biste promijenili naziv tablice, koristite Microsoft SQL Management Studio. Na primjer, preimenovati tablice u obliku &lt;naziv tablice&gt;_old.

    **Napomena** Da biste izbjegli blokiranje, provjerite ima li bez aktivnosti izvodi u tablici koja se naziva. Ako naiđete na probleme, provjerite je li izvršili taj postupak tijekom održavanja prozora.

2. Vraćanje sigurnosne kopije baze podataka točku na koju želite vratiti u način za [Vraćanje točke In_Time](sql-database-recovery-using-backups.md#point-in-time-restore) .

    **Bilješke**:
    - Naziv vraćene baze podataka će se u obliku DBName + vremenska oznaka; na primjer, **Adventureworks2012_2016-01-01T22-12Z**. U ovom koraku neće prebrisati postojeći naziv baze podataka na poslužitelju. To je mjera sigurnost, a on je namijenjen omogućuje vam da biste provjerili vraćenu bazu podataka da bi se one ispustite svoje trenutne baze podataka i preimenovati vraćenu bazu podataka za korištenje radnog.
    - Sve razine performanse od jednostavnih na Premium automatski sigurnosno usluga s promjenjivih metrika sigurnosne kopije zadržavanja, ovisno o tome u sloju:

| Vraćanje baze podataka | Osnovni sloju | Standardni razine | Razine Premium |
| :-- | :-- | :-- | :-- |
|  Vraćanje točke u vremenu |  Bilo koji točku vraćanja unutar sedam dana|Bilo koji točku vraćanja 35 dana| Bilo koji točku vraćanja 35 dana|

## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>Kopirajte tablicu iz vraćene baze podataka pomoću alata za migraciju baze podataka SQL
1. Preuzmite i instalirajte [Čarobnjak za migraciju SQL baze podataka](https://sqlazuremw.codeplex.com).

2. Otvorite čarobnjak Migracija baze podataka sustava SQL na stranici **Odaberite postupak** , odaberite **bazu podataka u odjeljku analiza-Migracija**, a zatim **Dalje**.
![Migracija baze podataka SQL Čarobnjak za – a zatim odaberite postupak](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)
3. U dijaloškom okviru **za povezivanje s poslužiteljem** primijeniti sljedeće postavke:
 - **Naziv poslužitelja**: instancu vaše SQL Azure
 - **Provjera autentičnosti**: **Provjera autentičnosti SQL poslužitelja**. Unesite vjerodajnice za prijavu.
 - **Baza podataka**: **Matrica DB (popis sve baze podataka)**.
 - **Napomena** Čarobnjak se po zadanom sprema podatke za prijavu. Ako ne želite da, odaberite **Zaboraviti podatke za prijavu**.
![Migracija baze podataka SQL korak čarobnjaka – Odabir izvora - 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. U dijaloškom okviru **Odabir izvora** odaberite naziv vraćenu bazu podataka iz odjeljka **pripremnih koraka** kao izvor, a zatim kliknite **Dalje**.

    ![Migracija baze podataka SQL korak čarobnjaka – Odabir izvora - 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)

5. U dijaloškom okviru **Odabir objekata** odaberite mogućnost **Odabir objekata određene baze podataka** , a zatim table(or tables) koju želite migrirati poslužitelju ciljni.
![Čarobnjak za migraciju baze podataka SQL - odaberite objekata](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)

6. Na stranici **Sažetak čarobnjaka za skripte** kliknite **da** kada se pojavi obavijest o tome spremni ste za generiranje SQL skripta. Imate mogućnost spremanja TSQL skripte za kasnije korištenje.
![Čarobnjak za migraciju baze podataka SQL - sažetak čarobnjaka za skripte](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)

7. Na stranici **Sažetak rezultata** , kliknite **Dalje**.
![Čarobnjak za migraciju baze podataka SQL - sažetak rezultata](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)

8. Na stranici **Postavljanje veza s poslužiteljem cilj** kliknite **Poveži se s poslužiteljem**pa unesite detalje na sljedeći način:
    - **Naziv poslužitelja**: instanca poslužitelja cilj
    - **Provjera autentičnosti**: **Provjera autentičnosti sustava SQL Server**. Unesite vjerodajnice za prijavu.
    - **Baza podataka**: **Matrica DB (popis sve baze podataka)**. Ova mogućnost navodi sve baze podataka na poslužitelju ciljni.

    ![Čarobnjak za Migracija baze podataka SQL - postavljanje veza s poslužiteljem cilj](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)

9. Kliknite **Poveži**, odaberite ciljni bazu podataka koju želite premjestiti u tablici da biste i zatim kliknite **Dalje**. To treba završavanje pokretanje prethodno generirani skripte i trebali biste vidjeti upravo premjestili tablicu kopirati ciljna baza podataka.

## <a name="verification-step"></a>Provjera korak
1. Upita, a zatim testirajte upravo kopirane tablice da biste bili sigurni da je podatke netaknuta. Nakon potvrde, možete je ispustite obrazac preimenovane tablice sekcije **pripremnih koraka** . (na primjer, &lt;naziv tablice&gt;_old).

## <a name="next-steps"></a>Daljnji koraci

[Automatsko sigurnosno kopiranje baze podataka SQL](sql-database-automated-backups.md)
