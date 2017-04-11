<properties 
    pageTitle="Provjera autentičnosti sustava Windows i Azure višestruka provjera autentičnosti poslužitelja"
    description="To je stranica Azure višestruke provjere autentičnosti koje će vam pomoći u uvođenju provjeru autentičnosti sustava Windows i Azure višestruke provjere autentičnosti poslužitelja."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Provjera autentičnosti sustava Windows i Azure višestruka provjera autentičnosti poslužitelja

U odjeljku Provjera autentičnosti sustava Windows omogućuje administratora Omogućivanje i konfiguriranje provjere autentičnosti sustava Windows za jedan ili više programa.  Slijedi popis stvari koje treba imati na umu prije postavljanja provjeru autentičnosti sustava Windows.

-  ponovno pokrenite računalo potrebno je prije Azure višestruke provjere autentičnosti za Terminal Services bit će na snazi.
-  Ako je potvrđen "Zahtijevaju Azure višestruka provjera autentičnosti korisnika podudaranje", a niste u popis korisnika, nećete moći prijaviti na računalu nakon ponovnog pokretanja.
-  Pouzdani IP-ovi ovisi li aplikacija možete unijeti IP klijenta s provjeru autentičnosti. Podržana je trenutno samo Terminal Services.  







>[AZURE.NOTE]Ta značajka nije podržana za sigurnu Terminal Services u sustavu Windows Server 2012 R2.




## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Da biste sigurne aplikacije pomoću provjere autentičnosti sustava Windows, slijedite postupak u nastavku.

1. U okvir poslužitelj za Azure višestruke provjere autentičnosti kliknite ikonu za provjeru autentičnosti sustava Windows.
![Provjera autentičnosti sustava Windows](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Potvrdite okvir Omogući Windows provjeru autentičnosti. Po zadanom isključena se taj okvir.
3. Kartica aplikacija omogućuje administrator da biste konfigurirali jednu ili više aplikacija za provjeru autentičnosti sustava Windows.
4. Odaberite poslužitelj ili aplikacije – odrediti je li omogućeno poslužitelja/aplikacije. Kliknite u redu.
5. Kliknite Dodaj... gumb.
6. Na kartici pouzdani IP-ovi omogućuje da biste preskočili Azure višestruke provjere autentičnosti za Windows sesije s određenim IP-ovi. Na primjer, ako zaposlenici koristiti aplikacije iz sustava office i Početna stranica, možda odlučite želite ne svojim telefonima zvonjave za Azure višestruke provjere autentičnosti u uredu. To, želite navesti podmreže office kao pouzdane IP-ovi stavku.
7. Kliknite Dodaj... gumb.
8. Ako želite preskočiti jednom IP adresom, odaberite jedan IP.
9. Odaberite raspon IP ako želite preskočiti cijeli raspon za IP. Primjer 10.63.193.1-10.63.193.100.
10. Podmreže odaberite želite li da biste odredili u rasponu od IP-ovi pomoću podmreže notacije. Unesite početni IP podmreže na, a zatim odaberite odgovarajuće maska mreže s padajućeg popisa.
11. Kliknite gumb u redu.
