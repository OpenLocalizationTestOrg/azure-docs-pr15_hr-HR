<properties 
   pageTitle="Ažuriranje 8000 StorSimple 0.2 napomene | Microsoft Azure"
   description="Opisuju nove značajke i rješenja, otvorene teme i dostupne zaobilazna rješenja za siječnju 2015 izdanje sustava Microsoft Azure StorSimple (ažuriranje 0,2)."
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
   ms.date="05/16/2016"
   ms.author="v-sharos" />


# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>StorSimple 8000 niz ažuriranje 0.2 napomene - siječnju 2015.

## <a name="overview"></a>Pregled

Sljedeće napomene odredite kritičnih problema otvorena u siječnju 2015 izdanju programa Microsoft Azure StorSimple. Koje sadrže i popis StorSimple softver i opremu ažuriranja uključena u ovom izdanju. Ovo je drugo izdanje nakon verzije StorSimple 8000 niz pustite unesena načelu dostupan u srpnju 2014..
  
To ažuriranje promjena verzija softvera fizički uređaj listopad Update. Ona i dalje biti verzija 6.3.9600.17312. Slika koristi slika virtualnog uređaja promijenio u ovom izdanju. Dakle, sve nove virtualne uređaje stvorene nakon 20/1/2015 vode verzije kao 6.3.9600.17361.  

Pregledajte sljedeće informacije koje se nalaze u napomene za ažuriranje siječnju 2015.

> [AZURE.IMPORTANT]  
>
>- Ažuriranje nije dostupan putem servisa Windows Update i nije moguće instalirati kao što su druga ažuriranja. Vaš uređaj neće primiti ažuriranje čak i ako ste primijenili ažuriranja pomoću portala za Azure klasični. To ažuriranje će se primijeniti samo na virtualne uređaji stvorene nakon 20. siječnju 2015.. 
> 
>- Siječanj izdanje StorSimple ne sadrži sva ažuriranja StorSimple fizički uređaj. I dalje možete primijeniti raspoloživa ažuriranja za Windows virtualnog uređaja, uključujući nedavne sigurnosne popravke, ali nećete vidjeti promjene u verziji za uređaj fizički StorSimple.

## <a name="whats-new-in-the-january-release"></a>Što je novo u siječnju izdanje

