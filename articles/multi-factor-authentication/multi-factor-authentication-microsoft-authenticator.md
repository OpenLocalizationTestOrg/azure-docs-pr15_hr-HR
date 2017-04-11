<properties
    pageTitle="Aplikacija Microsoft Authenticator za mobilne telefone | Microsoft Azure"
    description="Saznajte kako nadograditi na najnoviju verziju programa Azure autentikatora."
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
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="microsoft-authenticator"></a>Microsoft autentikatora

Aplikaciju Microsoft Authenticator nudi dodatnu razinu sigurnosti u račun za Azure (, na primjer, bsimon@contoso.onmicrosoft.com), lokalnih račun tvrtke (Ako, na primjer, bsimon@contoso.com), ili Microsoftov račun (Ako, na primjer, bsimon@outlook.com).

Aplikaciju funkcionira na jedan od dva načina:

- **Obavijesti**. Aplikaciju može pridonijeti spriječili neovlašteni pristup s računima i zaustavljanje lažno transakcije pritiskom obavijest Pametni telefon ili tablet. Samo prikaz obavijesti i ako je valjane, odaberite **Potvrdi**. U suprotnom, možete odabrati **Uskrati**. Informacije o odbijanju obavijesti potražite u članku korištenje Uskrati i značajka prijevare izvješća za višestruke provjere autentičnosti.

- **Lozinka s kod za potvrdu**. Aplikacija se može koristiti kao token softver za stvaranje programa OAuth kod za potvrdu. Unesite kod nudi aplikaciju u na zaslon za prijavu u, uz korisničko ime i lozinku, kada se to od vas zatraži. Kod za potvrdu nudi drugi obrazac provjere autentičnosti.

Uz izdanje aplikaciju Microsoft Authenticator staru aplikaciju Azure autentikatora je zamijenjena.  Aplikaciju Azure autentikatora nastavit će funkcionirati, no ako odlučite da biste premjestili nove aplikacije Microsoft Authenticator, u ovom se članku ne mogu vam pomoći.  

## <a name="install-the-app"></a>Instalirajte aplikaciju

