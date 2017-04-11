<properties
   pageTitle="Implementacija Node.js aplikacije da biste Linux virtualnim strojevima servisu Azure"
   description="Saznajte kako implementirati aplikaciju Node.js za Linux virtualnim strojevima u Azure."
   services=""
   documentationCenter="nodejs"
   authors="stepro"
   manager="dmitryr"
   editor=""/>

<tags
   ms.service="multiple"
   ms.devlang="nodejs"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/02/2016"
   ms.author="stephpr"/>

# <a name="deploy-a-nodejs-application-to-linux-virtual-machines-in-azure"></a>Implementacija Node.js aplikacije da biste Linux virtualnim strojevima servisu Azure

Pomoću ovog praktičnog vodiča pokazuje kako da biste snimili Node.js aplikacije i implementacija Linux virtualnim strojevima sa servisu Azure. U operacijskom sustavu koji se može pokrenuti Node.js mogu slijediti upute u ovom ćete praktičnom vodiču.

Ćete saznati kako:

- O ogranku i Kloniraj GitHub spremište koje sadrže jednostavan popis obveza aplikaciju;
- Stvaranje i konfiguriranje dva Linux virtualnim strojevima u Azure da biste pokrenuli aplikaciju;
- Iteracija u aplikaciji pritiskom ažuriranja za web sučelju virtualnog računala.

> [AZURE.NOTE]
> Da biste dovršili ovaj Praktični vodič, morate GitHub račun i račun sustava Microsoft Azure i mogućnost korištenja brojka razvoj računalu.

