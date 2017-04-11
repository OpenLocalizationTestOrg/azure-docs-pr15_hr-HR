<properties 
    pageTitle="Konfiguriranje nadzorne ploče, nadzor, mjerilo, i veze hibridnog BizTalk Services | Microsoft Azure" 
    description="Dodatne informacije o kontrole i praćenje performansi na karticama klasični portala za servise BizTalk: nadzorne ploče, Monitor, mjerilo, konfiguriranje i hibridnog veze. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="mandia"/>




# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Pregled kartice nadzorne ploče, Monitor, mjerilo, konfiguriranje i hibridnog veze

Nakon što stvorite BizTalk usluge i implementacija aplikacije, možete promijeniti neke postavke servisa BizTalk i praćenje performansi aplikacije. 

Prilikom otvaranja Azure klasični portal se postavljaju automatski na kartici **Sve stavke** . Da biste pogledali na servisu BizTalk, odaberite servis za BizTalk na kartici **Sve stavke** ili odaberite karticu **SERVISI BIZTALK** ; a zatim odaberite naziv BizTalk servisa.

Time se otvara novi prozor sa sljedećim karticama. U ovoj se temi opisuju karticama.

## <a name="quick-start-quick-startquickstart"></a>Brzi početak rada (![Brzi početak rada][QuickStart])
Ovisno o BizTalk Services Edition sve mogućnosti koje su navedene možda neće biti dostupne. 
<table border="1">
    <tr>
        <td><strong>Početak alata</strong></td>
        <td>Preuzmite Services SDK BizTalk da biste instalirali predložaka za projekt programa Visual Studio na računalu lokalnog razvoj. Te predloške stvorite <strong>BizTalk Services</strong> (most) i projektima Visual Studio <strong>BizTalk servis artefakte</strong> (pretvorba) koji su raspoređeni u funkcioniranju servisa BizTalk.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Kako pokrenuti pomoću Azure BizTalk Services SDK</a> i <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">instalirate Azure BizTalk Services SDK</a> navedeni koraci za početak rada.
        </td>
    </tr>
    <tr>
        <td><strong>Stvaranje partnera ugovori</strong></td>
        <td>Portal servisa BizTalk Azure hostirane na Azure gdje dodavanje partnera i stvaranje X12, AS2, otvorit će se i uređivanje EDIFACT ugovori.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfiguriranje komponente za uređivanje poruka na portalu servisa BizTalk</a> navedeni koraci za početak rada.
        </td>
    </tr>

<tr>
        <td><strong>Dodatne informacije o servisima BizTalk</strong></td>
        <td>Idite u <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">Centar za učenje</a> da biste saznali više o uslugama BizTalk Azure.</td>
</tr>
</table>


Na traci sa zadacima pri dnu možete učiniti sljedeće:

<table border="1">

<tr>
<td><strong>Upravljanje</strong> implementaciju sustava aplikacije</td>
<td>Otvorit će se na portalu servisa Azure BizTalk Services. Na portalu servisa BizTalk je ulaska konfiguraciju za uređivanje, uključujući dodavanje partnera i stvaranje X12, AS2, a EDIFACT ugovore.
<br/><br/>
To je ista kao <strong>ugovore partnera za stvaranje</strong> na kartici <strong>Brzi početak rada</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfiguriranje komponente za uređivanje poruka na portalu servisa BizTalk</a> navedene su dodatne informacije na portalu servisa BizTalk.</td>
</tr>

<tr>
<td><strong>Informacije o vezi</strong> prostora za naziv kontrole programa Access</td>
<td>Kad odaberete podatke o vezi, zatim prostora za naziv kontrole programa Access, zadani izdavač i zadani ključ prikazuju. Možete kopirati te vrijednosti.
<br/><br/>
Možete otvoriti i portala za kontrolu pristupa. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Stvaranje kontrola pristupa prostor naziva</a> navedene su dodatne informacije na portalu za kontrolu pristupa.</td>
</tr>

