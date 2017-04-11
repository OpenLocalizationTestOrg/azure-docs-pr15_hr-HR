<properties
    pageTitle="Pojmovnik za zaštitu identiteta Azure Active Directory | Microsoft Azure"
    description="Pojmovnik za zaštitu identiteta Azure Active Directory"
    services="active-directory"
    keywords="Otkrivanje aplikacije oblaka, Upravljanje aplikacijama, sigurnost, rizika, razina rizika, slabe, sigurnosna pravila, Pojmovnik Azure active directory identiteta zaštitu"
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

# <a name="azure-active-directory-identity-protection-glossary"></a>Pojmovnik za zaštitu identiteta Azure Active Directory 


### <a name="at-risk-user"></a>Rizik (korisnik)  
Korisnik s jednog ili više aktivni rizika događajima. 

### <a name="atypical-sign-in-location"></a>Atypical mjesto za prijavu   
Na prijava s zemljopisnog mjesta koja nije uobičajeni za određenog korisnika, slično korisnika ili na klijentu.

### <a name="azure-ad-identity-protection"></a>Zaštita Azure AD identiteta    
Sigurnost modul Azure Active Directory koja omogućuje prikaz Konsolidirani u događaja rizik i potencijalne slabe točke utjecaja identiteta u tvrtki ili ustanovi.

### <a name="conditional-access"></a>Uvjetno programa access  
Pravila za osiguravanje pristupa resursima. Pravila za uvjetno pristup se pohranjuju u Azure Active Directory i vrednuju Azure AD prije nego date pristup resursu.  Primjer pravila sadrže ograničavanje pristupa na temelju lokacije korisnika uređaj korisnika ili stanje načina provjere autentičnosti.

### <a name="credentials"></a>Vjerodajnice     
Informacije koje obuhvaća identifikaciju i dokaz identifikacijski koja se koristi za pristup lokalnog i mrežni resursi. Primjeri vjerodajnice su korisnička imena i lozinke, pametne kartice i certifikati.

### <a name="event"></a>Događaja   
Zapis aktivnosti u Azure Active Directory.

### <a name="false-positive-risk-event"></a>FALSE pozitivnog (rizika događaj) 
Status rizika događaja ručno postaviti zaštitu identitet korisnika, koja navodi događaja rizika je imaju, a neispravno označena kao rizika događaja.

### <a name="identity"></a>Identitet    
Osoba ili entitet koji morate provjeriti pomoću provjere autentičnosti na temelju kriterija kao što su lozinka ili certifikat.

### <a name="identity-risk-event"></a>Identitet rizika događaja     
AAD događaja koji je označena kao anomalous zaštitu identiteta, a može značiti da je identitet ugrožena.

### <a name="ignored-risk-event"></a>Zanemaruje (rizika događaj)    
Status rizika događaja ručno postaviti zaštitu identitet korisnika, koji označava događaja rizika zatvoren bez poduzimanja akcija za olakšava.

### <a name="impossible-travel-from-atypical-locations"></a>Nemoguće putovanja s atypical mjesta   
Rizik događaja koji se aktiviraju kada dva znak dodatke za istoga korisnika otkrije, pri čemu barem jedan je od atypical mjesta prijavu, a pri čemu je vrijeme između dodataka znak kraće minimalno će trajati fizički putovanja između tih lokacija.  

###<a name="investigation"></a>Istraživanja    
Postupak pregledavanje aktivnosti, zapisnika i ostale važne informacije vezane uz rizika događaj odlučiti hoće li se olakšava ili ublažiti korake potrebno, razumijevanje ako i načinu na koji identitet je ugrožena i razumijevanje korištenja compromised identitet.

### <a name="leaked-credentials"></a>Licenciranjem vjerodajnice  

Rizik događaja koji se aktivira kada se pronađu vjerodajnice trenutnog korisnika (korisničko ime i lozinka) u koju je objavio javno web-mjestu tamno naše istraživači su.

