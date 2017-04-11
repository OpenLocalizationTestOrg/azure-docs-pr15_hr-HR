<properties
    pageTitle="Cloud Sigurnost aplikacije otkrivanje i pitanja vezana uz privatnost | Microsoft Azure"
    description="U ovoj se temi opisuju sigurnost i zaštita privatnosti napomene vezane uz otkrivanje aplikacija oblaka."
    services="active-directory"
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
    ms.date="10/10/2016"
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Oblak aplikacije otkrivanje sigurnost i pitanja vezana uz izjave o zaštiti privatnosti

Microsoft predano radi na zaštiti privatnosti i zaštiti podataka tijekom izlaganja softvera i usluga koje olakšavaju upravljanje sigurnosti vaše tvrtke ili ustanove. <br>
Ne možemo prepoznati koji entrust podataka s drugima, pouzdanost zahtijeva inženjerskim ulaganja stroge sigurnost i stručna znanja da biste ga ponovno.
Microsoft se pridržava izričite usklađenost i upute o sigurnosti iz operacijskom servisa sigurne softver razvoj životni ciklus načini. <br>
Zaštita i zaštiti podataka je gornja prioritet Microsoft.

U ovoj se temi objašnjava kako koji se prikupljaju, obrađuju i zaštićenim unutar Azure Active Directory oblaka aplikacije otkrivanje podataka




##<a name="overview"></a>Pregled

Otkrivanje aplikacije oblak je značajka servisa Azure AD, a nalazi se u Microsoft Azure. <br>
Agent za otkrivanje aplikacije oblaka krajnjoj točki koristi se za prikupljanje podataka za otkrivanje aplikacije s kojim upravlja IT strojeva. <br>
Prikupljene podaci se šalju sigurno putem kanala šifrirane servisom za otkrivanje aplikacije Azure AD oblaka. <br>
Oblak aplikacije otkrivanje za organizaciju podaci pa vidljiv na portalu za Azure. <br>


<center>![Funkcioniranje oblaka aplikacije otkrivanje](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png)</center> <br>


U sljedećim se odjeljcima praćenje tijeka informacija i opisuju kako je osigurani kao ona prelazi iz vaše tvrtke ili ustanove sa servisom Cloud aplikacije otkrivanje i konačni portal za otkrivanje aplikacije oblaka.



## <a name="collecting-data-from-your-organization"></a>Prikupljanje podataka iz vaše tvrtke ili ustanove

Da biste mogli koristiti značajke za otkrivanje oblaka aplikacija Azure Active Directory, da biste dobili uvida u aplikacijama zaposlenicima u tvrtki ili ustanovi, morate najprije implementacija agent za otkrivanje aplikacija Azure AD oblaka krajnjoj točki računala u vašoj tvrtki ili ustanovi.

Administratori klijentu Azure Active Directory (i njihov delegat) možete preuzeti instalacijski paket agent s portala za Azure. Agenta možete biti ručno instalirati ili instalirati na više računala u tvrtki ili ustanovi pomoću SCCM ili pravila grupe.

Dodatne upute o mogućnostima implementacije potražite u članku [Oblaka aplikacija otkrivanje grupe pravila vodič za implementaciju](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).
<br>

### <a name="data-collected-by-the-agent"></a>Podatke prikupljene putem agenta

