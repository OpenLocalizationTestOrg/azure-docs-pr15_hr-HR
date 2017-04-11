<properties
    pageTitle="Otklanjanje poteškoća: Azure AD lozinku upravljanje | Microsoft Azure"
    description="Otklanjanje uobičajenih koraka za upravljanje Azure AD lozinku, uključujući ponovno postavljanje, promjena, upisima, Registracija, a koje će se informacije obuhvaćaju kada tražite pomoć."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="asteen"/>

# <a name="how-to-troubleshoot-password-management"></a>Otklanjanje poteškoća s upravljanje lozinkama

> [AZURE.IMPORTANT] **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).

Ako imate poteškoća s upravljanje lozinkama, rado ćemo vam pomoći. Većina problema možete pokrenuti u se može riješiti s nekoliko jednostavnih koraka za otklanjanje poteškoća koje možete pročitati dolje da biste otklonili implementaciju sustava:

* [**Informacije da biste dodali kada vam je potrebna pomoć**](#information-to-include-when-you-need-help)
* [**Problemi s konfiguracijom upravljanje lozinkama na portalu za upravljanje Azure**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**Problemi s lozinku upravljanje izvješćima na portalu za upravljanje Azure**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**Portal za registraciju za ponovno postavljanje probleme s lozinkom**](#troubleshoot-the-password-reset-registration-portal)
* [**Problemi s lozinku ponovno postavljanje portala**](#troubleshoot-the-password-reset-portal)
* [**Problemi s Upisima lozinke**](#troubleshoot-password-writeback)
  - [Kodovi pogrešaka za lozinku Upisima zapisnik događaja](#password-writeback-event-log-error-codes)
  - [Problemi s povezivanjem Upisima lozinke](#troubleshoot-password-writeback-connectivity)

Ako ste pokušali dolje navedene upute za otklanjanje poteškoća, a još rade na probleme, možete postavite pitanje na [forumima za Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) ili obratiti službi za podršku i smo ćete pogledajte problem čim možemo.

## <a name="information-to-include-when-you-need-help"></a>Informacije da biste dodali kada vam je potrebna pomoć

Ako ne uspijete riješiti problem s uputama u nastavku, obratite se našem inženjeri za podršku. Kada se obraćate ih, preporučuje se da biste uključili sljedeće informacije:

 - **Opći opis pogreške** – što točno poruka o pogrešci jeste li korisnik potražite u članku?  Ako je ne prikazuje se poruka o pogrešci, opišite neočekivano ponašanje primijetili detaljno.
 - **Stranica** – koju stranicu jeste li na kada se pojavi pogreška (Navedite URL)?
 - **Datum / vrijeme / vremenska zona** – što je precizno datum i vrijeme vidjeli pogreške (uključuje se vremenska zona)?
 - **Podržava kod** – što je podrška kod kada korisnik prikazivalo pogreške (za to pronaći, ponoviti pogreške, a zatim kliknite vezu podržava kod pri dnu zaslona i slanje inženjer za podršku GUID rezultata).
   - Ako ste na stranici bez koda podrške pri dnu, pritisnite F12 i traženje SID i CID i pošaljite tih dvaju rezultata inženjer za podršku.

    ![][001]

 - **Korisnički ID** – što je ID korisnika koji se pojavi pogreška (npr.user@contoso.com)?
 - **Informacije o korisniku** – je korisnik vanjskog, lozinku raspršivanje sinkronizirali, samo oblak?  Jeste li korisnik imati licencu za AAD Premium ili AAD osnovni dodijeljeni?
 - **Zapisnik događaja aplikacije** – ako koristite Upisima lozinku i pogreška se infrastruktura za lokalni, provjerite poštanski broj kopija aplikacije događaj iz poslužitelj za Azure AD Connect i poslati zajedno s vaš zahtjev.

Uključivanju sljedećih informacija pridonijet ćete za najbrže što može riješiti problem.


## <a name="troubleshoot-password-reset-configuration-in-the-azure-management-portal"></a>Otklanjanje poteškoća s konfiguracija ponovno postavljanje lozinke u Portal za upravljanje Azure
Ako naiđete na pogrešku prilikom za ponovno postavljanje lozinke za konfiguriranje, možda nećete moći Razrješavanje slijedeći dolje navedene upute za otklanjanje poteškoća:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Pogreška slučaja</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Što je pogreška vidjeti korisnika?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Rješenja</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ne prikazuje se u odjeljku <strong>Pravila za ponovno postavljanje lozinke korisničkog </strong>na kartici <strong>Konfiguriraj</strong> na portalu za upravljanje Azure</p>
            </td>
            <td>
              <p>U odjeljku <strong>Pravila za ponovno postavljanje lozinke korisnika </strong>nije vidljiv na kartici <strong>Konfiguriraj</strong> na portalu za upravljanje Azure.</p>
            </td>
            <td>
              <p>To se može dogoditi ako nemate licencu za AAD Premium ili AAD osnovni dodijeljene administrator obavljanju ovog postupka. </p>
              <p>Da biste to nedostajući, dodijelite licencu AAD Premium ili AAD osnovni za administratorskog računa tako da odete na karticu <strong>licenci</strong> i pokušajte ponovno.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ne vidim bilo koju od mogućnosti konfiguracije u odjeljku <strong>Pravila za ponovno postavljanje lozinke korisnika</strong> koji su opisani u dokumentaciji.</p>
            </td>
            <td>
              <p>Vidljiv je u odjeljku <strong>Pravila za ponovno postavljanje lozinke korisničkog </strong>, ali samo zastavice koja će se pojaviti ispod nje je zastavicom <strong>Korisnicima omogućiti za ponovno postavljanje lozinke</strong> .</p>
            </td>
            <td>
              <p>Ostale korisničkog sučelja prikazat će se kada se prebacite zastavice <strong>Korisnicima omogućiti za ponovno postavljanje lozinke</strong> za <strong>da.</strong></p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ne prikazuje mogućnost određeni konfiguracije.</p>
            </td>
            <td>
              <p>Na primjer, ne vidim mogućnost <strong>broj dana prije korisnika morate potvrditi svoje podatke za kontakt</strong> kada se pomicati u odjeljku <strong>Pravila za ponovno postavljanje lozinke korisničkog</strong> (ili još primjera isti problem).</p>
            </td>
            <td>
              <p>Mnoge elemente korisničkog sučelja nisu vidljivi dok su potrebne. Pokušajte Omogućivanje mogućnosti na stranici ako želite vidjeti.</p>
              <p>Dodatne informacije o sve kontrole koje su vam na raspolaganju potražite u članku <a href="active-directory-passwords-customize.md#password-management-behavior">Upravljanje lozinkama ponašanje</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ne prikazuje mogućnost konfiguracije <strong>Pisanje natrag lozinke na lokalni</strong></p>
            </td>
            <td>
              <p>Mogućnost <strong>Pisanje natrag lozinke na lokalni</strong> nije vidljiva na kartici <strong>Konfiguriraj</strong> na portalu za upravljanje Azure</p>
            </td>
            <td>
              <p>Ta mogućnost vidljiv je samo ako ste preuzeli Azure AD Connect i konfigurirali Upisima lozinku. Kada to učinite, tu mogućnost pojavit će se i omogućuje vam da biste omogućili ili onemogućili upisima iz oblaka.</p>
              <p>Dodatne informacije o tome kako to učiniti potražite u članku <a href="active-directory-passwords-getting-started.md#step-2-enable-password-writeback-in-azure-ad-connect">Omogućivanje Upisima lozinku u Azure AD Connect</a> .</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-management-reports-in-the-azure-management-portal"></a>Otklanjanje poteškoća s lozinku upravljanje izvješća na portalu za upravljanje Azure
Ako naiđete na pogrešku prilikom korištenja upravljanja izvješća lozinke, možda nećete moći Razrješavanje slijedeći dolje navedene upute za otklanjanje poteškoća:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Pogreška slučaja</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Što je pogreška vidjeti korisnika?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Rješenja</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ne vidim sve izvješća upravljanja lozinkom</p>
            </td>
            <td>
              <p>Izvješća o <strong>aktivnosti za ponovno postavljanje lozinke</strong> i <strong>aktivnosti registraciju za ponovno postavljanje lozinke</strong> nisu vidljive u odjeljku <strong>Zapisnik aktivnosti</strong> izvješća na kartici <strong>izvješća</strong> .</p>
            </td>
            <td>
              <p>To se može dogoditi ako nemate licencu za AAD Premium ili AAD osnovni dodijeljene administrator obavljanju ovog postupka. </p>
              <p>Da biste to nedostajući, dodijelite licencu AAD Premium ili AAD osnovni za administratorskog računa tako da odete na karticu <strong>licenci</strong> i pokušajte ponovno.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Registracija korisnika Pokaži više puta</p>
            </td>
            <td>
              <p>Kada korisnik registrira zamjenska e-pošte, mobilnog telefona i sigurnosna pitanja, svaki prikazuju se kao zasebna crte umjesto s jednim retkom.</p>
            </td>
            <td>
              <p>Trenutno kada korisnik registrira, ne možemo ne pretpostavlja da oni će registrirati sve na stranici za registraciju. Kao rezultat ćemo trenutno prijavite se svaki pojedinačne podatak koja je registrirana kao zasebna događaj.</p>
              <p>Ako želite aggregate te podatke, možete preuzeti izvješća i otvorite podatke kao što je zaokretne tablice u programu excel da biste imali više fleksibilnosti.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Otklanjanje poteškoća s portala za registraciju ponovno postavljanje lozinke
Ako naiđete na pogrešku prilikom Registracija korisnika za ponovno postavljanje lozinke, možda nećete moći Razrješavanje slijedeći dolje navedene upute za otklanjanje poteškoća:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Pogreška slučaja</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Što je pogreška vidjeti korisnika?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Rješenja</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Imenik nije omogućen za ponovno postavljanje lozinke</p>
            </td>
            <td>
              <p>Administrator je omogućio koristiti ovu značajku.</p>
            </td>
            <td>
              <p>Prijeđite zastavice <strong>Korisnicima omogućiti za ponovno postavljanje lozinke</strong> <strong>da</strong> i kliknite <strong>Spremi</strong> na kartici konfiguracije direktorija za Portal za upravljanje Azure. Morate imati Azure AD Premium ili osnovni licenca dodijeljena administrator obavljanju ovog postupka.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Korisnik nema licencu za AAD Premium ili AAD osnovni dodijeljeno</p>
            </td>
            <td>
              <p>Administrator je omogućio koristiti ovu značajku.</p>
            </td>
            <td>
              <p>Dodijelite licencu za Azure AD Premium ili Azure AD osnovni korisniku na kartici <strong>licence</strong> na portalu za upravljanje Azure. Morate imati Azure AD Premium ili osnovni licenca dodijeljena administrator obavljanju ovog postupka.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Zahtjev za obradu pogreške</p>
            </td>
            <td>
              <p>Korisnik vidi pogrešku statusi:</p>
              <p>

              </p>
              <p>Zahtjev za obradu pogreške </p>
              <p>Prilikom pokušaja ponovno postavljanje lozinke.</p>
            </td>
            <td>
              <p>To može uzrokovati različite uzroke, ali obično tu pogrešku uzrokuje ili na prekida ili konfiguracija problem sa servisom koje nije moguće riješiti. </p>
              <p>Ako ta se pogreška, a to je koje utječu na svoje tvrtke, obratite se podršci i pomoći ćemo vam HITNE. Pogledajte <a href="#information-to-include-when-you-need-help">informacije da biste dodali kada vam je potrebna pomoć</a> da biste vidjeli što mora sadržavati da biste inženjer za podršku da biste olakšali brzi rješenja.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-portal"></a>Otklanjanje poteškoća s portala za ponovno postavljanje lozinke
Ako naiđete na pogrešku prilikom ponovno postavljanje lozinke za korisnika, možda nećete moći Razrješavanje slijedeći dolje navedene upute za otklanjanje poteškoća:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Pogreška slučaja</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Što je pogreška vidjeti korisnika?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Rješenja</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Imenik nije omogućen za ponovno postavljanje lozinke</p>
            </td>
            <td>
              <p>Vaš račun nije omogućen za ponovno postavljanje lozinke</p>
              <p>Nažalost, ali vaš administrator nije postavio vaš račun za taj servis. </p>
              <p>

              </p>
              <p>Ako želite, možete smo obratite se administratoru u tvrtki ili ustanovi da biste ponovno postavili lozinku.</p>
            </td>
            <td>
              <p>Prijeđite zastavice <strong>Korisnicima omogućiti za ponovno postavljanje lozinke</strong> <strong>da</strong> i kliknite <strong>Spremi</strong> na kartici konfiguracije direktorija za Portal za upravljanje Azure. Morate imati Azure AD Premium ili osnovni licenca dodijeljena administrator obavljanju ovog postupka.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Korisnik nema licencu za AAD Premium ili AAD osnovni dodijeljeno</p>
            </td>
            <td>
              <p>Dok se ne možemo ne mogu ponovno postavljati lozinke za račun koji nisu administratori automatski, ne možemo možete pitajte administratora svoje tvrtke ili ustanove to učiniti.</p>
            </td>
            <td>
              <p>Dodijelite licencu za Azure AD Premium ili Azure AD osnovni korisniku na kartici <strong>licence</strong> na portalu za upravljanje Azure. Morate imati Azure AD Premium ili osnovni licenca dodijeljena administrator obavljanju ovog postupka.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Imenik je omogućen za ponovno postavljanje lozinke, ali korisnik ima nedostaju ili su mal oblikovan podatke za provjeru autentičnosti</p>
            </td>
            <td>
              <p>Vaš račun nije omogućen za ponovno postavljanje lozinke</p>
              <p>Nažalost, ali vaš administrator nije postavio vaš račun za taj servis. </p>
              <p>

              </p>
              <p>Ako želite, možete smo obratite se administratoru u tvrtki ili ustanovi da biste ponovno postavili lozinku.</p>
            </td>
            <td>
              <p>Provjerite je li korisnik ispravno oblikovana podatke o kontaktima na datoteku u direktoriju prije nastavka. Informacije o konfiguriranju podatke za provjeru autentičnosti u imeniku tako da korisnici vide ta se pogreška potražite <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">podataka koje koriste ponovno postavljanje lozinke</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Imenik je omogućen za ponovno postavljanje lozinke, ali korisnik ima samo jednog dijela podataka o kontaktima na datoteci kada je pravilnik postavljen da je potrebna dva koraka za potvrdu</p>
            </td>
            <td>
              <p>Vaš račun nije omogućen za ponovno postavljanje lozinke</p>
              <p>Nažalost, ali vaš administrator nije postavio vaš račun za taj servis. </p>
              <p>

              </p>
              <p>Ako želite, možete smo obratite se administratoru u tvrtki ili ustanovi da biste ponovno postavili lozinku.</p>
            </td>
            <td>
              <p>Provjerite je li taj korisnik ima najmanje dva pravilno konfigurirani metode kontakta (npr., mobilnog telefona i telefona) prije nastavka. Informacije o konfiguriranju podatke za provjeru autentičnosti u imeniku tako da korisnici vide ta se pogreška potražite <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">podataka koje koriste ponovno postavljanje lozinke</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Direktorija omogućena za ponovno postavljanje lozinke i korisničkog ispravno konfigurirano, ali korisnik ne može uspostaviti </p>
            </td>
            <td>
              <p>Oops!  Ne možemo naišao je do neočekivane pogreške tijekom kontaktirati.</p>
            </td>
            <td>
              <p>To može biti rezultat pogreška privremene servisa ili pogrešno podataka o kontaktima koji ne možemo pravilno pronaći. Ako korisnik čeka 10 sekundi, pokušajte ponovno i veza na "obratite se administratoru" pojavljuje se. Pokušajte ponovno kliknuti će ponovno isporuka poziva, dok je kliknete "obratite se administratoru" će poslati obrazac e-pošte korisnika, lozinka ili globalni administratori (tim redoslijedom prvenstva) zahtjev za vraćanje izvorne lozinke koje je potrebno izvršiti za taj račun.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Korisnik prima nikad ponovno postavljanje lozinke SMS i telefonskog poziva</p>
            </td>
            <td>
              <p>Korisnik klikova "tekst me" ili "Nazovi me", a zatim uopće ne primi ništa.</p>
            </td>
            <td>
              <p>To može biti rezultat mal oblikovan telefonskog broja u direktoriju. Provjerite je li telefonski broj u obliku "+ ccc xxxyyyzzzzXeeee". Dodatne informacije o oblikovanju telefonske brojeve potrebne za ponovno postavljanje lozinke potražite u članku <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">podataka koje koriste ponovno postavljanje lozinke</a>.</p>
              <p>Ako tražite datotečni nastavak da biste se usmjeriti u pitanju korisniku, imajte na umu te ponovno postavljanje lozinke ne podržava proširenja, čak i ako odredite u imeniku (se uklanjaju prije poziv je poslanih). Pokušajte broj bez datotečni nastavak ili integriranje proširenje u telefonski broj u vašem PBX.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Korisnik nikad ne prima e-pošte za ponovno postavljanje lozinke</p>
            </td>
            <td>
              <p>Korisnik klikne "e-pošte me" i zatim uopće ne primi ništa.</p>
            </td>
            <td>
              <p>Najčešći uzrok problema je da je poruku odbio filtra za neželjenu poštu. Provjerite neželjene e-pošte, mapu Bezvrijedna ili izbrisane stavke za e-poštu.</p>
              <p>Provjeriti provjeravate desnom e-pošte za isporuku poruke... velikim brojem ljudi imaju slične adrese e-pošte i završe Provjera pogrešan ulazne pošte za isporuku poruke. Ako nijedan od tih mogućnosti funkcioniraju, moguće je i je li adresa e-pošte u imeniku oblikovan, provjerite provjerite je li adresa e-pošte odgovarajuću domenu i pokušajte ponovno. Dodatne informacije o oblikovanju e-pošte adrese za ponovno postavljanje lozinke potražite u članku <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">podataka koje koriste ponovno postavljanje lozinke</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Koje ste postavili pravila za ponovno postavljanje lozinke, ali kada administratorskog računa koristi ponovno postavljanje lozinke, to pravilo nije primijenjena</p>
            </td>
            <td>
              <p>Administratorske račune ponovno postavljanje lozinke potražite u članku iste mogućnosti omogućiti za ponovno postavljanje lozinke, e-pošte i mobilni telefon, bez obzira na to je što pravilo postavljeno u odjeljku <strong>Pravila za ponovno postavljanje lozinke korisničkog</strong> <strong>Konfiguriraj</strong> kartice.</p>
            </td>
            <td>
              <p>Mogućnosti konfiguriran u odjeljku <strong>Pravila za ponovno postavljanje lozinke korisničkog</strong> kartice <strong>Konfiguriraj</strong> primijeniti samo na krajnjim korisnicima u tvrtki ili ustanovi.</p>
              <p>Microsoft upravlja i kontrole administratorsku lozinku ponovno postavljanje pravila da biste bili sigurni najvišu razinu sigurnosti</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Korisnik spriječio pokušaja za vraćanje izvorne lozinke Premašili broj pokušaja u dana</p>
            </td>
            <td>
              <p>Korisnik vidi o pogrešci:</p>
              <p>

              </p>
              <p>Koristite neku drugu mogućnost.</p>
              <p>Pokušali ste premašili broj pokušaja u zadnje 1 sati provjerite je li vaš račun. Sigurnosnih vam razloga, morat ćete Pričekajte 24 sati prije nego što možete pokušajte ponovno. </p>
              <p>Ako želite, možete smo obratite se administratoru u tvrtki ili ustanovi da biste ponovno postavili lozinku.</p>
            </td>
            <td>
              <p>Ne možemo implementirati automatsko regulacije mehanizam bloka korisnicima iz pokuša ponovno postavljanje lozinke Premašili broj pokušaja u kratkom vremenskom razdoblju. To se događa kada:</p>
              <ol class="ordered">
                <li>
Korisnik pokuša provjeru telefonskog broja 5 puta u jedan sat.<br\><br\></li>
                <li>
Korisnik nije pokušava koristiti prolaz pitanja sigurnosti 5 puta u jedan sat.<br\><br\></li>
                <li>
Korisnik pokuša ponovno postavljanje lozinke za isti korisnički račun 5 puta u jedan sat.<br\><br\></li>
              </ol>
              <p>Da biste riješili taj problem, uputite korisnika u 24 sata nakon zadnjeg pokušaj i korisnik bit će moći ponovno postavljanje lozinke za svoj.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Korisnik vidi pogreške pri provjeri valjanosti broj telefona</p>
            </td>
            <td>
              <p>Pri pokušaju da biste provjerili telefona da biste koristili kao način provjere autentičnosti, korisnik ne vidi o pogrešci:</p>
              <p>

              </p>
              <p>Netočan broj naveden.</p>
            </td>
            <td>
              <p>Ta se pogreška pojavljuje kada ste unijeli telefonski broj podudaraju telefonski broj na datoteci.</p>
              <p>Provjerite je li korisnik unose potpuni telefonski broj, uključujući područje i Država kod, prilikom pokušaja korištenja metode telefona za ponovno postavljanje lozinke.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Zahtjev za obradu pogreške</p>
            </td>
            <td>
              <p>Korisnik vidi pogrešku statusi:</p>
              <p>

              </p>
              <p>Zahtjev za obradu pogreške </p>
              <p>Prilikom pokušaja ponovno postavljanje lozinke.</p>
            </td>
            <td>
              <p>To može uzrokovati različite uzroke, ali obično tu pogrešku uzrokuje ili na prekida ili konfiguracija problem sa servisom koje nije moguće riješiti. </p>
              <p>Ako ta se pogreška, a to je koje utječu na svoje tvrtke, obratite se podršci i pomoći ćemo vam HITNE. Pogledajte <a href="#information-to-include-when-you-need-help">informacije da biste dodali kada vam je potrebna pomoć</a> da biste vidjeli što mora sadržavati da biste inženjer za podršku da biste olakšali brzi rješenja.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback"></a>Otklanjanje poteškoća s Upisima lozinke
Ako naiđete na pogrešku prilikom Omogućivanje, onemogućivanje ili pomoću lozinke Upisima, možda nećete moći Razrješavanje slijedeći dolje navedene upute za otklanjanje poteškoća:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Pogreška slučaja</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Što je pogreška vidjeti korisnika?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Rješenja</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Za uhodavanje Općenito i pokretanje neuspjeha</p>
            </td>
            <td>
              <p>Lokalno uz poruku o pogrešci 6800 u zapisniku događaja na računalu Azure AD Connect aplikacija ne pokreće servis za ponovno postavljanje lozinke.</p>
              <p>

              </p>
              <p>Nakon za uhodavanje vanjskog ili lozinku raspršivanje sinkroniziranih korisnika ne mogu ponovno postavljati lozinke.</p>
            </td>
            <td>
              <p>Kada je omogućen Upisima lozinku, modula za sinkroniziranje će poziv upisima biblioteke da biste izvršili konfiguraciju (za uhodavanje) razgovor sa servisa za uhodavanje u oblaku. Sve pronađene tijekom za uhodavanje ili prilikom pokretanja WCF krajnja točka za lozinku Upisima pogreške rezultirat će pogreške u zapisniku događaja u zapisniku događaja na računalu Azure AD Connect.</p>
              <p>Prilikom ponovnog pokretanja servisa ADSync, ako je konfiguriran upisima, krajnju točku WCF pokrenut će se prema gore. Međutim, krajnje točke za pokretanje ne uspije, ne možemo će jednostavno zapisivanje događaja 6800 i omogućuju pokretanje servisa za sinkronizaciju. Prisutnost ovaj događaj znači da Upisima lozinku krajnje točke nije pokrenuto prema gore. Zapisnik događaja detalje o tom događaju (6800) uz unose u zapisnik događaja generirati po PasswordResetService komponente će vas upozoriti Zašto krajnju točku nije moguće pokrenuti prema gore. Pregledajte te pogreške zapisnik događaja i pokušajte ponovno pokrenuti Azure AD Connect ako Upisima lozinku i dalje ne funkcionira. Ako se problem nastavi pojavljivati, pokušajte onemogućivanje i ponovno Omogućivanje Upisima lozinku.</p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Kada se korisnik pokuša ponovno postavljanje lozinke ili otključavanje račun s upisima lozinku omogućena, postupak neće uspjeti. K tome, pročitajte članak događaja koji sadrži Azure AD Connect zapisnik događaja: "program sinkronizacije vraća se pogreška hr = 800700CE, poruke = naziv datoteke ili proširenje predug" kada se pojavi operacija otključaj.
                            </p>
            </td>
            <td>
              <p>To se može dogoditi ako ste nadogradili iz starije verzije programa Azure AD Connect ili DirSync. Nadogradnja starijih verzija Azure AD Connect postavite lozinku 254 znaka za Azure AD Management Agent (novijim verzijama postavit duljinu lozinke 127 znakova). Tako dugo lozinke radi AD poveznik uvoz i izvoz operacije, ali ne podržava operaciju otključaj.
                            </p>
            </td>
            <td>
              <p>[Pronalaženje Active Directory račun](active-directory-aadconnect-accounts-permissions.md#active-directory-account) za Azure AD Connect i ponovno postavljanje lozinke koja će sadržavati najviše 127 znakova. Zatim otvorite **Servis za sinkronizaciju** s izbornika Start. Dođite do **poveznika** i pronaći **Poveznik za Active Directory**. Odaberite ga, a zatim kliknite **Svojstva**. Idite na stranicu **vjerodajnice** i unesite novu lozinku. Odaberite **u redu** da biste zatvorili stranicu.
                            </p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Pogreška pri konfiguraciji upisima tijekom instalacije Azure AD Connect.</p>
            </td>
            <td>
              <p>U posljednjem koraku postupka instalacije Azure AD Connect, vidjet ćete pogrešku Upisima lozinke ne može konfigurirati.</p>
              <p>

              </p>
              <p>Zapisnik događaja aplikacije povezivanje Azure AD sadrži pogrešku 32009 s tekstom "Pogreška početak provjere autentičnosti tokena".</p>
            </td>
            <td>
              <p>Ta se pogreška pojavljuje se u sljedećim slučajevima dva:</p>
              <ul>
                <li class="unordered">
Koje ste naveli netočnu lozinku za račun globalnog administratora naveden na početku postupak instalacije Azure AD Connect.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Pokušali ste koristiti pridruženi korisnik navedenog na početku postupak instalacije Azure AD Connect računa globalnog administratora.<br\><br\></li>
              </ul>
              <p>Da biste ispravili tu pogrešku, provjerite je li niste pomoću pridruženim računa za globalni administrator koje ste naveli na početku postupak instalacije Azure AD Connect i lozinka navedena ispravna.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Pogreška pri konfiguraciji upisima tijekom instalacije Azure AD Connect.</p>
            </td>
            <td>
              <p>Zapisnik događaja na računalu Azure AD Connect sadrži pogrešku 32002 izbačena tako da na PasswordResetService.</p>
              <p>

              </p>
              <p>Čita pogreška: "Pogreška povezujete s ServiceBus, tokena davatelj nije uspio dati sigurnosni token..."</p>
              <p>

              </p>
            </td>
            <td>
              <p>Uzrok te pogreške jest da servis za ponovno postavljanje lozinke radi u lokalnog okruženja ne može se povezati s bus krajnja točka servisa u oblaku. Ta se pogreška obično obično je uzrokovano pravilo vatrozid blokira izlazne veze na određeni priključak ili web-adresu.</p>
              <p>

              </p>
              <p>Provjerite dopušta li vatrozid izlazne veze za sljedeće:</p>
              <ul>
                <li class="unordered">
Sav promet putem TCP 443 (HTTPS)<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Izlazne veze <br\><br\></li>
              </ul>
              <p>

              </p>
              <p>Kada ste ažurirali ta pravila, ponovno pokrenuti računalo Azure AD Connect i Upisima lozinka trebali biste radom.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Lozinka Upisima krajnjoj točki lokalno nije dostupna</p>
            </td>
            <td>
              <p>Nakon radi za neke vrijeme vanjskog ili lozinku raspršivanje sinkroniziranih korisnika ne mogu ponovno postavljati lozinke.</p>
            </td>
            <td>
              <p>U nekim slučajevima rijetko Upisima lozinku servis možda neće uspjeti da biste ponovno pokrenuli kada je ponovno pokrenuti Azure AD Connect. U tim slučajevima, najprije provjerite hoće li Upisima lozinku čini se da je omogućena na-prem. To možete učiniti pomoću čarobnjaka za Azure AD Connect ili powershell (sekcija potražite u članku HowTos gore). Ako se značajka omogućiti, pokušajte omogućivati ili onemogućivati značajku ponovno korisničkog Sučelja ili PowerShell. U odjeljku "korak 2: Omogućivanje Upisima lozinku na računalu Sinkroniziranje direktorija &amp; konfigurirati pravila vatrozida" u <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">kako omogućiti ili onemogućiti lozinku Upisima</a> dodatne informacije o tome kako to učiniti.</p>
              <p>

              </p>
              <p>Ako to ne funkcionira, pokušajte potpune deinstalacije i ponovno instaliranje Azure AD Connect.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Dozvole pogreške</p>
            </td>
            <td>
              <p>Pridružene ili sinkronizaciju lozinke raspršivanje poslati korisnicima koji pokušaj vraćanja njihove lozinke potražite u članku pogreške nakon slanja lozinku koja označava došlo je do problema servisa.</p>
              <p>

              </p>
              <p>Osim toga, tijekom operacije ponovno postavljanje lozinke, možda ćete vidjeti pogreške vezane uz upravljanje agent odbijen pristup u vašem na lokalni zapisnika događaja.</p>
            </td>
            <td>
              <p>Ako vidite te pogreške u vašem zapisnik događaja, provjerite ima li AD MA račun (koji naveden u čarobnjaku za vrijeme konfiguraciju) potrebne dozvole za Upisima lozinku.</p>
              <p>

              </p>
              <p>Imajte na UMU da kada je zadan ovom dozvolom može potrajati do 1 sat dozvole za trickle dolje putem sdprop pozadinski zadatak na kontroler domene. </p>
              <p>Za ponovno postavljanje rad lozinke, dozvolu mora biti oznaku na sigurnosni opis objekta korisnika čija je u tijeku za ponovno postavljanje lozinke. Dok tu dozvolu prikazuje na korisničkom objektu, ponovno postavljanje lozinke i dalje će biti neuspješan uz pristup zabranjen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Pogreška prilikom konfiguriranja Upisima lozinke iz čarobnjaka za konfiguraciju Azure AD Connect </p>
            </td>
            <td>
              <p>Pogreška "Nije moguće pronađite MA" u čarobnjaku za tijekom Omogućivanje i onemogućivanje Upisima lozinke</p>
            </td>
            <td>
              <p>Postoji poznatih problema u objavljenu verziju Azure AD Connect koji manifesti u sljedećim situacijama:</p>
              <ol class="ordered">
                <li>
Konfiguriranje Azure AD Connect za klijent abc.com (domena je potvrđena) pomoću creds. Rezultat AAD poveznik s nazivom "abc.com – AAD" stvoren.<br\><br\></li>
                <li>
Pa zatim promijenite creds AAD poveznik (pomoću stare sinkronizaciju korisničkog Sučelja) (Imajte na umu je istom klijentu, ali drugi naziv domene). <br\><br\></li>
                <li>
Sada pokušate omogućiti ili onemogućiti Upisima lozinku. Čarobnjak će izgraditi naziv poveznik za korištenje na creds kao "abc.onmicrosoft.com – AAD" i prenesite cmdlet Upisima lozinku. To se neće uspjeti jer nema poveznik stvorene pomoću tog naziva.<br\><br\></li>
              </ol>
              <p>To je riješen u našem najnovija izdanja. Ako imate starije Sastavi, jedan zaobilazno rješenje je pomoću cmdleta ljuske powershell omogućiti ili onemogućiti značajke. U odjeljku "korak 2: Omogućivanje Upisima lozinku na računalu Sinkroniziranje direktorija &amp; konfigurirati pravila vatrozida" u <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">kako omogućiti ili onemogućiti lozinku Upisima</a> dodatne informacije o tome kako to učiniti.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nije moguće ponovno postavljanje lozinke za korisnike u posebne grupe kao što su administratori domene / Enterprise administratori itd.</p>
            </td>
            <td>
              <p>Pridružene ili sinkronizaciju lozinke raspršivanje poslati korisnicima koji su dio grupe zaštićene i pokušajte ponovno postaviti svoje lozinke potražite u članku pogreške nakon slanja lozinku koja označava došlo je do problema servisa.</p>
            </td>
            <td>
              <p>Povlaštene korisnike u servisu Active Directory zaštićeni su AdminSDHolder. <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">Http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a> dodatne pojedinosti potražite u članku. </p>
              <p>

              </p>
              <p>To znači opisnika sigurnosti na te objekte provjeravaju se povremeno tako da odgovara jedan navedeno u AdminSDHolder i vratiti ako se razlikuju. Dodatne dozvole koje su vam potrebne za lozinku Upisima stoga ne trickle takve korisnicima. To može rezultirati Upisima lozinka ne funkcionira kao što je korisnicima. Zbog toga ne podržavamo upravljanje lozinkama za korisnike unutar te grupe jer je prijelomi model sigurnosti AD.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nije moguće pronaći Vrati ne uspije operacije s objektom</p>
            </td>
            <td>
              <p>Pridružene ili sinkronizaciju lozinke raspršivanje poslati korisnicima koji pokušaj vraćanja njihove lozinke potražite u članku pogreške nakon slanja lozinku koja označava došlo je do problema servisa.</p>
              <p>

              </p>
              <p>Osim toga, tijekom operacije ponovno postavljanje lozinke, vidjet ćete pogrešku u evidenciji iz servisa Azure AD Connect u kojoj stoji da se pogreška "Nije moguće pronaći objekt".</p>
            </td>
            <td>
              <p>Ta se pogreška obično znači da je modula za sinkroniziranje nije moguće pronaći korisnički objekt u poveznik prostor AAD ili povezane MV ili AD poveznik prostor objekta. </p>
              <p>

              </p>
              <p>Da biste to riješili, provjerite je li korisnik uistinu sinkronizira s lokalno AAD putem trenutne instance Azure AD Connect i provjera stanja objekata u poveznik razmaka i MV. Provjerite je li objekt AD CS poveznika objekt MV putem pravila "Microsoft.InfromADUserAccountEnabled.xxx".</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ponovno postavljanje operacije ne uspije s više eror pronađena podudaranja</p>
            </td>
            <td>
              <p>Pridružene ili sinkronizaciju lozinke raspršivanje poslati korisnicima koji pokušaj vraćanja njihove lozinke potražite u članku pogreške nakon slanja lozinku koja označava došlo je do problema servisa.</p>
              <p>

              </p>
              <p>Osim toga, tijekom operacije ponovno postavljanje lozinke, vidjet ćete pogrešku u evidenciji iz servisa Azure AD Connect koja upućuje na pogrešku "Više maches pronađena".</p>
            </td>
            <td>
              <p>To označava da modula za sinkroniziranje prepoznaje je li MV objekt povezan s više objekata AD CS putem "Microsoft.InfromADUserAccountEnabled.xxx". To znači da korisnik ima račun omogućeni u više od jednog skupa stabala. </p>
              <p>

              </p>
              <p>Trenutno scenarij nije podržana za Upisima lozinku.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Lozinka operacije neće funkcionirati uz pogreške u konfiguraciji.</p>
            </td>
            <td>
              <p>Lozinka operacije neće funkcionirati uz pogreške u konfiguraciji. Zapisnik događaja aplikacije sadrži pogrešku Azure AD Connect 6329 s tekstom: 0x8023061f (operacija nije uspjelo jer na Agent za upravljanje nije omogućena sinkronizacija lozinku.)</p>
            </td>
            <td>
              <p>To se događa ako konfiguracije Azure AD Connect mijenja se da biste dodali&nbsp;novi skup stabala AD (ili za uklanjanje i ponovno dodavanje postojeći skup stabala) <strong>Kada</strong> je značajka Upisima lozinku omogućena. Operacije lozinke za korisnike u takvim novododani šuma neće uspjeti. Da biste riješili taj problem, onemogućivanje i ponovno Omogućivanje značajku lozinke Upisima nakon promjene konfiguracije skupa stabala dovršite.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ponovno pisanje lozinke koje ste vraćena tako da korisnici funkcionira ispravno, ali ponovno pisanje lozinke promijenio korisnika ili Vrati izvorne postavke za korisnika administrator neće uspjeti.</p>
            </td>
            <td>
              <p>Kada se pokuša ponovno postavljanje lozinke ime korisnika s portala za upravljanje Azure, vidite poruku koja govori: "servis za ponovno postavljanje lozinke radi u lokalnog okruženja ne podržava ponovno postavljanje korisničke lozinke za administratore. Nadogradite na najnoviju verziju programa Azure AD Connect da biste riješili taj problem."</p>
            </td>
            <td>
              <p>To se događa kada verziju modula za sinkronizaciju ne podržava određenu operaciju Upisima lozinku koja se koristila. Verzija Azure AD Connect kasnije od 1.0.0419.0911 podržava sve operacije upravljanja lozinkom, uključujući lozinku ponovno postavite upisima, upisima Promijeni lozinku i upisima ponovno postavljanje lozinke administratora pokrenut na portalu za upravljanje Azure.&nbsp; DirSync verzije kasnije od 1.0.6862 podržavaju samo upisima ponovno postavljanje lozinke. Da biste riješili taj problem, preporučujemo da instalirate najnoviju verziju Azure AD Connect ili Azure Active Directory povezivanje (Dodatne informacije potražite u odjeljku <a href="active-directory-aadconnect">Alati za integraciju direktorija</a>) da biste riješili taj problem i korištenje svih pogodnosti Upisima lozinku u tvrtki ili ustanovi.</p>
            </td>
          </tr>
        </tbody></table>


## <a name="password-writeback-event-log-error-codes"></a>Kodovi pogrešaka za lozinku Upisima zapisnik događaja
Najbolja praksa pri otklanjanju poteškoća s Upisima lozinka je provjera tu aplikaciju zapisnik događaja na računalu Azure AD Connect. U ovom zapisnik događaja sadržavat će događaje iz dva izvora koji vas zanimaju za Upisima lozinku. Izvor PasswordResetService opisane su operacije i problema vezanih uz operacija Upisima lozinku. Izvor ADSync opisane su operacije i problema vezanih uz postavljanje lozinke u svom okruženju AD.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Kod</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Naziv / poruka</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Izvor</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Opis</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>BAIL: MMS(4924) 0x80230619 – "ograničenja onemogućuje lozinku mijenja se u trenutni one koja je navedena."</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Ovaj se događaj kada servis Upisima lozinku pokuša postavite lozinku na lokalnom direktoriju koji ne zadovoljava starost lozinke, povijest, složenosti ili filtriranja zahtjevima domene.</p>
              <ul>
                <li class="unordered">
Ako imate starost minimalne lozinke i nedavno promijenili lozinku u tom prozoru vrijeme, nećete moći promijeniti lozinku ponovno sve dok ne postignete navedeni dob u vašoj domeni. Za testiranje, minimalno mora biti postavljeno na 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ako imate omogućen zahtjeve za lozinku povijest, a zatim odaberite lozinku koju ne koristi u zadnji N puta pri čemu je N povijest postavku lozinke. Ako ste odabrali lozinku kojom u zadnji N puta, zatim vidjet ćete pogreške u tom slučaju. Za testiranje, povijest mora biti postavljeno na 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ako imate složenosti lozinki, sve ih provest će se kada se korisnik pokuša promjena ili ponovno postavljanje lozinke.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ako imate lozinku filtri omogućeno i korisnik odabere lozinku koji zadovoljavaju kriterij filtriranja, zatim ponovno postavljanje ili promjena operacija neće uspjeti.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>Program sinkronizacije vraća se pogreška hr = 80230402, poruke = pokušaj dohvaćanja objekt nije uspjelo jer je dvostruko stavke s istom sidro</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Ovaj se događaj kada isti korisničkog id-a je omogućen u više domena.  Na primjer, ako su sinkroniziranje šuma računa/resursa, a imaju isti korisničkog id-a izlaganje i omogućena u svakoj, ta se pogreška može pojaviti.  </p>
              <p>Ta se pogreška može pojaviti i ako koristite atributa koji nisu jedinstveni sidro (kao što je pseudonim ili UPN-a) i dva korisnici zajednički koristiti iste atribut sidra.</p>
              <p>Da biste riješili taj problem, provjerite da ne morate dupliciranu korisnici unutar domene i koristite jedinstvene sidro atribut za svakog korisnika.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava lokalnog servisa otkriven zahtjev za s pridruženim za vraćanje izvorne lozinke ili sinkronizaciju lozinke raspršivanje poslati korisnik koji potječu iz oblaka. Ovaj događaj je prvi događaja u svakoj lozinku ponovno postavljanje upisima postupak.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava da korisnik odabrali novu lozinku tijekom operacije za ponovno postavljanje lozinke, utvrdili smo da ovu lozinku zadovoljava preduvjete tvrtke lozinku, a lozinku je uspješno napisan natrag u lokalnom okruženju AD.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj znači da korisnik odabrana lozinku, a da lozinku uspješno stigli da biste lokalnog okruženja, ali kad pokušao ne možemo postaviti lozinku u lokalnom okruženju AD, pojavila se pogreška. To se može dogoditi iz nekoliko razloga:</p>
              <ul>
                <li class="unordered">
Korisničke lozinke ne zadovoljava dob, povijest, složenosti ili filtrirati preduvjeti za domenu. Pokušajte potpuno novu lozinku da biste riješili taj problem.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Račun servisa MA nemate odgovarajuće dozvole da biste postavili novu lozinku na korisnički račun.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Na korisnički račun se zaštićene grupu, primjerice domene ili enterprise administratori koji ne dopušta operacije postavljanje lozinke.<br\><br\></li>
              </ul>
              <p>Potražite u članku <a href="#troubleshoot-password-writeback">Otklanjanje poteškoća s Upisima lozinku</a> da biste saznali više o drugim situtions mogu prouzročiti ta se pogreška.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj pojavljuje ako omogućite Upisima lozinku s Azure AD Connect i ukazuje na to da ne možemo pokrenuti za uhodavanje vašoj tvrtki ili ustanovi web-servisa Upisima lozinku.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj upućuje na to je za uhodavanje postupak uspio i je li lozinka Upisima mogućnost spreman za korištenje.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava da servis lokalnog otkrio zahtjev za promjenu lozinke za s pridruženim ili sinkronizaciju lozinke raspršivanje poslati korisnik koji potječu iz oblaka. Ovaj događaj je prvi događaj u svaki postupak za promjenu upisima lozinku.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava da korisnik odabrali novu lozinku tijekom operacije Promjena lozinke, utvrdili smo da ovu lozinku zadovoljava preduvjete tvrtke lozinku, a lozinku je uspješno napisan natrag u lokalnom okruženju AD.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj znači da korisnik odabrana lozinku, a da lozinku uspješno stigli da biste lokalnog okruženja, ali kad pokušao ne možemo postaviti lozinku u lokalnom okruženju AD, pojavila se pogreška. To se može dogoditi iz nekoliko razloga:</p>
              <ul>
                <li class="unordered">
Korisničke lozinke ne zadovoljava dob, povijest, složenosti ili filtrirati preduvjeti za domenu. Pokušajte potpuno novu lozinku da biste riješili taj problem.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Račun servisa MA nemate odgovarajuće dozvole da biste postavili novu lozinku na korisnički račun.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Na korisnički račun se zaštićene grupu, primjerice domene ili enterprise administratori koji ne dopušta operacije postavljanje lozinke.<br\><br\></li>
              </ul>
              <p>Potražite u članku <a href="#troubleshoot-password-writeback">Otklanjanje poteškoća s Upisima lozinku</a> da biste saznali više o drugim slučajevima može uzrokovati ta se pogreška.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Servis za lokalni otkrio zahtjev za s pridruženim za vraćanje izvorne lozinke ili sinkronizaciju lozinke raspršivanje poslati korisničko ime korisnika potječu od administratora. Ovaj događaj je prvi događaj u svaki postupak upisima za ponovno postavljanje lozinke za administratore pokrenut.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Administrator sustava odaberete novu lozinku tijekom operacije za ponovno postavljanje lozinke za administratore pokrenut, utvrdili smo da ovu lozinku zadovoljava preduvjete tvrtke lozinku, a zatim lozinku je uspješno napisan natrag na lokalni okruženje za AD.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Administrator odabrali lozinke ime korisnika i lozinku uspješno stigli da biste lokalnog okruženja, no kad pokušao ne možemo postaviti lozinku u lokalnom okruženju AD, pojavila se pogreška. To se može dogoditi iz nekoliko razloga:</p>
              <ul>
                <li class="unordered">
Korisničke lozinke ne zadovoljava dob, povijest, složenosti ili filtrirati preduvjeti za domenu. Pokušajte potpuno novu lozinku da biste riješili taj problem.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Račun servisa MA nemate odgovarajuće dozvole da biste postavili novu lozinku na korisnički račun.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Na korisnički račun se zaštićene grupu, primjerice domene ili enterprise administratori koji ne dopušta operacije postavljanje lozinke.<br\><br\></li>
              </ul>
              <p>Potražite u članku <a href="#troubleshoot-password-writeback">Otklanjanje poteškoća s Upisima lozinku</a> da biste saznali više o drugim situtions mogu prouzročiti ta se pogreška.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj pojavljuje ako onemogućite Upisima lozinku s Azure AD Connect i ukazuje na to da ne možemo pokrenuti offboarding vašoj tvrtki ili ustanovi web-servisa Upisima lozinku.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj upućuje na to je offboarding postupak uspio i da mogućnost Upisima lozinka uspješno onemogućena.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava proces offboarding nije uspjelo. To može biti zbog pogreške dozvole na oblaku ili na lokalnim poslužiteljima administratorski račun navedeno vrijeme konfiguraciju ili jer je pokušavate koristiti pridruženim oblaka globalni administrator kada onemogućivanje Upisima lozinku. Da biste riješili taj problem, provjerite svoje administratorske dozvole i ne koristite neku računa vanjskog tijekom konfiguriranja mogućnost Upisima lozinku.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava servis Upisima lozinka uspješno pokrene, a spreman za prihvaćanje zahtjeva za upravljanje lozinke iz oblaka.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava servis Upisima lozinku prestao i da sve zahtjeve za upravljanje lozinke iz oblaka neće biti uspješno.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava da ćemo uspješno dohvatiti ovlaštenja za globalni administrator navedeni tijekom postavljanja Azure AD Connect da biste započeli postupak offboarding ili za uhodavanje.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava da ćemo uspješno stvorili ključa za šifriranje lozinku koja će se koristiti za šifriranje lozinki iz oblaka slati lokalnog okruženja.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava do nepoznate pogreške tijekom operacije upravljanja lozinkom. Potražite tekst iznimke u događaj više pojedinosti. Ako imate problema, pokušajte onemogućivanje i ponovno Omogućivanje Upisima lozinku. Ako to ne pomogne, uključiti kopiju evidenciji uz navedeni id praćenje insider da biste inženjer za podršku.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>ServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava došlo je do pogreške prilikom povezivanja oblaka lozinku ponovno postavljanje servisa i obično događa kada je servis lokalnog ne može povezati s web-servisa za ponovno postavljanje lozinke. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava pojavila se pogreška prilikom povezivanja s bus instanca servisa za vaš klijent. To se može dogoditi jer onemogućuju izlazne veze lokalnog okruženja. Provjerite vatrozida da biste bili sigurni dopustite povezivanja putem TCP 443 i <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>i pokušajte ponovno. Ako i dalje imate problema, pokušajte onemogućivanje i ponovno Omogućivanje Upisima lozinku.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava da unos proslijediti naš API servisa web koji nisu valjani. Ponovite postupak.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava da došlo je do pogreške dešifriranja lozinku koju stigli iz oblaka. To može biti zbog dešifriranje ključa nepodudaranje između servisa u oblaku i lokalnog okruženja. Da biste riješili taj problem, onemogućivanje i ponovno Omogućivanje Upisima lozinku u lokalnog okruženja.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tijekom za uhodavanje, ne možemo spremanje informacija specifičnih za klijenta u konfiguracijskoj datoteci u lokalnog okruženja. Ovaj događaj označava pojavila se pogreška prilikom spremanja ove datoteke ili da kada postoji počeo servis je pogreška čitanja datoteke. Da biste riješili taj problem, pokušajte onemogućivanje i ponovno omogućivanjem Upisima lozinku da biste nametnuli je ponovno pisanje datoteke konfiguracije. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>UKINUTA – ovaj događaj ne postoji u Azure AD Connect, samo vrlo ranijeg sastavlja od DirSync koje podržava upisima.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tijekom za uhodavanje, mi šaljemo podatke iz oblaka lozinku lokalnog Vrati servisa. U datoteku u memoriji prije koja se šalje na servis za sinkronizaciju sigurno pohrane tih podataka na disku, zatim zapisivati tih podataka. Ovaj događaj upućuje na problem s pisanjem programa ili ažuriranje tih podataka u memoriji. Da biste riješili taj problem, pokušajte onemogućivanje i ponovno omogućivanjem Upisima lozinku da biste nametnuli je ponovno pisanje ovaj konfiguracije.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava smo dobili koji nisu valjani odgovor od web-servisa za ponovno postavljanje lozinke. Da biste riješili taj problem, pokušajte onemogućivanje i ponovno omogućivanjem Upisima lozinku.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava da ćemo nije moguće dohvatiti odobrenje tokena za navedenog tijekom postavljanja Azure AD Connect računa globalnog administratora. Ta se pogreška može uzrokovati neispravan korisničko ime ili lozinku za račun globalnog administratora ili vanjskog jer navedena računa globalnog administratora. Da biste riješili taj problem, ponovno pokrenite konfiguraciju ispravno korisničko ime i lozinku i provjerite je li administrator u upravljanih (samo oblak ili želite sinkronizaciju lozinke) računa.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava pojavila se pogreška pri stvaranju lozinku ključa za šifriranje ili dešifriranje lozinku koja dolazi iz servisa u oblaku. Ta se pogreška vjerojatno označava problem s vaše okruženje. Pogledajte detalje o evidenciji dodatne informacije i riješili taj problem. Također možete pokušati onemogućivanje i ponovno Omogućivanje servisa Upisima lozinku da biste riješili taj problem.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava da lokalnog servisa ne može ispravno komunicirati s web-usluge ponovno postavljanje lozinke da biste započeli postupak za uhodavanje. To može biti zbog pravila vatrozida ili problem početak token za provjeru autentičnosti za vaš klijent. Da biste riješili taj problem, provjerite se ne blokira izlazne veze putem TCP 443 i TCP 9350 9354 ili <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>, a koji koristite za onboard administratorskog računa AAD nije naveden kao pridruženi. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>UKINUTA – ovaj događaj ne postoji u Azure AD Connect, samo vrlo ranijeg sastavlja od DirSync koje podržava upisima.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava da lokalnog servisa ne može ispravno komunicirati s web-usluge ponovno postavljanje lozinke da biste započeli postupak offboarding. To može biti zbog pravila vatrozida ili problem početak oznaku ovlaštenja za vaš klijent. Da biste riješili taj problem, provjerite je li se ne blokira izlazne veze putem 443 ili <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>, i da AAD administratorskog računa koje koristite za offboard nije naveden kao pridruženi. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava smo morali ponovno povezati bus instanca servisa za vaš klijent. U običnom uvjetima to treba neće biti složen, ali ako vidite taj događaj više puta, razmislite o provjere mrežne veze tržišta servisa za osobito ako je visokom latencijom ili vezu niske propusnosti.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Da biste pratili stanje servisa Upisima lozinku, mi šaljemo intervala sustava podatke u našem lozinku ponovno postavljanje web-servisa svakih 5 minuta. Ovaj događaj označava da došlo je do pogreške prilikom slanja te informacije o stanju povratak na web-servisa oblaka. Ove informacije o stanju sustava obuhvaćaju podataka programa OII ili PII i je isključivo intervala sustava i Statistika osnovni servisa tako da se dajemo informacije o stanju servisa u oblaku.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava došlo je do nepoznate pogreške vratio AD, u evidenciji Azure AD Connect poslužitelja za događaje iz izvora ADSync dodatne informacije o pogrešci.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava da korisnika za kojeg želite ponovno postavljanje ili promjena lozinke nije pronađen u lokalnog imenika. To se može dogoditi kada korisnik je izbrisan lokalno, ali ne i u oblaku, ili ako je došlo do problema sa sinkronizacijom. Provjerite svoje zapisnika sinkronizaciju, kao i zadnji nekoliko sinkronizaciju pokrenuti detalje za dodatne informacije.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Kada ponovno postavljanje lozinke ili zahtjev za promjenom potječe iz oblaka, koristimo sidro oblaka navedenim tijekom postupka za postavljanje Azure AD Connect da biste saznali kako se ponovno povezali taj zahtjev za korisnika u lokalnog okruženja. Ovaj događaj označava smo pronašli dva korisnika u imeniku lokalne s isti atribut sidro oblaka. Provjerite svoje zapisnika sinkronizaciju, kao i zadnji nekoliko sinkronizaciju pokrenuti detalje za dodatne informacije.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava račun servisa Management Agent imate odgovarajuće dozvole na računu u pitanju da biste postavili novu lozinku. Provjerite imaju li račun MA u skup korisnika stabala ponovno postavljanje i promjena dozvola za lozinku na svim objektima u u skup stabala.  Dodatne informacije o tome kako učinite ovo, potražite u članku <a href="active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions">4 korak: postavljanje odgovarajuće dozvole za Active Directory</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava da smo pokušali ponovno postavljanje ili promjena lozinke za račun koji je onemogućen lokalno. Omogućavanje računa i pokušajte ponovno.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Događaj označava da smo pokušali ponovno postavljanje ili promjena lozinke za račun koji je zaključan lokalno. Lockouts se može dogoditi kada korisnik ima pokušao promjena ili ponovno postavljanje lozinke operacija Premašili broj pokušaja određenog razdoblja. Otključavanje račun i pokušajte ponovno.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava je li korisnik navedena ispravna trenutnu lozinku promjenama lozinke za izvođenje operacije. Navedite točne trenutnu lozinku i pokušajte ponovno.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj se događaj kada servis Upisima lozinku pokuša postavite lozinku na lokalnom direktoriju koji ne zadovoljava starost lozinke, povijest, složenosti ili filtriranja zahtjevima domene.</p>
              <ul>
                <li class="unordered">
Ako imate starost minimalne lozinke i nedavno promijenili lozinku u tom prozoru vrijeme, nećete moći promijeniti lozinku ponovno sve dok ne postignete navedeni dob u vašoj domeni. Za testiranje, minimalno mora biti postavljeno na 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ako imate omogućen zahtjeve za lozinku povijest, a zatim odaberite lozinku koju ne koristi u zadnji N puta pri čemu je N povijest postavku lozinke. Ako ste odabrali lozinku kojom u zadnji N puta, zatim vidjet ćete pogreške u tom slučaju. Za testiranje, povijest mora biti postavljeno na 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ako imate složenosti lozinki, sve ih provest će se kada se korisnik pokuša promjena ili ponovno postavljanje lozinke.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Ako imate lozinku filtri omogućeno i korisnik odabere lozinku koji zadovoljavaju kriterij filtriranja, zatim ponovno postavljanje ili promjena operacija neće uspjeti.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Ovaj događaj označava pojavio se problem pisanje lozinke natrag u lokalni direktorij zbog problem s konfiguracijom s servisa Active Directory. Potvrdite strojno Azure AD Connect zapisnik događaja aplikacije za poruke s servisa ADSync dodatne informacije o koje će se pogreška. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>UKINUTA – ovaj događaj ne postoji u Azure AD Connect, samo vrlo ranijeg sastavlja od DirSync koje podržava upisima.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>UKINUTA – ovaj događaj ne postoji u Azure AD Connect, samo vrlo ranijeg sastavlja od DirSync koje podržava upisima.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>UKINUTA – ovaj događaj ne postoji u Azure AD Connect, samo vrlo ranijeg sastavlja od DirSync koje podržava upisima.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback-connectivity"></a>Otklanjanje poteškoća s povezivanjem Upisima lozinke

Ako nailazite prekide servisa s komponentom Upisima lozinku Azure AD Connect, Evo nekih brzi koraci koje možete poduzeti da biste riješili taj problem:

 - [Ponovno pokretanje Azure AD povezivanje servisa za sinkronizaciju](#restart-the-azure-AD-Connect-sync-service)
 - [Onemogućivanje i ponovno Omogućivanje značajku Upisima lozinke](#disable-and-re-enable-the-password-writeback-feature)
 - [Instalirajte najnovije izdanje Azure AD Connect](#install-the-latest-azure-ad-connect-release)
 - [Otklanjanje poteškoća s Upisima lozinke](#troubleshoot-password-writeback)

Općenito govoreći, preporučujemo da u redoslijedu iznad da bi se vratiti na servisu najčešće brz način izvršiti ove korake.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Ponovno pokretanje Azure AD povezivanje servisa za sinkronizaciju
Ponovno pokrenuti Azure AD povezivanje sinkronizaciju servis može pomoći u rješavanju problema s povezivanjem ili druge tranzitne problemi sa servisom.

 1. Kao administrator, kliknite **Započni** na poslužitelju koji se izvodi **Azure AD Connect**.
 2. U okvir za pretraživanje upišite **"services.msc"** , a zatim pritisnite **Enter**.
 3. Potražite stavku **Microsoft Azure AD Connect** .
 4. Desnom tipkom miša kliknite stavku servisa, kliknite **ponovno pokrenite**i pričekajte da biste dovršili postupak.

    ![][002]

Sljedeće će ponovno uspostaviti vezu sa servisom cloud i rješavanje sve smetnje možda imate.  Ako ponovno pokrenuti sinkronizaciju servisa ne riješite problem, preporučujemo da pokušate onemogućivanje i ponovno Omogućivanje značajku Upisima lozinke kao sljedeći korak.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Onemogućivanje i ponovno Omogućivanje značajku Upisima lozinke
Onemogućivanje i ponovno omogućite značajku Upisima lozinku može pomoći za rješavanje problema s povezivanjem.

 1. Kao administrator, otvorite **Čarobnjak za konfiguraciju Azure AD Connect**.
 2. U dijaloškom okviru **za povezivanje s Azure AD** unesite **vjerodajnice za Azure AD globalni administrator**
 3. U dijaloškom okviru **za povezivanje s AD DS** unesite **servisa Active Directory Domain Services administratorske vjerodajnice**.
 4. U dijaloškom okviru **jedinstveno označavanje korisnika** kliknite gumb **Dalje** .
 5. U dijaloškom okviru **Dodatne značajke** poništite potvrdni okvir za **unos natrag za lozinku** .

    ![][003]

 6. Kliknite **Dalje** kroz preostale stranice dijaloški okvir bez promjene sve dok ne dođete do **spremno za konfiguraciju** stranice.
 7. Provjerite je li stranici Konfiguracija prikazuje **mogućnost pisanje natrag lozinke kao onemogućena** , a zatim kliknite zeleni gumb **Konfiguracija** da biste potvrdili promjene.
 8. U dijaloškom okviru **Dovršeno** poništite mogućnost **Sinkroniziraj sada** , a zatim kliknite **Završi** da biste zatvorili čarobnjak.
 9. **Čarobnjak za konfiguraciju Azure AD Connect**ponovno je otvorite.
 10.    **Ponovite korake 2-8**, osim provjerite je li **potvrdite mogućnost natrag za unos lozinke** na **Dodatne značajke** zaslona da biste ponovno omogućiti servis.

    ![][004]

Sljedeće će ponovno uspostaviti vezu s oglednim servis u oblaku i rješavanje sve smetnje možda imate.

Preporučujemo da ako onemogućivanje i ponovno omogućite značajku Upisima lozinke ne riješite problem, pokušajte ponovno instalirati Azure AD Connect kao sljedeći korak.

### <a name="install-the-latest-azure-ad-connect-release"></a>Instalirajte najnovije izdanje Azure AD Connect
Ponovno instalirati paket Azure AD Connect će riješiti konfiguracijskih problema koji mogu utjecati mogućnost ili povezivanje s naših usluga oblaka ili za upravljanje lozinkama u lokalnom okruženju AD.
Preporučujemo, dovršite ovaj korak tek nakon prva dva koraka navedenih u postupku.

 1. Preuzmite najnoviju verziju programa Azure AD Connect [ovdje](active-directory-aadconnect.md#install-azure-ad-connect).
 2. Budući da ste već instalirali Azure AD Connect, samo morat ćete izvršiti nadogradnju na mjestu na ažuriranje instalacije sustava Azure AD Connect na najnoviju verziju.
 3. Izvršavanje preuzeti paket, a zatim slijedite na zaslonu upute za ažuriranje računala Azure AD Connect.  Dodatni koraci ručno su potrebni osim ako ste prilagodili izlaz okvir Sinkroniziraj pravila, u tom slučaju, trebali biste **ih sigurnosno kopiranje prije nastavka nadogradnje i ručno ponovno uvesti ih nakon što završite**.

Sljedeće će ponovno uspostaviti vezu s oglednim servis u oblaku i rješavanje sve smetnje možda imate.

Ako instaliranje najnovije verzije sustava server Azure AD Connect ne riješite problem, preporučujemo vam da započnete onemogućivanje i ponovno omogućivanjem Upisima lozinke kao konačan korak nakon instalacije najnovije sinkronizaciju uspio.

Ako koji ne riješite problem, pa preporučujemo snimljenih [Otklanjanje poteškoća s Upisima lozinku](#troubleshoot-password-writeback) i [Azure AD lozinku najčešća pitanja o upravljanju](active-directory-passwords-faq.md) da biste vidjeli ako možda problem li se spominju.


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Dokumentacija za ponovno postavljanje veze na lozinke
U nastavku su veze na sve stranice dokumentaciju za ponovno postavljanje lozinke za Azure AD:

* **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).
* [**Kako to funkcionira**](active-directory-passwords-how-it-works.md) – Saznajte više o šest različitih komponenata servisa i što svaki ne
* [**Početak rada**](active-directory-passwords-getting-started.md) – upute kako bi korisnici mogli vratiti i mijenjati oblaka ili na lokalnim poslužiteljima lozinke
* [**Prilagodba**](active-directory-passwords-customize.md) – Saznajte kako prilagoditi izgled i dojam i ponašanje servisa potrebama vaše tvrtke ili ustanove
* [**Najbolje prakse**](active-directory-passwords-best-practices.md) – Saznajte kako brzo i učinkovito upravljanje lozinkama u tvrtki ili ustanovi
* [**Početak uvida**](active-directory-passwords-get-insights.md) – dodatne informacije o oglednim integrirane mogućnosti izvješćivanja
* [**Najčešća pitanja vezana uz**](active-directory-passwords-faq.md) – Pronađite odgovore na najčešća pitanja
* [**Saznajte više**](active-directory-passwords-learn-more.md) – otvorite obuhvaća više razina u tehničke pojedinosti funkcioniranje servisa



[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"
