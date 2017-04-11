<properties
    pageTitle="Azure AD Connect: Najčešća pitanja vezana uz | Microsoft Azure"
    description="Ova stranica sadrži najčešća pitanja o Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-faq"></a>Najčešća pitanja vezana uz za Azure AD Connect

## <a name="general-installation"></a>Općenito instalacije
**P: instalacije funkcioniraju ako Azure AD globalni administrator ima 2FA omogućeno?**  
S izdanja iz veljače 2016, to nije podržano.

**P: postoji način da biste instalirali Azure AD Connect nenadziranog?**  
Podržana je samo za instalaciju Azure AD Connect pomoću čarobnjaka za instalaciju. Instalacija nenadziranog i tihu nije podržana.

**P: imaju skupa stabala je koju ne možete uspostaviti jednu domenu. Kako instalirati Azure AD Connect?**  
S izdanja iz veljače 2016, to nije podržano.

**P: agent stanja AD DS funkcionira na poslužitelju core?**  
Da. Nakon instalacije agenta, dovršite postupak registracije pomoću sljedećih cmdleta ljuske PowerShell: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Mreža
**P: imaju vatrozid, mrežni uređaj ili nešto drugo koji ograničava veze maksimalno vrijeme može biti otvoreni u mojoj mreži. Koliko dugo Moje vremensko ograničenje praga za klijenta strani treba prilikom korištenja Azure AD Connect?**  
Sve umrežavanje, fizičkih uređaja ili nešto drugo koji ograničava maksimalno vrijeme veze može ostati otvoren trebali biste koristiti prag od najmanje pet minuta (300 sekundi) veze između poslužitelja na kojem je instaliran Azure AD Connect klijenta i Azure Active Directory. To se odnosi na sve hitnom Microsoft Identity alate za sinkronizaciju.

**P: su SLDs (jedan natpis domena) podržane?**  
Ne, Azure AD Connect ne podržava lokalnog šuma/domene pomoću SLDs.

**P: su "točkaste" NetBios pod nazivom podržane?**  
Ne, Azure AD Connect ne podržava lokalnog šuma/domene kojoj NetBios naziv sadrži razdoblje "." u nazivu.

## <a name="federation"></a>Vanjski pristup
**P: što učiniti ako se prikaže poruku e-pošte koji se traži da biste obnovili Moje certifikat za Office 365**  
Slijedite upute za koji je naveden u temi [obnoviti certifikate](active-directory-aadconnect-o365-certs.md) kako obnoviti certifikat.

**P: imaju "Automatski ažurirati potrebe za oslanjanjem strana" odredite proslave potrebe za oslanjanjem O365. Imati ništa poduzimati prilikom prelaska Moje token automatski potpisni certifikat?**  
Slijedite upute koje je navedene u članku [obnavljanje certifikata](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Okruženje
**P: je podržana da biste preimenovali poslužiteljem nakon instalacije Azure AD Connect?**  
ne. Promjena poslužitelja naziva uzrokovat će modula za sinkroniziranje da nećete moći povezati s bazom podataka sustava SQL i servis će moći pokrenuti.

## <a name="identity-data"></a>Identitet podataka
**P: UPN-a (userPrincipalName) atributa u Azure AD ne odgovara lokalno UPN - Zašto?**  
Potražite u sljedećim člancima:

- [Korisnička imena u sustavu Office 365, Azure ili Intune ne podudaranje lokalnog UPN-a ili ID za prijavu zamjenski](https://support.microsoft.com/en-us/kb/2523192)
- [Promjene nisu sinkronizirane alatom za Azure Active Directory Sync nakon što promijenite UPN korisnički račun da biste koristili različite pridruženoj domeni](https://support.microsoft.com/en-us/kb/2669550)

Možete konfigurirati i Azure AD dopustili modul sinkronizaciju da biste ažurirali na userPrincipalName, kao što je opisano u [Azure AD Connect sinkronizaciju servisa značajke](active-directory-aadconnectsyncservice-features.md).

## <a name="custom-configuration"></a>Prilagođena konfiguracija
**P: gdje su PowerShell cmdleti za Azure AD Connect navedenih?**  
Uz iznimku cmdleta navedenih na web-mjestu, drugi cmdleta ljuske PowerShell pronađeni u Azure AD Connect nisu podržani za korištenje klijentima.

* *P: mogu li koristiti "Izvoz poslužitelja poslužitelj uvoz" pronađeni u *Upravitelj sinkronizacije servisa* da biste se kretali konfiguracije servers? **  
ne. Ova mogućnost će dohvatiti sve konfiguracijske postavke i ne želite koristiti. Umjesto toga treba pomoću čarobnjaka za stvaranje osnovnu konfiguraciju na drugi poslužitelj i korištenje uređivač pravila za sinkronizaciju da biste generirali skripte komponente PowerShell da biste premjestili neki prilagođenog pravila s poslužitelja. Potražite u članku [Premještanje prilagođene konfiguracije iz Aktivno pripremna poslužitelj](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server).

## <a name="troubleshooting"></a>Otklanjanje poteškoća
**P: kako dobiti pomoć za Azure AD Connect?**

[Potražite u članku Microsoftove baze znanja (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

- Pretraživanje u programu Microsoft baze znanja (KB) za tehničke rješenja uobičajenih problema prijelom popravak o podršci za Azure AD Connect.

[Forumi za Microsoft Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

- Možete pretraživati i pregledavati za tehničkih pitanja i odgovore iz zajednice ili postavite vlastitu pitanje tako da kliknete [ovdje](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD Connect korisničke podrške](https://manage.windowsazure.com/?getsupport=true)

- Pomoću ove veze da biste dobili podršku putem portala za Azure.
