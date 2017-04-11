<properties 
    pageTitle="Azure prostora za pohranu pomoću neprekinuti Integracija rješenje Jenkins | Microsoft Azure" 
    description="Pomoću ovog praktičnog vodiča pokazalo kako se koristi servis blobova platforme Azure kao spremište artefakte stvorio rješenje neprekinuti Integracija Jenkins." 
    services="storage" 
    documentationCenter="java" 
    authors="dineshmurthy" 
    manager="jahogg" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="dinesh"/>

# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Prostor za pohranu Azure pomoću Jenkins neprekinuti Integracija rješenja

## <a name="overview"></a>Pregled

Sljedeće informacije saznati kako koristiti spremište blobova platforme kao spremište Sastavi artefakte stvorio rješenje Jenkins neprekinuti Integracija (CI) ili kao izvor podataka koje je moguće preuzeti datoteke koja će se koristiti u procesu Sastavi. Slučajevi u kojoj želite pronaći to korisno je kada koristite kodiranje u okruženju agilno razvoj (pomoću Java ili druge jezike), izgradi koristite ovisno o integraciji neprekinuti i spremište potrebne za vaše artefakte Sastavi tako da nije moguće, na primjer, zajedničko korištenje s drugim članovima tvrtke ili ustanove, klijentima, i održavanje arhivu. Drugi scenarij je kada vaš posao Sastavi sam zahtijeva druge datoteke, na primjer, ovisnosti da biste preuzeli kao dio sastavljanje za unos.

U ovom ćete praktičnom vodiču namjeravate koristiti dodatak za Azure prostora za pohranu za Jenkins CI učinio dostupnima Microsoft.

## <a name="overview-of-jenkins"></a>Pregled Jenkins ##

Jenkins omogućuje neprekinuti integraciju projekta softver dopuštanjem inženjerima omogućuje jednostavno integrirati njihove promjene kod, a ste izgradi proizveli automatski i često, čime ćete povećanju produktivnosti za razvojne inženjere. Izgradi su mjesto, a Sastavi artefakte moguće prenijeti u različitim spremišta. U ovoj se temi vode kako koristiti blobova platforme Azure kao spremište artefakte Sastavi. Također će prikazati kako preuzeti ovisnosti iz spremišta blobova platforme Azure.

Dodatne informacije o Jenkins pronaći ćete na [Zadovoljavaju Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins).

## <a name="benefits-of-using-the-blob-service"></a>Prednosti korištenja servisa blobova platforme ##

Prednosti korištenja servisa blobova platforme za hostiranje vaše artefakte Sastavi agilno razvoj obuhvaćaju sljedeće:

- Visoke dostupnosti vaše Sastavi artefakte i/ili koje je moguće preuzeti ovisnosti.
- Performanse prilikom rješenje Jenkins CI prenosi na Sastavi artefakte.
- Performanse prilikom vaših korisnika i partnera preuzimanje vaše artefakte Sastavi.
- Kontrolu nad korisnika pravilnike za pristup, odabir između anonimni pristup, pristup utemeljen na isteka zajednički pristup potpis, a zatim privatno pristup, itd.

## <a name="prerequisites"></a>Preduvjeti ##

Potrebno je na sljedeći način korištenja servisa za blobova sustavu Jenkins CI rješenja:

- Neprekinuti integraciju Jenkins rješenja.

    Ako trenutno ne postoji Jenkins CI rješenje, možete pokrenuti rješenje Jenkins CI pomoću sljedeći postupak:

    1. Na računalu s omogućenim Java preuzmite jenkins.war iz <http://jenkins-ci.org>.
    2. U naredbenom retku koja je otvorena u mapu koja sadrži jenkins.war, pokrenite:

        `java -jar jenkins.war`

    3. U pregledniku otvorite `http://localhost:8080/`. Otvorit će se nadzorna ploča Jenkins, koji će se koristiti za instalaciju i konfiguriranje dodatak za Azure prostora za pohranu.

        Dok je uobičajena rješenja Jenkins CI se postaviti da se pokreće kao servis, koji se izvodi Jenkins war u naredbenom retku će biti dovoljni za ovog praktičnog vodiča.

- Račun Azure. Možete se prijaviti za Azure račun pri <http://www.azure.com>.

