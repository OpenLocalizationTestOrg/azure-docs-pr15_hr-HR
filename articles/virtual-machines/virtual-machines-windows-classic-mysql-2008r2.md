<properties
    pageTitle="Stvaranje VM izvodi MySQL | Microsoft Azure"
    description="Stvaranje Azure virtualnog računala izvodi Windows Server 2012 R2 i baze podataka MySQL pomoću klasične implementacije modela."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="cynthn"/>


# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2012-r2"></a>Kliknite pločicu MySQL virtualnog računala stvorene pomoću klasične implementacije modelu koji se izvodi Windows Server 2012 R2

[MySQL](http://www.mysql.com) je popularne Otvori izvor, SQL baze podataka. Pomoću ovog praktičnog vodiča pokazuje kako instalirati i pokrenuti zajednice verziju MySQL 5.6.23 kao poslužitelj MySQL na virtualnog računala koja se izvodi Windows Server 2012 R2. Upute za instalaciju MySQL na Linux, pogledajte: [kako se instalira MySQL na Azure](virtual-machines-linux-mysql-install.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="create-a-virtual-machine-running-windows-server-2012-r2"></a>Stvaranje virtualnog računala koja se izvodi Windows Server 2012 R2

Ako još nemate VM izvodi Windows Server 2012 R2, možete koristiti ovaj [Praktični vodič](virtual-machines-windows-classic-tutorial.md) da biste stvorili virtualnog računala. 

## <a name="attach-a-data-disk"></a>Prilaganje podatkovni disk

Nakon stvaranja virtualnog računala po želji možete priložiti na disk dodatne podatke. To se preporučuje za proizvodnju radnih opterećenja i da bi se izbjeglo ponestati prostora na disku OS (C:), što obuhvaća operacijski sustav.

Saznajte [kako priložiti podatkovni disk na virtualnog računala za Windows](virtual-machines-windows-classic-attach-disk.md) i slijedite upute za pridruživanje prazan disk. Postavite postavku predmemorije glavno računalo **ništa** ili **samo za čitanje**.

## <a name="log-on-to-the-virtual-machine"></a>Prijavite se na virtualnog računala

Nakon toga prikazat će vam [prijavite se na virtualnog računala](virtual-machines-windows-classic-connect-logon.md) da biste mogli instalirati MySQL.

##<a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Instalirajte i pokrenite poslužitelj zajednice MySQL na virtualnog računala

Slijedite ove korake da biste instalirali, konfiguriranje i pokrenite verzije zajednice MySQL poslužitelja:

> [AZURE.NOTE] Korake u nastavku su u 5.6.23.0 zajednice verziju MySQL i Windows Server 2012 R2. Rad s može se razlikovati u različitim verzijama sustava MySQL ili Windows Server.

1.  Nakon što ste povezali s virtualnog računala putem udaljene radne površine, kliknite **Internet Explorer** na početnom zaslonu.
2.  Odaberite gumb **Alati** u gornjem desnom kutu (ikona cogged kotačić), a zatim kliknite **Internetske mogućnosti**. Kliknite karticu **Sigurnost** , kliknite ikonu **Pouzdana web-mjesta** , a zatim gumb **web-mjesta** . Dodavanje http://*. mysql.com na popis pouzdanih web-mjesta. Kliknite * *Zatvori**, a zatim kliknite * *u redu**.
3.  U adresnu traku programa Internet Eksplorera, upišite http://dev.mysql.com/downloads/mysql/.
4.  Koristite MySQL web-mjesta za pronalaženje i preuzimanje najnoviju verziju MySQL instalacijski program za Windows. Prilikom odabira MySQL instalacijski program, preuzmite verziju koja ima potpunu datoteku postavljanje (, na primjer, u mysql-instalacijskog programa – zajednica – 5.6.23.0.msi veličina datoteke 282.4 MB), a potom Spremi instalacijski program.
5.  Kada instalacijski program je završio s preuzimanjem, kliknite **Pokreni** da biste pokrenuli instalaciju.
6.  Na stranici **Licencni ugovor** , prihvatite licencni ugovor, a zatim kliknite **Dalje**.
7.  Na stranici **Odabir vrste instalacije** kliknite željenu vrstu postavljanje, a zatim kliknite **Dalje**. Sljedeći koraci pretpostavlja odabira vrstu instalacije **samo poslužitelj** .
8.  Na stranici za **instalaciju** kliknite **izvrši**. Nakon dovršetka instalacije, kliknite **Dalje**.
9.  Na stranici **Konfiguracija proizvod** , kliknite **Dalje**.
10. Na stranici **vrste i povezivanje s mrežom** , navedite željena konfiguracija vrsta i povezivanjem mogućnosti, uključujući TCP priključak ako je potrebno. Odaberite **Pokaži dodatne mogućnosti**, a zatim kliknite **Dalje**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_TypeNetworking.png)

11. Na stranici **računi i ulogama** navedite jaku lozinku korijenski MySQL. Po potrebi dodajte dodatne MySQL korisnički računi, a zatim kliknite **Dalje**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AccountsRoles_Filled.png)

