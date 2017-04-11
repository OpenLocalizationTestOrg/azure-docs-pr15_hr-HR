<properties
    pageTitle="Aplikacija Microsoft autentikatora najčešća Pitanja"
    description="Daje popis najčešća pitanja i odgovora vezane uz aplikacije Microsoft Authentication i Azure višestruke provjere autentičnosti."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar, librown"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="microsoft-authenticator-application-faq"></a>Najčešća pitanja vezana uz Microsoft Authenticator aplikacija

Aplikacija Microsoft Authenticator mijenja aplikaciju Azure autentikatora i aplikaciju preporučene kada koristite Azure višestruke provjere autentičnosti. Aplikacija nije dostupna za Windows Phone, Android i iOS.

## <a name="frequently-asked-questions"></a>Najčešća pitanja

- **Što se dogodilo s autentikatora Azure, provjera višestruke provjere autentičnosti i aplikacije za Microsoftov račun?**

    Aplikacija Microsoft Authenticator zamjenjuje međusobno te aplikacije. Azure autentikatora nadograditi Microsoft Authenticator. Ako koristite provjeru višestruke provjere autentičnosti ili Microsoftov račun aplikacije, instalirajte Microsoft Authenticator i ponovno dodajte svoje račune. Provjerite je li na tabulator dodajte svoje račune za nove aplikacije prije brisanja starih.

- **Koristim već aplikacije Microsoft Authenticator kodove za potvrdu. Kako se prebaciti jednim klikom automatske obavijesti?**  

    Odobravanje na prijavu pomoću automatske obavijesti dostupna je samo za Microsoft račune, a ne za proizvođača računa kao što su Google ili Facebook. Microsoftova računa tvrtke ili obrazovne ustanove vašoj tvrtki ili ustanovi možete odabrati da biste onemogućili tu mogućnost, no.

    Ako koristite Microsoftov račun za osobnog računa, a želite prijeći automatske obavijesti, morate je ponovno dodati račun. Ponovno registrirajte uređaj s vašim računom, i postavite automatske obavijesti.  

    Ako vaš račun nema dva koraka potvrdu omogućena, potražite u članku [o dva koraka potvrdu](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) odluči ako je vam najviše odgovara.  

- **Kada će se moći koristiti jednim klikom automatske obavijesti na iPhone ili iPad?**  

    Ta je značajka u beta do kraja kolovoz, kad postane dostupnima za Microsoftova računa. Ako se želite uključiti u naš program beta, slanje e-pošte da biste msauthenticator@microsoft.com. Vaše ime, prezime i Apple ID obuhvatiti poruku.  

- **Jednim klikom automatske obavijesti funkcioniraju za račune proizvođača?**  

    Ne, slanje obavijesti funkcioniraju samo Microsoft računi i računi servisa Azure Active Directory. Ako vaše tvrtke ili obrazovne ustanove koristi Azure AD račune, mogu onemogućiti tu značajku.  

- **Vraćena uređaju iz sigurnosne kopije, a moj račun kodovi nedostaju ili ne funkcionira. Šta se dogodilo?**  

    Iz sigurnosnih razloga ne možemo vraćanje računa iz sigurnosnih kopija aplikacije. Ako aplikaciju iOS vraćanje iz sigurnosne kopije, računima i dalje se prikazuju, ali ne funkcioniraju za primanje verifications za prijavu i generiranje šifre sigurnost. Nakon vratili aplikaciju, izbrišite svoje račune i ponovno dodajte.

- **Dobio sam na novi uređaj. Kako ukloniti aplikaciju Microsoft Authenticator iz stare uređaj i prijelaz na novu?**

    Dodavanje aplikacije Microsoft Authenticator na novi uređaj ne automatski ga ukloniti s drugih uređaja. Da biste upravljali uređaja koji je konfiguriran za vaš račun, posjetite web-mjesto isti koje omogućuju upravljanje potvrdu dva koraka pa odaberite da biste uklonili stare aplikacije.

    Osobni računi za Microsoft ovo web-mjesto je stranici [Sigurnost računa](https://account.microsoft.com/security) . Microsoftova računa tvrtke ili obrazovne ustanove ovo web-mjesto možda je [MyApps](https://myapps.microsoft.com) ili prilagođeni portal za vaša tvrtka ili ustanova postavila.

## <a name="contact-us"></a>Obratite nam se

Ako je ovo nije pronašli odgovor na pitanje, ostavite komentar pri dnu stranice. Ili, [obratite se podršci](https://support.microsoft.com/contactus) i smo ćete odgovorite na problem čim možemo.

Ako se obratite podršci, obuhvaćaju koliko je sljedeće informacije kao:

- **Korisnički ID** – što je adresa e-pošte koju ste pokušali prijavljujete?
- **Opći opis pogreške** – što točno poruka o pogrešci niste li možda vidjeti?  Ako je ne prikazuje se poruka o pogrešci, opisuju neočekivano ponašanje primijetili detaljno.
- **Stranica** – koju stranicu jeste li na kada se pojavi pogreška (Navedite URL)?
- **KodPogreške** - specifična Šifra pogreške primate.
- **ID sesije** - id određene sesije primate.
- **ID korelacije** – što je kod id korelacije koji su generirani prilikom korisnika prikazivalo pogrešku.
- **Vremenska oznaka** – što je precizno datum i vrijeme vidjeli pogreške (uključuje se vremenska zona)?

Većinu prednosti ove informacije možete pronaći na stranici za prijavu. Kada se ne Provjerite svoje prijave u vremenu, odaberite **Prikaz detalja**.

![Prijavite se u detalje o pogrešci](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Uključivanju sljedećih informacija pridonose da biste najbrže što može riješiti problem.

## <a name="related-topics"></a>Povezane teme

- [Najčešća pitanja vezana uz Azure višestruka provjera autentičnosti](multi-factor-authentication-faq.md)  
- [O provjeri dva koraka](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) za Microsoftovi računi
- [Imate li poteškoća s dva koraka potvrdu?](multi-factor-authentication-end-user-troubleshoot.md)
