<properties 
    pageTitle="Uobičajeni zadaci (klasični) oblaka usluga upravljanja | Microsoft Azure" 
    description="Informirajte se o upravljanju servise u oblaku na portalu za Azure klasični." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/10/2016"
    ms.author="adegeo"/>





# <a name="how-to-manage-cloud-services"></a>Kako upravljati servise u Oblaku

> [AZURE.SELECTOR]
- [Portal za Azure](cloud-services-how-to-manage-portal.md)
- [Azure klasični portal](cloud-services-how-to-manage.md)

U području **Servise u Oblaku** Azure klasični portal možete ažurirati uloge servisa ili implementaciju, Promicanje postupnu implementacije radnog, veza resursa na servis u oblaku, tako da možete vidjeti ovisnosti resursa i zajedno skaliranje resurse i brisanje servis u oblaku i implementacija.


## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Kako: ažuriranje uloge servisa oblaka ili uvođenje

Ako trebate ažurirati kod aplikacije servisa za oblak, koristite **Ažuriranje** na stranici nadzorne ploče, **Servise u Oblaku** , ili **instance** stranice. Možete ažurirati ulogu jedne ili svih uloga. Morat ćete prijenos novog paketa i datoteke za konfiguraciju servisa.

1. [Azure klasični portal](https://manage.windowsazure.com/)na stranici nadzorne ploče, **Servise u Oblaku** , ili **instance** stranice, kliknite **Ažuriraj**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. **Natpis za implementaciju**, unesite naziv za prepoznavanje implementacije (na primjer, mycloudservice4). Natpis za implementaciju u odjeljku **brzi početak** nalaze se na nadzornoj ploči.

3. U **paket**koristiti **Pregledaj** da biste prenijeli datoteku paketa servisa (.cspkg).

4. U **konfiguraciji**koristiti **Pregledaj** da biste prenijeli datoteku konfiguracije servisa (.cscfg).

5. U **ulogu**, odaberite **sve** ako želite li nadograditi svih uloga u servis u oblaku. Da biste izvršili jednu ulogu ažuriranja, odaberite uloge koje želite ažurirati. Čak i ako ste odabrali određene ulogu želite ažurirati, ažuriranja u konfiguracijskoj datoteci servisa primjenjuju se na sve uloge.

6. Ako se ažuriranje mijenja se broj uloga ili veličine sve uloge, uključite potvrdni okvir **Dopusti ažurirati ako promjene veličine uloga ili broj uloge** da biste omogućili ažuriranje da biste nastavili. 

    Imajte na umu da ako promijenite veličinu uloga (to jest, veličina virtualnog računala koje hostira uloga instance) ili broj uloge, svaku instancu uloga (virtualnog računala) mora biti ponovno preslikani i izgubit će se sve lokalne podatke.

7. Ako bilo koji servis uloge imate samo jednu instancu uloga, odaberite **Ažuriranje čak i ako jedan ili više uloga sadrže instancu potvrdni okvir** da biste omogućili nadogradnju da biste nastavili. 

    Azure možete samo jamči dostupnost usluge postotak 99.95 tijekom ažuriranja servisa oblaka ako svaka uloga ima najmanje dva uloga instance (virtualnim strojevima). Koji omogućuje da obradi zahtjeve klijenta dok drugi ažurira jednu virtualnog računala.

8. Kliknite **u redu** (kvačica) da biste započeli ažuriranje servis.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Kako: zamjena implementacijama Promicanje postupnu implementacije radnog

Koristite **Zamijeni** da biste povećali razinu pripremna implementacije neki servis u oblaku da biste radni. Kada se odlučite za implementaciju novo izdanje servis u oblaku, možete faze i testirati novo izdanje u svom okruženju pripremna oblaka servisa dok klijentima trenutnom izdanju u koristite radni. Kada budete spremni Promicanje novo izdanje da biste radni **Zamjena** možete koristiti da biste se prebacili URL-ovi koji su dvije implementacijama adresirana. 

Možete zamijenite implementacije na stranici za **Servise u Oblaku** ili na nadzornu ploču.

1. [Azure klasični portal](https://manage.windowsazure.com/)kliknite **Servise u Oblaku**.

2. Na popisu servise u oblaku kliknite servisa u oblaku da biste ga odabrali.

3. Kliknite **Zamijeni**.

    Otvorit će se sljedeće odzivnik za potvrdu.

    ![Zamjena Services oblaka](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Kada potvrdite informacije o implementaciji, kliknite **da** da biste zamijenili u implementacijama.

    Zamjena implementacije odvija brzo jer je jedini stvar koja se mijenja virtualne IP adrese (VIPs) za na implementacije.

    Računalnim troškova možete izbrisati implementacije u okruženje za pripremna kada ste sigurni da novi radni implementacije izvršava prema očekivanjima.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Kako: veza resursa na neki servis u oblaku

Da biste prikazali na cloud ovisnosti usluge iz drugih resursa, možete povezati instancu baze podataka SQL Azure ili spremanje računa servisa u oblaku. Možete povezati i prekidanje veze resursa na stranici **Povezani resursi** i praćenje njihova korištenja na nadzornoj ploči za servis oblaka. Ako je račun povezane prostora za pohranu nadzor uključen, možete nadzirati Ukupno zahtjeva na nadzornoj ploči za servis oblaka.

Pomoću **veze** da biste se povezali s novi ili postojeći baze podataka SQL instanci prostora za pohranu račun ili servis u oblaku. Zatim mogu mijenjati veličinu baze podataka te uloge servisa oblaka koja koristi ga na stranici **Promjena veličine** . (S računom za pohranu mijenja veličinu automatski kao korištenje povećava.) Dodatne informacije potražite u članku [upute za promjenu veličine u Oblaku i povezane resurse](cloud-services-how-to-scale.md). 

Također možete pratiti, upravljanje i promijenite veličinu baze podataka u **bazama podataka** čvor portala za Azure klasični. 

"Povezivanje" resursa u ovom smislu ne povezati aplikacije resurs. Ako stvorite novu bazu podataka putem **veze**, morat ćete dodati nizu za povezivanje kodu aplikacije i nadogradnje servisa u oblaku. Ćete morati dodati nizove veze ako aplikacije koristi resursa u računu povezane prostora za pohranu.

U nastavku opisuje kako povezati nove instance SQL baze podataka, implementiran na nove baze podataka SQL server, na servis u oblaku.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Da biste se povezali s instancom SQL baze podataka na servis u oblaku

1. [Azure klasični portal](http://manage.windowsazure.com/)kliknite **Servise u Oblaku**. Kliknite naziv servisa u oblaku da biste otvorili na nadzornoj ploči.

2. Kliknite **povezani resursi**.

    Otvorit će se stranica **Povezani resursi** .

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Kliknite **vezu resursa** ili **vezu**.

    Pokreće se čarobnjak za **Povezivanje resursa** .

    ![Page1 veze](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Kliknite **Stvori novi resurs** ili **vezu postojeći resurs**.

5. Odaberite vrstu resursa za povezivanje. [Azure klasični portal](http://manage.windowsazure.com/)kliknite **SQL baze podataka**. (Klasični portal za pretpregled Azure ne podržava povezivanje s računom za pohranu na servis u oblaku.)

6. Da biste dovršili konfiguraciju baze podataka, slijedite upute u članku pomoć za područje **Baze podataka SQL** Azure klasični portal.

    Možete pratiti napredak operacije povezivanja u područja za poruku.

    ![Tijeku veze](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkProgress.png)

    Po dovršetku povezivanje možete nadzirati status povezane resursa na nadzornoj ploči za servis oblaka. Informacije o skaliranje povezanu bazu podataka za SQL potražite u članku [za promjenu veličine u Oblaku i povezane resurse](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Da biste prekinuli vezu povezanih resursa

1. [Azure klasični portal](http://manage.windowsazure.com/)kliknite **Servise u Oblaku**. Kliknite naziv servisa u oblaku da biste otvorili na nadzornoj ploči.

2. Kliknite **Povezani resursi**, a zatim odaberite resurs.

3. Kliknite **Prekini vezu**. Zatim **da** prikaže odzivnik za potvrdu.

    Prekida veze baze podataka SQL ne utječe na bazu podataka ili aplikacije veza s bazom podataka. I dalje možete upravljati baze podataka u području **Baze podataka SQL** Azure klasični portal.



## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Kako: brisanje implementacije i servise u oblaku

Da biste izbrisali neki servis u oblaku, morate izbrisati svaki postojeće implementacije.

Da biste spremili računalnim troškove, možete izbrisati pripremna uvođenja kada potvrdite da implementaciju sustava radnog funkcioniraju kako želite. Trošak naplate računalnim uloga instance su čak i ako nije pokrenut servis u oblaku.

Pomoću sljedećeg postupka možete izbrisati implementacije ili servis u oblaku. 

1. [Azure klasični portal](http://manage.windowsazure.com/)kliknite **Servise u Oblaku**.

2. Odaberite servis u oblaku, a zatim kliknite **Izbriši**. (Da biste odabrali neki servis u oblaku bez otvaranja nadzornu ploču, kliknite bilo gdje osim naziv u stavku servisa oblaka.)

    Ako imate implementacije u održavanje ili proizvodnju, vidjet ćete izbornik mogućnosti slično onome sljedeće pri dnu prozora. Da biste izbrisali servisa u oblaku, morate izbrisati sve postojeće implementacije.

    ![Brisanje izbornika](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)


3. Da biste izbrisali implementacije, kliknite **implementacije proizvodnje** ili **Brisanje pripremna implementacije**. Zatim, kada se zatraži potvrda, kliknite **da**. 

4. Ako planirate da biste izbrisali servisa u oblaku, ponovite korak 3, ako je potrebno, da biste izbrisali vaše implementacije.

5. Da biste izbrisali servisa u oblaku, kliknite **Izbriši u oblaku**. Zatim, kada se zatraži potvrda, kliknite **da**.

> [AZURE.NOTE]
> Ako opširno nadzor je konfiguriran za servis u oblaku, Azure ne briše nadzora podataka s računa za pohranu kada izbrišete servisa u oblaku. Morat ćete ručno izbrisati podatke. Informacije o tome gdje da biste pronašli metriku tablica potražite u članku "Kako: opširno podataka izvan Azure klasični portal za nadzor pristupa" u [uputama za servise u Oblaku Monitor](cloud-services-how-to-monitor.md).

## <a name="next-steps"></a>Daljnji koraci

 * [Općenito konfiguracije servis u oblaku](cloud-services-how-to-configure.md).
* Saznajte kako [implementirati servis u oblaku](cloud-services-how-to-create-deploy.md).
* Konfiguriranje [Prilagođeni naziv domene](cloud-services-custom-domain-name.md).
* Konfiguriranje [ssl certifikata](cloud-services-configure-ssl-certificate.md).
