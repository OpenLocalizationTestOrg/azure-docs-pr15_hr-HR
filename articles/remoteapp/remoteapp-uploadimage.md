
<properties
    pageTitle="Prijenos prilagođenu sliku za Azure RemoteApp | Microsoft Azure"
    description="Saznajte kako prenijeti prilagođenu sliku za Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="ericor" />



# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Prijenos prilagođenu sliku za Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Sad kad ste stvorili prilagođeni predložak sliku ili ste ažurirali promjenama, morate prijenos toj slici biblioteku na servisu Azure RemoteApp slike. Pomoću ovih koraka.


## <a name="before-you-start"></a>Prije početka

1.      Provjerite ispunjava prilagođenu sliku [preduvjeti slike](remoteapp-imagereqs.md) i [preduvjeti za aplikaciju](remoteapp-appreqs.md).
2.      Instalirajte [Modul Azure PowerShell](../powershell-install-configure.md).

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Korak po korak na kako prenijeti prilagođenu sliku

1.      Otvorite Portal za upravljanje Azure i idite na stranicu RemoteApp.
2.      Na kartici **Slika predložaka** kliknite **Prenesi** pri dnu stranice.
4.      Upišite neslužbeni naziv za sliku, a zatim navedite mjesto za pohranu računa. Provjerite je li se nalaziti na isto mjesto vaše zbirke RemoteApp ili neko mjesto na koje želite stvoriti.
5.      Kada se to od vas zatraži, preuzmite skripte na lokalnom Računalu.
6.      Kopirati parametara naredbenog u tekstni okvir u međuspremnik.
7.      Otvorite prozor programa povećane Windows PowerShell.
8.      U prozoru povećane komponente Windows PowerShell, idite do isti koju ste preuzeli skriptu.
9.      Lijepljenje kopiranog naredbu i pritisnite **Enter**.

    Započet će prijenos postupak i trajanje možda ovise o mnogo je čimbenika uključujući brzini mreže i veličina slike

11.    Ako prijenos nije uspjela zbog prekida mreže ili elemente tako, uvijek možete nastaviti postupak prijenos ste je počeli. Da biste nastavili za prijenos pokrenuti skriptu ponovno korištenje isti naredbeni redak.

> [AZURE.WARNING] Nikad ne izmijeniti skripte prijenos. Imate određene provjere implementirana da biste bili sigurni ispunjava li slika preduvjeti slike i preduvjeti za aplikaciju.

## <a name="common-problems"></a>Uobičajeni problemi

- Provjerite je li pomoću komponente Windows PowerShell Azure ljuske PowerShell. Morate instalirati modul Azure PowerShell jer su vam potrebne određene moduli tijekom učitavanja.
- Nikad ne alter skriptu, provjera valjanosti postoje praktičan.
- Ako datoteku vhd dobiva zaključan tijekom prijenosa, kopirajte datoteku ili je premjestite na novu lokaciju i pokušaj prijenos ponovno. Možda postoje neka proces sustava Windows koji sprječava prijenos.  
