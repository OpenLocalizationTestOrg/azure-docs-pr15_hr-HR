<properties
    pageTitle="Serijalizirati podataka pomoću programa Microsoft Avro Library | Microsoft Azure"
    description="Saznajte kako Azure HDInsight pomoću Avro serijalizirati velikih skupova podataka."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Serijalizirati podataka u Hadoop s Microsoft Avro Library

Ova tema prikazuje kako koristiti <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro biblioteke</a> da bi ih zadržava memorije, baze podataka ili datoteke u strujanja serijalizirati objekte i druge strukture podataka te kako ukloniti ih da biste vratili izvorne objekte serijski broj.

[AZURE.INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

##<a name="apache-avro"></a>Apache Avro
<a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">U biblioteci Microsoft Avro</a> implementira Apache Avro podatkovnom sustavu serijalizacije Microsoft.NET okruženju. Apache Avro nudi oblik za razmjenu sažetom binarne podatke za serijalizacije. <a href="http://www.json.org" target="_blank">JSON</a> koristi za određivanje jezika agnostic sheme underwrites interoperabilnost jezik. U drugom je moguće čitati podataka serijalizirani na jednom jeziku. Trenutno C, C++, C#, Java, PHP, Python i Ruby podržani su. Detaljne informacije o obliku pronaći ćete u <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro specifikacija</a>. Imajte na umu da trenutnu verziju programa Microsoft Avro Library ne podržava dio udaljene procedure poziva (RPC-a) u ovom specifikacija.

Serijalizirani predstavljanje objekta u sustavu Avro sastoji se od dva dijela: sheme i stvarna vrijednost. Shema Avro opisuje jezik neovisno podatkovnog modela serijaliziranog podataka s JSON. To je izlaganje--usporedno s binarni prikaz podataka. Imate shemi neovisan binarni prikaz omogućuje svaki objekt pisana bez folije po vrijednost upućivanje small serijalizacije brza i prikaz.

##<a name="the-hadoop-scenario"></a>Scenarij Hadoop
Serijaliziranog oblika Apache Avro široko koristi web-mjesto servisa Azure HDInsight i u okvir za druge Apache Hadoop okruženju. Avro pruža jednostavan način za predstavljanje strukture složenih podataka unutar Hadoop MapReduce posao. Oblik datoteke Avro (Avro objekt spremnik datoteka) dizajniran tako da podržava raspodijeljeno model programiranja MapReduce. Ključna značajka koja omogućuje raspodjele je jesu li datoteke "splittable" u smislu da jedan možete traženje bilo koje mjesto u datoteci i pokretanje čitanje određeni blok.

##<a name="serialization-in-avro-library"></a>Serijalizacije u biblioteci Avro
Biblioteka .NET za Avro podržava dva načina Serijalizacija objekata:

- **Odraz** - u JSON sheme za vrste automatski ugrađen iz podataka ugovora atributa vrste .NET da biste se serijalizirati.
- **generički zapis** – A JSON sheme izričito naveden u zapisu predstavlja klase [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) kada nijedna vrsta .NET postoje opisuje sheme za podatke koje želite da se serijalizirati.

Ako podatkovnu shemu poznato je da writer i čitač toka, podatke možete biti poslan bez shemom. U slučajevima kada se koristi Avro spremnik objektna datoteka, shema pohranjen unutar datoteke. Možete navesti drugi parametri, kao što je kodek koristiti za spajanja podataka. Sljedećim scenarijima su navedene u više pojedinosti i podržali kod primjere u nastavku.


## <a name="install-avro-library"></a>Instalacija Avro biblioteke

Ovo su potrebne prije instalacije u biblioteku:

- <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a>
- <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 ili noviji)

Imajte na umu ovisnost Newtonsoft.Json.dll automatski preuzimaju s instalacijom programa Microsoft Avro Library. Postupak za to je ugrađena u sljedećem odjeljku.


Microsoft Avro Library distribuira kao NuGet paket koji se može instalirati s Visual Studio putem sljedećeg postupka:

