<properties 
   pageTitle="Azure mobilne radnje vodič – SDK za otklanjanje poteškoća" 
   description="Otklanjanje problema s SDK integracija u Azure Mobile radnje" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Vodič za probleme Integracija SDK za otklanjanje poteškoća

Slijedi popis mogućih problema koji se mogu pojaviti s kako će se radnje Mobile Azure integrira u aplikaciji.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>Problemi s SDK otkriti pogreška u drugom području aplikacije

### <a name="issue"></a>Problem
- Pogreška zbirke podataka korisničkog Sučelja (u analize, nadzor, segmente ili nadzornih ploča).
- Automatske pogreške (ih gura ne funkcioniraju u aplikaciji aplikacije ili i jedno i drugo).
- Dodatne značajke pogreške (praćenja, geolokacija-a ili platformu određene ih gura ne funkcioniraju).
- API pogreške (API-ji nije uspjelo često tihu bez poruke o pogreškama).
- Otkazivanja servisa (ništa Azure Mobile radnje radi aplikacije).

### <a name="causes"></a>Uzroci

- Većina problemi koje morate riješiti radnje SDK za Azure mobilni će može otkriti pogreške u aplikaciji (primjerice korisničko Sučelje nije uspjelo zbirke podataka, automatske pogreška, pogreška napredne značajke, API pogreške, ruši aplikacije ili vidljivu servisa prekida).  
- Ako u svojoj aplikaciji prije nikad ne radi određene značajke Azure Mobile radnje, morat ćete dovršiti integraciju sa servisom. 
- Ako određene značajke Azure Mobile radnje je rad i zaustaviti, morate nadograditi do zadnje verzije s radnje SDK za Azure Mobile. Imajte na umu da postoji neku drugu verziju programa Azure Mobile radnje SDK-a za svaki platformu podržava Azure Mobile radnje (Android, iOS, Windows i Windows Phone).

#### <a name="sdk-integration"></a>Integracija SDK

- Azure radnje Mobile nije pravilno integriran u SDK (analize).
- Dosegne nisu ispravno integriran u SDK (u aplikaciji i o ih gura aplikacije).
- Potvrda istekle ili nisu ispravne RN nasuprot Razvojni (samo za iOS).
- GCM ili Admin nisu ispravno integriran u SDK (Android samo – servis određene ih gura).
- Praćenje nisu ispravno integriran u SDK (Instaliraj trgovina praćenja).
- Drži mjesto ili mjesto GPS nisu ispravno integriran u SDK (Targeting po zemlj lokaciji).


**Vidi također:**

- [Dokumentacija SDK - Integracija vodilice][Link 5] 
- [Vodiča za otklanjanje pogrešaka - pribadače][Link 23]

#### <a name="sdk-upgrade"></a>Nadogradnja SDK

- Morate nadograditi SDK za rješavanje problema sa starijim verzijama programa SDK (često odnose se na noviju verziju uređaj s operacijskim Sustavom).
- Deinstalirajte sve starije verzije aplikacije s uređaja, a zatim ponovno instalirajte najnoviju verziju aplikacije, ponovno registrirajte uređaj ID-a iz Azure Mobile radnje korisničkog Sučelja da biste potvrdili da vaš uređaj koristi najnoviju verziju aplikacije.

**Vidi također:**

- [Dokumentacija SDK - napomene](http://go.microsoft.com/fwlink/?LinkId= 525554) 
- [Dokumentacija SDK - nadogradnje vodilice](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>SDK druge

- Pogreške u manifestu aplikacije "AndroidManifest.xml" mogu prouzročiti Azure Mobile radnje raditi (samo za Android).
- Uobičajeni problem s SDK integracije i način korištenja API je zbuniti ključ za SDK i ključ za API-JA.

**Vidi također:**

- [Pojmovi - Pojmovnik][Link 6]

## <a name="advanced-coding-issues"></a>Napredno kodiranje problemi

### <a name="issue"></a>Problem
-  Kod određene platformu nisu izravno povezani s mogućnostima Mobile Azure može izazvati probleme sa sustavima iOS, Android i Windows Phone.

### <a name="causes"></a>Uzroci

- Mnoge napredne kodiranje probleme s mogućnostima Mobile Azure je uzrokovano pogrešno napisanih platformu određene kod koji nisu izravno povezani s Azure Mobile radnje. Morat ćete pogledajte dokumentaciju specifične za platformu razvijate za osim Azure Mobile radnje dokumentaciju (Android, iOS, Web, Windows i Windows Phone).
- Nije pravilno konfiguriranja "Kategorija", onemogućuje povezivanje obavijest na drugo mjesto unutar ili izvan aplikaciju (samo za Android). 
- Ne postavka "UIKit.framework" za "neobavezno" u kodu iOS prikazuje "Symbol nije pronađen pogreške" i/ili zatvara se prilikom starije uređaje sa sustavom iOS (samo za iOS).
- Istekla potvrda ili nije pravilno pomoću verzije Razvojni ili grupa certifikata, uzroci automatske problemi (samo za iOS).
- Postoje određena ograničenja ugrađeno u platformu Azure Mobile radnje ne možete kontrolirati (kao što je centar za sustav funkcioniranje za iz aplikacije ih gura u Android i iOS).
- Azure Mobile radnje objavljuje cijeli popis interne paketa koriste Azure Mobile radnje za iOS i Android za referencu. Imajte na umu da su neke značajke Azure Mobile radnje specifične za platformu (Android, iOS, Web, Windows i Windows Phone).

### <a name="see-also"></a>Vidi također

 - [Vodiča za otklanjanje pogrešaka - pribadače][Link 23] 
 - [Dokumentacija SDK - napomene][Link 5]
 - [Dokumentacija SDK - nadogradnje vodilice][Link 5]

## <a name="application-crashes"></a>Ruši aplikacije

### <a name="issue"></a>Problem
- Zatvara se aplikacija na uređaju krajnjim korisnicima.

### <a name="causes"></a>Uzroci

- Informacije rušenje moguće je prikazati u *Analize korisničkog Sučelja* ili *Analize API -JA*
- Možete pronaći ID uređaja test uređaja i poduzmite iste akciju koje su uzrokovale aplikacije pad za krajnjeg korisnika koji će pomoći u prepoznavanju uzroka vaše pad sustava.
- Poznati problemi vezani uz radnje SDK za Azure Mobile koji uzrokuju aplikacije rušiti ponekad su razriješiti tako da na najnoviju verziju SDK. Provjerite je li provjera napomene o svoju platformu istražuje ruši.

### <a name="see-also"></a>Vidi također

- [Dokumentacija SDK - napomene][Link 5]
- [Dokumentacija SDK - nadogradnje vodilice][Link 5]

## <a name="app-store-upload-failures"></a>Aplikacije iz trgovine prijenos pogrešaka

### <a name="issue"></a>Problem
- Pogreške vezane uz prijenos najnoviju verziju aplikacije za Apple, Google ili Windows trgovine.

### <a name="causes"></a>Uzroci

- Aplikacija pohranjuje ponekad aplikacije blok s određene značajke omogućene (npr. trgovinu Apple onemogućuje korištenje IDFV u aplikacije u trgovini i u trgovini GooglePlay onemogućuje zajedničko korištenje aplikacije informacija između aplikacija). 
- Obavezno provjerite napomene o platforme i trenutnu verziju SDK ako imate poteškoća s prijenosom aplikacije u trgovini.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
