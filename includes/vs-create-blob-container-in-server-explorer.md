Azure prostora za pohranu redova možete stvoriti pomoću Visual Studio **Explorer poslužitelja**.

![Poslužitelj Explorer blob-ova][Image1]

1. Na izborniku **Prikaz** odaberite **Poslužitelj Explorer**.
2. U programu Explorer poslužitelja Proširite čvor **Azure** za pretplatu, proširite čvor **prostora za pohranu** i čvor za račun za pohranu koje ste naveli u servisu Azure prostora za pohranu povezani.
3. Odaberite čvor **redovi** , a zatim na kontekstnom izborniku odaberite **Stvaranje reda** .
4. Unesite naziv za spremnik, a zatim odaberite **u redu**.   

Po zadanom je novi spremnik privatna i morate navesti prostora za pohranu tipkovnog da biste preuzeli blob polja s ovom spremniku. Ako želite objaviti datoteke u spremniku odaberite spremnik u **Programu Explorer poslužitelja** i pritisnite `F4` da biste prikazali prozor **Svojstva** . **Javni pristup za čitanje** postavite na **Blob**. Svi na internetu mogu vidjeti blob-ova u spremniku javno, ali možete izmijeniti ili ih izbrisati samo ako imate odgovarajuće tipkovni prečac.


[Image1]: ./media/vs-create-blob-container-in-server-explorer/vs-storage-create-blob-containers-in-Server-Explorer.png