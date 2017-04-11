<properties 
    pageTitle="Provjera autentičnosti POLUMJER i Azure višestruka provjera autentičnosti poslužitelja"
    description="To je stranica Azure višestruke provjere autentičnosti koje će vam pomoći u uvođenju POLUMJER provjeru autentičnosti i Azure višestruke provjere autentičnosti poslužitelja."
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



# <a name="radius-authentication-and-azure-multi-factor-authentication-server"></a>Provjera autentičnosti POLUMJER i Azure višestruka provjera autentičnosti poslužitelja

U odjeljku Provjera autentičnosti POLUMJER omogućuje Omogućivanje i konfiguriranje provjere autentičnosti POLUMJER Azure višestruke provjere autentičnosti poslužitelja. POLUMJER je standardni protokol za prihvaćanje zahtjeva za provjeru autentičnosti i za obradu tih zahtjeva. Azure višestruku provjeru autentičnosti poslužitelja ponaša se kao POLUMJER poslužitelj i umeće se između POLUMJER klijenta (npr. potražite VPN-a) i vaš cilj provjeru autentičnosti, koja može biti Active Directory (AD), LDAP imenika ili drugi poslužitelj POLUMJER da biste dodali Azure višestruke provjere autentičnosti. Za Azure višestruka provjera autentičnosti funkciju, morate konfigurirati Azure višestruku provjeru autentičnosti poslužitelja tako da se komunikaciju s poslužitelja klijenta i ciljne provjere autentičnosti. Poslužitelj za provjeru autentičnosti Azure višestruke provjere prihvaća zahtjeve iz POLUMJER klijenta, Provjeri valjanost vjerodajnice odnosu cilj provjere autentičnosti, dodaje Azure višestruke provjere autentičnosti i šalje odgovor POLUMJER klijenta. Cijeli provjere autentičnosti neće uspjeti samo ako se primarni provjeru autentičnosti i višestruke provjere autentičnosti Azure uspjeti.

>[AZURE.NOTE]
>Poslužitelj za MFA podržava samo PAP (protokol za provjeru autentičnosti lozinke) i MSCHAPv2 (Microsoftovim rukovanja za ispitivanje provjere autentičnosti Protocol) POLUMJER protokoli kada ulozi POLUMJER poslužitelja.  Ostali protokoli kao što su EAP-a (extensible provjere autentičnosti protocol) može se koristiti kada poslužitelj MFA služi kao POLUMJER proxy drugom POLUMJER poslužitelju koji podržava tu protokol kao što je Microsoft NPS.
></br>
>Prilikom korištenja ostalih protokola u ovoj konfiguraciji, jednosmjerna SMS i OATH tokeni ne funkcionira jer je poslužitelj MFA moći pokrenuti uspješno POLUMJER pitanje odgovor pomoću tog protokola.


