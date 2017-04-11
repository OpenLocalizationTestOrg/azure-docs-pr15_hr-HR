<properties
    pageTitle="Azure Active Directory uvjetno pristup najčešća pitanja vezana uz | Microsoft Azure"
    description="Najčešća pitanja o uvjetno programa access "
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-faq"></a>Azure Active Directory uvjetno pristup najčešća Pitanja

## <a name="which-applications-work-with-conditional-access-policies"></a>Koje aplikacije raditi s pravila uvjetnog pristup?

**A:** Pogledajte temu, [uvjetno aplikacije programa access što su podržane](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Pravila uvjetnog pristup provode B2B suradnje i korisnicima gostima?

**A:** Korisnici suradnju B2B provode pravila. No u nekim slučajevima, korisnik možda nećete moći zadovoljili obavezne pravila ako, na primjer, tvrtke ili ustanove ne podržava višestruka provjera autentičnosti. 

Pravilnik trenutno ne nameće za korisnike goste sustava SharePoint. Odnos goste održava se u sustavu SharePoint. Korisnicima gostima računi nisu primjenjuju pristup pravila na poslužiteljima za provjeru autentičnosti. Pristup gosta upravlja se u sustavu SharePoint.

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Ne sustava SharePoint Online pravila primjenjuju na OneDrive za tvrtke?

**A:** Da.
 
## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Zašto nije moguće postaviti pravila u klijentskim aplikacijama, kao što su Word ili Outlook?

**A:** Pravilo uvjetnog access postavlja zahtjeve za pristup servisa i provode kada se dogodi provjere autentičnosti na tom servisu. Pravilnik nije postavljen izravno na klijentskoj aplikaciji; Umjesto toga, primijenit će se kada poziva u servisa. Na primjer, pravilo koje Postavi u sustavu SharePoint primjenjuje klijentima pozivanje sustava SharePoint i pravila postavljena na Exchange odnosi se na Outlook.


## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Pravilo uvjetnog pristup odnosi računa servisa?

**A:** Pristup uvjetnog pravila primjenjuju se na sve korisničke račune. To obuhvaća korisničke račune koji se koristi kao računa servisa. U mnogim slučajevima, račun servisa koja se pokreće nenadziranog nije uspio zadovoljavaju pravila. Ovo je, primjerice predmet, kad potreban je MFA. U tim slučajevima računa za servise možete izuzeti iz pravilnika, uvjetno pristup pravila upravljanja postavkama. Dodatne informacije o primjeni pravila korisnicima ovdje.
