<properties
   pageTitle="Brendiranje smjernice za aplikacije | Microsoft Azure"
   description="Vodič za potpun vezanima uz programiranje resursi za Azure Active Directory"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/23/2016"
   ms.author="mbaldwin"/>


# <a name="branding-guidelines-for-applications"></a>Brendiranje smjernice za aplikacije


U ovoj se temi opisuje izgleda trebali biste koristiti prilikom razvoja aplikacije s Azure Active Directory (Azure AD). Ovim smjernicama će vam pomoći izravno korisnici žele koristiti svoje tvrtke ili obrazovne ustanove račun, upravljati u Azure AD ili njihove osobnog računa za prijavu i prijava u aplikaciju.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Osobni računi za nasuprot rad ili školske računi tvrtke Microsoft

Microsoft upravlja dvije vrste korisničkih računa:

- **Osobni računi** (prijašnji Windows Live ID). Poslovne subjekte predstavljaju odnos između *pojedinačnih* korisnika i Microsoft, a služe za pristup potrošača uređaje i usluge tvrtke Microsoft. Poslovne subjekte su namijenjene osobnu upotrebu.

- **Računi tvrtke ili obrazovne ustanove.** Poslovne subjekte upravlja Microsoft ime tvrtkama ili ustanovama koje koriste Azure Active Directory. Poslovne subjekte koriste se za prijavu u Office 365 i drugih servisa za poslovno od Microsofta.

Microsoftova računa tvrtke ili obrazovne ustanove obično krajnjim korisnicima (zaposlenika, studenti, savezna Zaposlenici) po dodjeljuju svoje tvrtke ili ustanove (tvrtke, obrazovne ustanove, državne agencije). Poslovne subjekte su usvojili izravno u oblak platforme Azure AD ili sinkronizirali s Azure AD iz lokalnog imenika, kao što je Windows Server Active Directory. Microsoft je *custodian* računa tvrtke ili obrazovne ustanove, ali računi su posjeduje i kontrolira tvrtke ili ustanove.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Referenciranje Azure AD računa u aplikaciji

Microsoft ne izlažu krajnjim korisnicima u Azure ili nazive marke servisa Active Directory i nijednog treba vam.

- Kada ste prijavljeni korisnika, poslužite se naziv i logotip najveću moguću u tvrtki ili ustanovi. To je bolje nego generički uvjete navedene kao što su "vašoj tvrtki ili ustanovi".

- Kada korisnici ne prijavite, pogledajte njihove račune kao "rad ili školske računi" i korištenje logotip sustava Microsoft da biste prenijeli poslovne subjekte upravlja Microsoft. Nemojte koristiti termina kao što su "enterprise račun", "poslovni račun" ili "korporativni račun za" Stvaranje zbrku korisnika.

## <a name="user-account-pictogram"></a>Korisnički račun pictogram
U starijoj verziji programa te pogrešaka, preporučujemo korištenje "plavom iskaznice" pictogram. Na temelju povratnih informacija korisnika i za razvojne inženjere, sada preporučujemo korištenje logotip sustava Microsoft umjesto toga. To će korisnicima olakšati razumijevanje koje možete koristiti na račun koji koriste Office 365 ili druge Microsoftove servise za poslovno da biste se prijavili aplikaciju.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Registracija i prijaviti pomoću Azure AD

Aplikacije mogu predstavljati zasebnom putove za prijavu i prijaviti i sljedeći odjeljci sadrže vizualne upute za oba scenarija.

**Ako aplikacija podržava krajnji korisnik znak gore (npr. besplatnu probnu verziju ili freemium model)**: možete prikazati gumbu za **prijavu** koje korisnicima omogućuje pristup aplikacije s računa tvrtke ili svoj osobni račun. Azure AD će prikazati upit pristanak prvi put pristupi aplikacije.

**Ako aplikaciju potrebne dozvole koje samo administratori mogu pristajete, ili aplikacije potreban je tvrtke ili ustanove licenciranje**: treba odvojite acquisition administrator iz Prijava korisnika. **Gumb "Dohvati aplikacije"** bit ćete preusmjereni administratori da prijaviti, a zatim zatražite da biste dodijelili dozvole ime korisnika u tvrtki ili ustanovi. Ima dodatne prednosti Ukidanje krajnjim korisnicima pristanak upute za aplikacije.

