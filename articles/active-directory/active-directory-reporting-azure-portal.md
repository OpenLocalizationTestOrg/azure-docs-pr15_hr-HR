<properties
   pageTitle="Azure Active Directory izvješćivanje – pregled | Microsoft Azure"
   description="Popis različitih dostupnih izvješća za pretpregled Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="markusvi"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="markvi"/>

# <a name="azure-active-directory-reporting---preview"></a>Azure Active Directory izvješćivanje – pregled

> [AZURE.SELECTOR]
- [Portal za Azure](active-directory-reporting-azure-portal.md)
- [Azure klasični portal](active-directory-reporting-guide.md)

*Ovo je dokumentacija je dio [Azure Active Directory izvješćivanja vodič](active-directory-reporting-guide.md).*

Pomoću izvješća u pretpregledu Azure Active Directory, dobit sve potrebne informacije da biste odredili kako radi vaše okruženje. [Novosti u pretpregledu?](active-directory-preview-explainer.md)

Postoje dva glavna područja izvješćivanja:

- **Aktivnosti za prijavu** – informacije o korištenju upravljanih aplikacije i prijaviti se aktivnosti korisnika

- **Zapisnike nadzora** – sustava aktivnosti informacije o korisnicima i upravljanje grupe, upravljanih programa i aktivnosti direktorija

