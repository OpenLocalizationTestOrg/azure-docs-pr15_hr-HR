<properties
    pageTitle="Informirajte se o značajkama u izdanjima usluge BizTalk | Microsoft Azure"
    description="Usporedba mogućnosti izdanja BizTalk usluge: slobodno, za razvojne inženjere, Basic, standardna i Premium. MABS, WABS."
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
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="biztalk-services-editions-chart"></a>BizTalk servisa: Grafikon izdanja

Servisa Azure BizTalk nudi nekoliko izdanja. Određivanje koje izdanje najviše odgovara vašim potrebama scenarija i poslovnih pomoću ovog članka.


## <a name="compare-the-editions"></a>Usporediti izdanja

**Slobodno (pretpregled)**

Možete stvoriti web-mjesta i upravljanje vezama hibridnog. Veza za hibridno jednostavan je način za povezivanje Azure web-mjesta s lokalnog sustava, kao što je SQL Server.

**Za razvojne inženjere**

Obuhvaća veze hibridnog, EAI i uređivanje obrade poruke s jednostavan upotrebu poslovnih partnera portal za upravljanje i podrška za uobičajene Uređivanje sheme i obogaćenog obrada za uređivanje nad X12 i AS2. Možete stvoriti uobičajeni EAI scenariji povezivanja servise u oblaku putem HTTP/S OSTATKOM, FTP, WCF i SFTP protokola za čitanje i pisanje poruka.  Korištenje veza s lokalnim LOB sustavi s spremna za korištenje prilagodnika SAP, Oracle eBusiness, Oracle DB, Siebel i SQL Server. Korištenje okruženje za razvojne inženjere za usmjereni na uz Visual Studio tools za jednostavno razvoj i implementacija. Ograničen je na svrhu razvoja i testiranje samo s bez servisa razinu ugovor (SLA).

**Osnovni**

Obuhvaća iskoristili mogućnosti za razvojne inženjere s povećanjem hibridnog veze, EAI bridges, ugovori za uređivanje, a BizTalk prilagodnik paket povezivati. Nudi visoke dostupnosti, a zatim željenu mogućnost skaliranja s na servis razinu ugovor (SLA).

**Standardna**

Obuhvaća sve osnovne mogućnosti s povećanjem u hibridnog veze, bridges EAI, uređivanje ugovore i BizTalk prilagodnik paket veze. Nudi visoke dostupnosti, a zatim željenu mogućnost skaliranja s na servis razinu ugovor (SLA).

**Premium**

Obuhvaća sve standardne mogućnosti s povećanjem hibridnog veze, EAI bridges, ugovore za uređivanje, a BizTalk prilagodnik paket povezivati. Uključuje arhiviranje, visoke dostupnosti i mogućnost da biste skalirali s na servis razinu ugovor (SLA).


## <a name="editions-chart"></a>Grafikon izdanja
U sljedećoj su tablici navedeni razlike.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Slobodno (pretpregled)</th>
        <th>Za razvojne inženjere</th>
        <th>Osnovni</th>
        <th>Standardna</th>
        <th>Premium</th>
</tr>

