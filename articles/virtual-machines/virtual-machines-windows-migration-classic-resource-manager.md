<properties
    pageTitle="Podržane platforme migracije IaaS resursa od klasičnog Azure resursa upravitelju | Microsoft Azure"
    description="U ovom se članku objašnjavaju migracije podržane platforme resursa od klasičnog za Azure Voditelj resursa"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kasing"/>

# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Podržane platforme migracije IaaS resursa od klasičnog za Azure Voditelj resursa

U ovom se članku opisuju smo kako ste Omogućivanje migracije infrastrukture kao usluge (IaaS) resurse iz na klasični resursima implementacije modela. Dodatne informacije o [Voditelj resursa Azure značajke i pogodnosti paketa](../azure-resource-manager/resource-group-overview.md). Ne možemo detaljno povezivanju resursa iz dva implementacije modela koji postojati u svoju pretplatu pomoću pristupnika virtualne mreže web-mjesto. 

## <a name="goal-for-migration"></a>Cilj za migraciju

Voditelj resursa omogućuje implementacija složene aplikacije pomoću predložaka, konfigurira virtualnim strojevima pomoću VM proširenja i ugrađuje Upravljanje pristupom i označavanje. Azure Voditelj resursa obuhvaća prilagodljivi, paralelnog uvođenje za virtualnim strojevima u skupove dostupnost. Novi model implementacije omogućuje upravljanje životni ciklus računalnim, mreže i pohranu neovisno. Na kraju, postoji usmjerenjem za omogućivanje sigurnost po zadanom provođenja virtualnim strojevima u virtualne mreže.

Gotovo sve značajke iz modela uvođenje klasičnog podržane za računalnim, mreže i prostora za pohranu u odjeljku Upravitelj za Azure resursa. Da biste im nove mogućnosti u Azure Voditelj resursa, možete migrirati postojeću implementacijama iz modela implementacije klasični.

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Promjene Automatizacija i tooling nakon migracije

Tijekom migracije resursa iz modela implementacije klasični model implementacije Voditelj resursa, morate ažurirati postojeće Automatizacija ili tooling da biste bili sigurni da se i dalje funkcionira nakon migracije.

## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Značenje migracije IaaS resursa od klasičnog upravitelju resursa

Prije nego što smo dubinski analizirati prema dolje detalje, pogledajmo razlika između podataka ravnini i upravljanje ravnini operacije IaaS resursa.

- *Upravljanje ravnini* opisuju pozive koje se isporučuju u ravnini upravljanje ili API-JA za izmjenu resursi. Na primjer, operacija kao što su stvaranje na VM, ponovno pokrenuti na VM i ažuriranje virtualne mreže s novi podmreže upravljati izvodi resursi. Ne izravno utječu povezati instance.
- *Ravnini podataka* (aplikacije) u članku se opisuje izvođenje same aplikacije, a uključuje interakciju s instanci koje ne prolaze kroz Azure API-JA. Pristup web-mjesta ili podatke iz pokrenute instance sustava SQL Server ili poslužitelju MongoDB izvlačenja smatra se podaci interakcije ravnini ili aplikacije. Kopiranje blob s računa za pohranu i pristup javna IP adresa RDP ili SSH u virtualnog računala su i ravnini podataka. Te operacije Zadrži aplikacije koje se izvode preko računalnim, mreže i prostora za pohranu.

>[AZURE.NOTE] U nekim slučajevima migracije Azure platforme zaustavlja, deallocates i pokreće virtualnih računala. To uključuje kratki podataka ravnini isključiti.

## <a name="supported-scopes-of-migration"></a>Podržani opsega migracije

Postoje tri opsega migracije koje prvenstveno ciljne računalnim, mreže i prostora za pohranu. 

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migracija virtualnim strojevima (ne u virtualne mreže)

