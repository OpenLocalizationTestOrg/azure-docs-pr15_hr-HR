<properties
    pageTitle="Azure Active Directory B2C: Alat za Pomoćnik za prilagodbu korisničkog Sučelja stranice | Microsoft Azure"
    description="Pomoćnik za alat koji se koristi kao dokaz značajke prilagodbe korisničkog Sučelja web-stranice u Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: Pomoćnik za alat koji se koristi kao dokaz prilagodbe značajka stranice korisničko sučelje (UI)

Ovaj se članak odnosi pomoćnom [glavni članak za prilagodbu korisničkog Sučelja](active-directory-b2c-reference-ui-customization.md) u B2C Azure Active Directory (Azure AD). Sljedeći koraci opisuju kako postići značajku za prilagodbu korisničkog Sučelja stranice korištenjem HTML i CSS sadržaj uzorka koji smo ste naveli.

## <a name="get-an-azure-ad-b2c-tenant"></a>Početak klijent za Azure AD B2C

Prije nego što možete prilagoditi ništa, morat ćete [dobiti klijent za Azure AD B2C](active-directory-b2c-get-started.md) ako ga već nemate.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Stvaranje pravila registracije ili prijave

Ogledni sadržaj ne možemo ste naveli može se koristiti za slika dvije stranice u [registracije ili prijave pravila](active-directory-b2c-reference-policies.md): [sjedinjena stranica za prijavu u](active-directory-b2c-reference-ui-customization.md) i [koja se sama asserted atribute stranice](active-directory-b2c-reference-ui-customization.md). Prilikom [stvaranja pravilnika registracije ili prijave](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), dodajte lokalni račun (adresu e-pošte), Facebook, Google i Microsoft kao **davatelji identiteta**. To su samo IDPs koje će prihvatiti naš ogledni HTML sadržaj.  Možete dodati i podskup te IDPs ako želite.

## <a name="register-an-application"></a>Registracija aplikacije

Morat ćete [Registracija aplikacije](active-directory-b2c-app-registration.md) u klijentu B2C koje je moguće koristiti za izvršavanje pravilnika. Nakon registracije aplikacije, imate nekoliko mogućnosti koje možete koristiti zapravo pokretanje Pravilnik za prijavu:

