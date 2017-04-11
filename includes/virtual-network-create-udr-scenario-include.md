## <a name="scenario"></a>Scenarij

Da biste bolje prikazali kako stvoriti UDRs, dokument će koristiti scenarij u nastavku.

![OPIS SLIKE](./media/virtual-network-create-udr-scenario-include/figure1.png)

U ovom scenariju ćete stvoriti jednu UDR *podmreže završetka prednji plan* i drugi UDR za *podmreže završetka natrag* prema uputama u nastavku: 

- **UDR sučelju**. Sučelje UDR će se primijeniti na podmreži *sučelju* i sadrže jedan usmjeravanje:  
    - **RouteToBackend**. Ovu rutu će poslati svim promet pozadinskih podmreži omogućili **FW1** virtualnog računala.
- **UDR pozadinskog**. Pozadinska UDR će se primijeniti na podmreži *pozadinskog sustava* i sadrže jedan usmjeravanje: 
    - **RouteToFrontend**. Ovu rutu će poslati svim promet sučelje podmreži omogućili **FW1** virtualnog računala.

Kombinacija te usmjerava će osigurati da sve promet namijenjene prikazivanju iz jednog podmreže na drugi će biti proslijeđene virtualnog računala **FW1** koji se koristi kao virtualne potražite. Morate uključite IP prosljeđivanja za taj VM da biste bili sigurni da možete primati promet namijenjene prikazivanju na druge VMs.