U tom modelu implementaciju upravljanja resursima Poboljšana je sigurnost za aplikacije po zadanom. Sve VMs moraju biti u virtualne mreže u modelu Voditelj resursa. Ponovno pokreće platforme Azure (`Stop`, `Deallocate`, a `Start`) VMs kao dio migracije. Imate dvije mogućnosti za virtualne mreže:

- Možete zatražiti platformu da biste stvorili novi virtualne mreže i migrirati virtualnog računala u novi virtualne mreže.
- Postojeće virtualne mreže u upravitelju resursa možete migrirati virtualnog računala.

>[AZURE.NOTE] U ovom opsegu migracije postupci upravljanja ravnini i operacije podataka ravnini možda neće biti dopušteni za neko vrijeme tijekom migracije.

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migracija virtualnim strojevima (u virtualne mreže)

Za većinu konfiguracije VM, samo metapodataka je migracije između implementacije modelima klasični i resursima. Temeljni VMs su pokrenuti na isti hardver, u istoj mreži i uz isti prostor za pohranu. Upravljanje ravnini operacije možda nije dopuštena za određenog vremena tijekom migracije. Međutim, ravnini podataka nastaviti rad. To je aplikacija izvodi pri vrhu VMs (klasični) ne plaćati nedostupnost tijekom migracije.

Sljedećih konfiguracija trenutno nisu podržani. Ako je podrška dodaje se u budućnosti neke VMs u ovoj konfiguraciji možda plaćati nedostupnost (idite do zaustavljanja, deallocate pa ponovno pokrenite VM operacije).

-   Imate više od jedne dostupnost postaviti u jednom u oblaku.
-   Imate jedan ili više skupova dostupnosti i VMs koji nisu u raspoloživost postaviti u jednom u oblaku.

>[AZURE.NOTE] U ovom opsegu migracije ravnini upravljanje možda neće biti dopušteni za neko vrijeme tijekom migracije. Za određene konfiguracije kao što je opisano ranije, pojavljuje se podaci ravnini nedostupnost.

### <a name="storage-accounts-migration"></a>Migracija račune za pohranu

Da biste omogućili objedinjenog migracije, možete implementirati VMs Voditelj resursa u računu klasični prostora za pohranu. Tu mogućnost Računalni i mrežni resursi možete i potrebno migrirati neovisno o računima za pohranu. Nakon migracije na virtualnim računalima i virtualne mreže trebate migrirati putem računa za pohranu za dovršetak postupka migracije. 

>[AZURE.NOTE] Model implementacije resursima nema pojam klasični slike i diskova. Kada je račun za pohranu migrirani, klasični slike i diskova nisu vidljive u stogu Voditelj resursa, ali sigurnosnom VHDs ostaju na računu za pohranu. 

## <a name="unsupported-features-and-configurations"></a>Nepodržane značajke i konfiguracija

Ne možemo trenutno ne podržava neke značajke i konfiguracije. U sljedećim odjeljcima opisuju naš preporuke vezane uz njih.

### <a name="unsupported-features"></a>Nepodržane značajke

Sljedeće značajke trenutno nisu podržani. Možete po želji ukloniti te postavke, migrirati u VMs i ponovno Omogućivanje postavke u model implementacije Voditelj resursa.

Davatelja resursa | Značajka
---------- | ------------
Izračun | Diskova nepridružene virtualnog računala.
Izračun | Slika virtualnog računala.
Mreža | Krajnja točka ACL-a.
Mreža | Virtualne mreže pristupnika (web-mjesta za web-mjesta, Azure ExpressRoute, pristupnik za aplikaciju, pokažite na web-mjesta).
Mreža | Virtualne mreže pomoću VNet Peering. (Migriranje VNet u OBLAK, a zatim ravnopravni) Dodatne informacije o [VNet Peering] (.. /Virtual-Network/Virtual-Network-peering-overview.md).
Mreža | Upravitelj promet profila.

### <a name="unsupported-configurations"></a>Nepodržane konfiguracije

Sljedećih konfiguracija trenutno nisu podržani.

