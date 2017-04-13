<properties
   pageTitle="Implementacija Azure virtualnog računala s Chef | Microsoft Azure"
   description="Saznajte kako koristiti Chef uvođenja automatiziranog virtualnog računala i konfiguracije na Microsoft Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="diegoviso"
   manager="timlt"
   tags="azure-service-management,azure-resource-manager"
   editor=""/>

<tags ms.service="virtual-machines-windows" ms.workload="infrastructure-services"
ms.tgt_pltfrm="vm-multiple"
ms.devlang="na"
ms.topic="article"
ms.date="05/19/2015"
ms.author="diviso"/>

# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Automatizacija implementacija Azure virtualnog računala s Chef

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Chef je odličan alat za automatizaciju i želji stanje konfiguracije.

U našem najnovije izdanje oblaka api Chef omogućuje objedinjenog integraciju sa Azure, što će vam omogućiti mogućnost Dodjela resursa i implementacija konfiguracije stanja kroz jednu naredbu.

U ovom se članku li vam pokazati kako postaviti okruženja Chef Dodjela Azure virtualnim strojevima i vodi vas kroz stvaranje pravila ili "Kuharica" i uvođenja ovaj kuharica Azure virtualnog računala.

Počnimo!

## <a name="chef-basics"></a>Osnove Chef

Prije početka, li predložiti pregledajte osnovni koncepti Chef. Postoji sjajno materijala <a href="http://www.chef.io/chef" target="_blank">ovdje</a> i li preporučujemo da imate brzi čitanje prije nego se pokušate ovaj vodič. Koje će, međutim recap osnove prije nego što smo početak rada.

Sljedeći dijagram prikazuje više razine arhitektura Chef.

![][2]

Chef ima tri glavna arhitektonski komponente: Chef poslužitelj, Chef klijenta (čvora) i Chef radne stanice.

Poslužitelj Chef naš upravljanje točke i postoje dvije mogućnosti za poslužitelj Chef: na glavnom računalu ili na lokalnim rješenja. Ne možemo koristit će glavnom računalu rješenja.

Klijent Chef (čvora) je agent koji se nalazi na poslužiteljima na kojima upravljate.

Radne stanice Chef je naš administrator radne stanice gdje ćemo stvoriti pravila za naše i izvršavanje naš upravljanje naredbe. Ne možemo pokrenite naredbu **knife** iz radne stanice Chef da biste upravljali naše infrastrukture.

Postoji pojam "Cookbooks" i "Recepti". To su učinkovito pravilnike smo definirati i primijenite našim poslužiteljima.

## <a name="preparing-the-workstation"></a>Priprema radne stanice

Najprije omogućuje Priprema radne stanice. Koristim standardne radne stanice sustava Windows. Ne možemo potrebnih za stvaranje direktorija za spremanje naš konfiguracijska datoteka i cookbooks.

Najprije stvorite direktorij pod nazivom C:\chef.

Zatim stvorite drugi direktorij pod nazivom c:\chef\cookbooks.

Sada moramo preuzimanje datoteke našu Azure postavke tako da Chef mogli komunicirati sa našu Azure pretplate.

