<properties
   pageTitle="Pregled zaštite u spremištu podataka Lake | Microsoft Azure"
   description="Objašnjenje kako je Lake spremišta podataka za Azure više sigurne pohrane velikih skupova podataka"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/02/2016"
   ms.author="nitinme"/>

# <a name="security-in-azure-data-lake-store"></a>Sigurnost u spremištu Lake podataka za Azure

Mnoge korporacije izvodite prednost analize velikih skupova podataka tvrtke uvida olakšati donošenje odluka pametno. Organizacija možda okruženju složeniji i regulated s rastućim broj raznih korisnika. To je ključni za tvrtku da biste bili sigurni da ključnih poslovni podaci se pohranjuju sigurnije, uz ispravan razinu pristupa odobren pojedinačnim korisnicima. Spremište Lake podataka za Azure osmišljene su da biste lakše zadovoljava te preduvjete za sigurnost. U ovom se članku Informirajte se o sigurnosne mogućnosti spremišta Lake podataka, uključujući:

* Provjera autentičnosti
* Autorizacija
* Odvajanja mreže
* Zaštita podataka
* Nadzor

## <a name="authentication-and-identity-management"></a>Upravljanje provjeru autentičnosti i identiteta

Provjera autentičnosti je postupak koji je identitet korisnika potvrđena kada korisnik stupi u interakciju s spremišta Lake podataka ili bilo koji servis koja se povezuje s spremišta Lake podataka. Za upravljanje identitetom i provjeru autentičnosti, spremište Lake podataka koristi [Azure Active Directory](../active-directory/active-directory-whatis.md), potpun identiteta i pristup upravljanje oblaka rješenje koje pojednostavnjuje upravljanje korisnicima i grupama.

Azure pretplate mogu se pridružiti instance komponente Azure Active Directory. Samo korisnicima i identiteta servisa koji su definirani u servisu Azure Active Directory mogu pristupati računa spremišta Lake podataka pomoću portala za Azure, Alati za naredbenog retka ili putem klijentske aplikacije vašoj tvrtki ili ustanovi sastavlja pomoću SDK za spremište Lake Azure podataka. Ključne prednosti korištenja Azure Active Directory kao mehanizam za kontrolu pristupa središnje su:

* Pojednostavnjeni upravljanje identitetom životni ciklus. Identitet korisnika ili usluge (u glavni identitet servisa) možete brzo stvoriti i brzo povučen jednostavnim brisanjem ili onemogućivanje računa u direktoriju.

* Višestruka provjera autentičnosti. [Višestruka provjera autentičnosti](../multi-factor-authentication/multi-factor-authentication.md) pruža dodatne sloja sigurnosti za prijavu dodaci za korisnika i transakcije.

* Provjera autentičnosti iz bilo kojeg klijenta do standardni Otvori protokol, kao što su OAuth ili OpenID.

* Vanjski pristup sustavu enterprise imeničkim servisima i davatelji identiteta oblaka.

## <a name="authorization-and-access-control"></a>Autorizacije i pristupa kontrole

Nakon Azure Active Directory potvrđuje korisnika tako da korisnik može pristupiti Lake spremišta podataka za Azure, kontrole za autorizaciju pristupiti dozvola za spremište Lake podataka. Spremište Lake podataka odvaja autorizacije za aktivnosti vezane uz račun i povezanih podataka na sljedeći način:

* [Kontrola pristupa na temelju uloga](../active-directory/role-based-access-control-what-is.md) (RBAC) nudi Azure za upravljanje računima
* POSIX ACL-a za pristup podacima u trgovini


### <a name="rbac-for-account-management"></a>RBAC za upravljanje računima

Četiri osnovne uloge definiraju za pohranu podataka Lake prema zadanim postavkama. Uloge dopušta različite operacije na računu za spremište Lake podataka putem portala za Azure, cmdleta ljuske PowerShell i REST API-ji. Uloge vlasnika i suradnik možete izvršiti različite funkcije za administraciju na račun. Možete dodijeliti uloge Reader korisnike samo interakciju s podacima.

![Uloge RBAC] (./media/data-lake-store-security-overview/rbac-roles.png "Uloge RBAC")

Imajte na umu da je iako uloge dodjeljuju se za upravljanje računima, neke uloge utječu pristup podacima. Morate koristiti ACL-a za kontrolu pristupa operacije koje korisnik može izvoditi u datotečnom sustavu. Sljedeća tablica prikazuje sažetak upravljanja pravima i prava pristupa podacima za zadane uloge.

| Uloge                    | Upravljanje pravima               | Prava pristupa podacima | Objašnjenje |
| ------------------------ | ------------------------------- | ------------------ | ----------- |
| Dodijeljena uloga uloga         | Ništa                            | Uređena ACL-a    | Korisnik ne možete koristiti Azure portal ili Azure PowerShell cmdleti za pregledavanje spremišta Lake podataka. Korisnik može koristiti naredbenog retka Alati.
| Vlasnik  | Sve  | Sve  | Uloga vlasnika je na superkorisnik. Ta uloga sve možete upravljati i ima pristup podacima.
| Čitač   | Samo za čitanje  | Uređena ACL-a    | Ulozi čitač možete pogledati sve vezane uz upravljanje računima, kao što su koji se korisnik dodjeljuje koju ulogu. Ulozi čitač možete unositi promjene.   |
| Suradnik              | Sve osim Dodaj i Ukloni uloge | Uređena ACL-a    | Uloga suradnika možete upravljati neke aspekte računa, kao što su implementacije i stvaranje i upravljanje upozorenjima. Uloga suradnika ne možete dodati ili ukloniti uloge.
| Korisnički pristup administratora | Dodavanje i uklanjanje uloga            | Uređena ACL-a    | Ulogu administratora korisnik programa Access možete upravljati korisničkog pristupa s računima. |

