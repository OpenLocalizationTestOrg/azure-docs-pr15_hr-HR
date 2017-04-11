<properties
    pageTitle="Kako koristiti Azure datoteke sa Linux | Microsoft Azure"
        description="Stvaranje Azure zajedničkom u oblaku s ovom ćete praktičnom vodiču korak po korak. Upravljanje sadržajem za zajedničko korištenje svoje datoteke, a dostupnosti zajedničke datoteke iz Azure virtualnog računala (VM) sustavom Linux ili lokalni aplikaciju koja podržava SMB 3.0."
        services="storage"
        documentationCenter="na"
        authors="mine-msft"
        manager="aungoo"
        editor="tysonn" />

<tags ms.service="storage"
      ms.workload="storage"
      ms.tgt_pltfrm="na"
      ms.devlang="na"
      ms.topic="article"
      ms.date="10/18/2016"
      ms.author="minet" />


# <a name="how-to-use-azure-file-storage-with-linux"></a>Kako koristiti za pohranu datoteka Azure s Linux

## <a name="overview"></a>Pregled

Azure pohrani nudi zajedničko korištenje datoteka u oblak pomoću standardnih SMB protokol. Radite s datotekama Azure, možete migrirati enterprise aplikacije koje se temelje na poslužiteljima datoteka za Azure. Aplikacije koje rade u Azure možete jednostavno dostupnosti zajedničke datoteke iz Azure virtualnim strojevima izvodi Linux. A s najnovije izdanje prostora za pohranu datoteka, možete i postavljanje zajedničko korištenje datoteka iz lokalnog aplikacije koja podržava SMB 3.0.

