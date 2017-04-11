<properties
    pageTitle="Lokalni aplikacijom blobova (Java) | Microsoft Azure"
    description="Saznajte kako stvoriti konzole za aplikaciju koja prenosi slike Azure, a zatim prikazuje sliku u pregledniku. Primjere koda u Java"
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="on-premises-application-with-blob-storage"></a>Lokalni aplikacijom spremište blobova platforme

## <a name="overview"></a>Pregled

Sljedeći primjer pokazuje kako koristiti Azure prostora za pohranu za pohranu slika u Azure. Kod u ovom članku je za konzole za aplikaciju koja prenosi slike Azure i stvara HTML datoteku koja se prikazuje sliku u pregledniku.

## <a name="prerequisites"></a>Preduvjeti

- Na Java za razvojne inženjere Kit (JDK), 1,6 ili novijem, nije instaliran.
- Azure SDK nije instaliran.
- POSUDU za Azure biblioteke Java i staklenke sve odgovarajuće zavisnost su instalirani i put Sastavi koristi alata za Kompiliranje programskog jezika Java se nalazite. Informacije o instaliranju Azure biblioteka za Java potražite u članku [Preuzimanje Azure SDK za Java](java-download-azure-sdk.md).
- Račun za Azure prostora za pohranu je postavljen. Naziv računa i ključ računa za račun za pohranu koristit će kod u ovom članku. Informacije o stvaranju računa za pohranu i [Prikaz i kopiranje prostora za pohranu pristupnih tipki](storage-create-storage-account.md#view-and-copy-storage-access-keys) informacije o dohvaćanju ključ računa potražite u članku [upute za stvaranje računa za pohranu](storage-create-storage-account.md#create-a-storage-account) .

- Stvorite lokalnu slika datoteku pod nazivom spremljenu na put c:\\myimages\\image1.jpg. Umjesto toga možete izmijeniti Graditelj   **FileInputStream** u primjeru da biste koristili na drugom slikom put i naziv datoteke.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Da biste koristili blobova platforme Azure da biste prenijeli datoteke

Detaljni opis postupka raspoređene su u nastavku. Ako želite preskočiti unaprijed, cijeli kod raspoređene su u nastavku ovog članka.

Započnite kod uključivanjem uvozi za pohranu klase Azure core, klijent klase blobova platforme Azure, klase jezika Java IO i **URISyntaxException** predmete.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

Deklariranje klase pod nazivom **StorageSample**i uvrstite otvorene zagrade, **{**.

    public class StorageSample {

Unutar klase **StorageSample** deklarirati varijablu niza koja će sadržavati zadani protokol za krajnje točke, naziv računa spremišta i pristup ključ za pohranu, kao što je navedeno na vašem računu Azure prostora za pohranu. Zamjena vrijednosti rezervirano mjesto **vaše\_račun\_naziv** i **vaše\_račun\_ključ** s vlastitim računa imenom i računom ključ, odnosno.

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

Dodatka vaše deklariranje za **glavne**, uključiti blok **pokušajte** i obuhvaćaju potrebne otvorene zagrade, **{**.

    public static void main(String[] args)
    {
        try
        {

Deklariranje varijable vrste sljedeće (opise su za način na koji se koriste u ovom primjeru):

-   **CloudStorageAccount**: koristi za inicijalizaciju objekta poslovnog subjekta s naziv računa Azure prostora za pohranu i ključ, a da biste stvorili blob klijentskog objekta.
-   **CloudBlobClient**: služi za pristup servisu blob.
-   **CloudBlobContainer**: koristi za stvaranje spremnika blob, popis blob-ova u spremniku te izbrišite.
-   **CloudBlockBlob**: prenesene datoteke lokalne slike u spremniku.

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

Dodjeljuje se vrijednost varijabli **računa** .

    account = CloudStorageAccount.parse(storageConnectionString);

Dodjeljuje se vrijednost **serviceClient** varijabli.

    serviceClient = account.createCloudBlobClient();

**Spremnik** varijabli dodjeljuje se vrijednost. Ne možemo će se referenca kontejner **gettingstarted**.

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

Stvaranje kontejner. Ova metoda će stvoriti spremnik ako ga ne postoji (i vratiti **true**). Spremnik postojanje će vratiti **false**. Alternative **createIfNotExists** je metoda **Stvaranje** (koji će vratiti pogrešku ako već postoji spremnik).

    container.createIfNotExists();

Postavite anonimni pristup za spremnik.

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

Se referenca blob bloka koja će predstavljati blobova platforme Azure pohrane u.

    blob = container.getBlockBlobReference("image1.jpg");

Reference na lokalno stvoreni datoteke koju ćete prenijeti pomoću Graditelj **datoteke** . Provjerite je li stvarate datoteku prije pokretanja kod.

    File fileReference = new File ("c:\\myimages\\image1.jpg");

Prenesite lokalne datoteke putem poziva **CloudBlockBlob.upload** korakom. Prvi parametar metodu **CloudBlockBlob.upload** je **FileInputStream** objekt koji predstavlja lokalna datoteka koja će se prenijeti u Azure prostora za pohranu. Drugi parametar je veličina u bajtovima datoteke.

    blob.upload(new FileInputStream(fileReference), fileReference.length());

Pozivanje funkcije preglednika pod nazivom **MakeHTMLPage**, da biste HTML osnovnu stranicu koja sadrži programa ** &lt;slika&gt; ** element s izvorišnog web-mjesta postavljena na blob u kojem se sada je na vašem računu Azure prostora za pohranu. Kod za **MakeHTMLPage** će biti navedene u nastavku ovog članka.

    MakeHTMLPage(container);

Ispišite poruku o statusu i informacije o stvorenu HTML stranicu.

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

Zatvorite blok **pokušajte** tako da umetnete završna uglata zagrada: **}**

Rukovanje sljedeće iznimke:

-   **FileNotFoundException**: možete je izbacio constructors **FileInputStream** ili **FileOutputStream** .
-   **StorageException**: možete je izbacio Azure klijentska biblioteka za pohranu.
-   **URISyntaxException**: možete izbačena metodom **ListBlobItem.getUri** .
-   **Iznimke**: rukovanje generički iznimke.

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Zatvorite **glavni** tako da umetnete završna uglata zagrada: **}**

Stvaranje metode pod nazivom **MakeHTMLPage** da biste stvorili HTML osnovnu stranicu. Ta metoda ima parametar vrste **CloudBlobContainer**, koji će se koristiti iteracija po popisu prenesenih blob-ova. Ova metoda će vratiti iznimke vrste **FileNotFoundException**, koji možete izbačena Graditelj **FileOutputStream** i **URISyntaxException**, koji mogu biti izbačena metodom **ListBlobItem.getUri** . Lijeve uglate zagrade, **{**uključiti.

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

Stvaranje lokalne datoteke naziva **index.html**.

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

Pisanje lokalnu datoteku dodate u u ** &lt;html&gt;**, ** &lt;zaglavlje&gt;**, i ** &lt;tijelo&gt; ** elemente.

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

Iteracija po popisu prenesenih blob-ova. Za svaki blob u HTML stranicu Stvaranje programa ** &lt;img&gt; ** element koji je njegova **src** atribut slati URI blob-om kao ona se nalazi na vašem računu Azure prostora za pohranu.
Iako samo jedne slike dodane u ovom primjeru, ako ste dodali više, kod bi iteracija ih sve.

Zbog jednostavnosti, pretpostavlja se svaki prenesene blob je slika. Ako ste ažurirali blob-ova umjesto slike ili blob-Ova stranica umjesto bloka blob-ova, po potrebi prilagodite kod.

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

Zatvaranje u ** &lt;tijelo&gt; ** element i ** &lt;html&gt; ** element.

    stream.println("</body>");
    stream.println("</html>");

Zatvorite lokalne datoteke.

    stream.close();

Zatvorite **MakeHTMLPage** tako da umetnete završna uglata zagrada: **}**

Zatvorite **StorageSample** tako da umetnete završna uglata zagrada: **}**

Slijedi cjelovit kod u ovom primjeru. Imajte na umu da biste izmijenili vrijednosti rezervirano mjesto **vaše\_račun\_naziv** i **vaše\_račun\_ključ** da biste koristili naziv računa i ključ za račun.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

Osim prenijeti datoteku lokalne slika Azure prostora za pohranu, kod primjer stvara namedindex.html lokalne datoteke, koju možete otvoriti u pregledniku da biste pogledali prenesene sliku.

Budući da kod sadrži naziv računa i ključ računa, provjerite je li izvorni kod siguran.

## <a name="to-delete-a-container"></a>Da biste izbrisali spremnik

Budući da vam se naplatiti za pohranu, preporučujemo vam da biste izbrisali spremnik **gettingstarted** nakon što završite eksperimentiranja s ovom primjeru. Da biste izbrisali spremniku, pomoću metode **CloudBlobContainer.delete** .

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

Da biste nazvali metodu **CloudBlobContainer.delete** , pokretanje **CloudStorageAccount**, **ClodBlobClient**i **CloudBlobContainer** objekata postupak je isti kao što je prikazano za metodu **createIfNotExist** . U nastavku je dovršena primjer u kojem se briše kontejner **gettingstarted**.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

Pregled drugih klase spremište blobova platforme i načine, potražite u članku [upute za korištenje spremišta blobova iz Java](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Daljnji koraci

Slijedite ove veze da biste saznali više o zadacima složenije prostora za pohranu.

- [Azure prostora za pohranu SDK Java](https://github.com/azure/azure-storage-java)
- [Azure prostora za pohranu klijenta SDK Reference](http://dl.windowsazure.com/storage/javadoc/)
- [Servise za pohranu Azure REST API-JA](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Blog tima za Azure prostora za pohranu](http://blogs.msdn.com/b/windowsazurestorage/)
