<properties
   pageTitle="Upravljanje direktorija za pretplatu na Office 365 u Azure | Microsoft Azure"
   description="Upravljanje u imeniku pretplatu Office 365 pomoću servisa Azure Active Directory i Azure klasični portal"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>Upravljanje direktorija za pretplatu na Office 365 u Azure

U ovom se članku opisuje kako upravljati direktorija koji je stvoren za pretplatu na Office 365, pomoću portala za Azure klasični. Morate biti Administrator servisa ili zajednički administrator Azure pretplate za prijavu na portalu za Azure klasični. Ako još nemate Azure pretplatu, možete registrirati za je [besplatno 30-dnevnu probnu verziju](https://azure.microsoft.com/trial/get-started-active-directory/) danas i implementaciju prvi oblak rješenja u odjeljku pet minuta, pomoću ove veze. Pripazite da koristite račun tvrtke ili obrazovne ustanove koji koristite za prijavu u Office 365.

Kada dovršite Azure pretplatu, prijavite se na portal Azure klasični, a pristup Azure servisima. Kliknite nastavak servisa Active Directory za upravljanje imenik potvrđuje korisnika sustava Office 365.

Ako već imate pretplatu na Azure, postupak upravljanja dodatne direktorija je i jasan. Goran Novak, na primjer, možda imaju pretplatu na Office 365 za Contoso.com. Već ima i Azure pretplatu koju on se registrirali pomoću svoj Microsoftov račun msmith@hotmail.com. U ovom slučaju kojima upravlja dva direktorija.

  Pretplate |  Office 365  |  Azure
  -------------- | ------------- | -------------------------------
  Zaslonsko ime |  Contoso  |     Zadani direktorij Azure Active Directory (Azure AD)
  Naziv domene  |  contoso.com  | msmithhotmail.onmicrosoft.com

Britanski upravljanja identitetima korisnika u imeniku tvrtke Contoso dok je on je prijavljen Azure koristeći svoj Microsoftov račun tako da mu možete omogućiti Azure AD značajke kao što su multifactor provjeru autentičnosti. Na sljedećem su dijagramu mogu pomoći za ilustraciju postupka.

![Dijagram da biste upravljali dviju nezavisnih direktorija](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

U ovom slučaju dva direktorija su međusobno nezavisni.

## <a name="to-manage-two-independent-directories"></a>Da biste upravljali dviju nezavisnih direktorija
Da bi Goran Novak da biste upravljali oba direktorija dok je on je prijavljen Azure kao msmith@hotmail.com, on mora poduzeti sljedeće korake:

> [AZURE.NOTE]
> Ove korake možete obaviti samo kada je korisnik prijavljen pomoću Microsoftova računa. Ako je korisnik prijavljen pomoću tvrtke ili obrazovne ustanove, a zatim mogućnost **Koristi postojeći imenik** nije dostupna. Račun tvrtke ili obrazovne ustanove je moguće provjeriti autentičnost isključivo njegov osnovne mape (to jest, imeniku gdje je pohranjena računa tvrtke ili škole i vlasništvu tvrtke ili obrazovne ustanove).

1.  Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com) kao msmith@hotmail.com.
2.  Kliknite **Novo** > **aplikacije servisa** > **Servisa Active Directory** > **direktorija** > **Stvoriti prilagođene**.
3.  Kliknite koristi postojeći imenik, a zatim potvrdite okvir **sam spremni ste odjavljeni sada** .
4.  Prijavite se na portalu za Azure klasični kao globalni administrator Contoso.onmicrosoft.com (na primjer, msmith@contoso.com).
5.  Kada se to od vas zatraži da biste **Contoso directory pomoću Azure?**, kliknite **Nastavi**.
6.  Kliknite **Odjavi se sada**.
7.  Prijavite se na portal Azure klasični kao msmith@hotmail.com. Contoso directory i zadani direktorij pojavljuju se proširenja za Active Directory.

Nakon tih koraka msmith@hotmail.com globalnog administratora u imeniku tvrtke Contoso.

## <a name="to-administer-resources-as-the-global-admin"></a>Za administraciju resurse kao globalni administrator
Sada pretpostavimo da n. n. treba Administracija web-mjesta i baze podataka resursa koje su vezane uz Azure pretplatu za msmith@hotmail.com. Prije nego što je možete učiniti, Goran Novak morate dovršiti sljedeće dodatne korake:

1.  Prijavite se u [Azure klasični portal](https://manage.windowsazure.com) pomoću računa administratora servisa Azure pretplate (u ovom primjeru msmith@hotmail.com).
2.  Prijenos pretplate na imenik tvrtke Contoso: kliknite **Postavke** > **pretplate** > odaberite pretplatu > **Uređivanje direktorija** > odaberite **Contoso (Contoso.com)**. Kao dio prijenos, sve tvrtke ili obrazovne ustanove uklanjaju se račune koji su dodatnih administratora pretplate.
3.  Dodajte n. n. kao dodatnih administratora pretplate: kliknite **Postavke** > **Administratori** > odaberite pretplatu > **Dodaj** > vrsta **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o odnos između pretplate i direktorija potražite u članku [kako je pridružen direktorij pretplatu](active-directory-how-subscriptions-associated-directory.md).