Informacije navedene u nastavku popis prikupio agenta kad upišete veze u web-aplikaciju. Informacije prikupljaju se samo onih programa koji je administrator konfigurirao za predočavanje elektroničkih dokumenata. <br>
Moguće je upravljanje popisom oblaka aplikacije koje se agent nadzire putem oblaka aplikacije otkrivanje plohu Microsoft [Azure portal](https://portal.azure.com/)u odjeljku **Postavke**->**Prikupljanje podataka**->**popis zbirke aplikacija**. Dodatne informacije potražite u članku [Početak rada s oblaka aplikacije otkrivanje](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
<br>
**Kategorija podataka**: podaci o korisniku <br>
**Opis**: <br>
Windows korisničko ime procesa koji se poslao zahtjev ciljno web-aplikacije (npr.: domena/korisničko ime) i Windows sigurnost identifikator (SID) korisnika.


**Kategorija podataka**: Obrada podataka <br>
**Opis**: <br>
Naziv procesa koja je proizvela zahtjev ciljno web-aplikacije (npr.: "iexplore.exe")

**Kategorija podataka**: podaci na računalu <br>
**Opis**: <br>
Naziv računala NetBIOS na kojemu je instaliran agenta.

**Kategorija podataka**: podaci promet aplikacija <br>
**Opis**: <br>

Sljedeće informacije o vezi:

- Izvor (lokalno računalo) i odredišni IP adrese i brojevi priključka

- Javnu IP adresu putem kojega zahtjev vodi izvan tvrtke ili ustanove.

- Vrijeme zahtjev

- Volumen promet šalje i prima

- IP verziju (4 ili 6)

- Za TLS veze samo: naziv glavnog računala na ciljnom s nastavkom oznaka naziv poslužitelja ili certifikat poslužitelja.

HTTP sljedeće informacije:

- Način (GET, objavu, itd.)

- Protocol (HTTP/1.1 itd.)

- Niz agenta korisnika

- Naziv glavnog računala

- Cilj URI (bez nizu upita)

- Informacije o vrsti sadržaja

- Informacije o URL referrer (bez nizu upita)



> [AZURE.NOTE] Gore navedene informacije HTTP prikupljaju se za sve veze koje nisu šifrirane.
Veze za TLS ove informacije samo zabilježene kada je uključena postavka temeljito ispitivanje na portalu. Postavka po zadanom je "O".
Dodatne detalje potražite u nastavku, i [Početak rada s oblaka aplikacije otkrivanje](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)


Uz podatke koje agenta prikuplja o aktivnosti mreže i prikuplja anonimne podatke o softver i hardverske konfiguracije, izvješća o pogreškama i informacije o kako se koristi agenta.

<br><br>
### <a name="how-the-agent-works"></a>Kako funkcionira agenta

Instalacija agent sadrži dvije komponente:

- Komponenta korisničkom načinu rada

- Upravljački program za komponentu otklanjanje način (upravljački program filtriranje platformu sustava Windows)



Prilikom prve instalacije agenta pohranjuje specifične pouzdana ustanova na računalu koji zatim koristi da biste uspostavili sigurnu vezu sa servisom Cloud aplikacije otkrivanje. <br>
Agenta povremeno dohvaća konfiguracija pravila iz servisa za otkrivanje aplikacija oblaka putem ovu sigurnu vezu. <br>
Pravila sadrži informacije o aplikacijama koje oblaka monitoru i je li automatsko ažuriranje trebala bi biti omogućena, između ostalog.

Promet Web se šalje i prima na računalu iz preglednika Internet Explorer i Chrome, agent za otkrivanje aplikacije oblaka analizira promet i izdvaja odgovarajući metapodataka (u odjeljku **podatke prikupljene putem agenta** gore). <br>
Svake minute agenta prenosi prikupljene metapodataka servisom za otkrivanje aplikacije oblaka putem šifrirane kanala.

Komponente upravljačkog programa intercepts šifrirane promet i umeće samu u šifrirane toka. Da biste vidjeli više pojedinosti u odjeljku **Intercepting podatke iz šifrirane veze (temeljito ispitivanje)** .


### <a name="respecting-user-privacy"></a>Poštivanje izjave o zaštiti privatnosti korisnika

Naš je cilj omogućuju administratorima alate za da biste postavili saldo između detaljne optics u aplikaciji korištenje i korisnik izjave o zaštiti privatnosti po potrebi za svoje tvrtke ili ustanove. Da biste taj će se kraj dajemo sljedeće knobs na stranici Postavke na portalu:

- **Prikupljanje podataka**: administratori mogu odabrati da biste odredili koji aplikacije ili kategorije aplikacije žele da biste dobili otkrivanje podataka.

- **Temeljito ispitivanje**: administratori odaberite da biste odredili ako agenta prikuplja HTTP promet za SSL/TLS veze (ili **Temeljito ispitivanje**). Dodatne informacije o tome u sljedećem odjeljku.

- **Mogućnosti pristanak**: administratori mogu koristiti portal za otkrivanje aplikacija oblaka odabrati želite li obavještavanje korisnika o prikupljanje podataka po agenta i želite li korisnik pristanak prije pokretanja agent prikupljanja podataka za korisnika.

Agent za otkrivanje aplikacije oblaka krajnjoj točki samo prikuplja podatke o opisane u **podatke prikupljene putem agenta** prethodnom odjeljku.


### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Intercepting podatke iz šifrirane veze (temeljito ispitivanje)
Kao što smo ranije spomenutih, administratori mogu konfigurirati agent za praćenje podataka iz šifrirane veze ('temeljito ispitivanje'). TLS ([Prijenosa sloja sigurnost](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) jedan je od najčešćih protokoli koristi na Internetu danas. Šifriranjem komunikaciju s TLS klijenta možete uspostaviti kanala sigurne i privatni komunikacije s web-poslužiteljem; TLS nudi ključna zaštiti za prosljeđivanje vjerodajnice za provjeru autentičnosti i spriječili otkrivanja povjerljivih podataka.

Vrijeme završetka do kraja sigurne šifrirane kanal nudi TLS omogućuje važne sigurnost i zaštita privatnosti, protokol često se abused svrhe zlonamjeran ili nefarious. Puno tako, zapravo te TLS je često se nazivaju "univerzalni vatrozid zaobilazak protokol". Korijenu problem je da većina vatrozidima su za provjeru TLS komunikacije jer podataka aplikacijskom sloju šifriran pomoću SSL. Kada zna to, attackers često pod utjecajem TLS izlaganje zlonamjerni payloads sigurni čak i najčešće Inteligentna aplikacijskom sloju vatrozidima potpuno slijepe TLS, a morate jednostavno preusmjeravanje TLS komunikaciju između domaćini korisniku. Krajnji korisnici često pod utjecajem TLS da biste zaobišli pristup kontrolama za postavio svoje tvrtke vatrozide i proxy poslužitelje, pomoću za povezivanje za javne proxy poslužitelji i tuneliranje-TLS protokoli vatrozidu inače možda je blokirana pravilima.

Temeljito ispitivanje omogućuje agent za otkrivanje oblaka aplikacija će poslužiti kao na pouzdanih Muškarac-u-u – srednje. Po klijentski zahtjev za pristup resursu za HTTPS zaštićeni upravljački program Agent za krajnje točke intercepts veza i uspostavlja njegov SSL certifikat ime klijenta dohvaća podatke u novu vezu na odredišnom poslužitelju za. Agenta zatim potvrđuje da se potvrda možete pouzdanog (tako da potvrdite da je ne poništena, a izvođenje drugih provjere certifikat), a ako te lozinka, zatim Agent za krajnje točke kopira podatke iz certifikat poslužitelja i stvoriti vlastiti certifikat poslužitelja – poznati kao potvrdu presretanja – korištenje tih podataka. Potvrda presretanja je traje na-na-Potpaleta po agent za krajnje točke s s korijenskim certifikatom kojem je instaliran Windows spremište pouzdanih certifikata. Ovaj korijenski samopotpisani certifikat je označena nije moguće izvesti, a zatim je prodao ACL administratorima. Je namijenjen nikad ostavite na računalu na kojem je stvorena. Kada krajnjeg-korisnika klijentska aplikacija primi potvrdu presretanja, ona smatrao pouzdanima jer je uspješno možete provjeriti lanac certifikata usidrili za korijenski certifikat. Ovaj postupak je prozirne iz krajnjeg-korisnika, točku prikaza s nekoliko upozorenja prema uputama u nastavku.

Omogućivanjem temeljito ispitivanje Agent za krajnje točke otkrivanje oblaka aplikacije možete dešifrirati i provjera komunikacije TLS šifrirane dopuštanja servisa da biste smanjili Šum i dati uvid o korištenju aplikacija šifrirani oblaka.

#### <a name="a-word-of-caution"></a>Upozorenje
Prije uključivanja temeljito ispitivanje, svakako preporučuje komunikaciju vašim potrebama za pravne i HR odjela i nabavite njihove pristanak. Pregled krajnji korisnik privatno šifrirane komunikacije može biti osjetljive predmet, očite razloga. Prije nego što je radnog snimljene fotografije-niste u uredu temeljito ispitivanje, provjerite određene sigurnosti za tvrtku i pravila prihvatljiva korištenja su ažurirani da biste naznačili šifrirana komunikacija će potražiti. Obavijesti korisniku i izuzeća web-mjesta smatra osjetljive (npr. banking i mjehuričaste web-mjesta) mogu biti potrebno Ako konfigurirate oblaka aplikacije otkrivanje praćenje ih. Kao što je rečeno iznad administratori mogu koristiti portal za otkrivanje oblaka aplikacije da biste odabrali hoćete li obavještavanje korisnika o prikupljanje podataka po agenta i želite li obavezne dozvole korisnika prije pokretanja agenta prikupljanja podataka za korisnika.

### <a name="known-issues-and-drawbacks"></a>Poznati problemi i nedostaci
Postoji nekoliko slučajeva gdje TLS presretanja mogu utjecati na kraj korisničkog sučelja:

- Prošireni potvrde valjanosti (EV) prikaz adresne trake web-preglednika zeleni će poslužiti kao vizualni podsjetnik posjećujete pouzdana web-mjesta. Provjere TLS ne možete duplicirati EV certifikatu problemi se za klijenta, pa web-mjestima koja koriste EV certifikate će raditi na uobičajen način, ali adresnu traku neće se prikazati zelena.  

- Javni ključ prikvačivanje (se nazivaju i prikvačivanje certifikat) osmišljeni su za pomoć pri zaštiti korisnika od napada Muškarac-u-u – srednje i rogue ustanovama za izdavanje certifikata. Kada korijenski certifikat za prikvačene web-mjesta ne odgovara jednoj poznati dobar CA korisnika, web-pregledniku odbacuje vezu s pogreškom. Budući da TLS presretanja je, zapravo je Muškarac-u-na-sredine, te veze neće uspjeti.

- Ako korisnici kliknite ikonu Zaključaj u pregledniku traku adresu web-preglednik provjerite informacije o tom mjestu, neće vidjeti lanac u ustanove za izdavanje certifikata za potpisivanje certifikata web-mjesta, ali umjesto lanac certifikata koji završava s prozora pouzdana spremište certifikata.

Da biste smanjili pojave problemi, ne možemo pratio servisa u oblaku i klijentske aplikacije poznato korištenje prošireni provjere valjanosti ili javni ključ prikvačivanje i uputite Agent za krajnju točku da biste izbjegli intercepting utjecati veze. Čak i u tim slučajevima, no i dalje ćete primati izvješća pomoću tih aplikacija oblaka i količinu podataka koji se prenose, ali jer nisu precizno pregledavaju, bez detalja o korištenju aplikacije su bit će dostupni.


## <a name="sending-data-to-cloud-app-discovery"></a>Slanje podataka otkrivanje aplikacije oblaka

Kada metapodataka sadrži prikupio agenta, na računalu nije lokalno do jedne minute ili dok predmemoriranim podacima dosegne veličinu 5MB. Može se sažeti te šalje putem sigurne veze sa servisom Cloud aplikacije otkrivanje.

Ako je agenta može komunicirati sa servisom Cloud aplikacije otkrivanje iz bilo kojeg razloga, prikupljene metapodaci se spremaju u predmemoriji lokalna datoteka koja se može pristupiti samo povlaštene korisnike na računalu (kao što su grupe administratora). <br>
Agenta automatski pokušava ponovno predmemorirani metapodataka dok je uspješno primljen putem oblaka aplikacije otkrivanje usluge.



## <a name="receiving-the-data-at-the-service-end"></a>Primanje podataka na kraju usluge

Na agenata autentičnost aplikacija otkrivanje oblaka servisa pomoću certifikata za provjeru autentičnosti određene klijentskog računala poziva iznad i prosljeđuje podataka putem šifrirane kanala. <br>
Servis za otkrivanje aplikacija oblaka analize kanal obrađuje metapodataka za svakog korisnika zasebno po logično particija kroz sve faza kanala analize.
Obrađeno metapodataka pogoni različita izvješća na portalu.

Neprovedenih metapodataka i obrađeno metapodataka spremaju se do 180 dana unatrag. Osim toga, korisnici možete odabrati da biste snimili obrađeno metapodataka u računa spremišta blobova platforme Azure njihove odabiru.
To je korisno za izvanmrežne analizu metapodataka, kao i više zadržavanja podataka.

## <a name="accessing-the-data-using-the-azure-portal"></a>Pristup podacima pomoću portala za Azure

U trud u zaštiti metapodataka koji se prikupljaju, po zadanom samo globalni administratori klijentu imati pristup značajku otkrivanje oblaka aplikacije na portalu za Azure. <br>
Međutim, administratori možete dodijeliti pristup drugim korisnicima ili grupama.



> [AZURE.NOTE] Dodatne informacije potražite u članku [Početak rada s oblaka aplikacije otkrivanje](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

<br>
Bilo koji korisnik pristup podacima na portalu mora biti licenciran uz licencu za Azure AD Premium.



##<a name="additional-resources"></a>Dodatni resursi


* [Kako otkriti unsanctioned oblaka aplikacije koje se koriste u tvrtki ili ustanovi](active-directory-cloudappdiscovery-whatis.md)
* [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
