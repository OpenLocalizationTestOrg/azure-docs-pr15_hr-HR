<properties
    pageTitle="Provjera dva koraka za otklanjanje poteškoća s | Microsoft Azure"
    description="Ovaj dokument pronaći ćete korisnicima informacije o tome što učiniti ako se naiđete na problem s Azure višestruke provjere autentičnosti."
    services="multi-factor-authentication"
    keywords = "multifactor provjere autentičnosti klijent, problem provjere autentičnosti, ID korelacije"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="having-trouble-with-two-step-verification"></a>Imate li poteškoća s dva koraka provjere

U ovom se članku govori o neki problemi koje možete naići potvrdu dva koraka. Ako imate problem nije uključen ovdje, navedite detaljne povratne informacije u odjeljak Komentari tako da možemo poboljšati.

## <a name="i-lost-my-phone-or-it-was-stolen"></a>U slučaju gubitka mobitela ili ga je krađe

Da biste se vratili svoj račun na dva načina. Prvi je za prijavu pomoću zamjenski provjere autentičnosti telefonski broj, ako jedan ste postavili. Drugi je od administratora zatražite da isključite postavke.

Ako na telefon je gubitka ili krađe, preporučujemo i imate li vaš administrator ponovno postavljanje lozinki vaše aplikacije te poništite bilo pamti uređaja. Ako vaš administrator nije li kako to postigli, pokažite ih na članak: [Upravljanje korisnicima i uređajima](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords).


### <a name="use-an-alternate-phone-number"></a>Koristite zamjenski broj

Ako ste postavili više mogućnosti provjere valjanosti, uključujući sekundarne telefonski broj ili autentikatora aplikacije na neki drugi uređaj, koristite nešto od sljedećeg da biste se prijavili.

Da biste se prijavili pomoću zamjenski telefonski broj, slijedite ove korake:

1. Prijavite se uobičajen način.
2. Kada se to od vas zatraži da biste dodatno provjerite je li vaš račun, odaberite **koristi drugi potvrdu mogućnost**.

    ![Različite provjere valjanosti](./media/multi-factor-authentication-end-user-manage/differentverification.png)

3. Odaberite telefonski broj koji imate pristup.

    ![Zamjenski telefon](./media/multi-factor-authentication-end-user-manage/altphone2.png)

4. Nakon što ste vratili na vašem računu, [Upravljanje postavkama](multi-factor-authentication-end-user-manage-settings.md) da biste promijenili provjere autentičnosti telefonski broj.

>[AZURE.IMPORTANT]
>Važno je da konfiguriranje sekundarne provjere autentičnosti telefonski broj. Ako vaš primarni telefonski broj i aplikacije za mobilne uređaje na istom telefonu, koje morate treću mogućnost ako izgubite ili krađe telefonu.

### <a name="clear-your-settings"></a>Isključite postavke

Ako niste konfigurirali telefonskog broja sekundarne provjeru autentičnosti, a zatim morat ćete obratite se administratoru za pomoć. Uputite ih isključite postavke tako da se sljedeći put prijave, od vas će se zatražiti da biste [postavili račun](multi-factor-authentication-end-user-first-time.md) ponovno.


## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Koje se primanje tekst ili poziva na telefonu

Postoji nekoliko razloga zašto možete pokušati prijaviti, ali nećete primati teksta ili telefonskom pozivu. Ako uspješno primite tekstova ili telefonske pozive na telefon u prošlosti, zatim to je vjerojatno problem kod davatelja telefon, a nije vaš račun. Provjerite da imate dobar ćeliju signala, a ako želite primati tekstne poruke provjerite je li da vaša tarifa za Telefon i usluge podržava tekstne poruke.

Ako ste waited nekoliko minuta za tekst ili poziv, najbrži način da biste na račun je da biste isprobali neku drugu mogućnost.

1. Odaberite **koristite drugi potvrdu mogućnost** na stranici koja čeka provjeravanje.

    ![Različite provjere valjanosti](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

2. Odaberite telefonski broj ili isporuke način koji želite koristiti.

    Ako ste primili više kodova za potvrdu, funkcionira samo najnoviji od njih.

Ako nemate drugi način konfigurirana, obratite se administratoru i zamolite ga da biste očistili postavki. Kada se sljedeći put prijavite, zatražit će se da biste [postavili višestruke provjere autentičnosti](multi-factor-authentication-end-user-first-time.md) ponovno.


Ako često kašnjenja zbog signal neispravni ćelije, preporučujemo da koristite [Microsoft autentikatora aplikacija](multi-factor-authentication-microsoft-authenticator.md) na vašem Pametni telefon. Aplikaciju možete generiranje slučajnog sigurnost šifre koju koristite za prijavu u, a te šifre ne zahtijevaju bilo koju ćeliju signal ili internetska veza.


## <a name="app-passwords-are-not-working"></a>Aplikacija lozinke ne funkcioniraju

Najprije provjerite je li ispravno unijeli lozinku aplikacije.  Ako i dalje ne funkcionira pokušajte prijava u i [stvorite novu aplikaciju lozinku](multi-factor-authentication-end-user-app-passwords.md).  Ako to ne funkcionira, obratite se administratoru sustava i ih [izbrišite svoje postojeće lozinke aplikacije](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords) , a zatim možete stvoriti novi.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Koje niste pronašli odgovor s mojim problemom s.

Ako ste pokušali korake za otklanjanje poteškoća, ali se izvodi na probleme, obratite se administratoru ili osobe koja je postavila višestruke provjere autentičnosti za vas. Trebali biste moći pomoći.

Osim toga, možete postavite pitanje na [forumima za Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) ili [obratiti službi za podršku](https://support.microsoft.com/contactus) i smo ćete odgovaranje na problem čim možemo.

Ako se obratite podršci, dodajte sljedeće podatke:

- **Korisnički ID** – što je adresa e-pošte koju ste pokušali prijavljujete?
- **Opći opis pogreške** – što točno poruka o pogrešci niste li možda vidjeti?  Ako je ne prikazuje se poruka o pogrešci, opišite neočekivano ponašanje primijetili detaljno.
- **Stranica** – koju stranicu jeste li na kada se pojavi pogreška (Navedite URL)?
- **KodPogreške** - specifična Šifra pogreške primate.
- **ID sesije** - id određene sesije primate.
- **ID korelacije** – što je kod id korelacije koji su generirani prilikom korisnika prikazivalo pogrešku.
- **Vremenska oznaka** – što je precizno datum i vrijeme prikazivalo pogreške (uključuje se vremenska zona)?

Većinu prednosti ove informacije možete pronaći na stranici za prijavu. Kada se ne Provjerite svoje prijave u vremenu, odaberite **Prikaz detalja**.

![Prijavite se u detalje o pogrešci](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Uključivanju sljedećih informacija pridonose da biste najbrže što može riješiti problem.

## <a name="related-topics"></a>Povezane teme
- [Upravljanje postavkama za potvrdu dva koraka](multi-factor-authentication-end-user-manage-settings.md)  
- [Najčešća pitanja vezana uz Microsoft Authenticator aplikacija](multi-factor-authentication-app-faq.md)
