## <a name="service-chaining---transit-through-peered-vnet"></a>Servis Chaining - prijenosa putem peered VNet

Iako pomoću sustava usmjerava olakšava promet automatski za implementaciju sustava, postoje slučajevi u kojima želite upravljati usmjeravanje pakete putem virtualne uređaj.
U ovom scenariju postoje dvije VNets pretplatu, HubVNet i VNet1 kao što je opisano u dijagramu u nastavku. Implementacija mreže virtualne Appliance(NVA) u VNet HubVNet. Nakon uspostave VNet peering između HubVNet i VNet1, možete postaviti korisnički definirane usmjerava i navedite sljedeće mjesto da biste NVA u na HubVNet.

![NVA prijenosa](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [AZURE.NOTE] Jednostavnosti, pretpostavlja da sve VNets Ovdje su u okviru iste pretplate. No može se funkcionira i za više pretplata scenarij.

Svojstvo ključa da biste omogućili usmjeravanje prijenosa je parametar "Dopusti prosljeđuju promet". Time se omogućuje prihvaćanje i slanja promet s u NVA peered VNet.  
