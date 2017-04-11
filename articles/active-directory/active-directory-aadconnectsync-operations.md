<properties
   pageTitle="Azure AD Connect sinkronizacije: operativnih zadataka i napomene | Microsoft Azure"
   description="U ovoj se temi opisuju operativnih zadataka za Azure AD Connect sinkronizaciju i kako pripremiti za rad komponente."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD Connect sinkronizacije: operativnih zadataka i slučajevi
Je cilj u ovoj se temi opisuje operativnih zadataka za Azure AD Connect sinkronizaciju.

## <a name="staging-mode"></a>Pripremna načinu
Pripremna način moguće je koristiti za nekoliko scenarija, uključujući:

-   Visoke dostupnosti.
-   Testirajte i implementirajte novu promjene konfiguracije.
-   Predstavljanje novi poslužitelj i isključiti stari.

Na poslužitelju u načinu rada za pripremna možete napraviti promjene konfiguracije i pregledati promjene prije nego što ste aktivirali poslužitelj. Omogućuje vam pokretanje potpunog uvoza i potpune sinkronizacije da biste potvrdili da se sve promjene se očekuje prije promjene te u okruženju sustava radnog.

Tijekom instalacije, možete odabrati poslužitelja u **pripremna načinu rada**. Ovu akciju poslužitelj postaje aktivan za uvoz i sinkronizacija, ali pokrenuti sve izvozi. Na poslužitelju u načinu rada za pripremna nije pokrenut sinkronizaciju lozinke ili upisima lozinke, čak i ako ste odabrali te značajke tijekom instalacije. Kada onemogućite pripremna načinu poslužitelja započinje s izvozom, omogućuje sinkronizaciju lozinke, a omogućuje upisima lozinku.

Izvoz i dalje možete nametnuti pomoću upravitelja za sinkronizaciju servisa.

Na poslužitelju u načinu rada za pripremna i dalje primati promjene iz servisa Active Directory i Azure AD. Uvijek ima kopiju najnovije promjene i mogu vrlo brzo preuzimanje putem obaveze drugi poslužitelj. Ako izvršite promjene konfiguracije vaš primarni poslužitelj je odgovornost za isti promjene na poslužitelj u pripremna način.

Za one s znanja starije tehnologijama sinkronizacije, pripremna načina razlikuje jer je poslužitelj je vlastitu SQL baze podataka. U ovom arhitektura omogućuje pripremna poslužitelju način da bi se nalaziti u različitim podatkovnog centra.

### <a name="verify-the-configuration-of-a-server"></a>Provjerite konfiguraciju poslužitelja
Da biste primijenili taj način, slijedite ove korake:

