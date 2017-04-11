<properties
   pageTitle="Upravljanje Azure Analysis Services | Microsoft Azure"
   description="Informirajte se o upravljanju poslužitelju komponente Analysis Services u Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="manage-analysis-services"></a>Upravljanje Analysis Services

Nakon što stvorite poslužitelju komponente Analysis Services u Azure, možda postoji neki Administracija i upravljanje zadaci morate izvršiti odmah ili tijekom dolje putu. Ako, na primjer, pokrenite obradu Osvježavanje podataka i kontrolirati tko možete pristupiti modelima na poslužitelju ili praćenje stanja na poslužitelj. Neke zadatke upravljanja može izvršiti samo Azure portalu drugima u SQL Server Management Studio (SSMS), a neke zadatke moguće izvršiti bilo.

## <a name="azure-portal"></a>Portal za Azure
[Portal za Azure](http://portal.azure.com/) je gdje možete stvoriti i brisanje poslužitelje, praćenje resurse poslužitelja, promijenite veličinu, i upravljati tko ima pristup poslužiteljima.  Ako imate nekih problema, možete poslati i zahtjev za podršku.

![Pristupili naziv poslužitelja na servisu Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Povezivanje s poslužiteljem u Azure je baš kao i povezivanje s instanca poslužitelja u vašoj tvrtki ili ustanovi. Iz SSMS, možete izvršavaju mnoge zadatke kao što je postupak podataka ili stvoriti skriptu obrada, Upravljanje ulogama i pomoću komponente PowerShell.

![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

 Jedna od veće razlike je provjera autentičnosti pomoću kojih se povezati s poslužiteljem. Da biste se povezali s poslužiteljem za Azure Analysis Services, morate odabrati **Provjeru autentičnosti lozinke Active Directory**.

### <a name="to-connect-with-ssms"></a>Da biste se povezali s SSMS
1. Prije povezivanja, morate dobiti naziv poslužitelja. **Azure** portalu > poslužitelja > **Pregled** > **naziv poslužitelja**, kopirajte naziv poslužitelja.

    ![Pristupili naziv poslužitelja na servisu Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

2. U SSMS > **Explorer objekt**, kliknite **Poveži** > **Analysis Services**.

3. U dijaloškom okviru **za povezivanje s poslužiteljem** zalijepite u okviru naziv poslužitelja, a zatim u **provjeru autentičnosti**, odaberite nešto od sljedećeg:

    **Active Directory integrirane provjere** da biste pomoću jedinstvene prijave pomoću servisa Active Directory Azure Active Directory federation.

    **Provjera autentičnosti lozinke active Directory** da biste koristili račun tvrtke ili ustanove. Ako, na primjer, kada povezujete nije-domene pridruženo računala.

    Napomena: Ako ne vidite provjere autentičnosti za Active Directory, možda morate [omogućiti provjeru autentičnosti Azure Active Directory](#enable-azure-active-directory-authentication) u SSMS.

    ![Povezivanje u SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

Budući da upravljanje na poslužitelju u Azure pomoću SSMS je mnogo jednaki upravljanje lokalni poslužitelj, ne možemo nećete odlaze pojedinosti ovdje. U pomoći koju morate pronaći ćete u [Analysis Services instancu upravljanja](https://msdn.microsoft.com/library/hh230806.aspx) na MSDN-u.

## <a name="server-administrators"></a>Administratori poslužitelja
**Administratori Analysis Services** možete koristiti u plohu kontrole za poslužitelj za Azure portal ili SSMS da biste upravljali administratora poslužitelja. Administratori Analysis Services su administratori poslužitelja baze podataka s pravima za uobičajene zadatke administracije bazu podataka kao što su Dodavanje i uklanjanje baze podataka i upravljanje korisnicima. Prema zadanim postavkama korisnika koji stvara poslužitelj Azure portalu automatski se dodaju kao administrator Analysis Services

Također biste trebali znati:

-   Windows Live ID nije vrsta podržani identiteta za Azure Analysis Services.  
-   Administratori Analysis Services moraju biti valjane korisnici Azure Active Directory.
-   Ako stvorite poslužitelj za Azure Analysis Services putem Voditelj resursa Azure predlošci, administratori Analysis Services vodi JSON polja korisnika koji je potrebno dodati kao administratori.

Administratori Analysis Services može se razlikovati od administratora Azure resursa koje možete upravljati resursi za Azure pretplate. Time se zadržava kompatibilnost s postojećim XMLA i TSML upravljanje ponašanja u Analysis Services i omogućuje vam segregate obavezama koje imate između Upravljanje resursima Azure i analize servisima upravljanje bazom podataka.

Prikaz svih uloga i pristup vrste za vaše Azure Analysis Services resursa, pomoću kontrola pristupa (IAM) na plohu kontrolu.

## <a name="database-users"></a>Korisnici baze podataka
Azure korisnici baze podataka modela komponente Analysis Services moraju biti u Azure Active Directory. Korisnička imena navedeni modela baze podataka mora biti adresu e-pošte tvrtke ili ustanove ili UPN-a. Time se razlikuje od baze podataka modela lokalnog koje podržavaju korisnicima tako da korisnička imena domene u sustavu Windows.

Korisnike možete dodati pomoću [dodjele uloga u servisu Azure Active Directory](../active-directory/role-based-access-control-configure.md) ili pomoću [Jezika skriptiranje tablični Model](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) u programu SQL Server Management Studio.

**Primjer TMSL skripte**

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Users"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@contoso.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="enable-azure-active-directory-authentication"></a>Omogući provjeru autentičnosti Azure Active Directory
Da biste omogućili značajku provjere autentičnosti Azure Active Directory za SSMS u registru, stvoriti tekstnu datoteku pod nazivom EnableAAD.reg, a zatim kopirajte i zalijepite sljedeće:


```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\Microsoft Analysis Services\Settings]
"AS AAD Enabled"="True"
```

Spremanje, a zatim pokrenite datoteku.



## <a name="next-steps"></a>Daljnji koraci
Ako već niste implementiran tabličnog modela na novi poslužitelj, sada je jedan dobar. Dodatne informacije potražite u članku [uvođenja za Azure Analysis Services](analysis-services-deploy.md).

Ako ste implementiran modela na poslužitelju, spremni ste za povezivanje s njom pomoću klijenta ili preglednika. Da biste saznali više, pročitajte članak [dohvaćanje podataka iz komponente Analysis Services Azure server](analysis-services-connect.md).