Upute potražite u članku [Dodjeljivanje korisnike ili sigurnosne grupe s računima za spremište Lake podataka](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Korištenje ACL-a za operacije na datotečnih

Pohrana podataka Lake je hijerarhijsku datotečnom sustavu kao što su Hadoop Distributed datoteka sustava (HDFS) te podržava [POSIX ACL-a](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Ona određuje čitanje (r), pisanje (w) i izvršavanje (dozvole na resurse za uloga vlasnika, u grupi Vlasnici i za druge korisnike i grupe x). U Lake spremište javno pretpregleda podataka (trenutno izdanje), ACL-a omogućene su korijenska mapa, podmape i pojedinačne datoteke. ACL-a koji se odnose na korijenskoj mapi primijeniti na sve podređene mape i datoteke.

Preporučujemo da definirate ACL-a za više korisnika pomoću [sigurnosne grupe](../active-directory/active-directory-accessmanagement-manage-groups.md). Dodavanje korisnika u sigurnosne grupe, a zatim ACL-a za datoteku ili mapu dodijeliti tom sigurnosne grupe. To je korisno kada želite unijeti prilagođeni pristup jer ste ograničeni su na dodavanje maksimalno devet stavke za prilagođeni pristup. Dodatne informacije o tome da biste bolje sigurno podatke pohranjene u spremištu Lake podataka pomoću servisa Azure Active Directory sigurnosne grupe potražite u članku [Dodjela korisnika ili sigurnosnu grupu kao ACL-a u datotečni sustav Lake spremišta podataka za Azure](data-lake-store-secure-data.md#filepermissions).

![Pristup standardnih ili prilagođenih popisa] (./media/data-lake-store-security-overview/adl.acl.2.png "Pristup standardnih ili prilagođenih popisa")

## <a name="network-isolation"></a>Odvajanja mreže

Korištenje podataka Lake spremište za kontrolu pristupa za spremište podataka na razini mreže. Možete uspostaviti vatrozida i definirali raspon IP adresa za pouzdanih klijentima. S raspon IP adresa samo klijenti s IP adresom unutar određenog raspona možete povezati s spremišta Lake podataka.

![Postavke vatrozida i pristup IP] (./media/data-lake-store-security-overview/firewall-ip-access.png "Postavke vatrozida i IP adrese")

## <a name="data-protection"></a>Zaštita podataka

Tvrtke i ustanove dobro osigurati svoje tvrtke ključne podatke sigurna tijekom njegova životnog ciklusa. Za podatke na putu spremišta Lake podataka koristi protokol prijenosa sloja sigurnost (TLS) standardni radi zaštite podataka koje premješta između klijenta i spremište Lake podataka.

Zaštita podataka za podatke na ostale bit će dostupni u buduća izdanja.

## <a name="auditing-and-diagnostic-logs"></a>Nadzor i dijagnostički zapisnici

Možete koristiti nadzora ili dijagnostičke zapisnike, ovisno o tome koju tražite zapisnicima aktivnosti vezane uz upravljanje ili aktivnosti povezanih podataka.

*  Aktivnosti vezane uz upravljanje pomoću Azure resursima API-ji i naredbama na portalu za Azure putem zapisnika nadzora.
*  Aktivnosti u vezi s podacima pomoću WebHDFS REST API-ji i naredbama na portalu za Azure putem dijagnostičke zapisnike.

### <a name="auditing-logs"></a>Zapisnika nadzora

Radi usklađivanja s pravilnicima, tvrtkom ili ustanovom možda će biti potrebno odgovarajuće tragovi ako je potrebno istražujte u određenim kupljenih. Pohrana podataka Lake sadrži ugrađeni praćenje i nadzor, a bilježi sve aktivnosti Upravljanje računom.

Za tragovi Upravljanje računom, prikaz, a zatim odaberite stupce koje želite prijaviti. Također možete izvesti zapisnike nadzora Azure prostora za pohranu.

![Zapisnika nadzora] (./media/data-lake-store-security-overview/audit-logs.png "Zapisnika nadzora")

### <a name="diagnostic-logs"></a>Dijagnostički zapisnici

Možete postavljanje pristupa podacima tragovi Azure portal (u dijagnostičkih postavki) i stvorite račun spremište blobova platforme Azure pohranjuju zapisnike.

![Dijagnostički zapisnici] (./media/data-lake-store-security-overview/diagnostic-logs.png "Dijagnostički zapisnici")

Nakon konfiguriranja dijagnostičkih postavki zapisnike možete pogledati na kartici **Dijagnostičke zapisnike** .

Dodatne informacije o radu s dijagnostičke zapisnike s trgovinom Lake za Azure podataka potražite u članku [pristup dijagnostičke zapisnike za spremište Lake podataka](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Sažetak

Tvrtke potražnje oblaka platformu analize podataka koji je sigurna i jednostavno je za korištenje. Spremište Lake podataka za Azure osmišljene su da biste lakše adresu tim zahtjevima putem upravljanje identitetom i provjera autentičnosti putem integracije Azure Active Directory, koji se temelji na ACL autorizacije, odvajanja mreže, šifriranje podataka na putu i na postavite (uskoro u budućnosti) i nadzor.

Ako želite da biste vidjeli nove značajke u spremištu Lake podataka, pošaljite nam povratne informacije u na [forumu podataka Lake spremište UserVoice](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Vidi također

- [Pregled Lake spremišta podataka za Azure](data-lake-store-overview.md)
- [Početak rada s spremišta Lake podataka](data-lake-store-get-started-portal.md)
- [Zaštite podataka u spremištu Lake podataka](data-lake-store-secure-data.md)