1. Odaberite karticu **projekta** -> **Upravljanje NuGet paketa...**
2. Traženje "Microsoft.Hadoop.Avro" u okvir za **Pretraživanje Online** .
3. Kliknite gumb **instalirati** uz **Microsoft Azure HDInsight Avro biblioteke**.

Imajte na umu da se Newtonsoft.Json.dll (> = 6.0.4) ovisnost se automatski preuzimaju s Microsoft Avro Library.

Preporučujemo vam da <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">u biblioteci Microsoft Avro Početna stranica</a> da biste pročitali trenutni napomene.


Izvorni kod Microsoft Avro Library dostupna je na <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro biblioteka početnu stranicu</a>.

##<a name="compile-schemas-using-avro-library"></a>Kompiliranje sheme pomoću Avro biblioteke

Microsoft Avro Library sadrži utility za generiranje koda koji omogućuje stvaranje C# vrsta automatski na temelju prethodno definirane sheme JSON. Za generiranje koda ne distribuirati kao binarni izvršnu datoteku, ali možete jednostavno ugrađeni putem sljedećeg postupka:

1. Preuzmite datoteku .zip najnoviju verziju preglednika HDInsight SDK izvornog koda iz <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK Hadoop</a>. (Kliknite **Preuzimanje** ikonu, a ne **preuzima** karticu).

2. Izdvajanje SDK HDInsight direktorij na računalu s .NET Framework 4 instaliran i povezani s Internetom za preuzimanje paketa NuGet potrebne ovisnosti. U nastavku će pretpostavkom da je izdvojene izvornog koda C:\SDK.

3. Otvorite mapu C:\SDK\src\Microsoft.Hadoop.Avro.Tools i pokrenite build.bat. (Datoteke će pozivom MSBuild distribucija 32-bitni .NET Framework. Ako želite da biste koristili 64-bitnu verziju, uredite build.bat, pratiti komentare u datoteci.) Provjerite je li sastavljanje uspješno. (U nekim sustavima MSBuild može dati upozorenja. Ova upozorenja ne utječe na uslužni pod uvjetom da nema li pogrešaka Sastavi.)

4. Prevedeni program nalazi se u C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.


Da biste se upoznali Sintaksa naredbenog retka, pokrenite sljedeću naredbu iz mape u kojoj se nalazi program za generiranje koda:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

Da biste testirali uslužni, možete generirati C# klase iz datoteke sheme JSON uzorka dao izvornog koda. Pokrenite sljedeću naredbu:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

To je potrebno proizvesti dva C# datoteke u trenutnom direktoriju: SensorData.cs i Location.cs.

Da biste shvatili logiku koja se koristi za generiranje koda pretvaranju shemi JSON C# vrste, potražite u članku datoteke GenerationVerification.feature koja se nalazi u C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.

Napominjemo da se prostori naziva dobivenih iz shemi JSON pomoću logike opisane u datoteku navedenu u prethodni odlomak. Prostori naziva dobivenih iz sheme prednost pred ono navedeni su s parametrom /n u naredbenom retku utility. Ako želite odbaciti prostori koji se nalaze u shemi koristiti parametar /nf. Na primjer, da biste promijenili svi se prostori naziva u SampleJSONSchema.avsc my.own.nspace, pokrenite sljedeću naredbu:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="samples"></a>Uzorci
Šest navedene u ovoj temi Primjeri scenarija podržava Microsoft Avro Library. Microsoft Avro Library osmišljena je za rad s bilo kojeg strujanje. U ovim se primjerima podataka je podešavati putem strujanja memorije umjesto strujanja datoteke ili baze podataka za jednostavnosti i dosljednost. Pristup u okruženju proizvodnje ovisi o točno scenarij preduvjeti, izvora podataka i glasnoća, ograničenja performanse i ostali čimbenici.

