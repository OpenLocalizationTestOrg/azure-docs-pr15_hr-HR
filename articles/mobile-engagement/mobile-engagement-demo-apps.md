<properties
    pageTitle="Azure radnje mobilne aplikacije za pokazni videozapis | Microsoft Azure"
    description="Opisuje mjestu preuzimanja, kako koristiti i prednosti korištenja aplikacije pokazni videozapis Azure Mobile radnje"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/10/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-demo-app"></a>Azure radnje mobilne aplikacije za pokazni videozapis

Ne možemo objavili aplikacije pokazni videozapis Azure Mobile radnje za **iOS**, **Android**i platforme **Windows** da biste lakše pronašli korisne resurse i Saznajte više o mogućnostima Mobile.

Aplikaciju omogućuje vam sljedeće:

- Jednostavno pronaći korisne veze na resurse za mobilne radnje kao što su videozapisi, dokumentacija, na forumu za podršku i gdje Potenciranje zahtjeve za značajke.
- Sučelje uzorka obavijesti koje podržava Mobile radnje da biste dobili ideja za mobilne aplikacije.
- Referenca implementaciji koristite za proučavanje kako implementirati Mobile radnje u vlastiti aplikaciju. Dodatne informacije za:

    - Prikupljanje analize podataka.
    - Implementacija napredne obavijesti scenariji vrste kao što su *preko cijelog zaslona interstitial* ili *skočni*.
    - Implementacija ankete i ankete.
    - Implementacija tihu automatske podataka i automatske scenarija.   

## <a name="app-installation"></a>Instalacija aplikacije
Aplikacija nije dostupna u služi za pohranu sljedeće aplikacije:

