<properties 
   pageTitle="Bilješke o izdanju Azure SDK za .NET 2,6" 
   description="Bilješke o izdanju Azure SDK za .NET 2,6" 
   services="app-service/web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

 
# <a name="azure-sdk-for-net-26-release-notes"></a>Bilješke o izdanju Azure SDK za .NET 2,6

Ovaj dokument sadrži napomene u za Azure SDK za .NET 2,6 izdanje. 

S Azure SDK 2,6 razviti ciljanja .NET 4.5.2 ili .NET 4.6 pod uvjetom da ručno instalirati cilj .NET Framework na uloge servisa oblaka oblaka servisne aplikacije (PaaS). Potražite u članku [instalacija .NET na servis ulozi oblaka](http://go.microsoft.com/fwlink/?LinkID=309796).


##<a name="service-bus-updates"></a>Ažuriranja servisa Bus

- Koncentratora događaja: 

    - Sada omogućuje kontrolu pristupa ciljano prilikom slanja događaje tako da će se dodatne publisher krajnja točka za događaj koncentratora.
    - Dodatni stabilnosti i poboljšanja dodali značajku koncentratora događaja.
    - Dodavanje podršku za protokol Amqp putem WebSocket za razmjenu poruka i koncentratora za događaj.

##<a name="hdinsight-tools-for-visual-studio-updates"></a>Alati za HDInsight ima li ažuriranja za Visual Studio

- **Poboljšanje IntelliSense**: prijedlog udaljene metapodataka

    HDInsight alate za Visual Studio sada podržava dohvaćanje udaljene metapodataka prilikom uređivanja grozd skriptu. Ako, na primjer, možete upisati * *Odaberite* iz** i prikazat će se svi nazivi tablica. Osim toga, nazive stupaca prikazat će se nakon što odredite tablice.

- **Podrška za emulator HDInsight**

    Sada HDInsight alate za Visual Studio podržava povezivanje sa servisa HDInsight emulator, tako da nije razvijate skripte grozd lokalno bez Uvod u bilo kojem trošak, zatim izvršavanje te skripte na temelju vašeg klastere HDInsight. 

    Dodatne informacije potražite [u ovom ručno](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

- **Alati za HDInsight za Visual Studio podrške za klastere generički Hadoop** (Pretpregled)

    Alati za HDInsight za Visual Studio sada podržava generički klastere Hadoop, da biste mogli koristiti HDInsight alate za Visual Studio učiniti sljedeće:

    - povezivanje s svoj klaster 
    - pisanje grozd upit s poboljšanu podršku IntelliSense /-Samodovršetak 
    - Pogledajte sve zadatke u svoj klaster s intuitivno korisničko Sučelje. 

    Dodatne informacije potražite [u ovom ručno](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

##<a name="in-role-cache-updates"></a>Ažuriranja u ulozi predmemorije

- **Predmemorije u ulozi** ažurirana je da biste koristili **Microsoft Azure prostora za pohranu SDK** verziju 4,3. Do pak **Predmemorije u ulozi** koristio SDK za pohranu Azure verzija 1.7.

    Klijentima koristite Azure SDK 2,5 ili ispod potrebno ažuriranje 2,6 SDK Azure i njegovo premještanje novu verziju SDK za pohranu Azure. 

    Trenutno Azure prostora za pohranu verziju 2011 08 18 zakazanog uklonit kolovoz 1, 2016. Bilo koji migracije u ulozi predmemorije od 2,5 SDK Azure ili ispod 2,6 moraju biti cjelovit po ovaj put. Dodatne informacije o umirovljenje Azure prostora za pohranu verziju 2011 08 18 potražite u članku [Microsoft Azure prostora za pohranu servisa verziju uklanjanje Update: proširenje 2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

>[AZURE.IMPORTANT]Ne možemo ste objavljivanje studenom 30, 2016, umirovljenje za Azure Upravljani servis predmemorije i predmemorije u ulozi Azure. Preporučujemo da migrirati Azure Redis predmemoriju u Priprema za ovaj umirovljenje. Dodatne informacije o datume i upute za migraciju potražite u članku [koji predmemorije Azure nuditi najviše odgovara me?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

##<a name="azure-app-service-tools"></a>Alati za aplikacije servisa za Azure

Ažuriran je sljedeće stavke u izdanju 2,6 SDK Azure.

- Objavljivanje za Azure poboljšanom da biste uključili Azure API aplikacije kao odredište implementacije.
- API aplikacije dodjeljivanja funkcije da biste korisnicima omogućili stvaranje i dodjeljivanje funkcionalnošću API aplikacije.
- Poslužitelj Explorer izmijeniti da odražava novi čvor aplikacije servisa s weba, Mobile i API aplikacijama grupirane grupu resursa.
- Dodajte Azure API klijent aplikacije gesta dodane na većini C# projekata koji će se omogućiti automatsko generiranje Swagger omogućeno izvodi u Azure pretplata aplikacija API-JA.
- Tooling API aplikacije i aplikacije servisa za čvorove u programu Explorer Server dostupne su u Visual Studio 2013 samo. 

##<a name="azure-resource-manager-tools-updates"></a>Azure resursima Alati ažuriranja

Alati za Upravitelj Azure resursa ažurirane za uključivanje predložaka virtualnim strojevima, mreže i prostora za pohranu. Da biste dodali novi prikaz strukture za predloške i omogućuje uređivanje predložaka korištenje JSON isječci ažurirao JSON sučelje za uređivanje. Predlošci za implementiran putem Visual Studio pomoću koji ste dobili uz projekt, tako da se promjene koje ste napravili skripta će koristiti Visual Studio skriptu PowerShell.

##<a name="diagnostics-improvements-for-cloud-services"></a>Poboljšanja Dijagnostika za servise u Oblaku

Azure SDK 2,6 premješta natrag podršku za prikupljanje zapisnika dijagnostike u emulator Azure računalnim i prijenos razvoj prostora za pohranu. Bilo koji Dijagnostika zapisnike (uključujući aplikacije praćenja zapisnika događaja praćenje za zapisnici sustava Windows (ETW), mjerača performansi, Infrastruktura zapisnika događaja zapisnika i windows) generira kada se aplikacija izvodi u na emulator može prenijeti u razvoju spremište da biste potvrdili da vaše Dijagnostika zapisivanja radi na lokalnom računalu. 

Račun za pohranu Dijagnostika sada moguće navesti u datoteci konfiguracije (.cscfg) servisa olakšano koristiti različite Dijagnostika za pohranu račune za različite okruženja. Postoje neke najvažnije razlike između kako niz za povezivanje u kojoj je radio Azure SDK 2.4 i Azure SDK 2,6. Dodatne informacije o korištenju veze za pohranu Dijagnostika niz i kako ga utječe projekte potražite u članku [Konfiguriranje Dijagnostika za servise u Oblaku Azure](http://go.microsoft.com/fwlink/?LinkID=532784).

##<a name="breaking-changes"></a>Najnovije promjene

###<a name="azure-resource-manager-tools"></a>Alati za Azure Voditelj resursa 

- Vrsta projekta za **Oblak implementacije projekata** u SDK 2,5 Azure preimenovana je u **Grupu resursa Azure**.
- **Oblak implementacije projekata** vrsta projekata stvorene u SDK 2,5 Azure može koristiti u 2,6, ali uvođenje predloška iz Visual Studio neće uspjeti. Međutim, implementacija s skriptu PowerShell će i dalje funkcionirati prethodno onako kao.  Informacije o korištenju **Oblaka implementacije projekata** u 2,6 pročitajte ovaj [objaviti](http://go.microsoft.com/fwlink/?LinkID=534086).
 
##<a name="known-issues"></a>Poznati problemi

- Prikupljanje zapisnika dijagnostike u na emulator potreban je 64-bitni operacijski sustav. Prikuplja dijagnostički zapisnici će se kada se pokrene u 32-bitnom operacijskom sustavu. To ne utječe na druge funkcije emulator. 

- Azure SDK 2,6 objavio na 29/4/2015 ima dva problema: 

    - Univerzalni aplikacija nije moguće učitati u Visual Studio 2015 kada Azure SDK 2,6 je bio instaliran na računalu.
    - Ispravljanje pogrešaka u Oblaku projekta zadovoljavaju u Visual Studio 2013 i Visual Studio 2015 gdje Visual Studio postaje dostupan i zatvara istovremeno prikazujući dijaloški okvir s porukom "Konfiguriranje Dijagnostika za emulator".
    
    Ažuriranje Azure SDK 2,6 objavljen 18/5/2015. Je ažurirana verzija 2.6.30508.1601; sadrži ispravaka dvije probleme koji su prethodno opisan. Možete prepoznati sastavljanje SDK putem upravljačke ploče -> programi i značajke -> Alati sustava Microsoft Azure za Microsoft Visual Studio 2013 – v 2,6. Stupac verziju prikazat će broj izdanja.

    Ako i dalje su okrenuta prema gore problema, instalirajte najnoviju verziju sustava 2,6 SDK Azure za [Dodavanje veze za VANJSKIH 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [Dodavanje veze za VANJSKIH 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) ili [Dodavanje veze za VANJSKIH 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).
 
##<a name="see-also"></a>Vidi također

[Podrška i njegovo povlačenje iz upotrebe informacije za Azure SDK za .NET i API-](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