Prva dva primjeri pokazuju kako Serijalizacija i ukloniti serijski broj podataka u memoriji strujanje međuspremnika pomoću odraz i generički zapisa. Shema u tim slučajevima dva pretpostavlja se da je zajednički koristiti čitatelje i autorima do dospijeća.

Treći i četvrti primjeri pokazuju kako Serijalizacija i ukloniti serijski broj podataka pomoću datoteke spremnik Avro objekt. Kada se podaci se pohranjuju u datoteci Avro spremnik, shemom uvijek se sprema s njim jer shemi moraju biti zajedničke za deserijalizacija.

Uzorak koji sadrži prva četiri primjera mogu se preuzeti sa <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-86055923" target="_blank">primjere Azure koda</a> web-mjesta.

Peti primjer pokazuje kako kako koristiti prilagođene sažimanja kodek za Avro objekt spremnik datoteke. Uzorka koji sadrže kod u ovom primjeru mogu se preuzeti s web-mjesta <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111" target="_blank">primjere Azure koda</a> .

Šestog uzorka pokazuje kako koristiti serijalizacije Avro za prijenos podataka u spremište blobova platforme Azure, a zatim je analizirati pomoću grozd programa klaster HDInsight (Hadoop). Mogu se preuzeti s web-mjesta <a href="https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3" target="_blank">primjere Azure koda</a> .

Evo veze na šest uzoraka koji se spominju u temi:

 * <a href="#Scenario1">**Serijalizacije programa Excel**</a> – u JSON sheme za vrste da biste se serijalizirati automatski ugrađen iz podataka atribute ugovora.
 * <a href="#Scenario2">**Serijalizacije generički zapisu**</a> - u JSON sheme izričito naveden u zapisu kada nijedna vrsta .NET dostupna je za odraza.
 * <a href="#Scenario3">**Serijalizacije korištenje objekta spremnik datoteka programa Excel**</a> – u JSON sheme automatski ugrađena i zajednički se koristi s serijaliziranog podataka putem Avro spremnik objektna datoteka.
 * <a href="#Scenario4">**Serijalizacije pomoću objekta spremnik datoteke sa zapisom generički**</a> - u JSON sheme je izričito navedeno prije na serijalizacije i zajednički koristiti zajedno s podataka putem Avro spremnik objektna datoteka.
 * <a href="#Scenario5">**Serijalizacije korištenje objekta spremnik datoteka pomoću kodeka prilagođene sažimanje**</a> – na primjer pokazuje kako stvoriti datoteku spremnik Avro objekt s prilagođene implementaciju .NET kodeka za sažimanje podataka Deflate.
 * <a href="#Scenario6">**Korištenje Avro prijenos podataka za servis Microsoft Azure HDInsight**</a> – na primjer ilustrira interakciju serijalizacije Avro sa servisom HDInsight. Da biste pokrenuli ovaj primjer moraju aktivnu pretplatu Azure i pristup programa klaster Azure HDInsight.

###<a name="Scenario1"></a>Primjer 1: Serijalizacije programa Excel

Sheme JSON vrsta možete se automatski ugrađeni tako da Microsoft Library Avro putem odraza iz podataka ugovora atribute objekata C# da biste se serijalizirati. Microsoft Avro Library stvara sustava [**IAvroSeralizer<T> **](http://msdn.microsoft.com/library/dn627341.aspx) za prepoznavanje polja da biste se serijalizirati.

U ovom primjeru objekte (klase **SensorData** s struct za **mjesto** član) su serijalizirani memorije strujanje, a ovaj tok shodno deserijalizirati. Rezultat zatim uspoređuje početne instancu da biste provjerili je li objekt **SensorData** oporaviti identične u izvorno stanje.

Shema u ovom primjeru pretpostavlja se da je zajednički koristiti čitatelje i autorima, tako da se oblik spremnika Avro objekt nije obavezno. Primjer Serijalizacija i ukloniti serijski broj podataka u memorije međuspremnika pomoću odraz spremnik oblikovanje objekta kada je shema mora biti zajedničko korištenje s podacima, potražite u članku <a href="#Scenario3">serijalizacije korištenje objekta spremnik datoteka programa Excel</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-2-serialization-with-a-generic-record"></a>Primjer 2: Serijalizacije sa zapisom generički

