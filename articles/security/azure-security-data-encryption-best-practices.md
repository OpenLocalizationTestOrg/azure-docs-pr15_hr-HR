<properties
   pageTitle="Sigurnost podataka i šifriranje najbolje prakse | Microsoft Azure"
   description="U ovom se članku daje skup najbolje prakse za sigurnost podataka, a zatim šifriranje pomoću ugrađenih u Azure mogućnosti."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yuridio"/>

#<a name="azure-data-security-and-encryption-best-practices"></a>Najbolje prakse zaštite Azure podataka i šifriranje

Jednu od tipki za zaštitu podataka u oblaku računovodstvo za mogućih stanja u koji se može pojaviti podataka i koje kontrole su dostupne za to stanje. Sigurnost i šifriranje najbolje prakse preporuke svrhu Azure podataka bit će oko stanja sljedeće podatke:

- Pri – ostale: To uključuje sve podatke za objekte za pohranu, spremnika i vrste koje se na fizičke medijskih sadržaja, postoji statički biti ga magnetski ili optičko disk.

- Tranzitne: Kada podataka koji se prenose između komponente, mjesta ili programa, kao što su putem mreže, preko bus servisa (iz lokalnog na cloud i obratno, uključujući veze hibridnog kao što je ExpressRoute) ili je tijekom ulaza i izlaza ga je smatrati koja se u animacije.

U ovom članku smo obrađuje skup podataka Azure sigurnost i šifriranje najbolje prakse. Najbolje prakse potječu iz naših sučelje s sigurnost Azure podataka i šifriranje i iskustva klijenata kao što je sami.

Za svaki preporučenim načinom rada ne možemo ćete u članku se objašnjava:

- Što je najbolji način
- Zašto na koje želite omogućiti tu preporučenim načinom rada
- Ako omogućite preporučenim načinom rada što bi moglo biti rezultat
- Moguća alternativa najbolja praksa
- Kako saznati da biste omogućili najbolja praksa

U ovom se članku sigurnost podataka Azure i najbolje prakse za šifriranje temelji se na mišljenje na suglasnost i mogućnosti platforme Azure i skupove, kao što je postoje trenutku ovaj članak. Mišljenja i tehnologije mijenjaju tijekom vremena, a u ovom se članku ažurirat će se redovito u skladu s tim promjenama.

Azure podataka sigurnost i šifriranje najbolje prakse spominju u ovom članku obuhvaćaju sljedeće:

- Nametanje višestruka provjera autentičnosti
- Kontrola pristupa (RBAC) na temelju uloga za korištenje
- Šifriranje Azure virtualnim strojevima
- Korištenje modela sigurnost hardvera
- Upravljanje sigurne radne stanice
- Omogućivanje šifriranje podataka za SQL
- Zaštita podataka na putu
- Nametanje šifriranje datoteke razine podataka


## <a name="enforce-multi-factor-authentication"></a>Nametanje višestruka provjera autentičnosti

Prvi korak u pristupa podacima i kontrolu u Microsoft Azure je provjere autentičnosti korisnika. [Azure višestruke provjere autentičnosti (MFA)](../multi-factor-authentication/multi-factor-authentication.md) je metoda potvrđivanja identitet korisnika na neki drugi način od samo korisničko ime i lozinku. Provjera autentičnosti način olakšava zaštitu pristup s podacima i aplikacijama dok korisnik zahtjev za jednostavne postupak prijave za sastanak.

Omogućivanjem Azure MFA za korisnike dodajete drugu razinu zaštite znak dodaci za korisnika i transakcije. U ovom slučaju transakcije možda pristupa dokumenta koji se nalazi u datotečnom poslužitelju ili sustava SharePoint Online. Azure MFA pomaže IT da biste smanjili vjerojatnost da compromised vjerodajnica će imati pristup podacima tvrtke ili ustanove.

