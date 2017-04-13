<properties
    pageTitle="Snimite sliku sadržaja na VM Windows Azure | Microsoft Azure"
    description="Snimite sliku sadržaja sustava Azure Windows virtualnog računala stvorene pomoću model klasični implementacije."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

#<a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Snimite sliku sadržaja sustava Azure Windows virtualnog računala stvorene pomoću model klasični implementacije.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Voditelj resursa modela informacije potražite u članku [stvaranje kopije Windows VM radi u Azure](virtual-machines-windows-vhd-copy.md).


U ovom se članku objašnjava snimiti Azure virtualnog računala sa sustavom Windows da biste ga mogli koristiti kao sliku da biste stvorili druge virtualnih računala. Na ovoj se slici obuhvaća disk operacijski sustav i diskova sve podatke koje su priložene virtualnog računala. Da bi se morate konfigurirati one prilikom stvaranja virtualnim strojevima koji koriste slike ga ne obuhvaća umrežavanje konfiguracije.

Azure pohranjuje na slici u odjeljku **Moja slika**. Ovo je na istom mjestu na kojem su pohranjene sve slike koje ste prenijeli. Detalje o slikama potražite u članku [o slike za virtualnim računalima](virtual-machines-linux-classic-about-images.md).

##<a name="before-you-begin"></a>Prije početka##

Ove se pretpostavlja da ste već stvorili Azure virtualnog računala i konfigurirali operacijskom sustavu, uključujući prilaganja diskova sve podatke. Ako još niste učinili još, pogledajte sljedeće upute:

- [Stvaranje virtualnog računala od slika.](virtual-machines-windows-classic-createportal.md)
- [Kako priložiti podatkovni disk virtualnog računala](virtual-machines-windows-classic-attach-disk.md)
- Provjerite je li uloge poslužitelja podržane su s Sysprep. Dodatne informacije potražite u članku [Sysprep podrška za uloge poslužitelja](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.WARNING] Ovaj postupak briše izvornu virtualnog računala nakon što ga je zabilježene. 

Prije no što caputuring slika Azure virtualnog računala, preporučuje se sigurnosno cilj virtualnog računala. Azure virtualnim strojevima mogu se sigurnosno pomoću Azure sigurnosnu kopiju. Detalje potražite u članku [sigurnosno kopiranje Azure virtualnim računalima](../backup/backup-azure-vms.md). U potvrđenog partnera dostupne su druge mogućnosti. Da biste saznali što trenutno nije dostupno, pretraživanje servisa Azure Marketplace.


##<a name="capture-the-virtual-machine"></a>Snimite virtualnog računala

1. [Azure klasični portal](http://manage.windowsazure.com)za **Povezivanje** s virtualnog računala. Upute potražite u članku [kako se prijaviti virtualnog računala sa sustavom Windows Server] [].

2.  Otvorite prozor naredbenog retka kao administrator.

3.  Promjena direktorija za `%windir%\system32\sysprep`, a zatim pokrenite sysprep.exe.

4.  Pojavit će se dijaloški okvir **Alat za pripremu sustava** . Učinite sljedeće:

    - Na **Akcija čišćenja sustava**odaberite **sučelje – okvir unesite sustava (OOBE)** , a zatim provjerite je li isključena **konf** . Dodatne informacije o korištenju Sysprep potražite u članku [upute za korištenje Sysprep: programa Uvod][].

    - U odjeljku **Mogućnosti isključivanja**odaberite **isključivanje**.

    - Kliknite **u redu**.

    ![Pokrenite Sysprep](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)

7.  Sysprep isključuje virtualnog računala koje promjene status virtualnog računala na portalu za Azure klasični da biste **prekinuli**.

8.  Azure klasični portalu kliknite **virtualnim strojevima** i odaberite virtualnog računala koje želite snimiti.

9.  Na naredbenoj traci kliknite **snimanje**.

    ![Snimite virtualnog računala](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)

    Pojavit će se dijaloški okvir **snimanje virtualnog računala** .

10. U **Naziv slike**, upišite naziv za novu sliku.

11. Prije nego što dodate sliku Windows Server skupu prilagođene slike, GRG ga mora biti generalizirano tako da pokrenete Sysprep prema odredbama u prethodnim koracima. Kliknite da biste naznačili koje niste **pokrenem Sysprep na virtualnog računala** .

12. Kliknite kvačicu da biste snimili sliku. Nova slika sada je dostupan u odjeljku **slike**.

    ![Slika snimanje uspješno](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

##<a name="next-steps"></a>Daljnji koraci

Slike je moguće koristiti za stvaranje virtualnim računalima sustava. Da biste to učinili, pomoću stavku **Iz galerije** izbornika i odaberite sliku koju ste upravo stvorili stvorit ćete virtualnog računala. Upute potražite u članku [Stvaranje virtualnog računala slike](virtual-machines-windows-classic-createportal.md).



[Kako se prijaviti virtualnog računala sa sustavom Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Kako koristiti Sysprep: Uvod]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
