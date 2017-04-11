## <a name="scenario"></a>Scenarij

Da biste bolje prikazali kako stvoriti NSGs, dokument će koristiti scenarij u nastavku.

![Scenarij VNet](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

U ovom scenariju ćete stvoriti na NSG za svaki podmreži u mreži virtualne **TestVNet** prema uputama u nastavku: 

- **NSG sučelju**. Sučelje NSG će se primijeniti na podmreži *sučelju* i sadrže dva pravila:  
    - **rdp pravilo**. Tim se pravilom omogućuje RDP promet u *sučelju* podmreži.
    - **pravilo za web**. Ovo pravilo će dopušta promet HTTP u *sučelju* podmreži.
- **NSG pozadinskog**. Pozadinska NSG će se primijeniti na podmreži *pozadinskog sustava* i sadrže dva pravila: 
    - **pravilo za sql**. Tim se pravilom omogućuje SQL promet samo iz podmreže *sučelju* .
    - **pravilo za web**. Tim se pravilom onemogućava sva internetska vezana promet s *pozadinskom* podmreže.

Kombinacija ta pravila stvorite DMZ nalik scenarij, gdje podmreži pozadinskih možete primati samo promet za SQL iz podmreži sučelje i ima pristup Internetu, dok je sučelje podmreži možete komunicirati s Internetom i primati samo dolazni zahtjevi za HTTP.
 
