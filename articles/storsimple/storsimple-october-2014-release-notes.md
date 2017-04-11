<properties 
    pageTitle="Ažuriranje 8000 StorSimple 0,1 napomene | Microsoft Azure"
    description="Opisuju nove značajke i rješenja, otvorene teme i dostupne zaobilazna rješenja za listopad 2014 izdanje sustava Microsoft Azure StorSimple (ažuriranje 0,1)."
    services="storsimple"
    documentationCenter="NA"
    authors="alkohli"
    manager="carmonm"
    editor="" />
 <tags 
    ms.service="storsimple"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="TBD"
    ms.date="09/21/2016"
    ms.author="alkohli" />

# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>StorSimple 8000 niz ažuriranje 0,1 napomene – listopad 2014.  

## <a name="overview"></a>Pregled

Sljedeće napomene prepoznati kritičnih problema Otvori StorSimple 8000 niz ažuriranje 0,1 u listopadu 2014.. Koje sadrže i popis StorSimple softver i opremu ažuriranja uključena u ovom izdanju. To je prvo izdanje nakon verzije StorSimple 8000 niz pustite unesena načelu dostupan u srpnju 2014 i odgovara verzija softvera 6.3.9600.17312.  

Preporučujemo traženje i Primjena dostupna ažuriranja odmah nakon što instalirate uređaj. Možete uključiti i automatska ažuriranja za preuzimanje i instaliranje ažuriranja visokog prioriteta od Microsofta čim njihova objavljivanja. Dodatne informacije potražite u članku kako [ažurirati uređaju StorSimple](storsimple-update-device.md).  

Pregledajte informacije koje se nalaze u napomenama u prije implementacije ažuriranja u rješenje StorSimple.  

>[AZURE.IMPORTANT]
> 
-   Poslužite se servis upravitelja StorSimple i ne komponente Windows PowerShell za StorSimple za instalaciju ažuriranja listopad.  
-   Ažuriranja najčešće su potrebni 3 sata da biste dovršili.  
-   Listopad izdanje StorSimple ne sadrži sva ažuriranja StorSimple virtualnog uređaja. Uvijek možete primijeniti dostupna ažuriranja sustava Windows, uključujući nedavne sigurnosne popravke, ali nećete vidjeti promjene u verziji za virtualnog uređaja.  

Provjerite je li sljedeći preduvjeti su ispunjeni prije no što ažuriranje StorSimple uređaj.  

