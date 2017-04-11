<properties
    pageTitle="Vrste događaja rizika otkrio Azure Active Directory identiteta zaštita | Microsoft Azure"
    description="U ovoj se temi pruža detaljne pregled dostupne vrste rizika događaja u Azure Active Directory identiteta zaštitu"
    services="active-directory"
    keywords="Zaštita identiteta Azure active directory, otkrivanje aplikacije oblaka, Upravljanje aplikacijama, sigurnost, rizika, razina rizika, slabe, sigurnosna pravila"
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
    ms.date="10/27/2016"
    ms.author="markvi"/>

#<a name="types-of-risk-events-detected-by-azure-active-directory-identity-protection"></a>Vrste događaja rizika otkrio Azure Active Directory identiteta zaštitu 

U Azure Active Directory identiteta zaštita rizik događaji su događaji koje:

- nisu označene kao sumnjiva

- pokazuje da identitet možda ste je ugrožena. 

Ova tema sadrži detaljne pregled dostupne vrste rizika događaja.


## <a name="leaked-credentials"></a>Licenciranjem vjerodajnice

Vjerodajnice za licenciranjem nalaze se na koju je objavio javno web-mjestu tamno istraživači Microsoft sigurnost su. Te vjerodajnice obično nalaze se u običan tekst. Mogu se traži vjerodajnice za Azure AD, a ako postoji rezultat, su prijavljena kao "Leaked vjerodajnica" u zaštitu identitet.

Leaked vjerodajnice rizika događaje klasificirani kao u "Visoka" događaj rizika težinu, jer pružaju Očisti oznaka koje su dostupne napadaču korisničko ime i lozinku.

## <a name="impossible-travel-to-atypical-locations"></a>Nemoguće putovanje atypical mjesta

Te vrste događaja rizik označava dva znak dodataka potječu geografski daleka mjesta gdje barem jedno od mjesta mogu biti atypical za korisnika, dala prošle ponašanje. Osim toga, vrijeme između dva dodaci znak je kraće vrijeme ga želite uzeti korisniku putovanja na prvom mjestu drugom koja označava da je drugi korisnik pomoću istih vjerodajnica. 

U ovom strojnog učenja algoritam koji zanemaruje očite "*false positives*" pojačava uvjeta nemoguće putovanja, kao što su VPN-ovima i mjesta redovito koriste ostali korisnici u tvrtki ili ustanovi.  Sustav ima razdoblje početne učenje 14 dana tijekom kojeg je Pratit će ponašanje za prijavu novog korisnika.

Nemoguće putovanja obično je dobar pokazatelj koji haker mogao uspješno prijavu. Međutim, false positives može dogoditi kada korisnik je putovanje pomoću novog uređaja ili putem VPN-a koji se obično ne koristi drugi korisnici u tvrtki ili ustanovi. Aplikacije koje neispravno prosljeđivanje poslužitelja IP-ovi kao klijentskog IP-ovi, koji može dati izgled je drugi izvor false positives od znak ins mjesto snimanja iz podatkovnog centra u kojem je tu aplikaciju pozadinskih nalazi (često to su Microsoft podatkovnim centrima koje možda daju izgled znak ins poduzimanja postavite od Microsofta čiji je IP adresa). Kao rezultat ove false – positives razinu rizika za ovaj događaj rizika je "**Srednje**".

##<a name="sign-ins-from-infected-devices"></a>Znak dodataka zaražene uređaja

Te vrste događaja rizika označava znak dodaci s uređaja špijunskim programom zlonamjernog softvera, koji gube aktivno komunikaciju s poslužiteljem robot. To ovisi o correlating IP adresa uređaja korisnika na temelju IP adrese nisu kontaktu s poslužiteljem robot. 

Ovaj događaj rizika označava IP adresa nije uređaje. Ako nekoliko uređaja nalaze iza jednom IP adresom, i neke su kontrolira robot mreži, znak dodaci s drugih uređaja Moje okidača ovaj događaj nepotrebno, koji je uzrok klasifikaciju ovaj događaj rizika kao "**Nisko**".  

Preporučujemo da se obratite se korisniku i pregled svih korisnika uređaja. Moguće je i da je špijunskim korisničke osobnim uređajem ili kao što je rečeno ranije, koji vam je netko drugi koristio zaražene uređaj s istu IP adresu kao korisnik. Zaražene uređaji često su špijunskim po zlonamjernog softvera koji još nisu određena antivirusni softver, a mogu upućivati i kao navike neispravni korisnika koji mogu uzrokovati uređaj da biste postaju špijunskim.

Dodatne informacije o softverom adresu zlonamjernog softvera potražite u članku [Centar za zaštitu od zlonamjernog softvera](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409).


## <a name="sign-ins-from-anonymous-ip-addresses"></a>Znak dodaci s anonimnim IP adresa

