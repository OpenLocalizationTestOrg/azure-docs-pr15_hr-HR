<properties
    pageTitle="Proširenje ploče programa Access za otklanjanje poteškoća za Internet Explorer | Microsoft Azure"
    description="Kako koristiti pravila grupe za implementaciju dodatak preglednika Internet Explorer za portal Moje aplikacije."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Proširenje ploče programa Access za otklanjanje poteškoća za Internet Explorer

U ovom se članku pronaći ćete poteškoća sljedeće:

- Nećete moći pristupati aplikacije putem portala za Moje aplikacije pomoću programa Internet Explorer.
- Se prikaže poruka "Instalacija softvera" čak i ako ste već instalirali softver.

Ako ste administrator, Vidi također: [Kako implementirati nastavak ploča programa Access za Internet Explorer pomoću pravilnika grupe](active-directory-saas-ie-group-policy.md)

##<a name="run-the-diagnostic-tool"></a>Pokretanje dijagnostičkog alata

Problemi s instalacijom s nastavkom ploče pristup možete dijagnosticiranje preuzimanjem i pokretanje alata za dijagnostičkih ploča programa Access:

1. [Kliknite ovdje da biste preuzeli dijagnostički alat.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. Otvorite datoteku, a zatim pritisnite gumb **Izdvoji sve** .

    ![Pritisnite Izdvoji sve](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. Pritisnite gumb za **izdvajanje** da biste nastavili.

    ![Pritisnite Izdvoji](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. Da biste pokrenuli alat, desnom tipkom miša kliknite datoteku pod nazivom **AccessPanelExtensionDiagnosticTool**, a zatim odaberite **Otvori programom > Microsoft Windows temelji Script Host**.

    ![Otvori programom > Microsoft Windows temelji Script Host](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. Prikazat će se zatim sljedeće dijagnostičkih prozoru koji opisuje što bi moglo biti problem s instalacijom.

    ![Uzorak dijagnostičkih prozora](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. Kliknite "**da**" da biste omogućili program rješavanje problema koji nisu pronađene.

7. Da biste spremili promjene, zatvorite svakog prozora preglednika Internet Explorer, a zatim ponovno otvorite Internet Explorer.<br />Ako i dalje ne možete pristupiti aplikacijama, isprobajte korake u nastavku.

##<a name="check-that-the-access-panel-extension-is-enabled"></a>Provjerite je li omogućen nastavak ploča programa Access

Da biste provjerili proširenja ploče programa Access omogućena u programu Internet Explorer:

1. U pregledniku Internet Explorer, kliknite **ikonu zupčanika** u gornjem desnom kutu prozora. Pa odaberite **Internetske mogućnosti**.<br />(U starijim verzijama programa Internet Explorer možete pronaći to u odjeljku **Alati > internetske mogućnosti**.

    ![Kliknite Alati > internetske mogućnosti](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. Kliknite karticu **programi** , a zatim kliknite gumb **Upravljanje dodacima** .

    ![Kliknite Upravljanje dodacima](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. U ovom dijaloškom okviru odaberite **Nastavak ploča programa Access** , a zatim kliknite gumb **Omogući** .

    ![Kliknite Omogući](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. Da biste spremili promjene, zatvorite svakog prozora preglednika Internet Explorer i ponovno otvorite Internet Explorer.

##<a name="enable-extensions-for-inprivate-browsing"></a>Omogući proširenja za pregledavanje weba InPrivate

Ako koristite način pregledavanja weba InPrivate:

1. U pregledniku Internet Explorer, kliknite **ikonu zupčanika** u gornjem desnom kutu prozora. Pa odaberite **Internetske mogućnosti**.<br />(U starijim verzijama programa Internet Explorer možete pronaći to u odjeljku **Alati > internetske mogućnosti**.

    ![Uzorak dijagnostičkih prozora](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. Idite na karticu **Privatnost** , zatim **poništite** potvrdni okvir s natpisom **onemogućite alatne trake i proširenja prilikom pregledavanja weba InPrivate pokretanja**</p>

    ![Poništite potvrdni okvir Onemogući alatne trake i proširenja prilikom pregledavanja weba InPrivate pokretanja](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. Da biste spremili promjene, zatvorite svakog prozora preglednika Internet Explorer i ponovno otvorite Internet Explorer.

##<a name="uninstall-the-access-panel-extension"></a>Deinstalacija nastavak ploča programa Access

Da biste deinstalirali proširenja za pristup ploče s vašeg računala:

1. Na tipkovnici pritisnite tipku s **logotipom sustava Windows** da biste otvorili izbornik Start. Kad je otvoren u izborniku možete upisati ništa za pretraživanje. Upišite "Upravljačka ploča", a zatim otvorite **Upravljačku ploču** kada se pojavi u rezultatima pretraživanja.

    ![Pretraživanje unesite Upravljačka ploča](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. U gornjem desnom kutu na upravljačkoj ploči promijenite mogućnost **Prikaži po** **velikih**ikona. Zatim pronađite i kliknite gumb **Programi i značajke** .

    ![Promijenit prikaza za prikaz velike ikone](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. Na popisu odaberite **Nastavak ploča programa Access**, a zatim gumb **Deinstaliraj** .

    ![Kliknite Deinstaliraj](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. Zatim možete pokušati instalirajte proširenje ponovno da biste vidjeli je li problem riješen.

Ako naiđete na probleme deinstalacijom proširenje, koristeći alat [Microsoft rješavanje It](https://go.microsoft.com/?linkid=9779673) možete i ukloniti.

## <a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Program access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Kako implementirati nastavak ploča programa Access za Internet Explorer pomoću pravilnika grupe](active-directory-saas-ie-group-policy.md)
