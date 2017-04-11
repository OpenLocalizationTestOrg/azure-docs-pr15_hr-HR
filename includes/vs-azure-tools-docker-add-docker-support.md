1. U Visual Studio **Preglednik rješenja**, desnom tipkom miša kliknite projekt i odaberite **Dodavanje > podrška Docker** na kontekstnom izborniku.

    ![Dodavanje Kontekstni izbornik za podršku Docker](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. Dodavanje Docker podrška u ASP.NET osnovne web project rezultira Zbrajanje nekoliko Docker povezane datoteke koje se dodaju u projekt, uključujući Docker sastavite datoteke, skripte komponente Windows PowerShell za implementaciju i Docker svojstva datoteke. 

    ![Datoteke docker dodali projekt](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE]Ako koristite [Docker za Windows Beta](https://beta.docker.com), otvorite Properties\Docker.props, uklonite zadanu vrijednost i ponovno pokrenuti Visual Studio za vrijednost stupile na snagu.
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=='' "></DockerMachineName>
> ```