Servis | Konfiguracija | Preporuka
---------- | ------------ | ------------
Voditelj resursa | Uloga temelji pristup kontrola (RBAC) za klasični resursi | Budući da URI resursa je izmijenjena nakon migracije, preporučuje se planirate ažuriranja RBAC pravilnika koje je potrebno da se dogodi nakon migracije.
Izračun | Više podmreže pridružene na VM | Ažurirajte konfiguracije podmreže referentni samo podmreže.
Izračun | Virtualnim strojevima koji pripadaju virtualne mreže, ali ne morate izričito podmreže koji se dodjeljuje | Po želji možete izbrisati s VM.
Izračun | Virtualnim strojevima koji imaju upozorenja, a zatim automatsko skaliranje pravila | Migracija prolazi kroz i ispuštaju se ove postavke. Preporučuje se vrednuju vaše okruženje prije migracije. Osim toga, konfigurirajte postavke upozorenja nakon migracije.
Izračun | Nastavci XML VM (BGInfo 1.*, ispravljanje pogrešaka za Visual Studio, implementacija Web i daljinsko uklanjanje programskih pogrešaka) | Nije podržano. Preporučuje se da te proširenja uklanjanje virtualnog računala da biste nastavili migraciju ili one će biti odbačeni automatski tijekom postupka migracije.
Izračun | Pokretanje dijagnostike s Premium prostora za pohranu | Onemogućivanje Dijagnostika pokretanje značajke za na VMs prije nastavka migracije. Pokretanje dijagnostike u stogu Voditelj resursa možete omogućiti ponovno nakon migracije. Uz to, blob-ova koji se koriste za serijski Zapisnici i snimka treba moguće je izbrisati da bi vam se više neće naplatiti za te blob-ova.
Izračun | Servisi u oblaku koji sadrže web-tempiranja uloge | To trenutno nije podržano.
Mreža | Virtualne mreže koji sadrže virtualnih računala i web-tempiranja uloge |  To trenutno nije podržano.
Aplikacije servisa za Azure | Virtualne mreže koje sadrže aplikacije servisa za okruženja | To trenutno nije podržano.
Azure HDInsight | Virtualne mreže koje sadrže servisa HDInsight | To trenutno nije podržano.
Servisi Microsoft Dynamics životni ciklus | Virtualne mreže koje sadrže virtualnim strojevima upravlja usluge Dynamics životni ciklus | To trenutno nije podržano.
Izračun | Centar za sigurnost Azure proširenja pomoću VNET VPN pristupnika ili ER pristupnika s lokalno DNS poslužitelj | Centar za sigurnost Azure automatski instalira proširenja na virtualnim računalima i praćenje njihova sigurnost podizanje upozorenja. Ovim nastavcima obično se instalira automatski ako centar za sigurnost Azure pravila omogućena na pretplatu. Pristupnik za migraciju trenutno nije podržano i pristupnika nije potrebno izbrisati prije nastavka potvrđivanja migracije, pristup Internetu s računom za pohranu VM nestaje nakon brisanja pristupnika. Migracija će nakon toga kad se to dogodi dok se ne može biti ispunjena blob status agent za goste. Preporučuje se da biste onemogućili centar za sigurnost Azure pravila u sklopu pretplate 3 sata prije nastavka migracije.

## <a name="the-migration-experience"></a>Sučelje za migraciju

Prije nego što počnete sučelje za migraciju, preporučuje se sljedeće:

