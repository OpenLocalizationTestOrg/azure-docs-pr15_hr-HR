<properties
    pageTitle="Azure Active Directory B2C: Radni skalom nasuprot pretpregled B2C klijenata | Microsoft Azure"
    description="Tema o vrstama klijenata za Azure Active Directory B2C"
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
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-production-scale-vs-preview-b2c-tenants"></a>Azure Active Directory B2C: Samoposlužni radnog skalom nasuprot pretpregled B2C

Ako namjeravate pisanje u aplikaciji radnog B2C Azure Active Directory (Azure AD), morate biti sigurni imate li desnom klijent "Vrsta" da biste prešli na uživo. Da biste vidjeli što ste, slijedite ove korake da biste [došli do značajke plohu B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portala za Azure i potražite u odjeljku **Vrsta klijenta**.

## <a name="summary"></a>Sažetak

Azure AD B2C podržava aplikacija radnog samo na **radni skaliranje** B2C klijenata u Sjevernoj Americi.

| Vrsta klijenta | Države/regije | Općenito dostupan? |
| ----------- | -------------- | --------------------- |
| **Proizvodne skaliranje klijenta** | Sjevernoj Americi države/regije | Da |
| **Proizvodne skaliranje klijenta** | Sve države/regije osim Sjeverna Amerika | ne |
| **Pretpregled klijenta** | Sve države/regije | ne |

> [AZURE.NOTE]
Azure AD B2C drugih korisnika (za koje korisnici) su trenutno nije dostupna u nekoliko država ili regija koje su dostupne klijenata za Azure AD (za zaposlenike). Pročitajte sljedeće odjeljke više pojedinosti.

## <a name="production-scale-b2c-tenant-in-north-america"></a>Proizvodne skaliranje B2C klijenta u Sjevernoj Americi

Ako ste u Sjevernoj Americi [stvorili vaš klijent B2C](active-directory-b2c-get-started.md) odnosno u jednom od sljedećih država ili regija: United Državama, Kanadi, Kostarika, Dominikanska Republika, El Salvador, Gvatemala, Meksiko, Panama, Portoriko Trinidad i Tobago, i **Vrsta klijenta** na vaše korisničko Sučelje za administratore B2C piše **radnog mjerilo**, vaš klijent moguće je koristiti za aplikacije radnog.

> [AZURE.NOTE]
Radni skaliranje klijenata su instaliranog skaliranje 100s milijune identiteti korisnika po klijentu.

![Snimka zaslona s proizvodnje skaliranje klijenta](./media/active-directory-b2c-reference-tenant-type/production-scale-b2c-tenant.png)

## <a name="preview-b2c-tenant-in-any-countryregion"></a>Pretpregled B2C klijenta u sve države/regije

Ako ste stvorili B2C klijentu tijekom razdoblja Azure AD B2C pretpregled, vjerojatno je da vaš **klijent upišite** piše **Pretpregled klijenta**. Ako je to slučaj, morate koristiti vaš klijent samo za razvoj i testiranja, a ne i za proizvodnju aplikacije.

> [AZURE.IMPORTANT]
Postoji nema migracije puta iz pretpregleda B2C klijenta na radni skaliranje B2C klijent. Imajte na umu da su tamo Poznati problemi kada izbrišete pretpregled B2C klijenta i ponovno stvaranje radnog skaliranje B2C klijenta s istim nazivom domene. Morate stvoriti klijentom B2C proizvodnje Skaliranje s drugi naziv domene.

![Snimka zaslona s klijentom pretpregled](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)

## <a name="production-scale-b2c-tenant-outside-of-north-america"></a>Proizvodne skaliranje B2C klijentu izvan Sjeverne Amerike

Azure AD B2C trenutno nije načelu dostupan izvan Sjeverne Amerike. Potrebe no možete stvoriti i koristiti klijenata radnog mjerilo za razvoj i testiranje, s na jedan od sljedećih država ili regija: Alžir, Austrija, Azerbajdžan, Bahrein, Bjelarus, Belgija, Bugarska, Hrvatska, Cipar, Češka Republika, Danska, Egipat, Estonija, Finska, Francuska, Njemačka, Grčka, mađarska, Island, Irska, Izrael, Italija, Jordan, Kazahstan, Kenija, Kuvajt, Lativa, Libanon, Lihtenštajn, Lituania, Luksemburg, Makedonija Makedonija, Malta, Crna Gora, Maroko, Nizozemska, Nigerija, Norveška , Oman, Pakistan, poljska, Portugal, Katar, Rumunjska, Rusija, Saudijska Arabija, Srbija, Slovačka, Slovenija, Južnoafrička Republika, Španjolska, Švedska, Švicarska, Tunis, Turska, Ukrajina, Ujedinjeni Arapski Emirati i Velika Britanija.

Kada Azure AD B2C najavljuje Općenito dostupan u iznad država ili regija, možete nastaviti koristiti te radnog skaliranje klijenata i live Idi s aplikacijama radnog bez gubitka podataka.

## <a name="availability-of-b2c-tenants"></a>Raspoloživost B2C klijenata

Samoposlužni B2C su trenutno nije dostupna u sljedeće zemlje ili regije: Afganistan, Argentina, Australija, Brazil, Čile, Kolumbija, Ekvador, Hong Kong SAR, Indija, Indonezija, Irak, Japan, Koreja, Malezija, Novi Zeland, Paragvaj, Peru, Filipini, Singapur, Šri Lanka, Tajvan, thailand (ไทย), Urugvaj i Venecuela. Plan da biste ih dodali u budućnosti.
