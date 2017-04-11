<properties
    pageTitle="Što je Azure Automatizacija | Microsoft Azure"
    description="Saznajte koje vrijednost omogućuje automatizaciju Azure te Saznajte odgovore na najčešća pitanja da bi vam mogli početi s radom u stvaranju, pomoću runbooks i DSC Automatizacija Azure."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="što je Automatizacija, azure Automatizacija, Primjeri azure Automatizacija"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="05/10/2016"
    ms.author="magoedte;bwren"/>

# <a name="azure-automation-overview"></a>Pregled Azure automatizacije

Automatizacija Microsoft Azure omogućuje korisnicima omogućuje automatizaciju zadataka ručno, dugoročnih, podložni pogreške i često koji se ponavljaju, koji se često izvršavaju u okruženje oblaka i enterprise. Ga štedi vrijeme i povećava pouzdanosti obične administrativne zadatke i čak i rasporede da se automatski izvodi u pravilnim vremenskim razmacima. Možete Automatizacija procesa pomoću runbooks ili automatizirati konfiguracije upravljanja želji konfiguraciju stanje. Ovaj članak sadrži kratak pregled Automatizacija Azure i odgovori na uobičajena pitanja. Možete se referirati na druge članke u biblioteci za detaljnije informacije o različitim temama.


## <a name="automating-processes-with-runbooks"></a>Automatizacija procesa pomoću runbooks

U runbook je skup zadataka koje izvršiti neke automatiziranog procesa u automatizaciji Azure. Možda ćete jednostavan postupak kao što su početni virtualnog računala i stvaranje stavka evidencije ili imate složene runbook koji kombinira druge manje runbooks izvođenje složen proces preko više resursa ili čak i više oblaka i na lokalnu instancu okruženja.  

Na primjer, možda imate postojeće ručnog procesa za rezanja bazom podataka SQL ako približava maksimalne veličine koja sadrži više korake kao što su povezivanja s poslužiteljem, povezivanje s bazom podataka, dobiti Trenutna veličina baze podataka, provjerite prag premašila i zatim ga izrezati i obavijestili korisnika. Umjesto da ručno izvođenje svaki od ovih koraka, mogli biste stvoriti runbook koje obavljate sve zadatke kao jedan proces. Želite pokrenuti na runbook, navedite potrebne podatke kao što je SQL naziv poslužitelja, naziv baze podataka i e-pošte primatelja i zatim sjesti natrag dok po dovršetku postupka. 


## <a name="what-can-runbooks-automate"></a>Što je runbooks automatizirati?

Runbooks u automatizaciji Azure temelje se na web-mjesto komponente Windows PowerShell ili u okvir za Windows PowerShell tijek rada, pa ih sve što možete učiniti PowerShell. Ako je program ili servis API, na runbook možete raditi s njim. Ako imate modul ljuske PowerShell za aplikaciju, možete učitati taj modul Azure Automatizacija i uključiti tih cmdleta u vašem runbook. Azure runbooks Automatizacija pokreću se u oblak Azure i možete pristupiti oblaka resursi ni vanjskim izvorima kojemu je moguće pristupiti iz oblaka. Pomoću [Tempiranja Runbook hibridnog](automation-hybrid-runbook-worker.md)runbooks možete pokrenuti u centru za lokalnih podataka da biste upravljali Lokalni resursi. 


## <a name="getting-runbooks-from-the-community"></a>Dohvaćanje runbooks iz zajednice

