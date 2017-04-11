Pomoću upravitelja Azure resursa, definiranje parametara za vrijednosti koje želite da biste odredili kada je implementiran u predložak. Predložak uključuje sekcije naziva parametrima koji sadrži sve vrijednosti parametra.
Morate definirati parametar za te vrijednosti koje će se razlikovati koji se temelji na projektu implementirate ili ovise o okruženju implementacije za. Definiranje parametara za vrijednosti koje se uvijek će ostati isto. Svaku vrijednost parametra se koristi u predlošku da biste definirali resurse koji su implementacija. 

Pri definiranju parametara polje **allowedValues** koristiti da biste odredili vrijednosti koje korisnik pružaju tijekom implementacije. Polje **defaultValue** koristiti se dodjeljuje vrijednost za parametar, ako je vrijednost tijekom implementacije.

Ne možemo opisane su parametra u predlošku.

### <a name="logicappname"></a>logicAppName

Naziv aplikacije logika da biste stvorili.

    "logicAppName": {
        "type": "string"
    }