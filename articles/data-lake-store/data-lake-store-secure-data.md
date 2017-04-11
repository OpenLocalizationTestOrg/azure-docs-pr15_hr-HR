<properties 
   pageTitle="Zaštita podataka pohranjenih u spremištu Lake podataka za Azure | Microsoft Azure" 
   description="Saznajte kako radi zaštite podataka u spremištu Lake podataka za Azure pomoću grupe i pristup popisi za upravljanje" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/29/2016"
   ms.author="nitinme"/>

# <a name="securing-data-stored-in-azure-data-lake-store"></a>Zaštita podataka pohranjenih u spremištu Lake podataka za Azure

Zaštita podataka u spremištu Lake podataka za Azure je tri koraka pristup.

1. Započnite stvaranjem sigurnosne grupe u Azure Active Directory (AAD). Ove sigurnosne grupe koriste se za implementaciju kontrola pristupa na temelju uloga (RBAC) Azure portalu. Dodatne informacije potražite u članku [Kontrola pristupa na temelju uloga u Microsoft Azure](../active-directory/role-based-access-control-configure.md).

2. Dodijelite sigurnosne grupe AAD Lake spremišta podataka za Azure računu. To kontrolira pristup računu spremišta podataka Lake portal za upravljanje i operacija s portala i API-ji.

3. Dodijelite sigurnosne grupe AAD kao pristup kontrola popisi pristupom migriraju u datotečnom sustavu spremišta Lake podataka.

4. Uz to, raspon IP adresa možete postaviti za klijente koji može pristupiti podacima u spremištu Lake podataka.

