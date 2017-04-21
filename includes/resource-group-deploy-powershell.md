## <a name="how-to-deploy-with-powershell"></a>Kako implementirati sa servisom PowerShell

1. Prijavite se na račun za Azure.

          Add-AzureAccount

   Nakon unošenja vjerodajnice, naredba vraća podatke o vašem računu.

          Id                             Type       ...
          --                             ----    
          example@contoso.com            User       ...   

2. Ako imate više pretplata, unesite id pretplate koju želite koristiti za implementaciju. 

          Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. Prijeđite u modul Azure Voditelj resursa.

          Switch-AzureMode AzureResourceManager

4. Ako nemate postojeću grupu resursa, stvorite novu grupu resursa. Navedite naziv grupe resursa i mjesto na koje su vam potrebne za rješenje.

        New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"

   Vraća se sažetak novu grupu resursa.

        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. Da biste stvorili novu implementaciju za grupu resursa, pokrenite naredbu **Novo AzureResourceGroupDeployment** i navedite potrebne parametre. Parametri neće sadržavati naziv za implementaciju sustava, naziv grupu resursa, put ili URL-a za predložak koji ste stvorili i sve parametre potrebno za vašu situaciju. 
   
   Imate sljedeće mogućnosti za dohvat vrijednosti parametara: 
   
   - Korištenje parametara u istoj razini.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

   - Koristite parametar objekt.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

   - Pomoću datoteke parametar.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

  Kada je uveden u grupu resursa, vidjet ćete sažetak implementacije.

             DeploymentName    : ExampleDeployment
             ResourceGroupName : ExampleResourceGroup
             ProvisioningState : Succeeded
             Timestamp         : 4/14/2015 7:00:27 PM
             Mode              : Incremental
             ...

6. Da biste dobili informacije o implementacije pogreške.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. Da biste dobili detaljne informacije o implementacije pogreške.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
