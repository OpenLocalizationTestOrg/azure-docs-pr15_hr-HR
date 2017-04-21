# <a name="pull-request-etiquette-and-best-practices-for-microsoft-contributors-to-azure-documentation"></a>Povlačenje bontonu zahtjev i najbolje prakse za Microsoft suradnika u dokumentaciji Azure

Da biste objavili promjene u dokumentaciji, slanje zahtjeva za povlačite iz vaše o ogranku. Svaki zahtjev za istaknuti mora biti pregledava prije no što koji se spajaju. Ovaj članak da biste razumjeli kako treba raditi istaknuti zahtjev pregledavatelji i kako stvarati istaknuti zahtjeva za koje su lakše i brže da biste pregledali tako da povlačite red čekanja zahtjeva radi bolje svima.

## <a name="working-with-pull-request-reviewers"></a>Rad s istaknuti zahtjev pregledavatelja

Evo osnove što trebate znati o radu s istaknuti zahtjev pregledavatelji. 

- <b>Razumijevanje ulogu pregledavatelja istaknuti zahtjev. Pregledavatelj:</b>
  - Osigurava osnovni kvalitete sadržaja
  - Sprječava nedostatke iz spremišta
  - Omogućuje povratne informacije prije spajanja

  Pregledavatelji zahtjev istaknuti su u ulozi sadržaja upravljanja. Primarni cilj je ne jednostavno spajanje ono je poslati kao brzo moguće. Očekivati povratne informacije koje ćete morati ažurirati, posebice za nove i intenzivnog promijenjenih članke.

- <b>Planiranje uz vaše pregledavatelja istaknuti zahtjev:</b>
  - Istaknuti visokog prioriteta zahtjeva za
  - Istaknuti zahtjeve za timed/datirana izdanja
  - Zahtjevi za istaknuti koje se mijenjaju ili dodaju veći broj datoteka

- <b>SLA za istaknuti zahtjev za pregled</b>

  U spremištu privatne svaki put kada vaš zahtjev istaknuti unosi red čekanja zahtjeva istaknuti s natpisom jeste li spremni za spajanje tima pokušava pregledajte zahtjev za istaknuti unutar tvrtke 12 sati (M-F, 8 Prijepodne do 5 sati) i povratne informacije ili spajanje ako je potrebno bez povratne informacije. Ovaj SLA odnosi na čin pregledavanje Cijena, ne i spajanje. PRs spajaju se kada se oni ispunjavaju [kriterije za spajanje](contributor-guide-pr-criteria.md). 

## <a name="make-the-pull-request-queue-work-better-for-everyone"></a>Provjerite istaknuti red čekanja zahtjeva radi bolje svima

Postoje dvije osnovne realities u redu čekanja Cijena:

- Istaknuti zahtjeva za koje su na small u opsegu i koji sadrže slične promjene traje manje vremena za pregled. 
- Istaknuti zahtjeva za koje su na velike u opsegu ili koje sadrže različite, koriste različite vrste promjena trajati dulje da biste pregledali.

Možete pridonijeti provjerite istaknuti red čekanja zahtjeva radi bolje slijedeći ove najbolje prakse:

- Zasebna manji ažuriranja na postojeće članke iz novih članaka ili glavne pisanja. Rad na tih promjena u zasebnu grana rad. 

- Kada izbrišete članaka ili slike, nemojte kombinirati u brisanja s novi sadržaj dodatke i ažuriranja. Držač promjene novi sadržaj u zasebni radni grani.

- Izdanja ili restrukturiranje sadržaja, Planiranje s vašeg Cijena pregledavatelja. Možda ćete upisom pomoć da biste stvorili izdanje granu ili koordiniranje puta pisama uz objavljivanje vremena da bi se sadržaj objavljuje u pravo vrijeme.

- Ako pokušavate koordinaciji ažuriranjima u repo ACOM (ie, promjene u lijevom oknu za navigaciju odredišne stranice, preusmjeravanja ili karte učenje) s promjenama koje unosite u spremištu azure, sadržaj i cijena, koji funkcioniraju na vrijeme potrebno usklađivanju vaše pregledavatelja Cijena. U suprotnom rizikom imate mnogo prekinutih veza.

## <a name="criteria-for-expedited-pull-requests"></a>Kriteriji za poslani istaknuti zahtjeva

- Obratite se azdocprs da biste ubrzali PRs samo kada uistinu potrebne. Možete zatražiti poslane Cijena rukovanje Zone crveno, izjave o zaštiti privatnosti, Pravne informacije i sigurnosnim problemima; za doista prekinuta korisnička sučelja; kao i izvršni escalations. 
- Sadržaja za značajku izdanja zadovoljavate poslani rukovanje - sadržaj izdanje značajka zahtijeva prethodnog planiranja ili morate riješiti pomoću standardnih prioritet reda čekanja.


## <a name="in-a-hurry-submit-prs-that-can-be-accepted-automatically"></a>U žurbi? Slanje PRs koji prihvaćaju automatski

Pomoću pravila za automatizaciju PRMerger možete dobiti od vašeg svakodnevno PRs automatski spajaju.

PRMerger odgovarat vaše Cijena automatski ako:
* Utječe na 10 datoteke ili manje.
* Sadrži 15 pamti ili manje.
* Manje od 20% od promjena teksta.
* Birači/switchers nisu ažurirani na oznaku.
* Datoteka koje su izbrisane ili dodati.
* Nema slika iskustva, promijeniti ili izbrisati.

Ako zahtjev za istaknuti zadovoljava sljedeće kriterije, oznake "PRmerger ne možete spojiti" se automatski dodjeljuju da biste znali zahtijeva pregled Ljudski pregledavatelj Cijena.

### <a name="need-to-make-a-lot-of-little-changes"></a>Treba li vam da biste velik broj promjena malo?

Iskoristite svoje natuknica s iznad PRMerger Automatizacija pravila, a zatim učinite sljedeće:
* Slanje članaka s osnovnu promjene zajedno s Cijena s 10 ili manje datoteke.
* Stvorite zaseban Cijena članaka u promjenu slike ili Birači. Potreban je Ljudski pregled.
* Stvorite zaseban Cijena novi ili izbrisane članaka. Potreban je Ljudski pregled.

## <a name="related"></a>Odnosi

- [Kvaliteta kriterija za istaknuti zahtjev za pregled](contributor-guide-pr-criteria.md)

- [Povlačenje Automatizacija zahtjev komentara](contributor-guide-pull-request-comments.md)