- Provjerite je li oba kontrolera uređaj koristite prije nego Potraži ažuriranja. Ako neki kontroler nije pokrenut, pregled neće uspjeti. Da biste provjerili na kontrolera u dobar stanju, dođite do **Status hardvera** u odjeljku stranice za **Održavanje** . Ako postoje komponente **treba li vam potrebna**, obratite se Microsoft Support prije nastavka izvući.  
- Provjerite je li fiksnim IP-ovi za oba kontroler 0 i 1, kontroler su usmjeravati i možete povezati s Internetom, kao što je koriste se za održavanje ažuriranja na uređaju. Pomoću [cmdleta Testiraj vezu](https://technet.microsoft.com/library/hh849808.aspx) pomoću naredbe ping poznati adresu izvan mreže kao što je outlook.com da biste provjerili ima li kontrolerom veza s mjesta izvan mreže.  
- Provjerite je li potreban izlazni priključci dostupnih na uređaju StorSimple za izlazni komunikaciju. Dodatne informacije potražite u članku [preduvjeti za StorSimple uređaj za povezivanje s mrežom](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
- Ako je verzija softvera uređaj starije od 6.3.9600.17312 (ažuriranje iz listopad 2014.), onemogućiti priključke podataka 2 i 3 podataka ako je omogućen, prije početka ažuriranja. Ako ostavite podataka 2 ili 3 podataka priključke omogućeni prilikom primjene ažuriranja, može uzrokovati upravljaču uređaj da biste prešli u način rada za oporavak. Primijetite da kada onemogućite sučelje mreže povezane količine će biti izvanmrežno i nadziranje u/i će prekinuto trajanja ažuriranja.  

## <a name="whats-new-in-the-october-release"></a>Što je novo u izdanju listopad

To ažuriranje obuhvaća sljedeća poboljšanja:

- Sada možete koristiti servis upravitelja StorSimple korisničko Sučelje za upravljanje kontrolera vaš uređaj. Upravljanje akcije uključuju ponovno pokrenite isključivanja, ili uključite kontroler. Dodatne informacije potražite na [kontrolera upravljanje StorSimple uređaja](storsimple-manage-device-controller.md).  
- Možete planirati WAN propusnosti dodijeljeni prema kombinacije dan-u-tjednu i vrijeme dan. Omogućuje bolje iskoristiti WAN propusnosti najfrekventnija. Predlošci različite propusnosti su dopuštene spremnika drugu jedinicu. Dodatne informacije potražite na [Upravljanje StorSimple propusnosti predloške](storsimple-manage-bandwidth-templates.md).  
- Možete konfigurirati obavijesti e-poštom doći će obavijestiti u administrator(s) i drugima postojeće ili vjerojatno nadolazeće problema. Dodatne informacije potražite za [Konfiguriranje postavki za upozorenja](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-the-october-release"></a>Problemi s fiksiran listopad izdanje


Sljedeća tablica sadrži sažetak problema koje je fiksnih to ažuriranje.  

| ne. | Značajka | Problem | Odnosi se na fizički uređaj | Odnosi se na virtualnog uređaja |
|-----|---------|-------|---------------------------------|--------------------------------|
| 1 | Sučelje mreže | U prethodnog izdanja, sa softverom su zamjenjuju sučelje mreže podataka 2 i 3 podataka. To je riješen u to ažuriranje. Isključite postavke, a zatim onemogućite ove mreže sučelja prije instalacije ažuriranja. Nakon instalacije ažuriranja, morat ćete ponovno konfigurira ta sučelja. | Da | ne |
| 2 | Paket za podršku | U prethodnog izdanja, ako ste pokrenuli cmdlet komponente Windows PowerShell **Izvoz HcsSupportPackage** za dohvaćanje zapisnika Baseboard upravljanja kontroleru (BMC) postupak nije uspio uz sljedeću poruku: "operacija je uspjela na ovom kontroleru, ali failed on kontrolerom ravnopravnih članova zbog sljedećih pogrešaka. Provjerite je li ravnopravnim dobar i je li moguće je trenutnog čvora povezati s ravnopravnim." Taj se problem riješen sada. | Da | ne |
| 3 | Prebacivanje na uređaju | U prethodnog izdanja, pojavio izgledi nedosljednosti podataka ako posao **otkrijte sigurnosne kopije** nije uspjela tijekom uređaj prebacivanje. Taj se problem riješen sada. | Da | ne |
| 4 | Prebacivanje na uređaju | U prethodnog izdanja, nakon prebacivanje na uređaju sigurnosne kopije nisu vidljive, ali spremnik pridružene glasnoće nije se nalazila na ciljni uređaj. Taj se problem riješen sada. | Da | ne |
| 5 | Prebacivanje na uređaju | U prethodnog izdanja, pojavio pogrešku numeriranje sigurnosne kopije u oblak tijekom postupka registra vraćanja koja može potencijalno dovesti do nedosljednosti podataka ako je došlo je do problema s povezivanjem oblaka. | Da | ne |
| 6 | Ažuriranje opreme | U prethodnog izdanja, zadatak ažuriranja za firmver uređaja nije uspjela i prikazuje pogrešku koju navedenu u cmdleta nisu može prepoznati i ažuriranje failed kao rezultat. Kontrolerom pa nije u načinu rada za oporavak. Taj se problem riješen sada. | Da | ne |
| 7 | Instalacija | Pogreške uzrokovanih uređaj neće se digitalno obrađen pravilno tijekom instalacije sada riješen. | Da | ne |
| 8 | Ponovno postavljanje tvorničke | Sada možete po želji preskočiti firmver provjeri tvorničke Vrati izvorne postavke. Ovo je promjena iz starijeg izdanja. | Da | ne |
| 9 | Ponovno postavljanje tvorničke | U prethodnog izdanja firmver verzije provjerava cmdlet za ponovno postavljanje tvorničke pokretala, nastali se samo za neke komponente hardvera. Dodatna oprema provjere nastali nakon prvo pokretanje procesa može uzrokovati ponovno neće uspjeti. Taj se popravak osigurava sve provjere firmver nastaju kada na tvorničke ponovno cmdlet da se pokreće i prije nego što prvi sustava ponovno. | Da | ne |
| 10 | Prostor za pohranu računa ključa rotacije | Cmdlet **Pozovite HcsmServiceDataEncryptionKeyChange** koristi da biste zakrenuli tipki račun za pohranu sada se traži od korisnika da biste unijeli ključ za šifriranje podataka servisa. Ovo je promjena iz starijeg izdanja proslijeđen ključa za šifriranje podataka servisa kao parametar u istoj razini. | Da | ne |
| 11 | Failback u roku od 24 sata | Tijekom oporavka Izrada čišćenja na uređaju izvora nije dogoditi failback cleanly, uzrokuju pogrešku uvoza. To je riješen u ovom izdanju. | Da | ne |

## <a name="known-issues-in-the-october-release"></a>Poznati problemi u izdanju listopad

Sljedeća tablica daje sažetak Poznati problemi u ovom izdanju.

| ne. | Značajka | Problem | Komentari/zaobilazno rješenje | Odnosi se na fizički uređaj | Odnosi se na virtualnog uređaja |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Ponovno postavljanje tvorničke | U nekim slučajevima prilikom izvršavanja tvorničke Vrati izvorne postavke uređaja StorSimple ostaju i prikazati ova poruka: **ponovno postavljanje na tvorničke je u tijeku (faza 8)**. To se događa ako pritisnete kombinaciju tipki CTRL + C dok cmdlet je u tijeku. | Pritisnite CTRL + C nakon pokretanja tvorničke Vrati izvorne postavke. Ako se već nalaze u tom stanju, obratite se Microsoft Support za sljedeće korake. | Da | ne |
| 2 | Ponovno postavljanje tvorničke | Učinite tvorničke ponovno postavljanje StorSimple uređaj koji ažurira iz GA u listopadu 2014 izdanja. | Ovaj postupak funkcionirat će samo ako je instaliran na zakrpu. Obratite se Microsoftovoj Support da biste dobili potrebne zakrpu. | Da | ne | 
| 3 | Kvorum disk | U rijetko slučajevima, ako Većina diskova s prilozima EBOD 8600 uređaja niste povezani rezultira kvorum bez diska pa na resurse za pohranu bit će izvanmrežno. On će ostati izvanmrežne čak i ako na diskova se ponovno povezati sa sustavom. | Morat ćete ponovno pokrenite uređaj. Ako se problem nastavi pojavljivati, obratite se Microsoft Support za sljedeće korake. | Da | ne |
| 4 | Oblak snimku stanja pogreške | U rijetko slučajevima, snimka oblaka možda neće funkcionirati uz poruku o pogrešci **maksimalno ograničenje sigurnosne kopije dosegne**. To se događa ako prelaziti 255 clones online na istom uređaju s istom izvornu glasnoću koji je izbrisan. | | Da | Da |
| 5 | ID netočan kontrolera | Zamjena kontroler provodi, kontroler 0 možda prikazuju se kao kontroler 1. Tijekom zamjenski kontroler prilikom učitavanja slike iz čvor ravnopravnih članova ID kontrolera može prikazivati na početku kao kontrolerom ravnopravnih članova ID-a. U rijetko instancama takvo ponašanje mogu se vidjeti i nakon ponovnog pokretanja sustava. |Potreban je bez korisničku akciju. U ovom slučaju će riješiti sam nakon zamjenski kontroler. | Da | ne |
| 6 | Uređaj nadzor grafikoni | U servisu StorSimple Upravitelj grafikoni nadzora uređaj ne funkcioniraju kad je osnovni ili NTLM je omogućena provjera autentičnosti u konfiguraciji proxy poslužitelja za uređaj. | Izmjena web-konfiguracija proxy poslužitelja za uređaj registrirani sa servisom za Upravitelj StorSimple stoga autentičnosti postavljena na nema. Da biste to učinili, pokrenite sustava Windows PowerShell za postavljanje HcsWebProxy StorSimple cmdlet. | Da | Da |
| 7 | Računi za pohranu | Putem servisa za pohranu da biste izbrisali račun za pohranu je nepodržane scenarij. To će dovesti do situacije u kojima ne može dohvatiti podatke o korisniku. | | Da | Da |
| 8 | Prebacivanje na uređaju | Više failovers glasnoću spremnika na istom uređaju izvora različite ciljnih nije podržana. | Prebacivanje s jednog reagira uređaja na više uređaja će postati spremnika glasnoće na prvom nije uspjela putem uređaja izgubiti vlasništvo nad podacima. Nakon takve prebacivanje te spremnika glasnoću će prikazuju se ili se drugačije ponašaju prilikom prikaza na portalu za Azure klasični. | Da | ne |
| 9 | Instalacija | Tijekom StorSimple prilagodnik za instalaciju sustava SharePoint, morate unijeti IP uređaj za instalaciju da biste uspješno završili.    | | Da | ne |
| 10 | Web proxy | Ako je vaše web-konfiguracija proxy HTTPS kao navedeni protokol, utjecat će vaše komunikacije servisa uređaja i će izvanmrežni uređaj. Podrška za pakete i moguće generirati u postupku troše značajan resursa na uređaju. | Provjerite ima li web-URL proxy HTTP kao navedeni protokol. Dodatne informacije o tome kako [Konfiguriraj web proxy za svoj uređaj](storsimple-configure-web-proxy.md). | Da | ne |
| 11 | Web proxy | Ako konfigurirati i omogućite web proxy na uređaju registriranog, zatim morat ćete ponovno pokrenite active kontroler na uređaju. | | Da | ne |
| 12 | Latencija visoke oblaka i visoke/i radno opterećenje | Kada naiđe na uređaju StorSimple kombinaciju vrlo visoka oblaka latencies (redoslijeda sekundi) i visoke/i radno opterećenje, količine uređaj idite u su smanjene stanje i na I/Os možda neće funkcionirati uz poruku o pogrešci "uređaj nije spreman". | Morat ćete ručno ponovno kontrolera uređaja ili ponovno prebacivanje uređaj da biste oporavili u tom slučaju. | Da | ne |

## <a name="physical-device-updates-in-the-october-release"></a>Pustite fizički uređaj ažuriranja u u listopadu

Prilikom ažuriranja primjenjuju se na fizički uređaj, verzija softvera pretvorit će se u 6.3.9600.17312. Ako je naveden u suprotnom, te napomene Primijeni na sve modela StorSimple uređaja. Dodatne informacije o ažuriranjima potražite u odjeljku [listopad 2014 uređaj fizički softvera ažuriranje za Microsoft Azure StorSimple uređaj](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Serijski priložene kontroler SCSI (SAS) i ažuriranja opreme u u listopadu izdanja

To je izdanje ažurirati upravljački program i opremu kontrolerom SAS fizičke uređaja. Dodatne informacije o kontrolerom SAS ažuriranje potražite u članku [Ažuriranje za Microsoft Azure StorSimple potražite kontrolera LSI SAS listopad 2014.](http://support.microsoft.com/kb/2987020).   

U ovom izdanju odnosi i kumulativna firmver ažuriranje koje rješava pouzdanosti probleme s uređajem hardverske komponente. Dodatne informacije o ažuriranje opreme, potražite u članku [listopad 2014 firmver ažuriranje za Microsoft Azure StorSimple uređaj](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-the-october-release"></a>Pustite virtualnog uređaja ažuriranja u u listopadu

Ovo izdanje sadrži sva ažuriranja za virtualnog uređaja. Primjena to ažuriranje neće se promijeniti softver verziju virtualnog uređaja.
 
