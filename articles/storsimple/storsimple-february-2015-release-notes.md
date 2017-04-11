<properties 
   pageTitle="Ažuriranje 8000 StorSimple 0,3 napomene | Microsoft Azure"
   description="Opisuje nove značajke i rješenja, otvorene teme i dostupne zaobilazna rješenja za veljača 2015 izdanje sustava Microsoft Azure StorSimple (ažuriranje 0,3)."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>StorSimple 8000 niz ažuriranje 0,3 napomene - veljača 2015.

## <a name="overview"></a>Pregled

Sljedeće napomene prepoznati kritičnih problema Otvori StorSimple 8000 niz ažuriranje 0,3 u veljači 2015. Koje sadrže i popis StorSimple softver i opremu ažuriranja uključena u ovom izdanju. Ovo je treća izdanje nakon verzije StorSimple 8000 niz pustite unesena načelu dostupan u srpnju 2014..
  
To ažuriranje promjena verzija softvera uređaj Update siječanj. Ona i dalje biti verzija 6.3.9600.17312. Možete potvrditi da je ažuriranje potvrđivanjem datum **Zadnje ažuriranje** instalirano. Ako je datum 2/10/2015 ili noviji, a zatim ažuriranje uspješno je instaliran.  

Preporučujemo traženje i Primjena dostupna ažuriranja odmah nakon instaliranja StorSimple uređaja. Možete uključiti i automatska ažuriranja za preuzimanje i instaliranje ažuriranja visokog prioriteta od Microsofta čim njihova objavljivanja. Dodatne informacije potražite u članku [Ažuriranje uređaju StorSimple](storsimple-update-device.md).  

Pregledajte informacije koje se nalaze u napomenama u prije implementacije ažuriranja u rješenje StorSimple.  

>[AZURE.IMPORTANT]   
>
> - Pomoću upravitelja StorSimple servisa i ne komponente Windows PowerShell za StorSimple instalirajte ažuriranje iz veljače.   
> - Traje otprilike jedan sat da biste instalirali to ažuriranje. Međutim, ako instalirate kumulativnim ažuriranjima, postupak može potrajati 3 sata da biste dovršili.  
> - Izdanje veljače StorSimple ne sadrži sva ažuriranja StorSimple virtualnog uređaja. I dalje možete primijeniti raspoloživa ažuriranja za Windows virtualnog uređaja, uključujući nedavne sigurnosne popravke, ali nećete vidjeti promjene u verziji za virtualnog uređaja.  

Provjerite je li sljedeći preduvjeti zadovoljeni prije no što ažuriranje uređaju StorSimple.  

