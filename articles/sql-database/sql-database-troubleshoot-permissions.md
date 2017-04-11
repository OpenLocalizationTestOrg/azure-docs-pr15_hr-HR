<properties
    pageTitle="Kako učiniti administratorskih zadataka, npr poništiti administratorsku lozinku | Microsoft Azure"
    description="U članku se opisuje kako izvoditi uobičajene administrativne zadatke u SQL baze podataka. Na primjer, ponovno postavljanje lozinke za administratore, dodjelu i uklanjanje programa access."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="ponovno postavljanje lozinke za administratore"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="how-to-perform-common-administrative-tasks-such-as-resetting-admin-password-in-azure-sql-database"></a>Kako izvoditi uobičajene administrativne zadatke kao što je ponovno postavljanje lozinke za administratore u bazi podataka SQL Azure
Pomoću ove teme brzi koraci dopustiti i uklanjanje pristupa s bazom podataka Azure SQL. Detaljnije informacije potražite u članku:

- [Upravljanje bazama podataka i prijave u bazi podataka SQL Azure](sql-database-manage-logins.md)
- [Zaštita baze podataka sustava SQL](sql-database-security.md)
- [Centar za sigurnost za modul za baze podataka SQL Server i baze podataka Azure SQL](https://msdn.microsoft.com/library/bb510589)


[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="to-reset-admin-password-for-a-logical-server"></a>Da biste ponovno postavljanje administratorske lozinke za logičke poslužitelj

- [Azure Portal](https://portal.azure.com) kliknite **SQL Server**, s popisa odaberite poslužitelj, a zatim **Vrati izvornu lozinku**.

## <a name="to-help-make-sure-only-authorized-ip-addresses-are-allowed-to-access-the-server"></a>Da biste bili sigurni da samo ovlašteni IP adrese je li dopušten pristup poslužitelju
- U odjeljku [Kako: Konfiguriranje postavki vatrozida na baze podataka SQL](sql-database-configure-firewall-settings.md).

## <a name="to-create-contained-database-users-in-the-user-database"></a>Da biste stvorili korisnici sadržavao baze podataka u bazi podataka za korisnika
- Pomoću naredbe [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx) i potražite u članku [Nalazi korisnici baze podataka – upućivanje vaše baze podataka prijenosni](https://msdn.microsoft.com/library/ff929188.aspx).

## <a name="to-authenticate-contained-database-users-by-using-your-azure-active-directory"></a>Za provjeru autentičnosti korisnika sadrži baze podataka pomoću Azure Active Directory
- Potražite u članku [Povezivanje s bazom podataka SQL pomoću provjere autentičnosti Azure Active Directory](sql-database-aad-authentication.md).

## <a name="to-create-additional-logins-for-high-privileged-users-in-the-virtual-master-database"></a>Da biste stvorili dodatne prijave za visoke povlaštene korisnike u virtualne glavnom bazom podataka
- Koristite naredbu za [Prijavu za stvaranje](https://msdn.microsoft.com/library/ms189751.aspx) pa u odjeljku Upravljanje prijave [Upravljanje bazama podataka](sql-database-manage-logins.md) i prijave u bazi podataka SQL Azure više detalja.
