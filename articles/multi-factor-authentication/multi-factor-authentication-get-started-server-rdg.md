<properties 
    pageTitle="Udaljene radne površine pristupnik i pomoću POLUMJER na poslužitelj za Azure višestruke provjere autentičnosti"
    description="To je stranica Azure višestruke provjere autentičnosti koje će vam pomoći u uvođenju udaljene radne površine (RD) pristupnika i pomoću POLUMJER na poslužitelj za Azure višestruke provjere autentičnosti."
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
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Udaljene radne površine pristupnik i pomoću POLUMJER na poslužitelj za Azure višestruke provjere autentičnosti

U mnogim slučajevima, pristupni koristi lokalni NPS za provjeru autentičnosti korisnika. Ovaj dokument opisuje usmjeravanje POLUMJER pošalje zahtjev za odgovor udaljene radne površine pristupnika (putem lokalnog NPS) s poslužiteljem višestruke provjere autentičnosti.

Provjera autentičnosti poslužitelja višestruku trebala biti instalirana na istom poslužitelju, koje će zatim proxy zahtjev za POLUMJER natrag na NPS na udaljeni poslužitelj pristupnika za radnu površinu. Kada NPS Provjeri valjanost korisničko ime i lozinku, će vratiti odgovor na poslužiteljima za provjeru autentičnosti multi-factor koji se izvodi drugi faktor provjere autentičnosti prije vraćanja rezultat pristupnika.





## <a name="configure-the-rd-gateway"></a>Konfiguriranje RD pristupnika

Da biste poslali provjere autentičnosti POLUMJER poslužitelj za Azure višestruke provjere autentičnosti, morate ga konfigurirati RD pristupnik. Kada je instaliran pristupnik RD konfigurirana, a je rada, idite u svojstva RD pristupnik. Idite na karticu RD Kapaciteta pohrane i promijenite ga da biste koristili središnje poslužitelju koji se izvodi NPS umjesto lokalni poslužitelj koji se izvodi NPS. Dodavanje poslužitelje Azure višestruke provjere autentičnosti kao POLUMJER poslužitelji i navedite zajedničke tajna za svaki poslužitelj.





## <a name="configure-nps"></a>Konfiguriranje NPS

Polje RD pristupnik koristi NPS da biste poslali zahtjev za POLUMJER Azure višestruke provjere autentičnosti. Da biste spriječili RD pristupnika iz isteklo prije višestruka provjera autentičnosti dovrši moraju promijeniti vremenskog ograničenja. Da biste konfigurirali NPS pomoću sljedećeg postupka.

1. U NPS, proširite izbornik POLUMJER klijenata i poslužitelja u lijevom stupcu, a zatim kliknite na udaljenom grupa POLUMJER poslužitelja. Idite u svojstva GRUPI TS PRISTUPNIK POSLUŽITELJA. Uredite poslužitelji POLUMJER prikazati, a zatim idite na karticu opterećenja. Promjena na "broj sekundi bez odgovora prije nego što se smatra se zahtjev za ispuštanje" i "Broj sekundi između zahtjeva kada poslužitelj je označena kao nedostupan" da biste 30 do 60 sekundi. Kliknite karticu račun za/provjeru autentičnosti i provjerite je li da odgovara priključke POLUMJER navedeni priključci koji provjeru autentičnosti poslužitelja multi-factor će se priključuje na.
2. NPS morate ga konfigurirati i ponovno primanje POLUMJER authentications Azure višestruku provjeru autentičnosti poslužitelja. Kliknite na POLUMJER klijente na lijevom izborniku. Dodajte Azure višestruku provjeru autentičnosti poslužitelja kao POLUMJER klijenta. Odaberite neslužbeni naziv i navedite zajedničke tajna.
3. Proširite odjeljak pravila u lijevom navigacijskom oknu, a zatim kliknite na vezu zahtjev pravila. Ona mora sadržavati vezu zahtjev za pravilo koje se zove TS PRISTUPNIKA AUTORIZACIJE pravila koja je stvorena kada je konfiguriran RD pristupnik. Ovo pravilo prosljeđuje POLUMJER zahtjeva provjeru autentičnosti poslužitelja višestruke provjere.
4. Kopirajte ovo pravilo da biste stvorili novi. U novog pravila, dodajte uvjet koji odgovara neslužbeni naziv klijenta s neslužbeni naziv postavljanje u koraku 2 za Azure višestruke provjere autentičnosti poslužitelja POLUMJER klijenta. Promijeniti davatelja usluge provjere autentičnosti na lokalno računalo. Ovo pravilo osigurava da zahtjev za POLUMJER primitku s Azure višestruke provjere autentičnosti poslužitelja provjeru autentičnosti pojavljuje lokalno umjesto slanja zahtjeva za POLUMJER poslužitelj za provjeru autentičnosti višestruku Azure čijim petlja uvjeta. Da biste spriječili uvjet petlje, ova novog pravilnika mora naručiti iznad izvorne pravila koja prosljeđuje s poslužiteljem višestruke provjere autentičnosti.

## <a name="configure-azure-multi-factor-authentication"></a>Konfiguriranje Azure višestruka provjera autentičnosti


--------------------------------------------------------------------------------



Poslužitelj za provjeru autentičnosti Azure multi-factor konfiguriran kao proxy POLUMJER između RD pristupnika i NPS.  Trebala biti instalirana na poslužitelju domene pridruženo koji razlikuje se od poslužitelj RD pristupnika. Konfiguriranje provjere autentičnosti poslužitelja za višestruku Azure pomoću sljedećeg postupka.

1. Otvorite Azure višestruku provjeru autentičnosti poslužitelja, a zatim kliknite ikonu POLUMJER provjere autentičnosti. Potvrdite okvir Omogući POLUMJER provjere autentičnosti.
2. Na kartici klijenata, provjerite je li priključke odgovaraju što je konfiguriran u NPS i kliknite gumb Dodaj... gumb. Dodajte IP adresa poslužitelja RD pristupnika, naziv aplikacije (nije obavezno) i dijeljena tajna. Dijeljena tajna bit će potrebno na poslužitelj za Azure višestruke provjere autentičnosti i RD pristupnik.
3. Kliknite karticu odredišne i odaberite POLUMJER poslužitelji izborni gumb.
4. Kliknite gumb Dodaj... gumb. Unesite IP adresu, dijeljena tajna i priključke NPS poslužitelja. Osim ako se koriste središnje NPS POLUMJER klijenta i POLUMJER ciljne će biti ista. Dijeljena tajna mora podudarati jedan postavljanje u odjeljku klijentski POLUMJER NPS poslužitelja.

![Provjera autentičnosti polumjer](./media/multi-factor-authentication-get-started-server-rdg/radius.png)
