
<properties
    pageTitle="Azure autentikatora za Android | Microsoft Azure"
    description="Aplikacija Microsoft Azure autentikatora mogu se prijaviti da biste pristupili resursima Rad. Aplikaciju Azure autentikatora vas obavještava o zahtjeva za potvrdu na čekanju dvofaktorska analiza varijance prikazom upozorenja za mobilni uređaj."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="azure-authenticator-for-android"></a>Azure autentikatora za Android

IT administrator može imati preporučuje korištenje autentikatora za Microsoft Azure za prijavu da biste pristupili resursi za rad. Ova aplikacija nudi sljedeće dvije mogućnosti za prijavu:

* Višestruka provjera autentičnosti omogućuje sigurne računa tvrtke ili obrazovne ustanove s dva koraka provjere. Se prijavite u nešto znati (na primjer, lozinku) i zaštita račun pomoću još više brojem imate (sigurnosni ključ iz aplikacije). Aplikaciju Azure autentikatora vas obavještava o zahtjeva za potvrdu na čekanju dvofaktorska analiza varijance prikazom upozorenja za mobilni uređaj. Trebate samo prikaz zahtjev u aplikaciji i dodirnite potvrda ili poništavanje. Umjesto toga koje možda zatražiti da biste unijeli pristupni izraz prikazati u aplikaciji.

* Poslovni račun omogućuje pretvaranje Android telefona ili tableta u pouzdani uređaj i navedite jednu prijavu (SSO) tvrtke aplikacijama. IT administrator može potrebno da biste dodali poslovni račun radi pristupa resursima tvrtke. SSO omogućuje prijavite se u jednom i automatski avail za prijavu u svim aplikacijama tvrtke učinio dostupnima vama.

## <a name="installing-the-azure-authenticator-app"></a>Instaliranje aplikacije za Azure autentikatora

Možete instalirati aplikaciju Azure autentikatora iz trgovine Google Play.
Upute za dodavanje računa za rad s uređaja za dodavanje veze za vanjskih u ne - Samsung Android Samsung Android uređaj su malo drugačije. Upute za obje navedena su u nastavku.

<a name="adding-the-work-account-from-samsung-android-device"></a>Dodavanje računa za rad s uređaja sa sustavom Samsung Android
----------------------------------------------------------------------------------------------------------------
###<a name="adding-the-work-account-through-the-app-home-screen"></a>Dodavanje poslovnog računa putem aplikacija na početnom zaslonu

Sljedeće upute primjenjuju Samsung GS3 i iznad gumba telefona ili bilješka 2 i iznad tableta.

1. Na početnom zaslonu aplikacije Prihvatite licencni ugovor za krajnjeg korisnika (EULA).
2. Na zaslonu aktivirati račun kliknite Kontekstni izbornik s desne strane i odaberite **poslovnog računa**.
3. Na zaslonu za dodavanje računa odaberite** Račun za rad**.
4. Na zaslonu administratora uređaja za aktiviraj kliknite **Aktiviraj**.
5. Na zaslonu pravilnik o zaštiti privatnosti potvrdite okvir, a zatim kliknite **Potvrdi**.
6. Na zaslonu za uključivanje radnom mjestu unesite ID korisnika u svojoj tvrtki ili ustanovi, a zatim kliknite **Pridruži se**.
7. Da biste se prijavili aplikaciju Azure autentikatora, unesite svoje tvrtke ili ustanove s *** poslovnog subjekta i lozinku i kliknite **Prijava**.
8. Na sljedećem zaslonu koji prikazuje informacije o višestruke provjere autentičnosti (MFA) je za dodatna sigurnost, a nije obavezno. Vidjet ćete ovaj zaslon Ako vaše tvrtke ili obrazovne ustanove zahtijeva druge provjere autentičnosti za stvaranje poslovnog računa. Pruža upute da biste dodatno provjerite je li vaš račun.
9. Na zaslonu uključivanje radnom mjestu prikazuje poruku "**Pridruživanje vašem radnom mjestu**". Aplikaciju Azure autentikatora pokuša pridružiti uređaj na vašem radnom mjestu.
10. Trebali biste vidjeti poruku pridruženo radnog prostora na sljedećem zaslonu.

>[AZURE.NOTE]
> Račun za jedan radni dopušteno na uređaju.

### <a name="adding-the-work-account-from-the-settings-menu"></a>Dodavanje računa za rad na izborniku postavke
Kada instalirate aplikaciju Azure autentikatora, možete i stvoriti poslovnog računa iz Upravitelj računa Android.

1. Na izborniku postavke dođite do **računi** i kliknite **Dodaj račun**.
2. Slijedite korake od 3 10 u postupak dodavanja računa rad putem aplikacije početnog zaslona da biste dodali račun za rad.

<a name="adding-the-work-account-from-a-non-samsung-android-device"></a>Dodavanje računa za rad s uređaja koji nisu iz programa Samsung Android
------------------------------------------------------------------------------------------------------------------
### <a name="adding-the-work-account-through-the-app-home-screen"></a>Dodavanje poslovnog računa putem aplikacija na početnom zaslonu