<tr>
<td><strong>Sinkronizacija tipke</strong> na računu za pohranu</td>
<td>Kada stvorite račun za pohranu, primarni ključ i sekundarne ključ automatski stvaraju. Ove ključeva za šifriranje kontrolirati pristup računu za pohranu. Servis za BizTalk automatski koristi primarni ključ. <strong>Sinkronizacija tipke</strong> omogućuju korisnicima prebacivanje između primarni ključ i sekundarne ključ ne prekida BizTalk servisa.
<br/><br/>
Na primjer, želite BizTalk servisa da biste koristili novi primarni ključ za račun za pohranu. Da biste to učinili:
<br/><br/>
<ol>
<li>Odaberite servis za BizTalk i <strong>Sinkronizaciju tipke</strong>. Odaberite sekundarni ključ. Kada to učinite, servis BizTalk pokreće korištenje sekundarnog ključa.</li>
<li>Azure klasični portalu odaberite svoj račun za pohranu i Obnovi primarni ključ. Imajte na umu usluge BizTalk koristi tipku sekundarne.</li>
<li>Odaberite servis za BizTalk i <strong>Sinkronizaciju tipke</strong>. Sada odaberite primarni ključ. Ovo je novi primarni ključ koji regenerated.</li>
<li>Azure klasični portalu odaberite svoj račun za pohranu i Obnovi sekundarne ključ.</li>
</ol>
<br/>
To se naziva "dinamične tipke". Svrha je omogućiti korisnicima prebacivanje između primarni ključ i sekundarne ključ ne prekida BizTalk servis.</td>
</tr>

<tr>
<td><strong>Brisanje</strong> aplikacije</td>
<td>Kad odaberete brisanje BizTalk usluge i uklanjaju se sve stavke u uveden u nju.</td>
</tr>
</table>


## <a name="dashboard"></a>Nadzorna ploča
Ovisno o BizTalk Services Edition sve mogućnosti koje su navedene možda neće biti dostupne. 

Kada odaberete naziv BizTalk servisa, prikazuje se kartica nadzorna ploča. Na nadzornoj ploči možete učiniti sljedeće:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Pregled korištenja: Prikazuje broj korištenih hibridnog veze
Prikazuje u GB i korištenja podataka. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Metričkim grafikonu: Prikazuje nepromjenjivi popis metriku performansi
Ove metriku navedite u stvarnom vremenu vrijednosti vezane uz stanje servisa BizTalk. Možete odabrati i **relativne** i **apsolutne** vrijednosti i vremenski raspon **Interval** metriku koje su prikazane u grafikonu. 

