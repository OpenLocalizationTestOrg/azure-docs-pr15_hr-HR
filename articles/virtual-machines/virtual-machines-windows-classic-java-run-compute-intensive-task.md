<properties
    pageTitle="Računalnim ćete morati usko Java aplikaciju na na VM | Microsoft Azure"
    description="Saznajte kako stvoriti Azure virtualnog računala koja se pokreće aplikacije Java računalnim ćete morati usko koje je moguće nadzirati neki drugi program Java."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Kako pokrenuti računalnim ćete morati usko zadatka u Java virtualnog računala

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

S Azure, virtualnog računala možete koristiti za rukovanje računalnim ćete morati usko zadatke. Virtualnog računala, na primjer, možete rukovati zadaci i izlaganje rezultate klijentskim računalima ili mobilne aplikacije. Kad pročitate članak, morat ćete poznavati kako stvoriti virtualnog računala koja se pokreće aplikacije Java računalnim ćete morati usko koje je moguće nadzirati neki drugi program Java.

Pomoću ovog praktičnog vodiča pretpostavlja znati kako stvoriti Java konzola aplikacije, možete uvesti biblioteke u aplikaciji Java i možete generirati Java arhiva (POSUDU). Poznavanje Microsoft Azure pretpostavlja se da je.

Saznat ćete:

* Upute za stvaranje virtualnog računala s na Java Development Kit (JDK) već instaliran.
* Upute za daljinsko se prijaviti na virtualnog računala.
* Kako stvoriti polje naziva za bus servisa.
* Upute za stvaranje Java aplikacije koja izvršava računalnim ćete morati usko zadatka.
* Upute za stvaranje Java aplikacije koji nadzire tijek zadatka računalnim ćete morati usko.
* Upute za izvođenje aplikacije Java.
* Kako spriječiti Java aplikacije.

Pomoću ovog praktičnog vodiča koristit će Problem Traveling Salesman računalnim ćete morati usko zadatka. Slijedi primjer Java aplikacije koje se izvode računalnim ćete morati usko zadatka.

![Alat za rješavanje problema s Traveling Salesman][solver_output]

Slijedi primjer aplikacije Java nadzor računalnim ćete morati usko zadatka.

![Problem Traveling Salesman klijenta][client_output]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Da biste stvorili virtualnog računala

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com).
2. Kliknite **Novo**, kliknite **izračunati**, kliknite **virtualnog računala**, a zatim **Iz galerije**.
3. U dijaloškom okviru **Odabir slike virtualnog računala** odaberite **JDK 7 Windows Server 2012**.
Imajte na umu da je **JDK 6 Windows Server 2012** dostupna, u slučaju da imate stari aplikacije koji još nisu spremni pokrenuti u JDK 7.
4. Kliknite **Dalje**.
4. U dijaloškom okviru **Konfiguriranje virtualnog računala** :
    1. Navedite naziv virtualnog računala.
    2. Odredite veličinu da biste koristili virtualnog računala.
    3. U polje **Korisničko ime** unesite naziv za administratora. Imajte na umu ovo ime i lozinku ćete unijeti sljedeće, će ih koristiti kad daljinski prijaviti na virtualnog računala.
    4. Unesite lozinku u polju **novu lozinku** , a zatim ponovno unesite u polje **Potvrdi** . Ovo je administratorsku lozinku za račun.
    5. Kliknite **Dalje**.
5. U sljedećem **Konfiguracija virtualnog računala** dijaloškom okviru:
    1. Za **servis u Oblaku**, koristite zadani **Stvori novi servis u oblaku**.
    2. Vrijednost za **naziv DNS servis oblaka** mora biti jedinstvena preko cloudapp.net. Ako je potrebno, tu vrijednost promijeniti tako da se taj Azure označava jedinstveni.
    2. Navedite regija, afinitet grupe ili virtualne mreže. Za potrebe ovog praktičnog vodiča, navedite regiju kao što su **Zapad SAD -a**.
    2. **Račun za pohranu**, odaberite **Koristi račun automatski generirani prostora za pohranu**.
    3. **Postavljanje dostupnosti**, odaberite **(ništa)**.
    4. Kliknite **Dalje**.
5. U konačnih dijaloški okvir **Konfiguracija virtualnog računala** :
    1. Prihvatite zadanih krajnje točke unosa.
    2. Kliknite **dovrši**.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>Daljinsko se prijaviti na virtualnog računala

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com).
2. Kliknite **virtualnih računala**.
3. Kliknite naziv virtualnog računala koje želite prijaviti na.
4. Kliknite **Poveži**.
5. Odgovaranje na upite po potrebi možete povezati virtualnog računala. Kada se zatraži administrator ime i lozinku, koristiti vrijednosti koju ste naveli prilikom stvaranja virtualnog računala.

