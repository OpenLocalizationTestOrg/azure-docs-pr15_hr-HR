## <a name="defining-a-backup-policy"></a>Definiranje sigurnosne kopije pravila

Sigurnosne kopije pravila definira matrica kada uzimaju snimke podataka i koliko se zadržavaju te snimke. Prilikom definiranja pravila za sigurnosno kopiranje s VM, možete pokrenuti sigurnosno kopiranje *jednom dnevno*. Prilikom stvaranja novog pravilnika se primjenjuje na zbirke ključeva. Sučelje za sigurnosne kopije pravila izgleda ovako:

![Sigurnosne kopije pravila](./media/backup-create-policy-for-vms/backup-policy.png)

Da biste stvorili pravilo:

1. Unesite naziv za **naziv pravila**.

2. Brze snimke podataka se može preuzeti intervalima dnevnih ili tjedno. Pomoću **Učestalost sigurnosno kopiranje s** padajućeg izbornika odaberite hoće li se snimke podataka uzimaju dnevno ili tjedno.

    - Ako odaberete dnevnih interval, pomoću istaknuta kontrola odaberite doba dana za snimke. Da biste promijenili sat, poništite odabir sat, a zatim odaberite novi sat.

    ![Dnevni sigurnosne kopije pravila](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>

    - Ako odaberete tjedno interval, odaberite dane u tjednu i doba dana da biste preuzeli snimku pomoću istaknuta kontrola. Na izborniku dan, odaberite jedan ili više dana. Na izborniku sat odaberite jedan sat. Da biste promijenili sat, poništite odabir odabranog sat, a zatim odaberite novi sat.

    ![Tjedni sigurnosne kopije pravila](./media/backup-create-policy-for-vms/backup-policy-weekly.png)

3. Prema zadanim postavkama, odabrane su sve mogućnosti **Zadržavanja raspon** . Poništite sve ograničenje zadržavanja raspon ne želite koristiti. Zatim navedite interval(s) za korištenje.

    Mjesečne i godišnje zadržavanja raspona omogućuju da biste odredili snimaka na temelju tjednih i dnevnih povećanje.

    >[AZURE.NOTE] Kada zaštite u VM, sigurnosno kopiranje pokreće se jednom dnevno. Vrijeme kada se pokrene sigurnosne kopije se ne razlikuje za svaki raspon zadržavanja.

4. Nakon što postavite sve mogućnosti pravila, pri vrhu na plohu kliknite **Spremi**.

    Novog pravilnika odmah primjenjuje se na zbirke ključeva.
