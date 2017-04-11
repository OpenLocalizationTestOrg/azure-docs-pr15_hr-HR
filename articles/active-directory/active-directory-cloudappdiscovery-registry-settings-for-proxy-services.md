<properties 
    pageTitle="Postavke registra aplikacije otkrivanja Proxy servisa u oblaku | Microsoft Azure" 
    description="Cilj u ovoj se temi je da vam pruži korake morate ponovno postaviti priključak računala s programom agent za otkrivanje aplikacije oblaka." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Postavke registra aplikacije otkrivanje oblaka za servise proxy poslužitelja

Prema zadanim postavkama, agent za otkrivanje aplikacije oblak je konfiguriran za korištenje samo priključke 80 i 443. Ako namjeravate instalirate otkrivanje oblak aplikacije u okruženju s proxy poslužitelj koji koristi prilagođene priključak (80 ni 443), morate konfigurirati svoje agente za korištenje tog priključka. Konfiguracija temelji se na ključ registra.


Cilj u ovoj se temi je da vam pruži korake morate ponovno postaviti priključak računala s programom agent za otkrivanje aplikacije oblaka.



**Da biste izmijenili priključak koji se koristi na računalu pokrenut agent za otkrivanje aplikacije oblaka, učinite sljedeće:**


1. Pokrenite uređivač registra. <br> ![Pokreni](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)

2. Otvorite ili stvorite sljedećeg ključa registra: <br> **Discovery\Endpoint HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud aplikacije** 

3. Stvorite novu vrijednost **niza više** naziva **priključci**. ![Novi](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)

4. Da biste otvorili dijaloški okvir **Uređivanje više nizova** , dvokliknite priključke vrijednost.


5. U tekstni okvir podaci vrijednosti, unesite sljedeće vrijednosti, a zatim dodajte sve prilagođene priključke koji se koriste u vašoj tvrtki ili ustanovi: <br><br>
**80** <br>
**8080** <br>
**8118** <br>
**8888** <br>
**81** <br>
**12080** <br>
**6999** <br>
**30606** <br>
**31595** <br>
**4080** <br>
**443** <br>
**broj 1110** <br><br>
![Uređivanje više nizova](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)

6. Kliknite **u redu** da biste zatvorili dijaloški okvir **Uređivanje više nizova** .



**Dodatni resursi**


* [Kako otkriti unsanctioned oblaka aplikacije koje se koriste u tvrtki ili ustanovi](active-directory-cloudappdiscovery-whatis.md) 


