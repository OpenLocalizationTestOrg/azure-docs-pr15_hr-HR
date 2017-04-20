## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Priprema za provjeru autentičnosti zahtjeva Azure upravitelj resursa

Morate provjeriti autentičnost sve operacije koje izvršavaju na resurse pomoću [Upravitelja resursa Azure] [ lnk-authenticate-arm] s Azure Active Directory (AD). Najjednostavniji način da biste konfigurirali ovaj je koristite PowerShell ili Azure CLI za Defragmentaciju.

Trebali biste instalirati [Azure PowerShell 1.0] [ lnk-powershell-install] ili noviji prije nego nastavite.

Pokaži sljedeće korake kako postaviti provjeru autentičnosti lozinke za zatvaranje AD pomoću PowerShell. Ove naredbe možete pokrenuti u standardni PowerShell sesije.

1. Prijavite se u pretplatu Azure pomoću sljedeće naredbe:

    ```
    Login-AzureRmAccount
    ```

2. Zabilježite **TenantId** i **SubscriptionId**. Morate ih kasnije.

3. Stvaranje nove aplikacije Azure Active Directory pomoću sljedeće naredbe zamjenu osobnijeg mjesto:

    - **{Zaslonsko ime}:** ime za prikaz aplikacije poput **MySampleApp**
    - **{Početnoj stranici URL}:** URL početnoj stranici app što **http://mysampleapp/home**. Ovaj URL ne trebate pokažite realni aplikacije.
    - **{Identifikator aplikacije}:** Jedinstveni identifikator poput **http://mysampleapp**. Ovaj URL ne trebate pokažite realni aplikacije.
    - **{Lozinku}:** Lozinka koja će koristiti za provjeru autentičnosti s vaše app.

    ```
    New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
    ```
    
4. Zabilježite **ApplicationId** aplikaciju koju ste stvorili. Trebat će vam to kasnije.

5. Stvaranje novog servisa glavni koristeći sljedeću naredbu **{MyApplicationId}** zamjenu s **ApplicationId** iz prethodnog koraka:

    ```
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
    
6. Postavljanje dodjelu uloge koristeći sljedeću naredbu **{MyApplicationId}** zamjenu s **ApplicationId**.

    ```
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```
    
Sada ste završili stvaranje Azure AD aplikacije koji će omogućiti provjeru iz prilagođene C# aplikacije. U nastavku ovog praktičnog vodiča će trebati sljedeće vrijednosti:

- TenantId
- SubscriptionId
- ApplicationId
- Lozinka

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: ../articles/powershell-install-configure.md
