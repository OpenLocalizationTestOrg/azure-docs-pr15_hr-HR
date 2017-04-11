<properties
    pageTitle="Rješavanje problema: Stvaranje i povezivanje s radnim prostorom strojnog učenja | Microsoft Azure"
    description="Rješenja uobičajenih problema u stvaranju i povezivanje s radnim prostorom Azure strojnog učenja"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="garye"/>


# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Vodič za otklanjanje poteškoća: Stvaranje i povezivanje s radnim prostorom strojnog učenja

Ovaj vodič sadrži rješenja za neke često naišao na izazove prilikom postavljanja radne prostore Azure strojnog učenja.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Vlasnik radnog prostora

Prilikom stvaranja novog radnog prostora strojnog učenja ID unesete u polje Vlasnik radnog prostora mora biti Microsoftova računa (prijašnji Windows Live ID), na primjer, john-contoso@live.com ili john-contoso@hotmail.com. Ne može biti koje nisu Microsoftova računa, kao što je račun e-pošte tvrtke. Da biste stvorili besplatan Microsoftov račun, idite na [www.live.com](http://www.live.com).

Imajte na umu da račun koji ste koristili za prijavu na portal za Azure da biste stvorili radni prostor automatski nemate dozvolu za *Otvaranje* tog radnog prostora ako ne navedete taj račun kao vlasnik. Da biste otvorili radni prostor u strojnog učenja Studio, morate biti prijavljeni Microsoftovim Account koja je definirana kao vlasnik radnog prostora ili morate primiti pozivnicu od vlasnika da biste se uključili u radni prostor. Na portalu Azure klasični možete, međutim, *Upravljanje* radni prostor, što obuhvaća mogućnost promjena vlasnika i konfiguriranje programa access.

Dodatne informacije o upravljanju radnog prostora potražite u članku [Upravljanje radnog prostora programa Azure strojnog učenja].

[Upravljanje programa Azure strojnog učenja radnog prostora]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Dopuštene regije

Strojnog učenja trenutno nije dostupno u ograničen broj područja. Ako vaša pretplata obuhvaća jednu od ovih područja, vidjet ćete poruku o pogrešci, "Imate bez pretplate u dopuštene regijama."

Da biste zatražili da područje dodati vašoj pretplati, odaberite **Obratite se podršci Microsoft** na portalu klasični Azure, odaberite **naplata** kao vrstu problema i slijedite upute da biste poslali zahtjev.

![Obratite se Microsoftovoj podršci][screen1]

## <a name="storage-account"></a>Račun za pohranu

Servis strojnog učenja mora prostora za pohranu računa radi pohrane podataka. Možete koristiti postojeći račun za pohranu ili stvorite novi račun za pohranu prilikom stvaranja novog radnog prostora za učenje računalo (Ako imate kvote da biste stvorili novi prostor za pohranu račun).

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Stvaranje radnog prostora][screen2]

Nakon stvaranja novog radnog prostora za strojnog učenja se možete prijaviti na računalu učenje Studio pomoću Microsoftova računa koje ste naveli kao vlasnik radnog prostora. Ako se pojavi poruka o pogrešci, "Radni prostor nije pronađen" (slično sljedeće snimka), koristite sljedeće korake da biste izbrisali kolačići preglednika.

![Radni prostor nije pronađen][screen3]

**Da biste izbrisali kolačići preglednika**

Ako koristite Internet Explorer, kliknite gumb **Alati** u gornjem desnom kutu i odaberite **Internetske mogućnosti**.  

![Internetske mogućnosti][screen4]

Na kartici **Općenito** kliknite **Izbriši...**

![Na kartici Općenito][screen5]

U dijaloškom okviru **Brisanje povijesti pregledavanja** obavezno **Kolačići i podaci s web-mjesta** , a zatim kliknite **Izbriši**.

![Brisanje kolačića][screen6]

Nakon što izbrišete kolačiće, ponovno pokrenite preglednik, a zatim prijeđite na stranici [Microsoft Azure strojnog učenja](https://studio.azureml.net) . Kada se pojavi upit za korisničko ime i lozinku, unesite isti Microsoftov račun koje ste naveli kao vlasnik radnog prostora.

Naš cilj je strojnog učenja ostvarivanje objedinjenog, kao što je moguće. Ponovno objavite sve komentare i pitanja na [forumu Azure strojnog učenja](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) omogućuju vam poslužiti bolje.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
