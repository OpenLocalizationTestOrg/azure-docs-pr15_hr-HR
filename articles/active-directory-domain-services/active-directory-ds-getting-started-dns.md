<properties
    pageTitle="Azure servisa Active Directory Domain Services: Ažuriranje DNS postavki za Azure virtualne mreže | Microsoft Azure"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Azure servisa Active Directory Domain Services – ažuriranje DNS postavki za Azure virtualne mreže

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Zadatak 4: Ažuriranje DNS postavki za Azure virtualne mreže
Na prethodni zadatke konfiguracije uspješno ste omogućili Azure servisa Active Directory Domain Services za direktorija. Sljedeći zadatak je da biste bili sigurni da će računala u virtualne mreže možete povezati i korištenje tih servisa. Ažurirajte postavke DNS poslužitelja za virtualne mreže na dva IP adresa na kojoj je dostupan na virtualne mreže Azure servisa Active Directory Domain Services.

> [AZURE.NOTE] Imajte na umu dolje IP adresa za Azure AD domenske servise koji se prikazuju na kartici **Konfiguriraj** direktorija, nakon što ste omogućili Azure AD domenske servise za direktorij.

Izvršite sljedeće korake konfiguracije da biste ažurirali postavku DNS poslužitelja za virtualne mreže u kojima ste omogućili Azure servisa Active Directory Domain Services.

1. Dođite do **Azure klasični portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Odaberite čvor **mreža** u lijevom oknu.

    ![Čvor virtualne mreže](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. Na kartici **Virtualne mreže** odaberite virtualne mreže omogućena Azure servisa Active Directory Domain Services da biste pogledali svojstva.

4. Kliknite karticu **Konfiguracija** .

    ![Čvor virtualne mreže](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. U odjeljku **DNS poslužitelja** unesite IP adrese Azure servisa Active Directory Domain Services.

6. Provjerite je li unesete i IP adresama koje su prikazane u odjeljku **Domain Services** na kartici **Konfiguriraj** direktorija.

7. Da biste spremili postavke DNS poslužitelja za virtualne mreže, u oknu zadatka pri dnu stranice kliknite **Spremi** .

   ![Ažuriranje DNS postavki poslužitelja za virtualne mreže.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] Nakon ažuriranja DNS postavke poslužitelja za virtualne mreže može potrajati neko vrijeme da se virtualnim strojevima na mrežu da biste dobili ažurirani DNS konfiguraciju. Ako je virtualnog računala ne može povezati s domenom, koje možete ispraznite DNS predmemoriju (npr. ' ipconfig/flushdns da biste') na virtualnog računala. Ta se naredba navodi osvježavanje postavke DNS-a na virtualnog računala.


## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>Zadatak 5 - Omogući sinkronizaciju lozinke Azure servisa Active Directory Domain Services
Sljedeći zadatak konfiguracije je [omogućiti sinkronizaciju lozinke za Azure servisa Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md).