Preuzimanje vaše postavke iz [ovdje](https://manage.windowsazure.com/publishsettings/)objavljivanja.

Spremite datoteku s postavkama Objavi u C:\chef.

##<a name="creating-a-managed-chef-account"></a>Stvaranje upravljanih računa Chef

Prijavite se na glavnom računalu Chef račun [ovdje](https://manage.chef.io/signup).

Tijekom postupka prijave će se zatražiti da biste stvorili novi tvrtke ili ustanove.

![][3]

Nakon stvaranja tvrtke ili ustanove, preuzmite paket starter.

![][4]

> [AZURE.NOTE] Ako vam se prikaže upozorenje da će vratiti ključeva za upit, je u redu da biste nastavili kao imamo bez postojeću infrastrukturu konfigurirati kao još.

Starter kit zip datoteka sadrži vaše tvrtke ili ustanove config datoteke i tipki.

##<a name="configuring-the-chef-workstation"></a>Konfiguriranje radne stanice Chef

Izdvajanje sadržaja chef starter.zip C:\chef.

Kopirajte sve datoteke u odjeljku chef starter\chef repo\.chef u direktoriju c:\chef.

Direktorija sada trebala bi izgledati nekako kao u sljedećem primjeru.

![][5]

Trebala bi se četiri datoteka uključujući Azure objavljivanje datoteke u korijenskom direktoriju c:\chef.

Datoteka PEM sadrže tvrtki ili ustanovi i privatni ključevi administrator za komunikaciju dok knife.rb datoteka sadrži konfiguraciju knife. Ne možemo morat ćete uređivati datoteku knife.rb.

Otvorite datoteku u uređivaču odabir i izmjena "cookbook_path" uklanjanjem na /... / put tako da se prikazuje kao što je prikazano u sljedećem.

    cookbook_path  ["#{current_dir}/cookbooks"]

Dodati na sljedeći redak o naziv vaše Azure objavljivanje datoteka postavki.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Datoteku knife.rb trebala izgledati slično kao u sljedećem primjeru.

![][6]

Te retke će osigurati reference direktorija cookbooks u odjeljku c:\chef\cookbooks Knife i koristi naše datoteke postavke objavljivanja Azure tijekom operacije Azure.

## <a name="installing-the-chef-development-kit"></a>Instaliranje Chef Development Kit

Sljedeći [Preuzmite i instalirajte](http://downloads.getchef.com/chef-dk/windows) ChefDK (Chef Development Kit) da biste postavili vaše radne stanice Chef.

![][7]

Instalirajte na zadano mjesto c:\opscode. Instalacije se oko 10 minuta.

Potvrda varijablu put sadrži unose za C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Ako su ne postoji, provjerite je li dodati ovih!

*IMAJTE NA UMU REDOSLIJEDA PUT JE VAŽNO!* Ako vaš opscode putovi nisu ispravnim redoslijedom imate problema.

Ponovno vaše radne stanice prije nastavka.

Nakon toga ne možemo instalirati proširenje Knife Azure. To omogućuje Knife s "Modul Azure".

Pokrenite sljedeću naredbu.

    chef gem install knife-azure ––pre

> [AZURE.NOTE] Argument – pre osigurava primate najnoviju verziju RC modul Azure Knife koji omogućuje pristup najnovijim skup API-ji.

Vjerojatno da brojne zavisnosti instalirat će se i istovremeno je.

![][8]


Da biste bili sigurni sve ispravno konfigurirano, pokrenite sljedeću naredbu.

    knife azure image list

Ako je sve ispravno konfigurirano, vidjet ćete popis dostupnih Azure slika pomičite se po.

Čestitamo. Postavljanje radne stanice!

##<a name="creating-a-cookbook"></a>Stvaranje na kuharica

Na kuharica koristi Chef definirati skup naredbi koje želite izvršiti na upravljanih klijent. Stvaranje na kuharica je jednostavne i koristimo naredbu **chef generiranje kuharica** da biste generirali naše kuharica predložak. Koje će se poziva Moje web-poslužitelj kuharica kao aspekt pravila koja se automatski uvodi IIS.

U odjeljku direktorija C:\Chef pokrenite sljedeću naredbu.

    chef generate cookbook webserver

Generirat će skup datoteke u direktoriju C:\Chef\cookbooks\webserver. Ne možemo sada potrebno definirati skup naredbi željeli bismo naš klijent Chef izvršiti na našem upravljanih virtualnog računala.

Naredbe spremaju se u datoteku default.rb. U ovoj datoteci li ćete biti koji definira skup naredbi koje se instalira IIS, pokreće IIS i kopira datoteku predloška u mapu wwwroot.

Izmjena C:\chef\cookbooks\webserver\recipes\default.rb datoteku i dodajte sljedeće retke.

    powershell_script 'Install IIS' do
        action :run
        code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
        action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
        source 'Default.htm.erb'
        rights :read, 'Everyone'
    end

Kada završite, spremite datoteku.

## <a name="creating-a-template"></a>Stvaranje predloška

Kao što smo što je već rečeno, moramo generiranje datoteku predloška koji će se koristiti kao našu stranicu default.html.

Pokrenite sljedeću naredbu da biste generirali predložak.

    chef generate template webserver Default.htm

Sada dođite do datoteke C:\chef\cookbooks\webserver\templates\default\Default.htm.erb. Uređivanje datoteke tako da dodate neke jednostavne "Pozdrav" HTML Šifra svjetskih, a zatim spremite datoteku.



## <a name="upload-the-cookbook-to-the-chef-server"></a>Prijenos na kuharica s poslužiteljem Chef

U ovom ćete koraku ćemo su poduzimanja kopiju kuharica koje smo stvorili na našem lokalnom stroju i prenosi na poslužitelj Hosted Chef. Nakon prijenosa na kuharica pojavit će se na kartici **pravila** .

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Implementacija virtualnog računala s Knife Azure

Ne možemo sada će implementacija Azure virtualnog računala i primijenite kuharica "Webserver" koje će se instalirati naš IIS web servisa i zadane web-stranicu.

Da biste to učinili, poslužite se naredbom **server knife azure stvaranje** .

Sam pojavit će se primjer naredbe sljedeće.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Parametri nisu self-explanatory. Zamjena određeni varijabli i pokrenite.

> [AZURE.NOTE] Putem na naredbenog retka mogu se i automatske pravila krajnje točke filtra mreže pomoću parametra – tcp-krajnje točke. Što učiniti nakon otvaranja gore priključke 80 i 3389 za pristup mojim web-stranicu i RDP sesiju.

Kada pokrenete naredbu, idite na portal za Azure i prikazat će se računalu početi dodjele resursa.

![][13]

U naredbeni redak prikazuje se sljedeći.

![][10]

Nakon dovršetka implementacijskih smo moći povezati s web-servisa putem priključak 80 kao što smo imali otvoriti priključak dok ne možemo dodjeli virtualnog računala pomoću naredbe Knife Azure. Kao što je ovaj virtualni stroj je samo virtualnog računala u servis u oblaku, istodobno ćete povezati oblaka servisa URL-om.

![][11]

Kao što vidite, dobio sam kreativni Moje kodom HTML.

Nemojte zaboraviti smo možete povezati putem sesiju RDP na portalu Azure klasični putem priključak 3389.

Nadam se da je to korisno! Otvorite i Započni sustavu kao kod putovanje s Azure danas!


<!--Image references-->
[2]: ./media/virtual-machines-windows-chef-automation/2.png
[3]: ./media/virtual-machines-windows-chef-automation/3.png
[4]: ./media/virtual-machines-windows-chef-automation/4.png
[5]: ./media/virtual-machines-windows-chef-automation/5.png
[6]: ./media/virtual-machines-windows-chef-automation/6.png
[7]: ./media/virtual-machines-windows-chef-automation/7.png
[8]: ./media/virtual-machines-windows-chef-automation/8.png
[9]: ./media/virtual-machines-windows-chef-automation/9.png
[10]: ./media/virtual-machines-windows-chef-automation/10.png
[11]: ./media/virtual-machines-windows-chef-automation/11.png
[13]: ./media/virtual-machines-windows-chef-automation/13.png


<!--Link references-->
