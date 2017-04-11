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


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups---public-preview"></a>Objaviti na zasebnom mreža i lokacija pomoću poveznika grupe – javno pretpregled

> [AZURE.SELECTOR]
- [Portal za Azure](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure klasični portal](active-directory-application-proxy-connectors.md)


Poveznik grupe su korisne za različite scenarije, uključujući:

- Web-mjesta s više međusobno povezanih podatkovnim centrima. U ovom slučaju koje želite zadržati suvišni promet unutar s podatkovnim centrom moguće jer su veze unakrsno Standard skupi i sporo. Možete implementirati poveznika u svakom podatkovnog centra za aplikacije koje se nalaze unutar s podatkovnim centrom. Taj se način minimizira unakrsno Standard veze te pružaju potpuno prozirne korisnicima.
- Upravljanje aplikacija instalirana na Izolirani mreža koje nisu dio glavnog poslovnoj mreži. Poveznik grupe možete koristiti da biste instalirali namjenski poveznika na Izolirani mreža i izdvojiti aplikacije s mrežom.
- Za aplikacije instalirana na IaaS pristup putem oblaka, poveznika grupe omogućuju uobičajenih servisa da biste siguran pristup za sve aplikacije. Poveznik grupe ne stvorite dodatne ovisnosti na mreži tvrtke ili fragmentiraj okruženja aplikacije. Poveznika moguće je instalirati na svakoj oblaka podatkovnog centra, a služe samo aplikacije koje se nalaze u ovu mrežu. Možete instalirati nekoliko poveznika da biste postigli visoke dostupnosti.
- Podrška za više skupa stabala okruženja u kojima određene poveznika mogu implementiran po skupa stabala i postavljanje da bi služio određene aplikacije.
- Poveznik grupe se mogu koristiti u oporavak Izrada web-mjesta ili otkriti prebacivanje ili kao sigurnosnu kopiju za glavno web-mjesto.
- Poveznik grupe i omogućuje vam poslužiti više tvrtki iz jednog klijenta.

## <a name="prerequisite-create-your-connectors"></a>Preduvjeta: Stvaranje vaše poveznika
Da biste grupirali vaše poveznika, morate biti sigurni da [instaliran većeg broja poveznika](active-directory-application-proxy-enable.md). Nakon što instalirate novi poveznika, automatski spaja poveznik grupi **zadano** .

## <a name="step-1-create-connector-groups"></a>Korak 1: Stvaranje grupe poveznika
Možete stvoriti željeni broj grupa poveznik. Stvaranje grupe Connector se postiže [Azure portal](https://portal.azure.com).

1. Odaberite **Azure Active Directory** da biste prešli na nadzornoj ploči upravljanja za direktorija. Iz nje, odaberite **aplikacijama** > **proxy aplikacije**.

2. Odaberite gumb **Connector grupe** . Pojavit će se nova grupa poveznik plohu.

3. Nova grupa poveznik dajte naziv, a zatim pomoću na padajućem izborniku odaberite koje poveznika pripadaju u ovoj grupi.

4. Nakon dovršetka poveznik grupe, odaberite **Spremi** .

## <a name="step-2-assign-applications-to-your-connector-groups"></a>Korak 2: Dodjeljivanje aplikacije grupama poveznika
Posljednji korak je da biste postavili svaku aplikaciju grupi poveznik koja će služiti.

1. Na nadzornoj ploči upravljanja za direktorija, odaberite **aplikacijama** > **sve aplikacije** > aplikaciju koju želite dodijeliti grupi connector > **Proxy aplikacije**.
2. U odjeljku **grupe Connector**pomoću na padajućem izborniku odaberite željenu grupu aplikaciju koju želite koristiti.
3. Odaberite **Spremi** da biste primijenili promjene.


## <a name="see-also"></a>Vidi također

- [Omogućivanje Proxy aplikacije](active-directory-application-proxy-enable.md)
- [Omogućivanje jedinstvene prijave na](active-directory-application-proxy-sso-using-kcd.md)
- [Omogućivanje pristupa uvjetno](active-directory-application-proxy-conditional-access.md)
- [Otklanjanje poteškoća s Proxy aplikacije](active-directory-application-proxy-troubleshoot.md)

Najnovije vijesti i ažuriranja, potražite u članku [blog Proxy aplikacije](http://blogs.technet.com/b/applicationproxyblog/)