Opis te metriku performanse, idite na [Dostupne metriku](#Metrics) u ovoj temi.


##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Brzi pogled: Navodi svojstva BizTalk servisa

<table border="1">

<tr>
<td><strong>Ažurirajte vjerodajnice za bazu podataka za praćenje</strong></td>
<td>Promjena korisničkog imena i lozinke koje se koriste za prijavu u bazu podataka za praćenje.</td>
</tr>
<tr>
<td><strong>Ažuriranje SSL certifikata</strong></td>
<td>Možete ažurirati BizTalk servisa da biste koristili različite SSL certifikata. Samopotpisani certifikat SSL automatski stvara <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Stvaranje BizTalk servisa</a>.</td>
</tr>
<tr>
<td><strong>Preuzmite certifikat</strong></td>
<td>Možete preuzeti SSL certifikat koji se koristi na servisu BizTalk na lokalno računalo.</td>
</tr>
<tr>
<td><strong>Status</strong></td>
<td>Prikazuje trenutni status servisa BizTalk. U odjeljku <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk usluge: grafikona stanje servisa</a>. </td>
</tr>
<tr>
<td><strong>URL servisa</strong></td>
<td>URL servisa za BizTalk. To je ista <strong>URL domene</strong> unesene stvaranja BizTalk usluge.</td>
</tr>
<tr>
<td><strong>Adresa javnog virtualne IP (VIP)</strong></td>
<td>IP adresa dodijeljena BizTalk usluge. Koristi se za sve krajnje točke unosa i je izvorna adresa za izlazni promet. U ovom IP adresa pripada usluge BizTalk dok god je stvoren. Ako izbrišete servis BizTalk, IP adresa je dodijeljen drugih servisa za BizTalk.</td>
</tr>
<tr>
<td><strong>Prostor za naziv ACS</strong></td>
<td>Potvrđuje sa servisom BizTalk.</td>
</tr>
<tr>
<td><strong>Izdanje</strong></td>
<td>Popisi izdanje unijeli prilikom stvaranja servisa BizTalk.</td>
</tr>
<tr>
<td><strong>Mjesto</strong></td>
<td>Prikazuje regiji koji hostira vaša BizTalk usluga.</td>
</tr>
<tr>
<td><strong>Stvorili</strong></td>
<td>Prikazuje datum i vrijeme stvaranja BizTalk servisa.</td>
</tr>
<tr>
<td><strong>Praćenje baze podataka</strong></td>
<td>Naziv baze podataka SQL Azure kojoj su pohranjeni tablica za praćenje servis za BizTalk koristi. 
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Objašnjenje preduvjeti</a> sadrži detalje na bazu podataka za praćenje.</td>
</tr>
<tr>
<td><strong>Nadzor/arhiviranje prostora za pohranu</strong></td>
<td>Naziv računa spremišta Azure kojoj su pohranjeni nadzora izlaz BizTalk servisa.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Objašnjenje preduvjeti</a> sadrži detalje o računu za pohranu.</td>
</tr>
<tr>
<td><strong>Naziv pretplate</strong></td>
<td>Popis pretplatu u koju hostira BizTalk usluge. Pretplata određuje pristup portalu Azure klasični.</td>
</tr>
<tr>
<td><strong>ID pretplate</strong></td>
<td>Nakon stvaranja pretplatu, automatski se generira ID pretplate. Prilikom korištenja REST API-ji, možda ćete morati unijeti ID pretplate.</td>
</tr>
</table>

[BizTalk usluge: dodjele resursa Azure pomoću portala za klasični](http://go.microsoft.com/fwlink/p/?LinkID=302280) navedeni koraci za stvaranje BizTalk servisa.


##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Upravljanje podatke o vezi na tipke sinkronizaciju i brisanje na traci zadataka:

<table border="1">

<tr>
<td><strong>Upravljanje</strong> implementaciju sustava aplikacije</td>
<td>Otvorit će se na Portal servisa Azure BizTalk. Na portalu servisa BizTalk je ulaska konfiguraciju za uređivanje, uključujući dodavanje partnera i stvaranje X12, AS2, a EDIFACT ugovore.
<br/><br/>
To je ista kao <strong>ugovore partnera za stvaranje</strong> na kartici <strong>Brzi početak rada</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfiguriranje komponente za uređivanje poruka na portalu servisa BizTalk</a> navedene su dodatne informacije na portalu servisa BizTalk.</td>
</tr>
<tr>
<td><strong>Informacije o vezi</strong> prostora za naziv kontrole programa Access</td>
<td>Prikazuje prostora za naziv kontrole programa Access, zadani izdavač i ključ zadane vrijednosti; koje se mogu kopirati.
<br/><br/>
Možete otvoriti i portala za kontrolu pristupa. Ovaj Portal za kontrolu pristupa je isti kao koristeći servisa Active Directory u lijevom navigacijskom oknu.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Upravljanje prostor naziva za vaše ACS</a> navedene su dodatne informacije na portalu za kontrolu pristupa.</td>
</tr>
<tr>
<td><strong>Sinkronizacija tipke</strong> na računu za pohranu</td>
<td>Kada stvorite račun za pohranu, primarni ključ i sekundarne ključ automatski stvaraju. Ove ključeva za šifriranje kontrolirati pristup računu za pohranu. Servis za BizTalk automatski koristi primarni ključ. <strong>Sinkronizacija tipke</strong> omogućuju korisnicima prebacivanje između primarni ključ i sekundarne ključ ne prekida BizTalk servisa.
<br/><br/>
Na primjer, želite BizTalk servisa da biste koristili novi primarni ključ za račun za pohranu. Da biste to učinili:
<br/><br/>
<ol>
<li>Odaberite servis za BizTalk i <strong>Sinkronizaciju tipke</strong>. Odaberite sekundarni ključ. Kada to učinite, servis BizTalk pokreće korištenje sekundarnog ključa.</li>
<li>Azure klasični portalu odaberite svoj račun za pohranu i Obnovi primarni ključ. Imajte na umu usluge BizTalk koristi tipku sekundarne.</li>
<li>Odaberite servis za BizTalk i <strong>Sinkronizaciju tipke</strong>. Sada odaberite primarni ključ. Ovo je novi primarni ključ koji regenerated.</li>
<li>Azure klasični portalu odaberite svoj račun za pohranu i Obnovi sekundarne ključ.</li>
</ol>
<br/>
To se naziva "dinamične tipke". Svrha je omogućiti korisnicima prebacivanje između primarni ključ i sekundarne ključ ne prekida BizTalk servis.</td>
</tr>

<tr>
<td><strong>Brisanje</strong> aplikacije</td>
<td>Servis za BizTalk i sve stavke u uveden u nju se uklanjaju.</td>
</tr>
</table>


## <a name="monitor"></a>Monitora
Primjena besplatne Edition.

Kada odaberete naziv BizTalk servisa, na kartici Monitor je dostupna i prikazuje sljedeće:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Metričkim grafikonu: Prikazuje metriku odabrane performansi
Ove metriku navedite u stvarnom vremenu vrijednosti vezane uz stanje servisa BizTalk. Odaberite koje metriku performanse prikazuju. Maksimalno šest metriku performanse mogu se prikazati istodobno. 

Možete odabrati i **relativne** i **apsolutne** vrijednosti i vremenski raspon **Interval** metrike koji se prikazuju. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Da biste uklonili ili prikaz metriku u grafikonu:
1. Odaberite karticu **Monitor** .
2. Odaberite **Dodaj metriku** na traci zadataka:  
![Odaberite Dodaj mjerenja][AddMetrics]
3. Provjerite performanse metrike koju želite prikazati.
4. Odaberite kvačicu da biste se vratili na kartici **Monitor** .
5. Odaberite kruga uz metriku da biste prikazali taj mjerenje vrijednosti u grafikonu.  

    Na primjer, metriku **Procesora** zasivljena je; rezultat ne prikazuju se u grafikonu sustava:  
![Metrika procesora zasivljena je][GrayedMetric]  

    Odaberite na sivih out krug da biste omogućili metriku **Procesora koristi** da bi se prikazao rezultat u grafikonu:  
![Metrika procesora omogućena][EnabledMetric]

6. Da biste uklonili metrike grafikonu prikaza i na popisu, odaberite **Izbriši metriku** na programskoj traci. Da biste dodali metričkim natrag na popisu, odaberite **Dodaj metriku** programske trake, provjerite metriku i odaberite kvačicu da biste se vratili na kartici **Monitor** . Odaberite na sivih out krug da biste omogućili metriku.

## <a name="Metrics"></a>Dostupna mjerenja
Dostupne su sljedeće mjerača/metriku performanse:

<table border="1">

<tr>
<td><strong>Latencija RountdTrip</strong></td>
<td>Prikazuje Prosječno vrijeme u milisekundama (ms) za obradu poruke od vremena primio poruku dok je servis BizTalk potpuno obraditi poruku kroz sve bridges. Broje se samo poruke koje su uspješno obrađen.<br/><br/>
Prilikom sljedećeg događaja, stvara se vremenske oznake:
<ul>
<li>Poruka unosi pristupnika</li>
<li>Poruka je usmjerena na odredište</li>
<li>Odgovor odredište primitku</li>
<li>Odredište potvrđivanje odgovora poslane pristupnika</li>
</ul>
<br/>
Ova metrika prikazuje rezultat izračuna za sljedeće:
<br/><br/>
[Odredište potvrđivanje pošalje odgovor s pristupnikom] - [poruke unosi pristupnika]</td>
</tr>
<tr>
<td><strong>Do neuspjele izvoru</strong></td>
<td>Prikazuje ukupni broj poruka koja se nije uspjela servis BizTalk kada izvlačenja poruke od krajnje točke izvora.</td>
</tr>
<tr>
<td><strong>Korištenje središnjih procesora</strong></td>
<td>Prikazuje prosječnu % procesor vrijeme sve instance uloga.</td>
</tr>
<tr>
<td><strong>Latencija obrada</strong></td>
<td>Prikazuje Prosječno vrijeme u milisekundama (ms) za obradu poruku pomoću usluge BizTalk kroz sve bridges bez vrijeme utrošeno na odredišta. Broje se samo poruke koje su uspješno obrađen.<br/><br/>
Prilikom svakog od sljedećih događaja, stvara se vremenske oznake:

<ul>
<li>Poruka unosi pristupnika</li>
<li>Poruka je usmjerena na odredište</li>
<li>Primljen odgovor koji odredište</li>
<li>Odredište potvrđivanje odgovor šalje pristupnika</li>
</ul>
<br/>Ova metrika prikazuje rezultat izračuna za sljedeće:<br/><br/>
[Odredište potvrđivanje pošalje odgovor s pristupnikom] - [poruke unosi pristupnika] - [odredište je odaziv primljen] + [poruka je usmjerena na odredište]</td>
</tr>
<tr>
<td><strong>Do neuspjele u tijeku</strong></td>
<td>Prikazuje ukupni broj poruka koja se nije uspjela tijekom obrade servis BizTalk preko svih bridges unutar vremenskog intervala.</td>
</tr>
<tr>
<td><strong>Poruke poslane</strong></td>
<td>Prikazuje ukupni broj poruke poslane servis BizTalk preko svih bridges unutar vremenskog intervala. Ova metrika se povećava kada poruku poslao na kanal dosegne usmjeravanje odredište. Ova metrika označiti poruku u uspješno obrađen.<br/><br/>
U scenariju s zahtjev za odgovor metriku se povećava prilikom odredište usmjeravanje šalje potvrda potvrđivanje natrag kanal.</td>
</tr>
<tr>
<td><strong>Poruke koje ste primili</strong></td>
<td>Prikazuje ukupni broj poruka BizTalk servis primio preko svih bridges unutar vremenskog intervala. Ova metrika se povećava se po primitku nove poruke tako da kanal.</td>
</tr>
<tr>
<td><strong>Poruka u tijeku</strong></td>
<td>Prikazuje ukupni broj poruka koji se trenutno obrađuje servis BizTalk unutar vremenskog intervala.</td>
</tr>
<tr>
<td><strong>Poruke koje se obrađuju</strong></td>
<td>Prikazuje ukupni broj poruka uspješno obradili servis BizTalk preko svih bridges unutar vremenskog intervala. Ova metrika se povećava kada poruka je uspješno primio kanal i uspješno usmjeriti na odredište.</td>
</tr>
</table>


## <a name="scale"></a>Promjena veličine
Na kartici skaliranje možete dodati ili oduzimanje broj jedinica servis za BizTalk koristi. Prema zadanim postavkama, postoji jedinica konfigurirana. Za promjenu veličine na servisu BizTalk možete dodati dodatne jedinicama. Kada povećate skale se povećava propusnost. Iznos resurse i povećava, uključujući distribuiranih bridges, ugovore, LOB veze i obradu power. Ako, na primjer, povećajte skale iz 1 jedinice 2 jedinice. U tom slučaju možete uvesti dvostruki broj bridges, dvostruki ugovore, dvostruke veze LOB i dvostruki power obrada.

Neka vam izdanja BizTalk nude skaliranje mogućnost. U tom slučaju jedinice je dopušteno. Da biste odredili koliko je jedinica vaše izdanje možete neproporcionalno, pogledajte [BizTalk usluge: izdanja grafikona](biztalk-editions-feature-chart.md).

Povećava broj jedinica mogu utjecati na cijene. Ako povećate jedinice, odaberete **Spremanje** prikazuje poruku koja kaže da ako naplatu utjecati. Zatim odaberite Dalje. Kada povećavate broj jedinica, BizTalk servis stanja promjene iz Aktivno ažuriranje. U stanju ažuriranje na servisu BizTalk nastavlja se izvoditi.

[BizTalk usluge: izdanja grafikona](biztalk-editions-feature-chart.md) definira "Jedinica".


## <a name="configure"></a>Konfiguriranje
Primijenite hibridnog veze.

Sigurnosno kopiranje Status postavlja ništa ili automatsku. Kada je postavljeno na ništa, bez sigurnosne kopije se automatski stvaraju. Kada je postavljeno na automatski, konfigurirati mjesto za sigurnosne kopije, učestalost sigurnosnog kopiranja, a koliko da biste zadržali datoteke sigurnosne kopije. 

[BizTalk usluge: sigurnosnog kopiranja i vraćanja](biztalk-backup-restore.md) sadrži detalje. 


## <a name="HybridConnections"></a>Hibridno veze
Veza za hibridno povezivanje Azure aplikaciju, kao što je web-aplikacijama ili mobilne aplikacije u Azure aplikacije servisa programa lokalnog resursa koji koristi statički TCP priključak, kao što su SQL Server, MySQL, HTTP Web API-ji i većina prilagođene web-servisi. Hibridno veze upravlja se BizTalk servisa Azure klasični portalu.

Da biste stvorili veze hibridnog u servisu Azure aplikacije, potražite u članku [pristup lokalnog resursa pomoću hibridnog veze u aplikacije servisa za Azure](../app-service-web/web-sites-hybrid-connection-get-started.md).

Stvaranje i upravljanje vezama hibridnog Azure BizTalk Services, potražite u članku [Hibridno veze](integration-hybrid-connection-overview.md).



## <a name="next"></a>Sljedeći
Sad kad ste upoznati s različitim karticama, možete saznati više o značajkama servisa Azure BizTalk:

- [BizTalk usluge: ograničavanje](biztalk-throttling-thresholds.md)  
- [BizTalk servisa: Naziv izdavača i izdavatelj ključ](biztalk-issuer-name-issuer-key.md)  
- [BizTalk servisa: Sigurnosno kopiranje i vraćanje](biztalk-backup-restore.md)

## <a name="see-also"></a>Vidi također
- [Hibridno veze](integration-hybrid-connection-overview.md)  
- [BizTalk servisa: Za razvojne inženjere, Basic, standardna i Premium izdanjima grafikona](biztalk-editions-feature-chart.md)  
- [BizTalk servisa: Azure pomoću portala za klasični dodjele resursa](biztalk-provision-services.md)  
- [Servisi za BizTalk: Stanje servisa BizTalk grafikona](biztalk-service-state-chart.md)  
- [Kako pokrenuti pomoću Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[QuickStart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png
 
