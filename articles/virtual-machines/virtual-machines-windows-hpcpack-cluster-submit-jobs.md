<properties
 pageTitle="Slanje zadataka paketa HPC skupine servisu Azure | Microsoft Azure"
 description="Saznajte kako postaviti je na lokalno računalo da biste poslali zadatke programa paketa HPC klaster servisu Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>Slanje HPC poslove s lokalnog računala programa paketa HPC klaster implementiran u Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Konfiguriranje lokalnog klijentsko računalo slanje poslove [Paket Microsoft HPC](https://technet.microsoft.com/library/cc514029) klaster u Azure. U ovom se članku objašnjava postavljanje lokalnom računalu s klijentskim alatima za slanje posla putem HTTP klaster u Azure. Na taj način više korisnika klaster možete poslati zadatke na oblaku klaster HPC paket, ali bez povezivanje izravno na glavni čvor VM ili pristup Azure pretplate.

![Slanje posla klaster servisu Azure][jobsubmit]

## <a name="prerequisites"></a>Preduvjeti

* **Glavni čvor paket HPC uveden u programa Azure VM** - preporučujemo korištenje automatiziranog alate kao što su [Azure brzi početak rada predloška](https://azure.microsoft.com/documentation/templates/) ili [skriptu Azure PowerShell](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) za implementaciju glavni čvora i klaster. Potreban je DNS naziva glavni čvor i vjerodajnice administratora klaster prođite kroz potrebne korake u ovom članku.

* **Klijentsko računalo** - morate Windows ili Windows Server klijentskog računala na kojemu možete pokrenuti uslužni programi za klijent HPC paket (potražite u članku [sistemski preduvjeti](https://technet.microsoft.com/library/dn535781.aspx)). Ako samo želite koristiti HPC paket web-portal ili REST API-JA za slanje zadatke, možete koristiti bilo koju klijentsko računalo po izboru.

* **Medij za instalaciju paketa HPC** - da biste instalirali paket HPC uslužnih klijent besplatne instalacijski paket za najnoviju verziju paketa HPC (HPC paket 2012 R2) dostupna iz [Microsoftova centra za preuzimanje](http://go.microsoft.com/fwlink/?LinkId=328024). Provjerite je li preuzimate istu verziju HPC paket koji je instaliran na glavni čvor VM.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Korak 1: Instaliranje i konfiguriranje web-komponente na glavni čvor

Da biste omogućili OSTALE sučelje za slanje poslova klaster putem HTTP, provjerite je li web-komponente paketa HPC konfigurirane na glavni čvor HPC paket. Ako već nisu instalirani, najprije instalirajte web-komponente tako da pokrenete HpcWebComponents.msi instalacijsku datoteku. Nakon toga konfigurirati komponente tako da pokrenete skriptu HPC PowerShell **Skup HPCWebComponents.ps1**.

Detaljne postupke potražite u članku [Instalacija web-komponente Microsoft HPC paket](http://technet.microsoft.com/library/hh314627.aspx).

>[AZURE.TIP] Nekih predložaka Azure brzi početak rada za paket HPC instalirajte i konfiguriranje web-komponente automatski. Ako koristite [HPC paket IaaS implementacijsku skriptu](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) da biste stvorili klaster, po želji možete instalirate i konfigurirate web-komponente kao dio uvođenje.

**Da biste instalirali web-komponente**

1. Povezivanje s čvor glavni VM pomoću vjerodajnica klaster administratora.

2. Iz mape instalacijski paket HPC izvoditi HpcWebComponents.msi na glavni čvor.

3. Slijedite korake u čarobnjaku za instalaciju web components

**Da biste konfigurirali web-komponente**

1. Na glavni čvor pokrenuti HPC PowerShell kao administrator.

2. Da biste promijenili direktorija mjesto skripte za konfiguraciju, upišite sljedeću naredbu:

    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. Konfigurirajte OSTALE sučelja i započnite HPC web-servisa, upišite sljedeću naredbu:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```

4. Kada se to od vas zatraži da biste odabrali certifikat, odaberite certifikat koji odgovara nazivu glavni čvora javno DNS-a. Na primjer, ako pokrenete čvor glavni VM pomoću modela klasični implementaciju, naziv certifikata izgleda CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Ako koristite model implementacije Voditelj resursa, naziv certifikata izgleda CN =&lt;*HeadNodeDnsName*&gt;. &lt; *regija*&gt;. cloudapp.azure.com.

    >[AZURE.NOTE] Odaberite certifikat kasnije kada pošaljete poslove čvor glavni s lokalnog računala. Ne potvrdite ili konfiguriranje certifikata koji odgovara nazivu računala glavni čvora u domene servisa Active Directory (na primjer, CN =*MyHPCHeadNode.HpcAzure.local*).

5. Da biste konfigurirali web-portal za predavanje posla, upišite sljedeću naredbu:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Po završetku skriptu zaustavite i ponovno pokrenite servis Raspored za HPC upisivanjem sljedeće naredbe:

    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Korak 2: Instalacija uslužni programi za klijent HPC paket na na lokalnom računalu

Ako želite instalirati uslužni programi za klijent HPC paketa na računalu, iz [Microsoftova centra za preuzimanje](http://go.microsoft.com/fwlink/?LinkId=328024)preuzmite datoteke za postavljanje HPC paket (Potpuna instalacija). Kada započnete s instalacijom, odaberite mogućnost instalacijski program za **uslužni programi HPC paket klijenta**.

Da biste koristili HPC paket klijentskih alata za slanje poslova na glavni čvor VM, morate izvesti certifikat iz glavni čvor i instalirajte ga na klijentskom računalu. Certifikat mora biti u. Oblik CER.

**Da biste izvezli certifikat glavni čvorovi**

1. Na glavni čvor dodati dodatak za potvrde Microsoft Management Console za račun lokalnog računala. Korake da biste dodali dodatak, potražite u članku [Dodavanje certifikati dodatak za blog](https://technet.microsoft.com/library/cc754431.aspx).

2. U stablu konzole proširite **Certifikati – lokalnom računalu** > **osobnih**, a zatim **Certifikati**.

3. Pronađite certifikat koji ste konfigurirali za web-komponente HPC paketa u [Korak 1: instalacija i konfiguriranje web-komponente na glavni čvor](#step-1:-install-and-configure-the-web-components-on-the-head-node) (na primjer, CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net).

4. Desnom tipkom miša kliknite certifikat, a zatim kliknite **Sve zadatke** > **Izvoz**.

5. U čarobnjaka za izvoz certifikata, kliknite **Dalje**, a da **ne, izvezi privatni ključ** provjerite je li odabrana.

6. Slijedite preostale korake čarobnjaka za izvoz certifikata DER kodirana binarni X.509 (. Oblik CER).


**Da biste uvezli certifikat na klijentskom računalu**


1. Kopirajte certifikat koji ste izvezli glavni čvor u mapu na klijentskom računalu.

2. Na klijentskom računalu pokrenite certmgr.msc.

3. U upravitelju certifikat proširite **Certifikati – trenutnog korisnika** > **Izdavanje korijenskih** **certifikata**desnom tipkom miša, a zatim kliknite **Sve zadatke** > **Uvoz**.

4. U čarobnjaku uvoz certifikata kliknite **Dalje** , a zatim slijedite korake da biste uvezli certifikat koji ste izvezli čvor glavni izdavanje korijenskih spremište.



>[AZURE.TIP] Možda će se prikazati u sigurnosnih upozorenja, jer ustanova za izdavanje certifikata na glavni čvor ne prepoznaje klijentskom računalu. Za testiranje, možete zanemariti upozorenje i dovršiti uvoz certifikata.

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Korak 3: Cilja Testiraj zadatke na klaster

Da biste provjerili konfiguraciju, pokušajte izvođenje zadataka na klasteru u Azure s lokalnog računala. Ako, na primjer, možete koristiti HPC paket GUI Alati ili naredbenog retka naredbe za slanje poslova klaster. Portal za utemeljen na webu možete koristiti i slanje zadataka.


**Da biste pokrenuli naredbi za predavanje posla na klijentskom računalu**


1. Na klijentskom računalu instaliranim uslužni programi za klijent HPC paket, otvorite naredbeni redak.

2. Upišite naredbu uzorka. Ako, na primjer, popis svih zadataka na klaster, upišite naredbu sličnu nešto od sljedećeg, ovisno o punog DNS naziva glavni čvor:

    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
    
    ili
    
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```

    >[AZURE.TIP] Koristite puni naziv DNS glavni čvor ne IP adresu, u URL-u raspored. Ako ste odredili IP adresu, pogreška izgledat će otprilike "certifikat poslužitelja treba da se valjani niz pouzdanost ili smjestiti u spremištu pouzdanom korijenu."

3. Kada se to od vas zatraži, unesite korisničko ime (u obliku &lt;DomainName&gt;\\&lt;korisničko ime&gt;) i lozinku administratora klaster HPC ili drugom korisniku klaster koje ste konfigurirali. Možete odabrati da biste pohranili lokalno za dodatne operacije posao.

    Pojavit će se popis zadataka.


**Da biste koristili HPC posao Manager na klijentskom računalu**

1. Ako već niste spremanje vjerodajnica domene za klaster korisnika prilikom slanja posao, možete je dodati vjerodajnice u upravitelj vjerodajnica.

    na. Na upravljačkoj ploči na klijentskom računalu pokrenite upravitelj vjerodajnica.

    b. Kliknite **Vjerodajnice sustava Windows** > **Dodaj generički vjerodajnica**.

    c. Odredite internetsku adresu (na primjer, https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler ili https://&lt;HeadNodeDnsName&gt;.&lt; Regija&gt;.cloudapp.azure.com/HpcScheduler), i korisničko ime (&lt;DomainName&gt;\\&lt;korisničko ime&gt;) i lozinku administratora klaster ili drugom korisniku klaster koje ste konfigurirali.

2. Na klijentskom računalu pokrenite upravitelj HPC posao.

3. U dijaloškom okviru **Odaberite čvor glave** upišite URL-a na glavni čvor u Azure (na primjer, https://&lt;HeadNodeDnsName&gt;. cloudapp.net ili https://&lt;HeadNodeDnsName&gt;.&lt; Regija&gt;. cloudapp.azure.com).

    Upravitelj posao HPC otvara i prikazuje popis zadataka na glavni čvor.

**Korištenje web-portal na glavni čvor**

1. Pokretanje web-pregledniku na klijentskom računalu pa unesite jednu od sljedeće adrese, ovisno o punog DNS naziva glavni čvor:

    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
    
    ili
    
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. U dijaloškom okviru sigurnost, koji će se pojaviti upišite vjerodajnice domene HPC klaster administratora. (Možete dodati druge korisnike klaster različite uloge. Potražite u članku [Upravljanje korisnicima klaster](https://technet.microsoft.com/library/ff919335.aspx))

    Otvorit će se web-portal u prikazu popisa posla.

3. Da biste poslali uzorka zadatak koji vraća niz "Pozdrav svijeta" iz klaster, u lijevom navigacijskom oknu kliknite **novi zadatak** .

4. Na stranici **Novi zadatak** , u odjeljku **sa stranice slanje**, kliknite **HelloWorld**. Pojavljuje se stranica za predavanje posla.

5. Kliknite **Pošalji**. Ako se to od vas zatraži, unesite vjerodajnice domene HPC klaster administratora. Šalje se zadatak, a ID zadatka se pojavljuje na stranici **Moji zadaci** .

6. Da biste vidjeli rezultate posla koji ste poslali, kliknite ID zadatka, a zatim **Pregled zadataka** da biste pogledali rezultatu naredbe (u odjeljku **Izlaz**).

## <a name="next-steps"></a>Daljnji koraci

* Možete poslati i zadacima Azure klaster s [HPC paket REST API -JA](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).

* Ako želite poslati klaster poslovi iz klijenta Linux potražite u članku uzorka Python [HPC paket 2012 R2 SDK i uzorak koda](https://www.microsoft.com/download/details.aspx?id=41633).


<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
