<properties
    pageTitle="Više klijentu Web aplikacije uzorak | Microsoft Azure"
    description="Pronađite arhitektonski pregled i dizajn uzorke koje opisuju kako implementirati više klijentu web-aplikacije na Azure."
    services=""
    documentationCenter=".net"
    authors="wadepickett" 
    manager="wpickett"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/05/2015"
    ms.author="wpickett"/>

# <a name="multitenant-applications-in-azure"></a>Složene aplikacije u Azure

Složene aplikacija je zajednički resurs koji omogućuje zasebnom korisnika ili "klijenata," da biste vidjeli aplikaciju kao da je vlastite. Uobičajeni scenarij u kojem se lends sam složene aplikacije jest ona u kojima svi korisnici aplikacije možda želite prilagodbu korisničkog sučelja, ali u suprotnom imaju isti zahtjeve osnovnih mogućnosti poslovnog. Primjeri velike složene aplikacije su Office 365, Outlook.com i visualstudio.com.

Iz perspektive davateljem aplikacije prednosti multitenancy uglavnom odnose se na domena i troškovima učinkovitosti. Jedna verzija aplikacije možete potrebe više klijenata/klijenata, dopustite konsolidacije sustava administrativnih zadataka kao što su nadzor, ugađanju performansi, softver održavanja i sigurnosne kopije podataka.

Popis najčešće znatnog ciljeva i preduvjeti iz perspektive nekog davatelja usluge nudi.

- **Provisioning**: moraju imati mogućnost Dodjela nove klijenata za aplikaciju.  Za složene aplikacije s velikim brojem klijenata je obično potrebno da biste automatizirali postupak omogućivanjem samoposlužno dodjele resursa.
- **Maintainability**: moraju imati mogućnost da biste nadogradili aplikacije i izvedite druge zadatke održavanja dok više klijenata ga koristite.
- **Nadzor**: moraju imati mogućnost praćenje aplikacije cijelo vrijeme za prepoznavanje probleme i otklanjanje poteškoća s njima. To obuhvaća praćenje načina na koji svaki klijent putem aplikacije.

Ispravno implementirani složene aplikacije pruža sljedeće prednosti korisnicima.

- **Odvajanja**: aktivnosti klijenata za pojedinačne ne utječu na korištenje aplikacije tako da drugim korisnicima. Samoposlužni ne može pristupiti tuđe podataka. Pojavljuje se u klijent kao da su isključivo korištenje aplikacije.
- **Dostupnost**: klijenata za pojedinačne želite aplikacije da bi bio stalno dostupan, možda s jamstva koji su definirani u programa SLA. Ponovno aktivnosti drugih klijenata ne bi trebali utjecati dostupnosti aplikacija.
- **Skalabilnost**: aplikacija mijenja veličinu da bi odgovarao zahtjev pojedinačne drugih korisnika. Podaci o prisutnosti i akcije drugih klijenata treba utjecati na performanse aplikacije.
- **Troškovi**: troškovi su manja od više tenancy omogućuje zajedničko korištenje resursa pokrenut namjenski, jednog klijenta aplikacije.
- **Prilagodljivost**. Mogućnost da biste prilagodili aplikacije za pojedinačne klijent na različite načine, kao što su Dodavanje ili uklanjanje značajki, promjenu boje i logotipa ili čak i dodavanje vlastitih kod ili skriptu.

Ukratko, dok su mnoge pitanja vezana uz koje morate poduzeti u obzir visoko skalabilni usluga, postoje i broj ciljeva i preduvjeti koji su zajednički mnoge složene aplikacije. Neke možda nije odgovarajuće u određenim slučajevima, a u svakom slučaju razlikovat će se važnost pojedinačne ciljeva i preduvjeti. Kao davatelj složene aplikacije i imat ćete ciljeve i preduvjeti kao što, sastanak na klijenata ciljeva i preduvjeti, dobiti od klijenata, naplata, više razine usluge, dodjele resursa, nadzor maintainability i automatizaciju.