- Provjerite je li oba kontrolera uređaj koristite prije nego Potraži ažuriranja. Ako neki kontroler nije pokrenut, pregled neće uspjeti. Da biste provjerili na kontrolera u dobar stanju, dođite do **Status hardvera** u odjeljku stranice za **Održavanje** . Ako postoje komponente **treba li vam potrebna**, obratite se Microsoft Support prije nastavka izvući.
- Provjerite je li fiksnim IP-ovi za kontroler 0 i 1 kontroler su usmjeravati i možete povezati s Internetom, kao što je koriste se za održavanje ažuriranja na uređaju. Pomoću [cmdleta Testiraj vezu](https://technet.microsoft.com/library/hh849808.aspx) pomoću naredbe ping poznati adresu izvan mreže, kao što je outlook.com da biste provjerili ima li kontrolerom veza s mjesta izvan mreže.
- Priključke 80 i 443 provjerite jesu li dostupni na uređaju StorSimple za izlazni komunikaciju. Dodatne informacije potražite u članku [preduvjeti za StorSimple uređaj za povezivanje s mrežom](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
- Ako je verzija softvera uređaj starije od 6.3.9600.17312 (ažuriranje iz listopad 2014.), onemogućiti priključke podataka 2 i 3 podataka ako je omogućen, prije početka ažuriranja. Ostavite podataka 2 ili 3 podataka priključke omogućeni prilikom primjene ažuriranja može uzrokovati upravljaču uređaj da biste prešli u način rada za oporavak. Primijetite da kada onemogućite sučelje mreže povezane količine će biti izvanmrežno i nadziranje na I/Os će prekinuto trajanja ažuriranja.  
  
## <a name="whats-new-in-the-february-release"></a>Što je novo u izdanju veljača

To ažuriranje sadrži popravak za problem koji je došlo je do za vraćanje na tvorničke uređaje koje ste u GA nadogradili pustite izdanje listopad 2014.. Dodatne informacije potražite u članku [problemi fiksno u ovom izdanju](#issues-fixed-in-the-february-release).   

To ažuriranje ne sadrži nove značajke i funkcije.  

## <a name="issues-fixed-in-the-february-release"></a>Problemi s fiksiran izdanje veljača

U sljedećoj su tablici opisani problem koji je fiksiran to ažuriranje.  
 
| ne. | Značajka | Problem | Odnosi se na fizički uređaj | Odnosi se na virtualnog uređaja |
|-----|---------|-------|---------------------------------|-------------------------------|
| 1 | Ponovno postavljanje tvorničke | Pokušate izvesti tvorničke vratiti na uređaju koji izvorno imali izdanje GA (verzija 6.3.9600.17215) instaliran, ali je ažuriran u listopadu izdanje (verzija 6.3.9600.17312). Na tvorničke Vrati ne uspije, a uređaj postane nestabilan. | Da | ne |


## <a name="known-issues-in-the-february-release"></a>Poznati problemi u izdanju veljača

Sljedeća tablica daje sažetak Poznati problemi u ovom izdanju.
 
| ne. | Značajka | Problem | Komentari/zaobilazno rješenje | Odnosi se na fizički uređaj  | Odnosi se na virtualnog uređaja |
|-----|---------|-------|----------------------------|-----------------------------|--------------------------|
| 1 | Ponovno postavljanje tvorničke | U nekim slučajevima prilikom izvršavanja tvorničke Vrati izvorne postavke uređaja StorSimple ostaju i prikazati ova poruka: **Vrati tvorničke je u tijeku (faza 8)**. To se događa ako pritisnete kombinaciju tipki CTRL + C dok cmdlet je u tijeku. | Pritisnite CTRL + C nakon pokretanja tvorničke Vrati izvorne postavke. Ako se već nalaze u tom stanju, obratite se Microsoft Support za sljedeće korake. | Da | ne |
| 2 | Kvorum disk | U rijetko slučajevima, ako Većina diskova s prilozima EBOD od programa 8600device niste povezani rezultira kvorum bez diska pa na resurse za pohranu bit će izvanmrežno. On će ostati izvanmrežne čak i ako na diskova se ponovno povezati sa sustavom. | Morat ćete ponovno pokrenite uređaj. Ako se problem nastavi pojavljivati, obratite se Microsoft Support za sljedeće korake. | Da | ne |
| 3 | Oblak snimku stanja pogreške | U rijetko slučajevima, snimka oblaka možda neće funkcionirati uz poruku o pogrešci **maksimalno ograničenje sigurnosne kopije dosegne**. To se događa ako prelaziti 255 clones online na istom uređaju s istom izvornu glasnoću koji je izbrisan. |  | Da | Da |
| 4 | ID netočan kontrolera | Zamjena kontroler provodi, kontroler 0 možda prikazuju se kao kontroler 1. Tijekom zamjenski kontroler prilikom učitavanja slike iz čvor ravnopravnih članova ID kontrolera može prikazivati na početku kao kontrolerom ravnopravnih članova ID-a. U rijetko instancama takvo ponašanje mogu se vidjeti i nakon ponovnog pokretanja sustava. | Potreban je bez korisničku akciju. U ovom slučaju će riješiti sam nakon zamjenski kontroler. | Da | ne |
| 5 | Uređaj nadzor grafikoni | U servisu StorSimple Upravitelj grafikoni nadzora uređaj ne funkcioniraju kad je osnovni ili NTLM je omogućena provjera autentičnosti u konfiguraciji proxy poslužitelja za uređaj. | Izmjena web-konfiguracija proxy poslužitelja za uređaj registrirani sa servisom za Upravitelj StorSimple stoga autentičnosti postavljena na nema. Da biste to učinili, pokrenite sustava Windows PowerShell za postavljanje HcsWebProxy StorSimple cmdlet. | Da | Da |
| 6 | Računi za pohranu | Putem servisa za pohranu da biste izbrisali račun za pohranu je nepodržane scenarij. To će dovesti do situacije u kojima ne može dohvatiti podatke o korisniku. |  | Da | Da |
| 7 | Prebacivanje na uređaju | Više failovers glasnoću spremnika na istom uređaju izvora različite ciljnih nije podržana.  Prebacivanje s jednog reagira uređaja na više uređaja će postati spremnika glasnoće na prvom nije uspjela putem uređaja izgubiti vlasništvo nad podacima. Nakon takve prebacivanje te spremnika glasnoću će prikazuju se ili se drugačije ponašaju prilikom prikaza na portalu za Azure klasični. |   | Da | ne |
| 8 | Instalacija | Tijekom StorSimple prilagodnik za instalaciju sustava SharePoint, morate unijeti IP uređaj za instalaciju da biste uspješno završili. |  | Da | ne |
| 9 | Web proxy | Ako je vaše web-konfiguracija proxy HTTPS kao navedeni protokol, utjecat će vaše komunikacije servisa uređaja i će izvanmrežni uređaj. Podrška za pakete i moguće generirati u postupku troše značajan resursa na uređaju. | Provjerite ima li web-URL proxy HTTP kao navedeni protokol. Dodatne informacije o tome kako [Konfiguriraj web proxy za svoj uređaj](storsimple-configure-web-proxy.md). | Da | ne |
| 10 | Web proxy | Ako konfigurirati i omogućite web proxy na uređaju registriranog, zatim morat ćete ponovno pokrenite active kontroler na uređaju. |  | Da | ne |
| 11 | Latencija visoke oblaka i visoke/i radno opterećenje | Kada naiđe na uređaju StorSimple kombinaciju vrlo visoka oblaka latencies (redoslijeda sekundi) i visoke/i radno opterećenje, količine uređaj idite u su smanjene stanje i na I/Os možda neće funkcionirati uz poruku o pogrešci "uređaj nije spreman". | Morat ćete ručno ponovno kontrolera uređaja ili ponovno prebacivanje uređaj da biste oporavili u tom slučaju. | Da | ne |

## <a name="physical-device-updates-in-the-february-release"></a>Pustite fizički uređaj ažuriranja u u veljači

To ažuriranje rješava problem tvorničke Vrati se pojavio na uređaje koje ste u GA nadogradili pustite izdanje listopad 2014.. Ne sadrži sva ažuriranja StorSimple uređaj.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Serijski priloženom kontroler SCSI (SAS) i ažuriranja opreme u u veljači izdanja

Ovo izdanje sadrži sva ažuriranja SCSI (SAS) kontrolerom serijski priložene ili firmver. Ažuriranje upravljačkog programa je u listopadu 2014 izdanje.  

## <a name="virtual-device-updates-in-the-february-release"></a>Pustite virtualnog uređaja ažuriranja u u veljači

Ovo izdanje sadrži sva ažuriranja za virtualnog uređaja. Primjena to ažuriranje neće se promijeniti softver verziju virtualnog uređaja.
 