Shemu JSON moguće izričito navesti u zapisu generički kada odraz nije moguće koristiti jer podataka nije moguće prikazati putem .NET klase podataka ugovora. Ta je metoda obično manja od zrcaljenjem. U tim slučajevima sheme za podatke možda će vas dinamički, odnosno nije poznata Kompiliranje vrijeme. Predstavljen kao datoteke vrijednosti odvojenih zarezom (CSV) čije shemu nije poznat dok je oblik Avro transformacije vrijeme izvođenja podaci primjera ovu vrstu dinamički scenarija.

Ovaj primjer pokazuje kako stvoriti i pomoću [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) izričito navedite shemu JSON, kako popuniti s podacima, a zatim serijalizirati i ukloniti ga serijski broj. Rezultat zatim uspoređuje početne instancu da biste provjerili je li zapis oporaviti identične u izvorno stanje.

Shema u ovom primjeru pretpostavlja se da je zajednički koristiti čitatelje i autorima, tako da se oblik spremnika Avro objekt nije obavezno. Primjer Serijalizacija i ukloniti serijski broj podataka u memorije međuspremnika pomoću generički zapis spremnik oblikovanje objekta kada shemi mora biti uključeno serijaliziranog podacima, potražite u članku primjer <a href="#Scenario4">serijalizacije pomoću objekta spremnik datoteke sa zapisom generički</a> .


    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn will be then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Primjer 3: Serijalizacije objekt spremnik datoteke i serijalizacije pomoću odraza

U ovom se primjeru slično je scenarija u <a href="#Scenario1">prvom primjeru</a>, kojoj se u shemi implicitno navedeni programa Excel. Razlika koje je ovdje, shemu nije pretpostavlja da poznate čitača koji je deserializes. Objekata **SensorData** da biste se serijalizirati i njihove implicitno određenom shemom spremaju se u datoteci Avro objekt spremnik predstavlja klase [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) .