<tr>
<td><strong>Početni cijena</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Servisa Azure BizTalk cijene</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full">Azure cijene kalkulatora</a></td>
</tr>
<tr>
<td><strong>Zadana minimalne konfiguracija</strong></td>
<td>1 besplatne jedinica</td>
<td>Jedinica 1 za razvojne inženjere</td>
<td>1 osnovna jedinica</td>
<td>1 standardne jedinica</td>
<td>1 premium jedinica</td>
</tr>
<tr>
<td><strong>Promjena veličine</strong></td>
<td>Bez promjena veličine</td>
<td>Bez promjena veličine</td>
<td>Da, u koracima od 1 osnovna jedinica</td>
<td>Da, u koracima od 1 standardne jedinice</td>
<td>Da, u koracima 1 Premium jedinice</td>
</tr>
<tr>
<td><strong>Najveći dopušteni skaliranje izgleda</strong></td>
<td>Bez promjena veličine</td>
<td>Bez promjena veličine</td>
<td>Do 8 jedinica</td>
<td>Do 8 jedinica</td>
<td>Do 8 jedinica</td>
</tr>
<tr>
<td><strong>EAI Bridges po jedinici</strong></td>
<td>Neće biti uvršteni</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>UREĐIVANJE, AS2</strong>
<br/><br/>
Obuhvaća TPM ugovori</td>
<td>Neće biti uvršteni</td>
<td>Uključena. 10 ugovore po jedinici.</td>
<td>Uključena. 50 ugovore po jedinici.</td>
<td>Uključena. 250 ugovore po jedinici.</td>
<td>Uključena. 1000 ugovori po jedinici.</td>
</tr>
<tr>
<td><strong>Hibridno veze po jedinici</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Hibridno veze prijenos podataka (GB) po jedinici</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>BizTalk prilagodnik servisa veze s lokalnim LOB sustavi</strong></td>
<td>Neće biti uvršteni</td>
<td>1 veze</td>
<td>2 veze</td>
<td>5 veze</td>
<td>25 veze</td>
</tr>
<tr>
<td align="left"><strong>Podržani protokoli/sustavi:</strong>
<ul>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Servis Bus (SB)</li>
<li>Blobova platforme Azure</li>
<li>REST API-ji</li>
</ul>
</td>
<td>Neće biti uvršteni</td>
<td>Dio</td>
<td>Dio</td>
<td>Dio</td>
<td>Dio</td>
</tr>
<tr>
<td><strong>Visoke dostupnosti</strong>
<br/><br/>
Za servis razinu ugovor (SLA), potražite u članku <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Azure BizTalk Services cijene</a>.
</td>
<td>Neće biti uvršteni</td>
<td>Neće biti uvršteni</td>
<td>Dio</td>
<td>Dio</td>
<td>Dio</td>
</tr>
<tr>
<td><strong>Sigurnosno kopiranje i vraćanje</strong></td>
<td>Neće biti uvršteni</td>
<td>Dio</td>
<td>Dio</td>
<td>Dio</td>
<td>Dio</td>
</tr>
<tr>
<td><strong>Praćenje</strong></td>
<td>Neće biti uvršteni</td>
<td>Dio</td>
<td>Dio</td>
<td>Dio</td>
<td>Dio</td>
</tr>
<tr>
<td><strong>Arhiviranje</strong><br/><br/>
Obuhvaća Osigurano priznavanje zaprimanja (NRR) i preuzimanje praćene poruke</td>
<td>Neće biti uvršteni</td>
<td>Dio</td>
<td>Neće biti uvršteni</td>
<td>Neće biti uvršteni</td>
<td>Dio</td>
</tr>
<tr>
<td><strong>Korištenje prilagođenog koda</strong></td>
<td>Neće biti uvršteni</td>
<td>Dio</td>
<td>Dio</td>
<td>Dio</td>
<td>Dio</td>
</tr>
<tr>
<td><strong>Korištenje pretvorbe, uključujući prilagođene XSLT-a</strong></td>
<td>Neće biti uvršteni</td>
<td>Dio</td>
<td>Dio</td>
<td>Dio</td>
<td>Dio</td>
</tr>
</table>

> [AZURE.NOTE] Za otpornost protiv hardverske pogreške visoke dostupnosti podrazumijeva imate više VMs unutar jedna jedinica BizTalk.


## <a name="faqs"></a>Najčešća pitanja

#### <a name="what-is-a-biztalk-unit"></a>Što je jedinica BizTalk?
"Jedinica" je atomske razinu implementacije sustava servisa Azure BizTalk Services. Svako izdanje isporučuje se s jedinicu koja ima kapacitet različitih računalnim i memorije. Ako, na primjer, osnovna jedinica sadrži više računalnim od za razvojne inženjere, standardna ima više računalnim od osnovni i tako dalje. Kada skaliranja BizTalk servisa, skaliranja pomoću jedinice.

#### <a name="what-is-the-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Koja je razlika između servisa BizTalk i Azure BizTalk VM?
Usluge BizTalk nudi true arhitekture platforme kao-na-usluga (PaaS) za izrade rješenja za integraciju u oblaku. S modelom PaaS potpuno fokusiranje na logike aplikacije i ostavite sva upravljanja infrastrukture Microsoftu, uključujući:

- Nema potrebe za upravljanje ili zakrpu virtualnih računala.
- Microsoft osigurava dostupnost.
- Upravljanje skaliranje na zahtjev putem jednostavno poruke više ili manje kapaciteta putem portala za Azure.

BizTalk Server na virtualnim strojevima sa sustavom Azure nudi Arhitektura programa infrastrukturu-kao-na-Service (IaaS). Stvaranje virtualnim strojevima i konfiguriranje ih točno onako kao što su lokalnog okruženja olakšano pokretanje postojeće aplikacije u oblak bez ikakvih promjena kod. S IaaS, ste odgovorni i dalje za konfiguriranje virtualnim strojevima, upravljanje virtualnim strojevima (na primjer, instaliranje softvera i zakrpa OS) i architecting aplikacije za visoke dostupnosti.

Ako tražite izrade rješenja s novi Integracija minimiziranje vaše trud upravljanje infrastrukture, koristite BizTalk servisa. Ako vas zanimaju da biste brzo migriranje na postojeće BizTalk rješenja ili tražite na zahtjev okruženje za razvoj i testiranje BizTalk Server aplikacije, koristite BizTalk Server na programa Azure virtualnog računala.

