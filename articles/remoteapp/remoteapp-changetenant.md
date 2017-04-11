
<properties
    pageTitle="Promjena klijentu Azure Active Directory RemoteApp Azure | Microsoft Azure"
    description="Saznajte kako promijeniti klijentu Azure Active Directory pridružene Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="change-the-azure-active-directory-tenant-in-azure-remoteapp"></a>Promjena klijentu Azure Active Directory RemoteApp Azure

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Azure RemoteApp koristi Azure Active Directory (Azure AD) da biste omogućili korisničkog pristupa. Klijent samo Azure AD koje možete koristiti u Azure RemoteApp je povezan s pretplatom Azure. Možete pogledati povezane pretplate na stranici **Postavke** na portalu. Pregled **direktorija** stupac na kartici **pretplate** .

> [AZURE.NOTE] Da bi ta promjena uspjela, najprije uklonite sve korisnike s postojećem klijentu za Azure Active Directory iz svih zbirki Azure RemoteApp. Da biste to učinili, otvorite Azure Portal, idite na karticu **Azure RemoteApp** i otvorite svakoj zbirci Azure RemoteApp. Idite na karticu **korisnika** i uklanjanje korisnika koji pripadaju vaš trenutni klijent Azure Active Directory. Ponovite za sve postojeće zbirke Azure RemoteApp. Bez na taj način, neće moći da biste stvorili ili zakrpu zbirke.

Ako želite koristiti drugi klijent, koristite ove korake da biste promijenili pridruživanje pretplate:

1. Na portalu ukloniti sve korisnike Azure AD na koji ste dobili pristup zbirkama Azure RemoteApp. (Vidi bilješku iznad upute o tome kako to učiniti.)


2. Postavljanje Microsoftova računa (prije se zvao Live ID) kao administrator servisa. (Ne znate već jeste li administrator servisa? Možete saznati tako da kliknete **Postavke -> administratori**.) Sada, Evo kako ga promijeniti:
    1. Kliknite korisnika u gornjem desnom kutu, a zatim kliknite **Prikaz računa**.
    2. Kliknite pretplatu. Zatim na novu stranicu, pomaknite se prema dolje i kliknite **Uređivanje Detalji o pretplati** u desnom kutu. (Sortiranje srednjem slobodnog prostora ako taj pomaže vam ga pronašli.)
    3. Unesite Microsoftov račun za korisnika koji treba administratora servisa.

3. Sada odjavili s portala, a zatim se ponovno prijavite pomoću Microsoftova računa koje ste naveli u prethodnom koraku.


4. Kliknite **New -> aplikacije servisa -> aktivno direktorija -> direktorija -> Prilagođena stvaranje**.
5. U odjeljku **direktorija**, odaberite **Koristi postojeći imenik**. Ne možemo ćete morati sada ste odjavili s portala, pa odaberite **sam spremni ste odjavljeni sada**.
6. Prijavite se natrag u portalu kao globalni administrator direktorija za koju želite dodati. (Ako je već je nisu globalni administrator, bit ćete nakon round od prijavite, a zatim Odjava.)
7. Koje će se zatražiti prilikom prijave želite li koristiti postojećem klijentu za AD s pretplatom. Kliknite **Nastavi**, a zatim kliknite **odjavili**.
5. Ponovna prijava u nju i vratite se u **postavkama -> pretplate**. Odaberite pretplatu, a zatim **Uređivanje direktorija**. Odaberite Azure AD klijentu koji želite koristiti.



Možete odmah koristi novi Azure AD klijenta za kontrolu pristupa Azure pretplate i konfiguriranje korisničkog pristupa u Azure RemoteApp.
