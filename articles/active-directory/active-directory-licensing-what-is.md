<properties
    pageTitle="Što je Microsoft Azure Active Directory licenciranje? | Microsoft Azure"
    description="Opis licenci za Microsoft Azure Active Directory, kako to funkcionira, upute za početak rada i najbolje prakse, uključujući Office 365, Microsoft Intune potražite i Azure Active Directory Premium i osnovni izdanja"
    services="active-directory"
      keywords="Azure AD licenciranje"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>

# <a name="what-is-microsoft-azure-active-directory-licensing"></a>Što je Microsoft Azure Active Directory licenciranje?

##<a name="description"></a>Opis
Azure Active Directory (Azure AD) je identitet tvrtke Microsoft kao rješenje servisa (IDaaS) i platforme. Azure AD je ponuđen broj verzije funkcionirati i tehničke rasponu od Azure AD slobodno, koji je dostupan uz bilo koji servis za Microsoft, kao što je Office 365, Dynamics, Microsoft Intune i Azure (Azure AD ne generira bilo kakvih troškova potrošnje u tom načinu), Azure AD platili verzije kao što je Enterprise mobilnost paket (EMS), Azure AD Premium i Basic, kao i Azure višestruke provjere autentičnosti (MFA). Kao što su mnoge Microsoft online services najčešće Azure AD plaćenu verzije se isporučuju do prava po korisniku, kao što su u Office 365, Microsoft Intune i Azure AD. U tim slučajevima kupnju servisa predstavlja s jednog ili više pretplate, a svaki pretplata obuhvaća prije kupnje broj licenci u klijentu. Prava na po korisniku postići kroz dodjele licenci, stvaranje veze između korisnika i proizvod, omogućivanje komponente za servis za korisnika i koristi neku od pretplatnog licenci.

