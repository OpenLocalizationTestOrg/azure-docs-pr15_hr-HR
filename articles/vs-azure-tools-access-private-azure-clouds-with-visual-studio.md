<properties 
   pageTitle="Pristup privatnim Azure oblaka Visual Studio | Microsoft Azure"
   description="Saznajte kako pristupiti privatne oblaka resursima pomoću Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Pristup privatnim Azure oblaka Visual Studio

##<a name="overview"></a>Pregled

Prema zadanim postavkama Visual Studio podržava krajnje točke OSTALE javno Azure oblaka. To može biti problem, no ako koristite Visual Studio sa privatne Azure oblaka. Certifikati možete koristiti da biste konfigurirali Visual Studio da biste pristupili krajnje točke OSTALE privatne Azure oblaka. To možete dobiti certifikati putem vaše Azure objavljivanje datoteka postavki.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Da biste pristupili privatne Azure oblaka u Visual Studio

1. [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885) za privatne spremanja preuzeti datoteku postavke objavljivanja ili zatražite od administratora datoteka postavki za objavljivanje. Na javnom verziji Azure vezu da biste preuzeli to je [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Datoteka preuzimate mora imati nastavkom .publishsettings.)

1. U programu **Explorer poslužitelja** u Visual Studio, odaberite čvor **Azure** i na izborniku prečacu odaberite naredbu **Upravljanje pretplatama** .

    ![Upravljanje pretplatama naredbe](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. U dijaloškom okviru **Upravljanje pretplate na Microsoft Azure** odaberite karticu **potvrde** , a zatim odaberite gumb **Uvezi** .

    ![Uvoz certifikata za Azure](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. U dijaloškom okviru **Uvoz Microsoft Azure pretplate** pronađite mapu u koju ste spremili datoteku postavke objavljivanja odaberite datoteku, a zatim odaberite gumb **Uvezi** . To uvozi potvrde u datoteci postavki Objavi u Visual Studio. Sada trebali biste moći interakciju s privatnim oblaka resurse.

    ![Uvoz postavki objavljivanja](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

## <a name="next-steps"></a>Daljnji koraci

[Objavljivanje u Azure Oblaku iz Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

[Kako: preuzimanje i uvoz objavljivanje pretplate informacije i postavke](https://msdn.microsoft.com/library/dn385850(v=nav.70).aspx)

