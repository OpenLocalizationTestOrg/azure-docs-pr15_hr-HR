<properties 
    pageTitle="Provjera autentičnosti IIS i Azure višestruka provjera autentičnosti poslužitelja"
    description="To je stranica Azure višestruke provjere autentičnosti koje će vam pomoći u uvođenju IIS provjere autentičnosti i Azure višestruke provjere autentičnosti poslužitelja."
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

# <a name="iis-authentication"></a>Provjera autentičnosti IIS

U odjeljku Provjera autentičnosti IIS Azure višestruke provjere autentičnosti poslužitelja možete omogućiti i konfigurirati IIS provjere autentičnosti za integraciju s Microsoft IIS web-aplikacijama. Azure višestruku provjeru autentičnosti poslužitelja instalira dodatak koji možete filtrirati zahtjeve u tijeku pokušaj IIS web-poslužitelj da biste dodali Azure višestruke provjere autentičnosti. Dodatak za IIS pruža podršku za provjeru autentičnosti utemeljenu na obrascu i integriranu provjeru autentičnosti sustava Windows HTTP-a. Pouzdani IP-ovi mogu se konfigurirati izuzetih interne IP adrese iz dvostruka provjera autentičnosti.


![Provjera autentičnosti IIS](./media/multi-factor-authentication-get-started-server-iis/iis.png)


## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Pomoću provjere autentičnosti utemeljene na obrascu IIS Azure višestruka provjera autentičnosti poslužitelja

Sigurnost IIS web-aplikacije koja koristi provjeru autentičnosti utemeljenu na obrascu, instalirajte Azure višestruke provjere autentičnosti poslužitelja na web-poslužitelju IIS i konfiguriranje poslužitelja za postupak u nastavku.