Ovaj članak sadrži upute o korištenju Azure portal za izvođenje iznad zadataka. Detaljne informacije o kako implementira spremišta podataka Lake sigurnosti na razini račun i podataka potražite u članku [Sigurnost u spremištu Lake za Azure podataka](data-lake-store-security-overview.md). Obuhvaća više razina dive informacije na kako se primjenjuju ACL-a u spremištu Lake za Azure podataka potražite u članku [Pregled od kontrola pristupa u spremištu Lake podataka](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).
- **Račun programa Azure spremišta Lake podataka**. Upute o tome kako ga stvoriti potražite u članku [Početak rada s Lake spremišta podataka za Azure](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Stvaranje sigurnosne grupe u servisu Azure Active Directory

Upute o stvaranju AAD sigurnosne grupe i dodavanje korisnika u grupu, potražite u članku [Upravljanje sigurnosnim grupama u servisu Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Dodijelite korisnike ili sigurnosne grupe računima Lake spremišta podataka za Azure

Kada dodjeljujete korisnike ili sigurnosne grupe računi Azure podataka Lake trgovine, upravljanje pristupom operacija upravljanja na račun pomoću Azure portal i API Azure Voditelj resursa. 

1. Otvorite račun za Azure podataka Lake trgovine. U lijevom oknu kliknite **Pregledaj**, kliknite **Trgovina Lake podataka**i zatim plohu spremišta Lake podataka kliknite naziv računa koji želite dodijeliti korisnika ili sigurnosne grupe.

2. U vašem plohu računa spremišta Lake podataka kliknite **Postavke**. Plohu **Postavke** kliknite **korisnika**.

    ![Dodjeljivanje sigurnosne grupe račun Lake spremišta podataka za Azure] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Dodjeljivanje sigurnosne grupe račun Lake spremišta podataka za Azure")

3. Plohu **korisnika** po zadanom prikazuje grupe **Administratori pretplate** vlasnik. 

    ![Dodavanje korisnika i uloga] (./media/data-lake-store-secure-data/adl.add.group.roles.png "Dodavanje korisnika i uloga")
 
    Da biste dodali grupe i dodijelite relevantnih uloga na dva načina.

    * Dodavanje korisnika/grupe na račun, a zatim dodijelite uloge, ili
    * Dodavanje uloge, a zatim dodijelite korisnika/grupe uloga.

    U ovom ćete odjeljku smo pogledajte prvom pristupu Dodavanje grupe i dodjela dozvola. Na raspolaganju vam slične korake da biste najprije odaberite uloge, a zatim grupe dodijeliti uloge.
    
4. U plohu **korisnika** , kliknite **Dodaj** da biste otvorili plohu **Dodaj pristup** . U plohu **Dodaj access** kliknite **Odaberite ulogu**pa odaberite ulogu za korisnika/grupe.

     ![Dodavanje uloge za korisnika] (./media/data-lake-store-secure-data/adl.add.user.1.png "Dodavanje uloge za korisnika")

    **Vlasnik** i ulogu **suradnika** omogućuje pristup različite funkcije za administraciju na računu lake podataka. Za korisnike koji će interakciju s podacima u lake podataka, možete ih dodati ulozi **čitač **. Opseg te uloge ograničeno je na operacija upravljanja vezane uz račun za Azure podataka Lake trgovine.

    Za podatke operacije pojedinačne datoteke sustava dozvole definirati što možete učiniti korisnika. Dakle, korisnik ima ulozi čitač možete vidjeti samo administratorske postavke povezanu s računom, ali možete potencijalno čitanje i pisanje podataka na temelju dodijeljena dozvola za datoteku sustava. Pri [dodijeliti sigurnosne grupe kao ACL-a u datotečni sustav Lake spremišta podataka za Azure](#filepermissions)opisane su dozvole za spremište podataka Lake datoteke sustava.



5. U plohu **Dodaj access** kliknite **Dodavanje korisnika** da biste otvorili plohu **Dodavanje korisnika** . U ovom plohu potražite sigurnosne grupe koji ste prethodno stvorili Azure Active Directory. Ako imate mnogo grupa za pretraživanje, koristiti tekstni okvir na vrhu da biste filtrirali naziv grupe. Kliknite **Odaberi**.

    ![Dodavanje sigurnosne grupe] (./media/data-lake-store-secure-data/adl.add.user.2.png "Dodavanje sigurnosne grupe")

    Ako želite da biste dodali grupe/korisnika koji nije naveden, možete ih pozvati pomoću ikona **Pozovi** i navođenjem adresu e-pošte za korisnika/grupe.

6. Kliknite **u redu**. Trebali biste vidjeti sigurnosnu grupu dodali kao što je prikazano u nastavku.

    ![Sigurnosna grupa dodali] (./media/data-lake-store-secure-data/adl.add.user.3.png "Sigurnosna grupa dodali")

7. Vaše korisnika i sigurnosne grupe sada ima pristup računu Lake spremišta podataka za Azure. Ako želite omogućuju pristup određene korisnike, možete ih dodati sigurnosne grupe. Isto tako, ako želite opozvati pristup za korisnika, možete ih ukloniti iz sigurnosne grupe. Možete dodijeliti i više sigurnosnih grupa s poslovnim subjektom. 

## <a name="filepermissions"></a>Dodjela korisnika ili sigurnosnu grupu kao ACL-a datotečnom sustavu Lake spremišta podataka za Azure

Dodjelom korisnika i sigurnosne grupe u datotečni sustav Lake Azure podataka postavite kontrola pristupa na podatke pohranjene u spremištu Lake za Azure podataka.

1. U vašem plohu računa spremišta Lake podataka kliknite **Explorer podataka**.

    ![Stvaranje imenika računa spremišta Lake podataka] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Stvaranje imenika Lake podataka računa")

2. U plohu **Explorer podataka** kliknite datoteku ili mapu za koju želite konfigurirati ACL-a, a zatim **programa Access**. Da biste dodijelili ACL datoteke, mora kliknuti **programa Access** iz plohu **Pretpregled datoteka** .

    ![Postavljanje ACL-a podataka Lake datotečnom sustavu] (./media/data-lake-store-secure-data/adl.acl.1.png "Postavljanje ACL-a podataka Lake datotečnom sustavu")

3. **Pristup** plohu navedeni standardne pristup i prilagođeni pristup već dodijelili korijenskom. Kliknite ikonu **Dodaj** da biste dodali ACL-a Prilagođena razina.

    ![Pristup standardnih ili prilagođenih popisa] (./media/data-lake-store-secure-data/adl.acl.2.png "Pristup standardnih ili prilagođenih popisa")

    * **Standardna pristup** je pristup UNIX stil koju navedete pročitano, pisanje, izvršavanje (rwx) za tri različita korisnika klasa: vlasnika, grupa i drugim korisnicima.
    * **Prilagođeni pristup** odgovara POSIX ACL-a koji omogućuje postavljanje dozvola za određene imenovani korisnike ili grupe, i ne samo njezin vlasnik ili grupu. 
    
    Dodatne informacije potražite u članku [HDFS ACL-a](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Dodatne informacije o kako se primjenjuju ACL-a u spremištu Lake podataka, potražite u članku [Kontrola pristupa u spremištu Lake podataka](data-lake-store-access-control.md).

4. Kliknite ikonu **Dodaj** da biste otvorili plohu **Dodajte prilagođeni pristup** . U ovom plohu kliknite **Odabir korisnika ili grupu**, a zatim **Odaberite korisnika ili grupu** plohu potražite sigurnosne grupe koji ste prethodno stvorili Azure Active Directory. Ako imate mnogo grupa za pretraživanje, koristiti tekstni okvir na vrhu da biste filtrirali naziv grupe. Kliknite grupu koju želite dodati, a zatim kliknite **Odaberi**.

    ![Dodaj grupu] (./media/data-lake-store-secure-data/adl.acl.3.png "Dodaj grupu")

5. Kliknite **Odaberite dozvole**odaberite dozvole i ima li želite dodijeliti dozvole kao zadano ACL pristup ACL ili i jedno i drugo. Kliknite **u redu**.

    ![Dodjela dozvola grupi] (./media/data-lake-store-secure-data/adl.acl.4.png "Dodjela dozvola grupi")

    Dodatne informacije o dozvolama u spremištu Lake podataka i zadane/pristup ACL-a potražite u članku [Kontrola pristupa u spremištu Lake podataka](data-lake-store-access-control.md).


6. U plohu **Dodajte prilagođeni pristup** kliknite **u redu**. Grupi novododani s dozvolama za povezane sada će se prikazati u **programu Access** plohu.

    ![Dodjela dozvola grupi] (./media/data-lake-store-secure-data/adl.acl.5.png "Dodjela dozvola grupi")

    > [AZURE.IMPORTANT] U trenutnom se izdanju možete imati samo 9 stavke u odjeljku **Prilagođeno pristup**. Ako želite dodati više od 9 korisnike potrebno stvoriti sigurnosnih grupa, dodati korisnike na sigurnosnih grupa, omogućuju pristup tim sigurnosne grupe za pohranu podataka Lake račun.

7. Ako je potrebno, možete mijenjati dozvole za pristup nakon dodavanja grupe. Isključite ili uključite potvrdni okvir za svaku vrstu dozvole (čitanje, pisanje, izvrši) ovisno o tome želite da biste uklonili ili dodijeliti ta dozvola sigurnosne grupe. Kliknite **Spremi** da biste spremili promjene ili **Odbaci** da biste poništili promjene.

## <a name="set-ip-address-range-for-data-access"></a>Postavljanje rasponu IP adresa za pristup podacima

Spremište Lake podataka za Azure omogućuje daljnje zaključavanje pristup podacima iz trgovine na razini mreže. Možete omogućiti vatrozid, odredili IP adresu ili definirali raspon IP adresa za pouzdanih klijentima. Kada je omogućen, samo klijenti IP adrese unutar određenog raspona možete povezati s spremište.

![Postavke vatrozida i pristup IP] (./media/data-lake-store-secure-data/firewall-ip-access.png "Postavke vatrozida i IP adrese")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Uklanjanje sigurnosne grupe za račun Lake spremišta podataka za Azure

Kada uklonite sigurnosne grupe s računa spremišta Lake podataka za Azure, mijenjate samo pristup za upravljanje operacije na račun pomoću Azure Portal i Azure resursima API-ji.

1. U vašem plohu računa spremišta Lake podataka kliknite **Postavke**. Plohu **Postavke** kliknite **korisnika**.

    ![Dodjeljivanje sigurnosne grupe Azure podataka Lake račun] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Dodjeljivanje sigurnosne grupe Azure podataka Lake račun")

2. U plohu **korisnika** kliknite sigurnosne grupe koje želite ukloniti.

    ![Sigurnosne grupe da biste uklonili] (./media/data-lake-store-secure-data/adl.add.user.3.png "Sigurnosne grupe da biste uklonili")

3. U plohu za sigurnosnu grupu, kliknite **Ukloni**.

    ![Sigurnosna grupa ukloniti] (./media/data-lake-store-secure-data/adl.remove.group.png "Sigurnosna grupa ukloniti")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Uklanjanje sigurnosne grupe ACL-a iz spremišta Lake podataka za Azure datotečnom sustavu

Kada uklonite sigurnosne grupe ACL-a iz spremišta Lake podataka za Azure datotečnog sustava, promjena pristupa s podacima u spremištu Lake podataka.

1. U vašem plohu računa spremišta Lake podataka kliknite **Explorer podataka**.

    ![Stvaranje imenika Lake podataka računa] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Stvaranje imenika Lake podataka računa")

2. U plohu **Explorer podataka** kliknite datoteku ili mapu za koju želite ukloniti ACL-a, a zatim plohu vaš račun, kliknite ikonu **programa Access** . Da biste uklonili ACL datoteke, mora kliknuti **programa Access** iz plohu **Pretpregled datoteka** .

    ![Postavljanje ACL-a podataka Lake datotečnom sustavu] (./media/data-lake-store-secure-data/adl.acl.1.png "Postavljanje ACL-a podataka Lake datotečnom sustavu")

3. U plohu **programa Access** , u odjeljku **Prilagođeno Access** kliknite sigurnosne grupe koje želite ukloniti. U plohu **Pristup Prilagođeno** kliknite **Ukloni** , a zatim kliknite **u redu**.

    ![Dodjela dozvola grupi] (./media/data-lake-store-secure-data/adl.remove.acl.png "Dodjela dozvola grupi")


## <a name="see-also"></a>Vidi također

- [Pregled Lake spremišta podataka za Azure](data-lake-store-overview.md)
- [Kopirajte podatke iz blob polja za pohranu Azure u spremištu Lake podataka](data-lake-store-copy-data-azure-storage-blob.md)
- [Korištenje analize Lake Azure podataka s spremišta Lake podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight pomoću spremišta Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Početak rada s spremišta Lake podataka pomoću komponente PowerShell](data-lake-store-get-started-powershell.md)
- [Početak rada s spremišta Lake podataka pomoću .NET SDK-a](data-lake-store-get-started-net-sdk.md)
- [Pristup dijagnostičke zapisnike za spremište Lake podataka](data-lake-store-diagnostic-logs.md)
