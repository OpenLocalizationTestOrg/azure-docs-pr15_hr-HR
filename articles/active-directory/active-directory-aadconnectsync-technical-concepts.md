<properties
    pageTitle="Azure AD Connect sinkronizacije: Tehnički pojmovi | Microsoft Azure"
    description="U članku se objašnjava Tehnički pojmovi Azure AD Connect sinkronizacije."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Connect sinkronizacije: Tehnički pojmovi
Ovaj se članak odnosi sažetak temu [Razumijevanje arhitektura](active-directory-aadconnectsync-technical-concepts.md).

Azure AD Connect sinkronizaciju nadovezuje puna metadirectory sinkronizacije platforme.
U sljedećim se odjeljcima predstavljanje koncepata metadirectory sinkronizacije.
Stvaranje nakon MIIS, ILM i FIM, sinkronizaciju usluga za Active Directory Azure nudi sljedeće platforme za povezivanje s izvorima podataka, sinkroniziranje podataka između izvora podataka, kao i za dodjelu resursa i deprovisioning identiteta.

![Tehnički pojmovi](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

Sljedeći odjeljci sadrže dodatne detalje o sljedeće aspekte FIM servisu sinkronizacije:

- Poveznik
- Atribut tijek
- Poveznik prostora
- Metaverse
- Dodjela resursa

## <a name="connector"></a>Poveznik

Moduli koda koji se koriste za komunikaciju s povezanim direktorij nazivaju poveznika (prijašnjeg upravljanje agenti (MAs)).

Te su instalirani na računalu s operacijskim sustavom Azure AD Connect sinkronizaciju.
Poveznika pružaju agentless mogućnost raspravljaju pomoću protokola udaljeni sustav tipkanje implementacije specijalizirane agenata. To znači da smanjiti rizik i vremena za implementaciju, osobito kada povezanima s ključnih aplikacije i sustave.

Na gornjoj slici, poveznik sinonim je za prostor connector, ali obuhvaća sve komunikacije s vanjskim sustavom.

Poveznik je zadužen za sve uvoz i izvoz funkcijama sustava i oslobađa razvojnim inženjerima iz koje je potrebno znati kako se povezati sa svakom sustavu nativno prilikom korištenja deklarativno dodjele resursa da biste prilagodili transformacije podataka.

Uvozi i izvozi samo pojaviti kada zakazano, čime se omogućuje daljnje insulation od promjena koje su se pojavile u sustavu, budući da se promjene ne automatski prenijeti na izvora povezanih podataka. Osim toga, razvojni inženjeri i stvoriti vlastite poveznici za povezivanje s gotovo bilo kojeg izvora podataka.

## <a name="attribute-flow"></a>Atribut tijek

U metaverse je konsolidirani prikaz svih spojenih identitete iz susjednom poveznik razmake. Na slici iznad tijek atributa koji će se po recima vrhova strelica za ulazni i izlazni tijek. Atribut tijek je postupak kopiranjem ili pretvorba podataka iz sustava u drugu i svih atributa tokova (dolazne i odlazne).

Atribut protok pojavljuje se između poveznik prostora i u metaverse bi-directionally prilikom sinkronizacije (cijelog ili delta) operacije zakazani da biste pokrenuli.

Atribut protok pojavljuje se samo kada se izvoditi te sinkronizacijom. Atribut tokova se definiraju pravila za sinkronizaciju. To se može biti ulaznog (ISR na gornjoj slici) ili izlazni (OSR na gornjoj slici).

## <a name="connected-system"></a>Povezani sustava

Povezani sustava (ili povezani imenik) je koje upućuju na udaljeni sustav Azure AD Connect sinkronizaciju spojio i čitanje i pisanje identiteta podataka iz.

## <a name="connector-space"></a>Poveznik prostora

Filtrirani podskup objekti i atributi koje prostor connector smanjivati svaki izvor povezanih podataka.
To omogućuje sinkronizaciju servisa lokalno raditi bez potrebe za kontakt udaljeni sustav prilikom sinkroniziranja objekte i ograničava interakcije prema uvozi i izvozi samo.

Kada izvora podataka i poveznik ima mogućnost pružanja popis promjena (delta uvoz), zatim radu učinkovitosti povećava znatno kao samo promjene jer su razmijenili posljednje ciklusa ankete. Poveznik prostor insulates izvora povezanih podataka iz promjena automatski prijenos uz obaveznu da raspored poveznik izvozi i uvozi. Ova osiguranja dodane daje brigu tijekom testiranja, pretpregled ili potvrđivanje sljedeće ažuriranje.

## <a name="metaverse"></a>Metaverse

U metaverse je konsolidirani prikaz svih spojenih identitete iz susjednom poveznik razmake.

Kako identiteta povezanih i izdavanje dodijeljuje se za različite atribute putem mapiranja tijek uvoza, središnje metaverse objekt se započinje prikupljanje informacija iz više sustava. Iz ovaj tijek atribut objekt mapiranja mogu sadržavati podatke iz sustava.

Objekti se stvaraju kada ih mjerodavne sustava projekata u na metaverse. Čim se uklanjaju se sve veze, briše se objekt metaverse.

Objekti u na metaverse nije moguće uređivati izravno. Sve podatke u objektu mora biti pridonio kroz tijek atribut. Na metaverse održava stalni poveznika s svaki poveznik razmak. Ove poveznika zahtijevati ponovnu procjenu za svaki sinkronizacija. To znači sinkronizaciju Azure AD Connect nije da biste pronašli odgovarajući udaljene objekt svaki put. Taj se način izbjegavaju potrebe za skup agenata da biste spriječili promjene atribute koje bi inače odgovoran za correlating objekte.

Kada otkrivanje novih izvora podataka možda već postojeća objekata koje je potrebno je upravljati Azure AD Connect sinkronizaciju koristi proces naziva spoj pravilo da biste procijenili potencijalne kandidata pomoću kojih se uspostaviti vezu.
Uspostavljanja vezu ovaj procjenu pojavi ponovno i normalno atribut tijek se može dogoditi između izvora udaljene povezanih podataka i na metaverse.

## <a name="provisioning"></a>Dodjela resursa

Kada se projekata pouzdanih izvora novi objekt u metaverse novi objekt prostora poveznik moguće stvoriti u drugom poveznik koji predstavlja izvor do povezanih podataka.

Time se čini uspostavlja veza, a tijek atribut možete nastaviti bi-directionally.

Kad god se pravilo određuje da novi objekt poveznik razmak treba će biti stvoren, on se zove dodjele resursa. Međutim, jer ovaj postupak samo odvija unutar prostora poveznika, ga ne prenijeti u izvora povezanih podataka dok se izvodi izvoz.

## <a name="additional-resources"></a>Dodatni resursi

* [Azure AD povezivanje sinkronizacije: Mogućnosti za prilagodbu sinkronizacije](active-directory-aadconnectsync-whatis.md)
* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
