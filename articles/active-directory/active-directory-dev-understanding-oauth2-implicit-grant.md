<properties
   pageTitle="Objašnjenje OAuth2 implicitno dodijeliti tijek servisa Azure Active Directory | Microsoft Azure"
   description="Saznajte više o Azure Active Directory o implementaciji sustava OAuth2 implicitno dodijeliti protok i je li to najbolje odgovara aplikacije."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Objašnjenje implicitno grant tijek OAuth2 u Azure Active Directory (AD)

Dodjela implicitno OAuth2 je notorious za dopuštanje najdulje popisom problemima sigurnosti u specifikacija OAuth2. I još pristup ADAL JS i jedan preporučujemo da se prilikom pisanja SPA aplikacije. Što omogućuje? To je sve pitanje tradeoffs: i kao Pretvori, implicitno grant je najbolji način možete vezi primjene dijagnosticiranja za aplikacije koje API Web putem JavaScript u pregledniku.

## <a name="what-is-the-oauth2-implicit-grant"></a>Što je Dodjela implicitno OAuth2?

Quintessential [dodijeliti OAuth2 autorizacije kod](https://tools.ietf.org/html/rfc6749#section-1.3.1) je Dodjela autorizacije koji koristi dvije zasebne krajnje točke. Krajnja točka za autorizaciju koristi se za faze interakcija korisnika, zbog čega je kod autorizacije. Tokena krajnje točke pa koristi klijent za razmjenu šifru token za pristup i često kao i token osvježavanja. Web-aplikacije su potrebni za predstavljanje vlastite aplikacije vjerodajnice za krajnju točku tokena tako da se provjeru autentičnosti poslužitelja možete provjeriti autentičnost klijent.

[Dodjela OAuth2 implicitno](https://tools.ietf.org/html/rfc6749#section-1.3.2) je varijanta dopuštanja klijent da biste dobili token pristup (i id_token slučaju [OpenId povezivanje](http://openid.net/specs/openid-connect-core-1_0.html)) izravno iz krajnju točku autorizacije bez uspostavljanje veze krajnju točku tokena ni provjere autentičnosti klijentske aplikacije. Tu varijantu posebno dizajnirane za JavaScript temelji aplikacije koje rade u web-pregledniku: u izvornom specifikacija OAuth2 tokeni vraćaju se u URI fragment. Koje čini tokena bita dostupnim JavaScript kod u klijentu, ali ga se jamčiti neće biti uključen u preusmjeravanja prema poslužitelj. Vraćanje tokeni putem preglednika preusmjerava izravno iz krajnja točka za autorizaciju. Ima prednost eliminiranju sve zahtjeve za unakrsni polazište poziva, koje su potrebne ako JavaScript aplikacije potreban je da se obratite tokena krajnjoj točki.

Važno osobine grant implicitno OAuth2 je fact kao što su teče nikad povrata osvježavanja tokeni klijentu. Ne možemo će potražite u sljedećem odjeljku koji nije doista potreban i zapravo bio sa sigurnošću.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Odgovarajuću scenariji za dopuštanje implicitno OAuth2

Kao specifikacija OAuth2 sam deklarira, implicitno grant sadrži je Smislio da biste omogućili korisnički agent aplikacije – što znači, JavaScript aplikacije izvršavanja preglednika. Definiranje osobine takve aplikacije je JavaScript kod služi za pristup resurse poslužitelja (obično Web API) i ažuriranje aplikacije UX sukladno tome. Zamislite aplikacije kao što su Gmail ili Outlook Web Access: kad odaberete poruke u mapi Ulazna pošta, samo na ploči vizualizacije poruke promijeni tako da prikazuje novi odabir dok nepromijenjenu ostatak stranice. Ovo je in contrast with tradicionalni temelje za preusmjeravanje web-aplikacijama gdje svaki interakcije s korisnikom rezultira preko cijele stranice poslužitelj i renderiranje cijeloj stranici novi odgovor poslužitelja.

Aplikacijama koje koriste pristup JavaScript koji se temelji na njegov ekstremne nazivaju jednu stranicu aplikacije ili SPAs: je ideja koje te aplikacije služi samo početnu stranicu HTML i pridruženi JavaScript, uz sve kasnije interakcije koji se pokreću Web API poziva izvršiti putem JavaScript. Međutim, hibridnog pristupa aplikacije kojima je uglavnom utemeljenih na poslužitelj, ali izvodi Povremeni JS pozive, nisu neuobičajeno – raspravu o korištenju implicitno tijek vrijedi za one kao i.

Aplikacije utemeljene na preusmjeravanje obično sigurne njihove zahtjeva putem kolačića, međutim, pristup funkcioniraju kao i za aplikacije JavaScript. Kolačići funkcioniraju samo protiv domene oni imaju generiran, tijekom poziva JavaScript možda usmjerena drugih domena. Zapravo, koji će često biti slučaj: zamislite aplikacije pozivanje API Azure Office API Microsoft Graph API – sve residing izvan domene s kojima je poslužena aplikacije. Sve veći trend za aplikacije JavaScript je imati bez pozadinskog uopće potrebe za oslanjanjem 100% na 3 strana Web API-ji za implementaciju njihove funkcije tvrtke.

Trenutno željeni način za zaštitu poziva Web API je za korištenje tokena pristup nošenja OAuth2 gdje se svaki poziv praćeni pristupnog tokena OAuth2. Web API pregledava dolazne token za pristup i, ako se pronađe u njemu potrebno dosega, dopušta pristup traženi postupak. Implicitno tijek omogućuje praktično mehanizam za JavaScript aplikacije da biste dobili pristup tokeni za API na Webu koja nudi brojne prednosti u odnosu na Kolačići:

- Tokeni moguće pouzdano dobiti bez bilo koje morate za unakrsni polazište pozive – obavezna Registracija preusmjeravanje URI na koji se tokeni povrata jamčiti se ne displaced tokena
- JavaScript aplikacija možete nabaviti potrebna, za onoliko Web API-ji su ciljani – s bez ograničenja na domena proizvoljan broj pristupna tokena
- HTML5 značajke kao što su sesiju ili lokalno spremište dodijeliti potpunu kontrolu nad tokena predmemoriranje i upravljanje vijek, dok je upravljanje kolačići neprozirne aplikaciju
- Pristupna tokena nisu susceptible od napada krivotvorina (CSRF) zahtjev za web-mjesta

Implicitno grant tijek problema osvježavanje tokena, uglavnom sigurnosnih vam razloga. Osvježavanje token kao narrowly nije ograničena kao pristupna tokena dodjelu znatno više energije dakle inflicting znatno više štetu u slučaju da ga je leaked. U implicitno tijek tokeni isporučuju se u URL-u, dakle rizika presretanja veće od kod grant autorizacije.

Međutim, imajte na umu da JavaScript aplikacije na njegov rashoda za obnavljanje pristupna tokena bez pritišćite pitanja korisnika za vjerodajnice ima drugi mehanizam. Aplikaciju možete koristiti skrivene iframe za izvođenje nove zahtjeve za tokena protiv autorizacije krajnjoj točci Azure AD: pod uvjetom da u pregledniku i dalje ima aktivne sesije (Pročitajte: sadrži kolačić sesije) protiv domene Azure AD zahtjev za provjeru autentičnosti uspješno javlja se bez potrebe sve interakcije s korisnikom. 

Ovaj model daje aplikacija JavaScript mogućnost neovisno obnoviti pristupna tokena i čak i Nabava nove za nove API (pod uvjetom da korisnik prethodno pristanak za njih. Taj se način izbjegavaju dodane teret pri dohvaćanju, održavanje i zaštita artefakt visoke vrijednosti kao što su token Osvježi. Artefakt koja omogućuje tihu obnavljanje, sesiju kolačić Azure AD upravlja izvan aplikacija. Prednost takvog je korisnik može Odjava iz Azure AD pomoću bilo koje aplikacije prijavili Azure AD koji se izvodi u bilo kojem od kartice preglednika. Rezultat te brisanje kolačića sesije Azure AD, a aplikacija JavaScript automatski će se izgubiti mogućnost obnoviti tokena traje odgovor.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Je li implicitno grant prikladna za aplikacije?

Implicitno grant predstavlja više rizika od drugih daje. Područja morate obratite pažnju na su dobro navedenih (pogledajte primjer [Zloupotreba programa Access tokena vlasniku oponašati resursa u implicitno tijeka] [ OAuth2-Spec-Implicit-Misuse] i [OAuth 2.0 prijetnju modela i sigurnosna pitanja vezana uz][OAuth2-Threat-Model-And-Security-Implications]). Međutim, veća profila rizika je uglavnom blokirale u zbog činjenice u kojoj je namijenjena da biste omogućili aplikacije koje izvršavanje aktivnog kod posluživanje sustavom udaljene resursa u pregledniku. Ako namjeravate Arhitektura programa SPA bez pozadinski komponente ili namjeravate pozivanje API Web putem JavaScript, preporučuje se korištenje implicitno tijek za nabavu tokena.

Ako je vaša aplikacija nativni klijent, implicitno tijek nije sjajno Prilagodi. Izostanak kolačića u kontekstu nativni klijent za sesiju Azure AD deprives aplikacije od srednje vrijednosti održavanje dugo lived sesiju. Što znači aplikacije tražit će više puta od korisnika, prilikom dohvaćanja pristupna tokena za nove resurse.

Ako razvijate web-aplikacije koja obuhvaća s pozadinskom i potrošnje API-JA s njegov kod pozadinski implicitno tijek nije ni dobro rješenje. Ostale daje dati znatno više energije. Na primjer, grant vjerodajnice za klijenta OAuth2 nudi mogućnost da biste dobili tokeni koje odražavaju dozvole dodijeljene na same aplikacije, umjesto delegations korisnika. To znači da klijent ima mogućnost da biste zadržali programskog pristupa resursima čak i kada korisnik ne aktivno aktivan u sesiju i tako dalje. Ne samo one, ali takve daje nudi veću sigurnost jamstva. Ako, primjerice, pristup tokeni nikad prijenosa putem preglednika korisnika, oni ne rizikom spremaju u povijesti preglednika i tako dalje. Klijentska aplikacija možete izrađivati Jaka provjera autentičnosti prilikom traženja token.

## <a name="next-steps"></a>Daljnji koraci

- Popis resursa za razvojne inženjere, uključujući referentne informacije za protokoli i OAuth2 autorizacije dodijeliti tokova podršku tako da Azure AD, pogledajte [Azure AD programere vodič][AAD-Developers-Guide]
- Saznajte [kako integrirati aplikacije s Azure AD]  [ ACOM-How-To-Integrate] za dodatne dubinske na postupka Integracija aplikacija.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16 
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

