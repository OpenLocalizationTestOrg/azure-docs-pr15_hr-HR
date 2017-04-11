<properties
    pageTitle="Najbolje prakse za aplikacije servisa za Azure"
    description="Saznajte najbolje prakse i otklanjanje poteškoća za aplikacije servisa za Azure."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="best-practices-for-azure-app-service"></a>Najbolje prakse za aplikacije servisa za Azure

U ovom članku navedene su Praktični savjeti za korištenje [Aplikacije servisa za Azure](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Colocation
Kada Azure resursi rješenja kao što su web-aplikacijama i baze podataka za sastavljanje poruke se nalaze u različitim područjima efekte možete obuhvaćaju sljedeće:

*  Povećana kašnjenje u komunikaciji između resursi
*  Novčane naknade za izlazne podatke prijenos više regije-kako je navedeno na [Azure cijene stranice](https://azure.microsoft.com/pricing/details/data-transfers).

Colocation u istom području je najbolje za Azure resurse rješenja kao što su web-aplikacijama i baze podataka ili prostora za pohranu račun koristili za držite sadržaja ili podataka za sastavljanje poruke. Prilikom stvaranja resursi provjerite je li se ona nalazi u istom Azure području osim ako imate određene poslovne ili dizajnirati razloga za njih ne. Aplikacije servisa za aplikacije na istom području bazu podataka možete premjestiti tako korištenje u [aplikacije servisa za kloniranje značajka](app-service-web-app-cloning-portal.md) trenutno dostupna za planiranje za Premium aplikacije servisa aplikacije.   

## <a name="memoryresources"></a>Kada aplikacija zauzeti dodatnu memoriju nego je očekivano
Kada primijetite aplikacije troši dodatnu memoriju nego je očekivano kao što je naznačeno putem nadzor ili servis za preporuke razmotriti [značajka automatskog popravka govornog servisa aplikacija](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Jednu od mogućnosti za značajku automatskog popravka automatski traje prilagođenih akcija na temelju memorije praga. Akcije obuhvaćati spektra od obavijesti e-poštom da biste istrage putem ispis memorije za ublažiti na-na-mjesto tako da recikliranje radnog procesa. Automatskog popravka automatski se može konfigurirati putem web.config i putem neslužbeni korisničkog sučelja kao što je opisano u u blogu za [Web-mjesta proširenja za podršku za aplikaciju servisa](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Kada aplikacija zauzeti više procesora nego je očekivano
Kada uočite aplikacije troši više procesora nego je očekivano ili iskustvo ponavlja procesora krivina kao što je naznačeno putem nadzor ili servis za preporuke razmislite o skaliranje prema gore ili skaliranje izvan aplikacije servisa za planiranje. Ako je vaša aplikacija statefull, skaliranje je mogućnost, dok je li vaša aplikacija bez praćenja stanja, skaliranja izlaz steći ćete više fleksibilnosti i veće mjerilo potencijal. 

Dodatne informacije o aplikacijama "bez praćenja stanja" za dodavanje veze za vanjskih "statefull" pogledajte ovaj videozapis: [Planiranje aplikacije za više razina skalabilni završetka do kraja na Microsoft Azure Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Dodatne informacije o aplikacije servisa za mogućnosti skaliranja i autoscaling: [Skaliranje Web App u aplikacije servisa za Azure](web-sites-scale.md).  

## <a name="socketresources"></a>Kada su iscrpljena socket resursi
Uobičajeni razlog curenja izlazne veze TCP je korištenje biblioteka klijenta koji nije implementirana da biste ponovno koristili TCP veze ili, u slučaju veći razine protokol kao što su HTTP - produžite koji se ne leveraged. Pregledajte potražite u dokumentaciji svim bibliotekama koje referencira aplikacije u vaše aplikacije servisa Plan da biste bili sigurni konfiguriran ili pristupiti u kodu za učinkovito korištenje izlazne veze. I slijedite upute dokumentaciju biblioteka za stvaranje proper i izdanje ili čišćenje da biste izbjegli propušta veze. Dok takve biblioteke klijent istrage su u tijeku utjecaj se možda mitigated po skaliranje više instanci.  

## <a name="appbackup"></a>Kada aplikacije sigurnosno kopiranje pokreće ne uspijeva
Dva najčešćih razloga zašto se sigurnosno kopiranje ne uspije aplikacije: postavke prostora za pohranu koji nisu valjani i konfiguraciji baze podataka nije valjana. Te pogreške obično događa kada su promjene na resurse za pohranu ili u bazi podataka ili promjene za pristup sljedećim resursima (npr. vjerodajnice ažurira odabran u postavke za sigurnosno kopiranje baze podataka). Sigurnosno kopiranje obično se izvoditi na raspored i zahtijevaju pristup za pohranu (za bilježiti sigurnosne kopije datoteka) i baza podataka (za kopiranje i sadržaj koji želite uvrstiti u sigurnosne kopije za čitanje). Rezultat ne uspijeva pristupiti bilo koju od tih resursa bi dosljedan sigurnosne kopije nije uspjelo. 

Kada sigurnosne kopije neuspjeha dogoditi, pregledajte najnovije rezultata da biste shvatili se događa koju vrstu pogreške. U slučaju Nemogućnost pristupa za pohranu, pregledajte i ažuriranje postavki za pohranu koriste u Konfiguracija sigurnosnog kopiranja. U slučaju pogreške na baze podataka programa access, pregledajte i ažuriranje nizovima veze kao dio postavki aplikacije; zatim nastavite da biste ažurirali Konfiguracija sigurnosnog kopiranja pravilno uključiti potrebna baze podataka. Dodatne informacije o sigurnosnu kopiju aplikacije potražite u dokumentaciji za [sigurnosno kopiranje web app u aplikacije servisa za Azure](web-sites-backup.md) .

## <a name="nodejs"></a>Kada su implementiran novih Node.js aplikacija za aplikacije servisa za Azure
Azure Konfiguracija zadane aplikacije servisa za aplikacije Node.js je namijenjen najbolje odgovaraju potrebama najčešćih aplikacija. Ako konfiguracije aplikacije Node.js bi im personalizirane usklađivanje da biste poboljšali performanse ili optimizirati korištenje resursa za procesora i memorije/mrežni resursi, nije moguće pregledajte naše Praktični savjeti i upute za otklanjanje poteškoća. Dokumentacija u ovom se članku opisuje postavke iisnode možda ćete morati konfigurirati Node.js aplikacije, opisuju scenariji za različite ili problemi možda nasuprotne aplikacije, a prikazuje način za rješavanje tih problema: [najbolje prakse i otklanjanje poteškoća vodič za čvor aplikacije na aplikacije servisa za Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   


