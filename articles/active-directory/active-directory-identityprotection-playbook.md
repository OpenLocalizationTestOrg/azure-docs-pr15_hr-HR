<properties
    pageTitle="Azure Active Directory identiteta zaštita playbook | Microsoft Azure"
    description="Saznajte kako Azure AD identiteta zaštita omogućuje vam ograničiti mogućnost napadaču izrabljuje ugrožena identiteta ili uređaj i sigurne identitet ili uređaj koji je prethodno sumnjivu ili zna da biti ugrožena."
    services="active-directory"
    keywords="Zaštita identiteta servisa Azure active directory, otkrivanje aplikacija oblaka, Upravljanje aplikacijama, sigurnost, rizika, razina rizika, slabe, sigurnosna pravila"
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
    ms.date="08/22/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory identiteta zaštita playbook 

U ovom playbook pomaže vam:

- Popunjavanje podataka u okruženju identitet zaštitu simulaciju događaje rizik i slabe točke
- Postavljanje pravila za pristup uvjetno utemeljen na rizik i testiranje utjecaj ta pravila


## <a name="simulating-risk-events"></a>Simulaciju rizika događaja

U ovom se odjeljku nudi korake za simulaciju sljedećim vrstama rizika događaja:

- Znak dodataka iz anonimni IP adrese (jednostavno)
- Znak dodaci s nepoznatim mjesta (moderiranje)
- Nemoguće putovanje atypical mjesta (teško)

Druge događaje rizika ne može biti Simulirani siguran način.


### <a name="sign-ins-from-anonymous-ip-addresses"></a>Znak dodaci s anonimnim IP adresa

Te vrste događaja rizika označava korisnicima koji imaju uspješno prijavljeni s IP adresu koja je označen kao anonimni proxy IP adresa. Ove proxy poslužitelji koriste osobe koje želite sakriti svoj uređaj IP adresa i može se koristiti za zloupotrebu.

**U programu Publisher na prijava s anonimnim IP, učinite sljedeće**:

