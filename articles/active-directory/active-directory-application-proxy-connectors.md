<properties
    pageTitle="Rad s Proxy aplikacije za Azure AD poveznika | Microsoft Azure"
    description="Opisuje kako stvoriti i upravljanje grupama poveznika u Proxy aplikacije za Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Objaviti na zasebnom mreža i lokacija pomoću poveznika grupe

> [AZURE.SELECTOR]
- [Portal za Azure](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure klasični portal](active-directory-application-proxy-connectors.md)


Poveznik grupe su korisne za različite scenarije, uključujući:

- Web-mjesta s više međusobno povezanih podatkovnim centrima. U ovom slučaju koje želite zadržati suvišni promet unutar s podatkovnim centrom moguće jer više Standard veze obično su skupi i sporo. Možete implementirati poveznika u svakom podatkovnog centra za aplikacije koje se nalaze unutar s podatkovnim centrom. Taj se način minimizira unakrsno Standard veze te pružaju potpuno prozirne korisnicima.
- Upravljanje aplikacija instalirana na Izolirani mreža koje nisu dio glavnog poslovnoj mreži. Poveznik grupe možete koristiti da biste instalirali namjenski poveznika na Izolirani mreža i izdvojiti aplikacije s mrežom.
- Za aplikacije instalirana na IaaS pristup putem oblaka, poveznika grupe omogućuju uobičajenih servisa da biste siguran pristup za sve aplikacije. Poveznik grupe ne stvorite dodatne ovisnosti na mreži tvrtke ili fragmentiraj okruženja aplikacije. Poveznika moguće je instalirati na svakoj oblaka podatkovnog centra, a služe samo aplikacije koje se nalaze u ovu mrežu. Možete instalirati nekoliko poveznika da biste postigli visoke dostupnosti.
- Podrška za više skupa stabala okruženja u kojima određene poveznika mogu implementiran po skupa stabala i postavljanje da bi služio određene aplikacije.
- Poveznik grupe se mogu koristiti u oporavak Izrada web-mjesta ili otkriti prebacivanje ili kao sigurnosnu kopiju za glavno web-mjesto.
- Poveznik grupe i omogućuje vam poslužiti više tvrtki iz jednog klijenta.

## <a name="prerequisite-create-your-connectors"></a>Preduvjeta: Stvaranje vaše poveznika
Da biste grupirali vaše poveznika, imate provjerite jesu li [instalirani većeg broja poveznika](active-directory-application-proxy-enable.md)i koju ste nazvali ih i grupirati ih. Na kraju, morate ih dodijeliti određeni aplikacije.

## <a name="step-1-create-connector-groups"></a>Korak 1: Stvaranje grupe poveznika
Možete stvoriti željeni broj grupa poveznik. Stvaranje grupe poveznika se postiže Azure klasični portalu.

1. Odaberite direktorija, a zatim kliknite **Konfiguriraj**.  
    ![Proxy poslužitelj aplikacije konfiguriranje snimka – kliknite Upravljanje grupama poveznika](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)

2. U odjeljku Proxy aplikacije kliknite **Upravljanje grupama poveznika** , a zatim stvorite novu grupu poveznik dodjeljivanjem grupi naziv.  
    ![Aplikacija proxy poveznik grupe snimka - naziv nove grupe](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Korak 2: Dodijeliti poveznika grupe
Nakon stvaranja grupe poveznika, Premještanje poveznika odgovarajuće grupe.

1. U odjeljku **Proxy aplikacije**kliknite **Upravljanje poveznike**.
2. U odjeljku **grupe**odaberite grupu u koju želite za svaki poveznik. Poveznika može potrajati do 10 minuta postane aktivan u novu grupu.  
    ![Aplikacija proxy poveznika snimka - odabranom grupom padajućem izborniku](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Korak 3: Dodjeljivanje aplikacije grupama poveznika
Posljednji korak je da biste postavili svaku aplikaciju grupi poveznik koja će služiti.

1. Azure klasični portalu u direktoriju, odaberite aplikaciju koju želite dodijeliti grupi, a zatim kliknite **Konfiguriraj**.
2. U odjeljku **grupe poveznika**, odaberite željenu grupu aplikaciju koju želite koristiti. Ta promjena se odmah primjenjuje.  
    ![Aplikacija proxy poveznik grupe snimka - odabranom grupom padajućem izborniku](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)


## <a name="see-also"></a>Vidi također

- [Omogućivanje Proxy aplikacije](active-directory-application-proxy-enable.md)
- [Omogućivanje jedinstvene prijave na](active-directory-application-proxy-sso-using-kcd.md)
- [Omogućivanje pristupa uvjetno](active-directory-application-proxy-conditional-access.md)
- [Otklanjanje poteškoća s Proxy aplikacije](active-directory-application-proxy-troubleshoot.md)

Najnovije vijesti i ažuriranja, potražite u članku [blog Proxy aplikacije](http://blogs.technet.com/b/applicationproxyblog/)
