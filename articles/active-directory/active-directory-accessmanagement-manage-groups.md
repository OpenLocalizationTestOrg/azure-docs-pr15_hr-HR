<properties
    pageTitle="Upravljanje grupama u servisu Azure Active Directory | Microsoft Azure"
    description="Upute za stvaranje i upravljanje grupama za upravljanje Azure korisnika putem Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/29/2016"
    ms.author="curtand"/>


# <a name="managing-groups-in-azure-active-directory"></a>Upravljanje grupama u servisu Azure Active Directory

> [AZURE.SELECTOR]
- [Portal za Azure](active-directory-groups-create-azure-portal.md)
- [Azure klasični portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)


Neke značajke Upravljanje korisnicima za Azure Active Directory (Azure AD) je mogućnost stvaranja grupe korisnika. Koristiti grupe za izvođenje zadataka upravljanja kao što je Dodjela licence ili dozvole broj korisnika istodobno. Možete koristiti i grupe za dodjeljivanje dopuštenje za pristup

- Resursi kao što su objekti u imeniku
- Resursi izvan imenika kao što je aplikacija SaaS, Azure services, web-mjesta sustava SharePoint ili lokalnog resursa

Osim toga, vlasnik resursa možete dodijeliti pristup resursa u grupu Azure AD posjeduje netko drugi. Ta se Dodjela daje članovi te grupe pristup resursu. Nakon toga vlasnika grupe upravlja članstvo u grupi. Učinkovito, vlasnik resursa delegates vlasniku grupe dozvola korisnicima dodijeliti njihove resursa.

## <a name="how-do-i-create-a-group"></a>Kako stvoriti grupu?

Ovisno o servisima na koje je vaša tvrtka ili ustanova pretplaćena, možete stvoriti grupu pomoću nešto od sljedećeg:
- Azure klasični portal
- portal za račun sustava Office 365
- portal za račun sustava Windows Intune

Kako izvesti na portalu za Azure klasični smo ćete opišite zadatke. Dodatne informacije o korištenju koje nisu Azure portalima da biste upravljali Azure AD direktorija, potražite u članku [administriranje Azure AD direktorija](active-directory-administer.md).

1. [Azure klasični portala](https://manage.windowsazure.com)odaberite **Servisa Active Directory**, a zatim odaberite naziv direktorija za tvrtku ili ustanovu.

2. Odaberite karticu **grupe** .

3. Odaberite **Dodaj grupu**.

4. U prozoru **Dodaj grupu** , navedite naziv i opis grupe.


## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Kako dodati ili ukloniti pojedinačne korisnike u sigurnosnoj grupi?

**Da biste dodali pojedinačnih korisnika u grupu**

1. [Azure klasični portala](https://manage.windowsazure.com)odaberite **Servisa Active Directory**, a zatim odaberite naziv direktorija za tvrtku ili ustanovu.

2. Odaberite karticu **grupe** .

3. Otvorite grupu u koju želite dodati članove. Otvorite karticu **Članovi** odabrane grupe ako se još ne prikazuje.

4. Odaberite **Dodaj članove**.

5. Na stranici **Dodavanje članova** odaberite ime korisnika ili grupu koju želite dodati kao član grupe. Provjerite je li u oknu **Odabrana** se dodaje taj naziv.


**Da biste uklonili pojedinačnog korisnika iz grupe**

1. [Azure klasični portala](https://manage.windowsazure.com)odaberite **Servisa Active Directory**, a zatim odaberite naziv direktorija za tvrtku ili ustanovu.

2. Odaberite karticu **grupe** .

3. Otvorite grupu iz koje želite ukloniti članove.

4. Odaberite karticu **Članovi** , odaberite ime člana koji želite ukloniti iz grupe, a zatim **Ukloni**.

6. Provjerite je li kada se zatraži da želite ukloniti ovog člana iz grupe.


## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>Kako dinamično upravljati članstvo grupe?

U Azure AD vrlo lako postavljate jednostavno pravilo da biste utvrdili koji korisnici trebaju biti članovi grupe. Jednostavno pravilo jest ona koja postaje samo jedan usporedbu. Na primjer, ako SaaS aplikacija je dodijeljen grupi, možete postaviti pravila da biste dodali korisnike koji imaju na naziv radnog mjesta "Prodajni predstavnik." Tim se pravilom zatim dopušta pristup ovu aplikaciju SaaS svi korisnici s tom poslovna titula u direktoriju.

Kada se procjenjuje za sve atribute promjena korisnika u sustavu sva pravila dinamički grupe u direktoriju da biste vidjeli ako promjenu atributa korisnika želite pokrenuti bilo koje grupe dodaje ili uklanja. Ako korisnik zadovoljava pravilo na grupu, ona se dodaju kao član u toj grupi. Ako se više ne zadovoljava pravila grupe su članovi, oni se uklanjaju kao na članove iz te grupe.

> [AZURE.NOTE] Možete postaviti pravila za dinamičku članstvo na sigurnosnim grupama i grupama sustava Office 365. Članstva ugniježđenih grupa trenutno nisu podržani za grupne dodjele aplikacijama.
>
> Dinamični članstva u grupama zahtijevaju Azure AD Premium licencu dodijeliti
>
> - Administrator koja upravlja pravilo na grupu
> - Svi članovi grupe

**Da biste omogućili dinamički članstvo grupe**

1. [Azure klasični portala](https://manage.windowsazure.com)odaberite **Servisa Active Directory**, a zatim odaberite naziv direktorija za tvrtku ili ustanovu.

2. Odaberite karticu **grupe** , a zatim otvorite grupu koju želite urediti.

3. Odaberite karticu **Konfiguracija** , a zatim **Omogućiti dinamički članstva** postavljeno na **da**.

4. Postavljanje jednostavne jedan pravila grupe da biste odredili kako dinamički članstvo za ovaj grupne funkcije. Provjerite je li u **dodati korisnike kojima** mogućnost nije odabrana, a zatim svojstvo korisničkog s popisa (na primjer, odjel, jobTitle itd.),

5. Zatim odaberite uvjet (nije jednako, jednako, ne počinje s, počinje s, ne sadrži, sadrži, ne podudaraju, Match).

6. Određivanje Usporedba vrijednosti za svojstvo odabranog korisnika.

Da biste saznali više o stvaranju *naprednih* pravila (pravila koja se može sadržavati više usporedbe) za članstvo u grupama za dinamičku, potražite u članku [korištenje atributa stvaranju naprednih pravila](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>Dodatne informacije

Ti članci sadrže dodatne informacije na Azure Active Directory.

* [Upravljanje pristupom resursima s grupama Azure Active Directory](active-directory-manage-groups.md)

* [Cmdleti za Azure Active Directory za konfiguriranje postavki grupe](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)

* [Što je Azure Active Directory?](active-directory-whatis.md)

* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
