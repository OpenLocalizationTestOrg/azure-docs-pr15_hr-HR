<properties
    pageTitle="Instalirajte IIS na vaš prvi VM Windows | Microsoft Azure"
    description="Eksperimentirajte s prvom virtualnog računala Windows instalacijom IIS i otvaranje priključka 80 pomoću portala za Azure."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cynthn"/>

# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>Eksperimentirajte s instaliranjem uloge na VM sustava Windows
    
Nakon što dodate na prvu virtualnog računala (VM) prema gore i pokrenut, možete premjestiti instaliranje softvera i usluga. Za ovaj vodič smo namjeravate koristiti Upravitelj poslužitelja na poslužitelj VM Windows da biste instalirali IIS. Zatim ćemo stvoriti na mreži sigurnosne grupe (NSG) pomoću portala za Azure da biste otvorili priključak 80 IIS promet. 

Ako već niste stvorili vaš prvi VM, koje treba vratite da biste [stvorili prvu strojno virtualne Windows Azure portalu](virtual-machines-windows-hero-tutorial.md) prije nastavka ovog praktičnog vodiča.

## <a name="make-sure-the-vm-is-running"></a>Provjerite je li instaliran na VM

1. Otvorite [portal za Azure](https://portal.azure.com).
2. Na izborniku koncentrator kliknite **virtualnih računala**. S popisa odaberite virtualnog računala.
3. Ako je status **zaustavljen (Deallocated)**, kliknite gumb **Start** na plohu **Essentials** od na VM. Ako status nije **pokrenut**, možete premjestiti na sljedeći korak.

## <a name="connect-to-the-virtual-machine-and-sign-in"></a>Povežite se s virtualnog računala i prijavite se u

1.  Na izborniku koncentrator kliknite **virtualnih računala**. S popisa odaberite virtualnog računala.

3. Na plohu za virtualnog računala, kliknite **Poveži**. Stvara i preuzimanja protokola udaljene radne površine datoteke (.rdp datoteka) koje je kao prečac za povezivanje s računala. Možda želite spremiti datoteku na radnu površinu radi lakšeg pristupa. **Otvaranje** datoteka povezati svoje VM.

    ![Snimka zaslona s portala Azure pokazuje kako se povezati s vašeg VM](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. Prikazat će se upozorenje da je u .rdp Nepoznato izdavača. To je normalno. U prozoru udaljene radne površine kliknite **Poveži** da biste nastavili.

    ![Snimka zaslona upozorenja o Nepoznat izdavač](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. U prozoru sigurnost sustava Windows, upišite korisničko ime i lozinku za lokalni račun koji ste stvorili stvaranja na VM. Korisničko ime koje je unesena kao *vmname*& #92; *korisničko ime*, zatim kliknite **u redu**.

    ![Snimka zaslona s unosom VM ime, korisničko ime i lozinku](./media/virtual-machines-windows-hero-tutorial/credentials.png)
    
6.  Prikazat će se upozorenje da se potvrda ne može provjeriti. To je normalno. Kliknite **da** da biste provjerili identitet virtualnog računala i završi prijave.

    ![Snimka zaslona s prikazom poruke kompatibilnosti potvrđivanja identiteta u VM](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Ako ste naiđete na problem prilikom pokušaja povezivanja, potražite u članku [Otklanjanje poteškoća s veze udaljene radne površine da biste je utemeljen na sustavu Windows Azure virtualnog računala](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="install-iis-on-your-vm"></a>Instalirajte IIS na vašem VM

Sad kad ste prijavljeni na na VM, ne možemo instalirat će uloga poslužitelja tako da možete isprobati više.

1. Ako već nije otvoren, otvorite **Upravitelj poslužitelja** . Kliknite izbornik **Start** , a zatim kliknite **Upravitelj poslužitelja**.
2. U **Upravitelju poslužitelja**, u lijevom oknu odaberite **Lokalni poslužitelj** . 
3. Na izborniku odaberite **Upravljanje** > **Dodavanje uloge i značajke**.
4. U čarobnjaku za značajke, na stranici **Instalacija vrsta** i dodavanje uloge odaberite **Instalacija na temelju uloga ili značajkama utemeljen**, a zatim kliknite **Dalje**.

    ![Snimka zaslona s prikazom karticu dodavanje uloge i značajke Čarobnjak za vrstu instalacije](./media/virtual-machines-windows-hero-tutorial/role-wizard.png)

5. Odaberite na VM resurse poslužitelja, a zatim kliknite **Dalje**.
6. Na stranici **Uloge poslužitelja** odaberite **Web Server (IIS)**.

    ![Snimka zaslona s prikazom karticu dodavanje uloge i značajke Čarobnjak za uloge poslužitelja](./media/virtual-machines-windows-hero-tutorial/add-iis.png)

7. U skočnom prozoru o dodavanju značajke koje su potrebne za IIS obavezno da **sadrži alate za upravljanje** , a zatim kliknite **Dodaj značajke**. Nakon zatvaranja skočni prozor, kliknite **Dalje** u čarobnjaku.

    ![Snimka zaslona s prikazom skočni da biste potvrdili dodavanje IIS uloga](./media/virtual-machines-windows-hero-tutorial/confirm-add-feature.png)

8. Na stranici značajke kliknite **Dalje**.
9. Na stranici **Uloga poslužitelja na Web (IIS-a)** , kliknite **Dalje**. 
10. Na stranici **Uloge servisa** kliknite **Dalje**. 
11. Na stranici za **potvrdu** kliknite **Instaliraj**. 
12. Nakon dovršetka instalacije, kliknite **Zatvori** čarobnjaka.



## <a name="open-port-80"></a>Otvorite priključak 80 

Vaš VM da biste prihvatili dolazni promet putem priključka 80, morate dodati ulazna pravila sigurnosne grupe mreže. 

1. Otvorite [portal za Azure](https://portal.azure.com).
2. Na **virtualnim računalima sustava** odaberite VM koji ste stvorili.
3. U odjeljku postavke virtualnim strojevima odaberite **sučelje mreže** , a zatim odaberite postojeći mrežnog sučelja.

    ![Snimka zaslona s prikazom postavka virtualnog računala sučelje mreže](./media/virtual-machines-windows-hero-tutorial/network-interface.png)

4. **Osnove** za mrežno sučelje pritisnite **mreže sigurnosne grupe**.

    ![Snimka zaslona s prikazom u odjeljku Osnove mrežnog sučelja](./media/virtual-machines-windows-hero-tutorial/select-nsg.png)

5. U plohu **Essentials** za na NSG, trebali biste dobiti jednu postojećeg zadani ulazni pravila za **zadani-Dopusti-rdp** koji omogućuje vam da se prijavite na VM. Dodat će se drugo pravilo ulazne da dopušta promet IIS. Kliknite **pravilo ulazne sigurnost**.

    ![Snimka zaslona s prikazom u odjeljku Osnove za na NSG](./media/virtual-machines-windows-hero-tutorial/inbound.png)

6. U **Ulazna pravila za sigurnost**, kliknite **Dodaj**.

    ![Snimka zaslona s prikazom gumba za dodavanje sigurnosna pravila](./media/virtual-machines-windows-hero-tutorial/add-rule.png)

7. U **Ulazna pravila za sigurnost**, kliknite **Dodaj**. Upišite **80** u raspon priključaka i obavezno odaberite **Dopusti** . Kada završite, kliknite **u redu**.

    ![Snimka zaslona s prikazom gumba za dodavanje sigurnosna pravila](./media/virtual-machines-windows-hero-tutorial/port-80.png)
 
Dodatne informacije o NSGs, ulazni i izlazni pravila, u odjeljku [Dopusti vanjski pristup vašem VM pomoću portala za Azure](virtual-machines-windows-nsg-quickstart-portal.md)
 
## <a name="connect-to-the-default-iis-website"></a>Povezivanje s zadano IIS web-mjesto

1. Na portalu Azure kliknite **virtualnim računalima sustava** , a zatim odaberite vaše VM.
2. U plohu **Essentials** kopirajte **javnu IP adresa**.

    ![Snimka zaslona s prikazom gdje možete pronaći javnu IP adrese sustava VM](./media/virtual-machines-windows-hero-tutorial/ipaddress.png)

2. Otvorite preglednik i u adresnu traku upišite IP adresu javnog ovako: http://<publicIPaddress> i pritisnite **Enter** da biste prešli na tu adresu.
3. Vaš preglednik otvarajte zadani IIS web-stranicu. Izgleda otprilike ovako:

    ![Snimka zaslona s prikazom kako zadane stranice IIS izgleda u pregledniku](./media/virtual-machines-windows-hero-tutorial/iis-default.png)

    

## <a name="next-steps"></a>Daljnji koraci

- Možete i isprobati [prilaganja podatkovni disk](virtual-machines-windows-attach-disk-portal.md) na virtualnog računala. Diskova podataka navedite dodatnog prostora za pohranu za virtualnog računala.