### <a name="mitigation"></a>Ublažiti  
Radnja da biste ograničili ili uklonili mogućnost napadaču izrabljuje ugrožena identiteta ili uređaj bez vraćanja identiteta ili uređaj u sigurnom stanje. Na ublažiti riješiti prethodne rizika događaje vezane uz identiteta ili uređaj.

### <a name="multi-factor-authentication"></a>Višestruka provjera autentičnosti 
Način provjere autentičnosti koje je potrebno dva ili više metoda provjere autentičnosti, koji mogu sadržavati nešto korisnik ima, takve potvrde; nešto korisnika zna, kao što su korisnička imena, lozinke ili pristupni izrazi; Fizička atribute, kao što su otisak prsta; i osobne atribute, kao što su osobnog potpisa.

### <a name="offline-detection"></a>Otkrivanje izvan mreže   
Otkrivanje anomalies i procjenu rizika događaja kao što je pokušaj prijave nakon toga, za događaj koji već dogodilo.

### <a name="policy-condition"></a>Uvjet pravila    
Dio sigurnosna pravila koja definira entiteti (grupe, korisnici, aplikacije, platforme uređaja, stanja uređaja, IP rasponi, vrstama klijenata) uvrstiti u pravilo ili izuzeti iz nje.

### <a name="policy-rule"></a>Pravilo     
Dio sigurnosna pravila koja opisuje okolnostima koji bi aktiviraju pravila, a akcija izvedena kada se pokrene pravila.

### <a name="prevention"></a>Sprječavanje  
Akcije da biste spriječili oštećenje tvrtke ili ustanove putem zloupotreba identiteta ili uređaja sumnjivu ili bi biti ugrožena. Sprječavanje akcija sigurne uređaja ili identitet, a ne riješite prethodne rizika događaja.

### <a name="privileged-user"></a>Povlaštene (korisnik)
Korisnika koji se u trenutku u događaj rizika imali trajnih ili privremenih administratorske dozvole za jedan ili više resursa u Azure Active Directory, kao što je globalni Administrator, Administrator naplate, Administrator servisa, administrator korisnika i administratora za lozinke. 

###<a name="real-time"></a>U stvarnom vremenu    
Potražite u članku otkrivanje u stvarnom vremenu.

###<a name="real-time-detection"></a>Otkrivanje u stvarnom vremenu  
Da biste nastavili je dopušteno otkrivanje anomalies i procjenu rizika događaja kao što je pokušaj prijave prije događaja.

### <a name="remediated-risk-event"></a>Remediated (rizika događaj)     
Status rizika događaja automatski postavlja zaštitu identiteta, koja označava da je događaja rizika remediated pomoću akcije standardne olakšava za ovu vrstu događaja rizika. Ako, na primjer, kada korisnik je za ponovno postavljanje lozinke, broj rizika događaja koje označavaju da je prethodna lozinka ugrožena se automatski remediated.

### <a name="remediation"></a>Olakšava     
Radnja sigurnost identitet ili uređaj koji su prethodno sumnjivu ili zna da biti ugrožena. Olakšava akcija vraća identiteta ili uređaj sigurni stanje, a razrješava prethodne rizika događaje vezane uz identiteta ili uređaj.

### <a name="resolved-risk-event"></a>Riješite (rizika događaj)   
Status rizika događaja ručno postavljanje Protection identiteta korisnicima, koja označava da korisnik snimljene akciju odgovarajuće olakšava izvan zaštitu identiteta, a da događaja rizika razmatranje zatvoren.

###<a name="risk-event-status"></a>Status rizika događaja    
Svojstvo događaja rizika, koji označava hoće li se aktivna događaja, a ako Zatvoreno, razlog zatvaranja.

###<a name="risk-event-type"></a>Vrsta događaja rizika  
Kategorija za događaj rizika, koji označava vrstu značajkom koje su uzrokovale događaja smatraju opasan.

###<a name="risk-level-risk-event"></a>Razina rizika (rizika događaj)  
Oznaka (visoke, Srednje ili najniža) težinu rizika događaja da bi korisnicima identiteta zaštita prioritet akcije proizvela smanjenju rizika svoje tvrtke ili ustanove. 

