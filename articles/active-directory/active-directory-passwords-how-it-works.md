<properties
    pageTitle="Kako to funkcionira: Upravljanje lozinkama Azure AD | Microsoft Azure"
    description="Saznajte više o različite komponente za upravljanje Azure AD lozinku, uključujući gdje na koji korisnici Registriranje, ponovno postavljanje i promjena lozinke, i gdje administratori konfigurirali, izvješće na, a zatim Omogući upravljanje lokalnim servisom Active Directory lozinki."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="how-password-management-works"></a>Funkcioniranje upravljanja lozinkom

> [AZURE.IMPORTANT] **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).

Upravljanje lozinkama u servisu Azure Active Directory sastoji se od nekoliko logičke komponente koje su opisane u nastavku.  Kliknite svaku vezu da biste saznali više o toj komponenti.

- [**Konfiguriranje Portal za upravljanje lozinku**](#password-management-configuration-portal) – administratori mogu kontrolirati različite pozornici od načina upravljanja lozinke u svojim korisnicima tako da odete na karticu konfiguracija njihove direktorija [Azure Portal za upravljanje](https://manage.windowsazure.com).
- [**Portal Registracija korisnika**](#user-registration-portal) – korisnicima možete koja se sama registrirati za vraćanje izvorne lozinke putem portala za web.
- [**Portal za ponovno postavljanje lozinke korisnika**](#user-password-reset-portal) – korisnicima možete ponovno postaviti vlastitu lozinke pomoću nekoliko različitih izazove skladu pravila ponovno postavljanje lozinke administratora kontrolirano
- [**Promjena Portal lozinku za korisnika**](#user-password-change-portal) – korisnici mogu promijeniti vlastite lozinke u bilo kojem trenutku tako da unesete njihove staru lozinku, a zatim odaberete novu lozinku pomoću portala za web
- [**Lozinka upravljanje izvješćima**](#password-management-reports) – administratori možete pogledati i analizirati lozinke vratiti i registracija aktivnosti u svoje klijentu tako da odete do odjeljka "Izvješća o aktivnosti" kartica "Izvješća" njihove direktorija za [Portal za upravljanje Azure](https://manage.windowsazure.com)
- [**Komponenta Upisima lozinku Azure AD Connect**](#password-writeback-component-of-azure-ad-connect) – administratori po želji možete omogućiti značajku Upisima lozinku prilikom instalacije Azure AD Connect da biste omogućili upravljanje vanjskog ili lozinku sinkronizirati korisničkih lozinki iz oblaka.

## <a name="password-management-configuration-portal"></a>Konfiguriranje Portal za upravljanje lozinke
Možete konfigurirati pravila upravljanja lozinku za određeni direktorij pomoću [Portala za upravljanje Azure](https://manage.windowsazure.com) tako da odete u odjeljku **Pravila za ponovno postavljanje lozinke korisničkog** u imenik **Konfiguriraj** kartica.  S ove stranice konfiguracije možete upravljati mnogo aspektima upravljanju lozinke u tvrtki ili ustanovi, uključujući:

- Omogućivanje i onemogućivanje za vraćanje izvorne lozinke za sve korisnike u imeniku
- Postavite broj izazove (ili jednom ili dvjema) korisnik mora proći kroz za ponovno postavljanje lozinke upisom
- Postavljanje određene vrste izazove želite omogućiti za korisnike u tvrtki ili ustanovi dolje:
 - Mobilni telefon (koda za potvrdu preko teksta ili govorni poziv)
 - Office telefon (govorni poziv)
 - Zamjenska e-pošte (koda za potvrdu putem e-pošte)
 - Sigurnosna pitanja (znanja provjere autentičnosti utemeljene na)
- Postavite broj pitanja korisnika morate se registrirati za korištenje sigurnosna pitanja načina provjere autentičnosti (vidljivo samo ako je omogućeno sigurnosna pitanja)
- Postavite broj pitanja korisnika morate unijeti tijekom Vrati izvorno da biste koristili sigurnosna pitanja načina provjere autentičnosti (vidljivo samo ako je omogućeno sigurnosna pitanja)
- Korištenje unaprijed Konzervirano, lokalizirane, sigurnosna pitanja, a koje korisnik može koristiti prilikom registracije za lozinke ponovno postavljanje (vidljivo samo ako je omogućeno sigurnosna pitanja)
- Definiranje prilagođenog sigurnosna pitanja koje korisnik može koristiti prilikom registracije za lozinke ponovno postavljanje (vidljivo samo ako je omogućeno sigurnosna pitanja)
- Zahtijevanje korisnika za registraciju na prilikom izmjene aplikaciju Access ploče na [http://myapps.microsoft.com](http://myapps.microsoft.com)za vraćanje izvorne lozinke.
- Zahtijevanje korisnicima ponovno provjerite svoje podatke iz prethodno registrirani nakon konfigurirati broja dana prošlo (vidljivo samo ako je omogućeno nametnuti Registracija)
- Pružanje prilagođena služba za e-pošte ili URL koji će se prikazati korisnicima u slučaju da imaju problema ponovno postavljanje lozinke
- Omogućavanje ili onemogućavanje mogućnost Upisima lozinke (kada je lozinka Upisima implementiran pomoću AAD povezivanja)
- Prikaz statusa agent za lozinku Upisima (kada je lozinka Upisima implementiran pomoću AAD povezivanja)
- Omogućivanje obavijesti e-poštom korisnicima kada vlastitu lozinku ponovno (pronaći u odjeljku **obavijesti** za [Portal za upravljanje Azure](https://manage.windowsazure.com))
- Omogućivanje obavijesti e-poštom administratorima kada drugi administratori ponovno vlastite lozinke (pronaći u odjeljku **obavijesti** [Portal za upravljanje Azure](https://manage.windowsazure.com)
- Brendiranje korisničku lozinku ponovno postavljanje portala i ponovno postavljanje lozinke poruke e-pošte s logotipom i naziv vaše tvrtke ili ustanove pomoću klijentu brendiranje značajke prilagodbe (pronaći u odjeljku **Svojstva direktorija** [Portal za upravljanje Azure](https://manage.windowsazure.com)

Da biste saznali više o konfiguriranju upravljanje lozinkama u vašoj tvrtki ili ustanovi, pročitajte članak [Početak rada: Upravljanje lozinkama Azure AD](active-directory-passwords-getting-started.md).

##<a name="user-registration-portal"></a>Portal Registracija korisnika
Prije nego što korisnici će moći koristiti ponovno postavljanje lozinke, korisničkih računa oblaka mora se ažurirati s podacima točan provjere autentičnosti da biste bili sigurni da mogu prolaziti kroz odgovarajući broj lozinku ponovno postavljanje izazove definirani administrator.  Administratori možete definirati podatke za provjeru autentičnosti u svoje korisničko ime pomoću na Azure ili Office web portalima, DirSync / Azure AD Connect ili komponente Windows PowerShell.

Međutim, ako biste radije imali korisnici registrirati vlastite podatke, i dajemo web-stranice koje korisnici mogu prijeći da bi se navedite informacije.  Ova stranica će korisnicima da biste odredili podatke za provjeru autentičnosti skladu lozinka ponovno postaviti pravila kojima je omogućeno u tvrtki ili ustanovi.  Kada je potvrđena te podatke, sprema se u njihove oblaka korisnički račun će se koristiti za oporavak računa kasnije. Evo što Registracija portala izgleda kao što su:

  ![][001]

Dodatne informacije potražite u članku [Početak rada: Upravljanje lozinkama Azure AD](active-directory-passwords-getting-started.md) i [najbolje prakse: Upravljanje lozinkama Azure AD](active-directory-passwords-best-practices.md).

##<a name="user-password-reset-portal"></a>Korisnički Portal za ponovno postavljanje lozinke
Nakon što ste omogućili samostalno ponovno postavljanje, postavite svoje tvrtke ili ustanove samostalno ponovno postavljanje pravila i ensured koju korisnici imaju odgovarajuće podatke za kontakt u imeniku, korisnici u vašoj tvrtki ili ustanovi moći ponovno postavljanje vlastite lozinki automatski s bilo kojeg web-stranice koji koristi račun tvrtke ili obrazovne ustanove za prijavu (primjerice [portal.microsoftonline.com](https://portal.microsoftonline.com)). Na stranicama kao što su oni će korisnici vidjeti na **ne možete pristupiti računu?** vezu.

  ![][002]

Klikom na sljedeću vezu pokrenut će se na portal samostalno ponovno postavljanje.

  ![][003]

Da biste saznali više o kako korisnici možete vratiti vlastite lozinke, potražite u članku [Početak rada: Upravljanje lozinkama Azure AD](active-directory-passwords-getting-started.md).

##<a name="user-password-change-portal"></a>Korisnički Portal za Promjena lozinke
Ako korisnicima želite promijeniti vlastite lozinke, oni možete učiniti pomoću portala za promjenu lozinke u bilo kojem trenutku.  Korisnici mogu pristupati portalu Promjena lozinke putem stranice profila ploča programa Access ili kliknete vezu "Promijeni lozinku" iz aplikacija sustava Office 365.  U slučaju prilikom isteka lozinke, korisnici će se zatražiti da ih mijenjati automatski prilikom prijave.

  ![][004]

U oba slučaja ako omogućila Upisima lozinku i korisnika ili vanjskog ili način sinkronizaciju lozinke, takve promijenjene lozinke zapisuju natrag na lokalni Active Directory. Evo što portala izgleda za promjenu lozinke kao:

  ![][005]

Dodatne informacije o načinu na koji korisnici mogu promijeniti vlastite lozinke lokalnog servisa Active Directory potražite u članku [Početak rada: Upravljanje lozinkama Azure AD](active-directory-passwords-getting-started.md).

##<a name="password-management-reports"></a>Upravljanje lozinkama izvješća
Navigacija do kartice **izvješća** i traženje u odjeljku **Zapisnika aktivnosti** , vidjet ćete dva izvješća u upravljanje lozinkama: **aktivnosti za ponovno postavljanje lozinke** i **aktivnosti registraciju za ponovno postavljanje lozinke**.  Pomoću tih dvaju izvješća, možete dobiti prikaz korisnika registracije za i koristite za vraćanje izvorne lozinke u tvrtki ili ustanovi. Evo kako takva izvješća izgledati [Portal za upravljanje Azure](https://manage.windowsazure.com):

  ![][006]

Dodatne informacije potražite u članku [dobiti uvid: Upravljanje lozinkama Azure AD izvješća](active-directory-passwords-get-insights.md).

##<a name="password-writeback-component-of-azure-ad-connect"></a>Komponenta Upisima lozinku Azure AD Connect
Ako lozinke korisnika u tvrtki ili ustanovi potječu iz lokalnog okruženja (ili putem web-mjesto vanjski pristup ili u okvir za sinkronizaciju lozinke), možete instalirati najnoviju verziju programa Azure AD Connect da biste omogućili ažuriranje te lozinke izravno iz oblaka.  To znači da kada korisnici zaboraviti ili želite promijeniti lozinku AD, što se može činiti pa izravno s weba.  Evo gdje možete pronaći Upisima lozinke u čarobnjaku za instalaciju Azure AD Connect:

  ![][007]

Dodatne informacije o Azure AD Connect potražite u članku [Početak rada: Azure AD Connect](active-directory-aadconnect.md). Dodatne informacije o Upisima lozinke potražite u članku [Početak rada: Upravljanje lozinkama Azure AD](active-directory-passwords-getting-started.md).


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Dokumentacija za ponovno postavljanje veze na lozinke
U nastavku su veze na sve stranice dokumentaciju za ponovno postavljanje lozinke za Azure AD:

* **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).
* [**Početak rada**](active-directory-passwords-getting-started.md) – upute kako bi korisnici mogli vratiti i mijenjati oblaka ili na lokalnim poslužiteljima lozinke
* [**Prilagodba**](active-directory-passwords-customize.md) – Saznajte kako prilagoditi izgled i dojam i ponašanje servisa potrebama vaše tvrtke ili ustanove
* [**Najbolje prakse**](active-directory-passwords-best-practices.md) – Saznajte kako brzo i učinkovito upravljanje lozinkama u tvrtki ili ustanovi
* [**Početak uvida**](active-directory-passwords-get-insights.md) – dodatne informacije o oglednim integrirane mogućnosti izvješćivanja
* [**Najčešća pitanja vezana uz**](active-directory-passwords-faq.md) – Pronađite odgovore na najčešća pitanja
* [**Otklanjanje poteškoća**](active-directory-passwords-troubleshoot.md) – Saznajte kako brzo otklanjanje poteškoća sa servisom
* [**Saznajte više**](active-directory-passwords-learn-more.md) – otvorite obuhvaća više razina u tehničke pojedinosti funkcioniranje servisa



[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"
