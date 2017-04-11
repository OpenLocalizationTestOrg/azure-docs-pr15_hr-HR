<properties
    pageTitle="Prijava iskustvo Azure AD identiteta zaštita | Microsoft Azure"
    description="Kada identiteta zaštita mitigated ili remediated korisnika ili višestruka provjera autentičnosti potreban je pravilima TechNet sadrži Pregled korisničkog sučelja."
    services="active-directory"
    keywords="Zaštita identiteta Azure active directory, otkrivanje aplikacije oblaka, Upravljanje aplikacijama, sigurnost, rizika, razina rizika, slabe, sigurnosna pravila"
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
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Prijava pomoću značajke zaštite Azure AD identiteta

Sa Azure Active Directory identiteta, možete učiniti sljedeće:

- od korisnika da biste registrirali za multi-factor authentication

- rukovanje opasan znak dodataka i ugrožena korisnika

Odgovor sustava te probleme ima utjecaj na sučelje za prijavu u korisničke jer samo izravno prijava u unosom korisničkog imena i lozinke neće biti moguće više. Dodatne korake potrebne za sigurno se korisnik zaključali tvrtke.

Ova tema sadrži pregled korisnikove prijave sučelja za sve slučajeve koji se mogu pojaviti.

**Višestruka provjera autentičnosti**

- Registracija višestruka provjera autentičnosti



**Prijavite se u adresi rizika**

- Opasan oporavak za prijavu

- Opasan prijavu blokiran

- Višestruka provjera autentičnosti Registracija tijekom na opasan prijavu
 

**Korisnik rizik**

- Oporavak ugrožena računa

- Račun za ugrožena blokiran




## <a name="multi-factor-authentication-registration"></a>Registracija višestruka provjera autentičnosti

Najbolji korisničko sučelje za oba na račun ugrožena oporavak i na opasan prijavu tijek, je kada korisnik koja se sama možete oporaviti. Ako korisnici registrirane za višestruke provjere autentičnosti, već imaju telefonski broj povezan s računa koje je moguće koristiti za prosljeđivanje izazove sigurnost. Da biste vratili iz narušena pouzdanost račun nije potrebno bez pomoći službe za korisnike ili administrator involvement. Stoga je preporučljivo da biste se registrirali za višestruke provjere autentičnosti korisnika. 

Administratori mogu:

- Postavljanje pravilnika za koje je potrebno korisnicima da biste postavili svoje račune radi dodatne sigurnosti provjere valjanosti. 
- omogućuju preskakanje višestruka provjera autentičnosti Registracija do 30 dana, za slučaj da žele korisnicima besplatno razdoblje prije registracije.

**Višestruka provjera autentičnosti Registracija sastoji se od tri koraka:**

1. U prvom koraku, korisnik može vidjeti obavijest o zahtjev za postavljanje računa za višestruke provjere autentičnosti. 

    ![Olakšava] (./media/active-directory-identityprotection-flows/140.png "Olakšava")


2. Da biste postavili višestruke provjere autentičnosti, morate omogućiti znati kako želite da se obratili.

    ![Olakšava] (./media/active-directory-identityprotection-flows/141.png "Olakšava")
 
3. Sustav šalje test da ste vi i treba odgovoriti.

    ![Olakšava] (./media/active-directory-identityprotection-flows/142.png "Olakšava")

 



## <a name="risky-sign-in-recovery"></a>Opasan oporavak za prijavu

Kada je administrator konfigurirao pravila za prijavu rizika, zahvaćene korisnike su obavijesti prilikom pokušaja prijave. 

**Tijek opasan prijavu sastoji se od dva koraka:** 

1. Korisnik se obavještavaju da nešto neobično otkriven o njihovim prijavu, kao što su prijavi na novo mjesto, uređaja ili aplikacija. 

    ![Olakšava] (./media/active-directory-identityprotection-flows/120.png "Olakšava")

2. Korisnik mora da biste dokazali svoj identitet po rješavanja sigurnosna pitanja i odgovora. Ako je registriran za višestruke provjere autentičnosti koje su im potrebne za round-trip sigurnosni kod za njegov telefonski broj. Budući da je to je samo opasan prijava u i ne compromised računa, korisnik neće imati da biste promijenili lozinku u ovom tijek. 

    ![Olakšava] (./media/active-directory-identityprotection-flows/121.png "Olakšava")



 
## <a name="risky-sign-in-blocked"></a>Opasan prijavu blokiran
Administratori možete odabrati i da biste postavili pravila odgovornost za prijavu na korisnicima onemogućite nakon prijave ovisno o razini rizika. Da biste dobili deblokiran, krajnjim korisnicima mora obratite se administratoru ili pomoć službe za korisnike ili možete pokušati prijave u poznate mjesto ili uređaj. Samostalno oporaviti tako da rješavanja višestruka provjera autentičnosti nije dostupna u tom slučaju.

![Olakšava] (./media/active-directory-identityprotection-flows/200.png "Olakšava")




## <a name="compromised-account-recovery"></a>Oporavak ugrožena računa

Kada korisničkih pravila zaštite rizika konfiguriran, korisnici koji zadovoljavaju korisnika rizikom razinu naveden u pravilo (i zato da su ugrožena) prije nego što se možete prijaviti, morate proći kroz tijek za oporavak narušena pouzdanost korisnika. 

**Oporavak tijek narušena pouzdanost korisnik ima tri koraka:**

1. Korisnik se obavještavaju da njihove sigurnost računa na rizik zbog sumnjivoj aktivnosti ili leaked vjerodajnice.

    ![Olakšava] (./media/active-directory-identityprotection-flows/101.png "Olakšava")

2.  Korisnik mora da biste dokazali svoj identitet po rješavanja sigurnosna pitanja i odgovora. Ako je registriran za multi-factor authentication ih možete oporaviti koja se sama iz se ugrožena. Oni morat ćete round-trip sigurnosni kod za njegov telefonski broj. 

    ![Olakšava] (./media/active-directory-identityprotection-flows/110.png "Olakšava")


3.  Na kraju, korisnik mora promijeniti lozinku jer je netko drugi imali pristup putem računa. Snimke zaslona ovog problema su ispod.
 
    ![Olakšava] (./media/active-directory-identityprotection-flows/111.png "Olakšava")



## <a name="compromised-account-blocked"></a>Račun za ugrožena blokiran 

Da biste korisnika koji je blokirao korisničkih rizika pravila zaštite deblokiranje, korisnik mora obratite se administratoru ili služba za podršku. Samostalno oporaviti tako da rješavanja višestruka provjera autentičnosti nije dostupna u tom slučaju.


![Olakšava] (./media/active-directory-identityprotection-flows/104.png "Olakšava")



 
## <a name="reset-password"></a>Ponovno postavljanje lozinke

Ako su korisnici ugrožena blokirane s prijavom, administrator mogu uzrokovati privremenu lozinku za njih. Korisnici će imati da biste promijenili lozinku tijekom na sljedeći prijavu.

![Olakšava] (./media/active-directory-identityprotection-flows/160.png "Olakšava")


 




 

## <a name="see-also"></a>Vidi također

- [Zaštita identiteta Azure Active Directory](active-directory-identityprotection.md) 