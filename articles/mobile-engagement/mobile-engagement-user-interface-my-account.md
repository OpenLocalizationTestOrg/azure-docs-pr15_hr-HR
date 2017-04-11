<properties 
   pageTitle="Azure mobilne radnje korisničko sučelje - moj račun" 
   description="Saznajte kako možete upravljati računom profil i testiranje pomoću Azure Mobile radnje" 
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

# <a name="how-to-manage-your-account-profile-and-test-devices"></a>Kako upravljati računom profil i test
 
U ovom se članku opisuju stranici **Početna stranica** portala za **Mobilne radnje** . Pomoću portala za **Mobilne radnje** za nadzor i upravljanje mobilne aplikacije. 
 
Da biste došli do stranice **moj račun** , kliknite svoj račun pri vrhu stranice.

U odjeljku moj račun korisničkog sučelja je gdje možete pogledati i promijeniti postavke povezan s vašim računom, uključujući postavki profila i testiranje uređaju ID-a. Ove postavke sadrže stavke koje se također može pristupiti putem uređaja API-JA.

![MyAccount1][7]  

## <a name="profile"></a>Profil:
Možete prikazati ili promijenite postavke svog računa prikazano u nastavku. Možete dati i neki drugi korisnik dozvolu za korištenje aplikacije koji se temelji na adresu e-pošte na [Početak](mobile-engagement-user-interface-home.md).

![MyAccount2][8]  

## <a name="devices"></a>Uređaji:
Prikaz, dodavanje i uklanjanje testiranje uređaj ID's uređaja za testiranje koje možete koristiti da biste testirali **dosegne** ili **automatske** kampanja. Kontekstna upute za pronalaženje ID uređaja uređaja za svaki platformu (iOS i Android, Windows Phone, itd.) se prikazuju kada kliknete "Novi uređaj". 
 
![MyAccount3][9]  
 
Da biste koristili automatske API-JA ili uređaj API što trebate znati vaših korisnika Jedinstveni identifikator uređaja (parametar deviceid). Da biste je dohvatiti na nekoliko načina:
 
1. Iz pozadine, pomoću značajke "Dohvati" od API uređaj da biste dobili cijeli popis identifikatora na uređaj.
2. Iz aplikacije programa da biste ga možete koristiti SDK-a. (Na Android poziva funkciju getDeviceID() klase Agent, a zatim na iOS, čitati svojstvo deviceid klase Agent).
3. Iz razgovor objava, ako je URL akcije pridružene objava sadrži uzorak {deviceid} je bit će automatski zamijenjene identifikator uređaja za pokretanje akcije.
http://<example>.com/registeruser?deviceid = {deviceid} & otherparam = myparamdata bit će zamijenjene: http://<example>.com/registeruser?deviceid = XXXXXXXXXXXXXXXX & otherparam = myparamdata 
4. Iz objava web razgovor, ako HTML kod priopćenja sadrži uzorak {deviceid} je bit će automatski zamijenjene identifikator uređaja prikaz objava web.
Ovo je moj identifikator uređaja: {deviceid} bit će zamijenjene: Ovo je moj identifikator uređaja: XXXXXXXXXXXXXXXX
5.  Otvorite aplikacije na uređaju, a zatim izvedite događaja u aplikaciji koji je označen.
Na "Detalji UI - svoje događaje aplikacije - Monitor - -", pronaći događaja koji se izvode na popisu.
Kliknite da biste taj događaj na monitoru.
Trebali biste pronaći identifikacijskog Broja za uređaj na popisu uređaje koje ste obavili ovaj događaj.
Nakon toga možete kopirati ovaj uređaj ID i registrirati u na "UI - moj račun - uređaji – novi uređaj – odaberite svoju platformu uređaj".
>(Imajte na umu da kada IDFA onemogućen je za iOS, ID uređaja mijenjaju tijekom vremena ako deinstalaciju i instalaciju pokrenite aplikaciju.)

##<a name="troubleshooting-guide"></a>Vodič za otklanjanje poteškoća
-  [Vodiča za otklanjanje pogrešaka - usluge][Link 24]

## <a name="see-also"></a>Vidi također
-  [Korisničko Sučelje dokumentaciju – Početna stranica][Link 13]


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


 
 
