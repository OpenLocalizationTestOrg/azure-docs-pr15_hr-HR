<properties 
    pageTitle="Pošalji personalizirane obavijest s mogućnostima Mobile Azure" 
    description="Kako poslati personalizirane obavijesti tako da uvrstite podatke korisničkog profila u obavijesti kao što su njihova imena"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="all" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="personalize-notifications-by-including-user-name"></a>Personalizacija obavijesti uključivanjem korisničko ime

U dobar da biste obavijesti privlačnije korisnicima aplikacije, trebali biste ih personalizacija. Jedan Napredna pristup sastoji se od selektivno adrese obavijesti da biste ih dodatnih osobnih korištenja aplikacije korisnička imena. Upozorenje ovdje - trebali biste postići dodavanjem korisnička imena da biste obavijesti pažljivo jer ako pretjerana upotreba ovaj strategije pa ga nije moguće došli creepy, za neke korisnike aplikacije. Trebali biste provjerite da su da korisnik uključivanje kako bi pružio te osobnih podataka na punu pristanak u mobilnoj aplikaciji prije početka da biste mogli koristiti. 

Tehnički, s mogućnostima Mobile Azure, koje možete izvršiti personalizacija obavijesti slijedeći korake u nastavku u kojem koristit ćemo scenarij uključujući korisničko ime u obavijesti. Koristite pojam aplikacije informacije ili oznake čije vrijednosti ili se proslijediti tako da na SDK-ovi integrirani putem API-ji ili mobilnu aplikaciju. Zatim može koristiti ove aplikacije Infos ili oznake:

1. za određivanje obavijesti za određene korisnike koji se temelje na vrijednostima aplikacije informacije ili 
2. kao rezervirana mjesta u obavijesti koja će biti zamijenjene vrijednosti koje se odnose na uređaju/korisnik prilikom slanja obavijesti na tom uređaju. 

> [AZURE.IMPORTANT] Imajte na umu brzinu slanja obavijesti će vidjeti smanjena zbog ovu dodatnu obradu zamjene aplikacije informacije vrijednosti s svaki obavijesti. 

##<a name="register-app-info-in-the-mobile-engagement-portal"></a>Registracija aplikacije informacije na portalu za mobilne radnje

1) Započnite Registracija aplikacije informacije ili oznake na portalu za Azure. Idite na **Postavke** -> **oznaka (aplikacija informacije)** za to.  

![][1]  

2) Kliknite **Nova oznaka (aplikacija informacije)** i navedite naziv kao *korisničko_ime* i vrsta kao *niz* , pa kliknite **Pošalji**. 

![][2]

3) Prikazat će se na kraju aplikacije-informacije registrirana kao što je sljedeća:

![][3]

##<a name="send-app-info-from-the-client-sdk"></a>Šalji podatke za aplikacije iz klijentskog programa SDK

Ovdje se pomoću Windows univerzalni primjer aplikacije, ali ekvivalentan metode postoje za naše ostale SDK-ovi i. 

Pod pretpostavkom da imate metode u mobilnoj aplikaciji gdje dohvatiti podatke profila korisnika kao što su njihova imena vjerojatno nakon provjere autentičnosti ih, će poziva `SendAppInfo` način ovdje i popunjavanje vrijednosti u `user_name` informacije aplikaciju koju ste ranije registrirali u pozadinski usluga mobilne radnje. 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

##<a name="send-personalized-notifications"></a>Slanje obavijesti personaliziranu

Sada su postavljena za slanje obavijesti o korištenju **korisničko_ime**. 

1) Idite na Portal radnje Mobile na kartici **dosegne** da biste stvorili obavijest i koristite to rezervirano mjesto u sljedećem obliku bilo gdje u naslovu obavijesti ili tijelo. 

![][4]  

> [AZURE.NOTE] Sve korisnike za koje informacije aplikacije korisničko_ime nije postavljeno, će se sve obavijesti. Ako pokrenete kampanje obavijesti u načinu rada za testiranje i ako nemate aplikaciju informacije postavljanje, a zatim ćemo poslat će '?' znak da biste zamijenili rezervirano mjesto. 

2) Kada radnje mobilni će odaberite odgovarajući uređaj da biste poslali obavijest, a zatim će pogledajte ovu aplikaciju informacije i zamijenite vrijednost u rezerviranom mjestu.  
Na primjer, ako ne možemo postavljene `str = "Scott"` korisnik od uređaj Registracija će dobiti pridružen aplikacije informacije o **korisničko_ime = LUKA** za ovog korisnika i taj korisnik će vidjeti za vrijeme odsutnosti aplikacije automatske obavijesti u sljedećem obliku. 

![][5]  

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png

