### <a name="app-service-plan"></a>Aplikacije servisa za planiranje

Stvara tarifa za servis za hostiranje web-aplikaciji. Navedite naziv plana kroz parametar **hostingPlanName** . Mjesto plana je na isto mjesto koje se koriste za grupu resursa. U parametara **sku** i **workerSize** određeni su klauzulom cijene veličina sloju i tempiranja

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

