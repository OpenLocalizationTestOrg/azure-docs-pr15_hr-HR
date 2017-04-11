<properties
    pageTitle="Azure Active Directory hibridnog identiteta zahtjevi dizajna – definiranje Strategije za zaštitu podataka | Microsoft Azure"
    description="Ćete definirati strategija zaštite podataka za vaše rješenje hibridnog identiteta zadovoljava preduvjete za tvrtke koji ste definirali."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Definiranje strategije zaštite podataka za identitet rješenje hibridnog

U ovom ćete zadatku ćete definirati Strategije za zaštitu podataka za hibridno identiteta rješenje za zadovoljava preduvjete za tvrtke koji ste definirali u:

- [Određivanje potrebama za zaštitu podataka](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
- [Određivanje preduvjeti za upravljanje sadržajem](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
- [Određivanje preduvjeti za kontrolu pristupa](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
- [Određivanje incidenta odgovor preduvjeti](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Definiranje mogućnosti za zaštitu podataka
Kao što je opisano je u [članku preduvjeti za sinkronizaciju direktorija za određivanje](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md), Microsoft Azure AD mogu se sinkronizirati s vašeg Active Directory Domain Services (AD DS) koja se nalazi na lokalni. Ta integracija omogućuje tvrtkama ili ustanovama odražava Azure AD da biste provjerili korisnikove vjerodajnice kada pokušavate pristupiti resursima tvrtke. To možete učiniti za oba scenarija: podataka na ostale lokalne i u oblaku.  Pristup podacima u Azure AD zahtijeva provjeru autentičnosti korisnika putem servis sigurnosnih tokena (STS).

Nakon provjere autentičnosti korisnikovo Glavno ime (UPN) je čitanja token za provjeru autentičnosti i repliciranu particija i ovisi o kontejneru odgovaraju korisnikove domene. Autorizacija sustav koristi podatke o korisnikovoj postojanje, stanja i ulogama za određivanje hoće li tražene pristup na ciljnom klijentu dozvolu za tog korisnika u ovu sesiju. Određene akcije ovlašteni (Konkretno, korisnika, ponovno postavljanje lozinke za stvaranje) stvaranje popratnu koje je moguće koristiti administrator klijenta za upravljanje usklađenost naporima ili istrage.

Premještanje podataka iz lokalnog Standard u prostor za pohranu Azure putem internetske veze uvijek možda nije izvedivo glasnoću podataka, dostupnost propusnosti ili drugih pitanja vezana uz. [Servis za uvoz/izvoz za pohranu Azure](../storage/storage-import-export-service.md) omogućuje hardverski mogućnosti za smještanje/dohvaćanje velike količine podataka u spremište blobova platforme. Omogućuje slanje diskovnih pogona [BitLocker šifrirane](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) tvrdom izravno Azure Standard gdje oblaka operatora će prijenos sadržaj na račun servisa za pohranu i preuzimanje Azure podataka pogonima da biste se vratili na. Samo šifrirane diskova prihvaćaju za taj proces (pomoću BitLocker ključ generira sam servis tijekom postavljanja posla). Ključ BitLocker dostupni su Azure zasebno, što pruža iz područja ključa zajedničko korištenje.

Budući da podatke na putu može obaviti u različitim scenarijima, također se relevantne informacije Microsoft Azure koristi [virtualne umrežavanje](https://azure.microsoft.com/documentation/services/virtual-network/) radi izdvajanja promet klijenata, employing mjera kao što su vatrozida za glavno računalo i goste razinu, filtriranje paketa za IP, priključak blokiranje i HTTPS krajnje točke. Međutim, većina Interna komunikacije Azure korisnika, uključujući infrastrukture za infrastrukturu i infrastrukture kupca primatelja (lokalno), i šifriran. Drugi scenarij važno je komunikacije unutar Azure podatkovnim centrima; Microsoft upravlja mreže da biste dobili bez VM možete oponašati ili eavesdrop na IP adresu drugog. TLS/SSL koristi prilikom pristupanja Azure prostora za pohranu ili baze podataka SQL ili prilikom povezivanja sa servisa u Oblaku. U ovom slučaju je administrator klijenta odgovoran za dobivanje TLS/SSL certifikata i implementacija njihove infrastrukture klijenta. Podataka promet pomicanje među virtualnim strojevima isti implementacije ili drugih korisnika u jednom implementaciju putem virtualne mreže Microsoft Azure možete zaštititi i putem protokola šifrirane komunikacije, kao što su HTTPS, SSL/TLS i drugim korisnicima.

Ovisno o tome kako odgovoriti na pitanja u [Određivanje potrebama za zaštitu podataka](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md), mora biti da biste odredili način na koji želite zaštititi podatke i kako rješenje identiteta hibridnog pomoći će vam na koji. U tablici prikazane su mogućnosti podržava Azure koji su dostupni za svaki scenarij za zaštitu podataka.


| Mogućnosti za zaštitu podataka         | Na ostale u oblaku | Na ostale lokalne | Tijekom prijenosa |
|---------------------------------|----------------------|---------------------|------------|
| BitLocker šifriranje pogona      | X                    | X                   |            |
| SQL Server za šifriranje baze podataka | X                    | X                   |            |
| Šifriranje VM VM             |                      |                     | X          |
| SSL/TLS                         |                      |                     | X          |
| VPN-A                             |                      |                     | X          |


>[AZURE.NOTE]
Pročitajte [usklađenosti pomoću značajke](https://azure.microsoft.com/support/trust-center/services/) u [Centru za pouzdanost Microsoft Azure](https://azure.microsoft.com/support/trust-center/) da biste se podrobnije informirali o certifications koja je usklađena sa svakog servisa Azure.
Budući da se mogućnosti za zaštitu podataka pomoću multilayer pristup, usporedbe između tih mogućnosti nisu primjenjivi za taj zadatak. Provjerite da su korištenje svih mogućnosti dostupne za svakog pojedinog statusa koji će biti podatke.

## <a name="define-content-management-options"></a>Definiranje mogućnosti za upravljanje sadržajem
Jedna od prednosti korištenja Azure AD za upravljanje infrastruktura za identitet hibridnog je postupak je potpuno prozirno iz perspektive krajnjeg korisnika. Korisnik će pokušati pristupiti zajedničkim resursom, resurs zahtijeva provjeru autentičnosti, korisnik će imati da biste poslali zahtjev za provjeru autentičnosti Azure AD da biste dobili token i pristup resursu. Cijelog postupka se događa u pozadini, bez interakcije s korisnikom. I moguće je da biste dodijelili dozvolu za [grupu](active-directory-manage-groups.md#getting-started-with-access-management) korisnika da bi im za obavljanje određenog uobičajenih akcija.

Tvrtkama ili ustanovama koje su složen o privatnosti podataka obično zahtijeva podatke klasifikacija njihova rješenja. Ako svoje trenutne infrastrukture lokalnog već koristi klasifikacija podataka, moguće je odražava Azure AD kao glavni spremište za identitet korisnika. Alat za uobičajene je korištenih lokalnog za klasifikaciju podataka zove [Komplet alata za klasifikaciju podataka](https://msdn.microsoft.com/library/Hh204743.aspx) za Windows Server 2012 R2. Ovaj alat može pomoći za prepoznavanje, klasifikaciju i zaštititi podatke na poslužiteljima datoteka u vaše privatne oblaka. Također moguće je odražava [Klasifikacija datoteka](https://technet.microsoft.com/library/hh831672.aspx) u sustavu Windows Server 2012 da biste to postigli.

Ako vašoj tvrtki ili ustanovi nema podataka klasifikacija na mjestu, ali potrebno zaštiti osjetljivih datoteka bez dodavanja novog poslužitelje lokalnog, možete koristiti Microsoft [Usluga upravljanja pravima za Azure](https://technet.microsoft.com/library/JJ585026.aspx).  Azure RMS koristi šifriranje, identiteta i pravila za provjeru autentičnosti radi zaštite vaše datoteke i e-pošte, a funkcionira na više uređaja – telefonima, tabletima i računalima. Budući da Azure RMS servis u oblaku, nema potrebe da biste konfigurirali izričito trusts s drugim tvrtkama ili ustanovama prije zaštićenog sadržaja možete zajednički koristiti s njima. Ako već imaju račun za Office 365 ili u imeniku Azure AD, podržana je automatski suradnju preko tvrtke ili ustanove. Ne možete sinkronizirati samo atributa direktorija koji Azure RMS treba podrške uobičajenih identiteta za svoje račune servisa Active Directory za lokalni pomoću servisa Azure Active Directory sinkronizacije Services (AAD Sync) ili Azure AD Connect.

Ključni dio upravljanje sadržajem je razumjeti tko pristupa koja resursa, stoga je važno da rješenje za upravljanje identitetom obogaćenog zapisivanje mogućnost. Azure AD zapisnika omogućuje veće od 30 dana uključujući:

- Promjene u članstvo u ulozi (ex: korisnik dodan uloga globalnog administratora)
- Vjerodajnica ažuriranja (ex: promjene lozinke)
- Upravljanje domenom (ex: Provjera prilagođenu domenu, ukloniti domene)
- Dodavanje ili uklanjanje aplikacija
- Upravljanje korisnicima (ex: dodavanjem, uklanjanjem, ažuriranje korisnika)
- Dodavanje ili uklanjanje licence

>[AZURE.NOTE]
Pročitajte [web-mjesto Microsoft Azure sigurnosti i u okvir za upravljanje zapisnika nadzora](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) dodatne informacije o mogućnostima značajke zapisivanje u Azure.
Ovisno o tome kako odgovoriti na pitanja u [zahtjevima za upravljanje sadržajem određivanje](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md), mora biti da biste odredili kako želite da se sadržaj upravlja u rješenje hibridnog identiteta. Dok sve mogućnosti prikazanima u tablicu 6 može integrirati s Azure AD, važno je da biste odredili koji je prikladnije za svoje poslovne potrebe.

| Mogućnosti za upravljanje sadržajem                                                               | Prednosti                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Nedostaci                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Središnje lokalnog (Active Directory Rights Management Server)                      | Potpunu kontrolu nad Infrastruktura server odgovoran za klasifikaciju podataka <br> Ugrađene mogućnosti u sustavu Windows Server, bez potrebe za dodatne licence ili pretplatu <br> Može se integrirati s Azure AD u scenariju hibridnog <br> Podržava mogućnosti upravljanja (IRM) prava na informacije u Microsoft Online services kao što je Exchange Online i SharePoint Online, kao i Office 365 <br> Podržava lokalni poslužitelj za proizvode tvrtke Microsoft, kao što su Exchange Server, sustava SharePoint Server i datotečne poslužitelje sa sustavom Windows Server i datoteke klasifikacija infrastrukture (FCI). | Novija verzija, održavanja (Zadrži gore sa ažuriranja, konfiguriranje i potencijalne nadogradnje), od IT je vlasnik poslužitelja <br> Obavezan Infrastruktura server lokalnog<br> Doesn'tleverage Azure mogućnosti nativno                                     |
| Centralizirane u oblaku (Azure RMS)                                                     | Jednostavnije upravljati u usporedbi s lokalnim rješenja <br> Može se integrirati s AD DS u scenariju hibridnog <br>  Posve integriran s Azure AD <br> Nije potreban na informacije o lokalnom poslužitelju da bi se implementaciju usluge <br> Podržava lokalnim poslužiteljskim proizvodima Microsoft Exchange Server, sustava SharePoint, poslužitelja i datotečne poslužitelje sa sustavom Windows Server i klasifikaciju datoteka, Infrastruktura (FCI) <br> IT, možete imaju potpunu kontrolu nad ključ njihove klijenta s mogućnošću pretraživanja kroz BYOK.                                                                                    | Vaša tvrtka ili ustanova morate imati pretplatu oblak koji podržava RMS-a <br> Vaša tvrtka ili ustanova mora imati u imeniku Azure AD za podršku za provjeru autentičnosti korisnika u RMS-a                                                                                  |
| Hibridno (Azure RMS integriran s, lokalnog Active Directory Rights Management Server) | Ovaj scenarij akumulira prednosti obje središnje na lokalno i u oblaku.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Vaša tvrtka ili ustanova morate imati pretplatu oblak koji podržava RMS-a <br> Vaša tvrtka ili ustanova mora imati u imeniku Azure AD za podršku za provjeru autentičnosti korisnika u RMS, <br> Potrebna je veza između servisa Azure oblaka i lokalnog infrastrukture |

## <a name="define-access-control-options"></a>Definiranje mogućnosti kontrole programa access
Po korištenje provjeru autentičnosti, autorizacije i pristupa određuju mogućnosti dostupne u Azure AD moći ćete omogućiti svojoj tvrtki korištenje središnje identiteta spremište istovremeni korisnika i partnera za korištenje jedinstvenu prijavu (SSO), kao što je prikazano na slici u nastavku:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Središnje upravljanje i potpuno Integracija s drugim direktorija

Azure Active Directory sadrži jedinstvenu prijavu na tisuće SaaS aplikacije i lokalne web-aplikacije. Pročitajte u [Azure Active Directory federation kompatibilnosti popisa: davatelji identiteta drugih proizvođača koji se mogu koristiti za implementaciju jedinstvenu prijavu](https://msdn.microsoft.com/library/azure/jj679342.aspx) članka dodatne informacije o SSO drugih proizvođača koji su testira Microsoft. Ova mogućnost omogućuje tvrtki ili ustanovi implementaciju različite scenarije B2B uz zadržavanje kontrolu nad upravljanje identiteta i pristup. Međutim, tijekom na B2B postupka dizajniranja važno je znati metoda provjere autentičnosti koja će se koristiti partnera i provjeriti ako ovu metodu podržava Azure. Ovo su trenutno metoda podržava Azure AD:

- Sigurnost pridruživanju Markup Language (SAML)
- OAuth
- Kerberos
- Tokeni
- Certifikati


>[AZURE.NOTE] Pročitajte [Azure Active Directory provjere autentičnosti protokoli](https://msdn.microsoft.com/library/azure/dn151124.aspx) zanimaju dodatne informacije o svaki protokol i njegovim mogućnostima u Azure.

Korištenje podrška za Azure AD, mobilne poslovnih aplikacija možete koristiti isti jednostavno mobilne usluge provjere autentičnosti rad s da biste omogućili zaposlenici se prijaviti u svoje mobilne aplikacije s svoje tvrtke vjerodajnice za Active Directory. S tom značajkom Azure AD podržana kao davateljem identiteta u mobilne usluge duž s na druge davatelji identiteta već podržavamo (što obuhvaća Microsoft Accounts, Facebook ID, Google ID-a i na Twitteru ID). Ako aplikacija lokalnog koristi vjerodajnica korisnika smještene u AD DS tvrtke, pristup s partnera i korisnicima koji dolaze iz oblaka mora biti prozirni. Možete upravljati kontrola uvjetno pristupa korisnika (u oblaku) web-aplikacije, API na webu, Microsoftovim servisima u oblaku, 3 aplikacijama SaaS proizvođača i aplikacije za nativni klijent (mobilni) i imaju prednosti sigurnosti, nadzor, izvješćivanje o pogreškama na jednom mjestu. Međutim, preporučuje se da biste provjerili u okruženju koje nisu proizvodnje ili tijekom ograničenog vremenskog korisnika.


>[AZURE.TIP] Važno je da spomenuti da Azure AD nema pravila grupe kao što je AD DS. Da bi se provode pravila za uređaje potrebno je rješenje za upravljanje mobilnim uređajima kao što je [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx).

Kada korisnik provjere autentičnosti pomoću Azure AD, važno je da biste procijenili razinu pristupa da korisnik će imati ga. Razinu pristupa koju korisnik će imati putem resursa može se razlikovati, dok Azure AD možete dodati u sloju dodatne sigurnosti pristupom nekih resursa, koje morate Imajte na umu da resursa sam također može imati vlastiti popis za kontrolu pristupa odvojeno, kao što su kontrole pristupa za datoteke koje se nalaze u datotečnom poslužitelju. Na slici u nastavku navedene su razine kontrole programa access koje se mogu nalaziti scenarija hibridnog:

![](./media/hybrid-id-design-considerations/accesscontrol.png)


Svaki interakciju u dijagramu prikazivao u X slika predstavlja jedan pristup kontrola scenarij u kojem možete prekriveno skupovima Azure AD. U nastavku su opis svake scenarija:

1. uvjetno pristup aplikacijama koje su hostira lokalnog: registrirani uređaja možete koristiti s pravilima programa access za aplikacije koje su konfigurirana za korištenje servisa AD FS sa sustavom Windows Server 2012 R2. Dodatne informacije o postavljanju uvjetno pristupa za lokalni potražite u članku [Postavljanje lokalnog pristupa uvjetno pomoću Azure Active Directory Registracija uređaja](active-directory-conditional-access-on-premises-setup.md).
2. pristup kontrolu Portal za upravljanje Azure: Azure je i mogućnost za kontrolu pristupa Portal za upravljanje korištenjem RBAC (uloga temelji kontrola pristupa). Ovaj postupak omogućuje tvrtki da biste ograničili količinu operacije koje pojedinac možete učiniti kada se on ima pristup Portal za upravljanje Azure. Pomoću RBAC za kontrolu pristupa portalu IT administratorima ca prava pristupa delegiranju pomoću sljedeće postupke za upravljanje programa access:

 - Dodjela grupne uloge: access možete dodijeliti Azure AD grupe koje se mogu se sinkronizirati s lokalni Active Directory. Omogućuje vam korištenje postojećih ulaganja načinjene tvrtke ili ustanove u tooling i postupke za upravljanje grupama. Možete koristiti i značajku upravljanja ovlaštenog grupa od Azure AD Premium.
 - Leverage ugrađena ulogama u Azure: koristite tri uloge – vlasnik, suradnika i Reader, proizvod tvrtke da biste bili sigurni da korisnici i grupe imati dozvolu za samo zadatke koje su im potrebne za svoje zadatke.
 - Zrnastog pristupa resursima: možete dodijeliti uloge korisnicima i grupama za određenu pretplatu, grupa resursa ili pojedinačnog Azure resursa kao što je web-mjesta ili u bazi podataka. Na taj način možete omogućiti da korisnici imaju pristup svim resursima koje su im potrebne i bez pristupa resursima koje su im potrebne za upravljanje.

>[AZURE.NOTE] [Kontrola pristupa na temelju uloga u Azure](https://azure.microsoft.com/updates/role-based-access-control-in-azure-preview-portal/) zanimaju dodatne informacije o ovoj mogućnosti za čitanje. Za razvojne inženjere koji su stvaranje aplikacija i Prilagodba kontrolu pristupa za njih, također je moguće koristiti uloge aplikacije za Azure AD za autorizaciju. Pročitajte članak u ovom se [primjeru WebApp RoleClaims DotNet](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) opisuje sastavljanje aplikacije da biste koristili tu mogućnost.

3. uvjetno pristup aplikacijama sustava Office 365 s Microsoft Intune: IT administratorima možete Dodjela uvjetno pristup uređaj pravila zaštita tvrtke resursa, dok je u isto vrijeme dopuštanja zaposlenici zaduženi za podatke na uređaji za pristup servisima. Dodatne informacije potražite u članku [Uvjetnog pravila pristupa uređaj za servise sustava Office 365](active-directory-conditional-access-device-policies.md).

4.u uvjetno pristup za Saas aplikacije: [Ova značajka](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) omogućuje konfiguriranje pravila za pristup po aplikacija višestruke provjere autentičnosti i mogućnost da biste blokirali pristup za korisnike ne pouzdani mreže. Višestruka provjera autentičnosti pravila možete primijeniti na sve korisnike koji su vam dodijeljeni aplikaciji ili samo za korisnike u navedenom sigurnosne grupe. Korisnici mogu izuzeti iz obavezne višestruka provjera autentičnosti ako su pristup aplikacije iz IP adresu koja u unutar ustanove mreže.

Budući da se mogućnosti za kontrolu pristupa koristite multilayer pristup, usporedbe između tih mogućnosti nisu primjenjivi za taj zadatak. Provjerite da su korištenje sve mogućnosti dostupne za svaki scenarij u kojem je potrebno za kontrolu pristupa resursa.

## <a name="define-incident-response-options"></a>Definiranje mogućnosti incidenta odgovora
Azure AD mogu pomoći IT identitetu potencijalni sigurnosni rizik u okruženju prateći aktivnosti korisnika, mogu IT pod utjecajem Azure AD pristup i korištenje izvješća mogućnost da bi se dobio uvid u integritet i sigurnost imenik tvrtke ili ustanove. Pomoću tih informacija administrator možete bolje odrediti gdje može biti moguće sigurnosni rizik tako da ih možete planirati odgovarajući način prevladavanje tih rizika.  [Azure AD Premium pretplate](active-directory-get-started-premium.md) sadrži skup sigurnosnih izvješća koja možete omogućiti IT da biste nabavili taj podatak. [Azure AD izvješća](active-directory-view-access-usage-reports.md) kategorizirane kao što je prikazano u nastavku:

- **Značajkom izvješća**: sadrže događaje koje smo pronašli biti anomalous za prijavu. Naš cilj je pretvorite svoju takve aktivnost i omogućuju vam da biste mogli bi određivanja o tome je li događaj sumnjive.
- **Izvješće integrirane aplikacije**: nudi uvida u kako oblaka aplikacije koji se koriste u tvrtki ili ustanovi. Azure Active Directory nudi Integracija s tisuće oblaka aplikacija.
- **Izvješća o pogreškama**: označio pogreške koji se mogu pojaviti prilikom dodjele resursa računi da biste vanjskim aplikacijama.
- **Specifične za korisnika izvješća**: prikaz uređaja i prijavite se u aktivnosti podataka za određenog korisnika.
- **Aktivnosti zapisnika**: sadrže zapis o svim događajima nadziru unutar zadnja 24 sata, zadnjih sedam dana ili zadnjih 30 dana, kao i promjena aktivnost u grupama i vratiti i registracija aktivnosti lozinku.

>[AZURE.TIP]
Drugo izvješće koje mogu pomoći Incident odgovor timskog rada na slučaju je izvješće o [korisniku s licenciranjem vjerodajnicama](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) .  Izvješće surfaces sve rezultate između tih popisa licenciranjem vjerodajnice i vaš klijent.

Ostale važna ugrađeni u izvješćima Azure AD koje je moguće koristiti tijekom istrazi incidenta odgovor i su:

- **Ponovno postavljanje lozinke aktivnosti**: administrator pružiti uvida u aktivno kako se koristi ponovno postavljanje lozinke u tvrtki ili ustanovi.
- **Registracija aktivnosti za ponovno postavljanje lozinke**: daje uvid u koju korisnici registriranih njihove postupci za ponovno postavljanje lozinke i koje metode oni ste odabrali.
- **Grupa aktivnosti**: nudi povijest promjena u grupi (ex: korisnici dodani ili uklonjeni) koji su pokrenuti na ploči programa Access.

Osim core izvješćivanja mogućnost dostupna u Azure AD Premium koji mogu biti leveraged tijekom Incident odgovor istrage procesa, IT mogu i pod utjecajem nadzora izvješće da biste dobili informacije kao što su:

- Promjene u članstvo u ulozi (ex: korisnik dodan uloga globalnog administratora)
- Vjerodajnica ažuriranja (ex: promjene lozinke)
- Upravljanje domenom (ex: Provjera prilagođenu domenu, ukloniti domene)
- Dodavanje ili uklanjanje aplikacija
- Upravljanje korisnicima (ex: dodavanjem, uklanjanjem, ažuriranje korisnika)
- Dodavanje ili uklanjanje licence

Budući da se mogućnosti za incidenta odgovor koristi multilayer pristup, usporedbe između tih mogućnosti nisu primjenjivi za taj zadatak. Provjerite da su korištenje sve mogućnosti dostupne za svaki scenarij u kojem je potrebno koristiti mogućnost za izvješćivanje Azure AD tijekom procesa incidenta odgovor vaše tvrtke.

## <a name="next-steps"></a>Daljnji koraci
[Određivanje zadatke upravljanja identiteta hibridnog](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)


## <a name="see-also"></a>Vidi također
[Pregled pitanja vezana uz dizajn](active-directory-hybrid-identity-design-considerations-overview.md)
