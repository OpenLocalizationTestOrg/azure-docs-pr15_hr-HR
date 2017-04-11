<properties
   pageTitle="Servisa Azure centar za sigurnost i baze podataka SQL Azure | Microsoft Azure"
   description="U ovom se članku prikazuje kako centar za sigurnost olakšavaju sigurne baze podataka u bazi podataka SQL Azure."
   services="sql-database"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-and-azure-sql-database-service"></a>Servisa Azure centar za sigurnost i baze podataka SQL Azure

[Centar za sigurnost Azure](https://azure.microsoft.com/documentation/services/security-center/) pomaže spriječiti, otkrivanje i odgovaranje na prijetnji. Pruža integriranu sigurnost nadzor i pravila upravljanja preko pretplate Azure, pomaže u otkrivanju prijetnji koje možda u suprotnom otvorite uočen i radi s Bogata paleta sigurnost rješenja.

U ovom se članku prikazuje kako centar za sigurnost olakšavaju sigurne baze podataka u bazi podataka SQL Azure.

## <a name="why-use-security-center"></a>Zašto koristiti centar za sigurnost?

Centar za sigurnost pomaže vam zaštitili podataka u bazi podataka SQL unosom uvid u sigurnost sve poslužitelje i baza podataka. Uz centar za sigurnost, možete učiniti sljedeće:

- Definiranje pravilnika za šifriranje baze podataka SQL i nadzor.
- Praćenje sigurnost SQL baze podataka resursa preko svih pretplata.
- Brzo prepoznate i remediate sigurnosnim problemima.
- Integrirati upozorenja s [bazom podataka SQL Azure prijetnju otkrivanje](../sql-database/sql-database-threat-detection-get-started.md).

Osim zaštitite resursa u SQL baze podataka, centar za sigurnost omogućuje sigurnost nadzor i upravljanje za Azure virtualnim strojevima, servise u Oblaku, aplikacije servisa, virtualne mreže i drugo. Dodatne informacije o centru za sigurnost [ovdje](security-center-intro.md).

## <a name="prerequisites"></a>Preduvjeti

Za početak rada s centar za sigurnost, morate imati pretplatu na Microsoft Azure. Besplatni sloj centar za sigurnost je omogućeno za vašu pretplatu. Dodatne informacije o slobodnom centar za sigurnost i standardne razine potražite u članku [Cijene za centar za sigurnost](https://azure.microsoft.com/pricing/details/security-center/).

Centar za sigurnost podržava pristupa na temelju uloga. Da biste saznali više o kontrola pristupa na temelju uloga (RBAC) u Azure, potražite u članku [Utemeljen na Azure Active Directory uloga kontrola pristupa](../active-directory/role-based-access-control-configure.md). Najčešća pitanja vezana uz centar za sigurnost navedeni opisi [kako se upravlja dozvole u centru za sigurnost](security-center-faq.md#how-are-permissions-handled-in-azure-security-center).

## <a name="access-security-center"></a>Centar za sigurnost programa Access

Centar za sigurnost pristupiti s [portala za Azure](https://azure.microsoft.com/features/azure-portal/). [Prijavite se na portal](https://portal.azure.com/) i odaberite željenu **mogućnost centar za sigurnost**.

![Mogućnost centar za sigurnost][1]

Otvorit će se plohu **Centar za sigurnost** .
![Centar za sigurnost plohu][2]

## <a name="set-security-policy"></a>Postavljanje sigurnosnih pravilnika

Sigurnosna pravila definira skup kontrola koje su preporučena resursi unutar navedena pretplata ili grupu resursa. U centru za sigurnost, definirajte pravila za pretplate ili grupe resursa prema potrebama sigurnosti vaše tvrtke i vrstu aplikacije ili osjetljivosti podatke iz pretplate.

Možete postaviti pravila da biste prikazali preporuke za nadzor SQL i šifriranje prozirne podataka SQL (TDE).

- Kada uključite **SQL nadzor i otkrivanje prijetnju**centar za sigurnost preporučuje da nadzor pristup bazi podataka Azure omogućena za usklađenost s dodatnim otkrivanje i istrage svrhe.
- Kada uključite **šifriranje prozirne podataka SQL**, centar za sigurnost preporučuje te šifriranje na ostale biti omogućen za baze podataka SQL Azure, pridruženi sigurnosne kopije i datoteke zapisnika transakcije.

Da biste postavili sigurnosna pravila, odaberite pločicu **pravila** na plohu centar za sigurnost. Na plohu **Sigurnosna pravila** odaberite pretplatu na kojoj želite omogućiti sigurnosna pravila. Odaberite **pravilnik o sprječavanju** i **uključite preporuke o sigurnosti koje želite koristiti na ovu pretplatu** .
![Sigurnosna pravila][3]

Dodatne informacije potražite u odjeljku [Postavljanje sigurnosnih pravilnika](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Upravljanje preporuka za sigurnost

Centar za sigurnost povremeno analizira stanja sigurnosti Azure resurse. Kada centar za sigurnost označava potencijalne slabe, stvara preporuke. Preporuke će vas voditi kroz postupak konfiguriranja potrebnih kontrola.

Nakon što postavite sigurnosna pravila, centar za sigurnost analizira stanja sigurnosti resurse da biste odredili potencijalne slabe točke. Preporuke prikazuju se u obliku tablice pri čemu svaki redak predstavlja jednu određenu preporuke. Koristite u sljedećoj su tablici kao referenca pomoću kojih se objašnjava dostupna preporuke za baze podataka SQL Azure i svaki preporuke funkcija ako ga primijeniti. Odabir i preporuke vodi vas na članak na kojem se objašnjava kako implementirati na preporuke u centru za sigurnost.

| Preporuka | Opis |
| ----- | ----- |
| [Omogući nadzor i prijetnju otkrivanje na SQL Server](security-center-enable-auditing-on-sql-servers.md) | Preporučuje uključivanje nadzor i prijetnju otkrivanje za baze podataka SQL poslužitelja. (Samo za SQL baze podataka usluga. Ne obuhvaća Microsoft SQL Server na virtualnim strojevima.) |
| [Omogući nadzor i prijetnju otkrivanje na baze podataka SQL](security-center-enable-auditing-on-sql-databases.md) | Preporučuje uključivanje nadzor i prijetnju otkrivanje za baze podataka SQL baze podataka. (Samo za SQL baze podataka usluga. Ne obuhvaća Microsoft SQL Server na virtualnim strojevima.) |
| [Omogućivanje šifriranje prozirne podataka](security-center-enable-transparent-data-encryption.md) | Preporučuje se da omogućite šifriranje za baze podataka SQL. (SQL baze podataka usluga samo.) |

Da biste vidjeli preporuke za Azure resursa, odaberite pločicu **preporuke** na plohu centar za sigurnost. Na plohu **preporuke** odaberite preporuke da biste vidjeli detalje. U ovom primjeru, recimo odaberite **Omogući nadzor i otkrivanje prijetnju na SQL Server**.

![Preporuke][4]

Kao što je prikazano u nastavku, centar za sigurnost prikazuje SQL poslužitelja kojem nadzor i prijetnju otkrivanje nisu omogućeni. Kada uključite nadzor možete konfigurirati postavke prijetnju otkrivanja i postavke e-pošte primati sigurnosna upozorenja. Otkrivanje prijetnju upozorenja kada utvrdi aktivnosti anomalous baze podataka koje označavaju potencijalnih sigurnosnih rizika u bazu podataka. Upozorenja prikazuju se u Centar za sigurnost nadzornu ploču.
![Nadzor i prijetnju otkrivanje][5]

Slijedite korake u [Uvod u SQL baze podataka prijetnju otkrivanje](../sql-database/sql-database-threat-detection-get-started.md) da biste uključili i konfiguriranje prijetnju otkrivanje i konfiguriranje na popisu poruka e-pošte koji će primati sigurnosna upozorenja o otkrivanje anomalous aktivnosti.

Da biste saznali više o preporuke, potražite u članku [Upravljanje sigurnost preporuke](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Nadzor stanja sigurnosti

Kada omogućite [sigurnosne pravilnike](security-center-policies.md) za resurse za pretplatu na centar za sigurnost će analiza sigurnosti resurse da biste odredili potencijalne slabe točke.  Stanja sigurnosti resursa možete pogledati na pločici **stanja sigurnosti resursa** . Kada kliknete **podataka** na pločici **stanja sigurnosti resursa** , plohu **Izvor podataka** otvara se s SQL preporuke za probleme kao što su nadzora i prozirne šifriranje podataka nisu omogućeni. Ima i preporuke za opće stanja baze podataka.
![Stanje sigurnosti resursa][6]

Dodatne informacije potražite u članku [Nadzor stanja sigurnost](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Upravljanje i odgovaranje na sigurnosnih upozorenja

Centar za sigurnost automatski prikuplja, analizira i integrira podaci iz zapisnika [Azure SQL prijetnju otkrivanje](../sql-database/sql-database-threat-detection-get-started.md), kao i ostale resurse za Azure možete otkriti realni prijetnji i smanjiti false positives. Uz informacije koje morate brzo Istraživanje problema i preporuke za remediate u slučaju napada prikazan je popis prioritet sigurnosnih upozorenja u centru za sigurnost.

Da biste vidjeli upozorenja, odaberite pločicu **sigurnosnih upozorenja** na plohu centar za sigurnost. Na plohu **sigurnosnih upozorenja** odaberite upozorenje da biste saznali više o događajima koji ga je pokrenuo na upozorenja i što, ako nešto korake morate poduzeti da biste remediate u slučaju napada. U ovom primjeru odaberimo **potencijalne SQL unos**.
![Sigurnosna upozorenja][7]

Kao što je prikazano u nastavku, centar za sigurnost pruža dodatne pojedinosti koje nude uvid u što pokrene upozorenju cilj resursa, kada je primjenjivo IP adresu izvora i preporuke o tome kako remediate.
![Potencijalne unos SQL][8]

Dodatne informacije potražite u članku [Upravljanje i odgovarati na njih sigurnosnih upozorenja](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Daljnji koraci

- [Najčešća pitanja o centru za sigurnost](security-center-faq.md) – traženje najčešća pitanja o korištenju servisa.
- [Vodič za planiranje i operacije centar za sigurnost](security-center-planning-and-operations-guide.md) - prati skup koraka i zadataka da biste optimizirali korištenje centar za sigurnost na temelju sigurnosne preduvjete i model upravljanja oblaka vaše tvrtke ili ustanove.
- [Sigurnost podataka u centru za sigurnost](security-center-data-security.md) – Saznajte kako centar za sigurnost prikuplja i obrađuje podatke o Azure resursa, uključujući informacije o konfiguraciji, metapodatke, zapisnika događaja, rušenje ispis datoteka i drugo.
- [Rukovanje sigurnošću](security-center-incident.md) – Saznajte kako koristiti mogućnost sigurnosnih upozorenja u centru za sigurnost kao pomoć u rukovanje sigurnošću.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
