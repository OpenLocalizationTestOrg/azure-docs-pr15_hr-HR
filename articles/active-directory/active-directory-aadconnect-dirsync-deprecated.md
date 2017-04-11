<properties
    pageTitle="Nadogradnja DirSync i sinkronizacija Azure AD | Microsoft Azure"
    description="U članku se opisuje nadogradili DirSync i sinkronizacija Azure AD za Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>


# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>Nadogradnja sinkronizaciju Windows Azure Active Directory ("DirSync") i sinkronizaciju servisa Azure Active Directory ("Azure AD sinkroniziranje")
Azure AD Connect je najbolji način za povezivanje lokalnog imenika s Azure AD i Office 365. To je provedite nadogradili na Azure AD Connect Windows Azure Active Directory Sync (DirSync) ili Azure AD sinkronizaciju te alate sada ukinuta i će doći do kraja podrška na Travanj 13, 2017.

Alati za sinkronizaciju dvije identitet koji su ukinuta su koje se nude za kupce jedan skup stabala (DirSync) i za više skupa stabala i druge napredne kupcima (Azure AD Sync). Te starije alate zamijenjene jedinstveno rješenje koji je dostupan za sve scenarije: Azure AD Connect. Nudi nove funkcije, poboljšane značajke i podrška za novih scenarija. Da biste mogli nastaviti sinkronizirati lokalnih podataka identiteta za Azure AD i sustava Office 365, preporučujemo da nadogradite Azure AD Connect.

Zadnje verzije DirSync objavljen u srpnju 2014, a zatim zadnje verzije Azure AD sinkronizaciju izdavanja svibanj 2015.

## <a name="what-is-azure-ad-connect"></a>Što je Azure AD Connect
Azure AD Connect nije naslijediti DirSync i Azure AD sinkronizaciju. Scenariji za sve se kombinira pri sljedeća dva podržana. Dodatne informacije o tome u [integriranje vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Raspored postupku

Datum | Komentar
 --- | ---
Travanj 13, 2016 | Windows Azure Active Directory Sync ("DirSync") i Microsoft Azure Active Directory Sync ("Azure AD sinkroniziranje") se objaviti kao što je zastario.
Travanj 13, 2017 | Podržava završava. Korisnici više moći otvoriti slučaja podršku bez nadogradnje Azure AD Connect najprije.

## <a name="how-to-transition-to-azure-ad-connect"></a>Kako prijelaz na Azure AD Connect
Ako koristite DirSync na dva načina možete nadograditi: lokalno nadogradnje i paralelnih implementacije. Nadogradnju na mjestu preporučuje se većina korisnika, a ako imate na nedavni operacijski sustav i manje od 50.000 objekte. U drugim slučajevima preporučuje se da biste učinili paralelno implementacije koje će se konfiguracijom DirSync premjestiti novi poslužitelj koji se izvodi Azure AD Connect.

Ako koristite Azure AD sinkronizacijom preporučuje se nadogradnju na mjestu. Ako želite, moguće je da biste instalirali novi poslužitelj za Azure AD Connect paralelno i provesti swing migraciju iz poslužitelj za Azure AD sinkronizaciju Azure AD Connect.

Rješenja | Scenarij
----- | -----
[Nadogradnja s DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Ako imate postojeću poslužitelj DirSync već pokrenut.</li>
[Nadogradnju iz Azure AD sinkronizacije](active-directory-aadconnect-upgrade-previous-version.md)| <li>Ako premještate iz Azure AD sinkronizaciju.</li>

Ako želite da biste vidjeli kako učiniti nadogradnju na mjestu s DirSync Azure AD Connect, zatim pogledajte ovaj videozapis 9 kanala:

> [AZURE.VIDEO azure-active-directory-connect-in-place-upgrade-from-legacy-tools]

## <a name="faq"></a>NAJČEŠĆA PITANJA
**P: koje ste primili obavijest e-poštom iz tima Azure i/ili poruke iz centra za poruke sustava Office 365, ali koristim za povezivanje.**  
Obavijesti i poslao klijentima pomoću Azure AD Connect broj izdanja 1.0. \*.0 (pomoću pre 1.1 izdanje). Microsoft preporučuje klijente ostati u toku s Azure AD Connect izdanja. S 1.1 značajku [Automatska Nadogradnja](active-directory-aadconnect-feature-automatic-upgrade.md) će vam tako biti jednostavno uvijek imate najnoviju verziju Azure AD Connect instaliran.

**P: će DirSync/Azure AD sinkronizaciju prekinuti rad na Travanj 13, 2017?**  
ne. Datum kada ih više moći možete komunicirati s Azure AD će se objaviti kasnije. Moći da biste pronašli informacije u ovoj temi kada su dostupni.

**P: koje DirSync verzije možete nadograditi iz?**  
Podržana je za nadogradnju s bilo kojeg DirSync izdanje trenutno koristi.

**P: što je s Azure AD poveznik za FIM-MIM?**  
Poveznik za Azure AD **za FIM-MIM nije** objaviti kao što je zastario. Je na **značajku Zamrzavanje**; dodaje bez novih funkcija te primi nema popravci programskih pogrešaka. Microsoft preporučuje korisnike koji koriste planirati da biste premjestili iz nje Azure AD Connect. Preporučuje za sve nove implementacijama korištenja koji se neće pokrenuti. Ovaj poveznik će se objaviti ukinuta u budućnosti.

## <a name="additional-resources"></a>Dodatni resursi

* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
