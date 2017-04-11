<properties
    pageTitle="Povezivanje s Azure snop s EŽA | Microsoft Azure"
    description="Saznajte kako koristiti različite platforme sučelja naredbenog retka (EŽA) da biste upravljali i implementacija resurse u stogu Azure"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-and-configure-azure-stack-cli"></a>Instaliranje i konfiguriranje EŽA snop Azure

U ovom dokumentu, ne možemo vas voditi kroz postupak korištenja Azure sučelje naredbenog retka (EŽA) za upravljanje resursima stogu Azure platforme Linux i Mac klijenta.  

## <a name="install-azure-stack-cli"></a>Instalacija Azure stogu EŽA

Ako ste na računalu Mac ili Linux, možete dobiti na EŽA pomoću sljedeće naredbe:
  
    `npm install -g azure-cli@0.10.4`.


## <a name="connect-to-azure-stack"></a>Povezivanje s Azure stogu
U sljedećim koracima konfigurirati Azure EŽA povezati Azure stogu. Zatim prijavite i Dohvaćanje informacija o pretplati.

1.  Dohvaćanje vrijednosti za active directory-resursa – id izvršavanjem ovaj PowerShell:
        
          (Invoke-RestMethod -Uri https://api.azurestack.local/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]

2.  Koristite sljedeću naredbu EŽA da biste dodali Azure stogu okruženju, pazeći da biste ažurirali *– active directory-resursa – id* s podacima URL dohvatiti u prethodnom koraku:

           azure account env add AzureStack --resource-manager-endpoint-url "https://api.azurestack.local" --management-endpoint-url "https://api.azurestack.local" --active-directory-endpoint-url  "https://login.windows.net" --portal-url "https://portal.azurestack.local" --gallery-endpoint-url "https://portal.azurestack.local" --active-directory-resource-id "https://azurestack.local-api/" --active-directory-graph-resource-id "https://graph.windows.net/"

3.  Prijavite se pomoću sljedeće naredbe (Zamijeni *korisničko ime* pomoću svojeg korisničkog imena):

        azure login -e AzureStack -u “<username>”

    >[AZURE.NOTE]Ako nailazite na probleme provjeru valjanosti certifikata, onemogućite provjeru valjanosti certifikata tako da pokrenete naredbu `set        NODE_TLS_REJECT_UNAUTHORIZED=0`.

4.  Postavite Azure konfigurira Azure resursa upravitelju pomoću sljedeće naredbe:

        azure config mode arm

5.  Dohvatiti popis pretplata.

        azure account list     

## <a name="next-steps"></a>Daljnji koraci

[Uvođenje predloška Azure EŽA](azure-stack-deploy-template-command-line.md)

[Povezivanje sa servisom PowerShell](azure-stack-connect-powershell.md)

[Upravljanje korisničkim dozvolama](azure-stack-manage-permissions.md)