- Provjerite ne koristite resursa koje želite migrirati nepodržane značajke ni konfiguracije. Obično platforme otkriva te probleme i generira pogrešku.
- Ako imate VMs koji nisu u virtualne mreže, oni će se zaustavio i deallocated kao dio Priprema operacije. Ako ne želite izgubiti na javnu IP adresu, pogleda rezervirati IP adresu prije pokretanje postupka Priprema. Međutim, ako se nalazite u VMs virtualne mreže, oni se ne zaustavio i deallocated.
- Planiranje migracije tijekom koje nisu-radno vrijeme kako bi odgovarao za sve neočekivane pogreške koja se može dogoditi tijekom migracije.
- Preuzmite Trenutna konfiguracija sustava VMs pomoću komponente PowerShell, naredbe sučelje naredbenog retka (EŽA) ili REST API-ji radi lakšeg provjere valjanosti nakon Priprema korak.
- Ažurirajte automatizaciju/operationalization skripte za rukovanje model implementacije resursima prije nego što počnete migracije. Po želji možete učiniti početak operacije kada su resursi u pripremljeni stanju.
- Analiza RBAC pravilnike koji konfigurirane na klasični IaaS resurse i planiranje nakon migracije.

Tijek rada preseljenja je na sljedeći način

![Snimka zaslona koja prikazuje tijek rada preseljenja](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] Sve operacije što je opisano u sljedećim odjeljcima su idempotent. Ako imate problema koji nije nepodržane značajke ili pogreške u konfiguraciji, preporučuje se ponovno pokušajte Priprema, prekinuti ili izvršavanje operacija. Azure platforme pokuša željenu akciju.

### <a name="validate"></a>Provjera valjanosti

Operacija Provjera prvi je korak postupka migracije. Cilj ovaj korak je da biste analizirali podatke u pozadini za resurse u odjeljku migracije i vratili uspjeh/pogreške ako su resursi koji podržavaju migracije.

Odaberite virtualne mreže ili na servis (Ako je ne virtualne mreže) da biste provjerili valjanost za migraciju.

* Ako je resurs koji podržavaju migracije, Azure platforme navodi sve na razloga zašto nije podržan za migraciju.

### <a name="prepare"></a>Priprema

Priprema operacija drugi je korak postupka migracije. Cilj ovaj korak je kao zamjenu za transformaciju resursa IaaS od klasičnog resursima Voditelj resursa i izlaganje usporedno na vizualizaciju.

Odaberite virtualne mreže ili na servis (Ako je ne virtualne mreže) koje želite pripremiti za migraciju.

* Ako je resurs koji podržavaju migracije, Azure platforme zaustavlja se postupak migracije popise i razloga zašto Priprema operacija nije uspjela.
* Ako je resurs koji podržavaju migracije, zaključavanja prvog platforme Azure dolje operacije upravljanja ravnini za resurse u odjeljku migracije. Na primjer, niste mogli dodati podatkovni disk na VM u odjeljku migracije.

Azure platforme zatim pokreće migracije metapodatke od klasičnog upravitelju resursa za migriranje resurse.

Po dovršetku postupka Priprema imate mogućnost vizualizacija resursa u oba klasični i resursima. Za svaki servis u oblaku u modelu klasični implementaciju, platforme Azure stvara naziv grupe resursa koji sadrži uzorak `cloud-service-name>-migrated`.

>[AZURE.NOTE] Virtualnim strojevima koji nisu u klasični virtualne mreže su zaustavljena deallocated u ovoj fazi migracije.

### <a name="check-manual-or-scripted"></a>Provjera (ručno ili skriptiranih)

U koraku potvrdite po želji možete koristiti konfiguracije koji ste preuzeli ranije da biste provjerili valjanost migracije izgledom. Umjesto toga se možete prijaviti portal i mjesto provjerite svojstava i resursa za provjeru valjanosti izgleda li dobro metapodatke migracije.

Ako premještate virtualne mreže, većina konfiguracije virtualnim strojevima je ponovno pokrenuti. Za aplikacije na te VMs, možete provjeriti je li aplikacija i dalje s radom.

Možete testirati i nadzor/Automatizacija i radu skripte da biste vidjeli ako su u VMs funkcioniraju kako želite, a ažurirane skripte funkcionira ispravno. Podržani su samo početak operacije kada su resursi u pripremljeni stanju.

