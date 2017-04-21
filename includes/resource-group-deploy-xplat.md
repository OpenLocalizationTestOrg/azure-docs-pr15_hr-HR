## <a name="how-to-deploy-with-azure-cli"></a>Kako implementirati s EŽA Azure

1. Prijavite se na račun za Azure.

        azure login

  Nakon unošenja vjerodajnice, naredba vraća rezultat vašu prijavu.

        ...
        info:    login command OK

2. Ako imate više pretplata, unesite id pretplate koju želite koristiti za implementaciju.

        azure account set <YourSubscriptionNameOrId>

3. Prijelaz na modula Azure Voditelj resursa

        azure config mode arm

   Primit ćete potvrdu novi način rada.

        info:     New mode is arm

4. Ako nemate postojeću grupu resursa, stvorite novu grupu resursa. Navedite naziv grupe resursa i mjesto na koje su vam potrebne za rješenje.

        azure group create -n ExampleResourceGroup -l "West US"

   Vraća se sažetak novu grupu resursa.

        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Da biste stvorili novu implementaciju za grupu resursa, pokrenite sljedeću naredbu i navedite potrebne parametre. Parametri neće sadržavati naziv za implementaciju sustava, naziv grupu resursa, put ili URL-a za predložak koji ste stvorili i sve parametre potrebno za vašu situaciju.

   Imate sljedeće mogućnosti za dohvat vrijednosti parametara:

   - Korištenje parametara u istoj razini i lokalne predložak.

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Korištenje parametara u istoj razini i veza na predložak.

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Korištenje parametra datoteke.

             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

  Kada je uveden u grupu resursa, vidjet ćete sažetak implementacije.

         info:    Executing command group deployment create
         + Initializing template configurations and parameters
         + Creating a deployment
         ...
         info:    group deployment create command OK


6. Da biste dobili informacije o najnovijim uvođenja.

         azure group log show -l ExampleResourceGroup

7. Da biste dobili detaljne informacije o implementacije pogreške.

         azure group log show -l -v ExampleResourceGroup
