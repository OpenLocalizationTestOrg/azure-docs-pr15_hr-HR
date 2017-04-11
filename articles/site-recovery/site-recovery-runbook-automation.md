<properties
   pageTitle="Dodavanje runbooks Azure automatizacije planove za oporavak | Microsoft Azure"
   description="U ovom se članku opisuje kako oporavak Azure web-mjesta sada omogućuje vam da biste proširili tarife za oporavak pomoću Azure Automatizacija da biste dovršili složene zadatke tijekom oporavka za Azure"
   services="site-recovery"
   documentationCenter=""
   authors="ruturaj"
   manager="gauravd"
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="powershell"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required"
   ms.date="10/23/2016"
   ms.author="ruturajd@microsoft.com"/>


# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Dodavanje runbooks Azure automatizacije planove za oporavak


Pomoću ovog praktičnog vodiča opisuje kako oporavak web-mjesta Azure integrira Azure automatizaciju omogućuju proširenja za tarife za oporavak. Tarife za oporavak možete orkestrirali oporavak virtualnim računalima sustava zaštićene upravljanjem oporavak Azure web-mjesta za replikaciju sekundarnu oblak i replikacije na Azure scenarije. Pomažu i u mijenjanju za oporavak **dosljedno točne**, **kontrolirani**i **automatiziranog**. Ako se ne uspijeva putem virtualnim strojevima za Azure, Integracija s Azure Automatizacija proširuje tarife za oporavak i daje mogućnost dopušta izvršavanje runbooks, stoga dopuštanja Napredna automatizaciju zadataka.