- Sastavljanje nešto AD B2C Azure Brzi start aplikacije navedene u odjeljku "Započnite rad" [prijavite gore i prijavite se u koje korisnici u aplikacija](active-directory-b2c-overview.md#getting-started).
- Pomoću aplikacije gotove [Azure AD B2C Playground](https://aadb2cplayground.azurewebsites.net) . Ako odlučite koristiti u playground, morate se registrirati aplikacije u klijentu B2C pomoću **preusmjeravanje URI** `https://aadb2cplayground.azurewebsites.net/`.
- Pomoću gumba **Izvedi sada** na pravilnika za [Azure portal](https://portal.azure.com/).

## <a name="customize-your-policy"></a>Prilagodba pravilnika

Da biste prilagodili izgled i dojam pravilnika, morate najprije stvoriti HTML i CSS datoteka pomoću određenih konvencije za Azure AD B2C. Zatim možete prenijeti statički sadržaj javno dostupna mjesto tako da Azure AD B2C mogu pristupiti. To se može biti namjenski web-poslužitelj, spremište blobova platforme Azure, Azure mreže za isporuku sadržaja ili bilo kojeg drugog statične resursa hosting davatelja usluga. Preduvjeti za samo su sadržaj je dostupan putem HTTP, a možete pristupiti pomoću CORS. Nakon što ste izložen statički sadržaj na webu, možete urediti pravilo da biste na tom mjestu i izlaganje sadržaja klijentima. [Glavni članak prilagođavanje UI](active-directory-b2c-reference-ui-customization.md) opisuju detaljno funkcioniranje značajke prilagodbe Azure AD B2C.

Za potrebe ovog praktičnog vodiča, ne možemo ste već stvoreni neki sadržaj uzorak i hostirane na spremište blobova platforme Azure. Ogledni sadržaj je vrlo osnovni prilagodbe u temi naše fictional tvrtke ili ustanove "Wingtip igračke". Želite isprobati vlastite pravila, slijedite ove korake:

1. Prijavite se u klijent za [portal za Azure](https://portal.azure.com/) , a zatim otvorite značajke plohu B2C.
2. Kliknite **registracije ili prijave pravila** , a zatim kliknite pravilo (na primjer, "b2c\_1\_znak\_prema gore\_znak\_u").
3. Kliknite **prilagodbu korisničkog Sučelja stranice** , a zatim **Unified registracije ili prijave stranice**.
4. Uključivanje ili isključivanje parametar **Koristi prilagođenu stranicu** na **da**. U polje **prilagođene stranice URI** unesite `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Kliknite **u redu**.
5. Kliknite **stranicu za prijavu lokalni račun**. Uključivanje ili isključivanje parametar **koristi prilagođeni predložak** na **da**. U polje **prilagođene stranice URI** unesite `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
5. Isti korak ponovite za **društvene račun stranicu za prijavu**.
 Kliknite **u redu** dvaput da biste zatvorili blades za prilagodbu korisničkog Sučelja.
6. Kliknite **Spremi**.

Sada možete isprobavanje prilagođenog pravilnika. Koristite vlastitu aplikacije ili playground Azure AD B2C ako želite, ali možete i jednostavno kliknuti naredbu **Izvedi sada** u plohu pravila. U padajućem okviru odaberite aplikaciju i odaberite odgovarajuće preusmjeravanje URI. Kliknite gumb **Pokreni odmah** . Otvorit će se na novoj kartici preglednika i pokrenete kroz korisničko sučelje iz Registracija aplikacije s novim sadržajem na mjestu!

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>Prijenos ogledni sadržaj u spremište blobova platforme Azure

Ako želite da biste koristili spremište blobova platforme Azure za hostiranje sadržaj stranice, možete stvoriti račun za pohranu i prenesite datoteke pomoću alata za Pomoćnik za naše B2C.

### <a name="create-a-storage-account"></a>Stvaranje računa za pohranu

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Kliknite **+ Novo** > **podataka + prostor za pohranu** > **računa za pohranu**. Trebat će vam Azure pretplate da biste stvorili račun spremište blobova platforme Azure. Možete se prijaviti besplatnu probnu verziju na sustava [Azure web-mjesta](https://azure.microsoft.com/pricing/free-trial/).
3. Navedite **naziv** računa za pohranu (na primjer, "contoso"), a zatim odaberite odgovarajuće odabire za **određivanje cijena sloju**, **grupa resursa** i **pretplate**. Provjerite imate li mogućnost **Prikvači na Startboard** potvrđen. Kliknite **Stvori**.
4. Vratite se na Startboard i kliknite račun za pohranu koji ste upravo stvorili.
5. U odjeljku **Sažetak** kliknite **spremnika**, a zatim kliknite **+ Dodaj**.
6. Navedite **naziv** za spremnik (na primjer, "b2c"), a zatim odaberite **Blob** kao **Vrsta pristupa**. Kliknite **u redu**.
7. Spremnik koji ste stvorili pojavit će se na popisu plohu **blob-ova** . Zapišite URL spremnika; na primjer, ona bi trebala izgledati ovako za `https://contoso.blob.core.windows.net/b2c`. Zatvorite plohu **blob-ova** .
8. Na račun plohu prostora za pohranu kliknite **tipke** , a zatim zapišite vrijednosti polja **Naziv računa za pohranu** i **Primarni ključ za Access** .

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Kliknite **+ Novo** > **podataka + prostor za pohranu** > **računa za pohranu**. Trebat će vam Azure pretplate da biste stvorili račun spremište blobova platforme Azure. Možete se prijaviti besplatnu probnu verziju na sustava [Azure web-mjesta](https://azure.microsoft.com/pricing/free-trial/).
3. Odaberite **Blobova** u odjeljku **Vrsta računa**, a ostavite druge vrijednosti kao zadano.  Ako želite da se možete urediti grupa resursa i mjesto.  Kliknite **Stvori**.
4. Vratite se na Startboard i kliknite račun za pohranu koji ste upravo stvorili.
5. U odjeljku **Sažetak** kliknite **+ spremnik**.
6. Navedite **naziv** za spremnik (na primjer, "b2c"), a zatim odaberite **Blob** kao **Vrsta pristupa**. Kliknite **u redu**.
7. Otvorite spremnik **Svojstva**, a zatim zapišite URL spremnika; na primjer, ona bi trebala izgledati ovako za `https://contoso.blob.core.windows.net/b2c`. Zatvorite plohu kontejner.
8. Na račun plohu prostora za pohranu kliknite **Ikonu ključa** , a zatim zapišite vrijednosti polja **Naziv računa za pohranu** i **Primarni ključ za Access** .

> [AZURE.NOTE]
    **Primarni ključ Access** je vjerodajnicu za zaštitu.

### <a name="download-the-helper-tool-and-sample-files"></a>Preuzimanje preglednika alata i ogledne datoteke

Možete preuzeti [spremište blobova platforme Azure pomoćne alat i ogledne datoteke kao .zip datoteku](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) ili Kloniraj iz GitHub:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Sadrži ovo spremište na `sample_templates\wingtip` direktorija koji sadrži primjer HTML, CSS i slike. Za te predloške referentni spremište blobova platforme Azure račun, morat ćete HTML datoteke. Otvaranje `unified.html` i `selfasserted.html` i Zamijeni sve instance `https://localhost` URL-om vlastite spremniku koji zapisali u prethodnim koracima. Morate koristiti apsolutni put HTML datoteka jer je u tom slučaju posluživanje HTML po Azure AD, u odjeljku domene `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>Prijenos ogledne datoteke

U istom spremištu raspakiraj `B2CAzureStorageClient.zip` i pokretanje u `B2CAzureStorageClient.exe` datoteka unutar. Ovaj program jednostavno će prijenos svih datoteka u direktoriju koji ste naveli za svoj račun za pohranu i omogućivanje pristupa CORS tim datotekama. Ako ste pratili navedene korake, HTML i CSS datoteka sada pokažete na račun servisa za pohranu. Imajte na umu da je naziv vašeg računa za pohranu dio koji prethodi `blob.core.windows.net`; na primjer, `contoso`. Možete provjeriti da sadržaj prenesen pravilno tako da pristupa `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` na pregledniku. Da biste provjerili je li sadržaj sada CORS omogućeno koristiti [http://test-cors.org/](http://test-cors.org/) . (Potražite "XHR status: 200" u rezultatu.)

### <a name="customize-your-policy-again"></a>Prilagodba pravilnika, ponovno

Sad kad ste prenijeli oglednog sadržaja na račun za pohranu, potrebno je urediti registracije pravilnika referencu. Ponovite korake iz odjeljka ["Prilagodi pravilnika"](#customize-your-policy) iznad, pomoću URL-ovi vlastiti račun za pohranu. Ako, primjerice, mjesto vašeg `unified.html` datoteke bio `<url-of-your-container>/wingtip/unified.html`.

Sada možete koristiti gumb **Izvedi sada** ili vlastite aplikacije za ponovno izvođenje pravilnika. Rezultat trebao bi izgledati točno gotovo isti – koje koriste isti uzorak HTML i CSS-a u oba slučaja. Međutim, police sada pozivate vlastite pojavljivanja spremište blobova platforme Azure i ste besplatno da biste uredili i ponovno prenijeti datoteke ponovno kao. Dodatne informacije o prilagođavanju HTML i CSS-a odnose se na [glavni članak za prilagodbu korisničkog Sučelja](active-directory-b2c-reference-ui-customization.md).