[Pokušajte Azure AD premium sada.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [AZURE.NOTE] Portal za administraciju servisa Azure AD je dio Azure klasični portal. Tijekom korištenja Azure AD ne zahtijeva sve Azure Nabava, pristup ovaj portal potreban je aktivna pretplata na Azure ili [Azure probnu pretplatu](https://azure.microsoft.com/pricing/free-trial/).

Pregled Općenite mogućnosti servisa Azure AD potražite u članku [što je Azure AD](active-directory-whatis.md).
[Dodatne informacije o razinama servisa Azure AD](https://azure.microsoft.com/support/legal/sla/)

> [AZURE.NOTE]  Pretplate Azure Idi plaćanje kao što su različiti: dok predstavljala u direktoriju, ove pretplate omogućiti stvaranje Azure resursa i mapirajte ih način plaćanja. U ovom slučaju postoje nema licencu broji povezan s pretplatom. Pridruživanje korisnika s pretplatom, korisnici mogu pristupiti Upravljanje resursima za pretplatu, možete učiniti tako da dodjelu dozvole da biste upravljali Azure resursi mapirani s pretplatom.


##<a name="how-does-azure-ad-licensing-work"></a>Kako Azure AD licenciranje funkcionira?

Utemeljen na licence (prava sustavom) Azure AD services rad tako da aktiviranje pretplate na klijentu imeničkom servisu Azure AD. Kada je aktivna pretplata mogućnosti usluge može biti upravlja imeničkom servisu administratori i koristiti licencirani korisnici.

Kada kupite ili aktivirati paket mobilnost za tvrtke, Azure AD Premium ili Azure AD osnovni, direktorija ažurira se s pretplatom, uključujući njene razdoblje valjanosti i pretplatnog licence. Podatke o pretplati, uključujući status, sljedeći događaj životni ciklus i broj licenci koje su dodijeljene ili dostupno je dostupan putem Azure klasični portal na kartici licence za određeni direktorij. To je najbolje mjesto za upravljanje sustava dodjele licenci.

Pretplate sastoji se od jednog ili više usluga tarife, svaki mapiranja uključene funkcionalnu razinu vrsta servisa; na primjer, Azure AD Azure MFA, Microsoft Intune, Exchange Online ili SharePoint Online. Upravljanje licencama za Azure AD zahtijevaju usluga plana razinu upravljanje. To se razlikuje od sustava Office 365 koja ovisi o tom načinu Napredna konfiguracija za upravljanje pristupom obuhvaćenih servisa. Azure AD ovisi u Konfiguracija servisa za omogućivanje značajki i upravljanje pojedinačnih dozvola.

Općenito govoreći, podatke o pretplati Azure AD upravlja se putem Azure klasični portalu, na kartici licence za određeni direktorij. Azure AD pretplate, osim Azure AD Premium, neće se prikazivati na portalu sustava Office.

> [AZURE.IMPORTANT] Azure AD Premium i osnovni, kao i pretplate na paket Enterprise mobilnost su ograničeni na njihove dodijeljenu direktorija/klijent. Pretplate nije moguće podijeliti između direktorija ili za entitle korisnika u druge mape. Premještanje pretplatu iz direktorija moguće je, ali prijavljuju na zahtjev za podršku možete ili otkazivanje i ponovno kupnju slučaju Izravni Nabava.

> Kada kupite Azure AD ili Enterprise mobilnost paket putem količinskog licenciranja aktivaciju pretplate dogodit će se automatski kada ugovor obuhvaća i druge servise Microsoft Online, primjerice sustava Office 365.

Plaćena Azure AD značajke obuhvaćaju breadth direktorija. Primjeri:
- Grupne dodjele aplikacijama, koji je omogućen u određenoj aplikaciji upravljate.
- Napredno, a zatim mogućnosti upravljanja samostalno grupe su dostupne u odjeljku konfiguracije directory ili iz određene grupe.
- Izvješća o sigurnosti Premium su na kartici izvješćivanja
- Otkrivanje oblaka aplikacija prikazuje na portalu za Azure u odjeljku identitet.

###<a name="assigning-licenses"></a>Dodjela licence
Nabavljanje pretplate bit će sve morate konfigurirati plaćenu mogućnosti, pomoću svoje Azure AD plaćenu značajke potreban je raspodjela licence desnom pojedincima. Licence mora biti dodijeljena Općenito, bilo koji korisnik tko treba imati pristup ili tko upravlja se putem Azure AD plaćenu značajke. Dodjele licence je mapiranja između korisnika i kupljeni servisa, kao što su Azure AD Premium, Basic ili paket mobilnost za tvrtke.

Upravljanje koje korisnicima u direktoriju mora imati licencu je jednostavno. To je moguće napraviti dodjelom grupe da biste stvorili dodjele pravila putem portala za administraciju Azure AD ili izravno na desni pojedinaca kroz portal, PowerShell ili API-ji dodjeli licenci. Prilikom dodjeljivanja licence u grupu, sve članove grupe dodjeljuju licence. Ako korisnici su dodani ili uklonjeni iz grupe kao što su oni će se dodijeliti ili ukloniti potrebne licence. E-pošta mogu koristiti sve dostupne upravljanje grupe i dosljedan grupne dodjele aplikacijama. Koristite ovaj pristup, možete postaviti pravila tako da se automatski dodjeljuju svi korisnici u direktoriju, provjerite imaju li svi koji imaju odgovarajuće zanimanje licence ili čak i delegiranje odluku da želite druge upravitelja u tvrtki ili ustanovi.

S Dodjela licence grupne bilo koji korisnik nema mjesto korištenja će naslijediti mjesto imenika tijekom dodjele. To mjesto administrator može se promijeniti u bilo kojem trenutku. U slučajevima gdje automatiziranog dodjele nije uspjela zbog pogreške korisničke informacije u odjeljku vrste licence odražavaju to stanje.

##<a name="getting-started-with-azure-ad-licensing"></a>Početak rada s Azure AD licenciranje

Uvod u Azure AD je jednostavno; uvijek možete stvoriti direktorija kao dio prijave za besplatan Azure probno razdoblje. [Dodatne informacije o prijavi kao tvrtkom ili ustanovom](sign-up-organization.md). Sljedeće omogućuju direktorija najbolje poravnali s druge Microsoftove servise možda koristi ili planiranje zauzeti i ciljeve nabavljanja servis.

Evo nekoliko najbolje prakse:
- Ako koristite Microsoftove servise za tvrtke ili ustanove, već imate u imeniku Azure AD. U ovom slučaju, nastavite koristiti isti direktorija za druge servise tako da se upravljanje identitetom core, uključujući dodjelu resursa i hibridnog SSO, možete se koristi se u svim servisima. Korisnici će imati sučelje za prijavu jedan i će im bogatiji mogućnosti svim servisima. Kao rezultat, ako odlučite da biste kupili programa Azure AD plaćenu služba za vaše snage, preporučujemo da koristite isti direktorija da biste to učinili.
- Ako namjeravate koristiti Azure AD za drugi skup korisnika (partnera, klijentima i tako dalje) ili ako želite procijeniti servisa Azure AD i želite učiniti odvajanja servisa proizvodnje ili ako vas zanimaju testnog okruženja za servisa za postavljanje, preporučujemo da prvo stvorite novi direktorij putem klasične portala za Azure Azure. [Dodatne informacije o stvaranju novog Azure AD direktorija na portalu za Azure klasični](active-directory-licensing-directory-independence.md). Novi direktorij stvorit će se pomoću računa kao vanjskog korisnika s dozvolama globalnog administratora. Kada se prijaviti na portal Azure klasični s ovim računom, moći ćete vidjeti taj imenik i pristup svih administrativnih zadataka za direktorija. Preporučujemo da ste stvorili lokalni račun pomoću odgovarajuće ovlasti za upravljanje druge Microsoftove servise (one nije dostupno putem portala za Azure klasični). [Dodatne informacije o stvaranju korisničkim računima u Azure AD](active-directory-create-users.md).

> [AZURE.NOTE] Azure AD podržava "vanjskim korisnicima," koji su korisnički računi u instance komponente Azure AD koje su stvorene pomoću na račun za Microsoft (MSA) ili identitet Azure AD iz drugog imenika. Dok se ne možemo zauzeti proširivanje tu mogućnost u sve Microsoftove servise za tvrtke ili ustanove odmah te računi nisu podržane u dio sučelja na servise; na primjer, portal za administraciju sustava Office 365 trenutno ne podržava tim korisnicima. Zbog toga vanjskim korisnicima s računima za Microsoft će moći pristupiti portal za administraciju sustava Office 365 uopće dok vanjskih korisnika iz druge Azure AD direktorija će je zanemariti. U potonjem slučaju, samo lokalni račun korisnika, Azure AD ili direktorija za Office 365 gdje korisnika koji su izvorno stvoren, će moći pristupiti kroz ove sučelja.

Kao što je naznačeno, Azure AD ima različite plaćenu verzije. Ove verzije imaju neke manji razlike u njihove dostupnosti za kupnju:


| Proizvoda   | EA-VL     | Otvaranje  |   CSP |   Prava na korištenje mreže Microsoftovih Partnera  |   Izravni za kupnju | Probna verzija |
|---|---|---|---|---|---|---|
| Paket mobilnost za tvrtke |   X | X | X | X |  |      X |
| Azure AD Premium  | X | X | X |   | X | X |
| Azure AD Basic    | X | X | X | X |  <br /> |  <br />  |

###<a name="select-one-or-more-license-trials"></a>Odaberite jednu ili više licenci probne verzije
 U svakom slučaju, možete je aktivirati Azure AD Premium ili paket Enterprise mobilnost probnu pretplatu na tako da odaberete određenu probno razdoblje koje želite na kartici licenci u direktoriju. Neki probnu verziju sadrži pretplatu od 30 dana 100 licenci.

![Tarife probnom licencom za Azure Active Directory](./media/active-directory-licensing-what-is/trial_plans.png)

![Tarife probnom licencom paketa mobilnost za tvrtke](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Aktivni probnom licencom tarife](./media/active-directory-licensing-what-is/active_license_trials.png)

###<a name="assign-licenses"></a>Dodjela licenci
Kada je aktivna pretplata, trebali biste dodijelite si licencu i osvježite preglednik da bi vam se prikazuje sve značajke. Sljedeći je korak da biste dodijelili licence za korisnika koje ćete morati pristup ili obuhvatiti plaćenu Azure AD značajke. Kao što smo spomenutih u "Dodjela licenci", da biste to učinili, najbolje je da biste označili grupu koji predstavlja željeni ciljne skupine i Dodjela licence; na taj način korisnici koji su dodani ili uklonjeni iz grupe iz tijekom njegova životnog ciklusa će se dodijeliti ili uklonili licencu.

Da biste licencu dodijelili grupe ili pojedinačnih korisnika, odaberite plan licence koje želite dodijeliti, a zatim kliknite **Dodijeli** na naredbenoj traci.

![Aktivni probnom licencom tarife](./media/active-directory-licensing-what-is/assign_licenses.png)

Jednom u dijaloški okvir Dodjela za odabrani plan možete odabrati da korisnici i njihovo **Dodjeljivanje** stupac na desnoj strani. Možete pomicali kroz popis korisnika ili traženje određenim osobama pomoću u potrazi staklu u gornjem desnom kutu rešetki korisnika. Da biste dodijelili grupe, odaberite "Grupe" na izborniku **Prikaz** , a zatim kliknite gumb Provjeri na desnoj strani da biste osvježili dodjele koje se prikazuju.

![Dodjela licence za grupe](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

Sada možete pretraživati ili stranicu kroz grupe i njihovo dodavanje u stupcu **dodijeliti** na isti način. To možete koristiti da biste dodijelili kombinaciju korisnicima i grupama u jednoj operaciji. Da biste dovršili postupak dodjele, kliknite gumb Provjeri u donjem desnom kutu stranice.

![Poruka o tijeku dodjela licenci](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

Kada je dodijeljen grupi, članovima nasljeđuju licence sljedećih 30 minuta, ali obično nekoliko minuta 1-2.

Pogreške prilikom dodjele se može dogoditi prilikom dodjele licence za Azure AD, ali su relativno rijetko. Potencijalne pogreške dodjele ograničeni su na:
- Sukob dodjele – kada korisnik prethodno dodijeljena licenca koja nije kompatibilan s trenutnom licencu. U ovom slučaju, dodijelite novu licencu potrebno uklanjanje na prethodni slajd.
- Prekoračena raspoloživih licenci – kada je broj korisnika u dodijeljene grupama premašuju raspoloživih licenci status Dodjela korisnika odražavaju pogreške za dodjelu zbog nedostaju licence.

###<a name="view-assigned-licenses"></a>Pregled dodijeljenih licenci

Sažeti prikaz dodijeljenih licenci uključujući događaja životni ciklus pretplate dostupna, dodijeljene i dalje prikazuju se na kartici **licence** .

![Prikaz broja dodijeljene licence](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

Detaljni popis dodijeljenih korisnicima i grupama, uključujući dodjelu status i put (izravno ili naslijeđeno iz jedne ili više grupa) dostupna navigacijom u plan licence.

![Prikaz detalja licenci dodjeljuje za tarifu licence](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Uklanjanje licence je samo dovoljno im dodijelite. Ako je korisnik izravno dodijeljen ili dodijeljenog grupi, možete ukloniti licence tako da odaberete vrstu licence, odaberite **Ukloni**, dodate korisnika ili grupu na popis Ukloni i potvrđivanje akciju. Umjesto toga možete otvoriti vrstu licence, odaberite određenog korisnika ili grupu i na naredbenoj traci dodirnite **Ukloni** . Da biste prekinuli nasljeđivanje korisničke licence iz grupe, jednostavno uklonite korisnika iz grupe.

###<a name="extending-trials"></a>Proširivanje probne verzije

Dostupne su probne proširenja za klijente kao samoposlužnog putem portala sustava Office 365. Administrator za klijenta možete se kretati na [portal sustava Office](https://portal.office.com/#Billing) (ovisi o dozvola za portal sustava Office), a zatim odaberite Azure AD Premium probnu verziju. Kliknite vezu **Produlji probno razdoblje** i slijedite upute. Morat ćete unijeti kreditnom karticom, ali neće biti naplaćen.

![Proširivanje probne verzije za licence na portalu sustava Office](./media/active-directory-licensing-what-is/extend_license_trial.png)

Korisnici možete zatražiti nastavkom probne slanjem zahtjeva za podršku. Administrator za klijenta možete doći do sustava Office 365 portala [podršku](http://aka.ms/extendAADtrial) (ovisi o dozvolama za stranicu podrška za Office). Na ovoj stranici odaberite "Pretplate i pokušaji" u odjeljku značajke i "Probnu verziju pitanja" u odjeljku simptoma. Na kraju unesite podatke na okolnostima

![Proširivanje probnu verziju licence pomoću zahtjev za podršku](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Daljnji koraci

Sada možda ste spremni za konfiguriranje i korištenje nekih značajki Azure AD Premium.

- [Samostalno ponovno postavljanje lozinke](active-directory-manage-passwords.md)
- [Upravljanje samostalno grupe](active-directory-accessmanagement-self-service-group-management.md)
- [Azure AD Connect heath](active-directory-aadconnect-health.md)
- [E-pošta s aplikacijama](active-directory-manage-groups.md)
- [Azure višestruka provjera autentičnosti](../multi-factor-authentication/multi-factor-authentication.md)
- [Izravni kupnju licenci Azure AD Premium](http://aka.ms/buyaadp)
