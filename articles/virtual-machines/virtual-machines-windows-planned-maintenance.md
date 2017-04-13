<properties
    pageTitle="Planirano održavanje za Windows VMs | Microsoft Azure"
    description="Razumijevanje koje Azure planiranog održavanja je i kako utječe na virtualnim računalima sustava Windows radi u Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="drewm"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="drewm"/>

# <a name="planned-maintenance-for-virtual-machines-in-azure"></a>Planirano održavanje virtualnim strojevima servisu Azure


Objašnjenje mogućnosti koje Azure planiranog održavanja je i kako može utjecati na dostupnost virtualnim strojevima sustava Windows. U ovom se članku i dostupna je za [virtualnim strojevima Linux](virtual-machines-linux-planned-maintenance.md). 

Ovaj članak sadrži pozadinu kao postupka Azure planiranog održavanja. Ako su zanima otkrijte zašto vaša VM sustava, možete [čitati ovom blogu objavu dovršenog prikaz VM ponovno zapisnika](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="why-azure-performs-planned-maintenance"></a>Zašto se izvodi Azure planirano održavanje

Microsoft Azure povremeno izvodi ažuriranja diljem svijeta da biste poboljšali pouzdanost, performanse i sigurnost infrastrukture glavnog računala koja je u pozadini virtualnim računalima. Mnoge od tih ažuriranja se izvode bez utjecaja na virtualnim strojevima ili servise u Oblaku, uključujući memorije očuvanje ažuriranja.

Međutim, neka ažuriranja zahtijevaju ponovno pokrenite računalo da biste virtualnim strojevima da biste primijenili ažuriranja koja su potrebna za infrastrukturu. Virtualnim strojevima se isključuje dok smo zakrpu Infrastruktura, a zatim ponovno pokrenete virtualnim računalima.

Primijetite da postoje dvije vrste održavanja koji mogu utjecati na dostupnost virtualnim strojevima: planirane i Neplanirana. Ova stranica opisuje način na koji Microsoft Azure izvodi planiranog održavanja. Dodatne informacije o neplanirano održavanja potražite u članku [objašnjenje planirano nasuprot neplanirano održavanja](virtual-machines-windows-manage-availability.md).

[AZURE.INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]