Podaci Serijalizirano je u ovom primjeru s [**SequentialWriter<SensorData> **](http://msdn.microsoft.com/library/dn627340.aspx) i deserijalizirati s [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Rezultat zatim uspoređuje se s početnog instance da biste bili sigurni u identitet.

Sažimanje podataka u spremniku datoteka objekta putem zadane [**Deflate**] [ deflate-100] kodek iz .NET Framework 4. Pogledajte <a href="#Scenario5">primjer pete</a> u ovoj temi da biste saznali kako koristiti nedavnih i nadređenog verziju [**Deflate**] [ deflate-110] dostupne u .NET Framework 4,5 kodek.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will be compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Primjer 4: Serijalizacije objekt spremnik datoteke i serijalizacije pomoću generički zapisa

Ovaj primjer sličan je scenarija u <a href="#Scenario2">drugi primjer</a>gdje se u shemi izričito navedeno s JSON. Razlika koje je ovdje, shemu nije pretpostavlja da poznate čitača koji je deserializes.

Skup podataka test je prikupljeni u popis objekata [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) putem shemom eksplicitno definirane JSON i pohranjuju u datoteci spremnik objekt predstavlja klase [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) . Ovaj spremnik datoteka stvara writer koja se koristi za serijalizirati podatke, dekomprimirati memorije strujanje zatim spremljenu datoteku. Parametar [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) koji se koristi za stvaranje čitač određuje se ti podaci će se sažeti.

Podaci se čitanja datoteke i deserijalizirati u zbirku objekata. Ova zbirka uspoređuje se s početnog popis Avro zapisa da biste potvrdili da su jednake.


    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will not be compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




###<a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Primjer 5: Serijalizacije objekt spremnik datoteke pomoću kodeka prilagođene sažimanja

Peti primjer pokazuje kako kako koristiti prilagođene sažimanja kodek za Avro objekt spremnik datoteke. Uzorka koji sadrže kod u ovom primjeru mogu se preuzeti s web-mjesta [primjere Azure koda](http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111) .

[Specifikacija Avro](http://avro.apache.org/docs/current/spec.html#Required+Codecs) omogućuje korištenje programa neobavezno kodek (osim **Null** i **Deflate** zadane postavke). U ovom se primjeru je implementacijom posve nove kodek kao što su Snappy (koje se spominju kao podržani neobavezno kodek u [Avro specifikacija](http://avro.apache.org/docs/current/spec.html#snappy)). Pokazuje kako koristiti implementacije .NET Framework 4,5 [**Deflate**] [ deflate-110] kodek, koji omogućuje bolje sažimanja algoritam temeljen na biblioteke za komprimiranje [zlib](http://zlib.net/) od na zadanu verziju .NET Framework 4.


    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It will catch the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it will rely on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

###<a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Primjer 6: Korištenje Avro za prijenos podataka za servis Microsoft Azure HDInsight

Šesti primjer prikazuje neki programski tehnike vezane uz interakcija sa servisom Azure HDInsight. Uzorka koji sadrže kod u ovom primjeru mogu se preuzeti s web-mjesta [primjere Azure koda](https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3) .

Uzorak čini sljedeće:

* Povezuje se s postojećeg HDInsight klaster za servis.
* Serializes nekoliko CSV datoteke i prenese rezultat spremište blobova platforme Azure. (CSV datoteke su distribuirati zajedno s uzorak i predstavljaju programa izdvojiti iz podataka povijesne AMEX burzovni distribuira [Infochimps](http://www.infochimps.com/) za razdoblje 1970 2010. Uzorka čita podatke iz CSV datoteke, pretvara zapise u instance klase **burzovni** i serializes ih pomoću odraza. Definicija burzovni vrste stvara se iz sheme JSON putem uslužni generiranje koda za Microsoft Avro Library.
* Stvara novu tablicu vanjskih pod nazivom **dionice** u grozd i veze da biste podatke prenijeli u prethodnom koraku.
* Upit se izvršava pomoću grozd iznad tablice **dionice** .

Osim toga, uzorka izvodi proceduru čišćenja prije i nakon izvršavanja glavne operacije. Tijekom na čišćenja, sve povezane podatke blobova platforme Azure i mape uklanjaju se, a tablicu vrste Hive se prekine. Možete pozvati i čišćenja postupak iz naredbenog retka uzorka.

Uzorak sadrži sljedeće preduvjete:

* Aktivna pretplata na Microsoft Azure i njegov pretplate ID-a.
* Upravljanje certifikat za pretplatu za odgovarajući privatni ključ. Certifikat trebala biti instalirana na trenutnog korisnika privatnih prostora za pohranu na računalu se pokreće uzorka.
* Aktivni HDInsight klaster.
* Račun za Azure pohranu povezana klaster HDInsight iz prethodne preduvjeta, zajedno s odgovarajućeg ključa primarni ili sekundarni pristup.

Svi podaci iz preduvjete potrebno unijeti u oglednu datoteku za konfiguraciju prije pokretanja uzorka. Dva su moguća načina to učiniti:

* Uređivanje datoteke app.config u korijenskom direktoriju uzorak i stvorite uzorka
* Najprije sastavljanje uzorka, a zatim uredite AvroHDISample.exe.config u direktoriju Sastavi

U oba slučaja sve izmjene računajući na **<appSettings>** sekciji postavke. Slijedite komentare u datoteci.
Pokretanje uzorka iz naredbenog retka izvršavanjem sljedeće naredbe (gdje .zip datoteke s uzorka je pretpostavlja da se izdvajati na C:\AvroHDISample; ako u suprotnom koristite odgovarajuću put):

    AvroHDISample run C:\AvroHDISample\Data

Da biste očistili klaster, pokrenite sljedeću naredbu:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
