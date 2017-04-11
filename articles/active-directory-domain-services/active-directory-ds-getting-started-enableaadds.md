<properties
    pageTitle="Azure servisa Active Directory Domain Services: Omogućivanje Azure servisa Active Directory Domain Services | Microsoft Azure"
    description="Uvod u Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="enable-azure-ad-domain-services"></a>Omogućivanje Azure servisa Active Directory Domain Services

## <a name="task-3-enable-azure-ad-domain-services"></a>Zadatak 3: Omogućivanje Azure servisa Active Directory Domain Services
U ovom ćete zadatku najprije omogućiti Azure servisa Active Directory Domain Services za direktorija. Izvršite sljedeće korake konfiguracije da biste omogućili Azure servisa Active Directory Domain Services za direktorija.

1. Dođite do **Azure klasični portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Odaberite čvor **Servisa Active Directory** u lijevom oknu.

3. Odaberite klijenta Azure AD (imenik) za koju želite omogućiti Azure servisa Active Directory Domain Services.

    ![Odabir Azure AD direktorija](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Kliknite karticu **Konfiguracija** .

    ![Konfiguriranje kartica direktorija](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Pomaknite se prema dolje do odjeljka pod naslovom **domenske servise**.

    ![Sekciji konfiguracija servisa za domene](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. Uključivanje ili isključivanje mogućnosti pod naslovom **omogućiti domenske servise za taj imenik** na **da**. Obratite pozornost nekoliko dodatne mogućnosti konfiguracije za Azure servisa Active Directory Domain services pojavljuje na stranici.

    ![Omogućivanje domenske servise](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] Kada omogućite Azure AD domenske servise za vaš klijent, Azure AD stvara i sprema raspršivanja vjerodajnica Kerberos i NTLM koji su potrebni za provjeru autentičnosti korisnika.

7. Navedite **naziv domene DNS domenske servise**.

   - Zadani naziv domene direktorija (to jest, završavaju na **. onmicrosoft.com** nastavak domene) po zadanom je potvrđen.

   - Popis sadrži sve domene koje nije konfiguriran direktorija Azure AD – uključujući provjeriti kao i Neprovjereni domene koje se konfigurirati na kartici "Domene".

   - Osim toga, možete upisati i prilagođeni naziv domene. U ovom primjeru smo ste unijeli u prilagođenog naziva domene "contoso100.com".

     > [AZURE.WARNING] Provjerite je li domena prefiks naziv domene koju navedete (na primjer, contoso100 naziv domene 'contoso100.com') manje od 15 znakova. Ne možete stvoriti programa Azure AD domene domenskih servisa s prefiksom domene više od 15 znakova.

8. Provjerite je li da DNS naziv domene koji ste odabrali za upravljane domene ne postoji u virtualne mreže. Konkretno, potvrdite ako:

   - već imate domenu s istim nazivom domene DNS-a na virtualne mreže.

   - virtualne mreže ste odabrali je VPN veza lokalne mreže, a imate domenu s istim nazivom domene DNS-a na lokalnu mrežu.

   - postojeći servisi oblaka s tim nazivom uključeno virtualne mreže.

9. Sljedeći je korak da biste odabrali virtualne mreže u kojoj želite Azure servisa Active Directory Domain Services da bi bio dostupan. Odaberite virtualne mreže i namjenski podmreže koju ste stvorili u padajućem popisu pod naslovom **domenske servise za povezivanje s mrežom virtualne**.

   - Provjerite je li virtualne mreže koje ste naveli pripada programa Azure regija podržava Azure servisa Active Directory Domain Services. Odnose se na stranicu [servisa Azure po regijama](https://azure.microsoft.com/regions/#services/) znati Azure regije u kojoj je dostupan Azure servisa Active Directory Domain Services.

   - Virtualne mreže pripadaju područja kojima se Azure servisa Active Directory Domain Services ne podržava neće se prikazivati na padajućem popisu.
   
   - Korištenje namjenski podmreže unutar virtualne mreže za Azure servisa Active Directory Domain Services. Provjerite ne odaberete podmreže pristupnika. U odjeljku [mreža pitanja vezana uz](active-directory-ds-networking.md). 

   - Isto tako, virtualne mreže koje su stvorene pomoću upravitelja resursa Azure ne prikazuju na padajućem popisu. Voditelj resursa sustavom virtualne mreže ne trenutno podržava Azure servisa Active Directory Domain Services.

10. Da biste omogućili Azure servisa Active Directory Domain Services, iz okna zadatka pri dnu stranice kliknite **Spremi** .

11. Na stranici prikazuju se na ' na čekanju... " stanje, dok je u tijeku Azure servisa Active Directory Domain Services omogućena za direktorija.

    ![Omogućivanje domenske servise – na čekanju stanja](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Azure servisa Active Directory Domain Services nudi visoke dostupnosti za upravljane domene. Kada omogućite Azure servisa Active Directory Domain Services, imajte na umu IP adresa na kojoj su dostupne na virtualne mreže prikazuju se jedan po jedan domenske servise. Drugi IP adresa prikazuje se uskoro, kao što je uskoro servis omogućuje visoke dostupnosti za svoju domenu. Kada visoke dostupnosti je konfiguriran i aktivni za vašu domenu, trebali biste vidjeti dva IP adresa u odjeljku **domenske servise** na kartici **Konfiguriraj** .

12. Nakon otprilike 20 – 30 minuta, pročitajte članak prvi IP adresu na kojoj je dostupan na mreži virtualne u polje **IP adresa** na stranici **Konfiguracija** domenske servise.

    ![Resursi za Domain Services omogućena – prvi IP](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

13. Kada je visoke dostupnosti radu sa servisom za vašu domenu, vidjet ćete dva IP adresa koji se prikazuje na stranici. Upravljani domene dostupan je na odabrani virtualne mreže na ove dvije IP adrese. Imajte na umu dolje IP adrese tako da možete ažurirati postavke DNS-a za virtualne mreže. Taj korak omogućuje virtualnim strojevima na virtualne mreže za povezivanje s domenu za operacija kao što su domene spoja.

    ![Domain Services omogućena – resursi za oba IP-ovi](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] Ovisno o veličini klijentu za Azure AD (broj korisnika, grupa etc.), sinkronizacija upravljanih domene potrebno neko vrijeme. U ovom postupku sinkronizacije se događa u pozadini. Za velike korisnicima s tens tisuće objekata, može proći dan do dva za sve korisnike, članstva u grupi i vjerodajnice za sinkronizaciju.

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Zadatak 4 – ažuriranje DNS postavki za Azure virtualne mreže
Sljedeći zadatak konfiguracije je [ažuriranje DNS postavki za Azure virtualne mreže](active-directory-ds-getting-started-dns.md).