Na primjer: Ako nametnete Azure MFA za korisnike i konfigurirajte ga tako da biste upotrijebili telefonski poziv ili tekstna poruka provjere valjanosti, ako ugrožena vjerodajnica korisnika, napadač nećete moći pristupiti bilo kojeg resursa jer on će automatski dobiti pristup telefona korisnika. Dodati ovaj dodatni slojeva zaštite identiteta tvrtkama ili ustanovama su podložnija za napada krađe vjerodajnica, što može dovesti do narušena pouzdanost podataka.

Jedan odlučite tvrtkama ili ustanovama koje želite zadržati u provjere autentičnosti kontrola lokalnog se [Azure višestruke provjere autentičnosti poslužitelja](../multi-factor-authentication/multi-factor-authentication-get-started-server.md)naziva se i MFA lokalni. Ako koristite taj način i dalje moći Nametni višestruka provjera autentičnosti, uz zadržavanje na MFA informacije o lokalnom poslužitelju.

Dodatne informacije o Azure MFA, pročitajte članak [Početak rada s Azure višestruke provjere autentičnosti u oblaku](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Kontrola pristupa (RBAC) na temelju uloga za korištenje
Ograničavanje pristupa na temelju sigurnost načela [informacije](https://en.wikipedia.org/wiki/Need_to_know) i [najmanjih ovlasti](https://en.wikipedia.org/wiki/Principle_of_least_privilege) . Ovo je izuzetne tvrtkama ili ustanovama koje želite da biste nametnuli sigurnosna pravila za pristup podacima. Azure na temelju uloga pristup kontrola (RBAC) mogu se dodijeliti dozvole za korisnike, grupe i aplikacije, na određene opseg. Opseg Dodjela uloga može biti pretplatu, grupu resursa ili pojedinačni resurs.

Na raspolaganju su [ugrađene RBAC uloge](../active-directory/role-based-access-built-in-roles.md) Azure korisnicima dodijeliti prava. Razmislite o korištenju *Suradničke račun za pohranu* za operatora cloud koji su potrebni za upravljanje računima za pohranu i ulogu *Klasični suradnika račun za pohranu* za upravljanje računima klasični prostora za pohranu. Oblak operatore koje su potrebne za upravljanje VMs i račun za pohranu, razmislite o dodavanju ulogu *Suradnika virtualnog računala* .

Tvrtke i ustanove kojih se provode kontrola pristupa podacima tako da korištenje kao što su RBAC možda dodjeljivanja dodatne ovlasti od potrebne za svoje korisnike. To može dovesti do narušena pouzdanost podataka tako da neki korisnici imate pristup podacima koji imaju bi trebalo lakim.

Saznajte više o Azure RBAC tako da pročitate članak [Azure Role-Based kontrola pristupa](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Šifriranje Azure virtualnim strojevima
Za mnoge organizacije [šifriranje podataka na ostale](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) je obavezna korak prema podataka o zaštiti privatnosti i usklađenost samostalnosti podataka. Azure šifriranje omogućuje administratori za šifriranje diskova Windows i Linux IaaS virtualnog računala (VM). Azure šifriranje upravlja industrijskih standardne BitLocker značajka sustava Windows i značajka DM Crypt Linux omogućuje šifriranje jedinice za os-a i diskova podataka.

Omogućuje korištenje Azure šifriranje radi zaštite i zaštitili vaše podataka da bi zadovoljili vaše tvrtke ili ustanove sigurnosti i zahtjeve za usklađenosti. Tvrtke i ustanove obzir morate uzeti i pomoću šifriranja da biste lakše prevladavanje rizike vezane uz neovlašteno podataka access. Također je preporučljivo šifriranja pogona prije pisanja povjerljive podatke.

Provjerite je li za šifriranje vaše VM količine podataka i pokretanje jedinici za zaštitu podataka na ostale na vašem računu Azure prostora za pohranu. Zaštitili ključeva za šifriranje i tajne po korištenje [Sigurnog ključ Azure](../key-vault/key-vault-whatis.md).

Za vaše lokalne poslužitelje sustava Windows, imajte na umu sljedeće šifriranje najbolje prakse:

- Korištenje [programa BitLocker](https://technet.microsoft.com/library/dn306081.aspx) za šifriranje podataka
- Spremanje informacija za oporavak u AD DS.
- Ako postoji neki problem da ste je ugrožena ključevima programa BitLocker, preporučujemo da ili oblikovanje pogon da biste uklonili sve instance programa BitLocker metapodataka iz pogon ili šifriranje i ponovno šifriranje cijeli pogon.

Tvrtke i ustanove kojih se provode šifriranje podataka veća je vjerojatnost da je izložen problema s integritetom podataka, kao što je zlonamjeran ili rogue korisnicima onemogućuje krađu podataka i ugrožena računi neovlašteni pristup podacima u Očisti oblikovanje. Osim tih rizika tvrtkama koje imaju radi usklađivanja s pravilnicima industrijskih, morate dokazuje oni su uporni i koristite kontrole ispravnu sigurnost da biste poboljšali sigurnost podataka.

Saznajte više o šifriranje Azure tako da pročitate članak [Azure Disk šifriranje za Windows i Linux IaaS VMs](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Korištenje module za sigurnost hardvera

Rješenja za šifriranje industrijskih tipkama tajnu za šifriranje podataka. Stoga je važnosti da ove tipke sigurno spremaju. Upravljanje ključevima postaje sastavni dio zaštita podataka jer će leveraged za pohranu tajnu prečaci koji se koriste za šifriranje podataka.

Azure šifriranje koristi [Azure ključ sigurnog](https://azure.microsoft.com/services/key-vault/) će vam pomoći odrediti i upravljanje ključeva za šifriranje diska i tajne u pretplatu ključa sigurnog uz jamstvo da se svi podaci u diskova virtualnog računala šifrirane na ostale u Azure prostora za pohranu. Koristite Azure ključ sigurnog u nadzornom ključeva i korištenju pravila.

Nema više postojećih rizike vezane uz nemate odgovarajuće sigurnosne kontrole na mjestu da biste zaštitili tajne ključeve korištene šifriranje podataka. Ako attackers ima pristup tajne ključeve, oni će moći dešifrirati podatke i potencijalno pristup na povjerljive informacije.

Dodatne informacije o općenite preporuke za upravljanje certifikatom u Azure tako da pročitate članak [Upravljanje certifikatom u Azure: Što činiti, a](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Dodatne informacije o Azure ključ sigurnog pročitajte [Početak rada s sigurnog ključ Azure](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Upravljanje sigurne radne stanice

Budući da se većina na napada ciljani krajnjeg korisnika, krajnju točku postaje jedne od točaka primarni napada. Ako napadaču compromises krajnju točku, on omogućuje korištenje korisnikove vjerodajnice za pristup podacima tvrtke ili ustanove. Većina krajnjoj točki napada su mogu iskoristiti fact krajnji korisnici su je administratore u svoje lokalne radne stanice.

Pomoću radne stanice sigurne upravljanje možete smanjiti tih rizika. Preporučujemo da koristite [Povlaštene pristup radne stanice (OTISCI)](https://technet.microsoft.com/library/mt634654.aspx) da biste smanjili plošni napada u radne stanice. Ove radne stanice sigurne upravljanje omogućuju prevladavanje nekih od tih napada osigurati se podaci nalaze na. Provjerite je li OTISCI poboljšati i pomoću zaključati vaše radne stanice. Ovo je važnih koraka možete unijeti jamstva visoku razinu sigurnosti za povjerljive poslovne kontakte, zadatke i zaštitu podataka.

Nedostatak Zaštita na krajnjoj točki možda ugroziti podataka, provjerite jeste li pojačati zaštitu sigurnosti preko svih uređaja koje se koriste za podatke, bez obzira na to mjesto podataka (oblaka ili lokalno).

Dodatne informacije o povlaštene radne stanice pristupiti tako da pročitate članak [Zaštita povlaštene pristupa](https://technet.microsoft.com/library/mt631194.aspx).

## <a name="enable-sql-data-encryption"></a>Omogućivanje šifriranje podataka za SQL

[Šifriranje baze podataka SQL Azure prozirne podataka](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) pomaže u zaštiti prijetnju zlonamjerni aktivnosti izvođenjem u stvarnom vremenu šifriranje i dešifriranje baze podataka, pridruženi sigurnosne kopije i datoteke zapisnika transakcije na ostale bez promjene u aplikaciju.  TDE šifrira prostora za pohranu cijelu bazu podataka pomoću ključa simetričnu naziva ključa za šifriranje baze podataka.

Čak i kada cijelu pohranu šifriran, važno je da biste šifriranja baze podataka sam. Ovo je implementacija obrane u dubine pristup za zaštitu podataka. Ako koristite [Baze podataka SQL Azure](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) , a želite zaštititi povjerljive podatke kao što su kreditnom karticom ili broj iskaznice zdravstvenog osiguranja, možete šifriranje baze podataka s FIPS 140-2 provjeriti 256 bitno AES šifriranje koji zadovoljava preduvjete mnogo industrijske standarde (npr., HIPAA, PCI).

Važno da biste shvatili da datoteke vezane uz [proširenje grupe međuspremnik](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) nisu šifrirane kada baze podataka šifriran pomoću TDE je. Poslužite se alatima za šifriranje razine sustava datoteke kao što je BitLocker ili [Šifriranje datotečni sustav](https://technet.microsoft.com/library/cc700811.aspx) (EFS) za BPE povezanih datoteka.

Od ovlaštenog korisnika kao što su administratora sigurnosti ili administratora baze podataka možete pristupiti podacima čak i ako je baza podataka šifrirana s TDE, i slijedite preporuke u nastavku:

- SQL provjera autentičnosti na razini baze podataka
- Azure AD autentičnosti pomoću RBAC uloga
- Korisnicima i aplikacijama trebali biste koristiti odvojene račune za provjeru autentičnosti. Na taj način možete ograničiti dozvole dodijeljene korisnicima i aplikacijama i smanjiti količinu rizika zlonamjerni aktivnosti
- Sigurnost na razini baze podataka Implementacija sadržaja pomoću uloge fixed baze podataka (primjerice db_datareader ili db_datawriter) ili možete stvoriti prilagođene uloge za svoju aplikaciju da biste dodijelili izričite dozvole objekata odabrane baze podataka

Tvrtkama ili ustanovama koje koriste razine šifriranje baze podataka može biti podložnija za napada koje mogu ugroziti podatke koji se nalaze u bazama podataka za SQL.

Saznajte više o SQL TDE šifriranje tako da pročitate članak [Prozirne šifriranje podataka s bazom podataka SQL Azure](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Zaštita podataka na putu

Zaštita podataka na putu mora biti Ključni dio Strategije za zaštitu podataka. Budući da se podaci će Premještanje natrag i naprijed s više mjesta, opće preporuke je uvijek koristite SSL/TLS protokoli za razmjenu podataka preko različitih mjesta. U nekim okolnostima možda želite izdvojiti kanala cijelu komunikacije između lokalnog i oblaka infrastrukture putem virtualne privatne mreže (VPN-a).

Za premještanje između lokalnog Infrastruktura i Azure podatke, razmislite o odgovarajuće zaštitu kao što su HTTPS ili VPN-a.

U tvrtkama ili ustanovama koje su potrebne za siguran pristup s više radne stanice nalazi lokalnog za Azure, koristite [Azure VPN-a web-mjesto](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Za tvrtkama ili ustanovama koje su potrebne za siguran pristup iz jedne radne stanice nalazi lokalnog sustava Azure, korištenje [točke-na-web-mjesta VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Veće podataka postavlja se možete prijeđete preko namjenski velika brzina WAN veze kao što su [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Ako odlučite koristiti ExpressRoute, mogućnost šifriranja podataka na razini aplikacije putem [SSL/TLS](https://support.microsoft.com/kb/257591) i ostali protokoli za dodati zaštitu.

Ako su interakcija s Azure prostora za pohranu putem portala za Azure, sve transakcije će se pojaviti putem HTTP. [REST API -JA za pohranu](https://msdn.microsoft.com/library/azure/dd179355.aspx) putem HTTP može se koristiti i interakciju s [Azure prostora za pohranu](https://azure.microsoft.com/services/storage/) i [Baze podataka SQL Azure](https://azure.microsoft.com/services/sql-database/).

Tvrtkama ili ustanovama koje ne prođu za zaštitu podataka na putu su podložnija za [Muškarac-u-u – srednje](https://technet.microsoft.com/library/gg195821.aspx), [eavesdropping](https://technet.microsoft.com/library/gg195641.aspx) i hijacking sesiju. Tih napada ne smije biti prvi korak u pristupe povjerljivim podacima.

Saznajte više o Azure VPN mogućnost tako da pročitate članak [Planiranje i dizajniranje za pristupnik za VPN-a](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Nametanje šifriranje datoteke razine podataka

Dodatnom sloju zaštite možete povećati razinu sigurnosti za podatke je šifriranja datoteke, bez obzira na to mjesto datoteke.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) koristi pravila šifriranja, identiteta i autorizacije radi zaštite vaše datoteke i e-pošte. Azure RMS funkcionira na više uređaja – telefonima, tabletima i računalima zaštitom unutar tvrtke ili ustanove i izvan tvrtke ili ustanove. Ova mogućnost je moguće jer Azure RMS dodaje razinu zaštite ostaje s podacima, čak i kad ostavlja granice vaše tvrtke ili ustanove.

Kada pomoću servisa Azure RMS zaštiti vaših datoteka, koristite standardni šifriranja s puna podrška [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Kada pod utjecajem Azure RMS za zaštitu podataka, imate jamstvo koji zaštita ostaje s datotekom, čak i ako se ona se kopira prostora za pohranu koji nije u odjeljku kontrolu nad IT, kao što je servis za pohranu u oblaku. Isti se pojavljuje kod datoteka zajedničko korištenje putem e-pošte, zaštićena je kao privitak poruci e-pošte s uputama kako otvoriti zaštićeni privitak.

Prilikom planiranja Azure RMS prihvaćanja preporučujemo vam sljedeće:

- Instalirajte [RMS zajedničkog korištenja aplikacije](https://technet.microsoft.com/library/dn339006.aspx). Aplikacija integrira sa sustavom Office aplikacije instalacijom sustava Office dodatak tako da korisnici mogu jednostavno zaštiti datoteka izravno.
- Konfiguriranje aplikacije i servisi za podršku Azure RMS
- Stvaranje [prilagođene predloške](https://technet.microsoft.com/library/dn642472.aspx) koje odražavaju s poslovnim potrebama. Na primjer: predloška za gornje tajnu podatke koji se primjenjuje na sve gornji tajna povezane poruke e-pošte.

Tvrtkama ili ustanovama koje su loš na zaštita [podataka klasifikaciju](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) i datoteke mogu biti podložnija oštećenja podataka. Bez zaštite odgovarajućem datotečnom tvrtkama ili ustanovama nećete moći nabaviti uvida tvrtke, praćenje za zloupotrebu i spriječili zlonamjerni pristup datotekama.

Saznajte više o Azure RMS tako da pročitate članak [Početak rada s upravljanjem pravima za Azure](https://technet.microsoft.com/library/jj585016.aspx).