- **Aplikacija Windows univerzalni pokazni videozapis**:

    - Preuzmite aplikaciju na [pohranu aplikacija u sustavu Windows](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
    - Aplikacija je razvio kao Windows 10 univerzalni aplikacije. Izvorni kod dostupna je na [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows).

- **iOS Pokaz aplikacije**:

    - Preuzmite aplikaciju na [Apple pohranu](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
    - Aplikacija je razvio u iOS Swift. Izvorni kod dostupna je na [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios).

- **Aplikacija sa sustavom android pokazni videozapis**:

    - Preuzmite aplikaciju na [pohranu Google Play](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
    - Izvorni kod dostupna je na [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android).

![Aplikacija Windows univerzalni pokazni videozapis][1]

![iOS Pokaz aplikacije][2]
![aplikacija sa sustavom Android pokazni videozapis][3]


## <a name="usage"></a>Korištenje

Koristite ovu aplikaciju na sljedeće načine:

**Preuzmite aplikaciju na uređaju iz aplikacije iz trgovine veza (koje se nalaze ranije):**

>[AZURE.IMPORTANT] Nije potreban račun za Azure ili morate povezati aplikacije u pozadini. Aplikaciju radi zasebno.

- Kada aplikacija na uređaju, može proći veze na izborniku s lijeve strane da biste pronašli korisne resurse vezane uz korištenje mobilne.
- Dodali smo [usluge RSS sažetaka sadržaja](https://aka.ms/azmerssfeed) u ovoj aplikaciji tako da se uvijek ažuriranja o najnovijim ažuriranjima proizvoda.
- Može proći i sučelje vrstu obavijesti koje podržava Mobile radnje za svaki platformu scenarija obavijesti uzorka. Ove obavijesti možete biti iskustvom lokalno – koja je, možete kliknuti gumbe na zaslonima da bi se prikazala obavijesti doživljaj, što je jednako slanja obavijesti s platformu Mobile radnje.

![Izbornik aplikacije za Windows][4]

![Izbornik aplikacije za iOS][5]
![izbornik aplikacije za Android][6]

**Preuzmite izvornog koda iz GitHub veza (koje se nalaze ranije):**

- Nakon što preuzmete izvornog koda, otvorite je u odgovarajući razvojno okruženje – XCode za iOS, Android Studio za Android i Visual Studio za Windows.
- Slijedite naše [Osnovni koraci za integraciju SDK](mobile-engagement-windows-store-dotnet-get-started.md) sljedeće tako da se možete povezati ovu aplikaciju Mobile radnje pozadinsku vlastito.
    - Morate konfigurirati niza za povezivanje u aplikaciji.
    - Morate konfigurirati platformu automatske obavijesti za aplikacije.
- Primijetit ćete da se samoj aplikaciji instrumented s mobilnog radnje. Dakle, otvorite aplikaciju nakon povezivanja s pozadinska, ćete moći vidjeti korisničke sesije, aktivnosti, događaji i tako dalje, na kartici **Monitor** .
- Također ćete moći slanje obavijesti za aplikaciju iz vlastite instance radnje Mobile, umjesto korištenja lokalnog obavijesti.
    - Ovdje možete dodati uređaju kao uređaj za testiranje pomoću izbornika stavke **dobili ID uređaja** u aplikaciji. Tako ćete dobiti ID uređaja koji se zatim registrirate kao uređaj za testiranje vašoj pozadinskoj instanci platforme.

    ![ID uređaja u sustavu Windows][7]

    ![ID uređaja u sustavu iOS][8]
    ![ID uređaja u sustavu Android][9]

## <a name="key-features-of-the-demo-app"></a>Ključne značajke aplikacije pokazni videozapis

- Kao što je navedeno ranije, uz aplikaciju, imate ključne resurse za mobilne radnje u Ruka. Može proći kroz veze na lijevom izborniku.

- Možete doći obavijesti iz aplikacije za svaku platformu. Ove obavijesti mogu biti isporučeni kao **samo obavijesti**, gdje kliknete obavijesti jednostavno otvara izvorni zaslona aplikacije (pomoću **precizno povezivanje**) – ili na **web-objava**, gdje izlaganje dodatne HTML sadržaj s mobilnog radnje pozadinska će se prikazati kada se klikne obavijesti.

    ![Obavijesti iz aplikacija][29]

- Na iOS, morate zatvoriti aplikaciju da biste vidjeli automatske obavijesti iz aplikacije ili sustav. Možete pogledati u implementaciji za dodavanje **Akcijski gumbi**, kao što su koji se dodaju se ova obavijest iz aplikacije za *povratne informacije* i *zajedničko korištenje* (tako da korisnik može potrajati akcija desno iz obavijesti o sam).

    ![Obavijesti iz aplikacije na iOS][11] ![Prikaz obavijesti iz aplikacije na iOS][14]

- Na Android mogućnosti koje su podržane dodajete s više redaka teksta (**Veliki tekst**) ili obavijesti slika (**Širu sliku**) na obavijest, zajedno s **akcijskim gumbima** (kao što je podržano u iOS).

    ![Obavijesti iz aplikacije na sustavu Android][12] ![Prikaz obavijesti iz aplikacije na sustavu Android][15]

- Na Windows 10, vidjet ćete kako se obavijesti izgledati na Računalu. Ta se obavijest također se prikazuje u **Centar za obavijesti**za Windows 10. Ne postoji podrška za dodavanje **akcijskih gumba** u trenutku u Windows SDK.

    ![Obavijesti iz aplikacije u sustavu Windows][10] ![Prikaz – aplikacija u sustavu Windows][13]

- Možete doći zadani "u samoj aplikaciji" obavijesti za svaki platformu. Ovo je sučelje za dva koraka gdje se prozor za **obavijesti** prikazuje prvi put. Kada je kliknete, otvara se preko cijelog zaslona **Objava**, kao što je prikazano u sljedećim snimku zaslona. Sadržaj ova objava potječe iz vašoj pozadinskoj instanci Mobile radnje. SDK sadrži predloške za obavijesti i objave. Možete jednostavno prilagođavati njima, kao što je prikazano u ovu aplikaciju pokazni videozapis s dodavanja naš logotip i Bojanje.  

    ![U aplikaciji obavijesti u sustavu Windows][16]

    ![U aplikaciji obavijesti o sustavu iOS][17]  ![U aplikaciji obavijesti na sustavu Android][18]

    **iOS**, **sa sustavom Android**

- Značajka **kategorija** radnje Mobile možete koristiti i da biste prilagodili tog problema zadani. U aplikaciji pokazni videozapis ne možemo ste planirati uobičajenih načina za promjenu sučelje za obavijesti. Imajte na umu da kategorija značajka još nije podržana u Windows SDK.

    **Preko cijelog zaslona interstitial:**

    ![U aplikaciji obavijesti - Interstitial kategorija][30]

    ![Interstitial kategorija u sustavu iOS][21]  ![Interstitial kategorija na sustavu Android][22]

    **Skočna obavijest:**

    ![U aplikaciji obavijesti - skočni kategorija][31]

    ![Skočna obavijest u sustavu iOS][19]   ![Skočna obavijest na sustavu Android][20]

**iOS**, **sa sustavom Android**

- Korištenje mobilne podržava i specijalizirane vrstu obavijesti u aplikaciji naziva **ankete**. Omogućuje pošaljite brzi ankete korisnicima segmentiranog aplikacija. Možete dodati pitanja i mogućnosti za svako pitanje kao sljedeće snimku zaslona. To pa se prikazat će kao u aplikaciji obavijest o korisniku aplikacije.   

    ![Ankete obavijesti][32]

    ![Anketa u sustavu Windows][26]

    ![Upitnik u sustavu iOS][27]   ![Anketa u Android][28]

**iOS**, **sa sustavom Android**

- Korištenje mobilne podržava i slanje tihu **Podataka automatske** obavijesti. Pomoću ove obavijesti možete poslati podatke iz servisa (kao što je JSON podatke u sljedećem primjeru), koje možete učiniti u aplikaciji i poduzeti neku akciju. Primjer je kako ćemo mijenjate cijenu stavke selektivno pomoću podataka automatske obavijesti.

    ![Podaci automatske obavijesti][33]

    ![Podaci automatske obavijesti u sustavu Windows][23]

    ![Podaci automatske obavijesti na iOS][24]  ![Podaci automatske obavijesti na sustavu Android][25]

**iOS**, **sa sustavom Android**

> [AZURE.NOTE] Detaljne detaljne upute za bilo koju od tih obavijesti možete vidjeti klikom na **kliknite ovdje da biste upute za slanje te obavijesti o Mobile radnje platformu** na bilo kojem zaslonu obavijesti uzorka.


[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