Ovisno o opseg podataka koji tražite, možete pristupiti takva izvješća ili tako da kliknete **korisnici i grupe** ili **aplikacijama** na popisu servisa [Azure portal](https://portal.azure.com).

## <a name="sign-in-activities"></a>Aktivnosti za prijavu

### <a name="user-sign-in-activities"></a>Prijaviti se aktivnosti korisnika

S podacima koje ste dobili od izvješća za prijavu korisnika, pronađete odgovore na pitanja kao što su:

- Što je uzorak Prijava korisnika?
- Koliko korisnika su korisnici prijavljeni putem tjedan?
- Što je status te prijavu dodataka?

Točku unosa u ovim podacima je grafikon korisnika, prijavu u odjeljku **Pregled** u odjeljku **korisnici i grupe**.

 ![Izvješćivanje o pogreškama] (./media/active-directory-reporting-azure-portal/05.png "Izvješćivanje o pogreškama")

Korisnik prijavu na grafikonu se prikazuju tjedni zbrajanja znak dodaci za sve korisnike u određenom vremenskom razdoblju. Zadano za razdoblje je 30 dana.

![Izvješćivanje o pogreškama] (./media/active-directory-reporting-azure-portal/02.png "Izvješćivanje o pogreškama")

Kada kliknete na dan u grafikonu za prijavu, prikazat će se detaljni popis aktivnosti za prijavu.

![Izvješćivanje o pogreškama] (./media/active-directory-reporting-azure-portal/03.png "Izvješćivanje o pogreškama")

Svaki redak na popisu aktivnosti za prijavu pruža detaljne informacije o prijavu odabrane kao što su:

- Tko je potpisao?

- Što je povezane UPN-a?

- Koje aplikacije je cilj prijavu?

- Što je IP adresu za prijavu?

- Što je status prijavu?

### <a name="usage-of-managed-applications"></a>Korištenje upravljanih programa

S računala usmjereni na prikaz podataka za prijavu, možete odgovoriti pitanja kao što su:

- Tko koristi moje aplikacije?

- Što su gornje 3 aplikacije u vašoj tvrtki ili ustanovi?

- Koje ste nedavno poslednjeg izvan programa. Kako to radi?


Točku unosa u ovim podacima je gornja 3 aplikacije u tvrtki ili ustanovi unutar posljednje izvješće 30 dana u odjeljku **Pregled** **aplikacijama**.

 ![Izvješćivanje o pogreškama] (./media/active-directory-reporting-azure-portal/06.png "Izvješćivanje o pogreškama")


U aplikaciju korištenje grafikonu tjedni zbrajanja od prijavite dodaci za aplikacije vrha 3 u određenom vremenskom razdoblju. Zadano za razdoblje je 30 dana.

![Izvješćivanje o pogreškama] (./media/active-directory-reporting-azure-portal/78.png "Izvješćivanje o pogreškama")

Ako želite, možete postaviti fokusa na određenu aplikaciju.

![Izvješćivanje o pogreškama] (./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Izvješćivanje o pogreškama")


Kada kliknete na dan u grafikonu korištenja aplikacije, prikazat će se detaljni popis aktivnosti za prijavu.


![Izvješćivanje o pogreškama] (./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Izvješćivanje o pogreškama")



**Dodaci za prijavu** mogućnost daje cjelovit pregled svih događaja za prijavu aplikacija.

![Izvješćivanje o pogreškama] (./media/active-directory-reporting-azure-portal/85.png "Izvješćivanje o pogreškama")

Pomoću biranje stupaca možete odabrati polja podataka koje želite prikazati.

![Izvješćivanje o pogreškama] (./media/active-directory-reporting-azure-portal/column_chooser.png "Izvješćivanje o pogreškama")



### <a name="filtering-sign-ins"></a>Filtriranje dodatke za prijavu

Dodaci za prijavu možete filtrirati prema vremenskom razdoblju za ograničavanje prikazane podatke.

![Izvješćivanje o pogreškama] (./media/active-directory-reporting-azure-portal/927.png "Izvješćivanje o pogreškama")


Drugi način da biste filtrirali stavke aktivnosti prijave je da biste potražili određene stavke.
Način pretraživanja omogućuje opsega vaše znak dodataka oko određene **korisnike**, **grupe** ili **aplikacije**.


![Izvješćivanje o pogreškama] (./media/active-directory-reporting-azure-portal/84.png "Izvješćivanje o pogreškama")

## <a name="audit-logs"></a>Zapisnika nadzora

Zapisnike nadzora u servisu Azure Active Directory sadrže zapise aktivnosti sustava za usklađenost.

Postoje tri glavna kategorije za nadzor povezane aktivnosti Azure portalu:

- Korisnici i grupe   

- Aplikacija

- Direktorija   


Popis svih aktivnosti nadzora izvješća potražite u članku [popis nadzora izvješće događaja](active-directory-reporting-audit-events.md#list-of-audit-report-events).


Ulaza za sve podatke za nadzor je **zapisnike nadzora** u odjeljku **aktivnosti** **Azure Active Directory**.


![Nadzor] (./media/active-directory-reporting-azure-portal/61.png "Nadzor")


Zapisnik nadzora sadrži prikaz popisa koji se prikazuje na Glumci (koji je), aktivnosti (što) i na ciljnih web-mjesta.


![Nadzor] (./media/active-directory-reporting-azure-portal/345.png "Nadzor")


Klikom na stavku u prikazu popisa možete dobiti dodatne detalje o njoj.

![Nadzor] (./media/active-directory-reporting-azure-portal/873.png "Nadzor")




### <a name="users-and-groups-audit-logs"></a>Korisnici i grupe zapisnika nadzora


S korisnika i izvješća o nadzoru grupne, možete dobiti odgovore na pitanja kao što su:

- Što je izričito vrste ažuriranja primijenjena korisnike?

- Koliko korisnika promijenjene?

- Koliko lozinke promijenjene?

- Što je administrator dovršili u imeniku?

- Što su grupe koje su dodane?

- Postoje li grupe s članstva?

- Vlasnici grupe promijenjene?

- Licence za koje su dodijeljene grupi ili korisnika?


Ako samo želite pregledati nadzora podatke koji se odnose na korisnike i grupe, filtrirani prikaz u odjeljku **zapisnika nadzora** možete pronaći u odjeljku **aktivnosti** **korisnika**i grupa.


![Nadzor] (./media/active-directory-reporting-azure-portal/93.png "Nadzor")


### <a name="application-audit-logs"></a>Aplikacija zapisnika nadzora

Sa sustavom aplikacije nadzora izvješća, možete dobiti odgovore na pitanja kao što su:

- Što su aplikacije koje se dodaje ili ažurira?

- Što su aplikacije koje su uklonjene?

- Načelo servis za aplikaciju se promijenilo?

- Nazivi aplikacije promijenjene?

- Tko dodijelili dozvole u aplikaciju?


Ako samo želite pregledati nadzora podatke koji su povezani s aplikacijama, filtrirani prikaz u odjeljku **zapisnika nadzora** možete pronaći u odjeljku **aktivnosti** **aplikacijama**.


![Nadzor] (./media/active-directory-reporting-azure-portal/134.png "Nadzor")


### <a name="filtering-audit-logs"></a>Filtriranje zapisnika nadzora

Izvješće o nadzoru možete filtrirati prema vremenskom razdoblju za ograničavanje prikazane podatke.

![Nadzor] (./media/active-directory-reporting-azure-portal/324.png "Nadzor")

Drugi način da biste filtrirali stavke zapisnik nadzora je da biste potražili određene stavke.

![Nadzor] (./media/active-directory-reporting-azure-portal/237.png "Nadzor")

## <a name="next-steps"></a>Daljnji koraci

Pogledajte [Vodič za izvješćivanje o pogreškama Azure Active Directory](active-directory-reporting-guide.md).
