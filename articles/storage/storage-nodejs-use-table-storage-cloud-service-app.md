<properties 
    pageTitle="Web app s spremište tablica (Node.js) | Microsoft Azure" 
    description="Praktični vodič koji unapređuje na web-aplikaciji s Express vodič dodavanjem servise za pohranu Azure i modul Azure." 
    services="cloud-services, storage" 
    documentationCenter="nodejs" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robmcm"/>

# <a name="nodejs-web-application-using-storage"></a>Node.js web-aplikacije pomoću prostora za pohranu

## <a name="overview"></a>Pregled

U ovom ćete praktičnom vodiču će proširivanje aplikacije stvoren u [Node.js web-aplikacije pomoću Express] praktičnom vodiču pomoću sustava Microsoft Azure klijenta biblioteke za Node.js za rad sa servisa za upravljanje podacima. Aplikacije za stvaranje aplikacije utemeljene na web-popis zadataka koje možete implementirati za Azure će se proširiti. Na popisu zadataka korisnicima omogućuje dohvaćanje zadataka, dodajte nove zadatke i označite zadatke kao dovršeno.

Stavke zadataka spremaju se u Azure prostora za pohranu. Azure prostora za pohranu nudi nestrukturirane podatke prostora za pohranu pogreške i vrlo dostupna. Azure prostora za pohranu sadrži nekoliko strukture podataka, gdje možete spremiti i pristup podacima, a možete koristiti servise za pohranu API-ji obuhvaćeno Azure SDK Node.js ili putem REST API-ji. Dodatne informacije potražite u članku [Storing i pristup podacima u Azure].

Pomoću ovog praktičnog vodiča pretpostavlja da ste završili [Node.js web-aplikacije] i [Node.js s Express][Node.js web-aplikacije pomoću Express] vodiči.

Saznat ćete:

-   Upute za rad s modul Jade predloška
-   Kako funkcioniraju sa servisima za upravljanje podacima Azure

Snimka zaslona dovršene aplikacija je ispod:

![Dovršenom web-stranicu u pregledniku internet explorer](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Postavljanje vjerodajnica za pohranu u Web.Config

Da biste pristupili Azure prostora za pohranu, morate proći vjerodajnice za pohranu. Da biste to učinili, koristite postavke web.config aplikacije.
Te postavke će se proslijediti kao varijable okruženja čvor, koji se zatim pročitati Azure SDK.

> [AZURE.NOTE] Vjerodajnice za pohranu koriste samo kada je aplikacija implementiran na Azure. Kada se pokrene u na emulator, aplikacija će koristiti emulator prostora za pohranu.

Poduzmite sljedeće korake za dohvaćanje vjerodajnice za pohranu računa i njihovo dodavanje u postavkama web.config:

1.  Ako već nije otvorite, najprije Azure PowerShell s izbornika **Start** proširivanje **Svi programi, Azure**, desnom tipkom miša kliknite **Azure PowerShell**i odaberite **Pokreni kao Administrator**.

2.  Promijenite direktorija u mapu koja sadrži vaše aplikacije. Na primjer, C:\\čvor\\tasklist\\WebRole1.

3.  U prozoru Azure Powershell upišite sljedeći cmdlet za dohvaćanje podataka o računu za pohranu:

        PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts

    Dohvaća podatke na popisu računa za pohranu i račun tipke povezan sa servisom za glavnom računalu.

    > [AZURE.NOTE] Budući da Azure SDK stvara račun za pohranu kada Implementacija servisa, s računom za pohranu treba već postoji iz implementacija aplikacije u prethodno vodilice.

4.  Otvorite **ServiceDefinition.csdef** datoteku koja sadrži postavke okruženje koje koriste se za aplikaciju je implementiran na Azure:

        PS C:\node\tasklist> notepad ServiceDefinition.csdef

5.  Umetnite sljedeći blok u odjeljku element **okruženja** , zamjenjujući {prostora za POHRANU RAČUN} i {prostora za POHRANU TIPKOVNOG} s nazivom i primarni ključ za račun za pohranu koji želite koristiti za implementaciju:

        <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
        <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

    ![Sadržaj datoteke web.cloud.config](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6.  Datoteku spremite i zatvorite Blok za pisanje.

### <a name="install-additional-modules"></a>Instalacija dodatnih modula

2. Koristite sljedeću naredbu da biste instalirali [azure], [čvor-uuid], [nconf] i [asinkrone] module u lokalno i kao spremite unos za njih **package.json** datoteke:

        PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save

    Izlaz iz ta naredba trebao izgledati otprilike ovako:

        node-uuid@1.4.1 node_modules\node-uuid

        nconf@0.6.9 node_modules\nconf
        ├── ini@1.1.0
        ├── async@0.2.9
        └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

        azure-storage@0.1.0 node_modules\azure-storage
        ├── extend@1.2.1
        ├── xmlbuilder@0.4.3
        ├── mime@1.2.11
        ├── underscore@1.4.4
        ├── validator@3.1.0
        ├── node-uuid@1.4.1
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)

##<a name="using-the-table-service-in-a-node-application"></a>Putem servisa tablica u aplikaciji čvor

U ovom odjeljku će proširivanje osnovnih aplikacija stvorila naredbu **eksplicitnih** dodavanjem **task.js** datoteku koja sadrži modela za svoje zadatke. Ćete izmijeniti postojeću **app.js** i stvorite novu **tasklist.js** datoteku koja se koristi u modelu.

### <a name="create-the-model"></a>Stvaranje modela

1. U direktoriju **WebRole1** , stvorite novi direktorij pod nazivom **modela**.

2. U imeniku **modela** stvoriti novu datoteku pod nazivom **task.js**. Datoteka će sadržavati model za zadatke koji je stvorila vaša aplikacija.

3. Na početku **task.js** datoteku, dodajte sljedeći kod referentni potrebna biblioteka:

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. Nakon toga će dodati kod da biste definirali i izvoz objekt zadatka. Taj objekt je zadužen za povezivanje s tablicom.

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. Nakon toga dodati sljedeći kod da biste definirali dodatne načine na objekt zadatka koje omogućuju interakcija s podacima koji se pohranjuju u tablicu:

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };

            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. Spremite i zatvorite datoteku **task.js** .

