<properties
    pageTitle="Postavljanje Apache Tomcat na Linux VM | Microsoft Azure"
    description="Saznajte kako postaviti Apache Tomcat7 pomoću programa Azure virtualnog računala (VM) radi Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="how-to-set-up-tomcat7-on-a-linux-virtual-machine-with-microsoft-azure"></a>Kako postaviti Tomcat7 na Linux virtualnog računala s Microsoft Azure

Apache Tomcat (ili jednostavno Tomcat, prethodno i Tomcat Džakarta) je Otvori izvor web-poslužitelj i servlet spremnik razvio tako da na Apache softver Foundation (ASF). Tomcat implementira Java Servlet i specifikacije JavaServer stranice (JSP) iz Ned Microsystems i njihovi čisto Java HTTP web-poslužitelj okruženju u kojem želite pokrenuti kod Java. U konfiguraciji najjednostavniji Tomcat pokreće se u jedan operacijski sustav procesa. Ovaj postupak pokreće Java virtualnog računala (JVM). Svaki HTTP zahtjev putem preglednika za Tomcat obrađuje kao zasebna niti u postupku Tomcat.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


U ovom vodiču ćete instalirati tomcat7 na sliku Linux i implementacije u Microsoft Azure.  

Saznat ćete:  

-   Kako stvoriti virtualnog računala u Azure.
-   Kako pripremiti virtualnog računala za tomcat7.
-   Kako se instalira tomcat7.

