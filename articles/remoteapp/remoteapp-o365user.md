
<properties 
    pageTitle="Kako koristiti Azure RemoteApp s korisničkim računima u sustavu Office 365 | Microsoft Azure"
    description="Saznajte kako pomoću servisa Azure RemoteApp Moje korisničke račune sustava Office 365"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-azure-remoteapp-with-office-365-user-accounts"></a>Kako koristiti Azure RemoteApp s korisničkim računima u sustavu Office 365

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Ako imate pretplatu na Office 365 imate Azure Active Directory kojoj su pohranjeni korisnička imena i lozinke za pristup servisima sustava Office 365. Ako, na primjer, kada korisnici aktivacija sustava Office 365 ProPlus oni provjeru autentičnosti Azure AD da biste provjerili licence. Većina korisnika želite koristiti isti direktorija s Azure RemoteApp.

Ako implementirate Azure RemoteApp najvjerojatnije koristite Azure pretplatu koja je povezana s različitim Azure AD. Da biste koristili direktorija sustava Office 365, morat ćete premjestiti Azure pretplate u tom direktoriju.

Informacije o tome kako uvesti klijentskim aplikacijama sustava Office 365 potražite u članku [kako koristiti pretplatu na Office 365 s Azure RemoteApp](remoteapp-officesubscription.md).
 
## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Faza 1: Registrirati besplatne pretplatu na Office 365 Azure Active Directory
Ako koristite portala za Azure klasični, slijedite korake u [dnevniku pretplate besplatne Azure Active Directory](https://technet.microsoft.com/library/dn832618.aspx) da biste dobili Administrativni pristup vašem Azure AD putem portala za upravljanje Azure. Kao rezultat postupak trebali biste moći prijaviti na portal za Azure i pogledajte postoji – direktorija sada neće se prikazati znatno jer je potpuni Azure pretplata pomoću Azure RemoteApp u neki drugi direktorij.

Imajte na umu ime i lozinku za administratorski račun koji ste stvorili u ovom ćete koraku – će biti potrebno u fazi 2.

Ako se pomoću portala za Azure, pogledajte [kako registrirati i aktivirati na besplatna Azure Active Directory pomoću portala Office 365](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-the-azure-ad-associated-with-your-azure-subscription"></a>Faza 2: Promjena Azure AD povezanog s pretplatom na Azure.
Ne možemo prolaze za promjenu pretplate Azure iz njegova trenutnog direktorija u sustavu Office 365 directory smo radili u fazi 1.

Slijedite upute u uputama iz članka [Promjena klijentu Azure Active Directory RemoteApp Azure](remoteapp-changetenant.md). Posebno obratite pažnju na sljedeće korake:

- Korak #1: Ako ste u ovu pretplatu implementirana Azure RemoteApp (ARA), provjerite je li uklonite sve korisničke račune za Azure AD iz neke zbirke ARA najprije prije bilo što drugo. Umjesto toga razmotrite brisanje bilo koje postojeće zbirke.
- Korak #2: To je ključnih koraka. Morate koristiti Microsoftov račun (npr. @outlook.com) kao Administrator servisa na pretplatu, to je zato što smo ne mogu se sve korisničke račune iz postojeće Azure AD povezan s pretplatom – ne možemo li smo nećete moći premjestiti u drugu Azure AD.
- : #4 prilikom dodavanja postojeći imenik, sustav će zatražiti da se prijavi pomoću administratorskog računa za taj imenik. Provjerite jeste li koristili administratorski račun s faza 1.
- #5 korak: Promjena direktorij nadređenog pretplatu za Office 365 direktorija. Rezultat mora biti da u odjeljku postavke -> pretplate na pretplatu popisi direktorija za Office 365. 
![Promjena direktorija nadređenog pretplate](./media/remoteapp-o365user/settings.png)
 

Sada je povezana sa sustavom Office 365 Azure AD; pretplate Azure RemoteApp možete koristiti postojeće korisničke račune za Office 365 s Azure RemoteApp!




