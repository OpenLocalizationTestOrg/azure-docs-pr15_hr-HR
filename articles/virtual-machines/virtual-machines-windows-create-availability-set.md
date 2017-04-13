<properties
    pageTitle="Stvaranje skupa dostupnost VM | Microsoft Azure"
    description="Saznajte kako stvoriti raspoloživost za virtualnim računalima sustava pomoću portala za Azure ili PowerShell pomoću modela implementacije Voditelj resursa."
    keywords="Postavljanje dostupnosti"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>


# <a name="create-an-availability-set"></a>Stvaranje skupa dostupnosti 

Kada pomoću portala, ako želite da se vaša VM kao dio sustava dostupnost postavljanje potrebnih za stvaranje dostupnost postaviti prvo.

Dodatne informacije o stvaranju i korištenju dostupnost skupove potražite u članku [Upravljanje dostupnost virtualnih računala](virtual-machines-windows-manage-availability.md).


## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a>Stvaranje raspoloživost postaviti prije stvaranja vaše VMs pomoću portala

1. Na izborniku koncentrator kliknite **Pregledaj** , a zatim odaberite **postavlja dostupnost**.

2. Na na **dostupnost postavlja plohu**, kliknite **Dodaj**.

    ![Snimka zaslona koja prikazuje gumb Dodaj za stvaranje novog dostupnost postavite.](./media/virtual-machines-windows-create-availability-set/add-availability-set.png)

3. Na plohu **Postavljanje dostupnosti stvaranje** dovršite informacije o skupu.

    ![Snimka zaslona koja prikazuje podatke morate unijeti da biste stvorili skup dostupnost.](./media/virtual-machines-windows-create-availability-set/create-availability-set.png)

    - **Ime** - naziv mora biti 1 80 znakova koji se sastoji od brojeva, slova, razdoblja, podvlake ili crtice. Prvi znak mora biti slovo ili broj. Zadnji znak mora biti slova, brojeva i podvlake.
    - **Domena kvara** - domene kvara definiranje grupa virtualnim strojevima zajedničko korištenje uobičajenih power izvora i mrežne skretnice. Prema zadanim postavkama, u VMs odvojene svim do tri kvara domenama i moguće je promijeniti u između 1 i 3.
    - **Ažuriranje domene** - pet ažuriranje domene dodjeljuju se po zadanom, i to može se postaviti između 1 i 20. Ažuriranje domene određuju grupe virtualnim strojevima i temeljni fizičke hardver koji možete ponovno pokrenuti u isto vrijeme. Na primjer, ako ne možemo odredite pet ažuriranje domene, kad više od pet virtualnim strojevima konfigurirane u jednom skupu dostupnosti, šestog virtualnog računala će se nalaziti u istoj ažuriranje domene kao prvi virtualnog računala, sedmog u istom UD kao drugi virtualnog računala i tako dalje. Redoslijed na ponovna možda neće biti uzastopne, ali samo jedan ažuriranje domene će biti sustava odjednom.
    - **Pretplata** - odaberite pretplatu koristite ako imate više od jedne.
    - **Grupa resursa** - odaberite postojeću grupu resursa tako da kliknete strelicu, a zatim odaberete grupu resursa iz na padajućem izborniku prema dolje. Možete stvoriti i novu grupu resursa tako da upišete naziv. Naziv može sadržavati sljedeće znakove: slova, brojeve, razdoblja, crtice, podvlake i otvaranje ili zagradu. Naziv ne može završavati točkom. Svim VMs u grupi dostupnost potrebna će biti stvoren u istoj grupi resursa postavljenog dostupnost.
    - **Mjesto** – odaberite mjesto s padajućeg popisa.

4. Kada završite unos informacija, kliknite **Stvori**. Nakon što grupe dostupnosti, možete ga vidjeti na popisu nakon osvježavanja portalu.

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a>Pomoću portala za stvaranje virtualnog računala i raspoloživost postaviti u isto vrijeme

Ako stvarate novu VM pomoću portala, možete stvoriti i novi dostupnost postavljena za na VM prilikom stvaranja prvog VM u skupu.

![Snimka zaslona koja prikazuje postupak stvaranja novog dostupnost postavljanje tijekom stvaranja na VM.](./media/virtual-machines-windows-create-availability-set/new-vm-avail-set.png)


## <a name="add-a-new-vm-to-an-existing-availability-set"></a>Dodavanje novog VM postojeći skup dostupnosti

Za svaku dodatnu VM koje ste stvorili koje treba pripadaju u skupu, provjerite je li stvoriti u istoj **grupi resursa** i odaberite postojeći dostupnost postavljanje u koraku 3. 

![Snimka zaslona s prikazom načina da biste odabrali postojeći dostupnost koji se koristi za vaše VM.](./media/virtual-machines-windows-create-availability-set/add-vm-to-set.png)



## <a name="use-powershell-to-create-an-availability-set"></a>Korištenje ljuske PowerShell za stvaranje skupa dostupnosti

U ovom se primjeru stvara raspoloživost postavljanje u grupi resursa **RMResGroup** mjestu **Zapad SAD -a** . To je potrebno učiniti prije stvaranja prvog VM koji će biti u skupu.

    New-AzureRmAvailabilitySet -ResourceGroupName "RMResGroup" -Name "AvailabilitySet03" -Location "West US"
    
Dodatne informacije potražite u članku [Novo AzureRmAvailabilitySet](https://msdn.microsoft.com/library/mt619453.aspx).


## <a name="troubleshooting"></a>Otklanjanje poteškoća

- Kada stvorite VM, ako nije željeni skup dostupnost na padajućem popisu na portalu možda ste stvorili ga u grupi različitih resursa. Ako ne znate grupi resursa za vašu dostupnost postavljanje, idite na izbornik koncentratora i kliknite Pregledaj > dostupnost postavlja da biste vidjeli popis skupova dostupnosti i koji pripadaju grupama resursa.


## <a name="next-steps"></a>Daljnji koraci

Dodajte dodatni prostor za pohranu u vašem VM dodatni [podatkovni disk](virtual-machines-windows-attach-disk-portal.md).