1.  Preuzmite [Tor preglednika](https://www.torproject.org/projects/torbrowser.html.en).
2.  Pomoću preglednika Tor, pomaknite se do [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3.  Unesite vjerodajnice za račun će se prikazivati u izvješću **znak dodaci s anonimnim IP adresa** .

Prijavite se u prikazat će se na nadzornoj ploči za zaštitu identiteta za 5 minuta. 


###<a name="sign-ins-from-unfamiliar-locations"></a>Znak dodaci s nepoznatim mjesta

Rizik nepoznatim mjesta je mehanizam za procjenu u stvarnom vremenu prijavu koji smatra prošle mjesta za prijavu (IP, zemljopisnu širinu i dužinu i ASN) da biste odredili novi / nepoznatim mjesta. Sustav pohranjuje prethodne IP-ovi, zemljopisnu širinu i dužinu i ASNs korisnika i smatra ih biti upoznati mjesta. Mjesto za prijavu smatra nepoznatim ako mjesto za prijavu ne odgovaraju nijednom postojećih poznatih lokacija.

Zaštita identiteta Azure Active Directory:  

 - sadrži razdoblje početne učenje 14 dana tijekom kojeg je Označi sve nova mjesta kao nepoznatim lokacije.
 - zanemaruje znak dodataka iz poznatih uređaji i mjesta koja geografski blizu postojeće poznatih mjesto.

U programu Publisher nepoznatim mjesta, morate prijaviti s mjesto i uređaj koji račun sadrži niste prijavljeni s prije. 


**U programu Publisher na prijava s nepoznatim mjesta, učinite sljedeće**:

1.  Odaberite račun koji ima najmanje 14 dana prijavu povijest. 

2.  Učinite nešto od sljedećeg:
    
    na. Prilikom korištenja VPN, dođite do [https://myapps.microsoft.com](https://myapps.microsoft.com) i unesite vjerodajnice za račun koji želite kao zamjenu za događaj rizika.

    b. Obratite se povezati na neko drugo mjesto za prijavu pomoću vjerodajnica na račun (ne preporučuje se).

Prijavite se u prikazat će se na nadzornoj ploči za zaštitu identiteta za 5 minuta.
 
### <a name="impossible-travel-to-atypical-location"></a>Nemoguće putovanja atypical mjesto
Simulaciju uvjeta nemoguće putovanja teško je jer algoritam koristi strojnog učenja za weed out false positives kao što su nemoguće putovanja s poznatih uređaja ili znak dodataka iz VPN-ovi koje koristi drugih korisnika u imeniku. Uz to, algoritam zahtijeva povijest prijavu od 3 do 14 dana za korisnika prije započinje generiranje događaji rizika.

**U programu Publisher nemoguće putovanja atypical mjesto, učinite sljedeće**:

1.  Dođite do [https://myapps.microsoft.com](https://myapps.microsoft.com)pomoću standardne preglednika.  

2.  Unesite vjerodajnice za račun koji želite generirati događaja nemoguće putovanja odgovornost za.

3.  Promijenite korisnički agent. Možete promijeniti korisnički agent u pregledniku Internet Explorer iz razvojnih alata ili promijeniti korisnički agent u pregledniku Firefox ili Chrome korištenjem dodatka za alat za prebacivanje korisnički agent.

4.  Promijenite IP adresa. IP adrese možete promijeniti putem virtualne privatne MREŽE, dodatak programa Tor ili koji se vrti gore novo računalo u Azure u centru za različite podatke.

5.  Prijava za [https://myapps.microsoft.com](https://myapps.microsoft.com) kao prije pomoću istih vjerodajnica i za nekoliko minuta nakon prijave prethodne.

Prijavite se u prikazat će se na nadzornoj ploči identitet zaštite u roku od 2 do 4 sati.<br>
Zbog na složenim strojnog učenja modela koji je uključen, postoji mogućnost da ga neće dobiti sakrije prema gore.<br> Možda ćete morati replicirati ove upute za više Azure AD računa.


## <a name="simulating-vulnerabilities"></a>Simulaciju slabe točke 
Slabe točke su slabosti u okruženju Azure AD koje možete napasti bad glumca. Trenutno u Azure AD identiteta zaštita pod utjecajem ostalih značajki Azure AD naredbama 3 vrste slabe točke. Ove slabe točke prikazat će se na nadzornoj ploči za zaštitu identiteta automatski kada su te značajke postavite.

-   Azure AD [višestruka provjera autentičnosti?](../multi-factor-authentication/multi-factor-authentication.md)
-   Azure AD [Otkrivanje aplikacija oblaka](active-directory-cloudappdiscovery-whatis.md).
-   [Upravljanje identitetom povlaštene](active-directory-privileged-identity-management-configure.md)Azure AD. 



##<a name="user-compromise-risk"></a>Rizik narušena pouzdanost korisnika

**Da biste testirali opasno narušena pouzdanost korisnika, učinite sljedeće**:

1.  Prijava na [https://portal.azure.com](https://portal.azure.com) s vjerodajnicama globalni administrator klijenta.

2.  Dođite do **identiteta zaštitu**. 

3.  Na glavnom plohu **Azure AD identiteta zaštita** kliknite **Postavke**. 

4.  Na plohu **Portal postavke** u odjeljku **Sigurnost pravila**kliknite **korisnički ugroziti rizika**. 

5.  Na plohu **prijavite rizika** isključite **omogućite pravilo** , a zatim **Spremi** postavke.

6.  Za dani korisničkog računa, kao zamjenu za nepoznatim mjesta ili anonimne IP rizika događaja. To će **Srednje**pretvoriti rizika razina korisničke za tog korisnika.

7.  Pričekajte nekoliko minuta, a zatim provjerite je li korisnik razinu korisnički **Srednje**.

8.  Idite na **Postavke Videoportala** plohu.

9.  **Na plohu **Rizika narušena pouzdanost korisnika** , u odjeljku **omogućite pravilo**, odaberite.** 

10. Odaberite jednu od sljedećih mogućnosti:

    na. Da biste blokirali, odaberite **Srednje** u odjeljku **Blokiranje prijavite**.

    b. Da biste nametnuli promjena sigurne lozinke, odaberite **Srednje** u odjeljku **zahtijevaju višestruke provjere autentičnosti**.

13. Kliknite **Spremi**.

14. Sada možete testirati i rizika temelje uvjetno access tako da se prijavite u korisnika pomoću razinu povećane rizika. Ako je korisnik rizika srednje, ovisno o konfiguraciji pravilnika, vaše prijavu je ili blokirati ili su prisilno za promjenu lozinke. 
<br><br>
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
<br>

 
##<a name="sign-in-risk"></a>Odgovornost za prijavu

 
**Da biste testirali znak u rizika, učinite sljedeće:**

1.  Prijava na [https://portal.azure.com](https://portal.azure.com) s vjerodajnicama globalni administrator klijenta.

2.  Dođite do **identiteta zaštitu**.

3.  Na glavnom plohu **Azure AD identiteta zaštita** kliknite **Postavke**. 

4.  Na plohu **Portal postavke** u odjeljku **Sigurnost pravila**kliknite **prijavite se u rizika**.

5.  Na plohu **prijavite rizika **odaberite **na** pod **omogućite pravilo**. 

7.  Odaberite jednu od sljedećih mogućnosti:

    na. Da biste blokirali, odaberite **Srednje** u odjeljku **Prijava bloka**

    b. Da biste nametnuli promjena sigurne lozinke, odaberite **Srednje** u odjeljku **zahtijevaju višestruke provjere autentičnosti**.

8.  Da biste blokirali, odaberite Srednje ispod bloka za prijavu.

9.  Da biste nametnuli višestruka provjera autentičnosti, odaberite **Srednje** u odjeljku **zahtijevaju višestruke provjere autentičnosti**.

10. Kliknite **Spremi**.

11. Sada možete testirati i pristup uvjetno utemeljen na rizika po simulaciju nepoznatim mjesta ili anonimne događaje rizika IP jer su oba **Srednje** rizika događaja.

<br>
![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")
<br>


## <a name="see-also"></a>Vidi također

 - [Zaštita identiteta Azure Active Directory](active-directory-identityprotection.md)
