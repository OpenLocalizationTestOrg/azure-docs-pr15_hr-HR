<properties
    pageTitle="Napomene za servise Azure BizTalk | Servisa Microsoft Azure BizTalk"
    description="Poznati problemi za Azure BizTalk servise za popise" 
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="release-notes-for-azure-biztalk-services"></a>Napomene za Azure BizTalk servise

Napomene za Microsoft Azure BizTalk Services sadrže Poznati problemi u ovom izdanju.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>Što je novo u studenom ažuriranje BizTalk servisa
* Šifriranje na ostale može se omogućiti na portalu servisa BizTalk. Potražite u članku [Omogućivanje šifriranja na ostale BizTalk Services portalu](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Povijest ažuriranja

### <a name="october-update"></a>Ažuriranja u listopadu

* Podržani su računi tvrtke ili ustanove:  
 * **Scenarij**: registrirana implementacije BizTalk servis pomoću Microsoftova računa (kao što su user@live.com). U ovom scenariju samo korisnici Microsoftov Account možete upravljati BizTalk servis pomoću portala za BizTalk Services. Račun tvrtke ili ustanove ne mogu se koristiti.  
 * **Scenarij**: registrirana implementacije BizTalk servis pomoću računa tvrtke ili ustanove u Azure Active Directory (kao što su user@fabrikam.com ili user@contoso.com). U ovom scenariju samo korisnici Azure Active Directory unutar iste tvrtke ili ustanove mogu upravljati BizTalk servis pomoću portala za BizTalk Services. Microsoftov račun nije moguće koristiti.  
* Kada stvarate BizTalk servisa na portalu za Azure klasični, su automatski registered na portalu servisa BizTalk.
 * **Scenarij**: prijaviti Azure klasični portal stvaranje BizTalk servisa, a zatim odaberite **Upravljanje** vrlo prvi put. Kada na portalu servisa BizTalk otvori, servis BizTalk automatski registrira i spreman je za vaše implementacije.  
 Potražite u članku [Registracija i ažuriranje implementacije BizTalk servisa na portalu servisa BizTalk](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>Ažuriranja za kolovoz 14
* Slaganje i most decoupling – trgovina ugovore partnera i bridges su sada samostalne na portalu servisa BizTalk. Sada stvorite ugovore i bridges zasebno, a prilikom izvođenja bridges odnosi se na temelju vrijednosti u poruci za uređivanje ugovor. Potražite u članku [Stvaranje ugovore Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689908.aspx), [stvoriti programa most Uređivanje pomoću portala za usluge BizTalk](https://msdn.microsoft.com/library/azure/dn793986.aspx), [Stvaranje AS2 aplikacije bridge pomoću portala za usluge BizTalk](https://msdn.microsoft.com/library/azure/dn793993.aspx)i [kako bridges ne riješite ugovore prilikom izvođenja?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* Ukinute je mogućnost stvaranja predloške za ugovore.  
* Za ugovor za slanje strani sada možete odrediti graničnik različite skupove za svaku shemu. Tu konfiguraciju naveden je u odjeljku postavke protokola za slanje strani ugovor. Dodatne informacije potražite u članku [Stvaranje pojavila X12 ugovor Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689847.aspx) i [Stvaranje ugovor EDIFACT Azure BizTalk Services](https://msdn.microsoft.com/library/azure/dn606267.aspx). Dva novi entiteti i dodaju TPM OM API-JA u istu svrhu. U odjeljku [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) i [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standardni XSD konstrukta, uključujući izvedena vrste sada podržava. Potražite u članku [koristi standardnu XSD konstrukcije u vašem karte](https://msdn.microsoft.com/library/azure/dn793987.aspx) i [Korištenje izvedena Vrsta mapiranja scenariji i primjeri](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 podržava novih Mikrofon algoritama za potpisivanje poruka i novu algoritama šifriranja. Potražite u članku [Stvaranje ugovor AS2 Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
## <a name="know-issues"></a>Saznajte problema

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Problema s povezivanjem nakon BizTalk Services ažuriranje portala

  Ako imate Portal servisa BizTalk otvoriti dok BizTalk usluge će se nadograditi poništiti promjene na servis, možda će biti namijenjeno problema s povezivanjem s portala za usluge BizTalk.  
  Kao zaobilazno rješenje, možda ponovno pokrenite preglednik, brisanje predmemorije u pregledniku ili pokretanje portal u privatnu način rada.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE ne možete pronaći u artefakt ako kliknete pogreške ili upozorenja u programu project BizTalk Services
Instalirajte Visual Studio 2012 ažuriranje 3 RC 1 da biste riješili problem.  

### <a name="custom-binding-project-reference"></a>Prilagođeno povezivanje projekta reference
Razmislite o sljedećim situacijama s projektom BizTalk servisa Visual Studio rješenja:  
* U istoj rješenja za Visual Studio postoji BizTalk Services projekta i prilagođeno povezivanje projekta. Project BizTalk servisa ima referencu u ovoj datoteci Prilagođeno povezivanje projekta.
* Servis BizTalk projekt ima referencu prilagođene povezivanje/ponašanje DLL.

"Stvaranja' rješenja u Visual Studio uspješno. Nakon toga 'Ponovno stvaranje' ili 'Čišćenje' rješenja. Nakon toga ponovno stvaranje ili očistiti ponovno javlja se sljedeća pogreška:  
  Kopiranje datoteke nije moguće <Path to DLL> za "bin\Debug\FileName.dll". Postupak ne može pristupiti datoteci 'bin\Debug\FileName.dll' jer je koristi neki drugi proces.  

#### <a name="workaround"></a>Zaobilazno rješenje
* Ako je instaliran [Visual Studio 2012 ažuriranje 3](https://www.microsoft.com/download/details.aspx?id=39305) , imate sljedeće dvije mogućnosti:

  * Ponovno pokrenite Visual Studio ili

  * Ponovno pokrenite rješenja. Nakon toga izvršiti samo Sastavi rješenja.  

* Ako nije instaliran [Visual Studio 2012 ažuriranje 3](https://www.microsoft.com/download/details.aspx?id=39305) , otvorite upravitelj zadataka, kliknite karticu procesa, MSBuild.exe postupak, kliknite, a zatim gumb Završi proces.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Usmjeravanje BasicHttpRelay krajnje točke nije podržana bridges i Portal servisa BizTalk ako znakove koji se ne ispisuju su promaknuti kao HTTP zaglavlja

Ako koristite znakove koji se ne ispisuju kao dio poboljšanih svojstva za poruke, te poruke ne može biti proslijeđene preusmjeravanja odredišta koje koriste BasicHttpRelay povezivanja. Osim toga, poboljšanih svojstva koje dostupni su dio praćenje su kodiran URL-om za blob-ova i nepotvrđen kodiranih za odredišta.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>MDN asinkrono poslane čak i ako je isključena mogućnost MDN asinkronog za slanje  
Razmislite o ovom scenariju – Ako potvrdite okvir **Pošalji asinkronog MDN** i, navedite URL-a za slanje asinkrone MDN, a zatim poništite potvrdni okvir **Pošalji asinkronog MDN** ponovno na MDN i dalje šalju se u navedenom URL čak i ako nije odabrana mogućnost slanja asinkrone MDNs.  
Kao zaobilazno rješenje, poništite navedenom URL prije potvrdite okvir **Pošalji asinkronog MDN** i implementacija AS2 ugovor.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Znakove razmaka iza valjani interchange rezultirati porukom prazan slati krajnjoj suspend  
Ako postoje razmaci izvan programa IEA segment, u disassembler to smatra na kraj trenutnog razmjenu i izgleda na sljedeći niz razmaci kao sljedeću poruku. Jer to nije valjana interchange, možda Primijetit jedan uspješno poruka šalje usmjeravanje odredište, a jedan prazna poruka poslana krajnju točku suspend.  
### <a name="tracking-in-biztalk-services-portal"></a>Praćenje na portalu servisa BizTalk  
Praćenje događaja snimaju do obrade poruke uređivanje te na bilo kojem korelacije. Ako poruke ne uspije izvan protokol fazu, praćenje će se prikazati kao uspjelo. U tom slučaju potražite u odjeljku ZAPISNIK u odjeljku stupac **detalja** u **praćenja** detalje o pogrešci.
Na X12 primanja i slanja postavke ([Stvori upozorenje X12 ugovor Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689847.aspx)) pronaći informacije o fazi protokol.  

### <a name="update-agreement"></a>Ugovor za ažuriranje  
Na portalu servisa BizTalk omogućuje vam da biste izmijenili razdjelnik identitet kada je konfiguriran ugovor. To može rezultirati inconsistence svojstva. Na primjer, postoji ugovor pomoću ZZ:1234567 i ZZ:7654321 na razdjelnika. U odjeljku postavke profila Portal servisa BizTalk promijenite ZZ:1234567 biti 01:ChangedValue. Otvorite ugovor i 01:ChangedValue prikazuju se umjesto ZZ:1234567.
Da biste promijenili razdjelnik identitet, brisanje ugovor, ažurirajte **identiteta** u profilu partnera, a zatim ponovno stvorite ugovor.  
> AZURE. Upozorenje takvo ponašanje utječe X12 i AS2.  

### <a name="as2-attachments"></a>AS2 privitaka  
Privitke AS2 poruke nisu podržane u slati i primati. Konkretno, tihu zanemaruju privici i tijelo poruke obrađuje kao obični AS2 poruku.  
### <a name="resources-remembering-path"></a>Resursi: Plaćanju put  
Prilikom dodavanja **resursa**, prozoru dijaloškog okvira može zapamtiti put već koristili da biste dodali resurs. Zapamtiti put već koristili, pokušajte ga dodati Portal servisa BizTalk web-mjesta **Pouzdana web-mjesta** u pregledniku Internet Explorer.  
### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Ako je preimenovati entitet naziv most i zatvorite projekt bez spremanja promjena, ponovno otvoriti entitet rezultira pogreškom
Zamislite sljedećim redoslijedom:  
* Dodavanje most (na primjer u XML One-Way most) u projekt BizTalk servisa  

* Preimenovanje na most navođenjem vrijednosti za svojstvo entitet naziv. To preimenuje povezane .bridgeconfig datoteku pod nazivom koje ste naveli.  

* Zatvorite datoteku .bcs (zatvaranjem na kartici u Visual Studio) bez spremanja promjena.  

* Ponovno otvorite datoteku .bcs iz programa Explorer rješenja.  
Primijetit ćete da .bridgeconfig pridružene datoteke koju je novi naziv koje ste naveli, entitet naziv na površini za dizajniranje je i dalje stari naziv. Ako pokušate otvoriti most konfiguraciju tako da dvokliknete komponentu most, prikazat će se sljedeća pogreška:  
  "<old name>"Entitet je povezane datoteke"<old name>.bridgeconfig' ne postoji  
Da biste izbjegli izvodi u ovom scenariju, provjerite je li spremiti promjene nakon preimenovanja entiteti u programu project BizTalk servisa.  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>Sastavlja uspješno BizTalk servisa project čak i ako artefakt sadrži je izuzeti iz projekta za Visual Studio
Razmislite o scenarij u kojem dodate artefakt (na primjer, XSD datoteku) BizTalk servis projekt, uključite tu artefakt u konfiguraciji most (na primjer, navođenjem ga kao vrstu poruke zahtjev) i ga isključiti iz projekta za Visual Studio. U tom slučaju Stvaranje projekta ne steći sve pogreške dok god izbrisane artefakt dostupna je na disku na isto mjesto na kojem je uvršten u projekta za Visual Studio.
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>Project BizTalk servisa ne provjerava dostupnost sheme tijekom konfiguriranja na bridges
U projektu BizTalk servisa, ako sheme koja se dodaje u projekt uvozi drugu shemu projekta BizTalk servisa ne provjerava li uvezene sheme dodane u projekt. Kada pokušate da biste sastavili takav projekt, neće se pogrešaka u kompletu.
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>Poruka s odgovorom za zahtjev za odgovor most XML je uvijek skup znakova UTF-8
U ovom izdanju skup znakova odgovor poruke iz programa XML zahtjev za odgovor mosta uvijek postavljen je na UTF-8.
### <a name="user-defined-datatypes"></a>Korisnički definirane vrste podataka
Paket prilagodnik BizTalk prilagodnika unutar značajka BizTalk prilagodnik servisa mogu koristiti korisnički definirane vrste podataka za operacije prilagodnika.
Kada pomoću korisnički definiranih vrste podataka, kopirajte datoteke (.dll) pogon: \Programske Service\BAServiceRuntime\bin\ prilagodnika BizTalk ili da biste u globalnu predmemoriju sklopova (GAC) na poslužitelj za servis BizTalk prilagodnik servisa. U suprotnom sljedeća pogreška može pojaviti na klijentskom računalu:  
```<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <faultcode>s:Client</faultcode>
  <faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
  <detail>
    <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
    </AFConnectRuntimeFault>
  </detail>
</s:Fault> ```  
> [AZURE.IMPORTANT] Preporučuje se da biste koristili GACUtil.exe da biste instalirali datoteke u globalnu predmemoriju skupa. GACUtil.exe dokumenti kako koristiti ovaj alat i mogućnostima naredbenog retka za Visual Studio.  

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>Ponovno pokretanje BizTalk prilagodnik servisa Web-mjesta
Instalacije **Izvođenje servisa za BizTalk prilagodnik** * stvara na * *BizTalk prilagodnik servisa* * web-mjesto u IIS koja sadrži u * *BAService* * aplikacije.* *BAService** aplikacija interno koristi preusmjeravanja povezivanje da biste proširili razgovor krajnja točka servisa na lokaciji u oblak. Na servis koji se nalazi na lokalnom krajnju točku odgovarajuće preusmjeravanja će se registrirati na servis Bus samo pri pokretanju lokalnog servisa.  

Ako zaustavite i pokrenuti aplikaciju, konfiguraciju za automatsko pokretanje aplikacija se nije na snazi. Tako da kada se **BAService** Zaustavi, morate uvijek ponovno pokrenuti na web-mjestu **Servisa prilagodnik BizTalk** umjesto toga. Pokrenite ili zaustavite **BAService** aplikacije.
### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Posebni znakovi ne treba koristiti za adresu i entitet nazive LOB komponenti
Nemojte koristiti posebnih znakova za adresu i entitet nazive LOB komponente. Ako to učinite, će se pogreška pri implementaciji servisa BizTalk projekta. Za određene znakove kao što su "%", web-mjestu servisa prilagodnik BizTalk možda idite u prestao stanje i morat ćete ručno pokrenuti.
### <a name="test-map-with-get-context-property"></a>Testiranje kartu s svojstvo Get kontekst
Pretvaranjem sadrži kartu operacije **Dobiti svojstvo kontekst** , **Karte Test** neće uspjeti. Kao zaobilazno privremene zamijenite karte postupak **Dohvati svojstvo kontekst** niz Concatenate karte operacije koje sadrže podatke sustava. To će popunjavanje ciljne sheme i omogućuju testiranje druge funkcije pretvorbe.
### <a name="test-map-property-does-not-display"></a>Testiranje Mapiraj svojstvo prikaz
Svojstva **Mape za testiranje** ne prikazuju u Visual Studio. To se može dogoditi ako prozor **Svojstva** i prozora **Preglednika rješenja** su ne istodobno usidren. Da biste riješili taj problem, sidrenje **Svojstva** i windows **Explorer rješenja** .  
### <a name="datetime-reformat-drop-down-is-grayed-out"></a>DateTime preoblikovati padajućem zasivljena je
Kada DateTime preoblikovati postupak za kartu dodaje na površinu za dizajniranje i konfigurirali, padajući popis oblik možda biti zasivljene. To se može dogoditi ako je računalo prikaza postavljen **Srednje – 125%** ili **veće – od 150 posto**. Da biste riješili, postavite prikaz **manje – 100% (zadano)** pomoću sljedećih koraka:  
1. Otvorite **Upravljačku ploču** , a zatim kliknite **Izgled i personalizacija**.
2. Kliknite **Prikaz**.
3. Kliknite **manje – 100% (zadano)** , a zatim kliknite **Primijeni**.

Padajući popis **oblik** sada surađivati prema očekivanjima.
### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Dupliciranje ugovore na portalu servisa BizTalk
Zamislite sljedeće:
1. Stvorite ugovor pomoću trgovina API partnera OM za upravljanje.
2. Otvorite ugovor na portalu servisa BizTalk u dvije različite kartice.
3. Implementacija ugovor iz obje kartice.
4. Zbog toga oba ugovore implementiraju rezultira duplicirane stavke na portalu servisa BizTalk

**Zaobilazno rješenje**. Otvorite bilo koju od dupliciranih ugovore na portalu servisa BizTalk i poništiti uvođenje.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Bridges korištenje ažurirane potvrde čak i nakon ažurirane certifikat u spremištu artefakt
Razmislite o sljedećim scenarijima:  

**Scenarij 1: Korištenje utemeljen na otisak prsta certifikata za osiguravanje prijenos poruke s mosta krajnja točka servisa**  
Razmislite o scenarij u kojem koristite utemeljen na otisak prsta potvrde u projektu BizTalk servisa. Ažuriranje certifikata na portalu servisa BizTalk s istim nazivom, ali različite otisak prsta, ali ne ažuriraju sukladno tome projekta BizTalk servisa. U takvim scenariju na most mogu nastaviti obraditi poruke jer starije podatke o certifikatu možda još uvijek u predmemoriji za kanal. Nakon toga obrade poruke neće uspjeti.  

**Zaobilazno rješenje**: ažurirajte certifikat u programu project BizTalk servisa i ponovno implementirate projekta.  

**Scenarij 2: Korištenje naziv temelji ponašanja za prepoznavanje certifikata za osiguravanje prijenos poruke s mosta krajnja točka servisa**

Zamislite gdje se koristi naziv temelji ponašanja za prepoznavanje potvrde u projektu BizTalk servisa. Ažuriranje certifikata na portalu servisa BizTalk, ali ne ažuriraju sukladno tome projekta BizTalk servisa. U takvim scenariju na most mogu nastaviti obraditi poruke jer starije podatke o certifikatu možda još uvijek u predmemoriji za kanal. Nakon toga obrade poruke neće uspjeti.  

**Zaobilazno rješenje**: ažurirajte certifikat u programu project BizTalk servisa i ponovno implementirate projekta.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Bridges nastaviti da obradi poruke, čak i kad je izvan mreže SQL baze podataka
Bridges BizTalk servise i dalje obradu poruka neko vrijeme, čak i ako je izvan mreže Microsoft Azure SQL baze podataka (koji pohranjuje izvodi podatke kao što su distribuiranih artefakte i kanali). To je zato BizTalk Services koristi predmemorirani artefakte i konfiguraciji most.
Ako ne želite da se bridges da obradi poruke kada je izvan mreže SQL baze podataka, možete koristiti BizTalk Services PowerShell cmdleti možete zaustaviti ili odgoditi BizTalk servis. U odjeljku [Azure BizTalk servis za upravljanje uzorka](http://go.microsoft.com/fwlink/p/?LinkID=329019) za Windows PowerShell cmdleti za upravljanje operacije.  
### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>XML poruku unutar na most prilagođeni kod komponenta za čitanje sadrži znak dodatni Sastavnice
Zamislite mjesto na koje želite pročitati XML poruke unutar na most prilagođeni kod. Ako koristite System.Text.Encoding.UTF8.GetString(bytes) API .NET znak dodatni Sastavnice je uvršten u Izlaz na početku poruku. Tako, ako ne želite da se rezultat da biste uvrstili dodatne Sastavnice znak, morate koristiti ```System.IO.StreamReader().ReadToEnd()```.
### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Slanje poruka most pomoću WCF skaliranja
Poruke poslane most pomoću WCF skaliranja. Umjesto toga koristite HttpWebRequest ako želite da se skalabilni klijenta.
### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>NADOGRADNJA: Pogreške Token davatelja nakon nadogradnje s BizTalk servise za opće dostupnih (GA)
Je li uređivanje ili AS2 ugovor s aktivni serije. Kada servis BizTalk će se nadograditi iz pretpregleda GA, može se pojaviti na sljedeći način:
* Pogreška: Tokena davatelj nije uspio dati sigurnosni token. Tokena davatelja vraća poruka: naziv udaljenog nije moguće razriješiti.

* Obradu zadaci otkazuju.

**Zaobilazno rješenje**: nakon u BizTalk servis ažurira da biste opće dostupnih (GA), ponovno implementirate ugovor.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>NADOGRADNJA: Alatnog okvira prikazuje stare ikone most nakon nadogradnje usluge SDK BizTalk
Nakon nadogradnje starije verzije programa Services SDK BizTalk koji imali stare ikone koje predstavljaju na bridges alatnog okvira nastavlja prikazivati stare ikone za na bridges. Međutim, ako je most dodali na servis BizTalk projekta dizajnera površina, površina prikazuje nove ikone.  

**Zaobilazno rješenje**. Taj se problem možete riješiti brisanjem .tbd datoteka u odjeljku <system drive>: \Users\<korisnik > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>NADOGRADNJA: Ažuriranje BizTalk Portal iz pretpregleda da biste GA može prikazati pogreške koja označava da je mogućnost uređivanje nije dostupna
Ako ste prijavljeni na Portal servisa BizTalk dok usluge BizTalk nadogradili s pretpregled da biste GA, mogla bi vam se sljedeća pogreška na portalu:  

Ova mogućnost nije dostupna kao dio ovo izdanje sustava Microsoft Azure BizTalk Services. Da biste koristili te mogućnosti prijeđite na odgovarajući izdanje.  

**Razlučivost**: zapisnik odgovor na portal, zatvaranje i Otvori u pregledniku, a zatim Prijava na portal.  
### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>NADOGRADNJA: Nove podatke o praćenju neće se prikazati nakon nadogradnje BizTalk Services da biste GA
Pretpostavimo scenarij u kojem je instaliran na most XML implementiran BizTalk Services pretpregled pretplate na. Slanje poruke u most i odgovarajući praćenje podataka dostupna je na portalu servisa BizTalk. Sada su runtime bitova BizTalk Services Portal i servise BizTalk nadograditi GA, a poruku poslali isti most krajnju točku implementiran ranije, praćenje podataka neće se prikazivati za poruke poslane nakon nadogradnje.  

### <a name="pipelines-vs-bridges"></a>Kanali v/s Bridges
U ovom dokumentu termina 'kanali' i 'bridges' koriste naizmjence. Oba zapravo označavaju istu stvar, koji je implementiran na servisima BizTalk jedinica obrada poruke.  

### <a name="concepts"></a>Koncepti  

[BizTalk Services](https://msdn.microsoft.com/library/azure/hh689864.aspx)   
