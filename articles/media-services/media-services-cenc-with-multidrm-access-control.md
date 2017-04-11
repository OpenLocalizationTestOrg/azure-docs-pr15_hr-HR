<properties 
    pageTitle="CENC s više DRM i kontrola pristupa: referenca dizajna i implementaciju Azure i Azure Media Services | Microsoft Azure" 
    description="Upute za licenciranje Microsoft® Smooth Streaming klijent Porting Kit." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan"  
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="willzhan;kilroyh;yanmf;juliako"/>

#<a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC s više DRM i kontrola pristupa: referenca dizajna i implementaciju Azure i Azure Media Services

##<a name="key-words"></a>Ključne riječi
 
Azure Active Directory, servisa Azure Media Services Azure Media Player, dinamični šifriranje licence isporuke, PlayReady, Widevine, FairPlay, uobičajenih Encryption(CENC), višestruki DRM, Axinom, CRTICE, EME, MSE, JSON Web tokena (JWT), zahtjevima, Moderna preglednici, dinamične ključ, simetričnu ključ, asimetričnim ključ, OpenID povezati, X509 certifikata. 

##<a name="in-this-article"></a>U ovom članku

U ovom članku možete pronaći u sljedećim temama:

- [Uvod](media-services-cenc-with-multidrm-access-control.md#introduction)
    - [Pregled ovog članka](media-services-cenc-with-multidrm-access-control.md#overview-of-this-article)
- [Referenca dizajna](media-services-cenc-with-multidrm-access-control.md#a-reference-design)
- [Mapiranje dizajna s tehnologije za implementaciju](media-services-cenc-with-multidrm-access-control.md#mapping-design-to-technology-for-implementation)
- [Implementacija](media-services-cenc-with-multidrm-access-control.md#implementation)
    - [Postupci za implementaciju](media-services-cenc-with-multidrm-access-control.md#implementation-procedures)
    - [Neke preprekama u implementaciji](media-services-cenc-with-multidrm-access-control.md#some-gotchas-in-implementation)
- [Dodatne teme za implementaciju](media-services-cenc-with-multidrm-access-control.md#additional-topics-for-implementation)
    - [HTTP ili HTTPS](media-services-cenc-with-multidrm-access-control.md#http-or-https)
    - [Potpisivanje ključa prilikom prelaska Azure Active Directory](media-services-cenc-with-multidrm-access-control.md#azure-active-directory-signing-key-rollover)
    - [Gdje je pristup tokena?](media-services-cenc-with-multidrm-access-control.md#where-is-the-access-token)
    - [Što je s Live strujanje?](media-services-cenc-with-multidrm-access-control.md#what-about-live-streaming)
    - [Što je s poslužiteljima licence izvan servisa Azure Media Services?](media-services-cenc-with-multidrm-access-control.md#what-about-license-servers-outside-of-azure-media-services)
    - [Što ako želim koristiti prilagođene STS?](media-services-cenc-with-multidrm-access-control.md#what-if-i-want-to-use-a-custom-sts)
- [Dovršeni sustava i test](media-services-cenc-with-multidrm-access-control.md#the-completed-system-and-test)
    - [Korisničko ime za prijavu](media-services-cenc-with-multidrm-access-control.md#user-login)
    - [Pomoću proširenja šifrirane medijskih sadržaja za PlayReady](media-services-cenc-with-multidrm-access-control.md#using-encrypted-media-extensipons-for-playready)
    - [Pomoću EME za Widevine](media-services-cenc-with-multidrm-access-control.md#using-eme-for-widevine)
    - [Ne ovlašteni korisnici](media-services-cenc-with-multidrm-access-control.md#not-entitled-users)
    - [Pokretanje prilagođene tokena servis za sigurnu](media-services-cenc-with-multidrm-access-control.md#running-custom-secure-token-service)
- [Sažetak](media-services-cenc-with-multidrm-access-control.md#summary)

##<a name="introduction"></a>Uvod

To je dobro poznate je složeno za dizajniranje i stvaranje Podsustav DRM za programa OTT ili online strujanje rješenja. Te je uobičajeno operatori online videozapisa davateljima spoljni taj dio specijalizirane DRM davateljima usluga. Cilj ovog dokumenta je kako bi se prikazao referenca dizajna i implementacija završetka do kraja Podsustav DRM OTT ili online strujanje rješenja.

Ciljano čitatelji ovog dokumenta su inženjeri za rad u Podsustav DRM OTT ili online strujanje/višestruka-screen rješenja ili neki čitači zanimati Podsustav DRM. Pretpostavlja se čitatelji upoznati s barem jedno od tehnologije DRM na tržištu, kao što su PlayReady, Widevine, FairPlay ili Adobeova programa Access.

Po DRM, ne možemo uvrštavati CENC (uobičajenih šifriranje) s više DRM. Glavna trend u mreži strujanje i industrijskog OTT je za uporabu CENC višestruka-native-DRM na različite platforme klijenta koji je shift iz prethodne trend pomoću jedne DRM i SDK klijenta za različite platforme klijenta. Ako koristite CENC uz višestruka-native-DRM, PlayReady i Widevine šifrirane po specifikacija [Uobičajenih šifriranja (CENC ISO/IEC 23001 7)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) .

Prednosti CENC s više DRM su na sljedeći način:

1. Smanjuje šifriranje cijena Budući da jedan šifriranje obrada koristi ciljanja različite platforme s nativni DRMs;
1. Smanjuje troškove upravljanje sredstva šifriranim jer je potreban samo jednu kopiju sredstva šifriranim;
1. Uklanja DRM klijent licenciranje trošak jer nativni klijent DRM obično je besplatna na njegov izvorni platformi.

Microsoft je active promoter CRTICE i CENC zajedno s neke uređaje glavne industrijskih. Microsoft Azure Media Services sadrži je pruža podršku CRTICE i CENC. Nedavne najave potražite Mingfei, blogove: [Objavljivanje Google Widevine licence isporukom u servisa Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)i [Azure Media Services dodaje Google Widevine pakiranja za višestruki DRM strujanje](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Pregled ovog članka

Cilj ovaj članak obuhvaća sljedeće:

1. Omogućuje dizajn referenca od Podsustav DRM CENC pomoću više DRM;
1. Nudi pregled implementacije na platformi Microsoft Azure/servisa Azure Media Services;
1. U članku se opisuje neke teme dizajna i implementaciju.

U članku "višestruki DRM" obuhvaća sljedeće:

1. Microsoft PlayReady
1. Google Widevine
1. Apple FairPlay (još nije podržano u web-servisa Azure Media Services)

U sljedećoj su tablici navedene nativni platformu/lokalnog aplikacije i preglednici koje podržava svaki DRM.

**Klijent platforme**|**Podrška za izvorni DRM**|**Preglednik/aplikacije**|**Strujanje oblici**
----|------|----|----
**Pametno televizori, operator STBs, OTT STBs**|PlayReady prvenstveno, i/ili Widevine i/ili druge|Linux, Opera, WebKit, drugi|Različite oblike
**Uređaje Windows 10 (Windows PC, tabletima sa sustavom Windows, Windows Phone, Xbox)**|PlayReady|MS rub/IE11/EME<br/><br/><br/>UWP|CRTICU (za HLS, PlayReady nije podržan)<br/><br/>CRTICE, Smooth Streaming (za HLS, PlayReady nije podržan) 
**Uređaje sa sustavom android (telefona, tableta TV)**|Widevine|Chrome/EME|CRTICA
**iOS (iPhone, iPad), klijenti za OS X i Apple TV**|FairPlay|Safari 8 +/ EME|HLS
**Dodatak: Adobe Primetime**|Pristup Primetime|Dodatak za preglednik|HDS, HLS

Considering trenutno stanje implementacije za svaki DRM servisa će obično želite implementirati 2 ili 3 DRMs da biste provjerili adrese sve vrste krajnje točke na najbolji mogući način.

Postoji tradeoff između složenost logike servisa i složenosti na klijentskoj strani dosegne određenu razinu korisničkog sučelja u različitim klijentima.

Da biste odabir, imajte na umu sljedeće činjenice:

- PlayReady nativno implementirana u svakoj uređaj u sustavu Windows, na nekim uređajima sa sustavom Android i dostupne putem softver SDK-ovi na gotovo bilo koju platformu
- Widevine nativno implementirana u svim uređajima sa sustavom Android, Chrome i neki drugi uređaji
- FairPlay je dostupan samo na iOS i Mac OS klijente ili putem iTunes.

Da bi se uobičajeni DRM višestruki bio sljedeći:

- Mogućnost 1: PlayReady i Widevine
- Mogućnost 2: PlayReady, Widevine i FairPlay


## <a name="a-reference-design"></a>Referenca dizajna

U ovom odjeljku ne možemo prikazati referenca dizajn koja je agnostic za tehnologije za implementaciju ga.

Podsustav DRM može sadržavati sljedeće komponente:

1. Upravljanje ključevima
1. Pakiranje DRM
1. Isporuka DRM licence
1. Provjera prava
1. Provjera autentičnosti/autorizacije
1. Player
1. Polazište/CDN

Sljedeći dijagram prikazuje više razine interakcije među komponente Podsustav DRM.

![Podsustav DRM s CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)
 
Postoje tri osnovna "slojevima" u dizajnu:

1. Sloj pozadinskih (crno) koji se ne prikazuje kao vanjski;
1. "DMZ" sloja (plava) koji sadrži sve krajnje točke nasuprotne javno;
1. Javni Internet sloja (Svijetloplava) koji sadrži CDN i uređaje s promet javnog Interneta.
 
Mora biti alat za upravljanje sadržajem za kontrolu DRM zaštitu, regardless statički ili dinamički šifriranje. Trebali biste unosi za šifriranje DRM obuhvaćaju sljedeće:

1. MBR videosadržaj;
1. Sadržaja key.
1. URL-ovi acquisition licence.

Reprodukcija vrijeme više razine tijek je:

1. Provjere autentičnosti korisnika;
1. Ovlaštenja se stvara za korisnika;
1. DRM zaštićeni sadržaj (manifest) se preuzima u player.
1. Reproduktor šalje zahtjev za nabavu licence licence poslužiteljima zajedno s ključem ID i autorizacije tokena.

Prije prelaska na sljedeću temu, nekoliko riječi o dizajnu upravljanja ključem.

**ContentKey – da biste resursa**|**Scenarij**
------|---------------------------
1 – 1|Najjednostavniji slova. Pruža resi kontrolu. No to obično rezultira najveće trošak isporuke licence. Pri minimalne jednom licenca za svaku zaštićeni imovine potreban je zahtjev.
1 – prema-više|Možete koristiti isti sadržaja ključa za više resursi. Na primjer, za sva sredstva u logičke grupe kao što je stupca ili podskup stupca (ili Gene filma), možete koristiti jedne tipke sadržaja.
–-1|Više tipki sadržaja su vam potrebne za svaku resursa. <br/><br/>Ako, na primjer, ako morate primijeniti dinamičke CENC zaštitu s više DRM za MPEG-CRTICOM i dinamični AES 128 šifriranje za HLS, morat ćete dvije zasebne sadržaja tipke, svaka s vlastitom ContentKeyType. (Za sadržaja koristi se za dinamičku CENC zaštitu ključ ContentKeyType.CommonEncryption treba koristiti, za sadržaja ključa koji se koristi za dinamičku AES 128 šifriranje, ContentKeyType.EnvelopeEncryption želite koristiti.)<br/><br/>Drugi primjer u CENC zaštitu CRTICE sadržaj u teorija nešto sadržaja ključ može se koristiti za strujanje videozapisa i druge tipke sadržaja da biste zaštitili strujanje audiozapisa zaštiti. 
Više-prema - više|Kombinacija iznad dva scenarija: jedan skup sadržaja tipke koriste se za svaki od više resursima u istom resursa "grupe".


Drugi važan faktor treba uzeti u obzir je korištenje stalni i koje nisu stalni licence.

Zašto u obzir sljedeće važne? 

Ako koristite javno oblaka za isporuku licence imaju Izravni utjecaj na trošak isporuke licence. Pogledajmo razmislite o sljedećim dvije različite dizajn slučajevima za ilustraciju:



1. Mjesečni pretplatu: korištenje trajnog licence i 1-prema-više sadržaja ključ resursa mapiranje. Npr. za sve djecu filmova, koristimo jedan sadržaja ključa za šifriranje. U ovom slučaju: 

    Ukupno # licence potrebne za sve djecu filmovi/uređaj = 1

1. Mjesečni pretplatu: korištenje koje nisu stalni licence i 1-1 mapiranja između sadržaja ključ i resursa. U ovom slučaju:

    Ukupno # licence potrebne za sve djecu filmovi/uređaj = [# filmova nadzirane] x [# sesije]

Kao što možete lako vidjeti, dvije različite dizajne vrlo različito licence zahtjev uzoraka dakle rezultat licence isporuke troškova Ako je servis za isporuku licence nudi javno oblaku kao što su servisa Azure Media Services.

## <a name="mapping-design-to-technology-for-implementation"></a>Mapiranje dizajna s tehnologije za implementaciju

Zatim ćemo mapirajte naš generički dizajn tehnologije na platformi Microsoft Azure/servisa Azure Media Services navođenjem tehnologiji koju želite koristiti za svaki sastavni blok.

Sljedeća tablica prikazuje mapiranje:

**Sastavnog bloka**|**Tehnologija**
------|-------
**Player**|[Azure Media Player](https://azure.microsoft.com/services/media-services/media-player/)
**Davatelja identiteta (IDP)**|Azure Active Directory
**Sigurna tokena (STS)**|Azure Active Directory
**Tijek rada za zaštitu DRM**|Azure Media Services dinamički zaštitu
**Isporuka DRM licence**|1. azure Media Services licence isporuke (PlayReady, Widevine, FairPlay) ili <br/>2. poslužitelj za licence Axinom <br/>3. prilagođene PlayReady licencni poslužitelj
**Polazište**|Azure Media Services strujanje krajnje točke
**Upravljanje ključevima**|Nije potreban za implementaciju reference
**Upravljanje sadržajem**|C# program konzole

Drugim riječima, davatelja identiteta (IDP) i sigurne tokena servisa (STS) bit će Azure AD. Za reproduktora, koristit ćemo [Azure Media Player API -JA](http://amp.azure.net/libs/amp/latest/docs/). Servisa Azure Media Services i Azure Media Player podržava CRTICE i CENC s više DRM.

Sljedeći dijagram prikazuje cjelokupni strukture i tijek s iznad tehnologije mapiranje.

![CENC na AMS](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Da biste postavili dinamički CENC šifriranja, alat za upravljanje sadržajem će koristiti sljedeće unose:

1. Otvaranje sadržaja
1. Sadržaja ključ iz ključa generacije/upravljanja;
1. URL-ovi acquisition licence;
1. Popis informacije iz Azure AD.

Izlaz iz alata za upravljanje sadržajem će biti:

1. ContentKeyAuthorizationPolicy koja sadrži specifikacija na način isporuke licence potvrđuje JWT token i DRM specifikacije licence;
1. AssetDeliveryPolicy koja sadrži specifikacije streaming format, DRM zaštitu i URL-ovi za nabavu licence.

Tijekom izvođenja tijeka je kao ispod:

1. Nakon provjere autentičnosti korisnika JWT token generira;
1. Jedna od zahtjevima sadržani u JWT token jest "grupe" zahtjeva koji sadrži ID objekta grupe "EntitledUserGroup". U ovom zahtjeva će se koristiti za prosljeđivanje "prava potvrdite".
1. Reproduktora preuzimanja klijent manifest od na CENC zaštićeni sadržaj, a "vidi" sljedeće:
    1. ključ ID-a, 
    1. sadržaj je CENC zaštićena,
    1. URL-ovi acquisition licence.

1. Reproduktor čini zahtjev za nabavu licencu koji se temelji na podržani preglednik/DRM. U zahtjevu za nabavu licence ključa ID i JWT token će biti poslana. Servis za isporuku licence ćete provjeriti JWT token i zahtjevima nalaze prije pokretanja potrebne licence.

##<a name="implementation"></a>Implementacija

###<a name="implementation-procedures"></a>Postupci za implementaciju

Implementacije neće sadržavati sljedeće korake:

1. Priprema imovina test: kodiranje i paket probni videozapis da biste višestruki-brzina prijenosa fragmentirane MP4 u servisa Azure Media Services. U ovom resursa nije DRM zaštićen. Zaštita DRM će poduzeti dinamički zaštitu kasnije.
1. Stvaranje ključa ID i sadržaj ključa (po želji iz ključa Početni broj). Za naše svrhu sustava upravljanja ključem nije potrebna jer smo radite samo jedan skup ključa ID i sadržaja ključ za nekoliko imovine test;
1. Koristite AMS API-JA za konfiguriranje višestruki DRM licence isporukom za testiranje resursa. Ako koristite prilagođeni licence poslužitelja tako da vaše tvrtke ili dobavljače vaša tvrtka umjesto licence servisa Azure Media Services, možete preskočiti ovaj korak i navedite URL-ovi za nabavu licence u koraku za konfiguriranje isporuke licence. Da biste odredili neke detaljne konfiguracijama, kao što je ograničenje pravila za provjeru autentičnosti, predlošci odgovor licencu za različite servise DRM licence i itd., potrebno je AMS API-JA. Trenutačno Azure portal ne još nudi potrebne korisničkog Sučelja za tu konfiguraciju. Možete pronaći API razine informacija i poslušajte kod u dokumentu Julia Kornich: [pomoću PlayReady i/ili Widevine dinamički uobičajenih šifriranje](media-services-protect-with-drm.md). 
1. Koristite AMS API-JA za konfiguriranje pravilnika isporuke resursa za testiranje resursa. Možete pronaći API razine informacija i poslušajte kod u dokumentu Julia Kornich: [pomoću PlayReady i/ili Widevine dinamički uobičajenih šifriranje](media-services-protect-with-drm.md).
1. Stvaranje i konfiguriranje klijent za Azure Active Directory u Azure;
1. Stvaranje nekoliko korisničke račune i grupe u klijentu za Azure Active Directory: potrebno stvoriti barem "EntitledUser" Grupiranje i dodavanje korisnika u ovoj grupi. Korisnici u ovoj grupi proći kroz prava potvrdite u licence, a korisnici ne u ovoj grupi neće uspjeti za prosljeđivanje Provjera i nećete moći dobiti licencu. Članovi ove grupe "EntitledUser" je potrebna "grupe" zahtjeva u JWT token izdala Azure AD. Taj zahtjev zahtjeva moraju biti naveden u konfiguriranje višestruki DRM licence isporuke services korak.
1. Stvaranje aplikacije za ASP.NET MVC koji će se nalaze na videozapisa za reprodukciju. Ta aplikacija ASP.NET će biti zaštićene pomoću provjere autentičnosti korisnika u odnosu na klijentu Azure Active Directory. PROPER zahtjevima obuhvaćene pristupna tokena dobiti nakon provjere autentičnosti korisnika. Za ovaj korak, preporučuje se OpenID povezivanje API-JA. Morate instalirati sljedeća paketa NuGet:
    - Microsoft.Azure.ActiveDirectory.GraphClient instalaciju paketa
    - Microsoft.Owin.Security.OpenIdConnect instalaciju paketa
    - Microsoft.Owin.Security.Cookies instalaciju paketa
    - Microsoft.Owin.Host.SystemWeb instalaciju paketa
    - Microsoft.IdentityModel.Clients.ActiveDirectory instalaciju paketa
1. Stvaranje reproduktora pomoću [Azure Media Player API -JA](http://amp.azure.net/libs/amp/latest/docs/). [Azure Media Player ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) omogućuje vam da navedete tehnologiji koju DRM za korištenje na različite platforme DRM.
1. Matrica test:

**DRM**|**Preglednik**|**Rezultat za ovlašteni korisnika**|**Rezultat za nepotvrđen ovlašteni korisnika**
---|---|-----|---------
**PlayReady**|Web-mjesto MS rub ili u okvir za IE11 na Windows 10|Slijedi|Nije uspjelo
**Widevine**|Chrome na Windows 10|Slijedi|Nije uspjelo
**FairPlay** |TBD||

Goran Trifonov Azure Media Services tima sadrži napisali bloga pružaju detaljne upute u postavljanju Azure Active Directory za aplikaciju reproduktora ASP.NET MVC: [integrirati Azure Media Services OWIN MVC temelji aplikaciju pomoću servisa Azure Active Directory i ograničavanje isporuku sadržaja ključa koji se temelji na zahtjevima JWT](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

Goran sadrži napisan bloga na [JWT tokena provjere autentičnosti u web-mjesto servisa Azure Media Services i u okvir za dinamičku šifriranje](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/). I Evo njegovog [uzorka vezana uz integraciju Azure AD pomoću servisa Azure Media Services ključa isporuke](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Dodatne informacije o Azure Active Directory:

- Za razvojne inženjere informacije možete pronaći u [vodiču za Azure Active Directory programere](../active-directory/active-directory-developers-guide.md).
- Informacije za administratore možete pronaći u [Administriranje vaše Azure AD direktorija](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Neke preprekama u implementaciji

Postoje neka "preprekama" u implementaciji. Međutim sljedeći popis "preprekama" može vam pomoći u slučaju da naiđete na probleme za otklanjanje poteškoća.

1. **Izdavač** URL-a mora završavati **"/"**.  

    **Ciljne skupine** mora biti ID klijenta za aplikaciju reproduktora i trebali biste dodati i **"/"** na kraju URL izdavač.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" /> 

    U [JWT sadržaja drugog](http://jwt.calebb.net/), trebali biste vidjeti **aud** i **iss** kao ispod u JWT tokena:
    
    ![1st gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)


2. Dodavanje dozvola za aplikaciju u AAD (na kartici Konfiguriranje aplikacije). Ovo je obavezan za svaku aplikaciju (lokalni i distribuiranih verzije).

    ![gotcha 2.](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)


3. Koristite desno izdavač postaviti dinamičke CENC zaštite:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>
    
    Sljedeće će funkcionirati:
    
        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />
    
    GUID je ID klijenta AAD GUID pronaći ćete u skočnom krajnje točke Azure portalu.

4. Članstvo u grupi dodijelite zahtjeve ovlasti. Provjerite je li u datoteci manifesta za aplikaciju AAD, imamo sljedeće

    "groupMembershipClaims": "Sve" (Zadana je vrijednost null)

5. Postavljanje proper TokenType prilikom stvaranja preduvjeti ograničenja.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Nakon dodavanja podršku od JWT (AAD) uz SWT (ACS), zadane TokenType je TokenType.JWT. Ako koristite SWT/ACS, morate postaviti na TokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Dodatne teme za implementaciju
Zatim ćemo će disk uss neke dodatne teme u našem dizajna i implementaciju.

###<a name="http-or-https"></a>HTTP ili HTTPS?

Aplikacija reproduktora ASP.NET MVC smo u komponenti mora podržavati sljedeće:

1. Provjera autentičnosti korisnika putem Azure AD koja se treba nalaziti u odjeljku HTTPS;
1. JWT tokena exchange između klijenta i Azure AD koja se treba nalaziti u odjeljku HTTPS;
1. Dobivanje licence DRM klijenta koji je potreban za biti u odjeljku HTTPS ako je licenca isporuke nudi servisa Azure Media Services. Naravno, PlayReady proizvoda paketa ne prema HTTPS za isporuku licence. Ako je vaš poslužitelj licence PlayReady izvan servisa Azure Media Services, HTTP ili HTTPS ne može se koristiti.

Stoga ASP.NET aplikacija reproduktora HTTPS koristit će se kao preporučenim načinom rada. To znači da Azure Media Player će se prikazivati na stranici u odjeljku HTTPS. Međutim, za strujanje smo radije HTTP, dakle moramo treba uzeti u obzir kombiniranim sadržaja problem.

1. Preglednik ne dozvoljava kombinirani sadržaj. No dodaci kao što su Silverlight i OSMF dodatak prelazili putem glatke i CRTICU Dopusti. Kombinirani sadržaj je sigurnosni problem: to je zbog prijetnju mogućnost Ubaci zlonamjerni JS što može izazvati korisničkih podataka biti rizika.  Preglednici blokirati to prema zadanim postavkama, a zatim dosad je jedini način da biste zaobišli ga na strani poslužitelja (izvor) da biste omogućili sve domene (bez obzira na to https ili HTTP-a). To vjerojatno neće dobro je jedan od sljedećih načina.
1. Ne možemo izbjegavajte kombinirani sadržaj: ili oboje koristite HTTP ili oboje HTTPS. Kada se reproducira kombinirani sadržaj, silverlightSS Tehnički zahtijeva poništavanjem kombiniranim sadržaja upozorenje. Tehnički flashSS rukuje kombinirani sadržaj bez kombiniranim sadržaja upozorenja.
1. Ako je vaš strujanje krajnjoj točki stvorena prije kolovoz 2014., HTTPS ne podržava. U ovom slučaju, pročitajte stvaranje i korištenje krajnju točku strujanje za HTTPS.

U implementaciji referenca za DRM zaštićeni sadržaj, aplikacije i strujanje će se u odjeljku HTTTPS. Za otvaranje sadržaja reproduktora nije potrebna provjera autentičnosti ili licence, da biste imali liberty da biste koristili HTTP ili HTTPS.

### <a name="azure-active-directory-signing-key-rollover"></a>Potpisivanje ključa prilikom prelaska Azure Active Directory

To je važno točke uzeti u obzir implementaciju. Ako u svoju implementaciju ne preporučujemo to, dovršenih sistemskih naposljetku prestati raditi potpuno unutar najviše 6 tjedana.

Azure AD koristi standardnu industrijskih uspostaviti pouzdanosti između sa samim sobom i aplikacije koje koriste Azure AD. Konkretno, Azure AD koristi potpisivanja ključ koji se sastoji od javnim i privatnim ključ par. Kada Azure AD stvara sigurnosni token koji sadrži podatke o korisniku, ovaj token je potpisao Azure AD pomoću privatni ključ prije slanja natrag u aplikaciji. Token provjerite je li valjan i zapravo potječe od Azure AD, aplikacija morate provjeriti potpis na token pomoću javni ključ koji prikazuje Azure AD koji je sadržan u dokumentu metapodataka za vanjski pristup na klijentu. U ovom javni ključ – i tipku potpisa iz koje potječe – jednak je onim koji se koristi za sve klijenata u Azure AD.

Detaljne informacije o Azure AD ključa dinamične pronaći ćete u dokumentu: [Važne informacije o potpisivanja ključa prilikom prelaska u Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Između [javno privatni ključ par](https://login.windows.net/common/discovery/keys/) 

- Privatni ključ koristi Azure Active Directory za generiranje JWT token;
- Javni ključ koristi aplikacija kao što su DRM licence isporukom u AMS da biste provjerili JWT token;
 
Sigurnost svrhu Azure Active Directory zakreće certifikat povremeno (svakih 6 tjednima). U slučaju breaches sigurnost ključa dinamične može se pojaviti bilo kojem trenutku. Stoga isporukom licence u AMS morati ažurirati javni ključ koji se koristi kao Azure AD zakreće ključ, u suprotnom neće uspjeti tokena provjere autentičnosti u AMS i će izdati nema licencu. 

To možete postići tako da postavite TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument prilikom konfiguriranja DRM licence isporukom.

Tijek tokena JWT je kao ispod:

1.  Azure AD će izdati JWT token s trenutnom privatni ključ za korisnika čija je autentičnost provjerena;
2.  Kada je reproduktor vidi CENC sa sadržajem višestruki DRM zaštićen, najprije pronađite token JWT izdala Azure AD.
3.  Reproduktora šalje zahtjev za nabavu licence s JWT token isporukom i licence u AMS;
4.  Isporukom licence u AMS koristit će trenutni valjani javni ključ Azure AD da biste provjerili JWT token prije pokretanja licence.

DRM licence isporukom uvijek se provjerava trenutni valjani javni ključ Azure AD. Javni ključ izlaže Azure AD bit će ključa koji se koristi za potvrdu JWT token izdala Azure AD.

Što se događa ako ključa dinamične se događa nakon AAD generira JWT tokena, ali prije nego što se JWT token je poslao reproduktora kako bi DRM licence isporukom u AMS za potvrdu? 

Jer ključa može biti vraćeni u bilo kojem trenutku, uvijek više valjani ključ javno dostupna je u dokumentu metapodataka za vanjski pristup. Isporuka licencu za Azure Media Services možete koristiti bilo koji tipke koji je naveden u dokumentu, budući da jedan ključ može biti uskoro vraćeni, drugi mogu biti njegov zamjenski itd.

### <a name="where-is-the-access-token"></a>Gdje je pristup tokena?

Pogledajte kako web-aplikacijama poziva aplikaciju API-JA u odjeljku [Identitet aplikacije s OAuth 2.0 klijent vjerodajnice Grant](active-directory-authentication-scenarios.md#web-application-to-web-api)tijek provjere autentičnosti li kao ispod:

1.  Korisnik prijavljen je na Azure AD u web-aplikaciji (pogledajte [Web-preglednika u web-aplikaciju](active-directory-authentication-scenarios.md#web-browser-to-web-application).
2.  Krajnja točka za autorizaciju Azure AD preusmjerava korisnički agent natrag na klijentskoj aplikaciji pripadnog koda za autorizaciju. Korisnički agent vraća autorizacije kod klijentska aplikacija preusmjeravanje URI.
3.  Web-aplikaciju mora Nabava pristupnog tokena tako da mogu autentičnost API na webu i dohvaćanje željeni resursa. Čini zahtjeva za krajnje tokena Azure AD, koja omogućuje vjerodajnicu, ID klijenta i API-zadržavanja ID URI. Prikazuju se autorizacija kod da biste dokazali da korisnik ima pristanak.
4.  Azure AD potvrđuje aplikacije i vraća token za pristup JWT koji se koristi za poziv u API na webu.
5.  Web-aplikaciji putem HTTP, koristi vraćeni JWT token za pristup da biste dodali JWT niz s "Nošenja" oznaka u zaglavlju autorizacije zahtjeva za API na webu. API na webu pa Provjeri valjanost JWT token i ako Provjera valjanosti ne uspije, vraća željeni resursa.

U ovom tijek "identitet aplikacije" web API smatra pouzdanima web-aplikaciji provjere autentičnosti korisnika. Zbog toga ovaj uzorak zove pouzdani podsustav. [Dijagram na ovoj stranici](http://msdn.microsoft.com/library/azure/dn645542.aspx/) opisuje kako dodijeliti autorizacije kod tijek funkcionira.

U dobivanje licence s tokena ograničenja, ne možemo prate isti uzorak pouzdani podsustav. I servis za isporuku licence u servisa Azure Media Services web API resursa, "resursa pozadinskog" u web-aplikaciju treba pristupiti. Stoga gdje je token za pristup?

Uistinu, ne možemo su stjecanje token za pristup s Azure AD. Nakon provjere autentičnosti korisnika za uspješan, vraća se autorizacije kod. Kod autorizacije zatim koristi se zajedno s klijenta i ID- a aplikacije ključ, razmjenu token pristup. I token za pristup za pristup pokazivanjem na aplikaciju "pokazivač" ili koji predstavlja servis za isporuku licence servisa Azure Media Services.

Potrebna da biste registrirali i konfiguriranje aplikaciju "pokazivač" u Azure AD slijedeći korake u nastavku:

1.  U klijentu za Azure AD

    - Dodavanje aplikacije (resurs) s URL za prijavu: 

    https://[resource_name].azurewebsites.NET/ i 

    - ID URL-a aplikacije: 
    
    https://[aad_tenant_name].onmicrosoft.com/[resource_name]; 
2.  Dodavanje novog ključa za aplikaciju resursa
3.  Ažuriranje aplikacije manifesta datoteke tako da svojstvo groupMembershipClaims ima sljedeću vrijednost: "groupMembershipClaims": "Sve,"  
4.  U aplikaciji Azure AD koja pokazuje na web-aplikaciji reproduktora, u odjeljku "dozvole drugim aplikacijama" dodati aplikaciju resursa koja je dodana u korak 1. U odjeljku "dozvole delegiranog" potvrdite kvačicu "Pristup [resource_name]". Tako ćete dobiti dozvolu za stvaranje tokena pristup za pristup aplikaciji resursa za aplikaciju web. Trebali biste to za lokalni i distribuiranih verziju web-aplikacije ako razvijate i Visual Studio i Azure web app.
    
Stoga JWT token izdala Azure AD se uistinu token za pristup za pristup ovaj resurs "pokazivač".

### <a name="what-about-live-streaming"></a>Što je s Live strujanje?

U gornjem primjeru naš rasprave sadrži je usredotočite na imovinu na zahtjev. Što je s live strujanje?

Dobra je vijest koje možete koristiti točno istom dizajna i implementaciju za zaštitu uživo streaming u servisa Azure Media Services tretira resursa koji je povezan s programom kao "VOD resursa".

Konkretno, je dobro poznate da da biste učinili uživo strujanje u servisa Azure Media Services potrebnih za stvaranje kanala, a zatim program u odjeljku kanal. Da biste stvorili program, potrebnih za stvaranje sredstva koja će sadržavati uživo arhiva za program. Da bi pružio CENC zaštitu višestruki DRM aktivni sadržaj, sve ne morate učiniti, je da biste primijenili istu postavljanje/obrada na imovinu kao da je "VOD resursa" prije nego što počnete program.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Što je s poslužiteljima licence izvan servisa Azure Media Services?

Često korisnici možda imaju uložiti u licence farme poslužitelja u vlastite podatke centrirali ili hostira po DRM davateljima usluga. Srećom, zaštitu sadržaja servisa Azure Media omogućuje rad u načinu rada za hibridno: sadržaj hostira i dinamički zaštićene u servisa Azure Media Services dok DRM licence isporučuju se po poslužiteljima izvan servisa Azure Media Services. U ovom slučaju postoje Imajte na umu sljedeće promjene:

1. Tokena servis za sigurnu mora problema tokeni koje su vam prihvatljivi i možete provjeriti tako da na farmi poslužitelja licence. Na primjer, licencni poslužitelji Widevine nudi Axinom potreban je određeni token JWT koja sadrži "prava poruke". Dakle, morate koristiti programa STS izdavanje takve JWT token. Autori dovršite takve implementacija i detalje možete pronaći u sljedeći dokument u [Centru za Azure dokumentacija](https://azure.microsoft.com/documentation/): [Korištenje Axinom izlaganje Widevine licence servisa Azure Media Services](media-services-axinom-integration.md). 
1. Više ne morate konfigurirati servis za isporuku licence (ContentKeyAuthorizationPolicy) u web-servisa Azure Media Services. Što vam je potrebno učiniti jest licencu URL-ovi acquisition (za PlayReady, Widevine i FairPlay) kada konfigurirate AssetDeliveryPolicy prilikom postavljanja CENC više DRM.
 
### <a name="what-if-i-want-to-use-a-custom-sts"></a>Što ako želim koristiti prilagođene STS?

Može postojati nekoliko razloga zbog kojih klijenta možete koristiti prilagođene STS (tokena servis za sigurnu) za pružanje JWT tokeni. Neka od njih su:

1.  Na identiteta davatelja (IDP) koristi koje je korisnik odabrao ne podržava STS. U ovom slučaju prilagođene STS možda mogućnost.
2.  Klijent možda će biti potrebno bio prilagodljiviji i učiniti kontrolu u integriranje STS s pretplaćenih klijenta sustava za naplatu. Na primjer, MVPD operator može ponuditi više paketa pretplatnika OTT kao što su osnovni premium Sportovi, itd. Operator preporučuje se da odgovara zahtjevima u token s na pretplatnički paket tako da se samo sadržaj u desnom paket postane dostupna. U ovom slučaju prilagođene STS sadrži potrebne fleksibilnost i kontrolu.

Dva promjene potrebno izvršiti prilikom korištenja prilagođenih STS:

1.  Prilikom konfiguriranja servis za isporuku licencu za sredstvo, morate navesti sigurnosni ključ koji se koristi za potvrdu prilagođene STS (više detalja ispod) umjesto trenutni ključ iz servisa Azure Active Directory.
2.  Kada je generiran JTW tokena, sigurnosni ključ nije naveden umjesto privatni ključ trenutni X509 certifikata Azure Active Directory.

Postoje dvije vrste sigurnosni ključevi:

1.  Simetričnu ključ: tipku isti se koristi za generiranje i provjera JWT token;
2.  Asimetričnim ključ: par ključa javno Privatno u programa X509 certifikat služi privatnim ključem za šifriranje/generiranje JWT token i javni ključ za potvrdu token.

####<a name="tech-note"></a>Tehnički bilješke

Ako koristite .NET Framework / C# kao razvojne platforme, na X509 certifikat koji se koriste za asimetričnim sigurnosni ključ morate imati barem 2048 duljinu ključa. To je potrebna klase System.IdentityModel.Tokens.X509AsymmetricSecurityKey u .NET Framework. U suprotnom se će izbačena sljedeću iznimku:

IDX10630: 'System.IdentityModel.Tokens.X509AsymmetricSecurityKey' za potpisivanje ne može biti manji od '2048' bitova. 

## <a name="the-completed-system-and-test"></a>Dovršeni sustava i test

Ne možemo će voditi kroz nekoliko scenarija u sustavu dovršeni završetka do kraja tako da se čitatelji može imati basic "Slika" ponašanje prije nego prijeđete na račun za prijavu.

Web-aplikacije reproduktora i njegov prijave moguće je pronaći [u nastavku](https://openidconnectweb.azurewebsites.net/).

Ako je što vam je potrebno "koje nisu-integriran" scenarij: video resursi koji se nalazi u servisa Azure Media Services koji su ili Zaštita ili DRM zaštićen, ali bez tokena provjere autentičnosti (izdavanja licencu za obavi koje ga zahtijeva), ga možete testirati bez prijave (prebacivanjem HTTP ako videoprikaza strujanje putem HTTP-a).

Ako je što vam je potrebno završetka do kraja integrirani scenarij: video imovine je u odjeljku dinamički zaštita DRM u servisa Azure Media Services s tokena provjeru autentičnosti i JWT token generira Azure AD, morate prijaviti.

### <a name="user-login"></a>Korisničko ime za prijavu

Da biste testirali sustava integrirani DRM završetka do kraja, morate imati "račun" napravili ili dodali. 

Koji račun?

Iako Azure izvorno dopuštenje za pristup samo korisnici Microsoftov račun, sada omogućuje pristup korisnici iz oba sustava. To je učinjeno tako da sva Azure svojstva Vjeruj Azure AD za provjeru autentičnosti, imate Azure AD provjeru autentičnosti tvrtke ili ustanove korisnika i stvaranjem odnosa vanjski pristup gdje Azure AD smatra pouzdanima identitet sustava Microsoft potrošačkog računa za provjeru autentičnosti korisnika potrošača. Zbog toga Azure AD nije moguće provjeriti autentičnost "goste" Microsoftova računa, kao i "nativni" Azure AD račune.

Budući da Azure AD smatra pouzdanima domene Microsoftova računa (MSA), možete dodati svih računa iz bilo kojeg od sljedećih domene za prilagođenu Azure AD smjernice i pomoću računa za prijavu:

**Naziv domene**|**Domene**
-----------|----------
**Prilagođeno Azure AD klijentu domene**|somename.onmicrosoft.com
**Domene tvrtke**|Microsoft.com
**Domene Microsoftova računa (MSA)**|Outlook.com, live.com, hotmail.com

Možete se obratiti svih autora koji imaju račun napravili ili dodali umjesto vas. 

U nastavku su snimke zaslona različite prijava stranice koje koristi računi drugu domenu.

**Prilagođene Azure AD klijentskog domene računa**: U tom slučaju potražite na stranici prijava prilagođene prilagođena Azure AD klijentu domene.

![Račun za domenu Prilagođeno Azure AD klijenta](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Microsoftov račun domene pomoću pametne kartice**: U ovom slučaju vam se prikaže stranica za prijavu prilagođavati tvrtke Microsoft IT s provjerom autentičnosti dvofaktorska analiza varijance.

![Račun za domenu Prilagođeno Azure AD klijenta](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoftova računa (MSA)**: U ovom slučaju potražite na stranici prijava Microsoftov Account kupcima.

![Račun za domenu Prilagođeno Azure AD klijenta](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Pomoću proširenja šifrirane medijskih sadržaja za PlayReady

Na Moderna preglednika s šifrirane Media proširenja (EME) za podršku za PlayReady, kao što su preglednika Internet Explorer 11 sustavu Windows 8.1 i prema gore i preglednika Microsoft Edge na Windows 10 PlayReady bit će podlozi DRM za EME.

![Pomoću EME PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Područje tamno reproduktora je blokirale u zbog činjenice kojoj PlayReady zaštita sprječava jedan od strane snimke zaslona zaštićenog videozapisa. 

Na sljedećem zaslonu prikazuje reproduktora dodataka i MSE/EME podrška.

![Pomoću EME PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

EME u Microsoft Edge i 11 preglednika Internet Explorer u sustavu Windows 10 omogućuje pozivanje od [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) na Windows 10 uređajima koji podržavaju ga. PlayReady SL3000 ćete otključati tijek poboljšane premium sadržaja (4K, Zaglavlje, itd.) i nove modele isporuku sadržaja (Prijevremeni prozor za poboljšani sadržaj).

Usredotočite se na uređajima Windows: PlayReady je samo DRM u Hardverski dostupan na uređajima Windows (PlayReady SL3000). Servis za strujanje možete koristiti PlayReady putem EME ili UWP aplikacije i ponuditi Viša kvaliteta videozapisa pomoću PlayReady SL3000 od drugog DRM. Obično će sadržaj 2K tijeka kroz Chrome i Firefox, a 4K sadržaj preko ruba IE11 Microsoft ili UWP aplikacije na istom uređaju (ovisno o postavkama servisa i implementacija).


#### <a name="using-eme-for-widevine"></a>Pomoću EME za Widevine

Na Moderna preglednika uz EME/Widevine podršku, kao što je Chrome 41 + na Windows 10, Windows 8.1, Mac OSX Yosemite i Chrome na Android 4.4.4 Google Widevine je DRM iza EME.

![Pomoću EME za Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Obratite pozornost na to da Widevine ne sprječava jedan od strane snimke zaslona zaštićenog videozapisa.

![Pomoću EME za Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Ne ovlašteni korisnici

Ako korisnik nije član grupe "Korisnici pravo", korisnik će moći prolaz "prava potvrdite" i servis za licence višestruki DRM odbiti će izdati traženu licencu, kao što je prikazano u nastavku. Detaljan opis je "Nabava licence nije uspjela", koja je namijenjena.

![Nepotvrđen ovlašteni korisnici](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Pokretanje prilagođene tokena servis za sigurnu

Scenarija izvršenja prilagođene sigurne tokena servisa (STS) JWT token će izdao prilagođene STS pomoću simetričnu ili asimetričnim ključ. 

Slučaj upotrebe simetričnu ključ (Chrome):

![Pokretanje prilagođene STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

Slučaj upotrebe asimetričnim ključ putem programa X509 certifikata (pregledniku Microsoft moderni).

![Pokretanje prilagođene STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

U obje iznad slučajeva provjere autentičnosti korisnika ostaje na isti način – putem Azure AD. Jedina razlika je JWT tokeni su izdala prilagođene STS umjesto Azure AD. Naravno, prilikom konfiguriranja dinamičke CENC zaštitu ograničenja licence servis za isporuku navodi vrstu tokena JWT simetričnu ili asimetričnim ključ.

## <a name="summary"></a>Sažetak

U ovom dokumentu, ne možemo spominju CENC s kontrolom višestruka-native-DRM i pristup putem tokena provjere autentičnosti: dizajn i njegova implementacija pomoću Azure, servisa Azure Media Services i Azure Media Player.

- Referenca dizajn raspoređene koja sadrži sve potrebne komponente u DRM/CENC podsustav;
- Implementacija referenca na Azure, servisa Azure Media Services i Azure Media Player.
- Neke teme izravno uvrštene u dizajnu i implementaciju također se spominju.


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Potvrda 

William Zhang, Mingfei Yan, franak u Roland, Kilroy Hughes, Julia Kornich
