<properties
    pageTitle="Stvaranje web-servisa Azure BizTalk Services na portalu za Azure | Microsoft Azure"
    description="Upute za dodjelu resursa i stvaranje Azure BizTalk servisa na portalu Azure; MABS, WABS"
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
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>



# <a name="create-biztalk-services-using-the-azure-portal"></a>Stvaranje BizTalk servisa pomoću portala za Azure

Stvaranje web-servisa Azure BizTalk Services na portalu za Azure.

> [AZURE.TIP] Upute za prijavu na portal sustava Azure potreban je račun za Azure i Azure pretplate. Ako nemate račun, možete stvoriti pomoću računa za probno razdoblje za nekoliko minuta. U odjeljku [Azure besplatnu probnu verziju](http://go.microsoft.com/fwlink/p/?LinkID=239738).

## <a name="create-a-biztalk-service"></a>Stvaranje BizTalk servisa
Ovisno o izdanju odaberete, sve postavke servisa BizTalk možda biti dostupne.

1. Prijavite se na [portal za Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. U navigacijskom oknu dolje odaberite **NOVO**:  
![Odaberite gumb novo][NEWButton]

3. Odaberite **Aplikaciju usluge** > **BIZTALK servis** > **PRILAGOĐENE stvaranje**:  
![Odaberite BizTalk servis, a zatim odaberite Stvori prilagođene][NewBizTalkService]

4. Unesite postavke BizTalk servisa:

    <table border="1">
    <tr>
    <td><strong>Naziv BizTalk usluge</strong></td>
    <td>Možete unijeti bilo koje ime, ali moguće određenim. Primjeri obuhvaćaju sljedeće:<br/><br/>
    <em>mycompany</em>. biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>. biztalk.windows.net<br/>
    <em>myapplication</em>. biztalk.windows.net<br/><br/>". biztalk.windows.net" automatski se dodaju ime koje unesete. Time ste stvorili URL-a koji se koristi da biste pristupili servisu BizTalk poput <strong>https://<em>myapplication</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Izdanje</strong></td>
    <td>Ako ste u fazi testiranje/razvoj, odaberite <strong>za razvojne inženjere</strong>. Ako ste u fazi proizvodnje, koristite na <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk usluge: izdanja grafikona</a> za utvrđivanje <strong>Premium</strong>, <strong>standardnih</strong>ili <strong>Osnovni</strong> točan izbor za vašu situaciju tvrtke.
    </td>
    </tr>
    <tr>
    <td><strong>Regija</strong></td>
    <td>Odaberite regiji za hostiranje na servisu BizTalk.</td>
    </tr>
    <tr>
    <td><strong>URL domene</strong></td>
    <td><strong>Nije obavezno</strong>. Po zadanom je URL domene <em>YourBizTalkServiceName</em>. biztalk.windows.net. Također možete unijeti naziv prilagođene domene. Na primjer, ako je vaša domena <em>contoso</em>, možete unijeti: <br/><br/>
    <em>MyCompany</em>. contoso.com<br/>
    <em>MyCompanyMyApplication</em>. contoso.com<br/>
    <em>MyApplication</em>. contoso.com<br/>
    <em>YourBizTalkServiceName</em>. contoso.com<br/>
    </td>
    </tr>
    </table>
Odaberite strelicu sljedeće.

5. Unesite prostora za pohranu i baze podataka postavke:

    <table border="1">
    <tr>
    <td><strong>Račun za pohranu za nadzor i arhiviranja</strong></td>
    <td>Odaberite postojeći račun za pohranu ili stvorite novi račun za pohranu. <br/><br/>Ako stvorite novi račun za pohranu, unesite <strong>Naziv računa za pohranu</strong>.</td>
    </tr>
    <tr>
    <td><strong>Praćenje baze podataka</strong></td>
    <td>Ako koristite postojeću bazu podataka SQL Azure, nije moguće koristiti neki drugi servis BizTalk. Potrebno je ime za prijavu i lozinka kada unesete poslužitelj baze podataka SQL Azure stvorena.<br/><br/><strong>Savjet</strong> Stvorite bazu podataka za praćenje i račun za pohranu praćenja i arhiviranja u području isti kao BizTalk servis.</td>
    </tr>
    </table>
Odaberite strelicu sljedeće.

6. Unesite postavki baze podataka:

    <table border="1">
    <tr>
    <td><strong>Ime</strong></td>
    <td>Dostupna je kada odaberete <strong>Stvori novu instancu SQL baze podataka</strong> na prethodni zaslon.
    <br/><br/>
   Unesite naziv baze podataka SQL se može koristiti u servisu BizTalk.</td>
    </tr>
    <tr>
    <td><strong>Poslužitelj</strong></td>
    <td>Dostupna je kada odaberete <strong>Stvori novu instancu SQL baze podataka</strong> na prethodni zaslon.
    <br/><br/>
   Odaberite postojeću SQL Server za bazu podataka ili stvorite nove baze podataka SQL server.</td>
    </tr>
    <tr>
    <td><strong>Ime za prijavu poslužitelja</strong></td>
    <td>Unesite korisničko ime za prijavu.</td>
    </tr>
    <tr>
    <td><strong>Lozinka za prijavu poslužitelja</strong></td>
    <td>Unesite lozinku za prijavu.</td>
    </tr>
    <tr>
    <td><strong>Regija</strong></td>
    <td>Dostupna je kada je odabrana <strong>Stvori novu instancu SQL baze podataka</strong> . Odaberite regiji za hostiranje SQL baze podataka.</td>
    </tr>
    </table>

Odaberite potvrdni okvir da biste dovršili Čarobnjak. Pojavit će se ikona tijeku:  
![Ikona tijeku prikazuje po dovršetku][ProgressComplete]

Po dovršetku BizTalk servisa Azure su stvorene i jeste li spremni za aplikacije. Zadane postavke nisu potrebne. Ako želite da biste promijenili zadane postavke, odaberite **BIZTALK SERVISA** u lijevom navigacijskom oknu, a zatim BizTalk usluge. Dodatne postavke prikazuju se na [karticama nadzorne ploče, Monitor, i promjena veličine](biztalk-dashboard-monitor-scale-tabs.md) na vrhu.

Ovisno o stanju servisa BizTalk, postoje neka operacije koje nije moguće. Za popis te operacije, idite na [BizTalk servisa stanja grafikona](biztalk-service-state-chart.md).


## <a name="post-provisioning-steps"></a>Nakon dodjele resursa korake

-  [Instalirati certifikat na lokalnom računalu](#InstallCert)
-  [Dodavanje podrška za proizvodnju certifikata](#AddCert)
-  [Dohvaćanje naziva kontrola pristupa](#ACS)

#### <a name="InstallCert"></a>Instalirati certifikat na lokalnom računalu
Kao dio BizTalk servis dodjele resursa, samopotpisani certifikat se stvara i povezanog s pretplatom na BizTalk servisa. Morate preuzeti ovaj certifikat i instalirajte ga na računalima s kojima implementacija BizTalk servisne aplikacije ili slanje poruka na krajnjoj točki BizTalk servisa.

1. Prijavite se na [portal za Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Odaberite **BIZTALK SERVISA** u lijevom navigacijskom oknu, a zatim pretplate BizTalk servisa.
3. Odaberite karticu **nadzorne ploče** .
4. Odaberite **Preuzimanje SSL certifikata**:  
![Izmjena SSL certifikata][QuickGlance]
5. Dvokliknite certifikat i pokrenite slijediti čarobnjak i instalirati certifikat. Provjerite je li instalirati certifikat u spremištu **Pouzdanih korijenskih certifikata za izdavanje certifikata** .

#### <a name="AddCert"></a>Dodavanje podrška za proizvodnju certifikata
Samopotpisani certifikat koji se automatski stvara prilikom stvaranja servisa BizTalk namijenjen za korištenje u okruženju za razvoj samo. Za scenarije radnog zamijeniti ga podrška za proizvodnju certifikat.

1. Na kartici **nadzorne ploče** , odaberite **Update SSL certifikata**.
2. Pronađite svoje privatne SSL certifikat (*CertificateName*.pfx) koji sadrži naziv BizTalk servisa, unesite lozinku i kliknite kvačicu.

#### <a name="ACS"></a>Dohvaćanje naziva kontrola pristupa

1. Prijavite se na [portal za Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Odaberite **BIZTALK SERVISA** u lijevom navigacijskom oknu, a zatim na servisu BizTalk.
3. Na programskoj traci odaberite **Podatke o vezi**:  
![Odaberite podatke o vezi][ACSConnectInfo]

4. Kopirajte vrijednosti za kontrolu pristupa.

Ako pokrenete servis BizTalk projekta iz Visual Studio, unesite imenski kontrola pristupa. Prostor naziva za kontrolu pristupa automatski se stvara BizTalk servisa.

Kontrola pristupa vrijednosti može se koristiti s bilo kojom aplikacijom. Prilikom stvaranja servisa Azure BizTalk imenski kontrola pristupa kontrole provjere autentičnosti u svojoj implementaciji BizTalk servisa. Ako želite da biste promijenili pretplata ili upravljanje naziva, **Servisa ACTIVE DIRECTORY** u lijevom navigacijskom oknu odaberite i odaberite vaš prostor naziva. Na programsku traku popis mogućnosti.

Kliknite **Upravljanje** otvara Portal za upravljanje kontrole programa Access. Na portalu za upravljanje kontrolu pristupa servis BizTalk koristi **identiteta servisa**:  
![Identiteti ACS servisa u pristup kontrolirati Portal za upravljanje][ACSServiceIdentities]

Kontrola pristupa identiteta servisa je skup vjerodajnica koje omogućuju aplikacije ili klijenata za provjeru autentičnosti izravno s kontrola pristupa i primanje token.

> [AZURE.IMPORTANT]Servis BizTalk koristi **vlasnik** zadanog identiteta servisa i vrijednost **lozinke** . Ako koristite vrijednost simetričnu ključ umjesto vrijednost lozinke, može doći do sljedeće pogreške.<br/><br/>*Nije mogao povezati s računom servisa za upravljanje kontrole programa Access s navedene vjerodajnice*

[Upravljanje prostor naziva za vaše ACS](https://msdn.microsoft.com/library/azure/hh674478.aspx) navedeni smjernice i preporuke.

## <a name="requirements-explained"></a>Preduvjeti za objašnjenje

Tim preduvjetima ne primjenjuju se na besplatnu Edition.
<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Što vam je potrebno</strong></td>
        <td><strong>Zašto je potrebna</strong></td>
</tr>
<tr>
<td>Azure pretplate</td>
<td>Pretplata određuje tko možete se prijaviti na portal sustava Azure. Vlasnik računa stvara pretplate na <a HREF="https://account.windowsazure.com/Subscriptions">Azure pretplate</a>.
<br/><br/>
Račun za Azure može imati više pretplata, a njome se upravlja svatko tko je dopušteno. Ako, na primjer, vaš račun za Azure vlasnik stvara pretplatu pod nazivom <em>BizTalkServiceSubscription</em> i daje administratori BizTalk unutar tvrtke (Ako, na primjer, ContosoBTSAdmins@live.com) pristup ove pretplate. U ovom scenariju administratori BizTalk prijavite se na portal za Azure te kako preuzeti cijeli administratorske ovlasti na glavnom računalu servise za pretplatu, uključujući Azure BizTalk usluge. Administratori BizTalk nisu vlasnici račun za Azure i stoga nemaju pristup podatke o naplati.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Upravljanje pretplatama i račune za pohranu na portalu za Azure</a> navedene su dodatne informacije.
</td>
</tr>
<tr>
<td>Baze podataka Azure SQL</td>
<td>Sprema tablicama, prikazima i pohranjene procedure koristi BizTalk servisa, uključujući podatke za praćenje.
<br/><br/>
Kada stvorite BizTalk servis, možete koristiti postojećim Azure SQL Server, baze podataka SQL Azure, ili automatski stvoriti novi poslužitelja ili baze podataka.
<br/><br/>
Skaliranje baze podataka SQL automatski konfigurirana. Obično je zadana mjera potrebne za uslugu BizTalk. Promjena skale utječe cijene. U odjeljku <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">računi i naplata u bazi podataka Azure SQL</a>
<br/><br/>
<strong>Bilješke</strong>
<br/>
<ul>
<li> Kada stvorite novu Azure SQL Server i bazu podataka, servisa Azure je automatski omogućena. Servis BizTalk zahtijeva servisa Azure biti omogućene.</li>
<li>Ako stvorite novu bazu podataka SQL Azure na postojećim Azure SQL Server, pravila vatrozida poslužitelja neće se promijeniti. Zbog toga moguće je drugih servisa Azure nije dopušteno pristupa na poslužitelju baze podataka.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure kontrola pristupa prostor naziva</td>
<td>Potvrđuje pomoću servisa Azure BizTalk. Ako pokrenete servis BizTalk projekta iz Visual Studio, unesite imenski kontrola pristupa. Kada stvorite BizTalk servisa, automatski se stvara prostora za naziv kontrole pristupa.</td>
</tr>

<tr>
<td>Račun za Azure prostora za pohranu</td>
<td>Daje pristup tablica, blob-ova i redova servis za BizTalk koristi da biste spremili na sljedeći način:

<ul>
<li>Datoteka zapisnika koje praćenje BizTalk servis. Nadzor izlaz prikazuje se na kartici **praćenja** na portalu za Azure.</li>
<li>Prilikom stvaranja programa X12 ili AS2 ugovor između partnera, možete omogućiti značajku arhiviranje za pohranu svojstva poruke. Ti se podaci spremaju u račun za pohranu.</li>
</ul>
<br/>
Kada stvorite BizTalk servis, možete koristiti postojeći račun za pohranu ili automatski Stvori novi račun za pohranu.
<br/><br/>
Zadane postavke za pohranu su potrebne za uslugu BizTalk.
<br/><br/>
Stvaranje računa za pohranu primarni ključ i sekundarne ključ se automatski stvaraju. Ove tipke kontrolirati pristup računu za pohranu. Servis za BizTalk automatski koristi primarni ključ.
<br/><br/>
Dodatne informacije potražite u članku <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">prostora za pohranu</a> .
</td>
</tr>

<tr>
<td>SSL certifikat privatno</td>
<td>
Pri stvaranju na servis BizTalk Azure stvara se i HTTPS URL koji sadrži naziv BizTalk servisa. Ovaj URL automatski konfigurirana za korištenje samopotpisani certifikat samo za razvoj. Za proizvodnju, potrebno vam je privatna SSL certifikata.
<br/><br/>
<strong>Informacije o važne SSL certifikata</strong>

<ul>
<li>Datum isteka certifikat mora biti manja od 5 godina.</li>
<li>Svi certifikati privatne potrebna je lozinka. Znate lozinku i kao preporučenim načinom rada zajednički koristite ovu lozinku s vašeg administratorima.</li>
<li>Samopotpisane potvrde se koriste u okruženje za testiranje/razvoj. Prilikom korištenja samopotpisane potvrde, uvoz certifikata u spremište osobnog certifikata i spremište za izdavanje korijenskih certifikata.</li>
</ul>
<br/>Kada pošaljete zahtjev za certifikatom radnog ustanova za izdavanje, ocijenili sljedeća svojstva certifikata učinite sljedeće:
<br/>

<ul>
<li><strong>Poboljšana Upotreba ključa</strong>: barem servisa Azure BizTalk zahtijeva provjeru autentičnosti poslužitelja.</li>
<li><strong>Uobičajeni naziv</strong>: Unesite na potpuno kvalificirani naziv domene (FQDN) URL servisa Azure BizTalk. U ovom članku u odjeljku <a HREF="#BizTalk">Stvaranje BizTalk servisa</a> .</li>
</ul>
<br/>
Novi ili drukčiji certifikat je moguće dodati nakon stvaranja BizTalk servisa.
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>Hibridno veze

Kada stvorite na servis BizTalk Azure, dostupna je karticu **Hibridnog veze** :

![Kartica hibridnog veze][HybridConnectionTab]

Hibridno veze se koriste za povezivanje Azure web-mjesta ili Azure servis za mobilne uređaje s bilo kojeg lokalnog resursa koji koristi statičnog TCP priključak, kao što su SQL Server, MySQL, HTTP Web API-ji, mobilne usluge i većina prilagođene web-servisi.  Hibridno veze i servis za prilagodnik BizTalk se razlikuju. Servis za prilagodnik BizTalk se koristi za povezivanje servisa Azure BizTalk s lokalnim sustavom za redak od tvrtke (LOB).

 Potražite u članku [Hibridno veze](integration-hybrid-connection-overview.md) da biste saznali više, uključujući stvaranje i upravljanje vezama hibridnog.


## <a name="next-steps"></a>Daljnji koraci

Sad kad BizTalk servis je stvorili, upoznajte se s različitim [BizTalk usluge: kartica nadzorne ploče, praćenje i skaliranje](biztalk-dashboard-monitor-scale-tabs.md). Servis za BizTalk spreman za aplikacije. Da biste započeli stvarati aplikacije, idite na [Web-servisa Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Vidi također
- [BizTalk servisa: Grafikon izdanja](biztalk-editions-feature-chart.md)<br/>
- [Servisi za BizTalk: Stanja grafikona](biztalk-service-state-chart.md)<br/>
- [BizTalk servisa: Sigurnosno kopiranje i vraćanje](biztalk-backup-restore.md)<br/>
- [BizTalk usluge: ograničavanje](biztalk-throttling-thresholds.md)<br/>
- [BizTalk servisa: Naziv izdavača i izdavatelj ključ](biztalk-issuer-name-issuer-key.md)<br/>
- [Kako pokrenuti pomoću Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
- [Hibridno veze](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
