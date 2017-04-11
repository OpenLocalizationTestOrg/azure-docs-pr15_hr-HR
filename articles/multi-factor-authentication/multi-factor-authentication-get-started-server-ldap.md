<properties 
    pageTitle="Višestruka provjera autentičnosti poslužitelja LDAP provjeru autentičnosti i Azure"
    description="To je stranica Azure višestruke provjere autentičnosti koje će vam pomoći u uvođenju LDAP provjeru autentičnosti i Azure višestruke provjere autentičnosti poslužitelja."
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

# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>Višestruka provjera autentičnosti poslužitelja LDAP provjeru autentičnosti i Azure


Prema zadanim postavkama Azure višestruku provjeru autentičnosti poslužitelja je konfiguriran za uvoz i sinkronizacija korisnika iz servisa Active Directory. Međutim, moguće je konfigurirati za povezivanje s različitim LDAP direktorijima, kao što je SLIKA imenika ili određene kontroler domene servisa Active Directory. Kada je konfiguriran za povezivanje sa servisom directory putem LDAP, Azure višestruku provjeru autentičnosti poslužitelja mogu konfigurirati će poslužiti kao LDAP proxy poslužitelja za izvođenje authentications. Omogućuje i korištenje LDAP vezanja kao cilj POLUMJER, prije provjere autentičnosti korisnika prilikom korištenja provjere autentičnosti IIS ili primarni provjere autentičnosti na portalu Azure višestruke provjere autentičnosti korisnika.

Prilikom korištenja Azure višestruka provjera autentičnosti kao proxyja kojemu je provjerena LDAP, Azure višestruku provjeru autentičnosti poslužitelja umetnut će se između klijenta za LDAP (npr. VPN uređaj, aplikacija) i LDAP imenički poslužitelj da biste dodali višestruke provjere autentičnosti. Za Azure multi-factor Authentication funkcije Azure višestruku provjeru autentičnosti poslužitelja morate konfigurirati za komunikaciju s poslužitelja klijenta i LDAP imenika. U ovoj konfiguraciji Azure višestruku provjeru autentičnosti poslužitelja prihvaća LDAP zahtjeve iz poslužitelja klijenta i aplikacije i prosljeđuje cilj LDAP imenički poslužitelj za provjeru valjanosti primarni vjerodajnice. Ako odgovor LDAP imenik pokazuje da primarni vjerodajnice su valjane, Azure višestruka provjera autentičnosti izvodi druge provjere autentičnosti i šalje odgovor LDAP klijent. Cijeli provjere autentičnosti će uspjeti samo ako je provjera autentičnosti za LDAP poslužitelj i višestruka provjera autentičnosti uspjeti.





## <a name="ldap-authentication-configuration"></a>Konfiguriranje provjere LDAP


Konfiguriranje provjere autentičnosti LDAP, instalirajte Azure višestruku provjeru autentičnosti poslužitelja na Windows server. Pomoću sljedećeg postupka:

