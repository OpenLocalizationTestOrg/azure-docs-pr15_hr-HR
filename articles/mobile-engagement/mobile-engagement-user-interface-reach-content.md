<properties 
   pageTitle="Azure mobilne radnje korisničko sučelje - sadržaj razgovor" 
   description="Informirajte se o upravljanju jedinstveni sadržaj različitih vrsta automatske obavijesti kampanje u Azure Mobile radnje" 
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

# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a>Kako upravljati jedinstveni sadržaj različitih vrsta kampanje automatske obavijesti
 
Sekciji sadržaj nove kampanje razgovor možete koristiti da biste izmijenili sadržaj vaše objave, ankete, ih gura podataka i pločice (samo za Windows Phone). Postavku sadržaja automatske kampanja odgovara vrsti kampanje. 
 
### <a name="content-types"></a>Vrste sadržaja:
- Objava
- Ankete
- Ih gura podataka
- Pločice (samo za Windows Phone)
 
## <a name="content-of-announcements"></a>Sadržaj objave
 ![Razgovor Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a>Odaberite vrstu vaše objave:
-    Samo obavijest: je jednostavno standardna obavijest. Što znači da ako korisnik klikne, pojavit će se bez dodatnog prikaza, ali će se izvršiti samo akcija koja je povezana s njom.
-    Objava teksta: je obavijest o engages da korisnik ima susret prikaza teksta.
-    Objava na webu: je obavijest o engages da korisnik ima potražite na web-prikaz.

### <a name="see-also"></a>Vidi također
- [Dosegne – kako Tos - objave][Link 3] 

### <a name="about-web-view-announcements"></a>O Web prikaz objava:
Pojava uzorak "{deviceid}" u HTML koda ili JavaScript koda ovdje unesete automatski zamjenjuju se identifikator uređaja prikaz objava. To je jednostavan je način za dohvaćanje radnje Azure mobilni uređaj identifikatora u servis za vanjskog web koji se nalaze na pozadine sustava office.
Ako želite stvoriti web-prikaz preko cijelog zaslona (bez zadanu akciju izlaz gumbe i dajemo) možete koristiti sljedeće funkcije iz vaše web prikaz objava JavaScript kod: 

-    Izvođenje akcije objave: ReachContent.actionContent()
-    Izlaz iz priopćenja: ReachContent.exitContent()
 
### <a name="choose-your-action"></a>Odabir radnje:

### <a name="about-action-urls"></a>O URL-ova akcija:
Bilo koji URL koji je moguće ga je protumačiti ciljano uređaj operacijski sustav može se koristiti kao URL akcije.
Bilo koji namjenski URL koji se možda podržava aplikacija (npr. da bi korisnici prešli na određeni zaslon) mogu se koristiti kao URL akcije.
Svako pojavljivanje uzorak {deviceid} automatski zamijenjen je identifikator uređaja koji se izvodi akciju. To se može koristiti da biste jednostavno dohvatiti radnje Azure mobilni uređaj identifikatora putem servis za vanjske web koji se nalazi na vašem pozadinskih.

- **Android + iOS akcije**
    - Otvorite web-stranicu
    - http://\[web, web-mjesta i domene\] 
    - Primjer: http://www.azure.com
    - Slanje poruke e-pošte
    - mailto:\[e-pošte primatelja\]?subject =\[predmet\]& tijelo =\[poruke\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Slanje u SMS PORUKE
    - SMS:\[broj telefona\] 
    - Primjer: sms:2125551212
    - Biranje telefonskog broja
    - Tel:\[broj telefona\] 
    - Primjer: tel:2125551212
- **Android samo akcije**
    - Preuzimanje aplikacije u trgovini Play
    - market://details?id=\[paketa aplikacije\] 
    - Primjer: market://details?id=com.microsoft.office.word
    - Početak pretraživanja lokalizirani za zemlj.
    - GEO:0, 0?q =\[upit za pretraživanje\] 
    - Primjer: geo:0, 0? pitanja = starbucks, Pariz
- **samo akcije za iOS**
    - Preuzimanje aplikacije u trgovini App Store
    - /id /app/ [naziv aplikacije] http://iTunes.Apple.com/ [Država] [id aplikacije]? mt = 8 
    - Primjer: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
    - Akcije za Windows
    - Otvorite web-stranicu
    - http://\[web, web-mjesta i domene\] 
    - Primjer: http://www.azure.com
    - Slanje poruke e-pošte
    - mailto:\[e-pošte primatelja\]?subject =\[predmet\]& tijelo =\[poruke\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Slanje SMS (Skype trgovine obavezno)
    - SMS:\[broj telefona\] 
    - Primjer: sms:2125551212
    - Birati telefonski broj (Skype trgovine obavezno)
    - Tel:\[broj telefona\] 
    - Primjer: tel:2125551212
    - Preuzimanje aplikacije u trgovini Play
    - MS-windows-iz trgovine: PDP?PFN =\[ID paketa aplikacije\] 
    - Primjer: ms-windows-iz trgovine: PDP? PFN = 4d91298a-07cb-40fb-aecc-4cb5615d53c1
    - Početak bingmaps pretraživanja
    - bingmaps:?q =\[upit za pretraživanje\] 
    - Primjer: bingmaps:? pitanja = starbucks, Pariz
    - Korištenje prilagođene sheme
    - \[prilagođene sheme\]://\[params prilagođene sheme\] 
    - Primjer: myCustomProtocol://myCustomParams
    - Korištenje podataka paketa (aplikacija trgovina za proširenje čitati obavezno)
    - \[mapa\]\[podataka\]. \[proširenja\] 
    - Example:myfolderdata.txt
 
### <a name="build-a-tracking-url"></a>Sastavljanje praćenje URL:
-    U odjeljku "Postavke" na <UI Documentation> za upute o izgradnji praćenje URL-a koji će korisnicima da biste preuzeli neke druge aplikacije.
 
### <a name="define-the-texts-of-your-announcement"></a>Definiranje tekstova vaše objave
Unesite naslov, sadržaj i gumb tekstova vaše priopćenja. Usmjeriti možete ciljne skupine buduće kampanje na temelju povratnih informacija razgovor kako korisnici odgovorio kampanje. Ciljne skupine mogu se temeljiti na povratne informacije o tome je li kampanje je samo pomiču, odgovora, actioned ili napuste.

### <a name="see-also"></a>Vidi također
- [Korisničko Sučelje dokumentaciju - razgovor – novi automatske kriterij][Link 28]

## <a name="content-of-polls"></a>Sadržaj anketa
![Razgovor Content2][31] unesite naslov, opis i gumb tekstova vaše priopćenja. Zatim dodajte pitanja i mogućnosti za odgovore na svoja pitanja.
Usmjeriti možete ciljne skupine buduće kampanje na temelju povratnih informacija razgovor kako korisnici odgovorio kampanje. Ciljne skupine mogu se temeljiti na li kampanje je samo pomiču, odgovora, actioned ili napuste. Ciljne skupine mogu se temeljiti na anketu odgovora povratne informacije koje koriste pitanja i odgovora odabir kao kriterija.

### <a name="see-also"></a>Vidi također
- [Korisničko Sučelje dokumentaciju - razgovor – novi automatske kriterij][Link 28]
 
## <a name="content-of-data-pushes"></a>Sadržaj ih gura podataka
![Razgovor Content3][32] 

### <a name="choose-the-type-of-your-data"></a>Odaberite vrstu podataka:
- Tekst
- Binarni podaci
- Base64 podataka

### <a name="define-the-content-of-your-data"></a>Definiranje sadržaja podataka
- Ako ste odabrali automatske tekstne podatke, kopirajte i zalijepite tekst u okvir "sadržaj".
- Ako ste odabrali automatske binarni ili base64 podataka, pomoću gumba "Prenesite datoteku" Prenesite datoteku.
- Usmjeriti možete ciljne skupine buduće kampanje na temelju povratnih informacija razgovor kako korisnici odgovorio kampanje. Ciljne skupine mogu se temeljiti na li kampanje je samo pomiču, odgovora, actioned ili napuste.

### <a name="see-also"></a>Vidi također
- [Korisničko Sučelje dokumentaciju - razgovor – novi automatske kriterij][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>Sadržaj pločice (samo za Windows Phone)
![Razgovor Content4][33]

### <a name="define-the-content-of-your-tile"></a>Definiranje sadržaja pločicu
Opseg pločica je tekst koji će se prikazivati na pločicu aplikacije na uređajima Windows Phone.
Pločica automatske je Microsoft automatske obavijesti servisa (MPNS) verzija nativni automatske za Windows Phone. Automatske vrsta pločice je samo automatske vrstu koju ste odgovor i tako da publika buduće kampanje nije moguće napraviti na stranici rezultata s kampanjom automatske pločica. 

### <a name="see-also"></a>Vidi također
- [API dokumentaciju - razgovor API - nativni pribadače][Link 4]

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