1. Na početnom zaslonu aplikacije Prihvatite licencni ugovor za krajnjeg korisnika (EULA).
2. Na zaslonu aktivirati račun kliknite Kontekstni izbornik s desne strane i odaberite **poslovnog računa**.
3. Na zaslonu računi kliknite **Dodaj račun**.
4. Ako vidite na zaslonu računi, kliknite **Dodaj račun**. Ako poslovni račun već stvorili prethodno, vidjet ćete sinkronizaciju zaslona prikazuje postojeći račun tvrtke. Poslovni račun možete zadržati dodirom na strelicu natrag na početni zaslon. Umjesto toga možete odabrati račun koji želite ukloniti i ponovno stvoriti novi rad računa u sustavu u radnom prostoru uključiti zaslon, unesite ID korisnika u svojoj tvrtki ili ustanovi i kliknite Pridruži se.
5. Da biste se prijavili aplikaciju autentikatora Azure unesite račun tvrtke ili ustanove i lozinku, a zatim kliknite **Prijava**.
7. Na sljedećem zaslonu koji prikazuje informacije o višestruke provjere autentičnosti (MFA) je za dodatna sigurnost, a nije obavezno. Vidjet ćete ovaj zaslon Ako vaše tvrtke ili obrazovne ustanove zahtijeva druge provjere autentičnosti za stvaranje poslovnog računa. Pruža upute da biste dodatno provjerite je li vaš račun.
8. Na sljedećem zaslonu kliknite **u redu** . Promijenite naziv certifikata.
poruka, "Uključivanja vašem radnom mjestu". Aplikaciju Azure autentikatora pokuša pridružiti uređaj na vašem radnom mjestu.
Trebali biste vidjeti poruku pridruženo radnog prostora na sljedećem zaslonu.

>[AZURE.NOTE]
> Račun za jedan radni dopušteno na uređaju.

Kada instalirate aplikaciju Azure autentikatora, možete i stvoriti poslovnog računa iz Upravitelj računa Android.

1. Na izborniku **Postavke** dođite do računi i kliknite **Dodaj račun**.
2. Slijedite korake 2-7 u postupak dodavanja računa rada kroz u aplikaciju početnom zaslonu **, da biste dodali račun za rad.

### <a name="how-to-find-out-which-version-is-installed"></a>Kako saznati koja je verzija instalirana

1. Možete saznati koja je verzija aplikacije Azure autentikatora i verzije pridružene usluge su instalirani na vašem uređaju.
2. Na skočnom izborniku kliknite **o**.
3. Zaslon o prikazuje usluge aplikaciju i verzija instalirana na vaš uređaj.
 
### <a name="sending-log-files-to-report-issues"></a>Slanje datoteke zapisnika probleme s izvješćima

1. Slijedite navedene upute na Microsoftove Support prijaviti incident uz aplikaciju Azure autentikatora, dobili incidenta broj i slanje datoteke zapisnika protiv dodijeljene incidenta broj na sljedeći način:
2. Na skočnom izborniku kliknite **Prijava**.
3. Ako imate incident otvorena s Microsoft Support, zabilježite broj incidenta (morat ćete je za kasnije korak). Ako već niste stvorili slučajem za podršku, a želite pomoć, slijedite upute na [Microsoftovoj službi za podršku](https://support.microsoft.com/en-us/contactus) da biste otvorili novi incident.
4. Na zaslonu za zapisivanje, kliknite **Pošalji odmah**.
5. Odaberite davatelja usluga e-pošte koji želite koristiti.
7. Ako već imate otvorene incident Microsoft Support, obratite se dodijeljena problem da biste saznali kako poslati podatke te je pridružen prijave inženjer za podršku. Inženjer za podršku će vam dati informacije za e-pošte redak adrese i predmet. Ako već nemate slučajem za podršku, slijedite upute na Microsoftove Support da biste otvorili novi incident.
9. Uredite redak **Prima** i **Predmet s podacima koje ste primili iz Microsoft Support** .
10. Aplikaciju Azure autentikatora pridružuje datoteka zapisnika e-pošte koju šaljete. Opišite problem kada se pojave, ažuriranje popisa primatelja (nije obavezno) i slanje e-pošte.

### <a name="deleting-the-work-account-and-leaving-your-workplace"></a>Brisanje računa tvrtke i ostavite vašeg radnog prostora

Možete ukloniti poslovnog računa koji ste stvorili u bilo kojem trenutku na sljedeći način:

**Da biste izbrisali poslovnog računa na izborniku postavke**

1. Upravitelj za račune odaberite **poslovnog računa**.
2. Na zaslonu poslovnog računa u odjeljku **Općenite postavke**odaberite **Postavke računa – ostavite mrežom na radnom mjestu**.
3. Odaberite **ostavite** na zaslonu **Uključivanje radnom mjestu** .
4. Kada se prikaže poruka "Su jeste li sigurni da želite ostaviti radnom mjestu", kliknite **u redu** .
5. Time ste izbrisali poslovni račun s vašeg radnog prostora.

>[AZURE.NOTE]
>Preporučuje se da ne koristite mogućnost Ukloni račun da biste izbrisali poslovnog računa, kao što je ta mogućnost ne funkcioniraju u nekim starijim verzijama Android.

##<a name="uninstalling-the-app"></a>Deinstalacijom aplikacije

Na uređaju sa sustavom Samsung Android uređaj administratorske ovlasti mora se ukloniti na sljedeći način prije deinstalacije sustava 
1. Na stranici **Postavke**u odjeljku **sustav**odaberite **Sigurnost**.
2. Unutar D**evice Administracija**kliknite **Administratori uređaja**. Provjerite je li poništen potvrdni okvir pokraj **Azure autentikatora** .

##<a name="troubleshooting"></a>Otklanjanje poteškoća

Ako se prikaže **Pogreška Keystore**, možda nemate zaključavanje zaslona skup gore PIN-om. Da biste riješili taj problem, deinstalirajte aplikaciju Azure autentikatora, konfiguriranje PIN za zaključani zaslon i ponovno instalirajte aplikaciju.
