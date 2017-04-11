<properties
 pageTitle="Predlošci aplikacija logike | Microsoft Azure"
 description="Saznajte kako pomoću unaprijed stvoreni predlošci aplikacija logike olakšati početak rada"
 authors="kevinlam1"
 manager="dwrede"
 editor=""
 services="app-service\logic"
 documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="klam"/>

# <a name="logic-app-templates"></a>Predlošci aplikacija logike

## <a name="what-are-logic-app-templates"></a>Što su predlošci aplikacija logike

Predložak za aplikacije logike je unaprijed pripremljeni logike aplikacije koje možete koristiti za brzo pokretanje stvaranja vlastitog tijeka rada. 

Ti su predlošci dobar način za otkrivanje razne uzorke koje možete izgrađene pomoću aplikacije logike. Pomoću tih predložaka kao-je ili ih tako da stane scenariju mijenjati.

## <a name="overview-of-available-templates"></a>Pregled dostupni predlošci

Postoji mnogo dostupni predlošci trenutno objavljenim u članku platformu logike aplikacije. Neke primjer kategorije, kao i vrstu poveznika koji se koriste u njih navedena su u nastavku.

### <a name="enterprise-cloud-templates"></a>Poslovni predlošci oblaka
Predlošci integriranih Dynamics CRM, Salesforce, okvirima, blobova platforme Azure i druge poveznici za vaše potrebe oblaka enterprise. Primjeri od što možete učiniti s te predlošci uključuju organiziranje potencijalnih klijenata i sigurnosno kopiranje podataka tvrtke datoteke.

### <a name="enterprise-integration-pack-templates"></a>Poslovni Integracija s programom paketa predlošci
Konfiguracija VETER (Provjera valjanosti, izdvojiti, pretvorba, obogaćivanje, usmjeravanje) kanali, prima programa X12 uređivanje dokumenta putem AS2 i Pretvorba XML, kao i X12 i AS2 poruku pakiranja.

### <a name="protocol-pattern-templates"></a>Protokol uzorak predložaka
Te predloške sastoje se od logike aplikacije koje sadrže protokol uzoraka kao što je odgovor na zahtjev putem HTTP-a, kao i integracije preko FTP i SFTP. Upotrijebite te kako postoje ili kao osnovu za stvaranje uzoraka složenije protokol.  

### <a name="personal-productivity-templates"></a>Predlošci za osobnu produktivnost
Uzorci radi poboljšanja osobnu produktivnost uključivati predloške postavljanje dnevnih podsjetnika, pretvoriti važne stavke popisa zadataka i automatiziranje dugotrajan zadataka prema dolje do koraka odobrenje jednog korisnika.

### <a name="consumer-cloud-templates"></a>Predlošci potrošača oblaka
Jednostavni predlošci integriranih s društvenih mreža servise kao što je Twitter, prazan hod i e-pošte, konačni instaliranog strengthening društvenih mreža marketinške inicijative. To obuhvaća i predloške kao što su cloudy kopiranja, što može pomoći u povećanju produktivnosti spremanjem vrijeme utrošeno na zadatke Najčešći koji se ponavljaju. 

## <a name="how-to-create-a-logic-app-using-a-template"></a>Kako stvoriti logike aplikacije pomoću predloška 

Da biste počeli koristiti predložak za aplikacije logike, idite u dizajneru logike aplikacije. Ako je dizajner unosa tako da otvorite postojeći logike aplikacije, aplikaciju logike automatski učitava u prikazu dizajnera. Međutim, ako stvarate novu aplikaciju logike, vidjet ćete zaslona u nastavku.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

U ovom zaslonu ili možete odabrati da biste započeli prazne logike aplikacije ili unaprijed pripremljeni predložak. Ako odaberete neki od predložaka, ponudit će vam se s dodatnim informacijama. U ovom primjeru koristimo predloška *prilikom stvaranja nove datoteke na servisu Dropbox, kopirajte ih na OneDrive* .  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Ako odlučite koristiti predložak, samo odaberite gumb *pomoću ovog predloška* . Koje će se zatražiti da biste se prijavili svoje račune koji se temelji na koji poveznika koristi predložak. Ili, ako ste prethodno uspostaviti vezu s ove poveznika, možete odabrati i dalje kao što se vidi ispod.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Nakon uspostave veze, a zatim odaberite *Dalje*, aplikaciju logike otvara se u prikaz dizajnera.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

U primjeru iznad, kao što je to slučaj s mnogo predložaka neka polja obavezno svojstvo možda ispunjavanje unutar poveznika; Međutim, neke i dalje morat vrijednost mogli pravilno implementirati aplikaciju logike. Ako pokušate uvesti bez unosa neka polja koja nedostaju, primit ćete obavijest pojavljuje poruka o pogrešci.

Ako želite da biste se vratili u pregledniku predložak, na gornjoj navigacijskoj traci odaberite gumb *Predlošci* . Prebacivanjem natrag u pregledniku predložak izgubiti sve nespremljene napredak. Prije prebacivanja natrag u pregledniku predložak, vidjet ćete poruku upozorenja koja vas obavještava o to.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>Kako implementirati logike aplikacije stvorene iz predloška

Kad ste učitati predloška i napravili bilo kakve promjene, odaberite Spremi gumb u gornjem lijevom kutu. Time se sprema i objavljuje logike aplikacije.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Ako želite dodatne informacije na kako dodati dodatne korake u postojećeg logike predloška aplikacije ili provjerite uređuje Općenito, pročitajte više u [Stvaranje logike aplikacije](app-service-logic-create-a-logic-app.md).