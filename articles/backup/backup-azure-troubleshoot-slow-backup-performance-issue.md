<properties
   pageTitle="Otklanjanje poteškoća s sporo sigurnosne kopije datoteka i mapa sigurnosnih kopija Azure | Microsoft Azure"
   description="Sadrži upute za otklanjanje poteškoća da biste lakše dijagnosticiranje uzrokuju probleme s performansama Azure sigurnosne kopije"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jimpark"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="genli"/>

# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Otklanjanje poteškoća s sporo sigurnosnu kopiju datoteke i mape u Azure sigurnosne kopije

Ovaj članak sadrži upute za otklanjanje poteškoća da biste lakše dijagnosticiranje uzroka slabe performanse sigurnosne kopije za datoteke i mape kada koristite Azure sigurnosne kopije. Kada koristite Azure sigurnosne kopije agent za stvaranje sigurnosnih kopija, postupak sigurnosnog kopiranja može potrajati dulje nego je očekivano. Odgode mogući uzrok nešto od sljedećeg:

-   [Nema učinka grla na računalu na kojem se stvara sigurnosnu kopiju.](#cause1)
-   [Neki drugi proces ili antivirusni softver ometa postupak sigurnosnog kopiranja Azure.](#cause2)
-   [Agent za sigurnosne kopije radi Azure virtualnog računala (VM).](#cause3)  
-   [Koristite sigurnosno velik broj (milijune) datoteke.](#cause4)

Prije nego što počnete otklanjanju poteškoća, preporučujemo da preuzmete i instalirate [najnovija Azure Backup agent](http://aka.ms/azurebackup_agent). Dajemo česta ažuriranja za Backup agent za otklanjanje problema vezanih uz različite, dodajte značajke i poboljšanja performansi.

Preporučujemo i da pregledate [Najčešća pitanja vezana uz sigurnosne kopije Azure servisa](backup-azure-backup-faq.md) da biste bili sigurni da neće se pojavi bilo koji od uobičajenih problema s konfiguracijom.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>
## <a name="cause-performance-bottlenecks-on-the-computer"></a>Uzrok: Performanse grla na računalu

Grla na računalu na kojem se stvara sigurnosnu kopiju mogu prouzročiti kašnjenja. Na primjer, na računalu omogućuje čitanje ili pisanje disk ili raspoložive propusnosti slanje podataka putem mreže, mogu prouzročiti grla.

Windows nudi ugrađene alat koji sadrži naziva [Nadzor performansi](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (programom Perfmon) da biste otkrili te grla.

Evo nekih mjerača performansi i raspone koji se mogu vam pomoći u dijagnosticiranje grla za optimalne sigurnosne kopije.

| Brojač  | Status  |
|---|---|
|Logičkog diska (fizički Disk) – % neaktivan   | • 100% neaktivno na 50% neaktivnosti = Zdravo</br>• 49% neaktivno 20% neaktivnosti = upozorenja ili monitora</br>• 19% neaktivno na 0% neaktivnosti = od ključne važnosti ili izvan specifikacija|
|  Logičkog diska (fizički Disk) – Prosječno % Na disku Sec čitanje ili pisanje |  • 0,001 ms za 0.015 ms = Zdravo</br>• 0.015 ms za 0.025 ms = upozorenja ili monitora</br>• 0.026 ms ili više = od ključne važnosti ili izvan specifikacija|
|  Logičkog diska (fizički Disk) – trenutni Duljina reda čekanja na disku (za sve instance) | 80 zahtjeva za više od 6 minuta |
| Memorija – Straničeno skup koji nisu bajtova|• Manje od 60% skup potrošena = Zdravo<br>• 61 za 80% od skup potrošena = upozorenja ili monitora</br>• Veće od 80% skup potrošena = od ključne važnosti ili izvan specifikacija|
| Memorija – Straničeno skup bajtova |• Manje od 60% skup potrošena = Zdravo</br>• 61 za 80% od skup potrošena = upozorenja ili monitora</br>• Veće od 80% skup potrošena = od ključne važnosti ili izvan specifikacija|
| Memorija – dostupna megabajta| • 50% besplatnog memorije ili više = Zdravo</br>• 25% od slobodne memorije dostupne = monitora</br>• 10% besplatnog memorije = upozorenje</br>• Manje od 100 MB ili 5% besplatnog memorije = od ključne važnosti ili izvan specifikacija|
|Procesor –\%procesor vrijeme (sve instance)|• Manje od 60% potrošena = Zdravo</br>• 61 za 90% potrošena = Monitor ili upozorenje</br>• 91 na 100% potrošena = od ključne važnosti|


> [AZURE.NOTE] Ako zaključite da je Infrastruktura utvrditi uzrok, preporučujemo da defragmentirate diskova redovito za bolje performanse.

<a id="cause2"></a>
## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Uzrok: Drugi proces ili antivirusnog softvera ometaju Azure sigurnosnog kopiranja

Smo vidjeli više instanci gdje drugi procesi u sustavu Windows ste negativno utjecati na performanse postupka agent Azure sigurnosnu kopiju. Na primjer, ako koristite agent za sigurnosno kopiranje Azure i neki drugi program za sigurnosno kopiranje podataka ili ako antivirusni softver i zatvorite zaključati na datotekama za sigurnosno kopiranje, višekratnik zaključavanja na datoteke može uzrokovati Nadmetanje. U tom slučaju možda neće uspjeti sigurnosno kopiranje ili posao može trajati dulje nego je očekivano.

Najbolji preporuke u ovom scenariju je da biste isključili drugi program za sigurnosne kopije da biste vidjeli je li sigurnosno kopiranje vrijeme za Azure sigurnosne kopije agent mijenja. Obično pazeći da više sigurnosne kopije zadataka nije pokrenut istovremeno je dovoljno da biste spriječili da ih utjecaja međusobno povezani.

Antivirusnog programa, preporučujemo da isključi sljedeće datoteke i mjesta:

- C:\Programske datoteke\Microsoft Azure oporavak Services Agent\bin\cbengine.exe kao proces
- C:\Programske datoteke\Microsoft Azure oporavak Services Agent\ mape
- Mjesto za odlaganje (Ako ne koristite standardne mjesto)

<a id="cause3"></a>
## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Uzrok: Backup agent sustavom Azure virtualnog računala

Ako koristite agent za sigurnosne kopije na na VM performanse bit će manja od kada pokrenete ga na računalo fizičke. Očekivan je zbog ograničenja IOPS.  Međutim, možete optimizirati performanse prebacivanjem pogona podataka koje su se sigurnosno Azure Premium spremište. Radimo na rješavanje taj problem i popravak bit će dostupni u buduće izdanje.

<a id="cause4"></a>
## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Uzrok: Sigurnosno kopiranje velik broj (milijune) datoteke

Premještanje veliku količinu podataka će trajati dulje nego premještanje manju količinu podataka. U nekim slučajevima sigurnosne kopije vrijeme povezana je s ne samo na veličinu podatke, već i kao broj datoteka ili mapa. Ovo je osobito true kad milijune male datoteke (nekoliko bajtova za nekoliko kilobajta) se stvara sigurnosnu kopiju.

To se događa jer dok ste sigurnosno kopiranje podataka, a da je premjestite na Azure, Azure je istodobno katalogiziranju datoteka. U nekim slučajevima rijetko operaciju kataloga može potrajati dulje nego je očekivano.

Sljedeći pokazatelji mogu olakšati razumijevanje na usko grlo i sukladno tome radite na sljedeće korake:

- **Korisničko Sučelje prikazuju tijek za prijenos podataka**. Podaci i dalje prenose. Propusnost mreže i veličina podataka možda uzrokuje kašnjenja.

- **Korisničko Sučelje ne prikazuju tijek za prijenos podataka**. Otvaranje zapisnika koja se nalazi na C:\Microsoft Azure oporavak usluge Agent\Temp, a zatim provjerite za unos FileProvider::EndData u zapisnicima. Ova stavka označava da završite prijenos podataka i operaciju kataloga se događa. Nemoj otkazati sigurnosne kopije zadataka. Umjesto toga, Pričekajte malo više operaciju kataloga da biste završili. Ako se problem nastavi pojavljivati, obratite se [Azure podržava](https://portal.azure.com/#create/Microsoft.Support).
