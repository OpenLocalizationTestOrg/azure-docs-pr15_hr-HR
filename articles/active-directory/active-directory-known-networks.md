<properties 
    pageTitle="Poznati mreža | Microsoft Azure" 
    description="Konfiguriranjem poznati mreža možete izbjegnete IP adrese koje su u vlasništvu tvrtke ili ustanove u sklopu ins znak s više geographies i ins znak s IP adresa s izvješćima sumnjivoj aktivnosti." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"  
    editor=""/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markvi"/>

# <a name="known-networks"></a>Poznati mreža


Azure Active Directory, access i izvješća o korištenju možete koristiti da bi se dobio uvid u integritet i sigurnost imenik tvrtke ili ustanove. Pomoću tih informacija direktorija administrator možete bolje odrediti gdje može biti moguće sigurnosni rizik tako da ih možete planirati odgovarajući način prevladavanje tih rizika.

Moguće je izvješća "*ins znak s više geographies*" i "*ins znak iz IP adresa pomoću sumnjivoj aktivnosti*" neispravno zastavica IP adrese doista vlasnik tvrtke ili ustanove. 

To se može, na primjer, dogoditi kada: 

- Korisnik u vašem Meso bostonske office prijavio daljinski podatkovnog centra u Pula pokrene izvješće "Prijavite ins iz više geographies" 

- Korisnik vaše tvrtke ili ustanove pokuša prijave nekoliko puta s je neispravna lozinka okidača izvješće "Odjavili dodaci na IP adrese s sumnjivoj aktivnosti" 

Da biste spriječili generiranje izvješća neispravne sigurnost tim slučajevima, trebali biste dodali poznati rasponi IP adresa na popis javnu IP adresu tvrtke ili ustanove.    


###<a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>Da biste dodali svoje tvrtke ili ustanove javno rasponi IP adresa, poduzmite sljedeće korake: 

1.  Prijava na [portal za upravljanje Azure](https://manage.windowsazure.com).

2.  U lijevom oknu kliknite **Servisa Active Directory**. <br><br>![Kako funkcionira otkrivanje aplikacije oblaka](./media/active-directory-known-networks/known-netwoks-01.png)

3.  Na kartici **direktorija** odaberite direktorija.

4.  Na izborniku na vrhu kliknite **Konfiguriraj**. <br><br>![Kako funkcionira otkrivanje aplikacije oblaka](./media/active-directory-known-networks/known-netwoks-02.png)

5.  Na kartici Konfiguriraj idite na **vaše tvrtke ili ustanove javno rasponi IP adresa** <br><br>![Kako funkcionira otkrivanje aplikacije oblaka](./media/active-directory-known-networks/known-netwoks-03.png)

6.  Kliknite **Dodaj rasponi poznati IP adresa**.

7.  Dodavanje raspone adresa u dijaloškom okviru koji će se pojaviti, a zatim gumb Provjera kada završite. <br><br>![Kako funkcionira otkrivanje aplikacije oblaka](./media/active-directory-known-networks/known-netwoks-04.png)


**Dodatni resursi**


* [Prikaz izvješća pristupa i korištenja](active-directory-view-access-usage-reports.md)
* [Prijavite se s IP adresa ins pomoću sumnjivoj aktivnosti](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Prijavite se ins iz više geographies](active-directory-reporting-sign-ins-from-multiple-geographies.md)


