<properties
   pageTitle="Uvod u sigurnost paket za upravljanje operacije i rješenje nadzora | Microsoft Azure"
   description="Ovaj dokument olakšava početak rada s operacije upravljanja paket sigurnost i mogućnosti rješenje nadzora praćenje vaše hibridnog oblaka."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="get-started-article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>
 
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Uvod u sigurnost paket za upravljanje operacije i rješenje nadzora
Ovaj dokument pomaže vam brz početak s mogućnostima rješenje operacije upravljanja paket (OMS) sigurnost i nadzora tako da vam navođenjem preko svake mogućnosti.

## <a name="what-is-oms"></a>Što je OMS?
Microsoft operacije upravljanja paket (OMS) je Microsoftov oblaku IT rješenja upravljanja koji olakšava upravljanje i zaštita lokalnih i infrastrukture u oblaku. Dodatne informacije o OMS, pročitajte članak [Paket za upravljanje operacije](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>Nadzorna ploča OMS sigurnost i nadzora

Rješenje OMS sigurnost i nadzora nudi potpun prikaz u vašoj tvrtki ili ustanovi IT sigurnost posture s upitima ugrađeno pretraživanje za najvažnije problema koje je potrebno obratiti pozornost. Nadzorna ploča za **Sigurnost i nadzora** je početnog zaslona za sve vezano uz sigurnost u OMS. Pruža više razine uvid u stanja sigurnosti računala. Sadrži i mogućnost da biste prikazali sve događaje iz prethodnih 24 sata, 7 dana ili bilo koji drugi prilagođeni vremenski okvir. Da biste pristupili nadzorne ploče za **Sigurnost i nadzora** , slijedite ove korake:

1. U **Paketu za upravljanje operacije Microsoft** glavna nadzorna ploča kliknite pločice **Postavke** na lijevoj strani.
2. U plohu **Postavke** u odjeljku **rješenja** kliknite **Sigurnost i nadzora** mogućnost.
3. Pojavit će se na nadzornoj ploči **Sigurnost i nadzora** :

    ![Nadzorna ploča OMS sigurnost i nadzora](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Ako prvi put pristupate ovu nadzornu ploču, a nemate uređaji nadzire OMS, pločice ne popunjavaju podacima dobiti agenta. Kada instalirate agenta, može potrajati neko vrijeme da biste popunili, stoga što vidite prethodno možda nedostaje neki podaci kao što je i dalje prijenos u oblak.  U ovom slučaju uobičajeno je da biste vidjeli neke pločice bez materijalna informacija. Pročitajte [računalima povezivanje Windows izravno OMS](https://technet.microsoft.com/library/mt484108.aspx) dodatne informacije o instaliranju OMS agent u sustavu Windows i [računalima povezivanje Linux OMS](https://technet.microsoft.com/library/mt622052.aspx) dodatne informacije o tome da biste izvršili taj zadatak u sustavu Linux.

> [AZURE.NOTE] Agenta će prikupiti podatke na temelju trenutne događaje koje su omogućene, primjerice naziv računala, IP adresu i korisničko ime. No dokument datoteke, naziv baze podataka niti privatne podatke prikuplja.   

Rješenja su zbirka logike, vizualizacija i podataka iz pravila acquisition tu adresu izazove glavni klijent. Sigurnost i nadzora jedan rješenja, drugim korisnicima možete dodati posebno. Pročitajte članak [Dodavanje rješenja](https://technet.microsoft.com/library/mt674635.aspx) za dodatne informacije o dodavanju novog rješenja.

Na nadzornoj ploči OMS sigurnost i nadzora organizirani u četiri glavna kategorije:

- **Sigurnost domene**: u ovom području moći da biste dodatno Istraživanje sigurnost zapisa s vremenom, pristup rizika od zlonamjernog softvera, ažurirati rizika, mrežne sigurnosti, identiteta i pristup podacima, računalima s sigurnosne događaje i brzo pristupiti centru za sigurnost Azure nadzorne ploče.
- **Najvažnije problema**: tu mogućnost vam omogućuje da brzo prepoznavanje broj aktivni problema i težinu te probleme.
- **Dlp-a (pretpregled)**: omogućuje vam da biste odredili uzorke napada po vizualizacija sigurnosnih upozorenja, kao što su odvija protiv resursa.
- **Obavještavanje prijetnju**: omogućuje vam da biste odredili uzorke napada po vizualizacija ukupan broj poslužitelje s odlazni promet IP zlonamjernih, vrstu zlonamjerni prijetnju i kartu koja prikazuje gdje su sljedeće IP-ovi dolaze iz. 
- **Uobičajene upite sigurnost**: tu mogućnost daje popis Najčešći upiti sigurnost koje možete koristiti za praćenje vaše okruženje. Kada kliknete iz nekog od tih upita, otvara plohu **pretraživanja** s rezultata za taj upit.

> [AZURE.NOTE] Dodatne informacije o kako OMS čuva podatke sigurne, pročitajte kako OMS secures podataka.

## <a name="security-domains"></a>Sigurnost domene

Kada nadzor resursa, važno je da biste mogli brzo pristupiti trenutnog stanja u okruženju sustava. No i važno je da biste mogli pratiti natrag događaja koji se u prošlosti koji može uzrokovati da biste bolje razumjeli što se događa u okruženju u kojem u određenom trenutku u vremenu. 

> [AZURE.NOTE] zadržavanje podataka sastavljanja da biste na OMS cijene plan. Dodatne informacije potražite u [Paketu za upravljanje operacije Microsoft](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) cijene stranice.

Incidenta scenariji istrage odgovor i forensics izravno će im dostupnih u **Sigurnost zapisa s vremenom** pločice rezultata.

![Sigurnost zapisa tijekom vremena](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Kada kliknete na toj pločici, plohu **pretraživanja** otvorit će se, rezultat upita za **Sigurnost događaje** (Vrsta = SecurityEvents) s podacima koji se temelji na zadnjih sedam dana, kao što je prikazano u nastavku:

![Sigurnost zapisa tijekom vremena](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

Rezultat pretraživanja podijeljen u dva okna: okno na lijevoj strani daje razrada broja sigurnost događaja koji nisu pronađeni računala u kojoj te događaje pronađene su, broj računa koji su otkriven ta računala i vrste aktivnosti. U desnom oknu sadrži ukupno rezultata i kronološki prikaz sigurnosne događaje s nazivom i događaja aktivnosti na računalu. Možete kliknuti **Prikaži više** da biste vidjeli dodatne detalje o tom slučaju, kao što su podaci o događaju, ID događaja i izvor događaja.

> [AZURE.NOTE] Dodatne informacije o OMS upit za pretraživanje, pročitajte [OMS pretraživanje reference](https://technet.microsoft.com/library/mt450427.aspx).

### <a name="antimalware-assessment"></a>Procjena Antimalware

Ta mogućnost omogućuje vam da biste brzo pronašli računala s nedovoljno zaštitu i računala na kojima su ugrožena po dio zlonamjernog softvera. Pročitati status rizika od zlonamjernog softvera i otkriven prijetnji na nadziranim poslužiteljima, a zatim podaci se šalju OMS servisu u oblaku za obradu. Poslužitelji otkrio poslužitelje s nedovoljno zaštitu i prijetnji prikazuju se u nadzornu ploču procjenu zlonamjernog softvera, koja je dostupna kada kliknete pločicu **Antimalware procjenu** . 

![Procjena od zlonamjernog softvera](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Baš kao i sve druge pločicu uživo dostupne u OMS nadzorne ploče, prilikom klika na njega plohu **pretraživanja** će se otvoriti s rezultata upita. Za tu mogućnost ako kliknete mogućnost **Ne izvješćivanje o pogreškama** u odjeljku **Zaštita Status**će automatski dobiti rezultat upita koji prikazuje jednu stavku koja sadrži naziv računala i njegov položaj, kao što je prikazano u nastavku:

![rezultat pretraživanja](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [AZURE.NOTE] *položaj* je ocjene dodjeljivanja tako da odražava stanje zaštitu (uključeno, isključeno, ažurirati, itd) i prijetnji koje se nalaze. Koje imate kao broj omogućuje da zbrajanja.

Ako kliknete naziv računala, morat ćete kronološki Prikaz statusa zaštite računala. Ovo je vrlo koristan scenarija u kojem morate znati ako na antimalware jednom instaliran i trenutku uklonjena je.   

### <a name="update-assessment"></a>Ažuriranje procjenu 

Ta mogućnost omogućuje vam da brzo utvrde cjelokupan izlaganje potencijalni problemi sa sigurnosnim i je li ili kako ključnih te ažuriranja za vaše okruženje. OMS sigurnost i rješenje nadzora samo pružaju vizualizacije ažuriranja, stvarnih podataka dolazi iz [Rješenja ažuriranja sustava](https://technet.microsoft.com/library/mt484096.aspx), što je drugi modul unutar OMS. Evo primjera ažuriranja:

![ažuriranja sustava](./media/oms-security-getting-started/oms-getting-started-fig6.png)

> [AZURE.NOTE] Dodatne informacije o ažuriranjima rješenja, pročitajte [Ažuriranje poslužitelja s rješenjem ažuriranja sustava](https://technet.microsoft.com/library/mt484096.aspx).

### <a name="identity-and-access"></a>Identitet i pristup

Identitet mora biti ravnini kontrola za svoju tvrtku zaštita vaš identitet mora biti glavni prioritet. Dok je u prošlosti su perimeters oko tvrtke ili ustanove i one perimeters su nešto granice primarni defensive, danas s više podataka i drugih aplikacija premještanja u oblak identitet postaje novi opseg. 

> [AZURE.NOTE] trenutno podataka temelji samo podatke za prijavu sigurnosne događaje (događaj 4624 ID-a) u buduće prijave u Office 365 i Azure AD podataka bit će uključene.

Prateći aktivnosti identiteta će moći poduzeti određene proaktivne prije incident vodi mjesto ili ponovno aktiviranje akcije da biste zaustavili pokušaj napada. Na nadzornoj ploči **identiteta i pristup** sadrži pregled stanje vašeg identiteta, uključujući broj neuspjelih pokušaja da biste se prijavili, na korisnički račun koji su se koristile tijekom te pokušaji, račune koji su zaključan, računa s promijenjene ili ponovno postavljanje lozinke i trenutno broj računa koji ste prijavljeni. 

Kada kliknete pločicu **identiteta i pristup** prikazat će se nadzorna ploča za sljedeće:

![identitet i pristup](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

Informacije koje su dostupne u ovu nadzornu ploču možete odmah vam pomoći da biste odredili potencijalne sumnjivoj aktivnosti. Ako, na primjer, postoje 338 pokušava prijavite se kao **Administrator** i 100% od ovih pokušaji nije uspjelo. Uzrok može biti brute prisilno napada s ovim računom. Ako kliknete na tom računu će dobiti dodatne informacije koje vam mogu pomoći određivanje ciljnog resurs za ovu mogućnost napada:

![Rezultati pretraživanja](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

Detaljno izvješće sadrži važne informacije o tom događaju, uključujući: ciljnog računala, vrstu prijave (u ovom slučaja Mrežna prijava), aktivnosti (u ovom slučaju događaju 4625) i potpun vremenske crte svaki pokušaj. 

### <a name="computers"></a>Računala

Ova pločica se može koristiti za pristup svim računalima s aktivno sigurnosne događaje. Kada kliknete u toj pločici vidjet ćete popis računala s sigurnosne događaje i broj događaja na svakom računalu:

![Računala](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Možete nastaviti vaše istrage tako da kliknete na svakom računalu i pregledajte sigurnosne događaje koje su označene.

### <a name="azure-security-center"></a>Centar za sigurnost Azure

Toj pločici zapravo je prečac da biste pristupili centru za sigurnost Azure nadzorne ploče. Dodatne informacije o rješenje pročitajte [Uvod u Centar za sigurnost Azure](../security-center/security-center-get-started.md) .

## <a name="notable-issues"></a>Najvažnije problema

Cilju ovu grupu mogućnosti tako da omogućuje brzi prikaz problemima koji se nalaze u okruženju sustava tako da ih kategoriziranje od ključne važnosti, upozorenja i Informational. Pločica vrsta aktivnog problem je vizualizacije te probleme, ali ne dopušta da biste istražili dodatne detalje o njima, za ćete morati koristiti donjem dijelu toj pločici koja ima naziv problema (ime), koliko objekata imali tu (broj) te kako ključnih je (TEŽINU).

![Najvažnije problema](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Vidjet ćete da te probleme već su prekriveni u različitim područjima grupe za **Sigurnost domene** koje pojačavaju cilj prikaz: vizualizacija najvažnije probleme u okruženju sustava s jednoga mjesta.

## <a name="detections-preview"></a>Dlp-a (pretpregled)

Cilju Ova mogućnost je omogućiti IT brzo prepoznavanje potencijalnih prijetnji svoje okruženje putem i težinu ovaj prijetnju.

![Intel prijetnju](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Tu mogućnost možete se koristi tijekom istrazi incidenta odgovor za izvođenje na vrednovanje i dobiti dodatne informacije o na napada.

> [AZURE.NOTE] Dodatne informacije o korištenju OMS Incident odgovor, pogledajte [upute za korištenje Azure centar za sigurnost i Microsoft operacije upravljanja paket za je Incident odgovor](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703).

## <a name="threat-intelligence"></a>Obavještavanje prijetnju

Nova sekcija obavještavanje prijetnju rješenja za sigurnost i nadzora vizualiziraju uzoraka mogućih napada na nekoliko načina: ukupan broj poslužitelje s odlazni promet IP zlonamjernih, vrstu zlonamjerni prijetnju i kartu koja prikazuje gdje su sljedeće IP-ovi dolaze iz. Možete komunicirati s kartom i kliknite na IP-ovi za dodatne informacije.

Žuti pribadače na karti označavaju dolazne promet od zlonamjernog IP-ovi. Nije neuobičajeno za poslužitelje koji su izložen putem Interneta da biste vidjeli zlonamjerni promet, no preporučujemo da pročitate ove pokušaja da biste bili sigurni nijedan od njih nije uspio. Ove pokazatelja temelje se na IIS zapisnike, WireData i prijavljuje Vatrozid za Windows.  

![Intel prijetnju](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Uobičajene upite sigurnosti

Popis uobičajenih sigurnosnih upita dostupna može biti korisno možete brzo pristupiti podacima resursa i prilagodite ga u skladu potrebama vaše okruženje. Te zajednički upiti su:

- Sve aktivnosti sigurnosti
- Sigurnost aktivnosti na računalu "computer01.contoso.com" (replace uz naziv računala)
- Sigurnost aktivnosti na računalu "computer01.contoso.com" za račun "Administrator" (Zamijeni ovim vlastite nazive računalu i račun)
- Prijavite se na aktivnosti na računalu
- Računi koji je prekinut Microsoft antimalware na bilo kojem računalu
- Računalima na kojima je prekinuta antimalware postupka Microsoft
- Izvršeno na računalima na kojima je "hash.exe" (replace pod nazivom drugi proces)
- Svi nazivi postupak koja su izvršava
- Prijavite se na aktivnosti račun
- Računi koji daljinski prijavljeni na računalu "computer01.contoso.com" (replace uz naziv računala)

## <a name="see-also"></a>Vidi također

U ovom dokumentu su predstavljena OMS sigurnost i nadzora rješenja. Dodatne informacije o sigurnosti OMS potražite u sljedećim člancima:

- [Pregled operacije upravljanja paketa (OMS)](operations-management-suite-overview.md)
- [Praćenje i odgovarati na njih sigurnosnih upozorenja u operacije upravljanja paketa sigurnost i rješenje nadzora](oms-security-responding-alerts.md)
- [Nadzor resursa u operacije upravljanja paketa sigurnost i rješenje nadzora](oms-security-monitoring-resources.md)