To ažuriranje sadrži rješenju problema vezanih uz količine izvanmrežni na virtualnog uređaja. (Pogledajte [riješenog problema u ovom izdanju](#issues-fixed-in-the-january-release)).  

Ažuriranje ne sadrži nove značajke i funkcije.  

## <a name="issues-fixed-in-the-january-release"></a>Problemi s fiksiran siječanj izdanje

U sljedećoj su tablici opisani problem koji je fiksiran to ažuriranje.

|ne.|Značajka|Problem|Odnosi se na fizički uređaj|Odnosi se na virtualnog uređaja 
|---|-------|-----|--------------------------|-------------------------
|1|Količine izvanmrežni|Kada latencies visoke oblaka zadržavaju se za nekoliko minuta, količine virtualnog uređaja StorSimple raditi izvanmrežno na hosts. Taj se popravak povećava praga za latencies oblak, čime ćete minimiziranje situacijama koja može izazvati jedinicama u izvanmrežni na glavno računalo.|ne|Da  

## <a name="known-issues-in-the-january-release"></a>Poznati problemi u siječnju izdanje

Sljedeća tablica daje sažetak Poznati problemi u ovom izdanju.
 
|ne.|Značajka|Problem|Komentari/zaobilazno rješenje|Odnosi se na fizički uređaj|Odnosi se na virtualnog uređaja 
|---|-------|-----|-------------------|--------------------------|-------------------------
|1| Ponovno postavljanje tvorničke|  U nekim slučajevima prilikom izvršavanja tvorničke Vrati izvorne postavke uređaja StorSimple ostaju i prikazati ova poruka: **Vrati tvorničke je u tijeku (faza 8).** To se događa ako pritisnete kombinaciju tipki CTRL + C dok cmdlet je u tijeku.| Pritisnite CTRL + C nakon pokretanja tvorničke Vrati izvorne postavke. Ako se već nalaze u tom stanju, obratite se Microsoft Support za sljedeće korake.|Da|ne|
|2|Kvorum disk| U rijetko slučajevima, ako Većina diskova s prilozima EBOD 8600 uređaja niste povezani rezultira kvorum bez diska pa na resurse za pohranu bit će izvanmrežno. On će ostati izvanmrežne čak i ako na diskova se ponovno povezati sa sustavom.|Morat ćete ponovno pokrenite uređaj. Ako se problem nastavi pojavljivati, obratite se Microsoft Support za sljedeće korake.|Da |ne
|3|Oblak snimku stanja pogreške|U rijetko slučajevima, snimka oblaka možda neće funkcionirati uz poruku o pogrešci **maksimalno ograničenje sigurnosne kopije dosegne**. To se događa ako prelaziti 255 clones online na istom uređaju s istom izvornu glasnoću koji je izbrisan.||Da|Da
|4|ID netočan kontrolera|Zamjena kontroler provodi, kontroler 0 možda prikazuju se kao kontroler 1. Tijekom zamjenski kontroler prilikom učitavanja slike iz čvor ravnopravnih članova ID kontrolera može prikazivati na početku kao kontrolerom ravnopravnih članova ID-a.  U rijetko instancama takvo ponašanje mogu se vidjeti i nakon ponovnog pokretanja sustava.|Potreban je bez korisničku akciju. U ovom slučaju će riješiti sam nakon zamjenski kontroler.|Da|ne 
|5| Uređaj nadzor grafikoni|U servisu StorSimple Upravitelj grafikoni nadzora uređaj ne funkcioniraju kad je osnovni ili NTLM je omogućena provjera autentičnosti u konfiguraciji proxy poslužitelja za uređaj.|Izmjena web-konfiguracija proxy poslužitelja za uređaj registrirani sa servisom za Upravitelj StorSimple stoga autentičnosti postavljena na nema. Da biste to učinili, pokrenite sustava Windows PowerShell za postavljanje HcsWebProxy StorSimple cmdlet.|Da|Da
|6| Računi za pohranu|Putem servisa za pohranu da biste izbrisali račun za pohranu je nepodržane scenarij. To će dovesti do situacije u kojima ne može dohvatiti podatke o korisniku.|| Da |  Da
|7|Prebacivanje na uređaju| Više failovers glasnoću spremnika na istom uređaju izvora različite ciljnih nije podržana.| Prebacivanje s jednog reagira uređaja na više uređaja će postati spremnika glasnoće na prvom nije uspjela putem uređaja izgubiti vlasništvo nad podacima. Nakon takve prebacivanje te spremnika glasnoću će prikazuju se ili se drugačije ponašaju prilikom prikaza na portalu za Azure klasični.|Da|ne
|8| Instalacija|Tijekom StorSimple prilagodnik za instalaciju sustava SharePoint, morate unijeti IP uređaj za instalaciju da biste uspješno završili.||Da|ne
|9| Web proxy|Ako je vaše web-konfiguracija proxy HTTPS kao navedeni protokol, utjecat će vaše komunikacije servisa uređaja i će izvanmrežni uređaj. Podrška za pakete i moguće generirati u postupku troše značajan resursa na uređaju.|Provjerite ima li web-URL proxy HTTP kao navedeni protokol. Da biste pogledali dodatne informacije o tome kako [Konfiguriraj web proxy za svoj uređaj](storsimple-configure-web-proxy.md).|Da |ne
|10|Web proxy|  Ako konfigurirati i omogućite web proxy na uređaju registriranog, zatim morat ćete ponovno pokrenite active kontroler na uređaju.|| Da |ne
|11|Latencija visoke oblaka i visoke/i radno opterećenje|Kada naiđe na uređaju StorSimple kombinaciju vrlo visoka oblaka latencies (redoslijeda sekundi) i visoke/i radno opterećenje, količine uređaj idite u su smanjene stanje i na I/Os možda neće funkcionirati uz poruku o pogrešci "uređaj nije spreman".|Morat ćete ručno ponovno kontrolera uređaja ili ponovno prebacivanje uređaj da biste oporavili u tom slučaju.|Da|ne

## <a name="physical-device-updates-in-the-january-release"></a>Pustite fizički uređaj ažuriranja u u siječnju

To ažuriranje sadrže druge promjene na uređaju StorSimple.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Serijski priložene kontroler SCSI (SAS) i ažuriranja opreme u u siječnju izdanja

Ovo izdanje sadrži sva ažuriranja SCSI (SAS) kontrolerom serijski priložene ili firmver. Ažuriranje upravljačkog programa je u listopadu 2014 izdanje. 

## <a name="virtual-device-updates-in-the-january-release"></a>Pustite virtualnog uređaja ažuriranja u u siječnju

Ovo izdanje sadrži ažurirane sliku za virtualnog uređaja. Virtualna uređaji stvorene nakon 20. siječnju 2015 prikazat će se verzije programa kao 6.3.9600.17361.

 
