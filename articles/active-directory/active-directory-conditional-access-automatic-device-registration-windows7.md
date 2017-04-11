<properties
    pageTitle="# Konfiguriranje Registracija automatsko uređaja za uređaje domene pridruženo Windows 7 | Microsoft Azure"
    description="Upute za konfiguriranje domene sustava Windows 7 pridruženo uređaji za automatski Registriranje Azure AD. te korake za implementaciju softvera paket za registraciju uređaja na vašu domenu u sustavu Windows 7 pridruženo uređaja pomoću sustava raspodjele softver kao što je Upravitelj konfiguracije centar sustava."
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
    ms.author="MarkVi"/>

# <a name="configure-automatic-device-registration-for-windows-7-domain-joined-devices"></a>Konfiguriranje Registracija automatsko uređaja za uređaje domene združeni Windows 7

Kao administrator, možete konfigurirati uređajima domene pridruženo Windows 7 automatski registrirati s Azure AD. Da biste to napravili vlastite domene u sustavu Windows 7 morate uvesti paket softver za registraciju uređaja pridruženo uređaja pomoću sustava raspodjele softver kao što je Upravitelj konfiguracije centar sustava. Svakako pročitajte i dovršili preduvjeti automatsko Registracija uređaja s Azure Active Directory za Windows domene pridruženo uređajima na popisu.

>[AZURE.NOTE]
 Najnovije upute za postavljanje Registracija automatsko uređaja potražite u člancima, [Postavljanje automatskog registracije domene Windows pridruženo uređaja s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

##<a name="installing-the-device-registration-software-package-on-windows-7-domain-joined-devices"></a>Instalacija softvera paket za registraciju uređaja na sustavu Windows 7 domene pridruženo uređaja

Registracija uređaja u sustavu Windows 7 nije dostupan kao [koje je moguće preuzeti MSI paketa](https://connect.microsoft.com/site1164). Paket mora biti instaliran na računalima sustava Windows 7 pridruženim domena aktivnog imenika. Treba uvesti paket koji se koristi sustav raspodjele softver kao što je Upravitelj konfiguracije centar sustava. MSI paketa podržava na standardni tihu mogućnosti instalacije pomoću na/quiet parametar.
Paket softver je preuzeti na adresi sustava [Microsoft povezivanje web-mjesta](https://connect.microsoft.com/site1164). Ovdje možete odabrati i preuzimanje radnog prostora uključivanje u sustavima Windows 7.

![](./media/active-directory-conditional-access/device-registration-process-windows7.gif)

## <a name="workplace-join-with-azure-active-directory"></a>Uključivanje radnog prostora s Azure Active Directory
Registracija uređaja za uređaje Windows 7 domene združeni potrebna ili ne sadrži korisničko sučelje. Kada je instaliran na računalu, bilo koji korisnik domene koji zapisuje u računalo će se automatski i tihu registrirati s objektom uređaj u Azure AD. U Azure AD za sve korisnike registrirani fizički uređaj će biti jedan objekt uređaja.

Instalacijski program stvara zadatka planirati sustav koji se izvodi u kontekstu korisnika i aktivira na korisničku prijavu. Zadatak tihu registrira korisnika i uređaj s Azure AD kada korisnik znak u dovršetka.
Zadatka planirati pronaći ćete u biblioteka rasporeda zadataka u odjeljku **Microsoft** > **Uključivanje radnom mjestu**.
Zadatak će pokrenuti i registrirati sve korisnici servisa Active Directory koji Prijava na računalu.
Na sljedećoj slici navedene su detaljne upute za registraciju automatsko uređaja.

![](./media/active-directory-conditional-access/automatic-device-registration-windows7.png)

1. Korisnik (zaduženih za informacije) prijavi na sustavu Windows 7 klijentsko računalo pomoću vjerodajnica domene servisa Active Directory.
1. Na radnom mjestu pridružite se izvršava zakazani zadatak.
1. AD fs pomoću integrirane provjere autentičnosti u sustavu Windows tihu autentičnost korisnika.
1. Računalu sa sustavom Windows 7 je registrirana korisniku u Azure AD.
1. U Azure AD stvara se objekt uređaja i certifikata. Predstavlja objekt na user@device.
1. Certifikat za uključivanje radnom mjestu pohranjen na računalu.

## <a name="unregistering-your-windows-7-domain-joined-devices"></a>Poništavanje registriranja domene sustava Windows 7 pridruženo uređaja

Možete odabrati unregister uređajima Windows 7 domene pridružili tako da učinite sljedeće: Deinstalacija uključivanje radnom mjestu softverski paket s vaše domene u sustavu Windows 7 pridruženo uređaja pomoću sustava raspodjele softver kao što je Upravitelj konfiguracije centar sustava.

Zatim otvorite naredbeni redak na računalu Windows 7 i pokrenite sljedeću naredbu Odjava uređaj:

    %ProgramFiles%\Microsoft Workplace Join\AutoWorkplace.exe /leave

>[AZURE.NOTE]
>Ta se naredba morate pokrenuti u kontekstu svakog domene korisnika koji je potpisan na računalu.
Preglednik događaja i pogrešaka u sustavima Windows 7 domene pridruženo uređaja.

Zapisnika događaja sustava Windows na računalu Windows 7 prikazat će se poruka koji se odnose na radnom mjestu uključiti. Poruke možete pronaći za uspješnih i neuspješnih radnom mjestu uključivanje događaje. Zapisnik događaja možete pronaći u događaja preglednika u odjeljku Zapisnici programa i servisa > Uključivanje Microsoftovo radno mjesto.

## <a name="additional-topics"></a>Dodatne teme

- [Pregled Azure Registracija uređaja za Active Directory](active-directory-conditional-access-device-registration-overview.md)
- [Registracija za automatsko uređaja s uređajima Azure Active Directory for Windows Domain-Joined](active-directory-conditional-access-automatic-device-registration.md)
- [Konfiguriranje Registracija automatsko uređaja za uređaje domene pridruženo Windows 8.1](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Registracija za automatsko uređaja s uređajima domene pridruženo Azure Active Directory za Windows 10](active-directory-azureadjoin-devices-group-policy.md)