Ova vrsta događaja rizik označava korisnicima koji imaju uspješno prijavljeni s IP adresu koja je označen kao anonimni proxy IP adresu. Ove proxy poslužitelji koriste osobe koje želite sakriti IP adresa za svoj uređaj, a mogu se koristiti za zloupotrebu.

Preporučujemo da odmah o korisniku da biste provjerili ako ste koristili anonimni IP adrese. Razina rizika za tu vrstu događaja rizika je "**Srednje**" zato sam po sebi anonimni IP ne istaknuti oznaka narušena pouzdanost na račun.

## <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Znak dodataka iz IP adresa pomoću sumnjivoj aktivnosti

Ova vrsta događaja rizika označava IP adrese iz kojeg velik broj neuspjelih pokušaja prijave su vidjeti, preko više korisničkih računa, kratki vremenskom razdoblju. To odgovara promet uzorcima IP adrese koristi Napadači, a to je istaknuti pokazatelj računi su već, ili ćete biti ugrožena. Ovo je strojnog učenja algoritam koji zanemaruje očite "*false positives*", kao što je IP adresa koje redovito služe drugim korisnicima u tvrtki ili ustanovi.  Sustav ima razdoblje početne učenje 14 dana gdje ga Pratit će ponašanje prijavu novog korisnika i novi klijent.

Preporučujemo da se obratite se korisniku da biste provjerili ako oni zapravo prijavljeni s IP adresom koja je označena kao sumnjiva. Razina odgovornost za tu vrstu događaja je "**Srednje**" jer nekoliko uređaja možda iza istu IP adresu dok samo neke mogu biti zaduženi za sumnjivoj aktivnosti. 


## <a name="sign-in-from-unfamiliar-locations"></a>Prijava s nepoznatim mjesta

Ta rizika događaj je vrsta mehanizam za procjenu u stvarnom vremenu prijavu koji smatra prošle mjesta za prijavu (IP, zemljopisnu širinu i dužinu i ASN) da biste odredili novi / nepoznatim mjesta. Sustav pohranjuje informacije o prethodna mjesta koji se koristi za korisnika i smatra tih "poznatih" mjesta. Rizik čak i aktivira kada se prijavite u s mjesta koje još nije na popisu poznatih mjesta. Sustav ima razdoblje početne učenje 14 dana tijekom kojeg je Označi sve nova mjesta kao nepoznatim lokacije. Sustav zanemaruje i znak dodataka iz poznatih uređaji i mjesta koja geografski blizu poznatih mjesto. <br>
Nepoznatim mjesta unijeti istaknuti indikator koji napadaču može pokušaja korištenja ukradeno identitet. FALSE positives može dogoditi kada korisnik je na putu, isprobavate novi uređaj ili koristi novi VPN-a. Kao rezultat ove false positives razinu rizika za tu vrstu događaj je "**Srednje**".


## <a name="azure-ad-anomalous-activity-reports"></a>Azure AD Anomalous aktivnosti izvješća

Neke od tih rizika događaja da su dostupne putem Azure AD Anomalous aktivnosti izvješća na portalu za Azure. U tablici u nastavku navedeni različitih vrsta događaja rizik i odgovarajuće **Azure AD Anomalous aktivnosti** izvješća. Microsoft se nastavlja na ulažete u prostor i tarife da biste kontinuirano poboljšajte točnost prepoznavanja postojeće događaje rizik i dodali nove vrste događaja rizika pridonositi. 



| Vrsta događaja rizika zaštite za identitet | Odgovarajući Azure AD izvješće o Anomalous aktivnosti |
| :-- | :-- |
| Licenciranjem vjerodajnice    | Korisnici s licenciranjem vjerodajnice |
| Nemoguće putovanje atypical mjesta | Nepravilni aktivnosti za prijavu |
| Znak dodataka zaražene uređaja    | Znak dodataka vjerojatno zaražene uređaja |
| Znak dodaci s anonimnim IP adresa  | Znak dodataka iz nepoznatih izvora |
| Znak dodataka iz IP adresa pomoću sumnjivoj aktivnosti | Znak dodataka iz IP adresa pomoću sumnjivoj aktivnosti |
| Znak u s nepoznatim mjesta    | - |
| Lockout događaja    | - |

Sljedeće izvješća Azure AD Anomalous aktivnosti nisu obuhvaćeni kao rizika događaji u Azure AD identiteta zaštitu i zbog toga neće biti dostupne putem identiteta zaštitu. Ta izvješća i dalje su dostupni na portalu za Azure no oni će biti ukinuta na neko vrijeme u budućnosti kao oni su se zamijenila rizika događaja u zaštitu identitet.

- Znak dodataka nakon većeg broja neuspješnih pokušaja
- Znak dodataka iz više geographies


## <a name="see-also"></a>Vidi također

- [Zaštita identiteta Azure Active Directory](active-directory-identityprotection.md)