- Račun za Azure prostora za pohranu. Ako već nemate račun za pohranu, možete stvoriti jednu slijedeći korake na [Stvaranje računa za pohranu](storage-create-storage-account.md#create-a-storage-account).

- Poznavanje rješenjem Jenkins CI preporučuje se, ali nije obavezan, kao što je osnovni primjer sljedeći sadržaj će se koristiti da bi se prikazala korake potrebne prilikom korištenja servisa Blob kao spremište za Jenkins CI sastavljanje artefakte.

## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>Kako pomoću servisa Blob pomoću Jenkins CI ##

Da biste koristili servis Blob s Jenkins, morat ćete instalirati dodatak za pohranu Azure, konfiguriranje dodatak za korištenje računa za pohranu, a zatim stvorite nakon Sastavi akcija koja prenosi na Sastavi artefakte na račun servisa za pohranu. Ove korake opisane u sljedećim odjeljcima.

## <a name="how-to-install-the-azure-storage-plugin"></a>Kako instalirati dodatak za pohranu za Azure ##

1. Unutar nadzorne ploče Jenkins kliknite **Upravljanje Jenkins**.
2. Na stranici **Upravljanje Jenkins** kliknite **Upravljanje dodacima**.
3. Kliknite karticu **dostupno** .
4. U odjeljku **Uploaders artefakt** potvrdite **dodatak za spremište na platformi Microsoft Azure**.
5. Kliknite **Instaliraj bez ponovnog pokretanja** ili **Odmah preuzmite i instalirajte nakon ponovnog pokretanja**.
6. Ponovno pokrenite Jenkins.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Kako konfigurirati dodatak za Azure prostora za pohranu za korištenje računa za pohranu ##

1. Unutar nadzorne ploče Jenkins kliknite **Upravljanje Jenkins**.
2. Na stranici **Upravljanje Jenkins** kliknite **Konfiguriranje sustava**.
3. U odjeljku **Konfiguracija računa sustava Microsoft Azure prostora za pohranu** :
    1. Unesite naziv računa spremišta koje možete dobiti od [Azure Portal](https://portal.azure.com).
    2. Unesite svoj ključ računa za pohranu, također obtainable s [Portala za Azure](https://portal.azure.com).
    3. Ako koristite javno Azure oblak, koristite zadanu vrijednost za **URL krajnje točke servisa za Blob** . Ako koristite drugu Azure oblak, koristite krajnju točku kao što je navedeno [Azure Portal](https://portal.azure.com) za vaš račun za pohranu. 
    4. Kliknite **Provjeri valjanost vjerodajnice za pohranu** za provjeru valjanosti računa za pohranu. 
    5. [Neobavezni] Ako imate dodatni prostor za pohranu računa koji želite učiniti dostupnima vaše CI Jenkins, kliknite **Dodaj dodatne račune za pohranu**.
    6. Kliknite **Spremi** da biste spremili postavke.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Kako stvoriti nakon Sastavi akcija koja prenosi na Sastavi artefakte na račun servisa za pohranu ##

Upute za svrhe najprije ćemo morat ćete stvoriti zadatak koji ćete stvoriti nekoliko datoteka, a zatim dodajte u akciji nakon Sastavi da biste prenijeli datoteke na račun servisa za pohranu.

1. Unutar nadzorne ploče Jenkins kliknite **Nova stavka**.
2. Naziv zadatka **MyJob**, pritisnite **Sastavi projekta softver slobodno stila**pa kliknite **u redu**.
3. U odjeljku **Sastavljanje** konfiguracije posao kliknite **Dodaj sastavljanje korak** i odaberite **izvršavanje Windows obrade naredbe**.
4. **Naredba**, koristite sljedeće naredbe:

        md text
        cd text
        echo Hello Azure Storage from Jenkins > hello.txt
        date /t > date.txt
        time /t >> date.txt
 
5. U odjeljku **Akcije nakon Sastavi** konfiguracije zadatka kliknite **Dodaj nakon Sastavi akcija** , a zatim odaberite **Prenesi artefakte za spremište blobova platforme Azure**.
6. Za **naziv računa za pohranu**, odaberite račun za pohranu za korištenje.
7. Za **naziv spremnika**, navedite naziv spremnika. (Spremnik stvorit će se ako se još ne postoji kada se prenese artefakte Sastavi.) Možete koristiti i varijable okruženja, pa u ovom primjeru unesite **${JOB_NAME}** kao naziv spremnika.

    **Savjet**
    
    Ispod **naredba** odjeljak koji ste unijeli skripte za **izvršavanje Windows skupna naredba** je veza varijable okruženja prepoznaje Jenkins. Kliknite tu vezu da biste saznali okruženje varijable naziva i opisa. Imajte na umu da varijable okruženja, koje sadrže posebne znakove, kao što su varijablu okruženja **BUILD_URL** nije dopušteno kao naziv spremnika ili uobičajenih virtualnog puta.

8. U ovom primjeru kliknite **objavite novi spremnik prema zadanim postavkama** . (Ako želite koristiti spremnik za privatni, morat ćete stvoriti potpis zajednički pristup dopustiti pristup. Koji izlazi iz ove teme. Dodatne informacije o potpisima zajednički pristup pri [Korištenju zajednički pristup potpisa (SAS)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Neobavezni] Kliknite **Clean spremnik prije prijenosa** ako želite da se spremnik tabulatore sadržaja prije Sastavi artefakte prenose (ostavite je potvrđena ako želite očistiti sadržaj spremnika).
10. **Popis artefakte da biste prenijeli**u unesite * *tekst /*.txt**.
11. Za **uobičajene virtualnog puta prenesene artefakte**za potrebe ovog praktičnog vodiča unesite **${SASTAVLJANJE\_ID} / ${SASTAVLJANJE\_broj}**.
12. Kliknite **Spremi** da biste spremili postavke.
13. Na nadzornoj ploči Jenkins kliknite **Sastavljanje sada** da biste pokrenuli **MyJob**. Pregledajte izlaz konzole za status. Poruke o statusu za Azure pohranu obuhvaćene izlaz konzole prilikom pokretanja akciju nakon Sastavi da biste prenijeli artefakte Sastavi.
14. Nakon uspješan dovršetak zadatka artefakte Sastavi možete provjeriti i tako da otvorite javno blob.
    1. Prijavite se na [Portal za Azure](https://portal.azure.com).
    2. Kliknite **prostor za pohranu**.
    3. Kliknite naziv računa za pohranu koji ste koristili za Jenkins.
    4. Kliknite **spremnika**.
    5. Kliknite kontejner **myjob**, koji je mala verzija naziv zadatka koji ste dodijelili stvaranja Jenkins posao. Spremnik nazive i nazive blob se mala slova (i velika i mala slova) u Azure prostora za pohranu. Unutar popisa blob-ova za kontejner **myjob** trebali biste vidjeti **hello.txt** i **date.txt**. Kopiranje URL-a za bilo koju od tih stavki i otvorite je u pregledniku. Vidjet ćete tekstnu datoteku koju je učitana kao Sastavi artefakt.

Po poslu moguće je stvoriti samo jedan nakon Sastavi akcija koja prenosi artefakte blobova platforme Azure. Imajte na umu da jedan nakon Sastavi akcije da biste prenijeli artefakte blobova platforme Azure možete odrediti različite datoteke (uključujući zamjenskih znakova) i putovi do datoteka unutar **Popisa artefakte da biste prenijeli** pomoću točka-zarez kao razdjelnik. Ako, na primjer, ako vaš Sastavi Jenkins daje POSUDU i TXT datoteke u mapi **stvaranja** radnog prostora, a želite prenijeti i za spremište blobova platforme Azure, koristite sljedeće vrijednosti **Popisa artefakte da biste prenijeli** : **Sastavljanje /\*.jar; Sastavi /\*.txt**. Sintaksa dvaput zarez možete koristiti i da biste odredili put za korištenje unutar blob naziv. Ako, na primjer, ako želite staklenke da biste dobili prenijeti pomoću **binarne datoteke** u blob put i TXT datoteke da biste dobili prenijeti pomoću **obavijesti** na putu blob, koristite sljedeće vrijednosti **Popisa artefakte da biste prenijeli** : **Sastavljanje /\*. jar::binaries; Sastavi /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Kako stvoriti Sastavi korak koji se preuzima iz spremišta blobova platforme Azure ##

Na sljedeći način prikažite kako konfigurirati Sastavi korak da biste preuzeli stavke iz spremišta blobova platforme Azure. To će biti korisno ako želite uvrstiti stavki u vašem Sastavi, na primjer, staklenke koje se nalaze u Azure bloba prostora za pohranu.

1. U odjeljku **Sastavljanje** konfiguracije posao kliknite **Dodaj sastavljanje korak** , a zatim odaberite **preuzeti iz spremišta blobova platforme Azure**.
2. Za **naziv računa za pohranu**, odaberite račun za pohranu za korištenje.
3. Za **naziv spremnika**, navedite naziv kontejner s blob polja koje želite preuzeti. Možete koristiti varijable okruženja.
4. U odjeljku **naziv Blob**Navedite naziv blob. Možete koristiti varijable okruženja. Osim toga, možete koristiti zvjezdica, kao zamjenski znak nakon što odredite početni letter(s) blob naziva. Na primjer, **projekta\* ** određuju čiji nazivi počinju s **programom project**sve blob-ova.
5. [Neobavezni] **Preuzimanje put**, navedite put na računalu Jenkins mjesto na koje želite preuzeti datoteke iz blobova platforme Azure. Varijable okruženja može se koristiti. (Ako ne nude vrijednost za **Preuzimanje put**datoteke iz blobova platforme Azure će se preuzeti s radnim prostorom posla.)

Ako imate dodatnih stavki koje želite preuzeti iz spremišta blobova platforme Azure, možete stvoriti Sastavi dodatne korake.

Kada pokrenete na Sastavi, provjerite Sastavi povijest konzole za izlaz ili izgledati na vašoj lokaciji preuzimanje da biste vidjeli je li blob-ova očekujete uspješno preuzeti.  

## <a name="components-used-by-the-blob-service"></a>Komponente Blob servis koristi ##

Pregled komponente za servis Blob nudi.

- **Račun za pohranu**: pristupa za pohranu Azure obavlja putem računa za pohranu. Ovo je najvišu razinu naziva za pristup blob-ova. Račun može sadržavati neograničen broj spremnika, pod uvjetom da im ukupnu veličinu je u odjeljku 100TB.
- **Spremnik**: spremniku nudi grupiranja skupa blob-ova. Sve blob polja moraju biti u spremniku. Račun može sadržavati neograničen broj spremnika. Spremnik pohranjujete neograničen broj blob-ova.
- **Blob**: datoteke bilo koje vrste i veličine. Postoje dvije vrste blob polja koje se mogu pohraniti u spremište Azure: blokiranje i stranice blob-ova. Većina datoteka su bloka blob-ova. Jedan blok blob može biti najviše do 200GB. Pomoću ovog praktičnog vodiča koristi bloka blob-ova. Blob polja stranice na neku drugu vrstu blob može biti do 1TB veličinom i učiniti ga učinkovitijim su kada raspona bajtova u datoteci se često mijenjaju. Dodatne informacije o blob polja potražite u članku [objašnjenje bloka blob-ova, dodavanje blob-ova, i blob-Ova stranica](http://msdn.microsoft.com/library/azure/ee691964.aspx).
- **Oblik URL-a**: blob-Ova se adresirati pomoću sljedećih URL oblik:

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
    
    (Iznad oblikovanje primjenjuje se na javno Azure oblaka. Ako koristite drugu Azure oblaka, koristite krajnjoj točki unutar [Azure Portal](https://portal.azure.com) za određivanje na krajnjoj točki URL-a.)

    U obliku iznad, `storageaccount` predstavlja naziv računa spremišta `container_name` predstavlja naziv kontejneru, a `blob_name` predstavlja naziv blob, odnosno. U naziv spremnika može imati više putova odvojene kose crte, **/**. Naziv oglednog kontejnera ovog praktičnog vodiča je **MyJob**i **${SASTAVLJANJE\_ID} / ${SASTAVLJANJE\_broj}** korišten za uobičajene virtualnog puta, rezultira blob pojavljuju URL obrasca za sljedeće:

    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="next-steps"></a>Daljnji koraci

- [Susret Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)
- [Azure prostora za pohranu SDK Java](https://github.com/azure/azure-storage-java)
- [Azure prostora za pohranu klijenta SDK Reference](http://dl.windowsazure.com/storage/javadoc/)
- [Servise za pohranu Azure REST API-JA](https://msdn.microsoft.com/library/azure/dd179355.aspx )
- [Blog tima za Azure prostora za pohranu](http://blogs.msdn.com/b/windowsazurestorage/)

Dodatne informacije i potražite u [Centru za razvojne inženjere Java](https://azure.microsoft.com/develop/java/).