> Ako nemate račun za GitHub, možete se prijaviti [u nastavku](https://github.com/join).

> Ako nemate račun sustava [Microsoft Azure](https://azure.microsoft.com/) , možete se prijaviti za na BESPLATNU probnu [ovdje](https://azure.microsoft.com/pricing/free-trial/). To će i vas voditi kroz postupak prijave za [Microsoftov račun](http://account.microsoft.com) ako nije već postoji. Umjesto toga, ako ste pretplatnik Visual Studio, možete ga [aktivirati svoje prednosti MSDN](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> Ako nemate brojka na vašem računalu razvoj, pa ako koristite računalo Macintosh ili Windows, instalirajte brojka [ovdje](http://www.git-scm.com). Ako koristite Linux, instalirajte brojka pomoću mehanizam najprikladnije, kao što su `sudo apt-get install git`.

## <a name="forking-and-cloning-the-todo-application"></a>Forking i kloniranje aplikacija za obveze

Aplikacija za obveze koristi ovog praktičnog vodiča implementira u sučelju jednostavni web putem instancu komponente MongoDB evidentira popis obveze. Nakon prijave u GitHub, idite [u nastavku](https://github.com/stepro/node-todo) da biste pronašli aplikacije i fork ga pomoću veze u gornjem desnom kutu. Na vašem računu pod nazivom *accountname*/node-todo to potrebno stvoriti spremište.

Sada na vašem računalu razvoj Kloniraj ovo spremište:

    git clone https://github.com/accountname/node-todo.git

Ćemo koristiti ovaj lokalne Kloniraj u spremištu malo kasnije prilikom mijenjanja izvornog koda.

## <a name="creating-and-configuring-the-linux-virtual-machines"></a>Stvaranje i konfiguriranje virtualnim strojevima Linux

Azure ima sjajno podršku za neobrađenog računalnim pomoću virtualnim strojevima Linux. Taj dio vodiča prikazuje kako možete jednostavno Okretni gore dva Linux virtualnim strojevima i implementaciju aplikacija za obveze na njih radi sučelju web na jednom i instancu MongoDB na drugi.

### <a name="creating-virtual-machines"></a>Stvaranje virtualnim strojevima

Da biste stvorili novi virtualnog računala u Azure najjednostavnije da biste koristili Portal za Azure. Kliknite [ovdje](https://portal.azure.com) za prijavu i pokretanje Azure Portal u web-pregledniku. Kada na portalu Azure učitana, slijedite sljedeće korake:

- Kliknite vezu "+ novi";
- Odaberite kategoriju "Računalnim", a zatim odaberite "Ubuntu Server 14.04 LTS";
- Odaberite model implementacije "Voditelj resursa", a zatim kliknite "Stvori";
- Ispunite osnove slijedite ove smjernice:
  - Navedite naziv možete jednostavno prepoznati kasnije;
  - U ovom ćete praktičnom vodiču odaberite provjeru autentičnosti lozinke;
  - Stvorite novu grupu resursa identifiable naziva.
- Veličina virtualnog računala "A1 standardni" je pametnije izbor za ovog praktičnog vodiča.
- Za dodatne postavke, provjerite je li na disku vrsta "Standard" i prihvatiti sve preostale zadane vrijednosti.
- Izbaciti stvaranja na stranicu sažetak.

Izvođenje postupka iznad dvaput da biste stvorili dva Linux virtualnim strojevima, jedan za sučelju web i jedan za instancu MongoDB. Stvaranje virtualnim strojevima će otprilike 5 10 minuta.

### <a name="assigning-a-dns-entry-for-virtual-machines"></a>Dodjeljivanje unos DNS virtualnim strojevima

Virtualnim strojevima stvorene u Azure su po zadanom samo pristupiti kroz javnu IP adresu kao što je 1.2.3.4. Pogledajmo provjerite strojeva jednostavnije njenu tako da im dodijelite DNS stavke.

Portal označava virtualnim strojevima ste je stvorili, kliknite vezu "Virtualnim strojevima" u lijevom navigacijska traka i pronađite vašeg računala. Na svakom računalu:

- Pronađite karticu osnove, a zatim kliknite na javnu IP adresu;
- Javnu IP adresa konfiguracija dodijeliti oznaku naziv DNS-a i spremite.

Na portalu će provjerite je li navedeno ime, dostupne. Kada spremite konfiguraciju virtualnim strojevima će imati slično nazivi glavnog računala `machinename.region.cloudapp.azure.com`.

### <a name="connecting-to-the-virtual-machines"></a>Povezivanje s virtualnim strojevima

Kada su tamo virtualnim strojevima, one su unaprijed konfigurirana putem SSH dopustiti daljinske veze. Ovo je mehanizam koristit ćemo da biste konfigurirali virtualnih računala. Ako koristite Windows za vaše razvoj, morat ćete dobiti klijent za SSH ako nije već postoji. Uobičajeni odabir je PuTTY, možete preuzeti s [ovdje](http://www.chiark.greenend.org.uk/~sgtatham/putty/). Macintosh i operacijskim sustavom Linux isporučuje se s verziju SSH unaprijed instalirano.

### <a name="configuring-the-web-frontend-virtual-machine"></a>Konfiguriranje Web sučelju virtualnog računala

SSH stroj sučelju web koji ste stvorili pomoću PuTTY, ssh naredbenog retka ili na drugi omiljene SSH alat. Trebali biste vidjeti poruke dobrodošlice slijedi naredbeni redak.

Najprije ćemo provjerite da brojka i čvor jesu li oba instalirani:

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs
    
Budući da sučelju web aplikacije ovisi o neke nativni modula Node.js, moramo i instalirajte ključna skup alata za sastavljanje:

    sudo apt-get install -y build-essential

Na kraju, recimo instalirati aplikaciju Node.js naziva *zauvijek*, što pomaže da biste pokrenuli Node.js poslužitelj aplikacije:

    sudo npm install -g forever
    
To su sve ovisnosti potrebno na ovom virtualnog računala da biste mogli pokrenuti sučelju web aplikacije, pa ćemo dobiti te pokrenut. Da biste to učinili, ne možemo će stvoriti Gola Kloniraj u spremištu GitHub prethodno forked tako da možete jednostavno objavljivanje ažuriranja virtualnog računala (ćemo objasniti scenarij ažuriranje kasnije), a Kloniraj Gola Kloniraj omogućuju verziju spremišta koje možete izvršiti zapravo.

Pokretanje iz imenika Polazno (~), pokrenite sljedeće naredbe (zamjena *accountname* naziv GitHub korisničkog računa):

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

Sada unesite direktorij čvor obveze i pokrenite sljedeće naredbe:

    npm install
    forever start server.js
    
Sada traje sučelju web aplikacije, no postoji još jedan korak aplikaciju mogli pristupiti u web-pregledniku. Virtualnog računala koju ste stvorili zaštićen Azure resursa koji se naziva *mreže sigurnosne grupe*, koji je stvoren za vas pri dodjeli virtualnog računala. Trenutno ovaj resurs samo omogućuje vanjske zahtjevi za priključak 22 za usmjeriti virtualnog računala koja omogućuje SSH komunikacije s računala, ali ništa drugo. Da biste pregledavali aplikacija za obveze koji je konfiguriran za pokretanje na priključak 8080, priključak i potrebno može otvoriti prema gore.

Vratite se na Azure Portal i napravite sljedeće:

- Kliknite na "Grupa resursa" u lijevom navigacijska traka;
- Odaberite grupu resursa koji sadrži virtualnog računala;
- Na popisu rezultata resursa odaberite mrežni sigurnosne grupe (jedan s ikonom štita);
- U svojstvima, odaberite "ulaznog sigurnosna pravila";
- Na alatnoj traci kliknite "Dodaj";
- Navedite naziv kao što su "zadani-Dopusti-obveze";
- Postavljanje protokola za "TCP";
- Postavljanje odredišni priključak raspon za "8080";
- Kliknite u redu i pričekajte da sigurnosna pravila će biti stvoren.

Kada stvorite pravilo za sigurnost, aplikacija za obveze je javno vidljivo na Internetu, a možete pregledavati, na primjer pomoću URL-a kao što su:

    http://machinename.region.cloudapp.azure.com:8080

Primijetit ćete da čak i ako se ne možemo još niste konfigurirali virtualnog računala MongoDB, aplikacija za obveze čini se da je prilično funkcionirati. To je zato je spremište izvora koji nisu da biste koristili unaprijed distribuiranih MongoDB instance. Nakon što smo ste konfigurirali virtualnog računala MongoDB, ne možemo će vratite i promijenite izvornog koda mogu koristiti naše privatne instancu MongoDB umjesto toga.

### <a name="configuring-the-mongodb-virtual-machine"></a>Konfiguriranje MongoDB virtualnog računala

SSH na drugo računalo koje ste stvorili pomoću PuTTY, ssh naredbenog retka ili na drugi omiljene SSH alat. Nakon što se prikazuju poruke dobrodošlice i naredbeni redak, instalirajte MongoDB (ove upute okrenutim iz [ovdje](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

Prema zadanim postavkama MongoDB konfiguriran je tako da ga može pristupiti samo lokalno. Za ovaj vodič smo će konfigurirati MongoDB tako da se može pristupiti iz aplikacije virtualnog računala. U kontekstu sudo, otvorite datoteku /etc/mongod.conf i pronađite u `# network interfaces` sekciju. Promjena u `net.bindIp` konfiguracije vrijednost `0.0.0.0`.

> [AZURE.NOTE]
> U ovom konfiguriran za potrebe ovog praktičnog vodiča samo. **Nije preporučena sigurnosna vježbe** , a ne treba koristiti u radnom okruženju.

Sada bi MongoDB pokrenut servis za:

    sudo service mongod restart

MongoDB pristajete putem priključka 27017 prema zadanim postavkama. Tako, na isti način koje ćemo potrebne da biste otvorili priključak 8080 na webu sučelju virtualnog računala moramo da biste otvorili priključak 27017 na MongoDB virtualnog računala.

Vratite se na Azure Portal i napravite sljedeće:

* Kliknite na "Grupa resursa" u lijevom navigacijska traka;
* Odaberite grupu resursa koji sadrži virtualnog računala MongoDB;
* Na popisu rezultata resursa odaberite mrežni sigurnosne grupe (jedan s ikonom štita) s istim nazivom koji ste dodijelili virtualnog računala MongoDB;
* U svojstvima, odaberite "ulaznog sigurnosna pravila";
* Na alatnoj traci kliknite "Dodaj";
* Navedite naziv kao što su "zadani-Dopusti-mongo";
* Postavljanje protokola za "TCP";
* Postavljanje odredišni priključak raspon za "27017";
* Kliknite u redu i pričekajte da sigurnosna pravila će biti stvoren.

## <a name="iterating-on-the-todo-application"></a>Iterating aplikacija za obveze
Dosad ste dodjeli dva Linux virtualnim strojevima: onaj koji se izvodi u aplikaciji web sučelju i jedan koji se izvodi MongoDB instance. No postoji problem – sučelju web nije zapravo dodijeljenu instancu MongoDB još koristite. Pogledajmo riješite koji ažuriranjem kod sučelju web tako da koristi varijabla okruženja umjesto instancu komponente programiranih.

### <a name="changing-the-todo-application"></a>Promjena obveze aplikacije

Na računalu razvoj gdje najprije klonirana spremište čvor obveze otvorite na `node-todo/config/database.js` datoteku u uređivaču za omiljene i promijenite vrijednost url iz programiranih vrijednosti kao što su `mongodb://...` da biste `process.env.MONGODB`.

Zapiši promjene, a zatim na matrici GitHub:

    git commit -am "Get MongoDB instance from env"
    git push origin master

Nažalost, to ne objavite promjene web sučelju virtualnog računala. Pogledajmo mijenjati nekoliko više tog virtualnog računala da biste omogućili jednostavni, ali učinkovitih mehanizam za objavljivanje ažuriranja tako da možete brzo pridržavajte promjene u aktivnom okruženju.

### <a name="configuring-the-web-frontend-virtual-machine"></a>Konfiguriranje Web sučelju virtualnog računala
Opoziv da ne možemo ranije stvorili Gola Kloniraj u spremištu čvor poslova na webu sučelju virtualnog računala. Pretvorit će se stvaraju li se ta akcija u novi brojka udaljene na koje je moguće pomiču promjene. Međutim, jednostavno margina na tom udaljene prilično osobama ne brzog iteracije modela koji razvojni inženjeri tražite kada radite na njihove kod.

Što željeli bismo mogli učiniti je osigurati da kada dođe do automatske udaljene spremište na virtualnog računala, pokrenuti program obveze automatski ažurira. Srećom, to je jednostavno da biste postigli s brojka.

Brojka izlaže broj spojnica koje se nazivaju u određenom trenutku da biste brzo akcije poduzete na spremištu. Te su navedeni pomoću skripti ljuske u u spremištu `hooks` mapu. Priključak koje se najčešće primijeniti scenarija automatsko ažuriranje je na `post-update` događaj.

U SSH sesije web sučelju virtualnog računala, promijenite u `~/node-todo.git/hooks` direktorija i dodavanje sljedeće sadržaja u datoteku pod nazivom `post-update` (zamjene `machinename` i `region` uz svoje podatke za virtualnog računala MongoDB):

    #!/bin/bash
    
    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info
    
Provjerite je li datoteka izvršna ponovnim pokretanjem sljedeće naredbe:

    chmod 755 post-update

Ova skripta osigurava da trenutnoj aplikaciji poslužitelj je zaustavljena, kod u kloniranu spremište ažurirana je da bi najnoviji, sve ažurirane ovisnosti su zadovoljena i ponovnog pokretanja poslužitelj. Također osigurava da konfigurirano okruženje u pripremu za primanje naš prvog ažuriranja aplikacije da biste instancu MongoDB s varijabla okruženja.

### <a name="configuring-your-development-machine"></a>Konfiguriranje računalu razvoj
Sada ćemo dobiti računalu razvoj priključiti na webu sučelju virtualnog računala. To je jednostavno dodavanje Gola spremište na virtualnog računala kao u alat za analizu daljinske. Pokrenite sljedeću naredbu da biste to učinili (zamjenjuju *korisničko* ime za prijavu za web sučelju virtualnog računala i *machinename* i *regije* po potrebi):

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

To je sve što je potrebno da biste omogućili margina, ili na snazi objavljivanja, promjene web sučelju virtualnog računala.

### <a name="publishing-updates"></a>Objavljivanje ažuriranja

Pogledajmo objavite jedan promjenu koju izvršena dosad tako da se aplikacija će koristiti našim MongoDB instance:

    git push azure master

Trebali biste vidjeti izlaz sličnu ovoj:

    Counting objects: 4, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 406 bytes | 0 bytes/s, done.
    Total 4 (delta 1), reused 0 (delta 0)
    remote: info:    Forever stopped processes:
    remote: data:        uid  command         script    forever pid  id logfile                         uptime
    remote: data:    [0] 0Lyh /usr/bin/nodejs server.js 9064    9301    /home/username/.forever/0Lyh.log 0:0:3:17.487
    remote: From /home/username/node-todo
    remote:    5f31fd7..5bc7be5  master     -> origin/master
    remote: From /home/username/node-todo
    remote:  * branch            master     -> FETCH_HEAD
    remote: Updating 5f31fd7..5bc7be5
    remote: Fast-forward
    remote:  config/database.js | 2 +-
    remote:  1 file changed, 1 insertion(+), 1 deletion(-)
    remote: npm WARN package.json node-todo@0.0.0 No repository field.
    remote: npm WARN package.json node-todo@0.0.0 No license field.
    remote: warn:    --minUptime not set. Defaulting to: 1000ms
    remote: warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
    remote: info:    Forever processing file: /home/username/node-todo/server.js
    To username@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

Po završetku ta naredba pokušajte osvježite aplikacije u web-pregledniku. Trebali biste moći vidjeti da na popisu obveze navedene ovdje je prazan i više neodlučene zajedničke distribuiranih MongoDB instanci.

Da biste dovršili vodič, recimo promjenu druga, vidljivije. Na računalu razvoj otvorite datoteku node-todo/public/index.html pomoću omiljene uređivač. Pronađite zaglavlje jumbotron i promijenite naslov s "Sam aholic obveze" da "Sam na aholic obveze na Azure!".

Sada ćemo izvršavanje:

    git commit -am "Azurify the title"

Ovaj put, recimo objavite promjene Azure prije nego ga u pozadinu GitHub repo margina:

    git push azure master

Nakon ta naredba dovrši, osvježite web-stranicu i vidjet ćete promjene. Budući da izgledaju dobro automatske promjene natrag na izvor udaljene: 

    git push origin master

## <a name="next-steps"></a>Daljnji koraci
U ovom se članku prikazivao upute da biste snimili Node.js aplikacije i implementacija Linux virtualnim strojevima sa servisu Azure. Da biste saznali više o Linux virtualnim strojevima u Azure, potražite u članku [Uvod u Linux na Azure](/documentation/articles/virtual-machines-linux-introduction/).
    
Dodatne informacije o razvoju aplikacija Node.js na Azure potražite u članku [Razvojni centar za Node.js](/develop/nodejs/).