![Provjera autentičnosti polumjer](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="radius-authentication-configuration"></a>Konfiguriranje provjere POLUMJER

Konfiguriranje provjere autentičnosti POLUMJER, instalirajte Azure višestruku provjeru autentičnosti poslužitelja na Windows server. Ako imate okruženju Active Directory, poslužitelj trebala bi biti spojena domenu unutar mreže. Konfiguriranje provjere autentičnosti poslužitelja za višestruku Azure pomoću sljedećeg postupka:

1. Unutar Azure višestruke provjere autentičnosti poslužitelja kliknite ikonu provjere autentičnosti POLUMJER u lijevom izborniku.
2. Potvrdite okvir Omogući POLUMJER provjere autentičnosti.
3. Na kartici klijente promijenite priključke za provjeru autentičnosti i računovodstveni priključke ako servisa Azure višestruke provjere autentičnosti POLUMJER treba povezati nestandardne priključke da biste preslušali za zahtjeve POLUMJER klijenata koji će se konfigurirati.
4. Kliknite Dodaj... gumb.
5. U dijaloškom okviru Dodavanje POLUMJER klijenta unesite IP adresu potražite/poslužitelja koji će provjeriti autentičnost Azure višestruku provjeru autentičnosti poslužitelja, naziv aplikacije (nije obavezno) i zajednički se koristi tajna. Dijeljena tajna bit će potrebno isti poslužitelj za Azure višestruke provjere autentičnosti i potražite/poslužitelja. Naziv aplikacije će se prikazati u izvješćima Azure višestruke provjere autentičnosti i može se prikazati unutar SMS ili mobilnu aplikaciju poruka provjere autentičnosti.
6. Potvrdite okvir zatraži višestruke provjere autentičnosti korisnika match ako svi korisnici su ili će biti uvezeni u poslužitelja i predmet višestruke provjere autentičnosti. Ako značajan broj korisnika još nisu uvezeni u poslužitelja ili bit će izuzeti iz pravilnika višestruka provjera autentičnosti, ostavite okvir poništen. Potražite datoteku pomoći za dodatne informacije o toj značajci.
7. Potvrdite okvir Omogući pričuvni OATH tokena ako će korisnici koristiti provjeru autentičnosti Azure višestruka provjera autentičnosti mobilne aplikacije, a želite koristiti OATH lozinke kao pričuvni provjere autentičnosti za-grupiranje telefonski poziv, SMS ili automatske obavijesti.
8. Kliknite gumb u redu.
9. Možda ponovite korake od 4 do 8 da biste dodali dodatne POLUMJER klijente.
10. Kliknite karticu odredišne.
11. Ako je na poslužitelju domene pridružili u okruženju Active Directory instaliran Azure višestruku provjeru autentičnosti poslužitelja, odaberite domenu za Windows.
12. Ako korisnici moraju autentičnost LDAP imenik, odaberite vezanja LDAP. Prilikom korištenja vezanja LDAP, morate kliknite ikonu Integracija direktorija i urediti konfiguracije LDAP na kartici postavke da bi poslužitelj možete povezati direktorija. Upute za konfiguriranje LDAP pronaći ćete u vodiču za konfiguraciju LDAP Proxy.
13. Ako korisnici moraju autentičnost drugi poslužitelj POLUMJER, odaberite POLUMJER poslužitelji.
14. Konfiguriranje poslužitelja koji poslužitelj će proxy POLUMJER zahtjevi za tako da kliknete Dodaj... gumb.
15. U dijaloškom okviru Dodavanje POLUMJER poslužitelja unesite IP adresu poslužitelja POLUMJER i zajednički se koristi tajna. Dijeljena tajna bit će potrebno na poslužitelj za Azure višestruke provjere autentičnosti poslužitelja i POLUMJER. Promijenite priključak za provjeru autentičnosti i priključak računovodstvenog ako koriste različite priključke POLUMJER poslužitelj.
16. Kliknite gumb u redu.
17. Morate dodati Azure višestruku provjeru autentičnosti poslužitelja kao POLUMJER klijenta na drugi poslužitelj za POLUMJER tako da se obradit će zahtjeva za pristup za slanje s Azure višestruke provjere autentičnosti poslužitelja. Morate koristiti istu dijeljena tajna konfiguriran u okvir poslužitelj za Azure višestruke provjere autentičnosti.
18. Možda Ponovite taj korak za dodavanje dodatnih POLUMJER poslužitelji i konfiguriranje redoslijedom u kojem poslužitelj nazovite ih s gumbima za premještanje gore i dolje. Time se dovršava konfiguracija Azure višestruke provjere autentičnosti poslužitelja. Poslužitelj sada priključuje na konfigurirani priključke za zahtjeva za pristup POLUMJER konfigurirani klijente.   


## <a name="radius-client-configuration"></a>Konfiguriranje klijenta POLUMJER

Da biste konfigurirali POLUMJER klijent, slijedite smjernice:

- Konfiguriranje potražite/poslužitelja za provjeru autentičnosti putem POLUMJER Azure višestruke provjere autentičnosti poslužitelja IP adresu koja će služiti kao POLUMJER poslužitelja.
- Koristite isti dijeljena tajna koji je konfiguriran iznad.
- Konfiguriranje vremenskog ograničenja POLUMJER u 30 do 60 sekundi tako da je vrijeme za provjeru vjerodajnica korisnika, izvođenje višestruka provjera autentičnosti, dobiti odgovor i odgovoriti na zahtjev za pristup POLUMJER.
