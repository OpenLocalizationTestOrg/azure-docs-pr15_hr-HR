<properties 
   pageTitle="Putem udaljene radne površine Azure uloga | Microsoft Azure"
   description="Putem udaljene radne površine Azure uloga"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-remote-desktop-with-azure-roles"></a>Putem udaljene radne površine Azure uloga

Pomoću Azure SDK i udaljene radne površine možete pristupiti Azure uloge i virtualnim strojevima koji se nalaze Azure. U Visual Studio, možete konfigurirati udaljene radne površine iz Azure projekta. Da biste omogućili udaljene radne površine, morate stvoriti radni projekt koji sadrži jednu ili više uloga i objaviti Azure.

>[AZURE.IMPORTANT] Trebali biste pristupiti Azure uloge za otklanjanje poteškoća ili samo razvoj. Svrha svaki virtualnog računala je da biste pokrenuli određene uloga u programu za Azure ne da biste pokrenuli ostale klijentske aplikacije. Ako želite koristiti Azure za hostiranje virtualnog računala koje možete koristiti bilo koju svrhu potražite u članku pristup virtualnim računalima sustava Azure iz programa Explorer poslužitelja.

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Da biste omogućili i korištenje udaljene radne površine za ulogu Azure

1. U pregledniku rješenja, otvorite izbornik prečaca za projekt, a zatim **Objavi**.

    Pojavit će se čarobnjak za **Objavljivanje Azure aplikacije** .

    ![Naredba za projekt u Oblaku za objavljivanje](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)

1. Pri dnu stranice **Postavke objavljivanja za Microsoft Azure** čarobnjaka, odaberite **Omogući udaljene radne površine** za sve uloge. 

    Pojavit će se dijaloški okvir **Konfiguracija udaljene radne površine** .

1. Pri dnu dijaloškog okvira **Konfiguracije udaljene radne površine** , odaberite gumb **Dodatne mogućnosti** . 
 
    Prikazat će se okvir padajućeg popisa koje možete stvoriti ili odabrati certifikat tako da se podaci o vjerodajnicama možete šifrirati prilikom povezivanja putem udaljene radne površine.

1. Na padajućem popisu odaberite ** &lt;stvaranje >**, ili odaberite postojeći s popisa. 

    Ako odaberete postojeći certifikat, prijeđite na sljedeći način.

    >[AZURE.NOTE] Certifikati koje su vam potrebne za udaljene radne površine razlikuju se od certifikate koji koristite za druge Azure operacije. Certifikat za daljinski pristup mora imati privatni ključ.

    Pojavit će se dijaloški okvir **Stvaranje certifikata** .

    1. Navedite neslužbeni naziv certifikata, a zatim odaberite gumb **u redu** . Pojavit će se novi certifikat u okvir padajućeg popisa.

    1. U dijaloškom okviru **Konfiguriranje udaljene radne površine** navedite korisničko ime i lozinku.
    
        Ne možete koristiti postojeći račun. Ne navedete Administrator kao korisničko ime za novi račun.

        >[AZURE.NOTE] Ako lozinka ne zadovoljava preduvjete složenosti, crvenu ikonu prikazat će se pokraj tekstni okvir lozinka. Lozinka mora sadržavati velika slova, mala slova i brojeve ili simbole.

    1. Odaberite datum koji će isteći račun i iza koje udaljene radne površine veze bit će blokirane.

    1. Nakon što ste naveli sve potrebne informacije, odaberite gumb **u redu** .
    
        Nekoliko postavki koje omogućuju udaljene komponente Access Services dodaju se u .cscfg i .csdef datoteke.

1. U čarobnjaku za **Postavke objavljivanja za Microsoft Azure** odaberite gumb **u redu** kada budete spremni objaviti servis u oblaku.

    Ako niste spremni za objavljivanje, odaberite gumb **Odustani** . Postavke konfiguracije se spremaju, a servis u oblaku možete objaviti kasnije.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Povezivanje s ulogu Azure putem udaljene radne površine

Nakon što ste objavili na servis u oblaku na Azure, možete koristiti poslužitelj Explorer da se prijavite u virtualnim strojevima koji sadrži Azure. 

1. U programu Explorer poslužitelja Proširite čvor **Azure** , a zatim proširite čvor za servise u oblaku i neke njegove uloge da bi se prikazao popis instanci.

1. Otvaranje izbornika prečaca za programa čvor instancu, a zatim odaberite **Poveži pomoću udaljene radne površine**.

    ![Povezivanje putem udaljene radne površine](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)

1. Unesite korisničko ime i lozinku koju ste prethodno stvorili. Sada ste prijavljeni udaljenu sesiju.


