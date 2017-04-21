
<!--
includes/sql-database-include-connection-string-40-config.md

Latest Freshness check:  2015-09-04 , GeneMi.

## Connection string
-->


### <a name="example-config-file-for-connection-string-security"></a>Primjer konfiguracijska datoteka za sigurnost niz veze


To je unsound da biste postavili niz za povezivanje kao literala u C# kod. Da biste postavili niz za povezivanje u konfiguracijskoj datoteci bolje je. Postoji možete urediti niz bilo kojem trenutku bez potrebe za prevoditi.

Pogledajmo pretpostavlja kompilirane C# program pod nazivom **ConsoleApplication1.exe**i u ovom .exe nalazi se u na **bin\debug\* * direktorija.

U ovom primjeru Većina dijelove niz za povezivanje spremaju se u konfiguracijskoj datoteci pod nazivom točno **ConsoleApplication1.exe.config**. Konfiguracijska datoteka također moraju se nalaziti u **bin\debug\**.

U XML sljedeće konfiguracijskoj datoteci vidjeti niza za povezivanje s nazivom **ConnectionString4NoUserIDNoPassword**. C# kod traži niz.

Potrebno je urediti realni nazive u rezerviranih mjesta:

- {your_serverName_here}
- {your_databaseName_here}



        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
        
            <connectionStrings>
                <clear />
                <add name="ConnectionString4NoUserIDNoPassword"
                providerName="System.Data.ProviderName"
        
                connectionString=
                "Server=tcp:{your_serverName_here}.database.windows.net,1433;
                Database={your_databaseName_here};
                Connection Timeout=30;
                Encrypt=True;
                TrustServerCertificate=False;" />
            </connectionStrings>
        </configuration>



Za ovaj ilustracija smo odabrali izostaviti dva parametra:

- Korisnički ID = {your_userName_here};
- Lozinka = {your_password_here};


Njih možete obuhvatiti, ali ponekad je bolje da bi se vaš program za dohvaćanje vrijednosti iz unosa na tipkovnici korisnik. Ovisi.



<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
