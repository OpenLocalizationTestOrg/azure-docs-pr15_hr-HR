<properties
    pageTitle="Što su lozinke aplikacije u Azure MFA?"
    description="Ova stranica pomoći će korisnici razumjeti što su lozinke aplikacije i koje se koriste za s uzima u obzir za Azure MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>



# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Što su lozinke aplikacije u Azure višestruka provjera autentičnosti?

Određene aplikacije koje nisu preglednik, kao što su Apple klijent nativni e-pošte koja koristi Exchange Active Sync, trenutno ne podržava višestruke provjere autentičnosti. Višestruka provjera autentičnosti je omogućena po korisniku. To znači da ako korisnik omogućena za multi-factor authentication, a oni pokušavate koristiti aplikacije koje nisu preglednika, oni neće biti moguće da biste to učinili. U aplikaciji lozinka omogućuje to da se pokreće.

>[AZURE.NOTE] Moderna provjere autentičnosti za klijente za Office 2013
>
> Klijenti za Office 2013 (uključujući Outlook) sada podržava novi protokole za provjeru autentičnosti i mogu biti omogućene za podršku višestruke provjere autentičnosti.  To znači da kada je omogućen, aplikacija lozinke nisu potrebni za korištenje s klijentskim programima sustava Office 2013.  Dodatne informacije potražite u članku [Office 2013 Moderna provjere autentičnosti javno pretpregled objaviti](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

## <a name="how-to-use-app-passwords"></a>Kako koristiti aplikaciju lozinke

Slijede neki napomene o korištenju lozinki za aplikacije.

- Stvarni lozinka automatski se generiraju i nije naveden korisnik. To je jer je teže napadaču pogoditi automatski generirani lozinku i sigurnija.
- Trenutno postoji ograničenje od 40 lozinke po korisniku. Ako pokušate stvoriti nakon što ste dostigli ograničenje, zatražit će se da biste izbrisali jedan vaše postojeće lozinke aplikacije da biste stvorili novi.
- Preporučuje se da lozinke aplikacije mogu stvarati po uređaja, a ne svaku aplikaciju. Na primjer, možete stvoriti jednu aplikaciju lozinku za vaše računalo i koristiti aplikacije lozinku svih aplikacija na tom prijenosno računalo.
- Ponudit će vam je lozinka aplikacije prvi put se prijavite u.  Ako vam je potrebna dodatna one, možete stvoriti ih.

![Postavljanje](./media/multi-factor-authentication-end-user-app-passwords/app.png)

Nakon što dodate u aplikaciju lozinku, to umjesto koristite izvornu lozinku s te aplikacije koje nisu preglednika.  Tako da na primjer, ako koristite višestruke provjere autentičnosti i Apple klijent nativni e-pošte na telefonu.  Pomoću aplikacije lozinku tako da ga možete zaobići višestruke provjere autentičnosti i nastaviti s radom.

## <a name="creating-and-deleting-app-passwords"></a>Stvaranje i brisanje aplikacije lozinki
Tijekom vaša početna prijavu ponudit će vam je lozinka aplikacije koje možete koristiti.  Uz to možete stvoriti i kasnije brisanje lozinki aplikacije.  Kako to učiniti ovisi o tome kako koristite višestruke provjere autentičnosti.  Odaberite onaj te najčešće se na vas odnosi.

Kako koristiti višestruke provjere authentiation|Opis
:------------- | :------------- |
[Mogu se koristiti uz Office 365](#creating-and-deleting-app-passwords-with-office-365)|  To znači da će želite stvoriti aplikaciju lozinke putem portala sustava Office 365.
[ne znam](#creating-and-deleting-app-passwords-with-myapps-portal)|To znači da ćete stvoriti aplikaciju lozinke putem [https://myapps.microsoft.com](https://myapps.microsoft.com)
[Koristim za Microsoft Azure](#create-app-passwords-in-the-azure-portal)| To znači da ćete stvoriti aplikaciju lozinke putem portala za Azure.

## <a name="creating-and-deleting-app-passwords-with-office-365"></a>Stvaranje i brisanje lozinki aplikacija sa sustavom Office 365

Ako koristite višestruke provjere autentičnosti u sustavu Office 365 želite za stvaranje i brisanje lozinki aplikaciju putem portala sustava Office 365.

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Da biste stvorili aplikacije lozinke na portalu za Office 365
--------------------------------------------------------------------------------

1. Prijavite se na [portal sustava Office 365](https://login.microsoftonline.com/).
2. U gornjem desnom kutu odaberite na miniaplikacije, a zatim postavke sustava Office 365.
3. Kliknite dodatne sigurnosti provjere.
4. Na desnoj strani kliknite vezu koja vas obavještava da **Ažuriranje Moji telefonski brojevi koji se koriste za sigurnost računa.** 
 ![Postavljanje](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. To će vas odvesti na stranicu omogućit će vam da biste promijenili postavke.
![Oblak](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Pri vrhu, uz dodatne sigurnosti potvrdu, kliknite na **aplikacije lozinki.**
7. Kliknite **Stvori**.
![Oblak](./media/multi-factor-authentication-end-user-app-passwords-create-o365/apppass.png)
8. Unesite naziv aplikacije lozinku, a zatim kliknite **Dalje**.
![Stvaranje lozinke za aplikacije](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
9. Kopirajte lozinku aplikacije u međuspremnik i zalijepiti u aplikaciju.
![Stvaranje lozinke za aplikacije](./media/multi-factor-authentication-end-user-app-passwords/create2.png)


### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>Da biste izbrisali aplikaciju lozinke pomoću portala za Office 365
--------------------------------------------------------------------------------


1. Prijavite se na [portal sustava Office 365](https://login.microsoftonline.com/).
2. U gornjem desnom kutu odaberite na miniaplikacije, a zatim postavke sustava Office 365.
3. Kliknite dodatne sigurnosti provjere.
4. Na desnoj strani kliknite vezu koja vas obavještava da **Ažuriranje Moji telefonski brojevi koji se koriste za sigurnost računa.** 
 ![Instalacije](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. To će vas odvesti na stranicu omogućit će vam da biste promijenili postavke.
![Oblak](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Pri vrhu, uz dodatne sigurnosti potvrdu, kliknite na **aplikacije lozinki.**
7. Uz aplikaciju lozinku koju želite izbrisati, kliknite **Izbriši**.
![Izbrišite je aplikacija lozinku](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
8. Da biste potvrdili brisanje klikom na **da**.
![Potvrdite brisanje](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
9. Kada se briše lozinku aplikacije možete kliknite **Zatvori**.
![Zatvorite](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="creating-and-deleting-app-passwords-with-myapps-portal"></a>Stvaranje i brisanje lozinki aplikacije pomoću portala za Myapps.
Ako niste sigurni kako koristiti višestruke provjere autentičnosti, zatim uvijek stvaranje i brisanje lozinki aplikaciju putem portala za myapps.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Da biste stvorili programa lozinku aplikacije pomoću portala za Myapps

1. Prijava na [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Pri vrhu odaberite profil.
3. Odaberite dodatne sigurnosti provjere.
![Oblak](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. To će vas odvesti na stranicu omogućit će vam da biste promijenili postavke.
![Postavljanje](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Pri vrhu, uz dodatne sigurnosti potvrdu, kliknite na **aplikacije lozinki.**
6. Kliknite **Stvori**.
![Stvaranje lozinke za aplikacije](./media/multi-factor-authentication-end-user-app-passwords/create3.png)
7. Unesite naziv aplikacije lozinku, a zatim kliknite **Dalje**.
![Stvaranje lozinke za aplikacije](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
8. Kopirajte lozinku aplikacije u međuspremnik i zalijepiti u aplikaciju.
![Stvaranje lozinke za aplikacije](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Da biste izbrisali je lozinka aplikacije pomoću portala za Myapps

1. Prijava na [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Pri vrhu odaberite profil.
3. Odaberite dodatne sigurnosti provjere.
![Oblak](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. To će vas odvesti na stranicu omogućit će vam da biste promijenili postavke.
![Postavljanje](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Pri vrhu, uz dodatne sigurnosti potvrdu, kliknite na **aplikacije lozinki.**
6. Uz aplikaciju lozinku koju želite izbrisati, kliknite **Izbriši**.
![Izbrišite je aplikacija lozinku](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
7. Da biste potvrdili brisanje klikom na **da**.
![Potvrdite brisanje](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
8. Kada se briše lozinku aplikacije možete kliknite **Zatvori**.
![Zatvorite](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="create-app-passwords-in-the-azure-portal"></a>Stvaranje aplikacije lozinke na portalu za Azure

Ako koristite višestruke provjere autentičnosti u sustavu Azure želite stvoriti aplikaciju lozinke putem portala za Azure.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>Da biste stvorili aplikacije lozinke na portalu za Azure

1. Prijava na portal za upravljanje Azure.
2. U gornjem desnom tipkom miša kliknite korisničko ime, a zatim odaberite dodatna sigurnost provjere.
3. Na stranici proofup na vrhu, odaberite aplikaciju lozinki
4. Kliknite **Stvori**.
5. Unesite naziv aplikacije lozinku, a zatim kliknite **Dalje**
6. Kopirajte lozinku aplikacije u međuspremnik i zalijepiti u aplikaciju.
![Oblak](./media/multi-factor-authentication-end-user-app-passwords-create-azure/app2.png)

### <a name="to-delete-app-passwords-in-the-azure-portal"></a>Da biste izbrisali aplikaciju lozinke na portalu za Azure

1. Prijava na portal za upravljanje Azure.
2. U gornjem desnom tipkom miša kliknite korisničko ime, a zatim odaberite dodatna sigurnost provjere.
3. Pri vrhu, uz dodatne sigurnosti potvrdu, kliknite na **aplikacije lozinki.**
4. Uz aplikaciju lozinku koju želite izbrisati, kliknite **Izbriši**.
5. Da biste potvrdili brisanje klikom na **da**.
6. Kada se briše lozinku aplikacije možete kliknite **Zatvori**.
![Zatvorite](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)