[Galerija Runbook](automation-runbook-gallery.md#runbooks-in-runbook-gallery) sadrži runbooks od Microsofta i zajednice možete upotrijebiti nepromijenjena u svom okruženju ili ih prilagoditi vlastitim svrhe. Također su korisni kao reference da biste saznali kako stvoriti vlastiti runbooks. Čak i mogu pridonijeti vlastite runbooks u galeriju mislite da drugi korisnici mogu pronaći korisne. 


## <a name="creating-runbooks-with-azure-automation"></a>Stvaranje Runbooks s Azure automatizacije 

Možete [stvoriti vlastitu runbooks](automation-creating-importing-runbook.md) od nule ili izmjena runbooks iz [Galerije Runbook](http://msdn.microsoft.com/library/azure/dn781422.aspx) s vlastitim potrebama. Postoje tri različite [vrste runbook](automation-runbook-types.md) koje možete odabrati na temelju preduvjeti i sučelje komponente PowerShell. Ako biste radije radite izravno s kodom PowerShell, pa možete koristiti [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) ili [runbook PowerShell tijeka rada](automation-runbook-types.md#powershell-workflow-runbooks) koje uređujete izvanmrežno ili pomoću [tekstnih editor](http://msdn.microsoft.com/library/azure/dn879137.aspx) na portalu za Azure. Ako biste radije da biste uredili u runbook bez izlaganja podlozi kod, možete stvoriti [grafički runbook](automation-runbook-types.md#graphical-runbooks) pomoću [grafičkog editor](automation-graphical-authoring-intro.md) na portalu za Azure. 

Radije gledaju za čitanje? Imate pogledati na ispod videozapisa s Microsoft Ignite sesiju svibanj 2015. Napomena: Dok koncepata i značajke koje se spominju u ovom ćete videozapisu ispravni, automatizacija Azure progressed sadrži mnogo jer je naveden u ovom se videozapisu, je sada sadrži širi korisničko Sučelje na portalu za Azure i podržava dodatne mogućnosti.

> [AZURE.VIDEO microsoft-ignite-2015-automating-operational-and-management-tasks-using-azure-automation]


## <a name="automating-configuration-management-with-desired-state-configuration"></a>Automatizacija konfiguracije upravljanja stanje konfiguraciji želji 

[PowerShell DSC](https://technet.microsoft.com/library/dn249912.aspx) je upravljanje platformu koja omogućuje upravljanje, uvođenje i Nametni konfiguraciju za fizičke domaćini i virtualnih računala pomoću deklarativno PowerShell sintakse. Možete definirati konfiguracije na središnje DSC istaknuti poslužitelju koji cilj strojeva možete automatski dohvatiti i primijeniti. DSC pruža skup cmdleta ljuske PowerShell koje možete koristiti da biste upravljali konfiguraciju i resursa.  

[DSC Automatizacija Azure](automation-dsc-overview.md) je rješenje u oblaku za PowerShell DSC koja omogućuje servise koji su potrebni za korporacijskom okruženju.  Možete upravljati DSC resursa u automatizaciji Azure i Primjena Konfiguracija virtualne ili fizička strojeva koji dohvaćaju ih s poslužitelja odvojiti za DSC u Azure oblaka.  Također nudi servisu reporting services obavijestili o važnim događajima kao što su kada čvorove odstupaju iz svoje dodijeljene konfiguracije i Nova konfiguracija primijenjen. 


## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Stvaranje vlastite konfiguracije DSC s Automatizacija Azure

[Konfiguracija DSC](automation-dsc-overview.md#azure-automation-dsc-terms) navedite željeni stanje čvor.  Više čvorove možete primijeniti istu konfiguraciju da biste dobili svi oni zadržali jednake stanje.  Možete stvoriti uređivač konfiguracije pomoću sav tekst na lokalnom računalu i uvezite ga u Azure Automatizacija gdje ga možete sastaviti i primijenite ga čvorove.


## <a name="getting-modules-and-configurations"></a>Preuzimanje modula i konfiguracija 

Možete dobiti [PowerShell moduli](automation-runbook-gallery.md#modules-in-powershell-gallery) koji sadrži cmdleta koje možete koristiti u runbooks i konfiguracija DSC iz [Galerije PowerShell](http://www.powershellgallery.com/). Možete pokrenuti galerije s portala za Azure i izravno uvesti modula Azure Automatizacija ili možete preuzeti i uvesti ih ručno. Nije moguće instalirati module izravno na portalu Azure, ali ih možete preuzeti instalirajte ih kao i sve druge module. 


## <a name="example-practical-applications-of-azure-automation"></a>Primjer praktično aplikacije Automatizacija Azure 

Slijede samo nekoliko primjera koji su vrste scenariji za automatizaciju Azure automatizaciju. 

* Stvaranje i kopiranje virtualnim strojevima u različite Azure pretplate. 
* Zakazivanje kopiranja datoteka s lokalnog računala u spremniku spremište blobova platforme Azure. 
* Automatiziranje sigurnost funkcije kao što je odbiti zahtjeve od klijenta kada se otkrije uskraćivanje napada servisa. 
* Provjerite je li strojeva neprestano poravnali s konfiguriranog sigurnosna pravila.
* Upravljanje neprekinuti implementacije kod aplikaciju putem oblaka i na lokalni infrastrukture. 
* Stvaranje skupa stabala u servisu Active Directory u Azure okruženju sustava Laboratorija. 
* Izrežite tablice u bazi podataka sustava SQL ako DB približava maksimalne veličine. 
* Daljinsko ažuriranje postavki okruženja za Azure web-mjesto. 


## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Kako Azure Automatizacija ne odnose na druge alate za automatizaciju?

Da biste automatizirali zadatke upravljanja u privatnu oblaka namijenjen je [Usluga upravljanja Automatizacija (SMA)](http://technet.microsoft.com/library/dn469260.aspx) . Je instaliran lokalno u podatkovnom centru kao dio [Paketa Microsoft Azure](https://www.microsoft.com/en-us/server-cloud/). SMA i Azure Automatizacija koristiti isti runbook oblik koji se temelji na Windows PowerShell i tijek rada sustava Windows PowerShell, ali SMA ne podržava [grafički runbooks](automation-graphical-authoring-intro.md).  

[Sustav centar 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) namijenjen Automatizacija lokalnog resursa. Koristi oblik različite runbook od Automatizacija Azure i Automatizacija usluga upravljanja i sadrži grafičkog sučelja da biste stvorili runbooks bez sve skriptiranje. Njegov runbooks sastoje se od aktivnosti iz Integracija s programom paketa koji se pišu namijenjenu Orchestrator. 


## <a name="where-can-i-get-more-information"></a>Gdje pronaći dodatne informacije? 

Raznih resursa su dostupne za dodatne informacije o Automatizacija Azure i stvaranje vlastite runbooks. 

* **Biblioteka za automatizaciju Azure** je gdje se odmah. U člancima u tu biblioteku navedite potpuni dokumentaciju o konfiguraciji i administracije Automatizacija Azure i za stvaranje vlastitog runbooks. 
* [Cmdleti za Azure PowerShell](http://msdn.microsoft.com/library/jj156055.aspx) navedene informacije za automatiziranje Azure operacije pomoću komponente Windows PowerShell. Runbooks pomoću tih cmdleta za rad s resursima za Azure. 
* [Upravljanje Blog](https://azure.microsoft.com/blog/tag/azure-automation/) nudi najnovije informacije na Automatizacija Azure i druge tehnologije upravljanje od Microsofta. Morate se pretplatiti na blog da biste bili u tijeku s novostima tima za automatizaciju Azure. 
* [Forum za automatizaciju](http://go.microsoft.com/fwlink/p/?LinkId=390561) omogućuje objavljivati pitanja o Azure Automatizacija da biste biti adresirane Microsoft i automatizaciju zajednice. 
* [Cmdleti za automatizaciju Azure](https://msdn.microsoft.com/library/mt244122.aspx) pruža informacije za automatiziranje zadataka administracije. Cmdleti za upravljanje računima Automatizacija, resursi, runbooks, DSC sadrži.


## <a name="can-i-provide-feedback"></a>Mogu poslati povratne informacije? 

**Pošaljite nam poslali povratne informacije!** Ako tražite rješenja programa automatizacije Azure runbook ili modul za integraciju, objavite zahtjev skripte na centar za skripte. Ako imate povratne informacije ili značajku zahtjeva za automatizaciju Azure, objavite ih na [Glas korisnika](http://feedback.windowsazure.com/forums/34192--general-feedback). Hvala! 