12. Na stranici **Servisa Windows** navedite promjene zadanih postavki za pokretanje poslužitelj MySQL kao servis Windows prema potrebi, a zatim kliknite **Dalje**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_WindowsService.png)

13. Na stranici **Napredne mogućnosti** odredite promjene mogućnosti zapisivanja prema potrebi, a zatim kliknite **Dalje**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AdvOptions.png)

14. Na stranici za **Konfiguraciju poslužitelja za primjenu** kliknite **izvrši**. Kada završite navedeni koraci za konfiguraciju, kliknite **Završi**.
15. Na stranici **Konfiguracija proizvod** , kliknite **Dalje**.
16. Na stranici **Instalacija dovrši** , kliknite **Zapisnik Kopiraj u međuspremnik** ako želite da biste je kasnije pregledati, a zatim kliknite **Završi**.
17. Na početnom zaslonu upišite **mysql**, a zatim **MySQL 5.6 naredbenog retka klijenta**.
18. Unesite lozinku za korijenske koje ste naveli u koraku 11 i prikazat će se upit s upitom gdje možete izdati naredbe za konfiguriranje MySQL. Detalji o naredbi i sintaksa potražite u članku [Priručnike MySQL referencu](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_CommandPrompt.png)

19. Konfiguracija zadane postavke poslužitelja, kao što su osnovni i podataka iz direktorija i pogona, možete konfigurirati i sa stavkama u datoteci 5.6\my-default.ini poslužitelja \MySQL\MySQL C:\Program Files (x86). Dodatne informacije potražite u članku [5.1.2 zadanih postavki konfiguriranje poslužitelja](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Konfiguriranje krajnje točke

Ako želite da se servis poslužitelj MySQL da bi bio dostupan MySQL klijentskim računalima na Internetu, morate konfigurirati krajnje točke za priključak TCP na kojoj se servis poslužitelj MySQL priključuje i stvorili dodatna pravila vatrozida za Windows. Ovo je TCP priključak 3306 osim ako drugi priključak naveden na stranici za **vrstu i povezivanje s mrežom** (korak 10 od prethodnog postupka).


> [AZURE.NOTE] Pažljivo razmislite o utjecaju na sigurnost na taj način, jer to će biti servis poslužitelj MySQL dostupne na svim računalima na Internetu. Možete definirati skup izvorni IP adrese dopušteni da biste koristili krajnju točku s pristupom kontrola popisa (ACL). Dodatne informacije potražite [u](virtual-machines-windows-classic-setup-endpoints.md)članku postavljanje točke za virtualnog računala.


Da biste konfigurirali krajnje točke za servis MySQL poslužitelja:

1.  Azure klasični portalu kliknite **virtualnih računala**, kliknite naziv svoje MySQL virtualnog računala, a zatim **krajnje točke**.
2.  Na naredbenoj traci kliknite **Dodaj**.
3.  Na stranici **Dodavanje krajnje točke za virtualnog računala** , kliknite strelicu desno.
4.  Ako koristite zadani je priključak za 3306 MySQL TCP, kliknite **MySQL** u **nazivu**, a zatim kvačicu.
5.  Ako koristite drugi TCP priključak, u **odjeljak naziv**upišite jedinstveni naziv. Odaberite **TCP** protokol, unesite broj priključka u **Priključak javne** i **Privatne priključak**i kliknite kvačicu.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>Dodavanje pravila vatrozida za Windows da dopušta promet MySQL

Da biste dodali pravilo vatrozida za Windows koje dopušta promet MySQL s Interneta, pokrenite sljedeću naredbu povećane komponente Windows PowerShell naredbeni redak MySQL poslužitelja virtualnog računala.

    New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public


    
## <a name="test-your-remote-connection"></a>Testiranje veze s udaljenom


Da biste testirali udaljene vezu sa servisom poslužitelj MySQL sustavom Azure virtualnog računala, prvo morate odrediti naziv DNS-a koji odgovara servisa u oblaku koja sadrži virtualnog računala koja se izvodi MySQL Server.

1.  Azure klasični portalu kliknite **virtualnim strojevima**, kliknite naziv svoje MySQL poslužitelj virtualnog računala, a zatim **nadzorne ploče**.
2.  Na nadzornoj ploči virtualnog računala Imajte na umu vrijednost iz polja **Naziv DNS-a** u odjeljku **Brzi Glance** . Evo jednog primjera:

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_DNSName.png)

3.  S lokalnog računala na kojem MySQL ili MySQL klijentu, pokrenite sljedeću naredbu da biste se prijavili kao korisnik MySQL.

        mysql -u <yourMysqlUsername> -p -h <yourDNSname>

    Na primjer, dbadmin3 programa MySQL korisničko ime i naziv DNS testmysql.cloudapp.net virtualnog računala, koristite sljedeću naredbu.

        mysql -u dbadmin3 -p -h testmysql.cloudapp.net


## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o pokretanju MySQL potražite u [Dokumentaciji MySQL](http://dev.mysql.com/doc/).
