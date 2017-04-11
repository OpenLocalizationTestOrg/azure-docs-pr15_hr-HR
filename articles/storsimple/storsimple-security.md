<properties 
   pageTitle="Sigurnost StorSimple | Microsoft Azure" 
   description="U članku se opisuje značajke sigurnosti i privatnosti koji zaštita StorSimple servisa, uređaj i podataka lokalno i u oblaku." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="05/03/2016"
   ms.author="v-sharos"/>

# <a name="storsimple-security-and-data-protection"></a>Zaštita StorSimple sigurnost i podataka

## <a name="overview"></a>Pregled

Sigurnost nije važna svakome tko je usvojio novu tehnologiju, osobito kada tehnologija koristi uz povjerljivih ili vlasničkih podataka. Kao što je procijenili različite tehnologije, morate uzeti u obzir povećana rizike i troškove za zaštitu podataka. Microsoft Azure StorSimple nudi sigurnosti i privatnosti rješenja za zaštitu podataka osiguravanje: 

- **Povjerljivosti** – samo ovlašteni entiteti možete vidjeti vaše podatke. 
- **Integritet** – samo ovlašteni entiteti možete izmijeniti ili izbrisati podatke.

Rješenja za Microsoft Azure StorSimple sastoji se od četiri glavna komponente koje interakciju jedna s drugom:

- **Servis upravitelja StorSimple smješten u Microsoft Azure** – servisa za upravljanje koju koristite za konfiguriranje i dodjela StorSimple uređaja.
- **Uređaj StorSimple** – fizičke uređaj instaliran na vašem podatkovnog centra. Sve domaćini i klijenti za generiranje podataka koji se povezati s uređajem StorSimple, a uređaj upravlja podatke i premješta Azure oblak sukladno situaciji.
- **Klijenti/domaćini povezani s uređajem** – klijente u sustavu povezati s uređajem StorSimple i generiranje podataka koje je potrebno zaštititi.
- **Za pohranu u oblaku** – mjesto u oblak Azure pohrane podataka.

U sljedećim odjeljcima opisuju značajke sigurnosti StorSimple zaštitu svaki od tih komponenti i podatke pohranjene na njima. Sadrži popis pitanja koja možda imate o sigurnosti Microsoft Azure StorSimple i odgovarajuće odgovore.

## <a name="storsimple-manager-service-protection"></a>Zaštita StorSimple Upravitelj servisa

Upravitelj StorSimple servis je servisa za upravljanje smješten u Microsoft Azure, a služe za upravljanje uređajima s sve StorSimple sadrži nabavljaju tvrtke ili ustanove. Servis StorSimple Manager možete pristupiti pomoću vaše tvrtke ili ustanove vjerodajnice za prijavu na portalu Azure klasični putem web-preglednika. 

Pristup servisu StorSimple Upravitelj zahtijeva vašoj tvrtki ili ustanovi Azure pretplatu koja obuhvaća StorSimple. Vaša pretplata određuje značajke kojima možete pristupiti na portalu za Azure klasični. Ako vaša tvrtka ima Azure pretplate i želite saznati više o njima, potražite u članku [registrirati za Azure kao tvrtkom ili ustanovom](../active-directory/sign-up-organization.md). 

