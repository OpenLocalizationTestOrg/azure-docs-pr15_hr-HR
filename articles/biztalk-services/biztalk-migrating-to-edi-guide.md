<properties   
    pageTitle="Migracija BizTalk Server uređivanje rješenja BizTalk Services Tehnički vodič | Microsoft Azure"
    description="Migriranje uređivanje u MABS; Servisa Microsoft Azure BizTalk"
    services="biztalk-services"
    documentationCenter="na"
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


# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>Migracija BizTalk Server uređivanje rješenja BizTalk servisima: Tehnički vodič

Autor: Tim Wieman i Nitin Mehrotra

Pregledavatelji: Karthik Bharthy

Pisani korištenjem: Microsoft Azure BizTalk Services – veljača 2014 izdanja.

## <a name="introduction"></a>Uvod

Elektronička podataka za razmjenu (Uređivanje) jedan je od najviše sustavi sredstva koja razmjena podataka tvrtke elektroničkom obliku i termed kao tvrtke tvrtke ili B2B transakcije. BizTalk Server je prodao uređivanje podrška za putem deset godina, nakon početnog izdanja BizTalk poslužitelja. Pomoću servisa BizTalk Microsoft nastavlja podrška za rješenja za uređivanje na platformi Microsoft Azure. Transakcije B2B uglavnom nalaze izvan tvrtke ili ustanove, a dakle jednostavnije je implementirati ako je implementiran na cloud platformi. Microsoft Azure sadrži tu mogućnost putem servisa BizTalk.

Dok neki klijenti vidjeli BizTalk servise kao "greenfield" platformu za nova rješenja za uređivanje, broju korisnika imati trenutni uređivanje poslužitelja BizTalk rješenja koje želi migrirati Azure. Jer je architected BizTalk Services uređivanje temelje se na istom ključa entiteti kao BizTalk Server uređivanje arhitektura (trgovina partnera, entiteti, ugovore), moguće je da biste migrirali BizTalk Server uređivanje artefakte BizTalk servisima.