Dodatne informacije o dodatnim dizajn pitanja vezana uz složene aplikacije potražite u članku [nalaze više klijentske aplikacije na Azure][]. Informacije o uobičajenih podataka arhitektura uzoraka aplikacija za baze podataka za više klijentu softver-kao-na-service (SaaS) potražite u članku [Dizajn obrazaca za više klijentske aplikacije SaaS s bazom podataka SQL Azure](./sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure nudi brojne značajke koje omogućuju za rješavanje problema s ključem naišao na prilikom dizajniranja složene sustava.

**Odvajanja**

- Klijenata segmenta web-mjesta tako da glavno računalo zaglavlja sa ili bez SSL komunikacije
- Web-mjesto segmenta klijenata po parametrima upita
- Web-servisi uloge suradnika
    - Uloga suradnika. koji se obično obrada podataka na pozadinska aplikacija.
    - Web uloge koje obično poslužiti kao sučelju za aplikacije.

**Prostor za pohranu**

Upravljanje podacima kao što su baze podataka SQL Azure ili Azure prostora za pohranu servisa kao što su tablice usluga koje nude usluge za pohranu velikih količina podataka nestrukturirane i Blob servis koji omogućuje servise za pohranu velikim količinama teksta nestrukturirane ili binarne podatke kao što su videozapisa, zvuka i slike.

- Zaštiti Multitenant podataka u bazi podataka SQL odgovarajuće po-klijentu prijave u sustav SQL Server.
- Koristite Azure tablice za aplikaciju resursi navođenjem pravilo spremnik razinu pristupa, možete ga mogućnost da biste prilagodili dozvole bez potrebe za izdavanje novi URL adrese za resurse zaštićena je zajednički pristup potpisa.
- Azure reda čekanja za aplikaciju resursi Azure redove obično koristi za obradu pogon ime klijenata, ali mogu se koristiti za raspodjelu radi što je potrebno za dodjelu resursa i upravljanje.
- Servis Bus redova za aplikaciju resursa koji ih gura rade na zajedničku servisa, možete koristiti jedan red gdje svakom pošiljatelju klijentu ima dozvole samo (kao što je izveden iz zahtjevima izdaje ACS) da biste na taj red, dok samo primatelja iz servisa da biste izvukli iz reda čekanja podatke koji dolaze iz više klijenata za dozvolu.


**Povezivanje i sigurnosnih servisa**

- Servis Bus Azure, infrastruktura za razmjenu poruka koji se nalazi između aplikacija što im omogućuje o razmjeni poruka slobodnije coupled način poboljšane skaliranja i otpornost.

**Servisi za povezivanje s mrežom**

Azure nudi nekoliko mrežnih usluga koji podržavaju provjeru autentičnosti i poboljšati mogućnost upravljanja koje smještene aplikacije. Ove usluge obuhvaćaju sljedeće:

- Azure virtualne mreže omogućuje vam Dodjela resursa i upravljanje virtualne privatne mreže (VPN-ovi) u Azure kao i sigurno povezivanje s lokalnim IT infrastrukturu.
- Virtualna mrežni promet Upravitelj omogućuje vam da biste učitali dolaznih promet saldo preko većem broju servisa Azure glavnom računalu li ih koristite u istom podatkovnog centra ili preko različitih podatkovnim centrima diljem svijeta.
- Azure Active Directory (Azure AD) je Moderna, OSTALE servis koji omogućuje identiteta upravljanje i pristup mogućnosti upravljanja za aplikacije oblaka. Pomoću Azure AD za Azure AD za omogućuje jednostavan način provjere autentičnosti i dopuštanja korisnicima da biste pristupili web-aplikacijama i servisima istovremeni značajke provjere autentičnosti i autorizacije da biste se factored iz kod resursima aplikacija.
- Azure Bus servis nudi sigurne razmjene poruka i distribuirati mogućnost protok podataka za i hibridnog aplikacija, kao što je komunikaciju između Azure hostirane aplikacije i lokalne aplikacije i servise, bez složene infrastructures vatrozid i sigurnost. Pomoću servisa Bus preusmjeravanja za resurse aplikacije servise koji su predstavljeni kao krajnje točke možda pripadate klijenta (na primjer, hostira izvan sustava, kao što je na lokaciji) ili možda je riječ o servisima dodjeli posebno za klijenta (jer povjerljive podatke specifične za klijent prelazi preko njih).



**Resursi za dodjelu resursa**

Azure nudi brojne načine klijenata za nove dodjele resursa za aplikaciju. Za složene aplikacije s velikim brojem klijenata je obično potrebno da biste automatizirali postupak omogućivanjem samoposlužno dodjele resursa.

- Uloga suradnika omogućuju dodjele resursa i de-dodjele po klijentu resursa (primjerice kada novi klijent predznaku gore ili otkazuje), prikupljanje metriku mjerenja koristiti te upravljanje skaliranje pratiti za određene raspored ili kao odgovor na sijeku pragovi Ključni pokazatelji uspješnosti. Ta uloga isti se mogu koristiti i za automatske out ažuriranja i nadogradnje rješenje.
- Azure blob-ova možete koristiti za dodjeljivanje računalnim ili unaprijed pokrenut prostora za pohranu resursa za novog klijenata tijekom pružanja spremnik razinu pristupa pravila za zaštitu s računalnim servisa paketa, VHD slike i ostale resurse.
- Mogućnosti za dodjelu resursa SQL baze podataka resursa za klijent obuhvaćaju sljedeće:

    -   DDL u skriptama ili uloženog kao resurse u skupine
    -   SQL Server 2008 R2 DAC paketa implementiran programski.
    -   Kopiranje iz glavnog reference baze podataka
    -   Dodjela nove baze podataka iz datoteke pomoću baze podataka uvoz i izvoz.



<!--links-->

[Hostiranje više klijentske aplikacije na Azure]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