### <a name="create-the-controller"></a>Stvaranje kontrolerom

1. U direktoriju **WebRole1/usmjerava** stvorite novu datoteku pod nazivom **tasklist.js** i otvorite je u uređivaču teksta.

2. Dodajte sljedeći kod **tasklist.js**. Time se učitava modula azure i asinkrone, koji se koriste u **tasklist.js**. To definira i **TaskList** funkciju, koja se prenosi instancu objekta **zadatka** definirali ranije:

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

        function TaskList(task) {
          this.task = task;
        }

2. Nastavite dodavati datoteku **tasklist.js** dodavanjem metode **showTasks**, **addTask**i **completeTasks**:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this      
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

3. Spremite datoteku **tasklist.js** .

### <a name="modify-appjs"></a>Izmjena app.js

1. U direktoriju **WebRole1** otvorite **app.js** datoteku u uređivaču teksta. 

2. Na početku datoteku, dodajte sljedeće da biste učitali modul azure i postavite ključ ime i particija tablice:

        var azure = require('azure-storage');
        var tableName = 'tasks';
        var partitionKey = 'hometasks';

3. U datoteci app.js, pomaknite se do kojem ćete vidjeti sljedeći redak:

        app.use('/', routes);
        app.use('/users', users);

    Zamijenite kôda prikazanog ispod gornje crte. To će pokrenuti instance komponente <strong>zadatka</strong> s vezom na račun servisa za pohranu. Time se prenosi u <strong>TaskList</strong>, koji će ga koristiti za komunikaciju sa servisom za tablicu:

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
    
4. Spremite datoteku **app.js** .

### <a name="modify-the-index-view"></a>Izmjena prikaza indeksa

1. Promjena direktorija u direktoriju **prikaza** i otvorite **index.jade** datoteku u uređivaču teksta.

2. Sadržaj datoteke **index.jade** zamijenite kod u nastavku. Ovim se definira prikaza za prikaz postojeće zadatke, kao i obrazac za dodjeljivati nove zadatke i označavanje postojeće kao dovršen.

        extends layout

        block content
          h1= title
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if tasks != []
                tr
                  td 
              else
                each task in tasks
                  tr
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name: 
            input(name="item[name]", type="textbox")
            label Item Category: 
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. Spremite i zatvorite datoteku **index.jade** .