Imajte na umu Bus servisa Azure funkcije potrebna je Zagrebu CyberTrust korijenski certifikat da biste se instalira u sklopu vaše JRE **cacerts** spremišta. Taj certifikat se automatski uključuje u na Java Runtime okruženje (JRE) koristi ovog praktičnog vodiča. Ako nemate certifikat u spremištu **cacerts** JRE, u odjeljku [Dodavanje certifikat za pohranu certifikat za Java CA] [ add_ca_cert] dodatne informacije o dodavanju ga (kao i informacije o prikazu potvrde u spremištu cacerts).

## <a name="how-to-create-a-service-bus-namespace"></a>Kako stvoriti polje naziva za bus servisa

Da biste počeli koristiti servis Bus redova u Azure, prvo morate stvoriti polje naziva servisa. Polje naziva servisa omogućuje scoping spremnik adresiranja resursima servisa Bus unutar vaše aplikacije.

Da biste stvorili polje naziva servisa:

1.  Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com).
2.  U donjem lijevom navigacijskom oknu portala za Azure klasični kliknite **servis Bus, kontrola pristupa i Međuspremanje**.
3.  U oknu gornji lijevi portala za Azure klasični kliknite čvor **Bus servisa** , a zatim gumb **Novo** .  
    ![Snimka zaslona servisa Bus čvor][svc_bus_node]
4.  U dijaloškom okviru **Stvaranje nove polje naziva servisa** unesite **prostor za naziv**, a zatim da biste bili sigurni da je jedinstveni, kliknite gumb **Provjeri dostupnost** .  
    ![Stvorite novi prostor za naziv snimke zaslona][create_namespace]
5.  Kada provjerite polja naziva nije dostupan, odaberite državu ili regiju u kojoj vaš prostor naziva moraju se nalaziti, a zatim kliknite gumb **Stvori prostor naziva** .  

    Prostor za naziv koji ste stvorili će se pojaviti na portalu za Azure klasični i traje trenutak da biste aktivirali. Pričekajte dok se status **aktivan** prije nego što nastavite na sljedeći korak.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>Pribavljanje upravljanje vjerodajnica zadano za prostor za naziv

Da bi se izvođenje operacija upravljanja, kao što su stvaranje reda, na novi prostor za naziv, morate dobiti upravljanje vjerodajnice za naziva.

1.  U lijevom navigacijskom oknu kliknite čvor **Bus servisa** da biste prikazali popis dostupnih prostorima naziva.
    ![Snimka zaslona za dostupna prostora naziva][avail_namespaces]
2.  Odaberite prostora za naziv koji ste upravo stvorili s popisa koji se prikazuju.
    ![Snimka zaslona prostora za naziv popisa][namespace_list]
3.  U desnom oknu **Svojstva** navodi svojstva za novi prostor za naziv.
    ![Snimka zaslona okna svojstva][properties_pane]
4.  **Zadani ključ** je skriven. Kliknite gumb za **Prikaz** da biste prikazali sigurnosne vjerodajnice.
    ![Snimka zaslona zadani ključ][default_key]
5.  Zabilježite **Zadani izdavač** i **Zadani ključ** jer će vam ove informacije u nastavku za izvođenje operacija s prostorom.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Kako stvoriti Java aplikacije koja izvršava računalnim ćete morati usko zadatka