Možete stvarati zajedničke Azure datoteke pomoću [Portala za Azure](https://portal.azure.com), cmdleta ljuske PowerShell za pohranu Azure, klijent biblioteka za pohranu Azure ili Azure prostora za pohranu REST API-JA. Uz to, budući da zajedničke datoteke SMB dionice, možete im pristupiti putem standardni datotečni sustav API-ji.

Pohrani se temelji na istoj tehnologiji kao spremište blobova platforme, tablice i reda čekanja da pohrani nudi dostupnost, rok trajanja, skalabilnost i zemlj zalihosti koja je ugrađena u platforme Azure prostora za pohranu. Detalje o ciljeve za pohranu datoteka i ograničenja, potražite u članku [skalabilnost Azure prostora za pohranu i ciljeve](storage-scalability-targets.md).

Pohrani sada je Općenito dostupan i podržava SMB 2.1 i SMB 3.0. Dodatne informacije o pohrani potražite u članku [servis datoteka REST API -JA](https://msdn.microsoft.com/library/azure/dn167006.aspx).

>[AZURE.NOTE] Linux SMB klijent još ne podržavaju šifriranje, pa postavljanje zajedničko korištenje datoteka iz Linux i dalje potreban je da se klijent nalaziti u istom Azure području kao zajedničko korištenje datoteka. Međutim, šifriranje podrška za Linux nalazi se na vodič razvojnim inženjerima Linux dužni SMB funkcionalnosti. Distribucija Linux koje podržava šifriranje u budućnosti moći s bilo kojeg mjesta i postavljanje programa za zajedničko korištenje datoteka Azure.

## <a name="video-using-azure-file-storage-with-linux"></a>Videozapis: Korištenje Azure pohrani s Linux

Ovdje se nalazi videozapis koji pokazuje kako stvoriti i koristiti zajedničke datoteke Azure na Linux.

> [AZURE.VIDEO azure-file-storage-with-linux]

## <a name="choose-a-linux-distribution-to-use"></a>Odaberite Linux distribucije za korištenje ##

Pri stvaranju Linux virtualnog računala u Azure, možete odrediti Linux slike koji podržava SMB 2.1 ili noviji iz galerije Azure slike. Slijedi popis preporučenih Linux slike:

- Poslužitelj Ubuntu 14.04 +
- RHEL 7 +
- CentOS 7 +
- Debian 8
- openSUSE 13.2 +
- SUSE Linux Enterprise Server 12
- SUSE Linux Enterprise Server 12 (Premium slike)

## <a name="mount-the-file-share"></a>Postavljanje zajedničko korištenje datoteka ##

Postavljanje zajedničko korištenje datoteka iz virtualnog računala koja se izvodi Linux, morate instalirati klijent za SMB/CIFS ako raspodjele koristite nema ugrađenu klijenta. To je naredba iz Ubuntu da biste instalirali jedan odabir cifs-uslužni:

    sudo apt-get install cifs-utils

Nakon toga koje morate donijeti postavljanja pokažite (mkdir mymountpoint), a problema naredbu postavljanja sličnu ovoj:

     sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename ./mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777

Postavke možete dodati i u vašem /etc/fstab postavljanje zajedničko korištenje.

Imajte na umu da ovdje 0777 predstavljaju šifru dozvola direktorija/datoteka koja daje izvođenja/čitanje/pisanje dozvole za sve korisnike. Možete je zamijeniti drugim kod datoteka dozvole praćenja Linux datoteka dozvole dokumenta.

Također da biste zadržali postavljen nakon ponovnog pokretanja zajedničke datoteke, možete dodati postavke kao što su ispod u vašem /etc/fstab:

    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777

Na primjer, ako ste stvorili Azure VM pomoću slika Linux Ubuntu poslužitelja 15.04 (koji je dostupan u galeriji Azure slike), možete postavljate datoteku kao ispod:

    azureuser@azureconubuntu:~$ sudo apt-get install cifs-utils
    azureuser@azureconubuntu:~$ sudo mkdir /mnt/mountpoint
    azureuser@azureconubuntu:~$ sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    azureuser@azureconubuntu:~$ df -h /mnt/mountpoint
    Filesystem  Size  Used Avail Use% Mounted on
    //myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint

Ako koristite CentOS 7.1, možete postaviti datoteku kao ispod:

    [azureuser@AzureconCent ~]$ sudo yum install samba-client samba-common cifs-utils
    [azureuser@AzureconCent ~]$ sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    [azureuser@AzureconCent ~]$ df -h /mnt/mountpoint
    Filesystem  Size  Used Avail Use% Mounted on
    //myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint

Ako koristite Otvori SUSE 13.2, postavljate datoteku kao ispod:

    azureuser@AzureconSuse:~> sudo zypper install samba*  
    azureuser@AzureconSuse:~> sudo mkdir /mnt/mountpoint
    azureuser@AzureconSuse:~> sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    azureuser@AzureconSuse:~> df -h /mnt/mountpoint
    Filesystem  Size  Used Avail Use% Mounted on
    //myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint

## <a name="manage-the-file-share"></a>Upravljanje zajedničko korištenje datoteka ##

[Portal za Azure](https://portal.azure.com) predstavlja korisničko sučelje za upravljanje prostorom za pohranu datoteka Azure. U web-pregledniku možete izvoditi sljedeće radnje:

- Prijenos i preuzimanje datoteka i iz vaše za zajedničko korištenje datoteka.
- Praćenje stvarnu upotrebu svaki za zajedničko korištenje datoteka.
- Prilagodba veličine kvote za zajedničko korištenje datoteka.
- Kopiraj u `net use` naredbu da biste koristili postavljanja sustava za zajedničko korištenje datoteka iz klijenta za Windows.

Da biste upravljali zajedničko korištenje datoteka možete koristiti i sučelje naredbenog retka za različite platforme Azure (Azure EŽA) iz Linux. Azure EŽA pruža skup Otvori izvor, više platformi naredbe za rad s Azure prostor za pohranu, uključujući za pohranu datoteka. Nudi velik broj isti funkcionalnosti pronaći Azure Portal, kao i funkcije obogaćenog podataka programa access. Primjeri, potražite u članku [Korištenje EŽA Azure s Azure prostora za pohranu](storage-azure-cli.md).

## <a name="develop-with-file-storage"></a>Razvoj s pohrani ##

Kao programer, možete izraditi aplikacije s pohrani pomoću [Azure prostora za pohranu klijenta biblioteke za Java](https://github.com/azure/azure-storage-java). Primjeri kod, potražite u članku [kako koristiti za pohranu datoteka iz Java](storage-java-how-to-use-file-storage.md).

[Azure prostora za pohranu klijenta biblioteku Node.js](https://github.com/Azure/azure-storage-node) možete koristiti i za razvoj protiv za pohranu datoteka.

## <a name="feedback-and-more-information"></a>Povratne informacije i dodatne informacije ##

Korisnici Linux želimo da biste čuli!

Azure pohrani za grupu korisnika Linux nudi forum za zajedničko korištenje povratnih informacija, kao što je procijeniti i prihvaćaju pohrani na Linux. E-pošte [Azure datoteka za pohranu Linux korisnicima](mailto:azurefileslinuxusers@microsoft.com) pridruživanje grupi korisnika.

## <a name="next-steps"></a>Daljnji koraci

Pogledajte ove veze za dodatne informacije o pohrani Azure.

### <a name="conceptual-articles-and-videos"></a>Konceptualni članke i videozapise

- [Azure za pohranu datoteka: frictionless oblaka SMB datotečnog sustava za Windows i Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
- [Početak rada s Azure pohrani u sustavu Windows](storage-dotnet-how-to-use-files.md)

### <a name="tooling-support-for-file-storage"></a>Tooling podrška za pohranu datoteka

- [Prijenos podataka s AzCopy naredbenog retka uslužni](storage-use-azcopy.md)
- [Stvaranje i Upravljanje zajedničkim datotekama](storage-azure-cli.md#create-and-manage-file-shares) pomoću EŽA Azure

### <a name="reference"></a>Referenca

- [Referenca datoteke servisa REST API-JA](http://msdn.microsoft.com/library/azure/dn167006.aspx)
- [Azure datoteke članka otklanjanje poteškoća](storage-troubleshoot-file-connection-problems.md)

### <a name="blog-posts"></a>Objava na blogu

- [Sada je Općenito dostupan Azure pohrani](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
- [Unutrašnji Azure pohrani](https://azure.microsoft.com/blog/inside-azure-file-storage/)
- [Uvod u Microsoft Azure servis datoteka](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
- [Održavanju veze na Microsoft Azure datoteke](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)