### <a name="modify-the-global-layout"></a>Izmjena globalni rasporeda

Datoteka **layout.jade** u direktoriju **Prikazi** se koristi kao globalni predložak za ostale datoteke **.jade** . U ovom ćete koraku će se promijeniti koristiti [Twitteru samopokretanja programa](https://github.com/twbs/bootstrap)koji je alata koji olakšava dizajnirati bolje izgleda web-mjesto.

1. Preuzmite i izdvojiti datoteke za [Twitteru samopokretanja programa](http://getbootstrap.com/). Kopirajte datoteku **bootstrap.min.css** iz na **Samopokretanje\\dist\\CSS-a** mapu na **javno\\listovi stilova** imeničkog tasklist aplikacije.

2. Iz mape **prikaza** otvorite **layout.jade** u uređivaču teksta, a zatim zamijenite sadržaj sljedeće:

        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body.app
            nav.navbar.navbar-default
              div.navbar-header
                a.navbar-brand(href='/') My Tasks
            block content

3. Spremite datoteku **layout.jade** .

### <a name="running-the-application-in-the-emulator"></a>Pokretanje aplikacije u na Emulator

Da biste pokrenuli aplikaciju u na emulator, koristite sljedeću naredbu.

    PS C:\node\tasklist\WebRole1> start-azureemulator -launch

Web-pregledniku otvorit će se i prikazuje sljedeće stranice:

![Na web-mjestu Straničeno pod naslovom moj popis zadataka s tablicom koja sadrži zadatke i polja koja želite dodati novi zadatak.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

Dodavanje stavke pomoću obrasca ili uklonite postojeće stavke tako da ga označite dovršenim.

## <a name="publishing-the-application-to-azure"></a>Objavljivanje aplikacija za Azure


U prozoru komponente Windows PowerShell nazovite sljedeći cmdlet implementirati na glavnom računalu servisu za Azure.

    PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch

Zamijenite **myuniquename** jedinstveni naziv za ovu aplikaciju. **Datacentername** zamijenite nazivom centra Azure podataka, kao što su **Zapad SAD -a**.

Nakon dovršetka implementaciju, trebali biste vidjeti odgovor sličnu ovoj:

    PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
    WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
    WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
    WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
    WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
    WARNING: 2:19:01 PM - Connecting...
    WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
    WARNING: 2:19:40 PM - Upgrading...
    WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
    WARNING: 2:22:48 PM - Initializing...
    WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.

Što prije, jer ste naveli u **– pokretanje** mogućnosti web-pregledniku otvara i prikazuje aplikacija izvodi u Azure po dovršetku objavljivanje.

![Prikaz stranice moj popis zadataka prozoru preglednika. URL označava stranice se sada se hostira na Azure.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Zaustavljanje i brisanje aplikacije

Nakon što implementirate aplikacije, preporučujemo vam da biste onemogućili da biste mogli izbjegli troškove ili stvaranja i implementacija drugih aplikacija u besplatnu probnu vremenskom razdoblju.

Azure računi web-uloga instance po satu potrošena vrijeme na poslužitelju.
Vrijeme na poslužitelju potrošnje kada aplikacija je implementiran, čak i ako instance nije pokrenut i u prestao stanju.

Sljedeći koraci pokazati kako zaustaviti i brisanje aplikacije.

1.  U prozoru komponente Windows PowerShell Zaustavljanje servisa implementacije stvorili u prethodnom odjeljku s sljedeći cmdlet:

        PS C:\node\tasklist\WebRole1> Stop-AzureService

    Zaustavljanje servisa može potrajati nekoliko minuta. Kada servis je zaustavljen, dobit ćete poruku da je on prestao.

3.  Da biste izbrisali servisa, nazovite sljedeći cmdlet:

        PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist

    Kada se to od vas zatraži, unesite **Y** da biste izbrisali servis.

    Brisanje servis može potrajati nekoliko minuta. Nakon brisanja servis primit ćete poruku izbrisan je servis.

  [Node.js web-aplikacije pomoću Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
  [Spremanje i pristup podacima u Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Node.js web-aplikacije]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/
 
 