Ovaj dokument iznose neki od razlike vezanih uz migracije BizTalk Server uređivanje artefakte BizTalk servisima. Ovaj dokument podrazumijeva rad znanja obrada uređivanje BizTalk Server i trgovina ugovore partnera. Dodatne informacije o uređivanje BizTalk Server potražite u članku [Trgovina partnera koristeći BizTalk poslužitelju za upravljanje](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Koju verziju sustava BizTalk Server uređivanje artefakte migrira se na BizTalk usluge?

Modul za uređivanje BizTalk Server znatno poboljšani za BizTalk Server 2010 kada ga je ponovno modeliranim da biste uključili partnera, profili i ugovore. Usluge BizTalk koristi iste model da biste organizirali poslovnih partnera i tvrtke odjelima unutar te poslovnih partnera. Migracija uređivanje artefakte s BizTalk Server 2010 i novijim verzijama BizTalk servisima, kao rezultat je znatno jednostavan postupak. Da biste migrirali uređivanje artefakte povezan s verzijama BizTalk Server 2010, morate nadograditi BizTalk Server 2010 i prenesite na uređivanje artefakte BizTalk Services.

## <a name="scenariosmessage-flow"></a>Tijek scenariji/poruke

Kao s poslužiteljem BizTalk uređivanje obrade BizTalk Services ugrađena je oko rješenje trgovina partnera Management (TPM). Rješenje TPM sadrži sljedeće ključne komponente:

- Trgovina partnere koji predstavljaju tvrtke ili ustanove u B2B transakcije.
- Profila koji predstavljaju odjelima unutar poslovnih partnera.
- Trgovina ugovore partnera (ili ugovore) koji predstavlja ugovor tvrtke između dva partnere/profile.

Sljedeća ilustracija prikazuje sličnosti, kao i razlike između BizTalk Server uređivanje rješenja i uređivanje usluge BizTalk rješenja:

![][EDImessageflow]

Ključne razlike i sličnosti između programa tijek rješenje uređivanje u BizTalk Server i servise BizTalk su:

- Jednako kao BizTalk Server koristi za kanal EDIReceive primati poruke uređivanje i na kanal EDISend da biste poslali poruku uređivanje, BizTalk Services koristi uređivanje primanje most prima i uređivanje slanje most slanja poruke za uređivanje. U BizTalk Server u kanali pridružuju ugovor pomoću slanje ili primanje priključci. U BizTalk servise, ugovor sam označava slanje ili primanje most.
- U BizTalk Server nakon kanal EDIReceive obrađuje poruku uređivanje poruka je dumped s bazom podataka sustava SQL Server. Kanal EdiSend uzima poruke iz baze podataka sustava SQL Server, obrađuje je i šalje ga poslovnih partneru.

    U BizTalk servise, nakon na uređivanje primite most obrađuje poruku za uređivanje, preusmjerava poruke vanjski postupak. Vanjski postupak ne može se izvoditi na Microsoft Azure ili na lokalnim poslužiteljima. Vanjski postupak treba usmjeravanje poruka pošalji most uređivanje; Slanje most uvesti čini poruku. Nakon obrade poruka, slanje most uređivanje usmjerava poruku poslovnih partnera.

BizTalk usluge pružaju konfiguracije jednostavno koristite da biste brzo stvoriti i implementirati B2B ugovor između trgovina partnere bez konfiguriranje izračunati bilo koji Microsoft Azure instance (weba ili tempiranja uloge), nijedna baza podataka Microsoft Azure SQL ili svih računa za pohranu za Microsoft Azure Složenije scenarije zahtijevaju zauzimanja u tijekove rada ili druge usluge obrada "oko rubova" na trgovina partnerom, odnosno prije ili nakon obrade most Trgovina uređivanje ugovor partnera. Detaljno, sljedeće nizove događaja pojaviti tijekom obrade BizTalk Services poruke za uređivanje.

1. Za uređivanje je poslao poruku trgovina partnera, Fabrikam.  Primanje poruka uređivanje poslovnih partnera, BizTalk Services podržava prijenosa protokoli kao što su FTP, SFTP, AS2 i HTTP/S.

2. Kotiranja obrada primanje strani ugovor partnera rastavlja uređivanje poruke u obliku XML.  Za krajnje točke Bus servisa kao što su krajnje točke servisa Bus preusmjeravanja, tema Bus servisa, red čekanja Bus servisu ili usluge BizTalk most mogu usmjeravati disassembled uređivanje poruke (u XML formatu).

3. Zatim se primljene disassembled poruke XML od krajnja točka za daljnje prilagođene obrada.  Te krajnje točke nije moguća komponenta za lokalni ili instancu komponente Microsoft Azure izračunati daljnje obrade poruke u servisu Windows tijeka rada (WF) ili Windows Communication Foundation (WCF), na primjer.

4. "Obrada Pošalji strane" poslovnih partnera ugovor skupiti XML poruke u obliku za uređivanje pa šalje poslovnih partnera, Contoso.  Za slanje poruka uređivanje poslovnih partnerima, BizTalk Services podržava protokola za isti kao i za primanje poruka za uređivanje.

Ovaj dokument sadrži konceptualne smjernice na neki drugi artefakte BizTalk Server uređivanje migracije BizTalk Services.

## <a name="sendreceive-ports-to-trading-partners"></a>Slanje/primanje priključke za partnere trgovina

U BizTalk Server postavljate primanje mjesta i primanje priključaka primati poruke uređivanje/XML poslovnih partnera i postavite slanje priključke za slanje poruka uređivanje/XML poslovnih partnera. Zatim povežite gore navedenim priključcima korištenju konzole za administraciju BizTalk Server poslovnih partnerom. U BizTalk servise, mjesta gdje primati poruke od trgovina partnera i kojima šaljete poruke trgovina partnere konfigurirane kao dio sustava poslovnih partnerom sam (kao dio postavke prijenosa) na portalu servisa BizTalk.  Pa zaista nemate pojam "Pošalji priključke" i "primate mjesta", godišnja se BizTalk Services. Dodatne informacije potražite u članku [Stvaranje ugovore](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Kanali (Bridges)

U BizTalk Server uređivanje, kanali su entiteti obrada poruka koje možete dodati prilagođene logike za značajke određene obrada, po potrebi aplikacija. Za servise BizTalk u jednaku vrijednost želite most za uređivanje. No u BizTalk servise, zasad bridges Uređivanje "zatvorene".  To jest, prilagođene aktivnosti ne možete dodati u most uređivanje. Sve prilagođene obrada mora poduzeti izvan mosta uređivanje u aplikaciji prije ili nakon poruku unosi most konfigurirati kao dio trgovina partnerom. EAI bridges postoji mogućnost da biste učinili prilagođene obrada. Ako želite prilagođeni obrada, možete koristiti EAI bridges prije ili nakon most uređivanje obraditi poruku. Dodatne informacije potražite u članku [kako uključiti prilagođeni kod u Bridges](https://msdn.microsoft.com/library/azure/dn232389.aspx).

Možete umetnuti Tijek objavljivanja/pretplate s prilagođenog koda i/ili pomoću servisa Bus poruka redova i teme prije poslovnih partnerom primi poruku ili pak nakon ugovor obrađuje poruku i usmjerava na krajnjoj točki Bus servisa.

Potražite u članku **Tijek scenariji/poruku** u ovoj temi za uzorak tijek poruke.

## <a name="agreements"></a>Ugovori

Ako ste upoznati s BizTalk Server 2010 Trading partnera ugovore koristi za obradu za uređivanje, zatim BizTalk Services trgovina partnera ugovore izgledati vrlo poznat. Većina postavki ugovor isti i koristite istu terminologija. U nekim slučajevima, postavke ugovor mnogo su jednostavniji u usporedbi s istih postavki u BizTalk Server. Microsoft Azure BizTalk Services podržava X12, EDIFACT i AS2 prijenos.

Microsoft Azure BizTalk Services također nudi na TPM podataka alat za **Migraciju poslovnih partnera i ugovore o iz modula BizTalk Server trgovina partnera BizTalk Services portal** . Alat za migraciju podataka TPM dostupna je kao dio paketa Alati koji mogu se preuzeti sa [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). Paket sadrži i readme koja sadrži upute o korištenju alata i osnovne informacije o otklanjanju poteškoća za odabrani alat.

## <a name="schemas"></a>Shema

BizTalk servisa omogućuje uređivanje sheme koje se mogu koristiti u rješenja BizTalk usluga.  K tome, BizTalk Server Uređivanje sheme mogu također se sa servisima BizTalk jer je Korijenski čvor sheme uređivanje isti preko BizTalk Server, kao i BizTalk servisa. Dakle, moći izravno iskoristite svoje BizTalk Server Uređivanje sheme i njihovu korištenju u rješenja za uređivanje koji razvijaju pomoću BizTalk Services. Sheme možete preuzeti i iz [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Karte (pretvaranja)

Karte u BizTalk Server nazivaju pretvorbe BizTalk Services. Karte migracije iz BizTalk Server BizTalk servisima može biti nešto složenije zadataka da biste postigli (ovisno o složenosti mapa). Mapiranje alat koji se koristi za servise BizTalk razlikuje se od BizTalk mapiranje. Iako u mapiranje izgleda uglavnom ista, temeljni obliku karte razlikuje se. U functoids (pod nazivom **Karta operacije** BizTalk Services) dostupne korisnicima te se razlikuju.  Zapravo, ne možete koristiti kartu BizTalk izravno BizTalk Services. Osim toga, sve functoids koje su dostupne u BizTalk Server dostupne kao karte operacije BizTalk Services.

### <a name="new-transform-operations"></a>Nova operacija transformacije

Dok je na popisu mapa operacija transformacije dostupne mogu prestati prilično razlikuje od mapiranje BizTalk Server, BizTalk usluge pretvorbe omogućiti novi načina izvršavanje iste zadatke. BizTalk usluge pretvorbe, na primjer, imate **Popis operacije** dostupne. Nije dostupno u BizTalk mapiranje.  **Popis operacije** omogućuju stvaranje i raditi na "Popis", gdje je popis skup stavki (poznat i kao "reci") i gdje svaku stavku možete imati više članova (poznat i kao "stupaca").  Možete sortirati popis, odaberite stavke na temelju uvjeta, itd.

Drugi primjer primjene nove funkcije u BizTalk usluge pretvorbe su **Operacije petlje**.  Ovo je teško stvaranje ugniježđene petlje u mapiranje BizTalk Server.  Dakle, operacije karta ponavljanje dodat će se za pretvorbe BizTalk Services.

Još drugi primjer je postupak za karte **If-zatim-Else** izraz.  Način if-zatim-else operacija nije moguće u BizTalk mapiranje, ali je potrebno više functoids da biste izvršili prividno jednostavan zadatak.

### <a name="migrating-biztalk-server-maps"></a>Karte migracije BizTalk Server

Microsoft Azure BizTalk Services nudi alata za migraciju BizTalk Server karte servisa BizTalk pretvorbe. Dostupna je **BTMMigrationTool** kao dio paketa **alata** koji ste dobili uz [Preuzimanje BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). Dodatne informacije o alatu potražite u članku [Pretvaranje mapa BizTalk Pretvorba na servisima BizTalk](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Možete pogledati i uzorka po Sandro Pereira, BizTalk MVP za [migriranje BizTalk Server karte da biste BizTalk servisa pretvorbi](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Orchestrations

Ako je potrebno migrirati BizTalk Server djelovanje obrade Microsoft Azure na orchestrations morate biti potrebno ponovno napisati jer Microsoft Azure ne podržava izvodi orchestrations BizTalk Server.  Nije moguće dopune funkciju djelovanje u servisu Windows tijeka rada Foundation 4.0 (WF4).  Na ovom bi dovršavanje novog teksta kao što je trenutno nema migracije sa BizTalk Server orchestrations na WF4. Evo nekih resursa za tijek rada sustava Windows:

- [*Integraciji servisa WCF tijeka rada s servisa Bus redovima i teme*](https://msdn.microsoft.com/library/azure/hh709041.aspx) tako da Paolo Salvatori. 

- [Sesije *sastavne aplikacije s web-mjesto Windows Foundation tijeka rada i u okvir za Azure* ](http://go.microsoft.com/fwlink/p/?LinkId=237314) iz konferencije Sastavi 2011.

- [*Razvojni centar za Foundation tijeka rada sustava Windows*](http://go.microsoft.com/fwlink/p/?LinkId=237315) na MSDN-u.

- [*Dokumentacija Windows tijeka rada Foundation 4 (WF4)*](https://msdn.microsoft.com/library/dd489441.aspx) na MSDN-u.

## <a name="other-considerations"></a>Druge napomene

Slijede nekoliko pitanja vezana uz koji morate unijeti tijekom korištenja servisa BizTalk.

### <a name="fallback-agreements"></a>Pričuvni ugovora

Obrada BizTalk Server uređivanje ima pojam "Pričuvne ugovore".  Ne BizTalk Services **ne** ste dosad pričuvne ugovor pojam.  Saznajte BizTalk dokumentaciju tema [U ulozi ugovore u obrada uređivanje](http://go.microsoft.com/fwlink/p/?LinkId=237317) i [Konfiguriranje globalnih pričuvne ugovor svojstva ili](https://msdn.microsoft.com/library/bb245981.aspx) informacije kako se koriste pričuvne ugovore u BizTalk Server.

### <a name="routing-to-multiple-destinations"></a>Usmjeravanje više odredišta

Bridges BizTalk Services u njegovu trenutnom stanju ne podržava usmjeravanje poruka više odredišta pomoću u Objavi-pretplata modela. Umjesto toga nije usmjeravanje poruka iz servisa BizTalk mosta servisa Bus temu koju možete imati više pretplata poruka na više od jedne krajnje točke.

## <a name="conclusion"></a>Zaključak

Microsoft Azure BizTalk Services će se ažurirati na obični faze da biste dodali dodatne značajke i mogućnosti. Sa svakom ažuriranju smo izgled naprijed povećana funkcijama koje olakšavaju kraja do kraja rješenja pomoću servisa BizTalk i druge tehnologije Azure stvara za podršku.

## <a name="see-also"></a>Vidi također

[Razvoj aplikacijama s Azure](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