Aplikacija Microsoft Authenticator je dostupne za [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="add-accounts-to-the-app"></a>Dodavanje računa u aplikaciju

Za svaki račun koji želite dodati aplikaciju Microsoft Authenticator, koristite neku od sljedećih postupaka.

### <a name="add-an-account-to-the-app-by-using-the-qr-code-scanner"></a>Dodavanje računa u aplikaciju pomoću skenera QR kod

1. Idite na zaslon s postavkama provjere sigurnost.  Informacije o tome kako prenijeti na zaslonu potražite u članku [Promjena postavki sigurnosti](multi-factor-authentication-end-user-manage-settings.md).

2. Odaberite **Konfiguriranje**.

    ![Gumb konfiguracija na zaslonu postavke provjere valjanosti za sigurnost](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Tako ćete prikazati na zaslon s QR kod na njemu.

    ![Zaslon na kojem se nudi QR kod](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Otvorite aplikaciju Microsoft autentikatora. Na zaslonu **računi** odaberite **+**, a zatim navedite želite li dodati račun tvrtke ili obrazovne ustanove.

    ![Zaslon računi znak plus](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Zaslon za određivanje račun tvrtke ili obrazovne ustanove](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. Skenirajte QR kod pomoću kamera, a zatim odaberite **gotovo** da biste zatvorili zaslon za QR kod.

    ![Zaslon za pregledavanje QR kod](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

    Ako fotoaparata ne radi ispravno, možete unijeti QR kod i URL ručno. Dodatne informacije potražite u članku [Ručno dodavanje računa u aplikaciju](#add-an-account-to-the-app-manually).

5. Pričekajte dok je aktivirana račun. Kada se dovrši aktivaciju, odaberite **Kontakt**.  To će poslati obavijest ili kod za potvrdu na telefon.  Odaberite **provjeriti**.

    ![Zaslon gdje odaberite potvrdi da biste se prijavili](./media/multi-factor-authentication-end-user-first-time-mobile-app/verify.png)

6. Ako vaša tvrtka potreban PIN-a za odobravanje potvrdu za prijavu, unesite je.

    ![Okvir za unos PIN-a](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

7. Nakon dovršetka Dužina PIN-a, odaberite **Zatvori**. U ovom trenutku provjeravanje mora biti uspješno.
8. Preporučujemo da u slučaju izgubiti pristup aplikacije unesite broj mobilnog telefona. Odredite vašu državu s padajućeg popisa, a zatim unesite broj mobilnog telefona u okvir pokraj naziva države. Odaberite **Dalje**.
9. Sada ste postavili način kontakta. Sada je vrijeme da biste postavili lozinke aplikacije za aplikacije koje nisu preglednik, kao što su Outlook 2010 ili stariji. Ako ne koristite ove aplikacije, odaberite **gotovo**. U suprotnom, prijeđite na sljedeći korak.

    ![Zaslon za stvaranje lozinke za aplikaciju](./media/multi-factor-authentication-end-user-first-time-mobile-app/step4.png)

10. Ako koristite aplikacije koje nisu pregledniku, kopirajte aplikaciju ponuđena lozinku i zalijepite lozinku u aplikacijama. Upute za pojedinačne aplikacije kao što su Outlook i Lync potražite u članku kako promijeniti lozinku u e-poštu aplikacije lozinku i kako promijeniti lozinku u aplikaciji lozinki aplikacije.
11. Odaberite **gotovo**.

Sada trebali biste vidjeti novog računa na zaslonu **računi** .

![Računi zaslona](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Ručno dodavanje računa u aplikaciju

1. Idite na zaslon s postavkama provjere sigurnost.  Informacije o tome kako prenijeti na zaslonu potražite u članku [Promjena postavki sigurnosti](multi-factor-authentication-end-user-manage-settings.md).

2. Odaberite **Konfiguriranje**.

    ![Gumb konfiguracija na zaslonu postavke provjere valjanosti za sigurnost](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Tako ćete prikazati na zaslon s QR kod na njemu.  Imajte na umu kod i URL.

    ![Zaslon na kojem se nudi QR kod i URL-a](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Otvorite aplikaciju Microsoft autentikatora. Na zaslonu **računi** odaberite **+**, a zatim navedite želite li dodati račun tvrtke ili obrazovne ustanove.

    ![Zaslon računi znak plus](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Zaslon za određivanje račun tvrtke ili obrazovne ustanove](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. U skener, odaberite **Ručni unos koda**.

    ![Zaslon za pregledavanje QR kod](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

5. Unesite šifru i URL-a u odgovarajuće okvire u aplikaciji.

    ![Zaslon za unos koda i URL-a](./media/multi-factor-authentication-azure-authenticator/manual.png)

    ![Zaslon za unos koda i URL-a](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

6. Pričekajte dok je aktivirana račun. Kada se dovrši aktivaciju, odaberite **Kontakt**. To će poslati obavijest ili kod za potvrdu na telefon. Odaberite **provjeriti**.

Sada trebali biste vidjeti novog računa na zaslonu **računi** .

![Računi zaslona](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-by-using-touch-id"></a>Dodavanje računa u aplikaciju pomoću dodira ID

Aplikacija Microsoft Authenticator u sustavu iOS podržava dodirom ID-a.  Azure višestruke provjere autentičnosti omogućuje tvrtke i ustanove moraju potreban PIN-a za uređaje. Dodirnite ID korisnika iOS ne morate unijeti PIN. Umjesto toga možete skenirati svoj otisak prsta i odaberite **odobravanje**.

Postavljanje dodira ID-a s Microsoft Authenticator je jednostavno. Dovršite test normalni potvrdu PIN-om. Ako vaš uređaj podržava dodirne ID, Microsoft Authenticator će ga postaviti automatski za taj račun.

![Provjera instalacije dodira ID](./media/multi-factor-authentication-azure-authenticator/touchid1.png)

Od točke naprijed, kada ste potrebno da biste provjerili vaše prijave, odaberite primljene automatske obavijesti i skeniranje vaš otisak prsta umjesto unošenja PIN-a.

![Automatske obavijesti](./media/multi-factor-authentication-azure-authenticator/touchid2.png)

## <a name="uninstall-the-old-azure-authentication-app"></a>Deinstalirajte staru aplikaciju za provjeru autentičnosti Azure

Kada dodate sve račune novu aplikaciju staru aplikaciju možete deinstalirati putem mobitela.

## <a name="delete-an-account"></a>Brisanje računa

Da biste uklonili račun iz aplikacije Microsoft Authenticator, odaberite račun, a zatim odaberite **Izbriši**.

![Gumb Izbriši](./media/multi-factor-authentication-azure-authenticator/remove.png)