1. Na računalu razvoj (koji ne moraju biti virtualnog računala koju ste stvorili), preuzmite [Azure SDK za Java](https://azure.microsoft.com/develop/java/).
2. Stvaranje aplikacije konzole Java pomoću koda primjer na kraju ovaj odjeljak. U ovom ćete praktičnom vodiču ćemo koristiti **TSPSolver.java** kao Java naziv datoteke. Izmjena na **vaše\_servisa\_bus\_prostor naziva**, **vaše\_servisa\_bus\_vlasnik**, i **vaše\_servisa\_bus\_ključ** rezerviranih mjesta za korištenje na servisu sabirnice **prostor naziva**, **Zadani izdavač** i **Ključ zadane** vrijednosti, odnosno.
3. Nakon kodiranje, izvoz aplikacije runnable Java arhivu (POSUDU), a pakiranje potrebne biblioteke u generirani POSUDU. U ovom ćete praktičnom vodiču ćemo koristiti **TSPSolver.jar** kao generirani POSUDU naziv.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>Kako stvoriti Java program koji nadzire tijek zadatka računalnim ćete morati usko

1. Na računalu razvoj stvaranje aplikacije konzole Java pomoću koda primjer na kraju ovaj odjeljak. U ovom ćete praktičnom vodiču ćemo koristiti **TSPClient.java** kao Java naziv datoteke. Kao što je prikazano ranije izmjena na **vaše\_servisa\_bus\_prostor naziva**, **vaše\_servisa\_bus\_vlasnik**, i **vaše\_servisa\_bus\_ključ** rezerviranih mjesta za korištenje na servisu sabirnice **prostor naziva**, **Zadani izdavač** i **Ključ zadane** vrijednosti, odnosno.
2. Izvezite aplikacije runnable POSUDU i pakiranje potrebne biblioteke u generirani POSUDU. U ovom ćete praktičnom vodiču ćemo koristiti **TSPClient.jar** kao generirani POSUDU naziv.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Pokretanje aplikacije Java
Pokrenite aplikaciju računalnim ćete morati usko prvi put da biste stvorili red, zatim da biste riješili Problem za putovanje Saleseman što će dodati trenutni najbolje usmjeravanje bus čekanja za servis. Dok je aplikacija računalnim ćete morati usko pokrenuti (ili naknadno), pokrenite klijent za prikaz rezultata iz servisa bus reda.

### <a name="to-run-the-compute-intensive-application"></a>Da biste pokrenuli aplikaciju računalnim ćete morati usko

1. Prijavite se na virtualnog računala.
2. Stvorite mapu gdje će pokrenuti aplikaciju. Na primjer, **c:\TSP**.
3. Kopiranje **TSPSolver.jar** **c:\TSP**
4. Stvorite datoteku pod nazivom **c:\TSP\cities.txt** sljedeće sadržajem.

        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33

5. U naredbeni redak, promijenite direktorija c:\TSP.
6. Provjerite je li u JRE smeće mape u varijablu okruženja put.
7. Morat ćete stvoriti red čekanja bus servisu prije nego što pokrenete alat za rješavanje permutacija TSP. Pokrenite sljedeću naredbu da biste stvorili bus red servisa.

        java -jar TSPSolver.jar createqueue

8. Sad kad reda čekanja je stvorili, možete pokrenuti permutacija alata za rješavanje TSP. Na primjer, pokrenite sljedeću naredbu da biste pokrenuli alat za rješavanje 8 gradova.

        java -jar TSPSolver.jar 8

 Ako ne navedete broj, pokrenut će 10 gradova. Kao što je alat za rješavanje pronađe trenutno pretraživanje po najkraćoj usmjerava, će ih dodati u red.

> [AZURE.NOTE]
> Veći broj koji navedete, dulje će pokrenuti alat za rješavanje. Na primjer, radi 14 gradova može potrajati nekoliko minuta i pokretanje 15 gradova može potrajati nekoliko sati. Povećanje 16 ili više gradova može rezultirati dana izvođenja (naposljetku tjedna, mjeseci i godina). To je zbog brzog povećava broj permutacija za alat za rješavanje procijenio kao broj gradova povećava.

### <a name="how-to-run-the-monitoring-client-application"></a>Pokretanje nadzora klijentska aplikacija
1. Prijavite se na vaše računalo na kojem će pokrenuti klijentske aplikacije. To ne moraju biti iste računalu s instaliranim programom aplikacije **TSPSolver** iako može biti.
2. Stvorite mapu gdje će pokrenuti aplikaciju. Na primjer, **c:\TSP**.
3. Kopiranje **TSPClient.jar** **c:\TSP**
4. Provjerite je li u JRE smeće mape u varijablu okruženja put.
5. U naredbeni redak, promijenite direktorija c:\TSP.
6. Pokrenite sljedeću naredbu.

        java -jar TSPClient.jar

    Možete i navesti broj minuta u stanje mirovanja između Provjera reda po Prenos u argumenta naredbenog retka. Zadano razdoblje mirovanja za provjeru reda čekanja je 3 minute koja se koristi ako nema argumenta naredbenog retka se prenosi **TSPClient**. Ako želite koristiti neku drugu vrijednost za interval mirovanja, na primjer, jednu minutu, pokrenite sljedeću naredbu.

        java -jar TSPClient.jar 1

    Klijent će se izvoditi dok ne vidi reda čekanja poruka "Dovršeno". Imajte na umu da pokrenete većeg broja pojavljivanja alat za rješavanje bez izvođenja klijentskog programa, možda ćete morati pokrenuti klijent više puta da biste potpuno ispraznili reda. Umjesto toga, možete izbrisati reda čekanja i ponovno ga stvoriti. Da biste izbrisali red, pokrenite sljedeću naredbu **TSPSolver** (ne **TSPClient**).

        java -jar TSPSolver.jar deletequeue

    Alat za rješavanje će se izvoditi dok ne završi Provjera svi putovi.

## <a name="how-to-stop-the-java-applications"></a>Kako spriječiti aplikacije Java
Za rješavanje i klijentske aplikacije, možete pritisnuti **Ctrl + C** da biste izašli iz ako želite da biste završili prije normalni dovršenog.


[solver_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../java-add-certificate-ca-store.md
