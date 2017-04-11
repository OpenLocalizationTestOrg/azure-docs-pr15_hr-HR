<properties
   pageTitle="Dodavanje aplikacije Vatrozid za web u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pokazuje kako implementirati preporuke centar za sigurnost Azure **Finalize aplikacije zaštita**i **Dodavanje aplikacija Vatrozid za web** ."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Dodavanje aplikacije Vatrozid za web u centru za sigurnost Azure

Centar za sigurnost Azure preporučuje se da dodate aplikaciju vatrozid web (WAF) iz programa Microsoft partner da bi se osigurala web-aplikacije. Ovaj dokument vodit će vas kroz primjera kako to učiniti.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer.  Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. U plohu **preporuke** odaberite **sigurne web-aplikacije pomoću vatrozida aplikaciju za web**.
![Sigurno web-aplikacije][1]

2. U plohu **sigurne web-aplikacija web aplikacije vatrozidom** odaberite web-aplikaciju. Otvorit će se plohu **Dodaj vatrozid aplikaciju za Web** .
![Dodavanje aplikacije Vatrozid za web][2]
3. Možete odabrati da biste koristili postojeću web Vatrozid za računala ako je dostupna, ili stvorite novi. U ovom primjeru dostupnih bez postojeće WAFs pa ćemo ćete stvoriti novi WAF.

4. Da biste stvorili novi WAF, odaberite rješenje na popisu integrirani partnera. U ovom primjeru smo ćete odabrati **Barracuda Web aplikacije vatrozid**.
5. Otvorit će se plohu **Barracuda Web aplikacije vatrozid** dajući informacije o rješenja partnera. Odaberite **Stvori** plohu informacije.
![Plohu informacije vatrozid][3]

6. Plohu **Novi Vatrozid Web aplikacije** otvorit će se, gdje možete provesti navedeni koraci za **Konfiguraciju VM** i navedite **WAF podatke**. Odaberite **VM konfiguracija**.

7. U **Konfiguraciji VM** plohu unesite potrebne informacije za Okretni gore virtualnog računala koji će se izvoditi na WAF.
![Konfiguriranje VM][4]
8. Vratite plohu **Novi Vatrozid Web aplikacije** i odaberite **WAF informacije**. U plohu **WAF informacije** konfigurirajte WAF sam. Korak 7 omogućuje vam da biste konfigurirali virtualnog računala na kojem će se izvoditi na WAF i korak 8 omogućuje Dodjela WAF sam.

## <a name="finalize-application-protection"></a>Dovršavanje zaštite računala

1. Vratite se u plohu **preporuke** . Nova stavka generirana nakon stvaranja WAF, **Finalize**zaštite računala. Stavka obavještava da morate dovršiti postupak zapravo linije gore WAF unutar Azure virtualne mreže tako da ga možete zaštititi aplikaciju.
![Dovršavanje zaštite računala][5]

2. Odaberite **Zaštita Finalize aplikacije**. Otvorit će se novi plohu. Vidjet ćete da je web-aplikacija koje je potrebno imati njegov promet preusmjereni.
3. Odaberite web-aplikaciju. Otvorit će se na plohu koje vam upute za dovršavanje Postava vatrozid aplikacije za web. Dovršite korake, a zatim odaberite **ograničenog promet**. Centar za sigurnost pa učinite linije kopirane umjesto vas.
![Ograničavanje promet][6]

> [AZURE.NOTE] Dodavanjem te aplikacije na vaše postojeće WAF implementacijama možete zaštititi više web-aplikacija u centru za sigurnost. WAF aparata (stvorena pomoću modela implementaciju upravljanja resursima) moraju biti implementirano u zasebnom virtualne mreže. WAF aparata (stvorena pomoću modela uvođenje klasičnog) su ograničeni na korištenje mreže sigurnosne grupe. Podrška za ovaj će se proširiti da biste potpuno Prilagođeno implementacije potražite WAF (klasični) u budućnosti. Dodatne informacije o [Klasični i resursima implementacije modela](../azure-classic-rm.md) za Azure resurse.

U zapisnicima iz tog WAF sada posve integriran. Centar za sigurnost možete pokrenuti automatski prikupljanje i analiza zapisnike tako da vam koja može ponuditi važnih sigurnosnih upozorenja.

## <a name="see-also"></a>Vidi također

Ovaj dokument prikazivao kako implementirati preporuke centar za sigurnost "Dodavanje web-aplikaciji." Dodatne informacije o konfiguriranju Vatrozida aplikaciju za web, pogledajte sljedeće:

- [Konfiguriranje aplikacije vatrozid Web (WAF) za aplikacije servisa za okruženje](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Nadzor sigurnost stanja u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/) – blog traženje članaka o Azure sigurnost i usklađenost

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
