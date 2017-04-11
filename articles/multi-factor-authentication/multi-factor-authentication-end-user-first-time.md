<properties
    pageTitle="Postavljanje potvrdu dva koraka za moj račun tvrtke ili obrazovne ustanove"
    description="Kada tvrtke konfigurira Azure višestruke provjere autentičnosti, zatražit će se registrirati za potvrdu dva koraka. Saznajte kako to postaviti. "
    services="multi-factor-authentication"
    keywords="kako koristiti azure direktorija, servisa active directory u oblaku, vodič servisa active directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="set-up-my-account-for-two-step-verification"></a>Postavljanje računa za potvrdu dva koraka

Provjera dva koraka je korak dodatne sigurnosti koji olakšava zaštititi svoj račun tako da teže drugim osobama da biste prekinuli u. Ako čitate ovog članka, poruke e-pošte vjerojatno dobivenog od administratoru tvrtke ili obrazovne ustanove o višestruke provjere autentičnosti. Ili možda prijave i primio poruku da biste postavili dodatne sigurnosti provjere. Ako je predmet, **ne možete se prijaviti dok ne dovršite postupak automatskog registraciju**.

Ovaj članak sadrži postavljanje **ili školske računa**. Ako želite da biste omogućili potvrdu dva koraka za vlastitih, osobni Microsoftov račun, pročitajte članak [o potvrdu dva koraka](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="determine-how-you-will-use-multi-factor-authentication"></a>Odredite kako će koristiti višestruke provjere autentičnosti

Provjera dva koraka radi tako da upitom za dva komada identifikacijski prilikom prijave. Najprije ćemo zatražite od korisničkog imena i lozinke kao i obično. Nakon toga potrebna kontakta telefonski bismo znali pripada vama i koje potvrda da je pokušaj prijave valjane.  

Da biste započeli postupak instalacije, pokušajte se prijaviti na račun servisa kao što to obično činite. Ako je administrator konfigurirao vaš račun za potvrdu dva koraka, zatražit će se da biste započeli postupak automatskog registraciju. Započnite postupak tako da kliknete **je sada postavili.**

![Postavljanje](./media/multi-factor-authentication-end-user-first-time/first.png)

Prvo pitanje tijekom postupka Registracija je način stupanje u kontakt s vama. Pogledajte mogućnosti u tablici, a pomoću veze da biste prešli na korake za postavljanje za svaki način.

| Vrsta kontakta | Opis |
| --- | --- |
[Mobilne aplikacije](#use-a-mobile-app-as-the-contact-method) | - **Primanje obavijesti za potvrdu.** Ta mogućnost ih gura obavijest aplikaciju autentikatora pametnog telefona ili tableta. Prikaz obavijesti i, ako je valjane, odaberite **provjere autentičnosti** u aplikaciji. Tvrtke ili obrazovne ustanove možda ćete morati unijeti PIN prije nego što ste provjeru autentičnosti.<br>- **Koristite kod za potvrdu.** U tom načinu autentikatora app generirat će kod za potvrdu koji ažurira svakih 30 sekundi. U sučelje za prijavu unesite najnovije kod za potvrdu.<br>Aplikacija Microsoft Authenticator je dostupne za [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
[Poziv na mobilni telefon ili teksta](#use-your-mobile-phone-as-the-contact-method) | - **Telefonskog poziva** stavlja poziv automatskog telefonskog unesete telefonskom broju. Odgovorili na poziv, a zatim pritisnite # na tipkovnici telefona za provjeru autentičnosti.<br>- **Tekstne poruke** završava tekstnu poruku koja sadrži kod za potvrdu. Sljedeći upit u tekstu, odgovaranje na poruku teksta ili unesite kod za potvrdu navedene u sučelje za prijavu. |  
[Office telefonski poziv](#use-your-office-phone-as-the-contact-method) | Postavlja programa automatskog telefonskog poziva telefonski broj koji navedete. Odgovorili na poziv i pritisne # u telefonske tipkovnice za provjeru autentičnosti. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Korištenje mobilne aplikacije kao vrstu kontakta

Pomoću ove metode potrebno je instalirati aplikaciju autentikatora na telefonu ili tabletu. Koraci u ovom članku temelje se na aplikaciju Microsoft Authenticator koji je dostupan za [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

1. S padajućeg popisa odaberite **mobilne aplikacije** .
2. Odaberite **primati obavijesti za potvrdu** ili **Korištenje kod za potvrdu**, a zatim odaberite **Postavljanje**.

    ![Zaslon za potvrdu dodatne sigurnosti](./media/multi-factor-authentication-end-user-first-time-mobile-app/mobileapp.png)

3. Na telefonu ili tabletu, otvorite aplikaciju i odaberite **+** da biste dodali račun. (Uređajima sa sustavom Android, odaberite tri točke, zatim **Dodaj račun**.)
4. Navedite želite li dodati račun tvrtke ili obrazovne ustanove. Otvorit će se skener QR kod na telefonu. Ako fotoaparata ne radi ispravno, možete odabrati Ručni unos podataka o tvrtki. Dodatne informacije potražite u članku [Ručno dodavanje računa](#add-an-account-manually).  
5. Skeniranje slike QR kod koji su se pojavili sa zaslona za konfiguriranje aplikacije za mobilne uređaje.  Odaberite **gotovo** da biste zatvorili zaslon za QR kod.  

    ![Zaslon QR kod](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

6. Kada se dovrši aktivaciju na telefonu, odaberite **Kontakt**.  Ovaj korak šalje obavijest ili kod za potvrdu telefonu. Odaberite **provjeriti**.  
7. Ako vaša tvrtka potreban PIN-a za odobravanje potvrdu za prijavu, unesite je.

    ![Okvir za unos PIN-a](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

8. Nakon dovršetka Dužina PIN-a, odaberite **Zatvori**. U ovom trenutku provjeravanje mora biti uspješno.
9. Preporučujemo da unesite broj mobilnog telefona u slučaju izgubiti pristup aplikacije za mobilne uređaje. Odredite vašu državu s padajućeg popisa, a zatim unesite broj mobilnog telefona u okvir pokraj naziva države. Odaberite **Dalje**.
10. Sada se od vas zatraži da biste postavili lozinke aplikacije za aplikacije koje nisu preglednika kao što su Outlook 2010 ili stariji ili aplikaciju nativni e-pošte na uređajima Apple. To je zato neke se aplikacije ne podržavaju potvrdu dva koraka. Ako ne koristite ove aplikacije, kliknite **gotovo** i preskočite ostatak postupka.
11. Ako koristite aplikacije, kopiju lozinke za aplikacije navedene i zalijepiti u aplikaciji umjesto obične lozinku. Možete koristiti istu lozinku za aplikaciju za više aplikacija. Dodatne informacije, [pomoć za aplikacije lozinke].
12. Kliknite **gotovo**.


### <a name="add-an-account-manually"></a>Ručno dodavanje računa
Ako želite da biste dodali račun za mobilne aplikacije ručno, umjesto korištenja QR reader, slijedite ove korake.

1. Odaberite gumb **Ručni unos računa** .  
2. Unesite šifru i URL koji su navedeni su na istoj stranici koja prikazuje crtičnog koda. Informacije prelazi u okvire **kod** i **URL-a** na aplikacije za mobilne uređaje.

    ![Postavljanje](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

3. Po završetku aktivaciju, odaberite **Kontakt**. Ovaj korak šalje obavijest ili kod za potvrdu telefonu. Odaberite **provjeriti**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Pomoću svojeg mobitela kao vrstu kontakta

1. S padajućeg popisa odaberite **Telefona za provjeru autentičnosti** .  

    ![Postavljanje](./media/multi-factor-authentication-end-user-first-time-mobile-phone/phone.png)  

2. S padajućeg popisa odaberite svojoj zemlji pa unesite broj mobilnog telefona.
3. Odaberite način na koji želite koristiti s mobilnog telefona - teksta ili poziv.
4. Odaberite **kontakt me** da biste provjerili telefonski broj. Ovisno o načinu na koji ste odabrali smo na informativnoj traci ili poziva. Slijedite upute na zaslonu, a zatim odaberite **Potvrdi**.
5. Sada se od vas zatraži da biste postavili lozinke aplikacije za aplikacije koje nisu preglednika kao što su Outlook 2010 ili stariji ili aplikaciju nativni e-pošte na uređajima Apple. To je zato neke se aplikacije ne podržavaju potvrdu dva koraka. Ako ne koristite ove aplikacije, kliknite **gotovo** i preskočite ostatak postupka.
6. Ako koristite aplikacije, kopiju lozinke za aplikacije navedene i zalijepiti u aplikaciji umjesto obične lozinku. Možete koristiti istu lozinku za aplikaciju za više aplikacija. Dodatne informacije, [pomoć za aplikacije lozinke].
7. Kliknite **gotovo**.

## <a name="use-your-office-phone-as-the-contact-method"></a>Uz pomoć mobitela office kao vrstu kontakta

1. Odaberite **Office telefon** s padajućeg popisa  

    ![Postavljanje](./media/multi-factor-authentication-end-user-first-time-office-phone/office.png)  

2. Okvir s brojem telefona automatski se ispunjava vaše tvrtke podaci za kontakt. Ako je broj netočne ili ih nema, od administratora zatražite da biste unijeli promjene.
4. Odaberite **kontakt me** da biste provjerili telefonski broj, a ne možemo će se poziva na telefon. Slijedite upute na zaslonu, a zatim odaberite **Potvrdi**.
5. Sada se od vas zatraži da biste postavili lozinke aplikacije za aplikacije koje nisu preglednika kao što su Outlook 2010 ili stariji ili aplikaciju nativni e-pošte na uređajima Apple. To je zato neke se aplikacije ne podržavaju potvrdu dva koraka. Ako ne koristite ove aplikacije, kliknite **gotovo** i preskočite ostatak postupka.
6. Ako koristite aplikacije, kopiju lozinke za aplikacije navedene i zalijepiti u aplikaciji umjesto pravilnim lozinku. Možete koristiti istu lozinku za aplikaciju za više aplikacija. Dodatne informacije potražite u članku [što su lozinke aplikacije](multi-factor-authentication-end-user-app-passwords.md).
7. Kliknite **gotovo**.

## <a name="next-steps"></a>Daljnji koraci

- Promijenite željene mogućnosti i [Upravljanje postavkama za potvrdu dva koraka](multi-factor-authentication-end-user-manage-settings.md)
- Postavljanje [lozinke za aplikaciju](multi-factor-authentication-end-user-app-passwords.md) za aplikacije nativni uređaja koji ne podržavaju potvrdu dva koraka.
- Čak i kada nemate servis cell, pročitajte članak [Microsoft autentikatora aplikacija](multi-factor-authentication-microsoft-authenticator.md) za brzo, sigurnu provjeru autentičnosti.
