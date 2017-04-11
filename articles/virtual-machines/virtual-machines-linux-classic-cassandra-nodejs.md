<properties
    pageTitle="Mada Cassandra s Linux Azure | Microsoft Azure"
    description="Pokretanje Cassandra klaster na Linux virtualnim računalima sustava u Azure iz Node.js aplikacije"
    services="virtual-machines-linux"
    documentationCenter="nodejs"
    authors="hanuk"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="hanuk;robmcm"/>

# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>Radi Cassandra s Linux Azure i pristup s Node.js

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvesti sljedeće korake pomoću modela Voditelj resursa](https://azure.microsoft.com/documentation/templates/datastax-on-ubuntu/).

## <a name="overview"></a>Pregled
Microsoft Azure je platforme Otvori oblaka koja se pokreće i Microsoft kao softver i kao drugih proizvođača koji uključuje operacijski sustavi, poslužitelja aplikacije, razmjenu proizvod kao i baza podataka sustava SQL i NoSQL iz obje modela komercijalne i Otvori izvor. Gradnje prebacuju usluge na javno oblaka uključujući Azure zahtijeva Pažljivo planiranje i namjerne arhitektura za oba poslužitelje aplikacije kao Slojevi dobro prostora za pohranu. Arhitektura raspodijeljeno prostora za pohranu na Cassandra prirodan pomaže u iznimno dostupna sustavima koji kvara pogreške za klaster pogreške. Cassandra je skalu oblaka baze podataka NoSQL održava Apache softver Foundation pri cassandra.apache.org; Cassandra napisan Java i zato izvodi u oba sustava Windows, kao i Linux platformama.

Da biste prikazali Cassandra implementaciju na Ubuntu kao jedan i više podataka centra klaster korištenje virtualnim računalima sustava Microsoft Azure i virtualne mreže je žarište ovog članka. Uvođenje klaster za radnih opterećenja radnog optimiziran je izvan opsega ovog članka potrebno više disk čvor konfiguracije, odgovarajući Nazovi topologije dizajna i Modeliranje za podršku potrebne replikacije, dosljednost podataka, propusnost i preduvjeti visoke dostupnosti podataka.

Ovaj članak traje osnovna pristup da biste prikazali što je uključen u Cassandra klaster usporedbi Docker, Chef ili Puppet koje možete implementacije infrastrukture uvelike olakšali.  

## <a name="the-deployment-models"></a>Modeli implementacije
Povezivanje s mrežom Microsoft Azure omogućuje implementacije Izolirani privatne klastere razine pristupa koja može biti ograničeno na postići zaštita precizno grained mreže.  Budući da je ovaj članak o prikazivanju implementaciju Cassandra temeljne razine, ne možemo se ne fokus na razini dosljednost i dizajn optimalnih prostora za pohranu za propusnost. Slijedi popis umrežavanje preduvjeti za naše hipotetska klaster:

- Vanjski sustavi ne može pristupiti Cassandra bazu podataka unutar ili izvan Azure
- Klaster Cassandra mora biti iza opterećenja thrift promet
- Implementacija Cassandra čvorove u dvije grupe u svaku podatkovnog centra za raspoloživost Poboljšana klaster
- Zaključati klaster pa koje samo farme poslužitelja aplikacije izravno ima pristup bazi podataka
- Javni umrežavanje krajnje točke osim SSH
- Svaki Cassandra čvor mora fixed interne IP adrese

Cassandra može uvesti jedno područje Azure ili više područja na temelju raspodijeljeno prirode povećavaju. Model implementacije više područja se može leveraged da bi služio krajnjim korisnicima bliže određeni Zemljopis putem iste Cassandra infrastrukture. Cassandra, ugrađene čvor replikacije traje Oprez sinkronizacije više matrica piše s više centre za podatke i prikazuje dosljedan prikaz podataka u aplikacijama. Uvođenje više područja mogu pomoći s ublažiti rizik od šira kvarove Azure servisa. Cassandra na tunable dosljednost i replikacije topologije će pomoći u različite potrebe RPO aplikacija za sastanak.

### <a name="single-region-deployment"></a>Uvođenje jedno područje
Ne možemo će započnite s jedno područje implementacije i Festival sredine learnings u stvaranju model više područja. Azure virtualne mreže će se koristiti za stvaranje Izolirani podmreže tako da se prethodno navedenim sigurnosne preduvjete mreže može biti zadovoljen.  Postupak opisan u stvaranju implementacije jedno područje koristi Ubuntu 14.04 LTS i Cassandra 2.08; Međutim, postupak se prihvatile jednostavno da biste druge varijante Linux. Evo nekih systemic osobine implementaciju jedno područje.  

**Visoke dostupnosti:** Čvorovi Cassandra prikazano slika 1 su implementiran na dva skupa dostupnost tako da se širi čvorove između više domena kvara za visoke dostupnosti. VMs dodavati napomene uz svaki skup dostupnost mapirani 2 kvara domene.  Koristi Microsoft Azure pojam kvara domene za upravljanje vremenom (npr. hardver ili softver neuspjeha) neplanirano prema dolje dok pojam nadogradnje domene (primjerice glavno računalo ili gost OS zakrpa/nadogradnje, nadogradnje aplikacije) koristi se za upravljanje zakazano put. Potražite u članku [oporavak Izrada i visoke dostupnosti za Azure aplikacije](http://msdn.microsoft.com/library/dn251004.aspx) za ulogu kvara i nadogradite domena u attaining visoke dostupnosti.

![Uvođenje jedno područje](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux1.png)

Slika 1: Jedno područje implementacije

Imajte na umu da u vrijeme pisanja ovog Azure ne dopušta eksplicitnih mapiranje grupe VMs s domenom određene kvara; Dakle, čak i uz implementaciju model prikazano slika 1, a je statistically probable sve virtualnih računala može se mapirati u dvije kvara domene umjesto četiri.

**Opterećenja promet Thrift:** Thrift klijent biblioteke unutar web-poslužitelj povežite se s klaster putem interne opterećenja. Za tu radnju postupak dodavanja Interna opterećenja podmreže "data" (pogledajte slika 1) u kontekstu hostinga klaster Cassandra servisa u oblaku. Kada je definiran Interna opterećenja, svaki čvor zahtijeva rasporediti opterećenje krajnju točku koja će se dodati s primjedbama skupa rasporediti opterećenje s nazivom raspoređivača opterećenja prethodno definirane. Potražite u članku [Azure Interna opterećenja ](../load-balancer/load-balancer-internal-overview.md)više pojedinosti.

**Skupine sjemenke:** Važno da biste odabrali čvorove Većina iznimno dostupne za sjemenke kao novi čvorovi će komunicirati s čvorove Početni broj da biste otkrili topologije klaster je. Jedan čvor iz svaki skup dostupnost je označen kao Početni broj čvorove da biste izbjegli jednu točku nije uspjelo.

**Varijance s replikacijom i dosljednost razinu:** Cassandra na Sastavi u visoku dostupnost i podataka rok trajanja je određene argumentima faktor replikacije (RF - broj primjeraka svakog retka pohranjenu na klaster) i dosljednost razinu (broj replike biti čitanje/napisanih prije vraćanja rezultat pozivatelju). Varijance s replikacijom navedeni su prilikom stvaranja KEYSPACE (slično relacijske baze podataka) dok razinu dosljednost navedena je prilikom izdavanja CRUD upita. Potražite u dokumentaciji Cassandra [Konfiguriranje dosljednost](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) detalje dosljednost i formula za izračunavanje kvorum.

Cassandra podržava dvije vrste modela iz integriteta podataka – dosljednost i usmjerenog dosljednost; varijance s replikacijom i dosljednost razinu zajedno određuje ako podaci budu dosljedni čim dovršetka operacija pisanja ili će biti naposljetku dosljedni. Na primjer, određivanje KVORUM razinu dosljednost uvijek će omogućuje čitanje podataka dosljednost uz bilo kojoj razini dosljednost, ispod broja replike pisana ako želite postići KVORUM (npr. JEDNE) rezultira podaci naposljetku dosljedni.

Klaster 8 čvor uz faktor replikacije brojeva 3 i KVORUM gore navedenoj sintaksi, (2 čvorove čitati se ili sastavljene za dosljednost) čitanje/pisanje dosljednost razine, možete preživi theoretical gubitak od najviše 1 čvor po grupi replikacije prije početka aplikacije pazeći pogreške. Pretpostavlja da sve ključne razmake dobro imati raspoređen čitanje/pisanje zahtjeva.  Slijede parametri koristimo za distribuiranih klaster:

Jedno područje Cassandra klaster konfiguracija:

| Parametar klaster | Vrijednost | Napomene |
| ----------------- | ----- | ------- |
| Broj čvorovi (N) | 8   | Ukupan broj čvorove u skupine |
| Replikacija varijance (RF) | 3 | Broj replike navedeni retka |
| Razina dosljednost (pisanje) | QUORUM[(RF/2) +1) = 2] rezultat formule se zaokružuje prema dolje | Piše od najviše 2 replike prije slanja odgovora pozivatelju; 3 replike napisan na koncu dosljedan način. |
| Razina dosljednost (za čitanje) | KVORUM [(RF/2) + 1 = 2] rezultat formule se zaokružuje prema dolje | Čita 2 replike prije slanja odgovora pozivatelju. |
| Strategije replikacije | NetworkTopologyStrategy potražite u članku [Replikacije podataka](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) u dokumentaciji Cassandra dodatne informacije | Možete koristiti topologije implementacije i smješta replike na čvorove tako da sva replike ne završavaju na istom bicikle |
| Snitch | GossipingPropertyFileSnitch potražite u članku [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) u dokumentaciji Cassandra dodatne informacije | NetworkTopologyStrategy koristi pojam snitch da biste shvatili topologije. GossipingPropertyFileSnitch daje Bolja kontrola u mapiranje svaki čvor na web-mjesto centra za podatke i u okvir za bicikle. Klaster koristi ogovaranje proširiti taj podatak. Ovo je mnogo jednostavnijim postavkom dinamičke IP odnosu PropertyFileSnitch |


**Azure zahtjevi za Cassandra klaster:** Mogućnost virtualnim računalima sustava Microsoft Azure koristi spremište blobova platforme Azure postojanost disk; Azure prostora za pohranu sprema 3 replike svakom disku za visoke rok trajanja. To znači da svaki redak podataka umetnutih u tablicu Cassandra već pohranjena u 3 replike i zato dosljednost podataka je već snimili brigu o čak i ako se faktor replikacije (RF) je 1. Glavni problem s replikacijom faktor 1 u tijeku je da aplikacija će se dogoditi nedostupnost čak i ako se ne uspije jedan čvor Cassandra. Međutim, ako je čvor prema dolje za probleme (npr. hardver, neuspješne softvera sustava) prepoznaje kontroler tkanina Azure, ga će Dodjela novi čvor umjesto nje koristeći istu pogona prostora za pohranu. Dodjeljivanje novi čvor da biste zamijenili staru može potrajati nekoliko minuta.  Na sličan način planiranog održavanja aktivnosti kao što su promjene OS za goste, Cassandra nadograđuje i promjene aplikacije Azure tkanina kontroler izvodi vodoravnim nadogradnje čvorove u klasteru.  Vodoravnim nadogradnje također može potrajati dolje nekoliko čvorove istovremeno i zato klaster primijetiti isključiti kratko za nekoliko particije. Međutim, podaci neće biti prekinuta zbog ugrađene zalihosti Azure prostora za pohranu.  

Za sustave implementiran na Azure koji ne zahtijevaju visoke dostupnosti (primjerice oko 99.9 koja je jednaka 8.76 sati/godina; detalje potražite u članku [Visoke dostupnosti](http://en.wikipedia.org/wiki/High_availability) ) možda ćete moći pokrenuti s RF = 1 i dosljednost razinu = JEDAN.  Za aplikacije potrebe visoke dostupnosti, RF = 3 i dosljednost razinu = KVORUM će tolerate dolje vrijeme jedne od čvorovi od na replike. RF = 1 u tradicionalni implementacije (npr. na lokaciji) nije moguće koristiti zbog gubitka mogućih podataka koja je rezultat probleme kao što su pogreške na disku.   

## <a name="multi-region-deployment"></a>Uvođenje regije
Cassandra-podataka – centar za-umu replikacije i dosljednost modela prethodno opisan da biste implementaciju više područja iz okvira bez potrebe za sve vanjske tooling. To je prilično razlikuje od tradicionalni relacijske baze podataka pri čemu može biti vrlo složen instalacijski program za baze podataka sustava za više osnovne zapisivanja. Cassandra u više područja postavljanje pomoći scenariji za korištenje uključujući sljedeće:

**Blizina temelji implementacije:** Više klijentske aplikacije, s Očisti mapiranje korisnika klijentu-na-regiju, možete se benefited prema niskoj latencies klaster više područja. Na primjer učenje upravljanje sustavi za obrazovne ustanove možete implementirati raspodijeljeno klaster u Istočni SAD-u i Zapad sad regijama da bi služio odgovarajući campuses za transakcijskih kao i analize. Podatke možete biti lokalno dosljedan na vrijeme čitanja i pisanja, a može biti naposljetku dosljedan preko obje regije. Postoje još primjera kao što su distribucija medijskih sadržaja, e-trgovine i ništa i sve što služi zemlj concentrated korisnik osnovni je iskorištavanje slučaj za ovaj model implementacije.

**Visoke dostupnosti:** Zalihosti je ključa faktor u attaining visoke dostupnosti softvera i hardvera; potražite u članku sastavnih pouzdanog oblaka sustavi na Microsoft Azure detalje. Na Microsoft Azure samo pouzdan način od zadanih true zalihosti je implementacija klaster više područja. Aplikacije koje će biti implementirano u načinu aktivno aktivan "ili" aktivno pasivni, a ako je jedan od regija prema dolje, Upravitelj promet Azure preusmjeravanje prometa na aktivni regija.  U implementaciji jedno područje ako je dostupnost 99.9, dva područja implementacije možete postići raspoloživost 99.9999 izračunati formula: (1-(1-0.999) *(1-0.999))*100); iznad papira detalje potražite u članku.

**Izrada oporavak:** Više područja Cassandra klaster, ako je ispravno namijenjena, možete withstand kvarove centar do teškog oštećenja podataka. Ako je određenu regiju prema dolje, aplikacija implementiran na drugim regijama možete pokrenuti posluživanje krajnjim korisnicima. Kao što je sve druge poslovne continuity implementacije, aplikacija mora biti pogreške za gubitak podataka koja je rezultat podatke u asinkronog kanal. Međutim, Cassandra čini oporavka mnogo swifter od vremena zauzima procesa oporavak običnoj bazi podataka. Slika 2 prikazuje model uobičajeni regije implementaciju s osam čvorovi u svakom području. Obje regije su zrcalne slike međusobno na isti simetrije; stvarnog svijeta dizajna ovise o radno opterećenje vrstu (npr. transakcijskih ili analytical), RPO, RTO, dosljednost podataka i preduvjeti za dostupnost.

![Višestruki regija implementacije](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux2.png)

Slika 2: Više područja Cassandra implementacije

### <a name="network-integration"></a>Integracija s mrežom
Skupovi virtualnim strojevima implementiran na privatne mreže koja se nalazi na dva područja komunicira međusobno pomoću VPN tunelom. Tunelom VPN povezuje dva softver pristupnika dodjeli tijekom postupka implementacije mreže. Obje regije imaju slične arhitektura mreže pomoću podmreže "web" i "data"; Azure umrežavanje omogućuje stvaranje proizvoljan broj podmreže prema potrebi i primijenite ACL-a prema potrebi po mrežne sigurnosti. Prilikom dizajniranja topologije klaster suč Ekonomske učinak potrebe za mrežni promet smatraju i Latencija komunikacije centar za podataka.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Dosljednost podataka više podatkovnog centra implementacije
Raspodijeljeno implementacijama treba imati na umu klaster topologije utjecaj na propusnost i visoke dostupnosti. RF i dosljednost razinu moraju biti odabran takav način da na kvorum ne ovisi o dostupnosti sve Centre za podatke.
Za sustav koji je potrebno visoke dosljednost LOCAL_QUORUM dosljednost razine (za čitanja i pisanja) će provjerite da lokalne čitanja i pisanja su zadovoljena iz lokalne čvorove dok asinkrono replicirati podataka na centri za udaljeni podaci.  Tablica 2 navedene konfiguracije pojedinosti klaster regije navedene u nastavku pisanje prema gore.

**Dva područja Cassandra klaster konfiguracija**


| Parametar klaster | Vrijednost | Napomene |
| ----------------- | ----- | ------- |
| Broj čvorovi (N) | 8 + 8 | Ukupan broj čvorove u skupine |
| Replikacija varijance (RF) | 3 | Broj replike navedeni retka |
| Razina dosljednost (pisanje) | LOCAL_QUORUM [(sum(RF)/2) +1) = 4] rezultat formule se zaokružuje prema dolje | 2 čvorove će biti napisani podatkovnog centra u prvom sinkronizirano; Dodatni 2 čvorove koja su potrebna za kvorum asinkrono biti napisani 2 podatkovnog centra. |
| Razina dosljednost (za čitanje) | LOCAL_QUORUM ((RF/2) + 1) = 2 rezultat formule se zaokružuje prema dolje | Zahtjevi za čitanje su zadovoljena iz samo jednog područja; 2 čvorove su za čitanje prije slanja odgovora natrag na klijentu. |
| Strategije replikacije | NetworkTopologyStrategy potražite u članku [Replikacije podataka](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) u dokumentaciji Cassandra dodatne informacije | Možete koristiti topologije implementacije i smješta replike na čvorove tako da sva replike ne završavaju na istom bicikle |
| Snitch | GossipingPropertyFileSnitch potražite u članku [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) u dokumentaciji Cassandra dodatne informacije | NetworkTopologyStrategy koristi pojam snitch da biste shvatili topologije. GossipingPropertyFileSnitch daje Bolja kontrola u mapiranje svaki čvor na web-mjesto centra za podatke i u okvir za bicikle. Klaster koristi ogovaranje proširiti taj podatak. Ovo je mnogo jednostavnijim postavkom dinamičke IP odnosu PropertyFileSnitch |


##<a name="the-software-configuration"></a>KONFIGURIRANJE SOFTVER
Sljedeće verzije softver koriste tijekom uvođenja:

<table>
<tr><th>Softver</th><th>Izvor</th><th>Verzija</th></tr>
<tr><td>JRE </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu  </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

Budući da preuzimanje JRE zahtijeva ručno prihvaćanje Oracle licence, da biste pojednostavnili implementaciju, preuzmite potrebni softver na radnu površinu za kasnije prijenos u sliku predložak Ubuntu ćemo hoće li stvarati kao precursor za implementaciju klaster.

Preuzmite iznad softver u poznati preuzimanje imenik (npr. %TEMP%/downloads u sustavu Windows ili ~/Downloads na većini Linux distribucija ili Mac) na lokalnom računalu.

### <a name="create-ubuntu-vm"></a>STVARANJE UBUNTU VM
U ovom ćete koraku postupka smo stvorit će Ubuntu sliku sa softverom za da biste tako da se slika se može ponovno koristiti za dodjelu resursa nekoliko Cassandra čvorove.  
####<a name="step-1-generate-ssh-key-pair"></a>KORAK 1: Stvaranje SSH ključa par
Azure mora se X509 javni ključ koji je PEM ili DER kodirana prilikom dodjele resursa. Generiranje javno/privatno ključa par prema uputama nalazi se na upute za korištenje SSH s Linux na Azure. Ako namjeravate koristiti putty.exe kao SSH klijent sustava Windows ili Linux potrebno pretvoriti PEM kodirani RSA privatni ključ PPK obliku pomoću puttygen.exe; upute za to nalazi se u gornjem web-stranicu.

####<a name="step-2-create-ubuntu-template-vm"></a>KORAK 2: Stvaranje predloška Ubuntu VM
Da biste stvorili predložak VM, prijavite se na portal sustava Azure klasični i koristiti sljedeće niz: kliknite NOVO, RAČUNALNIM, VIRTUALNOG računala, iz GALERIJE, UBUNTU, Ubuntu Server 14.04 LTS, a zatim kliknite strelicu desno. Praktični vodič koji opisuje kako stvoriti Linux VM, potražite u članku Stvaranje virtualnog računala radi Linux.

Na zaslonu "virtualnog računala konfiguracije" #1 unesite sljedeće podatke:

<table>
<tr><th>NAZIV POLJA              </td><td>       VRIJEDNOST POLJA               </td><td>         NAPOMENE                </td><tr>
<tr><td>DATUM IZDANJA VERZIJA    </td><td> Odaberite datum u drow prema dolje</td><td></td><tr>
<tr><td>NAZIV VIRTUALNOG RAČUNALA    </td><td> cass predloška                </td><td> Ovo je glavnog računala na VM </td><tr>
<tr><td>RAZINA                     </td><td> STANDARDNA                        </td><td> Ostavite zadani              </td><tr>
<tr><td>VELIČINA                     </td><td> A1                              </td><td>Odaberite na VM na temelju UI mora; u tu svrhu ostavite zadani </td><tr>
<tr><td> NOVO KORISNIČKO IME           </td><td> localadmin                      </td><td> "administrator" je rezervirana korisničkog imena u Ubuntu 12. xx i poslije</td><tr>
<tr><td> PROVJERA AUTENTIČNOSTI      </td><td> Kliknite potvrdni okvir                 </td><td>Potvrdite ako želite sigurne pomoću ključa SSH </td><tr>
<tr><td> CERTIFIKAT             </td><td> Naziv datoteke certifikata javnog ključa </td><td> Korištenje javni ključ generira prethodno</td><tr>
<tr><td> Novu lozinku   </td><td> neprobojne lozinke </td><td> </td><tr>
<tr><td> Potvrdite lozinku   </td><td> neprobojne lozinke </td><td></td><tr>
</table>

Na zaslonu "virtualnog računala konfiguracije" #2, unesite sljedeće podatke:

<table>
<tr><th>NAZIV POLJA             </th><th> VRIJEDNOST POLJA                       </th><th> NAPOMENE                                 </th></tr>
<tr><td> SERVIS U OBLAKU  </td><td> Stvaranje nove servise u oblaku    </td><td>Servis u oblaku je resursi za računalnim spremnika kao što su virtualnim strojevima</td></tr>
<tr><td> NAZIV DNS SERVISA OBLAKA. </td><td>ubuntu template.cloudapp.net   </td><td>Dajte naziv raspoređivača učitavanja agnostic računalu</td></tr>
<tr><td> PODRUČJE/GRUPE AFINITET/VIRTUALNE MREŽE </td><td>    Zapad SAD-a </td><td> Odaberite područje s kojeg web-aplikacijama pristup Cassandra klaster</td></tr>
<tr><td>RAČUN ZA POHRANU </td><td>   Koristite zadani </td><td>Korištenje računa za zadani prostor za pohranu ili računa unaprijed stvoreni prostora za pohranu unutar određenog područja</td></tr>
<tr><td>POSTAVLJANJE DOSTUPNOSTI </td><td>  Ništa </td><td>  Ostavite prazno</td></tr>
<tr><td>KRAJNJE TOČKE   </td><td>Koristite zadani </td><td>  Konfiguracija SSH zadani </td></tr>
</table>

Kliknite strelicu desno, ostavite na zadane postavke na zaslonu #3, a zatim gumb "potvrdite" da biste dovršili VM dodjeljivanje postupak. Nakon nekoliko minuta VM s nazivom "ubuntu-predloškom" mora biti u status "izvodi".

###<a name="install-the-necessary-software"></a>INSTALACIJA NUŽNIH SOFTVERA
####<a name="step-1-upload-tarballs"></a>KORAK 1: Prijenos tarballs
Koristite pronađenim ili pscp, kopirajte prethodno preuzeti softver ~/downloads direktorij koristeći oblik sljedeće naredbe:

#####<a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp poslužitelj-jre-8u5-linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz

Ponovite gore naredbu za JRE kao i kao Cassandra bitova.

####<a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>KORAK 2: Priprema strukturu direktorija i izdvajanje na arhive
Prijavite se na VM i stvoriti strukturu direktorija i izdvojiti softver kao super korisnik pomoću tulumu skripte u nastavku:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change the ownership to the service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile to add JRE to the PATH"
    echo "installation is complete"


Ako zalijepite ovu skriptu u prozoru vim, pripazite da biste uklonili u novi red ('\r ") pomoću naredbe za sljedeće:

    tr -d '\r' <infile.sh >outfile.sh

####<a name="step-3-edit-etcprofile"></a>Korak 3: Uređivanje itd/profila
Dodavanjem sljedeće na kraju:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

####<a name="step-4-install-jna-for-production-systems"></a>Korak 4: Instalacija JNA za sustave proizvodnje
Koristite sljedeće naredbe niz: sljedeću naredbu će instalirajte jna 3.2.7.jar a jna-platformu-3.2.7.jar /usr/share.java direktorij sudo Zemaljska get libjna java

Stvaranje simboličke veze u direktoriju CASS_HOME/biblioteka $ tako da se Cassandra skriptu za pokretanje možete pronaći te staklenke:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

####<a name="step-5-configure-cassandrayaml"></a>Korak 5: Konfiguriranje cassandra.yaml
Uređivanje cassandra.yaml na svakom VM u skladu s vizualnim potrebna tako da sva virtualnim strojevima [smo će dotjerati to tijekom stvarni dodjeljivanja] konfiguracija:

<table>
<tr><th>Naziv polja   </th><th> Vrijednost  </th><th> Napomene </th></tr>
<tr><td>cluster_name </td><td>  "CustomerService"   </td><td> Koristite naziv koji označava implementaciju sustava</td></tr>
<tr><td>listen_address  </td><td>[ostavite prazno]   </td><td> Brisanje "localhost" </td></tr>
<tr><td>rpc_addres   </td><td>[ostavite prazno]  </td><td> Brisanje "localhost" </td></tr>
<tr><td>sjemenke   </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8" </td><td>Popis svih IP adresa koji su označeni kao sjemenke.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Koristi se tako da na NetworkTopologyStrateg za inferring web-mjesto centra za podatke i u okvir za bicikle u VM</td></tr>
</table>

####<a name="step-6-capture-the-vm-image"></a>Korak 6: Snimiti VM
Prijavite se u virtualnog računala pomoću glavnog računala (hk-CA-template.cloudapp.net) i privatni ključ SSH prethodno stvorili. Pogledajte kako se koristi SSH s Linux na Azure detalje o tome kako se prijaviti pomoću naredbe ssh ili putty.exe.

Izvođenje sljedeće niza akcija da biste snimili sliku:
#####<a name="1-deprovision"></a>1. deprovision
Pomoću naredbe "sudo waagent – deprovision + korisnika" da biste uklonili virtualnog računala instancu određenih informacija. Potražite u članku [kako snimiti Linux virtualnog računala](virtual-machines-linux-classic-capture-image.md) koristi kao predložak dodatne detalje na proces snimanja slike.

#####<a name="2-shutdown-the-vm"></a>2: isključivanja s VM
Provjerite je li istaknuta je virtualnog računala, a zatim kliknite vezu ISKLJUČIVANJA s trake s naredbama dolje.

#####<a name="3-capture-the-image"></a>3: snimiti
Provjerite je li istaknuta je virtualnog računala, a zatim kliknite vezu SNIMKU s trake s naredbama dolje. Na sljedećem zaslonu dajte naziv SLIKE (primjerice hk-cas-2-08-ub-14-04-2014071), odgovarajuće opis SLIKE pa kliknite "" kvačicu da biste dovršili postupak za snimanje.

To može potrajati nekoliko sekundi i slika mora biti dostupna u odjeljku MOJA SLIKA Galerija slika. Izvor VM bit će automatski delated nakon slike uspješno zabilježene.

##<a name="single-region-deployment-process"></a>Postupak implementacije jedno područje
**Korak 1: Stvaranje virtualne mreže** Prijavite se na portal sustava Azure klasični i stvorite virtualne mreže i atribute Prikaži u tablici. Detaljne korake postupka potražite u članku [Konfiguriranje Cloud-Only virtualne mreže na portalu za Azure klasični](../virtual-network/virtual-networks-create-vnet-classic-portal.md) .      

<table>
<tr><th>Naziv atributa VM</th><th>Vrijednost</th><th>Napomene</th></tr>
<tr><td>Ime</td><td>vnet-cass-Zapad-hr</td><td></td></tr>
<tr><td>Regija</td><td>Zapad SAD-a</td><td></td></tr>
<tr><td>DNS poslužitelji </td><td>Ništa</td><td>Zanemarivanje kao što smo ne koristite DNS poslužitelj</td></tr>
<tr><td>Konfiguriranje mjesta točke VPN-a</td><td>Ništa</td><td> Zanemari ovo</td></tr>
<tr><td>Konfiguriranje web-mjesto VPN-a</td><td>Nnone</td><td> Zanemari ovo</td></tr>
<tr><td>Prostor adrese</td><td>10.1.0.0/16</td><td></td></tr>
<tr><td>Pokretanje IP</td><td>10.1.0.0</td><td></td></tr>
<tr><td>CIDR </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Dodajte sljedeće podmreže:

<table>
<tr><th>Ime</th><th>Pokretanje IP</th><th>CIDR</th><th>Napomene</th></tr>
<tr><td>web</td><td>10.1.1.0</td><td>/ 24 (251)</td><td>Podmreže za web-farme</td></tr>
<tr><td>podataka</td><td>10.1.2.0</td><td>/ 24 (251)</td><td>Podmreže za čvorove baze podataka</td></tr>
</table>

Podaci i podmreže Web možete zaštititi putem mreže sigurnosnih grupa opseg koji je izvan opsega ovog članka.  

**Korak 2: Dodjela virtualnim strojevima** Pomoću slika stvoreno, ne možemo će stvoriti virtualnim računalima sustava u oblaku server "hk c-svc-Zapadni" i ih povezati s odgovarajući podmreže, kao što je prikazano u nastavku:

<table>
<tr><th>Naziv računala    </th><th>Podmreže </th><th>IP adresa </th><th>Postavljanje dostupnosti</th><th>Kontroler/za bicikle</th><th>Početni broj?</th></tr>
<tr><td>HK-c1-Zapad-hr   </td><td>podataka   </td><td>10.1.2.4   </td><td>HK-c-aset-1    </td><td>kontroler = WESTUS za bicikle = rack1 </td><td>Da</td></tr>
<tr><td>HK-c2-Zapad-hr   </td><td>podataka   </td><td>10.1.2.5   </td><td>HK-c-aset-1    </td><td>kontroler = WESTUS za bicikle = rack1 </td><td>ne </td></tr>
<tr><td>HK-c3-Zapad-hr   </td><td>podataka   </td><td>10.1.2.6   </td><td>HK-c-aset-1    </td><td>kontroler = WESTUS za bicikle = rack2 </td><td>Da</td></tr>
<tr><td>HK-c4-Zapad-hr   </td><td>podataka   </td><td>10.1.2.7   </td><td>HK-c-aset-1    </td><td>kontroler = WESTUS za bicikle = rack2 </td><td>ne </td></tr>
<tr><td>HK-c5-Zapad-hr   </td><td>podataka   </td><td>10.1.2.8   </td><td>HK-c-aset-2    </td><td>kontroler = WESTUS za bicikle = rack3 </td><td>Da</td></tr>
<tr><td>HK-c6-Zapad-hr   </td><td>podataka   </td><td>10.1.2.9   </td><td>HK-c-aset-2    </td><td>kontroler = WESTUS za bicikle = rack3 </td><td>ne </td></tr>
<tr><td>HK-c7-Zapad-hr   </td><td>podataka   </td><td>10.1.2.10  </td><td>HK-c-aset-2    </td><td>kontroler = WESTUS za bicikle = rack4 </td><td>Da</td></tr>
<tr><td>HK-c8-Zapad-hr   </td><td>podataka   </td><td>10.1.2.11  </td><td>HK-c-aset-2    </td><td>kontroler = WESTUS za bicikle = rack4 </td><td>ne </td></tr>
<tr><td>HK-w1-Zapad-hr   </td><td>web    </td><td>10.1.1.4   </td><td>HK-w-aset-1    </td><td>                       </td><td>N/D</td></tr>
<tr><td>HK-w2-Zapad-hr   </td><td>web    </td><td>10.1.1.5   </td><td>HK-w-aset-1    </td><td>                       </td><td>N/D</td></tr>
</table>

Stvaranje iznad popisa VMs potrebne su sljedeći postupak:

1.  Stvaranje prazne oblaku unutar određenog područja
2.  Stvaranje na VM iz prethodno snimljenu sliku i priložili virtualne mreže stvoreno; Ponovite to za sve VMs
3.  Dodavanje internog opterećenja servisa u oblaku i priložite podmreže "data"
4.  Za svaki VM stvoreno, dodajte krajnje rasporediti opterećenje za thrift promet kroz skup rasporediti opterećenje s prethodno stvorena Interna opterećenja

Postupak iznad izvršavaju pomoću Azure klasični portala; korištenje računala za Windows (koristi VM na Azure ako nemate pristup računala za Windows), koristite sljedeću skriptu komponente PowerShell Dodjela sve 8 VMs automatski.

**Popis 1: Skriptu PowerShell za dodjelu resursa virtualnim strojevima**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. crate a list of VMs from the template

        #fundamental variables - change these to reflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores the list of azure vm configuration objects
        #create the list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add add the thrift endpoint to the internal load balancer for all the VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**Korak 3: Konfiguriranje Cassandra na svakom VM**

Prijavite se na VM i učinite sljedeće:

* Uređivanje $CASS_HOME/conf/cassandra-rackdc.properties da biste odredili svojstva web-mjesto centra za i u okvir za bicikle podataka:

       dc =EASTUS, rack =rack1

* Uređivanje cassandra.yaml da biste konfigurirali čvorove Početni broj kao ispod:

       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Korak 4: Pokretanje u VMs i testiranje klaster**

Prijavite se u jednu čvorove (npr. hk-c1-Zapad-HR), a zatim pokrenite sljedeću naredbu da biste vidjeli status skupine:

       nodetool –h 10.1.2.4 –p 7199 status

Trebali biste vidjeti slično onome ispod za na 8 čvor klaster prikaza:

<table>
<tr><th>Status</th><th>Adresa  </th><th>Učitavanje   </th><th>Tokeni </th><th>Vlasništvu </th><th>ID glavnog računala  </th><th>Za bicikle</th></tr>
<tr><th>PONIŠTAVANJE  </td><td>10.1.2.4   </td><td>87.81 KB   </td><td>256    </td><td>38.0%  </td><td>GUID (uklanja)</td><td>rack1</td></tr>
<tr><th>PONIŠTAVANJE  </td><td>10.1.2.5   </td><td>41.08 KB   </td><td>256    </td><td>68.9%  </td><td>GUID (uklanja)</td><td>rack1</td></tr>
<tr><th>PONIŠTAVANJE  </td><td>10.1.2.6   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (uklanja)</td><td>rack2</td></tr>
<tr><th>PONIŠTAVANJE  </td><td>10.1.2.7   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (uklanja)</td><td>rack2</td></tr>
<tr><th>PONIŠTAVANJE  </td><td>10.1.2.8   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (uklanja)</td><td>rack3</td></tr>
<tr><th>PONIŠTAVANJE  </td><td>10.1.2.9   </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (uklanja)</td><td>rack3</td></tr>
<tr><th>PONIŠTAVANJE  </td><td>10.1.2.10  </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (uklanja)</td><td>rack4</td></tr>
<tr><th>PONIŠTAVANJE  </td><td>10.1.2.11  </td><td>55.29 KB   </td><td>256    </td><td>68.8%  </td><td>GUID (uklanja)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Testiranje klaster jedno područje
Da biste testirali klaster, poduzmite sljedeće korake:

1.    Pomoću cmdleta Get-AzureInternalLoadbalancer za naredbu Powershell dohvaćanje IP adrese sustava interne opterećenja (npr.  10.1.2.101). Vidjet ćete da Sintaksa naredbe prikazano u nastavku: Get-AzureLoadbalancer – naziv servisa "hk-c-svc-Zapad-hr" [prikazuje detalje o Interna raspoređivača opterećenja uz IP adrese]
2.  Prijava na web-farme VM (npr. hk-w1-Zapad-HR) pomoću Putty ili ssh
3.  Izvršavanje $CASS_HOME/smeće/cqlsh 10.1.2.101 9160
4.  Da biste provjerili radi li klaster, koristite sljedeće naredbe CQL:

        CREATE KEYSPACE customers_ks WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 };
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');

        SELECT * FROM Customers;

Trebali biste vidjeti prikaz kao ispod:

<table>
  <tr><th> customer_id </th><th> ime </th><th> Prezime </th></tr>
  <tr><td> 1 </td><td> Ivica </td><td> N. </td></tr>
  <tr><td> 2 </td><td> N. </td><td> N. </td></tr>
</table>

Uzmite u obzir keyspace stvorili u koraku 4 koristi SimpleStrategy s replication_factor od 3. SimpleStrategy preporučuje jedan podatkovni centar implementacijama dok NetworkTopologyStrategy više podataka centriranje implementacije. Replication_factor od 3 steći toleranciju na pogreške čvor pogreške.

##<a id="tworegion"> </a>Postupak implementacije regije
Bit će pod utjecajem implementacije jedno područje dovršiti i ponovite isti postupak za instalaciju drugu regiju. Ključne razlike između implementacije jednostrukih i višestrukih regija je tunelom postavljanje VPN-a za komunikaciju među regija; ne možemo će započeti s instalacijom mreže, Dodjela resursa u VMs i konfiguriranje Cassandra.

###<a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>Korak 1: Stvaranje virtualne mreže na područje 2.
Prijavite se na portal sustava Azure klasični i stvorite virtualne mreže i atribute Prikaži u tablici. Detaljne korake postupka potražite u članku [Konfiguriranje Cloud-Only virtualne mreže na portalu za Azure klasični](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) .      

<table>
<tr><th>Naziv atributa    </th><th>Vrijednost    </th><th>Napomene</th></tr>
<tr><td>Ime    </td><td>vnet-cass-Istok-hr</td><td></td></tr>
<tr><td>Regija  </td><td>Istočni SAD-a</td><td></td></tr>
<tr><td>DNS poslužitelji     </td><td></td><td>Zanemarivanje kao što smo ne koristite DNS poslužitelj</td></tr>
<tr><td>Konfiguriranje mjesta točke VPN-a</td><td></td><td>     Zanemari ovo</td></tr>
<tr><td>Konfiguriranje web-mjesto VPN-a</td><td></td><td>      Zanemari ovo</td></tr>
<tr><td>Prostor adrese   </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>Pokretanje IP </td><td>10.2.0.0   </td><td></td></tr>
<tr><td>CIDR    </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Dodajte sljedeće podmreže:
<table>
<tr><th>Ime    </th><th>Pokretanje IP    </th><th>CIDR   </th><th>Napomene</th></tr>
<tr><td>web </td><td>10.2.1.0   </td><td>/ 24 (251)  </td><td>Podmreže za web-farme</td></tr>
<tr><td>podataka    </td><td>10.2.2.0   </td><td>/ 24 (251)  </td><td>Podmreže za čvorove baze podataka</td></tr>
</table>


###<a name="step-2-create-local-networks"></a>Korak 2: Stvaranje lokalne mreže
Lokalne mreže u Azure virtualne mreže je prostor za proxy adresu koja mapira udaljenom mjestu uključujući privatne oblaka ili neke druge regije Azure. Taj prostor proxy adrese povezana je s udaljenog pristupnik za usmjeravanje mrežu desnom umrežavanje odredišta. Upute za uspostavljanje veze VNET VNET potražite u članku [Konfiguriranje VNet VNet vezu](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) .

Stvaranje dvije lokalne mreže po sljedeće detalje:

| Network Name | Adresa pristupnika VPN-a | Prostor adrese | Napomene |
| ------------ | ------------------- | ------------- | ------- |
| HK-lnet-Map-to-East-US | 23.1.1.1  | 10.2.0.0/16   | Prilikom stvaranja lokalne mreže dati rezerviranog mjesta adresa pristupnika. Adresa realni pristupnika uneseni nakon stvaranja pristupnika. Provjerite je li adresni prostor u potpunosti podudara odgovarajući udaljene VNET; u ovom slučaju na VNET stvorena u regiji Istočni SAD-a. |
| HK-lnet-Map-to-West-US | 23.2.2.2  | 10.1.0.0/16   | Prilikom stvaranja lokalne mreže dati rezerviranog mjesta adresa pristupnika. Adresa realni pristupnika uneseni nakon stvaranja pristupnika. Provjerite je li adresni prostor u potpunosti podudara odgovarajući udaljene VNET; u ovom slučaju na VNET stvorena u regiji Zapad SAD-a. |


###<a name="step-3-map-local-network-to-the-respective-vnets"></a>Korak 3: Karta "Lokalne" mreže da biste odgovarajući VNETs
Azure klasični, na portalu odaberite svaku vnet, kliknite "Konfiguriraj", provjerite "Povezivanje s lokalnom mrežom" pa odaberite lokalne mreže po sljedeće detalje:


| Virtualne mreže | Lokalne mreže |
| --------------- | ------------- |
| HK-vnet-Zapad-hr | HK-lnet-Map-to-East-US |
| HK-vnet-Istok-hr | HK-lnet-Map-to-West-US |


###<a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Korak 4: Stvaranje pristupnika VNET1 i VNET2
Na nadzornoj ploči i virtualne mreža kliknite Stvaranje PRISTUPNIKA koji će pokrenuti pristupnika VPN dodjeljivanje postupak. Nakon nekoliko minuta na nadzornoj ploči svaki virtualne mreže treba prikazati adresa stvarni pristupnika.

###<a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>Korak 5: Ažuriranje "Lokalnim" mrežama s adresama odgovarajući "pristupnika"###
Uređivanje i lokalne mreže da biste zamijenili rezervirano mjesto pristupnik IP adresa realni IP adresa samo dodijeljenu pristupnika. Pomoću sljedećih mapiranja:

<table>
<tr><th>Lokalne mreže    </th><th>Pristupnik virtualne mreže</th></tr>
<tr><td>HK-lnet-Map-to-East-US </td><td>Pristupnik hk-vnet-Zapad-sad</td></tr>
<tr><td>HK-lnet-Map-to-West-US </td><td>Pristupnik hk-vnet-Istok-SAD</td></tr>
</table>

###<a name="step-6-update-the-shared-key"></a>Korak 6: Ažuriranje zajednički ključ
Koristite sljedeću skriptu komponente Powershell da biste ažurirali tipku IPSec svaki VPN pristupnika [koristi tipku sake za oba pristupnika]: postavljanje AzureVNetGatewayKey - VNetName hk-vnet-Istok-hr - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey skup AzureVNetGatewayKey D9E76BKK - VNetName hk-vnet-Zapad-hr - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

###<a name="step-7-establish-the-vnet-to-vnet-connection"></a>Korak 7: Uspostaviti vezu VNET VNET
Azure klasični, na portalu pomoću izbornika "Nadzorne PLOČE" oba virtualne mreža uspostaviti vezu pristupnika pristupnika. Pomoću stavke "POVEZIVANJE" na donjoj alatnoj traci. Nakon nekoliko minuta na nadzornoj ploči treba prikazati pojedinosti veze grafički.

###<a name="step-8-create-the-virtual-machines-in-region-2"></a>8 korak: Stvaranje virtualnim strojevima u regiji #2
Stvaranje slike Ubuntu kao što je opisano u regiji #1 implementaciji slijedeći iste korake ili kopiji slikovne datoteke VHD na račun za Azure pohrane koja se nalazi u regiji #2 i stvorite sliku. Koristite ovu sliku i stvorite sljedeći popis virtualnim strojevima u novi servis u oblaku hk-c-svc-Istok-hr:


| Naziv računala | Podmreže | IP adresa | Postavljanje dostupnosti | Kontroler/za bicikle | Početni broj? |
| ------------ | ------ | ---------- | ---------------- | ------- | ----- |
| HK-c1-Istok-hr | podataka  | 10.2.2.4   | HK-c-aset-1      | kontroler = EASTUS za bicikle = rack1 | Da |
| HK-c2-Istok-hr | podataka  | 10.2.2.5   | HK-c-aset-1      | kontroler = EASTUS za bicikle = rack1 | ne  |
| HK-c3-Istok-hr | podataka  | 10.2.2.6   | HK-c-aset-1      | kontroler = EASTUS za bicikle = rack2 | Da |
| HK-c5-Istok-hr | podataka  | 10.2.2.8   | HK-c-aset-2      | kontroler = EASTUS za bicikle = rack3 | Da |
| HK-c6-Istok-hr | podataka  | 10.2.2.9   | HK-c-aset-2      | kontroler = EASTUS za bicikle = rack3 | ne  |
| HK-c7-Istok-hr | podataka  | 10.2.2.10  | HK-c-aset-2      | kontroler = EASTUS za bicikle = rack4 | Da |
| HK-c8-Istok-hr | podataka  | 10.2.2.11  | HK-c-aset-2      | kontroler = EASTUS za bicikle = rack4 | ne  |
| HK-w1-Istok-hr | web   | 10.2.1.4   | HK-w-aset-1      | N/D                    | N/D |
| HK-w2-Istok-hr | web   | 10.2.1.5   | HK-w-aset-1      | N/D                    | N/D |


Slijedite upute za isti kao područje #1, ali koristiti 10.2.xxx.xxx adresni prostor.

###<a name="step-9-configure-cassandra-on-each-vm"></a>9 korak: Konfiguriranje Cassandra na svakom VM
Prijavite se na VM i učinite sljedeće:

1. Uređivanje $CASS_HOME/conf/cassandra-rackdc.properties da biste odredili svojstva web-mjesto centra za i u okvir za bicikle podataka u obliku: kontroler = EASTUS za bicikle = rack1
2. Uređivanje cassandra.yaml da biste konfigurirali Početni broj čvorove: sjemenke: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

###<a name="step-10-start-cassandra"></a>10 korak: Pokretanje Cassandra
Prijavite se u svakom VM i najprije Cassandra u pozadini pokretanjem sljedeće naredbe: $CASS_HOME/smeće/cassandra

## <a name="test-the-multi-region-cluster"></a>Testiranje klaster regije
Po sada Cassandra implementiran 16 čvorove s 8 čvorove u svakom području Azure. Ove čvorove su u istom klaster by virtue of naziv uobičajenih klaster i konfiguracija čvor Početni broj. Da biste testirali klaster, koristite sljedeći postupak:

###<a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>Korak 1: Dobivanje IP raspoređivača opterećenja interne za obje regije pomoću komponente PowerShell
- Get-AzureInternalLoadbalancer - naziv servisa "hk-c-svc-Zapad-hr"
- Get-AzureInternalLoadbalancer - naziv servisa "hk-c-svc-Istok-hr"  

    Imajte na umu IP adrese (npr. Zapad - 10.1.2.101, Istok - 10.2.2.101) prikazuje.

###<a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>Korak 2: Izvršiti sljedeće u regiji Zapad nakon prijave na hk-w1-Zapad-hr
1.    Izvršavanje $CASS_HOME/smeće/cqlsh 10.1.2.101 9160
2.  Izvršavanje naredbe CQL sljedeće:

        CREATE KEYSPACE customers_ks
        WITH REPLICATION = { 'class' : 'NetworkToplogyStrategy', 'WESTUS' : 3, 'EASTUS' : 3};
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Trebali biste vidjeti prikaz kao ispod:

| customer_id | ime | Prezime |
| ----------- | --------- | -------- |
| 1           | Ivica      | N.      |
| 2           | N.      | N.      |


###<a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>Korak 3: Izvršiti sljedeće u regiji Istok nakon prijave na hk-w1-Istok-hr:
1.    Izvršavanje $CASS_HOME/smeće/cqlsh 10.2.2.101 9160
2.  Izvršavanje naredbe CQL sljedeće:

        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Isti prikaz trebali biste vidjeti kako je vide za regije zapada regiju:


| customer_id | ime | Prezime   |
|------------ | --------- | ---------- |
| 1           | Ivica      | N.        |
| 2           | N.      | N.        |


Izvođenje nekoliko više umeće i potražite u članku one se replicirati za regije zapada-hr dio klaster.

## <a name="test-cassandra-cluster-from-nodejs"></a>Test Cassandra klaster iz Node.js
Pomoću jednog od VMs Linux crated u "web" sloju prethodno, ne možemo će izvršiti jednostavnu skriptu Node.js za čitanje prethodno umetnute podataka

**Korak 1: Instalacija Node.js i Cassandra klijenta**

1. Instalirajte Node.js i npm
2. Instalacija čvor paket "cassandra klijenta" pomoću npm
3. Izvršavanje sljedeću skriptu naredbenom ljuske koja prikazuje niz json dohvaćene podatke:

        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };

        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed to create Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }

        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed to create column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }

        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);

           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }

        //update will also insert the record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }

        //read the two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }

        //exectue the code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)


## <a name="conclusion"></a>Zaključak
Microsoft Azure je fleksibilne platformu koja omogućuje pokretanje i Microsoft kao i Otvori izvor softver kao što je prikazano ovu vježbu. Iznimno dostupna klastere Cassandra može uvesti u centru za jedan podatkovni kroz šire čvorove klaster preko više domena kvara. Cassandra klastere može se uvesti i preko više geografski daleka Azure područja za Izrada probni sustave. Azure i Cassandra zajedno omogućuje izgradnju od iznimno prilagodljivi, Visoko dostupne i servise koje se mogu vratiti oblaka Izrada potrebno putem Interneta današnjeg Skaliraj services.  

##<a name="references"></a>Reference##
- [http://cassandra.Apache.org](http://cassandra.apache.org)
- [http://www.datastax.com](http://www.datastax.com)
- [http://www.nodejs.org](http://www.nodejs.org)