1. Unutar Azure višestruke provjere autentičnosti poslužitelja kliknite ikonu IIS provjere autentičnosti na lijevom izborniku.
2. Kliknite karticu utemeljene na obrascu.
3. Kliknite gumb Dodaj... gumb.
4. Da biste otkrili korisničko ime, lozinku i domenu varijabli automatski, unesite URL za prijavu (npr. https://localhost/contoso/auth/login.aspx) u dijaloškom okviru Auto-Configure Form-Based web-mjesta i kliknite u redu.
5. Potvrdite okvir zatraži višestruke provjere autentičnosti korisnika ako svi korisnici su ili će biti uvezeni u poslužitelja i predmet višestruke provjere autentičnosti. Ako značajan broj korisnika još nisu uvezeni u poslužitelja ili bit će izuzeti iz pravilnika višestruka provjera autentičnosti, ostavite okvir poništen.
6. Varijable stranice nije moguće automatski otkriti, kliknite na odredite ručno... gumb u dijaloškom okviru Auto-Configure Form-Based web-mjesta.
7. U dijaloškom okviru Add Form-Based web-mjesta unesite URL na stranicu za prijavu u polje URL za slanje, a zatim unesite naziv aplikacije (neobavezno). Naziv aplikacije će se prikazati u izvješćima Azure višestruke provjere autentičnosti i može se prikazati unutar SMS ili mobilnu aplikaciju poruka provjere autentičnosti. Datoteka pomoći za dodatne informacije potražite na URL za slanje.
8. Odaberite ispravnom obliku zahtjev. To je postavljeno na "Objavu ili DOBITI" za većinu web-aplikacije.
9. Unesite varijabla korisničko ime, lozinka varijabla i varijabla domene (Ako je on se pojavljuje na stranici prijava). Možda ćete morati idite na stranicu za prijavu u web-pregledniku, na stranici, kliknite desnom tipkom miša i odaberite "Prikaži izvor" da biste pronašli imena okvire unosa unutar stranice.
10. Potvrdite okvir zatraži Azure višestruka provjera autentičnosti korisnika podudaranje ako svi korisnici su ili će biti uvezeni u poslužitelja i predmet višestruke provjere autentičnosti. Ako značajan broj korisnika još nisu uvezeni u poslužitelja ili bit će izuzeti iz pravilnika višestruka provjera autentičnosti, ostavite okvir poništen. Potražite datoteku pomoći za dodatne informacije o toj značajci.
11.  Kliknite naprednog... gumb da biste pregledali Napredne postavke, uključujući mogućnost za odabir datoteke na stranici prilagođene uskraćivanje predmemoriju uspješno authentications na web-mjesto za neko vrijeme pomoću kolačiće i odaberite želite li provjeru autentičnosti primarni vjerodajnica protiv domeni sustava Windows, LDAP imenika ili POLUMJER poslužitelja. Po završetku kliknite gumb u redu da biste se vratili u dijaloški okvir Add Form-Based web-mjesta. Pogledajte datoteke pomoći za dodatne informacije o Napredne postavke.
12. Kliknite gumb u redu.
13. Kada varijable URL i stranice su otkrivena ili unijeli, podataka web-mjesta prikazat će se na ploči utemeljene na obrascu.
14. Potražite u članku Omogućivanje IIS dodataka za Azure višestruke provjere autentičnosti poslužitelja odjeljak izravno ispod da biste dovršili konfiguraciju IIS provjere autentičnosti.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Pomoću provjere autentičnosti Integriranom Windows Azure višestruka provjera autentičnosti poslužitelja

Sigurnost IIS web-aplikacije koja koristi provjeru autentičnosti integriranu Windows HTTP, instalirajte Azure višestruku provjeru autentičnosti poslužitelja s IIS web-poslužitelja i konfigurirati poslužitelj za postupak u nastavku.

1. Unutar Azure višestruke provjere autentičnosti poslužitelja kliknite ikonu IIS provjere autentičnosti na lijevom izborniku.
2. Kliknite karticu HTTP-a. Kliknite karticu utemeljene na obrascu.
3. Kliknite gumb Dodaj... gumb.
4. U dijaloški okvir Dodavanje osnovni URL unesite URL za web-mjesto gdje se nalazi HTTP provjeru autentičnosti izvršiti (npr. http://localhost/owa) u polje Osnovni URL i upišite naziv aplikacije (neobavezno). Naziv aplikacije će se prikazati u izvješćima Azure višestruke provjere autentičnosti i može se prikazati unutar SMS ili mobilnu aplikaciju poruka provjere autentičnosti.
5. Prilagodite neaktivnosti vremenskog ograničenja i maksimum sesiju puta ako zadani nije dovoljno.
6. Potvrdite okvir zatraži višestruke provjere autentičnosti korisnika ako svi korisnici su ili će biti uvezeni u poslužitelja i predmet višestruke provjere autentičnosti. Ako značajan broj korisnika još nisu uvezeni u poslužitelja ili bit će izuzeti iz pravilnika višestruka provjera autentičnosti, ostavite okvir poništen. Potražite datoteku pomoći za dodatne informacije o toj značajci.
7. Po želji, potvrdite okvir predmemoriju kolačića.
8. Kliknite gumb u redu.
9. U odjeljku [Omogućivanje IIS dodatke za Azure višestruke provjere autentičnosti poslužitelja](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server) izravno ispod da biste dovršili konfiguraciju IIS provjere autentičnosti.


## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>Omogući dodatke za IIS za poslužitelj za Azure višestruka provjera autentičnosti

Nakon što ste konfigurirali u utemeljene na obrascu ili u IIS HTTP URL-ovi provjeru autentičnosti i postavke, morate odabrati mjesta gdje treba učitavanje i omogućeno Azure višestruke provjere autentičnosti IIS dodataka. Pomoću sljedećeg postupka:

1. Ako je pokrenut na sustavu IIS 6, kliknite karticu ISAPI i odaberite web-mjesto sa web-aplikaciju u odjeljku (npr. zadani Web-mjesta) da biste omogućili filtar Azure višestruke provjere autentičnosti ISAPI dodatak za web-mjesta.
2. Ako se izvodi na IIS 7 ili noviji, kliknite karticu nativni modul i odaberite poslužitelj, web-mjesta ili aplikacije da biste omogućili IIS dodatka na željeni razina.
3. Potvrdite okvir Omogući IIS provjere autentičnosti pri vrhu zaslona. Azure višestruka provjera autentičnosti sada je zaštita odabrane IIS aplikacije. Provjerite je li da korisnici uvezete na poslužitelj. Ako želite whitelist interne IP adrese pa dvofaktorska analiza varijance autentičnosti nije potrebna kada je bilježenje u zapisnik web-mjesto s tim mjestima potražite u članku pouzdana IP-ovi odjeljak u nastavku.


## <a name="trusted-ips"></a>Pouzdani IP-ovi

U pouzdana IP-ovi korisnicima omogućuje zaobilaženje Azure višestruka provjera autentičnosti zahtjeva za web-mjesto s određenim IP adresama ili podmreže za. Ako, na primjer, trebali biste izuzetih korisnika iz Azure višestruke provjere autentičnosti prilikom prijave u iz sustava office. To, želite navesti podmreže office kao pouzdane IP-ovi unos. Konfiguriranje pouzdanih IP-ovi pomoću sljedećeg postupka:

1. U odjeljku Provjera autentičnosti IIS karticu pouzdana IP-ovi.
2. Kliknite gumb Dodaj... gumb.
3. Kada se pojavi dijaloški okvir Dodavanje IP-pouzdana ovi, odaberite jedan IP, rasponu IP podmreže izborni gumb.
4. Centar za pouzdanost IP adresa raspon IP adresa ili podmreže koje treba whitelisted. Ako unesete podmreži, odaberite odgovarajući maska mreže i kliknite gumb u redu. Sada je dodana u whitelist.
