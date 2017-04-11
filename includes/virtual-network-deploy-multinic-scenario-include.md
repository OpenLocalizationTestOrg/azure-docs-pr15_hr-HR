## <a name="scenario"></a>Scenarij

Ovaj dokument će voditi kroz implementacije koja koristi više NIC-ovi u VMs u scenariju s određenim. U ovom scenariju, imate dvije tiered IaaS radno opterećenje smješten u Azure. Svaki sloju je uveden u vlastitom podmreži u virtualne mreže (VNet). Razina sučelje sastoji se od nekoliko web-poslužiteljima grupirane raspoređivača opterećenja za visoke dostupnosti. Pozadinska sloju sastoji se od nekoliko poslužitelja baze podataka. Sljedećim poslužiteljima baza podataka će biti implementirano s dva NIC-ovi, jedan za pristup bazi podataka na drugi način za upravljanje. Scenarij sadrži i mreže sigurnosnih grupa (NSGs) da biste odredili prometu je dopušteno svaki podmreži i NIC implementacije. Na slici u nastavku prikazuje osnovni arhitektura scenarij.  

![Scenarij MultiNIC](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

