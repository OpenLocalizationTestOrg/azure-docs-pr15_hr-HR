<properties
    pageTitle="Konfiguriranje Registracija automatsko uređaja za uređaje domene pridruženo Windows 8.1 | Microsoft Azure"
    description=" Upute za konfiguriranje pravilnika grupe za Windows 8.1 domene pridruženo uređaje da biste automatski morate registrirati Azure AD. "
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="Markvi"/>

# <a name="configure-automatic-device-registration-for-windows-81-domain-joined-devices"></a>Konfiguriranje Registracija automatsko uređaja za uređaje domene pridruženo Windows 8.1

Active Directory pravila za grupe možete koristiti da biste konfigurirali uređajima domene pridruženo Windows 8.1 automatski registrirati s Azure AD. Da biste konfigurirali pravilnika grupe, morate imati barem jedan domene spojene Windows Server 2012 R2 ili Windows 8.1 računala pomoću značajke za upravljanje pravilnikom grupe instaliran. Kada se to pravilo grupe je omogućeno za vašu domenu, bilo koji korisnik domene koji zapisuje u računalo će se automatski i tihu registrirati s objektom uređaj u Azure AD. U Azure AD za sve korisnike registrirani fizički uređaj će biti jedan objekt uređaja. Svakako pročitajte i dovršili preduvjeti iz automatskog Registracija uređaja s Azure Active Directory for Windows Domain-Joined uređajima.

>[AZURE.NOTE]
 Najnovije upute za postavljanje Registracija automatsko uređaja potražite u člancima, [Postavljanje automatskog registracije domene Windows pridruženo uređaja s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).



## <a name="configure-the-group-policy-for-your-windows-81-domain-joined-devices"></a>Konfiguriranje pravila grupe za uređaje sa sustavima Windows 8.1 domene se pridružite

1. Otvorite upravitelj poslužitelja, a zatim otvorite **Alati** > **Upravljanje pravilnikom grupe**.
2. Iz upravljanje pravilnikom grupe, pomaknite se do čvor domene koja odgovara domene u kojem želite da biste omogućili **Automatsko pridruživanje radnom mjestu**.
3. Desnom tipkom miša kliknite **Objekti pravilnika grupe** , a zatim odaberite **Novo**. Naziv objekta pravilnika grupe, na primjer, **Automatsko pridruživanje radnom mjestu**. Kliknite **u redu**.
4. Desnom tipkom miša kliknite novi objekt pravilnika grupe, a zatim odaberite **Uređivanje**.
5. Dođite do **konfiguracije računala** > **pravila** > **Administrativni predlošci** > **komponente Windows** > **Uključivanje radnom mjestu**.
6. Desnom tipkom miša kliknite automatski radnom mjestu spoj klijentskim računalima, a zatim odaberite **Uređivanje**.
7. Odaberite omogućeno izborni gumb, a zatim kliknite Primijeni. Kliknite **u redu**.
8. Sada mogu povezati objekt pravilnika grupe na mjesto po izboru. Da biste omogućili ovo pravilo za sve domene koje se spajaju uređaje Windows 8.1 u tvrtki ili ustanovi, veza pravila grupe s domenom.

## <a name="unregistering-your-windows-81-domain-joined-devices"></a>Poništavanje registriranja domene Windows 8.1 pridruženo uređaja

Možete odabrati unregister uređajima Windows 8.1 domene pridružili tako da učinite sljedeće: izmjena na radnom mjestu uključivanje postavke pravilnika grupe stvorili u prethodnom odjeljku. Postavljanje na automatski radnom mjestu spoj klijentskim računalima pravila onemogućeno. To će spriječiti da novim uređajima automatsko pridruživanje na radnom mjestu.

Unregister postojeće strojeva Windows 8.1 domene pridruženo po jedna od dvije mogućnosti u nastavku:

* Mogućnost 1: **neregistriranja Windows 8.1 domene pridruženo uređaja koji koristi postavke PC -JA**
  1. Na uređaju Windows 8.1, idite na **Postavke PC -JA** > **mrežni** > **radnom mjestu**
  2. Odaberite **Napusti**.
Za svakog korisnika domene koja sadrži prijavljeni na računalo i je li automatski radnom mjestu pridruženo morate mogu ponoviti postupak.

* Mogućnost 2: Unregister Windows 8.1 domene spojene uređaj pomoću skripte
    1. Otvorite naredbeni redak na računalu Windows 8.1, a zatim pokrenite sljedeću naredbu:` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
Ta se naredba morate pokrenuti u kontekstu svakog domene korisnika koji je potpisan na računalu.

##<a name="event-viewer--errors-for-windows-81-domain-joined-devices"></a>Preglednik događaja i pogreške za Windows 8.1 domene pridruženo uređaja

Zapisnika događaja sustava Windows na računalu Windows 8.1 prikazuje poruke koje su vezane uz Registracija uređaja. Tražit će poruke za uspješnih i neuspješnih događaje. 

Zapisnik događaja možete pronaći u događaja preglednika u odjeljku aplikacija i servisa **zapisnika** > **Microsoft** > **Windows > Uključivanje radnom mjestu**.

##<a name="additional-details"></a>Dodatne informacije

Pravilnik grupe omogućuje zadatka planirati na sustav koji se izvodi u kontekstu korisnika i aktivira na korisničku prijavu. Zadatak tihu će registrirati korisnicima i uređajima s Azure AD po dovršetku prijavu. Zadatka planirati nalazi se na uređajima Windows 8.1 u biblioteka rasporeda zadataka u odjeljku **Microsoft** > **Windows** > **Uključivanje radnom mjestu**. Zadatak će pokrenuti i registrirati sve korisnike servisa Active Directory te prijavu u. 

## <a name="additional-topics"></a>Dodatne teme
- [Pregled Azure Registracija uređaja za Active Directory](active-directory-conditional-access-device-registration-overview.md)
- [Registracija za automatsko uređaja s uređajima domene pridruženo Azure Active Directory za Windows 10](active-directory-conditional-access-automatic-device-registration.md)
- [Konfiguriranje Registracija automatsko uređaja za uređaje domene pridruženo Windows 7](active-directory-conditional-access-automatic-device-registration-windows7.md)

