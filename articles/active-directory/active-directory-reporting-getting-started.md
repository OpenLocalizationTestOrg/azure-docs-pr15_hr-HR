<properties
   pageTitle="Azure Active Directory izvješćivanja: Početak rada | Microsoft Azure"
   description="Popisi različite dostupnih izvješća u izvješćivanju Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="getting-started-with-azure-active-directory-reporting"></a>Početak rada s Azure Active Directory izvješćivanje o pogreškama

## <a name="what-it-is"></a>Što je to

Azure Active Directory (Azure AD) obuhvaća sigurnost, aktivnosti i izvješća o nadzoru za direktorija. Slijedi popis izvješća obuhvaća:

### <a name="security-reports"></a>Izvješća o sigurnosti

- Znak dodataka iz nepoznatih izvora
- Znak dodataka nakon većeg broja neuspješnih pokušaja
- Znak dodataka iz više geographies
- Znak dodataka iz IP adresa pomoću sumnjivoj aktivnosti
- Nepravilni aktivnosti za prijavu
- Znak dodataka vjerojatno zaražene uređaja
- Korisnici s anomalous aktivnosti za prijavu

### <a name="activity-reports"></a>Izvješća o aktivnosti

- Korištenje aplikacije: sažetka
- Korištenje aplikacije: detaljne
- Nadzorna ploča za aplikaciju
- Račun pogreške u dodjeljivanju
- Uređaji pojedinačnog korisnika
- Pojedinačne korisničke aktivnosti
- Izvješće o aktivnosti grupe
- Izvješće o aktivnosti registraciju za ponovno postavljanje lozinke
- Aktivnosti za ponovno postavljanje lozinke

### <a name="audit-reports"></a>Izvješća o nadzoru

- Izvješće nadzora direktorija

> [AZURE.TIP] Za dodatne dokumentaciju na Azure AD izvješćivanja potražite u članku [Prikaz izvješća pristupa i korištenja](active-directory-view-access-usage-reports.md).



## <a name="how-it-works"></a>Kako funkcionira


### <a name="reporting-pipeline"></a>Izvješćivanje o pogreškama kanal

Kanal za izvješćivanje sastoji se od tri glavna koraka. Svaki put prijave ili postala je za provjeru autentičnosti, događa se sljedeće:

- Prvo autentičnost korisnika (uspješno ili neuspješno), a rezultat je pohranjena u bazama podataka za servisa Azure Active Directory.
- U pravilnim vremenskim razmacima, sve nedavne znak obrađuju dodaci. Sada naš sigurnost i algoritama anomalous aktivnosti pretraživanja svih nedavnih znak dodaci za sumnjivoj aktivnosti.
- Nakon obrade, izvješća su napisali, predmemorirani i poslužena na portalu za Azure klasični.

### <a name="report-generation-times"></a>Prijava vremena generacije

Zbog velik broj authentications i prijavite ins obradili platforme Azure AD, na zadnjoj znak dodataka obrađuju, u jedan sat stari. U koje se rijetko pojavljuju se slučajevima može proći do osam sati za obradu najnovije dodaci znak.

Možete pronaći najnovije obrađeni prijavu pomoću Provjera teksta pomoći pri vrhu svake izvješća.

![Tekst pomoći pri vrhu svake izvješća](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] Za dodatne dokumentaciju na Azure AD izvješćivanja potražite u članku [Prikaz izvješća pristupa i korištenja](active-directory-view-access-usage-reports.md).



## <a name="getting-started"></a>Početak rada


### <a name="sign-into-the-azure-classic-portal"></a>Prijavite se na portalu za Azure klasični

Najprije morate prijaviti na [portal za Azure klasični](https://manage.windowsazure.com) kao globalni ili administrator za usklađenost. Morate biti administrator servisa Azure pretplatu ili zajednički administratora ili koristite "Pristup Azure AD" Azure pretplate.

### <a name="navigate-to-reports"></a>Otvorite izvješća

Da biste pogledali izvješća, pomaknite se do kartica izvještaji na vrhu direktorija.

Ako je ovo prvi put da prikazujete izvješća, morat ćete pristajete da dijaloški okvir prije prikaza izvješća. To je da biste bili sigurni da je prihvatljiva za administratore u tvrtki ili ustanovi za prikaz tih podataka koje se smatraju osobne podatke u nekim Državama.

![Dijaloški okvir](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Istražite svako izvješće

Idite u svako izvješće da biste vidjeli podatke koji se prikupljaju i znak-ins obrađuju. Možete pronaći [popis svih izvješća ovdje](active-directory-reporting-guide.md).

![Sva izvješća](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>Preuzimanje izvješća kao CSV

Svako izvješće mogu se preuzeti kao CSV datoteke (vrijednosti razdvojene zarezom). Možete koristiti te datoteke u programu Excel, PowerBI ili drugih proizvođača analitički programa za daljnje analize podataka.

Da biste preuzeli svako se izvješće kao CSV, idite na izvješće pa kliknite "Preuzimanje" pri dnu.

![Gumb za preuzimanje](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] Za dodatne dokumentaciju na Azure AD izvješćivanja potražite u članku [Prikaz izvješća pristupa i korištenja](active-directory-view-access-usage-reports.md).





## <a name="next-steps"></a>Daljnji koraci

### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Prilagodba upozorenja za anomalous znak u aktivnosti

Idite na karticu "Konfiguriraj" imenik.

Pomaknite se do odjeljka "Obavijesti".

Omogućivanje i onemogućivanje u odjeljku "E-pošte obavijesti o Anomalous znak dodaci".

![U odjeljak obavijesti](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Integracija s Azure AD izvješćivanja API-JA

Potražite u članku [Uvod u rad s API izvješćivanje](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Višestruka provjera autentičnosti sudjelovati na korisnike

Odaberite korisnika u izvješću.

Kliknite gumb "Omogući MFA" pri dnu zaslona.

![Gumb višestruka provjera autentičnosti pri dnu zaslona](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] Za dodatne dokumentaciju na Azure AD izvješćivanja potražite u članku [Prikaz izvješća pristupa i korištenja](active-directory-view-access-usage-reports.md).




## <a name="learn-more"></a>uči više


### <a name="audit-events"></a>Događaje nadzora

Informirajte se o koji su se događaji revizije u direktoriju u [Azure Active Directory izvješća nadzornih događaja](active-directory-reporting-audit-events.md).

### <a name="api-integration"></a>Integracija API-JA

Pogledajte [Uvod u rad s API izvješćivanje](active-directory-reporting-api-getting-started.md) i [API referentnu dokumentaciju](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Stupanje u vezu

E-pošte [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) za povratne informacije, pomoć ili možda imate pitanja.

> [AZURE.TIP] Za dodatne dokumentaciju na Azure AD izvješćivanja potražite u članku [Prikaz izvješća pristupa i korištenja](active-directory-view-access-usage-reports.md).