Nema prozora skup vrijeme prije kojeg ćete morati izvršiti migracije. Koliko vremena koje želite u tom stanju može potrajati. Međutim, ravnini upravljanje zaključan za ove resurse dok ste prekinuti ili izvršavanje.

Ako vidite probleme, uvijek možete prekinuti migracije i vratite se u model klasični implementacije. Kada se vratite, Azure platforme otvorit će se postupci upravljanja ravnini za resurse tako da možete nastaviti s normalnom operacije te VMs u modelu klasični implementacije.

### <a name="abort"></a>Prekid

Prekid korak nije obavezan koje možete koristiti da biste vratili promjene u model uvođenje klasičnog i zaustavljanje migracije.

>[AZURE.NOTE] Ovaj postupak nije moguće izvršiti nakon što ste pokrene izvršavanje operacija.  

### <a name="commit"></a>Izvršavanje

Nakon što završite Provjera valjanosti, možete izvršiti migracije. Resurse ne prikazuju se više klasičnom i dostupne su samo u model implementacije Voditelj resursa. Resursi za migriranim upravlja se samo u novom portalu.

>[AZURE.NOTE] Ovo je postupak idempotent. Ako ga ne uspije, preporučuje se ponovno pokušajte postupak. Ako se i dalje nije uspjela, stvorite zahtjev za podršku možete ili stvoriti objavu forum s oznakom ClassicIaaSMigration na našem [VM forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

## <a name="frequently-asked-questions"></a>Najčešća pitanja

**Tarifu za migraciju utječe na bilo koji od Moje postojeće servise ili aplikacije koji se izvode na Azure virtualnih računala?**

ne. (Klasične) VMs su potpuno podržane usluge u Općenito dostupan. Možete nastaviti koristiti ove resurse da biste proširili vaše ostavlja manji trag pri na Microsoft Azure.

**Što se događa Moje VMs ako se ne planiram o migraciji u skorijoj budućnosti?**

Ne možemo su deprecating postojeće klasični API-ji i model resursa. Želimo koji olakšavaju migracije, preporučuje se napredne značajke koje su dostupne u modelu implementacije Voditelj resursa. Preporučujemo da pregledate za [neke od na napretke](virtual-machines-windows-compare-deployment-models.md) koje su dio IaaS u odjeljku upravitelj resursa.

**Što tarifu za migraciju znači za moju postojeće tooling?**

Ažuriranje sustava tooling implementaciju model upravljanja resursima jedan je od najvažnijih promjene koje ste da bi se u tarifama za migraciju.

**Koliko dugo nedostupnost upravljanje ravnini bit će?**

Ovisi o broju resursa koji se migriraju. U slučaju manje implementacije (nekoliko tens VMs) cijeli migracije traje manje od jednog sata. Za veliki implementacije (stotine VMs) Migracija može potrajati nekoliko sati.

**Možete se spojiti natrag nakon Moje migriranje Resursi su izvršene u upravitelju resursa?**

Možete prekinuti migraciju pod uvjetom da su resursi u pripremljeni stanju. Vraćanje nije podržan kada resurse imate uspješno migrirani kroz postupak potvrđivanja.

**Mogu li vratiti Moje migracije ako potvrdi ne uspije?**

Migracija ne možete prekinuti ako potvrdi ne uspije. Sve operacije migracije, uključujući postupak potvrđivanja su idempotent. Stoga preporučujemo da ponovno pokušajte postupak nakon kratko vrijeme. Ako i dalje biti namijenjeno pogrešku, stvorite zahtjev za podršku možete ili stvoriti objavu forum s oznakom ClassicIaaSMigration na našem [VM forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

**Imati za kupnju drugog eksplicitnih usmjeravanje elektronička ako imam da biste koristili IaaS u odjeljku Upravljanje resursima?**

ne. Ne možemo nedavno omogućen i [Premještanje krugova ExpressRoute od klasičnog model implementacije Voditelj resursa](../expressroute/expressroute-move.md). Ne morate kupiti novi elektronička ExpressRoute ako već postoji.

**Što se događa ako pravila na temelju uloga kontrola pristupa li imali konfigurirana za moj klasični IaaS resursa?**

Tijekom migracije resurse transformirati od klasičnog upravitelju resursa. Stoga preporučujemo da planirate ažuriranja RBAC pravila koje je potrebno da se dogodi nakon migracije.

**Što ako koristim oporavak Azure web-mjesta ili sigurnosne kopije Azure danas?**

Da biste migrirali virtualnog računala koje su omogućene za sigurnosne kopije, pogledajte [li sigurnosnu kopiju moje klasični VMs u sigurnosno kopiranje zbirke ključeva. Sada želim migrirati Moje VMs iz klasičan način rada u načinu Voditelj resursa. Kako se sigurnosno kopirajte ih u oporavak servisa sigurnog?](../backup/backup-azure-backup-ibiza-faq.md#i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode-how-can-i-backup-them-in-recovery-services-vault)

**Može li provjeriti valjanost Moje pretplate ili resurse da biste vidjeli ako takvi stupci koji podržavaju migracije?**

Da. U mogućnosti podržane platforme migracije u prvom koraku pripremate za migraciju je da biste provjerili jesu li resurse koji podržavaju migracije. U slučaju da Provjeri valjanost postupak ne uspije, primili ste poruku sve razloga nije moguće završiti migracije.

**Što se događa ako se nađete u kvote pogreške prilikom pripreme IaaS resursi za migraciju?**

Preporučujemo prekinuti migraciju i zatim se prijavite zahtjev za podršku da biste povećali kvote u području koje premještate u VMs. Kada je Odobri zahtjev za kvote, možete početi ponovno izvođenje postupka migracije.

**Kako se prijaviti problema?**

Objavite problemi i pitanja o migraciji na našem [forumu VM](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows), pomoću ključne riječi ClassicIaaSMigration. Preporučujemo da objavljivanje svoja pitanja na ovom forum. Ako imate ugovor za podršku, ste dobrodošlice da biste se prijavili kao i karata podrška.

**Što ako se ne sviđa nazive resursa koju platformu odabrali tijekom migracije?**

Sve resurse koji izričito pružaju nazive u modelu uvođenje klasičnog zadržavaju se tijekom migracije. U nekim slučajevima se stvaraju nove resurse. Na primjer: za svaku VM stvara se mrežno sučelje. Trenutno ne podržavamo mogućnost da biste odredili imena tih novi resursa stvorene tijekom migracije. Prijavite se glasova tu značajku na [forum za Azure povratne informacije](http://feedback.azure.com).

* *Dobio sam poruku *"VM izvješćivanje o pogreškama ukupnog statusa agent kao nije spreman. Dakle, na VM nije moguće migrirati. Provjerite je li VM Agent je prijavljivanje ukupnog statusa agent kao spremna"* ili *"VM sadrži kućni broj čiji je Status je koji se prijavili iz na VM. Dakle, ova VM nije moguće migrirati."***

Ova poruka je primio kada se VM nema izlaznog povezani s Internetom. Agent za VM koristi izlaznog povezivanje dosegne račun za Azure prostora za pohranu za ažuriranje statusa agent svakih pet minuta.


## <a name="next-steps"></a>Daljnji koraci
Sada kada razumijete migracije klasični IaaS resursi za Voditelj resursa, možete početi migracije resursi.

- [Tehnički precizno dive na platformi podržane migracije od klasičnog za Azure Voditelj resursa](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [Migrirajte resursi IaaS od klasičnog Azure resursa upravitelju pomoću komponente PowerShell](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [Migrirajte resursi IaaS od klasičnog Azure resursa upravitelju pomoću EŽA](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Kloniraj klasični virtualnog računala da biste Azure Voditelj resursa putem skripti komponente PowerShell zajednice](virtual-machines-windows-migration-scripts.md)
