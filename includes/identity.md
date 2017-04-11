Upravljanje identitetom samo kao Važno je u javnim oblaku kao što je lokalno. Da bi se uz to, Azure podržava nekoliko različitih oblaka identiteta tehnologije. To su ovo:

- Windows Server Active Directory (pod nazivom i samo AD) možete pokrenuti u oblak pomoću virtualnim strojevima stvorene pomoću Azure virtualnog računala. Taj se način smisla kada koristite Azure širenje vaše lokalne podatkovnog centra u oblak.


- Azure Active Directory možete koristiti da bi se dobilo vaše korisnike jedinstvenu prijavu aplikacijama [softver kao Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) . Tvrtke Microsoft Office 365 koristi ove tehnologije, na primjer, a aplikacija na web-mjesto Azure ili u okvir za druge platforme oblaka možete koristiti.


- Aplikacije koje rade u oblaku ili na lokalnim poslužiteljima pomoću Azure Active Directory kontrola pristupa zapisniku omogućite korisnicima u korištenju identiteta s Facebooka, Google, Microsoft i drugih davatelja identiteta.


U ovom se članku opisuju sva tri od sljedećih mogućnosti.

## <a name="table-of-contents"></a>Tablice sadržaja

- [Pokretanje Active Directory za Windows Server na virtualnim računalima sustava](#adinvm)

- [Pomoću servisa Azure Active Directory](#ad)

- [Pomoću kontrola pristupa Azure Active Directory](#ac)


## <a name="adinvm"></a>Pokretanje Active Directory za Windows Server na virtualnim računalima sustava

Sa sustavom Windows Server AD na Azure virtualnim računalima sustava je vrlo nalik radi lokalnog. [Slika 1](#fig1) prikazuje uobičajeni primjera kako to izgleda.

![Azure Active Directory u virtualnog računala](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Slika 1: Windows Server Active Directory mogu se izvoditi na Azure virtualnim računalima sustava povezani s podatkovnim centrom za lokalne organizacije pomoću Azure virtualne mreže.

U ovom primjeru, Windows Server AD se izvodi u VMs stvoren pomoću tehnologije IaaS na platformi Azure virtualnim računalima. Ove VMs i nekoliko druga grupiraju se u virtualne mreže s lokalnim podatkovnog centra pomoću Azure virtualne mreže. Virtualne mreže carves izvan grupe oblaka virtualnim strojevima interakciju s lokalne mreže putem virtualne privatne mreže (VPN-a) veze. Način omogućuje te Azure virtualnim strojevima izgledati samo drugi podmreže podatkovnim centrom za lokalni. Kao što je prikazano na slici, dva te VMs izvodi Windows Server AD kontrolera domena. Ostale virtualnim strojevima u virtualne mreže mogu imati aplikacija, kao što je SharePoint ili na neki drugi način, kao što su za koriste razvoj i testiranje. Podatkovnog centra za lokalni i radi dva kontrolera Windows Server AD domena.

Za povezivanje kontrolera domena u oblaku s onima izvodi lokalno na nekoliko načina:

- Provjerite sve ih dio jedne domene servisa Active Directory.

- Stvaranje zasebnom AD domena lokalne i u oblaku koje su dio isti skup stabala.

- Stvorite zaseban AD šuma u oblak i lokalne, a zatim povezivanje šuma pomoću trusts unakrsno-skupa stabala ili Windows Server Active Directory Federation Services (AD FS), koje možete pokrenuti na virtualnim računalima sustava na Azure.

Napravi bilo kakve izbora, administrator provjerite je li da zahtjevi za provjeru autentičnosti iz lokalne korisnike otvorite kontrolera domena oblaka samo kada je to potrebno, budući da je veza s oblakom vjerojatno će biti manja od lokalne mreže. Drugi faktor treba uzeti u obzir u oblak povezivanja i kontrolera domena lokalne je promet generira replikacije. Kontrolera oblaka su obično vlastita AD web-mjesta, koja omogućuje administrator da biste zakazali učestalost ponavljanja obavlja. Azure naknade za promet poslana iz Azure podatkovnog centra, iako ne za bajtova Pristigla, koji može utjecati na administratore replikacije odabira. To vrijedi i ističu da dok Azure omogućuje vlastitu podršku za upravljanje pravima na informacije (DNS-Domain Name System), taj servis nema značajke potrebnih servisa Active Directory (kao što je podrška za dinamičku DNS i SRV zapisi). Zbog toga sa sustavom Windows Server AD na Azure zahtijeva postavljanje vlastite DNS poslužitelji u oblaku.

Sa sustavom Windows Server AD u Azure VMs možete smisla u nekoliko različitih situacija. Evo nekoliko primjera:

- Ako koristite virtualnim računalima sustava Azure kao datotečni nastavak vlastite podatkovnog centra, možda pokrenuti aplikacije u oblaku koje su potrebne lokalne domene kontrolera za rukovanje stvari kao što su zahtjeva za integriranu provjeru autentičnosti sustava Windows ili LDAP upita. SharePoint, na primjer, često sa sustavom servisa Active Directory i tako dok je moguće Azure pomoću lokalnog imenika pokrenuti farme sustava SharePoint, postavljanju kontrolera oblaka će znatno poboljšati performanse. (Nije važno shvatite da to ne moraju nužno biti potrebna, no; mnogo aplikacije možete pokrenuti uspješno u oblak pomoću samo kontrolera domena lokalne.)

- Pretpostavimo da daleke podružnici nedostaju resursa za izvođenje vlastitu kontrolera domena. Danas, korisnici moraju provjeru autentičnosti kontrolera domena na drugoj strani svijeta – prijave su sporo. Sustavom Azure Active Directory u bliže podatkovnog centra u programu Microsoft možete ubrzati to ne zahtijeva više poslužitelja u podružnici.

- Tvrtkom ili ustanovom koja koristi Azure za oporavak Izrada možda imaju malom aktivni VMs u oblaku, uključujući kontroler domene. To možete pa se pripremite da biste proširili ovo web-mjesto kao što je potreban za preuzimanje uloge za pogreške na neko drugo mjesto.

Postoje i druge mogućnosti. Na primjer, niste trebaju povezati Windows Server AD u oblaku za lokalni podatkovnog centra. Ako želite pokrenuti farme sustava SharePoint koja su služila određeni skup korisnika, na primjer, sve kojemu želite prijaviti isključivo uz identitete utemeljene na oblaku, možete stvoriti skup stabala samostalne na Azure. Kako koristiti tehnologije ovisi o koji su ciljeve. (Za detaljnije upute o korištenju Windows Server AD s Azure, [ovdje potražite u članku](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Pomoću servisa Azure Active Directory

Kao što je aplikacija SaaS postaju više i uobičajene, oni podići očite pitanje: kakve imeničkom servisu te aplikacije utemeljene na oblaku koristiti? Microsoftovi odgovor na to pitanje je Azure Active Directory.

Postoje dvije glavne mogućnosti za korištenje ovaj imenički servis u oblaku:

- Osobe i tvrtke ili ustanove koji koriste samo SaaS aplikacije možete koristiti Azure Active Directory kao njihove snosite imeničkom servisu.

- Tvrtke i ustanove sa sustavom Windows Server Active Directory možete povezati svoje lokalnog imenika Azure Active Directory, a zatim ga koristiti da bi se dobilo svojim korisnicima jedinstvenu prijavu SaaS aplikacijama.


[Slika 2](#fig2) prikazuje prvi od sljedeće dvije mogućnosti pri čemu je Azure Active Directory sve što je potrebno.

![Azure Active Directory u virtualnog računala](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Slika 2: Azure Active Directory omogućuje tvrtki ili ustanovi korisnicima jedinstvenu prijavu SaaS aplikacije, uključujući Office 365.

Kao što je prikazano na slici, Azure AD je servis više klijenta. To znači da istodobno može podržati mnogo različitih tvrtkama ili ustanovama pohranu direktorija podataka o korisnicima na svaku od njih. U ovom primjeru korisnik u tvrtki ili ustanovi odgovora pokušava pristupiti SaaS aplikacije. Ove aplikacije mogu biti dio sustava Office 365, kao što je SharePoint Online ili može biti nešto drugo – aplikacije drugih proizvođača možete koristiti tehnologije. Budući da Azure AD podržava protokol SAML 2.0, sve što je potrebno iz aplikacije je mogućnost interakciju standardu ovaj industrijskih. (Zapravo aplikacije koje koriste Azure AD može pokrenuti u bilo kojem podatkovnog centra, ne samo Azure Standard.)

Postupak započinje kad korisnik pristupa SaaS aplikacije (korak 1). Da biste koristili ovu aplikaciju, korisnik mora izlaganje token izdala Azure AD.

Ovaj token sadrži podaci koje služe kao korisnik i digitalno potpisanu pomoću Azure AD. Da biste dobili token, korisnik potvrđuje sebi za Azure AD unosom korisničkog imena i lozinke (korak 2). Zatim Azure AD vraća token on mora (korak 3).

Ovaj token šalje se zatim SaaS aplikacije (koraku 4), koji ovjerava potpis na token te koristi njegov sadržaj (korak 5). Aplikacija će koristiti informacije o identitetu token sadrži odluči informacije koje korisnik može pristupiti, a možda na druge načine.

Ako aplikacija treba dodatne informacije o korisniku nego što se nalazi u token, to možete zatražiti izravno iz Azure AD pomoću Azure AD grafikonu API-JA (korak 6). U Početna verzija Azure AD je prilično jednostavan sheme direktorija: sadrži samo korisnici i grupe i odnosa među njima. Aplikacije mogu koristiti taj podatak dodatne informacije o veze između korisnika. Ako, na primjer, pretpostavimo da nije potrebno znati tko je upravitelj ovog korisnika odlučite li on dopušten pristup nekim bloka podataka aplikaciju. To možete saznati postavljanjem upita Azure AD pomoću API grafikonu.

API grafikonu koristi običan RESTful protokol koji olakšava jasan da biste koristili Većina klijenata, uključujući mobilnih uređaja. U API podržava i proširenja definira OData, dodavanje značajki kao što su jezika za upite da biste omogućili pristup podacima klijenti korisnijim načine. (Dodatne informacije o OData, potražite u članku [Uvod u OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf)) Jer API grafikon može se koristiti za dodatne informacije o odnosima između korisnika, omogućuje aplikacije razumijevanje društvenih grafikonu koji je ugrađen u shemi Azure AD za određenu tvrtku ili ustanovu (što je Zašto se zove grafikonu API-JA). I radi provjere autentičnosti za Azure AD grafikonu API zahtjeva za aplikaciju koristi OAuth 2.0.

Ako je tvrtka ili ustanova ne koristi Windows Server Active Directory - nema lokalne poslužitelje ili domena - i ovisi isključivo oblaka aplikacije koje koriste Azure AD, koristeći samo taj imenik oblaka želite dati na potvrđeni korisnici jedinstvenu prijavu ih sve. Još dok scenarij dobije uobičajene svakodnevno, većina tvrtkama ili ustanovama još uvijek koristi lokalne domene koje su stvorene pomoću Windows Server Active Directory. Azure AD sadrži korisne ulogu želite reproducirati ovdje i, kao što je [Slika 3](#fig3) prikazuje.

![Azure Active Directory u virtualnog računala](./media/identity/identity_03_AD.png)
<a id="fig3"></a>slika 3: tvrtki ili ustanovi mogu združivanje Windows Server Active Directory s Azure Active Directory da bi se dobilo njegov korisnici jedinstvenu prijavu SaaS aplikacijama.

U ovom scenariju korisnika u tvrtki ili ustanovi B želi pristup SaaS aplikaciji. Prije nego što je to, administratori u tvrtki ili ustanovi direktorija morate uspostaviti odnos vanjski pristup s Azure AD pomoću servisa AD FS, kao što je prikazano na slici. Te administratori morate konfigurirati sinkronizacije podataka između ustanove lokalnog Windows Server AD i Azure AD. To automatski kopira korisnički i grupni podatak iz lokalnog imenika Azure AD. Obratite pozornost na to što to dopušta: na snazi je tvrtki ili ustanovi proširivanje njegov lokalnog imenika u oblak. Kombiniranje Windows Server AD i Azure AD na taj način omogućuje tvrtki ili ustanovi koji se može upravljati kao jedan entitet, u oblaku i tijekom i dalje pojavljuju na ostavlja manji trag pri oba lokalnog imeničkog servisa.

Da biste koristili Azure AD, korisnik prvo prijavi svoj lokalne domene servisa Active Directory kao i obično (korak 1). Kada korisnik pokuša pristup aplikaciji SaaS (korak 2), postupka Federacija rezultira Azure AD izdavanja njegove token za ovu aplikaciju (korak 3). (Dodatne informacije o kako funkcionira vanjski pristup, potražite u članku [utemeljene na zahtjevima identiteta za Windows: tehnologije i scenariji](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Kao i prije, ovaj token sadrži informacije koja služi za identifikaciju korisnika, a digitalno potpisanu pomoću Azure AD. Ovaj token šalje se zatim SaaS aplikacije (koraku 4), koji ovjerava potpis na token te koristi njegov sadržaj (korak 5). I u prethodnom scenariju SaaS aplikacije API grafikon možete koristiti da biste saznali više o korisniku ako je potrebno (korak 6).

Danas, Azure AD nije dovršeno zamjena za lokalni Windows Server AD. Spomenuti već, direktorija oblaka ima mnogo jednostavnijim shemu, a nedostaje i značajki kao što su pravila grupe, mogućnost da biste pohranili podatke o strojeva i podršku posvećeno LDAP. (Zapravo stroj Windows nije moguće konfigurirati da biste korisnicima koji se prijavite pomoću samo Azure AD, – to nije podržana scenarij.) Umjesto toga početnih ciljeva Azure AD obuhvaćaju da korisnici pristup aplikacijama u oblaku bez održavanje zasebnom prijava i odbacivanje lokalnog imenika administratori ručno s svaka SaaS aplikacija njihov tvrtka ili ustanova koristi sinkronizaciju njihove lokalnog imenika. Tijekom vremena, no očekivan ovaj oblaka imenički servis za rješavanje širem krugu scenarija.

## <a name="ac"></a>Pomoću kontrola pristupa Azure Active Directory

Oblaku identiteta tehnologije može se koristiti za rješavanja raznih problema. Azure Active Directory možete dati tvrtki ili ustanovi korisnicima jedinstvenu prijavu više SaaS aplikacije, na primjer. Ali tehnologije identiteta u oblaku možete koristiti i na druge načine.

Pretpostavimo da, na primjer, da se aplikacija želje njegov korisnicima omogućiti prijavite se pomoću tokena koje izdaje više *davatelji identiteta (IdPs)*. Mnogo različitih identiteta danas, postoje uključujući Facebook, Google, Microsoft i druge, a aplikacije često korisnicima prijavite se pomoću jedne od ovih identiteta. Zašto treba zamarati aplikaciju za održavanje vlastiti popis korisnika i lozinke prilikom umjesto toga možete ovise o identiteta koje već postoje? Prihvaćanje postojećih čini vijek jednostavniji za korisnike koji imaju nešto manje korisničko ime i lozinku za Imajte na umu i za osobe koje stvaraju aplikacije, koje više nisu potrebne da bi se zadržao vlastite popise korisnička imena i lozinke.

No dok se svaki davatelja identiteta problemi neke vrste tokena, te se tokeni nisu standardni – svaki IdP oblikovanjem vlastitu. Osim toga, informacije u tim tokeni i nije standard. Aplikacije sustava želi prihvatiti tokena koje izdaje, izgovorite, Facebook, Google i Microsoft se nađu test o pisanju jedinstveni koda za rukovanje svaki od tih različite oblike.

No zašto je to? Zašto ne umjesto toga stvoriti posrednik koji mogu uzrokovati jedan oblik tokena s uobičajenih reprezentaciju identiteta podataka? Taj se način bi vijek jednostavniji za razvojne inženjere koji stvaraju aplikacije, budući da se sada potrebno učiniti samo jednu vrstu tokena. Azure Active Directory pristupne kontrole to točno, koja omogućuje posrednik u oblaku za rad s raznih tokena. [Slika 4](#fig4) prikazuje načina funkcioniranja tijeka rada

![Azure Active Directory u virtualnog računala](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>slika 4: Azure Active Directory kontrola pristupa olakšava aplikacije da biste prihvatili identiteta tokeni izdao različite identiteta.

Postupak započinje kada se korisnik pokuša pristupiti aplikaciju putem preglednika. Aplikacija preusmjerava njegove da biste je IdP svoj odabir (i da aplikacije i smatra pouzdanima). Ana potvrđuje herself za ovaj IdP, kao što su tako da unesete korisničko ime i lozinku (korak 1), a na IdP vraća token koji sadrži informacije o svoj (korak 2).

Kao što je prikazano na slici, kontrolu pristupa podržava raspon različite oblaku IdPs, uključujući račune stvorio Google, Yahoo, Facebook, Microsoft (prijašnjeg Windows Live ID) i bilo kojeg davatelja usluga OpenID. Podržava i identiteti stvoren pomoću servisa Azure Active Directory i putem pridruženi AD FS, Windows Server Active Directory. Cilj je Popratno najčešće korištenih identitete danas, hoće li se izdala IdPs u oblaku ili na lokalnim poslužiteljima.

Kada korisnikov preglednik ima programa IdP tokena iz svoj odabrali IdP, šalje to tokena za kontrolu pristupa (korak 3). Kontrola pristupa Provjeri valjanost token, pazeći da je zaista izdala ovaj IdP, a zatim stvara novi token prema pravilima koji su definirani za ovu aplikaciju. Kao što su Azure Active Directory, kontrola pristupa je servis više klijent, ali u klijenata su aplikacije umjesto kupca tvrtke ili ustanove. Svaku aplikaciju možete dobiti vlastiti prostor naziva, kao što je prikazano na slici, a možete definirati različita pravila o autorizacije i još mnogo toga.

Ta pravila omogućuju definiranje načina mora biti u token za kontrolu pristupa transformacije tokena iz različitih IdPs administratora za svaku aplikaciju. Ako, na primjer, ako drugi IdPs koristili različite vrste koji predstavlja korisnička imena, pravila za kontrolu pristupa možete pretvoriti sve te u uobičajeni upišite korisničko ime. Kontrola pristupa zatim šalje ovo novi token natrag preglednik (koraku 4), koji se šalje na aplikaciju (korak 5). Kada je token za kontrolu pristupa, aplikacija potvrđuje da ovaj token zaista izdala kontrola pristupa, a zatim koristi informacije koje sadrži (korak 6).

Taj postupak može vam prvi pogled činiti malo složene, zapravo čini vijek znatno jednostavniji za alat za stvaranje aplikacije. Umjesto rukovati raznih tokeni koje sadrže druge podatke, aplikacija možete prihvatiti identitete izdala više davatelji identiteta tijekom i dalje primati samo jednu token poznatih informacijama. Osim toga, umjesto zahtijeva svaku aplikaciju mora biti konfigurirana tako da pouzdanost različite IdPs, tim statusima pouzdanost umjesto održava kontrola pristupa – potrebna samo pouzdanosti aplikacije.

To vrijedi ističu se ništa o kontrola pristupa uz Windows – nije baš kao i koristit aplikacija Linux koji prihvatili samo identiteta Google i Facebook. I čak i ako je kontrola pristupa dio obitelji Azure Active Directory, možete smatrati je potpuno distinct servisa negoli je opisano u prethodnom odjeljku. Premda oba tehnologije radili s identiteta, oni adresa prilično probleme (iako Microsoft je rečeno da očekuje da biste integrirali dva trenutku).

Rad s identiteta je važno u skoro sve aplikacije. Cilj kontrola pristupa je lakše inženjerima omogućuje stvaranje aplikacija koje prihvatiti identitete davatelja identiteta raznih. Postavljanjem taj servis u oblaku, Microsoft omogućila je na bilo koji program koji se izvodi na svim platformama.

##<a name="about-the-author"></a>O autoru

Nevena Chappell je Chappell glavni od i suradnicima [www.davidchappell.com](http://www.davidchappell.com) u Pula Kaliforniji.
