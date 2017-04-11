<properties 
   pageTitle="Azure mobilne radnje korisničko sučelje – postavke" 
   description="Informirajte se o upravljanju globalnih postavki aplikacije pomoću Azure Mobile radnje" 
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

# <a name="how-to-manage-the-global-settings-of-your-application"></a>Kako upravljati globalnih postavki aplikacije

**Postavke** izbornika mogućnosti dostupne za aplikacije razlikovanje, ovisno o platformu aplikacije i dozvola koje ste dobili za aplikaciju. Postavke obuhvaćaju sljedeće: pojedinosti, projektima, nativni automatske, automatske brzinom, oznake (aplikacija informacije) i komercijalne tlaka. Mogućnost izbornika (aplikacija informacije) oznaku odjeljka postavke upravlja aplikacije (pomoću SDK-a) ili svoje pozadinskog (pomoću uređaja API-JA). 


>[AZURE.NOTE] Broj sekcija portala za **Mobilne radnje** korisničkog Sučelja sadrže gumb **PRIKAŽI pomoć** . Pritisnite taj gumb da biste saznali više kontekstne o sekcije.

## <a name="details"></a>Pojedinosti

Omogućuje vam da biste promijenili naziv i opis aplikacije, prikaz vlasnika vaše aplikacije i dozvole za uloge. 

Konfiguriranje analize omogućuje vam da biste pogledali ili promijenili početak tjedna na dan i vrijeme zadržavanja dana.
 
  ![settings1][46]
 
## <a name="projects"></a>Projekti

Omogućuje vam da biste odabrali sve projekte želite da se aplikacija prikaže u. 

Traženje projekta i prikaz naziv, opis, vlasnika i dozvole za uloge sve projekta aplikacija je dio.

Dodatne informacije potražite u članku: [Korisničko Sučelje dokumentaciju – Početna stranica][Link 13]
 
  ![settings3][48]

## <a name="native-push"></a>Izvorni pribadače

Omogućuje vam da biste registrirali novi certifikat ili Izbriši i postojeći certifikat za korištenje s nativni pribadače. Izvorni automatske omogućuje Azure Mobile radnje da biste primijenili na vaše aplikacije u bilo kojem trenutku, čak i ako nije pokrenut. 

Nakon unošenja vjerodajnice ili potvrde barem jedan nativni automatske servisa, možete odabrati "Uvijek" pri stvaranju dosegne kampanje i koristite parametar "obavijesti" u API AUTOMATSKE.



### <a name="apple-push-notification-service-apns"></a>Servis za Apple automatske obavijesti (APN-ove)

Da biste omogućili nativni automatske putem servisa Apple automatske obavijesti morate registrirati certifikata. Morat ćete odrediti vrstu certifikata kao razvoj (Razvojni) ili proizvodnje (grupa). Zatim će potrebno prenesete i lozinka za potvrdu.

Dodatne informacije potražite u članku: [SDK dokumentaciji – iOS – kako pripremiti aplikacije Apple automatske obavijesti][Link 5]
 
![settings4][49]
 
### <a name="windows-push-notification-service-wpns"></a>Servis automatske obavijesti u sustavu Windows (WPNS)

Da biste omogućili nativni automatske putem servisa obavijesti za Windows, morate unijeti vjerodajnice za vaše aplikacije. Trebat će vam paketa sigurnosni identifikator (SID) i tajnu ključ.
 
![settings5][50]
 
### <a name="google-cloud-messaging-for-android-gcm"></a>Google Cloud poruka za Android (GCM)

Da biste omogućili korištenje GCM nativni automatske, morate pomoću uputa iz Google. Zatim morate zalijepiti poslužitelja jednostavne API ključa konfiguriran bez ograničenja IP. Integracija s SDK-a potreban za Android v1.12.0 +.

Dodatne informacije potražite u članku: 

- [Android dokumentaciju SDK integraciji GCM][Link 5]
- [Vodič za GCM Google za razvojne inženjere](http://developer.android.com/guide/google/gcm/gs.html)
 
### <a name="amazon-device-messaging-for-android-adm"></a>Amazon uređaj poruka za Android (Admin)

Da biste omogućili korištenje Admin nativni automatske, morate unijeti Amazon <OAuth credentials> sastoji se od ID klijenta i tajna klijenta (zahtijeva Integracija s SDK za Android v2.1.0 +).

Dodatne informacije potražite u članku: 

- [Android dokumentaciju SDK integraciji Admin][Link 5]
- [Dokumentacija za Admin Amazon za razvojne inženjere](https://developer.amazon.com/sdk/adm/credentials.html#Getting)
 
![settings6][51]

## <a name="push-speed"></a>Automatske brzine

Prikazuje trenutni automatske brzinu aplikacije i omogućuje da odredite brzinu automatske aplikacije.
 
  ![settings7][52]

## <a name="tag-app-info"></a>Oznaka (aplikacija informacije)

![settings11][56]
  
## <a name="commercial-pressure"></a>Komercijalni tlaka


![settings12][57]


## <a name="see-also"></a>Vidi također

- [Koncepti][Link 6]
- [Vodič za servis za otklanjanje poteškoća][Link 24]

 

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
