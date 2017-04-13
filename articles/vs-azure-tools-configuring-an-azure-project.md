<properties
   pageTitle="Konfiguriranje programa Project servisa Azure oblaka uz Visual Studio | Microsoft Azure"
   description="Saznajte kako konfigurirati za projekt servisa Azure oblak u Visual Studio, ovisno o zahtjevima za taj projekt."
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

# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Konfiguriranje programa Project servisa Azure oblaka uz Visual Studio

Možete konfigurirati projekt servisa Azure oblaka, ovisno o zahtjevima za taj projekt. Možete postaviti svojstva projekta za sljedećih kategorija:

- **Objavljivanje na servis u oblaku za Azure**

  Možete postaviti svojstva da biste bili sigurni da postojeći oblaka servis implementiran na Azure ne slučajno izbrisali.

- **Pokretanje ili neki servis u oblaku na lokalnom računalu za ispravljanje pogrešaka**

  Možete odabrati konfiguracija servisa za korištenje, a zatim odredite želite li pokrenuti emulator Azure prostora za pohranu.

- **Provjerite valjanost paket servisa oblaka prilikom stvaranja**

  Možete odlučiti Smatraj sva upozorenja pogreške tako da možete biti sigurni da će paket servisa oblaka implementacija bez problema. Time se smanjuje vrijeme čekanja ako uvođenje i otkrivanje da pojavila se pogreška.

Sljedeća ilustracija prikazuje kako odabrati konfiguracije ćete koristiti prilikom pokretanja ili na servis u oblaku lokalno za ispravljanje pogrešaka. Svojstva projekta koje su vam potrebne u tom prozoru možete postaviti kao što je prikazano na slici.

![Konfiguriranje Microsoft Azure projekta](./media/vs-azure-tools-configuring-an-azure-project/IC713462.png)

## <a name="to-configure-an-azure-cloud-service-project"></a>Da biste konfigurirali za projekt servisa Azure oblaka

1. Da biste konfigurirali oblaka servisa projekta iz **Programa Explorer rješenja**, otvorite izbornički prečac za projekt servisa oblaka, a zatim odaberite **Svojstva**.

  U uređivaču za Visual Studio pojavit će se stranica s nazivom servisa project oblaka.

1. Odaberite karticu **razvoj** .

1. Da biste bili sigurni da ne slučajno izbrišete postojeće implementacije u Azure, u upit prije brisanja postojećeg popisa implementaciju, odaberite **True**.

1. Da biste odabrali konfiguracija servisa koji želite koristiti kada pokrenete ili ispravljanje pogrešaka na servis u oblaku lokalno, na popisu **Konfiguracija servisa** odaberite konfiguraciju servisa.

  >[AZURE.NOTE] Ako želite stvoriti konfiguracija servisa za korištenje, pročitajte članak Kako: Upravljanje konfiguracija servisa i profile. Ako želite da biste izmijenili konfiguracija servisa za uloge, potražite u članku [kako konfigurirati ulogama za Azure oblaku s Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Da biste pokrenuli emulator Azure prostora za pohranu kada pokrenete ili na servis u oblaku lokalno, za ispravljanje pogrešaka u **emulator prostora za pohranu za pokretanje Azure**odaberite **True**.

1. Da biste bili sigurni da nije moguće objaviti ako postoje pogreške provjere valjanosti paket, u **zbriši upozorenja kao pogreške**, odaberite **True**.

1. Da biste bili sigurni da vaša uloga web koristi isti priključak prilikom svakog pokretanja lokalno u IIS Express u **priključke za projekt koristi web**odaberite **True**. Da biste koristili određeni priključak određenom web-mjestu projekta, otvorite izbornički prečac za web project, odaberite karticu **Svojstva** odaberite karticu **Web** i promijeniti broj priključka u postavci **Url projekta** u odjeljku **IIS Express** . Na primjer, unesite `http://localhost:14020` kao URL projekta.

1. Da biste spremili sve promjene koje ste načinili svojstva servisa projekta oblaka, odaberite gumb **Spremi** na alatnoj traci.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o konfiguriranju projekata servisa Azure oblak u Visual Studio potražite u članku [Konfiguriranje Azure projektu pomoću više konfiguracija servisa](vs-azure-tools-multiple-services-project-configurations.md).
