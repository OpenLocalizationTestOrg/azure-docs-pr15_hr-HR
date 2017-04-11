<properties
    pageTitle="Zaštita oblaka resursa s Azure MFA i AD FS"
    description="To je stranica za provjeru autentičnosti Azure multi-factor koji opisuje način za početak rada s Azure MFA i AD FS u oblaku."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Zaštita oblaka resursi Azure višestruke provjere autentičnosti i AD FS

Ako je vaša organizacija pridružene s Azure Active Directory, koristite Azure višestruke provjere autentičnosti ili Active Directory Federation Services radi zaštite resursa koje je moguće pristupiti putem Azure AD. Pomoću sljedećih postupaka zaštita resursa Azure Active Directory multi-factor Authentication Azure ili Active Directory Federation Services.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Zaštita resursa Azure AD pomoću servisa AD FS

Radi zaštite vaše oblaka resursa, najprije omogućiti račun za korisnike, a zatim postavljanje zahtjevima pravila. Slijedite ovaj postupak da biste prošli kroz korake:

1. Slijedite korake navedene u [Turn-on višestruke provjere autentičnosti za korisnike](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users) da biste omogućili račun.
2. Pokrenite konzolu za upravljanje AD FS.
![Oblak](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)
3. Dođite do **Potrebe za oslanjanjem smatra pouzdanima strana** , a zatim desnom tipkom miša kliknite na potrebe za oslanjanjem strana pouzdanim. Odaberite **Uređivanje zahtjeva pravila...**
4. Kliknite **Dodaj pravilo...**
5. S padajućeg popisa odaberite **Pošalji zahtjevima pomoću prilagođenih pravila** , a zatim kliknite **Dalje**.
6. Unesite naziv pravila zahtjev.
7. U odjeljku Prilagođeno pravilo: dodajte sljedeći tekst:

    ```
    => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");
    ```

    Odgovarajući zahtjev:

    ```
    <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
    <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
    </saml:Attribute>
    ```

8. Kliknite **u redu** , a zatim **Završi**. Zatvorite konzolu za upravljanje AD FS.

Korisnici zatim dovrše prijave pomoću metode lokalnog (kao što su pametne kartice).

## <a name="trusted-ips-for-federated-users"></a>Pouzdani IP-ovi za vanjske korisnike
Pouzdani IP-ovi administratorima omogućuje potvrdu by-pass dva koraka za određene IP adresa ili vanjski korisnici koji imaju zahtjevi za potječu unutar vlastite intranetu. U sljedećim odjeljcima opisuju kako konfigurirati Azure višestruke provjere autentičnosti pouzdana IP-ovi s vanjskim korisnicima i by-pass dva koraka potvrdu kada zahtjev potječe unutar vanjski korisnici intraneta. To se postiže konfiguriranjem AD FS da biste koristili u prolazni ili filtriranje dolaznih zahtjeva predloška s vrstom zahtjeva u korporacijskom mrežom.

U ovom se primjeru koristi Office 365 za naše potrebe za oslanjanjem strana smatra pouzdanima.

### <a name="configure-the-ad-fs-claims-rules"></a>Konfiguriranje pravila zahtjevima za AD FS

Najprije ćemo potrebno je da biste konfigurirali zahtjevima za AD FS. Ne možemo će stvoriti pravila dva zahtjevima, jedan za Vrsta zahtjeva u korporacijskom mrežom i dodatne jedan čuvanja naših korisnika prijavljeni.

1. Otvorite Upravljanje AD FS.
2. Na lijevoj strani odaberite **Potrebe za oslanjanjem strana smatra pouzdanima**.
3. Desnom tipkom miša kliknite na **Platformi identiteta za Microsoft Office 365** , a zatim odaberite **Uređivanje zahtjeva pravila...** 
 ![Oblaka](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Na izdavanja transformacija pravila kliknite **Dodaj pravilo.** 
 ![Oblaka](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. Dodavanje pretvorbe zahtjeva pravila čarobnjaka, s padajućeg popisa odaberite **proći kroz ili filtriranje dolaznih zahtjeva** , a zatim kliknite **Dalje**.
![Oblak](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. U okvir pokraj naziva za pravilo zahtjeva i naziv pravila. Na primjer: InsideCorpNet.
7. S padajućeg popisa pokraj ulazne Vrsta zahtjeva, odaberite **U korporacijskom mrežom**.
![Oblak](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Kliknite **Završi**.
9. Na izdavanja transformacija pravila, kliknite **Dodaj pravilo**.
10. Dodavanje pretvorbe zahtjeva pravila čarobnjaka, s padajućeg popisa odaberite **pomoću pravila prilagođena zahtjevima za slanje** , a zatim kliknite **Dalje**.
11. U okviru u odjeljku naziv pravila zahtjeva: unesite *Zadržati korisnici prijave*.
12. U dijaloškom okviru Prilagođeno pravilo unesite:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Oblak](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Kliknite **Završi**.
14. Kliknite **Primijeni**.
15. Kliknite **u redu**.
16. Zatvorite upravljanje AD FS.



### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Konfiguriranje Azure višestruke provjere autentičnosti pouzdana IP-ovi s vanjskim korisnicima
Sad kad na zahtjevima će se prikazati, ne možemo možete konfigurirati pouzdanih IP-ovi.

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com).
2. Na lijevoj strani kliknite **Servisa Active Directory**.
3. U odjeljku direktorija, odaberite imenik na mjesto na koje želite postaviti pouzdanih IP-ovi.
4. Na imenik koje ste odabrali, kliknite **Konfiguriraj**.
5. U odjeljku višestruka provjera autentičnosti kliknite **Postavke servisa za upravljanje**.
6. Na stranici Postavke servisa u odjeljku pouzdanih IP-ovi, odaberite **preskočiti višestruka-provjera autentičnosti-zahtjeva za vanjske korisnike na moje intranetu.** 
 ![Oblaka](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Kliknite **Spremi**.
8. Kad su primijenjeni ažuriranja, kliknite **Zatvori**.


To je to! Sada vanjske korisnike sustava Office 365 treba samo morate koristiti MFA kada zahtjev potječe izvan tvrtke intraneta.
