<properties 
    pageTitle="Stvaranje i vraćanje sigurnosne kopije BizTalk Services | Microsoft Azure" 
    description="BizTalk servise obuhvaća sigurnosnog kopiranja i vraćanja. Saznajte kako stvoriti i vraćanje sigurnosne kopije i određuju što može vidjeti sigurnosnu kopiju. MABS, WABS" 
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
    ms.date="08/15/2016" 
    ms.author="mandia"/>


# <a name="biztalk-services-backup-and-restore"></a>BizTalk servisa: Sigurnosno kopiranje i vraćanje

Azure BizTalk servise obuhvaća mogućnosti sigurnosnog kopiranja i vraćanja. U ovoj se temi opisuje kako sigurnosnog kopiranja i vraćanja BizTalk servisa pomoću portala za Azure klasični.

Također može sigurnosno kopirati BizTalk usluge korištenja [BizTalk servisa REST API -JA](http://go.microsoft.com/fwlink/p/?LinkID=325584). 

> [AZURE.NOTE] Hibridno veze neće se sigurnosno, bez obzira na to izdanje. Morate ponovno stvoriti hibridnog veze.

## <a name="before-you-begin"></a>Prije početka

- Sigurnosno kopiranje i vraćanje možda neće biti dostupan za sve izdanja. U odjeljku [BizTalk usluge: izdanja grafikona](biztalk-editions-feature-chart.md).

- Pomoću portala za Azure klasični, možete stvoriti sigurnosnu kopiju sustava na zahtjev ili stvoriti zakazano sigurnosno kopiranje. 

- Sigurnosno kopiranje sadržaja može ih vratiti na istom servis BizTalk ili novom servisu BizTalk. Da biste vratili servisa BizTalk pomoću istog naziva, morate izbrisati postojeći BizTalk servis, a naziv mora biti dostupna. Kada izbrišete BizTalk servisa, to može potrajati dulje od željeli za isti naziv da bi bio dostupan. Ako ne možete Pričekajte isti naziv da bi bio dostupan, zatim vratiti novog BizTalk servisa.

- BizTalk Services može ih vratiti isti edition ili noviji edition. Vraćanje servisa BizTalk na donjem izdanje, iz kada je snimljena sigurnosnog kopiranja, nije podržano.

    Ako, na primjer, sigurnosnu kopiju pomoću Osnovno izdanje može vratiti Premium izdanje. Sigurnosne kopije pomoću Premium Edition nije moguće vratiti na standardnu izdanje.

- Uređivanje kontrola brojeve se sigurnosno kopirali da biste zadržali continuity brojeva kontrolu. Ako je poruka obrađuju se nakon zadnje sigurnosne kopije, vraćanje sigurnosne kopije sadržaja mogu prouzročiti duplicirane kontrola brojeva.

- Ako je seriji aktivni poruke, procesa na obradu **prije** pokretanja sigurnosnu kopiju. Kada stvorite sigurnosnu kopiju (kao što je potrebno ili zakazani), poruke u serijama ne spremaju. 

    **Ako se sigurnosnu kopiju aktivnog porukama u grupu, te poruke se sigurnosno kopirali pa stoga se gube.**

- Neobavezno: Na portalu servisa BizTalk zaustavite sve operacije upravljanja.


## <a name="create-a-backup"></a>Stvaranje sigurnosne kopije

Sigurnosne kopije možete poduzeti u bilo kojem trenutku i potpuno upravlja koje. U ovom se odjeljku navedeni koraci za stvaranje sigurnosne kopije pomoću Azure klasični portala, uključujući:

[Sigurnosnu kopiju zahtjev](#backupnow)

[Zakazivanje sigurnosne kopije](#backupschedule)

#### <a name="backupnow"></a>Sigurnosnu kopiju zahtjev
1. Azure klasični portalu odaberite **BizTalk servise**, a zatim odaberite uslugu BizTalk koju želite sigurnosno kopirati.
2. Na kartici **nadzorne ploče** odaberite **sigurnosno kopiranje** pri dnu stranice.
3. Unesite naziv sigurnosne kopije. Na primjer, unesite *myBizTalkService*Grafičke*datuma*.
4. Odaberite račun za spremište blobova platforme i odaberite kvačicu da biste pokrenuli sigurnosnu kopiju.

Kada se dovrši sigurnosnog kopiranja, kontejner s sigurnosne kopije ime koje unesete stvara se u račun za pohranu. Ovaj spremnik sadrži Konfiguracija sigurnosnog kopiranja BizTalk servisa.

#### <a name="backupschedule"></a>Zakazivanje sigurnosne kopije

1. Azure klasični portalu odaberite **BizTalk usluge**, odaberite naziv BizTalk servis koji želite zakazati sigurnosnu kopiju i zatim odaberite karticu **Konfiguracija** .
2. Postavljanje **statusa sigurnosne kopije** za **Uvoz tablica**. 
3. Odaberite **Račun za pohranu** za spremanje sigurnosne kopije, unesite željenu **Učestalost** za stvaranje sigurnosne kopije, a koliko čuvajte (**Zadržavanja dana**):

    ![][AutomaticBU]

    **Bilješke**   
    - U **Dana zadržavanja**razdoblje zadržavanja mora biti veći od sigurnosne kopije učestalost.
    - Odaberite **uvijek zadržati najmanje jednu sigurnosnu kopiju**, čak i ako je istekao razdoblje zadržavanja.
    

4. Odaberite **Spremi**.


Kada se pokrene zakazani sigurnosno kopiranje, stvara spremnik (za spremanje sigurnosne kopije podataka) u račun za pohranu koje ste unijeli. Naziv spremnika pod nazivom *BizTalk servis ime-datuma i vremena*. 

Ako na nadzornoj ploči BizTalk servisa prikazuje status **nije uspjelo** :

![Zadnji status zakazanog sigurnosnog kopiranja][BackupStatus] 

Veza otvara zapisnike operacija servisa za upravljanje pomoći u rješavanju problema. U odjeljku [BizTalk usluge: rješavanje problema s korištenjem zapisnik postupka](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Vraćanje

Sigurnosne kopije možete vratiti na portalu Azure klasični ili [Vraćanje BizTalk servis REST API -JA](http://go.microsoft.com/fwlink/p/?LinkID=325582). U ovom se odjeljku navedene korake da biste vratili pomoću portala za klasični.

#### <a name="before-restoring-a-backup"></a>Prije vraćanja sigurnosne kopije

- Novi praćenja, arhiviranje i nadzor sprema se može unijeti tijekom vraćanje BizTalk servisa.

- Vratit će se iste podatke Runtime uređivanje. Uređivanje izvođenja sigurnosne kopije pohranjuje kontrola brojeve. Vraćena kontrola jesu u nizu od sigurnosnu kopiju. Ako je poruka obrađuju se nakon zadnje sigurnosne kopije, vraćanje sigurnosne kopije sadržaja mogu prouzročiti duplicirane kontrola brojeva.

#### <a name="restore-a-backup"></a>Vraćanje sigurnosne kopije

1. Azure klasični portalu odaberite **Novo** > **Aplikacije servisa** > **BizTalk servis** > **Vraćanje**:

    ![Vraćanje sigurnosne kopije][Restore]

2. **Sigurnosno kopiranje URL-a**, odaberite ikonu mape, a zatim proširite stavku račun Azure prostora za pohranu koji pohranjuje sigurnosne kopije konfiguracije BizTalk servisa. Proširite spremnik i u desnom oknu odaberite odgovarajući datoteku sigurnosne kopije .txt. 
<br/><br/>
Odaberite **Otvori**.

3. Na stranici **Vraćanja BizTalk servisa** unesite **Naziv BizTalk usluge** i provjerite **URL domene**, **Edition**i **regija** vraćene BizTalk servisa. **Stvaranje nove instance SQL baze podataka** za bazu podataka za praćenje:

    ![][RestoreBizTalkService]

    Odaberite strelicu sljedeće.

4.  Provjerite naziv baze podataka SQL, unesite fizičke poslužitelja na kojem će se stvoriti bazom podataka sustava SQL i korisničkog imena i lozinke za taj poslužitelj.


    Ako želite konfigurirati SQL edition baze podataka, veličine i ostalih svojstava odaberite **Konfiguriranje Napredne postavke baze podataka**. 

    Odaberite strelicu sljedeće.

5. Stvaranje novog računa za pohranu ili unesite postojeći račun za pohranu servisa za BizTalk.

7. Odaberite kvačicu da biste pokrenuli vraćanja.

Kada vraćanje uspješno završi, novi BizTalk servis je naveden u obustavljenom stanju na stranici BizTalk servisa Azure klasični portalu.



### <a name="postrestore"></a>Nakon vraćanja sigurnosne kopije

Servis za BizTalk uvijek vratit će se u stanju **obustavljeno** . U ovom stanju mogli unositi promjene konfiguracije prije nego što je novo okruženje funkcionirati, uključujući:

- Ako ste stvorili BizTalk servisnim aplikacijama koje su pomoću Azure BizTalk Services SDK, ćete morati ažurirati vjerodajnice za kontrolu pristupa (ACS) u tim aplikacijama za rad s vraćene okruženju.

- Vraćanje BizTalk servis za replikaciju okruženje BizTalk servisa. U tom slučaju ako postoje ugovore konfiguriran na portalu izvorne BizTalk Services koje koriste FTP mape za izvor ćete morati ažurirati ugovore u okruženju upravo vraćene koristiti drugi izvor FTP mape. U suprotnom, možda postoje dvije različite ugovore pokušaja odvojiti ista poruka.

- Ako vraćen imati više servisa BizTalk okruženja, provjerite je li ciljani točan okruženje u aplikacijama za Visual Studio, cmdleta ljuske PowerShell, REST API-ji ili trgovina partnera upravljanje OM API-ji.

- Dobro da biste konfigurirali automatizirano sigurnosno kopiranje upravo vraćene okruženje BizTalk servis je.

Da biste započeli BizTalk servisa na portalu za Azure klasični, odaberite vraćene BizTalk servisa, a zatim **Nastavi** na programskoj traci. 



## <a name="what-gets-backed-up"></a>Što će sigurnosne kopije

Kada se stvori sigurnosnu kopiju, sljedeće stavke se sigurnosno kopirali:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Stavke sigurnosne kopije</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Portal servisa Azure BizTalk</strong></td>
</tr> 
<tr>
<td>Konfiguracija i izvođenja</td> 
<td>
<ul>
<li>Detalji o partnera i profila</li>
<li>Ugovori partnera</li>
<li>Prilagođeni sklopova implementiran</li>
<li>Bridges implementiran</li>
<li>Certifikati</li>
<li>Pretvaranja implementiran</li>
<li>Kanali</li>
<li>Predlošci stvara i sprema na portalu servisa BizTalk</li>
<li>X12 mapiranja ST01 i GS01</li>
<li>Kontrola brojeva (Uređivanje)</li>
<li>Vrijednosti AS2 poruke Mikrofona</li>
</ul>
</td>
</tr> 
 
<tr>
<td colspan="2">
 <strong>Azure BizTalk servisa</strong></td>
</tr> 
<tr>
<td>SSL certifikat</td> 
<td>
<ul>
<li>SSL certifikat podataka</li>
<li>SSL certifikat lozinke</li>
</ul>
</td>
</tr> 
<tr>
<td>Postavke servisa BizTalk</td> 
<td>
<ul>
<li>Broj jedinica skala</li>
<li>Izdanje</li>
<li>Verzija proizvoda</li>
<li>Područje/podatkovnim centrom</li>
<li>Prostor za naziv programa Access za kontrola servisa (ACS) i tipki</li>
<li>Niz za povezivanje baze podataka za praćenje</li>
<li>Arhiviranje niz za povezivanje računa za pohranu</li>
<li>Nadzor prostora za pohranu niz za povezivanje računa</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Dodatne stavke</strong></td>
</tr> 
<tr>
<td>Praćenje baze podataka</td> 
<td>Prilikom stvaranja servisa BizTalk pojedinosti praćenja baze podataka se unose, uključujući poslužitelj baze podataka SQL Azure i naziv baze podataka za praćenje. Baza podataka za praćenje se automatski sigurnosnu kopiju.
<br/><br/>
<strong>Važno</strong><br/>
Ako se briše baze podataka za praćenje i bazu podataka potrebno oporaviti prethodne sigurnosne kopije mora postojati. Ako ne postoji sigurnosnu kopiju baze podataka za praćenje i njegove podatke nisu koje se mogu vratiti. U tom slučaju stvorite novu praćenja bazu podataka s istim nazivom u bazi podataka. Preporučuje se zemlj replikacije.</td>
</tr> 
</table>

## <a name="next"></a>Sljedeći

Da biste stvorili Azure BizTalk servisa na portalu za Azure klasični, idite na [BizTalk usluge: dodjeljivanje Azure pomoću portala za klasični](http://go.microsoft.com/fwlink/p/?LinkID=302280). Da biste započeli stvarati aplikacije, idite na [Web-servisa Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Vidi također
- [Sigurnosno kopiranje BizTalk servisa](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [Vraćanje servisa BizTalk iz sigurnosne kopije](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [Servisi za BizTalk: Za razvojne inženjere, Basic, standardna i Premium izdanja grafikona](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalk servisa: Azure pomoću portala za klasični dodjele resursa](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [Servisi za BizTalk: Dodjeljivanje Status grafikona](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk servisa: Kartica nadzorne ploče, Monitor i skaliranje](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [BizTalk usluge: ograničavanje](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalk servisa: Naziv izdavača i izdavatelj ključ](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Kako pokrenuti pomoću Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png
 