Ako koju ste ne čuli o Azure Automatizacija još, prijavite se [ovdje](https://azure.microsoft.com/services/automation/) i preuzmite njihove ogledne skripte [ovdje](https://azure.microsoft.com/documentation/scripts/). Pročitajte više o [Oporavak Azure web-mjesta](https://azure.microsoft.com/services/site-recovery/) i kako orkestrirali oporavak za Azure pomoću tarife za oporavak [ovdje](https://azure.microsoft.com/blog/?p=166264).

U ovom ćete praktičnom vodiču će smo pogledajte kako integrirati Automatizacija Azure runbooks u tarife za oporavak. Radimo će automatizirati jednostavne zadatke koje ranije potrebne ručno intervencije i Saznajte kako pretvoriti više koraka oporavak akcija oporavka jednim klikom. Ne možemo će pogledati kako mogu otkloniti jednostavnu skriptu ako odlazi u redu.

## <a name="customize-the-recovery-plan"></a>Prilagodba plan za oporavak

1. Javite nam započeti operning plohu resursa tarifa za oporavak. Vidjet ćete tarifa za oporavak sadrži dvije virtualnim strojevima dodavanja za oporavak. 

    ![](media/site-recovery-runbook-automation-new/essentials-rp.PNG)
---------------------

2. Kliknite gumb Prilagodba da počnete dodavati na runbook. Otvorit će se plan za oporavak Prilagodba plohu.


    ![](media/site-recovery-runbook-automation-new/customize-rp.PNG)


3. Desnom tipkom miša kliknite start grupa 1 i odaberite da biste dodali na Dodaj akciju-objavu.

4. Odaberite da biste odabrali skriptu u novi plohu.

5. Dodijelite naziv skripte "Pozdrav svijeta".

6. Odaberite naziv računa za automatizaciju. Ovo je račun za Azure automatizaciju. Imajte na umu da se ovaj račun može biti u bilo kojem Azure Zemljopis, ali mora biti iste pretplate na kao sigurnog oporavak web-mjesta.

7. Odaberite na runbook s računa za automatizaciju. Ovo je skriptu koja će se pokrenuti tijekom izvođenja tarifa za oporavak nakon oporavka prve grupe.

    ![](media/site-recovery-runbook-automation-new/update-rp.PNG)


8. Odaberite u redu da biste spremili skriptu. To će dodajte skriptu grupu akcija objavu grupe 1: Start.


    ![](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="salient-points-of-adding-a-script"></a>Dodavanje skripte upadljivim točaka

1. Možete desnom tipkom miša kliknite skripta i odaberite "delete korak" ili "ažurirati skripte".

2. Skripta mogu se izvoditi na Azure tijekom prebacivanje iz lokalnog za Azure, a mogu se izvoditi na Azure kao primarni skripti prije zatvaranja, tijekom failback iz Azure na lokalni.

3. Kada se pokrene skriptu, ga ubacivanje kontekstu tarifu za oporavak.

U nastavku je primjera izgleda varijabla kontekst.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Sljedeća tablica sadrži naziv i opis za svaku varijablu u kontekstu.

**Naziv varijable** | **Opis**
---|---
RecoveryPlanName | Naziv plana pokrenut. Omogućuje da iskoristite akcija na temelju ime koristeći istu skripte
FailoverType | Određuje je li u prebacivanje je testirati, planiranog ili neplanirano.
FailoverDirection | Odredite hoće li se oporavak primarni ili sekundarni
GroupID | Prepoznavanje broj grupe unutar plan za oporavak kada je pokrenut plan
VmMap | Polja sve virtualnim strojevima u grupi
VMMap ključ | Jedinstveni ključ (GUID) za svaki VM. Gdje je to primjenjivo je isti kao VMM ID virtualnog računala.
Naziv uloge | Naziv VM Azure koji je obnavljaju
CloudServiceName | Azure servis u Oblaku naziv pod kojim se stvara virtualnog računala.
CloudServiceName (u model implementacije Voditelj resursa) | Grupa resursa Azure naziv pod kojim se stvara virtualnog računala.


## <a name="using-complex-variables-per-recovery-plan"></a>Pomoću varijabli kompleksnog po oporavak plan

Ponekad je runbook zahtijeva više informacija od samo RecoveryPlanContext. Postoji bez prve klase mehanizam za prosljeđivanje parametra u runbook. Ako želite koristiti isti skriptu putem više tarifa za oporavak koje možete koristiti kontekst planiranje oporavak varijabla 'RecoveryPlanName' i koristiti u nastavku eksperimentalne postupak da biste koristili varijablu kompleksnog Automatizacija Azure u na runbook. U sljedećem primjeru pokazuje kako možete stvoriti tri različite kompleksnog varijabla resursi i ih koristiti u runbook temelji se na nazivu plan za oporavak.

Razmislite o koji želite koristiti 3 dodatne parametre u na runbook. Javite nam kodiranje u obrazac JSON {"Var1": "testautomation", "Var2": "Neplanirano", "Var3": "PrimaryToSecondary"}

Koristite [varijablu AA kompleksnog](../automation/automation-variables.md#variable-types Complex variable) da biste stvorili novi Automatizacija resursa.
Naziv varijable kao <RecoveryPlanName>- params.
Poslužite se referenca da biste stvorili [varijabla kompleksnog](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396).

Tarife za različite oporavak naziv varijable kao

1. recoveryPlanName1 >-params
2. recoveryPlanName2 >-params
3. recoveryPlanName3 >-params

Sada u skripti odnose params kao

1. Dohvaćanje naziva to iz na $rpname = $Recoveryplancontext varijabla
2. Početak resursa $paramValue = "$($rpname) params"
3. Služi kao kompleksnog varijabla za oporavak plan tako da nazovete Get-AzureAutomationVariable [-AutomationAccountName] <String> -naziv $paramValue.

Na primjer, da biste dobili kompleksnog varijabla/parametar za oporavak plan SharepointApp, stvoriti varijablu kompleksnog Azure Automatizacija koja se pod nazivom "SharepointApp params".

Koristite u planu oporavak tako što izvlači varijabla iz resursa pomoću izjave Get-AzureAutomationVariable [-AutomationAccountName] <String> [-naziv] $paramValue. [Upućuju na to dodatne informacije](https://msdn.microsoft.com/library/dn913772.aspx)

Na taj način isti skripte može se koristiti za različite oporavak plan spremanjem plan određene kompleksnog varijabla u imovinu.

## <a name="sample-scripts"></a>Ogledne skripte

Spremište skripte koje možete uvesti izravno na račun za automatizaciju potražite [Kristian Nese OMS spremište za skripte](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/Solutions/asrautomation)

Skripta ovdje je predložak Azure Voditelj resursa koji će sve implementacija u nastavku skripte

* NSG

NSG runbook će dodijeliti javnu IP adrese svaki VM unutar oporavak planiranje i njihove virtualne mrežnih prilagodnika priložiti sigurnosne grupe mreže omogućit će zadane komunikacije

* PublicIP

Javnu IP runbook dodijeliti javnu IP adrese svaki VM unutar planiranje oporavak. Pristup računalima i aplikacije ovisi o postavke vatrozida u svakom goste


* CustomScript

CustomScript runbook će dodijeliti javnu IP adrese svaki VM unutar oporavak planiranje i instalirajte proširenje za prilagođene skripte koje će istaknuti skripte uputiti tijekom implementacije predloška

* NSGwithCustomScript

NSGwithCustomScript runbook dodijeliti javnu IP adrese svaki VM unutar na oporavak planiranje, instalacija prilagođene skripte korištenje nastavka i povezati NSG dopuštanja virtualne mrežnih prilagodnika zadani ulazni i izlazni komunikacije za daljinski pristup

## <a name="additional-resources"></a>Dodatni resursi

[Azure Automation Services pokreće kao račun](../automation/automation-sec-configure-azure-runas-account.md)

[Pregled Azure automatizacije] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Pregled Azure automatizacije")

[Ogledne skripte Azure automatizacije] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Ogledne skripte Azure automatizacije")
