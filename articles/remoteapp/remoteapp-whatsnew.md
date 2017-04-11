
<properties
    pageTitle="Što je novo u Azure RemoteApp? | Microsoft Azure"
    description="Dodatne informacije o promjenama i poboljšane Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="whats-new-in-azure-remoteapp"></a>Što je novo u Azure RemoteApp?

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Jedna od prednosti Azure RemoteApp je omogućuje poboljšanje se uvijek radite. Svaki put kada moramo, ne možemo ćete objava te promjene.

## <a name="future-updates"></a>Buduća ažuriranja
Hej - jeste li znali u tim Azure RemoteApp objava mjesečnog ažuriranja bloga ZAPISI? Možete pronaći ne samo što je promjena Azure RemoteApp, ali i druge podatke o korištenju RDS. Pogledajte njihove blogu [Udaljene radne površine usluge bloga](https://blogs.msdn.microsoft.com/rds/), informacije. Na primjer, na sljedeća dva tjedna, oni objavili stavku o [dizanja i pomicanje radnih opterećenja s Azure RemoteApp i Azure AD](https://blogs.msdn.microsoft.com/rds/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services/).
 
## <a name="september-2015"></a>Rujan 2015.
- Dodane Infopath Microsoft Office 365 predloška i Galerija sliku. Ako želite omogućiti zajedničko korištenje programa Infopath, provjerite je li ažurirati zbirke s najnovijim slike.
- Ažuriranja za klijent:
    - Windows klijent ažurirati da bi korisnicima omogućiti zajedničko korištenje povratne informacije, osobito oko problema s povezivanjem.
    - iOS klijent ažurirati da biste riješili problem poruka o pogrešci, a da biste riješili problem gdje istekla vjerodajnice ranije nego je očekivano.
- Radimo na početak testirati podršku za Office 2016. Kada se dovrši koji, potražite ažurirani slike.
- Objaviti novi članak o [razlike u oblak i hibridnog zbirke](remoteapp-collections.md) – to će vam pomoći odaberite vrstu zbirke koja najbolje odgovara aplikacije – samo oblak, oblaka + VNET ili hibridnog.
- Želite omogućiti zajedničko korištenje QuickBooks pomoću Azure RemoteApp, ali niste sigurni korake? Pogledajte [novi članak Eric mjesta](remoteapp-quickbooks.md) koja vas obavještava točno što učiniti.

## <a name="august-2015"></a>Kolovoz 2015.
Veliki promjene se dogodilo u kolovozu – Evo isticanja:

- Sada možete koristiti u VNET Azure s oblaka zbirkom! Pogledajte [upute za stvaranje oblaka](remoteapp-create-cloud-deployment.md) za nove korake.
- Sada je moguće dodati aplikacije na izbornik **Start **za klijentski program Windows RemoteApp. Aplikacija će se vidjeti na popisu aplikacija, a možete ih prikvačiti na izbornik **Start **u sustavu Windows.
- Da biste dodali novu sliku u galeriju Azure VM – Windows Server udaljene radne površine glavnog za Microsoft Office 365 ProPlus.
- Fiksni klijenta Mac pa aplikacije s modalni windows će se zaustaviti Zamrzavanjem.
- Navedenih kako možete koristiti svoju [pretplatu za Office 365 ProPlus](remoteapp-officesubscription.md) s Azure RemoteApp.
- Detaljne kako možete [sigurne aplikacije i podataka](remoteapp-secure.md) u sklopu Azure RemoteApp zbirke.

## <a name="july-2015"></a>Srpanj 2015.

Srpanj pripreme promjene uskoro u kolovozu, tako da nema mnogo razgovarati o now, uglavnom ažuriranja dokumenta. Evo najnovije promjene:

- Dodaje karticu **podržava** portalu da biste mogli jednostavnije pristupati resurse za podršku, kao što je na forumima.
- Reworked informacije o otklanjanju poteškoća za stvaranje hibridnog zbirke. Pogledajte [najnovije i najveće](remoteapp-hybridtrouble.md) za Savjeti za otklanjanje poteškoća sviđa, prepoznavanje odgovarajući priključci da biste konfigurirali za vaše VNET.
- Navedenih kako [korisničkih podataka](remoteapp-upd.md) se stvara i sprema Azure RemoteApp.
- Navedenih kako [Zaključavanje aplikacije](remoteapp-secure.md).
- Objaviti [cmdleta Azure RemoteApp](https://msdn.microsoft.com/library/mt428031.aspx).
- I na kraju, ne možemo pokrenuti razgovor s nekim će korisnicima Azure RemoteApp o terminologija. Potražite promjene načina na koji objedinjuje mogućnosti različite zbirke.

## <a name="june-2015"></a>Lipnja 2015.

Toliko promjene! U tim je vrlo zauzet u lipnja:

- Redizajnirano za Azure RemoteApp [odredišne stranice](https://www.remoteapp.windowsazure.com/) - potražite u članku!
- Ažuriranje softver na sve slike koje su dostupne u sklopu pretplate.
- Upućuje poboljšanja hibridnog zbirke, uključujući prisilne tuneliranje podršku te provjeru IP podmreže veličinu prije no što pokušate stvoriti zbirku.
- Pronađu koji se * zamjenskih ne funkcionira za web-kamera. Umjesto toga, morate navesti instance ID ili GUID. Ćete smo ažuriranje preusmjeravanje da informacije odražavaju koji.
- Sada je tako da možete dodati prilagođene antivirusni softver sliku prilikom stvaranja sliku predloška iz galerije Azure.

Imamo još promjena vodoravnim u srpnju, pa ne možemo će uskoro vratiti s drugo ažuriranje.

## <a name="may-2015"></a>Svibanj 2015.

Putem možda su broj dodatke (i mjeseci) jer smo stvoreno ovoj se temi tako da se ovaj popis cheats malo i je od početka ožujak. Pogledajte sljedeće značajke:

- Automatiziranje sve – Azure RemoteApp sada ima [cmdleta u modul Azure PowerShell](remoteapp-tutorial-arawithpowershell.md).
- [Stvaranje Azure RemoteApp sliku s Azure virtualnog računala](remoteapp-image-on-azurevm.md). Čini se prenosi prilagođenu sliku Azure mnogo brže.
- Pomoću programa Azure VNET umjesto RemoteApp VNET povezati resursa u korporacijskom mrežom Azure. Ne možemo ste ažurirali [hibridnog zbirke upute](remoteapp-create-hybrid-deployment.md) za vas provesti kroz stvaranje programa Azure VNET (to je korak 1).
- Govoriti od VNETs, pogledajte [novu smjernice](remoteapp-vnetsizing.md) oko ograničenja veličine VNET i ograničenja.
- I govoriti od ograničenja – samo koje su [usluge ograničenja i zadanim postavkama](../azure-subscription-service-limits.md)?

Želite li saznati više o Azure RemoteApp? U tim RemoteApp je odgovor na snazi pri Ignite nekoliko tjedna. Pogledajte videozapis Eric korisnika [u osnove sustava Microsoft Azure RemoteApp Management i Administracija](http://channel9.msdn.com/Events/Ignite/2015/BRK3868).

Treba li vam da biste vidjeli Azure RemoteApp u stvarnom svijetu? Pogledajte vodič za [izvođenje bilo koju aplikaciju na bilo kojem uređaju bilo kojeg mjesta](remoteapp-anyapp.md) – pokazuje kako omogućiti zajedničko korištenje korisnika, uključujući i zajedničko korištenje datoteke baze podataka programa Access. Na Praktični vodič i imamo na [Office 365 za upućivanje](remoteapp-tutorial-o365anywhere.md) na isti se izvoditi na bilo kojem uređaju.

Hvala ostaje odabrana s nama – sigurnosno sljedeći mjesec s više ažuriranja.


### <a name="help-us-help-you"></a>Pomozite nam da vam pomoći
Jeste li znali da osim ocjena u ovom se članku i upućivanje komentare dolje ispod, koje možete unijeti promjene sam članak? Nešto nedostaje? Nešto nije u redu? Nije li moguće napisati nešto što je samo pregledniji? Pomicanje prema gore, a zatim kliknite **Uređivanje na GitHub** da biste unijeli promjene – one će dođite do nas za pregled, a zatim jednom smo odjaviti na njima, vidjet ćete promjene i poboljšanja ovdje.
