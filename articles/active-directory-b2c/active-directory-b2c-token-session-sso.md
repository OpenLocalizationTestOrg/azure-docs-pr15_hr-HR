<properties
    pageTitle="Azure Active Directory B2C: Tokena, sesije i jedan konfiguracije za prijavu | Microsoft Azure"
    description="Tokena, sesije i jedan prijave konfiguracija u Azure Active Directory B2C"
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
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Tokena, sesije i jedan konfiguracije za prijavu

Ova značajka omogućuje preciznije kontrole, na [temelj svakog pravila](active-directory-b2c-reference-policies.md)od:
 
1. Vijekom trajanja sigurnosnih tokena čuje po B2C Azure Active Directory (Azure AD).
2. Vijekom trajanja web aplikacije sesija upravlja Azure AD B2C.
3. Jedinstvenu prijavu (SSO) ponašanje preko više aplikacija i pravila u klijentu B2C.

Možete koristiti ovu značajku u klijentu B2C na sljedeći način:

1. Slijedite ove korake da biste [došli do značajke plohu B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portala za Azure.
2. Kliknite **pravila za prijavu**. *Bilješke: pomoću ove značajke na bilo koju vrstu pravila ne samo na* *Pravila za prijavu***.
3. Da biste otvorili pravilo tako da ga kliknete. Na primjer, kliknite **B2C_1_SiIn**.
4. Kliknite **Uređivanje** pri vrhu na plohu.
5. Kliknite **tokena, sesije i jedan config za prijavu**.
6. Izvršite željene promjene. Saznajte više o dostupnih svojstava u sljedećim odjeljcima.
7. Kliknite **u redu**.
8. Kliknite **Spremi** pri vrhu stranice na plohu.

![Snimka zaslona tokena, sesije i jedan config za prijavu](./media/active-directory-b2c-token-session-sso/token-session-sso.png)

## <a name="token-lifetimes-configuration"></a>Konfiguriranje tokena vijekom trajanja

Azure AD B2C podržava [protokol za autorizaciju OAuth 2.0](active-directory-b2c-reference-protocols.md) za omogućivanje sigurnog pristupa zaštićenim resursi. Možete implementirati tu podršku Azure AD B2C emits različite [sigurnosnih tokena](active-directory-b2c-reference-tokens.md). Ovo su svojstva možete koristiti za upravljanje vijekom trajanja od sigurnosnih tokena čuje po Azure AD B2C:

- **Pristup i ID tokena vijekom trajanja (u minutama)**: vijek nošenja token OAuth 2.0 koristiti da biste pristupili zaštićeni resursa. Azure AD B2C problemi samo ID tokena trenutno. Ta vrijednost primjenjivati pristupna tokena kao i kada dodat ćemo podršku za njih.
   - Zadani = 60 minuta.
   - Najmanji (uključujući te dvije vrijednosti) = 5 minuta.
   - Maksimalno (uključujući te dvije vrijednosti) = 1440 minuta.
- **Osvježavanja tokena života (dana)**: Maksimalno vremensko razdoblje prije kojeg token osvježavanja može se koristiti za Nabavite novu access ili token ID-a (i po želji novi osvježavanja token, ako je dodijeljen vaše aplikacije u `offline_access` opseg).
   - Zadani = 14 dana.
   - Najmanji (uključujući te dvije vrijednosti) = 1 dana.
   - Maksimalno (uključujući te dvije vrijednosti) = 90 dana.
- **Osvježavanja token animiranog prozor života (dana)**: nakon sljedećeg vremenskog razdoblja minutama korisnik mora ponovno autentičnost, bez obzira je razdoblje valjanosti najnovija Osvježi token dobivene aplikacija. To može pružati ako je parametar postavljen na **Bounded**. Ona mora biti veći od ili jednako na **Osvježavanje tokena života (dana)** vrijednost. Ako parametar postavljen **Unbounded**, ne može pronaći određenu vrijednost.
   - Zadani = 90 dana.
   - Najmanji (uključujući te dvije vrijednosti) = 1 dana.
   - Maksimalno (uključujući te dvije vrijednosti) = 365 dana.

To su nekoliko predmeta pomoću kojih možete omogućiti pomoću tih svojstava:

- Dopusti korisnika da ostanete traje u mobilnu aplikaciju beskonačno, pod uvjetom da korisnik aktivan neprestano u aplikaciji. To možete učiniti tako da postavite na **Osvježavanje tokena klizača prozor života (dana)** prijeđite **Unbounded** pravila za prijavu.
- Nađite vašu gospodarsku granu sigurnost i zahtjeve za usklađenosti postavljanjem vijekom trajanja tokena odgovarajući pristup.

## <a name="session-configuration"></a>Konfiguriranje sesiju

Azure AD B2C podržava [protokol za povezivanje OpenID provjere autentičnosti](active-directory-b2c-reference-oidc.md) za omogućivanje sigurna prijava na web-aplikacije. Ovo su svojstva možete koristiti za upravljanje sesije web aplikacije:

- **Web-aplikacije sesiju života (u minutama)**: vijek Azure AD B2C sesiju kolačića pohranjene na korisnikovu pregledniku nakon uspješne provjere autentičnosti.
   - Zadani = 1440 minuta.
   - Najmanji (uključujući te dvije vrijednosti) = 15 minuta.
   - Maksimalno (uključujući te dvije vrijednosti) = 1440 minuta.
- **Vremensko ograničenje web app**: Ako taj parametar postavljen **apsolutnih**, korisnik mora ponovno autentičnost nakon vremenskog razdoblja naveden minutama **Web app sesiju života (u minutama)** . Ako je taj parametar postavljen na **Rolling** (Zadana postavka), korisnik ostaje prijavljeni pod uvjetom da je korisnik neprestano aktivan u web-aplikacije.

To su nekoliko predmeta pomoću kojih možete omogućiti pomoću tih svojstava:

- Zadovoljava vaše industrijskih sigurnost i usklađenost preduvjete postavljanjem sesiju odgovarajuće web aplikacije vijekom trajanja.
- Pokrenuti ponovnu provjeru autentičnosti nakon određeno vremensko razdoblje tijekom korisničke interakciju s visoke sigurnosti dio web-aplikacije. 

## <a name="single-sign-on-sso-configuration"></a>Konfiguracija za jedinstvenu prijavu (SSO)

Ako imate više aplikacija i pravila u klijentu B2C, možete upravljati korisničke interakcije preko njih pomoću svojstvo **jedan konfiguracije za prijavu** . Postavite svojstvo na jedan od sljedećih postavki:

- **Klijent**: to je zadana postavka. Ta postavka omogućuje više aplikacija i pravila u klijentu B2C da biste zajednički koristili istu sesiju korisnika. Na primjer, kada korisnik potpiše u aplikaciju, košarice Contoso, ali ih potom možete jednostavno i prijavite se na drugi jednu Contoso Pharmacy, nakon pristupa.
- **Aplikacija**: Time Vođenje sesije korisnik isključivo za aplikaciju, neovisno o drugim aplikacijama. Ako, na primjer, ako želite da korisnik za prijavu na Contoso Pharmacy (s istim vjerodajnicama), čak i ako se korisnik već prijavljeni u košarice Contoso, neku drugu aplikaciju na istom B2C klijenta. 
- **Pravila**: Time Vođenje sesije korisnik isključivo za pravilo, neovisno o aplikacije koje koriste ga. Na primjer, ako se korisnik već prijavljeni i dovršen korak višestruki faktor provjere autentičnosti (MFA), on može biti zadano pristup veća sigurnost dijelove više aplikacija sve dok ne istek sesije uz pravila.
- **Onemogućeni**: to Prisiljava korisnika da biste pokrenuli kroz putovanja cijelu korisnika za svaki izvođenja pravila. Na primjer, to će omogućiti više korisnika se prijaviti u aplikaciju (u zajedničkoj radnoj površini scenarij), čak i tijekom jednom korisniku ostaje prijavljeni tijekom cijele.