1. [Priprema](#prepare)
2. [Uvoz i sinkronizacija](#import-and-synchronize)
3. [Provjerite je li](#verify)
4. [Promjena aktivnog poslužitelja](#switch-active-server)

#### <a name="prepare"></a>Priprema

1. Instalacija Azure AD Connect odaberite **pripremna način**i poništavanje **započeti sinkronizaciju** na zadnjoj stranici čarobnjaka za instalaciju. Ovaj način omogućuje vam da biste ručno pokrenuli modula za sinkroniziranje.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Prijava isključivanje/za prijavu i s izbornika start odaberite **Servis za sinkronizaciju**.

#### <a name="import-and-synchronize"></a>Uvoz i sinkronizacija

1. Odaberite **poveznika**, a zatim poveznik za prvi s vrstom **Active Directory Domain Services**. Kliknite **Pokreni**, odaberite **potpuni uvoz**, a zatim **u redu**. Učini sljedeće korake za sve poveznike ove vrste.
2. Odaberite poveznik s vrstom **Azure Active Directory (Microsoft)**. Kliknite **Pokreni**, odaberite **potpuni uvoz**, a zatim **u redu**.
3. Provjerite je li i dalje odabrana kartica poveznike. Za svaki poveznik s vrstom **Active Directory Domain Services**, kliknite **Pokreni**, odaberite **Delta sinkronizacije**, a zatim **u redu**.
4. Odaberite poveznik s vrstom **Azure Active Directory (Microsoft)**. Kliknite **Pokreni**, odaberite **Delta sinkronizacije**, a zatim **u redu**.

Koje su sada kopirana bez postavljanja izvoz mijenja Azure AD i lokalnog servisa Active Directory (Ako koristite hibridna implementacija sustava Exchange). Daljnji koraci omogućuju Provjera što uskoro će promjena prije zapravo Izvoz u direktorije.

#### <a name="verify"></a>Provjerite je li

1. Pokretanje cmd redak, a zatim prijeđite na drugi`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Pokrenite:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Naziv poveznik pronaći ćete u servis za sinkronizaciju. Ima naziv koji je slična "contoso.com – AAD" za Azure AD.
3. Pokrenite:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Imate li datoteku u % temp % pod nazivom export.csv koje možete pregledavaju u programu Microsoft Excel. Datoteka sadrži sve promjene koje želite izvesti.
5. Izvršite potrebne promjene podataka ili konfiguraciju i pokretanje korake ponovno (uvoz i Sinkronizacija i potvrda) sve dok se očekuje promjene koje želite izvesti.

**Objašnjenje export.csv datoteka**

Većina datoteka je self-explanatory. Neke kratice da biste shvatili sadržaja:

- OMODT – vrsta objekta izmjene. Označava je li postupak na razini objekta dodavanje, ažuriranje ili Izbriši.
- AMODT – Vrsta atributa izmjene. Označava je li postupak na razini atributa dodavanje, ažuriranje ili Izbriši.

Ako je vrijednost atributa s više vrijednosti, prikazuje se ne svake promjene. Vidljiv je samo broj vrijednosti dodaju i uklanjaju.

#### <a name="switch-active-server"></a>Promjena aktivnog poslužitelja

1. Na trenutno aktivnog poslužitelja isključite server (DirSync/FIM/Azure AD Sync) da bi se ne izvoz za Azure AD ili postaviti u pripremna način (Azure AD Connect).
2. Pokrenite čarobnjak za instalaciju na poslužitelju u **pripremna način** te Onemogući **pripremna načinu rada**.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Izrada oporavak
Dio dizajna implementaciju je plan za što učiniti ako postoji u Izrada gdje izgubiti poslužitelj za sinkronizaciju. Postoje različite modelima koristiti i onaj za korištenje ovisi o nekoliko čimbenika uključujući:

-   Što je vaš odstupanje za ne uspije promijenite objektima u Azure AD tijekom na nedostupnost?
-   Ako koristite sinkronizaciju lozinke, korisnici prihvaćate moraju koristiti staru lozinku u Azure AD za slučaj da je mijenjaju lokalnog?
-   Imate li ovisnosti u stvarnom vremenu operacija, kao što su upisima lozinku?

Ovisno o odgovori na ta pitanja i pravila vaše tvrtke ili ustanove, jedan od sljedećih načina može se implementirati:

-   Ponovno stvaranje po potrebi.
-   Imati rezervnih čekanja poslužitelju, naziva **pripremna načinu rada**.
-   Korištenje virtualnih računala.

Ako ne koristite na ugrađenu SQL Express baze podataka, zatim pregledajte i u odjeljku [SQL visoke dostupnosti](#sql-high-availability) .

### <a name="rebuild-when-needed"></a>Ponovno stvaranje po potrebi
Izgledna strategije je plan za poslužitelj izvođenje po potrebi. Obično instaliranje modula za sinkroniziranje i do početne uvoz i sinkronizacija možete obaviti u roku od nekoliko sati. Ako nije dostupna rezervnih poslužitelju, moguće je privremeno korištenje kontroler domene za glavno računalo modula za sinkroniziranje.

Poslužitelj za sinkronizaciju modul pohraniti sve stanje o objektima pa ponovno bazu podataka možete stvoriti iz podataka u servisu Active Directory i Azure AD. Da biste se pridružili objekte iz lokalnog poslužitelja u oblak i koristi se atribut **sourceAnchor** . Ponovno stvaranje poslužitelja s postojećeg objekata lokalnog poslužitelja u oblak i zatim modula za sinkroniziranje odgovara li te objekte zajedno ponovno na ponovne instalacije. Stvari koje treba dokumenta i spremanje su konfiguracije promjene na poslužitelj, kao što je filtriranje i sinkronizacija pravila. Ove prilagođene konfiguracije mora biti ponovno primijeniti prije nego što počnete sinkroniziranje.

### <a name="have-a-spare-standby-server---staging-mode"></a>Imate rezervnih čekanja poslužitelju – pripremna načinu rada
Ako imate složenije okruženja, preporučuje se da imate jedan ili više čekanja poslužitelja. Tijekom instalacije, možete omogućiti na poslužitelju u **pripremna načinu rada**.

Dodatne informacije potražite u članku [pripremna načinu rada](#staging-mode).

### <a name="use-virtual-machines"></a>Korištenje virtualnih računala
Uobičajeni i podržanim način je da biste pokrenuli sinkronizaciju modul u virtualnog računala. U slučaju da glavno računalo ima problema, slika s poslužiteljem za sinkronizaciju modula moguće je premjestiti na drugi poslužitelj.

### <a name="sql-high-availability"></a>SQL visoke dostupnosti
Ako se ne koriste sustava SQL Server Express koji se isporučuju s Azure AD Connect, zatim visoke dostupnosti za SQL Server i razmatranje. Rješenje samo visoke dostupnosti podržano je SQL Klasteriranje. Nepodržane rješenja obuhvaćaju zrcaljenja i uvijek na.

## <a name="next-steps"></a>Daljnji koraci

**Pregled tema**  

- [Azure AD Connect sinkronizacije: razumijevanje i Prilagodba sinkronizacije](active-directory-aadconnectsync-whatis.md)  
- [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)  