Pretpostavlja se da je čitač već ima Azure pretplate.  Ako ne možete se prijaviti za besplatnu probnu verziju na [http://azure.microsoft.com](https://azure.microsoft.com/). Ako imate pretplatu na MSDN, potražite u članku [Microsoft Azure posebne cijene: MSDN, mreže Microsoftovih Partnera i pogodnosti Bizspark](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Da biste saznali više o Azure, pročitajte članak [što je Azure?](https://azure.microsoft.com/overview/what-is-azure/).

U ovoj se temi podrazumijeva Osnovni radni znanja tomcat i Linux.  

##<a name="phase-1-create-an-image"></a>Faza 1: Stvaranje slike
U ovoj fazi će stvoriti virtualnog računala pomoću Linux slike u Azure.  

###<a name="step-1-generate-an-ssh-authentication-key"></a>Korak 1: Stvaranje ključa SSH provjera autentičnosti
SSH je važno alat za administratore sustava. Međutim, konfiguriranje lozinke Ljudski određuje na temelju sigurnosti programa access nije preporučenim načinom rada. Zlonamjerni korisnici možete prekinuti u sustav koji se temelji na korisničko ime i lozinku loš.

Dobra je vijest da postoji način za daljinski pristup ostaviti otvorenu i ne morate brinuti o lozinkama. Način sastoji se od provjera autentičnosti s asimetričnim šifriranja. Privatni ključ korisnika je onaj koji daje provjeru autentičnosti. Možete čak i obvezno na korisnički račun da bi se onemogućilo potpuno provjeru autentičnosti lozinke.

Prednosti ove metode je potrebno različite lozinke za prijavu na različitim poslužiteljima. Možete provjeriti autentičnost na svim poslužiteljima pomoću osobnog privatni ključ koja onemogućava na umu nekoliko lozinke.

Također moguće je prijava s lozinke ako koristite taj način.

Slijedite ove korake da biste generirali tipku SSH provjeru autentičnosti.

1.  Preuzmite i instalirajte puttygen iz na sljedećem mjestu: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2.  Pokrenite PUTTYGEN. EXE.
3.  Kliknite **Generiraj** da biste generirali tipki. U postupku možete povećati nepravilnosti Pomicanjem miša iznad prazno područje u prozoru.  
![][1]
4.  Nakon postupka Generiraj Puttygen.exe vode generirani ključ. Ako, na primjer:  
![][2]
5.  Odaberite i kopirajte javni ključ u **ključu** i spremite ga u datoteku pod nazivom publicKey.pem. Nemojte kliknuti **Spremi javni ključ**, jer se razlikuje od javni ključ želimo oblik datoteke spremljene javnog ključa.
6.  Kliknite **Spremi privatni ključ** i spremite ga u datoteku pod nazivom privateKey.ppk.

###<a name="step-2-create-the-image-in-the-azure-portal"></a>Korak 2: Stvaranje slike na portalu za Azure.
[Portal za Azure](https://portal.azure.com/)kliknite **Novo** na traci zadataka da biste stvorili sliku, odabir slike Linux ovisno o vašim potrebama. Sljedeći primjer koristi Ubuntu 14.04 sliku.
![][3]

Za **Naziv glavnog računala** za Navedite naziv URL koji vam i Internet klijenti će koristiti za pristup ovom virtualnog računala. Definiranje u posljednjem dijelu DNS naziva, primjerice tomcatdemo i Azure će generiranje URL-a kao tomcatdemo.cloudapp.net.  

Za **SSH provjere autentičnosti ključ**, kopirajte vrijednost ključa iz datoteke **publicKey.pem** koja sadrži javni ključ generira puttygen.  
![][4]

Konfigurirajte ostale postavke prema potrebi, a zatim kliknite Stvori.  

##<a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Faza 2: Priprema virtualnog računala za Tomcat7
U ovoj fazi će konfigurirati krajnje točke za tomcat promet i povežite se s novi virtualnog računala.
###<a name="step-1-open-the-http-port-to-allow-web-access"></a>Korak 1: Otvaranje HTTP priključak dopustiti pristup web
Krajnje točke u Azure se sastojati od protocol (TCP i UDP), zajedno s javnim i privatnim priključak. Privatni priključak je priključka koji se priključuje usluge na virtualnog računala. Javni priključak je priključka koji Azure oblaku priključuje na vanjsko za dolazne, internetske promet.  

TCP priključak 8080 je zadani broj priključka na koji tomcat očekuje podatke. Otvaranje priključak s krajnje Azure omogućuje vam i druge klijente pristup Internetu tomcat stranice.  

1.  Na portalu Azure kliknite **Pregledaj** -> **virtualnog računala**, a zatim kliknite virtualnog računala koju ste stvorili.  
![][5]
2.  Da biste dodali krajnje virtualnog računala, kliknite okvir **krajnje točke** .
![][6]
3.  Kliknite **Dodaj**.  
    1.  Za **krajnje točke**upišite naziv za krajnju točku u krajnje točke, a zatim upišite 80 priključkom **Javno**.  

        Ako je postavljena na 80, ne morate uključiti broj priključka u URL koji vam omogućuje pristup tomcat. Na primjer, http://tomcatdemo.cloudapp.net.    

        Ako je postavljeno na drugu vrijednost, kao što su 81, morate dodati broj priključka URL za izravan pristup tomcat. Na primjer, http://tomcatdemo.cloudapp.net:81 /.
    2.  Upišite 8080 priključkom privatno. Prema zadanim postavkama tomcat očekuje podatke TCP priključak 8080. Ako ste promijenili zadani preslušali priključak tomcat, trebali biste ažurirati priključak privatno da je isti kao u tomcat preslušali priključak.  
    ![][7]

4.  Kliknite **u redu** da biste dodali krajnju točku virtualnog računala.



###<a name="step-2-connect-to-the-image-you-created"></a>Korak 2: Povežite se sa slikom koju ste stvorili.
Možete odabrati bilo koji SSH alat za povezivanje s virtualnog računala. U ovom primjeru koristimo Putty.  

Najprije pronađite DNS naziva virtualnog računala s portala za Azure. **Kliknite Pregledaj** -> **virtualnim strojevima** -> naziv virtualnog računala -> **Svojstva**, a zatim u polje **Naziv domene** pločice **Svojstva** .  

Da biste dobili broj priključka za SSH veze iz polja **SSH** . Evo primjera.  
![][8]

Preuzmite Putty iz [ovdje](http://www.putty.org/) .  

Nakon preuzimanja, kliknite izvršne datoteke PUTTY. EXE. Konfiguriranje osnovnih mogućnosti uz naziv glavnog računala i priključka broj dobivenog svojstva virtualnog računala. Evo jednog primjera:  
![][9]

U lijevom oknu kliknite **vezu** -> **SSH** -> **provjeru autentičnosti** , a zatim kliknite **Pregledaj** da biste odredili mjesto datoteke **privateKey.ppk** koja sadrži privatni ključ generira puttygen u fazi 1: Stvaranje slike. Evo jednog primjera:  
![][10]

Kliknite **Otvori**. Možda će upozoreni tako da u okviru poruke. Ako ste konfigurirali naziv DNS-a, a broj priključka pravilno, kliknite **da**.
![][11]  


Trebali biste vidjeti sljedeće:  
![][12]

Unesite korisničko ime navedeno prilikom stvaranja virtualnog računala u fazi 1: Stvaranje slike. Prikazat će otprilike ovako:  
![][13]





##<a name="phase-3-install-software"></a>Faza 3: Instalacija softvera
U ovoj fazi instalirajte okruženje za izvođenje Java, tomcat i druge komponente tomcat.  

###<a name="java-runtime-environment"></a>Okruženje za izvođenje Java
Tomcat napisan Java. Postoje dvije vrste Java razvojni paketi (JDKs) (OpenJDK i Oracle JDK), a možete odabrati onu koju želite.  

>AZURE. Napomena: Obje JDKs imati gotovo jednake šifre klasa Java API, ali šifru virtualnog računala razlikuje se zapravo. Kada je riječ o bibliotekama, OpenJDK ima da biste koristili Otvori biblioteke pri Oracle ima da biste koristili zatvoreni one. No Oracle JDK sadrži više klase i neke popravi programskih pogrešaka i Oracle JDK je više stabilan od OpenJDK.

Sljedeće naredbe preuzmite različite JDKs.  

Otvori jdk   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  

Oracle jdk  

-   Da biste preuzeli na JDK Oracle web-mjestu:  

        wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  

-   Da biste stvorili direktorij koja će sadržavati JDK datoteke:  

        sudo mkdir /usr/lib/jvm  

-   Da biste izdvojili JDK datoteke u/korsnik pitanje/biblioteka/jvm/imenik:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  

-   Da biste postavili Oracle JDK kao zadani JVM:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  
        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

####<a name="test"></a>Test:
Da biste testirali ako okruženje za izvođenje Java ispravno instaliran možete koristiti naredbu ovako:  

    java -version  

Ako ste instalirali Otvori jdk, trebali biste vidjeti poruku kao što je sljedeća:![][14]

Ako ste instalirali oracle jdk, trebali biste vidjeti poruku kao što je sljedeća:![][15]

###<a name="tomcat7"></a>Tomcat7
Da biste instalirali tomcat7 putem sljedeću naredbu:  

    sudo apt-get install tomcat7  

Ako se ne koriste tomcat7, pomoću odgovarajuće varijacije naredbe.  

####<a name="test"></a>Test:

Da biste provjerili je li tomcat7 uspješno instaliran, pronađite naziv poslužitelja tomcat DNS-a (http://tomcatexample.cloudapp.net/ je primjeru URL-a u ovom članku). Ako se prikaže stranica ovako, imate tomcat7 instaliran ispravan.
![][16]


###<a name="install-other-tomcat-components"></a>Instalacija druge komponente Tomcat
Postoje drugi neobavezno tomcat komponente koje možete instalirati.  

Koristite naredbu **tomcat7 sudo Zemaljska predmemoriju pretraživanja** da biste vidjeli sve dostupne komponente. Sljedeće naredbe su primjeri da biste instalirali neki dijelovi korisno.  

    sudo apt-get install tomcat7-admin      #admin web applications
    sudo apt-get install tomcat7-user         #tools to create user instances  

##<a name="phase-4-configure-tomcat"></a>Faza 4: Konfiguriranje Tomcat
U ovoj fazi administriranje tomcat.
###<a name="start-and-stop-tomcat7"></a>Pokretanje i zaustavljanje tomcat7
Poslužitelj tomcat7 će se automatski pokrenuti prilikom instalacije. Možete započeti i ga sami pomoću sljedeće naredbe:   

    sudo /etc/init.d/tomcat7 start

Da biste prekinuli tomcat7:  

    sudo /etc/init.d/tomcat7 stop

Da biste pogledali status tomcat7:  

    sudo /etc/init.d/tomcat7 status

Da biste ponovno pokrenuli tomcat usluge:  

    sudo /etc/init.d/tomcat7 restart

###<a name="tomcat-administration"></a>Administracija tomcat
Možete urediti konfiguracijska datoteka Tomcat korisnik postavljanje administratorske vjerodajnice pomoću sljedeće naredbe:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Evo jednog primjera:  
![][17]  

>AZURE. Napomena: Stvaranje neprobojne lozinke za administratore korisničko ime.  

Nakon uređivanja tu datoteku, morate ponovno započeti tomcat7 usluge s ugovorima o sljedeću naredbu da bi se promjene primijenile:  

    sudo /etc/init.d/tomcat7 restart  

Otvorite preglednik i unesite URL **http://<your tomcat server DNS name>/Upravitelj/html**. Na primjer u ovom članku, URL je http://tomcatexample.cloudapp.net/manager/html.  

Nakon povezivanja, trebali biste vidjeti nešto slično sljedećem:  
![][18]

##<a name="common-issues"></a>Uobičajeni problemi

###<a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Ne mogu pristupiti virtualnog računala s Tomcat i Moodle s Interneta

-   **Simptoma**  
Tomcat je pokrenut, ali ne možete vidjeti Tomcat zadane stranice s vašim preglednikom.
-   **Mogući korijenski slučaja**   
    1.  Priključak preslušavanja tomcat nije isti kao privatno priključak krajnje točke virtualnog računala za tomcat promet.  

        Provjerite postavke krajnje točke za priključak javne i privatne priključak i provjerite je li Port privatno jednako kao i u tomcat preslušali port (priključak). U odjeljku faza 1: Stvaranje slike upute o konfiguriranju krajnje točke za virtualnog računala.  

        Da biste utvrdili priključak preslušavanja tomcat, otvorite /etc/httpd/conf/httpd.conf (crveno je vaša izdanje) ili /etc/tomcat7/server.xml (Debian izdanje). Po zadanom je priključak za preslušavanja tomcat 8080. Evo jednog primjera:  

            <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"  URIEncoding="UTF-8"            redirectPort="8443" />  

        Ako koristite virtualnog računala kao Debian ili Ubuntu i želite da biste promijenili u zadani port (priključak) od Tomcat preslušali (na primjer 8081), trebali biste otvoriti i priključka za os-a. Najprije otvorite profila:  

            sudo vi /etc/default/tomcat7  

        Zatim uklonite posljednjeg retka i promijenite "ne" u "da".  

            AUTHBIND=yes

    2.  Vatrozid je onemogućio priključak preslušavanja tomcat.

        Ako možete vidjeti samo Tomcat zadane stranice s lokalnog računala, zatim problem najvjerojatnije priključak koji je Uvažili po tomcat je blokirala vatrozida. Alat za w3m možete koristiti da biste pregledali web-stranicu. Sljedeće naredbe instalacija w3m i otvorite stranicu na zadane Tomcat:  

            sudo yum  install w3m w3m-img
            w3m http://localhost:8080  

-   **Rješenja**
    1. Ako na tomcat preslušali priključak nije isti kao privatno priključak krajnje točke radi prometa virtualnog računala, morate promijeniti priključak privatno da je isti kao u tomcat preslušali priključak.   

    2.  Ako se problem uzrokuje vatrozid/iptables, dodajte sljedeće retke /etc/sysconfig/iptables:  

            -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
            -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT  

        Imajte na umu da u drugom retku samo potrebne za https promet.  

        Provjerite je li to iznad sve retke koji se globalno želite ograničiti pristup, kao što je sljedeće:  

            -A INPUT -j REJECT --reject-with icmp-host-prohibited  

        Da biste ponovno učitali u iptables, pokrenite sljedeću naredbu:  

            service iptables restart  

        To testirano na 6,3 CentOS.

###<a name="permission-denied-when-upload-you-project-files-to-varlibtomcat7webapps"></a>Dozvole zabranjen kada prijenos projektnih datoteka /var/lib/tomcat7/webapps /  

-   **Simptoma**  
Kada koristite bilo kojeg SFTP klijenta (primjerice FileZilla) za povezivanje s virtualnog računala, a zatim otvorite /var/lib/tomcat7/webapps/za objavljivanje na web-mjesto, koje primite poruku o pogrešci slična sljedećoj:  

        status: Listing directory /var/lib/tomcat7/webapps
        Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
        Error:  /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
        Error:  File transfer failed

-   **Mogući korijenski slučaja** Nemate dozvolu za pristup mapi /var/lib/tomcat7/webapps.  
-   **Rješenja**  
Morate dobiti dozvolu s računa za korijen. Vlasništvo nad tu mapu možete promijeniti iz korijenske za korisničko ime koje ste koristili prilikom dodjele resursa na računalu. Evo primjera s nazivom azureuser:  

        sudo chown azureuser -R /var/lib/tomcat7/webapps

    Koristite mogućnost -R da biste primijenili dozvole za sve datoteke unutar direktorij previše.  

    Imajte na umu da ta naredba funkcionira i za direktorija. Mogućnost -R je promijenio dozvole za sve datoteke i mape unutar imenika. Evo jednog primjera:  

        sudo chown -R username:group directory  

    Ta se naredba mijenja vlasništvo (korisnika i grupa) za sve datoteke i mape unutar direktorija i imenik sam.  

    Sljedeću naredbu samo promjene dozvola direktorija za mapu, ali ostavlja samostalno datoteka i mapa u direktoriju.  

        sudo chown username:group directory



[1]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