###<a name="risk-level-sign-in"></a>Razina rizika (Prijava) 
Oznaka (visoke, Srednje ili najniža) vjerojatnost da za na određene prijavu, netko drugi pokušava koristiti identitet korisnika.

###<a name="risk-level-user-compromise"></a>Razina rizika (narušena pouzdanost korisnika) 
Oznaka (visoke, Srednje ili najniža) vjerojatnost da je identitet ugrožena.

### <a name="risk-level-vulnerability"></a>Razina rizika (slabe)  
Oznaka (visoke, Srednje ili najniža) težinu slabe da bi korisnicima identiteta zaštita prioritet akcije proizvela smanjenju rizika svoje tvrtke ili ustanove.

### <a name="secure-identity"></a>Sigurne (identitet)   
Olakšava akcija kao što su promjena lozinke ili računalu reimaging da biste vratili potencijalno compromised identiteta neometanog stanje.

### <a name="security-policy"></a>Sigurnosna pravila
Skup pravila i uvjet. Pravilo primjenjuje se na entiteti kao što su korisnika, grupe, aplikacije, uređajima, platforme uređaja, stanja uređaja, IP rasponi i vrstama klijenata za Auth2.0. Kada je omogućen pravilo, vrednuje se kad god entitet uvrstiti u pravilo izdaje token resursa.

### <a name="sign-in-v"></a>Prijavite se u (v) 
Za provjeru autentičnosti identiteta servisa Azure Active Directory.

### <a name="sign-in-n"></a>(N) za prijavu 
Postupak ili akciju provjere autentičnosti identiteta u Azure Active Directory i događaj koji opisuje ovaj postupak.

###<a name="sign-in-from-anonymous-ip-address"></a>Prijava s anonimnim IP adrese    
Rizik događaja koji se prikazuje kada je uspješno prijavu iz IP adresa koji je označen kao anonimni proxy IP adresu.

###<a name="sign-in-from-infected-device"></a>Prijava putem zaražene uređaja 
Rizik događaja koji se aktiviraju kada je prijavu potječe IP adresa koji se nazivaju se može koristiti u jednu ili više ugrožena uređaja koji je aktivno pokušaja komunikacije s poslužiteljem robot.

###<a name="sign-in-from-ip-address-with-suspicious-activity"></a>Prijava na IP adresa s sumnjivoj aktivnosti 
Rizik događaja koji se prikazuje kada je uspješno prijavu iz IP adresa s velik broj pokušaja prijave nije uspjelo preko više korisničkih računa kratki vremenskom razdoblju.

###<a name="sign-in-from-unfamiliar-location"></a>Prijava s nepoznatim mjesta 
Rizik događaja koji se aktiviraju kada korisnik uspješno prijavi na novo mjesto (IP, zemljopisnu širinu i dužinu i ASN).

###<a name="sign-in-risk"></a>Rizik za prijavu     
U odjeljku rizika razine (Prijava)

###<a name="sign-in-risk-policy"></a>Pravila odgovornost za prijavu  
Pristup uvjetnog pravila koja vrednuje rizika na određene prijaviti i primjenjuje mitigations utemeljenog na unaprijed definirane Uvjeti i pravila.

###<a name="user-compromise-risk"></a>Rizik narušena pouzdanost korisnika     
U odjeljku rizika razine (narušena pouzdanost korisnika)

###<a name="user-risk"></a>Rizik korisnika    
U odjeljku rizika razine (narušena pouzdanost korisnika).

###<a name="user-risk-policy"></a>Korisnička rizika pravila     
Pristup uvjetno pravilo koje smatra prijavu i primjenjuje mitigations utemeljenog na unaprijed definirane Uvjeti i pravila.

###<a name="users-flagged-for-risk"></a>Korisnici koji su označene za rizika   
Korisnici koji imaju rizika događaje koje su aktivna ni remediated

###<a name="vulnerability"></a>Slabe    
Konfiguraciju ili uvjeta u Azure Active Directory čega direktoriju susceptible da biste opasnosti ili prijetnji.


## <a name="see-also"></a>Vidi također

- [Zaštita identiteta Azure Active Directory](active-directory-identityprotection.md)