#### <a name="what-is-the-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Koja je razlika između servisa prilagodnik BizTalk i hibridnog veze?
Servis za prilagodnik BizTalk koristi na servis BizTalk Azure. Servis za prilagodnik BizTalk koristi paket BizTalk prilagodnik za povezivanje s lokalnim sustavom za redak od tvrtke (LOB). Veza za hibridno omogućuju jednostavno i praktičan povezati Azure aplikacije, kao što su značajke web-aplikacije u aplikacije servisa za Azure i Azure mobilne usluge programa lokalnog resursa.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-the-limit-is-reached"></a>Što znači "Hibridnog veze prijenos podataka (GB) po jedinici"? Je li ovo po minute/sat/dan/tjedan/mjesec? Što se događa kada je dostigne ograničenje?

Jedinični trošak hibridnog veze ovisi o BizTalk Services edition. Jednostavno staviti troškove ovise na kojom količinom podataka prijenos. Ako, na primjer, svakodnevno prijenos podataka 10 GB troškove manji od svakodnevno prijenos 100 GB. Određivanje troškovi pomoću [Kalkulatora cijene](https://azure.microsoft.com/pricing/calculator/?scenario=full) za BizTalk servise. Obično ograničenja provode se svakog dana. Ako premašite ograničenje, sve pokrivenost se naplaćuje brzinom $1 po GB.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-the-number-of-bridges-go-up-by-two-instead-of-just-one"></a>Prilikom stvaranja ugovor BizTalk Services, zašto ne broj bridges se pomaknuli prema gore po dva umjesto samo jedan?

Svaki ugovor sastoji se od od dvije različite bridges: komunikacije most strani za slanje i primanje most strani komunikacije.

####  <a name="what-happens-when-i-hit-the-quota-limit-on-the-number-of-bridges-or-agreements"></a>Što se događa kada se kliknite ograničenje kvote broj bridges ili ugovore?

Niste moći uvesti bilo koji novi bridges ili stvorite sve nove ugovore. Da biste implementirali više, morate proširenjem dodatne jedinice za usluge BizTalk ili nadogradnju na noviji edition.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-to-another"></a>Migriranje iz jednog sloju BizTalk servisa u drugu?

Besplatni edition nije moguće migrirati ili 'skalirana ' za drugi sloju i ne mogu sigurnosnu kopiju i vratiti na drugi sloju. Ako vam je potrebna drugi sloju, stvorite novi BizTalk servis pomoću novog sloju. Sve artefakte stvoren pomoću besplatnog edition, uključujući veze hibridnog morati ponovno stvoriti u novom servisu BizTalk. 

Za preostale izdanja pomoću sigurnosno kopiranje i vraćanje za Migracija sustava artefakte s jednog sloju u drugu. Ako, na primjer, sigurnosno kopirati vaše artefakte u standardni sloju i njihova vraćanja sloju Premium. [BizTalk usluge: sigurnosnog kopiranja i vraćanja](biztalk-backup-restore.md) opisuje putova podržani migracije i popise koje artefakte se sigurnosno kopirali. Imajte na umu da se veza hibridnog se sigurnosno kopirali. Nakon sigurnosno kopiranje i vraćanje na novu sloju, zatim stvoriti hibridnog veze.  


#### <a name="is-the-biztalk-adapter-service-included-in-the-service-how-do-i-receive-the-software"></a>Je servis prilagodnik BizTalk obuhvaćeno servis? Kako dobiti softver?

Da, servis prilagodnik BizTalk prilagodnik paketom BizTalk uvršteni su Azure BizTalk Services SDK za [Preuzimanje](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Daljnji koraci

Da biste stvorili Azure BizTalk servisa na portalu za Azure, idite na [BizTalk usluge: dodjeljivanje pomoću portala za Azure](biztalk-provision-services.md). Da biste započeli stvarati aplikacije, idite na [Web-servisa Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>Dodatni resursi
- [Servisi za BizTalk: Dodjeljivanje pomoću portala za Azure](biztalk-provision-services.md)<br/>
- [Servisi za BizTalk: Dodjeljivanje Status grafikona](biztalk-service-state-chart.md)<br/>
- [BizTalk servisa: Kartica nadzorne ploče, Monitor i skaliranje](biztalk-dashboard-monitor-scale-tabs.md)<br/>
- [BizTalk servisa: Sigurnosno kopiranje i vraćanje](biztalk-backup-restore.md)<br/>
- [BizTalk usluge: ograničavanje](biztalk-throttling-thresholds.md)<br/>
- [BizTalk servisa: Naziv izdavača i izdavatelj ključ](biztalk-issuer-name-issuer-key.md)<br/>
- [Kako pokrenuti pomoću Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
