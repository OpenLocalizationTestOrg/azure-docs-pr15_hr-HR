## <a name="scenario"></a>Scenarij

Da biste bolje prikazali kako stvoriti na VNet i podmreže, dokument će koristiti scenarij u nastavku.

![Scenarij VNet](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

U ovom slučaju morate stvoriti VNet pod nazivom **TestVNet** s rezerviranim blok CIDR **192.168.0.0./16**. Vaše VNet će sadržavati sljedeće podmreže: 

- **Sučelju**, pomoću **192.168.1.0/24** kao njegov CIDR blok.
- **Pozadinskog**, pomoću **192.168.2.0/24** kao njegov CIDR blok.

 