Budući da servis za upravitelja StorSimple nalazi se u Azure, je zaštićena upravljanjem Azure sigurnosne značajke. Dodatne informacije o sigurnosnim značajkama nudi Microsoft Azure, otvorite [Centar za pouzdanost Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>Zaštita StorSimple uređaja

Uređaj StorSimple je lokalnog hibridnog prostora za pohranu uređaj koji sadrži pune stanje pogona (SSDs) i tvrdom diskovnih pogona (HDDs), zajedno s suvišnih kontrolera i automatsko prebacivanje mogućnosti. Na kontrolera Upravljanje prostorom za pohranu tiering, smještanje trenutno korištenih (ili tipkovni) podataka na lokalno spremište (u odjeljku StorSimple uređaja ili lokalne poslužitelje), tijekom premještanja manje često korištenih podataka s oblakom.

Samo ovlašteni StorSimple uređaji dopušteno da biste pristupili servisu StorSimple Manager koju ste stvorili u pretplatu za Azure. Da biste autorizirali uređaj, morate se registrirati ga sa servisom StorSimple Upravitelj unosom ključa za registraciju servisa. Ključa za registraciju servis je 128-bitno slučajni ključ generira Azure klasični portalu. 

![Ključa za registraciju](./media/storsimple-security/ServiceRegistrationKey.png)

Da biste saznali kako ključ usluga registracije, idite na [Korak 2: Registracija ključ servis](storsimple-deployment-walkthrough.md#step-2-get-the-service-registration-key).

Ključa za registraciju servis je dugo ključ koji sadrži 100 + znakove. Možete kopirati tipku i spremite ga u tekst datoteka na sigurnom mjestu tako da ga možete koristiti da biste autorizirali dodatnih uređaja prema potrebi. Ako ključa za registraciju servisa gubi se nakon registriranja prvom uređaju, možete stvoriti novi ključ StorSimple Upravitelj servisa. To neće utjecati na operaciju postojećih uređaja. 

Kada je registrirana na uređaju, koristi tokeni možete komunicirati s Microsoft Azure. Nakon Registracija uređaja ne koristi ključa za registraciju servisa.

> [AZURE.NOTE] Preporučujemo da nakon svakog korištenja Obnovi ključa za registraciju servisa.

## <a name="protect-your-storsimple-solution-via-passwords"></a>Zaštita rješenje StorSimple putem lozinke

Lozinke su važne aspekt sigurnost računala i se često koriste u rješenje StorSimple kako biste bili sigurni da je dostupna samo ovlašteni korisnici podataka. StorSimple omogućuje vam da biste konfigurirali sljedeće lozinke:

- StorSimple uređaj administratorsku lozinku
- Izazov rukovanja provjere autentičnosti protokolom (CHAP) pokretač i ciljne lozinki
- Upravitelj snimka StorSimple lozinke

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>Windows PowerShell za StorSimple i StorSimple administratorsku lozinku za uređaj

Windows PowerShell za StorSimple je sučelje naredbenog retka koje možete koristiti da biste upravljali StorSimple uređaja. Windows PowerShell za StorSimple sadrži značajke koje omogućuju da biste registrirali uređaj, konfiguriranje mrežnog sučelja na uređaju, instalirajte određene vrste ažuriranja, rješavanje problema s uređajem tako da pristupite sesiju podršku te promijenite stanje uređaja. Možete pristupiti komponente Windows PowerShell za StorSimple povezivanjem s konzole za serijski na uređaju, ili pomoću komponente Windows PowerShell remoting.

PowerShell remoting mogu se izvršiti putem HTTP ili HTTP-a. Ako je omogućeno daljinsko upravljanje putem HTTP, morat ćete Preuzmite certifikat za daljinsko upravljanje s uređaja i instalirajte ga na udaljenom klijenta. Dodatne informacije o PowerShell remoting otvorite [Povezivanje daljinski s uređajem StorSimple](storsimple-remote-connect.md).

Kada pomoću komponente Windows PowerShell za StorSimple povezati s uređajem, morat ćete unijeti uređaj administratorsku lozinku za prijavu na uređaju.

![Uređaj administratorsku lozinku](./media/storsimple-security/DeviceAdminPW.png)

Sljedeće najbolje prakse Imajte na umu:

- Po zadanom je isključeno daljinskog upravljanja. Da biste omogućili, poslužite se StorSimple Upravitelj servisa. Kao sigurnost praksa, daljinski pristup trebala bi biti omogućena samo tijekom vremenskog razdoblja koje je doista potreban.
- Ako promijenite lozinku, ne zaboravite obavijesti sve korisnike za daljinski pristup tako da se neće doći do gubitka neočekivane povezivanje.
- Servis StorSimple Manager nije moguće dohvatiti postojeće lozinke: ga možete vratiti samo odabranih ih. Preporučujemo da spremite sve lozinke na sigurnom mjestu tako da ne morate ponovno postavljanje lozinke ako ga je zaboravili. Ako je potrebno ponovno postavljanje lozinke, obavezno obavijestiti sve korisnike prije nego što vam je ponovno postaviti. 

Sučelje za Windows PowerShell možete pristupiti pomoću serijskog veze na uređaju. Možete i joj pristupiti daljinski putem HTTP ili HTTPS, čime se omogućuje dodatne sigurnosti. HTTPS nudi višu razinu sigurnosti od serijski ili HTTP vezu. Međutim, da biste koristili HTTPS, najprije morate instalirati certifikat na klijentskom računalu koja će imati pristup uređaj. Certifikat za daljinski pristup možete preuzeti iz stranicu za konfiguriranje uređaja u StorSimple Upravitelj servisa. Ako je certifikata za daljinski pristup izgubiti, morate preuzeti novi certifikat i prenijeti na sve klijente koji imaju dozvolu za korištenje daljinsko upravljanje.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Izazov rukovanja provjeru autentičnosti protokolom (CHAP) pokretač i odredišno lozinki

CHAP je shema za provjeru autentičnosti koji se koristi uređaj StorSimple Provjera identiteta udaljenim klijentima. Provjera temelji se na zajedničkom lozinku. CHAP može biti jednosmjerna (unidirectional) ili zajednički (dvosmjerni). S jednosmjerna CHAP cilj (StorSimple uređaj) potvrđuje pokretač (glavno računalo). Međusobna ili obrnutim CHAP potreban je cilj autentičnost pokretač, a zatim pokretač autentičnost cilj. Da biste koristili neke od ovih metoda moguće je konfigurirati svoje StorSimple.

Imajte na umu sljedeće prilikom konfiguriranja CHAP:

- CHAP korisničko ime mora sadržavati manje od 233 znakova.
- Lozinka CHAP mora biti između 12 i 16 znakova. Pokušaj koristi više korisničko ime ili lozinku, rezultirat će neuspješne provjere autentičnosti na glavnom računalu za Windows.
- Ne možete koristiti istu lozinku za pokretač CHAP i CHAP cilj.
- Nakon što postavite lozinku, mogu se mijenjati, ali se ne može vratiti. Ako se promijeni lozinku, ne zaboravite obavijesti sve korisnike za daljinski pristup tako da mogu uspješno povezati s uređajem StorSimple.

Dodatne informacije o CHAP i kako konfigurirati za rješenje StorSimple otvorite [Konfiguriranje CHAP za svoj uređaj StorSimple](storsimple-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>Upravitelj snimka StorSimple lozinke

Upravitelj snimka StorSimple je Microsoft Management Console (BLOG) dodatku koji koristi glasnoću grupe i servis za kopiranje Windows glasnoću sjene za generiranje aplikacije dosljedan sigurnosne kopije. Osim toga, možete koristiti StorSimple snimke upravitelja za stvaranje sigurnosne kopije rasporede i Kloniraj ili vraćanje količine.

Kada konfigurirate uređaj da biste pomoću upravitelja snimka StorSimple će ćete morati StorSimple snimke Upravitelj lozinku. Ovu lozinku je najprije postaviti u ljusci Windows PowerShell za StorSimple tijekom registracije. Lozinku možete postaviti i promijeniti iz StorSimple Upravitelj servisa. Ovu lozinku potvrđuje s StorSimple snimku Upravitelj uređaja.

![Upravitelj snimka StorSimple lozinke](./media/storsimple-security/SnapshotMgrPassword.png)

Lozinka StorSimple snimke upravitelj mora biti 14 do 15 znakova i mora sadržavati 3 ili više od kombinacije velika slova, mala slova, numeričke i posebnih znakova. Nakon što postavite lozinku Upravitelj StorSimple snimke, mogu se mijenjati, ali se ne može vratiti. Ako promijenite lozinku, ne zaboravite obavijesti sve udaljene korisnike.

Dodatne informacije o upravitelju za StorSimple snimke, idite na [što je upravitelj StorSimple snimke?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Najbolje prakse za lozinke

Preporučujemo da kako biste bili sigurni da su lozinke StorSimple istaknuti i dobro zaštićenim pridržavajte se sljedećih smjernica:

- Promijeniti svoje lozinke svaka tri mjeseca. Promjena lozinke Poboljšana je jedanput godišnje.
- Koristite neprobojne lozinke. Dodatne informacije potražite u članku [Stvaranje neprobojne lozinke](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/)i Zaštitno.
- Uvijek koristi različite lozinke za različite pristup mehanizme; svaki od lozinke koju navedete mora biti jedinstvena.
- Zajednički ne koriste lozinke sa svima koji je ovlašten za pristup StorSimple uređaja.
- Ne govori o lozinke ispred drugima ili podsjetnik oblik lozinku.
- Ako mislite da je račun ili lozinka ugrožena, incident dojavite odjelu za sigurnost podataka.
- Sve lozinke Smatraj osjetljive, povjerljive informacije. 

## <a name="storsimple-data-protection"></a>Zaštita podataka StorSimple

U ovom se odjeljku opisuju značajke sigurnosti StorSimple koji zaštiti podataka na putu i pohranjene podatke.

Kao što je opisano u ostale sekcije lozinke koriste se da biste autorizirali i provjeru autentičnosti korisnika prije nego što se može ostvariti pristup StorSimple rješenje. Drugi razmotriti sigurnost zaštita podataka od neovlaštenog korisnika dok se prenosi između sustavi za pohranu i dok se ne nalaze. U sljedećim odjeljcima opisuju značajke zaštite podataka koji ste dobili uz StorSimple.

> [AZURE.NOTE] Poništavanje duplikacije pruža dodatne zaštite podataka pohranjen na uređaju StorSimple i Microsoft Azure prostora za pohranu. Kada je deduplicated podataka, objekti podataka spremaju se zasebno od metapodataka za mapiranje i im pristupati: ne postoji dostupno razina pohrane kontekst da biste ponovno sastavit podataka na temelju glasnoću strukturu, datotečnog sustava ili naziv datoteke.

## <a name="protect-data-flowing-through-the-service"></a>Zaštita podataka slijedi pomoću usluge

Glavna svrha StorSimple Upravitelj servis je za upravljanje i konfiguriranje StorSimple uređaja. Servis za upravitelja StorSimple pokreće se u Microsoft Azure. Pomoću portala za Azure klasični za unos podataka konfiguracije uređaja i zatim Microsoft Azure koristi StorSimple Upravitelj servisa za slanje podataka na uređaju. StorSimple koristi sustav parova asimetričnim ključeva kako biste bili sigurni da narušena pouzdanost servisa Azure neće rezultirati narušena pouzdanost pohranjene podataka. 

![Šifriranje podataka u letovima](./media/storsimple-security/DataEncryption.png)

Asimetričnim ključ sustava pomaže u zaštiti podataka koji se piše pomoću usluge na sljedeći način:

1. Certifikat za šifriranje podataka koji koristi asimetričnim javne i privatne ključa par se generira na uređaju te se koristi za zaštitu podataka. Tipke generiraju kada je registrirana kao prvi uređaj. 
2. Ključevi certifikata za šifriranje podataka izvoze se Razmjena osobnih podataka (.pfx) u datoteku koja je zaštićena servisa podataka ključa za šifriranje, koja je neprobojne 128-bitno tipke koje slučajno se generira prema prvom uređaju tijekom registracije.
3. Javni ključ certifikata postane sigurno dostupna na servis za Upravitelj StorSimple i privatni ključ ostaje s uređajem.
4. Podaci unos servis je šifriran pomoću javnog ključa i dešifriranu pomoću privatni ključ koji je pohranjen na uređaju osiguravanje servisa Azure ne može dešifrirati podataka slijedi na uređaju.

Ključ za šifriranje podataka servisa generira na samo prvi uređaj registriran u servisu. Sve sljedeće uređaje registrirane sa servisom morate koristiti istu servisa podataka ključa za šifriranje. 

> [AZURE.IMPORTANT] 
> 
> Vrlo je važno kopiju ključa za šifriranje podataka servisa i spremite ga na sigurnom mjestu. Kopija ključa za šifriranje podataka servisa bi se trebale nalaziti tako da ga ovlašteni osobe mogu pristupiti i možete jednostavno prenosi za administratora uređaja.
>
> Ako nestaje ključa za šifriranje podataka servisa osoba iz Microsoftove podrške omogućuju dohvaćanje pod uvjetom da imate barem jedan uređaj u mrežni rad. Preporučujemo da Promjena ključa za šifriranje servisa podataka kada se preuzimaju se. Upute za prijeđite na odjeljak [Promjena ključa za šifriranje podataka servisa](storsimple-service-dashboard.md#change-the-service-data-encryption-key).

Ključa za šifriranje podataka servisa i odgovarajuće certifikata za šifriranje podataka možete promijeniti tako da odaberete mogućnost **Promjena ključa za šifriranje podataka servisa** na nadzornoj ploči za servis. Da biste bili sigurni da je sigurnost podataka ne ugrožena, morate koristiti fizički StorSimple uređaj da biste promijenili ključa za šifriranje podataka servisa. Izmjena ključeva za šifriranje traži da se svi uređaji ažurirati novim ključem. Stoga preporučujemo da Promjena ključa svih uređaja koji su na mreži. Uređaji povezani s Internetom ključ mogu se promijeniti u različitim vrijeme. Uređaje s tipkama zastarjele i dalje moći pokrenuti sigurnosne kopije, ali ih nećete moći vratiti podatke dok se ne ažurira tipku. Dodatne informacije, idite na članak [Korištenje nadzorna ploča za StorSimple Upravitelj servisa](storsimple-service-dashboard.md).

Ključ za šifriranje podataka servisa i certifikata za šifriranje podataka isteći. No preporučujemo da promijenite ključa za šifriranje podataka servisa godišnje da biste spriječili narušena pouzdanost ključa.

## <a name="protect-data-at-rest"></a>Zaštita podataka na ostale

Uređaj StorSimple upravlja podatke spremanjem razine lokalno i u oblaku, ovisno o učestalosti korištenja. Svim računalima glavnog računala koji su povezani s uređajem slati podatke s uređajem koji prelazi podataka u oblaku, po potrebi. Podaci se prenose s uređaja s oblakom sigurno putem Interneta. Svakom uređaju sadrži jedan odredište iSCSI koji surfaces sve zajedničke jedinice na tom uređaju. Svi podaci šifriran prije slanja za pohranu u oblaku. 

![Oblak ključa za šifriranje prostora za pohranu](./media/storsimple-security/CloudStorageEncryption.png)

Da biste sigurnost i integritet podataka premještena u oblak, StorSimple omogućuje vam da biste definirali ključeva za šifriranje oblaka za pohranu na sljedeći način:

- Navedite ključa za šifriranje oblaka za pohranu prilikom stvaranja spremniku glasnoću. Ključ nije moguće mijenjati ili dodati kasnije. 
- Sve jedinice u spremniku glasnoću zajednički koristiti iste ključa za šifriranje. Ako želite nekog drugog obrasca šifriranja za određene jedinicu, preporučujemo da stvorite novi spremnik jedinica za hostiranje tu jedinicu.
- Prilikom unosa ključa za šifriranje oblaka za pohranu na servisu StorSimple upravitelj, ključ je šifriran pomoću javnog dio ključa za šifriranje podataka servisa i šalje na uređaj.
- Ključ za šifriranje oblaka za pohranu nije pohranjena bilo gdje u servisu i poznato je samo na uređaj.
- Određivanje ključa za šifriranje prostora za pohranu u oblaku nije obavezno. Možete poslati podatke koje je kodirana na glavnom računalu s uređajem.

### <a name="additional-security-best-practices"></a>Najbolje prakse za dodatne sigurnosti

- Podjela promet: izdvajanja vaše iSCSI SAN od korisnika promet na tvrtke LAN implementacija u cijelosti zarezom mreži i korištenjem VLANs gdje fizičke odvajanja nije dostupna. Namjenski mreža za pohranu iSCSI će jamči sigurnost i performanse tvrtke važnih podataka. Miješanje promet za pohranu i korisnika putem tvrtke LAN ne preporučuje se i možete povećati Latencija i uzrokovati pogreške mreže.

- Da biste postigli Sigurnost mrežne drugo glavno računalo koristite sučelje mreže s podrškom za TCP/IP Offload modul (TOE). TOE smanjuje opterećenje procesora za obradu TCP na mrežnog prilagodnika.

## <a name="protect-data-via-storage-accounts"></a>Zaštita podataka putem računa za pohranu

Microsoft Azure pretplate možete stvoriti jednu ili više prostora za pohranu račune. (S računom za pohranu omogućuje jedinstveni prostor za naziv za rad s podacima pohranjen u oblaku Azure.) Pristup računu za pohranu upravlja tipki pretplate i pristup povezanim s tim računom za pohranu. 

Kada stvorite račun za pohranu, Microsoft Azure stvara dva 512-bitni prostora za pohranu tipke za pristup, koja se koristi za provjeru autentičnosti kad uređaj StorSimple pristupa računu za pohranu. Imajte na umu da samo jedna od tih ključeva koristi. Ostale tipke čuva se rezervnih, što vam omogućuje da povremeno rotiranje tipki. Da biste zakrenuli tipke, aktiviranje sekundarne ključ, a zatim izbrišite primarni ključ. Zatim stvorite novi ključ za korištenje tijekom sljedećeg rotacije. (Sigurnosnih vam razloga mnogo podatkovnim centrima potreban ključ zakretanja.) 

Preporučujemo da slijedite ove najbolje prakse za ključne rotaciju:

- Trebali biste rotirati tipke račun za pohranu redovito da biste bili sigurni da vaš račun za pohranu ne pristupa neovlaštenih.
- Azure administrator povremeno, trebali biste promijeniti ili Obnovi primarni ili sekundarni ključ pomoću odjeljka prostora za pohranu Azure klasični portal za izravno pristup računu za pohranu.


## <a name="protect-data-via-encryption"></a>Zaštita podataka putem šifriranje

StorSimple koristi sljedeće algoritama šifriranja za zaštitu podataka pohranjenih u ili putovanje između komponenti StorSimple rješenje.

| Algoritam | Duljina ključa | Aplikacija/protokoli/komentara |
| --------- | ---------- | ------------------------------- |
| RSA       | 2048       | RSA PKCS 1 v1.5 koristi Azure klasični portala za šifriranje konfiguracijskih podataka koji se šalju na uređaju:, na primjer, za pohranu vjerodajnica, StorSimple uređaj konfiguracija računa i ključeva za šifriranje prostora za pohranu u oblaku. |
| AES       | 256        | AES s CBC koristi se za šifriranje javno dio ključa za šifriranje podataka servisa prije slanja Azure klasični portal s uređaja StorSimple. Također koristi se StorSimple uređaj za šifriranje podataka prije slanja podataka na račun za pohranu oblaka. |


## <a name="storsimple-virtual-device-security"></a>Sigurnost StorSimple virtualnog uređaja

[AZURE.INCLUDE [storsimple virtual device security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Najčešća pitanja

Slijede neki pitanja i odgovori o sigurnosti i Microsoft Azure StorSimple.

**Q:** Moj servis ugrožena. Što treba Moj daljnji koraci?

**A:** Trebali biste odmah Promjena ključa za šifriranje podataka servisa i tipki račun za pohranu za račun za pohranu koji se koristi za tiering podataka. Upute potražite u članku da biste: 

- [Promjena ključa za šifriranje podataka usluge](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Ključni zakretanje račune za pohranu](storsimple-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**Q:** Imam novi uređaj StorSimple koji se traži ključa za registraciju servisa. Kako dohvatiti ga?

**A:** Ovaj ključ stvoren prilikom stvaranja StorSimple Upravitelj servisa. Kada koristite Upravitelj StorSimple servisa za povezivanje s uređajem, poslužite se brzo početnu stranicu servisa da biste pogledali ili Obnovi ključa za registraciju servisa. Stvaranje novog ključa registraciju za servis ne utječe na postojeće registrirani uređaja. Upute potražite u članku da biste:

- [Prikaz ili Obnovi ključa za registraciju servisa](storsimple-service-dashboard.md#view-or-regenerate-the-service-registration-key)

**Q:** U slučaju gubitka ključ za šifriranje podataka servisa. Što učiniti?

**A:** Obratite se Microsoftovoj podršci. Oni mogu prijaviti u sesiju podršci na uređaju i pomoć za dohvaćanje tipku (pod uvjetom da je barem jedan uređaj na Internetu). Odmah nakon što nabavite ključa za šifriranje podataka servisa, promijenite ga da biste bili sigurni da novi ključ je poznato samo vama. Upute potražite u članku da biste:

- [Promjena ključa za šifriranje podataka usluge](storsimple-service-dashboard.md#change-the-service-data-encryption-key)

**Q:**  Li ovlašteni uređaj za servis ključa promjenu podataka šifriranja, ali nije pokrenut proces promjeni ključa. Što da radim?

**A:** Ako je istekla isteka razdoblja, morat ćete reauthorize uređaja za servis ključa promjena podataka šifriranje i ponovno pokrenite postupak.

**Q:**  Nakon promjene ključa za šifriranje servisa podataka, ali se nije moguće ažurirati druge uređaje unutar četiri sata. Imati početi ispočetka?

**A:** 4-satnom vremensko razdoblje je samo za pokretanje promjene. Nakon pokretanja postupka ažuriranja na uređaju ovlašteni StorSimple, autorizacija vrijedi dok se ne ažuriraju se svim uređajima.

**Q:** Naš StorSimple administrator je otišao tvrtke. Što da radim?

**A:** Promjena i ponovno postavljanje lozinki koje omogućuju pristup StorSimple uređaja i promjena ključa za šifriranje podataka servisa da biste bili sigurni novim podacima nije poznat neovlašteno osoblju. Upute potražite u članku da biste:

- [Korištenje upravitelja StorSimple servisa da biste promijenili svoje storsimple lozinke](storsimple-change-passwords.md)
- [Promjena ključa za šifriranje podataka usluge](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Konfiguriranje CHAP za svoj uređaj StorSimple](storsimple-configure-chap.md)

**Q:** Želim upišete lozinku StorSimple snimke Upravitelj na glavno računalo na koje se povezuje s uređajem StorSimple, ali lozinku nije dostupna. Što mogu učiniti?

**A:** Ako ste zaboravili lozinku, potrebno je stvoriti novi. Zatim, pripazite da obavijestite sve postojeće korisnike lozinka je promijenjena i trebali biste da ažurira svoje klijente za korištenje s novom lozinkom. Upute potražite u članku da biste:

- [Promjena lozinke za Upravitelj StorSimple snimke](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password)
- [Autentičnost uređaja](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**Q:** Certifikat za daljinski pristup servisu Windows PowerShell za StorSimple promijenjen je na uređaju. Kako se ažurira svoje klijente za daljinski pristup?

**A:** Možete preuzeti novi certifikat StorSimple Upravitelj servisa i pošaljite ga instalirati u spremištu certifikata klijente daljinski pristup. Upute potražite u članku da biste:

- [Cmdlet uvoz certifikata](https://technet.microsoft.com/library/hh848630.aspx)

**Q:** If zaštićeni moje podatke je upravitelj StorSimple ugrožena servisa?

**A:** Servis za konfiguraciju podataka uvijek je šifriran s javnim ključem prilikom prikaza u web-pregledniku. Jer je servis nema pristup privatni ključ, servis neće moći vidjeti podatke. Servis za upravitelja StorSimple ugrožena, postoji li bez utjecaja kao nema ključeva pohranjene na servisu StorSimple Manager.

**Q:** Ako netko poboljšava pristup certifikata za šifriranje podataka, će Moji podaci biti ugrožena?

**A:** Microsoft Azure pohranjuje ključ šifriranja kupca data (.pfx datoteka) u šifriranom obliku. Budući da .pfx datoteka je šifrirana i servis za StorSimple nema ključa za šifriranje podataka servisa dešifrirati .pfx datoteka, jednostavno početak pristup .pfx datoteka ne Izloži sve tajne.

**Q:** Što se događa ako vladine entitet traži Microsoft moje podatke?

**A:** Jer se svi podaci šifriran na servis i privatni ključ čuva se s uređajem, vladine entitet morate zatražite kupca podatke. 

## <a name="next-steps"></a>Daljnji koraci

[Uvođenje StorSimple uređaj](storsimple-deployment-walkthrough.md).
 