## <a name="visual-guidance-for-app-acquisition"></a>Vizualne upute za nabavu aplikacije

Veza "nabavite aplikaciju" obavezno preusmjeravanje korisnika dopuštanje pristupa Azure AD (autorizaciju) stranice da biste omogućili administrator u tvrtki ili ustanovi da biste autorizirali aplikacije da biste imali pristup na podatke svoje tvrtke ili ustanove koji hostira Microsoft. Pojedinosti o tome kako zatražiti pristup se spominju u članku [Integraciji aplikacije pomoću servisa Azure Active Directory](active-directory-integrating-applications.md) .

Kada administratori pristanak na aplikaciju, možete odabrati da biste ga dodali korisnika sustava Office 365 okruženja aplikacije za pokretač (dostupno iz ikone s kvadratićima i [https://portal.office.com/myapps](https://portal.office.com/myapps)). Ako želite Oglasite tu mogućnost, možete koristiti izraze "Dodajte ovu aplikaciju u vašoj tvrtki ili ustanovi", a zatim Prikaži gumb ovako:

![Vrsta aplikacija i scenarijima](./media/active-directory-branding-guidelines/add-to-my-org.png)

No preporučujemo da pišete tekst objašnjenja tipkanje gumba. Ako, na primjer:
> *Ako već koristite Office 365 ili drugih servisa za poslovne od Microsofta, možete jednostavno dodijeliti < your_app_name > pristup podacima iz vaše tvrtke ili ustanove. To će Dopusti korisnicima da biste pristupili < your_app_name > s postojeći poslovni subjekti rad.*


## <a name="visual-guidance-for-sign-in"></a>Vizualne upute za prijavu
Pokrenite aplikaciju treba prikazati znak u gumb kojim se preusmjerava korisnici krajnjoj za prijavu koja odgovara protokol koristiti za integraciju s Azure AD. Sljedeći odjeljak sadrži detalje na taj gumb treba izgleda.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Pictogram i ", prijavite se Microsoft"
Je pridruživanje logotip sustava Microsoft i uvjete "Znak u s Microsoft" koji jedinstveno označava Azure AD između drugih davatelja identiteta aplikacije mogu podržavati. Ako nemate dovoljno prostora za "Znak u s Microsoft" je u redu da biste skratili na "Za prijavu".

![Vrsta aplikacija i scenarijima](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Vrsta aplikacija i scenarijima](./media/active-directory-branding-guidelines/sign-in-light.png)

Tamna primijenjen možete koristiti i za gumbe.

![Vrsta aplikacija i scenarijima](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Vrsta aplikacija i scenarijima](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Brendiranje što činiti, a

**Ne** koristite "računa tvrtke ili škole" u kombinaciji s gumb "Prijava u s Microsoft" Dodatne objašnjenje pomažu krajnjim korisnicima prepoznaje li će ga moći koristiti. **Nemojte** koristiti ostale termine kao što su "enterprise račun", "poslovni račun" ili "korporativni račun."

**Nemojte** koristiti "ID za Office 365" ili "ID-JA Azure". Office 365 je i naziv potrošača koja nudi od Microsofta koji ne koristi Azure AD za provjeru autentičnosti.

Logotip sustava Microsoft **ne** mijenjati.

**Ne** izlažu krajnji korisnici marke Azure ili servisa Active Directory. To je u redu no da biste koristili te uvjete s razvojnim inženjerima, stručnjaci za IT i administratori.

## <a name="navigation-dos-and-donts"></a>Navigacija što činiti, a

**Ne** omogućuje korisnicima odjavite, a prebaciti na neki drugi račun. Dok većina ljudi imati jedan osobnog računa iz Microsoft/Facebook/Google/Twitter, korisnici često povezane su s više od jedne tvrtke ili ustanove. Podrška za više korisnika za prijavljeni u uskoro dostupno.