1. Unutar Azure višestruke provjere autentičnosti poslužitelja kliknite ikonu LDAP provjere autentičnosti na lijevom izborniku.
2. Potvrdite okvir Omogući provjeru autentičnosti LDAP.![Provjera autentičnosti LDAP](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)
3. Na kartici klijenti promijenite TCP priključak i SSL priključak ako servisa Azure višestruke provjere autentičnosti LDAP treba povezati nestandardne priključke da biste preslušali za LDAP zahtjeve klijenata koji će se konfigurirati.
4. Ako namjeravate koristiti LDAPS iz klijenta na poslužitelj za Azure višestruke provjere autentičnosti, SSL certifikat mora biti instaliran na poslužitelju na kojem se na poslužitelju izvodi na. Kliknite gumb pregledavanje... gumb Dalje da biste okvir SSL certifikat, a zatim odaberite instalirani certifikat koji će se koristiti za sigurnu vezu.
5. Kliknite Dodaj... gumb.
6. U dijaloškom okviru Dodavanje LDAP klijenta unesite IP adresu potražite, poslužitelja ili aplikacije koja će provjeru autentičnosti poslužitelja i naziv aplikacije (neobavezno). Naziv aplikacije će se prikazati u izvješćima Azure višestruke provjere autentičnosti i može se prikazati unutar SMS ili mobilnu aplikaciju poruka provjere autentičnosti.
7. Potvrdite okvir zatraži Azure višestruka provjera autentičnosti korisnika podudaranje ako svi korisnici su ili će biti uvezeni u poslužitelja i predmet mutli provjere autentičnosti. Ako značajan broj korisnika još nisu uvezeni u poslužitelja ili bit će izuzeti iz pravilnika za mutli provjere autentičnosti, ostavite okvir poništen. Potražite datoteku pomoći za dodatne informacije o toj značajci.
8. Možda ponovite korake od 5 do 7 da biste dodali dodatne LDAP klijente.
9. Kada Azure višestruka provjera autentičnosti je konfiguriran za primanje LDAP authentications, ga morate proxy te authentications LDAP direktorij. Stoga kartica ciljne samo prikazuje u jednoj, zasivljena mogućnost za korištenje programa LDAP cilj. Da biste konfigurirali LDAP vezu direktorija, kliknite ikonu Integracija direktorija.
10. Na kartici postavke odaberite koristi određeni LDAP konfiguracije izborni gumb.
11. Kliknite Uredi... gumb.
12. U dijaloškom okviru Uređivanje konfiguracije LDAP ispunite obavezna polja uz podatke potrebne za povezivanje s LDAP imenika. Opisi polja nalaze se u tablici u nastavku. Napomena: Taj podatak je obuhvaćen i datoteke pomoći Azure višestruke provjere autentičnosti poslužitelja.![Integracija direktorija](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)
13. Testiranje veze LDAP tako da kliknete gumb za testiranje.
14. Ako Testiranje veze LDAP je uspio, kliknite gumb u redu.
15. Kliknite karticu filtre. Poslužitelj je unaprijed konfigurirana za učitavanje spremnika, sigurnosnim grupama i korisnicima iz servisa Active Directory. Ako povezivanje drugi direktorij LDAP, vjerojatno ćete morati Uredi filtri prikazana. Kliknite vezu pomoć za dodatne informacije o filtrima.
16. Kliknite karticu atribute. Poslužitelj je unaprijed konfigurirana za mapiranje atribute iz servisa Active Directory.
17. Ako je povezivanje da biste drugi LDAP direktorij ili promijenili unaprijed konfigurirana atribut mapiranja, kliknite Uredi... gumb.
18. U dijaloškom okviru Uređivanje atributa izmijeniti mapiranja atributa LDAP za direktorija. Atribut naziva se možete upisati ili odabrati tako da kliknete na... gumb uz svako polje.
19. Kliknite vezu pomoć za dodatne informacije o atribute.
20. Kliknite gumb u redu.
21. Kliknite ikonu postavke tvrtke, a zatim odaberite karticu razlučivost korisničko ime.
22. Ako povezivanje sa servisom Active Directory s poslužitelja domene pridruženo moći da biste napustili sigurnosni identifikatori koristi Windows (SID-ovi) za koji se podudaraju korisnička imena izborni gumb odabran. U suprotnom odaberite atribut LDAP koristi jedinstveni identifikator za odgovarajući izborni gumb korisnička imena. Kada je odabrana ta mogućnost, Azure višestruku provjeru autentičnosti poslužitelja će pokušati riješiti svaki korisničko ime za Jedinstveni identifikator u imeniku LDAP. LDAP pretraživanje provest će se na korisničko ime atribute definirane u Integracija direktorija -> atribute. Kada korisnik se autentificira servisa, korisničko ime koje će biti razriješiti Jedinstveni identifikator u imeniku LDAP i Jedinstveni identifikator će se koristiti za koji se podudaraju u podatkovnoj datoteci Azure višestruke provjere autentičnosti korisnika. Time se omogućuje velika i mala slova usporedbe, kao što je korisničko ime i kao dugih i kratkih oblikuje to dovršava konfiguracija Azure višestruke provjere autentičnosti poslužitelja. Poslužitelj sada se priključuje na konfigurirani priključke za zahtjeve za pristup LDAP konfigurirani klijenata i postavite proxy zahtjeve za LDAP imenik za provjeru autentičnosti.


## <a name="ldap-client-configuration"></a>Konfiguriranje klijenta LDAP

Da biste konfigurirali LDAP klijent, slijedite smjernice:

- Konfiguriranje uređaj, poslužitelja ili aplikaciju za provjeru autentičnosti putem LDAP na poslužitelj za Azure višestruke provjere autentičnosti kao da je LDAP imenika. Koristite iste postavke koje bi inače koristite izravno povezivanje s LDAP imenik, osim naziv poslužitelja ili IP adresu koja će se dogoditi da Azure višestruke provjere autentičnosti poslužitelja.
- Konfiguriranje LDAP vremenskog ograničenja za 30 do 60 sekundi tako da postoji vremena za provjeru vjerodajnica korisnika s LDAP imenik, izvedite druge provjera autentičnosti, dobiti odgovor i odgovoriti na zahtjev za pristup LDAP.
- Ako koristite LDAPS, potražite ili poslužitelj za upućivanje LDAP upita mora vjerovati SSL certifikat koji je instaliran na poslužitelju Azure višestruke provjere autentičnosti.
