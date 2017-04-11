## <a name="peering-across-subscriptions"></a>Peering putem pretplate

U ovom slučaju morate stvoriti peering između dva VNets pripadaju različitim pretplate.

![scenarij unakrsni sub](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

VNet peering ovisi o kontrola pristupa na temelju uloga (RBAC) za autorizaciju. Za više pretplata scenarij, morate najprije da biste dodijelili odgovarajuće dozvole za korisnike koji će se stvoriti peering veze:

> [AZURE.NOTE] Ako se isti korisnik ima ovlasti putem oba pretplate, možete preskočiti korak 1-4 ispod.
